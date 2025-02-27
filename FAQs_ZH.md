[<img _ngcontent-c2="" src="" style="background-color: transparent;">](https://pinny888.github.io)

[English](FAQs.md)\|[Chinese](FAQs_ZH.md)|[Russian](FAQs_RU.md)

# 常见问题解答 
### 如何查找相关赛事？

可以使用 [Get Fixtures](https://pinny888.github.io/docs/?api=lines#tag/Fixtures/operation/Fixtures_V3_Get) 中的 parentId 将相关赛事分组到“parent”赛事。

以下几点可以帮到你：

 - 我们有不同的赛事，分别是赛前和实时赛，二者可通过liveStatus区分。
 - 在某些情况下，在一场赛事中可能会有多个实时赛事，但我们不会同时在这两个赛事上提供相同的盘口。
 - Parent赛事是指没有parentId的赛事。
 - Parent赛事始终是赛前赛事（liveStatus = 0 或 liveStatus = 2），但 MLB 联赛和电子竞技除外，其中直播赛事（liveStatus=1）可能缺少 parentid
   
 
 示例：

 ``` json
 {
    "sportId": 29,
    "last": 148671597,
    "league": [
        {
            "id": 2331,
            "name": "Norway - 1st Division",
            "events": [
                {
                    "id": 837721686,
                    "starts": "2018-04-10T17:00:00Z",
                    "home": "Viking Fk",
                    "away": "Mjondalen",
                    "rotNum": "6751",
                    "liveStatus": 1,
                    "status": "O",
                    "parlayRestriction": 2,
                    "parentId": 834342247,
                    "altTeaser": false
                },
                {
                    "id": 834342247,
                    "starts": "2018-04-10T17:00:00Z",
                    "home": "Viking Fk",
                    "away": "Mjondalen",
                    "rotNum": "6751",
                    "liveStatus": 2,
                    "status": "I",
                    "parlayRestriction": 2,
                    "altTeaser": false
                }
            ]
        },
        {
            "id": 6816,
            "name": "Norway - 1st Division Corners",
            "events": [
                {
                    "id": 837721684,
                    "starts": "2018-04-10T17:00:00Z",
                    "home": "Viking Fk (角球)",
                    "away": "Mjondalen (角球)",
                    "rotNum": "6751",
                    "liveStatus": 1,
                    "status": "O",
                    "parlayRestriction": 1,
                    "parentId": 834342247,
                    "altTeaser": false
                },
                {
                    "id": 837721615,
                    "starts": "2018-04-10T17:00:00Z",
                    "home": "Viking Fk (角球)",
                    "away": "Mjondalen (角球)",
                    "rotNum": "6751",
                    "liveStatus": 2,
                    "status": "I",
                    "parlayRestriction": 1,
                    "parentId": 834342247,
                    "altTeaser": false
                }
            ]
        }
    ]
} 
```

此处有一个赛前的Parent赛事id为834342247，它与现场赛事 id 837721686 相关联，还包括两个角球赛事，分别属于不同的联赛—— 一个是实时赛事（837721684），另一个是赛前赛事（837721615）。

引入Parent赛事消除了对轮换号码的需求。请注意，在下一个版本的 /fixtures 中，rotNum 属性将被停用。


### 盘口何时开放投注？

当 [Get Odds](https://pinny888.github.io/docs/?api=lines#tag/Odds)响应中所有以下条件都成立时，某一周期的盘口（如moneyline、spread、totals、teamtotal）才开放进行投注：
1. 周期状态 = 1
2. 盘口存在
3. 周期的截止时间在未来。


示例: 当不提供 moneyline 盘口时，整个对象将丢失。其他盘口类型也是如此。
```json
                 {
                            "lineId": 527557614,
                            "number": 1,
                            "cutoff": "2018-06-30T21:00:00Z",
                            "maxSpread": 250,
                            "maxTotal": 250,
                            "status": 1,
                            "spreads": [
                                {
                                    "hdp": -0.25,
                                    "home": 140,
                                    "away": -172
                                }
                            ],
                            "totals": [
                                {
                                    "points": 0.75,
                                    "over": -128,
                                    "under": 107
                                }
                            ]
                        }

```
 


请注意，对于现场赛事，赔率和时段状态变化相当频繁。由于这些频繁变化，您在 [Get Line](https://pinny888.github.io/docs/?api=lines#tag/Line/operation/Line_Straight_V2_Get) 响应中获得 NOT_EXISTS 状态的频率可能比赛前赛事更高。



### 如何在实时赛事中下注？

对直播延迟赛事的下注与其他投注的处理方式不同。只有在下注被接受后，才会为其分配betId。


要查找此类下注的状态，唯一方法是通过 uniqueRequestIds 查询 Get Bets:

`/v1/bets?uniqueRequestIds=86a90ab9-fca1-4703-a11c-ce329a85584e`

只要投注处于PENDING_ACCEPTANCE状态，响应将是：

```json
{
    "straightBets": [
        {
            "uniqueRequestId": "86a90ab9-fca1-4703-a11c-ce329a85584e",
            "betStatus": "PENDING_ACCEPTANCE"
        }
    ]
}

```


注意：状态可以从“待接受”变为“未接受”或“已接受”。

如果投注为“未接受”，则响应将是：

```json

{
    "straightBets": [
        {
            "uniqueRequestId": "86a90ab9-fca1-4703-a11c-ce329a85584e",
            "betStatus": "NOT_ACCEPTED"
        }
    ]
}


```

如果投注被接受，响应将包括完整的下注详情：

```json

{
    "straightBets": [
        {
            "betId": 800110193,
            "uniqueRequestId": "86a90ab9-fca1-4703-a11c-ce329a85584e",
            "wagerNumber": 1,
            "placedAt": "2017-12-20T21:57:22Z",
            "betStatus": "ACCEPTED",
            "betType": "MONEYLINE",
            "win": 2519.25,
            "risk": 11991.63,
            "oddsFormat": "AMERICAN",
            "updateSequence": 175277412,
            "sportId": 29,
            "leagueId": 5595,
            "eventId": 799939900,
            "price": -476,
            "teamName": "Universitario",
            "team1": "Universitario",
            "team2": "Club Petrolero",
            "periodNumber": 0,
            "isLive": "TRUE"
        }
    ]
}
 
```

我们实施大约6秒的实时延迟，因此第一次调用Get Bets V2接口时应在下注后6秒进行。

下注30分钟后，我们将停止返回提供的 uniqueRequestId 的响应。这是由于缓存清理以保持最佳性能。

另请注意，正在运行的投注列表不会返回任何 PENDING_ACCEPTANCE 或 NOT_ACCEPTED 的实时延迟投注。


### 我可以多久刷新一次赔率？

请参阅我们的合理使用政策。

### 你们会缓存响应吗？缓存多久？
所有具有相同参数（例如相同的客户端ID/API密钥/运动ID）feed命令请求都会缓存一分钟。

Get Odds和Get Fixtures的snapshot快照调用会缓存60秒。
Get Odds和Get Fixtures的delta增量调用不会被缓存。


### 如何只返回有数据的体育项目和联赛？

Get Sports和Get Leagues操作具有hasOfferings属性，该属性指示盘口的可用性。

### 如何计算非美元货币的最大投注金额？

我们遵循oanda.com，每24小时更新一次汇率。请注意，对于非美元货币，我们将其四舍五入为小于或等于货币转换后指定小数的最大整数。例如，最大投注金额 345.23 英镑将四舍五入为 345 英镑。

### API使用的是哪个时区？

所有时间均为GMT格林威治标准时间。

### 如何知道某个赛事是被删除或结算？

请使用Get Settled Fixtures接口查看赛事的时间段是否已结算或赛事是否被删除。

 
 
 ### 支持哪些TLS（传输层安全性）版本？
 
 为了符合安全要求，API仅支持TLS 1.2（最好）和TLS 1.1。
 
 
 ### 为什么我无法访问电子竞技（Esports）？

 所有帐户都无法访问电子竞技。要获得访问权限，请联系客服说明您的业务需求。
 

### 如何获取赔率变化？

1) 调用snapshot快照接口/odds（没有since参数）— 这将返回缓存的赔率快照。
2) 调用delta增量接口/odds（带有since参数，从快照响应中获取）— 获取自快照以来的所有更改。

Delta增量响应随着当前所有盘口的变化而变化。例如，如果我们在某个赛事上提供了周期0和周期1的赔率，并且周期1的某个盘口发生了变化，则Delta调用将仅返回周期1，包含当前提供的所有盘口，而周期0不会出现在Delta响应中。

示例：

1) 快照调用返回周期1和周期0，且两者都有moneyline和spread赔率。
2) 随后的增量调用仅返回周期1，包括moneyline和totals。
    
 => 这意味着周期0没有任何变化，而在周期1中，spread不再提供，totals现在提供，moneyline可能有新的赔率。

 
###  如何根据 /odds 中的最大交易量限制计算最大风险？

/odds操作返回最大投注量限制。

要根据最大交易量计算最大风险，对于十进制赔率格式的价格，可以使用以下公式：

如果价格 > 2，则:  
```
maxRisk = maxVolume  
```
, 否则，当价格 < 2 时: 
```
 maxRisk = maxVolume/(price - 1)

```



##### 示例:

当/odds返回以下moneyline报价时：
```json 
{
                            "lineId": 242220498,
                            "number": 1,
                            "cutoff": "2019-08-30T21:00:00Z",
                            "maxMoneyline": 250,
                            "status": 1,
                            "moneyline": {
                                "home": 1.819,
                                "away": 2.03
                            }
                        }

```
最大投注量为250。
主队的最大风险为305。
客队的最大风险为250。
