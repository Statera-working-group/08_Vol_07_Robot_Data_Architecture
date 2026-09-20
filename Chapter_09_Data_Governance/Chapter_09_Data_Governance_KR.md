**Volume 07 Robot Data Architecture**

# 09. Data Governance

## 09.01 Robot Data Governance Framework Overview

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

로봇 데이터 거버넌스(Robot Data Governance)는 로봇이 생성하는 데이터를 전체 수명주기(Lifecycle)에 걸쳐 어떻게 식별하고, 수집하고, 분류하고, 저장하고, 접근하고, 변환하고, 공유하고, 보존하고, 삭제할 것인지를 결정하는 조직적·기술적·운영적 규칙을 확립한다. 기존 기업 데이터와 달리 로봇 데이터는 센서(Sensor), 액추에이터(Actuator), 내비게이션 시스템(Navigation System), AI 모델(AI Model), 운영자(Operator), 환경(Environment), 플릿 인프라(Fleet Infrastructure)와의 물리적 상호작용에서 지속적으로 생성된다. 따라서 거버넌스는 정보 관리(Information Management)를 실제 로봇 운영(Real-World Robot Operations)과 연결해야 한다.

로봇 데이터 거버넌스 프레임워크(Robot Data Governance Framework)는 독립된 관리 계층으로 운영되는 것이 아니라 전체 데이터 아키텍처(Data Architecture)를 포괄해야 한다. 이 볼륨(Volume)에서 설명하는 아키텍처에는 데이터 모델링(Data Modeling), 데이터 파이프라인(Data Pipeline), 텔레메트리(Telemetry), 이벤트 스트리밍(Event Streaming), 시계열 데이터(Time-Series Data), 센서 데이터(Sensor Data), AI 데이터셋(AI Dataset), 디지털 트윈(Digital Twin), 어노테이션(Annotation), 합성 데이터(Synthetic Data), 운영 사례(Operational Case Study)가 포함된다. 거버넌스는 이러한 영역에 공통 정책과 책임 체계를 적용하여 독립적으로 개발된 파이프라인들이 상호 호환되고 추적 가능하며 안전하고 관리 가능한 상태를 유지하도록 한다.

프레임워크는 명확한 데이터 소유권(Data Ownership)과 데이터 스튜어드십(Data Stewardship)을 정의하는 것에서 시작한다. 데이터 소유권은 데이터셋(Dataset)에 대해 책임을 지는 조직이나 업무 기능을 식별하며, 데이터 스튜어드십은 데이터의 품질, 메타데이터(Metadata), 분류, 접근성, 수명주기 규칙을 유지하는 책임을 정의한다. 기술 관리자는 저장 플랫폼과 파이프라인을 운영하며, 로봇 엔지니어, AI 엔지니어, 안전팀, 보안팀, 비즈니스 사용자는 명확하게 정의된 책임 체계에 따라 데이터 생산자(Data Producer) 또는 소비자(Data Consumer)가 된다.

로봇 데이터는 하나의 동질적인 자원으로 취급하기보다 서로 다른 데이터 도메인(Data Domain)의 집합으로 관리해야 한다. 텔레메트리, 이미지(Image), 비디오(Video), 포인트 클라우드(Point Cloud), 관성 측정 장치(IMU) 측정값, 위치추정 정보(Localization Information), 지도(Map), 진단 이벤트(Diagnostic Event), 구성 기록(Configuration Record), 임무 이력(Mission History), AI 어노테이션(AI Annotation), 디지털 트윈 상태(Digital Twin State), 합성 데이터셋(Synthetic Dataset)은 각각 데이터 생성 속도, 크기, 민감도, 보존 요구사항, 운영 가치가 다르다. 따라서 거버넌스 정책은 공통된 기업 프레임워크(Enterprise Framework) 아래에서 도메인별 제어 정책을 제공해야 한다.

거버넌스의 기본 메커니즘 중 하나는 사용 가능한 데이터 자산(Data Asset)을 검색 가능한 형태로 관리하는 데이터 카탈로그(Data Catalog)이다. 카탈로그 항목에는 데이터셋 이름, 소유자, 스키마(Schema), 데이터 생성 로봇, 센서 유형, 타임스탬프(Timestamp), 물리 단위, 저장 위치, 데이터 분류, 보존 정책, 허용된 사용 목적 등이 포함되어야 한다. 이후 장에서는 이러한 카탈로그 기능을 구현하기 위한 기술로 아파치 아틀라스(Apache Atlas)와 데이터허브(DataHub)를 다루며, 거버넌스 개념을 실제 메타데이터 관리(Metadata Management) 인프라와 연결한다.

로봇 정보가 복잡한 처리 체인을 통과하는 환경에서는 메타데이터만으로 충분하지 않다. 데이터 리니지(Data Lineage)는 정보가 최초 데이터 소스(Data Source)에서 시작하여 수집 에이전트(Collection Agent), 메시지 브로커(Message Broker), 데이터 변환(Transformation), 데이터베이스(Database), 피처 파이프라인(Feature Pipeline), 어노테이션 시스템(Annotation System), 학습 데이터셋(Training Dataset), AI 모델까지 어떻게 이동했는지를 기록한다. 리니지 그래프(Lineage Graph)를 활용하면 특정 분석 결과나 학습 모델에 어떤 로봇, 센서, 소프트웨어 버전, 변환 프로세스, 데이터셋이 기여했는지를 확인할 수 있으며, 이를 통해 재현성(Reproducibility)과 근본 원인 분석(Root-Cause Analysis)을 지원할 수 있다.

데이터 품질 거버넌스(Data Quality Governance)는 로봇 정보가 특정 목적에 사용하기에 충분히 신뢰할 수 있는지를 판단하기 위한 측정 가능한 기준을 정의한다. 주요 품질 요소에는 완전성(Completeness), 유효성(Validity), 일관성(Consistency), 정확성(Accuracy), 타임스탬프 무결성(Timestamp Integrity), 동기화(Synchronization), 최신성(Freshness), 고유성(Uniqueness)이 포함된다. 센서 파이프라인에서는 누락 프레임(Missing Frame), 손상 패킷(Corrupted Packet), 캘리브레이션 상태(Calibration Status), 이상치(Outlier), 동기화 오류 등을 추가로 모니터링할 수 있다. 이러한 요구사항은 데이터 품질 서비스 수준 협약(Data Quality SLA)으로 정의하고 자동화된 파이프라인의 일부로 지속적으로 평가할 수 있다.

거버넌스는 데이터의 민감도(Sensitivity)와 운영 영향도(Operational Impact)에 따라서도 데이터를 구분해야 한다. 공개 문서, 일반 운영 텔레메트리, 독점 엔지니어링 정보(Proprietary Engineering Information), 개인 식별 정보(PII), 안전 관련 기록(Safety-Related Record), 인증정보(Credential), 제한 시설 지도, 보안에 민감한 로봇 구성 정보는 동일한 보호 수준을 적용해서는 안 된다. 데이터 분류 정책(Data Classification Policy)은 보호 등급을 설정하고 이를 암호화(Encryption), 인증(Authentication), 권한 부여(Authorization), 로깅(Logging), 네트워크 제어(Network Control), 공유 제한, 승인된 저장 환경과 연결한다.

접근 거버넌스(Access Governance)는 이러한 데이터 분류를 실제로 집행 가능한 권한으로 변환한다. 역할 기반 접근 제어(RBAC)는 로봇 운영자, 유지보수 엔지니어, 데이터 엔지니어, AI 연구자, 플릿 관리자(Fleet Administrator), 보안 관리자(Security Administrator), 외부 파트너와 같은 역할에 따라 권한을 할당할 수 있다. 목적은 정상적인 업무 수행에 필요한 충분한 접근 권한을 제공하면서 불필요한 데이터 노출을 제한하는 것이다. 접근 결정은 감사 가능해야 하고 주기적으로 검토되어야 하며, 온보드 컴퓨터(Onboard Computer), 엣지 인프라(Edge Infrastructure), 온프레미스 시스템(On-Premise System), 클라우드 플랫폼(Cloud Platform), 데이터 저장소 전반에서 일관되게 적용되어야 한다.

개인정보 보호(Privacy)는 이동형 로봇이 애초에 데이터셋 생성을 목적으로 하지 않았던 사람과 환경을 관찰할 수 있기 때문에 특히 중요하다. RGB 카메라는 얼굴이나 문서를 촬영할 수 있고, 마이크는 대화를 녹음할 수 있으며, 위치 기록은 이동 패턴을 드러낼 수 있다. 또한 서비스 로봇은 병원, 사무실, 공장, 공공장소에서 운영될 수 있다. 따라서 거버넌스에는 데이터 수집 목적, 최소 수집(Data Minimization), 마스킹(Masking), 익명화(Anonymization), 허용된 재사용, 데이터 전송, 보존, 삭제 정책이 포함되어야 하며 적용 가능한 개인정보 보호 요구사항과 연계되어야 한다.

데이터 보존 거버넌스(Data Retention Governance)는 각 로봇 데이터 유형을 얼마 동안 유지하고 유용한 기간 또는 법적으로 요구되는 기간이 종료된 이후 어떻게 처리할지를 결정한다. 고속으로 생성되는 원시 센서 스트림(Raw Sensor Stream)은 저장 비용 때문에 짧은 보존 기간이 필요할 수 있지만, 선택된 사고 기록, 집계 텔레메트리(Aggregated Telemetry), 유지보수 이력, 검증된 학습 데이터셋, 안전 증거(Safety Evidence)는 훨씬 장기간 보존해야 할 수 있다. 자동화된 수명주기 규칙(Automated Lifecycle Rule)은 데이터를 서로 다른 저장 계층(Storage Tier)으로 이동시키고 최종적으로 정책에 따른 통제된 삭제를 수행할 수 있다.

AI는 학습 데이터(Training Data)가 로봇의 행동에 직접적인 영향을 미치기 때문에 추가적인 거버넌스 영역을 요구한다. 학습 데이터셋에는 데이터 출처(Provenance), 어노테이션 이력(Annotation History), 변환 기록, 버전 식별자(Version Identifier), 품질 평가 결과, 라이선스 정보(Licensing Information), 학습된 모델과의 관계가 보존되어야 한다. 또한 거버넌스는 데이터 대표성(Representativeness), 부적절한 콘텐츠, 수집 조건, 어노테이션 편향(Annotation Bias), 허가된 사용 목적을 검토해야 한다. 이러한 기록을 통해 조직은 특정 AI 모델이 환경, 로봇 유형, 운영 조건에 따라 서로 다른 행동을 보이는 이유를 재구성할 수 있다.

디지털 트윈 데이터(Digital Twin Data)는 물리적 로봇과 플릿의 디지털 표현(Digital Representation)을 유지하기 때문에 거버넌스의 범위를 더욱 확장한다. 앞선 장에서는 로봇 디지털 트윈 모델, 실제-디지털 동기화(Real-to-Digital Synchronization), 시계열 저장, 시뮬레이션-실환경 루프(Simulation-to-Real Loop), 예지 정비(Predictive Maintenance), 플릿 데이터 집계(Fleet Aggregation)를 다룬다. 거버넌스는 물리적 자산(Physical Asset)과 디지털 표현 사이의 식별 관계와 시간적 관계를 보존하여 오래된 상태(Stale State), 시뮬레이션 상태, 추정 상태(Estimated State), 직접 측정된 상태가 동일한 정보로 잘못 취급되지 않도록 해야 한다.

거버넌스는 문서와 수동 승인만으로 구현하기보다 데이터 플랫폼(Data Platform) 자체에 내재화되어야 한다. 스키마 검증(Schema Validation), 카탈로그 등록(Catalog Registration), 리니지 수집(Lineage Capture), 품질 검사(Quality Check), 접근 제어 집행, 암호화, 보존 정책 실행, 삭제 워크플로(Deletion Workflow), 감사 로깅(Audit Logging)을 데이터 수집 및 처리 파이프라인 내부의 자동화된 제어 기능으로 구현할 수 있다. 이러한 접근 방식은 거버넌스를 주기적으로 수행하는 규정 준수 활동(Compliance Activity)에서 데이터와 함께 지속적으로 실행되는 운영 기능으로 전환한다.

궁극적으로 거버넌스 프레임워크는 로봇 데이터 수명주기(Robot Data Lifecycle) 위에서 동작하는 제어 계층(Control Plane)의 역할을 수행한다. 정책(Policy)은 무엇이 수행되어야 하는지를 정의하고, 메타데이터는 어떤 데이터가 존재하는지를 식별하며, 데이터 소유권은 책임 소재를 확립한다. 데이터 리니지는 정보의 출처를 설명하고, 품질 제어(Quality Control)는 데이터를 신뢰할 수 있는지를 판단하며, 보안 및 개인정보 보호 제어는 누가 데이터를 사용할 수 있는지를 결정한다. 수명주기 정책은 데이터가 얼마 동안 유지될지를 결정한다. 이러한 메커니즘을 통해 빠르게 증가하는 로봇 데이터는 관리되지 않는 파일과 스트림에서 체계적으로 관리되는 조직의 데이터 자산(Data Asset)으로 전환된다.

거버넌스 성숙도(Governance Maturity)는 로봇 배치 규모가 증가함에 따라 점진적으로 발전한다. 초기 시스템에서는 명명 규칙(Naming Convention), 수동으로 관리되는 데이터 목록, 기본적인 접근 권한에 의존할 수 있다. 그러나 대규모 플릿 환경에서는 자동화된 데이터 카탈로그, 데이터 리니지, 품질 모니터링, 정책 집행(Policy Enforcement), 개인정보 보호 제어, 보존 자동화, 거버넌스 지표(Governance Metrics)가 필요하다. 따라서 이 볼륨은 이러한 프레임워크 개요를 시작으로 데이터 카탈로그, 리니지, 데이터 품질 SLA, 개인정보 보호, 역할 기반 접근 제어(RBAC), 보존 정책, AI 윤리(AI Ethics), 보안 분류(Security Classification), 그리고 최종적으로 거버넌스 성숙도 모델(Governance Maturity Model)과 로드맵(Roadmap)으로 확장된다.

## 09.02 Data Catalog Build: Apache Atlas / DataHub [w/Code]

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

데이터 카탈로그(Data Catalog)는 로봇 데이터 아키텍처(Robot Data Architecture) 전반에 분산되어 있는 데이터 자산(Data Asset)을 검색하고 체계적으로 관리할 수 있도록 제공하는 통합 목록이다. 로보틱스(Robotics) 환경에서 이러한 자산에는 텔레메트리 스트림(Telemetry Stream), ROS 2 토픽(ROS 2 Topic), 센서 기록(Sensor Recording), 시계열 테이블(Time-Series Table), 이벤트 스트림(Event Stream), 지도(Map), 디지털 트윈 상태(Digital Twin State), 어노테이션 데이터셋(Annotated Dataset), 합성 데이터(Synthetic Data), AI 학습 데이터셋(AI Training Dataset) 등이 포함될 수 있다. 카탈로그는 엔지니어와 자동화 시스템이 어떤 데이터가 존재하고, 어디에 저장되어 있으며, 누가 소유하고, 어떻게 사용할 수 있는지를 파악할 수 있도록 공통 메타데이터 계층(Common Metadata Layer)을 제공한다.

로봇을 운영하는 조직에서는 온보드 컴퓨터(Onboard Computer), 엣지 서버(Edge Server), 온프레미스 인프라(On-Premise Infrastructure), 오브젝트 스토리지(Object Storage), 데이터베이스(Database), 메시지 브로커(Message Broker), 클라우드 플랫폼(Cloud Platform) 전반에 데이터가 축적되는 경우가 많다. 데이터 카탈로그가 없다면 이러한 자산에 대한 지식은 개별 팀이나 애플리케이션 코드(Application Code) 내부에 머무르게 된다. 데이터 카탈로그는 이러한 분산된 지식을 데이터셋, 스키마(Schema), 저장 위치, 소유자, 분류, 품질 정보, 수명주기 정책(Lifecycle Policy), 데이터 자산 간 관계를 설명하는 구조화된 메타데이터(Structured Metadata)로 전환한다.

카탈로그가 수집하는 메타데이터(Metadata)는 개념적으로 기술 메타데이터(Technical Metadata), 운영 메타데이터(Operational Metadata), 거버넌스 메타데이터(Governance Metadata)로 구분할 수 있다. 기술 메타데이터는 스키마, 필드(Field), 데이터 타입(Data Type), 토픽(Topic), 테이블(Table), 파일(File), API, 저장 시스템을 설명한다. 운영 메타데이터는 업데이트 주기, 데이터 용량, 최신성(Freshness), 파이프라인 실행과 같은 속성을 기록한다. 거버넌스 메타데이터는 소유권, 민감도 분류(Sensitivity Classification), 보존 요구사항, 접근 정책, 비즈니스 설명, 데이터 사용이 승인된 목적 등을 식별한다.

로보틱스 데이터 카탈로그는 데이터의 디지털 저장 위치뿐만 아니라 데이터가 발생한 물리적 출처(Physical Origin)도 표현해야 한다. 따라서 센서 정보는 로봇 ID(Robot ID), 플릿 ID(Fleet ID), 카메라 또는 라이다 식별자(LiDAR Identifier), 좌표 프레임(Coordinate Frame), 펌웨어 버전(Firmware Version), 캘리브레이션 버전(Calibration Version), 데이터 수집 임무(Collection Mission), 타임스탬프(Timestamp)와 연결될 수 있다. 동일한 파일 형식을 사용하는 두 데이터라도 서로 크게 다른 물리적 조건을 나타낼 수 있기 때문에 이러한 문맥(Context)이 중요하다. 메타데이터를 활용하면 이후 데이터를 사용하는 사람이 원래의 로봇 구성을 직접 재구성하지 않고도 해당 조건을 이해할 수 있다.

