# MVC
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.pk</groupId>
  <artifactId>MVCtest</artifactId>
  <packaging>war</packaging>
  <version>0.0.1-SNAPSHOT</version>
  <name>Employee-Management-Systems Maven Webapp</name>
  <url>http://maven.apache.org</url>

  <dependencies>

    <!-- ✅ Spring Web MVC (Updated to Jakarta EE) -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>6.0.12</version> <!-- Updated for Jakarta compatibility -->
    </dependency>

    

    <!-- ✅ MySQL Connector -->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.30</version>
    </dependency>

    <!-- ✅ Spring JDBC -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>6.0.12</version>
    </dependency>

    <!-- ✅ Hibernate ORM -->
    <dependency>
      <groupId>org.hibernate</groupId>
      <artifactId>hibernate-core</artifactId>
      <version>6.3.1.Final</version> <!-- Latest Hibernate -->
    </dependency>

    <!-- ✅ Spring ORM -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-orm</artifactId>
      <version>6.0.12</version>
    </dependency>

    

    <!-- ✅ JSTL (Jakarta Edition) -->
    <dependency>
      <groupId>jakarta.servlet.jsp.jstl</groupId>
      <artifactId>jakarta.servlet.jsp.jstl-api</artifactId>
      <version>2.0.0</version>
    </dependency>

    <!-- ✅ JUnit 5 -->
    <dependency>
      <groupId>org.junit.jupiter</groupId>
      <artifactId>junit-jupiter-api</artifactId>
      <version>5.8.2</version>
      <scope>test</scope>
    </dependency>

    <!-- JUnit Engine -->
    <dependency>
      <groupId>org.junit.jupiter</groupId>
      <artifactId>junit-jupiter-engine</artifactId>
      <version>5.8.2</version>
      <scope>test</scope>
    </dependency>

  </dependencies>

  <build>
    <finalName>MVCtest</finalName>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-war-plugin</artifactId>
        <version>3.4.0</version>
      </plugin>
    </plugins>
  </build>

</project>

--------------------------------- spring-servlet.xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="  
        http://www.springframework.org/schema/beans  
        http://www.springframework.org/schema/beans/spring-beans.xsd  
        http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd
        http://www.springframework.org/schema/context  
        http://www.springframework.org/schema/context/spring-context.xsd  
        http://www.springframework.org/schema/mvc  
        http://www.springframework.org/schema/mvc/spring-mvc.xsd">

	<context:component-scan base-package="com.pk.*" />
	<bean id="viewResolver"
		class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/pages/" />
		<property name="suffix" value=".jsp" />
	</bean>


	<bean id="ds"
		class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName"
			value="com.mysql.cj.jdbc.Driver"></property>
		<property name="url"
			value="jdbc:mysql://localhost:3306/employees"></property>
		<property name="username" value="root"></property>
		<property name="password" value="root"></property>
	</bean>
	<bean id="sessionFactory"
		class="org.springframework.orm.hibernate5.LocalSessionFactoryBean">
		<property name="dataSource" ref="ds"></property>
		<property name="hibernateProperties">
			<props>
				<prop key="hibernate.dialect">org.hibernate.dialect.MySQL8Dialect</prop>
				<prop key="hibernate.hbm2ddl.auto">update</prop>
				<prop key="hibernate.show_sql">true</prop>
			</props>
		</property>
		<property name="packagesToScan" value="com.pk.*" />

	</bean>





</beans>
---------------------------- web.xml
<!DOCTYPE web-app PUBLIC
 "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
 "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
	<display-name>Archetype Created Web Application</display-name>
	<servlet>
		<servlet-name>spring</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>


	</servlet>
	<servlet-mapping>
		<servlet-name>spring</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>

</web-app>
---------------------------- codebase mvc
package com.cetpa.config;

import java.util.Properties;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.jdbc.datasource.DriverManagerDataSource;
import org.springframework.orm.hibernate5.LocalSessionFactoryBean;
import org.springframework.web.servlet.ViewResolver;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurerAdapter;
import org.springframework.web.servlet.view.InternalResourceViewResolver;

@EnableWebMvc
@Configuration
@ComponentScan(basePackages = {"com.cetpa"})
public class ServletConfig extends WebMvcConfigurerAdapter
{
	public void addResourceHandlers(ResourceHandlerRegistry registry) 
	{
	        registry.addResourceHandler("/CSS/**").addResourceLocations("/CSS/");
	        registry.addResourceHandler("/JS/**").addResourceLocations("/JS/");
	}
	@Bean
	public ViewResolver getViewResolver()
	{
		InternalResourceViewResolver rs=new InternalResourceViewResolver();
		rs.setPrefix("/WEB-INF/pages/");
		rs.setSuffix(".jsp");
		return rs;
	}
	private DriverManagerDataSource getSource()
	{
		DriverManagerDataSource ds=new DriverManagerDataSource();
		ds.setDriverClassName("com.mysql.cj.jdbc.Driver");
		ds.setUrl("jdbc:mysql://localhost/mvc4");
		ds.setUsername("root");
		ds.setPassword("mysql");
		return ds;
	}
	@Bean
	public LocalSessionFactoryBean getFactory()
	{
		System.out.println("Session factory created...");
		LocalSessionFactoryBean factory=new LocalSessionFactoryBean();
		factory.setDataSource(getSource());
		factory.setHibernateProperties(getProperties());
		factory.setPackagesToScan("com.cetpa.models");
		return factory;
	}
	public Properties getProperties()
	{
		Properties p=new Properties();
		p.put("hibernate.dialect", "org.hibernate.dialect.MySQL5Dialect");
		p.put("hibernate.hbm2ddl.auto","update");
		p.put("hibernate.show_sql", true);
		return p;
	}
}
-------- webcong
package com.cetpa.config;

import org.springframework.web.servlet.support.AbstractAnnotationConfigDispatcherServletInitializer;

public class WebConfig extends AbstractAnnotationConfigDispatcherServletInitializer 
{
	protected Class<?>[] getRootConfigClasses() 
	{
		return null;
	}
	protected Class<?>[] getServletConfigClasses() 
	{
		return new Class[] {ServletConfig.class};
	}
	protected String[] getServletMappings() 
	{
		return new String [] {"/"};
	}
}

