---
title: DbContext の構成-EF Core
description: Entity Framework Core を使用した DbContexts の構成方法
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 3e45199e6fc0c8c105ccb0bb03175b4b08716b3f
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/09/2020
ms.locfileid: "89617794"
---
# <a name="configuring-a-dbcontext"></a><span data-ttu-id="27904-103">DbContext の構成</span><span class="sxs-lookup"><span data-stu-id="27904-103">Configuring a DbContext</span></span>

<span data-ttu-id="27904-104">この記事では、を使用してを構成し、 `DbContext` `DbContextOptions` 特定の EF Core プロバイダーとオプションの動作を使用してデータベースに接続するための基本的なパターンについて説明します。</span><span class="sxs-lookup"><span data-stu-id="27904-104">This article shows basic patterns for configuring a `DbContext` via a `DbContextOptions` to connect to a database using a specific EF Core provider and optional behaviors.</span></span>

## <a name="design-time-dbcontext-configuration"></a><span data-ttu-id="27904-105">デザイン時の DbContext 構成</span><span class="sxs-lookup"><span data-stu-id="27904-105">Design-time DbContext configuration</span></span>

<span data-ttu-id="27904-106">[移行](xref:core/managing-schemas/migrations/index)などのデザイン時ツールでは、 `DbContext` アプリケーションのエンティティ型についての詳細を収集し、データベーススキーマにマップする方法について、型の動作するインスタンスを検出して作成できる必要があります。 EF Core</span><span class="sxs-lookup"><span data-stu-id="27904-106">EF Core design-time tools such as [migrations](xref:core/managing-schemas/migrations/index) need to be able to discover and create a working instance of a `DbContext` type in order to gather details about the application's entity types and how they map to a database schema.</span></span> <span data-ttu-id="27904-107">このプロセスは、実行時に構成するのと同じように構成されるように、ツールがを簡単に作成できる限り、自動的に行うことができます `DbContext` 。</span><span class="sxs-lookup"><span data-stu-id="27904-107">This process can be automatic as long as the tool can easily create the `DbContext` in such a way that it will be configured similarly to how it would be configured at run-time.</span></span>

<span data-ttu-id="27904-108">に必要な構成情報を提供するパターンは `DbContext` 実行時に動作しますが、デザイン時にを使用する必要があるツール `DbContext` は、限られた数のパターンでのみ機能します。</span><span class="sxs-lookup"><span data-stu-id="27904-108">While any pattern that provides the necessary configuration information to the `DbContext` can work at run-time, tools that require using a `DbContext` at design-time can only work with a limited number of patterns.</span></span> <span data-ttu-id="27904-109">これらの詳細については、「 [デザイン時コンテキストの作成](xref:core/miscellaneous/cli/dbcontext-creation) 」セクションで説明します。</span><span class="sxs-lookup"><span data-stu-id="27904-109">These are covered in more detail in the [Design-Time Context Creation](xref:core/miscellaneous/cli/dbcontext-creation) section.</span></span>

## <a name="configuring-dbcontextoptions"></a><span data-ttu-id="27904-110">DbContextOptions の構成</span><span class="sxs-lookup"><span data-stu-id="27904-110">Configuring DbContextOptions</span></span>

<span data-ttu-id="27904-111">`DbContext``DbContextOptions`任意の処理を実行するには、のインスタンスが必要です。</span><span class="sxs-lookup"><span data-stu-id="27904-111">`DbContext` must have an instance of `DbContextOptions` in order to perform any work.</span></span> <span data-ttu-id="27904-112">インスタンスには、次のよう `DbContextOptions` な構成情報が含まれます。</span><span class="sxs-lookup"><span data-stu-id="27904-112">The `DbContextOptions` instance carries configuration information such as:</span></span>

