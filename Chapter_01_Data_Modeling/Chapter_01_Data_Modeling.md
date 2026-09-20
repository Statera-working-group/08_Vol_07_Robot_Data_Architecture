**Volume 07 Robot Data Architecture**


# 01. Data Modeling

##  

## 01.01 Robot Data Modeling Principles: Domain-Driven Design

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

Robot data modeling begins with the recognition that a robot is not merely a collection of sensors, actuators, and software nodes. It is a physical system that continuously changes state while interacting with people, equipment, infrastructure, and other robots. A useful data model must therefore represent identity, configuration, operational state, observations, actions, missions, faults, and environmental context as connected domain concepts.

Domain-Driven Design provides a practical foundation for organizing this complexity because it starts from the operational meaning of data rather than database technology. Instead of designing schemas directly from ROS 2 messages, device registers, API payloads, or database tables, engineers first identify the concepts that matter to the robot domain. Terms such as Robot, Mission, Task, Pose, Sensor, Battery, Fault, Map, and Fleet become part of a shared domain language.

This shared vocabulary is commonly described as a ubiquitous language. Software engineers, robotics engineers, AI developers, operators, and business teams should use important terms consistently across specifications, source code, APIs, telemetry, databases, and operational dashboards. If "robot state" means navigation mode in one subsystem and overall operational condition in another, integration becomes ambiguous. Explicit definitions reduce semantic inconsistency as the platform grows.

A robot domain should also be divided into bounded contexts so that one universal data model does not attempt to represent every concern. Robot Control may describe velocity, actuator commands, emergency stops, and controller modes, while Fleet Management represents assignments, missions, availability, and traffic coordination. Maintenance can independently model components, diagnostic events, service records, degradation indicators, and replacement history.

Bounded contexts are especially important because the same physical object can have different representations depending on its purpose. A battery in real-time control may be represented by voltage, current, temperature, and state of charge. In maintenance, the same battery may be represented by serial number, installation date, cycle count, health trend, and service history. These models are related, but forcing them into a single structure creates unnecessary coupling.

Domain entities represent objects whose identity remains meaningful even when their attributes change. A Robot, Sensor, Mission, Task, Map, or Battery can therefore be modeled with a persistent identifier and a lifecycle. A robot may change location, firmware, operational mode, or assigned mission while remaining the same entity. Stable identity enables historical analysis, traceability, fleet coordination, maintenance records, and relationships across distributed data systems.

Value objects describe information primarily through their values rather than independent identity. Position, orientation, velocity, geographic coordinates, temperature ranges, timestamps, and battery measurements are typical examples. Modeling these concepts explicitly improves consistency because units, coordinate frames, precision, validation rules, and allowed ranges can be defined once. A Pose should communicate not only numbers but also frame, timestamp, and representation conventions.

Aggregates define consistency boundaries around closely related entities and value objects. A Robot aggregate might contain its operational state, capabilities, active configuration, and references to installed components, while detailed telemetry remains outside the aggregate because it arrives at a much higher frequency. Aggregate boundaries should reflect operational transactions rather than physical containment, preventing large robot objects from becoming inefficient synchronization structures.

Domain events represent facts that have already occurred and are important to other parts of the system. Examples include RobotRegistered, MissionAssigned, NavigationStarted, ChargingStarted, EmergencyStopActivated, LocalizationLost, TaskCompleted, and FaultDetected. Unlike continuously sampled telemetry, domain events describe meaningful state transitions. They allow fleet, maintenance, analytics, digital-twin, and notification services to react without direct dependencies on the producing subsystem.

Robot data architecture must clearly distinguish commands, state, events, and observations. A command expresses an intention such as StartMission or StopRobot. State represents the latest known condition, such as IDLE, EXECUTING, CHARGING, or FAULT. An event records a transition that occurred, while an observation represents measured information from sensors or software. Mixing these categories makes replay, auditing, synchronization, and failure diagnosis significantly more difficult.

Time and spatial context are first-class modeling concerns in robotics. Every important observation should carry sufficient temporal meaning to determine when it was generated, and spatial data should identify the coordinate frame in which it is valid. Sensor time, system time, synchronization quality, map frame, robot frame, geographic frame, and transformation relationships may all affect interpretation. Values without reliable temporal and spatial context can become operationally misleading.

A scalable model separates stable master data from rapidly changing operational data. Robot identity, hardware configuration, capabilities, calibration references, and component metadata change relatively slowly. Pose, velocity, sensor measurements, CPU load, battery current, and perception outputs may change many times per second. Separating these lifecycles allows each category to use appropriate storage, retention, indexing, streaming, and synchronization mechanisms.

Identifiers must remain stable across edge computers, fleet servers, cloud platforms, databases, and AI pipelines. A practical hierarchy can connect fleet, robot, subsystem, sensor, mission, task, and dataset identifiers without depending on mutable names or network addresses. Correlation identifiers should also connect commands, events, telemetry, logs, and resulting actions. This makes distributed robot behavior traceable from a business mission down to individual system observations.

Schema design should preserve semantic meaning while allowing controlled evolution. Data contracts need explicit field definitions, units, coordinate conventions, nullability, enumerations, timestamps, identifiers, and compatibility rules. New robot models or software versions will inevitably introduce additional capabilities and fields. Versioned schemas and backward-compatible changes allow older consumers to continue operating while newer components progressively adopt expanded representations.

Domain-Driven Design also creates a bridge between low-level robotics data and enterprise information. ROS 2 topics and messages can remain optimized for real-time robot communication, while domain adapters translate relevant information into Robot, Mission, Task, Asset, Fault, and Maintenance concepts. This prevents middleware-specific structures from becoming permanent business schemas and allows the same domain model to support APIs, databases, analytics, digital twins, and fleet services.

The resulting robot data model should be treated as a living architectural contract rather than a static database diagram. Domain concepts evolve as robots acquire new sensors, autonomy functions, AI models, fleet behaviors, and business roles. Maintaining clear bounded contexts, stable identities, explicit semantics, traceable relationships, and controlled schema evolution creates a durable foundation for later telemetry, event streaming, sensor management, AI data pipelines, governance, and digital-twin architecture.

로봇 데이터 모델링(Robot Data Modeling)은 로봇을 단순히 센서(Sensor), 액추에이터(Actuator), 소프트웨어 노드(Software Node)의 집합으로 보지 않는 것에서 시작한다. 로봇은 사람, 장비, 인프라, 다른 로봇과 지속적으로 상호작용하면서 상태가 변화하는 물리 시스템(Physical System)이다. 따라서 데이터 모델은 식별 정보, 구성, 운용 상태, 관측, 행동, 임무, 고장, 환경 맥락을 서로 연결된 도메인 개념으로 표현해야 한다.

도메인 주도 설계(Domain-Driven Design, DDD)는 이러한 복잡성을 체계적으로 구성하기 위한 실용적인 기반을 제공한다. 데이터베이스(Database) 기술에서 직접 스키마(Schema)를 설계하기보다 데이터가 실제 로봇 운영에서 어떤 의미를 가지는지를 먼저 정의한다. 로봇(Robot), 임무(Mission), 작업(Task), 자세(Pose), 센서(Sensor), 배터리(Battery), 고장(Fault), 지도(Map), 플릿(Fleet) 등이 공통 도메인 개념이 된다.

이러한 공통 어휘는 보편 언어(Ubiquitous Language)라고 한다. 소프트웨어 엔지니어, 로봇 엔지니어, AI 개발자, 운영자, 비즈니스 조직이 중요한 용어를 사양서, 소스 코드, API, 텔레메트리(Telemetry), 데이터베이스, 운영 대시보드에서 일관되게 사용해야 한다. 예를 들어 로봇 상태(Robot State)가 시스템마다 서로 다른 의미로 사용되면 통합 과정에서 의미적 충돌이 발생할 수 있다.

로봇 도메인은 하나의 거대한 데이터 모델로 모든 기능을 표현하기보다 경계가 있는 컨텍스트(Bounded Context)로 구분하는 것이 효과적이다. 로봇 제어(Robot Control)는 속도, 액추에이터 명령, 비상 정지, 제어 모드를 관리하고, 플릿 관리(Fleet Management)는 임무 배정, 작업, 가용성, 교통 조정을 표현할 수 있다. 유지보수(Maintenance)는 부품, 진단 이벤트, 정비 기록, 열화 상태 등을 독립적으로 모델링한다.

경계가 있는 컨텍스트(Bounded Context)가 중요한 이유는 동일한 물리 객체도 사용 목적에 따라 서로 다른 데이터 표현을 가질 수 있기 때문이다. 실시간 제어에서 배터리는 전압, 전류, 온도, 충전 상태(State of Charge)로 표현될 수 있다. 반면 유지보수에서는 일련번호, 설치일, 충방전 횟수, 상태 추세, 정비 이력이 중요하다. 두 모델은 관련되어 있지만 하나의 구조로 강제할 필요는 없다.

도메인 엔티티(Domain Entity)는 속성이 변경되더라도 지속적으로 의미를 가지는 식별자(Identity)를 가진 객체를 의미한다. 로봇, 센서, 임무, 작업, 지도, 배터리 등이 대표적인 사례다. 로봇의 위치, 펌웨어, 운용 모드, 임무가 변경되더라도 동일한 로봇 엔티티로 유지된다. 안정적인 식별 체계는 이력 분석, 추적성, 플릿 조정, 유지보수 기록을 연결하는 기반이 된다.

값 객체(Value Object)는 독립적인 식별자보다 객체가 가진 값 자체를 중심으로 의미가 정의되는 데이터이다. 위치(Position), 방향(Orientation), 속도(Velocity), 지리 좌표, 온도 범위, 타임스탬프(Timestamp), 배터리 측정값 등이 해당한다. 이러한 개념을 명확히 모델링하면 단위, 좌표계, 정밀도, 검증 규칙을 일관되게 정의할 수 있다. 자세(Pose) 역시 수치뿐 아니라 좌표 프레임과 시간 정보를 포함해야 한다.

애그리게이트(Aggregate)는 서로 밀접하게 연결된 엔티티(Entity)와 값 객체(Value Object)의 일관성 경계를 정의한다. 예를 들어 로봇 애그리게이트(Robot Aggregate)는 운용 상태, 기능, 현재 구성, 설치된 구성요소에 대한 참조를 포함할 수 있다. 반면 고주파 텔레메트리는 별도로 관리하는 것이 효율적이다. 애그리게이트의 경계는 물리적 포함 관계보다 실제 운용 트랜잭션(Transaction)을 기준으로 설정해야 한다.

도메인 이벤트(Domain Event)는 이미 발생했으며 다른 시스템에서도 의미가 있는 사실을 나타낸다. 로봇 등록(RobotRegistered), 임무 할당(MissionAssigned), 내비게이션 시작(NavigationStarted), 충전 시작(ChargingStarted), 비상 정지 활성화(EmergencyStopActivated), 위치추정 상실(LocalizationLost), 작업 완료(TaskCompleted), 고장 감지(FaultDetected) 등이 대표적이다. 지속적으로 수집되는 텔레메트리와 달리 중요한 상태 변화를 표현한다.

로봇 데이터 아키텍처(Robot Data Architecture)에서는 명령(Command), 상태(State), 이벤트(Event), 관측(Observation)을 명확하게 구분해야 한다. 명령은 임무 시작(StartMission)이나 로봇 정지(StopRobot)처럼 실행 의도를 표현한다. 상태는 현재 알려진 조건을 나타내며, 이벤트는 발생한 상태 변화를 기록한다. 관측은 센서나 소프트웨어에서 측정된 정보를 의미한다. 이를 혼합하면 재현, 감사, 동기화, 고장 분석이 어려워진다.

시간(Time)과 공간(Spatial Context)은 로봇 데이터 모델링에서 핵심적인 요소다. 중요한 관측 데이터는 언제 생성되었는지를 판단할 수 있는 시간 정보를 포함해야 하며, 공간 데이터는 어떤 좌표 프레임(Coordinate Frame)을 기준으로 하는지 명확해야 한다. 센서 시간, 시스템 시간, 동기화 품질, 지도 프레임(Map Frame), 로봇 프레임(Robot Frame), 지리 좌표계 사이의 관계가 데이터 해석에 직접 영향을 준다.

확장 가능한 데이터 모델은 안정적인 마스터 데이터(Master Data)와 빠르게 변화하는 운용 데이터(Operational Data)를 분리한다. 로봇 식별 정보, 하드웨어 구성, 기능, 보정 정보, 구성요소 메타데이터는 상대적으로 천천히 변화한다. 반면 자세, 속도, 센서 측정값, CPU 부하, 배터리 전류, 인지 결과는 초당 여러 차례 변경될 수 있다. 이러한 차이를 반영하여 저장, 보존, 인덱싱, 스트리밍 전략을 다르게 적용해야 한다.

식별자(Identifier)는 엣지 컴퓨터(Edge Computer), 플릿 서버(Fleet Server), 클라우드 플랫폼(Cloud Platform), 데이터베이스, AI 파이프라인 전체에서 안정적으로 유지되어야 한다. 플릿, 로봇, 서브시스템, 센서, 임무, 작업, 데이터셋의 식별자를 계층적으로 연결할 수 있다. 또한 상관관계 식별자(Correlation Identifier)를 사용하면 명령, 이벤트, 텔레메트리, 로그와 실제 수행 결과를 하나의 흐름으로 추적할 수 있다.

스키마 설계(Schema Design)는 데이터의 의미를 보존하면서도 통제된 방식으로 발전할 수 있어야 한다. 데이터 계약(Data Contract)은 필드 정의, 단위, 좌표 규칙, 널 허용 여부, 열거형(Enumeration), 타임스탬프, 식별자, 호환성 규칙을 명확히 정의해야 한다. 새로운 로봇 모델과 소프트웨어 버전이 추가되더라도 버전 기반 스키마(Versioned Schema)와 하위 호환성(Backward Compatibility)을 통해 기존 시스템과 새로운 시스템을 함께 운영할 수 있다.

도메인 주도 설계(Domain-Driven Design)는 저수준 로봇 데이터와 기업 정보 시스템을 연결하는 역할도 수행한다. ROS 2 토픽(Topic)과 메시지(Message)는 실시간 로봇 통신에 최적화된 구조를 유지하면서, 도메인 어댑터(Domain Adapter)가 필요한 데이터를 로봇, 임무, 작업, 자산, 고장, 유지보수 등의 비즈니스 개념으로 변환한다. 이를 통해 특정 미들웨어 구조가 영구적인 비즈니스 데이터 모델로 고착되는 것을 방지할 수 있다.

최종적인 로봇 데이터 모델(Robot Data Model)은 고정된 데이터베이스 다이어그램이 아니라 지속적으로 발전하는 아키텍처 계약(Architectural Contract)으로 관리해야 한다. 새로운 센서, 자율주행 기능, AI 모델, 플릿 동작, 비즈니스 역할이 추가되면서 도메인도 계속 변화한다. 명확한 컨텍스트 경계, 안정적인 식별자, 명시적인 의미 체계, 추적 가능한 관계, 통제된 스키마 진화를 유지하는 것이 장기적인 데이터 아키텍처의 핵심이다.

이러한 데이터 모델링 기반은 이후 텔레메트리 아키텍처(Telemetry Architecture), 이벤트 스트리밍(Event Streaming), 센서 데이터 관리(Sensor Data Management), AI 데이터 파이프라인(AI Data Pipeline), 데이터 거버넌스(Data Governance), 디지털 트윈(Digital Twin)으로 확장되는 공통 기반이 된다. 즉, 도메인 중심 데이터 모델은 개별 로봇의 데이터를 저장하는 구조를 넘어 전체 로봇 소프트웨어와 데이터 생태계를 연결하는 핵심 설계 계층으로 기능한다.

##  

## 01.02 Time Series Data Model: Sensor / Telemetry Schema

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

Time-series data is one of the most fundamental data categories in robotics because almost every robot continuously produces measurements that change over time. Motor current, wheel velocity, battery voltage, temperature, CPU utilization, localization confidence, joint position, and network latency are meaningful only when associated with time. A time-series model therefore organizes observations around timestamps, identities, measurements, and operational context.

A sensor telemetry schema defines how these continuously generated measurements are represented consistently across robot hardware, onboard software, edge infrastructure, fleet servers, and cloud platforms. The objective is not simply to store numerical values but to preserve enough context to interpret them correctly later. Each observation should identify what produced the value, when it was measured, what the value means, and under which robot configuration or operating condition it was generated.

The timestamp is the central axis of a time-series model. Robotics systems may contain sensor timestamps, acquisition timestamps, processing timestamps, transmission timestamps, and server ingestion timestamps. These values should not be treated as interchangeable because communication delays and processing queues can create significant differences between them. Whenever possible, the schema should preserve the original measurement time while separately recording later processing or ingestion times.

Clock synchronization becomes critical when observations from multiple sensors must be correlated. Cameras, LiDARs, IMUs, GNSS receivers, motor controllers, and edge computers may operate with independent clocks and different sampling frequencies. Technologies such as GNSS PPS, PTP, or other synchronization mechanisms can establish a common temporal reference. The data model should retain synchronization status or timing quality when precise temporal alignment affects downstream interpretation.

A practical telemetry record usually combines dimensions and measurements. Dimensions describe the source and context, such as robot identifier, subsystem identifier, sensor identifier, sensor type, firmware version, mission identifier, or operating mode. Measurements contain changing values such as temperature, voltage, current, speed, acceleration, utilization, or confidence. Separating relatively stable dimensions from rapidly changing measurements improves query efficiency and semantic consistency.

Measurement names must be standardized rather than created independently by each subsystem. Terms such as battery_voltage, motor_current, cpu_temperature, wheel_speed, localization_confidence, and network_latency should have explicit definitions. Naming conventions should describe the physical or logical quantity without embedding unnecessary implementation details. Consistent names make telemetry from different robot models easier to compare and allow dashboards, analytics, and maintenance applications to reuse common processing logic.

Units are part of the meaning of a measurement and should never be left implicit. Velocity may be represented in meters per second, angular velocity in radians per second, temperature in degrees Celsius, voltage in volts, current in amperes, and latency in milliseconds. A schema should define canonical units and conversion rules so that values produced by different devices cannot be accidentally combined under incompatible measurement systems.

Data types should reflect both the physical measurement and its intended processing behavior. Continuous sensor values are commonly represented as floating-point numbers, counters may use integers, operational flags may use Boolean values, and discrete robot conditions may use controlled enumerations. Choosing explicit types improves validation, storage efficiency, compression, and query performance while preventing ambiguous representations such as encoding numerical measurements as arbitrary strings.

Telemetry schemas should include data-quality information when the validity of a measurement cannot be assumed. A value may be valid, missing, stale, estimated, interpolated, saturated, out of range, or generated while a sensor is degraded. Quality flags allow downstream applications to distinguish a genuine physical observation from a questionable value. This distinction is particularly important for diagnostics, predictive maintenance, AI training, and safety-related analysis.

Sampling frequency strongly influences both schema design and storage architecture. An IMU may generate hundreds or thousands of observations per second, while battery state or environmental temperature may require only low-frequency updates. High-rate signals should not automatically use the same storage and retention policy as slow-changing operational metrics. The data model should preserve sampling characteristics while enabling aggregation, downsampling, and selective retention according to operational value.

Robot telemetry also requires clear relationships between raw measurements and derived metrics. Raw motor current, voltage, vibration, and temperature may be stored as direct observations, while energy consumption, health scores, anomaly indicators, or degradation trends are computed from them. Derived values should identify their calculation method, source data, and processing version when reproducibility matters. This prevents calculated metrics from being mistaken for direct sensor measurements.

Spatial context may be required alongside time-series observations. A temperature reading from a stationary machine differs from the same measurement collected by a mobile robot moving through a facility. Robot pose, map identifier, coordinate frame, zone, floor, or geographic position can provide valuable context for later analysis. Rather than duplicating full spatial data in every record, telemetry can reference synchronized pose streams or contextual identifiers when appropriate.

The schema must also account for intermittent connectivity, which is common in mobile robotics. Telemetry generated while a robot is disconnected should retain its original measurement timestamps and sequence information when buffered locally. When connectivity returns, delayed records can be uploaded without appearing to have been generated at the upload time. Sequence numbers, source timestamps, ingestion timestamps, and unique identifiers help detect missing, duplicated, or reordered observations.

At fleet scale, telemetry modeling should allow thousands of signals from heterogeneous robot types to coexist without losing common semantics. Shared fields such as fleet_id, robot_id, subsystem_id, sensor_id, timestamp, metric_name, value, unit, and quality can establish a common envelope, while robot-specific attributes remain extensible. This balance enables standard fleet analytics without forcing every robot platform to expose exactly the same measurements.

Schema evolution is unavoidable as new sensors, robot models, firmware versions, and diagnostic functions are introduced. Telemetry contracts should therefore support optional fields, controlled extensions, explicit schema versions, and backward-compatible changes. Consumers should not fail simply because a newer robot publishes an additional metric. At the same time, changes to units, meanings, identifiers, or coordinate conventions should be treated as semantic changes rather than harmless field modifications.

Storage design should follow the characteristics of time-series workloads. Recent telemetry is frequently queried by time range, robot, subsystem, and metric, while older data is more often used for trend analysis, incident investigation, or AI training. Time-series databases can support operational monitoring, whereas object storage or data lakes can retain larger historical datasets. The logical telemetry schema should remain understandable even when physical storage technologies differ.

Retention and downsampling policies should reflect the value of information over time. High-frequency raw telemetry may be essential during immediate fault investigation but unnecessarily expensive to retain indefinitely. Older data can often be summarized into minimum, maximum, average, percentile, count, or statistical windows. Critical fault intervals and unusual operating conditions may be preserved at full resolution while routine periods are progressively aggregated.

A well-designed sensor telemetry schema ultimately creates a common temporal language for the robot data architecture. It connects physical measurements with robot identity, time, quality, configuration, and operational context while remaining scalable across individual robots and large fleets. This foundation supports later telemetry pipelines, time-series databases, anomaly detection, predictive maintenance, digital twins, fleet monitoring, and AI data systems without requiring each application to reinterpret raw robot signals independently.

시계열 데이터(Time-Series Data)는 로봇 공학에서 가장 기본적인 데이터 유형 중 하나이다. 거의 모든 로봇은 시간에 따라 변화하는 측정값을 지속적으로 생성하기 때문이다. 모터 전류, 휠 속도, 배터리 전압, 온도, CPU 사용률, 위치추정 신뢰도, 관절 위치, 네트워크 지연시간 등은 시간 정보와 연결될 때 의미를 가진다. 따라서 시계열 모델(Time-Series Model)은 타임스탬프(Timestamp), 식별자(Identity), 측정값(Measurement), 운용 맥락(Operational Context)을 중심으로 관측 데이터를 구성한다.

센서 텔레메트리 스키마(Sensor Telemetry Schema)는 지속적으로 생성되는 측정값을 로봇 하드웨어, 온보드 소프트웨어(Onboard Software), 엣지 인프라(Edge Infrastructure), 플릿 서버(Fleet Server), 클라우드 플랫폼(Cloud Platform) 전체에서 일관된 방식으로 표현하는 방법을 정의한다. 목적은 단순한 수치 저장이 아니라 데이터 생성 주체, 측정 시점, 값의 의미, 로봇 구성 및 운용 조건을 함께 보존하는 것이다.

타임스탬프(Timestamp)는 시계열 모델의 중심축이다. 로봇 시스템에는 센서 타임스탬프(Sensor Timestamp), 획득 타임스탬프(Acquisition Timestamp), 처리 타임스탬프(Processing Timestamp), 전송 타임스탬프(Transmission Timestamp), 서버 수집 타임스탬프(Ingestion Timestamp)가 존재할 수 있다. 통신 지연이나 처리 대기열로 인해 서로 차이가 발생하므로 동일하게 취급해서는 안 되며, 가능한 경우 원래 측정 시간을 별도로 보존해야 한다.

여러 센서의 관측 데이터를 연계하려면 시계 동기화(Clock Synchronization)가 중요하다. 카메라, 라이다(LiDAR), 관성측정장치(IMU), 위성항법시스템(GNSS), 모터 제어기, 엣지 컴퓨터는 서로 독립적인 시계와 다른 샘플링 주파수(Sampling Frequency)를 사용할 수 있다. GNSS PPS, 정밀 시간 프로토콜(PTP) 등의 동기화 기술로 공통 시간 기준을 구축하고, 정밀한 시간 정렬이 필요한 경우 동기화 상태와 시간 품질 정보도 데이터 모델에 포함해야 한다.

실용적인 텔레메트리 레코드(Telemetry Record)는 일반적으로 차원(Dimension)과 측정값(Measurement)을 결합한다. 차원은 로봇 식별자, 서브시스템 식별자, 센서 식별자, 센서 유형, 펌웨어 버전, 임무 식별자, 운용 모드 등 데이터의 출처와 맥락을 설명한다. 측정값은 온도, 전압, 전류, 속도, 가속도, 사용률, 신뢰도처럼 지속적으로 변화하는 값을 나타낸다. 이들을 분리하면 질의 효율과 의미적 일관성을 향상시킬 수 있다.

