---
title: API reference
---

## API document
Version 1.  
Url path requires v1.

### GET /auth
Verify token endpoint.

#### HTTP Header
|Name|Required|Description|Example|
|---|---|---|---|
|Authorization|required|Token|Bearer JWT|
|Client Secret|optional|Client secret info. If decided request server, recommended set Client-Secret.|Secret|

#### Path Parameter
None

#### Query Parameter
|Name|Required|Description|Example|
|---|---|---|---|
|group_uuid|optional|Verify belong group user.|UUIDv4|
|role|optional|Comma separated multiple. Verify a role of group.|admin,data_manager|
|permission|optional|Comma separated multiple. Verify a permission of group.|read,write|

#### Request
Example.
```bash
$ jwt={Token}
$ curl -H 'Authorization: Bearer ${jwt}' https://{HOST NAME}/auth
```

#### Response
|Value|Schema|Description|
|---|---|---|
|grant|Boolean|Result.|

Example.
```bash
$ curl -i -H 'Authorization: Bearer ${jwt}' https://{HOST NAME}/auth
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Date: Wed, 30 Dec 2020 13:56:13 GMT

{"grant": true | false}
```

### GET /services
Get all services.

#### HTTP Header
None
#### Path Parameter
None

#### Query Parameter
None

#### Request
Example.
```bash
$ curl https://{HOST NAME}/services
```

#### Response
|Value|Schema|Description|
|---|---|---|
|id|Integer|Uniq id.|
|internal_id|String|Internal uniq id.|
|uuid|String|UUIDv4.|
|name|String|Service name.|
|secret|String|Service client secret.|
|created_at|Date|Generated date.|
|updated_at|Date|Updated date.|

Example.
```bash
$ curl -i https://{HOST NAME}/services
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Date: Wed, 30 Dec 2020 13:56:13 GMT

[
  {
    "id": 1,
    "internal_id": "xxxxxxxxxxxxx",
    "uuid": "8a7ba9ec-947e-44bd-a9b4-e3f338606dbe",
    "name": "food_deliverly",
    "secret": "1d2f92d224604679b456c656cc55acc6",
    "created_at": "yyyymmdd hhmmsssss",
    "updated_at": "yyyymmdd hhmmsssss"
  }
]
```

### POST /token
Issue token.

#### HTTP Header
None

#### Path Parameter
None

#### Query Parameter
|Name|Required|Description|Example|
|---|---|---|---|
|type|optional|The user type. `user` or `operator`|user|

#### Request
|Value|Required|Schema|Description|
|---|---|---|---|
|password|required|String|Password.|
|email|required|String|Email.|
|grant_type|required|String|`password` or `refresh_token`|
|refresh_token|optional|String|Refresh token.|

Example.
```bash
$ curl -X POST https://{HOST NAME}/token \
  -d "{\"password\": \"xxxxxxx\", \"email\": \"xxxxxxx@gmail.com\", \"grant_type\": \"password\"}"
```

#### Response
|Value|Schema|Description|
|---|---|---|
|token|String|Token.|
|refresh_token|String|Refresh token.|

Example.
```bash
$ curl -X POST https://{HOST NAME}/token \
  -d "{\"password\": \"xxxxxxx\", \"email\": \"xxxxxxx@gmail.com\", \"grant_type\": \"password\"}"
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Date: Wed, 30 Dec 2020 13:56:13 GMT

{
  "token": "xxxxxxxxx",
  "refresh_token": "xxxxxxxxx"
}
```

### POST /users
Generate user.

#### HTTP Header
|Name|Required|Description|Example|
|---|---|---|---|
|Client Secret|required|Client secret info. |Service secret|

#### Path Parameter
None

#### Query Parameter
None

#### Request
|Value|Required|Schema|Description|
|---|---|---|---|
|username|required|String|Username.|
|email|required|String|Email.|
|password|required|String|Password.|

Example.
```bash
$ service_secret={Service secret}
$ curl -X POST https://{HOST NAME}/users \
  -H 'Client-Secret: ${service_secret}' \
  -d "{\"password\": \"xxxxxxx\", \"email\": \"xxxxxxx@gmail.com\", \"username\": \"test\"}"
```

