# SmartNews Ads Management API - (apidoc v0.8 20221017)

The SmartNews Ads Advertising API allows partners integrate with the SmartNews advertising platform in their own advertising solutions.

## Overview

### Getting Started

SmartNews Ads Advertising API provides programmatic access to advertising accounts. Partners can integrate their solutions with the API to promote SmartNews Ads advertising accounts, schedule campaigns, manage audiences, and more.

#### Using the API

The Advertising API accessed on https://ads.smartnews.com/api. The Advertising API enforces HTTPS, Attempts to access an endpoint with HTTP will result in a redirect response.

The Advertising API outputs JSON. All identifiers are strings and all strings are UTF-8. The Advertising API is versioned and the version is specified as the first path element of any resource URL (such as `/v1.0` or `/v2.0`).

#### Authentication

API authentication is achieved via a bearer token which identifies a single user. You can use a generated API key. You pass the API key into the `X-Auth-Api` http header like this.

```
curl -H 'X-Auth-Api: <YOUR_API_KEY>' 'https://ads.smartnews.com/api'
```

> :warning: **We have migrated our domain from `partners.smartnews-ads.com` to `ads.smartnews.com`. Please be aware that access to the old domain will return a 301 redirect response after Nov 15th, 2022.**

#### Time

DateTime values are always returned in UTC time. DateTimes may be specified in any timezone in a POST command using the ISO 8601 standard format for timezone. Time is represented using a subset of ISO-8601.

#### Currency

The type of a currency is identified using ISO-4217. This is a three-letter string like a `JPY`.
Currently, it is supported JPY only.

## Basics

SmartNews Ads Management API Basics provides you with some fundamental information around how todo some basic things. It has all the information you may need with regards to doing things about errors.

### Error Codes & Responses

#### Typical Response Structure

Successful responses are indicated with a 200-series HTTP code and JSON-based payload containing the object(s) requested, created, modified, or deleted along with an expression of the server's interpretation of your request.

The data field in JSON responses will contain the specific objects associated with the leveraged resource. The format of the `data` node will be returned as JSON array when the response may contain one or more results.

```
{
  "data": {
    /* contents payload... */
  }
}
```

#### Error Response Structure

Error Responses are served with a non-200-series HTTP code. Usually a JSON response will be attached, but some errors will respond with different kinds of body. For instance, you may occasionally see a HTTP 404 along with a HTML response. In this case, it's safe to assume that the content cannot be found.

The nature of the error will be communicated in an `error` node of the response. The `error/message` node will indicate a human-readable description of the error in English. Additional fields may be attached to indicate finer-grained detail about the error.

```
{
  "error": {
    "code": 1,
    "message": "Bid Too Low: Your bid is below the minimum for its placement.",
    "parameter": "bidAmount"
  }
}
```

#### Error Codes

**Work In Progress**

### Pagination & Sorting

Pagination & Sorting is currently not supported. It's future works.

### Timezones

#### Timezone in the Ads Management API

Timezone is currently not supported. It's future works.

## Campaign Management

Programmatically schedule campaigns and manage ads on SmartNews through this suite of APIs.

### API Use Case : Call Sequence from Creating a Campaign until Delivering it

1. Retrive the account id - `GET /v1.0/accounts` and `GET /v1.0/accounts/{accountId}/campaigns`
2. Create a campaign and associate it(account) - `POST /v1.0/accounts/{accountId}/campaigns`
3. Upload image for creative and Create some creatives and associate it(campaign) - `POST /v1.0/accounts/{accountId}/images/upload` and `POST /v1.0/campaigns/{campaignId}/creatives`
4. Request review campaign and associated creatives - `POST /v1.0/campaigns/{campaignId}/submit_review`
5. Enable campaign and creatives after approved - `POST /v1.0/campaigns/{campaignId}/update_enable` and `POST /v1.0/creatives/{creativeId}/update_enable`
6. Update campaign's item like a `budget` - `POST /v1.0/campaigns/{campaignId}/update`

### Campaigns

Campaigns define the schedule and budget of an ad. The advertiser specifies a daily and overall budget. The campaign can be bound to a specific start and end time, and until the budget is spent. Campaign identifiers area the base-36 representation of the base-10 value we present in the SmartNews Ads Partners UI.

#### GET /v1.0/accounts

Retrieve all of the advertising-enabled accounts the authenticating user has access to.

##### Example Result

```
{
  "data": [
    {
      "accountId": "1000000",
      "name": "SmartNews, Inc"
    },
    {
      "accountId": "1000001",
      "name": "Some company name"
    }
  ]
}
```

#### GET /v1.0/accounts/{accountId}/campaigns

Retrieve some or all campaigns associated with the current account.
Note: Campaigns that has Dynamic Creative ON can not be retrieved through this API.

