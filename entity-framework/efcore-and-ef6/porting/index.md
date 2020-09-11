---
title: EF6 から EF Core へ移植 - EF
description: Entity Framework 6 から Entity Framework Core へのアプリケーションの移植に関する一般的な情報
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 826b58bd-77b0-4bbc-bfcd-24d1ed3a8f38
uid: efcore-and-ef6/porting/index
ms.openlocfilehash: 132934df2ef7929372c4a092635c5c97227983f9
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/09/2020
ms.locfileid: "89619677"
---
# <a name="porting-from-ef6-to-ef-core"></a><span data-ttu-id="1cb4e-103">EF6 から EF Core へ移植</span><span class="sxs-lookup"><span data-stu-id="1cb4e-103">Porting from EF6 to EF Core</span></span>

<span data-ttu-id="1cb4e-104">EF Core には根本的な変更が加えられているため、変更するやむを得ない理由がない限り、EF6 アプリケーションの EF Core への移行はお勧めできません。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-104">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span>
<span data-ttu-id="1cb4e-105">EF6 から EF Core への移行は、アップグレードではなく移植と考える必要があります。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-105">You should view the move from EF6 to EF Core as a port rather than an upgrade.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1cb4e-106">移植プロセスを開始する前に、EF Core がご自身のアプリケーションのデータ アクセス要件を満たしていることを確認することが重要です。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-106">Before you start the porting process it is important to validate that EF Core meets the data access requirements for your application.</span></span>

## <a name="missing-features"></a><span data-ttu-id="1cb4e-107">実装されていない機能</span><span class="sxs-lookup"><span data-stu-id="1cb4e-107">Missing features</span></span>

<span data-ttu-id="1cb4e-108">ご自身のアプリケーション内で使用する必要があるすべての機能が EF Core に含まれていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-108">Make sure that EF Core has all the features you need to use in your application.</span></span> <span data-ttu-id="1cb4e-109">EF Core の機能セットと EF6 との詳細な比較については、[機能の比較](xref:efcore-and-ef6/index)に関するページをご覧ください。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-109">See [Feature Comparison](xref:efcore-and-ef6/index) for a detailed comparison of how the feature set in EF Core compares to EF6.</span></span> <span data-ttu-id="1cb4e-110">必要な機能が不足している場合は、EF Core に移植する前に、それらの機能の不足を補正できることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-110">If any required features are missing, ensure that you can compensate for the lack of these features before porting to EF Core.</span></span>

## <a name="behavior-changes"></a><span data-ttu-id="1cb4e-111">動作の変更</span><span class="sxs-lookup"><span data-stu-id="1cb4e-111">Behavior changes</span></span>

<span data-ttu-id="1cb4e-112">これは、EF6 と EF Core の間での動作に関する変更点の一覧です (すべてではありません)。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-112">This is a non-exhaustive list of some changes in behavior between EF6 and EF Core.</span></span> <span data-ttu-id="1cb4e-113">アプリケーションを移植する際に、これらの点に注意することが重要です。これらによってアプリケーションの動作方法が変更される可能性がありますが、EF Core へのスワップ後にコンパイル エラーとして表示されることはありません。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-113">It is important to keep these in mind as your port your application as they may change the way your application behaves, but will not show up as compilation errors after swapping to EF Core.</span></span>

### <a name="dbsetaddattach-and-graph-behavior"></a><span data-ttu-id="1cb4e-114">DbSet.Add/Attach とグラフの動作</span><span class="sxs-lookup"><span data-stu-id="1cb4e-114">DbSet.Add/Attach and graph behavior</span></span>

<span data-ttu-id="1cb4e-115">EF6 では、エンティティに対して `DbSet.Add()` を呼び出すと、そのナビゲーション プロパティで参照されているすべてのエンティティに対して再帰的な検索が実行されます。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-115">In EF6, calling `DbSet.Add()` on an entity results in a recursive search for all entities referenced in its navigation properties.</span></span> <span data-ttu-id="1cb4e-116">見つかったすべてのエンティティと、まだコンテキストによって追跡されていないエンティティも、追加済みとしてマークされます。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-116">Any entities that are found, and are not already tracked by the context, are also marked as added.</span></span> <span data-ttu-id="1cb4e-117">`DbSet.Attach()` の動作は、すべてのエンティティが変更なしとしてマークされる以外は同じです。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-117">`DbSet.Attach()` behaves the same, except all entities are marked as unchanged.</span></span>

<span data-ttu-id="1cb4e-118">**EF Core でも同様の再帰的な検索が実行されますが、若干異なる規則がいくつか使われます。**</span><span class="sxs-lookup"><span data-stu-id="1cb4e-118">**EF Core performs a similar recursive search, but with some slightly different rules.**</span></span>

*  <span data-ttu-id="1cb4e-119">ルート エンティティは常に要求された状態になります (`DbSet.Add` の場合は追加済み、`DbSet.Attach` の場合は変更なし)。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-119">The root entity is always in the requested state (added for `DbSet.Add` and unchanged for `DbSet.Attach`).</span></span>

