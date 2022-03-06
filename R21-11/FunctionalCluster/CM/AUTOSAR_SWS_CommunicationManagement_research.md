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

#### 7.10.2 Service skeleton creation
[SWS_CM_10410] InstanceIdentifier check during the creation of service skeleton  
「
Communication Managementは、InstanceIdentifier引数の値をチェックする必要があります。識別子は一意である必要があります。同じインスタンス識別子が同じサービスの複数のスケルトンインスタンスの作成に使用される場合、[SWS_CORE_00003]に従って違反として処理されるものとします。
」  

[SWS_CM_10450] InstanceSpecifier check during the creation of service skeleton   
「
Communication Managementは、InstanceSpecifier引数の値をチェックする必要があります。同じサービスの複数のスケルトンインスタンスを作成するために同じインスタンス指定子を使用する指定子は一意である必要があり、[SWS_CORE_00003]に従って違反として処理されます。
」  

[SWS_CM_10451] InstanceIdentifierContainer check during the creation of service skeleton  
「
Communication Managementは、InstanceIdentifierContainer引数の値をチェックする必要があります。  
* コンテナのサイズはゼロより大きくなければなりません  
* コンテナの識別子は一意でなければなりません  
* コンテナの識別子は、同じインスタンス指定子に対応する必要があります。  

失敗したチェックがあり、同じサービスの複数のスケルトンインスタンスの作成に同じInstanceIdentifierが使用された場合、[SWS_CORE_00003]に従って違反として処理されるものとします。
」  


#### 8.1.2 API Data Types
##### 8.1.2.1 Service Identifier Data Types
この章で説明するデータ型は、ara :: com API設計から派生し、APIの一部として、特定のサービスまたはサービスインスタンスを識別するために使用されます。  
サービスは、少なくとも完全修飾名とバージョンで識別できます。特定のサービススケルトンとプロキシクラス自体がサービスタイプを表すため、serviceIdentifierはara :: com APIに表示されませんが、CommunicationManagementソフトウェアの実装にはserviceIdentifierが必要です。ここでは、最小限の情報を保証するために定義されています。  

[SWS_CM_01010]{DRAFT} Service Identifier and Service Contract Version  
「
Communication Managementは、ServiceInterface.shortNameという名前のC ++クラスを提供する必要があります。このクラスには、少なくとも完全修飾名識別子（serviceIdentifier）、サービスコントラクトメジャー（serviceContractVersionMajor）、およびマイナー（serviceContractVersionMinor）バージョン番号が含まれています。  
serviceContractVersionMajorの値は、ServiceInterfaceのmajorVersion属性から取得する必要があります。 serviceContractVersionMinorの値は、ServiceInterfaceのminorVersion属性から取得する必要があります。  
serviceIdentifierの正確なタイプは、CommunicationManagementソフトウェアプロバイダーに固有です。その具体的な実現は、実装定義です。ただし、ログを作成し、アプリケーションを使用してC ++コンテナクラスに格納および管理できるようにするには、クラスのタイプは、表17のEqualityComparable要件、表18のLessThanComparable要件、および表18のCopyAssignable要件を満たす必要があります。 [30]のセクション17.6.3.1の23。これらの要件は、演算子operator ==、operator <、operator =、およびtoString（）メソッドが提供されている場合に満たされます。  
```
class <ServiceInterface.shortName> {
 public:
  static const serviceIdentifierType serviceIdentifier;
  static std::uint32_t serviceContractVersionMajor;
  static std::uint32_t serviceContractVersionMinor;
};
class serviceIdentifierType {
  bool operator==(const serviceIdentifierType& other) const;
  bool operator<(const serviceIdentifierType& other) const;
  serviceIdentifierType& operator=(const serviceIdentifierType& other);
  ara::core::StringView ToString() const;
};
```
」  
システム内にまったく同じサービスの異なるインスタンスが存在する可能性があります。これを処理するために、InstanceIdentifierまたはInstanceSpecifierを使用して、サービスの特定のインスタンスを識別します。これらは、スケルトン側とプロキシ側の両方に定義されたAPIの必要なパラメーターです。  
* サービススケルトン側では、[SWS_CM_00130]、[SWS_CM_00152]、[SWS_CM_00153]によってインスタンスを作成するときにサービスインスタンスを識別するために必要なパラメーターを入力します。  
* サービスプロキシ側では、[SWS_CM_00122]または[SWS_CM_00123]で特定のインスタンスを検索するときに、サービスインスタンスを識別するために必要なパラメータを入力します。  


