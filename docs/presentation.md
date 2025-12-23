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
  h1 { color: #4B0082; }
  h2 { color: #333; }
  strong { color: #E91E63; }
  ul { margin-top: 0.5rem; }
  li { margin-bottom: 0.5rem; }
  code { background: #f0f0f0; padding: 0.1em 0.3em; border-radius: 4px; color: #d63384; }
---

<!-- _class: lead -->

# Local-First Enterprise Doc-Informer
## Upstage Solar API 기반 RAG 시스템

**보안(Privacy) · 효율(Efficiency) · 품질(Quality)**

---

## 1. 배경 및 문제 의식 (Background)

**기존 RAG/LLM 도입의 장벽**
- ❌ **데이터 보안**: 사내 민감 문서를 클라우드에 전체 업로드하기 부담스러움.
- ❌ **비용 문제**: 모든 문서를 거대 모델(GPT-4 등)로 처리 시 토큰 비용 급증.
- ❌ **신뢰성 부족**: LLM이 거짓 정보를 생성하는 할루시네이션(Hallucination).

---

## 2. 솔루션: Local-First RAG

**하이브리드 접근 방식**
- 🔒 **Local Vector Store**: 문서는 로컬에만 저장/검색 (보안 해결).
- 🧠 **Cloud LLM (Solar)**: 추론과 답변 생성만 API 위임 (성능 확보).
- ⚖️ **Cost Optimization**: 검색된 핵심 문맥(Context)만 전송하여 비용 절감.

---

## 3. 핵심 가치 제안 (Value Proposition)

| 가치 | 설명 | 효과 |
| :--- | :--- | :--- |
| **Data Privacy** | 문서 파싱 및 임베딩을 **로컬(On-Premise)**에서 수행 | 민감 정보 유출 원천 차단 |
| **High Performance** | **Upstage Solar**의 탁월한 한국어/영어 처리 능력 | 정확하고 자연스러운 답변 |
| **Valid Metric** | **Ragas** 프레임워크를 통한 정량적 품질 보증 | "잘 된다"는 주관적 평가 탈피 |

---

## 4. 시스템 아키텍처 (Architecture Flow)

1. **Ingest (적재)**: `PDF` ➔ `PyMuPDF`(구조화) ➔ `Chunking` ➔ `ChromaDB`
2. **Retrieval (검색)**: `Query` ➔ `Vector Search` ➔ `MMR`(다양성 확보)
3. **Generation (생성)**: `Context` + `Prompt` ➔ `Upstage Solar API` ➔ `Answer`

---

## 5. 주요 기술 스택 선정 이유

- **Upstage Solar API**: 
  - 🚀 **속도/성능**: 7B/10.7B 규모로 빠르면서도 GPT-3.5 상회 성능.
  - 🇰🇷 **언어 특화**: 한국어 문서 처리에 탁월한 강점.
- **ChromaDB**: 서버 구축 없이 파일 기반으로 동작하는 가벼운 로컬 DB.
- **Ragas**: `Faithfulness`(근거 준수율)와 `Answer Relevance`(답변 적절성) 자동 채점.

---

## 6. 상세 기능: 구조적 파싱 (Advanced Parsing)

단순 텍스트 추출의 한계를 극복.

- **기존 방식**: 문단 구분 없이 텍스트만 긁어옴 ➔ 문맥 소실.
- **본 시스템**:
  - **Heading/Body 구분**: 문서의 계층 구조 인식.
  - **Metadata 보존**: 페이지 번호, 소스 파일명 등을 함께 저장하여 "근거 제시" 가능.

---

## 7. 구현 로드맵 (Roadmap)

- **Phase 1 (Data)**: PDF 구조 분석 및 로컬 DB 구축.
- **Phase 2 (Retrieval)**: LangChain Retriever 고도화 (MMR 적용).
- **Phase 3 (App)**: Streamlit 채팅 UI 및 Solar API 연동.
- **Phase 4 (Eval)**: Ragas 파이프라인 구축 및 성능 지표 달성(0.8+).

---

## 8. 활용 사례 (Use Cases)

- **사내 규정 Q&A**: "휴가 규정이 어떻게 되나요?" (인사팀 리소스 절감)
- **법률 계약서 검토**: "이 조항의 독소 조항 위험은?" (보안 필수)
- **기술 매뉴얼 검색**: "장비 A의 유지보수 주기는?" (방대한 문서 효율적 검색)

---

<!-- _class: lead -->

# Q & A
## 감사합니다
