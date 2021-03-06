# 第5课：备份策略和术语的实践

> 2020-03-16 BoobooWei

<!-- MDTOC maxdepth:6 firsth1:1 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

- [第5课：备份策略和术语的实践](#第5课：备份策略和术语的实践)   
   - [实践概述](#实践概述)   
   - [练习5-1：案例研究：制定备份策略](#练习5-1：案例研究：制定备份策略)   
      - [总览](#总览)   
      - [假设条件](#假设条件)   
      - [任务](#任务)   
   - [练习5-2：创建备份计划](#练习5-2：创建备份计划)   
      - [总览](#总览)   
      - [假设条件](#假设条件)   
      - [任务](#任务)   

<!-- /MDTOC -->

## 实践概述

在该实践中，您将为具有不同要求的不同类型的数据库开发备份策略。

## 练习5-1：案例研究：制定备份策略

### 总览

在该实践中，您将为具有不同要求的不同类型的数据库开发备份策略。

### 假设条件

提供了全套Oracle备份和恢复工具。

### 任务

1.第一种情况是在线事务处理（OLTP）数据库，每天处理大量事务。

* 业务需求是没有数据丢失，停机时间最少。
* 恢复和恢复的时间必须少于一个小时。
* 数据库是300GB。
* 几个TB的磁盘空间可用于备份。
* 所有可用磁盘都具有相同的属性（大小，I/O速率和延迟）。
* 磁带备份可用。

问题：您应采取什么步骤来保护数据库（例如，将数据库置于ARCHIVELOG模式）？

问题：您需要多少磁盘空间？

问题：什么是保留政策？

问题：您将使用快速恢复区吗？

问题：您将使用备份集还是映像副本？

问题：您将使用完整备份还是增量备份？

问题：您将使用哪种恢复方法？

2.该数据库是决策支持系统（DSS）。

* 每晚通过SQL * Loader文件从多个事务数据库中加载数据。
* DSS数据库将数据保存10年。
* 交易数据库仅保留1年的数据。
* 数据仅在事务数据库中更新，并在DSS数据库中被替换。
* 仅新的和更新的记录被传输到DSS数据库。
* DSS数据库为10 TB。
* 单独的表空间用于按年份保存数据。
* 大约有200个表空间。

问题：设计备份策略还需要了解什么？（示例：磁盘存储的成本，可用性和速度是多少？）

问题：您采取什么步骤来保护数据库？

问题：您需要多少磁盘或磁带空间？

问题：什么是保留政策？

问题：您将使用快速恢复区吗？

问题：您将使用备份集还是映像副本？

问题：您将使用完整备份还是增量备份？

问题：您将使用哪种恢复方法？


3.该数据库是恢复目录。

* 其中包含公司中20多个数据库的RMAN目录信息。
* 备份和还原操作可能随时进行。
* 数据库是关键任务。

问题：如果恢复目录不可用，如何恢复数据库？

问题：什么是保留政策？

问题：您会使用快速恢复区吗？

问题：您将使用备份集还是映像副本？

问题：您将使用完整备份还是增量备份？

问题：您将使用哪种恢复方法？

## 练习5-2：创建备份计划

### 总览

在该实践中，您将在Cloud Control中创建备份计划。您可以在“审阅”页面上查看RMAN脚本（第6步）。

### 假设条件

以前的实践已经完成。

您以SYSMAN身份登录到Cloud Control，并显示了rcat主页的菜单。

### 任务

为整个数据库（包括存档日志）安排每夜基于磁盘的增量式在线备份。完成备份后，配置存档日志以从磁盘删除。安排备份在11:00 PM执行。该时间表应无限期有效。

1.从rcat数据库主页，导航至Availability> Backup＆Recovery> Schedule Backup。

2.如果需要，选择NC_SYSDBA作为数据库凭据，然后单击登录。

3.确认或选择“ 整个数据库”作为要备份的对象，并选择“ NC_RCAT_HOST_ORACLE”作为主机凭据，然后单击“ 计划自定义备份”。

4.在“计划自定义备份：选项”页面上，确认或选择以下设置，然后单击“ 下一步”。

| 备份类型 | 增量备份                                                     |
| -------- | ------------------------------------------------------------ |
| 备份模式 | 在线备份                                                     |
| 高级     | 还备份磁盘上的所有存档日志。<br>成功备份后，从磁盘上删除所有已归档的日志。<br>删除过时的备份。 |

5.在“计划自定义备份：设置”页面上，选择“ 磁盘”作为备份位置，然后单击“ 下一步”。

6.在“计划自定义备份：计划”页面上，确认或选择以下设置，然后单击“ 下一步”。

| 职务名称   | NIGHTLY_BACKUP                                               |
| ---------- | ------------------------------------------------------------ |
| 职位描述   | 整个数据库备份                                               |
| 时间表类型 | 重覆                                                         |
| 频率类型   | 按天                                                         |
| 重复每一个 | 1（天）                                                      |
| 时区       | 您的时区或老师建议的时区。                                   |
| 开始日期   | 今天的日期                                                   |
| 开始时间   | 下午11:00 *（此时间不会影响您的正常上课时间。您的教练可能建议您改换其他时间。）* |
| 重复直到   | 不定                                                         |

7.在“计划自定义备份：查看”页面上，查看您的设置和RMAN脚本，然后单击提交作业。

8.您应该收到一条成功消息。单击查看作业。

9.在Execution：rcat页面上，您应该看到计划的作业。请注意您提供的一些特征。

10.单击导航栏中的“ 作业活动”链接。“作业活动”页面显示您的作业摘要。您可能还决定通过以下导航在以后的实践中使用它：企业>作业>活动。

导航提示：有多种方法可以导航到数据库主页，例如，如果要从RCAT导航到ORCL数据库，可以单击：目标>所有目标，然后单击 orcl 数据库实例链接或使用历史记录。

11.注销企业管理器云控制。
