# 1. Jemter的下载和启动
**下载网址：**[**https://jmeter.apache.org/download_jmeter.cgi**](https://jmeter.apache.org/download_jmeter.cgi)

<!-- 这是一张图片，ocr 内容为：TWITTER GITHUB DOWNLOAD APACHE JMETER YOU ARE SURENTY USNG HTER WITH  MITRO SELER MITH  YOU ENCOUNLER A PROBLEN WITH  THIS SELECT ANOT /FFA MIRRORS ARE FAILING. THERE ARE BACKUP MIRRORS (AT THE END OF THE MIRRORS LIST) THAT SHOULD BE AVAILE. OTHER MIRRORS:HTTPS://DLCDN.APACHE.ORG/ CHANGE A NEYS INKLNKS TO THE CODE SIANING KEYS USED TO STAN THE PRODUCT THE POR LINK DOWNLOADS CAMPATBLE STA THE KEY FTOM OUT NAIN STE, THE SHA-512  INK DOWNLOADS THE SHAS12 CHECKSUM FROM THE MAIN STERIV THE  NTEGILY  DOWNLOADED FILE FOR MORE INFORMATION CONCERNING APACHE JMETER, SEE THE APACHE JMETER SITE. KEYS APACHE JMETER 5.5(REQUIRES JAVA 8+) BINARIES APACHE-IMETER-5.5.TAZ SHA512 PGP APACHE-JMETER-5.5.ZIPSHA512 PGP SOURCE APACHE-JMETER-5.5 SRC.TGZ SHA512 PGP Z PGP APACHE-JMETER-5.5 SRC.ZIP SHA512P -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670327072301-6f1e4bcd-9a01-451d-ad9d-0ded94c05d95.png)

**解压使用：**

1. **打开bin下面的jmeter.properties中配置。修改**<font style="color:rgb(38, 38, 38);">sampleresult.default.encoding的值为UTF-8</font>

> **sampleresult.default.encoding=UTF-8**
>

2. **双击  jmeter.bat  启动**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1769742248948-7d20d37e-a954-46ab-a2d0-1e654772dd14.png)

3. **如果界面是英文，可以调整成中文**

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1769742144129-cbc2457e-396d-4e4a-a3fe-372efe31eb4d.png)

# 2. 开始使用
1. 测试计划--右键--线程--添加线程组

<!-- 这是一张图片，ocr 内容为：文件 工具 运行 帮助 查找 编辑 选项 TIVAR 测试计上 OPEN MODEL THREAD GROUP 线程(用户) 添加 SETUP线程组 配置元件 粘贴 CTRL-V TEARDOWN 线程组 监听器 打开.. 线程组 定时器 合并 选中部分保存为.. 前置处理器 后置处理器 保存节点为图片 CTRL-G 名称: 断言 保存屏幕为图片 CTRL+SHIFT-G 测试片段 启用 非测试元件 禁用 CTRL-T 切换 帮助 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670327753651-3bedf79c-05c5-45f9-b504-2f346cb0053a.png)

2. 线程组--右键--取样器--http请求

这里以请求百度为例，内容根据需要填上对应的值

<!-- 这是一张图片，ocr 内容为：文件编辑查找 选项 帮助 运行 工具 十 00:00:00 00/0 测试计划 HTTP请求 线程组 HTTP请求 名称: HTTP请求 以为请求百度为例 注释: 基本高级 WET.服务 协议 服务器名称或IP: 端口号 HTTP WWW.BALIDU.COM HIP请求 路径: GET 内容编码: 自动王定向 对POST使用MULIPART/FONM-DATA 跟随重定向 使用KEEPALIVE 与谢览器兼容的头 文件上传 参数消息体数据 同请求一起发送参数: 值 编码? 内容类型 包含等于? 名称: -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670327861179-71d23b95-563e-4195-a306-b81139fa8ae6.png)

3. 点中测试计划--右键--添加监听器--查看结果树

<!-- 这是一张图片，ocr 内容为：测试 线程(用户) 添加 十划 配置元件 粘贴 CTRL-V 查看结果树 监听器 打开.. 汇总报告 定时器 合并 聚合报告 选中部分保存为.. 前置处理器 后端监听器 后置处理器 保存节点为图片 CTRL-G JSR223监听器 断言 保存屏幕为图片 CTRL+SHIFT-G 保存响应到文件 测试片段 启用 响应时间图 非测试元件 禁用 图形结果 切换 CTRL-T 断言结果 帮助 比较断言可视化器 汇总图 生成概要结果 用表格查看结果 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670327964763-e2e31b65-115d-4c35-954f-1ec39df77eec.png)

4. 点中查看结果树，运行。

如果接口返回json格式化结果那里可以选择json

<!-- 这是一张图片，ocr 内容为：日 00:00:01 和试计划 查看结果树 线程组 HTTP请求 名称: 查看结果树 查看结果树 注样: 所有数据写入一个文件 仅成功日志 仅错误日志 测觉. 显示日志内容: 文件名 区分大小写 查找 重宵 正则表达式 查找: 取样器结果请求响应数据 HTML SOURCE FORMATTED HTTP请求 RESPONSE HEADERS RESPONSE BODY 区分大小写 FIND <LDODYPE HTML><!-STATUS OK--> <HTML> <HEAD> <META HTTP-EQUIV三"CONT-TYPE" CONTENTENT-"TEXT HTML:CHARSET二UTF-8"> 格式化返回结果 <META HTTP-EQUIV-"X-UA-COMPATIBLE"CONTENT-"IE-EDGE"> <META CONTENT-"ALWAYS" NAME-"REFERRER"> <LINK REL-"STYLESHEET" TYPE-"TEXT/ESS"HREF-"HTTP//S1.BDSTATICCOM/T/WWW/CACHE/BDORZ/BAIDUMINCS5> <LITLE>百度一下,你就知道</GLE> </HEAD> <BODY LINK-"#0000CC"> <DIV ID-"WRAPPER'> <DIVID-"HEAD">  <DIV DASS "HEAD WRAPPER"> <DIV DASS FORM"> <DIV DASS-'S FOMM WRAPPER"> <DIVID-'LG'> <ING HIDEFOCUS二"TRUE" SIC: 270-HEIGHIDU.COM/ING/BD LOGO1,PNG'WIDTH-"270-HEIGHE"129> </DIV> <FORM ID--FORM"NAME-"F ACTION三"/WWW.BAIDU.COM/S"CLASS-"FM"> <INPUT TYPE-"HIDDEN" NAME-"BDORZ COME"VALUE-"1"> <INPUT TYPE "HIDDEN" NAME-"IE" VALUE-"UTF-8"> -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670328150374-8b051edd-f4da-4bd8-914b-98cb4f137665.png)

5. 将测试计划保存下来，方便下次使用

<!-- 这是一张图片，ocr 内容为：APACHE JMETER(5.5) 选项工具帮助 文件 编辑查找运行 CTRL-L 新建 模板.. 打开 ARL-O HTTP请求 最近打开 1HTTP请求默认值JMX 名称: HTTP请求 合并 CTRL-S 保存 以为请求百度为例 注释: 保存测试计划为 CTRL+SHIFT-S 选中部分保存为.. 基本高级 保存为测试片段 WEB服务器 还原 服务器名称或IP: 协议: WWW.BAIDU.COM HTTP 重启 HTTP请求 路径: GET 眼随重定向 对POST快用MULTIPART/FORM-DATA 自动率定向 与浏览器兼容的头 使用KEEPALIVE 参数消息体数据文件上传 同请求一起发送参数; 名称: 值 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670329204646-a9b5e119-48b6-4e4c-a8f4-2ce8184f29b1.png)

# 3. 相关概念理解
1. **测试计划： **<font style="color:rgb(38, 38, 38);">测试计划可以看做一个项目</font>
2. **线程组：**<font style="color:rgb(38, 38, 38);">项目下面有多个模块（线程组）</font>
3. **http请求：**<font style="color:rgb(38, 38, 38);">模块下面有好多接口（http请求）</font>
4. **查看结果树：**<font style="color:rgb(38, 38, 38);">查看结果树就是接口返回的结果</font>

