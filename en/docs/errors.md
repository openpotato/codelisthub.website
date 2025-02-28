# Error Messages

The CodeListHub API returns error messages as JSON objects formatted according to RFC 9457.

## What is RFC 9457?

[RFC 9457](https://www.rfc-editor.org/rfc/rfc9457) is a standard defined by the Internet Engineering Task Force (IETF) that specifies a structured format for representing errors in HTTP APIs.

## Problem Details

At the core of RFC 9457 is the *Problem Details* object, a JSON document included in the response body of an API request when an error occurs. The CodeListHub API implements it with the following fields:

### `type` (string)

A URI reference that identifies the type of error. Typically, this is a reference to the HTTP status code specification on the web.

Example:  
`"https://tools.ietf.org/html/rfc9110#section-15.5.1"`

### `title` (string)

A short, human-readable summary of the problem.

Example:  
`"One or more validation errors occurred."`

### `status` (integer)

The HTTP status code describing the error. This should match the status code in the HTTP response header.

Example:  
`400`

### `errors` (object) *(optional)*

A JSON object containing further details about the error.

Example:
```json
{
  "pageSize": [
    "The field pageSize must be between 1 and 50."
  ]
}
```

### `traceId` (string)

A unique identifier that can be used for tracking errors in distributed systems.

Example:  
`"00-abc123def456789ghi-xyz987uvw654321tqr-01"`

!!! note "Extension Members"

    The fields `type`, `title`, and `status` are well-defined in RFC 9457. The fields `errors` and `traceId` are *extension members*, meaning they extend the standard fields.

## Example

The following API request:

=== "PowerShell 7"

    ``` powershell
    curl -X GET 'https://api.codelisthub.org/v1/documents/index?page=1&pageSize=99' -H 'accept: text/json' | ConvertFrom-Json | ConvertTo-Json
    ```

=== "Bash"

	``` bash
	curl -X GET 'https://api.codelisthub.org/v1/documents/index?page=1&pageSize=99' -H 'accept: text/json' | json_pp
	```

results in the following error message:

```json
{
  "type": "https://tools.ietf.org/html/rfc9110#section-15.5.1",
  "title": "One or more validation errors occurred.",
  "status": 400,
  "errors": {
    "pageSize": [
      "The field pageSize must be between 1 and 50."
    ]
  },
  "traceId": "00-0274bb16dcdf462bf27a7faedeacc79f-05c5cd5b5d411f8e-00"
}
```

### Error Analysis:

+ The server cannot process the request because it is incorrectly formulated (HTTP error code 400: *Bad Request*).

+ The message `"One or more validation errors occurred."` indicates that there was an issue with request parameter validation.

+ The issue is indeed a validation error: the parameter `pageSize` has an invalid value (`99`), exceeding the allowed range (`1-50`).