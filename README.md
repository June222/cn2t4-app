# 디렉토리 구조
```bash
cn2t4-bank-project/
├── frontend/        # Vanilla JS 프로젝트
│   ├── Dockerfile
│   ├── index.html
│   ├── css/
│   └── js/
├── backend/         # Django 프로젝트
│   ├── Dockerfile
│   ├── config/      # 프로젝트 설정
│   ├── api/             # (임시 테스트용)
│   ├── auth/            # 로그인, 인증 관련
│   ├── users/           # 사용자 관리
│   ├── accounts/        # 계좌 관리
│   ├── transactions/    # 거래 처리
│   ├── notifications/   # 알림 시스템
│   ├── common/          # 공통 유틸리티, 예외 처리 등
│   ├── manage.py
│   ├── requirements.txt
└── docker-compose.yml  # 전체 프로젝트를 관리하는 파일
```
---
# 📦 로컬 개발 환경 구성 (Docker Compose 사용)

✅ 사전 준비
- Docker Desktop 설치
- git 설치

📂 1. 프로젝트 클론
```bash
git clone https://github.com/nolzaheo/cn2t4-bank-project.git
```

🚀 2. Docker 이미지 빌드 및 컨테이너 실행
```bash
docker-compose up -d --build
```

🧪 3. 프로젝트 실행 및 API 연결 확인
웹 브라우저에 아래 주소 입력
- 프론트엔드(Vanilla JS + Nginx): http://localhost
- 백엔드(Django): http://localhost:8000
- API 테스트: http://localhost:8000/api/hello -> { "message": "Hello from Django!" }

🔁 4. 개발 중 실시간 반영  
- 프론트엔드와 백엔드 디렉토리는 volumes로 마운트되어 있어 코드 수정 시 자동 반영됩니다.

🛑 5. 종료
```bash
docker-compose down
```
---
# 📦 데이터베이스 마이그레이션 가이드

✅ 모델을 수정한 사람 (Git에 올리는 사람)

Django 모델을 수정하거나 새로운 모델을 추가한 경우, 다음 과정을 따라주세요.

1. 모델 수정 후 마이그레이션 파일 생성 및 마이그레이션 파일 반영

```bash
docker exec -it django_backend python manage.py makemigrations
docker exec django_backend python manage.py migrate

# 또는
# 다음 명령으로 컨테이너에 직접 진입하여 실행할 수도 있습니다:

docker exec -it django_backend bash
python manage.py makemigrations
python manage.py migrate
```

2. 생성된 마이그레이션 파일(migrations/000X_*.py)을 커밋 & push

```bash
git add .
git commit -m "모델 수정 및 마이그레이션 파일 추가"
git push
```

3. 🔥 팀원에게 모델이 변경 됐다고 알려주기!!

✅ 다른 개발자 (Git에서 내려받는 사람)

1. 자기 로컬 DB에 마이그레이션 하기

```bash
git pull
docker exec django_backend python manage.py migrate
```

---
