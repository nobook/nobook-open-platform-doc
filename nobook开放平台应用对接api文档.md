# 开放平台应用对接API文档

## 平台介绍
通过开放平台接口，可将NOBOOK虚拟实验作为自己网站产品的一个实验应用，通过对接接口，可实现账号打通，从自己的产品网站直接通过链接进入NOBOOK实验平台并自动登录。

## 使用说明

接口主要提供免密url、oauth2.0 两种方式对接，第一种需要自己写好免密url的接口，第二种如果自己系统具有标准的oauth协议，那么只需要为NOBOOK虚拟实验提供appid和appkey 然后通过NOBOOK官方配置，把NOBOOK实验作为独立应用接入到自己系统中（此方法一般针对教育云平台、区域云平台等具有自己独立应用系统和拥有第三方应用系统）。

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

> * 第三方有独立的用户系统
> * 第三方用户在自己平台登录，调用应用接口，直接访问应用
> * 创建应用时需要应用有免密登录权限才能对接
> * 需要根据免密地址需要传递必要的参数请看下方介绍

#### 请求说明

请求方式：get

编码说明：UTF-8

请求URL：https://{appname}-lab.nobook.com/withoutpwd/autologin?appid=123&uid=456&temp=1501112321&code=fdasfdDS93ASF8&return_url=www.baidu.com&jsoncallback=?


#### 请求参数说明

1）请求消息体

| 参数        | 是否必须   |  参数类型  | 限制长度 |参数说明|
| --------    | :-----     | :----     | :----   | :----|
| uid    | 必须     | str     | 32   | 用户唯一性标识，对应唯一一个用户且不可变|
| appid |必须 | str|  16   | 接口appid，应用的唯一标识|
| timestamp|必须| str|255   |1970-01-01开始的时间戳，秒为单位|
| redirect |必须 | str|  255  | 登录成功后的重定向地址,必须进行URL编码|
| sign |必须 | str|  255   | MD5签名 (除redirect参数外将所有的参数值与appkey按参数名升序进行排列）

### redirect 跳转产品页面汇总

| 页面       | 地址   |  说明 |
| --------    | :-----     |  :-----     | 
| 实验平台首页    | https://{appname}-lab.nobook.com     | 此为NOBOOK官方提供的实验平台入口|
| 物理实验页面 | https://{appname}-lab.nobook.com/go/1 | 物理实验地址|
| 化学实验页面 | https://{appname}-lab.nobook.com/go/5 | 化学实验地址|
| 初中生物实验页面 | https://{appname}-lab.nobook.com/go/2 | 初中生物实验地址|
| 高中生物实验页面 | https://{appname}-lab.nobook.com/go/3 | 高中生物实验地址|
| 小学科学实验页面 | https://{appname}-lab.nobook.com/go/4 | 小学科学实验地址|


### 1.1 签名生成规则
md5签名的原理如下： 将所有的参数值与appkey按参数名升序进行排列。
Md5(value1+value2+...appkey...+valueN)
appkey在签名中的顺序取决于他在所有参数名中的顺序。

2）请求示例

1. 参数：<br>
uid：uid <br>
appid：appid <br>
timestamp:timestamp <br>
appkey:appkey <br>
redirect:https://nobook.com <br>


2. 参数进行升序排列后生成的签名原串：
appid appkey subject timestamp uid
3. 签名后字符串 : 520aed5635dca93d250b809a26840a98

4. 签名url ：https://{appname}-lab.nobook.com/api/login/autologin?subject=phy&appid=appid&uid=uid&timestamp=timestamp&sign= 520aed5635dca93d250b809a26840a98&redirect=https%3a%2f%2fwww.nobook.com%2f

#### 响应说明
失败
{code: 500, data: "", msg: "错误信息"}
成功
免密登录成功
如果有回调地址跳到回调地址
return redirect($return_url);
如果没有回调地址直接进入实验平台首页
### 代码示例php
```<?php

//配置参数
$appid = '';
$appkey = '';
$timestamp = time();
$uid = '';
$redirect = urlencode('https://{appname}nobook.com/go/1');


function  sign($array)
{
    ksort($array);
    $string="";
    while (list($key, $val) = each($array)){
        $string = $string . $val ;
    }
    return md5($string);
}

//获取登录Url
function getLoginUrl($uid, $subject, $appid, $timestamp, $appkey)
{
    $arr = [
        'uid'=> $uid,
        'appid'=> $appid,
        'timestamp'=> $timestamp,
        'appkey'=> $appkey,
    ];
    $sign = sign($arr);
    $param = [
        'timestamp'=> $timestamp,
        'uid'=> $uid,
        'appid'=> $appid,
        'sign'=> $sign,
        'redirect'=> 'https://res-api.nobook.com/wuli/?sourceid=452385a5699233a32bc4c6b292800752',
    ];

    $url = 'https://res-api.nobook.com/api/login/autologin?'.http_build_query($param);

    return $url;


}

$getLoginUrl = getLoginUrl($uid, $subject, $appid, $timestamp, $appkey);

?>


<h4>实验列表URl</h4>


<p> <a target="_blank" href="<?=$getListUrl?>"><?=$getListUrl?></a> </p>

<h4>登录URl</h4>


<p> <a target="_blank" href="<?=$getLoginUrl?>"><?=$getLoginUrl?></a> </p>
```

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

let URL = `https://${APP_NAME}-lab.nobook.com/withoutpwd/autologin?appid=${APP_ID}&uid=${UID}&temp=${TEMP}&code=${CODE}`;

// 如果需要可以指定跳转的链接
let returnURL = 'https://huaxue.nobook.com/';
if (returnURL) {
	URL += `&return_url=${returnURL}`;
}

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






