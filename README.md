다음은 제공된 파일과 AWS ECR에 컨테이너 이미지를 업로드하고 AWS App Runner를 사용하여 실행하는 과정을 포함한 `README.md`입니다:

```markdown
# LangChain 애플리케이션

이 프로젝트는 LangChain 기반 애플리케이션입니다. 이 설정은 Docker 컨테이너를 빌드하고, 이를 AWS ECR에 업로드한 후 AWS App Runner를 사용하여 배포합니다.

## 프로젝트 구조

- `docker-compose.yaml`: Docker의 서비스, 네트워크 및 볼륨을 정의합니다.
- `Dockerfile.cloud`: Docker 이미지를 빌드하기 위한 Dockerfile입니다.
- `main.py`: 애플리케이션의 메인 진입점입니다.
- `requirements.txt`: Python 종속성을 나열합니다.

## 사전 요구 사항

- Docker
- AWS CLI
- AWS ECR 저장소
- AWS App Runner

## 설정 및 사용 방법

### 1. 로컬 환경에서 Docker 컨테이너 실행

```bash
docker-compose up --build
```

### 2. AWS ECR에 Docker 이미지 업로드

1. AWS CLI를 통해 로그인:

    ```bash
    aws ecr get-login-password --region <your-region> | docker login --username AWS --password-stdin <your-account-id>.dkr.ecr.<your-region>.amazonaws.com
    ```

2. ECR 리포지토리 생성:

    ```bash
    aws ecr create-repository --repository-name langchain-repo --region <your-region>
    ```

3. Docker 이미지 태그 지정:

    ```bash
    docker tag my_custom_image_name:langchain <your-account-id>.dkr.ecr.<your-region>.amazonaws.com/langchain-repo:latest
    ```

4. Docker 이미지 푸시:

    ```bash
    docker push <your-account-id>.dkr.ecr.<your-region>.amazonaws.com/langchain-repo:latest
    ```

### 3. AWS App Runner를 사용하여 컨테이너 실행

1. AWS Management Console에서 App Runner로 이동합니다.
2. 새 서비스 생성 버튼을 클릭합니다.
3. 소스 타입을 컨테이너 레지스트리로 선택합니다.
4. 배포할 ECR 이미지를 선택합니다.
5. 서비스 설정을 완료하고 서비스를 생성합니다.

## 파일 설명

### `docker-compose.yaml`

```yaml
version: '3.8'
services:
  langchain:
    build: .
    image: my_custom_image_name:langchain
    volumes:
      - ./.venv:/app/.venv
      - .:/app
      - ./projectDB:/app/projectDB
    ports:
      - "8000:8000"
    environment:
      - WATCHFILES_FORCE_POLLING=true

networks:
  my_custom_network:
    driver: bridge
```

### `requirements.txt`

```plaintext
fastapi==0.110.3
pydantic==2.7.1
pytube==15.0.0
pydub==0.25.1
simpleaudio==1.0.4
google-api-python-client==2.130.0
python-dotenv==1.0.1
pdfplumber==0.5.28
langchain==0.2.6
langchain-openai==0.1.11
langchain-community==0.2.6
pandas==2.2.2
chromadb==0.5.3
uvicorn[standard]==0.20.0
sqlalchemy==2.0.3
pymysql==1.0.2
aiomysql==0.1.1
pytest-asyncio==0.20.3
aiosqlite==0.18.0
httpx>=0.27.0
```

이 설명서를 통해 프로젝트를 설정하고 AWS에 배포할 수 있습니다. 추가적인 도움이 필요하면, 문서를 참조하거나 담당자에게 문의하세요.
```

이 `README.md` 파일은 프로젝트를 설정하고 사용하는 방법을 안내합니다. 필요에 따라 파일 내용을 조정해 주세요.