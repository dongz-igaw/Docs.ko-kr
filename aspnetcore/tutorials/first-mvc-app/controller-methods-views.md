---
title: "컨트롤러 메서드 및 보기"
author: rick-anderson
description: "컨트롤러 메서드, 보기 및 DataAnnotations 작업"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: c7313211-b271-4adf-bab8-8e72603cc0ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 8960853438d3227de3ef7c50936626149d8d5997
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/11/2017
---
# <a name="controller-methods-and-views"></a>컨트롤러 메서드 및 보기

작성자: [Rick Anderson](https://twitter.com/RickAndMSFT)

동영상 앱을 적절하게 시작했지만 프레젠테이션은 이상적이지 않습니다. 시간(아래 이미지에서 오전 12시)을 표시하지 않으려 하고 **ReleaseDate**는 두 단어이어야 합니다.

![인덱스 뷰: 릴리스 날짜는 한 단어(공백 없음)이며 모든 동영상 릴리스 날짜는 오전 12시를 표시합니다.](working-with-sql/_static/m55.png)

*Models/Movie.cs* 파일을 열고 아래 표시된 강조 표시된 줄을 추가합니다.

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]

빨간색 물결선 **> 빠른 작업 및 리팩터링**을 마우스 오른쪽 단추로 클릭합니다.

  ![바로 가기 메뉴는 **> 빠른 작업 및 리팩터링**을 표시합니다.](controller-methods-views/_static/qa.png)


`using System.ComponentModel.DataAnnotations;` 누르기

  ![목록 위쪽의 System.ComponentModel.DataAnnotations 사용](controller-methods-views/_static/da.png)

  Visual Studio는 `using System.ComponentModel.DataAnnotations;`를 추가합니다.

필요하지 않는 `using` 문을 제거합니다. 기본적으로 밝은 회색 글꼴로 표시됩니다. *Movie.cs* 파일에서 마우스 오른쪽 단추로 **> 제거 및 using 정렬**의 아무 위치나 클릭합니다.

![제거 및 using 정렬](controller-methods-views/_static/rm.png)

업데이트된 코드:

[!code-csharp[Main](./start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1)]

<!-- include start -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
[이전](working-with-sql.md)
[다음](search.md)  
