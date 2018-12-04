#异步编程之Promise

## demo
````angular2
    layui.use(["form", "jquery"], function () {
        var form = layui.form, $ = layui.jquery;

        var step1 = new Promise(function (resolve, reject) {
            $.ajax({
                type: "POST",
                url: '/base/getMenuTypeList',
                async: false,
                success: function (result) {
                    var selection = $('#menuType');
                    selection.find("option").remove();
                    selection.append(alloption);
                    result.data.forEach(function (currval, index, array) {
                        var option = '';
                        option = '<option value=' + currval.sysValue + '>' + currval.codeDesc + '</option>';
                        selection.append(option);
                    });
                    form.render('select', 'select-condition');
                    resolve("resolve1:渲染菜单类型数据");
                    console.log("渲染菜单类型数据");
                },
                error: function (data) {
                    layer.msg("初始化select失败, select id:");
                }
            });
        });
        var step2 = new Promise(function (resolve, reject) {
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
                    resolve("resolve2:渲染父菜单数据");
                },
                error: function (data) {
                    layer.msg("初始化select失败, select id:" + id.toString());
                }
            });
            console.log("ajax 父菜单渲染执行over");
        });
        Promise.all([step1, step2]).then(function (results) {
            console.log("promise execute success,");
            console.log(results);
        }).catch(function (results) {
            console.log("fail promise execute");
            console.log(results);
        });

        console.log("执行加载数据");
        // if (!isRender) {
        //     isRender = true;
        //
        //     initCodeTableSelect('menu_type', '/base/getMenuTypeList', null);
        //     initMenuList();
        // }

        console.log("执行 init");

        form.val('select-condition', {
            "fatherMenuId": data.fatherMenuId,
            "menuType": data.menuType
        });

        form.render('select');


    });
````

## reference
[理解javascript的async/await](https://segmentfault.com/a/1190000007535316)