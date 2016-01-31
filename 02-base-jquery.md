# jQuery 学习笔记

$$\sum i = 5050$$

## Ajax

`$(function() { ... })` 将会在浏览器加载完页面的基础内容之后立即执行。

`$('selector')` 选择一个用于操作的元素。

`element.bind('event', func)` 指定元素被单击时运行的函数，如果这个函数返回 false ，那么单击操作的默认行为将被取消。在本例中，点击操作的默认行为是导航到 # 链接标签。

### get 方法

使用一个 HTTP GET 请求从服务器加载数据。

    $(selector).get(url,data,success(response,status,xhr),dataType)  
参及描述
* `url` - 必需。规定将请求发送的哪个 URL。
* `data` - 可选。规定连同请求发送到服务器的数据。
* `success(response,status,xhr)`	 - 可选。规定当请求成功时运行的函数。
额外的参数：
    * `response` - 包含来自请求的结果数据
    * `status` - 包含请求的状态
    * `xhr` - 包含 XMLHttpRequest 对象
* `dataType` - 可选。规定预计的服务器响应的数据类型。
默认地，jQuery 将智能判断。
可能的类型：xml, html, text, script, json, jsonp

该函数是简写的 Ajax 函数，等价于：
```js
$.ajax({
  url: url,
  data: data,
  success: success,
  dataType: dataType
});
```


### getJSON 方法
使用一个 HTTP GET 请求从服务器加载 JSON 编码的数据。

    jQuery.getJSON(url,data,success(data,status,xhr))
    
    
参数及描述：
* `url` -- 必需。规定将请求发送的哪个 URL。
* `data` -- 可选。规定连同请求发送到服务器的数据。 
* `success(data,status,xhr)` -- 可选。规定当请求成功时运行的函数。
额外的参数：
    * `response` - 包含来自请求的结果数据
    * `status` - 包含请求的状态
    * `xhr` - 包含 XMLHttpRequest 对象

这是一个 Ajax 函数的缩写，这相当于:
```js
$.ajax({
  dataType: "json",
  url: url,
  data: data,
  success: success
});
```
数据会被附加到一个查询字符串的 URL 中，发送到服务器。如果该值的data参数是一个普通的对象，它会转换为一个字符串并使用 URL 编码，然后才追加到 URL 中。

在success回调中传入返回的数据，通常是一个 JavaScript 对象或数组所定义的 JSON 结构，使用 $.parseJSON()方法解析。它（success回调）也传入了响应状态文本。

### post 方法
使用一个 HTTP POST 请求从服务器加载数据。

    jQuery.post(url,data,success(data, textStatus, jqXHR),dataType)

参数及描述
* `url` - 必需。规定把请求发送到哪个 URL。
* `data` - 可选。发送给服务器的字符串或 Key/value 键值对。
* `success(data, textStatus, jqXHR)` - 可选。请求成功后执行的回调函数。
* `dataType` - 可选。规定预期的服务器响应的数据类型。
默认执行智能判断（xml、json、script 或 html）。

这相当于：
```js
$.ajax({
  type: "POST",
  url: url,
  data: data,
  success: success,
  dataType: dataType
});
```
`success` 回调函数会传入返回的数据，是根据 MIME 类型的响应，它可能返回的数据类型包括 XML 根节点, 字符串, JavaScript 文件, 或者 JSON 对象。同时还会传入描述响应状态的字符串。