*  <span data-ttu-id="1cb4e-120">**ナビゲーション プロパティの再帰的な検索中に検出されたエンティティの場合:**</span><span class="sxs-lookup"><span data-stu-id="1cb4e-120">**For entities that are found during the recursive search of navigation properties:**</span></span>

    *  <span data-ttu-id="1cb4e-121">**エンティティの主キーがストアで生成されたものだった場合**</span><span class="sxs-lookup"><span data-stu-id="1cb4e-121">**If the primary key of the entity is store generated**</span></span>

        * <span data-ttu-id="1cb4e-122">主キーが値に設定されていない場合、状態は追加済みに設定されます。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-122">If the primary key is not set to a value, the state is set to added.</span></span> <span data-ttu-id="1cb4e-123">主キーに対してそのプロパティの型の CLR 既定値 (たとえば、`int` に対して `0`、`string` に対して `null` など) が割り当てられていた場合、それは "未設定" と見なされます。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-123">The primary key value is considered "not set" if it is assigned the CLR default value for the property type (for example, `0` for `int`, `null` for `string`, etc.).</span></span>

        * <span data-ttu-id="1cb4e-124">主キーが値に設定されていた場合、その状態は変更なしに設定されます。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-124">If the primary key is set to a value, the state is set to unchanged.</span></span>

    *  <span data-ttu-id="1cb4e-125">主キーがデータベースで生成されたものではなかった場合、そのエンティティはルートと同じ状態になります。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-125">If the primary key is not database generated, the entity is put in the same state as the root.</span></span>

### <a name="code-first-database-initialization"></a><span data-ttu-id="1cb4e-126">Code First のデータベース初期化</span><span class="sxs-lookup"><span data-stu-id="1cb4e-126">Code First database initialization</span></span>

<span data-ttu-id="1cb4e-127">**EF6 には、データベース接続の選択やデータベースの初期化に関連して実行される魔法が大量に含まれています。たとえば、次のような規則があります。**</span><span class="sxs-lookup"><span data-stu-id="1cb4e-127">**EF6 has a significant amount of magic it performs around selecting the database connection and initializing the database. Some of these rules include:**</span></span>

* <span data-ttu-id="1cb4e-128">構成が実行されない場合、EF6 により SQL Express または LocalDb のデータベースが選択されます。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-128">If no configuration is performed, EF6 will select a database on SQL Express or LocalDb.</span></span>

* <span data-ttu-id="1cb4e-129">コンテキストと同じ名前の接続文字列がアプリケーションの `App/Web.config` ファイル内にある場合は、この接続が使用されます。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-129">If a connection string with the same name as the context is in the applications `App/Web.config` file, this connection will be used.</span></span>

* <span data-ttu-id="1cb4e-130">データベースが存在しない場合は、作成されます。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-130">If the database does not exist, it is created.</span></span>

* <span data-ttu-id="1cb4e-131">モデルのテーブルがデータベース内に 1 つも存在しない場合は、現在のモデルのスキーマがデータベースに追加されます。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-131">If none of the tables from the model exist in the database, the schema for the current model is added to the database.</span></span> <span data-ttu-id="1cb4e-132">移行が有効になっている場合、データベースを作成するためにそれらが使用されます。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-132">If migrations are enabled, then they are used to create the database.</span></span>

* <span data-ttu-id="1cb4e-133">データベースが存在し、EF6 によって既にスキーマが作成されていた場合は、スキーマに現在のモデルとの互換性があるかどうかがチェックされます。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-133">If the database exists and EF6 had previously created the schema, then the schema is checked for compatibility with the current model.</span></span> <span data-ttu-id="1cb4e-134">スキーマの作成後にモデルが変更されていた場合は、例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-134">An exception is thrown if the model has changed since the schema was created.</span></span>

<span data-ttu-id="1cb4e-135">**EF Core では、このような魔法は実行されません。**</span><span class="sxs-lookup"><span data-stu-id="1cb4e-135">**EF Core does not perform any of this magic.**</span></span>

* <span data-ttu-id="1cb4e-136">データベース接続は、コード内で明示的に構成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-136">The database connection must be explicitly configured in code.</span></span>

* <span data-ttu-id="1cb4e-137">初期化は実行されません。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-137">No initialization is performed.</span></span> <span data-ttu-id="1cb4e-138">`DbContext.Database.Migrate()` を使って移行を適用する必要があります (または、`DbContext.Database.EnsureCreated()` および `EnsureDeleted()` を使って、移行を使わずにデータベースの作成/削除を行います)。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-138">You must use `DbContext.Database.Migrate()` to apply migrations (or `DbContext.Database.EnsureCreated()` and `EnsureDeleted()` to create/delete the database without using migrations).</span></span>

### <a name="code-first-table-naming-convention"></a><span data-ttu-id="1cb4e-139">Code First のテーブルの名前付け規則</span><span class="sxs-lookup"><span data-stu-id="1cb4e-139">Code First table naming convention</span></span>

<span data-ttu-id="1cb4e-140">EF6 では、エンティティ クラス名を複数形化サービスにかけて、そのエンティティがマップされる既定のテーブル名を計算します。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-140">EF6 runs the entity class name through a pluralization service to calculate the default table name that the entity is mapped to.</span></span>

<span data-ttu-id="1cb4e-141">EF Core では、派生コンテキストでエンティティが公開される `DbSet` プロパティの名前が使われます。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-141">EF Core uses the name of the `DbSet` property that the entity is exposed in on the derived context.</span></span> <span data-ttu-id="1cb4e-142">エンティティに `DbSet` プロパティがない場合は、クラス名が使われます。</span><span class="sxs-lookup"><span data-stu-id="1cb4e-142">If the entity does not have a `DbSet` property, then the class name is used.</span></span>
