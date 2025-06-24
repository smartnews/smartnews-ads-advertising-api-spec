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
| partner_name   | URL parameter    | String     | 1aQ234Bc  | The name of integration side, which should represent business. Only digit and alphabet should be used  |
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
| Parameter name    | Value type         | Example                                                                                                             | Description                                                                                                                                                                                                                                                                                                                      |
|-------------------|--------------------|---------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| action_source     | String             | web, app, email, physical_store                                                                                     | Required. The value should be “web” for web conversion                                                                                                                                                                                                                                                                           |
| event_name        | String             | Purchase                                                                                                            | Required. The conversion event name, please follow SmartNews Standard Event name.                                                                                                                                                                                                                                                |
| event_value       | Float              | 500.0                                                                                                               | Optional. Can be the unit price of the item purchased. If `event_value` is also set in properties parameter, this `event_value` will overwrite the properties parameter `event_value`                                                                                                                                            |
| pixel_tag_id      | String             | 9112675ff25eb25f580feb82                                                                                            | Optional. Either pixel_tag_id or event_source_url need to be provided The Smartnews issued pixel tag id                                                                                                                                                                                                                          |
| event_source_url  | String             | http://www.smartnews.com?store=abc                                                                                  | Optional. Either pixel_tag_id or event_source_url need to be provided  The browser URL where the event happened. The URL must begin with http:// or https:// Please encode URL using GET method                                                                                                                                  |
| click_id          | String             | UnoPeo4IDmEwnHepAAEA                                                                                                | Optional. Either click_id or client_ip_address+client_user_agent must be provided The unique identifier for an ad request so that SN can accurately attribute conversion to click In order to use click_id, the advertisement landing page need to be configured properly (please refer to guide "Send Click ID within Request") |
| client_ip_address | String             | IPv4: 133.242.187.207 IPv6:  2001:ac8:40:12:5578:37e:193f:d5ea                                                      | Optional. Either click_id or client_ip_address+client_user_agent+os_version must be provided                                                                                                                                                                                                                                     |
| client_user_agent | String             | Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.141 Safari/537.36 | Optional. Either click_id or client_ip_address+client_user_agent+os_version must be provided                                                                                                                                                                                                                                     |
| client_os_version | String             |                                                                                                                     | Optional. Either click_id or client_ip_address+client_user_agent+os_version must be provided                                                                                                                                                                                                                                     |
| referrer          | String             | https://example.com/page?q=123                                                                                      | Optional.                                                                                                                                                                                                                                                                                                                        |
| event_time        | Long               | 1473668802                                                                                                          | Optional. The timestamp of event happens, accurate to the second                                                                                                                                                                                                                                                                 |
| properties        | Object (Key-value) | { "currency": "JPY",  "value": 5000, "content_ids": ['12345', '456789'], "content_type": "product" }                | Optional. This field is accepted only in POST requests. You may include additional conversion details—such as product price and quantity. For more information about this field, please see the following section.                                                                                                               |


