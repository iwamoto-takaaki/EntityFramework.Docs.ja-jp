---
title: EF Core のリリースの計画
description: Entity Framework Core の計画とリリースの方法に関する情報
author: ajcvickers
ms.date: 01/28/2020
uid: core/what-is-new/release-planning
ms.openlocfilehash: f84b8cef40a74245575df6013d94fcda5738e229
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94429144"
---
# <a name="release-planning-process"></a><span data-ttu-id="e0d34-103">リリースの計画プロセス</span><span class="sxs-lookup"><span data-stu-id="e0d34-103">Release planning process</span></span>

<span data-ttu-id="e0d34-104">特定のリリースに含める特定の機能をどのように選ぶかについて、よく質問を受けます。</span><span class="sxs-lookup"><span data-stu-id="e0d34-104">We often get questions about how we choose specific features to go into a particular release.</span></span>
<span data-ttu-id="e0d34-105">このドキュメントでは、使用するプロセスについて説明します。</span><span class="sxs-lookup"><span data-stu-id="e0d34-105">This document outlines the process we use.</span></span>
<span data-ttu-id="e0d34-106">プロセスは、計画するためのより優れた方法が見つかるたびに進化し続けますが、一般的な考え方は変わりません。</span><span class="sxs-lookup"><span data-stu-id="e0d34-106">The process is continually evolving as we find better ways to plan, but the general ideas remain the same.</span></span>

## <a name="different-kinds-of-releases"></a><span data-ttu-id="e0d34-107">さまざまな種類のリリース</span><span class="sxs-lookup"><span data-stu-id="e0d34-107">Different kinds of releases</span></span>

<span data-ttu-id="e0d34-108">リリースの種類によって、さまざまな種類の変更が含まれています。</span><span class="sxs-lookup"><span data-stu-id="e0d34-108">Different kinds of release contain different kinds of changes.</span></span>
<span data-ttu-id="e0d34-109">これは、リリースの計画がリリースの種類によって異なることを意味します。</span><span class="sxs-lookup"><span data-stu-id="e0d34-109">This in turn means that the release planning is different for different kinds of release.</span></span>

### <a name="patch-releases"></a><span data-ttu-id="e0d34-110">パッチ リリース</span><span class="sxs-lookup"><span data-stu-id="e0d34-110">Patch releases</span></span>

<span data-ttu-id="e0d34-111">パッチ リリースでは、バージョンの "パッチ" 部分のみが変更されます。</span><span class="sxs-lookup"><span data-stu-id="e0d34-111">Patch releases change only the "patch" part of the version.</span></span>
<span data-ttu-id="e0d34-112">たとえば、EF Core 3.1. **1** は、EF Core 3.1. **0** で見つかった問題を修正するためのリリースです。</span><span class="sxs-lookup"><span data-stu-id="e0d34-112">For example, EF Core 3.1. **1** is a release that patches issues found in EF Core 3.1. **0**.</span></span>

<span data-ttu-id="e0d34-113">パッチ リリースは、重大な顧客のバグを修正することを目的としています。</span><span class="sxs-lookup"><span data-stu-id="e0d34-113">Patch releases are intended to fix critical customer bugs.</span></span>
<span data-ttu-id="e0d34-114">つまり、パッチ リリースには、新機能は含まれていません。</span><span class="sxs-lookup"><span data-stu-id="e0d34-114">This means patch releases do not include new features.</span></span>
<span data-ttu-id="e0d34-115">特別な状況を除き、パッチ リリースでは API の変更は許可されていません。</span><span class="sxs-lookup"><span data-stu-id="e0d34-115">API changes are not allowed in patch releases except in special circumstances.</span></span>

