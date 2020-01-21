# RESTful API Guideline

## Overview
DOT Indonesia has a guideline as well as standardization in making RESTful API. This is about how to design and develop RESTful API for internal developers or vendor partners in order that to ensure that APIs are developed according to standards.

## URL Design
When determining an API’s URL structure, it is helpful to consider that all of its resources exist in a single “reference document” in which each resource is addressable at a unique path. Resources are grouped by type at the top level of this document. Individual resources are keyed by ID within these typed collections. Attributes and links within individual resources are uniquely addressable according to the resource object structure described above.

The URL should contain resources (nouns), not actions or verbs. And the resource should always be plural in the API endpoint and if we want to access one instance of the resource, we can always pass the id in the URL.

Here are the examples:
- `/photos`
- `/products`
- `/categories`
- etc.

The following examples are about breaking down a resource according to the HTTP verbs:
- GET `/photos`, should get the list of all photos
- GET `/photos/1`, should get the detail of photos with ID `1`
- POST `/photos`, should store single photo resource
- PUT `/photos/1`, should update detail of photos with ID `1`
- DELETE `/photos/1`, should delete photo with ID `1`

### Related Resource URL
It is recommended that a related resource URL be formed by appending the name of the relationship to the resource’s URL.

For example, the URL for product's `variants` would be:
```
/products/1/variants
```

And the URL for a product's `images` would be:
```
/products/1/images
```

## Request Format
## Response Format
This section describe the structure of a JSON:API document for response format. A JSON object **MUST** be at the root of every JSON:API response containing data. This object defines a document’s “top level".

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

For returning multiple validation problems, it **MUST** return error messages inside an array. Inside the `errors` array **MUST** contain the following element:
- `key` : taken from request body's property that doesn't pass the validation
- `value`: is the validation error message

Here's the example for multiple validation problems only contains value of error:
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

Here's the example for multiple validation problems contains field and value of error:
```
{
    "success": false,
    "error-code": null, /* or optional error payload, eg: 3001, 3002, etc. */
    "errors": {
        "email": "The email must be a valid email",
        "password": "The password must be at least 6 chaarcters",
        "phone": "The phone number is already used"
    },
    "message": "Error xyz has occurred"
}
```

## HTTP Response Code
HTTP response status code indicate whether a specific HTTP request hasbeen successfully completed. There are 3 types of response code usually used:
- Successful response (`2xx`)
- Client errors (`4xx`)
- Server errors (`5xx`)

### Successful Responses (`2xx`)
**200 OK**

The request has succeeded. The meaning of the success depends on the HTTP method:
- `GET`: The resource has been fetched and is transmitted in the message body.
- `PUT or POST`: The resource describing the result of the action is transmitted in the message body.

**201 Created**

The request has succeeded and a new resource has been created as a result. This is typically the response sent after `POST` requests, or some `PUT` requests.

### Client Error Responses (`4xx`)
**400 Bad Request**

The server could not understand the request due to invalid syntax.

**401 Unauthorized**

Although the HTTP standard specifies "unauthorized", semantically this response means "unauthenticated". That is, the client must authenticate itself to get the requested response.

**404 Not Found**

The server couldn't find requested resource. This can be the URL / endpoint or the data that client looking for is not exists.

**405 Method Not Allowed**

The request method is known by the server, but has been disabled and cannot be used. For example:
> An endpoint `/users/1` must be requested using `GET` HTTP verb, but we access it using `POST` HTTP verb.

According to the case above, the server will return a response with the `405` status code.

### Server Error Response (`5xx`)
**500 Internal Server Error**

This means that the server has encountered a situation it doesn't know how to handle. For example when a failure occurred in deleting data.

**502 Bad Gateway**

This error response means that the server, while working as a gateway to get a response needed to handle the request, got an invalid response. Or in short, when our server is down.

## Error Code
## Panduan Kontribusi

