# 1. 逻辑控制器
## 1. if逻辑控制器
要求：如果用户是张三我们就发送一个百度请求，否则就不发送。

1. 新增一个用户变量 userName

<!-- 这是一张图片，ocr 内容为：测试计划 测试计划 线程(用户) 添加 用户定 配置元件 CSV数据文件设置 线程组 粘贴 CTRL-V 监听器 HTTP信息头管理器 查看结: 打开.. HTTPCOOKIE管理器 定时器 合并 HTTP缓存管理器 选中部分保存为.. 前置处理器 HTTP请求默认值 后置处理器 保存节点为图片 CTRL-G BOLT CONNECTION CONFIGURATION 断言 保存屏幕为图片 CTRL+SHIFT-G DNS缓存管理器 测试片段 启用 FTP默认请求 非测试元件 禁用 HTTP授权管理器 切换 CTRL-T JDBC CONNECTION CONFIGURATION 帮助 JAVA默认请求 LDAP扩展请求默认值 LDAP默认请求 TCP取样器配置 密钥库配置 用户定义的变量 登陆配置元件/素 简单配置元件 计数器 随机变量 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670492245743-02722639-6b82-4fa7-a51a-ed62fe5642f6.png)

<!-- 这是一张图片，ocr 内容为：用户定义的变量 名称: 用户定义的变量 注释: 用户定义的变量 值 描述 名称: 张三 USERNAME -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670492253933-75b107c5-9503-43da-90bc-68cfe026c437.png)

2. 新增一个逻辑控制器 > if控制器

<!-- 这是一张图片，ocr 内容为：用户定义的变量 线程组 线程组 名称: 取样器 添加 逻辑控制器 IF控制器 为子线程添加响应时间 查看 事务控制器 启动 前置处理器 执行的动作 循环控制器 不停顿启动 后置处理器 停止测试 动下一进程循环 立即停止测 停止线程 WHILE控制器 验证 断言 FOREACH控制器 剪切 CTRL-X 定时器 INCLUDE控制器 复制 CTR-C 测试片段 RUNTIME控制器 粘贴 CTRL-V 配置元件 临界部分控制器 复写 CTRL+SHIFT-C 少): 监听器 交替控制器 删除 DELETE 仅一次控制器 永远 打开... 录制控制器 合并 N EACH ITERATION 简单控制器 选中部分保存为.. 随机控制器 呈直到需要 CTRL-G 保存节点为图片 随机顺序控制器 保存屏幕为图片 CTRL+SHIFT-G 吞吐量控制器 启用 SWITCH控制器 禁用 模块控制器 切换 CTRL-T 启动延迟(秒) 帮助 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670492296283-37688055-8fd8-4a69-aadb-86ad339b5546.png)

<!-- 这是一张图片，ocr 内容为：0/1 00:00:02 IF控制器 IF控制器 名称: 注释: FOR PERFORMANCE IT IS ADVISED TO CHECK "INTERPRET CONDITION AS VARLABLE EXDRESSLON"  AND USE -JEXI3 OR - GROOVY EVALUATING TRUE OR FALSE OR A VARIABLE THAT CONTAINS TRUE OR FALSE- FUSERNAME,"张三"张三" 条件 如果USERNAME是张三就往下面执行 $UMETERTHREADLAST SAMPLE OKY CAN BE USED TO TEST IF LAST SAMPLER WAS SUCCESSFUL USE STATUS OF LAST SAMPLE 把这个勾去掉 INTERPRET CONDITION AS VARIABLE EXPRESSION? EVALUATE FOR ALL CHILDREN? -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670492390963-b8289364-a792-4d03-b940-91e08a4cc389.png)

3. 把http请求拖到if控制器下面，让if控制器成为Http请求的父亲

<!-- 这是一张图片，ocr 内容为：测试计划 HTTP请求 用户定义的变量 专线招组 HIP请求 名称: 手控制器 HTTP请求 青春结果树 基木高级 WEB服务器 端口号: 服务器名称或IP: WWW.BAIDU.COM 协议: HTTP者求 GET 路径: 内容编码: 自动正定有?烟限更完自?实用KEEPAIVE HPOST使用MULLIPART/FOM-DATA 多数消思体数据文件上传 同请求一起发送多数: 包 值 内容类型 编码? -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670492462082-4f76284c-1c40-4d07-bb3b-a3ac2efb4e05.png)

4. 执行发现条件满足，请求成功发送

