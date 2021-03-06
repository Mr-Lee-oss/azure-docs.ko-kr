---
title: Azure SQL Database에 대한 일반적인 연결 문제 해결
description: Azure SQL Database 연결 문제를 해결 하 고 다른 Azure SQL Database 또는 Azure SQL Managed Instance 특정 문제를 해결 하는 단계를 제공 합니다.
services: sql-database
ms.service: sql-db-mi
ms.subservice: development
ms.topic: troubleshooting
ms.custom: seo-lt-2019, OKR 11/2019, sqldbrb=1
author: ramakoni1
ms.author: ramakoni
ms.reviewer: sstein,vanto
ms.date: 01/14/2020
ms.openlocfilehash: bcf11ef9b64a02383aad5175c19c5db58c3c39cf
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/28/2020
ms.locfileid: "92791344"
---
# <a name="troubleshooting-connectivity-issues-and-other-errors-with-azure-sql-database-and-azure-sql-managed-instance"></a>Azure SQL Database 및 Azure SQL Managed Instance를 사용 하 여 연결 문제 및 기타 오류 해결
[!INCLUDE[appliesto-sqldb-sqlmi](../includes/appliesto-sqldb-sqlmi.md)]

Azure SQL Database 또는 Azure SQL Managed Instance에 대 한 연결이 실패 하면 오류 메시지가 표시 됩니다. 이러한 연결 문제는 재구성, 방화벽 설정, 연결 제한 시간, 잘못 된 로그인 정보 또는 [응용 프로그램 디자인](develop-overview.md) 프로세스 중에 모범 사례 및 디자인 지침을 적용 하지 못한 경우에 발생할 수 있습니다. 또한 일부 Azure SQL Database 또는 SQL Managed Instance 리소스에 대 한 최대 제한에 도달 하면 더 이상 연결할 수 없습니다.

## <a name="transient-fault-error-messages-40197-40613-and-others"></a>일시적인 오류 메시지 (40197, 40613 및 기타)

Azure 인프라에 는 SQL Database 서비스에 과도한 워크로드 부하가 발생하는 경우 서버를 동적으로 다시 구성 하는 기능이 있습니다.  이 동적 동작으로 인해 클라이언트 프로그램에서 데이터베이스 또는 인스턴스에 대 한 연결이 끊어질 수 있습니다. 이러한 종류의 오류 상황을 *일시적 장애* 라고 합니다. 데이터베이스 재구성 이벤트는 계획된 이벤트(예: 소프트웨어 업그레이드) 또는 계획되지 않은 이벤트(예: 프로세스 충돌 또는 부하 분산) 때문에 발생합니다. 대부분의 다시 구성 이벤트는 보통 일시적으로 발생하며 최대 60초 이내에 완료됩니다. 그러나 큰 트랜잭션으로 인해 복구가 오래 실행되는 등 이러한 이벤트가 완료되는 데 시간이 더 오래 걸리는 경우도 있습니다. 다음 표에는에 연결할 때 응용 프로그램에서 받을 수 있는 다양 한 일시적인 오류가 나열 되어 있습니다 SQL Database

### <a name="list-of-transient-fault-error-codes"></a>일시적인 오류 코드 목록

