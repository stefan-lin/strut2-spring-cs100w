## Project : Conversion from Struts2 to Spring
####Team Sprintklers
**Author**:
  1. Alex Heritier
  2. Jeremy Asuncion
  3. Phillip Hui
  4. Sridivya Kondapali
  5. Stefan Lin

---
### Introduction - Project Environment Setup Guide
This instruction is targeting **Linux** users. Linux with _kernel 3+_ would be able to set up the project environment without any struggle.

### Hardware Requirements
| Name | Min. Requirements |
|:----:|:-------------|
| CPU | Dual-core 1.3 GHz + |
| RAM/Memory | 2 GB |
| Disk/Storage | 5 GB |

### Software Requirements
| Name | Min. Requirements |
|:----:|:------------------|
| Java | version 8+ |

---
### Preparation
You could get all the necessary packages from the following URLs or the attached compressed zip file.

##### Download URLs
| Package | URL |
|:-------:|:----|
| Eclipse IDE (EE) | http://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/neon1a |
| Apache Tomcat 7 | http://apache.mirrors.lucidnetworks.net/tomcat/tomcat-7/v7.0.72/bin/apache-tomcat-7.0.72.zip |
| Struts2 | http://apache.mirrors.hoobly.com/struts/2.5.5/struts-2.5.5-all.zip |
| Spring | Embed in XML file [**reference 1**]|
| Apache Maven | http://apache.mirrors.tds.net/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.zip |

##### Reference #1
Spring framework is using Apache Maven to port framework into project.
To include Spring framework,
  1. open your project directory and edit `pom.xml`
  2. within `<project>` elements, insert the following code block
      ```xml
      <dependencies>
		  <dependency>
			  <groupId>org.springframework</groupId>
			  <artifactId>spring-context</artifactId>
			  <version>4.3.3.RELEASE</version>
		  </dependency>
      </dependencies>
      ```
  3. done; You are now able to use Spring framework in your project
---
### Pre-Installation (Ubuntu)
  * **Java JDK**
    1. Open **terminal** of your choice, and enter following commands
    2. Update libraries
        * `sudo apt-get update`
    3. Validate Java version
        * `java -version`
        * If you get something like `java version "1.8.x_xx"` as the response, then you have Java installed on your machine. You can skip to the **environment variable** step
    4. Install Java Runtime Environment (JRE)
        * `sudo apt-get install default-jre`
    5. Install Java Development Kit (JDK)
        * `sudo apt-get install default-jdk`
        * If you wish to switch between different versions of JDK
          * `sudo update-alternatives --config java`
  * **Set up Java Environment Variable**
    * We need to set up `$JAVA_HOME` for Tomcat 7!
    * In terminal,
      * `readlink -f $(which java)`
        * The output will be something like: `/usr/lib/jvm/java-1.8.0-openjdk-amd64/bin/java`
        * Copy this part of path `/usr/lib/jvm/java-1.8.0-openjdk-amd64` (not including `/bin/java`)
      * `sudo vi /env/environment`
        * This will open a VI editor
          1. click `i` to be in _editing mode_
          2. enter `JAVA_HOME="/usr/lib/jvm/java-1.8.0-openjdk-amd64"`
          3. click ESC key to exit _editing mode_
          4. enter `:wq` to save and quit VI editor
      * Now we need to export the JAVA_HOME environment variable
        * Enter `source /etc/environment`
      * Validate JAVA_HOME by `echo $JAVA_HOME`
        * There should be a directory path as response

---
### Eclipse IDE Installation
The downloaded Eclipse IDE would be either in `.tar.gz` or `.zip` extensions. We need to extract the downloaded file first.
**Extraction**  
  * For tar ball (`.tar.gz`), enter following command to terminal
    * `tar -zvxf ~/Download/eclipse-xxxxxx.tar.gz`
    * The Eclipse will be extracted into a separated folder
    * Run Eclipse installer
      * Choose **Eclipse IDE for Java EE Developers**
      * Click 'Install' (You can change installation path)
      * Accept license and certification during installation
