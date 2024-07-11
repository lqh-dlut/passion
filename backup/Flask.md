###视图函数
```python
from markupsafe import escape

@app.route('/user/<name>')
def user_page(name):
    return f'User: {escape(name)}'
```

> 注意 用户输入的数据会包含恶意代码，所以不能直接作为响应返回，需要使用 MarkupSafe（Flask 的依赖之一）提供的 escape() 函数对 name 变量进行转义处理，比如把 < 转换成 &lt;。这样在返回响应时浏览器就不会把它们当做代码执行。

视图函数作用：

1. 表达含义
2. 作为代表某个路由的**端点endpoint**

包含变量和运算逻辑的 HTML 或其他格式的文本叫做模板，执行这些变量替换和逻辑计算工作的过程被称为渲染

```python
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False  # 关闭对模型修改的监控
```
### FLASK数据库
[Flask-SQLAlchemy](https://read.helloflask.com/database/#_4)

> **何时应该关闭？**
> 
> - 当你的应用不使用 Flask-SQLAlchemy 的事件系统，或者不需要监听对象的修改时，你应该将其设置为 False。
> 
> **何时应该开启？**
> 
> - 如果你的应用需要监听对象的修改事件，例如在对象被修改后执行某些操作（如日志记录、触发其他业务逻辑等），或者使用了与模型修改相关的自定义事件处理器，那么你可能需要将 SQLALCHEMY_TRACK_MODIFICATIONS 设置为 True。

**创建数据库表**
```python
(env) $ flask shell
>>> from app import db
>>> db.create_all()

#app.py
class Movie(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(20))
    year = db.Column(db.String(4))
```
> 上面打开 Python Shell 使用的是 flask shell命令，而不是 python。使用这个命令启动的 Python Shell 激活了“程序上下文”，它包含一些特殊变量，这对于某些操作是必须的（比如上面的 db.create_all()调用）。请记住，后续的 Python Shell 都会使用这个命令打开。

**自定义命令initdb**

```python
@click.option('--drop', is_flag=True, help='Create after drop.')  # 设置选项
@app.cli.command()  # 注册为命令，可以传入 name 参数来自定义命令
def initdb(drop):
...
```
> 通常建议将 Click 的装饰器（如 @click.option() 和 @click.argument()）放在前面，这样可以确保 Click 正确解析所有命令行参数之后，再由 Flask 的装饰器（如 @app.cli.command()）处理这些参数和函数的注册。这样做的好处是 Click 可以正确地处理 --help 和其他命令行参数，而不会受到 Flask 装饰器可能带来的干扰。
initdb作用：在env环境下使用调用函数进行数据库表的构造
```python
#构造
flask initdb
#删除后重新构造
flask initdb --drop
```
**数据库的增删改查**

> 导入模型类 `from app import User, Movie`
提交数据库会话 `db.session.commit()`，只有commit才是真正提交到数据库中

1. 新增数据
```python
>>> user = User(name='Grey Li')  # 创建一个 User 记录
>>> m1 = Movie(title='Leon', year='1994')  # 创建一个 Movie 记录
>>> m2 = Movie(title='Mahjong', year='1996')  # 再创建一个 Movie 记录
>>> db.session.add(user)  # 把新创建的记录添加到数据库会话
>>> db.session.add(m1)
>>> db.session.add(m2)
```
2. 删除数据
```python
>>> movie = Movie.query.get(1)
>>> db.session.delete(movie)  # 使用 db.session.delete() 方法删除记录，传入模型实例
>>> db.session.commit()  # 提交改动
```
3. 查找数据
```python

filter() 过滤
filter_by() 过滤(相等属性)
order_by() 排序
group_by() 分组

```
`User.query.filter_by(username='john_doe').first()`
这行代码相当于：
`User.query.filter(User.username == 'john_doe').first()`
> filter() 方法接受**一个或多个条件表达式**作为参数，每个表达式都是一个Python表达式，使用等于 (==)、不等于 (!=)、大于 (>)、小于 (<) 等。
filter_by() 方法接受关键字参数，**仅限于属性相等性**比较，不支持多个条件的组合，也不支持不等于（!=）、大于（>）、小于（<）等比较运算。
filter_by()大概是filter()的**真子集**。

<img src="https://github.com/lqh-dlut/passion/assets/63030247/a8524c6d-8fca-450c-acf6-43ca337385da"  width="600" height="300">

4. 改变数据
```python
>>> movie = Movie.query.get(2)
>>> movie.title = 'WALL-E'  # 直接对实例属性赋予新的值即可
>>> movie.year = '2008'
>>> db.session.commit()  # 注意仍然需要调用这一行来提交改动
```

### app.errorhandler() 装饰器
作用与视图函数类似，在发生HTTP错误时被调用
```python
@app.errorhandler(Exception)
def handle_exception(e):
    # 注意这里捕获的是所有异常
    return render_template('error.html', error=str(e))

### 模板上下文处理函数
```python
@app.context_processor
def inject_user():  # 函数名可以随意修改
    user = User.query.first()
    return dict(user=user)  # 需要返回字典，等同于 return {'user': user}
```
> 此时render_template()函数中不需要传入`user=user`

> Jinja2 模板引擎使用以下几种界定符：
{{ }} 用于变量插值，即在模板中插入变量的值。
{% %} 用于控制结构和其他模板标签。
#{# #} 用于注释，这些注释不会出现在生成的HTML中。

`movie = Movie.query.get_or_404(movie_id)`方法，它会返回对应主键的记录，如果没有找到，则返回 404 错误响应。

###werkzeug.security安全存储密码
```python
from werkzeug.security import generate_password_hash, check_password_hash

generate_password_hash(pw) #生成密码哈希值
check_password_hash(pw_hash, pw) #检查哈希值是否对应密码
`

