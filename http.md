[TOC]



# 状态码

## 4XX

### 400

1.前端传的格式不对,用 RequestBody 修饰 前端要传json格式,后端接收实体EquitySupplierVO  是Date类型

2.没加host属性

## 404

tomcat配置 注意下

如果本地配置host 用域名访问   Application context  为/         并且后面加8080  因为默认http端口是80

报404错误说明服务已经启动

## 415

用 RequestBody 修饰 前端要传json格式