##### Parameters

| Name          | Type     | Description                                |
|---------------|----------|--------------------------------------------|
| accountId     | string   | **required** The identifier for a advertiser account associated with the authenticating user. |

##### Example Result

```
{
  "data": [
    {
      "name": "SmartNews Installs 7/12",
      "actionType": "APP_INSTALL",
      "campaignId": "1000003",
      "accountId": "1000000",
      "approvalStatus": "PENDING",
      "enable": false,
      "startTime": "2015-07-01T13:40:00Z",
      "endTime": "2015-07-29T13:40:00Z",
      "totalBudget": 20000,
      "dailyBudget": 5000,
      "bidAmount": 30,
      "updatedAt": "2015-06-25T13:40:00Z"
    },
    {
      "name": "SmartNews Installs 8/12",
      "actionType": "APP_INSTALL",
      "campaignId": "1000004",
      "accountId": "1000000",
      "approvalStatus": "PENDING",
      "enable": false,
      "startTime": "2015-08-01T13:40:00Z",
      "endTime": "2015-08-29T13:40:00Z",
      "totalBudget": 20000,
      "dailyBudget": 5000,
      "bidAmount": 30,
      "updatedAt": "2015-06-25T13:40:00Z"
    }
  ]
}
```

#### GET /v1.0/campaigns/{campaignId}

Retrieve details for a specific campaign.
Note: Campaigns that has Dynamic Creative ON can not be retrieved through this API.

##### Parameters

| Name          | Type             | Description                                   |
|---------------|------------------|-----------------------------------------------|
| campaignId    | string           | **required** The identifier for a campaign.   |

##### Example Result

```
{
  "data": {
    "actionType": "APP_INSTALL",
    "name": "SmartNews Installs 7/12",
    "campaignId": "1000005",
    "accountId": "1000000",
    "enable": false,
    "startTime": "2015-07-01T13:40:00Z",
    "endTime": "2015-07-29T13:40:00Z",
    "totalBudget": 20000,
    "dailyBudget": 5000,
    "bidAmount": 30,
    "sponsoredName": "SmartAds",
    "targetCpa": 750,
    "adCategoryId": 3,
    "appSpec": {
      "application": "579581125",
      "urlscheme": "jp.gocro.smartnews://"
    },
    "trackingSpec": {
      "trackingType": "SAT"
    },
    "targeting": {
      "publishers": ["120", "121", "99810"],
      "devices": ["IPHONE", "IPOD", "IPAD"],
      "genders": ["FEMALE"],
      "cities": [
        {
          "key": "13000"
        }
      ],
      "schedules": [
        {
          "startSecond": 0,
          "endSecond": 14400
        }
      ]
    },
    "approvalStatus": "PENDING",
    "updatedAt": "2015-07-01T13:40:00Z"
  }
}
```

#### POST /v1.0/accounts/{accountId}/campaigns

Create a new campaign associated with the current account.

##### Parameters [in request path]

| Name          | Type     | Format       | Description                                |
|---------------|----------|--------------|--------------------------------------------|
| accountId     | string   |              | **required** The identifier for a advertiser account associated with the authenticating user. |

##### Parameters [in request payload]

| Name          | Type             | Format       | Description                                |
|---------------|------------------|--------------|--------------------------------------------|
| actionType    | string           | ActionType   | **required** The objective type for campaign. |
| name          | string           |              | **required** The name for campaign.        |
| sponsoredName | string           |              | **required** The sponsor name for this campaign. It is used by advertising display. |
| bidAmount        | number        | long         | **required in future** The bid amount for this campaign. |
| targetCpa        | number        | long         | **optional** The target CPA(Cost Per Acquisition) amount for this campaign. |
| totalBudget      | number        | long         | **required in future** The total budget amount to be allocated to the campaign. |
| dailyBudget      | number        | long         | **required in future** The daily budget amount to be allocated to the campaign. |
| startTime     | string           | ISO 8601     | **required** The UTC time that campaign will begin. |
| endTime       | string           | ISO 8601     | **required** The UTC time that campaign will end.   |
| adCategoryId  | string           | int          | **required** The ad_category for campaign. You get available ad_categories from `GET /adcategories` |
| appSpec       | object           | AppSpec      | **optional** The application information. (it is available if actionType is `APP_INSTALL`)   |
| trackingSpec  | object           | TrackingSpec | **optional** The tracking tool spec if you need. (it is available if actionType is `APP_INSTALL`) |
| targeting     | object           | Targeting    | **optional** The targeting for campaign. |

##### Parameters [AppSpec]

| Name          | Type     | Format       | Description                                |
|---------------|----------|--------------|--------------------------------------------|
| application   | string   |              | **required** The identifier for AppStore or PlayStore. |
| urlscheme     | string   |              | **optional** |