# <font style="color:rgb(38, 38, 38);">4. 查看结果树的额外说明</font>
1. 点中Http请求，ctrl+c。 再点中线程组 ctrl+v，复制一个Http请求（改个名字改成Http请求2）

<!-- 这是一张图片，ocr 内容为：文件编辑查找 运行 选项 工具 帮助 测试计划 线程组 线程组 HTTP请求 名称: 线程组 HTTP请求2 注释: 查看结果树 在取样器错误后要执行的动作 启动下一进程循环 立即停止测试 停止测试 停止线程 继续 线程属性 线程数: RAMP-UP时间(秒): 循环次数 永远 SAME USER ON EACH ITERATION 延退创建线程直到需要 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670329085789-bbcf8488-c81f-476e-9f95-1441aacbec71.png)

2. 点中线程组，ctrl+c 。再点中执行计划 ctrl+v，复制出一个线程组2

<!-- 这是一张图片，ocr 内容为：文件 工具 选项 直找 帮助 运行 测试计划 线程组 线程组 HTTP请求 线程组2 名称: HTTP请求2 注释: 线程组2 HTTP请求 在取样器错误后要执行的动作 HTTP请求2 停止线程 进程循环 立即停止测试 停止测试 启动下 继续 查看结果树 线程属性 线程数: RAMP-UP时间(秒): 永远 循环次数 SAME USER ON EACH ITERATION 延迟创建线程直到需要 调度器 持续时间(秒) 启动延迟(秒) -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670329263238-89f948eb-8439-42a1-91f3-f283b4a0e225.png)

3. 现在  查看结果树，线程组，线程组2平级。都是在测试计划下面。

 选中查看结果树点击运行按钮。我们可以看到四个请求全部发送

<!-- 这是一张图片，ocr 内容为：选项 运们 查找 具 测试计划 清除请求结果 查看结果树 线程组 HTTP请求 查看结果村 名称: HTTP请求2 注释: 线程组2 HTTP请求 -所有数据写入一个文件 HTTP请求2 文件名 查看结果树 区分大小写 正则表达式 查找 重置 查找: 请求响应数据 取样器结果 HTML SOURCE FORMATTED HTTP请求 RESPONSE BODY RESPONSE HEADERS HTTP请求 HTTP请求2 HTTP请求2 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670329542239-256d87f4-5491-4c9f-be55-8b48d485fdbb.png)

4. 把查看结果数拖到线程组内，再次点击运行，只会查看当前线程组内的请求结果

<!-- 这是一张图片，ocr 内容为：帮助 查找 编辑 选项 文件 工具 运行 测试计划 查看结果树 方线程组 HTTP请求 名称: 查看结果树 HTTP请求2 注释: 查看结果树 线程组2 所有数据写入一个文件 HTTP请求 文件名 HTTP请求2 区分大小写 正则表达式 重置 查找 查找: 取样器结果请求响应数据 HTML SOURCE FORMATTED HTTP请求 RESPONSE BODY RESPONSE HEADERS HTTP请求2 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670329600284-70788025-ac4b-4fca-b6d4-d6cea46e622e.png)

# 5. 并发和顺序执行
**并发执行**：多个线程组同时执行（默认情况）

<!-- 这是一张图片，ocr 内容为：选项 运行 帮助 查找 工具 十 测试计划 查看结果树 线程组 HTTP请求1-1 查看结果树 名称: HTTP请求1-2 注释: 线程组2 所有数据写入一个文件 HTTP请求2-1 HTTP请求2-2 文件名 查看结果树 正则表达式 区分大小写 甲质 查找 查找: 取样器结果请求响应数据 HTML SOURCE FORMATTED HTTP请求1-1 RESPONSE HEADERS RESPONSE BODY HITP请求2-1 HTTP请求2-2 HITP请求1-2 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670330035306-3b8cf8f5-bb35-4212-bffb-b3f936d448ef.png)

可以看出不同线程组下面是没有顺序的。



**顺序执行：**多个线程组顺序执行

在测试计划下勾上独立运行每个线程组

<!-- 这是一张图片，ocr 内容为：运行 文件 选项 编辑 查找 帮助 工具 测试计划 测试计划 线程组 HTTP请求1-1 测试计划 名称: HTTP请求1-2 注样: 线程组2 HTTP请求2-1 用户定义的变量 HTTP请求2-2 值 名称: 查看结果树 从剪贴板添加 利除 向上 添加 向下 详细 独立运行每个线程组(例如在一个组运行结束后启动下一个) 主线程结束后运行TEARDOWN 线程组 函数测试模式 只有当你否要记录每个请求从服务器数得的数易到文件时才备要选择函数测试技式,选择这个选项很影响性能. 叫除 清除 添加目录或JAR包到CLASSPATH 浏览.. -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670330167304-c58eb502-fd37-475a-a8ad-e2268db71e38.png)

再次运行就能看到请求是有顺序的

<!-- 这是一张图片，ocr 内容为：编辑 查找 选项 文件 帮助 工具 运行 测试计划 查看结果树 中线程组 HTTP请求1-1 名称: 查看结果树 HTTP请求1-2 注释: 线程组2 HTTP请求2-1 所有数据写入一个文件 HTTP请求2-2 文件名 查看结果树 区分大小写 查找: 正则表达式 查找 重置 请求响应数据 取样器结果 HTML SOURCE FORMATTED HTTP请求1-1 RESPONSE HEADERS RESPONSE BODY HTTP请求1-2 HTTP请求2-1 HTTP请求2-2 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670330202097-f03b989f-b0c1-4d91-9c35-4cf62d60f36d.png)

# 5. 优先和最后执行的线程组
点击测试计划新增两个线程组

<!-- 这是一张图片，ocr 内容为：帮助 工具 选项 文件 编辑 运行 查找 十 测试计 OPEN MODEL THREAD GROUP 线程(用户) 添加 TEI SETUP线程组 配置元件 粘贴 CTRL-V TEARDOWN线程组 监听器 打开... SE 线程组 合并 定时器 选中部分保存为.. 前置处理器 保存节点为图片 后置处理器 CTRL-G 名称: 断言 CTRL+SHIFT-G 保存屏幕为图片 查 测试片段 启用 非测试元件 禁用 CTRL-T 切换 帮助 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670331278347-8aa212dd-6839-4471-b0f8-5cccd5418f8a.png)

然后执行

<!-- 这是一张图片，ocr 内容为：工具 编辑查找运行选项 帮助 测试计划 查看结果树 TEARD OWN 线程组 最后执行的请求 查看结果村 名称: SETUP 线程组 注释: 优先执行的请求 线程组 所有数据写入一个文件 HTTP请求1-1 文件名 查看结果树 区分大小写 正则表达式 查找 重宵 查找: 取样器结果 请求 响应数据 HTML SOURCE FORMATTED 优先执行的请求 RESPONSE BODY RESPONSE HEADERS HTIP请求1-1 HTTP请求1-2 最后执行的请求 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670331318597-2b8a9d66-566a-45e0-a2c9-827e9adc53f6.png)

可以看出**setUp 线程组**的优先级比较高会先执行。

**tearDown** 线程组的优先级比较低会最后执行。



**疑问：如果有两个****<font style="color:rgb(38, 38, 38);">setUp或者两个tearDown哪个优先级高？</font>**