아파치 아틀라스(Apache Atlas)는 엔터티(Entity), 분류(Classification), 관계(Relationship), 리니지(Lineage)를 중심으로 구성되는 메타데이터 거버넌스 아키텍처(Metadata Governance Architecture)를 제공한다. 데이터 플랫폼에서는 데이터베이스, 테이블, 파일, 프로세스(Process), 데이터셋 등의 리소스를 유형화된 엔터티(Typed Entity)로 표현할 수 있다. 관계는 이러한 엔터티들을 메타데이터 그래프(Metadata Graph)로 연결하며, 분류는 기밀(Confidential), 개인정보(Personal), 안전 관련(Safety-Related), 학습 승인(Training-Approved)과 같은 거버넌스 의미를 부여한다. 이러한 모델은 로봇, 센서, 임무, 기록, AI 데이터셋을 표현하는 로봇 특화 엔터티 유형(Robot-Specific Entity Type)으로 확장할 수 있다.

아파치 아틀라스 기반 설계(Apache Atlas-Based Design)에서는 수집 커넥터(Ingestion Connector) 또는 사용자 정의 통합 서비스(Custom Integration Service)가 로봇 데이터 플랫폼에서 메타데이터를 수집하여 카탈로그에 등록한다. 텔레메트리 데이터베이스는 테이블과 스키마 정보를 제공할 수 있고, 오브젝트 스토리지는 센서 데이터셋의 저장 위치를 제공할 수 있으며, 데이터 처리 파이프라인은 입력과 출력 사이의 관계를 등록할 수 있다. 아틀라스는 이러한 정보를 중앙화된 메타데이터 표현(Centralized Metadata Representation)으로 구성하여 사용자가 원시 로봇 정보와 가공된 데이터 제품(Data Product)의 관계를 검색하고 확인할 수 있도록 한다.

데이터허브(DataHub)는 데이터 검색(Data Discovery), 메타데이터 수집(Metadata Ingestion), 검색(Search), 소유권(Ownership), 리니지, 도메인(Domain), 태그(Tag) 및 관련 거버넌스 기능을 중심으로 설계된 메타데이터 플랫폼(Metadata Platform) 접근 방식을 제공한다. 서로 다른 플랫폼에서 생성된 메타데이터를 통합 카탈로그(Unified Catalog)로 수집하고 조직적 문맥과 연결할 수 있다. 로봇 데이터 아키텍처에서는 엔지니어가 각각의 저장 플랫폼을 상세히 알지 못하더라도 텔레메트리 데이터셋, 센서 기록, 분석 테이블, 피처(Feature), 학습 데이터셋 등의 자산을 검색할 수 있는 인터페이스를 데이터허브를 통해 제공할 수 있다.

실제 데이터허브 구축(DataHub Deployment)은 데이터베이스, 데이터 레이크(Data Lake), 데이터 웨어하우스(Data Warehouse), 스트리밍 플랫폼(Streaming Platform), 오케스트레이션 시스템(Orchestration System) 및 기타 지원 데이터 소스에서 메타데이터를 수집하는 것으로 시작한다. ROS 2 백(ROS 2 Bag), MCAP 파일, 특수 센서 아카이브(Sensor Archive), 플릿 API(Fleet API)와 같은 로봇 특화 시스템은 기존 기업 환경의 테이블과 직접 대응하지 않을 수 있으므로 추가적인 어댑터(Adapter)가 필요할 수 있다. 사용자 정의 수집 로직(Custom Ingestion Logic)은 로봇 특화 메타데이터를 속성(Property), 태그, 도메인 또는 관계로 유지하면서 이러한 리소스를 카탈로그 엔터티로 변환할 수 있다.

카탈로그 구축은 단순히 저장 디렉터리(Storage Directory)를 검색하는 방식이 아니라 데이터 수명주기(Data Lifecycle)를 따라 이루어져야 한다. 로봇이 센서 데이터를 생성하면 메타데이터를 통해 데이터의 출처와 수집 환경을 식별할 수 있다. 파이프라인이 데이터를 변환하면 카탈로그는 파생 데이터셋(Derived Dataset)과 데이터 리니지를 기록할 수 있다. 어노테이션(Annotation) 과정에서는 라벨 버전(Label Version)과 품질 정보를 추가하고, AI 학습 과정에서는 데이터셋을 실험(Experiment)이나 모델(Model)과 연결한다. 이를 통해 서로 관련이 없는 파일과 데이터베이스 객체를 나열하는 평면적인 목록이 아니라 연결된 메타데이터 그래프(Connected Metadata Graph)를 구성할 수 있다.

로봇 플릿(Robot Fleet)은 새로운 데이터 자산을 지속적으로 생성할 수 있기 때문에 자동화(Automation)가 필수적이다. 수천 개의 센서 세션(Sensor Session), 텔레메트리 파티션(Telemetry Partition), 이벤트 토픽(Event Topic), 학습 데이터셋 버전이 생성되는 환경에서는 수동으로 카탈로그를 등록하는 것이 현실적이지 않다. 따라서 메타데이터 등록은 데이터 수집 파이프라인, ETL 또는 ELT 워크플로(Workflow), 스트리밍 인프라, 어노테이션 파이프라인, MLOps 프로세스에 통합되어야 한다. 그러면 새로운 데이터 자산이 정상적인 데이터 처리 과정의 일부로 자동으로 카탈로그에 등록될 수 있다.

유용한 카탈로그 항목(Catalog Entry)은 사용자가 실제 데이터에 접근하기 전에 해당 데이터셋의 적합성을 평가할 수 있을 정도로 충분한 문맥 정보를 제공해야 한다. 설명에는 운영 목적, 수집 환경, 로봇 플랫폼(Robot Platform), 센서 구성, 스키마, 단위(Unit), 좌표계(Coordinate System), 샘플링 속도(Sampling Rate), 시간 범위, 소유자, 품질 상태, 민감도 분류, 보존 정책 등이 포함될 수 있다. AI 데이터셋의 경우 어노테이션 스키마, 데이터셋 버전, 학습-검증-테스트 분할(Train-Validation-Test Partition), 라이선스(License), 데이터 출처(Provenance), 승인된 모델 개발 목적 등을 추가로 기록할 수 있다.

검색 및 발견(Search and Discovery)은 메타데이터를 실질적인 운영 가치로 전환한다. 엔지니어는 로봇 유형, 센서, 임무, 위치 범주(Location Category), 데이터 형식(Data Format), 기간, 소유자, 태그, 도메인, 사용 목적을 기준으로 데이터를 검색할 수 있어야 한다. AI 엔지니어는 저장 경로를 직접 탐색하는 대신 인식 모델 학습(Perception Training)에 적합하도록 검증된 카메라 데이터셋을 검색할 수 있고, 신뢰성 엔지니어(Reliability Engineer)는 특정 하드웨어 리비전(Hardware Revision)이나 고장 이벤트(Failure Event)와 관련된 텔레메트리를 검색할 수 있다.

소유권(Ownership)과 스튜어드십(Stewardship)은 카탈로그 레코드(Catalog Record)에서 직접 확인할 수 있어야 한다. 센서 데이터셋은 로보틱스 플랫폼 팀(Robotics Platform Team)을 소유자로 지정하고 데이터 엔지니어링 그룹(Data Engineering Group)을 기술 스튜어드(Technical Steward)로 지정할 수 있다. AI 학습 데이터는 어노테이션과 모델 개발 적합성을 담당하는 별도의 소유자를 가질 수 있다. 이러한 책임 관계를 명시하면 잘못된 스키마, 누락된 메타데이터, 품질 문제, 접근 제한, 허용된 데이터 사용과 관련된 문제가 발생했을 때 담당자를 파악하기 쉬워진다.

분류(Classification)와 태깅(Tagging)은 데이터 카탈로그를 보안(Security), 개인정보 보호(Privacy), 규정 준수(Compliance) 제어와 연결한다. 사람이 포함된 로봇 카메라 기록에는 개인정보 민감(Privacy-Sensitive) 분류를 적용할 수 있으며, 시설 지도는 보안 민감(Security-Sensitive) 데이터로 분류할 수 있다. 안전 로그(Safety Log), 독점 구성 데이터(Proprietary Configuration Data), 외부 라이선스 데이터셋(Externally Licensed Dataset)에는 서로 다른 태그를 적용할 수 있다. 이러한 분류 정보는 전체 거버넌스 아키텍처에서 접근 제어 결정, 보존 규칙, 암호화 요구사항, 검토 워크플로, 감사(Auditing)를 지원할 수 있다.

데이터 카탈로그는 데이터 리니지(Data Lineage)의 기반도 제공한다. 메타데이터 관계를 통해 원시 카메라 기록이 선택된 프레임(Selected Frame), 어노테이션 객체(Annotated Object), 학습 데이터셋, 피처, 최종 AI 모델로 변환되는 관계를 정의하면 사용자는 상류 및 하류 의존성(Upstream and Downstream Dependency)을 모두 탐색할 수 있다. 원본 데이터셋에 오류가 발견되면 리니지 정보를 이용하여 영향을 받을 수 있는 파생 데이터셋과 모델을 파악할 수 있으므로, 카탈로그 메타데이터는 재현성과 운영 장애 분석(Operational Incident Analysis)에 중요한 역할을 한다.

아파치 아틀라스와 데이터허브를 단순히 서로 경쟁하는 사용자 인터페이스(User Interface)로 이해해서는 안 된다. 두 플랫폼 모두 기업 수준의 메타데이터 및 거버넌스 계층(Enterprise Metadata and Governance Layer)을 구축하기 위한 접근 방식을 제공하며, 실제 선택은 기존 인프라, 통합 요구사항, 거버넌스 프로세스, 운영 전문성에 따라 달라진다. 로보틱스 조직은 메타데이터 모델링 유연성(Metadata Modeling Flexibility), 수집 지원, 리니지 기능, 검색 경험(Search Experience), API, 확장성(Extensibility), 배포 복잡성(Deployment Complexity), 기존 데이터 플랫폼과의 호환성을 기준으로 두 플랫폼을 평가할 수 있다.

구축된 데이터 카탈로그는 로봇 데이터 모델링, 데이터 파이프라인, 텔레메트리, 이벤트 스트리밍, 센서 데이터 관리, AI 데이터 아키텍처, 디지털 트윈 전반을 연결하는 공유 메타데이터 제어 지점(Shared Metadata Control Point)이 된다. 데이터 카탈로그가 데이터베이스, 오브젝트 스토리지, 메시지 브로커 또는 데이터셋 저장소(Dataset Repository)를 대체하는 것은 아니다. 대신 이러한 시스템에 존재하는 데이터 자산을 설명하고 서로 연결한다. 메타데이터 관리(Metadata Management)를 물리적 데이터 저장(Physical Data Storage)과 분리하면 실제 로봇 정보가 여러 인프라 계층에 분산되어 있더라도 조직은 통합된 데이터 관점(Unified Data View)을 유지할 수 있다.

성숙한 로봇 데이터 카탈로그(Robot Data Catalog)는 궁극적으로 개인의 경험이나 지식에 의존하던 데이터 탐색을 조직 전체의 역량(Organizational Capability)으로 전환한다. 엔지니어는 어떤 정보가 존재하는지 확인하고, 해당 데이터의 물리적·기술적 문맥을 이해하며, 책임 있는 소유자를 식별하고, 리니지를 확인하며, 데이터 품질과 분류 상태를 평가하고, 적절한 접근 권한을 요청할 수 있다. 아파치 아틀라스 또는 데이터허브는 이러한 기능을 위한 메타데이터 기반을 제공하며, 이후 데이터 리니지 추적, 품질 SLA 모니터링, 접근 제어, 보존 자동화, 보안 분류, AI 데이터 감사(AI Data Auditing)와 같은 거버넌스 기능으로 확장할 수 있다.

## 09.03 Data Lineage Tracking System Design [w/Code]

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

데이터 리니지(Data Lineage)는 데이터가 최초의 원천(Source)에서 시작하여 수집(Collection), 변환(Transformation), 저장(Storage), 분석(Analysis), 활용(Consumption) 단계로 이동하는 전체 이력을 추적 가능하게 표현한다. 로봇 시스템에서 리니지는 단순히 데이터베이스 테이블 간 관계만 기록해서는 안 된다. 물리적 로봇(Physical Robot), 센서(Sensor), ROS 2 토픽(ROS 2 Topic), 텔레메트리 스트림(Telemetry Stream), 이벤트 메시지(Event Message), 파일(File), 처리 파이프라인(Processing Pipeline), 어노테이션(Annotation), AI 학습 데이터셋(AI Training Dataset), 디지털 트윈(Digital Twin), 분석 결과물(Analytical Product), 배포된 모델(Deployed Model)을 탐색 가능한 의존성 그래프(Dependency Graph)로 연결해야 한다.

리니지 추적 시스템(Lineage Tracking System)은 중요한 데이터 자산(Data Asset)과 처리 활동(Processing Activity)에 지속적으로 유지되는 식별자(Persistent Identity)를 부여하는 것에서 시작한다. 로봇 ID(Robot ID), 플릿 ID(Fleet ID), 센서 ID(Sensor ID), 임무 ID(Mission ID), 데이터셋 버전(Dataset Version), 파이프라인 실행 ID(Pipeline Execution ID), 소프트웨어 버전(Software Version), 모델 버전(Model Version)을 리니지 그래프 내부의 참조 정보로 사용할 수 있다. 이러한 식별자는 서로 다른 로봇, 센서, 임무, 소프트웨어 구성에서 유사한 데이터가 반복적으로 생성될 때 발생할 수 있는 모호성을 방지하고 시스템이 변화한 이후에도 과거의 관계를 추적할 수 있도록 한다.

소스 리니지(Source Lineage)는 정보가 데이터 아키텍처(Data Architecture)에 최초로 유입된 위치를 기록한다. 카메라 프레임(Camera Frame)은 특정 임무를 수행하는 특정 로봇과 카메라에서 생성될 수 있으며, 텔레메트리는 모터 컨트롤러(Motor Controller), 배터리(Battery), 위치추정 모듈(Localization Module), 내비게이션 소프트웨어(Navigation Software)에서 생성될 수 있다. 타임스탬프(Timestamp), 좌표 프레임(Coordinate Frame), 캘리브레이션 버전(Calibration Version), 펌웨어 버전(Firmware Version), 수집 조건(Collection Condition)을 기록하면 모든 디지털 객체를 독립된 파일로 취급하지 않고 이후 데이터를 정확하게 해석하는 데 필요한 물리적 문맥(Physical Context)을 유지할 수 있다.

변환 리니지(Transformation Lineage)는 데이터가 파이프라인에 유입된 이후 어떤 처리를 거쳤는지를 설명한다. 로봇 센서 정보는 저장 시스템이나 분석 시스템에 도달하기 전에 디코딩(Decoding), 동기화(Synchronization), 필터링(Filtering), 압축(Compression), 리샘플링(Resampling), 집계(Aggregation), 익명화(Anonymization), 변환(Conversion), 보강(Enrichment) 등의 과정을 거칠 수 있다. 각각의 변환 과정은 입력, 출력, 처리 로직(Processing Logic), 소프트웨어 또는 컨테이너 버전(Container Version), 실행 시간, 관련 구성(Configuration)을 식별하여 엔지니어가 파생 데이터 자산(Derived Data Asset)이 어떻게 생성되었는지를 이해할 수 있도록 해야 한다.

리니지는 상류 및 하류 의존성(Upstream and Downstream Dependencies)을 모두 표현해야 한다. 상류 탐색(Upstream Navigation)은 특정 학습 데이터셋을 생성하기 위해 어떤 센서 기록과 변환 과정이 사용되었는지를 확인하는 데 활용된다. 하류 탐색(Downstream Navigation)은 선택한 원천 데이터에 어떤 분석 테이블, 대시보드(Dashboard), 데이터셋, 모델이 의존하는지를 확인한다. 손상된 센서 데이터, 잘못된 캘리브레이션, 결함이 있는 어노테이션, 파이프라인 오류가 하류 결과물이 이미 생성된 이후 발견되는 경우 양방향 탐색(Bidirectional Traversal)은 특히 중요하다.

실용적인 리니지 아키텍처(Lineage Architecture)는 리니지 수집(Lineage Capture), 리니지 저장(Lineage Storage), 시각화(Visualization)를 분리한다. 수집 컴포넌트(Capture Component)는 파이프라인, 데이터베이스, 스트리밍 플랫폼(Streaming Platform), 오케스트레이션 시스템(Orchestration System), 오브젝트 스토리지(Object Storage), 어노테이션 워크플로(Annotation Workflow), MLOps 환경에서 메타데이터(Metadata)를 수집한다. 중앙화된 리니지 서비스(Centralized Lineage Service)는 이러한 정보를 표준화된 엔터티(Entity)와 관계(Relationship)로 변환한다. 생성된 의존성 그래프는 API를 통해 조회하거나 엔지니어, 거버넌스 팀, 자동화 시스템이 사용할 수 있도록 데이터 카탈로그(Data Catalog) 인터페이스를 통해 제공할 수 있다.

로봇 플랫폼에서는 모든 데이터 이동 과정을 수동으로 문서화하는 방식이 확장되지 않기 때문에 자동화된 리니지 수집(Automated Lineage Capture)이 필수적이다. 파이프라인 오케스트레이션(Pipeline Orchestration)은 작업이 실행될 때마다 리니지 이벤트(Lineage Event)를 생성할 수 있으며, 데이터 수집 서비스(Ingestion Service)는 원천 및 목적지 자산을 자동으로 등록할 수 있다. 스트리밍 애플리케이션(Streaming Application)은 입력 토픽과 출력 스트림 사이의 관계를 기록하고, AI 파이프라인은 학습 과정에서 데이터셋과 모델의 의존성을 등록할 수 있다. 이러한 자동화를 통해 리니지는 별도의 문서화 작업이 아니라 정상적인 데이터 처리 과정에서 자연스럽게 생성되는 정보가 된다.

