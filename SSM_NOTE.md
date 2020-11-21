# **SSM(一) SayHi實作**

#### **Maven.pom配置**
```
最終版
```

#### **Log4j**
> [color=#4ddde2]log4j.xml
```

### log levels
log4j.rootLogger=DEBUG, CONSOLE
### output
log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender
log4j.appender.CONSOLE.Target=System.out
log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout
log4j.appender.CONSOLE.layout.ConversionPattern=[%-5p] %d{HH:mm:ss} (%F:%L) - %m%n
```



#### **JUnit**
> [color=#4ddde2]pom
```
//spring添加junit測試環境
@RunWith(SpringJUnit4ClassRunner.class)
//加載配置文件
@ContextConfiguration(locations = {"classpath:app-context.xml"})
@WebAppConfiguration() //設定web專案的環境，如果是Web專案，必須配置該屬性，否則無法獲取
public class BaseTest {

}
```


#### **Spring**

> [color=#4ddde2]WEB-INF/web.xml
```
<!-- Spring ApplicationContext配置文件的路径,可使用通配符，用于后面的Spring Context Loader -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath*:/app-context.xml</param-value>
		<!-- classpath：只会到你的class路径中查找找文件; classpath*：不仅包含class路径，还包括jar文件中(class路径)进行查找. -->
	</context-param>
	<!--ContextLoaderListener 啟動Web容器 自動Spring ApplicationContext載入 -->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
<!-- Spring end -->
```
> [color=#4ddde2]resources/app-context.xml
```
<context:component-scan base-package="com.lin" />

```


> [color=#4ddde2]test

```

public class SpringTest extends BaseTest {

	// @Transactional //標明此方法需使用事務
	@Test
	public void springTest() {
		// 要public 不能void  say() 也要加註解 
		//整個專案 run as  junit test
		String path = "classpath*:/app-context.xml";
		ApplicationContext ac = new ClassPathXmlApplicationContext(path);
		SpringTest st = (SpringTest) ac.getBean(SpringTest.class);
		st.say();
	}

	@Test
	public void say() {
		System.out.println("Hi~~~");

	}

}

```

> [color=#4ddde2]啟動 : 整個專案 run as  junit test

#### **SpringMVC**

> [color=#4ddde2]WEB-INF/web.xml
```
<!-- Spring MVC DispatcherServlet -->
	<servlet>
		<servlet-name>springServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>/WEB-INF/spring-mvc.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
		<async-supported>true</async-supported>
	</servlet>
	<servlet-mapping>
		<servlet-name>springServlet</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
<!-- Spring MVC DispatcherServlet end -->

```
> [color=#4ddde2]WEB-INF/spring-mvc.xml

```

	<!-- 自动扫描 -->
	<context:component-scan base-package="com.lin.controller" />
	<!-- 註冊 -->
	<mvc:annotation-driven />

	<!-- 視圖解析器 控制層回傳的字串+前後綴=jsp -->
	<bean
		class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/views/" />
		<property name="suffix" value=".jsp" />
	</bean>

```
> [color=#4ddde2]controller

```
//控制器
@Controller
//處理請求地址映射
@RequestMapping("/HI")
public class HI {
	//組合註解 = @RequestMapping(method=RequestMethod.GET)
	@GetMapping("/haha")
	public String hI(){
		System.out.println("MVC Hi");
		return "hi" ;
	}
	
	

```

> [color=#4ddde2]啟動 : 整個專案 run as maven build 
> 網址輸入: 127.0.0.1:8186/HI/haha 即可轉發至 WEB-INF/views/hi.jsp


#### **MyBatis**
> [color=#4ddde2]resources/app-context.xml
```
<!-- 數據庫 -->
	<!-- ignore-unresolvable 忽略找不到的屬性 -->
	<context:property-placeholder location="classpath:jdbc.properties"
		ignore-unresolvable="true" />

	<!-- 數據源 -->
	<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
		init-method="init" destroy-method="close">
		<property name="driverClassName" value="${jdbc.driverClassName}"></property>
		<property name="url" value="${jdbc.url}"></property>
		<property name="username" value="${jdbc.username}"></property>
		<property name="password" value="${jdbc.password}"></property>
	</bean>

	<!-- sqlSessioFactory  创建SqlSession实例的工厂-->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<!-- 注入 -->
		<property name="dataSource" ref="dataSource"></property>
		<!-- 掃描 mapper需要的文件 -->
		<property name="mapperLocations" value="classpath*:mapper/*.xml"></property>
	</bean>


	<!--掃描所有@mybatisDao註解接口 --><!-- value="sqlSession" -->
	<bean id="mapperScannerConfigurer" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
		<property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"></property>   
		<property name="basePackage" value="com.lin.dao"></property>
	</bean>


```
> [color=#4ddde2]resources/jdbc.properties
```
jdbc.driverClassName=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql:///test_user02?&serverTimezone=GMT
jdbc.username=root
jdbc.password=qwe123



```
> [color=#4ddde2]MySQL 表單

```

```

> [color=#4ddde2]物件

```

public class User implements Serializable {

	/**
	 * 
	 */
	private static final long serialVersionUID = 1L;
	
	private Integer id;
	private String name;
	private String password;

	public User() {
		super();
	}

	public Integer getId() {
		return id;
	}

	public void setId(Integer id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getPassword() {
		return password;
	}

	public void setPassword(String password) {
		this.password = password;
	}

	@Override
	public String toString() {
		return "User [id=" + id + ", name=" + name + ", password=" + password + "]";
	}

}

```
> [color=#4ddde2]dao

```
@Repository
public interface IUserDao {

	List<User> findAll();
	
}

```
> [color=#4ddde2]service

```
public interface IUserService {

	List<User> findAll();
	
	
}


```
> [color=#4ddde2]service實作

```

@Service
public class UserServiceImpl implements IUserService {

	@Resource
	private IUserDao userdao;
	
	
	public List<User> findAll() {
		return userdao.findAll();
	}

}

```
> [color=#4ddde2]controller

```

@Controller
@RequestMapping(value="/user")
public class UserController {
	@Resource
	private IUserService userservice;
	
	@GetMapping("/findAll")
	public String finAll(){
		List<User> userlist= userservice.findAll();
		for(User user:userlist){
			System.out.println("Id:"+user.getId()+"|"+"Name:"+user.getName());
		}
		return "hi";
	}

}
```
> [color=#4ddde2]resources/mapper/*Dao.xml

```
<mapper namespace="com.lin.dao.IUserDao">
	<sql id="user_Column">
	id,name,password
	</sql>
	
	<select id="findAll" resultType="com.lin.domain.User">
	select 
	<include refid="user_Column"/>
	from user
	</select>

</mapper>
```
> [color=#4ddde2]啟動 :
/user/findAll
轉跳頁面+撈出資料
(.jsp頁面呈現在下一個組題這裡先syso 印 資料庫數據)



# **SSM(二) 按爛功能實作**
#### **持久化類**

> [color=#6f6f6f]user
```

```
> [color=#4ddde2]web.xml
```

```
> [color=#4ddde2]test

```

```

#### **數據表**

| id | Column 2 | Column 3 |
| -------- | -------- | -------- |
| Text     | Text     | Text     |


| id  | Column 2 | Column 3 |
| -------- | -------- | -------- |
| Text     | Text     | Text     |


| id  | user_id | mood_id |
| -------- | -------- | -------- |
| Text     | Text     | Text     |






#### **DAO**
> [color=#4ddde2]pom
```

```
> [color=#4ddde2]web.xml
```

```
> [color=#4ddde2]test

```

```

#### **映射文件**
> [color=#4ddde2]pom
```

```
> [color=#4ddde2]web.xml
```

```
> [color=#4ddde2]test

```

```

#### **Service**
> [color=#4ddde2]pom
```

```
> [color=#4ddde2]web.xml
```

```
> [color=#4ddde2]test

```

```

#### **DTO**
> [color=#4ddde2]pom
```

```
> [color=#4ddde2]web.xml
```

```
> [color=#4ddde2]test

```

```


#### **Controller**
> [color=#4ddde2]pom
```

```
> [color=#4ddde2]web.xml
```

```
> [color=#4ddde2]test

```

```

#### **JSP**
> [color=#4ddde2]pom
```

```
> [color=#4ddde2]web.xml
```

```
> [color=#4ddde2]test

```

```



# **SSM 補充**

#### **Tomcat**
> [color=#4ddde2]
> Tomcat  
輕量級應用服務器  
自動部屬

#### **Maven**

軟體專案管理工具，使用Pom檔管理專案建構,、關聯性，自動下載和專案相關的Libraries(函式庫)。

> [color=#4ddde2]Library dependency
> 建構時檢查pom 
repository (檔案庫)
本地.m2、中央<dependency、遠程<respository，settings.xm文件中定義存放路>徑。
>

> [color=#4ddde2] Maven profile
> 軟體會面對不同的執行環境，比如開發環境(DEV)、測試環境(UAT)、生產環境，讓我們不用修改配置就能釋出到不同的環境中，比如資料來源配置、日誌檔案配置。resources目錄下spring-applicationContext.xml。jdbc.properties資料來源、logback.xm日誌等等。
> 

> [color=#4ddde2]Multi-module 多模組
> 將原本一個大專案切成多個模組專案更容易維護、更好管理、更好重用。

parent下的business、總控、後台、ｍｅｍｂｅｒ。

（重用：略同程式碼，依賴war來達到共用程式碼）

新增一個新的後台專案。因為總控、後台、ｍｅｍｂｅｒ都為獨立的專案，

可各自打包成jar，新的專案可直接加到依賴中，不用再依賴一整個war。

專案的依賴可由父專案的pom來統一管理，不用每個專案各自維護自己的依賴。

而各模組仍可依需求在自己的pom設定依賴。不用每次都build整個專案，你只需要build你所在的專案即可，節省build的時間。


> [color=#4ddde2]Maven plugin 插件
> 
編譯插件
 打包插件

部署插件

tomcat插件；自動部屬

多模組

Parant pom，tomcat插件；定義版本

Member、Control、Platform，tomcat插件:定義路徑、Port號





#### **SSM**
SSM框架就是使用Spring、SpringMVC、MyBatis 等
#### **Spring**
降低開發的複雜性、增加開發效率，達到高內聚力、鬆耦合。使用分層架構可以自由選用元件。

1. Data Access/Integration 資料存取/整合
> [color=#4ddde2]
> jdbc樣板  
> orm：hibernate、jpa、mybatis  
> oxm：object <-> xml 映射  
> jms：異步通信  
> transaction：管理事務
2. Web
> [color=#4ddde2]
> １２３
> websocket、 servlet 、web、 portlet
3. AOP、Aspects
> [color=#4ddde2]
> １２３
> 
4. 核心容器
> [color=#4ddde2]
> １３
> spring-beans、spring-core、 spring-context、spring-spel
> 

5. Test
> [color=#4ddde2]
> １３
> 




-Spring MVC

建構Web應用程式全功能MVC模組




 




#### **IOC AOP**
#### **MVC 常用註解**
#### **MVC**
#### **MyBatis 映射與動態SQL**
持久層框架
#### **MyBatis 緩存**
#### **MyBatis與JPA**
#### **SSM 分頁 校驗 事務 **

