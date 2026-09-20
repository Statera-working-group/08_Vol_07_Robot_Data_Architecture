**Volume 07 Robot Data Architecture**

# 11. Synthetic Data Management

## 11.01 Synthetic Data Necessity and Use Case Scenarios

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

합성 데이터(Synthetic Data)는 물리적 로봇(Physical Robot)이 강건한 인공지능(AI) 학습에 필요한 모든 조건을 현실 환경에서 수집하기 어렵기 때문에 현대 로봇 데이터 아키텍처(Robot Data Architecture)의 핵심 구성 요소가 되었다. 실제 데이터 획득(Real-World Data Acquisition)은 비용, 시간, 안전성, 하드웨어 가용성(Hardware Availability), 환경 접근성(Environmental Accessibility), 희귀 사건(Rare Event)의 낮은 발생 빈도 등에 의해 제한된다. 합성 데이터는 인지(Perception), 내비게이션(Navigation), 조작(Manipulation), 피지컬 AI(Physical AI) 개발을 위한 통제 가능하고 반복 가능하며 확장 가능한 데이터셋(Dataset)을 생성함으로써 실제 관측 데이터를 보완한다.

로봇 데이터 아키텍처(Robot Data Architecture)에서 합성 데이터(Synthetic Data)는 일시적인 시뮬레이션 출력(Simulation Output)이 아니라 관리되는 데이터 자산(Managed Data Asset)으로 취급되어야 한다. 이미지(Image), 깊이 맵(Depth Map), 분할 마스크(Segmentation Mask), 포인트 클라우드(Point Cloud), 자세(Pose), 궤적(Trajectory), 센서 메타데이터(Sensor Metadata), 정답 주석(Ground-Truth Annotation)은 정의된 생성(Generation), 검증(Validation), 저장(Storage), 버전 관리(Versioning), 학습(Training) 파이프라인을 거쳐야 한다. 이러한 접근 방식은 시뮬레이션을 AI 데이터 아키텍처, 어노테이션 프로세스(Annotation Process), 디지털 트윈(Digital Twin), 재현 가능한 모델 개발(Reproducible Model Development)과 직접 연결한다.

합성 데이터(Synthetic Data)는 중요한 운용 조건을 물리적으로 재현하기 어렵거나 위험한 경우에 특히 높은 가치를 가진다. 실외 로봇(Outdoor Robot)은 비정상적인 장애물, 낮은 가시성, 극단적인 조명, 충돌 직전 상황 등에 대한 데이터가 필요할 수 있으며, 실내 자율이동로봇(Indoor AMR)은 높은 보행자 밀도, 비정상적인 물체 배치, 복잡한 복도 상호작용 등에 대한 데이터가 필요할 수 있다. 매니퓰레이터(Manipulator) 역시 실제 장면을 반복적으로 구성하지 않고도 물체 자세(Object Pose), 형상(Geometry), 재질(Material), 파지 구성(Grasp Configuration)을 다양하게 변화시킨 데이터로부터 이점을 얻을 수 있다.

시뮬레이션 플랫폼(Simulation Platform)은 이러한 데이터셋을 대규모로 생성하는 데 필요한 통제 가능한 환경을 제공한다. 합성 데이터 생성 워크플로(Synthetic Generation Workflow)는 다수의 시나리오를 실행하기 전에 로봇 형상(Robot Geometry), 환경(Environment), 객체(Object), 재질(Material), 조명(Lighting), 카메라(Camera), 라이다(LiDAR), 깊이 센서(Depth Sensor), 물리적 상호작용(Physical Interaction)을 정의할 수 있다. Isaac Sim과 같은 플랫폼은 자동 생성된 정답 데이터(Ground Truth)와 함께 센서 관측값을 생성할 수 있어 실제 센서 데이터에서 일반적으로 요구되는 수작업 라벨링(Manual Labeling)을 줄일 수 있다.

합성 이미지 및 센서 생성(Synthetic Image and Sensor Generation)은 장면 상태(Scene State), 센서 구성(Sensor Configuration), 생성된 라벨(Generated Label) 사이의 관계를 유지해야 한다. 따라서 카메라 내부·외부 파라미터(Camera Intrinsic and Extrinsic Parameters), 객체 변환(Object Transform), 조명 설정(Illumination Setting), 시뮬레이션 시드(Simulation Seed), 에셋 버전(Asset Version), 센서 노이즈 모델(Sensor Noise Model)이 각 데이터셋과 함께 관리되어야 한다. 이러한 메타데이터가 없으면 시각적으로 사실적인 합성 샘플이라도 시뮬레이션 에셋과 학습 요구사항이 변경될 때 재현, 감사, 비교 또는 재생성하기 어려워질 수 있다.

도메인 랜덤화(Domain Randomization)는 모델 동작을 지배해서는 안 되는 다양한 파라미터를 체계적으로 변화시켜 합성 데이터셋(Synthetic Dataset)의 다양성을 확장한다. 조명 강도(Lighting Intensity), 텍스처(Texture), 색상(Color), 객체 위치(Object Position), 카메라 자세(Camera Pose), 배경 형태(Background Appearance), 센서 노이즈(Sensor Noise), 환경 형상(Environmental Geometry)을 시뮬레이션 실행마다 변경할 수 있다. 하나의 시각적으로 완벽한 가상 환경에 모델을 학습시키는 대신 다양한 현실 가능 조건에 노출시켜 더욱 강건한 특징 학습(Robust Feature Learning)을 유도한다.

그러나 랜덤화(Randomization)는 제약 없이 적용하기보다 실제 운용 도메인(Operational Domain)을 중심으로 설계해야 한다. 비현실적인 조합은 저장 공간과 학습 자원을 소비하면서도 유용한 정보를 거의 제공하지 못할 수 있다. 따라서 파라미터 범위(Parameter Range)는 목표 배치 환경(Target Deployment Environment), 알려진 실패 모드(Known Failure Mode), 의도적으로 선정된 스트레스 사례(Stress Case)를 반영해야 한다. 랜덤화 설정(Randomization Configuration) 자체도 버전 관리되는 데이터 자산으로 취급하여 특정 합성 데이터셋을 생성한 시나리오 분포(Scenario Distribution)를 정확하게 확인할 수 있어야 한다.

포인트 클라우드 생성(Point Cloud Generation)은 기하학적 정확도(Geometric Accuracy), 좌표계(Coordinate Frame), 가시성(Visibility), 샘플링 밀도(Sampling Density), 측정 거리 제한(Range Limit), 센서 특성(Sensor Characteristics)이 후속 인지 처리에 직접적인 영향을 주기 때문에 추가적인 요구사항을 가진다. 합성 라이다 파이프라인(Synthetic LiDAR Pipeline)은 시뮬레이터로부터 정확한 객체 식별자(Object Identity)와 자세(Pose)를 유지하면서 대규모의 라벨링된 3차원 관측 데이터를 생성할 수 있다. 이러한 특성은 수작업 3차원 어노테이션(3D Annotation)의 비용이 높은 검출, 분할, 위치 추정, 매핑, 장애물 인식 등의 공간 AI(Spatial AI) 작업에 유용하다.

생성된 포인트 클라우드(Point Cloud)는 해당 시뮬레이션 상태(Simulation State) 및 다중모달 센서 관측(Multimodal Sensor Observation)과 연결된 상태를 유지해야 한다. 동기화된 하나의 샘플에는 RGB 이미지(RGB Image), 깊이(Depth), 라이다(LiDAR), 의미론적 라벨(Semantic Label), 인스턴스 식별자(Instance Identifier), 로봇 자세(Robot Pose), 환경 상태(Environmental State)가 공통 타임스탬프(Common Timestamp) 또는 시나리오 식별자(Scenario Identifier)를 기준으로 포함될 수 있다. 이러한 관계를 유지하면 다중모달 학습(Multimodal Training)이 가능하며 예상하지 못한 모델 동작을 정확한 가상 장면까지 역추적할 수 있다.

합성 데이터 품질(Synthetic Data Quality)은 시각적 사실성(Visual Realism)만으로 판단할 수 없다. 데이터셋이 사실적으로 보이더라도 목표 모델(Target Model)에 중요한 통계적 또는 운용 특성을 제대로 표현하지 못할 수 있다. 따라서 품질 평가는 라벨 정확성(Label Correctness), 기하학적 일관성(Geometric Consistency), 시나리오 커버리지(Scenario Coverage), 다양성(Diversity), 센서 충실도(Sensor Fidelity), 분포 균형(Distribution Balance), 시뮬레이션과 실제 관측 사이의 도메인 갭(Domain Gap)을 고려해야 한다. FID(Fréchet Inception Distance)와 같은 지표는 이미지 분포 비교를 지원할 수 있지만 전체 평가 프로세스의 일부에 불과하다.

도메인 갭 분석(Domain-Gap Analysis)은 합성 환경과 물리적 환경 사이에서 모델에 실질적으로 영향을 주는 차이에 집중해야 한다. 엔지니어는 두 도메인에 대해 특징 분포(Feature Distribution), 검출 오류(Detection Error), 분할 성능(Segmentation Performance), 깊이 특성(Depth Characteristics), 포인트 클라우드 통계(Point-Cloud Statistics), 실패 패턴(Failure Pattern)을 비교할 수 있다. 목표는 반드시 합성 데이터를 현실과 구별할 수 없게 만드는 것이 아니라, 합성 데이터를 학습에 사용했을 때 대표적인 실제 검증 및 테스트 데이터셋에서 성능이 개선되는지를 확인하는 것이다.

이러한 이유로 합성 데이터와 실제 데이터의 혼합 학습(Mixed Synthetic and Real Training)은 두 데이터 소스를 서로 경쟁적인 대안으로 취급하는 것보다 효과적일 수 있다. 합성 데이터는 광범위한 커버리지와 저비용의 다양성을 제공하며, 실제 데이터(Real Data)는 실제 센서 동작과 배치 환경 특성에 학습을 연결한다. 학습 파이프라인은 샘플링 비율(Sampling Ratio), 커리큘럼 단계(Curriculum Stage), 도메인별 데이터 증강(Domain-Specific Augmentation), 미세조정 일정(Fine-Tuning Schedule), 데이터셋 가중치(Dataset Weight)를 제어할 수 있다.

최적의 데이터 혼합 비율(Optimal Data Mixture)은 모델 개발 과정에서 변화할 수 있다. 초기 학습에서는 대규모 합성 데이터셋을 사용하여 광범위한 표현(Representation)을 학습하고 이후 검증된 실제 데이터의 비중을 점진적으로 높일 수 있다. 희귀하거나 안전에 중요한 시나리오(Rare or Safety-Critical Scenario)는 합성 데이터에서 의도적으로 높은 비율로 유지할 수 있다. 모든 실험은 데이터셋 버전, 혼합 비율, 샘플링 정책, 전처리 파라미터(Preprocessing Parameter), 모델 구성을 기록해야 한다.

운영 규모(Production Scale)에서 합성 데이터 생성은 개별적인 시뮬레이션 실험이 아니라 자동화된 파이프라인으로 운영되어야 한다. 생성 파이프라인(Generation Pipeline)은 시나리오 정의와 파라미터 설정을 입력받아 시뮬레이션 워크로드(Simulation Workload)를 실행하고, 센서 출력을 수집하며, 생성 샘플을 검증하고, 메타데이터를 구성한 후 승인된 데이터셋을 관리형 저장소(Managed Storage)에 게시할 수 있다. 지속적 통합(Continuous Integration) 메커니즘을 이용하면 시뮬레이션 에셋, 로봇 모델, 센서 구성 또는 생성 소프트웨어가 변경될 때 필요한 데이터셋을 다시 생성할 수 있다.

대규모 시뮬레이션 작업은 기술적으로 완전하지만 실제 학습에는 사용할 수 없는 샘플을 생성할 수 있기 때문에 자동 검증(Automated Validation)이 필수적이다. 파이프라인 검사는 누락된 프레임(Missing Frame), 잘못된 라벨(Invalid Label), 손상된 파일(Corrupted File), 잘못된 좌표 변환(Incorrect Coordinate Transformation), 예상하지 못한 객체 분포, 정의된 범위를 벗어난 센서 출력 등을 탐지할 수 있다. 실패한 샘플은 학습 저장소에 자동으로 유입되지 않도록 격리하고 생성 로그(Generation Log)와 검증 보고서(Validation Report)를 데이터셋과 연결해 유지해야 한다.

저장 아키텍처(Storage Architecture)는 단순한 파일명이나 디렉터리 구조 이상으로 합성 데이터셋을 구분할 수 있어야 한다. 각 데이터셋 버전(Dataset Version)은 시뮬레이터 및 소프트웨어 버전, 장면 에셋(Scene Asset), 로봇 구성, 센서 모델, 랜덤화 정책(Randomization Policy), 시나리오 정의, 생성 코드, 어노테이션 스키마(Annotation Schema), 생성 파라미터를 식별해야 한다. 콘텐츠 해시(Content Hash) 또는 불변 식별자(Immutable Identifier)를 사용하면 우발적인 교체를 방지하고 데이터 카탈로그(Data Catalog)와 계보 기록(Lineage Record)을 통해 생성 데이터와 이를 사용한 실험 및 학습 모델을 연결할 수 있다.

합성 데이터 생성은 매우 큰 데이터 볼륨(Data Volume)을 만들 수 있으므로 수명주기 관리(Lifecycle Management)도 중요하다. 원시 시뮬레이터 출력(Raw Simulator Output), 중간 렌더링 결과(Intermediate Render Product), 검증된 학습 샘플(Validated Training Sample), 파생 데이터셋(Derived Dataset)은 서로 다른 보존 정책(Retention Policy)을 적용할 수 있다. 반복적으로 사용할 수 있는 정답 데이터와 설정 메타데이터는 유지하면서 재생성 가능한 중간 데이터는 삭제하거나 저비용 저장소로 이동하여 재현성과 저장 비용 사이의 균형을 유지할 수 있다.

데이터가 시뮬레이션에서 생성되더라도 법적·윤리적 고려사항(Legal and Ethical Considerations)은 여전히 중요하다. 합성 데이터셋은 저작권이 있는 에셋(Copyrighted Asset), 사용이 제한된 3D 모델, 라이선스가 적용된 텍스처(Licensed Texture), 편향된 시나리오 가정 또는 보호 대상 실제 데이터에서 파생된 표현을 포함할 수 있다. 따라서 조직은 에셋 출처(Asset Provenance), 사용 권한(Usage Rights), 생성 소스(Generation Source), 변환 이력(Transformation History)을 추적해야 하며 합성 데이터라는 이유만으로 무제한적인 소유권이나 개인정보·지식재산권 의무로부터의 자동 면제를 의미한다고 판단해서는 안 된다.

편향 관리(Bias Management)는 시뮬레이션 개발자가 생성 세계에 포함될 사람, 객체, 환경, 행동, 실패 상황을 직접 선택하기 때문에 특히 중요하다. 부적절한 시나리오 설계는 실제 배치에 필요한 조건을 체계적으로 제외하고 모델 성능에 대한 잘못된 확신을 만들 수 있다. 따라서 데이터셋 검토(Dataset Review)는 다양한 운용 상황의 커버리지를 확인하고 합성 데이터를 학습이나 검증에 승인하기 전에 의도적인 제외 항목, 가정, 한계, 알려진 데이터 공백(Data Gap)을 문서화해야 한다.

궁극적으로 합성 데이터 관리(Synthetic Data Management)는 생성된 샘플의 양이 아니라 측정 가능한 모델 성능 향상(Measurable Model Performance Improvement)을 기준으로 평가해야 한다. 성공적인 합성 데이터 프로그램은 선택된 합성 시나리오가 독립적인 실제 평가 데이터에서 검출, 분할, 내비게이션, 조작, 강건성 또는 희귀 사건 대응 능력을 향상시킨다는 것을 보여주어야 한다. 실제 데이터 전용, 합성 데이터 전용, 혼합 데이터 학습 실험을 비교하여 합성 데이터의 기여도를 가정이 아닌 측정 결과로 확인해야 한다.

이 과정은 합성 데이터 수명주기(Synthetic-Data Lifecycle)를 하나의 피드백 루프(Feedback Loop)로 완성한다. 실제 로봇의 실패는 부족한 시나리오를 발견하게 하고, 시뮬레이션은 해당 조건을 재현하고 다양화하며, 자동화 파이프라인은 라벨링된 데이터셋을 생성한다. 이후 새로운 샘플을 학습에 반영하고 실제 환경 평가를 통해 성능 개선 여부를 확인한다. 이러한 결과는 다시 시나리오 설계, 랜덤화 범위, 품질 기준, 저장 정책, 데이터셋 선택 과정으로 전달되어 가상 데이터 생산을 실제 로봇의 운용 요구사항에 지속적으로 정렬한다.

보다 광범위한 로봇 소프트웨어 구조(Robot Software Structure)에서 이 관리 계층(Management Layer)은 시뮬레이션 엔지니어링(Simulation Engineering)의 합성 데이터 생성 기능을 중복하는 것이 아니라 이를 보완한다. 시뮬레이션은 가상 환경과 센서 관측값을 생성하는 데 집중하고, 로봇 데이터 아키텍처는 이러한 출력이 어떻게 검증되고, 카탈로그화되고, 버전 관리되며, 실제 관측 데이터와 결합되고, AI 파이프라인으로 전달되는지를 관리한다. 이러한 역할 구분은 합성 데이터 생성(Synthetic Data Generation)을 더 큰 소프트웨어, 시뮬레이션, 피지컬 AI(Physical AI), 데이터 관리(Data Management) 생태계와 연결한다.

## 11.02 Isaac Sim Synthetic Image / Sensor Data Gen [w/Code]

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

NVIDIA Isaac Sim은 로보틱스(Robotics)와 피지컬 AI(Physical AI)를 위한 합성 이미지 및 센서 데이터셋(Synthetic Image and Sensor Dataset)을 생성할 수 있는 시뮬레이션 환경(Simulation Environment)을 제공한다. 개발자는 실제 데이터 수집에 전적으로 의존하는 대신 가상 로봇, 환경, 객체, 카메라, 라이다(LiDAR) 센서, 조명 조건을 구성하고 통제된 시나리오를 반복적으로 실행할 수 있다. 이렇게 생성된 데이터는 인지(Perception), 내비게이션(Navigation), 조작(Manipulation), 다중모달 AI 학습(Multimodal AI Training)을 지원할 수 있다.

Isaac Sim에서 합성 데이터 생성(Synthetic Data Generation)은 로봇 모델, 환경 형상(Environmental Geometry), 물리 객체(Physical Object), 재질(Material), 센서 구성(Sensor Configuration)을 포함하는 구조화된 시뮬레이션 장면(Simulation Scene)에서 시작한다. 이러한 구성 요소는 관측 데이터가 생성되는 가상 세계(Virtual World)를 형성한다. 장면 파라미터(Scene Parameter)를 프로그래밍 방식으로 제어할 수 있기 때문에 동일한 환경을 정확하게 재현하거나 체계적으로 변경하여 실제 테스트 환경을 다시 구축하지 않고도 대규모 변형 데이터를 생성할 수 있다.