---
### Apache Tomcat 7 Installation
The simplest way to set up Tomcat on Ubuntu is using `apt-get`. Please follow the steps below:
  1. Update libraries: `sudo apt-get update`
  2. Download and install `sudo apt-get install tomcat7`
  3. (Optional)
      * Modify the permitted memory for Tomcat
        * `sudo vi /etc/default/tomcat7`
        * Change line which starts with `JAVA_OPT`:  
         `JAVA_OPTS="-Djava.security.egd=file:/dev/./urandom -Djava.awt.headless=true -Xmx512m -XX:MaxPermSize=256m -XX:+UseConcMarkSweepGC"`
        * Feel free to change the `Xmx` and `MaxPermSize` values—these settings affect how much memory Tomcat will use
  4. Tomcat service commands
      * **restart server**: `sudo service tomcat7 restart`
      * **start server**: `sudo service tomcat7 start`
      * **stop server**: `sudo service tomcat7 stop`
  5. Port Number: **8080** by default
      * Local host example: `http://localhost:8080`
  6. (Optional) Install additional package for Tomcat
      `sudo apt-get install tomcat7-docs tomcat7-admin tomcat7-examples`
  7. (Optional) Configure Tomcat users
      * Enter `sudo nano /etc/tomcat7/tomcat-users.xml`
      * Edit this part   
      ```xml
      <tomcat-users>
        <user username="admin"
              password="password"
              roles="manager-gui,admin-gui"/>
      </tomcat-users>
      ```
  8. Tomcat Web Application Manager
      * Go to `http://server_IP_address:8080/manager/html`
      * Host Admin `http://server_IP_address:8080/host-manager/html/`
  * **NOTE**
    * In ubuntu, we don't need to set $CATALINA_HOME environment variable (required in MacOS)

--
### Maven Installation
Using `apt-get` to install Maven 3.x is the simplest way.
  * enter `sudo apt-get install maven`
  * validate `mvn --version`

---
### Struts2 Installation
Just make sure:
  1. **Apache Maven** is installed:
      * `mvn --version`
  2. Insert struts2 dependency to your project
      * ```xml
        <dependency>
          <groupId>org.apache.struts</groupId>
          <artifactId>struts2-core</artifactId>
          <version>[X.X.X.X]</version>
        </dependency>
        ```

---
### MySQL Installation
  * `sudo apt-get install mysql-server`

---

### Run Project
  1. Import project into Eclipse
  2. Open `pom.xml` (under Presto folder, or root folder)
  3. Replace
  ```xml
  <dependency>
      <groupId>com.sun</groupId>
      <artifactId>tools</artifactId>
      <version>1.5.0</version>
  </dependency>
  ```
  with  
  ```xml
  <dependency>
      <groupId>com.sun</groupId>
      <artifactId>tools</artifactId>
      <version>${java.version}</version>
      <scope>system</scope>
      <systemPath>${java.home}/../lib/tools.jar</systemPath>
  </dependency>
  ```
  4. Right click on `Presto` on the project navigation (on the left hand side) and select `Run As` --> `Maven clear`
  5. Right click on `Presto` on the project navigation and select `Run As` --> `Run on Server`

---
---
# Conversion Instructions
---
---
### Conversion Instruction from Struts2 to Spring
####Team Sprintklers
**Author**:
  1. Alex Heritier
  2. Jeremy Asuncion
  3. Phillip Hui
  4. Sridivya Kondapali
  5. Stefan Lin

---
### Introduction
We are targeting to replace all Struts2 framework libraries with Spring framework libraries, change `web.xml` file to Spring framework, alter configuration files into Spring framework, modify JSP files, and make changes to Action class.

---

### Struts2 libraries to Spring libraries
This task should be done with **Maven Build** (once we setup the dependency in XML). However, the following statement will show the basic libraries in Struts2 framework and Spring framework.

| Framework | Basic Libraries |
|:---------:|:----------------|
| Struts2 | struts.jar, struts-legacy.jar ... etc |
| Spring | standard.jar, org.springframework.asm-4.0.1.RELEASE-A.jar ... etc |

