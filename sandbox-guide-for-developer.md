# Developer Guide for SmartNews Ads Advertising API

We provide **SANDBOX** environment for SmartNews Ads Advertising API.

## Endpoint for SANDBOX

https://sandbox-partners.smartnews-ads.com/api


## Provided sandbox private APIs for developer

#### POST /\__sandbox/insights/dataimport

```
Content-Type:
  text/csv

Request-Payload:
  - begin_time_sec: <string, format:YYYYMMDD>
  - advertiser_id: <string>
  - campaign_id: <string>
  - creative_id: <string>
  - impression_count: <long>
  - viewable_impression_count: <long>
  - click_count: <long>
  - conversion_count: <long>
  - consumed_price: <long>

Request-Example:
  % echo '20160501,1000648,1000658,1000674,1,2,3,4,5' >> data.csv
  % echo '20160501,1000648,1000658,1000671,1,2,3,4,5' >> data.csv
  % curl -H "X-Auth-Api: <YOUR-API-KEY>" https://sandbox-partners.smartnews-ads.com/api/v1.0/__sandbox/insights/dataimport -X POST --data-binary @data.csv
```

#### POST /\__sandbox/creatives/{creativeId}/approve

```
Content-Type:
  application/json

Request-Payload:
  - None

Request-Example:
  % curl -H "X-Auth-Api: <YOUR-API-KEY>" https://sandbox-partners.smartnews-ads.com/api/v1.0/__sandbox/creatives/1000674/approve -X POST
```

#### POST /\__sandbox/creatives/{creativeId}/reject

```
Content-Type:
  application/json

Request-Payload:
  - None

Request-Example:
  % curl -H "X-Auth-Api: <YOUR-API-KEY>" https://sandbox-partners.smartnews-ads.com/api/v1.0/__sandbox/creatives/1000674/reject -X POST
```
