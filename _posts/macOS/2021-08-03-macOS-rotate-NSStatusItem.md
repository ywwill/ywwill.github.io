---
title: "旋转菜单栏的 NSStatusItem"
subtitle: "NSStatusItem 设置旋转动画"
layout: post
author: "Yang"
header-style: text
tags:
  - macOS
  - Swift
---

NSStatusItem 有个 button 属性，我们可以为这个 button 按钮设置动画，设置 button.layer 的 position 和 锚点 anchorPoint

```swift
guard let button = statusItem.button else { return }

let animation = CABasicAnimation(keyPath: "transform.rotation.z")
animation.fromValue = 0
animation.toValue = CGFloat.pi * 2
animation.duration = 0.2
animation.repeatCount = 1
button.layer?.position = NSPoint(x: NSMidX(button.frame), y: NSMidY(button.frame))
button.layer?.anchorPoint = NSPoint(x: 0.5, y: 0.5)
button.layer?.add(animation, forKey: "rotate")
```

