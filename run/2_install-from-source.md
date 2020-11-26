# [Option 2] Source Install

```note
FOSSology Source Code가 [Github Repository](https://github.com/fossology/fossology)에서 배포됩니다.
```

## 필요 사항

- Linux 환경

- PHP 5.6.x 혹은 7.0.x

- Postgresql

- Apache httpd 2.6

## 설치  방법

### 0. Source Code 다운로드

```
$ git clone https://github.com/fossology/fossology.git
$ cd fossology
```

### 1. 기존 FOSSology 삭제 / Dependency 설치/ 빌드

```
# utils/fo-cleanold
# utils/fo-installdeps
$ make
```

```note
fo-installdeps Script에는 lsb-release 명령이 사용되었습니다.
Fedora/RedHat 계열 OS에서 fo-installdeps를 사용하기 위해서는 redhat-lsb가 사용되도록 적절히 수정되어야합니다.
```

### 2. Install / Post script / Daemon 실행

```
# make install
# /usr/local/lib/fossology/fo-postinstall
# systemctl enable --now fossology
```

### 3. 환경 설정

#### 3.1. 저장 공간

```note
FOSSology 시스템은 임시 파일, Log, 업로드된 파일을 하나의 폴더에 저장합니다.
기본값은 /srv/fossology/repository 이므로, 별도의 마운트에 저장공간을 위치시키기 위해서는 fossology/install/defconf/fossology.conf.in의 path 변수를 변경해야 합니다.
```

- 예)
  - (기존) path = /srv/fossology/repository 
  - (변경) path = /home/fossy/repository

#### 3.2. Kernel Shared Memory

```note
PostgreSQL에 더 많은 Memory를 할당 할 수 있도록 Shared Memory를 조정합니다.
참고 : [http://www.postgresql.org/docs/9.0/static/kernel-resources.html](http://www.postgresql.org/docs/9.0/static/kernel-resources.html)
```

shm.sh 파일을 아래와 같이 생성합니다.
```
#!/bin/bash
page_size=`getconf PAGE_SIZE`
phys_pages=`getconf _PHYS_PAGES`
shmall=`expr $phys_pages / 2`
shmmax=`expr $shmall \* $page_size`
echo kernel.shmmax=$shmmax
echo kernel.shmall=$shmall
```

shm.sh 의 출력값을 반영합니다. (아래의 값은 예시입니다.)

```
# sysctl -w kernel.shmmax=17179869184
# sysctl -w kernel.shmall=4194304
```

변경 값이  부팅 시 자동 반영되도록 합니다.

```
# ./shm.sh >> /etc/sysctl.conf
```

#### 3.3. PostgreSQL 설정

PostgreSQL의 원활한 동작을 위해 설정값을 변경합니다.

```
# vi /etc/postgresql/<버전>/main/postgresql.conf
```

아래의 예시는 4GB RAM을 갖는 환경을 가정한 것입니다.

```
listen_addresses = '*'              # listen to connections from all IPs
max_connections = 50                # maximum number of client connections
                                    # allowed
shared_buffers = 1GB                # If you have a system with 1GB or more
                                    # of RAM, a reasonable starting value
                                    # for shared_buffers is 1/4 of the memory
                                    # in your system.
effective_cache_size = 2GB          # how much memory is available for disk
                                    # caching; 1/2-3/4 of memory
work_mem = 128MB                    # work_mem parameter allows PostgreSQL
                                    # to do larger in-memory sorts
maintenance_work_mem = 200MB        # roughly 50MB/GB of ram
fsync = on                          # turns forced synchronization on or off
full_page_writes = off              # recover from partial page writes
log_line_prefix = '%t %h %c'        # prepend a time stamp to all log entries
standard_conforming_strings = on
autovacuum = on                     # Enable autovacuum subprocess?  'on'
```

PostgreSQL 인증 설정을 변경합니다.

```
# vi /etc/postgresql/9.5/main/pg_hba.conf
```

FOSSology 서비스를 로컬에서만 사용한다면 local 열만 설정하고, 원격에서도 사용한다면 host 열을 설정합니다.

```
# local      DATABASE  USER  METHOD  [OPTION]
  local       all      all                md5
  host        all      all   0.0.0.0/0    password
```

PostgreSQL 서비스를 재시작 합니다.

```
# /etc/init.d/postgresql-<버전> restart
```

#### 3.4 PHP 설정

사용되는 PHP 버전에 따라 업로드 및 분석에 사용될 자원을 설정합니다.

```
# vi /etc/php5/apache2/php.ini
혹은
# vi /etc/php/7.0/apache2/php.ini
```

시스템이 허용하는 자원에 따라 아래와 같이 설정을 변경합니다.

```
; 일회성 분석에 사용되는 'one shot'이 동작 가능한 시간입니다.
max_execution_time = 300
; Web UI에서 업로드 시 사용되는 값입니다.
memory_limit = 702M
post_max_size = 701M
upload_max_filesize = 700M
```

#### 3.5 Apache 설정

```note
운영자의 선택에 따라 기존에 존재하는 Site 설정을 수정할 수도 있고, 새로운 Site를 추가할 수 있습니다.
아래의 예시에서는 새로운 Site를 추가하고, "/repo/" 경로에서 접속하도록 설정합니다.
```

Apache Site 목록에 FOSSology 설정 파일(fossology.conf)을 추가합니다.

```
# vi /etc/apache2/sites-available/fossology.conf
```

Host/repo/로 접속하면 설치된 FOSSology UI가 동작하도록 설정합니다.

```
Alias "/repo" "/usr/local/share/fossology/www/ui"
<Directory "/usr/local/share/fossology/www/ui">
    AllowOverride None
    Options FollowSymLinks
    <IfVersion < 2.3>
        order allow,deny
        allow from all
        Satisfy Any
    </IfVersion>
    <IfVersion >= 2.3>
        Require all granted
    </IfVersion>
    # uncomment to turn on php error reporting
    #php_flag display_errors on
    #php_value error_reporting 2039
</Directory>
```

새로 생성된 FOSSology 설정 파일이 정상적으로 작성되었는지 테스트 합니다.

```
# service apache2ctl configtest
```

Apache 서비스를 재구동 하여 FOSSology UI 서비스를 시작합니다.

```
# service apache2 restart
```
