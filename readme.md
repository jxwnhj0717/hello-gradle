# 功能
1. 更新svn发布目录
2. 打包项目
3. 拷贝项目到发布目录
4. 提交修改的文件

# 踩过的坑

### task执行时序
+ dependsOn(taskA, taskB) taskA, taskB的执行是无序的
+ taskA.mustRunAfter(taskB) 定义时序，参考orderingTasks()

### task Delete删除报错
+ 删除lib目录时，如果lib有子目录，子目录有子文件，删除时会报错，但实际上文件删除了
+ task deployCleanTarget报错影响了后续task的执行，希望特定任务报错时忽略错误，--continue会忽略所有task的错误，直接调用task.run()，用代码的形式catch异常
+ 修改task deployCleanTarget的执行方式，直接调用它的@TaskAction方法，参考task deployCopy的调用方式

### SVN删除操作
+ 对比拷贝前后的文件，被delete的文件要加入SvnCommit.source，源码参考SvnCommit.groovy 43,57行
+ 使用project.files()不能找出目录，改用File API