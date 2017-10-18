# 开放平台应用对接API文档

## 平台介绍

## 使用说明


## 接口对接设置
### 普通登录
#### 接口说明
应用输入用户名和密码的登录
#### 请求说明
请求方式：post

报文格式：form表单提交

编码说明：UTF-8

请求URL：https://{appname}-lab.nobook.com/login
#### 请求参数说明

1）请求消息体

参数 | 类型| 是否必须| 描述| 备注
---|------|------|------|---
username | string| 是| 用户名| 
passwodr | string| 是| 密码| 

2）请求示例

#### 响应说明
1）响应消息体

参数 | 类型| 是否必须| 描述| 备注
---|------|------|------|---
code| String| 是| 状态码| 
data| String| 是| 返回的数据| 
msg | String| 是| 状态信息| 
2）响应示例

    {
     "code": 200,
      "data": "",
     "msg": "\u767b\u5f55\u6210\u529f!"
    }
## 状态码附录

状态码 | 描述
---|---
200 | 成功
500 | 失败

### URL免密登录

#### 接口说明
主要是为第三方用户对接使用


功能说明：

    第三方有独立的用户系统
    
	第三方用户在自己平台登录，调用应用接口，直接访问应用
	
	创建应用时需要应用有免密登录权限才能对接
	
	需要根据免密地址需要传递必要的参数包括（格式，信息）

#### 请求说明

请求方式：post

编码说明：UTF-8

请求URL：https://{appname}-lab.nobook.com//withoutpwd/autologin?openapp_id=123&uid=456&temp=1501112321390&code=fdasfdDS93ASF8&dockurl=www.baidu.com&pid=1&scope=
1,2,3&jsoncallback=?

接口联系人电话：15101064033 

邮箱：lishuqiang@nobook.com

#### 请求参数说明
1）请求消息体

参数        | 类型 |是否必须| 描述      | 备注
------------|------|--------|-----------|---
openapp_id  | int  | 是     |应用ID     | 
uid         |  int | 是     | 第三方用户ID| 
temp        |string| 是     | 时间戳    | 
code        |string| 是     | 加密方式  | ksort（appid,appkey,temp,uid）md5加密
dockurl     |string| 是     | 对接URL   | 第三方url
pid         |int   | 是     | Vip产品id | 
scope       |string| 否     | 多产品id串| 
jsoncallback|string| 否     |回调地址   |   
2）请求示例

#### 响应说明
    无返回值，免密登录成功直接进入应用


### OAuth2.0协议登录
#### OAuth2.0概述
OAuth2.0较1.0相比，整个授权验证流程更简单更安全，也是未来最主要的用户身份验证和授权方式。

关于OAuth2.0协议的授权流程可以参考下面的流程图，其中Client指第三方应用，Resource Owner指用户，Authorization Server是我们的授权服务器，Resource Server是API服务器。
    
![image](/uploads/807f367f8385d8255a32859d71b613d8/image.png)

如要使用OAuth2.0进行对接，需要第三方提供标准的[OAuth2.0](http://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html)接口。然后到[NOBOOK开放平台](https://op.nobook.com/)创建应用，再根据原理进入应用后配置参数：“用户系统对接-->OAuth2.0协议登录”；
#### 对接说明
第三方提供接口
#### 请求说明
第三方定义请求

##### NB回调地址Redirect_url：

示例：http://6Z27v-lab.nobook.cc/authlogin/callback

回调地址可以在：“用户系统对接-->OAuth2.0协议登录”中获取；
#### OAuth2.0参数配置说明
开放平台-->应用管理-->用户系统对接-->OAuth协议登录
#####  client_id：
    申请应用时分配的client_id。

##### client_secret:
    申请应用时分配的client_secret.

##### 获取CODE地址:
    请求参数说明:其中的变量值请使用[client_id] 、[client_secret]、[redirect_uri] 
    代替例如：http://server.com/oauth/authorize?response_type=code&client_id=[client_id]&redirect_uri=[redirect_uri]

    返回参数说明:如获取code成功后则会跳转到指定的回调地址，并在redirect_uri地址上带上Authorization Code。 
    例如: http://auto.nobook.tech/oauth/callback?code=9A5F************************06AF

##### 获取access_token地址:
    如获取access_token地址:https://api.weibo.com/oauth2/access_token

###### 获取access_token的请求参数:

    如获取access_token的请求参数：response_type=code&code=[code]&client_id=[client_id]&redirect_uri=[redirect_url]

    说明:其中的变量值请使用[code]、[client_id] 、[client_secret]、[redirect_url] 代替

    返回参数说明:
    {
       "access_token": "FE04*********CCE2"
       "openid": "11110000"
    }
       


###### 获取access_token的请求类型:

    |post|get

##### 获取用户信息接口地址

    说明:
    如果access_token获取成功后会在获取用户信息接口地址后带上获取access_token时的返回值用GET去请求。

    列如:https://server.com/account/get_uid.json?access_token=FE04*********CCE2&openid=11110000

###### 获取用户唯一标识参数
    用户在第三方用户系统中的唯一标示；
    
###### 是否单独获取用户昵称
    |不单独获取 | 单独获取
###### 获取用户昵称参数

    说明:
    1.如用户信息返回参数格式如下:
    {
    "openid": 1111,      //用户唯一标识
    "username": 'lisi'
    }
    则用户唯一标识参数为: openid

    则用户昵称参数为: username

    2.如用户信息返回参数格式如下:
    {
    status:0,
    result:{
        "openid": 1111,      //用户唯一标识
        "username": 'lisi'
    }
    }
    则用户唯一标识参数为: result->openid






