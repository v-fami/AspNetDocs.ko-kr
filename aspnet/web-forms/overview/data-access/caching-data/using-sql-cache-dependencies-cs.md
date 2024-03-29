---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-cs
title: SQL 캐시 종속성 (C#)를 사용 하 여 | Microsoft Docs
author: rick-anderson
description: 가장 간단한 캐싱 전략 지정 된 기간 후에 만료 되도록 캐시 된 데이터를 허용 하는 것입니다. 하지만 이렇게 간단한 방식으로 캐시 된 데이터 maintai 의미 하는 중...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 0e91842c-7f10-4aed-8c23-4ee3e2774014
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-cs
msc.type: authoredcontent
ms.openlocfilehash: b6bc905abbe3b875b0cbe839090e43dae8f491a7
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65116913"
---
# <a name="using-sql-cache-dependencies-c"></a>SQL 캐시 종속성 사용(C#)

[Scott Mitchell](https://twitter.com/ScottOnWriting)

[코드를 다운로드](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_CS.zip) 또는 [PDF 다운로드](using-sql-cache-dependencies-cs/_static/datatutorial61cs1.pdf)

> 가장 간단한 캐싱 전략 지정 된 기간 후에 만료 되도록 캐시 된 데이터를 허용 하는 것입니다. 하지만 이렇게 간단한 방식으로 캐시 된 데이터에 너무 오래 보관 된 오래 된 데이터 또는 현재 데이터를 너무 일찍 만료 된 해당 기본 데이터 원본에 연관 시킬 의도가 없으며 유지 함을 의미 합니다. 데이터의 해당 기본 데이터 SQL database에서 수정 될 때까지 들이 캐시 된 상태로 남아 있도록 SqlCacheDependency 클래스를 사용 하는 것이 좋습니다. 이 자습서 방법을 보여 줍니다.

## <a name="introduction"></a>소개

캐싱 기술을 검사 합니다 [ObjectDataSource 사용 하 여 데이터 캐싱](caching-data-with-the-objectdatasource-cs.md) 하 고 [아키텍처에서 데이터 캐싱](caching-data-in-the-architecture-cs.md) 자습서는 시간 기반 만료 후 지정 된 캐시에서 데이터를 제거 하는 데 사용 기간입니다. 이 방법은 분산 된 부실 데이터에 대 한 캐싱 성능 향상에 대 한 가장 간단한 방법에 설명 합니다. 시간 만료를 선택 하 여 *x* 시간 (초), 페이지 개발자 concedes에 대 한 캐싱 전용의 성능 혜택을 누려 보십시오 *x* 초, 하지만 신경는 자신의 데이터 되지 부실 최대 이상 *x* 시간 (초)입니다. 물론, 정적 데이터에 대 한 *x* 에서 검사 된 대로 웹 응용 프로그램의 수명으로 확장할 수 있습니다 합니다 [응용 프로그램 시작 시 데이터 캐싱](caching-data-at-application-startup-cs.md) 자습서입니다.

데이터베이스 데이터를 캐시 하는 경우 시간 기반 만료 사용 편이성에 대 한 자주 선택 이지만 자주 부족 하 여 솔루션을 합니다. 이상적으로 데이터베이스 데이터는 데이터베이스에서 기본 데이터에 수정 될 때까지 캐시 유지 그런 후에 캐시를 제거 합니다. 이 방법은 캐시의 성능을 최대화 하 고 오래 된 데이터의 기간을 최소화 합니다. 그러나 있습니다 이러한 혜택을 얻으려면 하려면 기본 데이터베이스 데이터 수정 되었고 그 캐시에서 해당 항목을 제거 하면 알고 있는 일부 시스템 여야 합니다. ASP.NET 2.0 이전 페이지 개발자가이 시스템을 구현 하는 일을 담당 했습니다.

ASP.NET 2.0은 제공 된 [ `SqlCacheDependency` 클래스](https://msdn.microsoft.com/library/system.web.caching.sqlcachedependency.aspx) 및 캐시 된 항목을 해당 하는 데이터베이스에서 변경 발생 했을 때를 결정 하는 데 필요한 인프라를 제거할 수 있습니다. 기본 데이터가 변경 되 면 확인 하기 위한 두 가지 기술이 있습니다: 알림 및 폴링. 알림 및 폴링의 차이점에 설명 하는 후 만들겠습니다 인프라 폴링을 지원 하 고 다음 사용 하는 방법을 탐색 하는 데 필요한를 `SqlCacheDependency` 클래스 선언적 및 프로그래밍 방식으로 시나리오입니다.

## <a name="understanding-notification-and-polling"></a>이해 알림 및 폴링

경우에 데이터베이스의 데이터 수정 되었는지 확인 하는 두 가지 기술이 있습니다: 알림 및 폴링. 알림을 사용 하 여 쿼리가 마지막으로 실행 된 이후 변경 했을 특정 쿼리 결과를 할 때 자동으로 데이터베이스 ASP.NET 런타임 경고,이 시점에서 쿼리를 사용 하 여 연결 된 캐시 된 항목이 제거 됩니다. 폴링을 사용 하 여 데이터베이스 서버의 경우 특정 테이블이 마지막으로 업데이트 되었습니다에 대 한 정보를 유지 합니다. ASP.NET 런타임은 테이블이 변경 내용을 확인 하려면 데이터베이스를 주기적으로 폴링하여 캐시에 입력 된 순서 때문입니다. 해당 데이터가 수정 된 해당 테이블 제거 관련된 캐시 항목이 해당 합니다.

알림 옵션 폴링 보다 적은 설정이 필요 및 테이블 수준에서가 아니라 쿼리 수준에서 변경 내용을 추적 하므로 더 세부적입니다. 아쉽게도 알림은 Microsoft SQL Server 2005 (예: Express 이외 버전)의 전체 버전에서 사용할 수 있습니다. 그러나 7.0의 Microsoft SQL Server 2005로의 모든 버전에 대 한 폴링 옵션을 사용할 수 있습니다. 이 자습서는 SQL Server 2005 Express edition을 사용 하므로 설정 된 폴링 옵션을 사용 하 여에 살펴볼 것입니다. SQL Server 2005가의 알림 기능에 추가 리소스에 대 한이 자습서의 끝에 추가 정보 섹션을 참조 하세요.

폴링을 사용 하 여 데이터베이스 라는 테이블을 포함 하도록 구성 해야 합니다 `AspNet_SqlCacheTablesForChangeNotification` 세 열에 있는 `tableName`를 `notificationCreated`, 및 `changeId`합니다. 이 테이블에서 웹 응용 프로그램의 SQL 캐시 종속성을 사용할 수 해야 할 수 있는 데이터가 포함 된 각 테이블에 대 한 행을 포함 합니다. 합니다 `tableName` 열을 지정 하는 동안 테이블의 이름을 `notificationCreated` 테이블에 행이 추가 시간과 날짜를 나타냅니다. 합니다 `changeId` 형식의 열이 `int` 있고 초기 값이 0입니다. 테이블에 각 수정 작업을 사용 하 여 해당 값이 증가 합니다.

이외에 `AspNet_SqlCacheTablesForChangeNotification` 테이블 데이터베이스 해야 SQL 캐시 종속성에 나타날 수 있는 테이블의 각 트리거를 포함 합니다. 이러한 트리거 행 삽입, 업데이트 또는 삭제 될 때마다 실행 되 고 s 테이블이 증가 `changeId` 값 `AspNet_SqlCacheTablesForChangeNotification`합니다.

ASP.NET 런타임은 현재 추적 `changeId` 사용 하 여 데이터를 캐시 하는 경우 테이블을 `SqlCacheDependency` 개체입니다. 데이터베이스는 정기적으로 검사 하는 및 `SqlCacheDependency` 갖는 개체 `changeId` 데이터베이스의 값에서 다른는 다른 이후에 제거 됩니다 `changeId` 있었는지 테이블에 대 한 변경 데이터가 캐시 된 이후 값을 나타냅니다.

## <a name="step-1-exploring-theaspnetregsqlexecommand-line-program"></a>1단계: 탐색을`aspnet_regsql.exe`명령줄 프로그램

폴링 접근 방식으로 데이터베이스는 위에서 설명한 인프라를 포함 하는 설치 여야 합니다: 미리 정의 된 테이블 (`AspNet_SqlCacheTablesForChangeNotification`), 적은 수의 저장된 프로시저 및 트리거를 웹에서 SQL 캐시 종속성에 사용할 수 있는 테이블의 각 응용 프로그램입니다. 명령줄 프로그램을 통해 이러한 테이블, 저장된 프로시저 및 트리거를 만들 수 있습니다 `aspnet_regsql.exe`에서 찾을 수 있는 여 `$WINDOWS$\Microsoft.NET\Framework\version` 폴더입니다. 만들려는 `AspNet_SqlCacheTablesForChangeNotification` 테이블과 연결 된 저장된 프로시저를 명령줄에서 다음을 실행 합니다.

[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample1.cmd)]

> [!NOTE]
> 지정 된 데이터베이스 로그인에 있어야 합니다. 이러한 명령을 실행 하는 [ `db_securityadmin` ](https://msdn.microsoft.com/library/ms188685.aspx) 하 고 [ `db_ddladmin` ](https://msdn.microsoft.com/library/ms190667.aspx) 역할입니다. T-SQL을 전송 하 여 데이터베이스를 검사 하는 `aspnet_regsql.exe` 줄 프로그램 명령을 참조 하세요 [이 블로그 항목](http://scottonwriting.net/sowblog/posts/10709.aspx)합니다.

예를 들어 라는 폴링에 대 한 인프라는 Microsoft SQL Server 데이터베이스를 추가 하려면 `pubs` 이라는 데이터베이스 서버에 `ScottsServer` Windows 인증을 사용 하는 적절 한 디렉터리를 이동할를 명령줄에서 다음을 입력 합니다.

[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample2.cmd)]

데이터베이스 수준 인프라에 추가한 후 SQL 캐시 종속성에 사용할 해당 테이블에 트리거를 추가 해야 합니다. 사용 하 여는 `aspnet_regsql.exe` 명령줄 프로그램을 다시 하지만 사용 하 여 테이블 이름을 지정 합니다 `-t` 전환 및 사용 하는 대신를 `-ed` 사용 하 여 전환 `-et`, 다음과 같이:

[!code-html[Main](using-sql-cache-dependencies-cs/samples/sample3.html)]

트리거를 추가 하는 `authors` 및 `titles` 테이블을 `pubs` 데이터베이스에 `ScottsServer`를 사용 하 여:

[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample4.cmd)]

이 자습서에 대 한 트리거를 추가 합니다 `Products`, `Categories`, 및 `Suppliers` 테이블입니다. 3 단계에서에서 특정 명령줄 구문을 살펴보겠습니다.

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inappdata"></a>2단계: Microsoft SQL Server 2005 Express Edition 데이터베이스에서 참조`App_Data`

`aspnet_regsql.exe` 명령줄 프로그램에 필요한 폴링 인프라를 추가 하기 위해 데이터베이스 및 서버 이름이 필요 합니다. Microsoft SQL Server 2005 Express 데이터베이스에 상주 하는 서버 이름과 데이터베이스는 무엇 일까요는 `App_Data` 폴더? 데이터베이스 및 서버 이름이 있는 항목을 검색 하는 대신 필자 ve 가장 간단한 방법은 데이터베이스를 연결 하는 찾을 수는 `localhost\SQLExpress` 데이터베이스 인스턴스 및 사용 하 여 데이터 이름 바꾸기 [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx)합니다. 컴퓨터에 설치 된 SQL Server 2005의 전체 버전 중 하나에 있으면 다음 있을 가능성이 높습니다 이미 SQL Server Management Studio를 컴퓨터에 설치 합니다. Express edition에만 있는 경우 무료 다운로드할 수 있습니다 [Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796)합니다.

Visual Studio를 닫아 시작 합니다. 다음으로, SQL Server Management Studio를 열고 연결할 선택 된 `localhost\SQLExpress` Windows 인증을 사용 하 여 서버.

![Localhost\SQLExpress 서버에 연결](using-sql-cache-dependencies-cs/_static/image1.gif)

**그림 1**: 에 연결 된 `localhost\SQLExpress` 서버

서버에 연결한 후 Management Studio 서버 표시 되며 데이터베이스, 보안 등에 대 한 하위 폴더가 생성 됩니다. 데이터베이스 폴더 단추로 클릭 하 고 연결 옵션을 선택 합니다. 그러면 데이터베이스 연결 대화 상자 (그림 2 참조). 추가 단추를 클릭 하 고 선택 합니다 `NORTHWND.MDF` 데이터베이스에 웹 응용 프로그램의 폴더 `App_Data` 폴더입니다.

[![NORTHWND를 연결 합니다. App_Data 폴더의 MDF 데이터베이스](using-sql-cache-dependencies-cs/_static/image2.gif)](using-sql-cache-dependencies-cs/_static/image1.png)

**그림 2**: 연결 된 `NORTHWND.MDF` 에서 데이터베이스를 `App_Data` 폴더 ([클릭 하 여 큰 이미지 보기](using-sql-cache-dependencies-cs/_static/image2.png))

이렇게 하면 데이터베이스는 데이터베이스 폴더에 추가 됩니다. 데이터베이스 이름은 데이터베이스 파일의 전체 경로 수 또는 전체 경로 앞에 추가 된 [GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier)합니다. Aspnet를 사용 하는 경우이 긴 데이터베이스 이름을 입력 하지 않아도\_regsql.exe 명령줄 도구, 데이터베이스를 마우스 오른쪽 단추로 클릭 데이터베이스에만 더 친숙 한 이름 연결 하는 이름 바꾸기 및 선택의 이름을 변경 합니다. I ve DataTutorials로 데이터베이스를 변경 합니다.

![좀 더 친숙 한 이름으로 연결된 된 데이터베이스 이름 바꾸기](using-sql-cache-dependencies-cs/_static/image3.gif)

**그림 3**: 좀 더 친숙 한 이름으로 연결된 된 데이터베이스 이름 바꾸기

## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>3단계: Northwind 데이터베이스에 폴링 인프라 추가

에서는 연결 했으므로 합니다 `NORTHWND.MDF` 에서 데이터베이스를 `App_Data` 폴더 폴링 인프라를 추가 하려면 준비 된 것입니다. 다음 네 가지 명령을 실행 하는 적 DataTutorials에 데이터베이스 이름이 바뀌었거나, 가정.

[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample5.cmd)]

