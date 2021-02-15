---
title: New iOS 13 Project Without Storyboard
author: Onur Genes
date: 2019-11-26 20:04:00 +0300
tags: [storyboard, ios, autolayout]
categories: [iOS, AutoLayout]
math: false
toc: true
---

Some of the iOS Developers, including me, don't want to use Storyboards. We all may have different reasons but this is a need after all. But with iOS 13, we have new shiny **"SceneDelegate"** class. It is useful for multi window apps but it's shipping with storyborads on default.

# How to remove Storyboard?

- First, as usual, we need to remove "Main Interface" from `(Project Name on Left Panel) -> (Select Your Target) -> General -> Main Interface`.

![Remove Storyboard](/assets/img/scene_delegateremove_storyboard.png)

- Now change the line starting with `guard let _ = scene...` to:

```swift
guard let windowScene = scene as? UIWindowScene else { fatalError() }
```

- Add these lines to `func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions)` in `SceneDelegate` class:

```swift
window = UIWindow(windowScene: windowScene)
window?.rootViewController = ViewController()
window?.makeKeyAndVisible()
```

- Last thing is, go to `Info.plist` file:
  - Go into “Scene Configuration” > “Application Session Role” > “Item 0 (Default Configuration)”
  - Delete “Storyboard Name” row.

# Wrapping up

Well done! Now you got rid of Storyboards.

Enjoy your new project.