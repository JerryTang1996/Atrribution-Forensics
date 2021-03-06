## 报告初稿（PPt)使用

### 1.简介



### 2.行动简介
+ （1）HangOver
这次攻击主要是针对电信巨头Telenor的一次非法入侵，主要的攻击方式是鱼叉式网络钓鱼。
第一次的攻击的网络钓鱼文件包括两个附件文件，一个doc文档‘220113.doc’和一个可执行文件‘few important operational documents.doc.exe’，可执行文件是一个包含两个文件的自动提取ZIP压缩包，两个文件分别是：‘conhosts.exe’和诱饵文件‘legal operations.doc’。 ‘legal operations.doc’和‘220113.doc’除了大小外都是相同的，实际上是特质的RTF文件，旨在触发Microsoft Common Controls中的软件漏洞（CVE-2012-0158），通常在Microsoft Word中触发。如果触发漏洞，将运行文档中的恶意嵌入代码。
第二次攻击，攻击者创建了另外一个攻击包，并将它放在一个网址上，这个包和第一个包非常相似，只是这次的诱饵文件是powerpoint演示文稿。
+ （2）Snake In The Grass

这次的情报收集活动主要针对的是巴基斯坦的军事领域，时间是2013年12月底至2014年初。
这主要是本次攻击行动中的恶意代码大部分是用Python编写的脚本，然后使用PyInstaller 和 Py2Exe两种方式进行打包。相关恶意代码发生了较大的变化，其中出现了由 Python、AutoIt、Go 语言等开发的恶意代码。
本次攻击主要是用了鱼叉邮件的方式，邮件里面包含诱饵文档和恶意的python安装程序，诱饵文档多伪装为印度军方的相关机密文档，伪装成“pps、doc、pub、pdf、jpg”等类型的文件诱导用户点击，样本程序大多是自解压文件点击后在打开正常文件的同时可以释放并运行恶意程序。恶意的文件主要用 python 编写的脚本然后使用 PyInstaller 和 Py2Exe 两种方式进行打包。
恶意代码执行的步骤分为一下三步：

1. 调用系统工具检测虚拟机，上传指定目录下的文件到服务器，从服务器下载文件并执行。

2. 修改注册表添加启动项

3. 偷窃格式为““doc, xls, ppt, pps, inp, pdf, xlsx, docx, pptx”的文件。

   除此之外，记录键盘的操作到日记文件中。