이러한 4 개 명령은 실행 한 후 Management Studio에서 데이터베이스 이름을 마우스 오른쪽 단추로 클릭, 작업 하위 메뉴를 이동 및 분리를 선택 합니다. Management Studio를 닫고 Visual Studio를 다시 다음입니다.

Visual Studio를 다시 면 서버 탐색기를 통해 데이터베이스에 드릴 합니다. 새 테이블을 확인 (`AspNet_SqlCacheTablesForChangeNotification`)을 새 저장 프로시저 및 트리거에는 `Products`, `Categories`, 및 `Suppliers` 테이블입니다.

![데이터베이스에는 이제 필요한 폴링 인프라](using-sql-cache-dependencies-cs/_static/image4.gif)

**그림 4**: 데이터베이스에는 이제 필요한 폴링 인프라

## <a name="step-4-configuring-the-polling-service"></a>4단계: 폴링 서비스 구성

필요한 테이블, 트리거 및 저장된 프로시저에서 데이터베이스를 만든 후 마지막 단계를 통해 수행 하는 폴링 서비스를 구성 하는 `Web.config` 데이터베이스 사용 및 폴링 빈도를 밀리초 단위로 지정 하 여 합니다. 다음 태그는 Northwind 데이터베이스를 매초 한 번씩 폴링합니다.