카메라 기반 생성 파이프라인(Camera-Based Generation Pipeline)은 이미지 해상도(Image Resolution), 카메라 자세(Camera Pose), 초점 특성(Focal Characteristics), 클리핑 범위(Clipping Range), 관측 시점(Observation Viewpoint)과 같은 속성을 정의한다. 카메라는 목표 응용 분야에 따라 로봇에 장착하거나 외부에 배치할 수 있다. 시뮬레이션 중 렌더링된 RGB 프레임(Rendered RGB Frame)을 장면 정보와 함께 수집하여 시각 관측 데이터를 해당 데이터를 생성한 정확한 로봇 상태 및 환경 구성과 연결할 수 있다.

시뮬레이션 기반 이미지 생성(Simulation-Based Image Generation)의 주요 장점은 가상 장면으로부터 정답 정보(Ground-Truth Information)를 직접 얻을 수 있다는 것이다. 시뮬레이터는 객체의 식별자, 위치, 형상, 관계를 이미 알고 있기 때문에 렌더링된 관측 데이터와 함께 어노테이션 정보(Annotation Information)를 생성할 수 있다. 따라서 의미론적 분할(Semantic Segmentation), 인스턴스 정보(Instance Information), 깊이(Depth), 객체 자세(Object Pose) 등의 구조화된 라벨(Structured Label)을 모든 이미지에 수작업으로 어노테이션하지 않고 생성할 수 있다.

깊이 데이터(Depth Data)는 RGB 외형 정보를 보완하는 기하학적 정보(Geometric Information)를 제공한다. 시뮬레이션된 깊이 센서(Simulated Depth Sensor)는 관측 지점과 가시 표면 사이의 거리를 기록하면서 해당 카메라 프레임과의 동기화를 유지할 수 있다. 이를 통해 RGB와 깊이 관측값을 서로 독립적인 파일이 아니라 하나의 다중모달 샘플(Multimodal Sample)로 처리할 수 있으며, 장애물 인지, 객체 위치 추정, 장면 재구성(Scene Reconstruction), 조작 등의 응용 분야를 지원할 수 있다.

라이다 시뮬레이션(LiDAR Simulation)은 합성 데이터 생성을 이미지 공간(Image Space)의 관측에서 3차원 공간 센싱(Three-Dimensional Spatial Sensing)으로 확장한다. 가상 라이다(Virtual LiDAR)는 스캐닝 동작을 표현하고 시뮬레이션 환경의 포인트 클라우드(Point Cloud)를 생성할 수 있다. 센서 위치, 방향, 측정 범위, 샘플링 특성, 환경 형상은 생성되는 포인트 분포(Point Distribution)에 영향을 준다. 따라서 생성된 포인트 클라우드는 해당 센서 구성 및 좌표계(Coordinate Frame) 정보와 함께 저장해야 한다.

여러 센서 모달리티(Sensor Modality)가 공통 시뮬레이션 타임라인(Common Simulation Timeline)을 공유할 때 센서 데이터 생성의 활용도는 더욱 높아진다. RGB, 깊이, 분할(Segmentation), 라이다, 로봇 자세(Robot Pose), 관절 상태(Joint State), 환경 상태(Environmental State)를 동일한 시뮬레이션 단계에서 수집하고 타임스탬프(Timestamp) 또는 프레임 식별자(Frame Identifier)를 통해 연결할 수 있다. 이러한 동기화는 센서 융합(Sensor Fusion), 다중모달 학습(Multimodal Learning), 피지컬 AI 학습에 필요한 전체 관측 상황을 후속 파이프라인에서 재구성할 수 있도록 한다.

합성 데이터셋(Synthetic Dataset)은 센서 페이로드(Sensor Payload) 자체보다 더 많은 정보를 보존해야 한다. 생성된 각 샘플에는 시뮬레이션 장면, 로봇 모델, 카메라 또는 라이다 파라미터, 센서 변환(Sensor Transform), 에셋 버전(Asset Version), 환경 구성(Environmental Configuration), 생성 설정(Generation Setting)을 설명하는 메타데이터(Metadata)가 포함되는 것이 바람직하다. 특히 랜덤 시드(Random Seed)와 시나리오 식별자(Scenario Identifier)는 특정 샘플을 재현하거나 파이프라인 변경 이후 데이터셋을 다시 생성할 수 있도록 해준다.

Isaac Sim 생성 워크플로(Generation Workflow)는 반복 실행 과정에서 장면 파라미터를 변경할 수도 있다. 조명, 객체 위치, 텍스처(Texture), 재질, 카메라 시점, 배경, 환경 배치를 변경하여 데이터셋 다양성(Dataset Diversity)을 높일 수 있다. 이러한 변형은 도메인 랜덤화(Domain Randomization)의 기반을 형성하며, 그 목적은 단순히 더 많은 이미지를 생성하는 것이 아니라 실제 환경의 변화에 대한 강건성(Robustness)을 향상시키는 통제된 변화를 학습 시스템에 제공하는 것이다.

랜덤화 파라미터(Randomization Parameter)는 명확한 운용 요구사항(Operational Requirement)과 연결되어야 한다. 실내 자율이동로봇(Indoor AMR)의 경우 복도 형상, 장애물 위치, 보행자를 모사한 객체, 조명, 카메라 시점 등을 유용하게 변화시킬 수 있다. 조작 작업에서는 객체 자세, 표면 외형, 클러터(Clutter), 그리퍼 접근 방향(Gripper Approach), 작업공간 구성(Workspace Configuration)을 변화시킬 수 있다. 이를 통해 생성 데이터가 임의의 시각적 변형이 아니라 실제 목표 로봇 작업을 표현하도록 할 수 있다.

합성 데이터 생성은 실제 로봇으로 반복적으로 수집하기 비효율적이거나 위험한 시나리오를 생성하는 데에도 활용할 수 있다. 충돌 직전 상황, 비정상적인 장애물 배치, 어려운 조명 조건, 부분적으로 가려진 객체, 드물게 발생하는 객체 자세 등을 의도적으로 재현할 수 있다. 시나리오 발생 빈도(Scenario Frequency)를 실제 환경의 발생 빈도와 독립적으로 제어할 수 있으므로 드물지만 운용상 중요한 상황을 학습 데이터셋에서 충분히 표현할 수 있다.

대규모 환경에서 합성 데이터 생성은 수동 렌더링 절차(Manual Rendering Procedure)가 아니라 반복 가능한 데이터 파이프라인(Repeatable Data Pipeline)으로 운영되어야 한다. 하나의 구성(Configuration)을 통해 시뮬레이션 에셋, 센서, 시나리오 파라미터, 출력 모달리티(Output Modality), 랜덤화 규칙(Randomization Rule), 생성 데이터 규모를 정의할 수 있다. 자동화된 실행 과정은 장면을 초기화하고 시뮬레이션 에피소드(Simulation Episode)를 실행하며 센서 출력을 수집하고 라벨을 생성한 후 결과를 검증하여 완성된 샘플을 관리형 데이터셋 저장소(Managed Dataset Repository)에 게시할 수 있다.

수천 또는 수백만 개의 관측 데이터가 필요한 경우 프로그래밍 기반 생성(Programmatic Generation)이 특히 중요하다. 스크립트(Script)는 일관된 이름 규칙, 메타데이터, 디렉터리 구조를 유지하면서 시나리오 구성과 파라미터 조합을 반복 실행할 수 있다. 워크로드가 허용하는 경우 생성 작업(Generation Job)을 사용 가능한 컴퓨팅 자원(Compute Resource)에 분산할 수도 있다. 이를 통해 시뮬레이션을 대화형 시각화 환경(Interactive Visualization Environment)에서 확장 가능한 AI 학습 데이터 공급원으로 전환할 수 있다.

출력 데이터 구성(Output Organization)은 서로 다른 모달리티 간의 관계를 유지해야 한다. RGB 이미지, 깊이 맵(Depth Map), 분할 라벨(Segmentation Label), 포인트 클라우드를 독립된 데이터셋으로 취급하는 대신 공통 샘플 또는 프레임 식별자를 통해 서로 연결할 수 있다. 메타데이터는 해당 로봇 자세, 센서 변환, 장면 상태(Scene State), 시뮬레이션 타임스탬프를 추가로 참조할 수 있다. 이러한 구조는 이후 모델별 학습 형식(Model-Specific Training Format)으로 데이터를 변환하는 과정을 단순화한다.

시뮬레이션이 정상적으로 실행되었다고 해서 반드시 사용 가능한 데이터가 생성되는 것은 아니므로 생성 직후 검증(Validation)을 수행해야 한다. 자동화 검사는 누락된 프레임(Missing Frame), 비어 있는 센서 출력, 잘못된 깊이 값(Invalid Depth Value), 일관되지 않은 데이터 크기, 누락된 라벨, 잘못된 좌표 변환, 손상된 파일(Corrupted File)을 탐지할 수 있다. 이러한 검사를 통과하지 못한 샘플은 학습 파이프라인에 자동으로 포함되지 않도록 승인된 데이터셋과 분리해야 한다.

시각적 검사(Visual Inspection)와 통계적 검사(Statistical Inspection)는 자동화된 검증을 보완한다. 엔지니어는 객체 분포(Object Distribution), 조명 범위, 깊이 값, 포인트 밀도(Point Density), 라벨 빈도(Label Frequency)가 의도한 시나리오 설계와 일치하는지 확인할 수 있다. 데이터셋 통계(Dataset Statistics)는 개별 샘플을 살펴보는 것만으로 발견하기 어려운 과도한 반복이나 예상하지 못한 불균형을 찾아낼 수 있다. 이는 사람의 직접적인 감독 없이 랜덤화를 통해 대규모 데이터셋을 생성할 때 특히 중요하다.

생성된 데이터셋은 이를 생성한 구성과 함께 버전 관리(Versioning)되어야 한다. 로봇 형상, 센서 배치, 장면 에셋, 시뮬레이션 파라미터, 어노테이션 정의(Annotation Definition), 생성 스크립트가 변경되면 출력 파일명이 동일하더라도 생성되는 데이터는 달라질 수 있다. 각 데이터셋 버전을 생성 구성(Generation Configuration) 및 소프트웨어 환경(Software Environment)과 연결하면 실험 재현성(Reproducibility)을 확보하고 서로 다른 모델 버전을 신뢰성 있게 비교할 수 있다.

다중모달 시뮬레이션(Multimodal Simulation)은 짧은 시간에도 대규모 데이터 볼륨(Data Volume)을 생성할 수 있기 때문에 저장소 관리(Storage Management)가 중요하다. 동일한 시나리오에서 생성되는 고해상도 RGB 프레임, 깊이 맵, 분할 마스크(Segmentation Mask), 포인트 클라우드, 메타데이터는 전체 저장 공간 요구량을 빠르게 증가시킨다. 따라서 파이프라인은 원시 시뮬레이션 출력(Raw Simulation Output), 검증된 학습 데이터, 파생 형식(Derived Format), 임시 산출물(Temporary Artifact)을 구분하고 보존 정책(Retention Policy)을 적용해야 한다.

합성 데이터는 궁극적으로 실제 로봇 모델에 얼마나 유용한지를 기준으로 평가해야 한다. 시각적으로 뛰어난 Isaac Sim 데이터셋이라도 이를 사용해 학습한 모델이 실제 센서 환경에서 제대로 동작하지 않는다면 그 가치는 제한적이다. 따라서 생성 데이터는 대표적인 실제 관측 데이터와 비교해야 하며, 실제 검증 또는 테스트 데이터셋(Real Validation or Test Dataset)을 이용해 모델 성능을 측정함으로써 남아 있는 시뮬레이션-현실 도메인 갭(Simulation-to-Real Domain Gap)을 파악해야 한다.

실용적인 학습 전략(Training Strategy)은 Isaac Sim 데이터와 실제 로봇 데이터를 함께 사용하는 경우가 많다. 합성 샘플은 광범위한 시나리오 커버리지, 저비용 어노테이션, 희귀 사건 데이터를 제공할 수 있으며, 실제 샘플은 시뮬레이션이 완전히 재현하지 못할 수 있는 센서 노이즈(Sensor Noise), 환경 복잡성(Environmental Complexity), 물리적 특성을 반영한다. 샘플링 비율(Sampling Ratio)과 미세조정 단계(Fine-Tuning Stage)는 실제 배치 환경에서 측정한 성능을 기준으로 실험적으로 조정할 수 있다.

최종적으로 이러한 워크플로는 지속적인 시뮬레이션-데이터-모델 루프(Simulation-to-Data-to-Model Loop)를 형성한다. 실제 환경에서 발생한 모델 실패(Model Failure)를 통해 부족한 조건을 발견하고, 해당 조건을 Isaac Sim에서 재현하여 새로운 센서 관측 데이터와 라벨을 생성한다. 검증된 데이터셋은 다시 학습 파이프라인에 투입되고 업데이트된 모델은 실제 데이터에서 다시 평가된다. 이러한 피드백 과정을 통해 합성 이미지 및 센서 데이터 생성은 독립적인 시뮬레이션 활동이 아니라 로봇 데이터 아키텍처(Robot Data Architecture)의 운영 구성 요소로 발전한다.

## 11.03 Domain Randomization Design [w/Code]

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

도메인 랜덤화(Domain Randomization)는 인공지능(AI) 모델이 변화하는 실제 환경 조건에서도 유효한 특징을 학습하도록 시뮬레이션 파라미터(Simulation Parameter)를 의도적으로 변화시키는 합성 데이터 설계 기법(Synthetic Data Design Technique)이다. 하나의 완벽하게 사실적인 환경을 재현하려는 대신 시뮬레이터는 객체, 센서, 조명, 형상, 물리적 조건을 다양하게 변화시킨 현실적으로 가능한 환경을 생성한다. 이렇게 확보된 다양성은 모델의 강건성(Robustness)을 향상시키고 특정 시각적 또는 환경적 특성에 대한 의존성을 줄일 수 있다.

핵심 설계 원칙은 하나의 고정된 시뮬레이션 구성(Simulation Configuration)이 아니라 가능한 환경들의 분포(Distribution)를 정의하는 것이다. 각 시뮬레이션 에피소드(Simulation Episode)는 이러한 분포에서 파라미터를 샘플링하여 동일한 기본 작업에 대해 서로 다른 관측 결과를 생성한다. 따라서 로봇은 학습에 필요한 작업 의미(Task Semantics)를 유지하면서 서로 다른 색상, 텍스처(Texture), 객체 위치, 카메라 시점, 조명 수준, 센서 노이즈(Sensor Noise), 환경 배치를 경험할 수 있다.

랜덤화(Randomization)는 목표 운용 도메인(Target Operational Domain)을 명확하게 정의하는 것에서 시작해야 한다. 파라미터는 실제 배치된 로봇이 합리적으로 경험할 것으로 예상되는 변화와 모델의 취약점을 드러낼 수 있는 선택된 스트레스 조건(Stress Condition)을 표현해야 한다. 실내 자율이동로봇(Indoor AMR), 실외 로봇(Outdoor Robot), 매니퓰레이터(Manipulator), 휴머노이드(Humanoid)는 서로 다른 분포에서 동작하므로 재사용 가능한 랜덤화 프레임워크(Randomization Framework)는 일반적인 메커니즘과 응용 분야별 파라미터 범위 및 시나리오 제약을 구분해야 한다.

시각적 도메인 랜덤화(Visual Domain Randomization)는 조명, 색상, 텍스처, 재질 특성(Material Property), 그림자, 반사, 배경과 같은 외형 관련 변수를 변경한다. 조명은 강도, 방향, 색온도(Color Temperature), 광원 위치 등을 변화시킬 수 있으며 표면 재질은 거칠기(Roughness), 반사율(Reflectivity), 텍스처를 변경할 수 있다. 이러한 변화는 인지 모델(Perception Model)이 객체의 정체성을 특정 시각적 외형과 연결하여 학습하는 것을 억제하고 다양한 영상 조건에 대한 경험을 확대한다.

기하학적 랜덤화(Geometry Randomization)는 시뮬레이션 세계의 공간적 특성을 변화시킨다. 객체는 시나리오 규칙에 따라 이동, 회전, 크기 변경, 재배치, 추가 또는 제거될 수 있다. 가구, 팔레트, 상자, 장애물, 도구 및 기타 에셋(Asset)은 물리적으로 의미 있는 제약을 유지하면서 서로 다른 위치에 나타날 수 있다. 이러한 변화는 환경의 배치를 사전에 완전히 알 수 없는 상황에서 동작해야 하는 내비게이션 및 조작 시스템에 특히 중요하다.

카메라 랜덤화(Camera Randomization)는 관측 과정 자체를 변화시킨다. 카메라 위치, 방향, 시야각(Field of View), 초점 특성(Focal Characteristics), 노출(Exposure) 및 기타 파라미터를 검증된 범위 내에서 샘플링할 수 있다. 작은 변화는 설치 공차(Installation Tolerance)나 진동을 표현할 수 있으며 더 큰 변화는 학습 과정에서 시점 커버리지(Viewpoint Coverage)를 확장할 수 있다. 의도적인 분포 외(Out-of-Distribution) 테스트를 수행하는 경우가 아니라면 선택된 범위는 실제 센서 구성과 호환되어야 한다.

센서 랜덤화(Sensor Randomization)는 동일한 개념을 RGB 카메라 이외의 센서로 확장한다. 깊이 측정값(Depth Measurement)에는 노이즈, 누락값(Missing Value), 거리 의존적 불확실성(Range-Dependent Uncertainty)을 포함할 수 있으며 라이다(LiDAR) 시뮬레이션에서는 측정 노이즈, 샘플링 특성, 측정 거리, 관측 패턴을 변화시킬 수 있다. 관성측정장치(IMU)와 기타 시뮬레이션 센서에도 유사한 불확실성 모델(Uncertainty Model)을 적용할 수 있다. 이러한 변화는 모델이 실제 물리 시스템에서는 얻을 수 없는 완벽하게 깨끗한 합성 센서 출력을 가정하지 않도록 한다.

학습 목표가 상호작용 동역학(Interaction Dynamics)에 의존하는 경우 물리적 특성(Physical Property)도 랜덤화할 수 있다. 객체 질량, 마찰(Friction), 반발계수(Restitution), 질량 중심(Center of Mass), 액추에이터 특성(Actuator Characteristics), 접촉 특성(Contact Property)을 적절한 범위에서 변화시킬 수 있다. 특히 조작 및 강화학습(Reinforcement Learning) 응용에서는 하나의 정확한 물리 모델에 대해서만 학습한 정책(Policy)이 실제 객체나 로봇 메커니즘이 시뮬레이션 모델과 다를 때 실패할 수 있기 때문에 이러한 접근 방식이 유용하다.

물리적 또는 의미론적 관계가 존재하는 경우 랜덤화 파라미터를 서로 독립적인 값으로 취급해서는 안 된다. 예를 들어 객체 배치는 충돌 제약(Collision Constraint)을 준수해야 하며, 조명의 변화는 장면 형상(Scene Geometry)과 일관성을 유지해야 하고, 센서 위치는 실제로 가능한 장착 구성(Feasible Mounting Configuration)을 보존해야 한다. 조건부 랜덤화(Conditional Randomization)를 사용하면 하나의 샘플링된 파라미터가 다른 파라미터를 제한하도록 하여 물리적으로 불가능하거나 운용상 의미가 없는 데이터를 대량 생성하는 문제를 방지할 수 있다.

