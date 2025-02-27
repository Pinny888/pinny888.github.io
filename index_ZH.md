[<img _ngcontent-c2="" src="" style="background-color: transparent;">](https://pinny888.github.io)

[English](index.md)\|[Chinese](index_ZH.md)|[Russian](index_RU.md)

# API参考

[使用指南](GettingStarted_ZH.md)

[API变更日志](ChangesLog_ZH.md) 

[常见问题](FAQs_ZH.md)

[合理使用政策](FairUsePolicy_ZH.md)

#### API参考文档:

**[Lines API](https://pinny888.github.io/docs?api=lines)**

**[Bets API](https://pinny888.github.io/docs?api=bets)**

**[Customer API](https://pinny888.github.io/docs?api=customer)**


# 概述

#### 身份验证 


API 使用 HTTP Basic 访问身份验证。始终使用 HTTPS 访问 API。

您需要发送 HTTP 请求标头，如下所示：
```
Authorization: Basic <Base64 value of UTF-8 encoded “username:password”> 
```

示例:

```
Authorization: Basic dXNlcm5hbWU6cGFzc3dvcmQ= 
```


请注意，为了访问Pinnacle888 API，您必须拥有一个已充值的资金账户。

#### 数据格式

Pinnacle888 API仅支持JSON格式。
HTTP标头的Accept字段必须设置为：
```
    Accept: application/json
```
POST HTTP 请求必须具有 JSON 正文内容，并且必须设置 Content-Type HTTP 标头：

```
    Content-Type: application/json
```

#### 压缩 

Pinnacle888 API支持HTTP压缩。我们强烈建议使用压缩，这将提供最佳性能。

请确保设置User-Agent HTTP标头，否则压缩可能无法正常工作。

#### 日期时间格式 

所有日期和时间均使用格林威治标准时间（GMT）时区，格式为ISO 8601。

#### 重复数据删除

当客户端发出网络请求时，可能会发生请求超时或返回错误状态代码，表示投注可能未被接受。这可能导致同一请求被多次发送，从而产生重复的投注。重复数据删除是一种避免在重试创建请求时产生重复投注的技术。我们会自动为您进行重复数据删除。

如果投注被接受，我们会将uniqueRequestId存储在系统中30分钟。如果您在该时间范围内再次尝试使用相同的 uniqueRequestId 下注，您将收到相应的错误提示。

所有下注请求都支持重复数据删除功能。


# API状态
您可以关注 [Pinnacle888 状态页面](https://status.pinnacle888.com/) 并订阅以获取有关 API 状态的通知。


**[Pinnacle888的开放API规](https://github.com/pinny888/api-spec/blob/main/OpenAPI)** 范托管在GitHub上。


# 免责声明

 Pinnacle888 对以任何目的使用 API 不承担任何责任，API 以“原样”和“可用”为基础提供，不提供任何明示或暗示的保证，包括但不限于适销性、适用于特定用途或不侵权的暗示保证。

 
