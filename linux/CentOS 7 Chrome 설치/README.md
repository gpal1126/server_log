_Node.js에서 puppeteer 모듈을 사용하기 위해서 서버에 크롬이 필요하였다._  
_CentOS6 이하 버전은 Chrome 대신 Firefox를 권장하니 참고하길 바란다.(Chrome 설치 안됨...)_  
  
## CentOS 7 기준 Chrome 설치 
### - 크롬 저장소 생성  
```
$ sudo vi /etc/yum.repos.d/google-chrome.repo
```
  

### - vi에 내용 추가 : (esc -> :wq 저장)
```
[google-chrome]
name=google-chrome
baseurl=http://dl.google.com/linux/chrome/rpm/stable/$basearch
enabled=1
gpgcheck=1
gpgkey=https://dl-ssl.google.com/linux/linux_signing_key.pub
```
  
### - chrome 설치
```
$ sudo yum install google-chrome-stable
```
  