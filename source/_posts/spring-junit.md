---
title: Spring整合junit单元测试
date: 2018-03-22 08:59:31
tags: JUNIT
---
<h3>一.加入依赖包</h3>
使用spring的测试框架需要加入以下依赖包：
JUnit 4 （官方下载：http://www.junit.org/）
Spring Test （Spring框架中的test包）
Spring 相关其他依赖包（不再赘述了，就是context等包）
如果使用maven，在基于spring的项目中添加如下依赖：

	<dependency>  
	            <groupId>junit</groupId>  
	            <artifactId>junit</artifactId>  
	            <version>4.9</version>  
	            <scope>test</scope>  
	</dependency>   
	<dependency>  
	            <groupId>org.springframework</groupId>  
	            <artifactId>spring-test</artifactId>  
	            <version> 3.2.4.RELEASE  </version>  
	            <scope>provided</scope>  
	</dependency>
<h3>二.创建测试类</h3>
1)基类,其实就是用来加载配置文件的

	import java.util.List;
	 
	import org.junit.Test;
	import org.junit.runner.RunWith;
	import org.springframework.beans.factory.annotation.Autowired;
	import org.springframework.test.context.ContextConfiguration;
	import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
	import org.springframework.test.context.web.WebAppConfiguration;
	 
	import com.slintec.modules.userdb.xtcs.dao.FrmSysparaDao;
	import com.slintec.modules.userdb.xtcs.pojo.FrmSysparaSchema;
	 
	@RunWith(SpringJUnit4ClassRunner.class)
	@WebAppConfiguration 
	@ContextConfiguration(locations={"classpath:spring-core.xml"})
	public class TestDao {
	 
	    @Autowired
	    FrmSysparaDao frmSysparaDao;
	     
	    @Test
	    public void testSysPara(){
	        List<FrmSysparaSchema> list= frmSysparaDao.getAllSysParas();
	        for (FrmSysparaSchema frmSysparaSchema : list) {
	            System.out.println(frmSysparaSchema.getJyw());
	        }
	    }
	     
	}


解释下用到的注解:
@RunWith：用于指定junit运行环境，是junit提供给其他框架测试环境接口扩展，为了便于使用spring的依赖注入，spring提供了org.springframework.test.context.junit4.SpringJUnit4ClassRunner作为Junit测试环境
@ContextConfiguration({"classpath:applicationContext.xml","classpath:spring/buyer/applicationContext-service.xml"}) 
导入配置文件，这里我的applicationContext配置文件是根据模块来分类的。如果有多个模块就引入多个“applicationContext-service.xml”文件。如果所有的都是写在“applicationContext。xml”中则这样导入： 
@ContextConfiguration(locations = "classpath:applicationContext.xml")
@WebAppConfiguration 添加这个注解可以模拟支持被测试的BEAN注入request response session；

Spring 模拟请求测试

	import org.junit.Before;
	import org.junit.Test;
	import org.junit.runner.RunWith;
	import org.springframework.beans.factory.annotation.Autowired;
	import org.springframework.mock.web.MockHttpServletRequest;
	import org.springframework.test.context.ContextConfiguration;
	import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
	import org.springframework.test.context.web.WebAppConfiguration;
	import org.springframework.test.web.servlet.MockMvc;
	import org.springframework.test.web.servlet.MvcResult;
	import org.springframework.test.web.servlet.request.MockMvcRequestBuilders;
	import org.springframework.test.web.servlet.setup.MockMvcBuilders;
	import org.springframework.web.context.WebApplicationContext;

	@RunWith(SpringJUnit4ClassRunner.class)
	@WebAppConfiguration

	@ContextConfiguration(locations={"classpath:spring-core.xml","classpath:spring-mvc.xml"})
	public class MVCTest {
     //传入springmvc的ico
     @Autowired
     WebApplicationContext webApplicationContext;
     //虚拟mvc请求，获取请求结果
     MockMvc mockMvc;

     //每次调用初始化
     @Before
     public void init(){
           mockMvc =MockMvcBuilders.webAppContextSetup(webApplicationContext).build();
     }

     @Test
     public void TestMvc() throws Exception{
           //模拟请求，拿到返回值
           /*MvcResult result = mockMvc.perform(MockMvcRequestBuilders.get("authCodeVif.do").param("ss", "ss"))
                                     .andReturn();  带参数*/
           MvcResult result = mockMvc.perform(MockMvcRequestBuilders.get("authCodeVif.do"))
                     .andReturn();
           MockHttpServletRequest request = result.getRequest();

           System.out.println(request.getAttribute("hdNum"));

     }
}









