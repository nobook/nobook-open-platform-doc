# 一、免登录地址接口

------

## 1、接口定义

主要是为第三方用户对接使用 用户登录NOBOOK虚拟实验平台，需要开发者根据用户信息在服务端生成一个免登录url，通过这个url链接，NOBOOK才能知道是哪个用户来访问。 为了确保客户端每次请求到都是最新的免登陆url，客户端每次向服务器发的请求不能是固定的，以避免请求被缓存。 NOBOOK虚拟实验免登录url经过签名，该url地址5分钟失效，请务必在生成地址后立即使用，使用后页面会重定向进入NOBOOK。

同理

开发者服务器端需要开发一个支持重定向的接口实现动态生成免登录url地址，该接口地址配置在客户端，用户通过点击该地址访问应用。

功能说明：
> * 第三方有独立的用户系统
> * 第三方用户在自己平台登录，调用应用接口，直接访问应用
> * 创建应用时需要应用有免密登录权限才能对接
> * 需要根据免密地址需要传递必要的参数请看下方介绍



## 2. 免登录URL生成

### 请求说明
请求方式：get <br>
编码说明：UTF-8 <br>
请求URL：https://res-api.nobook.com/api/login/autologin?subject=phy&appid=123&uid=456&timestamp=1501112321&sign=fdasfdDS93ASF8&redirect=https%3a%2f%2fwww.nobook.com%2f

### 2.1 参数说明

