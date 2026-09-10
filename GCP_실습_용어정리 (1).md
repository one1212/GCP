# GCP 실습 용어정리

## 1. GCP 기본

| 용어 | 설명 |
|---|---|
| 프로젝트 (Project) | GCP 리소스를 묶는 최상위 단위. 고유한 Project ID(예: kdt6-01)를 가짐 |
| 리전 (Region) | 리소스가 위치하는 지리적 영역 (예: asia-northeast1 = 도쿄) |
| 존 (Zone) | 리전 내의 더 작은 물리적 구획 (예: asia-northeast1-a) |
| gcloud CLI | GCP 리소스를 명령줄에서 제어하는 공식 도구. Cloud SDK에 포함됨 |
| Cloud SDK | gcloud, bq, gsutil 등 GCP 명령줄 도구 모음 |
| Google Cloud Code | VSCode/IntelliJ 등에서 GCP 리소스를 다루는 확장 프로그램 |
| IAM | 누가 어떤 리소스에 어떤 권한을 갖는지 관리하는 시스템 |
| 애플리케이션 기본 사용자 인증정보 (ADC) | 로컬 도구가 GCP API를 호출할 때 사용하는 인증 정보 |
| 결제 계정 (Billing Account) | 실제 비용이 청구되는 단위. 하나 이상의 프로젝트에 연결됨 |

## 2. BigQuery

| 용어 | 설명 |
|---|---|
| BigQuery | GCP의 서버리스 데이터 웨어하우스(대규모 SQL 분석 서비스) |
| 데이터세트 (Dataset) | BigQuery에서 테이블들을 담는 상위 컨테이너 (예: billing_dataset) |
| 테이블 (Table) | 실제 행/열 데이터가 저장되는 단위 |
| 네이티브 테이블 | BigQuery 내부에 데이터를 직접 저장하는 일반적인 테이블 형태 |
| 스키마 (Schema) | 테이블의 열 이름과 데이터 타입 정의 |
| Avro | 데이터 직렬화 포맷 중 하나. 스키마 정보를 파일 자체에 포함 |
| bq CLI | BigQuery 전용 명령줄 도구 |
| 테이블 만료 | 지정한 시간 뒤 테이블이 자동 삭제되도록 설정하는 옵션 |
| 위치 (Multi-region) | 데이터가 저장되는 지리적 범위 (예: US). 생성 후 변경 불가 |

## 3. 청구(Billing) 데이터 관련

| 용어 | 설명 |
|---|---|
| 청구 내보내기 (Billing Export) | GCP 청구 데이터를 BigQuery 등 외부로 내보내는 기능 |
| 서비스 (Service) | 청구 대상이 되는 GCP 제품 단위 (예: Compute Engine, BigQuery) |
| SKU | 서비스 내에서 더 세분화된 과금 항목 (예: N1 Predefined Instance Core) |
| 사용량 (Usage) | 리소스가 소비된 양과 그 단위 (예: byte-seconds, seconds) |
| 비용 (Cost) | 실제 청구된 금액 |
| 통화 변환율 | 청구 통화를 다른 통화로 환산하는 비율 |
| 프리티어 (Free Tier) | 일정 사용량까지 무료로 제공되는 등급. 사용 기록은 있지만 비용이 $0 |
| location.country | 리소스 사용이 실제로 발생한 물리적 국가 |

## 4. 컨테이너 & 배포

| 용어 | 설명 |
|---|---|
| Docker 이미지 | 애플리케이션과 실행 환경을 하나로 묶은 실행 가능한 패키지 |
| Dockerfile | Docker 이미지를 어떻게 만들지 정의하는 설정 파일 |
| 컨테이너 (Container) | Docker 이미지를 실제로 실행한 인스턴스 |
| 이미지 태그 | 같은 이미지의 버전을 구분하는 이름표 (예: latest, v2-neon) |
| 다이제스트 (Digest) | 이미지 내용을 고유하게 식별하는 해시값 (sha256:...) |
| Cloud Build | GCP에서 소스 코드를 서버 측에서 빌드해주는 서비스. 로컬에 Docker가 없어도 이미지 빌드 가능 |
| Artifact Registry | Docker 이미지 등 빌드 산출물을 저장하는 GCP 저장소 서비스 |
| 저장소 (Repository) | Artifact Registry 내에서 이미지들을 담는 단위 (예: outlay) |
| nginx | 정적 웹 파일(HTML 등)을 서비스하는 가벼운 웹 서버 소프트웨어 |
| Cloud Run | 컨테이너 이미지를 서버 관리 없이 실행해주는 GCP 서버리스 서비스 |
| 리비전 (Revision) | Cloud Run에 배포될 때마다 생성되는 배포 버전 단위 |
| 공개 접근 허용 | 인증 없이 누구나 서비스 URL에 접근할 수 있도록 하는 설정 |

## 5. 기타

| 용어 | 설명 |
|---|---|
| 아티팩트 (Claude Code) | Claude Code에서 만든 웹페이지를 별도 URL로 게시하는 기능 (GCP Artifact Registry와는 다른 개념) |
| FX (환율) | 대시보드에서 USD ↔ KRW 환산에 사용한 임의 환율 표기 |
