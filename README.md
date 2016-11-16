# QST_Hadoop_Project

## 项目背景

某电商平台XD，每天有大量的用户访问，XD的产品经理ZB，想了解现在网站的访问情况，于是提出了一个需求3 

1. 需要看到网站的访问情况
    1. 包括PV、UV
2. 需要符合大数据的概念
3. 必须支持后的开发需求，要尽量高的开发效率

## 项目开展

本次项目的负责人是：马子越

项目分4个阶段开展

## Round 1

本回合完成任务主要完成：

1. 完成设计方案
    1. 方案内容填写在./Round1/README.md 文件里
    2. 方案包含所有操作的内容，由环境搭建，到最后的代码运行
2. 完成环境搭建
    1. 建议进行操作一遍
3. 完成以下统计需求
    1. 完成每天的UV统计
    2. 完成每天访问量Top10的Show统计
    3. 完成每天的次日留存统计

相关资料完成后，请发起pull request，里面标题填上自己的名字。

方案：
1、搭建hadoop环境：使用1台master机器和4台slave机器组成的集群，全部安装ubuntu操作系统
首先在指定的master机器
（1）安装java1.7环境：
 *下载java软件并安装
 *配置环境变量
（2）安装hadoop环境：
 *下载hadoop2.6.5解压到本地目录
 *配置hadoop环境
（3）复制master机器的文件到4台slave机器上
（4）建立master机器和4台slave机器的互信关系
（5）验证集群：
 *格式化namenode
 *启动hdfs，jps查看进程
22230 Master
30889 Jps
22478 Worker
30498 NameNode
30733 SecondaryNameNode
19781 ResourceManager
 *停止hdfs，jps查看
30336 Jps
22230 Master
22478 Worker
19781 ResourceManager
 *启动yarn，同样的jps
31233 ResourceManager
22230 Master
22478 Worker
30498 NameNode
30733 SecondaryNameNode
31503 Jps
 *停止yarn，jps
31167 Jps
22230 Master
22478 Worker
30498 NameNode
30733 SecondaryNameNode
*查看集群状态  ./bin/hdfs dfsadmin -report

2、集群测试成功之后，把要处理文件，存到hdfs下面，通过写java程序处理上面的数据，完成客户要求
（1）完成每天的UV统计
 思路：1.通过查看数据的格式和客户要求，处理数据。因为是每天的uv统计，可以近似的以数据去重之后的ip表示
2.根据数据的格式，选择用正则表达式或者是字符串的分割，来获得每个用户的ip
3.因为ip要去重，考虑把每一个不同的ip存储到 HashSet 里面，自动去重，那么容器的最终大小就近似的表示uv的统计
4.如果用mapreduce处理数据，map中 key设一个统一的 x，value 是ip，reduce中处理去重的ip，获得数量，输出，从而满足需求
（2）完成每天访问量Top10的Show统计
思路：1.完成需求需要统计每一个ip 和每个 ip每天的访问数量，通过比较得到Top10.
2.考虑使用实现map 和reduce的方式解决问题，每一个相同的ip作为key，数量当做是value，reduce中自动排序，输出前十，从而满足需求
（3）完成每天的次日存留统计
思路：1.通过mapReduce得到前一天的访文不相同的ip
2.同上获得次日的访文所以不相同的ip
3.得到的数据再做一个mapreduce ，类似于做wordcount的方法，统计每个ip的出现次数。
4.ip出现次数为2的就是留下的人




