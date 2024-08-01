# [Beta] SmartNews ConversionAPI (including Get Method)

## Introduction
This document provides the technical information of  the Conversion API used to send advertiser self-attributed conversion data to Smartnews through S2S communication which is widely provided by major ad networks


* Google: Conversion Management Ads API
* Meta: ConversionAPI
* TikTok: EventAPI

## Preparation
Before integrating ConversionAPI, the advertisers need reach out to Smartnews Ads Sales team, who will work with the engineering team in preparing the following assets which will be used in ConversionAPI request
* Partner name
* Authentication token
    * Important : While using GET requests, the auth token is not strictly enforced; however, we highly recommend including the token to enhance security.

## ConversionAPI Usage
### ConversionAPI URL
In order to send conversion events, advertisers need issue POST request to the following URLs with the authorization token included in the request header


Production Endpoint:
https://log.smartnews-ads.com/conversion_api/{api_version}/{partner_name}

Staging Endpoint (test only)
https://stg-log.smartnews-ads.com/conversion_api/{api_version}/{partner_name}

### URL path parameters

| Parameter name | Parameter scope  | Value type | Example    | Description                     |
|----------------|------------------|------------|------------|---------------------------------|
| partner_name   | URL parameter    | String     | smartnews  | The name of partner             |
| api_version    | URL parameter    | String     | v1         | API version                     |
| Authorization  | Header parameter | String     | v130ad1213 | Token for verifying the request |



### Example:
Post Request
```sh
curl 'https://log.smartnews-ads.com/conversion_api/v1/smartnews’
-X POST \
-H 'Authorization: {authorization_token_issued_by_SN}'
```
GET Request
```sh
curl 'https://log.smartnews-ads.com/conversion_api/v1/smartnews?clickId=xxxxx’
-X GET \
-H 'Authorization: {authorization_token_issued_by_SN}'
```

## Request parameters
**Important:**  Parameters can be included as query parameters in GET requests or as body parameters in POST requests.

### Parameters For Web Conversions
| Parameter name    | Value type         | Example                                                                                                             | Description                                                                                                                                                                                                                                                                                            |
|-------------------|--------------------|---------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| action_source     | String             | web, app, email, physical_store                                                                                     | Required The value should be “web” for web conversion                                                                                                                                                                                                                                                  |
| event_name        | String             | Purchase                                                                                                            | Required The conversion event name, please follow SmartNews Standard Event name.                                                                                                                                                                                                                       |
| pixel_tag_id      | String             | 9112675ff25eb25f580feb82                                                                                            | Optional Either pixel_tag_id or event_source_url need to be provided The Smartnews issued pixel tag id                                                                                                                                                                                                 |
| event_source_url  | String             | http://www.smartnews.com?store=abc                                                                                  | Optional Either pixel_tag_id or event_source_url need to be provided  The browser URL where the event happened. The URL must begin with http:// or https:// Please encode URL using GET method                                                                                                         |
| click_id          | String             | UnoPeo4IDmEwnHepAAEA                                                                                                | Optional  either click_id or client_ip_address+client_user_agent must be provided The unique identifier for an ad request so that SN can accurately attribute conversion to click In order to use click_id, the advertisement landing page need to be configured properly (please refer to guide "Send Click ID within Request") |
| client_ip_address | String             | IPv4: 133.242.187.207 IPv6:  2001:ac8:40:12:5578:37e:193f:d5ea                                                      | Optional  either click_id or client_ip_address+client_user_agent+os_version must be provided                                                                                                                                                                                                           |
| client_user_agent | String             | Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.141 Safari/537.36 | Optional  either click_id or client_ip_address+client_user_agent+os_version must be provided                                                                                                                                                                                                           |
| client_os_version | String             |                                                                                                                     | Optional  either click_id or client_ip_address+client_user_agent+os_version must be provided                                                                                                                                                                                                           |
| referrer          | String             | https://example.com/page?q=123                                                                                      | Optional                                                                                                                                                                                                                                                                                               |
| event_time        | Long               | 1473668802                                                                                                          | Required The timestamp of event happens, accurate to the second                                                                                                                                                                                                                                        |
| properties        | Object (Key-value) | { “Item_id”: “akashiro:10003938”, “unit_price”: 500, “currency”: “JPY”, “quantity”: 2, “shop_id”: “akashiro” }      | Optional Only support in Body Parameter Can provide extra information about the conversion, such as the purchased item’s price & quantity More descriptions of this field please refer to following section                                                                                            |