[!code-xml[Main](using-sql-cache-dependencies-cs/samples/sample6.xml)]

합니다 `name` 값을 `<add>` 요소 (NorthwindDB) 특정 데이터베이스를 사용 하 여 알기 쉬운 이름을 연결 합니다. SQL 캐시 종속성을 사용 하는 경우 캐시 된 데이터를 기반으로 하는 테이블 뿐만 아니라 여기에 정의 된 데이터베이스 이름을 참조 하도록 해야 합니다. 사용 하는 방법을 살펴보겠습니다는 `SqlCacheDependency` SQL 캐시 종속성을 프로그래밍 방식으로 연결 하는 데 6 단계에서에서 데이터를 캐시 합니다.

폴링 시스템에 정의 된 데이터베이스에 연결 됩니다 SQL 캐시 종속성을 설정한 후에는 `<databases>` 요소 마다 `pollTime` 시간 (밀리초) 및 실행을 `AspNet_SqlCachePollingStoredProcedure` 저장 프로시저. 추가 된이 저장된 프로시저-3 단계를 사용 하 여 다시를 `aspnet_regsql.exe` 명령줄 도구-반환 된 `tableName` 및 `changeId` 의 각 레코드에 대 한 값 `AspNet_SqlCacheTablesForChangeNotification`. SQL 캐시 종속성을 오래 된 캐시에서 제거 됩니다.

