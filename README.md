# 파이웹 심포지움 2024
## 주제 : 생산성을 높여주는 CI/CD With Django, GitHub Actions
- 날짜 2024-03-23

# 1. 역할

## 1. **Nginx**

Nginx는 고**성능의 웹 서버이자 리버스 프록시 서버입니다.**

- **정적 파일 서빙**: Nginx는 정적 파일(이미지, CSS, JavaScript 등)을 효율적으로 제공할 수 있습니다.
- **리버스 프록시**: 클라이언트의 요청을 받아 실제 장고 애플리케이션 서버로 전달합니다. 이 과정에서 **로드 밸런싱**, SSL 종료, 캐싱 등의 기능을 수행할 수 있습니다. (이정표)
- **보안 및 안정성**: NGINX는 클라이언트와 장고 애플리케이션 서버 간의 중간자 역할을 하여, 보안을 강화하고 **부하 분산**을 통해 서버의 안정성을 높입니다.

## 2. **WSGI**

WSGI(Web Server Gateway Interface)는 파이썬 웹 애플리케이션과 웹 서버 간의 표준 인터페이스(정의서)입니다. 장고는 WSGI 호환 웹 애플리케이션 프레임워크로, WSGI를 통해 다음과 같은 역할을 수행할 수 있습니다.

- **웹 서버와의 인터페이스**: WSGI는 웹 서버(예: NGINX)와 장고 애플리케이션 간의 통신 규약을 정의합니다. 이를 통해 서로 다른 웹 서버와 장고 애플리케이션이 호환되도록 합니다.
- **요청 및 응답 처리**: WSGI는 웹 서버로부터의 요청을 받아 장고 애플리케이션으로 전달하고, 애플리케이션의 응답을 다시 웹 서버로 반환합니다.

<aside>
💡 잠깐! WSGI와 ASGI는 무슨 차이가 있을까요?

</aside>

- **WSGI(Web Server Gateway Interface)**
    - 동기식 → 한번의 하나의 요청을 처리합니다. 즉 요청과 응답이 동시에 이루어집니다.
    - :WSGI는 웹 애플리케이션과 서버 간의 중간계층(middleware)을 지원합니다. 이를 통해 요청과 응답을 조작하거나 확장하는 기능을 추가할 수 있습니다.
    - 언제 사용해야 할까요?
        - 동기적인 필요한 웹 애플리케이션에 적합합니다.
        - 복잡한 비동기 로직이 필요하지 않은 전통적인 웹 애플리케이션에서 사용하기 좋습니다.
        - 높은 동시성 처리가 요구되지 않는 경우 유리합니다.
- **ASGI(Asynchronous Server Gateway Interface)**
    - 비동기식 → 한 번에 여러 요청을 비동기적으로 처리할 수 있습니다. 이렇게 되면 효율적인 자원 사용과 높은 동시성을 가능하게 합니다.
    - 웹소켓과 같은 양방향 통신 프로토콜과 HTTP/2를 지원합니다. 이는 실시간 통신이 필요한 애플리케이션에 적합합니다.
    - ASGI는 동기 및 비동기 Python 애플리케이션 모두를 지원합니다.
    - 언제 사용해야 할까요?
        - 실시간 데이터 처리나 웹소켓을 사용하는 애플리케이션에 적합합니다.
        - 높은 동시성과 비동기 처리가 필요한 경우 유리합니다.
        - HTTP/2, 웹소켓 등의 최신 프로토콜을 사용해야 할 때 적합합니다.
        

**핵심은 동적페이지 요청(Django)이 들어왔을 때 대부분의 웹 서버는 파이썬 프로그램을 호출할 수 있는 기능이 없기에 파이썬 프로그램을 호출하는 WSGI 서버를 중간에 두어서 대신 처리하도록 하는 것입니다.**

## 3. **Gunicorn**

Gunicorn은 WSGI 호환 웹 서버입니다. 장고 애플리케이션을 실행하기 위해 주로 사용됩니다.

