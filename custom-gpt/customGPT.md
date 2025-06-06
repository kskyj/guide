# Customize GPT 특징 요약

## 개요
- `Custom GPT`는 OpenAI가 제공하는 GPT Builder를 통해 사용자가 자신만의 맞춤형 챗봇을 생성할 수 있도록 해주는 기능입니다.  
- 업로드한 지식(Knowledge), 지침(Instructions), 액션(Actions)을 조합해 별도 인프라 없이도 RAG, 지능형 API 연동, 개인화 설정이 가능합니다.
- Plus Edition부터 생성할 수 있으며, 공유를 통해 Free Edition 사용이 가능합니다.

## 핵심 기능
- **지식 업로드 (Knowledge)**  
  파일을 업로드하면 그 내용을 읽고 활용할 수 있습니다.  
  대화 중 임베딩 검색을 통해 관련 정보를 검색·인용하여 답변 품질을 향상시킵니다 (RAG 방식).

- **프롬프트 지침 (Instructions)**  
  시스템 메시지 수준에서 어조, 응답 스타일, 콘텐츠 유형 및 범위를 정밀 제어할 수 있습니다.

- **작업 (Actions)**  
  외부 API 호출 스키마(OpenAPI 3.1.0 기반)를 등록하여 실시간 데이터 조회, 트랜잭션 처리, 외부 시스템 업데이트 등 기능을 확장할 수 있습니다.

- **기능 (Tools)**
  챗봇에서 사용할 기능을 체크박스로 추가 할 수 있습니다.
  - 웹 검색(Web Browsing)
    - 이 기능이 활성화되면, GPT가 실시간으로 인터넷을 검색할 수 있게 돼.
    예를 들어, 최신 뉴스나 실시간 정보를 찾아서 대답할 수 있어.
    사용자가 업로드한 파일만 검색하고 읽을 거면, "웹 검색" 기능을 해제하는 게 맞다.
  - 캔버스(Canvas)
    - 이 기능은 이미지 생성 또는 시각적인 작업을 지원하는 기능이야.
    예를 들어, 사용자가 제공한 데이터를 기반으로 그래프나 차트를 만들거나, 이미지 생성을 할 때 사용될 수 있어.
    지금은 드라이브에서 파일을 읽고 데이터 분석만 하려는 목적이라면, 캔버스는 필요 없을 것 같아.
    그래픽 작업을 하지 않으면 이 기능을 꺼도 된다.
  - 4o 이미지 생성(4o Image Generation)
    - 이 기능은 이미지 생성 관련 기능이야. GPT가 텍스트를 기반으로 이미지를 생성하거나 변환할 수 있어.
    예를 들어, GPT가 제공하는 텍스트 설명을 바탕으로 AI가 이미지를 만들어낼 수 있음.
  - 코드 인터프리터 및 데이터 분석(Code Interpreter & Data Analysis)
    - 이 기능은 GPT가 코드를 실행하고 데이터 분석을 하는 데 도움을 주는 기능이야.
    예를 들어, 업로드한한 엑셀 파일을 읽어와서 그 데이터를 분석하거나, 특정 계산을 할 때 유용해.
    데이터 분석을 위해 필요한 라이브러리나 코드 실행을 할 수 있게 도와줄 거야.

- **대화 UI 커스터마이징**  
  GPT 이름, 아이콘, 예시 질문 등을 설정할 수 있습니다.

- **권한·공유 설정**  
  비공개, 링크 사용자만 공개, 전체 공개(마켓 배포) 등 다양한 배포 범위 및 액세스 권한을 관리할 수 있습니다.

---

## 제한 및 주의사항
- **파일 제한**: 최대 20개, 파일당 최대 512MB. 텍스트 및 문서 포맷 사용 권장
- **이미지/비전 파일 제한**: OCR 및 시맨틱 이해 미지원. 비텍스트 데이터는 활용 제한
- **액션 사용 주의**: 대화 흐름 중 액션이 자동 트리거될 수 있으므로 API 인증 및 권한 관리 필수

---

## RAG 기반 지식 첨부와의 관계
- Custom GPT의 **Knowledge**는 벡터 임베딩 및 검색 기능을 내장한 _RAG Lite_ 방식입니다.  
- `지식 업로드` 통해 파일만 업로드하면 별도 서버나 백엔드 없이 즉시 사용할 수 있습니다.

---

## ✅ 할 수 있는 것
- 특정 목적에 맞춘 대화 스타일 만들기 (예: 변호사, 트레이너, 작가 등)
- 외부 API 연결을 통해 실시간 데이터 가져오기
- 업로드한 파일 기반으로 답변하기 (메뉴얼, 논문 등)
- 특정 행동을 강제하거나 대화 응답 형식을 고정하기 (예: 항상 요약 형태로 답변)
- OpenAI 기본 툴(Web Browsing, Python 실행 등)과 조합하여 복합적 작업 수행

---

## ❌ 할 수 없는 것
- **모델 자체 학습(Fine-tuning)**  
  → 새로운 데이터로 모델 자체를 재훈련하는 것은 불가합니다.

- **모델 구조 변경**  
  → GPT의 파라미터 수, 레이어 수 등 모델 아키텍처 자체를 변경하는 것은 불가합니다.

- **대규모 실시간 크롤링**  
  → 웹 브라우징 기능은 제공되지만 대규모/지속적 데이터 수집에는 제한이 있습니다.

---

## 요약
Custom GPT는 파일 기반 RAG, 지침 설정, 외부 API 액션을 결합해 빠르게 맞춤형 챗봇을 제작할 수 있는 도구입니다.  
보다 대규모이거나 정밀한 요구사항이 필요한 경우, 외부 벡터 DB 및 오케스트레이션 시스템을 포함한 별도 RAG 파이프라인 구축을 고려해야 합니다.

---

*참고: [Knowledge in GPTs – OpenAI Help Center](https://help.openai.com/en/articles/8843948-knowledge-in-gpts)*