파라미터 범위(Parameter Range)는 세심한 엔지니어링이 필요하다. 변화의 폭이 크다고 해서 자동으로 더 좋은 학습 데이터가 생성되는 것은 아니다. 지나치게 넓은 분포는 비현실적인 관측 데이터를 생성하여 컴퓨팅 및 저장 자원을 소비하면서 학습 효율을 저하시킬 수 있다. 반대로 범위가 지나치게 좁으면 시뮬레이션 특성에 과적합(Overfitting)될 수 있다. 초기 경계값은 센서 사양, 운용 측정값, 엔지니어링 공차, 현장 관측, 알려진 실패 사례를 기반으로 설정할 수 있다.

확률 분포(Probability Distribution) 역시 중요한 설계 요소를 제공한다. 균등 샘플링(Uniform Sampling)은 단순하지만 비정상적인 극단값에 지나치게 높은 확률을 부여할 수 있으며, 정규 분포(Normal Distribution), 절단 분포(Truncated Distribution), 범주형 분포(Categorical Distribution), 가중 분포(Weighted Distribution), 경험적 분포(Empirical Distribution)는 특정 변수의 특성을 더욱 적절하게 표현할 수 있다. 드물지만 안전에 중요한 조건은 충분한 학습 사례를 확보하기 위해 실제 발생 빈도보다 의도적으로 높은 샘플링 확률을 부여할 수 있다.

시나리오 수준 랜덤화(Scenario-Level Randomization)는 하나의 운용 상황을 중심으로 여러 변수를 함께 조정한다. 개별 객체를 독립적으로 변경하는 대신 혼잡한 복도, 부분적으로 차단된 경로, 복잡하게 물체가 배치된 작업공간, 어려운 조명, 비정상적인 객체 자세, 충돌 직전 상황과 같은 시나리오를 정의할 수 있다. 이후 각 시나리오 내부의 파라미터를 랜덤화하면서 해당 상황의 의미론적 목적을 유지하여 임의적인 변화가 아닌 통제된 다양성(Controlled Diversity)을 생성할 수 있다.

재현성(Reproducibility)을 확보하려면 랜덤화된 모든 시뮬레이션 실행의 구성을 보존해야 한다. 랜덤 시드(Random Seed), 파라미터 값, 에셋 식별자(Asset Identifier), 장면 버전(Scene Version), 센서 구성, 시뮬레이터 버전, 시나리오 정의를 생성된 샘플 또는 데이터셋 메타데이터(Dataset Metadata)와 함께 기록해야 한다. 이후 모델 실패가 발견되면 엔지니어는 해당 가상 조건을 재구성하고 원인을 조사한 후 랜덤화 정책을 수정하여 관련 샘플을 다시 생성할 수 있다.

따라서 도메인 랜덤화는 데이터셋 버전 관리(Dataset Versioning)와 밀접하게 연결된다. 파라미터 범위, 확률 분포, 에셋 풀(Asset Pool), 센서 노이즈 모델, 시나리오 제약 중 하나라도 변경되면 생성 소프트웨어 자체가 동일하더라도 실질적으로 새로운 데이터 분포가 만들어진다. 랜덤화 정책(Randomization Policy)에 명확한 버전을 부여하여 모델 실험에서 각 학습 데이터셋이 정확히 어떤 시뮬레이션 분포로부터 생성되었는지 식별할 수 있어야 한다.

자동화된 생성 파이프라인(Automated Generation Pipeline)은 랜덤화 정책을 대규모로 실행할 수 있다. 시나리오 구성은 파라미터 공간(Parameter Space)과 제약을 정의하고, 시뮬레이션 엔진(Simulation Engine)은 값을 샘플링하며, 각 에피소드는 동기화된 센서 출력과 라벨을 생성한다. 이후 검증 단계(Validation Stage)는 생성된 샘플이 기술적 및 의미론적 요구사항을 충족하는지 확인한 후 게시한다. 잘못된 구성이나 손상된 출력은 승인된 학습 데이터셋을 오염시키지 않도록 제거해야 한다.

품질 모니터링(Quality Monitoring)은 개별 파일을 검증하는 것뿐만 아니라 랜덤화 프로세스가 생성한 전체 분포를 확인해야 한다. 히스토그램(Histogram), 범주별 빈도(Category Frequency), 객체 수, 자세 분포(Pose Distribution), 조명 범위, 센서 통계, 시나리오 커버리지(Scenario Coverage)를 분석하면 생성된 데이터셋이 실제로 의도한 설계와 일치하는지 확인할 수 있다. 대규모 생성에서는 명목상의 파라미터 범위가 실제 관측 데이터에서 균형 잡힌 분포로 이어지지 않아 숨겨진 불균형이 발생할 수 있다.

도메인 갭 분석(Domain-Gap Analysis)은 랜덤화 정책을 개선하기 위한 피드백을 제공한다. 합성 데이터셋과 실제 데이터셋을 이미지 특성, 특징 분포(Feature Distribution), 깊이 통계, 포인트 클라우드(Point Cloud) 특성, 검출 오류(Detection Error), 분할 결과(Segmentation Result), 기타 작업별 측정값을 통해 비교할 수 있다. 실제 환경에서의 낮은 성능과 연관되는 차이가 발견되면 어떤 시뮬레이션 파라미터, 센서 모델 또는 시나리오 분포를 조정해야 하는지 판단할 수 있다.

모델 성능(Model Performance)은 가장 중요한 평가 기준으로 유지되어야 한다. 랜덤화 전략은 통제된 데이터셋 변형을 사용하여 모델을 학습하고 대표적인 실제 환경 검증 데이터(Real-World Validation Data)에서 평가해야 한다. 고정 시뮬레이션(Fixed Simulation), 랜덤화 시뮬레이션(Randomized Simulation), 실제 데이터 전용 학습(Real-Only Training), 실제-합성 혼합 학습(Mixed Real-Synthetic Training)을 비교하면 특정 랜덤화 정책이 단순히 데이터셋 크기를 증가시키는 것이 아니라 실제로 유용한 강건성을 제공하는지 확인할 수 있다.

커리큘럼 기반 랜덤화(Curriculum-Based Randomization)는 학습 또는 데이터 생성 과정에서 난이도를 점진적으로 높일 수 있다. 초기 시나리오에서는 정상 운용 조건 주변에서 적절한 수준의 변화를 적용하고, 이후 단계에서는 더 강한 시각적 변화, 증가된 클러터, 더 큰 자세 변화, 센서 성능 저하, 더욱 어려운 상호작용을 도입할 수 있다. 이러한 진행 방식은 학습 시스템이 점점 더 어려운 환경 분포에 대응하기 전에 기본적인 작업 동작을 먼저 확립하도록 지원할 수 있다.

적응형 도메인 랜덤화(Adaptive Domain Randomization)는 모델 성능이 이후의 파라미터 샘플링에 영향을 미치도록 이 개념을 확장한다. 모델이 이미 안정적으로 동작하는 조건에는 상대적으로 적은 생성 자원을 할당하고, 어렵거나 실패가 자주 발생하는 영역에는 더 많은 데이터 생성을 집중할 수 있다. 이를 통해 합성 데이터 생성은 유사한 샘플을 계속 생산하는 대신 아직 해결되지 않은 모델의 취약점에 점차 집중하는 피드백 메커니즘(Feedback Mechanism)을 형성한다.

피지컬 AI(Physical AI) 시스템에서 도메인 랜덤화는 인지(Perception), 상태 추정(State Estimation), 내비게이션(Navigation), 조작(Manipulation), 제어(Control)를 동시에 포함할 수 있다. 하나의 생성 에피소드에서 시각적 외형, 공간 배치, 센서 불확실성, 객체 동역학(Object Dynamics), 로봇 특성을 변화시키면서 동기화된 다중모달 관측(Synchronized Multimodal Observation)을 유지할 수 있다. 이를 통해 생성된 데이터셋은 각각의 모달리티를 독립적인 데이터 소스로 취급하는 대신 센싱, 물리 상태, 행동(Action) 사이의 상호작용을 표현할 수 있다.

전체 설계는 지속적인 현실-시뮬레이션 피드백 루프(Real-to-Simulation Feedback Loop)를 형성한다. 실제 로봇의 관측과 실패를 통해 부족한 변동성을 식별하고, 엔지니어는 이러한 결과를 랜덤화 파라미터와 시나리오로 변환하며, 시뮬레이션은 새로운 데이터셋을 생성한다. 이후 모델을 다시 학습하고 실제 물리 환경에서 성능을 평가한다. 따라서 도메인 랜덤화는 시뮬레이션 학습과 실제 로봇 운용 사이의 격차를 줄이고 학습 커버리지(Training Coverage)를 체계적으로 확장하기 위한 통제된 데이터 엔지니어링(Data Engineering) 메커니즘으로 기능한다.

## 11.04 Synthetic Point Cloud Data Gen Pipeline [w/Code]

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

합성 포인트 클라우드 생성(Synthetic Point-Cloud Generation)은 모든 장면을 실제 로봇으로 직접 스캔하지 않고도 3차원 센서 데이터셋(Three-Dimensional Sensor Dataset)을 생성하는 방법이다. 로보틱스(Robotics)에서 포인트 클라우드(Point Cloud)는 장애물 검출, 위치 추정(Localization), 매핑(Mapping), 객체 인식, 조작(Manipulation), 공간 추론(Spatial Reasoning)에 필수적이다. 시뮬레이션을 사용하면 장면 형상, 센서 구성, 객체 상태를 정확하게 파악하면서 대규모의 라벨링된 3차원 관측 데이터를 생성할 수 있다.

합성 포인트 클라우드 파이프라인(Synthetic Point-Cloud Pipeline)은 기하학적으로 정의된 에셋(Asset)을 포함하는 가상 환경에서 시작한다. 바닥, 벽, 건물, 선반, 팔레트, 차량, 도구, 식생 등의 객체는 위치와 방향이 알려진 3차원 형상으로 표현된다. 이후 로봇 모델과 이동 가능한 객체를 장면에 배치하여 시뮬레이션 거리 측정 센서(Simulated Ranging Sensor)가 관측 데이터를 생성할 수 있는 통제된 공간 환경을 구성한다.

포인트 클라우드의 특성은 시뮬레이션된 센싱 모델(Sensing Model)에 직접적으로 의존하기 때문에 센서 구성(Sensor Configuration)은 파이프라인에서 매우 중요한 부분이다. 가상 라이다(Virtual LiDAR)는 수평 및 수직 시야각(Field of View), 각도 해상도(Angular Resolution), 스캐닝 패턴(Scanning Pattern), 측정 범위, 갱신 주기(Update Rate), 센서 자세(Sensor Pose), 좌표계(Coordinate Frame)를 정의할 수 있다. 생성 데이터셋을 시뮬레이션-현실 전이(Simulation-to-Real Transfer)에 활용하려면 이러한 파라미터는 목표 실제 센서의 특성과 유사하게 설정해야 한다.

생성 과정은 센서와 환경에서 관측 가능한 표면 사이의 측정을 시뮬레이션한다. 각각의 유효한 측정값은 센서 좌표계 또는 별도로 정의된 좌표계에서 표현되는 3차원 점을 생성한다. 시뮬레이션 시스템에 따라 강도(Intensity), 의미론적 클래스(Semantic Class), 인스턴스 식별자(Instance Identity), 표면 정보, 타임스탬프(Timestamp) 등의 추가 속성을 각 점과 연결할 수 있으며, 이를 통해 단순한 XYZ 좌표보다 풍부한 데이터를 구성할 수 있다.

포인트 클라우드는 명확하게 정의된 공간 기준이 없으면 의미가 제한되므로 좌표계 관리(Coordinate-Frame Management)가 필수적이다. 센서 좌표계의 관측 데이터는 로봇 베이스(Robot Base), 오도메트리(Odometry), 지도(Map), 월드(World) 좌표계로 변환해야 할 수 있다. 따라서 생성된 포인트 클라우드를 일관되게 재구성하고 로봇 자세, 지도 또는 다른 센서의 관측 데이터와 결합할 수 있도록 센서 외부 파라미터(Sensor Extrinsic Parameter)와 좌표 변환 관계(Transformation Relationship)를 보존해야 한다.

정답 라벨링(Ground-Truth Labeling)은 합성 포인트 클라우드 생성의 주요 장점 중 하나이다. 시뮬레이션 엔진(Simulation Engine)은 어떤 가상 객체가 각각의 측정값을 생성했는지 알고 있기 때문에 의미론적 라벨(Semantic Label)과 인스턴스 라벨(Instance Label)을 장면으로부터 직접 생성할 수 있다. 따라서 수백만 개의 3차원 점을 수작업으로 라벨링하지 않고도 바닥, 벽, 차량, 보행자, 팔레트, 장애물, 로봇 구성 요소 등의 범주와 각각의 점을 연결할 수 있다.

객체 수준 정답 데이터(Object-Level Ground Truth)는 3차원 바운딩 박스(3D Bounding Box), 객체 자세(Object Pose), 크기(Dimension), 속도(Velocity), 식별자(Identifier)를 포함할 수도 있다. 이러한 라벨은 3차원 객체 검출(3D Object Detection), 추적(Tracking), 의미론적 분할(Semantic Segmentation), 인스턴스 분할(Instance Segmentation), 자세 추정(Pose Estimation) 등의 작업을 지원한다. 조작 데이터셋에서는 포인트 클라우드 관측을 객체 형상, 파지 목표(Grasp Target), 작업공간 상태, 로봇 엔드 이펙터 구성(End-Effector Configuration)과 연결할 수도 있다.

합성 포인트 클라우드는 독립적인 센서 파일로만 관리해서는 안 된다. 완전한 생성 파이프라인은 라이다 관측 데이터와 RGB 이미지, 깊이 맵(Depth Map), 분할 마스크(Segmentation Mask), 로봇 자세, 관절 상태(Joint State), 환경 메타데이터(Environmental Metadata)를 동기화할 수 있다. 공통 타임스탬프, 프레임 번호(Frame Number), 시나리오 식별자(Scenario Identifier)를 통해 이러한 모달리티를 연결하면 후속 AI 시스템이 시각적 외형, 3차원 형상, 로봇 상태, 물리적 상황 사이의 관계를 학습할 수 있다.

센서 사실성(Sensor Realism)을 확보하려면 이상적인 기하학적 교차점만 재현하는 것 이상이 필요하다. 실제 라이다 및 깊이 센서는 측정 불확실성(Measurement Uncertainty), 누락된 반환값(Missing Return), 제한된 측정 거리, 가림(Occlusion), 샘플링 아티팩트(Sampling Artifact), 표면 특성에 대한 민감성을 가진다. 합성 파이프라인에는 설정 가능한 노이즈 및 드롭아웃 모델(Noise and Dropout Model)을 적용하여 학습 데이터가 비현실적으로 깨끗해지는 것을 방지할 수 있다. 센서의 불완전성은 임의적인 왜곡이 아니라 현실적인 운용 특성을 기반으로 설계해야 한다.

가림(Occlusion)은 포인트 클라우드 생성에서 특히 중요하다. 일반적으로 센서 위치에서 실제로 보이는 표면만 측정값을 생성해야 하기 때문이다. 벽, 선반, 차량 또는 다른 장애물 뒤에 존재하는 객체는 단순히 해당 객체의 형상이 시뮬레이션에 존재한다는 이유로 포인트 클라우드에 나타나서는 안 된다. 올바른 가시성 처리(Visibility Handling)는 합성 관측 데이터가 실제 3차원 센싱에서 나타나는 부분적이고 시점 의존적인 특성을 유지하도록 한다.

포인트 밀도(Point Density)는 거리, 스캐닝 형상, 객체 방향, 센서 해상도에 따라서도 달라진다. 가까운 객체는 많은 측정점을 포함할 수 있지만 멀리 있거나 부분적으로 가려진 객체는 상대적으로 적은 수의 점만 포함할 수 있다. 유용한 합성 파이프라인은 객체 표면을 균일하게 샘플링하는 대신 이러한 관계를 보존해야 한다. 이를 통해 모델은 실제 거리 측정 센서가 생성하는 관측 데이터와 더욱 유사한 공간 패턴(Spatial Pattern)을 학습할 수 있다.

도메인 랜덤화(Domain Randomization)는 생성되는 포인트 클라우드의 다양성을 확장할 수 있다. 객체 위치, 방향, 크기, 환경 배치, 센서 자세, 장애물 구성, 물리적 에셋을 시뮬레이션 에피소드마다 변화시킬 수 있다. 센서 파라미터와 노이즈 특성 역시 통제된 범위에서 변경할 수 있다. 이를 통해 목표 로봇 작업의 의미론적 특성을 유지하면서 기하학적으로 다양한 관측 데이터를 대량으로 생성할 수 있다.

랜덤화(Randomization)는 물리적 및 운용상의 제약을 준수해야 한다. 팔레트가 벽 내부에 나타나서는 안 되며, 객체는 일반적으로 유효한 지지 표면(Supporting Surface) 위에 위치해야 하고, 센서 장착 위치는 실제 로봇 플랫폼에서 가능한 구성을 유지해야 한다. 조건부 규칙(Conditional Rule)을 사용하면 이러한 관계를 유지하면서도 상당한 다양성을 생성할 수 있다. 이러한 제약이 없다면 대규모 생성 과정에서 비현실적인 형상이 만들어져 데이터셋의 활용 가치가 감소할 수 있다.

합성 3차원 데이터셋에서는 희귀 시나리오(Rare Scenario)를 의도적으로 높은 비율로 생성할 수 있다. 부분적으로 가려진 장애물, 비정상적인 객체 방향, 혼잡한 환경, 좁은 통로, 어려운 파지 구성, 충돌 직전 상황을 정상적인 로봇 운용에서 발생하는 실제 빈도와 관계없이 반복적으로 생성할 수 있다. 이는 동일한 실제 포인트 클라우드를 수집하는 과정이 위험하거나 비용이 많이 들거나 운용상 비효율적인 경우에 특히 유용하다.

대규모 환경에서 포인트 클라우드 생성은 자동화된 데이터 파이프라인(Automated Data Pipeline)으로 구현해야 한다. 시나리오 구성(Scenario Configuration)은 장면, 에셋, 센서 모델, 랜덤화 범위, 출력 형식(Output Format), 생성 데이터 규모를 정의한다. 시뮬레이션 작업은 이러한 구성을 실행하여 관측 데이터를 수집하고 라벨과 메타데이터를 생성하며 결과 샘플을 검증한 후 승인된 데이터를 관리형 저장소(Managed Storage)에 게시한다. 자동화는 생성 과정을 반복 가능하게 만들고 지속적인 AI 개발에 활용할 수 있도록 한다.

출력 형식은 후속 처리 요구사항에 따라 선택하되 이후 변환에 필요한 충분한 정보를 유지해야 한다. 원시 포인트 배열(Raw Point Array)을 구조화된 메타데이터와 함께 저장할 수 있으며 PCD(Point Cloud Data), PLY(Polygon File Format) 또는 응용 분야별 표현 형식을 특정 처리 프레임워크에서 활용할 수 있다. 데이터셋 구조는 센서 페이로드(Sensor Payload)와 메타데이터를 논리적으로 구분하면서 관측 데이터, 라벨, 시나리오 구성 사이의 명확한 참조 관계를 유지해야 한다.

