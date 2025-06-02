<div align="center">

![GitHub README md 메인 이미지](https://raw.githubusercontent.com/Oshinavi/Oshinavi/main/thumbnail.jpg)

## 📌 오시나비(推しナビ)
## LLM 활용 트위터 일-한 번역 및 이벤트 일정 관리 서비스

</div>

---

## 🎯 프로젝트 소개

<details>
<summary>💡 <b>개발 동기 및 목적</b> (클릭)</summary>

| 구분 | 내용 |
|------|------|
| **🎯 Target** | 일본의 아티스트를 응원하는 해외(한국) 팬(이벤터) |
| **❗ Problem** | 1. 정확하지 않은 트위터의 기계식 번역<br>2. 번역 및 리플라이를 위한 수동 번역기 사용의 번거로움<br>3. 텍스트 기반 이벤트 정보 제공으로 인한 이벤트 일정 정리의 번거로움 |
| **✅ Solution** | 1. LLM+RAG 기반 문맥 기반 번역 제공으로 보다 자연스러운 번역 제공<br>2. 리플라이 few-shot 제공으로 상황에 맞는 리플라이 예시 생성 <br>3. 자동 이벤트 정보 추출 및 캘린더 등록 <br>4. 상기 기능이 실제 트위터와 연동될 것 |

</details>

### 🌟 프로젝트 특장점

<div align="center">

```mermaid
graph TD
    A[📱 All-in-one] --> B[트위터 연동 + LLM 기반 자동 번역 제공]
    C[✨ 높은 번역 품질] --> D[RAG 컨텍스트 기반 보정]
    E[📅 자동 이벤트 정리] --> F[텍스트 분석을 통한 이벤트 정보 자동 등록 ]
    G[🧩 높은 확장성] --> H[LangChain을 통한 모듈화 관리]
```

</div>


## 💻 기술 스택

### 🖥️ Frontend (Flutter)

<div align="center">

![Flutter](https://img.shields.io/badge/Flutter-02569B?style=for-the-badge&logo=flutter&logoColor=white) ![Dart](https://img.shields.io/badge/Dart-0175C2?style=for-the-badge&logo=dart&logoColor=white) ![Provider](https://img.shields.io/badge/Provider-FF6F00?style=for-the-badge&logo=flutter&logoColor=white) ![custom_calendar](https://img.shields.io/badge/custom__calendar-00BFA5?style=for-the-badge&logo=flutter&logoColor=white)

</div>

- **언어 & 프레임워크**: Flutter (>=2.10), Dart (>=2.16)  
- **상태 관리**: Provider  
- **캘린더 UI 컴포넌트**: custom_calendar  

---

### ⚙️ Backend (FastAPI + MySQL)

<div align="center">

![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white) ![Python 3.11+](https://img.shields.io/badge/Python_3.11+-3776AB?style=for-the-badge&logo=python&logoColor=white) ![SQLAlchemy](https://img.shields.io/badge/SQLAlchemy-376E8B?style=for-the-badge&logo=sqlalchemy&logoColor=white) ![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white) ![Pydantic](https://img.shields.io/badge/Pydantic-176F9C?style=for-the-badge&logo=pydantic&logoColor=white) ![python-jose](https://img.shields.io/badge/python--jose-000000?style=for-the-badge&logo=json-web-tokens&logoColor=white) ![Uvicorn](https://img.shields.io/badge/Uvicorn-000000?style=for-the-badge&logo=uvicorn&logoColor=white)

</div>

- **언어 & 프레임워크**: Python 3.11+, FastAPI  
- **비동기 ORM**: SQLAlchemy (AsyncSession)  
- **스키마·유효성 검사**: Pydantic (FastAPI 내장)  
- **DB 서버**: MySQL  
- **웹 서버**: Uvicorn  
- **인증·인가**:  
  - JWT (발급·검증 via python-jose)  
- **외부 API 연동**:  
  - Twikit (트윗 스크래핑 / 페이징)  
  - Selenium (t.co URL 해석 용도)  
- **마이그레이션/관리**: Alembic  

---

### 🔒 보안 및 인증

<div align="center">

![JWT](https://img.shields.io/badge/JWT-000000?style=for-the-badge&logo=json-web-tokens&logoColor=white) ![HTTPS](https://img.shields.io/badge/HTTPS-4285F4?style=for-the-badge&logo=google-chrome&logoColor=white)

</div>

- **토큰 기반 인증**: JWT (python-jose)  
- **쿠키 설정**: Secure, HttpOnly, SameSite=None 옵션  
- **API 보안**: FastAPI 의존성 주입 

---

### 🤖 LLM & RAG

<div align="center">

![LangChain](https://img.shields.io/badge/LangChain-FF6C37?style=for-the-badge&logo=langchain&logoColor=white) ![OpenAI](https://img.shields.io/badge/OpenAI-000000?style=for-the-badge&logo=openai&logoColor=white) ![Claude](https://img.shields.io/badge/Claude-3399FF?style=for-the-badge&logo=anthropic&logoColor=white) ![FAISS](https://img.shields.io/badge/FAISS-5243AA?style=for-the-badge&logo=faiss&logoColor=white) ![SentenceTransformer](https://img.shields.io/badge/SentenceTransformer-0072C6?style=for-the-badge&logo=transformer&logoColor=white) ![Fugashi](https://img.shields.io/badge/Fugashi-00B4D8?style=for-the-badge&logo=python&logoColor=white) ![BM25](https://img.shields.io/badge/BM25Okapi-3178C6?style=for-the-badge&logo=python&logoColor=white)

</div>

- **LLM 프레임워크**: LangChain  
- **벡터 검색 / 임베딩**:  
  - FAISS (semantic 검색)  
  - BM25Okapi (lexical 검색; Fugashi 일본어 형태소 분석)  
  - Sentence-Transformers(`all-MiniLM-L6-v2`)  
  - Fugashi (일본어 토크나이저)  
- **모델 API**:  
  - OpenAI(`o4-mini-2025-04-16`)  
  - Claude(`claude-3-7-sonnet`, `claude-3-5-haiku`)  
- **텍스트 전처리·후처리**:  
  - 해시태그/RT 접두사 마스킹 & 복원 (TextMasker)  
  - 이모지 추출 및 복원  
- **파이프라인 구성**:  
  - **TranslationChain**: 마스킹 → LLM 번역 → 마스킹 복원  
  - **ClassificationChain**: 카테고리·제목·상세정보 추출  
  - **ScheduleChain**: 텍스트 내 날짜/시간 추출  
  - **ReplyChain**: 자동 리플라이 생성

---

## 🌳 주요 기능

| 기능 | 설명 | 상태 |
|:------------------------------:|:-----------------------------------------------------------------:|:---:|
| 🔐 **회원가입·로그인 (Auth)** | 이메일/비밀번호 + 트위터 쿠키 기반 검증 → JWT 발급 및 쿠키 저장  | ![완료](https://img.shields.io/badge/완료-28a745?style=flat-square) |
| 🏠 **홈 피드** | 사용자의 최애(오시) 등록 상태에 따라 트윗 목록 표시 및 페이징  | ![완료](https://img.shields.io/badge/완료-28a745?style=flat-square) |
| 💖 **오시 관리** | 트위터 스크린네임 입력 → 트위터 프로필/바이오 정보 표시 및 변경/삭제 기능  | ![완료](https://img.shields.io/badge/완료-28a745?style=flat-square) |
| 📆 **일정 추출 & 등록** | 트윗 메타데이터 추출 → 제목·카테고리·시간 입력 → 일정 등록 (FastAPI ↔ MySQL) | ![완료](https://img.shields.io/badge/완료-28a745?style=flat-square) |
| 📱 **포스트(트윗) 상세·리플라이** | 포스트 본문 & 이미지 그리드 표시 → 리플라이 작성/전송 → 자동 생성 기능  | ![완료](https://img.shields.io/badge/완료-28a745?style=flat-square) |
| 👤 **프로필 조회** | 유저 프로필(이름·스크린네임·팔로워·바이오) 불러오기 & 표시  | ![완료](https://img.shields.io/badge/완료-28a745?style=flat-square) |
| ⚙️ **설정 (Settings)** | 다크 모드 토글 (SharedPreferences), 기타 커스텀 환경설정  | ![완료](https://img.shields.io/badge/완료-28a745?style=flat-square) |
