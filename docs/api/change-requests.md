---
sidebar_title: Change Requests
sidebar_position: 2
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

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
| from         | string | Date in ISO8601 format                 |
| to           | string | Date in ISO8601 format                 |
| search       | string |                                        |
| sort         | string |                                        |
| start        | number |                                        |
| count        | number |                                        |

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
