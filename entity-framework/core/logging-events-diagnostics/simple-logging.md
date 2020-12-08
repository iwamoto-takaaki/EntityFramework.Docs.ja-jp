---
title: 単純なログ記録-EF Core
description: LogTo を使用した EF Core DbContext からのログ記録
author: ajcvickers
ms.date: 10/03/2020
uid: core/logging-events-diagnostics/simple-logging
ms.openlocfilehash: 274fca39c45c4e2ccfd47f3c4eeb0834e00d18aa
ms.sourcegitcommit: 788a56c2248523967b846bcca0e98c2ed7ef0d6b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2020
ms.locfileid: "95003511"
---
# <a name="simple-logging"></a>シンプルなログ

> [!NOTE]
> この機能は EF Core 5.0 で導入されました。

> [!TIP]  
> [この記事のサンプル](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/SimpleLogging)は GitHub からダウンロードできます。

Entity Framework Core (EF Core) 単純なログを使用して、アプリケーションの開発とデバッグを行うときに簡単にログを取得できます。 この形式のログ記録では、最小限の構成と追加の NuGet パッケージは必要ありません。

> [!TIP]
> また EF Core は、さらに多くの構成を必要とする [が、多く](xref:core/logging-events-diagnostics/extensions-logging)の場合、実稼働アプリケーションでのログ記録に適しています。

## <a name="configuration"></a>構成

