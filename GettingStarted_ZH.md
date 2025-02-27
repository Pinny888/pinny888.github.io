[<img _ngcontent-c2="" src="" style="background-color: transparent;">](https://pinny888.github.io)

[English](GettingStarted.md)\|[Chinese](GettingStarted_ZH.md)|[Russian](GettingStarted_RU.md)

# 使用指南
## 步骤 1 – 注册

要开始使用，您需要创建一个账户。

请注意，要访问Pinnacle888 API，账户必须有资金。

### 步骤 2 - 获取提供的体育项目和联赛列表

您需要通过Get Sports操作获取体育项目列表。如果您对特定的联赛感兴趣，可以通过调用Get Leagues操作获取所有体育联赛。 **[Lines API](https://pinny888.github.io/docs?api=lines)**


### 步骤 3 - 下注

要下注，请查阅 “如何下注单注？”  和 “如何下注串关？” 部分。

### 步骤 4 - 查看下注状态

要查看已下注状态，您需要调用Get Bets操作。推荐使用下注ID来查看状态。

对处于 PENDING_ACCEPTANCE 状态的现场赛事投注，您可以每 5 秒调用一次 Get Bets 来获取投注的新状态，以查看它是被接受还是被拒绝。

# 如何下注单注？

### 步骤 1 – 调用 Get Fixtures 操作 **[Lines API](https://pinny888.github.io/docs?api=lines)**


这将返回当前提供的赛事列表。要获取更新，请使用增量请求（带有since参数）。

### 步骤 2 – 调用 Get Odds 操作 **[Lines API](https://pinny888.github.io/docs?api=lines)**


这将返回当前提供的赔率列表。要获取更新，请使用增量请求（带有since参数）。

### 步骤 3 – 调用 Get Line操作（可选）  **[Lines API](https://pinny888.github.io/docs?api=lines)**


如果您需要确切的投注限制，或只对某条特定线路感兴趣，可以调用Get Line操作。请注意，Get Feed响应中的限制只是一般性限制，而Get Line响应中的限制才是确切的限制。

### 步骤 4 - 下单投注API **[Bets API](https://pinny888.github.io/docs?api=bets)**


要下单投注，您需要调用Place Bet操作。

下表显示了如何将 "Get Odds" 操作的响应映射到 "Place Bet" 和 "Get Line" 请求中。

参数	"Get Odds" 响应参数 
sportId	sportId
leagueId	League Type -> id
eventId	Event Type -> id
periodNumber	Period Type -> number
team	
根据选定的赔率类型

· Spread Type
· Moneyline Type
· Team Total Points

请检查对应的 "Get Leagues" 响应 -> League Type -> homeTeamType，并设置相应的值。
示例 1 

假设:
    homeTeamType="Team1"
当:
    选定的赔率是点差类型（Spread Type） -> 客队（away）
那么:
    team=Team2

示例 2:
当:
    选定的赔率是让球类型（Moneyline Type） -> 平局（draw）
那么:
    team=Draw

示例  3: 

假设:
    homeTeamType="Team2"
当:
    选定的赔率是队伍总分类型（Team Total Points Type） -> 客队（away） 
那么:
    team=Team1
handicap	 点差类型（Spread Type) -> hdp
总分类型（Total Points Type) -> points
队伍总分类型（Team Total Points Type) -> Total Points 类型 -> points
lineId	Period Type -> lineId
altLineId	Spread Type ->altLineId
Total Points Type -> altLineId
如果您调用 "Get Line" 操作，请使用响应中的 lineId（或 altLineId）。

在下列情况下，某一赛段可以投注：

"Get Fixtures" 响应 -> 赛事类型 -> 状态为 "I" 或 "O"
赛事期间在 "Get Odds" 响应中有赔率
"Get Odds" 响应 -> 时间段类型 -> 截止时间在未来。
请注意，对于现场赛事，赔率和赛事状态（从 O/I 到 H 及反向）变化频繁。由于这些频繁变化，您可能会在Get Line响应中比死球赛事更频繁地获得状态NOT_EXISTS。

# 如何进行串关投注？

### 步骤 1 – 调用 Get Fixtures 操作. **[Lines API](https://pinny888.github.io/docs?api=lines)**

此操作将返回当前提供的赛事列表。若需更新，请使用带有 since 参数的增量请求。

### 步骤 2 – 调用 Get Odds 操作. **[Lines API](https://pinny888.github.io/docs?api=lines)**

此操作将返回当前提供的赔率列表。若需更新，请使用带有 since 参数的增量请求。

### 步骤 3 – 调用 Get Parlay Lines 操作. **[Lines API](https://pinny888.github.io/docs?api=lines)**

对于您想要下注的每个赛事和投注类型，为 Get Parlay Lines 调用构建 Leg 对象，并使用POST /line/parlay 进行串关投注操作。

#### POST /line/parlay **[Lines API](https://pinny888.github.io/docs?api=lines)**

如果响应包含无效的 Legs，删除它们并重新提交请求。

如果响应状态为“VALID”，则可以创建串关投注请求。

### 步骤 4 – 调用 Place Parlay Bet操作. **[Bets API](https://pinny888.github.io/docs?api=bets)**


使用 Get Parlay Lines 响应中的 lineId 值构造一个 Leg 列表，并指定在 Get Parlay Lines 响应中返回的 roundRobbinOptions。