##### Parameters [TrackingSpec]

| Name          | Type     | Format       | Description                                |
|---------------|----------|--------------|--------------------------------------------|
| trackingType  | string   | TrackingType | **required** The tracking tool type if you need. |

##### Parameters [Targeting]

| Name          | Type            | Format       | Description                                |
|---------------|-----------------|--------------|--------------------------------------------|
| publishers    | array of string | string       | **optional** media targeting. |
| media         | array of string | string       | **optional** media targeting. **this parameter is deprecated and delete near future release. please use `publishers`** |
| devices       | array of string | DeviceType   | **optional** device targeting. |
| genders       | array of string | GenderType   | **optional** gender targeting. |
| gender        | string          | GenderType   | **optional** gender targeting. **this parameter is deprecated and delete near future release. please use `publishers`** |
| cities        | array of object | CityTargeting | **optional** area targeting. |
| schedules     | array of object | TimeSchedule | **optional** schedule targeting. |

##### Parameters [CityTargeting]

| Name          | Type           | Format       | Description                                |
|---------------|----------------|--------------|--------------------------------------------|
| key           | string         | int          | **optional** media targeting. |

##### Request Example

```
{
  "actionType": "APP_INSTALL",
  "name": "SmartNews Installs 7/12",
  "sponsoredName": "SmartAds",
  "bidAmount": 30,
  "targetCpa": 300,
  "totalBudget": 20000,
  "dailyBudget": 5000,
  "startTime": "2015-07-01T13:40:00Z",
  "endTime": "2015-07-29T13:40:00Z",
  "adCategoryId": "3",
  "appSpec": {
    "application": "579581125",
    "urlscheme": "jp.gocro.smartnews://"
  },
  "trackingSpec": {
    "trackingType": "SAT"
  },
  "targeting": {
    "publishers": ["120", "121", "99810"],
    "devices": ["IPHONE", "IPOD", "IPAD"],
    "genders": ["MALE"],
    "cities": [
      {
        "key": "13000"
      }
    ],
    "schedules": [
      {
        "startSecond": 0,
        "endSecond": 14400
      }
    ]
  }
}
```

##### Example Response

```
{
  "data": {
    "campaignId": "1000006"
  }
}
```

#### POST /v1.0/campaigns/{campaignId}/update

Update an existing campaign.

##### Parameters [in request path]

| Name          | Type             | Format       | Description                                 |
|---------------|------------------|--------------|---------------------------------------------|
| campaignId    | string           | long         | **required** The identifier for a campaign. |

##### Parameters [in request payload]

| Name               | Type     | Format       | Description                                |
|--------------------|----------|--------------|--------------------------------------------|
| name               | string   |              | **optional** |
| sponsoredName      | string   |              | **optional** |
| bidAmount          | number   | long         | **optional** |
| targetCpa          | number   | long         | **optional** |
| totalBudget        | number   | long         | **optional** |
| dailyBudget        | number   | long         | **optional** |
| startTime          | string   | ISO 8601     | **optional** |
| endTime            | string   | ISO 8601     | **optional** |
| adCategoryId       | string   | int          | **optional** |
| appSpec            | object   | AppSpec      | **optional** |
| trackingSpec       | object   | TrackingSpec | **optional** |
| targeting          | object   | Targeting    | **optional** |

##### Example Request

```
{
  "name": "SmartNews Installs 7/12",
  "sponsoredName": "SmartAds",
  "bidAmount": 30,
  "targetCpa": 3000,
  "totalBudget": 20000,
  "dailyBudget": 5000,
  "startTime": "2015-07-01T13:40:00Z",
  "endTime": "2015-07-29T13:40:00Z",
  "adCategoryId": "3",
  "appSpec": {
    "application": "579581125",
    "urlscheme": "jp.gocro.smartnews://"
  },
  "trackingSpec": {
    "trackingType": "SAT"
  },
  "targeting": {
    "publishers": ["250", "251", "252"],
    "devices": ["IPHONE", "IPOD", "IPAD"],
    "genders": ["MALE"],
    "cities": [
      {
        "key": "13000"
      }
    ],
    "schedules": [
      {
        "startSecond": 0,
        "endSecond": 14400
      }
    ]
  }
}
```

##### Example Response

```
{
  "data": {
    "campaignId": "1000007"
  }
}
```

#### POST /v1.0/campaigns/{campaignId}/submit_review

Request to review all of `pending` creatives associated with the given campaign.

##### Example Request

Request with no content body.

##### Example Response

```
{
  "data": [
    { "creativeId": "1000007", "approvalStatus": "PENDING_REVIEW" },
    { "creativeId": "1000008", "approvalStatus": "PENDING_REVIEW" },
    { "creativeId": "1000009", "approvalStatus": "PENDING_REVIEW" }
  ]
}
```