本次攻击行动中可以从C&C服务器等网络行为联系到Hangover的攻击行动。
+ （3）Patchwork
这次攻击主要针对的是东南亚和中国南海，攻击自2015年12月被检测出到2016年7月报告发布，已感染了约2500台电脑，据报告显示活动最早可追溯到2014年。
Patchwork攻击主要针对军事和政治机构，相对其它APT攻击而言，该APT攻击所使用的全部攻击代码都来自于互联网上的开源代码，成本低，但攻击效果强。
通过Cymmetria MazeRunner以蜜罐的方式捕获了攻击者的活动，推测出其攻击流程如下：
首先，向目标发送以pps为附件的网络钓鱼邮件，当用户点击附件时，pps文件播放，触发漏洞，该漏洞为CVE-2014-4114,存在于未打补丁的Office PowerPoint 2013和2017中。漏洞执行，开始攻击，攻击可分为以下两个阶段：
第一阶段，利用编译的Autolt脚本，进行提权操作，与C&C控制器建立连接，扫描文件并上传。提权操作采用UACME方法，获得更高权限后，运行PowerSploit，通过MeterPreter建立远程连接，收集并上传文件。这一阶段的有效载荷是sysvolinfo.exe。
第二阶段，在确定目标价值后，释放恶意软件7zip.exe，执行磁盘文件的扫描、筛选与上传。同时，为了保证长期的控制，7zip.exe复制自身在C:\Windows\SysWOW64\目录下生成netvmon.exe文件，并以自启动服务Net Monitor 运行。
![image](https://github.com/Bessroy/picture/blob/master/patchwork/2018-10-18_001412.png)

图1 Patchwork攻击流程
图2展示了与C&C连接的分析结果

![image](https://github.com/Bessroy/picture/blob/master/patchwork/2018-10-18_001641.png)

图2 Patchwork溯源结果

monsoon（2016——Forcepoint）
此次攻击开始于2015年12月，再2016年5月开始被检测出，到2019年7月仍然在进行攻击。Monsoon攻击被与Operation Hangover联系到一起，因为有相同的基础设施和攻击目标。
此次攻击主要利用了钓鱼邮件投放恶意文档，包含的恶意软件有Unknown Logger，TINYTYPHON, BADNEWS和AutoIt的一个后门。通过对样本分析，其攻击流程为
![image](https://github.com/JerryTang1996/nothing/blob/master/monsoon.png)
其中利用的主要恶意软件被命名为BADNEWS，BADNEWS恶意软件是第一阶段恶意软件，如果目标感兴趣的话，很可能接收第二阶段恶意软件组件，但没有观察到这种行为。通过dll-sideloading 引出三个文件MicroScMgmt.exe ，msvcr71.dll ，jli.dll
用合法的MICSCMGMT.EXE（Sun Microsystems签名）运行时加载恶意jli.dll，BADNEWS此时开始执行恶意代码，这样可以绕过恶意软件检测。恶意软件将产生2个线程，一个执行关键日志记录，一个扫描本地硬盘驱动器。
BADNEWS同时安装一个注册表项来保持其持续性，有几个硬编码（给变量附固定值）的C＆C通道，C&C地址在blog中通过搜索’{{‘来获取，是一段加密的C&C地址。

+ （4）Indian Subcontinent Attack (2018.7)
    这次攻击主要针对巴基斯坦的军事和核能领域。Patchwork团队利用CVE-2015-2545、CVE-2017-0261漏洞，以涉及巴基斯坦军事升级、原子能委员会等主题的恶意文档作为诱饵，传播BADNEWS有效载荷。
    BADNEWS恶意软件充当攻击者的后门，为他们提供对受害者机器的完全控制，其利用合法的第三方网站来托管恶意软件的命令和控制信息，如下载和执行附加信息、上传感兴趣的文档以及获取桌面截图等。收集信息后，BADNEWS利用HTTP与远程服务器进行通信。

+ （5）对美国智库的攻击（2018年3月~4月）

    此次攻击中最值得注意的地方是Patchwork调整了攻击的目标，直接对美国智库发起攻击。此外，在这次攻击中，Patchwork除了发送恶意软件诱饵，该组织还在邮件中通过一个特定于每一个接收者的唯一跟踪链接，来识别哪一个接收到该邮件的人点开了恶意信息。

    **钓鱼邮件的发送**：钓鱼邮件都发送自攻击者控制的域名：mailcenter.support. 这个域名不仅用于发送钓鱼邮件，还用于跟踪打开了电子邮件的目标。攻击者通过在邮件中模仿美国知名的智库的域名或邮件主题来诱使收件人点开链接。在邮件的HTML格式的消息中，一个嵌入的图片tag包含了攻击者的域名和一个用于跟踪收件人的32字节标识符。

    **漏洞利用和恶意软件执行**：当被攻击目标点开包含恶意文档的链接，攻击者会利用“packager trick”来把初始的QuasarRAT dropper（qrat.exe）嵌入到RTF文档中（QuasarRAT是一个用C#编写的远程管理/访问工具，包含文件管理、上传、下载和执行、键盘记录、远程控制和反向代理等功能）。然后攻击者会利用CVE-2017-8570漏洞执行嵌入到RTF文档中的恶意脚本（”scriptlet“或.sct文件）。这个脚本启动qrat.exe，这个程序在`C:\Users\%username%\AppData\Roaming\Microsoft Network\microsoft_network\1.0.0.0`中创建一个目录，并且把最终用于攻击的二进制文件microsoft_network.exe放入其中。恶意软件还包含了一个名为Microsoft.Win32.TaskScheduler.dll的文件，用于操作系统上的任务调度。这个文件由AirVPN的数字证书进行签名。

    在恶意软件执行的时候QuasarRAT的调度任务被命名为Microsoft_Security_Task，每天凌晨12点运行。一旦进程被触发，它每五分钟运行一次，重复60天。执行时，microsoft_network.exe将向freegeoip.net发起请求，以确定受感染主机的地理位置。 在请求之后，恶意软件将立即与在攻击者控制下的域名 tautiaos.com（43.249.37.199）建立加密连接。

    整个样本执行流程如下：

    ![](https://i.imgur.com/iiwWHyY.jpg)

    **总结**：这次对美国智库的攻击体现了Patchwork的攻击目标在地理位置上的多样性。攻击者在钓鱼邮件中除了包含恶意软件之外，还是用特定于攻击者的唯一链接来跟踪目标是否被成功攻击，可以使之后的攻击更高效地进行。此外攻击者在这次攻击中使用了开源的RAT，使其可以灵活地与被攻击机器进行交互而无需使用自定义的恶意软件。


### 3.溯源的技术与流程
#### 3.1攻击主机的溯源取证
+ Patchwork：通过MazeRunner，研究人员可以及时知道网络动态，当攻击者发现网络并开始使用时，研究人员收到提醒并捕获到黑客的所有工具，进而可执行反编译等操作。通过MeterPreter的连接，可看出攻击主机的IP地址为45[.]43.192.172，端口号8443。

+ Monsoon（2016）：将截取的样本上传到Virus Total上进行分析，在此次攻击投放的RTF文档中发现修改文档的用户，对该作者进行VT搜索，找到了6个相关文件，并且这六个文件大小相同，利用同一个漏洞，很大可能来自于同一个人。
VT结果显示该文件是从IP地址37.58.60.195主机上WEB服务器提供的，该服务器还提供了其他类似的文件。
37.58.60.195 的地址是比利时的一个合法的邮件服务网站，YMLP公司。其他的恶意文档也指向YMLP。
利用YMLP，攻击者将恶意文档放在邮件中，并进行攻击。

#### 3.2 控制主机溯源取证
+ Patchwork：攻击者通过MeterPreter与攻击主机的通信，将文件内容上传至C&C控制主机，经监测，第一阶段的控制主机IP为212[.]129.13.110，第二阶段尝试对话的主机IP为212[.]83.191.156，遗憾的是，攻击方并没有成功进行第二阶段攻击。

+ Monsoon：Monsoon中的恶意代码在获取C&C地址的时候，相关C&C地址并未预留在恶意代码本身，而是存放在第三方可信网站中。
在第三方可信网站或论坛中预留C&C地址，如在github中
 ![image](https://github.com/JerryTang1996/nothing/blob/master/C%26C%201.png)
某论坛：
![image](https://github.com/JerryTang1996/nothing/blob/master/C%26C%202.png)
“{{“之后的内容就是加密后的C&C地址，但是该内容在原论坛中是看不到的，因为攻击者用白底白字进行评论。
进行解密之后，得到的C&C地址是http://5.254.98.68/mtzpncw/gate.php

+ 使用360威胁情报中心数据平台搜索一下其中一个C&C IP地址：94.242.249.206，可以看到相关IP已经被打上摩诃草的标签，这表明该IP曾经被摩诃草做为网络基础设施使用过：![](https://i.imgur.com/nJc4Zwd.png)

#### 3.3 攻击者以及攻击组织溯源取证
+ HangOver:  
溯源过程：  
1.通过相关文件扩展案例。  
已知文件的行为模式和文件结构使得可以搜索内部和公共数据库以查找类似情况，包括一些公共和商业数据库可用于额外的数据挖掘。  
——>由此发现的恶意软件数量惊人，推断出telenor不是单一入侵，而是持续攻击和危害全球政府和企业的一部分。  
意外发现：C&C服务器上包含一些世界可读文件夹，这些文件夹中包含的数据主要是来自受恶意软件影响的计算机的连接日志，键盘记和其他上载数据。许多收集到的日志来自属于安全公司的自动分析系统，但并不是全部。  
在这些服务器上的发现：额外的恶意可执行文件（可能是为了向受感染的用户提供服务），其中一些可执行文件使用2011年撤销的证书进行数字签字。于是搜索我们的证书数据库是，发现大量其他类似签名的可执行文件，都是一些恶意的应用程序。  
2.通过域名使用和注册进行案例扩展  
普遍情况下：1.攻击者注册的域名都是“隐私保护”，这意味这注册人已向域名注册商付款，以扣留与注册相关的身份信息。2.几乎所有属于此攻击者的网站都将其robot.txt设置为“禁止”以阻止被抓取。  
处理手段：通过在历史IP数据中搜索已知涉及的域的IP地址，发现许多可能属于同一基础架构的其他域，然后针对恶意软件进一步验证这些域，以确定与攻击者的有效连接。  
3.漏洞  
发现此攻击小组很少使用软件漏洞来植入恶意软件，他们只使用已知的漏洞，没有零日攻击。  
4.文件  
漏洞是CVE-2012-0158，此漏洞包含在Telenor攻击中使用的RTF文件中。这些文档虽然非常类似于其他文档，但是他已经被攻击者修改并用以其他目标攻击。设置了文档的超时时间，如果在超时时间之前漏洞被触发利用，shellcode会执行两项不同的操作：  
5.网页    
调查攻击者域结构时发现了更多的漏洞利用代码，这次他是作为域名 you-post.net的主网页上的脚本实现的，该漏洞是CVE-2012-4792，是个IE漏洞。当它触发时，会从域softmini.net下载并运行恶意可执行文件。  
6.smackdown木马  
在攻击的第一阶段恶意软件都是smackdown的变种。通常，该木马首先将系统信息上载到C&C服务器上的PHP脚本，攻击者可以单独决定哪个受感染的计算机应该接受其他恶意软件，第二阶段恶意软件通常是Hanove，也使用其他恶意软件系列。  
7.HangOver恶意软件，aka(also known as) 又称为Hanove  
第二阶段的恶意软件通常是HangOver的变体，是用C++编写的信息窃取程序。似乎实在一个通用框架上编写的，因为许多内部函数是相同的，但是从一个子类型到另一个子类型，整体功能可能会有很大差异。这些的第一个版本坑基于一个无辜备份实用程序的代码，因为经常出现“备份”一词。  
Hanove Uploaders以递归方式扫描文件夹，查找要上传的文件。Hanove keyloggers用以捕获键盘记录到文本文件中。其他变体也捕获其他数据，例如剪贴板内容，屏幕截图，打开窗口的标题和浏览器编辑字段的内容等。再设置定时时间将数据上载到远程服务器。  
+ Snake In The Grass：提取恶意代码的脚本之后，可以发现它与2013年的Hangover攻击使用的是相同的 C&C 服务器。同时在部分AutoIT的恶意程序中，HTTP请求与Hangover攻击中的HTTP请求格式相同。因此两者的网络构建是相关联的。所以说本次攻击与Hangover攻击是同一个组织。
+ Patchwork：通过控制一台C&C控制主机，研究人员获取了大量的恶意代码和PPS文件，发现了攻击时间、域名注册等时间和PPS文件最后编辑时间具有一致性，进而判断出攻击者所处的时区；再根据APT攻击的意图、需求以及钓鱼软件的内容，判断攻击者与印度有关。
+ Monsoon:攻击者：可以通过几点来确定谁参与到了Monsoon活动中
	1.	域名注册方式与Monsoon的注册方式类似
	2.	域名的注册者生活在印度
	3.	注册该域名的人在开源的代码交流论团上有留下了个人信息

#### 3. 4 攻击组织刻画
##### 3. 4. 2 组织来源地域
+ HangOver:  
1.项目与调试路径  
找到带有调试路径的恶意软件并不常见，但是这个组织的攻击者貌似并不介意留下这些迹象，或者他们可能都不知道他们的存在，这些调试路径提供了更多可帮助溯源的指标，表明攻击者是印度人。许多Visual Basic键盘记录器包含名称“Yash”，他可能是印度名称的缩写。  
2.托管环境  
项目路径还可以看到一些我们几乎从未见过的东西—托管恶意软件创建环境，其中多个开发人员负责特定的恶意软件提供。有许多不同的项目路径指向不同的人在不同的子项目上工作，但显然没有使用集中的源控制系统，这些项目似乎被委派，其中一些似乎遵循月度周期。某些系列的恶意软件包含“已交付”字符串，这些字符串与松散的项目结构一起可能表明该组织的开发工作是外包进行的。  
3.“Appin”词汇  
在大量的孤立案例和背景下，“Appin”这个词出现了，这似乎与名为Appin Security Group的印度公司存在某种联系。这其中也许有人试图通过伪造证据来牵连Appin，可能Appin Security Group中的一些流氓代理，或许还有其他的解释，但尚不得知。字符串“Appin”，“ Appin Security Group”和“Matrix”经常在可执行文件中的调试路径中找到，但是任何人都可以添加或更改此类文本字符串，所以并不能就此得出某些结论。  
4.域名注册  
虽然对于大部分的域名注册信息的隐私保护做的都很到位，但是在使用过的域名中其中有一些被确认为恶意域名就被暂停并失去了隐私保护，所以可以获取这些域名的注册信息。  
在研究对Telenor的攻击中可以发现，印度的攻击者已经对世界各地的企业，政府和政治组织进行了三年多的攻击。  
私营部门公司和与之相关的人员也可能受到了相关的攻击。  
无法确定该组织是否代表其他人进行了攻击，或者受谁委托。  
使用的攻击方法主要基于不同的社会工程策略而不是漏洞利用，但历史表明基于社会工程的攻击可以非常成功。  
+ Snake In The Grass：印度地区
+ Patchwork：印度或亲印地区
+ Monsoon(2016):Monsoon被匹配到印度拥有军事和政治力量的群体。许多受害者都在周边国家，包括孟加拉，斯里兰卡和巴基斯坦。但受害者也可能在更远的地方，包括非洲和远东地区。针对中国公民的攻击可能也与此组织，但同样也可能是对手单独活动的一部分，或者甚至作为他们以以前在HANGOVER集团中看到的类似活动的一部分。
##### 3. 4. 3组织主要目标及意图
HangOver:  
1.巴基斯坦  
最明显的目标似乎就是巴基斯坦，巴基斯坦的计算机是连接恶意域的最活跃的计算机。从2012日志里发现了疑似来自巴基斯坦大使馆的信息。为巴基斯坦量身定制的诱饵文件围绕着该地区长期持续冲突的的文化和宗教问题。  
2.中国  
中国是另一个显然在某种程度上称为目标的国家。例如，发现了一个看似从属于中国学术机构的计算机中收集的数据和键盘记录。这个数据集创建于2012年7月，包含word文档，ppt演示文稿和图像。也存在诱饵文件，但是数量上不如巴基斯坦。  
3.哈里斯坦运动   
政治分裂主义运动，试图在印度旁遮普地区建立一个叫做哈里斯坦（“净土”）的独立国家，作为锡克教徒的家园。自1971年该运动成立以来，历史上发生了哈里斯坦运动支持者和政府军之间的暴力事件。针对该运动的恶意软件的例子是md5的a4a2019717ce5a7d7aec8f2e1cb29f8和f70a54aacde816cb9e9db9e9263db4aa。而前者似乎是此文件：http://f00dlover.info/Khalistan/Victims_want_Sajjan_Kumar_punished.doc.zip。  
值得注意的是，f00dlover.info历史上与其他许多属于此组织的域共享IP173.236.24.254。例如Telenor入侵中使用的域researcherzone.net。此IP上还存在许多具有活动网页的域名，例如khalistancalling.com。目前，khalistancalling.com已经被转移到另外一个恶意IP上（46.182.14.83），应该是被攻击者所持有的IP。  
4.那加兰邦运动  
那加兰邦运动是另一个分裂主义组织，旨在为居住在印度东北部和缅甸西北部的纳伽人创建一个主权家园，已经发现至少两次袭击是针对此组织的。   
5.工业间谍  
Telenor案件最有可能是工业间谍活动，并且还在研究中发现了几个明确针对企业的相关攻击文件。  
1.可能的企业目标：欧亚自然资源公司（ENRC），是众多恶意软件的目标  
2.已知的企业目标：印度尼西亚Bumi PLC公司（一家在伦敦证券交易所上市的国际矿业集团），其董事长Samin Tan先生于2012年7月遭遇鱼叉式钓鱼攻击。  
3.可能的目标：保时捷，使用名称“webmailapp.exe”的恶意软件是一个包含多个文件的自解压存档，其中一个批处理文件打开一个缩短的URL，指向Porsche Holding在奥地利的Webmail前端。但是目前没有关于是否发生入侵的信息。  
4.可能的目标：餐饮业，证据1取自可执行文件的诱饵文件指向是对餐饮业的攻击，证据2某些恶意域名与正规餐饮业的域名类似。  
5.可能的目标：芝加哥商品交易所（Chicago Mercantile Exchange），在调查恶意域名web-mail-services.info时，发现许多其他域名与其共享IP地址188.95.48.99。其中一个域名是cemgroups.net，是对cemgroup.net的恶搞，而该正常域名属于芝加哥商品交易所（CME），CME是世界上最大的期货交易所公司。  

Snake In The Grass：巴基斯坦地区，窃取军事方面的情报。

Patchwork：东南亚及中国南海，窃取军事、政治情报。

Monsoon：中国，巴基斯坦，印度周围地区。

2018年出现了针对美国智库组织的攻击，意图包括收集文件、键盘记录和下载执行。
##### 3. 4. 4组织攻击方法

| 攻击方法 | 攻击载体               | 攻击描述                                                     |
| -------- | ---------------------- | ------------------------------------------------------------ |
| 钓鱼攻击 | 鱼叉邮件               | 主要使用了python编写的脚本，通过PyInstaller 和 Py2Exe 两种打包方式将其打包成 exe 程序。利用CxsSvce.exe,send.exe调用系统工具systeminfo.exe 检测虚拟机，上传指定目录下的文件到服务器，从服务器下载文件并执行；使用reg.exe，reg1.exe修改注册表添加启动项、winrm.exe，stisvc.exe偷窃文件，同时，使用sppsvc.exe，key.exe记录键盘活动。 |
| 钓鱼攻击 | 包含恶意代码的邮件附件 | Patchwork利用CVE-2014-4114漏洞，释放inf文件，执行Autolt脚本，首先通过sysvolinfo.exe进行提权并与攻击主机互联，向C&C控制主机上传文件，若有价值则释放7zip.exe，将其重命名隐藏在特定文件夹内，并设置为开机自启动，持续不断向控制主机上传内容。 |
| 钓鱼攻击 | 包含恶意文档链接的邮件 | Patchwork利用CVE-2017-8570漏洞，用户在打开RTF文档之后启动脚本，脚本启动%temp%下的可执行文件，最终调度执行一个QuasarRAT文件，用于收集用户计算机上的文件、记录用户键盘输入等恶意行为。 |


### 4. 溯源结果分析

#### 4.1 溯源方法评价

由于该组织使用最广泛的攻击手段是鱼叉邮件，因此溯源方法多为对攻击代码进行分析，通过捕获到黑客的工具，进而可执行反编译等操作，分析得到攻击主机的IP地址，进一步得到C&C控制主机的IP地址，同时，通过分析代码片段的相似性，分析其攻击对象、攻击手段，从而确定攻击组织。
该溯源方法从溯源的四个层次出发，一步一步确定到攻击组织，条理清晰，结果也比较令人信服。

#### 4.2 溯源结果可信性

通过监控网络状态，捕获黑客使用的恶意软件，对其进行反编译等操作可以对攻击主机进行溯源；通过分析恶意代码，找到代码中的C&C主机地址或存放在第三方可信网站中的C&C地址，可以对攻击控制主机进行溯源取证；通过对攻击中使用的代码、域名/IP关联分析，结合攻击者的活动时间、攻击目标等信息进行分析，可以确定这些攻击都是由来自印度的摩诃草组织所发起。溯源方法正确，过程也很详细。

比如在Snake in the Grass攻击中，提取恶意代码脚本之后，发现这次攻击与2013年的Hangover攻击使用的是相同的C&C服务器；并且在部分恶意程序中他们的HTTP格式相同，因此两次攻击的网络构建是相关联的。所以有理由相信这两次攻击由同一个组织发起。在Patchwork攻击中，通过分析恶意代码和PPS，可以得到攻击时间、域名注册时间，进而判断攻击者所处的时区。再结合APT攻击的意图、需求以及钓鱼软件的内容，判断攻击者来源于印度。在较晚一点的攻击中，使用360威胁情报中心数据平台搜索攻击中使用的C&C地址，已经可以看到此IP已经被打上摩诃草的标签，表示此IP曾被摩诃草作为网络基础设施使用过。

### 6. 总结

在本次实验中，我们小组阅读了各安全厂商对摩诃草组织的攻击行动的分析报告，学习了常见的APT事件的分析思路和报告框架，对该组织使用的攻击方法有了一定的了解，也调研了各安全厂商使用的溯源技术，加深了对课堂所学内容的理解。
