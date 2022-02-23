### 7.10 Communication API
#### 7.10.1 Offer service
[SWS_CM_00102]{DRAFT} Uniqueness of offered service on local machine  
「
OfferService（）を呼び出すと、Communication Managementは、サービスディスカバリで利用可能な情報を使用して、ローカルマシンで提供されたサービスの一意性を確認する必要があります。実装が重複を検出した場合（つまり、同じVLAN上に同じserviceInstanceId、serviceInterfaceId、majorVersionを持つサービス（[6]の[constr_1723]による）がすでに登録されている場合、要求されたサービスの提供は開始されず、機能エラーがログに記録された後、確実に戻ります。
」
注：提供されるサービスのシステム/車両全体の一意性(see [RS_CM_00108]);  
システム/車両全体の一意性は、最善の方法でターゲットにする必要があります。つまり、リモートで提供されるサービスに関する知識が利用できる場合は、この知識を一意性チェックに使用する必要があります。  

> ServiceIDとかはユニークである必要があるって話かな。


[SWS_CM_00103] Protocol where a service is offered  
「
アプリケーションによって新しいサービスが提供される場合、Communication Managementは、このサービスが提供されるプロトコルを確認する必要があります。 
この情報は、ロールserviceInterfaceで提供されるServiceInterfaceを参照するServiceInterfaceDeploymentのクラスで構成されます。  
ServiceInterfaceDeploymentのタイプに応じて、CommunicationManagementはそれぞれのプロトコルを介してサービス提供をトリガーするものとします。  
クラスがSomeipServiceInterfaceDeploymentの場合、Some / IPネットワークバインディングは、[SWS_CM_00203]で説明されているようにOfferService呼び出しを処理する必要があります。  
クラスがDdsServiceInterfaceDeploymentの場合、DDSネットワークバインディングは、[SWS_CM_11001]で説明されているようにOfferService呼び出しを処理する必要があります。  
クラスがUserDefinedServiceInterfaceDeploymentの場合、Communication Management実装者は、適切な方法でOfferServiceメソッドを実装する責任があります。
」
> プロトコルの変化によって呼び出し方が変わるのかな？

[SWS_CM_00104]{DRAFT} StopOfferService  
「
サービスがStopOfferServiceを呼び出すと、Communication Managementは、提供されたサービスをバインドしているどのネットワークを停止するかをチェックします。  
この情報は、ロールserviceInterfaceで提供されるServiceInterfaceを参照するServiceInterfaceDeploymentのクラスで構成されます。  
クラスがSomeipServiceInterfaceDeploymentの場合、Some / IPネットワークバインディングは、[SWS_CM_00204]で説明されているようにStopOfferServiceメソッドのマッピングを処理する必要があります。  
クラスがDdsServiceInterfaceDeploymentの場合、DDSネットワークバインディングは、[SWS_CM_11005]で説明されているようにStopOfferServiceのマッピングを処理する必要があります。クラスがUserDefinedServiceInterfaceDeploymentの場合、Communication Management実装者は、適切な方法でStopOfferServiceメソッドを実装する責任があります。
」
> これもプロキシの種類によってサービスの停止方法が異なるみたいな話かな。


#### 8.1.3 API Reference
ServiceInterfaceの説明は、サービスAPIヘッダーファイルのコンテンツを生成するための入力です。  
プロキシヘッダーファイルとスケルトンヘッダーファイルには、ServiceInterface自体とその要素のイベント、メソッド、およびフィールドを表すさまざまなクラスが含まれています。

> クライアント側がプロキシ。サーバー側がスケルトン。

[SWS_CM_00002]{DRAFT} Service skeleton class  
「CMは、[SWS_CM_01006]で定義された名前空間内のサービススケルトンヘッダーファイルで<name> Skeletonという名前のC ++クラスの定義を提供するものとします。ここで、<name>は大文字のキャメルケース形式のServiceInterface.shortNameです。」  
```
1 class UpperCamelCase(<ServiceInterface.shortName>)Skeleton {
2 ...
3 };
```
> Skeletonのクラスが提供されるよーってこと。

[SWS_CM_00003]{DRAFT} Service skeleton Event class  
「ロールイベントのServiceInterfaceで定義されたVariable-DataPrototypeごとに、Variable-DataPrototypeの大文字のキャメルケース形式のshortNameを使用したC ++クラスの定義は、[SWS_CM_01009]で定義された名前空間内のサービススケルトンヘッダーファイルで提供されます。」  
> Eventのクラスが提供されるよーってこと。  
```
1 class UpperCamelCase(<VariableDataPrototype.shortName>) {
2 ...
3 };
```

[SWS_CM_00007]{DRAFT} Service skeleton Field class  
「ロールフィールドのServiceInterfaceで定義された各フィールドについて、フィールドの大文字のキャメルケース形式のshortNameを使用したC ++クラスの定義は、[SWS_CM_01031]で定義された名前空間内のサービススケルトンヘッダーファイルで提供されます。」  
```
1 class UpperCamelCase(<Field.shortName>) {
2 ...
3 };
```
> Fieldのクラスが提供されるよーってこと

