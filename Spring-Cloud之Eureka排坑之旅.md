##1 快速demo
###1.0 环境说明
&emsp;&emsp;Intelli IDEA+Spring Boot
###1.1 新建工程chap52（通过New Project->Spring Initializer->web）	
修改pom文件：
	
	<groupId>com.chen4du</groupId>
	<artifactId>chap52</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>pom</packaging>

	<name>chap52</name>
	<description>Demo project for Spring Boot</description>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.2.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
		<spring-cloud.version>Dalston.SR1</spring-cloud.version>
	</properties>

	<dependencyManagement>
		<dependencies>
			<dependency>
				<groupId>org.springframework.cloud</groupId>
				<artifactId>spring-cloud-dependencies</artifactId>
				<version>${spring-cloud.version}</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
		</dependencies>
	</dependencyManagement>
###1.2 新建module：eurekaserver（通过New Project->Spring Initializer->web），修改pom文件
	
	<parent>
		<groupId>com.chen4du</groupId>
		<artifactId>chap52</artifactId>
		<version>0.0.1-SNAPSHOT</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-eureka-server</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>
添加application.yml文件
	
    server:
      port: 8761
    
    eureka:
      instance:
    hostname: localhost
      client:
    	registerWithEureka: false
    	fetchRegistry: false
    	serviceUrl:
      		defaultZone:
    			http://${eureka.instance.hostname}:${server.port}/eureka/
为EurekaserverApplication文件添加注解
	
    @EnableEurekaServer
    @SpringBootApplication
    public class EurekaserverApplication {
    
    	public static void main(String[] args) {
    
    		SpringApplication.run(EurekaserverApplication.class, args);
    	}
    }
###1.3 新建module：eurekaclient（通过New Project->Spring Initializer->web），修改pom文件
	
	<parent>
		<groupId>com.chen4du</groupId>
		<artifactId>chap52</artifactId>
		<version>0.0.1-SNAPSHOT</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-eureka</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>
添加文件bootstrap.yml
	
    eureka:
     	client:
    		serviceUrl:
      			defaultZopne: http://localhost:8761/eureka/
    server:
      	port: 8762
    spring:
      	application:
    		name: eureka-client
为EurekaclientApplication添加注解
	
    @SpringBootApplication
    @EnableEurekaClient
    public class EurekaclientApplication {
    
    	public static void main(String[] args) {
    		SpringApplication.run(EurekaclientApplication.class, args);
    	}
    }
###1.4 调试
运行EurekaserverApplication，访问http:localhost:8761
运行EurekaclientApplication，访问http:localhost:8761 可以看到服务已注册

##2 排坑之旅
###2.1 注解添加无效
开始以为是包之间冲突，各种调版本，后来发现是因为chap52的pom文件中打包方式有误，默认是jar，要改成pom，后续子模块才能依赖到父模块的包
###2.2 yml文件格式
注意value和key之间有个空格，勿丢。