<!-- 这是一张图片，ocr 内容为：测试计划 查看结果树 用户定义的变量 线程组 名称: 查看结果树 IF控制器 注释: HTTP请求 查看结果树 所有数据写入一个文件 文件名 区分大小写 正则表达式 查找 重置 查找: 请求响应数据 取样器结果 TEXT HTTP请求 THREAD NAME:线程组 1-1 SAMPLE START:2022-12-08 17:41:39 CST LOAD TIME:27 CONNECT TIME:13 LATENGY:27 SIZE IN BYTES:2497 SENT BYTES:115 HEADERS SIZE IN BYTES:116 BODY SIZE IN BYTES:2381 SAMPLE COUNT:1 ERROR COUNT:O DATA TYPE('TEXT"BIN"BIN"):TEXT RESPONSE CODE:200 RESPONSE MESSAGE:OK HTTPSAMPLERESULT FIELDS: CONTENTTYPE:TEXT/HTML DATAENCODING:NULL -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670492517181-391fc82d-d86b-441b-9f69-f2f654608c36.png)

5. 改变条件。再次运行。我们可以发现条件不满足，请求不会发送

<!-- 这是一张图片，ocr 内容为：IF控制器 名称:IF控制器 注释: S ADVISED TO CHECK "INTERPRET COR CONDITION AS VARIABLE EXPRESSION" FOR PERFORMANCE IT IS AO OR A VARIABLE THAT CONTAINS TRUE OR FALSE. AND USE _JEXL3 OR. FALSE _GROOVY EVALUATING TO TRUE OR FIL 张三11111 条件 USERNAME -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670492573454-6f923793-ec88-4378-b756-f2adab3f0d76.png)

## 2. foreach控制器
要求：有一组关关键字，需要循环取出然后在用百度搜索

1. 先创建出如下结构

<!-- 这是一张图片，ocr 内容为：测试计划 FOREACH控制器 用户定义的变量 线程组 名称: FOREACH控制器 FOREACH控制器 注释: HITP请求 查看结果树 输入变量前缓 开始循环字段(不包含) 结束循环宇段(含) 输出变量名称 数字之前加上下划线"? -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670492958980-fcfd4a9d-28ed-461c-85ca-f605d4169909.png)

2. 创建3个有规律的变量名

<!-- 这是一张图片，ocr 内容为：文化 吉找运行连项工具帮助 00:00:02 测试计划 用户定义的变量 户定文的变量 线程组 用户定义的变量 名称: FOREACH控节(器 注释: 青春结果行 用户定义的变量 名称: 值 措注 NOME 2 NAMA.3 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670493020044-ce42ba6b-b781-4153-9756-0c02cdbaa8dc.png)

3. 设置foreach控制器

<!-- 这是一张图片，ocr 内容为：有我运行选项 工具帮助 中国+一个 00 00:00:02 加试计划 FOREACH控制器 用户定文的变量 线程组 FOREACH控制器 名称: FOREACH空制器 注释: HTTP请求 我们定义的变量名都是NAME开头 五省结果树 输入变量前缓 不包含0.即NAME.1开始 开始静环宁度(不包含) 包含3 NAME.3结束 结束静环宇段(含) 3 输出的变量名.作为HTTP请求的变量 输出变量名称 NAMERESULT 我们的变量名都有下划线 数字之前加上下划线"? -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670493222097-bfb5c5d7-083a-43e2-a999-550d947f29af.png)

4. 设置http请求并运行

<!-- 这是一张图片，ocr 内容为：刘试计划 HTTP请求 用户定义的变量 一线程组 HTTP请求 名称; IFORFACH控制器 注意: 金春结果树 WEB服务器 服务著名称或IP. 端口号: WWW.BAIDU.COM 协议:HTTP HIP请求 GET 路径: 内容编码: TSWD-SINAMELTESULLY SROST注册MULTPANT/FORM DATA 与创质量真容的头 白动重黑肉 网所集定间 北开KOOPALIVO 步数 消品体数据文件上传 同暗求一起发送参数: 值 编码7 名称: 内容类型 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670493278741-ee53ed86-bca6-4212-a19a-ceed08851583.png)

# 2. 接口关联
比如接口2的参数需要接口1的结果作为查询条件

要求：获取 www.jd.com 网页的title字段作为www.baidu.com的查询条件

1. 新建一个Http请求

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1769753829505-eade7db3-0da2-4d70-880f-1a197d830fce.png)

2. 在请求下面新建一个Xpath提取器。用来提取网页中的title

<!-- 这是一张图片，ocr 内容为：测试计划 HTTP请求 用户定义的变量 线程组 HTTP请求 名称: 断言 添加 注释: 查看 插入上级 定时器 剪切 CTRL-X 基本高级 前置处理器 复制 CSS/JQUERY提取器 CTRL-C 后置处理器 粘贴 CTRL-V JSON JMESPATH EXTRACTOR 配置元件 服 复写 CTRL+SHIFT-C JSON提取器 监听器 正则表达式提取器 删除 DELETE 边界提取器 路径: 打开... JSR223后置处理程序 合并 重定向 使用KEEPALIVE 选中部分保存为.. JDBC后置处理程序 上传 保存为测试片段 XPATH2 EXTRACTOR XPATH提取器 保存节点为图片 CTRL-G 结果状态处理器 保存屏幕为图片 CTRL+SHIFT-G 名称: 调试后置处理程序 启用 BEANSHELL后置处理程序 禁用 切换 CTRL-T 帮助 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670495121852-6c886524-3991-44b5-b878-cc2f04522495.png)

