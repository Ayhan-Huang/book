# OBS

## 教员登陆

### 接口地址：`obs/login/`

### GET请求:  获取短信验证码

#### 请求参数

* `mobile` STR 教员手机号码

#### 响应内容

**200 发送验证码成功**

```javascript
{"retCode": 0, "retMsg": "成功 | Success"}
```

**400**

```javascript
{"retCode":10000,"retMsg":"请求参数错误"}
```

```javascript
{"retCode":30117,"retMsg":"申请短信验证码过于频繁"}  // 60秒内只能申请一次
```

**404**

```javascript
{"retCode":40026,"retMsg":"手机号不存在"}
```

### POST请求: 登陆

#### 请求参数

* `mobile` STR 教员手机号
* `verifyCode` STR 短信验证码

#### 响应内容

**200**

```javascript
{
    "retCode": 0,
    "retMsg": "成功 | Success",
    "data": {
        "token": "27bfb1d539915ac31ac685818a695b21",  // 32位md5值；下次请求时，放在请求头Authorization字段中作认证
        "teacher": {
            "id": 123,
            "name": "张三"
        }
    }
}
```

**400**

```javascript
{"retCode":10000,"retMsg":"请求参数错误"}
```

```javascript
{"retCode":40023,"retMsg":"验证码不正确"}
```

**404**

```javascript
{"retCode":40026,"retMsg":"手机号不存在"}
```

## 获取今日课程安排

### 接口地址：`obs/schedule/`

### 接口验证：token

### GET: 获取教员今日课程列表

#### 请求参数

* `userId` INT 教员id

#### 响应内容

**200**

```javascript
{
    "data": [{
        "courseName": "431金融学综合 （至尊一对三）3班",  // 课程名
        "chapterName": "第2节",  // 小节名
        "broadcastId": 1381,  // 直播ID
        "broadcastStatus": 1,  // 直播状态：0 初始状态（刚排课）； 2 即将开始； 1 正在直播；7 直播暂停
        "startTime": 1523626200,  // 直播开始时间，时间戳秒数
        "endDatetime": 1523629800, // 直播结束时间，时间戳秒数
    }, {
        "courseName": "431金融学综合 （至尊一对三）3班",
        "chapterName": "第3节",
        "broadcastId": 1382,
        "broadcastStatus": 1,
        "startTime": 1523755800,
        "endDatetime": 1523759400,
    }],
    "retCode": 0,
    "retMsg": "成功 | Success"
}
```

**400**

```javascript
{"retCode":10000,"retMsg":"请求参数错误"}
```

**403**

```javascript
{"retCode":30105,"retMsg":"token错误"}
```

**404**

```javascript
{"retCode":70001,"retMsg":"老师不存在"}
```

## 开始直播

### 接口地址：`obs/live_start/`

### 接口验证：token

### POST请求：开始直播

#### 请求参数

* `userId` INT 教员ID
* `broadcastId` INT 直播课程ID

#### 响应内容

**200**

```javascript
{
    "retCode": 0,
    "retMsg": "成功 | Success",
    "data": {
        "pushUrls": "http://xxxx"  // OBS推流地址
    }
}
```

**400**

```javascript
{"retCode":10000,"retMsg":"请求参数错误"}
```

**403**

```javascript
{"retCode":30105,"retMsg":"token错误"}
```

**404**

```javascript
{"retCode":50001,"retMsg":"直播不存在"}
```

```javascript
{"retCode":50008,"retMsg":"直播已结束"}
```

**500**

```javascript
{"retCode":50011,"retMsg":"获取推流地址失败"}
```

## 暂停直播

### 接口地址：`obs/live_pause/`

### 接口验证：token

### POST请求: 暂停直播

#### 请求参数

* `userId` INT 教员ID
* `broadcastId` INT 直播课程ID

#### 响应内容

**200**

```javascript
{"retCode": 0, "retMsg": "成功 | Success"}
```

#### 400

```javascript
{"retCode":10000,"retMsg":"请求参数错误"}
```

**403**

```javascript
{"retCode":30105,"retMsg":"token错误"}
```

#### 404

```javascript
{"retCode":50001,"retMsg":"直播不存在"}
```

```javascript
{"retCode":50002,"retMsg":"直播未开始或已结束"}
```

## 继续直播

### 接口地址：`obs/live_resume/`

### 接口验证：token

### POST请求：继续直播

#### 请求参数

* `userId` INT 教员ID
* `broadcastId` INT 直播课程ID

#### 响应内容

**200**

```javascript
{
    "retCode": 0,
    "retMsg": "成功 | Success",
    "data": {
        "pushUrls": "http://xxxx"  // OBS推流地址（推流地址可能过期，所以重新获取并返回）
    }
}
```

**400**

```javascript
{"retCode":10000,"retMsg":"请求参数错误"}
```

**404**

```javascript
{"retCode":50001,"retMsg":"直播不存在"}
```

```javascript
{"retCode":50010,"retMsg":"直播未暂停"}
```

**403**

```javascript
{"retCode":30105,"retMsg":"token错误"}
```

**500**

```javascript
{"retCode":50011,"retMsg":"获取推流地址失败"}
```

## 结束直播

### 接口地址：`obs/live_stop/`

### 接口验证：token

### POST 请求：结束直播

#### 请求参数

* `userId` INT 教员ID
* `broadcastId` INT 直播课程ID

#### 响应内容

**200**

```javascript
{"retCode": 0, "retMsg": "成功 | Success"}
```

**400**

```javascript
{"retCode":10000,"retMsg":"请求参数错误"}
```

```javascript
{"retCode":50009,"retMsg":"直播未开始"}
```

**404**

```javascript
{"retCode":50001,"retMsg":"直播不存在"}
```

**403**

```javascript
{"retCode":30105,"retMsg":"token错误"}
```

**500**

```javascript
{"retCode":50012,"retMsg":"停止直播失败"}
```

