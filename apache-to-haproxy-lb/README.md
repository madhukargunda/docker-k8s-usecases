```
Step 1 :

Apache already ships the load balancer , enable the load balance form the below

Enable the following so file 

LoadModule proxy_module mod_proxy.so
LoadModule proxy_http_module mod_proxy_http.so
LoadModule proxy_balancer_module mod_proxy_balancer.so
LoadModule lbmethod_byrequests_module modules/mod_lbmethod_byrequests.so
LoadModule slotmem_shm_module modules/mod_slotmem_shm.so


Step 2 :

 mod_proxy makes Apache become an (open) proxy server, and open proxy servers are dangerous both to your network and to the Internet at large

    ProxyRequests Off
    <Proxy \*>
    Order deny,allow
    Deny from all
    </Proxy>


Step3 : Add the below configuration

    Proxy balancer://wechat> 
        BalancerMember http://haproxy:80
        Order allow,deny
        Allow from all
        ProxySet lbmethod=byrequests
    </Proxy> 

    <Location /balancer-manager> 
        SetHandler balancer-manager
    </Location> 

    ProxyPass /balancer-manager !
    ProxyPass / balancer://wechat/


Step 4: 

  docker-compose -f docker-compose.yml up --build
  docker-compose -f docker-compose.yml up --build -d
  docker-compose -f docker-compose.yml down

Step 5:




```