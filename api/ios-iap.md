# IOS IAP

## 获取言币信息

### 接口地址：`iap/yancoin/`

### GET  请求：获取言币类别

#### 验证：无

#### 请求参数：无

#### 响应内容

```javascript
{
    "data": [{
        "identifier": "com.easymaster.iyikaoyan.yanbi01",
        "price": 1.0,
        "desc": "1元=1言币",
        "isEnabled": true
    }, {
        "identifier": "com.easymaster.iyikaoyan.yanbi06",
        "price": 998.0,
        "desc": "998元=998言币",
        "isEnabled": true
    }],
    "retCode": 0,
    "retMsg": "成功 | Success"
}
```

## 创建言币交易记录

### 接口地址：`iap/trade/`

### POST请求：创建言币交易记录

#### 验证：需要

#### 请求参数：

* `userId`  INT 用户ID
* `identifier`  STR 言币标识，比如 com.easymaster.iyikaoyan.yanbi06
* `quantity`  INT 购买数量
* `price`  FLOAT  价格

#### 响应内容

**200**

```javascript
{
    "data": {
        "type": 0,  // 0: 购买言币（默认）；1：退款
        "transactionNumber": "525919303408569",  // 订单号
        "receipt": false,  // 交易凭据
        "receiptVerifyResult": 0,  // 苹果验证结果
        "transactionStatus": 0,  // 交易状态
        "account": {  // 交易账户
            "id": 289,
            "name": null,
            "nickname": "黄涛",
            "avatarUrl": "http://public-img.51easymaster.com/bfea1ceb-4c9e-4e29-99e3-a72d571b9dcd.jpg_400x400",
            "mobile": "15927503464",
            "yancoinBalance": 0.0  // 当前言币余额
        },
        "product": {  // 交易产品
            "identifier": "com.easymaster.iyikaoyan.yanbi06",
            "price": 998.0,
            "desc": "998元=998言币",
            "isEnabled": true
        },
        "quantity": 1,
        "amount": 998.0,
        "createdAt": "1525919303.958000"
    },
    "retCode": 0,
    "retMsg": "成功 | Success"
}
```

**400**

```javascript
{"retCode":10000,"retMsg":"请求参数错误"}
```

```javascript
{"retCode":30101,"retMsg":" 用户不存在"}
```

```javascript
{"retCode":80000,"retMsg":"言币商品不存在"}  //  言币标识或价格错误
```

### DELETE请求：取消言币交易

#### 验证：需要

#### 请求参数：

* `userId` INT 用户ID
* `transactionNumber` STR 交易记录号 （之前创建记录时服务器返回的交易号）

#### 响应内容

**200**

```javascript
{"retCode": 0, "retMsg": "成功 | Success"}
```

**400**

```javascript
{"retCode": 80003, "retMsg": "言币交易记录不存在"}  // 记录不存在；只能取消处于`初始化`状态的交易
```

## 验证交易凭据

### 接口地址：`iap/verify/`

### POST 请求

#### 验证：需要

#### 请求参数：

* `transactionNumber` STR 交易订单号（之前创建交易记录时服务器返回的）
* `receipt` STR 交易凭据，base64编码

#### 响应内容：

**200**

```javascript
{
    "data": {
        "type": 0,  // 0: 购买言币（默认）；1：退款
        "transactionNumber": "525919303408569",  // 订单号
        "receipt": true,  // 交易凭据
        "receiptVerifyResult": 0,  // 苹果验证结果
        "transactionStatus": 0,  // 交易状态
        "account": {  // 交易账户
            "id": 289,
            "name": null,
            "nickname": "黄涛",
            "avatarUrl": "http://public-img.51easymaster.com/bfea1ceb-4c9e-4e29-99e3-a72d571b9dcd.jpg_400x400",
            "mobile": "15927503464",
            "yancoinBalance": 998.0  // 充值后言币余额
        },
        "product": {  // 交易产品
            "identifier": "com.easymaster.iyikaoyan.yanbi06",
            "price": 998.0,
            "desc": "998元=998言币",
            "isEnabled": true
        },
        "quantity": 1,
        "amount": 998.0,
        "createdAt": "1525919303.958000"
    },
    "retCode": 0,
    "retMsg": "成功 | Success"
}
```

**400**

```javascript
{"retCode":80002,"retMsg":"非法凭据"}  // 验证失败、凭据已使用、商品信息不符合
```

**409**

```javascript
{"retCode":80008,"retMsg":"言币交易已验证通过，无需重复验证"}
```

```javascript
{"retCode":80009,"retMsg":"言币交易已验证为非法，无需重复验证"}
```

```javascript
{"retCode":80010,"retMsg":"言币交易已取消"}
```

**404（暂不使用）**

```javascript
{"retCode":80004,"retMsg":"凭据验证方式已变更"}  // 苹果返回未知状态或数据
```

**408**

```javascript
{"retCode":80006,"retMsg":"苹果服务器验证超时"}   // 客户端需要重新请求验证
```

**500**

```javascript
{"retCode":80005,"retMsg":"言币发放错误，请联系客服"}   // 验证OK，但是保存数据错误
```

## 查询言币交易记录

### 接口地址：`iap/history/`

### GET 请求

#### 验证：需要

#### 请求参数

* `userId` INT 用户ID

#### 响应内容：

**200**

```javascript
{
    "retCode": 0,
    "retMsg": "成功 | Success"
    "data": {
        "records": [{ // 时间倒序显示
                "type": 1,  // 1 充值  0 消费
                "amount": 998.0,
                "date": "1525919303.958000"  // 时间戳
            },
            {
                "type": 0,
                "amount": 998.0,
                "date": "1525919303.958000"
            },
        ],
        "user": {  // 用户信息
            "id": 289,
            "name": null,
            "nickname": "黄涛",
            "avatarUrl": "http://public-img.51easymaster.com/bfea1ceb-4c9e-4e29-99e3-a72d571b9dcd.jpg_400x400",
            "mobile": "15927503464",
            "yancoinBalance": 0.0 
        }
    }
}
```

**400**

```javascript
{"retCode":30101,"retMsg":" 用户不存在"}
```

## 言币购买课程

### 接口地址：`iap/buy_course/`

### POST请求

#### 验证：需要

#### 请求参数：

* `userId`      INT 用户ID
* `courseId`  INT  课程ID

#### 响应内容：

#### 200

```javascript
{"retCode": 0, "retMsg": "成功 | Success"}
```

#### 400

```javascript
{"retCode": 80007, "retMsg": "言币余额不足"}
```

```javascript
{"retCode": 40028, "retMsg": "课程不存在"}
```

```javascript
{"retCode": 30101, "retMsg": "用户不存在"}
```

#### 404

```javascript
{"retCode": 40031, "retMsg": "课程购买人数超限"}
```

```javascript
{"retCode": 40035, "retMsg": "课程已下架"}
```

#### 500

```javascript
{"retCode": 20000, "retMsg": "保存数据错误"}
```

