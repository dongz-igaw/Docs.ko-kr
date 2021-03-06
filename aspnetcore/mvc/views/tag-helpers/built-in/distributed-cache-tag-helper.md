---
title: "분산 캐시 태그 도우미 | Microsoft Docs"
author: pkellner
description: "캐시 태그 도우미를 사용 하는 방법을 보여 줍니다."
keywords: "ASP.NET Core, 태그 도우미"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a022
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: 462c3775677924fc7b9b715cd6de75fe53ada89e
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/28/2017
---
# <a name="distributed-cache-tag-helper"></a>분산된 캐시 태그 도우미

작성자: [Peter Kellner](http://peterkellner.net) 


분산 캐시 태그 도우미 분산된 캐시 원본에 해당 콘텐츠를 캐시 하 여 ASP.NET Core 응용 프로그램의 성능이 크게 향상 하는 기능을 제공 합니다.

분산 캐시 태그 도우미 캐시 태그 도우미와 같은 기본 클래스에서 상속합니다.  캐시 태그 도우미에 연결 된 모든 특성은 Distributed 태그 도우미 에서도 작동 합니다.


분산 캐시 태그 도우미 뒤에 오는 **명시적 종속성 원칙** 라고 **생성자 삽입**합니다.  특히,는 `IDistributedCache` 인터페이스 컨테이너 분산 캐시 태그 도우미의 생성자에 전달 됩니다.  구체적 구현 없는 경우 `IDistributedCache` 에 생성 되었음을 `ConfigureServices`startup.cs에서 일반적으로 발견 차례로 분산 캐시 태그 도우미 기본 캐시 태그 도우미와 캐시 된 데이터를 저장 하기 위한 동일한 메모리 내 공급자를 사용 합니다.

## <a name="distributed-cache-tag-helper-attributes"></a>분산 캐시 태그 도우미 특성

- - -

### <a name="enabled-expires-on-expires-after-expires-sliding-vary-by-header-vary-by-query-vary-by-route-vary-by-cookie-vary-by-user-vary-by-priority"></a>만료 된 이후에 만료 만료-슬라이딩 활성화 vary 헤더 다 쿼리 다 경로 다 쿠키 다 사용자 다 by 우선 순위

정의 대 한 캐시 태그 도우미를 참조 하십시오. 분산된 캐시 태그 도우미 있으므로 이러한 모든 특성은 캐시 태그 도우미에서 일반적인 캐시 태그 도우미와 같은 클래스에서 상속 합니다.

- - -

### <a name="name-required"></a>이름 (필수)

| 특성 유형    | 예제 값     |
|----------------   |----------------   |
| string    | "my-distributed-cache-unique-key-101"     |

필요한 `name` 특성은 분산 캐시 태그 도우미의 각 인스턴스에 대해 저장 된 캐시에 키로 사용 합니다.  기본 캐시 태그 도우미 Razor 페이지 이름 및 razor 페이지의 태그 도우미의 위치에 따라 각 캐시 태그 도우미 인스턴스 키를 할당 하는와 달리 Distributed 캐시 태그 도우미만 기반 키 특성`name`

사용 예제를 보려면:

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>분산 캐시 태그 도우미 IDistributedCache 구현

두 개의 구현이 없는 `IDistributedCache` ASP.NET Core에서 기본적으로 제공 합니다.  에 따라 하나 **Sql Server** 기반 다른 **Redis**합니다. 명명 된 "작업 분산된 캐시" 아래에 참조 된 리소스의이 구현은 세부 정보를 찾을 수 있습니다. 두 구현 모두의 인스턴스를 설정할 포함 `IDistributedCache` 에서 ASP.NET Core **startup.cs**합니다.

특정 구현을 사용과 특별히 관련 된 태그 특성이 없으면 `IDistributedCache`합니다.



- - -



## <a name="additional-resources"></a>추가 리소스

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection#service-lifetimes-and-registration-options>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
