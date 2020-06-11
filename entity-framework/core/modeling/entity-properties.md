---
title: エンティティのプロパティ-EF Core
description: Entity Framework Core を使用してエンティティのプロパティを構成およびマップする方法
author: roji
ms.date: 12/10/2019
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/entity-properties
ms.openlocfilehash: e4a1867a90df1fb277e7dd44b93d6c2d47895030
ms.sourcegitcommit: 92d54fe3702e0c92e198334da22bacb42e9842b1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/10/2020
ms.locfileid: "84664157"
---
# <a name="entity-properties"></a><span data-ttu-id="084a2-103">エンティティのプロパティ</span><span class="sxs-lookup"><span data-stu-id="084a2-103">Entity Properties</span></span>

<span data-ttu-id="084a2-104">モデル内の各エンティティ型には、一連のプロパティがあります。これらのプロパティは、データベースから読み取りおよび書き込みを実行 EF Core ます。</span><span class="sxs-lookup"><span data-stu-id="084a2-104">Each entity type in your model has a set of properties, which EF Core will read and write from the database.</span></span> <span data-ttu-id="084a2-105">リレーショナルデータベースを使用している場合、エンティティのプロパティはテーブルの列にマップされます。</span><span class="sxs-lookup"><span data-stu-id="084a2-105">If you're using a relational database, entity properties map to table columns.</span></span>

## <a name="included-and-excluded-properties"></a><span data-ttu-id="084a2-106">包含プロパティと除外されるプロパティ</span><span class="sxs-lookup"><span data-stu-id="084a2-106">Included and excluded properties</span></span>

<span data-ttu-id="084a2-107">慣例により、getter と setter を持つすべてのパブリックプロパティがモデルに含まれます。</span><span class="sxs-lookup"><span data-stu-id="084a2-107">By convention, all public properties with a getter and a setter will be included in the model.</span></span>

<span data-ttu-id="084a2-108">特定のプロパティは、次のように除外できます。</span><span class="sxs-lookup"><span data-stu-id="084a2-108">Specific properties can be excluded as follows:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="084a2-109">データの注釈</span><span class="sxs-lookup"><span data-stu-id="084a2-109">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?name=IgnoreProperty&highlight=6)]

### <a name="fluent-api"></a>[<span data-ttu-id="084a2-110">Fluent API</span><span class="sxs-lookup"><span data-stu-id="084a2-110">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?name=IgnoreProperty&highlight=3,4)]

***

## <a name="column-names"></a><span data-ttu-id="084a2-111">列名</span><span class="sxs-lookup"><span data-stu-id="084a2-111">Column names</span></span>

<span data-ttu-id="084a2-112">規則により、リレーショナルデータベースを使用する場合、エンティティのプロパティは、プロパティと同じ名前のテーブル列にマップされます。</span><span class="sxs-lookup"><span data-stu-id="084a2-112">By convention, when using a relational database, entity properties are mapped to table columns having the same name as the property.</span></span>

<span data-ttu-id="084a2-113">異なる名前で列を構成する場合は、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="084a2-113">If you prefer to configure your columns with different names, you can do so as following:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="084a2-114">データの注釈</span><span class="sxs-lookup"><span data-stu-id="084a2-114">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnName.cs?Name=ColumnName&highlight=3)]

### <a name="fluent-api"></a>[<span data-ttu-id="084a2-115">Fluent API</span><span class="sxs-lookup"><span data-stu-id="084a2-115">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnName.cs?Name=ColumnName&highlight=3-5)]

***

## <a name="column-data-types"></a><span data-ttu-id="084a2-116">列のデータ型</span><span class="sxs-lookup"><span data-stu-id="084a2-116">Column data types</span></span>