배치 리니지(Batch Lineage)와 스트리밍 리니지(Streaming Lineage)는 서로 다른 추적 전략을 요구한다. 배치 파이프라인(Batch Pipeline)은 특정 실행 시점에 입력 데이터셋, 처리 작업, 출력 데이터셋 사이의 관계를 자연스럽게 생성한다. 반면 이벤트 스트리밍 시스템(Event Streaming System)은 메시지를 지속적으로 변환하기 때문에 모든 이벤트를 개별적으로 표현하면 리니지 규모가 지나치게 커질 수 있다. 따라서 스트리밍 리니지는 일반적으로 토픽, 스키마(Schema), 프로세서(Processor), 윈도(Window), 파생 스트림(Derived Stream), 영구 저장 대상(Persistent Sink) 간 관계를 중심으로 관리하고, 세부 추적이 운영상 필요한 경우에만 선택적으로 이벤트 수준 식별자(Event-Level Identifier)를 유지한다.

센서 데이터(Sensor Data)는 대용량 바이너리 객체(Binary Object)가 메타데이터와 분리되어 저장될 수 있기 때문에 추가적인 리니지 문제를 발생시킨다. 이미지(Image), 비디오(Video), 포인트 클라우드(Point Cloud), ROS 2 백(ROS 2 Bag), MCAP 기록은 오브젝트 또는 파일 스토리지(File Storage)에 저장되고 인덱스(Index)와 설명 정보는 다른 시스템에 존재할 수 있다. 리니지 시스템은 물리적 파일을 메타데이터 레코드(Metadata Record), 기록 세션(Recording Session), 센서, 임무, 타임스탬프, 처리 결과와 연결하여 원본 파일이 이동되거나 아카이빙(Archiving)되더라도 데이터 출처(Provenance)가 유지되도록 해야 한다.

AI 학습(AI Training)은 피지컬 AI 데이터 아키텍처(Physical AI Data Architecture)에서 가장 중요한 리니지 체인(Lineage Chain) 중 하나를 형성한다. 원시 센서 기록은 데이터 선택(Selection), 개인정보 보호 처리(Privacy Processing), 어노테이션, 품질 검증(Quality Validation), 데이터셋 구성(Dataset Assembly), 데이터 증강(Data Augmentation), 학습-검증-테스트 분할(Train-Validation-Test Partitioning), 피처 생성(Feature Generation), 모델 학습(Model Training) 단계를 거칠 수 있다. 리니지는 이러한 모든 단계를 최종 모델 버전과 연결하여 이후 물리적 로봇의 행동을 제어하거나 지원하는 모델에 어떤 데이터와 처리 결정이 영향을 미쳤는지를 재구성할 수 있도록 해야 한다.

어노테이션 리니지(Annotation Lineage)는 라벨(Label)의 출처뿐만 아니라 라벨의 변화 과정도 추적해야 한다. 하나의 데이터셋에는 사람이 직접 생성한 라벨(Manual Label), AI 보조 라벨(AI-Assisted Label), 수정된 어노테이션(Corrected Annotation), 이전 버전에서 가져온 라벨이 함께 존재할 수 있다. 어노테이션 도구 버전(Annotation Tool Version), 스키마 버전, 어노테이터 또는 워크플로 식별자, 품질 검토 결과(Quality Review Result), 데이터셋 리비전(Dataset Revision)을 기록하면 원시 관측 데이터(Raw Observation)와 사람 또는 기계가 생성한 해석을 구분할 수 있다. 이러한 구분은 모델 평가 과정에서 어노테이션 결함이 발견될 때 특히 중요하다.

디지털 트윈 리니지(Digital Twin Lineage)는 측정된 물리적 상태와 파생 또는 시뮬레이션된 표현 사이의 관계를 연결한다. 디지털 트윈의 값은 로봇 센서에서 직접 생성될 수도 있고, 집계 결과이거나 분석 모델이 추정한 값 또는 시뮬레이션(Simulation)에서 생성된 값일 수도 있다. 이러한 데이터의 출처는 명확하게 구분되어야 한다. 리니지가 없다면 사용자가 시뮬레이션 상태, 예측 상태(Predicted State), 오래된 상태(Stale State), 직접 측정된 상태를 동일한 정보로 잘못 판단할 수 있으며, 이는 유지보수 분석, 운영 모니터링, 시뮬레이션-실환경 워크플로(Simulation-to-Real Workflow)의 신뢰성을 저하시킬 수 있다.

리니지는 데이터 품질 관리(Data Quality Management)도 지원한다. 품질 규칙(Quality Rule)을 통해 누락된 타임스탬프, 비정상적인 값, 동기화 실패, 손상된 레코드가 발견되면 영향을 받은 데이터 자산에서 상류의 가능한 원인을 추적하고 하류의 종속 결과물을 확인할 수 있다. 모든 데이터셋을 개별적으로 조사하는 대신 엔지니어는 의존성 관계를 이용하여 잠재적인 영향 범위(Impact Radius)를 파악하고 검증, 재처리(Reprocessing), 재학습(Retraining), 롤백(Rollback) 작업의 우선순위를 결정할 수 있다.

데이터 카탈로그와 통합하면 리니지는 일반적인 데이터 검색(Data Discovery) 과정의 일부가 된다. 카탈로그 항목에서는 데이터 자산의 소유자, 스키마, 분류(Classification), 품질 상태, 보존 정책(Retention Policy)과 함께 상류 및 하류 관계를 표시할 수 있다. 따라서 아파치 아틀라스(Apache Atlas)나 데이터허브(DataHub)와 같은 메타데이터 플랫폼(Metadata Platform)을 리니지 그래프를 탐색하기 위한 인터페이스로 활용할 수 있으며, 사용자 정의 어댑터(Custom Adapter)를 통해 로봇, 센서, 임무, ROS 2 기록, 데이터셋, AI 모델과 같은 로봇 특화 엔터티(Robot-Specific Entity)를 등록할 수 있다.

리니지 기록 자체도 민감한 아키텍처 정보를 노출할 수 있기 때문에 거버넌스(Governance)가 필요하다. 의존성 그래프에는 저장 위치, 내부 시스템 이름, 데이터 흐름(Data Flow), 보안 분류(Security Classification), 모델 관계, 운영 인프라(Operational Infrastructure)가 포함될 수 있다. 따라서 리니지에 대한 접근은 적절한 권한 부여 정책(Authorization Policy)을 따라야 하며 메타데이터 변경은 감사 가능(Auditable)해야 한다. 또한 스키마, 파이프라인, 플랫폼이 변경되더라도 과거의 의존성이 사라지지 않도록 리니지 저장소(Lineage Repository)에 신뢰할 수 있는 보존 및 버전 관리(Versioning)를 적용해야 한다.

버전을 인식하는 리니지(Version-Aware Lineage)는 빠르게 변화하는 로봇 소프트웨어 환경에서 특히 중요하다. 논리적인 파이프라인 이름은 동일하게 유지되더라도 알고리즘(Algorithm), 구성 매개변수(Configuration Parameter), 스키마, 캘리브레이션 파일, 컨테이너 이미지(Container Image)가 변경될 수 있다. 따라서 특정 데이터셋이 하나의 파이프라인을 통과했다는 사실만 기록하는 것으로는 충분하지 않다. 리니지 관계는 관련 실행 정보와 버전 문맥(Version Context)을 함께 보존하여 이전 결과를 재현하거나 소프트웨어 업데이트 이후 생성된 데이터와 비교할 수 있도록 해야 한다.

성숙한 리니지 시스템(Mature Lineage System)은 변경 이후뿐만 아니라 변경 이전의 영향 분석(Impact Analysis)도 지원한다. 엔지니어가 스키마를 수정하거나, 텔레메트리 필드를 제거하거나, 센서 캘리브레이션을 교체하거나, 데이터셋을 폐기(Deprecate)하려는 경우 배포 전에 하류 의존성을 확인할 수 있다. 이를 통해 영향을 받는 대시보드, 파이프라인, AI 데이터셋, 모델, 디지털 트윈 프로세스를 식별하고 관련 팀이 변경 작업을 사전에 조율함으로써 로봇 데이터 아키텍처 전반의 숨겨진 결합(Hidden Coupling)을 줄일 수 있다.

궁극적으로 데이터 리니지는 물리적 현실(Physical Reality)과 디지털 의사결정(Digital Decision)을 연결하는 증거 체인(Evidence Chain)을 제공한다. 로봇 데이터가 어디에서 생성되었고, 어떻게 변화했으며, 어떤 시스템이 이를 사용했고, 어떤 데이터 제품이나 AI 모델이 생성되었는지를 설명한다. 자동화된 리니지 추적(Automated Lineage Tracking)을 데이터 카탈로그, 품질 관리, 접근 제어(Access Control), 데이터 보존(Retention), 보안 분류와 결합하면 분산된 로봇 데이터 파이프라인을 관찰 가능하고 책임 추적이 가능한 아키텍처로 전환하여 확장 가능한 플릿 운영(Scalable Fleet Operations)과 피지컬 AI 개발(Physical AI Development)을 지원할 수 있다.

## 09.04 Data Quality SLA Definition and Monitoring [w/Code]

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

데이터 품질 서비스 수준 협약(Data Quality SLA)은 로봇 데이터 아키텍처(Robot Data Architecture) 전반에서 사용되는 데이터의 신뢰성에 대해 측정 가능한 기대 수준을 정의한다. 주로 인프라 가용성(Infrastructure Availability)이나 응답 시간(Response Time)에 초점을 맞추는 기존 서비스 수준 협약(SLA)과 달리, 데이터 품질 SLA는 데이터 자체가 특정 목적에 사용하기에 충분한 완전성(Completeness), 유효성(Validity), 적시성(Timeliness), 일관성(Consistency), 동기화(Synchronization), 정확성(Accuracy)을 갖추었는지를 정의한다. 이를 통해 신뢰할 수 있는 로봇 데이터에 대한 추상적인 기대를 지속적으로 모니터링할 수 있는 명확한 운영 지표(Operational Metric)로 전환한다.

로봇 시스템은 서로 매우 다른 품질 특성을 가진 이기종 정보(Heterogeneous Information)를 생성한다. 텔레메트리(Telemetry)는 높은 빈도로 연속적으로 도착하고, 카메라는 순서가 있는 이미지 프레임(Image Frame)을 생성하며, 라이다(LiDAR)는 대용량 포인트 클라우드(Point Cloud)를 생성하고, 관성 측정 장치(IMU)는 정밀한 시간 정보를 요구한다. 이벤트 스트림(Event Stream)은 개별적인 상태 변화를 나타내며, 지도(Map), 어노테이션(Annotation), AI 데이터셋(AI Dataset), 디지털 트윈 상태(Digital Twin State)는 추가적인 품질 요구사항을 가진다. 따라서 하나의 보편적인 품질 임계값을 적용하는 것은 적절하지 않으며, 데이터 도메인(Data Domain)과 운영 목적에 따라 품질 목표를 설정해야 한다.

완전성(Completeness)은 예상되는 데이터가 실제로 존재하는지를 측정한다. 텔레메트리에서는 특정 시간 구간 동안 수신될 것으로 예상된 측정값 가운데 실제로 수신된 비율을 나타낼 수 있다. 카메라나 라이다 기록에서는 누락된 프레임(Missing Frame)이나 불완전한 기록 세션(Incomplete Session)을 나타낼 수 있다. 학습 데이터셋에서는 필수 라벨(Label), 메타데이터(Metadata), 데이터 분할(Partition) 정보가 존재해야 할 수 있다. 완전성 SLA는 이러한 기대 수준을 임계값(Threshold)으로 정의하여 누락된 정보가 분석이나 AI 파이프라인으로 조용히 전파되기 전에 탐지할 수 있도록 한다.

유효성(Validity)은 데이터가 정의된 구조적·의미적 규칙(Structural and Semantic Rule)을 준수하는지를 판단한다. 스키마 검증(Schema Validation)은 필드(Field), 데이터 타입(Data Type), 필수 속성, 형식(Format), 허용 값(Allowed Value)을 확인하고, 도메인 검증(Domain Validation)은 측정값이 물리적으로 의미 있는 범위에 있는지를 검사한다. 배터리 충전 상태(State of Charge), 로봇 속도, 온도 측정값, 좌표(Coordinate), 임무 상태(Mission Status)는 각각 예상된 제약 조건을 충족해야 한다. 유효하지 않은 레코드는 정책에 따라 거부, 격리(Quarantine), 표시 또는 조사 대상으로 전달할 수 있다.

일관성(Consistency)은 동일한 정보를 표현하는 관련 데이터들이 서로 다른 시스템과 처리 단계에서도 일치하는지를 측정한다. 로봇 식별자(Robot Identifier)는 텔레메트리, 임무 기록, 센서 아카이브(Sensor Archive), 플릿 데이터베이스(Fleet Database) 사이에서 일관되게 유지되어야 한다. 단위(Unit), 좌표 프레임(Coordinate Frame), 상태 코드(Status Code), 스키마 해석도 데이터 생산자와 소비자 사이에서 예상하지 못하게 변경되어서는 안 된다. 특히 엣지(Edge), 온프레미스(On-Premise), 클라우드(Cloud), 분석(Analytics), AI 플랫폼이 독립적으로 관리되는 로봇 상태 정보를 교환할 때 일관성 검사가 중요하다.

최신성(Freshness)은 운영 요구사항을 기준으로 데이터가 얼마나 최근에 생성되거나 갱신되었는지를 나타낸다. 플릿 모니터링(Fleet Monitoring)은 불과 몇 초 전에 생성된 텔레메트리를 요구할 수 있지만, 유지보수 보고서(Maintenance Report)는 더 긴 지연을 허용할 수 있다. AI 학습 데이터셋은 훨씬 오랜 기간 유용할 수 있지만 하드웨어, 환경 또는 소프트웨어의 데이터 분포(Distribution)가 변화하면 적합성을 잃을 수 있다. 따라서 최신성 SLA는 각각의 데이터 소비자가 새로운 정보를 얼마나 빠르게 필요로 하는지에 따라 허용 가능한 데이터 연령(Data Age)이나 업데이트 지연(Update Latency)을 정의해야 한다.

타임스탬프 무결성(Timestamp Integrity)과 동기화(Synchronization)는 인식(Perception)과 제어(Control)가 동일한 물리적 순간을 나타내는 여러 센서 데이터에 의존하는 경우가 많기 때문에 로보틱스에서 매우 중요한 품질 요소이다. 카메라, 라이다, IMU, 휠 오도메트리(Wheel Odometry), 위성항법시스템(GNSS), 액추에이터 텔레메트리(Actuator Telemetry)는 클록 드리프트(Clock Drift)가 발생하거나 타임스탬프가 누락, 중복, 순서 변경 또는 잘못 생성되면 잘못된 정보를 제공할 수 있다. 따라서 품질 모니터링은 다중 모달 정렬(Multimodal Alignment)이 중요한 환경에서 타임스탬프 연속성, 순서, 클록 오프셋(Clock Offset), 동기화 오류와 기타 시간 이상(Time Anomaly)을 측정해야 한다.

정확성(Accuracy)은 데이터가 스키마를 만족하더라도 실제 물리적 현실(Physical Reality)을 잘못 표현할 수 있기 때문에 자동으로 모니터링하기가 더 어렵다. 캘리브레이션 결과(Calibration Result), 기준 측정값(Reference Measurement), 중복 센서(Redundant Sensor), 알려진 랜드마크(Known Landmark), 운영 제약조건(Operational Constraint), 통계 모델(Statistical Model)을 비교 기준으로 활용할 수 있다. 따라서 정확성 SLA는 자동화가 쉬운 검증과 외부 기준, 캘리브레이션 절차 또는 정기적인 엔지니어링 검증(Engineering Verification)이 필요한 측정을 구분해야 하며, 모든 데이터 값이 지속적으로 정확하다고 검증할 수 있다고 가정해서는 안 된다.

고유성(Uniqueness)과 중복 검사(Duplication Check)는 반복된 레코드가 하류 분석(Downstream Analysis)을 왜곡하는 것을 방지한다. 네트워크 재시도(Network Retry), 메시지 재생(Message Replay), 파이프라인 복구(Pipeline Recovery), 동기화 오류, 중복 업로드(Duplicate Upload)는 논리적으로 동일한 로봇 관측값의 복사본을 여러 개 생성할 수 있다. 고유 이벤트 식별자(Unique Event Identifier), 복합 키(Composite Key), 시퀀스 번호(Sequence Number), 타임스탬프, 데이터셋 객체 ID를 이용하여 중복을 탐지할 수 있다. 허용 가능한 중복 비율은 하류 시스템이 멱등 처리(Idempotent Processing)를 지원하는지와 중복 정보가 결과에 미치는 영향에 따라 결정해야 한다.

데이터 품질 SLA는 각 지표를 명확한 범위(Scope), 목표(Target), 측정 구간(Measurement Window), 평가 방법(Evaluation Method), 책임자(Owner), 대응 정책(Response Policy)과 연결해야 한다. 예를 들어 텔레메트리 스트림은 몇 분 단위로 평가되는 완전성 목표를 요구할 수 있지만, AI 데이터셋은 데이터셋이 릴리스(Release)될 때 평가할 수 있다. 시간 구간, 평가 대상 집단(Population), 계산 방법이 정의되지 않은 백분율은 일관되게 해석할 수 없기 때문에 측정 문맥(Measurement Context)을 정의하는 것은 임계값을 정의하는 것만큼 중요하다.

품질 모니터링(Quality Monitoring)은 데이터 파이프라인(Data Pipeline)에 직접 내재화되어야 한다. 검증은 로봇 측 데이터 수집(Robot-Side Collection)에서 시작하여 엣지 수집(Edge Ingestion)과 스트리밍 과정에서 계속 수행하고, 데이터가 데이터베이스, 레이크하우스(Lakehouse), 어노테이션 시스템, AI 학습 워크플로(Training Workflow)에 입력될 때 다시 실행할 수 있다. 초기 검사는 명백하게 결함이 있는 데이터가 불필요하게 이동하는 것을 방지하고, 하류 검사는 데이터 변환 과정에서 발생한 오류를 탐지한다. 이러한 분산 검증(Distributed Validation)은 하나의 중앙 검사 단계에 의존하는 대신 로봇 데이터 수명주기(Robot Data Lifecycle) 전반에 여러 품질 게이트(Quality Gate)를 구성한다.

