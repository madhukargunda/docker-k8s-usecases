```

In this example we are going to containerizing the two apache webservers and one haproxy container.
Haproxy container will act as the load balancer which balances the request to apache containers
using Round Robin Algorithom


Request goes to HAProxy container.
HAProxy container calls either Apache container 1 or 2.
Response is served by Apache container 1 or 2.


Step1 : Run the following command.when we run command below without -d flag, you will see HAProxy pinging Apache servers every 1 seconds. 
        This proves that our setup works fine.

    docker-compose up --build

Step2 : Verify the logs haproxy is sending the request to apache

Step 3:

docker container ls
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS                   NAMES
4f461055b68c        test-project_apache-b   "httpd-foreground"       4 seconds ago       Up 3 seconds        0.0.0.0:32800->80/tcp   apache-b
ec047e90f59d        test-project_apache-a   "httpd-foreground"       4 seconds ago       Up 3 seconds        0.0.0.0:32801->80/tcp   apache-a
e4d7f3af3d00        test-project_haproxy    "/docker-entrypoint.…"   4 seconds ago       Up 3 seconds        0.0.0.0:80->80/tcp      haproxy

docker exec -it 4f /bin/bash
root@4f461055b68c:/usr/local/apache2# pwd
/usr/local/apache2
root@4f461055b68c:/usr/local/apache2# cd /usr/local/apache2/
root@4f461055b68c:/usr/local/apache2# ls
bin  build  cgi-bin  conf  error  htdocs  icons  include  logs	modules
root@4f461055b68c:/usr/local/apache2# cd htdocs/
root@4f461055b68c:/usr/local/apache2/htdocs# ls
apache.jpeg  index.html  sample-B.gif
root@4f461055b68c:/usr/local/apache2/htdocs# ls -lrt
total 576
-rw-r--r-- 1 root root   7025 May 25 05:01 apache.jpeg
-rw-r--r-- 1 root root 577295 May 25 05:21 sample-B.gif
-rw-r--r-- 1 root root    310 May 25 05:36 index.html
root@4f461055b68c:/usr/local/apache2/htdocs# 


Step4 : 

Perform the benchmark test
You can do benchmark with command below. It will send total of 10000 requests and 30 concurrent requests at a time. 
When you run this command, you can also run htop command in one of the Apache containers to see how CPU and Memory are used

ab -n 10000 -c 30 http://localhost/


```