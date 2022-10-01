---
title: Polly
urlname: label
tags:
  - DotNET
categories: DotNET
---

# Polly

Polly 是一个 .NET 恢复和瞬态故障处理库，它允许开发人员以流畅且线程安全的方式表达策略，如重试、断路器、超时、隔板隔离和回退。

Polly 的目标：.NET Standard 1.1（覆盖范围：.NET Core 1.0、Mono、Xamarin、UWP、WP8.1+）和 .NET Standard 2.0+（覆盖范围：.NET Core 2.0+、.NET Core 3.0 以及更高版本的 Mono、Xamarin 和 UWP 目标）。nuget 包还包括 .NET Framework 4.6.1 和 4.7.2 的直接目标。

[GitHub](https://github.com/App-vNext/Polly "Polly")

通过 www.thepollyproject.org 随时了解新功能公告，提示和技巧以及其他新闻

## Installing via NuGet

```shell
Install-Package Polly
```

## 瞬态故障处理和主动弹性工程

Transient fault handling and proactive resilience engineering

当今基于云的、基于微服务的或物联网的应用程序通常依赖于通过不可靠的网络与其他系统进行通信。

由于暂时性故障（如网络问题和超时），或者子系统处于脱机状态、负载不足或其他无响应，此类系统可能不可用或无法访问。

Polly 提供的弹性策略提供了一系列用于处理暂时性故障的反应策略，以及用于促进弹性和稳定性的先发制人和主动策略。

### 使用重试和断路器处理瞬态故障

Handling transient faults with retry and circuit-breaker

暂时性故障是其原因预计为临时情况（如临时服务不可用或网络连接问题）的错误。

#### Retry

重试允许调用方重试操作，期望许多故障是暂时的，并且可能会自我纠正：如果重试，操作可能会成功，可能在短暂的延迟之后。

在重试之间等待可以让故障有时间进行自我纠正。指数退避和抖动等做法通过安排重试来优化这一点，以防止它们成为进一步负载或峰值的来源。

#### Circuit-Breaker

断路器检测通过它发出的呼叫中的故障级别，并在超过可配置的故障阈值时阻止调用。
