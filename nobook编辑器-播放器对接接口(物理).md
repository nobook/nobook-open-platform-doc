
 <h1 class="curproject-name"> 校园版对接接口 </h1> 
 校园版对接接口


# 授权登录接口

## 授权登录接口
<a id=授权登录接口> </a>
### 基本信息

**Path：** /api/resapi/v1/login

**Method：** GET

**接口描述：**
<p>正式环境请求地址：<a href="http://res-api.nobook.com/one">https://res-api.nobook.com</a>/api/resapi/v1/login</p>
<p>签名有效期：5分钟</p>


### 请求参数
**Query**

| 参数名称  |  是否必须 | 示例  | 备注  |
| ------------ | ------------ | ------------ | ------------ |
| pid | 是  |  NiFEjb83nJL4 |  物理:NiFEjb83nJL4,生物:JuFhE84jRhEh,高中生物:EjEViMk33jNr,科学:JeFJeFvReEI9,化学:IfKiInEcZu9c |
| uid | 是  |  1 |  用户ID |
| nickname | 否  |  张三 |  用户昵称 |
| appid | 是  |  dsfhksjdeur98uusdihsdj |  appid |
| sign | 是  |  签名 |  签名  md5(appid appkey nickname pid timestamp uid) |
| timestamp | 是  |  1526441147 |  时间戳(秒) |

### 返回数据

<table>
  <thead class="ant-table-thead">
    <tr>
      <th key=name>名称</th><th key=type>类型</th><th key=required>是否必须</th><th key=default>默认值</th><th key=desc>备注</th><th key=sub>其他信息</th>
    </tr>
  </thead><tbody className="ant-table-tbody"><tr key=0-0><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> code</span></td><td key=1><span>number</span></td><td key=2>非必须</td><td key=3></td><td key=4><span>200：成功   500:失败</span></td><td key=5></td></tr><tr key=0-1><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> data</span></td><td key=1><span>object</span></td><td key=2>非必须</td><td key=3></td><td key=4><span></span></td><td key=5></td></tr><tr key=0-1-0><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> nickname</span></td><td key=1><span>null</span></td><td key=2>非必须</td><td key=3></td><td key=4><span>用户昵称</span></td><td key=5></td></tr><tr key=0-1-1><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> uid</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span>用户唯一ID</span></td><td key=5></td></tr><tr key=0-1-2><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> token</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span>TOKEN</span></td><td key=5></td></tr><tr key=0-1-3><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> vip</span></td><td key=1><span>number</span></td><td key=2>非必须</td><td key=3></td><td key=4><span>1.VIP  0.非VIP</span></td><td key=5></td></tr><tr key=0-1-4><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> vip_endtime</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span>VIP过期时间</span></td><td key=5></td></tr><tr key=0-2><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> msg</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span>提示信息</span></td><td key=5></td></tr>
               </tbody>
              </table>
            
# 固定资源接口

## 获取资源列表
<a id=获取资源列表> </a>
### 基本信息

**Path：** /api/resapi/v1/resources/get

**Method：** GET

**接口描述：**
<p>正式环境地址：<a href="http://res-api.nobook.com/api/resapi/v1/resources/get">https://res-api.nobook.com/api/resapi/v1/resources/get</a><br data-tomark-pass=""><br>
<br data-tomark-pass=""></p>


### 请求参数
**Query**

| 参数名称  |  是否必须 | 示例  | 备注  |
| ------------ | ------------ | ------------ | ------------ |
| pid | 是  |  NiFEjb83nJL4 |  物理:NiFEjb83nJL4,生物:JuFhE84jRhEh,高中生物:EjEViMk33jNr,科学:JeFJeFvReEI9,化学:IfKiInEcZu9c |
| token | 是  |  1232423423423wwrw |  token |
| type | 否  |  classification |  实验类别   值为chapter是通过章节获取实验列表，值为classification时通过分类获取实验列表 |
| typename | 否  |  电学 |  type值存在时classification必须填写 |

