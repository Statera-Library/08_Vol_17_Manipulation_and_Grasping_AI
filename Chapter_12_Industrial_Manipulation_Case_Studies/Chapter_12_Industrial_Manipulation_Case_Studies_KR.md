**Volume 17 Manipulation and Grasping AI**

# Chapter 12. Industrial Manipulation Case Studies

## 12.01. Bin Picking Random Pose Object Grasp Case

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

산업용 빈 피킹(Industrial Bin Picking)은 컨테이너 내부에 무작위로 배치된 물체를 로봇이 검출하고, 선택하고, 파지하고, 꺼내야 하는 대표적인 로봇 조작(Robot Manipulation) 문제이다. 구조화된 픽앤플레이스(Pick-and-Place) 작업과 달리 각 물체의 위치와 자세(Pose)는 사전에 알려져 있지 않으며, 주변 물체로 인해 가림(Occlusion), 접촉(Contact), 기하학적 제약(Geometric Constraint)이 발생한다. 따라서 성공적인 시스템을 구현하려면 인지(Perception), 파지 계획(Grasp Planning), 모션 계획(Motion Planning), 피드백 제어(Feedback Control)가 하나의 통합된 조작 파이프라인(Manipulation Pipeline)으로 동작해야 한다.

일반적인 응용에서는 제조, 보관 또는 물류 공정에서 직접 공급된 부품들이 빈(Bin) 내부에 들어 있는 상태에서 작업이 시작된다. 부품들은 서로 겹쳐 있거나, 컨테이너 벽에 기대어 있거나, 부분적으로 다른 물체 아래에 묻혀 있거나, 기계적으로 불안정한 자세로 놓여 있을 수 있다. 로봇은 단순히 보이는 모든 물체를 식별하는 것이 아니라 현재 어떤 물체에 접근할 수 있는지를 판단해야 한다. 이러한 특성으로 인해 빈 피킹은 단순한 물체 인식(Object Recognition) 문제가 아니라, 한 번의 성공적인 인출(Extraction)이 남아 있는 물체들의 기하학적 배치를 변화시키는 순차적 의사결정 과정(Sequential Decision Process)이 된다.

인지(Perception)는 일반적으로 빈 상부 또는 로봇에 장착된 RGB-D 카메라(RGB-D Camera), 구조광 센서(Structured-Light Sensor), 스테레오 비전(Stereo Vision), 산업용 3차원 스캐너(Industrial 3D Scanner)를 이용한다. 센서는 컬러 영상(Color Image), 깊이 맵(Depth Map), 포인트 클라우드(Point Cloud)를 생성하며, 이를 이용하여 물체의 표면과 경계를 추정한다. 센서, 로봇 베이스(Robot Base), 엔드 이펙터(End Effector), 작업 셀(Workcell)의 좌표계 사이에 정확한 캘리브레이션(Calibration)이 이루어져야 한다. 시각 검출이 정확하더라도 자세 정보를 로봇 좌표계로 일관되게 변환하지 못하면 실제 조작에는 활용하기 어렵기 때문이다.

무작위 자세(Random Pose)의 물체는 기존 자세 추정(Pose Estimation) 기법에 상당한 어려움을 발생시킨다. 물체는 임의의 롤(Roll), 피치(Pitch), 요(Yaw) 각도로 놓일 수 있으며 표면의 일부만 보일 수도 있다. 반사성이 높은 금속, 어두운 재질, 반복적인 형상, 텍스처가 없는 표면(Textureless Surface)은 깊이 측정의 품질을 더욱 저하시킬 수 있다. 따라서 현대적인 시스템은 대상 부품의 특성에 따라 기하학적 정합(Geometric Registration), 특징점 매칭(Feature Matching), 분할 네트워크(Segmentation Network), 학습 기반 자세 추정기(Learned Pose Estimator), 다중 시점 관측(Multi-View Observation)을 조합하여 사용한다.

물체 형상을 알고 있고 미리 정의된 파지 자세(Grasp Pose)를 검출된 물체 자세에 따라 변환할 수 있다면 6자유도 자세 추정(6-DoF Pose Estimation)이 유용한 표현 방법이 된다. 그러나 항상 완전한 물체 자세를 추정해야 하는 것은 아니다. 파지 지향 인지(Grasp-Oriented Perception)는 대신 현재 보이는 표면 영역 가운데 실현 가능한 그리퍼 구성(Gripper Configuration)을 지원하는 위치를 직접 예측할 수 있다. 이러한 접근 방법은 하나의 물체에 여러 개의 유효한 파지 자세가 존재하거나, 정확한 의미론적 방향(Semantic Orientation)보다 안정적인 인출이 더 중요한 경우에 특히 효과적이다.

파지 계획기(Grasp Planner)는 기하학적 및 물리적 기준을 사용하여 후보 접촉점(Contact Candidate)을 평가한다. 평행 조 그리퍼(Parallel-Jaw Gripper)의 경우 중요한 요소에는 조 개방 폭(Jaw Opening), 표면 법선(Surface Normal), 대척 접촉 품질(Antipodal Contact Quality), 충돌 여유 공간(Collision Clearance), 삽입 깊이(Insertion Depth), 외력에 대한 예상 저항성이 포함된다. 흡착 기반 시스템(Suction-Based System)은 대신 표면 평탄도(Surface Flatness), 밀봉 품질(Seal Quality), 국부 곡률(Local Curvature), 재료 투과성(Material Permeability), 인출 방향(Extraction Direction)을 고려한다. 하이브리드 엔드 이펙터(Hybrid End Effector)는 흡착과 기계식 파지를 결합하여 처리할 수 있는 물체 자세의 범위를 확대할 수 있다.

높은 파지 점수(Grasp Score)가 반드시 성공적인 피킹(Picking)을 보장하는 것은 아니다. 선택된 파지는 로봇 암(Robot Arm), 손목(Wrist), 그리퍼(Gripper), 빈 벽(Bin Wall), 주변 물체와 충돌하지 않으면서 접근 가능해야 한다. 따라서 파지 생성(Grasp Generation)과 모션 실현 가능성(Motion Feasibility)은 함께 평가되어야 한다. 기하학적으로 안정적으로 보이는 파지 후보라 하더라도 역기구학(Inverse Kinematics)이 실패하거나, 관절 제한(Joint Limit)을 초과하거나, 필요한 접근 궤적(Approach Trajectory)이 컨테이너 내부의 점유 공간을 통과한다면 제외되어야 한다.

빈 피킹을 위한 모션 계획(Motion Planning)은 일반적으로 접근(Approach), 파지(Grasp), 인출(Extraction), 이송(Transfer), 배치(Placement) 단계로 구분된다. 접근 궤적은 충돌 여유 공간을 유지하면서 엔드 이펙터를 사전 파지 자세(Pre-Grasp Pose)로 이동시킨다. 최종 삽입 단계에서는 그리퍼가 불확실성이 존재하는 물체 표면 가까이에서 동작하므로 보다 느린 카테시안 운동(Cartesian Motion)이 필요할 수 있다. 파지가 완료된 이후에는 먼저 주변 물체와의 간섭을 최소화하는 방향으로 물체를 인출한 다음 목적지까지 일반적인 충돌 회피 궤적(Collision-Free Trajectory)으로 전환해야 한다.

인출 단계(Extraction Phase)는 외관상 고립되어 보이는 물체라도 주변 부품과 기계적으로 얽혀 있을 수 있기 때문에 특히 중요하다. 마찰(Friction), 갈고리 형태의 구조(Hook), 공동(Cavity), 케이블(Cable), 불규칙한 형상으로 인해 여러 물체가 동시에 움직일 수 있다. 힘·토크 센싱(Force-Torque Sensing)을 이용하면 들어 올리는 과정에서 비정상적인 저항을 감지하고 제어기가 정지하거나 후퇴하거나 인출 방향을 변경하도록 할 수 있다. 이러한 피드백은 과도한 힘으로 인해 제품, 그리퍼, 로봇 또는 컨테이너가 손상되는 것을 방지한다.

파지 검증(Grasp Verification)은 또 하나의 핵심적인 피드백 계층(Feedback Layer)을 제공한다. 진공 압력(Vacuum Pressure)을 이용하여 흡착 상태를 확인할 수 있으며, 그리퍼 위치와 모터 전류(Motor Current)를 이용하여 기계식 파지가 실제로 물체를 잡았는지를 판단할 수 있다. 비전(Vision)을 이용하여 물체가 빈에서 완전히 빠져나왔는지, 의도하지 않은 다른 물체가 함께 인출되지 않았는지도 확인할 수 있다. 검증 과정이 없다면 파지 실패가 후속 공정으로 전파되어 배치 오류, 빈 상태의 장비 로딩(Empty-Machine Loading), 잘못된 생산 수량 계산으로 이어질 수 있다.

신뢰할 수 있는 파지가 존재하지 않는 경우 효과적인 시스템은 낮은 신뢰도의 파지를 반복해서 시도해서는 안 된다. 대신 밀기(Pushing), 분리(Separating), 회전(Rotating), 장애물 물체의 재배치(Relocation)와 같은 능동 조작(Active Manipulation)을 수행하여 새로운 파지 가능 표면을 노출할 수 있다. 이 경우 문제는 단순한 피킹이 아니라 재배치 계획(Rearrangement Planning)으로 확장된다. 이러한 기능은 특히 빈 바닥 부근에서 중요하다. 마지막까지 남아 있는 물체들은 모서리나 벽면에 붙어 파지하기 어려운 자세를 형성하는 경우가 많기 때문이다.

딥러닝(Deep Learning)은 국부적인 기하 구조와 성공적인 로봇 동작 사이의 관계를 대규모 데이터셋(Dataset) 또는 시뮬레이션(Simulation)으로부터 학습함으로써 파지 검출(Grasp Detection)의 성능을 크게 향상시켰다. 네트워크(Network)는 RGB-D 영상이나 포인트 클라우드에서 직접 파지 자세를 예측하고 수천 개의 후보를 빠르게 평가할 수 있다. 합성 데이터 생성(Synthetic Data Generation)은 무작위 물체 자세, 조명 조건, 센서 노이즈(Sensor Noise), 재질, 빈 구성을 자동으로 생성할 수 있기 때문에 모든 실제 장면을 사람이 직접 라벨링(Labeling)하지 않아도 된다는 장점이 있다.

시뮬레이션(Simulation)은 드물게 발생하거나 처리하기 어려운 물체 배치를 체계적으로 시험하는 데에도 활용된다. 수천 또는 수백만 회의 무작위 낙하(Randomized Drop)를 통해 다양한 배치를 생성한 후, 후보 정책(Policy)을 파지 성공률, 충돌률, 사이클 타임(Cycle Time), 빈 비우기 성능(Bin-Clearing Performance) 측면에서 평가할 수 있다. 도메인 무작위화(Domain Randomization)는 시뮬레이션과 현실 사이의 차이를 줄이는 데 도움이 되지만, 실제 마찰, 컴플라이언스(Compliance), 깊이 센서 아티팩트(Depth-Sensor Artifact), 접촉 동역학(Contact Dynamics)을 완벽하게 재현하기는 어렵기 때문에 실제 시스템 검증이 여전히 필요하다.

산업 현장의 사례에서는 깊은 부품 빈 옆에 6축 로봇(Six-Axis Robot)을 배치하고, 상부 3차원 카메라(3D Camera)와 평행 조 또는 흡착식 그리퍼를 구성할 수 있다. 장면을 획득한 후 시스템은 보이는 물체를 분할하고, 후보 파지 구성을 추정하고, 충돌 기하 구조(Collision Geometry)를 이용해 후보를 필터링한 다음 남은 후보의 순위를 결정한다. 가장 높은 순위의 실현 가능한 파지는 로봇 좌표계로 변환되며 이후 접근, 획득(Acquisition), 인출, 검증, 생산 설비로의 전달 과정이 순차적으로 수행된다.

사이클 타임(Cycle Time)은 파지 신뢰성과 함께 고려해야 한다. 최적의 파지를 탐색하는 데 수초가 필요한 매우 정교한 계획기는 충분히 신뢰할 수 있는 해를 즉시 생성하는 단순한 방법보다 전체 생산성을 낮출 수도 있다. 따라서 산업 시스템에서는 단계적 계산(Staged Computation)을 자주 사용한다. 빠른 인지가 가능성이 높은 파지를 제안하고, 계산 비용이 낮은 기하학적 검사를 통해 명백한 실패 후보를 제거하며, 계산량이 큰 계획은 어려운 장면이나 복구 작업(Recovery Operation)에 제한적으로 적용한다.

따라서 성능은 인지 정확도만이 아니라 전체 시스템 수준(System Level)에서 평가되어야 한다. 주요 지표에는 첫 시도 파지 성공률(First-Attempt Grasp Success Rate), 성공적인 인출률(Successful Extraction Rate), 시간당 피킹 횟수(Picks per Hour), 평균 계획 지연시간(Mean Planning Latency), 충돌 빈도(Collision Frequency), 복구 수행 빈도(Recovery Frequency), 완전히 비운 빈의 비율(Bin-Clearing Rate), 평균 잔여 물체 수(Average Number of Residual Objects)가 포함된다. 특히 빈 비우기 비율은 상층의 쉬운 물체에서는 높은 성능을 보이지만 어려운 자세의 물체만 남으면 반복적으로 실패하는 시스템을 구별할 수 있다는 점에서 중요하다.

신뢰성(Reliability)은 불확실성(Uncertainty)을 명시적으로 처리하는 능력에도 좌우된다. 깊이 측정, 캘리브레이션, 물체 자세, 그리퍼 정렬, 마찰, 로봇 위치에는 모두 오차가 존재한다. 따라서 강건한 파지 계획(Robust Grasp Planning)은 충돌 경계에 매우 가까운 이론적으로 최적인 접촉보다 충분한 기하학적 여유(Geometric Margin)를 확보한 구성을 선호한다. 엔드 이펙터 또는 로봇 제어기의 컴플라이언스(Compliance)를 활용하면 작은 정렬 오차를 흡수하고 삽입 및 파지 과정에서 발생하는 접촉력을 줄일 수 있다.

복구 동작(Recovery Behavior)은 실험실 수준의 데모(Laboratory Demonstration)와 실제 생산용 시스템(Production-Ready System)을 구분하는 중요한 요소이다. 흡착 실패, 불완전한 그리퍼 폐쇄, 예상하지 못한 접촉, 도달 불가능한 자세, 인지 손실(Perception Loss), 물체 낙하와 같은 상황마다 정의된 대응 절차가 필요하다. 로봇은 장면을 다시 획득하거나, 다른 파지를 선택하거나, 접근 방향을 변경하거나, 재배치 동작을 수행하거나, 반복적인 실패 이후 작업자 지원(Operator Assistance)을 요청할 수 있다. 이러한 복구 로직(Recovery Logic)은 모든 예외 상황을 시스템 전체의 고장으로 처리하지 않고 작업 셀이 지속적으로 운영될 수 있도록 한다.

따라서 가장 효과적인 아키텍처는 폐루프 인지-행동 구조(Closed Perception-Action Loop)를 형성한다. 각각의 피킹 동작은 장면을 변화시키므로 시스템은 이전의 월드 모델(World Model)이 계속 유효하다고 가정하는 대신 빈을 다시 관측해야 한다. 새로운 센서 데이터는 물체의 가시성(Visibility), 점유 상태(Occupancy), 파지 기회(Grasp Opportunity)를 갱신하며, 실행 피드백은 선택된 전략에 대한 신뢰도를 수정한다. 이러한 관측(Observe), 계획(Plan), 행동(Act), 검증(Verify), 재관측(Re-observe)의 지속적인 순환은 무작위 자세 조작에 내재된 불확실성에 대응하는 강건성을 제공한다.

빈 피킹 사례는 산업용 조작(Industrial Manipulation)의 보다 일반적인 원리를 보여준다. 자율성(Autonomy)은 하나의 고성능 알고리즘만으로 구현되는 것이 아니라 인지, 계획, 제어, 복구 기능의 유기적인 협력을 통해 형성된다. 신뢰할 수 있는 무작위 자세 파지(Random-Pose Grasping)를 위해서는 로봇이 물체의 기하 구조, 접근 가능성(Accessibility), 충돌 제약, 접촉 역학(Contact Mechanics), 불확실성, 생산 목표를 동시에 고려해야 한다. 이러한 통합 아키텍처는 머신 텐딩(Machine Tending), 키팅(Kitting), 분류(Sorting), 디팔레타이징(Depalletizing), 조립 부품 공급(Assembly Feeding) 및 기타 유연 생산(Flexible Manufacturing) 응용을 위한 기반을 제공한다.

## 12.02. Screw Driving and Assembly Force Control Case

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

산업용 나사 체결 및 조립(Industrial Screw Driving and Assembly)은 로봇이 부품을 정렬하고, 제어된 접촉을 형성하고, 체결 부품(Fastener)을 삽입한 다음, 제품을 손상시키지 않으면서 지정된 체결 조건에 도달해야 하는 대표적인 접촉 중심 조작(Contact-Rich Manipulation) 작업이다. 자유 공간 픽앤플레이스(Free-Space Pick-and-Place) 작업과 달리 성공 여부는 기하학적 정확도뿐만 아니라 상호작용 힘(Interaction Force)에도 좌우된다. 따라서 위치 제어(Position Control), 힘 센싱(Force Sensing), 컴플라이언스(Compliance), 토크 제어(Torque Regulation), 공정 검증(Process Verification)이 하나의 통합된 조립 시스템으로 동작해야 한다.

일반적인 자동화 스테이션(Automated Station)은 산업용 로봇(Industrial Robot) 또는 협동 로봇(Collaborative Robot), 전동 스크루드라이버(Electric Screwdriver), 나사 공급 장치(Screw-Feeding Mechanism), 지그(Fixture), 카메라(Camera), 힘 또는 토크 센서(Force or Torque Sensor)로 구성된다. 부품은 먼저 대략적인 조립 공차(Assembly Tolerance) 범위 내에 배치되며, 로봇은 실행 과정에서 남아 있는 불확실성을 보상한다. 목표는 단순히 나사를 회전시키는 것이 아니라 반복 가능한 예압(Preload), 안착 상태(Seating Condition), 깊이(Depth), 품질(Quality)을 갖는 기계적으로 올바른 체결부(Joint)를 형성하는 것이다.

기하학적 불확실성(Geometric Uncertainty)은 로봇 나사 체결에서 가장 기본적인 어려움 중 하나이다. 지그 공차(Fixture Tolerance), 부품 변형(Component Deformation), 제조 편차(Manufacturing Variation), 캘리브레이션 오차(Calibration Error), 누적된 로봇 위치 오차로 인해 명목상의 나사 구멍 위치와 실제 위치가 달라질 수 있다. 작은 횡방향 또는 각도 오차만 발생해도 정상적인 결합을 방해하거나 나사산을 손상시키거나 크로스 스레딩(Cross-Threading)을 유발할 수 있다. 따라서 시스템은 프로그램된 좌표에만 의존하는 대신 국부 정렬 전략(Local Alignment Strategy)을 사용해야 한다.

비전(Vision)은 나사 구멍, 부품 자세(Component Pose), 주변 기하 구조에 대한 초기 추정값을 제공할 수 있다. 작업대 상부 또는 엔드 이펙터(End Effector) 부근에 장착된 카메라는 기준 특징(Reference Feature)을 검출하고 부품별 편차를 보상할 수 있다. 그러나 스크루드라이버가 작업물에 접근하여 물리적인 접촉이 시작되면 시각적 위치 추정만으로 전체 접촉 상태(Contact State)를 표현하기 어렵다. 이 단계부터는 힘, 토크, 모터 전류(Motor Current), 변위(Displacement), 회전 신호가 더욱 중요해진다.

접근 단계(Approach Phase)에서는 일반적으로 위치 제어 또는 카테시안 모션 제어(Cartesian Motion Control)를 이용하여 공구를 사전에 정의된 접촉 전 자세(Pre-Contact Pose)까지 빠르게 이동시킨다. 예상되는 표면에 가까워지면 충돌 에너지(Impact Energy)를 제한하고 안정적으로 접촉을 감지할 수 있도록 이동 속도를 낮춘다. 축방향 힘(Axial Force)이 작은 임계값을 초과하면 제어기는 물리적 접촉을 인식하고 자유 공간 운동에서 컴플라이언트 상호작용 모드(Compliant Interaction Mode)로 전환한다. 이러한 상태 전환은 체결 부품과 조립체를 모두 보호하는 데 중요하다.

불확실성이 허용 가능한 삽입 공차(Insertion Tolerance)를 초과하는 경우 구멍 탐색(Hole Searching)이 필요할 수 있다. 로봇은 표면에 대해 제어된 축방향 힘을 유지하면서 나선형(Spiral), 래스터(Raster), 원형(Circular) 또는 작은 진동 형태의 탐색 동작을 수행할 수 있다. 위치와 힘의 변화를 이용하여 나사 끝이나 정렬 특징(Alignment Feature)이 목표 구멍에 진입했는지를 판단한다. 지나친 횡방향 운동은 표면에 흠집을 발생시키거나 나사를 변형시키거나 잘못된 결합(False Engagement)을 유발할 수 있으므로 탐색 파라미터(Search Parameter)는 신중하게 제한해야 한다.

컴플라이언스(Compliance)는 프로그램된 궤적을 환경에 강제로 적용하는 대신 엔드 이펙터가 작은 위치 및 방향 오차를 수용할 수 있도록 한다. 기계적 컴플라이언스(Mechanical Compliance)는 원격 중심 컴플라이언스 장치(Remote Center Compliance Device), 플로팅 공구 홀더(Floating Tool Holder), 스프링 메커니즘(Spring Mechanism), 컴플라이언트 커플링(Compliant Coupling)을 통해 구현할 수 있다. 소프트웨어 컴플라이언스(Software Compliance)는 임피던스 제어(Impedance Control), 어드미턴스 제어(Admittance Control), 하이브리드 위치-힘 제어(Hybrid Position-Force Control)를 통해 구현할 수 있다. 산업 시스템에서는 강건성을 높이기 위해 기계적 방식과 소프트웨어 방식을 함께 사용하는 경우가 많다.

하이브리드 위치-힘 제어(Hybrid Position-Force Control)는 작업 방향에 따라 운동 목표를 분리한다. 나사가 결합되는 동안 로봇은 횡방향 위치와 공구 방향을 제어하면서 나사 축 방향의 접촉력을 조절할 수 있다. 이를 통해 정렬 상태를 유지하면서 스크루드라이버가 작업물을 과도하게 누르는 것을 방지한다. 명령 힘(Commanded Force)은 비트(Bit)의 결합을 유지할 만큼 충분해야 하지만 부품 변형, 나사산 손상, 불필요한 마찰을 발생시키지 않을 정도로 제한되어야 한다.

나사 체결 단계(Screw-Driving Phase)는 정렬 및 접촉 조건이 사전에 정의된 기준을 만족한 이후에만 시작된다. 스핀들(Spindle)이 회전하는 동안 제어기는 토크, 회전 각도(Angular Displacement), 축방향 변위, 힘, 모터 전류를 모니터링한다. 이러한 신호들은 체결 공정에 대한 동적인 정보를 제공한다. 생산 시스템은 나사 체결을 단순한 회전 명령으로 처리하지 않고 변화하는 토크-각도(Torque-Angle) 및 힘-변위(Force-Displacement) 거동을 해석하여 정상적인 결합 여부를 판단한다.

