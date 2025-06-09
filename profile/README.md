<div align="center">

![GitHub README md 메인 이미지](https://raw.githubusercontent.com/Oshinavi/Oshinavi/main/thumbnail.jpg)

## 📌 오시나비(推しナビ)
## LLM 활용 트위터 일-한 번역 및 이벤트 일정 관리 서비스

</div>

---

## 🎯 프로젝트 소개

오시나비는 트위터 기반 정보 제공 및 소통이 이루어지는 일본의 전반적인 아티스트 업계가 갖는 언어 및 정보 장벽을 뛰어넘어, 트위터와 연동되어 문맥 기반 자동 번역을 제공하고 이벤트 일정 자동 분석 및 캘린더 등록 기능을 제공하는 서비스입니다.

<details>
<summary>💡 <b>개발 동기 및 목적</b> (클릭)</summary>

| 구분 | 내용 |
|------|------|
| **🎯 Target** | 일본의 아티스트를 응원하는 해외(한국) 팬(이벤터) |
| **❗ Problem** | 1. 정확하지 않은 트위터의 기계식 번역<br>2. 번역 및 리플라이를 위한 수동 번역기 사용의 번거로움<br>3. 텍스트 기반 이벤트 정보 제공으로 인한 이벤트 일정 정리의 번거로움 |
| **✅ Solution** | 1. LLM+RAG 기반 문맥 기반 번역 제공으로 보다 자연스러운 번역 제공<br>2. 리플라이 few-shot 제공으로 상황에 맞는 리플라이 예시 생성<br>3. 자동 이벤트 정보 추출 및 캘린더 등록<br>4. 상기 기능이 실제 트위터와 연동될 것 |

</details>

### 🌟 프로젝트 특장점

<div align="center">

```mermaid
graph TD
    A[📱 All-in-one] --> B[트위터 연동 + LLM 기반 자동 번역 제공]
    C[✨ 높은 번역 품질] --> D[RAG 컨텍스트 기반 보정]
    E[📅 자동 이벤트 정리] --> F[텍스트 분석을 통한 이벤트 정보 자동 등록]
    G[🧩 높은 확장성] --> H[LangChain을 통한 모듈화 관리]
```

</div>

---

## 🏛️ 시스템 아키텍처

<div align="center">
<img src="https://github.com/user-attachments/assets/23ef2bdd-c3df-4ba8-bc48-c4851c85cd72" alt="번역 성능 비교" width="500" />
</div>

<div align="center">

```mermaid
graph TB
    User[👤 User] --> Client[📱 Flutter Client]
    
    subgraph Docker["🐳 Docker Compose"]
        subgraph APIServer["⚙️ API Server"]
            LangChain[🤖 LangChain Pipeline]
            Crawler[🔍 Crawler]
        end
        
        subgraph CrawlerDetail["🕷️ Crawler Components"]
            Twikit[📡 Twikit]
            Selenium[🌐 Selenium]
        end
        
        subgraph RAGSystem["🧠 RAG"]
            FAISS[(🗄️ FAISS VectorDB)]
            Fugashi[🔧 Fugashi Tokenizer]
        end
        
        MySQL[(🗃️ MySQL DB)]
    end
    
    subgraph External["🌐 External Services"]
        Twitter[🐦 X/Twitter]
        LLMAPIs["🤖 LLM API"]
    end
    
    Client <--> APIServer
    Crawler --> CrawlerDetail
    Twikit <--> Twitter
    Selenium --> Twitter
    LangChain --> RAGSystem
    LangChain --> LLMAPIs
    APIServer --> MySQL
    RAGSystem --> LangChain
    
    style Client fill:#02569B,stroke:#fff,stroke-width:2px,color:#fff
    style LangChain fill:#FF6C37,stroke:#fff,stroke-width:2px,color:#fff
    style Crawler fill:#009688,stroke:#fff,stroke-width:2px,color:#fff
    style Twikit fill:#1DA1F2,stroke:#fff,stroke-width:2px,color:#fff
    style Selenium fill:#43B02A,stroke:#fff,stroke-width:2px,color:#fff
    style FAISS fill:#5243AA,stroke:#fff,stroke-width:2px,color:#fff
    style MySQL fill:#4479A1,stroke:#fff,stroke-width:2px,color:#fff
    style Twitter fill:#000,stroke:#fff,stroke-width:2px,color:#fff
    style LLMAPIs fill:#412991,stroke:#fff,stroke-width:2px,color:#fff
    style Docker fill:#2496ED,stroke:#fff,stroke-width:2px,color:#fff
```

</div>

### 🎯 시스템 구성 요소

#### 📱 Client
- **Pull-to-refresh**: 무한 스크롤 적응을 통한 데이터 갱신 접근성 향상
- **Flutter Provider 기반**: 상태관리 / 메모리 캐싱으로 데이터 지속성 유지

#### ⚙️ Server
- **Twikit + Selenium 조합**: 트위터 인증 크롤링 완전 자동화
- **LLM 처리결과와 DB 영속화**: 레이턴시·서버 부하·API 비용 절감
- **Keyset 기반 페이징**: 대용량 데이터 효율적 조회

#### 🤖 LLM Pipeline
- **각 기능을 독립된 LLMChain으로 구현**: (유지보수·기능 확장성)
- **무손실 멀티미디어 전후처리로 번역 중**: 의미왜곡 방지 및 번역 품질 향상
- **RAG 기반 프롬프팅**: 번역 정확도 향상

---

## 🔄 LLM 파이프라인 처리 흐름

<div align="center">

```mermaid
graph TD
    Input["🐦 트위터 원문 Post"]
    
    %% 4개 처리 경로 분기점
    Input --> TranslationPath["🔵 번역 경로"]
    Input --> ClassificationPath["🟡 분류 경로"] 
    Input --> SchedulePath["🟢 추출 경로"]
    Input --> ReplyPath["🔴 리플라이 생성 경로"]
    
    %% 번역 경로: 전체 파이프라인
    subgraph TranslationFlow["🔵 번역 처리 흐름"]
        PreProcess[1️⃣ Pre-Processing]
        TranslationRAG[2️⃣ RAG Context<br/>FAISS + Fugashi]
        TranslationLLM[3️⃣ Claude LLM]
        PostProcess[4️⃣ Post-Processing]
    end
    
    %% 분류 경로: RAG + LLM
    subgraph ClassificationFlow["🟡 분류 처리 흐름"]
        ClassificationRAG[2️⃣ RAG Context<br/>FAISS + Fugashi]
        ClassificationLLM[3️⃣ OpenAI LLM]
    end
    
    %% 추출 경로: LLM만
    subgraph ScheduleFlow["🟢 추출 처리 흐름"]
        ScheduleLLM[3️⃣ OpenAI LLM]
    end
    
    %% 리플라이 생성 경로: LLM만  
    subgraph ReplyFlow["🔴 리플라이 생성 처리 흐름"]
        ReplyLLM[3️⃣ Claude LLM]
    end
    
    %% Few-shot Examples
    FewShot["📝 Few-shot Examples"]
    TwitterReplies["🐦 실제 트위터 리플라이<br/>실시간 수집 데이터"]
    
    Output["💾 DB 저장 / Response"]
    
    %% 연결
    TranslationPath --> PreProcess
    PreProcess --> TranslationRAG
    TranslationRAG --> TranslationLLM
    TranslationLLM --> PostProcess
    PostProcess --> Output
    
    ClassificationPath --> ClassificationRAG
    ClassificationRAG --> ClassificationLLM
    ClassificationLLM --> Output
    
    SchedulePath --> ScheduleLLM
    ScheduleLLM --> Output
    
    ReplyPath --> ReplyLLM
    ReplyLLM --> Output
    
    %% Few-shot 연결
    FewShot -.-> TranslationLLM
    FewShot -.-> ClassificationLLM
    FewShot -.-> ScheduleLLM
    TwitterReplies -.-> ReplyLLM
    
    %% 스타일링
    style Input fill:#4CAF50,stroke:#fff,stroke-width:3px,color:#fff
    style Output fill:#4CAF50,stroke:#fff,stroke-width:3px,color:#fff
    
    style TranslationPath fill:#2196F3,stroke:#fff,stroke-width:2px,color:#fff
    style ClassificationPath fill:#FF9800,stroke:#fff,stroke-width:2px,color:#fff
    style SchedulePath fill:#4CAF50,stroke:#fff,stroke-width:2px,color:#fff
    style ReplyPath fill:#F44336,stroke:#fff,stroke-width:2px,color:#fff
    
    style TranslationFlow fill:#E3F2FD,stroke:#2196F3,stroke-width:2px
    style ClassificationFlow fill:#FFF3E0,stroke:#FF9800,stroke-width:2px
    style ScheduleFlow fill:#E8F5E8,stroke:#4CAF50,stroke-width:2px
    style ReplyFlow fill:#FFEBEE,stroke:#F44336,stroke-width:2px
    
    style FewShot fill:#9C27B0,stroke:#fff,stroke-width:2px,color:#fff
    style TwitterReplies fill:#1DA1F2,stroke:#fff,stroke-width:2px,color:#fff
    
    %% 링크 색상
    linkStyle 0 stroke:#2196F3,stroke-width:3px
    linkStyle 1 stroke:#FF9800,stroke-width:3px
    linkStyle 2 stroke:#4CAF50,stroke-width:3px
    linkStyle 3 stroke:#F44336,stroke-width:3px
    
    linkStyle 4 stroke:#2196F3,stroke-width:3px
    linkStyle 5 stroke:#2196F3,stroke-width:3px
    linkStyle 6 stroke:#2196F3,stroke-width:3px
    linkStyle 7 stroke:#2196F3,stroke-width:3px
    linkStyle 8 stroke:#2196F3,stroke-width:3px
    
    linkStyle 9 stroke:#FF9800,stroke-width:3px
    linkStyle 10 stroke:#FF9800,stroke-width:3px
    linkStyle 11 stroke:#FF9800,stroke-width:3px
    
    linkStyle 12 stroke:#4CAF50,stroke-width:3px
    linkStyle 13 stroke:#4CAF50,stroke-width:3px
    
    linkStyle 14 stroke:#F44336,stroke-width:3px
    linkStyle 15 stroke:#F44336,stroke-width:3px
```

</div>

### 🔧 LLM 파이프라인 상세 처리 과정

#### 1️⃣ Pre-Processing
- RT 접두사 및 해시태그 마스킹
- 이모지 추출 및 보존

#### 2️⃣ RAG Context Retrieval
- 일본어-한국어 고유명사/신조어 단어쌍을 FAISS VectorDB에 임베딩, RAG 컨텍스트 추출로 관련 top-k개 단어쌍 반환

#### 3️⃣ LLMChain Modules
- 각각의 번역 체인, 분류 체인, 일정 추출 체인, 리플라이 체인은 독립적, 비동기적으로 실행
- 번역, 분류, 일정 추출 체인은 사전에 정의된 Few-shot 예시를, 리플라이 체인은 해당 포스트에 포함된 실제 k개 리플라이를 제공받아 프롬프팅에 사용

#### 4️⃣ Post-Processing
- 전처리 단계에서 마스킹되었던 RT 접두사 및 해시태그 복구
- 누락된 이모지 재삽입

### 🎯 파이프라인 특징

- **🔵 번역**: 전체 4단계 파이프라인으로 문맥 기반+RAG 보정 번역 제공
- **🟡 분류**: RAG 기반 보정으로 정확한 카테고리 분류  
- **🟢 추출**: 직접 LLM 처리로 빠른 일정 정보 추출
- **🔴 리플라이**: 실제 트위터 리플라이 데이터를 Few-shot으로 활용한 자연스러운 응답 생성

### 💡 성능 향상을 위해 사용한 기법

- **문맥 기반 번역**: RAG를 통한 도메인 특화 용어 정확도 향상
- **Few-shot 프롬프팅**: 고품질 번역 예시를 통한 번역 일관성 개선
- **하이브리드 검색**: Lexical + Semantic 검색으로 최적 컨텍스트 제공
- **전후처리 파이프라인**: 해시태그, 이모지 등 멀티미디어 요소 무손실 처리

---

## 🧠 RAG 컨텍스트 추출 과정

<div align="center">

```mermaid
graph TD
    UserQuery["🔍 User Query<br/>(트위터 포스트)"]
    
    subgraph RAGProcess["🧠 RAG 컨텍스트 추출"]
        Lexical["📝 Lexical Retrieval<br/>(BM25Okapi)"]
        Semantic["🔮 Semantic Retrieval<br/>(FAISS + Embedding)"]
        
        subgraph Scoring["📊 스코어링"]
            LexicalScore["L̃ᵢ = bᵢ/max_j bⱼ"]
            SemanticScore["S̃ᵢ = dᵢ/max_j dⱼ"]
            FinalScore["Scoreᵢ = α·L̃ᵢ + β·S̃ᵢ"]
        end
        
        TopK["🎯 Top-k 결과 선택"]
    end
    
    Output["📤 최적 컨텍스트<br/>LLM 프롬프트에 삽입"]
    
    UserQuery --> Lexical
    UserQuery --> Semantic
    
    Lexical --> LexicalScore
    Semantic --> SemanticScore
    
    LexicalScore --> FinalScore
    SemanticScore --> FinalScore
    
    FinalScore --> TopK
    TopK --> Output
    
    style UserQuery fill:#4CAF50,stroke:#fff,stroke-width:2px,color:#fff
    style Output fill:#4CAF50,stroke:#fff,stroke-width:2px,color:#fff
    style Lexical fill:#FF9800,stroke:#fff,stroke-width:2px,color:#fff
    style Semantic fill:#2196F3,stroke:#fff,stroke-width:2px,color:#fff
    style FinalScore fill:#9C27B0,stroke:#fff,stroke-width:2px,color:#fff
    style TopK fill:#F44336,stroke:#fff,stroke-width:2px,color:#fff
```

</div>

### 🎯 하이브리드 검색 알고리즘

| 구분 | 기술 | 설명 |
|------|------|------|
| **📝 Lexical 검색** | **BM25Okapi** | 일본어 형태소(Fugashi) 단위로 토큰화 후, TF-IDF 기반 키워드 유사도 계산 |
| **🔮 Semantic 검색** | **FAISS + Embedding** | all-MiniLM-L6-v2로 쿼리 벡터화 후, FAISS 인덱스에서 코사인 유사도 계산 |
| **⚖️ 점수 정규화** | **Score Normalization** | 각 스코어 정규화 후, 각각에 가중치(α, β) 곱연산 후 더해 최종 점수 산출 |
| **🎯 결과 선택** | **Top-k Selection** | Top-k개 일본어-한국어 어휘쌍 컨텍스트를 반환하여 **프롬프트에 삽입** |

### 💡 핵심 특징

- **이중 검색**: 키워드 매칭과 의미적 유사성을 동시 고려
- **정밀 토큰화**: Fugashi를 통한 일본어 특화 형태소 분석
- **가중치 조절**: α, β 파라미터로 검색 방식 간 균형 조정
- **실시간 처리**: 효율적인 벡터 인덱싱으로 빠른 응답 속도 보장

---

## 📊 번역 성능 평가

### 🎯 성능 비교 결과

LLM+Few-shot prompting과 RAG를 적용하여 일본어-한국어 번역 품질이 크게 개선되었습니다.

<div align="center">

```mermaid
graph TD
    Google["🔴 Google 번역<br/><b>BLEU: 19.75</b><br/><b>ROUGE-1: 0.3011</b><br/><b>BERTScore: 0.9014</b>"]
    LLM+Few-shot["🟡 LLM+Few-shot<br/><b>BLEU: 38.81</b><br/><b>ROUGE-1: 0.4812</b><br/><b>BERTScore: 0.9531</b>"]
    LLM+Few-shot+RAG["🟢 LLM+Few-shot+RAG<br/><b>BLEU: 40.61</b><br/><b>ROUGE-1: 0.5034</b><br/><b>BERTScore: 0.9547</b>"]
    
    Google ==> LLM+Few-shot
    LLM+Few-shot ==> LLM+Few-shot+RAG
    
    style Google fill:#FFE6E6,stroke:#D32F2F,stroke-width:3px,color:#000
    style LLM+Few-shot fill:#FFF2CC,stroke:#F57F17,stroke-width:3px,color:#000
    style LLM+Few-shot+RAG fill:#E8F5E8,stroke:#388E3C,stroke-width:3px,color:#000
    
    linkStyle 0 stroke:#666,stroke-width:4px
    linkStyle 1 stroke:#666,stroke-width:4px
```

</div>

### 📈 상세 성능 지표

| 평가 모델 | BLEU | ROUGE-1 | ROUGE-2 | ROUGE-L | BERTScore F1 |
|:--------:|:-------:|:-------:|:-------:|:-------:|:------------:|
| **🔴 Google 번역** | 19.75 | 0.3011 | 0.1426 | 0.3005 | 0.9014 |
| **🟡 LLM+Few-shot** | 38.81 | 0.4812 | 0.2488 | 0.4801 | 0.9531 |
| **🟢 LLM+Few-shot+RAG 적용** | **40.61** | **0.5034** | **0.2861** | **0.5030** | **0.9547** |

### 📊 단계별 성능 개선율

| 지표 | Google → LLM+Few-shot | LLM+Few-shot → RAG적용 |
|:----:|:-----------------:|:-----------------:|
| **BLEU** | +96.5% | +4.6% |
| **ROUGE-1** | +59.8% | +4.6% |
| **ROUGE-2** | +74.5% | +15.0% |
| **ROUGE-L** | +59.8% | +4.8% |
| **BERTScore F1** | +5.7% | +0.17% |

### 🎯 핵심 성과

- **📊 ROUGE-1**: Google 번역 대비 **67.2% 향상** (0.3011 → 0.5034)
- **📊 ROUGE-2**: Google 번역 대비 **100.6% 향상** (0.1426 → 0.2861) 
- **🧠 BERTScore**: 의미적 유사도 **5.9% 향상** (0.9014 → 0.9547)
- **🎯 RAG 효과**: 특히 ROUGE-2에서 Baseline 대비 **15.0% 추가 향상**
- **⚡ 일관성**: 모든 지표에서 지속적인 성능 향상 확인

---

## 💻 기술 스택

### 🖥️ Frontend (Flutter)

<div align="center">

![Flutter](https://img.shields.io/badge/Flutter-02569B?style=for-the-badge&logo=flutter&logoColor=white) ![Dart](https://img.shields.io/badge/Dart-0175C2?style=for-the-badge&logo=dart&logoColor=white) ![Provider](https://img.shields.io/badge/Provider-FF6F00?style=for-the-badge&logo=flutter&logoColor=white) ![custom_calendar](https://img.shields.io/badge/custom__calendar-00BFA5?style=for-the-badge&logo=flutter&logoColor=white)

</div>

- **언어 & 프레임워크**: Flutter (>=2.10), Dart (>=2.16)  
- **상태 관리**: Provider  
- **캘린더 UI 컴포넌트**: custom_calendar

이 프로젝트는 [pickywawa/infinite_calendar_view](https://github.com/pickywawa/infinite_calendar_view)를 포크하여 `custom_calendar/` 폴더에서 직접 수정한 버전을 사용합니다.

**변경된 부분:**
- 날짜 선택 UI 커스터마이징
- 월 전환 애니메이션 개선

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
- **인증·인가**: JWT (발급·검증 via python-jose)  
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
  - OpenAI(`gpt-4o-mini-2024-07-18`)  
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
| 💖 **최애 관리** | 트위터 스크린네임 입력 → 트위터 프로필/바이오 정보 표시 및 변경/삭제 기능  | ![완료](https://img.shields.io/badge/완료-28a745?style=flat-square) |
| 📆 **일정 추출 & 등록** | 트윗 메타데이터 추출 → 제목·카테고리·시간 입력 → 일정 등록 (FastAPI ↔ MySQL) | ![완료](https://img.shields.io/badge/완료-28a745?style=flat-square) |
| 📱 **포스트(트윗) 상세·리플라이** | 포스트 본문 & 이미지 그리드 표시 → 리플라이 작성/전송 → 자동 생성 기능  | ![완료](https://img.shields.io/badge/완료-28a745?style=flat-square) |
| 👤 **프로필 조회** | 유저 프로필(이름·스크린네임·팔로워·바이오) 불러오기 & 표시  | ![완료](https://img.shields.io/badge/완료-28a745?style=flat-square) |
| ⚙️ **설정 (Settings)** | 다크 모드 토글 (SharedPreferences), 기타 커스텀 환경설정  | ![완료](https://img.shields.io/badge/완료-28a745?style=flat-square) |



### 📱 기능 상세 설명

<details>
<summary>🔐 <b>로그인 & 최애 등록</b> (클릭하여 펼치기)</summary>

#### 🎯 핵심 기능
- **Twikit 라이브러리를 통해 회원가입 시 입력한 본인의 트위터 계정과 연동되어 로그인**
- **등록한 최애(오시) 만의 번역된 트윗과 이벤트 정보만을 모아서 볼 수 있도록 구현**

#### 📸 기능 스크린샷

<div align="center">
<img width="200" alt="로그인 화면" src="https://github.com/user-attachments/assets/9a63e400-8a4f-4074-84e4-732a41e5abbd" style="margin: 5px;" />
<img width="200" alt="최애 등록 화면" src="https://github.com/user-attachments/assets/82f9a767-4888-4eb1-84bc-5d1b3cb3c92f" style="margin: 5px;" />
<img width="200" alt="홈 피드 화면" src="https://github.com/user-attachments/assets/a66868e5-64f5-4a6b-becd-ea965869ee53" style="margin: 5px;" />
<img width="200" alt="프로필 화면" src="https://github.com/user-attachments/assets/106d9427-63d5-4205-b215-5df56057594b" style="margin: 5px;" />
</div>

#### 💡 특징
- 트위터 계정 연동
- 개인화된 최애 아티스트 콘텐츠 큐레이션
- 안전한 JWT 기반 인증 시스템

</details>

<details>
<summary>🏠 <b>홈 피드 & 포스트</b> (클릭하여 펼치기)</summary>

#### 🎯 핵심 기능
- **Pull-to-refresh 및 무한 스크롤 적용 페이징**
- **LLM + RAG 기반 자동 번역 제공(토글로 원문 전환 가능)**
- **높은 번역 정확도 + 자연스러움**
- **LLM 수행 결과 DB 영속화**
- **Selenium을 통한 트위터 미디어 수집 및 제공**

#### 📸 기능 스크린샷

<div align="center">
<img width="200" alt="홈 피드 화면" src="https://github.com/user-attachments/assets/4e0f6bca-01da-471c-a548-31dcb2783a25" />
<img width="200" alt="번역 토글 기능" src="https://github.com/user-attachments/assets/3e8665ee-1da2-41e0-be9f-b0301e78628f" />
<img width="200" alt="미디어 표시 화면" src="https://github.com/user-attachments/assets/076a8f79-18db-40e3-ae50-e0b1d892e118" />
<img width="200" alt="무한 스크롤" src="https://github.com/user-attachments/assets/08bb0640-73ca-4104-9533-6746fc62cfb8" />
</div>

#### 💡 특징
- 직관적인 무한 스크롤 인터페이스
- 원문/번역문 실시간 전환 기능
- 트위터와 동일한 미디어 표시 경험

</details>

<details>
<summary>💬 <b>리플라이 자동 생성 & 전송</b> (클릭하여 펼치기)</summary>

#### 🎯 핵심 기능
- **페르소나 기반 팬 시선의 리플라이**
- **실제 리플라이 Few-shot 제공**
- **리플라이 전송 및 삭제**

#### 📸 기능 스크린샷

<div align="center">
<img width="200" alt="리플라이 생성 화면" src="https://github.com/user-attachments/assets/51040052-887d-4635-9d04-00dc38c98e24" />
<img width="200" alt="리플라이 전송 화면" src="https://github.com/user-attachments/assets/6f25b2b0-328b-4a90-a329-7691eb693bb1" />
<img width="200" alt="실제 트위터 전송 확인" src="https://github.com/user-attachments/assets/117f83a1-a0a1-4e10-ad55-4d44b406a570" />
<img width="200" alt="리플라이 관리" src="https://github.com/user-attachments/assets/4308420e-508a-460d-85fd-f493e38ab9d8" />
</div>

#### 💡 특징
- AI 기반 자연스러운 팬 리플라이 생성
- 실제 트위터 리플라이 데이터 학습
- 원클릭 전송 및 관리 기능
- 팬 문화에 맞는 톤앤매너 적용

</details>

<details>
<summary>📅 <b>이벤트 일정 추출 & 캘린더 등록</b> (클릭하여 펼치기)</summary>

#### 🎯 핵심 기능
- **텍스트 분석을 통한 자동 카테고라이징 & 이벤트 일정 정보 추출**
- **일정 등록 시간 획기적 단축**
- **각 아티스트별 캘린더에 일정 정보 자동 등록**
- **다른 유저와 일정 정보 공유**

#### 📸 기능 스크린샷

<div align="center">
<img width="200" alt="캘린더 등록 화면" src="https://github.com/user-attachments/assets/f84335d8-cd33-4504-a97a-00f74a9f4e8f" />
<img width="200" alt="아티스트별 캘린더" src="https://github.com/user-attachments/assets/48bd3743-5a06-4986-ae85-006e4082d1ac" />
<img width="200" alt="일정 추출 화면" src="https://github.com/user-attachments/assets/ab94817d-f634-4028-9b4e-6d2a04ab420b" />
</div>

#### 💡 특징
- 스마트한 이벤트 정보 자동 인식
- 아티스트별 개별 캘린더 관리
- 타 유저와 일정 정보 공유
- 수동 입력 대비 90% 시간 절약

</details>

<details>
<summary>👤 <b>프로필 조회 & 설정</b> (클릭하여 펼치기)</summary>

#### 🎯 핵심 기능
- **실제 트위터 프로필 및 바이오 정보 표시**
- **라이트/다크 모드 테마 제공**

#### 📸 기능 스크린샷

<div align="center">
<img width="200" alt="프로필 화면" src="https://github.com/user-attachments/assets/eecdef50-b7a2-4bf3-82d7-3b414f4e16cc" />
<img width="200" alt="설정 화면" src="https://github.com/user-attachments/assets/8c091591-dbd2-480f-89a2-51d7d590f253" />
<img width="200" alt="다크모드" src="https://github.com/user-attachments/assets/c7f58adc-035f-4eb0-bd88-b8296c60a671" />
<img width="200" alt="계정 관리" src="https://github.com/user-attachments/assets/c4c6b951-84fe-4f36-b730-61aba6f27247" />
</div>

#### 💡 특징
- 실시간 트위터 프로필 동기화
- 직관적인 테마 전환 기능
- SharedPreferences 기반 설정 저장

</details>