측정값 이름(Measurement Name)은 각 서브시스템이 독립적으로 정의하기보다 표준화해야 한다. battery_voltage, motor_current, cpu_temperature, wheel_speed, localization_confidence, network_latency와 같은 항목에는 명확한 정의가 필요하다. 명명 규칙(Naming Convention)은 불필요한 구현 세부사항을 포함하기보다 물리적 또는 논리적 의미를 표현해야 하며, 이를 통해 서로 다른 로봇 모델의 텔레메트리를 동일한 분석 체계에서 비교할 수 있다.

단위(Unit)는 측정값의 의미를 구성하는 일부이므로 암묵적으로 처리해서는 안 된다. 선속도는 초당 미터(m/s), 각속도는 초당 라디안(rad/s), 온도는 섭씨(°C), 전압은 볼트(V), 전류는 암페어(A), 지연시간은 밀리초(ms) 등으로 표현할 수 있다. 스키마는 표준 단위(Canonical Unit)와 변환 규칙을 정의하여 서로 다른 장치에서 생성된 값이 호환되지 않는 단위로 잘못 결합되는 것을 방지해야 한다.

데이터 유형(Data Type)은 물리적 측정값의 특성과 이후 처리 방식을 모두 반영해야 한다. 연속적인 센서 값은 일반적으로 부동소수점(Float), 카운터는 정수(Integer), 운용 플래그는 불리언(Boolean), 불연속적인 로봇 상태는 통제된 열거형(Enumeration)으로 표현할 수 있다. 명확한 데이터 유형은 검증, 저장 효율, 압축, 질의 성능을 향상시키고 숫자 데이터를 임의의 문자열로 저장하는 것과 같은 모호한 표현을 방지한다.

측정값의 유효성을 항상 보장할 수 없는 경우 텔레메트리 스키마에는 데이터 품질(Data Quality) 정보가 포함되어야 한다. 측정값은 정상, 누락, 오래됨, 추정, 보간, 포화, 범위 초과 또는 센서 성능 저하 상태에서 생성된 값일 수 있다. 품질 플래그(Quality Flag)를 사용하면 실제 물리적 관측값과 신뢰하기 어려운 값을 구분할 수 있으며, 이는 진단, 예지보전(Predictive Maintenance), AI 학습, 안전 분석에서 특히 중요하다.

샘플링 주파수(Sampling Frequency)는 스키마 설계와 저장 아키텍처 모두에 큰 영향을 미친다. 관성측정장치(IMU)는 초당 수백에서 수천 개의 데이터를 생성할 수 있지만 배터리 상태나 환경 온도는 낮은 주기의 갱신만으로 충분할 수 있다. 고주파 신호에 저주파 운용 지표와 동일한 저장 및 보존 정책을 적용할 필요는 없다. 데이터 모델은 샘플링 특성을 보존하면서 집계, 다운샘플링(Downsampling), 선택적 보존을 지원해야 한다.

로봇 텔레메트리는 원시 측정값(Raw Measurement)과 파생 지표(Derived Metric)의 관계도 명확히 정의해야 한다. 모터 전류, 전압, 진동, 온도는 직접 관측한 원시 데이터인 반면 에너지 소비량, 상태 점수, 이상 지표, 열화 추세 등은 이를 이용하여 계산된 값이다. 재현성(Reproducibility)이 중요한 경우 파생 데이터에는 계산 방법, 원본 데이터, 처리 버전 정보를 함께 기록하여 직접 센서 측정값과 혼동되지 않도록 해야 한다.

시계열 관측 데이터에는 공간적 맥락(Spatial Context)이 함께 필요할 수 있다. 고정된 장비에서 측정한 온도와 시설 내부를 이동하는 로봇이 수집한 동일한 온도 값은 의미가 다를 수 있다. 로봇 자세(Pose), 지도 식별자(Map ID), 좌표 프레임(Coordinate Frame), 구역(Zone), 층(Floor), 지리적 위치 등을 이용하면 분석에 필요한 공간 정보를 제공할 수 있다. 모든 레코드에 전체 공간 데이터를 복제하기보다 동기화된 자세 스트림을 참조하는 방법도 사용할 수 있다.

모바일 로봇에서 흔히 발생하는 간헐적 연결(Intermittent Connectivity)도 스키마에서 고려해야 한다. 통신이 끊어진 동안 생성된 텔레메트리는 로컬에 버퍼링(Buffering)하면서 원래의 측정 타임스탬프와 시퀀스 정보를 유지해야 한다. 연결이 복구되면 지연된 데이터를 업로드하더라도 업로드 시점에 생성된 것처럼 처리해서는 안 된다. 시퀀스 번호, 소스 타임스탬프, 수집 타임스탬프, 고유 식별자를 활용하면 누락, 중복, 순서 변경을 탐지할 수 있다.

플릿 규모(Fleet Scale)의 텔레메트리 모델은 서로 다른 종류의 로봇에서 생성되는 수천 개의 신호를 공통 의미 체계를 유지하면서 처리할 수 있어야 한다. fleet_id, robot_id, subsystem_id, sensor_id, timestamp, metric_name, value, unit, quality와 같은 공통 필드를 표준 데이터 외피(Common Envelope)로 정의하고, 로봇별 특수 속성은 확장 가능하게 구성할 수 있다. 이를 통해 모든 로봇에 동일한 측정값을 강제하지 않고도 공통 플릿 분석이 가능하다.

새로운 센서, 로봇 모델, 펌웨어 버전, 진단 기능이 추가되면서 스키마 진화(Schema Evolution)는 필연적으로 발생한다. 텔레메트리 계약(Telemetry Contract)은 선택적 필드, 통제된 확장, 명시적인 스키마 버전, 하위 호환 변경(Backward-Compatible Change)을 지원해야 한다. 새로운 로봇이 추가 측정값을 전송하더라도 기존 소비자가 실패해서는 안 된다. 반면 단위, 의미, 식별자, 좌표 규칙의 변경은 단순한 필드 변경이 아니라 의미적 변경(Semantic Change)으로 관리해야 한다.

저장 설계(Storage Design)는 시계열 워크로드(Time-Series Workload)의 특성을 반영해야 한다. 최근 텔레메트리는 시간 범위, 로봇, 서브시스템, 측정 항목을 기준으로 자주 조회되는 반면 오래된 데이터는 추세 분석, 사고 조사, AI 학습 등에 활용된다. 시계열 데이터베이스(Time-Series Database)는 실시간 운영 모니터링을 지원하고, 객체 저장소(Object Storage)나 데이터 레이크(Data Lake)는 대규모 장기 이력 데이터를 보관하는 데 활용할 수 있다.

보존 정책(Retention Policy)과 다운샘플링(Downsampling)은 시간이 지나면서 변화하는 데이터의 가치에 따라 설계해야 한다. 고주파 원시 텔레메트리는 고장 직후의 원인 분석에는 중요하지만 무기한 보존하기에는 비용이 크다. 오래된 데이터는 최소값, 최대값, 평균값, 백분위수(Percentile), 개수 등의 통계 구간으로 요약할 수 있다. 중요한 고장 구간이나 비정상 운용 조건은 전체 해상도로 보존하고 일반적인 운용 구간은 점진적으로 집계할 수 있다.

잘 설계된 센서 텔레메트리 스키마(Sensor Telemetry Schema)는 궁극적으로 로봇 데이터 아키텍처 전체에서 사용할 수 있는 공통 시간 언어(Common Temporal Language)를 구축한다. 물리적 측정값을 로봇 식별 정보, 시간, 데이터 품질, 구성, 운용 맥락과 연결하면서 개별 로봇부터 대규모 플릿까지 확장할 수 있어야 한다. 이러한 구조를 통해 각 응용 시스템이 원시 로봇 신호를 독립적으로 다시 해석해야 하는 문제를 줄일 수 있다.

이러한 기반은 이후 텔레메트리 파이프라인(Telemetry Pipeline), 시계열 데이터베이스(Time-Series Database), 이상 탐지(Anomaly Detection), 예지보전(Predictive Maintenance), 디지털 트윈(Digital Twin), 플릿 모니터링(Fleet Monitoring), AI 데이터 시스템(AI Data System)으로 확장된다. 즉, 표준화된 시계열 데이터 모델은 로봇의 실시간 상태와 장기 운용 이력을 연결하고 분석 가능한 데이터 자산으로 전환하는 핵심 기반이 된다.

##  

## 01.03 Spatial Data Model: Maps, Routes, Positions

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

Spatial data modeling provides the foundation for describing where a robot, object, destination, obstacle, or operational event exists within the physical world. Unlike conventional business data, robot spatial information is meaningful only when its coordinate system, reference frame, timestamp, and environment are known. Maps, routes, positions, poses, zones, landmarks, and geographic coordinates therefore require explicit relationships rather than isolated numerical fields.

A position represents a location within a defined spatial reference, typically expressed using two-dimensional or three-dimensional coordinates. However, coordinates such as x, y, and z have no independent meaning without a coordinate frame. A robust spatial schema therefore associates every position with a frame identifier, timestamp, unit, and reference system. This prevents coordinates generated by different localization, mapping, simulation, or navigation components from being incorrectly interpreted as equivalent.

A robot pose extends position by describing orientation as well as location. In planar mobile robotics, pose is often represented by x, y, and heading, while three-dimensional systems may use x, y, z together with quaternion or Euler-angle orientation. The schema should explicitly define orientation conventions, axis directions, units, and rotation order. Ambiguous pose representations can produce serious errors when data is exchanged between navigation, perception, digital-twin, and analytics systems.

Coordinate frames establish relationships among different spatial representations. A robot may simultaneously use a global map frame, an odometry frame, a robot base frame, and multiple sensor frames. Transformations describe how positions and orientations in one frame relate to another. The spatial data model should preserve frame identifiers and transformation relationships so that observations from cameras, LiDARs, GNSS receivers, manipulators, and other sensors can be interpreted within a common spatial structure.

Maps provide persistent representations of environments in which robots operate. Depending on the application, a map may contain occupancy grids, geometric features, semantic objects, lanes, navigation graphs, elevation information, restricted areas, or three-dimensional point clouds. Rather than treating a map as a single binary file, the data model should describe map identity, version, coordinate reference, resolution, spatial extent, layers, creation time, and relationships to operational environments.

Map layers allow different forms of spatial knowledge to coexist without being merged into one representation. An occupancy layer can describe free and occupied space, a semantic layer can identify doors, elevators, workstations, charging stations, or storage areas, and a navigation layer can describe traversable paths. Additional layers may represent safety zones, speed restrictions, temporary obstacles, infrastructure, or localization landmarks while remaining referenced to the same map.

Map identity and versioning are essential because physical environments change over time. Furniture may move, construction may alter passages, warehouse racks may be relocated, and outdoor roads or facilities may be modified. Robots should therefore identify the exact map version used for localization and navigation. Historical telemetry, routes, incidents, and AI datasets can then be associated with the spatial environment that was valid when those records were generated.

Routes describe intended movement through a spatial environment and should be distinguished from the actual trajectory traveled by a robot. A route may consist of ordered waypoints, graph nodes, lane segments, or path sections connecting an origin to a destination. Each route should retain identifiers, map references, direction constraints, permitted robot classes, speed limits, and operational conditions when these attributes influence navigation or fleet coordination.

Waypoints represent meaningful positions along routes and may include more information than coordinates alone. A waypoint can describe a pickup point, charging location, elevator entrance, inspection location, waiting area, or navigation transition. Attributes such as approach direction, stopping tolerance, maximum speed, required action, and semantic type make waypoints reusable operational objects rather than arbitrary points embedded directly inside mission definitions.

A trajectory represents the actual or planned motion of a robot over time and therefore combines spatial and temporal information. Each trajectory sample may contain position, orientation, velocity, acceleration, curvature, and timestamp. Planned trajectories can be compared with executed trajectories to analyze tracking performance, navigation efficiency, localization drift, or abnormal behavior. This distinction also supports simulation, validation, and autonomous-navigation development.

Spatial zones provide a higher-level method for describing operational areas. Instead of reasoning only with individual coordinates, applications can define polygons or volumes representing loading zones, pedestrian areas, restricted regions, charging areas, hazardous locations, or service boundaries. A robot position can then be evaluated against these zones to determine permissions, behaviors, speed policies, mission rules, or safety constraints without embedding geographic logic directly in application code.

Indoor and outdoor robots often require different spatial reference systems. Indoor AMRs commonly use local metric coordinates referenced to building maps, floors, and localization frames, while outdoor robots may use latitude, longitude, altitude, projected coordinates, or GNSS-based references. A unified spatial model should support both forms without assuming that one coordinate representation is universal. Explicit coordinate reference system metadata enables reliable transformation between local and geographic spaces.

Multi-floor facilities introduce another modeling requirement because identical x and y coordinates may represent completely different physical locations on different floors. Floor identifiers, building identifiers, map identifiers, and vertical transitions should therefore be modeled explicitly. Elevators, ramps, stairs, and transfer points can be represented as connectivity relationships between spatial regions, allowing fleet and navigation systems to reason about movement across multiple maps or levels.

Spatial relationships can provide more useful information than absolute coordinates alone. Concepts such as inside, adjacent to, connected to, near, intersects, reachable from, or belongs to a zone can describe the structure of an environment. These relationships enable semantic navigation and business-level queries such as determining which robot is inside a loading area, which charging station is nearest, or whether a destination can be reached through an authorized route.

Dynamic spatial information should be separated from relatively stable map information. Walls, lanes, charging stations, and fixed infrastructure may change infrequently, while robot positions, moving obstacles, temporary closures, and detected objects can change continuously. Separating static, semi-static, and dynamic spatial data allows different update frequencies, storage strategies, validity periods, and synchronization mechanisms while preserving relationships through common map and frame identifiers.

Spatial uncertainty should also be represented when location estimates are probabilistic rather than exact. GNSS, SLAM, visual localization, and sensor fusion systems may produce covariance, confidence, accuracy, or quality indicators together with estimated poses. Storing only the estimated coordinates hides important information about localization reliability. Downstream navigation, diagnostics, mapping, and analytics systems can make better decisions when uncertainty information accompanies spatial observations.

At fleet scale, stable spatial identifiers enable multiple robots to share maps, routes, zones, and infrastructure references. Instead of each robot independently defining "Charging Station 1" or "Warehouse Zone A," fleet-level objects can use persistent identifiers that are referenced by missions, telemetry, events, and maintenance records. This allows heterogeneous robots to operate within a shared spatial vocabulary while still supporting platform-specific navigation representations.

Spatial schemas must evolve as facilities, maps, robot capabilities, and navigation algorithms change. Map versions, route revisions, zone updates, coordinate transformations, and semantic annotations should therefore be traceable over time. Compatibility rules are particularly important when a fleet contains robots operating with different map revisions. Controlled spatial versioning prevents historical positions or mission records from being silently interpreted against a newer and geometrically different environment.

A well-designed spatial data model ultimately creates a common geometric and semantic language for robot systems. Positions describe where entities exist, poses describe location and orientation, maps describe environments, routes express intended movement, trajectories record motion through time, and zones capture operational meaning. Connected through coordinate frames, identifiers, timestamps, and versions, these concepts form the spatial foundation for localization, navigation, fleet coordination, digital twins, simulation, analytics, and Physical AI.

공간 데이터 모델링(Spatial Data Modeling)은 로봇, 객체, 목적지, 장애물 또는 운용 이벤트가 물리적 세계에서 어디에 존재하는지를 표현하기 위한 기반을 제공한다. 일반적인 비즈니스 데이터와 달리 로봇의 공간 정보는 좌표계(Coordinate System), 기준 프레임(Reference Frame), 타임스탬프(Timestamp), 환경 정보가 함께 정의되어야 의미를 가진다. 따라서 지도(Map), 경로(Route), 위치(Position), 자세(Pose), 구역(Zone), 랜드마크(Landmark), 지리 좌표(Geographic Coordinate)는 단순한 숫자가 아니라 명시적인 관계를 통해 모델링해야 한다.

위치(Position)는 정의된 공간 기준에서 특정 지점을 나타내며 일반적으로 2차원 또는 3차원 좌표로 표현된다. 그러나 x, y, z와 같은 좌표값은 좌표 프레임(Coordinate Frame)이 없으면 독립적인 의미를 가질 수 없다. 따라서 견고한 공간 스키마(Spatial Schema)는 모든 위치에 프레임 식별자(Frame Identifier), 타임스탬프, 단위(Unit), 기준 시스템(Reference System)을 연결해야 한다. 이를 통해 위치추정, 매핑, 시뮬레이션, 내비게이션 시스템에서 생성된 서로 다른 좌표가 동일한 것으로 잘못 해석되는 것을 방지할 수 있다.

로봇 자세(Robot Pose)는 위치뿐만 아니라 방향(Orientation)까지 포함하여 로봇의 공간 상태를 표현한다. 평면 이동 로봇에서는 일반적으로 x, y와 진행 방향(Heading)을 사용하며, 3차원 시스템에서는 x, y, z와 함께 쿼터니언(Quaternion) 또는 오일러 각(Euler Angle)을 사용할 수 있다. 스키마는 방향 표현 방식, 축 방향, 단위, 회전 순서(Rotation Order)를 명확히 정의해야 한다. 모호한 자세 표현은 내비게이션, 인지, 디지털 트윈, 분석 시스템 간 데이터 교환에서 심각한 오류를 발생시킬 수 있다.

좌표 프레임(Coordinate Frame)은 서로 다른 공간 표현 사이의 관계를 정의한다. 하나의 로봇은 동시에 전역 지도 프레임(Global Map Frame), 오도메트리 프레임(Odometry Frame), 로봇 베이스 프레임(Robot Base Frame), 여러 센서 프레임(Sensor Frame)을 사용할 수 있다. 변환(Transformation)은 한 프레임의 위치와 방향이 다른 프레임에서 어떻게 표현되는지를 정의한다. 공간 데이터 모델은 프레임 식별자와 변환 관계를 보존하여 카메라, 라이다(LiDAR), GNSS, 매니퓰레이터(Manipulator) 등의 데이터를 공통 공간 구조에서 해석할 수 있도록 해야 한다.

지도(Map)는 로봇이 운용되는 환경을 지속적으로 표현하는 공간 데이터 구조이다. 응용 분야에 따라 점유 격자(Occupancy Grid), 기하학적 특징(Geometric Feature), 의미 객체(Semantic Object), 차선(Lane), 내비게이션 그래프(Navigation Graph), 고도 정보(Elevation Information), 제한 구역(Restricted Area), 3차원 포인트 클라우드(Point Cloud) 등을 포함할 수 있다. 지도는 하나의 바이너리 파일로만 취급하지 않고 식별자, 버전, 좌표 기준, 해상도, 공간 범위, 레이어, 생성 시간 등을 함께 모델링해야 한다.

지도 레이어(Map Layer)를 사용하면 서로 다른 형태의 공간 정보를 하나의 표현으로 강제로 통합하지 않고 공존시킬 수 있다. 점유 레이어(Occupancy Layer)는 자유 공간과 점유 공간을 표현하고, 의미 레이어(Semantic Layer)는 문, 엘리베이터, 작업대, 충전소, 보관 구역 등을 식별할 수 있다. 내비게이션 레이어(Navigation Layer)는 주행 가능한 경로를 표현하며, 안전 구역, 속도 제한, 임시 장애물, 인프라, 위치추정 랜드마크 등을 추가 레이어로 구성할 수 있다.

지도 식별자(Map Identity)와 버전 관리(Versioning)는 물리적 환경이 시간에 따라 변화하기 때문에 필수적이다. 가구가 이동하거나 공사로 통로가 변경될 수 있고, 창고 랙이나 실외 도로 및 시설도 변경될 수 있다. 따라서 로봇은 위치추정과 내비게이션에 사용한 정확한 지도 버전(Map Version)을 식별할 수 있어야 한다. 이를 통해 과거의 텔레메트리, 이동 경로, 사고 기록, AI 데이터셋을 당시 실제로 사용된 공간 환경과 연결할 수 있다.

경로(Route)는 공간 환경에서 계획된 이동을 표현하며 로봇이 실제로 이동한 궤적(Trajectory)과 구분해야 한다. 경로는 출발점과 목적지를 연결하는 순서화된 웨이포인트(Waypoint), 그래프 노드(Graph Node), 차선 구간(Lane Segment), 경로 구간(Path Section) 등으로 구성될 수 있다. 각 경로에는 식별자, 지도 참조, 진행 방향 제약, 허용 로봇 유형, 속도 제한, 운용 조건 등의 정보를 포함할 수 있다.

웨이포인트(Waypoint)는 경로상의 의미 있는 위치를 표현하며 단순한 좌표 이상의 정보를 포함할 수 있다. 웨이포인트는 픽업 지점(Pickup Point), 충전 위치(Charging Location), 엘리베이터 입구, 검사 위치, 대기 구역 또는 내비게이션 전환 지점 등을 나타낼 수 있다. 접근 방향, 정지 허용 오차(Stopping Tolerance), 최대 속도, 수행해야 할 동작, 의미 유형(Semantic Type)을 속성으로 정의하면 임무 내부의 임의 좌표가 아니라 재사용 가능한 운용 객체가 된다.

궤적(Trajectory)은 시간에 따른 로봇의 실제 또는 계획된 움직임을 나타내므로 공간 정보와 시간 정보를 결합한다. 각각의 궤적 샘플(Trajectory Sample)은 위치, 방향, 속도, 가속도, 곡률(Curvature), 타임스탬프를 포함할 수 있다. 계획 궤적과 실제 수행 궤적을 비교하면 추종 성능(Tracking Performance), 내비게이션 효율, 위치추정 드리프트(Localization Drift), 비정상 동작 등을 분석할 수 있으며 시뮬레이션과 검증에도 활용할 수 있다.

공간 구역(Spatial Zone)은 운용 영역을 보다 높은 수준에서 표현하는 방법을 제공한다. 개별 좌표만 사용하는 대신 적재 구역, 보행자 구역, 제한 구역, 충전 구역, 위험 지역, 서비스 영역 등을 폴리곤(Polygon)이나 볼륨(Volume)으로 정의할 수 있다. 로봇의 위치가 특정 구역에 포함되는지를 판단하여 접근 권한, 행동, 속도 정책, 임무 규칙, 안전 제약 등을 결정할 수 있으며, 애플리케이션 코드에 직접 공간 규칙을 삽입하는 것을 줄일 수 있다.

실내 로봇과 실외 로봇은 서로 다른 공간 기준 시스템(Spatial Reference System)을 사용하는 경우가 많다. 실내 자율이동로봇(AMR)은 일반적으로 건물 지도, 층, 위치추정 프레임을 기준으로 하는 로컬 미터 좌표(Local Metric Coordinate)를 사용한다. 실외 로봇은 위도, 경도, 고도, 투영 좌표(Projected Coordinate), GNSS 기반 좌표를 사용할 수 있다. 통합 공간 모델은 하나의 좌표 표현을 강제하지 않고 두 환경을 모두 지원해야 한다.

다층 시설(Multi-Floor Facility)은 동일한 x, y 좌표가 서로 다른 층의 완전히 다른 물리적 위치를 의미할 수 있기 때문에 추가적인 모델링이 필요하다. 따라서 층 식별자(Floor ID), 건물 식별자(Building ID), 지도 식별자(Map ID), 수직 이동 연결 정보를 명시적으로 모델링해야 한다. 엘리베이터, 램프, 계단, 환승 지점(Transfer Point)은 공간 영역 사이의 연결 관계로 표현하여 플릿과 내비게이션 시스템이 여러 지도와 층을 연결한 이동을 판단할 수 있도록 해야 한다.

공간 관계(Spatial Relationship)는 절대 좌표만 사용하는 것보다 환경에 대한 더 유용한 정보를 제공할 수 있다. 내부에 있음(Inside), 인접함(Adjacent To), 연결됨(Connected To), 가까움(Near), 교차함(Intersects), 도달 가능함(Reachable From), 특정 구역에 속함(Belongs To) 등의 관계를 이용하여 환경 구조를 표현할 수 있다. 이를 통해 특정 로봇이 적재 구역에 있는지, 가장 가까운 충전소가 어디인지, 목적지까지 허용된 경로로 이동할 수 있는지를 판단할 수 있다.

동적 공간 정보(Dynamic Spatial Information)는 상대적으로 안정적인 지도 정보와 분리하는 것이 바람직하다. 벽, 차선, 충전소, 고정 인프라는 비교적 천천히 변화하지만 로봇 위치, 이동 장애물, 임시 폐쇄 구역, 감지 객체는 지속적으로 변화한다. 정적(Static), 준정적(Semi-Static), 동적(Dynamic) 공간 데이터를 분리하면 각 데이터에 서로 다른 갱신 주기, 저장 전략, 유효 기간, 동기화 메커니즘을 적용하면서 공통 지도 및 프레임 식별자를 통해 관계를 유지할 수 있다.

위치추정 결과가 정확한 하나의 값이 아니라 확률적 추정값인 경우 공간 불확실성(Spatial Uncertainty)도 함께 표현해야 한다. GNSS, 동시적 위치추정 및 지도작성(SLAM), 비전 위치추정(Visual Localization), 센서 융합(Sensor Fusion)은 추정 자세와 함께 공분산(Covariance), 신뢰도(Confidence), 정확도(Accuracy), 품질 지표(Quality Indicator)를 생성할 수 있다. 추정 좌표만 저장하면 위치 신뢰성에 대한 중요한 정보가 사라지므로 이러한 불확실성 정보를 함께 보존해야 한다.

