---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
title: ASP.NET 호스팅 옵션 (C#) | Microsoft Docs
author: rick-anderson
description: ASP.NET 웹 응용 프로그램은 일반적으로 설계, 생성 및 로컬 개발 환경에서 테스트 및 프로덕션 환경 o에 배포 해야 하는 중...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 89a1d2bc-fdfd-4c5c-a3b0-49a08baaf63a
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
msc.type: authoredcontent
ms.openlocfilehash: 0ec92a3b719116d8ef457156788ac451a300dbfc
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65130662"
---
# <a name="aspnet-hosting-options-c"></a>ASP.NET 호스팅 옵션(C#)

[Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF 다운로드](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial01_Basics_cs.pdf)

> ASP.NET 웹 응용 프로그램은 일반적으로 디자인, 생성 및 테스트 로컬 개발 환경 및 릴리스에 대 한 준비가 되 면 프로덕션 환경에 배포 해야 합니다. 이 자습서 배포 프로세스의 상위 수준 개요를 제공 하 고이 자습서 시리즈에 대 한 기본적인 사항을 제공 합니다.

## <a name="introduction"></a>소개

웹 응용 프로그램은 일반적으로 디자인, 생성 및 사이트에서 작업 하는 프로그래머에만 액세스할 수 있는 개발 환경에서 테스트 응용 프로그램을 릴리스할 준비가 되 면 이동 프로덕션 환경에 인터넷에서 누구나 사이트에 액세스할 수 있습니다. 이 배포 프로세스는 많은 문제를 제공합니다.

- 프로덕션 환경에 존재 해야 하며 적절 하 게 설치 전에 ASP.NET 응용 프로그램을 배포할 수 있습니다. 또한 프로덕션 환경 최신 보안 패치를 사용 하 여 최신 상태로 유지 되어야 합니다.
- 태그 파일과 코드 파일 지원 파일의 올바른 집합 개발 환경에서 프로덕션 환경에 복사 해야 합니다. 데이터 기반 응용 프로그램에 대 한 데이터베이스 스키마 및/또는 데이터를 복사 필요할 수 있습니다.
- 두 환경 간의 구성 차이 있을 수 있습니다. 개발 환경에서 사용 되는 데이터베이스 연결 문자열 또는 전자 메일 서버에서 프로덕션 환경과 다를 가능성이 큽니다. 게다가 환경에 응용 프로그램의 동작은 달라질 수 있습니다. 예를 들어, 개발에서 오류가 발생 하는 경우 오류 세부 정보 화면에서 표시할 수 있습니다 않으며 프로덕션 환경에서 오류가 발생 하는 경우 친숙 한 오류 페이지가 대신 표시 됩니다. 오류 세부 정보를 개발자에 게 전자 메일로 전송 합니다.

여러 개인 사용자와 비즈니스 아웃소싱를 프로덕션 환경-설정 및 프로덕션 환경을 유지 관리-첫 번째 질문을 제거 하려면 *웹 호스팅 공급자*합니다. 웹 호스팅 공급자에는 사용자를 대신해 프로덕션 환경을 관리 하는 회사입니다. 수많은 웹 호스트 공급자를 사용 하 여 다양 한 가격 및 서비스 수준을; 각각 가지 이러한 서비스 공급자 찾기에 대 한 팁 "호스트 웹 공급자 찾기" 섹션을 참조 하세요.

이 웹 호스트 공급자가 관리 되는 프로덕션 환경에 ASP.NET 웹 응용 프로그램을 배포 하는 단계를 확인 하는 자습서 시리즈의 첫 번째입니다. 이 자습서의 과정을 살펴보겠습니다.

- 파일 웹 호스트 공급자에 게 배포 해야 합니다.
- 배포 프로세스를 간소화 하는 것에 대 한 도구입니다.
- 데이터베이스를 배포 하는 방법입니다.
- 사용 하는 데이터베이스를 배포 하기 위한 팁 [SQL 기반 멤버 자격 및 역할 공급자를](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md), 프로덕션 환경에서 웹 사이트 관리 도구를 모방 하는 방법입니다.
- 원활 하 게 개발 하는 동안 변경 내용으로 프로덕션 환경에서 데이터베이스를 업데이트 하기 위한 전략입니다.
- 프로덕션 및 오류가 발생할 때 개발자에 알리는 방법에서 발생 하는 오류를 기록에 대 한 기술입니다.

간결 하 고 프로세스를 안내 하는 시각적으로 스크린 샷을 많은 단계별 지침을 제공 하려면이 자습서에 맞게 구성 합니다. 이 첫 자습서 찾기 웹 호스팅 공급자에 조언을 고 ASP.NET 배포 프로세스의 개요를 제공 합니다. 이제 시작 하겠습니다.

## <a name="an-overview-of-the-aspnet-deployment-process"></a>ASP.NET 배포 프로세스 개요

간단히 말해 ASP.NET 응용 프로그램 배포는 다음 세 단계가 포함 됩니다.

1. 프로덕션 환경에서 웹 응용 프로그램, 웹 서버 및 데이터베이스를 구성 합니다.
2. ASP.NET 페이지, 코드 파일, 어셈블리에 동기화 된 `Bin` 폴더 및 CSS 및 JavaScript 파일과 같은 HTML 관련 지원 파일.
3. 데이터베이스 스키마 및/또는 데이터를 동기화 합니다.

일반적으로 웹 응용 프로그램에 대 한 구성 정보에는 `Web.config` 파일 및 데이터베이스 연결 문자열, 오류 처리 조건 URL 재작성 규칙 및 전자 메일 서버 정보를 포함 합니다. 종종이 정보는 응용 프로그램 개발 및 프로덕션 환경에서 동일한 응용 프로그램의에서 다릅니다. 예를 들어, 응용 프로그램을 개발 하는 경우 프로덕션 데이터베이스에 대해 테스트할 수 있도록 개발 데이터베이스를 사용 하는 합니다. 결과적으로, 데이터베이스 연결 문자열을 일반적으로 개발 및 프로덕션 응용 프로그램 간에 서로 다릅니다. 이러한 차이로 인해 배포의 일부로 웹 응용 프로그램의 구성 정보를 변경 하는 포함 됩니다.

1 단계는 웹 응용 프로그램 구성 변경 하는 것 외에도 웹 서버 및 데이터베이스에 대 한 구성도 해야 할 수도 있습니다. 예를 들어, ASP.NET 페이지를 만들거나 웹 서버의 디렉터리에서 파일을 삭제 하는 경우 다음 웹 서버 해야 이러한 파일 시스템 수정 허용 하도록 구성 합니다. 마찬가지로 데이터베이스에 대 한 필요한 사용 권한 또는 인증 설정 있을 수 있습니다.

2 단계에서는 essential ASP.NET 페이지 및 개발 및 프로덕션 환경 간에 지원 파일 집합을 동기화 합니다. 특정은 ASP의 집합입니다. 두 환경 간의 동기화 해야 하는 NET 관련 파일 다음 자습서의 설명을 이며 Visual Studio에서 만든 프로젝트의 유형에 따라 달라 집니다 [ *확인 파일 필요한배포할*](determining-what-files-need-to-be-deployed-cs.md). 세 번째와 네 번째 자습서- [ *Your 사이트를 사용 하 여 FTP 배포* ](deploying-your-site-using-an-ftp-client-cs.md) 하 고 [ *배포 사이트를 사용 하 여 Visual Studio* ](deploying-your-site-using-visual-studio-cs.md) -검사 다양 한 도구 및 기술을 이러한 파일을 동기화 합니다.

사용 중인 일반적으로 두 데이터베이스는 데이터 기반 응용 프로그램을 빌드할 때: 개발 및 프로덕션에 각각 하나씩 있습니다. 개발 하는 동안 개발 데이터베이스의 스키마를 새 테이블, 열, 저장된 프로시저 및 트리거를 포함 하도록 수정할 수 있습니다 또는 제거 하거나 기존 데이터베이스 개체 이름을 바꾸기로 수정할 수 있습니다. 이러한 변경 내용이 적용 될 당시와 응용 프로그램이 프로덕션으로 배포 될 때 개발 및 프로덕션 데이터베이스에 동기화 되지 않습니다. 이 비동기 배포 프로세스 중 수정 해야 합니다. 이후 자습서에서 이러한 문제를 검사할 수 됩니다.

## <a name="finding-a-web-host-provider"></a>웹 호스트 공급자 찾기

.NET Framework 및 인터넷 정보 서비스 (IIS)가 설치 되어 있는 모든 웹 서버에 ASP.NET 응용 프로그램을 배포할 수 있습니다. 개인 컴퓨터에서 인터넷 및 파악 하는 광대역 연결 들어오는 웹 요청을 허용 하도록 라우터를 구성 하는 방법으로 열었던 가정 하 고 사이트를 호스트할 수 있습니다. 또한 대부분의 회사와 마찬가지로 인트라넷 컴퓨터에서 사이트를 호스트할 수 있습니다. 그러나이 자습서의 포커스를 웹 호스트 공급자를 사용 하 여 웹 사이트를 호스팅입니다.

> [!NOTE]
> [IIS](https://www.iis.net/) Microsoft의 엔터프라이즈 수준 웹 서버입니다. Windows, Windows Server 2008 등 특정 버전의 Windows Vista-홈 버전을 사용 하 여 제공 합니다. Visual Studio ASP.NET 개발 웹 서버를 포함 하는 대로 개발 환경에서 ASP.NET 응용 프로그램을 작동 하려면 IIS를 설치 하려면 필요가 없습니다. 그러나 ASP.NET 개발 웹 서버를 로컬 연결만 허용 하 고 프로덕션 환경에서 사용할 수 없습니다.

웹 호스트 공급자에 사이트를 배포 하기 전에 사용 하 여 업무에 어떤 회사를 먼저 결정 해야 합니다. 수많은 웹 호스팅 회사 marketplace에는 "웹 호스팅 회사를"에 대 한 검색을 5 백만 개 결과 반환 합니다. 적합 한 하나 하려면 어떻게 해야 하십니까? 원하는 검색 엔진이 좋은 시작 위치와 같은 웹 사이트 [TopHosts](http://www.tophosts.com/) 하 고 [HostCritique](http://www.hostcritique.net/)를 비교 하 고 다양 한 호스팅 서비스를 대조 하는 합니다. 또한는 것이 좋습니다으로 동료와 동료 모든 권장 사항을 요청 권장 사항에 대 한 문의할 수도 있습니다는 [Open Forum 호스팅](https://forums.asp.net/158.aspx) 여기에 [ASP.NET 포럼](https://forums.asp.net/).

일반적으로 웹 호스팅 회사는 공유 호스팅 계획을 제공 하 고 전용 호스팅 계획 합니다. 사용 하 여 공유는 단일 웹 서버 호스트 수백 없으면 수십 서로 다른 웹 사이트의 호스팅. 전용 호스팅 사용 하 여 사이트와 사이트를 단독으로 사용 되는 회사에서 컴퓨터를 임대 합니다. 공유 호스팅 계획을 $9.95/월에 대 한 Microsoft Access 데이터베이스, 5GB의 디스크 공간 및 100GB의 월간 대역폭 트래픽에 사용할 수 있게 ASP.NET 페이지에 대 한 지원이 포함 될 수 있습니다. 다른 공유 호스팅 계획에는 $19.95 / 월에 대 한 Microsoft SQL Server 2008 데이터베이스 서버, 10GB의 디스크 공간 및 250GB의 월간 대역폭 트래픽에 액세스 ASP.NET 페이지에 대 한 지원을 포함할 수 있습니다. 전용 호스팅 계획은 일반적으로 훨씬 더 비용이 많이 드는, 비용 몇 백 달러 월 하지만 더 나은 성능을 제공 하 고 공유 보다 세부적인 제어가 호스팅 옵션입니다. 어떤 계획을 선택 하면 예산에 따라 달라 집니다, 얼마나 많은 트래픽을 웹 사이트를 받고 기능 수를 예상 해야 합니다.

웹 호스트 공급자를 선택할 때 두 가지 중요 한 고려 사항에는 고객 서비스 및 서비스 품질입니다. 질문 또는 구성 문제가 있는 경우이 얼마를 응답에 도달할 때까지 기술 지원팀 웹 호스트의 문제를 제출? 회사의 서비스 안정성을 파악할? 데이터베이스 작동 중단을 자주 이러한가지고 있습니까? 자신의 전자 메일 서버는 얼마나 자주 오프 라인으로 전환? 가 작동 하는 방법에 대 한 세부 정보를 제공 하 고 해당 고객 서비스 정책에 대 한 질의 시에 회사를 항상 확인 수 이지만 더 있으리라는 온라인 포럼, 뉴스 그룹 및 메일 listservs를 통해 수행할 수 있는 현재 및 과거 고객의 피드백을 .

> [!NOTE]
> 일부 웹 호스팅 회사 업무를.NET과 같은 특정 기술 스택에서 집중 또는 [LAMP](http://en.wikipedia.org/wiki/LAMP_stack) (**L** inux 합니다 **A** pache, **M** ySQL, 및 **P** HP)를 선택 하면 회사 ASP.NET 응용 프로그램을 호스트 하 고 있는지 확인 합니다. 또한 응용 프로그램을 빌드하는 데 사용할 ASP.NET 버전 지 하는지 확인 합니다. 및 데이터 기반 응용 프로그램을 작성 하는 경우 웹 호스트는 동일한 데이터베이스 서버 및 사용 중인 버전을 제공 하 고 있는지 확인 합니다.

## <a name="summary"></a>요약

ASP.NET 웹 응용 프로그램은 일반적으로 디자인, 생성 및 로컬 개발 환경에서 테스트 버전을 릴리스할 준비가 되 면 프로덕션 환경으로 이동 합니다. 호스트 ASP.NET 웹 사이트 또는 회사 내 서버 개인용 컴퓨터에서 수 있지만, 많은 기업 및 개인 웹 호스트 공급자에 해당 호스트를 아웃소싱하는 데 선택 합니다.

이 자습서 시리즈에서는 일반적인 문제를 탐색 하는 웹 호스트 공급자를 ASP.NET 응용 프로그램을 배포 하는 단계를 검토 합니다. 이 자습서는 ASP.NET 배포 프로세스의 상위 수준 개요를 제공 하 고 적합 한 웹 호스트 공급자를 찾기 위한 팁을 제공 했습니다. 다음 자습서 웹 사이트를 배포 하는 경우 프로덕션 환경에 복사 해야 하는 ASP.NET 관련 파일에 살펴봅니다.

즐거운 프로그래밍!

### <a name="special-thanks-to"></a>특별히 감사 하는 중...

이 자습서 시리즈는 많은 유용한 검토자가 검토 되었습니다. 이 자습서에 대 한 선행 검토자 Teresa Murphy 했습니다. 내 향후 MSDN 문서를 검토에 관심이 있으십니까? 그렇다면 삭제 나에서 선 [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)합니다.

> [!div class="step-by-step"]
> [다음](determining-what-files-need-to-be-deployed-cs.md)
