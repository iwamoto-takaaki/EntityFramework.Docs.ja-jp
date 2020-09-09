---
title: 空間型のプロバイダーサポート-EF6
description: Entity Framework 6 の空間型のプロバイダーサポート
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
uid: ef6/fundamentals/providers/spatial-support
ms.openlocfilehash: 060d662aa8f03ea3510bd6b1fb7bdf904585efab
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/09/2020
ms.locfileid: "89615799"
---
# <a name="provider-support-for-spatial-types"></a><span data-ttu-id="5b258-103">空間型のプロバイダーサポート</span><span class="sxs-lookup"><span data-stu-id="5b258-103">Provider Support for Spatial Types</span></span>
<span data-ttu-id="5b258-104">Entity Framework は、DbGeography クラスまたは Dbgeography クラスを使用した空間データの操作をサポートします。</span><span class="sxs-lookup"><span data-stu-id="5b258-104">Entity Framework supports working with spatial data through the DbGeography or DbGeometry classes.</span></span> <span data-ttu-id="5b258-105">これらのクラスは、Entity Framework プロバイダーによって提供されるデータベース固有の機能に依存しています。</span><span class="sxs-lookup"><span data-stu-id="5b258-105">These classes rely on database-specific functionality offered by the Entity Framework provider.</span></span> <span data-ttu-id="5b258-106">すべてのプロバイダーが空間データをサポートするわけではありません。また、空間型アセンブリのインストールなどの追加の前提条件を持つプロバイダーもあります。</span><span class="sxs-lookup"><span data-stu-id="5b258-106">Not all providers support spatial data and those that do may have additional prerequisites such as the installation of spatial type assemblies.</span></span> <span data-ttu-id="5b258-107">サポートされている空間型のプロバイダーの詳細については、以下を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5b258-107">More information about provider support for spatial types is provided below.</span></span>  

<span data-ttu-id="5b258-108">アプリケーションでの空間型の使用方法に関する追加情報については、次の2つのチュートリアルを参照してください。1つは Code First 用、もう1つは Database First または Model First です。</span><span class="sxs-lookup"><span data-stu-id="5b258-108">Additional information on how to use spatial types in an application can be found in two walkthroughs, one for Code First, the other for Database First or Model First:</span></span>  

- [<span data-ttu-id="5b258-109">Code First の空間データ型</span><span class="sxs-lookup"><span data-stu-id="5b258-109">Spatial Data Types in Code First</span></span>](xref:ef6/modeling/code-first/data-types/spatial)  
- [<span data-ttu-id="5b258-110">EF デザイナーの空間データ型</span><span class="sxs-lookup"><span data-stu-id="5b258-110">Spatial Data Types in EF Designer</span></span>](xref:ef6/modeling/designer/data-types/spatial)  

## <a name="ef-releases-that-support-spatial-types"></a><span data-ttu-id="5b258-111">空間型をサポートする EF のリリース</span><span class="sxs-lookup"><span data-stu-id="5b258-111">EF releases that support spatial types</span></span>  

<span data-ttu-id="5b258-112">空間型のサポートは、EF5 で導入されました。</span><span class="sxs-lookup"><span data-stu-id="5b258-112">Support for spatial types was introduced in EF5.</span></span> <span data-ttu-id="5b258-113">ただし、EF5 空間型は、アプリケーションが .NET 4.5 を対象として実行されている場合にのみサポートされます。</span><span class="sxs-lookup"><span data-stu-id="5b258-113">However, in EF5 spatial types are only supported when the application targets and runs on .NET 4.5.</span></span>  

<span data-ttu-id="5b258-114">EF6 空間型以降では、.NET 4 と .NET 4.5 の両方を対象とするアプリケーションでサポートされています。</span><span class="sxs-lookup"><span data-stu-id="5b258-114">Starting with EF6 spatial types are supported for applications targeting both .NET 4 and .NET 4.5.</span></span>  

## <a name="ef-providers-that-support-spatial-types"></a><span data-ttu-id="5b258-115">空間型をサポートする EF プロバイダー</span><span class="sxs-lookup"><span data-stu-id="5b258-115">EF providers that support spatial types</span></span>  

### <a name="ef5"></a><span data-ttu-id="5b258-116">EF5</span><span class="sxs-lookup"><span data-stu-id="5b258-116">EF5</span></span>  

