---
layout:  post
title:  APPcrashview 查看别的目录的windows wer报告
subtitle: APPcrashview 查看别的目录的windows wer报告 
date: 2019-03-26
author: ivo
catalog: true
header-img:
tags:
    - win7
    - appcrash
---

### 0x00 简介
APPcrashview 是一个非常好用的日志查看的软件，可以比windows自带的查看器有更好的显示效果。今天简单说一下用它查看其他目录的方法。

### 0x01 使用
默认打开都是查看的默认的机器的错误日志要想查看其他目录的必须使用命令行的才可以。由于没有相应的中文的资料我这里就举个实际例子说明一下。
```
D:\AppCrashView.exe /ReportsFolder D:\WER\ReportArchive

D:\AppCrashView.exe /ReportsFolder D:\WER\ReportQueue
```
说明一下，首先要写绝对路径比较好。其次下下面列出来了中间可用的参数，实际上就第一条有用。最后路径写到路径里面

```
Command-Line Options
/ProfilesFolder <Folder>	Specifies the user profiles folder (e.g: c:users) to load. If this parameter is not specified, the profiles folder of the current operating system is used.
/ReportsFolder <Folder>	Specifies the folder that contains the WER files you wish to load.
/ShowReportQueue <0 | 1>	Specifies whether to enable the 'Show ReportQueue Files' option. 1 = enable, 0 = disable
/ShowReportArchive <0 | 1>	Specifies whether to enable the 'Show ReportArchive Files' option. 1 = enable, 0 = disable
/stext <Filename>	Save the list of application crashes into a regular text file.
/stab <Filename>	Save the list of application crashes into a tab-delimited text file.
/scomma <Filename>	Save the list of application crashes into a comma-delimited text file (csv).
/stabular <Filename>	Save the list of application crashes into a tabular text file.
/shtml <Filename>	Save the list of application crashes into HTML file (Horizontal).
/sverhtml <Filename>	Save the list of application crashes into HTML file (Vertical).
/sxml <Filename>	Save the list of application crashes into XML file.
/sort <column>	This command-line option can be used with other save options for sorting by the desired column. If you don't specify this option, the list is sorted according to the last sort that you made from the user interface. The <column> parameter can specify the column index (0 for the first column, 1 for the second column, and so on) or the name of the column, like "Event Name" and "Process File". You can specify the '~' prefix character (e.g: "~Event Time") if you want to sort in descending order. You can put multiple /sort in the command-line if you want to sort by multiple columns.Examples:
AppCrashView.exe /shtml "f: empcrashlist.html" /sort 2 /sort ~1
AppCrashView.exe /shtml "f: empcrashlist.html" /sort "Process File"
/nosort	When you specify this command-line option, the list will be saved without any sorting.
```



本内容同步更新在我的个人博客 「我们都是害虫」 [https://ezlost.com](https://ezlost.com)  搜索查询关注订阅评论 更加方便快捷.内容转载请注明来源。
