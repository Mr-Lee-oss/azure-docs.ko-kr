---
title: Azure Synapse Analytics로 tb의 데이터 로드
description: Azure Data Factory를 사용 하 여 15 분 이내에 Azure Synapse Analytics에 1TB의 데이터를 로드 하는 방법을 보여 줍니다.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.assetid: a6c133c0-ced2-463c-86f0-a07b00c9e37f
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 4e6b0afab5c86131575d0e3d12b9984a8463f5a3
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93321102"
---
# <a name="load-1-tb-into-azure-synapse-analytics-under-15-minutes-with-data-factory"></a>Data Factory를 사용 하 여 15 분 이내에 Azure Synapse Analytics에 1TB를 로드 합니다.
> [!NOTE]
> 이 아티클은 Data Factory 버전 1에 적용됩니다. 최신 버전의 Data Factory 서비스를 사용 하는 경우 [Data Factory를 사용 하 여 Azure Synapse Analytics (이전의 SQL Data Warehouse) 간에 데이터 복사](../connector-azure-sql-data-warehouse.md)를 참조 하세요.


[Azure Synapse Analytics](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is.md) 는 관계형 데이터와 비관계형 데이터를 모두 처리할 수 있는 클라우드 기반 스케일 아웃 데이터베이스입니다.  MPP (대규모 parallel processing) 아키텍처를 기반으로 하는 Azure Synapse Analytics는 엔터프라이즈 데이터 웨어하우스 워크 로드에 최적화 되어 있습니다.  스토리지를 확장하고 개별적으로 계산할 수 있는 클라우드 탄력성을 유연하게 제공합니다.

이제 **Azure Data Factory** 를 사용 하 여 Azure Synapse Analytics를 시작 하는 것이 더 쉬워졌습니다.  Azure Data Factory는 완전히 관리 되는 클라우드 기반 데이터 통합 서비스로 서 Azure Synapse Analytics를 기존 시스템의 데이터로 채우는 데 사용할 수 있으며 Azure Synapse Analytics를 평가 하 고 분석 솔루션을 구축 하는 동안 귀중 한 시간을 절약할 수 있습니다. Azure Data Factory를 사용 하 여 Azure Synapse Analytics로 데이터를 로드 하는 경우의 주요 이점은 다음과 같습니다.

* **간편한 설정** : 스크립팅이 필요하지 않은 직관적인 5단계 마법사.
* **다양 한 데이터 저장소 지원** : 다양 한 온-프레미스 및 클라우드 기반 데이터 저장소 집합을 기본으로 지원 합니다.
* **보안 및 호환** : 데이터가 HTTPS 또는 ExpressRoute를 통해 전송되고 글로벌 서비스를 제공하므로 데이터가 지리적 경계를 벗어나지 않음
* **Polybase를 사용 하는 뛰어난 성능** – polybase를 사용 하는 것은 Azure Synapse Analytics로 데이터를 이동 하는 가장 효율적인 방법입니다. 스테이징 Blob 기능을 사용하면 Polybase가 기본적으로 지원하는 Azure Blob Storage를 제외한 모든 유형의 데이터 저장소에서 높은 로드 속도를 얻을 수 있습니다.

이 문서에서는 Data Factory 복사 마법사를 사용 하 여 Azure Blob Storage의 1TB 데이터를 15 분 내에 1.2 GBps 이상의 처리량으로 Azure Synapse Analytics로 로드 하는 방법을 보여 줍니다.

이 문서에서는 복사 마법사를 사용 하 여 Azure Synapse Analytics로 데이터를 이동 하는 방법에 대 한 단계별 지침을 제공 합니다.

> [!NOTE]
>  Azure Synapse Analytics 간에 데이터를 이동 하는 Data Factory 기능에 대 한 일반적인 내용은 Azure Data Factory를 [사용 하 여 Azure Synapse analytics 간 데이터 이동](data-factory-azure-sql-data-warehouse-connector.md) 을 참조 하세요.
>
> Visual Studio, PowerShell 등을 사용 하 여 파이프라인을 빌드할 수도 있습니다. Azure Data Factory에서 복사 작업을 사용 하는 방법에 대 한 단계별 지침을 보려면 [자습서: Azure Blob에서 데이터 복사를 Azure SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) 합니다.  
>
>

