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