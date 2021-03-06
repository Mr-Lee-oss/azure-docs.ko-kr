---
title: Azure Key Vault 보안
description: Azure Key Vault, 키 및 비밀에 대한 액세스 권한을 관리합니다. Key Vault의 인증 및 권한 부여 모델과 키 자격 증명 모음을 보호하는 방법을 설명합니다.
services: key-vault
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 09/30/2020
ms.author: mbaldwin
ms.openlocfilehash: c3dd4e5138741a3c035507358830f3572cf92751
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91739693"
---
# <a name="azure-key-vault-security"></a>Azure Key Vault 보안

Azure Key Vault를 사용 하 여 클라우드에서 인증서, 연결 문자열, 암호 등의 암호화 키와 암호를 보호 합니다. 중요 한 중요 한 데이터를 저장 하는 경우 자격 증명 모음 및 저장소에 저장 된 데이터의 보안을 최대화 하는 단계를 수행 해야 합니다.

## <a name="identity-and-access-management"></a>ID 및 액세스 관리

Azure 구독에 Key Vault을 만들 때 해당 구독의 Azure AD 테넌트에 자동으로 연결됩니다. 자격 증명 모음에서 콘텐츠를 관리하거나 검색하려는 사용자는 Azure AD의 인증을 받은 사용자여야 합니다.

- 인증은 호출자의 ID를 설정합니다.
- 권한은 호출자가 수행할 수 있는 작업을 결정합니다. Key Vault의 권한 부여에서는 RBAC([역할 기반 액세스 제어](../../role-based-access-control/overview.md))와 Azure Key Vault 액세스 정책을 함께 사용합니다.

### <a name="access-model-overview"></a>액세스 모델 개요

자격 증명 모음에 대한 액세스는 두 가지 인터페이스 또는 평면에서 이루어집니다. 평면에는 관리 평면과 데이터 평면이 있습니다.

- ‘관리 평면’은 Key Vault 자체를 관리하는 곳으로, 자격 증명 모음을 만들고 삭제하는 데 사용되는 인터페이스입니다. 키 자격 증명 모음 속성을 읽고 액세스 정책을 관리할 수도 있습니다.
- ‘데이터 평면’에서는 키 자격 증명 모음에 저장된 데이터로 작업할 수 있습니다. 키, 비밀 및 인증서를 추가, 삭제 및 수정할 수 있습니다.

두 평면에서 키 자격 증명 모음에 액세스하려면 모든 호출자(사용자 또는 애플리케이션)에게 적절한 인증 및 권한이 있어야 합니다. 두 평면은 인증을 위해 Azure AD(Azure Active Directory)를 사용합니다. 권한 부여를 위해 관리 평면에서는 RBAC(역할 기반 액세스 제어)를 사용하고, 데이터 평면에서는 Key Vault 액세스 정책을 사용합니다.

두 평면에 대한 인증에 단일 메커니즘을 사용할 경우 다음과 같은 몇 가지 이점이 있습니다.

- 조직에서는 조직의 모든 Key Vault에 대한 액세스를 중앙에서 제어할 수 있습니다.
- 사용자가 떠나는 경우 곧바로 조직의 모든 Key Vault에 대한 액세스 권한을 잃게 됩니다.
- 조직은 Azure AD에 있는 옵션(예: 추가 보안을 위해 다단계 인증 사용)을 사용하여 인증을 사용자 지정할 수 있습니다.

### <a name="managing-administrative-access-to-key-vault"></a>Key Vault에 대한 관리 액세스 관리

리소스 그룹에서 키 자격 증명 모음을 만든 경우 Azure AD를 사용하여 액세스를 관리합니다. 사용자 또는 그룹에 리소스 그룹에서 key vault를 관리하는 기능을 부여합니다. 적절 한 Azure 역할을 할당 하 여 특정 범위 수준에서 액세스 권한을 부여할 수 있습니다. key vault를 관리하기 위해 사용자에게 액세스 권한을 부여하려면 특정 범위에 속한 사용자에게 미리 정의된 `key vault Contributor` 역할을 할당합니다. Azure 역할에 할당 될 수 있는 범위 수준은 다음과 같습니다.

- **구독**: 구독 수준에서 할당 된 Azure 역할은 해당 구독 내의 모든 리소스 그룹 및 리소스에 적용 됩니다.
- **리소스 그룹**: 리소스 그룹 수준에서 할당 된 Azure 역할은 해당 리소스 그룹의 모든 리소스에 적용 됩니다.
- **특정 리소스**: 특정 리소스에 할당 된 Azure 역할이 해당 리소스에 적용 됩니다. 이 경우 리소스는 특정 Key Vault입니다.

미리 정의된 몇 가지 역할이 있습니다. 미리 정의된 역할이 요구에 맞지 않는 경우 고유한 역할을 정의할 수 있습니다. 자세한 내용은 [RBAC: 기본 제공 역할](../../role-based-access-control/built-in-roles.md)을 참조하세요.

> [!IMPORTANT]
> 사용자에게 Key Vault 관리 평면에 대한 `Contributor` 사용 권한이 있는 경우 이 사용자는 Key Vault 액세스 정책을 설정하여 스스로 데이터 평면에 대한 액세스 권한을 부여할 수 있습니다. Key Vault에 대한 `Contributor` 역할 액세스 권한이 있는 사용자를 긴밀하게 제어해야 합니다. 권한이 있는 사람만 키 자격 증명 모음, 키, 비밀 및 인증서에 액세스하고 관리할 수 있는지 확인합니다.

<a id="data-plane-access-control"></a>
### <a name="controlling-access-to-key-vault-data"></a>Key Vault 데이터에 대한 액세스 제어

