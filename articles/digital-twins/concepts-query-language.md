---
title: 쿼리 언어
titleSuffix: Azure Digital Twins
description: Azure Digital Twins 쿼리 언어의 기본 사항을 이해 합니다.
author: baanders
ms.author: baanders
ms.date: 3/26/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: d656f19f6f4030025ff1393c3e5017466b3333fd
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92044397"
---
# <a name="about-the-query-language-for-azure-digital-twins"></a>Azure Digital Twins의 쿼리 언어 정보

Azure digital 쌍의 중심은 **디지털** 쌍 및 **관계**에서 생성 된 쌍 [**그래프**](concepts-twins-graph.md)입니다. 이 그래프를 쿼리하여 디지털 쌍 및 여기에 포함 된 관계에 대 한 정보를 가져올 수 있습니다. 이러한 쿼리는 **Azure Digital Twins 쿼리 언어**라고 하는 사용자 지정 SQL 유사 쿼리 언어로 작성 됩니다.

클라이언트 앱에서 서비스로 쿼리를 제출 하려면 Azure Digital Twins [**쿼리 API**](/dotnet/api/azure.digitaltwins.core.digitaltwinsclient.query?view=azure-dotnet-preview)를 사용 합니다. 이를 통해 개발자는 쿼리를 작성 하 고 필터를 적용 하 여 쌍 그래프에서 디지털 쌍 집합을 찾을 수 있으며, Azure digital 쌍 시나리오에 대 한 기타 정보를 찾을 수 있습니다.

[!INCLUDE [digital-twins-query-operations.md](../../includes/digital-twins-query-operations.md)]

## <a name="next-steps"></a>다음 단계

쿼리를 작성 하 고 [*방법: 쌍 그래프 쿼리*](how-to-query-graph.md)에서 클라이언트 코드 예제를 확인 하는 방법에 대해 알아봅니다.