---
title: "NSTextField 宽高自适应"
subtitle: "NSTextField 宽高根据字体和内容自动调整尺寸"
layout: post
author: "Yang"
header-style: text
tags:
  - macOS
  - Swift
---

需求：在修改 NSTextField 的内容和字体的时候，NSTextField 的宽高自适应。开始的想法是计算它的文本宽高，然后根据计算的结果来设置 frame，这个想法是从 iOS 开发上来的，用的方法是 NSString 的 ，这个计算出来的结果会明显偏小，导致文本内容显示不全。

```swift
func boundingRect(with size: NSSize, 
          options: NSString.DrawingOptions = [], 
       attributes: [NSAttributedString.Key : Any]? = nil, 
          context: NSStringDrawingContext?) -> NSRect
```

后面在查看 NSTextField 的文档时，发现它的父类 NSControl 有一个方法计算size的，这个计算出的结果就比较符合预期

```swift
func sizeThatFits(_ size: NSSize) -> NSSize
```

计算的时候，先设置好 NSTextField 的文本内容和字体，然后传入最大的 width 和 height。

```swift
let label = NSTextField()
label.stringValue = "100"
label.font = NSFont(name: "PingFang SC", size: NSFont.systemFontSize)
let size = resultLabel.sizeThatFits(NSSize(width: 200, height: 200))

label.frame = NSRect(x: label.frame.origin.x, y: label.frame.origin.y, width: size.width, 	height: size.height)
```