<!-- 这是一张图片，ocr 内容为：文件系编辑 帮助 查找 选项 工具 运行 测试计划 查看结果树 SETUP线程组2 优先执行的请求2 名称: 查看结果树 TEARDOWN线程组 注释: 最后执行的请求 TEARDOWN线程组2 -所有数据写入一个文件 最后执行的请求2 文件名 SETUP线程组 优先执行的请求 区分大小写 正则表达式 查找 重胃 查找: 线程组 HTTP请求1-1 取样器结果请求响应数据 HTML SOURCE FORMATTED HTTP请求1-2 查看结果树 优先执行的请求2 RESPONSE BODY RESPONSE HEADERS 优先执行的请求 HTTP请求1-1 <!DOCTYPE HTML><!--STATUS OK--> HTTP请求1-2 <HTML> 最后执行的请求 <HEAD> 最后执行的请求2 PE" CONTENT-"TEXT/HTML'CHARSET-UTF-8"> <META HTTP-EQUIV-"CONTENT-TYPE" CON <META HTTP-EQUIV-"X-UA-COMPATIBLE" CONTENT三"IE-EDGE"> <META CONTENT-"ALWAYS"NAME"REFERRER"> <LINK REL-"STYLESHEET" TYPE-"TEXT/ESS"HREF- HTTP://S1.BDSTATICCOM/T/WWW/CACHE/BA <TILE>百度一下,你就知道</ITLE> </HEAD> <BODY LINK"#000CC"> <DIV ID -WRAPPER"> <DIV ID  HEAD"> <DIV CLASS "HEAD WRAPPER"> -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670331627092-50264d34-45f2-4631-8061-0e93c49c0d5c.png)

**结论：哪个线程组在上面，哪个就是爸爸。优先级高**

# 6. 线程组常用的属性
<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1769742926403-7219f7a2-88ee-4642-8bd4-42f30871759c.png)

# 7. HTTP请求默认值
对于多个接口url而言，可能就是请求路径部分不同。而请求协议，请求的服务器名或ip，端口都一样。这样我就可以吧共同的部分抽取出来。在jmeter中http请求默认值可能做到这一点。

1. 新增一个Http请求默认值

<!-- 这是一张图片，ocr 内容为：运行 帮助 工具 选项 文件编辑查找 测试 线程(用户) 添加 江山 CSV数据文件设置 配置元件 粘贴 CTRL-V 丝 HTTP信息头管理器 监听器 打开.. HTTP COOKIE管理器 合并 定时器 HIP缓存官理希 选中部分保存为.. 前置处理器 HTTP请求默认值 保存节点为图片 后置处理器 CTRL-G BOLT CONNECTION CONFIGURATION 断言 名称: 保存屏幕为图片 CTRL+SHIFT-G DNS缓存管理器 测试片段 启用 FTP默认请求 非测试元件 禁用 HTTP授权管理器 切换 CTRL-T JDBC CONNECTION CONFIGURATION 帮助 JAVA默认请求 LDAP扩展请求默认值 LDAP默认请求 TCP取样器配置 家相良配架 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670475413505-631731c9-3559-441f-b6e9-f84fa736311d.png)

2. 把共同的部分抽取出来

<!-- 这是一张图片，ocr 内容为：带助 十 00:00:00 /T 001 测试计划 HTTP请求默认值 HTTP请求默认值 线程组 名称: HTTP请求默认值 HTTP请求 注释: 查看结果树 基木高级 WAH服务果 协议: 服务器名称或IP: 端口号: 8080 HTTP LOCALHOST HIP请求 内容编码: 路径: UTF-8 参数 消息体数据 同请求一起发送参数: 值 内容类型 包含等于? 名称: 演码? -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670475477969-15222f8d-c668-401b-823b-dce0dc5492e8.png)

3. http请求中只要写路径

<!-- 这是一张图片，ocr 内容为：0 0/ 00:00:00 测试计划 HTTP请求 HTP请求默认值 线程组 名称 HTTP请求 HTTP请求 注释: 查看结果树 基本高级 WEB服务器 端口号: 服务器名称或IP: 协议: HITP请求 内容编码: GET 路径 /HELLO 自动重定向 对POST使用MULTIPART/FORM-DATA 与浏览器兼容的头 规范章定向 使用 KEEPALIVE 参数消息体数据 文件上传 同请求一起发送参数: 值 包含等于 名称: 内容类型 编码? TEXT/PLAIN -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670475526502-ba586545-413e-4bbe-a0ba-cb99d41c8d94.png)

# 8. HTTP信息头管理器
对于一些新增或者修改接口。我们需要提交一些json数据，如果没添加请求头可能会报一些错误。

如下：新增一个用户，接口报错

<!-- 这是一张图片，ocr 内容为：文件编辑有我运行进项工具表 0( 00:00:00 利试计划 HTTP请求 HTTP请求意认值 0.线理组 名称 新增一个USER 注释 查看结果树 奇级 服务器名称照IP 岩口号: HIP法求 内容编码 路径: POST 与对总器作育的头 三动画定向 参数消息体数据 文件上传 101 3 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670476080708-c66e4a73-4294-4b1a-a659-7b117cb5c7a2.png)

<!-- 这是一张图片，ocr 内容为：连项 工具是助 查找 运行 000000 51000 001 查看结果树 TIP请求实认值 名称: 查看结果树 注释: 查看结果树 所有数据写人一个文件 仅钻没日志 仅成功日志 显示目志内容 五件名 区天人小写 三刘小达去 查找 取样器结果请求 中应数据 新增一个USTIR RESSONSE BODY RESPONSE HEADERS 正品专 区分大小弓 [THINEZTAMP;;3022 12 Q8TO59321 248:00.3'UNSUPPOR;'NSUPPOR;UNSUPPORTED MEDIS TYPE''MESSAGE' -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670476112137-e3aeee63-b434-4fd3-9373-165692ee263b.png)

这时我们需要用**http信息头管理器**添加请求头解决

<!-- 这是一张图片，ocr 内容为：文件 选项 工具 编辑 查找 运行 帮助 测试计划 测试计划 线程(用户) 添加 CSV数据文件设置 配置元件 粘贴 CTRL-V 线 HTTP信息头管理器 监听器 打开.. HTTP COOKIE管理器 定时器 合并 HTTP缓存管理器 查 选中部分保存为.. 前置处理器 HTTP请求默认值 后置处理器 CTRL-G 保存节点为图片 名称: BOLT CONNECTION CONFIQURATION 断言 保存屏幕为图片 CTRL+SHIFT-G DNS缓存管理器 测试片段 启用 FTP默认请求 非测试元件 禁用 HTTP授权管理器 切换 CTRL-T JDBC CONNECTION CONFIGURATION 帮助 JAVA默认请求 LDAP扩展请求默认值 LDAP默认请求 TCP取样器配置 密钥库配置 用户定义的变量 登陆配置元件/素 简单配置元件 计数器 随机变量 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670475936372-4806c7e6-224d-463e-b3d5-6e153a97e78c.png)

<!-- 这是一张图片，ocr 内容为：件 运行 选项 帮助 直找 工具 001 00:00:00 测试计划 HTTP信息头管理器 HIIP信息头管理器 HTTP请求默认值 名称: HTTP信息头管理器 4线程组 注释: 新增一个USER 查看结果树 信息头存储在信息头管理器中 名称: CONTENT-TYPE APPLICATION/JSONICHARSET-UTF-8 保存 载入 从剪贴板添加 添加 利除 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670476317833-17c66dc3-0068-481e-9406-f07f9feeaa20.png)

# 9. 参数化
## 1. 用户定义的变量
<!-- 这是一张图片，ocr 内容为：带助 互议 远项 神雅 十I个 测试计划 测试计划 线程(用户) 添加 HT 配置元件 CSV数据文件设置 粘贴 CTRL-V HT 监听器 HTTP信息头管理器 线 打开.. HTTPCOOKIE管理器 定时器 合并 HTTP缓存管理器 选中部分保存为.. 前置处理器 HTTP请求默认值 后置处理器 保存节点为图片 CTRL-G BOLT CONNECTION CONFIGURATION 断言 保存屏幕为图片 CTRL+SHIFT-G DNS缓存管理器 测试片段 启用 FTP默认请求 非测试元件 禁用 HTTP授权管理器 切换 CTRL-T JDBC CONNECTION CONFIGURATION 帮助 JAVA默认请求 LDAP扩展请求默认值 LDAP默认请求 TCP取样器配置 密钥库配置 用户定义的变量 登陆配置元件/素 简单配置元件 计数器 随机变量 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670476856016-4fe318c8-a9e2-42cf-a909-c60b0d1a2727.png)

