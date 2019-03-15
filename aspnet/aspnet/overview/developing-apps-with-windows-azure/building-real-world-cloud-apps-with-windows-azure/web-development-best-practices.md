---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: 웹 개발 모범 사례 (실제 클라우드 앱 빌드 azure) | Microsoft Docs
author: MikeWasson
description: 실제 세계 클라우드 앱 빌드 Azure 전자책을 사용 하 여 Scott Guthrie를 개발한 프레젠테이션을 기반으로 합니다. 13 패턴과 그을 수 있는 방법을 설명 하는 중...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: 930b9be35ef2e0dd85cee8f6584b9e90c80933b9
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029230"
---
<a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="8e50f-104">웹 개발 모범 사례 (Azure 사용 하 여 빌드 실제 클라우드 앱)</span><span class="sxs-lookup"><span data-stu-id="8e50f-104">Web Development Best Practices (Building Real-World Cloud Apps with Azure)</span></span>
====================
<span data-ttu-id="8e50f-105">하 여 [Mike Wasson](https://github.com/MikeWasson)하십시오 [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="8e50f-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="8e50f-106">[다운로드 해결 프로젝트](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) 또는 [전자책 다운로드](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="8e50f-106">[Download Fix It Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) or [Download E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span></span>

> <span data-ttu-id="8e50f-107">합니다 **실제 세계 클라우드 앱 빌드 Azure** 전자책 Scott Guthrie를 개발한 프레젠테이션을 기반으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-107">The **Building Real World Cloud Apps with Azure** e-book is based on a presentation developed by Scott Guthrie.</span></span> <span data-ttu-id="8e50f-108">13 패턴 설명 하 고 도움이 될 수 있는 사례 성공적인 클라우드를 위한 웹 앱을 개발할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-108">It explains 13 patterns and practices that can help you be successful developing web apps for the cloud.</span></span> <span data-ttu-id="8e50f-109">전자책에 대 한 정보를 참조 하세요 [첫 번째 장에서](introduction.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-109">For information about the e-book, see [the first chapter](introduction.md).</span></span>


<span data-ttu-id="8e50f-110">처음 세 패턴은 민첩 한 개발 프로세스; 설정에 대 한 되었습니다. 아키텍처 및 코드에 대 한 나머지가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-110">The first three patterns were about setting up an agile development process; the rest are about architecture and code.</span></span> <span data-ttu-id="8e50f-111">이 웹 개발 모범 사례 컬렉션:</span><span class="sxs-lookup"><span data-stu-id="8e50f-111">This one is a collection of web development best practices:</span></span>

- <span data-ttu-id="8e50f-112">[상태 비저장 웹 서버](#stateless) 스마트 부하 분산 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-112">[Stateless web servers](#stateless) behind a smart load balancer.</span></span>
- <span data-ttu-id="8e50f-113">[세션 상태를 방지](#sessionstate) (또는 것을 방지할 수 없습니다, 하는 경우 데이터베이스 보다는 분산된 캐시 사용).</span><span class="sxs-lookup"><span data-stu-id="8e50f-113">[Avoid session state](#sessionstate) (or if you can't avoid it, use distributed cache rather than a database).</span></span>
- <span data-ttu-id="8e50f-114">[CDN을 사용 하 여](#cdn) -에 지 캐시 정적 파일 자산 (이미지, 스크립트).</span><span class="sxs-lookup"><span data-stu-id="8e50f-114">[Use a CDN](#cdn) to edge-cache static file assets (images, scripts).</span></span>
- <span data-ttu-id="8e50f-115">[.NET 4.5의 비동기 지원을 사용 하 여](#async) 호출을 차단 하지 않도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-115">[Use .NET 4.5's async support](#async) to avoid blocking calls.</span></span>

<span data-ttu-id="8e50f-116">이러한 사례는 클라우드 앱에 대 한 뿐 아니라 모든 웹 개발을 위한 유효 하 하지만 클라우드 앱에 대해 특히 중요 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-116">These practices are valid for all web development, not just for cloud apps, but they're especially important for cloud apps.</span></span> <span data-ttu-id="8e50f-117">클라우드 환경에서 제공 하는 매우 유연한 크기 조정의 사용을 최적화 하기 위해 함께 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-117">They work together to help you make optimal use of the highly flexible scaling offered by the cloud environment.</span></span> <span data-ttu-id="8e50f-118">이러한 사례를 따르지 않으면 응용 프로그램을 확장 하려고 할 때 한계에 부 딪 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-118">If you don't follow these practices, you'll run into limitations when you try to scale your application.</span></span>

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a><span data-ttu-id="8e50f-119">스마트 부하 분산 장치 상태 비저장 웹 계층</span><span class="sxs-lookup"><span data-stu-id="8e50f-119">Stateless web tier behind a smart load balancer</span></span>

<span data-ttu-id="8e50f-120">*상태 비저장 웹 계층* 웹 서버의 메모리 나 파일 시스템에서 응용 프로그램 데이터를 저장 하지 것을 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-120">*Stateless web tier* means you don't store any application data in the web server memory or file system.</span></span> <span data-ttu-id="8e50f-121">상태 비저장 웹 계층을 유지를 사용 하면 모두 더 나은 고객 경험을 제공 하 고 비용을 절감할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-121">Keeping your web tier stateless enables you to both provide a better customer experience and save money:</span></span>

- <span data-ttu-id="8e50f-122">웹 계층 상태 비저장 이며 부하 분산 장치 뒤에 응답할 수 있습니다 신속 하 게 응용 프로그램 트래픽의 변경 내용에 동적으로 추가 하거나 서버를 제거 하 여.</span><span class="sxs-lookup"><span data-stu-id="8e50f-122">If the web tier is stateless and it sits behind a load balancer, you can quickly respond to changes in application traffic by dynamically adding or removing servers.</span></span> <span data-ttu-id="8e50f-123">위치에 대 한 요금만 서버 리소스를 실제로 사용, 클라우드 환경에서 수요 변화에에서 응답 하는 해당 기능이 비용이 크게 절감으로 변환할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-123">In the cloud environment where you only pay for server resources for as long as you actually use them, that ability to respond to changes in demand can translate into huge savings.</span></span>
- <span data-ttu-id="8e50f-124">상태 비저장 웹 계층은 아키텍처 측면에서 훨씬 간단 하 게 응용 프로그램을 확장 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-124">A stateless web tier is architecturally much simpler to scale out the application.</span></span> <span data-ttu-id="8e50f-125">너무 응답 요구를 보다 신속 하 게 확장 하 고 개발 및 테스트에 적은 비용을 투자할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-125">That too enables you to respond to scaling needs more quickly, and spend less money on development and testing in the process.</span></span>
- <span data-ttu-id="8e50f-126">온-프레미스 서버와 같이 클라우드 서버 패치 되어; 경우에 따라 다시 부팅 필요 및 웹 계층 상태 비저장 인 경우 오류나 예기치 않은 동작이 발생 하지 서버가 일시적으로 중단 하는 경우 트래픽을 다시 라우팅하는 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-126">Cloud servers, like on-premises servers, need to be patched and rebooted occasionally; and if the web tier is stateless, re-routing traffic when a server goes down temporarily won't cause errors or unexpected behavior.</span></span>

<span data-ttu-id="8e50f-127">대부분의 실제 응용 프로그램을 웹 세션;에 대 한 상태를 저장할 필요가 요점은 여기 웹 서버에 저장 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-127">Most real-world applications do need to store state for a web session; the main point here is not to store it on the web server.</span></span> <span data-ttu-id="8e50f-128">쿠키 또는 프로세스 서버 쪽 ASP.NET 세션 상태 캐시 공급자를 사용 하 여에서 클라이언트와 같은 다른 방법으로 상태를 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-128">You can store state in other ways, such as on the client in cookies or out of process server-side in ASP.NET session state using a cache provider.</span></span> <span data-ttu-id="8e50f-129">파일에 저장할 수 있습니다 [Windows Azure Blob storage](unstructured-blob-storage.md) 로컬 파일 시스템 대신 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-129">You can store files in [Windows Azure Blob storage](unstructured-blob-storage.md) instead of the local file system.</span></span>

<span data-ttu-id="8e50f-130">웹 계층은 상태 비저장 이어야 하는 경우 응용 프로그램을 Windows Azure 웹 사이트에서 확장을 얼마나 쉬운지의 예를 들어, 참조를 **확장** 관리 포털에서 Windows Azure 웹 사이트에 대 한 탭:</span><span class="sxs-lookup"><span data-stu-id="8e50f-130">As an example of how easy it is to scale an application in Windows Azure Web Sites if your web tier is stateless, see the **Scale** tab for a Windows Azure Web Site in the management portal:</span></span>

![크기 조정 탭](web-development-best-practices/_static/image1.png)

<span data-ttu-id="8e50f-132">웹 서버를 추가 하려는 경우에 오른쪽으로 인스턴스 개수 슬라이더를 끌어만 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-132">If you want to add web servers, you can just drag the instance count slider to the right.</span></span> <span data-ttu-id="8e50f-133">5로 설정 하 고 클릭 **저장할**, 몇 초 이내 5 명의 웹 서버에에서 있으면 Windows Azure 웹 사이트의 트래픽을 처리 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-133">Set it to 5 and click **Save**, and within seconds you have 5 web servers in Windows Azure handling your web site's traffic.</span></span>

![5 개의 인스턴스](web-development-best-practices/_static/image2.png)

<span data-ttu-id="8e50f-135">인스턴스 수 1까지 다시 또는 3으로 간단 하 게 설정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-135">You can just as easily set the instance count down to 3 or back down to 1.</span></span> <span data-ttu-id="8e50f-136">비용을 절약 하기 시작 백을 확장할 때 바로 Windows Azure 시가 아니라 분 단위로 청구 하기 때문에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-136">When you scale back, you start saving money immediately because Windows Azure charges by the minute, not by the hour.</span></span>

<span data-ttu-id="8e50f-137">또한 Windows Azure를 자동으로 늘리거나 CPU 사용량을 기준으로 하는 웹 서버 수를 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-137">You can also tell Windows Azure to automatically increase or decrease the number of web servers based on CPU usage.</span></span> <span data-ttu-id="8e50f-138">다음 예제에서는 2 개를 웹 서버 수가 줄어듭니다 CPU 사용률이 60% 아래로 떨어지면에 CPU 사용량이 80%를 초과 되 면 웹 서버 수가 4의 최대 증가 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-138">In the following example, when CPU usage goes below 60%, the number of web servers will decrease to a minimum of 2, and if CPU usage goes above 80%, the number of web servers will be increased up to a maximum of 4.</span></span>

![CPU 사용량 기준 크기 조정](web-development-best-practices/_static/image3.png)

<span data-ttu-id="8e50f-140">또는 사이트만 되도록 사용 중인 작업 시간을 알고 있는 경우?</span><span class="sxs-lookup"><span data-stu-id="8e50f-140">Or what if you know that your site will only be busy during working hours?</span></span> <span data-ttu-id="8e50f-141">Windows Azure 주간 여러 서버를 실행 하 고 단일 서버 evenings, 일, 시간 및 주말 감소를 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-141">You can tell Windows Azure to run multiple servers during the daytime and decrease to a single server evenings, nights, and weekends.</span></span> <span data-ttu-id="8e50f-142">다음과 같은 일련의 스크린샷이 오전 8 시에서 오후 5 시 근무 시간 중 시간에 한 서버 및 4 대의 서버를 실행 하는 웹 사이트를 설정 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-142">The following series of screen shots shows how to set up the web site to run one server in off hours and 4 servers during work hours from 8 AM to 5 PM.</span></span>

![일정에 따라 크기 조정](web-development-best-practices/_static/image4.png)

![예약 시간 설정](web-development-best-practices/_static/image5.png)

![주간 일정](web-development-best-practices/_static/image6.png)

![Weeknight 일정](web-development-best-practices/_static/image7.png)

![주말 일정](web-development-best-practices/_static/image8.png)

<span data-ttu-id="8e50f-148">물론이 모든 작업을 수행 하려면 포털와 같이 스크립트를</span><span class="sxs-lookup"><span data-stu-id="8e50f-148">And of course all of this can be done in scripts as well as in the portal.</span></span>

<span data-ttu-id="8e50f-149">동적으로 추가 하거나 상태 비저장 웹 계층을 유지 하 여 server Vm 2 개를 제거 하려면 장애를 방지 된다면 스케일 아웃 응용 프로그램의 기능 Windows azure에서 거의 제한 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-149">The ability of your application to scale out is almost unlimited in Windows Azure, so long as you avoid impediments to dynamically adding or removing server VMs, by keeping the web tier stateless.</span></span>

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a><span data-ttu-id="8e50f-150">세션 상태를 방지 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-150">Avoid session state</span></span>

<span data-ttu-id="8e50f-151">사용자 세션에 대 한 일종의 상태를 저장 하지 않으려면 실제 클라우드 앱에서 대개 유용 하지만 몇 가지 접근 방식을 성능 및 확장성을 다른 항목 보다 더 많은 영향을 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-151">It's often not practical in a real-world cloud app to avoid storing some form of state for a user session, but some approaches impact performance and scalability more than others.</span></span> <span data-ttu-id="8e50f-152">상태를 저장 해야 할 경우는 가장 좋은 방법은 상태의 크기를 작게 유지 하 고 쿠키에 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-152">If you have to store state, the best solution is to keep the amount of state small and store it in cookies.</span></span> <span data-ttu-id="8e50f-153">다음 최상의 솔루션에 대 한 공급자를 사용 하 여 ASP.NET 세션 상태를 사용 하는 것 경우는 불가능 [메모리 내 분산된 캐시](distributed-caching.md#sessionstate)합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-153">If that isn't feasible, the next best solution is to use ASP.NET session state with a provider for [distributed, in-memory cache](distributed-caching.md#sessionstate).</span></span> <span data-ttu-id="8e50f-154">성능 및 확장성 측면에서 최악의 솔루션의 경우 데이터베이스를 사용 하도록 지원 세션 상태 제공자</span><span class="sxs-lookup"><span data-stu-id="8e50f-154">The worst solution from a performance and scalability standpoint is to use a database backed session state provider.</span></span>

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a><span data-ttu-id="8e50f-155">정적 파일 자산을 캐시 하도록 CDN을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-155">Use a CDN to cache static file assets</span></span>

<span data-ttu-id="8e50f-156">CDN은 Content Delivery Network에 대 한 머리글자어입니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-156">CDN is an acronym for Content Delivery Network.</span></span> <span data-ttu-id="8e50f-157">CDN 공급자에 이미지 및 스크립트 파일과 같은 정적 파일 자산을 제공 하 고 공급자 사용자 응용 프로그램에 액세스 하는 경우 어디서 나 얻을 수 있도록 비교적 빠른 응답 및 짧은 대기 시간에 캐시 된 전 세계 데이터 센터에서 이러한 파일을 캐시 자산입니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-157">You provide static file assets such as images and script files to a CDN provider, and the provider caches these files in data centers all over the world so that wherever people access your application, they get relatively quick response and low latency for the cached assets.</span></span> <span data-ttu-id="8e50f-158">이 사이트의 전체 부하 시간 속도 웹 서버의 부하를 줄입니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-158">This speeds up the overall load time of the site and reduces the load on your web servers.</span></span> <span data-ttu-id="8e50f-159">Cdn은 광범위 하 게 지리적으로 분산 되는 대상에 도달 하는 경우에 특히 중요 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-159">CDNs are especially important if you are reaching an audience that is widely distributed geographically.</span></span>

<span data-ttu-id="8e50f-160">Windows Azure에 CDN, 및 Windows Azure 또는 모든 웹 호스팅 환경에서 실행 되는 응용 프로그램에서 다른 Cdn을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-160">Windows Azure has a CDN, and you can use other CDNs in an application that runs in Windows Azure or any web hosting environment.</span></span>

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a><span data-ttu-id="8e50f-161">.NET 4.5의 비동기 지원을 사용 하 여 호출 차단 방지</span><span class="sxs-lookup"><span data-stu-id="8e50f-161">Use .NET 4.5's async support to avoid blocking calls</span></span>

<span data-ttu-id="8e50f-162">작업을 비동기적으로 처리를 더 간단 하 게 하기 위해 C# 및 VB 프로그래밍 언어를 개선 하는.NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="8e50f-162">.NET 4.5 enhanced the C# and VB programming languages in order to make it much simpler to handle tasks asynchronously.</span></span> <span data-ttu-id="8e50f-163">비동기 프로그래밍의 장점은 여러 웹 서비스 호출을 동시에 시작 하려는 경우 같은 병렬 처리 상황에 대 한 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-163">The benefit of asynchronous programming is not just for parallel processing situations such as when you want to kick off multiple web service calls simultaneously.</span></span> <span data-ttu-id="8e50f-164">또한 웹 서버를 보다 효율적으로 수행할 수 있습니다 및 부하가 높은 상황에서 신뢰할 수 있는 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-164">It also enables your web server to perform more efficiently and reliable under high load conditions.</span></span> <span data-ttu-id="8e50f-165">웹 서버를 사용할 수 있는 스레드 수가 제한에 있고 부하가 높은 상황에서 사용 중인 모든 스레드의 경우 들어오는 요청 스레드가 해제 될 때까지 기다려야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-165">A web server only has a limited number of threads available, and under high load conditions when all of the threads are in use, incoming requests have to wait until threads are freed up.</span></span> <span data-ttu-id="8e50f-166">응용 프로그램 코드에서 쿼리 및 웹 서비스 호출을 비동기적으로 데이터베이스와 같은 태스크를 처리 하지 않는 경우 서버 I/O 응답을 기다리는 동안 많은 스레드 불필요 하 게 연결 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-166">If your application code doesn't handle tasks like database queries and web service calls asynchronously, many threads are unnecessarily tied up while the server is waiting for an I/O response.</span></span> <span data-ttu-id="8e50f-167">이 서버 부하가 높은 상황에서 처리할 수 있습니다 트래픽 양을 제한 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-167">This limits the amount of traffic the server can handle under high load conditions.</span></span> <span data-ttu-id="8e50f-168">비동기 프로그래밍을 사용 하 여 웹 서비스 또는 데이터를 반환 하는 데이터베이스에 대 한 대기 중인 스레드는 새 요청을 처리 하는 최대 때까지 해제 데이터를 수신 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-168">With asynchronous programming, threads that are waiting for a web service or database to return data are freed up to service new requests until the data the is received.</span></span> <span data-ttu-id="8e50f-169">사용량이 많은 웹 서버에서 수백 또는 수천 개의 요청 다음 처리할 수 있습니다 신속 하 게 스레드를 확보할 수이 고, 그렇지 대기입니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-169">In a busy web server, hundreds or thousands of requests can then be processed promptly which would otherwise be waiting for threads to be freed up.</span></span>

<span data-ttu-id="8e50f-170">앞서 살펴본 것 처럼 이러한 증가 하는 것 만큼 웹 사이트를 처리 하는 웹 서버의 수를 줄이려면으로 쉽습니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-170">As you saw earlier, it's as easy to decrease the number of web servers handling your web site as it is to increase them.</span></span> <span data-ttu-id="8e50f-171">따라서 서버는 더 높은 처리량을 달성할 수 있습니다, 필요가 없으면 그렇지 않은 경우 보다 더 적은 서버는 지정 된 트래픽 볼륨에 대 한 필요 하기 때문에 비용을 줄일 수 있습니다 하 고 많은으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-171">So if a server can achieve greater throughput, you don't need as many of them and you can decrease your costs because you need fewer servers for a given traffic volume than you otherwise would.</span></span>

<span data-ttu-id="8e50f-172">Web Forms, MVC 및 웹 API에 대 한 ASP.NET 4.5에서는.NET 4.5의 비동기 프로그래밍 모델에 대 한 지원이 포함 됩니다. Entity Framework 6에는 [Windows Azure 저장소 API](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx)합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-172">Support for the .NET 4.5 asynchronous programming model is included in ASP.NET 4.5 for Web Forms, MVC, and Web API; in Entity Framework 6, and in the [Windows Azure Storage API](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx).</span></span>

### <a name="async-support-in-aspnet-45"></a><span data-ttu-id="8e50f-173">ASP.NET 4.5의 비동기 지원</span><span class="sxs-lookup"><span data-stu-id="8e50f-173">Async support in ASP.NET 4.5</span></span>

<span data-ttu-id="8e50f-174">ASP.NET 4.5의 비동기 프로그래밍 언어 뿐만 아니라도 MVC, Web Forms 및 Web API 프레임 워크에 추가 된 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-174">In ASP.NET 4.5, support for asynchronous programming has been added not just to the language but also to the MVC, Web Forms, and Web API frameworks.</span></span> <span data-ttu-id="8e50f-175">예를 들어, ASP.NET MVC 컨트롤러 동작 메서드 웹 요청에서 데이터를 수신 하 고 브라우저에 전송할 수 있도록 HTML을 만드는 다음 뷰로 데이터를 전달 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-175">For example, an ASP.NET MVC controller action method receives data from a web request and passes the data to a view which then creates the HTML to be sent to the browser.</span></span> <span data-ttu-id="8e50f-176">자주 작업 메서드는 웹 페이지에 표시 하기 위해 또는 웹 페이지에 입력 된 데이터를 저장 하는 데이터베이스 또는 웹 서비스에서 데이터를 가져올 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-176">Frequently the action method needs to get data from a database or web service in order to display it in a web page or to save data entered in a web page.</span></span> <span data-ttu-id="8e50f-177">이러한 시나리오에서는 비동기 작업 메서드를 손쉽게: 반환 하는 대신는 *ActionResult* 개체를 반환 하면 *태스크&lt;ActionResult&gt;*  메서드를 표시 사용 하 여 합니다 *비동기* 키워드입니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-177">In those scenarios it's easy to make the action method asynchronous: instead of returning an *ActionResult* object, you return *Task&lt;ActionResult&gt;* and mark the method with the *async* keyword.</span></span> <span data-ttu-id="8e50f-178">메서드 내부에서 코드 줄을 대기 시간을 포함 하는 작업을 시작 하는 경우 표시할 있습니다 await 키워드를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-178">Inside the method, when a line of code kicks off an operation that involves wait time, you mark it with the await keyword.</span></span>

<span data-ttu-id="8e50f-179">데이터베이스 쿼리에 대 한 리포지토리 메서드를 호출 하는 간단한 작업 메서드는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-179">Here is a simple action method that calls a repository method for a database query:</span></span>

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

<span data-ttu-id="8e50f-180">그리고은 데이터베이스 호출을 비동기적으로 처리 하는 동일한 메서드.</span><span class="sxs-lookup"><span data-stu-id="8e50f-180">And here is the same method that handles the database call asynchronously:</span></span>

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

<span data-ttu-id="8e50f-181">내부적으로 컴파일러는 해당 비동기 코드를 생성합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-181">Under the covers the compiler generates the appropriate asynchronous code.</span></span> <span data-ttu-id="8e50f-182">응용 프로그램에 호출을 수행 하는 경우 `FindTaskByIdAsync`, ASP.NET을 사용 하 여 `FindTask` 요청 다음 작업자 스레드를 해제 하 고 다른 요청을 처리 하는 데 사용할 수 있도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-182">When the application makes the call to `FindTaskByIdAsync`, ASP.NET makes the `FindTask` request and then unwinds the worker thread and makes it available to process another request.</span></span> <span data-ttu-id="8e50f-183">경우는 `FindTask` 을 요청을 수행 하는 스레드를 호출 하는 다음에 오는 코드 처리를 계속 하려면 다시 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-183">When the `FindTask` request is done, a thread is restarted to continue processing the code that comes after that call.</span></span> <span data-ttu-id="8e50f-184">될 때 까지는 그 중의 `FindTask` 요청 시작 되 고 있는 대기 하느라 정체 될 응답에 대 한 유용한 작업을 수행 하기 위한 스레드 데이터 반환 되 면 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-184">During the interim between when the `FindTask` request is initiated and when the data is returned, you have a thread available to do useful work which otherwise would be tied up waiting for the response.</span></span>

<span data-ttu-id="8e50f-185">비동기 코드에 대 한 오버 헤드가 발생 하지만 낮은 부하 조건에서 오버 헤드는 그렇지 않은 경우 사용 가능한 스레드가 있을 때까지 보유 수는 요청을 처리할 수는 부하가 높은 조건 하는 동안 미미 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-185">There is some overhead for asynchronous code, but under low-load conditions, that overhead is negligible, while under high-load conditions you're able to process requests that otherwise would be held up waiting for available threads.</span></span>

<span data-ttu-id="8e50f-186">이러한 종류의 ASP.NET 1.1부터 비동기 프로그래밍을 할 수 있었습니다 했지만 작성 하는 것, 오류 발생률 및 디버그 하기가 어렵습니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-186">It has been possible to do this kind of asynchronous programming since ASP.NET 1.1, but it was difficult to write, error-prone, and difficult to debug.</span></span> <span data-ttu-id="8e50f-187">ASP.NET 4.5에서에 대 한 코딩을 간소화 했습니다 했으므로 더 이상 그럴 필요가 없는 이유는.</span><span class="sxs-lookup"><span data-stu-id="8e50f-187">Now that we've simplified the coding for it in ASP.NET 4.5, there's no reason not to do it anymore.</span></span>

### <a name="async-support-in-entity-framework-6"></a><span data-ttu-id="8e50f-188">Entity Framework 6에 대 한 비동기 지원</span><span class="sxs-lookup"><span data-stu-id="8e50f-188">Async support in Entity Framework 6</span></span>

<span data-ttu-id="8e50f-189">4.5의 비동기 지원의 일환으로 웹 서비스 호출, 소켓 및 파일 시스템 I/O에 대 한 비동기 지원 출시 하지만 웹 응용 프로그램에 대 한 가장 일반적인 패턴은 데이터베이스에 도달 하 고 데이터 라이브러리 비동기를 지원 하지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-189">As part of async support in 4.5 we shipped async support for web service calls, sockets, and file system I/O, but the most common pattern for web applications is to hit a database, and our data libraries didn't support async.</span></span> <span data-ttu-id="8e50f-190">이제 Entity Framework 6에는 데이터베이스 액세스에 대 한 비동기 지원을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-190">Now Entity Framework 6 adds async support for database access.</span></span>

<span data-ttu-id="8e50f-191">Entity Framework 6 모든 메서드는 쿼리 또는 데이터베이스를 전송할 수 있도록 명령에는 비동기 버전입니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-191">In Entity Framework 6 all methods that cause a query or command to be sent to the database have async versions.</span></span> <span data-ttu-id="8e50f-192">예제에서는 비동기 버전을 표시 합니다 *찾을* 메서드.</span><span class="sxs-lookup"><span data-stu-id="8e50f-192">The example here shows the async version of the *Find* method.</span></span>

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

<span data-ttu-id="8e50f-193">삽입, 삭제, 업데이트 및 간단한 발견 한 뿐 아니라이 비동기 지원 작동을 LINQ 쿼리를 사용 하 여 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-193">And this async support works not just for inserts, deletes, updates, and simple finds, it also works with LINQ queries:</span></span>

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

<span data-ttu-id="8e50f-194">`Async` 버전의는 `ToList` 메서드 이므로이 코드에서는 데이터베이스를 전송할 수 있도록 쿼리를 발생 시키는 메서드.</span><span class="sxs-lookup"><span data-stu-id="8e50f-194">There's an `Async` version of the `ToList` method because in this code that's the method that causes a query to be sent to the database.</span></span> <span data-ttu-id="8e50f-195">`Where` 및 `OrderByDescending` 메서드는만 쿼리를 구성 하는 동안는 `ToListAsync` 메서드는 쿼리를 실행 하 고 응답을 저장를 `result` 변수입니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-195">The `Where` and `OrderByDescending` methods only configure the query, while the `ToListAsync` method executes the query and stores the response in the `result` variable.</span></span>

## <a name="summary"></a><span data-ttu-id="8e50f-196">요약</span><span class="sxs-lookup"><span data-stu-id="8e50f-196">Summary</span></span>

<span data-ttu-id="8e50f-197">모든 웹 프로그래밍 프레임 워크 및 모든 클라우드 환경에서에서 여기에 설명 된 웹 개발 모범 사례를 구현할 수 있지만, 쉽게 ASP.NET 및 Windows Azure의 도구를 사용 했습니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-197">You can implement the web development best practices outlined here in any web programming framework and any cloud environment, but we have tools in ASP.NET and Windows Azure to make it easy.</span></span> <span data-ttu-id="8e50f-198">이러한 패턴을 따르는 경우에 웹 계층을 쉽게 확장할 수 및 각 서버는 더 많은 트래픽을 처리할 수 있기 때문에 프로그램 비용을 최소화 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-198">If you follow these patterns, you can easily scale out your web tier, and you'll minimize your expenses because each server will be able to handle more traffic.</span></span>

<span data-ttu-id="8e50f-199">합니다 [다음 장에서](single-sign-on.md) 클라우드 single sign-on 시나리오를 사용 하는 방법을 살펴봅니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-199">The [next chapter](single-sign-on.md) looks at how the cloud enables single sign-on scenarios.</span></span>

## <a name="resources"></a><span data-ttu-id="8e50f-200">자료</span><span class="sxs-lookup"><span data-stu-id="8e50f-200">Resources</span></span>

<span data-ttu-id="8e50f-201">자세한 내용은 다음 리소스를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="8e50f-201">For more information see the following resources.</span></span>

<span data-ttu-id="8e50f-202">상태 비저장 웹 서버:</span><span class="sxs-lookup"><span data-stu-id="8e50f-202">Stateless web servers:</span></span>

- <span data-ttu-id="8e50f-203">[Microsoft Patterns and Practices-자동 크기 조정 지침](https://msdn.microsoft.com/library/dn589774.aspx)합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-203">[Microsoft Patterns and Practices - Autoscaling guidance](https://msdn.microsoft.com/library/dn589774.aspx).</span></span>
- <span data-ttu-id="8e50f-204">[사용 하지 않도록 설정 ARR 선호도 Windows Azure 웹 사이트에서 인스턴스](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/)합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-204">[Disabling ARR's Instance Affinity in Windows Azure Web Sites](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/).</span></span> <span data-ttu-id="8e50f-205">Erez benari 블로그 게시물에는 세션 선호도 Windows Azure 웹 사이트에 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-205">Blog post by Erez Benari, explains session affinity in Windows Azure Web Sites.</span></span>

<span data-ttu-id="8e50f-206">CDN:</span><span class="sxs-lookup"><span data-stu-id="8e50f-206">CDN:</span></span>

- <span data-ttu-id="8e50f-207">[FailSafe: 확장성, 복원 력 있는 클라우드 서비스를 만드는 데](https://channel9.msdn.com/Series/FailSafe)합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-207">[FailSafe: Building Scalable, Resilient Cloud Services](https://channel9.msdn.com/Series/FailSafe).</span></span> <span data-ttu-id="8e50f-208">Marc Mercuri, Ulrich Homann, Mark Simms에서 비디오 시리즈를 9 개 부분으로 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-208">Nine-part video series by Ulrich Homann, Marc Mercuri, and Mark Simms.</span></span> <span data-ttu-id="8e50f-209">에피소드 3 시작 1시 34분: 00에서 CDN을 참조 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-209">See the CDN discussion in episode 3 starting at 1:34:00.</span></span>
- [<span data-ttu-id="8e50f-210">Microsoft 패턴 및 사례 정적 콘텐츠 호스팅 패턴</span><span class="sxs-lookup"><span data-stu-id="8e50f-210">Microsoft Patterns and Practices Static Content Hosting pattern</span></span>](https://msdn.microsoft.com/library/dn589776.aspx)
- <span data-ttu-id="8e50f-211">[CDN 검토](http://www.cdnreviews.com/)합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-211">[CDN Reviews](http://www.cdnreviews.com/).</span></span> <span data-ttu-id="8e50f-212">여러 Cdn의 개요입니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-212">Overview of many CDNs.</span></span>

<span data-ttu-id="8e50f-213">비동기 프로그래밍:</span><span class="sxs-lookup"><span data-stu-id="8e50f-213">Asynchronous programming:</span></span>

- <span data-ttu-id="8e50f-214">[ASP.NET MVC 4에서에서 비동기 메서드를 사용 하 여](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md)입니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-214">[Using Asynchronous Methods in ASP.NET MVC 4](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).</span></span> <span data-ttu-id="8e50f-215">Rick anderson 자습서입니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-215">Tutorial by Rick Anderson.</span></span>
- <span data-ttu-id="8e50f-216">[비동기 비동기를 사용 하 여 프로그래밍 및 Await (C# 및 Visual Basic)](https://msdn.microsoft.com/library/vstudio/hh191443.aspx)합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-216">[Asynchronous Programming with Async and Await (C# and Visual Basic)](https://msdn.microsoft.com/library/vstudio/hh191443.aspx).</span></span> <span data-ttu-id="8e50f-217">MSDN 백서는 비동기 프로그래밍에 대 한 근거, ASP.NET 4.5의 작동 방식 및 구현 하는 코드를 작성 하는 방법을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-217">MSDN white paper that explains rationale for asynchronous programming, how it works in ASP.NET 4.5, and how to write code to implement it.</span></span>
- [<span data-ttu-id="8e50f-218">Entity Framework 비동기 쿼리 및 저장</span><span class="sxs-lookup"><span data-stu-id="8e50f-218">Entity Framework Async Query and Save</span></span>](https://msdn.microsoft.com/data/jj819165)
- <span data-ttu-id="8e50f-219">[비동기를 사용 하 여 ASP.NET 웹 응용 프로그램을 빌드하는 방법을](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7)합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-219">[How to Build ASP.NET Web Applications Using Async](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7).</span></span> <span data-ttu-id="8e50f-220">Rowan Miller의 비디오 프레젠테이션 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-220">Video presentation by Rowan Miller.</span></span> <span data-ttu-id="8e50f-221">그래픽 데모를 포함 합니다. 비동기 프로그래밍의 부하가 높은 상황에서 웹 서버 처리량으로 증대할 쉽게 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-221">Includes a graphic demonstration of how asynchronous programming can facilitate dramatic increases in web server throughput under high load conditions.</span></span>
- <span data-ttu-id="8e50f-222">[FailSafe: 확장성, 복원 력 있는 클라우드 서비스를 만드는 데](https://channel9.msdn.com/Series/FailSafe)합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-222">[FailSafe: Building Scalable, Resilient Cloud Services](https://channel9.msdn.com/Series/FailSafe).</span></span> <span data-ttu-id="8e50f-223">Marc Mercuri, Ulrich Homann, Mark Simms에서 비디오 시리즈를 9 개 부분으로 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-223">Nine-part video series by Ulrich Homann, Marc Mercuri, and Mark Simms.</span></span> <span data-ttu-id="8e50f-224">확장성 비동기 프로그래밍의 영향에 대 한 토론에 대 한 에피소드 4 및 8 에피소드를 참조 하십시오.</span><span class="sxs-lookup"><span data-stu-id="8e50f-224">For discussions about the impact of asynchronous programming on scalability, see episode 4 and episode 8.</span></span>
- <span data-ttu-id="8e50f-225">[ASP.NET 4.5와 중요 한 과제는 비동기 메서드를 사용 하 여의 마법](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx)합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-225">[The Magic of using Asynchronous Methods in ASP.NET 4.5 plus an important gotcha](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx).</span></span> <span data-ttu-id="8e50f-226">비동기를 사용 하 여 ASP.NET Web Forms 응용 프로그램에서에 대 한 주로 Scott hanselman 블로그 게시물입니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-226">Blog post by Scott Hanselman, primarily about using async in ASP.NET Web Forms applications.</span></span>

<span data-ttu-id="8e50f-227">추가 웹 개발 모범 사례에 대 한 다음 리소스를 참조 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-227">For additional web development best practices, see the following resources:</span></span>

- <span data-ttu-id="8e50f-228">[수정이 샘플 응용 프로그램-모범 사례](the-fix-it-sample-application.md#bestpractices)합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-228">[The Fix It Sample Application - Best Practices](the-fix-it-sample-application.md#bestpractices).</span></span> <span data-ttu-id="8e50f-229">이 전자책에 대 한 부록 Fix It 응용 프로그램에서 구현 된 모범 사례의 수를 나열 합니다.</span><span class="sxs-lookup"><span data-stu-id="8e50f-229">The appendix to this e-book lists a number of best practices that were implemented in the Fix It application.</span></span>
- [<span data-ttu-id="8e50f-230">웹 개발자 검사 목록</span><span class="sxs-lookup"><span data-stu-id="8e50f-230">Web Developer Checklist</span></span>](http://webdevchecklist.com/asp.net)

> [!div class="step-by-step"]
> <span data-ttu-id="8e50f-231">[이전](continuous-integration-and-continuous-delivery.md)
> [다음](single-sign-on.md)</span><span class="sxs-lookup"><span data-stu-id="8e50f-231">[Previous](continuous-integration-and-continuous-delivery.md)
[Next](single-sign-on.md)</span></span>