## <a name="prerequisites"></a>필수 구성 요소
* Azure Blob Storage: 이 실험에서는 Azure Blob Storage(GRS)를 사용하여 TPC-H 테스트 데이터 세트를 저장합니다.  Azure Storage 계정이 없을 경우 [스토리지 계정을 만드는 방법](../../storage/common/storage-account-create.md)을 참조하세요.
* [TPC-H](http://www.tpc.org/tpch/) 데이터: 테스트 집합으로는 TPC-H를 사용할 것입니다.  이렇게 하려면 데이터 세트를 생성하도록 도와주는 TPC-H 도구 키트의 `dbgen`을 사용해야 합니다.  [TPC 도구](http://www.tpc.org/tpc_documents_current_versions/current_specifications5.asp)에서 `dbgen`에 대한 원본 코드를 다운로드하여 직접 컴파일하거나, [GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/SamplesV1/TPCHTools)에서 컴파일된 이진 파일을 다운로드할 수 있습니다.  dbgen.exe를 다음 명령과 함께 실행하여 10개 파일에 분산되어 있는 `lineitem` 표에 대한 1TB의 플랫 파일을 생성합니다.

  * `Dbgen -s 1000 -S **1** -C 10 -T L -v`
  * `Dbgen -s 1000 -S **2** -C 10 -T L -v`
  * …
  * `Dbgen -s 1000 -S **10** -C 10 -T L -v`

    이제 생성된 파일을 Azure Blob에 복사합니다.  ADF 복사를 사용하여 이 작업을 하는 방법에 대한 자세한 내용은 [Azure Data Factory를 사용하여 온-프레미스 파일 시스템 간 데이터 이동](data-factory-onprem-file-system-connector.md)을 참조하세요.    
* Azure Synapse Analytics:이 실험은 6000 DWUs를 사용 하 여 만든 Azure Synapse Analytics로 데이터를 로드 합니다.

    Azure Synapse Analytics 데이터베이스를 만드는 방법에 대 한 자세한 지침은 [Azure Synapse 분석 만들기](../../synapse-analytics/sql-data-warehouse/create-data-warehouse-portal.md) 를 참조 하세요.  Polybase를 사용 하 여 Azure Synapse Analytics에 가장 적합 한 부하 성능을 얻으려면 성능 설정에서 허용 되는 최대 DWUs (데이터 웨어하우스 단위) 수 (6000 DWUs)를 선택 합니다.

  > [!NOTE]
  > Azure Blob에서 로드 하는 경우 데이터 로드 성능은 Azure Synapse Analytics에 대해 구성 하는 DWUs의 수에 비례 합니다.
  >
  > 1000 DWU로 1tb를 로드 하는 Azure Synapse Analytics는 87 분 (~ 200 MBps 처리량)으로 1tb를 로드 합니다. 2000 DWU Azure Synapse Analytics는 분 (~ 46 MBps 처리량)으로 1tb를 로드 합니다.
  >
  >

    6000 DWUs를 사용 하 여 전용 SQL 풀을 만들려면 성능 슬라이더를 오른쪽으로 이동 합니다.

    ![성능 슬라이더](media/data-factory-load-sql-data-warehouse/performance-slider.png)

    6,000 DWU를 포함하도록 구성되지 않은 기존 데이터베이스의 경우 Azure Portal을 사용하여 확장할 수 있습니다.  Azure Portal에서 데이터베이스로 이동하면 다음 이미지에 표시된 **개요** 패널에 **크기 조정** 단추가 있습니다.

    ![크기 조정 단추](media/data-factory-load-sql-data-warehouse/scale-button.png)    

    **크기 조정** 단추를 클릭하여 다음 패널을 열고, 슬라이더를 최대값으로 이동하고 나서, **저장** 단추를 클릭합니다.

    ![크기 조정 대화 상자](media/data-factory-load-sql-data-warehouse/scale-dialog.png)

    이 실험은 리소스 클래스를 사용 하 여 Azure Synapse Analytics로 데이터를 로드 `xlargerc` 합니다.

    가능한 최상의 처리량을 얻으려면 리소스 클래스에 속한 Azure Synapse Analytics 사용자를 사용 하 여 복사를 수행 해야 `xlargerc` 합니다.  [사용자 리소스 클래스 변경 예제](../../synapse-analytics/sql-data-warehouse/resource-classes-for-workload-management.md)에 따라 이 작업을 하는 방법을 알아보세요.  
* 다음 DDL 문을 실행 하 여 Azure Synapse Analytics 데이터베이스에서 대상 테이블 스키마를 만듭니다.

    ```SQL  
    CREATE TABLE [dbo].[lineitem]
    (
        [L_ORDERKEY] [bigint] NOT NULL,
        [L_PARTKEY] [bigint] NOT NULL,
        [L_SUPPKEY] [bigint] NOT NULL,
        [L_LINENUMBER] [int] NOT NULL,
        [L_QUANTITY] [decimal](15, 2) NULL,
        [L_EXTENDEDPRICE] [decimal](15, 2) NULL,
        [L_DISCOUNT] [decimal](15, 2) NULL,
        [L_TAX] [decimal](15, 2) NULL,
        [L_RETURNFLAG] [char](1) NULL,
        [L_LINESTATUS] [char](1) NULL,
        [L_SHIPDATE] [date] NULL,
        [L_COMMITDATE] [date] NULL,
        [L_RECEIPTDATE] [date] NULL,
        [L_SHIPINSTRUCT] [char](25) NULL,
        [L_SHIPMODE] [char](10) NULL,
        [L_COMMENT] [varchar](44) NULL
    )
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    ```
  필수 구성 요소 단계가 완료되면 이제 복사 마법사를 사용하여 복사 활동을 구성할 수 있습니다.

## <a name="launch-copy-wizard"></a>복사 마법사 시작
1. [Azure Portal](https://portal.azure.com)에 로그인합니다.
2. 왼쪽 위 모서리에서 **리소스 만들기** 를 클릭하고 **인텔리전스 + 분석** 및 **Data Factory** 를 차례로 클릭합니다.
3. **새 데이터 팩터리** 창에서 다음을 수행합니다.

   1. **이름** 으로 **LoadIntoSQLDWDataFactory** 를 입력합니다.
       Azure Data Factory 이름은 전역적으로 고유해야 합니다. **데이터 팩터리 이름 "LoadIntoSQLDWDataFactory"를 사용할 수 없습니다. 라는** 오류가 표시 되 면 데이터 팩터리의 이름 (예: yournameLoadIntoSQLDWDataFactory)을 변경 하 고 다시 만들어 보세요. 데이터 팩터리 아티팩트에 대한 명명 규칙은 [데이터 팩터리 - 명명 규칙](data-factory-naming-rules.md) 항목을 참조하세요.  
   2. Azure **구독** 을 선택합니다.
   3. 리소스 그룹에 대해 다음 단계 중 하나를 수행합니다.
      1. **기존 항목 사용** 을 선택하고 기존 리소스 그룹을 선택합니다.
      2. **새로 만들기** 를 선택하고 리소스 그룹의 이름을 입력합니다.
   4. Data Factory의 **위치** 를 선택합니다.
   5. 블레이드 하단에서 **대시보드에 고정** 확인란을 선택합니다.  
   6. **만들기** 를 클릭합니다.
4. 만들기가 완료되면 다음 이미지와 같이 **Data Factory** 블레이드가 표시됩니다.

   ![데이터 팩터리 홈페이지](media/data-factory-load-sql-data-warehouse/data-factory-home-page-copy-data.png)
5. 데이터 팩터리 홈 페이지에서 **데이터 복사** 타일을 클릭하여 **복사 마법사** 를 시작합니다.

   > [!NOTE]
   > 웹 브라우저가 "권한 부여..." 상태로 중지된 것을 확인하면 **타사 쿠키 및 사이트 데이터 차단** 설정을 사용 안 함/선택 취소하고 (또는) 계속 사용하지 않습니다. 그리고 **login.microsoftonline.com** 에 대한 예외를 만든 다음 마법사를 다시 시작해봅니다.
   >
   >

## <a name="step-1-configure-data-loading-schedule"></a>1단계: 데이터 로드 일정 구성
첫 번째 단계에서는 데이터 로드 일정을 구성합니다.  

**속성** 페이지에서 다음을 수행합니다.

1. **작업 이름** 으로 **CopyFromBlobToAzureSqlDataWarehouse** 를 입력합니다.
2. **지금 한 번 실행** 옵션을 선택합니다.   
3. **다음** 을 클릭합니다.  

    ![복사 마법사 - 속성 페이지](media/data-factory-load-sql-data-warehouse/copy-wizard-properties-page.png)

## <a name="step-2-configure-source"></a>2단계: 원본 구성
이 섹션에서는 1TB TPC-H 라인 항목 파일이 포함된 원본: Azure Blob을 구성하는 단계를 보여 줍니다.

1. 데이터 저장소로 **Azure Blob Storage** 를 선택하고 **다음** 을 클릭합니다.

    ![복사 마법사 - 원본 페이지 선택](media/data-factory-load-sql-data-warehouse/select-source-connection.png)

2. Azure Blob Storage 계정의 연결 정보를 입력하고 **다음** 을 클릭합니다.

    ![복사 마법사 - 원본 연결 정보](media/data-factory-load-sql-data-warehouse/source-connection-info.png)

3. TPC-H 라인 항목 파일이 포함된 **폴더** 를 선택하고 **다음** 을 클릭합니다.

    ![복사 마법사 - 입력 폴더 선택](media/data-factory-load-sql-data-warehouse/select-input-folder.png)

4. **다음** 을 클릭하면 파일 형식 설정이 자동으로 검색됩니다.  열 구분 기호가 기본 쉼표 ', ' 대신 ' | ' 인지 확인 하십시오.  데이터를 미리 본 후 **다음** 을 클릭합니다.

    ![복사 마법사 - 파일 형식 설정](media/data-factory-load-sql-data-warehouse/file-format-settings.png)

## <a name="step-3-configure-destination"></a>3단계: 대상 구성
이 섹션에서는 `lineitem` Azure Synapse Analytics 데이터베이스에서 대상: 테이블을 구성 하는 방법을 보여 줍니다.

1. **Azure Synapse Analytics** 를 대상 저장소로 선택 하 고 **다음** 을 클릭 합니다.

    ![복사 마법사 - 대상 데이터 저장소 선택](media/data-factory-load-sql-data-warehouse/select-destination-data-store.png)

2. Azure Synapse Analytics에 대 한 연결 정보를 입력 합니다.  `xlargerc` 역할(자세한 내용은 **필수 구성 요소** 섹션 참조)의 구성원인 사용자를 지정하는지 확인하고 **다음** 을 클릭합니다.

    ![복사 마법사 - 대상 연결 정보](media/data-factory-load-sql-data-warehouse/destination-connection-info.png)

3. 대상 테이블을 선택하고 **다음** 을 클릭합니다.

    ![복사 마법사 - 테이블 매핑 페이지](media/data-factory-load-sql-data-warehouse/table-mapping-page.png)

4. 스키마 매핑 페이지에서 "열 매핑 적용" 옵션을 선택 취소하고 **다음** 을 클릭합니다.

## <a name="step-4-performance-settings"></a>4단계: 성능 설정

**Polybase 허용** 은 기본적으로 선택됩니다.  **다음** 을 클릭합니다.

![복사 마법사 - 스키마 매핑 페이지](media/data-factory-load-sql-data-warehouse/performance-settings-page.png)

## <a name="step-5-deploy-and-monitor-load-results"></a>5단계: 로드 결과 배포 및 모니터링
1. **마침** 단추를 클릭하여 배포합니다.

    ![복사 마법사-요약 페이지 1](media/data-factory-load-sql-data-warehouse/summary-page.png)

2. 배포가 완료된 후 `Click here to monitor copy pipeline`을 클릭하여 복사 실행 진행률을 모니터링합니다. **Activity Windows** (활동 기간) 목록에서 직접 만든 복사 파이프라인을 선택합니다.

    ![복사 마법사-요약 페이지 2](media/data-factory-load-sql-data-warehouse/select-pipeline-monitor-manage-app.png)

    오른쪽 패널의 **Activity Window Explorer** (활동 기간 탐색기)에서는 원본에서 읽고 대상에 쓴 데이터 볼륨, 기간, 평균 실행 처리량을 비롯한 복사 실행 세부 정보를 확인할 수 있습니다.

    다음 스크린샷에서 볼 수 있듯이 Azure Blob Storage 1TB를 Azure Synapse Analytics로 복사 하는 데 14 분이 걸리며 1.22 g b 처리량을 효과적으로 달성 했습니다.

    ![복사 마법사 - 성공 대화 상자](media/data-factory-load-sql-data-warehouse/succeeded-info.png)

## <a name="best-practices"></a>모범 사례
Azure Synapse Analytics 데이터베이스를 실행 하는 몇 가지 모범 사례는 다음과 같습니다.

* 클러스터형 COLUMNSTORE 인덱스로 로드될 경우 더 큰 리소스 클래스를 사용합니다.
* 더 효율적으로 조인하려면 기본 라운드 로빈 배포가 아닌 열 선택에 의한 해시 배포를 사용하는 것이 좋습니다.
* 로드 속도를 높이려면 임시 데이터에 힙을 사용하는 것이 좋습니다.
* Azure Synapse Analytics에 대 한 로드를 완료 한 후에 통계를 만듭니다.

자세한 내용은 [Azure Synapse Analytics 모범 사례](../../synapse-analytics/sql-data-warehouse/sql-data-warehouse-best-practices.md) 를 참조 하세요.

## <a name="next-steps"></a>다음 단계
* [Data Factory 복사 마법사](data-factory-copy-wizard.md) - 이 문서에서는 복사 마법사에 대해 자세히 설명합니다.
* [복사 작업 성능 및 조정 가이드](data-factory-copy-activity-performance.md) - 이 문서에는 참조 성능 측정값과 조정 가이드가 포함됩니다.