- **애플리케이션 서버**: Gunicorn은 장고 애플리케이션을 서빙하는 애플리케이션 서버로 작동합니다. 이는 WSGI 인터페이스를 구현하여 장고 애플리케이션과 통신합니다.
- **동시성 관리**: Gunicorn은 동시에 여러 요청을 처리할 수 있는 워커(Worker)를 생성하여, 장고 애플리케이션의 동시 요청 처리 능력을 향상시킵니다.
- **부하 분산**: Gunicorn을 사용하면 여러 워커를 통해 부하를 분산시킬 수 있으며, 이를 통해 애플리케이션의 성능과 안정성을 높일 수 있습니다.

## 4. 결론

위 아키텍처(흐름)로 구성하게 되면 다음과 같은 장점을 가질 수 있습니다.

- **성능 최적화**: NGINX를 통한 정적 파일 서빙과 Gunicorn의 효율적인 동시성 처리로 전체적인 성능을 향상시킵니다.
- **보안 강화**: NGINX를 리버스 프록시로 사용하여 보안을 강화하고,  수행할 수 있습니다.
- **확장성 및 안정성**: 부하 분산과 동시성 관리를 통해 대규모 트래픽에도 안정적으로 서비스를 제공할 수 있습니다.
- **핵심은 동적페이지 요청(Django)이 들어왔을 때 대부분의 웹 서버는 파이썬 프로그램을 호출할 수 있는 기능이 없기에 파이썬 프로그램(Django)을 호출하는 WSGI 서버를 중간에 두어서 대신 처리하도록 하는 것입니다.**
  
