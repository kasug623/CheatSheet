# イメージにおけるbullseye, buster, slimの意味
Debianのイメージでは、バージョンごとにコードネームがつけられていて、Dockerイメージのタグにはそのコードネームが使われている。  

|コードネーム|バージョン|読み方|
| ---- | ---- | ---- |
|bullseye|v11|ブルズアイ|
|buster|v10|バスター|
|stretch|v9|ストレッチ|
|jessie|v8|ジェシー|

[https://prograshi.com/platform/docker/docker-image-tags-difference/](https://prograshi.com/platform/docker/docker-image-tags-difference/)

# docker-composeで起動
```
sudo docker-compose up -d
```

# 滅びの呪文
一回コンテナを停止し、イメージごと消す。  
（イメージを消してもキャッシュに残ってるので再度イメージを入手する際は一瞬で終わる）
```
docker-compose down --rmi all --volumes --remove-orphans
```

# how to run several programs on "docker start"
startup.shを用意して、それを一つ実行する中で、複数プログラムを呼ぶ。
## startup.sh 
```
#!/bin/bash

for script in `ls -1 /root/start-scripts/`
do
    /root/start-scripts/$script 
done
```