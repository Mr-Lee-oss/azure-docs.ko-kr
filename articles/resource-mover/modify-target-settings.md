---
title: Azure 리소스 이동 기를 사용 하 여 지역 간에 Azure Vm을 이동할 때 대상 설정 수정
description: Azure 리소스 이동 기를 사용 하 여 지역 간에 Azure Vm을 이동할 때 대상 설정을 수정 하는 방법에 대해 알아봅니다.
manager: evansma
author: rayne-wiselman
ms.service: resource-move
ms.topic: how-to
ms.date: 09/10/2020
ms.author: raynew
ms.openlocfilehash: ca22def6bc152d03c3992ed7e94ac2b5ccf179e9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90604481"
---
# <a name="modify-target-settings"></a>대상 설정 수정

이 문서에서는 azure [리소스](overview.md)를 사용 하 여 azure 지역 간에 리소스를 이동할 때 대상 설정을 수정 하는 방법을 설명 합니다.


## <a name="modify-vm-settings"></a>VM 설정 수정

Azure Vm 및 관련 리소스를 이동할 때 대상 설정을 수정할 수 있습니다. 

- 이동 컬렉션의 유효성을 검사 한 후에만 대상 설정을 변경 하는 것이 좋습니다.
- 준비가 완료 된 후에는 일부 대상 속성을 편집에 사용할 수 없기 때문에 리소스를 준비 하기 전에 설정을 수정 하는 것이 좋습니다.

단,
- 원본 리소스를 이동 하는 경우 일반적으로 이동 시작 프로세스를 시작할 때까지 대상 설정을 수정할 수 있습니다.
- 원본 영역에 기존 리소스를 할당 하는 경우 이동 커밋이 완료 될 때까지 대상 설정을 수정할 수 있습니다.

### <a name="settings-you-can-modify"></a>수정할 수 있는 설정

수정할 수 있는 구성 설정은 표에 요약 되어 있습니다.

