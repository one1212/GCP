# Google Cloud 실습 - 주요 용어 정리

## Google Cloud 인터페이스 관련

| 용어 | 설명 |
| --- | --- |
| **Cloud console** | GUI(그래픽 사용자 인터페이스) 기반 관리 환경. 메뉴 클릭으로 작업 |
| **Cloud Shell** | 브라우저에서 사용하는 CLI(명령줄 인터페이스) 환경. 임시 VM 기반 |
| **gcloud** | Compute Engine 등 대부분의 Google Cloud 서비스를 다루는 핵심 CLI 명령어 |
| **gcloud storage** | Cloud Storage(버킷) 전용 CLI 명령어 |
| **kubectl** | GKE(Kubernetes) 작업용 명령어 |
| **bq** | BigQuery 작업용 명령어 |

## Cloud Storage 관련

| 용어 | 설명 |
| --- | --- |
| **버킷 (Bucket)** | Cloud Storage에서 파일(객체)을 담는 컨테이너. 이름이 전역적으로 고유해야 함 |
| **전역적으로 고유한 이름 (globally unique name)** | 전 세계 모든 GCP 사용자와 겹치지 않아야 하는 이름 규칙 |
| **--recursive** | 버킷 안의 모든 객체·버전까지 통째로 삭제하는 옵션. 되돌릴 수 없음 |

## Cloud Shell VM 관련

| 용어 | 설명 |
| --- | --- |
| **임시 VM (ephemeral/temporary VM)** | Cloud Shell이 실행되는 Compute Engine 인스턴스. 재시작/유휴 종료 시 초기화됨 |
| **$HOME (영구 디스크, 5GB)** | VM은 초기화돼도 $HOME 디렉터리와 그 안의 파일은 유지되는 영구 저장 공간 |
| **웹 미리보기 (Web Preview)** | Cloud Shell에서 실행 중인 웹 앱을 브라우저로 바로 확인하는 기능 |
| **Restart (다시 시작)** | Cloud Shell VM을 명시적으로 초기화하는 동작. 터미널 패널을 닫는 것만으로는 초기화되지 않음 |

## 환경 설정 관련

| 용어 | 설명 |
| --- | --- |
| **환경 변수 (Environment Variable)** | `export`로 설정하는 값(리전, 프로젝트 ID 등). 오타 방지·재사용을 위해 사용 |
| **config 파일** | 환경변수를 저장해두는 사용자 정의 파일 (`~/infraclass/config`) |
| **.profile** | 셸이 시작될 때 자동 실행되는 설정 파일. 여기에 `source` 명령을 넣으면 매번 자동으로 환경변수가 로드됨 |
| **source 명령어** | 파일 안의 명령어(주로 export 문)를 현재 셸 세션에 즉시 적용시키는 명령 |
| **Project ID** | GCP 프로젝트를 식별하는 고유 ID. `gcloud config get-value project`로 조회 가능 |

## 핵심 개념 대비

- **Console vs Shell**: Console은 빠르고 검증됨 / Shell은 세밀한 제어와 자동화(스크립팅) 가능
- **재시작 전후 차이**: `.profile` 수정 전엔 재시작 시 환경변수 소실 → 수정 후엔 재시작해도 유지 (이 실습의 핵심 학습 포인트)

# Private Google Access & Cloud NAT — 핵심 용어 정리

## 네트워크 기본 구조

| 용어 | 설명 |
| --- | --- |
| **VPC (Virtual Private Cloud)** | 프로젝트 안에 만드는 가상 네트워크. 이 실습에선 `privatenet` |
| **서브넷 (Subnet)** | VPC를 리전별로 쪼갠 IP 대역. 이 실습에선 `privatenet-us` (`10.130.0.0/20`) |
| **외부 IP (External IP)** | 인터넷에서 직접 접근 가능한 공인 IP. `vm-internal`은 이게 **없음(None)**이 핵심 조건 |
| **방화벽 규칙 (Firewall rule)** | 특정 IP 대역·포트·프로토콜의 트래픽만 허용/차단하는 VPC 수준 규칙 |
| **CIDR (Classless Inter-Domain Routing)** | IP 주소 범위를 표기하는 방식 (예: `10.130.0.0/20`) |