플릿 규모(Fleet Scale)에서는 안정적인 공간 식별자(Spatial Identifier)를 통해 여러 로봇이 지도, 경로, 구역, 인프라 참조를 공유할 수 있다. 각 로봇이 독립적으로 'Charging Station 1'이나 'Warehouse Zone A'를 정의하는 대신 플릿 수준의 객체에 영구 식별자를 부여하고 임무, 텔레메트리, 이벤트, 유지보수 기록에서 이를 참조할 수 있다. 이를 통해 이기종 로봇(Heterogeneous Robot)이 서로 다른 내비게이션 구조를 사용하더라도 공통 공간 어휘를 공유할 수 있다.

시설, 지도, 로봇 기능, 내비게이션 알고리즘이 변화함에 따라 공간 스키마(Spatial Schema)도 지속적으로 발전해야 한다. 지도 버전, 경로 수정, 구역 변경, 좌표 변환, 의미 주석(Semantic Annotation)은 시간에 따라 추적 가능해야 한다. 특히 서로 다른 지도 버전을 사용하는 로봇이 하나의 플릿에 존재하는 경우 호환성 규칙이 중요하다. 통제된 공간 버전 관리(Spatial Versioning)를 통해 과거 위치나 임무 기록이 새로운 환경 구조를 기준으로 잘못 해석되는 것을 방지할 수 있다.

잘 설계된 공간 데이터 모델(Spatial Data Model)은 궁극적으로 로봇 시스템 전체가 공유할 수 있는 공통 기하학적·의미적 언어(Common Geometric and Semantic Language)를 구축한다. 위치(Position)는 객체가 어디에 있는지, 자세(Pose)는 위치와 방향을, 지도(Map)는 환경을, 경로(Route)는 의도된 이동을, 궤적(Trajectory)은 시간에 따른 움직임을, 구역(Zone)은 운용상의 의미를 표현한다. 이들을 좌표 프레임, 식별자, 타임스탬프, 버전으로 연결하면 위치추정, 내비게이션, 플릿 조정, 디지털 트윈, 시뮬레이션, 분석 및 피지컬 AI(Physical AI)를 위한 공간적 기반을 구축할 수 있다.

##  

## 01.04 Event Data Model: Robot Lifecycle Event Classification

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

An event data model represents meaningful facts that occur during the operation and lifecycle of a robot. Unlike telemetry, which continuously reports measurements such as temperature, velocity, or battery voltage, an event describes something that happened at a specific point in time. RobotRegistered, MissionStarted, ChargingCompleted, LocalizationLost, EmergencyStopActivated, and FaultDetected are examples of events that communicate significant changes to other systems.

A robot event should describe an observed fact rather than an intention or request. StartMission is normally a command because it asks the robot to perform an action, whereas MissionStarted is an event confirming that the transition actually occurred. Maintaining this distinction between commands and events is fundamental to reliable distributed systems because a requested action may be rejected, delayed, interrupted, or fail before producing the corresponding event.

Every event requires a consistent envelope containing information that allows it to be identified, ordered, traced, and interpreted. Typical fields include event_id, event_type, event_time, robot_id, source_id, schema_version, correlation_id, severity, and payload. The envelope provides common metadata across heterogeneous events, while the payload contains information specific to the event, such as fault codes, mission identifiers, battery values, or localization status.

Event identity must be globally stable enough to distinguish one occurrence from another across robots, edge computers, brokers, fleet servers, and cloud systems. A unique event_id supports deduplication when network retries cause the same event to be transmitted more than once. Robot and source identifiers establish ownership, while correlation identifiers connect related commands, tasks, events, telemetry, and logs into a traceable operational sequence.

Event time should represent when the event actually occurred at the source whenever possible. In distributed robot systems, this may differ from the time when an edge gateway receives the event or a cloud server stores it. Source event time, ingestion time, and processing time can therefore be modeled separately. Preserving these distinctions enables accurate reconstruction of incidents even when communication is delayed or temporarily unavailable.

Robot lifecycle events can be organized according to major operational phases. Commissioning events describe registration, configuration, calibration, activation, and initial deployment. Operational events represent startup, readiness, mission execution, navigation, task processing, charging, docking, and shutdown. Maintenance events describe inspection, repair, component replacement, software updates, and return to service, creating a continuous history throughout the robot lifecycle.

Operational state-transition events form an important category because they explain how a robot moves between meaningful states. Examples include RobotBooted, RobotReady, MissionAssigned, MissionStarted, NavigationStarted, TaskCompleted, MissionCompleted, ChargingStarted, and RobotShutdown. Each event should describe a completed transition rather than simply duplicating the current state, allowing historical sequences to reconstruct how the robot reached its present condition.

Navigation and localization events describe significant changes in autonomous mobility rather than every pose update. RouteAssigned, GoalReached, NavigationPaused, PathBlocked, LocalizationDegraded, LocalizationLost, LocalizationRecovered, and GeofenceEntered are examples. High-frequency positions remain telemetry or spatial observations, while events are generated when a condition crosses a meaningful operational boundary that requires recording or reaction.

Mission and task events connect low-level robot behavior with fleet and business operations. MissionCreated, MissionAssigned, MissionAccepted, TaskStarted, TaskCompleted, TaskFailed, MissionCancelled, and MissionCompleted can describe the execution lifecycle. Mission and task identifiers should remain consistent across the event sequence so that operators can reconstruct execution history, calculate performance metrics, investigate failures, and connect robot activities with external workflows.

Energy and charging events represent transitions that influence robot availability. BatteryLow, BatteryCritical, ChargingRequested, DockingStarted, ChargingStarted, ChargingCompleted, and UndockingCompleted can be modeled as lifecycle events, while voltage, current, temperature, and state-of-charge remain telemetry measurements. This separation allows fleet systems to react to meaningful energy conditions without processing every individual battery sample.

Diagnostic events describe abnormal conditions detected by hardware, software, sensors, or AI components. FaultDetected, FaultConfirmed, FaultCleared, SensorDegraded, MotorOverTemperature, CommunicationLost, and ComputeResourceCritical are examples. Diagnostic events should reference standardized fault or diagnostic codes when available, while the payload may contain subsystem identity, severity, supporting measurements, and contextual information required for root-cause analysis.

Safety events require particularly clear semantics because they may trigger immediate operational responses and later compliance analysis. EmergencyStopActivated, SafetyScannerTriggered, CollisionDetected, ProtectiveStopEntered, RestrictedZoneViolation, and EmergencyStopReleased can represent safety-relevant transitions. Such events should preserve source, timestamp, severity, robot state, and relevant context so that the sequence surrounding a safety incident can be reconstructed accurately.

Event severity provides a standardized indication of operational importance but should remain separate from event type. Informational events may document normal transitions, warnings may indicate degraded conditions, errors may represent failed functions, and critical events may require immediate intervention. Separating severity from classification allows the same event family to express different levels of impact without creating excessive numbers of narrowly defined event types.

Events may also be classified by domain so that consumers can subscribe only to information relevant to their responsibilities. Lifecycle, mission, navigation, localization, energy, maintenance, diagnostic, safety, cybersecurity, perception, and infrastructure domains can form a practical classification hierarchy. Fleet management may consume mission and availability events, while maintenance systems focus on diagnostic and service events and security systems process cybersecurity events.

Event causality should be represented whenever one event results from an earlier command, event, or operational condition. Correlation and causation identifiers make it possible to determine that a MissionStarted event resulted from a particular StartMission command, or that a ProtectiveStopEntered event was triggered by a specific safety observation. These relationships are essential for distributed tracing, incident investigation, event sourcing, and automated workflow orchestration.

Event schemas must support evolution because robot capabilities and operational requirements change over time. New event types, fields, diagnostic information, or safety attributes will appear as platforms mature. Explicit schema versions, optional fields, compatibility rules, and controlled naming conventions allow producers and consumers to evolve independently. Changes that alter the semantic meaning of an existing event should be versioned rather than silently redefining previously established behavior.

Reliable event processing must also account for duplication, delayed delivery, reordering, and temporary network disconnection. Mobile robots may buffer events locally and publish them later when connectivity returns. Unique identifiers, source sequence numbers, event timestamps, and idempotent consumers help maintain consistency under these conditions. Critical lifecycle and safety events should not be discarded merely because communication with a central platform is temporarily unavailable.

Event retention creates a durable operational history that complements high-volume telemetry. Telemetry explains detailed physical behavior, while events identify important moments that organize that data into meaningful episodes. An incident investigation can first locate FaultDetected or EmergencyStopActivated and then retrieve telemetry surrounding that timestamp. This relationship significantly reduces the effort required to analyze long periods of continuous robot operation.

A well-designed robot lifecycle event model ultimately provides a common language for describing what happened, when it happened, where it originated, why it may have occurred, and what operational context surrounded it. Standardized event identities, classifications, timestamps, severities, lifecycle relationships, and schemas connect robot control, fleet management, maintenance, safety, analytics, digital twins, and business systems into a traceable event-driven architecture.

이벤트 데이터 모델(Event Data Model)은 로봇의 운용 및 생명주기(Lifecycle) 동안 발생하는 의미 있는 사실을 표현한다. 온도, 속도, 배터리 전압처럼 측정값을 지속적으로 보고하는 텔레메트리(Telemetry)와 달리 이벤트(Event)는 특정 시점에 발생한 사건을 나타낸다. RobotRegistered, MissionStarted, ChargingCompleted, LocalizationLost, EmergencyStopActivated, FaultDetected 등은 다른 시스템에 중요한 상태 변화를 전달하는 대표적인 이벤트이다.

로봇 이벤트(Robot Event)는 의도나 요청이 아니라 실제로 관측된 사실을 표현해야 한다. StartMission은 로봇에게 행동 수행을 요청하므로 일반적으로 명령(Command)에 해당하지만, MissionStarted는 실제 상태 전환이 발생했음을 확인하는 이벤트이다. 요청된 행동은 거부되거나 지연되고 중단되거나 실패할 수 있으므로 명령과 이벤트를 구분하는 것은 신뢰성 높은 분산 시스템(Distributed System)의 기본 원칙이다.

모든 이벤트에는 이벤트를 식별하고 순서를 결정하며 추적하고 해석할 수 있도록 일관된 이벤트 외피(Event Envelope)가 필요하다. 일반적인 필드에는 event_id, event_type, event_time, robot_id, source_id, schema_version, correlation_id, severity, payload 등이 포함된다. 이벤트 외피는 서로 다른 이벤트에 공통 메타데이터(Metadata)를 제공하며, 페이로드(Payload)는 고장 코드, 임무 식별자, 배터리 값, 위치추정 상태 등 이벤트별 정보를 포함한다.

이벤트 식별자(Event Identity)는 로봇, 엣지 컴퓨터(Edge Computer), 메시지 브로커(Message Broker), 플릿 서버(Fleet Server), 클라우드 시스템 전체에서 각각의 발생 건을 구분할 수 있도록 전역적으로 안정적이어야 한다. 고유한 event_id는 네트워크 재시도로 동일 이벤트가 여러 번 전송되는 경우 중복 제거(Deduplication)를 지원한다. 로봇 및 소스 식별자는 데이터 소유 주체를 나타내며, 상관관계 식별자(Correlation Identifier)는 관련 명령, 작업, 이벤트, 텔레메트리, 로그를 하나의 추적 가능한 운용 흐름으로 연결한다.

이벤트 시간(Event Time)은 가능한 경우 이벤트가 실제 소스에서 발생한 시점을 나타내야 한다. 분산 로봇 시스템에서는 이 시간이 엣지 게이트웨이(Edge Gateway)가 이벤트를 수신한 시점이나 클라우드 서버가 저장한 시점과 다를 수 있다. 따라서 소스 이벤트 시간(Source Event Time), 수집 시간(Ingestion Time), 처리 시간(Processing Time)을 별도로 모델링할 수 있다. 이를 구분하여 보존하면 통신이 지연되거나 일시적으로 단절된 경우에도 사고의 시간적 순서를 정확하게 재구성할 수 있다.

로봇 생명주기 이벤트(Robot Lifecycle Event)는 주요 운용 단계에 따라 구성할 수 있다. 시운전 이벤트(Commissioning Event)는 등록, 구성, 보정, 활성화, 최초 배치를 나타낸다. 운용 이벤트(Operational Event)는 기동, 준비, 임무 수행, 내비게이션, 작업 처리, 충전, 도킹, 종료 등을 표현한다. 유지보수 이벤트(Maintenance Event)는 검사, 수리, 부품 교체, 소프트웨어 업데이트, 서비스 복귀 등을 기록하여 로봇 전체 생명주기에 걸친 연속적인 이력을 형성한다.

운용 상태 전환 이벤트(Operational State-Transition Event)는 로봇이 의미 있는 상태 사이에서 어떻게 변화했는지를 설명하기 때문에 중요한 분류에 해당한다. RobotBooted, RobotReady, MissionAssigned, MissionStarted, NavigationStarted, TaskCompleted, MissionCompleted, ChargingStarted, RobotShutdown 등이 대표적이다. 각 이벤트는 단순히 현재 상태를 복제하는 것이 아니라 완료된 상태 전환을 나타내야 하며, 이를 통해 과거 이벤트의 순서만으로도 로봇이 현재 상태에 도달한 과정을 재구성할 수 있다.

내비게이션 및 위치추정 이벤트(Navigation and Localization Event)는 모든 자세(Pose) 갱신을 기록하는 대신 자율 이동에서 중요한 변화를 표현한다. RouteAssigned, GoalReached, NavigationPaused, PathBlocked, LocalizationDegraded, LocalizationLost, LocalizationRecovered, GeofenceEntered 등이 이에 해당한다. 고주파 위치 정보는 텔레메트리 또는 공간 관측 데이터로 유지하고, 기록이나 대응이 필요한 의미 있는 운용 경계를 넘어설 때 이벤트를 생성하는 방식이 적절하다.

임무 및 작업 이벤트(Mission and Task Event)는 저수준 로봇 동작과 플릿 및 비즈니스 운영을 연결한다. MissionCreated, MissionAssigned, MissionAccepted, TaskStarted, TaskCompleted, TaskFailed, MissionCancelled, MissionCompleted 등을 통해 임무 실행 생명주기를 표현할 수 있다. 이벤트 전체에서 임무 및 작업 식별자를 일관되게 유지하면 운영자는 실행 이력을 재구성하고 성능 지표를 계산하며 실패 원인을 조사하고 로봇 활동을 외부 업무 흐름과 연결할 수 있다.

에너지 및 충전 이벤트(Energy and Charging Event)는 로봇의 가용성(Availability)에 영향을 주는 상태 전환을 표현한다. BatteryLow, BatteryCritical, ChargingRequested, DockingStarted, ChargingStarted, ChargingCompleted, UndockingCompleted 등을 생명주기 이벤트로 모델링할 수 있다. 반면 전압, 전류, 온도, 충전 상태(State of Charge)는 텔레메트리 측정값으로 유지한다. 이러한 분리를 통해 플릿 시스템은 모든 배터리 샘플을 처리하지 않고도 중요한 에너지 상태에 대응할 수 있다.

진단 이벤트(Diagnostic Event)는 하드웨어, 소프트웨어, 센서 또는 AI 구성요소에서 감지된 비정상 상태를 표현한다. FaultDetected, FaultConfirmed, FaultCleared, SensorDegraded, MotorOverTemperature, CommunicationLost, ComputeResourceCritical 등이 대표적이다. 가능한 경우 진단 이벤트는 표준화된 고장 또는 진단 코드(Diagnostic Code)를 참조해야 하며, 페이로드에는 서브시스템 식별자, 심각도, 관련 측정값, 근본 원인 분석(Root-Cause Analysis)에 필요한 맥락 정보를 포함할 수 있다.

안전 이벤트(Safety Event)는 즉각적인 운용 대응과 이후의 규정 준수 분석(Compliance Analysis)을 유발할 수 있기 때문에 특히 명확한 의미 체계가 필요하다. EmergencyStopActivated, SafetyScannerTriggered, CollisionDetected, ProtectiveStopEntered, RestrictedZoneViolation, EmergencyStopReleased 등이 안전 관련 상태 전환을 표현한다. 이러한 이벤트에는 소스, 타임스탬프, 심각도, 로봇 상태 및 관련 맥락을 보존하여 안전 사고 전후의 사건 순서를 정확하게 재구성할 수 있어야 한다.

이벤트 심각도(Event Severity)는 운용상 중요도를 표준화하여 나타내지만 이벤트 유형(Event Type)과는 분리해서 관리해야 한다. 정보 수준(Informational)은 정상적인 상태 전환을 기록하고, 경고(Warning)는 성능 저하 상태를 나타내며, 오류(Error)는 기능 실패를 표현할 수 있다. 치명적 수준(Critical)은 즉각적인 개입이 필요한 상황을 나타낼 수 있다. 심각도와 분류를 분리하면 지나치게 많은 세부 이벤트 유형을 생성하지 않고도 동일한 이벤트 계열에서 서로 다른 영향 수준을 표현할 수 있다.

이벤트는 도메인(Domain)에 따라 분류하여 각 소비자(Consumer)가 자신의 책임 영역과 관련된 정보만 구독할 수 있도록 구성할 수도 있다. 생명주기, 임무, 내비게이션, 위치추정, 에너지, 유지보수, 진단, 안전, 사이버보안(Cybersecurity), 인지(Perception), 인프라(Infrastructure) 등이 실용적인 분류 계층을 구성할 수 있다. 플릿 관리는 임무와 가용성 이벤트를 사용하고, 유지보수 시스템은 진단과 서비스 이벤트에 집중하며, 보안 시스템은 사이버보안 이벤트를 처리할 수 있다.

하나의 이벤트가 이전 명령, 이벤트 또는 운용 조건으로 인해 발생한 경우에는 이벤트 인과관계(Event Causality)를 표현해야 한다. 상관관계 식별자(Correlation Identifier)와 인과관계 식별자(Causation Identifier)를 이용하면 MissionStarted 이벤트가 특정 StartMission 명령으로부터 발생했는지 또는 ProtectiveStopEntered 이벤트가 특정 안전 관측으로 인해 발생했는지를 확인할 수 있다. 이러한 관계는 분산 추적(Distributed Tracing), 사고 조사, 이벤트 소싱(Event Sourcing), 자동화된 워크플로 오케스트레이션(Workflow Orchestration)에 중요하다.

로봇의 기능과 운용 요구사항은 지속적으로 변화하므로 이벤트 스키마(Event Schema)는 이러한 변화에 대응할 수 있어야 한다. 플랫폼이 발전하면서 새로운 이벤트 유형, 필드, 진단 정보, 안전 속성이 추가된다. 명시적인 스키마 버전(Schema Version), 선택적 필드(Optional Field), 호환성 규칙(Compatibility Rule), 통제된 명명 규칙을 사용하면 생산자(Producer)와 소비자가 독립적으로 발전할 수 있다. 기존 이벤트의 의미 자체가 변경되는 경우에는 기존 정의를 조용히 변경하지 말고 새로운 버전으로 관리해야 한다.

신뢰성 있는 이벤트 처리(Reliable Event Processing)는 중복, 지연 전송, 순서 변경, 일시적인 네트워크 단절도 고려해야 한다. 모바일 로봇은 연결이 끊어졌을 때 이벤트를 로컬에 버퍼링(Buffering)하고 통신이 복구되면 나중에 전송할 수 있다. 고유 식별자, 소스 시퀀스 번호(Source Sequence Number), 이벤트 타임스탬프, 멱등성 소비자(Idempotent Consumer)를 활용하면 이러한 조건에서도 일관성을 유지할 수 있다. 특히 중요한 생명주기 및 안전 이벤트는 중앙 플랫폼과의 통신이 일시적으로 끊어졌다는 이유로 손실되어서는 안 된다.

이벤트 보존(Event Retention)은 대용량 텔레메트리를 보완하는 지속적인 운용 이력(Durable Operational History)을 생성한다. 텔레메트리는 로봇의 세부적인 물리적 동작을 설명하고, 이벤트는 이러한 데이터를 의미 있는 운용 시점과 구간으로 구성한다. 사고 분석에서는 먼저 FaultDetected 또는 EmergencyStopActivated 이벤트를 찾아낸 뒤 해당 타임스탬프 전후의 텔레메트리를 조회할 수 있다. 이러한 연계는 장시간의 연속적인 로봇 운용 데이터를 분석하는 데 필요한 작업량을 크게 줄여준다.

잘 설계된 로봇 생명주기 이벤트 모델(Robot Lifecycle Event Model)은 궁극적으로 무엇이 발생했는지, 언제 발생했는지, 어디에서 시작되었는지, 왜 발생했을 가능성이 있는지, 그리고 어떤 운용 맥락에서 발생했는지를 설명하는 공통 언어(Common Language)를 제공한다. 표준화된 이벤트 식별자, 분류, 타임스탬프, 심각도, 생명주기 관계, 스키마를 통해 로봇 제어, 플릿 관리, 유지보수, 안전, 분석, 디지털 트윈(Digital Twin), 비즈니스 시스템을 추적 가능한 이벤트 주도 아키텍처(Event-Driven Architecture)로 연결할 수 있다.

##  

## 01.05 Robot Configuration Data Model Design

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

Robot configuration data describes the relatively stable parameters that determine what a robot is, what hardware and software it contains, and how those components should operate together. Unlike telemetry, which changes continuously, configuration data changes mainly during commissioning, maintenance, calibration, software deployment, or mission preparation. A structured configuration model provides a reliable source of truth for robot identity, capabilities, components, parameters, and operational limits.

A configuration model should separate robot identity from mutable settings. Stable attributes such as robot_id, model, serial number, manufacturer, hardware revision, and production information identify the physical asset, while parameters such as speed limits, sensor settings, controller gains, network addresses, or operational policies may change over time. Keeping these categories distinct prevents configuration updates from unintentionally modifying the identity of the robot.

Robot configuration is naturally hierarchical because a robot consists of multiple subsystems and components. A top-level Robot configuration may reference mobility, compute, power, perception, communication, safety, manipulation, and payload subsystems. Each subsystem can contain components such as motors, motor controllers, batteries, cameras, LiDARs, IMUs, GNSS receivers, GPUs, network interfaces, or safety devices with their own parameters and metadata.

Component identity should remain stable independently of configuration values. Each installed component can have a component_id, component_type, manufacturer, model, serial_number, hardware_version, firmware_version, installation_position, and lifecycle status. This structure enables maintenance systems to determine which physical device produced particular telemetry or faults and allows replacement history to remain traceable even when a component is removed and another is installed.

Hardware configuration describes the physical composition and engineering characteristics of the robot. It can include wheel dimensions, drive type, motor ratings, battery capacity, sensor mounting positions, compute resources, communication interfaces, payload limits, and mechanical dimensions. These properties influence control, navigation, energy management, safety, and simulation, so they should be represented using explicit units, data types, allowed ranges, and engineering definitions.

Software configuration describes which software components are deployed and how they are configured to operate. Relevant information may include operating-system versions, firmware, middleware, ROS 2 packages, container images, AI models, navigation stacks, controller versions, and application releases. Recording software configuration together with hardware configuration makes it possible to reproduce the operational environment associated with a particular robot behavior, incident, or dataset.

Runtime parameters represent adjustable values used by software during operation. Examples include maximum velocity, acceleration limits, obstacle margins, planner parameters, controller gains, localization thresholds, sensor rates, logging levels, and communication timeouts. Parameters should include data type, unit, valid range, default value, current value, and description where appropriate. Explicit constraints prevent invalid values from reaching safety-critical or performance-sensitive components.

Configuration values should be organized according to ownership and scope rather than stored in one large undifferentiated file. Some parameters belong to an individual sensor, others to a subsystem, robot model, specific robot, fleet, site, or mission. A layered configuration model allows common defaults to be defined once and specialized values to override them at narrower scopes, reducing duplication while preserving the ability to customize individual robots.

Configuration inheritance can support efficient management of heterogeneous fleets. A base robot model may define common hardware and software defaults, while a variant adds different sensors or payloads. A specific robot can then override calibration values or network settings, and a deployment site can apply local speed or safety policies. The effective configuration is produced by combining these layers according to explicit precedence rules rather than manually duplicating complete configuration files.

Sensor configuration requires both device parameters and spatial information. Camera resolution, frame rate, exposure, LiDAR scan frequency, IMU range, GNSS settings, and sensor enablement may be combined with mounting position, orientation, coordinate frame, calibration reference, and synchronization settings. Connecting sensor configuration with the spatial data model ensures that sensor observations can later be interpreted using the correct geometry and calibration state.

Calibration data should be managed as a specialized configuration category because it affects the interpretation of physical measurements. Camera intrinsic parameters, sensor extrinsics, wheel radius corrections, steering offsets, IMU biases, and actuator calibration coefficients may change after service or recalibration. Each calibration record should therefore include identity, version, creation time, validity period, calibration method, and applicable component or robot reference.

Capability information describes what a configured robot can actually perform. Capabilities may include autonomous navigation, elevator integration, charging, towing, manipulation, inspection, thermal sensing, outdoor operation, or specific payload functions. Fleet and mission systems can use capability data when selecting robots for tasks. Separating capabilities from low-level component details prevents higher-level applications from needing to understand every hardware parameter.

