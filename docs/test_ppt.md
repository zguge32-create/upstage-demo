---
marp: true
theme: gaia
paginate: true
backgroundColor: #fff
style: |
  section {
    font-family: 'Malgun Gothic', 'Apple SD Gothic Neo', sans-serif;
    font-size: 1.5rem;
  }
  h1 { color: #2c3e50; }
  h2 { color: #34495e; }
  strong { color: #e74c3c; }
  ul { margin-top: 0.5rem; }
  li { margin-bottom: 0.5rem; }
---

<!-- _class: lead -->

# Local-First Enterprise Doc-Informer
## MVP 프로젝트 제안서

**로컬 보안과 고성능 추론의 결합**

---

## 1. 프로젝트 개요 (Overview)

**핵심 컨셉**
> 로컬 리소스(CPU/RAM)로 보안과 효율을 확보하고, 핵심 추론만 **Solar API**에 위임하는 하이브리드 아키텍처.

**주요 기능**
- **로컬 색인(Indexing)**: 비즈니스 문서의 구조적 파싱 및 저장.
- **질의 응답(QA)**: 로컬 검색 기반의 정확한 답변 생성.
- **성능 증명(Evaluation)**: 정량적 지표 기반의 품질 검증.

---

## 2. 기술 스택 (Local-Lean Stack)

로컬 환경 최적화를 위한 기술 선정.

- **UI**: `Streamlit` (로컬 웹 서버)
- **Engine**: `LangChain` / `LlamaIndex`
- **Data Parsing**: `PyMuPDF` (속도), `Unstructured` (구조)
- **Vector DB**: `ChromaDB` (로컬 파일 저장)
- **Embedding**: `HuggingFaceBge` (로컬 CPU 구동)
- **LLM**: **Upstage Solar API** (고성능 추론)

---

## 3. 핵심 구현 단계: Phase 1 & 2

**Step 1: 고성능 데이터 파싱**
- 단순 텍스트가 아닌 **문서 구조(Heading/Body)** 유지.
- 계층적 Chunking 전략 수립.

**Step 2: 로컬 벡터 스토리지**
- **ChromaDB** 활용 영구 저장.
- **MMR(Max Marginal Relevance)** 알고리즘으로 검색 다양성 확보.

---

## 4. 핵심 구현 단계: Phase 3 & 4

**Step 3: 답변 생성 (Solar API)**
- **System Prompting**: "제공된 문서에 기반하여 답변 및 근거 명시".
- 할루시네이션(Hallucination) 제어.

**Step 4: 성능 평가 (Ragas)**
- **Faithfulness** (충실도) & **Answer Relevance** (관련성) 측정.
- 정량적 데이터를 통한 성능 입증.

---

## 5. 프로젝트 구조 (Structure)

```text
local-doc-rag/
├── data/       # 테스트 문서 (Upstage 블로그 등)
├── db/         # ChromaDB 로컬 저장소
├── src/
│   ├── ingest.py  # 구조적 파싱 및 적재
│   ├── chain.py   # RAG 파이프라인
│   └── eval.py    # Ragas 평가 로직
└── app.py      # Streamlit 진입점
```

---

## 6. 차별화 전략 (Strategy)

1.  **데이터의 차별화**: Upstage 기술 블로그/가이드 문서 활용.
2.  **기술 활용 강조**: `UpstageEmbeddings`, `ChatUpstage` 적극 도입.
3.  **검증된 성능**: Solar 모델 적용 전후의 Ragas 점수 비교표 제시.

---

<!-- _class: lead -->

# 감사합니다
## Q & A
