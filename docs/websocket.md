# Websocketについて

1つのtcpコネクション上で、任意のタイミングで上りと下りの双方向通信が可能なプロトコル。
Web上でtcpソケット通信を可能としたもので、HTTPの制約事項を解決する。

## HTTPの制約事項
HTTPは「要求した情報を取得する」ことに主眼を置いたプロトコルであるため、以下の制約事項がある。
- Request & Response型のプロトコルのため、クライアントから要求されない限り、サーバからは応答がえられない
- Request & Responseのたびに、送受信ともに大量のHTTPヘッダが付与される。  
このオーバーヘッド(数百バイト)のために、通信が頻繁となるケースでは特に大量のネットワークリソースとサーバリソースを消費する。
- 1つのコネクションでは、サーバから応答が返ってこないと次の要求を送信できない

## サーバリソースの考え
### 従来のWeb
現状のWebのようにRequest & Responseに対応したWebサーバ(Apacheなど)では、クライアントの要求に対し、  
要求ごとにそれぞれ個別のプロセスもしくはスレッドを生成される。  
生成されたプロセス/スレッドの中で通信相手とのデータの送受信が完了するまで、処理をブロックする同期型のソケット通信  
を使ってデータの送受信を行う。  
クライアントからのリクエストに対するレスポンスを返すと、数秒後にはtcpコネクションが終了し、個別に生成された  
プロセス/スレッドが解放されるため、短時間で生成と解放が繰り返される。  
また、アクセス数が1万程度のシステムであっても、サーバへの同時コネクション数ははるかに少ない値となる。

### Websocketでは
従来のWebとは異なり、生成したコネクションは切断処理をするまでずっと使用し続けるため、1万の接続がある場合、  
サーバでは同時に1万のコネクションを処理する必要がある。
Linuxでは、1つのプロセス/スレッドあたりデフォルトで約2Mバイトのメモリーが使用されるため、おそよ20Gバイトのメモリーが必要となる。  
したがって、Websocket利用時には、1つのプロセス/スレッドで複数のコネクションを処理する必要がある。  

## Websocketサーバ
Blocking I/O(同期I/O)を持つサーバープロセスは一度に1つのクライアントとしかコネクションを張れないため、  
WebSocketを利用する場合には、通常Non Blocking I/Oを持つサーバーが使われる。  
有名どころはnode.jsだが、reverse proxyとしてよく利用されるnginxもWebSocketに対応した。  
また、WebSocketに対応するためのライブラリとしては、socket.ioやwebsocket-railsがある。
