---
title: '빠른 시작: C#용 Text Analytics 클라이언트 라이브러리 | Microsoft Docs'
description: C#용 Text Analytics 클라이언트 라이브러리 시작
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 10/07/2020
ms.author: aahi
ms.reviewer: assafi
ms.openlocfilehash: b5249850a572090a32b0d3515cbeaec9bfb112ae
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91977835"
---
<a name="HOLTop"></a>

# <a name="version-31-preview"></a>[버전 3.1 미리 보기](#tab/version-3-1)

[v3.1 참조 설명서](https://docs.microsoft.com/dotnet/api/azure.ai.textanalytics?view=azure-dotnet-previews&preserve-view=true) | [v3.1 라이브러리 소스 코드](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics) | [v3.1 패키지(NuGet)](https://www.nuget.org/packages/Azure.AI.TextAnalytics/5.1.0-beta.1) | [v3.1 샘플](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/textanalytics/Azure.AI.TextAnalytics/samples)

# <a name="version-30"></a>[버전 3.0](#tab/version-3)

[v3 참조 설명서](https://aka.ms/azsdk-net-textanalytics-ref-docs) | [v3 라이브러리 소스 코드](https://github.com/Azure/azure-sdk-for-net/tree/Azure.AI.TextAnalytics_5.0.0/sdk/textanalytics/Azure.AI.TextAnalytics) | [v3 패키지(NuGet)](https://www.nuget.org/packages/Azure.AI.TextAnalytics) | [v3 샘플](https://github.com/Azure/azure-sdk-for-net/tree/Azure.AI.TextAnalytics_5.0.0/sdk/textanalytics/Azure.AI.TextAnalytics/samples)

# <a name="version-21"></a>[버전 2.1](#tab/version-2)

[v2 참조 설명서](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/textanalytics) | [v2 라이브러리 소스 코드](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/cognitiveservices/Language.TextAnalytics) | [v2 패키지(NuGet)](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics/) | [v2 샘플](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples)

---

## <a name="prerequisites"></a>사전 요구 사항

* Azure 구독 - [체험 구독 만들기](https://azure.microsoft.com/free/cognitive-services)
* [Visual Studio IDE](https://visualstudio.microsoft.com/vs/)
* Azure 구독을 보유한 후에는 Azure Portal에서 <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title="Text Analytics 리소스를 만들어"  target="_blank">Text Analytics 리소스를 만들어<span class="docon docon-navigate-external x-hidden-focus"></span></a> 키와 엔드포인트를 가져옵니다.  배포 후 **리소스로 이동**을 클릭합니다.
    * 애플리케이션을 Text Analytics API에 연결하려면 만든 리소스의 키와 엔드포인트가 필요합니다. 이 빠른 시작의 뒷부분에 나오는 코드에 키와 엔드포인트를 붙여넣습니다.
    * 평가판 가격 책정 계층(`F0`)을 통해 서비스를 사용해보고, 나중에 프로덕션용 유료 계층으로 업그레이드할 수 있습니다.

## <a name="setting-up"></a>설치

### <a name="create-a-new-net-core-application"></a>새 .NET Core 애플리케이션 만들기

Visual Studio IDE를 사용하여 새 .NET Core 콘솔 앱을 만듭니다. 이렇게 하면 *program.cs*라는 단일 C# 원본 파일이 포함된 "Hello World" 프로젝트가 생성됩니다.

# <a name="version-31-preview"></a>[버전 3.1 미리 보기](#tab/version-3-1)

**솔루션 탐색기**에서 솔루션을 마우스 오른쪽 단추로 클릭하고 **NuGet 패키지 관리**를 선택하여 클라이언트 라이브러리를 설치합니다. 열리는 패키지 관리자에서 **찾아보기**를 선택하고 `Azure.AI.TextAnalytics`를 검색합니다. **사전 출시 포함** 상자를 선택하고 버전 `5.1.0-beta.1`을 선택한 다음, **설치**를 선택합니다. [패키지 관리자 콘솔](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package)을 사용할 수도 있습니다.

# <a name="version-30"></a>[버전 3.0](#tab/version-3)

**솔루션 탐색기**에서 솔루션을 마우스 오른쪽 단추로 클릭하고 **NuGet 패키지 관리**를 선택하여 클라이언트 라이브러리를 설치합니다. 열리는 패키지 관리자에서 **찾아보기**를 선택하고 `Azure.AI.TextAnalytics`를 검색합니다. `5.0.0` 버전, **설치**를 차례로 선택합니다. [패키지 관리자 콘솔](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package)을 사용할 수도 있습니다.


> [!TIP]
> 한 번에 전체 빠른 시작 코드 파일을 보시겠습니까? [GitHub에서](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/dotnet/TextAnalytics/program.cs) 찾을 수 있으며 이 빠른 시작의 코드 예제를 포함합니다. 

# <a name="version-21"></a>[버전 2.1](#tab/version-2)

**솔루션 탐색기**에서 솔루션을 마우스 오른쪽 단추로 클릭하고 **NuGet 패키지 관리**를 선택하여 클라이언트 라이브러리를 설치합니다. 열리는 패키지 관리자에서 **찾아보기**를 선택하고 `Microsoft.Azure.CognitiveServices.Language.TextAnalytics`를 검색합니다. 해당 항목을 클릭한 다음, **설치**를 클릭합니다. [패키지 관리자 콘솔](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-powershell#find-and-install-a-package)을 사용할 수도 있습니다.

> [!TIP]
> 한 번에 전체 빠른 시작 코드 파일을 보시겠습니까? [GitHub에서](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/samples/TextAnalytics/synchronous/Program.cs) 찾을 수 있으며 이 빠른 시작의 코드 예제를 포함합니다. 

---

# <a name="version-31-preview"></a>[버전 3.1 미리 보기](#tab/version-3-1)

*program.cs* 파일을 열고 다음 `using` 지시문을 추가합니다.

```csharp
using Azure;
using System;
using System.Globalization;
using Azure.AI.TextAnalytics;
```

애플리케이션의 `Program` 클래스에서 리소스의 키 및 엔드포인트에 대한 변수를 만듭니다.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```csharp
private static readonly AzureKeyCredential credentials = new AzureKeyCredential("<replace-with-your-text-analytics-key-here>");
private static readonly Uri endpoint = new Uri("<replace-with-your-text-analytics-endpoint-here>");
```

애플리케이션의 `Main` 메서드를 바꿉니다. 나중에 여기서 호출되는 메서드를 정의합니다.

```csharp
static void Main(string[] args)
{
    var client = new TextAnalyticsClient(endpoint, credentials);
    // You will implement these methods later in the quickstart.
    SentimentAnalysisExample(client);
    SentimentAnalysisWithOpinionMiningExample(client);
    LanguageDetectionExample(client);
    EntityRecognitionExample(client);
    EntityLinkingExample(client);
    RecognizePIIExample(client);
    KeyPhraseExtractionExample(client);

    Console.Write("Press any key to exit.");
    Console.ReadKey();
}
```

# <a name="version-30"></a>[버전 3.0](#tab/version-3)

*program.cs* 파일을 열고 다음 `using` 지시문을 추가합니다.

```csharp
using Azure;
using System;
using System.Globalization;
using Azure.AI.TextAnalytics;
```

애플리케이션의 `Program` 클래스에서 리소스의 키 및 엔드포인트에 대한 변수를 만듭니다.

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```csharp
private static readonly AzureKeyCredential credentials = new AzureKeyCredential("<replace-with-your-text-analytics-key-here>");
private static readonly Uri endpoint = new Uri("<replace-with-your-text-analytics-endpoint-here>");
```

애플리케이션의 `Main` 메서드를 바꿉니다. 나중에 여기서 호출되는 메서드를 정의합니다.

```csharp
static void Main(string[] args)
{
    var client = new TextAnalyticsClient(endpoint, credentials);
    // You will implement these methods later in the quickstart.
    SentimentAnalysisExample(client);
    LanguageDetectionExample(client);
    EntityRecognitionExample(client);
    EntityLinkingExample(client);
    KeyPhraseExtractionExample(client);

    Console.Write("Press any key to exit.");
    Console.ReadKey();
}
```

# <a name="version-21"></a>[버전 2.1](#tab/version-2)

*program.cs* 파일을 열고 다음 `using` 지시문을 추가합니다.

[!code-csharp[Import directives](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=imports)]

애플리케이션의 `Program` 클래스에서 리소스의 키 및 엔드포인트에 대한 변수를 만듭니다. 

[!INCLUDE [text-analytics-find-resource-information](../find-azure-resource-info.md)]

```csharp
private static readonly string key = "<replace-with-your-text-analytics-key-here>";
private static readonly string endpoint = "<replace-with-your-text-analytics-endpoint-here>";
```

애플리케이션의 `Main` 메서드를 바꿉니다. 나중에 여기서 호출되는 메서드를 정의합니다.

[!code-csharp[main method](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=main)]

---

## <a name="object-model"></a>개체 모델

Text Analytics 클라이언트는 키를 사용하여 Azure를 인증하고 텍스트를 단일 문자열 또는 일괄 처리로 허용하는 함수를 제공하는 `TextAnalyticsClient` 개체입니다. 텍스트는 API에 동기식 또는 비동기식으로 보낼 수 있습니다. 응답 개체에는 보내는 각 문서에 대한 분석 정보가 포함됩니다. 

서비스 버전 `3.x`를 사용하는 경우 선택적 `TextAnalyticsClientOptions` 인스턴스를 사용하여 다양한 기본 설정(예: 기본 언어 또는 국가/지역 힌트)으로 클라이언트를 초기화할 수 있습니다. Azure Active Directory 토큰을 사용하여 인증할 수도 있습니다. 

## <a name="code-examples"></a>코드 예제

* [감정 분석](#sentiment-analysis)
* [의견 마이닝](#opinion-mining)
* [언어 감지](#language-detection)
* [명명된 엔터티 인식](#named-entity-recognition-ner)
* [엔터티 연결](#entity-linking)
* [핵심 구 추출](#key-phrase-extraction)

## <a name="authenticate-the-client"></a>클라이언트 인증

# <a name="version-31-preview"></a>[버전 3.1 미리 보기](#tab/version-3-1)

이전의 main 메서드에서 엔드포인트와 자격 증명을 사용하여 새 클라이언트 개체를 만들었는지 확인합니다.

```csharp
var client = new TextAnalyticsClient(endpoint, credentials);
```

# <a name="version-30"></a>[버전 3.0](#tab/version-3)

이전의 main 메서드에서 엔드포인트와 자격 증명을 사용하여 새 클라이언트 개체를 만들었는지 확인합니다.

```csharp
var client = new TextAnalyticsClient(endpoint, credentials);
```

# <a name="version-21"></a>[버전 2.1](#tab/version-2)

자격 증명을 저장하여 클라이언트의 요청에 추가하는 새 `ApiKeyServiceClientCredentials` 클래스를 만듭니다. 이 안에 `Ocp-Apim-Subscription-Key` 헤더에 키를 추가하는 `ProcessHttpRequestAsync()`에 대한 재정의를 만듭니다.

[!code-csharp[Client class](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=clientClass)]

엔드포인트 및 키를 포함하는 `ApiKeyServiceClientCredentials` 개체를 사용하여 [TextAnalyticsClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclient) 개체를 인스턴스화하는 메서드를 만듭니다.

[!code-csharp[Client authentication](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=authentication)]

---

## <a name="sentiment-analysis"></a>정서 분석

# <a name="version-31-preview"></a>[버전 3.1 미리 보기](#tab/version-3-1)

앞에서 만든 클라이언트를 사용하고 해당 `AnalyzeSentiment()` 함수를 호출하는 `SentimentAnalysisExample()`이라는 새 함수를 만듭니다. 반환된 `Response<DocumentSentiment>` 개체에는 전체 입력 문서의 감정 레이블과 점수가 포함되며, 성공하는 경우 각 문장에 대한 감정 분석도 포함됩니다. 오류가 발생하면 `RequestFailedException`이 throw됩니다.

```csharp
static void SentimentAnalysisExample(TextAnalyticsClient client)
{
    string inputText = "I had the best day of my life. I wish you were there with me.";
    DocumentSentiment documentSentiment = client.AnalyzeSentiment(inputText);
    Console.WriteLine($"Document sentiment: {documentSentiment.Sentiment}\n");

    foreach (var sentence in documentSentiment.Sentences)
    {
        Console.WriteLine($"\tText: \"{sentence.Text}\"");
        Console.WriteLine($"\tSentence sentiment: {sentence.Sentiment}");
        Console.WriteLine($"\tPositive score: {sentence.ConfidenceScores.Positive:0.00}");
        Console.WriteLine($"\tNegative score: {sentence.ConfidenceScores.Negative:0.00}");
        Console.WriteLine($"\tNeutral score: {sentence.ConfidenceScores.Neutral:0.00}\n");
    }
}
```

### <a name="output"></a>출력

```console
Document sentiment: Positive

        Text: "I had the best day of my life."
        Sentence sentiment: Positive
        Positive score: 1.00
        Negative score: 0.00
        Neutral score: 0.00

        Text: "I wish you were there with me."
        Sentence sentiment: Neutral
        Positive score: 0.21
        Negative score: 0.02
        Neutral score: 0.77
```

### <a name="opinion-mining"></a>의견 마이닝

이전에 만든 클라이언트를 사용하고 `AdditionalSentimentAnalyses.OpinionMining` 옵션을 사용하여 해당 `AnalyzeSentimentBatch()` 함수를 호출하는 `SentimentAnalysisWithOpinionMiningExample()`이라는 새 함수를 만듭니다. 반환된 `AnalyzeSentimentResultCollection` 개체에는 `Response<DocumentSentiment>`를 나타내는 `AnalyzeSentimentResult`의 컬렉션이 포함됩니다. `SentimentAnalysis()`와 `SentimentAnalysisWithOpinionMiningExample()` 간의 차이점은 후자는 각 문장에 `MinedOpinion`를 포함하여 분석된 측면과 관련된 의견을 표시한다는 것입니다. 오류가 발생하면 `RequestFailedException`이 throw됩니다.

```csharp
static void SentimentAnalysisWithOpinionMiningExample(TextAnalyticsClient client)
{
    var documents = new List<string>
    {
        "The food and service were unacceptable, but the concierge were nice.",
        "The rooms were beautiful. The AC was good and quiet.",
        "The breakfast was good, but the toilet was smelly.",
        "Loved this hotel - good breakfast - nice shuttle service - clean rooms.",
        "I had a great unobstructed view of the Microsoft campus.",
        "Nice rooms but bathrooms were old and the toilet was dirty when we arrived.",
        "We changed rooms as the toilet smelled."
    };

    AnalyzeSentimentResultCollection reviews = client.AnalyzeSentimentBatch(documents, options: new AnalyzeSentimentOptions()
    {
        AdditionalSentimentAnalyses = AdditionalSentimentAnalyses.OpinionMining
    });

    foreach (AnalyzeSentimentResult review in reviews)
    {
        Console.WriteLine($"Document sentiment: {review.DocumentSentiment.Sentiment}\n");
        foreach (SentenceSentiment sentence in review.DocumentSentiment.Sentences)
        {
            Console.WriteLine($"\tText: \"{sentence.Text}\"");
            Console.WriteLine($"\tSentence sentiment: {sentence.Sentiment}");
            Console.WriteLine($"\tPositive score: {sentence.ConfidenceScores.Positive:0.00}");
            Console.WriteLine($"\tNegative score: {sentence.ConfidenceScores.Negative:0.00}");
            Console.WriteLine($"\tNeutral score: {sentence.ConfidenceScores.Neutral:0.00}\n");

            foreach (MinedOpinion minedOpinion in sentence.MinedOpinions)
            {
                Console.WriteLine($"\tAspect: {minedOpinion.Aspect.Text}, Value: {minedOpinion.Aspect.Sentiment}");
                foreach (OpinionSentiment opinion in minedOpinion.Opinions)
                {
                    Console.WriteLine($"\t\tRelated Opinion: {opinion.Text}, Value: {opinion.Sentiment}");
                }
            }
        }
        Console.WriteLine($"\n");
    }
}
```

### <a name="output"></a>출력

```console
Document sentiment: Positive

        Text: "The food and service were unacceptable, but the concierge were nice."
        Sentence sentiment: Positive
        Positive score: 0.84
        Negative score: 0.16
        Neutral score: 0.00

        Aspect: food, Value: Negative
                Related Opinion: unacceptable, Value: Negative
        Aspect: service, Value: Negative
                Related Opinion: unacceptable, Value: Negative
        Aspect: concierge, Value: Positive
                Related Opinion: nice, Value: Positive


Document sentiment: Positive

        Text: "The rooms were beautiful."
        Sentence sentiment: Positive
        Positive score: 1.00
        Negative score: 0.00
        Neutral score: 0.00

        Aspect: rooms, Value: Positive
                Related Opinion: beautiful, Value: Positive
        Text: "The AC was good and quiet."
        Sentence sentiment: Positive
        Positive score: 1.00
        Negative score: 0.00
        Neutral score: 0.00

        Aspect: AC, Value: Positive
                Related Opinion: good, Value: Positive
                Related Opinion: quiet, Value: Positive


Document sentiment: Negative

        Text: "The breakfast was good, but the toilet was smelly."
        Sentence sentiment: Negative
        Positive score: 0.01
        Negative score: 0.99
        Neutral score: 0.00

        Aspect: breakfast, Value: Positive
                Related Opinion: good, Value: Positive
        Aspect: toilet, Value: Negative
                Related Opinion: smelly, Value: Negative


Document sentiment: Positive

        Text: "Loved this hotel - good breakfast - nice shuttle service - clean rooms."
        Sentence sentiment: Positive
        Positive score: 1.00
        Negative score: 0.00
        Neutral score: 0.00

        Aspect: hotel, Value: Positive
                Related Opinion: good, Value: Positive
        Aspect: breakfast, Value: Positive
                Related Opinion: nice, Value: Positive
        Aspect: shuttle service, Value: Positive
                Related Opinion: loved, Value: Positive
        Aspect: rooms, Value: Positive
                Related Opinion: good, Value: Positive
                Related Opinion: nice, Value: Positive
                Related Opinion: clean, Value: Positive
                Related Opinion: loved, Value: Positive


Document sentiment: Positive

        Text: "I had a great unobstructed view of the Microsoft campus."
        Sentence sentiment: Positive
        Positive score: 1.00
        Negative score: 0.00
        Neutral score: 0.00

        Aspect: view, Value: Positive
                Related Opinion: great, Value: Positive
                Related Opinion: unobstructed, Value: Positive


Document sentiment: Negative

        Text: "Nice rooms but bathrooms were old and the toilet was dirty when we arrived."
        Sentence sentiment: Negative
        Positive score: 0.00
        Negative score: 1.00
        Neutral score: 0.00

        Aspect: rooms, Value: Positive
                Related Opinion: nice, Value: Positive
        Aspect: bathrooms, Value: Negative
                Related Opinion: old, Value: Negative
        Aspect: toilet, Value: Negative
                Related Opinion: dirty, Value: Negative


Document sentiment: Neutral

        Text: "We changed rooms as the toilet smelled."
        Sentence sentiment: Neutral
        Positive score: 0.01
        Negative score: 0.03
        Neutral score: 0.96

        Aspect: rooms, Value: Negative
                Related Opinion: smelled, Value: Negative
        Aspect: toilet, Value: Negative
                Related Opinion: smelled, Value: Negative
```

# <a name="version-30"></a>[버전 3.0](#tab/version-3)

앞에서 만든 클라이언트를 사용하고 해당 `AnalyzeSentiment()` 함수를 호출하는 `SentimentAnalysisExample()`이라는 새 함수를 만듭니다. 반환된 `Response<DocumentSentiment>` 개체에는 전체 입력 문서의 감정 레이블과 점수가 포함되며, 성공하는 경우 각 문장에 대한 감정 분석도 포함됩니다. 오류가 발생하면 `RequestFailedException`이 throw됩니다.

```csharp
static void SentimentAnalysisExample(TextAnalyticsClient client)
{
    string inputText = "I had the best day of my life. I wish you were there with me.";
    DocumentSentiment documentSentiment = client.AnalyzeSentiment(inputText);
    Console.WriteLine($"Document sentiment: {documentSentiment.Sentiment}\n");

    foreach (var sentence in documentSentiment.Sentences)
    {
        Console.WriteLine($"\tText: \"{sentence.Text}\"");
        Console.WriteLine($"\tSentence sentiment: {sentence.Sentiment}");
        Console.WriteLine($"\tPositive score: {sentence.ConfidenceScores.Positive:0.00}");
        Console.WriteLine($"\tNegative score: {sentence.ConfidenceScores.Negative:0.00}");
        Console.WriteLine($"\tNeutral score: {sentence.ConfidenceScores.Neutral:0.00}\n");
    }
}
```

### <a name="output"></a>출력

```console
Document sentiment: Positive

        Text: "I had the best day of my life."
        Sentence sentiment: Positive
        Positive score: 1.00
        Negative score: 0.00
        Neutral score: 0.00

        Text: "I wish you were there with me."
        Sentence sentiment: Neutral
        Positive score: 0.21
        Negative score: 0.02
        Neutral score: 0.77
```

# <a name="version-21"></a>[버전 2.1](#tab/version-2)

앞에서 만든 클라이언트를 사용하고 해당 [Sentiment()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.sentiment) 함수를 호출하는 `SentimentAnalysisExample()`이라는 새 함수를 만듭니다. 반환된 [SentimentResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.sentimentresult) 개체에는 성공하면 감정 `Score`가 포함되고 그렇지 않으면 `errorMessage`가 포함됩니다. 

점수가 0에 가까울수록 부정적인 감정을 나타내고, 1에 가까울수록 긍정적인 감정을 나타냅니다.

[!code-csharp[Sentiment analysis](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=sentiment)]

```console
Sentiment Score: 0.87
```

---

## <a name="language-detection"></a>언어 검색

# <a name="version-31-preview"></a>[버전 3.1 미리 보기](#tab/version-3-1)


앞에서 만든 클라이언트를 사용하고 해당 `DetectLanguage()` 함수를 호출하는 `LanguageDetectionExample()`이라는 새 함수를 만듭니다. 반환된 `Response<DetectedLanguage>` 개체에는 해당 이름 및 ISO-6391 코드와 함께 검색된 언어가 포함됩니다. 오류가 발생하면 `RequestFailedException`이 throw됩니다.

> [!Tip]
> 일부 경우에는 입력에 따라 언어를 명확하게 구분하는 것이 어려울 수 있습니다. 2자로 된 국가/지역 코드는 `countryHint` 매개 변수를 사용하여 지정할 수 있습니다. 기본적으로 API는 "US"를 기본 countryHint로 사용합니다. 이 동작을 제거하려면 이 값을 빈 문자열의 `countryHint = ""`로 설정하여 이 매개 변수를 다시 설정하면 됩니다. 다른 기본값을 설정하려면 `TextAnalyticsClientOptions.DefaultCountryHint` 속성을 설정하고 클라이언트를 초기화 중에 이를 전달합니다.

```csharp
static void LanguageDetectionExample(TextAnalyticsClient client)
{
    DetectedLanguage detectedLanguage = client.DetectLanguage("Ce document est rédigé en Français.");
    Console.WriteLine("Language:");
    Console.WriteLine($"\t{detectedLanguage.Name},\tISO-6391: {detectedLanguage.Iso6391Name}\n");
}
```

### <a name="output"></a>출력

```console
Language:
        French, ISO-6391: fr
```

# <a name="version-30"></a>[버전 3.0](#tab/version-3)


앞에서 만든 클라이언트를 사용하고 해당 `DetectLanguage()` 함수를 호출하는 `LanguageDetectionExample()`이라는 새 함수를 만듭니다. 반환된 `Response<DetectedLanguage>` 개체에는 해당 이름 및 ISO-6391 코드와 함께 검색된 언어가 포함됩니다. 오류가 발생하면 `RequestFailedException`이 throw됩니다.

> [!Tip]
> 일부 경우에는 입력에 따라 언어를 명확하게 구분하는 것이 어려울 수 있습니다. 2자로 된 국가/지역 코드는 `countryHint` 매개 변수를 사용하여 지정할 수 있습니다. 기본적으로 API는 "US"를 기본 countryHint로 사용합니다. 이 동작을 제거하려면 이 값을 빈 문자열의 `countryHint = ""`로 설정하여 이 매개 변수를 다시 설정하면 됩니다. 다른 기본값을 설정하려면 `TextAnalyticsClientOptions.DefaultCountryHint` 속성을 설정하고 클라이언트를 초기화 중에 이를 전달합니다.

```csharp
static void LanguageDetectionExample(TextAnalyticsClient client)
{
    DetectedLanguage detectedLanguage = client.DetectLanguage("Ce document est rédigé en Français.");
    Console.WriteLine("Language:");
    Console.WriteLine($"\t{detectedLanguage.Name},\tISO-6391: {detectedLanguage.Iso6391Name}\n");
}
```

### <a name="output"></a>출력

```console
Language:
        French, ISO-6391: fr
```

# <a name="version-21"></a>[버전 2.1](#tab/version-2)

앞에서 만든 클라이언트를 사용하고 해당 [DetectLanguage()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.detectlanguage#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_DetectLanguage_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) 함수를 호출하는 `languageDetectionExample()`이라는 새 함수를 만듭니다. 반환된 [LanguageResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.languageresult) 개체에는 성공하면 `DetectedLanguages`에서 검색된 언어 목록이 포함되고 그렇지 않으면 `errorMessage`가 포함됩니다. 처음 반환된 언어를 출력합니다.

> [!Tip]
> 일부 경우에는 입력에 따라 언어를 명확하게 구분하는 것이 어려울 수 있습니다. 2자로 된 국가/지역 코드는 `countryHint` 매개 변수를 사용하여 지정할 수 있습니다. 기본적으로 API는 "US"를 기본 countryHint로 사용합니다. 이 동작을 제거하려면 이 값을 빈 문자열의 `countryHint = ""`로 설정하여 이 매개 변수를 다시 설정하면 됩니다.

[!code-csharp[Language Detection example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=languageDetection)]

### <a name="output"></a>출력

```console
Language: English
```

---

## <a name="named-entity-recognition-ner"></a>NER(명명된 엔터티 인식)

# <a name="version-31-preview"></a>[버전 3.1 미리 보기](#tab/version-3-1)


앞에서 만든 클라이언트를 사용하는 `EntityRecognitionExample()`이라는 새 함수를 만들고, `RecognizeEntities()` 함수를 호출하여 결과를 반복합니다. 반환된 `Response<CategorizedEntityCollection>` 개체에는 검색된 엔터티 `CategorizedEntity`의 컬렉션이 포함됩니다. 오류가 발생하면 `RequestFailedException`이 throw됩니다.

```csharp
static void EntityRecognitionExample(TextAnalyticsClient client)
{
    var response = client.RecognizeEntities("I had a wonderful trip to Seattle last week.");
    Console.WriteLine("Named Entities:");
    foreach (var entity in response.Value)
    {
        Console.WriteLine($"\tText: {entity.Text},\tCategory: {entity.Category},\tSub-Category: {entity.SubCategory}");
        Console.WriteLine($"\t\tScore: {entity.ConfidenceScore:F2},\tLength: {entity.Length},\tOffset: {entity.Offset}\n");
    }
}
```

### <a name="output"></a>출력

```console
Named Entities:
        Text: trip,     Category: Event,        Sub-Category:
                Score: 0.61,    Length: 4,      Offset: 18

        Text: Seattle,  Category: Location,     Sub-Category: GPE
                Score: 0.82,    Length: 7,      Offset: 26

        Text: last week,        Category: DateTime,     Sub-Category: DateRange
                Score: 0.80,    Length: 9,      Offset: 34
```

### <a name="entity-linking"></a>엔터티 연결

앞에서 만든 클라이언트를 사용하는 `EntityLinkingExample()`이라는 새 함수를 만들고, `RecognizeLinkedEntities()` 함수를 호출하여 결과를 반복합니다. 반환된 `Response<LinkedEntityCollection>` 개체에는 검색된 엔터티 `LinkedEntity`의 컬렉션이 포함됩니다. 오류가 발생하면 `RequestFailedException`이 throw됩니다. 연결된 엔터티가 고유하게 식별되므로 동일한 엔터티의 발생은 `LinkedEntity` 개체 아래에서 `LinkedEntityMatch` 개체 목록으로 그룹화됩니다.

```csharp
static void EntityLinkingExample(TextAnalyticsClient client)
{
    var response = client.RecognizeLinkedEntities(
        "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, " +
        "to develop and sell BASIC interpreters for the Altair 8800. " +
        "During his career at Microsoft, Gates held the positions of chairman, " +
        "chief executive officer, president and chief software architect, " +
        "while also being the largest individual shareholder until May 2014.");
    Console.WriteLine("Linked Entities:");
    foreach (var entity in response.Value)
    {
        Console.WriteLine($"\tName: {entity.Name},\tID: {entity.DataSourceEntityId},\tURL: {entity.Url}\tData Source: {entity.DataSource}");
        Console.WriteLine("\tMatches:");
        foreach (var match in entity.Matches)
        {
            Console.WriteLine($"\t\tText: {match.Text}");
            Console.WriteLine($"\t\tScore: {match.ConfidenceScore:F2}");
            Console.WriteLine($"\t\tLength: {match.Length}");
            Console.WriteLine($"\t\tOffset: {match.Offset}\n");
        }
    }
}
```

### <a name="output"></a>출력

```console
Linked Entities:
        Name: Microsoft,        ID: Microsoft,  URL: https://en.wikipedia.org/wiki/Microsoft    Data Source: Wikipedia
        Matches:
                Text: Microsoft
                Score: 0.55
                Length: 9
                Offset: 0

                Text: Microsoft
                Score: 0.55
                Length: 9
                Offset: 150

        Name: Bill Gates,       ID: Bill Gates, URL: https://en.wikipedia.org/wiki/Bill_Gates   Data Source: Wikipedia
        Matches:
                Text: Bill Gates
                Score: 0.63
                Length: 10
                Offset: 25

                Text: Gates
                Score: 0.63
                Length: 5
                Offset: 161

        Name: Paul Allen,       ID: Paul Allen, URL: https://en.wikipedia.org/wiki/Paul_Allen   Data Source: Wikipedia
        Matches:
                Text: Paul Allen
                Score: 0.60
                Length: 10
                Offset: 40

        Name: April 4,  ID: April 4,    URL: https://en.wikipedia.org/wiki/April_4      Data Source: Wikipedia
        Matches:
                Text: April 4
                Score: 0.32
                Length: 7
                Offset: 54

        Name: BASIC,    ID: BASIC,      URL: https://en.wikipedia.org/wiki/BASIC        Data Source: Wikipedia
        Matches:
                Text: BASIC
                Score: 0.33
                Length: 5
                Offset: 89

        Name: Altair 8800,      ID: Altair 8800,        URL: https://en.wikipedia.org/wiki/Altair_8800  Data Source: Wikipedia
        Matches:
                Text: Altair 8800
                Score: 0.88
                Length: 11
                Offset: 116
```

### <a name="personally-identifiable-information-recognition"></a>개인 식별 정보 인식

앞에서 만든 클라이언트를 사용하는 `RecognizePIIExample()`이라는 새 함수를 만들고, `RecognizePiiEntities()` 함수를 호출하여 결과를 반복합니다. 반환된 `PiiEntityCollection`은 검색된 PII 엔터티의 목록을 나타냅니다. 오류가 발생하면 `RequestFailedException`이 throw됩니다.

```csharp
static void RecognizePIIExample(TextAnalyticsClient client)
{
    string document = "A developer with SSN 859-98-0987 whose phone number is 800-102-1100 is building tools with our APIs.";

    PiiEntityCollection entities = client.RecognizePiiEntities(document).Value;

    Console.WriteLine($"Redacted Text: {entities.RedactedText}");
    if (entities.Count > 0)
    {
        Console.WriteLine($"Recognized {entities.Count} PII entit{(entities.Count > 1 ? "ies" : "y")}:");
        foreach (PiiEntity entity in entities)
        {
            Console.WriteLine($"Text: {entity.Text}, Category: {entity.Category}, SubCategory: {entity.SubCategory}, Confidence score: {entity.ConfidenceScore}");
        }
    }
    else
    {
        Console.WriteLine("No entities were found.");
    }
}
```

### <a name="output"></a>출력

```console
Redacted Text: A developer with SSN *********** whose phone number is ************ is building tools with our APIs.
Recognized 2 PII entities:
Text: 859-98-0987, Category: U.S. Social Security Number (SSN), SubCategory: , Confidence score: 0.65
Text: 800-102-1100, Category: Phone Number, SubCategory: , Confidence score: 0.8
```

# <a name="version-30"></a>[버전 3.0](#tab/version-3)


> [!NOTE]
> `3.0` 버전의 새로운 기능:
> * 엔터티 연결은 이제 엔터티 인식과 분리됩니다.


앞에서 만든 클라이언트를 사용하는 `EntityRecognitionExample()`이라는 새 함수를 만들고, `RecognizeEntities()` 함수를 호출하여 결과를 반복합니다. 반환된 `Response<IReadOnlyCollection<CategorizedEntity>>` 개체에는 검색된 엔터티의 목록이 포함됩니다. 오류가 발생하면 `RequestFailedException`이 throw됩니다.

```csharp
static void EntityRecognitionExample(TextAnalyticsClient client)
{
    var response = client.RecognizeEntities("I had a wonderful trip to Seattle last week.");
    Console.WriteLine("Named Entities:");
    foreach (var entity in response.Value)
    {
        Console.WriteLine($"\tText: {entity.Text},\tCategory: {entity.Category},\tSub-Category: {entity.SubCategory}");
        Console.WriteLine($"\t\tScore: {entity.ConfidenceScore:F2}\n");
    }
}
```

### <a name="output"></a>출력

```console
Named Entities:
        Text: trip,     Category: Event,        Sub-Category:
                Score: 0.61

        Text: Seattle,  Category: Location,     Sub-Category: GPE
                Score: 0.82

        Text: last week,        Category: DateTime,     Sub-Category: DateRange
                Score: 0.80
```

### <a name="entity-linking"></a>엔터티 연결

앞에서 만든 클라이언트를 사용하는 `EntityLinkingExample()`이라는 새 함수를 만들고, `RecognizeLinkedEntities()` 함수를 호출하여 결과를 반복합니다. 반환된 `Response<IReadOnlyCollection<LinkedEntity>>`는 검색된 엔터티의 목록을 나타냅니다. 오류가 발생하면 `RequestFailedException`이 throw됩니다. 연결된 엔터티가 고유하게 식별되므로 동일한 엔터티의 발생은 `LinkedEntity` 개체 아래에서 `LinkedEntityMatch` 개체 목록으로 그룹화됩니다.

```csharp
static void EntityLinkingExample(TextAnalyticsClient client)
{
    var response = client.RecognizeLinkedEntities(
        "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, " +
        "to develop and sell BASIC interpreters for the Altair 8800. " +
        "During his career at Microsoft, Gates held the positions of chairman, " +
        "chief executive officer, president and chief software architect, " +
        "while also being the largest individual shareholder until May 2014.");
    Console.WriteLine("Linked Entities:");
    foreach (var entity in response.Value)
    {
        Console.WriteLine($"\tName: {entity.Name},\tID: {entity.DataSourceEntityId},\tURL: {entity.Url}\tData Source: {entity.DataSource}");
        Console.WriteLine("\tMatches:");
        foreach (var match in entity.Matches)
        {
            Console.WriteLine($"\t\tText: {match.Text}");
            Console.WriteLine($"\t\tScore: {match.ConfidenceScore:F2}\n");
        }
    }
}
```

### <a name="output"></a>출력

```console
Linked Entities:
        Name: Altair 8800,      ID: Altair 8800,        URL: https://en.wikipedia.org/wiki/Altair_8800  Data Source: Wikipedia
        Matches:
                Text: Altair 8800
                Score: 0.88

        Name: Bill Gates,       ID: Bill Gates, URL: https://en.wikipedia.org/wiki/Bill_Gates   Data Source: Wikipedia
        Matches:
                Text: Bill Gates
                Score: 0.63

                Text: Gates
                Score: 0.63

        Name: Paul Allen,       ID: Paul Allen, URL: https://en.wikipedia.org/wiki/Paul_Allen   Data Source: Wikipedia
        Matches:
                Text: Paul Allen
                Score: 0.60

        Name: Microsoft,        ID: Microsoft,  URL: https://en.wikipedia.org/wiki/Microsoft    Data Source: Wikipedia
        Matches:
                Text: Microsoft
                Score: 0.55

                Text: Microsoft
                Score: 0.55

        Name: April 4,  ID: April 4,    URL: https://en.wikipedia.org/wiki/April_4      Data Source: Wikipedia
        Matches:
                Text: April 4
                Score: 0.32

        Name: BASIC,    ID: BASIC,      URL: https://en.wikipedia.org/wiki/BASIC        Data Source: Wikipedia
        Matches:
                Text: BASIC
                Score: 0.33
```

# <a name="version-21"></a>[버전 2.1](#tab/version-2)

> [!NOTE]
> 버전 2.1에서 엔터티 연결은 NER 응답에 포함됩니다.

앞에서 만든 클라이언트를 사용하고 해당 [Entities()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.entities#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_Entities_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) 함수를 호출하는 `RecognizeEntitiesExample()`이라는 새 함수를 만듭니다. 결과를 반복합니다. 반환된 [EntitiesResult](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.models.entitiesresult) 개체에는 성공하면 `Entities`에서 검색된 엔티티 목록이 포함되고 그렇지 않으면 `errorMessage`가 포함됩니다. 검색된 각 엔터티에 대해 해당 Type(형식), Sub-Type(하위 형식), Wikipedia 이름(있는 경우) 외에도 원본 텍스트의 위치를 출력합니다.

[!code-csharp[Entity Recognition example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=entityRecognition)]

--- 


### <a name="key-phrase-extraction"></a>핵심 문구 추출

# <a name="version-31-preview"></a>[버전 3.1 미리 보기](#tab/version-3-1)

앞에서 만든 클라이언트를 사용하고 해당 `ExtractKeyPhrases()` 함수를 호출하는 `KeyPhraseExtractionExample()`이라는 새 함수를 만듭니다. 반환된 `<Response<KeyPhraseCollection>` 개체에는 검색된 핵심 구의 목록이 포함됩니다. 오류가 발생하면 `RequestFailedException`이 throw됩니다.

```csharp
static void KeyPhraseExtractionExample(TextAnalyticsClient client)
{
    var response = client.ExtractKeyPhrases("My cat might need to see a veterinarian.");

    // Printing key phrases
    Console.WriteLine("Key phrases:");

    foreach (string keyphrase in response.Value)
    {
        Console.WriteLine($"\t{keyphrase}");
    }
}
```

### <a name="output"></a>출력

```console
Key phrases:
    cat
    veterinarian
```

# <a name="version-30"></a>[버전 3.0](#tab/version-3)

앞에서 만든 클라이언트를 사용하고 해당 `ExtractKeyPhrases()` 함수를 호출하는 `KeyPhraseExtractionExample()`이라는 새 함수를 만듭니다. 반환된 `<Response<IReadOnlyCollection<string>>` 개체에는 검색된 핵심 구의 목록이 포함됩니다. 오류가 발생하면 `RequestFailedException`이 throw됩니다.

```csharp
static void KeyPhraseExtractionExample(TextAnalyticsClient client)
{
    var response = client.ExtractKeyPhrases("My cat might need to see a veterinarian.");

    // Printing key phrases
    Console.WriteLine("Key phrases:");

    foreach (string keyphrase in response.Value)
    {
        Console.WriteLine($"\t{keyphrase}");
    }
}
```

### <a name="output"></a>출력

```console
Key phrases:
    cat
    veterinarian
```

# <a name="version-21"></a>[버전 2.1](#tab/version-2)

앞에서 만든 클라이언트를 사용하고 해당 [KeyPhrases()](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.textanalytics.textanalyticsclientextensions.keyphrases#Microsoft_Azure_CognitiveServices_Language_TextAnalytics_TextAnalyticsClientExtensions_KeyPhrases_Microsoft_Azure_CognitiveServices_Language_TextAnalytics_ITextAnalyticsClient_System_String_System_String_System_Nullable_System_Boolean__System_Threading_CancellationToken_) 함수를 호출하는 `KeyPhraseExtractionExample()`이라는 새 함수를 만듭니다. 결과에는 성공하면 `KeyPhrases`에서 검색된 핵심 구 목록이 포함되고, 그렇지 않으면 `errorMessage`가 포함됩니다. 검색된 핵심 구를 출력합니다.

[!code-csharp[Key phrase extraction example](~/cognitive-services-dotnet-sdk-samples/samples/TextAnalytics/synchronous/Program.cs?name=keyPhraseExtraction)]


### <a name="output"></a>출력

```console
Key phrases:
    cat
    veterinarian
```

---