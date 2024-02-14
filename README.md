# The specification of SmartNews Ads Advertising API

- [SmartNews Ads Management API](smartnews-ads-management-api.md)
- [SmartNews Ads Insights API](smartnews-ads-insights-api.md)

# Ads Manager V2 (AMv2) Update for API Users
To facilitate a smooth transition to Ads Manager V2 (AMv2), AMv2 data will be integrated into our existing public RESTful APIs. This integration allows for a smooth transition from Ads Manager V1 (AMv1) to AMv2, ensuring that our users can access the latest features without disruption.

## Accessing AMv2 Data
AMv2 data is automatically available alongside AMv1 data in the following API endpoints:

- `GET /api/v1.0/creatives/{creativeId}/insights`
- `GET /api/v1.0/accounts/{accountId}/insights`
- `GET /api/v1.0/campaigns/{campaignId}/insights`
- `GET /api/v1.0/campaigns/{campaignId}`
- `GET /api/v1.0/accounts/{accountId}/campaigns`
- `GET /api/v1.0/creatives/{creativeId}`
- `GET /api/v1.0/campaigns/{campaignId}/creatives`

## Distinguishing AMv2 Data
To help you easily identify AMv2 data:

- All AMv2 objects within the endpoints will include a nested object named `amV2`. This object's presence indicates that the data is from Ads Manager V2.
- AMv1 data will not contain the `amV2` nested object, allowing users to differentiate between data from AMv1 and AMv2 seamlessly.
- The structure and location of the `amV2` object may vary depending on the endpoint. To understand exactly where to find the `amV2` object for each specific endpoint, please refer to the detailed API documentation.

## Release Schedule
The support for AMv2 data is being rolled out gradually. Each endpoint will have its own release date for AMv2 data support. We encourage API users to refer to our detailed API documentation ([SmartNews Ads Management API](smartnews-ads-management-api.md) and [SmartNews Ads Insights API](smartnews-ads-insights-api.md) ) for the most current release status of AMv2 data across different endpoints.

Please check our API documentation regularly for updates on the availability of AMv2 data and further enhancements to our APIs.

# Contact
 [https://smartnews-ads.zendesk.com/hc/ja/requests/new?ticket_form_id=1900000456167](https://smartnews-ads.zendesk.com/hc/ja/requests/new?ticket_form_id=1900000456167)