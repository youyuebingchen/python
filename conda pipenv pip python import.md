# conda + pipenv + pip + python "import " 
----
## conda

##### Conda是一个包管理器，包管理器是自动化软件安装，更新，卸载的一种工具。其核心功能是包管理与环境管理。Miniconda是最小的conda安装环境， 一个干净的conda环境。conda和Anaconda没有必然关系，Anaconda才是一个python发行版。

##### pip可以允许你在任何环境中安装python包，而conda允许你在conda环境中安装任何语言包。
### 一、包管理命令
- 安装 conda
- 查看 conda 帮助
	- conda -h /--help
	- conda  comand -h  :详细信息
- 显示当前 conda 安装的所有信息
	- conda info -e / --envs
- 列出当前环境下所有安装的 conda 包
	- conda list
- 查询 conda 包，只有经过 conda 重新编译入库的才能使用 conda search 查询得到
	- conda search scrapy
- 安装 conda 包，会自动处理包之间的依赖。
	- conda install scrapy
- 安装指定版本包
	-  conda install scrapy=1.5.0
- 新 conda 包到最新版本
	- conda update scrapy
- 卸载 conda 包
	- conda remove scrapy
	- conda uninstall

### 二、环境管理命令
- 创建虚拟环境，可以在创建环境的同时安装包。由于 conda 将 python 也作为包，所以可以像其他包一样安装。
	- conda create --name tf python=3.5.2 tensorflow
- 查看环境
	- conda info -e
	- conda env list
- 激活环境 默认处于 base 环境
	-  source activate tf
- 退出激活当前环境 默认回到 base 环境
	- source deactivate
- 删除环境
	- conda remove -n tf --all
	- conda env remove -n tf
- 拷贝环境
	- conda create --clone tensorflow --name tf
- 在指定环境中管理包
	- conda install --name myenv redis
	- conda remove --name myenv redis

conda 会在每个用户家目录下创建 .conda 目录，用于管理创建的环境，而配置文件存放于 .condarc（没有可以新建）。

参考：
<br>1.<https://blog.csdn.net/chenfeidi1/article/details/80873993#_conda_3>

---
## pipenv

##### 1、不指定版本直接pip install numpy时默认安装最新版本
##### 2、即便指定版本 pip install numpy==1.0.1 numpy若依赖于其他库也会出现问题
##### 3、<确定构建>与<不记录所有模块版本>
##### 4、多个项目依赖不同版本的子模块：用虚拟环境进行隔开
##### 5、依赖分析：在requirement.txt里面添加版本范围


#####tips
pipenv 是Kenneth Reitz大神的作品，可以帮助我们管理Python和第三方库的版本。主要包含了Pipfile、pip、click、requests和virtualenv。Pipfile 文件是 TOML 格式而不是 requirements.txt 这样的纯文本。

一个项目对应一个 Pipfile，支持开发环境与正式环境区分。默认提供 default 和 development 区分。
提供版本锁支持，存为 Pipfile.lock

可以设置国内源：Pipfile文件中[source]下面url属性，比如修改成：url = "https://pypi.tuna.tsinghua.edu.cn/simple"

- 安装在某用户目录下 pip install pipenv --user [username]
	- 更新：pip install --user --upgrade pipenv

- 创建虚拟环境
	- cd project1; pipenv install会生成pipfile和pipfile.lock
	- pipenv install的时候有三种逻辑：
		- 如果目录下没有Pipfile和Pipfile.lock文件，表示创建一个新的虚拟环境；
		- 如果有，表示使用已有的Pipfile和Pipfile.lock文件中的配置创建一个虚拟环境；
		- 如果后面带诸如django这一类库名，表示为当前虚拟环境安装第三方库。
- 会使用当前系统的Python3创建环境
	- pipenv --three / --two
- 指定某一Python版本创建环境
	- pipenv --python 3.6 

- 激活虚拟环境
	- pipenv shell
- 显示目录信息
	- /home/jiahuan/pipenvtest
- 显示虚拟环境信息
	- pipenv --venv
- 显示Python解释器信息
	- pipenv --py
-安装和卸载第三方库
	- pipenv install numpy
	- pipenv uninstall numpy /--all
- 只安装在开发环境才使用的包
	- pipenv install pytest --dev
- 当前环境的模块lock住,它会更新Pipfile.lock文件
	- pipenv lock
- 生成生产环境的requirements.txt
	- pipenv lock -r > requirements.txt
- 生成开发环境的requirements-dev.txt
	- pipenv lock -r --dev > requirements-dev.txt
