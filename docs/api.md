<!-- Generator: Widdershins v4.0.1 -->

<h1 id="draftout-community-api">Draftout Community API v0.1.0</h1>

> Scroll down for code samples, example requests and responses. Select a language for code samples from the tabs above or the mobile navigation menu.

Community-maintained OpenAPI description for the public Draftout stats
endpoints observed at draftoutmc.com.

This is not an official Draftout API specification. It was inferred from
live responses on 2026-05-26 and may be incomplete.

Base URLs:

* <a href="https://draftoutmc.com">https://draftoutmc.com</a>

<h1 id="draftout-community-api-stats">Stats</h1>

Player stats, match details, and Elo history.

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

#### Enumerated Values

|Parameter|Value|
|---|---|
|filter|competitive|
|filter|quick-play|

> Example responses

> Player stats page.

```json
{
  "player": {
    "uuid": "9a8e24df-4c85-49d6-96a6-951da84fa5c4",
    "username": "Feinberg",
    "elo": 1705,
    "ranked": true,
    "rank": 1,
    "rankName": "Guardian I",
    "rankColor": "#45686e"
  },
  "record": {
    "matches": 43,
    "completedMatches": 39,
    "wins": 40,
    "losses": 2,
    "draws": 1,
    "winRate": 0.9523809523809523,
    "averageFinishTime": 1487023.7837837837,
    "averageGoals": 6.230769230769231
  },
  "aggregate": {
    "peakElo": 1705,
    "bestStreak": 18,
    "fastestWinMs": 693452,
    "forfeitCount": 0
  },
  "matches": [
    {
      "id": 20010,
      "matchType": "competitive",
      "outcome": "finished",
      "completedAt": 1779754939814,
      "durationMs": 2316432,
      "participants": [
        {
          "uuid": "9a8e24df-4c85-49d6-96a6-951da84fa5c4",
          "username": "Feinberg",
          "won": true,
          "score": 13,
          "eloBefore": 1692,
          "eloChange": 13,
          "eloAfter": 1705
        }
      ]
    }
  ],
  "page": 1,
  "totalPages": 3,
  "filter": "competitive"
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
    "ranked": true,
    "rank": 1,
    "rankName": "Guardian I",
    "rankColor": "#45686e"
  },
  "match": {
    "id": 0,
    "matchType": "competitive",
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
        "eloAfter": 0
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
          "timedOut": true
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
  ]
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

Elo rank bands.

## getRanks

<a id="opIdgetRanks"></a>

> Code samples

```shell
# You can also use wget
curl -X GET https://draftoutmc.com/api/ranks \
  -H 'Accept: application/json'

```

`GET /api/ranks`

*Get Elo rank bands*

Returns the configured Draftout Elo rank bands in ascending Elo order.
The lowest and highest bands are open-ended and use `null` for the
missing bound.

> Example responses

> Rank bands.

```json
[
  {
    "name": "Coal",
    "min": null,
    "max": 499,
    "color": "#424646"
  },
  {
    "name": "Guardian I",
    "min": 1700,
    "max": 1799,
    "color": "#45686e"
  },
  {
    "name": "Warden",
    "min": 2000,
    "max": null,
    "color": "#2b4450"
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
|» min|integer,null|true|none|Minimum Elo for the band. `null` means no lower bound.|
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

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|competitive|
|*anonymous*|quick-play|

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
|min|integer,null|true|none|Minimum Elo for the band. `null` means no lower bound.|
|max|integer,null|true|none|Maximum Elo for the band. `null` means no upper bound.|
|color|string|true|none|none|

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
    "ranked": true,
    "rank": 1,
    "rankName": "Guardian I",
    "rankColor": "#45686e"
  },
  "record": {
    "matches": 0,
    "completedMatches": 0,
    "wins": 0,
    "losses": 0,
    "draws": 0,
    "winRate": 0,
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
          "eloAfter": 0
        }
      ]
    }
  ],
  "page": 1,
  "totalPages": 1,
  "filter": "competitive"
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
|filter|[MatchFilter](#schemamatchfilter)|true|none|none|

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
    "ranked": true,
    "rank": 1,
    "rankName": "Guardian I",
    "rankColor": "#45686e"
  },
  "match": {
    "id": 0,
    "matchType": "competitive",
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
        "eloAfter": 0
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
          "timedOut": true
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
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|startElo|integer|true|none|none|
|points|[[EloPoint](#schemaelopoint)]|true|none|none|

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
  "ranked": true,
  "rank": 1,
  "rankName": "Guardian I",
  "rankColor": "#45686e"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|uuid|string(uuid)|true|none|none|
|username|string|true|none|none|
|elo|integer|true|none|none|
|ranked|boolean|true|none|none|
|rank|integer,null|true|none|Leaderboard rank. Observed as `null` for quick-play responses.|
|rankName|string|true|none|none|
|rankColor|string|true|none|none|

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
  "winRate": 0,
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
|averageGoals|number,null|true|none|none|

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
      "eloAfter": 0
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|integer|true|none|none|
|matchType|[MatchType](#schemamatchtype)|true|none|none|
|outcome|[MatchOutcome](#schemamatchoutcome)|true|none|Match outcome values observed in live responses.|
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
      "eloAfter": 0
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
        "timedOut": true
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
  "eloAfter": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|uuid|string(uuid)|true|none|none|
|username|string|true|none|none|
|won|boolean|true|none|none|
|score|integer|true|none|none|
|eloBefore|integer|true|none|none|
|eloChange|integer|true|none|none|
|eloAfter|integer|true|none|none|

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
      "timedOut": true
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
  "timedOut": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|data|string,null|true|none|none|
|picked|boolean|true|none|none|
|timedOut|boolean|true|none|none|

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
|outcome|[MatchOutcome](#schemamatchoutcome)|true|none|Match outcome values observed in live responses.|
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

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|competitive|
|*anonymous*|quick-play|

<h2 id="tocS_MatchOutcome">MatchOutcome</h2>
<!-- backwards compatibility -->
<a id="schemamatchoutcome"></a>
<a id="schema_MatchOutcome"></a>
<a id="tocSmatchoutcome"></a>
<a id="tocsmatchoutcome"></a>

```json
"finished"

```

Match outcome values observed in live responses.

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|Match outcome values observed in live responses.|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|finished|
|*anonymous*|forfeited|
|*anonymous*|draw_by_vote|