크로스 스레딩(Cross-Threading)은 가장 중요한 고장 모드(Failure Mode) 중 하나이다. 나사 축이 나사 구멍의 축과 정렬되지 않았거나 첫 번째 나사산이 잘못 결합되면 발생할 수 있다. 이 경우 예상보다 훨씬 이른 시점에서 비정상적인 토크가 나타날 수 있으며, 동시에 삽입 깊이가 충분하지 않은 현상이 발생할 수 있다. 강건한 제어기는 이러한 신호 패턴을 검출하여 즉시 회전을 정지하고, 나사를 역회전시키고, 축방향 힘을 해제한 다음 재정렬을 수행하고 제어된 조건에서 다시 결합을 시도한다.

나사산 시작점 검출(Thread-Start Detection)을 위해 정방향 체결 전에 짧은 역회전(Reverse Rotation)을 사용할 수도 있다. 역회전을 통해 나사산이 상대 나사산과 자연스럽게 맞춰질 수 있으며 시작점에 도달했을 때 나타나는 변화를 검출할 수 있다. 이후 제어기는 회전 방향을 전환하여 삽입을 시작한다. 구체적인 전략은 나사의 형상과 재질에 따라 달라지지만, 제어된 나사산 시작 동작은 손상된 체결 부품과 불량 조립품의 발생을 크게 줄일 수 있다.

나사가 전진함에 따라 제어기는 런다운 영역(Rundown Region)과 최종 체결 영역(Final Tightening Region)을 구분한다. 런다운 과정에서는 나사가 주로 나사산 마찰(Thread Friction)을 극복하면서 이동하기 때문에 토크가 비교적 낮게 유지된다. 나사 머리 또는 와셔(Washer)가 상대 표면에 접촉하면 토크가 크게 증가하기 시작한다. 이러한 안착 이벤트(Seating Event)는 중요한 공정 기준점(Process Landmark)이 되며, 시스템이 고속 체결에서 보다 정밀한 최종 체결 제어로 전환할 수 있도록 한다.

최종 체결(Final Tightening)은 토크, 각도, 토크-각도(Torque-Plus-Angle), 깊이 또는 이러한 변수들의 조합을 이용하여 제어할 수 있다. 토크 제어(Torque Control)는 구현이 간단하지만 마찰 변화에 크게 영향을 받을 수 있다. 각도 기반 전략(Angle-Based Strategy)은 안착 이후 체결부의 변형에 관한 추가 정보를 제공하며, 깊이 센싱(Depth Sensing)은 불완전한 삽입을 검출할 수 있다. 따라서 고품질 조립 시스템은 목표 토크에 도달했다는 이유만으로 체결부를 정상으로 판정하지 않고 여러 신호를 함께 평가한다.

힘 제어(Force Control)는 컴플라이언트하거나 취약하거나 얇거나 여러 층으로 구성된 부품을 체결할 때 특히 중요하다. 과도한 축방향 하중은 패널을 휘게 만들거나 플라스틱 하우징(Plastic Housing)을 손상시키거나 실(Seal)을 변형시키거나 체결이 완료되기 전에 부품 위치를 변화시킬 수 있다. 로봇은 비트 캠아웃(Bit Cam-Out)을 방지하기에 충분한 압력을 유지하면서 조립체로 전달되는 힘을 제한해야 한다. 적응형 힘 제어(Adaptive Force Regulation)는 작업 중 측정되는 접촉 거동에 따라 힘 명령을 변경할 수 있다.

스크루드라이버 자체도 폐루프 공정(Closed-Loop Process)의 일부를 구성한다. 전동 체결 공구(Electric Fastening Tool)는 스핀들 토크, 회전 각도, 속도, 완료 상태를 로봇 또는 셀 제어기(Cell Controller)에 직접 제공할 수 있다. 로봇은 카테시안 위치, 방향, 접촉력, 운동 상태를 제공한다. 이러한 신호들을 결합하면 각 하위 시스템을 독립적으로 사용하는 것보다 훨씬 풍부한 공정 시그니처(Process Signature)를 구성할 수 있으며 불완전한 체결이나 비정상적인 기계 상태를 보다 안정적으로 검출할 수 있다.

공정 검증(Process Verification)은 각 나사 체결 작업 직후 수행되어야 한다. 합격 기준(Acceptance Criteria)에는 지정된 범위 내의 최종 토크, 요구되는 회전 각도, 올바른 삽입 깊이, 안정적인 안착 힘(Seating Force), 비정상적인 토크 피크(Torque Peak)의 부재 등이 포함될 수 있다. 비전은 추가적으로 나사의 존재 여부와 나사 머리 위치를 확인할 수 있다. 이렇게 얻어진 측정 결과는 제품 식별 정보(Product Identifier)와 연결하여 제조 품질 관리(Manufacturing Quality Management)를 위한 추적성(Traceability)을 제공할 수 있다.

조립 실패를 단순히 더 엄격한 위치 공차만으로 완전히 제거할 수는 없기 때문에 복구 로직(Recovery Logic)이 필요하다. 접촉이 감지되지 않으면 로봇은 목표 자세를 다시 획득할 수 있다. 구멍 탐색에 실패하면 안전 범위 내에서 탐색 영역을 확대할 수 있다. 비정상적인 토크가 크로스 스레딩을 나타내면 나사를 역회전시킨 후 다시 시도할 수 있다. 반복적인 시도에도 실패한다면 전체 생산 라인을 정지시키지 않고 해당 부품을 불량으로 처리하거나 작업자에게 전달할 수 있다.

지그 설계(Fixture Design)는 로봇 조립 성능에 큰 영향을 미친다. 지그는 불필요한 과구속(Overconstraint)을 발생시키지 않으면서 체결 반력(Fastening Reaction Force)에 저항할 수 있을 정도로 작업물을 충분히 구속해야 한다. 기준면(Datum Surface)과 위치 결정 특징(Locating Feature)은 반복 가능한 기준 기하 구조를 제공해야 하며, 필요한 접근 범위에서 공구가 조립 위치에 접근할 수 있어야 한다. 잘 설계된 지그는 인지와 힘 제어가 동적으로 보상해야 하는 불확실성의 크기를 감소시킨다.

로봇 강성(Robot Stiffness)과 체결 반력 토크(Fastening Reaction Torque)도 고려해야 한다. 긴 공구 연장부 또는 불리한 로봇 암 자세는 하중이 발생했을 때 변형을 증가시켜 체결 과정에서 스크루드라이버 정렬을 변화시킬 수 있다. 따라서 모션 계획(Motion Planning)은 충분한 강성, 관절 여유(Joint Margin), 충돌 여유 공간(Collision Clearance)을 확보할 수 있는 로봇 자세를 선택해야 한다. 높은 토크가 필요한 응용에서는 전체 반력을 로봇 암을 통해 전달하는 것보다 외부 반력 구조(External Reaction Structure) 또는 전용 체결 장비를 사용하는 것이 적합할 수 있다.

사이클 타임 최적화(Cycle-Time Optimization)를 위해서는 작업 단계별로 서로 다른 제어 동작을 적용해야 한다. 자유 공간 접근은 빠르게 수행할 수 있고, 국부 탐색은 제어된 상태를 유지하면서 효율적으로 이루어져야 하며, 런다운은 비교적 높은 스핀들 속도를 사용할 수 있고, 최종 체결에서는 느리고 정밀한 제어가 필요하다. 전체 공정에 동일하게 보수적인 모션 파라미터를 적용하면 생산 시간이 낭비된다. 상태 기반 제어(State-Based Control)를 이용하면 현재 조립 단계에 따라 속도, 힘 제한, 컴플라이언스, 모니터링 임계값을 변경할 수 있다.

따라서 대표적인 생산 시퀀스(Production Sequence)는 목표 위치 추정(Target Localization), 접근, 접촉 검출(Contact Detection), 국부 정렬(Local Alignment), 나사산 결합(Thread Engagement), 런다운, 안착 검출(Seating Detection), 최종 체결, 검증, 공구 후퇴(Tool Retraction)의 순서로 진행된다. 각 단계의 전환은 단순한 경과 시간이 아니라 측정 가능한 조건에 의해 결정된다. 이러한 이벤트 기반 아키텍처(Event-Driven Architecture)는 로봇이 실제 조립체의 물리적 상태에 대응할 수 있도록 하므로 제조 편차에 대한 허용성을 높인다.

성능 평가는 단순히 성공적으로 체결된 나사의 비율만을 대상으로 해서는 안 된다. 유용한 성능 지표에는 첫 시도 결합 성공률(First-Attempt Engagement Rate), 크로스 스레딩 발생 빈도(Cross-Thread Frequency), 평균 정렬 시간(Mean Alignment Time), 최종 토크 편차(Final Torque Variation), 체결 각도 분포(Tightening-Angle Distribution), 사이클 타임, 재시도율(Retry Rate), 공구 마모(Tool Wear), 부품 손상률(Component Damage Rate), 장기 공정 능력(Long-Term Process Capability)이 포함된다. 이러한 변수의 추세를 모니터링하면 비트 마모, 지그 이동, 캘리브레이션 드리프트(Calibration Drift), 입고 부품 품질 변화와 같은 점진적인 성능 저하를 발견할 수 있다.

생산 데이터(Production Data)는 예측 품질(Predictive Quality)과 적응형 제어(Adaptive Control)에도 활용할 수 있다. 토크-각도 곡선, 축방향 힘 이력(Axial-Force History), 공구 변위, 재시도 이벤트를 저장하여 통계 분석(Statistical Analysis) 또는 머신러닝 모델(Machine-Learning Model)에 사용할 수 있다. 비정상적인 신호 패턴은 나사산 결함, 와셔 누락, 잘못된 나사, 손상된 부품, 공구 열화를 나타낼 수 있다. 따라서 체결 작업은 단순한 조립 동작을 넘어 제조 공정의 상태와 품질에 관한 정보를 생성하는 데이터 소스(Data Source)의 역할도 수행한다.

나사 체결 및 힘 제어 조립 사례(Screw-Driving and Force-Controlled Assembly Case)는 기하학적 지능(Geometric Intelligence)과 물리적 상호작용 지능(Physical Interaction Intelligence)을 통합하는 것이 얼마나 중요한지를 보여준다. 비전은 작업이 대략 어디에서 수행되어야 하는지를 결정하고, 힘, 토크, 변위, 컴플라이언스는 실제 접촉 인터페이스(Contact Interface)에서 무엇이 일어나고 있는지를 알려준다. 인지, 컴플라이언트 모션(Compliant Motion), 체결 제어, 검증, 복구를 하나의 폐루프(Closed Loop)로 결합함으로써 로봇 시스템은 실제 산업 환경의 불확실성 속에서도 정밀하고 반복 가능한 조립 작업을 수행할 수 있다.

## 12.03. PCB Component Pick and Place Precision Case

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

PCB 부품 픽앤플레이스(PCB Component Pick-and-Place)는 전자 부품을 획득하고, 검사하고, 정렬하고, 이송한 후 인쇄회로기판(Printed Circuit Board, PCB)의 지정된 위치에 배치해야 하는 대표적인 고정밀 산업용 조작(High-Precision Industrial Manipulation) 작업이다. 이 공정은 빠르고 반복적인 운동과 마이크로미터 수준의 위치 정밀도 요구사항을 동시에 결합한다. 따라서 신뢰성 있는 동작을 위해서는 기계 구조(Mechanics), 머신 비전(Machine Vision), 캘리브레이션(Calibration), 모션 제어(Motion Control), 진공 핸들링(Vacuum Handling), 지속적인 공정 검증(Process Verification)이 긴밀하게 통합되어야 한다.

일반적인 시스템은 테이프앤릴 피더(Tape-and-Reel Feeder), 트레이(Tray), 튜브(Tube) 또는 전용 부품 매거진(Component Magazine)을 통해 부품을 공급받는다. 배치 헤드(Placement Head)는 공급 위치와 PCB 사이를 이동하며 하나 이상의 노즐(Nozzle)이 진공을 이용하여 부품을 획득한다. 일반적인 로봇 핸들링과 달리 조작 대상은 매우 작고 가벼우며 취약하고 기하학적으로 다양할 수 있다. 저항기(Resistor), 커패시터(Capacitor), 집적회로(Integrated Circuit), 커넥터(Connector), 미세 피치 패키지(Fine-Pitch Package)는 각각 서로 다른 핸들링 파라미터와 배치 전략을 필요로 한다.

PCB 자체는 구조화되어 있지만 공차에 매우 민감한 작업 공간(Workspace)을 제공한다. 명목상의 부품 좌표는 전자 설계 및 제조 데이터(Electronic Design and Manufacturing Data)에서 얻지만 실제 기판은 이상적인 모델에 비해 평행 이동하거나 회전하거나 휘어지거나 치수가 달라질 수 있다. 지그 편차(Fixture Variation)와 컨베이어 위치 오차도 추가적인 불확실성을 발생시킨다. 따라서 생산 장비는 단순히 명목 좌표를 실행해서는 안 되며 정밀 배치를 시작하기 전에 실제 기판 좌표계(Board Coordinate Frame)를 설정해야 한다.

기준 마크 인식(Fiducial Recognition)은 이러한 좌표계를 결정하는 데 일반적으로 사용된다. 카메라는 PCB에 인쇄된 전역 또는 국부 기준 마크(Fiducial Mark)를 인식하고 이를 통해 평행 이동, 회전, 스케일(Scale), 경우에 따라 국부적인 기하학적 왜곡(Local Geometric Distortion)을 추정한다. 전역 기준 마크(Global Fiducial)는 전체 기판을 정렬하고, 미세 피치 소자 근처의 국부 기준 마크(Local Fiducial)는 중요 영역에서 더욱 높은 정확도를 제공할 수 있다. 배치 공차가 매우 작아질수록 정확한 특징 추출(Feature Extraction)과 서브픽셀 위치 추정(Subpixel Localization)이 필수적이다.

부품 획득(Component Acquisition)은 배치 노즐을 피더 또는 트레이 위치로 이동시키면서 시작된다. 노즐은 부품의 크기, 질량, 표면 형상, 사용 가능한 흡착 영역에 적합해야 한다. 접촉 또는 근접 접촉 이후 진공 압력(Vacuum Pressure)이 인가되며 시스템은 부품이 성공적으로 획득되었는지 검증한다. 진공이 충분하지 않다면 부품 누락, 잘못된 픽업 위치, 손상된 노즐, 누설(Leakage), 또는 선택된 노즐에 적합하지 않은 부품 표면을 의미할 수 있다.

작은 부품은 위치가 이동하거나 회전하거나 정전기적으로 부착되거나 캐리어 재료(Carrier Material)에 남아 있을 수 있으므로 픽업 동역학(Pickup Dynamics)을 제어해야 한다. 과도한 수직 가속도는 진공 흡착을 해제시킬 수 있으며 불필요한 접촉력은 민감한 패키지를 손상시킬 수 있다. 따라서 장비는 부품별 레시피(Component-Specific Recipe)에 따라 노즐 하강, 접촉 검출(Contact Detection), 진공 활성화, 상승 운동, 가속도 프로파일(Acceleration Profile)을 조정한다. 부품 크기가 작아질수록 이러한 파라미터의 중요성은 더욱 증가한다.

픽업 이후에는 부품 검사(Component Inspection)를 통해 노즐에 대한 실제 부품의 위치와 방향을 결정한다. 하부 비전 카메라(Bottom-View Camera)는 부품이 이송되는 동안 영상을 획득하여 에지(Edge), 리드(Lead), 볼(Ball), 패드(Pad), 패키지 외곽선(Package Contour)을 검출할 수 있다. 측정된 X, Y 위치 및 회전 오프셋(Rotation Offset)은 이후 배치 과정에서 보정된다. 이러한 폐루프 보정(Closed-Loop Correction)은 픽업 편차가 PCB의 배치 오차로 직접 전달되는 것을 방지한다.

머신 비전(Machine Vision)은 매우 다양한 패키지 형상과 광학적 특성을 처리해야 한다. 부품에는 반사성이 높은 리드, 어두운 몰드 본체(Molded Body), 금속 표면, 낮은 대비의 에지, 반복적인 패턴이 존재할 수 있다. 따라서 조명 구조(Illumination Geometry)는 비전 알고리즘만큼 중요하다. 동축 조명(Coaxial Illumination), 링 조명(Ring Illumination), 측면 조명(Side Illumination), 백라이트(Backlight), 다방향 조명(Multi-Directional Illumination)을 이용하여 필요한 특징을 강조할 수 있으며, 노출과 임계값 파라미터는 개별 부품군에 맞게 조정할 수 있다.

캘리브레이션(Calibration)은 카메라 픽셀, 장비 좌표, 노즐 위치, 피더 위치, PCB 좌표를 하나의 일관된 기하학적 모델(Geometric Model)로 연결한다. 이러한 변환 과정에서 발생하는 오차는 배치 정확도에 직접 누적된다. 따라서 카메라 내부 캘리브레이션(Camera Intrinsic Calibration), 카메라-장비 캘리브레이션(Camera-to-Machine Calibration), 노즐 중심 캘리브레이션(Nozzle-Center Calibration), 헤드 오프셋 캘리브레이션(Head-Offset Calibration), 높이 캘리브레이션(Height Calibration)을 세심하게 관리해야 한다. 장시간 생산 과정에서 발생하는 열팽창(Thermal Expansion)과 기계적 드리프트(Mechanical Drift)에 대응하기 위해 주기적인 재캘리브레이션 또는 자동 보정이 필요할 수도 있다.

배치 정확도(Placement Accuracy)는 명령 위치뿐만 아니라 반복 정밀도(Repeatability), 구조 강성(Structural Stiffness), 진동(Vibration), 서보 성능(Servo Performance), 안정화 거동(Settling Behavior)의 영향을 받는다. 높은 가속도는 처리량을 향상시키지만 기계 진동을 유발하여 최종 위치 정확도를 저하시킬 수 있다. 따라서 모션 프로파일(Motion Profile)은 저크(Jerk), 가속도, 안정화 시간을 제어하여 속도와 정밀도 사이의 균형을 유지해야 한다. 정밀 장비에서는 일반적으로 고속 이송 운동과 목표 배치 위치 근처에서의 느린 최종 접근 단계를 분리한다.

수직 배치 제어(Vertical Placement Control) 역시 중요하다. 노즐은 과도한 힘을 발생시키지 않으면서 부품이 솔더 페이스트(Solder Paste) 또는 목표 기판 표면에 접촉할 때까지 하강해야 한다. 기판 두께 편차, 휨(Warpage), 지그 높이, 패키지 두께에 따라 실제 접촉 위치가 달라질 수 있다. 높이 센싱(Height Sensing), 레이저 측정(Laser Measurement), 힘 검출(Force Detection), 보정된 Z축 모델(Calibrated Z-Axis Model)을 이용하면 이러한 변화를 보상하고 부품 손상이나 불충분한 안착을 방지할 수 있다.

솔더 페이스트(Solder Paste)는 추가적인 물리적 상호작용 효과를 발생시킨다. 배치 과정에서 부품은 방출 이후 안정적으로 유지될 수 있도록 페이스트와 충분히 접촉해야 하지만 과도한 압축은 페이스트를 이동시키거나 인접한 패드를 브리지(Bridge)시키거나 미세 구조를 손상시킬 수 있다. 따라서 제어된 배치 힘(Placement Force)과 Z축 높이가 필요하다. 매우 작거나 취약한 소자의 경우 수직 운동의 작은 편차도 이후의 솔더 리플로우(Solder Reflow) 품질과 전기적 신뢰성에 영향을 미칠 수 있다.

부품 방출(Component Release) 역시 정밀하게 제어해야 한다. 부품이 목표 자세에 도달하면 진공을 제거하며, 노즐에서 확실하게 분리시키기 위해 짧은 시간 동안 양압(Positive Air Pressure)을 가할 수도 있다. 방출이 너무 빠르면 부품이 기판과 접촉하기 전에 떨어지거나 위치가 이동할 수 있다. 반대로 방출이 너무 늦으면 노즐과 부품 사이의 부착력으로 인해 배치 헤드가 후퇴할 때 의도한 위치가 변할 수 있다.

미세 피치 집적회로(Fine-Pitch Integrated Circuit)는 많은 리드 또는 솔더 연결부가 PCB 패드와 동시에 일치해야 하므로 특히 높은 정렬 정확도를 요구한다. 작은 평행 이동 또는 각도 오차만으로도 리플로우 이후 개방 회로(Open Circuit) 또는 브리징(Bridging)이 발생할 수 있다. 따라서 비전 알고리즘은 단순히 패키지 외곽선만 이용하지 않고 개별 리드, 패키지 에지 또는 볼 그리드 패턴(Ball-Grid Pattern)을 검사할 수 있다. 국부 PCB 기준 마크를 사용하면 중요 소자 주변의 정렬 오차를 더욱 줄일 수 있다.

매우 작은 수동 부품(Passive Component)은 또 다른 종류의 어려움을 발생시킨다. 질량이 매우 작기 때문에 정전기력(Electrostatic Force), 공기 흐름(Airflow), 노즐 오염(Nozzle Contamination), 솔더 페이스트 표면력이 부품 방향에 영향을 줄 수 있다. 잘못된 배치는 리플로우 이후 툼스토닝(Tombstoning), 비틀림(Skew), 부품 누락(Missing Component) 등의 결함을 유발할 수 있다. 따라서 명목상의 부품 형상이 단순하더라도 안정적인 픽업, 제어된 가속도, 정확한 Z축 높이, 신뢰성 있는 방출이 필요하다.

PCB 조립은 구조화된 환경에서 수행되지만 충돌 회피(Collision Avoidance) 역시 중요하다. 이미 배치된 높은 부품, 커넥터, 실드(Shield), 지그 요소, 주변 장비 구조가 노즐의 접근을 제한할 수 있다. 따라서 이후 작업을 방해할 수 있는 높은 구조물을 배치하기 전에 낮은 높이의 부품을 먼저 설치하도록 배치 순서(Placement Sequence)를 최적화할 수 있다. 시스템은 부품의 풋프린트(Component Footprint)뿐만 아니라 노즐 형상과 헤드 여유 공간(Head Clearance)도 고려해야 한다.

생산 처리량(Production Throughput)은 일반적으로 시간당 부품 수(Components per Hour)로 표현하지만 배치 헤드의 명목 속도만으로 실제 생산성을 결정할 수는 없다. 피더 접근 시간, 비전 검사 지연(Vision Inspection Latency), 노즐 교환, 기판 이송, 배치 안정화, 재시도, 장비 중단이 모두 실제 사이클 타임(Cycle Time)에 영향을 준다. 다중 노즐 헤드(Multi-Nozzle Head)는 여러 부품을 동시에 운반하여 처리량을 높일 수 있으며 최적화된 경로는 피더와 배치 위치 사이의 불필요한 이동을 감소시킨다.

배치 순서 최적화(Placement Sequence Optimization)는 제약 조건이 존재하는 경로 계획 문제(Constrained Routing Problem)와 유사하다. 장비는 피더 위치, 노즐 호환성(Nozzle Compatibility), 부품 우선순위, 충돌 제약, 공정 요구사항을 만족하면서 효율적인 픽업 및 배치 순서를 결정해야 한다. 이론적으로 가장 짧은 궤적이라도 잦은 노즐 교환이나 추가적인 비전 검사를 요구한다면 가장 높은 생산성을 제공하지 못할 수 있다. 따라서 실용적인 최적화에서는 단순한 기하학적 이동 거리보다 전체 장비 사이클을 평가해야 한다.