<!-- 这是一张图片，ocr 内容为：测试计划 XPATH提取器 用户定义的变量 线程组 名称: XPATH提取带 汁料: XPALH提取器 APPLY TO 直行结果村 SUB-SAMPLES ONLY IMELER WERIABLE NARNE TO USE MAIN SAMPLE AND SUBSAMPLOS MAIN SAMPLA ONLY XML PARSING OPTIONS 宗勾选后,解析HTML数据.不勾选,解析XML数据; 显示勾选 USE TIDY ITOLERANT PARSERJ 聚告异常 VOLIDATE XML USE NAMESPACES FETCH EXTERNAL DTDS LGNORE WHITESPACE RETUM ENTIRE XPATH FRAGMENT INSTEAD OF LED CONTENT? 引用名称: 变量名 XPATH AUERY: 取TITLE LUUE 匹配数字(0代表随机): 取匹配到的第一个TITLE 执行证: 如果没有匹配到,给个默认值 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670495655938-742352ad-682f-43a1-9071-72f256a11dd3.png)

3. 再新建一个请求百度，并引用val变量

<!-- 这是一张图片，ocr 内容为：HTTP请求 用户定文的变量 线程组 名称: HTTP请求 HTTP请求 注样 XPATH提取器 HTTP请求 基本高级 查看结果树 WEB服务器 服务器名称或P: 协议: 出口号: WWW.BAIDU.CCIM HTTP请求 路径: 内容编码: UTF-8 GET (  线苑定河 区  汽天KEPALURE   对POST使用RULTIPAT/FORM-DATA 与负览器养育的头 白动重宝向 参数 文件上传 消息体数据 国请求走发送参教: 编码? 名称: 内容类型 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670496162156-151fe82c-55ce-49f2-99a3-9745c04c91c7.png)

4. 运行之后是可以看到title属性已经取到了

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1769753802160-48308d32-1873-4d25-a02c-d00396880a2b.png)

# 3. Json提取器
上一个我们说了xpath提取器这里我们演示一下json提取器，其他的可以自行尝试



要求： 从接口返回的json中提取所有name值

1. 设置http请求

<!-- 这是一张图片，ocr 内容为：创试计划 HTTP请求 名称 HTTP请求 JSON是政策 注程 调试取样星 百斤结果料 番本高级 WEB服务器 服务器名称或P. 共议:HTTP 8080 出口号: LOCALHOST HTTP请求 内容编码: 路价: GET 国前重定向 区 使月KCONALINE [     月MULTIPAN/FARM-DATA  白动重充向 密数消息体数据文件上传 同请求一起发送参数: 信 油码了 名称 内容类型 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670500285375-64da8d1a-252a-406e-8623-0adb0397f5ad.png)

2. 新增一个json提取器

<!-- 这是一张图片，ocr 内容为：测试计划 HTTP请求 用户定义的变量 线程组 名称: HTTP请求 HTTP违击 断言 添加 注释: JS 插入上级 定时器 HTTP 高级 基本 剪切 CTRL-X 前置处理器 查看结果 复制 CSS/JQUERY提取器 CTRL-C 后置处理器 粘贴 CTRL-V JSON JMESPATH EXTRACTOR 配置元件 服务 复写 CTRL+SHIFT-C JSON提取器 监听器 删除 DELETE 正则表达式提取器 边界提取器 /USERS 打开.. JSR223后置处理程序 合并 它向 使用KEEPALIVE 选中部分保存为.. JDBC后置处理程序 保存为测试片段 XPATH2 EXTRACTOR XPATH提取器 保存节点为图片 CTRL-G 结果状态处理器 保存屏幕为图片 CTRL+SHIFT-G 名称: 调试后置处理程序 启用 BEANSHELL 后置处理程序 禁用 切换 CTRL-T 帮助 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670499973847-e49a7126-d243-453a-b3cf-c19983290c7f.png)

<!-- 这是一张图片，ocr 内容为：JSON提取器 ITTP请求 名称 JSON提取器 JSON提取帮 许裕: 调试取杆器 APPLY TOE 金看结果树 ) MAIN SAMPLE AND SUB-SAMPLES MAN SAMPLE ONLY SUB-SAMPLES ONLY O JMETER VARIABLE NAME TO USE 为提取到的值取个变量名 NEMES OF CRENLED VARIAHLESE MYNAME S.NAME JSONPATH语法.表示取出所有NAME MATCH NO.(O FOR RANDOML: -1代表所有0代表随机,1代表匹配到的第一个值2代表匹配到的第二个值以此类推 COMPUTE CONCATENATIONVAR(SUTFIX ALL)ALL) ALL) 如果没有匹配的值,给个默认值 DEFAULT VALUES -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670500423664-de3aea68-a400-4bfd-a249-c3cd8e26f8a2.png)

