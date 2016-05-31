---
title: 动高服务
comments: true
date: 2016-03-22 08:47:49
update: 2016-03-22 08:47:49
categories: 作品集
tags: ['Android','我的作品','上线项目','火车票订票']
---

在`中科大先进技术研究院-龙芯中科联合实验室`工作半年左右之后，在做项目的过程中也积累了一定的经验，而此时我们也接到了一个线上项目，要求开发动高服务APP其中的一个模块-火车票订票模块。这也是我负责开发的第一个线上项目，这次项目的开发为我提供了很多宝贵的项目开发经验。

<!-- more -->

## 一、项目概述

"动高服务"是一款面向动车及高铁乘客提供餐饮服务、列车购票、娱乐休闲等功能的App。本团队负责开发列车购票模块，本人为主力程序员，负责客户端40%的功能实现，并且独立完成本模块的数据库及服务器端的设计与开发。

> * 项目名：动高服务
> * 开发模块：火车票订票模块
> * 开发语言：Android
> * 运行平台：Android平台
> * 数据库：MySQL
> * 开发工具：Eclipse、MyEclipse 2013
> * 版本控制器：Git
> * 下载地址：[http://www.dgaotech.com/dgfw.html](http://www.dgaotech.com/dgfw.html)

## 二、系统功能

我们团队负责的模块为：火车票订票模块。主要功能如下：

> * 城市/站点检索
> * 站到站车次查询
> * 车次分类查询
> * 车票预订
> * 选择/添加乘车人
> * 订单支付（三种方式：1.我的钱包；2.支付宝；3.微信）
> * 线上退票
> * 历史订单查看

本人负责的开发内容：

> * 本模块数据库设计
> * 本模块服务器端设计与开发
> * 本模块通信接口设计
> * 车票预定界面
> * 选择/添加乘车人界面
> * 选择坐席界面
> * 订单支付界面
> * 订单详情界面

下面的内容仅围绕本人负责的开发内容已做介绍。

## 三、系统设计

在本模块的系统设计上，主要分为三个部分：

> 1. 数据库设计
> 2. 服务器端设计
> 3. 客户端设计

### 3.1 数据库设计

数据库E-R图如下所示：

![E-R图](http://7xrnl9.com1.z0.glb.clouddn.com/image%2F%E7%81%AB%E8%BD%A6%E7%A5%A8%E8%AE%A2%E7%A5%A8%2F%E6%95%B0%E6%8D%AE%E5%BA%93E-R%E5%9B%BE.png)

主要为五个表：

> 1. 用户表 - app_user_info
> 2. 乘车人表 - ticket_passenger
> 3. 订单概要表 - ticket_order_info
> 4. 订单详情表 - ticket_order_detail
> 5. 车票详情表 - ticket_detail

### 3.2 通信接口设计

根据项目功能需求，设计相应的通信接口，以用户添加乘车人信息为例，格式如下：

> 添加乘车人接口(AddPassenger)
	
请求数据：

```json
"requestType"："AddPassenger"
"params"：{
	"mobile"："xxx",
	"passenger_name"："xxx",
	"passport_code"："xxx",
	"passport_name"："xxx",
	"passport_no"："xxx",
	"ticket_type_code"："xxx",
	"ticket_type_name"："xxx"
}
```

返回数据：

```json
{
    "resultCode"：0
    "reason"："成功添加乘车人"
    "requestType"："AddPassenger"
    "result"：""
}
```
resultCode的值及其含义：

> 0：成功添加乘车人 
> 1：乘车人证件信息已存在
> -1：乘车人信息添加失败

其他通信接口格式与此格式一致。

## 四、程序设计

### 4.1 客户端程序设计

在客户端的程序开发上，我负责实现的界面有六个，下面只介绍几个主要界面。

#### 4.1.1 填写订单界面

根据上一界面传递的数据进行界面显示，同时可以重新选择坐席类型，可以添加乘车人。在输入了必要数据之后，可以点击确认购票生成订单并且占座，占座成功之后便可支付。

#### 4.1.2 添加乘车人界面

在本界面，用户可以添加乘车人，并使用身份证验证代码对身份证号进行严格验证，避免身份证号出错。

```java
public boolean isValidate18Idcard(String idcard) {
	// 非18位为假
	if (idcard.length() != 18) {
		return false;
	}
	// 获取前17位
	String idcard17 = idcard.substring(0, 17);
	// 获取第18位
	String idcard18Code = idcard.substring(17, 18);
	char c[] = null;
	String checkCode = "";
	// 是否都为数字
	if (isDigital(idcard17)) {
		c = idcard17.toCharArray();
	} else {
		return false;
	}
	if (null != c) {
		int bit[] = new int[idcard17.length()];
		bit = converCharToInt(c);
		int sum17 = 0;
		sum17 = getPowerSum(bit);
		// 将和值与11取模得到余数进行校验码判断
		checkCode = getCheckCodeBySum(sum17);
		if (null == checkCode) {
			return false;
		}
		// 将身份证的第18位与算出来的校码进行匹配，不相等就为假
		if (!idcard18Code.equalsIgnoreCase(checkCode)) {
			return false;
		}
	}
	return true;
}
```

#### 4.1.3 订单详情界面

当用户预订的车票占座成功，即可进入订单详情界面，在订单详情界面上方有一个30分钟倒计时，占座成功30分钟之后订单将失效，在订单未失效前用户可以点击支付按钮进行订单支付，也可以点击取消按钮，取消订单。

#### 4.1.4 订单支付界面

在本界面有三种支付方式(支付宝及微信支付具体实现由同事完成)：

> 1. 我的钱包支付
> 2. 支付宝支付
> 3. 微信支付

用户可以根据自身情况选择不同的支付方式进行支付。当用户支付成功之后即执行出票操作，出票成功之后将短信通知用户出票成功及车票信息。

### 4.2 服务器端设计

服务器端分为4层：

> 1. Servlet - 处理请求
> 2. Service - 业务逻辑
> 3. DAO - 数据访问对象
> 4. DTO - 数据传输对象

服务器端有一个统一的`请求Servlet（/ServletFactory）`,所有的客户端请求均请求此Servlet,ServletFactory再通过反射将请求参数分发传递到相应的处理类中，进行处理，处理类调用Service层中的业务逻辑，Service层则调用DAO层对数据库进行增、删、改、查的操作。当处理类将请求处理完成之后返回结果，完成完整的请求-响应过程。

#### 4.2.1 HandleRequest接口

所有的处理类均需实现`HandleRequest`接口，对请求的处理在handleRequest方法中进行，处理完数据之后将结果返回。

```java
public interface HandleRequest {
	public int handleRequest(JSONObject params); // 处理请求的主要方法
	public String getRequestType();	// 获取请求类型
	public String getReason();
	public void setReason(String reason);
	public JSONArray getResponseResult(); //获取响应参数Result
	public void setResponseContent(JSONArray responseResult); //设置返回的响应数据
}
```

#### 4.2.2 处理类

处理类需要实现`HandleRequest`接口，主要用来处理请求，处理类调用Service层的方法来处理业务逻辑。

#### 4.2.3 Service

Service是系统中的业务逻辑，将复杂的业务逻辑封装之后供上层调用，同时Service调用DAO层(数据访问对象)操作数据库，完成数据的增、删、改、查。

#### 4.2.4 DAO 

DAO层是数据访问对象，系统在这一层直接对数据库进行操作，降低系统的耦合性，提高可维护性。

#### 4.2.5 DTO 

DTO层是数据传输对象，负责在各层之间传递数据。

## 五、界面及功能展示

### 5.1 选择/添加乘车人界面

![选择/添加乘车人界面](http://7xrnl9.com1.z0.glb.clouddn.com/image%2F%E7%81%AB%E8%BD%A6%E7%A5%A8%E8%AE%A2%E7%A5%A8%2Fchoose_add_passenger.png)

### 5.2 填写订单界面

![填写订单界面](http://7xrnl9.com1.z0.glb.clouddn.com/image%2F%E7%81%AB%E8%BD%A6%E7%A5%A8%E8%AE%A2%E7%A5%A8%2FTXDD.png)

### 5.3 订单详情界面

![订单详情界面](http://7xrnl9.com1.z0.glb.clouddn.com/image%2F%E7%81%AB%E8%BD%A6%E7%A5%A8%E8%AE%A2%E7%A5%A8%2FDDXQ.png)

### 5.4 订单支付界面

![订单支付界面](http://7xrnl9.com1.z0.glb.clouddn.com/image%2F%E7%81%AB%E8%BD%A6%E7%A5%A8%E8%AE%A2%E7%A5%A8%2FZFDD.png)

## 六、总结

本次参与线上项目的开发，使我获得了很丰富的项目开发经验，学习到了很多新技术，在开发的期间虽然遇到了很多困难以及难题，但是当我们解决了问题之后也学习到了很多东西。

同时感谢项目组的每一个人，项目开发期间大家都很辛苦，但是总算不负众望，产品已成功上线。