공정 검증(Process Verification)은 픽업 단계에서 시작되어 배치가 완료될 때까지 지속된다. 진공 센싱(Vacuum Sensing)은 부품 획득 여부를 확인하고, 비전은 부품의 종류와 방향을 검증하며, 모션 피드백(Motion Feedback)은 헤드가 명령된 위치에 도달했는지를 확인한다. 배치 후 검사(Post-Placement Inspection)를 통해 부품의 존재 여부, 중심 정렬, 올바른 회전 방향, 적절한 안착 상태를 판단할 수 있다. 이러한 검사를 통해 결함이 솔더 리플로우 또는 후속 조립 공정으로 전달되기 전에 검출할 수 있다.

자동 광학 검사(Automated Optical Inspection, AOI)는 배치 또는 솔더링 이후 추가적인 피드백 계층을 제공할 수 있다. 검출된 위치 편차를 통계적으로 분석하면 무작위적인 부품 편차와 체계적인 장비 오차(Systematic Machine Error)를 구분할 수 있다. 여러 배치 위치에서 일관된 오프셋이 나타난다면 캘리브레이션 드리프트(Calibration Drift), 피더 위치 변화, 노즐 편심(Nozzle Eccentricity), 기판 정합 오류(Board-Registration Error)를 의미할 수 있다. 따라서 공정 데이터는 즉각적인 품질 관리뿐만 아니라 장기적인 장비 유지보수에도 활용할 수 있다.

생산 강건성(Production Robustness)을 확보하기 위해서는 복구 동작(Recovery Behavior)이 필요하다. 진공 픽업에 실패하면 장비는 동일한 부품을 다시 시도하거나 피더를 다음 위치로 전진시킬 수 있다. 비전 검사에서 획득한 부품이 불합격으로 판정되면 해당 부품을 폐기하고 새로운 부품으로 교체할 수 있다. 기준 마크 검출에 실패하면 작업자 개입을 요청하기 전에 조명 또는 탐색 파라미터를 조정할 수 있다. 명확하게 정의된 복구 상태(Recovery State)는 개별 부품 오류가 전체 조립 라인의 불필요한 정지로 이어지는 것을 방지한다.

전자 제조에서는 추적성(Traceability)의 중요성이 지속적으로 증가하고 있다. 각각의 기판은 배치 레시피(Placement Recipe), 부품 로트(Component Lot), 피더 위치, 검사 결과, 장비 파라미터, 타임스탬프(Timestamp), 검출된 이상 상태와 연결할 수 있다. 이러한 기록을 이용하면 이후 품질 문제가 발생했을 때 제조 당시의 생산 조건을 재구성할 수 있다. 또한 통계적 공정 관리(Statistical Process Control), 예지 정비(Predictive Maintenance), 배치 파라미터 최적화를 위한 구조화된 데이터셋(Structured Dataset)으로 활용할 수 있다.

따라서 성능은 여러 지표를 이용하여 평가해야 한다. 중요한 지표에는 픽업 성공률(Pickup Success Rate), 배치 정확도(Placement Accuracy), 회전 오차(Rotational Error), 부품 손실률(Component Loss Rate), 비전 불합격률(Vision Rejection Rate), 배치 힘 편차(Placement-Force Variation), 시간당 부품 수, 재시도 빈도(Retry Frequency), 노즐 교환 시간(Nozzle-Change Time), 검사 또는 리플로우 이후 확인되는 결함률(Defect Rate)이 포함된다. 공정 능력 지표(Process Capability Measure)를 이용하면 평균 오차만 평가하는 것이 아니라 위치 분포가 제조 공차 내에서 안정적으로 유지되는지를 판단할 수 있다.

PCB 픽앤플레이스 사례(PCB Pick-and-Place Case)는 높은 처리량과 극도의 정밀도를 동시에 달성해야 할 때 산업용 조작이 어떻게 변화하는지를 보여준다. 성공적인 시스템은 센싱(Sensing), 기하학적 캘리브레이션(Geometric Calibration), 진공 핸들링, 고성능 모션 제어, 국부 비전 보정(Local Visual Correction), 제어된 접촉(Controlled Contact), 검증, 복구 기능의 유기적인 협력을 통해 구현된다. 이러한 폐루프 아키텍처(Closed-Loop Architecture)를 통해 전자 조립 시스템은 다양한 부품을 빠르게 배치하면서도 신뢰성 높은 대량 생산에 필요한 정확도와 반복 정밀도를 지속적으로 유지할 수 있다.

## 12.04. Welding Torch Manipulation Path Following Case

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

로봇 용접 토치 조작(Robotic Welding Torch Manipulation)은 엔드 이펙터(End Effector)가 안정적인 용접 공정을 유지하면서 제어된 위치(Position), 방향(Orientation), 속도(Velocity), 이격 거리(Stand-Off Distance)를 기반으로 용접 이음부(Joint)를 따라 이동해야 하는 대표적인 연속 경로 산업 작업(Continuous-Path Industrial Task)이다. 개별적인 픽앤플레이스(Pick-and-Place) 작업과 달리 용접 품질은 시간에 따른 전체 궤적(Trajectory)에 의해 결정된다. 작은 경로 오차도 용입(Penetration), 비드 형상(Bead Geometry), 입열량(Heat Input), 접합 강도(Joint Strength)를 변화시킬 수 있으므로 센싱(Sensing), 계획(Planning), 모션 제어(Motion Control), 공정 제어(Process Regulation)가 지속적으로 함께 동작해야 한다.

일반적인 로봇 용접 셀(Robotic Welding Cell)은 6축 산업용 로봇(Six-Axis Industrial Robot), 용접 토치(Welding Torch), 전원 장치(Power Source), 와이어 피더(Wire Feeder), 작업물 지그(Workpiece Fixture), 보호 가스 시스템(Shielding-Gas System), 안전 장비(Safety Equipment)로 구성된다. 추가 센싱 장치로 레이저 심 추적기(Laser Seam Tracker), 카메라(Camera), 아크 센서(Arc Sensor), 힘 센서(Force Sensor), 열 모니터링(Thermal Monitoring) 장치를 사용할 수 있다. 로봇은 공간적 조작(Spatial Manipulation)을 담당하고 용접 시스템은 전기 및 재료 관련 파라미터를 제어하며, 성공적인 작업을 위해 두 하위 시스템은 전체 용접 과정에서 동기화되어야 한다.

명목 용접 경로(Nominal Welding Path)는 일반적으로 CAD 형상(CAD Geometry), 오프라인 프로그래밍(Offline Programming), 티칭(Teaching), 또는 수동으로 기록된 로봇 자세를 기반으로 생성된다. 이 기준 궤적(Reference Trajectory)은 예상되는 용접선 위치와 이음부를 따라 요구되는 토치 방향을 정의한다. 그러나 실제 작업물은 명목 모델과 완벽하게 일치하지 않는다. 제조 공차(Manufacturing Tolerance), 지그 편차(Fixture Variation), 열 변형(Thermal Deformation), 조립 간극(Assembly Gap), 부품 위치 오차로 인해 실제 용접선이 프로그램된 궤적에서 벗어날 수 있다.

따라서 용접을 시작하기 전에 작업물 위치 추정(Workpiece Localization)을 수행한다. 비전 시스템(Vision System), 터치 센싱(Touch Sensing), 레이저 스캐너(Laser Scanner), 알려진 기준 특징(Reference Feature)을 이용하여 명목 작업물 좌표계와 실제 설치된 부품 사이의 변환 관계를 결정할 수 있다. 이후 로봇은 이 변환 정보를 이용하여 용접 궤적을 갱신한다. 전역 정합(Global Registration)을 통해 전체적인 위치 오차를 보상할 수 있지만 국부적인 용접선 편차는 작업 중 실시간 추적(Real-Time Tracking)을 추가로 요구할 수 있다.

토치 자세(Torch Pose)는 단순히 공구 중심점(Tool Center Point)의 위치만으로 정의되지 않는다. 진행 각도(Travel Angle), 작업 각도(Work Angle), 전극 방향(Electrode Orientation), 콘택트 팁-작업물 거리(Contact-Tip-to-Work Distance), 용접선에 대한 횡방향 오프셋(Lateral Offset)은 모두 용접 거동에 영향을 준다. 따라서 로봇은 단순히 기하학적인 선을 따라 움직이는 것이 아니라 전체 6자유도 궤적(Six-Degree-of-Freedom Trajectory)을 제어해야 한다. 방향 전환 역시 갑작스러운 아크 방향 변화나 토치 접근성 저하를 방지하기 위해 부드럽게 이루어져야 한다.

경로 생성(Path Generation)에서는 용접 이음부 자체의 형상을 고려해야 한다. 맞대기 이음(Butt Joint), 겹치기 이음(Lap Joint), 필릿 용접(Fillet Weld), 곡선 용접선(Curved Seam), 모서리(Corner), 3차원 교차부(Three-Dimensional Intersection)는 서로 다른 토치 접근 조건을 요구한다. 표면 법선(Surface Normal)과 용접선 접선(Seam Tangent)을 이용하여 용접 경로를 따라 국부 좌표계(Local Coordinate Frame)를 구성할 수 있다. 이를 기준으로 용접선이 3차원 공간에서 휘어지는 경우에도 요구되는 진행 각도와 작업 각도를 일관되게 정의할 수 있다.

궤적 보간(Trajectory Interpolation)은 불연속적인 경로 점(Path Point)을 부드러운 로봇 운동으로 변환한다. 용접선 형상과 제어기의 기능에 따라 선형 보간(Linear Interpolation), 원호 보간(Circular Interpolation), 스플라인 보간(Spline Interpolation), 고차 보간(Higher-Order Interpolation)을 사용할 수 있다. 경유점(Waypoint)이 지나치게 적으면 기하학적 경로 오차가 증가하고, 불필요하게 많으면 프로그래밍 복잡성이 증가하거나 제어기에서 불연속성이 발생할 수 있다. 따라서 용접선 정확도를 유지하면서 부드러운 속도, 가속도, 방향 프로파일을 확보하는 것이 중요하다.

용접 속도(Welding Speed)는 단위 길이당 입열량에 직접적인 영향을 주는 핵심 공정 변수이다. 로봇이 지나치게 느리게 이동하면 과도한 열로 인해 용입, 변형, 비드 폭이 증가할 수 있다. 반대로 지나치게 빠르게 이동하면 융합(Fusion)이 불완전해지고 비드가 좁아지거나 불연속적으로 형성될 수 있다. 따라서 복잡한 경로를 따라 이동하면서 로봇 관절 속도가 크게 변하더라도 궤적 제어기는 명령된 진행 속도(Travel Speed)를 안정적으로 유지해야 한다.

로봇 기구학(Robot Kinematics) 특성으로 인해 일정한 카테시안 용접 속도(Constant Cartesian Welding Speed)를 유지하기 어려울 수 있다. 부드러운 공구 경로라도 특이점(Singularity) 또는 불리한 로봇 암 자세 근처에서는 개별 관절 속도가 급격하게 변할 수 있다. 오프라인 시뮬레이션(Offline Simulation)을 이용하면 생산에 적용하기 전에 이러한 영역을 확인할 수 있다. 이후 경로 위치, 로봇 베이스 위치, 작업물 방향, 외부 축 구성(External-Axis Configuration)을 수정하여 충분한 관절 여유(Joint Margin)를 확보하고 급격한 속도 증가를 방지할 수 있다.

외부 포지셔너(External Positioner)는 접근성과 용접 품질을 향상시키기 위해 자주 사용된다. 회전 테이블(Rotary Table), 틸트 포지셔너(Tilt Positioner), 다축 작업물 매니퓰레이터(Multi-Axis Workpiece Manipulator)를 이용하여 로봇이 토치를 제어하는 동안 부품의 자세를 변경할 수 있다. 로봇과 외부 축 사이의 협조 운동(Coordinated Motion)을 이용하면 용접부를 유리한 자세로 유지하면서 극단적인 로봇 자세를 줄일 수 있다. 이는 동기화된 궤적 생성이 필요한 다축 경로 추종 문제(Multi-Axis Path-Following Problem)를 형성한다.

실시간 용접선 추적(Real-Time Seam Tracking)은 명목 형상만으로 예측할 수 없는 편차를 보상한다. 토치 근처에 장착된 레이저 프로파일 센서(Laser Profile Sensor)는 용접 지점 앞쪽의 이음부 형상을 측정하고 횡방향 및 수직 방향의 용접선 오프셋을 추정할 수 있다. 제어기는 이러한 보정값을 기준 경로에 점진적으로 적용한다. 원시 센서 측정에는 노이즈, 반사, 스패터(Spatter) 간섭, 일시적인 특징 손실이 포함될 수 있으므로 필터링(Filtering)이 필요하다.

비전 기반 용접선 추적(Vision-Based Seam Tracking)은 광학 필터링(Optical Filtering)이 적용된 카메라를 이용하여 이음부, 용융지(Weld Pool), 구조광 레이저 패턴(Structured Laser Pattern)을 관측할 수 있다. 용접 아크는 매우 밝고 동적 범위(Dynamic Range)가 크기 때문에 일반적인 영상 획득이 어렵다. 따라서 조명, 노출, 필터링, 센서 위치를 세심하게 설계해야 한다. 영상 처리(Image Processing) 또는 학습 기반 인지 모델(Learned Perception Model)을 이용하여 용접선 중심, 이음부 경계, 간극 폭(Gap Width), 토치 오프셋을 추정하고 피드백 제어에 활용할 수 있다.

아크 센싱(Arc Sensing)은 용접 공정 자체의 전기적 특성을 이용하여 이음부를 추적하는 또 다른 방법이다. 용접 전류 또는 전압의 변화에는 특히 위빙 운동(Weaving Motion) 중에 이음부에 대한 토치의 상대적 위치 정보가 포함될 수 있다. 아크 센싱은 별도의 광학적 가시선(Line of Sight)을 필요로 하지 않기 때문에 연기, 스패터 또는 제한된 기하 구조로 인해 외부 비전을 사용하기 어려운 환경에서 유용하게 활용할 수 있다.

위빙 궤적(Weaving Trajectory)은 토치가 단순히 용접선 중심만 따라가는 것이 아니라 용접선을 가로질러 진동해야 하는 경우 사용된다. 위빙 진폭(Weave Amplitude), 주파수(Frequency), 정지 시간(Dwell Time), 파형(Waveform)은 측벽 융합(Sidewall Fusion)과 비드 형상에 영향을 준다. 제어기는 전진 용접 속도를 유지하면서 이러한 국부 진동(Local Oscillation)을 전역 용접선 궤적에 중첩한다. 용접선 추적과 위빙은 모두 명령 토치 위치를 변경하므로 두 기능을 신중하게 조정해야 한다.

이격 거리 제어(Stand-Off Distance Control)는 안정적인 공정 조건을 유지하는 데 필수적이다. 작업물 높이 변화, 변형 또는 경로 추정 오차는 토치와 표면 사이의 거리를 변화시킬 수 있다. 용접 공정의 종류에 따라 아크 전압(Arc Voltage), 레이저 측정 또는 기하학적 센싱을 이용하여 이 거리에 관한 피드백을 얻을 수 있다. 제어기는 요구되는 용접선 상대 방향을 유지하면서 토치 위치를 조정하여 이격 거리 변화를 보상할 수 있다.

용접 파라미터(Welding Parameter)는 로봇 운동과 동기화되어야 한다. 전류(Current), 전압(Voltage), 와이어 공급 속도(Wire-Feed Speed), 진행 속도, 보호 가스 유량(Shielding-Gas Flow), 파형 설정(Waveform Setting)은 이음부 형상이나 경로 구간에 따라 달라질 수 있다. 모서리, 간극 변화, 시작 영역 또는 종료 영역은 긴 직선 구간과 다른 파라미터를 요구할 수 있다. 따라서 현대적인 용접 셀은 로봇 궤적과 용접 전원을 서로 독립적인 명령으로 처리하는 대신 통합된 공정 레시피(Process Recipe)를 실행한다.

용접 시작과 종료(Weld Start and Stop)는 이러한 전환 구간에서 결함이 자주 발생하기 때문에 특수한 모션 제어를 필요로 한다. 아크 점화(Arc Ignition)는 토치가 허용 가능한 시작 자세에 도달하고 공정 조건이 준비된 이후에만 이루어져야 한다. 종료 시에는 크레이터 충전(Crater Filling), 제어된 감속, 와이어 제어 또는 짧은 정지 시간이 필요할 수 있다. 진입 및 이탈 궤적(Lead-In and Lead-Out Trajectory)을 사용하면 이러한 전환 효과를 최종 이음부의 구조적으로 중요한 영역에서 벗어나게 할 수 있다.

열 변형(Thermal Deformation)은 동적으로 변화하는 경로 오차의 원인이 된다. 용접이 진행되면서 국부적인 가열과 냉각으로 인해 작업물이 변형되고 남아 있는 용접선의 위치가 원래 위치에서 이동할 수 있다. 긴 용접부, 얇은 구조물, 비대칭 조립체는 특히 이러한 현상에 민감하다. 적절한 용접 순서(Weld Sequencing), 지그 설계(Fixture Design), 간헐 용접 전략(Intermittent Welding Strategy), 실시간 추적, 적응형 경로 보정(Adaptive Path Correction)을 이용하여 누적 변형의 영향을 줄일 수 있다.

충돌 회피(Collision Avoidance)는 정적인 제약뿐만 아니라 작업 중 변화하는 제약까지 고려해야 한다. 로봇 암, 토치 본체, 케이블 패키지(Cable Package), 센서, 작업물, 지그, 포지셔너가 운동 과정에서 서로 간섭할 수 있다. 공구 중심점 궤적이 기하학적으로 유효하더라도 로봇의 다른 부분에서 충돌이 발생할 수 있다. 따라서 오프라인 시뮬레이션과 온라인 모니터링(Online Monitoring)은 전체 용접 경로에 걸쳐 로봇 전체의 구성을 평가해야 한다.

용접 토치에는 전력선, 와이어, 가스, 냉각 라인, 센서 연결부가 포함되므로 케이블 관리(Cable Management)가 특히 중요하다. 로봇 자체가 관절 제한 범위 내에 있더라도 손목이 크게 회전하면 케이블 패키지가 비틀리거나 당겨질 수 있다. 경로 계획에서는 불필요한 손목 회전을 최소화하고 케이블 배선(Cable Routing)을 고려해야 한다. 전용 중공 손목 로봇(Hollow-Wrist Robot) 또는 통합 드레스 패키지(Integrated Dress Package)를 사용하면 반복 정밀도를 높이고 유지보수를 줄일 수 있다.

공정 모니터링(Process Monitoring)은 명령된 궤적이 허용 가능한 용접 결과를 생성했는지를 판단할 수 있는 근거를 제공한다. 로봇 위치, 진행 속도, 전류, 전압, 와이어 공급 속도, 용접선 추적 보정값, 고장 상태를 지속적으로 기록할 수 있다. 추가 센싱을 이용하여 온도, 비드 형상 또는 용융지 거동을 추정할 수도 있다. 모션 데이터와 공정 데이터를 결합하면 시간적으로 정렬된 기록(Time-Aligned Record)을 생성할 수 있으며 품질 평가와 생산 추적성(Production Traceability)을 지원할 수 있다.

비정상 상태(Abnormal Condition)가 발생하면 즉각적인 복구 동작(Recovery Behavior)이 필요하다. 아크 손실(Loss of Arc), 와이어 공급 중단, 과도한 용접선 추적 오차, 센서 손실, 충돌 검출, 보호 가스 공급 실패, 예상 범위를 벗어난 공정 값은 제어된 대응을 발생시켜야 한다. 심각도에 따라 시스템은 작업을 일시 정지하거나, 토치를 후퇴시키거나, 용접선을 다시 획득하거나, 중첩 구간(Overlap Region)에서 재시작하거나, 결함이 있는 용접을 계속 수행하는 대신 작업자의 개입을 요청할 수 있다.

품질 검사(Quality Inspection)는 용접 중 또는 용접 이후에 수행할 수 있다. 공정 중 모니터링(In-Process Monitoring)은 예상되는 전기적 또는 기하학적 신호와의 편차를 검출할 수 있으며, 후공정 검사(Post-Process Inspection)에서는 비전, 레이저 프로파일링(Laser Profiling), 초음파 검사(Ultrasonic Testing), 기타 비파괴 검사(Non-Destructive Testing) 방법을 사용할 수 있다. 비드 폭, 보강 높이(Reinforcement), 언더컷(Undercut), 연속성, 위치, 표면 결함을 합격 기준과 비교할 수 있으며 검사 결과를 기록된 궤적 및 공정 이력과 연결할 수 있다.

적응형 용접(Adaptive Welding)은 측정된 이음부 상태에 따라 공정 파라미터를 변경함으로써 이러한 아키텍처를 확장한다. 센싱을 통해 간극, 용접선 위치 또는 형상의 변화를 확인하면 시스템은 검증된 범위 내에서 진행 속도, 위빙, 와이어 공급, 토치 자세 또는 전기적 파라미터를 조정할 수 있다. 이를 통해 로봇은 단순한 궤적 재생 장치(Trajectory Playback Device)에서 벗어나 작업물의 물리적 변화에 대응하는 폐루프 공정 시스템(Closed-Loop Process System)으로 발전한다.

성능 평가는 로봇 성능과 용접 공정 성능을 모두 포함해야 한다. 주요 지표에는 경로 추종 오차(Path-Tracking Error), 방향 오차(Orientation Error), 용접선 추적 보정량(Seam-Tracking Correction), 속도 안정성(Velocity Stability), 이격 거리 편차, 아크 온 시간(Arc-On Time), 사이클 타임(Cycle Time), 재시작 빈도(Restart Frequency), 결함률(Defect Rate), 비드 형상 일관성(Bead Geometry Consistency), 최초 합격률(First-Pass Acceptance Rate)이 포함된다. 로봇 반복 정밀도만 평가해서는 충분하지 않으며 뛰어난 모션 정확도가 반드시 금속학적으로 허용 가능한 용접 품질을 보장하는 것은 아니다.

용접 토치 경로 추종 사례(Welding Torch Path-Following Case)는 산업용 조작이 기하학적 계획(Geometric Planning)과 물리적 공정 제어(Physical Process Control) 사이의 지속적인 상호작용으로 발전할 수 있음을 보여준다. 로봇은 공간 궤적을 따라 이동하면서 용접선 변화를 감지하고, 토치 방향을 유지하고, 속도를 제어하고, 용접 파라미터를 동기화하며, 외란(Disturbance)에 대응해야 한다. 위치 추정(Localization), 경로 계획(Path Planning), 용접선 추적, 모션 제어, 공정 모니터링, 검사, 복구 기능을 폐루프로 통합함으로써 실제 제조 환경의 불확실성에서도 일관된 용접 품질을 확보할 수 있다.

## 12.05. Cloth Folding Laundry Manipulation Case

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

