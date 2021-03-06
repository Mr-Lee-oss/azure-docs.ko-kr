---
title: Azure HDInsight에서 지역 오류에 장애 도메인이 부족 합니다.
description: Azure HDInsight의 지역에 장애 도메인이 충분 하지 않아 클러스터를 만들지 못했습니다.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 08/09/2019
ms.openlocfilehash: 070ba5ecb6f151099c13082eee650c5720e63b2a
ms.sourcegitcommit: d767156543e16e816fc8a0c3777f033d649ffd3c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/26/2020
ms.locfileid: "92532597"
---
# <a name="scenario-cluster-creation-failed-due-to-not-sufficient-fault-domains-in-region-in-azure-hdinsight"></a>시나리오: `not sufficient fault domains in region` Azure HDInsight에서로 인해 클러스터를 만들지 못했습니다.

이 문서에서는 Azure HDInsight 클러스터와 상호 작용할 때 문제에 대 한 문제 해결 단계 및 가능한 해결 방법을 설명 합니다.

## <a name="issue"></a>문제

Apache Kafka 클러스터를 만들려고 할 때와 유사한 오류 메시지가 표시 `not sufficient fault domains in region` 됩니다.

## <a name="cause"></a>원인

장애 도메인은 Azure 데이터 센터에 있는 기본 하드웨어의 논리적 그룹입니다. 장애 도메인마다 공통 전원과 네트워크 스위치를 공유합니다. HDInsight 클러스터 내의 노드를 구현하는 가상 머신과 관리 디스크는 이러한 장애 도메인에 분산되어 있습니다. 이 아키텍처에서는 실제 하드웨어 오류의 잠재적 영향을 제한합니다.

Azure 지역마다 특정 수의 장애 도메인이 있습니다. 도메인 목록과 여기에 포함 된 장애 도메인의 수에 대 한 자세한 내용은 [가용성 집합](../../virtual-machines/manage-availability.md)에 대 한 설명서를 참조 하세요.

HDInsight에서 Kafka 클러스터는 세 개 이상의 장애 도메인을 포함 하는 지역에서 프로 비전 해야 합니다.

## <a name="resolution"></a>해결 방법

클러스터를 만들려는 지역에 충분 한 장애 도메인이 없는 경우 세 개의 장애 도메인이 없더라도 제품 팀에 연락 하 여 클러스터를 프로 비전 할 수 있도록 합니다.

## <a name="next-steps"></a>다음 단계

문제가 표시되지 않거나 문제를 해결할 수 없는 경우 다음 채널 중 하나를 방문하여 추가 지원을 받으세요.

* [Azure 커뮤니티 지원](https://azure.microsoft.com/support/community/)을 통해 Azure 전문가로부터 답변을 얻습니다.

* [@AzureSupport](https://twitter.com/azuresupport)(고객 환경을 개선하기 위한 공식 Microsoft Azure 계정)에 연결합니다. Azure 커뮤니티를 적절한 리소스(답변, 지원 및 전문가)에 연결합니다.

* 도움이 더 필요한 경우 [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)에서 지원 요청을 제출할 수 있습니다. 메뉴 모음에서 **지원** 을 선택하거나 **도움말 + 지원** 허브를 엽니다. 자세한 내용은 [Azure 지원 요청을 만드는 방법](../../azure-portal/supportability/how-to-create-azure-support-request.md)을 참조하세요. 구독 관리 및 청구 지원에 대한 액세스는 Microsoft Azure 구독에 포함되며 [Azure 지원 플랜](https://azure.microsoft.com/support/plans/) 중 하나를 통해 기술 지원이 제공됩니다.