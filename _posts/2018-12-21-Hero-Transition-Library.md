---
title: Hero Transitions with Xcode 10 and Swift 4.2
author: Onur Genes
date: 2018-12-21 11:02:00 +0300
tags: [ios, autolayout, animation]
categories: [iOS, AutoLayout]
math: false
toc: true
permalink: /posts/hero-transitions-with-xcode-10-and-swift-4-2
---


<figure>
	<blockquote>
		<p>“Hero is a library for building iOS view controller transitions. It provides a declarative layer on top of the UIKit’s cumbersome transition APIs — making custom transitions an easy task for developers.”</p>
		<footer>
			<cite>—Hero’s GitHub ReadMe</cite>
		</footer>
	</blockquote>
</figure>

I’ve tried searching on Google so many times but couldn’t find any up to date tutorial. I’ve decided to make my own tutorial. So, here we go.

## Let’s Start

I am passing the installation stuff because it’s really well explained on the Hero’s documentation pages. Insted we will start with code. I am saying code because I am not using Storyboards for almost a year. (Thanks to <a href="https://twitter.com/buildthatapp" target="_blank">@buildthatapp</a>)

## Creating Simple UI (Without Storyboard)

After the “pod install” and “open myapp.xcworkspace” first thing we will do is deleting “Main.storyboard” from Targets -> General -> Deployment Info -> Main Interface.

![Deleted "Main Interface" field](/assets/img/1.png)

If we run the app, it will show black screen. Because we don’t have UIWindow yet. (Actually it is ‘nil’ now.)

So let’s figure that out.

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        
        window = UIWindow(frame: UIScreen.main.bounds)
        window?.makeKeyAndVisible()
        
        let viewController = ViewController()
        let navigationController = UINavigationController(rootViewController: vc)
        navigationController.hero.isEnabled = true // Just add that. We will explain that later.
        window?.rootViewController = navigationController
        
        return true
}
```

1. Simply, we created **UIWindow** and made it main screen.
2. Created **viewController**'s instance.
3. Created a **navigationController** because we will need that later.
4. **navigationController**'s **hero** is enabled. (I will explain that later.)
5. Last but not least, from now on, our **rootViewController** is **navigationController**

## Creating First ViewController

We will create a UIView with `UIColor.green` backgroundColor.

```swift
lazy var greenView: UIView = {
  let v = UIView()
  v.backgroundColor = .green
  return v
}()
```

We will enable hero on **ViewController()**:
```swift
self.hero.isEnabled = true
```
Next, give our greenView, hero.id:
```swift
greenView.hero.id = "greenView"
```

Let’s say we want to use **UINavigationController**. We need to enable **hero** on **navigationController** too (did you remember the `AppDelegate.swift`?):

```swift
// AppDelegate.swift
navigationController.hero.isEnabled = true
```

And lastly, added **UITapGestureRecognizer** for **push**ing **UIViewController** to **UINavigationController**.

Currently I am using **“SteviaLayout”** for **AutoLayout**. It’s worth checking out.

Our **ViewController** class looking like this:

```swift
//
//  ViewController.swift
//  learnherotransitions
//
//  Created by Onur Geneş on 29.06.2018.
//  Copyright © 2018 Onur Geneş. All rights reserved.
//
import UIKit
import Stevia
import Hero

class ViewController: UIViewController {
    
    lazy var greenView: UIView = {
        let v = UIView()
        v.backgroundColor = .green
        return v
    }()

    override func viewDidLoad() {
        super.viewDidLoad()
        
        greenView.hero.id = "greenView"
        
        greenView.addGestureRecognizer(UITapGestureRecognizer(target: self, action: #selector(openSecondVC)))
        
        view.backgroundColor = .white
        
        view.sv(
            greenView
        )
        
        greenView.height(50).width(50).centerInContainer()
    }
    
    @objc func openSecondVC() {
        let nc = UINavigationController(rootViewController: SecondViewController())
        nc.hero.isEnabled = true
        self.navigationController?.pushViewController(SecondViewController(), animated: true)
    }
    
}
```

Next, we will create SecondViewController very similarly;

1. Create a new **greenView**
2. Give a **hero.id**
3. We will enable hero with **self.hero.isEnabled = true**
4. Add UITapGestureRecognizer for **pop**ing **UIViewController** from **UINavigationController**

```swift
//
//  SecondViewController.swift
//  learnherotransitions
//
//  Created by Onur Geneş on 29.06.2018.
//  Copyright © 2018 Onur Geneş. All rights reserved.
//
import UIKit
import Stevia
import Hero

class SecondViewController: UIViewController {
    
    lazy var greenView: UIView = {
        let v = UIView()
        v.backgroundColor = .green
        return v
    }()

    override func viewDidLoad() {
        super.viewDidLoad()
        
        self.hero.isEnabled = true
        greenView.hero.id = "greenView"
        
        view.backgroundColor = .blue
        
        greenView.addGestureRecognizer(UITapGestureRecognizer(target: self, action: #selector(popVC)))

        view.sv(
            greenView
        )
        
        
        greenView.height(150).width(150).centerHorizontally().centerVertically(300)
        
    }
    
    @objc func popVC() {
        self.navigationController?.popViewController(animated: true)
    }
}
```

And we’re done.

## Let’s Run, Finally

Let’s run the app and see our final result:

![Final Result](/assets/img/2.gif)

Let me know what you think!