의류 접기 및 세탁물 조작(Cloth Folding and Laundry Manipulation)은 로봇이 상호작용 과정에서 형상이 지속적으로 변화하는 재료를 인지하고, 파지하고, 펼치고, 정렬하고, 접고, 배치해야 하는 대표적인 변형 가능 물체 조작(Deformable-Object Manipulation) 작업이다. 강체(Rigid Object)와 달리 의류는 단순한 6자유도 변환(Six-Degree-of-Freedom Transform)으로 표현할 수 있는 고정된 자세를 유지하지 않는다. 의류의 상태는 주름(Wrinkle), 접힘(Fold), 자기 접촉(Self-Contact), 중력(Gravity), 마찰(Friction), 이전 조작 동작에 따라 달라진다.

일반적인 로봇 세탁물 처리 시스템(Robotic Laundry System)은 하나 또는 두 개의 매니퓰레이터 암(Manipulator Arm), RGB 또는 RGB-D 카메라, 작업 표면(Work Surface), 적절한 그리퍼(Gripper), 인지 및 계획 컴퓨터(Perception and Planning Computer)로 구성된다. 양팔 조작(Bimanual Manipulation)은 한 손으로 천을 잡거나 장력을 유지하면서 다른 손으로 정렬 또는 접기 작업을 수행할 수 있어 특히 유용하다. 대상 응용에 따라 작업 셀(Workcell)에 상부 카메라, 손목 카메라(Wrist Camera), 촉각 센싱(Tactile Sensing), 공기 보조 장치(Air-Assisted Device), 전용 접기 작업면을 추가할 수도 있다.

의류의 초기 상태(Initial State)는 매우 불확실한 경우가 많다. 세탁물은 여러 개의 의류가 서로 겹치거나 얽히고 대부분의 표면이 가려진 더미(Pile) 상태로 공급될 수 있다. 접기 작업을 시작하기 전에 로봇은 하나의 의류를 분리하고 보이는 영역 가운데 어느 부분이 해당 의류에 속하는지 추정해야 한다. 따라서 분할(Segmentation)은 첫 번째 주요 인지 문제가 되며, 특히 의류의 색상이 유사하거나 복잡한 패턴을 가지거나 상호 가림(Mutual Occlusion)이 심한 경우 더욱 어려워진다.

의류 더미에서 하나의 의류를 집는 작업은 강체 부품을 파지하는 것과 근본적으로 다르다. 로봇은 여러 겹의 천 또는 여러 개의 의류를 동시에 집을 수 있으며, 시각적으로 적절해 보이는 지점도 들어 올리는 과정에서 그리퍼로부터 미끄러질 수 있다. 따라서 파지 후보 위치(Grasp Candidate)는 접근 가능성(Accessibility), 국부 두께(Local Thickness), 가장자리일 가능성(Edge Likelihood), 마찰, 하나의 의류만 파지할 확률 등을 고려해야 한다. 불확실한 픽업 이후에는 들어 올려 관찰하기(Lift-and-Observe) 동작을 통해 추가적인 정보를 얻을 수 있다.

의류를 들어 올리면 중력(Gravity)에 의해 느슨한 천이 아래로 늘어지면서 의류의 형상이 단순화될 수 있다. 로봇은 이러한 효과를 활용하여 여러 층을 분리하고, 경계(Boundary)를 노출시키고, 복잡한 자기 접촉을 감소시킬 수 있다. 두 번째 로봇 암이 다른 지점을 파지하고 의류를 부드럽게 펼쳐 구조를 드러낼 수도 있다. 이러한 능동 인지(Active Perception)는 인지와 행동을 독립적인 단계로 취급하는 대신 조작 자체를 이용하여 센싱을 개선할 수 있음을 보여준다.

의류 상태 추정(Garment State Estimation)에는 기존 강체 자세 추정(Rigid-Body Pose Estimation)과 다른 표현 방법이 필요하다. 시스템은 모서리(Corner), 소매 끝(Sleeve End), 칼라(Collar), 허리 영역(Waist Region), 커프(Cuff), 밑단 지점(Hem Point)과 같은 키포인트(Keypoint)를 식별하고 이들의 관계를 통해 의류의 상태를 추론할 수 있다. 다른 접근법에서는 외곽선(Contour), 분할 마스크(Segmentation Mask), 밀집 대응(Dense Correspondence), 메시(Mesh), 학습된 잠재 표현(Learned Latent Representation)을 추정한다. 적절한 표현 방식은 작업이 분류, 펼치기, 정렬 또는 정밀 접기 중 무엇을 요구하는지에 따라 달라진다.

키포인트 검출(Keypoint Detection)은 구조화된 접기 작업에서 특히 유용하다. 수건(Towel)의 경우 네 개의 모서리만으로도 접기 전략을 결정하기에 충분할 수 있다. 셔츠(Shirt)는 어깨, 소매, 칼라, 하단 모서리를 포함하는 더 많은 의미론적 구조(Semantic Structure)가 필요하다. 실제 또는 합성 영상으로 학습된 비전 네트워크(Vision Network)는 주름과 부분적인 가림이 존재하더라도 이러한 랜드마크(Landmark)를 예측할 수 있지만, 잘못된 의미론적 키포인트가 완전히 부적절한 조작 순서로 이어질 수 있으므로 신뢰도(Confidence)를 함께 고려해야 한다.

심하게 구겨진 천에서는 정확한 기하학적 기준을 얻기 어렵기 때문에 일반적으로 정밀 접기 전에 펼치기(Unfolding)를 수행한다. 로봇은 흔들기(Shaking), 들어 올리기(Lifting), 펼치기(Spreading), 끌기(Dragging), 양팔 스트레칭(Bimanual Stretching)을 수행하여 보이는 표면적을 증가시키고 주름을 줄일 수 있다. 목표는 반드시 완벽하게 평평한 상태를 만드는 것이 아니라 중요한 랜드마크와 접는 선(Fold Line)을 안정적으로 추정할 수 있는 상태를 만드는 것이다.

작업 표면(Work Surface)은 유용한 물리적 제약(Physical Constraint)을 제공한다. 의류를 테이블 위에 놓으면 중력으로 인해 대부분의 천이 2차원 지지 평면(Support Plane)에 제한되므로 인지와 계획이 단순해진다. 로봇은 표면을 따라 모서리를 끌거나, 주름을 펴거나, 가장자리를 기준 방향에 정렬할 수 있다. 그러나 테이블의 마찰은 예상하지 못한 변형을 발생시킬 수도 있으므로 계획된 동작에서는 천이 접촉 중 어떻게 미끄러지고, 늘어나고, 회전하는지를 고려해야 한다.

그리퍼 설계(Gripper Design)는 조작 신뢰성에 큰 영향을 준다. 평행 조 그리퍼(Parallel-Jaw Gripper)는 안정적인 집기(Pinching)가 가능하지만 평평한 표면에서 얇은 천 한 겹만 집는 데 어려움이 있을 수 있다. 흡착 장치(Suction Device)는 넓은 영역에서 천을 들어 올릴 수 있지만 성능은 공기 투과성(Porosity)과 표면 질감에 따라 달라진다. 특수 핑거팁(Fingertip), 컴플라이언트 패드(Compliant Pad), 니들(Needle), 정전 접착(Electroadhesion), 하이브리드 그리퍼(Hybrid Gripper)를 이용하여 핸들링 성능을 향상시킬 수 있다. 엔드 이펙터(End Effector)는 의류 소재와 작업 요구사항에 맞게 선택해야 한다.

양팔 협조(Bimanual Coordination)는 수행 가능한 동작의 범위를 크게 확장한다. 두 개의 로봇 암은 서로 반대쪽 모서리를 파지하거나, 가장자리에 장력을 형성하거나, 공중에 매달린 의류를 회전시키거나, 정렬을 유지하면서 접기를 수행할 수 있다. 한쪽 로봇 암의 움직임은 다른 로봇 암이 관측하는 천의 상태를 변화시키므로 각 암을 독립적으로 계획하는 것은 충분하지 않다. 협조 궤적(Coordinated Trajectory)은 양손의 상대 위치, 천의 장력(Fabric Tension), 충돌 회피(Collision Avoidance), 파지점 사이에서 예상되는 변형을 함께 고려해야 한다.

접기(Folding)는 접는 선(Fold Line)과 목표 영역(Target Region)에 의해 정의되는 일련의 기하학적 변환(Geometric Transformation)으로 표현할 수 있다. 직사각형 수건의 경우 한쪽 면을 접는 선을 기준으로 반사시켜 반대편 영역 위에 배치할 수 있다. 더 복잡한 의류는 몸통을 접기 전에 소매를 안쪽으로 접는 것과 같은 의미론적 접기 순서(Semantic Fold Sequence)를 필요로 한다. 계획기(Planner)는 원하는 최종 형상을 중간 천 상태와 이에 대응하는 로봇 동작으로 변환한다.

실제 천의 궤적(Fabric Trajectory)은 단순한 기하학적 반사만으로 완벽하게 예측할 수 없다. 소재의 강성(Stiffness), 두께, 마찰, 탄성(Elasticity), 기존 주름이 천의 이동과 안착 방식에 영향을 준다. 따라서 로봇은 명목상의 접기 동작을 실행하고 결과를 관측한 다음 다음 단계로 진행하기 전에 남아 있는 정렬 오차를 수정할 수 있다. 이러한 인지-행동-피드백 순환(Perception-Action-Feedback Cycle)은 명령된 그리퍼 궤적이 정확한 천의 형상을 생성한다고 가정하는 방식보다 강건하다.

힘 및 촉각 센싱(Force and Tactile Sensing)은 천과 상호작용하는 동안 신뢰성을 향상시킬 수 있다. 그리퍼 힘(Gripper Force)을 이용하여 천이 실제로 파지되었는지 판단할 수 있으며, 촉각 배열(Tactile Array)은 접촉 위치, 미끄러짐(Slip), 천의 층 두께(Layer Thickness)를 추정할 수 있다. 양팔 스트레칭 중에는 측정된 장력을 이용하여 민감한 의류를 변형시킬 정도로 과도하게 당기는 것을 방지할 수 있다. 이러한 신호는 그리퍼로 인해 천이 가려지거나 시각 정보만으로 접촉 상태를 충분히 판단할 수 없을 때 비전을 보완한다.

천은 명확한 시각적 변화 없이 핑거 사이에서 점진적으로 미끄러질 수 있으므로 미끄러짐 검출(Slip Detection)이 특히 중요하다. 촉각 압력(Tactile Pressure), 그리퍼 위치, 모터 전류(Motor Current), 관측된 키포인트 움직임의 변화를 통해 파지 안정성 저하를 검출할 수 있다. 제어기는 안전한 범위 내에서 파지력을 높이거나, 가속도를 낮추거나, 천을 다시 파지할 수 있다. 제어되지 않은 미끄러짐을 방지하면 추정된 의류 상태와 실제 상태 사이의 대응 관계를 유지하는 데 도움이 된다.

가능한 모든 천의 상태를 수작업으로 모델링하는 것은 현실적으로 어렵기 때문에 학습 기반 접근법(Learning-Based Approach)의 활용이 증가하고 있다. 모방 학습(Imitation Learning)은 사람의 시연(Human Demonstration)으로부터 접기 전략을 학습할 수 있으며, 강화학습(Reinforcement Learning)은 시뮬레이션 또는 실제 물리적 상호작용을 통해 조작 정책(Manipulation Policy)을 최적화할 수 있다. 비전-언어-행동(Vision-Language-Action) 또는 트랜스포머 기반 정책(Transformer-Based Policy)은 여러 작업 유형에서 시각적 관측, 의미론적 의류 구조, 조작 행동 사이의 관계를 학습할 수도 있다.

시뮬레이션(Simulation)은 대규모 학습 데이터를 제공할 수 있지만 변형 가능 물체 시뮬레이션(Deformable-Object Simulation)은 계산량이 많다. 정확한 천의 거동을 표현하려면 굽힘(Bending), 신장(Stretching), 자기 충돌(Self-Collision), 마찰, 접촉, 경우에 따라 이방성 재료 특성(Anisotropic Material Property)을 모델링해야 한다. 단순화된 시뮬레이터는 다양한 상태를 효율적으로 생성할 수 있으며, 도메인 무작위화(Domain Randomization)를 통해 천의 물성, 색상, 텍스처, 조명, 카메라 조건을 변화시킬 수 있다. 그러나 실제 천의 거동은 시뮬레이션과 크게 다를 수 있으므로 실제 환경에서의 미세 조정(Physical Fine-Tuning)이 여전히 중요하다.

합성 데이터(Synthetic Data)는 조작 동역학과 독립적으로 인지 모델을 학습하는 데에도 활용할 수 있다. 가상 의류(Virtual Garment)를 수천 가지 형상, 접힘 상태, 시점, 텍스처, 가림 조건으로 렌더링하면서 분할 마스크, 깊이(Depth), 키포인트, 표면 대응(Surface Correspondence), 접힘 라벨(Fold Label)을 자동으로 생성할 수 있다. 이러한 데이터셋은 수작업 어노테이션(Manual Annotation)의 부담을 줄이고 실제 세탁 환경에서 체계적으로 수집하기 어려운 다양한 상태를 인지 모델에 제공한다.

검증(Verification)은 주요 조작 단계가 완료될 때마다 수행되어야 한다. 픽업 이후에는 하나의 의류만 획득했는지 확인하고, 펼치기 이후에는 충분한 표면 영역과 키포인트가 보이는지 검사한다. 정렬 이후에는 가장자리와 랜드마크 오차를 측정하며, 각각의 접기 이후에는 결과 실루엣(Silhouette)과 모서리 위치를 목표 중간 상태와 비교할 수 있다. 이를 통해 초기 단계에서 발생한 오류가 전체 접기 과정으로 전파되는 것을 방지할 수 있다.

변형 가능 물체 조작에는 피할 수 없는 불확실성이 존재하기 때문에 복구 동작(Recovery Action)이 필수적이다. 여러 개의 의류가 동시에 들어 올려지면 로봇은 흔들거나, 분리하거나, 다시 더미에 내려놓을 수 있다. 키포인트가 사라지면 더 나은 관측을 위해 의류를 재배치할 수 있다. 접기 결과에서 과도한 정렬 오차가 발생하면 전체 과정을 폐기하는 대신 문제가 발생한 영역을 다시 펼치고, 표면을 정리하고, 모서리를 재파지하여 해당 동작을 반복할 수 있다.

작업 성공 여부는 로봇이 사전에 정의된 모션을 완료했는지만으로 평가해서는 안 된다. 유용한 성능 지표에는 단일 의류 픽업 성공률(Single-Item Pickup Success), 키포인트 위치 추정 오차(Keypoint Localization Error), 펼치기 품질(Unfolding Quality), 가시 영역 비율(Visible-Area Ratio), 모서리 정렬 오차(Corner Alignment Error), 접는 선 편차(Fold-Line Deviation), 최종 실루엣 유사도(Final Silhouette Similarity), 주름 수준(Wrinkle Level), 사이클 타임(Cycle Time), 재파지 빈도(Regrasp Frequency), 복구율(Recovery Rate)이 포함된다. 상업용 세탁 응용에서는 적층 일관성(Stack Consistency)과 시간당 처리 의류 수 역시 중요한 시스템 수준의 지표이다.

서로 다른 의류는 서로 다른 조작 전략을 필요로 한다. 수건과 시트(Sheet)는 주로 모서리와 가장자리의 기하 구조가 중요하지만 셔츠와 바지는 소매, 바짓단, 허리선, 칼라에 대한 의미론적 이해(Semantic Understanding)가 필요하다. 섬세한 직물은 낮은 파지력이 필요할 수 있고 무거운 소재는 더 강한 파지와 넓은 로봇 작업 공간을 요구한다. 따라서 범용 세탁 시스템(General Laundry System)은 의류 종류를 분류하고 이에 따라 인지, 파지, 접기 정책을 적응시켜야 한다.

세탁물 조작은 하나의 최종 목표뿐만 아니라 상태 전환(State Transition)이 중요하다는 점도 보여준다. 의류는 더미 상태(Pile State)에서 분리 상태(Isolated State), 공중에 매달린 상태(Suspended State), 부분적으로 펼쳐진 상태(Partially Unfolded State), 평평한 상태(Flat State), 정렬된 상태(Aligned State), 접힌 상태(Folded State), 적층 상태(Stacked State)로 진행할 수 있다. 각 단계에서는 관측 가능한 정보와 수행 가능한 행동이 달라진다. 이러한 상태를 명시적으로 모델링하면 작업이 진행됨에 따라 제어기가 적절한 인지 및 조작 전략을 선택할 수 있다.

따라서 실용적인 생산 아키텍처(Production Architecture)는 관측(Observe), 상태 추정(Estimate State), 행동 선택(Select Action), 조작(Manipulate), 검증(Verify)이 반복되는 폐루프(Closed Loop)로 동작한다. 천의 상태는 접촉이 발생할 때마다 정확하게 예측하기 어려운 방식으로 변화할 수 있으므로 중요한 접촉 이후에는 월드 모델(World Model)을 갱신해야 한다. 신뢰도 추정(Confidence Estimation)을 이용하면 로봇이 계획된 순서를 계속 진행할지, 추가 정보를 얻기 위한 행동을 수행할지, 또는 복구 동작을 실행할지를 결정할 수 있다.

의류 접기 및 세탁물 조작 사례(Cloth-Folding and Laundry Manipulation Case)는 고급 산업용 조작(Advanced Industrial Manipulation)의 핵심적인 도전 과제를 보여준다. 로봇은 상호작용 자체에 의해 상태가 형성되고 변화하는 물체를 제어해야 한다. 신뢰성 있는 성능은 능동 인지, 변형 상태 추정(Deformable-State Estimation), 적절한 파지, 양팔 협조, 컴플라이언트 제어(Compliant Control), 학습(Learning), 검증, 복구 기능의 통합을 통해 구현된다. 이러한 원리는 세탁물뿐만 아니라 섬유(Textile), 유연 포장재(Flexible Packaging), 케이블(Cable), 시트, 멤브레인(Membrane) 등 미래 자동화 생산에서 다루어야 할 다양한 변형 가능 재료로 확장될 수 있다.

## 12.06. Cable Routing Harness Assembly Manipulation Case

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}

케이블 라우팅 및 하네스 조립(Cable Routing and Harness Assembly)은 로봇이 핸들링 과정에서 형상이 변화하는 유연 부품(Flexible Component)을 파지하고, 방향을 조정하고, 경로를 따라 배치하고, 삽입하고, 클립에 체결하고, 커넥터를 연결한 후 결과를 검증해야 하는 대표적인 변형 가능 물체 조작(Deformable-Object Manipulation) 작업이다. 강체 부품과 달리 케이블은 굽힘(Bending), 비틀림(Twisting), 중력(Gravity), 마찰(Friction), 접촉(Contact)에 따라 형상이 달라지므로 하나의 자세(Pose)만으로 충분히 표현할 수 없다. 따라서 신뢰성 있는 자동화를 위해서는 인지(Perception), 조작 계획(Manipulation Planning), 힘 제어(Force Control), 검증(Verification)이 연속적인 폐루프(Closed Loop)로 동작해야 한다.

일반적인 하네스 조립 셀(Harness Assembly Cell)은 하나 또는 두 개의 로봇 암(Robot Arm), RGB-D 또는 스테레오 카메라(Stereo Camera), 힘-토크 센서(Force-Torque Sensor), 적절한 그리퍼(Gripper), 지그(Fixture), 클립(Clip), 커넥터(Connector), 하네스 보드(Harness Board) 또는 제품 구조물로 구성된다. 양팔 조작(Bimanual Manipulation)은 한쪽 로봇 암이 장력(Tension)을 유지하거나 기준점을 고정하는 동안 다른 로봇 암이 자유 구간을 라우팅할 수 있어 유용하다. 손목 카메라(Wrist Camera)와 촉각 센서(Tactile Sensor)는 커넥터, 클립, 케이블 구간이 상부 비전에서 가려지는 경우 국부적인 정보를 제공할 수 있다.

케이블의 초기 형상(Initial Configuration)은 매우 불확실할 수 있다. 유연한 케이블은 감긴 상태(Coiled State), 꼬인 상태(Twisted State), 자기 자신과 교차된 상태 또는 다른 하네스 분기와 섞인 상태로 공급될 수 있다. 라우팅을 시작하기 전에 로봇은 올바른 케이블을 식별하고 보이는 중심선(Centerline), 끝점(Endpoint), 커넥터, 분기(Branch), 관련 체결 특징(Attachment Feature)을 추정해야 한다. 얇은 케이블은 영상에서 차지하는 픽셀 수가 적고 배경이나 주변 전선과 색상이 유사할 수 있기 때문에 분할(Segmentation)이 특히 어렵다.

케이블 상태 추정(Cable State Estimation)은 일반적으로 강체 자세 대신 순서가 정의된 곡선(Ordered Curve)으로 물체를 표현한다. 비전(Vision)을 이용하여 2차원 또는 3차원에서 케이블 형상을 근사하는 중심선이나 일련의 키포인트(Keypoint)를 추출할 수 있다. 끝점과 커넥터는 다음 조작 동작을 결정하는 경우가 많기 때문에 특별한 의미론적 라벨(Semantic Label)을 부여한다. 분기형 하네스(Branched Harness)의 경우 이러한 표현을 접합점(Junction), 분기, 커넥터, 클립, 라우팅 목표를 포함하는 그래프(Graph)로 확장할 수 있다.

가림(Occlusion)은 완전한 상태 복원을 어렵게 만든다. 케이블의 일부 구간은 다른 분기 아래, 지그 뒤쪽 또는 라우팅 채널 내부에서 보이지 않을 수 있다. 따라서 시스템은 관측된 형상(Observed Geometry)과 추론된 형상(Inferred Geometry)을 구분하고 보이지 않는 구간에 대한 불확실성(Uncertainty)을 유지해야 한다. 케이블을 들어 올리거나, 분기를 이동시키거나, 카메라 시점을 변경하거나, 케이블에 장력을 가하여 구조를 보다 쉽게 관측하도록 만드는 능동 인지(Active Perception)를 통해 이러한 불확실성을 줄일 수 있다.

케이블 파지 계획(Cable Grasp Planning)은 다음 작업의 종류에 크게 의존한다. 커넥터 삽입에는 커넥터 근처를 파지하는 것이 적합할 수 있지만 클립을 따라 라우팅하는 경우에는 케이블 중간 지점을 잡아야 할 수 있다. 로봇은 국부 곡률(Local Curvature), 사용 가능한 여유 공간(Clearance), 케이블 직경, 파지 안정성(Grasp Stability), 절연체(Insulation) 손상 위험을 고려해야 한다. 작업 영역에서 너무 멀리 파지하면 제어되지 않은 굽힘이 발생할 수 있고, 지나치게 가까이 파지하면 목표 특징에 접근하는 것을 방해할 수 있다.

커넥터 핸들링(Connector Handling)은 변형 가능한 케이블 문제에 강체 조작(Rigid-Body Manipulation)을 추가한다. 커넥터는 명확한 위치와 방향을 갖지만 연결된 케이블에서 발생하는 힘과 모멘트가 삽입 과정에 영향을 준다. 비전을 이용하여 커넥터 자세(Connector Pose)를 추정하고 로봇은 결합 인터페이스(Mating Interface)에 정렬된 접근 경로를 계획할 수 있다. 동시에 케이블의 느슨한 정도(Slack)를 관리하여 장력이 커넥터를 회전시키거나 의도된 삽입 궤적에서 벗어나게 하지 않도록 해야 한다.