Operational constraints define boundaries within which the robot is permitted to operate. Maximum speed, payload, slope, turning limits, minimum battery reserve, temperature range, geographic restrictions, sensor dependencies, or safety-mode limitations can be represented as configuration policies. These values may originate from engineering specifications, site rules, or mission requirements and should be distinguishable from dynamically measured operating conditions.

Safety-related configuration requires stronger governance than ordinary convenience settings. Emergency-stop behavior, safety scanner zones, protective speed limits, collision thresholds, braking parameters, and safety-controller settings should have explicit ownership, validation, authorization, and change history. Systems should distinguish between parameters that operators may adjust routinely and parameters that require engineering approval or controlled deployment procedures.

Configuration versioning is essential because a robot\'s behavior can change significantly even when its physical identity remains unchanged. Every deployable configuration should have a version or immutable revision identifier. Historical telemetry, events, faults, missions, and AI data can then reference the configuration active when they were generated. This allows engineers to determine whether a behavioral change resulted from hardware, software, calibration, parameter, or environmental differences.

Configuration changes should be recorded as auditable events rather than silently overwriting previous values. A change record can identify which parameter changed, its previous and new values, who or what initiated the change, when it occurred, why it was made, and which configuration revision resulted. This history supports debugging, compliance, rollback, fleet comparison, and root-cause analysis when problems appear after configuration updates.

Validation should occur before configuration is activated on a robot. Schema validation can verify data types, required fields, enumeration values, units, and ranges, while semantic validation checks relationships between parameters. For example, a maximum operational speed should not exceed the hardware or safety limit. Cross-component validation can also detect incompatible firmware, unsupported sensor combinations, missing calibration data, or conflicting configuration dependencies.

Deployment status should be modeled separately from desired configuration. A fleet server may define a desired configuration, while a robot currently runs an older applied configuration because it is offline or an update failed. Recording desired_version, applied_version, deployment_status, and activation_time makes configuration drift visible. Fleet management systems can then identify robots that have not successfully converged to the approved configuration baseline.

Configuration rollback is an important operational capability when a new parameter set or software release causes unexpected behavior. Because previous configuration revisions are retained, operators can restore a known stable version without reconstructing settings manually. Rollback records should preserve the reason, target revision, execution result, and resulting active configuration so that recovery actions remain fully traceable.

A well-designed robot configuration data model ultimately becomes the authoritative description of how each robot is assembled, configured, calibrated, constrained, and enabled to perform its functions. By combining stable identity, hierarchical components, layered parameters, versioning, validation, audit history, deployment state, and capability information, the model connects engineering definitions with actual fleet operation.

This configuration foundation also links directly with telemetry, event, spatial, maintenance, digital-twin, simulation, and AI data architectures. Telemetry can reference the configuration under which measurements were produced, events can identify configuration changes, and digital twins can instantiate the correct robot variant. Configuration data therefore acts as a reproducibility and traceability layer that connects the physical robot with its software-defined operational state.

로봇 구성 데이터(Robot Configuration Data)는 로봇이 무엇인지, 어떤 하드웨어와 소프트웨어로 구성되어 있는지, 그리고 이러한 구성요소가 어떻게 함께 동작해야 하는지를 결정하는 비교적 안정적인 매개변수를 표현한다. 지속적으로 변화하는 텔레메트리(Telemetry)와 달리 구성 데이터는 주로 시운전(Commissioning), 유지보수, 보정, 소프트웨어 배포 또는 임무 준비 과정에서 변경된다. 구조화된 구성 모델(Configuration Model)은 로봇 식별 정보, 기능, 구성요소, 매개변수, 운용 한계에 대한 신뢰할 수 있는 기준 정보(Source of Truth)를 제공한다.

구성 모델(Configuration Model)은 로봇 식별 정보(Robot Identity)와 변경 가능한 설정(Mutable Settings)을 분리해야 한다. robot_id, 모델, 일련번호, 제조사, 하드웨어 리비전(Hardware Revision), 생산 정보와 같은 안정적인 속성은 물리적 자산을 식별한다. 반면 속도 제한, 센서 설정, 제어기 게인(Controller Gain), 네트워크 주소, 운용 정책 등은 시간에 따라 변경될 수 있다. 두 범주를 분리하면 구성 변경으로 로봇의 고유 식별 정보가 의도하지 않게 변경되는 것을 방지할 수 있다.

로봇은 여러 서브시스템(Subsystem)과 구성요소(Component)로 이루어지므로 로봇 구성은 자연스럽게 계층 구조(Hierarchical Structure)를 가진다. 최상위 로봇 구성은 이동, 컴퓨팅, 전원, 인지, 통신, 안전, 조작, 페이로드(Payload) 서브시스템을 참조할 수 있다. 각 서브시스템에는 모터, 모터 제어기, 배터리, 카메라, 라이다(LiDAR), 관성측정장치(IMU), 위성항법시스템(GNSS), GPU, 네트워크 인터페이스, 안전 장치 등이 포함될 수 있으며 각각 자체 매개변수와 메타데이터(Metadata)를 가진다.

구성요소 식별 정보(Component Identity)는 구성값과 독립적으로 안정적으로 유지되어야 한다. 설치된 각 구성요소에는 component_id, component_type, 제조사, 모델, serial_number, hardware_version, firmware_version, 설치 위치(Installation Position), 생명주기 상태(Lifecycle Status) 등을 정의할 수 있다. 이를 통해 유지보수 시스템은 어떤 물리적 장치에서 특정 텔레메트리나 고장이 발생했는지를 파악할 수 있으며, 부품이 제거되고 새로운 부품으로 교체된 이후에도 교체 이력을 지속적으로 추적할 수 있다.

하드웨어 구성(Hardware Configuration)은 로봇의 물리적 구성과 공학적 특성을 표현한다. 휠 치수, 구동 방식, 모터 정격, 배터리 용량, 센서 장착 위치, 컴퓨팅 자원, 통신 인터페이스, 페이로드 한계, 기계적 치수 등이 포함될 수 있다. 이러한 속성은 제어, 내비게이션, 에너지 관리, 안전, 시뮬레이션에 영향을 주므로 명시적인 단위(Unit), 데이터 유형(Data Type), 허용 범위(Allowed Range), 공학적 정의와 함께 표현해야 한다.

소프트웨어 구성(Software Configuration)은 어떤 소프트웨어 구성요소가 배포되어 있으며 어떻게 동작하도록 설정되어 있는지를 나타낸다. 운영체제 버전, 펌웨어(Firmware), 미들웨어(Middleware), ROS 2 패키지, 컨테이너 이미지(Container Image), AI 모델, 내비게이션 스택(Navigation Stack), 제어기 버전, 애플리케이션 릴리스(Application Release) 등이 포함될 수 있다. 소프트웨어 구성과 하드웨어 구성을 함께 기록하면 특정 로봇 동작, 사고 또는 데이터셋이 생성되었을 당시의 운용 환경을 재현할 수 있다.

런타임 매개변수(Runtime Parameter)는 소프트웨어가 운용 중 사용하는 조정 가능한 값을 의미한다. 최대 속도, 가속도 제한, 장애물 여유 거리, 플래너 매개변수(Planner Parameter), 제어기 게인, 위치추정 임계값, 센서 주기, 로깅 수준, 통신 타임아웃 등이 대표적인 예이다. 각 매개변수에는 필요에 따라 데이터 유형, 단위, 유효 범위, 기본값(Default Value), 현재값(Current Value), 설명을 포함해야 한다. 명확한 제약 조건은 잘못된 값이 안전 또는 성능에 민감한 구성요소에 적용되는 것을 방지한다.

구성값(Configuration Value)은 하나의 거대한 파일에 구분 없이 저장하기보다 소유권(Ownership)과 적용 범위(Scope)에 따라 구성해야 한다. 일부 매개변수는 개별 센서에 속하고 다른 값은 서브시스템, 로봇 모델, 특정 로봇, 플릿(Fleet), 사이트(Site), 임무(Mission)에 적용될 수 있다. 계층형 구성 모델(Layered Configuration Model)을 사용하면 공통 기본값을 한 번 정의하고 더 좁은 범위에서 필요한 값만 재정의(Override)할 수 있어 중복을 줄이면서 개별 로봇의 맞춤 구성을 유지할 수 있다.

구성 상속(Configuration Inheritance)은 이기종 로봇 플릿(Heterogeneous Robot Fleet)을 효율적으로 관리하는 데 활용할 수 있다. 기본 로봇 모델(Base Robot Model)에 공통 하드웨어와 소프트웨어 기본값을 정의하고, 변형 모델(Variant)은 다른 센서나 페이로드를 추가할 수 있다. 특정 로봇은 보정값이나 네트워크 설정을 재정의하고, 배치 사이트는 지역별 속도 또는 안전 정책을 적용할 수 있다. 최종 유효 구성(Effective Configuration)은 전체 구성 파일을 반복 복사하지 않고 명확한 우선순위 규칙(Precedence Rule)에 따라 이러한 계층을 결합하여 생성한다.

센서 구성(Sensor Configuration)은 장치 매개변수뿐만 아니라 공간 정보(Spatial Information)도 함께 포함해야 한다. 카메라 해상도, 프레임 속도(Frame Rate), 노출, 라이다 스캔 주파수, IMU 범위, GNSS 설정, 센서 활성화 여부 등을 장착 위치, 방향, 좌표 프레임(Coordinate Frame), 보정 참조(Calibration Reference), 동기화 설정과 결합할 수 있다. 센서 구성을 공간 데이터 모델(Spatial Data Model)과 연결하면 이후 센서 관측 데이터를 정확한 기하 구조와 보정 상태를 기준으로 해석할 수 있다.

보정 데이터(Calibration Data)는 물리적 측정값의 해석에 직접적인 영향을 미치므로 특수한 구성 범주로 관리해야 한다. 카메라 내부 파라미터(Camera Intrinsic Parameter), 센서 외부 파라미터(Sensor Extrinsic), 휠 반경 보정값, 조향 오프셋(Steering Offset), IMU 바이어스(Bias), 액추에이터 보정 계수 등이 정비나 재보정 이후 변경될 수 있다. 따라서 각 보정 기록에는 식별 정보, 버전, 생성 시간, 유효 기간, 보정 방법, 적용 대상 구성요소 또는 로봇 참조 정보를 포함해야 한다.

기능 정보(Capability Information)는 구성된 로봇이 실제로 무엇을 수행할 수 있는지를 설명한다. 기능에는 자율 내비게이션, 엘리베이터 연동, 자동 충전, 견인, 조작, 검사, 열화상 감지, 실외 운용 또는 특정 페이로드 기능 등이 포함될 수 있다. 플릿 및 임무 시스템은 작업에 적합한 로봇을 선택할 때 이러한 기능 데이터를 사용할 수 있다. 기능을 저수준 구성요소 정보와 분리하면 상위 애플리케이션이 모든 하드웨어 매개변수를 직접 이해할 필요가 없다.

운용 제약조건(Operational Constraint)은 로봇이 허용된 범위 내에서 운용되도록 경계를 정의한다. 최대 속도, 페이로드, 경사도, 회전 한계, 최소 배터리 잔량, 온도 범위, 지리적 제한, 센서 의존성, 안전 모드 제한 등을 구성 정책(Configuration Policy)으로 표현할 수 있다. 이러한 값은 공학 사양, 현장 규칙 또는 임무 요구사항에서 정의될 수 있으며, 실시간으로 측정되는 운용 상태(Operating Condition)와 구분하여 관리해야 한다.

안전 관련 구성(Safety-Related Configuration)은 일반적인 편의성 설정보다 강력한 거버넌스(Governance)가 필요하다. 비상 정지 동작, 안전 스캐너 구역, 보호 속도 제한, 충돌 임계값, 제동 매개변수, 안전 제어기 설정에는 명확한 소유권, 검증, 권한 관리, 변경 이력이 필요하다. 또한 운영자가 일상적으로 조정할 수 있는 매개변수와 엔지니어링 승인 또는 통제된 배포 절차가 필요한 매개변수를 명확하게 구분해야 한다.

구성 버전 관리(Configuration Versioning)는 물리적으로 동일한 로봇이라도 구성 변경에 따라 동작이 크게 달라질 수 있기 때문에 필수적이다. 배포 가능한 모든 구성에는 버전 또는 변경 불가능한 리비전 식별자(Immutable Revision Identifier)가 있어야 한다. 과거의 텔레메트리, 이벤트, 고장, 임무, AI 데이터가 당시 활성화되어 있던 구성 버전을 참조하도록 하면 동작 변화가 하드웨어, 소프트웨어, 보정, 매개변수 또는 환경 차이에서 발생했는지를 분석할 수 있다.

구성 변경(Configuration Change)은 기존 값을 조용히 덮어쓰는 방식이 아니라 감사 가능한 이벤트(Auditable Event)로 기록해야 한다. 변경 기록에는 어떤 매개변수가 변경되었는지, 이전값과 새로운 값, 변경을 시작한 사용자 또는 시스템, 변경 시점, 변경 이유, 생성된 구성 리비전(Configuration Revision)을 포함할 수 있다. 이러한 이력은 디버깅, 규정 준수, 롤백(Rollback), 플릿 비교, 구성 변경 이후 발생한 문제의 근본 원인 분석(Root-Cause Analysis)을 지원한다.

구성은 로봇에서 활성화되기 전에 검증(Validation)되어야 한다. 스키마 검증(Schema Validation)은 데이터 유형, 필수 필드, 열거형 값, 단위, 허용 범위를 확인하고, 의미 검증(Semantic Validation)은 매개변수 사이의 관계를 확인한다. 예를 들어 최대 운용 속도는 하드웨어 또는 안전 한계를 초과해서는 안 된다. 구성요소 간 검증(Cross-Component Validation)을 통해 호환되지 않는 펌웨어, 지원되지 않는 센서 조합, 누락된 보정 데이터, 충돌하는 구성 의존성도 탐지할 수 있다.

배포 상태(Deployment Status)는 목표 구성(Desired Configuration)과 별도로 모델링해야 한다. 플릿 서버가 목표 구성을 정의했더라도 로봇이 오프라인이거나 업데이트에 실패하여 이전 구성을 실행하고 있을 수 있다. desired_version, applied_version, deployment_status, activation_time을 기록하면 구성 드리프트(Configuration Drift)를 확인할 수 있다. 이를 통해 플릿 관리 시스템은 승인된 구성 기준선(Configuration Baseline)에 정상적으로 수렴하지 못한 로봇을 식별할 수 있다.

구성 롤백(Configuration Rollback)은 새로운 매개변수 세트나 소프트웨어 릴리스가 예상하지 못한 동작을 발생시킬 때 중요한 운용 기능이다. 이전 구성 리비전을 보존하고 있으면 운영자는 설정을 수동으로 다시 구성하지 않고도 검증된 안정 버전(Known Stable Version)으로 복원할 수 있다. 롤백 기록에는 수행 이유, 대상 리비전, 실행 결과, 최종 활성 구성을 보존하여 복구 작업 전체를 추적할 수 있도록 해야 한다.

잘 설계된 로봇 구성 데이터 모델(Robot Configuration Data Model)은 궁극적으로 각 로봇이 어떻게 조립되고, 구성되고, 보정되고, 제약되며, 어떤 기능을 수행하도록 설정되어 있는지를 설명하는 권위 있는 기준 정보(Authoritative Description)가 된다. 안정적인 식별 정보, 계층적 구성요소, 계층형 매개변수, 버전 관리, 검증, 감사 이력, 배포 상태, 기능 정보를 결합함으로써 공학적 정의와 실제 플릿 운용을 연결할 수 있다.

이러한 구성 기반(Configuration Foundation)은 텔레메트리(Telemetry), 이벤트(Event), 공간 데이터(Spatial Data), 유지보수(Maintenance), 디지털 트윈(Digital Twin), 시뮬레이션(Simulation), AI 데이터 아키텍처와도 직접 연결된다. 텔레메트리는 측정값이 생성될 당시의 구성을 참조하고, 이벤트는 구성 변경을 기록하며, 디지털 트윈은 정확한 로봇 변형 모델을 생성할 수 있다. 따라서 구성 데이터는 물리적 로봇과 소프트웨어 정의 운용 상태(Software-Defined Operational State)를 연결하는 재현성(Reproducibility)과 추적성(Traceability)의 핵심 계층으로 기능한다.

##  

## 01.06 AI Training Data Model: Annotation Schema Standard

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}

AI training data for robotics must represent more than raw sensor files and labels because learning systems depend on the context in which observations were produced. Images, point clouds, audio, force signals, trajectories, actions, and robot states may all become training samples. A training data model should therefore connect sensor observations with robot identity, timestamps, spatial context, configuration, task information, annotations, and dataset provenance.

A training sample is the fundamental unit that connects source data with learning metadata. Depending on the application, one sample may represent a single image, synchronized multi-camera frames, a LiDAR scan, a short temporal sequence, or an entire manipulation episode. Each sample should have a stable sample_id and references to its source files, timestamps, sensors, robot, mission, environment, and annotation records without requiring unnecessary duplication of large binary data.

Dataset identity should be separated from individual samples so that collections can be versioned and managed independently. A dataset record may contain dataset_id, name, version, creation time, task type, source domain, license, ownership, and lifecycle status. It should also reference the included samples and annotation schema. This structure allows the same underlying observations to participate in different datasets for perception, navigation, manipulation, or multimodal learning.

Annotation schemas define how human-generated, machine-generated, or automatically derived labels are represented consistently. A schema should specify annotation type, class taxonomy, geometry, attributes, confidence, source, and version. Bounding boxes, polygons, segmentation masks, keypoints, 3D cuboids, trajectories, actions, language descriptions, and quality flags require different structures but should share common identification and provenance conventions.

A class taxonomy provides controlled semantic definitions for annotation labels. Instead of allowing arbitrary strings such as person, pedestrian, human, or worker to be used interchangeably, the dataset should define canonical class identifiers and names. Hierarchical taxonomies can represent relationships such as vehicle as a parent class and forklift, truck, or mobile robot as specialized classes, supporting both detailed and generalized learning tasks.

Two-dimensional vision annotations typically reference an image and describe objects using bounding boxes, polygons, masks, or keypoints. Each annotation should identify its sample, class, geometry, attributes, visibility, occlusion state, and confidence when applicable. Coordinates must follow an explicit convention defining image origin, width and height interpretation, normalization, and clipping rules so that training frameworks do not interpret identical annotations differently.

Three-dimensional annotations require additional spatial definitions because geometry may be represented in sensor, robot, map, or global coordinate frames. A 3D cuboid can include center position, dimensions, orientation, class, velocity, and tracking identifier. The annotation must identify the coordinate frame and calibration reference used when it was created. Without this information, labels cannot be reliably transformed across LiDAR, camera, robot, or world representations.

Temporal annotation becomes important when robot behavior or object motion must be learned across sequences rather than independent frames. A track_id can connect the same object across multiple observations, while start and end timestamps define temporal segments for actions or events. Navigation and manipulation datasets may represent complete episodes containing observations, states, actions, rewards, task descriptions, and termination conditions in chronological order.

Robot learning introduces action annotations that are fundamentally different from conventional perception labels. An action may represent velocity commands, joint targets, end-effector poses, gripper states, discrete skills, or higher-level task primitives. Each action should be associated with the observation and robot state from which it was executed. This observation-action relationship is essential for imitation learning, policy learning, and Vision-Language-Action model training.

Multimodal datasets require explicit synchronization relationships among cameras, LiDAR, IMU, audio, force sensors, robot states, and language information. Rather than assuming files with similar names were captured simultaneously, the data model should preserve timestamps, synchronization groups, calibration references, and modality identifiers. This allows training pipelines to construct reliable multimodal samples even when sensors operate at different rates.

Language annotations can connect physical observations and actions with semantic instructions. A sample or episode may include task instructions, scene descriptions, question-answer pairs, action explanations, object references, or operator commands. Language records should identify language, annotation source, timestamp or temporal scope, and relationships to objects, actions, or episodes. These relationships become particularly important for multimodal foundation models and Vision-Language-Action systems.

Annotation provenance records how a label was produced. An annotation may originate from a human annotator, an automated model, simulation ground truth, rule-based processing, or a combination of these methods. Fields such as annotator_id, tool_version, model_version, creation_time, review_status, and confidence help determine whether a label is appropriate for training, evaluation, or further review and make annotation processes reproducible.

Human annotation workflows often require multiple lifecycle states rather than a simple labeled or unlabeled distinction. An annotation can progress through created, submitted, reviewed, corrected, approved, or rejected states. Reviewer information and change history should be retained when quality assurance is important. This makes it possible to measure disagreement, identify systematic labeling errors, and prevent unverified annotations from silently entering production training datasets.

Annotation quality should be represented using measurable criteria appropriate to each task. Object detection may use intersection over union, segmentation can evaluate mask agreement, keypoints can use positional error, and classification can use annotator agreement. Three-dimensional and temporal tasks require their own consistency measures. Quality metadata should remain connected to the annotation version so that corrections do not erase evidence about earlier labeling quality.

Dataset splitting must be modeled explicitly rather than implemented only through temporary training scripts. Samples may belong to training, validation, test, calibration, or benchmark partitions. Split assignment should be reproducible and protected against leakage, especially when sequential frames, repeated environments, identical robots, or related episodes are highly correlated. Group identifiers can ensure that related observations remain within the same partition when necessary.

Negative and difficult examples are important parts of a robotics training dataset and should be represented intentionally. Samples containing no target object, severe occlusion, unusual lighting, sensor degradation, rare obstacles, or failure conditions may be especially valuable for robust learning. Difficulty level, scenario tags, environmental conditions, and edge-case classifications can be stored as metadata so that training and evaluation can measure performance across meaningful operational conditions.

Training data should maintain lineage back to its original source. A transformed image, cropped region, downsampled point cloud, augmented sample, or synchronized multimodal package should reference the raw observation and processing operation from which it was produced. Transformation parameters, software versions, and pipeline identifiers allow engineers to reproduce generated datasets and determine whether a model issue originated from raw data, preprocessing, annotation, or augmentation.

Dataset versioning should capture changes in samples, annotations, taxonomies, splits, and preprocessing rules. Adding new data, correcting labels, modifying a class definition, or changing train-test partitions can alter model performance significantly. Immutable dataset versions and explicit parent-child relationships allow experiments to reference the exact data used for training and make comparisons between model generations scientifically meaningful.

Privacy and governance metadata should accompany training data when robots collect information in human environments. Images, audio, location records, or behavioral data may contain personally identifiable or sensitive information. The schema can record privacy classification, consent or legal basis where applicable, anonymization status, retention policy, geographic restrictions, and access level. Governance information should remain linked to derived datasets so restrictions are not lost during transformation.

Synthetic data should follow the same logical principles as real-world training data while preserving its origin. Simulation engine, scene identifier, asset versions, randomization parameters, environmental conditions, ground-truth generation method, and simulation version can be stored as provenance. A source_domain field can distinguish real, simulated, augmented, or generated samples, allowing experiments to measure domain gaps and control real-to-synthetic training ratios.

A standardized AI training data model ultimately connects raw robot observations, annotations, actions, language, provenance, quality, governance, and dataset versions into one traceable structure. The objective is not to force every AI task into an identical label format, but to provide common identities and relationships around task-specific schemas. This foundation enables reproducible perception, imitation learning, multimodal learning, Physical AI, and Vision-Language-Action training pipelines.

로보틱스를 위한 AI 학습 데이터(AI Training Data)는 단순한 원시 센서 파일과 라벨(Label) 이상을 표현해야 한다. 학습 시스템은 관측 데이터가 어떤 상황에서 생성되었는지에 대한 맥락에 의존하기 때문이다. 이미지, 포인트 클라우드(Point Cloud), 오디오, 힘 신호, 궤적, 행동, 로봇 상태 등이 모두 학습 샘플이 될 수 있다. 따라서 학습 데이터 모델은 센서 관측을 로봇 식별 정보, 타임스탬프, 공간적 맥락, 구성, 작업 정보, 어노테이션(Annotation), 데이터셋 출처 정보(Provenance)와 연결해야 한다.

학습 샘플(Training Sample)은 원본 데이터와 학습 메타데이터(Learning Metadata)를 연결하는 기본 단위이다. 응용 분야에 따라 하나의 샘플은 단일 이미지, 동기화된 다중 카메라 프레임, 라이다(LiDAR) 스캔, 짧은 시간 시퀀스 또는 전체 조작 에피소드(Manipulation Episode)를 나타낼 수 있다. 각 샘플은 안정적인 sample_id를 가지고 원본 파일, 타임스탬프, 센서, 로봇, 임무, 환경, 어노테이션 기록을 참조하되 대용량 바이너리 데이터를 불필요하게 중복하지 않아야 한다.

데이터셋 식별 정보(Dataset Identity)는 개별 샘플과 분리하여 데이터 집합 자체를 독립적으로 버전 관리하고 운영할 수 있도록 해야 한다. 데이터셋 레코드에는 dataset_id, 이름, 버전, 생성 시간, 작업 유형(Task Type), 소스 도메인(Source Domain), 라이선스, 소유권, 생명주기 상태(Lifecycle Status) 등이 포함될 수 있다. 또한 포함된 샘플과 어노테이션 스키마를 참조해야 한다. 이를 통해 동일한 원본 관측 데이터를 인지, 내비게이션, 조작 또는 멀티모달 학습용 서로 다른 데이터셋에 활용할 수 있다.

