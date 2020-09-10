---
title: Entity Framework 用語集-EF6
description: Entity Framework 6 用語集
author: divega
ms.date: 10/23/2016
ms.assetid: 3f05ffdd-49bc-499c-9732-4a368bf5d2d7
uid: ef6/resources/glossary
ms.openlocfilehash: 19d5e9e3a480337c2bcb93be5f989cc622b67dad
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/09/2020
ms.locfileid: "89620197"
---
# <a name="entity-framework-glossary"></a><span data-ttu-id="fe710-103">Entity Framework 用語集</span><span class="sxs-lookup"><span data-stu-id="fe710-103">Entity Framework Glossary</span></span>
## <a name="code-first"></a><span data-ttu-id="fe710-104">Code First</span><span class="sxs-lookup"><span data-stu-id="fe710-104">Code First</span></span>
<span data-ttu-id="fe710-105">コードを使用して Entity Framework モデルを作成する。</span><span class="sxs-lookup"><span data-stu-id="fe710-105">Creating an Entity Framework model using code.</span></span> <span data-ttu-id="fe710-106">モデルでは、既存のデータベースまたは新しいデータベースを対象にすることができます。</span><span class="sxs-lookup"><span data-stu-id="fe710-106">The model can target an existing database or a new database.</span></span>

## <a name="context"></a><span data-ttu-id="fe710-107">コンテキスト</span><span class="sxs-lookup"><span data-stu-id="fe710-107">Context</span></span>
<span data-ttu-id="fe710-108">データベースとのセッションを表すクラス。これにより、データのクエリと保存を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="fe710-108">A class that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="fe710-109">コンテキストは、DbContext または ObjectContext クラスから派生します。</span><span class="sxs-lookup"><span data-stu-id="fe710-109">A context derives from the DbContext or ObjectContext class.</span></span>

## <a name="convention-code-first"></a><span data-ttu-id="fe710-110">規則 (Code First)</span><span class="sxs-lookup"><span data-stu-id="fe710-110">Convention (Code First)</span></span>
<span data-ttu-id="fe710-111">クラスからモデルの形状を推論するために Entity Framework 使用する規則。</span><span class="sxs-lookup"><span data-stu-id="fe710-111">A rule that Entity Framework uses to infer the shape of you model from your classes.</span></span>

## <a name="database-first"></a><span data-ttu-id="fe710-112">Database First</span><span class="sxs-lookup"><span data-stu-id="fe710-112">Database First</span></span>
<span data-ttu-id="fe710-113">EF デザイナーを使用して、既存のデータベースを対象とする Entity Framework モデルを作成します。</span><span class="sxs-lookup"><span data-stu-id="fe710-113">Creating an Entity Framework model, using the EF Designer, that targets an existing database.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="fe710-114">一括読み込み</span><span class="sxs-lookup"><span data-stu-id="fe710-114">Eager loading</span></span>
<span data-ttu-id="fe710-115">ある種類のエンティティに対するクエリもクエリの一部として関連エンティティを読み込む、関連データを読み込むパターン。</span><span class="sxs-lookup"><span data-stu-id="fe710-115">A pattern of loading related data where a query for one type of entity also loads related entities as part of the query.</span></span>

## <a name="ef-designer"></a><span data-ttu-id="fe710-116">EF デザイナー</span><span class="sxs-lookup"><span data-stu-id="fe710-116">EF Designer</span></span>
<span data-ttu-id="fe710-117">ボックスと行を使用して Entity Framework モデルを作成できる Visual Studio のビジュアルデザイナー。</span><span class="sxs-lookup"><span data-stu-id="fe710-117">A visual designer in Visual Studio that allows you to create an Entity Framework model using boxes and lines.</span></span>

## <a name="entity"></a><span data-ttu-id="fe710-118">Entity</span><span class="sxs-lookup"><span data-stu-id="fe710-118">Entity</span></span>
<span data-ttu-id="fe710-119">顧客、製品、注文などのアプリケーション データを表すクラスまたはオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="fe710-119">A class or object that represents application data such as customers, products, and orders.</span></span>