실시간 모니터링(Real-Time Monitoring)은 텔레메트리와 이벤트 스트림에서 특히 유용하다. 스트리밍 품질 프로세서(Streaming Quality Processor)는 롤링 윈도(Rolling Window)를 이용하여 메시지 누락률, 스키마 위반(Schema Violation), 지연 시간(Latency), 타임스탬프 순서 오류, 중복 비율, 비정상적인 데이터 분포를 계산할 수 있다. 측정값이 SLA 임계값을 벗어나면 시스템은 경고(Alert) 또는 품질 이벤트(Quality Event)를 생성할 수 있다. 이러한 이벤트에는 영향을 받은 데이터 자산, 지표, 관측값, 임계값, 평가 기간, 엔지니어가 문제를 효율적으로 조사하는 데 필요한 문맥 정보가 포함되어야 한다.

배치 품질 모니터링(Batch Quality Monitoring)은 주기적으로 생성되거나 대규모 단위로 처리되는 데이터셋에 대해 스트리밍 검증을 보완한다. 센서 아카이브, 어노테이션 데이터셋, 과거 텔레메트리, 디지털 트윈 스냅샷(Digital Twin Snapshot), AI 학습 데이터 릴리스는 예약된 프로파일링(Profiling)과 검증을 수행할 수 있다. 배치 작업은 데이터셋이 신뢰할 수 있는 처리 또는 학습 단계로 승격되기 전에 완전성, 널 비율(Null Ratio), 데이터 분포 변화, 라벨 통계(Label Statistics), 중복 레코드, 스키마 적합성과 기타 품질 지표를 계산할 수 있다.

품질 상태(Quality Status)는 데이터 카탈로그(Data Catalog)와 리니지 시스템(Lineage System)에 통합되어야 한다. 카탈로그 항목은 일반적인 메타데이터와 함께 품질 점수(Quality Score), SLA 상태, 최근 검증 결과, 책임자, 알려진 장애(Known Incident)를 표시할 수 있다. 리니지를 이용하면 품질 문제를 발생시켰을 가능성이 있는 상류 원천(Upstream Source)과 영향을 받을 수 있는 하류 데이터셋, 대시보드, 디지털 트윈, AI 모델을 식별할 수 있다. 이러한 연결을 통해 품질 모니터링은 독립된 대시보드가 아니라 더 광범위한 거버넌스 및 영향 분석(Impact Analysis)의 일부가 된다.

모든 품질 문제에 동일한 대응을 적용해서는 안 된다. 과거 분석 데이터의 사소한 최신성 지연은 로그 기록만 필요할 수 있지만, 안전 관련 텔레메트리(Safety-Related Telemetry)의 누락은 즉각적인 에스컬레이션(Escalation)을 요구할 수 있다. 정책은 위반을 심각도(Severity)에 따라 분류하고 경고, 격리, 파이프라인 중단(Pipeline Suspension), 재처리(Reprocessing), 장애조치(Failover), 데이터셋 거부(Dataset Rejection), 사람에 의한 검토(Human Review) 등의 작업을 시작할 수 있다. 대응 절차는 모든 SLA 위반을 동일하게 처리하기보다 영향을 받는 데이터가 운영에 미치는 결과를 반영해야 한다.

품질 모니터링은 모니터링 프로세스 자체에 대한 관측 가능성(Observability)도 필요로 한다. 검증 작업(Validation Job)이 실패하거나 품질 지표 업데이트가 중단될 수 있으며, 스키마 변경 이후 품질 규칙이 오래된 상태가 될 수도 있다. 따라서 시스템은 검사가 정상적으로 실행되는지, 예상된 데이터 자산이 검사 대상에 포함되는지, 규칙 버전(Rule Version)이 현재 스키마와 일치하는지를 모니터링해야 한다. 그렇지 않으면 정상 상태를 나타내는 대시보드가 실제로는 건강한 데이터를 의미하는 것이 아니라 측정 자체가 누락된 상태를 의미할 수 있어 잘못된 신뢰를 만들 수 있다.

과거 품질 지표(Historical Quality Metrics)는 장기적인 추세(Long-Term Trend)를 식별하기 위한 근거를 제공한다. 조직은 특정 센서 유형에서 데이터 누락이 반복적으로 발생하는지, 특정 소프트웨어 릴리스가 스키마 위반을 증가시키는지, 특정 로봇 플릿에서 동기화 문제가 발생하는지를 분석할 수 있다. 품질 이력을 로봇 ID, 하드웨어 리비전(Hardware Revision), 펌웨어 버전, 임무, 리니지 정보와 결합하면 개별적인 장애와 구조적인 아키텍처 또는 운영상의 취약점을 구분하는 데 도움이 된다.

AI 데이터 파이프라인(AI Data Pipeline)은 결함이 명확한 파이프라인 장애를 발생시키지 않은 상태에서도 모델 행동(Model Behavior)에 영향을 줄 수 있기 때문에 특히 엄격한 품질 제어가 필요하다. 학습 데이터 SLA는 어노테이션 완전성(Annotation Completeness), 클래스 분포(Class Distribution), 중복 샘플, 손상된 미디어(Corrupted Media), 데이터셋 분할 무결성(Dataset Partition Integrity), 데이터 출처(Provenance), 필수 메타데이터 등을 포함할 수 있다. 품질 게이트를 이용하면 검증되지 않은 데이터셋 버전이 학습에 사용되는 것을 차단하고, 검증 결과를 해당 데이터셋 버전과 함께 보존하여 재현성(Reproducibility)과 향후 모델 문제 분석을 지원할 수 있다.

궁극적으로 데이터 품질 SLA(Data Quality SLA)는 데이터 생산자(Data Producer), 플랫폼 운영자(Platform Operator), 데이터 소비자(Data Consumer) 사이의 계약(Contract)을 형성한다. 생산자는 자신의 출력 데이터에 요구되는 특성을 이해하고, 플랫폼 팀은 파이프라인이 이러한 특성을 유지하는지 모니터링하며, 소비자는 데이터가 특정 목적에 적합한지를 판단할 수 있다. 데이터 카탈로그, 리니지, 소유권(Ownership), 접근 제어(Access Control), 수명주기 거버넌스(Lifecycle Governance)와 지속적인 SLA 모니터링을 결합하면 데이터 품질을 비공식적인 기대 수준에서 벗어나 확장 가능한 로봇 및 피지컬 AI 시스템(Physical AI System) 전반에서 측정 가능하고 책임을 추적할 수 있는 역량으로 전환할 수 있다.

## 09.05 Privacy Protection: GDPR / PIPA for Robot Data

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

로봇 데이터 개인정보 보호(Privacy Protection)는 로봇을 사람, 사물, 작업장, 가정, 공공 환경과 지속적으로 상호작용하는 이동형 데이터 수집 시스템(Mobile Data-Collection System)으로 간주하는 것에서 시작한다. 카메라(Camera), 마이크(Microphone), 라이다(LiDAR), 위치추정 시스템(Localization System), 접근 로그(Access Log), 텔레메트리(Telemetry), 인간-로봇 상호작용 기록(Human-Robot Interaction Record)은 식별 가능한 개인과 직간접적으로 관련된 정보를 포함할 수 있다. 따라서 거버넌스(Governance)는 데이터가 축적된 이후에만 개인정보 문제를 다루는 것이 아니라 수집, 처리, 저장, 공유, AI 학습, 삭제 전 과정에 개인정보 보호 제어를 통합해야 한다.

유럽연합 일반개인정보보호법(General Data Protection Regulation, GDPR)은 개인정보 처리에 관한 포괄적인 프레임워크를 제공하며, 한국의 개인정보 보호법(Personal Information Protection Act, PIPA)은 국내 법률 환경에서 주요 개인정보 보호 의무를 규정한다. 로봇 운영 조직은 관할권(Jurisdiction), 조직의 역할, 처리 환경(Processing Context), 데이터 흐름(Data Flow)에 따라 어떤 법적 요구사항이 적용되는지를 판단해야 한다. 기술 아키텍처(Technical Architecture)는 하나의 보편적인 설정이 모든 배포 환경을 만족한다고 가정하지 않고 관련 규정 준수(Compliance)를 지원할 수 있도록 설계되어야 한다.

기본적인 요구사항은 로봇이 생성한 정보가 개인정보(Personal Data)에 해당하는지를 판단하는 것이다. 명확하게 보이는 얼굴, 녹음된 음성, 직원 식별자(Employee Identifier), 개인과 연결된 차량 식별자, 계정 정보(Account Information)는 비교적 명확한 사례이지만 다른 정보도 추가 데이터셋과 결합되면 개인정보가 될 수 있다. 위치 이력(Location History), 이동 궤적(Movement Trajectory), 상호작용 기록, 타임스탬프(Timestamp), 반복적인 관측 정보는 개인의 이름을 직접 저장하지 않더라도 개인을 식별할 가능성을 만들 수 있다.

개인정보 거버넌스(Privacy Governance)는 처리 목적의 명확화(Purpose Specification)에서 시작해야 한다. 정보를 수집하기 전에 조직은 로봇이 해당 데이터를 필요로 하는 이유와 데이터를 어떻게 사용할 것인지를 정의해야 한다. 내비게이션(Navigation), 장애물 회피(Obstacle Avoidance), 안전 사고 조사(Safety Investigation), 원격 지원(Remote Assistance), 유지보수(Maintenance), 분석(Analytics), AI 학습(AI Training)은 서로 다른 목적이며 각각 다른 처리 방식이 필요할 수 있다. 운영 목적으로 처음 수집된 데이터가 저장 시스템에 존재한다는 이유만으로 제한 없이 AI 학습 데이터로 사용되어서는 안 된다.

데이터 최소화(Data Minimization)는 명확하게 정의된 목적에 필요한 정보만 수집하고 보존함으로써 개인정보 보호 위험을 줄인다. 장애물의 기하학적 정보(Obstacle Geometry)가 필요한 로봇이 항상 개인을 식별할 수 있는 RGB 비디오를 보존해야 하는 것은 아니며, 운영 지표(Operational Metric)는 전체 원시 센서 스트림(Raw Sensor Stream)을 저장하지 않고도 보존할 수 있다. 최소화는 센서 선택(Sensor Selection), 해상도 축소, 관심 영역 필터링(Region Filtering), 선택적 기록(Selective Recording), 이벤트 기반 캡처(Event-Triggered Capture), 집계(Aggregation), 불필요한 정보의 조기 삭제를 통해 구현할 수 있다.

설계 단계부터의 개인정보 보호(Privacy by Design)는 이러한 제어를 로봇 데이터 아키텍처에 배포 이전부터 통합한다. 데이터 수집 에이전트(Collection Agent)는 민감한 데이터 소스를 분류하고, 엣지 프로세서(Edge Processor)는 불필요한 정보를 제거하며, 파이프라인은 마스킹(Masking)을 적용하고, 저장 시스템은 보존 규칙(Retention Rule)을 실행하며, 데이터 카탈로그(Data Catalog)는 개인정보 분류(Privacy Classification)를 기록할 수 있다. 이러한 접근 방식은 개인정보 보호 요구사항을 기술 시스템이 실행할 수 있는 속성으로 구현하므로 정책이나 수동 검토에만 의존하는 방식보다 강력하다.

엣지 처리(Edge Processing)는 민감한 정보가 로봇이나 로컬 환경을 벗어나기 전에 변환함으로써 불필요한 노출을 줄일 수 있다. 기술적으로 적절한 경우 얼굴 흐림 처리(Face Blurring), 번호판 마스킹(License-Plate Masking), 오디오 필터링(Audio Filtering), 이미지 자르기(Cropping), 특징 추출(Feature Extraction) 등의 개인정보 보호 변환을 데이터 소스 가까이에서 수행할 수 있다. 아키텍처는 이러한 변환이 수행되었다는 메타데이터를 보존하여 하류 데이터 소비자가 원본, 마스킹된 데이터, 익명화된 데이터, 파생 데이터(Derived Data)를 구분할 수 있도록 해야 한다.

가명처리(Pseudonymization)는 직접적인 식별 정보를 통제된 식별자(Controlled Identifier)로 대체하면서 정당하게 필요한 경우 일부 레코드의 연계 가능성을 유지한다. 로봇 ID(Robot ID), 사용자 ID(User ID), 운영자 식별자(Operator Identifier), 상호작용 기록에는 직접적인 개인 식별정보 대신 가명 참조(Pseudonymous Reference)를 사용할 수 있다. 그러나 재식별(Re-identification)이 가능한 경우 가명처리된 정보도 개인정보에 해당할 수 있으므로 완전히 익명화된 정보(Anonymous Information)와 동일한 것으로 자동 판단해서는 안 된다.

익명화(Anonymization)는 적용되는 기준에 따라 개인을 더 이상 식별할 수 없도록 데이터를 변환하는 것을 목표로 하지만, 정보가 풍부한 로봇 데이터셋에서 신뢰할 수 있는 익명화를 달성하는 것은 어려울 수 있다. 비디오, 음성, 이동 궤적, 공간적 문맥(Spatial Context), 행동 패턴(Behavioral Pattern), 여러 메타데이터의 결합을 통해 개인이 재식별될 가능성이 있다. 따라서 개인정보 보호 엔지니어링(Privacy Engineering)은 이름이나 얼굴을 제거하는 것만으로 데이터셋이 익명화되었다고 가정하지 않고 전체 정보 환경(Information Environment)을 평가해야 한다.

접근 제어(Access Control)는 개인정보에 민감한 로봇 데이터를 누가 열람하거나 처리할 수 있는지를 제한한다. 역할 기반 접근 제어(Role-Based Access Control, RBAC)는 로봇 운영자, 유지보수 엔지니어, AI 연구자, 데이터 엔지니어, 보안팀, 외부 파트너를 구분할 수 있다. 원시 기록(Raw Recording)은 익명화된 분석 결과보다 더욱 엄격한 권한을 요구할 수 있다. 인증(Authentication), 최소 권한 기반 권한 부여(Least-Privilege Authorization), 주기적인 접근 권한 검토(Access Review), 관리 책임의 분리(Separation of Duties)를 통해 부적절한 내부 또는 외부 사용 위험을 줄일 수 있다.

암호화(Encryption)는 개인정보가 로봇, 엣지 시스템, 온프레미스 인프라(On-Premise Infrastructure), 클라우드 플랫폼(Cloud Platform), 저장 서비스 사이를 이동하는 동안 데이터를 보호한다. 전송 중 데이터(Data in Transit)와 저장 데이터(Data at Rest)는 적절한 보호를 받아야 하며, 암호화 키(Encryption Key)는 조직의 보안 정책(Security Policy)에 따라 별도로 관리되어야 한다. 암호화는 데이터 최소화나 접근 제어를 대체하지 않지만 저장 장치, 통신 채널, 백업(Backup), 인프라 구성요소가 침해되었을 때 정보 노출을 줄일 수 있다.

보존 정책(Retention Policy)은 개인정보 처리 목적과 해당 정보를 유지해야 하는 기간을 연결해야 한다. 일시적인 내비게이션 지원을 위해 수집한 고해상도 기록은 승인된 안전 사고 조사를 위해 보존되는 기록과 서로 다른 보존 기간을 요구할 수 있다. 자동화된 수명주기 정책(Automated Lifecycle Policy)은 데이터 분류와 목적에 따라 정보를 만료(Expire), 아카이빙(Archive), 익명화 또는 삭제할 수 있으며, 이를 통해 민감한 로봇 관측 데이터가 무기한 축적되면서 발생하는 위험을 줄일 수 있다.

로봇 정보가 복제 스토리지(Replicated Storage), 백업, 파생 데이터셋(Derived Dataset), 어노테이션 시스템(Annotation System), 피처 스토어(Feature Store), AI 파이프라인으로 전파된 경우 데이터 삭제(Deletion)는 더욱 복잡해진다. 데이터 리니지(Data Lineage)는 민감한 원천 데이터가 어디로 이동했으며 어떤 하류 데이터 자산이 생성되었는지를 식별하는 데 도움을 줄 수 있다. 따라서 개인정보 보호를 고려한 아키텍처는 하나의 데이터베이스나 오브젝트 스토리지(Object Storage)에서 데이터를 제거하는 것만으로 전체 수명주기 삭제가 완료되었다고 판단하지 않고, 카탈로그 및 리니지 정보와 삭제·보존 정책을 연계해야 한다.

데이터 카탈로그는 민감한 데이터 자체를 노출하지 않으면서 개인정보 보호 메타데이터(Privacy Metadata)를 제공할 수 있다. 카탈로그 항목에는 책임 소유자(Responsible Owner), 수집 목적(Collection Purpose), 민감도 분류(Sensitivity Classification), 적용되는 보존 규칙, 접근 제한(Access Restriction), 처리 상태(Processing Status), 마스킹 또는 가명처리 여부 등을 기록할 수 있다. 이를 통해 거버넌스 팀과 자동화 시스템은 사용자가 실제 로봇 데이터에 접근하거나 데이터를 전송하기 전에 개인정보 보호 의무를 확인할 수 있다.

AI 학습은 여러 로봇, 위치, 시간대에서 수집된 대규모 데이터셋을 결합할 수 있기 때문에 추가적인 개인정보 보호 문제를 발생시킨다. 데이터셋 준비 과정에서는 정보가 의도된 학습 목적에 사용하도록 허용되었는지와 필요한 개인정보 보호 변환(Privacy Transformation)이 적용되었는지를 확인해야 한다. 학습 데이터 버전(Training-Data Version)에는 데이터 출처(Provenance), 개인정보 보호 상태(Privacy Status), 변환 이력(Transformation History), 승인 정보(Approval Information)를 보존하여 이후 모델 조사 과정에서 데이터셋이 어떻게 구성되었는지를 재구성할 수 있도록 해야 한다.

합성 데이터(Synthetic Data)는 일부 민감한 실제 관측 데이터에 대한 의존성을 줄일 수 있지만 합성 데이터라고 해서 자동으로 개인정보 위험이 사라지는 것은 아니다. 합성 데이터 생성 프로세스(Synthetic Data Generation Process)는 실제 데이터를 입력으로 사용할 수 있으며 생성된 결과가 원본 데이터셋에서 파생된 특성을 유지할 수도 있다. 따라서 거버넌스는 합성 데이터가 어떻게 생성되었고 어떤 원천 정보가 사용되었으며, 생성된 데이터셋을 광범위하게 공유하거나 재사용하기 전에 개인정보 보호 평가(Privacy Evaluation)가 필요한지를 기록해야 한다.