### Parameters For App Conversions
| Parameter name    | Value type         | Example                                                                                                             | Description                                                                                                                                                                                                                                                                                                     |
|-------------------|--------------------|---------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| action_source     | String             | web, app, email, physical_store                                                                                     | Required. The value should be app for app conversion                                                                                                                                                                                                                                                            |
| event_name        | String             | Install                                                                                                             | Required. The conversion event name, please follow SmartNews Standard Event name.                                                                                                                                                                                                                               |
| event_value       | Float              | 500.0                                                                                                               | Optional. Can be the unit price of the item purchased. If `event_value` is also set in properties parameter, this `event_value` will overwrite the properties parameter `event_value`                                                                                                                           |
| store_id          | String             | iOS: 579581125 Android:jp.gocro.smartnews.android                                                                   | Required. iOS or Android App Store ID                                                                                                                                                                                                                                                                           |
| mobile_platform   | String             | iOS / Android                                                                                                       | Required. Mobile platform, either iOS or Android                                                                                                                                                                                                                                                                |
| click_id          | String             | UnoPeo4IDmEwnHepAAEA                                                                                                | Optional. Either click_id or ad_id or client_ip_address+client_user_agent must be provided The unique identifier for an ad request so that SN can accurately attribute conversion to click In order to use click_id, the advertisement landing page need to be configured properly (please refer to guide "Send Click ID within Request") |
| ad_id             | String             |                                                                                                                     | Optional. Either click_id or ad_id or client_ip_address+client_user_agent must be provided IDFA on iOS system AAID on Android system                                                                                                                                                                            |
| client_ip_address | String             | IPv4: 133.242.187.207 IPv6:  2001:ac8:40:12:5578:37e:193f:d5ea                                                      | Optional. Either click_id or ad_id or client_ip_address+client_user_agent must be provided                                                                                                                                                                                                                      |
| client_user_agent | String             | Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.141 Safari/537.36 | Optional. Either click_id or ad_id or client_ip_address+client_user_agent must be provided                                                                                                                                                                                                                      |
| client_os_version | String             |                                                                                                                     | Optional. OS version is required when click_id or ad_id cannot be provided and mobile platform is iOS                                                                                                                                                                                                           |
| event_time        | Long               | 1473668802                                                                                                          | Optional. The timestamp of event happens, accurate to the second                                                                                                                                                                                                                                                |
| properties        | Object (Key-value) | { "currency": "JPY",  "value": 5000, "content_ids": ['12345', '456789'], "content_type": "product" }                | Optional. This field is accepted only in POST requests. You may include additional conversion details—such as product price and quantity. For more information about this field, please see the following section.                                                                                              |


### Supported Events and Recommended Properties
**Special Notes for Dynamic Ads**

The four events — “ViewContent,” “AddToCart,” “Purchase,” and “Lead”—are used directly to optimize delivery (e.g., retargeting and product recommendations).
For Dynamic Ads, always pass the appropriate parameters with each of these events. Other events can be tracked, but they are not referenced for delivery optimization.

**Full List**