---
### Changes in web.xml
**web.xml** in _Struts2_
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<web-app
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns="http://java.sun.com/xml/ns/javaee"
    xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
    xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
    http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
    id="WebApp_ID"
    version="3.0">  
  <display-name>Struts2MyFirstApp</display-name>  
  <filter>  
    <filter-name>struts2</filter-name>  
    <filter-class>  
      org.apache.struts2.dispatcher.FilterDispatcher  
    </filter-class>  
  </filter>  

  <filter-mapping>  
    <filter-name>struts2</filter-name>  
    <url-pattern>/*</url-pattern>  
  </filter-mapping>  
  <welcome-file-list>  
    <welcome-file>Login.jsp</welcome-file>  
  </welcome-file-list>  
</web-app>
```
**web.xml** in _Spring_
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<web-app
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns="http://java.sun.com/xml/ns/javaee"
    xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
    http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
    id="WebApp_ID"
    version="3.0">  
  <display-name>springApp</display-name>  
  <servlet>  
    <servlet-name>springApp</servlet-name>  
    <servlet-class>  
      org.springframework.web.servlet.DispatcherServlet  
    </servlet-class>  
    <load-on-startup>1</load-on-startup>  
  </servlet>  
  <servlet-mapping>  
    <servlet-name>springApp</servlet-name>  
    <url-pattern>/</url-pattern>  
  </servlet-mapping>  
</web-app>  
```
The xml tags we are interesting in are:
| Struts2 tag | Spring tag |
|:-----------:|:----------:|
| `<filter> </filter>` | `<servlet> </servlet>` |
| `<filter-name> </filter-name>` | `<servlet-name> </servlet-name>` |
| `<filter-class> </filter-class>` | `<servlet-class> </servlet-class>` |
| `<filter-mapping> </filter-mapping>` | `<servlet-mapping> </servlet-mapping>` |

---
### Modification on Configuration Files
Configuration file in _Struts2_
```xml
<struts>  
  <constant
      name="struts.enable.DynamicMethodInvocation"
      value="false"/>  
  <constant name="struts.devMode" value="false" />  
  <constant name="struts.custom.i18n.resources" value="myapp" />  

  <package
      name="default"
      extends="struts-default"
      namespace="/">  
    <action
        name="login"
        class="com.example.struts2.login.LoginAction">  
      <result name="success">Welcome.jsp</result>  
      <result name="error">Login.jsp</result>  
    </action>  
  </package>  
</struts>  
```
Configuration file in _Spring_
```xml   
<beans
    xmlns="http://www.springframework.org/schema/beans"    
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:p="http://www.springframework.org/schema/p"      
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"    
    xsi:schemaLocation=
        "http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans-4.0.xsd  
         http://www.springframework.org/schema/context    
         http://www.springframework.org/schema/context/spring-context-4.0.xsd">
  <context:component-scan  
      base-package="com.example.spring.login.controller" />    
  <bean
      id="viewResolver"  
      class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix">    
      <value>/WEB-INF/views/</value>    
    </property>    
    <property name="suffix">    
      <value>.jsp</value>    
    </property>    
  </bean>    
</beans>
```
Over here, we are replacing `<struts>` with `<beans>`. And `context:component-scan>` tag will inform Spring to scan through the package and load all the components in it.

---
### JSP Changes
Here are two parts needed to be modified.
  1. All **.tld** files:   
  ```xml
  <%@ taglib
      uri="http://struts.apache.org/tags-bean"
      prefix="bean" %>  
  <%@ taglib
      uri="http://struts.apache.org/tags-html"
      prefix="html" %>  
  <%@ taglib
      uri="http://struts.apache.org/tags-logic"
      prefix="logic" %>  
  <%@ taglib
      uri="http://struts.apache.org/tags-tiles"
      prefix="tiles" %>
  ```
  Convert them into Spring
  ```xml
  <%@ taglib
      prefix="form"
      uri="http://www.springframework.org/tags/form"%>  
  <%@ taglib
      prefix="spring"
      uri="http://www.springframework.org/tags"%>
  ```

  2. In HTMLs

  | Struts2 | Spring |
  |:--------|:-------|
  | `<html:form action="/addLogin" method="post">` | `<form:form method="POST" commandName="loginForm" name="loginForm" action="login.do">` |

---
### Action Class Modification
**Struts2**
```java
package com.example.struts2.login;  
import com.opensymphony.xwork2.ActionSupport;  

@SuppressWarnings("serial")  
public class LoginAction  extends ActionSupport{  
  private String username;  
    private String password;  

  public String execute() {  
    if (this.username.equals("usrname")   
        &&
        this.password.equals("password")) {  
      return "success";  
    }
    else {  
      addActionError(getText("error.login"));  
      return "error";  
    }  
  }  // END execute METHOD

  public String getUsername() {  
    return username;  
  }  

  public void setUsername(String username) {  
    this.username = username;  
  }  

  public String getPassword() {  
    return password;  
  }  

  public void setPassword(String password) {  
    this.password = password;  
  }  
}  
```
**Spring**
```java
package com.example.spring.login.controller;  
import org.springframework.stereotype.Controller;  
import org.springframework.ui.ModelMap;  
import org.springframework.web.bind.annotation.RequestMapping;  
import org.springframework.web.bind.annotation.RequestMethod;  

@Controller  
public class LoginController {  

  @RequestMapping(value="/login.do", method = RequestMethod.GET)  
  public String doLogin(ModelMap model, LoginForm loginForn) {  
    if (this.username.equals("usrname")   
        && this.password.equals("password")) {  
      model.addAttribute("message", "Login Success");  
    }
    else {  
      model.addAttribute("message", "Login Failure");  
    }  
    return "home";  
  }  
}
```

---
### Validate and Test
**JSP Validation Changes**
```xml
<!-- Struts2 -->
<%  
  ActionErrors actionErrors
  (ActionErrors)request.getAttribute(
    "org.apache.struts.action.ERROR"
  );  
%>
```
```xml
<!-- Spring -->
<form:errors path="*" cssClass="error" />
```