新增一个addUser的变量。值为接口路径（/user）

<!-- 这是一张图片，ocr 内容为：工具 选项 查找 运行 带助 00:00:00 001 测试计划 用户定义的变量 用户定义的变量 HITP信息头管理器 用户定义的变量 名称: HTTP请求默认值 注样: 线程组 新增一个USER 用户定义的变量 查看结果树 名称: 值 /USER ADDUSER 向上 从剪贴板添加 添加 利除 详细 向下 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670476975036-03e7e4cb-0ec4-400f-9fd6-d76b75564b65.png)

在请求中引用变量即可

<!-- 这是一张图片，ocr 内容为：选项 文件 运行 热线 查找 帮助 十 00:00:00 001 测试计划 HTTP请求 用户定义的变量 HTTP信息头管理器 新增一个USER 名称: HTTP请求欧认值 注程: 线程组 新增一个USER 基本 高级 查看结果树 WEB服务器 服务器名称或IP; 协议: HTTP请求 路径 $LADDUSER] POST 对POST使用MULTIPART/FORM-DATA 自动重定向 与浏览器兼容的头 使用KEEPALIVE 跟防重定向 文件上传 参数消息体数据 1日 Z3 "NAME":"ZXX" 0 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670476994370-c47c1e97-0307-4190-aae1-4cd5419915c0.png)

## 2. csv批量操作
假设我想新增多个user，我们可以用csv进行批量添加。

1. 我们先用Excel写好三个用户并另存为csv文件。

<!-- 这是一张图片，ocr 内容为：K A 张三 18 李四 8 王五 20 保存此文件 文件名 待新增的用户 .CSV 选择位置 文档 ONEDRIVE-个人 要共享此文件吗? 更多选项... 保存(S) 取消 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670477646670-bab95372-ce09-4a80-a763-84ad18626be0.png)

2. 新增csv数据文件设置

<!-- 这是一张图片，ocr 内容为：AD 测试计划 测试计划 线程(用户) 添加 用人 配置元件 CSV数据文件设置 粘贴 CTRL-V HTTP信息头管理器 监听器 HT 打开.. HTTP COOKIE管理器 线 定时器 合并 HTTP缓存管理器 选中部分保存为.. 前置处理器 查 HTTP请求默认值 后置处理器 保存节点为图片 CTRL-G BOLT CONNECTION CONFIGURATION 断言 保存屏幕为图片 CTRL+SHIFT-G DNS缓存管理器 测试片段 启用 FTP默认请求 非测试元件 禁用 HTTP授权管理器 切换 CTRL-T JDBC CONNECTION CONFIGURATION 帮助 JAVA默认请求 LDAP扩展请求默认值 LDAP默认请求 TCP取样器配置 密钥库配置 用户定义的变量 登陆配置元件/素 简单配置元件 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670477846456-e8458970-0f2f-4d88-9031-cd3228a18865.png)

3. 我们的csv中有两列。我们在变量名称那一行定义好变量名用英文逗号分割

**变量名name,age对应csv中的姓名和年龄**

<!-- 这是一张图片，ocr 内容为：数据文件设置 CSV数据文件设置 名称: 注释: 设置CSV数据文件 C:/USERS/ZWRAN/DOCUMENTS/待新增的用户.CSV 浏览... UTF-8 变量名称(西文逗号间隔: NAME.AGE 忽略首行(只在设置了变量名称后才生效: FALSE 分阳符(用"T代替制表符: 是否允许带引号? FALSE 遇到文件结束符再次循环? FALSE 遇到文件结束符停止线程? TRUE 所有线程 线程共享模式 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670478113191-c85287b9-1514-4d3a-88d6-eb1efbe78f90.png)

4. 在请求中引用name和age变量

<!-- 这是一张图片，ocr 内容为：文件 工具帮助 编料 选项 查找 运行 测式计划 HTTP请求 用户定义的变量 HTTP信息头管理器 名称: 新增多个个USER HTTP请求默认值 注释: CSV数据文件设置 线程组 基本 高级 新增多个个USER 查看结果树 WEB服务器 服务器名称或IP 出口号: 协议: HTTP请求 POST SHADDUSER) 格径: 与则览卷兼容的头 对天POST使用MULTIPART/FORM-DATA 使用KECPALIVE 文件上店 "AGE"$FAGEL" -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670478357962-9e921d78-798e-4545-bcea-cb049099dbcd.png)

5. 选中查看结果树，点击运行

<!-- 这是一张图片，ocr 内容为：工具 帮助 运行 文件 选项 查找 十 测试计划 查看结果树 用户定文的变量 HTTP信息头管理器 查看结果树 名称: HTTP请求歌认值 注样: CSV数据文件设置 线程组 所有数据写入一个文件 新增多个个USER 显示日志内容: 文件名 流览 查看结果网 区分大小写 重量 正则表达式 查找: 直找 取样器结果请求响应数据 TEAYT 新增多个个USER REQUEST BODY REQUEST HEADERS POST HTTP://LOCALHOST:8080/USER 1234567890 POST DATA 张 "NAME "AGE 18 NO COOKIES -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670478532770-cd43c8e2-663e-44a6-9504-e7276beac06f.png)

我们发现只是新增了一个。还有的数据没有新增。是因为我们还需要做一项设置，把线程组的永远勾上。就会循环添加所有用户了。

<!-- 这是一张图片，ocr 内容为：测试计划 线程组 用户定义的变量 HTTP信息头管理器 线程组 名称: HTTP请求默认值 注释: CSV数据文件设置 线程组 -在取样器错误后要执行的动作- 新增多个个USER 停止线程 立即停止测试 启动下一进程循环 停止测试 继续 查看结果树 线程属性 线程数: RAMP-UP时间(秒): 循环次数 永远 SAME USER ON EACH ITERATION 延迟创建线程直到需要 调度器 持续时间(秒) 启动延迟(秒) -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670478589525-0e3b4260-667c-4f4c-8d2b-47a00b1b1eaf.png)

6. 再次运行

<!-- 这是一张图片，ocr 内容为：文件 编辑 查找 运行 选项 工具 帮助 AD 测试计划 查看结果树 用户定义的变量 HTTP信息头管理器 名称: 查看结果树 HTTP请求默认值 注释: CSV数据文件设置 线程组 所有数据写入一个文件 新增多个个USER 文件名 查看结果树 区分大小写 查找: 取样器结果 请求响应数 LEXT 新增多个个USER REQUEST HE REQUEST BODY 新增多个个USER 新增多个个USER -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670478638434-44ba5cd1-527e-4138-8f45-97da38323da2.png)

## 3. 用户参数
1. 在请求上添加用户参数

<!-- 这是一张图片，ocr 内容为：澳风双 HTTP请求 用户定义的变量 HTTP信息头管理器 名称: 新增USER HTTP请求默认值 注释: 线程组 新增USER 基本 高级 断言 查看结 添加 插入上级 定时器 WEB服务器 剪切 CTRL-X 前置处理器 JSR223预处理程序 服务器名称或IP 复制 CTRL-C 用户参数 后置处理器 粘贴 CTRL-V 配置元件 HTML链接解析器 路径: $FADDUSER] 复写 CTRL+SHIFT-C 监听器 HTTPURL重写修饰符 删除 DELETE 随重定向 使用KEEPALIVE 对POST使 JDBC预处理程序 打开.. 取样器超时 件上传 合并 正则表达式用户参数 BEANSHELL预处理程序 选中部分保存为.. NAME 保存为测试片段 :"${AGE}" 34 AGE 保存节点为图片 CTRL-G 保存屏幕为图片 CTRL+SHIFT-G 白田 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670484176847-7373e5c8-9a4e-442d-9cb5-a6fdc64678d6.png)