<span data-ttu-id="e0d34-116">パッチ リリースで変更を実施するためのハードルは非常に高くなっています。</span><span class="sxs-lookup"><span data-stu-id="e0d34-116">The bar to make a change in a patch release is very high.</span></span>
<span data-ttu-id="e0d34-117">これは、パッチ リリースによって新しいバグを発生させないことが重要であるためです。</span><span class="sxs-lookup"><span data-stu-id="e0d34-117">This is because it is critical that patch releases do not introduce new bugs.</span></span>
<span data-ttu-id="e0d34-118">そのため、決定プロセスにおいては、高価値および低リスクが重要視されます。</span><span class="sxs-lookup"><span data-stu-id="e0d34-118">Therefore, the decision process emphasizes high value and low risk.</span></span>

<span data-ttu-id="e0d34-119">次の場合、問題が修正される可能性が高くなります。</span><span class="sxs-lookup"><span data-stu-id="e0d34-119">We are more likely to patch an issue if:</span></span>

* <span data-ttu-id="e0d34-120">複数の顧客に影響がある</span><span class="sxs-lookup"><span data-stu-id="e0d34-120">It is impacting multiple customers</span></span>
* <span data-ttu-id="e0d34-121">以前のリリースからの再発である</span><span class="sxs-lookup"><span data-stu-id="e0d34-121">It is a regression from a previous release</span></span>
* <span data-ttu-id="e0d34-122">問題が原因でデータが破損する</span><span class="sxs-lookup"><span data-stu-id="e0d34-122">The failure causes data corruption</span></span>

<span data-ttu-id="e0d34-123">次の場合、問題が修正される可能性は低くなります。</span><span class="sxs-lookup"><span data-stu-id="e0d34-123">We are less likely to patch an issue if:</span></span>

* <span data-ttu-id="e0d34-124">適切な回避策がある</span><span class="sxs-lookup"><span data-stu-id="e0d34-124">There are reasonable workarounds</span></span>
* <span data-ttu-id="e0d34-125">修正によって他の問題が発生する危険性が高い</span><span class="sxs-lookup"><span data-stu-id="e0d34-125">The fix has high risk of breaking something else</span></span>
* <span data-ttu-id="e0d34-126">バグが奥深い場所にある</span><span class="sxs-lookup"><span data-stu-id="e0d34-126">The bug is in a corner-case</span></span>

<span data-ttu-id="e0d34-127">このハードルは、[長期サポート (LTS)](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) リリースの有効期間において徐々に上がっていきます。</span><span class="sxs-lookup"><span data-stu-id="e0d34-127">This bar gradually rises through the lifetime of a [long-term support (LTS)](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) release.</span></span> <span data-ttu-id="e0d34-128">これは、LTS リリースでは安定性が重視されるためです。</span><span class="sxs-lookup"><span data-stu-id="e0d34-128">This is because LTS releases emphasize stability.</span></span>

<span data-ttu-id="e0d34-129">問題を修正するかどうかについての最終的な決定は、Microsoft の .NET ディレクターによって行われます。</span><span class="sxs-lookup"><span data-stu-id="e0d34-129">The final decision about whether or not to patch an issue is made by the .NET Directors at Microsoft.</span></span>

### <a name="minor-releases"></a><span data-ttu-id="e0d34-130">マイナー リリース</span><span class="sxs-lookup"><span data-stu-id="e0d34-130">Minor releases</span></span>

<span data-ttu-id="e0d34-131">マイナー リリースでは、バージョンの "マイナー" 部分のみが変更されます。</span><span class="sxs-lookup"><span data-stu-id="e0d34-131">Minor releases change only the "minor" part of the version.</span></span>
<span data-ttu-id="e0d34-132">たとえば、EF Core 3. **1**.0 は、EF Core 3. **0**.0 に対する改良を行うためのリリースです。</span><span class="sxs-lookup"><span data-stu-id="e0d34-132">For example, EF Core 3. **1**.0 is a release that improves on EF Core 3. **0**.0.</span></span>

<span data-ttu-id="e0d34-133">マイナー リリース:</span><span class="sxs-lookup"><span data-stu-id="e0d34-133">Minor releases:</span></span>

