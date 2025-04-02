---
sidebar_title: General Information
sidebar_position: 1
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# General Information

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
