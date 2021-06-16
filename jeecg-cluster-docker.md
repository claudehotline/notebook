```shell
docker run --name jeecg-system -e NACOS_SERVER_ADDR="192.168.1.2" -e MYSQL-HOST="192.168.1.2" -p 7001:7001 claudehotline/jeecg-system:v2.4.5
docker run --name jeecg-system2 -e NACOS_SERVER_ADDR="192.168.1.2" -e MYSQL-HOST="192.168.1.2" -p 7000:7000 claudehotline/jeecg-system:v2.4.5
docker run --name jeecg-system3 -e NACOS_SERVER_ADDR="192.168.1.2" -e MYSQL-HOST="192.168.1.2" -p 6999:6999 claudehotline/jeecg-system:v2.4.5

docker run --name jeecg-demo -e NACOS_SERVER_ADDR="192.168.1.2" -e MYSQL-HOST="192.168.1.2" -p 7002:7002 claudehotline/jeecg-demo:v2.4.5
docker run --name jeecg-demo2 -e NACOS_SERVER_ADDR="192.168.1.2" -e MYSQL-HOST="192.168.1.2" -p 7003:7003 claudehotline/jeecg-demo:v2.4.5
docker run --name jeecg-demo3 -e NACOS_SERVER_ADDR="192.168.1.2" -e MYSQL-HOST="192.168.1.2" -p 7004:7004 claudehotline/jeecg-demo:v2.4.5

docker run --name jeecg-gateway -e NACOS_SERVER_ADDR="192.168.1.2" -p 9999:9999 claudehotline/jeecg-gateway:v2.4.5
docker run --name jeecg-gateway2 -e NACOS_SERVER_ADDR="192.168.1.2" -p 9998:9999 claudehotline/jeecg-gateway:v2.4.5
docker run --name jeecg-gateway3 -e NACOS_SERVER_ADDR="192.168.1.2" -p 9997:9999 claudehotline/jeecg-gateway:v2.4.5
```