2. 添加一个调式取样器

<!-- 这是一张图片，ocr 内容为：测试计划 线程组 用户定义的变量 线程组 HTTP请求 取样器 添加 H 测试活动 为子线程添加响应时间 逻辑控制器 查看结 调试取样器 启动 前置处理器 JSR223 SAMPLER 不停顿启动 后置处理器 程循环 停止线程 AJP/1.3取样器 验证 断言 ACCESS LOG SAMPLER 剪切 CTRL-X 定时器 BEANSHELL取样器 CTRL-C 复制 测试片段 BOLT REQUEST 粘贴 CTRL-V 配置元件 FTP请求 复写 CTRL+SHIFT-C 监听器 GRAPHQL HTTP REQUEST 删除 DELETE JDBC REQUEST 打开.. JMS发布 合并 RATION JMS点到点 选中部分保存为.. JMS订阅 保存节点为图片 CTRL-G JUNIT请求 保存屏幕为图片 CTRL+SHIFT-G JAVA请求 启用 LDAP扩展请求默认值 禁用 LDAP请求 CTRL-T 切换 OS进程取样器 帮助 SMTP取样器 TCP取样器 邮件阅读者取样器 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670500132535-6df748a2-0446-4152-af20-46b9f819fd4c.png)

3. 运行查看接口返回和调式取样器的结果

<!-- 这是一张图片，ocr 内容为：? 测试计划 查看结果树 线程组 HTTP请求 查看结果树 名称: JSON提取器 注释: 调试取样器 所有数据写入一个文件 查看结果树 文件名 区分大小写正则表达式 查找: 查找 重置 响应数据 取样器结果请求响 TEXT HTTP请求 RESPONSE BODY RESPONSE HEADERS 调试取样器 HNAME:张三",AGE":18M"NAME:"李四",AGE.20H -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670500451151-1cf32c7b-3b35-437f-81e1-a697f8a3f73a.png)

<!-- 这是一张图片，ocr 内容为：测试计划 查看结果树 线程组 HTTP请求 名称: 查看结果树 JSON提取器 注样: 调试取样器 所有数据写入一个文件 文件名 正则表达式 查找 区分大小写 查找: 重置 取样器结果请求响应数据 TEXT HTTP请求 RESPONSE BODY RESPONSE HEADERS 调试取样器 JMETERVARIABLES: JMETERTHREAD.LAST SAMPLE OK TRUE JMETERTHREAD,PACK-ORG.APACHEJMETER.THREADS,SAMPLEPACKAGE@5D67FEA START.HMS134723  STARLMS1670478443908 START.YMD20221208 TESTSTART.MS-1670500430845 JM 线程组 IDX-0 _JMETER.UT 线程组1-1 MYNAME_1 MYNAME_2李四 MYNARNE.MATCHNR-2 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670500461515-df564d98-f78e-4cc0-bfbe-5f24c5fc0536.png)

# 4. 跨越线程组传值
我的定义的变量一般只能在当前线程组中使用。不同线程组之间变量无法引用。那我们要怎么解决？

还是上面从<font style="color:rgb(38, 38, 38);">获取</font>www.jd.com<font style="color:rgb(38, 38, 38);">网页的title字段作为www.baidu.com的查询条件的例子。</font>

<font style="color:rgb(38, 38, 38);">现在我们把www.jd.com</font>的请求和www.baidu.com的请求放在不同线程组

1. 构建请求

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1769754807001-b7506e55-aae4-4c1b-aa63-f55013f2a9df.png)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1769754780455-a0e6e3d9-ae07-4733-b3ea-c6c4758c3001.png)

2. 使用函数助手给全局设置一个变量

<!-- 这是一张图片，ocr 内容为：十 测试计划 线程组 一线程组 请求WWWITEAST.CN 线程组 名称: XPATH是取器 注样: 线程组 二百度请求 在取样器错误后要执行的动作 查看结果树 立即停止测式 停止线程 启动下 进程循环 停止测试 线程属性 线程数: X 函数助手 RAMP-UO时 古助 SEFPROPERTY 循环次数 函数参数 SAME  名称; 给变量起个名字 延迟创属性名称 GVAL 值要引用哪个变量 VALUE OF PROPERTY SLVALL 调度器 持续时间( 生成 拷贝并粘贴函数字符串 重要交量 L.SETPROPERTY(GVAL,SLVALL.) 启动延迟 THE RESULT OF THE FUNCTION IS START.MS-1670478443908 当前JMETER变量 TESTSTART.MS-1670500430845 DMETERTHREAD.LAST SAMPLE OK TRUE START.HMS-134723 START.YMD-20221208 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670501383643-8a8ed8c1-0fff-410e-a605-dcc88baae24c.png)

