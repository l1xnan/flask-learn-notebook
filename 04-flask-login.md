# 登陆

```
pip install flask-login
```

目录结构：

```
+---------------------
|   config.py
|   manage.py
|   student_db.sqlite
+---app-------------
|   |   models.py
|   |   __init__.py
|   |
|   +---main
|   |       forms.py
|   |       views.py
|   |       __init__.py   
|   |
|   +---static
|   |   |   favicon.ico
|   |   +---css
|   |   |       bootstrap-theme.css
|   |   |       bootstrap.css
|   |   +---js
|   |           bootstrap.js
|   |           jquery.js
|   +---templates
|           base.html
|           index.html
|           login.html
+---migrations
```

`app/__init__.py` 代码片段：

```py
from flask.ext.login import LoginManager
login_manager = LoginManager()
login_manager.session_protection = 'strong'
login_manager.login_view = 'main.login' # 未登陆时重新定向的路由
```

- `LoginManager.session_protection`: 为会话提供不同的安全等级防止用户会话遭篡改。可设值：
  - `None`: 禁用。
  - `"strong"`: Flask-Login 会记录客户端 IP 地址和浏览器的用户代理信息，如果发现异动就登出用户（例如：关闭浏览器）。
  - `"basic"`: 默认值，会话是永久的。

- `LoginManager.login_view`: 定义浏览受限视图的重定向地址。例中 `main.login` 的 `main.` 为 `main` 蓝图中定义了登陆登陆路由，所以重定向到了此路由。

Flask-Login要求实现的用户方法，
- `is_authenticated()` --如果用户已经登录，必须返回 True ，否则返回 False
- `is_active()` --如果允许用户登录，必须返回 True ，否则返回 False 。如果要禁用账户，可以返回 False
- `is_anonymous()` --对普通用户必须返回 False
- `get_id()` --必须返回用户的唯一标识符，使用 Unicode 编码字符串

这 4 个方法可以在模型类中作为方法直接实现，不过还有一种更简单的替代方案。Flask- Login 提供了一个 UserMixin 类，其中包含这些方法的默认实现，且能满足大多数需求。 我们在定义用户 User 模型时，可以直接继承它：

`app/models.py`：

```py
from flask.ext.login import UserMixin
class User(UserMixin, db.Model):
    __tablename__ = 'users'
    ... ...
```

你可用使用

```
from flask.ext.login import current_user`
```

 代理来访问登录的用户，在每一个模板中都可以使用 `current_user`:

```html
{% if current_user.is_authenticated() %}
  Hi {{ current_user.name }}!
{% endif %}
```

需要用户登入 的视图可以用 `login_required` 装饰器来装饰:

```
@app.route("/settings")
@login_required
def settings():
    pass
```

当用户要登出时:

```py
@app.route("/logout")
@login_required
def logout():
    logout_user()
    return redirect(url_for('main.login'))
```

--------------------------------------------------------------------------------
