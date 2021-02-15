---
title: ASWebAuthenticationSession for Web Login in iOS App
author: Onur Genes
date: 2020-11-01 11:02:00 +0300
tags: [ios, network, web]
categories: [iOS, Network]
math: false
toc: true
image:
    src: /assets/img/aswebauth-header.png
    alt: ASWebAuth Image
---

Most of the time when you are building an app, you have to have some kind of login or sign in mechanism. Thankfully big providers like GitHub, Twitter or Google giving an easy way for this.

# Sign in with GitHub

You can easily add Sign in with GitHub to your account. Most of the time providers like [Twitter asks for a complicated process for implementing sign in methods](https://developer.twitter.com/en/docs/authentication/guides/log-in-with-twitter). For this type of situations, for example Twitter, you should consider using [an open source library](https://github.com/mattdonnelly/Swifter).

But perfect providers like GitHub made process easy to use and easy to implement.  Let's look at it.

## Create a ASWebAuthenticationSession

For starting:

```swift
import AuthenticationServices
...
...
class ViewController: UIViewController {
    var session: ASWebAuthenticationSession?
    
...
...
    func startSignIn() {
        let authURL = URL(string: "https://github.com/login/oauth/authorize?client_id=YOUR_CLIENT_ID")
        let scheme = "aswebauthdemo://auth"

        self.session = ASWebAuthenticationSession.init(url: authURL!, callbackURLScheme: scheme, completionHandler: { callbackURL, error in
            guard error == nil, let successURL = callbackURL else {
                return
            }

            let oauthToken = URLComponents(string: (successURL.absoluteString))?.queryItems?.filter({$0.name == "code"}).first

            print(oauthToken ?? "No OAuth Token")
        })
        
        session?.presentationContextProvider = self
        self.session?.start()
    }  
}
```

In this snippet we have built the `var session: ASWebAuthenticationSession?` and gave it a `callbackSchema` and URL for authentication.

Rest of the process handled by `ASWebAuthenticationSession` and it gave back the result with OAuth token in it.

Also do not forget to implement `ASWebAuthenticationPresentationContextProviding`. Otherwise you will not see the process. 

**Some developers trying to implement this method by giving it a `UIWindow` object _(Even official Apple documentations shows this)_ but you can't do that all the time. For example, in a SwiftUI 2 only app with no window.**

Just give it a `ASPresentationAnchor()` and it will work on all platforms.


```swift
extension ViewController: ASWebAuthenticationPresentationContextProviding {
    func presentationAnchor(for session: ASWebAuthenticationSession) -> ASPresentationAnchor {
        ASPresentationAnchor()
    }
}
```

And last thing is adding a URL Scheme matching with above. For this,
- Click the name of your project from `Project Navigator` on the left
- Click on `Info` tab
- Expand URL Types
- Click `+` and fill necessary info like below

![Screen-Shot-2020-11-05-at-18.31.22](/assets/img/aswebauth.png)

That's it. You just implemented an OAuth based Web Authentication.