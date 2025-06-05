### query

```
POST /blacklist/mgmt/query HTTP/1.1
Authorization: Bearer <identity-info>

{
  "pagination": {
    "page": 0,
    "size": 5,
    "direction": "ASC",
    "sortField": "createdAt"
  },
  "systemNames": [
  ],
  "mode": "ACTIVES",
  "issuers": [
    "sysop"
  ],
  "revokers": [
  ],
  "reason": "temporary ban",
  "alivesAt": "2025-06-05T23:59:59Z"
}
```

Lehet az active true, úgy is, hogy lejárt a record!

```
{
  "entries": [
    {
      "systemName": "AlertConsumer1",
      "createdBy": "Sysop",
      "createdAt": "2025-06-05T13:43:07Z",
      "updatedAt": "2025-06-05T13:43:07Z",
      "reason": "temporary ban",
      "expiresAt": "2025-12-31T23:59:59Z",
      "active": true
    },
    {
      "systemName": "AlertConsumer2",
      "createdBy": "Sysop",
      "createdAt": "2025-06-05T13:43:07Z",
      "updatedAt": "2025-06-05T13:43:07Z",
      "reason": "temporary ban",
      "expiresAt": "2025-12-31T23:59:59Z",
      "active": true
    }
  ],
  "count": 2
}
```

```
{
  "errorMessage": "Mode is invalid. Possible values: ALL, ACTIVES, INACTIVES",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /blacklist/mgmt/query"
}
```

### create

```
POST /blacklist/mgmt/create HTTP/1.1
Authorization: Bearer <identity-info>

{
  "entities": [
    {
      "systemName": "TemperatureProvider1",
      "expiresAt": "",
      "reason": "This provider is broken and sends too many false alarms. Should be fixed."
    },
    {
      "systemName": "AlertConsumer1",
      "expiresAt": "2025-12-31T23:59:59Z",
      "reason": "temporary ban"
    },
    {
      "systemName": "AlertConsumer2",
      "expiresAt": "2025-12-31T23:59:59Z",
      "reason": "temporary ban"
    }
  ]
}
```

```
{
  "entries": [
    {
      "systemName": "TemperatureProvider1",
      "createdBy": "Sysop",
      "createdAt": "2025-06-05T13:43:06.568772600Z",
      "updatedAt": "2025-06-05T13:43:06.568772600Z",
      "reason": "This provider is broken and sends too many false alarms. Should be fixed.",
      "active": true
    },
    {
      "systemName": "AlertConsumer1",
      "createdBy": "Sysop",
      "createdAt": "2025-06-05T13:43:06.705704400Z",
      "updatedAt": "2025-06-05T13:43:06.705704400Z",
      "reason": "temporary ban",
      "expiresAt": "2025-12-31T23:59:59Z",
      "active": true
    },
    {
      "systemName": "AlertConsumer2",
      "createdBy": "Sysop",
      "createdAt": "2025-06-05T13:43:06.707704700Z",
      "updatedAt": "2025-06-05T13:43:06.707704700Z",
      "reason": "temporary ban",
      "expiresAt": "2025-12-31T23:59:59Z",
      "active": true
    }
  ],
  "count": 3
}
```

```
{
  "errorMessage": "You cannot blacklist a system without specifying the reason",
  "errorCode": 400,
  "exceptionType": "INVALID_PARAMETER",
  "origin": "POST /blacklist/mgmt/create"
}
```

### remove

```
DELETE /blacklist/mgmt/remove/AlertConsumer1,AlertConsumer2 HTTP/1.1
Authorization: Bearer <identity-info>
```

```
{
  "errorMessage": "TemperatureProvider1 system is blacklisted",
  "errorCode": 403,
  "exceptionType": "FORBIDDEN"
}
```