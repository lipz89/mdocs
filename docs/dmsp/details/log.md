物资云系统的日志输出统一使用log4net模块实现
物资云系统通过日志总线对log4net进行封装，通过配置工具对日志进行配置，实现功能：
1. 可区分模块输出日志
2. 自动创建日志文件
3. 日志文件大小和数量均可限制，超出数量限制会滚动删除最早的日志文件

#### 配置
日志配置界面如下：
![配置界面](/images/dmsp/log.png)
日志路径：不可修改，固定为web项目下Logs目录，所有的日志文件均输出在此文件夹内；
日志级别：标识输出日志的级别，包含以下级别：

>全部：输出所有日志
调试：输出所有级别不小于debug的日志
信息：输出所有级别不小于info的日志
警告：输出所有级别不小于warn的日志
错误：输出所有级别不小于error的日志
无：不输出任何日志

模块日志数量上限：模块由业务系统定义，每个模块的日志文件数量单独计算；
文件大小上限：单个日志文件的大小上限，单位兆(MB)

#### 日志文件管理

1. 首先日志总线按模块区分文件夹，在单个模块的文件夹中由log4net管理组织日志文件；
2. 按照当前日期命名文件，文件不存在则自动创建，并将日志内容追加到该文件，如：log.2019-09-26.log；
3. 如果文件大小超出限制，将会创建新的log.2019-09-26.log文件，原有的文件则重命名，本例为log.2019-09-26.1.log；文件管理组织方式为滚动日志，如下以模块日志数量上限设置为10为例：
    |源文件名|操作|新文件名|
    |:--|:--:|:--|
    |log.2019-09-26.9.log|删除||
    |log.2019-09-26.8.log|重命名为|log.2019-09-26.9.log|
    |...|...|...|
    |log.2019-09-26.2.log|重命名为|log.2019-09-26.3.log|
    |log.2019-09-26.1.log|重命名为|log.2019-09-26.2.log|
    |log.2019-09-26.log|重命名为|log.2019-09-26.1.log|
    ||创建新的|log.2019-09-26.log|
    
    同一日期的日志文件中，无序号后缀的文件始终是最新的，序号越大，文件越早。
4. 如果同一模块的日志文件数量超出上限，log4net会自动删除同一模块中最早的日志文件