라우팅(Routing)은 일반적으로 일련의 기하학적 및 위상학적 제약(Geometric and Topological Constraint)에 의해 정의된다. 케이블은 장애물의 지정된 측면을 지나고, 가이드(Guide)를 통과하고, 브래킷(Bracket) 아래를 지나며, 모서리를 돌아 여러 클립에 삽입된 후 커넥터까지 도달해야 할 수 있다. 최종 케이블 형상만 일치시키는 것으로는 충분하지 않다. 끝점이 정확한 위치에 있더라도 장애물을 잘못된 방향으로 우회했다면 위상 구조(Topology)가 잘못될 수 있기 때문이다.

따라서 하네스 조립에서는 위상학적 추론(Topological Reasoning)이 중요하다. 계획기(Planner)는 케이블을 제품 구조물 사이로 이동시키면서 위(Over), 아래(Under), 내부(Inside), 외부(Outside), 왼쪽(Left), 오른쪽(Right)과 같은 관계를 유지해야 한다. 잘못된 라우팅은 교차(Crossing), 간섭(Interference), 과도한 굽힘 또는 후속 생산 공정의 조립 문제를 발생시킬 수 있다. 그래프 기반 월드 모델(Graph-Based World Model)은 라우팅 랜드마크와 연결 관계를 표현하고, 기하학적 계획(Geometric Planning)은 그 사이의 정밀한 3차원 운동을 처리할 수 있다.

양팔 조작(Bimanual Manipulation)은 케이블 형상과 장력을 직접적으로 제어할 수 있도록 한다. 한쪽 로봇 암은 이미 라우팅된 구간을 고정하고 다른 로봇 암은 다음 가이드 방향으로 다른 구간을 이동시킬 수 있다. 두 로봇 암은 케이블을 부드럽게 당겨 루프(Loop)를 제거하거나, 이전에 완료된 라우팅을 방해하지 않으면서 커넥터 위치를 변경하도록 협조할 수도 있다. 계획 과정에서는 로봇 암 사이의 충돌, 케이블 장력, 두 파지점 사이에서 발생하는 변형을 고려해야 한다.

장력 관리(Tension Management)는 핵심적인 제어 문제이다. 장력이 너무 낮으면 루프와 불확실한 케이블 움직임이 발생하고, 장력이 지나치게 높으면 도체(Conductor)가 손상되거나 커넥터가 당겨지거나 클립이 변형되거나 이미 설치된 구간이 이탈할 수 있다. 힘-토크 센서를 이용하여 상호작용 힘(Interaction Force)을 추정하고 컴플라이언트 제어(Compliant Control)를 통해 로봇 이동 중 장력을 조절할 수 있다. 요구 장력은 케이블을 펴는지, 라우팅하는지, 클립에 삽입하는지, 커넥터를 체결하는지에 따라 달라질 수 있다.

케이블을 클립에 삽입하는 작업(Clip Insertion)은 접촉 중심 조작(Contact-Rich Manipulation)이다. 로봇은 케이블을 클립 개구부 위에 배치한 후 절연체나 지그를 손상시키지 않으면서 케이블이 제자리에 체결되도록 적절한 방향으로 힘을 가해야 한다. 위치 불확실성과 케이블 컴플라이언스 때문에 순수한 위치 제어만으로는 안정적인 삽입이 어렵다. 힘 제어, 임피던스 제어(Impedance Control), 어드미턴스 제어(Admittance Control)를 이용하면 접촉 과정에서 작은 정렬 오차를 엔드 이펙터가 수용할 수 있다.

클립 삽입은 여러 신호를 이용하여 검증할 수 있다. 특징적인 힘 피크(Force Peak)가 나타난 후 힘이 갑자기 감소하면 정상적인 스냅 결합(Snap Engagement)이 이루어졌음을 의미할 수 있다. 비전은 케이블이 클립 내부에 위치하는지를 확인할 수 있으며 촉각 센싱은 국부적인 접촉 변화를 감지할 수 있다. 클립 설계, 케이블 직경, 재료 특성에 따라 삽입 신호가 달라지므로 하나의 신호에 의존하는 것보다 기하학적 정보와 힘 정보를 결합하는 것이 더욱 신뢰성이 높다.

커넥터 삽입(Connector Insertion)은 더욱 정밀한 제어를 필요로 한다. 로봇은 삽입력을 가하기 전에 병진 및 회전 자유도(Translational and Rotational Degrees of Freedom)를 정렬해야 한다. 모따기(Chamfer)와 기계적 가이드(Mechanical Guide)는 작은 오차를 허용할 수 있지만 지나친 정렬 오차는 단자(Terminal)를 휘게 하거나 잠금 구조(Locking Feature)를 손상시킬 수 있다. 컴플라이언트 탐색 동작(Compliant Search Motion)을 이용하여 결합 인터페이스를 찾은 후 힘과 변위를 모니터링하면서 삽입을 진행할 수 있다.

삽입 완료(Insertion Completion)는 명령된 운동만으로 추정하지 않고 검증해야 한다. 최종 깊이(Final Depth), 힘 프로파일(Force Profile), 커넥터 자세, 래치 결합(Latch Engagement), 청각 또는 촉각으로 감지되는 클릭(Click) 등이 성공적인 연결의 근거가 될 수 있다. 완성된 하네스 조립에는 전기적 연속성 검사(Electrical Continuity Testing)를 추가적인 검증 계층으로 적용할 수 있다. 삽입력이 예상보다 일찍 증가하면 제어기는 잘못된 상태에서 커넥터를 강제로 밀어 넣지 않고 정지한 후 후퇴해야 한다.

비틀림 제어(Twist Control) 역시 하네스 조작에서 중요한 요소이다. 커넥터를 회전시키거나 분기의 위치를 변경하면 케이블 또는 다중 전선 번들(Multi-Wire Bundle)에 비틀림이 누적될 수 있다. 과도한 비틀림은 응력을 증가시키고 라우팅 형상을 변화시키며 후속 조립을 어렵게 할 수 있다. 시스템은 커넥터 방향과 케이블 형상을 추적하여 비틀림 상태(Torsional State)를 추정하고, 필요하면 파지 방향을 의도적으로 회전시키거나 케이블을 놓았다가 다시 파지하여 비틀림을 해소할 수 있다.

최소 굽힘 반경(Minimum Bend Radius)은 계획 및 실행 전체 과정에서 준수되어야 한다. 과도하게 날카로운 굽힘은 즉각적인 외관상 결함이 나타나지 않더라도 도체, 차폐층(Shielding), 광섬유(Optical Fiber), 절연체를 손상시킬 수 있다. 따라서 경로 계획은 지나친 곡률을 갖는 형상에 페널티를 부여하고 검증된 기계적 한계 내에서 케이블 형상을 유지해야 한다. 케이블 종류에 따라 요구되는 굽힘 제약이 달라질 수 있으므로 소재별 조작 파라미터(Material-Specific Manipulation Parameter)가 필요하다.

하네스 보드와 지그는 알려진 라우팅 랜드마크(Routing Landmark)를 제공하여 작업을 단순화할 수 있다. 페그(Peg), 채널(Channel), 클립, 커넥터 마운트(Connector Mount)는 가능한 케이블 형상을 제한하고 불확실성을 줄인다. 그러나 지그 공차(Fixture Tolerance)는 실제 형상에 여전히 영향을 준다. 상호작용 전에 비전 또는 터치 센싱(Touch Sensing)으로 주요 특징을 위치 추정하면 모든 클립과 커넥터가 정확한 CAD 위치에 있다고 가정하는 대신 명목 라우팅 목표를 실제 환경에 맞게 갱신할 수 있다.

모션 계획(Motion Planning)은 로봇 형상뿐만 아니라 케이블 형상도 고려해야 한다. 로봇 암 자체에는 충돌이 없는 궤적이라도 케이블이 이동 과정에서 장애물을 휩쓸거나, 구조물에 걸리거나, 이미 라우팅된 분기를 잡아당길 수 있다. 따라서 계획기는 이동 과정에서 케이블이 차지하는 영역(Cable Envelope)을 근사적으로 예측할 필요가 있다. 정확한 변형 시뮬레이션이 실시간 처리에 지나치게 많은 계산을 요구하는 경우 보수적인 스윕 볼륨 모델(Swept-Volume Model)을 이용하여 실용적인 충돌 보호를 구현할 수 있다.

변형 가능 물체 시뮬레이션(Deformable-Object Simulation)은 계획, 학습, 검증을 지원할 수 있다. 케이블 모델은 강체 세그먼트 체인(Chain of Rigid Segments), 로드(Rod), 입자(Particle), 유한요소(Finite Element), 미분 가능한 물리 표현(Differentiable Physical Representation) 등을 이용하여 근사할 수 있다. 중요한 물성에는 굽힘 강성(Bending Stiffness), 비틀림 강성(Torsional Stiffness), 인장 저항(Stretching Resistance), 마찰, 접촉 특성이 포함된다. 시뮬레이션을 통해 어려운 형상을 생성하고 배치 전에 조작 전략을 평가할 수 있지만 실제 환경에 대한 캘리브레이션은 여전히 필요하다.

케이블 동역학을 정확하게 예측하기 어려운 영역에서는 학습 기반 방법(Learning-Based Method)이 해석적 모델링(Analytical Modeling)을 보완할 수 있다. 모방 학습(Imitation Learning)은 사람의 시연으로부터 라우팅 기술을 학습할 수 있으며, 강화학습(Reinforcement Learning)은 클립 삽입이나 케이블 펴기와 같은 국부 동작을 최적화할 수 있다. 학습 기반 인지 모델은 케이블 중심선, 끝점, 커넥터, 가려진 구간을 추정할 수 있다. 하이브리드 아키텍처(Hybrid Architecture)는 학습 기반 인지와 기하학적 계획, 힘 제어 실행을 결합하는 방식으로 구성할 수 있다.

검증(Verification)은 전체 조립이 완료된 이후에만 수행하는 것이 아니라 각각의 라우팅 마일스톤(Routing Milestone) 이후에 수행해야 한다. 시스템은 케이블이 장애물의 올바른 측면을 통과했는지, 목표 가이드에 들어갔는지, 클립에 정상적으로 결합되었는지, 적절한 여유 길이(Slack)를 유지하는지, 올바른 커넥터에 도달했는지를 확인할 수 있다. 단계별 검증은 초기 위상 오류가 이후의 하네스 분기 아래에 가려져 대규모 재작업으로 이어지는 것을 방지한다.

유연 물체 조작에서는 예상하지 못한 형상이 불가피하게 발생하므로 복구 동작(Recovery Behavior)이 필수적이다. 케이블이 그리퍼에서 미끄러지면 로봇은 보이는 구간에서 다시 파지할 수 있다. 루프가 형성되면 해당 영역을 들어 올려 펴는 동작을 수행할 수 있다. 클립 삽입 실패 시 국부 재정렬과 재시도를 수행하고, 커넥터에서 비정상적인 힘이 검출되면 후퇴한 후 비전으로 다시 검사하고 제어된 삽입을 다시 시도할 수 있다.

성능 평가는 최종 조립 완료 여부만을 대상으로 해서는 안 된다. 주요 지표에는 케이블 분할 정확도(Cable Segmentation Accuracy), 끝점 위치 추정 오차(Endpoint Localization Error), 라우팅 성공률(Routing Success Rate), 클립 결합률(Clip Engagement Rate), 커넥터 삽입 성공률(Connector Insertion Success), 최대 상호작용 힘(Peak Interaction Force), 장력 편차(Tension Variation), 최소 굽힘 반경 위반(Minimum Bend-Radius Violation), 재파지 빈도(Regrasp Frequency), 사이클 타임(Cycle Time), 복구율(Recovery Rate), 최종 위상 정확도(Final Topology Correctness)가 포함된다. 이러한 지표를 통해 공정이 기하학적으로 성공했을 뿐만 아니라 기계적으로도 안전한지를 평가할 수 있다.

생산 추적성(Production Traceability)을 이용하면 각 하네스를 라우팅 결과, 커넥터 삽입력, 클립 삽입 시그니처(Clip Insertion Signature), 비전 검사, 전기적 검사, 재시도 횟수, 검출된 이상 상태와 연결할 수 있다. 이러한 정보를 통계적으로 분석하면 지그 드리프트(Fixture Drift), 그리퍼 마모(Gripper Wear), 케이블 물성 변화, 특정 라우팅 위치에서 반복되는 문제를 확인할 수 있다. 따라서 조작 시스템은 단순한 조립 장치를 넘어 제조 품질 정보를 생성하는 데이터 소스(Data Source)의 역할도 수행한다.

케이블 라우팅 및 하네스 조립(Cable Routing and Harness Assembly)은 산업용 조작이 기하학적 추론(Geometric Reasoning), 위상학적 추론(Topological Reasoning), 물리적 추론(Physical Reasoning)을 어떻게 통합해야 하는지를 보여준다. 로봇은 케이블 구간이 어디에 있는지, 환경을 통해 어떤 관계로 지나가야 하는지, 유연한 물체와 상호작용할 때 힘이 어떻게 전달되는지를 동시에 이해해야 한다. 인지, 양팔 조작, 컴플라이언트 제어, 위상 인식 계획(Topology-Aware Planning), 검증, 복구를 폐루프로 통합함으로써 자동차, 전자제품, 항공우주 시스템, 기계 장비 및 미래 로봇 제조 환경에서 복잡한 배선 작업을 자동화하기 위한 기반을 구축할 수 있다.

## 12.07. Humanoid Bimanual Assembly Task Case

![](images/image7.png){width="7.268055555555556in" height="7.268055555555556in"}

휴머노이드 양팔 조립(Humanoid Bimanual Assembly)은 두 개의 로봇 암(Robot Arm), 손(Hand), 몸통(Torso), 인지(Perception), 전신 제어(Whole-Body Control)가 협력하여 원래 사람을 위해 설계된 작업 공간에서 부품을 조립하는 고급 산업용 조작(Advanced Industrial Manipulation) 작업이다. 고정형 단일 로봇 암과 달리 휴머노이드는 한 손으로 부품을 잡은 상태에서 반대쪽 손으로 다른 부품을 조작할 수 있다. 이를 통해 인간과 유사한 작업 흐름을 구현할 수 있지만 강력한 협조 제어(Coordination), 안정성(Stability), 충돌 회피(Collision Avoidance), 힘 제어(Force Control)가 요구된다.

일반적인 휴머노이드 조립 플랫폼(Humanoid Assembly Platform)은 두 개의 다자유도 로봇 암(Multi-Degree-of-Freedom Arm), 덱스터러스 또는 적응형 손(Dexterous or Adaptive Hand), 움직일 수 있는 몸통, 머리 장착 카메라(Head-Mounted Camera), 손목 카메라(Wrist Camera), 힘-토크 센서(Force-Torque Sensor), 관절 토크 센싱(Joint Torque Sensing)으로 구성된다. 일부 시스템은 손가락이나 손바닥에 촉각 센서(Tactile Sensor)를 추가한다. 조작 제어기는 이러한 센싱 채널을 기구학(Kinematics), 모션 계획(Motion Planning), 파지 제어(Grasp Control), 작업 순서 계획(Task Sequencing)과 통합하면서 장비 및 사람 주변에서 안전한 동작을 유지해야 한다.

양팔 조립은 작업 공간과 관련 부품을 이해하는 것에서 시작된다. RGB-D 카메라 또는 스테레오 비전(Stereo Vision)은 부품, 지그(Fixture), 공구(Tool), 커넥터(Connector), 조립 목표를 검출하고 3차원 자세(Three-Dimensional Pose)를 추정할 수 있다. 의미론적 인지(Semantic Perception)는 물체의 위치뿐만 아니라 기능적 역할까지 판단한다. 하나의 부품은 베이스 부품(Base Part), 삽입 부품(Insertion Part), 체결 부품(Fastener), 공구, 지지 물체(Support Object), 임시 지그(Temporary Fixture) 등으로 구분될 수 있다.

두 손은 일반적으로 동일한 동작을 수행하기보다 상호 보완적인 역할(Complementary Role)을 수행한다. 한 손은 하우징(Housing)을 고정하고 다른 손은 커넥터를 삽입하거나, 한 손이 패널(Panel)의 위치를 유지하는 동안 반대쪽 손이 체결 부품을 설치할 수 있다. 또는 한 손이 공구를 잡고 다른 손이 작업물을 조작할 수 있다. 이러한 역할은 작업 과정에서 변경될 수 있으므로 효과적인 협조를 위해서는 어떤 손이 각 물체를 지지하고, 조작하고, 재배치하고, 전달해야 하는지 명시적으로 판단해야 한다.

작업 할당(Task Allocation)은 도달 가능성(Reachability), 조작성(Dexterity), 물체 형상, 필요한 힘, 휴머노이드의 현재 자세에 따라 달라진다. 모든 작업을 단순히 가장 가까운 손에 영구적으로 할당하면 불리한 자세 또는 이후의 운동 제약이 발생할 수 있다. 계획기(Planner)는 여러 미래 단계에 걸쳐 왼손과 오른손의 다양한 작업 할당을 평가하고 로봇 암 교차, 불필요한 재파지(Regrasping), 관절 한계(Joint Limit) 접근을 줄이는 작업 순서를 선택할 수 있다.

공유 물체 조작(Shared-Object Manipulation)은 두 손이 동일한 부품을 동시에 파지할 때 발생한다. 대형 패널, 긴 부품, 유연한 조립체 또는 무거운 물체는 양팔의 협조된 지지가 필요할 수 있다. 이때 두 손 사이의 상대 변환(Relative Transformation)은 물체가 목표 궤적을 따라 이동하는 동안 물체의 기하 구조와 일치하도록 유지되어야 한다. 작은 동기화 오차(Synchronization Error)도 물체, 그리퍼(Gripper), 로봇 관절에 부하를 가하는 내부 힘(Internal Force)을 발생시킬 수 있다.

공유 물체를 조작할 때는 폐쇄 체인 기구학(Closed-Chain Kinematics)이 중요해진다. 두 손이 동일한 부품을 강체적으로 파지하면 두 로봇 암과 물체가 제약된 기구학적 루프(Kinematic Loop)를 형성한다. 독립적인 궤적 명령은 이러한 제약과 서로 충돌할 수 있다. 따라서 협조 제어(Coordinated Control)는 물체 자세, 파지 변환(Grasp Transform), 관절 한계, 충돌 제약, 허용 가능한 내부 힘을 만족하면서 두 로봇 암의 운동을 동시에 계산해야 한다.

전신 운동(Whole-Body Motion)을 이용하면 조작 가능한 작업 공간을 확장할 수 있다. 휴머노이드는 로봇 암만 사용하는 대신 몸통을 회전하거나 이동하고, 자세를 조절하며, 이동형 시스템의 경우 베이스(Base) 또는 발의 위치를 변경할 수 있다. 작은 몸통 움직임만으로도 로봇 암의 조작성(Manipulability)을 크게 향상시키고 관절 부하를 줄일 수 있다. 따라서 전신 계획(Whole-Body Planning)은 조립 작업을 전체 기구학 구조가 참여하는 협조 운동 문제로 다룬다.

자립형 휴머노이드(Free-Standing Humanoid)의 경우 조작 작업은 균형(Balance)도 고려해야 한다. 삽입, 체결, 밀기, 당기기 과정에서 발생하는 힘은 반력(Reaction Force)을 생성하고, 이 힘은 몸 전체를 통해 지지 접촉점(Support Contact)으로 전달된다. 제어기는 작업을 수행하는 동안 질량 중심(Center of Mass)과 접촉력을 안정 영역(Stable Region) 내부에 유지해야 한다. 큰 힘이 필요한 조작에서는 충분한 안정 여유(Stability Margin)를 확보하기 위해 자세 조정이나 환경의 추가적인 지지가 필요할 수 있다.

파지 선택(Grasp Selection)은 이후의 조립 동작과 밀접하게 연결되어 있다. 부품을 운반하기에는 적합한 파지가 삽입이나 체결에 필요한 표면을 가릴 수 있다. 따라서 계획기는 후속 작업, 공구 접근성(Tool Accessibility), 예상 접촉력, 핸드오버(Handover) 요구사항을 고려하여 파지 자세를 선택해야 한다. 작업 지향 파지(Task-Oriented Grasping)를 이용하면 재파지 횟수를 줄이고 전체 조립 순서의 연속성을 향상시킬 수 있다.

부품의 방향을 변경해야 하거나 현재 사용 중인 손이 기존 파지 영역에 접근해야 하는 경우에는 재파지(Regrasping)가 필요하다. 로봇은 부품을 지그에 임시로 내려놓거나, 한 손 안에서 회전시키거나, 두 손 사이에서 전달할 수 있다. 양팔 핸드오버(Bimanual Handover)에서는 전달하는 손이 열리기 전에 받는 손이 안정적인 파지를 형성해야 한다. 힘, 촉각, 비전 피드백을 이용하여 전달이 안전하게 완료되었는지 검증할 수 있다.

정밀 삽입(Precise Insertion)은 휴머노이드 조립에서 일반적으로 나타나는 작업이다. 페그(Peg), 커넥터, 샤프트(Shaft), 커버(Cover), 모듈형 부품은 작은 간극을 가진 결합 구조에 삽입되어야 하는 경우가 많다. 비전은 초기 상대 자세(Relative Pose)를 제공하지만 캘리브레이션 오차와 제조 편차가 여전히 존재한다. 로봇은 접촉이 발생하면 위치 제어 기반 접근 동작에서 컴플라이언트 삽입(Compliant Insertion)으로 전환하여 물리적 상호작용을 통해 작은 정렬 오차를 수정할 수 있다.

힘-토크 센싱(Force-Torque Sensing)은 이러한 접촉 중심 작업(Contact-Rich Operation)에서 중요한 정보를 제공한다. 예상하지 못한 횡방향 힘(Lateral Force)은 정렬 오류를 의미할 수 있으며, 삽입 깊이가 증가하지 않은 상태에서 축방향 힘(Axial Force)이 계속 증가하면 걸림(Jamming)이 발생했음을 나타낼 수 있다. 제어기는 작업을 정지하고 약간 후퇴한 다음 방향을 조정하고 작은 탐색 동작(Search Motion)을 수행한 후 다시 시도할 수 있다. 이는 부품을 명목 궤적을 따라 강제로 밀어 넣어 조립체를 손상시키는 방식보다 안전하다.

임피던스 제어(Impedance Control)를 이용하면 휴머노이드 로봇 암이 프로그래밍 가능한 기계적 컴플라이언스(Mechanical Compliance)를 갖도록 할 수 있다. 자유 공간에서 부품을 운반할 때는 높은 강성(High Stiffness)을 사용할 수 있고, 접촉 표면에 접근할 때는 낮은 강성을 적용할 수 있다. 카테시안 방향(Cartesian Direction)에 따라 서로 다른 강성 값을 적용하면 구속된 축에서는 정확성을 유지하면서 정렬 불확실성이 존재하는 방향으로는 유연하게 움직일 수 있다. 이는 삽입 및 결합 작업에서 특히 유용하다.