![아키텍처 구상도](https://prod-files-secure.s3.us-west-2.amazonaws.com/579fe283-28aa-489d-ae65-d683304becfc/7c7b54f0-c4ac-4775-8640-252c84a0dacd/Untitled.png)

아키텍처 구상도

아키텍처 구상도

# 2. 구축 과정

## 1. 환경 셋팅 (DRF)

프로젝트는 아래의 튜토리얼을 기반으로 진행합니다.

[Quickstart - Django REST framework](https://www.django-rest-framework.org/tutorial/quickstart/)

[GitHub - weniv/drf_ci-cd: DRF 상용 배포 가이드 + CI/CD 예제 프로젝트입니다.](https://github.com/weniv/drf_ci-cd)

`settings.py`

### 1. **WSGI 설정**

```python
...
WSGI_APPLICATION = "{프로젝트이름}.wsgi.application"
DEBUG = False # 배포환경에서는 DEBUG 옵션을 해제해야합니다.
```
```

- **DEBUG모드를 True가 아닌 False로 설정해주는 이유**
    - `True`일 경우 디버깅 정보를 표시합니다. 이 정보에는 시스템 정보, 환경변수, 코드 스택 트레이스(함수 호출 스택의 상태)가 있어 보안에 취약합니다.
    - 추가적인 메모리와 처리 과정을 요구합니다.
    - 오류가 발생하면 화면에 직접 표시됩니다.

### 2. **database 설정 (서버 기준, postgresql)**

```python
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.postgresql",
        "NAME": DB_NAME, # env파일
        "USER": DB_USER,  # env파일
        "PASSWORD": DB_PASSWORD,  # env파일
        "HOST": DB_HOST,  # env파일
        "PORT": DB_PORT,  # env파일
    }
}
### postgresql을 사용하려면 psycopg2 설치가 수행되어야합니다. 
```

---

## 2. 서버 셋팅 (Ubuntu)

```bash
cd /home/{사용자명} # app 디렉터리로 이동 
git pull {장고 레포지토리 주소}
# 깃허브 아이디 , 비밀번호(깃허브 액세스 토큰) 입력
git config --global credential.helper store # 깃 정보 전역 설정
git branch --set-upstream-to=origin/{브랜치이름} {브랜치이름} 
# 브랜치 이름은 동일하게 가져가시는 것을 추천드립니다.

# 가상환경 적용
source {가상환경이름}/bin/activate
# 안된다면 아래와 같은 모듈 설치
sudo apt install python3-pip
sudo apt install python3.10-venv # 3.10버전일 경우 우분투 기준현재 기본3.10
python3 -m venv venv # venv라는 가상환경 생성

# 설치
pip install -r requirements.txt

```

---

## 3.  Gunicorn 셋팅 및 실행

1. 일반적인 수행방법

```python
pip install gunicorn # 구니콘 설치

cd /home/{사용자명}/{프로젝트 루트 디렉터리}

gunicorn --bind 0:5000 {프로젝트이름}.wsgi:application
"""
5000번 포트로 WSGI 서버(gunicorn)와 연결된 WSGI 애플리케이션(django)를 실행시킨다.
# nginx

[2023-11-28 10:56:09 +0900] [52669] [INFO] Starting gunicorn 21.2.0
[2023-11-28 10:56:09 +0900] [52669] [INFO] Listening at: http://0.0.0.0:5000 (52669)
[2023-11-28 10:56:09 +0900] [52669] [INFO] Using worker: sync
[2023-11-28 10:56:09 +0900] [52670] [INFO] Booting worker with pid: 52670

이렇게 나오면 WSGI 서버와 애플리케이션이 실행된것입니다.
"""
```

1. 유닉스(우분투) 소켓을 활용한 수행항법

유닉스 소켓: IPC 소켓이라고도 불리며 시스템내  프로세스 간의 데이터를 교환하기 위한 수단.

- **유닉스 환경에서** **서비스 시 포트말고 소켓을 사용하는 이유**
    
    유닉스 소켓은 동일한 호스트 내의 프로세스 간 통신(IPC)에 사용되며, 네트워크 스택을 거치지 않기에 전송 경로가 훨씬 짧고 포트를 사용한 서비스보다 더 빠릅니다.
    
    동일한 시스템 내의 서버와 애플리케이션 간 통신에서는 유닉스 소켓이 네트워크 소켓보다 선호됩니다.
    
    [프로토콜 스택](https://ko.wikipedia.org/wiki/프로토콜_스택)
    

```python
cd /home/{사용자명}/{프로젝트 루트 디렉터리}

gunicorn --bind unix:/tmp/gunicorn.sock {프로젝트명}.wsgi:application

"""
[2023-11-28 11:56:09 +0900] [52669] [INFO] Starting gunicorn 21.2.0
[2023-11-28 11:56:09 +0900] [52669] [INFO] Listening at: unix:/tmp/gunicorn.sock (52669)
[2023-11-28 11:56:09 +0900] [52669] [INFO] Using worker: sync
[2023-11-28 11:56:09 +0900] [52670] [INFO] Booting worker with pid: 52670
"""

```

<aside>
💡 유닉스 소켓 방식으로 gunicorn을 실행하면 반드시 Nginx와 같은 웹 서버가 필요합니다.

</aside>

1. 우분투에 Gunicron 서비스 등록

```bash
**sudo** vim /etc/systemd/system/{프로젝트명}.service # 편집기 실행

```

```bash
### {프로젝트명}.service

[Unit]
Description=gunicorn daemon
After=network.target

[Service]
User=ubuntu # 유저이름 확인해주세요.
Group=ubuntu
WorkingDirectory=/home/ubuntu/{프로젝트 디렉토리} # 프로젝트 루트 디렉터리
EnvironmentFile=/home/ubuntu/{프로젝트 디렉토리}/.env # 환경변수 파일
ExecStart=/home/ubuntu/{프로젝트 디렉토리}/venv/bin/gunicorn \ #가상환경에 설치된 gunicorn
        --workers 2 \ #워커 갯수
        --bind unix:/tmp/gunicorn.sock \ # WSGI실행 명령
        {프로젝트명}.wsgi:application # WSGI(Django) 애플리케이션

[Install]
WantedBy=multi-user.target
```

<aside>
💡 복사 붙여넣기 시 문법에러가 발생하는 경우가 있어 주석을 제외하고 따라치시며 구현하시길 바랍니다.

</aside>

```bash
# 서비스 실행
sudo systemctl start {위에서 작성했던 파일명}
```

```bash
# 실행확인
sudo systemctl status {위에서 작성했던 파일명} # .service 빼고입니다.
```

![정상 실행](https://prod-files-secure.s3.us-west-2.amazonaws.com/579fe283-28aa-489d-ae65-d683304becfc/59c03dc0-4b21-4f81-a60b-e29ce1810be4/Untitled.png)

정상 실행

```bash
sudo systemctl enable {위에서 작성했던 파일명} # .service 빼고입니다.
# 서버가 재실행 될 때 Gunicorn 서비스가 자동 실행되게 만들어 줍니다.

sudo systemctl restart {위에서 작성했던 파일명} # .service 빼고입니다.
# 서비스 재실행 명령
```

## 4. Nginx 셋팅

<aside>
💡 **프록시(Proxy**) : 한 네트워크 기기가 다른 기기를 대신하여 인터넷 또는 다른 네트워크에 연결되는 것을 의미합니다. 프록시는 중간 매개체의 역할을 하여 두 개의 네트워크 지점 간의 통신을 중계합니다.

</aside>

<aside>
💡 **프록시 서버**: 클라이언트(프론트)와 서버(백엔드) 사이에 위치하는 서버로, 클라이언트의 요청을 받아 이를 대신해서 다른 서버에 전달하고, 그 결과를 다시 클라이언트에게 전달하는 역할을 합니다.

</aside>

- **프록시 서버의 주요기능**
    1. 중계와 필터링 : 클라이언트의 요청을 다른 서버로 전달하면서 필요에 따라 해당 요청을 수정하거나, 특정 요청을 차단할 수 있습니다.
    2. 보안: 외부의 위협으로부터 내부 네트워크를 보호하는 역할을 합니다. 예를 들어, 외부 공격자가 내부 네트워크의 실제 IP를 알 수 없게 하여 보안성을 높 수 있습니다.(서버의 공개 IP주소와 내부IP주소는 다릅니다.)
    3. 규제 우회: 일부 지역에서 차단된 콘텐츠에 접근할 수 있도록 프록시 서버를 사용하여 지리적 제한을 우회할 수도 있습니다.

```bash
sudo vim /etc/nginx/sites-enabled/default # nginx 설정파일 편집 실행

server{
	root /var/www/html;
	server_name # 도메인 주소 없으면 IPV4 주소

	location /api/v1/{ # 클라이언트에서 오는 /api/v1/ 으로 시작되는 모든 요청 핸들링
             include proxy_params;
             proxy_pass http://unix:/tmp/gunicorn.sock;
        }

}
```

- proxy_pass : Nginx가 프론트의 요청을 전달해야 하는 백엔드 서버의 주소를 지정하는 명령어
- proxy_params
    - proxy_set_header Host `$host`;
        - 프론트 요청의 호스트 이름을 백엔드 서버에 전달하는 부분 (**`$host`** 변수는 ****헤더의 Host 부분**)**
    - proxy_set_header X-Real-IP `$remote_addr`;
        - `$remote_addr` 변수는 프론트의 실제 IP주소
    - proxy_set_header X-Forwarded-For `$proxy_add_x_forwarded_for;`
        - **`X-Forwarded-For` :** HTTP 헤더의 일종으로, 클라이언트의 원래 IP 주소를 포함합니다.
        - **`$proxy_add_x_forwarded_for`** 이 변수는 현재의 프론트 IP 주소와 기존에 **`X-Forwarded-For`** 헤더에 있던 모든 IP 주소를 포함합니다.
        요청이 여러 프록시를 거치면, 각 프록시의 IP 주소가 이 헤더에 추가됩니다. 따라서 이 헤더를 보면, 요청이 어떤 경로를 통해 왔는지 알 수 있습니다.
        - 결국 해당 부분은 클라이언트(프론트)의 요청이 인터넷을 통해 여러 서버를 거치며 전달될 때, 그 요청의 원래 출발지(IP 주소)를 백엔드 서버에게 알려주는 역할을 합니다.
    - proxy_set_header X-Forwarded-Proto `$scheme`;
        - 프로토콜(예: http 또는 https)을 백엔드 서버에 전달하는 부분 **`$scheme`** 변수는 요청이 사용한 프로토콜을 나타냅니다.
        
    
    ![포스트맨 실행](https://prod-files-secure.s3.us-west-2.amazonaws.com/579fe283-28aa-489d-ae65-d683304becfc/b05809db-bf57-464b-9e6d-bf86c9e24d6d/Untitled.png)
    
    포스트맨 실행
    

# 3. Github Actions를 통한 CI/CD 구축

## 1. 환경 구축

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/579fe283-28aa-489d-ae65-d683304becfc/b01b7fa7-8180-4500-8df8-9553402c4415/Untitled.png)

1. 레포지토리 설정 → Secrets and variables 클릭

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/579fe283-28aa-489d-ae65-d683304becfc/1355870b-a250-4a7c-bd71-c67f6dacc127/Untitled.png)

1. Gitaction에서 사용할 시크릿 키 추가하기

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/579fe283-28aa-489d-ae65-d683304becfc/a61da3b1-06b8-4f9f-8e9f-e195c5bb2729/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/579fe283-28aa-489d-ae65-d683304becfc/19e23e83-21f1-4f07-9baf-1b7b0a871ed1/Untitled.png)

1. Name에는 시크릿 이름을 Secret엔 값을 추가하면 등록이 됩니다. (특히, 관례상 시크릿 키는 대문자 및 언더바(_)로 작성합니다.)
2. 상단 Actions 탭 클릭
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/579fe283-28aa-489d-ae65-d683304becfc/ad0b54a1-f52c-41cb-a6fc-e51d4770ec88/Untitled.png)
    
    1. 액션 탭이 없다면 아래처럼 퍼미션을 수정해야합니다.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/579fe283-28aa-489d-ae65-d683304becfc/3fec1556-dacf-4213-8cdc-e2f251918275/Untitled.png)
    
3. **set up a workflow yourself** 클릭

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/579fe283-28aa-489d-ae65-d683304becfc/00b724db-b20a-4dfb-905b-02ede5cc1495/Untitled.png)

1. 깃허브 액션 트리거 파일 생성화면
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/579fe283-28aa-489d-ae65-d683304becfc/40f02cf8-e2f4-43a9-b0ee-6d4e8c50ff10/Untitled.png)
    

## 2. Git Actions 각 단계 설명

```yaml
name: Django CI/CD

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: 체크아웃 레포지토리
        uses: actions/checkout@v3

      - name: 파이썬 설정
        uses: actions/setup-python@v3
        with:
          python-version: '3.11'

      - name: 의존성 설치
        run: |
          pip install --upgrade pip
          pip install -r requirements.txt
          
      - name: 서버 배포
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.SERVER_HOST }} 
          username: ${{ secrets.SERVER_USER }}
          password: ${{ secrets.SERVER_PASSWORD }}
          script: |
						set -e
            cd /home/ubuntu/{레포지토리주소}
            git pull origin main

            echo "SECRET_KEY=\"${{ secrets.SECRET_KEY }}\"" > .env
            echo "DEBUG='${{ secrets.DEBUG }}'" >> .env

            source venv/bin/activate
            pip install -r requirements.txt
            sudo systemctl restart {아까등록한서비스이름}.service