<span data-ttu-id="5b258-117">空間型をサポートしていることを認識している EF5 の Entity Framework プロバイダーは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="5b258-117">The Entity Framework providers for EF5 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="5b258-118">Microsoft SQL Server プロバイダー</span><span class="sxs-lookup"><span data-stu-id="5b258-118">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="5b258-119">このプロバイダーは、EF5 の一部として出荷されています。</span><span class="sxs-lookup"><span data-stu-id="5b258-119">This provider is shipped as part of EF5.</span></span>  
    - <span data-ttu-id="5b258-120">このプロバイダーは、インストールする必要があるその他の下位レベルのライブラリに依存しています。詳細については、以下を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5b258-120">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="5b258-121">Devart dotConnect for Oracle</span><span class="sxs-lookup"><span data-stu-id="5b258-121">Devart dotConnect for Oracle</span></span>](https://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="5b258-122">これは Devart のサードパーティプロバイダーです。</span><span class="sxs-lookup"><span data-stu-id="5b258-122">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="5b258-123">空間型をサポートする EF5 プロバイダーがわかっている場合は、連絡先を取得してください。この一覧に追加することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="5b258-123">If you know of an EF5 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

### <a name="ef6"></a><span data-ttu-id="5b258-124">EF6</span><span class="sxs-lookup"><span data-stu-id="5b258-124">EF6</span></span>  

<span data-ttu-id="5b258-125">空間型をサポートしていることを認識している EF6 の Entity Framework プロバイダーは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="5b258-125">The Entity Framework providers for EF6 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="5b258-126">Microsoft SQL Server プロバイダー</span><span class="sxs-lookup"><span data-stu-id="5b258-126">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="5b258-127">このプロバイダーは、EF6 の一部として出荷されています。</span><span class="sxs-lookup"><span data-stu-id="5b258-127">This provider is shipped as part of EF6.</span></span>  
    - <span data-ttu-id="5b258-128">このプロバイダーは、インストールする必要があるその他の下位レベルのライブラリに依存しています。詳細については、以下を参照してください。</span><span class="sxs-lookup"><span data-stu-id="5b258-128">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="5b258-129">Devart dotConnect for Oracle</span><span class="sxs-lookup"><span data-stu-id="5b258-129">Devart dotConnect for Oracle</span></span>](https://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="5b258-130">これは Devart のサードパーティプロバイダーです。</span><span class="sxs-lookup"><span data-stu-id="5b258-130">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="5b258-131">空間型をサポートする EF6 プロバイダーがわかっている場合は、連絡先を取得してください。この一覧に追加することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="5b258-131">If you know of an EF6 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a><span data-ttu-id="5b258-132">Microsoft SQL Server を使用した空間型の前提条件</span><span class="sxs-lookup"><span data-stu-id="5b258-132">Prerequisites for spatial types with Microsoft SQL Server</span></span>  

<span data-ttu-id="5b258-133">SQL Server 空間サポートは、下位レベルの SQL Server 固有の型 SqlGeography および Sqlgeography に依存します。</span><span class="sxs-lookup"><span data-stu-id="5b258-133">SQL Server spatial support depends on the low-level, SQL Server-specific types SqlGeography and SqlGeometry.</span></span> <span data-ttu-id="5b258-134">これらの型は Microsoft.SqlServer.Types.dll アセンブリに存在します。このアセンブリは、EF の一部として、または .NET Framework の一部として出荷されることはありません。</span><span class="sxs-lookup"><span data-stu-id="5b258-134">These types live in Microsoft.SqlServer.Types.dll assembly, and this assembly is not shipped as part of EF or as part of the .NET Framework.</span></span>  

<span data-ttu-id="5b258-135">Visual Studio をインストールすると、多くの場合、SQL Server のバージョンもインストールされます。これには、Microsoft.SqlServer.Types.dll のインストールが含まれます。</span><span class="sxs-lookup"><span data-stu-id="5b258-135">When Visual Studio is installed it will often also install a version of SQL Server, and this will include installation of the Microsoft.SqlServer.Types.dll.</span></span>  

<span data-ttu-id="5b258-136">空間型を使用するコンピューターに SQL Server がインストールされていない場合、または空間型が SQL Server のインストールから除外されている場合は、手動でインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="5b258-136">If SQL Server is not installed on the machine where you want to use spatial types, or if spatial types were excluded from the SQL Server installation, then you will need to install them manually.</span></span> <span data-ttu-id="5b258-137">`SQLSysClrTypes.msi`これらの型は、Microsoft SQL Server Feature Pack の一部であるを使用してインストールできます。</span><span class="sxs-lookup"><span data-stu-id="5b258-137">The types can be installed using `SQLSysClrTypes.msi`, which is part of Microsoft SQL Server Feature Pack.</span></span> <span data-ttu-id="5b258-138">空間の種類は SQL Server バージョンによって異なります。そのため、Microsoft ダウンロードセンターで ["SQL Server Feature Pack" を検索](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) し、使用する SQL Server のバージョンに対応するオプションを選択してダウンロードすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="5b258-138">Spatial types are SQL Server version-specific, so we recommend [search for "SQL Server Feature Pack"](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) in the Microsoft Download Center, then select and download the option that corresponds to the version of SQL Server you will use.</span></span>