#### POST /v1.0/campaigns/{campaignId}/pullback_pending_review

Pull back all of creatives in status of `pending_review`.

##### Example Request

Request with no content body.

##### Example Response

```
{
  "data": [
    { "creativeId": "1000010", "approvalStatus": "PENDING" },
    { "creativeId": "1000011", "approvalStatus": "PENDING" },
    { "creativeId": "1000012", "approvalStatus": "PENDING" }
  ]
}
```

#### POST /v1.0/campaigns/{campaignId}/update_enable

Change campaign `enable` field to `true` or `false`. (`true` means deliverable state, and `false` means *NOT* deliverable state)

##### Example Request

```
true
```

##### Example Response

```
{
  "data": {
    "campaignId": "1000007"
  }
}
```

### Creatives

#### GET /v1.0/campaigns/{campaignId}/creatives

Retrieve creative information associated with the current campaign.

##### Example Response

For general image or video creative

```
{
  "data": [
    {
      "name": "label for creative management",
      "creativeId": "1000013",
      "enable": false,
      "title": "Trending News & Stories",
      "text": "Your news in one minute. Get the award-winning, addictively simple news app downloaded by over 12 million readers in 150 countries!",
      "imageset": {
        "a": {
          "imageId": "1000015",
          "imageUrl": "http://creative.smartnews-ads.com/path/to/aaa.jpg"
        },
        "b": {
          "imageId": "1000016",
          "imageUrl": "http://creative.smartnews-ads.com/path/to/aaa.jpg"
        },
        "c": {
          "imageId": "1000017",
          "imageUrl": "http://creative.smartnews-ads.com/path/to/aaa.jpg"
        }
      },
      "trackingUrl": "http://foo.trackingsystem.com/?a=b&c=d&e=f",
      "targeting": {
        "genres": ["1", "2"]
      },
      "approvalStatus": "PENDING",
      "merchandiseCatalogId": "1000024",
      "urlTags": "utm_source=smartnews&key=val"
    }
  ]
}
```

If it is carousel type creative, creative object does not have `title` and `imageset` property under the object root. Instead, it has an `assetGroups` property.
You can determine if the carousel feature is enabled by the `isStoryCreative` flag.

```
{
  "data": [
    {
      "name": "label for creative management",
      "creativeId": "1000013",
      "enable": false,
      "text": "Your news in one minute. Get the award-winning, addictively simple news app downloaded by over 12 million readers in 150 countries!",
      "assetGroups": {
        "a": {
          "title": "First Carousel Item Title",
          "image": {
            "imageId": "1000015",
            "imageUrl": "http://creative.smartnews-ads.com/path/to/aaa.jpg"
          }
        },
        "b": {
          "title": "Second Carousel Item Title",
          "image": {
            "imageId": "1000016",
            "imageUrl": "http://creative.smartnews-ads.com/path/to/aaa.jpg"
          }
        },
        "c": {
          "title": "Third Carousel Item Title",
          "image": {
            "imageId": "1000017",
            "imageUrl": "http://creative.smartnews-ads.com/path/to/aaa.jpg"
          }
        }
      },
      "approvalStatus": "PENDING",
      "isStoryCreative": true,
      "merchandiseCatalogId": "1000024",
      "urlTags": "utm_source=smartnews&key=val"
    }
  ]
}

```

#### GET /v1.0/creatives/{creativeId}

Retrieve creative information.

##### Example Response

```
{
  "data": {
    "name": "label for creative management",
    "creativeId": "1000020",
    "enable": false,
    "title": "Trending News & Stories",
    "text": "Your news in one minute. Get the award-winning, addictively simple news app downloaded by over 12 million readers in 150 countries!",
    "imageset": {
      "a": {
        "imageId": "1000022",
        "imageUrl": "http://creative.smartnews-ads.com/path/to/aaa.jpg"
      },
      "b": {
        "imageId": "1000023",
        "imageUrl": "http://creative.smartnews-ads.com/path/to/aaa.jpg"
      },
      "c": {
        "imageId": "1000024",
        "imageUrl": "http://creative.smartnews-ads.com/path/to/aaa.jpg"
      }
    },
    "trackingUrl": "http://foo.trackingsystem.com/?a=b&c=d&e=f",
    "targeting": {
      "genres": ["1", "2"]
    },
    "approvalStatus": "PENDING",
    "merchandiseCatalogId": "1000024",
    "urlTags": "utm_source=smartnews&key=val"
  }
}
```

#### POST /v1.0/campaigns/{campaignId}/creatives

Creates a new creative associated to a given campaign.

##### Parameters [in request path]

