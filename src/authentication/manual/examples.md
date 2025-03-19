# **Examples of data transfer objects sent and received**

This file contains specific examples of what payloads are expected and returned by each endpoint maintained by the Authentication system.

# Identity endpoints

## **Login**

  POST /authentication/identity/login
  
  Request body: 
  ~~~
  {
    "systemName": "consumer1",
    "credentials": {
      "password": "abcdef"
    }
  }
  ~~~

  Response body:
  ~~~
  {
	"token": "713bca0b-c550-4cb9-ae60-4852b9ee3669",
	"expirationTime": "2025-03-07T11:59:01.178225900Z"
  }
  ~~~

## **Change**

  POST /authentication/identity/change
  
  Request body: 
  ~~~
  {
	"systemName": "consumer1",
	"credentials": {
	  "password": "abcdef"
	},
	"newCredentials": {
	  "password": "123456"
	}
  }
  ~~~

  Response code: 200

## **Logout**

  POST /authentication/identity/logout
  
  Request body: 
  ~~~
  {
	"systemName": "consumer1",
	"credentials": {
	  "password": "123456"
	}
  }
  ~~~

  Response code: 200  

## **Verify**

  To use this operation, the requester must identify itself by providing its own identity token in the Authorization header.
  
  ~~~
  -H 'Authorization: Bearer IDENTITY-TOKEN//9333c248-7b16-478c-b6c8-605abbbc361a'
  ~~~

  GET /authentication/identity/{token}
  
  Path parameter: 713bca0b-c550-4cb9-ae60-4852b9ee3669
  ~~~
  http://localhost:8444/authentication/identity/713bca0b-c550-4cb9-ae60-4852b9ee3669
  ~~~

  Response body:

  ~~~
  {
	"verified": true,
	"systemName": "consumer1",
	"sysop": false,
	"loginTime": "2025-03-07T11:54:01Z",
	"expirationTime": "2025-03-07T12:54:01Z"
  }
  ~~~  

# Identity Management endpoints

  To use any of the following operations, the requester must identify itself by providing its own identity token in the Authorization header.
  
  ~~~
  -H 'Authorization: Bearer IDENTITY-TOKEN//9333c248-7b16-478c-b6c8-605abbbc361a'
  ~~~
  
## **Create**

  POST /authentication/mgmt/identities
  
  Request body:
  ~~~
  {
	"authenticationMethod": "PASSWORD",
	"identities": [
	  {
	    "systemName": "consumer1",
		"credentials": {
		  "password": "abcdef"
		},
		"sysop": false
	  },
	  {
		"systemName": "provider1",
		"credentials": {
		  "password": "123456"
		},
		"sysop": false
	  }
    ]
  }
  ~~~
  
  Response body:
  ~~~
  {
    "identities": [
      {
        "systemName": "consumer1",
        "authenticationMethod": "PASSWORD",
        "sysop": false,
        "createdBy": "sysop",
        "createdAt": "2025-03-07T12:52:30Z",
        "updatedBy": "sysop",
        "updatedAt": "2025-03-07T12:52:30Z"
      },
      {
        "systemName": "provider1",
        "authenticationMethod": "PASSWORD",
        "sysop": false,
        "createdBy": "sysop",
        "createdAt": "2025-03-07T12:52:30Z",
        "updatedBy": "sysop",
        "updatedAt": "2025-03-07T12:52:30Z"
      }
    ],
    "count": 2
  }
  ~~~
  
## **Query**
  
  POST /authentication/mgmt/identities/query
  
  Request body:
  ~~~
  {
    "pagination": {
      "page": 0,
      "size": 10,
      "direction": "ASC",
      "sortField": "name"
    },
    "createdBy": "sysop",
    "creationFrom": "2025-03-07T06:00:00Z"
  }
  ~~~
  
  Response body:
  ~~~
  {
    "identities": [
      {
        "systemName": "consumer1",
        "authenticationMethod": "PASSWORD",
        "sysop": false,
        "createdBy": "sysop",
        "createdAt": "2025-03-07T12:52:30Z",
        "updatedBy": "sysop",
        "updatedAt": "2025-03-07T12:52:30Z"
      },
      {
        "systemName": "provider1",
        "authenticationMethod": "PASSWORD",
        "sysop": false,
        "createdBy": "sysop",
        "createdAt": "2025-03-07T12:52:30Z",
        "updatedBy": "sysop",
        "updatedAt": "2025-03-07T12:52:30Z"
      }
    ],
    "count": 2
  }
  ~~~ 
  