어노테이션 스키마(Annotation Schema)는 사람이 생성하거나 기계가 생성하거나 자동으로 도출한 라벨을 일관된 방식으로 표현하는 방법을 정의한다. 스키마에는 어노테이션 유형, 클래스 분류체계(Class Taxonomy), 기하 정보(Geometry), 속성, 신뢰도, 생성 출처, 버전을 정의해야 한다. 경계 상자(Bounding Box), 폴리곤(Polygon), 분할 마스크(Segmentation Mask), 키포인트(Keypoint), 3D 직육면체(3D Cuboid), 궤적, 행동, 언어 설명, 품질 플래그 등은 서로 다른 구조를 사용하지만 공통 식별 및 출처 규칙을 공유해야 한다.

클래스 분류체계(Class Taxonomy)는 어노테이션 라벨에 대해 통제된 의미 정의를 제공한다. person, pedestrian, human, worker와 같은 임의 문자열을 혼용하는 대신 데이터셋은 표준 클래스 식별자와 이름을 정의해야 한다. 계층적 분류체계(Hierarchical Taxonomy)를 사용하면 vehicle을 상위 클래스로 정의하고 forklift, truck, mobile robot 등을 하위 특수 클래스로 표현할 수 있어 세부 학습과 일반화된 학습 작업을 모두 지원할 수 있다.

2차원 비전 어노테이션(2D Vision Annotation)은 일반적으로 이미지를 참조하고 경계 상자, 폴리곤, 마스크 또는 키포인트를 이용하여 객체를 표현한다. 각 어노테이션은 해당 샘플, 클래스, 기하 정보, 속성, 가시성(Visibility), 가림 상태(Occlusion State), 필요한 경우 신뢰도를 식별해야 한다. 좌표는 이미지 원점(Image Origin), 너비와 높이의 해석 방법, 정규화(Normalization), 클리핑(Clipping) 규칙을 명시적으로 정의하여 학습 프레임워크마다 동일한 어노테이션을 다르게 해석하는 문제를 방지해야 한다.

3차원 어노테이션(3D Annotation)은 센서, 로봇, 지도 또는 전역 좌표 프레임에서 기하 정보가 표현될 수 있기 때문에 추가적인 공간 정의가 필요하다. 3D 직육면체는 중심 위치, 크기, 방향, 클래스, 속도, 추적 식별자(Tracking Identifier)를 포함할 수 있다. 어노테이션에는 생성 시 사용된 좌표 프레임(Coordinate Frame)과 보정 참조(Calibration Reference)를 명시해야 한다. 이러한 정보가 없으면 라이다, 카메라, 로봇, 월드 좌표 사이에서 라벨을 신뢰성 있게 변환할 수 없다.

시간적 어노테이션(Temporal Annotation)은 독립적인 프레임이 아니라 연속된 시퀀스에서 로봇 행동이나 객체 움직임을 학습해야 할 때 중요하다. track_id를 사용하여 여러 관측에서 동일한 객체를 연결하고, 시작 및 종료 타임스탬프를 이용해 행동이나 이벤트의 시간 구간을 정의할 수 있다. 내비게이션 및 조작 데이터셋은 관측, 상태, 행동, 보상(Reward), 작업 설명, 종료 조건(Termination Condition)을 시간 순서로 포함하는 전체 에피소드를 표현할 수 있다.

로봇 학습(Robot Learning)에는 일반적인 인지 라벨과 본질적으로 다른 행동 어노테이션(Action Annotation)이 필요하다. 행동은 속도 명령, 관절 목표값, 말단장치 자세(End-Effector Pose), 그리퍼 상태, 개별 스킬(Discrete Skill), 상위 수준 작업 프리미티브(Task Primitive) 등을 표현할 수 있다. 각 행동은 해당 행동이 실행된 시점의 관측 및 로봇 상태와 연결되어야 한다. 이러한 관측-행동 관계(Observation-Action Relationship)는 모방학습(Imitation Learning), 정책 학습(Policy Learning), 비전-언어-행동(Vision-Language-Action, VLA) 모델 학습의 핵심이다.

멀티모달 데이터셋(Multimodal Dataset)은 카메라, 라이다, IMU, 오디오, 힘 센서, 로봇 상태, 언어 정보 사이의 동기화 관계를 명시적으로 정의해야 한다. 이름이 유사한 파일이 동시에 수집되었다고 가정하는 대신 데이터 모델은 타임스탬프, 동기화 그룹(Synchronization Group), 보정 참조, 모달리티 식별자(Modality Identifier)를 보존해야 한다. 이를 통해 센서별 동작 주기가 서로 다르더라도 학습 파이프라인이 신뢰할 수 있는 멀티모달 샘플을 구성할 수 있다.

언어 어노테이션(Language Annotation)은 물리적 관측과 행동을 의미적 명령 또는 설명과 연결할 수 있다. 샘플이나 에피소드에는 작업 지시(Task Instruction), 장면 설명(Scene Description), 질의응답 쌍, 행동 설명, 객체 참조, 운영자 명령 등이 포함될 수 있다. 언어 레코드는 언어 종류, 어노테이션 출처, 타임스탬프 또는 시간적 범위, 객체·행동·에피소드와의 관계를 식별해야 한다. 이러한 관계는 멀티모달 파운데이션 모델(Multimodal Foundation Model)과 VLA 시스템에서 특히 중요하다.

어노테이션 출처 정보(Annotation Provenance)는 라벨이 어떤 방법으로 생성되었는지를 기록한다. 어노테이션은 사람 작업자(Human Annotator), 자동화 모델, 시뮬레이션 정답 데이터(Simulation Ground Truth), 규칙 기반 처리 또는 이들의 조합으로 생성될 수 있다. annotator_id, tool_version, model_version, creation_time, review_status, confidence 등의 필드를 이용하면 특정 라벨이 학습, 평가 또는 추가 검토에 적합한지를 판단하고 어노테이션 생성 과정을 재현할 수 있다.

사람 중심 어노테이션 워크플로(Human Annotation Workflow)는 단순히 라벨 있음과 없음으로 구분하기보다 여러 생명주기 상태를 필요로 한다. 어노테이션은 생성(Created), 제출(Submitted), 검토(Reviewed), 수정(Corrected), 승인(Approved), 거부(Rejected) 등의 상태를 거칠 수 있다. 품질 보증(Quality Assurance)이 중요한 경우 검토자 정보와 변경 이력을 보존해야 한다. 이를 통해 작업자 간 불일치를 측정하고 체계적인 라벨링 오류를 발견하며 검증되지 않은 어노테이션이 운영용 학습 데이터에 포함되는 것을 방지할 수 있다.

어노테이션 품질(Annotation Quality)은 각 작업에 적합한 측정 가능한 기준으로 표현해야 한다. 객체 검출(Object Detection)은 교집합 대비 합집합(Intersection over Union, IoU)을 사용할 수 있고, 분할(Segmentation)은 마스크 일치도를 평가할 수 있으며, 키포인트는 위치 오차를 사용할 수 있다. 분류 작업은 어노테이터 간 일치도(Annotator Agreement)를 사용할 수 있으며, 3차원 및 시간 기반 작업에는 각각 적합한 일관성 지표가 필요하다. 품질 메타데이터는 어노테이션 버전과 연결하여 수정 이후에도 이전 라벨의 품질 기록을 유지해야 한다.

데이터셋 분할(Dataset Split)은 임시 학습 스크립트에서만 처리하지 않고 데이터 모델에서 명시적으로 관리해야 한다. 샘플은 학습(Training), 검증(Validation), 테스트(Test), 보정(Calibration), 벤치마크(Benchmark) 파티션에 속할 수 있다. 특히 연속 프레임, 반복 환경, 동일 로봇 또는 관련 에피소드가 높은 상관관계를 가질 때 데이터 누출(Data Leakage)을 방지하도록 분할을 재현 가능하게 관리해야 한다. 필요한 경우 그룹 식별자를 사용하여 관련 관측이 동일한 파티션에 유지되도록 할 수 있다.

부정 샘플(Negative Example)과 어려운 샘플(Difficult Example)도 로봇 학습 데이터셋의 중요한 구성요소이며 의도적으로 관리해야 한다. 목표 객체가 없는 장면, 심각한 가림, 비정상적인 조명, 센서 성능 저하, 희귀 장애물, 실패 상황 등은 강건한 학습(Robust Learning)에 특히 가치가 있을 수 있다. 난이도 수준(Difficulty Level), 시나리오 태그, 환경 조건, 엣지 케이스(Edge Case) 분류를 메타데이터로 저장하면 의미 있는 운용 조건별로 학습 및 평가 성능을 분석할 수 있다.

학습 데이터는 원본 데이터까지 이어지는 데이터 계보(Data Lineage)를 유지해야 한다. 변환된 이미지, 잘라낸 영역, 다운샘플링된 포인트 클라우드, 증강 샘플(Augmented Sample), 동기화된 멀티모달 패키지는 모두 어떤 원본 관측과 처리 작업에서 생성되었는지를 참조해야 한다. 변환 매개변수, 소프트웨어 버전, 파이프라인 식별자를 기록하면 생성된 데이터셋을 재현하고 모델 문제가 원본 데이터, 전처리, 어노테이션 또는 데이터 증강(Data Augmentation) 중 어디에서 발생했는지 분석할 수 있다.

데이터셋 버전 관리(Dataset Versioning)는 샘플, 어노테이션, 분류체계, 데이터 분할, 전처리 규칙의 변경을 모두 추적해야 한다. 새로운 데이터를 추가하거나 잘못된 라벨을 수정하고 클래스 정의 또는 학습-테스트 분할을 변경하면 모델 성능이 크게 달라질 수 있다. 변경 불가능한 데이터셋 버전(Immutable Dataset Version)과 명시적인 부모-자식 관계(Parent-Child Relationship)를 사용하면 실험에서 실제 사용한 데이터를 정확하게 참조하고 서로 다른 모델 세대의 결과를 과학적으로 비교할 수 있다.

로봇이 사람이 존재하는 환경에서 데이터를 수집하는 경우 학습 데이터에는 개인정보 보호 및 거버넌스 메타데이터(Privacy and Governance Metadata)가 함께 포함되어야 한다. 이미지, 오디오, 위치 기록, 행동 데이터에는 개인 식별 정보나 민감 정보가 포함될 수 있다. 스키마에는 개인정보 분류, 필요한 경우 동의 또는 법적 근거, 익명화 상태(Anonymization Status), 보존 정책, 지리적 제한, 접근 수준을 기록할 수 있다. 이러한 거버넌스 정보는 파생 데이터셋에도 연결하여 데이터 변환 과정에서 제한 조건이 사라지지 않도록 해야 한다.

합성 데이터(Synthetic Data)는 실제 환경에서 수집된 학습 데이터와 동일한 논리적 원칙을 따르면서 생성 출처를 명확하게 보존해야 한다. 시뮬레이션 엔진(Simulation Engine), 장면 식별자, 자산 버전, 도메인 랜덤화(Domain Randomization) 매개변수, 환경 조건, 정답 데이터 생성 방법, 시뮬레이션 버전 등을 출처 정보로 저장할 수 있다. source_domain 필드를 사용하여 실제(Real), 시뮬레이션(Simulated), 증강(Augmented), 생성형(Generated) 샘플을 구분하면 도메인 갭(Domain Gap)을 분석하고 실제 데이터와 합성 데이터의 학습 비율을 통제할 수 있다.

표준화된 AI 학습 데이터 모델(Standardized AI Training Data Model)은 궁극적으로 로봇의 원시 관측 데이터, 어노테이션, 행동, 언어, 출처 정보, 품질, 거버넌스, 데이터셋 버전을 하나의 추적 가능한 구조로 연결한다. 목적은 모든 AI 작업을 하나의 동일한 라벨 형식으로 강제하는 것이 아니라 작업별 스키마(Task-Specific Schema)를 유지하면서 공통 식별자와 관계를 제공하는 것이다. 이러한 기반을 통해 재현 가능한 인지(Perception), 모방학습, 멀티모달 학습(Multimodal Learning), 피지컬 AI(Physical AI), 비전-언어-행동(Vision-Language-Action, VLA) 학습 파이프라인을 구축할 수 있다.

##  

## 01.07 Robot Diagnostic Data Model: DTC Code System

![](images/image7.png){width="7.268055555555556in" height="7.268055555555556in"}

A robot diagnostic data model provides a standardized structure for representing faults, warnings, degradation, recovery, and maintenance-relevant conditions across the complete robot system. Unlike ordinary telemetry, diagnostic data describes interpreted system conditions rather than continuous measurements alone. A diagnostic architecture should connect fault codes with robot identity, subsystem, component, timestamp, severity, operating state, supporting telemetry, and recommended service actions.

A Diagnostic Trouble Code, or DTC, acts as a stable identifier for a recognized abnormal condition. Instead of allowing individual software modules to generate arbitrary fault strings, the robot platform should maintain a controlled DTC namespace. Codes can represent conditions such as motor overtemperature, battery undervoltage, sensor communication loss, localization degradation, compute overload, brake failure, or safety-controller faults using consistent definitions across the fleet.

The DTC code structure should communicate enough hierarchy to identify where a problem belongs without embedding excessive implementation detail. A code may contain fields representing domain, subsystem, component, fault category, and specific condition. For example, mobility, power, perception, compute, communication, navigation, and safety can form top-level diagnostic domains. This structure allows operators and software to filter and aggregate faults at different levels.

Each DTC definition should be maintained separately from individual fault occurrences. The definition describes the permanent meaning of the code, including dtc_code, title, description, subsystem, component type, severity class, trigger condition, clearing condition, and recommended action. A diagnostic occurrence then references this definition and records when, where, and under what operating conditions the fault was actually detected.

Diagnostic severity indicates the operational impact of a condition and should not be confused with the DTC identity itself. A practical model can distinguish informational, warning, error, critical, and safety-critical conditions. Severity may determine whether the robot continues normally, enters degraded operation, pauses a mission, returns to a safe location, requests maintenance, or performs an immediate protective stop according to defined operational policies.

Fault lifecycle state is required because diagnostic conditions evolve after they are first detected. A fault may progress through detected, confirmed, active, intermittent, recovering, cleared, and archived states. Some conditions should be confirmed only after repeated observations or persistence thresholds to prevent transient noise from generating unnecessary alarms. Maintaining lifecycle state allows systems to distinguish a historical fault from a condition that currently affects robot operation.

Trigger conditions should be explicitly defined and traceable to measurable evidence. A motor overtemperature DTC may activate when temperature remains above a threshold for a defined duration, while communication-loss diagnostics may depend on missed messages or heartbeat timeouts. Diagnostic logic should identify the signal, threshold, duration, hysteresis, and evaluation method whenever possible so that fault generation can be reproduced and validated.

Clearing logic requires the same level of definition as activation logic. A fault should not disappear simply because a measurement briefly returns to normal. Recovery may require a lower threshold, a stable period, a successful self-test, operator acknowledgement, component restart, or maintenance action. Explicit clearing conditions prevent diagnostic oscillation and make the transition from abnormal to healthy state understandable to operators and automated systems.

A diagnostic occurrence should capture a snapshot of relevant operational context when a fault is detected. Robot mode, mission identifier, task, pose, battery state, velocity, software version, configuration revision, and environmental information may be useful depending on the fault. This diagnostic snapshot provides immediate evidence for troubleshooting without requiring every investigation to search through large volumes of unrelated historical telemetry.

Diagnostic data should remain linked to supporting telemetry for deeper analysis. A DTC occurrence can define a time window before and after detection so that engineers can retrieve motor current, temperature, vibration, CPU utilization, network latency, localization confidence, or other relevant signals. This relationship allows events to identify the important moment while time-series data explains the physical or computational behavior that led to the fault.

Freeze-frame data can provide a compact representation of critical measurements captured at the instant a diagnostic condition becomes active. The concept is similar to diagnostic snapshots used in other engineered systems, but robot-specific freeze frames may include navigation state, sensor health, compute load, safety state, mission context, and pose. The selected fields should depend on the diagnostic domain rather than forcing every fault to store identical data.

Subsystem and component identity must be explicit because the same DTC condition may occur on multiple physical devices. A robot may contain several drive motors, cameras, LiDARs, batteries, network interfaces, or compute modules. component_id, subsystem_id, hardware serial number, firmware version, and installation position allow maintenance teams to identify exactly which component generated the fault and correlate repeated failures with specific hardware.

Diagnostic codes should distinguish symptoms from root causes. A localization failure, for example, may be observed as a navigation problem while its underlying cause could be LiDAR communication loss, camera obstruction, map inconsistency, or compute overload. The diagnostic model should support relationships such as caused_by, related_to, consequence_of, or suspected_cause without assuming that every detected symptom immediately identifies the true root cause.

Fault aggregation and correlation become important when one physical problem generates many secondary DTCs. A power failure may trigger communication, sensor, navigation, and compute faults almost simultaneously. Without correlation, operators may see dozens of independent alarms for one incident. Correlation rules, causation identifiers, timestamps, and component relationships can group related diagnostic occurrences into an incident while preserving the original fault records.

Diagnostic data should support both onboard and fleet-level processing. The robot must detect conditions requiring immediate local response even when network connectivity is unavailable, while the fleet platform can correlate recurring faults across many robots. Local diagnostics therefore focus on fast detection and safe reaction, whereas centralized analytics can identify fleet trends, component reliability issues, site-specific patterns, and candidates for predictive maintenance.

DTC history should be retained even after a condition is cleared because recurrence patterns are valuable for maintenance and reliability analysis. Fields such as first_seen, last_seen, occurrence_count, active_duration, clear_count, and previous occurrences can summarize repeated behavior. Historical diagnostic information helps distinguish isolated transient faults from systematic degradation and supports metrics such as failure frequency, downtime, and mean time between failures.

Maintenance actions should be connected directly to diagnostic records when service is performed. A technician may inspect wiring, clean a sensor, update firmware, recalibrate a component, replace hardware, or confirm that no defect is present. Linking work orders, service actions, replaced component identifiers, technician findings, and closure results to DTC occurrences creates traceability from fault detection through diagnosis, repair, verification, and return to service.

Predictive diagnostics extend the model beyond discrete threshold-based DTCs. Vibration trends, battery degradation, motor current signatures, temperature patterns, or AI anomaly scores may indicate emerging problems before a hard failure occurs. The data model can represent predicted faults with confidence, estimated time to failure, model version, contributing signals, and recommendation while clearly distinguishing predictions from confirmed diagnostic conditions.

Diagnostic schema versioning is necessary as robot platforms evolve. New hardware, software, sensors, and algorithms introduce new DTCs, while existing diagnostic definitions may require refinement. Each code definition should have controlled version information and lifecycle status such as proposed, active, deprecated, or retired. Historical occurrences must continue to reference the diagnostic definition that was valid when the fault was generated.

Governance is particularly important for safety-related DTCs because changes to thresholds, severity, clearing logic, or automatic responses can alter robot behavior. Ownership, approval, validation evidence, software release, configuration revision, and change history should therefore be traceable. Diagnostic definitions that influence protective stops or safety functions require stronger change control than ordinary maintenance notifications or informational warnings.

A standardized robot diagnostic data model ultimately creates a common fault language across robot control, fleet management, maintenance, safety, engineering, and analytics systems. DTC definitions identify abnormal conditions, occurrences record actual faults, lifecycle states describe their progression, telemetry provides evidence, and maintenance records capture resolution. Together these relationships transform isolated error messages into structured and traceable engineering information.

This diagnostic foundation supports faster troubleshooting, fleet reliability analysis, automated recovery, predictive maintenance, digital twins, and continuous engineering improvement. When diagnostic codes are connected with configuration, telemetry, events, spatial context, software versions, and component history, engineers can reconstruct not only what failed but also the conditions surrounding the failure and whether similar patterns exist across the wider robot fleet.

로봇 진단 데이터 모델(Robot Diagnostic Data Model)은 전체 로봇 시스템에서 발생하는 고장, 경고, 성능 저하, 복구, 유지보수 관련 상태를 표준화된 구조로 표현한다. 일반적인 텔레메트리(Telemetry)와 달리 진단 데이터는 연속적인 측정값 자체보다 이를 해석하여 판단한 시스템 상태를 나타낸다. 진단 아키텍처(Diagnostic Architecture)는 고장 코드를 로봇 식별 정보, 서브시스템, 구성요소, 타임스탬프, 심각도, 운용 상태, 관련 텔레메트리, 권장 정비 조치와 연결해야 한다.

진단 고장 코드(Diagnostic Trouble Code, DTC)는 인식된 비정상 상태를 나타내는 안정적인 식별자 역할을 한다. 개별 소프트웨어 모듈이 임의의 고장 문자열을 생성하도록 하는 대신 로봇 플랫폼 전체에서 통제된 DTC 네임스페이스(DTC Namespace)를 관리해야 한다. 모터 과열, 배터리 저전압, 센서 통신 손실, 위치추정 성능 저하, 컴퓨팅 과부하, 브레이크 고장, 안전 제어기 고장 등을 플릿 전체에서 일관된 정의로 표현할 수 있다.

DTC 코드 구조(DTC Code Structure)는 과도한 구현 세부사항을 포함하지 않으면서 문제가 어느 영역에 속하는지를 식별할 수 있는 충분한 계층 구조를 제공해야 한다. 하나의 코드는 도메인, 서브시스템, 구성요소, 고장 범주, 세부 상태를 나타내는 필드로 구성할 수 있다. 이동(Mobility), 전원(Power), 인지(Perception), 컴퓨팅(Compute), 통신(Communication), 내비게이션(Navigation), 안전(Safety) 등을 최상위 진단 도메인으로 구성하면 운영자와 소프트웨어가 다양한 수준에서 고장을 분류하고 집계할 수 있다.

각 DTC 정의(DTC Definition)는 개별 고장 발생 기록과 분리하여 관리해야 한다. 정의에는 코드의 영구적인 의미를 나타내는 dtc_code, 제목, 설명, 서브시스템, 구성요소 유형, 심각도 등급, 발생 조건(Trigger Condition), 해제 조건(Clearing Condition), 권장 조치(Recommended Action) 등이 포함된다. 실제 진단 발생 기록(Diagnostic Occurrence)은 이 정의를 참조하면서 언제, 어디에서, 어떤 운용 조건에서 고장이 감지되었는지를 기록한다.

진단 심각도(Diagnostic Severity)는 상태가 로봇 운용에 미치는 영향을 나타내며 DTC 자체의 식별 정보와 혼동해서는 안 된다. 실용적인 모델에서는 정보(Informational), 경고(Warning), 오류(Error), 치명적(Critical), 안전 치명적(Safety-Critical) 상태를 구분할 수 있다. 심각도에 따라 로봇은 정상 운행을 계속하거나 성능 저하 모드로 전환하고, 임무를 일시 정지하거나 안전한 위치로 복귀하며, 정비를 요청하거나 즉각적인 보호 정지(Protective Stop)를 수행할 수 있다.

진단 상태는 최초 감지 이후 지속적으로 변화하기 때문에 고장 생명주기 상태(Fault Lifecycle State)가 필요하다. 고장은 감지(Detected), 확인(Confirmed), 활성(Active), 간헐적(Intermittent), 복구 중(Recovering), 해제(Cleared), 보관(Archived) 상태를 거칠 수 있다. 일시적인 노이즈로 불필요한 경고가 발생하는 것을 방지하기 위해 일부 상태는 반복적인 관측이나 지속 시간 임계값을 충족한 후에만 확정할 수 있다. 이러한 상태 관리를 통해 현재 운용에 영향을 주는 고장과 과거의 고장을 구분할 수 있다.

발생 조건(Trigger Condition)은 명확하게 정의하고 측정 가능한 근거까지 추적할 수 있어야 한다. 예를 들어 모터 과열 DTC는 온도가 특정 임계값 이상으로 일정 시간 지속될 때 활성화될 수 있으며, 통신 손실 진단은 메시지 누락이나 하트비트 타임아웃(Heartbeat Timeout)을 기준으로 판단할 수 있다. 가능한 경우 진단 로직은 신호, 임계값, 지속 시간, 히스테리시스(Hysteresis), 평가 방법을 정의하여 고장 발생 과정을 재현하고 검증할 수 있도록 해야 한다.

해제 로직(Clearing Logic)도 활성화 로직과 동일한 수준으로 명확하게 정의해야 한다. 측정값이 잠시 정상 범위로 돌아왔다고 해서 고장이 즉시 사라져서는 안 된다. 복구에는 더 낮은 해제 임계값, 일정한 정상 유지 시간, 성공적인 자체 진단(Self-Test), 운영자 확인, 구성요소 재시작 또는 유지보수 조치가 필요할 수 있다. 명확한 해제 조건은 진단 상태가 반복적으로 활성화와 해제를 오가는 현상을 방지하고 비정상 상태에서 정상 상태로 전환되는 과정을 이해할 수 있게 한다.