### Parameters For App Conversions
| Parameter name    | Value type         | Example                                                                                                             | Description                                                                                                                                                                                                                                                                                                     |
|-------------------|--------------------|---------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| action_source     | String             | web, app, email, physical_store                                                                                     | Required The value should be app for app conversion                                                                                                                                                                                                                                                             |
| event_name        | String             | Install                                                                                                             | Required The conversion event name, please follow SmartNews Standard Event name.                                                                                                                                                                                                                                |
| store_id          | String             | iOS: 579581125 Android:jp.gocro.smartnews.android                                                                   | Required iOS or Android App Store ID                                                                                                                                                                                                                                                                            |
| mobile_platform   | String             | iOS / Android                                                                                                       | Required Mobile platform, either iOS or Android                                                                                                                                                                                                                                                                 |
| click_id          | String             | UnoPeo4IDmEwnHepAAEA                                                                                                | Optional  either click_id or ad_id or client_ip_address+client_user_agent must be provided The unique identifier for an ad request so that SN can accurately attribute conversion to click In order to use click_id, the advertisement landing page need to be configured properly (please refer to guide "Send Click ID within Request") |
| ad_id             | String             |                                                                                                                     | Optional  either click_id or ad_id or client_ip_address+client_user_agent must be provided IDFA on iOS system AAID on Android system                                                                                                                                                                            |
| client_ip_address | String             | IPv4: 133.242.187.207 IPv6:  2001:ac8:40:12:5578:37e:193f:d5ea                                                      | Optional  either click_id or ad_id or client_ip_address+client_user_agent must be provided                                                                                                                                                                                                                      |
| client_user_agent | String             | Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.141 Safari/537.36 | Optional  either click_id or ad_id or client_ip_address+client_user_agent must be provided                                                                                                                                                                                                                      |
| client_os_version | String             |                                                                                                                     | Optional  OS version is required when click_id or ad_id cannot be provided and mobile platform is iOS                                                                                                                                                                                                           |
| event_time        | Long               | 1473668802                                                                                                          | Required The timestamp of event happens, accurate to the second                                                                                                                                                                                                                                                 |
| properties        | Object (Key-value) | { “Item_id”: “akashiro:10003938”, “unit_price”: 500, “currency”: “JPY”, “quantity”: 2, “shop_id”: “akashiro” }      | Optional Only support in Body Parameter for POST type request Can provide extra information about the conversion, such as the purchased item’s price & quantity More descriptions of this field please refer to following section                                                                               |



### The supported properties parameters
| Parameter name | Value type | Example           | Description                                          |
|----------------|------------|-------------------|------------------------------------------------------|
| item_id        | String     | akashiro:10003938 | Optional                                             |
| event_value    | Float      | 500.0             | Optional Can be the unit price of the item purchased |
| currency       | String     | JPY, USD          | Optional JPY by default if not provided              |
| quantity       | Integer    | 2                 | Optional The number of purchased items               |
| shop_id        | String     | akashiro          | Optional                                             |



## Send Click ID within Request
### About SmartNews Click ID
SmartNews Click Id  (click_id) is a tracking parameter that gets passed through ConversionAPI parameters.
It is strongly recommended to send back the click_id via ConversionAPI  for all events from your website. Doing so helps SmartNews accurately associate events on your website with the ad click and allows SmartNews to better report and optimize your campaign performance.

