[<img _ngcontent-c2="" src="" style="background-color: transparent;">](https://pinny888.github.io)

[English ](FairUsePolicy.md)\|[ Chinese ](FairUsePolicy_ZH.md)\|[ Russian](FairUsePolicy_RU.md)

# 合理使用政策
本API受版权法保护，仅提供给我们的玩家、合作伙伴和代理。

该资源的使用受到以下“合理使用政策”的约束。

使用本API即表示您同意该政策，如果我们认为您的使用违反或不符合本合理使用政策，我们可在不事先通知您的情况下阻止您访问我们的网站。

我们在未事先通知的情况下，保留可随时自行决定修改本合理使用政策的权利。任何此类修改将在公开发布后立即生效。您在修改后继续使用我们的 API 和本网站，即表示您接受本合理使用政策中修改后的条款。

## 合理使用

该API提供给玩家以便进行投注使用，不得用于数据收集、抓取或任何其他用途。API的使用必须与投注活动成比例，具体情况由Pinnacle888根据个案决定。

除非 Pinnacle888 明确书面同意，否则 API 的商业用途将导致您的访问权限被永久中止。

您不得尝试或鼓励他人：

- 干扰、破坏或禁用任何API功能；
- 未经Pinnacle888书面同意，将API用于商业目的；
- 未经 Pinnacle888 事先书面批准，向任何第三方出售、出租、租赁、转授子许可、重新分发或联合发布 API

### 规则 

1\. 在`/fixtures`、`/fixtures/special`、`/odds`和`/odds/special`端点中支持Delta调用和Snapshot调用。Delta调用返回自提供的 `since` 值以来的变化。对于Delta调用 `since` 参数不能设置为 0 或 1，必须始终将其设置为上一次调用响应的最后一个属性值。Snapshot调用返回当前状态，因为不能为Snapshot调用提供 `since` 参数。

2\. 始终首先发出Snapshot调用，然后继续进行Delta调用。这将缩短响应时间并减小响应负载。因此，客户端将更快地获得赔率/赛程更新。

3\. 客户端不得在循环中为每个体育联赛或赛程调用 `/odds` 或 `/fixture` 端点。如果客户端仅对某些联赛感兴趣，则必须将`leagueIds`参数设置为所有联赛标识符。对于 `eventIds` 参数，如果客户端只对特定事件感兴趣，在这种情况下，必须在同一调用中提供所有事件标识符。

4\. 对于`/sports`调用，必须遵守以下限制：

- 对`/sports`的请求必须限制为每60秒一次。体育列表不会经常更改。活跃事件的数量是过时的功能，最终将停用。

5\. 每项运动必须遵守以下限制：

- 无论leagueIds、eventIds或islive参数如何，对`/fixtures`和`/odds`端点的快照调用必须限制为每60秒一次。

- 无论 `leagueIds`、`eventIds` 或 `islive` 参数如何，对 `/fixtures` 和 `/odds` 端点的 Delta 调用必须限制为每 5 秒一次。      

- 对`/leagues`的调用必须限制为每60秒一次。

6\. 客户端不得在循环中调用`/line`端点。此端点的目的是在投注前检查价格。

### 每项运动必须遵守以下限制：

- 对/fixtures和/odds操作的请求，如果没有since参数，必须限制为每60秒一次；
- 对/fixtures和/odds操作的请求，如果有since参数，必须限制为每5秒一次。

#### 推荐调用频率
<table>
  <tr>
    <th>API</th>
    <th>推荐API版本</th>
    <th>推荐间隔时间</th>
  </tr>
  <tr>
    <td>/sports</td>
    <td>/v3</td>
    <td>每60秒一次</td>
  </tr>
  <tr>
    <td>/leagues</td>
    <td>/v3</td>
    <td>每60秒一次</td>
  </tr>
  <tr>
    <td>/fixtures</td>
    <td>/v3</td>
    <td><p>每项运动5秒一次（有since参数）</p><p>每项运动60秒一次（无since参数）</p></td>
  </tr>
  <tr>
    <td>/fixtures/settled</td>
    <td>/v3</td>
    <td><p>每项运动5秒一次（有since参数）</p><p>每项运动60秒一次（无since参数）</p></td>
  </tr>
  <tr>
    <td>/fixtures/special</td>
    <td>/v2</td>
    <td><p>每项运动5秒一次（有since参数）</p><p>每项运动60秒一次（无since参数）</p></td>
  </tr>
  <tr>
    <td>/fixtures/special/settled</td>
    <td>/v3</td>
    <td><p>每项运动5秒一次（有since参数）</p><p>每项运动60秒一次（无since参数）</p></td>
  </tr>
  <tr>
    <td>/odds</td>
    <td>/v3</td>
    <td><p>每项运动5秒一次（有since参数）</p><p>每项运动60秒一次（无since参数）</p></td>
  </tr>
  <tr>
    <td>/odds/parlay</td>
    <td>/v3</td>
    <td><p>每项运动5秒一次（有since参数）</p><p>每项运动60秒一次（无since参数）</p></td>
  </tr>
  <tr>
    <td>/odds/special</td>
    <td>/v2</td>
    <td><p>每项运动5秒一次（有since参数）</p><p>每项运动60秒一次（无since参数）</p></td>
  </tr>
  <tr>
    <td>/odds/teaser</td>
    <td>/v1</td>
    <td>每个Teaser ID 5秒一次</td>
  </tr>
  <tr>
    <td>/inrunning</td>
    <td>/v2</td>
    <td>每2秒一次</td>
  </tr>
  <tr>
    <td>/teaser/groups</td>
    <td>/v1</td>
    <td>每60秒一次</td>
  </tr>
  <tr>
    <td>/bets (获取投注)</td>
    <td>/v3</td>
    <td>每2秒一次</td>
  </tr>
  <tr>
    <td>/betting-status</td>
    <td>/v1</td>
    <td>每秒1次</td>
  </tr>
  <tr>
    <td>/cancellationreasons</td>
    <td>/v1</td>
    <td>每10分钟一次</td>
  </tr>
  <tr>
    <td>/currencies</td>
    <td>/v2</td>
    <td>每10分钟一次</td>
  </tr>
  <tr>
    <td>/line</td>
    <td>/v2</td>
    <td>按需调用</td>
  </tr>
  <tr>
    <td>/line/parlay</td>
    <td>/v3</td>
    <td>按需调用</td>
  </tr>
  <tr>
    <td>/line/teaser</td>
    <td>/v1</td>
    <td>按需调用</td>
  </tr>
  <tr>
    <td>/line/special</td>
    <td>/v2</td>
    <td>按需调用</td>
  </tr>
  <tr>
    <td>/client/balance</td>
    <td>/v1</td>
    <td>按需调用</td>
  </tr>
</table>


# 免责声明
Pinnacle888 对以任何目的使用 API 不承担任何责任，API 以“原样”和“可用”为基础提供，不提供任何明示或暗示的保证，包括但不限于适销性、适用于特定用途或不侵权的暗示保证。
