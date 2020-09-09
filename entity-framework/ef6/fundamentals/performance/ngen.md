---
title: NGen を使用した起動時のパフォーマンスの向上-EF6
description: Entity Framework 6 での NGen による起動時のパフォーマンスの向上
author: divega
ms.date: 10/23/2016
ms.assetid: dc6110a0-80a0-4370-8190-cea942841cee
uid: ef6/fundamentals/performance/ngen
ms.openlocfilehash: 53b9c2147c95fe096d459ad195a0549b32b67155
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/09/2020
ms.locfileid: "89616089"
---
# <a name="improving-startup-performance-with-ngen"></a><span data-ttu-id="98fc1-103">NGen を使用した起動時のパフォーマンスの向上</span><span class="sxs-lookup"><span data-stu-id="98fc1-103">Improving startup performance with NGen</span></span>
> [!NOTE]
> <span data-ttu-id="98fc1-104">**EF6 以降のみ** - このページで説明する機能、API などは、Entity Framework 6 で導入されました。</span><span class="sxs-lookup"><span data-stu-id="98fc1-104">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="98fc1-105">以前のバージョンを使用している場合、一部またはすべての情報は適用されません。</span><span class="sxs-lookup"><span data-stu-id="98fc1-105">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="98fc1-106">.NET Framework は、アプリケーションの起動を高速化する手段として、マネージアプリケーションとライブラリのネイティブイメージの生成をサポートしています。また、場合によっては、使用するメモリが少なくなります。</span><span class="sxs-lookup"><span data-stu-id="98fc1-106">The .NET Framework supports the generation of native images for managed applications and libraries as a way to help applications start faster and also in some cases use less memory.</span></span> <span data-ttu-id="98fc1-107">ネイティブイメージは、マネージコードアセンブリをアプリケーションの実行前にネイティブコンピューター命令を含むファイルに変換することによって作成されます。これにより、.NET JIT (Just-in-time) コンパイラは、アプリケーションランタイムでネイティブ命令を生成する必要がなくなります。</span><span class="sxs-lookup"><span data-stu-id="98fc1-107">Native images are created by translating managed code assemblies into files containing native machine instructions before the application is executed, relieving the .NET JIT (Just-In-Time) compiler from having to generate the native instructions at application runtime.</span></span>  

<span data-ttu-id="98fc1-108">Version 6 より前のバージョンでは、EF ランタイムのコアライブラリは .NET Framework に含まれており、ネイティブイメージは自動的に生成されました。</span><span class="sxs-lookup"><span data-stu-id="98fc1-108">Prior to version 6, the EF runtime’s core libraries were part of the .NET Framework and native images were generated automatically for them.</span></span> <span data-ttu-id="98fc1-109">バージョン6以降では、EF ランタイム全体が EntityFramework NuGet パッケージに結合されています。</span><span class="sxs-lookup"><span data-stu-id="98fc1-109">Starting with version 6 the whole EF runtime has been combined into the EntityFramework NuGet package.</span></span> <span data-ttu-id="98fc1-110">同様の結果を得るには、NGen.exe コマンドラインツールを使用してネイティブイメージを生成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="98fc1-110">Native images have to now be generated using the NGen.exe command line tool to obtain similar results.</span></span>  

<span data-ttu-id="98fc1-111">経験上の観察は、EF ランタイムアセンブリのネイティブイメージが、アプリケーションの起動時間の 1 ~ 3 秒間を切ることができることを示しています。</span><span class="sxs-lookup"><span data-stu-id="98fc1-111">Empirical observations show that native images of the EF runtime assemblies can cut between 1 and 3 seconds of application startup time.</span></span>  

## <a name="how-to-use-ngenexe"></a><span data-ttu-id="98fc1-112">NGen.exe の使用方法</span><span class="sxs-lookup"><span data-stu-id="98fc1-112">How to use NGen.exe</span></span>  

