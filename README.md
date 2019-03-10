# stack-docker
이 레파지토리의 Docker Compose 설정은 엘라스틱스택의 많은 요소들을 Docker 기반으로 단일 머신에서 모두 구동시키기 위한 것입니다.

## 사전 조건
- Docker와 Docker Compose가 필요합니다.
  * 윈도우나 맥을 사용하는 사용자들은 Docker for Mac 또는 Docker for Windows를 사용하여 자동으로 설치할 수 있습니다.

  * 리눅스 사용자들은 다음 [문서](https://docs.docker.com/compose/install/#install-compose)를 참고하여 아래와 같이 pip로 설치할 수 있습니다.

    ```
    pip install docker-compose
    ```

    
* 윈도우 사용자들은 반드시 다음 두가지 환경 변수를 설정해야 합니다.
  * `COMPOSE_CONVERT_WINDOWS_PATHS=1`
  * `PWD=/path/to/checkout/for/stack-docker`

    * 예를 들어 저는 로컬에 이 레파지토리를 git clone 한 경로로 다음 경로를 사용한다고 가정하겠습니다: `/c/Users/nick/elastic/stack-docker`
    * 주의: 반드시  `/c/path/to/place` 와 형식으로 경로를 작성해야 합니다. 일반적인 윈도우의 경로처럼  `C:\path\to\place`와 같이 작성하시면 안됩니다.
  * 다음 세가지 방법으로 설정할 수 있습니다.:
    1. 파워쉘을 사용하여 임시로 환경 변수를 추가하는 방법은 다음과 같습니다: `$Env:COMPOSE_CONVERT_WINDOWS_PATHS=1`
    2. 파워쉘을 사용하여 영구적으로 환경 변수를 추가하는 방법은 다음과 같습니다: `[Environment]::SetEnvironmentVariable("COMPOSE_CONVERT_WINDOWS_PATHS", "1", "Machine")`

      > 주의: 해당 설정을 반영하기 위해서는 refresh를 하거나 파워쉘을 새로 구동해야 할 수도 있습니다.
    3. 시스템 설정(System Properties)에 환경 변수를 추가합니다.


* 컨테이너 구동을 위해 최소한 4GiB의 RAM 메모리 공간이 필요합니다. 윈도우와 맥 사용자들은 반드시 기본값으로 설정되어 있는 2GiB의 RAM 메모리 공간보다 더 큰 메모리(4GiB 이상)로 Docker 가상 머신을 설정해야 합니다:

![Docker VM memory settings](screenshots/docker-vm-memory-settings.png)

* 리눅스 사용자들은 반드시 `root` 권한으로 다음 설정을 수행해야합니다:

```
sysctl -w vm.max_map_count=262144
```
기본값으로 할당되어 있는 가상 메모리 공간으로는 [부족합니다](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html).


## stack 시작하기

먼저 우리는 다음 절차를 수행해야합니다:

1. 기본 패스워드 설정
2. 패스워드들을 저장할 keystore 생성
3. 비츠와 apm에서 사용되는 인덱스 패턴 등을 위한 대시보드 설치

위 절차들은 setup.yml 파일을 사용하여 수행됩니다. 아래 명령을 실행하십시오:

```
docker-compose -f setup.yml up
```

설치가 완료되고나면 출력되는 note를 확인해주세요. 콘솔에 출력되는 내용에 패스워드가 포함되어 있습니다. 이 패스워드를 사용하여 elastic에 로그인 할 수 있습니다.

이제 우리는  `docker-compose up -d` 명령으로 stack을 실행할 수 있습니다. 이 명령을 통해 Elasticsearch, Kibana, Logstash, Auditbeat, Metricbeat, Filebeat, Packetbeat,
and Heartbeat로 구성된 데모용 Elastic Stack이 생성됩니다.

브라우저를 사용하여 [`http://localhost:5601`](http://localhost:5601) 접속하고 결과를 확인해보십시오.
> *주의*: 엘라스틱서치는 현재 자가서명(self-signed) 인증서(certs)로 설치되었습니다.

`elastic`에 로그인하기 위한 자동 생성된 패스워드는 설치시 표시된 패스워드를 사용합니다.