### 返回数据

<table>
  <thead class="ant-table-thead">
    <tr>
      <th key=name>名称</th><th key=type>类型</th><th key=required>是否必须</th><th key=default>默认值</th><th key=desc>备注</th><th key=sub>其他信息</th>
    </tr>
  </thead><tbody className="ant-table-tbody"><tr key=0-0><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> code</span></td><td key=1><span>number</span></td><td key=2>非必须</td><td key=3></td><td key=4><span>200-成功  500-失败</span></td><td key=5></td></tr><tr key=0-1><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> data</span></td><td key=1><span>object []</span></td><td key=2>非必须</td><td key=3></td><td key=4><span></span></td><td key=5><p key=3><span style="font-weight: '700'">item 类型: </span><span>object</span></p></td></tr><tr key=0-1-0><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> iconPath</span></td><td key=1><span>string</span></td><td key=2>必须</td><td key=3></td><td key=4><span>实验缩略图地址</span></td><td key=5></td></tr><tr key=0-1-1><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> name</span></td><td key=1><span>string</span></td><td key=2>必须</td><td key=3></td><td key=4><span>实验名称</span></td><td key=5></td></tr><tr key=0-1-2><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> parent</span></td><td key=1><span>string</span></td><td key=2>必须</td><td key=3></td><td key=4><span>章节</span></td><td key=5></td></tr><tr key=0-1-3><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> vip</span></td><td key=1><span>number</span></td><td key=2>必须</td><td key=3></td><td key=4><span>1-vip资源  0-非vip资源</span></td><td key=5></td></tr><tr key=0-1-4><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> path</span></td><td key=1><span>string,null</span></td><td key=2>必须</td><td key=3></td><td key=4><span>路径</span></td><td key=5></td></tr><tr key=0-1-5><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> url</span></td><td key=1><span>string</span></td><td key=2>必须</td><td key=3></td><td key=4><span>访问地址</span></td><td key=5></td></tr><tr key=0-2><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> msg</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span>提示消息</span></td><td key=5></td></tr>
               </tbody>
              </table>
            
## 获取资源类别
<a id=获取资源类别> </a>
### 基本信息

**Path：** /api/resapi/v1/resources/classifications

**Method：** GET

**接口描述：**
<p>正式环境地址：<a href="http://res-api.nobook.com/api/resapi/v1/resources/get">https://res-api.nobook.com/api/resapi/v1/resources/</a>classifications</p>
<p><br data-tomark-pass=""><br>
<br data-tomark-pass=""><br>
目前只有物理有资源类别</p>


### 请求参数
**Query**

| 参数名称  |  是否必须 | 示例  | 备注  |
| ------------ | ------------ | ------------ | ------------ |
| pid | 是  |  NiFEjb83nJL4 |  物理:NiFEjb83nJL4,生物:JuFhE84jRhEh,高中生物:EjEViMk33jNr,科学:JeFJeFvReEI9,化学:IfKiInEcZu9c |
| token | 是  |  1232423423423wwrw |  token |

### 返回数据

<table>
  <thead class="ant-table-thead">
    <tr>
      <th key=name>名称</th><th key=type>类型</th><th key=required>是否必须</th><th key=default>默认值</th><th key=desc>备注</th><th key=sub>其他信息</th>
    </tr>
  </thead><tbody className="ant-table-tbody"><tr key=0-0><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> code</span></td><td key=1><span>number</span></td><td key=2>非必须</td><td key=3></td><td key=4><span>200-成功  500-失败</span></td><td key=5></td></tr><tr key=0-1><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> data</span></td><td key=1><span>object []</span></td><td key=2>非必须</td><td key=3></td><td key=4><span></span></td><td key=5><p key=3><span style="font-weight: '700'">item 类型: </span><span>object</span></p></td></tr><tr key=0-2><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> msg</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span>提示消息</span></td><td key=5></td></tr>
               </tbody>
              </table>
            
