[TOC]

软件框架：一个软件框架是由多个软件模块组成的，每个模块都有特定的功能，模块和模块之间相互配合来完成软件的开发。


看官方文档库

<br>学习一个框架就是学习模块的功能以及怎么使用。
<br>MVC框架产生理念：分工。核心思想：解耦
<br>M: model 模型，和数据库进行交互
<br>V: View 视图，产生html页面
<br>C: Controller,控制器，接收请求，进行处理与M和V进行交互返回应答

Django 是用python语言写的开源web开发框架，并遵循MVC设计。
<br> 原则：快速开发；DRY don't repeat yourself
<br>Django MVT :python MVC M:数据保存到数据库   V:相当于C接收数据进行处理和M T进行交互  T:templacte 产生html页面


### 虚拟环境
pip install 会装在 usr/local/lib/python3.5/dist-pachages 安装同一个包的不同版本时，安装的包会把原来安装的包覆盖掉。
#flask
web开发的任务

- 视图开发：根据客户请求实现业务逻辑（视图）编写
- 模板、数据库等其他都是为帮助视图开发、不是必备的。

核心：实现路由和视图
### 框架的轻重
- 重量级框架：为方便业务程序的开发提供了丰富的工具组件如Django
- 轻量级框架：只提供web框架的核心功能，自由、灵活、高度定制：Flask、Tornado


## Flask 框架

### 什么是flask
Flask是一个用Python编写的Web应用程序框架,诞生于2010年，是Armin ronacher用python基于Werkzeug工具编写的轻量级框架。Flask框架的核心就是Werkzeug 和jinjia2，最灵活的框架之一

### flask 创建app对象

#### 初始化参数

- __name__指明根目录
- static_url_path
	- app = Flask(__name__,static_url_path="/python")# 访问静态资源的url前缀，默认值是static,指明是静态资源，和访问视图的资源分开。
- static_folder 静态文件的目录，默认就是static
	- app = Flask(__name__,static_folder="static")
- template_folder 静态文件的目录，默认就是templates
	- app = Flask(__name__,template_folder_folder="templates")

#### 配置参数

- 在根目录下创建config.cfg，使用配置文件
	- DEBUG = True
	
			app.config.from_pyfile("config.cfg")

- 创建类对象

			class config():
				DEBUG = True
				ITCAST = "python"
			app.config.from_objects(config)
	- 从config类中取值
		- 直接从全局对象app中config对象取值  app.config["ITCAST"]  app.config.get("ITCAST")
		- from flask import current_app 也就是app的代理
		- current_app.config["ITCAST"] 
- 直接操作app里面的config

			app.config["DEBUG"]=True

#### 启动

- 设置启动地址和端口，也可设置debug参数
	- app.run(host="192.168.17.70",port=5000,debug=True)
			
#### 路由

现代Web框架使用路由技术来帮助用户记住应用程序URL。可以直接访问所需的页面，而无需从主页导航。
Flask中的route()装饰器用于将URL绑定到函数。

- 查看当前app对象中都构造了哪些路由以及其对应的视图函数都有哪些
	- app.url_map
		- 这些视图函数都是以哪些请求方式来进行的访问
- 更换访问方式
	- @app.route("/post_only",methods=["GET","POST"])
- 对于不同视图绑定到同一个路径下时，访问时先访问的是第一个视图
	- 只有请求路径和方式都一样时才会第一个给覆盖
- 一个视图函数使用两个路径

		@app.route("/hi1")
		@app.route("/hi2")
		def hi():
			return "hello world"

#### 反解析

- from flask import redirect
	
- 使用 redirect

		@app.route(“login”)
		def login():
			url = "/"
			return redirect(url) 
- 使用url_for反解析 通常使用视图函数进行反推

		@app.route(“login”)
		def login():
			url = url_for("index") # 视图函数名
			return redirect(url) 
#### 转换器
- Flask 变量规则

通过向规则参数添加变量部分，可以动态构建URL。此变量部分标记为<variable-name>。它作为关键字参数传递给与规则相关联的函数。

- 按照正则进行匹配。可传入 int,float,path,str如果不加转换器类型的话默认是普通的字符串规则(除了斜线的任意字符)

		@app.route("/goods/<int:goods_id>")
		def good_detal(goods_id):
		    """定义视图函数"""
		    return "goods detail page %s" %goods_id
		# 进行传参：http://127.0.0.1:5000/goods/123

- 对于其他类型的要自己定义： 定义自己的转换器
		
		# 定义自己的转换器	
		from werkzeug.routing import BaseConverter
		class RegexConverter(BaseConverter):
			def __init__(self,url_map,regex):
				#调用父类的初始化方法
				super()
				# 将正则表达式的参数保存到对象属性中，flask会使用这个属性来进行路由的匹配
				self.regex = regex
			
		
		# 将自定义的转换器添加到flask的应用中,以键值对的方式进行保存
		app.url_map.converters["re"] = RegexConverter

		# 自定义转换器的使用
		@app.route("/send/<re(r'1[34578]\d{9}'):mobile>")
		def send_sms(mobile):
			return "send sms to %s
- to_python 方法 返回解析出来的值
- to_url 方法 将python中的数据转到url中取，url_for方法会用到，先调用to_url再返回。


