<!-- Generator: Widdershins v4.0.1 -->

<h1 id="draftout-community-api">Draftout Community API v0.2.0</h1>

> Scroll down for code samples, example requests and responses. Select a language for code samples from the tabs above or the mobile navigation menu.

Community-maintained OpenAPI description for the public Draftout stats
endpoints observed at draftoutmc.com.

This is not an official Draftout API specification. It was inferred from
live responses on 2026-09-14 and may be incomplete.

Base URLs:

* <a href="https://draftoutmc.com">https://draftoutmc.com</a>

<h1 id="draftout-community-api-stats">Stats</h1>

Leaderboards, player stats, match details, and rating history.

## getStatsLeaderboard

<a id="opIdgetStatsLeaderboard"></a>

> Code samples

```shell
# You can also use wget
curl -X GET https://draftoutmc.com/api/stats \
  -H 'Accept: application/json'

```

`GET /api/stats`

*Get the player leaderboard*

Returns competitive player statistics ordered by the selected metric.
Results can be limited, filtered by a case-insensitive username query,
and scoped to a rating era.

<h3 id="getstatsleaderboard-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|metric|query|[LeaderboardMetric](#schemaleaderboardmetric)|false|Statistic used to order the results:|
|q|query|string|false|Case-insensitive username search query.|
|limit|query|integer|false|Maximum number of rows to return. Defaults to `100`.|
|era|query|integer|false|Positive rating-era ID. When omitted, the latest era is used. Available|

#### Detailed descriptions

**metric**: Statistic used to order the results:

- `elo`: current rating.
- `winrate`: wins divided by wins plus losses; draws are excluded.
- `diff`: average goal differential across completed matches,
  calculated as player goals minus opponent goals.

Defaults to `elo`.

**era**: Positive rating-era ID. When omitted, the latest era is used. Available
eras are returned in leaderboard and player-stat responses.

#### Enumerated Values

|Parameter|Value|
|---|---|
|metric|elo|
|metric|winrate|
|metric|diff|

> Example responses

> Ranked player statistics.

```json
{
  "rows": [
    {
      "uuid": "92b63a39-b36a-445f-a94c-77ae212dcea3",
      "username": "bing_pigs",
      "elo": 1566,
      "rd": 102.36122699299446,
      "eraId": 2,
      "rankName": "Evoker III",
      "rankColor": "#D7D284",
      "matches": 16,
      "completedMatches": 12,
      "wins": 14,
      "losses": 2,
      "draws": 0,
      "winRate": 0.875,
      "averageFinishTime": 1436573.1,
      "averageGoals": 4.333333333333333,
      "ranked": true,
      "metricValue": 1566,
      "rank": 2
    }
  ],
  "metric": "elo",
  "query": "",
  "limit": 100,
  "eraId": 2,
  "eras": [
    {
      "id": 1,
      "firstMatchId": 0,
      "season": null,
      "system": "elo"
    },
    {
      "id": 2,
      "firstMatchId": 514840,
      "season": null,
      "system": "glicko2"
    }
  ]
}
```

<h3 id="getstatsleaderboard-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Ranked player statistics.|[LeaderboardResponse](#schemaleaderboardresponse)|

<aside class="success">
This operation does not require authentication
</aside>

## getPlayerStats

<a id="opIdgetPlayerStats"></a>

> Code samples

```shell
# You can also use wget
curl -X GET https://draftoutmc.com/api/stats/{username} \
  -H 'Accept: application/json'

```

`GET /api/stats/{username}`

*Get player stats and recent matches*

Returns player metadata, aggregate record information, and a paginated
list of matches for the selected filter. If the player is not found,
the endpoint still returns HTTP 200 with `player: null` and empty stats.