촉각 센싱(Tactile Sensing)은 비전만으로 얻기 어려운 국부 접촉 정보를 추가한다. 손가락 끝 센서(Fingertip Sensor)는 접촉 위치, 압력 분포(Pressure Distribution), 초기 미끄러짐(Incipient Slip), 파지 안정성 변화를 검출할 수 있다. 커넥터 결합 또는 스냅핏 조립(Snap-Fit Assembly)에서는 인터페이스가 카메라에서 가려져 있더라도 촉각 이벤트를 통해 결합 상태를 확인할 수 있다. 촉각 신호와 힘-토크 신호를 결합하면 물리적 상호작용을 통해 조립 상태를 추론하는 능력을 향상시킬 수 있다.

공구 사용(Tool Use)은 양팔 작업의 복잡성을 더욱 증가시킨다. 한 손으로 부품을 잡은 상태에서 다른 손으로 스크루드라이버(Screwdriver), 렌치(Wrench), 검사 프로브(Inspection Probe), 접착제 디스펜서(Adhesive Dispenser) 등의 공구를 사용할 수 있다. 로봇은 공구 자세를 추정하고 안정적인 파지를 유지하며 기능적인 공구 축(Tool Axis)을 정렬하고 반력을 관리해야 한다. 공구 교환 시스템(Tool-Changing System)을 이용하면 작업 능력을 확장할 수 있지만 추가적인 작업 순서 및 검증 절차가 필요하다.

충돌 회피(Collision Avoidance)는 두 로봇 암, 손, 몸통, 작업물, 지그, 공구, 주변 장비를 모두 고려해야 한다. 양팔 조작은 한쪽 로봇 암이 다른 로봇 암의 움직임을 방해할 수 있는 좁은 작업 공간을 형성한다. 두 로봇 암을 독립적으로 계획하면 각각은 충돌이 없는 궤적을 갖더라도 두 궤적이 동시에 실행될 때 서로 충돌할 수 있다. 따라서 협조 모션 계획(Coordinated Motion Planning)은 각각의 조작 동작 동안 휴머노이드 전체 구성을 평가해야 한다.

자기 충돌 회피(Self-Collision Avoidance)는 로봇 암이 몸의 중심선을 가로지를 때 특히 중요하다. 핸드오버 또는 중앙 작업 영역에서 조립을 수행할 때 팔꿈치, 전완(Forearm), 손목, 운반 중인 물체가 서로 간섭할 수 있다. 계획기는 안전 여유(Safety Margin)를 유지하면서 대체 팔꿈치 자세와 몸통 자세를 선택할 수 있다. 조작성 지표(Manipulability Measure)를 이용하여 작은 카테시안 움직임이 극단적인 관절 속도를 요구하는 자세도 회피할 수 있다.

휴머노이드가 움직이는 동안에도 시각 인지(Visual Perception)는 강건하게 유지되어야 한다. 로봇 자신의 팔이 조립 영역을 자주 가리고 머리 움직임에 따라 카메라 시점도 변화한다. 머리 카메라는 전역적인 상황 정보(Global Context)를 제공하고 손목 카메라는 손 주변의 세부적인 국부 관측(Local Observation)을 제공한다. 다중 시점 인지(Multi-View Perception)는 이러한 정보를 융합하여 하나의 카메라에서 일시적으로 물체가 보이지 않더라도 물체와 목표 위치 추정을 유지할 수 있다.

능동 인지(Active Perception)는 조립에 대한 신뢰도를 의도적으로 향상시킬 수 있다. 어려운 삽입을 수행하기 전에 휴머노이드는 머리 또는 손목 카메라를 이동하여 다른 각도에서 인터페이스를 검사할 수 있다. 숨겨진 특징을 노출하기 위해 부품을 회전시키거나, 한 손으로 장애물을 이동시키면서 다른 센서를 통해 관측할 수도 있다. 이러한 정보 획득 동작(Information-Gathering Action)은 접촉 중심 조작을 시작하기 전에 불확실성을 줄여준다.

조립 실행(Assembly Execution)은 위치 확인(Locate), 접근(Approach), 파지(Grasp), 안정화(Stabilize), 정렬(Align), 접촉(Contact), 삽입(Insert), 체결(Fasten), 검증(Verify), 해제(Release)와 같은 의미론적 상태(Semantic State)의 연속으로 구성할 수 있다. 상태 전환은 단순한 경과 시간이 아니라 측정된 이벤트에 따라 결정되어야 한다. 예를 들어 정렬 신뢰도가 충분한 경우에만 삽입을 시작하고, 안착(Seating)이 검증된 이후에 체결을 시작하며, 조립된 부품이 기계적으로 지지되고 있음이 확인된 이후에만 손을 놓는다.

양팔 작업은 작은 오류가 누적될 가능성이 많기 때문에 검증(Verification)이 특히 중요하다. 비전을 이용하여 최종 부품 자세를 확인하고 힘 프로파일(Force Profile)을 통해 삽입 또는 스냅 결합을 검증할 수 있다. 촉각 센싱을 통해 파지 해제를 확인하고 공구 피드백(Tool Feedback)을 이용하여 체결 토크(Fastening Torque)를 검증할 수 있다. 커넥터와 메카트로닉 조립체(Mechatronic Assembly)의 경우 전기적 또는 기능적 시험을 추가적인 검증 계층으로 사용할 수 있다.

복구 전략(Recovery Strategy)은 명목 실행이 실패하더라도 휴머노이드가 작업을 계속할 수 있도록 한다. 파지가 풀리면 물체를 다시 검출하고 재파지할 수 있다. 삽입에 실패하면 후퇴한 후 국부적인 비전 검사를 수행하고 재정렬하여 다시 컴플라이언트 삽입을 시도할 수 있다. 한쪽 로봇 암이 불리한 자세에 도달하면 전체 작업을 중단하는 대신 물체를 다른 손으로 전달하거나, 몸통 위치를 조정하거나, 부품을 임시로 내려놓은 후 작업을 계속할 수 있다.

학습(Learning)은 복잡한 양팔 동작을 프로그래밍하는 부담을 줄일 수 있다. 사람의 시연(Human Demonstration)은 손의 협조, 파지 전환, 공구 사용, 조립 순서에 대한 예제를 제공할 수 있다. 모방 학습(Imitation Learning)은 이러한 전략을 재현할 수 있으며 강화학습(Reinforcement Learning)은 시뮬레이션 또는 통제된 환경에서 접촉 동작을 최적화할 수 있다. 학습된 정책(Learned Policy)은 명시적인 기하학적 제약(Geometric Constraint)과 안전성을 고려한 제어기(Safety-Aware Controller)를 결합할 때 실제 산업 환경에서 더욱 효과적으로 적용할 수 있다.

디지털 트윈(Digital Twin)과 시뮬레이션(Simulation)은 고가의 휴머노이드 하드웨어에 배포하기 전에 작업 개발을 지원한다. 로봇 기구학, 충돌 형상, 지그, 부품, 공구를 가상 환경(Virtual Environment)에 표현하여 도달 가능성과 작업 순서의 실행 가능성을 검증할 수 있다. 접촉 시뮬레이션(Contact Simulation)은 삽입과 파지 거동을 근사할 수 있으며 합성 카메라 관측(Synthetic Camera Observation)은 인지 시스템 학습에 활용할 수 있다. 이후 하드웨어 인 더 루프 검증(Hardware-in-the-Loop Validation)을 통해 실제 제어기와 센서를 단계적으로 도입할 수 있다.

성능 평가는 작업 수준(Task-Level)과 로봇 수준(Robot-Level)의 지표를 모두 포함해야 한다. 주요 지표에는 조립 성공률(Assembly Success Rate), 최초 삽입 성공률(First-Attempt Insertion Rate), 파지 성공률(Grasp Success), 핸드오버 신뢰성(Handover Reliability), 자세 오차(Pose Error), 최대 접촉력(Peak Contact Force), 양팔 내부 힘(Internal Bimanual Force), 사이클 타임(Cycle Time), 재파지 빈도(Regrasp Frequency), 복구 빈도(Recovery Frequency), 충돌 여유(Collision Margin), 조작성, 에너지 소비량(Energy Consumption)이 포함된다. 이러한 지표를 통해 휴머노이드의 높은 유연성이 실제로 강건한 제조 성능으로 연결되는지를 평가할 수 있다.

휴머노이드 양팔 조립은 제품, 공구, 작업 스테이션이 빈번하게 변경되는 다품종 생산(High-Mix Production) 및 인간 중심 생산 환경(Human-Centered Production Environment)에서 특히 높은 가치를 가질 수 있다. 동일한 플랫폼이 각각의 작업 스테이션을 전용 자동화 장비에 맞게 재설계하지 않고도 핸들링, 조립, 체결, 검사, 자재 이송(Material Transfer)을 수행할 가능성이 있다. 따라서 휴머노이드의 장점은 최적화된 전용 장비를 단순히 대체하는 것보다 변화하는 작업 흐름에 적응 가능한 조작 능력을 제공하는 데 있다.

휴머노이드 양팔 조립 사례(Humanoid Bimanual Assembly Case)는 범용 산업용 조작(General-Purpose Industrial Manipulation)을 구현하기 위해 필요한 통합 구조를 보여준다. 신뢰성 있는 작업은 의미론적 인지, 작업 지향 파지, 협조 양팔 계획(Coordinated Dual-Arm Planning), 전신 제어, 컴플라이언트 접촉(Compliant Contact), 촉각 및 힘 피드백, 검증, 학습, 복구 기능의 통합을 통해 구현된다. 이러한 기능들이 하나의 폐루프 시스템(Closed-Loop System)으로 동작할 때 휴머노이드 로봇은 사전에 정의된 동작을 단순히 재생하는 수준에서 벗어나 복잡한 인간 중심 제조 환경에서 적응형 조립 행동(Adaptive Assembly Behavior)을 수행하는 방향으로 발전할 수 있다.

## 12.08. Mobile Manipulator Warehouse Picking Case

![](images/image8.png){width="7.268055555555556in" height="7.268055555555556in"}

모바일 매니퓰레이터 창고 피킹(Mobile Manipulator Warehouse Picking)은 자율주행(Autonomous Navigation)과 로봇 조작(Robotic Manipulation)을 하나의 시스템에 결합하여 저장 위치 사이를 이동하고, 요청된 재고를 식별하고, 물품을 파지한 후 목적지까지 운반하는 작업이다. 고정형 피킹 셀(Fixed Picking Cell)과 달리 로봇은 넓고 지속적으로 변화하는 환경에서 조작에 적합한 자세를 반복적으로 확보해야 한다. 따라서 성공적인 작업을 위해서는 위치 추정(Localization), 내비게이션(Navigation), 인지(Perception), 파지 계획(Grasp Planning), 전신 협조(Whole-Body Coordination), 검증(Verification), 복구(Recovery)의 긴밀한 통합이 필요하다.

일반적인 플랫폼은 자율이동 베이스(Autonomous Mobile Base), 하나 이상의 매니퓰레이터 암(Manipulator Arm), 그리퍼(Gripper), RGB-D 카메라, 라이다(LiDAR), 고유수용성 센서(Proprioceptive Sensor), 온보드 컴퓨팅(Onboard Computing)으로 구성된다. 모바일 베이스는 창고 규모의 이동성을 제공하고 로봇 암은 국부적인 조작을 수행한다. 추가 센서로 손목 카메라(Wrist Camera), 힘-토크 센서(Force-Torque Sensor), 촉각 센싱(Tactile Sensing), 바코드 리더(Barcode Reader), 안전 스캐너(Safety Scanner)를 사용할 수 있다. 이러한 구성 요소는 내비게이션과 조작이 동일한 월드 모델(World Model)에서 동작하도록 일관된 공간 표현을 공유해야 한다.

작업은 일반적으로 하나 이상의 재고 관리 단위(Stock Keeping Unit, SKU)와 배송 위치가 지정된 창고 관리 요청(Warehouse Management Request)에서 시작된다. 로봇은 이러한 주문을 일련의 내비게이션 및 조작 목표로 변환한다. 효율적인 실행을 위해서는 단순히 선반을 임의의 순서로 방문하는 것 이상의 계획이 필요하다. 물품 위치, 통로 교통량, 배터리 상태, 적재 용량(Payload Capacity), 주문 우선순위, 예상되는 조작 난이도 등이 작업 계획기(Task Planner)가 선택하는 순서에 영향을 줄 수 있다.

자율 위치 추정(Autonomous Localization)은 라이다, 카메라, 휠 오도메트리(Wheel Odometry), 관성 측정(Inertial Measurement) 또는 이러한 센서들의 조합을 이용하여 창고 내 로봇의 위치를 추정한다. 사전 지도(Prior Map)는 전역 기준을 제공할 수 있으며 필요한 경우 동시적 위치 추정 및 지도 작성(Simultaneous Localization and Mapping, SLAM)을 이용하여 환경 정보를 갱신할 수 있다. 정확한 베이스 위치 추정은 로봇, 선반, 빈(Bin), 목표 물체 사이의 관계를 기반으로 조작 계획이 시작되기 때문에 중요하다.

창고 내비게이션(Warehouse Navigation)은 랙(Rack), 팔레트(Pallet), 카트(Cart), 지게차(Forklift), 작업자, 다른 로봇 주변에서 안전하게 동작해야 한다. 전역 계획(Global Planning)은 시설 전체의 이동 경로를 결정하고 국부 계획(Local Planning)은 움직이는 장애물과 일시적인 통로 차단에 대응한다. 내비게이션 시스템은 운영 효율성을 유지하면서 적절한 안전 거리와 속도를 확보해야 한다. 혼잡한 통로에서는 대기, 우회 경로 선택 또는 플릿 수준 교통 관리(Fleet-Level Traffic Management)와의 협조가 필요할 수 있다.

올바른 선반에 도착하는 것만으로는 조작을 수행하기에 충분하지 않다. 모바일 베이스는 로봇 암이 특이점(Singularity), 충돌 또는 과도한 관절 신전 없이 목표 물체에 도달할 수 있는 자세에서 정지해야 한다. 따라서 베이스 배치(Base Placement)를 조작 계획의 일부로 다룰 수 있다. 선반 주변의 후보 자세를 도달 가능성(Reachability), 조작성(Manipulability), 카메라 가시성(Camera Visibility), 충돌 여유(Collision Margin), 예상 파지 품질(Expected Grasp Quality)에 따라 평가한 후 최종 도킹 위치를 결정할 수 있다.

도킹 정확도(Docking Accuracy)는 이후 조작의 난이도에 영향을 준다. 작은 베이스 위치 오차도 선반에 대한 전체 로봇 암 작업 공간을 이동시킬 수 있다. 내비게이션 자체에 극단적으로 높은 정밀도를 요구하는 대신 로봇은 정지 후 국부 인지(Local Perception)를 이용하여 실제 선반 형상을 추정할 수 있다. 이후 시각적 또는 기하학적 정합(Geometric Registration)을 통해 로봇과 저장 구조 사이의 상대 변환(Relative Transform)을 갱신함으로써 조작 시스템이 남아 있는 도킹 오차를 보상할 수 있다.

선반 인지(Shelf Perception)는 밀집되어 있고 시각적으로 유사할 수 있는 재고 가운데 요청된 물품을 식별해야 한다. 객체 검출(Object Detection) 또는 인스턴스 분할(Instance Segmentation)을 이용하여 후보 제품을 찾고 깊이 센싱(Depth Sensing)을 통해 3차원 형상을 획득할 수 있다. 문자 인식(Text Recognition), 바코드 검출, 포장 특징(Packaging Feature), 학습된 시각 임베딩(Learned Visual Embedding)은 유사한 제품을 구분하는 데 도움이 될 수 있다. 잘못된 물품을 식별하면 파지가 성공하더라도 주문 처리 오류가 발생하므로 인지 시스템은 불확실성도 함께 추정해야 한다.

가림(Occlusion)은 창고 빈에서 발생하는 주요 어려움이다. 요청된 물체가 주변 제품 뒤에 부분적으로 가려지거나 선반 깊숙한 곳에 위치할 수 있다. 따라서 하나의 카메라 영상만으로 충분하지 않을 수 있다. 로봇은 손목 카메라를 움직이거나, 로봇 암의 관측 시점을 변경하거나, 모바일 베이스 위치를 조금 이동시키거나, 시야를 가리는 물체를 조작하여 추가적인 관측 정보를 얻을 수 있다. 이러한 능동 인지(Active Perception)는 시점 선택 자체를 조작 전략의 일부로 만든다.

파지 계획(Grasp Planning)은 목표 물체뿐만 아니라 주변 선반 형상까지 고려해야 한다. 알려진 물체 모델, 검출된 표면, 포인트 클라우드(Point Cloud), 학습 기반 파지 네트워크(Learned Grasp Network)를 이용하여 후보 파지를 생성할 수 있다. 각각의 파지는 안정성(Stability), 충돌 여유, 그리퍼 접근성(Gripper Accessibility), 인출 동작(Extraction Motion)과의 호환성을 기준으로 평가해야 한다. 이론적으로 안정적인 파지라도 손가락이 물체에 도달하기 전에 선반 벽과 충돌한다면 실제로 사용할 수 없다.

창고 재고는 형상과 물리적 특성에서 매우 큰 다양성을 갖는다. 상자(Box), 병(Bottle), 봉지(Bag), 캔(Can), 카톤(Carton), 용기(Container), 불규칙한 포장 제품은 서로 다른 파지 전략을 필요로 할 수 있다. 평행 조 그리퍼(Parallel-Jaw Gripper)는 범용적인 기계적 파지를 제공하며 흡착컵(Suction Cup)은 적절한 표면을 가진 물체를 효율적으로 획득할 수 있다. 하이브리드 엔드 이펙터(Hybrid End Effector)는 파지 방식을 전환함으로써 빈번한 공구 교환 없이 더욱 다양한 제품에 대응할 수 있다.

인출 궤적(Extraction Trajectory)은 파지 자체보다 더 강한 제약을 받는 경우가 많다. 그리퍼를 닫은 후 로봇은 주변 재고나 선반 구조와 충돌하지 않으면서 물체를 꺼내야 한다. 유용한 전략은 초기에는 직선 방향으로 후퇴한 후 충분한 여유 공간이 확보되면 더 큰 자유 공간 운동(Free-Space Motion)으로 전환하는 것이다. 물품이 밀집된 빈에서는 충분한 공간을 확보하기 전에 물체를 들어 올리거나, 기울이거나, 회전시키거나, 미끄러뜨려야 할 수도 있다.

힘 센싱(Force Sensing)은 물체를 꺼내는 동안 예상하지 못한 접촉을 검출할 수 있다. 측정된 힘이 예상 범위를 넘어 증가한다면 물체가 끼어 있거나, 포장재와 얽혀 있거나, 선반 경계에 눌리고 있을 가능성이 있다. 명목 궤적을 계속 실행하면 제품이 손상되거나 주변 물품이 떨어질 수 있다. 컴플라이언트 제어기(Compliant Controller)는 모션 강성을 낮추거나 인출 동작을 정지시키거나 작은 보정 동작을 수행한 후 작업을 계속할 수 있다.

조작 중 모바일 베이스를 이동할 수 있는 시스템에서는 전신 계획(Whole-Body Planning)이 중요해진다. 내비게이션과 로봇 암 운동을 완전히 분리된 단계로 취급하는 대신 제어기는 베이스와 로봇 암의 운동을 협조하여 도달 범위를 확장하거나 조작성을 향상시킬 수 있다. 느린 베이스 이동을 통해 대형 물체를 다루거나 긴 선반에 접근하는 동안 로봇 암을 유리한 자세로 유지할 수 있다. 이러한 협조에는 세심한 안전 및 충돌 모니터링(Collision Monitoring)이 필요하다.

물품을 선반에서 꺼낸 후에는 실제로 올바른 물체를 획득했는지 검증해야 한다. 시각 검사(Visual Inspection), 바코드 판독, 무게 추정(Weight Estimation), RFID 또는 주문별 식별 방법을 통해 이를 확인할 수 있다. 그리퍼 센서는 운반 중 물체가 안정적으로 파지되어 있는지도 판단할 수 있다. 선반을 떠나기 전에 검증하면 잘못된 물품을 창고 전체로 운반한 후 배송 스테이션에서 오류를 발견하는 상황을 방지할 수 있다.

물품은 로봇에 탑재된 토트(Onboard Tote), 컨테이너(Container), 컨베이어 인터페이스(Conveyor Interface)에 배치하거나 그리퍼에 파지한 상태로 직접 운반할 수 있다. 배치 계획(Placement Planning)은 사용 가능한 공간, 제품의 취약성(Fragility), 방향, 적층 제약(Stacking Constraint)을 고려해야 한다. 무거운 물체를 민감한 제품 위에 올려놓아서는 안 되며 액체나 방향에 민감한 포장 제품은 직립 상태로 운반해야 할 수 있다. 로봇은 물품을 배치할 때마다 컨테이너 모델을 갱신하여 남아 있는 적재 공간을 추정할 수 있다.

다품목 주문(Multi-Item Order)은 라우팅과 패킹(Packing)이 결합된 문제를 만든다. 로봇은 어떤 물품을 함께 운반할 수 있는지와 어떻게 배치해야 하는지를 고려하면서 효율적인 선반 방문 순서를 결정해야 한다. 기하학적으로 가장 짧은 경로가 좋지 않은 적재 순서를 만들거나 반복적인 하역을 요구한다면 전체적으로 최적이지 않을 수 있다. 상위 수준 계획(Higher-Level Planning)은 이동 비용, 조작 난이도, 적재 제약, 배송 기한을 함께 고려할 수 있다.

동적인 창고 환경(Dynamic Warehouse Environment)은 지속적인 재계획(Replanning)을 요구한다. 계획된 통로가 차단되거나, 재고가 이동하거나, 다른 로봇이 도킹 위치를 점유하거나, 목표 물품이 예상된 위치에 존재하지 않을 수 있다. 시스템은 고정된 스크립트에 의존하는 대신 월드 모델을 갱신하고 대체 이동 경로, 선반 위치, 파지 방법 또는 작업 순서를 선택해야 한다. 이러한 적응성(Adaptability)은 자율 운영을 단순한 자동 운반과 구분하는 핵심 요소이다.

내비게이션과 조작의 실패는 서로 영향을 주기 때문에 실패 복구(Failure Recovery)가 특히 중요하다. 위치 추정 신뢰도가 충분하지 않다면 조작 전에 재위치 추정(Relocalization)을 수행해야 할 수 있다. 유효한 파지를 찾지 못하면 관측 시점이나 베이스 위치를 변경할 수 있다. 파지 실패 시 다시 물체를 검출하여 재시도하고, 접근할 수 없는 물체는 전체 주문을 중단시키는 대신 다른 로봇 또는 작업자에게 할당할 수 있다.

사람과 공유하는 창고에서는 인간 인식형 운영(Human-Aware Operation)이 필요하다. 모바일 매니퓰레이터는 사람이 보호 영역에 진입하면 속도를 줄이거나 정지해야 하며 작업자 주변에서 로봇 암을 예측하기 어렵게 움직여서는 안 된다. 안전 등급 스캐너(Safety-Rated Scanner), 비상 정지 시스템(Emergency-Stop System), 관절 토크 제한(Joint Torque Limit), 충돌 검출(Collision Detection), 제어된 모션 영역(Controlled Motion Zone)을 이용하여 다중 안전 계층을 구성할 수 있다. 작업 영역 주변에 충분한 여유 공간을 확보할 수 없는 경우 조작 동작 자체를 제한할 수도 있다.

