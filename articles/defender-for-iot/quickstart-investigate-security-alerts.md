---
title: '빠른 시작: 보안 경고 조사'
description: IoT 디바이스에서 Defender for IoT 보안 경고를 이해, 드릴다운 및 조사합니다.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/30/2020
ms.author: mlottner
ms.openlocfilehash: 172ae82288c2cb948839b69955b9491715eb4690
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/05/2020
ms.locfileid: "90945173"
---
# <a name="quickstart-investigate-security-alerts"></a>빠른 시작: 보안 경고 조사

Defender for IoT에서 발급한 경고의 예약된 조사 및 수정 작업이 IoT 솔루션에 대한 규정 준수 및 보호를 보장하는 가장 좋은 방법입니다.

이 빠른 시작에서는 각 IoT 보안 경고에서 사용할 수 있는 정보를 살펴보고 각 경고 및 관련 디바이스의 세부 정보를 드릴다운하고 사용하여 응답하고 수정하는 방법을 설명합니다. 

이제 시작하겠습니다. 


## <a name="investigate-new-security-alerts"></a>새 보안 경고 조사

IoT Hub 보안 경고 목록에는 IoT Hub에 대한 모든 집계된 보안 경고가 표시됩니다. 

1. Azure Portal에서 새 경고를 조사하려는 **IoT Hub**를 엽니다.
1. **보안** 메뉴에서 **경고**를 선택합니다. IoT Hub에 대한 모든 보안 경고가 표시되고 **새** 플래그가 포함된 경고는 지난 24시간 동안의 경고를 표시합니다.
:::image type="content" source="media/quickstart/investigate-new-security-alerts.png" alt-text="새 경고 플래그를 사용하여 새 IoT 보안 경고 조사":::
1. 목록에서 경고를 선택하고 열어 경고 세부 정보를 열고 경고 세부 정보로 드릴다운합니다. 

## <a name="security-alert-details"></a>보안 경고 세부 정보

각 집계된 경고를 열면 자세한 경고 설명, 수정 단계, 경고를 트리거한 각 디바이스에 대한 디바이스 ID 뿐만 아니라 Log Analytics를 사용하여 경고 심각도 및 직접 조사 액세스도 표시됩니다. 

1. **IoT Hub** > **보안** > **경고** 목록에서 보안 경고를 선택하고 엽니다. 
1. 집계 기간에 이 경고를 발행한 모든 디바이스의 경고 **설명**, **심각도**, **감지의 원본**, **디바이스 세부 정보**를 검토합니다.
:::image type="content" source="media/quickstart/drill-down-iot-alert-details.png" alt-text="새 경고 플래그를 사용하여 새 IoT 보안 경고 조사"::: 
1. 경고 세부 정보를 검토한 후 **수동 수정 단계** 지침을 사용하여 경고를 발생시킨 문제를 수정 및/또는 해결합니다. 
:::image type="content" source="media/quickstart/iot-alert-manual-remediation-steps.png" alt-text="새 경고 플래그를 사용하여 새 IoT 보안 경고 조사":::

1. 추가 조사가 필요한 경우 링크를 사용하여 **Log Analytics의 경고를 조사**합니다. 
:::image type="content" source="media/quickstart/investigate-iot-alert-log-analytics.png" alt-text="새 경고 플래그를 사용하여 새 IoT 보안 경고 조사":::

## <a name="next-steps"></a>다음 단계

Defender 경고 유형 및 가능한 사용자 지정에 대한 자세한 내용을 보려면 다음 문서로 이동하세요.

> [!div class="nextstepaction"]
> [IoT 보안 경고 이해](concept-security-alerts.md)
