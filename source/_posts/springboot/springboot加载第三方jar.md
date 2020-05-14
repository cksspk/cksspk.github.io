---
title: springboot使用第三方jar
p: springboot/springboot加载第三方jar
date: 2020-05-13 09:40:28
tags: [springboot,工具]
categories: springboot
---

# SpringBoot引入将第三方jar

### 一、导入jar
 <!-- more -->


- 在`dependency`中指定`jar`

  - 其中`groupId`、`artifactId`、`version`、可随意填写

  - `scope`必须为**system**

  - `systemPath`为`jar`包路径

  - `${project.basedir}`为项目根目录

    ```xml
    <!--        农行网上支付平台-->
        <dependency>
            <groupId>com.abc.ebusclient</groupId>
            <artifactId>ebusclient</artifactId>
            <version>3.1.8</version>
            <scope>system</scope>
    <!--            <systemPath>${project.basedir}/src/main/resources/lib/TrustPayClient-V3.1.8.jar</systemPath>-->
            <systemPath>${project.basedir}/lib/TrustPayClient-V3.1.8.jar</systemPath>
    	</dependency>
    ```

- 打包配置，需要在`build-plugins`中配置

  ```xml
     <!--如果是打jar包，则需在build的plugins中添加如下配置-->
      <plugin>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-maven-plugin</artifactId>
          <configuration>
              <!--值为true是指打包时包含scope为system的第三方Jar包-->
              <includeSystemScope>true</includeSystemScope>
          </configuration>
      </plugin>	
  ```

  

### 二、指定classptsh下配置文件

​		第三方`jar`需要指定位置配置文件，比如**classpath**，在`Springboot`中，`main/resources`目录为默认**classpath**

​		当通过<includeSystemScope>true</includeSystemScope>无法打入`jar`，可通过**pom.xml**文件中的**resource**指定，配置如下：	

```xml
<resources>
    <!-- 必须配置，将配置文件打包到classpath-->
    <resource>
        <directory>src/main/resources</directory>
    </resource>

    <!-- 必须，否则打包时无法将jar打入指定lib-->
    <resource>
        <!--<directory>src/main/resources/lib</directory>-->
        <directory>${project.basedir}/lib</directory>
        <targetPath>BOOT-INF/lib/</targetPath>
        <includes>
            <include>**/*.jar</include>
        </includes>
    </resource>
</resources>
```