## 접속 관련

| 용어 | 설명 |
| --- | --- |
| **IAP (Identity-Aware Proxy)** | 외부 IP나 베스천 호스트 없이도 IAM 권한만으로 SSH/RDP 접속을 가능하게 해주는 프록시 터널 |
| **베스천 호스트 (Bastion host)** | 원래는 외부 IP 없는 서버에 접속하기 위해 중간에 두는 점프 서버. IAP가 이 역할을 대체함 |
| **IAP Secured Tunnel User** | IAP 터널을 사용할 수 있는 IAM 역할. 기본적으로 인스턴스 소유자만 보유 |
| **35.235.240.0/20** | IAP 연결이 발생하는 전용 IP 대역. 방화벽 규칙을 이 범위로 제한하면 보안 강화 |

## 외부 연결 관련

| 용어 | 설명 |
| --- | --- |
| **Private Google Access** | 외부 IP 없는 VM이 **Google API/서비스**(Cloud Storage 등)에만 접근할 수 있게 해주는 서브넷 단위 설정 |
| **Cloud NAT (Network Address Translation)** | 외부 IP 없는 VM이 **일반 인터넷 전체**(패키지 업데이트 등)에 나갈 수 있게 해주는 관리형 서비스 |
| **Cloud Router** | Cloud NAT가 동작하기 위해 반드시 필요한 리전 단위 리소스. 라우팅 정보를 관리 |
| **아웃바운드(Outbound) / 인바운드(Inbound) NAT** | Cloud NAT는 아웃바운드(나가는 연결)만 지원. 외부에서 VM으로 새 연결을 먼저 시작하는 인바운드는 불가 |
| **기본 라우트 (0.0.0.0/0)** | 목적지가 특정되지 않은 모든 트래픽이 타는 기본 경로. 기본 인터넷 게이트웨이로 연결됨 |

## 로깅 관련

| 용어 | 설명 |
| --- | --- |
| **Cloud NAT Logging** | NAT를 통한 연결 생성/실패(포트 부족으로 인한 드롭)를 기록하는 기능 |
| **Cloud Logging** | GCP의 통합 로그 저장/조회 서비스. Cloud NAT 로그도 여기로 전송됨 |
| **Logs Explorer** | Cloud Logging에서 로그를 검색·필터링해서 보는 화면 |

## 고가용성 관련

| 용어 | 설명 |
| --- | --- |
| **관리형 서비스 (Managed service)** | Google이 인프라 운영·이중화를 대신 처리해주는 서비스. Cloud NAT가 대표적 |
| **고가용성 (High Availability, HA)** | 장애 발생 시에도 서비스가 끊기지 않고 지속되는 특성. Cloud NAT는 별도 설정 없이 HA 제공 |

# **05.Creating Virtual Machines_필수용어**

## **05. Creating Virtual Machines — 필수 용어 정리**

