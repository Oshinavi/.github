## 💻 기술 스택

### 🖥️ Frontend (Flutter)
<div align="center">

![Flutter](https://img.shields.io/badge/Flutter-02569B?style=for-the-badge&logo=flutter&logoColor=white)  
![Dart](https://img.shields.io/badge/Dart-0175C2?style=for-the-badge&logo=dart&logoColor=white)  
![Provider](https://img.shields.io/badge/Provider-FF6F00?style=for-the-badge&logo=flutter&logoColor=white)  
![Shared Preferences](https://img.shields.io/badge/Shared_Preferences-FF4081?style=for-the-badge&logo=flutter&logoColor=white)  
![intl](https://img.shields.io/badge/intl-2196F3?style=for-the-badge&logo=flutter&logoColor=white)  
![custom_calendar](https://img.shields.io/badge/custom__calendar-00BFA5?style=for-the-badge&logo=flutter&logoColor=white)  
![flutter_linkify](https://img.shields.io/badge/flutter__linkify-673AB7?style=for-the-badge&logo=flutter&logoColor=white)  
![url_launcher](https://img.shields.io/badge/url__launcher-F44336?style=for-the-badge&logo=flutter&logoColor=white)  
![flutter_native_splash](https://img.shields.io/badge/flutter__native__splash-007AFF?style=for-the-badge&logo=flutter&logoColor=white)  

</div>

- **언어 & 프레임워크**: Flutter (>=2.10), Dart (>=2.16)  
- **상태 관리**: Provider  
- **로컬 스토리지**: shared_preferences  
- **국제화·날짜 처리**: intl  
- **캘린더 UI 컴포넌트**: custom_calendar  
- **링크 감지 및 클릭 처리**: flutter_linkify + url_launcher  
- **스플래시 스크린**: flutter_native_splash  
- **UI/로딩 위젯**:  
  - CircularProgressIndicator (loading_indicator.dart, loading_circle.dart)  
  - Hero 애니메이션 (이미지 프리뷰)  
  - Material Widgets (AppBar, Drawer, AlertDialog 등)  

---

### ⚙️ Backend (FastAPI)
<div align="center">

![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)  
![Python](https://img.shields.io/badge/Python_3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)  
![SQLAlchemy](https://img.shields.io/badge/SQLAlchemy-376E8B?style=for-the-badge&logo=sqlalchemy&logoColor=white)  
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-336791?style=for-the-badge&logo=postgresql&logoColor=white)  
![Pydantic](https://img.shields.io/badge/Pydantic-176F9C?style=for-the-badge&logo=pydantic&logoColor=white)  
![python‐jose](https://img.shields.io/badge/python--jose-000000?style=for-the-badge&logo=json-web-tokens&logoColor=white)  
![Uvicorn](https://img.shields.io/badge/Uvicorn-000000?style=for-the-badge&logo=uvicorn&logoColor=white)  

</div>

- **언어 & 프레임워크**: Python 3.10+, FastAPI  
- **비동기 ORM**: SQLAlchemy (AsyncSession)  
- **스키마·유효성 검사**: Pydantic (FastAPI 내장)  
- **DB 서버**: PostgreSQL (또는 MySQL 등 SQL 계열)  
- **웹 서버**: Uvicorn  
- **인증·인가**:  
  - JWT (발급·검증 via python‐jose)  
  - FastAPI 의존성 주입(Depends)으로 `get_current_user`  
  - 쿠키 관리 (HttpOnly, Secure, SameSite=None)  
- **예외 처리 유틸**: 커스텀 예외 클래스 (`BadRequestError`, `UnauthorizedError` 등)  
- **외부 API 연동**:  
  - TwitterClientService (트위터 세션 쿠키 기반 로그인/정보 조회)  
  - TwitterUserService (사용자 정보 조회)  
- **마이그레이션/관리**: Alembic (권장)  

---

### 🔒 보안 및 인증
<div align="center">

![JWT](https://img.shields.io/badge/JWT-000000?style=for-the-badge&logo=json-web-tokens&logoColor=white)  
![HTTPS](https://img.shields.io/badge/HTTPS-4285F4?style=for-the-badge&logo=lock&logoColor=white)  
![OAuth2](https://img.shields.io/badge/OAuth2-0C6CF2?style=for-the-badge&logo=oauth&logoColor=white)  

</div>

- **토큰 기반 인증**: JWT (python‐jose)  
- **쿠키 설정**: Secure, HttpOnly, SameSite=None 옵션  
- **API 보안**: FastAPI 의존성 주입으로 `get_current_user` 구현  
- **TLS/SSL 적용 권장** (배포 시 HTTPS)  

---

### 📦 기타 라이브러리 및 도구
- **환경 변수 관리**: python‐dotenv 또는 FastAPI `Settings` (Pydantic 기반)  
- **도커라이즈(컨테이너화)**: Docker (백엔드)  
- **CI/CD**: GitHub Actions (권장)  
- **로깅**:  
  - Python `logging` 모듈 (Backend)  
  - Flutter `debugPrint` (Frontend)  
- **이메일 전송 (Backend)**: FastAPI Email 관련 라이브러리 (추후 필요 시)  

---

## 🌳 주요 기능

<div align="center">

| 기능 | 설명 | 상태 |
|:---:|:----------------------------|:---:|
| 🔐 **회원가입·로그인 (Auth)** | 이메일/비밀번호 + 트위터 쿠키 기반 검증 → JWT 발급 및 쿠키 저장 | ![완료](https://img.shields.io/badge/완료-28a745?style=flat-square) |
| 🏠 **홈 피드** | 사용자의 오시(Oshi) 등록 상태에 따라 트윗 목록 표시 및 페이징 | ![완료](https://img.shields.io/badge/완료-28a745?style=flat-square) |
| 💖 **오시 관리** | 트위터 스크린네임 입력 → 트위터 프로필/바이오 정보 표시 및 변경/삭제 기능 | ![완료](https://img.shields.io/badge/완료-28a745?style=flat-square) |
| 📆 **일정 추출 & 등록** | 트윗 메타데이터 추출 → 제목·카테고리·시간 입력 → 일정 등록 (FastAPI ↔ DB) | ![완료](https://img.shields.io/badge/완료-28a745?style=flat-square) |
| 📱 **포스트(트윗) 상세·리플라이** | 포스트 본문 & 이미지 그리드 표시 → 리플라이 작성/전송 → 자동 생성 기능 | ![완료](https://img.shields.io/badge/완료-28a745?style=flat-square) |
| 👤 **프로필 조회** | 유저 프로필(이름·스크린네임·팔로워·바이오) 불러오기 & 표시 | ![완료](https://img.shields.io/badge/완료-28a745?style=flat-square) |
| 📸 **이미지 미리보기** | Hero 애니메이션과 InteractiveViewer를 이용한 풀스크린 사진 뷰 | ![완료](https://img.shields.io/badge/완료-28a745?style=flat-square) |
| ⚙️ **설정 (Settings)** | 다크 모드 토글 (SharedPreferences), 기타 커스텀 환경설정 | ![완료](https://img.shields.io/badge/완료-28a745?style=flat-square) |

</div>
