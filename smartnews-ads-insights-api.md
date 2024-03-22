# SmartNews Ads Insights API - (apidoc v0.2 20230301)

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

The following fields are available with the fields parameter and can be requested with the fields parameter. Please note that metrics such as `videoP25Views`, `videoP50Views`, `videoP75Views`, `videoP95Views`, `videoCompleteViews`, `videoCompleteViewRate`, `videoAvgViewTime`, `videoAvgViewRate` will continue to update up to 48 hours after a video ad is served.

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

| Name                    | Type   | Format | Description                                                                                    |
|-------------------------|--------|--------|------------------------------------------------------------------------------------------------|
| impressions             | number | int    | The number of times your ad was served.                                                        |
| viewableImpressions     | number | int    | The number of times your ad was viewable.                                                      |
| clicks                  | number | int    | The total number of people who have clicked on your ad.                                        |
| conversions             | number | int    | The number of conversions taken on your ad.                                                    |
| spend                   | number | float  | The total amount you've spent so far.                                                          |
| cpm                     | number | float  | The average cost you've paid to have 1,000 impressions on your ad.                             |
| cpc                     | number | float  | The price you've paid divided by the number of clicks.                                         |
| ctr                     | number | float  | The number of clicks you received divided by the number of impressions.                        |
| vctr                    | number | float  | The number of clicks you received divided by the number of viewable impressions.               |
| cvr                     | number | float  | The number of conversions you received divided by the number of clicks.                        |
| cpa                     | number | float  | The cost you've paid divided by the number of conversions.                                     |
| videoViews              | number | int    | The number of times your video starts to play (100% in view).                                  |
| videoP25Views           | number | int    | The number of times your video was played at 25 % of its length.                               |
| videoP50Views           | number | int    | The number of times your video was played at 50 % of its length.                               |
| videoP75Views           | number | int    | The number of times your video was played at 75 % of its length.                               |
| videoP95Views           | number | int    | The number of times your video was played at 95 % of its length.                               |
| videoCompleteViews      | number | int    | The number of times your video was played through their entire duration to completion.         |
| videoCompleteViewRate   | number | float  | The average percentage that your video was played through their entire duration to completion. |
| videoAvgViewTime        | number | float  | The average time a video was played.                                                           |
| videoAvgViewRate        | number | float  | The average percentage that the video was played.                                              |
| addToCart               | number | int    | The number of addToCart conversions taken on your ad.                                          |
| purchase                | number | int    | The number of purchase conversions taken on your ad.                                           |
| subscribe               | number | int    | The number of subscribe conversions taken on your ad.                                          |
| completeRegistration    | number | int    | The number of completeRegistration conversions taken on your ad.                               |
| viewContent             | number | int    | The number of viewContent conversions taken on your ad.                                        |
| addToCartCpa            | number | float  | The cost you've paid divided by the number of addToCart conversions.                           |
| addToCartCvr            | number | float  | The number of addToCart conversions you received divided by the number of clicks.              |
| purchaseCpa             | number | float  | The cost you've paid divided by the number of purchase conversions.                            |
| purchaseCvr             | number | float  | The number of purchase conversions you received divided by the number of clicks.               |
| subscribeCpa            | number | float  | The cost you've paid divided by the number of subscribe conversions.                           |
| subscribeCvr            | number | float  | The number of subscribe conversions you received divided by the number of clicks.              |
| completeRegistrationCpa | number | float  | The cost you've paid divided by the number of completeRegistration conversions.                |
| completeRegistrationCvr | number | float  | The number of completeRegistration conversions you received divided by the number of clicks.   |
| viewContentCpa          | number | float  | The cost you've paid divided by the number of viewContent conversions.                         |
| viewContentCvr          | number | float  | The number of viewContent conversions you received divided by the number of clicks.            |

### Fields of others

| Name                        | Type             | Format       | Description                                                          |
|-----------------------------|------------------|--------------|----------------------------------------------------------------------|
| breakdowns                  | array            | Breakdown    | delivery breakdown information if you request `breakdowns` parameter |
| conversionOptimizationPoint | string           |              | The conversion optimization point of your campaign.                  |

#### In future release

These metrics are supported by future version.

| Name            | Type             | Format       | Description                                |
|-----------------|------------------|--------------|--------------------------------------------|
| frequency       | number           | float        | The average number of times your ad was served to each person. |
| reach           | number           | int          | The number of people your ad was served to. |

# AMv2 support (Scheduled to release at TBD)

