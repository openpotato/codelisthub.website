The CodeListHub API allows users to download individual documents in the OpenCodeList format or, if available, in CSV format. It also enables searching through the entire directory of documents on CodeListHub.

The easiest way to use the API is via the command line. In this chapter, we will use the command-line tool [curl](https://curl.se/).

On Linux, `curl` is typically pre-installed. On Windows, `curl` is defined as an alias for the PowerShell Cmdlet [Invoke-WebRequest](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-webrequest) and can be used via PowerShell. The commands used here vary slightly, so they are provided separately for PowerShell 7 (Windows) and Bash (Linux).

## Document Index

The document index allows searching through all documents stored on CodeListHub.

!!! info ""
    
    **`api.codelisthub.org/v1/documents/index`**
    
    :    Returns a list of all available code lists and code list collections. The query is subject to [paging](paging.md).
    
         Possible [HTTP request headers](https://developer.mozilla.org/docs/Web/HTTP/Headers) include: 

         + `Accept`: The [Accept](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept) header determines the response format. Possible values are `application/json`, `text/json`, and `text/plain`. If this header is not provided, `application/json` is assumed.

         Optional parameters include: 

         + `language (string)`: Language to filter by, specified as an [IETF BCP 47 language tag](https://datatracker.ietf.org/doc/html/rfc5646).
         + `searchTerm (string)`: Search for names and descriptions using [regular expressions](regex.md).
         + `tags (string)`: A comma-separated list of tags or keywords to filter by.
         + `type (string)`: Document type. Possible values:
            + `CodeList` 
            + `CodeListSet`
         + `publishedFrom (string)`: Oldest publication date to filter by.
         + `publishedUntil (string)`: Newest publication date to filter by.

    **`api.codelisthub.org/v1/documents/index/{canonicalUri}`**
    
    :    Returns a list of all available code lists and code list collections for a given canonical URI. The query is subject to [paging](paging.md).
    
         Possible [HTTP request headers](https://developer.mozilla.org/docs/Web/HTTP/Headers) include: 

         + `Accept`: The [Accept](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept) header determines the response format. Possible values are `application/json`, `text/json`, and `text/plain`. If this header is not provided, `application/json` is assumed.

         Optional parameters include: 

         + `language (string)`: Language to filter by, specified as an [IETF BCP 47 language tag](https://datatracker.ietf.org/doc/html/rfc5646).

Each document includes the following attributes:

+ `type (string)`: Document type. Possible values:
    + `CodeList` 
    + `CodeListSet`
+ `language (string)`: Document language as an [IETF BCP 47 language tag](https://datatracker.ietf.org/doc/html/rfc5646).
+ `shortName (string)`: Short name of the document.
+ `longName (string)`: Full name of the document.
+ `description (string)`: Short description of the document.
+ `version (string)`: Document version.
+ `canonicalUri (string)`: A URN that uniquely identifies all versions of the document.
+ `canonicalVersionUri (string)`: A URN that uniquely identifies this version of the document.
+ `tags (string-array)`: A list of tags or keywords that define the document's content.
+ `publishedAt (string)`: Publication date of the document.
+ `publisher.shortName (string)`: Publisher’s short name.
+ `publisher.longName (string)`: Publisher’s full name.
+ `publisher.url (string)`: Reference to additional external information (e.g., a website) about the publisher.
+ `publisher.identifier.Value (string)`: Key, code, or ID associated with the publisher.
+ `publisher.identifier.source.shortName (string)`: Short name of the source.
+ `publisher.identifier.source.longName (string)`: Full name of the source.
+ `publisher.identifier.source.url (string)`: Reference to additional external information (e.g., a website) about the source.

Example query: Retrieve all documents related to [OpenT8](https://openpotato.github.io/opent8/) standard.

=== "PowerShell 7"

    ```powershell
    curl -X GET 'https://api.codelisthub.org/v1/documents/index?searchTerm=opent8' -H 'accept: text/json' | ConvertFrom-Json | ConvertTo-Json
    ```

=== "Bash"

    ```bash
    curl -X GET 'https://api.codelisthub.org/v1/documents/index?searchTerm=opent8' -H 'accept: text/json' | json_pp
    ```

Example query: Retrieve all German-language code lists

=== "PowerShell 7"

    ```powershell
    curl -X GET 'https://api.codelisthub.org/v1/documents/index?type=CodeList&language=de' -H 'accept: text/json' | ConvertFrom-Json | ConvertTo-Json
    ```

=== "Bash"

    ```bash
    curl -X GET 'https://api.codelisthub.org/v1/documents/index?type=CodeList&language=de' -H 'accept: text/json' | json_pp
    ```

## Querying Individual Documents

To retrieve specific code lists or code list collections, use the following API endpoint:

!!! info ""

    **`api.codelisthub.org/v1/documents/{canonicalUri}`**

    :    Returns an OpenCodeList document specified by the `canonicalUri` parameter.

         Possible [HTTP request headers](https://developer.mozilla.org/docs/Web/HTTP/Headers) include: 

         + `Accept`: The [Accept](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept) header determines the response format. Possible values are `application/json`, `text/json`, and `text/plain`. If this header is not provided, `application/json` is assumed.
         + `Accept-Language`: The [Accept-Language](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Language) header specifies the preferred language of the response. The default should be an [IETF BCP 47 language tag](https://datatracker.ietf.org/doc/html/rfc5646). If this header is not provided, `de` is assumed.

         Optional parameters include: 

         + `language (string)`: Language in which the OpenCodeList document should be returned.
         + `metaOnly (boolean)`: Determines whether only metadata (i.e., an OpenCodeList document without data) should be returned.

A successful request returns a JSON document in the OpenCodeList format.

Example query: Retrieve the German ISO country list.

=== "PowerShell 7"

    ```powershell
    curl -X GET 'https://api.codelisthub.org/v1/documents/urn%3Acodelisthub%3Aiso%3Acountries%3Av1' -H 'accept: text/json' -H 'accept-language: de' | ConvertFrom-Json | ConvertTo-Json
    ```

=== "Bash"

    ```bash
    curl -X GET 'https://api.codelisthub.org/v1/documents/urn%3Acodelisthub%3Aiso%3Acountries%3Av1' -H 'accept: text/json' -H 'accept-language: de' | json_pp
    ```

## Querying Alternative Formats

Code lists can be retrieved in CSV format using the following API endpoint:

!!! info ""

    **`api.codelisthub.org/v1/documents/{canonicalUri}/alternative-format`**

    :    Returns a document specified by the `canonicalUri` parameter in an alternative format.
    
         Possible [HTTP request headers](https://developer.mozilla.org/docs/Web/HTTP/Headers) include: 

         + `Accept`: The [Accept](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept) header determines the format in which the document is returned. Possible values include `text/csv`. If this header is not provided, `text/csv` is assumed.
         + `Accept-Language`: The [Accept-Language](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Language) header determines the language in which the document is returned. By default, this should be an [IETF BCP 47 language tag](https://datatracker.ietf.org/doc/html/rfc5646). If this header is not provided, `de` (German) is assumed.

         Optional parameters include:

         + `language (string)`: The language in which the document should be returned, specified as an [IETF BCP 47 language tag](https://datatracker.ietf.org/doc/html/rfc5646).

A successful query returns the document in the requested format.

Example query: Retrieve the German ISO country list in CSV format.

=== "PowerShell 7"

    ```powershell
    curl -X GET 'https://api.codelisthub.org/v1/documents/urn%3Acodelisthub%3Aiso%3Acountries%3Av1/alternative-format' -H 'accept: text/csv' -H 'accept-language: de'
    ```

=== "Bash"

    ```bash
    curl -X GET 'https://api.codelisthub.org/v1/documents/urn%3Acodelisthub%3Aiso%3Acountries%3Av1/alternative-format' -H 'accept: text/csv' -H 'accept-language: de'
    ```

Example query: Retrieve the English ISO country list in CSV format.

=== "PowerShell 7"

    ```powershell
    curl -X GET 'https://api.codelisthub.org/v1/documents/urn%3Acodelisthub%3Aiso%3Acountries%3Av1/alternative-format' -H 'accept: text/csv' -H 'accept-language: en'
    ```

=== "Bash"

    ```bash
    curl -X GET 'https://api.codelisthub.org/v1/documents/urn%3Acodelisthub%3Aiso%3Acountries%3Av1/alternative-format' -H 'accept: text/csv' -H 'accept-language: en'
    ```

## Querying Tags

A list of all available tags or keywords can be retrieved using the following API endpoint:

!!! info ""

    **`api.codelisthub.org/v1/tags`**

    :    Returns a list of all tags available on CodeListHub. The query is subject to [paging](paging.md).

         Possible [HTTP request headers](https://developer.mozilla.org/docs/Web/HTTP/Headers) include: 

         + `Accept`: The [Accept](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept) header determines the format in which the tag list is returned. Possible values are `application/json`, `text/json`, and `text/plain`. If this header is not provided, `application/json` is assumed.

A successful query returns a JSON string array.

Example query:

=== "PowerShell 7"

    ```powershell
    curl -X GET 'https://api.codelisthub.org/v1/tags' -H 'accept: text/json'
    ```

=== "Bash"

    ```bash
    curl -X GET 'https://api.codelisthub.org/v1/tags' -H 'accept: text/json'
    ```
