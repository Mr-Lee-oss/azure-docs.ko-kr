---
title: Windows 가상 데스크톱 환경-Azure
description: 호스트 풀 및 앱 그룹과 같은 Windows 가상 데스크톱 환경의 기본 요소에 대해 알아봅니다.
author: Heidilohr
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 99a07dd1791b539ea44fcbab250aa9c227ee1705
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "88002610"
---
# <a name="windows-virtual-desktop-environment"></a>Windows Virtual Desktop 환경

>[!IMPORTANT]
>이 콘텐츠는 Azure Resource Manager Windows Virtual Desktop 개체를 통해 Windows Virtual Desktop에 적용됩니다. Azure Resource Manager 개체 없이 Windows Virtual Desktop(클래식)을 사용하는 경우 [이 문서](./virtual-desktop-fall-2019/environment-setup-2019.md)를 참조하세요.

Windows 가상 데스크톱은 사용자에 게 가상화 된 데스크톱 및 Remoteapp에 대 한 액세스를 쉽고 안전 하 게 제공 하는 서비스입니다. 이 항목에서는 Windows 가상 데스크톱 환경의 일반적인 구조에 대해 자세히 설명 합니다.

## <a name="host-pools"></a>호스트 풀

호스트 풀은 windows 가상 데스크톱 에이전트를 실행할 때 Windows 가상 데스크톱에 세션 호스트로 등록 하는 Azure 가상 컴퓨터의 컬렉션입니다. 일관 된 사용자 환경을 위해 호스트 풀의 모든 세션 호스트 가상 머신은 동일한 이미지에서 원본 이어야 합니다.

호스트 풀은 다음 두 가지 유형 중 하나일 수 있습니다.

- 개인-각 세션 호스트가 개별 사용자에 게 할당 됩니다.
- 풀링된-세션 호스트가 호스트 풀 내의 앱 그룹에 대해 권한이 부여 된 모든 사용자의 연결을 허용할 수 있습니다.

호스트 풀에서 추가 속성을 설정 하 여 부하 분산 동작, 각 세션 호스트에서 수행할 수 있는 세션 수, 사용자가 Windows 가상 데스크톱 세션에 로그인 한 동안 호스트 풀의 호스트를 세션에 대해 수행할 수 있는 작업을 변경할 수 있습니다. 앱 그룹을 통해 사용자에 게 게시 된 리소스를 제어할 수 있습니다.

## <a name="app-groups"></a>앱 그룹

앱 그룹은 호스트 풀의 세션 호스트에 설치 된 응용 프로그램의 논리적 그룹입니다. 앱 그룹은 다음 두 가지 유형 중 하나일 수 있습니다.

- 사용자가 앱 그룹에 개별적으로 선택 하 고 게시 하는 Remoteapp에 액세스 하는 RemoteApp
- 사용자가 전체 데스크톱에 액세스 하는 데스크톱

기본적으로 "데스크톱 응용 프로그램 그룹" 이라는 데스크톱 앱 그룹은 호스트 풀을 만들 때마다 자동으로 생성 됩니다. 언제 든 지이 앱 그룹을 제거할 수 있습니다. 그러나 데스크톱 응용 프로그램 그룹이 있는 동안에는 호스트 풀에서 다른 데스크톱 앱 그룹을 만들 수 없습니다. Remoteapp을 게시 하려면 RemoteApp 앱 그룹을 만들어야 합니다. 여러 가지 작업자 시나리오를 수용 하기 위해 여러 RemoteApp 앱 그룹을 만들 수 있습니다. 다른 RemoteApp 앱 그룹에는 겹치는 Remoteapp 포함 될 수도 있습니다.

사용자에 게 리소스를 게시 하려면 앱 그룹에 리소스를 할당 해야 합니다. 앱 그룹에 사용자를 할당 하는 경우 다음 사항을 고려 하세요.

- 사용자는 동일한 호스트 풀의 데스크톱 앱 그룹과 RemoteApp 앱 그룹 모두에 할당할 수 있습니다. 그러나 사용자는 세션당 앱 그룹의 한 가지 유형만 시작할 수 있습니다. 사용자는 단일 세션에서 두 유형의 앱 그룹을 동시에 실행할 수 없습니다.
- 사용자는 동일한 호스트 풀 내의 여러 앱 그룹에 할당 될 수 있으며, 피드는 두 앱 그룹의 누적 됩니다.

## <a name="workspaces"></a>작업 영역

작업 영역은 Windows 가상 데스크톱에서 응용 프로그램 그룹을 논리적으로 그룹화 한 것입니다. 각 Windows 가상 데스크톱 응용 프로그램 그룹은 사용자가 게시 된 원격 앱 및 데스크톱을 볼 수 있도록 작업 영역에 연결 되어야 합니다.

## <a name="end-users"></a>최종 사용자

사용자를 앱 그룹에 할당 한 후에는 Windows 가상 데스크톱 클라이언트를 사용 하 여 Windows 가상 데스크톱 배포에 연결할 수 있습니다.

## <a name="next-steps"></a>다음 단계

위임 된 액세스에 대 한 자세한 내용과 [Windows 가상 데스크톱의 위임 된 액세스](delegated-access-virtual-desktop.md)에서 사용자에 게 역할을 할당 하는 방법에 대해 알아보세요.

Windows 가상 데스크톱 호스트 풀을 설정 하는 방법에 대 한 자세한 내용은 [Azure Portal 사용 하 여 호스트 풀 만들기](create-host-pools-azure-marketplace.md)를 참조 하세요.

Windows 가상 데스크톱에 연결 하는 방법을 알아보려면 다음 문서 중 하나를 참조 하세요.

- [Windows 10 또는 Windows 7을 사용 하 여 연결](connect-windows-7-10.md)
- [웹 브라우저와 연결](connect-web.md)
- [Android 클라이언트와 연결](connect-android.md)
- [macOS 클라이언트와 연결](connect-macos.md)
- [iOS 클라이언트와 연결](connect-ios.md)