### Get Click ID through landing page
In order to obtain the click ID sent within request, you need append the macro {click_id} as part of landing page parameter (example https://smartnews.com/abc&click_id={click_id}) , smartnews ad server will automatically parse this macro with click identifier.

you can retrieve the Click ID by reading the click_id parameter from landing page URL, then and send it to your server to report via the ConversionAPI


## Example ConversionAPI requests

### Example of Android purchase event:
Example of POST Request
```sh
curl --request POST 'https://log.smartnews-ads.com/conversion_api/v1/smartnews \
--header 'Content-Type: application/json' \
--header ‘Authorization: {authorization token issued by SN}’ \
--data-raw '{
  "action_source": "app",
  "event_name": "Purchase",
  "store_id": "jp.gocro.smartnews.android",
  "mobile_platform": "Android",
  "click_id": "UnoPeo4IDmEwnHepAAEA",
  "event_time": 1473668802,
  "properties": {
    "item_id": "akashiro:10003938",
    "shop_id": "akashiro",
    "event_value": 100.1,
    "currency": "JPY",
    "quantity": 2
  }
}
'
```
Example of GET Request
```sh
curl --request GET 'https://log.smartnews-ads.com/conversion_api/v1/smartnews?action_source=app&event_name=Purchase&store_id=jp.gocro.smartnews.android&mobile_platform=Android&click_id=UnoPeo4IDmEwnHepAAEA&event_time=1473668802'
\
--header ‘Authorization: {authorization token issued by SN}’ \
```

### Example of web purchase event:
Example of POST Request
```sh
curl --request POST 'https://log.smartnews-ads.com/conversion_api/v1/smartnews \
--header 'Content-Type: application/json' \
--header ‘Authorization: {authorization token issued by SN}’ \
--data-raw '{
  "action_source": "web",
  "event_name": "purchase",
  "pixel_tag_id": "9112675ff25eb25f580feb82",
  "event_source_url": "http://www.smartnews.com?store=abc",
  "click_id": "UnoPeo4IDmEwnHepAAEA",
  "event_time": 1473668802,
  "properties": {
    "item_id": "akashiro:10003938",
    "shop_id": "akashiro",
    "event_value": 500,
    "currency": "JPY",
    "quantity": 2
  }
}
```

Example of GET Request
```sh
curl --request GET 'https://log.smartnews-ads.com/conversion_api/v1/smartnews?action_source=web&event_name=purchase&pixel_tag_id=9112675ff25eb25f580feb82&event_source_url=http%3A%2F%2Fwww.smartnews.com%3Fstore%3Dabc&click_id=UnoPeo4IDmEwnHepAAEA&event_time=1473668802' \
--header ‘Authorization: {authorization token issued by SN}’ \
```




## Response
| Field      | Value type | Description                                                                                     |
|------------|------------|-------------------------------------------------------------------------------------------------|
| message    | String     | Indicate whether the request is successful or not, and what type of error happens               |
| request_id | String     | The unique id of the request                                                                    |
| error      | Object     | Optional“issue”: indicate what kind of error happens “detail”: the detailed reason of the error |


### Example Response

```http
HTTPS/1.1 200 OK
Content-Type: application/json
{
    "message": "OK",
    "request_id": "a326f711-1566-4002-9729-2846ae5107c8"
}
```

Response when there is error:

Case 1: field missing

```http
HTTPS/1.1 400 Bad Request
Content-Type: application/json
{
    "message": "Bad Request",
    "request_id": "a326f711-1566-4002-9729-2846ae5107c8",
    "error": {
        "issue": "Input field missing",
        "detail": "The click_id field is required"
    }
}
```

Case 2: invalid argument (such as unit_price is not a number)

```http
HTTPS/1.1 400 Bad Request
Content-Type: application/json
{
    "message": "Bad Request",
    "request_id": "a326f711-1566-4002-9729-2846ae5107c8",
    "error": {
        "issue": "Invalid input provided",
        "detail": "The ‘unit_price’' field data type is incorrect"
    }
}
```

Case 3: internal server error

```http
HTTPS/1.1 500 Internal Server Error
Content-Type: application/json
{
    "message": "Internal Server Error",
    "request_id": "a326f711-1566-4002-9729-2846ae5107c8"
}
```
## Supported Conversion Events
### Web Conversion Events
| Event Name            | Event Code (Send through ConversionAPI) |
|-----------------------|-----------------------------------------|
| Purchase              | Purchase                                |
| Add To Cart           | AddToCart                               |
| Initiate Checkout     | InitiateCheckout                        |
| Submit Form           | SubmitForm                              |
| Subscribe             | Subscribe                               |
| Complete Registration | CompleteRegistration                    |
| Contact               | Contact                                 |
| Sign Up               | SignUp                                  |
| View Content          | ViewContent                             |
| Add Payment Info      | AddPaymentInfo                          |
| Add To Wish List      | AddToWishList                           |
| Visit Cart            | VisitCart                               |
| Customize Product     | CustomizeProduct                        |
| Search                | Search                                  |
| Booking               | Booking                                 |
| Download              | Download                                |
| Start Trial           | StartTrial                              |
| Share                 | Share                                   |
| Login                 | Login                                   |
| Donate                | Donate                                  |
| Find Location         | FindLocation                            |





### App Conversions Events
| Event Name             | Event Code  (Send through ConversionAPI) |
|------------------------|------------------------------------------|
| Level Achieve          | LevelAchieve                             |
| Open Push Notification | OpenPushNotification                     |
| Rate                   | Rate                                     |
| Reengage               | Reengage                                 |
| Invite                 | Invite                                   |
| Tutorial Completion    | TutorialCompletion                       |
| Launch                 | Launch                                   |
| Purchase History       | PurchaseHistory                          |
| Like                   | Like                                     |
| Install                | Install                                  |
