---
ms.openlocfilehash: b90e7963c5d9e5ef09fb519b72672c63bdffabee
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57188720"
---
> 앱 Id 데이터 저장소로 SQLite를 사용 하는 경우 일부 명령은 지원 되지 않습니다. 데이터베이스 엔진의 제한으로 인해 `Alter` 명령을 다음 예외를 throw 합니다.
>
> "System.NotSupportedException: SQLite 없으므로이 마이그레이션 작업 합니다. " 
>
> 해결 방법으로, 데이터베이스에서 테이블을 변경 하도록 Code First 마이그레이션을 실행 합니다.
