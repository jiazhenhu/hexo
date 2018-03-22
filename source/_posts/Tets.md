---
title: springMVC返回json数据乱码问题及@RequestMapping 详解
date: 2018-03-19 09:05:17
tags:
---
<h3>一、@RequestMapping</h3>
RequestMapping是一个用来处理请求地址映射的注解，可用于类或方法上。用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径。
RequestMapping注解有六个属性，下面我们把她分成三类进行说明。<br>
<h4>1、 value， method；</h4>
value： 指定请求的实际地址，指定的地址可以是URI Template 模式（后面将会说明）；<br>
method： 指定请求的method类型， GET、POST、PUT、DELETE等；<br>
<h4>2、 consumes，produces；</h4>
consumes： 指定处理请求的提交内容类型（Content-Type），例如application/json, text/html;<br>
produces: 指定返回的内容类型，仅当request请求头中的(Accept)类型中包含该指定类型才返回；<br>
<h4>3、 params，headers；</h4>
params： 指定request中必须包含某些参数值是，才让该方法处理。<br>
headers： 指定request中必须包含某些指定的header值，才能让该方法处理请求。<br>
<h3>二、json数据乱码</h3>
在springMVC controller中返回json数据出现乱码问题，因为没有进行编码，只需要简单的注解就可以了<br>
在@RequestMapping()中加入produces="text/html;charset=UTF-8"属性即可，如下：

	@RequestMapping(value="/respost",method=RequestMethod.GET,produces="text/html;charset=UTF-8")  
	@ResponseBody  
	public String postList(@RequestParam("topicId") String topicId){  
		List<Post> posts=new ArrayList<Post>();  
		System.out.println("topicId-----"+topicId);  
		posts=postService.findPostList(topicId);  
		JSONArray postJson=JSONArray.fromObject(posts);  
		return postJson.toString();  
	}