- <span data-ttu-id="27904-113">使用するデータベースプロバイダー。通常は、やなどのメソッドを呼び出すことによって選択され `UseSqlServer` `UseSqlite` ます。</span><span class="sxs-lookup"><span data-stu-id="27904-113">The database provider to use, typically selected by invoking a method such as `UseSqlServer` or `UseSqlite`.</span></span> <span data-ttu-id="27904-114">これらの拡張メソッドには、やなどの対応するプロバイダーパッケージが必要です `Microsoft.EntityFrameworkCore.SqlServer` `Microsoft.EntityFrameworkCore.Sqlite` 。</span><span class="sxs-lookup"><span data-stu-id="27904-114">These extension methods require the corresponding provider package, such as `Microsoft.EntityFrameworkCore.SqlServer` or `Microsoft.EntityFrameworkCore.Sqlite`.</span></span> <span data-ttu-id="27904-115">メソッドは、名前空間で定義されて `Microsoft.EntityFrameworkCore` います。</span><span class="sxs-lookup"><span data-stu-id="27904-115">The methods are defined in the `Microsoft.EntityFrameworkCore` namespace.</span></span>
- <span data-ttu-id="27904-116">データベースインスタンスの必要な接続文字列または識別子。通常は、上で説明したプロバイダー選択メソッドに引数として渡されます。</span><span class="sxs-lookup"><span data-stu-id="27904-116">Any necessary connection string or identifier of the database instance, typically passed as an argument to the provider selection method mentioned above</span></span>
- <span data-ttu-id="27904-117">プロバイダーレベルの任意の動作セレクター。通常は、プロバイダーの選択メソッドの呼び出しの内側にもチェーンされます。</span><span class="sxs-lookup"><span data-stu-id="27904-117">Any provider-level optional behavior selectors, typically also chained inside the call to the provider selection method</span></span>
- <span data-ttu-id="27904-118">一般的な EF Core 動作セレクター。通常は、プロバイダーセレクターメソッドの後または前にチェーンされます。</span><span class="sxs-lookup"><span data-stu-id="27904-118">Any general EF Core behavior selectors, typically chained after or before the provider selector method</span></span>

