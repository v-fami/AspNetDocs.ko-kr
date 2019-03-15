---
ms.openlocfilehash: e5565381f44480e2531925717ee7815da92d1ff5
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57030040"
---
# <a name="update-the-generated-pages"></a><span data-ttu-id="8a34e-101">생성된 페이지 업데이트</span><span class="sxs-lookup"><span data-stu-id="8a34e-101">Update the generated pages</span></span>

<span data-ttu-id="8a34e-102">작성자: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8a34e-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8a34e-103">동영상 앱을 적절하게 시작했지만 프레젠테이션은 이상적이지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="8a34e-103">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="8a34e-104">시간(아래 이미지에서 오전 12시)을 표시하지 않으려 하고 **ReleaseDate**는 **Release Date**(두 단어)이어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a34e-104">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![동영상 데이터를 표시하는 크롬에서 열린 동영상 애플리케이션](../../tutorials/razor-pages/sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="8a34e-106">생성된 코드 업데이트</span><span class="sxs-lookup"><span data-stu-id="8a34e-106">Update the generated code</span></span>

<span data-ttu-id="8a34e-107">*Models/Movie.cs* 파일을 열고 다음 코드에 표시된 강조 표시된 줄을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="8a34e-107">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[](code/Models/Movie.cs?highlight=2,11-12)]