[SWS_CM_00302]{DRAFT} Instance Identifier Class  
「
Communication Managementは、クラスInstanceIdentifierを提供するものとします。インスタンス情報とサービスタイプに関する情報が含まれています。これにより、InstanceIdentifierがさまざまなインスタンスに対して一意になります。  
InstanceIdentifierの定義は、Communication Managementソフトウェアプロバイダーによって拡張できますが、少なくとも指定されたNamed Constructor Create（）、クラスコンストラクター、およびクラスメソッドの署名を保持する必要があります。 InstanceIdentifierは、表17によるEqualityComparable要件、表18によるLessThanComparable要件、および[30]のセクション17.6.3.1の表23によるCopyAssignable要件をさらに満たして、InstanceIdentifierのロギングと保存および使用するアプリケーションによるC ++コンテナクラスのInstanceIdentifiersの管理。これらの要件は、演算子operator ==、operator <、operator =、およびToString（）メソッドが提供されている場合に満たされます。  

コンストラクターに渡される、またはToString（）メソッドによって返される文字列の形式は、Communication Managementソフトウェアプロバイダーに固有であり、標準化されていません。  

Create（）に提供される文字列表現の形式が破損している場合、またはソフトウェアプロバイダーの仕様に準拠していない場合は、エラーコードComErrc :: kIn-validInstanceIdentifierStringがResultタイプで返されます。
提供された文字列の形式が破損している場合、またはソフトウェアプロバイダーの仕様に準拠していない場合、クラスコンストラクターはComExceptionをスローします。  

```
class InstanceIdentifier {
 public:
 static ara::core::Result<InstanceIdentifier> Create(StringView
 serializedFormat) noexcept;
 explicit InstanceIdentifier( ara::core::StringView serializedFormat);
 ara::core::StringView ToString() const;
 bool operator==(const InstanceIdentifier& other) const;
 bool operator<(const InstanceIdentifier& other) const;
 InstanceIdentifier& operator=(const InstanceIdentifier& other);
};
```
」  

[SWS_CM_00319]{DRAFT} Instance Identifier Container Class  
「
Communication Managementは、InstanceIdentifierContainerの定義を提供するものとします。コンテナはInstanceIdentifierのリストを保持します。割り当てられたデータタイプは、Communication Managementソフトウェアプロバイダーによって変更できますが、セクション23.2.1の表96に基づく一般的なコンテナー要件と、セクション23.2.3の表100に基づくシーケンスコンテナー要件に準拠する必要があります。 [30]。たとえば、ara :: core :: Vectorはこれらの要件を満たします。  
```
using InstanceIdentifierContainer = ara::core::Vector<InstanceIdentifier>;
```
」  
次のデータ型は、サービスコンシューマ側でのサービスの処理に使用されるため、プロキシ側で定義されたAPIの一部です。  
サービスを見つけるためにトリガーされたリクエストを識別するために、[SWS_CM_00123]のStartFindServiceメソッドは、[SWS_CM_00125]で説明されているようにStopFindServiceでこのリクエストをキャンセルするパラメーターとして使用されるFindServiceHandleを返します。  

[SWS_CM_00303]{DRAFT} Find Service Handle  
「
Communication Managementは、まさにこの名前の不透明なFindServiceHandleの定義を提供するものとします。 FindServiceHandleは、表17のEqualityComparable要件、表18のLessThanComparable要件、および[30]のセクション17.6.3.1の表23のCopyAssignable要件を満たし、アプリケーションを使用してFindServiceHandlesをC ++コンテナクラスに格納および管理できるようにする必要があります。 。これらの要件は、operator ==、operator <、およびoperator =が指定されている場合に満たされます。 FindServiceHandleの正確な定義は、Communication Managementの実装に固有です。  
たとえば、FindServiceHandleの定義は次のようになります。  
```
struct FindServiceHandle {
  internal::ServiceId serviceIdentifier;
  internal::InstanceId instanceIdentifier;
  std::uint32_t uid;
  bool operator==(const FindServiceHandle& other) const;
  bool operator<(const FindServiceHandle& other) const;
  FindServiceHandle& operator=(const FindServiceHandle& other);
... 
};
```
」  

