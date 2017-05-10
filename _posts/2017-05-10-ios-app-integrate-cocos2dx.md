---
layout: page
title: iOS app integrate coocs2d-x
---

Some developers inegrated cocos2d-x into an existing iOS app. This blog will describe hwo to do it roughly. This blog will not write how to do step by step, just some high level concepts.

Suppose there is an existing iOS application, and it has a game channel. When clicking a game icon, it wants to load and play a cocos2d-x based game. Because cocos2d-x renders contents into a `UIView`(mentioned as game view following), so it is able to add game view as a sub view of the application, the codes may look like:

```
// pseudocode
view = new CCEAGLView
rootViewController = new RootViewController
rootViewController.view = view
navigateTo(rootViewController)

```

Can refer to [AppController.mm](https://github.com/cocos2d/cocos2d-x/blob/v3/tests/cpp-empty-test/proj.ios/AppController.mm) and [RootViewController.mm](https://github.com/cocos2d/cocos2d-x/blob/v3/tests/cpp-empty-test/proj.ios/RootViewController.mm) for detail information.

You may need to return from game to iOS app. You have to release memories uses by the game. So before returning from game, you should invoke `Director::end()` to release memories. Then you can return to parent view to back to the app.

If you use lua or JSB, you have to destroy script engine context too.