| Name          | Type             | Format       | Description                                |
|---------------|------------------|--------------|--------------------------------------------|
| campaignId    | string           |              | **required** The identifier for a campaign. |

##### Parameters [in request payload]

| Name          | Type     | Format       | Description                                |
|---------------|----------|--------------|--------------------------------------------|
| name          | string   |              | **required** The name for creative.        |
| title         | string   |              | **required** The title for creative. It is used as creative short text. |
| text          | string   |              | **required** The text for creative. It is used as creative long text. |
| imageset      | object   | ImageSetC    | **required** The image set for creative. |
| linkUrl       | string   | url          | **required if actionType is `APP_INSTALL`** The link destination on click ad. |
| trackingUrl   | string   | url          | **optional** The url for tracking tool. |
| targeting     | object   | CreativeTargeting | **optional** The targeting for creative. |

##### Parameters [ImageSetC]

| Name          | Type     | Format       | Description                                |
|---------------|----------|--------------|--------------------------------------------|
| a             | object   | Image        | **required** The image type for `A` (size of 300x300). |
| b             | object   | Image        | **required** The image type for `B` (size of 600x500). |
| c             | object   | Image        | **required** The image type for `C` (size of 1280x720). |

##### Parameters [Image]

| Name          | Type     | Format       | Description                                |
|---------------|----------|--------------|--------------------------------------------|
| imageId       | string   |              | **required** The identifier for a image. |

##### Parameters [CreativeTargeting]

| Name          | Type     | Format       | Description                                |
|---------------|----------|--------------|--------------------------------------------|
| genres        | array    | number       | **optional** Genre targeting. |

##### Example Request

```
{
  "name": "label for creative management",
  "title": "Trending News & Stories",
  "text": "Your news in one minute. Get the award-winning, addictively simple news app downloaded by over 12 million readers in 150 countries!",
  "imageset": {
    "a": { "imageId": "1000041" },
    "b": { "imageId": "1000042" },
    "c": { "imageId": "1000043" }
  },
  "trackingUrl": "http://foo.trackingsystem.com/?a=b&c=d&e=f",
  "targeting": {
    "genres": ["1", "2"]
  }
}
```

##### Example Response

```
{
  "data": {
    "creativeId": "1000025"
  }
}
```

#### POST /v1.0/creatives/{creativeId}/update

Updates an existing creative.

##### Parameters [in request path]

| Name          | Type     | Format       | Description                                |
|---------------|----------|--------------|--------------------------------------------|
| creativeId    | string   |              | **required** The identifier for a creative. |

##### Parameters [in request payload]

| Name          | Type     | Format       | Description                                |
|---------------|----------|--------------|--------------------------------------------|
| name          | string   |              | **optional** The name for creative.        |
| title         | string   |              | **optional** The title for creative. It is used as creative short text. |
| text          | string   |              | **optional** The text for creative. It is used as creative long text. |
| imageset      | object   | ImageSetU    | **optional** The image set for creative. |
| linkUrl       | string   | url          | **optional** The link destination on click ad. |
| trackingUrl   | string   | url          | **optional** The url for tracking tool. |
| targeting     | object   | CreativeTargeting | **optional** The taregeting for creative. |

##### Parameters [ImageSetU]

| Name          | Type     | Format       | Description                                |
|---------------|----------|--------------|--------------------------------------------|
| a             | object   | Image        | **optional** The image type for `A` (size of 300x300). |
| b             | object   | Image        | **optional** The image type for `B` (size of 600x500). |
| c             | object   | Image        | **optional** The image type for `C` (size of 1280x720). |

##### Example Request

```
{
  "name": "label for creative management",
  "creativeId": "10000030",
  "title": "Trending News & Stories",
  "text": "Your news in one minute. Get the award-winning, addictively simple news app downloaded by over 12 million readers in 150 countries!",
  "imageset": {
    "a": { "imageId": "10000031" },
    "b": { "imageId": "10000032" },
    "c": { "imageId": "10000033" }
  },
  "trackingUrl": "http://foo.trackingsystem.com/?a=b&c=d&e=f",
  "targeting": {
    "genres": ["1", "2"]
  }
}
```

##### Example Response

```
{
  "data": {
    "creativeId": "10000030"
  }
}
```

#### POST /v1.0/creatives/{creativeId}/update_enable

Change creative `enable` to `true` or `false`. (`true` means deliverable state, and `false` means *NOT* deliverable state)

##### Example Request

```
true
```

##### Example Response

```
{
  "data": {
    "creativeId": "1000037"
  }
}
```


#### POST /v1.0/accounts/{accountId}/images/upload

Upload an image for creative.
This api needs request's content body as `multipart/form-data` with `file` form parameter, and image upload size is up to 500KB.