| **용어** | **의미** | **실습/실무에서 알아둘 점** |
| --- | --- | --- |
| **Machine type (머신 유형)** | VM에 할당되는 vCPU 수와 메모리 용량의 조합 (예: e2-medium) | gcloud 명령에서 --machine-type 값으로 그대로 사용되는 심볼릭 이름 |
| **Series (시리즈)** | 머신 유형의 세대/계열 (E2, N2, C2 등) | E2는 범용(general-purpose) 저비용 계열. 계열마다 가격·성능 특성이 다름 |
| **Standard machine type** | 정해진 vCPU:메모리 비율의 표준 사양 (예: e2-standard-4) | vCPU를 늘리면 메모리도 같이 늘어나는 고정 비율 |
| **Shared-core machine type** | 여러 VM이 물리 코어 일부를 공유하는 저비용 사양 (예: e2-medium, e2-micro) | 상시 고부하가 아닌 소규모 유틸리티/관리용 VM에 적합 |
| **Custom machine type** | vCPU 수와 메모리 용량을 사용자가 직접 지정 | --custom-cpu, --custom-memory 로 생성. 표준 비율에 안 맞는 워크로드에 사용 |
| **CPU platform** | VM에 할당된 실제 물리 CPU 세대 (예: Intel Xeon, AMD EPYC 등) | **VM 생성 후 변경 불가** — 머신 유형·존과 함께 고정되는 속성 |
| **Zone (존) / Region (리전)** | 존=데이터센터 단위, 리전=존들의 지리적 묶음 | 존과 머신 유형은 인스턴스 생성 후 변경 불가 (재생성 필요) |
| **Boot disk (부팅 디스크)** | OS가 설치된 기본 디스크 | 인스턴스 삭제 시 기본적으로 함께 삭제됨 — 이미지 백업하려면 "Delete boot disk when instance is deleted" 옵션을 꺼야 함 |
| **Persistent Disk (PD)** | VM에 붙는 네트워크 기반 블록 스토리지 | 표준(HDD 기반, 저렴) vs SSD(고성능, 고비용) 타입 선택 가능 |
| **External IP address (외부 IP)** | 인터넷에서 VM에 직접 접근 가능한 공인 IP | 임시(ephemeral, 재시작 시 바뀜) vs 정적(static, 고정) 선택 가능. **None으로 설정하면 완전히 격리됨** |
| **Preemptible / Spot VM** | 언제든 중단될 수 있지만 훨씬 저렴한 VM | 한번 생성하면 이후 일반 VM으로 전환 불가 — **생성 시점에만 선택 가능** |
| **Automatic restart (자동 재시작)** | 장애/하드웨어 오류로 VM이 죽었을 때 자동으로 재시작하는 옵션 | 애플리케이션이 재시작을 안전하게 처리하는지(멱등성, idempotent) 확인 필요 |
| **Live migration (라이브 마이그레이션)** | 호스트 유지보수 시 VM을 중단 없이 다른 호스트로 옮기는 기능 | 기본 동작. 대신 종료(terminate)하도록 바꿀 수도 있음 |
| **Network tags (네트워크 태그)** | VM에 붙이는 라벨로 방화벽 규칙의 대상(target)을 지정 | 예: http-server, https-server 태그가 있어야 관련 방화벽 규칙이 적용됨 |
| **RDP (Remote Desktop Protocol)** | Windows VM에 원격 접속하는 프로토콜 | Linux는 SSH, Windows는 RDP — 콘솔의 연결 버튼도 OS에 따라 자동으로 바뀜 |
| **Windows password reset** | Windows VM 최초 로그인을 위한 비밀번호 발급 절차 | 비밀번호 없이는 로그인 불가. gcloud compute reset-windows-password로도 가능 |
| **Structured log view (구조화된 로그 뷰)** | Cloud Logging에서 필드별로 필터링 가능한 로그 화면 | VM 세부정보 페이지의 Logging 탭에서 바로 진입 가능 |

#### **시험/실무에서 자주 헷갈리는 포인트**

**머신 유형 · CPU 플랫폼 · 존**은 VM을 만든 후에는 절대 못 바꾼다 — 바꾸려면 새로 만들어야 함.

**Preemptible/Spot 전환은 생성 시점에만** 결정된다. 실행 중인 일반 VM을 나중에 Spot으로 바꿀 수 없다.

부팅 디스크에서 커스텀 이미지를 만들려면, 그 디스크가 **실행 중인 인스턴스에 붙어 있으면 안 된다** (VM을 먼저 중지해야 함).

외부 IP를 **None**으로 설정하는 것과 Private Google Access/Cloud NAT를 쓰는 것은 별개 개념이다 — [[04.Implement Private Google Access and Cloud NAT_필수정리]] 참고.
