# Product Requirements Document (PRD): Local-First Enterprise Doc-Informer

## 1. 프로젝트 개요 (Project Overview)
**프로젝트명:** Local-First Enterprise Doc-Informer  
**목적:** 로컬 환경의 리소스(CPU/RAM)를 효율적으로 활용하여 보안성과 속도를 챙기면서, 핵심 추론 능력은 Upstage Solar API를 통해 극대화하는 RAG(Retrieval-Augmented Generation) 시스템 구축.  
**핵심 가치:**  
- **Data Privacy:** 민감한 문서 처리를 로컬에서 수행.
- **Cost Efficiency:** 고비용 추론 작업만 API로 위임.
- **Quality Assurance:** 정량적 지표 기반의 성능 평가(Evaluation) 프로세스 내재화.

---

## 2. 주요 목표 (Goals & Objectives)
1.  **구조적 데이터 파싱:** 단순 텍스트 추출을 넘어, 문서의 제목과 본문 등 계층 구조를 유지하며 데이터를 처리한다.
2.  **로컬 벡터 검색 최적화:** 로컬 ChromaDB와 MMR(Max Marginal Relevance) 알고리즘을 활용해 검색 정확도와 답변의 다양성을 확보한다.
3.  **고성능 답변 생성:** Upstage Solar API를 활용하여 할루시네이션을 최소화하고 정확한 근거를 제시하는 답변을 생성한다.
4.  **정량적 성능 평가:** Ragas 프레임워크를 통해 모델의 충실도(Faithfulness)와 답변 관련성(Answer Relevance)을 수치화하여 증명한다.

---

## 3. 기술 스택 (Tech Stack)

### 3.1 Frontend / Application
-   **Streamlit:** 빠른 프로토타이핑 및 로컬 웹 서버 구동.

### 3.2 Core Engine / Framework
-   **Orchestration:** LangChain 또는 LlamaIndex.
-   **LLM (Generation):** Upstage Solar API (메인), Ollama (로컬 테스트 옵션).

### 3.3 Data Pipeline & Retrieval
-   **Parsing:** PyMuPDF (속도 중심), Unstructured (구조적 파싱).
-   **Embedding Model:** HuggingFaceBgeEmbeddings (로컬 CPU 구동 가능한 경량 모델).
-   **Vector DB:** ChromaDB (로컬 파일 시스템 기반 저장).

### 3.4 Evaluation
-   **Library:** Ragas (Retrieval Augmented Generation Assessment).

---

## 4. 기능 요구사항 (Functional Requirements)

### 4.1 데이터 파싱 및 적재 (Data Ingestion)
-   **FR-01:** 사용자는 PDF 문서를 시스템에 업로드하거나 지정된 폴더에 위치시킬 수 있어야 한다.
-   **FR-02:** 시스템은 PDF 내의 제목(Heading)과 본문(Body)을 구조적으로 구분하여 파싱(Chunking)해야 한다.
-   **FR-03:** 파싱된 데이터는 임베딩되어 로컬 ChromaDB에 영구 저장되어야 한다.

### 4.2 검색 (Retrieval)
-   **FR-04:** 사용자의 질문에 대해 벡터 유사도 검색(Similarity Search)을 수행해야 한다.
-   **FR-05:** MMR(Max Marginal Relevance) 기법을 적용하여 중복된 정보보다는 다양한 관점의 문서를 검색 결과로 반환해야 한다.

### 4.3 답변 생성 (Generation)
-   **FR-06:** 검색된 문서를 문맥(Context)으로 하여 Upstage Solar API에 질의를 전송해야 한다.
-   **FR-07:** 시스템 프롬프트를 통해 "제공된 문서에 기반해서만 답변할 것"과 "답변의 근거 페이지를 명시할 것"을 강제해야 한다.

### 4.4 성능 평가 (Evaluation)
-   **FR-08:** 미리 정의된 5~10개의 골든 데이터셋(질문-답변 쌍)에 대해 자동으로 평가를 수행할 수 있어야 한다.
-   **FR-09:** 평가 결과로 **Faithfulness(충실도)**와 **Answer Relevance(관련성)** 점수를 계산하여 사용자(터미널 또는 UI)에게 표시해야 한다.

---

## 5. 구현 로드맵 (Implementation Roadmap)

### Phase 1: Data Pipeline (Ingest)
-   PDF 문서 구조 분석 및 Chunking 전략 수립.
-   로컬 임베딩 모델 선정 및 ChromaDB 적재 구현.

### Phase 2: Retrieval System
-   LangChain을 활용한 Retriever 구성.
-   MMR 등 검색 최적화 알고리즘 적용 및 테스트.

### Phase 3: Generation & UI
-   Upstage Solar API 연동.
-   Streamlit UI 구성 (채팅 인터페이스).
-   프롬프트 엔지니어링 (System Prompting).

### Phase 4: Evaluation & Optimization
-   Ragas 기반 평가 스크립트 작성.
-   Solar 모델 적용 전후 성능 비교 데이터 확보.
-   README 및 문서 정리 (성능 지표 포함).

---

## 6. 프로젝트 구조 (Project Structure)

```text
local-doc-rag/
├── data/                # 테스트 데이터 (Upstage 기술 블로그, Solar 가이드 등)
├── db/                  # ChromaDB 데이터 저장소
├── src/
│   ├── ingest.py        # 데이터 파싱 및 임베딩 적재
│   ├── chain.py         # RAG 파이프라인 및 LLM 호출 로직
│   └── eval.py          # Ragas 평가 로직
├── app.py               # Streamlit 애플리케이션 진입점
├── .env                 # API Key 등 환경 변수
└── requirements.txt     # 의존성 패키지 목록
```

---

## 7. 성공 지표 (Success Metrics)
1.  **Ragas 점수:** Faithfulness, Answer Relevance 점수 평균 0.8 이상 목표.
2.  **처리 속도:** 로컬 환경(CPU)에서도 검색 및 답변 생성이 사용자 경험을 저해하지 않는 수준(수 초 이내).
3.  **구조적 정확성:** 문서의 표나 헤더 정보를 문맥에 맞게 정확히 참조하였는가.