| 오류 코드 | 심각도 | 설명 |
| ---:| ---:|:--- |
| 4060 |16 |로그인에서 요청된 데이터베이스 "%.&#x2a;ls"을(를) 열 수 없습니다. 로그인이 실패했습니다. 자세한 내용은 [오류 4000 ~ 4999](/sql/relational-databases/errors-events/database-engine-events-and-errors#errors-4000-to-4999) 를 참조 하세요.|
| 40197 |17 |서비스에서 요청을 처리하는 오류가 발생했습니다. 나중에 다시 시도하세요. 오류 코드 %d.<br/><br/>소프트웨어 또는 하드웨어 업그레이드, 하드웨어 오류 또는 기타 장애 조치 문제로 인해 서비스가 종료되는 경우 이 오류가 발생합니다. 오류 40197 메시지 내에 포함된 오류 코드(%d)는 발생한 오류 또는 장애 조치의 종류에 대한 추가 정보를 제공합니다. 오류 40197 메시지 내에 포함된 오류 코드의 일부 예제는 40020, 40143, 40166 및 40540입니다.<br/><br/>다시 연결 하면 데이터베이스의 정상 복사본으로 자동 연결 됩니다. 애플리케이션은 오류 40197을 포착하고 문제 해결을 위해 메시지 내에 포함된 오류 코드(%d)를 로그하고 리소스가 사용 가능하고 연결이 다시 설정될 때까지 SQL Database에 다시 접속을 시도해야 합니다. 자세한 내용은 [일시적인 오류](troubleshoot-common-connectivity-issues.md#transient-errors-transient-faults)를 참조 하세요.|
| 40501 |20 |서비스가 현재 사용 중입니다. 10초 후 요청을 다시 시도하십시오. 인시던트 ID: %ls. 코드: %d. 자세한 내용은 다음을 참조하세요. <br/>&bull;&nbsp; [논리 SQL server 리소스 제한](resource-limits-logical-server.md)<br/>&bull;&nbsp; [단일 데이터베이스에 대 한 DTU 기반 제한](service-tiers-dtu.md)<br/>&bull;&nbsp; [탄력적 풀에 대 한 DTU 기반 제한](resource-limits-dtu-elastic-pools.md)<br/>&bull;&nbsp; [단일 데이터베이스에 대 한 vcore 기반 제한](resource-limits-vcore-single-databases.md)<br/>&bull;&nbsp; [탄력적 풀에 대 한 vcore 기반 제한](resource-limits-vcore-elastic-pools.md)<br/>&bull;&nbsp; [Azure SQL Managed Instance 리소스 제한](../managed-instance/resource-limits.md)입니다.|
| 40613 |17 |서버의 데이터베이스 '%.&#x2a;ls' '%.&#x2a;ls'을(를) 사용할 수 없습니다. 나중에 연결을 다시 시도하십시오. 문제가 지속되면 고객 지원 서비스에 문의하고 세션 추적 ID '%.&#x2a;ls'을(를) 제공하십시오.<br/><br/> 이 오류는 데이터베이스에 기존 DAC (관리자 전용 연결)가 이미 설정 된 경우에 발생할 수 있습니다. 자세한 내용은 [일시적인 오류](troubleshoot-common-connectivity-issues.md#transient-errors-transient-faults)를 참조 하세요.|
| 49918 |16 |요청을 처리할 수 없습니다. 요청을 처리할 리소스가 부족합니다.<br/><br/>서비스가 현재 사용 중입니다. 요청을 나중에 다시 시도하세요. 자세한 내용은 다음을 참조하세요. <br/>&bull;&nbsp; [논리 SQL server 리소스 제한](resource-limits-logical-server.md)<br/>&bull;&nbsp; [단일 데이터베이스에 대 한 DTU 기반 제한](service-tiers-dtu.md)<br/>&bull;&nbsp; [탄력적 풀에 대 한 DTU 기반 제한](resource-limits-dtu-elastic-pools.md)<br/>&bull;&nbsp; [단일 데이터베이스에 대 한 vcore 기반 제한](resource-limits-vcore-single-databases.md)<br/>&bull;&nbsp; [탄력적 풀에 대 한 vcore 기반 제한](resource-limits-vcore-elastic-pools.md)<br/>&bull;&nbsp; [Azure SQL Managed Instance 리소스 제한](../managed-instance/resource-limits.md)입니다. |
| 49919 |16 |요청을 만들거나 업데이트하는 처리를 할 수 없습니다. 구독 "%ld"에 대해 진행 중인 작업을 너무 많이 만들거나 업데이트합니다.<br/><br/>서비스가 구독 또는 서버에 대한 여러 요청을 만들거나 업데이트하는 처리로 사용 중입니다. 요청은 현재 리소스 최적화에 대해 차단됩니다. 보류 중인 작업에 대해 [sys.dm_operation_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database)를 쿼리합니다. 만들기 또는 업데이트를 보류 중인 요청이 완료되거나 보류 중인 요청 중 하나를 삭제할 때까지 대기하고 나중에 요청을 다시 시도합니다. 자세한 내용은 다음을 참조하세요. <br/>&bull;&nbsp; [논리 SQL server 리소스 제한](resource-limits-logical-server.md)<br/>&bull;&nbsp; [단일 데이터베이스에 대 한 DTU 기반 제한](service-tiers-dtu.md)<br/>&bull;&nbsp; [탄력적 풀에 대 한 DTU 기반 제한](resource-limits-dtu-elastic-pools.md)<br/>&bull;&nbsp; [단일 데이터베이스에 대 한 vcore 기반 제한](resource-limits-vcore-single-databases.md)<br/>&bull;&nbsp; [탄력적 풀에 대 한 vcore 기반 제한](resource-limits-vcore-elastic-pools.md)<br/>&bull;&nbsp; [Azure SQL Managed Instance 리소스 제한](../managed-instance/resource-limits.md)입니다. |
| 49920 |16 |요청을 처리할 수 없습니다. 구독 "%ld"에 대해 진행 중인 작업을 너무 많습니다.<br/><br/>서비스가 구독에 대한 여러 요청을 처리하는 데 사용 중입니다. 요청은 현재 리소스 최적화에 대해 차단됩니다. 작업 상태에 대해 [sys.dm_operation_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database)를 쿼리합니다. 보류 중인 요청이 완료되거나 보류 중인 요청 중 하나를 삭제할 때까지 대기하고 나중에 요청을 다시 시도합니다. 자세한 내용은 다음을 참조하세요. <br/>&bull;&nbsp; [논리 SQL server 리소스 제한](resource-limits-logical-server.md)<br/>&bull;&nbsp; [단일 데이터베이스에 대 한 DTU 기반 제한](service-tiers-dtu.md)<br/>&bull;&nbsp; [탄력적 풀에 대 한 DTU 기반 제한](resource-limits-dtu-elastic-pools.md)<br/>&bull;&nbsp; [단일 데이터베이스에 대 한 vcore 기반 제한](resource-limits-vcore-single-databases.md)<br/>&bull;&nbsp; [탄력적 풀에 대 한 vcore 기반 제한](resource-limits-vcore-elastic-pools.md)<br/>&bull;&nbsp; [Azure SQL Managed Instance 리소스 제한](../managed-instance/resource-limits.md)입니다. |
| 4221 |16 |'HADR_DATABASE_WAIT_FOR_TRANSITION_TO_VERSIONING'에서 긴 대기로 인해 읽기 보조에 대한 로그인에 실패했습니다. 행 버전이 복제본이 재활용될 때 처리 중이었던 트랜잭션에 대해 누락되었기 때문에 로그인에 복제본을 사용할 수 없습니다. 주 복제본에서 활성 트랜잭션을 롤백하거나 커밋하여 문제를 해결할 수 있습니다. 주 복제본에서 긴 쓰기 트랜잭션을 방지하여 이 조건의 발생을 최소화할 수 있습니다. |

### <a name="steps-to-resolve-transient-connectivity-issues"></a>일시적인 연결 문제를 해결하는 단계

1. [Microsoft Azure 서비스 대시보드](https://azure.microsoft.com/status)에서 애플리케이션이 오류를 보고한 시간 동안 발생한 알려진 서비스 중단을 확인합니다.
2. Azure SQL Database와 같이 클라우드 서비스에 연결하는 애플리케이션에서는 주기적으로 다시 구성 이벤트가 발생하므로, 이러한 이벤트를 사용자에게 애플리케이션 오류로 표시하는 대신 해당 오류를 처리하는 다시 시도 논리를 구현해야 합니다.
3. 데이터베이스가 리소스 한계에 도달하면 일시적인 연결 문제가 발생한 것처럼 보일 수 있습니다. [리소스 한도](resource-limits-logical-server.md#what-happens-when-database-resource-limits-are-reached)를 참조하세요.
4. 연결 문제가 계속 발생하거나 애플리케이션에서 오류가 발생하는 기간이 60초를 초과하는 경우 또는 특정일에 오류가 여러 번 발생하는 경우에는 **Azure 지원** 사이트에서 [지원 받기](https://azure.microsoft.com/support/options)를 선택하여 Azure 지원 요청을 접수합니다.

#### <a name="implementing-retry-logic"></a>재시도 논리 구현

자체 해결을 위한 일시적 오류 시간이 지난 후 연결을 다시 시도할 수 있도록 클라이언트 프로그램에 재시도 논리가 있는 것이 좋습니다.  첫 번째 재시도 전에 5초간 지연하는 것이 좋습니다. 5초보다 짧은 지연 후 재시도는 클라우드 서비스에 많은 위험이 있습니다. 각 후속 재시도에 대해 지연 시간은 최대 60초까지 기하급수적으로 증가해야 합니다.

다시 시도 논리의 코드 예제는 다음을 참조하십시오.

- [ADO.NET을 사용하여 탄력적으로 SQL에 연결](/sql/connect/ado-net/step-4-connect-resiliently-sql-ado-net)
- [PHP를 사용하여 탄력적으로 SQL에 연결](/sql/connect/php/step-4-connect-resiliently-to-sql-with-php)

응용 프로그램에서 일시적인 오류를 처리 하는 방법에 대 한 자세한 내용은 [SQL Database 일시적 연결 오류 문제 해결을](troubleshoot-common-connectivity-issues.md) 참조 하세요.

ADO.NET를 사용 하는 클라이언트에 대 한 *차단 기간* 에 대 한 설명은 [연결 풀링 (ADO.NET)](/dotnet/framework/data/adonet/sql-server-connection-pooling)에서 사용할 수 있습니다.

## <a name="a-network-related-or-instance-specific-error-occurred-while-establishing-a-connection-to-your-server"></a>서버에 대 한 연결을 설정 하는 동안 네트워크 관련 또는 인스턴스 관련 오류가 발생 했습니다.

응용 프로그램에서 서버에 연결할 수 없는 경우이 문제가 발생 합니다.

이 문제를 해결 하려면 [일반적인 연결 문제를 해결 하는 단계](#steps-to-fix-common-connection-issues) 섹션에서 제시 된 순서 대로 단계를 시도 합니다.

## <a name="the-serverinstance-was-not-found-or-was-not-accessible-errors-26-40-10053"></a>서버/인스턴스를 찾을 수 없거나 액세스할 수 없습니다 (오류 26, 40, 10053).

### <a name="error-26-error-locating-server-specified"></a>오류 26: 지정 된 서버를 찾는 동안 오류 발생

``System.Data.SqlClient.SqlException: A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections.(provider: SQL Network Interfaces, error: 26 – Error Locating Server/Instance Specified)``

#### <a name="error-40-could-not-open-a-connection-to-the-server"></a>오류 40: 서버에 대 한 연결을 열 수 없습니다.

``A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections. (provider: Named Pipes Provider, error: 40 - Could not open a connection to SQL Server)``

#### <a name="error-10053-a-transport-level-error-has-occurred-when-receiving-results-from-the-server"></a>오류 10053: 서버에서 결과를 수신 하는 동안 전송 수준 오류가 발생 했습니다.

``10053: A transport-level error has occurred when receiving results from the server. (Provider: TCP Provider, error: 0 - An established connection was aborted by the software in your host machine)``

이러한 문제는 응용 프로그램에서 서버에 연결할 수 없는 경우에 발생 합니다.

이러한 문제를 해결 하려면 [일반적인 연결 문제를 해결 하는 단계](#steps-to-fix-common-connection-issues) 섹션에서 제시 된 순서 대로 단계를 시도 합니다.

## <a name="cannot-connect-to-server-due-to-firewall-issues"></a>방화벽 문제로 인해 서버에 연결할 수 없습니다.

### <a name="error-40615-cannot-connect-to--servername-"></a>오류 40615: < servername >에 연결할 수 없습니다.

이 문제를 해결 하려면 [Azure Portal를 통해 SQL Database에서 방화벽 설정을 구성](firewall-configure.md)합니다.

### <a name="error-5-cannot-connect-to--servername-"></a>오류 5: < servername >에 연결할 수 없습니다.

이 문제를 해결 하려면 클라이언트와 인터넷 간의 모든 방화벽에서 아웃 바운드 연결에 대 한 포트 1433가 열려 있는지 확인 합니다.

## <a name="unable-to-log-in-to-the-server-errors-18456-40531"></a>서버에 로그인 할 수 없습니다 (오류 18456, 40531).

### <a name="login-failed-for-user--user-name-"></a>사용자 ' < 사용자 이름 > '에 대해 로그인 하지 못했습니다.

``Login failed for user '<User name>'.This session has been assigned a tracing ID of '<Tracing ID>'. Provide this tracing ID to customer support when you need assistance. (Microsoft SQL Server, Error: 18456)``

이 문제를 해결 하려면 올바른 사용자 이름과 암호를 제공 하도록 서비스 관리자에 게 문의 하십시오.

일반적으로 서비스 관리자는 다음 단계를 사용 하 여 로그인 자격 증명을 추가할 수 있습니다.

1. SSMS (SQL Server Management Studio)를 사용 하 여 서버에 로그인 합니다.
2. 다음 SQL 쿼리를 실행 하 여 로그인 이름이 사용 하지 않도록 설정 되어 있는지 확인 합니다.

   ```sql
   SELECT name, is_disabled FROM sys.sql_logins
   ```

3. 해당 이름이 비활성화된 경우 다음 문을 사용하여 활성화합니다.

   ```sql
   Alter login <User name> enable
   ```

4. SQL 로그인 사용자 이름이 존재 하지 않는 경우 다음 단계를 수행 하 여 만듭니다.

   1. SSMS에서 **보안** 을 두 번 클릭 하 여 확장 합니다.
   2. **로그인** 을 마우스 오른쪽 단추로 클릭 한 다음 **새 로그인** 을 선택 합니다.
   3. 자리 표시 자가 있는 생성 된 스크립트에서 다음 SQL 쿼리를 편집 하 고 실행 합니다.

   ```sql
   CREATE LOGIN <SQL_login_name, sysname, login_name>
   WITH PASSWORD = ‘<password, sysname, Change_Password>’
   GO
   ```

5. **데이터베이스** 를 두 번 클릭 합니다.
6. 사용자에 게 권한을 부여 하려는 데이터베이스를 선택 합니다.
7. **보안** 을 두 번 클릭 합니다.
8. **사용자** 를 마우스 오른쪽 단추로 클릭 한 다음 **새 사용자** 를 선택 합니다.
9. 자리 표시 자가 있는 생성 된 스크립트에서 다음 SQL 쿼리를 편집 하 고 실행 합니다.

   ```sql
   CREATE USER <user_name, sysname, user_name>
   FOR LOGIN <login_name, sysname, login_name>
   WITH DEFAULT_SCHEMA = <default_schema, sysname, dbo>
   GO
   -- Add user to the database owner role

   EXEC sp_addrolemember N’db_owner’, N’<user_name, sysname, user_name>’
   GO
   ```

   > [!NOTE]
   > 를 사용 하 여 특정 `sp_addrolemember` 사용자를 특정 데이터베이스 역할에 매핑할 수도 있습니다.

자세한 내용은 [Azure SQL Database에서 데이터베이스 및 로그인 관리](./logins-create-manage.md)를 참조 하세요.

## <a name="connection-timeout-expired-errors"></a>연결 시간 제한 만료 오류

### <a name="systemdatasqlclientsqlexception-0x80131904-connection-timeout-expired"></a>SqlException (0x80131904): 연결 제한 시간이 만료 되었습니다.

``System.Data.SqlClient.SqlException (0x80131904): Connection Timeout Expired. The timeout period elapsed while attempting to consume the pre-login handshake acknowledgement. This could be because the pre-login handshake failed or the server was unable to respond back in time. The duration spent while attempting to connect to this server was - [Pre-Login] initialization=3; handshake=29995;``

### <a name="systemdatasqlclientsqlexception-0x80131904-timeout-expired"></a>SqlException (0x80131904): 시간 제한이 만료 되었습니다.

``System.Data.SqlClient.SqlException (0x80131904): Timeout expired. The timeout period elapsed prior to completion of the operation or the server is not responding.``

### <a name="systemdataentitycoreentityexception-the-underlying-provider-failed-on-open"></a>System.object. EntityException: 기본 공급자가 열기에 실패 했습니다.

``System.Data.Entity.Core.EntityException: The underlying provider failed on Open. -> System.Data.SqlClient.SqlException: Timeout expired. The timeout period elapsed prior to completion of the operation or the server is not responding. -> System.ComponentModel.Win32Exception: The wait operation timed out``

### <a name="cannot-connect-to--server-name-"></a>< 서버 이름에 연결할 수 없습니다 >

``Cannot connect to <server name>.ADDITIONAL INFORMATION:Connection Timeout Expired. The timeout period elapsed during the post-login phase. The connection could have timed out while waiting for server to complete the login process and respond; Or it could have timed out while attempting to create multiple active connections. The duration spent while attempting to connect to this server was - [Pre-Login] initialization=231; handshake=983; [Login] initialization=0; authentication=0; [Post-Login] complete=13000; (Microsoft SQL Server, Error: -2) For help, click: http://go.microsoft.com/fwlink?ProdName=Microsoft%20SQL%20Server&EvtSrc=MSSQLServer&EvtID=-2&LinkId=20476 The wait operation timed out``

이러한 예외는 연결 또는 쿼리 문제로 인해 발생할 수 있습니다. 연결 문제로 인해이 오류가 발생 했는지 확인 하려면 [연결 문제로 인해 오류가 발생 했는지](#confirm-whether-an-error-is-caused-by-a-connectivity-issue)확인을 참조 하세요.

응용 프로그램에서 서버에 연결할 수 없기 때문에 연결 제한 시간이 발생 합니다. 이 문제를 해결 하려면 [일반적인 연결 문제를 해결 하는 단계](#steps-to-fix-common-connection-issues) 섹션에서 제시 된 순서 대로 단계를 시도 합니다.

## <a name="resource-governance-errors"></a>리소스 관리 오류

### <a name="error-10928-resource-id-d"></a>오류 10928: 리소스 ID:% d

``10928: Resource ID: %d. The %s limit for the database is %d and has been reached. See http://go.microsoft.com/fwlink/?LinkId=267637 for assistance. The Resource ID value in error message indicates the resource for which limit has been reached. For sessions, Resource ID = 2.``

이 문제를 해결 하려면 다음 방법 중 하나를 시도해 보세요.

- 장기 실행 쿼리가 있는지 여부를 확인 합니다.

  > [!NOTE]
  > 이는 문제를 해결 하지 못할 수 있는 전적 방법입니다.

1. 다음 SQL 쿼리를 실행 하 여 [sys.dm_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) 보기를 확인 하 고 차단 요청을 확인 합니다.

   ```sql
   SELECT * FROM dm_exec_requests
   ```

2. 헤드 차단기에 대 한 **입력 버퍼** 를 확인 합니다.
3. 헤드 차단기 쿼리를 조정 합니다.

   심층 문제 해결 절차는 [내 쿼리가 클라우드에서 제대로 실행 되나요?](/archive/blogs/sqlblog/is-my-query-running-fine-in-the-cloud)를 참조 하세요.

블로킹 및 장기 실행 쿼리를 처리 하는 것에도 불구 하 고 데이터베이스가 지속적으로 제한에 도달한 경우 더 많은 리소스 [버전이](https://azure.microsoft.com/pricing/details/sql-database/)있는 버전으로 업그레이드 하는 것이 좋습니다.

동적 관리 뷰에 대 한 자세한 내용은 [시스템 동적 관리 뷰](/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views)를 참조 하세요.

데이터베이스 제한에 대 한 자세한 내용은  [서버에 대 한 리소스 제한 SQL Database](./resource-limits-logical-server.md)을 참조 하세요.

### <a name="error-10929-resource-id-1"></a>오류 10929: 리소스 ID: 1

``10929: Resource ID: 1. The %s minimum guarantee is %d, maximum limit is %d and the current usage for the database is %d. However, the server is currently too busy to support requests greater than %d for this database. See http://go.microsoft.com/fwlink/?LinkId=267637 for assistance. Otherwise, please try again later.``

### <a name="error-40501-the-service-is-currently-busy"></a>오류 40501: 서비스가 현재 사용 중입니다.

``40501: The service is currently busy. Retry the request after 10 seconds. Incident ID: %ls. Code: %d.``

이는 리소스 제한을 초과 했음을 나타내는 엔진 제한 오류입니다.

리소스 제한에 대 한 자세한 내용은 [논리 SQL server 리소스 제한](./resource-limits-logical-server.md)을 참조 하세요.

### <a name="error-40544-the-database-has-reached-its-size-quota"></a>오류 40544: 데이터베이스가 크기 할당량에 도달 했습니다.

``40544: The database has reached its size quota. Partition or delete data, drop indexes, or consult the documentation for possible resolutions. Incident ID: <ID>. Code: <code>.``

이 오류는 데이터베이스가 크기 할당량에 도달 했을 때 발생 합니다.

다음 단계를 통해 문제를 해결 하거나 추가 옵션을 제공할 수 있습니다.

1. Azure Portal에서 대시보드를 사용 하 여 데이터베이스의 현재 크기를 확인 합니다.

   > [!NOTE]
   > 가장 많은 공간을 사용 하는 테이블을 식별 하기 때문에 정리에 대 한 잠재적 후보가 다음 SQL 쿼리를 실행 합니다.

   ```sql
   SELECT o.name,
    SUM(p.row_count) AS 'Row Count',
    SUM(p.reserved_page_count) * 8.0 / 1024 AS 'Table Size (MB)'
   FROM sys.objects o
   JOIN sys.dm_db_partition_stats p on p.object_id = o.object_id
   GROUP BY o.name
   ORDER BY [Table Size (MB)] DESC
   ```

2. 현재 크기가 버전에 대해 지원 되는 최대 크기를 초과 하지 않는 경우 ALTER DATABASE를 사용 하 여 MAXSIZE 설정을 늘릴 수 있습니다.
3. 데이터베이스가 버전에 대해 지원 되는 최대 크기를 이미 초과 하는 경우 다음 단계 중 하나 이상을 시도 합니다.

   - 일반 데이터베이스 정리 작업을 수행 합니다. 예를 들어 잘라내기/삭제를 사용 하 여 원치 않는 데이터를 정리 하거나 SSIS (SQL Server Integration Services) 또는 bcp (대량 복사 프로그램) 유틸리티를 사용 하 여 데이터를 이동 합니다.
   - 데이터를 분할 또는 삭제하거나 인덱스를 삭제하거나 가능한 해결 방법에 대한 설명서를 참조하십시오.
   - 데이터베이스 크기를 조정 하려면 [단일 데이터베이스 리소스 크기 조정](./single-database-scale.md) 및 [탄력적 풀 리소스 크기 조정](./elastic-pool-scale.md)을 참조 하세요.

### <a name="error-40549-session-is-terminated-because-you-have-a-long-running-transaction"></a>오류 40549: 장기 실행 트랜잭션이 있으므로 세션이 종료 되었습니다.

``40549: Session is terminated because you have a long-running transaction. Try shortening your transaction.``

이 오류가 반복적으로 발생 하는 경우 다음 단계를 수행 하 여 문제를 해결 해 보십시오.

1. Sys.dm_exec_requests 뷰를 확인 하 여 total_elapsed_time 열에 대해 값이 높은 열려 있는 세션을 확인 합니다. 다음 SQL 스크립트를 실행 하 여이 확인을 수행 합니다.

   ```sql
   SELECT * FROM dm_exec_requests
   ```

2. 장기 실행 쿼리에 대 한 입력 버퍼를 확인 합니다.
3. 쿼리를 튜닝 합니다.

또한 쿼리를 일괄 처리 하는 것이 좋습니다. 일괄 처리에 대 한 자세한 내용은 [일괄 처리를 사용 하 여 SQL Database 응용 프로그램 성능 향상](../performance-improve-use-batching.md)을 참조 하세요.

심층 문제 해결 절차는 [내 쿼리가 클라우드에서 제대로 실행 되나요?](/archive/blogs/sqlblog/is-my-query-running-fine-in-the-cloud)를 참조 하세요.

### <a name="error-40551-the-session-has-been-terminated-because-of-excessive-tempdb-usage"></a>오류 40551: TEMPDB 사용량이 너무 많아 세션이 종료 되었습니다.

``40551: The session has been terminated because of excessive TEMPDB usage. Try modifying your query to reduce the temporary table space usage.``

이 문제를 해결하려면 아래 단계를 수행합니다.

1. 임시 테이블 공간 사용량을 줄이기 위해 쿼리를 변경 합니다.
2. 임시 개체가 더 이상 필요 하지 않은 경우 삭제 합니다.
3. 테이블을 자르거나 사용 하지 않는 테이블을 제거 합니다.

### <a name="error-40552-the-session-has-been-terminated-because-of-excessive-transaction-log-space-usage"></a>오류 40552: 과도 한 트랜잭션 로그 공간 사용으로 인해 세션이 종료 되었습니다.

``40552: The session has been terminated because of excessive transaction log space usage. Try modifying fewer rows in a single transaction.``

이 문제를 해결하려면 다음 방법을 사용해 보세요.

- 이 문제는 삽입, 업데이트 또는 삭제 작업으로 인해 발생할 수 있습니다.
일괄 처리 또는 분할을 여러 개의 작은 트랜잭션으로 구현 하 여 즉시 작동 하는 행 수를 줄여 보세요.
- 인덱스 다시 작성 작업으로 인해이 문제가 발생할 수 있습니다. 이 문제를 해결 하려면 테이블의 영향을 받는 행 수 * (업데이트 된 필드의 평균 크기 (바이트 + 80)) < 2 기가바이트 (GB)를 확인 합니다.

  > [!NOTE]
  > 인덱스를 다시 작성 하는 경우 업데이트 되는 필드의 평균 크기를 평균 인덱스 크기로 대체 해야 합니다.

### <a name="error-40553-the-session-has-been-terminated-because-of-excessive-memory-usage"></a>오류 40553: 과도 한 메모리 사용으로 인해 세션이 종료 되었습니다.

``40553 : The session has been terminated because of excessive memory usage. Try modifying your query to process fewer rows.``

이 문제를 해결 하려면 쿼리를 최적화 해 보세요.

심층 문제 해결 절차는 [내 쿼리가 클라우드에서 제대로 실행 되나요?](/archive/blogs/sqlblog/is-my-query-running-fine-in-the-cloud)를 참조 하세요.

### <a name="table-of-additional-resource-governance-error-messages"></a>추가 리소스 거 버 넌 스 오류 메시지 표

| 오류 코드 | 심각도 | 설명 |
| ---:| ---:|:--- |
| 10928 |20 |리소스 ID: %d입니다. 데이터베이스에 대한 %s 제한이 %d이며 이 제한에 도달했습니다. 자세한 내용은 [단일 데이터베이스 및 풀링된 데이터베이스에 대한 SQL Database 리소스 한도](resource-limits-logical-server.md)를 참조하세요.<br/><br/>리소스 ID는 제한에 도달한 리소스를 나타냅니다. 작업자 스레드의 경우 리소스 ID = 1입니다. 세션의 경우 리소스 ID = 2입니다.<br/><br/>이 오류 및 문제를 해결하는 방법에 대한 자세한 내용은 다음을 참조하세요. <br/>&bull;&nbsp; [논리 SQL server 리소스 제한](resource-limits-logical-server.md)<br/>&bull;&nbsp; [단일 데이터베이스에 대 한 DTU 기반 제한](service-tiers-dtu.md)<br/>&bull;&nbsp; [탄력적 풀에 대 한 DTU 기반 제한](resource-limits-dtu-elastic-pools.md)<br/>&bull;&nbsp; [단일 데이터베이스에 대 한 vcore 기반 제한](resource-limits-vcore-single-databases.md)<br/>&bull;&nbsp; [탄력적 풀에 대 한 vcore 기반 제한](resource-limits-vcore-elastic-pools.md)<br/>&bull;&nbsp; [Azure SQL Managed Instance 리소스 제한](../managed-instance/resource-limits.md)입니다. |
| 10929 |20 |리소스 ID: %d입니다. %s의 최소 보장은 %d이며, 최대 한도는 %d이고, 해당 데이터베이스의 현재 사용량은 %d입니다. 하지만 현재 서버 사용량이 너무 많아 해당 데이터베이스에 대해 %d 이상의 요청을 지원할 수 없습니다. 리소스 ID는 제한에 도달한 리소스를 나타냅니다. 작업자 스레드의 경우 리소스 ID = 1입니다. 세션의 경우 리소스 ID = 2입니다. 자세한 내용은 다음을 참조하세요. <br/>&bull;&nbsp; [논리 SQL server 리소스 제한](resource-limits-logical-server.md)<br/>&bull;&nbsp; [단일 데이터베이스에 대 한 DTU 기반 제한](service-tiers-dtu.md)<br/>&bull;&nbsp; [탄력적 풀에 대 한 DTU 기반 제한](resource-limits-dtu-elastic-pools.md)<br/>&bull;&nbsp; [단일 데이터베이스에 대 한 vcore 기반 제한](resource-limits-vcore-single-databases.md)<br/>&bull;&nbsp; [탄력적 풀에 대 한 vcore 기반 제한](resource-limits-vcore-elastic-pools.md)<br/>&bull;&nbsp; [Azure SQL Managed Instance 리소스 제한](../managed-instance/resource-limits.md)입니다. <br/>그렇지 않은 경우 나중에 다시 시도하세요. |
| 40544 |20 |데이터베이스가 크기 할당량에 도달했습니다. 데이터를 분할 또는 삭제하거나 인덱스를 삭제하거나 가능한 해결 방법에 대한 설명서를 참조하십시오. 데이터베이스 크기를 조정 하려면 [단일 데이터베이스 리소스 크기 조정](single-database-scale.md) 및 [탄력적 풀 리소스 크기 조정](elastic-pool-scale.md)을 참조 하세요.|
| 40549 |16 |트랜잭션을 오래 실행하여 세션이 종료됩니다. 트랜잭션을 줄여 보세요. 일괄 처리에 대 한 자세한 내용은 [일괄 처리를 사용 하 여 SQL Database 응용 프로그램 성능 향상](../performance-improve-use-batching.md)을 참조 하세요.|
| 40550 |16 |잠금을 너무 많이 획득하여 세션이 종료되었습니다. 단일 트랜잭션에서 읽거나 수정하는 행 수를 줄여 보세요. 일괄 처리에 대 한 자세한 내용은 [일괄 처리를 사용 하 여 SQL Database 응용 프로그램 성능 향상](../performance-improve-use-batching.md)을 참조 하세요.|
| 40551 |16 |과도한 `TEMPDB` 사용으로 인해 세션이 종료되었습니다. 쿼리를 수정하여 임시 테이블 공간 사용량을 줄여 보세요.<br/><br/>임시 개체를 사용하는 경우 세션에서 더 이상 필요하지 않을 때 임시 개체를 삭제하여 `TEMPDB` 데이터베이스의 공간을 절약하세요. SQL Database tempdb 사용에 대 한 자세한 내용은 [SQL Database의 tempdb 데이터베이스](/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database)를 참조 하세요.|
| 40552 |16 |트랜잭션 로그 공간 사용량이 너무 많아 세션이 종료되었습니다. 단일 트랜잭션에서 수정하는 행 수를 줄여 보세요. 일괄 처리에 대 한 자세한 내용은 [일괄 처리를 사용 하 여 SQL Database 응용 프로그램 성능 향상](../performance-improve-use-batching.md)을 참조 하세요.<br/><br/>`bcp.exe` 유틸리티 또는 `System.Data.SqlClient.SqlBulkCopy` 클래스를 사용하여 대량 삽입을 수행하는 경우 `-b batchsize` 또는 `BatchSize` 옵션을 사용하여 각 트랜잭션에서 서버로 복사된 행의 수를 제한하세요. `ALTER INDEX` 문을 사용하여 인덱스를 다시 작성하는 경우 `REBUILD WITH ONLINE = ON` 옵션을 사용하여 시도하십시오. VCore 구매 모델의 트랜잭션 로그 크기에 대 한 자세한 내용은 다음을 참조 하세요. <br/>&bull;&nbsp; [단일 데이터베이스에 대 한 vcore 기반 제한](resource-limits-vcore-single-databases.md)<br/>&bull;&nbsp; [탄력적 풀에 대 한 vcore 기반 제한](resource-limits-vcore-elastic-pools.md)<br/>&bull;&nbsp; [Azure SQL Managed Instance 리소스 제한](../managed-instance/resource-limits.md)입니다.|
| 40553 |16 |메모리 사용량이 너무 많아 세션이 종료되었습니다. 쿼리를 수정하여 처리할 행 수를 줄여 보세요.<br/><br/>Transact-SQL 코드에서 `ORDER BY` 및 `GROUP BY` 작업의 수를 줄이면 쿼리의 메모리 요구 사항이 줄어듭니다. 데이터베이스 크기를 조정 하려면 [단일 데이터베이스 리소스 크기 조정](single-database-scale.md) 및 [탄력적 풀 리소스 크기 조정](elastic-pool-scale.md)을 참조 하세요.|

## <a name="elastic-pool-errors"></a>탄력적 풀 오류

다음 오류는 탄력적 풀 만들기 및 사용하기와 관련이 있습니다.

| 오류 코드 | 심각도 | 설명 | 정정 작업 |
|:--- |:--- |:--- |:--- |
| 1132 | 17 |탄력적 풀이 스토리지 용량 한도에 도달했습니다. 탄력적 풀의 스토리지 사용량은 (%d)MB를 초과할 수 없습니다. 탄력적 풀이 스토리지 용량 한도에 도달했을 때 데이터베이스에 데이터를 기록하려고 했습니다. 리소스 제한에 대 한 자세한 내용은 다음을 참조 하세요. <br/>&bull;&nbsp; [탄력적 풀에 대 한 DTU 기반 제한](resource-limits-dtu-elastic-pools.md)<br/>&bull;&nbsp; [탄력적 풀에 대 한 vcore 기반 제한](resource-limits-vcore-elastic-pools.md)입니다. <br/> |가능하다면 탄력적 풀의 DTU를 늘리거나 탄력적 풀에 스토리지를 추가하여 스토리지 용량 한도를 늘리거나, 탄력적 풀에 있는 개별 데이터베이스에서 사용하는 스토리지를 줄이거나, 탄력적 풀에서 데이터베이스를 제거하는 것을 고려하세요. 탄력적 풀 크기 조정에 대해서는 [탄력적 풀 리소스 크기 조정](elastic-pool-scale.md)을 참조 하세요.|
| 10929 | 16 |%s의 최소 보장은 %d이며, 최대 한도는 %d이고, 해당 데이터베이스의 현재 사용량은 %d입니다. 하지만 현재 서버 사용량이 너무 많아 해당 데이터베이스에 대해 %d 이상의 요청을 지원할 수 없습니다. 리소스 제한에 대 한 자세한 내용은 다음을 참조 하세요. <br/>&bull;&nbsp; [탄력적 풀에 대 한 DTU 기반 제한](resource-limits-dtu-elastic-pools.md)<br/>&bull;&nbsp; [탄력적 풀에 대 한 vcore 기반 제한](resource-limits-vcore-elastic-pools.md)입니다. <br/> 그렇지 않은 경우 나중에 다시 시도하세요. 데이터베이스당 DTU/vCore 최솟값, 데이터베이스당 DTU/vCore 최댓값. 탄력적 풀에 있는 전체 데이터베이스의 동시 작업자(요청) 수 합계가 풀 한도를 초과하려고 했습니다. |가능하다면 탄력적 풀의 DTU 또는 vCore를 늘려 작업자 한도를 늘리거나 탄력적 풀에서 데이터베이스를 제거하는 것을 고려하세요. |
| 40844 | 16 |서버 '%ls'에 있는 데이터베이스 '%ls'은(는) 탄력적 풀에 포함된 '%ls' 버전 데이터베이스이며, 연속 복사 관계를 가질 수 없습니다.  |해당 없음 |
| 40857 | 16 |서버: '%ls'에서 탄력적 풀을 찾을 수 없음, 탄력적 풀 이름: '%ls'. 지정한 탄력적 풀이 지정한 서버에 존재하지 않습니다. | 유효한 탄력적 풀 이름을 입력하세요. |
| 40858 | 16 |탄력적 풀 '%ls'이(가) 서버 '%ls'에 이미 있습니다. 지정한 탄력적 풀이 지정한 서버에 이미 있습니다. | 새 탄력적 풀 이름을 입력하세요. |
| 40859 | 16 |탄력적 풀이 서비스 계층 '%ls'을(를) 지원하지 않습니다. 지정한 서비스 계층은 탄력적 풀 프로비저닝에 대해 지원되지 않습니다. |기본 서비스 계층을 사용하려면 오류를 수정하거나 서비스 계층을 빈 상태로 두세요. |
| 40860 | 16 |탄력적 풀 '%ls' 및 서비스 목표'%ls'의 조합은 유효하지 않습니다. 리소스 종류가 'ElasticPool'로 지정된 경우에만 탄력적 풀과 서비스 계층을 함께 지정할 수 있습니다. |탄력적 풀과 서비스 계층의 올바른 조합을 지정하세요. |
| 40861 | 16 |데이터베이스 버전 '%. *ls'이(가) '%.* ls'인 탄력적 풀 서비스 계층과 다를 수 없습니다. 데이터베이스 버전이 탄력적 풀 서비스 계층과 다릅니다. |탄력적 풀 서비스 계층과 다른 데이터베이스 버전을 지정하지 마세요.  데이터베이스 버전은 지정할 필요가 없습니다. |
| 40862 | 16 |탄력적 풀 서비스 목표를 지정한 경우 탄력적 풀 이름을 지정해야 합니다. 탄력적 풀 서비스 목표가 탄력적 풀을 고유하게 식별하지 못합니다. |탄력적 풀 서비스 목표를 사용하는 경우 탄력적 풀 이름을 지정하세요. |
| 40864 | 16 |탄력적 풀의 DTU는 서비스 계층 '%.*ls'에 대해 최소 (%d) DTU 이상이어야 합니다. 탄력적 풀의 DTU를 최소 한도 아래로 설정하려고 했습니다. |탄력적 풀의 DTU를 최소 한도 이상으로 다시 설정하세요. |
| 40865 | 16 |탄력적 풀의 DTU는 서비스 계층 '%.*ls'에 대해 (%d) DTU를 초과할 수 없습니다. 탄력적 풀의 DTU를 최대 한도 위로 설정하려고 했습니다. |탄력적 풀의 DTU를 최대 한도 미만으로 다시 설정하세요. |
| 40867 | 16 |데이터베이스당 DTU 최대값은 서비스 계층 '%.*ls'에 대해 최소 (%d) 이상이어야 합니다. 데이터베이스당 DTU 최대값을 지원되는 한도 아래로 설정하려고 했습니다. | 원하는 설정을 지원하는 탄력적 풀 서비스 계층을 사용하는 것을 고려하세요. |
| 40868 | 16 |데이터베이스당 DTU 최대값은 서비스 계층 '%.*ls'에 대해 (%d)을(를) 초과할 수 없습니다. 데이터베이스당 DTU 최대값을 지원되는 한도를 초과하여 설정하려고 했습니다. | 원하는 설정을 지원하는 탄력적 풀 서비스 계층을 사용하는 것을 고려하세요. |
| 40870 | 16 |데이터베이스당 DTU 최소값은 서비스 계층 '%.*ls'에 대해 (%d)을(를) 초과할 수 없습니다. 데이터베이스당 DTU 최소값을 지원되는 한도를 초과하여 설정하려고 했습니다. | 원하는 설정을 지원하는 탄력적 풀 서비스 계층을 사용하는 것을 고려하세요. |
| 40873 | 16 |데이터베이스 수(%d) 및 데이터베이스당 DTU 최소값(%d)은 탄력적 풀의 DTU(%d)를 초과할 수 없습니다. 탄력적 풀에 있는 데이터베이스의 DTU 최소값을 탄력적 풀의 DTU를 초과하는 값으로 지정하려고 했습니다. | 탄력적 풀의 DTU를 늘리거나, 데이터베이스당 DTU 최소값을 줄이거나, 탄력적 풀에 포함된 데이터베이스 수를 줄이는 것을 고려하세요. |
| 40877 | 16 |탄력적 풀에 데이터베이스가 포함되어 있으면 삭제할 수 없습니다. 탄력적 풀에 하나 이상의 데이터베이스가 포함되어 있으므로 삭제할 수 없습니다. |탄력적 풀을 삭제하려면 탄력적 풀에서 데이터베이스를 제거하세요. |
| 40881 | 16 |탄력적 풀 '%.*ls'이(가) 데이터베이스 개수 한도에 도달했습니다.  탄력적 풀의 데이터베이스 개수 한도는 DTU가 (%d)인 탄력적 풀의 경우 (%d)을(를) 초과할 수 없습니다. 탄력적 풀의 데이터베이스 개수 한도가 도달된 상태에서 탄력적 풀에 데이터베이스를 만들거나 추가하려고 했습니다. | 가능하다면 탄력적 풀의 DTU를 늘려 데이터베이스 한도를 확대하거나 탄력적 풀에서 데이터베이스를 제거하는 것을 고려하세요. |
| 40889 | 16 |데이터베이스에 충분한 스토리지 공간을 제공하지 못하게 되므로 스토리지 풀'%.*ls'의 DTU 또는 스토리지 한도를 줄일 수 없습니다. 탄력적 풀의 스토리지 한도를 스토리지 사용량 아래로 줄이려고 했습니다. | 탄력적 풀에 있는 개별 데이터베이스의 스토리지 사용량을 줄이거나 풀에서 데이터베이스를 제거하여 DTU 또는 스토리지 한도를 줄이는 것을 고려하세요. |
| 40891 | 16 |데이터베이스당 DTU 최소값 (%d)은(는) 데이터베이스당 DTU 최대값 (%d)을(를) 초과할 수 없습니다. 데이터베이스당 DTU 최소값을 데이터베이스당 DTU 최대값보다 높게 설정하려고 했습니다. |데이터베이스당 DTU 최소값이 데이터베이스당 DTU 최대값을 초과하지 않도록 하세요. |
| TBD | 16 |탄력적 풀에 있는 개별 데이터베이스의 스토리지 크기는 '%.*ls' 서비스 계층 탄력적 풀에서 허용하는 최대 크기를 초과할 수 없습니다. 데이터베이스의 최대 크기가 탄력적 풀 서비스 계층에서 허용하는 최대 크기를 초과합니다. |데이터베이스의 최대 크기를 탄력적 풀 서비스 계층에서 허용하는 최대 크기 한도 내로 설정하세요. |

## <a name="cannot-open-database-master-requested-by-the-login-the-login-failed"></a>로그인에서 요청한 데이터베이스 "master"를 열 수 없습니다. 로그인이 실패 했습니다.

이 문제는 계정에 master 데이터베이스에 대 한 액세스 권한이 없기 때문에 발생 합니다. 그러나 기본적으로 SSMS (SQL Server Management Studio)는 master 데이터베이스에 연결을 시도 합니다.

이 문제를 해결하려면 다음 단계를 수행하세요.

1. SSMS의 로그인 화면에서 **옵션** 을 선택한 다음 **연결 속성** 을 선택 합니다.
2. **데이터베이스에 연결** 필드에서 사용자의 기본 데이터베이스 이름을 기본 로그인 데이터베이스로 입력 하 고 **연결** 을 선택 합니다.

   ![연결 속성](./media/troubleshoot-common-errors-issues/cannot-open-database-master.png)

## <a name="confirm-whether-an-error-is-caused-by-a-connectivity-issue"></a>연결 문제로 인해 오류가 발생 했는지 확인

연결 문제로 인해 오류가 발생 했는지 여부를 확인 하려면 호출을 표시 하는 프레임에 대 한 스택 추적을 검토 하 여 다음과 같은 연결을 엽니다 ( **SqlConnection** 클래스에 대 한 참조).

```
System.Data.SqlClient.SqlConnection.TryOpen(TaskCompletionSource`1 retry)
 at System.Data.SqlClient.SqlConnection.Open()
 at AzureConnectionTest.Program.Main(String[] args)
ClientConnectionId:<Client connection ID>
```

쿼리가 쿼리 문제에 의해 트리거되는 경우 다음과 유사한 호출 스택이 표시 됩니다. **SqlCommand** 클래스에 대 한 참조를 확인 합니다. 이 경우 쿼리를 [조정](/archive/blogs/sqlblog/is-my-query-running-fine-in-the-cloud)합니다.

```
  at System.Data.SqlClient.SqlCommand.ExecuteReader()
  at AzureConnectionTest.Program.Main(String[] args)
  ClientConnectionId:<Client ID>
```

미세 조정 성능에 대 한 추가 지침은 다음 리소스를 참조 하세요.

- [Azure SQL 인덱스 및 통계를 유지 관리 하는 방법](https://techcommunity.microsoft.com/t5/Azure-Database-Support-Blog/How-to-maintain-Azure-SQL-Indexes-and-Statistics/ba-p/368787)
- [Azure SQL Database에서 쿼리 성능 수동 튜닝](./performance-guidance.md)
- [동적 관리 뷰를 사용 하 여 성능 Azure SQL Database 모니터링](./monitoring-with-dmvs.md)
- [Azure SQL Database에서 쿼리 저장소 운영](/sql/relational-databases/performance/best-practice-with-the-query-store#Insight)

## <a name="steps-to-fix-common-connection-issues"></a>일반적인 연결 문제 해결 단계

1. 응용 프로그램 서버에서 TCP/IP가 클라이언트 프로토콜로 사용 되는지 확인 합니다. 자세한 내용은 [클라이언트 프로토콜 구성](/sql/database-engine/configure-windows/configure-client-protocols)을 참조 하세요. SQL 도구가 설치 되어 있지 않은 응용 프로그램 서버에서 **cliconfg.exe** (SQL Server 클라이언트 네트워크 유틸리티)를 실행 하 여 tcp/ip를 사용 하도록 설정 했는지 확인 합니다.
2. 응용 프로그램의 연결 문자열이 올바르게 구성 되었는지 확인 합니다. 예를 들어 연결 문자열이 올바른 포트 (1433) 및 정규화 된 서버 이름을 지정 하는지 확인 합니다.
[연결 정보 가져오기](./connect-query-ssms.md#get-server-connection-information)를 참조 하세요.
3. 연결 제한 시간 값을 늘립니다. 30 초 이상의 연결 제한 시간을 사용 하는 것이 좋습니다.
4. [SSMS (SQL Server Management Studio)](./connect-query-ssms.md), UDL 파일, ping 또는 텔넷을 사용 하 여 응용 프로그램 서버와 Azure SQL Database 간의 연결을 테스트 합니다. 자세한 내용은 연결 문제 해결 및 [연결 문제](./troubleshoot-common-connectivity-issues.md#diagnostics) [해결](https://support.microsoft.com/help/4009936/solving-connectivity-errors-to-sql-server) 을 참조 하세요.

   > [!NOTE]
   > 문제 해결 단계로 다른 클라이언트 컴퓨터에서 연결을 테스트할 수도 있습니다.

5. 다시 시도 논리가 준비 되어 있는지 확인 하는 것이 가장 좋습니다. 재시도 논리에 대 한 자세한 내용은 [SQL Database 일시적 오류 및 연결 오류 문제 해결](./troubleshoot-common-connectivity-issues.md)을 참조 하세요.

이러한 단계를 수행 해도 문제가 해결 되지 않으면 더 많은 데이터를 수집한 다음 지원 담당자에 게 문의 하세요. 응용 프로그램이 클라우드 서비스인 경우 로깅을 사용 하도록 설정 합니다. 이 단계에서는 실패의 UTC 타임 스탬프를 반환 합니다. 또한 SQL Database는 추적 ID를 반환 합니다. [Microsoft 고객 지원 서비스](https://azure.microsoft.com/support/options/) 는이 정보를 사용할 수 있습니다.

로깅을 사용 하도록 설정 하는 방법에 대 한 자세한 내용은 [Azure App Service에서 앱에 대 한 진단 로깅 사용](../../app-service/troubleshoot-diagnostic-logs.md)을 참조 하세요.

## <a name="next-steps"></a>다음 단계

- [Azure SQL Database 연결 아키텍처](./connectivity-architecture.md)
- [Azure SQL Database 및 Azure Synapse Analytics 네트워크 액세스 제어](./network-access-controls-overview.md)