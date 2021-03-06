---
title: Azure Load Balancer 문제 해결
description: Azure Load Balancer의 알려진 문제를 해결하는 방법을 알아봅니다.
services: load-balancer
documentationcenter: na
author: asudbring
manager: dcscontentpm
ms.custom: seodoc18
ms.service: load-balancer
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/28/2020
ms.author: allensu
ms.openlocfilehash: 22922972049ec78cc26f4d060fa1981d1f23a3ce
ms.sourcegitcommit: d76108b476259fe3f5f20a91ed2c237c1577df14
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2020
ms.locfileid: "92912449"
---
# <a name="troubleshoot-azure-load-balancer"></a>Azure Load Balancer 문제 해결

이 페이지에서는 기본 및 표준 공통 Azure Load Balancer 질문에 대한 문제 해결 정보를 제공합니다. 표준 Load Balancer에 대한 자세한 내용은 [표준 Load Balancer 개요](load-balancer-standard-diagnostics.md)를 참조하세요.

Load Balancer 연결을 사용할 수 없을 때 가장 일반적인 증상은 다음과 같습니다. 

- Load Balancer 뒤의 VM이 상태 프로브에 응답하지 않습니다. 
- Load Balancer 뒤의 VM이 구성된 포트의 트래픽에 응답하지 않습니다.

백 엔드 VM에 대한 외부 클라이언트가 부하 분산 장치를 통과하면 클라이언트의 IP 주소가 통신에 사용됩니다. 클라이언트의 IP 주소가 NSG 허용 목록에 추가되어 있는지 확인합니다. 

## <a name="symptom-no-outbound-connectivity-from-standard-internal-load-balancers-ilb"></a>증상: 표준 내부 부하 분산 장치 (ILB)에서 아웃 바운드 연결이 없습니다.

**유효성 검사 및 해결**

표준 ILBs는 **기본적으로 안전** 합니다. 기본 ILBs는 *숨겨진* 공용 IP 주소를 통해 인터넷에 연결할 수 있습니다. IP 주소가 정적이 아니고 사용자가 소유 하는 NSGs를 통해 잠겨 있지 않으므로 프로덕션 워크 로드에는 것 좋습니다 되지 않습니다. 최근에 기본 ILB에서 표준 ILB로 이동 하는 경우 NSGs를 통해 IP의 잠금을 해제 하는 [아웃 바운드 전용](egress-only.md) 구성을 통해 명시적으로 공용 IP를 만들어야 합니다. 서브넷에서 [NAT 게이트웨이](../virtual-network/nat-overview.md) 를 사용할 수도 있습니다.

## <a name="symptom-vms-behind-the-load-balancer-are-not-responding-to-health-probes"></a>증상: Load Balancer 뒤의 VM이 상태 프로브에 응답하지 않습니다.
백 엔드 서버가 Load Balancer 집합에 참여하려면 프로브 검사를 통과해야 합니다. 상태 프로브에 대한 자세한 내용은 [Load Balancer 프로브 이해](load-balancer-custom-probe-overview.md)를 참조하세요. 

Load Balancer 백 엔드 풀 VM은 다음 이유 중 하나로 인해 프로브에 응답하지 않을 수 있습니다. 
- Load Balancer 백 엔드 풀 VM이 정상적인 상태가 아닙니다. 
- Load Balancer 백 엔드 풀 VM이 프로브 포트에서 수신하지 않습니다. 
- 방화벽 또는 네트워크 보안 그룹이 Load Balancer 백 엔드 풀 VM에서 포트를 차단하고 있습니다. 
- Load Balancer의 기타 구성 오류

### <a name="cause-1-load-balancer-backend-pool-vm-is-unhealthy"></a>원인 1: Load Balancer 백 엔드 풀 VM이 정상적인 상태가 아닙니다. 

**유효성 검사 및 해결**

이 문제를 해결하려면 참여하는 VM에 로그인하고 VM 상태가 정상인지와 풀에 있는 다른 VM의 **PsPing** 또는 **TCPing** 에 응답할 수 있는지 확인합니다. VM이 정상이 아니거나 프로브에 응답할 수 없으면 부하 분산에 사용하기 전에 문제를 해결하고 VM을 다시 정상 상태로 복구해야 합니다.

