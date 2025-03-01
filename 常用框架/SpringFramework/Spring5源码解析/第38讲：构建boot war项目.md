# Boot War项目
## 步骤1：创建模块，区别在于打包方式选择 war
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1690296455228-58c78347-ea0a-49a9-af4a-1b2b7c795584.png#averageHue=%23f4f3f3&clientId=u45965795-7bde-4&from=paste&height=467&id=uc9122c7e&originHeight=700&originWidth=833&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=116436&status=done&style=none&taskId=ubb62fc5e-25d2-4364-b2a5-2cc579d25d5&title=&width=555.3333333333334)
接下来勾选 Spring Web 支持
![image.png](https://cdn.nlark.com/yuque/0/2023/png/12600036/1690296474910-408fb6d7-b46e-4bcc-b7e9-4bf3022b49dc.png#averageHue=%23f6f6f6&clientId=u45965795-7bde-4&from=paste&height=475&id=u6f9bd75f&originHeight=713&originWidth=834&originalType=binary&ratio=1.5&rotation=0&showTitle=false&size=146581&status=done&style=none&taskId=u4db0a40e-d2a6-4c04-965a-e6ac004539e&title=&width=556)
## 步骤2：编写控制器
```java
@Controller
public class MyController {

    @RequestMapping("/hello")
    public String abc() {
        System.out.println("进入了控制器");
        return "hello";
    }
}
```
## 步骤3：编写 jsp 视图，新建 webapp 目录和一个 hello.jsp 文件，注意文件名与控制器方法返回的视图逻辑名一致
```java
src
	|- main
		|- java
		|- resources
		|- webapp
			|- hello.jsp
```
## 步骤4：配置视图路径，打开 application.properties 文件
```java
spring.mvc.view.prefix=/
spring.mvc.view.suffix=.jsp
```
```properties
spring.mvc.view.prefix=/
spring.mvc.view.suffix=.jsp
```

> 将来 prefix + 控制器方法返回值 + suffix 即为视图完整路径


