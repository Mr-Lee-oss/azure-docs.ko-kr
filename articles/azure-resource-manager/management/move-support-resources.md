---
title: 리소스 종류별 이동 작업 지원
description: 새 리소스 그룹이나 구독으로 이동할 수 있는 Azure 리소스 종류를 소개합니다.
ms.topic: conceptual
ms.date: 09/23/2020
ms.openlocfilehash: 675f7bb11ed98dd17e8f4ee4a0197d05128af851
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91627111"
---
# <a name="move-operation-support-for-resources"></a>리소스에 대한 이동 작업 지원

이 문서에서는 Azure 리소스 종류가 이동 작업을 지원하는지 여부에 대한 정보를 제공합니다. 또한 리소스를 이동할 때 고려해야 할 특수 조건에 대한 정보도 제공합니다.

> [!IMPORTANT]
> 대부분의 경우 자식 리소스를 부모 리소스와 독립적으로 이동할 수 없습니다. 자식 리소스의 형식에는 리소스 형식이 `<resource-provider-namespace>/<parent-resource>/<child-resource>` 있습니다. 예를 들어 `Microsoft.ServiceBus/namespaces/queues` 는의 자식 리소스입니다 `Microsoft.ServiceBus/namespaces` . 부모 리소스를 이동 하면 자식 리소스가 자동으로 함께 이동 됩니다. 이 문서에 자식 리소스가 표시 되지 않으면 부모 리소스와 함께 이동 하는 것으로 간주할 수 있습니다. 부모 리소스가 이동을 지원 하지 않는 경우 자식 리소스를 이동할 수 없습니다.

