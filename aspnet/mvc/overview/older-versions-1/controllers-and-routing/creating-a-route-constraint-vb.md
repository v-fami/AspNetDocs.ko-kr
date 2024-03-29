---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: 경로 제약 조건 (VB) 만들기 | Microsoft Docs
author: StephenWalther
description: 이 자습서에서는 Stephen walther가 브라우저 정규식을 사용 하 여 경로 제약 조건을 만들면 일치 경로 요청 하는 방법을 제어 하는 방법을 보여 줍니다.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 205742dd8f866c8828008c8aac7ab3f98b173ceb
ms.sourcegitcommit: 51b01b6ff8edde57d8243e4da28c9f1e7f1962b2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65123420"
---
# <a name="creating-a-route-constraint-vb"></a>경로 제약 조건 만들기(VB)

[Stephen walther가](https://github.com/StephenWalther)

> 이 자습서에서는 Stephen walther가 브라우저 정규식을 사용 하 여 경로 제약 조건을 만들면 일치 경로 요청 하는 방법을 제어 하는 방법을 보여 줍니다.

경로 제약 조건은 사용 하 여 특정 경로 일치 하는 브라우저 요청을 제한 합니다. 경로 제약 조건을 지정 하는 정규식을 사용할 수 있습니다.

예를 들어 정의 했다고 경로 목록 1에서 Global.asax 파일에 한다고 가정 합니다.

**1-Global.asax.vb 나열**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

목록 1 Product 라는 경로 포함 합니다. 브라우저 요청 목록 2에 포함 된 ProductController 매핑할 제품 경로 사용할 수 있습니다.

**Listing 2 - Controllers\ProductController.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

제품 컨트롤러에 의해 노출 Details() 작업 productId 라는 단일 매개 변수를 허용 하는지 확인 합니다. 이 매개 변수는 정수 매개 변수입니다.

목록 1에 정의 된 경로 다음 Url 중 하나라도 일치 합니다.

- / 제품/23
- / 제품/7

그러나 경로 다음 Url 일치도 합니다.

- / 제품/아니오
- / 제품/apple

Details() 작업에서 정수 매개 변수 예상 하기 때문에 정수 이외의 값을 포함 하는 요청을 수행 하면 오류가 발생 합니다. 예를 들어 URL /Product/apple 브라우저에 입력 하는 경우 다음 얻게 됩니다 오류 페이지 그림 1.

[![새 프로젝트 대화 상자](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)

**그림 01**: 분해 하는 페이지가 표시 ([클릭 하 여 큰 이미지 보기](creating-a-route-constraint-vb/_static/image2.png))

실제로 원하는 작업을 수행 하는 Url에는 적절 한 정수 productId와만 일치 합니다. 경로 일치 하는 Url을 제한 하는 제약 조건 경로 정의할 때 사용할 수 있습니다. 보기 3의 수정 된 제품 경로 정수 일치 하는 정규식 제약 조건을 포함 합니다.

**Listing 3 - Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

정규식 \d+ 정수 하나 이상을 찾습니다. 이 제약 조건으로 인해 다음 Url에 맞게 제품 경로:

- / 제품/3
- / 제품/8999

하지만 다음 Url에 없습니다.

- / 제품/apple
- / 제품

이러한 브라우저 요청을 다른 경로로 처리 또는 일치 하는 경로가 없을 경우는 *리소스를 찾을 수 없습니다* 오류가 반환 됩니다.

> [!div class="step-by-step"]
> [이전](creating-custom-routes-vb.md)
> [다음](creating-a-custom-route-constraint-vb.md)