2. 添加两个用户

<!-- 这是一张图片，ocr 内容为：漏筒查找运行选项工具 有自大百 001 CO:00:00 制造计划 用户参数 用户定文的变量 HITP信息头管理器 用户参啦 名称: 注行: 口每次达代更新一次 参粒 用户2 用户.1 名称: 麻子 添加变量:相当于添加多一行 添加用户:添加列 国际变量 济山变量 添如用户 刚除用户 向下 品质园个中 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670484497405-709980e4-e6b4-4e6c-a3c2-92e3404e7f6d.png)

3. 点击线程组，把线程数调整为2，因为我们这里模拟的是两个用户操作

<!-- 这是一张图片，ocr 内容为：维斜吉我运行 文化 选项 工具帮助 有目 十 00/3 00:00:02 测试计划 线程组 用户定义的变量 HIIP信息头竹理器 名称: HTTP请求职认值 江祥: 直着结果树 战程组 在取样器诺误后要执行的动作 新增USEN 停止门试 停止线程 立印停止交通 启动下一进程循环 线程易性 线程数: NAMP-UP时间间(秒): 循斗次教 口永远 延迟创建线程直到需要 调度器 持续时间秒 HA通秒) -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670484688644-d96f0419-99e6-476a-89ae-afc74d992833.png)

4. 点击查看结果树，运行

<!-- 这是一张图片，ocr 内容为：测试计划 查看结果树 用户定定的变量 名称: 查看结果树 HP请求实认值 注释: 吉利结果树 规程组 所有数据写入一个文件 制造USER 仅错误目志口仅成功目 是示日志内容 文件名 口正则未洁式 区分大小弓 直线: 直找 电应数据 取样器结果 新增USER 新增USER 区分大个写 P0ST HTTP://TOCATHOST:8080/USER OST DATA 567888 NO COOKIES -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670484719423-d258940e-5440-49c1-8067-f2d4f6554991.png)

## 4. 计数器函数
1. 我们定义一个请求百度的http请求，并给线程组设置如下

<!-- 这是一张图片，ocr 内容为：002 000008 测试计划 线程组 HTTP请求 名称: 线程组 直着结果树 注样: 在取样器错误后要执行的动作 停止线理 启动下一进程循坏 停止通试 主明平止建式 我们同性 设置两个线程 2 然开数: :1 HARNP-UP时间(秒). 每个线程执行3次 口永过3 钻吓改改 SANNE USES ON GARH ILCEALION 近迟创建线程直到需要 调度器 持续时间秒> 启动话退砂) -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670485333663-dcbe80bc-8d15-4563-93d1-7336e497ae1f.png)

2. 执行后我们可以看到发送了6次请求 （2 * 3 ）

<!-- 这是一张图片，ocr 内容为：工具帮助 选项 香找 编辑 运行 文件 中 测试计划 查看结果树 线程组 HTTP请求 直看结果树 名称: 查者结果树 注释: 所有数据写入一个文件 测览一 文件名 区分大小写 正则表达式 查找 重冒 直找: 取样器结果请求响应数据 HITP请求 KESPONSE BODY RESPONSE HEADERS HTTP请求 HITP请求 HTTP请求 HTTP请求 HTTP请求 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670485371457-af7f967e-15c8-44b5-a7c2-0722ed08d02c.png)

3. 这时我们想统计总共发了几次请求。就需要加一个计数器函数

<!-- 这是一张图片，ocr 内容为：文件 编辑 帮助 选项 工具 查找 运行 1点击这个生成函数 测试计划 查看结果树 线程组 HTTP请求 查看结果树 名称: 查看结果树 注释: 所有数据写入一个文件 文件名 函数助手 2选择COUNTERIM COUNTER 函数参数 名称: 3先写TRUE试效果 TRUE,每个用户有自己的计数器;FALSE,使用全局计数器 TRUE 存储结果的变量名(可选) 4复制这个变量名 拷贝并粘贴函数字符串 生成 重置变量 COUNTER(FALSE.)] SI THE RESULT OF THE FUNCTION IS START.MS-1670478443908 当前JMETER变量 TESTSTART.MS-1670485334833 23456 DMETERTHREAD.LAST SAMPLE OK TRUE START.HMS 134723 START.YMD-20221208 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670485578405-082d81bf-fbf1-4258-bc90-bb8dbcafd6ba.png)

4. 使用复制的变量

<!-- 这是一张图片，ocr 内容为：测试计划 HTTP请求 中线程组 HTTP请求$L_COUNTER(TRUE.)] HTTP请 名称: (S(COUNTER(TRUE.)) 查看结果树 注样: 基本 高级 WEB服务器 协议: 服务器名称或IP: HTTPS WWW.BAIDU.COM HTTP请求 路径: GET 使用 KEEPALIVE 与浏览器兼容的头 自动重定向 跟防重定向 对POST使用MULTIPART/FORM-DATA 参数消息体数据 文件上传 同请求一起发送参数: 名称: 编码? -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670485647490-da517dae-d9d9-4247-b9ff-8d85e6d9aa02.png)

5. 可以看到有2个用户。每个用户单独统计

<!-- 这是一张图片，ocr 内容为：测试计划 查看结果树 6线程组 HITP请求SL G COUNTERFTRUE.)] 名称: 查看结果树 香看结果网 注释; 所有数据写入一个文件 显示日志内容: 仅错误日志 文件名 正见真达式 区分大小写 重营 查找 响应数据 请求 取样器结果 HTTP请求1 RESPONSE BODY RESPONSE HEADERS HTTP请求2 FIND HTTP请求3 HTTP请求1 HITP请求2 HIPAX3 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670485656792-33e22b05-1649-4b64-95ed-0c204a177966.png)

6. 如果把变量中的true改成false 则每个请求都会计数一次，可以看到一共发送了6个请求

<!-- 这是一张图片，ocr 内容为：测试计划 查看结果树 线程组 HTTP请求$L_COUNTER(FALSE.) 查看结果树 名称: 查看结果树 注释: 所有数据写入一个文件 文件名 区分大小写 正则表达式 重置 查找 查找: 取样器结果请求响应数据 TEXT HTTP请求1 RESPONSE HEADERS RESPONSE BODY HTTP请求2 HTTP请求3 HTTP请求4 HTTP请求5 HTTP请求6 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670485722236-79504222-f1d9-45f1-b88b-d3327a8402ff.png)

## 5. 随机数函数
1. 复制出一个请求。然后将禁用上一个计数器函数的请求

<!-- 这是一张图片，ocr 内容为：文件 选项 查找 运行 加盟 . 省明 测试计划 HTTP请求 线程组 HTTP请求${ COUNTER(FALSE,) 包容. HTTP请求$L_COUNTER(FALSE,) 添加 HTTP请求$L_COUNTER(FALSE,) 插入上级 查看结果树 剪切 CTRL-X 高级 复制 CTRL-C 粘贴 CTRL-VB服务器 复写 CTRL+SHIFT-C 服务器名称或IP: HTTPS WWW.BAIDU.COM 删除 DELETE CP请求 打开.. 路径: 合并 选中部分保存为.. 使用 KEEPALIVE 跟随重定向 自动重定向 对POST使用MULTIPART/FORM-DATA 与浏览器养容的头 保存为测试片段 消息体数据 文件上传 保存节点为图片 CTRL-G 同请求 保存屏幕为图片 CTRL+SHIFT-G 名称: 启用 禁用 切换 CTRL-T 帮助 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670486059846-5ff7f61e-e0a5-47a0-8d38-6f7377a04b0d.png)

2. 选择随机数函数