#### Response
|Value|Schema|Description|
|---|---|---|
|message|String|Message.|

Example.
```bash
$ curl -X POST https://{HOST NAME}/users \
  -H 'Client-Secret: ${service_secret}' \
  -d "{\"password\": \"xxxxxxx\", \"email\": \"xxxxxxx@gmail.com\", \"username\": \"test\"}"
HTTP/1.1 201 OK
Content-Type: application/json; charset=utf-8
Date: Wed, 30 Dec 2020 13:56:13 GMT

{
  "message": "User creation succeeded."
}
```

### PUT /users
Update user.

#### HTTP Header
|Name|Required|Description|Example|
|---|---|---|---|
|Authorization|required|Token|Bearer JWT|
|Client Secret|required|Client secret info. |Service secret|

#### Path Parameter
None

#### Query Parameter
None

#### Request
|Value|Required|Schema|Description|
|---|---|---|---|
|username|required|String|Username. If update this one, change it.|
|email|required|String|Email. If update this one, change it.|
|password|required|String|Password. If update this one, change it.|

Example.
```bash
$ jwt={Token}
$ service_secret={Service secret}
$ curl -H 'Authorization: Bearer ${jwt}' \
  -H 'Client-Secret: ${service_secret}' \
  -X PUT https://{HOST NAME}/users \
  -d "{\"password\": \"xxxxxxx\", \"email\": \"xxxxxxx@gmail.com\", \"username\": \"test\"}"
```

#### Response
|Value|Schema|Description|
|---|---|---|
|message|String|Message.|

Example.
```bash
$ curl -H 'Authorization: Bearer ${jwt}' \
  -H 'Client-Secret: ${service_secret}' \
  -X PUT https://{HOST NAME}/users \
  -d "{\"password\": \"xxxxxxx\", \"email\": \"xxxxxxx@gmail.com\", \"username\": \"test\"}"
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Date: Wed, 30 Dec 2020 13:56:13 GMT

{
  "message": "User update succeeded."
}
```

### GET /users/group
Get groups of user.

#### HTTP Header
|Name|Required|Description|Example|
|---|---|---|---|
|Authorization|required|Token|Bearer JWT|
|Client Secret|optional|Client secret info.|Secret|

#### Path Parameter
None

#### Query Parameter
None

#### Request
Example.
```bash
$ jwt={Token}
$ curl -H 'Authorization: Bearer ${jwt}' \
  https://{HOST NAME}/users/group
```

#### Response
|Value|Schema|Description|
|---|---|---|
|id|Integer|Uniq id.|
|internal_id|String|Internal uniq id.|
|uuid|String|UUIDv4.|
|name|String|Group name.|
|created_at|Date|Generated date.|
|updated_at|Date|Updated date.|

Example.
```bash
$ curl -i -H 'Authorization: Bearer ${jwt}' \
  https://{HOST NAME}/users/group
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Date: Wed, 30 Dec 2020 13:56:13 GMT

[
  {
    "id": 1,
    "internal_id": "xxxxxxxxxxxxx",
    "uuid": "8a7ba9ec-947e-44bd-a9b4-e3f338606dbe",
    "name": "group01",
    "created_at": "yyyymmdd hhmmsssss",
    "updated_at": "yyyymmdd hhmmsssss"
  }
]
```

### POST /users/group
Register group of service.

#### HTTP Header
|Name|Required|Description|Example|
|---|---|---|---|
|Authorization|required|Token|Bearer JWT|
|Client Secret|required|Client secret info.|Secret|

#### Path Parameter
None

#### Query Parameter
None

#### Request
Example.
```bash
$ jwt={Token}
$ client_secret={Service Secret}
$ curl -X POST -H 'Authorization: Bearer ${jwt}' \
  -H 'Client-Secret: ${client_secret}' \
  https://{HOST NAME}/users/group \
  -d "{\"name\": \"xxxxxxx\"}"
```

#### Response
|Value|Schema|Description|
|---|---|---|
|id|Integer|Uniq id.|
|internal_id|String|Internal uniq id.|
|uuid|String|UUIDv4.|
|name|String|Group name.|
|created_at|Date|Generated date.|
|updated_at|Date|Updated date.|