##### Example Request

```
curl -v --header "X-Auth-Api: {api_key}" -F file=@1035456.png {endpoint}/accounts/1000648/images/upload
```

##### Example Response

```
{
  "data": {
    "imageId": "1000884",
    "imageUrl": "https://xxxx.xxxx.xxx/1000648/201510/pX9yAXmW5iBJjQjY/1000884.png",
    "filename": "1035456.png",
    "width": 1280,
    "height": 720,
    "imageType": "C"
  }
}
```

### Other Related Objects

#### GET /v1.0/publishers

Retrieve available media for targeting.

##### Response Example

```
{
  "data": [
    {
      "publisherId": "120",
      "name": "SmartNews"
    },
    {
      "publisherId": "121",
      "name": "mixi"
    },
    {
      "publisherId": "99810",
      "name": "cookpad"
    }
  ]
}
```

#### GET /v1.0/adcategories

Retrieve available categories for targeting.

##### Response Example

```
{
  "data": [
    {
      "name": "エンタテイメント",
      "categoryId": "3",
      "parentId": null,
      "level": 1
    },
    {
      "name": "ショー/演劇",
      "categoryId": "23",
      "parentId": "3",
      "level": 2
    },
    {
      "name": "コンピューター/電子機器",
      "categoryId": "5",
      "parentId" : null,
      "level": 1
    }
  ]
}
```

#### GET /v1.0/genres

Retrieve available genres for targeting.

##### Response Example

```
{
  "data": [
    {
      "genreId": "1",
      "name": "エンタメ"
    },
    {
      "genreId": "2",
      "name": "グルメ"
    }
  ]
}
```

#### GET /v1.0/cities

Retrieve available cities for targeting.

##### Response Example

```
{
  "data": [
    {
      "name": "東京都",
      "cityId": "13000",
      "parentId": null,
      "level": 1
    },
    {
      "name": "渋谷区",
      "cityId": "13113",
      "parentId": "13000",
      "level": 2
    },
    {
      "name": "神奈川県",
      "categoryId": "14000",
      "parentId" : null,
      "level": 1
    }
  ]
}
```

## Appendix

### Object Reference

#### General Types

| Name               | Type             | Spec                |
|--------------------|------------------|---------------------|
| boolean            | boolean          | (or true false)     |
| looseUrl           | string           | /^https?://.+/      |
| looseUrlScheme     | string           | /^[^:]+:.*/         |

#### Campaign

| Name                 | Type            | Format           | Required          | Spec         |
|----------------------|-----------------|------------------|-------------------|--------------|
| actionType           | string          | ActionType       | Y                 | |
| name                 | string          |                  | Y                 | (and nonEmpty (maxLength 128)) |
| sponsoredName        | string          |                  | Y                 | (and nonEmpty (maxLength 128)) |
| bidAmount            | long            |                  | Y                 | (min floorPrice) |
| targetCpa            | long            |                  | N                 | (min (multiple 100 1_000_000)) |
| totalBudget          | long            |                  | Y                 | (min 1_000_000) |
| dailyBudget          | long            |                  | Y                 | (min 1_000_000) |
| startTime            | string          | ISO 8601         | Y                 | (and (format "yyyy-MM-dd'T'HH:mm:ssZ") (min currentTime)) |
| endTime              | string          | ISO 8601         | Y                 | (and (format "yyyy-MM-dd'T'HH:mm:ssZ") (min startTime)) |
| adCategoryId         | int             |                  | Y                 | (existIn /adcategories) |
| appSpec              | object          | AppSpec          | Y if APP_INSTALL  | (and nonEmpty (maxLength 256) (or androidAppId iosAppId)) |
| trackingSpec         | object          | TrackingType     | N                 | it is available if actionType is `APP_INSTALL` |

#### AppSpec

| Name                 | Type            | Format   | Required          | Spec         |
|----------------------|-----------------|----------|-------------------|--------------|
| application          | string          |          | Y                 | (and nonEmpty (maxLength 256) (or androidAppId iosAppId)) |
| urlscheme            | string          |          | N                 | (and (maxLength 1024) looseUrlScheme) |

#### TargetingSpec

| Name                 | Type            | Format           | Required | Spec         |
|----------------------|-----------------|------------------|----------|--------------|
| trackingType         | string          | TrackingType     | N        | |

#### CampaignTargeting

| Name                 | Type             | Format            | Required | Spec         |
|----------------------|------------------|-------------------|----------|--------------|
| publishers           | array of string  |                   | N        | (existIn (GET /publishers)) |
| devices              | array of string  | DeviceType        | N        | |
| genders              | array of string  | GenderType        | N        | |
| cities               | array of object  | CityTargeting     | N        | (existIn (GET /cities)) |
| schedules            | array of object  | ScheduleTargeting | N        | |

