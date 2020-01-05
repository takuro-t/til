コンテナを起動し続ける

```
docker run -itd java:8 /bin/sh
```

```
version: '2'
services:
  java:
    image: java:8
    container_name: java
    tty: true
```