[SWS_CM_00122]および[SWS_CM_00123]で定義されているように、APIを使用してサービスインスタンスを検索すると、ハンドルのリストを保持するハンドルコンテナが提供されます。各ハンドルは既存のサービスインスタンスを表し、ハンドルをパラメーターとしてプロキシコンストラクター[SWS_CM_00131]に渡すことで、ara :: comAPIユーザーがこのサービスインスタンスにアクセスするためのプロキシインスタンスを作成できるようになります。  

[SWS_CM_00312]{DRAFT} Handle Type Class  
「
Communication Managementは、HandleTypeの定義を提供するものとします。特定のサービスインスタンスのハンドルを入力し、ServiceProxyの作成に必要な情報を含める必要があります。 HandleTypeの定義は、Communication Managementソフトウェアプロバイダーによって拡張できますが、少なくとも指定されたクラスおよびクラスメソッドのシグネチャを保持する必要があります。  
HandleTypeは、表17によるEqualityComparable要件と、[30]のセクション17.6.3.1の表18によるLessThanComparable要件を満たさなければなりません。これらの要件は、operator ==およびoperator <の演算子が提供されている場合に満たされます。これは、[SWS_CM_00317]および[SWS_CM_00318]とともに、アプリケーションを使用してC ++コンテナクラスにHandleTypesを格納および管理できるようにします。  
HandleTypeクラスの定義は、[SWS_CM_00004]で定義されたServiceProxyクラス内に配置する必要があります。これにより、Communication Managementソフトウェアは、表現されたサービスへのバインディングに応じて、さまざまな実装のハンドルを提供できます。  
```
class HandleType {
 public:
  bool operator==(const HandleType& other) const;
  bool operator<(const HandleType& other) const;
  const ara::com::InstanceIdentifier& GetInstanceId() const;
};
```
」  

Communication Managementソフトウェアはハンドルの作成を担当し、アプリケーションはそのインスタンスのみを使用するため、コンストラクターの署名はHandleType仕様の一部ではありません。  
[SWS_CM_00317]{DRAFT} Copy semantics of handle Type Class  
「
Communication Managementは、別のインスタンスからHandleTypeインスタンスをコピーコンストラクトおよびコピーアサインする可能性を提供するものとします。  
```
HandleType(const HandleType&);
HandleType& operator=(const HandleType&);
```
」  

[SWS_CM_00318]{DRAFT} Move semantics of handle Type Class  
「
Communication Managementは、コンストラクトを移動し、HandleTypeインスタンスを別のインスタンスから移動割り当てする可能性を提供するものとします。  
```
HandleType(HandleType &&);
HandleType& operator=(HandleType &&);
```
」  

[SWS_CM_11371]{DRAFT} HandleType destructor  
「
Communication Managementは、HandleTypeのデストラクタを提供するものとします。  
```
~HandleType() noexcept;
```
」  

[SWS_CM_00304]{DRAFT} Service Handle Container  
「
Communication Managementは、ServiceHandleContainerの定義を提供するものとします。コンテナはサービスハンドルのリストを保持し、FindServiceメソッドの戻り値として使用されます。割り当てられたデータタイプは、Communication Managementソフトウェアプロバイダーによって変更できますが、セクション23.2.1の表96に準拠した一般的なコンテナ要件と、セクション23.2.3の表100に準拠したシーケンスコンテナ要件に準拠する必要があります。 [30]。たとえば、ara :: core :: Vectorはこれらの要件を満たします。  
```
template <typename T>
using ServiceHandleContainer = ara::core::Vector<T>;
```
」  
[SWS_CM_00123]で定義されているハンドラー関数を登録してサービスを継続的に検索するには、そのようなハンドラー関数を定義する必要があります。関数の実装自体は、プロキシユーザーによって提供されます。  

[SWS_CM_00383]{DRAFT} Find Service Handler  
「
Communication Managementは、サービスの可用性が変更された場合にCommunicationManagementソフトウェアによって呼び出されるハンドラー関数の関数ラッパーとしてFindServiceHandlerの定義を提供するものとします。入力パラメーターとして、一致するすべてのサービスインスタンスのハンドルを含むハンドルコンテナーと、FindServiceHandler内からStopFindService（[SWS_CM_00125]を参照）を呼び出すために使用できるFindServiceHandleを取ります。  
```
template <typename T>
using FindServiceHandler =
std::function<void(ServiceHandleContainer<T>, FindServiceHandle)>;
```
」  

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