<!-- 这是一张图片，ocr 内容为：测试计划 线程细 线程 HTTP请求 取样器 添加 测试活动 逻辑控制器 为子线程添加响应时间 (线程 调试取样器 启动 前置处理器 JSR223 SAMPLER 不停顿启动 后置处理器 查看 动作 AJP/1.3取样器 验证 断言 停止线程 停止测试 进程循环 立即停止测试 ACCESS LOG SAMPLER 剪切 CTRL-X 定时器 BEANSHELL取样器 复制 CTRL-C 测试片段 BOLT REQUEST 粘贴 CTRL-V 配置元件 FTP请求 复写 CTRL+SHIFT-C 监听器 GRAPHQL HTTP REQUEST 删除 DELETE JDBC REQUEST 打开.. JMS发布 合并 JMS点到点 选中部分保存为.. TERATION JMS订阅 保存节点为图片 CTRL-G JUNIT请求 要 保存屏幕为图片 CTRL+SHIFT-G JAVA请求 启用 LDAP扩展请求默认值 禁用 LDAP请求 CTRL-T 切换 OS进程取样器 帮助 SMTP取样器 TCP取样器 邮件阅读者取样器 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670501429005-2eb3656a-947e-452d-9138-a333af5082b1.png)

<!-- 这是一张图片，ocr 内容为：测试计划 BEANSHELL取样器 线程组 请求WWW.ITCAST.CN 名称: BEANSHELL取样器 BEANSHELL取样器 注释: 线程组 每次调用前重置BSH.INTERPRETER HTTP请求 查看结果树 参数(> STRING PARAMETERS 和 STRING[]BSH.ARGS) 脚本文件 脚本(见下文所定义的变量) 填入刚才生成的表达式 1 SETPROPERTY(GVAL,SIVAL)))))) -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670502063193-4c2e88de-9eee-44a5-b080-ac9f2ff16baf.png)

3. 有了全局变量Gval我们怎么获取呢？还是使用函数助手生成获取全局变量的表达式

<!-- 这是一张图片，ocr 内容为：? 测试计划 HTTP请求 钱程组 请求WWW.ITCAST.N 名称: HTTP请求 BEANSHELL取样器 注释: 线程组 HTTP请求 基本 高级 直看结果树 WEB服务器 服务器名称或IP: 协议: WWW.BAIDU.COM HTTP HTTP请求 函数助手 X GET 帮助 PROPERTY 白动乘方 函数参数 参数 消息体 值 名称: 属性名称 GVAL 留码? 存储结果的变量名(可选) 重置变量 拷贝并粘贴函数了符串 生成 {PROPERTY(GVAL.)] THE RESULT OF THE FUNCUON IS I MIVALF 当前JMETER变量 START.M5-1670478443908 TESTSTART.MS 1670501814879 DMETERTHREAD.LAST SAMPLE OK TRUE START.HMS-134723 START.YMD-20221208 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670502189171-8e1f019a-1471-4bbb-a084-7a9deb2a80d7.png)

4. 给百度请求设置好变量

<!-- 这是一张图片，ocr 内容为：文件 箱辑合我运行选项 订 帮助 西 00.00:01 测试计划 HTTP请求 杭州WWWILCAST.CN 名称: BEANSHELL取样器 注释: 我们组 HIP请求 基本高级 查看结果树 进口号: 服务举名称或IP: 协议:HTTP HIP请求 路径: 内容编码: LSPWD-ST_PROPERTY(GWAL.JF GET 白放真定间 双阳奠定向 ER KOCPALNE DATE SPOST HUTT/FORM DATA 步数酒品体数据文件上传 同请求一起发送零数: 名称: 编码? -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670502239528-75d46cfa-205c-46b6-b4bc-f6f7d8bcac2c.png)

5. 为了保证每个线程组顺序执行，记得勾选独立运行每个线程组

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1769754835981-f190e6ce-a3aa-48e7-828f-bf4faa2c1afe.png)

6. 选中查看结果树，运行

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1769754741180-75ecff37-10cf-4fd9-98a1-758d6c5984cc.png)

# 5. 性能测试
模拟各种正常的，峰值的测试环境，检测程序的各项性能指标是否达标

## 1. 高并发
要求： 同一时刻有100个人去访问同一个接口。统计高并发情况下平均响应时间和错误率

1. 添加请求

<!-- 这是一张图片，ocr 内容为：测试计划 线程组 传线程组 名称: 请求获取用户接口 在眼科器语误后要执行的动作 停止测试 宜印停止测试 停止线程 启动下一进程循环 华美 规理同性 100 没理数: 钻环次数 京永远 还迟创建线理直到需要 调度器 持续时间(秒) 用动起道步) -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670503645805-ee608e6e-e751-4dc1-a46c-3e0e15a6e419.png)

