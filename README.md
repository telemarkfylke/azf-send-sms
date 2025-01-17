[![Build Status](https://travis-ci.com/vtfk/azf-send-sms.svg?branch=master)](https://travis-ci.com/vtfk/azf-send-sms)
[![js-standard-style](https://img.shields.io/badge/code%20style-standard-brightgreen.svg?style=flat)](https://github.com/feross/standard)

# azf-send-sms

HttpTriggered sending of SMS with [PSWinCom/LinkMobility](https://pswin.com/)

# Usage

POST json to function.


```javascript
{
  "receivers": ["4798765432", "4745678912"], // Required (10 digits) - must have country codes prefixed!
  "message": "<message>", // Required
  "sender": "<name-or-number>", // Optional
  "operation": 9 // Optional
}
```

```
$ curl https://azf-send-sms.azurewebsites.net/ -d "{ "receivers": ["4745678912"], "message": "Do you read me?" }" -H "Content-Type: application/json" -v
```

## Deploy

### Azure

You"ll need a valid subscription and have the following resources setup:
- resource group
- app service plan
- storage account


#### Setup function

The easiest way to make this function run is to setup an app service, configure the app and get the function from GitHub.

- add function app
  - Runtime stack -> Node

Configuration for app (Application settings):
```javascript
{
  "PSWIN_USERNAME": "username",
  "PSWIN_PASSWORD": "password",
  "DEFAULT_SENDER": "TelemarkFK"
}
```

- add function
  - Plattform features -> deployment center
  - github
  - branch master

## Development

1. First - install all tools needed for [local development](https://docs.microsoft.com/en-us/azure/azure-functions/functions-develop-local).
2. Clone the repo. Install dependencies.
3. Create a local.settings.json file:
    ```javascript
    {
      "IsEncrypted": false,
      "Values": {
        "FUNCTIONS_WORKER_RUNTIME": "node",
        "PSWIN_USERNAME": "username",
        "PSWIN_PASSWORD": "password",
        "DEFAULT_SENDER": "VTFK"
      }
    }
    ```

4. Start server:
    ```
    $ func start
    ```

POST testdata

```
$ curl http://localhost:7071/api/SendSMS -d "{ "receivers": ["+4745678912"], "message": "Do you read me?" }" -H "Content-Type: application/json" -v
```

# License

[MIT](LICENSE)