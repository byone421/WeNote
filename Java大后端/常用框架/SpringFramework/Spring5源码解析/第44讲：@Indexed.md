Spring5提供用于优化加快组件扫描速度。加速程序启动
```java
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context-indexer</artifactId>
    <optional>true</optional>
</dependency>
```
在编译阶段加入到spring.components文件中，
比如在@Component上面就有一个@Indexed
```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Indexed
public @interface Component {

	/**
	 * The value may indicate a suggestion for a logical component name,
	 * to be turned into a Spring bean in case of an autodetected component.
	 * @return the suggested component name, if any (or empty String otherwise)
	 */
	String value() default "";

}
```
# 总结

1. 在编译时就根据 [@Indexed ](/Indexed ) 生成 META-INF/spring.components 文件 
2. 扫描时 
   - 如果发现 META-INF/spring.components 存在, 以它为准加载 bean definition
   - 否则, 会遍历包下所有 class 资源 (包括 jar 内的)
3. 解决的问题，在编译期就找到 [@Component ](/Component ) 组件，节省运行期间扫描 [@Component ](/Component ) 的时间 
