# Docker Volume

## Docker Volume이란?

Docker Volume은 **컨테이너 외부에 데이터를 저장하는 공간**이다. 일반적으로 컨테이너 내부의 데이터는 컨테이너가 삭제되면 함께 사라지지만, Volume을 사용하면 데이터를 별도로 저장하여 컨테이너를 삭제하거나 다시 생성해도 데이터를 유지할 수 있다. 따라서 데이터베이스, 로그, 업로드 파일 등 영구적으로 보관해야 하는 데이터를 저장하는 데 주로 사용된다.

---

## Docker Volume의 종류

### 1. Volume

Docker가 직접 관리하는 저장 공간으로, 가장 일반적으로 사용되는 방식이다. 데이터 관리와 백업이 용이하며 안정적으로 사용할 수 있다.

### 2. Bind Mount

호스트 컴퓨터의 특정 디렉터리를 컨테이너와 연결하는 방식이다. 개발 환경에서 소스 코드를 실시간으로 공유할 때 많이 사용된다.

### 3. tmpfs Mount

데이터를 디스크가 아닌 메모리에 저장하는 방식이다. 입출력 속도가 빠르지만 컨테이너가 종료되면 데이터가 모두 삭제된다.

---

## Docker Volume 사용 방법

### 1. Volume 생성

```bash
docker volume create my-volume
```

### 2. Volume 목록 확인

```bash
docker volume ls
```

### 3. Volume 정보 확인

```bash
docker volume inspect my-volume
```

### 4. 컨테이너와 연결

```bash
docker run -d \
  --name my-container \
  -v my-volume:/data \
  nginx
```

위 예시는 `my-volume`을 컨테이너 내부의 `/data` 디렉터리와 연결하는 방법이다.

### 5. Volume 삭제

```bash
docker volume rm my-volume
```

---

## Docker Volume 활용 예시

MySQL 데이터베이스를 실행할 때 데이터를 Volume에 저장하면 컨테이너가 삭제되어도 데이터는 그대로 유지된다.

```bash
docker run -d \
  --name mysql-container \
  -e MYSQL_ROOT_PASSWORD=1234 \
  -v mysql-data:/var/lib/mysql \
  mysql:latest
```

---

## Docker Compose에서의 사용

Docker Compose에서는 `volumes` 항목을 통해 Volume을 쉽게 설정할 수 있다.

```yaml
version: "3.8"

services:
  db:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: 1234
    volumes:
      - db-data:/var/lib/mysql

volumes:
  db-data:
```

이 예시는 `db-data`라는 Volume을 생성하여 MySQL의 데이터를 저장한다.

---

## Docker Volume의 장점

* **데이터 영속성** : 컨테이너가 삭제되어도 데이터가 유지된다.
* **백업 및 복원 용이** : 데이터를 쉽게 백업하고 복원할 수 있다.
* **높은 성능** : Docker가 저장 공간을 직접 관리하여 효율적인 입출력을 제공한다.
* **데이터 공유** : 여러 컨테이너가 하나의 Volume을 함께 사용할 수 있다.

---

## Volume과 Bind Mount 비교

| 구분    | Volume       | Bind Mount       |
| ----- | ------------ | ---------------- |
| 관리 주체 | Docker       | 사용자              |
| 저장 위치 | Docker 관리 영역 | 호스트의 지정한 디렉터리    |
| 주요 용도 | 데이터 영구 저장    | 소스 코드 공유 및 개발 환경 |
| 보안성   | 높음           | 호스트 파일 접근 가능     |

---

## 결론

Docker Volume은 **컨테이너와 독립적으로 데이터를 저장하는 기능**으로, 컨테이너를 삭제하거나 재생성해도 데이터를 안전하게 유지할 수 있다. 데이터베이스, 로그, 업로드 파일 등 영구적인 저장이 필요한 서비스에서 필수적으로 사용되며, Docker Compose와 함께 사용하면 여러 컨테이너의 데이터를 효율적으로 관리할 수 있다.