> Please be familiarized with the [general information of Ads Manager V2 support](./README.md#ads-manager-v2-amv2-support) first.

## AMv2 Object
For users to distinguish AMv2 data from AMv1 data, the `amV2` object is added to the first level of each element of the data array.

### `amV2` object

| Name         | Type   | Format | Description                       |
|--------------|--------|--------|-----------------------------------|
| campaignId   | string |        | The identifier of the 3L campaign |
| adGroupId    | string |        | The identifier of the 3L ad group |
| campaignName | string |        | The name of the 3L campaign       |
| adGroupName  | string |        | The name of the 3L ad group       |

## Unsupported parameters and response fields of AMv2 Data
Because of the difference in the product specification of AMv1 and AMv2, the following request parameters and response fields will not available for AMv2 Data.

### Unsupported request parameters
Providing any of the below unsupported parameters doesn't result in a bad request, however, they have no effect on the AMv2 data.

| Name       | Value | Description                                      |
|------------|-------|--------------------------------------------------|
| breakdowns | *ALL* | AMv2 data do not have any breakdown insight      |
| format     | `csv` | The resulting CSV does not contain any AMv2 data |

### Unsupported response fields

| Name                   | Description                               |
|------------------------|-------------------------------------------|
| impressions            | this field is always `0` for AMv2 data    |
| conversions            | this field is always `0` for AMv2 data    |
| ctr                    | this field is always `0` for AMv2 data    |
| cvr                    | this field is always `0` for AMv2 data    |
| cpa                    | this field is always `0` for AMv2 data    |
| videoP100Views         | this field is always `null` for AMv2 data | 
| videoAvgViewRate       | this field is always `null` for AMv2 data |
| videoAvgViewTime       | this field is always `null` for AMv2 data |
| videoLength            | this field is always `null` for AMv2 data |
| skAdNetworkConversions | this field is always `null` for AMv2 Data |
| accountName            | this field is always `null` for AMv2 Data |

### Example
```json
{
  "data": [
    // in case of a AMv2 object
    {
      "amV2": {
        "campaignId": "10000001",
        "adGroupId": "10000002",
        "campaignName": "AMv2 Campaign",
        "adGroupName": "AMv2 Ad Group"
      },
      "accountId": "10000000",
      "campaignId": "10000002", // AMv2 Ad Group ID
      "clicks": 10,
      // ...
      // the following fields are not supported for AMv2 object
      "impressions": 0,
      "conversions": 0,
      "ctr": 0,
      "cvr": 0,
      "cpa": 0,
      "videoP100Views": 0,
      "skAdNetworkConversions": null,
      // AMv2 data doesn't support any breakdown 
      "insights": []
    },
    // in case of a AMv1 object
    {
      "amV2": null, // set to null
      "accountId": "10000000",
      "campaignId": "11000003",
      "impressions": 100,
      "clicks": 10,
      "ctr": 0.1,
      //...
      "insights": [
        {
          // amV2 is also included in the breakdown insight, but it is always null 
          "amV2": null, 
          "breakdowns": [{"type": "publisher", "value": "smartnews"}],
          //...
        }
      ]
    }
  ]
}
```

### Account insights
Account insights returns insights data separately for AMv1 and AMv2. As a general rule, the insights data for AMv1 is placed first, but if there are no available insights data for AMv1, AMv2 data may be returned first.
Additionally, in account insights, properties in `amV2` are all null. Please note that whether the `amV2` object itself is null or not indicates that it is data from AMv2.

Account insights response example
```json
{
  "meta": {
    "updatedAt": "2024-03-18T09:00:00Z"
  },
  "data": [
    {
      "accountId": "10000000",
      "accountName": "Advertiser Name",
      "campaignId": null,
      "campaignName": null,
      "creativeId": null,
      "creativeName": null,
      "conversionOptimizationPoint": null,
      "breakdowns": null,
      "impressions": 3075,
      "viewableImpressions": 534,
      "clicks": 353,
      "conversions": 98,
      "spend": 6.0,
      "cpm": 11.235955056179774,
      "cpc": 0.0169971671388102,
      "ctr": 11.479674796747968,
      "vctr": 66.10486891385767,
      "cvr": 27.762039660056658,
      "cpa": 0.061224489795918366,
      "videoP25Views": null,
      "videoP50Views": null,
      "videoP75Views": null,
      "videoP95Views": null,
      "videoP100Views": null,
      "videoLength": null,
      "videoViews": null,
      "videoViewableViews": null,
      "videoCompleteViews": null,
      "videoCompleteViewRate": null,
      "videoAvgViewTime": null,
      "videoAvgViewRate": null,
      "skAdNetworkConversions": null,
      "addToCart": null,
      "purchase": null,
      "subscribe": null,
      "completeRegistration": null,
      "viewContent": null,
      "addToCartCpa": null,
      "addToCartCvr": null,
      "purchaseCpa": null,
      "purchaseCvr": null,
      "subscribeCpa": null,
      "subscribeCvr": null,
      "completeRegistrationCpa": null,
      "completeRegistrationCvr": null,
      "viewContentCpa": null,
      "viewContentCvr": null,
      "insights": null,
      "amV2": null
    },
    {
      "accountId": "10000000",
      "accountName": null,
      "campaignId": null,
      "campaignName": null,
      "creativeId": null,
      "creativeName": null,
      "conversionOptimizationPoint": null,
      "breakdowns": null,
      "impressions": 0,
      "viewableImpressions": 63,
      "clicks": 1,
      "conversions": 0,
      "spend": 104.160708,
      "cpm": 1653.344571428571,
      "cpc": 104.160708,
      "ctr": 0.0,
      "vctr": 0.01587301587301587,
      "cvr": 0.0,
      "cpa": 0.0,
      "videoP25Views": null,
      "videoP50Views": null,
      "videoP75Views": null,
      "videoP95Views": null,
      "videoP100Views": null,
      "videoLength": null,
      "videoViews": null,
      "videoViewableViews": null,
      "videoCompleteViews": null,
      "videoCompleteViewRate": null,
      "videoAvgViewTime": null,
      "videoAvgViewRate": null,
      "skAdNetworkConversions": null,
      "addToCart": null,
      "purchase": null,
      "subscribe": null,
      "completeRegistration": null,
      "viewContent": null,
      "addToCartCpa": null,
      "addToCartCvr": null,
      "purchaseCpa": null,
      "purchaseCvr": null,
      "subscribeCpa": null,
      "subscribeCvr": null,
      "completeRegistrationCpa": null,
      "completeRegistrationCvr": null,
      "viewContentCpa": null,
      "viewContentCvr": null,
      "insights": null,
      "amV2": {
        "campaignId": null,
        "campaignName": null,
        "adGroupId": null,
        "adGroupName": null
      }
    }
  ]
}
```

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
