# nginx

## proxy_set_header
プロクシーサーバに送られるリクエストヘッダの フィールドを再定義、あるいは追加

### format  
proxy_set_header *field* *value*

### field
**X-Forwarded-For**  
通常はプロキシサーバを経由してHTTPリクエストが送信された場合、  
リクエストの送信元はプロキシサーバになるが、  
HTTPヘッダの「X-Forwarded-For」に要求元のクライアントのIPアドレスを通知することができる。  

** X-Forwarded-Proto**  
クライアントがサーバーへの接続に使用したプロトコル（HTTP またはHTTPS）を識別することができる。  
通常、サーバーアクセスログには、サーバーとプロキシサーバの間で使用されたプロトコルのみが含まれ、  
クライアントとプロキシサーバの間で使用されたプロトコルに関する情報は含まれない。  
