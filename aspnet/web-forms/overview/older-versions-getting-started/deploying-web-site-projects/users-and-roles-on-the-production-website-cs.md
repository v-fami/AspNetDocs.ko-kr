---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs
title: 사용자 및 역할 프로덕션 웹 사이트 (C#) | Microsoft Docs
author: rick-anderson
description: ASP.NET 웹 사이트 관리 도구 (WSAT) 멤버 자격 및 역할 설정을 구성 하 고 만들기를 위한 웹 기반 사용자 인터페이스를 제공 편집을 하는 중...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: dbc54313-5d05-4285-98b3-726edea6d0c9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs
msc.type: authoredcontent
ms.openlocfilehash: f08afe5f4ab379d1532f50267299892829c95dcc
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57048850"
---
<a name="users-and-roles-on-the-production-website-c"></a><span data-ttu-id="c9328-103">프로덕션 웹 사이트 (C#)의 사용자 및 역할</span><span class="sxs-lookup"><span data-stu-id="c9328-103">Users and Roles On The Production Website (C#)</span></span>
====================
<span data-ttu-id="c9328-104">[Scott Mitchell](https://twitter.com/ScottOnWriting)</span><span class="sxs-lookup"><span data-stu-id="c9328-104">by [Scott Mitchell](https://twitter.com/ScottOnWriting)</span></span>

[<span data-ttu-id="c9328-105">PDF 다운로드</span><span class="sxs-lookup"><span data-stu-id="c9328-105">Download PDF</span></span>](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial16_CustomAWAT_cs.pdf)

> <span data-ttu-id="c9328-106">ASP.NET 웹 사이트 관리 도구 (WSAT) 및 만들기, 편집 및 삭제 사용자 및 역할 멤버 자격 및 역할 설정을 구성 하기 위한 웹 기반 사용자 인터페이스를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-106">The ASP.NET Website Administration Tool (WSAT) provides a web-based user interface for configuring Membership and Roles settings and for creating, editing, and deleting users and roles.</span></span> <span data-ttu-id="c9328-107">아쉽게도 WSAT 에서만 작동 localhost에서 방문할 때 브라우저를 통해 프로덕션 웹 사이트의 관리 도구에 연결할 수 없는 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-107">Unfortunately, the WSAT only works when visited from localhost, meaning that you cannot reach the production website's Administration Tool through your browser.</span></span> <span data-ttu-id="c9328-108">좋은 소식은 프로덕션의 사용자 및 역할을 관리할 수 있도록 하는 해결 방법이 있는지 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-108">The good news is that there are workarounds that make it possible to manage users and roles on production.</span></span> <span data-ttu-id="c9328-109">이 자습서는 등과 같은 해결이 방법을 살펴봅니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-109">This tutorial looks at these workarounds and others.</span></span>


## <a name="introduction"></a><span data-ttu-id="c9328-110">소개</span><span class="sxs-lookup"><span data-stu-id="c9328-110">Introduction</span></span>

<span data-ttu-id="c9328-111">다양 한 ASP.NET 2.0에 도입 되었습니다 *응용 프로그램 서비스*에 웹 응용 프로그램에 추가할 수 있는 구성 요소 서비스 제품군입니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-111">ASP.NET 2.0 introduced a number of *application services*, which are a suite of building block services that you can add to your web application.</span></span> <span data-ttu-id="c9328-112">도 서 리뷰 웹 사이트에 멤버 자격 및 역할 서비스를 다시 추가한 합니다 [ *서비스를 구성 하는 웹 사이트를 사용 하 여 응용 프로그램* 자습서](configuring-a-website-that-uses-application-services-cs.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-112">We added the Membership and Roles services to the Book Reviews website back in the [*Configuring a Website That Uses Application Services* tutorial](configuring-a-website-that-uses-application-services-cs.md).</span></span> <span data-ttu-id="c9328-113">멤버 자격 서비스 사용자 계정 만들기 및 관리를 용이 하 게 역할 서비스는 사용자 그룹으로 분류에 대 한 API를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-113">The Membership service facilitates creating and managing user accounts; the Roles service offers an API for categorizing users into groups.</span></span> <span data-ttu-id="c9328-114">도 서 리뷰 사이트에 세 개의 사용자 계정이-Scott, Jisun, 고 Alice-Scott과 Jisun 관리자 역할의 관리자가 단일 역할을 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-114">The Book Reviews site has three user accounts - Scott, Jisun, and Alice - and a single role, Admin, with Scott and Jisun in the Admin role.</span></span>

<span data-ttu-id="c9328-115">ASP 합니다. NET의 응용 프로그램 서비스는 특정 구현에 연결 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-115">ASP.NET's application services are not tied to a specific implementation.</span></span> <span data-ttu-id="c9328-116">특정 사용 하도록 응용 프로그램 서비스를 지시 하는 대신 *공급자*, 및 해당 공급자는 특정 기술을 사용 하 여 서비스를 구현 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-116">Instead, you instruct the application services to use a particular *provider*, and that provider implements the service using a particular technology.</span></span> <span data-ttu-id="c9328-117">사용 하 여도 서 리뷰 웹 응용 프로그램을 구성 했습니다 합니다 `SqlMembershipProvider` 및 `SqlRoleProvider` 멤버 자격 및 역할 서비스에 대 한 공급자입니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-117">We configured the Book Reviews web application to use the `SqlMembershipProvider` and `SqlRoleProvider` providers for the Membership and Roles services.</span></span> <span data-ttu-id="c9328-118">이러한 두 공급자는 SQL Server 데이터베이스에서 사용자 계정 및 역할 정보를 저장 하 고 웹 호스팅 회사에 호스팅된 인터넷 기반 웹 응용 프로그램에 대 한 가장 자주 사용 되는 공급자.</span><span class="sxs-lookup"><span data-stu-id="c9328-118">These two providers store user account and role information in a SQL Server database and are the most commonly used providers for Internet-based web applications hosted at a web hosting company.</span></span>

<span data-ttu-id="c9328-119">사용자 및 프로덕션 환경에서 역할 멤버 자격 및 역할 서비스를 사용 하 여 개발자를 위한 일반적인 문제를 관리 하는 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-119">A common challenge for developers using the Membership and Roles services is managing the users and roles on the production environment.</span></span> <span data-ttu-id="c9328-120">프로덕션 웹 사이트에서 사용자 계정 삭제, 새 역할을 추가 하거나 어떻게 기존 역할에 기존 사용자를 추가?</span><span class="sxs-lookup"><span data-stu-id="c9328-120">How do you delete a user account from the production website, add a new role, or add an existing user to an existing role?</span></span> <span data-ttu-id="c9328-121">이 자습서에서는 프로덕션 웹 사이트의 사용자 및 역할을 관리 하기 위한 다양 한 기법을 살펴봅니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-121">This tutorial explores different techniques for managing users and roles on the production website.</span></span>

## <a name="using-the-aspnet-web-site-administration-tool"></a><span data-ttu-id="c9328-122">ASP.NET 웹 사이트 관리 도구를 사용 하 여</span><span class="sxs-lookup"><span data-stu-id="c9328-122">Using the ASP.NET Web Site Administration Tool</span></span>

<span data-ttu-id="c9328-123">ASP.NET에 포함 되어는 [웹 사이트 관리 도구](https://msdn.microsoft.com/library/yy40ytx0.aspx) (WSAT)는 손쉽게 사용자 계정 및 역할 만들기 및 관리 하 고 사용자 및 역할 기반 권한 부여 규칙을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-123">ASP.NET includes a [Web Site Administration Tool](https://msdn.microsoft.com/library/yy40ytx0.aspx) (WSAT) that makes it easy to create and manage user accounts and roles and to specify user- and role-based authorization rules.</span></span> <span data-ttu-id="c9328-124">WSAT는를 사용 하려면 솔루션 탐색기에서 ASP.NET 구성 아이콘 또는 메뉴로 이동 하 여 웹 사이트 또는 프로젝트 및 ASP.NET 구성 옵션을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-124">To use the WSAT, click the ASP.NET Configuration icon in the Solution Explorer, or go to the Website or Project menu and choose the ASP.NET Configuration option.</span></span> <span data-ttu-id="c9328-125">어느 방법이 든 웹 브라우저를 시작 하 고과 같은 주소에서 WSAT 가리키도록: `http://localhost:portNumber/asp.netwebadminfiles/default.aspx?applicationPhysicalPath=pathToApplication`</span><span class="sxs-lookup"><span data-stu-id="c9328-125">Either approach launches a web browser and points it to the WSAT at an address like: `http://localhost:portNumber/asp.netwebadminfiles/default.aspx?applicationPhysicalPath=pathToApplication`</span></span>

<span data-ttu-id="c9328-126">WSAT는 세 가지 섹션으로 구분 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-126">The WSAT is divided into three sections:</span></span>

- <span data-ttu-id="c9328-127">**보안** -사용자, 역할 및 권한 부여 규칙을 관리 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-127">**Security** - manage users, roles, and authorization rules.</span></span>
- <span data-ttu-id="c9328-128">**ApplicationConfiguration** -관리 합니다 &lt;appSettings&gt; 및 여기에서 SMTP 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-128">**ApplicationConfiguration** - manage the &lt;appSettings&gt; and SMTP settings from here.</span></span> <span data-ttu-id="c9328-129">있습니다 수도 오프 라인으로 응용 프로그램 및 디버깅 및 추적 여기에서 설정 관리 뿐만 기본 사용자 지정 오류 페이지를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-129">You can also take the application offline and manage debugging and tracing settings from here, as well as specify the default custom error page.</span></span>
- <span data-ttu-id="c9328-130">**ProviderConfiguration** -응용 프로그램 서비스에서 사용 하는 공급자를 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-130">**ProviderConfiguration** - configure the providers used by the application services.</span></span>

<span data-ttu-id="c9328-131">보안 섹션 (에서처럼 **그림 1**) 새 사용자를 만들기, 사용자 관리, 만들기 및 역할을 관리 하 고 만들기 및 관리에 대 한 액세스 규칙에 대 한 링크가 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-131">The Security section (shown in **Figure 1**) includes links for creating new users, managing users, creating and managing roles, and creating and managing access rules.</span></span> <span data-ttu-id="c9328-132">여기에서 수 시스템에 새 역할을 추가, 기존 사용자를 삭제 또는 추가 또는 특정 사용자 계정에서 역할을 제거 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-132">From here you can add a new role to the system, delete an existing user, or add or remove roles from a particular user account.</span></span>

[![](users-and-roles-on-the-production-website-cs/_static/image2.png)](users-and-roles-on-the-production-website-cs/_static/image1.png)

<span data-ttu-id="c9328-133">**그림 1**: WSAT 보안 섹션에는 관리 사용자 및 역할에 대 한 옵션이 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-133">**Figure 1**: The WSAT Security Section Includes Options for Managing Users and Roles</span></span>  
<span data-ttu-id="c9328-134">([클릭 하 여 큰 이미지 보기](users-and-roles-on-the-production-website-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c9328-134">([Click to view full-size image](users-and-roles-on-the-production-website-cs/_static/image3.png))</span></span>

<span data-ttu-id="c9328-135">아쉽게도 WSAT는만 액세스할 수 있는 로컬.</span><span class="sxs-lookup"><span data-stu-id="c9328-135">Unfortunately, the WSAT is only accessible locally.</span></span> <span data-ttu-id="c9328-136">원격 프로덕션 웹 사이트;에 WSAT를 방문할 수 없습니다. 방문 하는 경우 `www.yoursite.com/asp.netwebadminfiles/default.aspx` 404 찾을 수 없음 응답을 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-136">You cannot visit the WSAT on your remote production website; if you visit `www.yoursite.com/asp.netwebadminfiles/default.aspx` you get a 404 Not Found response.</span></span> <span data-ttu-id="c9328-137">WSAT 사용을 구동 하는 코드를 `Membership` 고 `Roles` 를 만들려면.NET Framework의 클래스를 편집 하 고 사용자 및 역할을 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-137">The code that powers the WSAT uses the `Membership` and `Roles` classes in the .NET Framework to create, edit, and delete users and roles.</span></span> <span data-ttu-id="c9328-138">이러한 클래스 사용에 어떤 공급자를 확인 하려면 웹 응용 프로그램의 구성 정보를 참조 하세요. 다시 합니다 [ *서비스를 구성 하는 웹 사이트를 사용 하 여 응용 프로그램* 자습서](configuring-a-website-that-uses-application-services-cs.md) 도 서 리뷰 웹 사이트를 사용 하 여 설정 합니다 `SqlMembershipProvider` 및 `SqlRoleProvider` 공급자.</span><span class="sxs-lookup"><span data-stu-id="c9328-138">These classes consult the web application's configuration information to determine what provider to use; back in the [*Configuring a Website That Uses Application Services* tutorial](configuring-a-website-that-uses-application-services-cs.md) we setup the Book Reviews website to use the `SqlMembershipProvider` and `SqlRoleProvider` providers.</span></span> <span data-ttu-id="c9328-139">추가이 뛰어나며 `<membership>` 하 고 `<roleManager>` 섹션을 `Web.config`입니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-139">This entailed adding `<membership>` and `<roleManager>` sections to `Web.config`.</span></span>

[!code-xml[Main](users-and-roles-on-the-production-website-cs/samples/sample1.xml)]

<span data-ttu-id="c9328-140">`<membership>` 및 `<roleManager>` 참조 섹션의 `SqlMembershipProvider` 및 `SqlRoleProvider` 의 공급자가 `type` 특성을 각각.</span><span class="sxs-lookup"><span data-stu-id="c9328-140">Note that the `<membership>` and `<roleManager>` sections reference the `SqlMembershipProvider` and `SqlRoleProvider` providers in their `type` attribute, respectively.</span></span> <span data-ttu-id="c9328-141">이러한 공급자는 지정된 된 SQL Server 데이터베이스에서 사용자 및 역할 정보를 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-141">These providers store the user and role information in a specified SQL Server database.</span></span> <span data-ttu-id="c9328-142">이러한 공급자에서 데이터베이스를 사용 하 여 지정 합니다 `connectionStringName` 특성인 `ReviewsConnectionString`에 정의 되어 있는 `~/ConfigSections/databaseConnectionStrings.config` 파일.</span><span class="sxs-lookup"><span data-stu-id="c9328-142">The database used by these providers is specified by the `connectionStringName` attribute, `ReviewsConnectionString`, which is defined in the `~/ConfigSections/databaseConnectionStrings.config` file.</span></span> <span data-ttu-id="c9328-143">이전에 설명한 대로 합니다 `databaseConnectionStrings.config` 반면 파일 개발 환경에서 개발 데이터베이스에 연결 문자열을 포함 합니다 `databaseConnectionStrings.config` 프로덕션 데이터베이스에 연결 문자열을 포함 하는 프로덕션에서 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-143">Recall that the `databaseConnectionStrings.config` file in the development environment contains the connection string to the development database whereas the `databaseConnectionStrings.config` file on production contains the connection string to the production database.</span></span>

<span data-ttu-id="c9328-144">간단히 말해 WSAT는 개발 환경을 통해 로컬로 액세스할 수 있어야 합니다 및 지정 된 데이터베이스의 사용자 및 역할 정보를 사용 하 여 작동 합니다 `databaseConnectionStrings.config` 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-144">In a nutshell, the WSAT must be accessed locally through the development environment, and it works with the user and role information in the database specified in the `databaseConnectionStrings.config` file.</span></span> <span data-ttu-id="c9328-145">따라서 연결 문자열 정보를 변경 하는 경우는 `databaseConnectionStrings.config` 파일 개발 환경에서 사용할 수 있습니다는 WSAT 로컬 사용자 및 프로덕션 환경에서 역할 관리 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-145">Consequently, if we change the connection string information in the `databaseConnectionStrings.config` file on the development environment we can use the WSAT locally to manage users and roles in the production environment.</span></span>

<span data-ttu-id="c9328-146">이 기능을 설명 하기 위해 엽니다는 `databaseConnectionStrings.config` 개발 환경에서 Visual Studio에서 파일 및 프로덕션 데이터베이스 연결 문자열을 사용 하 여 개발 하는 데이터베이스 연결 문자열을 대체 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-146">To illustrate this functionality, open the `databaseConnectionStrings.config` file in Visual Studio on the development environment and replace the development database connection string with the production database connection string.</span></span> <span data-ttu-id="c9328-147">WSAT는 시작, 보안 탭 이동 및 Sam 암호 "password!" 라는 새 사용자 추가</span><span class="sxs-lookup"><span data-stu-id="c9328-147">Then launch the WSAT, go the Security tab, and add a new user named Sam with password "password!"</span></span> <span data-ttu-id="c9328-148">(작은 따옴표는 표시) 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-148">(less the quotation marks).</span></span> <span data-ttu-id="c9328-149">**그림 2** 이 계정을 만들 때 WSAT 화면을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-149">**Figure 2** shows the WSAT screen when creating this account.</span></span>

[![](users-and-roles-on-the-production-website-cs/_static/image5.png)](users-and-roles-on-the-production-website-cs/_static/image4.png)

<span data-ttu-id="c9328-150">**그림 2**: 프로덕션 환경에서 Sam 라는 새 사용자 만들기</span><span class="sxs-lookup"><span data-stu-id="c9328-150">**Figure 2**: Create a New User Named Sam In the Production Environment</span></span>  
<span data-ttu-id="c9328-151">([클릭 하 여 큰 이미지 보기](users-and-roles-on-the-production-website-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="c9328-151">([Click to view full-size image](users-and-roles-on-the-production-website-cs/_static/image6.png))</span></span>

<span data-ttu-id="c9328-152">연결 문자열을 변경 했습니다 때문에 `databaseConnectionStrings.config` 프로덕션 데이터베이스 서버를 가리키도록 Sam은 프로덕션 환경에서 사용자로 추가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-152">Because we changed the connection string in `databaseConnectionStrings.config` to point to the production database server, Sam was added as a user in the production environment.</span></span> <span data-ttu-id="c9328-153">이 확인 하려면에서 연결 문자열을 변경 합니다 `databaseConnectionStrings.config` 개발 데이터베이스에 파일을 살펴본는 `Login.aspx` 개발 환경에서 페이지.</span><span class="sxs-lookup"><span data-stu-id="c9328-153">To verify this, change the connection string in the `databaseConnectionStrings.config` file back to the development database and then visit the `Login.aspx` page in the development environment.</span></span> <span data-ttu-id="c9328-154">Sam로 로그인 하려고 (참조 **그림 3**).</span><span class="sxs-lookup"><span data-stu-id="c9328-154">Try to sign in as Sam (see **Figure 3**).</span></span>

[![](users-and-roles-on-the-production-website-cs/_static/image8.png)](users-and-roles-on-the-production-website-cs/_static/image7.png)

<span data-ttu-id="c9328-155">**그림 3**: 개발 환경에서 Sam으로 로그인 할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-155">**Figure 3**: You Cannot Sign In As Sam in the Development Environment</span></span>  
<span data-ttu-id="c9328-156">([클릭 하 여 큰 이미지 보기](users-and-roles-on-the-production-website-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="c9328-156">([Click to view full-size image](users-and-roles-on-the-production-website-cs/_static/image9.png))</span></span>

<span data-ttu-id="c9328-157">로그인 할 수 없습니다 Sam으로 개발 환경에서 사용자 계정 정보를 로컬 데이터베이스에 없기 때문에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-157">You cannot sign in as Sam in the development environment because the user account information does not exist in the local database.</span></span> <span data-ttu-id="c9328-158">보다는 프로덕션 데이터베이스에 추가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-158">Rather, is was added to the production database.</span></span> <span data-ttu-id="c9328-159">이 확인 하려면의 내용 보기는 `aspnet_Users` 개발 및 프로덕션 데이터베이스의 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-159">To verify this, view the contents of the `aspnet_Users` table in both the development and production databases.</span></span> <span data-ttu-id="c9328-160">개발 환경에서에 사용자 Scott, Jisun, Alice에 대 한 세 개의 레코드 없어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-160">In the development environment there should be only three records for users Scott, Jisun, and Alice.</span></span> <span data-ttu-id="c9328-161">그러나는 `aspnet_Users` 프로덕션 데이터베이스의 테이블에 4 개의 레코드: Scott, Jisun, Alice 및 Sam입니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-161">However, the `aspnet_Users` table in the production database has four records: Scott, Jisun, Alice, and Sam.</span></span> <span data-ttu-id="c9328-162">따라서 개발 환경 통해서가 아니라 프로덕션 환경에서 웹 사이트를 통해 Sam 서명할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-162">Consequently, Sam can sign in through the website in production, but not through the development environment.</span></span>

[![](users-and-roles-on-the-production-website-cs/_static/image11.png)](users-and-roles-on-the-production-website-cs/_static/image10.png)

<span data-ttu-id="c9328-163">**그림 4**: Sam은 프로덕션 웹 사이트에 로그인 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-163">**Figure 4**: Sam Can Sign In On the Production Website</span></span>  
<span data-ttu-id="c9328-164">([클릭 하 여 큰 이미지 보기](users-and-roles-on-the-production-website-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="c9328-164">([Click to view full-size image](users-and-roles-on-the-production-website-cs/_static/image12.png))</span></span>

> [!NOTE]
> <span data-ttu-id="c9328-165">연결 문자열을 변경 해야 합니다 `databaseConnectionStrings.config` 완료 되 면 다시 개발 데이터베이스에 파일의 연결 문자열 작업할 프로덕션 데이터를 사용 하 여 개발을 통해 사이트를 테스트할 때이 고 그렇지 WSAT 사용 환경입니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-165">Don't forget to change the connection string in the `databaseConnectionStrings.config` file back to the development database's connect string when you're done working with the WSAT otherwise you will be working with production data when testing the site through the development environment.</span></span> <span data-ttu-id="c9328-166">또한 염두에 앞에서 설명한 기법을 사용 하면 WSAT는 사용자 및 역할을 원격으로 관리 하는 데, 하는 동안 다른 WSAT 구성 옵션 (액세스 규칙, SMTP 설정, 디버깅 및 추적 설정 및 등) 중 하나에 대 한 변경 내용을 수정 합니다 `Web.config` 파일입니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-166">Also keep in mind that while the technique we just discussed allows us to use the WSAT to remotely manage users and roles, changes to any of the other WSAT configuration options (access rules, SMTP settings, debugging and tracing settings, and so on) modify the `Web.config` file.</span></span> <span data-ttu-id="c9328-167">따라서 설정을 변경에는 프로덕션 환경이 아닌 개발 환경에 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-167">Consequently, any changes made to the settings apply to the development environment and not to the production environment.</span></span>


## <a name="creating-custom-user-and-role-management-web-pages"></a><span data-ttu-id="c9328-168">사용자 및 역할 관리 웹 페이지 만들기</span><span class="sxs-lookup"><span data-stu-id="c9328-168">Creating Custom User and Role Management Web Pages</span></span>

<span data-ttu-id="c9328-169">WSAT는 사용자 및 역할을 관리 하기 위한 상자 시스템의 부족을 제공 하지만 로컬로 실행할 수 있습니다 하며 프로덕션에서 역할과 사용자를 관리 하기 위해 연결 문자열 정보를 변경 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-169">The WSAT provides an out of the box system for managing users and roles, but can only be launched locally and requires making changes to the connection string information in order to manage the users and roles on production.</span></span> <span data-ttu-id="c9328-170">사용자 계정을 지 원하는 대부분의 웹 사이트에는 다양 한 사용자 및 역할 관리 웹 페이지를 통해 사이트 내의 페이지에서 사용자 및 역할을 관리 하려면 관리자가 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-170">Most websites that support user accounts also include a number of user and role administration web pages that enable administrators to manage users and roles from pages within the site.</span></span> <span data-ttu-id="c9328-171">이러한 웹 기반 관리 페이지 훨씬 쉽게 사용자 및 역할을 관리 하 고 사이트에 필수적인 경우 많은 관리자 또는 관리자에 대 한 액세스 또는 Visual Studio는 WSAT를 시작 하는 데 기술적 배경 없는 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-171">Such web-based administration pages make it much easier to manage users and roles and are essential for sites where there may be many administrators or administrators that do not have access to or the technical background to use Visual Studio to launch the WSAT.</span></span>

<span data-ttu-id="c9328-172">ASP.NET의 다양 한 끌어서 놓기 에서처럼 쉽지 이러한 관리 웹 페이지를 구현 하는 기본 제공 웹 로그인 관련 컨트롤 번호를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-172">ASP.NET includes a number of built-in Login-related Web controls that make implementing many of these administrative web pages as easy as drag and drop.</span></span> <span data-ttu-id="c9328-173">예를 들어, CreateUserWizard 컨트롤을 페이지로 끌어서 일부 속성을 설정 하 여 새 사용자 계정을 만들려면 관리자에 대 한 페이지를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-173">For example, you can create a page for administrators to create a new user account by dragging the CreateUserWizard control onto the page and setting a few properties.</span></span> <span data-ttu-id="c9328-174">사실 에서처럼 WSAT에서 사용자를 만들기 위한 페이지 **그림 2** 페이지에 추가할 수 있는 동일한 CreateUserWizard 컨트롤을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-174">In fact, the page for creating users in the WSAT shown in **Figure 2** uses the same CreateUserWizard control that you can add to your pages.</span></span> <span data-ttu-id="c9328-175">또한 멤버 자격 및 역할 서비스의 기능을 통해 프로그래밍 방식으로 사용할 수는 `Membership` 및 `Roles` .NET Framework의 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-175">Furthermore, the Membership and Roles services' functionalities are available programmatically through the `Membership` and `Roles` classes in the .NET Framework.</span></span> <span data-ttu-id="c9328-176">이러한 클래스를 사용 하 여 만들기, 편집 및 삭제 사용자 및 역할에도 역할에 사용자 추가 또는 제거에 대 한 어떤 역할에 사용자를 확인 하 고 다른 사용자 및 역할에 관련 된 작업을 수행 하는 코드를 작성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-176">With these classes you can write code to create, edit, and delete users and roles, as well as to add or remove users to roles, to determine what users are in what roles, and to perform other user- and role-related tasks.</span></span>

<span data-ttu-id="c9328-177">에 [ *서비스를 구성 하는 웹 사이트를 사용 하 여 응용 프로그램* 자습서](configuring-a-website-that-uses-application-services-cs.md) 페이지를 추가 합니다 `Admin` 라는 폴더 `CreateAccount.aspx`합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-177">In the [*Configuring a Website That Uses Application Services* tutorial](configuring-a-website-that-uses-application-services-cs.md) I added a page to the `Admin` folder named `CreateAccount.aspx`.</span></span> <span data-ttu-id="c9328-178">이 페이지에는 관리자가 사이트에 새 사용자 계정을 추가 하 고 새로 만든된 사용자가 관리자 역할에 있는지 여부를 지정할 수 있습니다 (참조 **그림 5**).</span><span class="sxs-lookup"><span data-stu-id="c9328-178">This page allows an administrator to add a new user account to the site and to specify whether or not the newly created user is in the Admin role (see **Figure 5**).</span></span>

[![](users-and-roles-on-the-production-website-cs/_static/image14.png)](users-and-roles-on-the-production-website-cs/_static/image13.png)

<span data-ttu-id="c9328-179">**그림 5**: 관리자는 새 사용자 계정을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-179">**Figure 5**: Administrators Can Create New User Accounts</span></span>  
<span data-ttu-id="c9328-180">([클릭 하 여 큰 이미지 보기](users-and-roles-on-the-production-website-cs/_static/image15.png))</span><span class="sxs-lookup"><span data-stu-id="c9328-180">([Click to view full-size image](users-and-roles-on-the-production-website-cs/_static/image15.png))</span></span>

<span data-ttu-id="c9328-181">에 대 한 자세한 사용에 대 한 단계별 지침과 함께 사용자 및 역할 관리 페이지를 구축 합니다 `Membership` 및 `Roles` 클래스 및 ASP.NET 웹 로그인 관련 컨트롤을 참조 해야 내 [웹 사이트 보안 자습서](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-181">For a more detailed look at building user and role administration pages, along with step-by-step instructions on using the `Membership` and `Roles` classes and the Login-related ASP.NET Web controls, be sure to read my [Website Security Tutorials](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).</span></span> <span data-ttu-id="c9328-182">찾을 수 있습니다 지침 새 계정 만들기, 역할 만들기 및 관리를 위한 웹 페이지를 구축 하는 방법에 역할 및 다른 일반적인 관리 작업에 사용자를 할당 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-182">There you'll find guidance on how to build web pages for creating new accounts, creating and managing roles, assigning users to roles, and other common administrative tasks.</span></span>

<span data-ttu-id="c9328-183">프로덕션 웹 사이트의 WSAT 유사한 기능을 구현 하에 항상 고유한 일련의 WSAT의 기능을 구현 하는 웹 페이지를 빌드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-183">To implement WSAT-like functionality on the production website you can always build your own series of web pages that implement the WSAT's features.</span></span> <span data-ttu-id="c9328-184">시작, 폴더에 위치한 WSAT 소스 코드를 체크 아웃 하는 데 `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\ASP.NETWebAdminFiles`합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-184">To help get started, check out the WSAT source code, which is located in the folder `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\ASP.NETWebAdminFiles`.</span></span> <span data-ttu-id="c9328-185">Dan Clem WSAT 대신이 문서의 공유 그는 사용 하는 다른 옵션도 [롤링 사용자 고유의 웹 사이트 관리 도구](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-185">Another option is to use Dan Clem's WSAT alternative, which he shares in his article, [Rolling Your Own Web Site Administration Tool](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx).</span></span> <span data-ttu-id="c9328-186">Dan은 판독기 사용자 지정 WSAT 같은 도구를 빌드하는 과정을 안내 다운로드 (C#), 응용 프로그램의 소스 코드를 포함 하며 자신의 사용자 지정 WSAT 호스팅되는 웹 사이트를 추가 하기 위한 단계별 지침을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-186">Dan walks readers through the process of building a custom WSAT-like tool, includes his application's source code for download (in C#), and gives step-by-step instructions for adding his custom WSAT to a hosted website.</span></span>

## <a name="summary"></a><span data-ttu-id="c9328-187">요약</span><span class="sxs-lookup"><span data-stu-id="c9328-187">Summary</span></span>

<span data-ttu-id="c9328-188">ASP.NET 웹 사이트 관리 도구 (WSAT) 웹 사이트에 대 한 사용자 및 역할 정보를 관리 하는 멤버 자격 및 역할 응용 프로그램 서비스를 사용 하 여 동시에 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-188">The ASP.NET Web Site Administration Tool (WSAT) can be used in tandem with the Membership and Roles application services to manage user and role information for your website.</span></span> <span data-ttu-id="c9328-189">아쉽게도 WSAT만 액세스할 수 있는 로컬 이며 프로덕션 웹 사이트를 방문할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-189">Unfortunately, the WSAT is only accessible locally and cannot be visited from your production website.</span></span> <span data-ttu-id="c9328-190">그러나 개발에서 연결 문자열을 변경 하 여 프로덕션 데이터베이스를 지점 환경 사용할 수는 WSAT 프로덕션 웹 사이트의 역할과 사용자를 관리할 수 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-190">However, by changing the connection string in the development environment to point to the production database you can use the WSAT to manage the users and roles on the production website.</span></span>

<span data-ttu-id="c9328-191">WSAT 접근 방식은 사용자 및 역할을 관리 하는 쉽고 빠르게 제공, 하는 동안 연결 문자열 정보를 임시 변경 뿐만 아니라 Visual Studio에서 WSAT 시작 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-191">While the WSAT approach affords a quick and easy way to manage users and roles, it necessitates launching the WSAT from Visual Studio as well as temporary changes to the connection string information.</span></span> <span data-ttu-id="c9328-192">WSAT는 작업은 번거롭습니다 있고 없거나 Visual Studio와는 WSAT에 익숙하지 않은 관리자 또는 여러 관리자를 사용 하 여 웹 사이트에 대해 제대로 작동 하지 않습니다 프로덕션의 사용자 및 역할을 관리 하는 빠른 방법을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-192">The WSAT offers a quick way to manage users and roles on production, but is cumbersome and does not work well for websites with multiple administrators or with administrators who do not have or are not familiar with Visual Studio and the WSAT.</span></span> <span data-ttu-id="c9328-193">이러한 이유로, 사용자 계정을 지 원하는 대부분의 웹 사이트 관리 웹 페이지의 집합을 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-193">For these reasons, most websites that support user accounts include a set of administrative web pages.</span></span> <span data-ttu-id="c9328-194">WSAT는 필요 하지 않습니다 하 고 모든 컴퓨터에서 다양 한 관리자가 사용 하는 웹 페이지의 이러한 집합.</span><span class="sxs-lookup"><span data-stu-id="c9328-194">Such a set of web pages eliminates the need for the WSAT and used by various administrative users from any computer.</span></span>

<span data-ttu-id="c9328-195">즐거운 프로그래밍!</span><span class="sxs-lookup"><span data-stu-id="c9328-195">Happy Programming!</span></span>

### <a name="further-reading"></a><span data-ttu-id="c9328-196">추가 정보</span><span class="sxs-lookup"><span data-stu-id="c9328-196">Further Reading</span></span>

<span data-ttu-id="c9328-197">이 자습서에서 다루는 항목에 대 한 자세한 내용은 다음 리소스를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="c9328-197">For more information on the topics discussed in this tutorial, refer to the following resources:</span></span>

- [<span data-ttu-id="c9328-198">ASP를 검사 합니다. NET의 멤버 자격, 역할 및 프로필</span><span class="sxs-lookup"><span data-stu-id="c9328-198">Examining ASP.NET's Membership, Roles, and Profile</span></span>](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [<span data-ttu-id="c9328-199">고유한 웹 사이트 관리 도구를 롤링합니다.</span><span class="sxs-lookup"><span data-stu-id="c9328-199">Rolling Your Own Web Site Administration Tool</span></span>](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [<span data-ttu-id="c9328-200">웹 사이트 관리 도구 개요</span><span class="sxs-lookup"><span data-stu-id="c9328-200">Web Site Administration Tool Overview</span></span>](https://msdn.microsoft.com/library/yy40ytx0.aspx)
- [<span data-ttu-id="c9328-201">웹 사이트 보안 자습서</span><span class="sxs-lookup"><span data-stu-id="c9328-201">Website Security Tutorials</span></span>](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> <span data-ttu-id="c9328-202">[이전](precompiling-your-website-cs.md)
> [다음](asp-net-hosting-options-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c9328-202">[Previous](precompiling-your-website-cs.md)
[Next](asp-net-hosting-options-vb.md)</span></span>