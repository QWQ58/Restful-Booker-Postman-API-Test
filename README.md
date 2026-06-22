# Restful-Booker 接口测试项目

## 项目简介

本项目使用 Postman 对 Restful-Booker 预订系统接口进行测试，覆盖登录鉴权、创建预订、查询预订、更新预订、删除预订以及删除后校验等核心接口流程。

通过本项目，练习了接口测试中的请求构造、环境变量管理、响应断言、接口关联、Collection Runner 批量执行等常用能力。

## 被测系统

- 项目名称：Restful-Booker
- 接口地址：https://restful-booker.herokuapp.com
- 接口文档：https://restful-booker.herokuapp.com/apidoc/index.html

## 使用工具

- Postman
- Postman Environment
- Post-response Scripts
- JavaScript Assertions
- Collection Runner

## 测试范围

本项目覆盖以下接口流程：

| 编号 | 接口名称 | 请求方法 | 接口路径 | 测试目标 |
| --- | --- | --- | --- | --- |
| 01 | 健康检查 | GET | `/ping` | 验证服务是否可用 |
| 02 | 登录获取 token | POST | `/auth` | 获取鉴权 token |
| 03 | 创建 booking | POST | `/booking` | 创建预订并保存 bookingid |
| 04 | 查询 booking 详情 | GET | `/booking/{{bookingid}}` | 验证创建后的预订信息 |
| 05 | 更新完整 booking | PUT | `/booking/{{bookingid}}` | 更新预订信息 |
| 06 | 查询更新后的 booking | GET | `/booking/{{bookingid}}` | 验证更新结果 |
| 07 | 删除 booking | DELETE | `/booking/{{bookingid}}` | 删除预订 |
| 08 | 确认 booking 已删除 | GET | `/booking/{{bookingid}}` | 验证删除后返回 404 |

## 环境变量

项目中使用 Postman 环境变量管理接口地址、token 和 bookingid。

| 变量名 | 说明 | 示例 |
| --- | --- | --- |
| `base_url` | 接口基础地址 | `https://restful-booker.herokuapp.com` |
| `token` | 登录接口返回的鉴权 token | 由脚本自动保存 |
| `bookingid` | 创建 booking 后返回的预订 ID | 由脚本自动保存 |

## 接口关联

本项目使用 Post-response Scripts 实现接口之间的数据传递。

登录接口获取 token：

```javascript
var jsonData = pm.response.json();
pm.environment.set("token", jsonData.token);
```

创建 booking 后保存 bookingid：

```javascript
var jsonData = pm.response.json();
pm.environment.set("bookingid", jsonData.bookingid);
```

后续接口通过变量引用：

```text
{{base_url}}/booking/{{bookingid}}
```

## 断言内容

项目中主要编写了以下类型的断言：

- 状态码断言
- 字段存在性断言
- 字段值断言
- 字段类型断言
- 删除后 404 校验

示例：

```javascript
pm.test("状态码是 200", function () {
    pm.response.to.have.status(200);
});

pm.test("firstname 是 Yun", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.firstname).to.eql("Yun");
});
```

## 运行方式

1. 打开 Postman。
2. 导入 Collection 文件。
3. 导入 Environment 文件。
4. 在右上角选择 `Restful-Booker测试环境`。
5. 打开 Collection Runner。
6. 按以下顺序执行全部接口：

```text
01_健康检查
02_登录获取token
03_创建booking
04_查询booking详情
05_更新完整booking
06_查询更新后的booking
07_删除booking
08_确认booking已删除
```

7. 查看运行结果，确认 `Failed = 0`、`Errors = 0`。

## 运行结果

本项目已通过 Collection Runner 批量执行，执行结果如下：

```text
All tests: 34
Passed: 34
Failed: 0
Errors: 0
```

## 项目收获

通过本项目，掌握了接口测试的基本流程：

- 使用 Postman 构造 GET、POST、PUT、DELETE 请求
- 使用 JSON Body 传递请求参数
- 使用 Headers 配置 `Content-Type`、`Accept` 和鉴权信息
- 使用环境变量管理公共地址和动态数据
- 使用 Post-response Scripts 提取响应字段
- 编写接口断言自动校验响应结果
- 使用 Collection Runner 批量执行接口测试
- 根据状态码定位问题，如 400、403、404、405 等

## 简历描述

Restful-Booker 接口测试项目：基于 Postman 完成 Restful-Booker 预订系统接口测试，覆盖登录鉴权、创建预订、查询、更新、删除及删除后校验等核心流程。使用环境变量管理 `base_url`、`token`、`bookingid`，通过 Post-response Scripts 自动提取接口返回值，并编写状态码、字段值、字段类型、字段存在性等断言，最终使用 Collection Runner 批量执行 34 条测试断言，结果全部通过。
