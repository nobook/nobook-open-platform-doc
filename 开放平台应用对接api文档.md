# 开放平台应用对接API文档

## 平台介绍

## 使用说明


## 接口对接设置


### URL免密登录

#### 接口说明
主要是为第三方用户对接使用
用户登录NOBOOK虚拟实验平台，需要开发者根据用户信息在服务端生成一个免登录url，通过这个url链接，NOBOOK才能知道是哪个用户来访问。
为了确保客户端每次请求到都是最新的免登陆url，客户端每次向服务器发的请求不能是固定的，以避免请求被缓存。
NOBOOK虚拟实验免登录url经过签名，该url地址1分钟失效，请务必在生成地址后立即使用，使用后页面会重定向进入NOBOOK。

同理

开发者服务器端需要开发一个支持重定向的接口实现动态生成免登录url地址，该接口地址配置在客户端，用户通过点击该地址访问应用。

功能说明：

    第三方有独立的用户系统
    
	第三方用户在自己平台登录，调用应用接口，直接访问应用
	
	创建应用时需要应用有免密登录权限才能对接
	
	需要根据免密地址需要传递必要的参数请看下方介绍

#### 请求说明

请求方式：get

编码说明：UTF-8

请求URL：https://{appname}-lab.nobook.com/withoutpwd/autologin?appid=123&uid=456&temp=1501112321&code=fdasfdDS93ASF8&return_url=www.baidu.com&jsoncallback=?


#### 请求参数说明
1）请求消息体

参数        | 类型 |是否必须| 描述      | 备注
------------|------|--------|-----------|---
appid  | int  | 是     |应用ID     | 
uid         |  int | 是     | 第三方用户ID| 
temp        |string| 是     | 时间戳（以秒为单位）    | 
code        |string| 是     | 加密方式  | 
return_url     |string| 否     | 应用登录成功第三方回调地址  | 第三方url
jsoncallback|string| 否     |   |   
2）请求示例
#### 对接说明
NOBOOK会提供对接相应信息。
或者控制面板中
#####  appid：
    申请应用时分配的appid。
##### appkey:
    申请应用时分配的appkey.
参数code说明
需要把appid appkey temp uid做排序 然后MD5加密
#### 响应说明
失败
{code: 500, data: "", msg: "错误信息"}
成功
免密登录成功
如果有回调地址跳到回调地址
return redirect($return_url);
如果没有回调地址直接进入应用首页

#### 代码示例(nodejs)

```nodejs
const md5 = require('md5');
// 应用程序名称
const APP_NAME = 'appname';
// 应用程序key
const APP_KEY = 'sd893sdg3iudsj30sds';
// 应用程序ID
const APP_ID = '999999';
// 第三方用户ID(仅能是数字)
const UID = 111111;

// 当前时间戳
const TEMP = parseInt(new Date().getTime() / 1000);
// 计算CODE
const CODE = md5(`${APP_ID}${APP_KEY}${TEMP}${UID}`);

const URL = `https://${APP_NAME}-lab.nobook.com/withoutpwd/autologin?appid=${APP_ID}&uid=${UID}&temp=${TEMP}&code=${CODE}`;
console.log(URL);
```

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