<span data-ttu-id="27904-119">次の例では、 `DbContextOptions` SQL Server プロバイダー、変数に含まれる接続、 `connectionString` プロバイダーレベルのコマンドタイムアウト、および既定ではすべてのクエリを `DbContext` [非追跡](xref:core/querying/tracking#no-tracking-queries) で実行する EF Core 動作セレクターを使用するようにを構成します。</span><span class="sxs-lookup"><span data-stu-id="27904-119">The following example configures the `DbContextOptions` to use the SQL Server provider, a connection contained in the `connectionString` variable, a provider-level command timeout, and an EF Core behavior selector that makes all queries executed in the `DbContext` [no-tracking](xref:core/querying/tracking#no-tracking-queries) by default:</span></span>

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> <span data-ttu-id="27904-120">前に説明したプロバイダーセレクターメソッドとその他の動作セレクターメソッドは、 `DbContextOptions` またはプロバイダー固有のオプションクラスの拡張メソッドです。</span><span class="sxs-lookup"><span data-stu-id="27904-120">Provider selector methods and other behavior selector methods mentioned above are extension methods on `DbContextOptions` or provider-specific option classes.</span></span> <span data-ttu-id="27904-121">これらの拡張メソッドにアクセスできるようにするには、スコープ内に名前空間 (通常は) が必要であり、 `Microsoft.EntityFrameworkCore` プロジェクトにパッケージの依存関係が追加されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="27904-121">In order to have access to these extension methods you may need to have a namespace (typically `Microsoft.EntityFrameworkCore`) in scope and include additional package dependencies in the project.</span></span>

<span data-ttu-id="27904-122">は、 `DbContextOptions` メソッドをオーバーライドする `DbContext` か、 `OnConfiguring` コンストラクター引数を使用して外部からに渡すことによってに渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="27904-122">The `DbContextOptions` can be supplied to the `DbContext` by overriding the `OnConfiguring` method or externally via a constructor argument.</span></span>

<span data-ttu-id="27904-123">両方が使用されている場合、は最後に適用され、 `OnConfiguring` コンストラクター引数に指定されたオプションを上書きできます。</span><span class="sxs-lookup"><span data-stu-id="27904-123">If both are used, `OnConfiguring` is applied last and can overwrite options supplied to the constructor argument.</span></span>

### <a name="constructor-argument"></a><span data-ttu-id="27904-124">コンストラクター引数</span><span class="sxs-lookup"><span data-stu-id="27904-124">Constructor argument</span></span>

<span data-ttu-id="27904-125">コンストラクターは、次のようにを受け入れるだけです `DbContextOptions` 。</span><span class="sxs-lookup"><span data-stu-id="27904-125">Your constructor can simply accept a `DbContextOptions` as follows:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

> [!TIP]  
> <span data-ttu-id="27904-126">DbContext の基本コンストラクターは、非ジェネリックバージョンのを受け取ることもでき `DbContextOptions` ますが、複数のコンテキストタイプを持つアプリケーションでは、非ジェネリックバージョンを使用することは推奨されません。</span><span class="sxs-lookup"><span data-stu-id="27904-126">The base constructor of DbContext also accepts the non-generic version of `DbContextOptions`, but using the non-generic version is not recommended for applications with multiple context types.</span></span>

<span data-ttu-id="27904-127">これで、アプリケーションは、コンテキストをインスタンス化するときに、次のようにを渡すことができます `DbContextOptions` 。</span><span class="sxs-lookup"><span data-stu-id="27904-127">Your application can now pass the `DbContextOptions` when instantiating a context, as follows:</span></span>

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a><span data-ttu-id="27904-128">OnConfiguring</span><span class="sxs-lookup"><span data-stu-id="27904-128">OnConfiguring</span></span>

<span data-ttu-id="27904-129">コンテキスト自体でを初期化することもでき `DbContextOptions` ます。</span><span class="sxs-lookup"><span data-stu-id="27904-129">You can also initialize the `DbContextOptions` within the context itself.</span></span> <span data-ttu-id="27904-130">この手法は基本的な構成に使用できますが、通常は、データベース接続文字列など、外部から特定の構成の詳細を取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="27904-130">While you can use this technique for basic configuration, you will typically still need to get certain configuration details from the outside, e.g. a database connection string.</span></span> <span data-ttu-id="27904-131">これは、構成 API またはその他の方法で行うことができます。</span><span class="sxs-lookup"><span data-stu-id="27904-131">This can be done with a configuration API or any other means.</span></span>

<span data-ttu-id="27904-132">コンテキスト内で初期化するには、 `DbContextOptions` メソッドをオーバーライド `OnConfiguring` し、指定されたでメソッドを呼び出し `DbContextOptionsBuilder` ます。</span><span class="sxs-lookup"><span data-stu-id="27904-132">To initialize `DbContextOptions` within the context, override the `OnConfiguring` method and call the methods on the provided `DbContextOptionsBuilder`:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlite("Data Source=blog.db");
    }
}
```

<span data-ttu-id="27904-133">アプリケーションは、コンストラクターに何も渡さずにこのようなコンテキストをインスタンス化できます。</span><span class="sxs-lookup"><span data-stu-id="27904-133">An application can simply instantiate such a context without passing anything to its constructor:</span></span>

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> <span data-ttu-id="27904-134">この方法は、テストが完全なデータベースを対象としている場合を除き、テストには適していません。</span><span class="sxs-lookup"><span data-stu-id="27904-134">This approach does not lend itself to testing, unless the tests target the full database.</span></span>

### <a name="using-dbcontext-with-dependency-injection"></a><span data-ttu-id="27904-135">依存関係の挿入での DbContext の使用</span><span class="sxs-lookup"><span data-stu-id="27904-135">Using DbContext with dependency injection</span></span>

<span data-ttu-id="27904-136">EF Core `DbContext` は、依存関係挿入コンテナーでのの使用をサポートしています。</span><span class="sxs-lookup"><span data-stu-id="27904-136">EF Core supports using `DbContext` with a dependency injection container.</span></span> <span data-ttu-id="27904-137">DbContext 型は、メソッドを使用してサービスコンテナーに追加でき `AddDbContext<TContext>` ます。</span><span class="sxs-lookup"><span data-stu-id="27904-137">Your DbContext type can be added to the service container by using the `AddDbContext<TContext>` method.</span></span>

<span data-ttu-id="27904-138">`AddDbContext<TContext>` は、DbContext 型、 `TContext` 、および対応するを、 `DbContextOptions<TContext>` サービスコンテナーからの挿入に使用できるようにします。</span><span class="sxs-lookup"><span data-stu-id="27904-138">`AddDbContext<TContext>` will make both your DbContext type, `TContext`, and the corresponding `DbContextOptions<TContext>` available for injection from the service container.</span></span>

<span data-ttu-id="27904-139">依存関係の挿入の詳細については [、以下を](#more-reading) 参照してください。</span><span class="sxs-lookup"><span data-stu-id="27904-139">See [more reading](#more-reading) below for additional information on dependency injection.</span></span>

<span data-ttu-id="27904-140">`DbContext`を依存関係の挿入に追加します。</span><span class="sxs-lookup"><span data-stu-id="27904-140">Adding the `DbContext` to dependency injection:</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

<span data-ttu-id="27904-141">これを行うには、を受け入れる DbContext 型に [コンストラクター引数](#constructor-argument) を追加する必要があり `DbContextOptions<TContext>` ます。</span><span class="sxs-lookup"><span data-stu-id="27904-141">This requires adding a [constructor argument](#constructor-argument) to your DbContext type that accepts `DbContextOptions<TContext>`.</span></span>

<span data-ttu-id="27904-142">コンテキストコード:</span><span class="sxs-lookup"><span data-stu-id="27904-142">Context code:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

<span data-ttu-id="27904-143">アプリケーションコード (ASP.NET Core):</span><span class="sxs-lookup"><span data-stu-id="27904-143">Application code (in ASP.NET Core):</span></span>

``` csharp
public class MyController
{
    private readonly BloggingContext _context;