##### 8.1.3.3 Service skeleton creation
[SWS_CM_00130] Creation of service skeleton using Instance ID  
「
Communication Managementは、2つの引数を取る特定のServiceSkeletonクラスごとにコンストラクターを提供するものとします。  
* InstanceIdentifier: システム内のまったく同じサービスの異なるインスタンスを区別するために必要な、サービスの特定のインスタンスの識別子。タイプの定義については、[SWS_CM_00302]を参照してください。
* MethodCallProcessingMode: デフォルトの引数として、これは、デフォルト値としてkEventを使用してサービスメソッド呼び出しを処理するためのサービス実装のモードです。タイプの定義については[SWS_CM_00301]を、動作の詳細については[SWS_CM_00198]を参照してください。  
```
ServiceSkeleton(
  ara::com::InstanceIdentifier instanceID,
  ara::com::MethodCallProcessingMode mode = ara::com::MethodCallProcessingMode::kEvent
);
```
」  

[SWS_CM_10435] Exception-less creation of service skeleton using Instance ID  
「
Communication Managementは、[SWS_CM_11326]に従ったNamed Constructorイディオムを使用して、特定のServiceSkeletonクラスごとに非スローコンストラクターを提供するものとします。名前付きコンストラクターはCreate（）と呼ばれ、次の2つの引数を取ります。  
* InstanceIdentifier: システム内のまったく同じサービスの異なるインスタンスを区別するために必要な、サービスの特定のインスタンスの識別子。タイプの定義については、[SWS_CM_00302]を参照してください。  
* MethodCallProcessingMode: デフォルトの引数として、これは、デフォルト値としてkEventを使用してサービスメソッド呼び出しを処理するためのサービス実装のモードです。タイプの定義については[SWS_CM_00301]を、動作の詳細については[SWS_CM_00198]を参照してください。  
```
static ara::core::Result<ServiceSkeleton> Create(
        const ara::com::InstanceIdentifier &instanceID,
        ara::com::MethodCallProcessingMode mode = ara::com::MethodCallProcessingMode::kEvent) noexcept ;
```
e2eで保護されたメソッドがサービスによって使用され、kEventのMethodCallProcessingModeがコンストラクターに渡された場合、エラーコードkWrongMethodCallProcessingModeが結果に返されます。  
Grantの実施に失敗した場合は、エラーコードComErrc :: kGrantEnforcementErrorが結果に返されます。  
」  

[SWS_CM_00152] Creation of service skeleton using Instance Spec  
「
Communication Managementは、2つの引数を取る特定のServiceSkeletonクラスごとにコンストラクターを提供するものとします。  
* InstanceSpecifier: システム内のまったく同じサービスの異なるインスタンスを区別するために必要な、サービスの特定のインスタンスの指定子。タイプの定義については、[SWS_CORE_08001]を参照してください。
* MethodCallProcessingMode: デフォルトの引数として、これは、デフォルト値としてkEventを使用してサービスメソッド呼び出しを処理するためのサービス実装のモードです。タイプの定義については[SWS_CM_00301]を、動作の詳細については[SWS_CM_00198]を参照してください。

```
ServiceSkeleton(
  ara::core::InstanceSpecifier instanceSpec,
  ara::com::MethodCallProcessingMode mode = ara::com::MethodCallProcessingMode::kEvent
);
```
」  

[SWS_CM_10436] Exception-less creation of service skeleton using Instance Spec  
「
Communication Managementは、[SWS_CM_11326]に従ったNamed Constructorイディオムを使用して、特定のServiceSkeletonクラスごとに非スローコンストラクターを提供するものとします。名前付きコンストラクターはCreate（）と呼ばれ、次の2つの引数を取ります。  
* InstanceSpecifier: システム内のまったく同じサービスの異なるインスタンスを区別するために必要な、サービスの特定のインスタンスの指定子。タイプの定義については、[SWS_CORE_08001]を参照してください。 
*  MethodCallProcessingMode: デフォルトの引数として、これは、デフォルト値としてkEventを使用してサービスメソッド呼び出しを処理するためのサービス実装のモードです。タイプの定義については[SWS_CM_00301]を、動作の詳細については[SWS_CM_00198]を参照してください。  

