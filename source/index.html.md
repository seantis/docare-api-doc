---
title: docare.ch API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://www.docare.ch'>docare.ch</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

search: true
---

# Introduction

Welcome to the docare.ch API! You can use our API to access docare.ch [FHIR](http://hl7.org/fhir/) API endpoints, which can get information on patients and encounters in our database.

# Authentication

> To get an access token, send the following request:

```shell
curl -X POST https://portal.docare.ch/oauth/v2/token
  -d "client_id=clientid"
  -d "client_secret=clientsecret"
  -d "grant_type=client_credentials"
```

> Make sure to replace `clientid` and `clientsecret` with your OAuth client id and secret.

> The above command returns JSON structured like this:

```json
{
  "token_type": "Bearer",
  "access_token": "add72ae475214adc83ea227c21fee0e5",
  "expires_in": 3600
}
```

docare.ch uses a token to allow access to the API. An OAuth client credentials request is used to obtain a token. [Contact us](https://www.docare.ch/api/) to get your OAuth `client id` and `client secret`. The token can be used for the number of sconds returned in the `expires_in` field.

docare.ch expects the access token to be included in all API requests to the server in a header that looks like the following:

`Authorization: Bearer 94b760b2dff748f992dc8e52e9a5bd51`

<aside class="notice">
You must replace <code>94b760b2dff748f992dc8e52e9a5bd51</code> with the access token you received from the OAuth request.
</aside>

# Patient

The docare.ch FHIR [Patient](http://hl7.org/fhir/patient.html) resource covers demographics and other administrative information about a patient.

## Get All Patients

> To get all patients with the string `Mulligan` in the name, send the following request:

```shell
curl -X GET "https://portal.docare.ch/fhir/v3/Patient?name=Mulligan"
  -H "Authorization: Bearer add72ae475214adc83ea227c21fee0e5"
```

> The above command returns JSON structured like this:

```json
{
  "resourceType": "Bundle",
  "type": "searchset",
  "total": 1,
  "entry": [
    {
      "fullUrl": "https://portal.docare.ch/fhir/v3/Patient/2f5da8c2-cbf1-42d1-9d7a-165f3ed80541",
      "resource": {
        "resourceType": "Patient",
        "id": "2f5da8c2-cbf1-42d1-9d7a-165f3ed80541",
        "name": [{
          "use": "usual",
          "text": "Buck Mulligan",
          "family": "Mulligan",
          "given": ["Buck"]
        }],
        "birthDate": "2017-03-05",
        "gender": "male"
      }
    }
  ]
}
```

This endpoint retrieves all patients.

### HTTP Request

`GET https://portal.docare.ch/fhir/v3/Patient`

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
name | [string](http://hl7.org/fhir/search.html#string) | A portion of the family or given name of the patient.

## Get a Specific Patient

> To get the patient with id `2f5da8c2-cbf1-42d1-9d7a-165f3ed80541`, send the following request:

```shell
curl -X GET "https://portal.docare.ch/fhir/v3/Patient/2f5da8c2-cbf1-42d1-9d7a-165f3ed80541"
  -H "Authorization: Bearer add72ae475214adc83ea227c21fee0e5"
```

> The above command returns JSON structured like this:

```json
{
  "resourceType": "Patient",
  "id": "2f5da8c2-cbf1-42d1-9d7a-165f3ed80541",
  "name": [{
    "use": "usual",
    "text": "Buck Mulligan",
    "family": "Mulligan",
    "given": ["Buck"]
  }],
  "telecom": [{
    "system": "phone",
    "use": "mobile",
    "value": "+41790000000"
  }],
  "birthDate": "2017-03-05",
  "gender": "male"
}
```

This endpoint retrieves a specific patient.

### HTTP Request

`GET https://portal.docare.ch/fhir/v3/Patient/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the patient to retrieve

# Encounter

The docare.ch FHIR [Encounter](http://hl7.org/fhir/encounter.html) resource covers a consultation recorded in docare.ch.

## Get All Encounters

> To get all encounters for the patient with id `2f5da8c2-cbf1-42d1-9d7a-165f3ed80541`, send the following request:

```shell
curl -X GET "https://portal.docare.ch/fhir/v3/Encounter?subject=https://portal.docare.ch/fhir/v3/Patient/2f5da8c2-cbf1-42d1-9d7a-165f3ed80541"
  -H "Authorization: Bearer add72ae475214adc83ea227c21fee0e5"
```

> The above command returns JSON structured like this:

```json
{
  "resourceType": "Bundle",
  "type": "searchset",
  "total": 1,
  "entry": [
    {
      "fullUrl": "https://portal.docare.ch/fhir/v3/Encounter/19d4c5a3-fa8d-4aa0-aa11-f9a1f31656d8",
      "resource": {
        "resourceType": "Encounter",
        "id": "19d4c5a3-fa8d-4aa0-aa11-f9a1f31656d8",
        "class": {
          "code": "ambulatory"
        },
        "subject": {
          "reference": "https://portal.docare.ch/fhir/v3/Patient/2f5da8c2-cbf1-42d1-9d7a-165f3ed80541"
        },
        "period": {
          "start": "2018-11-17",
          "end": "2018-11-17"
        }
      }
    }
  ]
}
```


This endpoint retrieves all encounters.

### HTTP Request

`GET https://portal.docare.ch/fhir/v3/Encounter`

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
subject | [reference](https://www.hl7.org/FHIR/search.html#reference) | The patient or group present at the encounter.


## Get a Specific Encounter

> To get the encounter with id `19d4c5a3-fa8d-4aa0-aa11-f9a1f31656d8`, send the following request:

```shell
curl -X GET "https://portal.docare.ch/fhir/v3/Encounter/19d4c5a3-fa8d-4aa0-aa11-f9a1f31656d8"
  -H "Authorization: Bearer add72ae475214adc83ea227c21fee0e5"
```

> The above command returns JSON structured like this:

```json
{
  "resourceType": "Encounter",
  "id": "19d4c5a3-fa8d-4aa0-aa11-f9a1f31656d8",
  "class": {
    "code": "ambulatory"
  },
  "subject": {
    "reference": "https://portal.docare.ch/fhir/v3/Patient/2f5da8c2-cbf1-42d1-9d7a-165f3ed80541"
  },
  "period": {
    "start": "2018-11-17",
    "end": "2018-11-17"
  }
}
```

This endpoint retrieves a specific encounter.

### HTTP Request

`GET https://portal.docare.ch/fhir/v3/Encounter/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the encounter to retrieve


# Errors

```shell
curl -X GET "https://portal.docare.ch/fhir/v3/Patient/invalid_id"
  -H "Authorization: Bearer add72ae475214adc83ea227c21fee0e5"
```

> The above command returns a 404 response containing JSON structured like this:

```json
{
  "error": "not_found"
}
```

The docare.ch API uses the following standard HTTP error codes.
The JSON response contains an addititonal `error` key.

Code | Error | Meaning
---- | ----- | -------
400 | validation_failed | Invalid body/arguments or schema validation failed
400 | resource_not_found | FHIR referenced resource not found
401 | unauthorized | Authorization header missing, Bearer token invalid or expired
401 | invalid_client | Invalid OAuth client_id or client_secret
401 | unsupported_grant_type | Unsupported OAuth grant_type
401 | invalid_grant | Invalid/expired OAuth authorization code
403 | forbidden | Required permission for resource missing
404 | not_found | Resource not found (invalid URL)
405 | method_not_allowed | HTTP method not allowed for URL
5xx | Server Error |

Responses with a 5xx error code contain HTML instead of JSON content.
