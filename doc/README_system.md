1. [System](README_system.md) (white)
1. [Request](README_request.md) (blue)
1. [Accept](README_accept.md) (green)
1. [Retrieve](README_retrieve.md) (white)
1. [Precondition](README_precondition.md) (yellow)
1. Create/Process
    * [Create](README_create.md) (violet)
    * [Process](README_process.md) (red)
1. [Response](README_response.md) (cyan)
1. [Alternative](README_alternative.md) (gray)

![HTTP headers status](https://rawgithub.com/for-GET/http-decision-diagram/master/httpdd.png)

## System

This block is in charge of "system"-level (request agnostic) checks.

| | callback | output | default
|:-- | ---: | :--- | :---
|B26 | [`start : in`](#start--in) | |
|B24 | [`is_service_available :bin`](#is_service_available-bin) | T / F | TRUE
|B23 | [`is_uri_too_long :bin`](#is_uri_too_long-bin) | T / F | FALSE
|B22 | [`are_headers_too_large :bin`](#are_headers_too_large-bin) | T / F | FALSE
|B21 | [`method :var`](#method-var) | *Method* | `Transaction.request.method`
| | [`create_methods :var`](#create_methods-var) | [ *Method* ] | [ POST<br>]
| | [`process_methods :var`](#process_methods-var) | [ *Method* ] | [ POST<br>, PATCH<br>]
| | [`implemented_methods :var`](#implemented_methods-var) | [ *Method* ] | [ OPTIONS<br>, HEAD<br>, GET<br>, DELETE<br>, TRACE<br>, `create_methods :var`<br>, `process_methods :var`<br>]
| | [`is_method_implemented : in`](#is_method_implemented--in) | T / F |
|B20 | [`implemented_content_headers :var`](#implemented_content_headers-var) | [ *HeaderName* ] | [ content-encoding<br>, content-language<br>, content-length<br>, content-md5<br>, content-type<br>]
| | [`are_content_headers_implemented : in`](#are_content_headers_implemented--in) | T / F |
|B19 | [`is_functionality_implemented :bin`](#is_functionality_implemented-bin) | T / F | TRUE
|B18 | [`implemented_expect_extensions :var`](#implemented_expect_extensions-var) | [ *ExtensionName* ] | []
| | [`are_expect_extensions_implemented : in`](#are_expect_extensions_implemented--in) | T / F |
|B17 | [`is_system_block_ok :bin`](#is_system_block_ok-bin) | T / F | TRUE
|P25 | [`beautify_headers :bin`](#beatify_headers-bin) | |
| | [`last : in`](#last--in) | |
| | [`camelcase_response_headers : in`](#camelcase_response_headers--in) | |
| | [`need_camelcased_response_headers :bin`](#need_camelcased_response_headers-bin) | |
| | [`override :bin`](#override-bin) | |
|P26 | [`finish : in`](#finish--in) | |



### `start : in`

Prepare *Transaction* for the request.

### `is_service_available :bin`

Return TRUE if the resource is accepting requests; return FALSE otherwise.

Reference: [HTTPbis](http://tools.ietf.org/html/draft-ietf-httpbis-p2-semantics-22#section-6.6.4), [RFC2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.5.4)

> The 503 (Service Unavailable) status code indicates that the server is currently unable to handle the request due to a temporary overload or scheduled maintenance, which will likely be alleviated after some delay.  The server MAY send a Retry-After header field (Section 7.1.3) to suggest an appropriate amount of time for the client to wait before retrying the request.

> > Note: The existence of the 503 status code does not imply that a server has to use it when becoming overloaded.  Some servers might simply refuse the connection.

### `is_uri_too_long :bin`

Return TRUE if the URI is too long; return FALSE otherwise.

Reference: [HTTPbis](http://tools.ietf.org/html/draft-ietf-httpbis-p2-semantics-22#section-6.5.12), [RFC2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.4.15)

> The 414 (URI Too Long) status code indicates that the server is refusing to service the request because the request-target (Section 5.3 of [Part1]) is longer than the server is willing to interpret. This rare condition is only likely to occur when a client has improperly converted a POST request to a GET request with long query information, when the client has descended into a "black hole" of redirection (e.g., a redirected URI prefix that points to a suffix of itself), or when the server is under attack by a client attempting to exploit potential security holes.

> A 414 response is cacheable unless otherwise indicated by the method definition or explicit cache controls (see Section 4.1.2 of [Part6]).

### `method :var`

If you allow the HTTP method to be overridden (e.g. via the _X-HTTP-Method-Override_ header) then return the intended method; return `Transaction.request.method` otherwise.

Reference: [Google Data APIs](https://developers.google.com/gdata/docs/2.0/basics#DeletingEntry)

> If your firewall does not allow DELETE, then do an HTTP POST and set the method override header as follows: `X-HTTP-Method-Override: DELETE`.

### `safe_methods :var`

FIXME

### `create_methods :var`

FIXME

### `process_methods :var`

FIXME

### `implemented_methods :var`

Return a list of HTTP methods that are implemented by the system.

### `is_method_implemented : in`

Return TRUE if `Transaction.request.method` is in `implemented_methods :var`; return FALSE otherwise.

### `implemented_content_headers :var`

Return a list of Content-* headers that are implemented by the system.

### `are_content_headers_implemented : in`

Return TRUE if Content-* headers match `implemented_content_headers :var`; return FALSE otherwise

### `is_functionality_implemented :bin`

Return TRUE if the requested functionality (other than methods and content headers) is implemented; return FALSE otherwise.

Reference: [HTTPbis](http://tools.ietf.org/html/draft-ietf-httpbis-p2-semantics-22#section-6.6.2), [RFC2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.5.2)

> The 501 (Not Implemented) status code indicates that the server does not support the functionality required to fulfill the request.  This is the appropriate response when the server does not recognize the request method and is not capable of supporting it for any resource.

> A 501 response is cacheable unless otherwise indicated by the method definition or explicit cache controls (see Section 4.1.2 of [Part6]).

### `implemented_expect_extensions :var`

Return a list of Expect extensions that are implemented by the system.

### `are_expect_extensions_implemented : in`

Return True if expect extensions match `implemented_expect_extensions :var`; return FALSE otherwise

Reference: [HTTPbis](http://tools.ietf.org/html/draft-ietf-httpbis-p2-semantics-22#section-6.5.14), [RFC2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.4.18)

> The 417 (Expectation Failed) status code indicates that the expectation given in the request's Expect header field (Section 5.1.1) could not be met by at least one of the inbound servers.

### `is_system_block_ok :bin`

FIXME

### `last : in`

FIXME

### `camelcase_response_headers :var`

FIXME

### `need_camelcased_response_headers :bin`

FIXME

### `override :bin`

Last chance for forcefully ammending the response of the *Transaction*.

Return TRUE if succeeded; return FALSE otherwise.

### `finish : in`

Finalize *Transaction*.