투명성(Transparency)과 책임성(Accountability)을 확보하려면 조직은 로봇 데이터 처리 활동을 이해하고 문서화해야 한다. 적용되는 법적 프레임워크와 처리 환경에 따라 조직은 고지(Notice), 처리 활동 기록(Records of Processing), 개인의 권리 처리, 사고 대응(Incident Response), 고위험 처리 평가(High-Risk Processing Assessment), 개인정보 보호 제어가 의도대로 작동한다는 것을 입증하는 메커니즘을 필요로 할 수 있다. 따라서 거버넌스 기록은 정책을 실제 시스템, 데이터셋, 책임자, 처리 활동과 연결해야 한다.

국제적 또는 조직 간 데이터 이전(Data Transfer)은 로봇 플릿(Robot Fleet)이 한 관할권에서 운영되면서 데이터 플랫폼, 클라우드 인프라, 개발팀 또는 AI 학습 환경이 다른 지역에 존재할 수 있기 때문에 추가적인 주의가 필요하다. 개인정보에 민감한 정보를 이전하기 전에 조직은 적용되는 요구사항과 승인된 이전 메커니즘(Transfer Mechanism)을 확인해야 한다. 아키텍처 제어는 데이터 저장 지역(Storage Region)을 식별하고, 복제(Replication)를 제한하며, 데이터 분류를 적용하고, 승인된 목적지(Authorized Destination)를 기록함으로써 이러한 관리를 지원할 수 있다.

개인정보 보호 모니터링(Privacy Monitoring)은 최초 시스템 승인 단계에서만 수행하는 것이 아니라 지속적으로 운영되어야 한다. 새로운 센서, 소프트웨어 릴리스(Software Release), AI 기능, 데이터 공유 체계(Data-Sharing Arrangement), 로봇 배포 환경이 추가되면 개인정보 보호 위험이 변화할 수 있다. 자동화된 검사는 분류되지 않은 데이터셋, 과도한 데이터 보존, 누락된 마스킹 단계, 예상하지 못한 데이터 전송 목적지, 비인가 접근 패턴(Unauthorized Access Pattern)을 탐지할 수 있으며, 정기적인 거버넌스 검토를 통해 기존의 처리 목적과 보호 제어가 여전히 적절한지를 판단할 수 있다.

효과적인 GDPR 및 PIPA 기반 로봇 데이터 거버넌스는 궁극적으로 법적 요구사항(Legal Requirement), 기술 아키텍처, 운영 책임성(Operational Accountability)을 결합한다. 목적 제한(Purpose Limitation), 데이터 최소화, 개인정보 보호 기반 처리(Privacy-Aware Processing), 접근 제어, 암호화, 데이터 보존, 삭제, 데이터 카탈로그, 리니지, 감사(Auditing), 지속적인 검토(Continuous Review)는 독립적인 제어가 아니라 상호 보완적인 보호 계층을 형성한다. 이를 통해 조직은 개인정보의 불필요한 노출을 줄이고 데이터 수명주기 전체의 추적 가능성(Traceability)을 유지하면서 확장 가능한 로봇 및 피지컬 AI 데이터 시스템(Physical AI Data System)을 개발할 수 있다.

## 09.06 Data Access Control and RBAC

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}

데이터 접근 제어(Data Access Control)는 누가 또는 어떤 시스템이 로봇 데이터에 접근할 수 있는지, 어떤 작업을 수행할 수 있는지, 그리고 어떠한 조건에서 해당 권한이 유효한지를 정의한다. 로봇 데이터 아키텍처(Robot Data Architecture)는 온보드 컴퓨터(Onboard Computer), 엣지 시스템(Edge System), 온프레미스 서버(On-Premise Server), 클라우드 플랫폼(Cloud Platform), 데이터베이스(Database), 오브젝트 스토리지(Object Store), 이벤트 스트림(Event Stream), AI 데이터셋(AI Dataset), 디지털 트윈(Digital Twin)까지 확장된다. 따라서 접근 제어는 데이터의 민감도, 운영 중요성, 사용 목적에 따라 정보를 보호하면서 분산된 인프라 전반에서 일관되게 동작해야 한다.

역할 기반 접근 제어(Role-Based Access Control, RBAC)는 모든 사용자에게 개별적으로 권한을 할당하는 대신 정의된 조직 또는 시스템 역할(Role)을 중심으로 권한을 구성한다. 역할은 로봇 운영자(Robot Operator), 유지보수 엔지니어(Maintenance Engineer), 데이터 엔지니어(Data Engineer), AI 연구자(AI Researcher), 플릿 관리자(Fleet Administrator), 보안 관리자(Security Administrator), 감사자(Auditor), 외부 파트너(External Partner)와 같은 책임 집합을 나타낸다. 사용자와 서비스에는 적절한 역할이 부여되고, 해당 역할에 따라 접근 가능한 데이터 자원과 수행 가능한 작업이 결정된다.

RBAC의 기본 관계는 사용자(User) 또는 서비스 식별자(Service Identity)를 역할에 할당하고, 역할을 권한(Permission)과 연결하며, 권한을 보호 대상 자원(Protected Resource)에 적용하는 구조로 표현할 수 있다. 권한은 일반적으로 텔레메트리 읽기, 구성 데이터 수정, 센서 기록 다운로드, 이벤트 발행, 데이터셋 관리, 아카이브 정보 삭제와 같이 작업(Action)과 자원(Resource)을 결합한다. 이러한 분리는 로봇 플릿과 조직이 확대될 때 접근 정책을 더욱 쉽게 이해하고 검토하며 변경할 수 있도록 한다.

인증(Authentication)과 권한 부여(Authorization)는 이러한 아키텍처에서 서로 다른 기능을 수행한다. 인증은 사람, 로봇, 애플리케이션(Application), 서비스(Service), 워크로드(Workload)의 신원을 확인하며, 권한 부여는 인증된 식별자가 무엇을 수행할 수 있는지를 결정한다. 사람의 인증에는 기업용 신원 관리 시스템(Enterprise Identity System)과 다중 요소 인증(Multi-Factor Authentication)을 사용할 수 있으며, 로봇과 서비스에는 배포 환경에 적합한 인증서(Certificate), 워크로드 식별자(Workload Identity), API 인증정보(API Credential) 등의 기계 중심 인증 방식을 사용할 수 있다.

로봇 데이터는 여러 세분화 수준(Granularity Level)에서 접근 정책을 적용해야 한다. 사용자는 집계된 플릿 텔레메트리(Aggregated Fleet Telemetry)를 확인할 수 있지만 원시 카메라 기록(Raw Camera Recording)은 다운로드하지 못하도록 설정할 수 있다. AI 엔지니어는 승인된 학습 데이터셋에 접근하면서 운영 로봇의 구성 정보를 수정할 권한은 갖지 않을 수 있다. 유지보수 엔지니어는 자신에게 할당된 로봇의 진단 기록(Diagnostic Record)을 확인하면서 플릿에서 수집된 관련 없는 개인정보 민감 데이터에는 접근하지 못하도록 구성할 수 있다.

최소 권한 원칙(Principle of Least Privilege)은 역할 설계의 기본 원칙이 되어야 한다. 각각의 식별자에는 정당한 업무 수행에 필요한 권한만 부여하고 불필요한 권한은 제공하지 않는다. 광범위한 관리자 권한(Administrator Privilege)은 초기 배포를 단순하게 만들 수 있지만 인프라가 확장될수록 상당한 위험을 발생시킨다. 읽기(Read), 쓰기(Write), 수정(Modify), 내보내기(Export), 승인(Approve), 관리(Administer), 삭제(Delete) 권한을 분리하면 실제 운영 책임에 더욱 적합한 역할을 구성할 수 있다.

직무 분리(Separation of Duties)는 민감한 작업에 추가적인 통제 수단을 제공한다. 데이터셋을 준비하는 사람이 반드시 외부 공개를 승인할 권한까지 가질 필요는 없으며, 보존 정책(Retention Policy)을 수정할 수 있는 엔지니어가 감사 기록(Audit Record)을 삭제할 권한까지 가질 필요도 없다. 중요한 워크플로(Workflow)에서는 생성, 검토, 승인, 실행을 서로 다른 역할에 할당할 수 있으며, 이를 통해 하나의 계정이 침해되거나 오용되더라도 영향력이 큰 전체 작업을 단독으로 수행할 가능성을 줄일 수 있다.

데이터 분류(Data Classification)는 접근 제어 정책에 직접적인 영향을 주어야 한다. 공개 또는 낮은 민감도의 운영 정보는 광범위한 내부 그룹에 제공할 수 있지만, 독점 엔지니어링 데이터(Proprietary Engineering Data), 개인정보(Personal Information), 안전 기록(Safety Record), 시설 지도(Facility Map), 인증정보(Credential), 보안 민감 구성(Security-Sensitive Configuration)은 점진적으로 더욱 강력한 제한을 적용해야 한다. 데이터 카탈로그(Data Catalog)의 분류 정보를 권한 부여 정책과 연결하면 데이터셋이 여러 저장 시스템과 처리 파이프라인 사이를 이동하더라도 일관된 보호 수준을 유지할 수 있다.

접근 제어는 실제 데이터뿐만 아니라 메타데이터(Metadata)도 보호해야 한다. 데이터 카탈로그와 리니지 그래프(Lineage Graph)는 사용자가 원본 파일에 접근할 수 없는 경우에도 데이터셋 이름, 저장 위치, 시스템 아키텍처, 로봇 식별자, 처리 관계, 보안 분류, 모델 의존성(Model Dependency)을 노출할 수 있다. 따라서 카탈로그 정보가 무해하거나 모든 사용자에게 공개되어도 된다고 가정하지 않고 메타데이터 권한(Metadata Permission)도 명확하게 설계해야 한다.

분산형 로봇 아키텍처(Distributed Robot Architecture)는 여러 제어 지점(Control Point)에서 권한을 집행해야 한다. 온보드 애플리케이션은 로컬 파일과 ROS 2 자원에 대한 접근을 제한할 수 있고, 엣지 게이트웨이(Edge Gateway)는 서비스 권한을 집행할 수 있으며, 데이터베이스는 테이블이나 뷰(View)에 대한 접근을 제어할 수 있다. 오브젝트 스토리지는 버킷(Bucket)과 객체를 보호하고 스트리밍 플랫폼(Streaming Platform)은 토픽(Topic) 접근을 제한할 수 있다. 클라우드와 온프레미스 시스템이 서로 다른 권한 부여 기술을 사용하더라도 각각 독립적인 규칙이 아니라 일관된 거버넌스 모델에서 정책이 파생되어야 한다.

기계 간 접근(Machine-to-Machine Access)은 로보틱스에서 사람의 접근만큼 중요하다. 데이터 수집 에이전트(Collection Agent), 텔레메트리 게이트웨이(Telemetry Gateway), 스트리밍 프로세서(Streaming Processor), 어노테이션 서비스(Annotation Service), 디지털 트윈 애플리케이션, 학습 파이프라인(Training Pipeline), 배포 시스템(Deployment System)은 사람의 직접적인 개입 없이 지속적으로 정보를 교환한다. 각 서비스에는 공통 인증정보를 공유하는 대신 자체적인 식별자와 최소 범위의 권한을 부여하여 침해된 서비스를 격리하고 접근 활동의 주체를 정확하게 추적할 수 있도록 해야 한다.

임시 접근(Temporary Access)과 상황 기반 접근(Contextual Access)은 영구적으로 부여되는 권한과 관련된 위험을 줄일 수 있다. 장애를 조사하는 엔지니어는 제한된 기간에만 높은 수준의 접근 권한이 필요할 수 있으며, 외부 파트너는 특정 프로젝트를 위해 지정된 데이터셋에만 접근해야 할 수 있다. 시간 제한 권한 부여(Time-Limited Authorization), 승인 워크플로(Approval Workflow), 범위가 제한된 토큰(Scoped Token), 자동 만료(Automatic Expiration)를 활용하면 예외적인 요구사항을 영구적인 권한으로 전환하지 않고 필요한 접근을 제공할 수 있다.

접근 정책은 개인정보에 민감한 로봇 정보도 고려해야 한다. 원시 카메라, 오디오(Audio), 위치(Location), 인간-로봇 상호작용 데이터(Human-Robot Interaction Data)는 동일한 원천에서 생성된 익명화 또는 집계 결과보다 더욱 강력한 접근 제한을 요구할 수 있다. RBAC를 이용하면 원시 정보를 처리할 수 있는 역할과 개인정보 보호 처리가 완료된 결과만 사용할 수 있는 역할을 분리할 수 있으며, 이를 마스킹(Masking), 가명처리(Pseudonymization), 암호화(Encryption), 보존 정책 등의 개인정보 보호 제어와 결합할 수 있다.

AI 데이터 파이프라인(AI Data Pipeline)은 추가적인 권한 부여 경계(Authorization Boundary)를 요구한다. 학습 데이터셋(Training Dataset), 어노테이션, 피처 스토어(Feature Store), 실험 결과(Experiment Result), 모델 아티팩트(Model Artifact), 배포 패키지(Deployment Package)가 자동으로 동일한 권한을 공유해서는 안 된다. 연구자는 승인된 데이터셋을 읽고 실험을 생성할 권한을 가질 수 있지만 실제 운영 환경의 배포는 지정된 엔지니어링 또는 운영 역할로 제한할 수 있다. 이러한 경계는 실험 환경에서 운영 로봇 시스템으로 의도하지 않은 변경이 전달되는 위험을 줄인다.

감사 로깅(Audit Logging)은 접근 제어 결정이 실제로 어떻게 수행되었는지를 기록한다. 중요한 이벤트에는 성공 및 실패한 인증, 권한 변경, 민감 데이터 읽기, 데이터 내보내기, 관리자 작업, 정책 변경, 삭제 요청 등이 포함된다. 감사 기록은 행위자(Actor), 자원, 작업, 타임스탬프(Timestamp), 결과(Result), 관련 문맥(Context)을 식별할 수 있어야 하며, 이를 통해 보안 및 거버넌스 팀이 사고 조사(Investigation)나 규정 준수 검토(Compliance Review) 과정에서 활동 이력을 재구성할 수 있도록 해야 한다.

접근 권한 검토(Access Review)는 사람, 프로젝트, 로봇, 조직 구조가 변화하더라도 권한이 적절한 상태로 유지되도록 한다. 직원은 다른 팀으로 이동할 수 있고, 계약자는 업무를 종료할 수 있으며, 서비스는 폐기될 수 있고, 데이터셋에는 새로운 분류가 적용될 수 있다. 정기적인 검토를 통해 사용되지 않는 역할, 과도한 권한(Excessive Privilege), 비활성 식별자(Inactive Identity), 소유자가 없는 서비스 계정(Orphaned Service Account), 현재 책임과 더 이상 일치하지 않는 권한을 식별해야 한다.

데이터 카탈로그는 데이터 자산을 소유자(Owner), 분류, 도메인(Domain), 접근 요구사항과 연결함으로써 RBAC의 중요한 통합 지점(Integration Point)이 될 수 있다. 제한된 데이터셋을 발견한 사용자는 비공식적인 파일 전달을 통해 거버넌스를 우회하는 대신 해당 데이터의 소유자를 확인하고 적절한 접근 권한을 요청할 수 있다. 승인 워크플로는 카탈로그 메타데이터를 이용하여 요청된 역할과 사용 목적이 조직 정책과 일치하는지를 판단할 수 있다.

접근 제어 실패(Access-Control Failure)는 관찰 가능한 보안 이벤트(Security Event)를 생성해야 한다. 반복되는 권한 부여 실패, 비정상적인 대량 다운로드, 예상하지 못한 관리자 작업, 비정상적인 환경에서의 접근, 제한된 데이터셋에 대한 접근 시도는 구성 문제나 보안 사고(Security Incident)를 나타낼 수 있다. 모니터링 시스템은 이러한 이벤트를 식별자, 역할, 데이터 분류, 로봇 운영 정보, 감사 이력과 연계하여 사고 조사와 자동화된 대응(Automated Response)을 지원할 수 있다.

RBAC는 고정된 역할 집합으로 남아 있는 것이 아니라 로봇 데이터 아키텍처의 발전과 함께 진화해야 한다. 초기 배포에서는 소수의 광범위한 역할을 사용할 수 있지만, 플릿 규모가 증가하면 보다 체계적인 역할 계층(Role Hierarchy), 자동화된 프로비저닝(Automated Provisioning), 중앙화된 신원 관리 통합(Centralized Identity Integration), 정기적인 권한 인증(Periodic Certification), 코드 기반 정책(Policy as Code)이 필요해진다. 역할 설계는 자동화된 로봇 및 AI 인프라에 필요한 확장성을 지원하면서도 거버넌스 팀이 이해하고 검토할 수 있을 정도로 명확하게 유지되어야 한다.

효과적인 데이터 접근 제어는 궁극적으로 신원(Identity), 조직의 책임(Organizational Responsibility), 데이터 분류, 기술적 정책 집행(Technical Enforcement)을 연결한다. RBAC는 관리 가능한 기본 구조를 제공하며, 최소 권한, 직무 분리, 서비스 식별자, 임시 접근, 감사, 모니터링, 정기적인 접근 권한 검토는 이러한 기반을 더욱 강화한다. 데이터 카탈로그, 리니지(Lineage), 개인정보 보호 제어, 암호화, 데이터 보존(Retention), 데이터 품질 거버넌스(Data Quality Governance)와 접근 제어를 결합하면 확장 가능한 피지컬 AI 시스템(Physical AI System) 전반에서 로봇 데이터가 불필요하게 노출되지 않으면서 승인된 데이터 소비자(Authorized Consumer)가 필요한 정보를 안전하게 사용할 수 있다.

## 09.07 Data Retention and Deletion Policy Automation [w/Code]

![](images/image7.png){width="7.268055555555556in" height="7.268055555555556in"}

