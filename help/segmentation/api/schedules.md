---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Schedules
topic: developer guide
---

# Schedules developer guide

intro

- Retrieve a list of schedules
- Create a new schedule
- Retrieve a specific schedule
- Update a specific schedule
- Delete a specific schedule

## Getting started

The API endpoints used in this guide are part of the Segmentation API. Before continuing, please review the [Segmentation developer guide](./getting-started.md).

In particular, the [getting started section](./getting-started.md#getting-started) of the Segmentation developer guide includes links to related topics, a guide to reading the sample API calls in the document, and important information regarding required headers that are needed to successfully make calls to any Experience Platform API.

## Retrieve a list of schedules

You can retrieve a list of all schedules for your IMS Organization by making a GET request to the `/config/schedules` endpoint.

**API format**

```http
GET /config/schedules
GET /config/schedules?{QUERY_PARAMETERS}
```

- `{QUERY_PARAMETERS}`: (*Optional*) Parameters added to the request path which configure the results returned in the response. Multiple parameters can be included, separated by ampersands (`&`). The available parameters are listed below.

**Query parameters**

The following is a list of available query parameters for listing schedules. All of these parameters are optional. Making a call to this endpoint with no parameters will retrieve all schedules available for your organization.

| Parameter | Description |
| --------- | ----------- |
| `start` | Specifies which page the offset will start from. By default, this value will be 0. |
| `limit` | Specifies the number of schedules returned. By default, this value will be 100. |

**Request**

```shell
curl -X GET https://platform.adobe.io/data/core/ups/config/schedules?limit=X \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**Response**

A successful response returns HTTP status 200 with a list of schedules for the specified IMS organization as JSON. 

```json
{
    "_page": {
        "totalCount": 1,
        "pageSize": 1
    },
    "children": [
        {
            "id": "4e538382-dbd8-449e-988a-4ac639ebe72b",
            "imsOrgId": "{IMS_ORG}",
            "sandbox": {
                "sandboxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
                "sandboxName": "prod",
                "type": "production",
                "default": true
            },
            "name": "Batch Segmentation",
            "state": "active",
            "type": "batch_segmentation",
            "schedule": "0 0 1 * * ?",
            "properties": {
                "segments": []
            },
            "createEpoch": 1573158851,
            "updateEpoch": 1574365202
        }
    ],
    "_links": {
        "next": {}
    }
}
```

## Create a new schedule

You can create a new schedule by making a POST request to the `/config/schedules` endpoint.

**API format**

```http
POST /config/schedules
```

**Request**

```shell
curl -X POST https://platform.adobe.io/data/core/ups/config/schedules \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '
{
  "name": "profile-default",
  "type": "batch_segmentation",
  "properties": {
    "segments": [
      "*"
    ]
  },
  "schedule": "0 0 1 * * ?",
  "state": "inactive"
}
 '
```

| Parameter | Description  |
| --------- | ------------ |
| `name` | **Required.** The name of the schedule as a string. |
| `type` | **Required.** The type of job as a string. The two supported types are `batch_segmentation` and `export`. |
| `properties` | **Required.** An object containing additional properties related to the schedule. |
| `properties.segments` | **Required when `type` equals `batch_segmentation`.** Using `["*"]` ensures all segments are included. |
| `schedule` | **Required.** A string containing the job schedule. For more information about cron schedules, please read the [cron expression format](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html) documentation. In this example, "0 0 1 * *" means that this schedule will run at midnight on the first of every month. |
| `state` | *Optional.* A string containing the schedule state. The two supported states are `active` and `inactive`. By default, the state is set to `inactive`. |

**Response**

A successful response returns HTTP status 200 with details of your newly created schedule.

```json
{
    "id": "4e538382-dbd8-449e-988a-4ac639ebe72b",
    "imsOrgId": "{IMS_ORG}",
    "sandbox": {
        "sandboxId": "e7e17720-c5bb-11e9-aafb-87c71c35cac8",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "name": "{SCHEDULE_NAME}",
    "state": "inactive",
    "type": "batch_segmentation",
    "schedule": "0 0 1 * * ?",
    "properties": {
        "segments": [
            "*"
        ]
    },
    "createEpoch": 1568267948,
    "updateEpoch": 1568267948
}
```

## Retrieve a specific schedule

You can retrieve detailed information about a specific schedule by making a GET request to the `/config/schedules` endpoint and providing the schedule's `id` value in the request path.

**API format**

```http
GET /config/schedules/{SCHEDULE_ID}
```

- `{SCHEDULE_ID}`: The `id` value of the schedule you want to retrieve.

**Request**

```shell
curl -X GET https://platform.adobe.io/data/core/ups/config/schedules/{SCHEDULE_ID}
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**Response**

A successful response returns HTTP status 200 with detailed information about the specified schedule.

```json
{
    "id": "4e538382-dbd8-449e-988a-4ac639ebe72b",
    "imsOrgId": "{IMS_ORG}",
    "sandbox": {
        "sandboxId": "e7e17720-c5bb-11e9-aafb-87c71c35cac8",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "name": "{SCHEDULE_NAME}",
    "state": "inactive",
    "type": "batch_segmentation",
    "schedule": "0 0 1 * * ?",
    "properties": {
        "segments": [
            "*"
        ]
    },
    "createEpoch": 1568267948,
    "updateEpoch": 1568267948
```

## Update details a specific schedule

You can update a specified schedule by making a PATCH request to the `/config/schedules` endpoint and providing the schedule's `id` value in the request path.

The PATCH request supports two different paths: `/state` and `/schedule`.

### Update schedule state

You can use `/state` to update the state of the schedule - ACTIVE or INACTIVE. To update the state, you will need to set the value as `active` or `inactive`.

**API format**

```http
PATCH /config/schedules/{SCHEDULE_ID}
```

- `{SCHEDULE_ID}`: The `id` value of the schedule you want to update.

**Request**

```shell
curl -X DELETE https://platform.adobe.io/data/core/ups/config/schedules/{SCHEDULE_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '
[
  {
    "op": "add",
    "path": "/state",
    "value": "active"
  }
]
'
```

| Parameter | Description |
| --------- | ----------- |
| `path` | The path of the value you want to patch. In this case, since you are updating the schedule's state, you need to set the value of `path` to `/state`. |
| `value` | The updated value of the `/state`. This value can either be set as `active` or `inactive` to activate or deactivate the schedule. |

**Response**

A successful response returns HTTP status 204 (No Content).

### Update schedule cron schedule

You can use `schedule` to update the cron schedule. For more information about cron schedules, please read the [cron expression format](http://www.quartz-scheduler.org/documentation/quartz-2.3.0/tutorials/crontrigger.html) documentation.

**API format**

```http
PATCH /config/schedules/{SCHEDULE_ID}
```

- `{SCHEDULE_ID}`: The `id` value of the schedule you want to update.

**Request**

```shell
curl -X DELETE https://platform.adobe.io/data/core/ups/config/schedules/{SCHEDULE_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '
[
  {
    "op": "add",
    "path": "/schedule",
    "value": "0 0 2 * *"
  }
]
'
```

| Parameter | Description |
| --------- | ----------- |
| `path` | The path of the value you want to patch. In this case, since you are updating the schedule's cron schedule, you need to set the value of `path` to `/schedule`. |
| `value` | The updated value of the `/state`. This value needs to be in the form of a cron schedule. In this example, the schedule will run on the second of every month. |

**Response**

A successful response returns HTTP status 204 (No Content).

## Delete a specific schedule

You can request to delete a specified schedule by making a DELETE request to the `/config/schedules` and providing the schedule's `id` value in the request path.

**API format**

```http
DELETE /config/schedules/{SCHEDULE_ID}
```

- `{SCHEDULE_ID}`: The `id` value of the schedule you want to delete.

**Request**

```shell
curl -X DELETE https://platform.adobe.io/data/core/ups/config/schedules/{SCHEDULE_ID} \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**Response**

A successful response returns HTTP status 204 (No Content) with the following message:

```json
(No Content) Schedule deleted successfully
```

## Next steps