# tips
### 查看版本
1.查看python的版本
-  python -V
-  python --version

2.查看python的安装位置
- python -c "import sys;print(sys.excutable)"
- python -c "import os; print os.sys.executable"

3.查看numpy版本
- numpy.__version__
- numpy.__file__
### 安装特定版本的库
- pip install luigi==2.1.0
### 读取信息
1.字符串前加 u
- 后面字符串以 Unicode 格式进行编码，一般用在中文字符串前面，防止因为源码储存格式问题，导致再次使用时出现乱码。

2.字符串前加 r
- 去掉反斜杠的转移机制。

3.字符串前加 b
- b" "前缀表示：后面字符串是bytes 类型。

4.字符串前加 f
- 以f开头表示在字符串内支持大括号内的python 表达式

### 两个变量同时迭代

  li1 = [1,2]
  li2 = [3,4]
  mix =list(zip(li1,l2))
  for i,j in mix:
    print(i,j,end="\n")
