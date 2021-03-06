---
title: Azure Front 도어 구성 문제 해결
description: 이 자습서에서는 Front Door에 발생할 수 있는 일반적인 문제 몇 가지를 자체적으로 해결하는 방법을 알아보세요.
services: frontdoor
documentationcenter: ''
author: duongau
editor: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/30/2020
ms.author: duau
ms.openlocfilehash: dbce9019e33c07dd4faa91ffd490eba4d313c675
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91630613"
---
# <a name="troubleshooting-common-routing-issues"></a>일반적인 라우팅 문제 해결

이 문서에서는 Azure Front 도어 구성에 직면 했을 수 있는 일반적인 라우팅 문제 중 일부를 해결 하는 방법을 설명 합니다.

## <a name="503-response-from-front-door-after-a-few-seconds"></a>몇 초 후에 전면 도어에서 응답 503

### <a name="symptom"></a>증상

* 프런트 도어를 거치지 않고 백 엔드로 전송 되는 일반 요청은 성공 하지만, 앞 도어를 통해 503 오류 응답이 발생 합니다.
* 앞 도어의 실패가 몇 초 후에 표시 됩니다 (일반적으로 30 초 이후).

### <a name="cause"></a>원인

이 문제의 원인은 다음 두 가지 중 하나일 수 있습니다.
 
* 프런트 도어에서 요청을 받기 위해 구성 된 시간 제한 (기본값은 30 초) 보다 백 엔드가 더 오래 걸리고 있습니다.
* 프런트 도어에서 요청에 대 한 응답을 보내는 데 걸리는 시간이 제한 시간 값 보다 오래 걸리고 있습니다. 

### <a name="troubleshooting-steps"></a>문제 해결 단계

* 프런트 도어를 거치지 않고 백 엔드에 직접 요청을 보내고 백 엔드가 응답 하는 데 걸리는 일반적인 시간을 확인 합니다.
* 전면 도어를 통해 요청을 보내고 503 응답을 받고 있는지 확인 합니다. 그렇지 않은 경우에는 문제가 시간 제한 문제가 아닐 수 있습니다. 지원에 문의
* 앞 도어를 통해 503 오류 응답 코드를 생성 하는 경우에는 `sendReceiveTimeout` 앞 도어에 대해 필드를 구성 합니다. 기본 시간 제한을 최대 4 분 (240 초)으로 확장할 수 있습니다. 설정은 아래에 `backendPoolSettings` 있고가 호출 됩니다 `sendRecvTimeoutSeconds` . 

## <a name="requests-sent-to-the-custom-domain-returns-400-status-code"></a>사용자 지정 도메인으로 전송 된 요청은 400 상태 코드를 반환 합니다.

### <a name="symptom"></a>증상

* 프런트 도어를 만들었지만 도메인 또는 프런트 엔드 호스트에 대 한 요청이 HTTP 400 상태 코드를 반환 합니다.
* 구성한 프런트 엔드 호스트에 대 한 사용자 지정 도메인에 대 한 DNS 매핑을 만들었습니다. 그러나 사용자 지정 도메인 호스트 이름에 요청을 보내면 HTTP 400 상태 코드가 반환 됩니다. 구성 된 백 엔드로 라우팅하는 것은 아닙니다.

### <a name="cause"></a>원인

프런트 엔드 호스트로 추가 된 사용자 지정 도메인에 대 한 라우팅 규칙을 구성 하지 않은 경우이 문제가 발생 합니다. 해당 프런트 엔드 호스트에 대해 라우팅 규칙을 명시적으로 추가 해야 합니다. 프런트 도어 하위 도메인 (*. azurefd.net)의 프런트 엔드 호스트에 대해 이미 구성 되어 있는 경우에도 마찬가지입니다.

### <a name="troubleshooting-steps"></a>문제 해결 단계

선택한 백 엔드 풀로 트래픽을 보낼 사용자 지정 도메인에 대 한 라우팅 규칙을 추가 합니다.

## <a name="front-door-doesnt-redirect-http-to-https"></a>전면 도어는 HTTP를 HTTPS로 리디렉션하지 않습니다.

### <a name="symptom"></a>증상

프런트 도어에는 HTTP를 HTTPS로 리디렉션하도록 지시 하는 라우팅 규칙이 있지만 도메인에 액세스 하는 것은 여전히 HTTP를 프로토콜로 유지 관리 합니다.

### <a name="cause"></a>원인

이 동작은 앞 도어에 대해 라우팅 규칙을 올바르게 구성 하지 않은 경우에 발생할 수 있습니다. 기본적으로 현재 구성은 구체적이 아니며 충돌 하는 규칙이 있을 수 있습니다.

### <a name="troubleshooting-steps"></a>문제 해결 단계

