**Volume 07 Robot Data Architecture**

# 06. Sensor Data Management

## 06.01 Sensor Data Characteristics: Image, PointCloud, IMU

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

로봇 센서 데이터(Robot Sensor Data)는 물리적 세계(Physical World)에 대한 연속적인 관측을 표현한다는 점에서 일반적인 비즈니스 데이터(Business Data)나 애플리케이션 데이터(Application Data)와 근본적으로 다르다. 카메라(Camera), 라이다(LiDAR), 깊이 센서(Depth Sensor), 관성 측정 장치(IMU)는 서로 다른 속도, 차원, 좌표계(Coordinate System), 불확실성 수준으로 데이터를 생성한다. 따라서 이러한 이기종 데이터 스트림(Heterogeneous Data Stream)을 관리하려면 데이터 형식뿐 아니라 로봇 데이터 아키텍처(Robot Data Architecture) 내에서 시간적·공간적·의미적 관계를 함께 이해해야 한다.

이미지 데이터(Image Data)는 일반적으로 RGB, 흑백(Monochrome), 스테레오(Stereo), 깊이(Depth), 열화상(Thermal) 또는 특수 카메라(Specialized Camera)에서 획득되는 2차원 픽셀 배열(Two-Dimensional Pixel Array)로 표현된다. 각 프레임(Frame)은 해상도(Resolution), 픽셀 형식(Pixel Format), 노출(Exposure), 초점 거리(Focal Length), 왜곡 파라미터(Distortion Parameter), 카메라 보정(Camera Calibration)에 따라 해석되는 공간 정보를 포함한다. 이미지 한 장에는 수백만 개의 값이 포함될 수 있으므로 카메라 스트림(Camera Stream)은 로봇 탑재 데이터(Onboard Data) 용량을 크게 증가시키는 주요 데이터 소스이다.

이미지 스트림(Image Stream)의 특성은 프레임률(Frame Rate)과 해상도(Resolution)에 크게 영향을 받는다. 초당 30프레임(30 FPS) 카메라는 매초 30개의 관측 데이터를 연속적으로 생성하며, 여러 카메라를 사용하면 다수의 동기화된 스트림(Synchronized Stream)이 동시에 생성된다. 원시 이미지(Raw Image)는 최대한의 정보를 보존하지만 저장 공간(Storage)과 네트워크 대역폭(Network Bandwidth)을 많이 사용한다. 인코딩 형식(Encoded Format)은 이를 줄일 수 있지만 압축(Compression) 과정에서 지연 시간(Latency), 계산 비용(Computational Cost), 화질(Image Quality), 향후 인공지능 학습(AI Training)이나 정밀 분석에 대한 적합성을 함께 고려해야 한다.

카메라 데이터(Camera Data)는 중요한 기하학적 의미(Geometric Meaning)도 가진다. 하나의 픽셀(Pixel)은 환경에서 직접적인 3차원 위치를 나타내는 것이 아니라 이미지 평면(Image Plane)의 위치를 나타낸다. 카메라 내부 파라미터(Intrinsic Parameter)는 초점 거리(Focal Length), 주점(Principal Point), 렌즈 왜곡(Lens Distortion)을 정의하고, 외부 파라미터(Extrinsic Parameter)는 다른 좌표 프레임(Coordinate Frame)에 대한 카메라의 자세(Pose)를 정의한다. 따라서 이미지가 위치 추정(Localization), 측정(Measurement), 센서 융합(Sensor Fusion), 3차원 재구성(3D Reconstruction)에 사용된다면 신뢰할 수 있는 보정 메타데이터(Calibration Metadata)가 이미지 데이터셋(Image Dataset)과 함께 관리되어야 한다.

포인트 클라우드(Point Cloud)는 물리적 환경을 일반적으로 X, Y, Z 좌표로 표현되는 3차원 점(Point)의 집합으로 나타낸다. 라이다(LiDAR)가 주요 생성원이지만 스테레오 카메라(Stereo Camera)와 깊이 카메라(Depth Camera)도 포인트 클라우드 표현을 생성할 수 있다. 개별 점에는 강도(Intensity), 반사도(Reflectivity), 타임스탬프(Timestamp), 링 번호(Ring Number), 신뢰도(Confidence), 색상(Color), 의미 속성(Semantic Attribute) 등이 추가될 수 있다. 따라서 포인트 클라우드는 일반적인 이미지처럼 고정된 2차원 배열이 아니라 불규칙한 공간 데이터(Irregular Spatial Data)로 이해해야 한다.

포인트 클라우드(Point Cloud)의 크기와 구조는 센서 아키텍처(Sensor Architecture)와 데이터 획득 조건(Acquisition Condition)에 따라 달라진다. 회전식 다채널 라이다(Rotating Multi-Channel LiDAR)는 초당 수십만 개에서 수백만 개의 점을 생성할 수 있으며, 솔리드 스테이트 센서(Solid-State Sensor)는 서로 다른 스캐닝 패턴(Scanning Pattern)과 밀도(Density)를 생성할 수 있다. 점의 분포는 거리, 관측 각도(Viewing Angle), 가림(Occlusion), 표면 반사도(Surface Reflectivity), 센서 움직임에 따라 변하기 때문에 일반적으로 균일하지 않다. 따라서 데이터 처리 과정에서는 기하학적 구조(Geometric Structure)와 센서별 샘플링 특성(Sampling Behavior)을 함께 고려해야 한다.

포인트 클라우드 데이터(Point-Cloud Data)는 특히 좌표 프레임(Coordinate Frame)에 크게 의존한다. 측정 데이터는 처음에는 라이다 센서 프레임(LiDAR Sensor Frame)에 존재하고 이후 로봇 베이스(Robot Base), 오도메트리(Odometry), 지도(Map), 전역 기준 프레임(Global Reference Frame)으로 변환될 수 있다. 이러한 변환에는 정확한 외부 보정(Extrinsic Calibration)과 타임스탬프 정렬(Timestamp Alignment)이 필요하다. 이동하는 로봇이 서로 다른 위치와 자세에서 획득한 스캔(Scan)을 결합할 때 작은 시간 오차나 보정 오차도 중복된 경계, 왜곡된 구조, 일관되지 않은 지도(Map)를 발생시킬 수 있다.

관성 측정 장치 데이터(IMU Data)는 이미지(Image)와 포인트 클라우드(Point Cloud) 모두와 크게 다른 특성을 가진다. 관성 측정 장치(Inertial Measurement Unit)는 일반적으로 서로 직교하는 세 축을 따라 가속도(Acceleration)와 각속도(Angular Velocity)를 측정하며, 초당 수십 회에서 수백 회 또는 수천 회까지 데이터를 생성할 수 있다. 일부 장치는 자기장(Magnetic Field), 자세 추정값(Orientation Estimate), 온도(Temperature), 내부 상태 정보(Internal Status Information)도 제공한다. 개별 샘플(Sample)의 크기는 작지만 데이터 스트림은 매우 조밀하며 시간 정확성(Time Accuracy)에 민감하다.

관성 측정값(Inertial Measurement)에는 잡음(Noise), 바이어스(Bias), 스케일 팩터 오차(Scale-Factor Error), 축 정렬 오차(Axis Misalignment), 온도 영향(Temperature Effect), 적분 누적 오차(Accumulated Integration Error)가 포함될 수 있다. 가속도계(Accelerometer) 측정값에는 움직임에 의한 가속도뿐 아니라 중력(Gravity)이 포함되며, 자이로스코프(Gyroscope)는 절대 자세가 아닌 각속도를 측정한다. 따라서 속도(Velocity), 위치(Position), 자세(Attitude)를 추정하려면 필터링(Filtering), 보정(Calibration), 적분(Integration)이 필요하며 카메라, 라이다, 위성항법시스템(GNSS), 휠 오도메트리(Wheel Odometry) 등과의 센서 융합(Sensor Fusion)이 함께 사용되는 경우가 많다.

타임스탬프 정확도(Timestamp Accuracy)는 이미지, 포인트 클라우드, 관성 측정 장치 데이터에 공통적으로 요구되는 핵심 특성이다. 카메라는 초당 수십 프레임을 생성하고, 라이다는 다른 주기로 스캔을 완료하며, 관성 측정 장치는 동일한 시간 동안 수백 개의 측정값을 생성할 수 있다. 센서 융합(Sensor Fusion)을 위해서는 어떤 관측값들이 동일한 물리적 상태(Physical State)에 대응하는지 결정해야 한다. 따라서 하드웨어 타임스탬프(Hardware Timestamp), 동기화 클록(Synchronized Clock), 트리거 신호(Trigger Signal), 정밀 시간 프로토콜(PTP), 위성항법시스템 시간(GNSS Timing) 또는 정교하게 설계된 소프트웨어 동기화(Software Synchronization)가 중요하다.

센서 데이터(Sensor Data)의 시간적 해석에서는 획득 시간(Acquisition Time)을 처리 시간(Processing Time), 발행 시간(Publication Time), 수신 시간(Reception Time), 저장 시간(Storage Time)과 구분해야 한다. 하나의 프레임은 특정 순간에 촬영되더라도 노출(Exposure), 버퍼링(Buffering), 인코딩(Encoding), 장치 드라이버(Device Driver), 미들웨어(Middleware), 네트워크 전송(Network Transmission) 때문에 수 밀리초 이후에 전달될 수 있다. 수신 시간을 획득 시간으로 사용하면 체계적인 오차(Systematic Error)가 발생할 수 있으므로 센서 기록에는 원본 타임스탬프(Source Timestamp)와 실제 관측 순서를 재구성할 수 있는 충분한 메타데이터(Metadata)를 보존해야 한다.

다중 모달 로봇 데이터셋(Multi-Modal Robot Dataset)은 타임스탬프(Timestamp)와 좌표계(Coordinate System) 사이의 관계도 명시적으로 관리해야 한다. 카메라 보정 정보가 없는 이미지, 기준 프레임(Reference Frame)이 없는 포인트 클라우드, 축 규약(Axis Convention)이 없는 관성 측정 장치 스트림은 파일 자체는 읽을 수 있어도 신뢰할 수 있는 로보틱스 처리(Robotics Processing)에 사용하기 어렵다. 따라서 센서 데이터 관리(Sensor Data Management)에서는 측정값과 함께 보정 버전(Calibration Version), 프레임 식별자(Frame Identifier), 센서 식별자(Sensor Identifier), 단위(Unit), 샘플링 설정(Sampling Configuration), 인코딩 정보(Encoding Information), 좌표 변환 관계(Transformation Relationship)를 보존해야 한다.

데이터 품질(Data Quality)의 의미도 센서 모달리티(Sensor Modality)에 따라 달라진다. 이미지 품질은 흐림(Blur), 과다 노출(Overexposure), 노출 부족(Underexposure), 오염(Contamination), 프레임 손실(Dropped Frame), 압축 아티팩트(Compression Artifact) 등으로 저하될 수 있다. 포인트 클라우드는 누락 영역(Missing Region), 다중 경로 반사(Multipath Reflection), 이상치(Outlier), 모션 왜곡(Motion Distortion), 낮은 반사율 표면 문제를 포함할 수 있다. 관성 측정 장치 스트림은 포화(Saturation), 바이어스 드리프트(Bias Drift), 진동(Vibration), 클리핑(Clipping), 타임스탬프 지터(Timestamp Jitter)의 영향을 받을 수 있으므로 모든 데이터에 하나의 일반적인 검증 규칙을 적용하기보다 센서별 품질 지표(Quality Indicator)를 평가해야 한다.

저장 아키텍처(Storage Architecture) 역시 이러한 차이를 반영해야 한다. 이미지와 포인트 클라우드는 일반적으로 대용량 페이로드(High-Volume Payload)이므로 파일 스토리지(File Storage), 객체 스토리지(Object Storage), 전문 데이터셋 스토리지(Specialized Dataset Storage)가 적합하며, 관성 측정 장치 데이터는 구조화된 시계열 레코드(Structured Time-Series Record)로 효율적으로 표현할 수 있다. 하지만 로봇 기록 시스템(Robot Recording System)은 모든 모달리티를 ROS 2 백(ROS 2 Bag)이나 MCAP 같은 동기화 컨테이너(Synchronized Container)에 저장하여 원래의 시간적 관계를 일관되게 재생하고 분석할 수도 있다.

로봇이 지속적으로 운영되는 환경에서는 선택적 저장(Selective Storage)이 필요하다. 모든 원시 카메라 프레임(Raw Camera Frame)과 라이다 반환값(LiDAR Return)을 장기간 보존하면 저장 공간과 업로드 요구량이 감당하기 어려운 수준으로 증가할 수 있다. 반대로 원시 데이터를 지나치게 제거하면 디버깅(Debugging)이나 향후 인공지능 학습(AI Training)에 필요한 정보가 사라질 수 있다. 실용적인 아키텍처는 단기 원시 데이터 버퍼링(Raw Data Buffering), 이벤트 기반 보존(Event-Triggered Preservation), 샘플링(Sampling), 압축(Compression), 메타데이터 인덱싱(Metadata Indexing), 전략적으로 가치 있는 센서 구간에 대한 장기 보존(Long-Term Retention)을 결합한다.

센서 데이터(Sensor Data)는 인지(Perception)와 피지컬 인공지능(Physical AI) 파이프라인의 기반이기도 하다. 이미지는 외형(Appearance)과 의미적 맥락(Semantic Context)을 제공하고, 포인트 클라우드는 명시적인 3차원 기하 정보(3D Geometry)를 제공하며, 관성 측정 장치는 상대적으로 느린 외부 관측 사이에서 빠른 움직임 정보(Motion Information)를 제공한다. 이러한 상호보완적 특성은 위치 추정(Localization), 지도 작성(Mapping), 객체 검출(Object Detection), 추적(Tracking), 내비게이션(Navigation), 장면 이해(Scene Understanding), 조작(Manipulation), 학습(Learning) 시스템의 신뢰성과 강건성(Robustness)을 향상시킨다.

따라서 데이터 아키텍처(Data Architecture)에서 중요한 원칙은 센서 관측값을 서로 분리된 파일이 아니라 동기화되고 보정되며 공간적으로 참조되는 레코드(Synchronized, Calibrated, Spatially Referenced Record)로 다루는 것이다. 이미지, 포인트 클라우드, 관성 측정 장치 스트림은 타임스탬프, 좌표 변환(Coordinate Transform), 보정 메타데이터, 로봇 식별 정보(Robot Identity), 임무 맥락(Mission Context), 데이터 출처 정보(Provenance)를 통해 연결되어야 한다. 이러한 기반은 이후의 기록(Recording), 압축(Compression), 동기화(Synchronization), 선택적 저장(Selective Storage), 클라우드 전송(Cloud Transfer), 품질 관리(Quality Management), 어노테이션(Annotation), 다중 모달 저장(Multimodal Storage) 단계를 체계적으로 지원한다.

## 06.02 ROS2 Bag / MCAP Sensor Data Record / Playback [w/Code]

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

ROS 2 백(ROS 2 Bag)은 ROS 2 시스템을 통해 교환되는 메시지 트래픽(Message Traffic)을 캡처하기 위한 기록 및 재생(Recording and Playback) 메커니즘을 제공한다. 각 센서를 개별 애플리케이션 전용 로거(Application-Specific Logger)를 통해 저장하는 대신, rosbag2는 선택된 토픽(Topic)을 구독하고 해당 메시지를 시간 정보와 토픽 메타데이터(Topic Metadata)와 함께 보존할 수 있다. 이를 통해 로봇 임무(Robot Mission)가 종료된 이후에도 센서 동작을 재구성할 수 있으며, 디버깅(Debugging), 검증(Validation), 데이터셋 생성(Dataset Generation), 반복 가능한 로보틱스 실험(Reproducible Robotics Experiment)을 위한 공통 기반을 제공한다.

로봇은 카메라 이미지(Camera Image), 라이다 포인트 클라우드(LiDAR Point Cloud), 관성 측정 장치 측정값(IMU Measurement), 휠 오도메트리(Wheel Odometry), 위성항법시스템 위치(GNSS Position), 좌표 변환(Transform), 진단 정보(Diagnostics), 제어 상태(Control State)를 서로 다른 ROS 2 토픽을 통해 발행할 수 있다. 이러한 토픽을 하나의 백(Bag)에 기록하면 이기종 관측 데이터(Heterogeneous Observation)를 하나의 연계된 데이터셋으로 보존할 수 있다. 결과적으로 생성되는 기록은 개별 센서 파일의 집합을 넘어 토픽 이름, 메시지 유형(Message Type), 타임스탬프(Timestamp), 직렬화 정보(Serialization Information)를 통해 특정 시점의 로봇 상태를 해석하는 데 필요한 관계를 유지한다.

rosbag2 아키텍처(rosbag2 Architecture)는 기록 로직(Recording Logic)을 기본 저장 구현(Storage Implementation)과 분리한다. ROS 2 미들웨어(Middleware)를 통해 도착한 메시지는 레코더(Recorder)에 의해 수신되고 직렬화(Serialization)된 후 저장 플러그인(Storage Plugin)을 통해 기록된다. 이러한 추상화(Abstraction)를 통해 센서 애플리케이션을 변경하지 않고도 다양한 저장 형식(Storage Format)을 지원할 수 있다. 재생(Playback)은 반대 과정으로 동작하며 저장된 메시지를 읽고 시간 순서를 재구성한 다음 ROS 2 통신 환경(Communication Environment)에 다시 발행한다.

MCAP은 이기종 로보틱스 및 센서 데이터셋(Heterogeneous Robotics and Sensor Dataset)을 위한 저장 형식으로 특히 유용하다. MCAP은 타임스탬프가 포함된 메시지(Timestamped Message)를 스키마(Schema), 채널(Channel), 메타데이터(Metadata), 인덱싱 정보(Indexing Information)와 함께 저장할 수 있도록 설계된 컨테이너(Container)이다. 따라서 이미지, 포인트 클라우드, 관성 측정 장치 샘플(IMU Sample), 좌표 변환 및 기타 ROS 2 메시지를 하나의 구조화된 기록(Structured Recording) 안에서 연계된 상태로 유지하면서 각각의 데이터 스트림(Data Stream)을 식별하고 디코딩하는 데 필요한 정보를 보존할 수 있다.

기록 설정(Recording Configuration)은 사용 가능한 모든 토픽을 단순히 저장하는 방식이 아니라 데이터셋의 목적에 맞게 설계해야 한다. 디버깅 세션(Debugging Session)에서는 제어 명령(Control Command), 진단 정보, 좌표 변환, 선택된 센서 스트림이 필요할 수 있으며, 인공지능 학습 데이터셋(AI Training Dataset)은 원시 이미지(Raw Image), 포인트 클라우드, 위치 추정 정보(Localization Information), 동기화된 상태 레이블(Synchronized State Label)을 우선적으로 저장할 수 있다. 적절한 토픽을 선택하면 불필요한 저장 공간 소비를 줄이고 이후 분석, 전송, 인덱싱(Indexing), 데이터셋 준비 과정을 보다 효율적으로 관리할 수 있다.

