# 【layui】tepmlet 格式化 table 数据


<!--more-->

### 代码如下所示

```js
<script src="/static/vendor/layuimini/lib/layui-v2.5.5/layui.js" charset="utf-8"></script>
 
<script>
    layui.use(['form', 'table'], function () {
        var $ = layui.jquery,
            form = layui.form,
            table = layui.table;

        table.render({
            elem: '#currentTableId',
            url: '/user/queryjson',
            toolbar: '#toolbarDemo',
            defaultToolbar: ['filter', 'exports', 'print', {
                title: '提示',
                layEvent: 'LAYTABLE_TIPS',
                icon: 'layui-icon-tips'
            }],
            cols: [[
                {type: "checkbox", width: 50},
                {field: 'id', width: 80, title: 'ID', sort: true},
                {field: 'name', width: 80, title: '用户名'},
                {field: 'sex', width: 80, title: '性别', sort: true, templet: sexFormat},
                {field: 'tel', width: 120, title: '联系方式'},
                {field: 'addr', width: 120, title: '地址'},
                {field: 'birthday', width: 180, title: '生辰', templet: birFormat},
                {title: '操作', minWidth: 150, toolbar: '#currentTableBar', align: "center"}
            ]],
            limits: [10, 15, 20, 25, 50, 100],
            limit: 15,
            page: true,
            skin: 'line'
        });
        // 格式化显示男女信息，后端返回 true 或者 false，这里做判断
        function sexFormat(d) {
            var str;
            if (d.sex) {
                str = '男';
            } else {
                str = '女';
            }
            return str;
        }
 	 			// 格式化显示日期，调用layui.util 工具集，参考：https://www.layui.com/doc/modules/util.html
        function birFormat(d) {
            birday = layui.util.toDateString(d.birthday, "yyyy-MM-dd");
            return birday;
        }
    });
</script>
```



###参考

https://m.yisu.com/zixun/12217.html
