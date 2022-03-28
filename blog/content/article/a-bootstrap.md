+++
author = "Raikay"  
title = "对bootstrap中confirm alert进行封装"  
date = "2020-12-06"  
description = "对bootstrap中confirm alert进行封装 弹窗 修改 居中"  

+++


### 效果图：

![IMG](https://raikay.coding.net/p/code/d/m1/git/raw/master/2020/12/06/20201206184525.png)



### HTML:

```html
<!-- system modal start -->
    <div id="ycf-alert" class="modal">
      <div class="modal-dialog modal-sm">
        <div class="modal-content">
          <div class="modal-header">
            <button type="button" class="close" data-dismiss="modal"><span aria-hidden="true">×</span><span class="sr-only">Close</span></button>
            <h5 class="modal-title"><i class="fa fa-exclamation-circle"></i> [Title]</h5>
          </div>
          <div class="modal-body small">
            <p>[Message]</p>
          </div>
          <div class="modal-footer" >
            <button type="button" class="btn btn-primary ok" data-dismiss="modal">[BtnOk]</button>
            <button type="button" class="btn btn-default cancel" data-dismiss="modal">[BtnCancel]</button>
          </div>
        </div>
      </div>
    </div>
  <!-- system modal end -->
```

不多说了，都是bootstrap modal的样式，不熟悉的朋友可以查查 bootstrap css ，中括号[....]的内容会最终替换为我们传入的参数显示。

### JS代码如下：

```javascript
$(function () {
  window.Modal = function () {
    var reg = new RegExp("\\[([^\\[\\]]*?)\\]", 'igm');
    var alr = $("#ycf-alert");
    var ahtml = alr.html();

    //关闭时恢复 modal html 原样，供下次调用时 replace 用
    //var _init = function () {
    //    alr.on("hidden.bs.modal", function (e) {
    //        $(this).html(ahtml);
    //    });
    //}();

    /* html 复原不在 _init() 里面做了，重复调用时会有问题，直接在 _alert/_confirm 里面做 */


    var _alert = function (options) {
      alr.html(ahtml);    // 复原
      alr.find('.ok').removeClass('btn-success').addClass('btn-primary');
      alr.find('.cancel').hide();
      _dialog(options);

      return {
        on: function (callback) {
          if (callback && callback instanceof Function) {
            alr.find('.ok').click(function () { callback(true) });
          }
        }
      };
    };

    var _confirm = function (options) {
      alr.html(ahtml); // 复原
      alr.find('.ok').removeClass('btn-primary').addClass('btn-success');
      alr.find('.cancel').show();
      _dialog(options);

      return {
        on: function (callback) {
          if (callback && callback instanceof Function) {
            alr.find('.ok').click(function () { callback(true) });
            alr.find('.cancel').click(function () { callback(false) });
          }
        }
      };
    };

    var _dialog = function (options) {
      var ops = {
        msg: "提示内容",
        title: "操作提示",
        btnok: "确定",
        btncl: "取消"
      };

      $.extend(ops, options);

      console.log(alr);

      var html = alr.html().replace(reg, function (node, key) {
        return {
          Title: ops.title,
          Message: ops.msg,
          BtnOk: ops.btnok,
          BtnCancel: ops.btncl
        }[key];
      });
      
      alr.html(html);
      alr.modal({
        width: 500,
        backdrop: 'static'
      });
    }

    return {
      alert: _alert,
      confirm: _confirm
    }

  }();
});
```