센서 대역폭(Sensor Bandwidth)은 기록 과정에서 중요한 고려 사항이다. 고해상도 카메라(High-Resolution Camera)와 고밀도 라이다(Dense LiDAR) 스트림은 관성 측정 장치, 오도메트리 또는 진단 토픽보다 훨씬 많은 데이터를 생성할 수 있다. 여러 고대역폭 센서(High-Bandwidth Sensor)가 동시에 동작하면 저장 처리량(Storage Throughput)이 제한 요소가 될 수 있다. 따라서 기록 아키텍처는 디스크 성능(Disk Performance), 메시지 전송률(Message Rate), 직렬화 오버헤드(Serialization Overhead), 사용 가능한 CPU 자원, 버퍼링 용량(Buffering Capacity), 지속적인 고부하 상태에서 발생할 수 있는 메시지 손실(Dropped Message)을 함께 고려해야 한다.

서로 다른 센서 스트림이 동일한 백(Bag)에 저장되더라도 타임스탬프(Timestamp)는 여전히 핵심적인 요소이다. 메시지가 수신되거나 기록되는 순서가 정확한 물리적 데이터 획득 순서(Physical Acquisition Order)를 의미하는 것은 아니다. 장치 드라이버(Device Driver), 미들웨어 큐(Middleware Queue), 이미지 인코딩(Image Encoding), 네트워크 전송(Network Transport), 처리 지연(Processing Latency)이 센서마다 다를 수 있기 때문이다. 따라서 강건한 기록 설계(Robust Recording Design)는 의미 있는 원본 시간 정보(Source Timing Information)를 보존하여 이후 카메라, 라이다, 관성 측정 장치, 로봇 상태 관측값을 정확하게 정렬할 수 있도록 해야 한다.

공간 센서 데이터(Spatial Sensor Data)를 재생하거나 분석하려면 좌표 프레임 정보(Coordinate-Frame Information)도 함께 기록해야 한다. 카메라 프레임(Camera Frame), 라이다 프레임(LiDAR Frame), 관성 측정 장치 프레임(IMU Frame), 로봇 베이스 프레임(Robot Base Frame), 오도메트리 프레임(Odometry Frame), 지도 프레임(Map Frame)은 일반적으로 ROS 2 좌표 변환 관계(Transform Relationship)를 통해 연결된다. 관련 정적 및 동적 좌표 변환(Static and Dynamic Transform)을 기록하면 후속 애플리케이션에서 각 센서 관측값이 해당 시점에 로봇 및 주변 환경과 어떤 상대적 위치 관계를 가지고 있었는지 재구성할 수 있다.

재생(Playback)은 기록된 데이터셋을 반복 가능한 가상 센서 시퀀스(Repeatable Virtual Sensor Sequence)로 변환한다. 저장된 메시지를 다시 발행하면 인지(Perception), 위치 추정(Localization), 지도 작성(Mapping), 모니터링(Monitoring), 진단 노드(Diagnostic Node)가 원래의 ROS 2 토픽 스트림과 유사한 데이터를 수신할 수 있다. 따라서 개발자는 실제 로봇을 다시 운용하지 않고도 동일한 관측 데이터를 대상으로 알고리즘을 반복적으로 실행할 수 있으며, 소프트웨어 버전, 파라미터(Parameter), 인지 모델(Perception Model), 위치 추정 알고리즘(Localization Algorithm)을 비교할 때 재현성(Reproducibility)을 향상시킬 수 있다.

재생 타이밍(Playback Timing)은 로보틱스 테스트(Robotics Testing)에서 특히 중요하다. 기록 당시의 시간적 관계(Temporal Relationship)에 따라 메시지를 재현하면 소프트웨어 구성 요소가 데이터 수집 당시 발생했던 것과 유사한 이벤트 순서(Event Sequence)를 경험하도록 할 수 있다. 실험 목적에 따라 정상 속도(Normal Speed)로 재생하거나 세부 분석을 위해 느리게 재생하고, 오프라인 처리(Offline Processing)를 위해 빠르게 재생할 수도 있다. 일시 정지(Pause)와 제어된 재생(Controlled Replay)을 사용하면 위치 추정 실패, 인지 오류, 비정상적인 로봇 동작과 관련된 짧은 구간을 분리하여 분석하는 데 도움이 된다.

기록된 백(Bag)은 데이터 획득(Data Acquisition)과 문제 분석(Problem Investigation)을 분리할 수 있기 때문에 디버깅에 매우 유용하다. 현장 임무(Field Mission) 중 발생한 장애는 실제 환경에서 다시 재현하기 어렵거나 많은 비용이 필요할 수 있지만, 충분히 완전한 기록이 존재한다면 장애 발생 전후의 센서 및 시스템 맥락(System Context)을 보존할 수 있다. 엔지니어는 관련 토픽을 확인하고 해당 시퀀스를 재생하며 알고리즘을 수정한 뒤 동일한 입력 조건을 기반으로 새로운 출력 결과를 비교할 수 있으므로 원래의 환경으로 반복해서 돌아갈 필요가 없다.

ROS 2 백 기록(ROS 2 Bag Recording)은 로봇 운영(Robot Operation)과 인공지능 데이터 파이프라인(AI Data Pipeline)을 연결하는 효과적인 가교 역할도 수행한다. 임무 중 수집된 원시 센서 토픽(Raw Sensor Topic)은 이미지 데이터셋(Image Dataset), 포인트 클라우드 시퀀스(Point-Cloud Sequence), 궤적(Trajectory), 어노테이션(Annotation), 다중 모달 학습 샘플(Multimodal Training Sample)로 추출하거나 변환할 수 있다. 원본 백을 소스 데이터셋(Source Dataset)으로 유지하면 데이터 출처(Provenance)를 보존할 수 있으며, 인지 학습, 센서 융합 실험, 이상 탐지(Anomaly Detection), 모방 학습(Imitation Learning), 피지컬 인공지능(Physical AI) 등의 목적으로 파생 데이터셋(Derived Dataset)을 생성할 수 있다.

기록 데이터의 수가 증가할수록 메타데이터 관리(Metadata Management)의 중요성도 높아진다. 유용한 센서 데이터셋은 백 파일 자체뿐 아니라 로봇 식별 정보(Robot Identity), 임무(Mission), 기록 시간(Recording Time), 소프트웨어 구성(Software Configuration), 센서 구성(Sensor Configuration), 보정 버전(Calibration Version), 환경(Environment), 운영 목적(Operational Purpose)을 식별할 수 있어야 한다. 이러한 맥락 정보(Contextual Information)가 없다면 기술적으로 정상적인 기록이 대량으로 축적되더라도 검색하거나 서로 비교하기 어려워질 수 있다. 따라서 데이터셋 카탈로그(Dataset Catalog)는 백 파일을 운영 및 구성 메타데이터와 연결해야 한다.

대규모 기록(Large Recording)은 수명주기 및 저장 계획(Lifecycle and Storage Planning)도 필요로 한다. 여러 카메라와 라이다를 기록하는 경우 지속적인 로봇 운영은 빠르게 수백 기가바이트 이상의 데이터를 생성할 수 있다. 실용적인 시스템에서는 기록을 관리 가능한 세그먼트(Segment)로 분할하고, 보존 정책(Retention Policy)을 적용하며, 이벤트 관련 구간(Event-Related Interval)을 보존하고, 선택된 데이터를 온보드 스토리지(Onboard Storage)에서 네트워크 결합 스토리지(NAS), 객체 스토리지(Object Storage), 클라우드 인프라(Cloud Infrastructure)로 이동할 수 있다. 압축(Compression)은 저장 공간 요구량을 줄일 수 있지만 처리 오버헤드와 향후 데이터 재사용 요구사항 사이의 균형을 고려해야 한다.

신뢰성(Reliability)을 확보하려면 비정상적인 기록 조건(Abnormal Recording Condition)에 대한 대응도 필요하다. 전원 손실(Power Loss), 애플리케이션 종료(Application Termination), 디스크 용량 부족(Insufficient Disk Capacity), 저장 지연(Storage Latency), 손상된 쓰기(Corrupted Write)는 데이터 수집을 중단시킬 수 있다. 따라서 실제 운영 환경을 위한 기록 아키텍처(Production-Oriented Recording Architecture)는 사용 가능한 저장 공간, 기록 상태(Recording Status), 처리량(Throughput), 메시지 손실(Message Loss), 파일 무결성(File Integrity)을 모니터링해야 한다. 장시간 임무를 여러 세그먼트로 분리하면 개별 장애의 영향을 제한하고 이후 업로드, 복구(Recovery), 인덱싱, 선택적 보존(Selective Retention)을 단순화할 수 있다.

보안(Security)과 개인정보 보호(Privacy)도 고려해야 한다. ROS 2 백에는 개발자가 처음 예상했던 것보다 훨씬 많은 정보가 포함될 수 있기 때문이다. 카메라 기록에는 식별 가능한 사람이나 사적인 환경이 포함될 수 있으며, 위치(Location), 오디오(Audio), 운영 명령(Operational Command), 진단 데이터(Diagnostic Data)는 민감한 시스템 정보(Sensitive System Information)를 노출할 수 있다. 따라서 접근 제어(Access Control), 암호화(Encryption), 보존 규칙(Retention Rule), 익명화(Anonymization), 통제된 데이터셋 배포(Controlled Dataset Distribution)를 기록 데이터가 축적된 이후에 추가하는 것이 아니라 센서 데이터 수명주기(Sensor-Data Lifecycle) 전체에 통합해야 한다.

보다 광범위한 로봇 데이터 아키텍처(Robot Data Architecture)에서 ROS 2 백(ROS 2 Bag)과 MCAP은 물리적 로봇 운영(Physical Robot Operation)을 오프라인 엔지니어링(Offline Engineering) 및 인공지능 워크플로(AI Workflow)와 연결하는 구조화된 캡처 및 재생 계층(Structured Capture and Replay Layer)으로 이해해야 한다. 이들은 이기종 ROS 2 센서 스트림을 재현 가능한 형태로 보존하고 이후 재생, 분석, 디버깅, 검증, 어노테이션, 학습에 활용할 수 있도록 한다. 동기화(Synchronization), 메타데이터, 보정(Calibration), 품질 관리(Quality Management), 저장 거버넌스(Storage Governance)와 결합하면 기록된 센서 데이터는 일시적인 운영 로그(Operational Log)를 넘어 반복적으로 활용할 수 있는 엔지니어링 자산(Reusable Engineering Asset)이 된다.

## 06.03 Sensor Data Compression: H265 / Draco [w/Code]

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

센서 데이터 압축(Sensor Data Compression)은 현대 로봇이 이미지(Image), 비디오(Video), 포인트 클라우드(Point Cloud), 깊이 정보(Depth Information), 기타 고대역폭 센서 스트림(High-Bandwidth Sensor Stream)을 지속적으로 대량 생성하기 때문에 로봇 데이터 아키텍처(Robot Data Architecture)의 핵심 기능이다. 압축하지 않으면 온보드 저장 공간(Onboard Storage)이 빠르게 소진되고 엣지-클라우드 전송(Edge-to-Cloud Transfer)이 과도한 네트워크 대역폭(Network Bandwidth)을 소비할 수 있다. 압축은 인지(Perception), 디버깅(Debugging), 재생(Playback), 인공지능 학습(AI Training), 장기 분석(Long-Term Analysis)에 필요한 정보를 최대한 보존하면서 데이터 용량을 줄인다.

압축 전략(Compression Strategy)은 모든 데이터에 하나의 알고리즘을 적용하는 것이 아니라 각 센서 모달리티(Sensor Modality)의 특성을 반영해야 한다. 카메라 스트림(Camera Stream)은 비디오 코덱(Video Codec)이 활용할 수 있는 공간적·시간적 중복성(Spatial and Temporal Redundancy)을 포함하는 반면, 라이다 포인트 클라우드(LiDAR Point Cloud)는 기하학 중심의 압축 방법(Geometry-Oriented Method)이 필요한 불규칙한 3차원 좌표와 속성으로 구성된다. 관성 측정 장치(IMU)와 스칼라 텔레메트리(Scalar Telemetry)는 데이터 크기가 훨씬 작기 때문에 계산 비용이 높은 멀티미디어 압축보다 경량 인코딩(Lightweight Encoding)이 적합할 수 있다.

H.265는 고효율 비디오 코딩(High Efficiency Video Coding, HEVC)이라고도 하며 로봇에서 생성되는 대용량 카메라 스트림을 압축하는 데 적합하다. H.265는 각각의 이미지 프레임을 독립적으로 저장하는 대신 개별 프레임 내부와 연속된 프레임 사이의 유사성을 활용한다. 이를 통해 연속 비디오 기록(Continuous Video Recording)에 필요한 데이터 용량을 크게 줄일 수 있으므로 여러 대의 고해상도 카메라(High-Resolution Camera)를 장착한 이동 로봇(Mobile Robot)에서도 장시간 영상 기록을 보다 현실적으로 수행할 수 있다.

시간적 압축(Temporal Compression)은 연속된 로봇 카메라 프레임 사이에 상당한 유사성이 존재하기 때문에 특히 효과적이다. H.265는 모든 픽셀을 독립적으로 인코딩하는 대신 일부 프레임을 인접 프레임에서 얻은 정보를 이용하여 표현할 수 있다. 로봇이 이동하더라도 연속 프레임 사이에서 시각적 장면(Visual Scene)의 일부 영역만 크게 변할 수 있다. 움직임 추정(Motion Estimation)과 예측(Prediction)을 사용하면 반복되는 시각 정보를 보다 효율적으로 표현하여 저장 공간과 전송 대역폭 요구량을 줄일 수 있다.

H.265의 장점에는 중요한 아키텍처 절충 관계(Architectural Tradeoff)가 따른다. 높은 압축률(Compression Ratio)은 대역폭과 저장 공간 사용량을 줄일 수 있지만 인코딩(Encoding)과 디코딩(Decoding)에 계산 자원이 필요하고 지연 시간(Latency)이 발생할 수 있다. 또한 압축 설정은 이미지 품질(Image Quality)과 미세한 세부 정보의 가시성에 영향을 미친다. 따라서 로봇 시스템은 데이터가 모니터링(Monitoring), 내비게이션 분석(Navigation Analysis), 사고 조사(Incident Investigation), 인공지능 학습 중 어디에 사용되는지에 따라 비트레이트(Bitrate), 해상도(Resolution), 프레임률(Frame Rate), 지연 시간, 연산 자원 사용량, 화질 사이의 균형을 설정해야 한다.

손실 압축(Lossy Compression)은 기록된 이미지가 향후 학습 데이터(Training Data)로 사용되는 경우 특히 주의해야 한다. 압축 아티팩트(Compression Artifact)는 인지 모델(Perception Model)이 사용하는 경계(Edge), 텍스처(Texture), 작은 객체(Small Object), 저조도 세부 정보(Low-Light Detail) 등의 시각적 특징을 변화시킬 수 있다. 주로 사람이 모니터링하기 위한 운영 영상(Operational Video)은 더 높은 압축을 허용할 수 있지만, 컴퓨터 비전(Computer Vision) 개발을 위한 데이터셋은 높은 품질 설정이나 선택적으로 저장된 원시 프레임(Raw Frame)이 필요할 수 있다. 따라서 압축 정책은 데이터의 후속 활용 목적(Downstream Use)에 연결되어야 한다.

하드웨어 가속(Hardware Acceleration)을 사용하면 엣지 로봇(Edge Robot)에서 비디오 압축을 보다 실용적으로 수행할 수 있다. GPU와 전용 비디오 인코딩 하드웨어(Dedicated Video Encoding Hardware)는 전체 작업을 범용 CPU 코어에 집중시키지 않고 카메라 스트림을 처리할 수 있다. 이는 온보드 컴퓨터(Onboard Computer)가 위치 추정(Localization), 객체 검출(Object Detection), 내비게이션(Navigation), 센서 융합(Sensor Fusion), 제어(Control)를 동시에 수행할 때 중요하다. 데이터 관리 기능이 안전 필수(Safety-Critical) 또는 실시간 로봇 기능(Real-Time Robot Function)을 방해하지 않도록 압축에 사용되는 자원을 제한하고 관리해야 한다.

포인트 클라우드(Point Cloud)는 기본 구조가 직사각형 픽셀 그리드(Rectangular Pixel Grid)가 아니라 3차원 기하 구조(Three-Dimensional Geometry)이기 때문에 다른 압축 접근 방식이 필요하다. 라이다 데이터는 한 번의 스캔에서 수십만 개의 점을 포함할 수 있으며, 각각의 점에는 좌표와 함께 강도(Intensity), 반사도(Reflectivity), 색상(Color), 타임스탬프(Timestamp), 의미 정보(Semantic Information) 등의 속성이 포함될 수 있다. 따라서 고밀도 포인트 클라우드를 반복적으로 기록하면 카메라 데이터가 이미 압축된 경우에도 상당한 저장 공간과 네트워크 자원이 필요하다.

드라코(Draco)는 포인트 클라우드와 메시(Mesh) 같은 3차원 기하 데이터(Three-Dimensional Geometric Data)를 위해 설계된 압축 기술이다. 포인트 클라우드 애플리케이션에서 드라코는 기하학적 구조와 효율적인 인코딩을 활용하여 공간 좌표(Spatial Coordinate)와 관련 속성(Attribute)의 표현 크기를 줄일 수 있다. 따라서 라이다 기반 데이터셋(LiDAR-Derived Dataset)이나 재구성된 3차원 정보(Reconstructed 3D Information)를 저장, 전송, 시각화하거나 엣지(Edge), 서버(Server), 클라우드(Cloud) 구성 요소 사이에서 교환해야 하는 환경에 활용할 수 있다.

포인트 클라우드 압축(Point-Cloud Compression)은 로보틱스 작업(Robotics Task)에 필요한 수준의 기하학적 정확도(Geometric Accuracy)를 보존해야 한다. 양자화(Quantization)는 더 적은 비트로 좌표를 표현하여 데이터 크기를 줄일 수 있지만 위치 근사 오차(Positional Approximation)를 발생시킨다. 시각화(Visualization)는 정밀 지도 작성(Precision Mapping)이나 위치 추정보다 더 큰 기하학적 손실을 허용할 수 있다. 마찬가지로 의미 인지(Semantic Perception)는 일정 수준의 점 밀도 변화를 허용할 수 있지만 보정 분석(Calibration Analysis)이나 정밀 인프라 점검(Detailed Infrastructure Inspection)은 훨씬 높은 공간 정확도(Spatial Fidelity)를 요구할 수 있다.