진단 발생 기록(Diagnostic Occurrence)은 고장이 감지된 시점의 관련 운용 상황을 스냅샷(Snapshot)으로 저장해야 한다. 고장의 특성에 따라 로봇 모드, 임무 식별자, 작업, 자세(Pose), 배터리 상태, 속도, 소프트웨어 버전, 구성 리비전(Configuration Revision), 환경 정보 등이 포함될 수 있다. 이러한 진단 스냅샷(Diagnostic Snapshot)은 문제 해결 시 방대한 과거 텔레메트리 전체를 먼저 검색하지 않고도 중요한 초기 근거를 제공한다.

진단 데이터는 보다 심층적인 분석을 위해 관련 텔레메트리(Supporting Telemetry)와 연결되어야 한다. 하나의 DTC 발생 기록은 고장 감지 전후의 시간 구간을 정의하여 모터 전류, 온도, 진동, CPU 사용률, 네트워크 지연, 위치추정 신뢰도 등의 관련 신호를 조회할 수 있도록 구성할 수 있다. 이러한 관계를 통해 이벤트는 중요한 시점을 식별하고 시계열 데이터(Time-Series Data)는 해당 고장에 이르게 된 물리적 또는 계산적 동작을 설명할 수 있다.

프리즈 프레임 데이터(Freeze-Frame Data)는 진단 상태가 활성화되는 순간의 주요 측정값을 압축된 형태로 제공할 수 있다. 다른 공학 시스템에서 사용하는 진단 스냅샷과 유사하지만 로봇 전용 프리즈 프레임에는 내비게이션 상태, 센서 상태, 컴퓨팅 부하, 안전 상태, 임무 맥락, 자세 정보 등이 포함될 수 있다. 모든 고장에 동일한 데이터를 강제하기보다 각 진단 도메인의 특성에 따라 필요한 필드를 선택해야 한다.

동일한 DTC 상태가 여러 물리적 장치에서 발생할 수 있으므로 서브시스템 및 구성요소 식별 정보(Subsystem and Component Identity)를 명확히 해야 한다. 하나의 로봇에는 여러 구동 모터, 카메라, 라이다, 배터리, 네트워크 인터페이스, 컴퓨팅 모듈이 존재할 수 있다. component_id, subsystem_id, 하드웨어 일련번호, 펌웨어 버전, 설치 위치를 기록하면 유지보수 담당자가 어떤 구성요소에서 고장이 발생했는지 정확히 식별하고 반복 고장을 특정 하드웨어와 연계할 수 있다.

진단 코드는 증상(Symptom)과 근본 원인(Root Cause)을 구분해야 한다. 예를 들어 위치추정 실패는 내비게이션 문제로 관측될 수 있지만 실제 원인은 라이다 통신 손실, 카메라 가림, 지도 불일치, 컴퓨팅 과부하일 수 있다. 진단 모델은 모든 증상이 즉시 실제 원인을 의미한다고 가정하지 않고 caused_by, related_to, consequence_of, suspected_cause와 같은 관계를 지원하여 원인과 결과를 구조적으로 연결할 수 있어야 한다.

하나의 물리적 문제가 여러 개의 2차 DTC를 발생시키는 경우 고장 집계(Fault Aggregation)와 상관관계 분석(Correlation)이 중요하다. 예를 들어 전원 고장은 거의 동시에 통신, 센서, 내비게이션, 컴퓨팅 고장을 발생시킬 수 있다. 상관관계가 없으면 운영자는 하나의 사고를 수십 개의 독립적인 경보로 인식할 수 있다. 상관관계 규칙, 인과관계 식별자, 타임스탬프, 구성요소 관계를 이용하면 원본 고장 기록을 유지하면서 관련 진단 발생을 하나의 사고(Incident)로 묶을 수 있다.

진단 데이터는 온보드(Onboard) 처리와 플릿 수준(Fleet-Level) 처리를 모두 지원해야 한다. 네트워크 연결이 없는 상황에서도 즉각적인 대응이 필요한 상태는 로봇 자체에서 감지해야 하며, 플릿 플랫폼에서는 여러 로봇에서 반복되는 고장을 분석할 수 있어야 한다. 로컬 진단(Local Diagnostics)은 빠른 감지와 안전 대응에 집중하고, 중앙 분석(Centralized Analytics)은 플릿 추세, 부품 신뢰성 문제, 현장별 패턴, 예지보전(Predictive Maintenance) 대상을 식별하는 데 활용할 수 있다.

DTC 이력(DTC History)은 고장이 해제된 이후에도 보존해야 한다. 반복 발생 패턴은 유지보수 및 신뢰성 분석에 중요한 정보를 제공하기 때문이다. first_seen, last_seen, occurrence_count, active_duration, clear_count, previous occurrences 등의 필드를 사용하여 반복적인 동작을 요약할 수 있다. 과거 진단 정보는 일시적인 단발성 고장과 체계적인 성능 저하를 구분하고 고장 빈도, 가동 중단 시간(Downtime), 평균 고장 간격(Mean Time Between Failures, MTBF) 등의 지표를 분석하는 데 활용된다.

정비가 수행되는 경우 유지보수 조치(Maintenance Action)를 진단 기록과 직접 연결해야 한다. 기술자는 배선을 검사하거나 센서를 청소하고, 펌웨어를 업데이트하거나 구성요소를 재보정하고, 하드웨어를 교체하거나 실제 결함이 없음을 확인할 수 있다. 작업 지시서(Work Order), 정비 조치, 교체된 구성요소 식별자, 기술자 판단, 종료 결과를 DTC 발생 기록과 연결하면 고장 감지부터 진단, 수리, 검증, 서비스 복귀까지 전체 과정을 추적할 수 있다.

예측 진단(Predictive Diagnostics)은 단순한 임계값 기반 DTC를 넘어 잠재적인 고장을 사전에 표현한다. 진동 추세, 배터리 열화, 모터 전류 특성, 온도 패턴, AI 이상 점수(Anomaly Score)는 실제 고장이 발생하기 전에 문제의 징후를 나타낼 수 있다. 데이터 모델은 예측 고장을 신뢰도(Confidence), 예상 고장 시점(Estimated Time to Failure), 모델 버전, 기여 신호(Contributing Signal), 권장 조치와 함께 표현하되 예측 결과와 확정된 진단 상태를 명확히 구분해야 한다.

로봇 플랫폼은 지속적으로 발전하므로 진단 스키마 버전 관리(Diagnostic Schema Versioning)가 필요하다. 새로운 하드웨어, 소프트웨어, 센서, 알고리즘이 추가되면서 새로운 DTC가 생성되고 기존 진단 정의도 개선될 수 있다. 각 코드 정의에는 통제된 버전 정보와 제안(Proposed), 활성(Active), 사용 중단 예정(Deprecated), 폐기(Retired) 등의 생명주기 상태를 포함해야 한다. 과거의 고장 발생 기록은 당시 유효했던 진단 정의를 계속 참조해야 한다.

안전 관련 DTC(Safety-Related DTC)는 임계값, 심각도, 해제 로직, 자동 대응 방식의 변경이 실제 로봇 동작에 영향을 줄 수 있으므로 특히 강력한 거버넌스(Governance)가 필요하다. 소유권, 승인, 검증 근거(Validation Evidence), 소프트웨어 릴리스, 구성 리비전, 변경 이력을 추적할 수 있어야 한다. 보호 정지나 안전 기능에 영향을 주는 진단 정의는 일반적인 유지보수 알림이나 정보성 경고보다 엄격한 변경 관리(Change Control)가 필요하다.

표준화된 로봇 진단 데이터 모델(Standardized Robot Diagnostic Data Model)은 궁극적으로 로봇 제어, 플릿 관리, 유지보수, 안전, 엔지니어링, 분석 시스템이 공유할 수 있는 공통 고장 언어(Common Fault Language)를 구축한다. DTC 정의는 비정상 상태의 의미를 식별하고, 발생 기록은 실제 고장을 기록하며, 생명주기 상태는 고장의 진행 과정을 설명한다. 텔레메트리는 고장의 근거를 제공하고 유지보수 기록은 해결 과정을 기록함으로써 단순한 오류 메시지를 구조화되고 추적 가능한 엔지니어링 정보로 전환한다.

이러한 진단 기반(Diagnostic Foundation)은 신속한 문제 해결, 플릿 신뢰성 분석(Fleet Reliability Analysis), 자동 복구(Automated Recovery), 예지보전, 디지털 트윈(Digital Twin), 지속적인 엔지니어링 개선을 지원한다. 진단 코드를 구성(Configuration), 텔레메트리, 이벤트, 공간적 맥락(Spatial Context), 소프트웨어 버전, 구성요소 이력과 연결하면 엔지니어는 무엇이 고장 났는지만 확인하는 것이 아니라 고장이 발생한 주변 조건과 동일한 패턴이 전체 로봇 플릿에서 반복되고 있는지까지 추적하고 분석할 수 있다.

##  

## 01.08 Multi-Robot Fleet Data Model: Robot ID / Task Tracking

![](images/image8.png){width="7.268055555555556in" height="7.268055555555556in"}

A multi-robot fleet data model provides a common structure for identifying robots, assigning work, tracking execution, and coordinating shared resources across heterogeneous autonomous systems. A fleet may contain robots with different hardware, software, mobility, payloads, and capabilities, yet fleet applications require a consistent operational view. The model should therefore connect fleet identity, robot identity, capabilities, availability, missions, tasks, locations, status, and execution history.

Robot identity is the fundamental reference for every fleet-level operation. Each physical robot should have a persistent robot_id that remains stable across software updates, network changes, maintenance activities, and mission assignments. Human-readable names may change for operational convenience, but the underlying identifier should remain immutable. Telemetry, events, diagnostics, configuration, missions, tasks, and maintenance records can then reference the same robot throughout its lifecycle.

Fleet identity provides an organizational boundary for groups of robots managed under a common operational context. A fleet_id can represent robots belonging to a facility, customer, region, service organization, or management domain. Large deployments may introduce hierarchical structures such as organization, site, fleet, group, and robot. This hierarchy enables access control, reporting, policy management, and workload allocation without embedding organizational assumptions directly into individual robot records.

A fleet robot record should provide a concise operational representation rather than duplicate the complete robot configuration. Typical attributes include robot_id, fleet_id, robot_type, model, capability references, current availability, operating mode, active mission, location reference, connectivity status, and configuration version. Detailed hardware or software information can remain in the configuration domain while the fleet model references only information required for scheduling and coordination.

Capability modeling allows fleet systems to determine whether a robot is suitable for a particular task. Capabilities may represent autonomous navigation, towing, delivery, inspection, manipulation, elevator use, outdoor operation, payload handling, or specialized sensing. Tasks can declare required capabilities, and the scheduler can compare these requirements against robot capability profiles. This approach avoids assigning work solely according to robot model names or manually maintained operator knowledge.

Availability should be modeled separately from basic connectivity because an online robot is not necessarily ready to accept work. A robot may be available, busy, charging, reserved, degraded, under maintenance, paused, or unavailable because of a safety condition. Availability can be derived from operational state, diagnostics, energy level, mission status, and maintenance constraints. Explicit availability semantics allow scheduling systems to make consistent assignment decisions.

Mission and task concepts should be separated because they represent different levels of work. A mission describes an operational objective such as delivering material from one area to another, performing an inspection route, or executing a sequence of service activities. A task represents an individual executable step within that mission. One mission may therefore contain navigation, pickup, waiting, docking, manipulation, inspection, or delivery tasks linked through execution dependencies.

Every mission should have a stable mission_id and maintain references to requesting systems, assigned robots, priority, creation time, operational context, and lifecycle state. Mission states may include created, queued, assigned, accepted, executing, paused, completed, cancelled, or failed. State transitions should be recorded as events rather than simply overwriting the current value, allowing fleet systems to reconstruct the complete history of how a mission progressed.

Tasks require their own task_id because individual steps may be retried, reassigned, skipped, or executed by different robots. A task record can describe task type, required capabilities, origin, destination, parameters, dependencies, priority, assigned robot, execution state, and timing information. Stable task identity enables precise tracking when complex missions contain many steps or when work is transferred between robots following failure or operational changes.

Task dependencies define the execution relationships between activities. Some tasks must execute sequentially, while others may run in parallel or begin only after a specified condition is satisfied. Relationships such as depends_on, follows, blocks, parallel_with, and optional_after can describe workflow structure. Modeling these dependencies explicitly allows the fleet system to represent complex operations without embedding every workflow rule directly into robot-specific control logic.

Assignment records should preserve the relationship between a robot and a task independently of the task definition itself. An assignment may include assignment_id, robot_id, task_id, assigned_time, accepted_time, assignment_status, scheduler version, and selection reason. This separation is useful because the same task may be offered to one robot, rejected, reassigned, and eventually completed by another robot while retaining a complete decision history.

Task tracking requires more than a simple completed flag. Execution may progress through queued, assigned, accepted, started, in_progress, paused, blocked, completed, failed, cancelled, or retrying states. Each transition should have a timestamp and relevant context. Current status provides an efficient operational view, while the transition history supports performance analysis, incident reconstruction, service-level measurement, and debugging of scheduling or robot behavior.

Time information is essential for fleet performance analysis. Missions and tasks may record requested_time, queued_time, assigned_time, start_time, completion_time, deadline, estimated_duration, and actual_duration. These timestamps support metrics such as queue delay, assignment latency, travel time, execution duration, completion rate, and deadline compliance. Estimated values should remain distinguishable from actual observations so prediction accuracy can also be evaluated.

Spatial references connect task tracking with the environment in which work occurs. Origins, destinations, waypoints, zones, maps, floors, charging stations, elevators, and docking locations should use stable spatial identifiers rather than arbitrary coordinate values embedded repeatedly in tasks. A task can reference a location object while the spatial data model provides coordinates, map version, access constraints, and semantic information required by navigation systems.

Robot position should remain linked to task execution when location is operationally relevant. The fleet system may maintain a current pose reference or summarized location such as zone, floor, or waypoint while high-frequency position data remains in the spatial or telemetry domain. This separation prevents fleet databases from becoming overloaded with continuous localization updates while still enabling scheduling decisions based on where robots currently operate.

Energy state influences task assignment because a technically capable robot may not have sufficient battery reserve to complete a mission safely. Fleet records can expose state of charge, estimated remaining runtime, charging state, and energy availability derived from telemetry. Scheduling policies may consider expected task energy, travel distance, charging opportunities, and minimum reserve constraints when deciding whether to assign work or send a robot to charge.

Resource tracking is required when multiple robots share infrastructure. Charging stations, elevators, doors, loading areas, work cells, narrow corridors, and docking points can be represented as shared resources with stable resource_id values. Reservation records can specify which robot or task owns a resource and for what time window. This allows coordination systems to prevent conflicts and provides historical evidence when congestion or resource contention affects mission performance.

Traffic coordination can extend the fleet data model with route reservations, lane ownership, intersection access, and temporary spatial locks. Rather than representing every navigation command centrally, the fleet layer can track coordination objects that affect multiple robots. Reservation identifiers, robot references, spatial segments, priorities, validity intervals, and release states provide a structured basis for managing shared movement without tightly coupling fleet logic to a specific navigation implementation.

Failure and reassignment must be treated as normal lifecycle conditions rather than exceptional database states. A task may fail because of robot faults, blocked routes, insufficient energy, communication loss, unavailable infrastructure, or external cancellation. The failure record should preserve reason, diagnostic references, execution context, and retry policy. If the task is reassigned, the original attempt should remain in history rather than being overwritten by the new assignment.

Heterogeneous fleets require abstraction boundaries between fleet-level semantics and vendor-specific robot interfaces. A fleet model should describe common concepts such as robot, mission, task, capability, state, location, and assignment without requiring every robot to expose identical internal structures. Adapters can translate proprietary APIs, ROS 2 interfaces, or interoperability standards into the common model while preserving vendor-specific extensions when necessary.

Fleet data should retain correlation identifiers that connect operational records across distributed systems. A mission may originate in a warehouse, hospital, manufacturing, or enterprise application and generate multiple tasks, commands, events, and robot actions. Correlation identifiers make these records traceable as one business transaction. This allows operators to follow an external request through scheduling, robot execution, completion, and downstream confirmation.

Historical fleet data supports optimization beyond immediate monitoring. Mission completion rates, task duration, robot utilization, charging frequency, travel distance, idle time, failure rates, reassignment counts, and resource congestion can be calculated from structured histories. These metrics can reveal whether performance problems originate from individual robots, scheduling policies, facility layouts, infrastructure limitations, or workload patterns.

A standardized multi-robot fleet data model ultimately creates a shared operational language across heterogeneous robots and higher-level applications. Stable robot, mission, task, assignment, location, and resource identities make every execution step traceable. Combined with lifecycle states, timestamps, capabilities, availability, energy, diagnostics, and spatial references, the model enables reliable scheduling, task tracking, fleet coordination, analytics, and scalable autonomous operations.

다중 로봇 플릿 데이터 모델(Multi-Robot Fleet Data Model)은 서로 다른 자율 시스템으로 구성된 환경에서 로봇을 식별하고, 작업을 할당하며, 실행 상태를 추적하고, 공유 자원을 조정하기 위한 공통 구조를 제공한다. 하나의 플릿(Fleet)에는 서로 다른 하드웨어, 소프트웨어, 이동 방식, 적재 능력, 기능을 가진 로봇이 포함될 수 있지만 플릿 애플리케이션은 일관된 운용 관점을 필요로 한다. 따라서 데이터 모델은 플릿 식별자, 로봇 식별자, 기능, 가용성, 임무, 작업, 위치, 상태, 실행 이력을 서로 연결해야 한다.

로봇 식별자(Robot Identity)는 모든 플릿 수준 운용을 위한 기본 참조 정보이다. 각각의 물리적 로봇에는 소프트웨어 업데이트, 네트워크 변경, 유지보수 작업, 임무 할당 이후에도 변하지 않는 영구적인 robot_id가 필요하다. 운영 편의를 위한 사람이 읽을 수 있는 이름은 변경될 수 있지만 기본 식별자는 불변(Immutable)으로 유지해야 한다. 이를 통해 텔레메트리, 이벤트, 진단, 구성, 임무, 작업, 유지보수 기록이 로봇의 전체 생명주기 동안 동일한 로봇을 참조할 수 있다.

플릿 식별자(Fleet Identity)는 공통 운용 환경에서 관리되는 로봇 그룹의 조직적 경계를 제공한다. fleet_id는 특정 시설, 고객, 지역, 서비스 조직 또는 관리 도메인에 속하는 로봇을 나타낼 수 있다. 대규모 배포에서는 조직(Organization), 사이트(Site), 플릿(Fleet), 그룹(Group), 로봇(Robot)과 같은 계층 구조를 도입할 수 있다. 이를 통해 개별 로봇 레코드에 조직 구조를 직접 포함하지 않고도 접근 제어, 보고, 정책 관리, 작업량 할당을 수행할 수 있다.

플릿 로봇 레코드(Fleet Robot Record)는 전체 로봇 구성을 중복 저장하기보다 간결한 운용 정보를 제공해야 한다. 일반적인 속성에는 robot_id, fleet_id, robot_type, model, 기능 참조(Capability Reference), 현재 가용성, 운용 모드, 활성 임무, 위치 참조, 연결 상태, 구성 버전 등이 포함된다. 상세한 하드웨어와 소프트웨어 정보는 구성 도메인(Configuration Domain)에 유지하고 플릿 모델에서는 스케줄링과 조정에 필요한 정보만 참조할 수 있다.

기능 모델링(Capability Modeling)은 플릿 시스템이 특정 작업에 적합한 로봇을 판단할 수 있게 한다. 기능에는 자율주행, 견인, 배송, 검사, 조작, 엘리베이터 이용, 실외 운용, 적재물 처리, 특수 센싱 등이 포함될 수 있다. 작업은 필요한 기능(Required Capabilities)을 선언하고 스케줄러(Scheduler)는 이를 로봇의 기능 프로파일과 비교할 수 있다. 이러한 방식은 단순한 로봇 모델명이나 운영자의 수동 지식에 의존하여 작업을 할당하는 문제를 방지한다.

가용성(Availability)은 기본적인 연결 상태(Connectivity)와 분리하여 모델링해야 한다. 온라인 상태의 로봇이라고 해서 반드시 새로운 작업을 수행할 준비가 되어 있는 것은 아니다. 로봇은 가용(Available), 작업 중(Busy), 충전 중(Charging), 예약됨(Reserved), 성능 저하(Degraded), 유지보수 중(Maintenance), 일시 정지(Paused), 안전 상태로 인한 사용 불가(Unavailable) 등의 상태가 될 수 있다. 가용성은 운용 상태, 진단, 에너지 수준, 임무 상태, 유지보수 제약으로부터 결정될 수 있으며 명확한 의미 정의를 통해 일관된 작업 할당을 지원한다.

임무(Mission)와 작업(Task)은 서로 다른 작업 수준을 나타내므로 분리해야 한다. 임무는 한 구역에서 다른 구역으로 자재를 운반하거나 검사 경로를 수행하고 일련의 서비스 활동을 실행하는 것과 같은 운용 목적을 의미한다. 작업은 해당 임무를 구성하는 개별 실행 단계이다. 따라서 하나의 임무에는 실행 종속성(Execution Dependency)으로 연결된 이동, 픽업, 대기, 도킹, 조작, 검사, 배송 등의 여러 작업이 포함될 수 있다.

각 임무는 안정적인 mission_id를 가져야 하며 요청 시스템, 할당된 로봇, 우선순위, 생성 시간, 운용 맥락, 생명주기 상태(Lifecycle State)를 참조해야 한다. 임무 상태에는 생성(Created), 대기(Queued), 할당(Assigned), 수락(Accepted), 실행 중(Executing), 일시 정지(Paused), 완료(Completed), 취소(Cancelled), 실패(Failed) 등이 포함될 수 있다. 상태 변경은 현재 값을 단순히 덮어쓰기보다 이벤트(Event)로 기록하여 임무가 어떤 과정을 거쳐 진행되었는지 전체 이력을 재구성할 수 있어야 한다.

개별 단계는 재시도, 재할당, 건너뛰기 또는 서로 다른 로봇에 의해 실행될 수 있으므로 작업에도 독립적인 task_id가 필요하다. 작업 레코드는 작업 유형, 요구 기능, 출발지, 목적지, 매개변수, 종속성, 우선순위, 할당된 로봇, 실행 상태, 시간 정보를 표현할 수 있다. 안정적인 작업 식별자는 복잡한 임무에 여러 단계가 포함되거나 고장 및 운용 변경으로 작업이 다른 로봇에 이전되는 경우에도 정확한 추적을 가능하게 한다.

작업 종속성(Task Dependency)은 활동 사이의 실행 관계를 정의한다. 일부 작업은 순차적으로 실행해야 하고 다른 작업은 병렬로 수행하거나 특정 조건이 만족된 이후에만 시작할 수 있다. depends_on, follows, blocks, parallel_with, optional_after 등의 관계를 사용하여 워크플로 구조(Workflow Structure)를 표현할 수 있다. 이러한 종속성을 명시적으로 모델링하면 모든 워크플로 규칙을 로봇별 제어 로직에 직접 구현하지 않고도 복잡한 운용 절차를 표현할 수 있다.

할당 레코드(Assignment Record)는 로봇과 작업 사이의 관계를 작업 정의 자체와 독립적으로 보존해야 한다. 할당에는 assignment_id, robot_id, task_id, assigned_time, accepted_time, assignment_status, scheduler_version, selection_reason 등이 포함될 수 있다. 동일한 작업이 한 로봇에 제안되었다가 거절되고 다른 로봇으로 재할당되어 최종적으로 완료될 수 있으므로 이러한 분리는 전체 할당 결정 이력(Decision History)을 유지하는 데 유용하다.

작업 추적(Task Tracking)은 단순한 완료 여부 이상의 상태를 표현해야 한다. 실행 과정은 대기(Queued), 할당(Assigned), 수락(Accepted), 시작(Started), 진행 중(In Progress), 일시 정지(Paused), 차단(Blocked), 완료(Completed), 실패(Failed), 취소(Cancelled), 재시도(Retrying) 등의 상태를 거칠 수 있다. 각 상태 전환에는 타임스탬프와 관련 맥락을 기록해야 하며 현재 상태는 운용 모니터링에 사용하고 상태 전환 이력은 성능 분석, 사고 재구성, 서비스 수준 측정, 스케줄링 및 로봇 동작 디버깅에 활용할 수 있다.

시간 정보(Time Information)는 플릿 성능 분석에서 핵심적인 요소이다. 임무와 작업에는 requested_time, queued_time, assigned_time, start_time, completion_time, deadline, estimated_duration, actual_duration 등을 기록할 수 있다. 이를 이용하여 대기 지연, 할당 지연, 이동 시간, 실행 시간, 완료율, 기한 준수율 등의 지표를 계산할 수 있다. 또한 예상값(Estimated Value)과 실제 관측값(Actual Observation)을 구분하여 예측 정확도까지 평가할 수 있어야 한다.

공간 참조(Spatial Reference)는 작업 추적 정보를 실제 작업이 수행되는 환경과 연결한다. 출발지, 목적지, 웨이포인트, 구역, 지도, 층, 충전소, 엘리베이터, 도킹 위치는 작업 내부에 임의의 좌표값을 반복적으로 저장하는 대신 안정적인 공간 식별자(Spatial Identifier)를 사용해야 한다. 작업은 위치 객체(Location Object)를 참조하고 공간 데이터 모델은 내비게이션 시스템에 필요한 좌표, 지도 버전, 접근 제약, 의미 정보를 제공할 수 있다.

