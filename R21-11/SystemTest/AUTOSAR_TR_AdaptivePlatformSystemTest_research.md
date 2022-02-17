# System Tests of Adaptive Platform
## 4 Test configuration and test steps for Communication Management
### 4.1 Test System
#### 4.1.1 Test configurations Communication Management
|  ConfigurationID  |  STC_CM_00001  |
|  Description  |  Standard Jenkins server for Communication Management test  |
|  ECU1  |  Hardware, 192.168.100.5  |
|  ECU2  |  Hardware, 192.168.100.2  |
|  Jenkins  |  Jenkins Server, 192.168.100.10  |
| ---- | ---- |

#### 4.2.3 [STS_CM_00003] Communication for Events based on polling-based style.
* Test Objective  
アプリケーションがサービスの提供、サブスクライブ、受信、およびサブスクライブの停止を実行できること、および通信がイベントの1対nの通信トポロジで機能することを確認します。アプリケーションは、イベントを受信し、ポーリングベースのスタイルでそれらにアクセスできます。  
* ID  
STS_CM_00003  
* State  
Draft  
* Affeted Functional Cluster  
Communication Management  
* Trace to RS Criteria  
[RS_CM_00101], [RS_CM_00102], [RS_CM_00104], [RS_CM_00105],   [RS_CM_00106], [RS_CM_00201], [RS_CM_00202], [RS_CM_00206]  
* Reference to Test Environment  
STC_CM_00002  