자동 검증(Automated Validation)은 기술적으로 잘못된 샘플이 학습 데이터셋에 들어가기 전에 이를 탐지해야 한다. 검사 과정에서는 비어 있는 포인트 클라우드, 유한하지 않은 좌표(Non-Finite Coordinate), 비정상적인 포인트 수, 잘못된 라벨, 누락된 좌표 변환, 일관되지 않은 타임스탬프, 설정된 센서 범위를 벗어난 점 등을 확인할 수 있다. 기하학 기반 검사(Geometry-Based Check)를 추가하여 바운딩 박스, 좌표 변환, 객체 위치와 해당 포인트 라벨 사이의 관계를 검증할 수도 있다.

통계적 검증(Statistical Validation)은 개별 샘플이 아니라 생성된 데이터셋 전체를 하나의 모집단(Population)으로 분석한다. 엔지니어는 포인트 수 분포(Point-Count Distribution), 객체 빈도, 거리 분포, 클래스 균형(Class Balance), 가림 수준, 공간 커버리지(Spatial Coverage), 시나리오 빈도를 측정할 수 있다. 이러한 통계를 통해 대규모 생성 작업이 의도한 운용 도메인을 실제로 표현하고 있는지 또는 제한된 일부 구성에 데이터가 우연히 집중되어 있는지를 확인할 수 있다.

고도로 자동화된 파이프라인에서도 시각적 검사(Visual Inspection)는 여전히 유용하다. 포인트 클라우드를 바운딩 박스, 의미론적 색상(Semantic Color), 센서 자세, 장면 형상과 함께 렌더링하여 공간 정렬(Spatial Alignment)을 확인할 수 있다. 동기화된 카메라 이미지에 3차원 포인트를 투영하면 수치적인 파일 검사만으로 발견하기 어려운 캘리브레이션(Calibration) 또는 좌표 변환 오류를 확인할 수 있다. 따라서 새로운 데이터셋 버전을 승인하기 전에 대표적인 샘플을 검사해야 한다.

데이터셋 버전 관리(Dataset Versioning)는 포인트 클라우드 파일뿐만 아니라 데이터를 생성한 구성도 포함해야 한다. 센서 모델, 스캐닝 패턴, 노이즈 파라미터, 장면 에셋, 좌표계 규칙(Coordinate Convention), 어노테이션 스키마(Annotation Schema), 랜덤화 정책이 변경되면 데이터 분포도 크게 달라질 수 있다. 이러한 의존 관계를 기록하면 이전 데이터셋을 재현하고 어떤 생성 과정의 변경이 모델 성능에 영향을 주었는지 확인할 수 있다.

포인트 클라우드 파이프라인은 많은 프레임과 시나리오에 걸쳐 수백만 또는 수십억 개의 점을 생성할 수 있기 때문에 저장 공간 요구사항(Storage Requirement)이 크게 증가할 수 있다. 압축(Compression), 청킹(Chunking), 선택적 보존(Selective Retention), 파생 데이터 정책(Derived-Data Policy)을 활용하면 저장 공간 부담을 줄일 수 있다. 재생성 비용이 높은 경우에만 원시 시뮬레이션 출력을 보존하고, 검증된 학습 데이터와 핵심 메타데이터에는 재현성 요구사항에 따라 더 긴 보존 기간을 적용할 수 있다.

합성 포인트 클라우드와 실제 포인트 클라우드는 최종적으로 도메인 갭(Domain Gap)을 측정하기 위해 비교해야 한다. 포인트 밀도, 거리 특성, 노이즈 분포, 누락 반환 패턴(Missing-Return Pattern), 객체 가시성, 기하학적 특징(Geometric Feature), 모델 오류 패턴 등을 비교할 수 있다. 실제 환경에서 낮은 성능과 연관된 차이가 발견되면 센서 시뮬레이션, 환경 에셋, 랜덤화 범위, 노이즈 모델을 조정하기 위한 근거로 활용할 수 있다.

학습 전략(Training Strategy)은 합성 포인트 클라우드와 실제 센서 관측 데이터를 결합할 수 있다. 합성 데이터는 확장 가능한 라벨과 통제된 시나리오 커버리지(Scenario Coverage)를 제공하며 실제 데이터는 하드웨어 고유의 아티팩트(Hardware-Specific Artifact)와 환경 복잡성을 반영한다. 모델은 합성 데이터에서 광범위한 기하학적 표현(Geometric Representation)을 먼저 학습한 후 실제 관측 데이터로 미세조정(Fine-Tuning)하거나 실험적으로 결정한 비율에 따라 두 데이터 소스를 함께 샘플링할 수 있다.

전체 파이프라인은 물리적 로봇과 가상 데이터 생성 사이의 피드백 루프(Feedback Loop)를 형성한다. 실제 환경에서 발생한 실패를 통해 부족한 형상, 센서 조건 또는 시나리오를 식별하고, 시뮬레이션에서 이러한 조건을 재현하고 다양화한다. 이후 파이프라인은 동기화되고 라벨링된 포인트 클라우드를 생성하며 모델을 다시 학습하고 실제 데이터에서 다시 평가한다. 따라서 합성 포인트 클라우드 생성은 3차원 인지(3D Perception)와 피지컬 AI(Physical AI) 시스템을 지속적으로 개선하기 위한 로봇 데이터 아키텍처(Robot Data Architecture)의 관리형 구성 요소로 기능한다.

## 11.05 Synthetic Data Quality Assessment: FID / Domain Gap

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}

합성 데이터 품질 평가(Synthetic Data Quality Assessment)는 생성된 데이터셋이 로봇 인공지능(AI) 시스템의 학습에 사용할 수 있을 만큼 정확하고, 다양하며, 대표성을 갖추고 있고, 실질적으로 유용한지를 판단하는 과정이다. 높은 시각적 사실성(Visual Realism)만으로 높은 데이터 품질을 보장할 수는 없다. 합성 데이터셋은 올바른 라벨, 유효한 기하학적 정보, 적절한 센서 동작을 제공하면서 모델 성능에 영향을 미치는 조건을 충분히 포함하고 목표 운용 환경(Target Operational Environment)의 작업 관련 특성을 보존해야 한다.

품질 평가는 통계적 유사성(Statistical Similarity)이나 모델 성능을 검토하기 전에 기술적 무결성(Technical Integrity)을 확인하는 것에서 시작해야 한다. 생성된 샘플에서 누락된 파일, 손상된 프레임, 잘못된 값, 일관되지 않은 데이터 크기, 잘못된 타임스탬프(Timestamp), 손상된 좌표 변환(Coordinate Transform), 불완전한 어노테이션(Annotation)을 검사해야 한다. 이러한 검사는 기술적으로 결함이 있는 샘플이 이후 평가 단계에 들어가 분포 통계를 왜곡하거나 합성 데이터에 대한 잘못된 결론을 만드는 것을 방지한다.

어노테이션 품질(Annotation Quality)은 자동 정답 데이터(Automatic Ground Truth)가 시뮬레이션의 주요 장점 중 하나이기 때문에 특히 중요하다. 의미론적 라벨(Semantic Label), 인스턴스 식별자(Instance Identifier), 분할 마스크(Segmentation Mask), 깊이 값(Depth Value), 3차원 바운딩 박스(3D Bounding Box), 객체 자세(Object Pose), 궤적(Trajectory)은 시뮬레이션 장면과 일관성을 유지해야 한다. 검증 과정에서는 불가능한 바운딩 박스, 잘못 라벨링된 포인트, 중복되는 인스턴스 식별자, 유효하지 않은 깊이 범위, 가시 객체와 생성된 어노테이션 사이의 불일치를 탐지할 수 있다.

기하학적 일관성(Geometric Consistency)은 로보틱스 데이터셋의 또 다른 중요한 품질 요소이다. 카메라 캘리브레이션(Camera Calibration), 센서 외부 파라미터(Sensor Extrinsics), 로봇 자세(Robot Pose), 좌표계(Coordinate Frame), 객체 변환(Object Transform)은 서로 다른 모달리티(Modality) 사이에서 일치해야 한다. RGB 이미지에 투영된 포인트 클라우드는 해당 장면 형상과 정렬되어야 하며, 깊이 및 분할 출력은 동일한 가시 표면을 표현해야 한다. 이러한 교차 모달 검사(Cross-Modal Check)는 센서 융합(Sensor Fusion), 3차원 인지(3D Perception), 위치 추정(Localization), 피지컬 AI(Physical AI) 학습에서 매우 중요하다.

통계적 품질 평가(Statistical Quality Assessment)는 단순히 기술적으로 유효한 파일을 생성하는 것을 넘어 합성 데이터가 의도한 분포를 충분히 포함하는지를 분석한다. 엔지니어는 객체 빈도, 클래스 균형(Class Balance), 자세 분포(Pose Distribution), 조명 조건, 센서 측정 범위, 포인트 밀도(Point Density), 가림 수준(Occlusion Level), 환경 배치, 시나리오 빈도를 분석할 수 있다. 이러한 측정은 생성 파이프라인이 설정된 파라미터 공간(Parameter Space)의 제한된 일부에 의도하지 않게 샘플을 집중시키고 있는지를 확인하는 데 도움이 된다.

다양성(Diversity)은 통제되지 않은 변화(Uncontrolled Variation)와 구분해야 한다. 시각적으로 서로 다른 샘플을 많이 포함한 데이터셋이라도 해당 변화가 의미 있는 실제 배치 조건과 연결되지 않는다면 운용 커버리지(Operational Coverage)는 부족할 수 있다. 유용한 다양성은 로봇 동작에 영향을 주는 시점, 객체 배치, 환경 형상, 센서 특성, 물리적 속성, 희귀 시나리오의 변화를 포함한다. 따라서 품질 평가에서는 생성된 다양성을 명확하게 정의된 운용 요구사항(Operational Requirement)과 비교해야 한다.

프레셰 인셉션 거리(Fréchet Inception Distance, FID)는 실제 이미지와 생성 이미지의 분포를 비교하는 방법 중 하나이다. FID는 이미지를 픽셀 단위로 직접 비교하는 대신 신경망(Neural Network)이 추출한 특징(Feature)을 통해 이미지 집합을 표현하고 해당 특징 분포의 통계적 특성을 비교한다. 일반적으로 낮은 FID 값은 선택된 특징 표현(Feature Representation)에서 평가된 두 이미지 분포가 서로 더 가깝다는 것을 의미한다.

개념적으로 FID는 실제 이미지 집합과 합성 이미지 집합에서 추출한 특징 벡터(Feature Vector)의 평균(Mean)과 공분산(Covariance)을 비교한다. 특징 평균의 차이는 두 데이터 집합의 중심적인 표현 차이를 나타내며, 공분산의 차이는 특징의 변화와 특징 사이 관계의 차이를 반영한다. 이렇게 계산된 거리는 서로 다른 합성 데이터 생성 구성(Synthetic Data Generation Configuration)을 비교하거나 데이터셋 버전 간 변화를 모니터링하는 데 활용할 수 있는 간결한 수치 지표를 제공한다.

FID를 합성 데이터 품질을 완전하게 표현하는 지표로 해석해서는 안 된다. FID 값은 특징 추출기(Feature Extractor), 샘플 집단, 이미지 전처리(Image Preprocessing), 데이터셋 크기, 평가 대상 도메인에 영향을 받는다. 낮은 FID가 자동으로 더 우수한 로봇 인지, 더 안전한 동작, 더 정확한 기하학적 표현 또는 향상된 시뮬레이션-현실 전이(Simulation-to-Real Transfer)를 의미하지는 않는다. 따라서 FID는 작업별 및 센서별 품질 측정값과 함께 사용해야 한다.

이러한 한계는 합성 로보틱스 데이터셋에 FID가 직접 평가하도록 설계되지 않은 다양한 모달리티가 포함되는 경우 특히 중요하다. 깊이 맵(Depth Map), 라이다 포인트 클라우드(LiDAR Point Cloud), 점유 표현(Occupancy Representation), 로봇 궤적(Robot Trajectory), 촉각 측정(Tactile Measurement), 제어 상태(Control State)에는 다른 평가 지표가 필요하다. RGB 이미지에서도 일반적인 이미지 특징 거리로 충분히 표현되지 않는 작업 핵심적인 기하학적 또는 의미론적 차이가 존재할 수 있다.

도메인 갭(Domain Gap)은 시뮬레이션 데이터와 실제 물리적 배치 환경에서 획득한 관측 데이터 사이의 차이를 의미한다. 이러한 차이는 렌더링 외형(Rendering Appearance), 조명, 재질, 형상, 센서 노이즈, 캘리브레이션, 객체 행동, 환경 복잡성, 물리 동역학(Physical Dynamics)에서 발생할 수 있다. 합성 데이터 품질 평가는 어떤 차이가 단순히 외형적인 차이인지, 그리고 어떤 차이가 실제 로봇 AI의 후속 성능 저하를 유발하는지를 식별해야 한다.

시각적 도메인 갭 분석(Visual Domain-Gap Analysis)은 밝기, 대비, 색상 분포, 텍스처 통계(Texture Statistics), 특징 임베딩(Feature Embedding), 객체 외형, 그림자, 환경 구성을 비교할 수 있다. 이러한 비교를 통해 합성 이미지가 실제 카메라 관측 데이터와 체계적으로 어떻게 다른지 확인할 수 있다. 그러나 시각적으로 명확한 차이가 항상 학습된 인지 시스템에 가장 큰 영향을 주는 차이는 아니므로 시각적 유사성은 모델 동작과 연계하여 평가해야 한다.

센서 도메인 분석(Sensor-Domain Analysis)은 측정 과정 자체에서 발생하는 특성을 평가한다. 합성 깊이 및 라이다 데이터는 거리 분포, 포인트 밀도, 드롭아웃 패턴(Dropout Pattern), 측정 노이즈, 가림 특성, 공간 커버리지(Spatial Coverage)를 이용하여 실제 관측 데이터와 비교할 수 있다. 관성측정장치(IMU)나 다른 센서 시뮬레이션도 실제 센서와 관련된 노이즈 분포, 바이어스(Bias), 드리프트(Drift), 타이밍 동작(Timing Behavior) 등의 특성을 기준으로 평가할 수 있다.

특징 공간 비교(Feature-Space Comparison)는 저수준 통계(Low-Level Statistics)와 모델 수준 평가(Model-Level Evaluation)를 연결하는 역할을 한다. 실제 샘플과 합성 샘플을 관련 특징 추출기, 인지 백본(Perception Backbone) 또는 학습된 모델에 입력하여 내부 표현(Internal Representation)을 비교할 수 있다. 두 데이터셋이 크게 다른 특징 분포를 생성한다면 어떤 환경 또는 센서 특성이 그 차이를 유발하는지 분석하고 시뮬레이션 에셋, 렌더링, 랜덤화(Randomization), 노이즈 모델을 개선할 수 있다.

커버리지 분석(Coverage Analysis)은 합성 데이터 생성이 로봇에 필요한 운용 조건을 충분히 표현하고 있는지를 평가한다. 데이터셋 전체의 도메인 갭이 상대적으로 작더라도 드물지만 중요한 상황이 누락될 수 있다. 따라서 시나리오 수준 지표(Scenario-Level Metric)를 사용하여 어려운 조명, 비정상적인 장애물, 부분 가림(Partial Occlusion), 혼잡한 환경, 극단적인 시점, 드문 객체 자세, 센서 성능 저하 등 응용 분야별 중요 조건의 커버리지를 측정해야 한다.

품질 평가는 시나리오 및 클래스 사이의 균형(Balance)도 확인해야 한다. 대규모 합성 데이터 파이프라인은 수백만 개의 샘플을 쉽게 생성할 수 있지만 많은 데이터의 양이 심각한 불균형을 가릴 수 있다. 자주 샘플링되는 환경이나 일반적인 객체가 학습 데이터를 지배하는 반면 희귀 클래스는 충분히 표현되지 않을 수 있다. 따라서 데이터셋 버전에는 클래스 수, 시나리오 빈도, 파라미터 범위, 주요 조건 조합을 설명하는 분포 보고서(Distribution Report)가 함께 제공되어야 한다.

실제 환경 검증(Real-World Validation)은 합성 데이터가 실질적으로 유용한지를 판단하는 가장 의미 있는 시험을 제공한다. 합성 데이터셋을 사용하여 학습한 모델은 목표 배치 환경을 대표하는 독립적인 실제 검증 또는 테스트 데이터에서 평가해야 한다. 검출 정확도(Detection Accuracy), 분할 품질(Segmentation Quality), 위치 추정 오차(Localization Error), 내비게이션 성공률, 파지 성공률(Grasp Success Rate), 추적 성능(Tracking Performance) 등의 작업별 지표를 통해 합성 데이터 품질 향상이 실제 운용 성능 향상으로 이어지는지 확인할 수 있다.

통제된 실험(Controlled Experiment)은 다른 학습 요인으로부터 합성 데이터의 기여도를 분리하는 데 유용하다. 엔지니어는 모델 아키텍처(Model Architecture)와 평가 조건을 동일하게 유지하면서 실제 데이터만 사용한 모델, 합성 데이터만 사용한 모델, 실제 데이터와 합성 데이터를 서로 다른 비율로 혼합한 모델을 비교할 수 있다. 추가적인 실험을 통해 랜덤화 정책, 센서 모델, 렌더링 품질, 시나리오 커버리지 또는 합성-실제 샘플링 비율(Synthetic-to-Real Sampling Ratio)의 영향을 개별적으로 분석할 수 있다.

품질 게이트(Quality Gate)는 생성된 데이터가 운영용 학습 저장소(Production Training Repository)에 들어가기 전에 충족해야 하는 승인 기준을 공식화할 수 있다. 하나의 데이터셋 버전은 파일 무결성 검사(File-Integrity Check), 어노테이션 검증, 기하학적 일관성 검사, 분포 임계값(Distribution Threshold), 시나리오 커버리지 요구사항, 선택된 도메인 갭 평가를 통과하도록 구성할 수 있다. 이러한 게이트는 합성 데이터 관리를 체계화하고 소수 샘플에 대한 주관적인 시각적 검사에 지나치게 의존하는 문제를 줄인다.

평가 결과는 데이터셋 메타데이터(Dataset Metadata)와 데이터 계보(Data Lineage)의 일부로 저장해야 한다. FID 값, 분포 통계, 검증 보고서, 시나리오 커버리지, 센서 품질 측정값, 모델 평가 결과는 이를 생성한 데이터셋 버전 및 생성 구성과 연결된 상태로 유지해야 한다. 이를 통해 엔지니어는 시간에 따른 데이터셋 세대를 비교하고 모델 성능 변화를 합성 데이터 생산 과정의 구체적인 변경 사항까지 추적할 수 있다.

도메인 갭 측정(Domain-Gap Measurement)은 도메인 랜덤화 설계(Domain-Randomization Design)를 직접적으로 개선하는 데 활용할 수 있다. 실제 이미지가 더 큰 조명 변화를 보인다면 조명 범위를 확장할 수 있으며, 실제 라이다에서 더 많은 누락 반환값이 발생한다면 시뮬레이션 센서 모델을 조정할 수 있다. 부분적으로 가려진 객체에서 주로 실패가 발생한다면 해당 시나리오의 생성 빈도를 증가시킬 수 있다. 따라서 평가는 단순한 최종 보고 활동이 아니라 새로운 데이터 생성 과정의 입력으로 기능한다.

