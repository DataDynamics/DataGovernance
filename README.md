# 데이터 거버넌스 분석

## 데이터 거버넌스에서 제공하는 기능

| 구분                                            | 주요 기능               | 세부 설명                                                 |
| --------------------------------------------- | ------------------- | ----------------------------------------------------- |
| **1. 메타데이터 관리 (Metadata Management)**         | 메타데이터 수집(Ingestion) | 다양한 소스(DB, Data Lake, BI, API 등)로부터 메타데이터 자동 수집 및 동기화 |
|                                               | 메타데이터 모델링           | 엔터프라이즈 데이터 모델 정의, Entity/Relation/Schema 구조 관리        |
|                                               | 메타데이터 버전 관리         | 메타데이터 변경 이력 추적, 이전 버전 복원                              |
|                                               | 데이터 프로파일링           | 데이터 품질, 분포, NULL 비율 등 통계 기반 데이터 분석                    |
| **2. 데이터 카탈로그 (Data Catalog)**                | 검색 및 탐색             | 비즈니스/기술 메타데이터 기반 검색, 태그/분류 기반 탐색                      |
|                                               | 분류체계(Taxonomy) 관리   | 조직별, 도메인별, 주제별 데이터 분류 구조 정의                           |
|                                               | 사용자 협업 기능           | 데이터 설명(Comment), 평점(Rating), 추천(Endorsement) 기능       |
| **3. 데이터 품질 관리 (Data Quality Management)**    | 품질 규칙 정의            | 데이터 품질 기준 정의 (정합성, 완전성, 정확성 등)                        |
|                                               | 품질 모니터링             | 품질 점검 자동화, 오류 감지 및 경보                                 |
|                                               | 품질 개선 이력            | 품질 이슈 식별, 개선 조치 및 검증 이력 관리                            |
| **4. 데이터 계보 (Data Lineage)**                  | 컬럼 수준(Lineage) 추적   | 컬럼 단위로 데이터 흐름(ETL → DB → BI 등)을 시각적으로 표시              |
|                                               | 변화 영향도 분석           | 스키마 변경 시 영향받는 다운스트림 시스템 분석                            |
|                                               | 계보 시각화              | 그래프 기반의 데이터 흐름 시각화 UI 제공                              |
| **5. 보안 및 접근 제어 (Security & Access Control)** | 데이터 분류 및 등급 관리      | 중요도, 기밀성, 규제 수준에 따른 보안 등급 지정                          |
|                                               | 접근권한 관리             | 사용자/그룹 단위의 접근 정책 및 권한 승인 프로세스                         |
|                                               | 민감정보 탐지/마스킹         | PII, 금융정보 등 민감 데이터 자동 탐지 및 마스킹 정책 적용                  |
| **6. 정책 및 표준 관리 (Policy & Standards)**        | 데이터 정책 관리           | 데이터 관리 규정, 표준, 가이드라인 문서화 및 배포                         |
|                                               | 규정 준수(Compliance)   | GDPR, ISO 27001, 개인정보보호법 등 컴플라이언스 관리                  |
|                                               | 정책-데이터 연계           | 정책별 적용 데이터셋 및 관리 주체 매핑                                |
| **7. 거버넌스 워크플로우 (Governance Workflow)**       | 승인 및 결재 프로세스        | 메타데이터 변경, 보안 등급 조정 시 승인 흐름 구성                         |
|                                               | 역할 기반 작업 배정         | Steward, Owner, Custodian 등 역할별 책임 관리                 |
|                                               | BPM 연계              | Flowable, Camunda 등 BPM 엔진 연동 통한 자동화된 승인/알림 프로세스      |
| **8. 감사 및 모니터링 (Audit & Monitoring)**         | 변경 이력 감사            | 메타데이터, 정책, 권한 변경 이력 추적                                |
|                                               | 접근 로그 관리            | 사용자별 접근 내역 및 행위 기록                                    |
|                                               | 대시보드                | 품질지표, 정책준수율, 변경추이 등 KPI 시각화                           |
| **9. 통합 및 연계 (Integration & API)**            | 외부 연동               | ETL, BI, Data Warehouse, MDM 등과의 통합                   |
|                                               | API 제공              | REST/GraphQL API를 통한 메타데이터 접근 및 관리                    |
|                                               | 이벤트 기반 연동           | Kafka, Webhook, gRPC 등 실시간 메타데이터 변경 이벤트 전달            |

## 데이터 거버넌스와 Datahub 비교