위치가 운용에 중요한 경우 로봇 위치(Robot Position)는 작업 실행과 연결되어야 한다. 플릿 시스템은 현재 자세(Current Pose)를 참조하거나 구역, 층, 웨이포인트 등의 요약 위치 정보를 유지하고 고주파 위치 데이터는 공간 또는 텔레메트리 도메인에서 관리할 수 있다. 이러한 분리는 플릿 데이터베이스가 지속적인 위치추정 업데이트로 과부하되는 것을 방지하면서도 현재 로봇 위치를 기반으로 스케줄링 결정을 수행할 수 있도록 한다.

에너지 상태(Energy State)는 기술적으로 작업 수행이 가능한 로봇이라도 임무를 안전하게 완료할 충분한 배터리 여유가 없을 수 있기 때문에 작업 할당에 영향을 준다. 플릿 레코드는 텔레메트리로부터 도출된 충전 상태(State of Charge), 예상 잔여 운용 시간, 충전 상태, 에너지 가용성을 제공할 수 있다. 스케줄링 정책은 작업 예상 에너지, 이동 거리, 충전 가능 지점, 최소 잔여 에너지 제약을 고려하여 작업을 할당하거나 로봇을 충전소로 이동시킬 수 있다.

여러 로봇이 인프라를 공유하는 환경에서는 자원 추적(Resource Tracking)이 필요하다. 충전소, 엘리베이터, 출입문, 적재 구역, 작업 셀, 좁은 통로, 도킹 지점 등을 안정적인 resource_id를 가진 공유 자원(Shared Resource)으로 표현할 수 있다. 예약 레코드(Reservation Record)는 특정 자원을 어떤 로봇 또는 작업이 어느 시간 구간 동안 사용하는지를 정의한다. 이를 통해 조정 시스템은 충돌을 방지하고 혼잡이나 자원 경합(Resource Contention)이 임무 성능에 미친 영향을 분석할 수 있다.

교통 조정(Traffic Coordination)은 경로 예약(Route Reservation), 차선 소유권, 교차로 접근 권한, 임시 공간 잠금(Spatial Lock) 등을 플릿 데이터 모델에 확장하여 표현할 수 있다. 모든 내비게이션 명령을 중앙에서 직접 표현하는 대신 플릿 계층에서는 여러 로봇에 영향을 주는 조정 객체를 관리할 수 있다. 예약 식별자, 로봇 참조, 공간 구간, 우선순위, 유효 시간, 해제 상태를 이용하면 특정 내비게이션 구현에 플릿 로직을 강하게 결합하지 않고도 공유 이동 공간을 관리할 수 있다.

고장과 재할당(Failure and Reassignment)은 예외적인 데이터베이스 상태가 아니라 정상적인 생명주기 조건으로 다루어야 한다. 작업은 로봇 고장, 차단된 경로, 에너지 부족, 통신 손실, 사용할 수 없는 인프라 또는 외부 취소로 실패할 수 있다. 실패 기록에는 원인, 진단 참조, 실행 맥락, 재시도 정책을 보존해야 한다. 작업이 다른 로봇으로 재할당되더라도 기존 실행 시도(Execution Attempt)는 새로운 할당으로 덮어쓰지 않고 이력에 유지해야 한다.

이기종 플릿(Heterogeneous Fleet)은 플릿 수준 의미 체계와 제조사별 로봇 인터페이스 사이에 추상화 경계(Abstraction Boundary)가 필요하다. 플릿 모델은 모든 로봇이 동일한 내부 구조를 제공하도록 강제하지 않고 로봇, 임무, 작업, 기능, 상태, 위치, 할당과 같은 공통 개념을 정의해야 한다. 어댑터(Adapter)는 독점 API, ROS 2 인터페이스 또는 상호운용성 표준(Interoperability Standard)을 공통 모델로 변환하면서 필요한 경우 제조사별 확장 정보를 보존할 수 있다.

플릿 데이터는 분산 시스템의 운용 기록을 연결하는 상관관계 식별자(Correlation Identifier)를 유지해야 한다. 하나의 임무는 창고, 병원, 제조 또는 기업 애플리케이션에서 생성되어 여러 작업, 명령, 이벤트, 로봇 행동으로 이어질 수 있다. 상관관계 식별자를 사용하면 이러한 기록을 하나의 비즈니스 트랜잭션(Business Transaction)으로 추적할 수 있다. 운영자는 외부 요청이 스케줄링, 로봇 실행, 완료, 후속 확인으로 이어지는 전체 과정을 연결하여 확인할 수 있다.

과거 플릿 데이터(Historical Fleet Data)는 현재 상태 모니터링을 넘어 운용 최적화를 지원한다. 구조화된 이력을 이용하여 임무 완료율, 작업 실행 시간, 로봇 활용률, 충전 빈도, 이동 거리, 유휴 시간, 고장률, 재할당 횟수, 자원 혼잡도를 계산할 수 있다. 이러한 지표를 분석하면 성능 문제가 개별 로봇, 스케줄링 정책, 시설 배치, 인프라 제약 또는 작업 부하 패턴 중 어디에서 발생하는지 파악할 수 있다.

표준화된 다중 로봇 플릿 데이터 모델(Standardized Multi-Robot Fleet Data Model)은 궁극적으로 이기종 로봇과 상위 애플리케이션 사이에서 공유되는 공통 운용 언어(Common Operational Language)를 구축한다. 안정적인 로봇, 임무, 작업, 할당, 위치, 자원 식별자는 모든 실행 단계를 추적 가능하게 만든다. 여기에 생명주기 상태, 타임스탬프, 기능, 가용성, 에너지, 진단, 공간 참조를 결합하면 신뢰성 있는 스케줄링, 작업 추적, 플릿 조정, 분석, 확장 가능한 자율 운용을 지원할 수 있다.

##  

## 01.09 Data Model Versioning and Schema Evolution Strategy

![](images/image9.png){width="7.268055555555556in" height="7.268055555555556in"}

Robot data models must evolve continuously because sensors, hardware, software, autonomy functions, fleet services, and business requirements change throughout the system lifecycle. A schema designed for the first robot prototype will rarely remain sufficient for production fleets. Schema evolution therefore requires an explicit architectural strategy that allows data structures to change without breaking deployed robots, historical datasets, analytics pipelines, APIs, or downstream applications.

Schema identity should be separated from schema version. A stable schema_id identifies the logical data contract, while schema_version identifies a specific definition of that contract. Telemetry, events, configurations, diagnostics, missions, and AI datasets can each maintain independent schema families. This separation allows systems to recognize that two records represent the same conceptual model even when their field structures differ across software generations.

Version information should travel with the data rather than being inferred only from deployment dates or software releases. Every serialized record, message envelope, file manifest, or dataset should contain or reference its applicable schema version. Explicit version metadata enables consumers to select the correct parser, validator, transformation rule, and compatibility policy even when old and new robot generations operate simultaneously within the same fleet.

Semantic versioning concepts can provide a useful foundation when adapted to data contracts. A major version indicates an incompatible structural or semantic change, a minor version introduces backward-compatible additions, and a patch version corrects definitions without changing the intended interface. Not every internal schema requires three numeric levels, but the organization should define consistent rules that explain what constitutes compatible and incompatible change.

Backward compatibility means a newer consumer can correctly interpret data produced by an older schema. Forward compatibility means an older consumer can tolerate data generated by a newer schema, usually by ignoring unknown optional fields. Full compatibility supports both directions within defined limits. Compatibility requirements should be selected according to operational needs rather than assuming that every schema must remain universally compatible forever.

Additive changes are generally the safest evolution mechanism. New optional fields, metadata, enumeration values, or extension objects can often be introduced without invalidating existing records. Required fields should be added cautiously because historical data and older producers cannot provide them. When new information is essential, defaults, derived values, migration rules, or a new major schema version may be necessary to preserve deterministic interpretation.

Removing or renaming fields is more disruptive because existing consumers may depend on them. A safer strategy is to mark fields as deprecated, introduce replacements, support both representations during a transition period, and remove the obsolete form only in a controlled major version. Deprecation metadata should identify the replacement field, deprecation version, expected retirement version, and migration guidance so developers can update integrations before compatibility is lost.

Changes in meaning can be more dangerous than structural changes. Keeping the same field name while changing its unit, coordinate frame, reference definition, sign convention, or semantic interpretation can silently corrupt downstream analysis. Such semantic changes should be treated as incompatible unless explicit conversion rules exist. Units, coordinate systems, enumerations, and reference conventions should therefore be part of the formal schema contract rather than undocumented assumptions.

Enumerations require special evolution rules because new states frequently appear as robot functionality expands. Consumers should avoid assuming that the known enumeration set is permanently complete. Unknown values should be handled safely, while safety-critical states may require stricter validation. Deprecated values should remain interpretable in historical records even after producers stop generating them, preserving the meaning of archived operational data.

A schema registry provides centralized governance for data contracts used across robot, edge, fleet, cloud, analytics, and AI systems. The registry can store schema identifiers, versions, definitions, ownership, compatibility mode, lifecycle status, documentation, and transformation references. Producers register approved schemas, while consumers can retrieve or validate the expected contract. This prevents independent teams from creating incompatible interpretations of the same logical data.

Schema validation should occur as early as practical in the data pipeline. Robot software can validate configuration or command structures before execution, while gateways and ingestion services can validate telemetry and events before persistent storage. Validation can check required fields, types, ranges, enumerations, units, identifier formats, and structural constraints. Invalid data should be rejected, quarantined, or explicitly marked rather than silently entering trusted datasets.

Schema evolution should distinguish the producer version from the consumer version. A robot may continue publishing an older schema while a fleet platform has already upgraded to a newer internal representation. Adapters or transformation services can translate between these contracts. Recording producer_schema_version and normalized_schema_version preserves lineage and helps engineers determine whether unexpected behavior originated in the source data or during transformation.

Canonical data models reduce the number of direct transformations required between heterogeneous systems. Vendor-specific, ROS 2, legacy, or site-specific formats can be mapped into a common canonical representation used by fleet and enterprise services. Evolution of the canonical model should remain controlled because changes can affect many adapters simultaneously. Vendor extensions can be retained in namespaced extension fields when information cannot be represented directly in the common schema.

Data migration converts previously stored records to a newer representation when required. Migration may occur eagerly by rewriting historical data or lazily when records are read. Large telemetry and AI datasets often make full rewriting expensive, so transformation-on-read or version-aware query layers may be preferable. Migration procedures should be deterministic, repeatable, auditable, and capable of identifying records that cannot be converted without information loss.

Historical data should remain interpretable according to the schema that existed when it was created. Replacing old definitions with current ones can destroy reproducibility and make previous operational events impossible to reconstruct accurately. Schema definitions should therefore be immutable after release. Corrections can create new versions, while old definitions remain available for decoding archived telemetry, events, diagnostics, configurations, and datasets.

Versioning must also preserve relationships between schemas. A diagnostic record may reference telemetry, a mission may reference spatial objects, and an AI dataset may reference sensor observations generated under another schema version. Cross-domain references should retain enough version information to reconstruct these relationships. Otherwise, individually valid records may become ambiguous when combined after several generations of schema evolution.

Distributed robot systems require rolling-upgrade compatibility because all components cannot normally be updated simultaneously. During deployment, old robots, new robots, edge services, fleet servers, and external applications may coexist. Compatibility windows should define which producer and consumer versions are supported together. Automated contract tests can verify these combinations before deployment and prevent a software release from unexpectedly breaking fleet communication.

Schema changes should pass through a controlled lifecycle such as draft, review, approved, active, deprecated, and retired. Ownership must be clear so that changes are evaluated for effects on robotics software, storage, APIs, analytics, AI pipelines, and external integrations. Change proposals should document motivation, compatibility impact, migration requirements, affected consumers, validation evidence, and rollback strategy before an updated contract becomes operational.

Observability is important during schema transitions. Systems should monitor validation failures, unknown fields, deprecated-field usage, transformation errors, incompatible messages, and producer-version distribution across the fleet. These metrics reveal whether migration is progressing as expected and identify consumers that still depend on obsolete structures. Operational dashboards can therefore turn schema evolution from a documentation activity into a measurable engineering process.

Rollback must be considered before deploying a new schema or transformation. If a software release is reverted, older components may again become active while newer records already exist in storage or message streams. Compatibility design should determine whether those components can tolerate the newer data. Where rollback compatibility cannot be guaranteed, migration checkpoints, dual writing, translation layers, or controlled deployment boundaries may be required.

Schema evolution is ultimately a governance problem as much as a serialization problem. Stable identifiers, explicit versions, compatibility policies, immutable historical definitions, registries, validation, adapters, migration procedures, and lifecycle controls allow robot data to evolve without losing meaning. When these mechanisms are applied consistently, telemetry, events, diagnostics, configuration, spatial data, fleet records, and AI datasets remain traceable across generations.

A mature versioning strategy enables long-lived robot platforms to change safely while preserving interoperability and analytical continuity. New robot models and software generations can introduce richer data without forcing immediate upgrades across the entire ecosystem. Historical information remains reproducible, current services remain compatible, and future systems can understand how each record was defined, transformed, and consumed throughout its complete data lifecycle.

로봇 데이터 모델(Robot Data Model)은 센서, 하드웨어, 소프트웨어, 자율주행 기능, 플릿 서비스, 비즈니스 요구사항이 시스템 생명주기 전체에 걸쳐 변화하기 때문에 지속적으로 진화해야 한다. 최초 로봇 프로토타입을 위해 설계된 스키마(Schema)가 실제 양산 플릿에서도 계속 충분한 경우는 드물다. 따라서 스키마 진화(Schema Evolution)는 배포된 로봇, 과거 데이터셋, 분석 파이프라인, API, 하위 애플리케이션을 중단시키지 않으면서 데이터 구조를 변경할 수 있는 명확한 아키텍처 전략을 필요로 한다.

스키마 식별자(Schema Identity)는 스키마 버전(Schema Version)과 분리해야 한다. 안정적인 schema_id는 논리적 데이터 계약(Logical Data Contract)을 식별하고 schema_version은 해당 계약의 특정 정의를 나타낸다. 텔레메트리, 이벤트, 구성, 진단, 임무, AI 데이터셋은 각각 독립적인 스키마 계열(Schema Family)을 유지할 수 있다. 이를 통해 필드 구조가 소프트웨어 세대에 따라 달라지더라도 두 레코드가 동일한 개념적 모델을 나타낸다는 것을 시스템이 인식할 수 있다.

버전 정보(Version Information)는 배포 날짜나 소프트웨어 릴리스만을 이용하여 추론하기보다 데이터 자체와 함께 전달되어야 한다. 직렬화된 레코드, 메시지 엔벌로프(Message Envelope), 파일 매니페스트(File Manifest), 데이터셋에는 적용되는 스키마 버전을 포함하거나 참조해야 한다. 명시적인 버전 메타데이터를 사용하면 구형 및 신형 로봇 세대가 동일한 플릿에서 동시에 운용되더라도 소비자가 올바른 파서(Parser), 검증기(Validator), 변환 규칙, 호환성 정책을 선택할 수 있다.

시맨틱 버전 관리(Semantic Versioning)의 개념은 데이터 계약에 맞게 적용하면 유용한 기반을 제공할 수 있다. 주 버전(Major Version)은 호환되지 않는 구조적 또는 의미적 변경을 나타내고, 부 버전(Minor Version)은 하위 호환성을 유지하는 기능 추가를 의미하며, 패치 버전(Patch Version)은 의도된 인터페이스를 변경하지 않는 정의 수정에 사용할 수 있다. 모든 내부 스키마에 반드시 세 단계 숫자 체계를 적용할 필요는 없지만 호환 가능한 변경과 호환되지 않는 변경을 구분하는 일관된 규칙은 정의해야 한다.

하위 호환성(Backward Compatibility)은 새로운 소비자(Consumer)가 이전 스키마에서 생성된 데이터를 올바르게 해석할 수 있음을 의미한다. 상위 호환성(Forward Compatibility)은 일반적으로 알 수 없는 선택적 필드를 무시함으로써 이전 소비자가 새로운 스키마로 생성된 데이터를 처리할 수 있음을 의미한다. 완전 호환성(Full Compatibility)은 정의된 범위 내에서 두 방향을 모두 지원한다. 모든 스키마가 영구적으로 완전 호환되어야 한다고 가정하기보다 실제 운용 요구사항에 따라 필요한 호환성 수준을 선택해야 한다.

추가형 변경(Additive Change)은 일반적으로 가장 안전한 스키마 진화 방법이다. 새로운 선택적 필드, 메타데이터, 열거형 값(Enumeration Value), 확장 객체(Extension Object)는 기존 레코드를 무효화하지 않고 추가할 수 있다. 반면 필수 필드(Required Field)는 과거 데이터와 구형 생산자(Producer)가 값을 제공할 수 없으므로 신중하게 추가해야 한다. 새로운 정보가 반드시 필요한 경우 기본값, 파생값, 마이그레이션 규칙 또는 새로운 주 스키마 버전이 필요할 수 있다.

필드 삭제 또는 이름 변경은 기존 소비자가 해당 필드에 의존할 수 있기 때문에 더 큰 영향을 미친다. 보다 안전한 전략은 기존 필드를 사용 중단 예정(Deprecated)으로 지정하고 대체 필드를 도입한 후 전환 기간 동안 두 표현을 모두 지원하며 통제된 주 버전에서만 기존 형식을 제거하는 것이다. 사용 중단 메타데이터에는 대체 필드, 사용 중단 버전, 예상 폐기 버전, 마이그레이션 지침을 기록하여 호환성이 사라지기 전에 개발자가 통합 시스템을 수정할 수 있도록 해야 한다.

의미 변경(Semantic Change)은 구조적 변경보다 더 위험할 수 있다. 동일한 필드 이름을 유지하면서 단위, 좌표 프레임, 참조 정의, 부호 규칙 또는 의미적 해석을 변경하면 하위 분석 시스템에서 오류를 감지하지 못한 채 데이터가 잘못 해석될 수 있다. 명확한 변환 규칙이 존재하지 않는다면 이러한 의미 변경은 비호환 변경으로 취급해야 한다. 따라서 단위, 좌표계, 열거형, 참조 규칙은 문서화되지 않은 가정이 아니라 공식적인 스키마 계약의 일부로 정의해야 한다.

열거형(Enumeration)은 로봇 기능이 확장되면서 새로운 상태가 지속적으로 추가되기 때문에 별도의 진화 규칙이 필요하다. 소비자는 현재 알려진 열거형 집합이 영구적으로 완전하다고 가정해서는 안 된다. 알 수 없는 값(Unknown Value)을 안전하게 처리할 수 있어야 하며 안전 중요 상태(Safety-Critical State)는 더 엄격한 검증이 필요할 수 있다. 사용이 중단된 값도 생산자가 더 이상 생성하지 않더라도 과거 레코드의 의미를 보존하기 위해 계속 해석할 수 있어야 한다.

스키마 레지스트리(Schema Registry)는 로봇, 엣지, 플릿, 클라우드, 분석, AI 시스템에서 사용하는 데이터 계약을 중앙에서 관리하기 위한 거버넌스 체계를 제공한다. 레지스트리에는 스키마 식별자, 버전, 정의, 소유권, 호환성 모드, 생명주기 상태, 문서, 변환 참조 등을 저장할 수 있다. 생산자는 승인된 스키마를 등록하고 소비자는 예상되는 계약을 조회하거나 검증할 수 있다. 이를 통해 독립적인 팀이 동일한 논리적 데이터에 대해 서로 호환되지 않는 해석을 만드는 문제를 방지할 수 있다.

스키마 검증(Schema Validation)은 데이터 파이프라인에서 가능한 한 이른 단계에 수행해야 한다. 로봇 소프트웨어는 실행 전에 구성 또는 명령 구조를 검증할 수 있으며 게이트웨이와 수집 서비스(Ingestion Service)는 영구 저장 전에 텔레메트리와 이벤트를 검증할 수 있다. 검증에서는 필수 필드, 데이터 유형, 범위, 열거형, 단위, 식별자 형식, 구조적 제약을 확인할 수 있다. 유효하지 않은 데이터는 신뢰할 수 있는 데이터셋에 조용히 포함시키지 않고 거부, 격리 또는 명확한 오류 표시를 수행해야 한다.

스키마 진화에서는 생산자 버전(Producer Version)과 소비자 버전(Consumer Version)을 구분해야 한다. 로봇이 이전 스키마를 계속 발행하는 동안 플릿 플랫폼은 이미 새로운 내부 표현으로 업그레이드될 수 있다. 어댑터(Adapter) 또는 변환 서비스(Transformation Service)를 이용하여 이러한 계약 사이를 변환할 수 있다. producer_schema_version과 normalized_schema_version을 기록하면 데이터 계보(Data Lineage)를 보존하고 예상하지 못한 동작이 원본 데이터에서 발생했는지 변환 과정에서 발생했는지 분석할 수 있다.

표준 데이터 모델(Canonical Data Model)은 이기종 시스템 사이에 필요한 직접 변환의 수를 줄여준다. 제조사 전용, ROS 2, 레거시 또는 사이트별 형식을 플릿 및 엔터프라이즈 서비스에서 사용하는 공통 표준 표현으로 매핑할 수 있다. 표준 모델의 변경은 여러 어댑터에 동시에 영향을 줄 수 있으므로 엄격하게 통제해야 한다. 공통 스키마에서 직접 표현할 수 없는 정보는 네임스페이스가 적용된 확장 필드(Namespaced Extension Field)에 제조사별 정보로 보존할 수 있다.

데이터 마이그레이션(Data Migration)은 필요한 경우 이전에 저장된 레코드를 새로운 표현으로 변환한다. 마이그레이션은 과거 데이터를 다시 작성하는 즉시 변환(Eager Migration) 방식이나 레코드를 읽을 때 변환하는 지연 변환(Lazy Migration) 방식으로 수행할 수 있다. 대규모 텔레메트리와 AI 데이터셋은 전체 재작성 비용이 매우 높을 수 있으므로 읽기 시 변환(Transformation-on-Read) 또는 버전 인식 쿼리 계층(Version-Aware Query Layer)이 더 적합할 수 있다. 마이그레이션 절차는 결정적이고 반복 가능하며 감사 가능해야 한다.

과거 데이터(Historical Data)는 생성 당시 존재했던 스키마에 따라 계속 해석할 수 있어야 한다. 이전 정의를 현재 정의로 덮어쓰면 재현성(Reproducibility)이 손상되고 과거 운용 이벤트를 정확하게 재구성하지 못할 수 있다. 따라서 공식 릴리스 이후의 스키마 정의는 불변(Immutable)으로 유지해야 한다. 수정이 필요한 경우 새로운 버전을 생성하고 기존 정의는 보관된 텔레메트리, 이벤트, 진단, 구성, 데이터셋을 해석할 수 있도록 계속 유지해야 한다.

버전 관리는 스키마 사이의 관계도 보존해야 한다. 진단 레코드는 텔레메트리를 참조할 수 있고 임무는 공간 객체를 참조하며 AI 데이터셋은 다른 스키마 버전에서 생성된 센서 관측 데이터를 참조할 수 있다. 도메인 간 참조(Cross-Domain Reference)는 이러한 관계를 재구성하는 데 필요한 충분한 버전 정보를 유지해야 한다. 그렇지 않으면 개별적으로는 유효한 레코드라도 여러 세대의 스키마 진화를 거쳐 결합할 때 의미가 모호해질 수 있다.

분산 로봇 시스템(Distributed Robot System)에서는 모든 구성요소를 동시에 업데이트하기 어려우므로 순차 업그레이드 호환성(Rolling-Upgrade Compatibility)이 필요하다. 배포 과정에서 구형 로봇, 신형 로봇, 엣지 서비스, 플릿 서버, 외부 애플리케이션이 동시에 존재할 수 있다. 호환성 구간(Compatibility Window)을 정의하여 어떤 생산자와 소비자 버전 조합을 지원하는지 명시해야 한다. 자동화된 계약 테스트(Contract Test)를 통해 배포 전에 이러한 조합을 검증하면 소프트웨어 릴리스가 플릿 통신을 예기치 않게 중단시키는 것을 방지할 수 있다.

스키마 변경은 초안(Draft), 검토(Review), 승인(Approved), 활성(Active), 사용 중단 예정(Deprecated), 폐기(Retired)와 같은 통제된 생명주기를 거쳐야 한다. 변경이 로봇 소프트웨어, 저장소, API, 분석, AI 파이프라인, 외부 통합 시스템에 미치는 영향을 평가할 수 있도록 명확한 소유권이 필요하다. 변경 제안에는 목적, 호환성 영향, 마이그레이션 요구사항, 영향을 받는 소비자, 검증 근거, 롤백 전략을 문서화한 후 새로운 계약을 실제 운용에 적용해야 한다.

스키마 전환 과정에서는 관측 가능성(Observability)이 중요하다. 시스템은 검증 실패, 알 수 없는 필드, 사용 중단 필드의 사용 현황, 변환 오류, 호환되지 않는 메시지, 플릿 전체의 생산자 버전 분포를 모니터링해야 한다. 이러한 지표는 마이그레이션이 예상대로 진행되는지 보여주고 여전히 오래된 구조에 의존하는 소비자를 식별할 수 있게 한다. 운용 대시보드를 활용하면 스키마 진화를 단순한 문서화 작업이 아니라 측정 가능한 엔지니어링 프로세스로 관리할 수 있다.

