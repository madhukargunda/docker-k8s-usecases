```
**Step-1 : Create first Service**

docker network create -d bridge wechat-service-network
docker pull madhukargunda/wechat-social-connector-tomcat:2.0.0
docker container run -itd --network wechat-service-network --name wechat-01 madhukargunda/wechat-social-connector-tomcat:2.0.0
docker container run -itd --network wechat-service-network --name wechat-02 madhukargunda/wechat-social-connector-tomcat:2.0.0

**Step-2: Create Second Service**

docker network create -d bridge hello-world-cloud-network
docker pull dockercloud/hello-world
docker container run -itd --network hello-world-cloud-network --name hello-world-01 dockercloud/hello-world
docker container run -itd --network hello-world-cloud-network --name hello-world-02 dockercloud/hello-world

**Step-3: Create HaProxy loadbalancer** 
docker network create -d bridge haproxy-network
docker pull haproxy
docker build -t myhaproxy:1.0 .
docker run -itd --network haproxy-network --name myhaproxy myhaproxy:1.0

**Step-4: Connect all service containers to haproxy network**
docker network connect haproxy-network wechat-01
docker network connect haproxy-network wechat-02
docker network connect haproxy-network hello-world-01
docker network connect haproxy-network hello-world-02

**Step-5: Create apache reverse proxy  **
docker network create -d bridge apache-web-network
docker build -t myapache-web:1.0 .
docker network connect apache-web-network myhaproxy
docker run -itd --network apache-web-network --name myapache-web myapache-web:1.0

**Step-6: Create apache web user proxy**
docker network create -d bridge apache-proxy-network
docker build -t myapache-proxy:1.0 .
docker network connect apache-proxy-network myapache-web
docker run -itd -p 80:80 --network apache-proxy-network --name myapache-proxy myapache-proxy:1.0

How test the flow :

http://<<ipaddress:port>>/helloworld
http://<<ipaddress:port>>/wechat/v1/madhukar/socialconnections/wechat
http://<<ipaddress:port>>/
http://<<ipaddress:port>>/wechat/v1/madhukar/socialconnections/wechat

PS: Refer the pdf to know the archiecture of this asssignment.

```






