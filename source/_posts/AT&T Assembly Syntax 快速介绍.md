---
title: AT&T Assembly Syntax 快速介绍
urlname: AT&T-syntax-quick-n-dirty
date: 2021-05-09 19:56:48
update: 2021-05-09 19:56:48
tags:
  - Assembly
  - AT&T
categories: Assembly
---

x86 汇编语言具有两个主要的语法分支：Intel 语法和 AT＆T 语法。

自从 Unix 由 AT＆T 贝尔实验室创建以来，Intel 语法在 DOS 和 Windows 世界中占主导地位，而 AT＆T 语法在 Unix 世界中占主导地位。

以下内容翻译自 [AT&T Assembly Syntax](http://www.sig9.com/articles/att-syntax)

本文是对 AT＆T 汇编语言语法的“快速而凌乱”的介绍，该语法在 GNU 汇编器 as（1）中实现。
第一次使用 AT＆T 语法可能有点令人困惑，但是如果您有任何汇编语言编程背景，一旦记住一些规则，就很容易赶上。
我假设您已经熟悉 x86 手册中描述的汇编语言指令的 INTEL 语法。
由于其简单性，我使用了 INTEL 语法的 NASM（Netwide 汇编程序）变体来引用两种格式之间的差异。

GNU 汇编器是 GNU Binary Utilities（binutils）的一部分，并且是 GNU Compiler Collection 的后端。
尽管不是编写相当大的汇编程序的首选汇编程序，但它是当代 Unix 类系统（尤其是内核级黑客程序）的重要组成部分。
人们常常批评它的 AT＆T 式语法含糊不清，认为它的编写着重于用作 GCC 的后端，而很少关注“开发者友好性”。
如果您是来自 INTEL-Syntax 背景的汇编程序员，那么您将在代码可读性和代码生成方面遇到种种窒息。
尽管如此，必须指出的是，许多操作系统的代码库都依赖于作为生成低级代码的汇编程序。

<!-- more -->

## The Basic Format （基本格式）

`mnemonic source, destination`

(助记符) 源操作数，目标操作数

## Registers （寄存器）

IA-32 架构的所有寄存器名称都必须以“％”符号作为前缀，例如：％al，％bx，％ds，％cr0 等。

`mov %ax, %bx`

mov 指令，它将值从 16 位寄存器 AX 移动到 16 位寄存器 BX。

## Literal Values （字面值）

所有字面值都必须以“$”符号作为前缀。

`mov $100, %bx`
`mov $A, %al`

第一条指令将值 100 移动到寄存器 AX 中，第二条指令将 ascii A 的数值移动到 AL 寄存器中。

## Memory Addressing （内存寻址）

在 AT＆T 语法中，通过以下方式引用内存：

segment-override:signed-offset(base,index,scale)

根据您想要的地址，可以省略其中的一部分。

`%es:100(%eax,%ebx,2)`

请注意，偏移量和比例尺不应以“ $”作为前缀。

等效的 NASM 语法示例：

| GAS memory operand | NASM memory operand |
| :----------------- | :------------------ |
| 100                | [100]               |
| %es:100            | [es:100]            |
| (%eax)             | [eax]               |
| (%eax,%ebx)        | [eax+ebx]           |
| (%ecx,%ebx,2)      | [ecx+ebx*2]         |
| (,%ebx,2)          | [ebx*2]             |
| -10(%eax)          | [eax-10]            |
| %ds:-10(%ebp)      | [ds:ebp-10]         |

`mov %ax, 100`
`mov %eax, -100(%eax)`

第一条指令将寄存器 AX 中的值移动到数据段寄存器的偏移量 100 中（默认情况下），第二条指令将 eax 寄存器中的值移动到[eax-100]中。

## Operand Sizes (操作数大小)

有时，尤其是将字面值移动到内存时，有必要指定传输大小或操作数大小。

例如指令：

`mov $10, 100`

仅指定将值 10 移动到内存偏移量 100，而不指定传输大小。

指定传输大小或操作数大小：

- 在 NASM 中，这是通过将强制转换关键字 byte / word / dword 等添加到任何操作数来完成的；

- 在 AT＆T 语法中，这是通过在指令后添加后缀 b / w / l 来完成的。

例如：

`movb $10, %es:(%eax)`

将字节值 10 移动到存储位置[es：eax]，但是，

`movl $10, %es:(%eax)`

将长值（dword）10 移动到同一位置。

再举几个例子:

```Assembly
movl $100, %ebx
pushl %eax
popw %ax
```

## Control Transfer Instructions （控制转移指令）

jmp,call,ret 等指令跳转控制代码从一个部分跳到另一个部分，它可以被归类为相同代码段的跳转(near)或者不同代码内（far）的中转。分去地址可能的类型为

`jmp`, `call`, `ret`等指令将控制权从程序的一部分转移到另一部分。
它们可以归类为到同一代码段（近）或到不同代码段（远）的控制传输。

分支寻址的可能类型为–相对偏移量（标签），寄存器，内存操作数和段偏移量指针。

相对偏移量使用标签指定，如下所示。

```Assembly
label1:
        .
        .
  jmp   label1
```

使用寄存器或存储器操作数的分支寻址必须以“\*”为前缀。

要指定“远”控制转移，必须在“ljmp”，“lcall”等中加上“l”作为前缀。例如，

| GAS syntax     | NASM syntax     |
| :------------- | :-------------- |
| jmp \*100      | jmp near [100]  |
| call \*100     | call near [100] |
| jmp \*%eax     | jmp near eax    |
| jmp \*%ecx     | call near ecx   |
| jmp \*(%eax)   | jmp near [eax]  |
| call \*(%ebx)  | call near [ebx] |
| ljmp \*100     | jmp far [100]   |
| lcall \*100    | call far [100]  |
| ljmp \*(%eax)  | jmp far [eax]   |
| lcall \*(%ebx) | call far [ebx]  |
| ret            | retn            |
| lret           | retf            |
| lret $0x100    | retf 0x100      |

段偏移量指针使用以下格式指定：

`jmp $segment, $offset`

示例：

`jmp $0x10, $0x100000`