### <a name="cause-2-load-balancer-backend-pool-vm-is-not-listening-on-the-probe-port"></a>원인 2: Load Balancer 백 엔드 풀 VM이 프로브 포트에서 수신하지 않습니다.
VM이 정상 상태인데도 프로브에 응답하지 않을 경우 한 가지 가능한 원인은 프로브 포트가 참여 VM에서 열려 있지 않거나 VM이 해당 포트에서 수신하지 않는 것입니다.

**유효성 검사 및 해결**

1. 백 엔드 VM에 로그인합니다. 
2. 명령 프롬프트를 열고 다음 명령을 실행하여 프로브 포트에서 수신 대기하는 애플리케이션이 있는지 확인합니다.   
            netstat -an
3. 포트 상태가 **LISTENING** 으로 표시되지 않으면 해당 포트를 구성합니다. 
4. 또는 **LISTENING** 으로 표시되는 다른 포트를 선택한 후 부하 분산 장치 구성을 적절하게 업데이트합니다.              

### <a name="cause-3-firewall-or-a-network-security-group-is-blocking-the-port-on-the-load-balancer-backend-pool-vms"></a>원인 3: 방화벽 또는 네트워크 보안 그룹이 부하 분산 장치 백 엔드 풀 VM에서 포트를 차단하고 있습니다.  
VM의 방화벽이 프로브 포트를 차단하고 있거나 서브넷 또는 VM에 구성된 하나 이상의 네트워크 보안 그룹이 프로브가 포트에 연결하도록 허용하지 않을 경우 VM이 상태 프로브에 응답할 수 없게 됩니다.          

**유효성 검사 및 해결**

* 방화벽이 사용하도록 설정된 경우 프로브 포트를 허용하도록 구성되어 있는지 확인합니다. 그렇지 않으면 프로브 포트의 트래픽을 허용하도록 방화벽을 구성한 후 다시 테스트합니다. 
* 네트워크 보안 그룹 목록에서 프로브 포트에서 들어오거나 나가는 트래픽에 간섭에 있는지 확인합니다. 
* 또한 VM 또는 서브넷의 NIC에 대해 LB 프로브 또는 트래픽을 허용하는 기본 규칙보다 우선 순위가 더 높은 **모두 거부** 네트워크 보안 그룹 규칙이 있는지 확인합니다(네트워크 보안 그룹은 168.63.129.16의 Load Balancer IP를 허용해야 함). 
* 이러한 규칙에 의해 프로브 트래픽이 차단되는 경우 해당 규칙을 제거한 후 프로브 트래픽을 허용하도록 다시 구성합니다.  
* VM이 상태 프로브에 응답하기 시작했는지 테스트합니다. 

### <a name="cause-4-other-misconfigurations-in-load-balancer"></a>원인 4: Load Balancer의 기타 구성 오류
위의 모든 원인에 대해 유효성이 검사되고 문제가 적절히 해결된 것 같으나 백 엔드 VM이 여전히 상태 프로브에 응답하지 않으면 연결을 수동으로 테스트하고 연결을 이해하기 위한 몇 가지 추적을 수집합니다.

**유효성 검사 및 해결**

* VNet 내의 다른 VM 중 하나에서 **Psping** 을 사용하여 프로브 포트 응답을 테스트하고(예: .\psping.exe -t 10.0.0.4:3389) 결과를 기록합니다. 
* VNet 내의 다른 VM 중 하나에서 **TCPing** 을 사용하여 프로브 포트 응답을 테스트하고(예: ..\tcping.exe 10.0.0.4 3389) 결과를 기록합니다. 
* 이러한 ping 테스트에서 응답이 수신되지 않으면 다음을 수행합니다.
    - 동일한 VNet에서 대상 백 엔드 풀 VM 및 다른 테스트 VM에 대해 동시 Netsh 추적을 실행합니다. 이제 얼마 동안 PsPing 테스트를 실행하고 일부 네트워크 추적을 수집한 후 테스트를 중지합니다. 
    - 네트워크 캡처를 분석하고 ping 쿼리와 관련해서 들어오고 나가는 패킷이 둘다 있는지 확인합니다. 
        - 백 엔드 풀 VM에서 들어오는 패킷이 확인되지 않으면 트래픽을 차단하는 네트워크 보안 그룹 또는 UDR 구성 오류가 있는 것일 수 있습니다. 
        - 백 엔드 풀 VM에서 나가는 패킷이 확인되지 않으면 VM에 관련 없는 문제(예: 프로브 포트를 차단하는 애플리케이션)가 있는지 확인해야 합니다. 
    - 프로브 패킷이 부하 분산 장치에 도달하기 전에 강제로 다른 대상으로 전달되는지 확인합니다(UDR 설정을 통해). 이로 인해 트래픽이 백엔드 VM에 절대 도달하지 못할 수 있습니다. 
