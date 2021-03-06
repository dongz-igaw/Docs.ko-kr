---
title: "묶음 및 축소에서 ASP.NET Core"
author: spboyer
description: 
keywords: "ASP.NET 코어 묶음 및 축소, CSS, JavaScript, 축소할 BuildBundlerMinifier"
ms.author: riande
manager: wpickett
ms.date: 02/28/2017
ms.topic: article
ms.assetid: d54230f9-8e5f-4861-a29c-1d3a14e0b0d9
ms.technology: aspnet
ms.prod: aspnet-core
uid: client-side/bundling-and-minification
ms.openlocfilehash: 11528cb2067ced79a09845f9ff78d897da033438
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/22/2017
---
# <a name="bundling-and-minification-in-aspnet-core"></a>묶음 및 축소에서 ASP.NET Core

묶음 및 축소는 다음 두 가지 기술 웹 응용 프로그램에 대 한 페이지 로드 성능 향상을 위해 ASP.NET에서 사용할 수 있습니다. 번들로 단일 파일에 여러 파일을 결합합니다. 다양 한 스크립트와 페이로드가 작을수록에 CSS를 서로 다른 코드 최적화를 수행 하는 축소 합니다. 함께 사용할 묶음 및 축소 부하 시간 여 성능을 향상 시킵니다 서버에 요청 수를 줄이고 CSS 및 JavaScript 파일과 같은 요청 된 자산의 크기를 줄이십시오.

이 문서에서는 묶음 및 축소, ASP.NET Core 응용 프로그램과 함께 이러한 기능은 사용할 수 있는 방법을 포함 하 여 사용의 이점에 설명 합니다.

## <a name="overview"></a>개요

ASP.NET Core 응용 프로그램에서 묶음 및 축소 클라이언트 리소스에 대 한 여러 옵션이 있습니다. MVC에 대 한 핵심 템플릿 구성 파일 및 BuildBundlerMinifier NuGet 패키지를 사용 하 여 기본적으로 솔루션을 제공 합니다. 와 같은 타사 도구 [Gulp](using-gulp.md) 및 [Grunt](using-grunt.md) 추가 워크플로 또는 복잡 한 프로세스 필요한 해야 동일한 작업을 수행할 수 있습니다. 디자인 타임 묶음 및 축소를 사용 하 여 응용 프로그램의 배포 하기 전에 축소 된 파일이 생성 됩니다. 묶음 및 축소 배포 하기 전에 서버 부하 감소의 이점이 있습니다. 그러나 해당 디자인 타임 번들로 인식 해야 하 고 축소 빌드 복잡성을 증가 하 고 정적 파일 에서만 작동 합니다.

묶음 및 축소는 주로 첫 번째 페이지 요청 부하 시간을 개선 합니다. 브라우저 캐시 자산 (JavaScript, CSS 및 이미지) 웹 페이지를 요청 되 면 페이지는 동일한 사이트 자산과 동일한 요청 하거나 묶음 및 축소 같은 페이지를 요청할 때 모든 성능 향상을 제공 하지 않습니다. 설정 하지 않으면는 사용자의 자산에서 올바르게 헤더 만료 묶음 및 축소를 사용 하지 않는, 브라우저의 새로 고침 추론 됩니다 자산 부실 몇 일 후 약관을 표시할 브라우저 각 자산에 대 한 유효성 검사 요청을 해야 합니다. 이 경우 묶음 및 축소 첫 번째 페이지 요청 후에 성능 향상을 제공합니다.

### <a name="bundling"></a>번들

번들로 결합 하거나 단일 파일에 여러 파일을 번들을 활용할 수 있는 기능입니다. 번들 파일 결합 하 여 여러 단일 파일로, 되므로 검색 하 고 웹 페이지 등의 웹 자산을 표시 하는 데 필요한 서버에 요청 수 감소 시킵니다. CSS, JavaScript 및 다른 번들을 만들 수 있습니다. 적은 파일 서버에 브라우저 또는 응용 프로그램을 제공 하는 서비스에서 더 적은 수의 HTTP 요청을 의미 합니다. 이 결과에서 첫 번째 페이지 부하 성능이 향상 되었습니다.

### <a name="minification"></a>축소

다양 한 요청 된 자산 (예: CSS, 이미지, JavaScript 파일)의 크기를 줄이기 위해 다른 코드 최적화를 수행 하는 축소 합니다. 축소의 일반적인 결과 불필요 한 공백 및 메모를 제거 하 고 한 문자를 변수 이름을 단축 포함 됩니다.

다음 JavaScript 함수가 있다고 합시다.

```javascript
AddAltToImg = function (imageTagAndImageID, imageContext) {
  ///<signature>
  ///<summary> Adds an alt tab to the image
  // </summary>
  //<param name="imgElement" type="String">The image selector.</param>
  //<param name="ContextForImage" type="String">The image context.</param>
  ///</signature>
  var imageElement = $(imageTagAndImageID, imageContext);
  imageElement.attr('alt', imageElement.attr('id').replace(/ID/, ''));
}
```

축소 후 함수가 다음과 감소 됩니다.