<!-- 这是一张图片，ocr 内容为：00:00:04 140 0/2 测试计划 HTTP请求 商 品求快取用户按口 HTTP请求 名称: HITP请求 注意: WEBM务器 8080 服务最名称欢IP: 出口号: LOCALHOST 协议: HTP HIP请求 内容编码: 路径: 5105N/ 比用 KEAPALWE 自动复定向 对P051 用MUTTIPARM-DATA 我数消息体数据文性上低 同培求一起发送零数: 值 编码? 名北: 内容类量 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670503676483-cb45bdc3-9e2e-47e3-a365-9ffbe16cbdcb.png)

2. 为请求添加同步器

<!-- 这是一张图片，ocr 内容为：件 编辑 查找 运行 选项 工具 帮助 测试计划 HTTP请求 请求获取用户接口 HTTP请求 HTTP请求 断言 添加 插入上级 固定定时器 定时器 统一随机定时器 剪切 CTRL-X 前置处理器 准确的吞吐量定时器 复制 CTRL-C 后置处理器 常数吞吐量定时器 粘贴 CTRL-V 配置元件 复写 CTRL+SHIFT-C JSR223定时器 监听器 服务器名称或IP: LOCAL 删除 同步定时器 DELETE 泊松随机定时器 打开... 高斯随机定时器 径: 合并 /USERS BEANSHELL 定时器 选中部分保存为.. 对POST使用MULTIPAR 使用KEEPALIVE 定向 保存为测试片段 文件上传 消息体数据 参数 保存节点为图片 CTRL-G 保存屏幕为图片 CTRL+SHIFT-G 启用 名称: 禁用 切换 CTRL-T 帮助 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670503729207-9d37cc57-130e-4dc2-bb7d-84492dbb603e.png)

<!-- 这是一张图片，ocr 内容为：002 00:00:04 同步定时器 请求获取用户接口 HTTP请求 同步定时器 名称: 同步定时器 注料: 分细 我们这里需要100个用户,所以就填100 模拟用户组的数量:100   等待时间如果等待了10MS.还是没有满100个直接开始请求 超时时间以毫秒为单位:10 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670503922000-8e79b0b3-4c08-44a0-9103-32ecede76b4f.png)

3. 添加聚合报告

<!-- 这是一张图片，ocr 内容为：APACHE JMETER(5.5) 运行 选项 工具 帮助 文件 编辑 查找 十I人 测试计划 线程(用户) 添加 配置元件 粘贴 CTRL-V 试计划 查看结果树 监听器 打开.. 汇总报告 定时器 合并 聚合报告 选中部分保存为.. 前置处理器 后端监听器 后置处理器 保存节点为图片 CTRL-G JSR223监听器 断言 保存屏幕为图片 CTRL+SHIFT-G 保存响应到文件 测试片段 启用 响应时间图 非测试元件 禁用 图形结果 切换 CTRL-T 断言结果 帮助 比较断言可视化器 汇总图 生成概要结果 用表格查看结果 简单数据写入器 邮件观察仪 BEANSHELL监听器 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670504006573-3664ac78-6cf8-4e9d-a4fc-e0b5f7f393d5.png)

4. 选中聚合报告，运行，查看结果

<!-- 这是一张图片，ocr 内容为：聚合报告 请求获取用户接口 HTTP请求 聚合报告 日守定时盗 注科: 所有数据与入一个文件 仅错误日志 仅成功日志 配置 文件名 览.是示日志内容 平均每个请求多少时间 有多少失败率 看吐量 90%百分位 最小值 接收 KB/SEC 95%百分位 发送 KE/SEC 量大街 异常粥 平均值 99%百分位 中位数 100 11.01 100.5/SEC 0.00% 11-25 2238 100L5/SEE -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670504133151-eb60e3ad-9600-4450-b823-efc0b80bb0fb.png)

## 2. 高频率
QPS: query per seconds 每秒查询数，每秒访问多少次服务器

要求： 一个用户以20QPS的频率访问一个接口，持续15秒，统计服务器平均响应时间

1. 添加一个常数吞吐量定时器

<!-- 这是一张图片，ocr 内容为：TTIV.. 06 测试计划 HTTP请求 请求获取用户接口 HTTP请 HTTP请求 名称: 断言 添加 常类 插入上级 固定定时器 定时器 聚合报告 统一随机定时器 剪切 CTRL-X 前置处理器 准确的吞吐量定时器 复制 CTRL-C 后置处理器 常数吞吐量定时器 粘贴 CTRL-V 配置元件 复写 CTRL+SHIFT-C JSR223定时器 监听器 服务器 删除 DELETE 同步定时器 泊松随机定时器 打开.. 高斯随机定时器 路径: 合并 /USERS BEANSHELL定时器 选中部分保存为.. 城重定向 使用KEEPALIVE 保存为测试片段 参数 消息体数据 文件上传 保存节点为图片 CTRL-G 保存屏幕为图片 CTRL+SHIFT-G 启用 名称: 禁用 切换 CTRL-T 帮助 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670504669539-b7b15ae5-e5a9-46a0-b662-3bc2864c117c.png)

