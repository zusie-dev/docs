import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# General information

## Authentication

All API calls (except the login route) need to be authenticated. In order to authenticate you need to send an `Authorization` header with the JWT user token. You receive the user token by calling the route [User Login](https://dev.zusie.cloud/api/docs#user-management-user-login). We use JWT technology to generate the token (see https://jwt.io/introduction/ for more information). You will receive two tokens. The first one is the Access Token. It is needed to access any secured endpoint, but it expires after 15 minutes per default. This timeout can be changed if needed. The second token is the Refresh Token. The Refresh Token expires after 30 days and can only be used to reissue a new access token. Therefore the endpoint [Refresh Token](https://dev.zusie.cloud/api/docs#user-management-token-refresh) exists.

<details>
<summary>
Expired Access Token Response (Status 401)
</summary>
  ```json
  {
    "status": 401,
    "message": "The token has expired"
  }
  ```
</details>
<details>
<summary>
Invalid Access Token Response (Status 422)
</summary>

```json
{
  "status": 422,
  "message": "Not enough parts"
}
```

</details>
<details>
<summary>
Revoked Token Response (Status 401)
</summary>

You may get this response, if the user logged out or was deleted from the web portal.

```json
{
  "status": 401,
  "message": "Token has been revoked"
}
```

</details>

## Error States

The common [HTTP Response Status Codes](https://github.com/for-GET/know-your-http-well/blob/master/status-codes.md) are used.

## Default response shape

The default shape of the responses for requests is as follows:

```json
{
  "status": 200,
  "message": "Ok",
  "data": {
    // ...
  }
}
```

The `data` field is generally optional (i.e. if an error occurs or if no data is returned back).

## Sorting

If a get request supports the `sort` query parameter, you can set it to the sort keyword. The valid keywords are specified in the endpoint documentation. The default sort direction is ascending, if you want to sort descending, just place a dash in front of the keyword, i.e. `-username`.

# Change Requests

## Get all Change Requests

**`GET` /api/change-requests**

### Query String Parameters

| Name         | Type   | Hint                                   |
| ------------ | ------ | -------------------------------------- |
| state        | enum   | Allowed: pending / accepted / rejected |
| reason       | string |                                        |
| type         | enum   | Allowed: core / premium / premium_test |
| district_ids | string | Comma separated list                   |
| …            |        |                                        |

### Payload

<details>
  <summary>Create a Change Request as a tracking deliverer</summary>

```json
{
  "district_id": "4711",
  "tour_date": "2025-04-02",
  "message": "I worked from 8 to 12, forgot to click stop"
}
```

</details>

<details>
<summary> Create a Change Request as a target time deliverer</summary>

```json
{
  "district_id": "4711",
  "tour_date": "2025-04-02",
  "reason": "Bad weather",
  "requested_time_difference": 30
}
```

</details>

### Response

<details>
<summary> 200 Normal Response</summary>

```json
{
  "status": 0,
  "message": "string",
  "data": [
    {
      "id": 1,
      "requested_district_id": "D4711",
      "requested_time_difference": -10,
      "created_by": {
        "username": "U1234",
        "firstname": "Max",
        "lastname": "Mustermann",
        "roles": ["deliverer"]
      },
      "requested_by": {
        "username": "U1234",
        "firstname": "Max",
        "lastname": "Mustermann",
        "tracking_type": "premium",
        "roles": ["deliverer"],
        "requested_at": "2021-10-13T13:23:34.245Z"
      },
      "accepted_time_difference": -12,
      "answered_by": {
        "username": "U1234",
        "firstname": "Max",
        "lastname": "Meier",
        "roles": ["deliverer_manager"],
        "answered_at": "2021-10-13T17:23:34.245Z"
      },
      "tour_id": 4711,
      "tour_date": "2021-10-13",
      "message": "I missed clicking stop",
      "reason": "forgetfulness",
      "district_id": "D4711",
      "target_time": 127,
      "tour_type": "target_time",
      "time_tracks": [
        {
          "id": 1,
          "start": "2018-01-01T05:32:46.142Z",
          "end": "2018-01-01T07:02:46.142Z",
          "type": "work",
          "task": "Delivering"
        }
      ],
      "state": "pending",
      "feedback": "It's ok for me"
    }
  ],
  "total": 103
}
```

</details>
