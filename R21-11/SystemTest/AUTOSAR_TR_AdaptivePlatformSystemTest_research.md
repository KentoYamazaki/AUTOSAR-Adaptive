# System Tests of Adaptive Platform
## 4 Test configuration and test steps for Communication Management
### 4.1 Test System
#### 4.1.1 Test configurations Communication Management
|  |  |
| ---- | ---- |
|  ConfigurationID  |  STC_CM_00001  |
|  Description  |  Standard Jenkins server for Communication Management test  |
|  ECU1  |  Hardware, 192.168.100.5  |
|  ECU2  |  Hardware, 192.168.100.2  |
|  Jenkins  |  Jenkins Server, 192.168.100.10  |

|  |  |
| ---- | ---- |
|  ConfigurationID  |  STC_CM_00002  |
|  Description  |   Scenario 2 Variant 2 - Reference Deployment |
|  ECU1  |  Hardware, 192.168.100.5  |
|  ECU2  |  Hardware, 192.168.100.2  |
|  Jenkins  |  Jenkins Server, 192.168.100.10  |

Communication Managementテスト（[CMテスター]）でジョブを実行しているJenkinsサーバーは、イーサネット経由で、システムテストアプリケーション[CMApp01]（および代替構成の[CMApp04]）をホストする[ECU1]と、[ECU2]に接続されます。ECU2はシステムテストアプリケーション[CMApp02]、[CMApp03]、[CMApp04]、および[CMApp05]をホストします。  
[CMテスター]が結果を収集することになっています。  
[CMテスター]とECU上のアプリケーション間の通信は、診断メッセージの形式でDiagnostics functional clusterを介して行われる場合があります。  

![](img/Figure4.1.png)

#### 4.2.3 [STS_CM_00003] Communication for Events based on polling-based style.
|  |  |
| ---- | ---- |
|  Test Objective  |  アプリケーションがサービスの提供、サブスクライブ、受信、およびサブスクライブの停止を実行できること、および通信がイベントの1対nの通信トポロジで機能することを確認します。アプリケーションは、イベントを受信し、ポーリングベースのスタイルでそれらにアクセスできます。  |
|  ID  |   STS_CM_00003 |
|  State  |  Draft  |
|  Affeted Functional Cluster  |  Communication Management  |
|  Trace to RS Criteria  |  [RS_CM_00101], [RS_CM_00102], [RS_CM_00104], [RS_CM_00105], [RS_CM_00106], [RS_CM_00201], [RS_CM_00202], [RS_CM_00206]  |
|  Reference to Test Environment  |  STC_CM_00002    |
|  Configuration Parameters  |  - 既存の通信サービスは次のもので構成されます（サービス名は任意です）。<br> - [CMService08]: Offered by [CMApp04], requested by [CMApp05].<br> - Service [CMService08] is an attribute of Events.<br> - Reception of services from Server to Proxy is possible using pooling-based style.  |
|  Summary  |  最初に[CMテスター]が[ECU1]と[ECU2]のアプリケーションにMachine StateをDrivingに遷移する要求を出します。<br>[CMテスター] [ECU1]と[ECU2]で拡張診断セッションを要求します。<br>[CMテスター]はアプリケーション[CMApp04] [ECU2]のサービス[CMService08]の提供をトリガーし、次にアプリケーション[CMApp04] [ECU2]または[ECU1]がサービス[CMService08]の提供を開始します。<br> [CMService08]は[ECU1]の[CMApp05]にSubscribeされます。<br>アプリケーション[CMApp05] [ECU1]キューはイベントを受信しました。<n>はキューの長さです。|

> diagnostic sessionって？
> 