# 用户资源接口

## 删除实验
<a id=删除实验> </a>
### 基本信息

**Path：** /api/resapi/v1/myresources/del

**Method：** POST

**接口描述：**
<p>正式环境地址：<a href="http://res-api.nobook.com/api/resapi/v1/myresources/get">https://res-api.nobook.com/api/resapi/v1/myresources/</a>del</p>


### 请求参数
**Headers**

| 参数名称  | 参数值  |  是否必须 | 示例  | 备注  |
| ------------ | ------------ | ------------ | ------------ | ------------ |
| Content-Type  |  application/x-www-form-urlencoded | 是  |   |   |
**Query**

| 参数名称  |  是否必须 | 示例  | 备注  |
| ------------ | ------------ | ------------ | ------------ |
| pid | 是  |  NiFEjb83nJL4 |  物理:NiFEjb83nJL4,化学:IfKiInEcZu9c |
| token | 是  |  3234234234234ewwer |  token |
| id | 是  |   |  实验id |

### 返回数据

<table>
  <thead class="ant-table-thead">
    <tr>
      <th key=name>名称</th><th key=type>类型</th><th key=required>是否必须</th><th key=default>默认值</th><th key=desc>备注</th><th key=sub>其他信息</th>
    </tr>
  </thead><tbody className="ant-table-tbody"><tr key=0-0><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> code</span></td><td key=1><span>number</span></td><td key=2>非必须</td><td key=3></td><td key=4><span>200-成功  500-失败</span></td><td key=5></td></tr><tr key=0-1><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> data</span></td><td key=1><span>object</span></td><td key=2>非必须</td><td key=3></td><td key=4><span></span></td><td key=5></td></tr><tr key=0-2><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> msg</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span>提示信息</span></td><td key=5></td></tr>
               </tbody>
              </table>
            
## 获取资源列表
<a id=获取资源列表> </a>
### 基本信息

**Path：** /api/resapi/v1/myresources/get

**Method：** GET

**接口描述：**
<p>正式环境地址：<a href="http://res-api.nobook.com/api/resapi/v1/myresources/get">https://res-api.nobook.com/api/resapi/v1/myresources/get</a><br data-tomark-pass=""><br>
<br data-tomark-pass=""><br>
<br data-tomark-pass=""><br>
<br data-tomark-pass=""></p>


### 请求参数
**Query**

| 参数名称  |  是否必须 | 示例  | 备注  |
| ------------ | ------------ | ------------ | ------------ |
| pid | 是  |  NiFEjb83nJL4 |  物理:NiFEjb83nJL4,化学:IfKiInEcZu9c |
| token | 是  |  3234234234234ewwer |  token |

### 返回数据

<table>
  <thead class="ant-table-thead">
    <tr>
      <th key=name>名称</th><th key=type>类型</th><th key=required>是否必须</th><th key=default>默认值</th><th key=desc>备注</th><th key=sub>其他信息</th>
    </tr>
  </thead><tbody className="ant-table-tbody"><tr key=0-0><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> code</span></td><td key=1><span>number</span></td><td key=2>非必须</td><td key=3></td><td key=4><span>200-成功  500-失败</span></td><td key=5></td></tr><tr key=0-1><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> data</span></td><td key=1><span>object</span></td><td key=2>非必须</td><td key=3></td><td key=4><span></span></td><td key=5></td></tr><tr key=0-1-0><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> error</span></td><td key=1><span>null</span></td><td key=2>非必须</td><td key=3></td><td key=4><span></span></td><td key=5></td></tr><tr key=0-1-1><td key=0><span style="padding-left: 20px"><span style="color: #8c8a8a">├─</span> data</span></td><td key=1><span>string []</span></td><td key=2>非必须</td><td key=3></td><td key=4><span>实验数据</span></td><td key=5><p key=3><span style="font-weight: '700'">item 类型: </span><span>string</span></p></td></tr><tr key=0-2><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> msg</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span>提示信息</span></td><td key=5></td></tr>
               </tbody>
              </table>
            
