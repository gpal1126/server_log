# 가상호스트(Virtual Host)란?
- 기본적으로 존재하는 메인 호스트를 제외한 나머지 호스트들  
  
  
## 이름기반의 가상호스트(Name-based virtual host)
- 하나의 IP 주소에 여러개의 가상호스트를 운용하는 것  
ex) host    x.x.x.x   
    domain    www.example.com  
    domain    www.example.kr  
    domain    www.example.net  
  
  
## 주소기반의 가상호스트(Ip-based virtual host)
- 가상 호스트 각각에 하나씩의 IP 주소를 할당하여 운용하는 것  
ex) host    x.x.x.x1  
    domain    www.example.com  
    host    x.x.x.x2  
    domain    www.example.kr  
    host    x.x.x.x3  
    domain    www.example.net  
  
  
## 포트기반의 가상호스트(Port-based virtual host)
- 하나의 호스트에 포트만 다르게 지정하여 운용하는 것  
ex) main-host    x.x.x.x:80  
    host       x.x.x.x:8001  
    domain       www.example.com  
    host       x.x.x.x:8002  
    domain       www.example.kr  
    host       x.x.x.x:8003  
    domain       www.example.net  
  
   
## 기본 가상호스트(Default virtual host)
- 해당사항이 없는 호스트의 로딩요구를 받았을 때 기본으로 응답하게 될 호스트를 지정  