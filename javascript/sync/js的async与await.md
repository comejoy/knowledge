
# 异步编程之async、await

## 使用方法

## idea使用前需要修改ECMA版本在6或者以上
setting => Languages & Frameworks => JavaScript >> JavaScript language version 

选择ECMAScript 6或者以上

## demo
````angular2
<script>
    async function initSelect() {
        var tmp1 = initCodeTableSelect('menu_type', '/base/getMenuTypeList', null);
        var tmp2 = initMenuList();

        await tmp1;
        await tmp2;
    }

    async function init(data) {
        layui.use(["form", "jquery"], function () {
            var form = layui.form, $ = layui.jquery;

            initSelect();

            console.log("执行 init");

            form.val('select-condition', {
                "fatherMenuId": data.fatherMenuId,
                "menuType": data.menuType
            });

            form.render('select');
        });
    }

    function initMenuList() {
        layui.use(["form", "jquery"], function () {
            var form = layui.form, $ = layui.jquery;

            $.ajax({
                url: 'getRootList',
                type: 'get',
                async: false,
                success: function (result) {
                    var selection = $('#father_menu_id');
                    selection.find("option").remove();

                    var alloption = '<option value=\"\" selected>请选择</option>';
                    selection.append(alloption);
                    result.forEach(function (currval, index, array) {
                        var option = '';
                        option = '<option value=' + currval.id + '>' + currval.title + '</option>';
                        selection.append(option);
                    });
                    form.render('select');
                    console.log("渲染父菜单数据");
                },
                error: function (data) {
                    layer.msg("初始化select失败, select id:" + id.toString());
                }
            });
            console.log("ajax 父菜单渲染执行over");
        });
    }
</script>
````

## reference
[JavaScript Promise迷你书](http://liubin.org/promises-book/)