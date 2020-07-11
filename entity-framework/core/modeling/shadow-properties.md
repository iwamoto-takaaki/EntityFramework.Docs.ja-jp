---
title: シャドウのプロパティ-EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: b24ff85d8f5910a5625910e7225a94112d769824
ms.sourcegitcommit: 31536e52b838a84680d2e93e5bb52fb16df72a97
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/10/2020
ms.locfileid: "86238282"
---
# <a name="shadow-properties"></a><span data-ttu-id="fe029-102">シャドウ プロパティ</span><span class="sxs-lookup"><span data-stu-id="fe029-102">Shadow Properties</span></span>

<span data-ttu-id="fe029-103">Shadow プロパティは、.NET エンティティクラスで定義されていないが、EF Core モデルでそのエンティティ型に対して定義されているプロパティです。</span><span class="sxs-lookup"><span data-stu-id="fe029-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="fe029-104">これらのプロパティの値と状態は、単に変更トラッカーに保持されます。</span><span class="sxs-lookup"><span data-stu-id="fe029-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span> <span data-ttu-id="fe029-105">シャドウプロパティは、マップされたエンティティ型では公開できないデータがデータベースに存在する場合に便利です。</span><span class="sxs-lookup"><span data-stu-id="fe029-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span>

## <a name="foreign-key-shadow-properties"></a><span data-ttu-id="fe029-106">外部キーのシャドウのプロパティ</span><span class="sxs-lookup"><span data-stu-id="fe029-106">Foreign key shadow properties</span></span>

<span data-ttu-id="fe029-107">シャドウプロパティは、外部キープロパティで最もよく使用されます。この場合、2つのエンティティ間のリレーションシップはデータベース内の外部キー値によって表されますが、リレーションシップはエンティティ型間のナビゲーションプロパティを使用してエンティティ型で管理されます。</span><span class="sxs-lookup"><span data-stu-id="fe029-107">Shadow properties are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span> <span data-ttu-id="fe029-108">慣例により、EF はリレーションシップが検出されたときに、外部キープロパティが依存エンティティクラスに見つからない場合に shadow プロパティを導入します。</span><span class="sxs-lookup"><span data-stu-id="fe029-108">By convention, EF will introduce a shadow property when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span>

<span data-ttu-id="fe029-109">このプロパティにはという名前が付けられ `<navigation property name><principal key property name>` ます (これは、プリンシパルエンティティを指し示す依存エンティティのナビゲーションで、名前付けに使用されます)。</span><span class="sxs-lookup"><span data-stu-id="fe029-109">The property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="fe029-110">プリンシパルキープロパティ名にナビゲーションプロパティの名前が含まれている場合、その名前はだけになり `<principal key property name>` ます。</span><span class="sxs-lookup"><span data-stu-id="fe029-110">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="fe029-111">依存エンティティにナビゲーションプロパティがない場合は、その代わりにプリンシパルの型名が使用されます。</span><span class="sxs-lookup"><span data-stu-id="fe029-111">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="fe029-112">たとえば、次のコードリストでは、 `BlogId` shadow プロパティがエンティティに導入され `Post` ます。</span><span class="sxs-lookup"><span data-stu-id="fe029-112">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions&highlight=21-23)]

## <a name="configuring-shadow-properties"></a><span data-ttu-id="fe029-113">シャドウプロパティの構成</span><span class="sxs-lookup"><span data-stu-id="fe029-113">Configuring shadow properties</span></span>

<span data-ttu-id="fe029-114">Fluent API を使用してシャドウプロパティを構成できます。</span><span class="sxs-lookup"><span data-stu-id="fe029-114">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="fe029-115">の文字列オーバーロードを呼び出した後は、 `Property` 他のプロパティに対して行う任意の構成呼び出しを連鎖させることができます。</span><span class="sxs-lookup"><span data-stu-id="fe029-115">Once you have called the string overload of `Property`, you can chain any of the configuration calls you would for other properties.</span></span> <span data-ttu-id="fe029-116">次の例では、 `Blog` にという名前の CLR プロパティがないため、 `LastUpdated` shadow プロパティが作成されています。</span><span class="sxs-lookup"><span data-stu-id="fe029-116">In the following sample, since `Blog` has no CLR property named `LastUpdated`, a shadow property is created:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]

<span data-ttu-id="fe029-117">メソッドに指定された名前が `Property` 既存のプロパティの名前 (shadow プロパティまたは entity クラスで定義されているプロパティ) と一致する場合、コードは新しい shadow プロパティを導入するのではなく、既存のプロパティを構成します。</span><span class="sxs-lookup"><span data-stu-id="fe029-117">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

## <a name="accessing-shadow-properties"></a><span data-ttu-id="fe029-118">シャドウプロパティへのアクセス</span><span class="sxs-lookup"><span data-stu-id="fe029-118">Accessing shadow properties</span></span>

<span data-ttu-id="fe029-119">シャドウプロパティの値は、API を使用して取得および変更でき `ChangeTracker` ます。</span><span class="sxs-lookup"><span data-stu-id="fe029-119">Shadow property values can be obtained and changed through the `ChangeTracker` API:</span></span>

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="fe029-120">シャドウプロパティは、静的メソッドを使用して LINQ クエリで参照でき `EF.Property` ます。</span><span class="sxs-lookup"><span data-stu-id="fe029-120">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method:</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

<span data-ttu-id="fe029-121">返されるエンティティは変更トラッカーによって追跡されないため、no tracking クエリの後にシャドウプロパティにアクセスすることはできません。</span><span class="sxs-lookup"><span data-stu-id="fe029-121">Shadow properties cannot be accessed after a no-tracking query since the entities returned are not tracked by the change tracker.</span></span>
