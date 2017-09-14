# 登录接口示例

> 维护人员：**Aorise**  
> 创建时间：2017-09-13

## 登录接口

本文主要描述APP与服务网关交互的接口信息。  

### 接口详情

NA

### 请求地址

/api/base/login

### 请求类型

POST

### Request参数

|  参数名  |  类型  |   必填   |   描述   |
| -------- |:------:|:--------:| -------- |
| username | String | required | 用户账户 |
| password | String | required | 用户密码 |

- 客户端请求Json示例

```javascript
{
    "username": "aorise",
    "password": "password"
}
```

### Response参数

|  参数名   |  类型  |   必填   |                   描述                    |
| --------- |:------:|:--------:| ----------------------------------------- |
| code      |  Int   | required | 本次操作的状态码：成功返回1 失败返回大于1 |
| msg       | String | required | 操作的成功或失败信息                      |
| name      | String | required | 姓名                                      |
| sex       | String |          | 性别                                      |
| birthday  | String |          | 生日                                      |
| idCard    | String | required | 身份证号码                                |
| telephone | String | required | 电话号码                                  |



- 服务器返回正常Json示例

```javascript
{
    "code": 1,
    "msg": "成功",
    "data": {
        "name": "aorise",
        "sex": "男",
        "birthday": "1994-09-08",
        "idCard": "43434343434343434",
        "telephone": "18900000000"
    }
}
```

- 服务器返回异常Json示例

```javascript
{
    "code": 10,
    "msg": "用户账户不存在",
    "data": null
}
```

### 备注说明
无

### 修改日志
- 【2017-09-13】     
   新增了idCard身份证字段    