* <span data-ttu-id="e0d34-134">以前のリリースの品質と機能を改良することを目的としています</span><span class="sxs-lookup"><span data-stu-id="e0d34-134">Are intended to improve the quality and features of the previous release</span></span>
* <span data-ttu-id="e0d34-135">通常、バグの修正と新機能が含まれています</span><span class="sxs-lookup"><span data-stu-id="e0d34-135">Typically contain bug fixes and new features</span></span>
* <span data-ttu-id="e0d34-136">重大な変更は意図的には含まれません</span><span class="sxs-lookup"><span data-stu-id="e0d34-136">Do not include intentional breaking changes</span></span>
* <span data-ttu-id="e0d34-137">いくつかのプレリリース レビューが NuGet にプッシュされています</span><span class="sxs-lookup"><span data-stu-id="e0d34-137">Have a few prerelease previews pushed to NuGet</span></span>

### <a name="major-releases"></a><span data-ttu-id="e0d34-138">メジャー リリース</span><span class="sxs-lookup"><span data-stu-id="e0d34-138">Major releases</span></span>

<span data-ttu-id="e0d34-139">メジャー リリースでは、EF の "メジャー" バージョン番号が変更されます。</span><span class="sxs-lookup"><span data-stu-id="e0d34-139">Major releases change the EF "major" version number.</span></span>
<span data-ttu-id="e0d34-140">たとえば、EF Core **3**.0.0 は、EF Core 2.2.x を大きく前進させたメジャー リリースです。</span><span class="sxs-lookup"><span data-stu-id="e0d34-140">For example, EF Core **3**.0.0 is a major release that makes a big step forward over EF Core 2.2.x.</span></span>

<span data-ttu-id="e0d34-141">メジャー リリース:</span><span class="sxs-lookup"><span data-stu-id="e0d34-141">Major releases:</span></span>

* <span data-ttu-id="e0d34-142">以前のリリースの品質と機能を改良することを目的としています</span><span class="sxs-lookup"><span data-stu-id="e0d34-142">Are intended to improve the quality and features of the previous release</span></span>
* <span data-ttu-id="e0d34-143">通常、バグの修正と新機能が含まれています</span><span class="sxs-lookup"><span data-stu-id="e0d34-143">Typically contain bug fixes and new features</span></span>
  * <span data-ttu-id="e0d34-144">EF Core の動作方法に対する根本的な変更となる新機能がある場合もあります</span><span class="sxs-lookup"><span data-stu-id="e0d34-144">Some of the new features may be fundamental changes to the way EF Core works</span></span>
* <span data-ttu-id="e0d34-145">通常、重大な変更が意図的に含まれています</span><span class="sxs-lookup"><span data-stu-id="e0d34-145">Typically include intentional breaking changes</span></span>
  * <span data-ttu-id="e0d34-146">重大な変更は、理解が進むとともに EF Core を進化させるために必要なものです</span><span class="sxs-lookup"><span data-stu-id="e0d34-146">Breaking changes are necessary part of evolving EF Core as we learn</span></span>
  * <span data-ttu-id="e0d34-147">ただし、顧客に影響を与える可能性があるため、重大な変更については慎重に検討を行います。</span><span class="sxs-lookup"><span data-stu-id="e0d34-147">However, we think very carefully about making any breaking change because of the potential customer impact.</span></span> <span data-ttu-id="e0d34-148">過去には、重大な変更をかなり積極的に行った部分もあるかもしれません。</span><span class="sxs-lookup"><span data-stu-id="e0d34-148">We may have been too aggressive with breaking changes in the past.</span></span> <span data-ttu-id="e0d34-149">今後は、アプリケーションを中断する変更を最小限に抑え、データベース プロバイダーや拡張機能を損なう変更を減らすことに努めています。</span><span class="sxs-lookup"><span data-stu-id="e0d34-149">Going forward, we will strive to minimize changes that break applications, and to reduce changes that break database providers and extensions.</span></span>
* <span data-ttu-id="e0d34-150">多くのプレリリース レビューが NuGet にプッシュされています</span><span class="sxs-lookup"><span data-stu-id="e0d34-150">Have many prerelease previews pushed to NuGet</span></span>