동일한 피드백 원칙(Feedback Principle)은 적응형 합성 데이터 생성(Adaptive Synthetic Data Generation)을 지원할 수 있다. 실제 검증 데이터에서 발생한 모델 오류를 환경 조건, 객체 유형, 센서 상태 또는 시나리오별로 그룹화할 수 있다. 이후 생성 자원을 성능이 여전히 낮은 영역에 집중할 수 있다. 새로운 데이터셋을 생성하고 평가하여 학습에 반영한 후 다시 검증함으로써 합성 데이터 생산을 해결되지 않은 실제 환경의 취약점에 점진적으로 집중시킬 수 있다.

어떤 하나의 지표만으로 합성 데이터셋이 로보틱스에 적합한지를 결정해서는 안 된다. FID는 이미지 분포 비교를 위한 유용한 정보를 제공할 수 있지만 기술적 유효성(Technical Validity), 어노테이션 정확성, 기하학적 일관성, 다양성, 커버리지, 센서 사실성(Sensor Realism), 도메인 갭 측정, 후속 모델 성능(Downstream Model Performance)과 함께 해석해야 한다. 이러한 다양한 관점을 결합해야 합성 데이터 품질을 더욱 신뢰성 있게 정의할 수 있다.

성숙한 품질 평가 파이프라인(Quality-Assessment Pipeline)은 시뮬레이션, 합성 데이터셋, 실제 관측 데이터, AI 모델을 연결하는 폐쇄형 루프(Closed Loop)를 형성한다. 생성 단계에서 후보 데이터를 만들고 자동 검사를 통해 무결성을 검증하며, 통계 및 도메인 갭 분석을 통해 분포 특성을 파악한다. 이후 실제 환경 모델 평가를 통해 실질적인 유용성을 측정하고 그 결과를 다시 시뮬레이션 파라미터와 시나리오에 반영한다. 이러한 순환 과정은 합성 데이터와 로봇이 실제로 동작해야 하는 물리 환경 사이의 정렬(Alignment)을 지속적으로 향상시킨다.

## 11.06 Synthetic and Real Data Mixed Training Strategy [w/Code]

![](images/image7.png){width="7.268055555555556in" height="7.268055555555556in"}

합성 데이터와 실제 데이터를 활용한 혼합 학습(Mixed Training)은 시뮬레이션의 확장성과 제어 가능성을 실제 배치된 로봇에서 수집한 관측 데이터의 물리적 충실도(Physical Fidelity)와 결합한다. 합성 데이터셋은 광범위한 시나리오 커버리지, 정확한 어노테이션(Annotation), 희귀 사건 사례를 제공할 수 있으며, 실제 데이터셋은 시뮬레이션이 완전히 재현하기 어려운 센서의 불완전성, 환경 복잡성, 물리적 상호작용을 반영한다. 목표는 서로 다른 데이터 분포를 제어하면서 두 데이터 소스의 장점을 활용하는 것이다.

혼합 학습 전략(Mixed-Training Strategy)은 사용 가능한 모든 샘플을 단순히 병합하는 것이 아니라 각 데이터 소스의 역할을 정의하는 것에서 시작한다. 합성 데이터는 환경 커버리지를 확대하고, 클래스 균형(Class Balance)을 향상시키며, 어려운 사례를 생성하거나 저비용으로 정답 데이터(Ground Truth)를 제공하는 데 사용할 수 있다. 실제 데이터는 실제 배치 조건에 대한 근거를 제공하고 학습된 표현(Representation)을 물리적 센서, 객체, 환경, 로봇 동작에 정렬하는 역할을 한다.

합성 샘플과 실제 샘플의 비율은 중요한 학습 파라미터(Training Parameter)이다. 두 데이터셋의 분포가 안정적이라면 고정 비율(Fixed Ratio)을 사용할 수 있지만 최적 비율은 작업 복잡성, 합성 데이터의 사실성, 실제 데이터 가용성, 남아 있는 도메인 갭(Domain Gap)에 따라 달라진다. 합성 데이터가 지나치게 많으면 모델이 시뮬레이션 특유의 패턴을 학습할 수 있으며, 반대로 합성 데이터가 너무 적으면 이를 생성한 본래 목적인 데이터 다양성을 충분히 확보하지 못할 수 있다.

따라서 샘플링 정책(Sampling Policy)을 명확하게 설계해야 한다. 학습 배치(Training Batch)에 사전에 정의된 비율의 합성 및 실제 샘플을 포함하거나 클래스, 시나리오, 난이도 또는 모델 성능에 따라 두 데이터 소스를 동적으로 샘플링할 수 있다. 균형 샘플링(Balanced Sampling)은 실제 데이터셋이 일반적인 조건에 집중되어 있고 합성 데이터가 희귀 객체, 비정상적인 자세, 어려운 조명, 안전 중요 상황(Safety-Critical Situation) 등의 목표 사례를 제공하는 경우 특히 유용하다.

일반적인 전략 중 하나는 합성 데이터 사전학습(Synthetic Pretraining) 이후 실제 데이터 미세조정(Real-Data Fine-Tuning)을 수행하는 것이다. 먼저 대규모 합성 데이터셋을 이용하여 광범위한 시각적, 기하학적 또는 행동 표현을 학습함으로써 대규모 실제 데이터 수집에 대한 의존성을 줄인다. 이후 검증된 실제 관측 데이터를 사용하여 사전학습된 모델을 적응시킨다. 미세조정은 시뮬레이션에서 확보한 유용한 커버리지를 유지하면서 학습된 표현을 실제 배치 도메인으로 이동시킨다.

공동 학습(Joint Training)은 전체 학습 과정에서 합성 샘플과 실제 샘플을 함께 제공하는 또 다른 접근 방식이다. 이를 통해 이후의 적응 과정에서 합성 데이터가 제공하는 유용한 다양성을 모델이 잊어버리는 것을 방지하고 두 도메인이 동시에 특징 학습(Feature Learning)에 영향을 주도록 할 수 있다. 그러나 합성 데이터셋의 규모가 훨씬 큰 경우 단순히 샘플 수의 차이로 인해 합성 데이터가 최적화 과정을 지배할 수 있으므로 샘플링과 손실(Loss)을 세심하게 모니터링해야 한다.

커리큘럼 기반 혼합 학습(Curriculum-Based Mixed Training)은 시간에 따라 데이터 구성을 변경한다. 초기 단계에서는 합성 데이터의 비중을 높여 모델이 광범위한 변화에 노출되고 기본적인 작업 지식을 확립하도록 할 수 있다. 이후 단계에서는 실제 관측 데이터의 비율을 점진적으로 증가시켜 물리적 센서 특성과 배치 환경 고유의 세부 사항에 학습을 집중시킨다. 실제 데이터셋에서 확보하기 어려운 커버리지를 제공한다면 어려운 합성 시나리오는 전체 커리큘럼 동안 계속 유지할 수 있다.

실제 데이터가 특정한 취약점을 드러내는 경우에는 반대 방향으로 전략을 운영할 수도 있다. 물리적 관측 데이터를 기반으로 학습한 기준 모델(Baseline Model)이 희귀한 조명, 비정상적인 장애물, 부분 가림(Partial Occlusion), 드문 객체 구성에서 실패할 수 있다. 시뮬레이션은 이러한 조건을 재현하고 목표 지향적인 합성 샘플(Targeted Synthetic Sample)을 생성할 수 있다. 이후 새로운 데이터를 학습에 반영하여 반복적인 실제 데이터 수집 없이 특정 실패 영역을 강화할 수 있다.

도메인 랜덤화(Domain Randomization)는 합성 관측 데이터가 하나의 제한된 가상 외형만 표현하지 않도록 하기 때문에 혼합 학습에서 중요한 역할을 한다. 생성 샘플마다 조명, 재질, 객체 자세, 환경 배치, 센서 파라미터, 노이즈, 물리적 특성을 변화시킬 수 있다. 적절한 랜덤화는 합성 데이터 분포를 확장하여 실제 관측 데이터가 학습 과정에서 경험한 조건으로부터 지나치게 멀리 벗어날 가능성을 줄인다.

도메인 적응(Domain Adaptation)을 통해 합성 데이터와 실제 데이터의 표현 차이를 추가로 줄일 수 있다. 이미지 변환(Image Transformation), 특징 정렬(Feature Alignment), 정규화(Normalization), 미세조정, 도메인 불변 특징(Domain-Invariant Feature)을 학습하도록 설계된 모델 아키텍처 등을 활용할 수 있다. 목적은 반드시 합성 샘플과 실제 샘플을 시각적으로 동일하게 만드는 것이 아니라 후속 인지, 내비게이션, 조작 또는 제어 작업을 방해하는 차이를 줄이는 것이다.

두 도메인에서 데이터 전처리(Data Preprocessing)를 세심하게 제어해야 한다. 이미지 크기 조정, 정규화, 좌표계 규칙(Coordinate Convention), 라벨 스키마(Label Schema), 포인트 클라우드 형식, 데이터 증강(Data Augmentation), 시간적 샘플링(Temporal Sampling)은 의도적인 도메인별 변환이 필요한 경우를 제외하면 서로 호환되어야 한다. 별도의 전처리 파이프라인에서 우연히 발생한 차이는 모델이 의미 있는 작업 특징 대신 학습하는 인위적인 도메인 식별자(Artificial Domain Indicator)가 될 수 있다.

라벨 일관성(Label Consistency) 역시 중요하다. 합성 데이터는 완벽하게 생성된 의미론적 라벨, 인스턴스 마스크(Instance Mask), 깊이, 자세, 3차원 바운딩 박스(3D Bounding Box)를 포함할 수 있지만 실제 데이터에는 서로 다른 정의나 불확실성을 가진 사람의 어노테이션이 포함될 수 있다. 공통 온톨로지(Shared Ontology)와 어노테이션 스키마는 클래스 의미, 무시 영역(Ignored Region), 좌표계 규칙, 품질 규칙을 정의하여 두 데이터 소스의 감독 정보(Supervision)가 의미론적으로 호환되도록 해야 한다.

신뢰도(Confidence)와 데이터 품질은 샘플 가중치(Sample Weighting)에 영향을 줄 수 있다. 물리적 충실도가 중요한 경우 검증된 실제 관측 데이터에 더 높은 가중치를 부여할 수 있으며, 충분히 표현되지 않은 클래스나 시나리오에서는 고품질 합성 샘플의 중요도를 높일 수 있다. 불확실한 실제 어노테이션이나 비현실적인 시뮬레이션 조건을 가진 샘플에는 낮은 가중치를 적용하거나 제외할 수 있다. 이러한 가중치 정책은 비공식적으로 조정하는 것이 아니라 실험 구성(Experiment Configuration)의 일부로 기록해야 한다.

희귀 사건 처리(Rare-Event Handling)는 혼합 학습을 사용하는 가장 중요한 이유 중 하나이다. 실제 데이터셋은 자연적인 사건 발생 빈도를 반영하므로 안전에 중요한 상황이 효과적인 학습에 필요한 수준보다 적게 나타날 수 있다. 합성 데이터 생성은 충돌 직전 상황, 비정상적인 장애물, 센서 성능 저하, 어려운 파지 자세 또는 기타 저빈도 조건의 표현을 의도적으로 증가시킬 수 있다. 학습에서는 합성 데이터의 빈도가 실제 자연 발생 확률을 의미한다고 가정하지 않으면서 이러한 사례의 영향력을 제어할 수 있다.

혼합 학습에서는 의미 있는 평가를 위해 별도의 검증 데이터셋(Validation Set)을 유지해야 한다. 실제 검증 및 테스트 데이터셋은 학습 데이터와 독립적으로 유지하고 목표 운용 환경을 대표해야 한다. 합성 검증 데이터는 통제된 시나리오 변화에 대한 성능을 측정할 수 있지만 실제 환경 평가를 대체할 수는 없다. 두 종류의 검증 데이터를 함께 유지하면 시뮬레이션 환경에서의 성능 향상과 실제 물리 환경으로 전이되는 성능 향상을 구분할 수 있다.

절제 실험(Ablation Experiment)은 각 데이터 소스가 성능에 어떻게 기여하는지를 확인하는 데 유용하다. 모델 아키텍처, 전처리, 평가 조건을 동일하게 유지하면서 실제 데이터 전용(Real-Only), 합성 데이터 전용(Synthetic-Only), 혼합 데이터(Mixed) 구성으로 모델을 학습할 수 있다. 추가 실험에서는 합성-실제 데이터 비율, 랜덤화 정책, 사전학습 기간, 미세조정 일정(Fine-Tuning Schedule), 시나리오 구성을 변화시켜 어떤 요소가 측정 가능한 성능 향상을 제공하는지 확인할 수 있다.

평가는 전체 정확도(Aggregate Accuracy)만을 기준으로 해서는 안 된다. 객체 클래스, 거리, 조명, 가림, 환경, 센서 조건, 시나리오 난이도별로 성능을 구분하여 분석할 수 있다. 혼합 데이터셋이 평균 성능을 향상시키면서도 중요한 특정 운용 조건의 성능을 저하시킬 가능성이 있다. 시나리오 수준 분석(Scenario-Level Analysis)은 이러한 절충 관계(Tradeoff)를 확인하고 샘플링 비율을 조정하거나 추가적인 목표 합성 데이터를 생성하기 위한 직접적인 근거를 제공한다.

학습 지표(Training Metric)도 합성 샘플과 실제 샘플에 대해 별도로 모니터링할 수 있다. 손실, 신뢰도, 특징 분포, 오류 패턴에서 큰 차이가 발생하면 모델이 두 도메인을 서로 다르게 처리하고 있음을 의미할 수 있다. 특징 공간 분석(Feature-Space Analysis)과 도메인 갭 측정(Domain-Gap Measurement)을 이용하면 학습이 공유된 표현으로 수렴하고 있는지 또는 단순히 합성 관측과 실제 관측에 대해 서로 다른 동작을 학습하고 있는지를 확인할 수 있다.

실질적인 학습 분포는 두 데이터 소스와 이들의 혼합 방식에 따라 결정되므로 데이터셋 버전 관리(Dataset Versioning)가 필수적이다. 각 실험은 합성 데이터셋 버전, 실제 데이터셋 버전, 샘플링 비율, 가중치 규칙, 전처리 구성, 데이터 증강 정책, 랜덤 시드(Random Seed)를 기록해야 한다. 이러한 정보가 없으면 코드 수준에서는 모델을 재현할 수 있더라도 실제 학습에 사용된 데이터 구성을 다시 구성하는 것은 불가능할 수 있다.

데이터 계보(Data Lineage)는 생성 또는 수집된 샘플에서 최종 학습 모델까지 연결되어야 한다. 합성 데이터 기록은 시뮬레이터 버전, 장면 에셋(Scene Asset), 랜덤화 정책, 생성 구성을 참조할 수 있으며 실제 데이터 기록은 수집 세션(Collection Session), 센서 구성, 어노테이션 버전을 참조할 수 있다. 이후 학습 매니페스트(Training Manifest)는 이러한 데이터셋의 어떤 부분이 실제 학습에 사용되었는지를 식별하여 데이터 관련 의사결정과 모델 동작 사이의 추적성(Traceability)을 확보한다.

저장 아키텍처(Storage Architecture)는 합성 데이터와 실제 데이터를 서로 구분된 관리형 데이터 소스(Governed Data Source)로 유지하면서 통합된 학습 뷰(Unified Training View)를 제공함으로써 혼합 학습을 지원할 수 있다. 이를 통해 서로 다른 혼합 데이터셋을 만들기 위해 대규모 데이터를 물리적으로 복제하는 것을 방지할 수 있다. 데이터셋 매니페스트 또는 쿼리 기반 선택(Query-Based Selection)을 이용하면 원본 데이터셋과 출처를 유지하면서 실험별 비율, 시나리오 또는 하위 집합을 동적으로 변경할 수 있다.

지속적 학습(Continuous Training)은 혼합 데이터 전략을 피드백 프로세스로 전환할 수 있다. 실제 로봇의 배치는 새로운 관측 데이터를 생성하고 실패 사례를 드러내며, 이러한 데이터는 검증 후 관리되는 실제 데이터셋에 추가된다. 시뮬레이션은 어려운 조건을 재현하고 이를 보완하는 합성 사례를 생성한다. 이후 업데이트된 데이터 혼합을 구성하여 모델을 다시 학습하고 독립적인 실제 환경 평가 데이터에서 성능을 다시 측정한다.

시간이 지나면서 합성-실제 데이터 비율(Synthetic-to-Real Ratio)은 영구적으로 고정된 설계값이 아니라 실증적 근거(Evidence)에 따라 조정되는 변수로 취급해야 한다. 더 많은 고품질 실제 데이터를 확보하면 일부 합성 데이터의 필요성이 감소할 수 있지만 새로운 로봇 작업이나 희귀 시나리오에서는 합성 데이터의 가치가 다시 증가할 수 있다. 학습 데이터 구성은 측정된 모델 성능, 도메인 갭 분석, 데이터 가용성, 운용 위험(Operational Risk)에 따라 지속적으로 변화해야 한다.

피지컬 AI(Physical AI)에서 혼합 학습은 인지(Perception), 공간 이해(Spatial Understanding), 상태 추정(State Estimation), 내비게이션(Navigation), 조작(Manipulation), 제어(Control)를 하나의 데이터 전략 안에서 연결할 수 있다. 합성 환경은 센싱과 물리적 상호작용 전반에서 통제된 변화를 제공하고 실제 로봇은 실제 동역학과 배치 환경의 복잡성을 반영하는 관측 데이터를 제공한다. 따라서 조정된 다중모달 데이터셋(Coordinated Multimodal Dataset)은 환경 상태, 로봇 상태, 센서 관측, 행동(Action) 사이의 관계를 학습하는 모델을 지원할 수 있다.

전체 전략은 폐쇄형 실제-합성 학습 루프(Closed Real-Synthetic Learning Loop)를 형성한다. 실제 환경 데이터는 모델을 물리적 운용 조건에 정렬하고, 합성 데이터 생성은 알려진 조건과 예상 가능한 조건 주변의 커버리지를 확장하며, 혼합 학습은 두 데이터 소스를 결합한다. 이후 독립적인 실제 환경 평가를 통해 생성된 모델의 성능 향상 여부를 측정한다. 다시 발견된 실패는 새로운 실제 데이터 수집과 시뮬레이션 생성을 유도하며, 이를 통해 합성-실제 데이터 혼합은 지속적으로 최적화되는 로봇 데이터 아키텍처(Robot Data Architecture)의 구성 요소로 발전한다.

## 11.07 Synthetic Data Gen Pipeline Automation: CI [w/Code]

![](images/image8.png){width="7.268055555555556in" height="7.268055555555556in"}

합성 데이터 생성 파이프라인 자동화(Synthetic Data Generation Pipeline Automation)는 시뮬레이션 기반 데이터 생산을 수동 엔지니어링 작업에서 반복 가능하고 확장 가능하며 관리되는 운영 프로세스로 전환한다. 로보틱스와 피지컬 AI(Physical AI)에서 데이터셋은 RGB 이미지, 깊이 맵(Depth Map), 분할 마스크(Segmentation Mask), 포인트 클라우드(Point Cloud), 로봇 상태(Robot State), 정답 라벨(Ground-Truth Label)을 포함할 수 있다. 자동화는 이러한 데이터의 생성, 검증, 패키징, 버전 관리, 배포를 조정하여 합성 데이터가 AI 개발을 지속적으로 지원하도록 한다.

