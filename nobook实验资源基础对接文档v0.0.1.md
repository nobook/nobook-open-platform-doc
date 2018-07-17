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
请求URL：https://school.nobook.com/login/autologin?appid=123&uid=456&timestamp=1501112321&sign=fdasfdDS93ASF8&nickname=zhangsan&redirect=https%3a%2f%2fwww.nobook.com%2f

### 2.1 参数说明

| 参数        | 是否必须   |  参数类型  | 限制长度 |参数说明|
| --------    | :-----     | :----     | :----   | :----|
| uid    | 必须     | str     | 32   | 第三方系统 用户唯一性标识，对应唯一一个用户且不可变，用于在NOBOOK系统生成对应的绑定用户|
| appid |必须 | str|  16   | 接口appid，应用的唯一标识（由NOBOOK提供）|
| timestamp|必须| str|255   |1970-01-01开始的时间戳，秒为单位|
| redirect |必须 | str|  255  | 登录成功后的重定向地址,必须进行URL编码 默认请使用appurl (独立平台地址，由NOBOOK提供) |
| sign |必须 | str|  255   | MD5签名 MD5(appid appkey  timestamp uid）详细参数请参考 2.2签名规则
|
| nickname |非必须 | str|  60   | 昵称|


**具体用例：**

1. 参数：<br>
uid：uid <br>
appid：appid <br>
timestamp:timestamp <br>
appkey:appkey <br>
redirect:https://school.nobook.com/xx (默认为独立平台首页 appurl ，由NOBOOK提供) <br>


2. 参数进行升序排列后生成的签名原串：
appid appkey timestamp uid
3. 签名后字符串 : c8f0a5841e0c553b3022438f50c8454f

4. 签名url ：https://school.nobook.com/login/autologin?appid=appid&uid=uid&timestamp=timestamp&sign=c8f0a5841e0c553b3022438f50c8454f&redirect=https%3a%2f%2fschool.nobook.com%2f

---




### 4、phpDemo 生成代签名的跳转url
```
<?php
$appid = 'appid';
$appkey = 'appkey';
$nickname = '昵称';
$timestamp = time();
$uid = '用户唯一ID';
$redirect = 'https://school.nobook.com/xx';
$url = 'https://school.nobook.com';

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
function getLoginUrl($uid, $appid, $timestamp, $appkey, $url, $redirect, $nickname)
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
        'nickname'=> $nickname,
        'redirect'=> $redirect,
    ];

    $url = $url.'/login/autologin?'.http_build_query($param);

    return $url;


}


$getLoginUrl = getLoginUrl($uid, $appid, $timestamp, $appkey, $url, $redirect, $nickname);

?>



<h4>登录URl</h4>

<p> <a target="_blank" href="<?=$getLoginUrl?>"><?=$getLoginUrl?></a> </p>

```