새로운 스키마 또는 변환 로직을 배포하기 전에 롤백(Rollback)을 고려해야 한다. 소프트웨어 릴리스가 이전 버전으로 복원되면 구형 구성요소가 다시 활성화될 수 있지만 저장소나 메시지 스트림에는 이미 새로운 형식의 레코드가 존재할 수 있다. 호환성 설계에서는 이러한 구형 구성요소가 새로운 데이터를 처리할 수 있는지를 판단해야 한다. 롤백 호환성을 보장할 수 없다면 마이그레이션 체크포인트, 이중 기록(Dual Writing), 변환 계층 또는 통제된 배포 경계가 필요할 수 있다.

스키마 진화는 직렬화(Serialization)의 문제인 동시에 궁극적으로 거버넌스(Governance)의 문제이다. 안정적인 식별자, 명시적인 버전, 호환성 정책, 불변의 과거 정의, 레지스트리, 검증, 어댑터, 마이그레이션 절차, 생명주기 제어를 적용하면 데이터의 의미를 잃지 않으면서 로봇 데이터를 발전시킬 수 있다. 이러한 메커니즘을 일관되게 적용하면 텔레메트리, 이벤트, 진단, 구성, 공간 데이터, 플릿 레코드, AI 데이터셋을 여러 세대에 걸쳐 추적할 수 있다.

성숙한 버전 관리 전략(Mature Versioning Strategy)은 장기간 운용되는 로봇 플랫폼이 상호운용성과 분석의 연속성을 유지하면서 안전하게 변화할 수 있도록 한다. 새로운 로봇 모델과 소프트웨어 세대는 전체 생태계에 즉각적인 업그레이드를 강제하지 않고도 더 풍부한 데이터를 도입할 수 있다. 과거 정보의 재현성을 유지하고 현재 서비스의 호환성을 보장하며 미래 시스템은 각각의 레코드가 전체 데이터 생명주기 동안 어떻게 정의되고 변환되며 소비되었는지를 이해할 수 있다.

##  

## 01.10 Robot Data Standardization: ROS2 to Business Model Mapping

![](images/image10.png){width="7.268055555555556in" height="7.268055555555556in"}

Robot data standardization must bridge two fundamentally different perspectives. ROS 2 represents robots through topics, messages, services, actions, parameters, nodes, and coordinate frames optimized for distributed robotic computation. Business systems instead require stable concepts such as robot, asset, mission, task, location, customer, service status, utilization, and maintenance. A mapping architecture connects these layers without forcing enterprise applications to understand robot middleware details.

Directly exposing ROS 2 interfaces to business applications creates unnecessary coupling. Topic names, message structures, namespaces, QoS settings, and node implementations may change as robot software evolves, while enterprise applications expect stable contracts. A standardization layer should therefore translate operational ROS 2 data into canonical business objects and events whose meaning remains stable even when underlying robot implementations or vendors change.

Robot identity provides the first mapping boundary. ROS 2 namespaces and node names can identify runtime software instances, but they should not become permanent enterprise identifiers. A persistent robot_id should represent the physical or logical fleet asset, while namespace, domain ID, node identity, hardware serial number, and network information remain technical attributes. This distinction allows software instances to restart or change without breaking maintenance, mission, or asset histories.

ROS 2 topics generally represent continuous state or asynchronous observations and can be mapped into telemetry, state, or event domains according to their semantics. Sensor messages such as images, laser scans, IMU measurements, battery states, and odometry belong primarily to operational data streams. Business systems rarely require every raw message, so gateways can extract summaries, health indicators, utilization metrics, exceptions, and references to retained raw data.

ROS 2 services represent request-response interactions and should be mapped carefully because enterprise operations usually require stronger transaction semantics. A service call may become a business command such as reset subsystem, request docking, change operating mode, or retrieve configuration. The standardized model should record command identity, requester, target robot, parameters, request time, result, status, and correlation identifiers rather than exposing only middleware-level request and response messages.

ROS 2 actions naturally align with long-running robot operations because they support goals, feedback, cancellation, and results. Navigation, docking, manipulation, inspection, and other actions can therefore map to task execution records. The business model should preserve task_id, robot_id, requested goal, lifecycle state, progress, timestamps, completion result, and failure reason while hiding implementation-specific action server names and message definitions.

Parameters and configuration interfaces can map into the standardized robot configuration model. ROS 2 parameters may describe controller gains, sensor settings, thresholds, frame names, planner options, or runtime behavior, but not every parameter should be exposed as a business attribute. The mapping layer should distinguish engineering configuration from operationally relevant settings and preserve parameter provenance, configuration version, validation status, and applied robot identity.

Coordinate frames and transforms require semantic mapping before business systems can use spatial information. ROS 2 may represent relationships among map, odom, base_link, sensors, and component frames through TF, while enterprise applications typically reason about sites, buildings, floors, zones, stations, routes, and named locations. A spatial standardization layer should associate technical coordinates with stable semantic location identifiers and map versions.

Mission and task models provide a major connection between enterprise intent and ROS 2 execution. A warehouse, hospital, factory, or facility system may request a business objective such as deliver item, inspect zone, or move material. The fleet platform decomposes this objective into tasks, and robot adapters translate those tasks into ROS 2 actions, services, or commands. Execution feedback is then normalized back into common mission and task lifecycle states.

This mapping should be bidirectional but not symmetrical. Downstream commands move from business intent through fleet orchestration and adapters toward ROS 2 interfaces, while upstream information moves from robot messages toward normalized telemetry, events, status, and business outcomes. Each direction requires different validation, authorization, timing, reliability, and safety controls. Treating the mapping as simple field conversion is therefore insufficient for production systems.

A canonical robot data model provides the stable intermediate representation between heterogeneous robot interfaces and business applications. Common entities may include Robot, Mission, Task, Assignment, Location, Resource, Diagnostic, Configuration, TelemetryMetric, Event, and MaintenanceRecord. Vendor-specific ROS 2 messages are transformed into these objects, allowing fleet, analytics, digital twin, ERP, WMS, MES, and service systems to consume a consistent data contract.

Not all ROS 2 data should be promoted into the canonical business model. High-frequency camera images, point clouds, control loops, transforms, and actuator-level signals may remain within the robot or specialized data platforms. The mapping architecture should classify information according to operational and business value. Raw data can be stored by reference, while normalized summaries, exceptions, KPIs, state transitions, and traceability metadata are exposed to higher layers.

Event normalization is particularly important because robot software may produce many implementation-specific status messages. These should be mapped into controlled domain events such as RobotAvailable, MissionStarted, TaskCompleted, ChargingStarted, LocalizationDegraded, SafetyStopActivated, or MaintenanceRequired. Standard event envelopes can include event_id, event_type, robot_id, timestamp, severity, correlation_id, source, schema_version, and context for consistent downstream processing.

Diagnostics require a similar translation boundary. ROS 2 diagnostic messages and component-specific errors can be mapped into the standardized DTC and diagnostic occurrence model. The mapping should retain the original technical source while adding canonical subsystem, component, severity, lifecycle state, and maintenance meaning. This enables business systems to reason about service impact without losing the engineering information required for troubleshooting.

Quality of Service, or QoS, is essential inside ROS 2 but should not leak directly into business semantics. Reliability, durability, history depth, and delivery behavior influence how robot messages are transported, while enterprise systems may use queues, event brokers, databases, or APIs with different guarantees. Gateways must translate communication behavior into appropriate delivery, persistence, retry, deduplication, and ordering policies for the standardized data layer.

Correlation identifiers provide traceability across these architectural boundaries. A business order may generate a mission, multiple tasks, ROS 2 actions, navigation goals, events, diagnostic records, and completion confirmations. Maintaining order_id, mission_id, task_id, command_id, action references, and correlation_id relationships allows operators to reconstruct the entire transaction from enterprise request to physical robot behavior and final business result.

Time semantics also require normalization. ROS 2 timestamps may originate from sensor clocks, system clocks, simulation time, or synchronized robot clocks, while business systems require consistent event and transaction timelines. The standardized envelope should distinguish source_time, robot_time, ingestion_time, and processing_time when necessary. Clock synchronization quality and timestamp uncertainty may also be retained for applications requiring precise reconstruction.

Schema translation should be version-aware because ROS 2 message definitions and canonical business schemas evolve independently. An adapter should record source interface version, mapping version, and target schema version so transformations remain reproducible. When a robot software update modifies a message, only the adapter may need to change while the business contract remains stable. This separation significantly reduces integration impact across large fleets.

Security and governance must be enforced at the mapping boundary. Enterprise systems should not automatically gain unrestricted access to ROS 2 topics, services, or actions simply because data integration exists. The gateway can apply authentication, authorization, command allowlists, validation, rate limits, audit logging, and safety policies. Sensitive engineering data can remain restricted while approved operational information is exposed through governed APIs and events.

The standardized layer also enables heterogeneous fleet interoperability. Robots from different vendors may use different ROS 2 distributions, namespaces, message packages, proprietary APIs, or non-ROS interfaces. Vendor adapters can translate each implementation into the same canonical robot and task model. Higher-level applications then operate on capabilities and business semantics rather than depending on the middleware technology used by each robot.

Mapping should preserve lineage from normalized business data back to its technical source. A KPI, fault, task result, or utilization metric should identify the robot data, transformation logic, schema version, and processing stage from which it was derived. This lineage supports debugging, audits, analytics validation, digital twins, AI pipelines, and regulatory evidence by making derived business information traceable to physical robot observations and actions.

A mature ROS 2-to-business mapping architecture therefore acts as an anti-corruption layer between rapidly evolving robotic software and long-lived enterprise systems. ROS 2 remains optimized for sensing, control, navigation, and distributed robot execution, while canonical models provide stable identities, tasks, events, diagnostics, locations, and business states. Adapters absorb implementation differences and protect both sides from unnecessary structural coupling.

The result is a standardized information flow from physical robots through ROS 2, edge gateways, fleet platforms, and enterprise applications. Commands can move safely from business objectives toward robot execution, while telemetry and events return as meaningful operational outcomes. This architecture enables scalable fleet integration, vendor independence, traceability, analytics, digital twins, maintenance, and continuous business optimization without sacrificing robotic engineering detail.

로봇 데이터 표준화(Robot Data Standardization)는 근본적으로 서로 다른 두 가지 관점을 연결해야 한다. ROS 2는 분산 로봇 연산에 최적화된 토픽(Topic), 메시지(Message), 서비스(Service), 액션(Action), 파라미터(Parameter), 노드(Node), 좌표 프레임(Coordinate Frame)을 통해 로봇을 표현한다. 반면 비즈니스 시스템은 로봇, 자산, 임무, 작업, 위치, 고객, 서비스 상태, 활용률, 유지보수와 같은 안정적인 개념을 필요로 한다. 매핑 아키텍처(Mapping Architecture)는 기업 애플리케이션이 로봇 미들웨어의 세부사항을 이해하도록 강제하지 않으면서 이 두 계층을 연결한다.

ROS 2 인터페이스를 비즈니스 애플리케이션에 직접 노출하면 불필요한 결합(Coupling)이 발생한다. 토픽 이름, 메시지 구조, 네임스페이스(Namespace), 서비스 품질(QoS) 설정, 노드 구현은 로봇 소프트웨어가 발전하면서 변경될 수 있지만 기업 애플리케이션은 안정적인 계약을 요구한다. 따라서 표준화 계층(Standardization Layer)은 운용 ROS 2 데이터를 기반 구현이나 로봇 제조사가 변경되더라도 의미가 안정적으로 유지되는 표준 비즈니스 객체(Canonical Business Object)와 이벤트로 변환해야 한다.

로봇 식별자(Robot Identity)는 첫 번째 매핑 경계를 제공한다. ROS 2 네임스페이스와 노드 이름은 런타임 소프트웨어 인스턴스를 식별할 수 있지만 영구적인 기업 식별자로 사용해서는 안 된다. 지속적인 robot_id를 물리적 또는 논리적 플릿 자산(Fleet Asset)의 식별자로 사용하고 네임스페이스, 도메인 ID, 노드 식별자, 하드웨어 일련번호, 네트워크 정보는 기술적 속성으로 관리해야 한다. 이러한 구분을 통해 소프트웨어 인스턴스가 재시작되거나 변경되어도 유지보수, 임무, 자산 이력이 단절되지 않는다.

ROS 2 토픽은 일반적으로 연속적인 상태 또는 비동기 관측(Asynchronous Observation)을 나타내며 의미에 따라 텔레메트리, 상태 또는 이벤트 도메인으로 매핑할 수 있다. 이미지, 레이저 스캔, IMU 측정값, 배터리 상태, 오도메트리(Odometry) 등의 센서 메시지는 주로 운용 데이터 스트림에 해당한다. 비즈니스 시스템은 모든 원시 메시지를 필요로 하지 않으므로 게이트웨이(Gateway)는 요약 정보, 상태 지표, 활용률 지표, 예외 정보와 보존된 원시 데이터에 대한 참조를 추출할 수 있다.

ROS 2 서비스(Service)는 요청-응답(Request-Response) 상호작용을 나타내며 기업 운용에서는 일반적으로 더 강력한 트랜잭션 의미 체계가 필요하므로 신중하게 매핑해야 한다. 하나의 서비스 호출은 서브시스템 재설정, 도킹 요청, 운용 모드 변경, 구성 정보 조회와 같은 비즈니스 명령(Business Command)으로 변환될 수 있다. 표준화 모델은 단순한 미들웨어 수준 요청과 응답만 노출하는 대신 명령 식별자, 요청자, 대상 로봇, 파라미터, 요청 시간, 결과, 상태, 상관관계 식별자를 기록해야 한다.

ROS 2 액션(Action)은 목표(Goal), 피드백(Feedback), 취소(Cancellation), 결과(Result)를 지원하기 때문에 장시간 실행되는 로봇 작업과 자연스럽게 연결된다. 내비게이션, 도킹, 조작, 검사 등의 액션은 작업 실행 기록(Task Execution Record)으로 매핑할 수 있다. 비즈니스 모델은 구현에 종속적인 액션 서버 이름과 메시지 정의를 숨기면서 task_id, robot_id, 요청 목표, 생명주기 상태, 진행률, 타임스탬프, 완료 결과, 실패 원인을 보존해야 한다.

파라미터 및 구성 인터페이스(Parameter and Configuration Interface)는 표준화된 로봇 구성 모델로 매핑할 수 있다. ROS 2 파라미터는 제어기 게인, 센서 설정, 임계값, 프레임 이름, 플래너 옵션, 런타임 동작 등을 나타낼 수 있지만 모든 파라미터를 비즈니스 속성으로 노출할 필요는 없다. 매핑 계층은 엔지니어링 구성(Engineering Configuration)과 운용상 중요한 설정을 구분하고 파라미터 출처, 구성 버전, 검증 상태, 적용된 로봇 식별자를 보존해야 한다.

좌표 프레임과 변환(Coordinate Frame and Transform)은 비즈니스 시스템에서 공간 정보를 활용하기 전에 의미적 매핑(Semantic Mapping)이 필요하다. ROS 2는 TF를 통해 map, odom, base_link, 센서 및 구성요소 프레임 사이의 관계를 표현할 수 있지만 기업 애플리케이션은 일반적으로 사이트, 건물, 층, 구역, 스테이션, 경로, 명명된 위치를 기준으로 판단한다. 공간 표준화 계층(Spatial Standardization Layer)은 기술적 좌표를 안정적인 의미적 위치 식별자 및 지도 버전과 연결해야 한다.

임무 및 작업 모델(Mission and Task Model)은 기업의 의도와 ROS 2 실행 사이의 핵심 연결점을 제공한다. 창고, 병원, 공장 또는 시설 시스템은 물품 배송, 구역 검사, 자재 이동과 같은 비즈니스 목적을 요청할 수 있다. 플릿 플랫폼은 이러한 목적을 여러 작업으로 분해하고 로봇 어댑터(Robot Adapter)는 해당 작업을 ROS 2 액션, 서비스 또는 명령으로 변환한다. 이후 실행 피드백은 다시 공통 임무 및 작업 생명주기 상태로 정규화(Normalization)된다.

이러한 매핑은 양방향(Bidirectional)이어야 하지만 대칭적(Symmetrical)이지는 않다. 하향 명령(Downstream Command)은 비즈니스 의도에서 플릿 오케스트레이션(Fleet Orchestration)과 어댑터를 거쳐 ROS 2 인터페이스 방향으로 전달되며, 상향 정보(Upstream Information)는 로봇 메시지에서 정규화된 텔레메트리, 이벤트, 상태, 비즈니스 결과로 전달된다. 각 방향에는 서로 다른 검증, 권한 부여, 타이밍, 신뢰성, 안전 제어가 필요하므로 단순한 필드 변환만으로 매핑을 구현해서는 안 된다.

표준 로봇 데이터 모델(Canonical Robot Data Model)은 이기종 로봇 인터페이스와 비즈니스 애플리케이션 사이에서 안정적인 중간 표현을 제공한다. 공통 엔티티에는 로봇(Robot), 임무(Mission), 작업(Task), 할당(Assignment), 위치(Location), 자원(Resource), 진단(Diagnostic), 구성(Configuration), 텔레메트리 지표(TelemetryMetric), 이벤트(Event), 유지보수 기록(MaintenanceRecord) 등이 포함될 수 있다. 제조사별 ROS 2 메시지를 이러한 객체로 변환하면 플릿, 분석, 디지털 트윈, ERP, WMS, MES, 서비스 시스템이 일관된 데이터 계약을 사용할 수 있다.

모든 ROS 2 데이터를 표준 비즈니스 모델로 승격시킬 필요는 없다. 고주파 카메라 영상, 포인트 클라우드, 제어 루프, 좌표 변환, 액추에이터 수준 신호는 로봇 내부 또는 전문 데이터 플랫폼에 유지할 수 있다. 매핑 아키텍처는 정보를 운용 가치와 비즈니스 가치에 따라 분류해야 한다. 원시 데이터는 참조 형태로 저장하고 정규화된 요약 정보, 예외, 핵심성과지표(KPI), 상태 전환, 추적성 메타데이터를 상위 계층에 제공할 수 있다.

이벤트 정규화(Event Normalization)는 로봇 소프트웨어가 다양한 구현 종속적 상태 메시지를 생성할 수 있기 때문에 특히 중요하다. 이러한 메시지는 RobotAvailable, MissionStarted, TaskCompleted, ChargingStarted, LocalizationDegraded, SafetyStopActivated, MaintenanceRequired와 같은 통제된 도메인 이벤트(Domain Event)로 매핑할 수 있다. 표준 이벤트 엔벌로프에는 event_id, event_type, robot_id, timestamp, severity, correlation_id, source, schema_version, context 등을 포함하여 하위 시스템에서 일관되게 처리할 수 있도록 해야 한다.

진단(Diagnostics)에도 이와 유사한 변환 경계가 필요하다. ROS 2 진단 메시지와 구성요소별 오류는 표준화된 진단 고장 코드(Diagnostic Trouble Code, DTC) 및 진단 발생 모델(Diagnostic Occurrence Model)로 매핑할 수 있다. 매핑 과정에서는 원래의 기술적 출처를 보존하면서 표준 서브시스템, 구성요소, 심각도, 생명주기 상태, 유지보수 의미를 추가해야 한다. 이를 통해 비즈니스 시스템은 문제 해결에 필요한 엔지니어링 정보를 잃지 않으면서 서비스에 미치는 영향을 판단할 수 있다.

서비스 품질(Quality of Service, QoS)은 ROS 2 내부에서는 핵심 요소이지만 비즈니스 의미 체계에 직접 노출해서는 안 된다. 신뢰성(Reliability), 지속성(Durability), 히스토리 깊이(History Depth), 전달 방식은 로봇 메시지 전송에 영향을 주지만 기업 시스템에서는 서로 다른 보장 특성을 가진 큐, 이벤트 브로커, 데이터베이스, API를 사용할 수 있다. 게이트웨이는 이러한 통신 특성을 표준 데이터 계층에 적합한 전달, 영속화, 재시도, 중복 제거, 순서 보장 정책으로 변환해야 한다.

상관관계 식별자(Correlation Identifier)는 이러한 아키텍처 경계를 넘어서 전체 추적성을 제공한다. 하나의 비즈니스 주문은 임무, 여러 작업, ROS 2 액션, 내비게이션 목표, 이벤트, 진단 기록, 완료 확인으로 이어질 수 있다. order_id, mission_id, task_id, command_id, 액션 참조, correlation_id 사이의 관계를 유지하면 운영자는 기업 요청에서 실제 로봇 동작과 최종 비즈니스 결과까지 전체 트랜잭션을 재구성할 수 있다.

시간 의미 체계(Time Semantics)도 정규화가 필요하다. ROS 2 타임스탬프는 센서 클록, 시스템 클록, 시뮬레이션 시간 또는 동기화된 로봇 클록에서 생성될 수 있지만 비즈니스 시스템은 일관된 이벤트 및 트랜잭션 시간축을 필요로 한다. 표준 엔벌로프는 필요한 경우 source_time, robot_time, ingestion_time, processing_time을 구분해야 한다. 정밀한 사건 재구성이 필요한 애플리케이션을 위해 클록 동기화 품질과 타임스탬프 불확실성도 함께 보존할 수 있다.

ROS 2 메시지 정의와 표준 비즈니스 스키마는 서로 독립적으로 발전하기 때문에 스키마 변환(Schema Translation)은 버전을 인식해야 한다. 어댑터는 source_interface_version, mapping_version, target_schema_version을 기록하여 변환 과정을 재현할 수 있도록 해야 한다. 로봇 소프트웨어 업데이트로 메시지가 변경되더라도 어댑터만 수정하고 비즈니스 계약은 안정적으로 유지할 수 있다. 이러한 분리는 대규모 플릿에서 통합 변경의 영향을 크게 줄인다.

보안 및 거버넌스(Security and Governance)는 매핑 경계에서 강제되어야 한다. 데이터 통합이 존재한다는 이유만으로 기업 시스템이 ROS 2 토픽, 서비스, 액션에 제한 없이 접근해서는 안 된다. 게이트웨이는 인증(Authentication), 권한 부여(Authorization), 명령 허용 목록(Command Allowlist), 검증, 호출 속도 제한(Rate Limit), 감사 로그(Audit Logging), 안전 정책을 적용할 수 있다. 민감한 엔지니어링 데이터는 제한하면서 승인된 운용 정보만 통제된 API와 이벤트를 통해 제공할 수 있다.

표준화 계층은 이기종 플릿 상호운용성(Heterogeneous Fleet Interoperability)도 가능하게 한다. 서로 다른 제조사의 로봇은 서로 다른 ROS 2 배포판, 네임스페이스, 메시지 패키지, 독점 API 또는 비 ROS 인터페이스를 사용할 수 있다. 제조사 어댑터(Vendor Adapter)는 각각의 구현을 동일한 표준 로봇 및 작업 모델로 변환할 수 있다. 상위 애플리케이션은 개별 로봇이 사용하는 미들웨어 기술에 의존하지 않고 기능(Capability)과 비즈니스 의미를 기준으로 동작할 수 있다.

매핑 과정에서는 정규화된 비즈니스 데이터에서 원래의 기술적 소스까지 이어지는 데이터 계보(Data Lineage)를 보존해야 한다. KPI, 고장, 작업 결과, 활용률 지표는 어떤 로봇 데이터, 변환 로직, 스키마 버전, 처리 단계로부터 생성되었는지를 식별할 수 있어야 한다. 이러한 계보는 파생된 비즈니스 정보를 실제 로봇 관측 및 행동까지 추적할 수 있게 하여 디버깅, 감사, 분석 검증, 디지털 트윈, AI 파이프라인, 규제 대응 근거를 지원한다.

성숙한 ROS 2-비즈니스 매핑 아키텍처(ROS 2-to-Business Mapping Architecture)는 빠르게 변화하는 로봇 소프트웨어와 장기간 유지되는 기업 시스템 사이에서 오염 방지 계층(Anti-Corruption Layer)의 역할을 수행한다. ROS 2는 센싱, 제어, 내비게이션, 분산 로봇 실행에 최적화된 구조를 유지하고 표준 모델은 안정적인 식별자, 작업, 이벤트, 진단, 위치, 비즈니스 상태를 제공한다. 어댑터는 구현 차이를 흡수하여 양쪽 시스템을 불필요한 구조적 결합으로부터 보호한다.

최종적으로 로봇 데이터 표준화는 물리적 로봇에서 ROS 2, 엣지 게이트웨이, 플릿 플랫폼, 기업 애플리케이션으로 이어지는 표준화된 정보 흐름을 구축한다. 명령은 비즈니스 목표에서 실제 로봇 실행 방향으로 안전하게 전달되고 텔레메트리와 이벤트는 의미 있는 운용 결과로 다시 반환된다. 이러한 아키텍처는 로봇 엔지니어링의 세부 정보를 보존하면서도 확장 가능한 플릿 통합, 제조사 독립성, 추적성, 분석, 디지털 트윈, 유지보수, 지속적인 비즈니스 최적화를 가능하게 한다.
