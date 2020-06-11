# Authentication and Authorization

## HTTP Basic Auth

Basic Auth (HTTP/1.0) is an authorization type that requires a verified username and password to access a data resource.

In Request headers, the value of Authorization: Basic ***** is username:password encoded by base64.

## HTTP Digest Auth

Refer to [https://www.jianshu.com/p/78faeb3a90e6](https://www.jianshu.com/p/78faeb3a90e6)

Digest Auth (HTTP/1.1) is an application of [MD5](https://en.wikipedia.org/wiki/MD5 "MD5")  [cryptographic hashing](https://en.wikipedia.org/wiki/Cryptographic_hash "Cryptographic hash") with usage of [nonce](https://en.wikipedia.org/wiki/Cryptographic_nonce "Cryptographic nonce") values to prevent replay attacks. It uses the HTTP protocol.

In a digest authentication flow, the client sends a request to a server, which sends back  **nonce**  and  **realm**  values for the client to authenticate. The client sends back a hashed username and password with the nonce and realm. The server then sends back the requested data.

```
HA1 = MD5( "Mufasa:testrealm@host.com:Circle Of Life" )
    = 939e7578ed9e3c518a452acee763bce9`

HA2 = MD5( "GET:/dir/index.html" )
    = 39aff3a2bab6126f332b942af96d3366
`Response = MD5( "939e7578ed9e3c518a452acee763bce9:\`

`dcd98b7102dd2f0e8b11d0f600bfb0c093:\`

`00000001:0a4f113b:auth:\`

`39aff3a2bab6126f332b942af96d3366" )`

`= 6629fae49393a05397450978507c4ef1`

```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTcwMzMxNzYyMywtMzQxMjI4NzNdfQ==
-->