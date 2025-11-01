# Open WebUI - Local Development Guide

개인 프로젝트를 위한 빠른 로컬 개발 환경 설정 가이드입니다.

## 🚀 빠른 시작

### 필수 요구사항

- **Node.js**: 18.13.0 이상, 22.x.x 이하
- **Python**: 3.11
- **Ollama**: (선택) LLM 모델 실행

### 1. 프로젝트 클론

```bash
git clone <repository-url>
cd open-webui
```

### 2. 프론트엔드 설정

```bash
# 의존성 설치
npm install

# 개발 서버 실행 (Hot reload 지원)
npm run dev
```

프론트엔드는 `http://localhost:5173`에서 실행됩니다.

### 3. 백엔드 설정

```bash
# uv 설치 (아직 없는 경우)
pip install uv

# 방법 1: uv sync 사용 (권장 - pyproject.toml 기반)
uv sync
source .venv/bin/activate  # Windows: .venv\Scripts\activate

# 방법 2: 수동 설정
cd backend
uv venv
source .venv/bin/activate  # Windows: .venv\Scripts\activate
uv pip install -r requirements.txt

# 개발 서버 실행 (Auto reload 지원)
cd backend  # 방법 1을 사용한 경우
./dev.sh
```

백엔드는 `http://localhost:8080`에서 실행됩니다.

### 4. Ollama 설정 (선택)

로컬에 Ollama가 없다면 Docker로 실행:

```bash
docker run -d -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama:latest
```

Ollama는 `http://localhost:11434`에서 실행됩니다.

## 📁 프로젝트 구조

```
open-webui/
├── src/                 # 프론트엔드 소스 (SvelteKit)
├── backend/            # 백엔드 소스 (FastAPI)
│   └── open_webui/    # 메인 백엔드 코드
├── static/            # 정적 파일
├── package.json       # 프론트엔드 의존성
└── docker-compose.yaml # Docker 구성
```

## 🔧 개발 스크립트

### 프론트엔드

```bash
npm run dev          # 개발 서버 (포트 5173)
npm run dev:5050     # 개발 서버 (포트 5050)
npm run build        # 프로덕션 빌드
npm run build:watch  # 빌드 watch 모드
npm run lint         # 코드 린팅
npm run format       # 코드 포맷팅
```

### 백엔드

```bash
cd backend
./dev.sh            # 개발 서버 (auto-reload)
./start.sh          # 프로덕션 서버
```

## 🐳 Docker 개발 환경

로컬에서 직접 실행하는 것이 가장 빠르지만, Docker를 사용할 수도 있습니다:

```bash
# docker-compose로 전체 스택 실행
docker-compose up -d

# 로그 확인
docker-compose logs -f

# 중지
docker-compose down
```

**참고**: 현재 docker-compose.yaml은 개발용 볼륨 마운트가 설정되어 있어 코드 변경이 즉시 반영됩니다.

## ⚙️ 환경 변수

`.env` 파일을 생성하여 환경 변수를 설정할 수 있습니다:

```bash
cp .env.example .env
```

주요 환경 변수:

```bash
# Ollama 설정
OLLAMA_BASE_URL=http://localhost:11434

# OpenAI API (선택)
OPENAI_API_BASE_URL=
OPENAI_API_KEY=

# CORS 설정 (개발용)
CORS_ALLOW_ORIGIN=http://localhost:5173;http://localhost:8080

# 텔레메트리 비활성화
SCARF_NO_ANALYTICS=true
DO_NOT_TRACK=true
ANONYMIZED_TELEMETRY=false
```

## 🔍 문제 해결

### 프론트엔드가 백엔드에 연결되지 않는 경우

1. 백엔드가 `http://localhost:8080`에서 실행 중인지 확인
2. CORS 설정 확인 (backend/dev.sh의 CORS_ALLOW_ORIGIN)

### 의존성 설치 오류

```bash
# npm 의존성 재설치
rm -rf node_modules package-lock.json
npm install

# Python 의존성 재설치
pip install --upgrade uv
uv pip install -r backend/requirements.txt
```

### Ollama 연결 오류

1. Ollama가 실행 중인지 확인: `curl http://localhost:11434/api/tags`
2. 환경 변수 `OLLAMA_BASE_URL` 확인

## 📝 개발 워크플로우

1. **프론트엔드 개발**: `npm run dev`로 개발 서버 실행 → 코드 수정 → 브라우저 자동 새로고침
2. **백엔드 개발**: `./backend/dev.sh`로 개발 서버 실행 → 코드 수정 → uvicorn auto-reload
3. **전체 스택 테스트**: 프론트엔드(5173) + 백엔드(8080) 동시 실행

## 🎯 빠른 개발을 위한 팁

- **Hot Reload**: 프론트엔드와 백엔드 모두 파일 변경 시 자동으로 재시작됩니다.
- **로컬 실행 권장**: Docker 없이 로컬에서 직접 실행하는 것이 가장 빠릅니다.
- **uv 사용**: pip보다 10-100배 빠른 패키지 설치 속도를 경험하세요.
- **포트 확인**:
  - 프론트엔드: 5173
  - 백엔드: 8080
  - Ollama: 11434

## 💡 uv를 사용하는 이유

- **속도**: pip보다 훨씬 빠른 패키지 설치 (Rust로 작성됨)
- **신뢰성**: 더 나은 의존성 해결
- **호환성**: pip와 완전히 호환되는 인터페이스

---

**개발 환경 준비 완료!** 🎉

코드를 수정하면 즉시 반영됩니다. 행복한 코딩 되세요!