<!-- 这是一张图片，ocr 内容为：APACHE JMETER(5.5) 运行 选项 工具帮助 文件演做查找 00:00:01 测试计划 常数春吐量定时器 请求获取用户接口 常数否吐量定时器 名称: 常效存计量定时器 聚合报告 在每个受影响的采样器之前延迟 每秒20次一分钟1200次 目标吞吐量(每分钟的样本量); 1200.0 只有此线程 基于计算看吐量: -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670504735640-8821a20f-f88e-43a7-840e-5c4e13e18775.png)

2. 设置线程组

<!-- 这是一张图片，ocr 内容为：00-0001 请求获取用户楼口 HTTP请求 请求我取用户按口 名称: 中国常数存吐量定时器 聚合报告 在取样器储暖后要执行的动作 修止线程 洗涤O日 停止驶 自动下一进程临环停 文印修止测试 线程同性 1个用户 线程数 RAMP-UO时间(秒); ]永远 300 钻环改数 15秒每秒20 延迟创建线程直到需要 |调发器 持续时间(秒) -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670504793019-5f02dad1-e99f-44ed-81e0-c896969a7ea5.png)

3.运行查看结果

<!-- 这是一张图片，ocr 内容为：测式计划 聚合报告 0 请求获取用户接口 名称! 聚合录告 常数吞吐量定时器 汁秤: 聚合业告 所有数据写入一个文件 汉谐读日志 仅成功日志 显示日志内容: 文件名 配置 看叶量 99%百分位 最大位 最小边 LABEL 95%百分位 发送KB/SEC 平均值 90%百分位 异常% 00 22 0.00% HTTP请求 20.1/SEC 300 4.41 2 4.47 U 001 0.00% 20.1/SEX 包计 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670504967666-17e5c2b6-6ab2-474a-ada8-2f449c45c760.png)

<!-- 这是一张图片，ocr 内容为：编辑 查找 运行 选项 工具 帮助 00/1 00:00:14 查看结果树 请求获取用户接口 HTTP请求 名称: 查看结果树 常数存吐量定时器 聚合报告 所有数据写入一个文件 查看结果树 仅错误日志 仅成功日志 显示日志内容 湘蓝 配置 文件名 区分大小写 正别表达式 亚置 直我 取样器结果请求 响应数据 RESPONSE HEADERS HTTP请求 区分大小写 正见表达式 O HTTP请求 HTTP请求 LL.NAMO....AGE.18月(NAME,李国,.AGE.20L HTTP请求 HTTP请求 HTTP请求 HTTP请求 HTTP请求 HTTP请求 HTTP请求 HTTP请求 HTTP请求 HTTP请求 HTTP请求 HTTPIN求 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670505024213-5ea7b189-1efd-4948-9c16-83a2ad585492.png)

# 6. 分布式
当单台机器的 CPU 或内存无法支撑数万甚至数十万的并发用户（Threads）时，就需要使用 **JMeter 分布式测试**

其核心架构是 **“一主多从”**：

+ **Master (控制机)**：负责分发测试脚本（.jmx）给各个 Slave，并汇总测试结果。
+ **Slaves (负载机/从机)**：负责执行具体的测试任务，向目标服务器发送请求。

为了测试，在本机运行两个jmeter

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1769757471668-ef28da67-dbb1-4df4-891a-b72b0a0fc65d.png)

## master配置
进入bin目录，修改`jmeter.properties`如下配置

```c
# 禁用 SSL
server.rmi.ssl.disable=true
# 添加从机的ip和地址
remote_hosts=127.0.0.1:11000
```

## slave配置
进入bin目录，修改`jmeter.properties`如下配置

```c
# 禁用 SSL
server.rmi.ssl.disable=true
```

## 启动从机
在从机的bin目录下面，启动完成后黑窗口不要关闭