`pollTime` 설정에서는 성능 및 데이터 부실 간에 균형을 유지 합니다. 작은 `pollTime` 값이 데이터베이스에 대 한 요청 수가 증가 하지만 더 신속 하 게 캐시에서 오래 된 데이터를 제거 합니다. 더 큰 `pollTime` 값 데이터베이스 요청 수가 줄어들지만 백 엔드 데이터가 변경 되는 관련된 캐시 항목이 제거 되는 때 사이의 지연 시간을 늘립니다. 다행 스럽게도 데이터베이스 요청을 간단한 저장된 프로시저를 작고 간단한 테이블에서 몇 행을 반환 하는 s를 실행 됩니다. 다른 실험 수행 하지만 `pollTime` 간의 최적 균형을 찾을 수 있는 값 데이터베이스 응용 프로그램에 대 한 액세스 및 데이터 부실 합니다. 최소 `pollTime` 허용 된 값은 500입니다.

> [!NOTE]
> 위의 예제에서는 단일 `pollTime` 값을 `<sqlCacheDependency>` 요소, 있지만 선택적으로 지정할 수는 `pollTime` 값을 `<add>` 요소. 지정 된 여러 데이터베이스가 있고 데이터베이스당 폴링 빈도 사용자 지정 하려는 경우에 유용 합니다.

## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>5단계: SQL 캐시 종속성을 선언적으로 사용

1 ~ 4 단계에서에서 필요한 데이터베이스 인프라를 설정 하 고 폴링 시스템을 구성 하는 방법에 살펴보았습니다. 이 인프라를 구축을 사용 하 여 프로그래밍 방식으로 또는 선언적 기술을 사용 하 여 연결 된 SQL 캐시 종속성을 사용 하 여 데이터 캐시에 항목을 이제 추가할 수 했습니다. 이 단계에서는 SQL 캐시 종속성을 선언적으로 사용 하는 방법을 살펴보겠습니다. 6 단계에서는 프로그래밍 방식을 살펴보겠습니다.

합니다 [ObjectDataSource 사용 하 여 데이터 캐싱](caching-data-with-the-objectdatasource-cs.md) 자습서 ObjectDataSource의 선언적 캐싱 기능을 탐색 합니다. 설정 하 여는 `EnableCaching` 속성을 `true` 및 `CacheDuration` 속성 일부 시간 간격을로 ObjectDataSource 지정된 된 간격에 대 한 기본 개체에서 반환 되는 데이터를 캐시 자동으로 됩니다. ObjectDataSource는 하나 이상의 SQL 캐시 종속성을 사용할 수도 있습니다.

SQL 캐시 종속성을 선언적으로 사용을 보여 주기 위해 엽니다는 `SqlCacheDependencies.aspx` 페이지에 `Caching` 폴더 및 디자이너 도구 상자에서 끌어서 GridView입니다. GridView가 설정 `ID` 하 `ProductsDeclarative` 하 고 스마트 태그, 라는 새로운 ObjectDataSource는 바인딩할 선택 `ProductsDataSourceDeclarative`합니다.

[![ProductsDataSourceDeclarative 라는 새로운 ObjectDataSource는 만들기](using-sql-cache-dependencies-cs/_static/image5.gif)](using-sql-cache-dependencies-cs/_static/image3.png)

**그림 5**: 명명 된 새 ObjectDataSource 만들려면 `ProductsDataSourceDeclarative` ([큰 이미지를 보려면 클릭](using-sql-cache-dependencies-cs/_static/image4.png))

ObjectDataSource를 사용 하 여 구성 합니다 `ProductsBLL` 클래스 및 드롭다운 목록을 선택 탭에서 설정 `GetProducts()`합니다. 업데이트 탭을 선택 합니다 `UpdateProduct` 세 개의 입력된 매개 변수를 사용 하 여 오버 로드 `productName`를 `unitPrice`, 및 `productID`합니다. INSERT 및 DELETE 탭에서 드롭 다운 목록을 (없음)을 설정 합니다.

[![세 개의 입력된 매개 변수를 사용 하 여 UpdateProduct 오버 로드를 사용 합니다.](using-sql-cache-dependencies-cs/_static/image6.gif)](using-sql-cache-dependencies-cs/_static/image5.png)

**그림 6**: 세 개의 입력 매개 변수를 사용 하 여 UpdateProduct 오버 로드를 사용 하 여 ([클릭 하 여 큰 이미지 보기](using-sql-cache-dependencies-cs/_static/image6.png))

[![드롭다운 목록 (없음) 삽입 및 삭제 탭에 대 한 설정](using-sql-cache-dependencies-cs/_static/image7.gif)](using-sql-cache-dependencies-cs/_static/image7.png)

**그림 7**: (없음) 드롭다운 목록을 삽입 및 삭제 하는 탭에 대 한 설정 ([클릭 하 여 큰 이미지 보기](using-sql-cache-dependencies-cs/_static/image8.png))

데이터 소스 구성 마법사를 완료 한 후 Visual Studio는 만들고 BoundFields CheckBoxFields GridView에서 각 데이터 필드에 대 한 합니다. 모든 필드를 제거 하지만 `ProductName`, `CategoryName`, 및 `UnitPrice`, 필요에 맞게 이러한 필드의 서식을 지정 하 고 있습니다. GridView가 스마트 태그에서의 페이징 사용, 정렬 설정 및 편집 사용 확인란을 선택 합니다. Visual Studio는 ObjectDataSource가 설정 됩니다 `OldValuesParameterFormatString` 속성을 `original_{0}`입니다. 이 속성 선언 구문 또는 해당 기본값으로 다시 설정에서 완전히 제거 하거나 제대로 작동 하려면 GridView의 편집 기능에 대 한 순서로 `{0}`합니다.