[LogTo](https://github.com/dotnet/efcore/blob/ec3df8fd7e4ea4ebeebfa747619cef37b23ab2c6/src/EFCore/DbContextOptionsBuilder.cs#L135) を使用すると、任意の種類のアプリケーションから EF Core ログにアクセスできます <!-- Issue #2748 <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.LogTo%2A> --> ([DbContext インスタンスの構成時](xref:core/dbcontext-configuration/index))。 この構成は、通常、<xref:Microsoft.EntityFrameworkCore.DbContext.OnConfiguring%2A?displayProperty=nameWithType> のオーバーライドで行われます。 次に例を示します。

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(Console.WriteLine);
-->
[!code-csharp[LogToConsole](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=LogToConsole)]

`LogTo`または、の一部として、 <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext%2A> または <xref:Microsoft.EntityFrameworkCore.DbContextOptions> コンストラクターに渡すインスタンスを作成するときに、を呼び出すことができ `DbContext` ます。

> [!TIP]
> AddDbContext が使用されている場合、または DbContext コンストラクターに DbContextOptions インスタンスが渡された場合、OnConfiguring は引き続き呼び出されます。 これにより、DbContext の構築方法に関係なく、コンテキスト構成を適用するための理想的な場所となります。

## <a name="directing-the-logs"></a>ログの指示

### <a name="logging-to-the-console"></a>コンソールへのログ記録

`LogTo`<xref:System.Action%601>文字列を受け入れるデリゲートが必要です。 EF Core は、生成されたログメッセージごとに文字列を使用して、このデリゲートを呼び出します。 次に、指定されたメッセージで何かを実行するデリゲートが作成されます。

<xref:System.Console.WriteLine%2A?displayProperty=nameWithType>このデリゲートには、前述のようにメソッドがよく使用されます。 この結果、各ログメッセージがコンソールに書き込まれます。

### <a name="logging-to-the-debug-window"></a>[デバッグ] ウィンドウへのログ記録

<xref:System.Diagnostics.Debug.WriteLine%2A?displayProperty=nameWithType> は、Visual Studio または他の Ide のデバッグウィンドウに出力を送信するために使用できます。 この場合、クラスはリリースビルドからコンパイルされるため、[ラムダ構文](/dotnet/csharp/language-reference/operators/lambda-expressions)を使用する必要があり `Debug` ます。 次に例を示します。

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(message => Debug.WriteLine(message));
-->
[!code-csharp[LogToDebug](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=LogToDebug)]

### <a name="logging-to-a-file"></a>ファイルへのログ記録

ファイルに書き込むに <xref:System.IO.StreamWriter> は、ファイルのまたは同様のを作成する必要があります。 この <xref:System.IO.StreamWriter.WriteLine%2A> メソッドは、上記の他の例のように使用できます。 コンテキストが破棄されたときにライターを破棄することによって、ファイルが正常に閉じられていることを確認してください。 次に例を示します。

<!--
    private readonly StreamWriter _logStream = new StreamWriter("mylog.txt", append: true); 

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(_logStream.WriteLine);

    public override void Dispose()
    {
        base.Dispose();
        _logStream.Dispose();
    }
    
    public override async ValueTask DisposeAsync()
    {
        await base.DisposeAsync();
        await _logStream.DisposeAsync();
    }
-->
[!code-csharp[LogToFile](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=LogToFile)]

> [!TIP]
> 実稼働アプリケーションのファイルへのログ記録には、 [Microsoft の拡張子](/aspnet/core/fundamentals/logging) を使用することを検討してください。

## <a name="getting-detailed-messages"></a>詳細なメッセージを取得する

### <a name="sensitive-data"></a>機微なデータ

既定では、EF Core には、例外メッセージ内のデータの値は含まれません。 これは、このようなデータは機密情報である可能性があるため、例外が処理されない場合に実稼働環境でも明らかになる可能性があるためです。

ただし、特にキーのデータ値を把握することは、デバッグ時に非常に役立ちます。 これは、を呼び出すことによって EF Core で有効にすることができ <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.EnableSensitiveDataLogging> ます。 次に例を示します。

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .LogTo(Console.WriteLine)
            .EnableSensitiveDataLogging();
-->
[!code-csharp[SensitiveDataLogging](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=SensitiveDataLogging)]

### <a name="detailed-query-exceptions"></a>詳細なクエリの例外

パフォーマンス上の理由により、EF Core は、try-catch ブロック内のデータベースプロバイダーから値を読み取る各呼び出しをラップしません。 ただし、これにより、特に、モデルで許可されていない場合にデータベースが NULL を返す場合に、診断が困難な例外が発生することがあります。

オンに <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.EnableDetailedErrors%2A> すると、EF によってこれらの try-catch ブロックが導入されるため、より詳細なエラーが発生します。 次に例を示します。

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .LogTo(Console.WriteLine)
            .EnableDetailedErrors();
-->
[!code-csharp[EnableDetailedErrors](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=EnableDetailedErrors)]

## <a name="filtering"></a>フィルター処理

### <a name="log-levels"></a>ログ レベル

すべての EF Core ログメッセージは、列挙型によって定義されたレベルに割り当てられ <xref:Microsoft.Extensions.Logging.LogLevel> ます。 既定では EF Core 簡易ログ記録には、レベル以上のすべてのメッセージが含まれ `Debug` ます。 `LogTo` は、一部のメッセージをフィルターで除外するために、より高いレベルを渡すことができます。 たとえば、を渡す `Information` と、データベースアクセスと一部のハウスキーピングメッセージに限定されたログの最小セットが得られます。

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(Console.WriteLine, LogLevel.Information);
-->
[!code-csharp[InfoOnly](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=InfoOnly)]

### <a name="specific-messages"></a>特定のメッセージ

すべてのログメッセージにはが割り当てられ <xref:Microsoft.Extensions.Logging.EventId> ます。 これらの Id <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId> には、クラスから、または <xref:Microsoft.EntityFrameworkCore.Diagnostics.RelationalEventId> リレーショナル固有のメッセージのクラスからアクセスできます。 データベースプロバイダーには、同様のクラスのプロバイダー固有の Id が含まれる場合もあります。 たとえば、 <xref:Microsoft.EntityFrameworkCore.Diagnostics.SqlServerEventId> SQL Server プロバイダーの場合はです。

`LogTo` は、1つまたは複数のイベント Id に関連付けられたメッセージのみをログに記録するように構成できます。 たとえば、初期化または破棄されたコンテキストのメッセージのみをログに記録するには、次のようにします。  

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .LogTo(Console.WriteLine, new[] { CoreEventId.ContextDisposed, CoreEventId.ContextInitialized });
-->
[!code-csharp[EventIds](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=EventIds)]

### <a name="message-categories"></a>メッセージの種類

すべてのログメッセージは、名前付きの階層 logger カテゴリに割り当てられます。 カテゴリは次のとおりです。

| カテゴリ                                                | [メッセージ]
|:-----------------------------------------------------|-------------------------------------------------
| Microsoft.EntityFrameworkCore                        | すべての EF Core メッセージ
| Microsoft EntityFrameworkCore.Database               | すべてのデータベースの相互作用
| Microsoft EntityFrameworkCore.Database.Connection    | データベース接続の使用
| Microsoft EntityFrameworkCore.Database.Command       | データベースコマンドの使用
| Microsoft EntityFrameworkCore.Database.Transaction   | データベーストランザクションの使用
| Microsoft EntityFrameworkCore.Updata                 | エンティティの保存、データベースとのやり取りの除外
| Microsoft EntityFrameworkCore.Model                  | すべてのモデルとメタデータの相互作用
| Microsoft EntityFrameworkCore.Model.Validation       | モデルの検証
| Microsoft EntityFrameworkCore.Query                  | クエリ、データベースとのやり取りの除外
| Microsoft EntityFrameworkCore.Infrastructure         | 一般的なイベント (コンテキストの作成など)
| Microsoft EntityFrameworkCore.Scaffolding            | データベースのリバースエンジニアリング
| Microsoft EntityFrameworkCore.Migrations             | 移行
| Microsoft EntityFrameworkCore.ChangeTracking         | 変更の追跡の相互作用

`LogTo` は、1つまたは複数のカテゴリからのメッセージのみをログに記録するように構成できます。 たとえば、データベースの相互作用のみをログに記録するには、次のようにします。

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .LogTo(Console.WriteLine, new[] { DbLoggerCategory.Database.Name });
-->
[!code-csharp[DatabaseCategory](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=DatabaseCategory)]

クラスは、 <xref:Microsoft.EntityFrameworkCore.DbLoggerCategory> カテゴリを検索するための階層的な API を提供し、文字列をハードコーディングする必要がないことに注意してください。

カテゴリは階層構造であるため、カテゴリを使用するこの例に `Database` は、サブカテゴリのすべてのメッセージ (、、および) が含まれ `Database.Connection` `Database.Command` `Database.Transaction` ます。

### <a name="custom-filters"></a>カスタム フィルター

`LogTo` 上のフィルターオプションが十分でない場合に、カスタムフィルターを使用できるようにします。 たとえば、レベル以上のメッセージ、 `Information` および接続を開いたり閉じたりするためのメッセージをログに記録する場合は、次のようになります。

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .LogTo(
                Console.WriteLine,
                (eventId, logLevel) => logLevel >= LogLevel.Information
                                       || eventId == RelationalEventId.ConnectionOpened
                                       || eventId == RelationalEventId.ConnectionClosed);
-->
[!code-csharp[CustomFilter](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=CustomFilter)]

> [!TIP]
> カスタムフィルターを使用したフィルター処理、またはここに示されている他のオプションの使用は、LogTo デリゲートでのフィルター処理よりも効率的です。 これは、フィルターによってメッセージがログに記録されないと判断された場合、ログメッセージは作成されません。

## <a name="configuration-for-specific-messages"></a>特定のメッセージの構成

EF Core <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.ConfigureWarnings%2A> API を使用すると、特定のイベントが発生したときの動作をアプリケーションで変更できます。 これは次の場合に使用できます。

* イベントがログに記録されるログレベルを変更する
* イベントのログ記録をスキップする
* イベントが発生したときに例外をスローする

### <a name="changing-the-log-level-for-an-event"></a>イベントのログレベルの変更

前の例では、カスタムフィルターを使用して、 `LogLevel.Information` のすべてのメッセージと、に対して定義されている2つのイベントをログに記録して `LogLevel.Debug` います。 次の2つのイベントのログレベルをに変更することで、同じことを実現でき `Debug` `Information` ます。

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .ConfigureWarnings(b => b.Log(
                (RelationalEventId.ConnectionOpened, LogLevel.Information),
                (RelationalEventId.ConnectionClosed, LogLevel.Information)))
            .LogTo(Console.WriteLine, LogLevel.Information);
-->
[!code-csharp[ChangeLogLevel](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=ChangeLogLevel)]

### <a name="suppress-logging-an-event"></a>イベントのログ記録を抑制する

同様に、個々のイベントをログに記録しないようにすることもできます。 これは、確認して理解した警告を無視する場合に特に便利です。 次に例を示します。

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .ConfigureWarnings(b => b.Ignore(CoreEventId.DetachedLazyLoadingWarning))
            .LogTo(Console.WriteLine);
-->
[!code-csharp[SuppressMessage](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=SuppressMessage)]

### <a name="throw-for-an-event"></a>イベントに対してスローする

最後に、特定のイベントに対してをスローするように EF Core を構成できます。 これは、警告をエラーに変更する場合に特に便利です。 (実際には、これはメソッドの本来の目的であるため、という `ConfigureWarnings` 名前です)。例えば：

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder
            .ConfigureWarnings(b => b.Throw(RelationalEventId.MultipleCollectionIncludeWarning))
            .LogTo(Console.WriteLine);
-->
[!code-csharp[ThrowForEvent](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=ThrowForEvent)]

## <a name="message-contents-and-formatting"></a>メッセージの内容と書式設定

の既定の内容 `LogTo` は、複数の行にわたって書式設定されます。 最初の行には、メッセージのメタデータが含まれています。

* <xref:Microsoft.Extensions.Logging.LogLevel>4 文字のプレフィックスとしての
* 現在のカルチャに対して書式設定されたローカルタイムスタンプ
* <xref:Microsoft.Extensions.Logging.EventId>コピー/貼り付けを使用して、 <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId> または他のいずれかのクラスからのメンバーを取得し、 `EventId` 生の ID 値を含む、フォーム内の。
* 前に説明したように、イベントカテゴリ。

次に例を示します。

```output
info: 10/6/2020 10:52:45.581 RelationalEventId.CommandExecuted[20101] (Microsoft.EntityFrameworkCore.Database.Command)
      Executed DbCommand (0ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE "Blogs" (
          "Id" INTEGER NOT NULL CONSTRAINT "PK_Blogs" PRIMARY KEY AUTOINCREMENT,
          "Name" INTEGER NOT NULL
      );
dbug: 10/6/2020 10:52:45.582 RelationalEventId.TransactionCommitting[20210] (Microsoft.EntityFrameworkCore.Database.Transaction)
      Committing transaction.
dbug: 10/6/2020 10:52:45.585 RelationalEventId.TransactionCommitted[20202] (Microsoft.EntityFrameworkCore.Database.Transaction)
      Committed transaction.
```

このコンテンツは、 [DbContextLoggerOptions](https://github.com/dotnet/efcore/blob/ec3df8fd7e4ea4ebeebfa747619cef37b23ab2c6/src/EFCore/Diagnostics/DbContextLoggerOptions.cs#L15)から値を渡すことによってカスタマイズできます。 <!-- Issue #2748 <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbContextLoggerOptions> -->(次のセクションを参照)。

> [!TIP]
> ログの書式設定をより詳細に制御するには、 [Microsoft の拡張子](/aspnet/core/fundamentals/logging) を使用することを検討してください。

### <a name="using-utc-time"></a>UTC 時刻の使用

既定では、timestamnps はデバッグ中にローカルで使用するように設計されています。 `DbContextLoggerOptions.DefaultWithUtcTime`代わりに、カルチャに依存しない UTC タイムスタンプを使用するためにを使用します。ただし、その他はすべて同じままにしておきます。 次に例を示します。

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(
            Console.WriteLine,
            LogLevel.Debug,
            DbContextLoggerOptions.DefaultWithUtcTime);
-->
[!code-csharp[Utc](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=Utc)]

この例では、次のようにログの書式が設定されます。

```output
info: 2020-10-06T17:55:39.0333701Z RelationalEventId.CommandExecuted[20101] (Microsoft.EntityFrameworkCore.Database.Command)
      Executed DbCommand (0ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE "Blogs" (
          "Id" INTEGER NOT NULL CONSTRAINT "PK_Blogs" PRIMARY KEY AUTOINCREMENT,
          "Name" INTEGER NOT NULL
      );
dbug: 2020-10-06T17:55:39.0333892Z RelationalEventId.TransactionCommitting[20210] (Microsoft.EntityFrameworkCore.Database.Transaction)
      Committing transaction.
dbug: 2020-10-06T17:55:39.0351684Z RelationalEventId.TransactionCommitted[20202] (Microsoft.EntityFrameworkCore.Database.Transaction)
      Committed transaction.
```

### <a name="single-line-logging"></a>単一行のログ記録

ログメッセージごとに1行だけを取得すると便利な場合があります。 これは、で有効にすることができ `DbContextLoggerOptions.SingleLine` ます。 次に例を示します。

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(
            Console.WriteLine,
            LogLevel.Debug,
            DbContextLoggerOptions.DefaultWithLocalTime | DbContextLoggerOptions.SingleLine);
-->
[!code-csharp[SingleLine](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=SingleLine)]

この例では、次のようにログの書式が設定されます。

```output
info: 10/6/2020 10:52:45.723 RelationalEventId.CommandExecuted[20101] (Microsoft.EntityFrameworkCore.Database.Command) -> Executed DbCommand (0ms) [Parameters=[], CommandType='Text', CommandTimeout='30']CREATE TABLE "Blogs" (    "Id" INTEGER NOT NULL CONSTRAINT "PK_Blogs" PRIMARY KEY AUTOINCREMENT,    "Name" INTEGER NOT NULL);
dbug: 10/6/2020 10:52:45.723 RelationalEventId.TransactionCommitting[20210] (Microsoft.EntityFrameworkCore.Database.Transaction) -> Committing transaction.
dbug: 10/6/2020 10:52:45.725 RelationalEventId.TransactionCommitted[20202] (Microsoft.EntityFrameworkCore.Database.Transaction) -> Committed transaction.
```

### <a name="other-content-options"></a>その他のコンテンツオプション

[DbContextLoggerOptions](https://github.com/dotnet/efcore/blob/ec3df8fd7e4ea4ebeebfa747619cef37b23ab2c6/src/EFCore/Diagnostics/DbContextLoggerOptions.cs#L15)のその他のフラグ <!-- Issue #2748 <xref:Microsoft.EntityFrameworkCore.Diagnostics.DbContextLoggerOptions> --> を使用すると、ログに含まれるメタデータの量を減らすことができます。 これは、単一行ログと組み合わせて使用すると便利です。 次に例を示します。

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(
            Console.WriteLine,
            LogLevel.Debug,
            DbContextLoggerOptions.UtcTime | DbContextLoggerOptions.SingleLine);
-->
[!code-csharp[TerseLogs](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=TerseLogs)]

この例では、次のようにログの書式が設定されます。

```output
2020-10-06T17:52:45.7320362Z -> Executed DbCommand (0ms) [Parameters=[], CommandType='Text', CommandTimeout='30']CREATE TABLE "Blogs" (    "Id" INTEGER NOT NULL CONSTRAINT "PK_Blogs" PRIMARY KEY AUTOINCREMENT,    "Name" INTEGER NOT NULL);
2020-10-06T17:52:45.7320531Z -> Committing transaction.
2020-10-06T17:52:45.7339441Z -> Committed transaction.
```

## <a name="moving-from-ef6"></a>EF6 からの移動

EF Core 単純なログ記録は、 <xref:System.Data.Entity.Database.Log?displayProperty=nameWithType> 次の2つの重要な点で EF6 で異なります。

* ログメッセージは、データベースとの対話のみに制限されません。
* ログは、コンテキストの初期化時に構成する必要があります。

最初の違いとして、上記のフィルター処理を使用して、ログに記録するメッセージを制限できます。

2番目の違いは、不要になったログメッセージを生成しないことで、パフォーマンスを向上させるための意図的な変更です。 ただし、でプロパティを作成し、設定されている場合にのみ使用することで、EF6 と同様の動作を実現でき `Log` `DbContext` ます。 次に例を示します。

<!--
    public Action<string> Log { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(s => Log?.Invoke(s));
-->
[!code-csharp[DatabaseLog](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=DatabaseLog)]