리소스 공급자 네임스페이스로 이동합니다.
> [!div class="op_single_selector"]
> - [Microsoft.AAD](#microsoftaad)
> - [microsoft.aadiam](#microsoftaadiam)
> - [Microsoft.Addons](#microsoftaddons)
> - [Microsoft.ADHybridHealthService](#microsoftadhybridhealthservice)
> - [Microsoft.Advisor](#microsoftadvisor)
> - [Microsoft.AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft.AnalysisServices](#microsoftanalysisservices)
> - [Microsoft.ApiManagement](#microsoftapimanagement)
> - [Microsoft.AppConfiguration](#microsoftappconfiguration)
> - [Microsoft.AppPlatform](#microsoftappplatform)
> - [Microsoft.AppService](#microsoftappservice)
> - [Microsoft.Attestation](#microsoftattestation)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft.Automation](#microsoftautomation)
> - [Microsoft.AVS](#microsoftavs)
> - [Microsoft.AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft.AzureData](#microsoftazuredata)
> - [Microsoft.AzureStack](#microsoftazurestack)
> - [Microsoft.AzureStackHCI](#microsoftazurestackhci)
> - [Microsoft.Batch](#microsoftbatch)
> - [Microsoft.Billing](#microsoftbilling)
> - [Microsoft.BingMaps](#microsoftbingmaps)
> - [Microsoft.BizTalkServices](#microsoftbiztalkservices)
> - [Microsoft.Blockchain](#microsoftblockchain)
> - [Microsoft.BlockchainTokens](#microsoftblockchaintokens)
> - [Microsoft.Blueprint](#microsoftblueprint)
> - [Microsoft.BotService](#microsoftbotservice)
> - [Microsoft.Cache](#microsoftcache)
> - [Microsoft.Capacity](#microsoftcapacity)
> - [Microsoft.Cdn](#microsoftcdn)
> - [Microsoft.CertificateRegistration](#microsoftcertificateregistration)
> - [Microsoft.ClassicCompute](#microsoftclassiccompute)
> - [Microsoft.ClassicInfrastructureMigrate](#microsoftclassicinfrastructuremigrate)
> - [Microsoft.ClassicNetwork](#microsoftclassicnetwork)
> - [Microsoft.ClassicStorage](#microsoftclassicstorage)
> - [Microsoft.ClassicSubscription](#microsoftclassicsubscription)
> - [Microsoft.CognitiveServices](#microsoftcognitiveservices)
> - [Microsoft.Commerce](#microsoftcommerce)
> - [Microsoft.Compute](#microsoftcompute)
> - [Microsoft.Consumption](#microsoftconsumption)
> - [Microsoft.ContainerInstance](#microsoftcontainerinstance)
> - [Microsoft.ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft.ContainerService](#microsoftcontainerservice)
> - [Microsoft.ContentModerator](#microsoftcontentmoderator)
> - [Microsoft.CortanaAnalytics](#microsoftcortanaanalytics)
> - [Microsoft.CostManagement](#microsoftcostmanagement)
> - [Microsoft.CustomerInsights](#microsoftcustomerinsights)
> - [Microsoft.CustomerLockbox](#microsoftcustomerlockbox)
> - [Microsoft.CustomProviders](#microsoftcustomproviders)
> - [Microsoft.DataBox](#microsoftdatabox)
> - [Microsoft.DataBoxEdge](#microsoftdataboxedge)
> - [Microsoft.Databricks](#microsoftdatabricks)
> - [Microsoft.DataCatalog](#microsoftdatacatalog)
> - [Microsoft.DataConnect](#microsoftdataconnect)
> - [Microsoft.DataExchange](#microsoftdataexchange)
> - [Microsoft.DataFactory](#microsoftdatafactory)
> - [Microsoft.DataLake](#microsoftdatalake)
> - [Microsoft.DataLakeAnalytics](#microsoftdatalakeanalytics)
> - [Microsoft.DataLakeStore](#microsoftdatalakestore)
> - [Microsoft.DataMigration](#microsoftdatamigration)
> - [Microsoft.DataProtection](#microsoftdataprotection)
> - [Microsoft.DataShare](#microsoftdatashare)
> - [Microsoft.DBforMariaDB](#microsoftdbformariadb)
> - [Microsoft.DBforMySQL](#microsoftdbformysql)
> - [Microsoft.DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Microsoft.DeploymentManager](#microsoftdeploymentmanager)
> - [Microsoft.DesktopVirtualization](#microsoftdesktopvirtualization)
> - [Microsoft.Devices](#microsoftdevices)
> - [Microsoft.DevOps](#microsoftdevops)
> - [Microsoft.DevSpaces](#microsoftdevspaces)
> - [Microsoft.DevTestLab](#microsoftdevtestlab)
> - [Microsoft.DigitalTwins](#microsoftdigitaltwins)
> - [Microsoft.DocumentDB](#microsoftdocumentdb)
> - [Microsoft.DomainRegistration](#microsoftdomainregistration)
> - [Microsoft.EnterpriseKnowledgeGraph](#microsoftenterpriseknowledgegraph)
> - [Microsoft.EventGrid](#microsofteventgrid)
> - [Microsoft.EventHub](#microsofteventhub)
> - [Microsoft.Experimentation](#microsoftexperimentation)
> - [Microsoft.Falcon](#microsoftfalcon)
> - [Microsoft.Features](#microsoftfeatures)
> - [Microsoft.Genomics](#microsoftgenomics)
> - [Microsoft.GuestConfiguration](#microsoftguestconfiguration)
> - [Microsoft.HanaOnAzure](#microsofthanaonazure)
> - [Microsoft.HardwareSecurityModules](#microsofthardwaresecuritymodules)
> - [Microsoft.HDInsight](#microsofthdinsight)
> - [Microsoft.HealthcareApis](#microsofthealthcareapis)
> - [Microsoft.HybridCompute](#microsofthybridcompute)
> - [Microsoft.HybridData](#microsofthybriddata)
> - [HybridNetwork](#microsofthybridnetwork)
> - [Microsoft.Hydra](#microsofthydra)
> - [Microsoft.ImportExport](#microsoftimportexport)
> - [microsoft.insights](#microsoftinsights)
> - [Microsoft.IoTCentral](#microsoftiotcentral)
> - [Microsoft.IoTSpaces](#microsoftiotspaces)
> - [Microsoft.KeyVault](#microsoftkeyvault)
> - [Microsoft.Kubernetes](#microsoftkubernetes)
> - [Microsoft.KubernetesConfiguration](#microsoftkubernetesconfiguration)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft.LabServices](#microsoftlabservices)
> - [Microsoft.LocationBasedServices](#microsoftlocationbasedservices)
> - [Microsoft.LocationServices](#microsoftlocationservices)
> - [Microsoft.Logic](#microsoftlogic)
> - [Microsoft.MachineLearning](#microsoftmachinelearning)
> - [Microsoft.MachineLearningCompute](#microsoftmachinelearningcompute)
> - [Microsoft.MachineLearningExperimentation](#microsoftmachinelearningexperimentation)
> - [Microsoft.MachineLearningModelManagement](#microsoftmachinelearningmodelmanagement)
> - [Microsoft.MachineLearningServices](#microsoftmachinelearningservices)
> - [Microsoft.Maintenance](#microsoftmaintenance)
> - [Microsoft.ManagedIdentity](#microsoftmanagedidentity)
> - [Microsoft.ManagedNetwork](#microsoftmanagednetwork)
> - [Microsoft.ManagedServices](#microsoftmanagedservices)
> - [Microsoft.Management](#microsoftmanagement)
> - [Microsoft.Maps](#microsoftmaps)
> - [Microsoft.Marketplace](#microsoftmarketplace)
> - [Microsoft.MarketplaceApps](#microsoftmarketplaceapps)
> - [Microsoft.MarketplaceOrdering](#microsoftmarketplaceordering)
> - [Microsoft.Media](#microsoftmedia)
> - [Microsoft.Microservices4Spring](#microsoftmicroservices4spring)
> - [Microsoft.Migrate](#microsoftmigrate)
> - [Microsoft.MixedReality](#microsoftmixedreality)
> - [Microsoft.NetApp](#microsoftnetapp)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft.NotificationHubs](#microsoftnotificationhubs)
> - [Microsoft.ObjectStore](#microsoftobjectstore)
> - [Microsoft.OffAzure](#microsoftoffazure)
> - [Microsoft.OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft.OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft.Peering](#microsoftpeering)
> - [Microsoft.PolicyInsights](#microsoftpolicyinsights)
> - [Microsoft.Portal](#microsoftportal)
> - [Microsoft.PowerBI](#microsoftpowerbi)
> - [Microsoft.PowerBIDedicated](#microsoftpowerbidedicated)
> - [Microsoft.ProjectBabylon](#microsoftprojectbabylon)
> - [Microsoft.ProviderHub](#microsoftproviderhub)
> - [Microsoft.Quantum](#microsoftquantum)
> - [Microsoft.RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft.RedHatOpenShift](#microsoftredhatopenshift)
> - [Microsoft.Relay](#microsoftrelay)
> - [Microsoft.ResourceGraph](#microsoftresourcegraph)
> - [Microsoft.ResourceHealth](#microsoftresourcehealth)
> - [Microsoft.Resources](#microsoftresources)
> - [Microsoft.SaaS](#microsoftsaas)
> - [Microsoft.Search](#microsoftsearch)
> - [Microsoft.Security](#microsoftsecurity)
> - [Microsoft.SecurityInsights](#microsoftsecurityinsights)
> - [Microsoft.SerialConsole](#microsoftserialconsole)
> - [Microsoft.ServerManagement](#microsoftservermanagement)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft.ServiceFabric](#microsoftservicefabric)
> - [Microsoft.ServiceFabricMesh](#microsoftservicefabricmesh)
> - [Microsoft.Services](#microsoftservices)
> - [Microsoft.SignalRService](#microsoftsignalrservice)
> - [Microsoft.SoftwarePlan](#microsoftsoftwareplan)
> - [Microsoft.Solutions](#microsoftsolutions)
> - [Microsoft.Sql](#microsoftsql)
> - [Microsoft.SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft.Storage](#microsoftstorage)
> - [Microsoft.StorageCache](#microsoftstoragecache)
> - [Microsoft.StorageSync](#microsoftstoragesync)
> - [Microsoft.StorageSyncDev](#microsoftstoragesyncdev)
> - [Microsoft.StorageSyncInt](#microsoftstoragesyncint)
> - [Microsoft.StorSimple](#microsoftstorsimple)
> - [Microsoft.StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft.StreamAnalyticsExplorer](#microsoftstreamanalyticsexplorer)
> - [Microsoft.Subscription](#microsoftsubscription)
> - [microsoft.support](#microsoftsupport)
> - [Microsoft.Synapse](#microsoftsynapse)
> - [Microsoft.TimeSeriesInsights](#microsofttimeseriesinsights)
> - [Microsoft.Token](#microsofttoken)
> - [Microsoft.VirtualMachineImages](#microsoftvirtualmachineimages)
> - [microsoft.visualstudio](#microsoftvisualstudio)
> - [Microsoft.VMware](#microsoftvmware)
> - [Microsoft.VMwareCloudSimple](#microsoftvmwarecloudsimple)
> - [Microsoft.VnfManager](#microsoftvnfmanager)
> - [Microsoft.VSOnline](#microsoftvsonline)
> - [Microsoft.Web](#microsoftweb)
> - [Microsoft.WindowsESU](#microsoftwindowsesu)
> - [Microsoft.WindowsIoT](#microsoftwindowsiot)
> - [WorkloadBuilder](#microsoftworkloadbuilder)
> - [Microsoft.WorkloadMonitor](#microsoftworkloadmonitor)

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | domainservices | 예 | 예 |

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | diagnosticsettings | 예 | 예 |
> | diagnosticsettingscategories | 예 | 예 |
> | privatelinkforazuread | 예 | 예 |
> | tenants | 예 | 예 |

## <a name="microsoftaddons"></a>Microsoft.Addons

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | supportproviders | 예 | 예 |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | aadsupportcases | 예 | 예 |
> | addsservices | 예 | 예 |
> | agents | 예 | 예 |
> | anonymousapiusers | 예 | 예 |
> | 구성 | 예 | 예 |
> | 로그 | 예 | 예 |
> | reports | 예 | 예 |
> | servicehealthmetrics | 예 | 예 |
> | services | 예 | 예 |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 구성 | 예 | 예 |
> | generaterecommendations | 예 | 예 |
> | metadata | 예 | 예 |
> | 동영상 추천 기능 | 예 | 예 |
> | suppressions | 예 | 예 |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | actionrules | 예 | 예 |
> | 경고 | 예 | 예 |
> | alertslist | 예 | 예 |
> | alertsmetadata | 예 | 예 |
> | alertssummary | 예 | 예 |
> | alertssummarylist | 예 | 예 |
> | smartdetectoralertrules | 예 | 예 |
> | smartgroups | 예 | 예 |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | servers | 예 | 예 |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!IMPORTANT]
> 소비 SKU로 설정된 API Management 서비스는 이동할 수 없습니다.

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | reportfeedback | 예 | 예 |
> | 서비스 | 예 | 예 |

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | configurationstores | 예 | 예 |
> | configurationstores / eventgridfilters | 예 | 예 |

## <a name="microsoftappplatform"></a>Microsoft.AppPlatform

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | spring | 예 | 예 |

## <a name="microsoftappservice"></a>Microsoft.AppService

> [!IMPORTANT]
> [App Service 이동 지침](./move-limitations/app-service-move-limitations.md)을 참조하세요.

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | apiapps | 예 | 예 |
> | appidentities | 예 | 예 |
> | gateways | 예 | 아니요 |

## <a name="microsoftattestation"></a>Microsoft.Attestation

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | attestationproviders | 예 | 예 |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | classicadministrators | 예 | 예 |
> | dataaliases | 예 | 예 |
> | denyassignments | 예 | 예 |
> | elevateaccess | 예 | 예 |
> | findorphanroleassignments | 예 | 예 |
> | locks | 예 | 예 |
> | 권한 | 예 | 예 |
> | policyassignments | 예 | 예 |
> | policydefinitions | 예 | 예 |
> | policysetdefinitions | 예 | 예 |
> | privatelinkassociations | 예 | 예 |
> | resourcemanagementprivatelinks | 아니요 | 예 |
> | roleassignments | 예 | 예 |
> | roleassignmentsusagemetrics | 예 | 예 |
> | roledefinitions | 예 | 예 |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!IMPORTANT]
> Runbook은 Automation 계정과 동일한 리소스 그룹에 있어야 합니다.
>
> 자세한 내용은 [Azure Automation 계정을 다른 구독으로 이동](../../automation/how-to/move-account.md?toc=/azure/azure-resource-manager/toc.json)을 참조 하세요.

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | automationaccounts | 예 | 예 |
> | automationaccounts / configurations | 예 | 예 |
> | automationaccounts / runbooks | 예 | 예 |

## <a name="microsoftavs"></a>Microsoft.AVS

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | privateclouds | 예 | 예 |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | b2cdirectories | 예 | 예 |
> | b2ctenants | 예 | 예 |

## <a name="microsoftazuredata"></a>Microsoft.AzureData

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | datacontrollers | 예 | 예 |
> | hybriddatamanagers | 예 | 예 |
> | postgresinstances | 예 | 예 |
> | sqlinstances | 예 | 예 |
> | sqlmanagedinstances | 예 | 예 |
> | sqlserverinstances | 예 | 예 |
> | sqlserverregistrations | 예 | 예 |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | cloudmanifestfiles | 예 | 예 |
> | registrations | 예 | 예 |

## <a name="microsoftazurestackhci"></a>Microsoft.AzureStackHCI

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | clusters | 예 | 예 |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | batchaccounts | 예 | 예 |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | billingaccounts | 예 | 예 |
> | billingperiods | 예 | 예 |
> | billingpermissions | 예 | 예 |
> | billingproperty | 예 | 예 |
> | billingroleassignments | 예 | 예 |
> | billingroledefinitions | 예 | 예 |
> | departments | 예 | 예 |
> | enrollmentaccounts | 예 | 예 |
> | invoices | 예 | 예 |
> | transfers | 예 | 예 |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | mapapis | 예 | 예 |

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | biztalk | 예 | 예 |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | blockchainmembers | 예 | 예 |
> | cordamembers | 예 | 예 |
> | watchers | 예 | 예 |

## <a name="microsoftblockchaintokens"></a>Microsoft.BlockchainTokens

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | tokenservices | 예 | 예 |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | blueprintassignments | 예 | 예 |
> | blueprints | 예 | 예 |

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | botservices | 예 | 예 |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!IMPORTANT]
> Azure Cache for Redis 인스턴스가 가상 네트워크를 사용하여 구성되면 이 인스턴스를 다른 구독으로 이동할 수 없습니다. [네트워킹 이동 제한](./move-limitations/networking-move-limitations.md)을 참조하세요.

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | redis | 예 | 예 |
> | redisenterprise | 예 | 예 |

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | appliedreservations | 예 | 예 |
> | calculateexchange | 예 | 예 |
> | calculateprice | 예 | 예 |
> | calculatepurchaseprice | 예 | 예 |
> | catalogs | 예 | 예 |
> | commercialreservationorders | 예 | 예 |
> | 교환 | 예 | 예 |
> | reservationorders | 예 | 예 |
> | reservations | 예 | 예 |
> | 리소스 | 예 | 예 |
> | validatereservationorder | 예 | 예 |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | cdnwebapplicationfirewallmanagedrulesets | 예 | 예 |
> | cdnwebapplicationfirewallpolicies | 예 | 예 |
> | edgenodes | 예 | 예 |
> | 프로필 | 예 | 예 |
> | profiles / endpoints | 예 | 예 |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!IMPORTANT]
> [App Service 이동 지침](./move-limitations/app-service-move-limitations.md)을 참조하세요.

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | certificateorders | 예 | 예 |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!IMPORTANT]
> [클래식 배포 이동 지침](./move-limitations/classic-model-move-limitations.md)을 참조하세요. 클래식 배포 리소스는 해당 시나리오와 관련된 작업을 통해 구독 간에 이동할 수 있습니다.

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | capabilities | 예 | 예 |
> | domainnames | 예 | 예 |
> | quotas | 예 | 예 |
> | resourcetypes | 예 | 예 |
> | validatesubscriptionmoveavailability | 예 | 예 |
> | virtualmachines | 예 | 예 |

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate

> [!IMPORTANT]
> [클래식 배포 이동 지침](./move-limitations/classic-model-move-limitations.md)을 참조하세요. 클래식 배포 리소스는 해당 시나리오와 관련된 작업을 통해 구독 간에 이동할 수 있습니다.

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | classicinfrastructureresources | 예 | 예 |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!IMPORTANT]
> [클래식 배포 이동 지침](./move-limitations/classic-model-move-limitations.md)을 참조하세요. 클래식 배포 리소스는 해당 시나리오와 관련된 작업을 통해 구독 간에 이동할 수 있습니다.

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | capabilities | 예 | 예 |
> | expressroutecrossconnections | 예 | 예 |
> | expressroutecrossconnections / peerings | 예 | 예 |
> | gatewaysupporteddevices | 예 | 예 |
> | networksecuritygroups | 예 | 예 |
> | quotas | 예 | 예 |
> | reservedips | 예 | 예 |
> | virtualnetworks | 예 | 예 |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!IMPORTANT]
> [클래식 배포 이동 지침](./move-limitations/classic-model-move-limitations.md)을 참조하세요. 클래식 배포 리소스는 해당 시나리오와 관련된 작업을 통해 구독 간에 이동할 수 있습니다.

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | disks | 예 | 예 |
> | images | 예 | 예 |
> | osimages | 예 | 예 |
> | osplatformimages | 예 | 예 |
> | publicimages | 예 | 예 |
> | quotas | 예 | 예 |
> | storageaccounts | 예 | 예 |
> | vmimages | 예 | 예 |

## <a name="microsoftclassicsubscription"></a>Microsoft.ClassicSubscription

> [!IMPORTANT]
> [클래식 배포 이동 지침](./move-limitations/classic-model-move-limitations.md)을 참조하세요. 클래식 배포 리소스는 해당 시나리오와 관련된 작업을 통해 구독 간에 이동할 수 있습니다.

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 작업 | 예 | 예 |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 계정 | 예 | 예 |

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | ratecard | 예 | 예 |
> | usageaggregates | 예 | 예 |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!IMPORTANT]
> [Virtual Machines 이동 지침](./move-limitations/virtual-machines-move-limitations.md)을 참조하세요.

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | availabilitysets | 예 | 예 |
> | diskaccesses | 예 | 예 |
> | diskencryptionsets | 예 | 예 |
> | disks | 예 | 예 |
> | galleries | 예 | 예 |
> | galleries / images | 예 | 예 |
> | galleries / images / versions | 예 | 예 |
> | hostgroups | 예 | 예 |
> | hostgroups / hosts | 예 | 예 |
> | images | 예 | 예 |
> | proximityplacementgroups | 예 | 예 |
> | restorepointcollections | 예 | 예 |
> | restorepointcollections / restorepoints | 예 | 예 |
> | sharedvmextensions | 예 | 예 |
> | sharedvmimages | 예 | 예 |
> | sharedvmimages / versions | 예 | 예 |
> | 스냅샷 | 예 | 예 |
> | sshpublickeys | 예 | 예 |
> | virtualmachines | 예 | 예 |
> | virtualmachines / extensions | 예 | 예 |
> | virtualmachinescalesets | 예 | 예 |

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | aggregatedcost | 예 | 예 |
> | balances | 예 | 예 |
> | budgets | 예 | 예 |
> | charges | 예 | 예 |
> | costtags | 예 | 예 |
> | credits | 예 | 예 |
> | events | 예 | 예 |
> | forecasts | 예 | 예 |
> | lots | 예 | 예 |
> | marketplaces | 예 | 예 |
> | pricesheets | 예 | 예 |
> | products | 예 | 예 |
> | reservationdetails | 예 | 예 |
> | reservationrecommendationdetails | 예 | 예 |
> | reservationrecommendations | 예 | 예 |
> | reservationsummaries | 예 | 예 |
> | reservationtransactions | 예 | 예 |
> | tags | 예 | 예 |
> | tenants | 예 | 예 |
> | terms | 예 | 예 |
> | usagedetails | 예 | 예 |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | containergroups | 예 | 예 |
> | serviceassociationlinks | 예 | 예 |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | registries | 예 | 예 |
> | registries / agentpools | 예 | 예 |
> | registries / buildtasks | 예 | 예 |
> | registries / replications | 예 | 예 |
> | registries / tasks | 예 | 예 |
> | registries / webhooks | 예 | 예 |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | containerservices | 예 | 예 |
> | managedclusters | 예 | 예 |
> | openshiftmanagedclusters | 예 | 예 |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 애플리케이션 | 예 | 예 |

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 계정 | 예 | 예 |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 경고 | 예 | 예 |
> | billingaccounts | 예 | 예 |
> | budgets | 예 | 예 |
> | cloudconnectors | 예 | 예 |
> | 커넥터 | 예 | 예 |
> | departments | 예 | 예 |
> | dimensions | 예 | 예 |
> | enrollmentaccounts | 예 | 예 |
> | exports | 예 | 예 |
> | externalbillingaccounts | 예 | 예 |
> | 예측 | 예 | 예 |
> | Query | 예 | 예 |
> | register | 예 | 예 |
> | reportconfigs | 예 | 예 |
> | reports | 예 | 예 |
> | 설정 | 예 | 예 |
> | showbackrules | 예 | 예 |
> | 뷰 | 예 | 예 |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | hubs | 예 | 예 |

## <a name="microsoftcustomerlockbox"></a>Microsoft.CustomerLockbox

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | requests | 예 | 예 |

## <a name="microsoftcustomproviders"></a>Microsoft.CustomProviders

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | associations | 예 | 예 |
> | resourceproviders | 예 | 예 |

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | jobs | 예 | 예 |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | availableskus | 예 | 예 |
> | databoxedgedevices | 예 | 예 |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | workspaces | 예 | 예 |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | catalogs | 예 | 예 |
> | datacatalogs | 예 | 예 |

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | connectionmanagers | 예 | 예 |

## <a name="microsoftdataexchange"></a>Microsoft.DataExchange

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 패키지 | 예 | 예 |
> | 플랜 | 예 | 예 |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | datafactories | 예 | 예 |
> | datafactoryschema | 예 | 예 |
> | factories | 예 | 예 |

## <a name="microsoftdatalake"></a>Microsoft.DataLake

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | datalakeaccounts | 예 | 예 |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 계정 | 예 | 예 |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 계정 | 예 | 예 |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | services | 예 | 예 |
> | services / projects | 예 | 예 |
> | slots | 예 | 예 |

## <a name="microsoftdataprotection"></a>Microsoft.DataProtection

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | backupvaults | 예 | 예 |

## <a name="microsoftdatashare"></a>Microsoft.DataShare

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 계정 | 예 | 예 |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | servers | 예 | 예 |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | flexibleServers | 예 | 예 |
> | servers | 예 | 예 |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | flexibleServers | 예 | 예 |
> | servergroups | 예 | 예 |
> | servers | 예 | 예 |
> | serversv2 | 예 | 예 |
> | singleservers | 예 | 예 |

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | artifactsources | 예 | 예 |
> | rollouts | 예 | 예 |
> | servicetopologies | 예 | 예 |
> | servicetopologies / services | 예 | 예 |
> | servicetopologies / services / serviceunits | 예 | 예 |
> | 단계 | 예 | 예 |

## <a name="microsoftdesktopvirtualization"></a>Microsoft.DesktopVirtualization

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | applicationgroups | 예 | 예 |
> | hostpools | 예 | 예 |
> | workspaces | 예 | 예 |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | elasticpools | 예 | 예 |
> | elasticpools / iothubtenants | 예 | 예 |
> | iothubs | 예 | 예 |
> | provisioningservices | 예 | 예 |

## <a name="microsoftdevops"></a>Microsoft.DevOps

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | pipelines | 예 | 예 |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | controllers | 예 | 예 |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | labcenters | 예 | 예 |
> | labs | 예 | 예 |
> | labs / environments | 예 | 예 |
> | labs / servicerunners | 예 | 예 |
> | labs / virtualmachines | 예 | 예 |
> | schedules | 예 | 예 |

## <a name="microsoftdigitaltwins"></a>Microsoft.DigitalTwins

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | digitaltwinsinstances | 예 | 예 |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | databaseaccountnames | 예 | 예 |
> | databaseaccounts | 예 | 예 |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | domains | 예 | 예 |
> | generatessorequest | 예 | 예 |
> | topleveldomains | 예 | 예 |
> | validatedomainregistrationinformation | 예 | 예 |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft.EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | services | 예 | 예 |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | domains | 예 | 예 |
> | eventsubscriptions | 아니요 - 독립적으로 이동할 수는 없지만, 구독된 리소스와 함께 자동으로 이동할 수 있습니다. | 아니요 - 독립적으로 이동할 수는 없지만, 구독된 리소스와 함께 자동으로 이동할 수 있습니다. |
> | extensiontopics | 예 | 예 |
> | partnernamespaces | 예 | 예 |
> | partnerregistrations | 예 | 예 |
> | partnertopics | 예 | 예 |
> | systemtopics | 예 | 예 |
> | topics | 예 | 예 |
> | topictypes | 예 | 예 |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | clusters | 예 | 예 |
> | 네임스페이스 | 예 | 예 |
> | sku | 예 | 예 |

## <a name="microsoftexperimentation"></a>Microsoft.Experimentation

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | experimentworkspaces | 예 | 예 |

## <a name="microsoftfalcon"></a>Microsoft.Falcon

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 네임스페이스 | 예 | 예 |

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | featureproviders | 예 | 예 |
> | 기능 | 예 | 예 |
> | providers | 예 | 예 |
> | subscriptionfeatureregistrations | 예 | 예 |

## <a name="microsoftgenomics"></a>Microsoft.Genomics

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 계정 | 예 | 예 |

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | automanagedaccounts | 예 | 예 |
> | automanagedvmconfigurationprofiles | 예 | 예 |
> | guestconfigurationassignments | 예 | 예 |
> | software | 예 | 예 |
> | softwareupdateprofile | 예 | 예 |
> | softwareupdates | 예 | 예 |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | hanainstances | 예 | 예 |
> | sapmonitors | 예 | 예 |

## <a name="microsofthardwaresecuritymodules"></a>Microsoft.HardwareSecurityModules

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | dedicatedhsms | 예 | 예 |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!IMPORTANT]
> 새 구독 또는 리소스 그룹에 HDInsight 클러스터를 이동할 수 있습니다. 그러나 HDInsight 클러스터에 연결된 네트워킹 리소스(예: Virtual Network, NIC 또는 부하 분산 장치)는 구독 간에 이동할 수 없습니다. 또한 클러스터에 대한 가상 머신에 연결된 NIC를 새 리소스 그룹으로 이동할 수 없습니다.
>
> HDInsight 클러스터를 새 구독으로 이동할 때 먼저 다른 리소스(예: 스토리지 계정)를 이동합니다. 그런 다음 자체적으로 HDInsight 클러스터를 이동합니다.

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | clusters | 예 | 예 |

## <a name="microsofthealthcareapis"></a>Microsoft.HealthcareApis

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | services | 예 | 예 |

## <a name="microsofthybridcompute"></a>Microsoft.HybridCompute

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | machines | 예 | 예 |
> | machines / extensions | 예 | 예 |

## <a name="microsofthybriddata"></a>Microsoft.HybridData

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | datamanagers | 예 | 예 |

## <a name="microsofthybridnetwork"></a>HybridNetwork

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | devices | 예 | 예 |
> | vnfs | 예 | 예 |

## <a name="microsofthydra"></a>Microsoft.Hydra

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | components | 예 | 예 |
> | networkscopes | 예 | 예 |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | jobs | 예 | 예 |

## <a name="microsoftinsights"></a>microsoft.insights

> [!IMPORTANT]
> 새 구독으로 이동하는 경우 [구독 할당량](azure-subscription-service-limits.md#azure-monitor-limits)을 초과하지 않아야 합니다.

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | actiongroups | 예 | 예 |
> | activitylogalerts | 예 | 예 |
> | alertrules | 예 | 예 |
> | autoscalesettings | 예 | 예 |
> | baseline | 예 | 예 |
> | components | 예 | 예 |
> | datacollectionrules | 예 | 예 |
> | diagnosticsettings | 예 | 예 |
> | diagnosticsettingscategories | 예 | 예 |
> | eventcategories | 예 | 예 |
> | eventtypes | 예 | 예 |
> | extendeddiagnosticsettings | 예 | 예 |
> | guestdiagnosticsettings | 예 | 예 |
> | listmigrationdate | 예 | 예 |
> | logdefinitions | 예 | 예 |
> | logprofiles | 예 | 예 |
> | 로그 | 예 | 예 |
> | metricalerts | 예 | 예 |
> | metricbaselines | 예 | 예 |
> | metricbatch | 예 | 예 |
> | metricdefinitions | 예 | 예 |
> | metricnamespaces | 예 | 예 |
> | 메트릭 | 예 | 예 |
> | migratealertrules | 예 | 예 |
> | migratetonewpricingmodel | 예 | 예 |
> | myworkbooks | 예 | 예 |
> | notificationgroups | 예 | 예 |
> | privatelinkscopes | 예 | 예 |
> | rollbacktolegacypricingmodel | 예 | 예 |
> | scheduledqueryrules | 예 | 예 |
> | 토폴로지 | 예 | 예 |
> | 트랜잭션 | 예 | 예 |
> | vminsightsonboardingstatuses | 예 | 예 |
> | webtests | 예 | 예 |
> | 웹 테스트/gettestresultfile | 아니요 | 예 |
> | workbooks | 예 | 예 |
> | workbooktemplates | 예 | 예 |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | apptemplates | 예 | 예 |
> | iotapps | 예 | 예 |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | graph | 예 | 예 |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!IMPORTANT]
> 디스크 암호화에 사용되는 Key Vault는 동일한 구독의 리소스 그룹으로 또는 구독 간에 이동할 수 없습니다.

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | deletedvaults | 예 | 예 |
> | hsmpools | 예 | 아니요 |
> | managedhsms | 예 | 예 |
> | vaults | 예 | 예 |

## <a name="microsoftkubernetes"></a>Microsoft.Kubernetes

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | connectedclusters | 예 | 예 |
> | registeredsubscriptions | 예 | 예 |

## <a name="microsoftkubernetesconfiguration"></a>Microsoft.KubernetesConfiguration

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | sourcecontrolconfigurations | 예 | 예 |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | clusters | 예 | 예 |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | labaccounts | 예 | 예 |
> | users | 예 | 예 |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 계정 | 예 | 예 |

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 계정 | 예 | 예 |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | hostingenvironments | 예 | 예 |
> | integrationaccounts | 예 | 예 |
> | integrationserviceenvironments | 예 | 예 |
> | integrationserviceenvironments / managedapis | 예 | 예 |
> | isolatedenvironments | 예 | 예 |
> | workflows | 예 | 예 |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | commitmentplans | 예 | 예 |
> | webservices | 예 | 예 |
> | workspaces | 예 | 예 |

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | operationalizationclusters | 예 | 예 |

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 계정 | 예 | 예 |
> | teamaccounts | 예 | 예 |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 계정 | 예 | 예 |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | workspaces | 예 | 예 |

## <a name="microsoftmaintenance"></a>Microsoft.Maintenance

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | configurationassignments | 예 | 예 |
> | maintenanceconfigurations | 예 | 예 |
> | updates | 예 | 예 |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | ID | 예 | 예 |
> | userassignedidentities | 예 | 예 |

## <a name="microsoftmanagednetwork"></a>Microsoft.ManagedNetwork

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | managednetworks | 예 | 예 |
> | managednetworks / managednetworkgroups | 예 | 예 |
> | managednetworks / managednetworkpeeringpolicies | 예 | 예 |
> | 알림 | 예 | 예 |

## <a name="microsoftmanagedservices"></a>Microsoft.ManagedServices

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | marketplaceregistrationdefinitions | 예 | 예 |
> | registrationassignments | 예 | 예 |
> | registrationdefinitions | 예 | 예 |

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | getentities | 예 | 예 |
> | managementgroups | 예 | 예 |
> | managementgroups / settings | 예 | 예 |
> | 리소스 | 예 | 예 |
> | starttenantbackfill | 예 | 예 |
> | tenantbackfillstatus | 예 | 예 |

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 계정 | 예 | 예 |
> | accounts / privateatlases | 예 | 예 |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | offers | 예 | 예 |
> | offertypes | 예 | 예 |
> | privategalleryitems | 예 | 예 |
> | privatestoreclient | 예 | 예 |
> | privatestores | 예 | 예 |
> | products | 예 | 예 |
> | publishers | 예 | 예 |
> | register | 예 | 예 |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | classicdevservices | 예 | 예 |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | agreements | 예 | 예 |
> | offertypes | 예 | 예 |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | mediaservices | 예 | 예 |
> | mediaservices / liveevents | 예 | 예 |
> | mediaservices / streamingendpoints | 예 | 예 |

## <a name="microsoftmicroservices4spring"></a>Microsoft.Microservices4Spring

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | appclusters | 예 | 예 |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | assessmentprojects | 예 | 예 |
> | migrateprojects | 예 | 예 |
> | movecollections | 예 | 예 |
> | projects | 예 | 예 |

## <a name="microsoftmixedreality"></a>Microsoft.MixedReality

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | holographicsbroadcastaccounts | 예 | 예 |
> | objectunderstandingaccounts | 예 | 예 |
> | remoterenderingaccounts | 예 | 예 |
> | spatialanchorsaccounts | 예 | 예 |

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | netappaccounts | 예 | 예 |
> | netappaccounts / capacitypools | 예 | 예 |
> | netappaccounts / capacitypools / volumes | 예 | 예 |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!IMPORTANT]
> [네트워킹 이동 지침](./move-limitations/networking-move-limitations.md)을 참조하세요.

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | applicationgateways | 예 | 예 |
> | applicationgatewaywebapplicationfirewallpolicies | 예 | 예 |
> | applicationsecuritygroups | 예 | 예 |
> | azurefirewalls | 예 | 예 |
> | bastionhosts | 예 | 예 |
> | bgpservicecommunities | 예 | 예 |
> | connections | 예 | 예 |
> | ddoscustompolicies | 예 | 예 |
> | ddosprotectionplans | 예 | 예 |
> | dnszones | 예 | 예 |
> | expressroutecircuits | 예 | 예 |
> | expressroutegateways | 예 | 예 |
> | expressrouteserviceproviders | 예 | 예 |
> | firewallpolicies | 예 | 예 |
> | frontdoors | 예 | 아니요 |
> | ipallocations | 예 | 예 |
> | ipgroups | 예 | 예 |
> | loadbalancers | 예 - 기본 SKU<br> 예-표준 SKU | 예 - 기본 SKU<br>아니요 - 표준 SKU |
> | localnetworkgateways | 예 | 예 |
> | natgateways | 예 | 예 |
> | networkexperimentprofiles | 예 | 예 |
> | networkintentpolicies | 예 | 예 |
> | networkinterfaces | 예 | 예 |
> | networkprofiles | 예 | 예 |
> | networksecuritygroups | 예 | 예 |
> | networkwatchers | 예 | 예 |
> | networkwatchers / connectionmonitors | 예 | 예 |
> | networkwatchers / flowlogs | 예 | 예 |
> | networkwatchers / pingmeshes | 예 | 예 |
> | p2svpngateways | 예 | 예 |
> | privatednszones | 예 | 예 |
> | privatednszones / virtualnetworklinks | 예 | 예 |
> | privatednszonesinternal | 예 | 예 |
> | privateendpointredirectmaps | 예 | 예 |
> | privateendpoints | 예 | 예 |
> | privatelinkservices | 예 | 예 |
> | publicipaddresses | 예 - 기본 SKU<br>예-표준 SKU | 예 - 기본 SKU<br>아니요 - 표준 SKU |
> | publicipprefixes | 예 | 예 |
> | routefilters | 예 | 예 |
> | routetables | 예 | 예 |
> | securitypartnerproviders | 예 | 예 |
> | serviceendpointpolicies | 예 | 예 |
> | trafficmanagergeographichierarchies | 예 | 예 |
> | trafficmanagerprofiles | 예 | 예 |
> | trafficmanagerprofiles / heatmaps | 예 | 예 |
> | trafficmanagerusermetricskeys | 예 | 예 |
> | virtualhubs | 예 | 예 |
> | virtualnetworkgateways | 예 | 예 |
> | virtualnetworks | 예 | 예 |
> | virtualnetworktaps | 예 | 예 |
> | virtualrouters | 예 | 예 |
> | virtualwans | 예 | 예 |
> | vpngateways(Virtual WAN) | 예 | 예 |
> | vpnserverconfigurations | 예 | 예 |
> | vpnsites(Virtual WAN) | 예 | 아니요 |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 네임스페이스 | 예 | 예 |
> | namespaces / notificationhubs | 예 | 예 |

## <a name="microsoftobjectstore"></a>Microsoft.ObjectStore

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | osnamespaces | 예 | 예 |

## <a name="microsoftoffazure"></a>Microsoft.OffAzure

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | hypervsites | 아니요 | 아니요 |
> | importsites | 아니요 | 예 |
> | serversites | 예 | 예 |
> | vmwaresites | 예 | 예 |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!IMPORTANT]
> 새 구독으로 이동하는 경우 [구독 할당량](azure-subscription-service-limits.md#azure-monitor-limits)을 초과하지 않아야 합니다.
>
> 연결된 자동화 계정이 있는 작업 영역은 이동할 수 없습니다. 이동 작업을 시작하기 전에 자동화 계정의 연결을 해제해야 합니다.

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | clusters | 예 | 예 |
> | deletedworkspaces | 예 | 예 |
> | linktargets | 예 | 예 |
> | storageinsightconfigs | 예 | 예 |
> | workspaces | 예 | 예 |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | managementassociations | 예 | 예 |
> | managementconfigurations | 예 | 예 |
> | solutions | 예 | 예 |
> | 뷰 | 예 | 예 |

## <a name="microsoftpeering"></a>Microsoft.Peering

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | legacypeerings | 예 | 예 |
> | peerasns | 예 | 예 |
> | peeringlocations | 예 | 예 |
> | peerings | 아니요 | 아니요 |
> | peeringservicecountries | 예 | 예 |
> | peeringservicelocations | 예 | 예 |
> | peeringserviceproviders | 예 | 예 |
> | peeringservices | 예 | 예 |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | policyevents | 예 | 예 |
> | policystates | 예 | 예 |
> | policytrackedresources | 예 | 예 |
> | remediations | 예 | 예 |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | consoles | 예 | 예 |
> | dashboards | 예 | 예 |
> | usersettings | 예 | 예 |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | workspacecollections | 예 | 예 |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | capacities | 예 | 예 |

## <a name="microsoftprojectbabylon"></a>Microsoft.ProjectBabylon

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 계정 | 예 | 예 |

## <a name="microsoftproviderhub"></a>Microsoft.ProviderHub

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | availableaccounts | 예 | 예 |
> | providerregistrations | 예 | 예 |
> | rollouts | 예 | 예 |

## <a name="microsoftquantum"></a>Microsoft.Quantum

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | workspaces | 예 | 예 |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!IMPORTANT]
> [Recovery Services 이동 지침](../../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json)을 참조하세요.

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | replicationeligibilityresults | 예 | 예 |
> | vaults | 예 | 예 |

## <a name="microsoftredhatopenshift"></a>Microsoft.RedHatOpenShift

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | openshiftclusters | 예 | 예 |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 네임스페이스 | 예 | 예 |

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 쿼리 | 예 | 예 |
> | resourcechangedetails | 예 | 예 |
> | resourcechanges | 예 | 예 |
> | 리소스 | 예 | 예 |
> | resourceshistory | 예 | 예 |
> | subscriptionsstatus | 예 | 예 |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | childresources | 예 | 예 |
> | emergingissues | 예 | 예 |
> | events | 예 | 예 |
> | metadata | 예 | 예 |
> | 알림 | 예 | 예 |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 배포 | 예 | 예 |
> | deploymentscripts | 예 | 예 |
> | deploymentscripts / logs | 예 | 예 |
> | 링크 | 예 | 예 |
> | providers | 예 | 예 |
> | resourcegroups | 예 | 예 |
> | 리소스 | 예 | 예 |
> | 구독 | 예 | 예 |
> | tags | 예 | 예 |
> | templatespecs | 예 | 예 |
> | templatespecs / versions | 예 | 예 |
> | tenants | 예 | 예 |

## <a name="microsoftsaas"></a>Microsoft.SaaS

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 애플리케이션 | 예 | 예 |
> | saasresources | 예 | 예 |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!IMPORTANT]
> 다른 지역에 있는 여러 Search 리소스를 한 번의 작업으로 이동할 수 없습니다. 대신 별도 작업으로 이동합니다.

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | resourcehealthmetadata | 예 | 예 |
> | searchservices | 예 | 예 |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | adaptivenetworkhardenings | 예 | 예 |
> | advancedthreatprotectionsettings | 예 | 예 |
> | 경고 | 예 | 예 |
> | allowedconnections | 예 | 예 |
> | applicationwhitelistings | 예 | 예 |
> | assessmentmetadata | 예 | 예 |
> | assessments | 예 | 예 |
> | autodismissalertsrules | 예 | 예 |
> | automations | 예 | 예 |
> | autoprovisioningsettings | 예 | 예 |
> | complianceresults | 예 | 예 |
> | compliances | 예 | 예 |
> | datacollectionagents | 예 | 예 |
> | devicesecuritygroups | 예 | 예 |
> | discoveredsecuritysolutions | 예 | 예 |
> | externalsecuritysolutions | 예 | 예 |
> | informationprotectionpolicies | 예 | 예 |
> | iotsecuritysolutions | 예 | 예 |
> | iotsecuritysolutions / analyticsmodels | 예 | 예 |
> | iotsecuritysolutions / analyticsmodels / aggregatedalerts | 예 | 예 |
> | iotsecuritysolutions / analyticsmodels / aggregatedrecommendations | 예 | 예 |
> | jitnetworkaccesspolicies | 예 | 예 |
> | 정책 | 예 | 예 |
> | pricings | 예 | 예 |
> | regulatorycompliancestandards | 예 | 예 |
> | regulatorycompliancestandards / regulatorycompliancecontrols | 예 | 예 |
> | regulatorycompliancestandards / regulatorycompliancecontrols / regulatorycomplianceassessments | 예 | 예 |
> | securitycontacts | 예 | 예 |
> | securitysolutions | 예 | 예 |
> | securitysolutionsreferencedata | 예 | 예 |
> | securitystatuses | 예 | 예 |
> | securitystatusessummaries | 예 | 예 |
> | servervulnerabilityassessments | 예 | 예 |
> | 설정 | 예 | 예 |
> | subassessments | 예 | 예 |
> | tasks | 예 | 예 |
> | topologies | 예 | 예 |
> | workspacesettings | 예 | 예 |

## <a name="microsoftsecurityinsights"></a>Microsoft.SecurityInsights

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | aggregations | 예 | 예 |
> | alertrules | 예 | 예 |
> | alertruletemplates | 예 | 예 |
> | automationrules | 예 | 예 |
> | bookmarks | 예 | 예 |
> | cases | 예 | 예 |
> | dataconnectors | 예 | 예 |
> | 엔터티 | 예 | 예 |
> | entityqueries | 예 | 예 |
> | incidents | 예 | 예 |
> | officeconsents | 예 | 예 |
> | 설정 | 예 | 예 |
> | threatintelligence | 예 | 예 |

## <a name="microsoftserialconsole"></a>Microsoft.SerialConsole

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | consoleservices | 예 | 예 |

## <a name="microsoftservermanagement"></a>Microsoft.ServerManagement

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | gateways | 예 | 예 |
> | nodes | 예 | 예 |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 네임스페이스 | 예 | 예 |
> | premiummessagingregions | 예 | 예 |
> | sku | 예 | 예 |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 애플리케이션 | 예 | 예 |
> | clusters | 예 | 예 |
> | containergroups | 예 | 예 |
> | containergroupsets | 예 | 예 |
> | edgeclusters | 예 | 예 |
> | managedclusters | 예 | 예 |
> | networks | 예 | 예 |
> | secretstores | 예 | 예 |
> | volumes | 예 | 예 |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 애플리케이션 | 예 | 예 |
> | containergroups | 예 | 예 |
> | gateways | 예 | 예 |
> | networks | 예 | 예 |
> | secrets | 예 | 예 |
> | volumes | 예 | 예 |

## <a name="microsoftservices"></a>Microsoft.Services

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | rollouts | 예 | 예 |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | signalr | 예 | 예 |

## <a name="microsoftsoftwareplan"></a>Microsoft.SoftwarePlan

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | hybridusebenefits | 예 | 예 |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | applicationdefinitions | 예 | 예 |
> | 애플리케이션 | 예 | 예 |
> | jitrequests | 예 | 예 |

## <a name="microsoftsql"></a>Microsoft.Sql

> [!IMPORTANT]
> 데이터베이스와 서버는 동일한 리소스 그룹에 있어야 합니다. SQL Server를 이동하면 모든 해당 데이터베이스도 함께 이동합니다. 이 동작은 Azure SQL Database 및 Azure Synapse Analytics 데이터베이스에 적용 됩니다.

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | instancepools | 예 | 예 |
> | 위치 | 예 | 예 |
> | managedinstances | 예 | 예 |
> | servers | 예 | 예 |
> | servers / databases | 예 | 예 |
> | servers / databases / backuplongtermretentionpolicies | 예 | 예 |
> | servers / elasticpools | 예 | 예 |
> | servers / jobaccounts | 예 | 예 |
> | servers / jobagents | 예 | 예 |
> | virtualclusters | 예 | 예 |

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | sqlvirtualmachinegroups | 예 | 예 |
> | sqlvirtualmachines | 예 | 예 |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | storageaccounts | 예 | 예 |

## <a name="microsoftstoragecache"></a>Microsoft.StorageCache

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | caches | 예 | 예 |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | storagesyncservices | 예 | 예 |

## <a name="microsoftstoragesyncdev"></a>Microsoft.StorageSyncDev

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | storagesyncservices | 예 | 예 |

## <a name="microsoftstoragesyncint"></a>Microsoft.StorageSyncInt

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | storagesyncservices | 예 | 예 |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | managers | 예 | 예 |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!IMPORTANT]
> 실행 중 상태인 Stream Analytics 작업은 이동할 수 없습니다.

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | clusters | 예 | 예 |
> | streamingjobs | 예 | 예 |

## <a name="microsoftstreamanalyticsexplorer"></a>Microsoft.StreamAnalyticsExplorer

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | environments | 예 | 예 |
> | 인스턴스 | 예 | 예 |

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 구독 | 예 | 예 |

## <a name="microsoftsupport"></a>microsoft.support

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | services | 예 | 예 |
> | supporttickets | 예 | 예 |

## <a name="microsoftsynapse"></a>Microsoft.Synapse

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | workspaces | 예 | 예 |
> | workspaces / bigdatapools | 예 | 예 |
> | workspaces / sqlpools | 예 | 예 |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | environments | 예 | 예 |
> | environments / eventsources | 예 | 예 |
> | environments / referencedatasets | 예 | 예 |

## <a name="microsofttoken"></a>Microsoft.Token

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | stores | 예 | 예 |

## <a name="microsoftvirtualmachineimages"></a>Microsoft.VirtualMachineImages

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | imagetemplates | 예 | 예 |

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio

> [!IMPORTANT]
> Azure DevOps 구독을 변경하려면 [청구에 사용되는 Azure 구독 변경](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json)을 참조하세요.

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | account | 예 | 예 |
> | account / extension | 예 | 예 |
> | account / project | 예 | 예 |

## <a name="microsoftvmware"></a>Microsoft.VMware

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | arczones | 예 | 예 |
> | resourcepools | 예 | 예 |
> | vcenters | 예 | 예 |
> | virtualmachines | 예 | 예 |
> | virtualmachinetemplates | 예 | 예 |
> | virtualnetworks | 예 | 예 |

## <a name="microsoftvmwarecloudsimple"></a>Microsoft.VMwareCloudSimple

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | dedicatedcloudnodes | 예 | 예 |
> | dedicatedcloudservices | 예 | 예 |
> | virtualmachines | 예 | 예 |

## <a name="microsoftvnfmanager"></a>Microsoft.VnfManager

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | devices | 예 | 예 |
> | vnfs | 예 | 예 |

## <a name="microsoftvsonline"></a>Microsoft.VSOnline

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 계정 | 예 | 예 |
> | 플랜 | 예 | 예 |
> | registeredsubscriptions | 예 | 예 |

## <a name="microsoftweb"></a>Microsoft.Web

> [!IMPORTANT]
> [App Service 이동 지침](./move-limitations/app-service-move-limitations.md)을 참조하세요.

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | availablestacks | 예 | 예 |
> | billingmeters | 예 | 예 |
> | certificates | 예 | 예 |
> | connectiongateways | 예 | 예 |
> | connections | 예 | 예 |
> | customapis | 예 | 예 |
> | deletedsites | 예 | 예 |
> | deploymentlocations | 예 | 예 |
> | georegions | 예 | 예 |
> | hostingenvironments | 예 | 예 |
> | kubeenvironments | 예 | 예 |
> | publishingusers | 예 | 예 |
> | 동영상 추천 기능 | 예 | 예 |
> | resourcehealthmetadata | 예 | 예 |
> | runtimes | 예 | 예 |
> | serverfarms | 예 | 예 |
> | serverfarms / eventgridfilters | 예 | 예 |
> | sites | 예 | 예 |
> | sites / premieraddons | 예 | 예 |
> | sites / slots | 예 | 예 |
> | sourcecontrols | 예 | 예 |
> | staticsites | 예 | 예 |

## <a name="microsoftwindowsesu"></a>Microsoft.WindowsESU

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | multipleactivationkeys | 예 | 예 |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | deviceservices | 예 | 예 |

## <a name="microsoftworkloadbuilder"></a>WorkloadBuilder

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | 작업 | 예 | 예 |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tableFixed"]
> | 리소스 유형 | Resource group | Subscription |
> | ------------- | ----------- | ---------- |
> | components | 예 | 예 |
> | componentssummary | 예 | 예 |
> | monitorinstances | 예 | 예 |
> | monitorinstancessummary | 예 | 예 |
> | monitors | 예 | 예 |

## <a name="third-party-services"></a>타사 서비스

타사 서비스는 현재 이동 작업을 지원하지 않습니다.

## <a name="next-steps"></a>다음 단계

리소스를 이동하는 명령은 [새 리소스 그룹 또는 구독으로 리소스 이동](move-resource-group-and-subscription.md)을 참조하세요.

쉼표로 구분된 값 파일과 동일한 데이터를 가져오려면 [move-support-resources.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/move-support-resources.csv) 파일을 다운로드합니다.
