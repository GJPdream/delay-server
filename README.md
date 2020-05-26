# delay-server
使用RabbitMQ作为延迟任务的公共服务

## 简介

依赖于RabiitMQ死信队列实现的延迟任务处理服务，极大的保证消息的可用

## 如何使用
1.拉取代码
```
git clone https://github.com/WinterChenS/delay-server.git
```

2.修改application.properties

3.编译
```
cd delay-server &&
mvn clean package
```

4.运行
```
cd target &&
nohup java -jar delay-server-0.0.1-SNAPSHOT.jar &
```

5.调用

OkHttp
```java
OkHttpClient client = new OkHttpClient();

MediaType mediaType = MediaType.parse("application/json");
RequestBody body = RequestBody.create(mediaType, "{\n    \"id\": \"0923840293429384023\",\n    \"expireTime\": 3,\n    \"message\": \"hello\",\n    \"callbackPath\": \"http://127.0.0.1:8088/test/success\",\n    \"currentTime\": 29387492384,\n    \"retryCount\": 3\n}");
Request request = new Request.Builder()
  .url("http://127.0.0.1:8088/api/v1/default/delay")
  .post(body)
  .addHeader("Content-Type", "application/json")
  .addHeader("cache-control", "no-cache")
  .build();

Response response = client.newCall(request).execute();
```
接口参数描述：

参数名 | 类型| 解释 | 是否必须 | 示例
---|---|---|---|---
id | String | 请求唯一ID | 是 | 982739492347932
expireTime | Long | 过期时间(秒) | 是 | 30
message | String | 消息体（随意字符串) | 是 | {"name":"ston","message":"23424"}
callbackPath | String | 过期回调地址 | 是 | http://127.0.0.1:8088/test/success
currentTime | Long | 当前系统时间 | 是 | 29387492384
retryCount | Integer | 失败重试次数（默认无限重试）| 否 | 0

## 使用注意事项

- 该项目仅供学习，请勿在生产中使用
- 使用该服务需要保证回调接口的幂等性


