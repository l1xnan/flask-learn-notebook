# 表格
主要用到 `jqGrid` 插件。

head 中加入引用
- css 文件引入：
- <link type="text/css" rel="stylesheet" href="jqGrid/themes/cupertino/jquery-ui-x.x.x.custom.min.css">
<link type="text/css" rel="stylesheet" href="jqGrid/themes/ui.jqgrid.css">
- js 文件引入：
- <script type="text/javascript" src="jquery-x.x.x.min.js" />
<script type="text/javascript" src="jqGrid/js/jquery-ui-x.x.x.custom.min.js"/>
<script type="text/javascript" src="jqGrid/js/i18n/grid.locale-cn.js"/>
<script type="text/javascript" src="jqGrid/js/jquery.jqGrid.min.js"/>

## 属性大全

属性名        | 默认值   | 说明
---------- | ----- | --------
rownumbers | false | 添加左侧行号
altRows    | false | 设置为交替行表格
autowidth  | false | 自动设置表格宽度

## 获取属性值

```js
colNames=$("#UsersGrid").jqGrid('getGridParam','colNames')
colModel=$("#UsersGrid").jqGrid('getGridParam','colModel')
```

## 表格宽度的调整
jqGrid 随窗口大小变化自适应宽度：

```js
$(function(){
            $(window).resize(function(){   
         $("#listId").setGridWidth($(window).width());
        });
       });
```

jqgrid 属性：
- `width`：Grid 的宽度，如果未设置，则宽度应为所有列宽的之和；如果设置了宽度，则每列的宽度将会根据 `shrinkToFit` 选项的设置，进行设置。　　
- `shrinkToFit`：此选项用于根据 width 计算每列宽度的算法。默认值为 true。如果 `shrinkToFit` 为 true 且设置了 width 值，则每列宽度会根据 width 成比例缩放；如果 `shrinkToFit` 为 false 且设置了 width 值，则每列的宽度不会成比例缩放，而是保持原有设置，而 Grid 将会有水平滚动条。　　
- `autowidth`：默认值为 false。如果设为 true，则 Grid 的宽度会根据父容器的宽度自动重算。重算仅 发生在 Grid 初始化的阶段；如果当父容器尺寸变化了，同时也需要变化 Grid 的尺寸的话，则需要在自己的代码中调用 setGridWidth 方法来完成。这些属性只能是保证第一次时的宽度，当浏览器大小变化如还想让表格宽度自适应，就需要用 jqgrid 的方法 setGridWidth, 它有两个参 数，new_width,shr，当第二个参数不设置时会按照 `shrinkToFit` 的设置值或默认值，而第一个参数则要设置的新的宽度值，所以在些可用 js 实现对浏览器宽度变化的自适应：　　
- ```js
- $(function(){　　
- $(window).resize(function(){　　
- $("#analyDataTab").setGridWidth($(window).width()*0.99);
- $("#charDataTab").setGridWidth(document.body.clientWidth*0.99);　　
- })});
- ```　
- 注：这里的百分比可按自己需要来设定，也可直接是浏览器的宽度大小。