Key Vault 액세스 정책은 키, 비밀 또는 인증서에 대해 개별적으로 권한을 부여합니다. 비밀이 아닌 키에 대해서만 사용자 액세스 권한을 부여할 수 있습니다. 키, 비밀 및 인증서에 대한 액세스 권한은 자격 증명 모음 수준에서 관리됩니다.

> [!IMPORTANT]
> Key Vault 액세스 정책은 특정 키, 비밀 또는 인증서와 같은 세분화된 개체 수준 권한을 지원하지 않습니다. 사용자에게 키를 만들고 삭제하는 권한이 부여되면 해당 사용자는 Key Vault의 모든 키에 대해 이러한 작업을 수행할 수 있습니다.

키 자격 증명 모음에 대 한 액세스 정책을 설정 하려면 [Azure Portal](assign-access-policy-portal.md), [Azure CLI](assign-access-policy-cli.md), [Azure PowerShell](assign-access-policy-powershell.md)또는 [Key Vault 관리 REST api](/rest/api/keyvault/)를 사용 합니다.

[Azure Key Vault에 대한 가상 네트워크 서비스 엔드포인트](overview-vnet-service-endpoints.md)를 사용하여 데이터 평면 액세스를 제한할 수 있습니다. 추가 보안 계층에 대한 [방화벽 및 가상 네트워크 규칙](network-security.md)을 구성할 수 있습니다.

## <a name="network-access"></a>네트워크 액세스

액세스 권한이 있는 IP 주소를 지정하여 자격 증명 모음의 노출을 줄일 수 있습니다. Azure Key Vault의 가상 네트워크 서비스 엔드포인트를 사용하면 지정된 가상 네트워크에 대한 액세스를 제한할 수 있습니다. 엔드포인트를 사용하면 IPv4(인터넷 프로토콜 버전 4) 주소 범위 목록에 대한 액세스를 제한할 수도 있습니다. 해당 소스 외부에서 키 자격 증명 모음에 연결하는 모든 사용자는 액세스가 거부됩니다.

방화벽 규칙이 적용되면 사용자의 요청이 허용되는 가상 네트워크 또는 IPv4 주소 범위에서 시작된 경우에만 사용자가 Key Vault에서 데이터를 읽을 수 있습니다. Azure Portal에서 Key Vault에 액세스하는 경우도 마찬가지입니다. 사용자가 Azure Portal에서 키 자격 증명 모음으로 이동할 수 있다고 해도 해당 클라이언트 머신이 허용 목록에 없는 경우 키, 비밀 또는 인증서를 나열하지 못할 수 있습니다. 다른 Azure 서비스의 Key Vault 선택기도 마찬가지입니다. 방화벽 규칙이 사용자의 클라이언트 머신을 금지하는 경우 해당 사용자는 키 자격 증명 모음 목록을 확인할 수 있지만 키를 나열하지는 못합니다.

Azure Key Vault 네트워크 주소에 대한 자세한 내용은 [Azure Key Vault의 가상 네트워크 서비스 엔드포인트](overview-vnet-service-endpoints.md)를 참조하세요.

## <a name="tls-and-https"></a>TLS 및 HTTPS

*   Key Vault 프런트 엔드 (데이터 평면)는 다중 테 넌 트 서버입니다. 즉, 서로 다른 고객의 키 자격 증명 모음이 동일한 공용 IP 주소를 공유할 수 있습니다. 격리를 위해 각 HTTP 요청은 다른 요청과 독립적으로 인증 되 고 권한이 부여 됩니다.
*   이전 버전의 TLS는 취약성을 보고할 수 있지만 공용 IP 주소는 공유 되므로 key vault 서비스 팀은 전송 수준에서 개별 키 자격 증명 모음에 대해 이전 버전의 TLS를 사용 하지 않도록 설정할 수 없습니다.
*   HTTPS 프로토콜을 사용 하면 클라이언트가 TLS 협상에 참여할 수 있습니다. **클라이언트는 최신 버전의 TLS를 적용할 수**있으며, 클라이언트에서이를 수행할 때마다 전체 연결에서 해당 하는 수준 보호를 사용 합니다. 이전 TLS 버전을 지 원하는 Key Vault 사실은 최신 TLS 버전을 사용 하는 연결 보안을 약화 하지 않습니다.
*   TLS 프로토콜의 알려진 취약성에도 불구 하 고 공격자가 취약성이 있는 TLS 버전과의 연결을 시작할 때 악의적인 에이전트가 키 자격 증명 모음에서 모든 정보를 추출할 수 있도록 하는 알려진 공격은 없습니다. 공격자는 자신을 인증 하 고 권한을 부여 해야 하며 합법적인 클라이언트가 항상 최신 TLS 버전에 연결 하는 한 이전 TLS 버전의 취약성으로 인해 자격 증명이 누출 되지 않을 수 있습니다.

## <a name="logging-and-monitoring"></a>로깅 및 모니터링

Key Vault 로깅은 자격 증명 모음에서 수행된 작업에 대한 정보를 저장합니다. 자세한 내용은 [Key Vault 로깅](logging.md)을 참조 하세요.

저장소 계정을 안전 하 게 관리 하는 방법에 대 한 권장 사항은 [Azure Storage 보안 가이드](../../storage/blobs/security-recommendations.md) 를 검토 하세요.

## <a name="next-steps"></a>다음 단계

- [Azure Key Vault의 가상 네트워크 서비스 엔드포인트](overview-vnet-service-endpoints.md)
- [RBAC: 기본 제공 역할](../../role-based-access-control/built-in-roles.md)을 참조하세요.