<span data-ttu-id="98fc1-113">NGen.exe ツールの最も基本的な機能は、アセンブリとそのすべての直接の依存関係のネイティブイメージを "インストール" (つまり、ディスクに保存) することです。</span><span class="sxs-lookup"><span data-stu-id="98fc1-113">The most basic function of the NGen.exe tool is to “install” (that is, to create and persist to disk) native images for an assembly and all its direct dependencies.</span></span> <span data-ttu-id="98fc1-114">これを実現する方法を次に示します。</span><span class="sxs-lookup"><span data-stu-id="98fc1-114">Here is how you can achieve that:</span></span>  

1. <span data-ttu-id="98fc1-115">管理者としてコマンド プロンプト ウィンドウを開きます。</span><span class="sxs-lookup"><span data-stu-id="98fc1-115">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="98fc1-116">現在の作業ディレクトリを、ネイティブイメージを生成するアセンブリの場所に変更します。</span><span class="sxs-lookup"><span data-stu-id="98fc1-116">Change the current working directory to the location of the assemblies you want to generate native images for:</span></span>

   ``` console
   cd <*Assemblies location*>  
   ```

3. <span data-ttu-id="98fc1-117">オペレーティングシステムとアプリケーションの構成によっては、32ビットアーキテクチャ、64ビットアーキテクチャ、またはその両方のネイティブイメージを生成することが必要になる場合があります。</span><span class="sxs-lookup"><span data-stu-id="98fc1-117">Depending on your operating system and the application’s configuration you might need to generate native images for 32 bit architecture, 64 bit architecture or for both.</span></span>

   <span data-ttu-id="98fc1-118">32ビットの場合は、次のように実行します。</span><span class="sxs-lookup"><span data-stu-id="98fc1-118">For 32 bit run:</span></span>

   ``` console
   %WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install <Assembly name>  
   ```

   <span data-ttu-id="98fc1-119">64ビットの場合は、次のように実行します。</span><span class="sxs-lookup"><span data-stu-id="98fc1-119">For 64 bit run:</span></span>
  
   ``` console
   %WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install <Assembly name>  
   ```

> [!TIP]
> <span data-ttu-id="98fc1-120">間違ったアーキテクチャのネイティブイメージを生成するのは非常に一般的な間違いです。</span><span class="sxs-lookup"><span data-stu-id="98fc1-120">Generating native images for the wrong architecture is a very common mistake.</span></span> <span data-ttu-id="98fc1-121">確信が持てない場合は、コンピューターにインストールされているオペレーティングシステムに適用されるすべてのアーキテクチャのネイティブイメージを簡単に生成できます。</span><span class="sxs-lookup"><span data-stu-id="98fc1-121">When in doubt you can simply generate native images for all the architectures that apply to the operating system installed in the machine.</span></span>  