## 刷新 jqGrid 数据。
参见：[http://blog.csdn.net/y0ungroc/article/details/12008879](http://blog.csdn.net/y0ungroc/article/details/12008879) 常用到刷新 jqGrid 数据的情况是，在用到查询的时候，根据查询条件，请求数据，并刷新 jqGrid 表格，使用方式如下：

```js
$("#search_btn").click( function (){
    // 此处可以添加对查询数据的合法验证  
    var data = $("#query").val();
    $("#list4").jqGrid('setGridParam',{
        datatype:'json',
        postData:{'query':data}, // 发送数据  
        page:1
    }).trigger("reloadGrid"); // 重新载入  
});
```

① setGridParam 用于设置 jqGrid 的 options 选项。返回 jqGrid 对象  ② datatype 为指定发送数据的格式；  ③ postData 为发送请求的数据，以 key:value 的形式发送，多个参数可以以逗号"," 间隔；  ④ page 为指定查询结果跳转到第一页；  ⑤ trigger("reloadGrid"); 为重新载入 jqGrid 表格。

## 无数据的提示信息。
当后台返回数据为空时，jqGrid 本身的提示信息在右下角，不是很显眼，下面方法将实现在无数据显示的情况下，在 jqGrid 表格中间位置提示 "无数据显示"。

```js
loadComplete:function() {// 如果数据不存在，提示信息
    var rowNum = $("#list4").jqGrid('getGridParam','records');
    if (rowNum)
        if($("#norecords").html() == null){
            $("#list4").parent().append("</pre><div>id="norecords"> 没有查询记录！</div><pre>");
        }
        $("#norecords").show();
    }else{// 如果存在记录，则隐藏提示信息。
        $("#norecords").hide();
    }
}
```

① loadComplete 为 jqGrid 加载完成，执行的方法； ② getGridParam 这个方法用来获得 jqGrid 的选项值。它具有一个可选参数 name，name 即代表着 jqGrid 的选项名，如果不传入 name 参数，则会返回 jqGrid 整个选项 options。例：

```js
$("#list4").jqGrid('getGridParam','records');// 获取当前 jqGrid 的总记录数；
```

注意：这段代码要加在 jqGrid 的选项设置 Option 之间，即：`$("#list4").jqGrid({});` 代码之间。且各个 option 之间加逗号间隔。

## jqGrid 的数据格式化。
jqGrid 中对列表 cell 属性格式化设置主要通过 colModel 中 formatter、formatoptions 来设置

基本用法：

```js
jQuery("#jqGrid_id").jqGrid({
...
   colModel:[
      ...
      {name:'price',index:'price',  formatter:'integer', formatoptions:{thousandsSeparator:','}},
      ...
   ]
...
});
```

formatter 主要是设置格式化类型 (integer、email 等以及函数来支持自定义类型),formatoptions 用来设置对应 formatter 的参数，jqGrid 中预定义了常见的格式及其 options：

integer thousandsSeparator： // 千分位分隔符, defaulValue number decimalSeparator, // 小数分隔符，如"." thousandsSeparator, // 千分位分隔符，如"," decimalPlaces, // 小数保留位数 defaulValue currency decimalSeparator, // 小数分隔符，如"." thousandsSeparator, // 千分位分隔符，如"," decimalPlaces, // 小数保留位数 defaulValue, prefix // 前缀，如加上"$" suffix// 后缀 date srcformat, //source 的本来格式 newformat // 新格式 email 没有参数，会在该 cell 是 email 加上： mailto:name@domain.com showlink baseLinkUrl, // 在当前 cell 中加入 link 的 url，如"jq/query.action" showAction, // 在 baseLinkUrl 后加入 & action=actionName addParam, // 在 baseLinkUrl 后加入额外的参数，如"&name=aaaa" target, idName // 默认会在 baseLinkUrl 后加入, 如".action?id=1″。改如果设置 idName="name", 那么".action?name=1″。其中取值为当前 rowid

checkbox disabled //true/false 默认为 true 此时的 checkbox 不能编辑，如当前 cell 的值是 1、0 会将 1 选中 select 设置下拉框，没有参数，需要和 colModel 里的 editoptions 配合使用

colModel:[

```
{name:'id',    index:'id',     formatter:  customFmatter},

{name:'name',  index:'name',   formatter:"showlink",formatoptions:{baseLinkUrl:"save.action",idName:"id",addParam:"&name=123"}},

{name:'price', index:'price',  formatter:"currency", formatoptions:{thousandsSeparator:",",decimalSeparator:".",prefix:"$"}},

{name:'email', index:'email',  formatter:"email"},

{name:'amount',index:'amount', formatter:"number", formatoptions:{thousandsSeparator:",", defaulValue:"",decimalPlaces:3}},

{name:'gender',index:'gender', formatter:"checkbox",formatoptions:{disabled:false}},

{name:'type',  index:'type',   formatter:"select",editoptions:{value:"0: 无效; 1: 正常; 2: 未知 "}}
```

]

其中 customFmatter 声明如下：

function customFmatter(cellvalue, options,rowObject){

```
console.log(cellvalue);

console.log(options);

console.log(rowObject);

return "["+cellvalue+"]";
```

};

在页面显示的效果如下：

当然还得支持自定义 formatter 函数，只需要在 formatter:customFmatter 设置 formatter 函数，该函数有三个签名：

function customFmatter(cellvalue, options,rowObject){

}

//cellvalue- 当前 cell 的值

//options- 该 cell 的 options 设置，包括 {rowId, colModel,pos,gid}

//rowObject- 当前 cell 所在 row 的值，如 {id=1, name="name1",price=123.1, ...}

当然对于自定义 formatter，在修改时需要获取原来的值，这里就提供了 unformat 函数，这里见官网的例子：

```js
jQuery("#grid_id").jqGrid({
...
   colModel:[
      ...
      {name:'price',index:'price', width:60, align:"center", editable:true, formatter:imageFormat,unformat:imageUnFormat},
      ...
   ]
...
});
function imageFormat(cellvalue, options,rowObject){
    return '</pre><imgsrc="'+cellvalue+'"alt="" /><pre>';
}
function imageUnFormat(cellvalue, options, cell){
    return $('img', cell).attr('src');
}
```

Format: [http://www.ok-soft-gmbh.com/jqGrid/ranking1_441.htm](http://www.ok-soft-gmbh.com/jqGrid/ranking1_441.htm)

jqGrid 行编辑配置: [http://www.w3dev.cn/article/20130702/jqGrid-inline-edit-config.aspx](http://www.w3dev.cn/article/20130702/jqGrid-inline-edit-config.aspx) [http://blog.csdn.net/ceoshun/article/details/24252235](http://blog.csdn.net/ceoshun/article/details/24252235)

jqGrid 与 Struts2 的结合应用（四） ---- 丰富多彩的 Pager Bar: [http://blog.csdn.net/gengv/article/details/5720707](http://blog.csdn.net/gengv/article/details/5720707) [https://github.com/jiangyuan/blog/tree/master/note/docOfjqGrid](https://github.com/jiangyuan/blog/tree/master/note/docOfjqGrid) 下拉多选框: [http://www.erichynds.com/examples/jquery-ui-multiselect-widget/demos/#headers](http://www.erichynds.com/examples/jquery-ui-multiselect-widget/demos/#headers) jquery 操作复选框 (checkbox) 的 12 个小技巧总结