## 重命名实验
<a id=重命名实验> </a>
### 基本信息

**Path：** /api/resapi/v1/myresources/rename

**Method：** POST

**接口描述：**
<p>正式环境地址：<a href="http://res-api.nobook.com/api/resapi/v1/myresources/get">https://res-api.nobook.com/api/resapi/v1/myresources/</a><span class="colour" style="color:rgba(13, 27, 62, 0.65)">rename</span><br data-tomark-pass=""><br>
<br data-tomark-pass=""></p>


### 请求参数
**Headers**

| 参数名称  | 参数值  |  是否必须 | 示例  | 备注  |
| ------------ | ------------ | ------------ | ------------ | ------------ |
| Content-Type  |  application/x-www-form-urlencoded | 是  |   |   |
**Query**

| 参数名称  |  是否必须 | 示例  | 备注  |
| ------------ | ------------ | ------------ | ------------ |
| pid | 是  |  NiFEjb83nJL4 |  物理:NiFEjb83nJL4,化学:IfKiInEcZu9c |
| token | 是  |  3234234234234ewwer |  token |
| id | 是  |   |  实验id |
| title | 是  |   |  实验名称 |

### 返回数据

<table>
  <thead class="ant-table-thead">
    <tr>
      <th key=name>名称</th><th key=type>类型</th><th key=required>是否必须</th><th key=default>默认值</th><th key=desc>备注</th><th key=sub>其他信息</th>
    </tr>
  </thead><tbody className="ant-table-tbody"><tr key=0-0><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> code</span></td><td key=1><span>number</span></td><td key=2>非必须</td><td key=3></td><td key=4><span>200-成功  500-失败</span></td><td key=5></td></tr><tr key=0-1><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> data</span></td><td key=1><span>object</span></td><td key=2>非必须</td><td key=3></td><td key=4><span></span></td><td key=5></td></tr><tr key=0-2><td key=0><span style="padding-left: 0px"><span style="color: #8c8a8a"></span> msg</span></td><td key=1><span>string</span></td><td key=2>非必须</td><td key=3></td><td key=4><span>提示信息</span></td><td key=5></td></tr>
               </tbody>
              </table>
            
# SDK标准化接口

## 保存实验接口-postMessage
<a id=保存实验接口-postMessage> </a>
### 基本信息

**Path：** //

**Method：** GET

**接口描述：**
<p>此接口为针对iframe交互的保存接口，如下<br>
html部分</p>
<pre><code>&lt;iframe id="editIframeId" src="http://localhost:3033/#/physics-courseware?token=5415ff907eb2b1f4e7540c158883c833&amp;uid=204521&amp;labid=5afe79f13e89089a70fb0c50"&gt;&lt;/iframe&gt;

</code></pre>
<p>主窗口发送保存命令</p>
<pre><code>$('#editIframeId')[0].contentWindow.postMessage({type: 'PHYSICS_SDK_INTERFACE_SAVE'}, '*');

</code></pre>
<p>保存完成后主窗口监听</p>
<pre><code>window.addEventListener('message', (event) =&gt; {
    const data = event.data || {};
    if (data.type === 'PHYSICS_SDK_INTERFACE_SAVE_RESPONSE') {        // 保存实验回调
        this._saveData_resolve(data.result);// {success:状态, id:实验id}
        this._saveData_resolve = null;
    }});

</code></pre>


### 请求参数

### 返回数据

