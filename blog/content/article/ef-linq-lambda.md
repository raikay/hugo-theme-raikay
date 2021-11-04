+++
author = "Raikay"  
title = "EF、Linq、Lambda常用查询表示用法示例详解"  
date = "2021-11-04"  
description = "EF、Linq、Lambda常用查询表示用法示例详解"  
tags = [  
    "lambda", "linq","dotnet",
]  

+++


### select
```
int[] scores = { 90, 71, 82, 93, 75, 81 };
//查大于80分的成绩
//Linq写法
var result = from s in scores
             where s > 80
             select s;   
             
 //lambda表达式写法             
var result1 = scores.Where(s => s > 80);  
```

#### 排序
```
//查大于80分的成绩
var result = from s in scores
             where s > 80 
             orderby s descending //倒序  正序为ascending
             select s;
             
var result1 = scores.Where(s => s > 80).OrderByDescending(s=>s);//倒序
var result1 = scores.Where(s => s > 80).OrderBy(s=>s);//正序

//多个字段
var memberList = from m in memberSet
                 orderby m.RoleId ascending, m.Name descending
                 select m;
                
var memberList = memberSet.OrderBy(m => m.RoleId).OrderByDescending(m => m.Name);
```

### 分页
```
var result= (from r in db.Product
              where r.rpId > 10
              orderby r.rpId descending
              select r).Skip(10).Take(10); //取第11条到第20条数据    
              
var result1 = db.Product.Where(p => p.rpId > 10).OrderByDescending(p => p.rpId).Skip(10).Take(10).ToList();
```

### 包含，类似like ‘%%’
```
var ss = from r in db.Product
                 where r.SortsText.Contains("来源")
                 select r;
                 
var ss1 = db.Product.Where(p => p.SortsText.Contains("来源")).ToList();
```

### 去重
```
int[] scores = { 90, 71, 82, 93, 75, 90 };

var result = (from s in scores
             where s > 80
             select s).Distinct();  
             
var result1 = scores.Where(s => s > 80).Distinct();

```

### 分组
```
var sums2 = from emp in empList
            group emp by new { emp.Age, emp.Sex } into g
            select new { Peo = g.Key, Count = g.Count() };
            
var sums = empList.GroupBy(x => new { x.Age, x.Sex })
     .Select(group => new {
        Peo = group.Key, Count = group.Count()
     });
```

### 连表
```
//内联
var ss = from p in db.Product
                 join s in db.ShoppingCart on p.Id equals s.pId
                 select r;
                 
var ss1 = db.Product.Join(db.ShoppingCart , p => p.Id, r => r.pid, (p, r) => p).ToList();
        
        
//左联
//加into
//右连，两个表调换顺序
var LeftJoin = from emp in ListOfEmployees
	       join dept in ListOfDepartment
	       on emp.DeptID equals dept.ID into JoinedEmpDept
	       from dept in JoinedEmpDept.DefaultIfEmpty()
	       select new{
                	EmployeeName	= emp.Name,
                	DepartmentName	= dept != null ? dept.Name : null
                };
```

### 复杂连表
需要完成的查询逻辑：A表内联B表，B表左联C表，A表左联D表，并且 C表的TotalCount>23,D表的ClassHour>8,最后查出A表的Id,代码如下：
```
IQueryable<int> data = uow.Biz_AAA.GetAll()
.Join(uow.Biz_BBB.GetAll(), a => a.CertificateId, b => b.Id, (a, b) => new { a, b })
.GroupJoin(uow.Biz_CCC.GetAll(), o => o.b.IDNumber, c => c.IDNumber, (o, c) => new { o.a, o.b, c })
.SelectMany(cc => cc.c.DefaultIfEmpty(), (o, cc) => new { o.a, o.b, c = cc })
.GroupJoin(uow.Biz_DDD.GetAll(), o => o.a.Id, d => d.CertificateDelayApplyRecordId, (o, d) => new { o.a, o.b, o.c, d })
.SelectMany(dd => dd.d.DefaultIfEmpty(), (o, dd) => new { o.a, o.b, o.c, d = dd })
.Select(o => new{
                  CertificateDelayApplyRecordId = o.a.Id,
                  CreateDate = o.a.CreateDate,
                  OnLineClassHour = o.c == null ? 0 : o.c.TotalCount,
                  OffLineClassHour = o.d == null ? 0 : o.d.ClassHour
                })
.Where(o => o.OnLineClassHour > 23 && o.OffLineClassHour > 8)
.Select(o => o.CertificateDelayApplyRecordId);
```

### 加载导航属性
```
public class KerRayOption : Entity
{
    public Guid KerRayId { set; get; }
    public string KerRayOptionText { set; get; }
}
public class KerRay : Entity
{
    public string Ribday { set; get; }

    [System.ComponentModel.DataAnnotations.Schema.ForeignKey("KerRayId")]
    public List<KerRayOption> KerRayOption { set; get; }
}


//默认不加载导航表
var data = await _kerRayRepository.Query.ToListAsync();
//Include 加载导航表，KerRay的KerRayOption有值
var data1 = await _kerRayRepository.Query.Include(_ => _.KerRayOption).ToListAsync();
//查询KerRay的全部数据，并且加载导航表KerRayOptionText=="option1"数据
var data2 = await _kerRayRepository.Query.Include(_ => _.KerRayOption.Where(c => c.KerRayOptionText == "option1")).ToListAsync();
//查询KerRay表Id等于"bf3ffee2-8a48-47d9-94a0-4a227be7f533"的数据并且加载KerRayOption数据，筛选KerRayOption== "option1"
//最终结果是KerRayOption类型
var data3 = await _kerRayRepository.Query.Where(_ => _.Id == Guid.Parse("bf3ffee2-8a48-47d9-94a0-4a227be7f533"))
    .Include(_ => _.KerRayOption).SelectMany(_ => _.KerRayOption.Where(c => c.KerRayOptionText == "option1")).ToListAsync();

// 插入数据包括导航表
var ss = await _kerRayRepository.InsertAsync(new KerRay
{
    Id = Guid.NewGuid(),
    Ribday = "day5",
    KerRayOption = new List<KerRayOption> {
        new KerRayOption {
            Id = Guid.NewGuid(),
        KerRayOptionText="KerRayOptionText5" } }
});
```



鸣谢：
> https://www.cnblogs.com/godbell/p/7349782.html  
> https://www.cnblogs.com/wqw553639736/p/11277091.html  
> https://www.cnblogs.com/wybin6412/p/11464829.html  
> https://blog.csdn.net/qq_41885871/article/details/84024643  
> SelectMany  
> https://www.cnblogs.com/dotnet261010/p/6849980.html  
> http://cw.hubwiz.com/card/c/558cf07ac086935f4a6fb7fb/1/4/9/  