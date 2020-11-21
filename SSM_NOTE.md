# **SSM(一) SayHi實作**

#### **Maven**




#### **SSM 建置**
#### **SSM 建置**

#### **Spring**

#### **SpringMVC**

#### **MyBatis**

#### **Log4j**
#### **JUnit**


# **SSM(二) 按爛功能實作**




# **SSM 補充**

#### **Tomcat**

輕量級應用服務器  
自動部屬

#### **Maven**

軟體專案管理工具，使用Pom檔管理專案建構,、關聯性，自動下載和專案相關的Libraries(函式庫)。


1. Library dependency
> [color=#4ddde2]25
> 建構時檢查pom 
repository (檔案庫)
本地.m2、中央<dependency、遠程<respository，settings.xm文件中定義存放路>徑。
>
2. Maven profile
> [color=#4ddde2]
> 軟體會面對不同的執行環境，比如開發環境(DEV)、測試環境(UAT)、生產環境，讓我們不用修改配置就能釋出到不同的環境中，比如資料來源配置、日誌檔案配置。resources目錄下spring-applicationContext.xml。jdbc.properties資料來源、logback.xm日誌等等。
> 
3. Multi-module 多模組
> [color=#4ddde2]
> 將原本一個大專案切成多個模組專案更容易維護、更好管理、更好重用。

parent下的business、總控、後台、ｍｅｍｂｅｒ。

（重用：略同程式碼，依賴war來達到共用程式碼）

新增一個新的後台專案。因為總控、後台、ｍｅｍｂｅｒ都為獨立的專案，

可各自打包成jar，新的專案可直接加到依賴中，不用再依賴一整個war。

專案的依賴可由父專案的pom來統一管理，不用每個專案各自維護自己的依賴。

而各模組仍可依需求在自己的pom設定依賴。不用每次都build整個專案，你只需要build你所在的專案即可，節省build的時間。

4. Maven plugin 插件
> [color=#4ddde2]
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