```

```yaml
on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]
```

- 메인 브랜치에 코드가 `push`또는 `pull_request` 가 발생했을 때 수행됩니다.

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
```

- **`jobs:`**
    - 이 구문은 워크플로우에서 실행할 작업들을 정의하는 부분입니다. 여기서는 `deploy`라는 하나의 작업만을 정의하고 있습니다.
- **`deploy:`**
    - 이 부분은 `deploy`라는 이름의 작업을 설정합니다. 이 이름은 워크플로우 내에서 이 작업을 식별하는 데 사용됩니다.
- **`runs-on:`**
    - 이 지시어는 작업이 실행될 가상 환경(또는 러너)의 유형을 지정합니다. GitHub Actions는 다양한 운영 체제에서 작업을 실행할 수 있도록 여러 러너를 제공합니다.
- **`ubuntu-latest`**
    - 이 부분은 가장 최신 버전의 Ubuntu 운영 체제를 사용하겠다는 것을 의미합니다. 
    `ubuntu-latest`는 GitHub Actions에서 제공하는 가상 환경 중 하나로, 이 환경에서는 Ubuntu의 최신 안정 버전이 실행됩니다.
    - 이 설정은 작업이 Ubuntu 리눅스 환경에서 실행되어야 할 때 유용하며, 특히 리눅스 기반의 응용 프로그램을 빌드하고 테스트하는 데 적합합니다.