여러 대의 모바일 매니퓰레이터가 시설을 공유하면 플릿 협조(Fleet Coordination)가 중요해진다. 플릿 관리자(Fleet Manager)는 주문을 할당하고, 교통 영역을 예약하고, 충전 일정을 조정하고, 자주 사용되는 선반 주변의 혼잡을 방지할 수 있다. 로봇마다 그리퍼 종류, 적재 한계, 도달 범위, 배터리 상태가 다를 수 있으므로 조작 능력도 작업 할당에 반영해야 한다. 따라서 플릿 수준 최적화(Fleet-Level Optimization)는 창고 물류와 개별 로봇의 물리적 능력을 연결한다.

배터리 관리(Battery Management)는 단순한 유지보수 문제가 아니라 생산 계획의 일부이다. 내비게이션과 조작은 서로 다른 비율로 에너지를 소비하며 충분한 에너지 여유가 없는 로봇은 안전하게 완료할 수 없는 주문을 시작해서는 안 된다. 수요가 낮은 시간이나 작업 사이에 기회 충전(Opportunistic Charging)을 수행할 수 있다. 작업 계획기는 특정 피킹 임무를 어느 로봇에 할당할지 결정할 때 예상 에너지 비용(Expected Energy Cost)을 포함할 수 있다.

디지털 창고 모델(Digital Warehouse Model)은 배포 및 최적화를 지원할 수 있다. 랙 형상, 내비게이션 지도, 선반 위치, 로봇 기구학(Robot Kinematics), 대표 재고를 실제 운영 전에 시뮬레이션 환경에 구현할 수 있다. 시뮬레이션을 이용하여 도킹 자세, 도달 가능성, 충돌 제약, 교통 패턴, 피킹 순서를 평가할 수 있다. 합성 센서 데이터(Synthetic Sensor Data)는 인지 모델 학습을 지원하고 하드웨어 인 더 루프 시험(Hardware-in-the-Loop Testing)을 통해 내비게이션과 조작 소프트웨어를 단계적으로 검증할 수 있다.

학습 기반 방법(Learning-Based Method)은 시스템의 여러 구성 요소를 향상시킬 수 있다. 신경망 기반 인지 모델(Neural Perception Model)은 재고를 인식하고 파지 후보를 추정할 수 있으며 학습된 파지 평가(Learned Grasp Scoring)는 과거 성공 이력을 기반으로 동작의 우선순위를 결정할 수 있다. 강화학습(Reinforcement Learning)은 국부적인 조작 행동을 최적화할 수 있으며 운영 데이터는 어려운 제품이나 선반 구성을 식별하는 데 활용할 수 있다. 학습 기반 방법은 기하학적 제약, 안전 규칙, 명시적인 검증 절차와 결합할 때 가장 높은 신뢰성을 확보할 수 있다.

생산 추적성(Production Traceability)을 통해 피킹 임무의 모든 단계를 기록할 수 있다. 로그에는 내비게이션 경로, 위치 추정 신뢰도, 물체 검출 결과, 파지 후보, 선택된 파지, 힘 측정값, 검증 결과, 재시도, 배송 위치, 타임스탬프(Timestamp)가 포함될 수 있다. 이러한 기록은 재고 감사(Inventory Auditing)와 실패 분석을 지원하며 부정확한 선반 데이터, 손상된 포장, 부적절한 도킹 위치, 그리퍼 성능 저하와 같은 체계적인 문제를 파악하는 데 활용할 수 있다.

성능은 조작 수준과 창고 시스템 수준에서 모두 평가해야 한다. 주요 지표에는 내비게이션 성공률(Navigation Success), 도킹 오차(Docking Error), 물품 인식 정확도(Item-Recognition Accuracy), 파지 성공률(Grasp Success Rate), 최초 시도 피킹 성공률(First-Attempt Pick Rate), 인출 성공률(Extraction Success), 검증 정확도(Verification Accuracy), 주문 완료 시간(Order Completion Time), 이동 거리(Travel Distance), 시간당 피킹 수(Picks per Hour), 복구 빈도(Recovery Frequency), 작업자 개입률(Human Intervention Rate), 에너지 소비량(Energy Consumption), 주문 처리 정확도(Fulfillment Accuracy)가 포함된다. 이동이나 복구 과정이 전체 사이클을 지배한다면 높은 파지 성공률만으로 효율적인 창고 시스템을 보장할 수 없다.

모바일 매니퓰레이터 창고 피킹 사례(Mobile Manipulator Warehouse Picking Case)는 독립적인 로봇 조작에서 체화 물류(Embodied Logistics)로의 전환을 보여준다. 로봇은 시설 규모의 내비게이션과 센티미터 수준의 물리적 상호작용을 동시에 추론하면서 재고, 장애물, 작업 상태에 대한 이해를 지속적으로 갱신해야 한다. 이동성(Mobility), 인지, 파지, 전신 계획, 검증, 플릿 협조, 복구 기능을 폐루프로 통합함으로써 지속적으로 변화하는 창고 환경에서 유연한 자동화를 구현할 수 있다.

## 12.09. General Purpose VLA Manipulation Pilot Case

![](images/image9.png){width="7.268055555555556in" height="7.268055555555556in"}

범용 비전-언어-행동 조작 파일럿(General-Purpose Vision-Language-Action Manipulation Pilot)은 로봇이 자연어 명령(Natural-Language Instruction)을 해석하고, 시각적 장면을 이해하며, 물체와 목표에 대해 추론하고, 여러 작업에 걸쳐 유용한 물리적 행동을 생성할 수 있는지를 평가한다. 고정된 작업 순서를 중심으로 구축되는 기존 자동화와 달리 비전-언어-행동(Vision-Language-Action, VLA) 시스템은 의미론적 이해(Semantic Understanding)를 조작과 직접 연결하려고 한다. 따라서 파일럿은 적응성(Adaptability)뿐만 아니라 물리적 배포에 필요한 안전성, 정밀도, 신뢰성, 복구 가능성(Recoverability)을 함께 평가한다.

일반적인 파일럿 플랫폼(Pilot Platform)은 하나 또는 두 개의 로봇 암(Robot Arm), 적절한 그리퍼(Gripper) 또는 덱스터러스 핸드(Dexterous Hand), RGB 또는 RGB-D 카메라, 힘-토크 센싱(Force-Torque Sensing), 고유수용성 감각(Proprioception), 엣지 컴퓨팅 시스템(Edge Computing System)으로 구성된다. VLA 모델은 시각 관측(Visual Observation)과 언어 명령을 입력받고, 하위 수준 제어기(Lower-Level Controller)는 궤적과 접촉 동작을 실행한다. 이러한 계층형 아키텍처(Hierarchical Architecture)를 통해 의미론적 추론은 결정론적 모션, 안전 기능, 하드웨어 제어 기능의 상위 계층에서 동작할 수 있다.

언어(Language)는 유연한 작업 인터페이스(Task Interface)를 제공한다. 모든 행동을 명시적으로 프로그래밍하는 대신 작업자는 부품을 트레이(Tray)에 배치하거나, 한 물체를 다른 물체 옆으로 이동하거나, 컨테이너(Container)를 열거나, 조립을 위해 부품을 준비하도록 요청할 수 있다. 시스템은 언어적 의도(Linguistic Intent)를 물리적 제약(Physical Constraint)으로 변환하고, 관련 개체(Entity)를 식별하며, 공간 관계(Spatial Relationship)를 추론하고, 실행 가능한 일련의 조작 단계를 결정해야 한다.

명령 그라운딩(Instruction Grounding)은 단어를 관측된 환경과 연결한다. 사용자가 지그 옆에 있는 파란색 컨테이너를 요청하면 로봇은 관측 가능한 영역 가운데 어떤 부분이 설명된 물체에 해당하는지, 그리고 어떤 구조물이 참조된 지그인지를 판단해야 한다. 여러 물체가 유사한 속성을 가지거나 설명이 불완전한 경우 그라운딩은 어려워진다. 따라서 모호한 언어가 안전하지 않은 행동으로 이어지는 것을 방지하기 위해 신뢰도 추정(Confidence Estimation)과 명확화 메커니즘(Clarification Mechanism)이 중요하다.

시각 인지(Visual Perception)는 물체의 정체성, 형상, 자세(Pose), 자유 공간(Free Space), 장애물, 작업 상태에 관한 정보를 제공한다. 파운데이션 시각 표현(Foundation Visual Representation)은 폭넓은 의미론적 인식을 제공할 수 있으며, 특화된 검출기(Detector), 분할 모델(Segmentation Model), 깊이 처리(Depth Processing), 자세 추정기(Pose Estimator)는 기하학적 정밀도를 제공한다. 실용적인 VLA 아키텍처에서는 하나의 모델이 모든 인지 문제를 해결할 필요가 없으며, 범용 의미론적 이해와 작업 특화 인지 모듈(Task-Specific Perception Module)을 결합할 수 있다.

장면 표현(Scene Representation)은 의미론적 정보와 기하학적 정보를 모두 유지해야 한다. 월드 모델(World Model)은 물체, 속성, 자세, 지지 표면(Support Surface), 컨테이너, 공구, 사람, 장애물과 함께 내부(Inside), 위(On), 근처(Near), 연결됨(Connected)과 같은 관계를 포함할 수 있다. 이러한 구조화된 표현(Structured Representation)은 언어 추론과 모션 계획(Motion Planning) 사이의 인터페이스를 제공한다. 또한 각각의 조작 행동 이후 장면에서 영향을 받은 부분만 갱신할 수 있도록 한다.

작업 분해(Task Decomposition)는 상위 수준 요청을 중간 목표(Intermediate Goal)로 변환한다. 작업 스테이션을 준비하라는 명령은 여러 부품을 찾고, 특정 영역을 정리하고, 공구를 이동시키고, 부품을 배치한 후 최종 상태를 검증하는 작업을 요구할 수 있다. VLA 시스템은 의미론적 계획(Semantic Plan)을 생성할 수 있으며 작업 실행기(Task Executive)는 제안된 각 단계가 사용 가능한 로봇 스킬(Robot Skill)로 지원되는지 확인한다. 지원되지 않는 행동은 하드웨어로 직접 전달하는 대신 거부하거나 대체해야 한다.

스킬 라이브러리(Skill Library)는 VLA 추론 계층 아래에서 재사용 가능한 물리적 능력을 제공한다. 스킬에는 검출(Detect), 접근(Approach), 파지(Grasp), 들어 올리기(Lift), 배치(Place), 삽입(Insert), 밀기(Push), 당기기(Pull), 열기(Open), 닫기(Close), 전달(Hand Over), 검사(Inspect), 복구(Recover) 등이 포함될 수 있다. 각 스킬은 사전 조건(Precondition), 실행 파라미터, 모니터링 신호, 성공 기준(Success Criteria)을 정의한다. 따라서 VLA 모델은 모든 작업에 대해 제한 없는 저수준 액추에이터 명령을 직접 생성하는 대신 검증된 기능을 선택하고 파라미터화한다.

파지 선택(Grasp Selection)을 위해서는 의미론적 의도를 기하학적 정보로 변환해야 한다. 명령은 어떤 물체를 왜 이동해야 하는지는 지정할 수 있지만 로봇은 여전히 충돌이 없고 기계적으로 안정적인 파지를 찾아야 한다. 기하학적 파지 계획(Geometric Grasp Planning), 학습 기반 파지 예측(Learned Grasp Prediction), 물체별 템플릿(Object-Specific Template)을 이용하여 후보를 생성할 수 있다. VLA 계층은 커넥터가 노출된 상태를 유지하거나 이후 조립에 필요한 표면을 확보하는 것과 같은 후속 작업의 의도에 따라 파지 선택에 영향을 줄 수 있다.

모션 계획은 선택된 행동을 안전한 로봇 궤적(Robot Trajectory)으로 변환한다. 충돌 검사는 로봇뿐만 아니라 운반 중인 물체, 작업 셀(Workcell), 지그, 주변 사람까지 포함해야 한다. 관절 한계(Joint Limit), 특이점(Singularity), 속도 제한, 공구 제약도 준수해야 한다. VLA 모델이 의미론적으로 올바른 행동을 제안하더라도 모션 시스템이 물리적으로 실행 가능하고 안전한 궤적이 존재한다고 확인한 이후에만 실제 실행을 진행해야 한다.

접촉 중심 작업(Contact-Rich Task)은 개방 루프 VLA 명령(Open-Loop VLA Command)이 아니라 하위 수준 피드백 제어기(Feedback Controller)를 필요로 한다. 삽입, 밀기, 열기, 공구 상호작용은 하나의 영상만으로는 알 수 없는 힘, 토크, 촉각, 변위 신호에 의존할 수 있다. 임피던스 제어(Impedance Control) 또는 힘 제어(Force Control)는 이러한 상호작용을 관리하고 의미론적 계층은 작업 진행 상태를 모니터링할 수 있다. 이러한 분리는 상위 수준의 불확실성이 민감한 물리적 접촉을 직접 제어하는 것을 방지한다.

행동 실행(Action Execution)은 장면을 변화시키므로 의미 있는 상호작용이 발생한 이후에는 인지를 다시 수행해야 한다. 로봇은 궤적 실행이 완료되었다는 이유만으로 물체가 예상된 자세에 도달했다고 가정해서는 안 된다. 파지가 미끄러지거나, 배치된 물체가 굴러가거나, 서랍이 완전히 닫히지 않을 수 있다. 재관측(Re-Observation)을 통해 월드 모델을 갱신하면 VLA 시스템은 다음 행동을 선택하기 전에 예측 결과와 실제 물리적 상태를 비교할 수 있다.

따라서 검증(Verification)은 파일럿의 핵심 구성 요소이다. 성공 기준에는 물체 존재 여부, 최종 자세, 공간 관계, 삽입 깊이(Insertion Depth), 힘 시그니처(Force Signature), 컨테이너 상태, 기능적 반응(Functional Response) 등이 포함될 수 있다. 작업마다 서로 다른 검증 방법이 필요하다. 배치 작업에서는 비전만으로 충분할 수 있지만 커넥터 삽입은 힘 또는 전기적 확인(Electrical Confirmation)이 필요할 수 있다. 명시적인 검증을 통해 작업 완료를 단순한 가정이 아니라 측정 가능한 상태 전환(State Transition)으로 변환할 수 있다.

불확실성(Uncertainty)은 시스템 전체의 행동 결정에 영향을 주어야 한다. 인지 신뢰도, 언어 모호성(Language Ambiguity), 파지 품질, 위치 추정 불확실성, 실행 피드백은 모두 특정 행동이 충분히 신뢰할 수 있는지를 나타낼 수 있다. 불확실성이 임계값을 초과하면 로봇은 근거가 부족한 가정에 따라 계속 진행하는 대신 추가 관측을 수행하거나, 시점을 변경하거나, 특화된 검출기를 사용하거나, 더 안전한 행동을 선택하거나, 사용자에게 명확화를 요청할 수 있다.

복구 행동(Recovery Behavior)은 강건한 VLA 시스템과 이상적인 조건에서만 성공하는 시연 시스템을 구분하는 핵심 능력이다. 파지에 실패하면 로봇은 다시 관측하고 다른 파지를 선택할 수 있다. 물체를 찾지 못하면 다른 영역을 검사할 수 있으며 배치 정확도가 부족하면 물체 자세를 수정할 수 있다. 삽입 실패 시에는 상위 수준의 작업 목표를 유지한 상태에서 후퇴, 검사, 재정렬, 재시도를 수행할 수 있다.

작업 내 메모리(In-Task Memory)는 완료된 행동과 이전 실패에 대한 정보를 유지할 수 있다. 로봇은 어떤 물체를 이미 이동했는지, 어떤 파지가 실패했는지, 어느 영역을 이미 탐색했는지를 알고 있어야 한다. 단기 에피소드 상태(Short-Term Episodic State)는 반복적인 행동을 방지하고 복구를 지원한다. 장기 운영 데이터(Long-Term Operational Data)는 반복적으로 발생하는 실패 모드를 식별할 수 있지만, 검증되지 않은 경험이 안전 중요 행동(Safety-Critical Behavior)을 자동으로 변경하지 않도록 생산 환경의 학습 과정은 통제되어야 한다.

파일럿 배포에서는 인간 상호작용(Human Interaction)이 특히 중요하다. 작업자는 내부 로봇 프로그램을 이해하지 않고도 목표, 수정 사항, 확인 명령, 정지 명령을 제공할 수 있어야 한다. 시스템이 모호한 상황이나 지원되지 않는 작업을 만났을 때 추측하는 것보다 사람에게 에스컬레이션(Escalation)하는 것이 바람직하다. 명확한 인터페이스는 로봇이 해석한 목표, 수행하려는 행동, 신뢰도, 실행 상태를 표시하여 작업자가 로봇의 의도를 이해할 수 있도록 한다.

안전(Safety)은 언어 모델의 추론과 독립적으로 유지되어야 한다. 비상 정지(Emergency Stop), 안전 스캐너(Safety Scanner), 충돌 모니터링(Collision Monitoring), 작업 공간 제한(Workspace Limit), 속도 제한, 힘 제한, 제어기 수준 인터록(Controller-Level Interlock)은 VLA 모델이 잘못된 행동을 제안하더라도 계속 작동해야 한다. 안전 감독기(Safety Supervisor)는 사전에 정의된 제약을 위반하는 행동을 거부할 수 있다. 이러한 아키텍처는 VLA 모델을 물리 하드웨어에 대한 최종 권한이 아니라 지능형 작업 생성기(Intelligent Task Generator)로 취급한다.

프롬프트 인젝션(Prompt Injection)과 의도하지 않은 시각적 명령(Unintended Visual Instruction)은 체화형 VLA 시스템(Embodied VLA System)에 추가적인 위험을 발생시킨다. 포장재, 화면, 라벨에 표시된 텍스트가 인증된 작업자의 명령을 자동으로 덮어써서는 안 된다. 시스템은 신뢰할 수 있는 작업 명령(Trusted Task Instruction)과 단순한 인지 데이터로 사용되는 환경 텍스트(Environmental Text)를 구분해야 한다. 명령 인증(Command Authorization), 입력 출처 분리(Source Separation), 정책 필터링(Policy Filtering)은 언어 모델이 로봇에 연결될 때 물리적 안전 메커니즘이 된다.

장기적인 목표가 범용 조작(General-Purpose Manipulation)이더라도 파일럿 범위(Pilot Scope)는 의도적으로 제한해야 한다. 유용한 평가 환경에서는 제한된 작업 공간, 검증된 물체 집합, 제한된 언어 표현 범위, 승인된 스킬 라이브러리, 명시적인 복구 절차를 정의할 수 있다. 이후 다양성을 단계적으로 확대할 수 있다. 이러한 단계적 접근(Staged Approach)을 통해 엔지니어는 자율성 범위를 확장하기 전에 실패 원인이 인지, 추론, 조작, 제어 또는 시스템 통합 중 어디에 있는지를 파악할 수 있다.

그러나 일반화(Generalization)를 평가하기 위해서는 작업 다양성(Task Diversity)이 필수적이다. 파일럿은 물체 종류, 위치, 방향, 명령 표현, 클러터(Clutter), 조명, 작업 순서에 변화를 포함해야 한다. 일부 조합은 학습 또는 시연 데이터에서 의도적으로 제외하여 시스템이 학습한 개념을 새로운 상황으로 전이할 수 있는지 시험해야 한다. 일반화는 로봇이 사전에 개별적으로 스크립트되지 않은 변형 상황에서도 성공할 때 의미를 가진다.

벤치마크 작업(Benchmark Task)은 픽앤플레이스(Pick-and-Place), 분류(Sorting), 재배치(Rearrangement), 컨테이너 상호작용, 단순 조립(Simple Assembly), 공구 핸들링(Tool Handling), 검사(Inspection) 등을 포함할 수 있다. 목표는 모든 작업의 난이도를 최대화하는 것이 아니라 공통된 인지-언어-행동 아키텍처가 여러 작업에서 능력을 재사용할 수 있는지를 평가하는 것이다. 성공적인 파일럿은 관련된 새로운 작업을 추가할 때 완전히 독립적인 자동화 프로그램을 구축하는 것보다 적은 엔지니어링 작업이 필요함을 보여주어야 한다.

시뮬레이션(Simulation)과 합성 데이터(Synthetic Data)는 실제 물리적 시험 이전에 평가 공간을 확장할 수 있다. 가상 작업 셀(Virtual Workcell)은 물체 배치, 텍스처, 카메라 자세, 조명, 방해 물체(Distractor)를 변화시키면서 자동으로 라벨을 생성할 수 있다. 로봇 시뮬레이션은 하드웨어 손상 위험 없이 상위 수준 계획과 충돌 제약을 시험할 수 있다. 그러나 파지 마찰, 접촉 동역학(Contact Dynamics), 센서 노이즈, 캘리브레이션 오차, 물체 편차를 완벽하게 재현하기 어렵기 때문에 실제 물리적 검증(Physical Validation)은 여전히 필요하다.

시연 데이터(Demonstration Data)는 언어, 관측, 행동 사이의 관계에 대한 예제를 제공할 수 있다. 원격조작(Teleoperation), 키네스테틱 티칭(Kinesthetic Teaching), 사람 영상(Human Video), 스크립트 기반 로봇 실행(Scripted Robot Execution)을 통해 학습 궤적을 확보할 수 있다. 일관되지 않은 시연은 모호한 행동을 학습시킬 수 있기 때문에 데이터셋의 양만큼 품질도 중요하다. 작업 성공, 실패, 물체 상태, 작업자 개입에 대한 메타데이터(Metadata)를 기록하면 이후 분석을 개선하고 보다 목표 지향적인 모델 개선을 수행할 수 있다.

대규모 멀티모달 모델(Large Multimodal Model)을 물리적 제어 루프에 적용할 때 지연 시간(Latency)은 실질적인 문제가 된다. 의미론적 추론은 서보 제어(Servo Control)보다 낮은 주기로 실행될 수 있지만 안전 및 힘 제어는 훨씬 빠른 응답을 요구한다. 따라서 아키텍처는 느린 숙고형 추론(Slow Deliberation)과 실시간 제어(Real-Time Control)를 분리해야 한다. 엣지 추론(Edge Inference), 모델 압축(Model Compression), 캐싱(Caching), 계층형 실행(Hierarchical Execution)을 이용하면 VLA 모델을 액추에이터 주기로 실행하지 않고도 지연을 줄일 수 있다.

성능 평가는 의미론적 지능(Semantic Intelligence)과 물리적 실행(Physical Execution)을 분리하여 평가해야 한다. 유용한 지표에는 명령 이해 정확도(Instruction-Understanding Accuracy), 그라운딩 정확도(Grounding Accuracy), 작업 계획 성공률(Task-Planning Success), 파지 성공률(Grasp Success), 모션 실행 가능성(Motion Feasibility), 최초 시도 완료율(First-Attempt Completion), 복구 성공률(Recovery Success), 작업자 개입률(Human Intervention Rate), 사이클 타임(Cycle Time), 검증 정확도(Verification Accuracy), 안전 규칙 위반(Safety-Rule Violation)이 포함된다. 최종 작업 성공률만 보고하면 실패 원인이 추론, 인지, 계획 또는 하드웨어 실행 중 어디에 있는지 파악하기 어렵다.