    public MyController(BloggingContext context)
    {
      _context = context;
    }

    ...
}
```

<span data-ttu-id="27904-144">アプリケーションコード (ServiceProvider を直接、あまり一般的ではない):</span><span class="sxs-lookup"><span data-stu-id="27904-144">Application code (using ServiceProvider directly, less common):</span></span>

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="avoiding-dbcontext-threading-issues"></a><span data-ttu-id="27904-145">DbContext スレッドの問題の回避</span><span class="sxs-lookup"><span data-stu-id="27904-145">Avoiding DbContext threading issues</span></span>

<span data-ttu-id="27904-146">Entity Framework Core では、同じインスタンスで実行されている複数の並列操作はサポートされていません `DbContext` 。</span><span class="sxs-lookup"><span data-stu-id="27904-146">Entity Framework Core does not support multiple parallel operations being run on the same `DbContext` instance.</span></span> <span data-ttu-id="27904-147">これには、非同期クエリの並列実行と、複数のスレッドからの明示的な同時使用の両方が含まれます。</span><span class="sxs-lookup"><span data-stu-id="27904-147">This includes both parallel execution of async queries and any explicit concurrent use from multiple threads.</span></span> <span data-ttu-id="27904-148">したがって、常に `await` 非同期呼び出しをすぐに `DbContext` 実行するか、並列で実行される操作に個別のインスタンスを使用します。</span><span class="sxs-lookup"><span data-stu-id="27904-148">Therefore, always `await` async calls immediately, or use separate `DbContext` instances for operations that execute in parallel.</span></span>

<span data-ttu-id="27904-149">EF Core がインスタンスを同時に使用しようとしたことを検出すると、次のようなメッセージが表示され `DbContext` `InvalidOperationException` ます。</span><span class="sxs-lookup"><span data-stu-id="27904-149">When EF Core detects an attempt to use a `DbContext` instance concurrently, you'll see an `InvalidOperationException` with a message like this:</span></span>

> <span data-ttu-id="27904-150">前の操作が完了する前に、このコンテキストで2番目の操作が開始されました。</span><span class="sxs-lookup"><span data-stu-id="27904-150">A second operation started on this context before a previous operation completed.</span></span> <span data-ttu-id="27904-151">これは通常、同じ DbContext のインスタンスを使用する異なるスレッドによって発生しますが、インスタンスメンバーはスレッドセーフであるとは限りません。</span><span class="sxs-lookup"><span data-stu-id="27904-151">This is usually caused by different threads using the same instance of DbContext, however instance members are not guaranteed to be thread safe.</span></span>

<span data-ttu-id="27904-152">同時アクセスが検出されない場合は、未定義の動作、アプリケーションのクラッシュ、データの破損が発生する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="27904-152">When concurrent access goes undetected, it can result in undefined behavior, application crashes and data corruption.</span></span>

<span data-ttu-id="27904-153">同じインスタンスに対して、誤って同時アクセスを引き起こす可能性がある一般的な誤りがあり `DbContext` ます。</span><span class="sxs-lookup"><span data-stu-id="27904-153">There are common mistakes that can inadvertently cause concurrent access on the same `DbContext` instance:</span></span>

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a><span data-ttu-id="27904-154">同じ DbContext で他の操作を開始する前に、非同期操作の完了を待機していません</span><span class="sxs-lookup"><span data-stu-id="27904-154">Forgetting to await the completion of an asynchronous operation before starting any other operation on the same DbContext</span></span>

<span data-ttu-id="27904-155">非同期メソッドを使用すると、ブロックしない方法でデータベースにアクセスする操作を開始 EF Core ことができます。</span><span class="sxs-lookup"><span data-stu-id="27904-155">Asynchronous methods enable EF Core to initiate operations that access the database in a non-blocking way.</span></span> <span data-ttu-id="27904-156">ただし、呼び出し元がこれらのメソッドのいずれかの完了を待機しておらず、に対して他の操作を実行しようとした場合、 `DbContext` の状態は `DbContext` (おそらく) 破損している可能性があります。</span><span class="sxs-lookup"><span data-stu-id="27904-156">But if a caller does not await the completion of one of these methods, and proceeds to perform other operations on the `DbContext`, the state of the `DbContext` can be, (and very likely will be) corrupted.</span></span>

<span data-ttu-id="27904-157">常に非同期メソッド EF Core 待機します。</span><span class="sxs-lookup"><span data-stu-id="27904-157">Always await EF Core asynchronous methods immediately.</span></span>  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a><span data-ttu-id="27904-158">依存関係の挿入によって複数のスレッド間で DbContext インスタンスを暗黙的に共有する</span><span class="sxs-lookup"><span data-stu-id="27904-158">Implicitly sharing DbContext instances across multiple threads via dependency injection</span></span>

<span data-ttu-id="27904-159">[`AddDbContext`](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext)拡張メソッドは、 `DbContext` 既定でスコープを持つ[有効期間](/aspnet/core/fundamentals/dependency-injection#service-lifetimes)を持つ型を登録します。</span><span class="sxs-lookup"><span data-stu-id="27904-159">The [`AddDbContext`](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) extension method registers `DbContext` types with a [scoped lifetime](/aspnet/core/fundamentals/dependency-injection#service-lifetimes) by default.</span></span>

<span data-ttu-id="27904-160">これは、特定の時点で各クライアント要求を実行するスレッドが1つだけであり、各要求が個別の依存関係挿入スコープ (したがって別のインスタンス) を取得するため、ほとんどの ASP.NET Core アプリケーションの同時アクセスの問題から安全です `DbContext` 。</span><span class="sxs-lookup"><span data-stu-id="27904-160">This is safe from concurrent access issues in most ASP.NET Core applications because there is only one thread executing each client request at a given time, and because each request gets a separate dependency injection scope (and therefore a separate `DbContext` instance).</span></span> <span data-ttu-id="27904-161">Blazor Server ホスティングモデルでは、Blazor ユーザー回線を維持するために1つの論理要求が使用されます。そのため、既定の挿入スコープが使用されている場合は、ユーザー回線ごとに1つのスコープされた DbContext インスタンスのみ使用できます。</span><span class="sxs-lookup"><span data-stu-id="27904-161">For Blazor Server hosting model, one logical request is used for maintaining the Blazor user circuit, and thus only one scoped DbContext instance is available per user circuit if the default injection scope is used.</span></span>

<span data-ttu-id="27904-162">複数のスレッドを明示的に並列実行するコードは、インスタンスに同時にアクセスされないようにする必要があり `DbContext` ます。</span><span class="sxs-lookup"><span data-stu-id="27904-162">Any code that explicitly executes multiple threads in parallel should ensure that `DbContext` instances aren't ever accessed concurrently.</span></span>

<span data-ttu-id="27904-163">依存関係の挿入を使用すると、コンテキストをスコープとして登録し、各スレッドに対して (を使用して) スコープを作成するか `IServiceScopeFactory` 、を `DbContext` (パラメーターを受け取るのオーバーロードを使用して) 一時的に登録することによって実現でき `AddDbContext` `ServiceLifetime` ます。</span><span class="sxs-lookup"><span data-stu-id="27904-163">Using dependency injection, this can be achieved by either registering the context as scoped, and creating scopes (using `IServiceScopeFactory`) for each thread, or by registering the `DbContext` as transient (using the overload of `AddDbContext` which takes a `ServiceLifetime` parameter).</span></span>

## <a name="more-reading"></a><span data-ttu-id="27904-164">その他の参考資料</span><span class="sxs-lookup"><span data-stu-id="27904-164">More reading</span></span>

- <span data-ttu-id="27904-165">DI の使用方法の詳細については、「 [依存関係の挿入](/aspnet/core/fundamentals/dependency-injection) 」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="27904-165">Read [Dependency Injection](/aspnet/core/fundamentals/dependency-injection) to learn more about using DI.</span></span>
- <span data-ttu-id="27904-166">詳細については、「 [テスト](xref:core/miscellaneous/testing/index) 」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="27904-166">Read [Testing](xref:core/miscellaneous/testing/index) for more information.</span></span>