<span data-ttu-id="084a2-117">リレーショナルデータベースを使用する場合、データベースプロバイダーは、プロパティの .NET 型に基づいてデータ型を選択します。</span><span class="sxs-lookup"><span data-stu-id="084a2-117">When using a relational database, the database provider selects a data type based on the .NET type of the property.</span></span> <span data-ttu-id="084a2-118">また、構成された[最大長](#maximum-length)、プロパティが主キーの一部であるかどうかなど、他のメタデータも考慮する必要があります。</span><span class="sxs-lookup"><span data-stu-id="084a2-118">It also takes into account other metadata, such as the configured [maximum length](#maximum-length), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="084a2-119">たとえば、SQL Server は、プロパティを列に、プロパティを列にマップし `DateTime` `datetime2(7)` `string` `nvarchar(max)` ます (または、 `nvarchar(450)` キーとして使用されるプロパティの場合)。</span><span class="sxs-lookup"><span data-stu-id="084a2-119">For example, SQL Server maps `DateTime` properties to `datetime2(7)` columns, and `string` properties to `nvarchar(max)` columns (or to `nvarchar(450)` for properties that are used as a key).</span></span>

<span data-ttu-id="084a2-120">列のデータ型を正確に指定するように列を構成することもできます。</span><span class="sxs-lookup"><span data-stu-id="084a2-120">You can also configure your columns to specify an exact data type for a column.</span></span> <span data-ttu-id="084a2-121">たとえば、次のコードは、の最大長を持つ非 unicode 文字列としてを構成し、の `Url` `200` `Rating` 有効桁数と小数点以下桁数を10進数に設定し `5` `2` ます。</span><span class="sxs-lookup"><span data-stu-id="084a2-121">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="084a2-122">データの注釈</span><span class="sxs-lookup"><span data-stu-id="084a2-122">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnDataType.cs?name=ColumnDataType&highlight=4,6)]

### <a name="fluent-api"></a>[<span data-ttu-id="084a2-123">Fluent API</span><span class="sxs-lookup"><span data-stu-id="084a2-123">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnDataType.cs?name=ColumnDataType&highlight=5-6)]

***

### <a name="maximum-length"></a><span data-ttu-id="084a2-124">最大長</span><span class="sxs-lookup"><span data-stu-id="084a2-124">Maximum length</span></span>

<span data-ttu-id="084a2-125">最大長を構成すると、特定のプロパティに対して選択する適切な列のデータ型について、データベースプロバイダーにヒントが提供されます。</span><span class="sxs-lookup"><span data-stu-id="084a2-125">Configuring a maximum length provides a hint to the database provider about the appropriate column data type to choose for a given property.</span></span> <span data-ttu-id="084a2-126">最大長は、配列のデータ型 (やなど) にのみ適用さ `string` `byte[]` れます。</span><span class="sxs-lookup"><span data-stu-id="084a2-126">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]
> <span data-ttu-id="084a2-127">Entity Framework は、データをプロバイダーに渡す前に、最大長の検証を行いません。</span><span class="sxs-lookup"><span data-stu-id="084a2-127">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="084a2-128">必要に応じて、プロバイダーまたはデータストアが検証する必要があります。</span><span class="sxs-lookup"><span data-stu-id="084a2-128">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="084a2-129">たとえば、SQL Server を対象とする場合、基になる列のデータ型では過剰なデータを格納できないため、最大長を超えると例外が発生します。</span><span class="sxs-lookup"><span data-stu-id="084a2-129">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

<span data-ttu-id="084a2-130">次の例では、最大長の500を構成すると、SQL Server に型の列が作成され `nvarchar(500)` ます。</span><span class="sxs-lookup"><span data-stu-id="084a2-130">In the following example, configuring a maximum length of 500 will cause a column of type `nvarchar(500)` to be created on SQL Server:</span></span>

#### <a name="data-annotations"></a>[<span data-ttu-id="084a2-131">データの注釈</span><span class="sxs-lookup"><span data-stu-id="084a2-131">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?name=MaxLength&highlight=4)]

#### <a name="fluent-api"></a>[<span data-ttu-id="084a2-132">Fluent API</span><span class="sxs-lookup"><span data-stu-id="084a2-132">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?name=MaxLength&highlight=3-5)]

***

## <a name="required-and-optional-properties"></a><span data-ttu-id="084a2-133">必須および省略可能なプロパティ</span><span class="sxs-lookup"><span data-stu-id="084a2-133">Required and optional properties</span></span>

