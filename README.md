# 欢迎使用WeChatBuilder

本项目计划开发一款用于微信开发的快速构建系统

```
java 开发后台
 weUI 开发前台
mysql 做数据库
``` 

## 流程

### 准备工作

1、购买服务器 （安装Tomcat 安装JDK 安装MySQL）

2、购买域名 （域名备案）

3、申请服务号 

### WeChatBuilder具体实现

1、处理请求的Servlet入口CoreServlet，用于处理get和post请求，所有微信的HTTP请求都经过这个CoreServlet
(此处需要配置url,token、appid、AppSecret)

2、微信公众号的基础功能有

普通消息：接收消息、发送消息，消息类型包括（文字、图文、音频、视频、地理信息、图片、连接、事件）
推送消息：关注、取消关注、上报地理位置、扫描二维码（已关注、未关注）、自定义菜单
系统对所有消息惊醒了详细的封装(此处需要针对微信的需求，封装成xml的形式输出，特别注意CDATA)，分别对xml的消息解析即系xml消息封装

3、当用户向公众帐号发消息时，微信服务器会将消息通过POST方式提交给我们在接口配置信息中填写的URL，而我们就需要在URL所指向的请求处理类CoreServlet的doPost方法中接收消息、处理消息和响应消息。通过调用CoreService类的processRequest方法接收、处理消息。

```
 // xml请求解析  
 Map<String, String> requestMap = MessageUtil.parseXml(request);  
 // 发送方帐号（open_id）  
 String fromUserName = requestMap.get("FromUserName");  
 // 公众帐号  
 String toUserName = requestMap.get("ToUserName");  
 // 消息类型  
 String msgType = requestMap.get("MsgType");  
 // 判断消息类型 
 //文本
 msgType.equals(MessageUtil.REQ_MESSAGE_TYPE_TEXT)
 // 事件推送  
 if (msgType.equals(MessageUtil.REQ_MESSAGE_TYPE_EVENT)) {  
                // 事件类型  
                String eventType = requestMap.get("Event");  
                // 订阅  
                if (eventType.equals(MessageUtil.EVENT_TYPE_SUBSCRIBE)) {  }  
                // 取消订阅  
                else if (eventType.equals(MessageUtil.EVENT_TYPE_UNSUBSCRIBE)) { }  
                // 自定义菜单点击事件  
                else if (eventType.equals(MessageUtil.EVENT_TYPE_CLICK)) {   }  
  }  
```

4、定义菜单（菜单分为10种，click和view两种最常见，其他扫码、照片等支持高版本微信）其中click菜单可以通过上一步骤的菜单点击事件相应。view菜单需要执行URL，此处URL可以为任意一个互联网可访问页面。

注意：view可与网页授权获取用户基本信息接口结合，获得用户基本信息。

5、access_tooken:
可以使用AppID和AppSecret调用本接口来获取access_token。AppID和AppSecret可在“微信公众平台-开发-基本配置”页中获得（需要已经成为开发者，且帐号没有异常状态）。调用接口时，请登录“微信公众平台-开发-基本配置”提前将服务器IP地址添加到IP白名单中，点击查看设置方法，否则将无法调用成功。
写单独线程来维护access_tooken。

6、发送模板消息，可以实现公众号向用户发起消息。


### 安装WeChatBuilder
0、新建项目，选择数据库jndi，完成安装操作

2、配置menu菜单，3*5菜单，配置返回消息，配置跳转URL

4、微信公众平台，填写URL，及token，完成部署

5、在WeChatBuilder提供的IDE中编写新的页面程序程序

### 配置服务号
[微信参考指南]https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421135319

1、填写服务器配置

2、验证服务器地址的有效性

3、依据接口文档实现业务逻辑

