---
title: Entity Framework 6 の概要 - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: 28a13879416a52cbe8035c23013f16390c75c4c9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/07/2020
ms.locfileid: "78412757"
---
# <a name="entity-framework-6"></a>Entity Framework 6
Entity Framework 6 (EF6) は、機能開発および安定化に何年もの時間が費やされてきた、十分に検証された .NET 用のオブジェクト リレーショナル マッパー (O/RM) です。

EF6 は、O/RM として、リレーショナルおよびオブジェクト指向の世界のインピーダンス不整合を減らします。開発者はこれにより、アプリケーションのドメインを表す厳密に型指定された .NET オブジェクトを使用した、リレーショナル データベースに格納されたデータをやり取りするアプリケーションを記述でき、通常は記述する必要のあるデータ アクセス "プラミング" コードの大部分を記述する必要がなくなります。

EF6 には、次のような多数の一般的な O/RM 機能が実装されています。
- どの EF の種類にも依存しない [POCO](xref:ef6/resources/glossary#poco) エンティティ クラスのマッピング
- 変更の自動追跡
- ID 解決および作業単位
- Eager、lazy および明示的読み込み
- [LINQ (言語統合クエリ)](https://aka.ms/AA6hsvu) を使用した厳密に型指定されたクエリの変換
- 次などのサポートを含む、豊富なマッピング機能があります。
  - 一対一、一対多、および多対多のリレーションシップ
  - 継承 (Table-Per-Hierarchy、Table-Per-Type、Table-Per-Concrete-lass)
  - 複合型
  - ストアド プロシージャ
- エンティティ モデルを作成するビジュアル デザイナー。
- コードを記述することにより、エンティティ モデルを作成する "Code First" エクスペリエンス。
- モデルは既存のデータベースから生成し手作業で編集するか、最初から生成して新しいデータベースの作成に使用できます。
- ASP.NET を含む .NET Framework アプリケーション モデルとの統合および、WPF と WinForms を介したデータ バインド。
- ADO.NET に基づくデータベース接続、および SQL Server、Oracle、MySQL、SQLite、PostgreSQL、DB2 などへの接続に使用可能な多数の[プロバイダー](xref:ef6/fundamentals/providers/index)。

## <a name="should-i-use-ef6-or-ef-core"></a>EF6 または EF Core のどちらを使用すべきか

EF Core は、EF6 と非常によく似た機能およびメリットを持つ、Entity Framework の最新の簡易拡張バージョンです。
EF Core は完全に書き直されたもので、EF6 にはない機能が多数含まれています。ただし、EF6 にある最も高度ないくつかのマッピング機能は含まれていません。
機能セットが要件に合う場合は、新しいアプリケーション内で EF Core を使用することを検討してください。
「[EF Core と EF6 を比較する](xref:efcore-and-ef6/index)」ではこの選択についてより詳しく検証しています。

## <a name="get-started"></a>[開始するには](xref:ef6/get-started)

EntityFramework NuGet パッケージをプロジェクトに追加するか、[Entity Framework Tools for Visual Studio](https://aka.ms/AA6i8c5) をインストールします。 次いでビデオを見たり、チュートリアルやより高度なドキュメントを読み、EF6 を最大限に活用できるようにします。

## <a name="past-entity-framework-versions"></a>過去の Entity Framework のバージョン

これは、Entity Framework 6 の最新バージョン用のドキュメントですが、ほとんどが過去のリリースにも当てはまります。
EF のすべてのリリース一覧およびそれらによって導入された機能については、「[What's New](xref:ef6/what-is-new/index)」 (新機能) と「[Past Releases](xref:ef6/what-is-new/past-releases)」 (以前のリリース) を参照してください。