<!-- 这是一张图片，ocr 内容为：测试计划 HTTP请求 :线程组 HTTP请求$( COUNTER(FALSE.] HTTP请求S[_COUNTER(FALSE.) 名称: HTTP请求筑_COUNTER(FALSE.] 注样: 查看结果树 基本高级 WEB服务器 端口 服务器名称或IP: 协议: HTTPS WWW.BAIDU.COM HTIP请求 承数助手 GET 帮助 RANDOM 自助重定 参数消息体 函数参数 名称: 一个范围内的最小值 一个范围内允许的最大值 10 重冒变量 拷贝并粘贴画数字符串 RANDOM(L,10.10.11 生成 THE RESUIT OF THE FUNCTION IS 当前JMELER变量 START.MS 1670478443908 23456 TESTSTART.MS 1670485714501 JMETERTHREAD.LAST SAMPLE OK TRUE TARTHMS13 START.YMD -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670486123083-57e58fe8-c4f4-4fe3-af4d-5ff7b0dce627.png)

3. 在参数位置使用随机函数可以生成随机数

<!-- 这是一张图片，ocr 内容为：创式计划 HTTP请求 HTTP信息头管理器 线程组 名称: HTTP请求制_RANDOM(1.10.1 HTTP请求S(_COUNTER(FALSE. 注释: HTTP请求S( RANDOML1,10.M 食看结果网 基本高级 WEH服务器 服务器名称成(P; LOCALHOST HTTP HTTP请求 路径: 卫生平幻幻?神月KEEPALIVE HTTPSTRMUPART/TORM-DAT 自动年字内 与多流器美容的头 多数消息体数据文件上传 1日 "${_RANDOM(1,10,))]" "AGE" -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670486467740-4e9cfc5e-0fc5-4127-99c1-499dd4b8fbac.png)

4. 运行结果

<!-- 这是一张图片，ocr 内容为：测试计划 查看结果树 HTTP信息头管理器 总线程组 查看结果村 名称: HTTP请求SI COUNTERTFALSE.丹 注任: HTTP清末S_RANDOMI1.10月 香看结果料 所有数据写入一个文件 仅错误目惠 是示日志内容; 文件名 区分大小三 正则大达式 查找 业堂 查找: 取样器结果 请求 响应数据 HTTP请求3 REQUEST HEADERS REQUEST BODY HITP请求1 HTTP请求10 POST HTTP://LOCALHOST:8080/USER HITP请求? HTTP 请求3 3POST  DATA: HTTP请求5 4日  "NAME";"HELLO" 5 "ABE" 46828 INO COOKIESL -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670486515902-d4dc21f5-b996-45f9-aafb-6eb96d0c9c8c.png)

## 6. 时间函数
1. 再复制一个请求，禁用调其他两个

<!-- 这是一张图片，ocr 内容为：测试计划 HTTP请求 HTTP信息头管理器 线程组 名称: 测试TIME HTTP请求$[_COUNTER(FALSE.) 注程: HTTP请求$L_RANDOM(1,10.)] 测试TIME 基木高级 查看结果树 WEB服务器 服务器名称或IP: LOCALHOST 协议: HTTP HTTP请求 路径: POST LUSER 对POST使用MULTIPART/FORM-DATA 与浏览器兼容的头 自动重定向 跟随重定向 使用KEEPALIVE 参数 消息体数据文件上传 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670486798306-ca55cfad-e8f3-41ea-b74a-485f4fd3ff8b.png)

2. 选择time函数

<!-- 这是一张图片，ocr 内容为：文件编辑查找运行运项 四 测试计划 HTTP请求 HITP信息头管理器 线程组 名称: 测试TIME HITP请求5L COUNTERLFALSE.LL 注释: HTTP请求S(_RANDOM(1,10.] 测试TIME 基木高级 香看结果树 WEB服务器 服务器名称或IP: 出口号: LOCALHOST 协议: HTTP HTTP请求 X 函数助手 POST 安徽请具体 函数参数 名称: 这个可以写日期的格式,如果啥都不写.默认就是时间戳 IF FORMAT STRING FOR SIMPLEDATEFOIMAT (OPTIONAL) YYY MM DD HHEMM:SS 存储结果的变量名(可选) 生成 拷贝开转贴函数字符申 $L_TIMELYYY MM DD HHIMMESS.J) 2822-12-88 16:05:46 THE RESULT OF THE FUNCTION IS START.MS-1670478443908 当前JMETER变量 TESTSTART.MS 1678486406531 1156 UMETERIHREAD.LAST SAMPLE OK-TRUE START.HMS-134723 START.YMD-20221208 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670486789475-23d59dd9-b6bc-46e6-8f1f-c1064368c0c8.png)

3. 运行可以看到每个请求后面都带了时间，

<!-- 这是一张图片，ocr 内容为：测试计划 查看结果树 HIIP信息头管理器 线程组 名称: 查看结果村 HTTPAXSI COUNTERIFALSE.LL 注释: HTTP请求ST RANDOML1 测试 LIS( TIMELYYYY-MM-DD HHERNESS)> 所有数据写入一个文件 查看结果树 显示日志内容: 浏览. 文件名 区分大小与 正则表达式 吉找 中肯 查找: 取样器结果 响应数据 请求 BOBODD 兰兰型兰兰兰 12022-12-08 16:07:27 REQUEST BODY REQUEST HEADERS 12022-12-08 16:07:27 12022-12-08160727 62022-12-08 16:07:28 L2022-12-08  16:07:28 2022-12-08 16:07:28 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670486886937-bb11f82d-2a6e-43f6-a714-2507c73d5d12.png)

4. 当然我们也可以用这个放在请求参数的位置，来作为请求参数，这里就不演示了

# 10.JDBC请求
我们可以创建一个jdbc请求来查询数据库

1. 在测试计划里面设置我们的mysql驱动包

<!-- 这是一张图片，ocr 内容为：工具帮助 选项 文件 运行 编辑 查我 团 00:00:08 测试计划 测试计划 HTTP信息头管理器 线程组 测试计划 名称: 测试 注释: 查看结果树 用户定义的变量 名称: 详细 刑除 添加 向下 从剪贴板添加 向上 独立运行每个线程组(例如在一个组运行结束后启动下一个) 主线程结束后运行TEARDOWN线程组 函数测试模式 只有当你需要记录每个污染从服务器取待的致损到文件时才驾要选择西数测试模式.选择这个选项很影响性能. 刑除 添加目录或JAR包到CLASSPATH 清除 浏览... E\SOFTLMAVENVENVEPOSITORYSQA-8.0.15JAR -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670487959943-5baf0cef-8730-420e-8b93-c0ff4b268ff8.png)

2. 添加jdbc配置元件

<!-- 这是一张图片，ocr 内容为：测试计划 线程(用户) 添加 CSV数据文件设置 配置元件 粘贴 CTRL-V 监听器 HTTP信息头管理器 打开.. HTTP COOKIE管理器 定时器 合并 HTTP缓存管理器 选中部分保存为.. 前置处理器 HTTP请求默认值 后置处理器 保存节点为图片 CTRL-G BOLT CONNECTION CONFIGURATION 断言 保存屏幕为图片 CTRL+SHIFT-G DNS缓存管理器 测试片段 启用 FTP默认请求 非测试元件 禁用 HTTP授权管理器 切换 CTRL-T JDBC CONNECTION CONFIGURATION 帮助 JDVD款明水 LDAP扩展请求默认值 LDAP默认请求 TCP取样器配置 密钥库配置 用户定义的变量 登陆配置元件/素 简单配置元件 计数器 随机变量 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670488148642-1b2f15e4-67d1-4181-a6e3-807958bb4898.png)

3. 配置如下：起个名字和配置一下连接信息

<!-- 这是一张图片，ocr 内容为：文件演辑查找运行连项工具 002 00.00.08 测试计划 JDBC CONNECTION CONFIQURATION HITP信息头管理器 JDBC CONNECTION CONFIGURATION 名称: :线程组 正者结果村 VARIABLE NAME BOUND TE POOL WARIABLE NAME FOR GEALED POOK MYPOOL CONNECTION POOL CONFIGURATION MAX NUMBER OF CONNECTIONS: O MAX WAIT IMST: 10000 TIME BETEVEEN EVICTION RUNS INSIS 60000 AUTO COMMIT TRUE TRANSACTION ISOLATION: DEFAULT POOL PREPARED STATEMENTS: 1 HEINTFONT FALSE TRIL SOU SLAFEMENLS SERBARALED BY NEYE BRE BRE CONNECTION VALIDATION BY POOL TEST WHILE LDLE TRUE SOFT MIN EVICTABLE IDLE TIMEIMSL: 5000 DETABASE CONNECTION IDBCMYSGB/192 168.10.1013306/DB_USER DETABASE URL: JDBC DIVER DOSS COM.MYSAL.IDBC.DRIVER ROOT -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670488326220-921ea303-3435-4ff3-950e-7a77f184baa0.png)

