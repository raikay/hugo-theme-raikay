+++
author = "raikay"  
title = "jqurey datatable  tableTools  自定义button元素 以及按钮定义事件"  
date = "2017-05-13"  
description = "jqurey datatable  tableTools  自定义button元素 以及按钮定义事件"  
tags = [
    " 前端",
]  

+++

### 版本 1.10.4

```js
"dom": 'T<"clear">lfrtip',
            "tableTools": {
                //"sSwfPath": "~/Resources/Scripts/plugins/DataTables/swf/copy_csv_xls_pdf.swf"

                "sRowSelect": "multi",
                "aButtons": [
                       //{ "sExtends": "new_record", "sButtonText": "Add" },
                       {
                           "sExtends": "select", "sButtonText": "Delete Recods",
                           "fnClick": function (nButton, oConfig, oFlash) {
                               //delete stuff comes here 
                               alert('test');
                           }

                       }
                ]
            }
```



### 全部代码：

```html
@Styles.Render("~/bundles/css/datatable")
@*<script src="~/Resources/Scripts/plugins/DataTables/js/dataTables.tableTools.js"></script>
    <link href="~/Resources/Scripts/plugins/DataTables/css/data-table.css" rel="stylesheet" />*@
<div>
    <ol class="breadcrumb pull-right">
        @if (ViewBag.Breadcrumbs != null)
        {
            var dict = ViewBag.Breadcrumbs as Dictionary<string, string[]>;
            foreach (var dictItem in dict)
            {
                if (dictItem.Value == null)
                {
                    <li class="active">@dictItem.Key</li>
                }
                else
                {
                    <li>@Ajax.ActionLink(dictItem.Key, dictItem.Value[1], dictItem.Value[0], new AjaxOptions { UpdateTargetId = "content" })</li>
                }
            }
        }
    </ol>
    <h1 class="page-header">
        @*<small>操作日志管理</small>*@
    </h1>

</div>

<div class="row">
    <!-- begin col-12 -->
    <div class="col-md-12 ui-sortable">
        <!-- begin panel -->
        <div class="panel panel-inverse">
            <div class="panel-heading">
                <div class="panel-heading-btn">
                    <!----工具栏--->
                    <a href="javascript:;" class="btn btn-xs btn-icon btn-circle btn-default" data-click="panel-expand" data-original-title="" title=""><i class="fa fa-expand"></i></a>
                    <a href="javascript:;" class="btn btn-xs btn-icon btn-circle btn-success" data-click="panel-reload" data-original-title="" title=""><i class="fa fa-repeat"></i></a>
                    @*<a href="javascript:;" class="btn btn-xs btn-icon btn-circle btn-warning" data-click="panel-collapse"><i class="fa fa-minus"></i></a>*@
                    @*<a href="javascript:;" class="btn btn-xs btn-icon btn-circle btn-danger" data-click="panel-remove"><i class="fa fa-times"></i></a>*@
                </div>

                <h4 class="panel-title">
                    查看账户<button type="button" class="btn btn-success btn-xs" style="margin-left:20px"><i class="fa fa-plus"></i> 添加账户</button>
                </h4>

            </div>
            <div class="panel-body">

                <!--------------------表头开始------------------->
                <div class="table-responsive">
                    <div id="data-table_wrapper" class="dataTables_wrapper no-footer">
                        <table id="data-table" class="table table-striped table-bordered dataTable no-footer" role="grid" aria-describedby="data-table_info">
                            <thead>
                                <tr><th>ID</th><th>内容</th><th>时间</th><th>类别</th><th>域</th><th>用户名</th><th>操作</th></tr>
                            </thead>

                        </table>
                    </div>
                </div>
                <!--------------------表头结束------------------->
            </div>
        </div>
        <!-- end panel -->
    </div>
    <!-- end col-12 -->
</div>
@Scripts.Render("~/bundles/DataTable/js")
<script>

    //datatable 开始
    $(document).ready(function () {
        handlePanelAction();
        $('#data-table').dataTable({

            "oLanguage": {
                "sLengthMenu": "每页显示 _MENU_ 条记录",
                "sZeroRecords": "没有检索到数据",
                "sInfo": "从 _START_ 到 _END_ /共 _TOTAL_ 条数据",
                "sInfoEmpty": "没有数据",
                "sInfoFiltered": "(从 _MAX_ 条数据中检索)",
                "sSearch": "查找账户",
                "oPaginate": {
                    "sFirst": "首页",
                    "sPrevious": "前一页",
                    "sNext": "后一页",
                    "sLast": "尾页"
                }
            },
            "sServerMethod": "POST",
            "sAjaxSource": "@Url.Action("GetAllLog")",
            "bServerSide": true,
            "sPaginationType": "full_numbers",
            "aoColumns": [{
                "mData": "ID",
                "sDefaultContent": "",
                "bVisible": false
            },
            {
                "mData": "Content",
                "mRender": function (data, type, full) {
                    //插件固定参数   data:mData接受的数据   full:全部json

                    if (data.length > 20) {
                        data = data.substring(0, 20) + "...";
                    }
                    return "<a href='javascript:void(0)' onclick='myonclick(" + full.ID + ")'>" + data + "</a>"
                }

            },
             {
                 "mData": "CreateTime",
                 "sDefaultContent": "",
                 "sClass": "center",

             }, {
                 "mData": "Category",
                 "sDefaultContent": "",
                 "sClass": "center"
             }, {
                 "mData": "DomainID",
                 "sDefaultContent": "",
                 "sClass": "center"
             }, {
                 "mData": "AccountName",
                 "sDefaultContent": "",
                 "sClass": "center"
             },
             {
                 //删除
                 "mData": "ID",
                 "sDefaultContent": "",
                 "sClass": "center",
                 "mRender": function (data, type, full) {
                     //插件固定参数   data:mData接受的数据   full:全部json
                     return "<button  class='btn btn-danger' onclick='deleteData(" + data + ")'>删除</button>";
                 }

             }],
            "columnDefs": [{ "bSortable": false, "aTargets": [2, 3, 4, 5, 6] }],

            "dom": 'T<"clear">lfrtip',
            "tableTools": {
                //"sSwfPath": "~/Resources/Scripts/plugins/DataTables/swf/copy_csv_xls_pdf.swf"

                "sRowSelect": "multi",
                "aButtons": [
                       //{ "sExtends": "new_record", "sButtonText": "Add" },
                       {
                           "sExtends": "select", "sButtonText": "Delete Recods",
                           "fnClick": function (nButton, oConfig, oFlash) {
                               //delete stuff comes here
                               alert('test');
                           }

                       }
                ]
            }
        });
    });

    //datatable 结束




    function myconfirm() {
        return confirm("您确定？");
    };
    function deleteData(id) {
        if (confirm("确定删除？")) {
            alert("删除成功  " + id);
        } else {

        }

    }
    function myonclick(id) {
        $.ajax({
            url: "/LogManager/LogInfo?log_id=" + id,
            cache: false,
            success: function (html) {
                $("#content").html("");
                $("#content").append(html);
            }
        });


    };
</script>


@*<script type="text/javascript" language="javascript" class="init">
        $(document).ready(function () {
            var t = $('#data-table').DataTable();
            var counter = 1;
            $('#addRow').on('click', function () {
                t.row.add([
                    counter + '.1',
                    '<input type="button" value="button"/>',
                    '<a href="xxx">超链接</a>',
                    counter + '.4',
                    counter + '.5'
                ]).draw();

                counter++;
            });
            // Automatically add a first row of data
            $('#addRow').click();
        });
    </script>*@
```