데이터 보존(Data Retention)은 로봇 정보를 얼마 동안 사용 가능한 상태로 유지해야 하는지를 정의하며, 삭제 정책(Deletion Policy)은 해당 정보의 운영적, 법적, 계약적 또는 분석적 목적이 종료되었을 때 어떻게 제거할 것인지를 결정한다. 로봇 플랫폼은 텔레메트리(Telemetry), 이미지(Image), 비디오(Video), 포인트 클라우드(Point Cloud), 이벤트(Event), 지도(Map), 로그(Log), 어노테이션(Annotation), 디지털 트윈 상태(Digital Twin State), AI 데이터셋(AI Dataset)을 지속적으로 생성한다. 자동화된 수명주기 제어(Lifecycle Control)가 없다면 이러한 데이터 자산은 엣지(Edge), 온프레미스(On-Premise), 클라우드(Cloud), 아카이브 인프라(Archival Infrastructure) 전반에 무기한 축적될 수 있다.

보존 요구사항(Retention Requirement)은 하나의 보편적인 저장 기간을 모든 데이터에 적용하기보다 데이터의 목적에 따라 정의해야 한다. 운영 모니터링에 사용되는 실시간 텔레메트리는 상세 데이터의 보존 기간이 비교적 짧을 수 있지만, 집계된 통계(Aggregated Statistics)는 장기간 분석에 활용될 수 있다. 안전 사고 기록, 유지보수 이력(Maintenance History), 검증된 AI 데이터셋, 감사 기록(Audit Record), 계약상 증거(Contractual Evidence)는 각각 서로 다른 보존 기간이 필요할 수 있다. 따라서 각 데이터 클래스(Data Class)는 비즈니스 및 거버넌스 환경에 적합한 명확한 수명주기를 가져야 한다.

보존 정책(Retention Policy)은 데이터 자산을 소유자(Owner), 분류(Classification), 목적(Purpose), 저장 위치(Storage Location), 보존 기간(Retention Period), 만료 조건(Expiration Condition), 최종 처리 방식(Final Disposition)과 연결해야 한다. 최종 처리에는 영구 삭제(Permanent Deletion), 익명화(Anonymization), 집계(Aggregation), 아카이빙(Archiving), 다른 통제된 저장 계층(Storage Tier)으로의 이전이 포함될 수 있다. 이러한 속성을 명확히 정의하면 수명주기 결정을 개별 팀마다 다르게 해석되는 비공식적인 지침이 아니라 기계가 해석할 수 있는 규칙(Machine-Readable Rule)으로 전환할 수 있다.

데이터 분류(Data Classification)는 보존 정책을 결정하는 중요한 입력 정보가 된다. 공개 운영 통계, 내부 텔레메트리, 기밀 엔지니어링 기록(Confidential Engineering Record), 개인정보 민감 센서 기록(Privacy-Sensitive Sensor Recording), 안전 관련 증거(Safety Evidence), 보안 로그(Security Log), 외부 라이선스 데이터셋(Externally Licensed Dataset)은 서로 다른 처리가 필요할 수 있다. 데이터 카탈로그(Data Catalog)의 분류 메타데이터(Classification Metadata)를 보존 규칙과 결합하면 새롭게 등록되는 데이터 자산이 민감도와 사용 목적에 적합한 수명주기 정책을 자동으로 상속하도록 구성할 수 있다.

로봇 데이터 수명주기(Robot Data Lifecycle)는 일반적으로 여러 저장 계층을 거친다. 높은 속도로 생성되는 센서 정보는 처음에는 로봇이나 엣지 서버(Edge Server)에 저장되고, 처리를 위해 온프레미스 또는 클라우드 오브젝트 스토리지(Object Storage)로 이동한 후 장기적으로는 저비용 아카이브 스토리지(Archive Storage)로 이전될 수 있다. 수명주기 자동화(Lifecycle Automation)는 데이터의 생성 후 경과 시간, 접근 빈도, 프로젝트 상태, 분류에 따라 이러한 이동을 수행할 수 있다. 저장 계층화(Storage Tiering)는 장기적 가치가 있는 정보를 유지하면서 저장 비용을 줄이는 데 도움이 된다.

데이터 만료(Expiration)가 항상 즉각적인 물리적 삭제를 의미하는 것은 아니다. 일부 정보는 먼저 접근이 제한된 아카이브(Restricted Archive)로 이동하거나 익명화 또는 집계된 통계로 변환된 후 원본 레코드가 삭제될 수 있다. 반면 일부 데이터셋은 승인된 사용 목적이 종료되는 즉시 폐기해야 할 수 있다. 따라서 정책 엔진(Policy Engine)은 여러 수명주기 작업을 지원하고 정상적인 접근 권한의 만료와 아카이빙, 변환(Transformation), 비가역적 삭제(Irreversible Deletion)를 명확하게 구분해야 한다.

자동화된 데이터 보존(Automated Retention)은 신뢰할 수 있는 메타데이터(Metadata)에서 시작한다. 생성 시각, 소유자, 분류, 데이터셋 버전(Dataset Version), 목적, 저장 위치를 알 수 없다면 시스템이 일관된 방식으로 데이터를 삭제하기 어렵다. 따라서 데이터 카탈로그는 데이터 자산이 생성되거나 등록될 때 수명주기 속성(Lifecycle Attribute)을 기록해야 한다. 파이프라인은 타임스탬프(Timestamp), 보존 클래스(Retention Class), 프로젝트 식별자(Project Identifier), 개인정보 분류(Privacy Classification), 소유권 정보를 자동으로 추가하여 이후 수동 메타데이터 입력에 대한 의존성을 줄일 수 있다.

보존 규칙은 가능한 한 실제 정보를 물리적으로 저장하는 시스템 가까이에서 집행해야 한다. 오브젝트 스토리지는 객체(Object)에 수명주기 규칙을 적용하고, 데이터베이스는 파티션(Partition)이나 레코드를 만료시키며, 시계열 플랫폼(Time-Series Platform)은 보존 기간을 적용하고, 로그 시스템은 오래된 데이터를 순환(Rotation)하거나 삭제할 수 있다. 중앙화된 거버넌스 계층(Centralized Governance Layer)은 공통 정책을 정의하고, 각각의 저장 시스템에 특화된 메커니즘이 해당 인프라 환경에서 필요한 작업을 실행하도록 구성할 수 있다.

원천 데이터 자산(Source Asset)이 여러 하류 복사본과 파생 데이터셋(Derived Dataset)을 생성한 경우 삭제는 더욱 어려워진다. 하나의 카메라 기록이 아카이브로 복사되고, 프레임(Frame)으로 변환되고, 어노테이션된 후 AI 데이터셋에 포함되어 여러 실험에서 참조될 수 있다. 원본 파일만 삭제하는 것으로는 이러한 파생 데이터를 처리할 수 없다. 따라서 데이터 리니지(Data Lineage)는 보존 및 삭제 워크플로(Deletion Workflow)를 상류 및 하류 관계와 연결하여 영향을 받는 데이터 자산을 체계적으로 발견할 수 있도록 해야 한다.

파생 데이터가 반드시 원천 데이터와 정확히 동일한 보존 규칙을 상속하는 것은 아니다. 집계된 지표(Aggregated Metric)는 원본 센서 스트림보다 민감한 정보를 훨씬 적게 포함할 수 있지만, 어노테이션된 이미지는 원본 이미지의 개인정보 특성을 대부분 그대로 유지할 수 있다. 따라서 거버넌스 규칙은 데이터가 변환되었다는 이유만으로 수명주기 의무가 사라졌다고 가정하지 않고 각각의 파생 데이터 자산이 가진 특성과 목적을 평가해야 한다.

AI 데이터셋은 모델 개발 과정에서 수많은 스냅샷(Snapshot), 서브셋(Subset), 어노테이션, 데이터 증강본(Augmentation), 실험용 복사본이 생성될 수 있기 때문에 버전을 인식하는 보존 정책(Version-Aware Retention)이 필요하다. 중요한 모델을 생성하는 데 사용된 데이터셋 버전은 재현성(Reproducibility)을 위해 보존해야 할 수 있지만 임시 중간 산출물(Intermediate Artifact)은 훨씬 빠르게 삭제할 수 있다. 보존 메타데이터는 데이터셋 버전을 실험(Experiment)과 모델 버전(Model Version)에 연결하여 정리 작업이 배포된 AI 행동을 재현하는 데 필요한 증거를 의도하지 않게 삭제하지 않도록 해야 한다.

개인정보 보호 요구사항(Privacy Requirement)은 카메라, 오디오(Audio), 위치(Location), 상호작용 정보 및 기타 잠재적인 개인정보를 포함하는 로봇 데이터에서 삭제를 특히 중요하게 만든다. 저장 용량이 충분하다는 이유만으로 데이터를 무기한 유지해서는 안 된다. 개인정보 보호 기반 수명주기 정책(Privacy-Aware Lifecycle Policy)은 데이터 수집 목적과 만료 규칙을 연결하고 승인된 보존 기간이 종료되면 삭제, 익명화 또는 검토(Review)를 시작하도록 구성할 수 있다. 이를 통해 책임 있는 데이터 처리를 지원하면서 불필요한 정보 노출을 줄일 수 있다.

삭제 워크플로는 단순히 삭제 명령이 실행되었다고 가정하지 않고 검증(Verification)을 포함해야 한다. 시스템은 어떤 데이터 자산이 삭제 대상으로 지정되었는지, 어떤 규칙이 작업을 실행했는지, 언제 삭제가 수행되었는지, 어떤 저장 시스템이 영향을 받았는지, 실행이 성공했는지를 기록할 수 있다. 삭제 실패(Deletion Failure)는 조사를 위한 예외(Exception)를 생성해야 한다. 이러한 검증 기록은 삭제된 콘텐츠 자체를 보존하지 않으면서도 자동화된 수명주기 정책이 설계대로 동작했다는 증거를 제공한다.

백업(Backup)과 복제본(Replica)은 기본 데이터가 삭제되더라도 다른 위치에 복사본이 남아 있을 수 있기 때문에 특별히 고려해야 한다. 복제 데이터베이스(Replicated Database), 재해 복구 환경(Disaster-Recovery Environment), 오브젝트 스토리지 복제본, 오프라인 백업(Offline Backup), 캐시(Cache), 개발용 복사본은 데이터의 실질적인 보존 기간을 연장할 수 있다. 보존 아키텍처는 이러한 복사본이 어떻게 만료되는지를 정의하고 복구 절차(Restoration Procedure)를 통해 이전에 삭제된 데이터가 복구 이후 활성 시스템으로 조용히 다시 유입되지 않도록 해야 한다.

법적, 계약적, 보안 또는 조사 요구사항은 일반적인 삭제 일정을 일시적으로 중단시킬 수 있다. 만료 예정인 데이터셋이 사고(Incident), 감사(Audit), 분쟁(Dispute) 또는 기타 승인된 보존 사유와 관련되어 있다면 계속 유지해야 할 수 있다. 자동화 시스템은 특정 데이터 자산에 대한 삭제를 중단하는 통제된 보존 홀드(Retention Hold)를 지원하고, 적용 이유와 승인 권한을 기록하며, 해당 홀드가 공식적으로 해제되면 정상적인 수명주기를 다시 시작할 수 있어야 한다.

데이터가 장기 보존 단계(Long-Term Retention)로 이동할수록 접근 제어(Access Control)는 더욱 엄격해져야 한다. 아카이브된 정보는 더 이상 빈번한 운영 접근이 필요하지 않을 수 있으므로 허가된 역할을 제한한 저장 공간에 배치할 수 있다. 암호화(Encryption), 불변 로그(Immutable Logging), 승인 워크플로(Approval Workflow), 통제된 복원(Controlled Restoration)을 통해 보존된 증거를 보호할 수 있다. 아카이빙이 만료된 운영 정보가 광범위하게 접근 가능한 상태로 남는 통제되지 않은 보조 데이터 레이크(Secondary Data Lake)가 되어서는 안 된다.

정책 자동화(Policy Automation)는 이벤트 기반 실행(Event-Driven Execution)을 활용하면 효과적이다. 데이터 자산이 생성되면 보존 클래스를 할당하고, 데이터 분류가 변경되면 수명주기 요구사항을 다시 계산하며, 프로젝트가 완료되면 만료 기간을 시작하고, 승인된 삭제 요청이 발생하면 삭제 워크플로를 실행할 수 있다. 또한 예약 작업(Scheduled Job)을 통해 만료된 데이터 자산을 정기적으로 검색할 수 있다. 이벤트와 주기적인 조정(Reconciliation)을 결합하면 장애나 레거시 시스템(Legacy System)으로 인해 정상적인 수명주기 처리를 벗어난 데이터를 식별하는 데 도움이 된다.

모니터링(Monitoring)은 보존 자동화가 얼마나 효과적으로 동작하는지를 측정해야 한다. 유용한 운영 지표에는 보존 정책이 지정되지 않은 데이터 자산, 처리를 기다리는 만료 데이터, 삭제 실패, 활성 상태의 보존 홀드, 만료된 정보가 차지하는 저장 공간, 수명주기 기한이 가까워지는 데이터셋 등이 포함된다. 대시보드(Dashboard)와 경고(Alert)를 통해 이러한 상태를 데이터 소유자와 거버넌스 팀에 제공하면 데이터 보존을 숨겨진 저장 시스템 설정이 아니라 관찰 가능한 운영 프로세스(Observable Operational Process)로 전환할 수 있다.

정책 변경(Policy Change)은 버전 관리(Versioning)와 감사 가능성(Auditability)을 지원해야 한다. 비즈니스 요구사항, 계약, 규정, 데이터 분류, 시스템 목적이 변화하면 보존 기간도 변경될 수 있다. 거버넌스 플랫폼은 어떤 정책 버전(Policy Version)이 특정 데이터 자산에 적용되었으며 변경 사항이 언제부터 효력을 가졌는지를 보존해야 한다. 과거 정책 기록(Historical Policy Record)을 통해 조직은 특정 시점에 데이터셋이 왜 보존, 아카이빙, 익명화 또는 삭제되었는지를 설명할 수 있다.

성숙한 데이터 보존 아키텍처(Mature Retention Architecture)는 데이터 카탈로그, 데이터 리니지, 접근 제어, 개인정보 분류, 저장 수명주기 메커니즘(Storage Lifecycle Mechanism), 감사 기록을 하나의 조정된 시스템(Coordinated System)으로 결합한다. 카탈로그는 어떤 데이터가 존재하고 어떤 정책이 적용되는지를 식별하며, 리니지는 종속된 데이터 자산을 확인하고, 저장 시스템은 수명주기 작업을 실행하며, 감사 메커니즘은 그 결과를 기록한다. 자동화는 이러한 구성요소를 연결하여 분산된 로봇 데이터 인프라 전반에서 정책 결정을 일관되게 집행할 수 있도록 한다.

데이터 보존 및 삭제 자동화(Retention and Deletion Automation)는 궁극적으로 로봇 데이터 플랫폼이 관리되지 않는 정보를 영구적으로 축적하는 저장소가 되는 것을 방지한다. 데이터가 생성되는 시점에 명확한 수명주기를 할당하고, 적절한 저장 계층을 통해 정보를 이동시키며, 정당한 근거가 있는 증거를 보존하고, 불필요한 데이터 자산을 신뢰할 수 있는 방식으로 제거함으로써 조직은 비용을 통제하고 개인정보 및 보안 노출 위험을 줄이며 책임 있는 데이터 운영(Accountable Data Operations)을 유지할 수 있다. 이러한 수명주기 관리 체계는 로봇 플릿(Robot Fleet)과 피지컬 AI 시스템(Physical AI System)이 대규모 데이터를 지속적으로 생성할수록 더욱 중요해진다.

## 09.08 AI Training Data Ethics Audit Framework

![](images/image8.png){width="7.268055555555556in" height="7.268055555555556in"}

AI 학습 데이터 윤리 감사(AI Training Data Ethics Audit)는 로봇 및 피지컬 AI(Physical AI) 모델 개발에 사용되는 데이터셋이 책임 있는 방식으로 수집, 선정, 라벨링, 변환, 관리되는지를 체계적으로 검토하는 절차를 구축한다. 이러한 감사는 기술적인 데이터 품질(Data Quality)을 넘어 대표성(Representation), 개인정보 보호(Privacy), 출처(Provenance), 동의 또는 권한(Consent or Authorization), 유해 콘텐츠(Harmful Content), 사용 제한(Usage Restriction), 잠재적인 하류 영향(Downstream Consequence)을 검토한다. 목적은 모델 개발 전에 학습 데이터가 정의된 윤리 및 거버넌스 통제를 통과했다는 추적 가능한 증거를 구축하는 것이다.

로봇 학습 데이터셋(Robot Training Dataset)은 사람, 물리적 환경, 작업장, 차량, 인프라, 인간 행동, 인간과 로봇 사이의 상호작용을 빈번하게 표현한다는 점에서 일반적인 정적 데이터셋(Static Dataset)과 다르다. 이미지, 비디오, 오디오, 궤적(Trajectory), 텔레메트리(Telemetry), 지도(Map), 조작 시연(Manipulation Demonstration), 멀티모달 기록(Multimodal Recording)은 민감한 상황 정보를 포함할 수 있다. 따라서 윤리 검토(Ethical Review)는 개별 데이터 요소뿐만 아니라 해당 관측 정보가 생성된 전체 물리적 상황까지 함께 검토해야 한다.

감사 프레임워크(Audit Framework)는 데이터셋 출처(Dataset Provenance)에서 시작해야 한다. 모든 중요한 데이터셋은 출처, 수집 방법, 획득 기간, 책임 조직, 적용 가능한 라이선스(License) 또는 사용 조건, 주요 처리 단계를 식별해야 한다. 내부에서 수집한 로봇 데이터는 로봇, 센서, 임무(Mission), 수집 환경과 연결되어야 하며, 외부에서 획득한 데이터셋은 원천 및 라이선스 정보를 유지해야 한다. 출처가 불분명하거나 충분히 문서화되지 않은 데이터는 학습 과정에 그대로 포함하기보다 거버넌스 위험(Governance Risk)으로 처리해야 한다.

