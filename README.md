# 데이터 거버넌스 분석

- [데이터 거버넌스 분석](#데이터-거버넌스-분석)
  - [데이터 거버넌스에서 제공하는 기능](#데이터-거버넌스에서-제공하는-기능)
  - [데이터 거버넌스와 Datahub 비교](#데이터-거버넌스와-datahub-비교)
  - [Datahub](#datahub)
    - [주요 기능](#주요-기능)
    - [오픈소스 버전](#오픈소스-버전)
    - [데이터 거버넌스 기능](#데이터-거버넌스-기능)
    - [거버넌스 관점에서 부족한 기능](#거버넌스-관점에서-부족한-기능)
    - [주요 구성요소](#주요-구성요소)
    - [실행 환경](#실행-환경)
  - [Flowable BPM](#flowable-bpm)
    - [주요 기능](#주요-기능-1)
    - [주요 구성요소](#주요-구성요소-1)
    - [실행 환경](#실행-환경-1)
  - [Datahub + Flowable](#datahub--flowable)
    - [통합시 고려사항](#통합시-고려사항)

## 데이터 거버넌스에서 제공하는 기능

| 구분                                                 | 주요 기능                  | 세부 설명                                                                   |
| ---------------------------------------------------- | -------------------------- | --------------------------------------------------------------------------- |
| **1. 메타데이터 관리 (Metadata Management)**         | 메타데이터 수집(Ingestion) | 다양한 소스(DB, Data Lake, BI, API 등)로부터 메타데이터 자동 수집 및 동기화 |
|                                                      | 메타데이터 모델링          | 엔터프라이즈 데이터 모델 정의, Entity/Relation/Schema 구조 관리             |
|                                                      | 메타데이터 버전 관리       | 메타데이터 변경 이력 추적, 이전 버전 복원                                   |
|                                                      | 데이터 프로파일링          | 데이터 품질, 분포, NULL 비율 등 통계 기반 데이터 분석                       |
| **2. 데이터 카탈로그 (Data Catalog)**                | 검색 및 탐색               | 비즈니스/기술 메타데이터 기반 검색, 태그/분류 기반 탐색                     |
|                                                      | 분류체계(Taxonomy) 관리    | 조직별, 도메인별, 주제별 데이터 분류 구조 정의                              |
|                                                      | 사용자 협업 기능           | 데이터 설명(Comment), 평점(Rating), 추천(Endorsement) 기능                  |
| **3. 데이터 품질 관리 (Data Quality Management)**    | 품질 규칙 정의             | 데이터 품질 기준 정의 (정합성, 완전성, 정확성 등)                           |
|                                                      | 품질 모니터링              | 품질 점검 자동화, 오류 감지 및 경보                                         |
|                                                      | 품질 개선 이력             | 품질 이슈 식별, 개선 조치 및 검증 이력 관리                                 |
| **4. 데이터 계보 (Data Lineage)**                    | 컬럼 수준(Lineage) 추적    | 컬럼 단위로 데이터 흐름(ETL → DB → BI 등)을 시각적으로 표시                 |
|                                                      | 변화 영향도 분석           | 스키마 변경 시 영향받는 다운스트림 시스템 분석                              |
|                                                      | 계보 시각화                | 그래프 기반의 데이터 흐름 시각화 UI 제공                                    |
| **5. 보안 및 접근 제어 (Security & Access Control)** | 데이터 분류 및 등급 관리   | 중요도, 기밀성, 규제 수준에 따른 보안 등급 지정                             |
|                                                      | 접근권한 관리              | 사용자/그룹 단위의 접근 정책 및 권한 승인 프로세스                          |
|                                                      | 민감정보 탐지/마스킹       | PII, 금융정보 등 민감 데이터 자동 탐지 및 마스킹 정책 적용                  |
| **6. 정책 및 표준 관리 (Policy & Standards)**        | 데이터 정책 관리           | 데이터 관리 규정, 표준, 가이드라인 문서화 및 배포                           |
|                                                      | 규정 준수(Compliance)      | GDPR, ISO 27001, 개인정보보호법 등 컴플라이언스 관리                        |
|                                                      | 정책-데이터 연계           | 정책별 적용 데이터셋 및 관리 주체 매핑                                      |
| **7. 거버넌스 워크플로우 (Governance Workflow)**     | 승인 및 결재 프로세스      | 메타데이터 변경, 보안 등급 조정 시 승인 흐름 구성                           |
|                                                      | 역할 기반 작업 배정        | Steward, Owner, Custodian 등 역할별 책임 관리                               |
|                                                      | BPM 연계                   | Flowable, Camunda 등 BPM 엔진 연동 통한 자동화된 승인/알림 프로세스         |
| **8. 감사 및 모니터링 (Audit & Monitoring)**         | 변경 이력 감사             | 메타데이터, 정책, 권한 변경 이력 추적                                       |
|                                                      | 접근 로그 관리             | 사용자별 접근 내역 및 행위 기록                                             |
|                                                      | 대시보드                   | 품질지표, 정책준수율, 변경추이 등 KPI 시각화                                |
| **9. 통합 및 연계 (Integration & API)**              | 외부 연동                  | ETL, BI, Data Warehouse, MDM 등과의 통합                                    |
|                                                      | API 제공                   | REST/GraphQL API를 통한 메타데이터 접근 및 관리                             |
|                                                      | 이벤트 기반 연동           | Kafka, Webhook, gRPC 등 실시간 메타데이터 변경 이벤트 전달                  |

## 데이터 거버넌스와 Datahub 비교

| 기능 범주                   | 기능명                                  | DataHub에서의 상태 | 설명                                                                                                                                                                                           |
| --------------------------- | --------------------------------------- | ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 메타데이터 관리             | 메타데이터 수집(Ingestion)              | ✅ 지원됨           | 다양한 커넥터로 메타데이터 수집 가능. ([docs.datahub.com][1])                                                                                                                                  |
|                             | 메타데이터 모델링/확장성                | ✅ 지원됨           | 유연한 메타데이터 모델 제공. ([scalefree.com][2])                                                                                                                                              |
|                             | 메타데이터 버전관리                     | ❌ 제한됨           | ‘버전관리’ 기능이 거버넌스 솔루션 수준에서 통합되어 제공된다는 언급은 제한적임.                                                                                                                |
| 데이터 카탈로그 / 검색 탐색 | 검색 및 탐색 기능                       | ✅ 지원됨           | 데이터 자산 탐색, 검색 UI/API 제공. ([docs.datahub.com][3])                                                                                                                                    |
|                             | 분류체계 / 비즈니스 용어 사전(Glossary) | ✅ 지원됨           | “Business Glossary” 등을 위한 커넥터 존재. ([docs.datahub.com][1])                                                                                                                             |
|                             | 사용자 협업 (주석, 평점 등)             | ❌ 제한됨           | 주석(Comment)이나 평점(Rating) 등 비즈니스 사용자 중심 기능이 기본 제공이라는 문서는 많지 않음.                                                                                                |
| 데이터 품질 관리            | 품질 규칙 정의 & 모니터링               | ⚠ 부분지원됨       | “metadata tests, assertions, data freshness checks” 등이 문서화되어 있음. ([docs.datahub.com][3]) 다만, 완전한 품질 엔진 수준의 기능(예: 자동 결함 탐지→개선 워크플로우)은 별도 구현이 필요함. |
| 데이터 계보(Lineage)        | 데이터 흐름/계보 추적                   | ⚠ 부분지원됨       | 테이블 수준 및 일부 컬럼 수준 계보 지원됨. ([Atlan][4]) 하지만 지원되는 소스가 제한적이고 일부 기능은 수동 또는 커스텀 구현이 필요함.                                                          |
| 보안 및 접근제어            | 데이터 등급ㆍ민감도 분류                | ⚠ 제한됨           | 메타데이터 상에서 기밀성·PII 태그 지정 가능성이 있으나, 자동 민감정보 탐지·마스킹 기능이 기본 제공이라는 언급은 많지 않음.                                                                     |
|                             | 접근권한 관리(RBAC 등)                  | ✅ 지원됨           | Access Management 기능 제공됨. ([datahub][5])                                                                                                                                                  |
| 정책 및 표준 관리           | 정책 문서화 및 연계                     | ❌ 제한됨           | 거버넌스 정책–데이터 연계, 워크플로우 기반 승인 등은 별도 구성이나 확장이 필요함.                                                                                                              |
| 거버넌스 워크플로우         | 승인/결재 프로세스, 역할 기반 작업 배정 | ❌ 제한됨           | 워크플로우 엔진 내장이라기보다는 API/SDK를 이용하여 구성해야 하는 측면이 강함.                                                                                                                 |
| 감사 및 모니터링            | 변경이력 감사, 접근로그, KPI 대시보드   | ⚠ 일부지원됨       | 메타데이터 변경 이력이나 로그는 일부 가능하나, 통합 거버넌스 KPI 대시보드 등은 기본 패키지에서 완전하게 제공된다고 보기 어려움.                                                                |
| 통합 및 연계                | 외부 시스템 연동(API/SDK)               | ✅ 지원됨           | API/SDK 제공, 다양한 커넥터 존재. ([docs.datahub.com][3])                                                                                                                                      |
|                             | 이벤트 기반 연계(Streaming)             | ⚠ 제한됨           | “Active metadata / streaming” 개념이 제시되어 있으나, 완전 자동화된 이벤트 기반 연계가 모든 소스에 준비되어 있지는 않음. ([Medium][6])                                                         |

[1]: https://docs.datahub.com/integrations?utm_source=chatgpt.com "The #1 Open Source Metadata Platform - DataHub"
[2]: https://www.scalefree.com/blog/data-warehouse/mastering-metadata-data-catalogs-in-data-warehousing-with-datahub/?utm_source=chatgpt.com "Data Catalogs in Data Warehousing with Datahub - Scalefree"
[3]: https://docs.datahub.com/docs/features?utm_source=chatgpt.com "What is DataHub? | DataHub"
[4]: https://atlan.com/know/data-catalog/datahub/column-level-lineage/?utm_source=chatgpt.com "DataHub Data Lineage: Features, Supported Sources & More - Atlan"
[5]: https://datahubproject.io/docs/features/feature-guides/access-management?utm_source=chatgpt.com "Access Management - DataHub"
[6]: https://medium.com/datahub-project/5-features-to-look-out-for-in-a-modern-data-catalog-31dfa4d32957?utm_source=chatgpt.com "5 Features to Look Out for in a Modern Data Catalog | DataHub"

## Datahub

### 주요 기능

| 기능 영역                              | 기능명                                     | 설명                                                                                                          | 비고 / 유의사항                                                                                             |
| -------------------------------------- | ------------------------------------------ | ------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| 데이터 탐색 & 발견 (Discovery)         | 검색(Search)                               | 전체 데이터 자산(데이터셋, 대시보드, ML 모델 등)을 이름·설명·속성 기반으로 검색 가능. ([docs.datahub.com][1]) | 대용량 환경에서도 색인 구조(Elasticsearch 등)를 활용한 빠른 검색이 특징. ([Medium][2])                      |
|                                        | 탐색(Browse)                               | 계층적 탐색 지원: 예컨대 데이터베이스 → 스키마 → 테이블 순으로 탐색 가능. ([GitHub][3])                       | UI 업데이트가 이루어진 버전(v1.0)에서 개선됨. ([GitHub][3])                                                 |
| 메타데이터 수집 (Ingestion)            | 자동／수동 메타데이터 수집                 | 다양한 데이터 소스(connector)를 통해 메타데이터를 수집하거나 수동 등록 가능. ([docs.datahub.com][4])          | 수집 대상 및 방식은 확장 가능한 아키텍처로 설계됨. ([Medium][2])                                            |
|                                        | UI 기반 수집(ingestion)                    | UI 통해 간단히 수집 설정 가능. ([docs.datahub.com][1])                                                        | 사용자가 기술적으로 설치하거나 개발하지 않고도 메타데이터 수집 설정 가능.                                   |
| 데이터 계보(데이터 라인리지) (Lineage) | 테이블/컬럼 수준 라인리지 표시             | 데이터 흐름(어디서 왔고 어디로 가는지) 시각화 가능. ([Atlan][5])                                              | 일부 데이터 소스에서는 컬럼 수준 지원이 제한적일 수 있음. ([Atlan][5])                                      |
|                                        | 영향 분석(Impact Analysis)                 | 특정 자산이 변경되었을 때 downstream/upstream에 미치는 영향 분석 가능. ([Medium][6])                          | 복잡한 파이프라인에서는 설정/수집이 추가로 필요할 수 있음.                                                  |
| 데이터 거버넌스(Governance)            | 소유자 및 책임자 지정(Ownership)           | 각 자산에 데이터 소유자 또는 책임자(owner)를 지정할 수 있어 관리 체계 강화됨. ([docs.datahub.com][1])         | 조직 내부 역할과 연계해 사용하면 효과적입니다.                                                              |
|                                        | 민감 데이터 식별 및 분류(Sensitivity/PII)  | 민감 데이터(PII 등)에 태그/분류를 지정할 수 있음. ([docs.datahub.com][1])                                     | 자동 스캐닝 기능은 별도 구현이 필요할 수 있음.                                                              |
|                                        | 태그 및 용어사전(Business glossary & Tags) | 비즈니스 용어 정의, 데이터 자산에 태그를 붙여 맥락 부여 가능. ([docs.datahub.com][4])                         | 조직 맞춤 용어사전을 구축하면 검색/이해도가 향상됨.                                                         |
| 데이터 품질(Data Quality)              | 메타데이터 기반 품질 지표                  | 메타데이터 정보(예: 최근 수집일, 통계 등)를 통해 품질 인사이트 제공. ([GitHub][3])                            | 완전한 데이터 품질 체크(assertions 등)는 오픈소스 버전에서 제한적일 수 있음. ([forum.datahubproject.io][7]) |
| API & SDK                              | 프로그래밍 방식 접근                       | 다양한 언어/SDK/API를 통해 메타데이터 조회·등록 가능. ([docs.datahub.com][1])                                 | 자동화나 커스텀 처리에 매우 유용합니다.                                                                     |
| 확장성(Extensibility)                  | 커넥터 및 플러그인 확장                    | 새로운 데이터 소스나 커스텀 메타데이터 모델을 추가할 수 있는 구조. ([Medium][2])                              | 조직 특성에 맞춰 메타데이터 모델을 설계할 수 있음.                                                          |
| 스케일／성능(Scalability)              | 대규모 메타데이터 처리                     | 수천 개 이상의 데이터셋, 복잡한 파이프라인 처리에 적합. ([scalefree.com][8])                                  | 인프라(Elasticsearch, Kafka 등) 구성 고려 필요. ([Reddit][9])                                               |
| 사용자 인터페이스(UI)                  | 사용자 친화적 UI 제공                      | 최신 버전에서 탐색, 라인리지 시각화, 필터 기능 등이 개선됨. ([GitHub][3])                                     | 사용성 향상을 위해 UI 버전이나 설정 확인 필요.                                                              |

[1]: https://docs.datahub.com/docs/features?utm_source=chatgpt.com "What is DataHub? | DataHub"
[2]: https://medium.com/%40wajahatullah.k/why-datahub-could-be-your-go-to-option-for-open-source-data-lineage-d57632989b58?utm_source=chatgpt.com "Why DataHub Could Be Your Go-To Option for Open-Source Data ..."
[3]: https://github.com/datahub-project/datahub/releases?utm_source=chatgpt.com "Releases · datahub-project/datahub - GitHub"
[4]: https://docs.datahub.com/integrations?utm_source=chatgpt.com "The #1 Open Source Metadata Platform - DataHub"
[5]: https://atlan.com/know/data-catalog/datahub/column-level-lineage/?utm_source=chatgpt.com "DataHub Data Lineage: Features, Supported Sources & More - Atlan"
[6]: https://medium.com/datahub-project/whats-next-for-datahub-519f5aee471c?utm_source=chatgpt.com "What's Next for DataHub?. A Sneak Peek into the 2025 Roadmap…"
[7]: https://forum.datahubproject.io/t/availability-of-assertions-feature-in-datahub-managed-vs-open-source-version/1264?utm_source=chatgpt.com "Availability of Assertions Feature in DataHub: Managed vs. Open ..."
[8]: https://www.scalefree.com/blog/data-warehouse/mastering-metadata-data-catalogs-in-data-warehousing-with-datahub/?utm_source=chatgpt.com "Data Catalogs in Data Warehousing with Datahub - Scalefree"
[9]: https://www.reddit.com/r/dataengineering/comments/yxrh9y/metadata_store_which_one_to_choose_openmetadata/?utm_source=chatgpt.com "Metadata Store - Which one to Choose ? OpenMetadata vs Datahub"

### 오픈소스 버전

| 기능 영역                       | 기능명                                             | 설명                                                                                                                            |
| ------------------------------- | -------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| **데이터 탐색 & 발견**          | 검색 (Search)                                      | 데이터셋, 테이블, 뷰, 대시보드, ML 모델 등 다양한 자산을 이름·설명·메타데이터 기반으로 검색 가능. ([docs.datahub.com][1])       |
|                                 | 탐색/브라우징 (Browse)                             | 계층 구조(예: 데이터베이스 → 스키마 → 테이블)나 태그·분류 기반으로 탐색 가능. ([Medium][2])                                     |
| **메타데이터 수집 (Ingestion)** | 커넥터 기반 메타데이터 수집                        | 다양한 데이터 소스(관계형 DB, 클라우드 데이터웨어하우스, 파일, 스토리지 등)로부터 메타데이터 수집 가능. ([docs.datahub.com][3]) |
|                                 | UI/CLI 기반 수집 설정                              | UI 또는 CLI를 통해 메타데이터 수집 파이프라인을 설정할 수 있음. ([docs.datahub.com][1])                                         |
| **메타데이터 모델링 & 관리**    | 확장 가능한 메타데이터 모델                        | 커스텀 자산 유형(custom entity types), 새로운 속성, 태그 등을 추가 가능. ([Medium][2])                                          |
| **데이터 계보 (Lineage)**       | 테이블/뷰/컬럼 수준 라인리지 표시                  | 자산 간 상하관계(lineage)를 시각화하고 추적할 수 있음. ([Atlan][4])                                                             |
| **데이터 거버넌스 & 관리**      | 소유자 & 책임자 지정(Ownership)                    | 각 자산에 소유자(owner)·책임자(responsible party) 등의 메타데이터를 지정 가능. ([Atlan][5])                                     |
|                                 | 태그/분류(Taxonomy) 및 용어사전(Business Glossary) | 데이터 자산에 태그를 붙이거나 비즈니스 용어사전을 구축해 맥락을 부여 가능. ([docs.datahub.com][3])                              |
|                                 | 접근 제어(Authorization / Policies)                | 메타데이터 플랫폼 수준에서 자산 접근 정책을 설정할 수 있는 기능 제공. ([datahub][6])                                            |
| **API & SDK**                   | 메타데이터 조회/등록 API 및 SDK                    | REST/GraphQL API 또는 SDK를 통해 메타데이터를 프로그램 방식으로 조회·등록 가능. ([docs.datahub.com][1])                         |
| **확장성 & 연계**               | 커넥터/플러그인 확장 구조                          | 새로운 데이터소스 커넥터를 추가하거나 사용자 정의 로직을 구현 가능. ([Medium][2])                                               |
|                                 | 대규모 환경 적용 가능성                            | Elasticsearch 기반 색인, Kafka 기반 메타데이터 스트리밍 등을 통한 확장성 지원. ([Medium][2])                                    |
| **UI/UX**                       | 웹 UI 제공                                         | 사용자 인터페이스로 자산 검색, 탐색, 메타데이터 조회, 라인리지 시각화 등을 제공. ([GitHub][7])                                  |

[1]: https://docs.datahub.com/docs/features?utm_source=chatgpt.com "What is DataHub? | DataHub"
[2]: https://medium.com/%40wajahatullah.k/why-datahub-could-be-your-go-to-option-for-open-source-data-lineage-d57632989b58?utm_source=chatgpt.com "Why DataHub Could Be Your Go-To Option for Open-Source Data ..."
[3]: https://docs.datahub.com/integrations?utm_source=chatgpt.com "The #1 Open Source Metadata Platform - DataHub"
[4]: https://atlan.com/know/data-catalog/datahub/column-level-lineage/?utm_source=chatgpt.com "DataHub Data Lineage: Features, Supported Sources & More - Atlan"
[5]: https://atlan.com/linkedin-datahub-metadata-management-open-source/?utm_source=chatgpt.com "LinkedIn DataHub Guide (2025): Setup, and Alternatives - Atlan"
[6]: https://datahubproject.io/docs/authorization/policies/?utm_source=chatgpt.com "Policies Guide - DataHub"
[7]: https://github.com/datahub-project/datahub?utm_source=chatgpt.com "The Metadata Platform for your Data and AI Stack - GitHub"

### 데이터 거버넌스 기능

| 기능                                                         | 설명                                                                                                                                            |
| ------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| 소유자/책임자 지정(Ownership)                                | 자산(데이터셋, 테이블 등)에 소유자(owner)나 책임자(responsible party)를 메타데이터로 지정할 수 있음. ([docs.datahub.com][1])                    |
| 태그/비즈니스 용어사전(Business Glossary) 및 도메인(Domains) | 자산에 태그(tag)를 붙이거나 용어사전(glossary) 항목을 관리하고, 도메인(Domain) 개념을 통해 조직 맥락을 부여할 수 있음. ([Atlan][2])             |
| 접근 관리(Access Management) / 역할 기반 제어(RBAC)          | 외부 역할(external roles) 또는 사용자 역할(user roles)을 자산에 매핑하고, 기본적인 접근 제어를 설정할 수 있는 기능이 있음. ([datahub][3])       |
| 데이터 라인리지(Lineage) 및 영향 분석(Impact Analysis)       | 데이터 흐름을 파악하기 위해 테이블 수준 또는 일부 컬럼 수준 라인리지를 지원하며, 상하관계(upstream/downstream)를 시각화할 수 있음. ([Atlan][4]) |
| 메타데이터 검색/탐색 및 거버넌스 맥락 제공                   | 거버넌스 활동을 지원하기 위한 메타데이터 검색, 탐색, 탐색된 자산에 대한 문맥 제공 기능 등이 있음. ([Medium][5])                                 |

[1]: https://docs.datahub.com/docs/features?utm_source=chatgpt.com "What is DataHub? | DataHub"
[2]: https://atlan.com/openmetadata-vs-datahub/?utm_source=chatgpt.com "OpenMetadata vs. DataHub: Which One to Choose in 2025? - Atlan"
[3]: https://datahubproject.io/docs/features/feature-guides/access-management?utm_source=chatgpt.com "Access Management - DataHub"
[4]: https://atlan.com/know/data-catalog/datahub/column-level-lineage/?utm_source=chatgpt.com "DataHub Data Lineage: Features, Supported Sources & More - Atlan"
[5]: https://medium.com/%40wajahatullah.k/why-datahub-could-be-your-go-to-option-for-open-source-data-lineage-d57632989b58?utm_source=chatgpt.com "Why DataHub Could Be Your Go-To Option for Open-Source Data ..."

### 거버넌스 관점에서 부족한 기능

| 거버넌스 관점                                  | 부족하거나 제한적인 기능                                                                                                               | 설명                                                                                                                                                                                                                                                                                                                                                                                  |
| ---------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **정책/표준의 실행·자동화**                    | 거버넌스 정책의 자동 강제(enforcement)·워크플로우(workflow)·승인(workflow) 기능이 제한적                                               | 예컨대 변경 요청(change requests), 자산 인증(asset certification), 준수(compliance) 폼 및 자동 알림 등의 거버넌스 운영 기능이 DataHub OSS에는 기본적으로 제공되지 않습니다. 공식 문서에서 “Enterprise Governance Mechanisms with dynamic compliance forms, certification & approval workflows”는 **DataHub Cloud(관리형 제품)**에만 제공된다고 명시하고 있음. ([docs.datahub.com][1]) |
| **속성 기반/도메인 기반 세부 권한 제어**       | Attribute-based access control (ABAC) 또는 도메인/속성 단위 권한 세분화 기능이 비교적 약함                                             | DataHub Cloud에서는 “Enterprise RBAC support with additional permissions for declaring domain- or attribute-scoped personas” 같은 기능이 포함되어 있음. OSS 버전에서는 기본 RBAC만 지원되는 수준으로 보임. ([docs.datahub.com][1])                                                                                                                                                    |
| **감사 로그(Audit Log) 및 거버넌스 리포팅**    | 변경 이력 추적, 감사 로그, 거버넌스 지표 대시보드 등이 기본 기능으로 강하게 제공되지는 않음                                            | 관리형 제품에서는 “Shared audit logs” 같은 기업용 기능이 제공됨. ([docs.datahub.com][1])                                                                                                                                                                                                                                                                                              |
| **데이터 품질 및 준수 통합**                   | 거버넌스와 연계된 데이터 품질 모니터링, 이상 탐지, 자동화된 검증(assertions) 기능이 제한적                                             | DataHub Cloud에서는 품질 모니터링, AI 이상 탐지 등의 기능이 “observability + governance” 영역에서 제공된다고 나와 있고 ([datahub][2]), OSS 버전에서는 이 부분이 플러그인/외부 연계로 구현해야 하는 경우가 많음.                                                                                                                                                                       |
| **거버넌스 중심 사용자 경험 및 팀 워크플로우** | 거버넌스·준수 담당자(CDO, Data Steward 등)를 위한 맞춤형 UI, 안내/작업 흐름(workflow)이 상대적으로 부족                                | 예컨대 “데이터 생산자(data producers)가 정책·표준을 설정하고 자산 등록 시 거버넌스를 좌우할 수 있도록 하는 shift-left 거버넌스” 같은 기능은 관리형 제품 문서에서 강조되고 있음. ([datahub][3])                                                                                                                                                                                        |
| **엔터프라이즈 SLA, 보안, 운영 수준**          | 대규모 조직 운영에 필요한 SLA 보장, 보안 인증(SOC2 등), VPC 지원, 대규모 롤아웃 지원 도구 등이 자체 설치 OSS 버전에서는 별도 구축 필요 | 공식 비교에서 OSS 버전은 “99.5% Uptime SLA”, “SOC-2”, “In-VPC Remote Execution Agent” 등이 지원되지 않는다고 명시되어 있음. ([docs.datahub.com][1])                                                                                                                                                                                                                                   |

[1]: https://docs.datahub.com/docs/managed-datahub/managed-datahub-overview?utm_source=chatgpt.com "How DataHub Cloud compares to DataHub OSS"
[2]: https://datahub.com/products/cloud-vs-core/?utm_source=chatgpt.com "DataHub OSS vs Cloud | Enterprise-Ready SaaS Solution"
[3]: https://datahub.com/products/data-governance/?utm_source=chatgpt.com "AI & Data Production | Data Governance Compliance - DataHub"

### 주요 구성요소

| 구성요소                                  | 주요 언어               | 런타임 / 실행 환경                | 역할                                                      |
| ----------------------------------------- | ----------------------- | --------------------------------- | --------------------------------------------------------- |
| **Metadata Service (GMS)**                | **Java (Spring Boot)**  | JVM (Java 17+)                    | 메타데이터 CRUD, GraphQL API, Kafka 이벤트 처리           |
| **Frontend (DataHub UI)**                 | **TypeScript + React**  | Node.js 18+                       | 웹 UI, 메타데이터 탐색, 권한 관리                         |
| **Ingestion Framework**                   | **Python (3.9+)**       | CLI 또는 Docker                   | 다양한 소스(Hive, MySQL, Kafka, etc.)에서 메타데이터 수집 |
| **Search Service**                        | Java                    | Elasticsearch / OpenSearch 백엔드 | 메타데이터 인덱싱 및 검색                                 |
| **MAE / MCE Consumer**                    | Java                    | Kafka Streams / Confluent         | 메타데이터 변경 이벤트 처리                               |
| **DataHub GraphQL Gateway**               | Java                    | Spring Boot + GraphQL Java        | API 게이트웨이 (UI 및 REST 호환)                          |
| **Docker Compose / Kubernetes 배포 환경** | YAML / Helm / Terraform | Docker, K8s                       | 운영 및 배포 자동화                                       |
| **Metadata Model Definition**             | Avro + JSONSchema       | Java/Python 공용                  | 데이터셋, 테이블, 파이프라인 등의 표준 메타모델 정의      |

### 실행 환경

| 구분              | 요구사항                                   | 설명                                  |
| ----------------- | ------------------------------------------ | ------------------------------------- |
| **OS**            | Linux / macOS / Windows (Docker 기반 권장) | 서버 또는 로컬 개발 환경 모두 가능    |
| **Java 환경**     | JDK 17 이상                                | Metadata Service 및 GraphQL Gateway용 |
| **Python 환경**   | Python 3.9+                                | Ingestion 및 스크립트 실행용          |
| **Node.js 환경**  | Node.js 18 이상 + Yarn                     | Frontend(UI) 빌드 및 실행용           |
| **데이터 저장소** | MySQL / PostgreSQL / Neo4j (선택)          | 메타데이터 및 관계 그래프 저장        |
| **검색엔진**      | Elasticsearch 7.x / OpenSearch 2.x         | 메타데이터 검색용                     |
| **메시징 시스템** | Apache Kafka                               | 메타데이터 변경 이벤트 버스           |
| **컨테이너**      | Docker / Kubernetes                        | 배포 및 운영 환경                     |
| **인증/보안**     | OAuth2 / OIDC / SSO                        | 조직 인증 시스템과 연동 가능          |

## Flowable BPM

### 주요 기능

| 구분                     | 기능 항목                | 설명                                                   | 제공 여부             |
| ------------------------ | ------------------------ | ------------------------------------------------------ | --------------------- |
| **기본 구성 요소**       | Flowable Engine          | BPMN 2.0 프로세스 정의 실행 엔진                       | ✅                     |
|                          | Flowable Task App        | 사용자 할당 업무(Task) 처리용 웹 UI                    | ✅                     |
|                          | Flowable IDM App         | 사용자·그룹 관리, 로그인 인증 (기본 UI 포함)           | ✅                     |
|                          | Flowable Admin App       | 실행 중인 프로세스, 배포 관리, 모니터링 UI             | ✅                     |
|                          | Flowable REST API        | REST 기반 프로세스 배포·실행·조회 API                  | ✅                     |
| **프로세스 정의 / 실행** | BPMN 2.0 표준 지원       | StartEvent, Task, Gateway, SubProcess 등 완전 지원     | ✅                     |
|                          | 이벤트(Event) 처리       | Signal, Timer, Message 이벤트 지원                     | ✅                     |
|                          | 프로세스 변수 관리       | Execution Variable, Local Variable 관리                | ✅                     |
|                          | 프로세스 버전 관리       | 동일 프로세스의 다중 버전 관리 가능                    | ✅                     |
|                          | 비동기 Job Executor      | 타이머, 비동기 서비스 Task 처리용 Job Executor 내장    | ✅                     |
| **업무(Task) 처리**      | 사용자 할당 (User Task)  | 특정 사용자 또는 그룹에 Task 할당 가능                 | ✅                     |
|                          | 자동 Task (Service Task) | Java Delegate / Spring Bean 기반 자동 실행             | ✅                     |
|                          | 폼(Form) 기반 Task 처리  | 단순 Form 속성 기반 Task 입력/출력 지원                | ⚙️ 기본 수준           |
| **모델 관리**            | BPMN XML 기반 모델 배포  | `.bpmn20.xml` 파일을 REST 또는 리소스 폴더로 배포 가능 | ✅                     |
|                          | 웹 모델러 제공           | 브라우저에서 BPMN 모델 작성/저장                       | ❌ (엔터프라이즈 전용) |
|                          | 외부 모델러 연동         | bpmn.io, Camunda Modeler 등 외부 도구 사용 가능        | ✅                     |
| **의사결정(Decision)**   | DMN 1.1 규격 지원        | 규칙 기반 의사결정 테이블 실행 엔진                    | ✅                     |
|                          | DMN 웹 모델러            | 웹 상에서 DMN 테이블 작성                              | ❌ (엔터프라이즈 전용) |
| **케이스 관리(CMMN)**    | CMMN 1.1 표준 지원       | 비정형 프로세스(Case Model) 실행 가능                  | ✅                     |
| **이벤트 처리(EDA)**     | Event Registry           | 메시지 기반 이벤트(AMQP, JMS, Kafka 등) 매핑           | ✅ (기본 모듈)         |
| **통합 / 확장성**        | Spring Boot 통합         | `flowable-spring-boot-starter`로 손쉬운 통합 가능      | ✅                     |
|                          | REST API 확장            | 커스텀 REST Endpoint 추가 가능                         | ✅                     |
|                          | Java API                 | RuntimeService, TaskService 등 Java API 직접 호출 가능 | ✅                     |
|                          | Database 독립성          | MySQL, PostgreSQL, Oracle 등 JDBC 기반 지원            | ✅                     |
| **관리 / 모니터링**      | Admin App                | 배포 프로세스, 실행 인스턴스, Job 모니터링             | ✅                     |
|                          | REST Query API           | 실행 인스턴스/히스토리 조회 API 제공                   | ✅                     |
|                          | 히스토리 데이터 관리     | 프로세스 인스턴스 실행 로그 저장                       | ✅                     |
| **보안 / 인증**          | 기본 IDM 모듈            | 사용자·그룹·권한 관리                                  | ✅                     |
|                          | 외부 인증 연동           | LDAP, SSO 연동 가능 (Spring Security 기반)             | ⚙️ 직접 설정 필요      |
| **배포 / 운영**          | 독립 실행형 WAR 배포     | Tomcat 등 서블릿 컨테이너에 배포 가능                  | ✅                     |
|                          | Spring Boot 내장 서버    | 내장형 Spring Boot 기반 실행                           | ✅                     |
|                          | 다중 데이터소스          | 각 엔진별 분리 설정 가능 (BPMN/DMN/CMMN/FORM)          | ✅                     |
|                          | 클러스터링               | DB 기반 Job Locking으로 다중 노드 실행 가능            | ✅                     |
| **UI / 포털**            | Task 관리 UI             | 할당된 업무(Task) 처리 UI                              | ✅                     |
|                          | Process 시작/조회 UI     | 사용자가 프로세스 시작 및 상태 확인                    | ✅                     |
|                          | Web Modeler              | 그래픽 모델링 UI                                       | ❌                     |
|                          | Form Designer            | 웹 폼 작성 UI                                          | ❌ (Enterprise 전용)   |
| **라이선스 / 배포 형태** | 라이선스                 | Apache License 2.0 (완전 오픈소스)                     | ✅                     |
|                          | 배포 형태                | 소스 코드, WAR, Docker 이미지                          | ✅                     |


### 주요 구성요소

| 구성요소                         | 주요 언어                              | 런타임 / 실행 환경 | 역할                                           |
| -------------------------------- | -------------------------------------- | ------------------ | ---------------------------------------------- |
| **Flowable Engine (Core)**       | **Java (Spring Boot 기반)**            | JVM (Java 17 이상) | BPMN, CMMN, DMN 실행엔진 (workflow 실행 핵심)  |
| **Flowable REST API**            | Java (Spring Boot)                     | JVM                | REST API로 엔진 기능 노출                      |
| **Flowable UI Apps**             | Java + TypeScript (AngularJS)          | JVM + Node.js      | Web UI 제공 (Modeler, Admin, Task 등)          |
| **Flowable Modeler**             | Java + AngularJS                       | JVM                | BPMN/DMN/CMMN 모델 작성용 UI                   |
| **Flowable Task**                | Java + AngularJS                       | JVM                | 사용자 업무(Task) 처리용 UI                    |
| **Flowable Admin**               | Java + AngularJS                       | JVM                | 배포/실행 상태 모니터링 UI                     |
| **Flowable IDM**                 | Java + AngularJS                       | JVM                | 사용자/그룹/권한 관리                          |
| **Flowable Spring Boot Starter** | Java                                   | Spring Boot        | 엔진을 내장하여 REST 또는 내장형으로 실행 가능 |
| **Database**                     | SQL (MySQL, PostgreSQL, Oracle, H2 등) | JDBC               | 프로세스 정의, 인스턴스, 히스토리 저장         |

### 실행 환경

| 구분                   | 요구사항                                            | 설명                              |
| ---------------------- | --------------------------------------------------- | --------------------------------- |
| **OS**                 | Linux / Windows / macOS                             | Docker 또는 독립형 실행 가능      |
| **Java 환경**          | JDK 11 이상 (권장: JDK 17)                          | Core 엔진 및 REST API용           |
| **Application Server** | 내장 Tomcat (Spring Boot), 또는 외부 Tomcat/WildFly | REST API 및 UI 배포용             |
| **Database**           | H2 (테스트용), MySQL, PostgreSQL, Oracle, MSSQL     | 메타데이터 및 히스토리 저장       |
| **Build System**       | Maven 또는 Gradle                                   | 패키징 및 배포                    |
| **UI 런타임**          | Node.js + npm (AngularJS 빌드 시)                   | Modeler 및 Admin UI 빌드용        |
| **Container 지원**     | Docker, Kubernetes                                  | `flowable/all-in-one` 이미지 제공 |
| **Auth / SSO**         | Basic Auth, LDAP, OAuth2                            | 기업 인증 연동 가능               |


## Datahub + Flowable

### 통합시 고려사항

| 통합 포인트       | 설명                                                                        |
| ----------------- | --------------------------------------------------------------------------- |
| **Trigger 방식**  | DataHub Metadata Change Event → Flowable REST API 호출                      |
| **프로세스 설계** | 승인 요청, 보안 등급 변경, lineage 업데이트 등 BPMN으로 정의                |
| **API 연계 방식** | REST (Spring Boot Flowable + DataHub Event Hook)                            |
| **인증 처리**     | OAuth2 토큰 또는 Basic Auth 기반 호출                                       |
| **추천 구조**     | Flowable 엔진은 별도 Spring Boot 서비스로 운영하고, DataHub에서 HTTP 트리거 |