Example.
```bash
$ curl -X POST -H 'Authorization: Bearer ${jwt}' \
    -H 'Client-Secret: ${client_secret}' \
    https://{HOST NAME}/users/group \
    -d "{\"name\": \"xxxxxxx\"}"

HTTP/1.1 201 OK
Content-Type: application/json; charset=utf-8
Date: Wed, 30 Dec 2020 13:56:13 GMT

{
  "id": 1,
  "internal_id": "xxxxxxxxxxxxx",
  "uuid": "8a7ba9ec-947e-44bd-a9b4-e3f338606dbe",
  "name": "group01",
  "created_at": "yyyymmdd hhmmsssss",
  "updated_at": "yyyymmdd hhmmsssss"
}
```

### GET /users/service
Get services of user.

#### HTTP Header
|Name|Required|Description|Example|
|---|---|---|---|
|Authorization|required|Token|Bearer JWT|

#### Path Parameter
None

#### Query Parameter
None

#### Request
Example.
```bash
$ jwt={Token}
$ curl -H 'Authorization: Bearer ${jwt}' \
  https://{HOST NAME}/users/service
```

#### Response
|Value|Schema|Description|
|---|---|---|
|id|Integer|Uniq id.|
|internal_id|String|Internal uniq id.|
|uuid|String|UUIDv4.|
|name|String|Service name.|
|secret|String|Service client secret.|
|created_at|Date|Generated date.|
|updated_at|Date|Updated date.|

Example.
```bash
$ curl -i -H 'Authorization: Bearer ${jwt}' \
  https://{HOST NAME}/users/service
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Date: Wed, 30 Dec 2020 13:56:13 GMT

[
  {
    "id": 1,
    "internal_id": "xxxxxxxxxxxxx",
    "uuid": "8a7ba9ec-947e-44bd-a9b4-e3f338606dbe",
    "name": "food_deliverly",
    "secret": "1d2f92d224604679b456c656cc55acc6",
    "created_at": "yyyymmdd hhmmsssss",
    "updated_at": "yyyymmdd hhmmsssss"
  }
]
```

### GET /users/policy
Get policies of user.

#### HTTP Header
|Name|Required|Description|Example|
|---|---|---|---|
|Authorization|required|Token|Bearer JWT|

#### Path Parameter
None

#### Query Parameter
None

#### Request
Example.
```bash
$ jwt={Token}
$ curl -H 'Authorization: Bearer ${jwt}' \
  https://{HOST NAME}/users/policy
```

#### Response
|Value|Schema|Description|
|---|---|---|
|name|String|Policy name.|
|role_name|String|Role name.|
|role_uuid|String|Role uniq id.|
|permission_name|String|Permission name.|
|permission_uuid|String|Permission uniq id.|
|service_name|String|Service name.|
|service_uuid|String|Service uniq id.|
|group_name|String|Group name.|
|group_uuid|String|Group uniq id.|

Example.
```bash
$ curl -i -H 'Authorization: Bearer ${jwt}' \
  https://{HOST NAME}/users/policy
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Date: Wed, 30 Dec 2020 13:56:13 GMT

[
  {
    "name": "policy01",
    "role_name": "role01",
    "role_uuid": "8a7ba9ec-947e-44bd-a9b4-e3f338606dbe",
    "permission_name": "permission01",
    "permission_uuid": "1cd1d41f-b322-4d03-af3b-023da5e60bb0",
    "service_name": "service01",
    "service_uuid": "4082f766-30b6-43e4-a4d7-9e8c7dd4f514",
    "group_name": "group01",
    "group_uuid": "09ecef1f-2d06-4776-9b3b-2b1b40c25652"
  }
]
```

### GET /groups
Get groups of a service.  
Required user role of service.

#### HTTP Header
|Name|Required|Description|Example|
|---|---|---|---|
|Authorization|required|Token|Bearer JWT|
|Client Secret|required|Client secret info|Secret|

#### Path Parameter
|Name|Description|Example|
|---|---|---|
|group uuid|Group uniq id|13209548-e7df-4ce7-b00f-e88d5de2f2d5|

#### Query Parameter
None

