---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - java	
  - python
  - javascript

toc_footers:
  - <a href='https://app.cybermdcare.com/api/index.do'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true
---

# Introduction

Welcome to the CyberMDCare API! You can use our API to access CyberMDCare API endpoints, which can make an appointment and get the information of appointment and cancel an appointment from our database.

We have language bindings in Shell. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Authentication

CyberMDCare uses the one-time-access-token to allow access to the API. You can get an one-time-access-token by API key. You can register a new CyberMDCare API key at our [developer portal](https://app.cybermdcare.com/api/index.do).

## Getting an One-time Access token

> To get one-time Access token, use this code:

```java
OkHttpClient client = new OkHttpClient().newBuilder()
								.build();
Request request = new Request.Builder()
								.url("https://app.cybermdcare.com/api/token?apiKey=[apiKey]")
								.method("GET", null)
								.build();
Response response = client.newCall(request).execute();
```

```python
import requests

url = "https://app.cybermdcare.com/api/token?apiKey=[apiKey]"
payload={}
files={}
headers = {}

response = requests.request("GET", url, headers=headers, data=payload, files=files)

print(response.text)
```

```shell
# With shell, you can just pass the correct header with each request
curl "https://app.cybermdcare.com/api/token?apiKey=[apiKey]" 
```	

```javascript
var settings = {
  "url": "https://app.cybermdcare.com/api/token?apiKey=[apiKey]",
  "method": "GET"
};

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> Make sure to replace `[apiKey]` with your API key.

A One-time access token can only be used once per API request. 

<aside class="warning">Once used, the one-time access token cannot be reused. 
<br>
Please re-issue a one-time access token to use another API request.
</aside>

### HTTP Request

`GET https://app.cybermdcare.com/api/token`

### Query Parameters

Parameter | Required | Description
--------- | ------- | -----------
apiKey | true | The ApiKey that CyberMDCare issued.


### HTTP Response 

Key | Description
--------- | -----------
response_cd |  response code.
message | server message. if errors, show the reason.
token | one-time access token.

CyberMDCare expects for the one-time Access-token to be included in all API requests to the server in a header that looks like the following:

`token: [Access-token]`

<aside class="notice">
You must replace <code>[Access-token]</code> with your personal Access-token.
</aside>

# Appointment

## Make an appointment


```java
OkHttpClient client = new OkHttpClient().newBuilder()
  								.build();
MediaType mediaType = MediaType.parse("application/json");
RequestBody body = RequestBody.create(mediaType, "{\r\n    \"npiNo\":\"1234567890\",\r\n    \"patNo\":\"1234\",\r\n    \"patNm\":\"ABCD\",\r\n    \"gender\":\"F\",\r\n    \"dob\":\"12/12/2020\"\r\n}");
Request request = new Request.Builder()
							  .url("https://app.cybermdcare.com/api/appot")
							  .method("POST", body)
							  .addHeader("Content-Type", "application/json")
							  .addHeader("token", "[Access-token]")
							  .build();
Response response = client.newCall(request).execute();
```

```python
import requests
import json

url = "https://app.cybermdcare.com/api/appot"
payload = json.dumps({
  "npiNo": "1234567890",
  "patNo": "1234",
  "patNm": "ABCD",
  "gender": "F",
  "dob": "12/12/2020"
})
headers = {
  'token': '[Access-token]',
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data=payload)

print(response.text)
```

```shell


curl -X POST "https://app.cybermdcare.com/api/appot" \
-H 'Content-Type: application/json'  \
-H 'token: [Access-token]' \	
--data-raw "{
    \"npiNo\":\"1234567890\",
    \"patNo\":\"1234\",
    \"patNm\":\"ABCD\",
    \"gender\":\"F\",
    \"dob\":\"12/12/2020\"
}"

```

```javascript
var settings = {
  "url": "https://app.cybermdcare.com/api/appot",
  "method": "POST",
  "headers": {
    "token": "[Access-token]",
    "Content-Type": "application/json"
  },
  "data": JSON.stringify({
    "npiNo": "1234567890",
    "patNo": "1234",
    "patNm": "ABCD",
    "gender": "F",
    "dob": "12/12/2020"
  }),
};

$.ajax(settings).done(function (response) {
  console.log(response);
});
```
> Make sure to replace `[Access-token]` with your Access-token.
> The above command returns JSON structured like this:

```json
{
    "response_cd": "200",
    "message": "ok",
    "appot_dt": "05-30-2021 15:52",
    "signup_url": null,
    "url": "https://app.cybermdcare.com/api/index.do",
    "appot_no": "12345678"
}
```

This endpoint makes telemedicine appointments.

### HTTP Request

`POST https://app.cybermdcare.com/api/appot`

### JSON Body 

Key | Required | Description
--------- | ------- | -----------
npiNo | true | Valid NPI number
state | false | State in which the doctor's NPI number is valid. if empty, default value is 'CA'.
patNo | false | Patient number managed by RPM. If the patient number is stored in our system, you can make an appointment with the npi number and patient number.
patNm | true | Patient name. If new patient, the value is required.
gender | true | Patient gender. choose 'M'(male) or 'F'(female). If new patient, the value is required.
dob | true | Patient date of birth. If new patient, the value is required. format : 12/31/1989. 


### HTTP Response 

Key | Description
--------- | -----------
response_cd |  response code.
message | server message. if errors, show the reason.
appot_dt | appointment date
appot_no | appointment number. You can use this number to get reservation information through other API endpoints.
signup_url | the url that get an account when new NPI number. it shows once only.
url | the url that connect to telemedicine page.


<aside class="success">
Remember — The API makes a request to the server with a one-time access token as a header!
</aside>

## Get appointment information

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  								.build();
Request request = new Request.Builder()
								.url("https://app.cybermdcare.com/api/appot/12345678")
								.method("GET", null)
								.addHeader("token", "[Access-token]")
								.build();
Response response = client.newCall(request).execute();
```

```python
import requests

url = "https://app.cybermdcare.com/api/appot/12345678"
payload={}
files={}
headers = {
  'token': '[Access-token]'
}

response = requests.request("GET", url, headers=headers, data=payload, files=files)

print(response.text)
```

```shell
curl "https://app.cybermdcare.com/api/appot/12345678" \
  -H 'token: [Access-token]'
```

```javascript
var settings = {
  "url": "https://app.cybermdcare.com/api/appot/12345678",
  "method": "GET",
  "headers": {
    "token": "[Access-token]"
  },
};

$.ajax(settings).done(function (response) {
  console.log(response);
});
```
> Make sure to replace `[Access-token]` with your Access-token.
> The above command returns JSON structured like this:

```json
{
    "response_cd": "200",
    "pt_nm": "ABCD ",
    "appot_status": "active",
    "appot_dt": "05-22-2021 16:35",
    "message": "",
    "appot_no": "12345678"
}
```

This endpoint retrieves information appointments.


### HTTP Request

`GET https://app.cybermdcare.com/api/appot/<Appot_No>`

### URL Parameters

Parameter | Description
--------- | -----------
Appot_No | The number of the appointment

### HTTP Response 

Key | Description
--------- | -----------
response_cd |  response code.
message | server message. if errors, show the reason.
appot_dt | appointment date
appot_no | appointment number. You can use this number to get reservation information through other API endpoints.
pt_nm | the name of patient.
appot_status | the status of appointment.

<aside class="success">
Remember — The API makes a request to the server with a one-time access token as a header!
</aside>


## Cancel an appointment

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  								.build();
MediaType mediaType = MediaType.parse("text/plain");
RequestBody body = new MultipartBody.Builder().setType(MultipartBody.FORM)
  								.build();
Request request = new Request.Builder()
								.url("https://app.cybermdcare.com/api/appot/12345678")
								.method("DELETE", body)
								.addHeader("token", "[Access-token]")
								.build();
Response response = client.newCall(request).execute();
```

```python
import requests

url = "https://app.cybermdcare.com/api/appot/12345678"
payload={}
files={}
headers = {
  'token': '[Access-token]'
}

response = requests.request("DELETE", url, headers=headers, data=payload, files=files)

print(response.text)
```

```shell
curl "https://api.cybermdcare.com/api/appot/12345678" \
  -X DELETE \
  -H 'token: [Access-token]'
```

```javascript
var settings = {
  "url": "https://app.cybermdcare.com/api/appot/12345678",
  "method": "DELETE",
  "headers": {
    "token": "[Access-token]"
  }
};

$.ajax(settings).done(function (response) {
  console.log(response);
});
```

> Make sure to replace `[Access-token]` with your Access-token.
> The above command returns JSON structured like this:

```json
{
    "response_cd": "200",
    "message": "The appointment has been cancelled. ; 12345678"
}
```

This endpoint cancels an appointment.

### HTTP Request

`DELETE https://app.cybermdcare.com/api/appot/<Appot_No>`

### URL Parameters

Parameter | Description
--------- | -----------
Appot_No | The number of the appointment

### HTTP Response 

Key | Description
--------- | -----------
response_cd |  response code.
message | server response message.

<aside class="success">
Remember — The API makes a request to the server with a one-time access token as a header!
</aside>

