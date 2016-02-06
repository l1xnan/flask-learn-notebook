# 文件的上传
文档结构：

```
/demo
    /tmp
    /templates
        /upload.html
    /upload_demo.py
```

## Form 表单方式
`upload_demo.py` 文件

```py
# -*- coding: utf-8 -*-
import os
from flask import Flask, request, render_template
from werkzeug.utils import secure_filename

basedir = os.path.abspath(os.path.dirname(__file__))

app = Flask(__name__)
app.config['UPLOAD_FOLDER'] = basedir + '\\tmp'
app.config['MAX_CONTENT_LENGTH'] = 16 << 20  # max upload size < 16M


@app.route('/', methods=['GET', 'POST'])
def upload_file():
    if request.method == 'POST':
        file = request.files['file']
        filename = secure_filename(file.filename)
        file.save(os.path.join(app.config['UPLOAD_FOLDER'], filename))
        return "<h>上传成功</h>"
    else:
        return render_template('upload.html')

if __name__ == '__main__':
    app.run(debug=True)
```

`upload.html` 文件

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>上传文件</title>
</head>
<body>
<h1>Upload new File</h1>
<form action="" method=post enctype=multipart/form-data>
    <p>
        <input type=file name=file>
        <input type=submit value=Upload>
    </p>
</form>
</body>
</html>
```

## Ajax 方式
`upload_demo.py` 文件：

```py
# -*- coding: utf-8 -*-
import os
from flask import Flask, request, render_template
from werkzeug.utils import secure_filename

basedir = os.path.abspath(os.path.dirname(__file__))

app = Flask(__name__)
app.config['UPLOAD_FOLDER'] = basedir + '\\tmp'
app.config['MAX_CONTENT_LENGTH'] = 16 << 20  # max upload size < 16M

@app.route('/', methods=['GET', 'POST'])
def upload_file():
    if request.method == 'POST':
        files = request.files.getlist("file")
        for file in files:
            filename = secure_filename(file.filename)
            file.save(os.path.join(app.config['UPLOAD_FOLDER'], filename))
        return "上传成功"
    else:
        return render_template('upload.html')

if __name__ == '__main__':
    app.run(debug=True)
```

`upload.html`  文件：

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>上传文件</title>
    <script src="http://apps.bdimg.com/libs/jquery/2.1.4/jquery.min.js"></script>

</head>
<body>
<h1>Upload new File</h1>
<input type="file" name="file" multiple>
<button id="upload-btn" type="button">upload</button>
<script>
    var files = [];
    $(document).ready(function () {
        $('input').change(function () {
            files = this.files;
        })
    });
    $("#upload-btn").click(function () {
        var fd = new FormData();
        for (var i = 0; i < files.length; i++) {
            fd.append("file", files[i]);
        }
        $.ajax({
            url: "",
            method: 'POST',
            data: fd,
            contentType: false, /// 告诉jQuery不要去设置Content-Type请求头
            processData: false, // 告诉jQuery不要去处理发送的数据
            cache: false,
            success: function (data) {
                console.log(data);
            }
        });
    })
</script>
</body>
</html>
```

注释：

```html
<input type="file" name="file" multiple>
```