#### Request
Example.
```bash
$ jwt={Token}
$ client_secret={Service secret}
$ curl -H 'Authorization: Bearer ${jwt}' \
  -H 'Client-Secret: ${client_secret}'
  https://{HOST NAME}/groups/13209548-e7df-4ce7-b00f-e88d5de2f2d5
```

#### Response
|Value|Schema|Description|
|---|---|---|
|id|Integer|Uniq id.|
|internal_id|String|Internal uniq id.|
|uuid|String|UUIDv4.|
|name|String|Group name.|
|created_at|Date|Generated date.|
|updated_at|Date|Updated date.|

Example.
```bash
$ curl -H 'Authorization: Bearer ${jwt}' \
  -H 'Client-Secret: ${client_secret}'
  https://{HOST NAME}/groups/13209548-e7df-4ce7-b00f-e88d5de2f2d5

HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Date: Wed, 30 Dec 2020 13:56:13 GMT

{
  "id": 1,
  "internal_id": "xxxxxxxxxxxxx",
  "uuid": "8a7ba9ec-947e-44bd-a9b4-e3f338606dbe",
  "name": "group01",
  "created_at": "yyyymmdd hhmmsssss",
  "updated_at": "yyyymmdd hhmmsssss"
}
```

### GET /groups/{G_ID}/user
Get users of group in a service.  
Required admin role of service.

#### HTTP Header
|Name|Required|Description|Example|
|---|---|---|---|
|Authorization|required|Token|Bearer JWT|
|Client Secret|required|Client secret info|Secret|

#### Path Parameter
|Name|Description|Example|
|---|---|---|
|group uuid|Group uniq id|13209548-e7df-4ce7-b00f-e88d5de2f2d5|

#### Query Parameter
None

#### Request
Example.
```bash
$ jwt={Token}
$ client_secret={Service secret}
$ curl -H 'Authorization: Bearer ${jwt}' \
  -H 'Client-Secret: ${client_secret}'
  https://{HOST NAME}/groups/13209548-e7df-4ce7-b00f-e88d5de2f2d5/user
```

#### Response
|Value|Schema|Description|
|---|---|---|
|uuid|Integer|User uniq id.|
|username|String|User name.|
|email|String|User email.|

Example.
```bash
$ curl -H 'Authorization: Bearer ${jwt}' \
  -H 'Client-Secret: ${client_secret}'
  https://{HOST NAME}/groups/13209548-e7df-4ce7-b00f-e88d5de2f2d5/user

HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Date: Wed, 30 Dec 2020 13:56:13 GMT

[
 {
   "uuid": "8a7ba9ec-947e-44bd-a9b4-e3f338606dbe",
   "username": "username01",
   "email": "xxx@gmail.com"
 }
]
```

### PUT /groups/{G_ID}/user
Add user of group in a service.  
Required admin role of service.

#### HTTP Header
|Name|Required|Description|Example|
|---|---|---|---|
|Authorization|required|Token|Bearer JWT|
|Client Secret|required|Client secret info|Secret|

#### Path Parameter
|Name|Description|Example|
|---|---|---|
|group uuid|Group uniq id|13209548-e7df-4ce7-b00f-e88d5de2f2d5|

#### Query Parameter
None

#### Request
|Value|Schema|Description|
|---|---|---|
|user_email|String|User email.|

Example.
```bash
$ jwt={Token}
$ client_secret={Service secret}
$ curl -X PUT \
  -H 'Authorization: Bearer ${jwt}' \
  -H 'Client-Secret: ${client_secret}'
  https://{HOST NAME}/groups/13209548-e7df-4ce7-b00f-e88d5de2f2d5/user \
  -d "{\"user_email\": \"xxxx@gmail.com\"}"
```

#### Response
|Value|Schema|Description|
|---|---|---|
|id|Integer|Uniq id.|
|internal_id|String|Internal uniq id.|
|uuid|String|UUIDv4.|
|user_uuid|String|User uniq id.|
|group_uuid|String|Group uniq id.|
|created_at|Date|Generated date.|
|updated_at|Date|Updated date.|

