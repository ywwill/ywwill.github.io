---
title: "NSTableView 移除 Column"
subtitle: "NSTableView 代码新建和删除列 Column"
layout: post
author: "Yang"
header-style: text
tags:
  - macOS
  - Swift
---

NSTableView 通过 *view*-based 创建，需求中列数是动态改变的，非静态的，需要通过 NSTableView 的 addTableColumn 方法手动添加。

在添加列前，先通过 removeTableColumn 移除所有的列，在移除过程中，出现了崩溃的问题，崩溃的原因如下。

NSTableCellView 和 NSTableRowView 的布局约束出现了问题。

**Unable to activate constraint with anchors <NSLayoutXAxisAnchor:0x60000303af40  "NSTableCellView.left" (names: NSTableCellView:0x7f9b034bc690)> and  <NSLayoutXAxisAnchor:0x600003ff06c0 "NSTableRowView:0x7f9b0359e690.left"> because they have no common ancestor.  Does the constraint or its anchors reference items in different view  hierarchies?  That's illegal.**

解决方案，移除列的时候倒序移除。

```swift
// 倒序移除
for column in tableView.tableColumns.reversed() {
  tableView.removeTableColumn(column)
}

tableView.reloadData()

// 新建 column
let keyColumn = NSTableColumn(identifier: NSUserInterfaceItemIdentifier("key"))
keyColumn.title = "key".localized
keyColumn.maxWidth = 300
keyColumn.minWidth = 100
tableView.addTableColumn(keyColumn)
datas.forEach { data in
               
	let column = NSTableColumn(identifier:NSUserInterfaceItemIdentifier("Identifier"))
	column.title = "column"
	column.maxWidth = 300
	column.minWidth = 100
	self.tableView.addTableColumn(column)
}
        
tableView.reloadData()
tableView.sizeToFit()
tableView.layout()
```