- `multiple`: 此设置可以选中多文件上传
- webkitdirectory`: 此设置可以选中文件夹上传

参见：
- [使用 FormData 对象](https://developer.mozilla.org/zh-CN/docs/Web/Guide/Using_FormData_Objects)
- [通过 jQuery Ajax 使用 FormData 对象上传文件](http://www.jianshu.com/p/46e6e03a0d53)

## 拖拽上传
### HTML 5 拖放
拖放（Drag 和 drop）是 HTML5 标准的组成部分。 Drag & Drop 包括以下事件：
- `dragstart`：要被拖拽的元素开始拖拽时触发，这个事件对象是被拖拽元素
- `dragenter`：拖拽元素进入目标元素时触发，这个事件对象是目标元素
- `dragover`：拖拽某元素在目标元素上移动时触发，这个事件对象是目标元素
- `dragleave`：拖拽某元素离开目标元素时触发，这个事件对象是目标元素
- `drop`：将被拖拽元素放在目标元素内时触发，这个事件对象是目标元素
- `dragend`：在 drop 之后触发，就是拖拽完毕时触发，这个事件对象是被拖拽元素

完成一次成功页面内元素拖拽的行为事件过程应该是： `dragstart` –> `dragenter` –> `dragover` –> `drop` –> `dragend`

HTML5 为元素新增了用于拖拽的属性  `draggable`，这个属性决定了元素是否能被拖拽，如果 `draggable="true"，`则元素可被拖拽，否则只能选择元素的文本。

在进行拖放操作时，用 DataTransfer 对象来保存被拖动的数据。它可以保存一项或多项数据、一种或者多种数据类型。dataTransfer 对象包含以下属性和方法：

属性：
- `dropEffect` ：返回已选择的拖放效果，如果该操作效果与起初设置的 effectAllowed 效果不符，则拖拽操作失败。可以设置修改，包含这几个值："none", "copy", "link" 和 "move"。
- `effectAllowed` ：返回允许执行的拖拽操作效果，可以设置修改，包含这些值：
  - "none"
  - "copy"
  - "copyLink"
  - "copyMove"
  - "link"
  - "linkMove"
  - "move"
  - "all"
  - "uninitialized"

- `files`：包含一个在数据传输上所有可用的本地文件列表。如果拖动操作不涉及拖动文件，此属性是一个空列表。此属性访问指定的 FileList 中无效的索引将返回未定义（undefined）。
- `types`：返回在 dragstart 事件出发时为元素存储数据的格式，如果是外部文件的拖拽，则返回"files" dataTransfer.clearData ([format] )：删除指定格式的数据，如果未指定格式，则删除当前元素的所有携带数据 dataTransfer.setData(format, data)：为元素添加指定数据

方法：
- `setData()`：为一个给定的类型设置数据。如果该数据类型不存在，它将添加到的末尾，这样类型列表中的最后一个项目将是新的格式。如果已经存在的数据类型，替换相同的位置的现有数据。就是，当更换相同类型的数据时，不会更改类型列表的顺序。
- `getData`(format)：返回指定数据，如果数据不存在，则返回空字符串 dataTransfer.files：如果是拖拽文件，则返回正在拖拽的文件列表 FileList
- `setDragImage(element, x, y)`：制定拖拽元素时跟随鼠标移动的图片，x、y 分别是相对于鼠标的坐标。
- `addElement(element)`：添加一起跟随拖拽的元素，如果你想让某个元素跟随被拖拽元素一同被拖拽，则使用此方法 (据测试，Chrome 暂不支持)
- `clearData()`：
- 删除与给定类型关联的数据。类型参数是可选的。如果类型为空或未指定，将删除所有类型相关联的数据。如果不存在指定类型的数据，或数据传输不包含任何数据，此方法将没有任何效果。
- 在 `dragstart` 事件触发时可以为被拖拽元素存储数据，就是使用上面说到的 `dataTransfer.setData`，`setData` 的数据格式一般有两种：`text/plain`(用于文本数据) 和`text/uri-list`(用于 url)，你可以先为某个可拖拽元素设置为数据，然后为它设置 `draggable` 属性为 true，之后在其 `dragstart` 事件触发时存储数据：

参见：[DataTransfer](https://developer.mozilla.org/zh-CN/docs/Web/API/DataTransfer)

### FileReader
`readAsDataURL` 方法：参数为要读取的文件对象，将文件读取为 DataUrl `onload` 事件：当读取文件成功完成的时候触发此事件

### 实现

```html
<!DOCTYPE HTML>
<html>
<head>
    <script type="text/javascript">
        $(function () {
            //阻止浏览器默认行。
            $(document).on({
                "dragleave": function (e) {    //拖离
                    e.preventDefault();
                },
                "drop": function (e) {  //拖后放
                    e.preventDefault();
                },
                "dragenter": function (e) {    //拖进
                    e.preventDefault();
                },
                "dragover": function (e) {    //拖来拖去
                    e.preventDefault();
                }
            });
            var box = document.getElementById('drop_area'); //拖拽区域
            box.addEventListener("drop", function (e) {
                e.preventDefault(); //取消默认浏览器拖拽效果
                var fileList = e.dataTransfer.files; //获取文件对象
                //检测是否是拖拽文件到页面的操作
                if (fileList.length == 0) {
                    return false;
                }
                //检测文件是不是图片
                if (fileList[0].type.indexOf('image') === -1) {
                    alert("您拖的不是图片！");
                    return false;
                }

                //拖拉图片到浏览器，可以实现预览功能
                var img = window.webkitURL.createObjectURL(fileList[0]);
                var filename = fileList[0].name; //图片名称
                var filesize = Math.floor((fileList[0].size) / 1024);
                if (filesize > 500) {
                    alert("上传大小不能超过500K.");
                    return false;
                }
                var str = "<img src='" + img + "'><p>图片名称：" + filename + "</p><p>大小：" + filesize + "KB</p>";
                $("#preview").html(str);

                //上传
                xhr = new XMLHttpRequest();
                xhr.open("post", "upload.php", true);
                xhr.setRequestHeader("X-Requested-With", "XMLHttpRequest");

                var fd = new FormData();
                fd.append('mypic', fileList[0]);

                xhr.send(fd);
            }, false);
        });
    </script>
</head>
<body>
<div id="drop_area">将图片拖拽到此区域</div>
<div id="preview"></div>
</body>
</html>
```

```
event.preventDefault()
```

取消事件的默认动作。该方法将通知 Web 浏览器不要执行与事件关联的默认动作。例如，如果 type 属性是 "submit"，在事件传播的任意阶段可以调用任意的事件句柄，通过调用该方法，可以阻止提交表单。

参见：
- [JavaScript 文件拖拽上传插件 dropzone.js 介绍](https://www.renfei.org/blog/dropzone-js-introduction.html)
- [文件上传的渐进式增强](http://www.ruanyifeng.com/blog/2012/08/file_upload.html)
- [jQuery+FormData + 文件上传 + 上传进度](https://segmentfault.com/a/1190000000393302)

## WebSocket
- [Flask-SocketIO](https://flask-socketio.readthedocs.org/en/latest/)
