---
title: Intercepting HTTP Requests with Proxyman
author: Onur Genes
date: 2019-11-24 20:04:00 +0300
tags: [network, debugging]
categories: [Network]
math: false
toc: true
image: 
    src: /assets/img/proxyman-header.png
    alt: Proxyman Logo
---

As a developer, whether you are mobile or web developer, most of the time we are dealing with **HTTP or HTTPS requests**. Mostly because of getting data from remote server.

But as you might already know, it can be really annoying because,

- Returning response can be wrong or not like as we expected or
- We are sending wrong request body, header, etc.

After trying for hours probably you are seeking for a better solution. This is the time when you need a HTTP Debugging Proxy.

## What is "HTTP Debugging Proxy"?

**HTTP Debugging Proxy** is a kind of tool which let's you to observe and manipulate HTTP/HTTPS requests.

## What is the solution?

The soultion is simple. Get an **HTTP Debugging Proxy** then watch your requests and responses or mainuplate them on the fly.

## Tools

There are few HTTP Debugging Proxies out there, like:

- [Proxyman](https://proxyman.io),
- [Fiddler](https://www.telerik.com/fiddler),
- [Charles Web Proxy](https://www.charlesproxy.com/),
- and many more. [Just Google it](https://lmgtfy.com/?q=HTTP+Debugging+Proxy).

## Why [Proxyman](https://proxyman.io)?

First of all, this post is not affiliated. I wish it was but I am writing this just for giving you an insight.

After that, I am using Proxyman because:

- It looks new and shiny,
- It is really easy to use,
- It has active development,
- I am using MacOS for development (if you are using Windows, you should look for alternatives),
- It is native MacOS App.

## Let's Get Started

First of all go to [Proxyman Website](https://proxyman.io) and download the free version.

I will be demonstrate debugging an iOS App but you can use it for every HTTP request from any app.

After installing Proxyman, you can see your requests:

![Requests](/assets/img/proxyman_intro.png)

But for debugging HTTPS responses, you need to install **root certificate** of Proxyman.

## Installing Certificates

You can click on "Certificate" tab at status bar:

![Installing Certificate](/assets/img/proxyman_certificate.png)

After clicking "Install Certificate iOS Devices..." certificate window will pop up:

![Setup Proxy](/assets/img/proxyman_rootcert.png)

First click on "Install Root Certificate", then go to your device.

Open Settings -> WiFi -> Current Wifi -> Configure Proxy and configure it like this:

![Setup iOS Proxy](/assets/img/proxyman_setupproxy.gif)

Then go to Safari and visit "**http://proxy.man/ssl**". If you correctly setup proxy, you should see something like this:

![Setup iOS Certificate](/assets/img/proxyman_ioscert.png)

Allow this. Then go to your settings and finish installing profile.

When you download and "Install Profile", you are ready to have some fun!

## Let's Take a Look At [BTC Tracker](https://apps.apple.com/tr/app/btc-tracker-crypto-coin-rates/id1328815527) App

[BTC Tracker](https://apps.apple.com/tr/app/btc-tracker-crypto-coin-rates/id1328815527) is an app where you can check Crypto Currency prices.

The developer of this app is, [me](https://onurgenes.com). Therefore it is not a problem to check it out.

It is using [CryptoCompare](https://www.cryptocompare.com/) API.

First time you click on the API address on the left panel you can't see the response. Because it is HTTPS response. You have to enable it by clicking this button:

![Enable Proxy](/assets/img/proxyman_firsttime.png)

After clicking the button, you have to restart the app or request. Now you can see everything.

This is the **Headers** of request:

![Headers](/assets/img/proxyman_headers.png)

And the **Body**:

![Body](/assets/img/proxyman_body.png)

# Wrapping Up

That was a basic introduction to HTTP Debugging Proxies and [Proxyman](https://proxyman.io).

This is a really powerful tool and you can intercept or debug any HTTP request even if it is not your app (but you probably shouldn't do that).

If you have any questions, feel free to get in touch.