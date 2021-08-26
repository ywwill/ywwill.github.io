---
title: "macOS SwiftUI 偏好设置"
subtitle: "macOS 中快捷键和点击显示 SwiftUI 偏好设置两种方式"
layout: post
author: "Yang"
header-style: text
tags:
  - macOS
  - SwiftUI
  - Swift
---

在 SwiftUI 中，可以通过新建一个 SwiftUI 文件来定义APP的入口，SwiftUI 有提供一个 Settings 的 View，可以直接导航至APP 的偏好设置，PreferencesView 是自定义的偏好设置的 SwiftUI 的 View，这个时候通过快捷键 “**⌘,**”  便可以进入偏好设置页面，使用这个 Settings 确实可以提供一些便利，但是也会出现一些意想不到的问题，这个应该是 SwiftUI 的问题，希望以后会优化吧。

```swift
import SwiftUI
@main
struct MainAPP: App {
    @NSApplicationDelegateAdaptor(AppDelegate.self) var appDelegate   
    var body: some Scene {
        Settings { // 偏好设置，快捷键 "cmd + ,"
            PreferencesView()
        }
    }
}
```

但是如果需要在其他点击事件时显示偏好设置页面，如果通过把 PreferencesView 加载到 NSPanel 然后显示，这种显示的效果和上面快捷键打开展示的效果有差异。

```swift
let panel = NSPanel(contentViewController: NSHostingController(rootView: PreferencesView()))
panel.styleMask = [.closable, .titled, .fullSizeContentView]
panel.titleVisibility = .hidden // 隐藏标题
panel.titlebarAppearsTransparent = true // titlebar 透明
panel.level = .mainMenu
panel.makeKeyAndOrderFront(nil)
```



这个时候可以通过以下的方式，这种方式打开的偏好设置会和上面快捷键打开的页面是一致的，已经通过苹果APP的审核，没有因为这句代码被拒。

```swift
NSApp.sendAction(Selector(("showPreferencesWindow:")), to: nil, from: nil)
```

