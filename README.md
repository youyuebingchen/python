# python
python_study
## decorators
### 在函数中定义函数
在函数中a定义函数b,b函数不可以在a函数外被调用。
### 在函数中返回函数
函数中也能返回函数，返回函数名后不带()表示该函数可以被传递，后面带小括号()表示该函数被执行。
### 装饰器定义
一种增加函数功能的简单方法，可以快速地给不同的函数或类插入相同的功能。其本质是函数，为其他函数进行装饰。
### 装饰器的作用（帮助函数）
在不修改函数本身而给他新的功能，也就是对已经存在的某些类进行装饰，以扩展一些功能。装饰器本质上是一个 Python 函数或类，它可以让其他函数或类在不需要做任何代码修改的前提下增加额外功能，装饰器的返回值也是一个函数或类对象

    - 打log日志
    - 检查
    - 数据缓存
    - 授权
    - 性能测试
### 将函数作为参数传给另一个函数
可以将一个函数作为参数传递给另一个函,装饰器的过程如下

    def a_decorator(a_func)
    def a_func_requiring_decoration()
    a_func_requiring_decoration = a_decorator(a_func_requiring_decorator)
    
  我们用
  <br>@a_decorator来简写a_func_requiring_decoration = a_decorator(a_func_requiring_decorator)
  <br>也就是我们将需要装饰的函数作为参数放进装饰器里，被装饰器所装饰。但是这样会改变a_func_requiring_decoration() 函数的名字和注释文档(docstring)
  我们用function.wraps来修改该内容。
  <br>@wraps接受一个函数来进行装饰，并加入了复制函数名称、注释文档、参数列表等等的功能。这可以让我们在装饰器里面访问在装饰之前的函数的属性。
  
  蓝本规范
  
    from functools import wraps
    def decorator_name(f):
        @wraps(f)
        def decorated(*args, **kwargs):
            if not can_run:
                return "Function will not run"
            return f(*args, **kwargs)
        return decorated

    @decorator_name
    def func():
        return("Function is running")
### 装饰器的顺序
一个函数还可以同时定义多个装饰器，它的执行顺序是从里到外，最先调用最里层的装饰器，最后调用最外层的装饰器
### 装饰器的原则
    - 不能修改被装饰的函数的源代码
    - 不能修改被装饰的函数的调用方式