포인트 클라우드의 각 점과 연계된 속성(Attribute)에도 명시적인 압축 정책(Compression Policy)이 필요하다. 강도, 색상, 분류(Classification), 신뢰도(Confidence), 타임스탬프, 센서 채널 정보(Sensor-Channel Information)는 특정 알고리즘에서 XYZ 기하 정보만큼 중요할 수 있다. 이러한 필드를 제거하거나 과도하게 압축하면 데이터셋 크기는 줄어들지만 향후 활용 가치도 감소할 수 있다. 따라서 강건한 데이터 아키텍처(Robust Data Architecture)는 각 기록 및 보존 등급(Recording and Retention Class)에 따라 필수 속성(Mandatory Attribute), 선택 속성(Optional Attribute), 파생 속성(Derived Attribute), 제거 가능한 속성(Disposable Attribute)을 정의해야 한다.

압축률(Compression Ratio)만으로 압축의 성공 여부를 평가해서는 안 된다. 인코딩 처리량(Encoding Throughput), 디코딩 처리량(Decoding Throughput), 지연 시간, CPU 및 GPU 사용률(Utilization), 메모리 소비량(Memory Consumption), 에너지 사용량(Energy Usage), 복원 품질(Reconstruction Quality)도 함께 평가해야 한다. 데이터 크기를 크게 줄일 수 있는 코덱이라도 센서 데이터 획득 속도를 따라가지 못한다면 버퍼링(Buffering)이나 메시지 손실(Message Loss)이 발생할 수 있다. 따라서 실시간 로봇 압축(Real-Time Robot Compression)은 실제 온보드 연산 부하 환경에서 입력되는 센서 데이터 속도를 지속적으로 처리할 수 있어야 한다.

데이터 파이프라인(Data Pipeline)에서 압축이 수행되는 위치도 중요하다. 압축은 센서 데이터 획득 직후, ROS 2 기록 과정, 네트워크 업로드 이전 또는 아카이브 처리(Archival Processing) 과정에서 수행할 수 있다. 조기 압축(Early Compression)은 저장 공간과 네트워크 부하를 최소화하지만 시스템이 특정 데이터 표현 방식을 일찍 확정하게 만든다. 후단 압축(Later Compression)은 보다 풍부한 원시 정보를 일시적으로 보존할 수 있지만 더 큰 온보드 저장 용량을 요구한다. 따라서 많은 로봇 아키텍처는 단기 원시 데이터 버퍼링(Short-Term Raw Buffering)과 압축된 운영 데이터 저장(Compressed Operational Storage)을 결합한다.

적응형 압축(Adaptive Compression)은 네트워크와 운영 조건이 변화하는 환경에서 효율성을 높일 수 있다. 고대역폭 와이파이(Wi-Fi)나 이더넷(Ethernet)에 연결된 로봇은 더 높은 품질의 데이터를 유지하거나 전송할 수 있지만 제한된 LTE 또는 5G 연결에서는 비트레이트 감소나 선택적 업로드(Selective Upload)가 필요할 수 있다. 중요한 이벤트(Critical Event)가 발생하면 고품질 센서 구간을 보존하고 일반적인 운영 구간에는 더 강한 압축을 적용할 수 있다. 이를 통해 저장 및 통신 자원을 데이터의 운영 가치(Operational Value)에 따라 배분할 수 있다.

압축은 기록 및 재생 워크플로(Recording and Playback Workflow)와도 호환되어야 한다. H.265 비디오나 드라코 압축 포인트 클라우드(Draco-Compressed Point Cloud)를 ROS 2 백(ROS 2 Bag)과 MCAP 데이터셋 내부 또는 연계된 형태로 저장하는 경우 메타데이터에는 인코딩 방식(Encoding Method), 파라미터(Parameter), 타임스탬프, 센서 식별 정보(Sensor Identity), 보정 맥락(Calibration Context)이 포함되어야 한다. 재생과 후속 처리에서는 압축된 표현을 안정적으로 복원하고 올바른 ROS 2 토픽 및 좌표 프레임(Coordinate Frame)에 연결할 수 있는 신뢰성 높은 디코딩이 필요하다.

성공적으로 인코딩되었다는 사실만으로 센서 데이터의 유용성이 보장되는 것은 아니므로 압축 이후 데이터 품질 검증(Data Quality Validation)을 수행해야 한다. 카메라 스트림은 프레임 손실(Frame Loss), 시각적 품질 저하(Visual Degradation), 데이터 손상(Corruption), 타임스탬프 연속성(Timestamp Continuity)을 검사할 수 있으며, 복원된 포인트 클라우드는 기하학적 오차(Geometric Error), 점 손실(Point Loss), 속성 보존(Attribute Preservation), 공간적 일관성(Spatial Consistency)을 평가할 수 있다. 품질 임계값(Quality Threshold)은 일반적인 코덱 파라미터가 아니라 실제 후속 활용 요구사항을 기준으로 정의해야 한다.

압축은 장기 데이터 거버넌스(Long-Term Data Governance)에도 영향을 미친다. 원시 센서 데이터(Raw Sensor Data)는 단기간 보존하고, 고품질 압축 데이터(High-Quality Compressed Data)는 엔지니어링 재사용(Engineering Reuse)을 위해 보존하며, 더 강하게 압축된 파생 데이터(Derived Data)는 운영 이력(Operational History)을 위해 장기간 유지할 수 있다. 인공지능 학습이나 사고 분석에 중요한 데이터 구간은 더 높은 품질과 장기 보존 정책을 적용할 수 있다. 따라서 기록 등급(Recording Class), 압축 프로파일(Compression Profile), 보존 정책(Retention Policy), 데이터셋 목적(Dataset Purpose)을 메타데이터를 통해 연결하여 향후 사용자가 어떤 정보가 보존되고 어떤 정보가 제거되었는지 이해할 수 있도록 해야 한다.

전체 로봇 센서 데이터 파이프라인(Robot Sensor Data Pipeline)에서 H.265와 드라코(Draco)는 두 가지 주요 고대역폭 모달리티(High-Bandwidth Modality)를 위한 상호보완적인 압축 방법이다. H.265는 공간적·시간적 중복성을 활용하여 카메라 및 비디오 데이터의 크기를 줄이고, 드라코는 3차원 데이터의 기하학적 중복성(Geometric Redundancy)을 줄인다. 선택적 저장(Selective Storage), ROS 2 백 또는 MCAP 기록, 동기화(Synchronization), 품질 관리(Quality Management), 엣지-클라우드 전송과 결합하면 센서 압축은 모든 관측 데이터를 동일한 비용의 원시 데이터로 취급하지 않으면서 확장 가능한 로봇 데이터 관리(Scalable Robot Data Management)를 가능하게 한다.

## 06.04 Sensor Data Timestamp Synchronization Design [w/Code]

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

센서 타임스탬프 동기화(Sensor Timestamp Synchronization)는 센서들이 서로 다른 주기와 독립적인 하드웨어 파이프라인(Hardware Pipeline)을 통해 물리적 세계를 관측하기 때문에 로봇 데이터 아키텍처(Robot Data Architecture)의 기본 요구사항이다. 카메라는 초당 수십 프레임으로 동작하고, 라이다(LiDAR)는 초당 여러 번 스캔하며, 관성 측정 장치(IMU)는 초당 수백 또는 수천 개의 샘플을 생성할 수 있다. 동기화는 이러한 관측값을 동일한 로봇 상태(Robot State)의 일부로 해석할 수 있도록 공통 시간 기준(Common Temporal Reference)을 확립한다.

타임스탬프(Timestamp)는 센서 데이터 수명주기(Sensor Data Lifecycle)에서 명확하게 정의된 이벤트를 나타내야 한다. 획득 시간(Acquisition Time)은 물리적 측정값이 캡처된 시점을 나타내며, 발행 시간(Publication Time), 수신 시간(Reception Time), 처리 시간(Processing Time), 저장 시간(Storage Time)은 파이프라인의 이후 단계를 나타낸다. 노출 시간, 센서 버퍼링(Buffering), 장치 드라이버(Device Driver), 직렬화(Serialization), 미들웨어 큐(Middleware Queue), 연산, 네트워크 전송으로 인해 이러한 시간은 서로 달라질 수 있다. 따라서 센서 융합(Sensor Fusion)에서는 물리적 획득 시점을 가장 정확하게 나타내는 타임스탬프를 사용해야 한다.

클록 차이(Clock Difference)는 동기화 오류(Synchronization Error)의 주요 원인이다. 개별 센서, 임베디드 컨트롤러(Embedded Controller), 온보드 컴퓨터(Onboard Computer), 외부 서버(External Server)는 서로 다른 오실레이터(Oscillator)를 사용하는 독립적인 클록으로 동작할 수 있으며 각각의 클록은 서로 다른 오프셋(Offset)과 주파수를 가질 수 있다. 두 장치의 시간이 처음에는 동일하더라도 오실레이터 드리프트(Oscillator Drift)로 인해 시간이 지나면서 차이가 증가할 수 있다. 따라서 동기화 아키텍처는 초기화 이후에도 클록이 계속 정렬되어 있다고 가정하지 않고 클록 오프셋과 클록 드리프트(Clock Drift)를 모두 제어해야 한다.

하드웨어 동기화(Hardware Synchronization)는 물리적인 타이밍 신호(Physical Timing Signal)를 통해 센서를 조정함으로써 높은 시간적 일관성(Temporal Consistency)을 제공한다. 트리거 펄스(Trigger Pulse)는 여러 카메라 또는 센서가 알려진 시점에 동시에 관측 데이터를 획득하도록 명령할 수 있으며, 초당 펄스(Pulse Per Second, PPS) 신호는 주기적인 시간 기준을 제공할 수 있다. 하드웨어가 이를 지원하는 경우 동기화된 트리거(Synchronized Triggering)는 소프트웨어 스케줄링, 운영체제 지연, 미들웨어 큐, 가변적인 통신 지연으로 발생하는 시간적 불확실성을 줄일 수 있다.

하드웨어 타임스탬프(Hardware Timestamp)는 실제 데이터 획득 또는 전송 이벤트에 가까운 위치에서 시간 정보를 부여하여 동기화 정확도를 향상시킨다. 센서 내부, 네트워크 인터페이스(Network Interface), 전용 타이밍 장치(Dedicated Timing Device)에서 생성된 타임스탬프는 데이터가 애플리케이션에 도착한 이후 생성되는 타임스탬프보다 운영체제 스케줄링의 영향을 적게 받는다. 따라서 아키텍처는 후속 처리 단계에서 원본 하드웨어 타임스탬프를 나중에 생성된 소프트웨어 수신 시간으로 대체하지 않고 지속적으로 보존해야 한다.

정밀 시간 프로토콜(Precision Time Protocol, PTP)은 일반적으로 PTP로 표현되며 IEEE 1588로 표준화된 네트워크 장치 간 클록 동기화 기술이다. PTP는 클록 사이에서 타이밍 정보를 교환하고 네트워크 지연(Network Delay)을 추정하여 참여 장치의 로컬 클록(Local Clock)을 공통 시간 기준에 맞춘다. 하드웨어 타임스탬핑(Hardware Timestamping)을 사용하면 예측하기 어려운 소프트웨어 처리 이후가 아니라 물리적 네트워크 인터페이스에 가까운 위치에서 패킷 시간을 측정할 수 있으므로 정밀도를 더욱 향상시킬 수 있다.

일반화 정밀 시간 프로토콜(Generalized Precision Time Protocol, gPTP)은 네트워크 구성 요소 전체에서 조정된 시간이 필요한 시스템에 정밀 타이밍 개념을 적용한다. 로보틱스에서는 결정론적 이더넷(Deterministic Ethernet)과 시간 인식 통신(Time-Aware Communication)이 동기화된 클록을 활용하여 센서 데이터 획득, 액추에이터 명령(Actuator Command), 분산 연산(Distributed Computation)을 하나의 공유 시간 영역(Shared Temporal Domain)에 연결할 수 있다. 이는 인지 및 제어 기능이 하나의 프로세서가 아니라 여러 엣지 컴퓨터(Edge Computer)에 분산되는 시스템에서 더욱 중요해진다.

위성항법시스템(GNSS)은 실외 로봇(Outdoor Robot), 자율주행 차량(Autonomous Vehicle), 무인항공기(UAV)에 외부 절대 시간 기준(External Absolute Time Reference)을 제공할 수 있다. GNSS 수신기는 시간 정보와 초당 펄스(PPS) 출력을 제공하여 로컬 클록을 기준 시간에 맞출 수 있다. 이를 통해 센서 측정값을 서로 연계할 뿐 아니라 전역적으로 참조되는 시간(Global Reference Time)과 연결할 수 있다. 그러나 실내 운용, 신호 차단, 간섭(Interference), 수신 품질 저하 상황에서는 로컬 타이밍 메커니즘(Local Timing Mechanism)을 통해 동기화를 유지해야 한다.

센서가 하드웨어 트리거를 받을 수 없거나 공통 클록 인프라(Common Clock Infrastructure)에 직접 참여할 수 없는 경우에는 소프트웨어 동기화(Software Synchronization)가 필요하다. 미들웨어(Middleware)는 타임스탬프를 이용하여 메시지를 연계하고 허용 가능한 시간 범위(Temporal Window)에 포함되는 관측값을 선택할 수 있다. 정확 동기화(Exact Synchronization)는 서로 매우 가까운 타임스탬프를 요구하지만 근사 동기화(Approximate Synchronization)는 제한된 범위의 시간 차이를 허용한다. 허용 오차(Tolerance)는 센서 주기, 로봇 움직임, 애플리케이션의 정확도 요구사항을 기준으로 결정해야 한다.

동기화 허용 오차(Synchronization Tolerance)는 직접적인 물리적 의미를 가진다. 정지된 로봇에서는 작은 타임스탬프 오차가 중요하지 않을 수 있지만 로봇, 센서 또는 관측 대상이 빠르게 움직이는 경우에는 중요한 문제가 될 수 있다. 움직임이 존재하면 서로 다른 시점의 측정값이 서로 다른 물리적 자세(Physical Pose)에 대응하기 때문에 시간 오차가 공간 오차(Spatial Error)로 변환된다. 따라서 고속 내비게이션(High-Speed Navigation), 조작(Manipulation), 무인항공기 비행(UAV Flight), 동적 장면 인지(Dynamic Scene Perception)는 일반적으로 저속 모니터링 애플리케이션보다 더 엄격한 동기화를 요구한다.

라이다 모션 왜곡(LiDAR Motion Distortion)은 시간과 기하학적 구조 사이의 관계를 보여주는 대표적인 사례이다. 회전식 라이다(Rotating LiDAR)는 전체 포인트 클라우드를 반드시 하나의 순간에 획득하는 것이 아니라 하나의 스캔 과정에서 점들을 순차적으로 획득할 수 있다. 이 시간 동안 로봇이 이동하면 서로 다른 점들이 서로 다른 센서 자세에 대응하게 된다. 점 단위 또는 패킷 단위 타임스탬프(Per-Point or Packet-Level Timestamp)를 관성 측정 장치나 오도메트리(Odometry) 정보와 결합하면 모션 보상(Motion Compensation)을 수행하고 기하학적으로 보다 일관된 포인트 클라우드를 재구성할 수 있다.

카메라 동기화(Camera Synchronization)에서는 노출 동작(Exposure Behavior)도 고려해야 한다. 프레임의 타임스탬프는 노출 시작(Exposure Start), 노출 중간(Exposure Midpoint), 노출 완료(Exposure Completion) 중 어느 시점을 기준으로 하는지 명확하게 정의되어야 한다. 스테레오 비전(Stereo Vision)이나 주변 인지(Surround Perception)에 사용되는 다중 카메라 시스템(Multi-Camera System)은 움직이는 객체를 서로 다른 위치에서 관측하는 문제를 방지하기 위해 노출 시점을 정밀하게 정렬해야 한다. 롤링 셔터 카메라(Rolling-Shutter Camera)는 이미지의 행마다 서로 다른 시점에 노출될 수 있기 때문에 정밀 센서 융합을 더욱 복잡하게 만든다.

관성 측정 장치(IMU)는 상대적으로 느린 센서 사이에서 고주파 측정값을 제공하는 시간적 연결 장치(Temporal Bridge) 역할을 하는 경우가 많다. 연속된 카메라 프레임이나 라이다 스캔 사이에서 여러 개의 IMU 샘플이 생성될 수 있으며, 이를 통해 해당 시간 구간의 움직임을 추정할 수 있다. 가속도와 각속도의 적분(Integration)은 샘플링 간격(Sampling Interval)에 민감하기 때문에 정확한 타임스탬프가 필수적이다. 따라서 시간 오차는 자세(Orientation), 속도(Velocity), 모션 보상, 위치 추정(Localization), 상태 추정(State Estimation)의 오차로 전파될 수 있다.

ROS 2 센서 파이프라인(ROS 2 Sensor Pipeline)은 데이터 획득부터 기록 및 재생까지 동기화 정보를 지속적으로 보존해야 한다. 메시지 헤더(Message Header)는 일반적으로 센서 타임스탬프를 포함하며, 좌표 변환(Coordinate Transform)과 관련 상태 정보도 서로 호환되는 시간에 대응해야 한다. ROS 2 백(ROS 2 Bag)과 MCAP 기록은 이러한 시간적 관계를 유지하여 오프라인 알고리즘(Offline Algorithm)이 메시지가 저장 장치에 기록된 순서에만 의존하지 않고 원래의 데이터 시퀀스를 재구성할 수 있도록 해야 한다.

동기화 메타데이터(Synchronization Metadata)는 데이터 수집 과정에서 사용된 타이밍 아키텍처(Timing Architecture)를 문서화해야 한다. 유용한 정보에는 클록 소스(Clock Source), 타임스탬프 기준점(Timestamp Origin), 동기화 방법(Synchronization Method), 트리거 설정(Trigger Configuration), 예상 정밀도(Expected Precision), 측정된 오프셋(Measured Offset), 센서 주파수(Sensor Frequency), 알려진 타이밍 제약(Known Timing Limitation)이 포함된다. 따라서 보정(Calibration)은 기하학적 보정뿐 아니라 시간적 보정(Temporal Calibration)도 포함해야 한다. 정확한 공간 좌표 변환이 존재하더라도 시간적 관계를 알 수 없는 데이터셋은 센서 융합 애플리케이션에서 잘못된 결과를 생성할 수 있다.

