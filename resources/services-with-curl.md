# Using tyntecs services with cURL

## Basic setup

All services mentioned are using https://api.tyntec.com as base URL and
require an api key in the http header apikey.

The api token is provided by us.

## SMS

### Send

curl -X POST \
  https://api.tyntec.com/messaging/sms/v1/ \
  -H 'Content-Type: application/json' \
  -H 'apikey: <API KEY>' \
  -d '{
	"from":"FTHackathon",
	"to":"<receiver>",
	"message":"Hi from the female tech leader hackathon"
}'

The result will be like

{
    "requestId": "3710-dfb35da3-96cc-4db9-a8d1-248c3153fa9c",
    "size": 1,
    "msgIdList": [
        "3710-1539839888436+4917624435312"
    ]
}

### Get Status

curl -X GET \
  https://api.tyntec.com/messaging/sms/v1/status/<requestId> \
  -H 'apikey: <API KEY>'

The result will be like

{
    "requestId": "3710-dfb35da3-96cc-4db9-a8d1-248c3153fa9c",
    "overallState": "DELIVERED",
    "size": 1,
    "userContext": "",
    "mccmnc": "26207",
    "ttid": "16",
    "contentList": [
        {
            "deliveryState": "DELIVERED",
            "statusText": "",
            "msgId": "3710-1539839888436+4917624435312",
            "errorCode": "0000",
            "sentDate": "2018-10-18T05:18:08+00:00",
            "doneDate": "2018-10-18T05:18:15+00:00"
        }
    ],
    "from": "FTHackathon",
    "to": "+4917624435312"
}

### Send a Mobile Chat message (Facebook or Viber)

curl -X POST \
  https://api.tyntec.com/messaging/mobile-chat/v1/ \
  -H 'Content-Type: application/json' \
  -H 'apikey: <API KEY>' \
  -d '{
	"from":"FTHackathon",
	"to":"<receiver>",
	"message":"via ott 5"
}'

The result will be like

{
    "requestId": "eb15e820-9978-4afe-8c6b-b4d92ad9d2e5",
    "errorText": ""
}

### Check the status

curl -X GET \
  https://api.tyntec.com/messaging/mobile-chat/v1/<requestId>/status \
  -H 'apikey: <API KEY>'

The result will be like

{
    "requestId": "eb15e820-9978-4afe-8c6b-b4d92ad9d2e5",
    "size": 1,
    "userContext": null,
    "overallState": "DELIVERED",
    "from": "FTHackathon",
    "to": "+4917624435312",
    "deliveryChannel": {
        "channel": "SMS",
        "mccmnc": "26207",
        "ttid": "16"
    },
    "sendParts": [
        {
            "messagePartId": 1,
            "sentDate": 1539840890820,
            "contentExcerpt": "0076006900610020006f",
            "deliveryState": "DELIVERED",
            "deliveryStateDate": 1539840896844,
            "errorCode": "0x00",
            "price": null,
            "currency": null,
            "priceEffective": null
        }
    ]
}

## One time password

### Send

curl -X POST \
  'https://api.tyntec.com/2fa/v1/otp?number=<receiver>' \
  -H 'apikey: <API KEY>'

The result will be like

{
    "accountId": "hackathoncpaas",
    "applicationId": "17113d73-ab56-3c0f-af3d-2b8d22b74d26",
    "otpId": "af145b90-c68b-4042-9e7a-1c3f866f2161",
    "number": "+4917624435312",
    "attemptCount": 0,
    "otpStatus": "ACTIVE",
    "referenceId": null,
    "expire": 1539841568254,
    "created": 1539841268254,
    "timestampCreated": "2018-10-18T05:41:08.254Z",
    "timestampExpire": "2018-10-18T05:46:08.254Z"
}

### Check the status

curl -X GET \
  https://api.tyntec.com/2fa/v1/otp/<otpId> \
  -H 'apikey: <API KEY>'

The result will be like

{
    "accountId": "hackathoncpaas",
    "applicationId": "17113d73-ab56-3c0f-af3d-2b8d22b74d26",
    "otpId": "af145b90-c68b-4042-9e7a-1c3f866f2161",
    "number": "+4917624435312",
    "attemptCount": 0,
    "otpStatus": "ACTIVE",
    "referenceId": null,
    "expire": 1539841568254,
    "created": 1539841268254,
    "timestampCreated": "2018-10-18T05:41:08.254Z",
    "timestampExpire": "2018-10-18T05:46:08.254Z"
}

### Resend the code

curl -X POST \
  'https://api.tyntec.com/2fa/v1/otp/<otpId>?via=VOICE' \
  -H 'apikey: <API KEY>'

The result will be like

{
    "accountId": "hackathoncpaas",
    "applicationId": "17113d73-ab56-3c0f-af3d-2b8d22b74d26",
    "otpId": "af145b90-c68b-4042-9e7a-1c3f866f2161",
    "number": "+4917624435312",
    "attemptCount": 0,
    "otpStatus": "ACTIVE",
    "referenceId": null,
    "expire": 1539841568254,
    "created": 1539841268254,
    "timestampCreated": "2018-10-18T05:41:08.254Z",
    "timestampExpire": "2018-10-18T05:46:08.254Z"
}

### Verify the code

curl -X POST \
  'https://api.tyntec.com/2fa/v1/otp/<otpId>/check?otpCode=<otpCode>' \
  -H 'apikey: <API KEY>'

The result will be like

{
    "accountId": "hackathoncpaas",
    "applicationId": "17113d73-ab56-3c0f-af3d-2b8d22b74d26",
    "otpId": "af145b90-c68b-4042-9e7a-1c3f866f2161",
    "number": "+4917624435312",
    "attemptCount": 1,
    "otpStatus": "VERIFIED",
    "referenceId": null,
    "expire": 1539841568254,
    "created": 1539841268254,
    "timestampCreated": "2018-10-18T05:41:08.254Z",
    "timestampExpire": "2018-10-18T05:46:08.254Z"
}

### Delete the code

curl -X DELETE \
  https://api.tyntec.com/2fa/v1/otp/<otpId> \
  -H 'apikey: <API KEY>'

The result will be like

  {
      "accountId": "hackathoncpaas",
      "applicationId": "17113d73-ab56-3c0f-af3d-2b8d22b74d26",
      "otpId": "af145b90-c68b-4042-9e7a-1c3f866f2161",
      "number": "+4917624435312",
      "attemptCount": 1,
      "otpStatus": "VERIFIED",
      "referenceId": null,
      "expire": 1539841568254,
      "created": 1539841268254,
      "timestampCreated": "2018-10-18T05:41:08.254Z",
      "timestampExpire": "2018-10-18T05:46:08.254Z"
  }
