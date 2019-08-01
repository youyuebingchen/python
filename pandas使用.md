#pandas
----
### 创建dataframe
- df = pd.DataFrame(np.arange(12).reshape(3,4))  行：axis = 0 列：axis = 1
- index:pd.DataFrame(np.arange(12).reshape(3,4),index=list("abc"),columns=list("xyz"))
- 可将字典放入dataframe中键为列索引，字典值用list
### dataframe的基本属性
- df.shape  行列
- df.dtypes 数据类型
- df.ndim   数据维度
- df.index  行索引
- df.columns列索引
- df.values 对象值，一个二维数组
- df.head()
- df.tail()
- df.info() 相关信息概览：行、列、列索引、列非空值个数、列类型、内存占用等
- df.describe() 快速统计结果：计数、均值、标准差、最大值、四分位数、最小值。
#### dataframe 排序
- df = df.sort_values(by="columns",ascending=False)
#### pandas 取行取列
- df[:20] 取前20  
- df[:20]["name"]：取前二十行的name
- df.loc 通过标签索引进行取行操作 df.loc["a","Z"] 取a行Z列的数据
	- .loc[["a":"c"]]取得就是a到c且包含c行
	- 取整行df.loc["a"] 
	- 取多行df.loc[["a","c"]]
	- 取多行多列 df.loc[["a","b"],["X","Y"]]  
- df.iloc 通过位置来获取数据
	- df.iloc[1,:] 取第一行
	- df.iloc[:,2]取第二列
	- df.iloc[:,[2,1]]取第2,1列
- 进行赋值
	- df.iloc[2,3]=10 对单个值进行赋值
	- df.iloc[:2,3:]=10 对一大块进行赋值
	- df.iloc[:2,3:]= np.NAN
- pandas的布尔索引，进行筛选 用()括起来
	- df[(df["count"]>80) &| (df["count"]<100)]
	- df[df["name"].str.len()>4 & df["name"].str.len()<8 ]
- pandas 字符串方法
	- cat\contains\count\endswith\startswith\findall\join\len\lower\upper\match\pad\center\repeat\slice\split\strip\lstrip\rstrip
	
	