<h3 id="getplayerstats-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|username|path|string|true|Minecraft username.|
|page|query|integer|false|One-based page number. Defaults to `1`.|
|filter|query|[MatchFilter](#schemamatchfilter)|false|Match queue filter. Defaults to `competitive`.|
|era|query|integer|false|Positive rating-era ID. When omitted, the latest era is used. Available|

#### Detailed descriptions

**era**: Positive rating-era ID. When omitted, the latest era is used. Available
eras are returned in leaderboard and player-stat responses.

#### Enumerated Values

|Parameter|Value|
|---|---|
|filter|competitive|
|filter|quick-play|
|filter|lobby|

> Example responses

> Player stats page.

```json
{
  "player": {
    "uuid": "92b63a39-b36a-445f-a94c-77ae212dcea3",
    "username": "bing_pigs",
    "elo": 1566,
    "rd": 102.36122699299446,
    "ranked": true,
    "rank": 2,
    "rankName": "Evoker III",
    "rankColor": "#D7D284",
    "eraId": 2
  },
  "record": {
    "matches": 16,
    "completedMatches": 12,
    "wins": 14,
    "losses": 2,
    "draws": 0,
    "winRate": 0.875,
    "averageFinishTime": 1436573.1,
    "averageGoals": 4.333333333333333
  },
  "aggregate": {
    "peakElo": 1566,
    "bestStreak": 12,
    "fastestWinMs": 861628,
    "forfeitCount": 0
  },
  "matches": [
    {
      "id": 516492,
      "matchType": "competitive",
      "gameMode": "draftout",
      "worldSeedMode": "random",
      "boardMode": "random",
      "usedCommands": false,
      "outcome": "forfeited",
      "completedAt": 1789426891172,
      "durationMs": 63264,
      "participants": [
        {
          "uuid": "5351a58a-3888-41a5-b128-7c090deba90e",
          "username": "Picklefish23350",
          "won": false,
          "score": 0,
          "eloBefore": 1442,
          "eloChange": -25,
          "eloAfter": 1417,
          "rd": 108.09685979882966
        },
        {
          "uuid": "92b63a39-b36a-445f-a94c-77ae212dcea3",
          "username": "bing_pigs",
          "won": true,
          "score": 4,
          "eloBefore": 1545,
          "eloChange": 21,
          "eloAfter": 1566,
          "rd": 102.36122699299446
        }
      ]
    }
  ],
  "page": 1,
  "totalPages": 1,
  "filter": "competitive",
  "eraId": 2,
  "eras": [
    {
      "id": 1,
      "firstMatchId": 0,
      "season": null,
      "system": "elo"
    },
    {
      "id": 2,
      "firstMatchId": 514840,
      "season": null,
      "system": "glicko2"
    }
  ]
}
```

<h3 id="getplayerstats-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Player stats page.|[PlayerStatsResponse](#schemaplayerstatsresponse)|

<aside class="success">
This operation does not require authentication
</aside>

## getPlayerMatch

<a id="opIdgetPlayerMatch"></a>

> Code samples

```shell
# You can also use wget
curl -X GET https://draftoutmc.com/api/stats/{username}/{matchId} \
  -H 'Accept: application/json'

```

`GET /api/stats/{username}/{matchId}`

*Get a player's match detail*

Returns player metadata and detailed match data for the requested match.
If the match is not found for the player, the endpoint returns HTTP 200
with `match: null`.

<h3 id="getplayermatch-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|username|path|string|true|Minecraft username.|
|matchId|path|integer|true|Numeric Draftout match ID.|

> Example responses

> 200 Response

```json
{
  "player": {
    "uuid": "095be615-a8ad-4c33-8e9c-c7612fbf6c9f",
    "username": "string",
    "elo": 0,
    "rd": 0,
    "ranked": true,
    "rank": 1,
    "rankName": "Guardian I",
    "rankColor": "#45686e",
    "eraId": 1
  },
  "match": {
    "id": 0,
    "matchType": "competitive",
    "gameMode": "draftout",
    "worldSeedMode": "random",
    "boardMode": "random",
    "usedCommands": true,
    "outcome": "finished",
    "completedAt": 0,
    "durationMs": 0,
    "participants": [
      {
        "uuid": "095be615-a8ad-4c33-8e9c-c7612fbf6c9f",
        "username": "string",
        "won": true,
        "score": 0,
        "eloBefore": 0,
        "eloChange": 0,
        "eloAfter": 0,
        "rd": 0
      }
    ],
    "seed": "string",
    "goals": [
      {
        "index": 0,
        "id": "OBTAIN_64_COLORED_CONCRETE",
        "data": "string",
        "completed": true,
        "completedByUuid": "7ca0775d-ca03-4d72-a053-72fd261cf2bf",
        "completedAtMs": 0
      }
    ],
    "draft": {
      "pickedFirstUuid": "bca05434-a5ae-411f-af81-33c4e3a191c0",
      "pool": [
        {
          "id": "string",
          "data": "string",
          "picked": true,
          "timedOut": true,
          "rerolled": true
        }
      ]
    }
  }
}
```

<h3 id="getplayermatch-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Match detail or `match: null`.|[MatchDetailResponse](#schemamatchdetailresponse)|

<aside class="success">
This operation does not require authentication
</aside>

## getPlayerEloSeries

<a id="opIdgetPlayerEloSeries"></a>

> Code samples

```shell
# You can also use wget
curl -X GET https://draftoutmc.com/api/stats/{username}/elo-series \
  -H 'Accept: application/json'

```

`GET /api/stats/{username}/elo-series`

*Get a player's Elo history*

Returns the player's starting Elo and one point per competitive match
used to build an Elo chart.

<h3 id="getplayereloseries-parameters">Parameters</h3>

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|username|path|string|true|Minecraft username.|
|era|query|integer|false|Positive rating-era ID. When omitted, the latest era is used. Available|

#### Detailed descriptions

**era**: Positive rating-era ID. When omitted, the latest era is used. Available
eras are returned in leaderboard and player-stat responses.

> Example responses

> 200 Response

```json
{
  "startElo": 1259,
  "points": [
    {
      "matchId": 0,
      "completedAt": 0,
      "eloBefore": 0,
      "eloAfter": 0,
      "eloChange": 0,
      "outcome": "finished",
      "won": true,
      "opponentUuid": "63613e40-8327-45d8-8c9c-6a784ccc7ba2",
      "opponentName": "string",
      "playerScore": 0,
      "opponentScore": 0,
      "durationMs": 0
    }
  ],
  "eraId": 1
}
```

<h3 id="getplayereloseries-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Elo series.|[EloSeriesResponse](#schemaeloseriesresponse)|

<aside class="success">
This operation does not require authentication
</aside>

<h1 id="draftout-community-api-ranks">Ranks</h1>

Rating rank bands.

## getRanks

<a id="opIdgetRanks"></a>

> Code samples

```shell
# You can also use wget
curl -X GET https://draftoutmc.com/api/ranks \
  -H 'Accept: application/json'

```

`GET /api/ranks`

*Get rating rank bands*

Returns the configured Draftout rating rank bands in ascending order.
The highest band is open-ended and uses `null` for its maximum.

> Example responses

> Rank bands.

```json
[
  {
    "name": "Silverfish I",
    "min": 0,
    "max": 99,
    "color": "#ACB1B4"
  },
  {
    "name": "Guardian I",
    "min": 1800,
    "max": 1899,
    "color": "#36775F"
  },
  {
    "name": "Warden",
    "min": 2000,
    "max": null,
    "color": "#1CCEDA"
  }
]
```

<h3 id="getranks-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Rank bands.|Inline|

<h3 id="getranks-responseschema">Response Schema</h3>

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[[Rank](#schemarank)]|false|none|none|
|» name|string|true|none|none|
|» min|integer|true|none|Inclusive minimum rating for the band.|
|» max|integer,null|true|none|Maximum Elo for the band. `null` means no upper bound.|
|» color|string|true|none|none|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

<h2 id="tocS_MatchFilter">MatchFilter</h2>
<!-- backwards compatibility -->
<a id="schemamatchfilter"></a>
<a id="schema_MatchFilter"></a>
<a id="tocSmatchfilter"></a>
<a id="tocsmatchfilter"></a>

```json
"competitive"

```

Match queue included in a player's record and match list:

- `competitive`: ranked competitive matches.
- `quick-play`: unranked quick-play matches.
- `lobby`: custom lobby matches.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Match queue included in a player's record and match list:<br><br>- `competitive`: ranked competitive matches.<br>- `quick-play`: unranked quick-play matches.<br>- `lobby`: custom lobby matches.|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|competitive|
|*anonymous*|quick-play|
|*anonymous*|lobby|

<h2 id="tocS_Rank">Rank</h2>
<!-- backwards compatibility -->
<a id="schemarank"></a>
<a id="schema_Rank"></a>
<a id="tocSrank"></a>
<a id="tocsrank"></a>

```json
{
  "name": "Guardian I",
  "min": 0,
  "max": 0,
  "color": "#45686e"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|name|string|true|none|none|
|min|integer|true|none|Inclusive minimum rating for the band.|
|max|integer,null|true|none|Maximum Elo for the band. `null` means no upper bound.|
|color|string|true|none|none|

<h2 id="tocS_LeaderboardResponse">LeaderboardResponse</h2>
<!-- backwards compatibility -->
<a id="schemaleaderboardresponse"></a>
<a id="schema_LeaderboardResponse"></a>
<a id="tocSleaderboardresponse"></a>
<a id="tocsleaderboardresponse"></a>

```json
{
  "rows": [
    {
      "uuid": "095be615-a8ad-4c33-8e9c-c7612fbf6c9f",
      "username": "string",
      "elo": 0,
      "rd": 0,
      "eraId": 1,
      "rankName": "string",
      "rankColor": "string",
      "matches": 0,
      "completedMatches": 0,
      "wins": 0,
      "losses": 0,
      "draws": 0,
      "winRate": 1,
      "averageFinishTime": 0,
      "averageGoals": 0,
      "ranked": true,
      "metricValue": 0,
      "rank": 1
    }
  ],
  "metric": "elo",
  "query": "string",
  "limit": 1,
  "eraId": 1,
  "eras": [
    {
      "id": 1,
      "firstMatchId": 0,
      "season": 1,
      "system": "elo"
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|rows|[[LeaderboardRow](#schemaleaderboardrow)]|true|none|none|
|metric|[LeaderboardMetric](#schemaleaderboardmetric)|true|none|Statistic used to order leaderboard rows:<br><br>- `elo`: current rating (`elo`).<br>- `winrate`: `wins / (wins + losses)`; draws are excluded.<br>- `diff`: average goal differential across completed matches, exposed<br>  as `averageGoals` and calculated as player goals minus opponent goals.|
|query|string|true|none|Normalized username query, or an empty string when omitted.|
|limit|integer|true|none|none|
|eraId|integer|true|none|none|
|eras|[[Era](#schemaera)]|true|none|none|

<h2 id="tocS_LeaderboardRow">LeaderboardRow</h2>
<!-- backwards compatibility -->
<a id="schemaleaderboardrow"></a>
<a id="schema_LeaderboardRow"></a>
<a id="tocSleaderboardrow"></a>
<a id="tocsleaderboardrow"></a>

```json
{
  "uuid": "095be615-a8ad-4c33-8e9c-c7612fbf6c9f",
  "username": "string",
  "elo": 0,
  "rd": 0,
  "eraId": 1,
  "rankName": "string",
  "rankColor": "string",
  "matches": 0,
  "completedMatches": 0,
  "wins": 0,
  "losses": 0,
  "draws": 0,
  "winRate": 1,
  "averageFinishTime": 0,
  "averageGoals": 0,
  "ranked": true,
  "metricValue": 0,
  "rank": 1
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|uuid|string(uuid)|true|none|none|
|username|string|true|none|none|
|elo|integer|true|none|none|
|rd|number,null|true|none|Rating deviation. `null` for eras that use Elo ratings.|
|eraId|integer|true|none|none|
|rankName|string|true|none|none|
|rankColor|string|true|none|none|
|matches|integer|true|none|none|
|completedMatches|integer|true|none|none|
|wins|integer|true|none|none|
|losses|integer|true|none|none|
|draws|integer|true|none|none|
|winRate|number|true|none|none|
|averageFinishTime|number,null|true|none|Average finish time in milliseconds.|
|averageGoals|number,null|true|none|Average goal differential for completed matches.|
|ranked|boolean|true|none|none|
|metricValue|number|true|none|Value of the selected leaderboard metric.|
|rank|integer,null|true|none|Position in the full metric leaderboard; may be `null` for a searched subset.|

<h2 id="tocS_LeaderboardMetric">LeaderboardMetric</h2>
<!-- backwards compatibility -->
<a id="schemaleaderboardmetric"></a>
<a id="schema_LeaderboardMetric"></a>
<a id="tocSleaderboardmetric"></a>
<a id="tocsleaderboardmetric"></a>

```json
"elo"

```

Statistic used to order leaderboard rows:

- `elo`: current rating (`elo`).
- `winrate`: `wins / (wins + losses)`; draws are excluded.
- `diff`: average goal differential across completed matches, exposed
  as `averageGoals` and calculated as player goals minus opponent goals.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Statistic used to order leaderboard rows:<br><br>- `elo`: current rating (`elo`).<br>- `winrate`: `wins / (wins + losses)`; draws are excluded.<br>- `diff`: average goal differential across completed matches, exposed<br>  as `averageGoals` and calculated as player goals minus opponent goals.|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|elo|
|*anonymous*|winrate|
|*anonymous*|diff|

<h2 id="tocS_Era">Era</h2>
<!-- backwards compatibility -->
<a id="schemaera"></a>
<a id="schema_Era"></a>
<a id="tocSera"></a>
<a id="tocsera"></a>

```json
{
  "id": 1,
  "firstMatchId": 0,
  "season": 1,
  "system": "elo"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|integer|true|none|none|
|firstMatchId|integer|true|none|First match ID included in the era.|
|season|integer,null|true|none|none|
|system|[RatingSystem](#schemaratingsystem)|true|none|Rating algorithm used by an era:<br><br>- `elo`: the legacy Elo rating system; rating deviation is `null`.<br>- `glicko2`: Glicko-2 ratings with rating deviation in `rd`.|

<h2 id="tocS_RatingSystem">RatingSystem</h2>
<!-- backwards compatibility -->
<a id="schemaratingsystem"></a>
<a id="schema_RatingSystem"></a>
<a id="tocSratingsystem"></a>
<a id="tocsratingsystem"></a>

```json
"elo"

```

Rating algorithm used by an era:

- `elo`: the legacy Elo rating system; rating deviation is `null`.
- `glicko2`: Glicko-2 ratings with rating deviation in `rd`.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Rating algorithm used by an era:<br><br>- `elo`: the legacy Elo rating system; rating deviation is `null`.<br>- `glicko2`: Glicko-2 ratings with rating deviation in `rd`.|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|elo|
|*anonymous*|glicko2|

<h2 id="tocS_PlayerStatsResponse">PlayerStatsResponse</h2>
<!-- backwards compatibility -->
<a id="schemaplayerstatsresponse"></a>
<a id="schema_PlayerStatsResponse"></a>
<a id="tocSplayerstatsresponse"></a>
<a id="tocsplayerstatsresponse"></a>

```json
{
  "player": {
    "uuid": "095be615-a8ad-4c33-8e9c-c7612fbf6c9f",
    "username": "string",
    "elo": 0,
    "rd": 0,
    "ranked": true,
    "rank": 1,
    "rankName": "Guardian I",
    "rankColor": "#45686e",
    "eraId": 1
  },
  "record": {
    "matches": 0,
    "completedMatches": 0,
    "wins": 0,
    "losses": 0,
    "draws": 0,
    "winRate": 1,
    "averageFinishTime": 0,
    "averageGoals": 0
  },
  "aggregate": {
    "peakElo": 0,
    "bestStreak": 0,
    "fastestWinMs": 0,
    "forfeitCount": 0
  },
  "matches": [
    {
      "id": 0,
      "matchType": "competitive",
      "gameMode": "draftout",
      "worldSeedMode": "random",
      "boardMode": "random",
      "usedCommands": true,
      "outcome": "finished",
      "completedAt": 0,
      "durationMs": 0,
      "participants": [
        {
          "uuid": "095be615-a8ad-4c33-8e9c-c7612fbf6c9f",
          "username": "string",
          "won": true,
          "score": 0,
          "eloBefore": 0,
          "eloChange": 0,
          "eloAfter": 0,
          "rd": 0
        }
      ]
    }
  ],
  "page": 1,
  "totalPages": 1,
  "filter": "competitive",
  "eraId": 1,
  "eras": [
    {
      "id": 1,
      "firstMatchId": 0,
      "season": 1,
      "system": "elo"
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|player|any|true|none|none|

anyOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|[Player](#schemaplayer)|false|none|none|

or

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|null|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|record|[Record](#schemarecord)|true|none|none|
|aggregate|[Aggregate](#schemaaggregate)|true|none|none|
|matches|[[MatchSummary](#schemamatchsummary)]|true|none|none|
|page|integer|true|none|none|
|totalPages|integer|true|none|none|
|filter|[MatchFilter](#schemamatchfilter)|true|none|Match queue included in a player's record and match list:<br><br>- `competitive`: ranked competitive matches.<br>- `quick-play`: unranked quick-play matches.<br>- `lobby`: custom lobby matches.|
|eraId|integer|true|none|none|
|eras|[[Era](#schemaera)]|true|none|none|

<h2 id="tocS_MatchDetailResponse">MatchDetailResponse</h2>
<!-- backwards compatibility -->
<a id="schemamatchdetailresponse"></a>
<a id="schema_MatchDetailResponse"></a>
<a id="tocSmatchdetailresponse"></a>
<a id="tocsmatchdetailresponse"></a>

```json
{
  "player": {
    "uuid": "095be615-a8ad-4c33-8e9c-c7612fbf6c9f",
    "username": "string",
    "elo": 0,
    "rd": 0,
    "ranked": true,
    "rank": 1,
    "rankName": "Guardian I",
    "rankColor": "#45686e",
    "eraId": 1
  },
  "match": {
    "id": 0,
    "matchType": "competitive",
    "gameMode": "draftout",
    "worldSeedMode": "random",
    "boardMode": "random",
    "usedCommands": true,
    "outcome": "finished",
    "completedAt": 0,
    "durationMs": 0,
    "participants": [
      {
        "uuid": "095be615-a8ad-4c33-8e9c-c7612fbf6c9f",
        "username": "string",
        "won": true,
        "score": 0,
        "eloBefore": 0,
        "eloChange": 0,
        "eloAfter": 0,
        "rd": 0
      }
    ],
    "seed": "string",
    "goals": [
      {
        "index": 0,
        "id": "OBTAIN_64_COLORED_CONCRETE",
        "data": "string",
        "completed": true,
        "completedByUuid": "7ca0775d-ca03-4d72-a053-72fd261cf2bf",
        "completedAtMs": 0
      }
    ],
    "draft": {
      "pickedFirstUuid": "bca05434-a5ae-411f-af81-33c4e3a191c0",
      "pool": [
        {
          "id": "string",
          "data": "string",
          "picked": true,
          "timedOut": true,
          "rerolled": true
        }
      ]
    }
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|player|any|true|none|none|

anyOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|[Player](#schemaplayer)|false|none|none|

or

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|null|false|none|none|

continued

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|match|any|true|none|none|

anyOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|[MatchDetail](#schemamatchdetail)|false|none|none|

or

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» *anonymous*|null|false|none|none|

<h2 id="tocS_EloSeriesResponse">EloSeriesResponse</h2>
<!-- backwards compatibility -->
<a id="schemaeloseriesresponse"></a>
<a id="schema_EloSeriesResponse"></a>
<a id="tocSeloseriesresponse"></a>
<a id="tocseloseriesresponse"></a>

```json
{
  "startElo": 1259,
  "points": [
    {
      "matchId": 0,
      "completedAt": 0,
      "eloBefore": 0,
      "eloAfter": 0,
      "eloChange": 0,
      "outcome": "finished",
      "won": true,
      "opponentUuid": "63613e40-8327-45d8-8c9c-6a784ccc7ba2",
      "opponentName": "string",
      "playerScore": 0,
      "opponentScore": 0,
      "durationMs": 0
    }
  ],
  "eraId": 1
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|startElo|integer|true|none|none|
|points|[[EloPoint](#schemaelopoint)]|true|none|none|
|eraId|integer|true|none|none|

<h2 id="tocS_Player">Player</h2>
<!-- backwards compatibility -->
<a id="schemaplayer"></a>
<a id="schema_Player"></a>
<a id="tocSplayer"></a>
<a id="tocsplayer"></a>

```json
{
  "uuid": "095be615-a8ad-4c33-8e9c-c7612fbf6c9f",
  "username": "string",
  "elo": 0,
  "rd": 0,
  "ranked": true,
  "rank": 1,
  "rankName": "Guardian I",
  "rankColor": "#45686e",
  "eraId": 1
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|uuid|string(uuid)|true|none|none|
|username|string|true|none|none|
|elo|integer|true|none|none|
|rd|number,null|true|none|Rating deviation. `null` for eras that use Elo ratings.|
|ranked|boolean|true|none|none|
|rank|integer,null|true|none|Leaderboard rank. Observed as `null` for quick-play responses.|
|rankName|string|true|none|none|
|rankColor|string|true|none|none|
|eraId|integer|true|none|none|

<h2 id="tocS_Record">Record</h2>
<!-- backwards compatibility -->
<a id="schemarecord"></a>
<a id="schema_Record"></a>
<a id="tocSrecord"></a>
<a id="tocsrecord"></a>

```json
{
  "matches": 0,
  "completedMatches": 0,
  "wins": 0,
  "losses": 0,
  "draws": 0,
  "winRate": 1,
  "averageFinishTime": 0,
  "averageGoals": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|matches|integer|true|none|none|
|completedMatches|integer|true|none|none|
|wins|integer|true|none|none|
|losses|integer|true|none|none|
|draws|integer|true|none|none|
|winRate|number|true|none|none|
|averageFinishTime|number,null|true|none|Average finish time in milliseconds.|
|averageGoals|number,null|true|none|Average goal differential for completed matches.|

<h2 id="tocS_Aggregate">Aggregate</h2>
<!-- backwards compatibility -->
<a id="schemaaggregate"></a>
<a id="schema_Aggregate"></a>
<a id="tocSaggregate"></a>
<a id="tocsaggregate"></a>

```json
{
  "peakElo": 0,
  "bestStreak": 0,
  "fastestWinMs": 0,
  "forfeitCount": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|peakElo|integer,null|true|none|none|
|bestStreak|integer|true|none|none|
|fastestWinMs|integer,null|true|none|Fastest win duration in milliseconds.|
|forfeitCount|integer|true|none|none|

<h2 id="tocS_MatchSummary">MatchSummary</h2>
<!-- backwards compatibility -->
<a id="schemamatchsummary"></a>
<a id="schema_MatchSummary"></a>
<a id="tocSmatchsummary"></a>
<a id="tocsmatchsummary"></a>

```json
{
  "id": 0,
  "matchType": "competitive",
  "gameMode": "draftout",
  "worldSeedMode": "random",
  "boardMode": "random",
  "usedCommands": true,
  "outcome": "finished",
  "completedAt": 0,
  "durationMs": 0,
  "participants": [
    {
      "uuid": "095be615-a8ad-4c33-8e9c-c7612fbf6c9f",
      "username": "string",
      "won": true,
      "score": 0,
      "eloBefore": 0,
      "eloChange": 0,
      "eloAfter": 0,
      "rd": 0
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|integer|true|none|none|
|matchType|[MatchType](#schemamatchtype)|true|none|Queue in which the match was played:<br><br>- `competitive`: ranked competitive queue.<br>- `quick_play`: unranked quick-play queue.<br>- `lobby`: custom lobby.|
|gameMode|[GameMode](#schemagamemode)|true|none|Board rules used by the match:<br><br>- `draftout`: players draft the goals on the shared board.<br>- `lockout`: players race on a randomly generated shared board.<br>- `blackout`: players cooperate on one shared board.<br><br>Competitive and quick-play matches use `draftout`; lobby matches may<br>use any of the three modes.|
|worldSeedMode|string|true|none|World-seed selection mode. Observed as `random`.|
|boardMode|string|true|none|Board selection mode. Observed as `random`.|
|usedCommands|boolean|true|none|Whether commands were used during the match.|
|outcome|[MatchOutcome](#schemamatchoutcome)|true|none|Match result:<br><br>- `finished`: completed normally.<br>- `forfeited`: ended by a forfeit.<br>- `draw_by_vote`: players voted to end the match as a draw.<br>- `draw`: recorded as a draw.<br>- `cancelled`: cancelled without a result.|
|completedAt|integer(int64)|true|none|Unix timestamp in milliseconds.|
|durationMs|integer|true|none|Match duration in milliseconds.|
|participants|[[Participant](#schemaparticipant)]|true|none|none|

<h2 id="tocS_MatchDetail">MatchDetail</h2>
<!-- backwards compatibility -->
<a id="schemamatchdetail"></a>
<a id="schema_MatchDetail"></a>
<a id="tocSmatchdetail"></a>
<a id="tocsmatchdetail"></a>

```json
{
  "id": 0,
  "matchType": "competitive",
  "gameMode": "draftout",
  "worldSeedMode": "random",
  "boardMode": "random",
  "usedCommands": true,
  "outcome": "finished",
  "completedAt": 0,
  "durationMs": 0,
  "participants": [
    {
      "uuid": "095be615-a8ad-4c33-8e9c-c7612fbf6c9f",
      "username": "string",
      "won": true,
      "score": 0,
      "eloBefore": 0,
      "eloChange": 0,
      "eloAfter": 0,
      "rd": 0
    }
  ],
  "seed": "string",
  "goals": [
    {
      "index": 0,
      "id": "OBTAIN_64_COLORED_CONCRETE",
      "data": "string",
      "completed": true,
      "completedByUuid": "7ca0775d-ca03-4d72-a053-72fd261cf2bf",
      "completedAtMs": 0
    }
  ],
  "draft": {
    "pickedFirstUuid": "bca05434-a5ae-411f-af81-33c4e3a191c0",
    "pool": [
      {
        "id": "string",
        "data": "string",
        "picked": true,
        "timedOut": true,
        "rerolled": true
      }
    ]
  }
}

```

### Properties

allOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[MatchSummary](#schemamatchsummary)|false|none|none|

and

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|object|false|none|none|
|» seed|string|true|none|Minecraft world seed represented as a string.|
|» goals|[[Goal](#schemagoal)]|true|none|none|
|» draft|[Draft](#schemadraft)|true|none|none|

<h2 id="tocS_Participant">Participant</h2>
<!-- backwards compatibility -->
<a id="schemaparticipant"></a>
<a id="schema_Participant"></a>
<a id="tocSparticipant"></a>
<a id="tocsparticipant"></a>

```json
{
  "uuid": "095be615-a8ad-4c33-8e9c-c7612fbf6c9f",
  "username": "string",
  "won": true,
  "score": 0,
  "eloBefore": 0,
  "eloChange": 0,
  "eloAfter": 0,
  "rd": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|uuid|string(uuid)|true|none|none|
|username|string|true|none|none|
|won|boolean|true|none|none|
|score|integer|true|none|none|
|eloBefore|integer,null|true|none|none|
|eloChange|integer,null|true|none|none|
|eloAfter|integer,null|true|none|none|
|rd|number,null|true|none|Rating deviation after the match. `null` for unranked participants or Elo eras.|

<h2 id="tocS_Goal">Goal</h2>
<!-- backwards compatibility -->
<a id="schemagoal"></a>
<a id="schema_Goal"></a>
<a id="tocSgoal"></a>
<a id="tocsgoal"></a>

```json
{
  "index": 0,
  "id": "OBTAIN_64_COLORED_CONCRETE",
  "data": "string",
  "completed": true,
  "completedByUuid": "7ca0775d-ca03-4d72-a053-72fd261cf2bf",
  "completedAtMs": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|index|integer|true|none|none|
|id|string|true|none|none|
|data|string,null|true|none|Goal-specific data, such as a color name.|
|completed|boolean|true|none|none|
|completedByUuid|string,null(uuid)|true|none|none|
|completedAtMs|integer,null|true|none|Milliseconds after match start when the goal was completed.|

<h2 id="tocS_Draft">Draft</h2>
<!-- backwards compatibility -->
<a id="schemadraft"></a>
<a id="schema_Draft"></a>
<a id="tocSdraft"></a>
<a id="tocsdraft"></a>

```json
{
  "pickedFirstUuid": "bca05434-a5ae-411f-af81-33c4e3a191c0",
  "pool": [
    {
      "id": "string",
      "data": "string",
      "picked": true,
      "timedOut": true,
      "rerolled": true
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|pickedFirstUuid|string,null(uuid)|true|none|none|
|pool|[[DraftPoolItem](#schemadraftpoolitem)]|true|none|none|

<h2 id="tocS_DraftPoolItem">DraftPoolItem</h2>
<!-- backwards compatibility -->
<a id="schemadraftpoolitem"></a>
<a id="schema_DraftPoolItem"></a>
<a id="tocSdraftpoolitem"></a>
<a id="tocsdraftpoolitem"></a>

```json
{
  "id": "string",
  "data": "string",
  "picked": true,
  "timedOut": true,
  "rerolled": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|data|string,null|true|none|none|
|picked|boolean|true|none|none|
|timedOut|boolean|true|none|none|
|rerolled|boolean|true|none|Whether this goal was replaced during the draft.|

<h2 id="tocS_EloPoint">EloPoint</h2>
<!-- backwards compatibility -->
<a id="schemaelopoint"></a>
<a id="schema_EloPoint"></a>
<a id="tocSelopoint"></a>
<a id="tocselopoint"></a>

```json
{
  "matchId": 0,
  "completedAt": 0,
  "eloBefore": 0,
  "eloAfter": 0,
  "eloChange": 0,
  "outcome": "finished",
  "won": true,
  "opponentUuid": "63613e40-8327-45d8-8c9c-6a784ccc7ba2",
  "opponentName": "string",
  "playerScore": 0,
  "opponentScore": 0,
  "durationMs": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|matchId|integer|true|none|none|
|completedAt|integer(int64)|true|none|Unix timestamp in milliseconds.|
|eloBefore|integer|true|none|none|
|eloAfter|integer|true|none|none|
|eloChange|integer|true|none|none|
|outcome|[MatchOutcome](#schemamatchoutcome)|true|none|Match result:<br><br>- `finished`: completed normally.<br>- `forfeited`: ended by a forfeit.<br>- `draw_by_vote`: players voted to end the match as a draw.<br>- `draw`: recorded as a draw.<br>- `cancelled`: cancelled without a result.|
|won|boolean|true|none|none|
|opponentUuid|string(uuid)|true|none|none|
|opponentName|string|true|none|none|
|playerScore|integer|true|none|none|
|opponentScore|integer|true|none|none|
|durationMs|integer|true|none|none|

<h2 id="tocS_MatchType">MatchType</h2>
<!-- backwards compatibility -->
<a id="schemamatchtype"></a>
<a id="schema_MatchType"></a>
<a id="tocSmatchtype"></a>
<a id="tocsmatchtype"></a>

```json
"competitive"

```

Queue in which the match was played:

- `competitive`: ranked competitive queue.
- `quick_play`: unranked quick-play queue.
- `lobby`: custom lobby.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Queue in which the match was played:<br><br>- `competitive`: ranked competitive queue.<br>- `quick_play`: unranked quick-play queue.<br>- `lobby`: custom lobby.|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|competitive|
|*anonymous*|quick_play|
|*anonymous*|lobby|

<h2 id="tocS_MatchOutcome">MatchOutcome</h2>
<!-- backwards compatibility -->
<a id="schemamatchoutcome"></a>
<a id="schema_MatchOutcome"></a>
<a id="tocSmatchoutcome"></a>
<a id="tocsmatchoutcome"></a>

```json
"finished"

```

Match result:

- `finished`: completed normally.
- `forfeited`: ended by a forfeit.
- `draw_by_vote`: players voted to end the match as a draw.
- `draw`: recorded as a draw.
- `cancelled`: cancelled without a result.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Match result:<br><br>- `finished`: completed normally.<br>- `forfeited`: ended by a forfeit.<br>- `draw_by_vote`: players voted to end the match as a draw.<br>- `draw`: recorded as a draw.<br>- `cancelled`: cancelled without a result.|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|finished|
|*anonymous*|forfeited|
|*anonymous*|draw_by_vote|
|*anonymous*|draw|
|*anonymous*|cancelled|

<h2 id="tocS_GameMode">GameMode</h2>
<!-- backwards compatibility -->
<a id="schemagamemode"></a>
<a id="schema_GameMode"></a>
<a id="tocSgamemode"></a>
<a id="tocsgamemode"></a>

```json
"draftout"

```

Board rules used by the match:

- `draftout`: players draft the goals on the shared board.
- `lockout`: players race on a randomly generated shared board.
- `blackout`: players cooperate on one shared board.

Competitive and quick-play matches use `draftout`; lobby matches may
use any of the three modes.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Board rules used by the match:<br><br>- `draftout`: players draft the goals on the shared board.<br>- `lockout`: players race on a randomly generated shared board.<br>- `blackout`: players cooperate on one shared board.<br><br>Competitive and quick-play matches use `draftout`; lobby matches may<br>use any of the three modes.|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|draftout|
|*anonymous*|lockout|
|*anonymous*|blackout|

