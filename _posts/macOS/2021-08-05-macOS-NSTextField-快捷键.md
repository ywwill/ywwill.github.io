---
title: "NSTextField 快捷键"
subtitle: "NSTextField 复制、粘贴、剪切、撤回快捷键"
layout: post
author: "Yang"
header-style: text
tags:
  - macOS
  - Swift
---

在 macOS 中，在 APP 中使用  ⌘C，⌘V，⌘Z，⌘X 这些基础的快捷键，这是因为菜单栏的“编辑”菜单中实现的功能，但有的时候 APP 没有菜单栏，比如 menu bar 类型的APP，并没有常见的菜单栏，在开发的时候就删除了。

![](/img/in-post/post_storyboard_menu.png)

这种情况下 NSTextField 输入框的快捷键功能就会消失，通过添加以下的 extension 实现快捷键功能。

```swift
extension NSTextField {
    open override var focusRingType: NSFocusRingType {
        get { .none }
        set { }
    }
    
    // NSTextField 快捷键支持
    open override func performKeyEquivalent(with event: NSEvent) -> Bool {
        if event.type == NSEvent.EventType.keyDown {
            if (event.modifierFlags.rawValue & NSEvent.ModifierFlags.deviceIndependentFlagsMask.rawValue) == NSEvent.ModifierFlags.command.rawValue {
                switch event.charactersIgnoringModifiers! {
                case "x":
                    if NSApp.sendAction(#selector(NSText.cut(_:)), to: nil, from: self) { return true }
                case "c":
                    if NSApp.sendAction(#selector(NSText.copy(_:)), to: nil, from: self) { return true }
                case "v":
                    if NSApp.sendAction(#selector(NSText.paste(_:)), to: nil, from: self) { return true }
                case "z":
                    if NSApp.sendAction(Selector(("undo:")), to: nil, from: self) { return true }
                case "a":
                    if NSApp.sendAction(#selector(NSResponder.selectAll(_:)), to: nil, from: self) { return true }
                    
                default:
                    break
                }
            }
        }
        
        return super.performKeyEquivalent(with: event)
    }
}
```