운영용 파이프라인(Production Pipeline)은 수동으로 설정하는 시뮬레이션 세션 대신 선언적 생성 구성(Declarative Generation Configuration)에서 시작한다. 구성 파일(Configuration File)은 시뮬레이션 환경, 로봇 모델, 센서, 장면 에셋(Scene Asset), 시나리오 파라미터, 랜덤화 정책(Randomization Policy), 출력 모달리티(Output Modality), 데이터셋 크기, 품질 요구사항을 정의할 수 있다. 구성을 실행 과정과 분리하면 동일한 파이프라인 로직으로 서로 다른 데이터셋을 생성하면서 각 데이터셋이 어떻게 만들어졌는지 명확한 기록을 유지할 수 있다.

시나리오 정의(Scenario Definition)는 자동화된 생성 작업의 운용 맥락을 제공한다. 하나의 시나리오는 창고 내비게이션, 객체 조작, 실외 이동, 인간-로봇 상호작용, 센서 성능 저하 또는 희귀한 안전 관련 조건을 표현할 수 있다. 각 시나리오는 생성 데이터의 의미적 목적을 유지하면서 객체 배치, 환경 상태, 센서 구성, 로봇 동작, 물리적 상호작용을 제어하는 파라미터 분포(Parameter Distribution)와 제약조건(Constraint)을 참조할 수 있다.

오케스트레이션 계층(Orchestration Layer)은 이러한 구성을 실행 가능한 시뮬레이션 작업(Simulation Job)으로 변환한다. 어떤 시나리오를 실행해야 하는지 결정하고, 컴퓨팅 자원을 할당하며, 시뮬레이션 환경을 초기화하고, 실행 상태를 모니터링하며, 생성된 결과를 수집한다. 대규모 워크로드(Workload)는 장면, 랜덤 시드(Random Seed), 시나리오 그룹 또는 에피소드 범위에 따라 독립적인 작업으로 분할할 수 있어 데이터셋의 의미를 변경하지 않고 여러 GPU 또는 컴퓨팅 노드로 합성 데이터 생산을 확장할 수 있다.

결정론적 실행(Deterministic Execution)은 디버깅(Debugging)과 재현성(Reproducibility)을 위해 중요하다. 모든 생성 작업은 랜덤 시드, 구성 버전, 시뮬레이션 소프트웨어 버전, 로봇 모델, 센서 정의, 에셋 식별자(Asset Identifier), 실행 파라미터를 기록해야 한다. 이후 유효하지 않은 샘플이나 모델 실패가 발견되었을 때 엔지니어는 원래 환경을 수동으로 추정하는 대신 해당 시뮬레이션 조건을 다시 구성할 수 있다.

생성된 센서 출력은 자동으로 동기화하고 체계적으로 구성해야 한다. RGB, 깊이, 의미론적 분할(Semantic Segmentation), 인스턴스 분할(Instance Segmentation), 라이다(LiDAR), 포인트 클라우드, 로봇 자세(Robot Pose), 관절 상태(Joint State), 궤적(Trajectory), 환경 상태는 동일한 시뮬레이션 프레임이나 에피소드에 속할 수 있다. 공통 타임스탬프(Timestamp), 프레임 식별자(Frame Identifier), 시나리오 식별자를 사용하면 이러한 관계를 유지하고 수동으로 데이터 연관성을 다시 구성하지 않고도 후속 다중모달 학습(Multimodal Training)에 활용할 수 있다.

자동 어노테이션 생성(Automated Annotation Generation)은 시뮬레이터가 이미 가상 환경의 구조화된 정보를 가지고 있기 때문에 파이프라인에 직접 통합된다. 객체 클래스, 인스턴스 식별 정보, 자세, 분할 마스크, 깊이, 3차원 바운딩 박스(3D Bounding Box) 및 기타 정답 정보(Ground-Truth Information)를 센서 관측 데이터와 함께 생성할 수 있다. 클래스 정의나 라벨링 규칙의 변경을 데이터셋 세대 간에 추적할 수 있도록 어노테이션 스키마(Annotation Schema)를 버전 관리해야 한다.

도메인 랜덤화(Domain Randomization)는 시뮬레이션 스크립트에 비공식적으로 포함하는 대신 관리되는 파이프라인 단계(Managed Pipeline Stage)로 실행할 수 있다. 조명, 재질, 텍스처, 객체 자세, 환경 배치, 센서 파라미터, 노이즈 모델, 물리적 특성을 명시적으로 버전 관리되는 분포에서 샘플링할 수 있다. 이를 통해 엔지니어는 랜덤화 정책을 비교하고 어떤 파라미터 범위가 실제 환경의 모델 성능 향상에 기여하는지를 판단할 수 있다.

지속적 통합(Continuous Integration, CI)은 소프트웨어 엔지니어링 방식을 합성 데이터 생성 과정으로 확장한다. 시뮬레이션 코드, 로봇 정의, 센서 모델, 에셋, 어노테이션 로직 또는 랜덤화 정책이 변경되면 운영용 생성 환경에 반영되기 전에 자동 검사를 실행할 수 있다. 목적은 규모가 크고 비용이 높은 생성 작업을 실행하기 전에 데이터셋의 품질이나 의미를 조용히 변화시키는 문제를 탐지하는 것이다.

CI 파이프라인(CI Pipeline)은 가벼운 구성 및 스키마 검증(Configuration and Schema Validation)에서 시작할 수 있다. 필수 파라미터의 존재 여부, 파일 참조의 유효성, 클래스 식별자의 승인된 온톨로지(Ontology) 준수 여부, 센서 구성의 허용 범위 유지 여부, 시나리오 정의의 구조적 규칙 충족 여부를 검증할 수 있다. 이러한 저비용 검사는 많은 오류를 상당한 GPU 자원을 소비하지 않고 발견할 수 있으므로 시뮬레이션 실행 전에 수행해야 한다.

다음 CI 단계에서는 소규모의 대표적인 시뮬레이션 테스트(Representative Simulation Test)를 실행할 수 있다. 전체 데이터셋을 생성하는 대신 선택된 시나리오에서 제한된 수의 프레임 또는 에피소드를 생성한다. 이러한 스모크 테스트(Smoke Test)를 통해 환경이 정상적으로 로드되는지, 로봇이 초기화되는지, 센서가 출력을 생성하는지, 어노테이션이 만들어지는지, 좌표 변환(Coordinate Transform)이 유지되는지, 출력 디렉터리가 예상 구조를 따르는지를 확인할 수 있다.

시뮬레이션 실행에 성공하면 데이터 검증(Data Validation)이 이어진다. 자동 검사는 누락된 프레임, 비어 있는 포인트 클라우드, 유효하지 않은 깊이 값, 손상된 이미지, 잘못된 크기, 누락된 라벨, 유한하지 않은 좌표(Non-Finite Coordinate), 일관되지 않은 타임스탬프 또는 센서 데이터와 어노테이션 사이의 손상된 관계를 탐지할 수 있다. 실패한 샘플과 작업은 자동으로 격리하여 유효하지 않은 출력이 승인된 학습 데이터셋에 들어가지 않도록 해야 한다.

교차 모달 검증(Cross-Modal Validation)은 다중모달 합성 데이터에 대해 더욱 강력한 품질 보증을 제공한다. 깊이 값을 장면 형상과 비교하고, 포인트 클라우드를 카메라 이미지에 투영하며, 분할 라벨을 가시 객체와 비교하고, 로봇 자세를 좌표 변환과 대조하여 검증할 수 있다. 이러한 테스트는 개별 파일이 단순히 독립적으로 유효한지를 넘어 서로 관련된 모달리티 사이에서 공간적·시간적 일관성(Spatial and Temporal Consistency)을 유지하는지를 확인한다.

통계적 검증(Statistical Validation)은 생성 작업에서 만들어진 데이터 분포를 분석한다. 클래스 빈도, 객체 수, 자세 분포, 조명 범위, 센서 거리, 포인트 밀도(Point Density), 가림 수준(Occlusion Level), 시나리오 커버리지를 예상 범위와 비교할 수 있다. 이를 통해 파이프라인이 기술적으로 유효한 샘플을 생성하더라도 랜덤화 로직, 에셋 또는 시나리오 구성의 변경으로 인해 의도하지 않은 데이터 분포를 만드는 문제를 탐지할 수 있다.

품질 게이트(Quality Gate)는 생성된 데이터가 다음 수명주기 단계(Lifecycle Stage)로 진행할 수 있는지를 결정한다. 무결성 검사에 실패하거나, 어노테이션 오류가 임계값을 초과하거나, 필수 시나리오가 누락되거나, 분포 통계가 승인 범위를 벗어나면 해당 생성 실행을 거부할 수 있다. 이러한 게이트를 통과하면 원시 생성 출력(Raw Generated Output)은 패키징, 카탈로그 등록, AI 학습에서의 통제된 사용에 적합한 검증된 데이터셋 후보(Validated Dataset Candidate)로 전환된다.

파이프라인 자동화에서는 임시 출력(Temporary Output)과 지속적으로 유지해야 하는 데이터 제품(Durable Data Product)을 구분해야 한다. 중간 렌더링 결과, 시뮬레이터 캐시(Cache), 디버그 로그, 원시 프레임, 검증된 샘플, 파생 학습 형식(Derived Training Format), 보고서는 서로 다른 수명주기 요구사항을 가진다. 성공적인 검증 후 임시 결과는 제거할 수 있지만 필수 메타데이터, 생성 구성, 품질 보고서, 승인된 데이터셋은 재현성과 거버넌스(Governance) 요구사항에 따라 보존해야 한다.

데이터셋 패키징(Dataset Packaging)은 검증된 출력을 후속 AI 시스템이 요구하는 표준화된 구조로 변환한다. 파이프라인은 원본 샘플 및 메타데이터와의 참조 관계를 유지하면서 매니페스트(Manifest), 인덱스(Index), 샤드(Shard), 압축 아카이브 또는 모델별 표현(Model-Specific Representation)을 생성할 수 있다. 동일하게 검증된 입력과 구성을 사용해 변환을 반복할 경우 동등한 학습 산출물이 생성되도록 패키징 과정은 결정론적이어야 한다.

버전 관리(Versioning)는 공개된 각 데이터셋을 이를 생성한 정확한 파이프라인 상태와 연결한다. 데이터셋 버전은 생성 구성, 시뮬레이션 소프트웨어, 장면 에셋, 로봇 모델, 센서 정의, 랜덤화 정책, 어노테이션 스키마, 검증 규칙, 패키징 로직을 참조해야 한다. 이러한 정보를 통해 팀은 성능 변화가 모델이나 학습 설정에서 발생했는지 또는 합성 데이터 생산 과정의 변경에서 발생했는지를 판단할 수 있다.

데이터 계보(Data Lineage)는 이러한 추적성을 이후 학습 단계까지 확장한다. 공개된 데이터셋에는 변경 불가능한 식별자(Immutable Identifier)를 부여할 수 있으며, 학습 매니페스트(Training Manifest)는 각 모델 실험에서 어떤 데이터셋 버전과 하위 집합을 사용했는지 기록할 수 있다. 이후 모델 평가 결과를 학습 데이터, 검증 보고서, 생성 작업, 시뮬레이션 구성까지 역방향으로 추적함으로써 합성 데이터 엔지니어링과 AI 성능 사이의 감사 가능한 관계(Auditable Relationship)를 구축할 수 있다.

CI에는 이전에 승인된 데이터셋 기준선(Dataset Baseline)에 대한 회귀 테스트(Regression Testing)도 포함할 수 있다. 대표적인 생성 실행 결과를 이미지 통계, 포인트 클라우드 특성, 라벨 분포, 시나리오 커버리지 또는 선택된 모델 기반 지표(Model-Based Metric)를 이용해 비교할 수 있다. 큰 차이가 반드시 실패를 의미하는 것은 아니지만 새로운 파이프라인 버전을 승인하기 전에 엔지니어가 해당 변화가 의도적인지 판단할 수 있도록 명확하게 식별해야 한다.

모델 인 더 루프 검증(Model-in-the-Loop Validation)은 추가적인 자동화 수준을 제공한다. 새롭게 생성된 데이터를 이용해 소규모 참조 모델(Reference Model)을 평가하거나 통제된 학습 실험을 통해 새로운 데이터셋과 이전 버전을 비교할 수 있다. 모델 동작의 예상치 못한 변화는 파일 수준 검사나 통계적 검사만으로 탐지하기 어려운 데이터 문제를 발견할 수 있으며, 특히 변경 사항이 작업과 관련된 미세한 특성에 영향을 주는 경우 유용하다.

CI 검증 이후에는 지속적 전달(Continuous Delivery) 원칙을 적용할 수 있다. 승인된 파이프라인 버전은 예약되거나 요청된 데이터셋 릴리스(Dataset Release)를 자동으로 생성할 수 있으며 검증에 실패하면 배포를 차단한다. 운영용 데이터 생성은 실험적인 시뮬레이션 개발과 분리하여 탐색적인 변경이 학습 팀이나 배포된 AI 시스템이 사용하는 관리형 데이터셋에 자동으로 영향을 주지 않도록 해야 한다.

대규모 시뮬레이션 작업은 파이프라인 정의가 정확하더라도 부분적으로 실패할 수 있으므로 모니터링(Monitoring)이 필요하다. 운영 지표(Operational Metric)를 통해 완료된 에피소드, 실패한 작업, 데이터 생성 처리량(Generation Throughput), GPU 사용률, 출력 데이터 규모, 검증 실패율, 처리 지연시간(Processing Latency)을 추적할 수 있다. 경고(Alert)를 통해 비정상적인 조건을 조기에 발견하고 재시도 정책(Retry Policy)을 이용하여 이미 완료되고 검증된 데이터를 다시 생성하지 않으면서 일시적인 실패를 복구할 수 있다.

효율적인 자동화는 변경이 발생할 때마다 전체 데이터셋을 다시 생성하는 대신 증분 생성(Incremental Generation)을 지원해야 한다. 특정 시나리오, 에셋 그룹 또는 센서 구성만 변경되었다면 영향을 받은 파티션(Partition)만 다시 생성하고 변경되지 않은 유효한 데이터는 유지할 수 있다. 따라서 시뮬레이션 입력이나 생성 로직의 변경으로 어떤 데이터셋 구성 요소가 무효화되는지를 판단하기 위한 의존성 추적(Dependency Tracking)이 중요하다.

보안(Security)과 거버넌스도 자동화 과정에 통합해야 한다. 접근 제어(Access Control)를 통해 승인된 생성 구성, 에셋, 검증 규칙, 공개 데이터셋을 수정할 수 있는 사용자를 제한할 수 있다. 에셋 라이선스(Asset License), 출처 정보(Provenance Information), 데이터셋 소유권, 보존 정책(Retention Policy)을 파이프라인 실행 과정에서 검사할 수 있다. 이를 통해 기술적으로 유효한 합성 데이터가 학습 생태계에 포함되기 전에 조직의 요구사항까지 충족하도록 할 수 있다.

전체 자동화 아키텍처는 지속적인 시뮬레이션-데이터-모델 피드백 루프(Continuous Simulation-Data-Model Feedback Loop)를 형성한다. 실제 로봇의 실패와 모델 평가를 통해 누락된 시나리오를 식별하고, 엔지니어는 시뮬레이션 구성이나 랜덤화 정책을 업데이트하며, CI는 변경 사항을 검증한다. 자동화된 생성 과정은 검증된 데이터셋 버전을 생산하고 학습 파이프라인은 이를 사용한다. 이후 새로운 모델을 다시 실제 환경에서 평가함으로써 합성 데이터 생성은 피지컬 AI를 위한 지속적으로 관리되는 데이터 생산 시스템(Continuously Governed Data Production System)으로 발전한다.

## 11.08 Synthetic Data Storage Management: Versioning [w/Code]

![](images/image9.png){width="7.268055555555556in" height="7.268055555555556in"}

합성 데이터 저장 관리(Synthetic Data Storage Management)는 생성된 데이터셋을 시뮬레이션 과정에서 일시적으로 만들어지는 출력이 아니라 관리되는 데이터 제품(Governed Data Product)으로 취급해야 한다. 로보틱스 파이프라인은 수백만 개의 RGB 이미지, 깊이 맵(Depth Map), 분할 마스크(Segmentation Mask), 포인트 클라우드(Point Cloud), 궤적(Trajectory), 로봇 상태(Robot State), 어노테이션(Annotation)을 생성할 수 있다. 명확한 저장 아키텍처(Storage Architecture)가 없으면 이러한 출력은 빠르게 검색, 재현, 검증하거나 이를 생성한 시뮬레이션 구성과 연결하기 어려워진다.

저장 설계(Storage Design)는 데이터를 수명주기(Lifecycle)와 목적에 따라 분리하는 것에서 시작한다. 원시 시뮬레이션 출력(Raw Simulation Output)은 처음 생성된 관측 데이터를 보존하고, 검증된 데이터셋(Validated Dataset)은 품질 검사를 통과한 샘플을 포함하며, 파생 데이터셋(Derived Dataset)은 특정 학습 프레임워크나 모델에 최적화된 형식을 제공한다. 임시 렌더링 파일, 캐시(Cache), 중간 산출물은 재현 가능한 학습 데이터와 보존 요구사항이 다르므로 별도로 관리해야 한다.

논리적 네임스페이스(Logical Namespace)는 데이터셋을 물리적인 저장 위치와 독립적으로 식별할 수 있어야 한다. 데이터셋 이름에는 프로젝트, 작업, 모달리티(Modality), 시나리오 계열 또는 생성 캠페인을 표현할 수 있으며, 변경 불가능한 식별자(Immutable Identifier)를 통해 개별 릴리스를 구분할 수 있다. 이러한 추상화를 이용하면 학습 파이프라인과 실험 기록이 참조하는 데이터셋의 정체성을 변경하지 않고 로컬 스토리지, 네트워크 연결 스토리지(Network-Attached Storage), 객체 스토리지(Object Storage), 아카이브 시스템 사이에서 데이터를 이동할 수 있다.

디렉터리 및 객체 키 구조(Directory and Object-Key Structure)는 데이터셋 세대가 달라져도 예측 가능한 형태를 유지해야 한다. 계층 구조는 데이터셋 버전, 시나리오, 에피소드, 센서, 모달리티, 프레임을 기준으로 데이터를 구성할 수 있으며, 매니페스트(Manifest)는 상위 수준의 인덱스를 제공한다. 일관된 구조는 사용자 정의 로딩 로직을 줄이고 자동화 도구가 수동으로 관리되는 경로에 의존하지 않고 관련 RGB, 깊이, 포인트 클라우드, 어노테이션, 로봇 상태 기록을 검색하도록 한다.

