# 功能说明及对接接口

------

## 1、接口定义

* NOBOOK将为用户独立生成资源平台，参考示例：https://school.nobook.com/xx  其中xx为应用名称，NOBOOK将为每个客户生成独立的应用名称。NOBOOK提供的对接参数包含：appid:应用id、appkey:应用密钥、appname:应用英文名称、appurl:独立平台地址。

* 主要是为第三方用户对接使用 用户登录NOBOOK虚拟实验平台，需要开发者根据用户信息在服务端生成一个免登录url，通过这个url链接，NOBOOK才能知道是哪个用户来访问。 为了确保客户端每次请求到都是最新的免登陆url，客户端每次向服务器发的请求不能是固定的，以避免请求被缓存。 NOBOOK虚拟实验免登录url经过签名，该url地址5分钟失效，请务必在生成地址后立即使用，使用后页面会重定向进入NOBOOK。
* 免密后的跳转地址为

同理

**开发者服务器端需要开发一个支持重定向的接口实现动态生成免登录url地址，该接口地址配置在客户端，用户通过点击该地址访问应用。**

功能说明：
> * 第三方有独立的用户系统
> * 第三方用户在自己平台登录，调用应用接口，直接访问应用
> * 创建应用时需要应用有免密登录权限才能对接
> * 需要根据免密地址需要传递必要的参数请看下方介绍



## 2. 免登录URL生成（用户同步登录）

### 请求说明
请求方式：get <br>
编码说明：UTF-8 <br>
请求URL：https://res-api.nobook.com/api/login/autologin?subject=phy&appid=123&uid=456&timestamp=1501112321&sign=fdasfdDS93ASF8&redirect=https%3a%2f%2fwww.nobook.com%2f

### 2.1 参数说明

| 参数        | 是否必须   |  参数类型  | 限制长度 |参数说明|
| --------    | :-----     | :----     | :----   | :----|
| uid    | 必须     | str     | 32   | 第三方系统 用户唯一性标识，对应唯一一个用户且不可变，用于在NOBOOK系统生成对应的绑定用户|
| subject    | 必须     | str     | 32   | 学科标识（phy：物理；chem：化学；biocz：初中生物；biogz：高中生物；sci：小学科学）|
| appid |必须 | str|  16   | 接口appid，应用的唯一标识（由NOBOOK提供）|
| timestamp|必须| str|255   |1970-01-01开始的时间戳，秒为单位|
| redirect |必须 | str|  255  | 登录成功后的重定向地址,必须进行URL编码 默认请使用appurl (独立平台地址，由NOBOOK提供) |
| sign |必须 | str|  255   | MD5签名 MD5(appid appkey subject timestamp uid）详细参数请参考 2.2签名规则
|


### 2.2 签名生成规则
md5签名的原理如下： 将所有的参数值与appkey按参数名升序进行排列，具体可参考下面的“具体用例”。
Md5(value1+value2+...appkey...+valueN)
appkey在签名中的顺序取决于他在所有参数名中的顺序。

**具体用例：**

1. 参数：<br>
uid：uid <br>
appid：appid <br>
timestamp:timestamp <br>
appkey:appkey <br>
subject:phy <br>
redirect:https://school.nobook.com/xx (默认为独立平台首页 appurl ，由NOBOOK提供) <br>


2. 参数进行升序排列后生成的签名原串：
appid appkey subject timestamp uid
3. 签名后字符串 : 520aed5635dca93d250b809a26840a98

4. 签名url ：https://res-api.nobook.com/api/login/autologin?subject=phy&appid=appid&uid=uid&timestamp=timestamp&sign=520aed5635dca93d250b809a26840a98&redirect=https%3a%2f%2fschool.nobook.com%2f

---

## 3.响应说明

失败 {code: 500, data: "", msg: "错误信息"}

成功 免密登录成功，跳到回调地址redirect



### 4、phpDemo 生成代签名的跳转url
```
<?php
//配置参数
$appid = 'appid';
$appkey = 'appkey';
$subject = 'phy';
$timestamp = time();
$uid = '23513301';
$redirect = urlencode('https://school.nobook.com/xx');

$url = 'https://res-api.nobook.com';

function  sign($array)
{
    ksort($array);
    $string="";
    while (list($key, $val) = each($array)){
        $string = $string . $val ;
    }
    return md5($string);
}


//获取免密登录Url
function getLoginUrl($uid, $subject, $appid, $timestamp, $appkey, $url)
{
    $arr = [
        'uid'=> $uid,
        'subject'=> $subject,
        'appid'=> $appid,
        'timestamp'=> $timestamp,
        'appkey'=> $appkey,
    ];
    $sign = sign($arr);
    $param = [
        'timestamp'=> $timestamp,
        'uid'=> $uid,
        'subject'=> $subject,
        'appid'=> $appid,
        'sign'=> $sign,
        'redirect'=> $redirect
    ];

    $url = $url.'/api/login/autologin?'.http_build_query($param);

    return $url;


}


$getLoginUrl = getLoginUrl($uid, $subject, $appid, $timestamp, $appkey, $url);

//跳转到独立实验平台
header("location:".$getLoginUrl."");

?>


```





### 5. nodejs demo

```
const express = require('express');
const app = express();
const md5 = require('md5');

const appid = '9999999';
const appkey = '8fh37fghr5vhdow93uyghiods';



// 跳转独立实验平台 
app.get('/demo', (req, res)=> {
    const redirect = encodeURIComponent('http://school.nobook.com/xx');//修改为NOBOOK分配的独立资源平台地址
    const subject = 'phy';
    const timestamp = Math.round(new Date().getTime() / 1000);
    const uid = '10000';
    const sign = md5(`${appid}${appkey}${subject}${timestamp}${uid}`);
    res.redirect(`https://res-api.nobook.com/api/login/autologin?appid=${appid}&uid=${uid}&subject=${subject}&timestamp=${timestamp}&sign=${sign}&redirect=${redirect}`);
});

app.listen(8080);

```

### 5. 视频demo
暂无