로봇 운용 중에는 동기화 품질(Synchronization Quality)이 변할 수 있으므로 지속적인 모니터링(Monitoring)이 필요하다. 시스템은 클록 오프셋, 동기화 상태(Synchronization Status), 타임스탬프 불연속(Timestamp Discontinuity), 메시지 지연(Message Latency), 지터(Jitter), 샘플 누락(Missing Sample), 비정상적인 시간 점프(Abnormal Time Jump)를 추적할 수 있다. 설정된 임계값(Threshold)을 초과하면 진단 이벤트(Diagnostic Event)를 생성하고 영향을 받은 센서 구간을 품질 저하 상태(Degraded)로 표시할 수 있다. 이를 통해 시간적으로 일관되지 않은 데이터가 정상 데이터처럼 지도 작성, 인공지능 학습 또는 평가 데이터셋에 유입되는 것을 방지할 수 있다.

재동기화(Resynchronization)와 장애 처리(Fault Handling)는 일시적인 네트워크 장애, 센서 재시작, 클록 소스 변경, 외부 시간 기준 손실에 대응할 수 있도록 설계해야 한다. 센서가 재시작되면 이전과 다른 클록 오프셋을 가질 수 있으며, 연결이 끊어진 노드는 자체 로컬 오실레이터(Local Oscillator)를 기반으로 계속 동작하면서 드리프트가 누적될 수 있다. 시스템은 이러한 변화를 감지하고 동기화를 다시 확립하며, 시간 정확성을 보장할 수 없는 구간을 식별해야 한다. 전체 기록을 동일한 수준으로 동기화된 데이터라고 가정해서는 안 된다.

강건한 로봇 타이밍 아키텍처(Robust Robot Timing Architecture)는 물리적 타이밍 메커니즘(Physical Timing Mechanism), 동기화된 클록(Synchronized Clock), 정확한 획득 타임스탬프(Acquisition Timestamp), 미들웨어 정렬(Middleware Alignment), 시간적 보정(Temporal Calibration), 지속적인 모니터링을 결합한다. 하드웨어 트리거와 하드웨어 타임스탬프는 정밀한 로컬 타이밍을 제공하고, PTP 또는 gPTP는 분산 장치의 시간을 조정하며, GNSS는 외부 기준 시간을 제공할 수 있고, 소프트웨어 동기화는 이기종 센서 스트림을 정렬한다. 이러한 메커니즘이 결합되어 신뢰할 수 있는 기록, 재생, 센서 융합, 지도 작성, 위치 추정, 인지, 피지컬 인공지능(Physical AI) 데이터 파이프라인에 필요한 공통 시간 기반(Common Time Foundation)을 형성한다.

## 06.05 Onboard Sensor Data Selective Storage Strategy [w/Code]

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

온보드 센서 데이터 저장(Onboard Sensor Data Storage)은 지속적인 센서 데이터 생성과 제한된 로봇 저장 용량 사이의 근본적인 불균형으로 인해 제약을 받는다. 카메라(Camera), 라이다(LiDAR), 깊이 센서(Depth Sensor), 관성 측정 장치(IMU), 진단 시스템(Diagnostic System)은 장시간 임무 수행 중 막대한 양의 데이터를 생성할 수 있다. 선택적 저장(Selective Storage)은 운영 가치와 향후 엔지니어링 가치에 따라 어떤 관측 데이터를 보존, 압축, 요약, 전송 또는 삭제할 것인지를 결정함으로써 이러한 문제를 해결한다.

선택적 저장(Selective Storage)의 목적은 단순히 데이터 용량을 최소화하는 것이 아니다. 지나치게 공격적인 삭제 정책(Deletion Policy)은 사고 분석(Incident Analysis), 알고리즘 디버깅(Algorithm Debugging), 인공지능 학습(AI Training), 시스템 검증(System Validation)에 필요한 정보를 제거할 수 있다. 반대로 모든 원시 관측 데이터(Raw Observation)를 저장하면 로컬 저장 공간을 소진하고 전송 비용을 증가시킬 수 있다. 따라서 실용적인 전략은 데이터 충실도(Data Fidelity), 저장 용량(Storage Capacity), 임무 시간(Mission Duration), 네트워크 가용성(Network Availability), 연산 자원(Computational Resource), 예상되는 후속 재사용(Downstream Reuse) 사이의 균형을 유지해야 한다.

센서 스트림(Sensor Stream)은 먼저 데이터 특성과 운영 중요도(Operational Importance)에 따라 분류되어야 한다. 고대역폭 카메라와 라이다 스트림은 일반적으로 관성 측정 장치, 오도메트리(Odometry), 진단 텔레메트리(Diagnostic Telemetry)보다 엄격한 저장 정책이 필요하다. 안전 관련 상태(Safety-Related State), 장애 이벤트(Fault Event), 제어 명령(Control Command), 위치 추정 결과(Localization Result), 동기화 메타데이터(Synchronization Metadata)는 상대적으로 적은 저장 공간을 사용하면서도 중요한 맥락을 제공할 수 있다. 따라서 저장 우선순위(Storage Priority)는 단순한 데이터 크기가 아니라 정보 가치(Information Value)를 반영해야 한다.

연속 원시 데이터 버퍼링(Continuous Raw Buffering)은 선택적 보존(Selective Retention)을 위한 효과적인 기반을 제공한다. 로봇은 입력되는 모든 센서 관측 데이터를 영구적으로 기록하는 대신 최근 수초 또는 수분의 원시 데이터를 포함하는 순환 버퍼(Rolling Buffer)를 유지할 수 있다. 정상 운용 중에는 오래된 데이터가 새로운 데이터로 덮어쓰기(Overwrite)되어 저장 공간 증가를 제한한다. 중요한 이벤트가 발생하면 선택된 시간 구간을 삭제 대상에서 보호하고 영구 저장 공간(Persistent Storage)에 기록하여 이후 분석에 활용할 수 있다.

이벤트 트리거 기록(Event-Triggered Recording)은 저장 결정을 로봇의 동작 및 시스템 상태와 연결함으로써 이러한 개념을 확장한다. 충돌 경고(Collision Warning), 비상 정지(Emergency Stop), 위치 추정 실패(Localization Failure), 인지 이상(Perception Anomaly), 내비게이션 오류(Navigation Error), 운영자 개입(Operator Intervention), 비정상 진동(Abnormal Vibration), 진단 장애(Diagnostic Fault)가 주변 센서 데이터의 보존을 트리거할 수 있다. 장애 원인이 시스템에서 이벤트로 인식되기 이전에 발생할 수 있으므로 이벤트 이전 구간(Pre-Event Window)과 이벤트 이후 구간(Post-Event Window)을 함께 저장하는 것이 중요하다.

보존되는 모든 데이터 구간에 동일한 품질이 필요한 것은 아니다. 저장 프로파일(Storage Profile)은 목적에 따라 서로 다른 충실도 수준(Fidelity Level)을 정의할 수 있다. 중요한 사고 구간은 원시 또는 준무손실(Near-Lossless) 이미지와 고밀도 포인트 클라우드(Dense Point Cloud)를 보존할 수 있지만, 일상적인 내비게이션은 압축 비디오(Compressed Video), 다운샘플링된 포인트 클라우드(Downsampled Point Cloud), 요약 텔레메트리(Summarized Telemetry)를 저장할 수 있다. 장기 운영 이력(Long-Term Operational History)은 전체 고주파 센서 스트림 대신 핵심 프레임(Key Frame), 궤적(Trajectory), 통계(Statistics), 이벤트(Event), 상태 지표(Health Indicator)만 포함할 수 있다.

샘플링(Sampling)과 다운샘플링(Downsampling)은 저장 공간 요구량을 줄이기 위한 추가적인 방법을 제공한다. 초당 30프레임으로 동작하는 카메라는 특정 모니터링 작업에서 모든 프레임을 저장할 필요가 없을 수 있으며, 고밀도 라이다 스캔도 공간적 또는 시간적으로 축소할 수 있다. 고주파 움직임 재구성(High-Frequency Motion Reconstruction)이 필요하지 않은 장기 분석에서는 관성 측정 장치 스트림도 요약할 수 있다. 그러나 샘플링 정책은 목표로 하는 후속 애플리케이션에 필요한 충분한 시간적·공간적 해상도를 유지해야 한다.

압축(Compression)은 선택적 저장을 대체하는 것이 아니라 보완해야 한다. H.265는 카메라 비디오 데이터의 용량을 줄일 수 있고, 드라코(Draco)와 같은 기하학 중심 압축 기술(Geometry-Oriented Compression)은 포인트 클라우드의 표현 크기를 줄일 수 있다. 압축을 사용하면 동일한 저장 용량에서 더 많은 관측 데이터를 보존할 수 있지만 압축된 데이터 역시 지속적으로 누적된다. 선택적 보존은 어떤 시간 구간을 저장할 가치가 있는지 결정하고, 압축은 선택된 구간을 얼마나 효율적으로 표현할 것인지를 결정한다.

다계층 저장(Multi-Tier Storage)은 데이터의 긴급성과 수명에 따라 저장 위치를 분리할 수 있다. 고속 온보드 SSD 또는 NVMe 저장 장치는 임시 원시 데이터 버퍼(Temporary Raw Buffer)와 활성 임무 기록(Active Mission Recording)을 지원하고, 장기간 보존할 선택 데이터는 보조 온보드 저장 장치, 네트워크 결합 스토리지(NAS), 엣지 서버(Edge Server), 클라우드 인프라(Cloud Infrastructure)로 이동할 수 있다. 이러한 계층 구조는 고속 데이터 획득 작업이 느린 아카이브 시스템(Archival System)의 성능에 의해 제한되는 것을 방지하고 데이터가 보존 가치에 따라 이동하도록 한다.

저장 결정(Storage Decision)은 네트워크 연결 상태(Network Connectivity)도 고려해야 한다. 고대역폭 와이파이(Wi-Fi) 또는 이더넷(Ethernet)을 사용할 수 있을 때는 선택된 데이터셋을 엣지 또는 중앙 저장소(Central Storage)로 빠르게 전송하여 온보드 저장 공간을 확보할 수 있다. 제한된 셀룰러 연결(Cellular Connection) 환경에서는 메타데이터, 썸네일(Thumbnail), 이벤트 또는 중요 데이터 구간만 업로드하고 더 큰 페이로드(Payload)는 로컬에 유지할 수 있다. 지연 동기화(Deferred Synchronization)를 사용하면 적절한 네트워크가 उपलब्ध해졌을 때 남은 데이터를 전송할 수 있다.

저장 예산(Storage Budget)은 데이터 보존을 정량적으로 제어하는 방법을 제공한다. 시스템은 센서 유형, 임무, 데이터 등급(Data Class), 우선순위별로 저장 용량을 할당하고 남은 공간을 지속적으로 모니터링할 수 있다. 사용 가능한 용량이 감소하면 정책에 따라 압축률을 높이고, 샘플링 비율을 낮추고, 원시 데이터 버퍼 유지 시간을 단축하거나, 우선순위가 낮은 데이터를 삭제할 수 있다. 중요한 안전 및 사고 기록(Critical Safety and Incident Record)은 정의된 보존 규칙(Retention Rule)에 따라 자동 삭제 대상에서 보호되어야 한다.

선택적 저장은 의도적인 데이터 공백(Intentional Data Gap)과 여러 품질 수준을 포함하는 데이터셋을 생성하기 때문에 메타데이터(Metadata)가 필수적이다. 각각의 보존 구간에는 로봇, 센서, 임무, 타임스탬프(Timestamp), 좌표 프레임(Coordinate Frame), 보정 버전(Calibration Version), 압축 프로파일(Compression Profile), 선택 이유(Selection Reason), 보존 등급(Retention Class)이 기록되어야 한다. 이벤트 기반으로 저장된 구간은 트리거 조건(Triggering Condition)도 기록해야 한다. 이러한 맥락이 없으면 향후 사용자가 의도적인 데이터 누락을 센서 장애로 잘못 해석할 수 있다.

센서 데이터의 일부만 보존하는 경우에도 동기화 관계(Synchronization Relationship)는 유지되어야 한다. 대응되는 좌표 변환(Transform), 관성 측정 장치 측정값, 타이밍 정보(Timing Information) 없이 카메라 시퀀스만 저장하면 위치 추정이나 센서 융합에 활용하기 어려울 수 있다. 따라서 필요한 경우 선택 정책은 논리적으로 연결된 다중 모달 데이터 그룹(Logically Related Multimodal Group)을 하나의 단위로 처리하여 중요한 이벤트 주변의 로봇 상태를 재구성하는 데 필요한 보조 데이터를 함께 보존해야 한다.

ROS 2 백(ROS 2 Bag)과 MCAP은 선택된 토픽(Topic)과 시간 구간을 구조화된 데이터셋(Structured Dataset)으로 구성하여 선택적 기록(Selective Recording)을 지원할 수 있다. 하나의 임무를 분할할 수 없는 단일 기록으로 취급하는 대신 운영 단계(Operational Phase), 이벤트, 보존 등급과 연결된 여러 세그먼트(Segment)를 생성할 수 있다. 이러한 세그먼트는 이후 독립적으로 인덱싱(Indexing), 재생(Playback), 전송(Transfer), 삭제할 수 있어 메시지 스키마(Message Schema)와 분석에 필요한 시간적 관계를 유지하면서 수명주기 관리(Lifecycle Management)를 개선할 수 있다.

인공지능 학습(AI Training)은 저장 가치(Storage Value)에 또 다른 차원을 제공한다. 일상적인 운영 데이터에는 반복적인 정보가 대량으로 포함될 수 있지만, 비정상적인 객체(Unusual Object), 어려운 조명 조건(Difficult Lighting), 위치 추정 불확실성(Localization Uncertainty), 충돌 직전 상황(Near-Collision Situation), 인지 결과 불일치(Perception Disagreement)는 상대적으로 높은 학습 가치를 제공할 수 있다. 선택 메커니즘(Selection Mechanism)은 이러한 정보 가치가 높은 관측값(Informative Observation)을 식별하고 더 높은 충실도로 보존하여 운영 중인 로봇을 목표 지향적 학습 데이터(Targeted Learning Data)의 분산 수집원으로 활용할 수 있다.

품질 인식 선택(Quality-Aware Selection)은 사용할 수 없는 관측 데이터가 저장 자원을 소비하는 것을 방지할 수 있다. 심각한 이미지 손상(Image Corruption), 잘못된 라이다 패킷(Invalid LiDAR Packet), 타임스탬프 누락(Missing Timestamp), 동기화 실패(Synchronization Failure), 센서 포화(Sensor Saturation)는 기록 데이터의 가치를 감소시킬 수 있다. 그러나 비정상 데이터는 하드웨어 또는 소프트웨어 장애를 나타낼 수도 있기 때문에 항상 삭제해서는 안 된다. 시스템은 의미 없는 데이터 손상과 진단 가치가 있는 품질 저하(Diagnostically Valuable Degradation)를 구분하고 보존되는 데이터 구간에 해당 상태를 표시해야 한다.

보존 정책(Retention Policy)은 수집 이후 저장 데이터가 어떻게 변화하는지를 결정한다. 임시 원시 데이터 버퍼는 수분 또는 수시간만 유지될 수 있고, 운영 기록은 수일 또는 수주 동안 유지될 수 있으며, 검증된 사고 데이터셋이나 인공지능 데이터셋은 훨씬 장기간 보존될 수 있다. 데이터의 가치가 명확해짐에 따라 서로 다른 등급 사이에서 전환할 수도 있다. 자동화된 수명주기 규칙(Automated Lifecycle Rule)은 데이터의 나이, 목적, 규제 요구사항(Regulatory Requirement), 엔지니어링 중요도에 따라 정보를 압축, 아카이브(Archive), 전송, 익명화(Anonymization), 삭제할 수 있다.

개인정보 보호(Privacy)와 보안(Security)은 불필요한 민감 데이터(Sensitive Data)가 영구적으로 저장되기 이전부터 선택 과정에 영향을 주어야 한다. 카메라, 오디오(Audio), 위치(Location), 환경 관측(Environmental Observation)에는 개인 정보나 기밀 정보(Confidential Information)가 포함될 수 있다. 로봇은 운영 또는 엔지니어링 요구사항에 의해 정당화되는 데이터만 보존하고 각 데이터 등급에 적합한 접근 제어(Access Control), 암호화(Encryption), 익명화, 삭제 정책을 적용해야 한다. 불필요한 데이터 보존을 최소화하면 개인정보 노출 위험과 사이버보안 영향(Cybersecurity Impact)을 동시에 줄일 수 있다.

강건한 온보드 선택적 저장 아키텍처(Robust Onboard Selective Storage Architecture)는 순환 원시 데이터 버퍼(Rolling Raw Buffer), 이벤트 기반 보존(Event-Triggered Preservation), 우선순위 등급(Priority Class), 적응형 샘플링(Adaptive Sampling), 압축, 저장 계층(Storage Tier), 메타데이터, 동기화 인식(Synchronization Awareness), 수명주기 정책을 결합한다. 모든 센서 관측값을 동일한 가치로 취급하는 대신 로봇은 어떤 데이터를 어느 수준의 충실도로 유지해야 하는지를 지속적으로 평가한다. 이를 통해 제한된 온보드 저장 용량을 운영, 디버깅, 검증, 인공지능 학습, 장기적인 피지컬 인공지능(Physical AI) 개선을 지원하는 관리 가능한 데이터 자원(Managed Data Resource)으로 전환할 수 있다.

## 06.06 Edge-to-Cloud Sensor Data Upload Pipeline [w/Code]

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}

엣지-클라우드 센서 데이터 업로드 파이프라인(Edge-to-Cloud Sensor Data Upload Pipeline)은 로봇 측 데이터 획득(Robot-Side Data Acquisition)을 중앙 집중형 저장소(Centralized Storage), 분석(Analytics), 인공지능 개발 환경(AI Development Environment)과 연결한다. 로봇은 이미지, 비디오, 포인트 클라우드(Point Cloud), 관성 측정 장치(IMU) 측정값, 텔레메트리(Telemetry), 진단 데이터(Diagnostics), 운영 이벤트(Operational Event)를 지속적으로 생성하지만 네트워크 용량만으로 모든 원시 관측 데이터를 즉시 전송하기는 어렵다. 따라서 이 파이프라인은 어떤 데이터를 로봇 외부로 전송할지, 언제 전송할지, 어떻게 패키징할지, 어디에 저장할지를 제어한다.

