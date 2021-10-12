+++
author = "Raikay"
title = "分布式唯一ID - 雪花ID Snowflake .Net版"
date = "2020-02-11"
description = "分布式唯一ID 解决方案 非 GUID UUID：雪花ID Snowflake DotNet版 DotNet Core"
tags = [
    "分布式",
]

+++



### 雪花Id介绍：

雪花ID是用一个64位的整形数字来做ID，对应.net中的long，数据库中的bigint，雪花算法的原始版本是[scala版](https://github.com/twitter/snowflake/releases/tag/snowflake-2010)，用于生成分布式ID（纯数字，时间顺序）,订单编号等。
#### 生成的雪花Id：

```
35324861574164480
35324861578358784
35324861578358785
```
####  对比自增和Guid:

**自增ID：** 对于数据敏感场景不宜使用，且不适合于分布式场景。  
**GUID：** 采用无意义字符串，且36位字符串太长，数据量增大时造成访问过慢，且不宜排序。

![137422-20180601005737392-1981165973](https://gitee.com/imgrep001/m1/raw/master/20200811132116.jpg)

#### 算法描述：
- 最高位是符号位，始终为0，不可用。
- 41位的时间序列，精确到毫秒级，41位的长度可以使用69年。时间位还有一个很重要的作用是可以根据时间进行排序。
- 10位的机器标识，10位的长度最多支持部署1024个节点。
- 12位的计数序列号，序列号即一系列的自增id，可以支持同一节点同一毫秒生成多个ID序号，12位的计数序列号支持每个节点每毫秒产生4096个ID序号。


### 雪花算法改进，时钟回拨：

雪花ID严重依赖系统当前时间，当系统时间被人为反后调整时，算法会出问题，可能会出重复ID．Snowflake原算法是在检测到系统时间被回调后直接抛异常．本代码在时钟回拨后,会将生成的ID时间戳停留在最后一次时间戳上(每当序列溢出时会往前走一毫秒),等待系统时间追上后即可以避过时钟回拨问题.

这种处理方式的优点是时钟回拨后不会异常，能一直生成出雪花ID，但缺点是雪花ID中的时间戳不是系统的当前时间，会是回拨前的最后记录的一次时间戳，但相差也不大．不知道有没有什么生产系统会对这个时间戳要求非常严格，无法使用这种补救方式的？

当然停掉系统后的时钟回拨是无法处理的,这种还是会有可能出现重复ID的．

 ####  生成Id以及解析Id方法:

源码git地址:[ https://gitee.com/itsm/learning_example/tree/master/snowflake-雪花Id]( https://gitee.com/itsm/learning_example/tree/master/snowflake-雪花Id)

```csharp
public class SnowflakeId
    {

        // 开始时间截((new DateTime(2020, 1, 1, 0, 0, 0, DateTimeKind.Utc)-Jan1st1970).TotalMilliseconds)
        private const long twepoch = 1577836800000L;

        // 机器id所占的位数
        private const int workerIdBits = 5;

        // 数据标识id所占的位数
        private const int datacenterIdBits = 5;

        // 支持的最大机器id，结果是31 (这个移位算法可以很快的计算出几位二进制数所能表示的最大十进制数) 
        private const long maxWorkerId = -1L ^ (-1L << workerIdBits);

        // 支持的最大数据标识id，结果是31 
        private const long maxDatacenterId = -1L ^ (-1L << datacenterIdBits);

        // 序列在id中占的位数 
        private const int sequenceBits = 12;

        // 数据标识id向左移17位(12+5) 
        private const int datacenterIdShift = sequenceBits + workerIdBits;

        // 机器ID向左移12位 
        private const int workerIdShift = sequenceBits;


        // 时间截向左移22位(5+5+12) 
        private const int timestampLeftShift = sequenceBits + workerIdBits + datacenterIdBits;

        // 生成序列的掩码，这里为4095 (0b111111111111=0xfff=4095) 
        private const long sequenceMask = -1L ^ (-1L << sequenceBits);

        // 数据中心ID(0~31) 
        public long datacenterId { get; private set; }

        // 工作机器ID(0~31) 
        public long workerId { get; private set; }

        // 毫秒内序列(0~4095) 
        public long sequence { get; private set; }

        // 上次生成ID的时间截 
        public long lastTimestamp { get; private set; }


        /// <summary>
        /// 雪花ID
        /// </summary>
        /// <param name="datacenterId">数据中心ID</param>
        /// <param name="workerId">工作机器ID</param>
        public SnowflakeId(long datacenterId,long workerId )
        {
            if (datacenterId > maxDatacenterId || datacenterId < 0)
            {
                throw new Exception(string.Format("datacenter Id can't be greater than {0} or less than 0", maxDatacenterId));
            }
            if (workerId > maxWorkerId || workerId < 0)
            {
                throw new Exception(string.Format("worker Id can't be greater than {0} or less than 0", maxWorkerId));
            }
            this.workerId = workerId;
            this.datacenterId = datacenterId;
            this.sequence = 0L;
            this.lastTimestamp = -1L;
        }

        /// <summary>
        /// 获得下一个ID
        /// </summary>
        /// <returns></returns>
        public long NextId()
        {
            lock (this)
            {
                long timestamp = GetCurrentTimestamp();
                if (timestamp > lastTimestamp) //时间戳改变，毫秒内序列重置
                {
                    sequence = 0L;
                }
                else if (timestamp == lastTimestamp) //如果是同一时间生成的，则进行毫秒内序列
                {
                    sequence = (sequence + 1) & sequenceMask;
                    if (sequence == 0) //毫秒内序列溢出
                    {
                        timestamp = GetNextTimestamp(lastTimestamp); //阻塞到下一个毫秒,获得新的时间戳
                    }
                }
                else   //当前时间小于上一次ID生成的时间戳，证明系统时钟被回拨，此时需要做回拨处理
                {                   
                    sequence = (sequence + 1) & sequenceMask;
                    if (sequence > 0) 
                    {
                        timestamp = lastTimestamp;     //停留在最后一次时间戳上，等待系统时间追上后即完全度过了时钟回拨问题。
                    }
                    else   //毫秒内序列溢出
                    {
                        timestamp = lastTimestamp + 1;   //直接进位到下一个毫秒                          
                    }
                    //throw new Exception(string.Format("Clock moved backwards.  Refusing to generate id for {0} milliseconds", lastTimestamp - timestamp));
                }

                lastTimestamp = timestamp;       //上次生成ID的时间截

                //移位并通过或运算拼到一起组成64位的ID
                var id= ((timestamp - twepoch) << timestampLeftShift)
                        | (datacenterId << datacenterIdShift)
                        | (workerId << workerIdShift)
                        | sequence;
                return id;
            }
        }

        /// <summary>
        /// 解析雪花ID
        /// </summary>
        /// <returns></returns>
        public static string AnalyzeId(long Id)
        {
            StringBuilder sb = new StringBuilder();

            var timestamp = (Id >> timestampLeftShift)  ;
            var time = Jan1st1970.AddMilliseconds(timestamp + twepoch);
            sb.Append(time.ToLocalTime().ToString("yyyy-MM-dd HH:mm:ss:fff"));

            var datacenterId = (Id ^ (timestamp  << timestampLeftShift)) >> datacenterIdShift;
            sb.Append("_" + datacenterId);

            var workerId = (Id ^ ((timestamp  << timestampLeftShift) | (datacenterId << datacenterIdShift))) >> workerIdShift;
            sb.Append("_" + workerId);

            var sequence = Id & sequenceMask;
            sb.Append("_" + sequence);

            return sb.ToString();
        }

        /// <summary>
        /// 阻塞到下一个毫秒，直到获得新的时间戳
        /// </summary>
        /// <param name="lastTimestamp">上次生成ID的时间截</param>
        /// <returns>当前时间戳</returns>
        private static long GetNextTimestamp(long lastTimestamp)
        {
            long timestamp = GetCurrentTimestamp();
            while (timestamp <= lastTimestamp)
            {
                timestamp = GetCurrentTimestamp();
            }
            return timestamp;
        }

        /// <summary>
        /// 获取当前时间戳
        /// </summary>
        /// <returns></returns>
        private static long GetCurrentTimestamp()
        {
            return (long)(DateTime.UtcNow - Jan1st1970).TotalMilliseconds; 
        }

        private static readonly DateTime Jan1st1970 = new DateTime(1970, 1, 1, 0, 0, 0, DateTimeKind.Utc);
    }
```

内容来自：

> [https://www.cnblogs.com/sunyuliang/p/12161416.html](https://www.cnblogs.com/sunyuliang/p/12161416.html)