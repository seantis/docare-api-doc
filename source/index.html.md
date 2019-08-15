---
title: docare.ch API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - http

toc_footers:
  - <a href='https://www.docare.ch'>docare.ch</a>

search: true
---

# Introduction

Welcome to the docare.ch API! You can use our API to access docare.ch [FHIR](http://hl7.org/fhir/) API endpoints, which can get information on patients and encounters in our database.

# Authentication

> To get an access token, send the following request:

```http
POST /oauth/v2/token HTTP/1.1
Content-Type: multipart/form-data
Host: https://portal.docare.ch

client_id: "clientid"
client_secret: "clientsecret"
grant_type: "client_credentials"
```

> Make sure to replace `clientid` and `clientsecret` with your OAuth client id and secret.

> The above request returns the following response:

```http
HTTP/1.1 200 OK
Content-Type: application/json

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


## Create a Patient

```http
POST /fhir/v4/Patient HTTP/1.1
Authorization: Bearer add72ae475214adc83ea227c21fee0e5
Content-Type: application/json
Host: https://portal.docare.ch

{
  "resourceType": "Patient",
  "name": [{
    "use": "usual",
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

> The above request returns the following response:

```http
HTTP/1.1 201 Created
Content-Type: application/json
Location: https://portal.docare.ch/fhir/v4/Patient/5b9613fc-43ef-4f90-bf10-9cbe7451fe02

{
  "resourceType": "Patient",
  "id": "5b9613fc-43ef-4f90-bf10-9cbe7451fe02",
  "identifier": [{
      "system": "https://portal.docare.ch/concepts/patient_number",
      "value": "103"
  }],
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

This endpoint creates a new patient.

### HTTP Request

`POST /fhir/v4/Patient`

### JSON Attributes

Name | Type | Description
--------- | ------- | -----------
resourceType | [string](http://hl7.org/fhir/datatypes.html#string) | "Patient" constant
name | [HumanName](http://hl7.org/fhir/datatypes.html#HumanName) | Name associated with the patient
telecom | [ContactPoint](http://hl7.org/fhir/datatypes.html#ContactPoint) | Contact detail for the patient
birthDate | [date](http://hl7.org/fhir/datatypes.html#date) | The date of birth for the patient
gender | [code](http://hl7.org/fhir/datatypes.html#code) | Gender of the patient: male, female

<aside class="notice">
The JSON structure must contain all attributes. Only one name is supported. One contact detail of the type system=phone and use=mobile is supported.
</aside>


## Get All Patients

> To get all patients with the string `Mulligan` in the name, send the following request:

```http
GET /fhir/v4/Patient?name=Mulligan HTTP/1.1
Authorization: Bearer add72ae475214adc83ea227c21fee0e5
Host: https://portal.docare.ch
```

> The above request returns the following response:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "resourceType": "Bundle",
  "type": "searchset",
  "total": 1,
  "entry": [
    {
      "fullUrl": "https://portal.docare.ch/fhir/v4/Patient/2f5da8c2-cbf1-42d1-9d7a-165f3ed80541",
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

`GET /fhir/v4/Patient`

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
name | [string](http://hl7.org/fhir/datatypes.html#string) | A portion of the family or given name of the patient.


## Get a Specific Patient

> To get the patient with id `2f5da8c2-cbf1-42d1-9d7a-165f3ed80541`, send the following request:

```http
GET /fhir/v4/Patient/2f5da8c2-cbf1-42d1-9d7a-165f3ed80541 HTTP/1.1
Authorization: Bearer add72ae475214adc83ea227c21fee0e5
Host: https://portal.docare.ch
```

> The above request returns the following response:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "resourceType": "Patient",
  "id": "2f5da8c2-cbf1-42d1-9d7a-165f3ed80541",
  "identifier": [{
      "system": "https://portal.docare.ch/concepts/patient_number",
      "value": "103"
  }],
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

`GET /fhir/v4/Patient/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the patient to retrieve


## Update a Patient

```http
POST /fhir/v4/Patient/5b9613fc-43ef-4f90-bf10-9cbe7451fe02 HTTP/1.1
Authorization: Bearer add72ae475214adc83ea227c21fee0e5
Content-Type: application/json
Host: https://portal.docare.ch

{
  "resourceType": "Patient",
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

> The above request returns the following response:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "resourceType": "Patient",
  "id": "5b9613fc-43ef-4f90-bf10-9cbe7451fe02",
  "identifier": [{
      "system": "https://portal.docare.ch/concepts/patient_number",
      "value": "103"
  }],
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

This endpoint updates all patient data.

### HTTP Request

`POST /fhir/v4/Patient/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the patient to update

### JSON Attributes

Name | Type | Description
--------- | ------- | -----------
resourceType | [string](http://hl7.org/fhir/datatypes.html#string) | "Patient" constant
name | [HumanName](http://hl7.org/fhir/datatypes.html#HumanName) | Name associated with the patient
telecom | [ContactPoint](http://hl7.org/fhir/datatypes.html#ContactPoint) | Contact detail for the patient
birthDate | [date](http://hl7.org/fhir/datatypes.html#date) | The date of birth for the patient
gender | [code](http://hl7.org/fhir/datatypes.html#code) | Gender of the patient: male, female

<aside class="notice">
The JSON structure must contain all attributes (incl. the ones that don't change). Missing attributes are treated as empty. Only one name is supported. One contact detail of the type system=phone and use=mobile is supported.
</aside>


## Delete a Patient

> To delete the patient with id `5b9613fc-43ef-4f90-bf10-9cbe7451fe02`, send the following request:

```http
DELETE /fhir/v4/Patient/5b9613fc-43ef-4f90-bf10-9cbe7451fe02 HTTP/1.1
Authorization: Bearer add72ae475214adc83ea227c21fee0e5
Host: https://portal.docare.ch
```

> The above request returns the following response:

```http
HTTP/1.1 204 No Content
```

This endpoint deletes a patient.

### HTTP Request

`DELETE /fhir/v4/Patient/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the patient to delete


# Encounter

The docare.ch FHIR [Encounter](http://hl7.org/fhir/encounter.html) resource covers a consultation recorded in docare.ch.


## Create an Encounter

> To create a new encounter for the patient with id `d0a31764-6030-4284-984e-3bd967106ea4`, send the following request:

```http
POST /fhir/v4/Encounter HTTP/1.1
Authorization: Bearer add72ae475214adc83ea227c21fee0e5
Content-Type: application/json
Host: https://portal.docare.ch

{
  "resourceType": "Encounter",
  "class": {
    "code": "AMB"
  },
  "subject": {
    "reference": "https://portal.docare.ch/fhir/v4/Patient/d0a31764-6030-4284-984e-3bd967106ea4"
  },
  "period": {
    "start": "2018-11-12",
    "end": "2018-11-12"
  }
}
```

> The above request returns the following response:

```http
HTTP/1.1 201 Created
Content-Type: application/json
Location: https://portal.docare.ch/fhir/v4/Encounter/931a68c2-62ca-470e-b1f5-a590577d2936

{
  "resourceType": "Encounter",
  "id": "931a68c2-62ca-470e-b1f5-a590577d2936",
  "status": "in-progress",
  "class": {
    "code": "AMB"
  },
  "subject": {
    "reference": "https://portal.docare.ch/fhir/v4/Patient/d0a31764-6030-4284-984e-3bd967106ea4"
  },
  "period": {
    "start": "2018-11-12",
    "end": "2018-11-12"
  },
  "serviceProvider": {
    "reference": "https://portal.docare.ch/fhir/v4/Organization/04d2256f-8424-432d-80fc-af58e10dcfe1"
  }
}
```

This endpoint creates a new encounter.

### HTTP Request

`POST /fhir/v4/Encounter`

### JSON Attributes

Name | Type | Description
--------- | ------- | -----------
resourceType | [string](http://hl7.org/fhir/datatypes.html#string) | "Encounter" constant
class | [Coding]https://www.hl7.org/FHIR/datatypes.html#Coding | Classification of patient encounter
subject | [Reference](http://hl7.org/fhir/references.html) | The patient present at the encounter
period | [Period](http://hl7.org/fhir/datatypes.html#Period) | The start and end time of the encounter


## Get All Encounters

> To get all encounters for the patient with id `2f5da8c2-cbf1-42d1-9d7a-165f3ed80541`, send the following request:

```http
GET /fhir/v4/Encounter?subject=Patient/2f5da8c2-cbf1-42d1-9d7a-165f3ed80541 HTTP/1.1
Authorization: Bearer add72ae475214adc83ea227c21fee0e5
Host: https://portal.docare.ch
```

> The above request returns the following response:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "resourceType": "Bundle",
  "type": "searchset",
  "total": 1,
  "entry": [
    {
      "fullUrl": "https://portal.docare.ch/fhir/v4/Encounter/19d4c5a3-fa8d-4aa0-aa11-f9a1f31656d8",
      "resource": {
        "resourceType": "Encounter",
        "id": "19d4c5a3-fa8d-4aa0-aa11-f9a1f31656d8",
        "class": {
          "code": "AMB"
        },
        "subject": {
          "reference": "https://portal.docare.ch/fhir/v4/Patient/2f5da8c2-cbf1-42d1-9d7a-165f3ed80541"
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

`GET /fhir/v4/Encounter`

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
subject | [reference](http://hl7.org/fhir/references.html) | The patient present at the encounter.


## Get a Specific Encounter

> To get the encounter with id `19d4c5a3-fa8d-4aa0-aa11-f9a1f31656d8`, send the following request:

```http
GET /fhir/v4/Encounter/19d4c5a3-fa8d-4aa0-aa11-f9a1f31656d8 HTTP/1.1
Authorization: Bearer add72ae475214adc83ea227c21fee0e5
Host: https://portal.docare.ch
```

> The above request returns the following response:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "resourceType": "Encounter",
  "id": "19d4c5a3-fa8d-4aa0-aa11-f9a1f31656d8",
  "class": {
    "code": "AMB"
  },
  "subject": {
    "reference": "https://portal.docare.ch/fhir/v4/Patient/2f5da8c2-cbf1-42d1-9d7a-165f3ed80541"
  },
  "period": {
    "start": "2018-11-17",
    "end": "2018-11-17"
  },
  "serviceProvider": {
    "reference": "https://portal.docare.ch/fhir/v4/Organization/04d2256f-8424-432d-80fc-af58e10dcfe1"
  }
}
```

This endpoint retrieves a specific encounter.

### HTTP Request

`GET /fhir/v4/Encounter/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the encounter to retrieve


## Update an Encounter

> To update the encounter with id `931a68c2-62ca-470e-b1f5-a590577d2936`, send the following request:

```http
POST /fhir/v4/Encounter/931a68c2-62ca-470e-b1f5-a590577d2936 HTTP/1.1
Authorization: Bearer add72ae475214adc83ea227c21fee0e5
Content-Type: application/json
Host: https://portal.docare.ch

{
  "resourceType": "Encounter",
  "class": {
    "code": "AMB"
  },
  "period": {
    "start": "2018-11-12",
    "end": "2018-11-12"
  }
}
```

> The above request returns the following response:

```http
HTTP/1.1 201 Created
Content-Type: application/json
Location: https://portal.docare.ch/fhir/v4/Encounter/931a68c2-62ca-470e-b1f5-a590577d2936

{
  "resourceType": "Encounter",
  "id": "931a68c2-62ca-470e-b1f5-a590577d2936",
  "subject": {
    "reference": "https://portal.docare.ch/fhir/v4/Patient/d0a31764-6030-4284-984e-3bd967106ea4"
  },
  "period": {
    "start": "2018-11-12",
    "end": "2018-11-12"
  },
}
```

This endpoint updates all encounter data.

### HTTP Request

`POST /fhir/v4/Encounter/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the encounter to update

### JSON Attributes

Name | Type | Description
--------- | ------- | -----------
resourceType | [string](http://hl7.org/fhir/datatypes.html#string) | "Encounter" constant
class | [Coding]https://www.hl7.org/FHIR/datatypes.html#Coding | Classification of patient encounter
period | [Period](http://hl7.org/fhir/datatypes.html#Period) | The start and end time of the encounter

<aside class="notice">
The JSON structure must contain all attributes (incl. the ones that don't change). Missing attributes are treated as empty.
</aside>


## Delete an Encounter

> To delete the encounter with id `931a68c2-62ca-470e-b1f5-a590577d2936`, send the following request:

```http
DELETE /fhir/v4/Encounter/931a68c2-62ca-470e-b1f5-a590577d2936 HTTP/1.1
Authorization: Bearer add72ae475214adc83ea227c21fee0e5
Host: https://portal.docare.ch
```

> The above request returns the following response:

```http
HTTP/1.1 204 No Content
```

This endpoint deletes an encounter.

### HTTP Request

`DELETE /fhir/v4/Encounter/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the encounter to delete


# QuestionnaireResponse

The docare.ch FHIR [QuestionnaireResponse](https://www.hl7.org/fhir/questionnaireresponse.html) resource contains all the answers from the patient's questionnaire.


## Get All Questionnaire Responses

> To get all questionnaire responses for the patient with id `2f5da8c2-cbf1-42d1-9d7a-165f3ed80541`, send the following request:

```http
GET /fhir/v4/QuestionnaireResponse?subject=Patient/2f5da8c2-cbf1-42d1-9d7a-165f3ed80541 HTTP/1.1
Authorization: Bearer add72ae475214adc83ea227c21fee0e5
Host: https://portal.docare.ch
```

> The above request returns the following response:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "resourceType": "Bundle",
  "type": "searchset",
  "total": 1,
  "entry": [
    {
      "fullUrl": "https://portal.docare.ch/fhir/v4/QuestionnaireResponse/d605cd56-c554-4cbd-9c6b-8d7b16903443",
      "resource": {
        "resourceType": "Encounter",
        "id": "d605cd56-c554-4cbd-9c6b-8d7b16903443",
        "status": "completed",
        "questionnaire": "https://portal.docare.ch/fhir/v4/Questionnaire/history_dermatology",
        "authored": "2018-04-25T14:15:33.811244+00:00",
        "subject": {
          "reference": "https://portal.docare.ch/fhir/v4/Patient/2f5da8c2-cbf1-42d1-9d7a-165f3ed80541"
        },
        "encounter": {
          "reference": "https://portal.docare.ch/fhir/v4/Encounter/931a68c2-62ca-470e-b1f5-a590577d2936"
        }
      }
    }
  ]
}
```

This endpoint retrieves all questionnaire responses for a given patient.

### HTTP Request

`GET /fhir/v4/QuestionnaireResponse`

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
subject | [reference](http://hl7.org/fhir/references.html) | The patient of the questions.


## Get a Specific Questionnaire Response

> To get the questionnaire response with id `d605cd56-c554-4cbd-9c6b-8d7b16903443`, send the following request:

```http
GET /fhir/v4/QuestionnaireResponse/d605cd56-c554-4cbd-9c6b-8d7b16903443 HTTP/1.1
Authorization: Bearer add72ae475214adc83ea227c21fee0e5
Host: https://portal.docare.ch
```

> The above request returns the following response:

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "resourceType": "QuestionnaireResponse",
  "id": "d605cd56-c554-4cbd-9c6b-8d7b16903443",
  "meta": {
    "lastUpdated": "2019-07-12T09:16:03.157753+00:00"
  },
  "text": {
    "status": "generated",
    "div": "<div xmlns=\"http://www.w3.org/1999/xhtml\"><p>Konsultationsgrund: Allergie. Beschwerden seit 2 Wochen. Bisherige Abkl&#228;rungen: Allergietest.</p><p>Keine Dauerbeschwerden. Beschwerden treten nicht bei bestimmten T&#228;tigkeiten auf.</p></div>"
  },
  "status": "completed",
  "authored": "2019-07-12T09:16:03.157753+00:00",
  "subject": {
    "reference": "https://portal.docare.ch/fhir/v4/Patient/2f5da8c2-cbf1-42d1-9d7a-165f3ed80541"
  },
  "encounter": {
    "reference": "https://portal.docare.ch/fhir/v4/Encounter/931a68c2-62ca-470e-b1f5-a590577d2936"
  },
  "questionnaire": "https://portal.docare.ch/fhir/v4/Questionnaire/history_dermatology",
  "item": [
    {
      "linkId": "ailment",
      "answer": [
        {
          "valueCoding": {
            "code": "ailment_allergy"
          }
        }
      ]
    },
    {
      "linkId": "ailment_duration",
      "answer": [
        {
          "valueQuantity": {
            "value": 2,
            "unit": "wk"
          }
        }
      ]
    },
    {
      "linkId": "permanent_ailment",
      "answer": [
        {
          "valueBoolean": false
        }
      ]
    },
    {
      "linkId": "ailment_activity",
      "answer": [
        {
          "valueBoolean": false
        }
      ]
    },
    {
      "linkId": "past_investigations",
      "answer": [
        {
          "valueCoding": {
            "code": "past_investigations_allergy_test"
          }
        }
      ]
    }
  ]
}
```

This endpoint retrieves a specific questionnaire response.

### HTTP Request

`GET /fhir/v4/QuestionnaireResponse/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the questionnaire response to retrieve


# Errors

```http
GET /fhir/v4/Patient/invalid_id HTTP/1.1
Authorization: Bearer add72ae475214adc83ea227c21fee0e5
Host: https://portal.docare.ch
```

> The above request returns the following 404 response:

```http
HTTP/1.1 404 Not Found
Content-Type: application/json

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