대규모 합성 데이터셋은 개별 파일에 대한 접근 효율이 규모 증가에 따라 낮아질 수 있으므로 파티셔닝(Partitioning)이 유용하다. 데이터는 시나리오, 장면, 에피소드 범위, 모달리티 또는 생성 작업을 기준으로 분할하고 관리 가능한 크기의 샤드(Shard)나 아카이브로 패키징할 수 있다. 적절한 파티셔닝은 수백만 개의 작은 독립 파일이 하나의 거대한 디렉터리에 저장되는 문제를 방지하면서 병렬 학습 접근, 데이터 전송 효율, 검증, 복구 성능을 향상시킨다.

저장 형식(Storage Format)은 각 모달리티의 특성을 반영해야 한다. 이미지는 표준 압축 이미지 형식을 사용할 수 있고, 포인트 클라우드는 바이너리 또는 구조화된 표현이 필요할 수 있으며, 수치형 로봇 상태 데이터에는 열 지향(Columnar) 또는 배열 지향(Array-Oriented) 형식을 사용할 수 있다. 저장 전략은 데이터 충실도(Fidelity), 압축률, 디코딩 비용, 임의 접근(Random Access) 요구사항, 상호운용성(Interoperability), 재현 가능한 로보틱스 실험에 필요한 메타데이터 보존 능력 사이의 균형을 고려해야 한다.

압축(Compression)은 저장 공간 요구량을 크게 줄일 수 있지만 선택한 방식은 후속 데이터 접근 패턴과 일치해야 한다. 최대 압축은 저장 용량을 절약할 수 있지만 CPU 부하와 학습 지연시간(Training Latency)을 증가시킬 수 있다. 자주 사용하는 데이터셋은 빠른 압축 해제를 우선할 수 있고 콜드 아카이브(Cold Archive)는 저장 효율을 우선할 수 있다. 따라서 압축 설정은 데이터셋 구성의 일부로 취급하고 공개되는 각 버전과 함께 기록해야 한다.

메타데이터(Metadata)는 저장된 파일과 그 공학적 의미를 연결한다. 데이터셋 수준 메타데이터(Dataset-Level Metadata)는 목적, 생성 시간, 시뮬레이션 환경, 모달리티, 스키마 버전, 시나리오 커버리지, 품질 상태를 설명할 수 있다. 샘플 수준 메타데이터(Sample-Level Metadata)는 타임스탬프, 프레임 식별자, 센서 구성, 객체 상태, 랜덤 시드(Random Seed), 생성 파라미터를 기록할 수 있다. 검색, 디버깅, 필터링, 재현성을 위해 두 수준의 메타데이터가 모두 필요하다.

데이터셋 매니페스트(Dataset Manifest)는 데이터 제품에 대한 기계 판독 가능 인벤토리(Machine-Readable Inventory)를 제공한다. 전체 저장 계층을 애플리케이션이 직접 검색하지 않고도 샘플 위치, 파티션, 체크섬(Checksum), 모달리티, 라벨, 시나리오 식별자, 검증 상태를 참조할 수 있다. 학습 파이프라인은 매니페스트를 이용하여 재현 가능한 하위 집합을 구성하고, 검증 및 거버넌스 시스템은 동일한 기록을 이용해 데이터셋의 완전성과 출처(Provenance)를 확인할 수 있다.

내구성이 필요한 데이터셋 산출물에는 체크섬을 생성하여 손상이나 의도하지 않은 변경을 탐지할 수 있도록 해야 한다. 생성, 전송, 아카이빙, 복원 또는 복제 이후에 무결성 검증(Integrity Verification)을 수행할 수 있다. 매우 큰 데이터셋에서는 운영 요구사항에 따라 파일, 샤드 또는 파티션 수준에서 체크섬을 관리할 수 있다. 합성 데이터셋을 여러 저장 계층이나 사이트로 복사하는 경우 무결성 메타데이터(Integrity Metadata)는 특히 중요한 역할을 한다.

버전 관리(Versioning)는 단순한 파일 이름 변경이 아니라 데이터에 발생한 의미 있는 변화를 표현해야 한다. 생성 파라미터, 장면 에셋(Scene Asset), 센서 모델, 어노테이션 스키마(Annotation Schema), 랜덤화 정책(Randomization Policy), 검증 규칙 또는 샘플 구성이 변경되면 새로운 버전이 필요할 수 있다. 해당 버전은 모델 실험에서 사용한 전체 논리적 데이터셋 상태를 표현하여 이후 동일한 학습 입력을 다시 구성할 수 있도록 해야 한다.

변경 불가능한 데이터셋 릴리스(Immutable Dataset Release)는 재현성을 위한 강력한 기반을 제공한다. 검증된 버전이 공개되고 모델 학습에서 참조되기 시작하면 그 내용을 암묵적으로 수정해서는 안 된다. 수정 사항은 새로운 버전으로 릴리스하고 이전 버전은 계속 식별할 수 있도록 유지해야 한다. 이를 통해 동일한 데이터셋 이름과 연결된 파일이 교체되거나 수정되어 이후 동일한 실험에서 다른 결과가 발생하는 문제를 방지할 수 있다.

데이터셋 버전 관리는 생성 구성 버전 관리(Generation Configuration Versioning)와 연결해야 한다. 공개된 데이터셋은 생성 과정에 사용된 정확한 시뮬레이터 버전, 시나리오 구성, 랜덤화 정책, 로봇 모델, 센서 정의, 장면 에셋, 어노테이션 스키마, 파이프라인 코드를 참조할 수 있다. 이러한 관계는 저장된 데이터 제품의 버전과 이를 생성한 구성 요소의 버전을 구분하면서 전체 추적성(Traceability)을 보존한다.

데이터 계보(Data Lineage)는 버전 정보를 변환 관계 그래프(Transformation Graph)로 확장한다. 검증된 데이터셋은 여러 생성 작업으로부터 만들어질 수 있고, 학습 데이터셋은 여러 검증된 파티션을 결합할 수 있으며, 모델별 데이터셋은 필터링, 크기 조정, 변환 또는 데이터 증강(Augmentation)을 통해 파생될 수 있다. 이러한 부모-자식 관계(Parent-Child Relationship)를 기록하면 엔지니어는 하나의 학습 샘플을 모든 변환 단계를 거쳐 원래 시뮬레이션까지 추적할 수 있다.

저장 시스템은 불필요한 물리적 복제 대신 참조(Reference)를 지원해야 한다. 서로 다른 학습 실험에는 다른 하위 집합, 클래스 균형, 시나리오 혼합 또는 합성-실제 데이터 조합이 필요할 수 있지만 각 조합마다 기본 파일 전체를 복사할 필요는 없다. 인프라가 허용하는 경우 매니페스트, 뷰(View), 인덱스 계층(Index Layer)을 이용해 논리적 데이터셋을 정의하면서 공유되는 변경 불가능 객체는 한 번만 저장할 수 있다.

중복 제거(Deduplication)는 시뮬레이션 캠페인에서 동일한 에셋이나 생성 산출물을 반복적으로 사용할 때 저장 용량을 추가로 줄일 수 있다. 콘텐츠 해시(Content Hash)를 이용해 중복 객체를 식별할 수 있으며 공유되는 장면 리소스와 메타데이터는 모든 데이터셋 릴리스에 복사하지 않고 참조할 수 있다. 저장 최적화가 재현성을 약화시키거나 과거 버전이 변경 가능한 외부 파일에 의존하도록 만들지 않도록 중복 제거 과정은 데이터셋 정체성에 대해 투명하게 동작해야 한다.

계층형 저장 아키텍처(Tiered Storage Architecture)는 데이터셋 사용 패턴에 따라 비용과 성능을 조정할 수 있다. 학습에 사용되는 활성 데이터셋(Active Dataset)은 컴퓨팅 자원과 가까운 고처리량 스토리지가 필요하고, 최근 완료된 데이터셋은 상대적으로 저렴한 웜 스토리지(Warm Storage)에 저장할 수 있다. 오래된 검증 버전, 원시 시뮬레이션 출력, 감사 기록(Audit Record)은 카탈로그를 통해 검색 가능하고 필요할 때 복원할 수 있는 상태로 콜드 아카이브 계층으로 이동할 수 있다.

보존 정책(Retention Policy)은 각 합성 데이터 유형을 얼마나 오래 유지해야 하는지를 결정한다. 임시 시뮬레이션 캐시는 빠르게 삭제할 수 있고 실패한 생성 출력은 디버깅 목적으로 제한된 기간만 보관할 수 있으며, 중요한 모델에서 사용된 검증 데이터 릴리스는 장기간 보존해야 할 수 있다. 정책은 재현성, 규제 의무, 에셋 라이선스, 저장 비용, 모델 수명주기, 과거 데이터셋이 조사에 필요할 가능성을 고려해야 한다.

삭제(Deletion)는 보존과 마찬가지로 신중하게 관리해야 한다. 배포된 모델, 실험, 검증 보고서 또는 파생 데이터셋이 참조하는 데이터셋을 제거하면 데이터 계보와 재현성이 손상될 수 있다. 삭제 전에 의존성 검사(Dependency Check)를 통해 후속 참조 관계를 식별해야 한다. 물리적인 제거가 필요한 경우에도 과거 데이터 제품에 대한 지속적인 증거로서 핵심 매니페스트, 메타데이터, 품질 보고서, 데이터 계보 기록을 보존해야 할 수 있다.

데이터셋 카탈로그(Dataset Catalog)는 대규모 합성 데이터 저장소를 사람과 자동화 시스템 모두가 사용할 수 있도록 만든다. 카탈로그 항목은 데이터셋 식별자, 버전, 모달리티, 시나리오 커버리지, 크기, 품질 상태, 생성 구성, 소유권, 저장 위치를 제공할 수 있다. 검색 가능한 메타데이터(Searchable Metadata)를 통해 엔지니어는 중복 데이터를 새로 생성하기 전에 기존 데이터를 발견할 수 있으며, 학습 시스템은 작업 및 품질 요구사항에 적합한 데이터셋을 선택할 수 있다.

접근 제어(Access Control)는 적절한 저장 및 데이터셋 경계에 적용해야 한다. 실제 개인 데이터가 포함되지 않은 합성 데이터라도 라이선스가 적용된 상업용 에셋, 독점적인 로봇 설계, 고객별 환경 또는 제한된 시뮬레이션 시나리오를 포함할 수 있다. 역할(Role)과 권한(Permission)을 통해 데이터셋 버전을 생성, 수정, 공개, 조회, 아카이빙 또는 삭제할 수 있는 사용자를 통제하고 감사 로그(Audit Log)를 통해 중요한 관리 작업을 기록할 수 있다.

백업 및 복제 전략(Backup and Replication Strategy)은 데이터셋을 다시 생성하는 비용을 고려해야 한다. 합성 데이터는 이론적으로 다시 생성할 수 있지만 재생성에는 많은 GPU 시간, 더 이상 사용할 수 없는 소프트웨어 버전, 라이선스 에셋 또는 과거 구성이 필요할 수 있다. 따라서 중요한 검증 데이터셋과 메타데이터는 복제를 적용할 가치가 있으며, 쉽게 재생성할 수 있는 중간 산출물은 복구 가치(Recovery Value)에 따라 상대적으로 저렴한 보호 방식을 사용할 수 있다.

저장 모니터링(Storage Monitoring)은 용량과 운영 상태에 대한 가시성을 제공한다. 시스템은 데이터셋 증가량, 모달리티별 데이터 규모, 샤드 수, 복제 상태, 접근 빈도, 전송 처리량(Transfer Throughput), 읽기 실패, 무결성 검사 결과, 저장 계층 사용률을 추적할 수 있다. 이러한 측정값은 비효율적인 생성 패턴을 식별하고 저장 공간의 제약으로 AI 개발이 중단되기 전에 압축, 이동, 보존 또는 확장에 관한 의사결정을 지원한다.

학습 통합(Training Integration)은 변경 가능한 디렉터리 이름이 아니라 변경 불가능한 버전 또는 매니페스트를 기준으로 데이터셋을 해석해야 한다. 학습 작업은 데이터셋 식별자, 버전, 선택된 파티션, 전처리 구성, 합성-실제 데이터 혼합 정보를 실험 메타데이터에 기록할 수 있다. 이후 모델 산출물(Model Artifact)은 이러한 참조 정보를 유지하여 평가 결과와 배포된 모델 동작을 학습에 사용된 정확한 데이터 구성까지 직접 추적할 수 있도록 한다.

버전 비교(Version Comparison)를 통해 생성 주기에 따라 합성 데이터가 어떻게 변화하는지를 확인할 수 있다. 보고서는 릴리스 사이의 샘플 수, 클래스 분포, 시나리오 커버리지, 센서 구성, 도메인 갭 지표(Domain-Gap Metric), 검증 결과, 저장 용량을 비교할 수 있다. 이러한 비교를 통해 새로운 버전이 커버리지를 확장했는지, 결함을 수정했는지, 분포를 변경했는지 또는 모델 동작에 영향을 줄 수 있는 의도하지 않은 차이를 도입했는지를 이해할 수 있다.

저장 관리는 궁극적으로 시뮬레이션 엔지니어링(Simulation Engineering)과 모델 거버넌스(Model Governance)를 연결한다. 모델은 단순히 '합성 데이터(Synthetic Data)'를 참조하는 것이 아니라 정확하게 식별되고, 검증되고, 변경 불가능하며, 추적 가능한 데이터셋 구성을 참조해야 한다. 구조화된 저장, 매니페스트, 무결성 검증, 버전 관리, 데이터 계보, 카탈로그화, 수명주기 정책, 접근 제어를 결합하면 합성 데이터는 단순히 축적된 시뮬레이션 파일이 아니라 재현 가능한 엔지니어링 자산(Reproducible Engineering Asset)이 된다.

결과적으로 이러한 아키텍처는 데이터 생성에서 아카이빙까지 이어지는 지속적인 데이터 수명주기(Continuous Data Lifecycle)를 형성한다. 시뮬레이션은 원시 관측 데이터를 생성하고, 검증 과정은 허용 가능한 샘플을 승격하며, 패키징은 관리되는 데이터셋 버전을 생성하고, 카탈로그는 이를 검색 가능하게 만든다. 학습은 변경 불가능한 참조를 사용하고 모델 평가는 데이터의 유용성에 대한 근거를 생성한다. 피드백은 새로운 데이터셋 버전을 유도하고 데이터 계보는 이력을 보존함으로써 피지컬 AI(Physical AI) 시스템이 모델을 뒷받침하는 데이터 기반에 대한 통제력을 잃지 않으면서 지속적으로 발전할 수 있도록 한다.

## 11.09 Synthetic Data Legal and Ethical Considerations

![](images/image10.png){width="7.268055555555556in" height="7.268055555555556in"}

합성 데이터(Synthetic Data)는 대규모 로보틱스 데이터셋을 수집하는 비용과 어려움을 줄일 수 있지만, 인공적으로 생성되었다는 사실이 법적 또는 윤리적 책임을 없애는 것은 아니다. 합성 데이터셋에는 여전히 저작권이 있는 에셋, 라이선스가 적용된 시뮬레이션 환경, 독점적인 로봇 모델, 개인의 초상이나 특성을 나타내는 자료, 제한된 원천 데이터 또는 실제 관측 데이터에서 파생된 정보가 포함될 수 있다. 따라서 법적·윤리적 평가는 데이터셋을 공개하거나 배포하기 직전에만 수행하는 별도의 검토가 아니라 합성 데이터 수명주기(Synthetic-Data Lifecycle)의 일부로 취급해야 한다.

데이터 출처 관리(Data Provenance)는 책임 있는 합성 데이터 관리의 출발점이다. 각 데이터셋에는 시뮬레이터, 원본 에셋, 생성 구성(Generation Configuration), 랜덤화 정책(Randomization Policy), 소프트웨어 의존성(Software Dependency), 그리고 시뮬레이션의 설계 또는 보정(Calibration)에 사용된 실제 데이터가 식별되어야 한다. 출처 기록을 통해 조직은 생성된 에셋의 기원이 어디인지, 어떤 사용 권한이 적용되는지, 그리고 후속 학습이나 상업적 이용이 해당 권한과 일치하는지를 확인할 수 있다.

시뮬레이션 에셋을 외부에서 가져오는 경우 저작권(Copyright)과 지식재산권(Intellectual Property Rights)에 특히 주의해야 한다. 3D 모델, 텍스처, 환경, 음향 파일, 로봇 정의, 소프트웨어 구성 요소는 저작권, 계약상 제한 또는 기타 지식재산권의 보호를 받을 수 있다. 어떤 에셋을 기술적으로 시뮬레이터에 가져올 수 있다는 사실만으로 해당 에셋을 합법적으로 재배포하거나 상용 데이터셋에 포함하거나 모든 종류의 학습 목적에 사용할 수 있다는 의미는 아니다.

따라서 라이선스 정보(Licensing Information)는 에셋 및 데이터셋 메타데이터와 함께 보존해야 한다. 각 외부 에셋에는 라이선스, 출처, 허용된 사용 범위, 저작자 표시(Attribution) 요구사항, 제한사항, 적용 버전을 연결할 수 있다. 데이터셋 패키징 과정에서 이러한 정보를 제거해서는 안 된다. 서로 다른 라이선스 조건을 가진 에셋을 하나의 데이터셋에 결합하는 경우에는 전체 데이터셋을 하나의 복합 제품(Composite Product)으로 검토하여 가장 제한적인 적용 조건이 누락되지 않도록 해야 한다.

합성 데이터가 실제 데이터를 기반으로 생성되는 경우에는 다른 종류의 법적 고려사항이 발생한다. 시뮬레이터는 현실적인 환경을 구성하거나 노이즈 모델(Noise Model)을 보정하기 위해 실제 이미지, 측정값, 궤적, 지도 또는 센서 통계를 사용할 수 있다. 최종 결과물이 합성 데이터라고 하더라도 원천 정보에는 개인정보 보호, 계약상 제한, 기밀성(Confidentiality), 지식재산권 또는 데이터 보호(Data Protection)와 관련된 요구사항이 계속 적용될 수 있다. 따라서 조직은 원천 데이터와 생성된 출력 사이의 관계를 문서화해야 한다.

개인정보 위험(Privacy Risk)은 합성 환경에 사람을 표현하는 요소가 포함될 때 특히 주의해야 한다. 인간 모델, 얼굴, 신체적 특성, 음성 또는 행동 패턴이 실제 데이터에서 파생되거나 실제 개인과 유사하게 의도적으로 설계될 수 있다. 합성 생성은 개인 정보에 대한 직접적인 노출을 줄일 수 있지만 모든 합성 표현이 자동으로 개인정보 보호와 관련된 법적 또는 윤리적 고려에서 자유롭다고 가정해서는 안 된다.

생체정보(Biometric Information)와 민감한 속성(Sensitive Attribute)이 합성 데이터셋에 표현되거나 추론되는 경우에는 추가적인 통제가 필요하다. 인지(Perception) 또는 인간-로봇 상호작용(Human-Robot Interaction)을 위한 시스템은 얼굴, 신체 자세, 연령과 관련된 외형, 의복 또는 기타 인간 특성에 대한 변형을 생성할 수 있다. 이러한 변형의 목적은 명확하게 정의되어야 하며 불필요한 민감 특성의 표현은 피해야 한다. 데이터 생성은 실제 로봇 작업 및 평가 요구사항에 비례하는 수준으로 유지되어야 한다.

