---
title: Azure 부팅 진단
description: Azure 부팅 진단 및 관리 되는 부팅 진단 개요
services: virtual-machines
ms.service: virtual-machines
author: mimckitt
ms.author: mimckitt
ms.topic: conceptual
ms.date: 08/04/2020
ms.openlocfilehash: 52a2b5e27cd5857416343e559237d08ea9a591be
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91972393"
---
# <a name="azure-boot-diagnostics"></a>Azure 부팅 진단

부팅 진단은 VM 부팅 실패를 진단할 수 있도록 하는 Azure VM (가상 머신)에 대 한 디버깅 기능입니다. 부팅 진단을 통해 사용자는 직렬 로그 정보와 스크린샷을 수집 하 여 부팅 하는 VM의 상태를 관찰할 수 있습니다.

## <a name="boot-diagnostics-storage-account"></a>부트 진단 저장소 계정
Azure Portal에서 VM을 만들 때 부트 진단은 기본적으로 사용 하도록 설정 됩니다. 권장 되는 부팅 진단 환경은 Azure VM을 만들 때 상당한 성능 향상을 제공 하므로 관리 되는 저장소 계정을 사용 하는 것입니다. 이는 Azure 관리 저장소 계정이 사용 되므로 부팅 진단 데이터를 저장 하는 새 사용자 저장소 계정을 만드는 데 걸리는 시간이 제거 되기 때문입니다.

대체 부팅 진단 환경은 사용자 관리 저장소 계정을 사용 하는 것입니다. 사용자는 새 저장소 계정을 만들거나 기존 저장소 계정을 사용할 수 있습니다.

> [!IMPORTANT]
> Azure 고객에 게는 10 월 2020 일까 지 관리 저장소 계정을 사용 하는 부팅 진단과 관련 된 저장소 비용이 청구 되지 않습니다.
>
> 로그와 스냅숏 이미지를 구성 하는 부팅 진단 데이터 blob은 관리 저장소 계정에 저장 됩니다. 고객은 디스크의 프로 비전 된 크기가 아닌 blob에서 사용 된 Gid에 대해서만 요금이 부과 됩니다. 스냅숏 미터는 관리 되는 저장소 계정의 요금 청구에 사용 됩니다. 관리 되는 계정은 표준 LRS 또는 표준 ZRS에 만들어지므로, 고객은 진단 데이터 blob의 크기에 대해서만 매월 $0.05/GB로 요금이 청구 됩니다. 이 가격에 대 한 자세한 내용은 [Managed disks 가격 책정](https://azure.microsoft.com/pricing/details/managed-disks/)을 참조 하세요. 고객은 VM 리소스 URI에 연결 되는이 요금을 볼 수 있습니다. 

## <a name="boot-diagnostics-view"></a>부트 진단 보기
가상 컴퓨터 블레이드에 있는 부팅 진단 옵션은 Azure Portal의 *지원 및 문제 해결* 섹션 아래에 있습니다. 부트 진단을 선택 하면 스크린샷 및 일련 로그 정보가 표시 됩니다. 직렬 로그는 커널 메시징을 포함 하 고 스크린샷은 Vm의 현재 상태에 대 한 스냅숏입니다. VM에서 Windows 또는 Linux를 실행 하 고 있는지 여부에 따라 예상 되는 스크린샷은 어떻게 나타나는지 결정 합니다. Windows의 경우 사용자에 게 데스크톱 배경 및 Linux가 표시 됩니다. 사용자에 게 로그인 프롬프트가 표시 됩니다.

:::image type="content" source="./media/boot-diagnostics/boot-diagnostics-linux.png" alt-text="Linux 부팅 진단 스크린샷":::
:::image type="content" source="./media/boot-diagnostics/boot-diagnostics-windows.png" alt-text="Linux 부팅 진단 스크린샷":::


## <a name="limitations"></a>제한 사항
- 부트 진단은 Azure Resource Manager Vm에만 사용할 수 있습니다. 
- 부팅 진단은 premium storage 계정을 지원 하지 않습니다. premium storage 계정이 부팅 진단에 사용 되 면 `StorageAccountTypeNotSupported` VM을 시작할 때 오류가 발생 합니다. 
- 관리 되는 저장소 계정은 리소스 관리자 API 버전 "2020-06-01" 이상에서 지원 됩니다.
- 현재 Azure 직렬 콘솔은 부트 진단을 위해 관리 되는 저장소 계정과 호환 되지 않습니다. [Azure 직렬 콘솔](./troubleshooting/serial-console-overview.md)에 대해 자세히 알아보세요.
- 저장소 관리 계정을 사용 하는 부팅 진단은 현재 Azure Portal을 통해서만 적용할 수 있습니다. 

## <a name="next-steps"></a>다음 단계

[Azure 직렬 콘솔](./troubleshooting/serial-console-overview.md) 에 대 한 자세한 내용과 부팅 진단을 사용 하 여 [azure의 가상 컴퓨터 문제를 해결](./troubleshooting/boot-diagnostics.md)하는 방법을 알아보세요.