#### CityTargeting

| Name                               | Type             | Format | Required | Spec              |
|------------------------------------|------------------|--------|----------|-------------------|
| key                                | string           |        | Y        | (existIn /cities) |

#### ScheduleTargeting

| Name                               | Type             | Format   | Required | Spec              |
|------------------------------------|------------------|----------|----------|-------------------|
| startSecond                        | number           | int      | Y        | (and (min 0) (max 86400) (max endSecond)) |
| endSecond                          | number           | int      | Y        | (and (min 0) (max 86400) (min startSecond)) |

#### Creative

| Name                               | Type             | Format            | Required | Spec         |
|------------------------------------|------------------|-------------------|----------|--------------|
| name                               | string           |                   | Y        | (and nonEmpty (maxLength 128)) |
| title                              | string           |                   | Y        | (and (minLength 10) (maxLength 25)) |
| text                               | string           |                   | Y        | (and (minLength 10) (maxLength 90)) |
| imageset                           | object           | CreativeImageSet  | Y        | |
| linkUrl                            | string           |                   | Y        | (and nonEmpty (maxLength 1024) looseUrl) |
| trackingUrl                        | string           |                   | N        | (and nonEmpty (maxLength 1024) looseUrl) |
| targeting                          | object           | CreativeTargeting | Y        | |

#### CreativeImage

| Name                               | Type             | Format   | Required | Spec         |
|------------------------------------|------------------|----------|----------|--------------|
| imageId                            | string           |          | Y        |              |

#### CreativeImageSet

| Name                               | Type             | Format         | Required | Spec         |
|------------------------------------|------------------|----------------|----------|--------------|
| a                                  | object           | CreativeImage  | Y        |              |
| b                                  | object           | CreativeImage  | Y        |              |
| c                                  | object           | CreativeImage  | Y        |              |

#### CreativeTargeting

| Name                               | Type             | Format          | Required | Spec                    |
|------------------------------------|------------------|-----------------|----------|-------------------------|
| genres                             | array of string  |                 | N        | (existIn (GET /genres)) |

### Ads Enumerations References

#### ActionType

| ActionType  |
|-------------|
| APP_INSTALL |
| WEBSITE_CONVERSION |

#### TrackingType

| TrackingType | Vendor Description            |
|--------------|-------------------------------|
| NONE         | no tracking used              |
| FOX          | FOX                           |
| ADX          | AD-X Tracking                 |
| MAT          | Mobile App Tracking           |
| KOCHAVA      |                               |
| PARTYTRACK   |                               |
| KING         |                               |
| ADJUST       |                               |
| SAT          | SmartNews Ads Tracking        |

#### DeviceType

| DeviceType     |
|----------------|
| IPHONE         |
| IPOD           |
| IPAD           |
| ANDROID        |
| ANDROID_TABLET |

#### GenderType

| GenderType     |
|----------------|
| MALE           |
| FEMALE         |

#### CampaignApprovalStatus

| CampaignApprovalStatus |
|------------------------|
| PENDING                |
| APPROVED               |

#### CreativeApprovalStatus

| CreativeApprovalStatus |
|------------------------|
| PENDING                |
| PENDING_REVIEW         |
| APPROVED               |
| REJECTED               |


## Custom Audience

The SmartNews Ads Custom Audience API allows partners to manipulate custom audiences without accessing the SmartNews advertising web console.

Steps
1. Upload CSV formatted audience file to SmartNews Ads server
2. Register the uploaded file as a Custom Audience data in SmartNews Ads platform
3. You can get your audience file status. Once you can confirm the custom audience data is ready to use, you can use it
4. If you don’t need the audience data anymore, you can delete the data file physically from SmartNews Ads server

### Uploading ID list file

#### POST /v2.0/audiences/idlist

Upload CSV formatted audience file to SmartNews Ads server. The file should contain 1 ID per line, and the first line
of the file should contain a string with the ID type (currently the only accepted type is `madid`).

##### Parameters [in request payload]

| Name          | Type             | Format              | Description                   |
|---------------|------------------|---------------------|-------------------------------|
| file          | (string, object) | (string, file data) |                               |
| accountId     | string           |                     | accountId you allow to access |

##### Example Result

```
{
  "data": {
    "idlistId": "L2N1c3RvbWF1ZGllbmNlL2lkbGC8yMaU1BvT0xLQS5neg=="
  }
}
```

### Register Audience

#### POST /v2.0/audiences

Register the uploaded file as a Custom Audience data in SmartNews Ads platform

##### Parameters [in request payload]

