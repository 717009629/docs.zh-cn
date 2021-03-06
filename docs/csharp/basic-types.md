---
title: "基本类型 | C# 指南"
description: "了解所有 C# 程序中的核心类型（数字、字符串和对象）"
keywords: .NET, .NET Core, C#
author: stevehoag
ms.author: shoag
ms.date: 10/10/2016
ms.topic: article
ms.prod: .net
ms.technology: devlang-csharp
ms.devlang: csharp
ms.assetid: 95c686ba-ae4f-440e-8e94-0dbd6e04d11f
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 57c5524a18535778db337500b0a7e9013ce93519
ms.lasthandoff: 03/13/2017

---

# <a name="types-variables-and-values"></a>类型、变量和值  
C# 是一种强类型语言。 每个变量和常量都有一个类型，每个求值的表达式也是如此。 每个方法签名为每个输入参数和返回值指定一个类型。 .NET Framework 类库定义了一组内置数值类型以及更复杂的类型，这些类型代表各种各样的逻辑构造，例如文件系统、网络连接、对象的集合和数组以及日期。 典型的 C# 程序使用类库中的类型以及用户定义类型，这些类型对程序的问题域的特定概念进行建模。  
  
存储在类型中的信息可以包括以下内容：  
  
-   类型变量所需的存储空间。  
  
-   它可以表示的最大值和最小值。  
  
-   它包含的成员（方法、字段、事件等）。  
  
-   它所继承自的基类型。  
  
-   运行时分配变量内存的位置。  
  
-   允许的操作种类。  
  
