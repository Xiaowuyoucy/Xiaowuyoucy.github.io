---
title: starta-cmd命令
date: 2021-05-06 18:04:05
tags:
categories: Windows命令
doc:
---

# starta-cmd命令

##  Start 

启动单独的“命令提示符”窗口来运行指定程序或命令。如果在没有参数的情况下使用，start 将打开第二个命令提示符窗口。 

语法 
`start ["title"] [/dPath] [/i] [/min] [/max] [{/separate | /shared}] [{/low |  /normal | /high | /realtime | /abovenormal | belownormal}] [/wait] [/b]  [FileName] [parameters] `

参数 
"title" 指定在“命令提示符”窗口标题栏中显示的标题。 
/dpatch 指定启动目录。 
/i 将 Cmd.exe 启动环境传送到新的“命令提示符”窗口。 
/min 启动新的最小化窗口。 
/max 启动新的最大化窗口。 
/separate 在单独的内存空间启动 16 位程序。 
/shared 在共享的内存空间启动 16 位程序。 
/low 以空闲优先级启动应用程序。 
/normal 以一般优先级启动应用程序。 
/high 以高优先级启动应用程序。 
/realtime 以实时优先级启动应用程序。 
/abovenormal 以超出常规优先级的方式启动应用程序。 
/belownormal 以低出常规优先级的方式启动应用程序。 
/wait 启动应用程序，并等待其结束。 
/b 启动应用程序时不必打开新的“命令提示符”窗口。除非应用程序启用 CTRL+C，否则将忽略 CTRL+C 操作。使用 CTRL+BREAK 中断应用程序。 
非执行文件只要将文件名作为命令键入，即可通过其文件关联运行该文件。有关使用 assoc 和 ftype 在命令脚本中创建这些关联的详细信息，请参阅“”。  

 在运行的命令的第一个标记为“CMD”字符串但不包括扩展名或路径限定符时，“CMD”将被 COMSPEC 变量的值取代。这样可以防止用户从当前目录选取 cmd。 
当您运行 32 位图形用户界面 (GUI) 应用程序时，cmd  不会在返回到命令提示符之前等待应用程序退出。如果从命令脚本运行应用程序，则不会发生这种新情况。在运行的命令中第一个符号不包括扩展名的情况下，Cmd.exe 使用 PATHEXT 环境变量的值确定要查找的扩展名以及查找顺序。PATHEXT 变量的默认值为：COM;.EXE;.BAT;.CMD（语法与 PATH 变量相同，使用分号分开不同元素）。当您搜索可执行文件且在任何扩展名上都没有匹配项时，start 将搜索目录名。 

具体例子： 

说明：如果你所在程序的路径中带有空格，那么必须用“”把路径括起来，否则系统会提示找不到XX文件，另外，在运行某些程序时，需在路径的前面加一对空白的“”，表示创建一个空白的窗口，它指向的程序是XXXXXXXX。还有就是别忘了空格。 

当我想运行位于“D:/draw/”的“photoshop.exe”使，应该使用以下命令： 
start “”“D:/draw/photoshop.exe” 表示以常规窗口运行程序 

如果想让程序以最大化窗口运行，则使用以下命令： 

start /max“”“D:/draw/photoshop.exe” 表示以最大化窗口运行程序 

最小化这是这样： 

start /min "" "D:/draw/photoshop.exe" 表示以最小化窗口运行程序 

等待某个程序允许完毕，也就是窗口关闭后，再打开下一个程序这可以这样： 

start /w "" "D:/draw/photoshop.exe" 
start "" cmd.exe                                 

## CMD

 cmd /c dir 是执行完dir命令后关闭命令窗口。

 cmd /k dir 是执行完dir命令后不关闭命令窗口。

 cmd /c start dir 会打开一个新窗口后执行dir指令，原窗口会关闭。

 cmd /k start dir 会打开一个新窗口后执行dir指令，原窗口不会关闭。