| Name          | Type       | Format      | Description                                          |
|---------------|------------|-------------|------------------------------------------------------|
| name          | string     |             | **required** The name of the custom audience         |
| idlistId      | string     |             | **required** idlistId that you got by idlist request |
| description   | string     |             | **optional** description for this custom audience    |
| accountId     | string     |             | **required** accountId to create audience under      |
| type          | string     |             | **required** must be "idlist"                        |



##### Example Result

```
{
   "data":{
      "audienceId":10000001,
      "accountId":10000000,
      "name":"Audience API test",
      "description":"This is a test ",
      "type":"idlist",
      "status":"enabled",
      "ready":false,
      "size":0,
      "configuration":{
         "type":"idlist",
         "data":{
            "type":"idlist",
            "idlistId":"L2N1c3RvbWF1ZGllbmNlL2lkbGC8yMaU1BvT0xLQS5neg=="
         }
      },
      "createdAt":"2021-07-20T09:32:25+0000",
      "updatedAt":"2021-07-20T09:32:25+0000"
   }
}
```

### Get an idlist status

#### GET /v2.0/audiences/idlist/{idlistId}

Retrieve an idlist status using idlistId

##### Example Result

```
{
   "data":{
      "idlistId":"L2N1c3RvbWF1ZGllbmNlL2lkbGC8yMaU1BvT0xLQS5neg==",
      "idlistStatus":"UPLOADED"
   }
}
```

### Get all custom audience info

#### GET /v2.0/audiences?accountId={accountId}

Get all audience info of an accountId. By adding &audienceIds={audienceId*1*},…,{audienceId*k*} to the url paramenter, you can get the audienceIds specific information.

##### Example Result

```
{
   "data":[
      {
         "audienceId":10000001,
         "accountId":10000000,
         "name":"Audience API test",
         "description":"this is a test",
         "type":"idlist",
         "status":"archived",
         "ready":false,
         "size":0,
         "configuration":{
            "type":"idlist",
            "data":{
               "type":"idlist",
               "idlistId":"L2N1c3RvbWF1ZGllbmNlL2lkbGC8yMaU1BvT0xLQS5neg=="
            }
         },
         "createdAt":"2021-07-13T07:40:35+0000",
         "updatedAt":"2021-07-13T09:49:41+0000"
      },
      {
         "audienceId":10000002,
         "accountId":10000000,
         "name":"Audience API test2",
         "description":"this is a test2",
         "type":"idlist",
         "status":"archived",
         "ready":false,
         "size":0,
         "configuration":{
            "type":"idlist",
            "data":{
               "type":"idlist",
               "idlistId":"F1ZGllbmNlL2lkbGC8yMaU1BvkbGC8yMaU1BvT0xLQS5neg=="
            }
         },
         "createdAt":"2021-07-13T07:41:05+0000",
         "updatedAt":"2021-07-13T09:49:30+0000"
      }
   ]
}
```

### Get an audience info

#### GET /v2.0/audiences/{audienceId}

Retrieve an audience info by audienceId

##### Example Result

```
{
   "data":{
      "audienceId":10000002,
      "accountId":1000000,
      "name":"Audience API test2",
      "description":"this is a test2",
      "type":"idlist",
      "status":"enabled",
      "ready":false,
      "size":0,
      "configuration":{
         "type":"idlist",
         "data":{
            "type":"idlist",
            "idlistId":" F1ZGllbmNlL2lkbGC8yMaU1BvkbGC8yMaU1BvT0xLQS5neg=="
         }
      },
      "createdAt":"2021-07-13T07:43:48+0000",
      "updatedAt":"2021-07-13T07:43:48+0000"
   }
}
```

### Delete an idlist

#### DELETE /v2.0/audiences/idlist/{idlistId}

Delete an idlistId file from SmartNews Ads server physically

##### Example Result

```
{
   "success":true
}
```

### Update status of an Audience

#### PUT /v2.0/audiences/{audienceId}/status

Update status of specified audience. There are following rules
1.	To switch status to "archive", the current status must be "disabled"
2.	In case the current status is "archived", you cannot switch to "enable" or "disable"

##### Parameters [in request payload]

| Name          | Type       | Format      | Description                                   |
|---------------|------------|-------------|-----------------------------------------------|
| status        | string     |             | must be one of "enable", "disable", "archive" |

##### Example Result

```
{
   "success":true
}
```


### Update Audience name and description

#### PUT /v2.0/audiences/{audienceId}

Update name and description of an existing audience. Note: at least one parameter must be set to receive a successful 
response.

##### Parameters [in request payload]

| Name          | Type       | Format      | Description                                          |
|---------------|------------|-------------|------------------------------------------------------|
| name          | string     |             | **optional** The name of the custom audience         |
| description   | string     |             | **optional** The description of the custom audience  |

##### Example Result

```
{
   "success":true
}
```