[SWS_CM_00004]{DRAFT} Service proxy class  
「Communication Managementは、[SWS_CM_01007]で定義された名前空間内のサービスプロキシヘッダーファイルに<name> Proxyという名前のC ++クラスの定義を提供します。ここで、<name>は大文字のキャメルケース形式のServiceInterface.shortNameです。」  
```
1 class UpperCamelCase(<ServiceInterface.shortName>)Proxy {
2 ...
3 };
```
> Proxyのクラスが提供されるよーってこと

[SWS_CM_00005]{DRAFT} Service proxy Event class  
「ロールイベントのServiceInterfaceで定義されたVariableDataPrototypeは、VariableDataPrototypeの大文字のキャメルケース形式のshortNameを使用したC ++クラスの定義を、[SWS_CM_01009]で定義された名前空間内のサービスプロキシヘッダーファイルで提供する必要があります。」  
```
1 class UpperCamelCase(<VariableDataPrototype.shortName>) {
2 ...
3 };
```

[SWS_CM_00006]{DRAFT} Service proxy Method class  
「ロールメソッドのServiceInterfaceで定義されたClientServerOperationごとに、ClientServerOperationの大文字のキャメルケース形式のshortNameを使用したC ++クラスの定義は、[SWS_CM_01015]で定義された名前空間内のサービスプロキシヘッダーファイルで提供されます。」  
```
1 class UpperCamelCase(<ClientServerOperation.shortName> {
2 ...
3 };
```

[SWS_CM_00008]{DRAFT} Service proxy Field class  
「役割フィールドのServiceInterfaceで定義された各フィールドについて、ServiceInterfaceの大文字のキャメルケース形式のshortNameを使用したC ++クラスの定義は、[SWS_CM_01031]で定義された名前空間内のサービスプロキシヘッダーファイルで提供されます。」  
```
1 class UpperCamelCase(<Field.shortName>) {
2 ...
3 };
```

[SWS_CM_99028]{DRAFT} Types of APIs - Communication and Service Dis- covery APIs  
「There are two categories of APIs: Service Discovery API and Communication API.  

Service Discovery API : These APIs are used in service discovery process. The following APIs are Service Discovery APIs:
* OfferService()
* StopOfferService()
* RegisterGetHandler() 
* RegisterSetHandler()
* FindService()
* StartFindService()
* StopFindService()
* GetHandle()
* Subscribe()
* Unsubscribe()
* GetSubscriptionState()
* SetSubscriptionStateChangeHandler()
* UnsetSubscriptionStateChangeHandler() 
* SetReceiveHandler()
* UnsetReceiveHandler()  
Communication API : These APIs are used in communication between Client and Server. The following APIs are Communication APIs:  
* Send()
* Allocate()
* Update()
* GetNewSamples()
* GetFreeSampleCount()
* Method call operator()
* Get()
* Set()
」  

[SWS_CM_00009]{DRAFT} Re-entrancy and thread-safety - General   
「
クラスインスタンスに関係なく、通信APIの同時呼び出しが許可されます。 -つまり、同じクラスインスタンスと異なるクラスインスタンスに対して、異なるメンバー関数の同時呼び出しが許可されます。  
サービスディスカバリAPIの同時呼び出しは、異なるクラスインスタンスに対して許可され、同じクラスインスタンスに対しては許可されません。  
通信APIのみが相互にスレッドセーフである必要があります（同じクラスのインスタンスの場合）。  
Service Discovery APIは、相互に、または通信API（同じプロキシ/スケルトンインスタンスの場合）に対してスレッドセーフであってはなりません。
」  

##### 8.1.3.2 Offer service
[SWS_CM_00101] Method to offer a service  
「
Communication Managementは、アプリケーションにサービスを提供するために、ServiceSkeletonクラスの一部としてOfferServiceメソッドを提供するものとします。  
```
ara::core::Result<void> OfferService();
```
提供されたサービスにフィールドが含まれ、OfferService（）が呼び出されたときに[SWS_CM_00128]に従ってフィールド値が無効である場合、サービスは提供されず、エラーコードComErrc :: kFieldValueIsNotValidがResultタイプで返されます。  
提供されるサービスにhasSetter = trueで定義されたフィールドが含まれ、SetHandlerがまだ登録されていない場合、サービスは提供されず、エラーコードComErrc :: kSetHandlerNotSetが結果タイプで返されます。 [SWS_CM_00129]を参照してください。
」  

[SWS_CM_00010]{DRAFT} Re-entrancy and thread-safety - OfferService  
「
OfferServiceは、さまざまなServiceSkeletonクラスインスタンスに対して再入可能で、スレッドセーフである必要があります。  
同じServiceSkeletonクラスインスタンスで再入可能または同時に呼び出された場合、動作は定義されていません。
」  

> リエントラントとは、あるプログラムやルーチンなどを実行中に、再び起動して実行し始めることができる性質。

[SWS_CM_00111] Method to stop offering a service  
「
Communication Managementは、アプリケーションへのサービスの提供を停止するために、ServiceSkeletonクラスの一部としてStopOfferServiceメソッドを提供するものとします。  
```
void StopOfferService();
```
」

[SWS_CM_00011]{DRAFT} Re-entrancy and thread-safety- StopOfferService  
「
StopOfferServiceは、さまざまなServiceSkeletonクラスインスタンスに対して再入可能でスレッドセーフである必要があります。同じServiceSkeletonクラスインスタンスで再入可能または同時に呼び出された場合、動作は定義されていません。
」  