```yaml
    steps:
      - name: 체크아웃 레포지토리
        uses: actions/checkout@v3
```

- `steps` 는 이 워크플로우가 진행되는 각 단계(**action)**들을 의미합니다.
- `actions/checkout@v3` 라는 액션을 사용하여 레포지토리의 코드를 체크아웃하고, 프로젝트 코드를 복사하는 역할을 합니다.

```yaml
      - name: 파이썬 설정
        uses: actions/setup-python@v3
        with:
          python-version: '3.11'
```

- `actions/setup-python@v3`를 사용하여 파이썬 환경을 설정합니다.
- `python-version: '3.11'`로 지정하여 파이썬 3.11 버전을 사용합니다.
- 이 단계는 필요한 파이썬 버전을 설치하고 설정하는 역할을 합니다.

```yaml
      - name: 의존성 설치
        run: |
          pip install --upgrade pip
          pip install -r requirements.txt
```

- `pip`를 사용하여 필요한 의존성을 설치합니다.
- 여기서 `requirements.txt` 파일에 명시된 모든 패키지가 설치됩니다.
- 이 단계는 프로젝트가 필요로 하는 모든 외부 라이브러리를 설치하여 프로젝트가 올바르게 실행될 수 있도록 준비합니다.

```yaml
      - name: 서버 배포
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.SERVER_HOST }} 
          username: ${{ secrets.SERVER_USER }}
          password: ${{ secrets.SERVER_PASSWORD }}
          script: |
						set -e
            cd /home/ubuntu/{레포지토리주소}
            git pull origin main

            echo "SECRET_KEY=\"${{ secrets.SECRET_KEY }}\"" > .env
            echo "DEBUG='${{ secrets.DEBUG }}'" >> .env

            source venv/bin/activate
            pip install -r requirements.txt
            sudo systemctl restart {아까등록한서비스이름}.service
```

