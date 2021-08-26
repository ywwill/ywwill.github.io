---
title: "macOS 删除 UserDefaults 数据"
subtitle: "删除 macOS 中使用 UserDefaults 保存的数据"
layout: post
author: "Yang"
header-style: text
tags:
  - macOS
---

UserDefaults 是保存应用首选项，设置相关，简单数据类型的工具，在开发过程中可能需要多次删除或重置它。

在 iOS 中，如果想删除应用程序 使用 UserDefaults 保存的相关设置，只需删除应用程序即可。 但 macOS 和 Catalyst 应用程序并非如此。

要在 macOS/Catalyst 上删除 UserDefaults 保存的数据，需要 default 命令。

**使用 default 命令删除 UserDefaults 数据意可以快速重置，而无需使用 removeObject(forKey:) 方法**

UserDefaults 有默认属于它的 domain，使用 default 命令删除 UserDefaults 数据需要知道是如何初始化 UserDefaults 的

(1)、第一种方式，默认初始化方法，这种方式系统会使用 APP 的  **bundle id** 作为 domain

```bash
UserDefaults.standard.set("Alice", forKey: "userName")
```

(2)、第二种方式，自定义 UserDefaults，suiteName 会被作为 domain。

如下，domain 是 "Custom"

```bash
UserDefaults(suiteName: "Custom").set("Alice", forKey: "userName")
```

**UserDefaults 删除命令**

打开终端，用以下的命令进行删除

```bash
// 第一种方式, UserDefaults 通过 默认初始化后设置的数据
// 若 APP 的  bundle id 为 "app.bundle.id"

defaults delete app.bundle.id

// 第二种方式，UserDefaults 使用自定义 domain 初始化后设置的数据

defaults delete Custom
```

