---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
title: This Is the Introduce
profile_picture:
    src: /assets/img/content/index-img.jpg
    alt: website picture
---

<u>밑줄이얌</u>
# MicroCloudChip-NATURAL

# 개요
* 전 작 [MicroCloudChip](https://github.com/SweetCase-Cobalto/MicroCloudChip) 의 후속작
* 원격 서버 및  NAS Server의 파일 관리 서비스를 지원하기 위해 개발된 설치형 저용량 파일 호스팅 서비스
* 타 서버로부터 금액을 지불하고 일정 용량을 할당 받는 것이 아닌, 개인 서버만 갖고 있으면 이 웹 어플리케이션을 이용해 파일 호스팅 서버를 운용할 수 있습니다.
* Docker Image를 이용할 경우 외부 저장소를 지원합니다. 외부 저장소 환경변수를 추가하면  불가피하게 Container가 shutdown되도 저장된 파일들을 접근할 수 있습니다.
* Database도 외부 MySQL같은 Database를 사용할 수 있습니다

**_NOTE:_** 윈도우 플랫폼에서는 서버를 테스트 할 수 있는 GUI Console만 지원하고 서비스 등록은 지원하지 않습니다.

# Installation
## Install With Docker Container
### Install With Internal Database (sqlite3)
```bash
sudo docker run -it -d -p [port]:[port] \
                -e SERVER_PORT=[port] \
                -e ADMIN_EMAIL=[your email] \
                -e HOST=[your host] \
                --name [container name] ghcr.io/sweetcase-cobalto/microcloudchip-natural:0.0.3-internal
```

### Install With External Database (MySQL, MariaDB)
```bash
sudo docker run -it -d -p [port]:[port] \
                -e SERVER_PORT=[port] \
                -e ADMIN_EMAIL=[your email] \
                -e HOST=[your host] \
                -e DB_NAME=[database name] \
                -e DB_USER=[database user name] \
                -e DB_PSWD=[database password] \
                -e DB_HOST=[database host] \
                -e DB_PORT=[database port] \
                --name [container name] ghcr.io/sweetcase-cobalto/microcloudchip-natural:0.0.3-mysql
```
### 외부 저장소 설정
**_NOTE:_** 외부 저장소를 사용할 경우 아래의 환경변수를 추가합니다.
```bash
-v [src]:[dst] \
-e STORAGE_ROOT=[dst] \
```

<br>

자세한 설치 방법은 [Gitbook](https://seokbong60.gitbook.io/microcloudchip-natural/v/v0.0.x/wiki-for-users-and-guests/how-to-install/by-docker-container) 참고
# Setting Project For Developers
이 프로젝트를 개작 또는 분석 하려는 개발자를 위한 세팅 방법입니다. <br>
자세한 방법은 [Gitbook](https://seokbong60.gitbook.io/microcloudchip-natural/v/v0.0.x/wiki-for-developers/project-setting) 을 참고해 주세요
## Windows
* powershell를 사용합니다.
1. Install Package
    1. install python (over 3.9.x)
    2. install nodejs (over 14.x)
    3. install perl *(설정 파일이나 기타 필수 텍스트 파일을 생성하기 위해 설치합니다.)*
2. download respository
    ```powershell
        PS X:\> git clone https://github.com/SweetCase-Cobalto/MicroCloudChip-NATURAL.git
    ```
    * Microcloudchip-NATURAL Repository는 크게 세 가지의 Branch가 관리되고 있습니다.
        1. **master**: Minor 이상의 버전 단위의 Branch로 기능을 추가하는 데 사용합니다.
        2. **0.?.x**: ```v0.?``` 의 Patch 버전 단위의 Branch로 어플리케이션 내 버그를 수정하는 데 사용합니다. ```master``` 로부터 파생돠어야 합니다. Release가 완료되면 master에서 진행하는 버전이 minor버전 까지 같을 경우,  ```master```로 병합합니다.
        3. **0.?.?-product**: ```0.?.x``` branch에서 목표치의 구현이 완성되었을 경우, Install Test를 하기 위해 ```0.?.x``` 로부터 분리된 Branch 입니다. Install Test가 완료되었으면 ```0.?.?``` 버전을 Release 합니다. **해당 Branch를 이용한 개발을 하지 않는 것을 권장합니다.**

3. Install python & nodejs packages
    ```powershell
        # goto project directory
        PS: X:\> cd project
        # setting frontend
        PS: X:\project> cd web
        PS: X:\project/web> npm i
        # back to project root
        PS: X:\project> cd ..
        # setting backend
        # 파이썬 가상머신을 설치했다고 가정합니다
        PS: (micro)X:\project> cd app
        PS: (micro)X\project\app> pip install -r requirements.txt
    ```
    **_NOTE:_** Backend package를 설치할 때 ```pip upgrade```를 해놓을 경우 package 설치에 문제가 있으니 ```pip upgrade```를 수행하지 않는 것을 권장합니다.

4. write config.json
    * config.json 은 Microcloudchip-NATURAL이 실행을 하기 위해 참고하는 설정 파일입니다. 어플리케이션을 설치할 때는 자동으로 생성해 주지만, 개발 단계에서는 perl script를 사용하여 직접 세팅해야 합니다.
    * 내부 데이터베이스(Sqlite3를 사용하는 경우)
        ```powershell
        PS: X:\project> cd bin
        PS: X:\project/bin> perl setConfigure-sqlite.pl [storage root] [port] [host] [email]
        ```
    * 외부 데이터베이스(MySQL, MariaDB)를 사용하는 경우 *(0.1.x 부터 개발 가능합니다.)*
        ```powershell
        PS: X:\project> cd bin
        PS: X:\project/bin> perl setConfigure-mysql.pl [storage root] [port] [host] [email] `
        >> [db-host] [db-port] [db-user-name] [db-user-pswd] [db-name]
        ```

```c
int main(void) {
    print("hello world");
}
```