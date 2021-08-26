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

NSStatusItem 有个 button 属性，可以考虑在这个 button 上实现想要的效果。

- 可以获取到这个 button 按钮属性

- 然后设置 CABasicAnimation 动画，时长，重复次数，以及旋转角度的范围
- 设置 button.layer 的 position 和 锚点 anchorPoint，最后 layer 加上动画

实现过程如下：

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