| Event name           |  Recommended Properties                                             | Event Description                                                                                 | 
| ---------------------| ------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- | 
| AddPaymentInfo       | content_ids, contents, currency, value                              | When payment information is added in the checkout flow. For example, a person clicks on a save billing information button. |  
| AddToCart            | content_ids, content_type, contents, currency, value                | When a product is added to the shopping cart. For example, a person clicks on an add to cart button. **`content_ids` are required for Dynamic Ads** | 
| AddToWishList        | content_ids, contents, currency, value                              | When a product is added to a wishlist. For example, a person clicks on an add to wishlist button. |  
| Booking              |                                                                     | When a reservation or booking is made for a service (hotel, flight, event ticket, etc.).          |
| CompleteRegistration | currency, value                                                     | When a registration form is completed. For example, a person submits a completed subscription or signup form. |  
| Contact              |                                                                     | When a person initiates contact with your business via telephone, SMS, email, chat, etc. For example, a person submits a question about a product. |  
| CustomizeProduct     |                                                                     | When a person customizes a product. For example, a person selects the color of a t-shirt.         |
| Donate               |                                                                     | When a person donates funds to your organization or cause. For example, a person adds a donation to the Humane Society to their cart. |  
| Download             |                                                                     | When a file, app, or other digital content is downloaded. For example, a person clicks a download button for a white paper. |  
| FindLocation         |                                                                     | When a person searches for a location of your store via a website or app, with an intention to visit the physical location. For example, a person wants to find a specific product in a local store. |
| InitiateCheckout     | content_ids, contents, currency, quantity, value                    | When a person enters the checkout flow prior to completing the checkout flow. For example, a person clicks on a checkout button. |  
| Lead                 | currency, value                                                     | When a sign-up is completed or other signal of interest is recorded. For example, a person clicks on pricing or submits a lead form. **`content_ids` are required for Dynamic Ads** | 
| Login                |                                                                     | When a person logs in to an existing account.                                                     |
| Purchase             | content_ids, content_type, contents, currency, quantity, value      | When a purchase is made or checkout flow is completed. For example, a person has finished the purchase or checkout flow and lands on thank-you or confirmation page. **`content_ids`, `currency`, and `value` are required for Dynamic Ads** |  
| Schedule             |                                                                     | When a person books an appointment to visit one of your locations. For example, a person selects a date and time for a tennis lesson. |
| Search               | content_ids, content_type, contents, currency, search_string, value | When a search is made. For example, a person searches for a product on your website. |  
| Share                |                                                                     | When a person shares content from your site or app via a share button or similar mechanism.       |
| SignUp               |                                                                     | When a person signs up for an account (pre-registration) but may not yet complete full registration. |
| StartTrial           | currency, predicted_ltv, value                                      | When a person starts a free trial of a product or service you offer. For example, a person selects a free week of your game. |  
| SubmitApplication    |                                                                     | When a person applies for a product, service, or program you offer. For example, a person applies for a credit card, educational program, or job. |
| SubmitForm           |                                                                     | When a person submits a form (e.g., contact form or survey) that is not covered by other events.  |
| Subscribe            | currency, predicted_ltv, value                                      | When a person applies to start a paid subscription for a product or service you offer. For example, a person subscribes to your streaming service. | 
| TimeSpent            |                                                                     | When the time a user spends on a page or screen is tracked and meets a threshold you define.      |
| ViewContent          | content_ids, content_type, contents, currency, value                | A visit to a web page you care about (for example, a product page or landing page). ViewContent tells you if someone visits a web page's URL, but not what they see or do on that page. For example, a person lands on a product details page. **content_ids are reuiqred for Dynamic Ads** | 
| VisitCart            |                                                                     | When a person views their shopping cart or basket page.                                           |


### Additional Events that are used for App
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


### Supported properties 
| Parameter name   | Value type                    | Example                                                             | Description                                              |
| ---------------- | ----------------------------- | ------------------------------------------------------------------- | ---------------------------------------------------------|
| content_category | String                        | electronics                                                         | Category of the page/product.                            |
| content_ids      | Array of Integers or String   | [12345, 'A67890']                                                   | Product IDs associated with the event, such as SKUs      |
| content_name     | String                        | special product A                                                   | Name of the page/product.                                |
| content_type     | String                        | product or product_group                                            | Either product or product_group based on the content_ids or contents being passed. If the IDs being passed in content_ids or contents parameter are IDs of products, then the value should be product. If product group IDs are being passed, then the value should be product_group.If no content_type is provided, SmartNews will match the event to every item that has the same ID, independent of its type. |
| contents         | Array of objects              | [{'id': 'ABC123', 'quantity': 2}, {'id': 'XYZ789', 'quantity': 2}]. | An array of JSON objects that contains the quantity and product or content identifier(s). id and quantity are the required fields. |
| currency         | String                        | JPY                                                                 | The currency for the value specified, like JPY, USD, EUR |
| quantity         | Integer                       | 2                                                                   | The number of items when checkout was initiated.         |
| predicted_ltv    | Integer, float                | 12000.5                                                             | Predicted lifetime value of a subscriber as defined by the advertiser and expressed as an exact value. |
| search_string    | String                        | wireless earbuds                                                    | Used with the Search event. The string entered by the user for the search. |
| status           | Boolean                       | true                                                                | Used with the CompleteRegistration event. Set true when the user completes full registration, and false when the user is still in a provisional state. |
| value            | Integer or float              | 39.99                                                               | The value of a user performing this event to the business. |
| time_spent       | Integer                       | 10000                                                               | Milli seconds of the user spend the time in the page |


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
    "currency": "JPY",
    "value": 5000,
    "content_ids": ['12346'],
    "content_type": "product"
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
    "currency": "JPY",
    "value": 5000,
    "content_ids": ['12345', '456789'],
    "content_type": "product"
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

