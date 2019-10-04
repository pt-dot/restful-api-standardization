# Standarisasi RESTFul API

DOT Indonesia memiliki guideline sekaligus standarisasi dalam pembuatan RESTful API.

## Overview
## Request Format
## Response Format
This section describe the structure of a JSON:API document for response format. A JSON object **MUST** be at the root of every JSON:API response containing data. This object defines a document’s “top level" and it's highly recommended to have same top-level structure in our API response. Not only for the successful response, but also the failed response.

A document **MUST** contain at least one of the following top-level members:
- `data` : the document's "primary data"
- `errors` : an array of error objects

### Successful Response

In a successful response, there is a primary data that's a representation of the resource or collection of resources targeted by request.

Primary data **MUST** be either:
- a single resource object or `null`, for requests that target single resource
- an array of resource objects or an empty array (`[]`), for requests that target resource collections

For example, the following primary data is a single resource object:
```
{
    "success": true,
    "message": "This is successful message",
    "data": {
        "id": 1,
        "type": "articles",
        "created_at": "2019-10-04 14:33"
    }
}
```

The following primary data is an array of resource objects:
```
{
    "success": true,
    "message": "This is successful message",
    "data": [
        {
            "id": 1,
            "type": "articles",
            "created_at": "2019-10-04 13:33"
        },
        {
            "id": 2,
            "type": "articles",
            "created_at": "2019-10-04 14:33"
        }
    ]
}
```

### Failed Response
A server **MAY** choose to stop processing as soon as a problem is encountered, or it **MAY** continue processing and encounter multiple problems. For instance, a server might process multiple attributes and then return multiple validation problems in a single response.

#### Error Objects
Error objects provide additional information about problems encountered while performing an operation. Error objects **MUST** be returned as an array keyed by errors in the top level of a JSON:API document.

The following example is format usually used for failed response with status code 400, 401, 404, or 500:
```
{
    "success": false,
    "error-code": null, /* or optional error payload, eg: 3001, 3002, etc. */
    "errors": [],
    "message": "Error xyz has occurred"
}
```

For returning multiple validation problems, here's the example:
```
{
    "success": false,
    "error-code": null, /* or optional error payload, eg: 3001, 3002, etc. */
    "errors": [
        "The email must be a valid email",
        "The password must be at least 6 chaarcters",
        "The phone number is already used"
    ],
    "message": "Error xyz has occurred"
}
```

## HTTP Response Code
## Error Code
## Panduan Kontribusi

