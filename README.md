# 🧊 Lakehouse - Iceberg in GCP

[![Apache Iceberg](https://img.shields.io/badge/Apache_Iceberg-REST_Catalog-blue)](https://iceberg.apache.org/) [![GCP](https://img.shields.io/badge/Google_Cloud-GCP-orange)](https://cloud.google.com/gcp) [![Dataproc](https://img.shields.io/badge/Dataproc-Serverless-green)](https://cloud.google.com/dataproc-serverless) [![BigQuery](https://img.shields.io/badge/BigQuery-BigLake-blue)](https://cloud.google.com/bigquery/docs/biglake-intro)

이 저장소는 **Google Cloud Platform (GCP)** 환경에서 **Apache Iceberg**를 활용한 다양한 실습 가이드를 제공합니다.

---

## 🧐 Why Iceberg in GCP?

단순한 데이터 저장을 넘어, GCP 상의 Iceberg는 오픈 소스의 유연성과 구글 클라우드의 강력한 인프라를 결합한 최적의 아키텍처를 제공합니다.

* **상호운용성 (Interoperability)**: Iceberg는 특정 벤더에 종속되지 않는 개방형 포맷입니다. [BigLake](https://cloud.google.com/bigquery/docs/biglake-intro)를 통해 데이터를 이동시키지 않고도 **BigQuery, Dataproc(Spark)** 등 다양한 GCP 서비스뿐만 아니라 **Trino, Hive** 등 다양한 OSS에서도 동일한 데이터를 동시에 쿼리하고 관리할 수 있습니다.
* **가버넌스 (Governance)**: Cloud Storage(GCS)에 저장된 Iceberg에 대해서도 BigQuery의 세밀한 권한 제어(Row/Column-level security)로 확장할 수 있으며 Dataplex를 결합해 데이터 환경의 전반적인 관리와 데이터 품질을 관리하여 거버넌스를 실현할 수 있습니다. (현재는, BigQuery Managed Iceberg Table에 한함.)
* **데이터 가용성 (Availability)**: 구글 클라우드의 핵심 저장소인 GCS에서 운영할 수 있습니다. GCS의 내구성과 가용성을 바탕으로 데이터 복사나 이동 없이도 거대한 규모의 데이터 레이크를 안정적으로 운영할 수 있습니다.  

## 🚀 Why Dataproc is Better for Iceberg?

Iceberg 데이터를 처리할 때 **Dataproc**은 단순한 Spark 실행 환경 이상의 가치를 제공합니다.

* **Lightning Engine (성능 가속)**: [Lightning Engine](https://docs.cloud.google.com/dataproc-serverless/docs/guides/lightning-engine?hl=ko#overview)은 Google이 최적화한 고성능 벡터화 쿼리 엔진입니다. 대규모 Iceberg 테이블 스캔 및 복잡한 JOIN 연산 시 오픈소스 Spark 대비해서 **최대 수 배 이상의 처리 속도**를 자랑합니다. (벤치마크 TPC-H 기준 3.6배 빠름)
* **Serverless의 편리함**: 인프라 관리 없이 코드에만 집중할 수 있는 [Dataproc Serverless](https://docs.cloud.google.com/dataproc-serverless/docs/overview?hl=ko#batch_workloads) 환경은 Iceberg의 복잡한 설정(Catalog 연결, Jar 의존성 등)을 간소화합니다.
* **최적화된 커넥터**: GCS 및 BigQuery REST Catalog와의 네이티브 통합을 통해 데이터 전송 병목을 최소화하고 최적화된 I/O 성능을 제공합니다.

---

## 📂 주요 실습 단계

### 1. BigQuery Managed Iceberg Table (Time Travel)
BigQuery가 직접 GCS 상의 Iceberg 테이블을 관리하는 아키텍처를 실습합니다.  
* 데이터 업데이트 후 과거 시점 스냅샷 조회

### 2. Iceberg Catalog 전략 비교
다양한 카탈로그 구성 방법을 실습합니다.  
* **BigLake metastore (Classic) + Custom Iceberg catalog**: 과거 BigLake metastore와 catalog를 활용한 방식. (Legacy)
* **Iceberg REST Catalog**: 오픈소스 생태계 표준 카탈로그 인터페이스.
* **BigQuery Catalog Federation**: 별도의 카탈로그 서버 운영 없이 BigQuery를 통해 메타데이터를 통합 관리하는 방식.

### 3. 데이터 및 메타데이터 파일 최적화 (Compaction)
유지보수를 위해 중요한 관리 기법을 실습합니다.
* **File Compaction**: 작은 파일 병합을 통한 최적화(Binpack 전략).
* **Metadata File Compaction**: 메타데이터 파일 자동 삭제 정책을 통한 최적화.

### 4. Dataproc Lightning Engine 성능 벤치마크
구글 공개 데이터세트 중 **Wikipedia** 데이터를 활용해 엔진 성능을 직접 비교합니다.
* **Standard vs Premium Tier**: Lightning Engine의 벡터화 기술이 작업 시간을 얼마나 단축시키는지 확인합니다.

---

## 🏁 시작하기

### 주의 사항
* Notebook in BQ에 필요한 API 활성화 및 인터넷 통신이 가능한 네트워크 환경이 구축되어 있어야 합니다.
* 실습은 서울 리전(`asia-northeast3`)을 기준으로 작성되었습니다. 필요 시 노트북 업로드 때와 노트북 내 코드 상의 리전 변수를 수정하세요.
* 작업 내 일부는 직접 변수 수정이 필요한 과정(Catalog > BQ Catalog Federation)이 있으므로 순차적으로 셀을 실행하며 진행합니다.  

### 실행 방법1
1.  **GCP Console** 접속 후 **BigQuery** 메뉴로 이동합니다.
2.  **Notebooks**에서 `Upload Notebook`을 선택하여 본 저장소의 `.ipynb` 파일을 업로드합니다.
3.  업로드된 노트북을 열고 **Runtime을 연결**합니다.

### 실행 방법2
1.  **GCP Console** 접속 후 **BigQuery** 메뉴로 이동합니다.
2.  **Notebooks**에서 `Upload Notebook`을 선택하여 URL 체크 후 아래 코드 복사 및 붙여넣은 후 업로드합니다.
   ```plaintext
  https://raw.githubusercontent.com/changsub214/icebergingcp/main/iceberg_gcp_korea.ipynb
  ```
4.  업로드된 노트북을 열고 **Runtime을 연결**합니다.


---

## 📧 Contact
* **Author**: Changseop
* **Email**: officialchangseop@gmail.com

---
*Disclaimer: 본 실습 가이드는 학습 목적으로 제작되었으며, 대용량 성능 테스트 시 높은 클라우드 비용이 발생할 수 있으니 주의하시기 바랍니다.*