## <a name="request-to-frontend-hostname-returns-404-status-code"></a>프런트 엔드 호스트 이름에 대한 요청이 404 상태 코드 반환

### <a name="symptom"></a>증상

 프런트 엔드 호스트, 백 엔드가 하나 이상 포함 된 백 엔드 풀 및 프런트 엔드 호스트를 백 엔드 풀에 연결 하는 라우팅 규칙을 구성 하 여 프런트 도어를 만들었습니다. 구성 된 프런트 엔드 호스트에 대 한 요청을 하면 HTTP 404 상태 코드가 반환 되므로 콘텐츠를 사용할 수 없습니다.

### <a name="cause"></a>원인

이 증상의 잠재적 원인에는 몇 가지가 있습니다.

* 백 엔드는 공용 백 엔드가 아니며 앞 도어에 표시 되지 않습니다.
* 프런트 도어가 잘못 된 요청을 보내기 때문에 백 엔드가 잘못 구성 되었습니다. 즉, 백 엔드에서 HTTP만 허용 하 고 HTTPS 허용을 선택 취소 하지 않았습니다. 따라서 전방 도어는 HTTPS 요청을 전달 하려고 합니다.
* 백 엔드가 백 엔드에 대한 요청으로 전달된 호스트 헤더를 거부하고 있습니다.
* 백 엔드에 대 한 구성이 아직 완전히 배포 되지 않았습니다.

### <a name="troubleshooting-steps"></a>문제 해결 단계

1. 배포 시간
   * 구성이 배포 될 때까지 10 분 정도 기다려야 합니다.

2. 백 엔드 설정 확인
    * 요청을 라우팅하는 백 엔드 풀로 이동 합니다 (라우팅 규칙을 구성 하는 방법에 따라 다름). *백 엔드 호스트 유형* 및 백 엔드 호스트 이름이 올바른지 확인 합니다. 백 엔드가 사용자 지정 호스트인 경우 철자가 정확한 지 확인 합니다. 

    * HTTP 및 HTTPS 포트를 확인합니다. 대부분의 경우 80 및 443 (각각)은 올바르지만 변경이 필요 하지 않습니다. 그러나 이러한 방식으로 백 엔드가 구성 되지 않아 다른 포트에서 수신 대기 하는 경우가 있습니다.

        * 프런트 엔드 호스트를 라우팅해야 하는 백 엔드에 _백 엔드 호스트 헤더_가 구성되어 있는지 확인하세요. 대부분의 경우 이 헤더는 *백 엔드 호스트 이름*과 동일해야 합니다. 그러나 백 엔드에서 기대한 값과 다를 경우 다양한 HTTP 4xx 상태 코드가 반환될 수 있습니다. 백 엔드의 IP 주소를 입력하는 경우 *백 엔드 호스트 헤더*를 백 엔드의 호스트 이름으로 설정해야 할 수 있습니다.

3. 라우팅 규칙 설정을 확인 합니다.
    * 해당 프런트 엔드 호스트 이름에서 백 엔드 풀로 라우팅되어야 하는 회람 규칙으로 이동합니다. 요청을 전달할 때 허용 된 프로토콜이 올바르게 구성 되어 있는지 확인 합니다. 허용 되는 *프로토콜* 필드에 따라 앞 도어를 수락할 요청을 결정 합니다. *전달 프로토콜* 은 프런트 도어를 사용 하 여 백 엔드에 요청을 전달 하기 위해 사용 해야 하는 프로토콜을 결정 합니다.
         * 예를 들어 백 엔드가 HTTP 요청만 허용하는 경우 다음과 같은 구성이 유효합니다.
            * *허용되는 프로토콜*은 HTTP 및 HTTPS입니다. *전달 프로토콜*은 HTTP입니다. HTTPS는 허용 되는 프로토콜 이며 요청이 HTTPS로 제공 되는 경우, 프런트 도어는 HTTPS를 사용 하 여 전달 하려고 시도 하기 때문에 일치 요청이 작동 하지 않습니다.

            * *허용되는 프로토콜*은 HTTP입니다. *전달 프로토콜이* 요청 또는 HTTP와 일치 합니다.

    - *Url 재작성* 은 기본적으로 사용 되지 않습니다. 이 필드는 사용 가능 하도록 설정 하려는 백 엔드 호스트 리소스의 범위를 좁히는 경우에만 사용 됩니다. 비활성화될 경우 Front Door가 수신한 요청 경로를 전달합니다. 이 필드를 잘못 구성할 수 있습니다. 따라서 프런트 도어는 사용할 수 없는 백 엔드에서 리소스를 요청 하는 경우 HTTP 404 상태 코드를 반환 합니다.