- 在生产环境确定构建：
	- pipenv install --ignore-pipfile
- 在另一个开发环境做开发，则将代码和Pipfile复制过去
	-  pipenv install --dev
- pipenv install
	- 会自动检测当前目录下的requirments.txt, 并生成Pipfile
	- 同 pipenv install -r requirements.txt
- 如果有开发环境的requirent-dev.txt, 可以用以下命令加入到Pipfile
	- pipenv install -r dev-requirements.txt --dev
- 查看目前安装的库及其依赖
	- pipenv graph
- 检查安全漏洞
	- pipenv check

- 退出虚拟环境
	- exit

- 管理开发环境 Pipenv 使用--dev标志区分两个环境(开发和生产)
	- 开发环境中
		- pipenv install --dev django
	- 在你的生产环境中使用下面命令安装已有的项目，该命令执行后项目中并不会安装django包
		- pipenv install

	- 如果开发人员将你的项目克隆到自己的开发环境中，他们可以使用--dev标志，将django也安装：
		- pipenv install --dev
- pipenv虚拟环境运行python命令
	- pipenv run python your_script.py

- pipenv 具有下列的选项：
		
		$ pipenv
		Usage: pipenv [OPTIONS] COMMAND [ARGS]...
		
		Options:
		  --update         更新Pipenv & pip
		  --where          显示项目文件所在路径
		  --venv           显示虚拟环境实际文件所在路径
		  --py             显示虚拟环境Python解释器所在路径
		  --envs           显示虚拟环境的选项变量
		  --rm             删除虚拟环境
		  --bare           最小化输出
		  --completion     完整输出
		  --man            显示帮助页面
		  --three / --two  使用Python 3/2创建虚拟环境（注意本机已安装的Python版本）
		  --python TEXT    指定某个Python版本作为虚拟环境的安装源
		  --site-packages  附带安装原Python解释器中的第三方库
		  --jumbotron      不知道啥玩意....
		  --version        版本信息
		  -h, --help       帮助信息
- pipenv 可使用的命令参数：
	  
		Commands:
		  check      检查安全漏洞
		  graph      显示当前依赖关系图信息
		  install    安装虚拟环境或者第三方库
		  lock       锁定并生成Pipfile.lock文件
		  open       在编辑器中查看一个库
		  run        在虚拟环境中运行命令
		  shell      进入虚拟环境
		  uninstall  卸载一个库
		  update     卸载当前所有的包，并安装它们的最新版本

- Pipfile它是 TOML 格式的，不用管子依赖包，只会把项目中实际用到的包放进去，子依赖包在pipenv install package的时候自动安装或更新。
- Pipfile.lock是JSON格式的，它包含了所有子依赖包的确定版本，它只应该由pipenv lock生成。


参考：
<br>1.<https://blog.csdn.net/wonengguwozai/article/details/80483864>
<br>2.<https://www.jianshu.com/p/d08a4aa0008e>

---
## pip
pip 是 Python 包管理工具，该工具提供了对Python 包的查找、下载、安装、卸载的功能。  

- pip --version 查看pip的版本
- sudo apt-get install python-pip 安装
- 安装特定包
	- pip install SomePackage              # 最新版本
	- pip install SomePackage==1.0.4       # 指定版本
	- pip install 'SomePackage>=1.0.4'     # 最小版本 
- 升级包
	- pip install --upgrade SomePackage 升级指定的包，通过使用==, >=, <=, >, < 来指定一个版本号。
- 卸载包
	- pip uninstall SomePackage
- 搜索包
	- pip search SomePackage
- 显示安装包信息
	- pip show 
	- pip show -f SomePackage
- 列出已安装的包
	- pip list
- 查看可升级的包
	- pip list -o
- 注意事项
	- 如果 Python2 和 Python3 同时有 pip，则使用方法如下：
	- python3 -m pip install XXX


参考：
<br>1.<https://www.runoob.com/w3cnote/python-pip-install-usage.html>
## python import
最好把导入模块放在代码的开头，解释器在执行语句时，遵循作用域原则。因为这和作用域有关系，如果在顶层导入模块，此时它的作用域是全局的；如果在函数内部导入了模块，那它的作用域只是局部的，不能被其它函数使用。如果其它函数也要用到这个模块，还需要再次导入比较麻烦。

- import语句导入模块顺序
	- python 标准库模块
	- python 第三方模块
	- 自定义模块
- 一次导入多个模块用逗号隔开
	- import sys,os
- from import语句作用
	- 也是导入模块的一种方法，更确切的说是导入指定的模块内的指定函数方法。


	
