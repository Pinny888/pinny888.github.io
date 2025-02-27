[<img _ngcontent-c2="" src="" style="background-color: transparent;">](https://pinny888.github.io)

[English](GettingStarted.md)\|[Chinese](GettingStarted_ZH.md)|[Russian](GettingStarted_RU.md)

# Как начать
## Шаг 1 – Зарегистрироваться

Для начала вам нужно создать профиль.

Обратите внимание, что для доступа к Pinnacle888 API профиль должен быть пополнен

### Шаг 2 – Получите список предлагаемых видов спорта и лиг.

Вам нужно получить список видов спорта с помощью операции Get Sports. Если вас интересуют конкретные лиги, вы можете получить все лиги видов спорта, вызвав операцию Get Leagues. **[Lines API](https://pinny888.github.io/docs?api=lines)**


### Шаг 3 – Сделать ставку

Чтобы сделать ставку, пожалуйста, ознакомьтесь с разделами «Как сделать ставку на одиночное событие?» и «Как сделать ставку на экспресс?»

### Шаг 4 – Получить ставки

Чтобы проверить статус размещённой ставки, нужно вызвать операцию Get Bets. Рекомендуемый способ использовать bet id.

Для ставок на живые события, которые находятся в состоянии PENDING_ACCEPTANCE, вы можете вызывать Get Bets каждые 5 секунд, чтобы получить новый статус ставки и узнать, была ли она принята или отклонена.

# Как сделать ставку на одиночное событие?

### Шаг 1 – Вызовите операцию Get Fixtures в **[Lines API](https://pinny888.github.io/docs?api=lines)**


Это вернёт список событий, которые в настоящее время предлагаются. Для получения обновлений используйте delta-запросы (с параметром since).

### Шаг 2 – Вызовите операцию Get Odds в **[Lines API](https://pinny888.github.io/docs?api=lines)**


Это вернёт список коэффициентов, которые в настоящее время предлагаются. Для получения обновлений используйте delta-запросы (с параметром since).

### Шаг 3 – Вызовите Get Line (по желанию) в  **[Lines API](https://pinny888.github.io/docs?api=lines)**


Вызовите операцию Get Line, если вам нужны точные лимиты ставок или если вас интересует только конкретная линия. Обратите внимание, что лимиты в ответе на Get Feed  это общие лимиты. Лимиты в ответе на Get Line  это точные лимиты.

### Шаг 4 – Разместить ставку в **[Bets API](https://pinny888.github.io/docs?api=bets)**


Чтобы сделать ставку, необходимо вызвать операцию Place Bet.

Таблица показывает, как сопоставить параметры ответа операции Get Odds с запросом Place Bet и Get Line.

Параметры	Параметр ответа Get Odds
sportId	sportId
leagueId	League Type -> id
eventId	Event Type -> id
periodNumber	Period Type -> number
team	
зависит от выбранных коэффициентов:

· Spread Type
· Moneyline Type
· Team Total Points

Проверьте значение в соответствующем ответе Get Leagues -> League Type -> homeTeamType и установите соответствующее значение.
Пример 1: 

Дано:
    homeTeamType=“Team1”
Когда:
    Выбранные коэффициенты — тип Spread -> away
Тогда:
    team=Team2

Пример 2:
Когда:
    Выбранные коэффициенты — тип Moneyline -> draw
Тогда:
    team=Draw

Пример 3:

Дано:
    homeTeamType="Team2"
When:
    Выбранные коэффициенты — Team Total Points Type -> away
Тогда:
    team=Team1
handicap	Spread Type -> hdp
Total Points Type -> points
Team Total Points Type -> Total Points Type -> points
lineId	Period Type -> lineId
altLineId	Spread Type ->altLineId
Total Points Type -> altLineId
Если вы вызываете операцию Get Line, используйте lineId (altLineId) из ответа.

Период в событии открыт для ставок, если:

В ответе Get Fixtures -> Event Type -> status имеет значение I или O
В периоде события есть коэффициенты в ответе Get Odds
В ответе Get Odds -> Period Type -> cutoff находится в будущем.
Обратите внимание, что для живых событий коэффициенты меняются довольно часто, а также изменяется статус события с O/I на H и наоборот. Из-за этих частых изменений возможно, что вы будете получать статус NOT_EXISTS в ответе Get Line чаще, чем для событий, не связанные с игровым процессом (dead ball events).

# Как сделать ставку на экспресс?

### Шаг 1 – Вызовите операцию Get Fixtures. **[Lines API](https://pinny888.github.io/docs?api=lines)**

Это вернёт список событий, которые в настоящее время предлагаются. Для получения обновлений используйте delta-запросы (с параметром since).

### Шаг 2 – Вызовите операцию Get Odds. **[Lines API](https://pinny888.github.io/docs?api=lines)**

Это вернёт список коэффициентов, которые в настоящее время предлагаются. Для получения обновлений используйте delta-запросы (с параметром since).

### Шаг 3 – Вызовите операцию Get Parlay Lines. **[Lines API](https://pinny888.github.io/docs?api=lines)**

Для каждого события и типа ставки, на который вы хотите сделать ставку, создайте объект Leg для вызова Get Parlay Lines и отправьте запрос с помощью

#### POST /line/parlay **[Lines API](https://pinny888.github.io/docs?api=lines)**

Если в ответе содержатся Invalid Legs — удалите их и повторно отправьте запрос.

Если в ответе статус = 'VALID' — можно создать запрос на размещение ставки на экспресс.

### Шаг 4 – Вызовите операцию Place Parlay Bet. **[Bets API](https://pinny888.github.io/docs?api=bets)**


Создайте список legs, используя значения lineId из ответа Get Parlay Lines, и укажите roundRobbinOptions, полученные в ответе Get Parlay Lines.