4. 在线程组下面添加一个Jdbc请求

<!-- 这是一张图片，ocr 内容为：测试计划 线程组 HTTP信息头管理器 线程组 HTTP请求 取样器 添加 查看结 测试活动 逻辑控制器 为子线程添加响应时间 调试取样器 启动 前置处理器 JSR223 SAMPLER 不停顿启动 后置处理器 AP/1.3取样器 停止线程 程循环 验证 断言 ACCESS LOG SAMPLER 剪切 CTRL-X 定时器 BEANSHELL取样器 复制 CTRL-C 测试片段 BOLT REQUEST 粘贴 CTRL-V 配置元件 FTP请求 复写 CTRL+SHIFT-C 监听器 GRAPHQL HTTP REQUEST 删除 DELETE JDBC REQUEST 打开. JMS发布 合并 JMS点到点 ATION 选中部分保存为.. JMS订阅 保存节点为图片 CTRL-G JUNIT请求 CTRL+SHIFT-G 保存屏幕为图片 JAVA请求 启用 LDAP扩展请求默认值 禁用 LDAP请求 切换 CTRL-T OS进程取样器 帮助 SMTP取样器 TCP取样器 邮件阅读者取样器 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670488035558-a7438a9e-1bf3-48ba-9564-879f16a5fb0f.png)

<!-- 这是一张图片，ocr 内容为：销辑 查找 运行 选项 工具 002 00:00:08 树试计划 JDBC REQUEST MIP信总头管理器 IDBCCONNECLION CONFIGUTALION JDBC REAJEST 名称: 晚程组 注释: JDBC REQUEST VARIABLE NAME BOUND TO POOL 直看结果网 写上上一步数据库连接配置的名字 VORIOBLE NOME OF POOL DEDARED IN JDBC CONNECION CONFIGURATIONC MYFOOL SUSNONAY 查询语言 CORRES PES SERED SRED SHED OUERY 写上你的SOL ABC PARAMETER VALUES: PORAMETER TYPES: 起一个USERNAME变量名后面会用 VARIBBLE NAMES:USERNAME RESULT VARIABLE NAME: QUERY TIMEOUT ISK LIMIT RESULISET HENDLE RESULLSEL: SLERE AS SHING -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670488536392-a695ba09-dd47-442c-a28b-c1ebe3643e4b.png)

5. 执行可以看到结果

<!-- 这是一张图片，ocr 内容为：测试计划 查看结果树 HTTP信息头管理器 JDBC CONNECTION CONFIGURATION 名称: 直看结果树 线程组 注程: JDBC REQUEST 查看结果树 所有数据写入一个文件 文件名 正则表达式 区分大小写 查找: 查找 重置 取样器结果请求响应数据 TEXT JDBC REQUEST RESPONSE BODY RESPONSE HEADERS UNAME 强哥 法外狂贴 张三 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670488766179-85c95924-40a5-4ade-b738-65486ee01040.png)

6. 再新建一个调式取样器

<!-- 这是一张图片，ocr 内容为：线程 HTTP请求 取样器 添加 测试活动 逻辑控制器 为子线程添加响应时间 力作 调试取样器 启动 前置处理器 查看 停止线程 JSR223 SAMPLER 生程循环 不停顿启动 后置处理器 AJP/1.3取样器 验证 断言 ACCESS LOG SAMPLER 剪切 CTRL-X 定时器 BEANSHELL取样器 复制 CTRL-C 测试片段 BOLT REQUEST 粘贴 CTRL-V 配置元件 1 FTP请求 复写 CTRL+SHIFT-C 监听器 GRAPHQL HTTP REQUEST 删除 DELETE 1 JDBC REQUEST 打开.. TERATION JMS发布 合并 IMS占到点 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670489158524-24688dce-8496-4ef4-9bdb-ce9a85a181bb.png)

7. 再次执行，我们就能看到之前的userName变量的用处了

<!-- 这是一张图片，ocr 内容为：甘戈运行 十 测试计划 查看结果树 HTIP信息头管理器 IDBC CONNEDION CONFIGURATION 查看结果树 名称: 线程组 注样: JDBC REQUEST 调试取样器 所有数据写入一个文件 查看结果村 测览 显示日志内容; 文件名 区分大小与 查探: 平肯 查找 取样器结果 请求响应数据 TEXL IDBC REQUEST RESPONSE BODY RESPONSE HEACERS 调试取样器 JMOTERVARIABLES: JMETERTHREAD.LAST SAMPLE OK-TRUE IMELERTHREAD.PACK-ORQAPACHEJINETER,THREADS SAMPLEPACKAGE@12813FED STARLHMS-134723 STARIMS-1670478443908 STARLYMD-20221208 TESTSTART.MS-1670489204744 JM 线程组 IDK-0 JMYSAMEUSER-TRUE HRYPOOL-ORQ APACHEJMETERPROTOCOLJDBCCONTIG.DATASOURCELLEMENTSDETASOURCECOMPONENTHMPLO0181818184F41 USENAME 1-强哥 USERNAME_2法外狂赌 USERNAME.3-张 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670489267398-a688020a-a979-403d-8625-74e15a478a54.png)

8. 我们可以拿到上一步的userName_xxx变量，请求百度搜索

<!-- 这是一张图片，ocr 内容为：月. 试计划 HTTP请求 HTTP后A头管理器 JDBC CONNECTION CONFIGURATION 名称 请求百度 线程组 注得 JDBC REQUEST 调试职样器 基本高级 请求白度 奇者结果树 VW上服务器 80 服务器名称成IP 出口号 协议:HTP 内容缩码 路径:SHWD-SLUSERNAME.11 与创览器卷的头 白动业定可 钱码重定可 V 座王KEEPALIX  XPOST FORATINAT LORMDELA 枣乱 消足体数据 文件上传 同请求一起发送参数: 内省类型 位 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670489490806-fc6199e5-225f-42ae-ae5d-3ccd7b6e9363.png)

<!-- 这是一张图片，ocr 内容为：测试计划 查看结果树 HTTP信思头管理器 IDBC CONNECTION CONFIGURATION 名称:  食看结果树 钱程组 注样: JDBC REQUEST 周试取样署 所有数据写入一个文件 请求百度 仅错 文件名 显示日志内容: 查看结果村 区分大小写口 正则表达式 查找 王霞 森找: 取样器结果请求响应数据 TEXT JDBC REQUEST RESPONSE BODY RESPONSE HEADERS 调试取样器 请求白度 BODYTH.TD.P1.P2(FONT-FAMILYCARIAL] P,FOUM,OL,LI,DL,DL,DD,H3FRNARGIN:O;PADDING:O-LISTYLENONE) TABLE,IMG(BORDER:OF TOFFONT-SIZE:9PT LINE-HEIGHT:18PX ERNFLONT-STYLENOMMALEOLORSFFCOO CITEFFONT STYLENORMALEOLORGREEN] M.A.MLCOLOR:666) A.MEVISITEDJCOLOR.11606 G.A.GLCOLONGREEN F14LFONT-SIZE:14PXD 110FFONT-SIZE:10.5P0 F16FFONT-SIZE:16PX 113(LONL-SIZE:13PX) FU,THEAD,FTOOL,FSEARCH,HFOOTFFONT-SIZE 12PX) LOGOFWIDTH:117PXHEIGHT:38PXCURSORPOINTERL P1FINE HEIGHT 120% MARG N-LEFT.12PT 中26WLDTH:100% INE-HEIGHT120%MARMARGIN-LEFT-12P号 WRAPPER[ZOOM:1] HRENTAINERTUARD-HEEAKSHREAK-ALFWERD-WTAN-BREAK-WERDENESSITIONERELSTHCEL -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670489508988-8715d302-69a7-40f0-87ad-9ed17fb0ac80.png)

