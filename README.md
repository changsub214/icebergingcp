# 🧊 Lakehouse - Iceberg in GCP

[![Apache Iceberg](https://img.shields.io/badge/Apache_Iceberg-REST_Catalog-blue)](https://iceberg.apache.org/) [![GCP](https://img.shields.io/badge/Google_Cloud-GCP-orange)](https://cloud.google.com/gcp) [![Dataproc](https://img.shields.io/badge/Dataproc-Serverless-green)](https://cloud.google.com/dataproc-serverless) [![BigQuery](https://img.shields.io/badge/BigQuery-BigLake-blue)](https://cloud.google.com/bigquery/docs/biglake-intro)

이 저장소는 **Google Cloud Platform (GCP)** 환경에서 **Apache Iceberg**를 활용한 다양한 실습 가이드를 제공합니다.

---

## 🧐 Why Iceberg in GCP?

단순한 데이터 저장을 넘어, GCP 상의 Iceberg는 오픈 소스의 유연성과 구글 클라우드의 강력한 인프라를 결합한 최적의 아키텍처를 제공합니다.

* **진정한 데이터 상호운용성 (Interoperability)**: Iceberg는 특정 벤더에 종속되지 않는 개방형 포맷입니다. [BigLake](https://cloud.google.com/bigquery/docs/biglake-intro)를 통해 데이터를 이동시키지 않고도 **BigQuery, Dataproc(Spark), Vertex AI** 등 다양한 GCP 서비스에서 동일한 데이터를 동시에 쿼리하고 관리할 수 있습니다.
* **엔터프라이즈 가버넌스 (Governance)**: Cloud Storage(GCS)에 저장된 오픈 데이터 포맷(Iceberg)에 대해서도 **BigQuery**의 세밀한 권한 제어(Row/Column-level security)**를 그대로 적용할 수 있습니다.
* **유연한 스키마 및 파티션 진화**: 데이터 재작성 없이도 테이블 구조를 변경하거나 파티션 전략을 수정할 수 있어 데이터 엔지니어링의 운영 부담이 대폭 감소합니다.

## 🚀 Why Dataproc is Better for Iceberg?

Iceberg 데이터를 처리할 때 **Dataproc**은 단순한 Spark 실행 환경 이상의 가치를 제공합니다.

* **Lightning Engine (성능 가속)**: Google이 최적화한 고성능 벡터화 쿼리 엔진입니다. 대규모 Iceberg 테이블 스캔 및 복잡한 JOIN 연산 시 오픈소스 Spark 대비 **최대 수 배 이상의 처리 속도**를 자랑합니다. [상세 문서](https://cloud.google.com/dataproc-serverless/docs/concepts/lightning-engine)
* **Serverless의 편리함**: 인프라 관리 없이 코드에만 집중할 수 있는 [Dataproc Serverless](https://cloud.google.com/dataproc-serverless) 환경은 Iceberg의 복잡한 설정(Catalog 연결, Jar 의존성 등)을 간소화합니다.
* **최적화된 커넥터**: GCS 및 BigQuery REST Catalog와의 네이티브 통합을 통해 데이터 전송 병목을 최소화하고 최적화된 I/O 성능을 제공합니다.

---

## 📂 주요 실습 단계

### 1. BigQuery Managed Iceberg Table (Time Travel)
BigQuery가 직접 GCS 상의 Iceberg 테이블을 관리하는 아키텍처를 실습합니다.
* 데이터 업데이트 후 과거 시점 스냅샷 조회 (`FOR SYSTEM_TIME AS OF`)
* [GCP 문서: Iceberg 테이블의 Time Travel 조회](https://cloud.google.com/bigquery/docs/iceberg-tables#time-travel)

### 2. Iceberg Catalog 전략 비교
메타데이터 가용성과 상호운용성을 극대화하는 카탈로그 구성 방법을 학습합니다.
* **Iceberg REST Catalog**: 오픈소스 생태계 표준 카탈로그 인터페이스. [상세 문서](https://cloud.google.com/bigquery/docs/manage-open-source-metadata)
* **BigQuery Catalog Federation**: 별도의 카탈로그 서버 운영 없이 BigQuery를 통해 메타데이터를 통합 관리하는 혁신적 방식.

### 3. 데이터 및 메타데이터 최적화 (Compaction)
실무에서 쿼리 성능 유지보수를 위해 가장 중요한 관리 기법을 실습합니다.
* **File Compaction**: `rewrite_data_files`를 통한 작은 파일 병합 및 I/O 최적화.
* **Metadata Compaction**: 메타데이터 파일 자동 삭제 정책 및 `expire_snapshots`를 통한 스토리지 절감.

### 4. Dataproc Lightning Engine 성능 벤치마크
구글 공개 데이터세트 중 **Wikipedia** 데이터를 활용해 엔진 성능을 직접 비교합니다.
* **Standard vs Premium Tier**: Lightning Engine의 벡터화 기술이 작업 시간을 얼마나 단축시키는지 확인합니다.

---

## 🏁 시작하기

### 주의 사항
* Notebook in BQ에 필요한 API 활성화 및 인터넷 통신이 가능한 네트워크 환경이 구축되어 있어야 합니다.
* 실습은 서울 리전(`asia-northeast3`)을 기준으로 작성되었습니다. 필요 시 노트북 코드 상의 리전 변수를 수정하세요.

### 실행 방법
1.  **GCP Console** 접속 후 **BigQuery** 메뉴로 이동합니다.
2.  **Notebooks (SQL 워크벤치)** 섹션에서 `Upload Notebook`을 선택하여 본 저장소의 `.ipynb` 파일을 업로드합니다.
3.  업로드된 노트북을 열고 **Runtime을 연결**합니다.
4.  작업 내 일부는 직접 변수 수정이 필요한 과정이 있으므로 순차적으로 셀을 실행하며 진행합니다.

---

## 📧 Contact
* **Author**: Changseop
* **Email**: officialchangseop@gmail.com

---
*Disclaimer: 본 실습 가이드는 학습 목적으로 제작되었으며, 대용량 성능 테스트 시 클라우드 비용이 발생할 수 있으니 주의하시기 바랍니다.*
