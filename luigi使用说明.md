# luigi使用
串行工作流。
## task
	class TestTask(luigi.Task):
	    //任务参数
	    _params = luigi.Parameters()
	    def require(self):
	      // 每个任务的入口
	      return LastTask()
	    def run(self):
	      // 每个任务具体执行内容
	      pass
	    def output(self):
	      // 每个任务的出口
	      pass
### 每个任务模块重载方法:
#### require方法:
	# 先运行require
	def require(self):
	    return LastTask()
	# 这里主要是说明依赖关系，当前任务需要上一个任务LastTask()的执行结果（一般是有文件生成），如若需要可以将结果作为输入
	# 可以依赖多个任务
	return [LastTask1(), LastTask2()]
	return {'x': LastTask1(), 'y': LastTask2()}
	yield LastTask1()
	yield LastTask2()
#### output方法:
	def output(self):
    return LocalTarget(path)
	# 这里主要是说明任务执行的输出结果（一般是输出文件），作为检验任务是否完成的依据
	# 返回的是一个luigi.Target对象
	# 可以有多个输出结果
	return [LocalTarget(path1), LocalTarget(path2)]
#### run方法：
	# 执行内容中需要把结果写入文件中，路径对应于output方法返回的Target的路径
	# 在执行内容前可以用Target的方法makedirs()检查输出文件夹是否存在并创建
	# 引用输出路径: self.output().path or self.output()['x'].path or self.output()[0].path
	# 引用依赖路径: self.input().path or self.input()['x'].path or self.input()[0].path
#### Parameters
	# 流程入口的Task的变量需要由Parameters对象来定义:
	class TestTask:
    	test1 = luigi.Parameters()
    	test2 = luigi.Parameters()
	# 命令行执行流程时可以直接给Parameters类的变量赋值:
	--test1 1 --test2 2