업로드 프로세스(Upload Process)는 일반적으로 센서 데이터 획득, 동기화(Synchronization), 기록(Recording), 로컬 선택(Local Selection)을 통해 활용 가능한 온보드 데이터셋(Onboard Dataset)이 구성된 이후 시작된다. ROS 2 백(ROS 2 Bag) 또는 MCAP 기록에는 동기화된 다중 모달 스트림(Multimodal Stream)이 포함될 수 있으며, 별도의 파일에는 압축된 H.265 비디오, 포인트 클라우드, 로그(Log), 파생 메타데이터(Derived Metadata)가 포함될 수 있다. 업로드 계층(Upload Layer)은 클라우드 측 시스템이 서로 관련 없는 개별 파일만 받는 것이 아니라 전체 임무 맥락(Mission Context)을 재구성할 수 있도록 이러한 데이터 간 관계를 유지해야 한다.

온보드 스테이징(Onboard Staging)은 실시간 로봇 운용과 네트워크 전송 사이에 제어 가능한 경계(Controlled Boundary)를 제공한다. 선택된 센서 데이터 구간은 스테이징 영역(Staging Area)으로 이동하여 인덱싱(Indexing), 검증(Validation), 압축(Compression), 암호화(Encryption), 업로드 준비 과정을 거칠 수 있다. 이를 통해 네트워크 작업이 센서 데이터 획득이나 제어 워크로드(Control Workload)에 직접적인 영향을 미치는 것을 방지한다. 또한 외부 네트워크 연결이 일시적으로 불가능하거나 불안정하더라도 로봇이 지속적으로 데이터를 수집할 수 있도록 한다.

데이터 패키징(Data Packaging)은 전체 임무 데이터를 하나의 매우 큰 객체(Object)로 처리하는 대신 관리 가능한 전송 단위(Transfer Unit)를 생성해야 한다. 기록 데이터는 시간 구간, 이벤트, 임무 단계(Mission Phase), 센서 그룹(Sensor Group), 저장 등급(Storage Class)을 기준으로 분할할 수 있다. 작은 세그먼트(Segment)는 재시도(Retry), 병렬 전송(Parallel Transfer), 무결성 검증(Integrity Verification), 선택적 우선순위 지정(Selective Prioritization)을 단순화한다. 그러나 분할 과정에서도 개별 조각을 일관된 데이터셋으로 재조립할 수 있도록 타임스탬프(Timestamp), 스키마(Schema), 보정 참조(Calibration Reference), 식별자(Identifier)를 유지해야 한다.

업로드 우선순위(Upload Priority)는 운영 가치(Operational Value)를 반영해야 한다. 안전 사고(Safety Incident), 심각한 장애(Critical Failure), 진단 이벤트(Diagnostic Event), 가치가 높은 인공지능 샘플(High-Value AI Sample)은 즉시 전송할 수 있지만 일상적인 센서 기록은 비용이 낮거나 대역폭이 높은 연결이 확보될 때까지 대기할 수 있다. 메타데이터와 작은 이벤트 요약(Event Summary)을 대용량 페이로드보다 먼저 업로드하면 중앙 시스템이 중요한 이벤트를 빠르게 발견하고 상세 분석이 필요한 경우 추가 센서 데이터 구간을 요청할 수 있다.

네트워크 인식 전송(Network-Aware Transfer)은 로봇이 이더넷(Ethernet), 와이파이(Wi-Fi), 프라이빗 5G(Private 5G), 공용 셀룰러 네트워크(Public Cellular Network), 또는 연결이 없는 환경 사이를 이동할 수 있기 때문에 중요하다. 파이프라인은 사용 가능한 연결을 감지하고 대역폭(Bandwidth), 지연 시간(Latency), 신뢰성(Reliability), 비용(Cost)에 따라 전송 동작을 조정해야 한다. 고대역폭 연결에서는 대량 동기화(Bulk Synchronization)를 수행하고, 제한된 연결에서는 더 나은 네트워크가 확보될 때까지 중요 이벤트, 썸네일(Thumbnail), 메타데이터 또는 강하게 압축된 데이터만 전송할 수 있다.

저장 후 전달(Store-and-Forward) 방식은 불안정하거나 간헐적인 네트워크에서도 신뢰할 수 있는 데이터 이동을 가능하게 한다. 연결이 끊어지면 업로드 대상 데이터는 삭제되지 않고 온보드 큐(Onboard Queue)에 안전하게 보존된다. 통신이 복구되면 파이프라인은 우선순위와 보존 정책(Retention Policy)에 따라 전송을 재개한다. 이러한 방식은 무선 네트워크 범위가 일정하지 않은 환경에서 동작하는 실외 로봇(Outdoor Robot), 물류 시스템(Logistics System), 원격 검사 플랫폼(Remote Inspection Platform), 이동 로봇(Mobile Robot)에 특히 중요하다.

재개 가능한 업로드(Resumable Upload)는 네트워크가 중단되었을 때 대용량 센서 데이터셋의 전송을 처음부터 다시 시작하는 문제를 방지한다. 대용량 객체는 청크(Chunk) 또는 멀티파트 단위(Multipart Unit)로 분할하여 전송할 수 있으며 완료된 부분은 독립적으로 추적된다. 연결 장애가 발생하면 누락되었거나 완료되지 않은 부분만 다시 전송하면 된다. 이를 통해 다중 기가바이트 기록이나 장시간 카메라 및 라이다 데이터셋을 전송할 때 대역폭 낭비를 줄이고 신뢰성을 향상시킬 수 있다.

무결성 검증(Integrity Verification)은 클라우드 측 데이터가 엣지에서 준비된 원본과 동일한지를 확인한다. 전송 전에 체크섬(Checksum) 또는 암호학적 해시(Cryptographic Hash)를 생성하고 업로드 이후 결과와 비교할 수 있다. 객체 크기(Object Size), 세그먼트 수(Segment Count), 메타데이터 일관성(Metadata Consistency), 컨테이너 유효성(Container Validity)도 함께 확인할 수 있다. 네트워크가 성공 응답을 반환했다고 해서 자동으로 유효한 데이터셋으로 판단해서는 안 되며, 파이프라인은 단순한 전송 완료와 영구 저장소(Persistent Storage)에 대한 검증된 수집(Verified Ingestion)을 구분해야 한다.

압축(Compression)은 전송 데이터량을 줄이지만 기존 센서 데이터 정책(Sensor Data Policy)과 조정되어야 한다. H.265 비디오, 압축된 포인트 클라우드, 압축된 ROS 2 또는 MCAP 페이로드는 네트워크 사용량을 크게 줄일 수 있다. 이미 압축된 데이터를 다시 압축하는 것은 큰 효과 없이 엣지 연산 자원을 소비하거나 데이터 품질을 저하시킬 수 있다. 따라서 파이프라인은 인코딩 프로파일(Encoding Profile)을 이해하고 향후 엔지니어링 또는 인공지능 활용 가치를 손상시킬 수 있는 불필요한 변환을 피해야 한다.

보안(Security)은 데이터가 물리적인 로봇 환경을 벗어나 이동하는 동안 데이터를 보호해야 한다. 인증(Authentication)은 어떤 로봇 또는 엣지 노드(Edge Node)가 업로드할 수 있는지를 확인하고, 권한 부여(Authorization)는 해당 장치가 접근할 수 있는 저장 위치와 서비스를 결정한다. 전송 암호화(Transport Encryption)는 이동 중 데이터(Data in Transit)를 보호하고 저장 데이터 암호화(Encryption at Rest)는 클라우드에 저장된 복사본을 보호할 수 있다. 자격 증명(Credential)은 센서 페이로드와 독립적으로 관리하고 과거 데이터셋을 수정하지 않고도 교체할 수 있어야 한다.

업로드되는 각 객체는 데이터 출처(Provenance)를 유지하는 메타데이터를 포함하거나 참조해야 한다. 유용한 필드에는 로봇 식별 정보(Robot Identity), 임무 식별자(Mission Identifier), 센서 유형(Sensor Type), 기록 시간 구간(Recording Interval), 소프트웨어 버전(Software Version), 보정 버전(Calibration Version), 압축 프로파일(Compression Profile), 동기화 상태(Synchronization Status), 선택 이유(Selection Reason), 데이터 품질 상태(Data-Quality State)가 포함된다. 클라우드 측 카탈로그(Cloud-Side Catalog)는 이러한 메타데이터를 이용하여 대규모 센서 저장소를 검색 가능하게 만들고 원시, 압축, 파생, 사고 관련, 인공지능용 데이터셋을 구분할 수 있다.

클라우드 데이터 수집(Cloud Ingestion)은 영구 객체 저장소(Durable Object Storage)와 메타데이터 및 인덱싱 서비스(Metadata and Indexing Service)를 분리해야 한다. 대용량 비디오, 포인트 클라우드, 기록 파일은 확장 가능한 객체 저장소(Scalable Object Storage)에 적합하며 검색 가능한 메타데이터는 데이터베이스(Database), 카탈로그(Catalog), 레이크하우스 테이블(Lakehouse Table)에 유지할 수 있다. 이러한 분리를 통해 엔지니어와 인공지능 시스템은 먼저 작은 메타데이터 레코드를 검색하고 분석이나 학습에 필요한 경우에만 비용이 큰 센서 페이로드를 가져올 수 있다.

파이프라인은 데이터 목적에 따라 여러 대상 저장 계층(Destination Tier)을 지원해야 한다. 최근 운영 데이터는 즉각적인 디버깅이나 모니터링을 위해 우선 핫 스토리지(Hot Storage)에 저장하고, 검증된 데이터셋은 이후 비용이 낮은 웜 스토리지(Warm Storage) 또는 콜드 스토리지(Cold Storage)로 이동할 수 있다. 중요한 사고 데이터와 선별된 인공지능 데이터셋(Curated AI Dataset)은 서로 다른 보존 규칙을 적용할 수 있다. 수명주기 정책(Lifecycle Policy)은 데이터셋의 논리적 식별 정보나 출처를 변경하지 않으면서 저장 계층 간 이동을 자동화할 수 있다.

관측 가능성(Observability)은 로봇 플릿(Robot Fleet) 전체의 업로드 작업을 관리하는 데 필요하다. 시스템은 대기 중인 데이터 용량(Queued Data Volume), 업로드 처리량(Upload Throughput), 네트워크 사용률(Network Utilization), 재시도 횟수(Retry Count), 전송 지연(Transfer Latency), 실패 원인(Failure Reason), 사용 가능한 온보드 저장 공간, 클라우드 수집 상태(Cloud Ingestion Status)를 모니터링해야 한다. 플릿 수준 대시보드(Fleet-Level Dashboard)를 통해 과도한 데이터 적체(Backlog)가 발생하거나 전송 실패가 반복되는 로봇을 파악하여 온보드 저장 공간이 소진되기 전에 인프라 문제를 발견할 수 있다.

백프레셔 관리(Backpressure Management)는 네트워크의 한계를 온보드 저장 정책과 연결한다. 업로드 속도가 장시간 센서 데이터 생성 속도보다 느리면 로컬 큐(Local Queue)가 계속 증가한다. 이 경우 로봇은 사전에 정의된 정책에 따라 샘플링 비율을 낮추거나, 압축률을 높이거나, 우선순위가 낮은 전송을 연기하거나, 보존 기간을 단축하거나, 삭제 가능한 데이터를 제거할 수 있다. 중요한 기록은 계속 보호하면서 상대적으로 가치가 낮은 데이터가 자원 부족의 영향을 우선적으로 흡수하도록 해야 한다.

클라우드 측 검증(Cloud-Side Validation)은 엣지에 저장된 복사본을 삭제하기 전에 업로드된 데이터를 실제로 사용할 수 있는지 확인해야 한다. 시스템은 체크섬, 필수 메타데이터(Required Metadata), 예상 파일(Expected File), 기록 데이터의 판독 가능성(Recording Readability), 타임스탬프 범위(Timestamp Range), 스키마 호환성(Schema Compatibility)을 검증할 수 있다. 영구 저장과 검증이 확인된 이후에만 로컬 수명주기 규칙(Local Lifecycle Rule)이 업로드된 세그먼트를 안전하게 삭제 가능한 상태로 표시해야 한다. 이러한 2단계 동작(Two-Phase Behavior)은 가치 있는 센서 데이터의 유일한 정상 복사본을 손실할 위험을 줄인다.

엣지-클라우드 파이프라인은 후속 인공지능 및 분석 워크플로(Downstream AI and Analytics Workflow)의 진입점 역할도 한다. 업로드된 기록은 인덱싱, 추출(Extraction), 어노테이션(Annotation), 변환(Transformation)을 거쳐 학습 또는 평가 데이터셋(Training or Evaluation Dataset)으로 변환될 수 있다. 희귀 이벤트(Rare Event)와 어려운 인지 사례(Difficult Perception Case)는 데이터 큐레이션 파이프라인(Data Curation Pipeline)으로 전달하고 운영 텔레메트리는 플릿 분석(Fleet Analytics)에 활용할 수 있다. 모델 개발 결과는 별도의 배포 파이프라인(Deployment Pipeline)을 통해 다시 로봇으로 전달될 수 있으며, 이를 통해 지속적인 데이터-학습 순환 구조(Continuous Data-Learning Cycle)를 형성할 수 있다.

강건한 엣지-클라우드 센서 데이터 업로드 아키텍처(Robust Edge-to-Cloud Sensor Data Upload Architecture)는 온보드 스테이징, 우선순위 지정(Prioritization), 적응형 네트워킹(Adaptive Networking), 저장 후 전달 큐(Store-and-Forward Queue), 재개 가능한 전송(Resumable Transfer), 무결성 검증, 보안, 메타데이터, 클라우드 데이터 수집, 수명주기 관리, 관측 가능성을 결합한다. 단순한 파일 복사로 업로드를 처리하는 대신 센서 데이터를 로봇에서의 획득부터 중앙 집중형 저장, 분석, 인공지능 학습, 장기적인 피지컬 인공지능(Physical AI) 개선까지 추적 가능한 가치 자산(Traceable and Valuable Asset)으로 관리한다.

## 06.07 Sensor Data Quality Management: Missing / Outlier [w/Code]

![](images/image7.png){width="7.268055555555556in" height="7.268055555555556in"}

센서 데이터 품질 관리(Sensor Data Quality Management)는 로봇 시스템이 신뢰할 수 있는 관측 데이터와 누락(Missing), 손상(Corrupted), 지연(Delayed), 불일치(Inconsistent), 물리적으로 타당하지 않은 측정값(Physically Implausible Measurement)을 구분할 수 있도록 한다. 인지(Perception), 위치 추정(Localization), 지도 작성(Mapping), 제어(Control), 인공지능 학습(AI Training)은 센서 데이터에 의존하므로 품질 문제는 전체 로봇 소프트웨어 스택(Robot Software Stack)으로 전파될 수 있다. 따라서 강건한 아키텍처는 수신된 모든 메시지가 유효한 물리적 관측을 나타낸다고 가정하지 않고 데이터 품질을 지속적으로 평가한다.

누락 데이터(Missing Data)는 예상된 측정값이 생성, 전송, 수신 또는 저장되지 않았을 때 발생한다. 원인에는 센서 장애(Sensor Failure), 네트워크 패킷 손실(Dropped Network Packet), 미들웨어 혼잡(Middleware Congestion), 기록 오류(Recording Error), 일시적인 연결 중단, 연산 과부하(Computational Overload)가 포함된다. 누락 정보는 메시지 부재, 타임스탬프 공백(Timestamp Gap), 불완전한 이미지 프레임, 손실된 라이다 패킷(LiDAR Packet), 누락된 필드 형태로 나타날 수 있다. 따라서 이를 감지하려면 예상 센서 주기, 메시지 구조, 시간적 연속성(Temporal Continuity)에 대한 정보가 필요하다.

타임스탬프 분석(Timestamp Analysis)은 누락된 측정값을 탐지하는 가장 간단한 방법 중 하나이다. 카메라가 초당 30프레임으로 동작하도록 설정되었다면 연속된 프레임은 대략 예측 가능한 시간 간격으로 도착해야 한다. 라이다, 관성 측정 장치(IMU), 오도메트리(Odometry), 위성항법시스템(GNSS), 텔레메트리(Telemetry) 스트림에도 유사한 기준을 정의할 수 있다. 도착 간 시간(Inter-Arrival Time), 시퀀스 번호(Sequence Number), 타임스탬프 연속성을 모니터링하면 정상적인 시간 변동과 비정상적인 데이터 손실을 구분할 수 있다.

이상치(Outlier)는 예상되는 물리적 또는 통계적 동작에서 크게 벗어나는 측정값이다. 관성 측정 장치가 갑자기 비현실적인 가속도를 보고하거나, 위성항법시스템 위치가 수 미터 순간 이동하거나, 깊이 센서(Depth Sensor)가 사용 가능한 범위를 벗어난 값을 생성할 수 있다. 이상치가 항상 하드웨어 장애를 의미하는 것은 아니며 반사(Reflection), 가림(Occlusion), 진동(Vibration), 전자기 간섭(Electromagnetic Interference), 환경 조건 또는 실제로 발생한 비정상 이벤트도 극단적인 측정값을 생성할 수 있다.

범위 검증(Range Validation)은 측정값이 물리적 또는 운영적으로 의미 있는 범위 안에 있는지를 확인한다. 센서 사양(Sensor Specification)은 기본적인 최소값과 최대값을 제공할 수 있으며, 로봇별 제약 조건(Robot-Specific Constraint)을 이용하여 더 좁은 운영 범위를 정의할 수도 있다. 휠 속도, 배터리 전압, 온도, 가속도, 깊이, 거리 측정값 등을 이러한 방식으로 검사할 수 있다. 범위를 벗어난 값은 애플리케이션에 따라 거부, 제한(Clipping), 표시(Flagging)하거나 진단 증거(Diagnostic Evidence)로 보존할 수 있다.

통계적 방법(Statistical Method)은 이상치 탐지를 위한 또 다른 계층을 제공한다. 이동 평균(Moving Average), 이동 중앙값(Moving Median), 분산(Variance), 표준편차(Standard Deviation), 백분위 범위(Percentile Range), 강건 통계(Robust Statistics)를 이용하여 최근 센서 동작을 표현할 수 있다. 지역적 데이터 분포(Local Distribution)에서 크게 벗어나는 측정값은 의심 데이터(Suspicious Data)로 표시할 수 있다. 로봇의 운영 조건은 시간에 따라 변화하므로 하나의 전역 임계값(Global Threshold)이 적합하지 않은 동적 환경에서는 슬라이딩 윈도 방식(Sliding-Window Method)이 특히 유용하다.