Example.
```bash
$ curl -X PUT \
  -H 'Authorization: Bearer ${jwt}' \
  -H 'Client-Secret: ${client_secret}'
  https://{HOST NAME}/groups/13209548-e7df-4ce7-b00f-e88d5de2f2d5/user \
  -d "{\"user_email\": \"xxxx@gmail.com\"}"

HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Date: Wed, 30 Dec 2020 13:56:13 GMT

{
  "id": 1,
  "internal_id": "xxxxx",   
  "uuid": "8a7ba9ec-947e-44bd-a9b4-e3f338606dbe",
  "user_uuid": "23ab98ca-dc05-474c-8c16-c03d77dd64da",
  "group_uuid": "aa1a92a4-20f1-4ba1-b178-cf0e1edf9566",
  "created_at": "yyyymmdd hhmmsssss",
  "updated_at": "yyyymmdd hhmmsssss"
}
```

### GET /groups/{G_ID}/policy
Get policies of all user group in a service.  
Required admin role of service.

#### HTTP Header
|Name|Required|Description|Example|
|---|---|---|---|
|Authorization|required|Token|Bearer JWT|
|Client Secret|required|Client secret info|Secret|

#### Path Parameter
|Name|Description|Example|
|---|---|---|
|group uuid|Group uniq id|13209548-e7df-4ce7-b00f-e88d5de2f2d5|

#### Query Parameter
None

#### Request
Example.
```bash
$ jwt={Token}
$ client_secret={Service secret}
$ curl -H 'Authorization: Bearer ${jwt}' \
  -H 'Client-Secret: ${client_secret}'
  https://{HOST NAME}/groups/13209548-e7df-4ce7-b00f-e88d5de2f2d5/policy
```

#### Response
|Value|Schema|Description|
|---|---|---|
|username|String|User name.|
|email|String|User email.|
|service_name|String|Service name.|
|policy_name|String|Policy name.|
|role_name|String|Role name.|
|permission_name|String|Permission name.|

Example.
```bash
$ curl -H 'Authorization: Bearer ${jwt}' \
  -H 'Client-Secret: ${client_secret}'
  https://{HOST NAME}/groups/13209548-e7df-4ce7-b00f-e88d5de2f2d5/policy

HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Date: Wed, 30 Dec 2020 13:56:13 GMT

[
 {
   "username": "username01",
   "email": "xxx@gmail.com",
   "service_name": "service_name01",
   "policy_name": "policy_name01",
   "role_name": "role_name01",
   "permission_name": "permission_name01"
 }
]
```

### PUT /groups/{G_ID}/policy
Update policy of user of group in a service.  
Required admin role of service.

#### HTTP Header
|Name|Required|Description|Example|
|---|---|---|---|
|Authorization|required|Token|Bearer JWT|
|Client Secret|required|Client secret info|Secret|

#### Path Parameter
|Name|Description|Example|
|---|---|---|
|group uuid|Group uniq id|13209548-e7df-4ce7-b00f-e88d5de2f2d5|

#### Query Parameter
None

#### Request
|Value|Required|Schema|Description|
|---|---|---|---|
|name|required|String|Policy name.|
|to_user_email|required|String|To user email.|
|role_uuid|required|String|Role uniq id.|
|permission_uuid|required|String|Permission uniq id.|

Example.
```bash
$ jwt={Token}
$ client_secret={Service secret}
$ curl -X PUT \
  -H 'Authorization: Bearer ${jwt}' \
  -H 'Client-Secret: ${client_secret}'
  https://{HOST NAME}/groups/13209548-e7df-4ce7-b00f-e88d5de2f2d5/policy \
  -d "\"name\": \"policy_name\", \"to_user_email\": \"xxx@gmail.com\", \"role_uuid\": \"d8791354-bcaa-43ae-891e-dc3e32212b95\", \"permission_uuid\": \"fa989c43-0eef-461c-a018-c0f2ca90366c\""
```

#### Response
|Value|Schema|Description|
|---|---|---|
|id|Integer|Uniq id.|
|internal_id|String|Internal uniq id.|
|name|String|Policy name.|
|role_uuid|String|Role uniq id.|
|permission_uuid|String|Permission uniq id.|
|service_uuid|String|Service uniq id.|
|user_group_uuid|String|User group uniq id.|
|created_at|Date|Generated date.|
|updated_at|Date|Updated date.|