<span data-ttu-id="98fc1-122">また NGen.exe は、インストールされているネイティブイメージのアンインストールと表示、複数のイメージの生成のキューなどの他の機能もサポートします。使用法の詳細については、 [NGen.exe のドキュメント](https://msdn.microsoft.com/library/6t9t5wcf.aspx)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="98fc1-122">NGen.exe also supports other functions such as uninstalling and displaying the installed native images, queuing the generation of multiple images, etc. For more details of usage read the [NGen.exe documentation](https://msdn.microsoft.com/library/6t9t5wcf.aspx).</span></span>  

## <a name="when-to-use-ngenexe"></a><span data-ttu-id="98fc1-123">NGen.exe を使用する場合</span><span class="sxs-lookup"><span data-stu-id="98fc1-123">When to use NGen.exe</span></span>  

<span data-ttu-id="98fc1-124">EF バージョン6以降に基づくアプリケーションでネイティブイメージを生成するアセンブリを決定する際には、次のオプションを考慮する必要があります。</span><span class="sxs-lookup"><span data-stu-id="98fc1-124">When it comes to decide which assemblies to generate native images for in an application based on EF version 6 or greater, you should consider the following options:</span></span>  

- <span data-ttu-id="98fc1-125">**主 ef ランタイムアセンブリ EntityFramework.dll**: 一般的な ef ベースのアプリケーションは、起動時またはデータベースへの最初のアクセス時にこのアセンブリから大量のコードを実行します。</span><span class="sxs-lookup"><span data-stu-id="98fc1-125">**The main EF runtime assembly, EntityFramework.dll**: A typical EF based application executes a significant amount of code from this assembly on startup or on its first access to the database.</span></span> <span data-ttu-id="98fc1-126">そのため、このアセンブリのネイティブイメージを作成すると、起動時のパフォーマンスが最大になります。</span><span class="sxs-lookup"><span data-stu-id="98fc1-126">Consequently, creating native images of this assembly will produce the biggest gains in startup performance.</span></span>  
- <span data-ttu-id="98fc1-127">**アプリケーションによって使用される EF プロバイダーアセンブリ**: 起動時間は、これらのネイティブイメージを生成する場合と若干同じ効果があります。</span><span class="sxs-lookup"><span data-stu-id="98fc1-127">**Any EF provider assembly used by your application**: Startup time can also benefit slightly from generating native images of these.</span></span> <span data-ttu-id="98fc1-128">たとえば、アプリケーションで SQL Server に EF プロバイダーを使用する場合は、EntityFramework.SqlServer.dll のネイティブイメージを生成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="98fc1-128">For example, if the application uses the EF provider for SQL Server you will want to generate a native image for EntityFramework.SqlServer.dll.</span></span>  
- <span data-ttu-id="98fc1-129">**アプリケーションのアセンブリとその他の依存関係**: [NGen.exe のドキュメント](https://msdn.microsoft.com/library/6t9t5wcf.aspx) では、ネイティブイメージを生成するアセンブリを選択するための一般的な条件と、セキュリティ上のネイティブイメージの影響、"ハードバインディング" などの高度なオプション、デバッグとプロファイルのシナリオでのネイティブイメージの使用などのシナリオなどについて説明します。</span><span class="sxs-lookup"><span data-stu-id="98fc1-129">**Your application’s assemblies and other dependencies**: The [NGen.exe documentation](https://msdn.microsoft.com/library/6t9t5wcf.aspx) covers general criteria for choosing which assemblies to generate native images for and the impact of native images on security, advanced options such as “hard binding”, scenarios such as using native images in debugging and profiling scenarios, etc.</span></span>  

> [!TIP]
> <span data-ttu-id="98fc1-130">起動時のパフォーマンスとアプリケーションの全体的なパフォーマンスの両方でネイティブイメージを使用した場合の影響を慎重に測定し、実際の要件と比較してください。</span><span class="sxs-lookup"><span data-stu-id="98fc1-130">Make sure you carefully measure the impact of using native images on both the startup performance and the overall performance of your application and compare them against actual requirements.</span></span> <span data-ttu-id="98fc1-131">一般にネイティブイメージは起動時のパフォーマンスの向上に役立ちますが、場合によってはメモリ使用量が少なくなることがありますが、一部のシナリオでは同等のメリットが得られません。</span><span class="sxs-lookup"><span data-stu-id="98fc1-131">While native images will generally help improve startup up performance and in some cases reduce memory usage, not all scenarios will benefit equally.</span></span> <span data-ttu-id="98fc1-132">たとえば、安定した状態で実行した場合 (つまり、アプリケーションによって使用されているすべてのメソッドが少なくとも1回呼び出された場合)、JIT コンパイラによって生成されるコードはネイティブイメージよりも若干優れたパフォーマンスをもたらします。</span><span class="sxs-lookup"><span data-stu-id="98fc1-132">For instance, on steady state execution (that is, once all the methods being used by the application have been invoked at least once) code generated by the JIT compiler can in fact yield slightly better performance than native images.</span></span>  

## <a name="using-ngenexe-in-a-development-machine"></a><span data-ttu-id="98fc1-133">開発用コンピューターでの NGen.exe の使用</span><span class="sxs-lookup"><span data-stu-id="98fc1-133">Using NGen.exe in a development machine</span></span>  

<span data-ttu-id="98fc1-134">開発中は、.NET JIT コンパイラによって、頻繁に変更されるコードに対する全体的なトレードオフが最適になります。</span><span class="sxs-lookup"><span data-stu-id="98fc1-134">During development the .NET JIT compiler will offer the best overall tradeoff for code that is changing frequently.</span></span> <span data-ttu-id="98fc1-135">EF runtime アセンブリなど、コンパイルされた依存関係のネイティブイメージを生成すると、各実行の開始時に数秒を減らすことで、開発とテストの時間を短縮できます。</span><span class="sxs-lookup"><span data-stu-id="98fc1-135">Generating native images for compiled dependencies such as the EF runtime assemblies can help accelerate development and testing by cutting a few seconds out at the beginning of each execution.</span></span>  

<span data-ttu-id="98fc1-136">EF ランタイムアセンブリを見つけるための最適な場所は、ソリューションの NuGet パッケージの場所です。</span><span class="sxs-lookup"><span data-stu-id="98fc1-136">A good place to find the EF runtime assemblies is the NuGet package location for the solution.</span></span> <span data-ttu-id="98fc1-137">たとえば、SQL Server で EF 6.0.2 を使用し、.NET 4.5 以降を対象とするアプリケーションの場合は、コマンドプロンプトウィンドウで次のように入力できます (管理者として開くことを忘れないでください)。</span><span class="sxs-lookup"><span data-stu-id="98fc1-137">For example, for an application using EF 6.0.2 with SQL Server and targeting .NET 4.5 or greater you can type the following in a Command Prompt window (remember to open it as an administrator):</span></span>  

```console
cd <Solution directory>\packages\EntityFramework.6.0.2\lib\net45
%WINDIR%\Microsoft.NET\Framework\v4.0.30319\ngen install EntityFramework.SqlServer.dll
%WINDIR%\Microsoft.NET\Framework64\v4.0.30319\ngen install EntityFramework.SqlServer.dll
```  

> [!NOTE]
> <span data-ttu-id="98fc1-138">これにより、SQL Server 用の EF プロバイダーのネイティブイメージをインストールするときに、メインの EF runtime アセンブリのネイティブイメージも既定でインストールされるという事実を活用できます。</span><span class="sxs-lookup"><span data-stu-id="98fc1-138">This takes advantage of the fact that installing the native images for EF provider for SQL Server will also by default install the native images for the main EF runtime assembly.</span></span> <span data-ttu-id="98fc1-139">これは、EntityFramework.dll が同じディレクトリにある EntityFramework.SqlServer.dll アセンブリの直接の依存関係であることを NGen.exe が検出できるために機能します。</span><span class="sxs-lookup"><span data-stu-id="98fc1-139">This works because NGen.exe can detect that EntityFramework.dll is a direct dependency of the EntityFramework.SqlServer.dll assembly located in the same directory.</span></span>  

## <a name="creating-native-images-during-setup"></a><span data-ttu-id="98fc1-140">セットアップ時のネイティブイメージの作成</span><span class="sxs-lookup"><span data-stu-id="98fc1-140">Creating native images during setup</span></span>  

<span data-ttu-id="98fc1-141">WiX Toolkit は、この [ハウツーガイド](https://wixtoolset.org/documentation/manual/v3/howtos/files_and_registry/ngen_managed_assemblies.html)で説明されているように、セットアップ時にマネージアセンブリのネイティブイメージの生成をキューによってサポートします。</span><span class="sxs-lookup"><span data-stu-id="98fc1-141">The WiX Toolkit supports queuing the generation of native images for managed assemblies during setup, as explained in this [how-to guide](https://wixtoolset.org/documentation/manual/v3/howtos/files_and_registry/ngen_managed_assemblies.html).</span></span> <span data-ttu-id="98fc1-142">別の方法として、NGen.exe コマンドを実行するカスタムセットアップタスクを作成することもできます。</span><span class="sxs-lookup"><span data-stu-id="98fc1-142">Another alternative is to create a custom setup task that execute the NGen.exe command.</span></span>  

## <a name="verifying-that-native-images-are-being-used-for-ef"></a><span data-ttu-id="98fc1-143">EF でネイティブイメージが使用されていることを確認しています</span><span class="sxs-lookup"><span data-stu-id="98fc1-143">Verifying that native images are being used for EF</span></span>  

<span data-ttu-id="98fc1-144">特定のアプリケーションがネイティブアセンブリを使用していることを確認するには、拡張子が ".ni.dll" または ".ni.exe" の読み込み済みアセンブリを探します。</span><span class="sxs-lookup"><span data-stu-id="98fc1-144">You can verify that a specific application is using a native assembly by looking for loaded assemblies that have the extension “.ni.dll” or “.ni.exe”.</span></span> <span data-ttu-id="98fc1-145">たとえば、EF のメインランタイムアセンブリのネイティブイメージは EntityFramework.ni.dll として呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="98fc1-145">For example, a native image for the EF’s main runtime assembly will be called EntityFramework.ni.dll.</span></span> <span data-ttu-id="98fc1-146">プロセスの .NET アセンブリを確認する簡単な方法は、 [プロセスエクスプローラー](https://technet.microsoft.com/sysinternals/bb896653)を使用することです。</span><span class="sxs-lookup"><span data-stu-id="98fc1-146">An easy way to inspect the loaded .NET assemblies of a process is to use [Process Explorer](https://technet.microsoft.com/sysinternals/bb896653).</span></span>  

## <a name="other-things-to-be-aware-of"></a><span data-ttu-id="98fc1-147">その他の注意点</span><span class="sxs-lookup"><span data-stu-id="98fc1-147">Other things to be aware of</span></span>  

<span data-ttu-id="98fc1-148">**アセンブリのネイティブイメージの作成は、アセンブリを[GAC (グローバルアセンブリキャッシュ)](https://msdn.microsoft.com/library/yf1d93sz.aspx)に登録する場合と混同しないようにしてください**。</span><span class="sxs-lookup"><span data-stu-id="98fc1-148">**Creating a native image of an assembly should not be confused with registering the assembly in the [GAC (Global Assembly Cache)](https://msdn.microsoft.com/library/yf1d93sz.aspx)**.</span></span> <span data-ttu-id="98fc1-149">NGen.exe では、GAC にないアセンブリのイメージを作成できます。実際には、特定のバージョンの EF を使用する複数のアプリケーションが同じネイティブイメージを共有できます。</span><span class="sxs-lookup"><span data-stu-id="98fc1-149">NGen.exe allows creating images of assemblies that are not in the GAC, and in fact, multiple applications that use a specific version of EF can share the same native image.</span></span> <span data-ttu-id="98fc1-150">Windows 8 では、GAC に配置されたアセンブリのネイティブイメージを自動的に作成できますが、EF ランタイムはアプリケーションと共に配置するように最適化されています。また、アセンブリの解決に悪影響を及ぼし、アプリケーションを他の側面でサービスするため、GAC に登録することはお勧めしません。</span><span class="sxs-lookup"><span data-stu-id="98fc1-150">While Windows 8 can automatically create native images for assemblies placed in the GAC, the EF runtime is optimized to be deployed alongside your application and we do not recommend registering it in the GAC as this has a negative impact on assembly resolution and servicing your applications among other aspects.</span></span>  