시간적 일관성 검사(Temporal Consistency Check)는 연속된 관측값 사이의 변화가 물리적으로 타당한지를 평가한다. 로봇 자세(Robot Pose)는 대응하는 속도 없이 순간적으로 먼 거리로 이동해서는 안 되며, 온도나 배터리 상태도 일반적으로 제한된 변화율 내에서 변한다. 따라서 변화율 제약(Rate-of-Change Constraint)을 이용하면 급격한 스파이크(Spike), 불연속(Discontinuity), 고정된 값(Frozen Value)을 탐지할 수 있다. 이러한 검사는 개별 측정값 자체는 유효하지만 이전 관측값과 일관되지 않는 경우를 탐지할 수 있으므로 단순한 범위 검증을 보완한다.

센서 간 일관성(Cross-Sensor Consistency)은 서로 다른 모달리티(Modality) 사이의 중복 정보를 이용하여 단일 센서만으로 탐지하기 어려운 품질 문제를 발견한다. 휠 오도메트리(Wheel Odometry)는 비주얼 오도메트리(Visual Odometry) 또는 라이다 오도메트리(LiDAR Odometry)와 비교할 수 있으며, 위성항법시스템의 움직임은 관성 측정 장치 측정값과 비교할 수 있고, 깊이 관측값은 기하학적 예상값(Geometric Expectation)과 비교할 수 있다. 큰 불일치가 어떤 센서가 잘못되었는지를 자동으로 결정하지는 않지만 신뢰도를 낮추거나 추가 검증을 수행해야 한다는 중요한 신호를 제공한다.

고정 센서 탐지(Frozen Sensor Detection)는 장애가 발생한 센서가 겉보기에는 유효한 값을 계속 발행할 수 있기 때문에 중요하다. 동일한 이미지의 반복, 변화하지 않는 관성 측정 장치 값, 일정한 거리 측정값, 고정된 위치는 정상적인 메시지 주기 검사를 통과하면서도 더 이상 실제 환경을 나타내지 않을 수 있다. 따라서 품질 관리는 통신 활동(Communication Activity)뿐 아니라 정보 변화(Information Change)도 함께 모니터링하여 정상적으로 동작하는 데이터 채널과 실제로 새로운 관측값을 생성하는 센서를 구분해야 한다.

데이터 손상(Data Corruption)은 획득, 직렬화(Serialization), 전송, 저장 또는 디코딩(Decoding) 과정에서 발생할 수 있다. 이미지 페이로드(Image Payload)가 불완전하거나, 포인트 클라우드 레코드에 유효하지 않은 값이 포함되거나, 기록된 메시지가 스키마(Schema) 또는 체크섬(Checksum) 검증에 실패할 수 있다. 구조적 검증(Structural Validation)은 메시지 크기, 필수 필드, 인코딩(Encoding), 수치 유효성(Numeric Validity), 컨테이너 무결성(Container Integrity), 사용 가능한 경우 체크섬을 확인해야 한다. 이를 통해 잘못 구성된 데이터가 후속 처리나 장기 데이터셋에 조용히 유입되는 것을 방지할 수 있다.

품질 관리는 데이터를 단순히 삭제하는 대신 명시적인 상태(Explicit Status)를 부여해야 한다. 측정값은 유효(Valid), 품질 저하(Degraded), 의심(Suspicious), 누락(Missing), 손상(Corrupted), 사용 불가(Unavailable) 상태로 분류할 수 있으며 필요한 경우 신뢰도 값(Confidence Value)을 함께 제공할 수 있다. 품질 상태를 보존하면 후속 알고리즘이 상황에 맞는 대응을 결정할 수 있다. 지도 작성 시스템은 품질이 저하된 측정값을 거부할 수 있지만 사고 분석 파이프라인은 장애 자체가 중요한 진단 정보를 포함하기 때문에 해당 데이터를 의도적으로 보존할 수 있다.

누락 데이터 처리는 센서 모달리티와 애플리케이션에 따라 달라진다. 저주파 텔레메트리에서 발생하는 짧은 공백은 경우에 따라 보간(Interpolation)할 수 있지만, 고주파 관성 측정 장치 샘플의 누락은 상태 추정(State Estimation)에 큰 영향을 줄 수 있다. 이전 값을 재사용하는 방식은 천천히 변화하는 상태 신호에는 허용될 수 있지만 동적인 움직임 측정에서는 위험할 수 있다. 따라서 아키텍처는 센서 유형별로 보간, 대체(Substitution), 마스킹(Masking), 거부(Rejection), 누락값의 명시적 표현에 대한 정책을 정의해야 한다.

이상치 처리(Outlier Handling) 역시 모든 비정상 관측값을 자동으로 제거하는 방식은 피해야 한다. 필터링 기술(Filtering Technique)은 고립된 잡음(Isolated Noise)을 억제할 수 있지만 지나치게 강한 필터링은 충돌, 급격한 기동(Rapid Maneuver), 예상하지 못한 장애물과 같은 실제 이벤트를 제거할 수 있다. 시스템은 운영 알고리즘을 위한 데이터 정제(Data Cleaning)와 진단 및 인공지능 개발을 위한 데이터 보존을 구분해야 한다. 따라서 저장 용량과 안전 요구사항이 허용한다면 원시 관측값(Raw Observation), 품질 플래그(Quality Flag), 정제된 표현(Cleaned Representation)을 함께 유지할 수 있다.

동기화 품질(Synchronization Quality)은 전체 센서 데이터 품질의 일부이다. 개별적으로 유효한 카메라, 라이다, 관성 측정 장치 측정값도 타임스탬프가 서로 정렬되지 않으면 잘못된 센서 융합 결과를 생성할 수 있다. 품질 검사는 클록 오프셋(Clock Offset), 타임스탬프 불연속, 동기화 허용 오차(Synchronization Tolerance), 메시지 지연(Message Latency)을 모니터링해야 한다. 동기화 장애가 발생한 데이터 구간은 명확하게 표시하여 오프라인 데이터셋에서 시간적으로 불일치하는 관측값을 정확하게 정렬된 다중 모달 샘플로 잘못 처리하지 않도록 해야 한다.

보정 품질(Calibration Quality)도 고려해야 한다. 정상적으로 전송된 측정값이라도 체계적으로 잘못된 결과를 생성할 수 있기 때문이다. 카메라 내부 파라미터(Camera Intrinsics), 센서 외부 파라미터(Sensor Extrinsics), 라이다 정렬(LiDAR Alignment), 관성 측정 장치 바이어스(IMU Bias), 스케일 파라미터(Scale Parameter)는 기계적 충격이나 하드웨어 교체 이후 드리프트하거나 유효하지 않게 될 수 있다. 데이터 품질 시스템은 관측값을 보정 버전(Calibration Version)과 연결하고 보정 성능 저하(Calibration Degradation)의 증거를 탐지하여 기하학적 불일치가 단순한 센서 잡음으로 잘못 해석되지 않도록 해야 한다.

품질 지표(Quality Metric)는 센서 데이터와 함께 메타데이터(Metadata)로 기록되어야 한다. 유용한 지표에는 예상 및 실제 메시지 전송률(Message Rate), 누락 샘플 비율(Missing-Sample Ratio), 이상치 개수(Outlier Count), 지연 시간(Latency), 지터(Jitter), 동기화 오류(Synchronization Error), 데이터 손상 이벤트(Corruption Event), 신뢰도(Confidence), 검증 상태(Validation Status)가 포함된다. 집계된 품질 지표는 기록 구간이나 전체 임무의 데이터 품질을 나타내며 엔지니어가 디버깅, 검증, 어노테이션(Annotation), 인공지능 학습에 사용하기 전에 신뢰성을 기준으로 데이터셋을 검색할 수 있도록 한다.

실시간 모니터링(Real-Time Monitoring)을 통해 데이터 품질이 운영 요구 수준 아래로 떨어질 때 로봇이 대응할 수 있다. 경고 임계값(Warning Threshold)을 초과하면 시스템 중요도에 따라 진단 기능, 센서 재시작 절차(Sensor Restart Procedure), 중복 메커니즘(Redundancy Mechanism), 성능 저하 운용 모드(Degraded Operating Mode), 안전 정지(Safe-Stop)를 실행할 수 있다. 따라서 품질 관리는 데이터 엔지니어링(Data Engineering)을 런타임 안전(Runtime Safety) 및 신뢰성(Reliability)과 연결하며, 모든 품질 위반을 동일하게 처리하는 대신 영향을 받는 센서의 중요도에 따라 대응해야 한다.

오프라인 품질 검증(Offline Quality Validation)은 기록 데이터가 엣지 서버(Edge Server), 네트워크 결합 스토리지(NAS), 클라우드 저장소(Cloud Storage)에 도착한 이후 더욱 심층적인 분석을 수행한다. 더 많은 연산 자원이 필요한 알고리즘을 사용하여 장기 데이터 분포, 센서 간 관계, 중복 데이터(Duplicate Data), 시간적 공백, 보정 일관성(Calibration Consistency), 비정상 패턴(Unusual Pattern)을 분석할 수 있다. 이러한 결과는 데이터셋 메타데이터를 갱신하고 각 데이터 구간이 인공지능 학습, 시뮬레이션 재생(Simulation Replay), 성능 평가(Performance Evaluation), 사고 조사(Incident Investigation), 장기 아카이빙(Long-Term Archival)에 적합한지를 결정하는 데 활용할 수 있다.

강건한 센서 데이터 품질 아키텍처(Robust Sensor Data Quality Architecture)는 누락 데이터 탐지(Missing-Data Detection), 이상치 분석(Outlier Analysis), 범위 및 시간적 검증(Range and Temporal Validation), 센서 간 일관성, 데이터 손상 검사(Corruption Check), 동기화 모니터링(Synchronization Monitoring), 보정 인식(Calibration Awareness), 명시적인 품질 메타데이터(Quality Metadata)를 결합한다. 모든 이상 데이터를 조용히 수정하는 대신 원본 관측값과 검증 또는 정제된 데이터의 차이를 보존한다. 이를 통해 로봇 운용, 디버깅, 지도 작성, 검증, 인공지능 학습, 장기적인 피지컬 인공지능(Physical AI) 개선을 위한 신뢰할 수 있는 센서 데이터셋(Trustworthy Sensor Dataset)을 구축할 수 있다.

## 06.08 Sensor Data Annotation Pipeline Integration [w/Code]

![](images/image8.png){width="7.268055555555556in" height="7.268055555555556in"}

센서 데이터 어노테이션 파이프라인 통합(Sensor Data Annotation Pipeline Integration)은 원시 로봇 관측 데이터(Raw Robot Observation)를 인지 모델(Perception Model), 다중 모달 학습(Multimodal Learning), 평가(Evaluation), 피지컬 인공지능(Physical AI) 개발에 필요한 의미 정보(Semantic Information)와 연결한다. 이미지, 비디오, 포인트 클라우드(Point Cloud), 깊이 맵(Depth Map), 궤적(Trajectory), 동기화된 센서 스트림은 객체, 영역, 행동, 상태, 관계, 이벤트를 구조화된 라벨(Structured Label)로 표현할 때 활용 가치가 크게 증가한다. 따라서 어노테이션(Annotation)은 독립적인 오프라인 작업이 아니라 로봇 데이터 아키텍처(Robot Data Architecture)의 일부로 다루어야 한다.

어노테이션 파이프라인(Annotation Pipeline)은 로봇이 생성하는 모든 관측 데이터가 아니라 신중하게 선택된 센서 데이터에서 시작한다. 온보드 선택적 저장(Onboard Selective Storage)과 엣지-클라우드 업로드(Edge-to-Cloud Upload) 메커니즘은 사고, 희귀 상황(Rare Situation), 불확실한 예측(Uncertain Prediction), 비정상 객체(Unusual Object), 대표적인 운영 조건을 보존할 수 있다. 비용이 많이 드는 어노테이션을 시작하기 전에 데이터 품질 검증(Data Quality Validation)을 수행하여 손상되거나 불완전하거나 동기화되지 않았거나 사용할 수 없는 데이터 구간이 신뢰할 수 있는 학습 가치를 제공하지 못한 채 라벨링 자원을 소비하는 것을 방지해야 한다.

데이터셋 준비(Dataset Preparation)는 운영 기록(Operational Recording)을 어노테이션이 가능한 단위(Annotation-Ready Unit)로 변환한다. ROS 2 백(ROS 2 Bag) 또는 MCAP 기록은 인덱싱(Indexing)과 분할(Segmentation)을 통해 카메라 시퀀스(Camera Sequence), 라이다 프레임(LiDAR Frame), 동기화된 다중 모달 샘플(Synchronized Multimodal Sample), 이벤트 중심 클립(Event-Centered Clip)으로 구성할 수 있다. 각각의 단위에는 타임스탬프(Timestamp), 센서 식별 정보(Sensor Identity), 좌표 프레임(Coordinate Frame), 보정 파라미터(Calibration Parameter), 로봇 상태(Robot State), 임무 맥락(Mission Context)이 유지되어야 한다. 이를 통해 어노테이션 도구가 일관된 관측 데이터를 표시하고 서로 다른 모달리티 사이의 관계를 유지할 수 있다.

어노테이션 스키마(Annotation Schema)는 사람 또는 자동화 시스템이 어떤 정보를 생성해야 하는지를 정의한다. 카메라 데이터셋에는 경계 상자(Bounding Box), 분할 마스크(Segmentation Mask), 키포인트(Keypoint), 차선(Lane), 객체(Object), 속성(Attribute), 추적 식별자(Tracking Identifier)가 필요할 수 있다. 포인트 클라우드에는 3차원 경계 상자(3D Bounding Box), 의미 클래스(Semantic Class), 인스턴스 라벨(Instance Label), 기하학적 영역(Geometric Region)이 필요할 수 있다. 로봇 데이터셋은 기존 컴퓨터 비전 라벨링을 넘어 행동(Action), 작업 단계(Task Phase), 장애(Failure), 상호작용(Interaction), 환경 상태(Environmental State), 시간적 이벤트(Temporal Event)를 추가로 표현할 수 있다.

잘 설계된 분류 체계(Taxonomy)는 데이터셋 전체에 일관된 의미를 제공한다. 대규모 라벨링을 시작하기 전에 클래스 이름(Class Name), 계층적 관계(Hierarchical Relationship), 속성, 미확인 범주(Unknown Category), 제외 규칙(Exclusion Rule), 모호한 사례(Ambiguous Case)를 정의해야 한다. 안정적인 정의가 없으면 서로 다른 어노테이터(Annotator)가 동일한 관측 데이터를 다르게 해석할 수 있다. 따라서 분류 체계 버전(Taxonomy Version)을 추적하여 서로 다른 정의에 따라 생성된 라벨을 식별하고, 마이그레이션(Migration)하거나 올바르게 평가할 수 있어야 한다.

다중 모달 어노테이션(Multimodal Annotation)을 수행하는 동안 동기화(Synchronization) 및 보정(Calibration) 정보를 계속 사용할 수 있어야 한다. 카메라 프레임에 표시된 2차원 객체는 라이다 데이터의 포인트 또는 깊이 맵의 특정 영역과 대응할 수 있다. 정확한 타임스탬프와 센서 외부 파라미터(Sensor Extrinsics)를 사용하면 필요한 경우 서로 다른 좌표계(Coordinate System) 사이에서 라벨을 투영(Projection)할 수 있다. 동기화 또는 보정 품질이 불확실하다면 자동으로 전송된 어노테이션을 독립적으로 검증된 정답 데이터(Ground Truth)와 동일하게 취급하지 않고 별도의 상태로 표시해야 한다.

수동 어노테이션(Manual Annotation)은 의미 해석에 사람의 판단이 필요한 경우 여전히 중요하다. 어노테이터는 자동화 방법으로 신뢰성 있게 이해하기 어려운 객체, 가림(Occlusion), 비정상 이벤트, 상호작용, 모호한 장면을 검토할 수 있다. 어노테이션 인터페이스(Annotation Interface)는 인접 프레임(Neighboring Frame)이나 동기화된 다른 모달리티를 포함한 충분한 맥락 정보를 제공해야 한다. 개별 프레임만으로는 움직임, 객체 식별 정보(Object Identity), 이벤트 의미가 명확하지 않을 수 있지만 시간적 맥락(Temporal Context)을 함께 확인하면 이를 보다 정확하게 판단할 수 있다.

모델 지원 어노테이션(Model-Assisted Annotation)은 초기 예측값을 생성한 후 어노테이터가 이를 검토하고 수정하도록 하여 사람의 작업량을 줄일 수 있다. 기존 객체 탐지(Detection), 분할(Segmentation), 추적(Tracking), 파운데이션 모델(Foundation Model)을 사용하여 대규모 데이터셋을 사전 라벨링(Pre-Labeling)하면 사람은 불확실하거나 잘못된 영역에 집중할 수 있다. 생성된 라벨에는 모델, 사람 또는 두 방식의 결합 중 어떤 과정에서 생성되었는지를 기록해야 한다. 이후 품질 분석을 지원하기 위해 모델 버전(Model Version)과 신뢰도(Confidence)도 함께 보존해야 한다.

능동 학습(Active Learning)은 어노테이션을 수행했을 때 모델 개선 효과가 가장 클 것으로 예상되는 샘플에 우선순위를 부여할 수 있다. 반복적인 운영 데이터를 대량으로 라벨링하는 대신 낮은 신뢰도의 예측, 모델 간 불일치(Model Disagreement), 새로운 환경(Novel Environment), 희귀 클래스(Rare Class), 비정상적인 로봇 동작을 식별할 수 있다. 이러한 샘플은 높은 우선순위로 어노테이션 큐(Annotation Queue)에 전달할 수 있으며, 이를 통해 배포된 로봇, 데이터 선택, 어노테이션, 모델 학습, 향후 배포 사이에 피드백 루프(Feedback Loop)를 형성할 수 있다.

어노테이션 자체에서도 오류가 발생할 수 있기 때문에 품질 보증(Quality Assurance)이 필요하다. 라벨이 누락되거나 기하학적으로 부정확하거나 분류 체계와 일치하지 않거나 잘못된 클래스에 할당되거나 프레임 사이에서 시간적으로 일관되지 않을 수 있다. 자동 검증(Automated Validation)은 유효하지 않은 좌표, 불가능한 크기, 필수 필드 누락, 중복 식별자(Duplicate Identifier), 손상된 트랙(Broken Track)을 탐지할 수 있다. 이후 사람의 검토(Human Review)는 구조적인 검사만으로 해결하기 어려운 의미적 모호성과 복잡한 사례에 집중할 수 있다.

합의 및 검토 워크플로(Consensus and Review Workflow)는 가치가 높은 데이터셋의 신뢰성을 향상시킨다. 여러 어노테이터가 선택된 샘플을 독립적으로 라벨링한 후 불일치 정도를 측정하고 검토자(Reviewer) 또는 도메인 전문가(Domain Expert)가 이를 해결할 수 있다. 이러한 방식은 모호한 클래스, 안전 중요 이벤트(Safety-Critical Event), 라벨 오류가 성능 측정에 직접적인 영향을 미치는 평가 데이터셋(Evaluation Dataset)에 특히 유용하다. 검토 결과는 불명확한 어노테이션 지침(Annotation Guideline)을 발견하고 추가 라벨링 이전에 이를 개선하는 데도 활용할 수 있다.