<span data-ttu-id="084a2-134">プロパティが有効である場合、プロパティは省略可能と見なされ `null` ます。</span><span class="sxs-lookup"><span data-stu-id="084a2-134">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="084a2-135">`null`がプロパティに割り当てられる有効な値でない場合、必須プロパティと見なされます。</span><span class="sxs-lookup"><span data-stu-id="084a2-135">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span> <span data-ttu-id="084a2-136">リレーショナルデータベーススキーマにマップする場合、必要なプロパティは null 非許容列として作成され、オプションのプロパティは null 値が許容される列として作成されます。</span><span class="sxs-lookup"><span data-stu-id="084a2-136">When mapping to a relational database schema, required properties are created as non-nullable columns, and optional properties are created as nullable columns.</span></span>

### <a name="conventions"></a><span data-ttu-id="084a2-137">規約</span><span class="sxs-lookup"><span data-stu-id="084a2-137">Conventions</span></span>

<span data-ttu-id="084a2-138">慣例により、.NET 型に null を含めることができるプロパティはオプションとして構成されますが、.NET 型に null を含めることができないプロパティは必要に応じて構成されます。</span><span class="sxs-lookup"><span data-stu-id="084a2-138">By convention, a property whose .NET type can contain null will be configured as optional, whereas properties whose .NET type cannot contain null will be configured as required.</span></span> <span data-ttu-id="084a2-139">たとえば、.net 値の型 (、、など) を持つすべてのプロパティは必須として構成され、 `int` `decimal` `bool` null 許容型の .net 値型 (、、など) を持つすべてのプロパティ `int?` `decimal?` `bool?` はオプションとして構成されます。</span><span class="sxs-lookup"><span data-stu-id="084a2-139">For example, all properties with .NET value types (`int`, `decimal`, `bool`, etc.) are configured as required, and all properties with nullable .NET value types (`int?`, `decimal?`, `bool?`, etc.) are configured as optional.</span></span>

<span data-ttu-id="084a2-140">C# 8 では、null[許容参照型](/dotnet/csharp/tutorials/nullable-reference-types)と呼ばれる新しい機能が導入されました。これにより、参照型に注釈を付けて、null を含むかどうかを示すことができます。</span><span class="sxs-lookup"><span data-stu-id="084a2-140">C# 8 introduced a new feature called [nullable reference types](/dotnet/csharp/tutorials/nullable-reference-types), which allows reference types to be annotated, indicating whether it is valid for them to contain null or not.</span></span> <span data-ttu-id="084a2-141">この機能は既定で無効になっています。有効にすると、EF Core の動作が次のように変更されます。</span><span class="sxs-lookup"><span data-stu-id="084a2-141">This feature is disabled by default, and if enabled, it modifies EF Core's behavior in the following way:</span></span>

* <span data-ttu-id="084a2-142">Null 許容の参照型が無効になっている場合 (既定)、.NET 参照型を持つすべてのプロパティは、規約によってオプションとして構成されます (例: `string` )。</span><span class="sxs-lookup"><span data-stu-id="084a2-142">If nullable reference types are disabled (the default), all properties with .NET reference types are configured as optional by convention (e.g. `string`).</span></span>
* <span data-ttu-id="084a2-143">Null 値を許容する参照型が有効になっている場合、プロパティは、その .NET 型の C# の null 値の許容属性に基づいて構成されます。 `string?` はオプションとして構成されますが、 `string` は必須として構成されます。</span><span class="sxs-lookup"><span data-stu-id="084a2-143">If nullable reference types are enabled, properties will be configured based on the C# nullability of their .NET type: `string?` will be configured as optional, whereas `string` will be configured as required.</span></span>

<span data-ttu-id="084a2-144">次の例は、必須プロパティと省略可能なプロパティを持つエンティティ型を示しています。 null 許容の参照機能が無効 (既定値) で、有効になっています。</span><span class="sxs-lookup"><span data-stu-id="084a2-144">The following example shows an entity type with required and optional properties, with the nullable reference feature disabled (the default) and enabled:</span></span>

#### <a name="without-nullable-reference-types-default"></a>[<span data-ttu-id="084a2-145">Null 値を許容する参照型を使用しない場合 (既定)</span><span class="sxs-lookup"><span data-stu-id="084a2-145">Without nullable reference types (default)</span></span>](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

#### <a name="with-nullable-reference-types"></a>[<span data-ttu-id="084a2-146">Null 値を許容する参照型を持つ</span><span class="sxs-lookup"><span data-stu-id="084a2-146">With nullable reference types</span></span>](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