실제 개인을 참조 자료로 사용하는 경우 인간 표현(Human Representation)은 동의(Consent)와 초상 및 유사성(Likeness)에 관한 고려사항도 발생시킨다. 사진, 스캔, 모션 캡처(Motion Capture), 음성 또는 기타 식별 가능한 자료가 합성 에셋에 기여한다면 생성 전에 허용되는 사용 목적과 범위를 명확히 해야 한다. 특정 활동을 위해 얻은 동의가 관련 없는 상업적, 연구, 학습 또는 공개 배포 목적에 대한 허가를 자동으로 의미하는 것은 아니다.

편향(Bias)은 개인정보가 직접 사용되지 않더라도 합성 환경의 설계 과정에서 도입될 수 있다. 에셋 선택, 객체 분포, 인간 표현, 환경에 대한 가정, 조명 조건, 시나리오 빈도에는 세상이 어떠해야 하는지 또는 사람이 어떻게 행동해야 하는지에 대한 특정 가정이 반영될 수 있다. 따라서 품질 평가는 생성 데이터셋이 중요한 환경을 체계적으로 과소대표하거나 후속 모델의 동작에 영향을 줄 수 있는 왜곡된 표현을 생성하는지 검토해야 한다.

합성 데이터는 라벨이 자동으로 생성되기 때문에 인위적인 객관성의 인상을 만들 수도 있다. 완벽한 정답 라벨(Ground-Truth Label)이 존재한다고 해서 기본적인 시나리오 설계가 중립적이거나 현실적이거나 적절하다는 의미는 아니다. 시뮬레이터는 기술적으로 정확한 라벨을 실제 운용을 제대로 대표하지 못하는 상황에 부여할 수도 있다. 따라서 윤리적 검토(Ethical Review)는 라벨의 정확성뿐 아니라 해당 시나리오 자체가 정당하고 관련성 높은 사용 사례를 표현하는지도 고려해야 한다.

안전(Safety)은 로보틱스의 합성 데이터 생성에서 또 다른 중요한 고려사항이다. 합성 데이터셋은 실제 환경과 상호작용하는 인지, 내비게이션, 조작 또는 제어 시스템을 학습하는 데 사용될 수 있다. 드물고 위험한 시나리오는 학습과 테스트를 위해 적절하게 과대표현할 수 있지만 생성된 행동을 실제 물리적 안전에 대한 증거로 해석해서는 안 된다. 시뮬레이션 결과는 적절한 물리적 테스트를 통해 검증된 증거와 명확하게 구분해야 한다.

안전과 관련된 모델에 합성 데이터가 사용되는 경우 추적성(Traceability)은 특히 중요하다. 조직은 어떤 데이터셋 버전, 생성 구성, 시뮬레이션 환경, 에셋 집합이 해당 모델에 기여했는지를 식별할 수 있어야 한다. 이후 문제가 있는 에셋, 시나리오 또는 생성 규칙이 발견되면 데이터 계보(Lineage)를 통해 영향을 받은 데이터셋과 모델을 식별하고 재학습 또는 추가 검증이 필요한지 판단할 수 있어야 한다.

투명성(Transparency)은 합성 데이터셋의 실제 사용자에게까지 확장되어야 한다. 데이터셋 문서에는 해당 데이터가 합성 데이터라는 사실, 적절한 수준의 생성 방법, 중요한 한계, 그리고 실제 데이터가 생성 과정의 설계·보정·검증에 사용되었는지를 명시해야 한다. 명확한 문서화는 후속 사용자가 합성 관측 데이터를 물리적 세계에서 직접 측정된 데이터로 잘못 취급하는 것을 방지한다.

합성 데이터 거버넌스(Synthetic-Data Governance)는 계약상 및 조직상의 책임도 포함해야 한다. 데이터셋이 내부에서 생성되더라도 라이선스가 적용된 시뮬레이션 소프트웨어, 외부 에셋, 클라우드 서비스, 상용 모델 또는 고객이 제공한 환경에 의존할 수 있다. 계약에는 데이터 처리, 재배포, 파생 저작물(Derivative Work), 모델 학습 또는 공개에 대한 제한이 포함될 수 있다. 이러한 조건은 별도의 법률 문서에만 보관하지 말고 데이터셋 거버넌스에 포함해야 한다.

합성 데이터가 여러 국가의 팀에 의해 생성, 저장, 이전 또는 사용되는 경우 국경 간 및 관할권(Jurisdiction) 관련 고려사항도 발생할 수 있다. 적용되는 요구사항은 원천 데이터의 출처, 데이터 처리 위치, 계약 관계, 표현된 정보의 성격에 따라 달라질 수 있다. 조직은 관련 관할권을 식별하고 모든 지역에서 동일한 법적 취급을 받는다고 가정하지 않아야 한다.

윤리적 검토는 의도된 응용 분야와 잠재적 영향에 비례하여 수행해야 한다. 기본적인 객체 검출 연구를 위한 합성 데이터셋은 감시(Surveillance), 작업장 모니터링, 인간-로봇 상호작용 또는 안전이 중요한 자율 제어에 사용되는 데이터셋과 서로 다른 우려사항을 가질 수 있다. 검토에서는 시스템의 목적, 영향을 받는 사람, 잠재적인 오용, 예측 가능한 결과, 그리고 선택한 데이터 생성 방식이 해당 목적에 필요한지 여부를 고려해야 한다.

데이터셋 문서화(Dataset Documentation)는 이러한 문제를 관리하는 실질적인 통제 수단이 될 수 있다. 합성 데이터 기록에는 출처, 에셋 라이선스, 원천 데이터 의존성, 개인정보 분류(Privacy Classification), 의도된 사용, 금지된 사용, 생성 구성, 품질 상태, 알려진 한계, 책임자, 승인 상태를 포함할 수 있다. 이러한 정보를 데이터셋 버전과 함께 유지하면 법적·윤리적 요구사항을 엔지니어링, 연구, 운영 팀이 직접 확인할 수 있다.

자동화된 거버넌스 검사(Automated Governance Check)는 이러한 요구사항을 데이터 생성 파이프라인에 통합할 수 있다. 공개 전에 시스템은 필수 라이선스 메타데이터가 존재하는지, 제한된 에셋이 제외되었는지, 승인된 구성이 사용되었는지, 개인정보 분류가 존재하는지, 필수 문서가 작성되었는지를 자동으로 확인할 수 있다. 거버넌스 검사를 통과하지 못한 데이터셋은 문제가 검토되고 해결될 때까지 승인된 학습 저장소(Training Repository)에 들어가지 않도록 해야 한다.

버전 관리(Version Control)는 데이터셋 구성이 변경될 때 법적·윤리적 상태가 달라질 수 있기 때문에 필요하다. 새로운 에셋 추가, 인간 표현의 변경, 새로운 참조 데이터셋의 도입, 라이선스 변경, 의도된 사용 범위의 확대는 시뮬레이션 소프트웨어 자체가 변경되지 않았더라도 새로운 검토를 요구할 수 있다. 따라서 데이터셋 버전 관리는 기술적 데이터 계보뿐 아니라 거버넌스 이력(Governance History)도 보존해야 한다.

책임 있는 합성 데이터 관리(Responsible Synthetic-Data Management)는 통제된 접근 및 삭제 절차(Deletion Procedure)도 필요로 한다. 제한된 에셋이나 원천 데이터에서 파생된 자료는 승인된 사용자만 접근할 수 있어야 하며, 파생 데이터셋에도 적용 가능한 처리 요구사항을 이어받아야 한다. 에셋이 철회되거나 사용 권한이 만료되면 의존성 추적(Dependency Tracking)을 통해 영향을 받는 데이터셋 버전을 식별하고 필요한 경우 추가 사용을 차단해야 한다. 삭제 과정에서도 과거의 책임성을 유지하기 위해 충분한 데이터 계보 기록을 보존해야 한다.

합성 데이터와 실제 데이터의 관계는 전체 수명주기에서 명확하게 유지되어야 한다. 합성 데이터셋은 학습, 검증 또는 도메인 적응(Domain Adaptation)을 위해 실제 관측 데이터와 결합될 수 있으며, 최종 모델은 두 데이터 소스의 특성을 모두 물려받을 수 있다. 학습 매니페스트(Training Manifest)는 이러한 관계를 기록하여 어떤 데이터가 인공적으로 생성되었고 어떤 데이터가 실제 환경에서 관측되었는지, 그리고 두 데이터가 어떤 방식으로 결합되었는지를 구분할 수 있도록 해야 한다.

성숙한 거버넌스 프로세스(Mature Governance Process)는 법적·윤리적 고려사항을 일회성 승인 단계가 아니라 지속적인 통제로 취급한다. 새로운 시뮬레이션 에셋, 원천 데이터셋, 생성 방법, 모델 응용 분야, 배포 환경은 새로운 위험을 발생시킬 수 있다. 정기적인 검토, 데이터셋 감사(Dataset Audit), 데이터 계보 검사, 모델 영향 평가(Model-Impact Assessment)를 통해 합성 데이터 생태계가 변화함에 따라 통제 체계를 업데이트할 수 있다.

전체 프레임워크는 출처 관리, 라이선싱(Licensing), 개인정보 보호, 동의, 편향, 안전, 투명성, 거버넌스, 접근 제어, 수명주기 관리를 하나의 체계로 연결한다. 합성 데이터는 그 기원, 허용된 사용 범위, 한계, 변환 과정, 후속 의존성을 입증할 수 있을 때 책임 있는 엔지니어링 자산(Responsible Engineering Asset)이 된다. 이러한 접근 방식을 통해 로보틱스 조직은 시뮬레이션의 확장성을 확보하면서도 피지컬 AI(Physical AI) 데이터 수명주기 전반에서 적절한 법적, 윤리적, 운영상의 책임성을 유지할 수 있다.

## 11.10 Model Performance Improvement via Synthetic Data Case

![](images/image11.png){width="7.268055555555556in" height="7.268055555555556in"}

합성 데이터(Synthetic Data)는 실제 환경에서 효율적으로 수집할 수 있는 범위를 넘어 학습 분포(Training Distribution)를 확장함으로써 로봇 AI 모델 성능을 향상시킬 수 있다. 성능 향상 사례(Performance-Improvement Case)는 먼저 기준 모델(Baseline Model)의 명확한 약점을 식별하는 것에서 시작해야 한다. 예를 들어 객체 다양성 부족, 어려운 조명 조건에서의 낮은 성능, 제한적인 시점(Viewpoint), 희귀 사건(Rare Event) 샘플 부족, 특정 센서에서 발생하는 오류 등이 이에 해당할 수 있다. 이후 합성 데이터는 단순히 데이터셋의 크기를 증가시키는 것이 아니라 이러한 약점을 해결하기 위한 통제된 수단으로 도입된다.

기준 모델은 먼저 독립적인 실제 환경 검증 데이터셋(Real-World Validation Dataset)을 사용하여 평가해야 한다. 평가 지표는 검출 정밀도와 재현율(Detection Precision and Recall), 분할 품질(Segmentation Quality), 위치 추정 오차(Localization Error), 추적 정확도(Tracking Accuracy), 깊이 추정 오차(Depth Estimation Error) 또는 작업별 성공률(Task-Specific Success Rate) 등을 포함할 수 있다. 또한 객체 클래스, 거리, 조명, 가림(Occlusion), 시점, 환경, 센서 상태와 같은 관련 조건별로 평가를 분리해야 한다. 이를 통해 합성 데이터가 기여한 정도를 측정할 수 있는 명확한 기준점(Reference)을 확보할 수 있다.

다음 단계에서는 시뮬레이션을 통해 현실적으로 해결할 수 있는 성능 격차(Performance Gap)를 식별한다. 예를 들어 인지 모델(Perception Model)이 일반적인 객체에서는 충분한 성능을 보이지만 객체가 부분적으로 가려지거나 일반적이지 않은 시점에서 관찰될 때 실패할 수 있다. 실제 데이터셋에는 효과적인 학습을 지원할 만큼 이러한 조건의 사례가 충분하지 않을 수 있다. 시뮬레이션은 이러한 특정 조건을 대상으로 추가 샘플을 의도적으로 생성하면서 정밀한 정답 어노테이션(Ground-Truth Annotation)을 유지할 수 있다.

합성 데이터 생성은 식별된 실패 모드(Failure Mode)에 의해 통제되어야 한다. 장면 구성, 객체 배치, 카메라 시점, 조명, 재질, 텍스처, 센서 노이즈, 가림, 환경 조건을 정의된 분포에 따라 변화시킬 수 있다. 도메인 랜덤화(Domain Randomization)는 비현실적인 샘플을 생성하지 않으면서 충분한 다양성을 제공해야 한다. 목표는 무작정 변화의 양을 최대화하는 것이 아니라 기준 모델이 측정 가능한 약점을 보이는 영역에서 유용한 데이터 커버리지를 증가시키는 것이다.

이후 합성 샘플은 명시적인 혼합 학습 전략(Mixed-Training Strategy)을 사용하여 실제 학습 데이터와 결합할 수 있다. 합성-실제 데이터 비율(Synthetic-to-Real Ratio)은 실험 변수로 취급해야 한다. 합성 데이터가 지나치게 많으면 모델이 시뮬레이션 특유의 특성을 학습할 수 있고, 반대로 합성 데이터가 너무 적으면 추가적인 커버리지를 충분히 제공하지 못할 수 있다. 모델 아키텍처와 평가 프로토콜을 일관되게 유지하면서 다양한 샘플링 비율, 가중치 정책 또는 커리큘럼 일정(Curriculum Schedule)을 평가할 수 있다.

통제된 비교(Control Comparison)에는 최소한 실제 데이터 기준선(Real-Data Baseline)과 혼합 데이터 구성(Mixed-Data Configuration)을 포함해야 한다. 합성 데이터 전용 구성(Synthetic-Only Configuration)도 시뮬레이션만으로 해당 작업을 얼마나 학습할 수 있는지를 이해하는 데 유용할 수 있다. 이러한 실험에서는 동일한 모델 아키텍처, 전처리 파이프라인, 필요한 경우 동일한 최적화 설정과 독립적인 실제 환경 테스트 데이터를 사용해야 한다. 이를 통해 실제 성능 향상이 다른 학습 조건으로 인해 발생한 변화가 아니라는 점을 구분할 수 있다.

합성 데이터의 품질은 모델 성능 향상의 원인을 설명하기 전에 검증되어야 한다. 생성된 이미지, 깊이 맵, 포인트 클라우드, 어노테이션, 자세(Pose), 센서 출력은 무결성 및 일관성 검사를 통과해야 한다. 분포 통계, 시나리오 커버리지(Scenario Coverage), 센서 특성, 도메인 갭 측정(Domain-Gap Measurement)을 실제 관측 데이터와 비교할 수 있다. 더 큰 합성 데이터셋이 자동으로 더 유용한 것은 아니며, 비현실적인 패턴을 도입하거나 실제 배치 조건과 크게 다른 분포를 강화할 경우 오히려 문제가 될 수 있다.

혼합 학습 이후에는 전체 성능과 시나리오 수준 성능을 모두 분석해야 한다. 평균 정확도의 향상이 특정 객체 클래스나 운용 조건의 성능 저하를 가릴 수 있다. 반대로 전체적인 향상이 작더라도 합성 데이터가 희귀하지만 중요한 시나리오에서 실패를 크게 감소시킨다면 실질적인 의미를 가질 수 있다. 따라서 오류 분석(Error Analysis)은 성능 변화와 데이터 생성 과정에서 의도적으로 추가한 합성 조건을 연결해야 한다.

유용한 사례 연구(Case Study)는 합성 데이터가 단순한 암기(Memorization)가 아니라 일반화(Generalization)를 향상시키는지도 검토해야 한다. 모델은 학습 데이터에 직접 포함되지 않은 실제 환경, 객체, 시점, 센서 조건에서 테스트해야 한다. 이전에 경험하지 않은 실제 조건에서 성능이 향상된다면 합성 데이터가 단순히 제한된 학습 사례를 맞추는 것이 아니라 유용한 표현(Representation)을 학습하는 데 기여했다는 보다 강한 근거를 제공할 수 있다.

절제 실험(Ablation Experiment)은 합성 데이터 생성의 어떤 요소가 관찰된 성능 향상을 만드는지를 식별할 수 있다. 서로 다른 도메인 랜덤화 범위, 시나리오 구성, 합성-실제 데이터 비율, 센서 노이즈 모델, 객체 라이브러리(Object Library), 데이터 생성량을 비교할 수 있다. 이러한 비교는 성능 향상이 단순히 추가 샘플 때문인지, 더 높은 다양성 때문인지, 향상된 센서 사실성(Sensor Realism) 때문인지, 희귀 사건 커버리지 때문인지, 또는 더욱 정확한 시뮬레이션 조건 때문인지를 판단하는 데 도움을 준다.

학습 파이프라인은 사례 연구 전체에서 완전한 데이터셋 및 실험 계보(Dataset and Experiment Lineage)를 유지해야 한다. 각 모델 결과는 실제 데이터셋 버전, 합성 데이터셋 버전, 생성 구성, 랜덤화 정책, 어노테이션 스키마, 샘플링 비율, 전처리 구성, 학습 파라미터를 참조해야 한다. 이를 통해 성능 향상을 재현할 수 있으며 특정 모델 버전에 어떤 합성 데이터 구성이 기여했는지를 정확하게 확인할 수 있다.

합성 데이터의 경제적 및 운영적 효과도 고려해야 한다. 실제 데이터 수집에는 로봇 운용, 카메라 또는 라이다(LiDAR) 배치, 어노테이션 작업, 시설 접근, 반복 실험이 필요할 수 있다. 반면 시뮬레이션에서는 환경과 시나리오 모델을 구축한 이후 대량의 라벨링된 데이터를 생성할 수 있다. 따라서 중요한 비교 대상은 단순히 생성된 합성 샘플의 개수가 아니라 실제 모델 성능에서 동등한 향상을 얻기 위해 필요한 엔지니어링 비용이다.

성숙한 성능 향상 사례는 모델 평가와 합성 데이터 생성을 연결하는 폐쇄 루프(Closed Loop)를 형성한다. 실제 환경 평가를 통해 실패를 식별하고, 실패 분석을 통해 누락된 데이터 조건을 정의하며, 시뮬레이션을 통해 목표 합성 샘플을 생성하고, 혼합 학습에 이를 반영한 후 독립적인 실제 환경 테스트를 통해 결과 변화를 측정한다. 새로운 평가 결과에 따라 추가적인 합성 데이터 생성이 필요한지를 결정함으로써 시뮬레이션-학습-실제 환경 사이의 반복적인 피드백 프로세스를 구축할 수 있다.

최종 목표는 실제 데이터를 합성 데이터로 대체하는 것이 아니라 시뮬레이션이 측정 가능한 이점을 제공하는 영역에서 활용하는 것이다. 실제 관측 데이터는 물리적 센서, 환경 복잡성, 실제 배치 동작을 표현하는 데 여전히 필수적이며, 합성 데이터는 확장 가능한 커버리지, 정밀한 어노테이션, 통제 가능한 변화, 희귀 조건의 효율적인 생성을 제공한다. 이러한 데이터 소스를 품질 평가, 버전 관리, 데이터 계보, 통제된 실험을 통해 관리한다면 합성 데이터는 로봇 AI 모델의 지속적인 성능 향상을 위한 실질적인 수단이 될 수 있다.
