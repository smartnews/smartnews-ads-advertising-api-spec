# The specification of SmartNews Ads Advertising API

- [SmartNews Ads Management API](smartnews-ads-management-api.md)
- [SmartNews Ads Insights API](smartnews-ads-insights-api.md)

# Ads Manager V2 (AMv2) support
To facilitate a smooth transition to Ads Manager V2 (AMv2), AMv2 data will be integrated into our existing public RESTful APIs. This integration allows a smooth transition from Ads Manager V1 (AMv1) to AMv2, ensuring that our users can access the latest features without disruption.

## Accessing AMv2 Data
AMv2 data is automatically available alongside AMv1 data in the following API endpoints:

- `GET /api/v1.0/creatives/{creativeId}/insights`
- `GET /api/v1.0/accounts/{accountId}/insights`
- `GET /api/v1.0/campaigns/{campaignId}/insights`
- `GET /api/v1.0/campaigns/{campaignId}`
- `GET /api/v1.0/accounts/{accountId}/campaigns`
- `GET /api/v1.0/creatives/{creativeId}`
- `GET /api/v1.0/campaigns/{campaignId}/creatives`

## Mapping of AMv2 Objects to Existing API Format
To maintain compatibility with existing workflows, it's important to understand how AMv2 objects map to the current API format. Below is a table that outlines these mappings:

| AMv2 Object | AMv1 API mapping          |
|-------------|---------------------------|
| Campaign    | *Not directly mapped*     |
| Ad Group    | Mapped to AMv1 Campaigns. |
| Ad          | Mapped to AMv1 Creatives. |

AMv2 campaign data is not directly accessible through AMv1 APIs. To obtain reporting data for AMv2 campaigns, it is necessary to aggregate the information using the `campaignId` provided in the [`amV2` objects](#distinguishing-amv2-data) within the relevant AMv1 API endpoints.

## Distinguishing AMv2 Data
To help you easily identify AMv2 data:

- All AMv2 objects within the endpoints will include a nested object named `amV2`. This object's presence indicates that the data is from Ads Manager V2.
- The `amV2` object of AMv1 data will be always `null`, allowing users to differentiate between data from AMv1 and AMv2 seamlessly.
- The structure and location of the `amV2` object may vary depending on the endpoint. To understand exactly where to find the `amV2` object for each specific endpoint, please refer to the detailed API documentation.

## Release Schedule
Starting from the end of February 2024, AMv2 data support across our API endpoints will be rolled out in a phased approach, with the rollout expected to be complete by early March 2024. Below is the provisional order of release for AMv2 data support:

1. [`GET /api/v1.0/creatives/{creativeId}/insights`](./smartnews-ads-insights-api.md#Endpoints) and [`GET /api/v1.0/campaigns/{campaignId}/insights`](./smartnews-ads-insights-api.md#Endpoints)
2. [`GET /api/v1.0/accounts/{accountId}/insights`](./smartnews-ads-insights-api.md#Endpoints)
3. [`GET /api/v1.0/campaigns/{campaignId}`](./smartnews-ads-management-api.md#get-v10campaignscampaignid) and [`GET /api/v1.0/creatives/{creativeId}`](./smartnews-ads-management-api.md#get-v10creativescreativeid)
4. [`GET /api/v1.0/accounts/{accountId}/campaigns`](./smartnews-ads-management-api.md#get-v10accountsaccountidcampaigns) and [`GET /api/v1.0/campaigns/{campaignId}/creatives`](./smartnews-ads-management-api.md#get-v10campaignscampaignidcreatives)

Please note that this order is provisional and subject to change based on development progress and testing outcomes. We encourage API users to refer to our detailed API documentation for the most current information on the release status of AMv2 data support for each endpoint.

# Contact
 [https://smartnews-ads.zendesk.com/hc/ja/requests/new?ticket_form_id=1900000456167](https://smartnews-ads.zendesk.com/hc/ja/requests/new?ticket_form_id=1900000456167)