Example.
```bash
$ curl -X PUT \
  -H 'Authorization: Bearer ${jwt}' \
  -H 'Client-Secret: ${client_secret}'
  https://{HOST NAME}/groups/13209548-e7df-4ce7-b00f-e88d5de2f2d5/policy \
  -d "\"name\": \"policy_name\", \"to_user_email\": \"xxx@gmail.com\", \"role_uuid\": \"d8791354-bcaa-43ae-891e-dc3e32212b95\", \"permission_uuid\": \"fa989c43-0eef-461c-a018-c0f2ca90366c\""

HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Date: Wed, 30 Dec 2020 13:56:13 GMT

{
  "id": 1,
  "internal_id": "xxxxxxxxxxxxx",
  "name": "policy01",
  "role_name": "role01",
  "role_uuid": "8a7ba9ec-947e-44bd-a9b4-e3f338606dbe",
  "permission_uuid": "1cd1d41f-b322-4d03-af3b-023da5e60bb0",
  "service_uuid": "4082f766-30b6-43e4-a4d7-9e8c7dd4f514",
  "user_group_uuid": "09ecef1f-2d06-4776-9b3b-2b1b40c25652",
  "created_at": "yyyymmdd hhmmsssss",
  "updated_at": "yyyymmdd hhmmsssss"
}
```

### GET /groups/{G_ID}/role
Get roles of group in a service.  
Required user role of service.

#### HTTP Header
|Name|Required|Description|Example|
|---|---|---|---|
|Authorization|required|Token|Bearer JWT|
|Client Secret|required|Client secret info|Secret|

#### Path Parameter
|Name|Description|Example|
|---|---|---|
|group uuid|Group uniq id|13209548-e7df-4ce7-b00f-e88d5de2f2d5|

#### Query Parameter
None

#### Request
Example.
```bash
$ jwt={Token}
$ client_secret={Service secret}
$ curl -H 'Authorization: Bearer ${jwt}' \
  -H 'Client-Secret: ${client_secret}'
  https://{HOST NAME}/groups/13209548-e7df-4ce7-b00f-e88d5de2f2d5/role
```

#### Response
|Value|Schema|Description|
|---|---|---|
|id|Integer|Uniq id.|
|internal_id|String|Internal uniq id.|
|uuid|String|UUIDv4.|
|name|String|Role name.|
|created_at|Date|Generated date.|
|updated_at|Date|Updated date.|

Example.
```bash
$ curl -H 'Authorization: Bearer ${jwt}' \
  -H 'Client-Secret: ${client_secret}'
  https://{HOST NAME}/groups/13209548-e7df-4ce7-b00f-e88d5de2f2d5/role

HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Date: Wed, 30 Dec 2020 13:56:13 GMT

[
 {
   "id": 1,
   "internal_id": "xxxxxxxxxxxxx",
   "uuid": "8a7ba9ec-947e-44bd-a9b4-e3f338606dbe",
   "name": "role01",
   "created_at": "yyyymmdd hhmmsssss",
   "updated_at": "yyyymmdd hhmmsssss"
 }
]
```

### POST /groups/{G_ID}/role
Register role of group in a service.  
Required admin role of service.

#### HTTP Header
|Name|Required|Description|Example|
|---|---|---|---|
|Authorization|required|Token|Bearer JWT|
|Client Secret|required|Client secret info.|Secret|

#### Path Parameter
None

#### Query Parameter
None

#### Request
|Value|Schema|Description|
|---|---|---|
|name|String|Role name.|

Example.
```bash
$ jwt={Token}
$ client_secret={Service Secret}
$ curl -X POST -H 'Authorization: Bearer ${jwt}' \
  -H 'Client-Secret: ${client_secret}' \
  https://{HOST NAME}/users/role \
  -d "{\"name\": \"xxxxxxx\"}"
```

#### Response
|Value|Schema|Description|
|---|---|---|
|id|Integer|Uniq id.|
|internal_id|String|Internal uniq id.|
|uuid|String|UUIDv4.|
|name|String|Role name.|
|created_at|Date|Generated date.|
|updated_at|Date|Updated date.|