```
static ara::core::Result<ServiceSkeleton> Create(
        const ara::core::InstanceSpecifier &instanceSpec,
        ara::com::MethodCallProcessingMode mode = ara::com::MethodCallProcessingMode::kEvent) noexcept;
```
e2eで保護されたメソッドがサービスによって使用され、kEventのMethodCallProcessingModeがコンストラクターに渡された場合、エラーコードkWrongMethodCallProcessingModeが結果に返されます。 [SWS_CM_10467]を参照してください。  
Grantの実施に失敗した場合は、エラーコードComErrc :: kGrantEnforcementErrorが結果に返されます。 [SWS_CM_90005]を参照してください。  
」  

[SWS_CM_00153] Creation of service skeleton using Instance ID Container  
「
Communication Managementは、次の2つの引数を取る特定のServiceSkeletonクラスごとにコンストラクターを提供する必要があります。  
* InstanceIdentifierContainer: サービスのインスタンスのコンテナ。各インスタンス要素は、システム内のまったく同じサービスの異なるインスタンスを区別するために必要です。タイプの定義については、[SWS_CM_00319]を参照してください。  
* MethodCallProcessingMode: デフォルトの引数として、これは、デフォルト値としてkEventを使用してサービスメソッド呼び出しを処理するためのサービス実装のモードです。タイプの定義については[SWS_CM_00301]を、動作の詳細については[SWS_CM_00198]を参照してください。  

```
ServiceSkeleton(
  ara::com::InstanceIdentifierContainer instanceIDs,
  ara::com::MethodCallProcessingMode mode =
  ara::com::MethodCallProcessingMode::kEvent
);
```
」  

[SWS_CM_10437] Exception-less creation of service skeleton using Instance ID Container  
「
Communication Managementは、[SWS_CM_11326]に従って、Named Constructorイディオムを使用して、特定のServiceSkeletonクラスごとに非スローコンストラクターを提供するものとします。名前付きコンストラクターはCreate（）と呼ばれ、次の2つの引数を取ります。  
* InstanceIdentifierContainer: サービスのインスタンスのコンテナ。各インスタンス要素は、システム内のまったく同じサービスの異なるインスタンスを区別するために必要です。タイプの定義については、[SWS_CM_00319]を参照してください。  
* MethodCallProcessingMode: デフォルトの引数として、これは、デフォルト値としてkEventを使用してサービスメソッド呼び出しを処理するためのサービス実装のモードです。タイプの定義については[SWS_CM_00301]を、動作の詳細については[SWS_CM_00198]を参照してください。  

```
static ara::core::Result<ServiceSkeleton> Create(
        const ara::com::InstanceIdentifierContainer &instanceIDs,
        ara::com::MethodCallProcessingMode mode = ara::com::MethodCallProcessingMode::kEvent) noexcept;
```
e2eで保護されたメソッドがサービスによって使用され、kEventのMethodCallProcessingModeがコンストラクターに渡された場合、エラーコードkWrongMethodCallProcessingModeが結果に返されます。 [SWS_CM_10467]を参照してください。  
Grantの実施に失敗した場合は、エラーコードComErrc :: kGrantEnforcementErrorが結果に返されます。 [SWS_CM_90005]を参照してください。  
」  

[SWS_CM_00134] Copy semantics of service skeleton class  
「
Communication Managementは、特定のServiceSkeletonクラスごとにコピーコンストラクターとコピー割り当て演算子の生成を無効にする必要があります。  

```
ServiceSkeleton(const ServiceSkeleton&) = delete;
ServiceSkeleton& operator=(const ServiceSkeleton&) = delete;
```
」  

[SWS_CM_00135] Move semantics of service skeleton class  
「
Communication Managementは、構成を移動し、別のインスタンスからServiceSkeletonインスタンスをmove割り当てする可能性を提供するものとします。
```
ServiceSkeleton(ServiceSkeleton &&);
ServiceSkeleton& operator=(ServiceSkeleton &&);
```
」 

[SWS_CM_11370]{DRAFT} ServiceSkeleton destructor  
「
Communication Managementは、ServiceSkeletonのデストラクタを提供するものとします。  
```
~ServiceSkeleton();
```
」  