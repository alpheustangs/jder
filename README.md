# JSON Data Error Response (JDER)

The JSON Data Error Response (JDER) specification defines a standardized structure for JSON responses that can be used for both success and failure scenarios.

## Structure

The following is the structure of the response.

### Root Strucutre

The root structure is a JSON object with the following fields:

| Field     | Type    | Description                                      |
| --------- | ------- | ------------------------------------------------ |
| `success` | Boolean | Indicates whether the response is success or not |
| `data`    | Object  | Contains the data payload                        |
| `error`   | Object  | Contains the error details                       |

### `success` Field

The `success` field indicates whether the response is successful. However, it can be omitted if the HTTP status code is already used to convey this information, as shown in the examples below:

Success response (HTTP Status: `200`):

```json
{
    "data": {
        "message": "Hello, World!"
    }
}
```

Failure response (HTTP Status: `404`):

```json
{
    "error": {
        "code": "not_found"
    }
}
```

### `data` Field

The `data` field contains the response payload when `success` is `true`.

### `error` Field

The `error` field provides error information when `success` is `false`.

| Field     | Type   | Description                 |
| --------- | ------ | --------------------------- |
| `code`    | String | Error code                  |
| `field`   | String | Field that caused the error |
| `message` | String | Error message               |

Each API should define a set of error codes for consistent error handling across clients. For example, a parsing error may use the code `parse_error`, a timeout error may use the code `timeout`, and so on.

## Examples

Below are examples of both success and failure responses:

### Success Response

A basic success response:

```json
{
    "success": true
}
```

A success response that returns a single object in the `data` field:

```json
{
    "success": true,
    "data": {
        "message": "Hello, World!"
    }
}
```

A success response that returns a list of items in the `data` field:

```json
{
    "success": true,
    "data": [
        {
            "id": 1,
            "name": "John"
        }, 
        {
            "id": 2,
            "name": "Jane"
        }
    ]
}
```

### Failure Response

A failure response with only the `code` field:

```json
{
    "success": false,
    "error": {
        "code": "not_found",
    }
}
```

A failure response representing an internal server error:

```json
{
    "success": false,
    "error": {
        "code": "server_error",
        "message": "Internal server error",
    }
}
```

A failure response for a validation error, indicating a problem with a specific field:

```json
{
    "success": false,
    "error": {
        "code": "parse_error",
        "field": "name",
        "message": "Invalid JSON input"
    }
}
```

## License

This specification is MIT licensed, you can find the license file [here](./LICENSE).