목적 평가(Purpose Assessment)는 의도된 AI 학습 활용이 데이터가 처음 수집되거나 획득된 목적과 일치하는지를 판단한다. 내비게이션(Navigation), 유지보수(Maintenance), 사고 분석(Incident Analysis)을 위해 수집된 운영 로봇 기록이 자동으로 제한 없는 학습 데이터가 되어서는 안 된다. 감사 과정에서는 제안된 학습 목적, 허용된 사용 범위, 제한 사항, 책임 소유자(Responsible Owner), 승인 상태(Approval Status)를 기록하여 데이터셋을 사용할 수 있다는 사실이 모든 모델 개발 활동에 대한 사용 권한으로 잘못 해석되지 않도록 해야 한다.

대표성 분석(Representation Analysis)은 데이터셋이 의도된 로봇 행동에 필요한 환경, 객체, 운영 조건, 상호작용을 적절하게 포함하는지를 검토한다. 하나의 시설, 조명 조건, 바닥 유형, 기상 조건 또는 객체 분포에 집중하여 학습된 모델은 다른 환경에서 서로 다른 성능을 나타낼 수 있다. 따라서 윤리 감사는 중요한 데이터 범위의 한계(Coverage Limitation)를 식별하고 실제 배포에서 중요한 상황이 학습 데이터에서 체계적으로 부족할 수 있는 영역을 문서화해야 한다.

로봇 데이터에 사람이 포함되는 경우 인간 대표성(Human Representation)은 추가적인 주의가 필요하다. 학습 기록은 신체적 특성, 의복, 이동 패턴(Mobility Pattern), 작업장 역할, 환경적 상황, 상호작용 방식 등에 따라 달라질 수 있다. 감사의 목적은 모든 데이터셋이 모든 인구집단(Population)을 동일하게 대표할 수 있다고 가정하는 것이 아니라, 의도된 사용 목적에 적합한 대표성을 확보했는지와 알려진 한계가 실제 배포 과정에서 불균형한 모델 행동으로 이어질 가능성이 있는지를 검토하는 것이다.

편향 감사(Bias Audit)는 데이터셋의 불균형(Dataset Imbalance)과 실제로 확인된 모델 행동(Model Behavior)을 구분해야 한다. 클래스 빈도(Class Frequency), 샘플링 방식(Sampling Practice), 어노테이션 결정(Annotation Decision), 센서 배치(Sensor Placement), 지리적 집중(Geographic Concentration), 데이터 수집 절차는 잠재적인 편향의 원인을 나타낼 수 있지만 데이터셋 통계만으로 배포된 모델이 불공정하게 행동한다는 사실을 입증할 수는 없다. 따라서 감사 보고서는 확인된 불균형, 가능한 위험, 필요한 평가를 문서화하고 불완전한 증거를 근거로 모델 결과에 대한 근거 없는 결론을 내려서는 안 된다.

어노테이션 윤리(Annotation Ethics)는 라벨(Label)이 관측된 상황에 대한 사람 또는 기계의 해석을 포함하기 때문에 중요하다. 감사자는 어노테이션 지침(Annotation Guideline), 라벨 정의(Label Definition), 품질 관리 절차(Quality-Control Procedure), 어노테이터 지침(Annotator Instruction), 자동 라벨링 방식(Automated Labeling Method), 의견 불일치 처리, 수정 이력(Correction History)을 검토해야 한다. 모호하거나 주관적인 범주는 원본 센서 관측이 기술적으로 정확하더라도 일관되지 않은 정의가 모델 행동에 내재될 수 있으므로 특별한 주의가 필요하다.

개인정보 보호 검토(Privacy Review)는 학습 데이터에 얼굴, 음성, 위치, 상호작용 이력, 식별자(Identifier), 반복적인 행동 관측 등 개인을 식별할 수 있거나 잠재적으로 식별 가능한 정보가 포함되어 있는지를 판단해야 한다. 감사에서는 관련 데이터 분류, 승인된 목적, 마스킹(Masking) 또는 가명처리(Pseudonymization), 보존 요구사항(Retention Requirement), 접근 제한(Access Restriction)을 확인해야 한다. 개인정보 보호 처리가 적용된 데이터셋 역시 어떤 보호 조치가 적용되었는지를 설명하는 메타데이터(Metadata)를 유지하여 원래 처리 상황에 대한 이력을 잃지 않아야 한다.

민감하고 유해한 콘텐츠(Sensitive and Harmful Content)는 데이터셋 워크플로(Dataset Workflow)에서 통제된 방식으로 처리해야 한다. 로봇 기록에는 사고, 위험한 행동, 기밀 문서, 화면 정보, 사적인 대화, 제한 시설 또는 제한 없이 학습에 사용하는 것이 부적절한 자료가 의도하지 않게 포함될 수 있다. 탐지 절차(Detection Procedure)를 통해 이러한 기록을 식별하고 조직 정책과 해당 애플리케이션의 정당한 요구사항에 따라 검토, 격리(Quarantine), 제외, 변환 또는 특별히 통제된 사용 대상으로 지정할 수 있다.

데이터셋 라이선스(Dataset Licensing)와 지식재산권(Intellectual Property) 조건은 학습을 시작하기 전에 감사해야 한다. 데이터가 공개적으로 이용 가능하다는 사실이 복사, 수정, 재배포, 상업적 사용 또는 모델 학습에 대한 무제한 권한을 의미하지는 않는다. 데이터셋 기록에는 라이선스, 계약상 제한(Contractual Restriction), 저작자 표시 요구사항(Attribution Requirement), 원천 이용 조건, 내부 승인 정보를 유지해야 한다. 여러 원천을 결합할 경우 최종 학습 데이터셋은 각각의 구성요소에 적용되는 조건까지 추적할 수 있어야 한다.

데이터 리니지(Data Lineage)는 반복 가능한 윤리 감사에 필요한 기술적 증거 사슬(Evidence Chain)을 제공한다. 원시 기록(Raw Recording)은 모델 학습에 사용되기 전에 필터링(Filtering), 개인정보 보호 처리, 어노테이션, 데이터 증강(Augmentation), 균형 조정(Balancing), 분할(Partitioning), 데이터셋 조립(Dataset Assembly)을 거칠 수 있다. 리니지는 각 데이터셋 버전(Dataset Version)을 이러한 변환 과정 및 최종 실험과 모델 버전에 연결하여 특정 모델에 어떤 원천 정보가 사용되었으며 어떠한 통제가 적용되었는지를 감사자가 재구성할 수 있도록 해야 한다.

합성 데이터(Synthetic Data) 역시 윤리 프레임워크에 포함되어야 한다. 시뮬레이션(Simulation)과 생성 프로세스(Generative Process)는 희귀한 조건의 데이터 범위를 확대하고 일부 실제 환경 기록에 대한 의존성을 줄일 수 있지만, 비현실적인 분포, 원천 데이터에서 상속된 특성, 시뮬레이션 설계에 포함된 숨겨진 가정을 도입할 수 있다. 감사에서는 생성 방식, 원천 데이터 의존성(Source Dependency), 시나리오 범위(Scenario Coverage), 검증 절차(Validation Procedure), 합성 데이터와 실제 데이터 사이의 의도된 관계를 문서화해야 한다.

데이터셋 변환(Dataset Transformation)은 윤리적 위험을 변화시킬 수 있으므로 감사 가능해야 한다. 크로핑(Cropping)은 상황 정보를 제거할 수 있고, 균형 조정은 관측된 분포를 변경할 수 있으며, 데이터 증강은 비현실적인 사례를 생성할 수 있고, 필터링은 어려운 사례를 체계적으로 제거할 수 있다. 변환 매개변수(Transformation Parameter), 소프트웨어 버전(Software Version), 선택 기준(Selection Criteria), 변환 이후의 데이터셋 통계를 보존하여 최종 학습 데이터 분포가 원래 수집된 정보와 어떻게 달라졌는지를 검토할 수 있도록 해야 한다.

윤리 감사는 AI 데이터 수명주기(AI Data Lifecycle)에 명확한 검토 게이트(Review Gate)를 설정해야 한다. 데이터셋은 원시 수집(Raw Collection)에서 정제 데이터(Curated Data), 승인된 학습 데이터(Approved Training Data), 검증된 릴리스(Validated Release)를 거쳐 최종적으로 아카이브 또는 폐기 상태(Deprecated Status)로 이동할 수 있다. 각 단계로 이동하기 전에 출처, 개인정보 보호, 라이선스, 품질, 대표성, 문서화 검사를 완료하도록 요구할 수 있다. 자동화된 게이트는 필수 메타데이터가 누락된 데이터셋을 차단하고, 규칙만으로 신뢰성 있게 판단할 수 없는 문제는 사람의 검토(Human Review)를 통해 처리할 수 있다.

위험 분류(Risk Classification)는 필요한 검토의 깊이를 결정하는 데 도움이 된다. 민감하지 않은 기계 텔레메트리로 구성된 소규모 내부 데이터셋은 비교적 간단한 통제로 충분할 수 있지만, 사람을 포함하는 대규모 비디오 데이터나 안전 관련 로봇 행동을 지원하는 데이터는 보다 광범위한 평가가 필요할 수 있다. 따라서 감사 프로세스는 모든 데이터셋에 동일한 절차를 적용하기보다 데이터 민감도, 배포 결과(Deployment Consequence), 불확실성(Uncertainty), 조직 정책에 따라 검토 수준을 조정해야 한다.

감사 증거(Audit Evidence)는 분리된 보고서에만 저장하지 않고 해당 데이터셋 버전과 함께 보존해야 한다. 증거에는 출처 기록, 품질 결과, 개인정보 보호 상태, 라이선스 정보, 대표성 통계(Representation Statistics), 검토 결정, 알려진 한계(Known Limitation), 승인 사항, 해결되지 않은 문제가 포함될 수 있다. 새로운 데이터 원천, 어노테이션, 필터링 규칙, 변환 과정이 추가되면 데이터셋 자체가 크게 달라질 수 있으므로 버전별 증거(Version-Specific Evidence)를 유지하는 것이 중요하다.

책임(Responsibility)은 명확하게 정의된 여러 역할에 분산되어야 한다. 데이터 소유자(Data Owner)는 목적과 출처를 확인하고, 엔지니어링 팀은 수집 및 변환 과정을 설명하며, 개인정보 보호 및 보안 팀은 민감한 정보를 평가하고, 도메인 전문가(Domain Expert)는 운영 관련성을 검토하며, AI 팀은 대표성과 모델에 미치는 영향을 분석할 수 있다. 명확한 소유권(Ownership)은 모든 사람이 다른 팀이 이미 수행했을 것이라고 가정하면서 윤리 검토가 비공식적인 책임으로 남는 것을 방지한다.

학습 후 검토(Post-Training Review)는 데이터셋 거버넌스와 실제로 관찰된 모델 행동을 연결한다. 평가 결과를 통해 데이터셋 검토 과정에서는 명확하지 않았던 특정 환경, 객체, 상호작용 시나리오 또는 데이터 분포와 관련된 실패 패턴(Failure Pattern)이 발견될 수 있다. 이러한 결과는 데이터 카탈로그(Data Catalog), 리니지, 데이터셋 문서(Dataset Documentation), 향후 데이터 수집 계획에 다시 반영되어야 한다. 따라서 윤리 감사는 최초 학습 전에 한 번 수행하는 승인 절차가 아니라 반복적인 수명주기 프로세스(Iterative Lifecycle Process)가 된다.

배포 이후에도 운영 경험을 통해 새로운 데이터 한계가 발견될 수 있으므로 지속적인 모니터링(Monitoring)이 필요하다. 플릿 사고(Fleet Incident), 엣지 케이스(Edge Case), 사람의 피드백(Human Feedback), 성능 드리프트(Performance Drift), 새롭게 발견된 환경은 기존 학습 데이터 분포의 부족한 영역을 나타낼 수 있다. 재학습(Retraining)을 위해 추가 데이터를 수집할 경우 이미 모델이 존재한다는 이유로 거버넌스를 우회하지 않고 새로운 데이터에도 동일한 출처, 개인정보 보호, 대표성, 라이선스, 품질, 승인 통제를 적용해야 한다.

성숙한 AI 학습 데이터 윤리 감사 프레임워크(AI Training Data Ethics Audit Framework)는 궁극적으로 데이터 카탈로그, 데이터 리니지, 데이터 품질 거버넌스(Data Quality Governance), 개인정보 보호, 접근 제어(Access Control), 데이터 보존(Data Retention), 문서화(Documentation), 사람의 검토, 모델 피드백(Model Feedback)을 지속적인 증거 기반 프로세스(Evidence-Based Process)로 통합한다. 이러한 프레임워크가 AI 시스템에서 발생할 수 있는 모든 윤리적 문제를 완전히 제거한다고 보장할 수는 없지만, 가정(Assumption), 한계, 의사결정, 책임을 명확하게 드러낸다. 이러한 추적 가능성(Traceability)은 확장 가능한 로봇 및 피지컬 AI 시스템을 보다 책임 있게 개발할 수 있도록 지원한다.

## 09.09 Robot Data Security Classification and Encryption

![](images/image9.png){width="7.268055555555556in" height="7.268055555555556in"}

로봇 데이터 보안 분류(Security Classification)는 다양한 유형의 로봇 정보를 어느 수준으로 보호해야 하는지를 결정하기 위한 체계적인 방법을 제공한다. 피지컬 AI(Physical AI) 시스템은 텔레메트리(Telemetry), 카메라 이미지(Camera Image), 라이다 포인트 클라우드(LiDAR Point Cloud), 지도(Map), 로봇 구성 정보(Robot Configuration), 내비게이션 기록(Navigation Record), 로그(Log), 어노테이션(Annotation), 학습 데이터셋(Training Dataset), 모델 아티팩트(Model Artifact), 운영 명령(Operational Command) 등을 생성할 수 있다. 이러한 자산이 모두 동일한 보안 요구사항을 갖는 것은 아니다. 따라서 분류(Classification)는 전체 데이터 수명주기(Data Lifecycle)에서 적절한 접근 제어(Access Control), 암호화(Encryption), 보존(Retention), 모니터링(Monitoring), 처리 정책(Handling Policy)을 적용하기 위한 기반을 제공한다.

보안 분류는 민감도(Sensitivity), 운영 중요성(Operational Importance), 비즈니스 가치(Business Value), 개인정보 보호 영향(Privacy Implication), 비인가 공개 또는 변경(Unauthorized Disclosure or Modification)으로 인해 발생할 수 있는 잠재적 결과를 고려해야 한다. 공개된 운영 통계(Public Operational Statistic)는 제한적인 보호만 필요할 수 있지만, 독점적인 로봇 구성 정보(Proprietary Robot Configuration), 시설 지도(Facility Map), 원시 센서 기록(Raw Sensor Recording), 인증정보(Credential), 안전 기록(Safety Record), AI 학습 데이터는 더 강력한 통제가 필요할 수 있다. 분류는 단순히 데이터가 어디에 저장되거나 어떤 애플리케이션에서 생성되었는지를 나타내는 것이 아니라, 해당 데이터가 침해되었을 때 발생할 수 있는 결과를 설명해야 한다.

실용적인 분류 모델(Practical Classification Model)은 조직의 정책에 따라 공개(Public), 내부(Internal), 기밀(Confidential), 제한(Restricted), 고도로 민감한 보안 또는 안전 정보(Highly Sensitive Security or Safety Information)와 같은 범주를 구분할 수 있다. 정확한 분류 명칭은 데이터 카탈로그(Data Catalog), 접근 제어 시스템(Access-Control System), 저장 플랫폼(Storage Platform), 운영 절차(Operational Procedure) 전반에서 일관되게 유지되어야 한다. 분류는 데이터셋이 생성되거나 등록될 때 중요한 메타데이터(Metadata)와 함께 지정하고, 목적, 콘텐츠, 소유권(Ownership), 처리 방법, 운영 환경이 변경될 경우 다시 검토해야 한다.

보안 분류를 결정할 때는 로봇 특화 문맥(Robot-Specific Context)이 중요하다. 비어 있는 실험실에서 촬영된 카메라 기록은 사람, 기밀 문서, 생산 장비 또는 제한된 시설을 포함하는 기록과 민감도가 다를 수 있다. 마찬가지로 지도는 일반적인 공간 데이터(Spatial Data)처럼 보일 수 있지만 보호 대상 인프라(Protected Infrastructure)를 나타내는 경우 매우 민감해질 수 있다. 따라서 보안 분류는 정보 자체뿐만 아니라 해당 정보가 표현하는 물리적 환경(Physical Environment)까지 고려해야 한다.

데이터 분류(Data Classification)는 소유권과 목적(Purpose)에 연결되어야 한다. 중요한 데이터 자산은 해당 정보가 왜 존재하는지, 누가 사용해야 하는지, 어느 수준의 보호가 필요한지를 결정할 수 있는 책임 소유자(Responsible Owner)를 식별해야 한다. 목적 정보는 운영 텔레메트리, 안전 증거(Safety Evidence), 연구 데이터, 고객 정보, AI 학습 자료를 구분하는 데 도움이 된다. 이러한 연결은 보안 라벨(Security Label)이 실제 데이터 사용 방식을 더 이상 반영하지 못하는 정적인 기술 태그(Static Technical Tag)가 되는 것을 방지한다.

접근 제어는 분류 결정을 단순한 문서가 아니라 실제 정책으로 집행해야 한다. 제한 데이터셋(Restricted Dataset)은 내부 데이터셋(Internal Dataset)보다 강력한 인증(Authentication), 더 좁은 역할(Role), 승인 워크플로(Approval Workflow), 더욱 상세한 감사 로깅(Audit Logging)을 요구할 수 있다. 역할 기반 접근 제어(Role-Based Access Control, RBAC)는 사용자와 서비스의 권한을 데이터 분류에서 파생된 정책과 연결할 수 있으며, 최소 권한(Least Privilege)은 정당한 업무에 필요한 정보만 접근하도록 제한한다.

