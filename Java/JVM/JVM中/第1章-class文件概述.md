# [https://docs.oracle.com/javase/specs/index.html](https://docs.oracle.com/javase/specs/index.html)
# 01-概述
## 字节码文件的跨平台性
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671187227067-934baccb-8428-4f4b-8c96-b6ec187ce018.png#averageHue=%23c8e3c0&clientId=u01844bf6-2745-4&from=paste&height=408&id=u68ed4da5&originHeight=612&originWidth=1075&originalType=binary&ratio=1&rotation=0&showTitle=false&size=401960&status=done&style=none&taskId=u02b6dd1b-db3c-4f03-a558-57e70653118&title=&width=716.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671187231506-d165eb6c-5525-46b1-8efe-fc635ed3e47e.png#averageHue=%23c6e2c2&clientId=u01844bf6-2745-4&from=paste&height=351&id=ua568f6ba&originHeight=527&originWidth=1079&originalType=binary&ratio=1&rotation=0&showTitle=false&size=350400&status=done&style=none&taskId=u5c0c283d-ffa8-4909-8ed8-53cd06cd892&title=&width=719.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671187237005-36ff691c-4f7f-461a-9912-146178846e43.png#averageHue=%23e8da68&clientId=u01844bf6-2745-4&from=paste&height=362&id=u51d31325&originHeight=543&originWidth=806&originalType=binary&ratio=1&rotation=0&showTitle=false&size=191219&status=done&style=none&taskId=u08d3450f-7c86-4037-b4f4-acada277b89&title=&width=537.3333333333334)
## Java的前端编译器
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671187290153-5a3c4bc8-af7d-4e3c-a86d-adf5fe45ad76.png#averageHue=%23d8d38a&clientId=u01844bf6-2745-4&from=paste&height=379&id=u928d09f2&originHeight=568&originWidth=1077&originalType=binary&ratio=1&rotation=0&showTitle=false&size=197861&status=done&style=none&taskId=ue76fa44a-a1e0-410e-ad77-afcad87eacd&title=&width=718)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671187300491-20eccfa0-bec5-4f3b-9a41-065cd582ae12.png#averageHue=%23bfdbbb&clientId=u01844bf6-2745-4&from=paste&height=337&id=ue0bd0e18&originHeight=505&originWidth=1083&originalType=binary&ratio=1&rotation=0&showTitle=false&size=591579&status=done&style=none&taskId=u811c127d-66df-49dc-a8ed-48438a5257d&title=&width=722)
## 透过字节码指令看代码细节
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671187334258-4b34a4c7-7538-4b40-bf0d-ebaba8933742.png#averageHue=%23c3e0bf&clientId=u01844bf6-2745-4&from=paste&height=65&id=u829eca2a&originHeight=98&originWidth=833&originalType=binary&ratio=1&rotation=0&showTitle=false&size=59394&status=done&style=none&taskId=u4432f159-53e0-4682-aebc-7a64beb6df3&title=&width=555.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671187346406-91712f98-cda1-4ed8-a9de-d3b52a530e35.png#averageHue=%23c6e4c2&clientId=u01844bf6-2745-4&from=paste&height=363&id=ubc53d75e&originHeight=544&originWidth=1097&originalType=binary&ratio=1&rotation=0&showTitle=false&size=153982&status=done&style=none&taskId=uf03340b7-8a9f-49f6-8364-da033246b0e&title=&width=731.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671187419082-9854845e-f987-475f-ad7e-2f3d2fcd24b3.png#averageHue=%23c4e2c0&clientId=u01844bf6-2745-4&from=paste&height=213&id=u981ce489&originHeight=319&originWidth=1093&originalType=binary&ratio=1&rotation=0&showTitle=false&size=124412&status=done&style=none&taskId=u730f925d-9379-414d-9600-51a7c7b0139&title=&width=728.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671187423403-af59599d-bcf9-497f-ba6f-64c8c0119ec2.png#averageHue=%23c6e4c2&clientId=u01844bf6-2745-4&from=paste&height=469&id=ud551b828&originHeight=703&originWidth=1092&originalType=binary&ratio=1&rotation=0&showTitle=false&size=222576&status=done&style=none&taskId=u3c3bb574-20b0-42fb-a6d0-cbee0f38fed&title=&width=728)
# 02-虚拟机的基石: Class文件
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671424331220-a9514685-bbde-4800-94a4-866147e06687.png#averageHue=%23c6e2c1&clientId=ueeec1a54-052d-4&from=paste&height=342&id=ua665b30e&originHeight=513&originWidth=1087&originalType=binary&ratio=1&rotation=0&showTitle=false&size=279776&status=done&style=none&taskId=u42b26fe6-577f-48b7-b91d-03b6ed97623&title=&width=724.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671424353547-9b005fd5-c9b3-4f32-8ea8-b09493caa27e.png#averageHue=%23c6e2c1&clientId=ueeec1a54-052d-4&from=paste&height=342&id=ucca2ac27&originHeight=513&originWidth=1087&originalType=binary&ratio=1&rotation=0&showTitle=false&size=279776&status=done&style=none&taskId=uf66cd2f2-9415-4692-89c3-61f22321771&title=&width=724.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671424366615-a1268518-91f7-4d72-b5f5-9df1191721cb.png#averageHue=%23c9e4c5&clientId=ueeec1a54-052d-4&from=paste&height=338&id=ubabe90ec&originHeight=507&originWidth=833&originalType=binary&ratio=1&rotation=0&showTitle=false&size=224001&status=done&style=none&taskId=ud57fd495-9341-4f2a-8c87-213eaa8b29a&title=&width=555.3333333333334)
# 03-Class文件结构
## 01-魔数：Class文件的标志
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671424608820-d727dac2-7271-4d0b-b004-bbf053543a63.png#averageHue=%23c2dfbe&clientId=ueeec1a54-052d-4&from=paste&height=259&id=ub55316f2&originHeight=389&originWidth=1090&originalType=binary&ratio=1&rotation=0&showTitle=false&size=294447&status=done&style=none&taskId=udf389894-3bb7-417f-8008-c1e09ddf66c&title=&width=726.6666666666666)
## 02-Class文件版本号
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671425236752-17f70c91-00b3-44e6-ad74-0a3ad03e00be.png#averageHue=%23c6d1b8&clientId=ueeec1a54-052d-4&from=paste&height=417&id=u967bb5ba&originHeight=626&originWidth=1088&originalType=binary&ratio=1&rotation=0&showTitle=false&size=248552&status=done&style=none&taskId=u8e977d55-dd44-441d-a4ec-606c70d74b4&title=&width=725.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671425240462-c97318f5-44cd-4237-a404-032a9c874cf3.png#averageHue=%23c2dfbd&clientId=ueeec1a54-052d-4&from=paste&height=137&id=ud4267c7b&originHeight=205&originWidth=1071&originalType=binary&ratio=1&rotation=0&showTitle=false&size=280296&status=done&style=none&taskId=u2d40bf01-e111-4eaa-b938-4faddd74bde&title=&width=714)
## 03-常量池：存放所有常量
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671425306079-dca2ecdc-cb51-4064-ae3c-b77f1f85eadb.png#averageHue=%23d1e6c7&clientId=ueeec1a54-052d-4&from=paste&height=387&id=u0b0039e8&originHeight=581&originWidth=1001&originalType=binary&ratio=1&rotation=0&showTitle=false&size=363635&status=done&style=none&taskId=ub49931aa-4bee-45ed-9088-bbd5380e3f2&title=&width=667.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671425310954-57af2d3f-1295-4e58-8abb-59a7795e654e.png#averageHue=%23c5e0c1&clientId=ueeec1a54-052d-4&from=paste&height=149&id=u7b72ce14&originHeight=223&originWidth=1080&originalType=binary&ratio=1&rotation=0&showTitle=false&size=189013&status=done&style=none&taskId=u7132339b-86fa-4c40-8bca-7e56b2b707d&title=&width=720)
### 1-常量池计数器
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671425595595-31978f2b-0206-49e0-b612-5725d336962e.png#averageHue=%23c3deba&clientId=ueeec1a54-052d-4&from=paste&height=253&id=ub06ae3c4&originHeight=379&originWidth=1087&originalType=binary&ratio=1&rotation=0&showTitle=false&size=310317&status=done&style=none&taskId=u68e87184-2292-4565-9489-c7a6884c34b&title=&width=724.6666666666666)
### 2-常量池表
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671425606629-62685618-69cb-4aec-9d40-2c2dda7035e4.png#averageHue=%23bdd9b9&clientId=ueeec1a54-052d-4&from=paste&height=82&id=u29cbf465&originHeight=123&originWidth=1076&originalType=binary&ratio=1&rotation=0&showTitle=false&size=181280&status=done&style=none&taskId=ue30f2eec-6b76-4be6-b97a-03cb0f61bd4&title=&width=717.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671425609434-69c213ed-8b97-4bb7-a176-dd2394f2e079.png#averageHue=%23e5e3dd&clientId=ueeec1a54-052d-4&from=paste&height=393&id=u2925c5b3&originHeight=590&originWidth=1068&originalType=binary&ratio=1&rotation=0&showTitle=false&size=263023&status=done&style=none&taskId=u4338ca7a-f188-4816-89e9-fd8c5e7992a&title=&width=712)
**2.1 字面量和符号引用**
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671426728941-e709dc7b-ba69-4f7f-a984-d448518b5a21.png#averageHue=%23c5e1c0&clientId=u6f50dfff-ccc7-4&from=paste&height=412&id=u1784102f&originHeight=618&originWidth=1079&originalType=binary&ratio=1&rotation=0&showTitle=false&size=432867&status=done&style=none&taskId=ufc92bbb1-3e9a-4182-bcef-94e017f1333&title=&width=719.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671426740427-df5cac3d-7338-4073-8cdf-9618ca006a12.png#averageHue=%23e3e1d9&clientId=u6f50dfff-ccc7-4&from=paste&height=313&id=u88f01a8f&originHeight=470&originWidth=1081&originalType=binary&ratio=1&rotation=0&showTitle=false&size=128106&status=done&style=none&taskId=u9f38c0e6-4796-4f6a-8ee4-b300bc2c271&title=&width=720.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671426744370-cc193669-6b83-4635-86cb-ac49d7e34242.png#averageHue=%23bfdbbb&clientId=u6f50dfff-ccc7-4&from=paste&height=255&id=ud8b8ba88&originHeight=383&originWidth=1086&originalType=binary&ratio=1&rotation=0&showTitle=false&size=494257&status=done&style=none&taskId=u460f4259-d5c4-4deb-a83e-e6da7182e83&title=&width=724)
**2.2 常量类型和结构
**![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671426775217-45b2324f-0251-4eda-a832-872bd321630a.png#averageHue=%23c2e0be&clientId=u6f50dfff-ccc7-4&from=paste&height=53&id=u5c6b76e8&originHeight=80&originWidth=1048&originalType=binary&ratio=1&rotation=0&showTitle=false&size=50408&status=done&style=none&taskId=u489153d6-0662-461c-8b14-f7fbf03bd0f&title=&width=698.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671426778944-b7618aa1-f9c6-429a-95eb-480e46475330.png#averageHue=%23f4c261&clientId=u6f50dfff-ccc7-4&from=paste&height=407&id=u09f7753c&originHeight=610&originWidth=1083&originalType=binary&ratio=1&rotation=0&showTitle=false&size=64141&status=done&style=none&taskId=u362a20ff-970e-495d-94e9-6f0b522d340&title=&width=722)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671426782545-dec9fb83-5f9a-4534-b1bd-9360ee6821dc.png#averageHue=%23f5c262&clientId=u6f50dfff-ccc7-4&from=paste&height=379&id=u3c60adb6&originHeight=569&originWidth=1083&originalType=binary&ratio=1&rotation=0&showTitle=false&size=49635&status=done&style=none&taskId=u30f7118c-4f32-439b-874c-86d722801f1&title=&width=722)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671426787271-0f869c38-d7c3-4494-a4d0-b1f057271979.png#averageHue=%23c4dbca&clientId=u6f50dfff-ccc7-4&from=paste&height=322&id=u9a30e1b7&originHeight=483&originWidth=1031&originalType=binary&ratio=1&rotation=0&showTitle=false&size=497462&status=done&style=none&taskId=u38a8a6b7-9d81-4ebf-aaa6-97ce9b4056b&title=&width=687.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671426790588-10256b49-9e78-4126-968e-a6e7b35ae103.png#averageHue=%23c7decb&clientId=u6f50dfff-ccc7-4&from=paste&height=253&id=u7f94ca12&originHeight=380&originWidth=1030&originalType=binary&ratio=1&rotation=0&showTitle=false&size=124012&status=done&style=none&taskId=ua12b36b5-be5b-48e0-9fb5-77837a1e8b1&title=&width=686.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671426795380-334eb334-d307-48f4-a7c5-27bad194c9d4.png#averageHue=%23c0d7c5&clientId=u6f50dfff-ccc7-4&from=paste&height=219&id=u935cb2fd&originHeight=328&originWidth=1046&originalType=binary&ratio=1&rotation=0&showTitle=false&size=326005&status=done&style=none&taskId=u2466f041-9ac2-4202-8a97-a87b4c28588&title=&width=697.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671426799080-a28bb63f-6ab1-41b3-b185-5962ea57f59b.png#averageHue=%23c0d6c4&clientId=u6f50dfff-ccc7-4&from=paste&height=174&id=u50366f9b&originHeight=261&originWidth=1044&originalType=binary&ratio=1&rotation=0&showTitle=false&size=325898&status=done&style=none&taskId=uaa698f4f-d3e5-4f7c-9ff4-644391fc259&title=&width=696)
## 04-访问标识
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671427217501-255635f1-acdd-4ad4-b920-5d857ed04df4.png#averageHue=%23c7e0ca&clientId=u6f50dfff-ccc7-4&from=paste&height=412&id=u10628c4b&originHeight=618&originWidth=1042&originalType=binary&ratio=1&rotation=0&showTitle=false&size=442877&status=done&style=none&taskId=u131abb7a-b464-4b7e-8b16-beffd5c6895&title=&width=694.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671427222739-b646b454-af39-4b68-8f92-00c74762f85e.png#averageHue=%23c4dfc9&clientId=u6f50dfff-ccc7-4&from=paste&height=369&id=u605c48ce&originHeight=553&originWidth=1041&originalType=binary&ratio=1&rotation=0&showTitle=false&size=543738&status=done&style=none&taskId=ue1d7afcb-ff6d-4f6b-aed6-043115559ea&title=&width=694)
## 05-类索引、父类索引、接口索引集合
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671427935857-3f27cfa1-39ec-4dc4-a211-55565aed7308.png#averageHue=%23c5dfc8&clientId=u6f50dfff-ccc7-4&from=paste&height=411&id=ua6a43382&originHeight=616&originWidth=1040&originalType=binary&ratio=1&rotation=0&showTitle=false&size=435265&status=done&style=none&taskId=ue295d934-ac00-4378-a3f3-42a3af9e581&title=&width=693.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671427941440-04e917fd-c541-4ce3-a43c-574740371c28.png#averageHue=%23c1dcc6&clientId=u6f50dfff-ccc7-4&from=paste&height=339&id=udd88fbc0&originHeight=508&originWidth=1042&originalType=binary&ratio=1&rotation=0&showTitle=false&size=467222&status=done&style=none&taskId=u2effc8c4-1da6-4496-a4d4-65ae0a5659f&title=&width=694.6666666666666)
## 06-字段表集合
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671427972580-7ef9633a-b85e-4423-a1b0-fe8a52857243.png#averageHue=%23c3e0be&clientId=u6f50dfff-ccc7-4&from=paste&height=292&id=u5fcadcff&originHeight=438&originWidth=1036&originalType=binary&ratio=1&rotation=0&showTitle=false&size=381630&status=done&style=none&taskId=u81794e28-c57f-4cb9-b8df-b935f8d5482&title=&width=690.6666666666666)
### 1-字段计数器
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671428103555-2091e1ca-2fee-4b08-93c3-af2ba9ea27f0.png#averageHue=%23bfdcbc&clientId=u6f50dfff-ccc7-4&from=paste&height=84&id=udc674610&originHeight=126&originWidth=1042&originalType=binary&ratio=1&rotation=0&showTitle=false&size=131068&status=done&style=none&taskId=u2f8ca2b1-4c58-482f-86b0-52f41346d83&title=&width=694.6666666666666)
### 2-字段表
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671428118039-2154b690-a3c8-4168-b458-847c1cfa3ccb.png#averageHue=%23c6e3c2&clientId=u6f50dfff-ccc7-4&from=paste&height=419&id=uc047efd7&originHeight=629&originWidth=974&originalType=binary&ratio=1&rotation=0&showTitle=false&size=316957&status=done&style=none&taskId=ued215204-37f4-40d0-8a88-a5ae18f4ebb&title=&width=649.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671428125794-381280e8-8284-4c17-857b-db4efb07b169.png#averageHue=%23c8e3c3&clientId=u6f50dfff-ccc7-4&from=paste&height=337&id=u6223367d&originHeight=505&originWidth=1030&originalType=binary&ratio=1&rotation=0&showTitle=false&size=294636&status=done&style=none&taskId=u36b30ad3-a637-464d-bb0f-fc05225e6c5&title=&width=686.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671428131244-1f119231-2b13-48cd-85e3-6ed4ed31b7aa.png#averageHue=%23c5e3c1&clientId=u6f50dfff-ccc7-4&from=paste&height=65&id=ufd453855&originHeight=97&originWidth=1015&originalType=binary&ratio=1&rotation=0&showTitle=false&size=36239&status=done&style=none&taskId=u6d1caf49-8bdc-432b-9cfa-cd1e2c8e14a&title=&width=676.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671428135480-cc84f076-1b12-4500-9a80-c50cf444b083.png#averageHue=%23c7e3c3&clientId=u6f50dfff-ccc7-4&from=paste&height=361&id=udcde2759&originHeight=541&originWidth=1041&originalType=binary&ratio=1&rotation=0&showTitle=false&size=246483&status=done&style=none&taskId=u36323d4d-4014-4418-9cff-c6dc152f25e&title=&width=694)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671428140406-be5ff9b5-8697-4f2a-9164-b9b9439c9e2b.png#averageHue=%23c5e2c0&clientId=u6f50dfff-ccc7-4&from=paste&height=193&id=ua92302c1&originHeight=290&originWidth=929&originalType=binary&ratio=1&rotation=0&showTitle=false&size=163277&status=done&style=none&taskId=u9f6128a1-5755-4510-a99b-7b147a525f2&title=&width=619.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671428146127-c0efbd09-9b02-4541-8d3b-207c25bf7a88.png#averageHue=%23d6e7ce&clientId=u6f50dfff-ccc7-4&from=paste&height=187&id=u6595b84d&originHeight=280&originWidth=955&originalType=binary&ratio=1&rotation=0&showTitle=false&size=177461&status=done&style=none&taskId=u6e91fc66-a642-4db9-903d-adf423e7722&title=&width=636.6666666666666)
## 07-方法表集合
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671428943084-3ecdca83-8beb-4394-a69d-c24f4823de47.png#averageHue=%23c0ddbc&clientId=u6f50dfff-ccc7-4&from=paste&height=377&id=ube54d4af&originHeight=566&originWidth=1041&originalType=binary&ratio=1&rotation=0&showTitle=false&size=598865&status=done&style=none&taskId=u9bbf3ad9-1f7d-4082-8d88-ebaa76f63e6&title=&width=694)
### 1-方法计数器
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671428962048-96c68e56-76ac-4dd6-89d2-5ef0591d6d8f.png#averageHue=%23c3e0bf&clientId=u6f50dfff-ccc7-4&from=paste&height=87&id=u8959f1c9&originHeight=130&originWidth=1021&originalType=binary&ratio=1&rotation=0&showTitle=false&size=78654&status=done&style=none&taskId=u2d37f665-d2a6-4ee4-ab84-fce333fa8ae&title=&width=680.6666666666666)
### 2-方法表
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671428987031-f82cf1a7-8d26-431a-a191-36ca28c8570d.png#averageHue=%23c6e1c1&clientId=u6f50dfff-ccc7-4&from=paste&height=287&id=u7177eab5&originHeight=430&originWidth=1045&originalType=binary&ratio=1&rotation=0&showTitle=false&size=297319&status=done&style=none&taskId=ub21dc36b-0d27-4291-81ca-b5048ab5ebd&title=&width=696.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671428991367-2646945b-f7b9-44c2-80e8-7a03e1985910.png#averageHue=%23cae4c5&clientId=u6f50dfff-ccc7-4&from=paste&height=180&id=ue2726d52&originHeight=270&originWidth=957&originalType=binary&ratio=1&rotation=0&showTitle=false&size=140438&status=done&style=none&taskId=u6007d57a-44d6-42ba-8feb-1cd244c514c&title=&width=638)、
## 08-属性表集合
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671429149071-06e70a67-6336-4352-a5b3-69336d5784af.png#averageHue=%23c1debd&clientId=u6f50dfff-ccc7-4&from=paste&height=195&id=ub722771b&originHeight=293&originWidth=1042&originalType=binary&ratio=1&rotation=0&showTitle=false&size=269004&status=done&style=none&taskId=u8df96cd0-f7d7-49c5-8630-27ae8831726&title=&width=694.6666666666666)
### 1-属性计数器
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671429190610-1dce5ce0-27e3-40b1-8480-36c08e67e67d.png#averageHue=%23bfdbbb&clientId=u6f50dfff-ccc7-4&from=paste&height=46&id=udcb36acf&originHeight=69&originWidth=930&originalType=binary&ratio=1&rotation=0&showTitle=false&size=63360&status=done&style=none&taskId=ue042067e-5528-419b-93a4-3dc4066bcfb&title=&width=620)
### 2-属性表
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671429198111-cf8caaee-c544-4384-8275-2f349e0f8395.png#averageHue=%23c7e2c2&clientId=u6f50dfff-ccc7-4&from=paste&height=219&id=ua88901f0&originHeight=329&originWidth=981&originalType=binary&ratio=1&rotation=0&showTitle=false&size=166423&status=done&style=none&taskId=u0eb607c2-f99e-4618-a214-2249c3e8dc0&title=&width=654)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671429205089-cc09832f-ac0f-4922-ac60-38cc71ca68d0.png#averageHue=%23cbe4c6&clientId=u6f50dfff-ccc7-4&from=paste&height=313&id=u0e5d5e20&originHeight=469&originWidth=898&originalType=binary&ratio=1&rotation=0&showTitle=false&size=234238&status=done&style=none&taskId=ubc4be740-b960-4ed0-9178-c48b460cd63&title=&width=598.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671429211767-de8ce813-a0ab-44c2-8832-79c9694535f0.png#averageHue=%23f2f2f2&clientId=u6f50dfff-ccc7-4&from=paste&height=383&id=uba949c31&originHeight=575&originWidth=702&originalType=binary&ratio=1&rotation=0&showTitle=false&size=235367&status=done&style=none&taskId=u91894bdc-8c3d-496f-a862-d7b2cbde834&title=&width=468)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671429235335-9717422a-68fc-4c50-bc8c-9d1c6bc6e3ae.png#averageHue=%23e1e5d9&clientId=u6f50dfff-ccc7-4&from=paste&height=140&id=u521183d5&originHeight=210&originWidth=706&originalType=binary&ratio=1&rotation=0&showTitle=false&size=125957&status=done&style=none&taskId=uaf78e3ef-4684-4ff6-86d7-9392092d5b6&title=&width=470.6666666666667)
或者（查看官网）
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671429246817-3ab97a7a-ce2c-4329-a999-0bcb7218ea8d.png#averageHue=%23f1f1f0&clientId=u6f50dfff-ccc7-4&from=paste&height=525&id=u906945ee&originHeight=788&originWidth=1106&originalType=binary&ratio=1&rotation=0&showTitle=false&size=80393&status=done&style=none&taskId=u7d345413-be9c-4179-aac9-ce174d04369&title=&width=737.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671429252502-cec8db6d-f1de-4ed9-a3de-370f86bd73d6.png#averageHue=%23c2dfbe&clientId=u6f50dfff-ccc7-4&from=paste&height=340&id=u92f318ec&originHeight=510&originWidth=997&originalType=binary&ratio=1&rotation=0&showTitle=false&size=255614&status=done&style=none&taskId=u4889f613-3377-4060-83b7-354122929a0&title=&width=664.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671429256180-a81ae9ab-bba9-4820-b9b6-b1fef3704626.png#averageHue=%23c6e3c2&clientId=u6f50dfff-ccc7-4&from=paste&height=387&id=u66aea3d3&originHeight=580&originWidth=1020&originalType=binary&ratio=1&rotation=0&showTitle=false&size=284771&status=done&style=none&taskId=uf73c3423-063f-43b4-b150-094c34f998d&title=&width=680)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671429259999-e372961a-7678-4ac8-a100-be3a2cc22c41.png#averageHue=%23bedaba&clientId=u6f50dfff-ccc7-4&from=paste&height=100&id=u7f8b251f&originHeight=150&originWidth=1045&originalType=binary&ratio=1&rotation=0&showTitle=false&size=152052&status=done&style=none&taskId=uf821d2b2-4138-450e-a7d7-8f1eb41cbc8&title=&width=696.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671429263497-71e11490-c65b-49fa-9b0f-5b33409bc53b.png#averageHue=%23c2ddc0&clientId=u6f50dfff-ccc7-4&from=paste&height=340&id=u2836eb5c&originHeight=510&originWidth=1047&originalType=binary&ratio=1&rotation=0&showTitle=false&size=318386&status=done&style=none&taskId=ua31f49c8-2549-4e55-a072-13c3ace839a&title=&width=698)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671429266806-f92beb6d-8a88-4931-9fe7-9ec58b6847a8.png#averageHue=%23c3dec1&clientId=u6f50dfff-ccc7-4&from=paste&height=379&id=u653bdeef&originHeight=569&originWidth=1035&originalType=binary&ratio=1&rotation=0&showTitle=false&size=353953&status=done&style=none&taskId=uc44cbbde-7f6a-4757-b041-e59e5168881&title=&width=690)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671429270447-8fcf4bff-0bb4-4d01-b534-2332d0999391.png#averageHue=%23bdd7bb&clientId=u6f50dfff-ccc7-4&from=paste&height=93&id=u1a2897ab&originHeight=140&originWidth=1037&originalType=binary&ratio=1&rotation=0&showTitle=false&size=137776&status=done&style=none&taskId=u623bc09a-9992-4742-945a-90a45e22bfb&title=&width=691.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671429274564-481f8666-dbfe-45c1-8c61-f43a10c7f3fd.png#averageHue=%23c8e4c3&clientId=u6f50dfff-ccc7-4&from=paste&height=265&id=u038ee21e&originHeight=397&originWidth=1038&originalType=binary&ratio=1&rotation=0&showTitle=false&size=162599&status=done&style=none&taskId=uf38cc01c-4744-4dd9-9201-86d00aaa420&title=&width=692)
## 09-小结
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671430004143-52dc1d09-28de-424e-a94d-bd102cf3ffec.png#averageHue=%23c3e0bf&clientId=u6f50dfff-ccc7-4&from=paste&height=147&id=uaab65be1&originHeight=221&originWidth=987&originalType=binary&ratio=1&rotation=0&showTitle=false&size=150507&status=done&style=none&taskId=u0a89a4e7-5d90-45fe-996a-b607e3ded07&title=&width=658)
# 04-使用javap指令解析Class文件
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671430058082-45e43764-71eb-4fd3-9380-1499ad056250.png#averageHue=%23bedbba&clientId=u6f50dfff-ccc7-4&from=paste&height=267&id=u6461ef3d&originHeight=400&originWidth=997&originalType=binary&ratio=1&rotation=0&showTitle=false&size=403101&status=done&style=none&taskId=ucc4210ad-9112-4a67-aa66-a28b72a952f&title=&width=664.6666666666666)
## 1-解析字节码的作用
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671430225590-cf84105d-c29e-4c6b-9fa7-a5677206cb34.png#averageHue=%23bfd9c4&clientId=u6f50dfff-ccc7-4&from=paste&height=141&id=u1526472b&originHeight=211&originWidth=952&originalType=binary&ratio=1&rotation=0&showTitle=false&size=212091&status=done&style=none&taskId=u40ca2f64-260a-4c73-8821-d25b3bca734&title=&width=634.6666666666666)
## 2-javac -g操作
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671430323000-dc404a2f-9e97-4621-a1f1-daf0e9b6fb91.png#averageHue=%23bfd9c3&clientId=u6f50dfff-ccc7-4&from=paste&height=117&id=u65418d40&originHeight=176&originWidth=959&originalType=binary&ratio=1&rotation=0&showTitle=false&size=197330&status=done&style=none&taskId=ubde2f8f4-7b4a-4625-9431-8455243666b&title=&width=639.3333333333334)
## 3-javap的用法
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671430360559-56ad7801-182d-4d9d-8c10-6bb3d07d20db.png#averageHue=%23c0dac4&clientId=u6f50dfff-ccc7-4&from=paste&height=309&id=ue3595c6e&originHeight=464&originWidth=964&originalType=binary&ratio=1&rotation=0&showTitle=false&size=291734&status=done&style=none&taskId=ue59ff334-1650-47c5-b1a3-014610d5ab0&title=&width=642.6666666666666)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671430378028-d428d6e1-4db9-47c2-8640-178acdf6092c.png#averageHue=%23a6dcaa&clientId=u6f50dfff-ccc7-4&from=paste&height=375&id=u0f190dc4&originHeight=563&originWidth=899&originalType=binary&ratio=1&rotation=0&showTitle=false&size=321336&status=done&style=none&taskId=u97f5e96b-e13e-4973-999c-f89537d55c6&title=&width=599.3333333333334)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671430389541-e5d5a5e3-d762-4b38-a0b9-99edd794352c.png#averageHue=%23c4dfc9&clientId=u6f50dfff-ccc7-4&from=paste&height=109&id=uebd7a7a0&originHeight=163&originWidth=977&originalType=binary&ratio=1&rotation=0&showTitle=false&size=103203&status=done&style=none&taskId=u92038d80-474a-4a06-b4e8-61a58772eb6&title=&width=651.3333333333334)
注意：①-v相当于-c -l ②-v也不会输出私有的字段、方法等信息，所以如果想输出私有的信息，那需要在-v后面加上-p才行
## 4-使用举例
```java
public class JavapTest {
    private int num;
    boolean flag;
    protected char gender;
    public String info;

    public static final int COUNTS = 1;
    static {
        String url = "www.atguigu.com";
    }
    {
        info = "java";
    }
    public JavapTest() {

    }
    private JavapTest(boolean falg) {
        this.flag = flag;
    }
    private void methodPrivate() {

    }
    int getNum(int i) {
        return num + i;
    }
    protected char showGender() {
        return gender;
    }
    public void showInfo() {
        int i = 100;
        System.out.println(info + i);
    }
}
```
字节码文件分析：
```java
Classfile /C:/Users/mingming/Desktop/JavapTest.class   // 字节码文件所属的路径
  Last modified 2021-2-24; size 1393 bytes   // 最后修改时间，字节码文件的大小
  MD5 checksum 2c764244fa3a95bfb346c9e416a7a3f6   // MD5散列值
  Compiled from "JavapTest.java"   // 源文件的名称
public class io.renren.JavapTest
  minor version: 0   // 副版本
  major version: 52   // 主版本
  flags: ACC_PUBLIC, ACC_SUPER   // 访问标识
*************************** 常量池********************************
Constant pool:
   #1 = Methodref          #16.#48        // java/lang/Object."<init>":()V
   #2 = String             #49            // java
   #3 = Fieldref           #15.#50        // io/renren/JavapTest.info:Ljava/lang/String;
   #4 = Fieldref           #15.#51        // io/renren/JavapTest.flag:Z
   #5 = Fieldref           #15.#52        // io/renren/JavapTest.num:I
   #6 = Fieldref           #15.#53        // io/renren/JavapTest.gender:C
   #7 = Fieldref           #54.#55        // java/lang/System.out:Ljava/io/PrintStream;
   #8 = Class              #56            // java/lang/StringBuilder
   #9 = Methodref          #8.#48         // java/lang/StringBuilder."<init>":()V
  #10 = Methodref          #8.#57         // java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
  #11 = Methodref          #8.#58         // java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
  #12 = Methodref          #8.#59         // java/lang/StringBuilder.toString:()Ljava/lang/String;
  #13 = Methodref          #60.#61        // java/io/PrintStream.println:(Ljava/lang/String;)V
  #14 = String             #62            // www.atguigu.com
  #15 = Class              #63            // io/renren/JavapTest
  #16 = Class              #64            // java/lang/Object
  #17 = Utf8               num
  #18 = Utf8               I
  #19 = Utf8               flag
  #20 = Utf8               Z
  #21 = Utf8               gender
  #22 = Utf8               C
  #23 = Utf8               info
  #24 = Utf8               Ljava/lang/String;
  #25 = Utf8               COUNTS
  #26 = Utf8               ConstantValue
  #27 = Integer            1
  #28 = Utf8               <init>
  #29 = Utf8               ()V
  #30 = Utf8               Code
  #31 = Utf8               LineNumberTable
  #32 = Utf8               LocalVariableTable
  #33 = Utf8               this
  #34 = Utf8               Lio/renren/JavapTest;
  #35 = Utf8               (Z)V
  #36 = Utf8               falg
  #37 = Utf8               MethodParameters
  #38 = Utf8               methodPrivate
  #39 = Utf8               getNum
  #40 = Utf8               (I)I
  #41 = Utf8               i
  #42 = Utf8               showGender
  #43 = Utf8               ()C
  #44 = Utf8               showInfo
  #45 = Utf8               <clinit>
  #46 = Utf8               SourceFile
  #47 = Utf8               JavapTest.java
  #48 = NameAndType        #28:#29        // "<init>":()V
  #49 = Utf8               java
  #50 = NameAndType        #23:#24        // info:Ljava/lang/String;
  #51 = NameAndType        #19:#20        // flag:Z
  #52 = NameAndType        #17:#18        // num:I
  #53 = NameAndType        #21:#22        // gender:C
  #54 = Class              #65            // java/lang/System
  #55 = NameAndType        #66:#67        // out:Ljava/io/PrintStream;
  #56 = Utf8               java/lang/StringBuilder
  #57 = NameAndType        #68:#69        // append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
  #58 = NameAndType        #68:#70        // append:(I)Ljava/lang/StringBuilder;
  #59 = NameAndType        #71:#72        // toString:()Ljava/lang/String;
  #60 = Class              #73            // java/io/PrintStream
  #61 = NameAndType        #74:#75        // println:(Ljava/lang/String;)V
  #62 = Utf8               www.atguigu.com
  #63 = Utf8               io/renren/JavapTest
  #64 = Utf8               java/lang/Object
  #65 = Utf8               java/lang/System
  #66 = Utf8               out
  #67 = Utf8               Ljava/io/PrintStream;
  #68 = Utf8               append
  #69 = Utf8               (Ljava/lang/String;)Ljava/lang/StringBuilder;
  #70 = Utf8               (I)Ljava/lang/StringBuilder;
  #71 = Utf8               toString
  #72 = Utf8               ()Ljava/lang/String;
  #73 = Utf8               java/io/PrintStream
  #74 = Utf8               println
  #75 = Utf8               (Ljava/lang/String;)V
******************************字段表集合的信息**************************************
{
  private int num;   // 字段名
    descriptor: I   // 字段表集合的信息
    flags: ACC_PRIVATE   // 字段的访问标识
 
  boolean flag;
    descriptor: Z
    flags:
 
  protected char gender;
    descriptor: C
    flags: ACC_PROTECTED
 
  public java.lang.String info;
    descriptor: Ljava/lang/String;
    flags: ACC_PUBLIC
 
  public static final int COUNTS;
    descriptor: I
    flags: ACC_PUBLIC, ACC_STATIC, ACC_FINAL
    ConstantValue: int 1   // 常量字段的属性：ConstantValue
******************************方法表集合的信息**************************************
  public io.renren.JavapTest();   // 无参构造器方法信息
    descriptor: ()V
    flags: ACC_PUBLIC
    Code:
      stack=2, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: aload_0
         5: ldc           #2                  // String java
         7: putfield      #3                  // Field info:Ljava/lang/String;
        10: return
      LineNumberTable:
        line 16: 0
        line 14: 4
        line 18: 10
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0      11     0  this   Lio/renren/JavapTest;
 
  private io.renren.JavapTest(boolean);   // 单个参数构造器方法信息
    descriptor: (Z)V
    flags: ACC_PRIVATE
    Code:
      stack=2, locals=2, args_size=2
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: aload_0
         5: ldc           #2                  // String java
         7: putfield      #3                  // Field info:Ljava/lang/String;
        10: aload_0
        11: aload_0
        12: getfield      #4                  // Field flag:Z
        15: putfield      #4                  // Field flag:Z
        18: return
      LineNumberTable:
        line 19: 0
        line 14: 4
        line 20: 10
        line 21: 18
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0      19     0  this   Lio/renren/JavapTest;
            0      19     1  falg   Z
    MethodParameters:
      Name                           Flags
      falg
 
  private void methodPrivate();
    descriptor: ()V
    flags: ACC_PRIVATE
    Code:
      stack=0, locals=1, args_size=1
         0: return
      LineNumberTable:
        line 24: 0
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0       1     0  this   Lio/renren/JavapTest;
 
  int getNum(int);
    descriptor: (I)I
    flags:
    Code:
      stack=2, locals=2, args_size=2
         0: aload_0
         1: getfield      #5                  // Field num:I
         4: iload_1
         5: iadd
         6: ireturn
      LineNumberTable:
        line 26: 0
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0       7     0  this   Lio/renren/JavapTest;
            0       7     1     i   I
    MethodParameters:
      Name                           Flags
      i
 
  protected char showGender();
    descriptor: ()C
    flags: ACC_PROTECTED
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: getfield      #6                  // Field gender:C
         4: ireturn
      LineNumberTable:
        line 29: 0
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0       5     0  this   Lio/renren/JavapTest;
 
  public void showInfo();
    descriptor: ()V   // 方法的描述符：方法的形参列表、返回值类型
    flags: ACC_PUBLIC   // 方法的访问标识
    Code:   // 方法的Code属性
      stack=3, locals=2, args_size=1   // stack：操作数栈的最大深度   locals：局部变量表的长度   args_size：方法接受参数的个数
// 偏移量  操作码   操作数
         0: bipush        100
         2: istore_1
         3: getstatic     #7                  // Field java/lang/System.out:Ljava/io/PrintStream;
         6: new           #8                  // class java/lang/StringBuilder
         9: dup
        10: invokespecial #9                  // Method java/lang/StringBuilder."<init>":()V
        13: aload_0
        14: getfield      #3                  // Field info:Ljava/lang/String;
        17: invokevirtual #10                 // Method java/lang/StringBuilder.append:(Ljava/lang/String;)Ljava/lang/StringBuilder;
        20: iload_1
        21: invokevirtual #11                 // Method java/lang/StringBuilder.append:(I)Ljava/lang/StringBuilder;
        24: invokevirtual #12                 // Method java/lang/StringBuilder.toString:()Ljava/lang/String;
        27: invokevirtual #13                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
        30: return
     // 行号表：指明字节码指令的偏移量与java源代码中代码的行号的一一对应关系
      LineNumberTable:
        line 32: 0
        line 33: 3
        line 34: 30
     // 局部变量表：描述内部局部变量的相关信息
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0      31     0  this   Lio/renren/JavapTest;
            3      28     1     i   I
 
  static {};
    descriptor: ()V
    flags: ACC_STATIC
    Code:
      stack=1, locals=1, args_size=0
         0: ldc           #14                 // String www.atguigu.com
         2: astore_0
         3: return
      LineNumberTable:
        line 11: 0
        line 12: 3
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
}
SourceFile: "JavapTest.java"   // 附加属性：指明当前字节码文件对应的源程序文件名
```
jclasslib展示的内容：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671430506465-148c4cc7-006b-48a2-8e14-b91995e81c62.png#averageHue=%23f1f1f0&clientId=u6f50dfff-ccc7-4&from=paste&height=557&id=u93b1f5e3&originHeight=836&originWidth=972&originalType=binary&ratio=1&rotation=0&showTitle=false&size=78542&status=done&style=none&taskId=u6b120c51-db0f-4df1-b40f-0f0f2420387&title=&width=648)
## 5-总结
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12600036/1671430531653-98483055-aa08-4d0d-9a46-6266939cc60f.png#averageHue=%23c2ddc7&clientId=u6f50dfff-ccc7-4&from=paste&height=244&id=u2eb72201&originHeight=366&originWidth=994&originalType=binary&ratio=1&rotation=0&showTitle=false&size=311922&status=done&style=none&taskId=u7dad59fe-005b-49b2-94b4-b70a7b3365e&title=&width=662.6666666666666)