## **Update**

  PUT /authentication/mgmt/identities
  
  Request body:
  ~~~
  {
	"authenticationMethod": "PASSWORD",
	"identities": [
	  {
	    "systemName": "consumer1",
		"credentials": {
		  "password": "123456"
		},
		"sysop": false
	  },
	  {
		"systemName": "provider1",
		"credentials": {
		  "password": "123456"
		},
		"sysop": true
	  }
    ]
  }
  ~~~
  
  Response body:
  ~~~
  {
    "identities": [
      {
        "systemName": "consumer1",
        "authenticationMethod": "PASSWORD",
        "sysop": false,
        "createdBy": "sysop",
        "createdAt": "2025-03-07T12:52:30",
        "updatedBy": "sysop",
        "updatedAt": "2025-03-07T12:59:01"
      },
      {
        "systemName": "provider1",
        "authenticationMethod": "PASSWORD",
        "sysop": true,
        "createdBy": "sysop",
        "createdAt": "2025-03-07T12:52:30Z",
        "updatedBy": "sysop",
        "updatedAt": "2025-03-07T12:59:01Z"
      }
    ],
    "count": 2
  }
  ~~~
  
## **Remove**

  DELETE /authentication/mgmt/identities
  
  Query parameters: provider1, provider2
  
  ~~~
  http://localhost:8444/authentication/mgmt/identities?names=provider1&names=provider2
  ~~~

  Response code: 200
  
## **Session Query**
  
  POST /authentication/mgmt/sessions
  
  Request body:
  ~~~
  {
    "pagination": {
      "page": 0,
      "size": 10,
      "direction": "ASC",
      "sortField": "name"
    },
    "loginFrom": "2025-03-07T10:00:00Z"
  }
  ~~~
  
  Response body:
  ~~~
  {
    "sessions": [
      {
        "systemName": "consumer1",
        "loginTime": "2025-03-07T11:54:01Z",
        "expirationTime": "2025-03-08T11:59:01Z"
      },
      {
        "systemName": "sysop",
        "loginTime": "2025-03-07T12:40:54Z",
        "expirationTime": "2025-03-08T12:45:54Z"
      }
    ],
    "count": 2
  }
  ~~~ 

## **Session Close**

  DELETE /authentication/mgmt/sessions
  
  Query parameters: consumer1
  
  ~~~
  http://localhost:8444/authentication/mgmt/sessions?names=consumer1
  ~~~

  Response code: 200

# General Management endpoints

  To use any of the following operations requester must identify itself by providing its own identity token in the Authorization header.
  
  ~~~
  -H 'Authorization: Bearer IDENTITY-TOKEN//9333c248-7b16-478c-b6c8-605abbbc361a'
  ~~~

## **Get Config**
  
  GET /authentication/mgmt/get-config

  Query parameters: management.policy, max.page.size

  ~~~
  http://localhost:8444/authentication/mgmt/get-config?keys=management.policy&keys=max.page.size
  ~~~

  Response body:
  ~~~
  {
    "map": {
      "management.policy": "authorization",
      "max.page.size": "1000"
    }
  }
  ~~~
  
## **Get Log**

  POST /authentication/general/mgmt/logs

  Request body:

  ~~~
  {
    "pagination": {
      "page": 0,
      "size": 5,
      "direction": "ASC",
      "sortField": "entryDate"
    },
    "from": "2025-03-07T00:00:00Z",
    "to": "2025-03-08T00:00:00Z",
    "severity": "INFO"
  }
  ~~~

  Response body:
  
  ~~~
  {
    "entries": [
      {
        "logId": "1d61efc1-fb39-11ef-b55e-2cf05d74cdac",
        "entryDate": "2025-03-07T09:46:59.203Z",
        "logger": "eu.arrowhead.authentication.AuthenticationMain",
        "severity": "INFO",
        "message": "Starting AuthenticationMain using Java 21.0.5 with PID 13072 (omitted)"
      },
      {
        "logId": "1d9b9d62-fb39-11ef-b55e-2cf05d74cdac",
        "entryDate": "2025-03-07T09:46:59.212Z",
        "logger": "eu.arrowhead.authentication.AuthenticationMain",
        "severity": "INFO",
        "message": "No active profile set, falling back to 1 default profile: \"default\""
      },
      {
        "logId": "1e2d2f03-fb39-11ef-b55e-2cf05d74cdac",
        "entryDate": "2025-03-07T09:47:00.973Z",
        "logger": "eu.arrowhead.common.http.filter.authorization.ManagementServiceFilter",
        "severity": "INFO",
        "message": "ManagementServiceFilter is active"
      },
      {
        "logId": "1e4cec04-fb39-11ef-b55e-2cf05d74cdac",
        "entryDate": "2025-03-07T09:47:01.058Z",
        "logger": "eu.arrowhead.authentication.http.filter.InternalAuthenticationFilter",
        "severity": "INFO",
        "message": "InternalAuthenticationFilter is active"
      },
      {
        "logId": "1f529c35-fb39-11ef-b55e-2cf05d74cdac",
        "entryDate": "2025-03-07T09:47:02.897Z",
        "logger": "eu.arrowhead.authentication.quartz.CleanerConfig$$SpringCGLIB$$0",
        "severity": "INFO",
        "message": "Cleaner job is initialized."
      }
    ],
    "count": 26
  }
  ~~~   