일반화 성능은 새로운 물체, 새로운 공간 배치, 의미가 동일한 다양한 표현의 명령(Paraphrased Instruction), 기존 스킬의 새로운 조합을 통해 측정할 수 있다. 강건성 시험(Robustness Testing)에서는 부분 가림(Partial Occlusion), 이동된 물체, 파지 실패, 예상하지 못한 장애물, 부정확한 초기 자세와 같은 제어된 교란(Controlled Disturbance)을 추가할 수 있다. 목표는 시스템이 단순히 기억된 작업 순서를 재생하는 것이 아니라 새로운 관측 정보에 따라 계획을 갱신할 수 있는지를 확인하는 것이다.

생산 추적성(Production Traceability)을 위해 명령, 시각적 상태, 해석된 목표, 선택된 스킬, 주요 모델 출력, 궤적 실행 결과, 검증 신호, 실패, 복구, 작업자 개입을 기록해야 한다. 이러한 로그는 예상하지 못한 행동의 원인을 분석할 수 있도록 하고 체계적인 개선을 위한 근거를 제공한다. 또한 모델, 프롬프트(Prompt), 스킬 라이브러리, 캘리브레이션(Calibration), 안전 정책(Safety Policy)을 버전 관리하여 특정 물리적 행동을 실제 배포 당시의 정확한 시스템 구성과 연결할 수 있어야 한다.

범용 VLA 조작 파일럿(General-Purpose VLA Manipulation Pilot)은 작업별 로봇 프로그래밍(Task-Specific Robot Programming)에서 의미론적이고 적응 가능한 물리 지능(Semantic and Adaptive Physical Intelligence)으로 발전하는 경로를 보여준다. 그 핵심 가치는 모션 제어를 언어 모델로 대체하는 데 있는 것이 아니라 폭넓은 시각-언어 추론을 검증된 로봇 스킬, 피드백 제어, 검증, 안전 기능과 연결하는 데 있다. 이해(Understand), 계획(Plan), 행동(Act), 관측(Observe), 검증(Verify), 복구(Recover)가 반복되는 폐루프(Closed Loop)는 엔지니어링 규율(Engineering Discipline)을 유지하면서 점진적으로 더욱 범용적인 조작 능력으로 발전하기 위한 기반을 제공한다.

## 12.10. Future Manipulation AI Roadmap

![](images/image10.png){width="7.268055555555556in" height="7.268055555555556in"}

미래 조작 인공지능(Future Manipulation AI)은 작업 특화 로봇 자동화(Task-Specific Robotic Automation)에서 다양한 조작 영역에 걸쳐 물체를 이해하고, 상호작용 결과를 예측하고, 스킬을 선택하며, 학습할 수 있는 적응형 물리 지능(Adaptive Physical Intelligence)으로 발전할 것이다. 이러한 로드맵은 단순히 더 큰 신경망(Neural Network)으로 발전하는 과정이 아니다. 인지(Perception), 월드 모델(World Model), 행동 표현(Action Representation), 덱스터러스 하드웨어(Dexterous Hardware), 제어(Control), 데이터(Data), 시뮬레이션(Simulation), 안전(Safety), 시스템 수준 평가(System-Level Evaluation)의 협력적인 발전이 필요하다.

이러한 발전의 첫 번째 단계는 신뢰성 있는 조작의 기반으로서 인지를 강화하는 것이다. 로봇은 단순한 물체 범주 검출을 넘어 형상(Geometry), 자세(Pose), 재료 특성(Material Property), 관절 구조(Articulation), 어포던스(Affordance), 접촉 영역(Contact Region), 불확실성(Uncertainty)을 추정할 수 있어야 한다. 멀티모달 센싱(Multimodal Sensing)은 RGB, 깊이(Depth), 촉각(Tactile), 힘(Force), 오디오(Audio), 고유수용성 감각(Proprioceptive Signal)을 결합하여 물체가 부분적으로 가려지거나 물리적으로 상호작용하는 동안에도 유용한 상태 추정(State Estimation)을 유지하도록 할 것이다.

객체 중심 장면 이해(Object-Centric Scene Understanding)는 점진적으로 구조화된 물리 월드 모델(Structured Physical World Model)로 발전할 것이다. 미래 시스템은 작업 공간을 픽셀이나 포인트 클라우드(Point Cloud)만으로 표현하는 대신 물체, 표면, 공구, 사람, 제약, 관계, 작업 상태를 지속적인 표현(Persistent Representation)으로 유지할 것이다. 이러한 모델은 의미 있는 상호작용 이후마다 갱신되고 불확실성을 유지하여 조작 계획이 관측된 상태와 가능한 숨겨진 상태를 모두 고려할 수 있도록 한다.

조작은 환경을 지속적으로 변화시키므로 시간적 예측(Temporal Prediction)의 중요성도 증가할 것이다. 월드 모델은 행동을 실행하기 전에 물체가 어떻게 이동하고, 변형되고, 미끄러지고, 충돌하며, 접촉에 반응할 수 있는지를 예측해야 한다. 예측이 모든 물리적 세부 사항을 완벽하게 재현할 필요는 없다. 더 안전한 행동을 선택하고, 좋지 않은 계획을 제거하며, 불확실성을 줄이는 데 필요한 관측을 결정할 수 있을 정도로 충분히 정확한 대안들을 제공해야 한다.

조작 표현(Manipulation Representation)은 강체의 6자유도 자세(Six-Degree-of-Freedom Pose)를 넘어 확장될 것이다. 로봇은 관절형 메커니즘(Articulated Mechanism), 케이블(Cable), 직물(Fabric), 가방(Bag), 공구(Tool), 입상 물질(Granular Material), 기타 변형 가능 또는 동적 물체(Deformable or Dynamic Object)를 표현할 수 있어야 한다. 키포인트(Keypoint), 메시(Mesh), 그래프(Graph), 접촉 상태(Contact State), 잠재 표현(Latent Representation), 하이브리드 기하-의미 모델(Hybrid Geometric-Semantic Model)이 함께 사용될 수 있다. 표현 방식은 예측과 제어에 필요한 정보에 따라 선택되어야 한다.

파지(Grasping)는 독립적인 파지 검출(Isolated Grasp Detection)에서 작업 지향 접촉 계획(Task-Oriented Contact Planning)으로 발전할 것이다. 로봇은 단순히 물체를 집을 수 있는지를 판단하는 것이 아니라 선택한 파지가 이후의 행동을 지원하는지를 판단해야 한다. 운반에 가장 좋은 파지는 삽입, 공구 사용, 핸드오버(Handover), 조립에 가장 좋은 파지와 다를 수 있다. 미래 계획기는 접촉 위치, 조작 순서, 접근 가능성(Accessibility), 후속 작업의 실행 가능성을 공동으로 최적화할 것이다.

덱스터러스 핸드(Dexterous Hand)는 가능한 조작 행동의 범위를 확대하지만 단순히 자유도(Degree of Freedom)를 추가하는 것만으로 손재주(Dexterity)가 만들어지는 것은 아니다. 신뢰성 있는 손안 조작(In-Hand Manipulation)을 위해서는 촉각 센싱(Tactile Sensing), 접촉 추정(Contact Estimation), 미끄러짐 검출(Slip Detection), 컴플라이언트 구동(Compliant Actuation), 정확한 캘리브레이션(Calibration), 다중 접촉점(Multiple Contact Point)을 협조할 수 있는 제어기가 필요하다. 실용적인 시스템은 기계적 복잡성과 강건성, 비용, 유지보수, 필요한 학습 데이터 양 사이의 균형을 고려해야 한다.

양팔 조작(Bimanual Manipulation)은 범용 로봇(General-Purpose Robot)의 핵심 능력으로 발전할 것이다. 두 개의 로봇 암은 안정화(Stabilization), 핸드오버, 협조 운반(Cooperative Transport), 케이블 라우팅(Cable Routing), 직물 조작(Cloth Manipulation), 공구 사용, 조립 등 단일 로봇 암으로 수행하기 어려운 전략을 가능하게 한다. 미래 시스템은 두 로봇 암을 독립적인 매니퓰레이터로 취급하는 대신 상호 보완적인 손 역할, 공유 물체 제약(Shared-Object Constraint), 내부 힘(Internal Force), 재파지 순서(Regrasp Sequence), 협조 궤적(Coordinated Trajectory)을 명시적으로 추론할 것이다.

전신 조작(Whole-Body Manipulation)은 손과 로봇 암을 몸통(Torso), 모바일 베이스(Mobile Base), 다리(Leg), 환경 접촉(Environmental Contact)과 연결할 것이다. 모바일 매니퓰레이터(Mobile Manipulator)와 휴머노이드(Humanoid)는 도달 범위, 가시성, 힘 발휘 능력, 조작성(Manipulability)을 개선하기 위해 몸 전체의 위치를 변경할 것이다. 계획은 내비게이션과 로봇 암의 움직임을 별도로 해결하는 방식에서 벗어나 전체 로봇 구성(Robot Configuration)을 최적화하는 방향으로 발전할 것이다. 다리형 조작에서는 균형(Balance)과 접촉 안정성(Contact Stability)이 핵심 제약이 될 것이다.

상위 수준 인공지능(High-Level AI)의 능력이 향상되더라도 접촉 중심 제어(Contact-Rich Control)는 계속해서 필수적이다. 물리적 상호작용은 의미론적 모델이 직접 안전하게 처리하기 어려운 높은 주파수와 정밀도 수준에서 발생한다. 따라서 힘 제어(Force Control), 임피던스 제어(Impedance Control), 촉각 피드백(Tactile Feedback), 반사 동작(Reflex), 실시간 안전 기능(Real-Time Safety Function)은 하드웨어 가까운 계층에 유지될 것이다. AI는 목표와 전략을 선택하고 검증된 제어기는 빠른 물리적 상호작용을 조절하는 구조로 발전할 것이다.

재사용 가능한 로봇 스킬(Reusable Robot Skill)은 상위 수준 추론과 하위 수준 제어 사이의 중요한 인터페이스를 제공할 것이다. 파지(Grasp), 삽입(Insert), 정렬(Align), 밀기(Push), 당기기(Pull), 열기(Open), 체결(Fasten), 전달(Hand Over), 검사(Inspect), 복구(Recover) 등의 스킬은 명확한 사전 조건(Precondition)과 성공 기준(Success Criteria)을 제공할 수 있다. 범용 AI는 이러한 스킬을 조합하고 파라미터화하며 결정론적 제어기(Deterministic Controller)는 모션과 접촉 제약을 강제함으로써 추론과 실행 사이에 확장 가능한 계층 구조를 형성할 수 있다.

비전-언어-행동 모델(Vision-Language-Action Model, VLA)은 조작의 의미론적 유연성(Semantic Flexibility)을 확대할 것이다. 로봇은 자연어 목표(Natural-Language Goal)를 해석하고, 문맥을 이용하여 익숙하지 않은 물체를 인식하고, 기존에 학습한 능력을 새로운 방식으로 재조합할 수 있게 될 것이다. 그러나 생산 환경의 VLA 시스템에는 그라운딩(Grounding), 검증(Verification), 명령 인증(Authorization), 안전 계층(Safety Layer)이 필요하다. 언어 추론은 임의의 텍스트에서 액추에이터 명령으로 직접 연결되는 무제한 경로가 아니라 물리적 실행을 안내하는 역할을 해야 한다.

행동 모델(Action Model)은 점차 더 예측적이고 멀티모달한 형태로 발전할 것이다. 관측을 하나의 즉각적인 명령으로 직접 변환하는 대신 미래 정책(Policy)은 여러 후보 행동 순서, 예측 결과, 신뢰도 추정(Confidence Estimation), 복구 대안(Recovery Alternative)을 생성할 수 있다. 계획기는 실행 전에 여러 미래 상태를 비교할 수 있다. 이러한 방식은 학습 모델의 광범위한 패턴 인식 능력과 불확실한 물리 환경에 적합한 명시적 의사결정(Explicit Decision-Making)을 결합한다.

시연 학습(Learning from Demonstration)은 앞으로도 조작 데이터의 주요 공급원이 될 것이다. 원격조작(Teleoperation), 키네스테틱 티칭(Kinesthetic Teaching), 사람 영상(Human Video), 웨어러블 센싱(Wearable Sensing), 로봇이 생성한 궤적(Robot-Generated Trajectory)은 상호 보완적인 감독 데이터(Supervision)를 제공할 수 있다. 미래 데이터셋은 성공적인 동작뿐만 아니라 접촉 상태, 실패, 수정, 복구 행동, 언어 설명, 작업 결과까지 기록하는 방향으로 발전할 것이다. 이러한 정보는 이상적인 시연만을 학습하는 것이 아니라 강건한 행동을 학습하는 데 필수적이다.

대규모 로봇 데이터셋(Large-Scale Robot Dataset)은 더욱 강력한 표준화(Standardization)가 필요하다. 로봇 형태(Embodiment), 카메라 구성, 행동 주파수, 좌표계(Coordinate Frame), 그리퍼, 작업 정의의 차이는 현재 플랫폼 간 학습(Cross-Platform Learning)을 어렵게 만든다. 관측, 행동, 캘리브레이션, 메타데이터(Metadata), 작업 의미(Task Semantics), 품질 라벨(Quality Label)에 대한 공통 스키마(Common Schema)는 데이터 재사용성을 향상시킬 수 있다. 로봇 형태 인식 표현(Embodiment-Aware Representation)을 이용하면 모델이 공통 개념을 학습하면서 서로 다른 로봇 구조에 맞게 실행을 적응시킬 수 있다.

합성 데이터(Synthetic Data)와 시뮬레이션은 조작 학습의 규모를 확장하는 핵심 요소가 될 것이다. 시뮬레이션은 물체 자세, 형상, 텍스처, 조명, 마찰, 질량, 클러터(Clutter), 작업 조건을 제어된 방식으로 변화시키면서 자동으로 정답 데이터(Ground Truth)를 생성할 수 있다. 절차적 생성(Procedural Generation)은 드물거나 적대적인 형상(Adversarial Configuration)까지 모델에 노출시킬 수 있다. 향후 핵심 문제는 단순히 데이터를 생성하는 것이 아니라 어떤 시뮬레이션 변형이 실제 물리 환경의 성능 개선으로 이어지는지를 결정하는 것으로 이동할 것이다.

따라서 시뮬레이션-현실 전이(Sim-to-Real Transfer)는 더욱 체계적으로 발전할 것이다. 도메인 무작위화(Domain Randomization), 시스템 식별(System Identification), 실제 데이터 미세 조정(Real-Data Fine-Tuning), 잔차 학습(Residual Learning), 적응 제어(Adaptive Control)를 통해 가상 환경과 물리 환경 사이의 차이를 줄일 수 있다. 하나의 시뮬레이터가 현실을 완벽하게 재현하기를 기대하는 대신 미래 학습 파이프라인은 다양한 가능한 물리 조건에 대한 불확실성을 모델링하고 시뮬레이션과 하드웨어 사이에 차이가 존재하더라도 정책이 안정적으로 동작하도록 학습할 것이다.

디지털 트윈(Digital Twin)은 단순한 시각화 도구에서 조작 시스템을 위한 지속적인 엔지니어링 환경(Continuous Engineering Environment)으로 발전할 것이다. 로봇 모델, 작업 셀, 센서, 물체, 제어기, 작업 로직(Task Logic)을 실제 배포 전에 평가할 수 있다. 실제 로봇에서 수집된 운영 데이터는 디지털 트윈을 갱신하고, 시뮬레이션 시험을 통해 제안된 소프트웨어 변경 사항을 평가할 수 있다. 이를 통해 설계, 학습, 검증, 배포, 유지보수를 연결하는 폐루프 엔지니어링(Closed-Loop Engineering)이 형성된다.

자기지도학습(Self-Supervised Learning)은 로봇이 대량의 라벨 없는 운영 데이터(Unlabeled Operational Data)에서 유용한 표현을 학습하도록 할 것이다. 반복적인 상호작용은 물체 영속성(Object Permanence), 접촉, 움직임, 인과관계(Cause and Effect), 행동 결과에 관한 자연스러운 학습 신호를 제공한다. 로봇은 모든 관측에 수작업 어노테이션을 요구하지 않고도 예측에 유용한 특징을 학습할 수 있다. 따라서 신중하게 통제된 자율 데이터 수집(Autonomous Data Collection)은 배포 이후 조작 능력을 확대하는 중요한 메커니즘이 될 수 있다.

강화학습(Reinforcement Learning)은 접촉 동역학과 연속적인 의사결정을 해석적으로 프로그래밍하기 어려운 행동에서 점점 더 유용해질 것이다. 시뮬레이션은 파지 개선(Grasp Refinement), 삽입, 이동-조작 협조(Locomotion-Manipulation Coordination), 덱스터러스 제어(Dexterous Control)를 위한 대규모 탐색을 지원할 수 있다. 생산 환경에서 강화학습은 고가의 하드웨어에서 제한 없는 시행착오를 수행하기보다 제한된 행동 공간(Constrained Action Space), 검증된 시뮬레이터, 안전 영역(Safety Envelope), 오프라인 데이터셋(Offline Dataset) 내에서 활용될 가능성이 높다.

복구 지능(Recovery Intelligence)은 예외 처리 기능이 아니라 핵심 능력으로 발전할 것이다. 미래 로봇은 예상된 작업 진행이 중단된 시점을 인식하고 가능한 실패 원인을 분류한 후 보정 행동(Corrective Behavior)을 선택해야 한다. 재관측(Re-Observation), 재파지(Regrasping), 재배치(Repositioning), 컴플라이언트 탐색(Compliant Search), 공구 변경, 대체 스킬, 작업자 에스컬레이션(Human Escalation)이 모두 복구 정책의 일부가 될 수 있다. 강건한 자율성(Robust Autonomy)은 정상 상태의 성공률을 높이는 것만큼 실패로부터 복구하는 능력에 의존한다.

불확실성 인식 의사결정(Uncertainty-Aware Decision Making)은 이러한 복구 능력을 지원할 것이다. 모델은 물체 정체성, 자세, 예측 결과, 파지 안정성, 작업 해석에 대한 신뢰도를 표현해야 한다. 이를 통해 로봇은 즉시 행동해도 되는 상황과 추가 센싱 또는 명확화가 필요한 상황을 구분할 수 있다. 보정된 불확실성(Calibrated Uncertainty)은 로봇이 점점 더 개방적이고 변화가 많은 생산 환경에 투입될수록 중요해질 것이다.

메모리(Memory)는 조작 시스템이 더 긴 시간 범위에서 경험을 활용하도록 한다. 단기 메모리(Short-Term Memory)는 작업 진행 상태, 이전 행동, 실패한 시도, 물체 상태 변화를 유지할 수 있다. 장기 운영 메모리(Long-Term Operational Memory)는 반복적으로 발생하는 문제, 성공적인 복구 패턴, 환경 특성을 식별할 수 있다. 과거 정보가 의사결정을 개선하면서도 검증된 행동에 통제되지 않은 변화를 발생시키지 않도록 메모리는 추적 가능하고 관리되는 구조로 운영되어야 한다.

플릿 학습(Fleet Learning)은 개별 로봇의 경험을 다수의 로봇 집단으로 확장할 것이다. 한 로봇에서 관측된 실패와 성공 전략은 검증 과정을 거친 후 다른 로봇의 모델 개선에 활용될 수 있다. 중앙 집중식 학습(Centralized Training)은 다양한 환경의 데이터를 통합하고 엣지 시스템(Edge System)은 실시간 추론을 수행할 수 있다. 잘못된 업데이트가 전체 플릿으로 확산되는 것을 방지하기 위해 버전 관리(Version Control), 데이터 출처 추적(Data Provenance), 배포 게이트(Deployment Gate), 롤백 메커니즘(Rollback Mechanism)이 필요하다.

파운데이션 조작 모델(Foundation Manipulation Model)은 궁극적으로 다양한 로봇과 작업에서 재사용할 수 있는 표현을 제공할 가능성이 있다. 이러한 모델은 이질적인 데이터셋(Heterogeneous Dataset)에서 학습한 시각적 이해, 언어, 행동 예측, 물리적 개념, 작업 지식을 통합할 수 있다. 그러나 그 가치는 단순한 모델 규모가 아니라 특정 로봇 형태에 그라운딩하고, 실제 센서에 맞게 캘리브레이션하고, 물리적 실행 가능성으로 제한하며, 신뢰성 있는 제어기와 통합할 수 있는지에 의해 결정될 것이다.

조작 모델의 규모가 증가하더라도 엣지 및 온프레미스 컴퓨팅(Edge and On-Premise Computing)은 계속 중요할 것이다. 실시간 인지, 국부 계획(Local Planning), 안전 모니터링, 제어는 낮은 지연 시간과 네트워크에 의존하지 않는 지속적인 동작의 이점을 얻는다. 더 큰 모델과 플릿 수준 학습은 필요에 따라 온프레미스 또는 클라우드 자원(Cloud Resource)을 사용할 수 있다. 계층형 컴퓨팅(Hierarchical Computing)은 지연 시간, 대역폭, 개인정보 보호, 신뢰성, 계산 요구사항에 따라 지능을 적절한 계층에 분산할 것이다.

안전 엔지니어링(Safety Engineering)은 자율성 증가와 함께 발전해야 한다. 미래 조작 시스템에는 독립적인 안전 모니터(Safety Monitor), 충돌 제약, 힘 및 속도 제한, 인증된 명령(Authenticated Command), 작업 공간 정책(Workspace Policy), 검증된 폴백 상태(Validated Fallback State)가 필요하다. 학습 모델은 강제 가능한 경계(Enforceable Boundary) 내부에서 동작해야 한다. 안전성 근거(Safety Evidence)는 하드웨어 인증뿐만 아니라 데이터셋 출처, 모델 버전 관리, 시나리오 시험(Scenario Testing), 불확실성에 대한 행동, 복구 성능까지 포함하는 방향으로 확장될 것이다.

평가(Evaluation)는 단일 작업 성공률에서 다차원적인 능력 평가(Multidimensional Capability Assessment)로 발전할 것이다. 벤치마크는 인지 정확도, 일반화(Generalization), 최초 시도 성공률, 복구 성능, 힘 안전성(Force Safety), 사이클 타임(Cycle Time), 작업자 개입, 에너지 사용량, 제어된 교란 상황에서의 성능을 측정해야 한다. 새로운 물체, 변경된 배치, 가림(Occlusion), 캘리브레이션 오차, 파지 실패, 예상하지 못한 장애물을 의도적으로 도입하여 시스템이 실제로 적응 가능한지를 평가해야 한다.

장기적인 로드맵은 다양한 환경에서 목표를 이해하고, 물리적 상황을 모델링하며, 스킬을 조합하고, 결과를 예측하고, 컴플라이언트 행동(Compliant Action)을 실행하고, 결과를 검증하며, 오류로부터 복구할 수 있는 조작 시스템을 지향한다. 이러한 발전은 학습과 엔지니어링 중 어느 하나만으로 이루어지는 것이 아니라 두 접근법의 통합을 통해 가능하다. 인지(Perceive), 이해(Understand), 예측(Predict), 계획(Plan), 행동(Act), 검증(Verify), 학습(Learn), 복구(Recover)가 반복되는 폐루프(Closed Loop)는 산업용 로봇을 점진적으로 적응 가능한 물리 지능(Adaptive Physical Intelligence)으로 전환시키는 핵심 아키텍처가 될 것이다.