| 参数        | 是否必须   |  参数类型  | 限制长度 |参数说明|
| --------    | :-----     | :----     | :----   | :----|
| uid    | 必须     | str     | 32   | 用户唯一性标识，对应唯一一个用户且不可变|
| subject    | 必须     | str     | 32   | 学科标识（phy：物理；chem：化学；biocz：初中生物；biogz：高中生物；sci：小学科学）|
| appid |必须 | str|  16   | 接口appid，应用的唯一标识|
| timestamp|必须| str|255   |1970-01-01开始的时间戳，秒为单位|
| redirect |必须 | str|  255  | 登录成功后的重定向地址,必须进行URL编码|
| sign |必须 | str|  255   | MD5签名 MD5(appid appkey subject timestamp uid）
|


### 2.2 签名生成规则
md5签名的原理如下： 将所有的参数值与appkey按参数名升序进行排列。
Md5(value1+value2+...appkey...+valueN)
appkey在签名中的顺序取决于他在所有参数名中的顺序。

用例：

1. 参数：<br>
uid：uid <br>
appid：appid <br>
timestamp:timestamp <br>
appkey:appkey <br>
subject:phy <br>
redirect:https://nobook.com <br>


2. 参数进行升序排列后生成的签名原串：
appid appkey subject timestamp uid
3. 签名后字符串 : 520aed5635dca93d250b809a26840a98

4. 签名url ：https://res-api.nobook.com/api/login/autologin?subject=phy&appid=appid&uid=uid&timestamp=timestamp&sign= 520aed5635dca93d250b809a26840a98&redirect=https%3a%2f%2fwww.nobook.com%2f

---

## 3.响应说明

失败 {code: 500, data: "", msg: "错误信息"}

成功 免密登录成功，跳到回调地址redirect



# 二、资源接口

------
## 1. 获取资源
### 请求说明
请求方式：get <br>
编码说明：UTF-8 <br>
请求URL：https://res-api.nobook.com/api/experiment/get?appid=123&subject=phy&timestamp=1501112321&sign=fdasfdDS93ASF8

### 参数说明

| 参数        | 是否必须   |  参数类型  | 限制长度 |参数说明|
| --------    | :-----     | :----     | :----   | :----|
| subject    | 必须     | str     | 32   | 学科标识（phy：物理；chem：化学；biocz：初中生物；biogz：高中生物；sci：小学科学）|
| appid |必须 | str|  16   | 接口appid，应用的唯一标识|
| timestamp|必须| str|255   |1970-01-01开始的时间戳，秒为单位|
| sign |必须 | str|  255   | MD5签名 MD5（appid appkey subject timestamp）|



## 响应说明

失败 {code: 500, data: "", msg: "错误信息"}

成功 {code: 200, data:_data,  msg: "ok"}


## 2. 获取章节
### 请求说明
请求方式：get <br>
编码说明：UTF-8 <br>
请求URL：https://res-api.nobook.com/api/experiment/chapter?appid=123&subject=phy&timestamp=1501112321&sign=fdasfdDS93ASF8

### 参数说明

| 参数        | 是否必须   |  参数类型  | 限制长度 |参数说明|
| --------    | :-----     | :----     | :----   | :----|
| subject    | 必须     | str     | 32   | 学科标识（phy：物理；chem：化学；biocz：初中生物；biogz：高中生物；sci：小学科学）|
| appid |必须 | str|  16   | 接口appid，应用的唯一标识|
| timestamp|必须| str|255   |1970-01-01开始的时间戳，秒为单位|
| sign |必须 | str|  255   | MD5签名 MD5（appid appkey subject timestamp）|



## 响应说明

失败 {code: 500, data: "", msg: "错误信息"}

成功 {code: 200, data:_data,  msg: "ok"}


## 3. 资源类别
### 请求说明
请求方式：get <br>
编码说明：UTF-8 <br>
请求URL：https://res-api.nobook.com/api/experiment/classifications?appid=123&subject=phy&timestamp=1501112321&sign=fdasfdDS93ASF8

### 参数说明

| 参数        | 是否必须   |  参数类型  | 限制长度 |参数说明|
| --------    | :-----     | :----     | :----   | :----|
| subject    | 必须     | str     | 32   | 学科标识【phy】|
| appid |必须 | str|  16   | 接口appid，应用的唯一标识|
| timestamp|必须| str|255   |1970-01-01开始的时间戳，秒为单位|
| sign |必须 | str|  255   | MD5签名 MD5（appid appkey subject timestamp）|



## 响应说明

失败 {code: 500, data: "", msg: "错误信息"}

成功 {code: 200, data:_data,  msg: "ok"}


### 4、phpDemo
```
<?php
//配置参数
$appid = 'appid';
$appkey = 'appkey';
$subject = 'phy';
$timestamp = time();
$uid = '23513301';
$redirect = urlencode('https://res-api.nobook.com/wuli/?sourceid=452385a5699233a32bc4c6b292800752');

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

//获取实验列表URL
function getListUrl($subject, $appid, $appkey, $timestamp, $url)
{
    $arr = [
        'subject'=> $subject,
        'appid'=> $appid,
        'appkey'=> $appkey,
        'timestamp'=> $timestamp,
    ];

    $sign = sign($arr);
    $param = [
        'timestamp'=> $timestamp,
        'subject'=> $subject,
        'appid'=> $appid,
        'type'=> 'chapter',
        'typename'=> '第十五章  电流和电路',
        'sign'=> $sign,
    ];

    $url = $url.'/api/experiment/get?'.http_build_query($param);

    return $url;
}


//获取章节URL
function getChapterUrl($subject, $appid, $appkey, $timestamp, $url)
{
    $param = [
        'timestamp'=> $timestamp,
        'subject'=> $subject,
        'appid'=> $appid,
        'sign'=> md5($appid.$appkey.$subject.$timestamp),
    ];

    $url = $url.'/api/experiment/chapter?'.http_build_query($param);

    return $url;

}


//获取资源类别URL
function getClassificationsUrl($subject, $appid, $appkey, $timestamp, $url)
{
    $param = [
        'timestamp'=> $timestamp,
        'subject'=> $subject,
        'appid'=> $appid,
        'sign'=> md5($appid.$appkey.$subject.$timestamp),
    ];

    $url = $url.'/api/experiment/classifications?'.http_build_query($param);

    return $url;

}


//获取登录Url
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
        'redirect'=> 'https://res-api.nobook.com/wuli/?sourceid=452385a5699233a32bc4c6b292800752',
    ];

    $url = $url.'/api/login/autologin?'.http_build_query($param);

    return $url;


}

$getListUrl = getListUrl($subject, $appid, $appkey, $timestamp, $url);
$getChapterUrl = getChapterUrl($subject, $appid, $appkey, $timestamp, $url);
$getClassificationsUrl = getClassificationsUrl($subject, $appid, $appkey, $timestamp, $url);

$getLoginUrl = getLoginUrl($uid, $subject, $appid, $timestamp, $appkey, $url);

?>


<h4>实验列表URl</h4>
<p> <a target="_blank" href="<?=$getListUrl?>"><?=$getListUrl?></a> </p>

<h4>实验章节URl</h4>
<p> <a target="_blank" href="<?=$getChapterUrl?>"><?=$getChapterUrl?></a> </p>

<h4>资源类别URl</h4>
<p> <a target="_blank" href="<?=$getClassificationsUrl?>"><?=$getClassificationsUrl?></a> </p>

<h4>登录URl</h4>

<p> <a target="_blank" href="<?=$getLoginUrl?>"><?=$getLoginUrl?></a> </p>





```

### 5. nodejs demo

```
const express = require('express');
const app = express();
const md5 = require('md5');

const appid = '9999999';
const appkey = '8fh37fghr5vhdow93uyghiods';

// 获取实验列表
app.get('/list', (req, res)=> {
    const url = 'https://res-api.nobook.com/api/experiment/get';
    const subject = 'chem';
    const timestamp = Math.round(new Date().getTime() / 1000);
    const sign = md5(`${appid}${appkey}${subject}${timestamp}`);
    const redirectURL = `${url}?appid=${appid}&subject=${subject}&timestamp=${timestamp}&sign=${sign}`;
    res.redirect(redirectURL);
});

// 跳转到某一个具体的实验 
app.get('/demo', (req, res)=> {
    const redirect = encodeURIComponent('http://res-api.nobook.com/wuli/?sourceid=98b7908cdb19fcadfe77a380b4769c63');
    const subject = 'phy';
    const timestamp = Math.round(new Date().getTime() / 1000);
    const uid = '10000';
    const sign = md5(`${appid}${appkey}${subject}${timestamp}${uid}`);
    res.redirect(`https://res-api.nobook.com/api/login/autologin?appid=${appid}&uid=${uid}&subject=${subject}&timestamp=${timestamp}&sign=${sign}&redirect=${redirect}`);
});

app.listen(8080);

```

### 5. 视频demo
![](https://downloadcdn.oss-cn-qingdao.aliyuncs.com/video/NOBOOK_VIDEO/without_password_demo.mp4)
demo 下载地址
https://downloadcdn.oss-cn-qingdao.aliyuncs.com/video/NOBOOK_VIDEO/without_password_demo.mp4


