---
title: モデルの作成と構成 - EF Core
description: Entity Framework Core を使用したモデルの作成と構成の概要
author: rowanmiller
ms.date: 11/05/2019
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: e980f11b08bee7b07156a80c6bead829e7a8b654
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/09/2020
ms.locfileid: "89616759"
---
# <a name="creating-and-configuring-a-model"></a><span data-ttu-id="e3118-103">モデルの作成と構成</span><span class="sxs-lookup"><span data-stu-id="e3118-103">Creating and configuring a model</span></span>

<span data-ttu-id="e3118-104">Entity Framework では、一連の規則を利用し、エンティティ クラスの形状に基づいてモデルがビルドされます。</span><span class="sxs-lookup"><span data-stu-id="e3118-104">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="e3118-105">規則にあるものを捕捉する/オーバーライドする目的で、追加構成を指定できます。</span><span class="sxs-lookup"><span data-stu-id="e3118-105">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="e3118-106">この記事では、あらゆるデータ ストアをターゲットにするモデルに適用できる構成、あらゆるリレーショナル データベースをターゲットにするときに適用できる構成について取り上げます。</span><span class="sxs-lookup"><span data-stu-id="e3118-106">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="e3118-107">プロバイダーも、特定のデータ ストアに固有の構成を有効にできます。</span><span class="sxs-lookup"><span data-stu-id="e3118-107">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="e3118-108">プロバイダー固有の構成については、「 [データベース プロバイダー](xref:core/providers/index) 」セクションを参照してください。</span><span class="sxs-lookup"><span data-stu-id="e3118-108">For documentation on provider specific configuration see the [Database Providers](xref:core/providers/index) section.</span></span>

> [!TIP]  
> <span data-ttu-id="e3118-109">この記事の [サンプル](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples) は GitHub で確認できます。</span><span class="sxs-lookup"><span data-stu-id="e3118-109">You can view this article’s [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="use-fluent-api-to-configure-a-model"></a><span data-ttu-id="e3118-110">fluent API を使用してモデルを構成する</span><span class="sxs-lookup"><span data-stu-id="e3118-110">Use fluent API to configure a model</span></span>

<span data-ttu-id="e3118-111">派生コンテキストで  `OnModelCreating`  メソッドをオーバーライドし、 `ModelBuilder API` を使用してモデルを構成できます。</span><span class="sxs-lookup"><span data-stu-id="e3118-111">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="e3118-112">これは最も強力な構成方法であり、エンティティ クラスを変更しなくても構成を指定できます。</span><span class="sxs-lookup"><span data-stu-id="e3118-112">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="e3118-113">Fluent API 構成には一番上の優先度が与えられ、規則やデータ注釈をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="e3118-113">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=12-14)]

## <a name="use-data-annotations-to-configure-a-model"></a><span data-ttu-id="e3118-114">データ注釈を使用してモデルを構成する</span><span class="sxs-lookup"><span data-stu-id="e3118-114">Use data annotations to configure a model</span></span>

<span data-ttu-id="e3118-115">クラスやプロパティに属性 (データ注釈と呼ばれています) を適用することもできます。</span><span class="sxs-lookup"><span data-stu-id="e3118-115">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="e3118-116">データ注釈は規則をオーバーライドしますが、Fluent API 構成で上書きされます。</span><span class="sxs-lookup"><span data-stu-id="e3118-116">Data annotations will override conventions, but will be overridden by Fluent API configuration.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=15)]