编译器使用类型信息来确保在代码中执行的所有操作都是类型安全。** 例如，如果声明 [int](https://msdn.microsoft.com/library/5kzh1b5w.aspx) 类型的变量，则编译器允许在加法和减法运算中使用该变量。 如果尝试对 [bool](https://msdn.microsoft.com/library/c8f5xwh7.aspx) 类型的变量执行这些相同操作，则编译器将生成错误，如以下示例所示：  
  
[!code-csharp[类型安全](../../samples/snippets/csharp/concepts/basic-types/type-safety.cs)]  
  
> [!NOTE]  
>  C 和 C++ 开发人员请注意，在 C# 中，[bool](https://msdn.microsoft.com/library/c8f5xwh7.aspx) 不能转换为 [int](https://msdn.microsoft.com/library/5kzh1b5w.aspx)。  
  
编译器将类型信息作为元数据嵌入可执行文件中。 公共语言运行时 (CLR) 在运行时使用元数据，以在分配和回收内存时进一步保证类型安全性。  

## <a name="specifying-types-in-variable-declarations"></a>在变量声明中指定类型  
当在程序中声明变量或常量时，必须指定其类型或使用 [var](https://msdn.microsoft.com/library/bb383973.aspx) 关键字让编译器推断类型。 以下示例显示了一些使用内置数值类型和复杂用户定义类型的变量声明：  
  
[!code-csharp[变量声明](../../samples/snippets/csharp/concepts/basic-types/variable-declaration.cs)]  
  
方法签名指定方法参数的类型和返回值。 以下签名显示了需要 [int](https://msdn.microsoft.com/library/5kzh1b5w.aspx) 作为输入参数并返回字符串的方法：  
  
[!code-csharp[方法签名](../../samples/snippets/csharp/concepts/basic-types/method-signature.cs)]  
  
在声明变量后，不能使用新类型重新声明该变量，并且不能为其分配与其声明的类型不兼容的值。 例如，不能声明 [int](https://msdn.microsoft.com/library/5kzh1b5w.aspx)，然后为其分配为 [true](https://msdn.microsoft.com/library/06d3w013.aspx) 的布尔值。 但是，值可以转换为其他类型，例如，可以把它们分配给新变量或作为方法参数传递。 不会导致数据丢失的类型转换由编译器自动执行。** 可能导致数据丢失的转换需要在源代码进行强制转换。** 

有关详细信息，请参阅[强制转换和类型转换](https://msdn.microsoft.com/library/ms173105.aspx)。
 
## <a name="built-in-types"></a>内置类型
C# 提供了一组标准的内置数值类型来表示整数、浮点值、布尔表达式、文本字符、十进制值和其他数据类型。 另外，还有内置**字符串**和**对象**类型。 这些类型可供在任何 C# 程序中使用。 有关内置类型的详细信息，请参阅[类型参考表](https://msdn.microsoft.com/library/1dhd7f2x.aspx)。  
  
## <a name="custom-types"></a>自定义类型  
可以使用[结构](https://msdn.microsoft.com/library/ah19swz4.aspx)、[类](https://msdn.microsoft.com/library/0b0thckt.aspx)、[接口](https://msdn.microsoft.com/library/87d83y5b.aspx)，和[枚举](https://msdn.microsoft.com/library/sbbt4032.aspx)构造创建你自己的自定义类型。 .NET Framework 类库本身是由 Microsoft 提供的自定义类型的集合，你可以在自己的应用程序中使用它。 默认情况下，该类库中最常用的类型在任何 C# 程序中均可用。 其他类型仅在你将项目引用显式添加到定义它们的程序集时可用。 编译器引用程序集之后，你可以声明在源代码的此程序集中声明的类型的变量（和常量）。 有关详细信息，请参阅 [.NET Framework 类库](https://msdn.microsoft.com/library/gg145045(v=vs.110).aspx)。  
  
## <a name="generic-types"></a>泛型类型  
可使用一个或多个类型参数声明、作为客户端代码在创建类型实例时将提供的实际类型（具体类型）的占位符的类型。**** 这种类型称为泛型类型。** 例如，.NET Framework 类型 @System.Collections.Generic.List%601 具有一个类型参数，它按照惯例被命名为 *T*。当创建类型的实例时，指定列表将包含的对象的类型，例如字符串：  
  
[!code-csharp[泛型类型](../../samples/snippets/csharp/concepts/basic-types/generic-type.cs)] 
  
通过使用类型参数，可重新使用相同类以保存任意类型的元素，且无需将每个元素转换为[对象](https://msdn.microsoft.com/library/9kkx3h3c.aspx)。 通用集合类称为强类型集合，因为编译器知道集合元素的特定类型，并且可能在编译时引发错误，例如尝试向之前示例中的 `strings` 对象添加整数的情况。** 有关详细信息，请参阅[泛型](programming-guide/generics/index.md)。 

## <a name="implicit-types-anonymous-types-and-tuple-types"></a>隐式类型、匿名类型和元组类型  
如前所述，你可以使用 [var](https://msdn.microsoft.com/library/bb383973.aspx) 关键字隐式键入一个局部变量（但不是类成员）。 变量在编译时仍可接收类型，但类型由编译器提供。 有关详细信息，请参阅[隐式类型本地变量](https://msdn.microsoft.com/library/bb384061.aspx)。  
  
在某些情况下，为不打算存储或传递外部方法边界的简单相关值集合创建命名类型是不方便的。 为此，你可以创建匿名类型。** 有关详细信息，请参阅[匿名类型](https://msdn.microsoft.com/library/bb397696.aspx)。

经常需要从方法返回多个值。 可以创建在单个方法调用中返回多个值的元组类型。** 有关详细信息，请参阅[元组](tuples.md)

## <a name="the-common-type-system"></a>通用类型系统  
了解 .NET Framework 中的类型系统的两个基本点非常重要：  
  
-   它支持继承原则。 类型可以从其他类型派生，称为基类型。** 派生类型继承（有一些限制）基类型的方法、属性和其他成员。 基类型可以继而从某种其他类型派生，在这种情况下，派生类型继承其继承层次结构中的两种基类型的成员。 所有类型（包括 @System.Int32 (C# keyword: `int`) 等内置数值类型）最终都派生自单个基类型，即 @System.Object (C# keyword: `object`)。 此统一类型层次结构称为[通用类型系统](../standard/common-type-system.md) (CTS)。 有关 C# 中的继承的详细信息，请参阅[继承](https://msdn.microsoft.com/library/ms173149.aspx)。  
  
-   CTS 中的每种类型被定义为值类型或引用类型。**** 这包括 .NET Framework 类库中的所有自定义类型以及你自己的用户定义类型。 使用 [struct](https://msdn.microsoft.com/library/ah19swz4.aspx) 关键字定义的类型是值类型；所有内置数值类型都是 **structs**。 有关值类型的详细信息，请参阅[结构](structs.md)。 使用 [class](https://msdn.microsoft.com/library/0b0thckt.aspx) 关键字定义的类型是引用类型。 有关引用类型的详细信息，请参阅[类](classes.md)。 引用类型和值类型具有不同的编译时规则和不同的运行时行为。
 
  
## <a name="see-also"></a>另请参阅
[结构](structs.md)

[类](classes.md)

