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
    <script>
        var files = [];
        $(document).ready(function () {
            $('input').change(function () {
                files = this.files;
            })
        });
        $("#upload-btn").click(function () {
            var fd = new FormData();    // 列表
            for (var i = 0; i < files.length; i++) {
                fd.append("file", files[i]);
            }
            $.ajax({
                url: "",
                method: 'POST',
                data: fd,
                contentType: false,
                processData: false,
                cache: false,
                success: function (data) {
                    console.log(data);
                }
            });
        })
    </script>
</head>
<body>
<h1>Upload new File</h1>
<input type="file" name="file" multiple>
<button id="upload-btn" type="button">upload</button>
</body>
</html>
```

注释：

```html
<input type="file" name="file" multiple>
```

- `multiple`: 此设置可以选中多文件上传
- webkitdirectory`: 此设置可以选中文件夹上传
