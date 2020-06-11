# Authentication and Authorization

## HTTP Basic Auth

Basic Auth (HTTP/1.0) is an authorization type that requires a verified username and password to access a data resource.

In Request headers, the value of Authorization: Basic ***** is username:password encoded by base64.

## HTTP Digest Auth

Refer to [https://www.jianshu.com/p/78faeb3a90e6](https://www.jianshu.com/p/78faeb3a90e6)

Digest Auth (HTTP/1.1) is an application of [MD5](https://en.wikipedia.org/wiki/MD5 "MD5")  [cryptographic hashing](https://en.wikipedia.org/wiki/Cryptographic_hash "Cryptographic hash") with usage of [nonce](https://en.wikipedia.org/wiki/Cryptographic_nonce "Cryptographic nonce") values to prevent replay attacks. It uses the HTTP protocol.

In a digest authentication flow, the client sends a request to a server, which sends back  **nonce**  and  **realm**  values for the client to authenticate. The client sends back a hashed username and password with the nonce and realm. The server then sends back the requested data.


<!--stackedit_data:
eyJoaXN0b3J5IjpbMTk3ODk1MjcwNywtMzQxMjI4NzNdfQ==
-->