GridView 및 집합 위에 Label 웹 컨트롤을 추가 하는 마지막으로, 해당 `ID` 속성을 `ODSEvents` 및 해당 `EnableViewState` 속성을 `false`입니다. 다음과 같이 변경한 후 페이지 s 선언적 태그 다음과 유사 합니다. 참고는 ve의 SQL 캐시 종속성 기능을 보여 주기 위해 필요 하지 않은 GridView 필드에 미적 사용자 지정을 수행 합니다.

[!code-aspx[Main](using-sql-cache-dependencies-cs/samples/sample7.aspx)]

다음으로 ObjectDataSource s에 대 한 이벤트 처리기를 만듭니다 `Selecting` 이벤트에 다음 코드를 추가 합니다.

[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample8.cs)]

이전에 설명한 대로 ObjectDataSource가의 `Selecting` 기본 개체에서 데이터를 검색 하는 경우에 이벤트가 발생 합니다. ObjectDataSource에서 고유한 캐시에서 데이터를 액세스할 경우이 이벤트는 발생 되지 않습니다.

이제 브라우저를 통해이 페이지를 방문 합니다. 이후로 아직 캐싱을 구현 하 든, 페이지, 정렬 또는 그리드 페이지를 편집할 때마다 ve 표시할 텍스트, 선택 하면 이벤트 발생 그림 8 있듯이.

[![각 편집 GridView 페이징 되는 시간 또는 Sorted ObjectDataSource가 선택 하면 이벤트 발생](using-sql-cache-dependencies-cs/_static/image8.gif)](using-sql-cache-dependencies-cs/_static/image9.png)

**그림 8**: ObjectDataSource s `Selecting` 이벤트 발생 합니다. 각 시간 페이징 되는 GridView, 편집, Sorted ([큰 이미지를 보려면 클릭](using-sql-cache-dependencies-cs/_static/image10.png))

보았듯이 [ObjectDataSource 사용 하 여 데이터 캐싱](caching-data-with-the-objectdatasource-cs.md) 자습서를 설정 합니다 `EnableCaching` 속성을 `true` ObjectDataSource 하 여 지정한 기간 동안 해당 데이터를 캐시 하면 해당 `CacheDuration` 속성. ObjectDataSource에는 [ `SqlCacheDependency` 속성](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx), 캐시 된 데이터의 패턴을 사용 하 여 하나 이상의 SQL 캐시 종속성을 추가 하는:

[!code-css[Main](using-sql-cache-dependencies-cs/samples/sample9.css)]

여기서 *databaseName* 에 지정 된 데이터베이스의 이름입니다는 `name` 특성을 `<add>` 요소에 `Web.config`, 및 *tableName* 데이터베이스 테이블의 이름입니다. 예를 들어, 데이터를 무기한으로 캐시 하는 ObjectDataSource를 만들려면 Northwind s에 대 한 SQL 캐시 종속성에 따라 `Products` 테이블에서 집합 ObjectDataSource s `EnableCaching` 속성을 `true` 고 `SqlCacheDependency` 속성을 NorthwindDB:Products 합니다.

> [!NOTE]
> SQL 캐시 종속성을 사용할 수 있습니다 *및* 를 설정 하 여 시간 기반 만료 `EnableCaching` 하 `true`, `CacheDuration` 시간 간격 및 `SqlCacheDependency` , 데이터베이스 및 테이블 이름에 합니다. ObjectDataSource를 때나 폴링 시스템 정보 중 발생 하는 먼저 기본 데이터베이스 데이터가 변경 된 시간 기반 만료에 도달한 경우 해당 데이터를 제거 합니다.

GridView `SqlCacheDependencies.aspx` -두 테이블의 데이터를 표시 `Products` 하 고 `Categories` (s 제품 `CategoryName` 필드를 통해 검색 되는 `JOIN` 에서 `Categories`). 따라서 두 개의 SQL 캐시 종속성을 지정 해야 합니다. NorthwindDB:Products; NorthwindDB:Categories 합니다.

[![SQL 캐시 종속성을 사용 하 여 제품 범주에 캐싱 지원 하기 위해 ObjectDataSource 구성](using-sql-cache-dependencies-cs/_static/image9.gif)](using-sql-cache-dependencies-cs/_static/image11.png)

**그림 9**: 지원 캐싱을 사용 하 여 SQL 캐시 종속성 ObjectDataSource 구성 `Products` 하 고 `Categories` ([클릭 하 여 큰 이미지 보기](using-sql-cache-dependencies-cs/_static/image12.png))

캐싱 지원 하기 위해 ObjectDataSource를 구성한 후 브라우저를 통해 페이지를 다시 확인 합니다. 다시 발생 하는 텍스트 선택 하면 이벤트 첫 번째 페이지 방문을 표시 해야 하지만 사라집니다 페이징, 정렬 또는 편집 취소 단추를 클릭 하는 경우. 이므로이 데이터는 ObjectDataSource가 캐시로 로드 되 면 남아까지 합니다 `Products` 또는 `Categories` 테이블을 수정 하거나 데이터를 GridView를 통해 업데이트 됩니다.