## <a name="entity-data-model"></a><span data-ttu-id="fe710-120">エンティティ データ モデル</span><span class="sxs-lookup"><span data-stu-id="fe710-120">Entity Data Model</span></span>
<span data-ttu-id="fe710-121">エンティティとその間のリレーションシップを記述するモデル。</span><span class="sxs-lookup"><span data-stu-id="fe710-121">A model that describes entities and the relationships between them.</span></span> <span data-ttu-id="fe710-122">EF では、EDM を使用して、開発者がプログラムを実行する概念モデルを記述します。</span><span class="sxs-lookup"><span data-stu-id="fe710-122">EF uses EDM to describe the conceptual model against which the developer programs.</span></span> <span data-ttu-id="fe710-123">EDM は、Dr. Peter Chen によって導入されたエンティティリレーションシップモデルに基づいて構築されます。</span><span class="sxs-lookup"><span data-stu-id="fe710-123">EDM builds on the Entity Relationship model introduced by Dr. Peter Chen.</span></span> <span data-ttu-id="fe710-124">EDM は、Microsoft の開発者およびサーバーテクノロジのスイート全体で共通のデータモデルになるという主な目標を使用して開発されました。</span><span class="sxs-lookup"><span data-stu-id="fe710-124">The EDM was originally developed with the primary goal of becoming the common data model across a suite of developer and server technologies from Microsoft.</span></span> <span data-ttu-id="fe710-125">EDM は、OData プロトコルの一部としても使用されます。</span><span class="sxs-lookup"><span data-stu-id="fe710-125">EDM is also used as part of the OData protocol.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="fe710-126">明示的読み込み</span><span class="sxs-lookup"><span data-stu-id="fe710-126">Explicit loading</span></span>
<span data-ttu-id="fe710-127">API を呼び出すことによって関連オブジェクトが読み込まれる場合に関連するデータを読み込むパターン。</span><span class="sxs-lookup"><span data-stu-id="fe710-127">A pattern of loading related data where related objects are loaded by calling an API.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="fe710-128">Fluent API</span><span class="sxs-lookup"><span data-stu-id="fe710-128">Fluent API</span></span>
<span data-ttu-id="fe710-129">Code First モデルを構成するために使用できる API。</span><span class="sxs-lookup"><span data-stu-id="fe710-129">An API that can be used to configure a Code First model.</span></span>

## <a name="foreign-key-association"></a><span data-ttu-id="fe710-130">外部キーの関連付け</span><span class="sxs-lookup"><span data-stu-id="fe710-130">Foreign key association</span></span>
<span data-ttu-id="fe710-131">外部キーを表すプロパティが依存エンティティのクラスに含まれるエンティティ間のアソシエーション。</span><span class="sxs-lookup"><span data-stu-id="fe710-131">An association between entities where a property that represents the foreign key is included in the class of the dependent entity.</span></span> <span data-ttu-id="fe710-132">たとえば、Product には CategoryId プロパティが含まれています。</span><span class="sxs-lookup"><span data-stu-id="fe710-132">For example, Product contains a CategoryId property.</span></span>

## <a name="identifying-relationship"></a><span data-ttu-id="fe710-133">リレーションシップの識別</span><span class="sxs-lookup"><span data-stu-id="fe710-133">Identifying relationship</span></span>
<span data-ttu-id="fe710-134">プリンシパル エンティティの主キーが依存エンティティの主キーの一部であるリレーションシップ。</span><span class="sxs-lookup"><span data-stu-id="fe710-134">A relationship where the primary key of the principal entity is part of the primary key of the dependent entity.</span></span> <span data-ttu-id="fe710-135">このようなリレーションシップでは、プリンシパル エンティティが存在しないと、依存エンティティは存在できません。</span><span class="sxs-lookup"><span data-stu-id="fe710-135">In this kind of relationship, the dependent entity cannot exist without the principal entity.</span></span>

## <a name="independent-association"></a><span data-ttu-id="fe710-136">独立関連付け</span><span class="sxs-lookup"><span data-stu-id="fe710-136">Independent association</span></span>
<span data-ttu-id="fe710-137">依存エンティティのクラスの外部キーを表すプロパティがないエンティティ間のアソシエーション。</span><span class="sxs-lookup"><span data-stu-id="fe710-137">An association between entities where there is no property representing the foreign key in the class of the dependent entity.</span></span> <span data-ttu-id="fe710-138">たとえば、Product クラスには Category へのリレーションシップが含まれていますが、CategoryId プロパティはありません。</span><span class="sxs-lookup"><span data-stu-id="fe710-138">For example, a Product class contains a relationship to Category but no CategoryId property.</span></span> <span data-ttu-id="fe710-139">Entity Framework は、2つのアソシエーション end のエンティティの状態に関係なく、関連付けの状態を追跡します。</span><span class="sxs-lookup"><span data-stu-id="fe710-139">Entity Framework tracks the state of the association independently of the state of the entities at the two association ends.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="fe710-140">遅延読み込み</span><span class="sxs-lookup"><span data-stu-id="fe710-140">Lazy loading</span></span>
<span data-ttu-id="fe710-141">ナビゲーションプロパティにアクセスしたときに関連オブジェクトが自動的に読み込まれる関連データの読み込みパターン。</span><span class="sxs-lookup"><span data-stu-id="fe710-141">A pattern of loading related data where related objects are automatically loaded when a navigation property is accessed.</span></span>

## <a name="model-first"></a><span data-ttu-id="fe710-142">Model First</span><span class="sxs-lookup"><span data-stu-id="fe710-142">Model First</span></span>
<span data-ttu-id="fe710-143">EF デザイナーを使用して Entity Framework モデルを作成し、その後、新しいデータベースを作成するために使用します。</span><span class="sxs-lookup"><span data-stu-id="fe710-143">Creating an Entity Framework model, using the EF Designer, that is then used to create a new database.</span></span>

