---
title: "NSImage 平均色"
subtitle: "获取 NSImage 图片的平均色"
layout: post
author: "Yang"
header-style: text
tags:
  - macOS
  - Swift
---

之前有这么一个需求：

需要获取到当前 macOS 系统活跃的应用程序，保存应用程序的相关属性到数据库中。当展示监听记录的时候，需要显示应用程序的 APP icon，列表的 cell 背景色为 APP icon 的平均色，以颜色来区分记录。

简单实现了一下，使用 CIFilter 的 "CIAreaAverage"，可以获取到平均色，但是获取到的颜色还是有点不是很满意，网上也没找到其他的解决方案。

```swift
extension NSImage {
    
    var averageColor: NSColor? {
        
        // 图片不可用
        if !isValid { return nil }
        
        // 创建 CGImage
        var imageRect = CGRect(
            x: 0,
            y: 0,
            width: self.size.width,
            height: self.size.height
        )
        
        let cgImageRef = self.cgImage(
            forProposedRect: &imageRect,
            context: nil,
            hints: nil
        )
        
        // 创建 CIVector
        let inputImage = CIImage(cgImage: cgImageRef!)
        let extentVector = CIVector(
            x: inputImage.extent.origin.x,
            y: inputImage.extent.origin.y,
            z: inputImage.extent.size.width,
            w: inputImage.extent.size.height
        )
        
        // CIFilter 过滤器
        let filter = CIFilter(
            name: "CIAreaAverage",
            parameters: [
                kCIInputImageKey: inputImage,
                kCIInputExtentKey: extentVector
            ]
        )
        
        let outputImage = filter!.outputImage!
        
        var bitmap = [UInt8](repeating: 0, count: 4)
        let context = CIContext(options: [.workingColorSpace: kCFNull!])
        context.render(
            outputImage,
            toBitmap: &bitmap,
            rowBytes: 4,
            bounds: CGRect(x: 0, y: 0, width: 1, height: 1),
            format: .RGBA8,
            colorSpace: nil
        )
        
        return NSColor(
            red: CGFloat(bitmap[0]) / 255,
            green: CGFloat(bitmap[1]) / 255,
            blue: CGFloat(bitmap[2]) / 255,
            alpha: CGFloat(bitmap[3]) / 255
        )
    }
}
```

