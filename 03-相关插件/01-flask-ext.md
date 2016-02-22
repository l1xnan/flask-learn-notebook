
```
# pip install -r requirement.text

flask-sqlalchemy
flask-migrate
flask-uploads
flask-wtf
flask-bootstrap

sqlalchemy
sqlalchemy-migrate
werkzeug
wtforms
mongokit
```

[Flask 中文教程](http://docs.jinkan.org/docs/flask/index.html) [探索 Flask](http://www.pythondoc.com/exploreflask/) [flask-migrate](http://flask-migrate.readthedocs.org/en/latest/) [flask-sqlalchemy 中文教程](http://www.pythondoc.com/flask-sqlalchemy/index.html)

# Pony ORM 系列
## flask-ponywhoosh
1. [Pony 中文教程](http://ponyorm-cn.readthedocs.org/zh_CN/draft/0-What-is-Pony-ORM/)
2. [Pony 官网](https://docs.ponyorm.com/)

## 开始
安装 Pony:

```
pip install pony
```

导入：

```
from poy.orm import *
```

创建数据库：

```
db = Database('sqlite', ':memory:')
# OR
db = Database('sqlite', 'test_db.sqlite', create_db=True)
```

# salalchemy 系列

```py
from flask import Flask
from flask.ext.sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:////tmp/test.db'
db = SQLAlchemy(app)


class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(80), unique=True)
    email = db.Column(db.String(120), unique=True)

    def __init__(self, username, email):
        self.username = username
        self.email = email

    def __repr__(self):
        return '<User %r>' % self.username
```

为了创建初始数据库，只需要从交互式 Python shell 中导入 `db` 对象并且调用 `SQLAlchemy.create_all()` 方法来创建表和数据库:

```py
from yourapp import db
db.create_all()
```

现在来创建一些用户:

```
from yourapplication import User
admin = User('admin', 'admin@example.com')
guest = User('guest', 'guest@example.com')
```

但是它们还没有真正地写入到数据库中，因此让我们来确保它们已经写入到数据库中:

```py
db.session.add(admin)
db.session.add(guest)
db.session.commit()
```

数据库更新：

```py
rows_changed = User.query.filter_by(role='admin').update(dict(permission='add_user'))
db.session.commit()
```

## flask-migrate
Flask-Migrate 是在 Flask 中用来处理SQLAlchem​​y的数据库迁移的扩展。我们用 Flask-Script 的命令行来操作数据库。

安装：

```
pip install Flask-Migrate
```

例如，有文件 `app.py`：

```py
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
from flask_script import Manager
from flask_migrate import Migrate, MigrateCommand

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///app.db'

db = SQLAlchemy(app)
migrate = Migrate(app, db)

manager = Manager(app)
manager.add_command('db', MigrateCommand)

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(128))

if __name__ == '__main__':
    manager.run()
```

有上面的应用程序，我们就可以创建和迁移数据库了。如果数据库已经存在，我们用下面的命令：

```
python app.py db init
```

这将添加一个迁移文件夹 `migrations` 到您的应用程序。该文件夹的内容需要与其他源文件一起被加入到版本控制。 然后, 您可以生成一个初始迁移:

```
python app.py db migrate
```

然后就可以迁移数据库了：

```
python app.py db upgrate
```

# 样式
[Flask-Bootstrap](http://www.pythonhosted.org/Flask-Bootstrap/index.html)