어노테이션 출처 정보(Annotation Provenance)는 라벨과 함께 메타데이터(Metadata)로 저장해야 한다. 유용한 정보에는 데이터셋 버전(Dataset Version), 원본 기록(Source Recording), 어노테이션 스키마, 분류 체계 버전, 어노테이터 또는 어노테이션 프로세스(Annotation Process), 모델 버전, 생성 시간(Creation Time), 검토 상태(Review Status), 품질 점수(Quality Score), 수정 이력(Modification History)이 포함된다. 출처 정보를 통해 팀은 라벨이 어떤 과정에서 생성되었는지 확인하고 현재의 학습 또는 평가 요구사항과 여전히 호환되는지를 판단할 수 있다.

어노테이션이 변화함에 따라 버전 관리(Version Management)가 중요해진다. 라벨은 검토 이후 수정될 수 있고, 새로운 분류 체계로 변환되거나 추가적인 속성이 포함되거나 개선된 모델을 이용하여 다시 생성될 수 있다. 원본 센서 데이터(Original Sensor Data)는 안정적으로 유지하면서 어노테이션 버전은 별도로 관리해야 한다. 이러한 분리는 대용량 원시 센서 페이로드를 복제하지 않고도 이전 데이터셋을 이용한 실험을 재현하고 서로 다른 라벨링 전략에 따른 모델 동작을 비교할 수 있도록 한다.

어노테이션 출력(Annotation Output)은 후속 시스템에서 수동 변환 없이 사용할 수 있는 구조화된 형식(Structured Format)을 사용해야 한다. 이미지 라벨, 포인트 클라우드 어노테이션, 시간적 이벤트, 다중 모달 관계는 서로 다른 물리적 표현을 사용할 수 있지만 일관된 데이터셋 식별자와 메타데이터 규칙을 공유해야 한다. 변환 계층(Conversion Layer)은 표준 어노테이션 표현(Canonical Annotation Representation)을 유지하면서 객체 탐지, 분할, 추적, 3차원 인지(3D Perception), 모방 학습(Imitation Learning), 평가를 위한 작업별 형식(Task-Specific Format)을 생성할 수 있다.

센서 데이터에는 사람, 차량 식별 정보, 위치, 음성, 기밀 환경(Confidential Environment)이 포함될 수 있으므로 어노테이션 과정에서도 개인정보 보호(Privacy)와 보안 통제(Security Control)가 필요하다. 어노테이션 작업자와 자동화 서비스에는 작업 수행에 필요한 데이터만 제공해야 한다. 접근 제어(Access Control), 익명화(Anonymization), 얼굴 또는 번호판 흐림 처리(Face or License-Plate Blurring), 암호화(Encryption), 감사 기록(Audit Record)을 적용하면 합법적인 라벨링 목적에 필요한 정보를 유지하면서 불필요한 데이터 노출을 줄일 수 있다.

데이터 레이크하우스(Data Lakehouse) 또는 데이터셋 카탈로그(Dataset Catalog)와 통합하면 조직 전체에서 어노테이션 데이터를 쉽게 검색할 수 있다. 원시 센서 데이터는 객체 저장소(Object Storage)에 유지하고 어노테이션 메타데이터, 데이터셋 버전, 품질 지표(Quality Metric), 의미 인덱스(Semantic Index)는 검색 가능한 테이블이나 카탈로그에 등록할 수 있다. 엔지니어는 모델 개발에 필요한 대용량 센서 페이로드를 가져오기 전에 특정 로봇, 환경, 클래스, 이벤트, 품질 수준, 어노테이션 버전을 조건으로 필요한 데이터셋을 검색할 수 있다.

완성된 어노테이션 데이터셋(Annotated Dataset)은 학습(Training), 검증(Validation), 시뮬레이션(Simulation), 벤치마킹(Benchmarking), 오류 분석(Error Analysis)의 입력으로 사용된다. 이후 모델 결과를 어노테이션과 비교하여 거짓 양성(False Positive), 거짓 음성(False Negative), 불확실한 클래스, 어려운 환경 조건을 식별할 수 있다. 이러한 분석 결과는 다시 데이터 선택 단계(Data Selection Stage)로 전달되어 추가 사례를 수집하거나 우선적으로 처리하도록 할 수 있다. 따라서 어노테이션은 라벨 생성으로 종료되는 것이 아니라 지속적인 데이터-모델 피드백 순환(Continuous Data-Model Feedback Cycle)에 참여한다.

강건한 센서 데이터 어노테이션 아키텍처(Robust Sensor Data Annotation Architecture)는 데이터 선택(Data Selection), 품질 검증, 동기화, 보정, 데이터셋 준비, 스키마 설계(Schema Design), 사람 기반 라벨링(Human Labeling), 모델 지원(Model Assistance), 능동 학습, 품질 보증, 출처 추적(Provenance), 버전 관리, 보안, 데이터셋 카탈로그를 통합한다. 원본 로봇 관측 데이터에서 검증된 의미 라벨(Validated Semantic Label)까지의 추적 가능성(Traceability)을 유지함으로써 운영 센서 데이터를 인지, 로봇 지능(Robotics Intelligence), 평가, 지속적인 피지컬 인공지능 개선에 활용할 수 있는 재사용 가능한 지식 자산(Reusable Knowledge Asset)으로 변환한다.

## 06.09 Multi-Modal Sensor Data Storage Design

![](images/image9.png){width="7.268055555555556in" height="7.268055555555556in"}

다중 모달 센서 데이터 저장(Multimodal Sensor Data Storage)은 로봇이 생성하는 이기종 관측 데이터(Heterogeneous Observation)를 각각 독립적인 파일 집합으로 취급하는 대신 서로 간의 관계를 보존해야 한다. 카메라, 라이다(LiDAR), 깊이 센서(Depth Sensor), 관성 측정 장치(IMU), 위성항법시스템(GNSS), 오도메트리(Odometry), 오디오(Audio), 진단 데이터(Diagnostics), 로봇 상태 스트림(Robot-State Stream)은 데이터 크기, 주기, 구조, 접근 패턴(Access Pattern)이 크게 다르다. 저장 아키텍처(Storage Architecture)는 이러한 차이를 수용하면서도 동일한 물리적 임무(Physical Mission)를 나타내는 일관된 표현을 유지해야 한다.

핵심 설계 원칙은 물리적 저장 형식(Physical Storage Format)과 논리적 데이터셋 식별 정보(Logical Dataset Identity)를 분리하는 것이다. 카메라 비디오는 H.265 형식으로, 포인트 클라우드(Point Cloud)는 압축 바이너리(Compressed Binary) 또는 기하학 중심 형식(Geometry-Oriented Format)으로, 고주파 상태 데이터는 ROS 2 백(ROS 2 Bag) 또는 MCAP 기록 내부에 저장할 수 있다. 이러한 데이터는 물리적으로 서로 다른 형식을 유지하면서도 공유 식별자(Shared Identifier), 타임스탬프(Timestamp), 메타데이터(Metadata), 보정 참조(Calibration Reference), 좌표 프레임 정의(Coordinate-Frame Definition)를 통해 하나의 논리적 임무 데이터셋(Logical Mission Dataset)에 포함될 수 있다.

시간(Time)은 다중 모달 관측 데이터를 연결하는 기본 축(Primary Axis)을 제공한다. 저장되는 모든 센서 샘플은 공통 로봇 시간 영역(Common Robot Time Domain)과 연결할 수 있는 획득 타임스탬프(Acquisition Timestamp)를 유지해야 한다. 이를 통해 카메라 프레임, 라이다 스캔, 관성 측정 장치 샘플, 자세(Pose), 제어 상태(Control State)를 동일한 시간 구간을 기준으로 검색할 수 있다. 저장 시스템은 물리적 획득 시점과 크게 다를 수 있는 파일 생성 시간(File Creation Time), 업로드 순서(Upload Order), 데이터베이스 삽입 시간(Database Insertion Time)에만 의존하지 않고 원본 타임스탬프를 보존해야 한다.

다중 모달 관측 데이터는 서로 다른 위치와 방향으로 장착된 센서에서 생성되므로 공간적 관계(Spatial Relationship)도 동일하게 중요하다. 따라서 좌표 프레임(Coordinate Frame), 센서 외부 파라미터(Sensor Extrinsics), 카메라 내부 파라미터(Camera Intrinsics), 보정 버전(Calibration Version), 좌표 변환 이력(Transform History)이 저장 데이터와 계속 연결되어 있어야 한다. 이러한 참조 정보가 없으면 개별적으로 유효한 이미지와 포인트 클라우드도 이후 센서 융합(Sensor Fusion), 지도 작성(Mapping), 위치 추정(Localization), 3차원 재구성(3D Reconstruction), 다중 모달 인공지능 학습(Multimodal AI Training)에 결합하기 어려워질 수 있다.

임무 중심 계층 구조(Mission-Oriented Hierarchy)는 실용적인 데이터 구성 모델을 제공한다. 데이터는 전역 고유 식별자(Globally Unique Identifier)를 유지하면서 로봇, 플릿(Fleet), 임무(Mission), 세션(Session), 시간 세그먼트(Time Segment), 센서, 데이터 산출물(Data Product)을 기준으로 그룹화할 수 있다. 이러한 계층 구조는 사람이 데이터를 탐색하기 쉽게 만들면서 후속 애플리케이션이 디렉터리 이름에만 의존하는 것을 방지한다. 데이터가 서로 다른 저장 시스템 사이를 이동하더라도 검색 가능성을 유지하려면 메타데이터 카탈로그(Metadata Catalog)가 논리적 관계를 정의하는 기준 정보가 되어야 한다.

대용량 바이너리 센서 페이로드(Large Binary Sensor Payload)는 일반적인 관계형 테이블(Relational Table)보다 객체 저장소(Object Storage) 또는 파일 저장소(File Storage)에 적합하다. 비디오, 이미지, 포인트 클라우드, ROS 2 백 파일, MCAP 기록, 대용량 깊이 시퀀스(Depth Sequence)는 불변 객체(Immutable Object) 또는 버전 관리 객체(Versioned Object)로 저장할 수 있다. 데이터베이스(Database)와 레이크하우스 테이블(Lakehouse Table)은 작은 크기의 메타데이터, 인덱스(Index), 품질 지표(Quality Metric), 이벤트 설명(Event Description), 객체 참조(Object Reference)를 별도로 유지하여 대용량 바이너리 페이로드를 반복적으로 검색하지 않고도 질의를 수행할 수 있도록 한다.

시계열 데이터베이스(Time-Series Database)는 고주파 수치 스트림(High-Frequency Numeric Stream)과 집계된 운영 텔레메트리(Aggregated Operational Telemetry)를 관리하기 위해 객체 저장소를 보완할 수 있다. 관성 측정 장치 요약값, 배터리 측정값, 온도, 지연 지표(Latency Metric), 진단 상태(Diagnostic State), 로봇 상태 지표(Robot Health Indicator)는 타임스탬프 기반 질의와 다운샘플링(Downsampling)의 이점을 활용할 수 있다. 아키텍처는 모든 모달리티를 하나의 데이터베이스 기술에 강제로 저장할 필요가 없으며, 서로 다른 저장 엔진(Storage Engine)이 일관된 식별자와 메타데이터를 통해 하나의 논리적 데이터셋에 참여하도록 구성할 수 있다.

ROS 2 백과 MCAP은 메시지 스키마(Message Schema), 토픽(Topic), 타임스탬프, 재생 가능한 로봇 통신(Replayable Robot Communication)을 보존하는 유용한 컨테이너(Container)를 제공한다. 특히 엔지니어가 시스템 동작을 재구성하거나 기록된 메시지를 이용하여 알고리즘을 다시 실행해야 하는 경우 유용하다. 그러나 장기 저장소(Long-Term Repository)에서는 선택된 이미지, 포인트 클라우드, 텔레메트리 또는 메타데이터를 특화된 저장 형식으로 추출할 수도 있다. 원본 기록과 추출된 데이터 산출물은 출처 정보(Provenance Information)를 통해 계속 연결되어야 한다.

세그먼트 분할(Segmentation)은 장시간 로봇 임무가 관리하기 어려운 하나의 거대한 저장 객체가 되는 것을 방지한다. 기록 데이터는 시간 구간, 운영 단계(Operational Phase), 이벤트(Event), 저장 등급(Storage Class)을 기준으로 제한된 크기의 세그먼트로 분할하면서 세그먼트 사이의 연속성 메타데이터(Continuity Metadata)를 유지할 수 있다. 작은 단위는 업로드, 재시도(Retry), 인덱싱(Indexing), 병렬 처리(Parallel Processing), 선택적 삭제(Selective Deletion), 부분 검색(Partial Retrieval)을 효율적으로 만든다. 다만 세그먼트 경계가 중요한 이벤트나 동기화된 다중 모달 시간 구간을 재구성하는 데 필요한 시간적 맥락을 손상시켜서는 안 된다.

특정 이벤트를 찾기 위해 전체 기록을 검색하는 것은 비효율적이므로 인덱스(Index)가 필수적이다. 시간 인덱스(Temporal Index)는 요청된 시간 범위에 포함되는 관측 데이터를 찾을 수 있으며 센서, 임무, 이벤트, 품질, 지리 정보(Geographic Information), 의미 정보(Semantic Information)를 이용한 인덱스를 통해 검색 범위를 더욱 축소할 수 있다. 엔지니어는 먼저 메타데이터를 통해 필요한 데이터를 식별하고 이후에 대용량 페이로드를 가져올 수 있어야 한다. 이러한 구조는 로봇 플릿이 수백만 개의 파일과 장시간 기록 데이터를 축적할수록 더욱 중요해진다.

다중 모달 저장은 누락되거나 사용할 수 없는 모달리티(Missing or Unavailable Modality)를 명시적으로 표현해야 한다. 하나의 데이터셋 세그먼트에 카메라와 관성 측정 장치 데이터는 존재하지만 센서 장애나 선택적 저장 결정으로 인해 라이다 데이터가 없을 수 있다. 메타데이터는 의도적인 생략(Intentional Omission)을 패킷 손실(Packet Loss), 데이터 손상(Data Corruption), 사용할 수 없는 하드웨어(Unavailable Hardware)와 구분해야 한다. 명시적인 모달리티 가용성(Modality Availability)은 후속 파이프라인이 모든 세그먼트에 동일한 센서 구성이 존재한다고 잘못 가정하는 것을 방지하고, 학습 시스템이 필요한 입력에 따라 데이터셋을 필터링할 수 있도록 한다.

품질 메타데이터(Quality Metadata)는 각 모달리티와 세그먼트에 계속 연결되어 있어야 한다. 누락 샘플 비율(Missing-Sample Ratio), 이상치 개수(Outlier Count), 동기화 오류(Synchronization Error), 보정 상태(Calibration Status), 프레임 손상(Frame Corruption), 지연 시간(Latency), 검증 결과(Validation Result)를 이용하여 특정 용도에 대한 데이터 적합성을 판단할 수 있다. 운영 디버깅(Operational Debugging)에 사용할 수 있는 데이터셋이라도 벤치마크 평가(Benchmark Evaluation)나 고품질 인공지능 학습에는 충분하지 않을 수 있다. 따라서 저장 설계는 단순한 저장 여부의 구분을 넘어 품질 기반 검색(Quality-Based Discovery)을 지원해야 한다.

파생 데이터(Derived Data)는 원본 관측 데이터(Original Observation)와 명확하게 구분되어야 한다. 보정된 이미지(Rectified Image), 필터링된 포인트 클라우드(Filtered Point Cloud), 모션 보상 스캔(Motion-Compensated Scan), 익명화된 비디오(Anonymized Video), 어노테이션(Annotation), 궤적, 특징(Feature), 모델 예측(Model Prediction)은 모두 동일한 원본 기록에서 생성될 수 있다. 이러한 데이터 산출물은 원본을 덮어쓰지 않고 출처 데이터와 처리 버전(Processing Version)을 참조해야 한다. 이러한 계보(Lineage)를 통해 재현성(Reproducibility)을 확보하고 처리 방법이 개선될 경우 새로운 알고리즘으로 파생 데이터를 다시 생성할 수 있다.

버전 관리(Versioning)는 보정, 어노테이션, 처리된 데이터셋에서 특히 중요하다. 기본 센서 페이로드는 변경되지 않더라도 보정 파라미터, 의미 라벨(Semantic Label), 품질 평가(Quality Assessment), 처리 결과는 계속 발전할 수 있다. 이러한 버전을 분리하면 대용량 바이너리 데이터의 불필요한 복제를 방지하고 과거 데이터셋 상태를 이용한 실험을 재현할 수 있다. 데이터셋 매니페스트(Dataset Manifest)는 특정 센서 객체, 메타데이터, 보정 버전, 어노테이션 버전을 하나의 재현 가능한 스냅샷(Reproducible Snapshot)으로 결합할 수 있다.

저장 계층(Storage Tier)은 데이터 접근 빈도(Access Frequency)와 데이터 가치(Data Value)를 모두 반영해야 한다. 고속 온보드 NVMe는 활성 기록(Active Recording)과 임시 원시 데이터 버퍼(Temporary Raw Buffer)를 저장하고, 엣지 또는 네트워크 결합 스토리지(NAS)는 최근 수집된 엔지니어링 데이터를 저장하며, 확장 가능한 객체 저장소(Scalable Object Storage)는 플릿 수준 저장소(Fleet-Level Repository)를 유지할 수 있다. 오래되었거나 자주 사용하지 않는 데이터셋은 비용이 낮은 아카이브 계층(Archival Tier)으로 이동할 수 있다. 물리적인 저장 위치가 변경되더라도 논리적 식별자와 카탈로그 항목(Catalog Entry)은 안정적으로 유지되어야 한다.

압축(Compression)과 선택적 보존(Selective Retention)은 다중 모달 관계를 고려하여 함께 조정해야 한다. 하나의 모달리티를 지나치게 압축하거나 이를 지원하는 다른 스트림을 삭제하면 가치 있는 데이터의 활용성이 감소할 수 있다. 비주얼-관성 연구(Visual-Inertial Research)를 위한 카메라 시퀀스에는 대응되는 관성 측정 장치와 자세 정보가 필요할 수 있으며, 라이다 지도 작성 데이터에는 좌표 변환과 보정 정보가 필요할 수 있다. 따라서 보존 정책(Retention Policy)은 개별 파일 크기만을 기준으로 결정하지 않고 데이터셋 사이의 의존 관계(Dataset Dependency)를 이해해야 한다.