## <a name="planning-for-majorminor-releases"></a><span data-ttu-id="e0d34-151">メジャーまたはマイナー リリースの計画</span><span class="sxs-lookup"><span data-stu-id="e0d34-151">Planning for major/minor releases</span></span>

### <a name="github-issue-tracking"></a><span data-ttu-id="e0d34-152">GitHub 問題追跡</span><span class="sxs-lookup"><span data-stu-id="e0d34-152">GitHub issue tracking</span></span>

<span data-ttu-id="e0d34-153">GitHub ([https://github.com/dotnet/efcore](https://github.com/dotnet/efcore)) は、すべての EF Core 計画の信頼できる情報源です。</span><span class="sxs-lookup"><span data-stu-id="e0d34-153">GitHub ([https://github.com/dotnet/efcore](https://github.com/dotnet/efcore)) is the source-of-truth for all EF Core planning.</span></span>

<span data-ttu-id="e0d34-154">GitHub 上の問題には、次の情報があります。</span><span class="sxs-lookup"><span data-stu-id="e0d34-154">Issues on GitHub have:</span></span>

* <span data-ttu-id="e0d34-155">状態</span><span class="sxs-lookup"><span data-stu-id="e0d34-155">A state</span></span>
  * <span data-ttu-id="e0d34-156">[Open](https://github.com/dotnet/efcore/issues) は、未対応の問題です。</span><span class="sxs-lookup"><span data-stu-id="e0d34-156">[Open](https://github.com/dotnet/efcore/issues) issues have not been addressed.</span></span>
  * <span data-ttu-id="e0d34-157">[Closed](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aclosed) は、対応された問題です。</span><span class="sxs-lookup"><span data-stu-id="e0d34-157">[Closed](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aclosed) issues have been addressed.</span></span>
    * <span data-ttu-id="e0d34-158">修正された問題にはすべて、[closed-fixed のタグが付けられます](https://github.com/dotnet/efcore/issues?q=is%3Aissue+label%3Aclosed-fixed+is%3Aclosed)。</span><span class="sxs-lookup"><span data-stu-id="e0d34-158">All issues that have been fixed are [tagged with closed-fixed](https://github.com/dotnet/efcore/issues?q=is%3Aissue+label%3Aclosed-fixed+is%3Aclosed).</span></span> <span data-ttu-id="e0d34-159">closed-fixed のタグが付けられた問題は、修正されマージされていますが、リリースされていない場合があります。</span><span class="sxs-lookup"><span data-stu-id="e0d34-159">An issue tagged with closed-fixed is fixed and merged, but may not have been released.</span></span>
    * <span data-ttu-id="e0d34-160">その他の `closed-` ラベルは、問題を終了する他の理由を示すものです。</span><span class="sxs-lookup"><span data-stu-id="e0d34-160">Other `closed-` labels indicate other reasons for closing an issue.</span></span> <span data-ttu-id="e0d34-161">たとえば、重複するものには closed-duplicate というタグが付けられています。</span><span class="sxs-lookup"><span data-stu-id="e0d34-161">For example, duplicates are tagged with closed-duplicate.</span></span>
* <span data-ttu-id="e0d34-162">種類</span><span class="sxs-lookup"><span data-stu-id="e0d34-162">A type</span></span>
  * <span data-ttu-id="e0d34-163">[Bugs](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-bug) は、バグを表します。</span><span class="sxs-lookup"><span data-stu-id="e0d34-163">[Bugs](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-bug) represent bugs.</span></span>
  * <span data-ttu-id="e0d34-164">[Enhancements](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-enhancement) は、新機能または既存の機能の改良された機能を表します。</span><span class="sxs-lookup"><span data-stu-id="e0d34-164">[Enhancements](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Atype-enhancement) represent new features or better functionality in existing features.</span></span>
* <span data-ttu-id="e0d34-165">マイルストーン</span><span class="sxs-lookup"><span data-stu-id="e0d34-165">A milestone</span></span>
  * <span data-ttu-id="e0d34-166">[マイルストーンがない問題](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+no%3Amilestone)は、チームによる検討中です。</span><span class="sxs-lookup"><span data-stu-id="e0d34-166">[Issues with no milestone](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+no%3Amilestone) are being considered by the team.</span></span> <span data-ttu-id="e0d34-167">この問題の対応に関する決定がまだ行われていないか、決定の変更が検討されています。</span><span class="sxs-lookup"><span data-stu-id="e0d34-167">The decision on what to do with the issue has not yet been made or a change in the decision is being considered.</span></span>
  * <span data-ttu-id="e0d34-168">[Backlog マイルストーン内の問題](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog)は、EF チームが将来のリリースで取り組むことを検討する予定の項目です</span><span class="sxs-lookup"><span data-stu-id="e0d34-168">[Issues in the Backlog milestone](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3ABacklog) are items that the EF team will consider working on in a future release</span></span>
    * <span data-ttu-id="e0d34-169">バックログ内の問題には、この作業項目が次のリリースのリスト内で上の位置にあることを示す、[consider-for-next-release というタグが付けられています](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Aconsider-for-next-release)。</span><span class="sxs-lookup"><span data-stu-id="e0d34-169">Issues in the backlog may be [tagged with consider-for-next-release](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+label%3Aconsider-for-next-release) indicating that this work item is high on the list for the next release.</span></span>
  * <span data-ttu-id="e0d34-170">バージョン番号が付けられたマイルストーン内の未対応問題は、チームがそのバージョンで取り組む予定の項目です。</span><span class="sxs-lookup"><span data-stu-id="e0d34-170">Open issues in a versioned milestone are items that the team plans to work on in that version.</span></span> <span data-ttu-id="e0d34-171">たとえば、[これらは EF Core 5.0 で取り組む予定の問題です](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A5.0.0)。</span><span class="sxs-lookup"><span data-stu-id="e0d34-171">For example, [these are the issues we plan to work on for EF Core 5.0](https://github.com/dotnet/efcore/issues?q=is%3Aopen+is%3Aissue+milestone%3A5.0.0).</span></span>
  * <span data-ttu-id="e0d34-172">バージョン番号が付けられたマイルストーン内の対応済みの問題は、そのバージョンで完了した問題です。</span><span class="sxs-lookup"><span data-stu-id="e0d34-172">Closed issues in a versioned milestone are issues that are completed for that version.</span></span> <span data-ttu-id="e0d34-173">バージョンがまだリリースされていない可能性があることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="e0d34-173">Note that the version may not yet have been released.</span></span> <span data-ttu-id="e0d34-174">たとえば、[これらは EF Core 3.0 で完了した問題です](https://github.com/dotnet/efcore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed)。</span><span class="sxs-lookup"><span data-stu-id="e0d34-174">For example, [these are the issues completed for EF Core 3.0](https://github.com/dotnet/efcore/issues?q=is%3Aissue+milestone%3A3.0.0+is%3Aclosed).</span></span>
* <span data-ttu-id="e0d34-175">投票</span><span class="sxs-lookup"><span data-stu-id="e0d34-175">Votes!</span></span>
  * <span data-ttu-id="e0d34-176">投票は、ある問題が自分にとって重要であるということを示す、最善の方法です。</span><span class="sxs-lookup"><span data-stu-id="e0d34-176">Voting is the best way to indicate that an issue is important to you.</span></span>
  * <span data-ttu-id="e0d34-177">投票するには、その問題に対して "上向きの親指" 👍を追加するだけです。</span><span class="sxs-lookup"><span data-stu-id="e0d34-177">To vote, just add a "thumbs-up" 👍 to the issue.</span></span> <span data-ttu-id="e0d34-178">たとえば、[これらが投票数の多い問題です](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc)</span><span class="sxs-lookup"><span data-stu-id="e0d34-178">For example, [these are the top-voted issues](https://github.com/dotnet/efcore/issues?q=is%3Aissue+is%3Aopen+sort%3Areactions-%2B1-desc)</span></span>
  * <span data-ttu-id="e0d34-179">また、そうすることで効果が生まれると思われる場合は、機能を必要とする具体的な理由についてコメントしてください。</span><span class="sxs-lookup"><span data-stu-id="e0d34-179">Please also comment describing specific reasons you need the feature if you feel this adds value.</span></span> <span data-ttu-id="e0d34-180">「+ 1」などのコメントを付けても、効果は生まれません。</span><span class="sxs-lookup"><span data-stu-id="e0d34-180">Commenting "+1" or similar does not add value.</span></span>

### <a name="the-planning-process"></a><span data-ttu-id="e0d34-181">計画プロセス</span><span class="sxs-lookup"><span data-stu-id="e0d34-181">The planning process</span></span>

<span data-ttu-id="e0d34-182">計画プロセスには、最もリクエストが多い機能をバックログから取得する以上のことが含まれます。</span><span class="sxs-lookup"><span data-stu-id="e0d34-182">The planning process is more involved than just taking the top-most requested features from the backlog.</span></span>
<span data-ttu-id="e0d34-183">これは、複数の関係者からのフィードバックを複数の方法で収集するためです。</span><span class="sxs-lookup"><span data-stu-id="e0d34-183">This is because we gather feedback from multiple stakeholders in multiple ways.</span></span>
<span data-ttu-id="e0d34-184">その後、次に基づいてリリースを形づくります。</span><span class="sxs-lookup"><span data-stu-id="e0d34-184">We then shape a release based on:</span></span>

* <span data-ttu-id="e0d34-185">顧客からのインプット</span><span class="sxs-lookup"><span data-stu-id="e0d34-185">Input from customers</span></span>
* <span data-ttu-id="e0d34-186">他の関係者からのインプット</span><span class="sxs-lookup"><span data-stu-id="e0d34-186">Input from other stakeholders</span></span>
* <span data-ttu-id="e0d34-187">戦略上の方向性</span><span class="sxs-lookup"><span data-stu-id="e0d34-187">Strategic direction</span></span>
* <span data-ttu-id="e0d34-188">利用可能なリソース</span><span class="sxs-lookup"><span data-stu-id="e0d34-188">Resources available</span></span>
* <span data-ttu-id="e0d34-189">スケジュール</span><span class="sxs-lookup"><span data-stu-id="e0d34-189">Schedule</span></span>

<span data-ttu-id="e0d34-190">次のような質問を検討します。</span><span class="sxs-lookup"><span data-stu-id="e0d34-190">Some of the questions we ask are:</span></span>

1. <span data-ttu-id="e0d34-191">**この機能を使用する開発者の数は? アプリケーション/エクスペリエンスをどの程度向上させますか?**</span><span class="sxs-lookup"><span data-stu-id="e0d34-191">**How many developers we think will use the feature and how much better will it make their applications or experience?**</span></span> <span data-ttu-id="e0d34-192">この質問に答えるために、多くのソースからフィードバックを収集します。問題についてのコメントと投票は、これらのソースの 1 つです。</span><span class="sxs-lookup"><span data-stu-id="e0d34-192">To answer this question, we collect feedback from many sources — Comments and votes on issues is one of those sources.</span></span> <span data-ttu-id="e0d34-193">重要な顧客との特定の契約は別のものです。</span><span class="sxs-lookup"><span data-stu-id="e0d34-193">Specific engagements with important customers is another.</span></span>

2. <span data-ttu-id="e0d34-194">**この機能をまだ実装していない場合、ユーザーが使用できる回避策とは?**</span><span class="sxs-lookup"><span data-stu-id="e0d34-194">**What are the workarounds people can use if we don't implement this feature yet?**</span></span> <span data-ttu-id="e0d34-195">たとえば、多対多のネイティブ サポートがないことを回避するために、多くの開発者は統合テーブルをマップすることができます。</span><span class="sxs-lookup"><span data-stu-id="e0d34-195">For example, many developers can map a join table to work around lack of native many-to-many support.</span></span> <span data-ttu-id="e0d34-196">当然ながら、すべての開発者がこれを実行するわけではありませんが、その多くは実行できます。そしてこれは決定の要因としてカウントされます。</span><span class="sxs-lookup"><span data-stu-id="e0d34-196">Obviously, not all developers want to do it, but many can, and that counts as a factor in our decision.</span></span>

3. <span data-ttu-id="e0d34-197">**他の機能を実装させるようにするなど、この機能の実装によって EF Core のアーキテクチャは進化しますか?**</span><span class="sxs-lookup"><span data-stu-id="e0d34-197">**Does implementing this feature evolve the architecture of EF Core such that it moves us closer to implementing other features?**</span></span> <span data-ttu-id="e0d34-198">他の機能の構成要素として動作する機能は優先される傾向があります。</span><span class="sxs-lookup"><span data-stu-id="e0d34-198">We tend to favor features that act as building blocks for other features.</span></span> <span data-ttu-id="e0d34-199">たとえば、プロパティ バッグ エンティティによって多対多のサポートを進めることができ、エンティティ コンストラクターによって遅延読み込みのサポートが可能になりました。</span><span class="sxs-lookup"><span data-stu-id="e0d34-199">For instance, property bag entities can help us move towards many-to-many support, and entity constructors enabled our lazy loading support.</span></span>

4. <span data-ttu-id="e0d34-200">**この機能は拡張ポイントですか?**</span><span class="sxs-lookup"><span data-stu-id="e0d34-200">**Is the feature an extensibility point?**</span></span> <span data-ttu-id="e0d34-201">拡張ポイントは通常の機能よりも優先される傾向があります。なぜなら、開発者がこれらを使用して、それぞれの独自の動作をフックし、不足している機能を補うことができるためです。</span><span class="sxs-lookup"><span data-stu-id="e0d34-201">We tend to favor extensibility points over normal features because they enable developers to hook their own behaviors and compensate for any missing functionality.</span></span>

5. <span data-ttu-id="e0d34-202">**他の製品と組み合わせて使用するときの機能のシナジーとは何ですか?**</span><span class="sxs-lookup"><span data-stu-id="e0d34-202">**What is the synergy of the feature when used in combination with other products?**</span></span> <span data-ttu-id="e0d34-203">.NET Core、最新バージョンの Visual Studio、Microsoft Azure など、その他の製品と共に EF Core を使用するエクスペリエンスを可能にする、または大幅に向上させる機能は、優先される傾向があります。</span><span class="sxs-lookup"><span data-stu-id="e0d34-203">We favor features that enable or significantly improve the experience of using EF Core with other products, such as .NET Core, the latest version of Visual Studio, Microsoft Azure, and so on.</span></span>

6. <span data-ttu-id="e0d34-204">**機能に取り組むために利用できるユーザーのスキルは何ですか? これらのリソースを最大限に活用する方法はありますか?**</span><span class="sxs-lookup"><span data-stu-id="e0d34-204">**What are the skills of the people available to work on a feature, and how can we best leverage these resources?**</span></span> <span data-ttu-id="e0d34-205">EF チームの各メンバーおよびコミュニティの共同作成者は、異なる領域におけるさまざまなレベルの経験を持っているので、それに応じて計画を立てる必要があります。</span><span class="sxs-lookup"><span data-stu-id="e0d34-205">Each member of the EF team and our community contributors has different levels of experience in different areas, so we have to plan accordingly.</span></span> <span data-ttu-id="e0d34-206">GroupBy の変換や多対多など、特定の機能に取り組むために "全員の協力" が欲しくなる場合であっても、それは実用的ではありません。</span><span class="sxs-lookup"><span data-stu-id="e0d34-206">Even if we wanted to have "all hands on deck" to work on a specific feature like GroupBy translations, or many-to-many, that wouldn't be practical.</span></span>