# 11. 断言
## 1. 响应断言
用来判断响应内容和响应状态码

1. 对请求添加一个响应断言

<!-- 这是一张图片，ocr 内容为：测试计划 HTTP请求 线程组 十年 HTTP 响应断言 断言 添加 响 JSON断言 插入上级 定时器 查看结果 大小断言 剪切 前置处理器 CTRL-X JSR223断言 复制 CTRL-C 后置处理器  XPATH2 ASSERTION 粘贴 CTRL-V 配置元件 HTML断言 复写 CTRL+SHIFT-C 监听器 JSON JMESPATH ASSERTION 删除 DELETE MD5HEX断言 打开.. SMIME断言 合并 XMLSCHEMA断言 选中部分保存为.. 向 XML断言 保存为测试片段 XPATH断言 保存节点为图片 CTRL-G 断言持续时间 保存屏幕为图片 CTRL+SHIFT-G 比较断言 启用 BEANSHELL断言 禁用 切换 CTRL-T 帮助 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670490825291-2f162669-b076-4ffb-adc3-4dccc3e44b6d.png)

2. 对断言进行设置

<!-- 这是一张图片，ocr 内容为：响应断言 名称: 响应断言 注释: APPLY TO: JMETER VARIABLE NAME TO USE MAIN SAMPLE AND SUB-SAMPLES SUB-SAMPLES ONLY MAIN SAMPLE ONLY 测试字段 响应信息 响应文本 响应代码 响应头 文档(文本) URL样本 请求头 忽略状态 请求数据 模式压配规则 否 或者 包括 相等 字符串 匹配 测试模式 测试模式 要求请求结果里面包含张三 张三 从剪贴板添加 删除 添加 自定义失败消息 请求结果里面没有张 断言不通过的时候,自定义失败消息 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670490919592-fabc35ee-65f5-4fcf-bf43-28f2e5369861.png)

3. 由于响应体里面包含张三所有没有报错

<!-- 这是一张图片，ocr 内容为：测试计划 查看结果树 1线招组 HITP请求 名称: 查看结果树 响应断言 注释: 查看结果树 所有数据写入一个文件 文件名 正则表达式 区分大小写 重置 查找 查找: 取样器结果 请求 响应数据 TEXT HTTP请求 RESPONSE BODY RESPONSE HEADERS ("NAME"张三",AGE":18] -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670491088482-5a9f61b8-e5e6-4cf4-9bd9-4822e6de2245.png)

4. 我们改一下条件让响应体里面包含李四，再次执行，可以看到断言失败

<!-- 这是一张图片，ocr 内容为：测试计划 查看结果树 线程组 HTTP请求 名称: 查看结果村 响应断言 注释: 查看结果树 所有数据写入一个文件 文件名 正则表达式 区分大小写 查找 重置 查找: ASSERTION RESULT TEXT HTTP请求 ASSERTION ERRORFALSE 响应断言 ASSERTION FAILUREITRUE ASSERTION FALUREMESSAGE:请求结果里面没有李四 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670491166933-cf8f35d0-4958-4ce9-8e4f-89b450b53119.png)

对于其他的响应类型可以自行尝试

<!-- 这是一张图片，ocr 内容为：测试字股 响应信息 响应代码 响应头 响应文不 请求头 文档(文本) 忽略状态 URL样本 请求数据 模式匹配规则 包括匹配相等符串串或者 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670491211085-2e164443-de9f-4518-a50c-b3e91be31ef2.png)

## 2. 大小断言
判断响应内容的字节长度

1. 新增一个大小断言

<!-- 这是一张图片，ocr 内容为：测试计划 HTTP请求 线程组 HTTP请求 响应断言 断言 添加 响应 JSON断言 插入上级 定时器 查看结果树 大小断言 剪切 CTRL-X 前置处理器 复制 CTRL-C 后置处理器 XPATH2 ASSERTION 粘贴 CTRL-V 配置元件 HTML断言 复写 CTRL+SHIFT-C 监听器 JSON JMESPATH ASSERTION 删除 DELETE MD5HEX断言 打开. SMIME断言 /USERS 合并 XMLSCHEMA断言 选中部分保存为.. 使用KEEP XML断言 保存为测试片段 XPATH断言 保存节点为图片 CTRL-G 断言持续时间 保存屏幕为图片 CTRL+SHIFT-G 比较断言 名称: 启用 BEANSHELL断言 禁用 切换 CTRF-T 帮助 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670491441422-306b7385-328d-431c-b9aa-de977abbe04a.png)

<!-- 这是一张图片，ocr 内容为：00:00:02 大小断言 名称: 大小断言 注释: APPLY TO: SUB-SAMPLES ONLY JMETER VARIABLE NAME TO USE MAIN SAMPLE ONLY MAIN SAMPLE AND SUB-SAMPLES 响应字段大小 响应的消息体 响应代码 完整响应 响应信息 响应头 SIZE TO ASSERT 比较类型 ()   )  )  () () () )  )  ) 三 O ! 字节大小: 1000 配置完整响应要大于1000字节 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670491482847-b1db7a6d-4426-4f7a-8ec4-9a893f49e760.png)

## 3. 断言持续时间
判断响应时间

1. 新增断言持续时间

<!-- 这是一张图片，ocr 内容为：线程组 HTTPI 响应断言 断言 添加 响大断 JSON断言 插入上级 定时器 大小断言 剪切 前置处理器 CTRL-X JSR223断言 复制 CTRL-C 后置处理器 查看结果 XPATH2 ASSERTION 粘贴 CTRL-V 配置元件 HTML断言 复写 CTRL+SHIFT-C 监听器 JSON JMESPATH ASSERTION 删除 DELETE MD5HEX断言 打开.. SMIME断言 /USE 合并 XMLSCHEMA断言 选中部分保存为.. XML断言 保存为测试片段 XPATH断言 保存节点为图片 CTRL-G 断言持续时间 保存屏幕为图片 CTRL+SHIFT-G 比较断言 名称: 启用 BEANSHELL断言 禁用 柯格 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670491520152-21bb0b11-67be-4b0b-b5b5-c541c3074cf3.png)

<!-- 这是一张图片，ocr 内容为：断言持续时间 名称: 断言持续时间 注样: APPLY TO: SUB-SAMPLES ONLY MAIN SAMPLE AND SUB-SAMPLES MAIN SAMPLE ONLY 断言持续时间 持续时间(毫秒): 配置这个请求不应该超过1MS. 这个对于接口时长要求还是很有用的. -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670491666125-9f935a4b-2df8-4d7c-8024-ba17c5241d1a.png)

2. 执行报错，因为接口超过了1ms

<!-- 这是一张图片，ocr 内容为：测试计划 查看结果树 线程组 HTTP请求 查看结果树 名称; 响应断言 注释: 大小断言 断言持续时间 所有数据写入一个文件 查看结果树 文件名 区分大小写 正则表达式 查找: 重冒 查找 TEXT ASSERTION RESUTT HTTP请求  ASSERTION ERRORFALSE 新言持续时间 ASSERTION FAILURETRUE ASSERTIONFAILUREMESSAGE;操作持续太长时间:他花费了3毫秒,但不应该超过1毫秒. -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670491709333-34a52f34-bb3f-4106-a9f8-dbbdac5fc56e.png)


# 12.参考

参考视频：https://www.bilibili.com/video/BV1ty4y1q72g
