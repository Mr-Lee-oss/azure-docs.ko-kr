---
title: 'Azure VPN Gateway에서 OpenVPN을 구성 하는 방법: PowerShell'
description: PowerShell을 사용 하 여 지점 및 사이트 간 환경용 Azure VPN Gateway에서 OpenVPN 프로토콜을 사용 하도록 설정 하는 방법을 알아봅니다.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 09/02/2020
ms.author: cherylmc
ms.openlocfilehash: 57bc53c28866b3f16e742c27b6a1600bfe6f44ab
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "89418826"
---
# <a name="configure-openvpn-for-azure-point-to-site-vpn-gateway"></a>Azure 지점 및 사이트 간 VPN Gateway에 대 한 OpenVPN 구성

이 문서에서는 Azure VPN Gateway에서 **Openvpn® 프로토콜** 을 설정 하는 방법을 설명 합니다. 이 문서에서는 작동하는 지점-사이트 간 환경이 이미 있다고 가정합니다. 환경이 없는 경우 1단계의 지침을 사용하여 지점-사이트 간 VPN을 만듭니다.



## <a name="1-create-a-point-to-site-vpn"></a><a name="vnet"></a>1. 지점 및 사이트 간 VPN 만들기

작동하는 지점-사이트 간 환경이 아직 없는 경우 지침에 따라 환경을 만듭니다. 기본 Azure 인증서 인증을 사용하여 지점-사이트 간 VPN 게이트웨이를 만들고 구성하려면 [지점-사이트 간 VPN 만들기](vpn-gateway-howto-point-to-site-resource-manager-portal.md)를 참조하세요. 

> [!IMPORTANT]
> 기본 SKU는 OpenVPN에서 지원되지 않습니다.

## <a name="2-enable-openvpn-on-the-gateway"></a><a name="enable"></a>2. 게이트웨이에서 OpenVPN을 사용 하도록 설정

게이트웨이에서 OpenVPN을 사용하도록 설정합니다. 다음 명령을 실행하기 전에 게이트웨이가 지점-사이트 간(IKEv2 또는 SSTP)에 대해 이미 구성되어 있는지 확인합니다.

```azurepowershell-interactive
$gw = Get-AzVirtualNetworkGateway -ResourceGroupName $rgname -name $name
Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -VpnClientProtocol OpenVPN
```

## <a name="next-steps"></a>다음 단계

OpenVPN 클라이언트를 구성하려면 [OpenVPN 클라이언트 구성](vpn-gateway-howto-openvpn-clients.md)을 참조하세요.

**"OpenVPN"은 OpenVPN i n c .의 상표입니다.**