| 기능 범주            | 기능명                         | DataHub에서의 상태 | 설명                                                                                                                                                 |
| ---------------- | --------------------------- | ------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| 메타데이터 관리         | 메타데이터 수집(Ingestion)         | ✅ 지원됨         | 다양한 커넥터로 메타데이터 수집 가능. ([docs.datahub.com][1])                                                                                                      |
|                  | 메타데이터 모델링/확장성               | ✅ 지원됨         | 유연한 메타데이터 모델 제공. ([scalefree.com][2])                                                                                                              |
|                  | 메타데이터 버전관리                  | ❌ 제한됨         | ‘버전관리’ 기능이 거버넌스 솔루션 수준에서 통합되어 제공된다는 언급은 제한적입니다.                                                                                                    |
| 데이터 카탈로그 / 검색 탐색 | 검색 및 탐색 기능                  | ✅ 지원됨         | 데이터 자산 탐색, 검색 UI/API 제공. ([docs.datahub.com][3])                                                                                                   |
|                  | 분류체계 / 비즈니스 용어 사전(Glossary) | ✅ 지원됨         | “Business Glossary” 등을 위한 커넥터 존재. ([docs.datahub.com][1])                                                                                          |
|                  | 사용자 협업 (주석, 평점 등)           | ❌ 제한됨         | 주석(Comment)이나 평점(Rating) 등 비즈니스 사용자 중심 기능이 기본 제공이라는 문서는 많지 않습니다.                                                                                   |
| 데이터 품질 관리        | 품질 규칙 정의 & 모니터링             | ⚠ 부분지원됨       | “metadata tests, assertions, data freshness checks” 등이 문서화되어 있음. ([docs.datahub.com][3]) 다만, 완전한 품질 엔진 수준의 기능(예: 자동 결함 탐지→개선 워크플로우)은 별도 구현이 필요합니다. |
| 데이터 계보(Lineage)  | 데이터 흐름/계보 추적                | ⚠ 부분지원됨       | 테이블 수준 및 일부 컬럼 수준 계보 지원됨. ([Atlan][4]) 하지만 지원되는 소스가 제한적이고 일부 기능은 수동 또는 커스텀 구현이 필요합니다.                                                              |
| 보안 및 접근제어        | 데이터 등급ㆍ민감도 분류               | ⚠ 제한됨         | 메타데이터 상에서 기밀성·PII 태그 지정 가능성이 있으나, 자동 민감정보 탐지·마스킹 기능이 기본 제공이라는 언급은 많지 않습니다.                                                                         |
|                  | 접근권한 관리(RBAC 등)             | ✅ 지원됨         | Access Management 기능 제공됨. ([datahub][5])                                                                                                           |
| 정책 및 표준 관리       | 정책 문서화 및 연계                 | ❌ 제한됨         | 거버넌스 정책–데이터 연계, 워크플로우 기반 승인 등은 별도 구성이나 확장이 필요합니다.                                                                                                  |
| 거버넌스 워크플로우       | 승인/결재 프로세스, 역할 기반 작업 배정     | ❌ 제한됨         | 워크플로우 엔진 내장이라기보다는 API/SDK를 이용하여 구성해야 하는 측면이 강합니다.                                                                                                  |
| 감사 및 모니터링        | 변경이력 감사, 접근로그, KPI 대시보드     | ⚠ 일부지원됨       | 메타데이터 변경 이력이나 로그는 일부 가능하나, 통합 거버넌스 KPI 대시보드 등은 기본 패키지에서 완전하게 제공된다고 보기 어렵습니다.                                                                       |
| 통합 및 연계          | 외부 시스템 연동(API/SDK)          | ✅ 지원됨         | API/SDK 제공, 다양한 커넥터 존재. ([docs.datahub.com][3])                                                                                                    |
|                  | 이벤트 기반 연계(Streaming)        | ⚠ 제한됨         | “Active metadata / streaming” 개념이 제시되어 있으나, 완전 자동화된 이벤트 기반 연계가 모든 소스에 준비되어 있지는 않습니다. ([Medium][6])                                                 |

[1]: https://docs.datahub.com/integrations?utm_source=chatgpt.com "The #1 Open Source Metadata Platform - DataHub"
[2]: https://www.scalefree.com/blog/data-warehouse/mastering-metadata-data-catalogs-in-data-warehousing-with-datahub/?utm_source=chatgpt.com "Data Catalogs in Data Warehousing with Datahub - Scalefree"
[3]: https://docs.datahub.com/docs/features?utm_source=chatgpt.com "What is DataHub? | DataHub"
[4]: https://atlan.com/know/data-catalog/datahub/column-level-lineage/?utm_source=chatgpt.com "DataHub Data Lineage: Features, Supported Sources & More - Atlan"
[5]: https://datahubproject.io/docs/features/feature-guides/access-management?utm_source=chatgpt.com "Access Management - DataHub"
[6]: https://medium.com/datahub-project/5-features-to-look-out-for-in-a-modern-data-catalog-31dfa4d32957?utm_source=chatgpt.com "5 Features to Look Out for in a Modern Data Catalog | DataHub"