클라우드와 엣지 저장소(Cloud and Edge Storage)는 물리적 인프라가 서로 다르더라도 일관된 데이터셋 모델(Consistent Dataset Model)을 공유해야 한다. 온보드 로봇은 먼저 동기화된 세그먼트를 로컬에 저장하고, 엣지 서버(Edge Server)는 여러 임무 데이터를 집계하며, 클라우드 저장소(Cloud Storage)는 플릿 규모의 장기 데이터셋을 구성할 수 있다. 업로드 프로세스(Upload Process)는 식별자, 체크섬(Checksum), 메타데이터, 출처 정보를 유지하여 저장 계층 사이의 이동이 데이터의 의미나 식별 정보를 변경하지 않고 물리적인 저장 위치만 변경하도록 해야 한다.

보안(Security)과 개인정보 보호 통제(Privacy Control)는 모달리티, 데이터셋, 저장 계층 수준에서 적용할 수 있다. 카메라, 오디오, 위치, 환경 데이터는 민감하지 않은 진단 데이터보다 강력한 접근 제한(Access Restriction)이 필요할 수 있다. 암호화(Encryption), 권한 부여(Authorization), 감사 기록(Audit Record), 익명화된 파생 데이터(Anonymized Derivative), 보존 규칙(Retention Rule)은 데이터셋 메타데이터와 계속 연결되어야 한다. 이를 통해 다중 모달 저장의 유용한 관계를 유지하면서 모든 사용자나 서비스가 모든 모달리티에 제한 없이 접근해야 하는 상황을 방지할 수 있다.

강건한 다중 모달 센서 데이터 저장 아키텍처(Robust Multimodal Sensor Data Storage Architecture)는 이기종 물리적 형식(Heterogeneous Physical Format)을 통합된 논리적 식별 정보(Unified Logical Identity), 공통 타임스탬프(Common Timestamp), 좌표 프레임, 보정, 메타데이터 카탈로그, 인덱싱, 품질 정보(Quality Information), 계보, 버전 관리, 계층형 저장(Tiered Storage)과 결합한다. 목표는 모든 센서 스트림을 하나의 기술에 저장하는 것이 아니라 서로 간의 관계를 보존하여 하나의 재구성 가능한 로봇 경험(Reconstructable Robot Experience)으로 만드는 것이다. 이를 통해 재생(Replay), 디버깅, 지도 작성, 분석(Analytics), 어노테이션, 인공지능 학습, 장기적인 피지컬 인공지능(Physical AI) 개발을 지원할 수 있다.

## 06.10 Privacy-Compliant Sensor Data Management: Face Blur

![](images/image10.png){width="7.268055555555556in" height="7.268055555555556in"}

개인정보 보호 준수 센서 데이터 관리(Privacy-Compliant Sensor Data Management)는 식별 가능한 사람이나 민감한 환경(Sensitive Environment)을 불필요하게 노출하지 않으면서 로봇 관측 데이터(Robot Observation)를 인지(Perception), 디버깅(Debugging), 분석(Analytics), 인공지능 개발(AI Development)에 활용할 수 있도록 보장한다. 특히 카메라는 일반적인 촬영 과정에서도 얼굴, 차량 번호판(License Plate), 직원 배지(Employee Badge), 화면, 문서, 사적 공간(Private Space)을 의도하지 않게 포함할 수 있기 때문에 중요하다. 따라서 개인정보 보호(Privacy Protection)는 데이터셋이 이미 배포된 이후에 추가하는 것이 아니라 센서 데이터 수명주기(Sensor-Data Lifecycle)에 통합되어야 한다.

첫 번째 원칙은 데이터 최소화(Data Minimization)이다. 로봇은 운영, 엔지니어링, 안전 또는 학습 목적에 필요한 센서 정보만 수집하고 보존해야 한다. 지속적인 기록(Continuous Recording)이 반드시 지속적인 장기 보존(Continuous Long-Term Retention)을 정당화하는 것은 아니다. 선택적 저장(Selective Storage), 이벤트 기반 기록(Event-Triggered Recording), 제한된 보존 기간(Limited Retention Period), 모달리티별 정책(Modality-Specific Policy)을 적용하면 익명화(Anonymization)가 필요해지기 전부터 불필요한 개인정보 노출을 줄이면서 정당한 기술적 목적에 필요한 정보를 유지할 수 있다.

개인정보 보호 처리(Privacy Processing)는 아키텍처의 여러 단계에서 수행할 수 있다. 민감한 콘텐츠(Sensitive Content)는 영구 저장(Persistent Storage) 전에 로봇에서 직접 탐지하거나, 클라우드 업로드 이전에 엣지 서버(Edge Server)에서 처리하거나, 데이터셋 준비(Dataset Preparation) 과정에서 익명화할 수 있다. 보다 이른 단계에서 처리하면 식별 가능한 정보를 전달받는 시스템의 수를 줄일 수 있지만 충분한 온보드 연산 자원(Onboard Compute)이 필요하다. 적절한 처리 위치는 개인정보 위험(Privacy Risk), 지연 시간(Latency), 연산 자원, 승인된 용도를 위한 증거 보존 필요성을 종합적으로 고려하여 결정해야 한다.

얼굴 블러 처리(Face Blurring)는 로봇 카메라 데이터에서 일반적으로 사용되는 시각적 익명화(Visual Anonymization) 방법이다. 얼굴 탐지기(Face Detector)가 사람의 얼굴이 포함될 가능성이 높은 영역을 식별하면 이미지나 비디오를 보다 광범위하게 사용하기 전에 해당 영역을 블러(Blur), 픽셀화(Pixelation), 마스킹(Masking), 대체(Replacement)할 수 있다. 한 프레임에서는 얼굴이 보호되지만 인접 프레임에서 그대로 노출되면 여전히 신원이 드러날 수 있으므로 익명화 작업은 프레임 전체에 걸쳐 일관되게 적용해야 한다.

따라서 비디오 익명화(Video Anonymization)는 프레임 단위 탐지(Frame-by-Frame Detection)뿐만 아니라 시간적 추적(Temporal Tracking)을 함께 활용하는 것이 효과적이다. 얼굴이 한 번 탐지되면 추적 기능을 이용하여 짧은 탐지 실패, 모션 블러(Motion Blur), 부분 가림(Partial Occlusion), 시점 변화(Changing Viewpoint)가 발생하더라도 보호 영역을 유지할 수 있다. 시간적 일관성(Temporal Consistency)은 익명화 마스크(Anonymization Mask)의 불필요한 변동도 줄인다. 그러나 추적만으로 개인정보 보호가 보장된다고 가정해서는 안 되며, 추적 대상의 손실이나 새롭게 등장하는 사람에 대응하기 위해 반복적인 탐지와 검증이 필요하다.

탐지 신뢰도(Detection Confidence)는 신중하게 처리해야 한다. 높은 임계값(Threshold)을 적용하면 오탐(False Detection)을 줄일 수 있지만 탐지하기 어려운 얼굴이 그대로 노출될 수 있으며, 지나치게 낮은 임계값은 민감하지 않은 이미지 영역까지 불필요하게 가릴 수 있다. 개인정보 보호 중심 시스템(Privacy-Oriented System)은 불확실성이 존재할 때 보수적인 보호(Conservative Protection)를 우선하는 경우가 많다. 익명화된 데이터셋이 어떤 방식으로 생성되었는지를 파악할 수 있도록 신뢰도 점수(Confidence Score), 탐지기 버전(Detector Version), 처리 파라미터(Processing Parameter), 검증 결과(Validation Result)를 메타데이터로 유지해야 한다.

얼굴 블러 처리만으로 완전한 개인정보 보호가 이루어지는 것은 아니다. 사람은 의복, 신체 특징(Body Appearance), 위치, 차량 정보, 배지, 텍스트 또는 주변 환경 맥락(Surrounding Context)을 통해 식별될 수 있다. 데이터셋과 사용 목적에 따라 차량 번호판 블러링(License-Plate Blurring), 텍스트 삭제(Text Redaction), 사람 마스킹(Person Masking), 오디오 익명화(Audio Anonymization), 정밀 위치 정보(Precise Location Information) 제거와 같은 추가 처리가 필요할 수 있다. 따라서 개인정보 보호 정책(Privacy Policy)은 얼굴만을 민감한 요소로 간주하지 않고 전체 관측 데이터(Entire Observation)를 고려해야 한다.

중요한 아키텍처 설계 결정 중 하나는 익명화된 파생 데이터(Anonymized Derivative)를 생성한 이후에도 식별 가능한 원본 데이터(Identifiable Original)를 보존할 것인지 여부이다. 일부 애플리케이션은 사고 조사(Incident Investigation), 품질 검증(Quality Verification), 법적으로 승인된 목적을 위해 제한된 기간 동안 원본 데이터가 필요할 수 있지만 일반적인 인공지능 개발에서는 익명화된 사본만 사용할 수 있다. 원본 데이터와 개인정보 보호 처리 데이터(Privacy-Processed Dataset)는 명확하게 다른 접근 권한(Access Permission), 보존 기간, 수명주기 규칙(Lifecycle Rule)을 가진 별도의 데이터 등급(Data Class)으로 저장해야 한다.

원본 데이터를 일시적으로 보존해야 하는 경우 접근은 최소 권한 원칙(Principle of Least Privilege)을 따라야 한다. 승인된 사용자 또는 서비스만 식별 가능한 기록을 검색할 수 있도록 하고, 일반적인 엔지니어링 팀은 익명화된 파생 데이터를 사용하도록 해야 한다. 인증(Authentication), 역할 기반 또는 속성 기반 권한 부여(Role-Based or Attribute-Based Authorization), 암호화(Encryption), 감사 로깅(Audit Logging), 통제된 내보내기 메커니즘(Controlled Export Mechanism)을 적용하면 민감한 센서 데이터가 의도된 환경 밖으로 복사될 가능성을 줄일 수 있다.

암호화(Encryption)는 민감한 데이터를 전송 중(In Transit)과 저장 중(At Rest) 모두에서 보호한다. 전송 암호화(Transport Encryption)는 로봇-엣지(Robot-to-Edge) 및 엣지-클라우드(Edge-to-Cloud) 통신을 보호해야 하며, 저장 데이터 암호화(Encryption at Rest)는 필요한 경우 온보드 저장소, 네트워크 결합 스토리지(NAS), 서버, 클라우드 객체(Cloud Object)를 보호해야 한다. 암호화는 승인된 사용자가 데이터를 복호화할 수 있으므로 익명화를 대체하지는 않지만 무단 접근, 도난 또는 인프라 침해(Infrastructure Compromise)에 대응하는 추가적인 보안 경계(Security Boundary)를 제공한다.

메타데이터(Metadata)는 개인정보 보호 상태(Privacy State)를 명시적으로 설명해야 한다. 센서 객체(Sensor Object)는 원본(Original), 제한됨(Restricted), 익명화됨(Anonymized), 부분 익명화됨(Partially Anonymized), 특정 후속 목적에 대해 승인됨(Approved for Downstream Purpose)과 같은 상태로 분류할 수 있다. 유용한 메타데이터에는 익명화 방법(Anonymization Method), 탐지기 버전, 처리 타임스탬프(Processing Timestamp), 개인정보 보호 검증 상태(Privacy Validation Status), 보존 등급(Retention Class), 접근 정책(Access Policy), 원본 데이터 참조(Source-Data Reference)가 포함될 수 있다. 이를 통해 후속 시스템이 모든 카메라 파일에 동일한 개인정보 보호 처리가 적용되었다고 잘못 가정하는 것을 방지할 수 있다.

개인정보 보호 변환(Privacy Transformation)은 원본 데이터와 파생 데이터 사이의 출처 정보(Provenance)를 유지해야 한다. 익명화된 비디오는 일반 사용자가 제한된 원본 페이로드(Restricted Payload)에 접근할 수 없도록 하면서도 해당 데이터를 생성한 원본 기록과 처리 구성(Processing Configuration)을 참조할 수 있어야 한다. 출처 정보는 재현성(Reproducibility), 감사(Auditing), 재처리(Reprocessing), 품질 조사(Quality Investigation)를 지원한다. 더 우수한 얼굴 탐지기가 개발되면 정책에서 허용하는 경우 승인된 시스템이 보존된 원본을 이용하여 개선된 익명화 파생 데이터를 다시 생성할 수 있다.

익명화 실패(Failed Anonymization)는 민감한 정보를 노출할 수 있기 때문에 품질 검증(Quality Validation)이 필수적이다. 자동 검사(Automated Check)를 통해 처리 이후에도 탐지 가능한 얼굴이 남아 있는지, 마스크가 의도된 영역을 충분히 가리고 있는지, 비디오 프레임이 누락되었는지를 확인할 수 있다. 선택된 데이터셋은 통제된 접근 환경(Controlled Access Environment)에서 사람의 검토(Human Review)가 추가로 필요할 수 있다. 개인정보 보호 준수가 단순히 파이프라인이 성공적으로 실행되었다는 사실만으로 가정되지 않고 측정될 수 있도록 검증 결과를 데이터셋 품질 메타데이터(Dataset Quality Metadata)에 포함해야 한다.

익명화는 의도된 기술적 작업에 필요한 정보를 충분히 보존해야 한다. 지나친 블러 처리(Excessive Blurring)는 장애물 탐지(Obstacle Detection), 인간-로봇 상호작용(Human-Robot Interaction), 행동 분석(Behavior Analysis), 장면 이해(Scene Understanding)에 필요한 특징까지 제거할 수 있다. 따라서 개인정보 보호 처리는 가능한 경우 신원 관련 정보(Identity-Related Information)와 작업 관련 구조(Task-Relevant Structure)를 구분해야 한다. 예를 들어 보호된 사람 영역에서도 로봇 알고리즘에 필요한 신체 위치(Body Position), 움직임(Motion), 분할 경계(Segmentation Boundary), 비식별 기하학 정보(Non-Identifying Geometric Information)를 유지할 수 있다.

어노테이션 파이프라인(Annotation Pipeline)은 개인정보 보호 통제와 일관되게 운영되어야 한다. 라벨링 작업에 식별 가능한 콘텐츠가 필요하지 않다면 외부 또는 내부 어노테이터(Annotator)에게 익명화된 데이터를 제공해야 한다. 제한된 원본 데이터가 실제로 필요한 경우 접근을 제한하고 기록하며 일반적인 어노테이션 워크플로(Annotation Workflow)와 분리해야 한다. 어노테이션 결과는 개인정보 보호가 적용된 데이터셋 식별자(Privacy-Safe Dataset Identifier)를 참조하여 라벨이 제한된 원본 데이터에 대한 새로운 연결을 의도치 않게 생성하지 않도록 해야 한다.

보존(Retention)과 삭제(Deletion)는 기본적인 개인정보 보호 메커니즘이다. 저장 용량이 충분하다는 이유만으로 민감한 원본 데이터를 무기한 유지해서는 안 된다. 수명주기 정책(Lifecycle Policy)은 다른 승인된 보존 요구사항이 존재하지 않는 경우 익명화된 파생 데이터의 검증이 완료된 후 임시 원시 기록(Temporary Raw Recording)을 자동으로 삭제할 수 있다. 삭제 상태(Deletion Status)를 추적하여 물리적 저장소에서 의도적으로 제거된 객체가 데이터 카탈로그(Data Catalog)에 계속 존재하는 것처럼 표시되지 않도록 해야 한다.

엣지-클라우드 업로드 정책(Edge-to-Cloud Upload Policy)은 데이터가 로봇이나 시설을 벗어나기 전에 개인정보 보호 분류(Privacy Classification)를 인식해야 한다. 익명화된 데이터는 일반적인 클라우드 수집(Cloud Ingestion)이 허용될 수 있지만 식별 가능한 기록은 제한된 저장 위치(Restricted Destination)로 전송해야 하거나 온프레미스 환경(On-Premise Environment)을 벗어나는 것이 금지될 수 있다. 따라서 개인정보 보호 분류는 라우팅 결정(Routing Decision)에 직접 참여하여 네트워크 전송이 로컬 저장소와 접근 정책에 설정된 보호 조치를 우회하지 않도록 할 수 있다.

다중 모달 데이터셋(Multimodal Dataset)은 이미지에서 신원 정보를 제거하는 것만으로 전체 데이터셋이 익명화되지 않을 수 있기 때문에 특별한 주의가 필요하다. 위성항법시스템 좌표(GNSS Coordinate), 오디오, 타임스탬프, 궤적(Trajectory), 환경적 맥락(Environmental Context), 관련 메타데이터는 서로 결합될 경우 민감한 정보를 노출할 수 있다. 개인정보 보호 평가(Privacy Assessment)는 모달리티 사이의 관계와 상관 데이터(Correlated Data)를 통한 재식별(Re-Identification) 가능성을 고려해야 한다. 따라서 접근 및 익명화 정책은 개별 파일 수준뿐만 아니라 데이터셋 수준(Dataset Level)에서도 동작해야 한다.

감사 가능성(Auditability)을 확보하면 조직은 민감한 로봇 데이터가 어떤 방식으로 처리되었는지를 입증할 수 있다. 로그(Log)는 민감한 콘텐츠 자체를 복제하지 않으면서 수집(Collection), 익명화, 접근, 전송(Transfer), 수정(Modification), 내보내기(Export), 삭제 이벤트를 기록할 수 있다. 이러한 기록을 데이터셋 버전(Dataset Version), 개인정보 보호 메타데이터, 처리 출처 정보(Processing Provenance)와 결합하면 원본 센서 데이터 획득부터 승인된 후속 사용까지 추적 가능성(Traceability)을 확보하고 개인정보 보호 처리 파이프라인의 실패를 식별할 수 있다.

강건한 개인정보 보호 준수 센서 데이터 아키텍처(Robust Privacy-Compliant Sensor Data Architecture)는 데이터 최소화, 선택적 보존, 얼굴 및 식별자 익명화(Face and Identifier Anonymization), 엣지 처리(Edge Processing), 접근 제어(Access Control), 암호화, 출처 정보, 검증, 수명주기 관리(Lifecycle Management), 감사 가능성을 통합한다. 개인정보 보호는 단일 얼굴 블러 작업(Face-Blur Operation)이 아니라 전체 로봇 데이터 파이프라인(Complete Robot Data Pipeline)의 속성이 된다. 이를 통해 식별 가능한 정보의 불필요한 노출을 체계적으로 줄이면서 센서 데이터를 디버깅, 어노테이션, 분석, 인공지능 학습, 장기적인 피지컬 인공지능(Physical AI) 개발에 활용할 수 있다.