표를 통해 페이징 및 선택 하면 이벤트의 부재 주목할 발생 후 텍스트를 새 브라우저 창을 열고 편집, 삽입 및 삭제 섹션의에서 기본 사항 자습서로 이동 (`~/EditInsertDelete/Basics.aspx`). 이름 또는 제품의 가격을 업데이트 합니다. 그런 다음에서 첫 번째 브라우저 창에 다른 페이지를 보려면 데이터, 표에서 정렬 또는 행의 편집 단추를 클릭 합니다. 이 이번에 발생 하는 이벤트 선택 해야 다시 기본 데이터베이스 데이터 수정 (그림 10 참조). 텍스트 표시 되지 않는 경우 몇 분 정도 기다린 후 다시 시도 하세요. 폴링 서비스는 변경 내용을 확인 하 고 있는 기억 합니다 `Products` 테이블 마다 `pollTime` 기본 데이터가 업데이트 될 때 캐시 된 데이터를 제거 하는 때 사이의 지연이 발생 하므로 밀리초.

[![캐시 제품 데이터를 제거 제품 테이블 수정](using-sql-cache-dependencies-cs/_static/image10.gif)](using-sql-cache-dependencies-cs/_static/image13.png)

**그림 10**: 캐시 제품 데이터를 제거 제품 테이블 수정 ([클릭 하 여 큰 이미지 보기](using-sql-cache-dependencies-cs/_static/image14.png))

## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>6단계: 프로그래밍 방식으로 작업을`SqlCacheDependency`클래스

합니다 [아키텍처에서 데이터 캐싱](caching-data-in-the-architecture-cs.md) 자습서 긴밀 하 게 ObjectDataSource를 사용 하 여 캐시를 연결 하는 대신 아키텍처에서 별도 캐싱 레이어를 사용 하는 이점은 살펴보았습니다. 이 자습서에서 만든를 `ProductsCL` 클래스를 프로그래밍 방식으로 캐시 작업에 데이터를 보여 줍니다. 캐싱 계층의 SQL 캐시 종속성을 사용 하려면 사용 된 `SqlCacheDependency` 클래스입니다.

폴링 시스템과 `SqlCacheDependency` 개체는 특정 데이터베이스 및 테이블 쌍으로 연결 되어야 합니다. 다음 코드 예를 들어 만듭니다는 `SqlCacheDependency` s Northwind 데이터베이스를 기반으로 개체 `Products` 테이블:

[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample10.cs)]

두 입력 매개 변수는 `SqlCacheDependency`의 생성자는 데이터베이스 및 테이블 이름을 각각. ObjectDataSource s와 마찬가지로 `SqlCacheDependency` 에 사용 되는 데이터베이스 이름 속성이에 지정 된 값과 동일 합니다 `name` 특성을 `<add>` 요소에 `Web.config`입니다. 테이블 이름은 데이터베이스 테이블의 실제 이름입니다.

연결 하는 `SqlCacheDependency` 데이터 캐시에 추가 하는 항목을 중 하나를 사용 합니다 `Insert` 종속성을 허용 하는 메서드 오버 로드 합니다. 다음 코드를 추가 *값* 는 정해 지지 않은 기간에 대 한 데이터 캐시에 있지만 연결 하는 `SqlCacheDependency` 에 `Products` 테이블입니다. 간단히 말해 *값* 폴링 시스템을 발견 했기 때문에 메모리 제약 조건으로 인해 제거 될 때까지 또는 캐시에 남아는 `Products` 테이블 캐시 된 이후 변경 되었습니다.

[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample11.cs)]

캐싱 계층 s `ProductsCL` 클래스에는 현재 데이터를 캐시 합니다 `Products` 는 시간 기반 만료 시간인 60 초를 사용 하 여 테이블입니다. SQL 캐시 종속성을 대신 사용할 수 있도록이 클래스를 업데이트 하는 s 수 있습니다. 합니다 `ProductsCL` s 클래스 `AddCacheItem` 캐시에 데이터를 추가 하는 일을 담당 하는 메서드는 현재 다음 코드를 포함 합니다.

[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample12.cs)]

이 코드를 사용 하 여 업데이트를 `SqlCacheDependency` 개체 대신는 `MasterCacheKeyArray` 캐시 종속성:

[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample13.cs)]

이 기능을 테스트 하려면 아래에 있는 기존 페이지에 GridView 추가 `ProductsDeclarative` GridView입니다. 이 새로운 GridView가 설정 `ID` 하 `ProductsProgrammatic` 및 해당 스마트 태그를 통해 바인딩할 라는 새로운 ObjectDataSource는 `ProductsDataSourceProgrammatic`합니다. ObjectDataSource를 사용 하 여 구성 합니다 `ProductsCL` 클래스를 업데이트 탭을 확인 하 고 드롭다운 목록에서 선택 `GetProducts` 및 `UpdateProduct`, 각각.

[![ProductsCL 클래스를 사용 하는 ObjectDataSource 구성](using-sql-cache-dependencies-cs/_static/image11.gif)](using-sql-cache-dependencies-cs/_static/image15.png)

**그림 11**: ObjectDataSource를 사용 하 여 구성 합니다 `ProductsCL` 클래스 ([큰 이미지를 보려면 클릭](using-sql-cache-dependencies-cs/_static/image16.png))

[![GetProducts 메서드 선택 탭의 드롭 다운 목록에서 선택](using-sql-cache-dependencies-cs/_static/image12.gif)](using-sql-cache-dependencies-cs/_static/image17.png)

**그림 12**: 선택 된 `GetProducts` 선택 탭의 드롭 다운 목록에서에서 메서드 ([큰 이미지를 보려면 클릭](using-sql-cache-dependencies-cs/_static/image18.png))

[![UpdateProduct 메서드와 같이 업데이트 탭의 드롭 다운 목록에서 선택](using-sql-cache-dependencies-cs/_static/image13.gif)](using-sql-cache-dependencies-cs/_static/image19.png)

**그림 13**: UpdateProduct 메서드와 같이 업데이트 탭의 드롭 다운 목록에서에서 선택 ([클릭 하 여 큰 이미지 보기](using-sql-cache-dependencies-cs/_static/image20.png))