- `set -e` 스크립트 중 어느 부분에서든 명령어가 실패하면 즉시 전체 스크립트가 중단되고, 이에 따라 해당 GitHub Actions 작업이 실패 상태로 표시됩니다.
- `appleboy/ssh-action@master`를 사용하여 서버에 SSH 접속합니다.
- `host`, `username`, `password`는 GitHub Secrets를 통해 안전하게 관리됩니다.
- 스크립트를 통해 서버에 있는 프로젝트 디렉토리로 이동한 다음, 최신 코드를 `git pull`로 가져옵니다.
- `.env` 파일을 생성하거나 수정하여 환경 변수를 설정합니다.
- 가상 환경을 활성화하고 필요한 패키지를 설치한 후, `site.service` `(또는 여러분이 등록한 서비스 이름)`를 재시작하여 변경사항을 반영합니다.
- 이 단계는 프로젝트의 최신 버전을 서버에 배포하고 필요한 설정을 적용하는 역할을 합니다.

![수행단계](https://prod-files-secure.s3.us-west-2.amazonaws.com/579fe283-28aa-489d-ae65-d683304becfc/d094c965-44eb-4891-ba83-91c058b5c0c6/Untitled.png)

수행단계

![성공시](https://prod-files-secure.s3.us-west-2.amazonaws.com/579fe283-28aa-489d-ae65-d683304becfc/f59907c4-7f97-41b9-b536-0d9a31af2486/Untitled.png)

성공시

![실패시](https://prod-files-secure.s3.us-west-2.amazonaws.com/579fe283-28aa-489d-ae65-d683304becfc/cbad45bb-e274-461c-8b47-1b0894481593/Untitled.png)

실패시












  
![image](https://github.com/chimaek/pyweb_ci_cd_2024/assets/24273120/d678d71f-dee8-4b7c-ab43-3813c10247e2)


- DUBUG : PROD 모드일 땐 False 권장
- SCRET_KEY : Django settings.py 에 있는 장고 시크릿 키
- SERVER_HOST : 서버 도메인 주소 또는 IPV4 주소
- SERVER_PASSWORD: 서버 패스워드
- SERVER_USER : 서버 유저 이름

각 내용을 레포지토리 Settings -> Actions secrets and variables -> Repository secrets -> new 를 클릭하여 추가하시면 됩니다.