**리소스** | **옵션** 
--- | --- | --- 
**VM 이름** | 옵션:<br/><br/> -대상 지역에 동일한 이름을 사용 하 여 새 VM을 만듭니다.<br/><br/> -대상 지역에 다른 이름을 사용 하 여 새 VM을 만듭니다.<br/><br/> -대상 지역에서 기존 VM을 사용 합니다.<br/><br/> 수정 하는 설정을 제외 하 고 새 VM을 만드는 경우 새 대상 VM에 원본과 동일한 설정이 할당 됩니다.
**VM 가용성 영역** | 대상 VM이 배치 될 가용성 영역입니다. 원본 설정을 변경 하지 않으려는 경우 또는 가용성 영역에 VM을 추가 하지 않으려는 경우에는 **NA** 로 표시 될 수 있습니다.
**VM SKU** | 대상 VM에 사용 되는 [vm 유형](https://azure.microsoft.com/pricing/details/virtual-machines/series/) (대상 지역에서 사용 가능).<br/><br/> 선택한 대상 VM은 원본 VM 보다 작을 수 없습니다.
**네트워킹 리소스** | 가상 네트워크 (Vnet)/네트워크 보안 그룹/네트워크 인터페이스에 대 한 옵션:<br/><br/> -대상 지역에 동일한 이름으로 새 리소스를 만듭니다.<br/><br/> -대상 지역에 다른 이름으로 새 리소스를 만듭니다.<br/><br/> -대상 지역에서 기존 네트워킹 리소스를 사용 합니다.<br/><br/> 새 대상 리소스를 만드는 경우 수정 하는 설정을 제외 하 고 원본 리소스와 동일한 설정이 할당 됩니다.
**공용 IP 주소 이름** | 이름을 지정합니다.
**공용 IP 주소 SKU** | [SKU](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#sku)를 지정 합니다.
**공용 IP 주소 영역** | 표준 공용 IP 주소에 대 한 [영역](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#standard) 을 지정 합니다.<br/><br/> 영역 중복으로 하려면 **영역 중복**으로를 입력 합니다.
**부하 분산 장치 이름** | 이름을 지정합니다.
**부하 분산 장치 SKU** | 기본 또는 표준. Standard를 사용 하는 것이 좋습니다.
**부하 분산 장치 영역** | 부하 분산 장치에 대 한 영역을 지정 합니다. <br/><br/> 영역 중복으로 하려면 **영역 중복**으로를 입력 합니다.
**리소스 종속성** | 각 종속성에 대 한 옵션:<br/><br/>-리소스는 대상 지역으로 이동 하는 소스 종속 리소스를 사용 합니다.<br/><br/> -리소스는 대상 영역에 있는 서로 다른 종속 리소스를 사용 합니다. 이 경우 대상 지역의 비슷한 리소스에서 선택할 수 있습니다.

### <a name="edit-vm-target-settings"></a>VM 대상 설정 편집

원본 지역에서 대상으로 리소스를 종속 하지 않으려는 경우 다음 두 가지 옵션을 사용할 수 있습니다.

- 대상 지역에 새 리소스를 만듭니다. 다른 설정을 지정 하지 않으면 새 리소스의 설정이 원본 리소스와 동일 하 게 설정 됩니다.
- 대상 지역에서 기존 리소스를 사용 합니다.

정확한 동작은 리소스 유형에 따라 달라 집니다. 대상 설정 수정에 [대해 자세히 알아보세요](modify-target-settings.md) .

리소스 이동 컬렉션의 **대상 구성** 항목을 사용 하 여 리소스에 대 한 대상 설정을 수정할 수 있습니다. 

설정을 수정 하려면: 

1. **지역 간** 페이지에서 **대상 구성** 열 > 리소스 항목에 대 한 링크를 클릭 합니다.
2. **구성 설정**에서 대상 지역에 새 VM을 만들 수 있습니다.
3. 새 가용성 영역, 가용성 집합 또는 SKU를 대상 VM에 할당 합니다. **가용성 영역** 및 **SKU**.

편집 중인 리소스에 대해서만 변경 내용이 적용 됩니다. 모든 종속 리소스를 개별적으로 업데이트 해야 합니다.


## <a name="modify-sql-settings"></a>SQL 설정 수정

Azure SQL Database 리소스를 이동 하는 경우 이동에 대 한 대상 설정을 수정할 수 있습니다. 

- SQL database의 경우:
    - 이동 하기 위해 준비 하기 전에 대상 구성 설정을 수정 하는 것이 좋습니다.
    - 대상 데이터베이스와 데이터베이스에 대 한 영역 중복성에 대 한 설정을 수정할 수 있습니다.
- 탄력적 풀의 경우:
    -  이동을 시작 하기 전에 언제 든 지 대상 구성을 수정할 수 있습니다.
    - 대상 탄력적 풀 및 풀에 대 한 영역 중복성을 수정할 수 있습니다. 

### <a name="sql-settings-you-can-modify"></a>수정할 수 있는 SQL 설정

**설정** | **SQL 데이터베이스** | **탄력적 풀**
--- | --- | ---
**이름** | 대상 지역에 동일한 이름을 사용 하 여 새 데이터베이스를 만듭니다.<br/><br/> 대상 지역에 다른 이름으로 새 데이터베이스를 만듭니다.<br/><br/> 대상 지역에서 기존 데이터베이스를 사용 합니다. | 대상 지역에 동일한 이름으로 새 탄력적 풀을 만듭니다.<br/><br/> 대상 지역에 다른 이름으로 새 탄력적 풀을 만듭니다.<br/><br/> 대상 지역에서 기존 탄력적 풀을 사용 합니다.
**영역 중복** | 영역 중복성을 지 원하는 지역에서 영역으로 이동 하려면 영역 설정에서 **사용 안 함** 을 입력 합니다.<br/><br/> 영역 중복성을 지원 하지 않는 지역에서 이동 하려면 영역 설정에서 **사용** 을 입력 합니다. | 영역 중복성을 지 원하는 지역에서 영역으로 이동 하려면 영역 설정에서 **사용 안 함** 을 입력 합니다.<br/><br/> 영역 중복성을 지원 하지 않는 지역에서 이동 하려면 영역 설정에서 **사용** 을 입력 합니다.

### <a name="edit-sql-target-settings"></a>SQL 대상 설정 편집

Azure SQL Database 리소스에 대 한 대상 설정은 다음과 같이 수정 합니다. 

1. **모든 지역**에서 수정 하려는 리소스에 대해 **대상 구성** 항목을 클릭 합니다.
2. **구성 설정**에서 위의 표에 요약 된 대상 설정을 지정 합니다.

## <a name="next-steps"></a>다음 단계

[AZURE VM](tutorial-move-region-virtual-machines.md) 을 다른 지역으로 이동 합니다.
