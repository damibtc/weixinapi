#联系作者微信

![20190923155502.png](https://raw.githubusercontent.com/damibtc/weixinapi/master/img/weixin.jpg)

## 第一步
### 开发者登录
####  获取API token

> POST  http://域名地址/foreign/auth/login 
> 请求参数 Request Form Data Parameters 

| 参数名         | 类型   | 说明       |
|----------------|--------|------------|
| password`必填` | string | 开发者密码 |
| phone`必填`    | string | 开发者账号 |
 
> 返回说明

| 字段名 | 类型   | 说明           |
|--------|--------|----------------|
| code   | int    | 1成功，0失败   |
| msg    | string | 反馈信息       |
| token  | string | 有效期交互密钥 |

---
+ `注意：`
    + 参数格式是Form Data格式、非Json格式
    + 此接口无需包装Headers信息

### 获取微信登录二维码
####  获取二维码图片

> POST  http://域名地址/foreign/message/scanNew 
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |
 
> 请求参数 Form Data Parameters

| 参数名             | 类型   | 说明                                                                              |
|--------------------|--------|-----------------------------------------------------------------------------------|
| extend             | string | 用户自定义内容，回调时返回                                                        |
| account            | string | 微信号(微信第一次扫码不传，以后必须传，否则您可能会出现登录设备已满，封号等情况） |
| callback_url`必填` | string | 回调地址url（用于接收二维码、开发者需提供外网可访  访问的接口方法）               |

> 返回说明

| 字段名  | 类型   | 说明         |
|---------|--------|--------------|
| code    | int    | 1成功，0失败 |
| msg     | string | 反馈信息     |
| task_id | int    | 任务Id       |

> Callback_url回调参数

|---------|--------|------------------------------|
| code    | int    | 1成功，0失败                 |
| url     | string | 二维码图片（有可能需要转义） |
| task_id | int    | 任务Id                       |
| extend  | string | 自定义字段                   |
|         |        |                              |
|         |        |                              |
|         |        |                              |
|         |        |                              |
|         |        |                              |
|         |        |                              |
|         |        |                              |
|         |        |                              |
|         |        |                              |
|         |        |                              |
|         |        |                              |
|         |        |                              |

>自定义字段
```javascript

{
	"url": "https://xxxx/b0fdf40df44c3431601e59e988ee9f3.jpg",
	"task_id": 40074,
	"extend": ""
}
```
----
+ `注意：`
    + 新微信号第一次调用此接口不传account，第二次至N次都要传account字段。
    + 您编写的回调接口一定是Post方式且是表单
    + 可以配置上下线回调地址获取扫码成功后的微信登录信息，缓存至数据库，下次对比是否是第一次登录

### 全局回调配置
####  配置所有微信回调通知
> POST  http://域名地址/foreign/user/setUrl
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |
> 请求参数 Form Data Parameters

| 参数名             | 类型   | 说明                                                                              |
|--------------------|--------|-----------------------------------------------------------------------------------|
| callbackSend   `必填`          | string |  通用接口                                              |
| delfriendlog            | string |  删除好友回调接口地址 |
| newfriendlog | string |  添加好友通知（待同意）               |
| messagelog | string |  单聊消息接口               |
| crowdlog | string |  群聊消息接口               |
| addfriendlog | string |  已添加好友通知（会话列表通知）               |
| wacatout | string |  接收微信状态接口地址（扫码成功后，会发送到此地址，上线，下线通知）              |
| addgrouplog | string |   入群回调              |

> 返回说明

| 字段名  | 类型   | 说明         |
|---------|--------|--------------|
| code    | int    | 1成功，0失败 |
| msg     | string | 反馈信息     |



## 账户管理

### 退出微信

####  退出微信
> POST  http://域名地址/foreign/wacat/out
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |
 
> 请求参数 Form Data Parameters

| 参数名           | 类型   | 说明       |
|------------------|--------|------------|
| my_account`必填` | string | 登录微信号 |
 
> 返回说明

| 字段名 | 类型   | 说明         |
|--------|--------|--------------|
| code   | int    | 1成功，0失败 |
| msg    | string | 反馈信息     |
 
### 查看微信在线状态

####   查看微信在线状态

> POST  http://域名地址/foreign/message/wxStatus
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |
 
> 请求参数 Form Data Parameters

| 参数名           | 类型   | 说明       |
|------------------|--------|------------|
| my_account`必填` | string | 登录微信号 |
 
> 返回说明

| 字段名 | 类型   | 说明         |
|--------|--------|--------------|
| code   | int    | 1成功，0失败 |
| msg    | string | 反馈信息     |

## 好友管理


###   添加好友
####   添加好友申请

> POST  http://域名地址/foreign/friends/add
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |
 
> 请求参数 Form Data Parameters

| 参数名           | 类型   | 说明                                              |
|------------------|--------|---------------------------------------------------|
| extend           | string | 自定义字段，通用回调--'加好友通知'原样返回        |
| my_account`必填` | string | 登录微信号                                        |
| account`必填`    | string | 微信号&#124; 手机号（多个用英文状态下的逗号隔开） |
| content`必填`    | string | 验证消息                                          |
 
 

> 返回说明

| 字段名 | 类型   | 说明         |
|--------|--------|--------------|
| code   | int    | 1成功，0失败 |
| msg    | string | 反馈信息     |

###  删除好友
####   删除好友申请

> POST  http://域名地址/foreign/friends/del
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |
 
> 请求参数 Form Data Parameters

| 参数名           | 类型   | 说明                                       |
|------------------|--------|--------------------------------------------|
| extend           | string | 自定义字段，通用回调--'加好友通知'原样返回 |
| my_account`必填` | string | 登录微信号                                 |
| to_account`必填` | string | 好友微信号|                                |
 
 

> 返回说明

| 字段名 | 类型   | 说明         |
|--------|--------|--------------|
| code   | int    | 1成功，0失败 |
| msg    | string | 反馈信息     |

### 获取标签
#### 获取标签
> POST  http://域名地址/foreign/Wacat/getLables
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名           | 类型   | 说明                 |
|------------------|--------|----------------------|
| my_account`必填` | string | 登录微信号           |
| tags `必填`      | string | 标签id 不传返回所有| |

### 创建标签
#### 创建好友标签
> POST  http://域名地址/foreign/Wacat/addLable
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名              | 类型   | 说明                        |
|---------------------|--------|-----------------------------|
| my_account`必填`    | string | 登录微信号                  |
| lables `必填`       | string | 标签名称 多个以英文 , 分割| |
| callback_url `必填` | string | 回调地址|                   |

 >callback_url 回调地址
|------------|--------|----------|
| lable_id   | int    | 标签id   |
| account    | string | 微信号   |
| lable_name | string | 标签名称 |
|            |        |          |
|            |        |          |
|            |        |          |
|            |        |          |
|            |        |          |
|            |        |          |
|            |        |          |
|            |        |          |
|            |        |          |
|            |        |          |
|            |        |          |
|            |        |          |
|            |        |          |
|            |        |          |
|            |        |          |
|            |        |          |
|            |        |          |
|            |        |          |
|            |        |          |
|            |        |          |
 
```javascript
{
	"code": 1,
	"info": [{
		"lable_id": 6,
		"account": "Forever4664666",
		"lable_name": "你好"
	}],
	"msg": "创建成功"
}
```

### 修改标签
#### 修改标签
> POST  http://域名地址/foreign/Wacat/updLable
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名              | 类型   | 说明                  |
|---------------------|--------|-----------------------|
| account`必填`       | string | 多个 英文 , 逗号分隔  |
| my_account`必填`    | string | 登录微信号            |
| lable_id `必填`     | string | 多个 英文 , 逗号分隔| |
| callback_url `必填` | string | 回调地址|             |

### 删除标签
#### 删除标签
> POST  http://域名地址/foreign/Wacat/delLable
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名              | 类型   | 说明                  |
|---------------------|--------|-----------------------|
| my_account`必填`    | string | 登录微信号            |
| lable_id            | string | 多个 英文 , 逗号分隔| |
| callback_url `必填` | string | 回调地址|             |

### 检测僵尸粉
#### 检测僵尸粉
> POST  http://域名地址/foreign/Friends/checkZombie
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名              | 类型   | 说明           |
|---------------------|--------|----------------|
| my_account`必填`    | string | 登录微信号     |
| account`必填`       | string | 要检测的微信号 |
| callback_url `必填` | string | 回调地址|      |

 

### 搜索微信号信息
#### 搜索微信号信息

> POST  http://域名地址/foreign/wacat/newGetWacatInfo
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名           | 类型    | 说明                    |
|------------------|---------|-------------------------|
| extend           | integer | 扩展字段 回调时原样返回 |
| my_account`必填` | string  | 登录微信号              |
| account`必填`    | string  | 好友微信号              |
 
### 好友待添加列表
#### 好友待添加列表

> POST  http://域名地址/foreign/friends/newFriendList
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名           | 类型   | 说明                                         |
|------------------|--------|----------------------------------------------|
| my_account`必填` | string | 登录微信号                                   |
| status`必填`     | string | -1：所有、 0：待验证、 1：已通过 、2：已过期 |
 

####  返回说明

| 字段名 | 类型   | 说明         |
|--------|--------|--------------|
| code   | int    | 1成功，0失败 |
| msg    | string | 反馈信息     |
| data   | array  |              |
 
 #### data解析
 
| 字段名            | 类型   | 说明                           |
|-------------------|--------|--------------------------------|
| id                | int    | ID                             |
| my_account        | string | 登录微信号                     |
| account           | string | 对方微信号                     |
| nickname          | string | 微信呢称                       |
| sign              | string | 签名                           |
| headHDImgUrl      | string | 微信头像地址                   |
| status            | int    | -1所有 0待验证 1已通过 2已过期 |
| start_time        | int    | 申请添加时间                   |
| check_msg         | string | 验证消息                       |
| source            | string | 来源                           |
| sourceNickname    | string | 推荐人昵称                     |
| sourceAccount     | string | 推荐人微信                     |
| sourceGroupNumber | string | 来源群号                       |
 
### 通过好友添加申请

#### 通过好友添加申请


> POST  http://域名地址/foreign/friends/passAddFriends
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名           | 类型   | 说明       |
|------------------|--------|------------|
| my_account`必填` | string | 登录微信号 |
| account`必填`    | string | 对方微信号 |
 

####  返回说明

| 字段名 | 类型   | 说明         |
|--------|--------|--------------|
| code   | int    | 1成功，0失败 |
| msg    | string | 反馈信息     |
  
 
### 获取微信好友列表

#### 获取微信好友列表


> POST  http://域名地址/foreign/Friends/syncFriendList
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名             | 类型   | 说明       |
|--------------------|--------|------------|
| my_account`必填`   | string | 登录微信号 |
| callback_url`必填` | string | 回调地址   |
 

#### 回调字段释义

| 字段名      | 类型   | 说明       |
|-------------|--------|------------|
| my_account  | string | 登录的微信 |
| currentPage | string | 当前页码   |
| info        | array  | 信息       |
| total       | int    | 总数       |
 
#### info解析后数据

| 字段名        | 类型   | 说明                                                                           |
|---------------|--------|--------------------------------------------------------------------------------|
| account       | string | 微信号                                                                         |
| account_alias | string | 微信ID                                                                         |
| name          | string | 昵称                                                                           |
| sex           | int    | 性别：0未知，1男，2女                                                          |
| area          | string | 地区                                                                           |
| thumb         | string | 头像                                                                           |
| description   | string | 描述                                                                           |
| form_name     | string | 好友备注                                                                       |
| disturb       | int    | 是否免打扰：1是、2否 【接口为mac直接回调】3表示未开启免打扰，505表示开启免打扰 |
| v1            | string | 唯一数据                                                                       |
| tag           | string | 标签                                                                           |
 
----

+ `注意：`
    + 发送到回调接口的数据，每页100条推送到回调地址，直到所有好友推送完
  
 
### 获取微信号个人信息

####  获取微信信息

> POST  http://域名地址/foreign/message/wxInfo
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名           | 类型   | 说明   |
|------------------|--------|--------|
| my_account`必填` | string | 微信号 |
 
 
 
#### 返回字段释义：

| 字段名        | 类型   | 说明                        |
|---------------|--------|-----------------------------|
| account       | string | 微信号                      |
| account_alias | string | 微信ID                      |
| name          | string | 昵称                        |
| sex           | int    | 性别：0未知，1男，2女       |
| area          | string | 地区                        |
| decription    | string | 个人签名                    |
| thumb         | string | 头像                        |
| description   | string | 描述                        |
| is_online     | int    | 在线状态（1：在线 2：离线） |
  
 
## 消息管理

###  发送普通消息

####  发送普通消息

> POST  http://域名地址/foreign/message/send
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名             | 类型   | 说明                                                                      |
|--------------------|--------|---------------------------------------------------------------------------|
| file_name          | string | 文件名称                                                                  |
| my_account`必填`   | string | 微信号                                                                    |
| to_account`必填`   | string | 好友微信号|微信群号                                                       |
| content`必填`      | string | 内容 （语音、文件、图片都是链接地址）                                     |
| content_type`必填` | string | 1文本|2图片|4语音必须是silk文件，MP3的代码可以自己处理|5视频|6文件|15动图 |
| type               | string | 1单聊，2群聊（默认1）                                                     |
| img_md5            | string | 动图必须图片的MD5                                                         |
| img_size           | string | 动图的文件大小                                                            |
 

####  返回说明

| 字段名 | 类型   | 说明         |
|--------|--------|--------------|
| code   | int    | 1成功，0失败 |
| msg    | string | 反馈信息     |

###  发送名片消息

####  发送名片消息


> POST  http://域名地址/foreign/message/wacatCard
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名           | 类型   | 说明                                                  |
|------------------|--------|-------------------------------------------------------|
| my_account`必填` | string | 微信号                                                |
| to_account`必填` | string | 好友微信号|微信群号                                   |
| card_name`必填`  | string | 推荐人微信号/公众号微信号（支持个人名片、公众号名片） |
| type             | string | 1单聊，2群聊（默认1）                                 |

 

####  返回说明

| 字段名 | 类型   | 说明         |
|--------|--------|--------------|
| code   | int    | 1成功，0失败 |
| msg    | string | 反馈信息     |


###  发送URL链接

####  发送URL链接

> POST  http://域名地址/foreign/message/sendUrl
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名           | 类型   | 说明                  |
|------------------|--------|-----------------------|
| thumb            | string | 链接图片地址          |
| describe         | string | 链接描述              |
| my_account`必填` | string | 微信号                |
| to_account`必填` | string | 好友微信号|微信群号   |
| url`必填`        | string | 链接地址              |
| title`必填`      | string | 链接标题              |
| type             | string | 1单聊，2群聊（默认1） |

####  返回说明

| 字段名 | 类型   | 说明         |
|--------|--------|--------------|
| code   | int    | 1成功，0失败 |
| msg    | string | 反馈信息     |
 

###  发送小程序


####  发送URL链接、分享公众号、分享第三方文章等

> POST  http://域名地址/foreign/message/sendApp
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名           | 类型   | 说明                                                                        |
|------------------|--------|-----------------------------------------------------------------------------|
| page_path        | string | 小程序跳转页面                                                              |
| app_name`必填`   | string | 小程序的id (小程序相关参数如不清楚，可以发送小程序消息，通过消息回调获取到) |
| title`必填`      | string | 小程序名称                                                                  |
| thumb_key`必填`  | string | 小程序预览图密钥                                                            |
| my_account`必填`  | string |  扫码登录的微信号                                                           |
| to_account`必填` | string | 好友微信号|微信群号                                                         |
| describe         | string | 小程序描述                                                                  |
| thumb_url        | string | 小程序预览图地址                                                            |
| type             | string | 1单聊，2群聊                                                                |

####  返回说明

| 字段名 | 类型   | 说明         |
|--------|--------|--------------|
| code   | int    | 1成功，0失败 |
| msg    | string | 反馈信息     |

##  群管理

###  设置群公告
####  设置群公告


> POST  http://域名地址/foreign/Group/setGroupNotic
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名           | 类型   | 说明       |
|------------------|--------|------------|
| my_account`必填` | string | 登录微信号 |
| number`必填`     | string | 微信群号   |
| notice `必填`    | string | 公告内容   |


####  返回说明

| 字段名 | 类型   | 说明         |
|--------|--------|--------------|
| code   | int    | 1成功，0失败 |
| msg    | string | 反馈信息     |
 
###  创建微信群
####  创建微信群

> POST  http://域名地址/foreign/group/found
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名           | 类型   | 说明                                 |
|------------------|--------|--------------------------------------|
| group_name`必填` | string | 创建群昵称                           |
| extend           | string | 扩充字段,可传入自定义数据            |
| my_account`必填` | string | 登录微信号                           |
| account`必填`    | string | 微信号（多个用英文状态下的逗号隔开） |
| callback_url     | string | 回调地址url                          |


####  返回说明

| 字段名 | 类型   | 说明         |
|--------|--------|--------------|
| code   | int    | 1成功，0失败 |
| msg    | string | 反馈信息     |
 
> callback_url回调释义

|-------------|--------|------------|
| data        | array  | 返回数据   |
| account     | string | 微信群号   |
| extend      | string | 扩充字段   |
| name        | string | 微信群名称 |
| headerImage | string | 微信群头像 |
|             |        |            |
|             |        |            |
|             |        |            |
|             |        |            |
|             |        |            |
|             |        |            |
|             |        |            |
|             |        |            |
|             |        |            |


>data字段
```javascript
"data": {
"account": "22156949594@chatroom",
"extend": " ",
"name": "从来处来从去处去",
"headerImage": "",
}
```
 
###  退出微信群

####  退出微信群


> POST  http://域名地址/foreign/group/outGroup
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名           | 类型   | 说明       |
|------------------|--------|------------|
| g_number`必填`   | string | 微信群群号 |
| my_account`必填` | string | 微信号     |


####  返回说明

| 字段名 | 类型   | 说明         |
|--------|--------|--------------|
| code   | int    | 1成功，0失败 |
| msg    | string | 反馈信息     |

###  修改群名称

####  修改群名称

> POST  http://域名地址/foreign/group/setName
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名           | 类型   | 说明       |
|------------------|--------|------------|
| name`必填`       | string | 微信群名称 |
| my_account`必填` | string | 微信号     |
| g_number`必填`   | string | 微信群群号 |


####  返回说明

| 字段名 | 类型   | 说明         |
|--------|--------|--------------|
| code   | int    | 1成功，0失败 |
| msg    | string | 反馈信息     |
 
###  邀请新成员

####  邀请新成员

> POST  http://域名地址/foreign/group/addMember
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名           | 类型   | 说明       |
|------------------|--------|------------|
| account`必填`       | string | 微信群名称 |
| my_account`必填` | string | 微信号     |
| g_number`必填`   | string | 微信群群号 |


####  返回说明

| 字段名 | 类型   | 说明         |
|--------|--------|--------------|
| code   | int    | 1成功，0失败 |
| msg    | string | 反馈信息     |



####  返回说明

| 字段名 | 类型   | 说明         |
|--------|--------|--------------|
| code   | int    | 1成功，0失败 |
| msg    | string | 反馈信息     |
###  踢出群成员


####  踢出群成员


> POST  http://域名地址/foreign/group/delMember
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名           | 类型   | 说明         |
|------------------|--------|--------------|
| account`必填`    | string | 群成员微信号 |
| my_account`必填` | string | 微信号       |
| g_number`必填`   | string | 微信群群号   |


####  返回说明

| 字段名 | 类型   | 说明         |
|--------|--------|--------------|
| code   | int    | 1成功，0失败 |
| msg    | string | 反馈信息     |

###  群聊@成员

####  群聊@成员


> POST  http://域名地址/foreign/group/groupAt
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名           | 类型   | 说明                                      |
|------------------|--------|-------------------------------------------|
| account`必填`    | string | 微信群号                                  |
| my_account`必填` | string | 登录微信号 (有wxid优先传wxid)             |
| atUser`必填`     | string | 微信用户id（假如@多人，用英文逗号, 拼接） |
| content`必填`    | string | '@'+成员的昵称+空格+内容                  |


####  返回说明

| 字段名 | 类型 | 说明         |
|--------|------|--------------|
| code   | int  | 1成功，0失败 |

###  获取群二维码

####  获取群二维码


> POST  http://域名地址/foreign/group/qrcode
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名             | 类型   | 说明                                      |
|--------------------|--------|-------------------------------------------|
| my_account`必填`   | string | 登录微信号                                |
| g_number`必填`     | string | 微信群群号                                |
| callback_url`必填` | string | 微信用户id（假如@多人，用英文逗号, 拼接） |
 


####  返回说明

| 字段名 | 类型   | 说明         |
|--------|--------|--------------|
| code   | int    | 1成功，0失败 |
| msg    | string | 反馈信息     |
| data   | array  | 一维数组     |
 
> data含义
|----------------|--------|------------|
| qrcode         | string | 群聊二维码 |
| group_nickname | string | 群聊别名   |
| group_number   | string | 群号       |
| headimg        | string | 群聊头像   |
| owner          | string | 群主       |
| type           | string | 类型       |
|                |        |            |
|                |        |            |
|                |        |            |
|                |        |            |
|                |        |            |
|                |        |            |
|                |        |            |
 
 

###  自动加入群聊

#### 自动加入群聊



> POST  http://域名地址/foreign/group/groupSet
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名        | 类型   | 说明          |
|---------------|--------|---------------|
| account`必填` | string | 微信群号      |
| status        | int    | 0-关闭 1-开启 |
 
 


####  返回说明

| 字段名 | 类型   | 说明         |
|--------|--------|--------------|
| code   | int    | 1成功，0失败 |
| msg    | string | 反馈信息     |
| data   | JSON   | 一维数组     |

###  获取群详细信息


#### 获取群详细信息




> POST  http://域名地址/foreign/group/getGroupInfo
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名          | 类型   | 说明     |
|-----------------|--------|----------|
| my_account`必填`   | string | 微信群号 |
| g_number `必填` | string | 群号     |
 
 


####  返回说明

| 字段名 | 类型   | 说明         |
|--------|--------|--------------|
| code   | int    | 1成功，0失败 |
| msg    | string | 反馈信息     |
| data   | arrary | 返回数据     |

 > data数据说明

| 参数名  | 类型   | 说明             |
|---------|--------|------------------|
| name    | string | 群昵称           |
| number  | string | 群号             |
| thumb   | string | 群头像           |
| disturb | string | 是否免打扰       |
| author  | string | 群主微信号或微信 |
| data    | string | 群成员信息       |

###  获取微信群列表

#### 获取微信群列表


> POST  http://域名地址/foreign/group/groupList
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名        | 类型   | 说明                               |
|---------------|--------|------------------------------------|
| my_account`必填` | string | 微信号                             |
| type      `必填`    | int    | 1所有群 2通讯录的群                  |



####  返回说明

| 字段名 | 类型   | 说明         |
|--------|--------|--------------|
| code   | int    | 1成功，0失败 |
| msg    | string | 反馈信息     |
| data   | arrary | 返回数据     |
| page   | int    | 总个数       |
| total  | int    | 反馈信息     |

 > data数据说明

| 参数名  | 类型   | 说明             |
|---------|--------|------------------|
| number  | string | 群号             |
| name    | string | 群昵称           |
| thumb   | string | 群头像           |
| disturb | string | 是否免打扰       |
| author  | string | 群主微信号或微信 |
 
###  获取微信群群主

#### 获取微信群群主



> POST  http://域名地址/foreign/group/owner
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名           | 类型   | 说明       |
|------------------|--------|------------|
| my_account`必填` | string | 登录微信号 |
| g_number `必填`  | string | 微信群群号 |
 

####  返回说明

| 字段名 | 类型   | 说明         |
|--------|--------|--------------|
| code   | int    | 1成功，0失败 |
| msg    | string | 反馈信息     |
| data   | arrary | 返回数据     |
| author | string | 群主微信号   |

###  获取微信群成员

#### 获取微信群成员

> POST  http://域名地址/foreign/Group/getAllMembers
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名              | 类型   | 说明             |
|---------------------|--------|------------------|
| number`必填`        | string | 群号             |
| my_account `必填`   | string | 扫码登录的微信号 |
| callback_url `必填` | string | 回调地址         |
 

####  返回说明

| 字段名           | 类型   | 说明         |
|------------------|--------|--------------|
| code             | int    | 1成功，0失败 |
| msg              | string | 反馈信息     |
| my_account       | string | 微信号wxID   |
| my_account_alias | string | 微信号       |
| info             | array  | 二维数组     |

> info解析

| 参数名        | 类型   | 说明   |
|---------------|--------|--------|
| nickName      | string | 昵称   |
| displayName   | string | 备注   |
| bigHeadImgUrl | string | 头像   |
| userName      | string | 微信ID |
| number        | string | 群号   |

###  发送群邀请链接
使用场景：群人员必须高于40人生效

#### 发送群邀请链接 （需本人同意方可入群）

> POST  http://域名地址/foreign/group/invateMember
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名            | 类型   | 说明                        |
|-------------------|--------|-----------------------------|
| g_number`必填`    | string | 群号                        |
| my_account `必填` | string | 扫码登录的微信号            |
| account `必填`    | string | 要邀请的微信 【优先微信id】 |

###  自动加入群聊设置

自动加入群聊设置
如果自动加入群聊选项关闭，对于收到的入群邀请链接，可以自行决定要不要入群，要的话对这个url链接发post请求即可，这个url链接是经过我们处理过的，处理前如下

#### 自动加入群聊设置

> POST  http://域名地址/foreign/group/groupSet
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名            | 类型   | 说明                        |
|-------------------|--------|-----------------------------|
| account`必填`    | string | 微信号                       |
| status `必填` | string | 自动加入群聊 0-关闭 1-开启 |


####  返回说明

| 字段名 | 类型   | 说明         |
|--------|--------|--------------|
| code   | int    | 1成功，0失败 |
| msg    | string | 反馈信息     |

##  朋友圈
###  发送朋友圈
#### 发送朋友圈

> POST  http://域名地址/foreign/FriendCircle/release
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名            | 类型   | 说明                                                                                            |
|-------------------|--------|-------------------------------------------------------------------------------------------------|
| my_account`必填`  | string | 登录的微信号                                                                                    |
| text `必填`       | string | 发送朋友圈内容                                                                                  |
| media_url `必填`  | string | 图片url最多9张, 用{#}号分隔 &#124;  视频{#}缩略图&#124; 链接URL{#}缩略图，没有缩略图也要带上{#} |
| media_type `必填` | string | 1:图片  2:文字  3:视频                                                                          |
| callback_url      | string | 回调地址url(接收结果)                                                                           |
| title             | string | 链接标题                                                                                        |
 

####  返回说明

| 字段名 | 类型   | 说明         |
|--------|--------|--------------|
| code   | int    | 1成功，0失败 |
| msg    | string | 反馈信息     |
###  朋友圈 -朋友圈点赞

####  朋友圈 -朋友圈点赞

> POST  http://域名地址/foreign/FriendCircle/point
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名            | 类型   | 说明          |
|-------------------|--------|---------------|
| my_account`必填`  | string | 登录的微信号  |
| circle_id `必填`  | string | 朋友圈id      |
| content `必填`    | string | 评论内容      |
| point_type `必填` | string | 1-点赞 2-评论 |

####  返回说明

| 字段名 | 类型   | 说明         |
|--------|--------|--------------|
| code   | int    | 1成功，0失败 |
| msg    | string | 反馈信息     |

###  获取朋友圈列表


####  获取朋友圈列表


> POST  http://域名地址/foreign/FriendCircle/getList
> Request Headers

| Headers参数名 | 类型   | 说明           |
|---------------|--------|----------------|
| token`必填`   | string | 有效期交互密钥 |

> 请求参数 Form Data Parameters

| 参数名            | 类型   | 说明                            |
|-------------------|--------|---------------------------------|
| statusId          | string | 根据id获取指定朋友圈信息        |
| extend            | string | 自定义拓展字段 回调时会原样返回 |
| my_account`必填`  | string | 登录的微信号                    |
| to_account   | string |  查询指定微信号的个人相册                       |
| load `必填`    | integer | 评论内容                        |
| callback_url `必填` | string |  获取列表数据回调的url|

####  返回说明

| 字段名 | 类型   | 说明         |
|--------|--------|--------------|
| code   | int    | 1成功，0失败 |
| msg    | string | 反馈信息     |
| data    | array | callback_url  回调 二维数组     |
| my_account    | string | 登录的微信号     |
| info    | string | 回调内容     |
| profileImageURLString    | string | 微信用户头像     |
| userString    | string |  微信号     |
| mediaType    | string | 类别：1-图片     |
| mediaObject    | string | 图片对象     |
| imageURLStrings    | string | 朋友圈图片地址     |
| nameString    | string | 昵称     |
| statusId    | string | 朋友圈ID     |
| contentstring    | string | 朋友圈内容|
| createTime    | int | 创建时间     |
| timeString    | int | 发表时间     |

## 回调说明
### 接收单聊天消息
####  接收单聊天消息 -当登录微信号收到好友消息时，会给你们的接口发送数据
> POST  需要你方提供接口地址(自己通过接口设置，接口说明在用户类菜单里)
> 示例

```javascript

{
{
    "data": {
        "my_account": "damibtc",
        "my_name": 12,
        "to_account": "damibtc",
        "to_name": 12,
        "content": "556",
        "content_type": 1,
        "file_name": "",
        "voice_len": 0,
        "type": 1
    }
}
}
```

| 参数名            | 类型   | 说明                            |
|-------------------|--------|---------------------------------|
| my_account          | string |登录的微信号        |
| my_name            | string | 我的微信昵称 |
| to_account  | string | 好友的微信号                    |
| to_name   | string |  好友的微信昵称                       |
| content    | integer | 内容                        |
| content_type  | string |  int	1文字、2图片、3表情、4语音、5视频、6文件、7公众号名片、8url链接、9个人名片 10系统消息 11小程序 12 地图 13紅包 14转账 15动图|
| file_name  | string |  文件名|
| voice_len  | string | 语音时长 |
| type | string |  类型：1登录号发的、2好友发的|
| msgid  | string |  消息ID可用于区分是否是同一条消息|

### 接收群聊天消息
当登录微信号收到群消息时，会给你们的接口发送数据
####  接收群聊天消息
> POST   
> 示例

```javascript
{
    "data": {
        "to_account": "damibtc",
        "voice_len": 0,
        "file_name": "",
        "content_type": 1,
        "to_name": "1266",
        "my_account": "damibtc",
        "g_name": "为了美好的明天而战888",
        "g_number": "237576396656@chatroom",
        "my_name": "1266",
        "to_account_alias": "damibtc",
        "content": "663"
    }
}
```

| 参数名            | 类型   | 说明                            |
|-------------------|--------|---------------------------------|
| my_account          | string |登录的微信号        |
| my_name            | string | 我的微信昵称 |
| to_account  | string | 好友的微信号                    |
| to_name   | string |  好友的微信昵称                       |
| content    | integer | 内容                        |
| content_type  | string |  int	1文字、2图片、3表情、4语音、5视频、6文件、7公众号名片、8url链接、9个人名片 10系统消息 11小程序 12 地图 13紅包 14转账 15动图|
| file_name  | string |  文件名|
| voice_len  | string | 语音时长 |
| type | string |  类型：1登录号发的、2好友发的|
| msgid  | string |  语音时长|
| msg_source  | string |  消息的额外信息|

### 接收新增好友信息
需要你方提供接口地址(自己通过接口设置，接口说明在用户类菜单里)
####  接收新增好友信息（添加成功后的回调）
> POST   
> 示例

```javascript
{
    "data": {
        "uid": 2339,
        "my_account": "damibtc",
        "my_name": "1266",
        "to_account": "66s",
        "data": {
            "name": "damibtc",
            "sex": 1,
            "area": "",
            "thumb": "",
            "type": 2,
            "time": 1565861411,
            "v1": ""
        }
    }
}
```

| 参数名            | 类型   | 说明                            |
|-------------------|--------|---------------------------------|
| my_account          | string |登录的微信号        |
| my_name            | string | 我的微信昵称 |
| to_account  | string | 好友的微信号                    |
| data   | string |  好友信息                       |
| name    | integer | 好友昵称                        |
| account  | string | 好友微信号|
| sex  | int |  性别：0未知，1男，2女|
| area  | string | 地区 |
| thumb | string | 头像|
| type  | int |  请指定类型：1我加好友、2好友加我|
 
### 接收微信退出、微信登录消息
需要你方提供接口地址(自己通过接口设置，接口说明在用户类菜单里)
####  接收微信退出、微信登录消息
> POST   
> 示例

```javascript
{
    "data": [{
        "type": "1",
        "account": "damibtc",
        "account_alias": "大米",
        "name": "1266",
        "task_id": 34100,
        "thumb": "",
        "extend": ""
    }]
}
```

| 参数名            | 类型   | 说明                            |
|-------------------|--------|---------------------------------|
| my_account          | string |登录的微信号        |
| my_name            | string | 我的微信昵称 |
| to_account  | string | 好友的微信号                    |
| data   | string |  好友信息                       |
| name    | integer | 好友昵称                        |
| account  | string | 好友微信号|
| sex  | int |  性别：0未知，1男，2女|
| area  | string | 地区 |
| thumb | string | 头像|
| type  | int |  请指定类型：1我加好友、2好友加我|
 
 
### 接收有人加进群消息
需要你方提供接口地址(自己通过接口设置，接口说明在用户类菜单里)
####  当登录微信所有群中有人加入其中一个群时，发出通知
> POST   
> 示例

```javascript
{
    "data": {
        "g_number": "76655666@chatroom",
        "account": "damibtc",
        "name": "1266",
        "my_account": "大米"
    }
}
```

| 参数名            | 类型   | 说明                            |
|-------------------|--------|---------------------------------|
| g_number          | string |微信群号        |
| account  | string | 好友微信号|
| name  | string |  微信号别名|
| my_account  | string | 微信号 |

### 接收新加好友信息(被加)
当登录微信号有新的好友添加申请是发出幸好有的信息**微信需要设置-->【我】→【设置】→【隐私】→开启按钮【加我为朋友时需要验证】
####  设置回调通知接口中提供的newfriendlog

> POST   
> 示例

```javascript
{

  'account' => 'damibtc',
  'my_account' => '大米',
  'account_alias' => 'wxid_95z5p9j8crbc22',
  'encodeUserName' => 'v1_c8a3a6dabff12763e89bffd8cf81ff28dd185cc55150b7e36fa5ea38c0ac@stranger',
  'nickname' => '一只猫',
  'sex' => '0',
  'country' => '',
  'province' => '',
  'city' => '',
  'sign' => '',
  'headHDImgUrl' => '',
  'headImgUrl' => '',
  'check_msg' => '我是一只猫',
  'source' => '通过扫码添加',
  'sourceNickname' => '',
  'sourceAccount' => '',
  'sourceGroupNumber' => '',
}
```

| 参数名            | 类型   | 说明                            |
|-------------------|--------|---------------------------------|
| account          | string |申请人的微信号        |
| my_account  | string | 登录的微信号|
| account_alias  | string |  申请人的微信ID|
| encodeUserName  | string | 用户名 |
| nickname  | string | 昵称 |
| sex  | int | 性别：0未知，1男，2女 |
| country  | string | 国家 |
| province  | string | 省份 |
| city  | string | 城市 |
| sign  | string | 签名 |
| headHDImgUrl  | string | 头像 |
| headImgUrl  | string | 头像 |
| check_msg  | string | 微信号 |
| source  | string | 来源 |
| sourceNickname  | string | 推荐人昵称 |
| sourceAccount  | string | 推荐人微信 |
| sourceGroupNumber  | string | 来源群 在群里申请时 |


### 接收新加好友信息(被加)
当登录微信号有新的好友添加申请是发出幸好有的信息**微信需要设置-->【我】→【设置】→【隐私】→开启按钮【加我为朋友时需要验证】
####  设置回调通知接口中提供的newfriendlog

> POST   
> 示例

```javascript
{

  'account' => 'damibtc',
  'my_account' => '大米',
  'account_alias' => 'wxid_9505p9j8crbc22',
  'encodeUserName' => 'v1_c8a3a6dabff12763e89bffd8cf81ff28dd185cc55150b7e36fa5ea38c0ac@stranger',
  'nickname' => '一只猫',
  'sex' => '0',
  'country' => '',
  'province' => '',
  'city' => '',
  'sign' => '',
  'headHDImgUrl' => '',
  'headImgUrl' => '',
  'check_msg' => '我是一只猫',
  'source' => '通过扫码添加',
  'sourceNickname' => '',
  'sourceAccount' => '',
  'sourceGroupNumber' => '',
}
```

| 参数名            | 类型   | 说明                            |
|-------------------|--------|---------------------------------|
| account          | string |申请人的微信号        |
| my_account  | string | 登录的微信号|
| account_alias  | string |  申请人的微信ID|
| encodeUserName  | string | 用户名 |
| nickname  | string | 昵称 |
| sex  | int | 性别：0未知，1男，2女 |
| country  | string | 国家 |
| province  | string | 省份 |
| city  | string | 城市 |
| sign  | string | 签名 |
| headHDImgUrl  | string | 头像 |
| headImgUrl  | string | 头像 |
| check_msg  | string | 微信号 |
| source  | string | 来源 |
| sourceNickname  | string | 推荐人昵称 |
| sourceAccount  | string | 推荐人微信 |
| sourceGroupNumber  | string | 来源群 在群里申请时 |


### 接收删除好友通知
当登录微信号发起删除好友申请执行删除后，发出通知
####  需要你方提供接口地址(自己通过接口设置，接口说明在用户类菜单里)

> POST   
> 示例

```javascript
{
    "code": 0,
    "msg": "删除成功",
    "data": [{
        "to_account": "damibtc",
        "form_account": "azhi0hao"
    }]
}
```

| 参数名            | 类型   | 说明                            |
|-------------------|--------|---------------------------------|
| account          | string |删除的微信号        |
| my_account  | string | 登录的微信号|

### 接收查询微信好友信息
当登录微信号收到好友消息时，会给你们的接口发送数据
####  需要你方提供接口地址(自己通过接口设置，接口说明在用户类菜单里)

> POST   
> 示例

```javascript
{
    "account": "damibtc",
    "account_alias": "大米",
    "area": "福建福州",
    "extend": "这边是获取微信好友列表",
    "header_url": "http://wx.qlogo.cn/mmhead/ver_1/XRc2e5njrM2v84vA9kr95fiaZVtmAynRicGsj475icKOXxoj3l22saiw2icsZ2xsfn5S6VXGJOXqOwWQTucKO3Lvny7TxFe3bAAvaUfWhVgC6XU/132",
    "myAccount": "wxid_6mq1pu8n0j3r2",
    "nickName": "Gu🦑",
    "sex": 2,
    "type": 2
}
```

| 参数名            | 类型   | 说明                            |
|-------------------|--------|---------------------------------|
| type          | int |2 表示回调的是查询的微信信息        |
| account  | string | 微信号|
| account_alias  | string | wxid|
| area  | string | 地区|
| header_url  | string | 头像|
| myAccount  | string | 登录的微信号|
| nickName  | string | 昵称|
| sex  | int | 性别 0未知 1男 2女|
| extend  | string | 扩展字段【自定义】|


### 获取微信所有标签列表
当登录微信号收到好友消息时，会给你们的接口发送数据
####  需要你方提供接口地址(自己通过接口设置，接口说明在用户类菜单里)

> POST   
> 示例

```javascript
{
    "account": "damibtc",
    "account_alias": "大米",
    "area": "福建福州",
    "extend": "这边是获取微信好友列表",
    "header_url": "http://wx.qlogo.cn/mmhead/ver_1/XRc2e5njrM2v84vA9kr95fiaZVtmAynRicGsj475icKOXxoj3l22saiw2icsZ2xsfn5S6VXGJOXqOwWQTucKO3Lvny7TxFe3bAAvaUfWhVgC6XU/132",
    "myAccount": "wxid_6mq1pu8n0j3r2",
    "nickName": "Gu🦑",
    "sex": 2,
    "type": 2
}
```

| 参数名            | 类型   | 说明                            |
|-------------------|--------|---------------------------------|
| type          | int |2 表示回调的是查询的微信信息        |
| account  | string | 微信号|
| account_alias  | string | wxid|
| area  | string | 地区|
| header_url  | string | 头像|
| myAccount  | string | 登录的微信号|
| nickName  | string | 昵称|
| sex  | int | 性别 0未知 1男 2女|
| extend  | string | 扩展字段【自定义】|



### 用户手机点击扫码确认按钮的回调
需要你方提供接口地址(自己通过接口设置，接口说明在用户类菜单里)
####  返回微信扫码状态

> POST   
> 示例

```javascript
{
    "data": [{
        "account": "damibtc",
        "account_alias": "wxid_7mx5bzt1qz12",
        "name": "大米",
        "headimg": "http:\\/\\/wx.qlogo.cn\\/mmhead\\/ver_1\\/GL1FASadmu9AwAnhYXoq8Z3pAYYNh4M6CoZLu5cObjr31vaCib1nXPOacficFgYCQwagDaLKnNrooiaAdGoxUeGHLfTDz636tzSYZfjInhFo\\/0",
        "status": 8,
        "type": 4,
        "app_token": "",
        "extend": ""
    }]
}
```

| 参数名            | 类型   | 说明                            |
|-------------------|--------|---------------------------------|
| type          | int |4 表示回调的是微信扫码状态        |
| status  | int | 8用户已经点击确认按钮|
| account  | string | 微信号|
| account_alias  | string | 微信号(wxid)|
| name  | string | 昵称|
| headimg  | string | 头像|
| extend  | string | 获取二维码时自定义参数|