데이터 소스 구성 마법사를 완료 한 후 Visual Studio는 만들고 BoundFields CheckBoxFields GridView에서 각 데이터 필드에 대 한 합니다. 이 페이지에 추가할 첫 번째 GridView를 사용 하 여 모든 필드를 제거 하는 같은 하지만 `ProductName`, `CategoryName`, 및 `UnitPrice`, 필요에 맞게 이러한 필드의 서식을 지정 하 고 있습니다. GridView가 스마트 태그에서의 페이징 사용, 정렬 설정 및 편집 사용 확인란을 선택 합니다. 와 마찬가지로 합니다 `ProductsDataSourceDeclarative` ObjectDataSource, Visual Studio 설정 합니다 `ProductsDataSourceProgrammatic` ObjectDataSource s `OldValuesParameterFormatString` 속성을 `original_{0}`합니다. GridView의 편집 기능이 제대로 작동 하려면이 속성을 설정 하기 위해 다시 `{0}` (또는 완전히 선언적 구문에서 속성 할당을 제거) 합니다.

이러한 태스크를 완료 한 후 결과 GridView 및 ObjectDataSource 선언적 태그는 다음과 같이 표시 됩니다.

[!code-aspx[Main](using-sql-cache-dependencies-cs/samples/sample14.aspx)]

SQL을 테스트 하려면 캐싱 계층의 캐시 종속성에 중단점을 설정 합니다 `ProductCL` s 클래스 `AddCacheItem` 메서드와 다음 디버깅을 시작 합니다. 처음 방문 하면 `SqlCacheDependencies.aspx`, 데이터를 처음으로 요청 하 고 캐시에 배치 하는 대로 중단점이 적중 됩니다. 그런 다음 GridView의 다른 페이지로 이동 하거나 열 중 하나를 정렬 합니다. 이후 캐시에서 데이터를 찾을 수는 있지만 이렇게 하면 해당 데이터를 쿼리할 GridView는 `Products` 데이터베이스 테이블 수정 되지 않았습니다. 데이터를 반복적으로 없는 캐시에 메모리가 충분 한 컴퓨터에 있는지 확인 하 고 다시 시도.

두 번째 브라우저 창을 열고 편집, 삽입 및 삭제 섹션의에서 기본 사항 자습서로 이동 후 GridView의 일부 페이지를 통해 페이징 (`~/EditInsertDelete/Basics.aspx`). Products 테이블에서 레코드를 업데이트 및 그런 다음 첫 번째 브라우저 창에서 새 페이지를 보거나 정렬 헤더 중 하나를 클릭 합니다.

이 시나리오에서는 두 가지 중 하나가 표시 됩니다 있습니다: 하거나 중단점 도달 하면 캐시 된 데이터를 데이터베이스에서 변경으로 인해 제거 된는 나타내는 또는 중단점이 적중 되지 것입니다, 즉 `SqlCacheDependencies.aspx` 는 이제 오래 된 데이터를 표시 합니다. 중단점이 적중 되지 않습니다 하는 경우에 있기 때문일 것 폴링 서비스 데이터가 변경 된 후 아직 발생 하지는 합니다. 폴링 서비스는 변경 내용을 확인 하 고 있는 기억 합니다 `Products` 테이블 마다 `pollTime` 기본 데이터가 업데이트 될 때 캐시 된 데이터를 제거 하는 때 사이의 지연이 발생 하므로 밀리초.

> [!NOTE]
> 이 지연은에서 GridView 통해 제품 중 하나를 편집 시 표시 될 가능성이 `SqlCacheDependencies.aspx`합니다. 에 [아키텍처에서 데이터 캐싱](caching-data-in-the-architecture-cs.md) 추가한 자습서는 `MasterCacheKeyArray` 종속성을 통해 편집 중인 데이터를 확인 하는 캐시는 `ProductsCL` s 클래스 `UpdateProduct` 메서드는 캐시에서 제거 되었습니다. 그러나 수정 하는 경우이 캐시 종속성 교체 합니다 `AddCacheItem` 이 단계에서는 이전 메서드 있어를 `ProductsCL` 폴링 시스템 정보는 변경 될 때까지 캐시 된 데이터를 표시 하려면 클래스는 계속를 `Products` 테이블. 다시 삽입 하는 방법을 살펴보겠습니다는 `MasterCacheKeyArray` 7 단계에서에서 종속성을 캐시 합니다.

## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>7단계: 캐시 된 항목을 사용 하 여 여러 종속성 연결

이전에 설명한 대로 합니다 `MasterCacheKeyArray` 캐시 종속성 확인 되 *모든* 제품 관련 데이터에 연결 된 모든 단일 항목이 업데이트 될 때 캐시에서 제거 됩니다. 예를 들어 합니다 `GetProductsByCategoryID(categoryID)` 메서드 캐시 `ProductsDataTables` 인스턴스 각각에 대 한 고유 *categoryID* 값입니다. 이러한 개체 중 하나를 제거 하는 경우는 `MasterCacheKeyArray` 캐시 종속성 확인 하는 다른 사용자도 제거 됩니다. 이 캐시 종속성 없이 캐시 된 데이터가 수정 될 때 가능성이 있습니다 다른 캐시 제품 데이터가 만료 될 수 있습니다. 결과적으로 해당 s에서는 유지 관리 하는 것이 중요 합니다 `MasterCacheKeyArray` SQL 캐시 종속성을 사용 하는 경우 종속성을 캐시 합니다. 그러나 데이터 캐시 s `Insert` 메서드는 단일 종속성 개체에 대 한 허용 합니다.

