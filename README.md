# Telstra Event Detection API
## Introduction
The TED API provides the ability to subscribe to and receive network events for a given set of mobile numbers.
The first step to use beta TED API is to create a developer account on beta dev portal and request for access to the API. To request for an API key contact us at [t.dev@team.telstra.com](t.dev@team.telstra.com)

## Beta release features
For this release we've included following features

host\_url: https://slot2.org005.t-dev.telstra.net <br>
oauth\_path: [/v1/oauth/token](https://slot2.org005.t-dev.telstra.net/v1/oauth/token) <br>
subscription\_path: [/v2/messages/provisioning/subscriptions](https://slot2.org005.t-dev.telstra.net/v2/messages/provisioning/subscriptions) <br>
get\_event: [/v1/eventdetection/events/simswap](https://slot2.org005.t-dev.telstra.net/v1/eventdetection/events/simswap) <br>


| Feature | Description |
| --- | --- |
| `Event Subscription` | Subscription informs the API to start listening for particular event. Current events are sim swap, port out(or number de-activation), international roaming. <br> Sim Swap is the only event available during beta release  |
| `Event Request` | Request for event history for a given list of mobile numbers <br> Event history is maintained for 90 days|
| `Push notification` | Provision a callback URL to get push notification as soon as the event occurs. <br> Event history is maintained for 90 days |
| `Documentation` | Hit Get Started to access technical documentation and get started within minutes |

## Getting access to the API
To get access to the API, go to MyApps page and create a new application with `Add New App` button. Creation of an app will trigger a request for access to API key.

## Authentication
To get an OAuth 2.0 Authentication token, pass through your Consumer Key and Consumer Secret that you received when you registered for the Messages API key. The `grant_type` should be left as `client_credentials` and the scope as `NSMS`. The token will expire in one hour. Get your keys by registering at our [Developer Portal](https://test-apac-demo3.devportal.apigee.com/).

```sh
#!/bin/bash
# Obtain these keys from the Telstra Developer Portal
CONSUMER_KEY="your consumer key"
CONSUMER_SECRET="your consumer secret"
curl -X POST -H 'Content-Type: application/x-www-form-urlencoded' \
  -d 'grant_type=client_credentials&client_id=$CONSUMER_KEY&client_secret=CONSUMER_SECRET&scope=NSMS' \
  'https://slot2.apipractice.t-dev.telstra.net/v1/oauth/token'
```
### Response
```json
{
  "expires_in" : "35999",
  "access_token" : "1234567890123456788901234567"
}
```

## Create subscription
Invoke subscription API to create one or more subscriptions

```sh
#!/bin/bash
curl -X POST \
  https://slot2.apipractice.t-dev.telstra.net/v2/messages/provisioning/subscriptions \
  -H 'authorization: Bearer $TOKEN' \
  -H 'cache-control: no-cache' \
  -H 'content-type: application/json' \
  -d '{
        "phoneNumbers": [
          "61412345678"
          ]
      }'
```
Parameters for the subscription API are;

| Parameter |Mandatory| Description |
| --- | --- | --- |
| `phoneNumbers` | Y | List of numbers to create a subscription for |
| `notifyURL` | N | A callback URL that will be POSTed to whenever a new message arrives at this destination address. If this is not provided then you can make use the Get Replies API to poll for messages |
### Response
A typical response will look like

```
{
    "todo": "Umesh to provide information"
}
```
The fields mean;

| Field | Description |
| --- | --- |
| `destinationAddress` | The provisioned number assigned to your app. Your customers can send replies to this number. If the `notifyURL` is provided then the replies will be POSTed to it. |

## Get Event (sim swap)
To get sim swap event invoke following API

```sh
#!/bin/bash
# Use the TED API to get events
AccessToken="Consumers Access Token"
Dest="Destination number"
curl -X post -H "Authorization: Bearer $AccessToken" \
  -H "Content-Type: application/json" \
  -d '{
      "event": "event-name"
      "phoneNumbers": [
        "61467754783"
        ]
      }' \
      https://slot2.org005.t-dev.telstra.net/v1/eventdetection/events/simswap
```
A number of parameters can be used in this call, these are;

| Parameter | Description |
| --- | --- |
| `event` | Name of event for which information is requested |
| `phoneNumbers` | List of numbers for information is requested |

### Response
A typical response will look like;

```json
{
    "todo": "Umesh to add this"
}
```
The fields mean;

| Field | Description |
| --- | --- |
| `todo` | Umesh to add this |


## Sample Apps
On the way

## SDKs
On the way
