        location ~* (/global/geoLocation)$
        {
                proxy_http_version 1.1;
                proxy_pass http://test-internal-general.xiaozhu.com;
                #proxy_pass http://127.0.0.1:8000;

        }

        location ~* (/Fangdong/index)$
        {
                proxy_http_version 1.1;
                proxy_pass http://test-internal-user.xiaozhu.com;
                #proxy_pass http://127.0.0.1:8080;
        }
        location ~ ^\/app\/xzf\w\/
        {
                if ($request_uri ~* ^(/Fangdong/index|/global/geoLocation))
                {
                        rewrite ^/app/(\w+)/(\w+)/([\d\w\.]+)/([\d\w\.]+)/([\d\w\.]+)$ /$4/$5?role=$1&client=$2&clientVersion=$3&$query_string last;
                }
        }



- regex_match: '(?i)^/app/(xzf\w)/([\w\d]+)/([\d.]+)/Fangdong/index'
  target: '/fangdong/index?role=1&clientFrom=2&clientVersion=3'

- regex_match: '(?i)^/Fangdong/index'
  target: /fangdong/index


---------------------------------------------


- regex_match: '(?i)^/app/(xzf\w)/([\w\d]+)/([\d.]+)/global/(geoLocation)'
  target: '/geo/geolocation?role=1&clientFrom=2&clientVersion=3'

- regex_match: '(?i)^/global/(geoLocation)'
  target: '/geo/geolocation?role=1&clientFrom=2&clientVersion=3'

-----------------------------------------------
- regex_match: '(?i)^/app/(xzf\w)/([\w\d]+)/([\d.]+)/global/(geoLocation)'
  target: '/geo/4|lower?role=1&clientFrom=2&clientVersion=3'

- regex_match: '(?i)^/global/(geoLocation)'
  target: '/geo/1|lower?role=1&clientFrom=2&clientVersion=3'
  




问题：

lxf.com/app/xzfk/android/6.35.00/global/geoLocations?type=nation&id=0&subItem=1
lxf.com/app/xzfk/android/6.35.00/global/geoLocation?type=nation&id=0&subItem=1

都会匹配上 /global/geoLocation，最终调用了 /global/geolocation 接口



如果 转发 /global/geoLocation， 不转发 /global/geoLocations

精确匹配：

        location ~* (/global/geoLocation)$
        {
                proxy_pass http://127.0.0.1:8000;
        }

        location ~* (/Fangdong/index)$
        {
                proxy_http_version 1.1;
                proxy_pass http://127.0.0.1:8080;
        }








如果 都转发 /global/geoLocation 和 /global/geoLocations


        location ~* ((/global/geoLocations)|(/global/geoLocation)$)
        {
                proxy_pass http://127.0.0.1:8000;
        }

        location ~* (/Fangdong/index)$
        {
                proxy_http_version 1.1;
                proxy_pass http://127.0.0.1:8080;
        }


go中配置 rewrite 注意顺序： url较长的放上面

- regex_match: '(?i)^/app/(xzf\w)/([\w\d]+)/([\d.]+)/global/(geoLocations)'
  target: '/geo/4|lower?role=1&clientFrom=2&clientVersion=3'

- regex_match: '(?i)^/app/(xzf\w)/([\w\d]+)/([\d.]+)/global/(geoLocation)'
  target: '/geo/4|lower?role=1&clientFrom=2&clientVersion=3'





如果go接口有问题了， 需要恢复到php:

    1. 恢复 nginx配置，重新找运维上线
    
    2. 如果有可供转发走的域名，可以在go的 rewrite 中转走

    - regex_match: '(?i)^/app/(xzf\w)/([\w\d]+)/([\d.]+)/global/(geoLocation)'
      target: '0role=1&clientFrom=2&clientVersion=3'
      target_host: 'http://test-service-uicproxy.xiaozhu.com'





以前的转发配置：

    center:

    usersession:
    servers:
    - 'tcp://:8e0d0f49b153bb04@redis-user-single-n0.idc.xiaozhu.com:6379?maxIdle=500&maxActive=1000&idleTimeout=86400'
    
    -
    regex_match: '(?i)/(user)/(checkUserSession)'
    target: /1/2|lower
      -
    regex_match: '(?i)/(internal)/(member)/(getMemberInfo)'
    target:  /2/3|lower
    
    
    ----------------------------------------------
    
    
    user:
    
    -
    regex_match: '(?i)^/(user)/(getByIds)'
    target: /1/getbyidbatch
    -
    regex_match: '(?i)^/(user/getById)'
    target: /1|lower






    ====================================================   如果有问题： ====================================================



    user:
    
    -
    regex_match: '(?i)^/(user/getByIds)'
    target: 0
    target_host: 'http://service-uicproxy.xiaozhu.com'
    -
    regex_match: '(?i)^/(user/getById)'
    target: 0
    target_host: 'http://service-uicproxy.xiaozhu.com'
    
    
    
    
    
    center:
    
    -
        regex_match: '(?i)/(user)/(checkUserSession)'
        target: 0
        target_host: 'http://service-uicproxy.xiaozhu.com'
    
    -
        regex_match: '(?i)/(internal)/(member)/(getMemberInfo)'
        target: 0
        response_type: raw
        target_host: 'http://service-uicproxy.xiaozhu.com'



