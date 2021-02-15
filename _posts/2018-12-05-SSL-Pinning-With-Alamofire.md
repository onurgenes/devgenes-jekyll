---
title: SSL Pinning With Alamofire
author: Onur Genes
date: 2018-12-05 16:51:00 +0300
tags: [ios, alamofire, network, ssl]
categories: [iOS, Network]
math: false
toc: true
image:
    src: /assets/img/sslpinning-header.jpg
    alt: Keylocks
---

## First of all, what is SSL pinning?

According to [Wikipedia](https://wikipedia.org), SSL pinning is:

_**HTTP Public Key Pinning (HPKP) is an Internet security mechanism delivered via an HTTP header which allows HTTPS websites to resist impersonation by attackers using mis-issued or otherwise fraudulent certificates. In order to do so, it delivers a set of public keys to the client (browser), which should be the only ones trusted for connections to this domain.**_

Basically, SSL pinning prevents [Man In The Middle](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) attack. (You can make a Google search for learning more about this.)

# Let's start implementing with [Alamofire](https://github.com/Alamofire/Alamofire).

First of all we need to get SSL certificate from our server with:

```bash
openssl s_client -showcerts -connect yourdomain.com:443 < /dev/null | openssl x509 -outform DER > yourdomain.cer
```

_Change the `yourdomain.com` with your server's address._

We got our SSL certificate.

We need a `SessionManager` for making request with SSL pinning:

```swift
let sessionManager: SessionManager = {
    let serverTrustPolicies: [String: ServerTrustPolicy] = ["yourdomain.com": .pinCertificates(certificates: ServerTrustPolicy.certificates(),
                                                                                                validateCertificateChain: true,
                                                                                                validateHost: true)]
    
    return SessionManager(serverTrustPolicyManager: ServerTrustPolicyManager(policies: serverTrustPolicies))
}()
```

### The only thing you need to check is you should add your domain as `yourdomain.com`, not `https://yourdomain.com`.

Yes! We got the `Manager`!

Now we can make all our request from that `Manager`. If the certificate is not valid, we will get error from Alamofire.

You can test your implementation with:

```swift
sessionManager.request("https://yourdomain.com").responseString { (dataResponse) in
    switch dataResponse.result {
    case .failure(let err):
        print(err)
    case .success(let val):
        print(val)
        if let headerFields = dataResponse.response?.allHeaderFields {
            print(headerFields)
        }
    }
}
```

That's it. You succesfully implemented SSL Certificate pinning with Alamofire.