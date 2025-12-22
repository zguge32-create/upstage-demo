# 프로젝트 태스크: Local-First Enterprise Doc-Informer

`docs/PRD.md`를 바탕으로 MVP 구현을 위해 필요한 단계별 작업 내역입니다.

## Phase 0: 프로젝트 초기화 (Project Initialization)
- [ ] **프로젝트 구조 설정**
    - 디렉토리 생성: `data/`, `db/`, `src/`
    - 파일 생성: `requirements.txt`, `.env`, `.gitignore`
- [ ] **환경 설정**
    - 가상 환경(Virtual Environment) 생성
    - 의존성 라이브러리 설치 (`langchain`, `streamlit`, `chromadb`, `pymupdf`, `unstructured`, `sentence-transformers`, `ragas` 등)

## Phase 1: 데이터 파이프라인 (Ingest)
- [ ] **PDF 로더 구현 (`src/ingest.py`)**
    - [ ] `PyMuPDF` 또는 `Unstructured`를 사용하여 `data/` 디렉토리의 PDF 파일 로드.
    - [ ] 제목(Heading)과 본문(Body)의 구조를 유지하며 텍스트 추출.
- [ ] **청킹(Chunking) 전략 구현 (`src/ingest.py`)**
    - [ ] `RecursiveCharacterTextSplitter` 또는 의미론적(Semantic) 청킹 적용.
    - [ ] 임베딩 모델에 최적화된 크기로 청크 분할.
- [ ] **벡터 저장소 구축 (`src/ingest.py`)**
    - [ ] `HuggingFaceBgeEmbeddings` 초기화 (로컬 CPU 최적화).
    - [ ] `Chroma` 클라이언트 초기화 (데이터 저장 경로: `db/`).
    - [ ] 청크를 임베딩하여 ChromaDB에 저장.
- [ ] **검증**
    - [ ] `python src/ingest.py` 실행 후 `db/` 디렉토리에 데이터가 생성되는지 확인.

## Phase 2: 검색 시스템 (Retrieval System)
- [ ] **Retriever 로직 개발 (`src/chain.py`)**
    - [ ] `db/`에서 기존 벡터 저장소 로드.
    - [ ] `search_type="mmr"` (Max Marginal Relevance)로 Retriever 설정.
    - [ ] 파라미터 튜닝: `k` (반환 문서 수), `fetch_k` (초기 후보군 크기).
- [ ] **검색 테스트**
    - [ ] 간단한 스크립트를 작성하여 Retriever 쿼리 테스트 및 문서 반환 확인.

## Phase 3: 답변 생성 및 UI (Generation & UI)
- [ ] **LLM 연동 (`src/chain.py`)**
    - [ ] Solar API Key를 사용하여 `ChatUpstage` (또는 OpenAI 호환 래퍼) 설정.
    - [ ] 시스템 프롬프트 정의: "반드시 문맥에 기반하여 답변할 것", "페이지 번호 인용할 것".
    - [ ] RAG 체인 구축 (`RetrievalQA` 또는 LCEL `Runnable`).
- [ ] **Streamlit UI 구현 (`app.py`)**
    - [ ] `st.chat_message`를 활용한 채팅 인터페이스 디자인.
    - [ ] 파일 업로드 기능 (선택 사항, 동적 처리 시) 또는 사전 로드된 DB 사용.
    - [ ] 사용자 입력 -> RAG 체인 -> 답변 출력 연결.
    - [ ] 근거 문서(Source Documents) 표시 기능 (Expandable section).

## Phase 4: 평가 및 최적화 (Evaluation & Optimization)
- [ ] **평가 데이터셋 준비 (`data/eval_set.json`)**
    - [ ] 문서를 기반으로 5-10개의 (질문, 정답) 쌍 생성.
- [ ] **평가 스크립트 구현 (`src/eval.py`)**
    - [ ] `ragas` 라이브러리 로드.
    - [ ] RAG 파이프라인을 통해 테스트 셋에 대한 답변 생성.
    - [ ] 지표 계산: `faithfulness`(충실도), `answer_relevance`(답변 관련성).
- [ ] **성능 검토**
    - [ ] 평가 결과를 터미널 또는 보고서 파일로 출력.
    - [ ] `README.md`에 달성한 성능 지표 업데이트.