암호화는 분류된 로봇 데이터에 대한 기술적 보호 계층(Technical Protection Layer)을 제공한다. 분류 수준과 위협 모델(Threat Model)이 요구하는 경우 데이터는 저장 중(Data at Rest)과 전송 중(Data in Transit) 모두 보호되어야 한다. 원시 센서 기록, 기밀 구성 정보, 인증정보, 민감한 데이터셋은 저위험 운영 통계보다 강력한 암호화 제어를 요구할 수 있다. 암호화 키(Encryption Key)는 보호 대상 데이터와 분리하여 관리하고, 접근 제어, 키 교체(Key Rotation) 절차, 감사 가능한 키 관리 작업(Key-Management Operation)을 적용해야 한다.

로봇, 엣지 장치(Edge Device), 온프레미스 서버(On-Premise Server), 클라우드 인프라(Cloud Infrastructure), 데이터베이스, 오브젝트 스토리지(Object Storage), AI 플랫폼 사이를 이동하는 데이터는 해당 분류 수준에 따라 보호되어야 한다. 전송 암호화(Transport Encryption)는 네트워크를 통과하는 동안 정보를 보호하고, 저장 암호화(Storage Encryption)는 영구적으로 저장된 복사본을 보호할 수 있다. 내부 네트워크에 존재한다는 사실만으로 충분한 보호가 제공된다고 가정해서는 안 된다. 내부 서비스나 계정이 침해될 경우 민감한 정보에 접근할 가능성이 있기 때문이다.

분류는 보존 및 삭제 정책(Retention and Deletion Policy)에도 영향을 주어야 한다. 매우 민감한 기록은 장기간 보존을 정당화하는 운영적, 법적, 안전 또는 분석 목적이 명확하게 문서화되지 않는 한 더 짧은 보존 기간을 요구할 수 있다. 아카이브된 데이터(Archived Data)는 활성 처리 단계에서와 동일한 광범위한 접근 권한을 자동으로 유지해서는 안 된다. 수명주기 정책(Lifecycle Policy)은 분류 정보를 목적 및 보존 규칙과 결합하여 승인된 사용 기간이 종료되면 데이터를 아카이빙, 익명화 또는 삭제할 수 있다.

분류된 정보가 파생 데이터 제품(Derived Product)으로 변환되는 경우 데이터 리니지(Data Lineage)가 중요하다. 원시 카메라 기록은 선택된 프레임, 익명화된 이미지, 어노테이션, 데이터셋, 피처(Feature), AI 모델을 생성할 수 있다. 각각의 변환이 원래의 민감도를 제거한다고 가정하지 말고 각 파생 자산의 보안 특성을 평가해야 한다. 리니지를 통해 거버넌스 팀은 민감한 정보가 어디에서 시작되었는지, 어떻게 변환되었는지, 어떤 하류 데이터 자산(Downstream Data Asset)에 추가적인 보호가 필요한지를 확인할 수 있다.

보안 분류는 AI 학습 데이터셋과 모델 개발 아티팩트(Model-Development Artifact)에도 적용되어야 한다. 학습 데이터셋에는 독점적인 운영 정보, 개인정보, 민감한 환경 정보 또는 라이선스가 적용된 자료가 포함될 수 있으며, 모델 체크포인트(Model Checkpoint)와 실험 결과(Experiment Result)는 학습 원천에서 유래한 정보를 간접적으로 보존할 수도 있다. 따라서 데이터셋 버전, 모델 아티팩트, 실험 기록, 배포 패키지(Deployment Package)는 해당 콘텐츠, 목적, 비인가 접근으로 인해 발생할 수 있는 결과에 적합한 분류를 부여해야 한다.

모니터링과 감사는 보안 제어가 의도한 대로 작동하고 있다는 증거를 제공한다. 중요한 이벤트에는 제한 데이터 접근, 권한 변경, 암호화 키 작업, 대규모 데이터 내보내기(Large Export), 예상하지 못한 데이터 전송, 분류 변경, 관리자 작업 등이 포함된다. 감사 기록은 행위자(Actor), 자원(Resource), 작업(Action), 시간(Time), 결과(Result), 관련 문맥(Context)을 식별해야 한다. 모니터링 시스템은 이러한 정보를 이용하여 비정상적인 패턴을 탐지하고 민감한 로봇 정보가 예상하지 못한 방식으로 접근되거나 전송되는 경우 조사를 지원할 수 있다.

보안 분류는 데이터 카탈로그와 통합되어 원본 콘텐츠 자체를 불필요하게 노출하지 않으면서 거버넌스 정보를 제공해야 한다. 카탈로그 기록에는 분류, 소유자, 목적, 보존 요구사항, 접근 제한, 암호화 상태(Encryption Status), 승인된 처리 절차(Approved Handling Procedure)를 표시할 수 있다. 이를 통해 엔지니어와 데이터 사용자는 데이터셋을 요청하거나 처리하기 전에 해당 데이터의 보안 조건을 이해할 수 있으며, 분산된 로봇 데이터 인프라 전반에서 공통적인 기준으로 활용할 수 있다.

분류는 한 번 지정하고 끝나는 작업이 아니라 수명주기 프로세스(Lifecycle Process)로 운영되어야 한다. 새로운 센서, 소프트웨어 릴리스(Software Release), 배포 환경(Deployment Environment), 외부 데이터 소스, AI 애플리케이션, 비즈니스 요구사항은 기존 정보의 민감도를 변경할 수 있다. 정기적인 검토와 이벤트 기반 재평가(Event-Driven Reassessment)를 통해 분류가 오래된 데이터 자산을 식별할 수 있다. 자동화된 정책 검사는 분류되지 않은 자산, 부적절한 저장 위치, 누락된 암호화, 과도한 권한, 할당된 보안 수준과 일치하지 않는 데이터 전송을 탐지할 수 있다.

성숙한 로봇 데이터 보안 아키텍처(Mature Robot Data Security Architecture)는 분류, 신원(Identity), 접근 제어, 암호화, 리니지, 보존, 모니터링, 감사를 하나의 통합된 보호 시스템(Coordinated Protection System)으로 결합한다. 분류는 필요한 보호 수준을 결정하고, 접근 제어는 누가 데이터를 사용할 수 있는지를 결정하며, 암호화는 저장 및 전송 과정에서 정보를 보호하고, 리니지는 데이터의 전파 과정을 추적하며, 수명주기 정책은 데이터가 얼마 동안 사용 가능한 상태로 남아 있는지를 관리한다. 이러한 메커니즘을 결합하면 확장 가능한 엔지니어링 및 운영에 필요한 데이터 가용성과 추적 가능성(Traceability)을 유지하면서 로봇 및 피지컬 AI 데이터(Physical AI Data)를 보호할 수 있는 보안 프레임워크(Security Framework)를 구축할 수 있다.

## 09.10 Data Governance Maturity Model and Roadmap

![](images/image10.png){width="7.268055555555556in" height="7.268055555555556in"}

로봇 및 피지컬 AI 시스템(Robot and Physical AI System)의 데이터 거버넌스 성숙도(Data Governance Maturity)는 비공식적이고 단편적인 데이터 관리 방식에서 통합되고 측정 가능하며 지속적으로 개선되는 거버넌스 역량으로 조직이 발전하는 과정을 설명한다. 성숙도 모델(Maturity Model)은 수집, 저장, 처리, AI 학습, 디지털 트윈(Digital Twin), 공유, 보존, 보안, 삭제를 포함하는 전체 로봇 데이터 수명주기(Robot Data Lifecycle)와 거버넌스를 연결해야 한다. Volume 07 구조에서 데이터 거버넌스(Data Governance)는 데이터 모델링, 파이프라인, 텔레메트리, 이벤트 스트리밍, 시계열, 센서, AI, 디지털 트윈 아키텍처를 뒤따르며, 이러한 선행 데이터 역량을 조정하는 제어 계층(Control Layer)으로 자리한다.

초기 성숙도 상태(Initial Maturity State)는 단편화된 소유권과 대부분 수동으로 이루어지는 데이터 관리가 특징이다. 로봇 데이터는 온보드 스토리지(Onboard Storage), 엣지 컴퓨터(Edge Computer), NAS 시스템, 데이터베이스, 클라우드 플랫폼, 개발 환경 등에 존재할 수 있지만 일관된 메타데이터나 책임 체계가 없을 수 있다. 각 팀은 자신이 관리하는 데이터가 어디에 있는지는 알고 있지만 공통 카탈로그, 리니지(Lineage), 분류(Classification), 보존 정책(Retention Policy), 품질 표준(Quality Standard)이 부족할 수 있다. 이 단계의 거버넌스는 주로 사후 대응적(Reactive)이며, 데이터가 이미 재사용되거나 이전되거나 AI 워크플로에 포함된 이후에 문제가 발견되는 경우가 많다.

정의된 거버넌스 단계(Defined Governance Stage)에서는 공통 용어, 소유권, 정책, 최소 운영 절차(Minimum Operating Procedure)가 수립된다. 데이터 자산에는 표준화된 이름, 식별자, 분류, 소유자, 목적, 수명주기 정보가 부여되기 시작한다. 기본 정책은 로봇 텔레메트리, 센서 기록, 지도, 어노테이션, 학습 데이터셋, 운영 기록을 어떻게 수집하고 처리해야 하는지를 정의할 수 있다. 이 단계의 목표는 즉시 최대한 복잡한 체계를 도입하는 것이 아니라, 서로 다른 엔지니어링 팀이 공통 조직 규칙에 따라 데이터를 관리할 수 있도록 일관된 기반을 만드는 것이다.

관리되는 단계(Managed Stage)에서는 측정 가능한 제어와 반복 가능한 거버넌스 프로세스가 도입된다. 데이터 카탈로그(Data Catalog)는 중요한 데이터 자산에 대한 가시성을 제공하고, 리니지 기록(Lineage Record)은 원천 데이터와 파생 데이터 사이의 관계를 기록하며, 데이터 품질 SLA(Data Quality SLA)는 완전성, 유효성, 최신성, 일관성 및 기타 품질 요소에 대한 측정 가능한 기대 수준을 정의한다. 접근 제어(Access Control), 개인정보 보호(Privacy Protection), 보존 자동화(Retention Automation), 보안 분류(Security Classification)는 단순한 문서가 아니라 실제 운영 메커니즘으로 발전한다. 거버넌스는 데이터 파이프라인 전반에서 데이터 정책이 실제로 적용되고 있다는 증거를 생성하기 시작한다.

통합 단계(Integrated Stage)에서는 거버넌스 제어가 전체 데이터 아키텍처와 연결된다. 카탈로그 메타데이터는 접근 정책에 영향을 줄 수 있고, 분류 정보는 암호화 및 보존 요구사항을 결정할 수 있으며, 리니지는 개인정보 삭제 및 영향 분석(Impact Analysis)을 지원할 수 있다. 품질 모니터링(Quality Monitoring)은 신뢰할 수 없는 데이터셋이 AI 학습이나 운영 분석에 사용되기 전에 이를 식별할 수 있다. AI 데이터 거버넌스(AI Data Governance)는 학습 데이터 버전을 출처(Provenance), 어노테이션, 실험, 모델 아티팩트(Model Artifact)와 연결할 수 있다. 따라서 거버넌스는 별도의 행정 기능으로 운영되는 것이 아니라 엔지니어링 워크플로와 통합된다.

고급 성숙도 상태(Advanced Maturity State)에서는 지속적인 측정, 자동화된 정책 집행, 위험 기반 거버넌스(Risk-Based Governance)가 도입된다. 시스템은 분류되지 않은 자산, 누락된 메타데이터, 만료된 정보, 과도한 권한, 품질 위반, 예상하지 못한 데이터 전송, 보안 이상(Security Anomaly)을 자동으로 탐지할 수 있다. 거버넌스 대시보드(Governance Dashboard)는 규정 준수 및 운영 상태를 보여주며, 자동화된 워크플로는 정의된 조건이 발생하면 검토, 격리(Quarantine), 개선(Remediation), 아카이빙 또는 삭제를 시작할 수 있다. 자동화된 규칙만으로 안전하게 판단하기 어려운 모호하거나 영향이 큰 의사결정에는 사람의 검토(Human Review)가 여전히 중요하다.

성숙도 모델은 조직 전체에 하나의 성숙도 값을 부여하기보다 서로 연결된 여러 차원에서 거버넌스를 평가해야 한다. 데이터 탐색 및 카탈로그화(Data Discovery and Cataloging)는 중요한 자산이 얼마나 잘 가시화되어 있는지를 측정한다. 리니지는 데이터 이동과 변환 과정을 얼마나 재구성할 수 있는지를 측정한다. 품질(Quality)은 데이터가 정의된 서비스 기대 수준을 충족하는지를 측정한다. 개인정보 보호와 접근 제어는 민감한 정보가 적절하게 보호되는지를 측정한다. 보존 및 삭제는 수명주기 규율(Lifecycle Discipline)을 측정하며, 보안 분류와 암호화는 비인가 공개 또는 변경으로부터 데이터를 보호하는 수준을 측정한다.

Hills 로드맵(Hills Roadmap)은 이러한 성숙도 모델을 활용하여 거버넌스 원칙을 단계적인 구현 순서로 전환할 수 있다. 초기 작업에서는 광범위한 자동화를 시도하기 전에 소유권, 분류, 메타데이터 표준(Metadata Standard), 기본 정책을 수립해야 한다. 다음 단계에서는 카탈로그와 리니지 역량을 우선적으로 구축하고, 이후 품질 모니터링, 개인정보 보호 제어, RBAC, 보존 자동화, AI 학습 데이터 윤리 감사(Ethics Audit), 보안 분류를 단계적으로 도입할 수 있다. 이러한 순서를 통해 거버넌스 역량은 실제 운영과 분리된 독립적인 거버넌스 도구가 아니라 기반이 되는 로봇 데이터 아키텍처와 함께 성숙할 수 있다.

Hills의 경우 거버넌스는 기존의 일반적인 엔터프라이즈 데이터뿐만 아니라 피지컬 AI 데이터의 특성을 중심으로 설계되어야 한다. 로봇 시스템은 멀티모달 센서 정보(Multimodal Sensor Information), 텔레메트리, 공간 정보(Spatial Information), 운영 이벤트(Operational Event), AI 학습 데이터, 디지털 트윈 상태, 파생된 모델 아티팩트를 지속적으로 생성한다. 이러한 데이터 제품(Data Product)은 로봇, 엣지 환경, 온프레미스 인프라, 클라우드 시스템, AI 학습 플랫폼, 외부 파트너 사이를 이동할 수 있다. 따라서 로드맵은 이러한 경계 사이의 추적 가능성(Traceability)을 강조하고 원천 데이터, 변환, 모델, 운영 활용 사이의 명확한 관계를 유지해야 한다.

실질적인 Hills 구현에서는 Chapter 09 전체에서 개발되는 데이터 카탈로그, 리니지, 품질, 개인정보 보호, 접근 제어, 보존, 윤리, 보안 역량을 연결하는 거버넌스 제어 평면(Governance Control Plane)을 구축할 수 있다. 카탈로그는 가시성을 제공하고, 리니지는 추적 가능성을 제공하며, 품질은 측정 가능한 신뢰성을 제공하고, 개인정보 보호와 접근 제어는 사용을 규제하며, 보존은 수명주기를 관리하고, 윤리 감사는 AI 학습 데이터를 관리하며, 보안 분류는 보호 요구 수준을 결정한다. 이러한 제어 기능을 결합하면 거버넌스는 독립적인 규정 준수 메커니즘의 집합이 아니라 로봇 데이터 아키텍처 상부에서 작동하는 통합된 거버넌스 계층을 형성한다.

성숙도 역시 단순히 문서나 도구가 존재하는지를 기준으로 측정해서는 안 되며 실제 증거를 기반으로 평가해야 한다. 유용한 증거에는 소유자가 식별된 중요 데이터셋의 비율, 카탈로그에 등록된 자산의 비율, 리니지 적용 범위(Lineage Coverage), 품질 SLA 준수율, 분류 적용 범위, 성공적으로 실행된 보존 작업, 접근 권한 검토 완료율, 개인정보 보호 제어 적용 범위, 해결되지 않은 거버넌스 예외(Governance Exception)의 수 등이 포함될 수 있다. 이러한 지표를 통해 Hills는 거버넌스가 실제 운영 역량으로 발전하고 있는지를 판단하고 추가적인 엔지니어링 노력이 필요한 영역을 식별할 수 있다.

로드맵은 로봇 및 피지컬 AI 아키텍처가 지속적으로 변화하기 때문에 반복적으로 운영되어야 한다. 새로운 센서, 로봇 플랫폼, AI 모델, 시뮬레이션 환경, 합성 데이터셋, 디지털 트윈, 배포 방식은 새로운 거버넌스 요구사항을 발생시킬 수 있다. 따라서 성숙도 모델은 성숙도를 영구적인 지정 상태로 취급하기보다 정기적인 재평가(Periodic Reassessment)와 이벤트 기반 업데이트(Event-Driven Update)를 지원해야 한다. 조직이 개별 로봇 프로젝트에서 다중 로봇 및 확장 가능한 피지컬 AI 운영으로 발전함에 따라 거버넌스 정책, 기술적 제어, 소유권 모델, 자동화 역시 함께 발전해야 한다.

목표 상태(Target State)는 최대한 복잡한 거버넌스를 구축하는 것이 아니라 비례적이고(Proportional), 자동화되며(Automated), 추적 가능하고(Traceable), 엔지니어링 운영에 유용한 거버넌스를 구축하는 것이다. 성숙한 Hills 데이터 거버넌스 역량은 팀이 신뢰할 수 있는 데이터를 발견하고, 데이터의 출처와 품질을 이해하며, 누가 데이터를 사용할 수 있는지 판단하고, 민감한 정보를 보호하며, 정당한 증거를 보존하고, 불필요한 데이터를 제거하며, 중요한 의사결정을 감사할 수 있도록 해야 한다. 이를 통해 로봇 데이터가 수집, 처리, AI 학습, 배포, 지속적인 개선의 전 과정을 안전하게 이동하면서 전체 피지컬 AI 수명주기에서 책임성을 유지할 수 있는 기반을 구축할 수 있다.
