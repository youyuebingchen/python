## decorators
### 在函数中定义函数
在函数a中定义函数b,函数b不可以在函数a外被调用。
### 在函数中返回函数
函数中也能返回函数，返回函数名后不带()表示该函数可以被传递，后面带小括号()表示该函数被执行。
### 装饰器定义
一种增加函数功能的简单方法，可以快速地给不同的函数或类插入相同的功能。其本质是callable，包括两种，一种是函数，一种是支持__call__()的对象。
### 装饰器的作用（帮助函数）
在不修改函数本身而给他新的功能，也就是对已经存在的某些类进行装饰，以扩展一些功能。装饰器本质上是一个 Python 函数或类，它可以让其他函数或类在不需要做任何代码修改的前提下增加额外功能，装饰器的返回值也是一个函数或类对象。

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
### 含参数装饰器
相比于普通装饰器，含参数装饰器又多了一层，可通过参数控制装饰器的效果。
- 将decorator返回


        from functools import wraps
        def logit(logfile='out.log'):
            def logging_decorator(func):
                @wraps(func)
                def wrapped_function(*args, **kwargs):
                    log_string = func.__name__ + " was called"
                    print(log_string)
                    # 打开logfile，并写入内容
                    with open(logfile, 'a') as opened_file:
                        # 现在将日志打到指定的logfile
                        opened_file.write(log_string + '\n')
                    return func(*args, **kwargs)
                return wrapped_function
            return logging_decorator

        @logit()
        def myfunc1():
            pass
        # 通过logfile参数来控制其输出文件名。
        @logit(logfile='func2.log')
        def myfunc2():
            pass
 
 - 数据检查，通过参数mustbe_cols，missing_rate，dtype控制装饰器检查哪些量。
 
        def check_output(mustbe_cols=[], missing_rate={}, dtype={}):
            def decorator(func, ):
                " logging decorator"
                @wraps(func)
                def wrapper(*args, **kwargs):
                    " wrapper "
                    try:
                        return_val = func(*args, **kwargs)
                        assert isinstance(return_val, pd.DataFrame)
                        if mustbe_cols:
                            try:
                                assert set(mustbe_cols).issubset(set(return_val.columns))
                            except Exception as exp:
                                need = set(mustbe_cols) - set(return_val.columns)
                                raise Exception(f"{func.__name__}'s' output need: {need}")
                        if missing_rate:
                            missing_dict = (return_val[return_val.keys()].isna().sum() / len(return_val)).to_dict()
                            for key in missing_rate:
                                assert missing_rate[key] > missing_dict[key]
                        if dtype:
                            dtype_dict = return_val.dtypes.to_dict()
                            for key in dtype:
                                assert dtype[key] == dtype_dict[key]
                    except Exception as exp:
                        logger.exception(exp)
                        raise exp
                    return return_val
                return wrapper
            return decorator

参考<https://www.runoob.com/w3cnote/python-func-decorators.html>
