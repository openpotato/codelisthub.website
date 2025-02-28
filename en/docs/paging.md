# Paging

Some API endpoints in the CodeListHub project implement what is known as **paging**.

## What is Paging?

Paging in web services is a technique used to divide large datasets into smaller, more manageable parts. Instead of sending all the results of a query at once, the content is split into pages, allowing a client to access different pages sequentially or as needed.

## Why Paging?

Let's start with an example. The following query:

=== "PowerShell 7"

    ``` powershell
    curl -X GET 'https://api.codelisthub.org/v1/documents/index' -H 'accept: text/json' | ConvertFrom-Json | ConvertTo-Json
    ```

=== "Bash"

    ``` bash
    curl -X GET 'https://api.codelisthub.org/v1/documents/index' -H 'accept: text/json' | json_pp
    ```

Without paging, this request would return all documents in CodeListHub. In the future, this could be a significant amount, which would put an excessive load on the server, increase network latency, and extend response times. A better approach is to transfer this data in small chunks, known as pages. You can configure the size of these chunks when making a request.

## How Does Paging Work?

Paging is configured using special URL parameters in the API request. These parameters specify which page of results the client wants to see and how many results should be displayed per page. The URL parameters are:

+ `page`: The index of the requested page.
+ `pageSize`: The number of items per page.

The following request returns the first 10 document entries:

=== "PowerShell 7"

    ``` powershell
    curl -X GET 'https://api.codelisthub.org/v1/documents/index?page=1&pageSize=10' -H 'accept: text/json' | ConvertFrom-Json | ConvertTo-Json
    ```

=== "Bash"

    ``` bash
    curl -X GET 'https://api.codelisthub.org/v1/documents/index?page=1&pageSize=10' -H 'accept: text/json' | json_pp
    ```

The next request returns the next 10 document entries:

=== "PowerShell 7"

    ``` powershell
    curl -X GET 'https://api.codelisthub.org/v1/documents/index?page=2&pageSize=10' -H 'accept: text/json' | ConvertFrom-Json | ConvertTo-Json
    ```

=== "Bash"

    ``` bash
    curl -X GET 'https://api.codelisthub.org/v1/documents/index?page=2&pageSize=10' -H 'accept: text/json' | json_pp
    ```

If the `page` parameter is omitted from the request, the default value `page=1` is used. If the `pageSize` parameter is omitted, the default value `pageSize=50` is used. The request:

=== "PowerShell 7"

    ``` powershell
    curl -X GET 'https://api.codelisthub.org/v1/documents/index' -H 'accept: text/json' | ConvertFrom-Json | ConvertTo-Json
    ```

=== "Bash"

    ``` bash
    curl -X GET 'https://api.codelisthub.org/v1/documents/index' -H 'accept: text/json' | json_pp
    ```

is equivalent to:

=== "PowerShell 7"

    ``` powershell
    curl -X GET 'https://api.codelisthub.org/v1/documents/index?page=1&pageSize=50' -H 'accept: text/json' | ConvertFrom-Json | ConvertTo-Json
    ```

=== "Bash"

    ``` bash
    curl -X GET 'https://api.codelisthub.org/v1/documents/index?page=1&pageSize=50' -H 'accept: text/json' | json_pp
    ```

The maximum page size is 50.

## How Do I Know How Many Pages Exist?

A successful API response includes additional information in the [HTTP response headers](https://developer.mozilla.org/docs/Web/HTTP/Headers). Here is an example of a typical response header list:

```
content-security-policy: default-src https: data: 'unsafe-inline' 
content-type: text/json; charset=utf-8 
date: Thu, 26 Jan 2025 09:26:48 GMT 
permissions-policy: accelerometer=(), camera=(), geolocation=(), gyroscope=(), magnetometer=(), microphone=(), payment=(), usb=(), interest-cohort=() 
referrer-policy: strict-origin-when-cross-origin 
server: nginx 
strict-transport-security: max-age=2592000 
x-content-type-options: nosniff 
x-frame-options: SAMEORIGIN  
x-page: 1 
x-page-size: 10 
x-total-count: 120 
x-total-pages: 12 
```

The last four values are particularly relevant:

+ `x-page`: The index of the requested page.
+ `x-page-size`: The number of items per page.
+ `x-total-count`: The total number of items for this request.
+ `x-total-pages`: The total number of pages for this request.

These values are returned for all API endpoints that implement paging.

!!! note "Why Not Include This in the JSON Payload?"

    The CodeListHub API provides paging metadata in the HTTP response headers. Some implementations include this information in the JSON payload that represents the actual response. However, we decided against this approach because we support not only JSON but also CSV as a payload format.