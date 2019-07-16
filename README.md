# python
python_study
## decorators
### 在函数中定义函数
在函数中a定义函数b,b函数不可以在a函数外被调用
### 在函数中返回函数
函数中也能返回函数，返回函数名后不带()表示该函数可以被传递，后面带小括号()表示该函数被执行。
### 将函数作为参数传给另一个函数
可以将一个函数作为参数传递给另一个函,装饰器的过程如下

    def a_decorator(a_func)
    def a_func_requiring_decoration()
    a_func_requiring_decoration = a_decorator(a_func_requiring_decorator)
    
  我们用@a_decorator来简写a_func_requiring_decoration = a_decorator(a_func_requiring_decorator)
  也就是我们将需要装饰的函数作为参数放进装饰器里，被装饰器所装饰。但是这样会改变a_func_requiring_decoration() 函数的名字和注释文档(docstring)
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