```javascript
{
   "$schema": "http://json-schema.org/draft-04/schema#",
   "type": "object",
   "properties": {
      "code": {
         "type": "number",
         "description": "200：成功   500:失败"
      },
      "data": {
         "type": "object",
         "properties": {
            "nickname": {
               "type": "null",
               "description": "用户昵称"
            },
            "uid": {
               "type": "string",
               "description": "用户唯一ID"
            },
            "token": {
               "type": "string",
               "description": "TOKEN"
            },
            "vip": {
               "type": "number",
               "description": "1.VIP  0.非VIP"
            },
            "vip_endtime": {
               "type": "string",
               "description": "VIP过期时间"
            }
         }
      },
      "msg": {
         "type": "string",
         "description": "提示信息"
      }
   }
}
```
## 刷新实验场景接口-postMessage
<a id=刷新实验场景接口-postMessage> </a>
### 基本信息

**Path：** //_1526630818205

**Method：** GET

**接口描述：**
<p>此接口为针对iframe交互的刷新实验场景接口(在修改iframe地址后会刷新一次实验场景~~~~)，如下<br>
html部分</p>
<pre><code>&lt;iframe id="editIframeId" src="http://localhost:3033/#/physics-courseware?token=5415ff907eb2b1f4e7540c158883c833&amp;uid=204521&amp;labid=5afe79f13e89089a70fb0c50"&gt;&lt;/iframe&gt;

</code></pre>
<p>主窗口发送保存命令</p>
<pre><code>$('#editIframeId')[0].contentWindow.postMessage({type: 'PHYSICS_SDK_INTERFACE_FRESH_DATA'}, '*');

</code></pre>


### 请求参数

### 返回数据

```javascript
{
   "$schema": "http://json-schema.org/draft-04/schema#",
   "type": "object",
   "properties": {
      "code": {
         "type": "number",
         "description": "200：成功   500:失败"
      },
      "data": {
         "type": "object",
         "properties": {
            "nickname": {
               "type": "null",
               "description": "用户昵称"
            },
            "uid": {
               "type": "string",
               "description": "用户唯一ID"
            },
            "token": {
               "type": "string",
               "description": "TOKEN"
            },
            "vip": {
               "type": "number",
               "description": "1.VIP  0.非VIP"
            },
            "vip_endtime": {
               "type": "string",
               "description": "VIP过期时间"
            }
         }
      },
      "msg": {
         "type": "string",
         "description": "提示信息"
      }
   }
}
```
## 物理播放器地址
<a id=物理播放器地址> </a>
### 基本信息

**Path：** https://wuliplayer.nobook.com?sourceid=5af171b8f2c2a22972fc04ab

**Method：** GET

**接口描述：**
<p>此地址为物理播放器地址,外部调用可用iframe嵌入,格式如下：<br>
<a href="https://wuliplayer.nobook.com?sourceid=5af171b8f2c2a22972fc04ab">https://wuliplayer.nobook.com?sourceid=5af171b8f2c2a22972fc04ab</a><br>
其中&nbsp;sourceid 为实验id</p>


### 请求参数

### 返回数据

```javascript
{
   "$schema": "http://json-schema.org/draft-04/schema#",
   "type": "object",
   "properties": {}
}
```
## 物理编辑器地址
<a id=物理编辑器地址> </a>
### 基本信息

**Path：** https://wuli.nobook.com/#/physics-courseware?token=5be1bad337baf2892947bd6b4957f5f4&uid=204521&labid=5afe79f13e89089a70fb0c50

**Method：** GET

**接口描述：**
<p>此地址为物理编辑器地址,外部调用可用iframe嵌入,格式如下：<br>
<a href="https://wuli.nobook.com/#/physics-courseware?token=5be1bad337baf2892947bd6b4957f5f4&amp;uid=204521&amp;labid=5afe79f13e89089a70fb0c50">https://wuli.nobook.com/#/physics-courseware?token=5be1bad337baf2892947bd6b4957f5f4&amp;uid=204521&amp;labid=5afe79f13e89089a70fb0c50</a><br>
其中&nbsp;labid 为实验id<br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;uid 为用户id<br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;token 从登陆信息中拿</p>


### 请求参数

### 返回数据

```javascript
{
   "$schema": "http://json-schema.org/draft-04/schema#",
   "type": "object",
   "properties": {}
}
```