```c
jmeter-server -Djava.rmi.server.hostname=127.0.0.1  -Dserver_port=11000
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1769757779823-3f73fa82-3be8-4558-8955-1fecd0df03d9.png)

## 正常启动Master
bin目录点击运行jmeter.bat，启动后能看到能连接到从机

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1769757824358-c8aceb89-4e0d-4dd7-9e25-a371aa23fd04.png)

随便建一个请求测试

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1769757871408-e60bed13-8441-4d25-94a3-66bc12766c77.png)



# 7. 生成图形化报告
在Jmeter中可以以图形化（饼状图，柱状图。。。）的方式显示脚本运行结果，较于之前的聚合报告或者查看结果树更直接美观

生成图形化报告命令：

> jmeter -n -t jmx脚本文件 -l 日志文件 -e -o 目录
>

```dockerfile
-n 无图形化运行
-t 被运行的脚本
-l 将日志写到哪里
-e 生成测试报告
-o 输出到制定目录
```

1. 我们拿这个做个测试

<!-- 这是一张图片，ocr 内容为：. 十 聚合报告 请求获取用户接口 HTTP请求 聚合报告 常数有生命定时盛 江科: 聚合报告 直看结果村 所有数据与人一个文件 仅借误目志 仅成功日志 测点. 文件名 配置 最示目志内容; 平均值 最小值 最大值 中位数 99%百分位 LABEL 发送KB/SEC 90%百分位 长样水 吞吐量 异常% 接收 KB/SEC 95%自分位 1.08 600 HTTP请求 0.00% 9.2/SEC 2.04 2 1.08 600 9.2/S0C 2.04 总体 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670506409432-6907a883-06a5-4af4-b6a5-695215b440cd.png)

2. 在jmter的bin目录执行一下命令

> <font style="color:rgb(38, 44, 49);">jmeter -n -t D:\文档\jmeter\聚合报告.jmx -l D:\文档\jmeter\test.log -e -o D:\文档\jmeter\res</font>
>

3. 查看生成的结果

<!-- 这是一张图片，ocr 内容为：DATA(D:)>文档>JMETER>RES 大小 类型 名称 修改日期 文件夹 2022/12/8 21:35 CONTENT 文件夹 SBADMIN2-1.0.7 2022/12/8 21:35 MICROSOFT EDGE HT... INDEX.HTML 2022/12/8 21:35 10 KB 1 KB JSON源文件 2022/12/8  21:35 STATISTICS.JSON -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670506632590-f8cfeb6f-44f7-4e60-bbae-c485f48c9a17.png)

<!-- 这是一张图片，ocr 内容为：R DASHBOARD APACHE JMETER DASHBOARD TEST AND REPORT INFORMATION CHARTS OVER TIME 'TEST.LOG 12/8/22  9:35 PM* START TIME THROUGHPUT 12/8/22 9:35 PM END TIME RESPONSE TIMES FILTER FOR DISPLAY LLL CUSTOMS GRAPHS APDEX(APPLICATION PERFORMANCE INDEX) REQUESTS SUMMARY FALL T(TOLERATION THRESHOLD) F(FRUSTRATION THRESHOLD) APDEK PASS 1 SEC 500 MS TOTAL 500MS 1.000 HTTP请求 1 SEO 500 MS 500 MS 1.000 PASS 100% STATISTICS RESPONSE TIMEA (MS) THROUGHPUT REQUESTS 99TH PCT 95TH PCT MIN ERROR% FAIL 90TH PCT MAX #SAMPLES MEDIAN SENT LABOL 0 2.00 20.09 28 1.00 300 0.00% 4.00 2.00 1.45 0 HTTP泊求 300 28 20.09 2.00 1.00 2.00 1.45 0.00% 4.47 4.00 2.43 -->
![](https://cdn.nlark.com/yuque/0/2022/png/12600036/1670506642752-862c3228-13ed-475c-93f8-647a89c731db.png)



## 汉化图形化报告
默认生成的生成图形化报告是英文的，如果想替换成中文需要一些额外的操作

### 步骤
1. 下载汉化模版Gitee (推荐):[https://gitee.com/smooth00/jmeter-cn-report-template](https://gitee.com/smooth00/jmeter-cn-report-template)
2. 备份原文件夹： 进入你的 JMeter 安装目录，找到 `bin/report-template`，将其改名为 `report-template_bak` 作为备份。
3. 替换模板： 将下载好的汉化包解压，把里面的 `report-template` 文件夹整个复制到 JMeter 的 `bin` 目录下
4. **重新生成报告：** 再次运行你的命令（记得先删除旧生成报告）

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/12600036/1770168438909-e55294c9-0915-4f09-9ac0-284aaf4c714f.png)

## 样式可以自定义样式吗
JMeter 自带的 HTML 图形化报告，本身不太适合“深度样式定制”，更多是偏「自动生成 + 统一风格」 ，因为JMeter 本质是 数据生产者，不是 BI 工具。真正“高度可定制”方式是利用Jmeter产出数据+其他可视化工具。

当然如果愿意花时间去修改去JMeter 自带的图形化报告也是可以的。这个需要自己一点点调整，自己检查报告的HTML的代码和使用的样式，然后找到对应的样式自己去修改。本人没有并没有深入研究，感兴趣的同学可以自行尝试一下。这里只给出2个参考方向，你可以通过以下几个层面进行自定义：

1. **修改配色与图标（CSS 层**）

所有的样式定义都在 `bin/report-template/content/css`目录下，可以通过修改样式来调整。

2. **自定义展示内容（模板层）**

JMeter 使用了 FreeMarker (.fmkr) 模板引擎 `bin/report-template/*.html.fmkr` 以及`content/pages/*.html.fmk`。需要了解FreeMarker的语法以及模版和界面对应的元素。

# 8.参考

参考视频：https://www.bilibili.com/video/BV1ty4y1q72g
