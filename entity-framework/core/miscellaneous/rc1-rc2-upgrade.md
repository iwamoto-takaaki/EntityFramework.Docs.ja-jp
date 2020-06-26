---
title: EF Core 1.0 RC1 から RC2-EF Core へのアップグレード
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: 5500addc98a0c0eb314c4d9f9401fa301ce5c6f6
ms.sourcegitcommit: ebfd3382fc583bc90f0da58e63d6e3382b30aa22
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/25/2020
ms.locfileid: "85370527"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a><span data-ttu-id="dec94-102">EF Core 1.0 RC1 から 1.0 RC2 へのアップグレード</span><span class="sxs-lookup"><span data-stu-id="dec94-102">Upgrading from EF Core 1.0 RC1 to 1.0 RC2</span></span>

<span data-ttu-id="dec94-103">この記事では、RC1 パッケージでビルドされたアプリケーションを RC2 に移動するためのガイダンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="dec94-103">This article provides guidance for moving an application built with the RC1 packages to RC2.</span></span>

## <a name="package-names-and-versions"></a><span data-ttu-id="dec94-104">パッケージ名とバージョン</span><span class="sxs-lookup"><span data-stu-id="dec94-104">Package Names and Versions</span></span>

<span data-ttu-id="dec94-105">RC1 と RC2 の間では、"Entity Framework 7" から "Entity Framework Core" に変更されました。</span><span class="sxs-lookup"><span data-stu-id="dec94-105">Between RC1 and RC2, we changed from "Entity Framework 7" to "Entity Framework Core".</span></span> <span data-ttu-id="dec94-106">この投稿の変更の理由の詳細については、 [Scott サバイバル selman を](https://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx)参照してください。</span><span class="sxs-lookup"><span data-stu-id="dec94-106">You can read more about the reasons for the change in [this post by Scott Hanselman](https://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx).</span></span> <span data-ttu-id="dec94-107">この変更により、パッケージ名はからに変わり、 `EntityFramework.*` `Microsoft.EntityFrameworkCore.*` バージョンはから `7.0.0-rc1-final` `1.0.0-rc2-final` (または `1.0.0-preview1-final` ツール) に変更されました。</span><span class="sxs-lookup"><span data-stu-id="dec94-107">Because of this change, our package names changed from `EntityFramework.*` to `Microsoft.EntityFrameworkCore.*` and our versions from `7.0.0-rc1-final` to `1.0.0-rc2-final` (or `1.0.0-preview1-final` for tooling).</span></span>

<span data-ttu-id="dec94-108">**RC1 パッケージを完全に削除し、RC2 をインストールする必要があります。**</span><span class="sxs-lookup"><span data-stu-id="dec94-108">**You will need to completely remove the RC1 packages and then install the RC2 ones.**</span></span> <span data-ttu-id="dec94-109">いくつかの一般的なパッケージのマッピングを次に示します。</span><span class="sxs-lookup"><span data-stu-id="dec94-109">Here is the mapping for some common packages.</span></span>

| <span data-ttu-id="dec94-110">RC1 パッケージ</span><span class="sxs-lookup"><span data-stu-id="dec94-110">RC1 Package</span></span>                                               | <span data-ttu-id="dec94-111">RC2 同等</span><span class="sxs-lookup"><span data-stu-id="dec94-111">RC2 Equivalent</span></span>                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| <span data-ttu-id="dec94-112">EntityFramework. Microsoft Sqlserver 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="dec94-112">EntityFramework.MicrosoftSqlServer        7.0.0-rc1-final</span></span> | <span data-ttu-id="dec94-113">Microsoft EntityFrameworkCore. SqlServer 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="dec94-113">Microsoft.EntityFrameworkCore.SqlServer         1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="dec94-114">EntityFramework-rc1-final</span><span class="sxs-lookup"><span data-stu-id="dec94-114">EntityFramework.SQLite                    7.0.0-rc1-final</span></span> | <span data-ttu-id="dec94-115">Microsoft EntityFrameworkCore. Sqlite 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="dec94-115">Microsoft.EntityFrameworkCore.Sqlite            1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="dec94-116">EntityFramework7. Npgsql 3.1.0-rc1-3</span><span class="sxs-lookup"><span data-stu-id="dec94-116">EntityFramework7.Npgsql                   3.1.0-rc1-3</span></span>     | <span data-ttu-id="dec94-117">NpgSql. EntityFrameworkCore. Postgres<to be advised></span><span class="sxs-lookup"><span data-stu-id="dec94-117">NpgSql.EntityFrameworkCore.Postgres             <to be advised></span></span>      |
| <span data-ttu-id="dec94-118">EntityFramework. SqlServerCompact35 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="dec94-118">EntityFramework.SqlServerCompact35        7.0.0-rc1-final</span></span> | <span data-ttu-id="dec94-119">EntityFrameworkCore. SqlServerCompact35 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="dec94-119">EntityFrameworkCore.SqlServerCompact35          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="dec94-120">EntityFramework. SqlServerCompact40 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="dec94-120">EntityFramework.SqlServerCompact40        7.0.0-rc1-final</span></span> | <span data-ttu-id="dec94-121">EntityFrameworkCore. SqlServerCompact40 1.0.0-rc2-final</span><span class="sxs-lookup"><span data-stu-id="dec94-121">EntityFrameworkCore.SqlServerCompact40          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="dec94-122">EntityFramework-rc1-final</span><span class="sxs-lookup"><span data-stu-id="dec94-122">EntityFramework.InMemory                  7.0.0-rc1-final</span></span> | <span data-ttu-id="dec94-123">Microsoft EntityFrameworkCore. InMemory-rc2-final</span><span class="sxs-lookup"><span data-stu-id="dec94-123">Microsoft.EntityFrameworkCore.InMemory          1.0.0-rc2-final</span></span>      |
| <span data-ttu-id="dec94-124">EntityFramework. IBMDataServer-beta1</span><span class="sxs-lookup"><span data-stu-id="dec94-124">EntityFramework.IBMDataServer             7.0.0-beta1</span></span>     | <span data-ttu-id="dec94-125">RC2 ではまだ使用できません</span><span class="sxs-lookup"><span data-stu-id="dec94-125">Not yet available for RC2</span></span>                                            |
| <span data-ttu-id="dec94-126">EntityFramework. Commands 7.0.0-final</span><span class="sxs-lookup"><span data-stu-id="dec94-126">EntityFramework.Commands                  7.0.0-rc1-final</span></span> | <span data-ttu-id="dec94-127">Microsoft EntityFrameworkCore. ツール 1.0.0-preview1-final</span><span class="sxs-lookup"><span data-stu-id="dec94-127">Microsoft.EntityFrameworkCore.Tools             1.0.0-preview1-final</span></span> |
| <span data-ttu-id="dec94-128">EntityFramework. 7.0.0-rc1-final</span><span class="sxs-lookup"><span data-stu-id="dec94-128">EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final</span></span> | <span data-ttu-id="dec94-129">Microsoft EntityFrameworkCore. SqlServer. Design-rc2-final</span><span class="sxs-lookup"><span data-stu-id="dec94-129">Microsoft.EntityFrameworkCore.SqlServer.Design  1.0.0-rc2-final</span></span>      |

## <a name="namespaces"></a><span data-ttu-id="dec94-130">名前空間</span><span class="sxs-lookup"><span data-stu-id="dec94-130">Namespaces</span></span>

<span data-ttu-id="dec94-131">パッケージ名と共に、名前空間はからに変更されました `Microsoft.Data.Entity.*` `Microsoft.EntityFrameworkCore.*` 。</span><span class="sxs-lookup"><span data-stu-id="dec94-131">Along with package names, namespaces changed from `Microsoft.Data.Entity.*` to `Microsoft.EntityFrameworkCore.*`.</span></span> <span data-ttu-id="dec94-132">この変更は、の検索と置換を使用して処理でき `using Microsoft.Data.Entity` `using Microsoft.EntityFrameworkCore` ます。</span><span class="sxs-lookup"><span data-stu-id="dec94-132">You can handle this change with a find/replace of `using Microsoft.Data.Entity` with `using Microsoft.EntityFrameworkCore`.</span></span>

## <a name="table-naming-convention-changes"></a><span data-ttu-id="dec94-133">テーブルの名前付け規則の変更</span><span class="sxs-lookup"><span data-stu-id="dec94-133">Table Naming Convention Changes</span></span>

<span data-ttu-id="dec94-134">RC2 では、 `DbSet<TEntity>` クラス名だけではなく、マップ先のテーブル名として指定されたエンティティのプロパティ名を使用しました。</span><span class="sxs-lookup"><span data-stu-id="dec94-134">A significant functional change we took in RC2 was to use the name of the `DbSet<TEntity>` property for a given entity as the table name it maps to, rather than just the class name.</span></span> <span data-ttu-id="dec94-135">この変更の詳細については[、関連する発表](https://github.com/aspnet/Announcements/issues/167)に関する問題を参照してください。</span><span class="sxs-lookup"><span data-stu-id="dec94-135">You can read more about this change in [the related announcement issue](https://github.com/aspnet/Announcements/issues/167).</span></span>

<span data-ttu-id="dec94-136">既存の RC1 アプリケーションでは、 `OnModelCreating` rc1 の名前付け方法を維持するために、メソッドの先頭に次のコードを追加することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="dec94-136">For existing RC1 applications, we recommend adding the following code to the start of your `OnModelCreating` method to keep the RC1 naming strategy:</span></span>

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

<span data-ttu-id="dec94-137">新しい名前付け方法を採用する場合は、残りのアップグレード手順を正常に完了してから、コードを削除し、移行を作成してテーブルの名前変更を適用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="dec94-137">If you want to adopt the new naming strategy, we would recommend successfully completing the rest of the upgrade steps and then removing the code and creating a migration to apply the table renames.</span></span>

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a><span data-ttu-id="dec94-138">AddDbContext/Startup.cs Changes (ASP.NET Core プロジェクトのみ)</span><span class="sxs-lookup"><span data-stu-id="dec94-138">AddDbContext / Startup.cs Changes (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="dec94-139">RC1 では、アプリケーションサービスプロバイダーに Entity Framework サービスを追加する必要がありました `Startup.ConfigureServices(...)` 。</span><span class="sxs-lookup"><span data-stu-id="dec94-139">In RC1, you had to add Entity Framework services to the application service provider - in `Startup.ConfigureServices(...)`:</span></span>

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="dec94-140">RC2 では、、などの呼び出しを削除でき `AddEntityFramework()` `AddSqlServer()` ます。</span><span class="sxs-lookup"><span data-stu-id="dec94-140">In RC2, you can remove the calls to `AddEntityFramework()`, `AddSqlServer()`, etc.:</span></span>

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

<span data-ttu-id="dec94-141">また、コンテキストオプションを受け取り、基本コンストラクターに渡すコンストラクターを派生コンテキストに追加する必要もあります。</span><span class="sxs-lookup"><span data-stu-id="dec94-141">You also need to add a constructor, to your derived context, that takes context options and passes them to the base constructor.</span></span> <span data-ttu-id="dec94-142">このことが必要になるのは、背後で仕込んする恐ろしいマジックの一部を削除したためです。</span><span class="sxs-lookup"><span data-stu-id="dec94-142">This is needed because we removed some of the scary magic that snuck them in behind the scenes:</span></span>

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a><span data-ttu-id="dec94-143">IServiceProvider を渡す</span><span class="sxs-lookup"><span data-stu-id="dec94-143">Passing in an IServiceProvider</span></span>

<span data-ttu-id="dec94-144">をコンテキストに渡す RC1 コードがある場合 `IServiceProvider` 、これは `DbContextOptions` 別のコンストラクターパラメーターではなく、に移動されました。</span><span class="sxs-lookup"><span data-stu-id="dec94-144">If you have RC1 code that passes an `IServiceProvider` to the context, this has now moved to `DbContextOptions`, rather than being a separate constructor parameter.</span></span> <span data-ttu-id="dec94-145">`DbContextOptionsBuilder.UseInternalServiceProvider(...)`サービスプロバイダーを設定するには、を使用します。</span><span class="sxs-lookup"><span data-stu-id="dec94-145">Use `DbContextOptionsBuilder.UseInternalServiceProvider(...)` to set the service provider.</span></span>

### <a name="testing"></a><span data-ttu-id="dec94-146">テスト</span><span class="sxs-lookup"><span data-stu-id="dec94-146">Testing</span></span>

<span data-ttu-id="dec94-147">これを行う最も一般的なシナリオは、テスト時に InMemory データベースのスコープを制御することでした。</span><span class="sxs-lookup"><span data-stu-id="dec94-147">The most common scenario for doing this was to control the scope of an InMemory database when testing.</span></span> <span data-ttu-id="dec94-148">RC2 を使用した例については、更新された[テスト](testing/index.md)に関する記事をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="dec94-148">See the updated [Testing](testing/index.md) article for an example of doing this with RC2.</span></span>

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a><span data-ttu-id="dec94-149">アプリケーションサービスプロバイダーからの内部サービスの解決 (ASP.NET Core プロジェクトのみ)</span><span class="sxs-lookup"><span data-stu-id="dec94-149">Resolving Internal Services from Application Service Provider (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="dec94-150">ASP.NET Core アプリケーションがあり、EF でアプリケーションサービスプロバイダーから内部サービスを解決する必要がある場合は、次のように構成できるのオーバーロードがあり `AddDbContext` ます。</span><span class="sxs-lookup"><span data-stu-id="dec94-150">If you have an ASP.NET Core application and you want EF to resolve internal services from the application service provider, there is an overload of `AddDbContext` that allows you to configure this:</span></span>

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider));
```

> [!WARNING]  
> <span data-ttu-id="dec94-151">内部の EF サービスをアプリケーションサービスプロバイダーに結合する理由がない限り、EF が独自のサービスを内部で管理できるようにすることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="dec94-151">We recommend allowing EF to internally manage its own services, unless you have a reason to combine the internal EF services into your application service provider.</span></span> <span data-ttu-id="dec94-152">これを行う主な理由は、EF が内部で使用するサービスをアプリケーションサービスプロバイダーを使用して置き換えることです。</span><span class="sxs-lookup"><span data-stu-id="dec94-152">The main reason you may want to do this is to use your application service provider to replace services that EF uses internally</span></span>

## <a name="dnx-commands--net-core-cli-aspnet-core-projects-only"></a><span data-ttu-id="dec94-153">DNX コマンド => .NET Core CLI (ASP.NET Core プロジェクトのみ)</span><span class="sxs-lookup"><span data-stu-id="dec94-153">DNX Commands => .NET Core CLI (ASP.NET Core Projects Only)</span></span>

<span data-ttu-id="dec94-154">ASP.NET 5 プロジェクト用のコマンドを以前に使用したことがある場合は `dnx ef` 、コマンドに移動しました `dotnet ef` 。</span><span class="sxs-lookup"><span data-stu-id="dec94-154">If you previously used the `dnx ef` commands for ASP.NET 5 projects, these have now moved to `dotnet ef` commands.</span></span> <span data-ttu-id="dec94-155">同じコマンド構文が引き続き適用されます。</span><span class="sxs-lookup"><span data-stu-id="dec94-155">The same command syntax still applies.</span></span> <span data-ttu-id="dec94-156">構文情報にはを使用でき `dotnet ef --help` ます。</span><span class="sxs-lookup"><span data-stu-id="dec94-156">You can use `dotnet ef --help` for syntax information.</span></span>

<span data-ttu-id="dec94-157">コマンドの登録方法は RC2 で変更されています。 DNX が .NET Core CLI に置き換えられているためです。</span><span class="sxs-lookup"><span data-stu-id="dec94-157">The way commands are registered has changed in RC2, due to DNX being replaced by the .NET Core CLI.</span></span> <span data-ttu-id="dec94-158">コマンドがのセクションに登録されるようになりました `tools` `project.json` 。</span><span class="sxs-lookup"><span data-stu-id="dec94-158">Commands are now registered in a `tools` section in `project.json`:</span></span>

``` json
"tools": {
  "Microsoft.EntityFrameworkCore.Tools": {
    "version": "1.0.0-preview1-final",
    "imports": [
      "portable-net45+win8+dnxcore50",
      "portable-net45+win8"
    ]
  }
}
```

> [!TIP]  
> <span data-ttu-id="dec94-159">Visual Studio を使用する場合は、パッケージマネージャーコンソールを使用して ASP.NET Core プロジェクトに対して EF コマンドを実行できるようになりました (これは RC1 ではサポートされていません)。</span><span class="sxs-lookup"><span data-stu-id="dec94-159">If you use Visual Studio, you can now use Package Manager Console to run EF commands for ASP.NET Core projects (this was not supported in RC1).</span></span> <span data-ttu-id="dec94-160">このためには、 `tools` 「」の「」セクションにコマンドを登録する必要があり `project.json` ます。</span><span class="sxs-lookup"><span data-stu-id="dec94-160">You still need to register the commands in the `tools` section of `project.json` to do this.</span></span>

## <a name="package-manager-commands-require-powershell-5"></a><span data-ttu-id="dec94-161">パッケージマネージャーのコマンドには PowerShell 5 が必要です</span><span class="sxs-lookup"><span data-stu-id="dec94-161">Package Manager Commands Require PowerShell 5</span></span>

<span data-ttu-id="dec94-162">Visual Studio のパッケージマネージャーコンソールで Entity Framework コマンドを使用する場合は、PowerShell 5 がインストールされていることを確認する必要があります。</span><span class="sxs-lookup"><span data-stu-id="dec94-162">If you use the Entity Framework commands in Package Manager Console in Visual Studio, then you will need to ensure you have PowerShell 5 installed.</span></span> <span data-ttu-id="dec94-163">これは、次のリリースで削除される一時的な要件です (詳細については、「[問題 #5327](https://github.com/aspnet/EntityFramework/issues/5327) 」を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="dec94-163">This is a temporary requirement that will be removed in the next release (see [issue #5327](https://github.com/aspnet/EntityFramework/issues/5327) for more details).</span></span>

## <a name="using-imports-in-projectjson"></a><span data-ttu-id="dec94-164">project.jsでの "imports" の使用</span><span class="sxs-lookup"><span data-stu-id="dec94-164">Using "imports" in project.json</span></span>

<span data-ttu-id="dec94-165">EF Core の依存関係の中には、まだ .NET Standard をサポートしていないものがあります。</span><span class="sxs-lookup"><span data-stu-id="dec94-165">Some of EF Core's dependencies do not support .NET Standard yet.</span></span> <span data-ttu-id="dec94-166">.NET Standard および .NET Core プロジェクトの EF Core には、一時的な回避策として project.jsに "imports" を追加することが必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="dec94-166">EF Core in .NET Standard and .NET Core projects may require adding "imports" to project.json as a temporary workaround.</span></span>

<span data-ttu-id="dec94-167">EF を追加すると、次のエラーメッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="dec94-167">When adding EF, NuGet restore will display this error message:</span></span>

``` Console
Package Ix-Async 1.2.5 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Ix-Async 1.2.5 supports:
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8 (.NETPortable,Version=v0.0,Profile=Profile78)
Package Remotion.Linq 2.0.2 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Remotion.Linq 2.0.2 supports:
  - net35 (.NETFramework,Version=v3.5)
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8+wpa81 (.NETPortable,Version=v0.0,Profile=Profile259)
```

<span data-ttu-id="dec94-168">この回避策は、ポータブルプロファイル "net451 + win8" を手動でインポートすることです。</span><span class="sxs-lookup"><span data-stu-id="dec94-168">The workaround is to manually import the portable profile "portable-net451+win8".</span></span> <span data-ttu-id="dec94-169">これにより、NuGet は、これに一致するこのバイナリを .NET Standard と互換性のあるフレームワークとして処理します。</span><span class="sxs-lookup"><span data-stu-id="dec94-169">This forces NuGet to treat this binaries that match this provide as a compatible framework with .NET Standard, even though they are not.</span></span> <span data-ttu-id="dec94-170">"Net451 + win8" は、.NET Standard と互換性が100% ではありませんが、PCL から .NET Standard への移行に十分対応しています。</span><span class="sxs-lookup"><span data-stu-id="dec94-170">Although "portable-net451+win8" is not 100% compatible with .NET Standard, it is compatible enough for the transition from PCL to .NET Standard.</span></span> <span data-ttu-id="dec94-171">EF の依存関係が最終的に .NET Standard にアップグレードされると、インポートを削除できます。</span><span class="sxs-lookup"><span data-stu-id="dec94-171">Imports can be removed when EF's dependencies eventually upgrade to .NET Standard.</span></span>

<span data-ttu-id="dec94-172">配列構文では、複数のフレームワークを "インポート" に追加できます。</span><span class="sxs-lookup"><span data-stu-id="dec94-172">Multiple frameworks can be added to "imports" in array syntax.</span></span> <span data-ttu-id="dec94-173">その他のインポートは、プロジェクトにライブラリを追加する場合に必要になることがあります。</span><span class="sxs-lookup"><span data-stu-id="dec94-173">Other imports may be necessary if you add additional libraries to your project.</span></span>

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

<span data-ttu-id="dec94-174">「[問題 #5176](https://github.com/aspnet/EntityFramework/issues/5176)」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="dec94-174">See [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).</span></span>
