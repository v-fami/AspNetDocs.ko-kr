---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: 애니메이션 (C#) 중 작업 사용 안 함 | Microsoft Docs
author: wenz
description: ASP.NET AJAX Control Toolkit에서 애니메이션 컨트롤 컨트롤 뿐 이지만 컨트롤에 애니메이션을 추가 하는 전체 프레임 워크 아닙니다. 또한 작업을 지원 하는 중...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: 4315f06ea1599bacb93a23a3759610e19754cfba
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57044350"
---
<a name="disabling-actions-during-animation-c"></a><span data-ttu-id="40de2-104">애니메이션 중에 작업 사용 안 함(C#)</span><span class="sxs-lookup"><span data-stu-id="40de2-104">Disabling Actions during Animation (C#)</span></span>
====================
<span data-ttu-id="40de2-105">by [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="40de2-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="40de2-106">[코드를 다운로드](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) 또는 [PDF 다운로드](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="40de2-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span></span>

> <span data-ttu-id="40de2-107">ASP.NET AJAX Control Toolkit에서 애니메이션 컨트롤 컨트롤 뿐 이지만 컨트롤에 애니메이션을 추가 하는 전체 프레임 워크 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="40de2-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="40de2-108">또한 마우스 클릭와 같은 작업을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="40de2-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="40de2-109">그러나 애니메이션을 시작 하는 마우스 클릭 때 애니메이션 하는 동안 마우스 클릭을 사용 하지 않도록 설정 하는 것이 바람직합니다.</span><span class="sxs-lookup"><span data-stu-id="40de2-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>


## <a name="overview"></a><span data-ttu-id="40de2-110">개요</span><span class="sxs-lookup"><span data-stu-id="40de2-110">Overview</span></span>

<span data-ttu-id="40de2-111">ASP.NET AJAX Control Toolkit에서 애니메이션 컨트롤 컨트롤 뿐 이지만 컨트롤에 애니메이션을 추가 하는 전체 프레임 워크 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="40de2-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="40de2-112">또한 마우스 클릭와 같은 작업을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="40de2-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="40de2-113">그러나 애니메이션을 시작 하는 마우스 클릭 때 애니메이션 하는 동안 마우스 클릭을 사용 하지 않도록 설정 하는 것이 바람직합니다.</span><span class="sxs-lookup"><span data-stu-id="40de2-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="40de2-114">단계</span><span class="sxs-lookup"><span data-stu-id="40de2-114">Steps</span></span>

<span data-ttu-id="40de2-115">첫째, 포함 된 `ScriptManager` 페이지 그런 다음 ASP.NET AJAX 라이브러리 로드 되 면 컨트롤 도구 키트를 사용 하 여:</span><span class="sxs-lookup"><span data-stu-id="40de2-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

<span data-ttu-id="40de2-116">다음과 같은 HTML 단추를 사용 하는 애니메이션 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="40de2-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

<span data-ttu-id="40de2-117">HTML 컨트롤을 포스트백 만들기 단추 하지 않을 것 이므로 웹 컨트롤을 대신 사용 됩니다 미국에 대 한 클라이언트 쪽 애니메이션을 실행 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="40de2-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="40de2-118">그런 다음 추가 `AnimationExtender` 페이지에서 제공 하는 `ID`, `TargetControlID` 특성과 필수 항목 이지만 `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="40de2-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

<span data-ttu-id="40de2-119">내 합니다 `<Animations>` 노드를 `<OnClick>` 적합 한 요소를 마우스 클릭을 처리 하는 합니다.</span><span class="sxs-lookup"><span data-stu-id="40de2-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="40de2-120">그러나도 애니메이션 하는 동안 단추를 클릭할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="40de2-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="40de2-121">`<EnableAction>` 요소를 처리할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="40de2-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="40de2-122">설정 `Enabled="false"` 애니메이션의 일부로 단추를 사용 하지 않도록 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="40de2-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="40de2-123">여러 개별 애니메이션 (사용 안 함 단추와 실제 애니메이션)를 사용 하 고 있으므로 `<Parallel>` 요소 하나로 함께 단일 애니메이션을 연결할 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="40de2-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="40de2-124">다음은 전체 태그를 `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="40de2-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

<span data-ttu-id="40de2-125">또한 목록 끝에 다음 XML 요소를 사용 하 여 애니메이션 후 단추를 다시 사용 하도록 설정 수: 있습니다.</span><span class="sxs-lookup"><span data-stu-id="40de2-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

<span data-ttu-id="40de2-126">그러나 특정된 시나리오에서이 쓸모 없게 단추 이후 페이드 아웃 및 애니메이션의 끝에 표시 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="40de2-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>


<span data-ttu-id="40de2-127">[![이 애니메이션은 실행 되는 즉시 단추가 비활성화 됩니다.](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="40de2-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span></span>

<span data-ttu-id="40de2-128">이 애니메이션은 실행 되는 즉시 단추가 비활성화 됩니다 ([클릭 하 여 큰 이미지 보기](disabling-actions-during-animation-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="40de2-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="40de2-129">[이전](animating-in-response-to-user-interaction-cs.md)
> [다음](triggering-an-animation-in-another-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="40de2-129">[Previous](animating-in-response-to-user-interaction-cs.md)
[Next](triggering-an-animation-in-another-control-cs.md)</span></span>