Example.
```bash
$ curl -X POST -H 'Authorization: Bearer ${jwt}' \
    -H 'Client-Secret: ${client_secret}' \
    https://{HOST NAME}/users/role \
    -d "{\"name\": \"xxxxxxx\"}"

HTTP/1.1 201 OK
Content-Type: application/json; charset=utf-8
Date: Wed, 30 Dec 2020 13:56:13 GMT

{
  "id": 1,
  "internal_id": "xxxxxxxxxxxxx",
  "uuid": "8a7ba9ec-947e-44bd-a9b4-e3f338606dbe",
  "name": "role01",
  "created_at": "yyyymmdd hhmmsssss",
  "updated_at": "yyyymmdd hhmmsssss"
}
```

### DELETE /groups/{G_ID}/role
Not implementation.

### GET /groups/{G_ID}/permission
Get permissions of group in a service.  
Required user role of service.

#### HTTP Header
|Name|Required|Description|Example|
|---|---|---|---|
|Authorization|required|Token|Bearer JWT|
|Client Secret|required|Client secret info|Secret|

#### Path Parameter
|Name|Description|Example|
|---|---|---|
|group uuid|Group uniq id|13209548-e7df-4ce7-b00f-e88d5de2f2d5|

#### Query Parameter
None

#### Request
Example.
```bash
$ jwt={Token}
$ client_secret={Service secret}
$ curl -H 'Authorization: Bearer ${jwt}' \
  -H 'Client-Secret: ${client_secret}'
  https://{HOST NAME}/groups/13209548-e7df-4ce7-b00f-e88d5de2f2d5/permission
```

#### Response
|Value|Schema|Description|
|---|---|---|
|id|Integer|Uniq id.|
|internal_id|String|Internal uniq id.|
|uuid|String|UUIDv4.|
|name|String|Permission name.|
|created_at|Date|Generated date.|
|updated_at|Date|Updated date.|

Example.
```bash
$ curl -H 'Authorization: Bearer ${jwt}' \
  -H 'Client-Secret: ${client_secret}'
  https://{HOST NAME}/groups/13209548-e7df-4ce7-b00f-e88d5de2f2d5/permission

HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Date: Wed, 30 Dec 2020 13:56:13 GMT

[
 {
   "id": 1,
   "internal_id": "xxxxxxxxxxxxx",
   "uuid": "8a7ba9ec-947e-44bd-a9b4-e3f338606dbe",
   "name": "permission01",
   "created_at": "yyyymmdd hhmmsssss",
   "updated_at": "yyyymmdd hhmmsssss"
 }
]
```

### POST /groups/{G_ID}/permission
Register permission of group in a service.  
Required admin role of service.

#### HTTP Header
|Name|Required|Description|Example|
|---|---|---|---|
|Authorization|required|Token|Bearer JWT|
|Client Secret|required|Client secret info.|Secret|

#### Path Parameter
None

#### Query Parameter
None

#### Request
|Value|Schema|Description|
|---|---|---|
|name|String|Permission name.|

Example.
```bash
$ jwt={Token}
$ client_secret={Service Secret}
$ curl -X POST -H 'Authorization: Bearer ${jwt}' \
  -H 'Client-Secret: ${client_secret}' \
  https://{HOST NAME}/users/permission \
  -d "{\"name\": \"xxxxxxx\"}"
```

#### Response
|Value|Schema|Description|
|---|---|---|
|id|Integer|Uniq id.|
|internal_id|String|Internal uniq id.|
|uuid|String|UUIDv4.|
|name|String|Permission name.|
|created_at|Date|Generated date.|
|updated_at|Date|Updated date.|

Example.
```bash
$ curl -X POST -H 'Authorization: Bearer ${jwt}' \
    -H 'Client-Secret: ${client_secret}' \
    https://{HOST NAME}/users/permission \
    -d "{\"name\": \"xxxxxxx\"}"

HTTP/1.1 201 OK
Content-Type: application/json; charset=utf-8
Date: Wed, 30 Dec 2020 13:56:13 GMT

{
  "id": 1,
  "internal_id": "xxxxxxxxxxxxx",
  "uuid": "8a7ba9ec-947e-44bd-a9b4-e3f338606dbe",
  "name": "permission01",
  "created_at": "yyyymmdd hhmmsssss",
  "updated_at": "yyyymmdd hhmmsssss"
}
```

### DELETE /groups/{G_ID}/permission
Not implementation.
