# SmartNews Ads Insights API - (apidoc v0.2 20230227)

We provide a family of methods for retrieving statistics on the performance of your campaigns.

## Endpoints

This endpoint is similar to the Ad Insights API. It exists as an edge mixed into any Ad Object.

- GET /accounts/{accountId}/insights
- GET /campaigns/{campaignId}/insights
- GET /creatives/{creativeId}/insights

## Response Structure

Successful responses are indicated with a 200-series HTTP code and JSON-based payload.

The format of the `data` node will be returned as JSON array when the response may contain one or more results.

```
{
  "data": [
    {
      "accountId": "10000000",
      "campaignId": "10000001",
      "impressions": 100,
      "clicks": 10,
      "ctr": 0.1
      ...

      /* it will contain `insights` object if you request with breakdown parameter */
      "insights": [
        {
          "breakdowns": [{"type": "publisher", "value": "smartnews"}],
          "accountId": "10000000",
          "campaignId": "10000001",
          "impressions": 100,
          "clicks": 10,
          "ctr": 0.1
          ...
        },
        {
          "breakdowns": [{"type": "publisher", "value": "mixi"}],
          "accountId": "10000000",
          "campaignId": "10000001",
          "impressions": 100,
          "clicks": 10,
          "ctr": 0.1
          ...
        }
      ]
    }
  ]
}
```

Error response structure is common with *SmartNews Ads Management API*. Please see that.

## Parameters

You can use the following endpoint parameters to retrieve report stats.

| Name           | Type             | Format                         | Description                                                              |
|----------------|------------------|--------------------------------|--------------------------------------------------------------------------|
| since          | string           | yyyy-MM-dd                     | `2015-01-01` refers to time Jan 1 00:00, use in conjunction with `until`, `since` can be any date between today and 1098 days ago from today. |
| until          | string           | yyyy-MM-dd                     | `2015-01-01` refers to time Jan 2 00:00, use in conjunction with `since`, `until` can be any date between `since` and 92 days after from `since`. |
| date_preset    | string           | DatePreset                     | it is ignored if since and until are specified                           |
| breakdowns     | string           | BreakdownType with comma separated | it can separate several breakdowns with comma(,)                         |
| level          | string           | Level                          | Represents the level of result.                                          |
| fields_presets | string           | FieldsPresets                  | Specify what additional metrics to get.                                  |

### In future release

This parameters are not supported by first release. Please waiting until future release.

| Name          | Type             | Format             | Description                                |
|---------------|------------------|--------------------|--------------------------------------------|
| filtering     | Filter           | Filter             |                                            |
| fields        | list of string   |                    |                                            |
| sort          | string           | {field}\_{desc\|asc} |                                            |
| limit         | number           | int                | The number of individual data blocks returned. This is a upper limit. Default value is 50. |

## Fields

The following fields are available with the fields parameter and can be requested with the fields parameter.

### Fields of identifier

| Name          | Type             | Format       | Description                                |
|---------------|------------------|--------------|--------------------------------------------|
| accountId     | string           |              | The identifier for a advertiser account |
| campaignId    | string           |              | The identifier for a campaign |
| creativeId    | string           |              | The identifier for a creative |

### Fields of object name

| Name          | Type             | Format       | Description                                |
|---------------|------------------|--------------|--------------------------------------------|
| accountName   | string           |              | The name for advertiser account |
| campaignName  | string           |              | The name for campaign |
| creativeName  | string           |              | The name for creative |

### Fields of insights metrics

| Name                  | Type   | Format | Description                                                                                                |
|-----------------------|--------|--------|------------------------------------------------------------------------------------------------------------|
| impressions           | number | int    | The number of times your ad was served.                                                                    |
| viewableImpressions   | number | int    | The number of times your ad was viewable.                                                                  |
| clicks                | number | int    | The total number of people who have clicked on your ad.                                                    |
| conversions           | number | int    | The number of conversions taken on your ad.                                                                |
| spend                 | number | float  | The total amount you've spent so far.                                                                      |
| cpm                   | number | float  | The average cost you've paid to have 1,000 impressions on your ad.                                         |
| cpc                   | number | float  | The price you've paid divided by the number of clicks.                                                     |
| ctr                   | number | float  | The number of clicks you received divided by the number of impressions.                                    |
| vctr                  | number | float  | The number of clicks you received divided by the number of viewable impressions.                           |
| cvr                   | number | float  | The number of clicks you received divided by the number of conversions.                                    |
| cpa                   | number | float  | The cost you've paid divided by the number of conversions.                                                 |
| videoViews            | number | int    | The number of times your video starts to play (100% in view).                                              |
| videoP25Views         | number | int    | The number of times your video was played at 25 % of its length.                                           |
| videoP50Views         | number | int    | The number of times your video was played at 50 % of its length.                                           |
| videoP75Views         | number | int    | The number of times your video was played at 75 % of its length.                                           |
| videoP95Views         | number | int    | The number of times your video was played at 95 % of its length.                                           |
| videoCompleteViews    | number | int    | The number of times your video was played through their entire duration to completion.                     |
| videoCompleteViewRate | number | float  | The average percentage that your video was played through their entire duration to completion.             |
| videoAvgViewTime      | number | float  | The average time a video was played, including any time spent replaying the video for a single impression. |
| videoAvgViewRate      | number | float  | The average percentage that the video was played.                                                          |

### Fields of others

| Name            | Type             | Format       | Description                                |
|-----------------|------------------|--------------|--------------------------------------------|
| breakdowns      | array            | Breakdown    | delivery breakdown information if you request `breakdowns` parameter |

#### In future release

These metrics are supported by future version.

| Name            | Type             | Format       | Description                                |
|-----------------|------------------|--------------|--------------------------------------------|
| frequency       | number           | float        | The average number of times your ad was served to each person. |
| reach           | number           | int          | The number of people your ad was served to. |

# Appendix

## Reference

### Breakdown

| Name            | Type             | Format        | Description                                |
|-----------------|------------------|---------------|--------------------------------------------|
| type            | string           | BreakdownType | breakdown type value |
| value           | string           |               | breakdown value |

### DatePreset

| DatePreset      | Description            |
|-----------------|------------------------|
| today           | today in JST           |
| yesterday       | yesterday in JST       |
| last_7_days     | last 7 days in JST     |
| last_30_days    | last 30 days in JST    |
| this_month      | this month in JST      |
| last_month      | last month in JST      |
| last_3_months   | last 3 months in JST   |

### BreakdownType

| BreakdownType   | Description                                                             |
|-----------------|-------------------------------------------------------------------------|
| publisher       | media delivery breakdown (publisher data like this: `smartnews`, `mixi`)     |
| platform        | platform delivery breakdown (platform data like this: `ios`, `android`, `web`)         |
| genre           | genre delivery breakdown (genre like this: `general`, `food`, `sports`)        |

### Level

| Level           | Description              |
|-----------------|--------------------------|
| account         |                          |
| campaign        |                          |
| creative        |                          |


### FieldsPresets

| FieldsPresets   | Description              |
|-----------------|--------------------------|
| video           | Get the video additional metrics, such as view completion rate, for video ads. If the value does not exist, the field will not be added. |
| skadnetwork     | Get the skadnetwork additional metrics, such ad skadnetwork_conversion, for skadnetwork measurement ads. If the value does not exist, the field will not be added. |