* 프로브 형식을 변경하고(예: HTTP에서 TCP로) 네트워크 보안 그룹 ACL의 해당 포트 및 방화벽이 프로브 응답 구성에 문제가 있는지 평가하도록 구성합니다. 상태 프로브 구성에 대한 자세한 내용은 [엔드포인트 부하 분산 장치 상태 프로브 구성](https://blogs.msdn.microsoft.com/mast/2016/01/26/endpoint-load-balancing-heath-probe-configuration-details/)을 참조하세요.

## <a name="symptom-vms-behind-load-balancer-are-not-responding-to-traffic-on-the-configured-data-port"></a>증상: Load Balancer 뒤의 VM이 구성된 데이터 포트의 트래픽에 응답하지 않습니다.

백 엔드 풀 VM이 정상으로 표시되고 상태 프로브에 응답하지만 여전히 부하 분산에 참여하지 않거나 데이터 트래픽에 응답하지 않을 경우 다음 이유 중 하나 때문일 수 있습니다. 
* Load Balancer 백 엔드 풀 VM이 데이터 포트에서 수신하지 않습니다. 
* 네트워크 보안 그룹이 Load Balancer 백 엔드 풀 VM에서 포트를 차단하고 있습니다.  
* 동일한 VM 및 NIC에서 Load Balancer에 액세스 
* 참여하는 Load Balancer 백 엔드 풀 VM에서 인터넷 Load Balancer 프런트 엔드에 액세스 

### <a name="cause-1-load-balancer-backend-pool-vm-is-not-listening-on-the-data-port"></a>원인 1: Load Balancer 백 엔드 풀 VM이 데이터 포트에서 수신 대기하지 않습니다. 
VM이 데이터 트래픽에 응답하지 않을 경우 대상 포트가 참여 VM에서 열려 있지 않거나 VM이 해당 포트에서 수신하지 않기 때문일 수 있습니다. 

**유효성 검사 및 해결**

1. 백 엔드 VM에 로그인합니다. 
2. 명령 프롬프트를 열고 다음 명령을 실행하여 데이터 포트에서 수신 대기하는 애플리케이션이 있는지 확인합니다.  
            netstat -an 
3. 포트가 "LISTENING" 상태로 표시되지 않으면 해당 수신기 포트를 구성합니다. 
4. 포트가 Listening으로 표시되어 있는 경우 해당 포트의 대상 애플리케이션에 문제가 있는지 확인합니다.

### <a name="cause-2-network-security-group-is-blocking-the-port-on-the-load-balancer-backend-pool-vm"></a>원인 2: 네트워크 보안 그룹이 Load Balancer 백 엔드 풀 VM에서 포트를 차단하고 있습니다.  

서브넷 또는 VM에 구성된 하나 이상의 네트워크 보안 그룹이 원본 IP 또는 포트를 차단하는 경우 VM이 응답할 수 없습니다.

공용 부하 분산 장치의 경우 클라이언트와 부하 분산 장치 백 엔드 VM 간의 통신에 인터넷 클라이언트의 IP 주소가 사용됩니다. 백 엔드 VM의 네트워크 보안 그룹에서 클라이언트의 IP 주소가 허용되는지 확인합니다.

1. 백 엔드 VM에 구성된 네트워크 보안 그룹을 나열합니다. 자세한 내용은 [네트워크 보안 그룹 관리](../virtual-network/manage-network-security-group.md)를 참조하세요.
1. 네트워크 보안 그룹 목록에서 다음을 확인합니다.
    - 데이터 포트에서 들어오거나 나가는 트래픽에 간섭이 있습니다. 
    - VM 또는 서브넷의 NIC에 대해 Load Balancer 프로브 및 트래픽을 허용하는 기본 규칙보다 우선 순위가 더 높은 **모두 거부** 네트워크 보안 그룹 규칙(네트워크 보안 그룹은 프로브 포트에 해당하는 168.63.129.16의 부하 분산 장치 IP를 허용해야 함).
1. 규칙에 의해 트래픽이 차단되는 경우 해당 규칙을 제거한 후 데이터 트래픽을 허용하도록 다시 구성합니다.  
1. VM이 상태 프로브에 응답하기 시작했는지 테스트합니다.

### <a name="cause-3-accessing-the-load-balancer-from-the-same-vm-and-network-interface"></a>원인 3: 동일한 VM 및 네트워크 인터페이스에서 Load Balancer에 액세스 

Load Balancer의 백 엔드 VM에서 호스트된 애플리케이션이 동일한 네트워크 인터페이스를 통해 동일한 백엔드 VM에서 호스트되는 다른 애플리케이션에 액세스하려고 하는 경우 이는 지원되지 않는 시나리오이며 실패합니다. 

**해결 방법** 다음 방법 중 하나를 사용하여 이 문제를 해결할 수 있습니다.
* 애플리케이션마다 별도 백 엔드 풀 VM을 구성합니다. 
* 각 애플리케이션이 자체 네트워크 인터페이스 및 IP 주소를 사용하도록 이중 NIC VM에 애플리케이션을 구성합니다. 

### <a name="cause-4-accessing-the-internal-load-balancer-frontend-from-the-participating-load-balancer-backend-pool-vm"></a>원인 4: 참여하는 Load Balancer 백 엔드 풀 VM에서 내부 Load Balancer 프런트 엔드에 액세스

내부 Load Balancer가 VNet 내에서 구성되고, 참여하는 백 엔드 Load Balancer 중 하나가 내부 Load Balancer 프런트 엔드에 액세스하려고 하면 흐름이 원본 VM에 매핑될 때 오류가 발생할 수 있습니다. 이 시나리오는 지원되지 않습니다.

**해결 방법** 프록시를 사용하여이 시나리오의 차단을 해제하는 방법이 몇 가지 있습니다. Application Gateway 또는 기타 타사 프록시(예: nginx 또는 haproxy)를 평가하세요. Application Gateway에 대한 자세한 내용은 [Application Gateway에 대한 개요](../application-gateway/application-gateway-introduction.md)를 참조하세요.

**세부 정보** Load Balancer는 둘 다 개인 IP 주소 공간에 있으므로 아웃바운드에서 시작된 연결을 내부 Load Balancer의 프런트 엔드로 변환하지 않습니다. 퍼블릭 Load Balancer는 가상 네트워크 내부의 프라이빗 IP 주소에서 퍼블릭 IP 주소로의 [아웃바운드 연결](load-balancer-outbound-connections.md)을 제공합니다. 내부 Load Balancer의 경우, 이렇게 하면 변환이 필요하지 않으면 고유한 내부 IP 주소 공간 내에서 SNAT 포트가 고갈될 가능성이 방지됩니다.

부작용은 백 엔드 풀에 있는 VM에서의 아웃바운드 흐름이 해당 풀에 있는 내부 Load Balancer의 프런트 엔드로 흐름을 유도하 _고_ 자체로 매핑되는 경우, 흐름의 두 레그가 일치하지 않는다는 것입니다. 두 레그가 일치하지 않기 때문에 흐름이 실패합니다. 흐름은 프런트 엔드로의 흐름을 생성한 백 엔드 풀의 동일한 VM에 다시 매핑되지 않으면 성공하게 됩니다.

흐름이 스스로에게 다시 매핑되는 경우 아웃바운드 흐름은 VM에서 시작된 후 프런트 엔드에 나타나며 해당 인바운드 흐름은 VM에서 시작된 후 자신에게 나타납니다. 게스트 운영 체제의 관점에서 동일한 흐름의 인바운드 및 아웃바운드 부분은 가상 머신 내에서 일치하지 않습니다. TCP 스택은 동일한 흐름의 이러한 절반을 동일한 흐름의 일부로서 인식하지 않습니다. 소스와 대상은 일치하지 않습니다. 흐름이 백 엔드 풀에서 다른 모든 VM에 매핑되는 경우 흐름의 절반은 일치하며 VM은 흐름에 응답할 수 있습니다.

이 시나리오에 대한 증상은 흐름이 흐름을 처음 시작한 동일한 백 엔드로 돌아갈 때 일시적인 연결 시간 제한입니다. 일반적인 해결 방법은 내부 Load Balancer 뒤에 프록시 계층을 삽입하고 DSR(Direct Server Return) 스타일 규칙을 사용하는 것입니다. 자세한 내용은 [Azure Load Balancer의 다중 프런트 엔드](load-balancer-multivip-overview.md)를 참조하세요.

타사 프록시에 내부 Load Balancer를 결합하거나 HTTP/HTTPS가 있는 프록시 시나리오에 대해 내부 [Application Gateway](../application-gateway/application-gateway-introduction.md)를 사용할 수 있습니다. 퍼블릭 Load Balancer를 사용하여 이 문제를 완화할 수는 있지만 결과 시나리오에서는 [SNAT 고갈](load-balancer-outbound-connections.md)이 발생할 가능성이 높습니다. 신중하게 관리하지 못할 경우에는 이 두 번째 방법을 사용하지 마세요.

## <a name="symptom-cannot-change-backend-port-for-existing-lb-rule-of-a-load-balancer-which-has-vm-scale-set-deployed-in-the-backend-pool"></a>증상: 백 엔드 풀에 배포된 VM 확장 집합이 있는 부하 분산 장치의 기존 LB 규칙에 대해 백 엔드 포트를 변경할 수 없습니다. 
### <a name="cause--the-backend-port-cannot-be-modified-for-a-load-balancing-rule-thats-used-by-a-health-probe-for-load-balancer-referenced-by-vm-scale-set"></a>원인 : VM 확장 집합에서 참조하는 부하 분산 장치의 상태 프로브에서 사용하는 부하 분산 규칙에 대해 백 엔드 포트를 수정할 수 없습니다.
**해결 방법** - 포트를 변경하려면 VM 크기 집합을 업데이트하여 상태 프로브를 제거하고 포트를 업데이트한 다음, 상태 프로브를 다시 구성합니다.

## <a name="symptom-small-traffic-is-still-going-through-load-balancer-after-removing-vms-from-backend-pool-of-the-load-balancer"></a>증상: 부하 분산 장치의 백 엔드 풀에서 VM을 제거한 후에도 작은 트래픽이 부하 분산 장치를 통해 계속 진행됩니다. 
### <a name="cause--vms-removed-from-backend-pool-should-no-longer-receive-traffic-the-small-amount-of-network-traffic-could-be-related-to-storage-dns-and-other-functions-within-azure"></a>원인 : 백 엔드 풀에서 제거된 VM은 더 이상 트래픽을 수신하지 않습니다. 소량의 네트워크 트래픽은 Azure 내의 스토리지, DNS 및 기타 기능과 관련될 수 있습니다. 
확인하려면 네트워크 추적을 수행하면 됩니다. Blob 스토리지 계정에 사용되는 FQDN은 각 스토리지 계정의 속성 내에 나열됩니다.  Azure 구독 내의 가상 머신에서 nslookup을 수행하여 해당 스토리지 계정에 할당된 Azure IP를 확인할 수 있습니다.

## <a name="additional-network-captures"></a>추가 네트워크 캡처
지원 사례를 열기로 결정한 경우 더 빠른 해결을 위해 다음 정보를 수집합니다. 단일 백 엔드 VM을 선택하여 다음과 같은 테스트를 수행합니다.
- VNet 내의 백 엔드 VM 중 하나에서 Psping을 사용하여 프로브 포트 응답을 테스트하고(예: psping 10.0.0.4:3389) 결과를 기록합니다. 
- 이러한 ping 테스트에서 응답이 수신되지 않으면 PsPing을 실행하는 동안 백 엔드 VM 및 VNet 테스트 VM에 대해 동시 Netsh 추적을 실행한 후 Netsh 추적을 중지합니다. 
 
## <a name="symptom-load-balancer-in-failed-state"></a>증상: 실패 상태 Load Balancer 

**해결 방법**

- 실패 상태에 있는 리소스를 확인 한 후 [Azure Resource Explorer](https://resources.azure.com/) 으로 이동 하 여이 상태에서 리소스를 식별 합니다. 
- 오른쪽 위 모퉁이의 설정/해제를 읽기/쓰기로 업데이트 합니다.
- 실패 상태에서 리소스에 대 한 편집을 클릭 합니다.
- PUT, GET을 차례로 클릭 하 여 프로 비전 상태가 성공으로 업데이트 되었는지 확인 합니다.
- 그런 다음 리소스가 실패 상태를 초과 하 여 다른 작업을 계속 진행할 수 있습니다.


## <a name="next-steps"></a>다음 단계

앞의 단계에서 문제가 해결되지 않으면 [지원 티켓](https://azure.microsoft.com/support/options/)을 엽니다.

