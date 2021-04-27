---
title: JSON
date: 2021-04-27 17:24:04
tags:
---
## 介绍

JSON（JavaScript Object Notation）是一种简单的数据交换格式。
从句法上讲，它类似于JavaScript的对象和列表。
它最常用于Web后端和浏览器中运行的JavaScript程序之间的通信，但它也用于许多其他地方。

## Encoding

要编码JSON数据，我们使用 `Marshal` 函数。

`func Marshal(v interface{}) ([]byte, error)`

只有可以表示为有效JSON的数据结构才会被编码：

* JSON对象仅支持将字符串作为键；
  要编码 `Go map` 类型，它必须采用 `map [string] T` 的形式（其中 `T` 是 `json` 包支持的任何Go类型）。

* `Channel`, `complex`, and `function` types 无法编码。

* 不支持循环数据结构；
  它们将导致 `Marshal` 陷入无限循环

* 指针将被编码为其指向的值（如果指针为 `nil`，则为 `null`）。

* json包仅访问结构类型（以大写字母开头的结构类型）的导出字段。

  因此，在JSON输出中仅会显示结构的导出字段。

## Decoding

要解码JSON数据，我们使用 `Unmarshal` 函数。

`func Unmarshal(data []byte, v interface{}) error`

* `Unmarshal` 将仅解码它可以在目标类型中找到的字段。

  当您希望从大型 `JSON Blob` 中仅选择几个特定字段时，此行为特别有用。
  这也意味着目标结构中任何未导出的字段都不会受到 `Unmarshal` 的影响。

## `interface{}`

`interface {}`（空接口）类型描述了一个零方法接口。

每个Go类型至少实现零个方法，因此满足空接口。

类型断言访问基础的具体类型，或者，如果基础类型未知，则由类型开关确定类型：

    switch v := i.(type) {
    case int:
        fmt.Println("twice i is", v*2)
    case float64:
        fmt.Println("the reciprocal of i is", 1/v)
    case string:
        h := len(v) / 2
        fmt.Println("i swapped by halves is", v[h:]+v[:h])
    default:
        // i isn't one of the types above
    }

json包使用 `map [string] interface {}` 和 `[] interface {}` 值存储任意JSON对象和数组；

它将很乐意将任何有效的 `JSON Blob` 编组为纯接口{}值。

默认的具体Go类型是：

* `bool` for JSON `booleans`;

* `float64` for JSON `numbers`;

* `string` for JSON `strings`;

* `nil` for JSON `null`。