**Success**
```http
HTTPS/1.1 200 OK
Content-Type: application/json
{
    "message": "OK",
    "request_id": "a326f711-1566-4002-9729-2846ae5107c8"
}
```

**Error**

**Case 1**: field missing

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

**Case 2**: invalid argument (such as unit_price is not a number)

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

**Case 3**: internal server error

```http
HTTPS/1.1 500 Internal Server Error
Content-Type: application/json
{
    "message": "Internal Server Error",
    "request_id": "a326f711-1566-4002-9729-2846ae5107c8"
}
```

## Batch Request [New since 2024/12/16]
The Batch Request feature allows advertisers to submit multiple conversions in a single request. All other supported parameters remain the same as described above. Note that only the POST method is supported for batch requests.

### Conversion URL
Production Endpoint: https://log.smartnews-ads.com/conversion_api/conversions/{api_version}/{partner_name}

Staging Endpoint (test only) https://stg-log.smartnews-ads.com/conversion_api/conversions/{api_version}/{partner_name}

### Example of batch request
```sh
curl --request POST 'https://log.smartnews-ads.com/conversion_api/conversions/v1/smartnews' \
--header 'Content-Type: application/json' \
--header 'Authorization: {authorization token issued by SN}' \
--data-raw '{
  "data": [
    {
      "action_source": "app",
      "event_name": "Purchase",
      "store_id": "jp.gocro.smartnews.android",
      "mobile_platform": "Android",
      "click_id": "UnoPeo4IDmEwnHepAAEA",
      "event_time": 1473668802,
      "properties": {
        "currency": "JPY",
        "value": 5000,
        "content_ids": ['12345', '456789'],
        "content_type": "product"
      }
    },
    {
      "action_source": "app",
      "event_name": "AddToCart",
      "store_id": "jp.gocro.smartnews.android",
      "mobile_platform": "Android",
      "click_id": "UnoPeo4IDmEwnHepAAEA",
      "event_time": 1473667802,
      "properties": {
        "currency": "JPY",
        "value": 5000,
        "content_ids": ['12346'],
        "content_type": "product"
      }
    },
    {
      "action_source": "app",
      "event_name": "AddToCart",
      "store_id": "579581125",
      "mobile_platform": "iOS",
      "click_id": "UnoFcd4UxewnHepAAEA",
      "event_time": 1473667802,
      "properties": {
        "currency": "JPY",
        "value": 5000,
        "content_ids": ['12348'],
        "content_type": "product"
      }
    }
  ]
}'
```

## Response
| Field      | Value type | Description                                                                                     |
|------------|------------|-------------------------------------------------------------------------------------------------|
| message    | String     | Indicate whether the request is successful or not, and what type of error happens               |
| request_id | String     | The unique id of the request                                                                    |
| error      | Object     | Optional“issue”: indicate what kind of error happens “detail”: the detailed reason of the error |


### Example Response
**Success**
```http
HTTPS/1.1 200 OK
Content-Type: application/json
{
    "message": "OK",
    "request_id": "a326f711-1566-4002-9729-2846ae5107c8"
}
```

**Error**

If there are multiple invalid events in the batch, the response will only include one error which is picked randomly. 

**Case 1**: field missing

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

**Case 2**: invalid argument (such as unit_price is not a number)

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

**Case 3**: internal server error

```http
HTTPS/1.1 500 Internal Server Error
Content-Type: application/json
{
    "message": "Internal Server Error",
    "request_id": "a326f711-1566-4002-9729-2846ae5107c8"
}
```


- You can send up to 1,000 conversion events in the data field. However, for optimal performance, it’s recommended to send events as soon as they occur, ideally within one hour of the event.
- Important: If any invalid events are included in the batch, the entire batch will be rejected.