또한 SQL 캐시 종속성을 사용 하 여 작업 하는 경우에 종속성으로 여러 데이터베이스 테이블을 연결 해야 할 수도 했습니다. 예를 들어,를 `ProductsDataTable` 에 캐시를 `ProductsCL` 각 제품 범주 및 공급 업체 이름을 포함 하는 클래스 이지만 `AddCacheItem` 메서드는 종속성에 대해서만 사용 `Products`합니다. 이 경우 사용자 범주 또는 공급 업체의 이름을 업데이트 하는 경우 캐시 된 제품 데이터 캐시에 남아를 기간이 만료 합니다. 캐시 제품 데이터에 종속 되 게 하고자 하므로 뿐 아니라는 `Products` 테이블 이지만 합니다 `Categories` 및 `Suppliers` 테이블도 합니다.

합니다 [ `AggregateCacheDependency` 클래스](https://msdn.microsoft.com/library/system.web.caching.aggregatecachedependency.aspx) 캐시 항목을 여러 종속성 연결에 대 한 수단을 제공 합니다. 만들어 시작을 `AggregateCacheDependency` 인스턴스. 다음으로 사용 하 여 종속성 집합을 추가 합니다 `AggregateCacheDependency` s `Add` 메서드. 데이터 캐시에 항목을 이후 삽입할 때 전달 된 `AggregateCacheDependency` 인스턴스. 때 *모든* 의 `AggregateCacheDependency` 인스턴스의 종속성이 변경, 캐시 된 항목을 제거 합니다.

다음에 대 한 업데이트 된 코드를 보여 줍니다.는 `ProductsCL` s 클래스 `AddCacheItem` 메서드. 메서드를 만듭니다는 `MasterCacheKeyArray` 종속성과 함께 캐시 `SqlCacheDependency` 에 대 한 개체를 `Products`를 `Categories`, 및 `Suppliers` 테이블입니다. 이 모든 하나로 결합 `AggregateCacheDependency` 개체인 `aggregateDependencies`에 전달 되는 `Insert` 메서드.

[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample15.cs)]

이 새 코드를 테스트 합니다. 이제 변경 합니다 `Products`, `Categories`, 또는 `Suppliers` 테이블 하면 캐시 된 데이터를 제거 합니다. 또한를 `ProductsCL` s 클래스 `UpdateProduct` GridView 통해 제품을 편집할 때 호출 되는 메서드를 제거 합니다 `MasterCacheKeyArray` 캐시는 캐시 된 종속성 `ProductsDataTable` 제거 될 수와 다음 다시 검색 될 데이터 요청입니다.

> [!NOTE]
> SQL 캐시 종속성 사용 하 여 사용할 수도 있습니다 [출력 캐싱을](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx)합니다. 이 기능의 데모를 참조 하세요. [ASP.NET을 사용 하 여 SQL Server를 사용 하 여 캐싱 출력](https://msdn.microsoft.com/library/e3w8402y(VS.80).aspx)합니다.

## <a name="summary"></a>요약

데이터베이스 데이터 캐싱, 데이터를 데이터베이스에 수정 될 때까지 캐시에 이상적으로 유지 됩니다. ASP.NET 2.0을 사용 하 여에 SQL 캐시 종속성을 생성 하 고 선언적 방법과 프로그래밍 시나리오에서 사용 될 수 있습니다. 데이터가 수정 되 면 검색이 방법 사용 하 여 과제 중 하나입니다. Microsoft SQL Server 2005의 전체 버전의 쿼리 결과 변경 되 면 응용 프로그램을 경고 하는 알림 기능을 제공 합니다. Express Edition의 SQL Server 2005 및 이전 버전의 SQL Server의 경우 폴링 시스템을 대신 사용 되어야 합니다. 다행 스럽게도 필요한 폴링 인프라 설정이 매우 간단 합니다.

즐거운 프로그래밍!

## <a name="further-reading"></a>추가 정보

이 자습서에서 다루는 항목에 대 한 자세한 내용은 다음 리소스를 참조 하세요.

- [Microsoft SQL Server 2005의에서 쿼리 알림 사용](https://msdn.microsoft.com/library/ms175110.aspx)
- [쿼리 알림 생성](https://msdn.microsoft.com/library/ms188669.aspx)
- [사용 하 여 ASP.NET에서 캐싱는 `SqlCacheDependency` 클래스](https://msdn.microsoft.com/library/ms178604(VS.80).aspx)
- [ASP.NET SQL Server 등록 도구 (`aspnet_regsql.exe`)](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)
- [개요 `SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>저자 소개

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), 7 ASP/ASP.NET 서적의 저자 이자 설립자입니다 [4GuysFromRolla.com](http://www.4guysfromrolla.com), 1998 Microsoft 웹 기술을 사용 하 여 왔습니다. Scott는 독립 컨설턴트, 강사, 그리고 기록기로 작동합니다. 최근 저서는 [ *Sams 설명 직접 ASP.NET 2.0 24 시간 동안의*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)합니다. 그에 도달할 수 있습니다 [ mitchell@4GuysFromRolla.com합니다.](mailto:mitchell@4GuysFromRolla.com) 찾을 수 있는 저자의 블로그를 통해 또는 [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)합니다.

## <a name="special-thanks-to"></a>특별히 감사

이 자습서 시리즈는 많은 유용한 검토자가 검토 되었습니다. 이 자습서에 대 한 선행 검토자 Marko Rangel, Teresa Murphy 및 Hilton giesenow가 되었습니다. 내 향후 MSDN 문서를 검토에 관심이 있으십니까? 그렇다면 삭제 나에서 선 [ mitchell@4GuysFromRolla.com합니다.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [이전](caching-data-at-application-startup-cs.md)
> [다음](caching-data-with-the-objectdatasource-vb.md)