1. gpedit.msc-----组策略 
2. sndrec32-------录音机 
3. Nslookup-------IP地址侦测器 
4. explorer-------打开资源管理器 
5. logoff---------注销命令 
6. tsshutdn-------60秒倒计时关机命令 
7. lusrmgr.msc----本机用户和组 
8. services.msc---本地服务设置 
9. oobe/msoobe /a----检查XP是否激活 
10. notepad--------打开记事本 
11. cleanmgr-------垃圾整理 
12. net start messenger----开始信使服务 
13. compmgmt.msc---计算机管理 
14. net stop messenger-----停止信使服务 
15. conf-----------启动netmeeting 
16. dvdplay--------DVD播放器 
17. charmap--------启动字符映射表 
18. diskmgmt.msc---磁盘管理实用程序 
19. calc-----------启动计算器 
20. dfrg.msc-------磁盘碎片整理程序 
21. chkdsk.exe-----Chkdsk磁盘检查 
22. devmgmt.msc--- 设备管理器 
23. regsvr32 /u *.dll----停止dll文件运行 
24. drwtsn32------ 系统医生 
25. rononce -p ----15秒关机 
26. dxdiag---------检查DirectX信息 
27. regedt32-------注册表编辑器 
28. Msconfig.exe---系统配置实用程序 
29. rsop.msc-------组策略结果集 
30. mem.exe--------显示内存使用情况 
31. regedit.exe----注册表 
32. winchat--------XP自带局域网聊天 
33. progman--------程序管理器 
34. winmsd---------系统信息 
35. perfmon.msc----计算机性能监测程序 
2. 36. winver---------检查Windows版本 
37. sfc /scannow-----扫描错误并复原 
38. taskmgr-----任务管理器（2000／xp／2003 
39. winver---------检查Windows版本 
40. wmimgmt.msc----打开windows管理体系结构(WMI) 
41. wupdmgr--------windows更新程序 
42. wscript--------windows脚本宿主设置 
43. write----------写字板 
44. winmsd---------系统信息 
45. wiaacmgr-------扫描仪和照相机向导 
46. winchat--------XP自带局域网聊天 
47. mem.exe--------显示内存使用情况 
48. Msconfig.exe---系统配置实用程序 
49. mplayer2-------简易widnows media player 
50. mspaint--------画图板 
51. mstsc----------远程桌面连接 
52. mplayer2-------媒体播放机 
53. magnify--------放大镜实用程序 
54. mmc------------打开控制台 
55. mobsync--------同步命令 
56. dxdiag---------检查DirectX信息 
57. drwtsn32------ 系统医生 
58. devmgmt.msc--- 设备管理器 
59. dfrg.msc-------磁盘碎片整理程序 
60. diskmgmt.msc---磁盘管理实用程序 
61. dcomcnfg-------打开系统组件服务 
62. ddeshare-------打开DDE共享设置 
63. dvdplay--------DVD播放器 
64. net stop messenger-----停止信使服务 
65. net start messenger----开始信使服务 
66. notepad--------打开记事本 
67. nslookup-------网络管理的工具向导 
68. ntbackup-------系统备份和还原 
69. narrator-------屏幕“讲述人” 
70. ntmsmgr.msc----移动存储管理器 
71. ntmsoprq.msc---移动存储管理员操作请求 
72. netstat -an----(TC)命令检查接口 
73. syncapp--------创建一个公文包 
74. sysedit--------系统配置编辑器 
75. sigverif-------文件签名验证程序 
76. sndrec32-------录音机 
77. shrpubw--------创建共享文件夹 
78. secpol.msc-----本地安全策略 
79. syskey---------系统加密，一旦加密就不能解开，保护windows xp系统的双重密码 
80. services.msc---本地服务设置 
81. Sndvol32-------音量控制程序 
82. sfc.exe--------系统文件检查器 
83. sfc /scannow---windows文件保护 
84. tsshutdn-------60秒倒计时关机命令 
3. 84. tsshutdn-------60秒倒计时关机命令 
85. tourstart------xp简介（安装完成后出现的漫游xp程序） 
86. taskmgr--------任务管理器 
87. eventvwr-------事件查看器 
88. eudcedit-------造字程序 
89. explorer-------打开资源管理器 
90. packager-------对象包装程序 
91. perfmon.msc----计算机性能监测程序 
92. progman--------程序管理器 
93. regedit.exe----注册表 
94. rsop.msc-------组策略结果集 
95. regedt32-------注册表编辑器 
96. rononce -p ----15秒关机 
97. regsvr32 /u *.dll----停止dll文件运行 
98. regsvr32 /u zipfldr.dll------取消ZIP支持 
99. cmd.exe--------CMD命令提示符 
100. chkdsk.exe-----Chkdsk磁盘检查 
101. certmgr.msc----证书管理实用程序 
102. calc-----------启动计算器 
103. charmap--------启动字符映射表 
104. cliconfg-------SQL SERVER 客户端网络实用程序 
105. Clipbrd--------剪贴板查看器 
106. conf-----------启动netmeeting 
107. compmgmt.msc---计算机管理 
108. cleanmgr-------垃圾整理 
109. ciadv.msc------索引服务程序 
110. osk------------打开屏幕键盘 
111. odbcad32-------ODBC数据源管理器 
112. oobe/msoobe /a----检查XP是否激活 
113. lusrmgr.msc----本机用户和组 
114. logoff---------注销命令 
115. iexpress-------木马捆绑工具，系统自带 
116. Nslookup-------IP地址侦测器 
117. fsmgmt.msc-----共享文件夹管理器 
118. utilman--------辅助工具管理器 
119. gpedit.msc-----组策略
120. explorer-------打开资源管理器