```javascript
AddAltToImg=function(t,a){var r=$(t,a);r.attr("alt",r.attr("id").replace(/ID/,""))};
```

주석 및 불필요 한 공백을 제거 하는 것 외에도 다음 매개 변수 및 변수 이름은 열의 이름을 (축약 됨) 다음과 같습니다.

원래 색 | 이름이 바뀜
--- | :---:
imageTagAndImageID | t
이미지 컨텍스트 | a
imageElement | r

## <a name="impact-of-bundling-and-minification"></a>묶음 및 축소의 영향

다음 표에서 모든 자산을 개별적으로 나열 하 고 간단한 웹 페이지에서 묶음 및 축소를 사용 하 여 간의 몇 가지 중요 한 차이점을 보여 줍니다.

작업 | M B/으로 | M B/없이 | 변경
--- | :---: | :---: | :---:
파일 요청 |7 | 18 | 157%
전송 KB | 156 | 264.68 | 70%
로드 시간 (밀리초) | 885 | 2360 | 167%

보낸 바이트 브라우저 요청에 적용 되는 HTTP 헤더와 함께 매우 자세한 정보는 번들 소프트웨어 크게 감소를 했습니다. 이 예제를 로컬로 실행 되지만 로드 시간이 상당히 개선을 보여 줍니다. 자산 묶음 및 축소를 사용 하 여 네트워크를 통해 전송 되 면 성능에 큰 향상 발생 합니다.

## <a name="using-bundling-and-minification-in-a-project"></a>묶음 및 축소를 사용 하 여 프로젝트의

MVC 프로젝트 템플릿은 제공는 `bundleconfig.json` 구성 파일을 각 번들에 대 한 옵션을 정의 합니다. 기본적으로 단일 번들 구성 사용자 지정 JavaScript에 대 한 정의 (`wwwroot/js/site.js`) 및 스타일 시트 (`wwwroot/css/site.css`) 파일입니다.

[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig.json)]

번들 옵션은 다음과 같습니다.

