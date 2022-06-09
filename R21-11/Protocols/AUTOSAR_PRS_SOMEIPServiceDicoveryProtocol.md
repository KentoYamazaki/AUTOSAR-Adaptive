

### 4.1.6 Publish/Subscribe with SOME/IP and SOME/IP-SD
[PRS_SOMEIPSD_00134] 
⌈  
SOME/IP-SDは、加入者数の構成されたMulticastThreshold（MULTICAST_THRESHOLDを参照）に達した場合、
クライアントサービスユニキャスト/マルチキャストエンドポイントからサーバーサービスマルチキャストエンドポイントへの自動切り替えと、サーバーサービスマルチキャストからクライアントサービスへの自動切り替えをサポートします。
サブスクライバーの数がマルチキャストしきい値を下回った場合のユニキャスト/マルチキャストエンドポイント。
⌋  

> MulticastThresholdの範囲内ではユニキャスト、範囲を超えるとマルチキャストで通信を行う的な？  

# 5 Configuration Parameters
* MULTICAST_THRESHOLD:  
サーバーがサーバーサービスマルチキャストエンドポイントへのイベントの送信を変更するようにトリガーする、
イベントグループごとに異なるエンドポイント情報を持つサブスクライブされたクライアントの数を指定します。
このマルチキャストエンドポイントはサーバーサービスによって構成され、SubscribeEventgroupAckで提供されます。  
• If configured to 0 only the client service unicast/multicast endpoint will be used.
• If configured to 1 the first client and all further subscribed clients will be served via the server service multicast endpoint.
• If configured to n up to n-1 clients with different endpoint information will be served 
via a client service unicast/multicast endpoint provided by the client within the SubscribeEventgroup. 
As soon as the number of subscribed clients with diffeent endpoint information reaches n, 
then all subscribed clients are served via the server service multicast endpoint.