## <a name="navigation-property"></a><span data-ttu-id="fe710-144">ナビゲーション プロパティ</span><span class="sxs-lookup"><span data-stu-id="fe710-144">Navigation property</span></span>
<span data-ttu-id="fe710-145">別のエンティティを参照するエンティティのプロパティ。</span><span class="sxs-lookup"><span data-stu-id="fe710-145">A property of an entity that references another entity.</span></span> <span data-ttu-id="fe710-146">たとえば、Product に Category ナビゲーションプロパティが含まれていて、Category に Products ナビゲーションプロパティが含まれているとします。</span><span class="sxs-lookup"><span data-stu-id="fe710-146">For example, Product contains a Category navigation property and Category contains a Products navigation property.</span></span>

## <a name="poco"></a><span data-ttu-id="fe710-147">POCO</span><span class="sxs-lookup"><span data-stu-id="fe710-147">POCO</span></span>
<span data-ttu-id="fe710-148">Plain Old CLR Object の頭字語。</span><span class="sxs-lookup"><span data-stu-id="fe710-148">Acronym for Plain-Old CLR Object.</span></span> <span data-ttu-id="fe710-149">任意のフレームワークとの依存関係を持たない単純なユーザークラス。</span><span class="sxs-lookup"><span data-stu-id="fe710-149">A simple user class that has no dependencies with any framework.</span></span> <span data-ttu-id="fe710-150">EF のコンテキストでは、EntityObject から派生していないエンティティクラスは、任意のインターフェイスを実装するか、EF で定義されているすべての属性を保持します。</span><span class="sxs-lookup"><span data-stu-id="fe710-150">In the context of EF, an entity class that does not derive from EntityObject, implements any interfaces or carries any attributes defined in EF.</span></span> <span data-ttu-id="fe710-151">永続化フレームワークから切り離されたこのようなエンティティクラスは、"永続化に依存しない" とも呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="fe710-151">Such entity classes that are decoupled from the persistence framework are also said to be "persistence ignorant".</span></span>  

## <a name="relationship-inverse"></a><span data-ttu-id="fe710-152">リレーションシップの逆</span><span class="sxs-lookup"><span data-stu-id="fe710-152">Relationship inverse</span></span>
<span data-ttu-id="fe710-153">リレーションシップの反対側 (product など)。カテゴリとカテゴリ。梱包.</span><span class="sxs-lookup"><span data-stu-id="fe710-153">The opposite end of a relationship, for example, product.Category and category.Product.</span></span>

## <a name="self-tracking-entity"></a><span data-ttu-id="fe710-154">自己追跡エンティティ</span><span class="sxs-lookup"><span data-stu-id="fe710-154">Self-tracking entity</span></span>
<span data-ttu-id="fe710-155">N 層の開発に役立つコード生成テンプレートから構築されたエンティティ。</span><span class="sxs-lookup"><span data-stu-id="fe710-155">An entity built from a code generation template that helps with N-Tier development.</span></span>

## <a name="table-per-concrete-type-tpc"></a><span data-ttu-id="fe710-156">具象型ごとのテーブル (TPC)</span><span class="sxs-lookup"><span data-stu-id="fe710-156">Table-per-concrete type (TPC)</span></span>
<span data-ttu-id="fe710-157">階層内の各非抽象型がデータベース内の別のテーブルにマップされる継承をマッピングする方法。</span><span class="sxs-lookup"><span data-stu-id="fe710-157">A method of mapping the inheritance where each non-abstract type in the hierarchy is mapped to separate table in the database.</span></span>

## <a name="table-per-hierarchy-tph"></a><span data-ttu-id="fe710-158">階層ごとのテーブル (TPH)</span><span class="sxs-lookup"><span data-stu-id="fe710-158">Table-per-hierarchy (TPH)</span></span>
<span data-ttu-id="fe710-159">階層内のすべての型がデータベース内の同じテーブルにマップされる継承をマッピングする方法。</span><span class="sxs-lookup"><span data-stu-id="fe710-159">A method of mapping the inheritance where all types in the hierarchy are mapped to the same table in the database.</span></span> <span data-ttu-id="fe710-160">識別子列は、各行が関連付けられている型を識別するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="fe710-160">A discriminator column(s) is used to identify what type each row is associated with.</span></span>

## <a name="table-per-type-tpt"></a><span data-ttu-id="fe710-161">型ごとのテーブル (TPT)</span><span class="sxs-lookup"><span data-stu-id="fe710-161">Table-per-type (TPT)</span></span>
<span data-ttu-id="fe710-162">階層内のすべての型の共通プロパティがデータベース内の同じテーブルにマップされる継承をマッピングする方法。ただし、それぞれの型に固有のプロパティは、別のテーブルにマップされます。</span><span class="sxs-lookup"><span data-stu-id="fe710-162">A method of mapping the inheritance where the common properties of all types in the hierarchy are mapped to the same table in the database, but properties unique to each type are mapped to a separate table.</span></span>

## <a name="type-discovery"></a><span data-ttu-id="fe710-163">型の検出</span><span class="sxs-lookup"><span data-stu-id="fe710-163">Type discovery</span></span>
<span data-ttu-id="fe710-164">Entity Framework モデルに含める必要がある型を識別するプロセス。</span><span class="sxs-lookup"><span data-stu-id="fe710-164">The process of identifying the types that should be part of an Entity Framework model.</span></span>
