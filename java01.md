Eclipse Maven Error - Cannot change version of project facet Danymic Web Module to 3.0
===
<br>


##문제현상
Eclipse에서 Maven Project를 생성 후 POM.xml에 Dependency를 설정하여 Update를 하면 다음과 같은 메시지가 Problems에 뜬다

>Cannot change version of project facet Dynamic Web Module to 3.0 <br>
>One or more constraints have not been satisfied


##해결방법
1. Elipse -> Window -> Show View -> Navigator 를 활성화 시킨다. <br>
2. .settings -> `org.eclipse.wst.common.project.facet.core.xml` 파일을 열어 다음과 같이 **jst.web** 버전과 **java** 버전을 알맞게 수정한다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<faceted-project>
  <fixed facet="wst.jsdt.web"/>
  <installed facet="jst.web" version="3.0"/>
  <installed facet="wst.jsdt.web" version="1.0"/>
  <installed facet="jst.jsf" version="2.2"/>
  <installed facet="jst.jaxrs" version="2.0"/>
  <installed facet="java" version="1.8"/>
</faceted-project>
```

3. web.xml 이 아래와 같이 되어 있다면 변경한다. Apache Tomcat Version 에 따른 Servlet Spec을 확인하여 수정한다.
([Apache Tomcat Versions 참고](http://tomcat.apache.org/whichversion.html "Apache Tomcat Versions"))

**변경 전**

```xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
  <display-name>Archetype Created Web Application</display-name>
</web-app>
```

**변경 후**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" version="3.1">
    <display-name>Archetype Created Web Application</display-name>
</web-app>
```
4. pom.xml에서 maven-compiler-plugin의 configuration > source, target을 jdk 버전과 동일하게 변경한다.

```xml
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
		<version>2.3.2</version>
        <configuration>
          <source>1.8</source>
          <target>1.8</target>
		  <encoding>UTF-8</encoding>          
        </configuration>
      </plugin>
```
5. Maven -> Update Project 를 실행한다.