<span data-ttu-id="084a2-147">C# コードで表される null 値の許容を EF Core のモデルとデータベースにフローさせるため、null 許容型を使用することをお勧めします。これにより、Fluent API またはデータ注釈を使用して同じ概念を2回表現することがなくなります。</span><span class="sxs-lookup"><span data-stu-id="084a2-147">Using nullable reference types is recommended since it flows the nullability expressed in C# code to EF Core's model and to the database, and obviates the use of the Fluent API or Data Annotations to express the same concept twice.</span></span>

> [!NOTE]
> <span data-ttu-id="084a2-148">既存のプロジェクトで null 値を許容する参照型を有効にする場合は注意してください。以前にオプションとして構成されていた参照型プロパティは、明示的に null 値が指定されていない限り、必須として構成されます。</span><span class="sxs-lookup"><span data-stu-id="084a2-148">Exercise caution when enabling nullable reference types on an existing project: reference type properties which were previously configured as optional will now be configured as required, unless they are explicitly annotated to be nullable.</span></span> <span data-ttu-id="084a2-149">リレーショナルデータベーススキーマを管理する場合、これにより、データベース列の null 値の許容属性を変更する移行が生成される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="084a2-149">When managing a relational database schema, this may cause migrations to be generated which alter the database column's nullability.</span></span>

<span data-ttu-id="084a2-150">Null 許容型の参照型と EF Core での使用方法の詳細については、[この機能の専用ドキュメントに関するページを参照してください](xref:core/miscellaneous/nullable-reference-types)。</span><span class="sxs-lookup"><span data-stu-id="084a2-150">For more information on nullable reference types and how to use them with EF Core, [see the dedicated documentation page for this feature](xref:core/miscellaneous/nullable-reference-types).</span></span>

### <a name="explicit-configuration"></a><span data-ttu-id="084a2-151">明示的な構成</span><span class="sxs-lookup"><span data-stu-id="084a2-151">Explicit configuration</span></span>

<span data-ttu-id="084a2-152">規約によってオプションとなるプロパティは、次のように必要になるように構成できます。</span><span class="sxs-lookup"><span data-stu-id="084a2-152">A property that would be optional by convention can be configured to be required as follows:</span></span>

#### <a name="data-annotations"></a>[<span data-ttu-id="084a2-153">データの注釈</span><span class="sxs-lookup"><span data-stu-id="084a2-153">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?name=Required&highlight=4)]

#### <a name="fluent-api"></a>[<span data-ttu-id="084a2-154">Fluent API</span><span class="sxs-lookup"><span data-stu-id="084a2-154">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?name=Required&highlight=3-5)]

***

## <a name="column-collations"></a><span data-ttu-id="084a2-155">列の照合順序</span><span class="sxs-lookup"><span data-stu-id="084a2-155">Column collations</span></span>

> [!NOTE]
> <span data-ttu-id="084a2-156">この機能は EF Core 5.0 で導入されています。</span><span class="sxs-lookup"><span data-stu-id="084a2-156">This feature is introduced in EF Core 5.0.</span></span>

<span data-ttu-id="084a2-157">照合順序は、テキスト列に対して定義し、比較および順序付けの方法を決定できます。</span><span class="sxs-lookup"><span data-stu-id="084a2-157">A collation can be defined on text columns, determining how they are compared and ordered.</span></span> <span data-ttu-id="084a2-158">たとえば、次の例では、SQL Server 列が大文字と小文字を区別しないように構成されています。</span><span class="sxs-lookup"><span data-stu-id="084a2-158">For example, the following configures a SQL Server column to be case-insensitive:</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/Collations/Program.cs?range=42-43)]

<span data-ttu-id="084a2-159">データベース内のすべての列で特定の照合順序を使用する必要がある場合は、代わりにデータベースレベルで照合順序を定義します。</span><span class="sxs-lookup"><span data-stu-id="084a2-159">If all columns in a database need to use a certain collation, define the collation at the database level instead.</span></span>

<span data-ttu-id="084a2-160">照合順序のサポート EF Core に関する一般情報については、[照合順序のドキュメントページ](xref:core/miscellaneous/collations-and-case-sensitivity.md)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="084a2-160">General information about EF Core support for collations can be found in the [collation documentation page](xref:core/miscellaneous/collations-and-case-sensitivity.md).</span></span>