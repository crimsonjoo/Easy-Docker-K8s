
# Docker Compose란 무엇인가?

Docker Compose는 여러 컨테이너를 오케스트레이션/관리하는 데 도움이 되는 Docker 생태계에서 제공하는 추가 도구입니다. 
단일 컨테이너의 빌드 및 실행을 간소화하는 데에도 사용할 수 있습니다.

## 왜 Docker Compose를 사용할까요?

아래는 매우 간단한 (가상의) 예시입니다. 그러나 이 애플리케이션에 필요한 모든 컨테이너를 실행하려면 상당히 많은 명령어를 실행하고 기억해야 합니다.
코드에 무언가를 변경하거나 다른 이유로 컨테이너를 다시 실행해야 할 때마다 이러한 명령어 중 대부분을 실행해야 합니다.

Docker Compose를 사용하면 이 과정이 훨씬 더 쉬워집니다.
컨테이너 구성을 `docker-compose.yaml` 파일에 넣고, 단 하나의 명령어로 전체 환경을 실행할 수 있습니다: `docker-compose up`.

## Docker Compose 파일

`docker-compose.yaml` 파일은 다음과 같은 구조를 가지고 있습니다:

```bash
docker network create shop
docker build -t shop-node .
docker run -v logs:/app/logs --network shop --name shope-web shop-node
docker build -t shop-database
docker run -v data:/data/db --network shop --name shop-db shop-database
```

### 예시 `docker-compose.yaml`

```yaml
version: "3.8" # Docker Compose 스펙의 버전
services: # "서비스"는 결국 앱에 필요한 컨테이너입니다.
  web:
    build: # 이 컨테이너의 이미지를 위한 Dockerfile 경로를 정의합니다.
      context: .
      dockerfile: Dockerfile-web
    volumes: # 필요한 볼륨/바인드 마운트를 정의합니다.
      - logs:/app/logs
  db:
    build:
      context: ./db
      dockerfile: Dockerfile-db
    volumes:
      - data:/data/db
```

이 파일을 언제든지 편리하게 수정할 수 있으며, 간단한 명령어 하나로 컨테이너를 실행할 수 있습니다:

```bash
docker-compose up
```

Docker Compose를 사용할 때 중요한 점은 모든 컨테이너에 대해 네트워크가 자동으로 생성된다는 것입니다. 
따라서 여러 네트워크가 필요하지 않는 한 별도로 네트워크를 추가할 필요는 없습니다!

## Docker Compose의 주요 명령어

- `docker-compose up`: Docker Compose 파일에 언급된 모든 컨테이너/서비스를 시작합니다.
  - `-d`: 백그라운드 모드로 시작
  - `--build`: 모든 이미지를 강제로 다시 빌드 (이미지가 없을 때만 빌드 수행)

- `docker-compose down`: 모든 컨테이너/서비스를 중지하고 제거합니다.
  - `-v`: 컨테이너에 사용된 모든 볼륨 제거 (컨테이너가 제거되어도 볼륨은 남아있음)

물론 더 많은 명령어가 존재합니다. 다른 섹션 (예: "유틸리티 컨테이너" 및 "Laravel 데모" 섹션)에서 더 많은 명령어를 확인할 수 있습니다. 
또는 공식 명령어 참조를 통해 직접 더 알아볼 수 있습니다: [Docker Compose 공식 명령어 참조](https://docs.docker.com/compose/reference/)