* outputFileName-출력 번들 파일의 이름입니다. 상대 경로 포함할 수 있습니다는 `bundleconfig.json` 파일입니다. **필수**
* inputFiles-함께 번들로 묶는 파일의 배열입니다. 이들은 구성 파일에 상대 경로입니다. **선택적**, * 빈 출력 파일에 빈 값이 발생 합니다. [와일드 카드 사용](http://www.tldp.org/LDP/abs/html/globbingref.html) 패턴이 지원 됩니다.
* 축소할-출력에 대 한 축소 옵션을 입력 합니다. **선택적**, *기본값-`minify: { enabled: true }`*
  * 구성 옵션은 출력 파일 형식을 사용할 수 있습니다.
    * [CSS Minifier입니다.](https://github.com/madskristensen/BundlerMinifier/wiki/cssminifier)
    * [JavaScript Minifier입니다.](https://github.com/madskristensen/BundlerMinifier/wiki)
    * [HTML Minifier입니다.](https://github.com/madskristensen/BundlerMinifier/wiki)
* includeInProject-프로젝트 파일 생성 된 파일을 추가 합니다. **선택적**, *기본값-false*
* -sourcemap을 내보내면 해당 번들된 파일에 대 한 소스 맵을 생성 합니다. **선택적**, *기본값-false*

### <a name="visual-studio-2015--2017"></a>Visual Studio 2015 / 2017

열기 `bundleconfig.json` Visual Studio에서 사용자 환경에 설치 해야 합니다; 확장명이 없는 묻는 메시지가 표시 됩니다 하나이 파일 형식을 지원할 수 있는지를 제안 합니다.

![BuildBundlerMinifier 확장 제안](../client-side/bundling-and-minification/_static/bundler-extension-suggestion.png)

확장 보기를 선택 하 고 설치는 **번들러 & Minifier** 확장 (Visual Studio가 필요 다시 시작)입니다.

![BuildBundlerMinifier 확장 제안](../client-side/bundling-and-minification/_static/view-extension.png)

다시 시작이 완료 되 면 축소 및 클라이언트 쪽 자산 번들의 프로세스를 실행 하 여 빌드를 구성 해야 합니다. 마우스 오른쪽 단추로 클릭는 `bundleconfig.json` 파일을 선택 *빌드에 사용 번들... *.

프로젝트를 빌드할 및 `bundleconfig.json` 구성에 따라 출력 파일을 생성 하기 위해 빌드 프로세스에 포함 됩니다.

```console
1>------ Build started: Project: BuildBundlerMinifierExample, Configuration: Debug Any CPU ------
1>
1>Bundler: Begin processing bundleconfig.json
1>Bundler: Done processing bundleconfig.json
1>BuildBundlerMinifierExample -> C:\BuildBundlerMinifierExample\bin\Debug\netcoreapp1.1\BuildBundlerMinifierExample.dll
========== Build: 1 succeeded or up-to-date, 0 failed, 0 skipped ==========
```

### <a name="visual-studio-code-or-command-line"></a>Visual Studio Code 또는 명령줄

Visual Studio 및 확장; GUI 제스처를 사용 하 여 묶음 및 축소 프로세스를 드라이브 그러나 동일한 기능을 사용할 수는 `dotnet` CLI 및 BuildBundlerMinifier NuGet 패키지 합니다.

프로젝트에 NuGet 패키지를 추가 합니다.

```console
dotnet add package BuildBundlerMinifier
```

종속성을 복원 합니다.

```console
dotnet restore
```

응용 프로그램을 빌드하십시오.

```console
dotnet build
```

빌드 명령의 출력은 축소 및/또는 구성에 따라 번들의 결과 보여줍니다.

```console
Microsoft (R) Build Engine version 15.1.545.13942
Copyright (C) Microsoft Corporation. All rights reserved.


  Bundler: Begin processing bundleconfig.json
     Minified wwwroot/css/site.min.css
  Bundler: Done processing bundleconfig.json
  BuildBundlerMinifierExample -> /BuildBundlerMinifierExample/bin/Debug/netcoreapp1.0/BuildBundlerMinifierExample.dll
```

## <a name="adding-files"></a>파일 추가

이 예제에서는 CSS 파일을 추가로 호출 추가 됩니다 `custom.css` 묶음 및 축소를 위해 구성 및 `site.css`단일에서 결과, `site.min.css`합니다.

custom.css

```css
.about, [role=main], [role=complementary]
{
    margin-top: 60px;
}

footer
{
    margin-top: 10px;
}
```

상대 경로를 추가할 `bundleconfig.json`합니다.

[!code-json[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/bundleconfig2.json)]

> [!NOTE]
> 또는 와일드 카드 사용 패턴을 사용할 수 없습니다- `"inputFiles": ["wwwroot/**/*(*.css|!(*.min.css)"]` 는 모든 CSS 파일을 가져오고 제외 되어 축소 된 파일 패턴입니다.

응용 프로그램을 빌드합니다 열면 및 `site.min.css`는 이제 알 수의 내용을 `custom.css` 파일의 끝에 추가 된 합니다.

## <a name="controlling-bundling-and-minification"></a>묶음 및 축소를 제어합니다.

일반적으로 프로덕션 환경에만 응용 프로그램의 번들로 묶은 및 축소 된 파일을 사용 하려면. 개발 하는 동안 앱을 쉽게 디버깅할 수 있도록 원본 파일을 사용 하려면.

스크립트와 레이아웃 페이지에서 환경 태그 도우미를 사용 하 여 페이지에 포함할 CSS 파일을 지정할 수 있습니다 (참조 [태그 도우미](../mvc/views/tag-helpers/index.md)). 환경 태그 도우미 특정 환경에서 실행 하는 경우 해당 콘텐츠를 렌더링만 합니다. 참조 [여러 환경 작업](../fundamentals/environments.md) 지정 현재 환경에 대 한 합니다.

실행할 때 다음과 같은 환경 태그 처리 되지 않은 CSS 파일을 렌더링 합니다는 `Development` 환경:

[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=3&range=9-12)]

실행 하는 경우에이 환경 태그는 번들 및 축소 된 CSS 파일을 렌더링 합니다 `Production` 또는 `Staging`:

[!code-html[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/Views/Shared/_Layout.cshtml?highlight=5&range=13-18)]

## <a name="consuming-bundleconfigjson-from-gulp"></a>Bundleconfig.json Gulp에서 사용합니다.

예: 이미지 처리, 캐시 busting, CDN assest 처리 등의 추가 프로세스를 해야 하는 응용 프로그램 묶음 및 축소 과정 번들 및 Minify 프로세스 Gulp를 변환할 수 있습니다.

> [!NOTE]
> 변환 옵션 Visual Studio 2015 및 2017 에서만 사용할 수 있습니다.

마우스 오른쪽 단추로 클릭는 `bundleconfig.json` 선택 **Gulp 변환... **. 자동으로 생성 됩니다는 `gulpfile.js` 하 고 필요한 npm 패키지를 설치 합니다.

![Gulp를 변환](../client-side/bundling-and-minification/_static/convert-togulp.png)

`gulpfile.js` 읽기 생성 되는 `bundleconfig.json` 파일 구성을 위해, 따라서를 계속 하려면는 입/출력 및 설정에 사용할 합니다.

[!code-javascript[Main](../client-side/bundling-and-minification/samples/BuildBundlerMinifierExample/gulpfile.js)]

Gulp를 Visual Studio 2017에서 프로젝트가 빌드될 때 사용 하도록 설정 하려면 *.csproj 파일에 다음을 추가 합니다.

```xml
<Target Name="MyPreCompileTarget" BeforeTargets="Build">
    <Exec Command="gulp min" />
</Target>
```

Visual Studio 2015에서 프로젝트가 빌드될 때 Gulp를 사용 하려면 다음을 추가 `project.json` 파일:

```json
"scripts": {
    "precompile": "gulp min"
}
```

## <a name="additional-resources"></a>추가 리소스

* [Gulp 사용](using-gulp.md)
* [Grunt 사용](using-grunt.md)
* [여러 환경 사용](../fundamentals/environments.md)
* [태그 도우미](../mvc/views/tag-helpers/index.md)
