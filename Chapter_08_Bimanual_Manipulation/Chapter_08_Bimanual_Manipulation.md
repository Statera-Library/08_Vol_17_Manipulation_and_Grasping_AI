**Volume 17 Manipulation and Grasping AI**


# Chapter 08. Bimanual Manipulation

##  

## 08.01. Bimanual Manipulation Challenges and Architectures

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

Bimanual manipulation extends robotic manipulation from controlling a single arm to coordinating two manipulators that interact with the same object, environment, or task. The central difficulty is not simply doubling the number of joints. Two arms create coupled kinematic, dynamic, perceptual, and planning constraints in which the motion of one arm directly changes the feasible actions of the other.

Human manipulation demonstrates why two-handed coordination is powerful. One hand may stabilize an object while the other performs precise motion, or both hands may jointly transport a large object. Humans continuously change these roles according to task context. A robotic system must reproduce similar coordination while respecting joint limits, collision constraints, grasp stability, actuator capabilities, and uncertainty in object pose.

The configuration space of a bimanual robot is significantly larger than that of a single manipulator. Two seven-degree-of-freedom arms already create a fourteen-dimensional joint space before gripper states, object poses, mobile-base motion, or environmental constraints are considered. Planning directly in this space can become computationally expensive because many combinations of arm configurations must be evaluated for feasibility.

Coordination introduces additional geometric constraints. When both grippers rigidly grasp the same object, the relative transformation between the end effectors is constrained by the object geometry and grasp locations. Motion planning therefore cannot treat the arms independently. A trajectory generated for one arm may make the other arm kinematically infeasible, create self-collision, or generate excessive internal forces on the manipulated object.

A fundamental architectural choice is whether the two arms are controlled independently, hierarchically, or through a unified coordinated controller. Independent control is appropriate when the arms perform loosely coupled tasks in separate workspaces. Strongly coupled manipulation, however, usually requires a shared representation of the object, robot configuration, contact state, and task constraints so that both manipulators are planned as one coordinated system.

Centralized architectures represent both arms within a single planning and control problem. A unified planner can simultaneously consider joint limits, inter-arm collision, object constraints, grasp geometry, and task objectives. This approach provides strong coordination and is especially useful for cooperative carrying, assembly, insertion, and manipulation of large objects, although computational complexity increases rapidly with system dimensionality.

Decentralized architectures divide the manipulation problem between arm-specific controllers. Each arm may generate local motion while exchanging desired poses, forces, contact states, or task intentions with the other controller. This structure can improve modularity and computational scalability, but synchronization becomes critical because independently generated actions can conflict when the manipulators interact through a shared object.

Hierarchical architectures combine these approaches by separating task-level coordination from low-level arm control. A high-level coordinator determines manipulation phases, grasp assignments, object trajectories, and arm roles, while individual controllers execute joint-space or Cartesian-space commands. Such decomposition allows sophisticated task reasoning without requiring every control decision to be solved within one monolithic optimization problem.

Leader-follower coordination is one useful architecture for tightly coupled tasks. One manipulator defines the primary object trajectory while the second maintains a specified relative pose or force relationship. This simplifies coordination because the follower derives its motion from the leader and the shared object constraint. However, fixed leader-follower relationships may reduce flexibility when task roles need to change dynamically.

Symmetric cooperative control instead treats both manipulators as equal contributors to object motion. The desired trajectory can be defined directly in object space, after which inverse kinematics or optimization distributes motion across both arms. This formulation is valuable for carrying heavy, fragile, or geometrically constrained objects because object behavior becomes the primary control objective rather than either individual end-effector trajectory.

Bimanual manipulation also requires explicit management of closed kinematic chains. Once both grippers establish rigid contacts with an object, the two arms and object form a closed-loop mechanical system. Small pose inconsistencies between controllers can generate large internal forces. Accurate calibration, compliant control, force sensing, and constraint-aware trajectory generation are therefore essential for safe cooperative manipulation.

Force coordination becomes particularly important during contact-rich tasks. The controller must distinguish forces required to move an object from internal forces generated between the two arms. Excessive internal force can deform an object, cause grasp failure, or overload joints. Hybrid position-force control, impedance control, admittance control, and optimization-based whole-body control provide mechanisms for regulating these interactions.

Perception represents another major challenge because both manipulators continuously modify the visual and geometric scene. Arms may occlude cameras, objects may move between hands, and contact can change object pose in ways that vision alone cannot immediately observe. Effective systems combine visual perception with proprioception, force-torque sensing, tactile sensing, and contact-state estimation to maintain a consistent representation of the manipulation state.

Grasp planning must consider pairs of grasps rather than isolated grasp candidates. A individually stable grasp may be unsuitable if it prevents the second arm from reaching its required contact region. Bimanual grasp selection therefore considers reachability, collision-free approach paths, object stability, manipulation direction, force transmission, and the possibility of subsequent regrasping or handover operations.

Role allocation is another defining aspect of bimanual manipulation. Tasks can employ symmetric roles, such as jointly lifting a container, or asymmetric roles, such as one hand holding a component while the other performs insertion. Roles may also change during execution. A robot assembling a part may begin with cooperative transport, transition to stabilization, and finally assign one arm to precision insertion.

Motion synchronization must operate across multiple temporal scales. Large transport motions may tolerate moderate timing differences, whereas coordinated insertion, handover, cutting, folding, or connector manipulation may require tightly synchronized trajectories. Shared clocks, synchronized control cycles, deterministic communication, and timestamped sensor measurements become increasingly important as interaction speed and contact stiffness increase.

Collision avoidance is more difficult than in single-arm manipulation because each manipulator becomes a dynamic obstacle for the other. The planner must consider arm-arm, arm-object, object-environment, and self-collision simultaneously. Distance constraints, collision geometries, signed-distance fields, sampling-based planning, trajectory optimization, and model-predictive control can be combined to preserve safety while maintaining task efficiency.

Redundancy provides both opportunities and challenges. Dual-arm systems often possess many more degrees of freedom than required to specify an object pose. This redundancy can be exploited to avoid singularities, increase manipulability, maintain camera visibility, reduce joint motion, avoid obstacles, or improve force distribution. Optimization-based controllers can encode these secondary objectives while preserving primary manipulation constraints.

Task planning must also reason about discrete manipulation modes. A complex operation may involve approaching, grasping, lifting, handing over, regrasping, placing, and releasing an object. Each transition changes the kinematic structure and feasible motion space. Bimanual manipulation therefore naturally combines continuous trajectory optimization with discrete task and contact planning, producing a hybrid planning problem.

Learning-based approaches provide an alternative to explicitly engineering every coordination rule. Demonstration learning, imitation learning, reinforcement learning, diffusion policies, transformer-based policies, and vision-language-action models can learn correlations between observations and coordinated dual-arm actions. Such methods are particularly attractive for tasks whose contact dynamics or manipulation strategies are difficult to model analytically.

Learning bimanual behavior remains data intensive because the action space is large and successful coordination depends on precise temporal relationships. Demonstrations must capture not only individual arm trajectories but also relative motion, contact timing, force interactions, and task phases. Data collected from teleoperation, motion capture, simulation, and human demonstrations can provide complementary sources of coordinated manipulation experience.

Modern architectures increasingly combine learning with classical robotics rather than replacing geometric control entirely. A learned policy may select grasps, predict object motion, generate task-level actions, or propose trajectories, while inverse kinematics, collision checking, impedance control, and safety constraints enforce physical feasibility. This hybrid structure can provide greater robustness than purely model-based or purely learned solutions.

Bimanual mobile manipulators introduce an additional coordination layer because the robot base becomes part of the manipulation system. Base motion can extend workspace, improve arm manipulability, and reposition the robot during large-object handling. At the same time, planning must coordinate locomotion, torso motion, two manipulators, perception, and environmental constraints within a unified whole-body framework.

Robust architectures must explicitly address uncertainty. Calibration errors, object-pose uncertainty, actuator backlash, communication latency, grasp variation, and imperfect contact models can accumulate across two manipulators. Feedback control and online state estimation are therefore essential. The system should continuously compare expected and measured states and modify trajectories, forces, or manipulation modes when deviations occur.

Safety becomes particularly important because coordinated arms can generate substantial forces between themselves, the object, and the environment. Joint torque limits, workspace constraints, collision monitoring, force thresholds, emergency stopping, and compliant behavior should operate independently of high-level planning. Safety supervision must remain effective even when perception, communication, or learned policies produce incorrect commands.

A scalable bimanual architecture therefore benefits from layered organization. Perception maintains the scene and contact state, task reasoning determines manipulation phases and arm roles, motion planning generates coordinated object and end-effector trajectories, whole-body optimization resolves redundancy and constraints, and low-level controllers regulate position, velocity, torque, and interaction forces. Feedback connects every layer to execution.

Ultimately, bimanual manipulation should be understood as coordinated physical interaction rather than two independent instances of single-arm control. Successful systems integrate shared perception, coupled planning, synchronized control, contact reasoning, force r양팔 조작(Bimanual Manipulation)은 단일 로봇 팔(Single Arm)을 제어하는 수준에서 확장하여, 두 개의 매니퓰레이터(Manipulator)가 동일한 객체(Object), 환경(Environment), 또는 작업(Task)과 상호작용하도록 협조 제어하는 기술이다. 핵심적인 어려움은 단순히 관절(Joint)의 수가 두 배로 증가한다는 데 있지 않다. 두 팔은 서로 결합된 운동학적(Kinematic), 동역학적(Dynamic), 지각적(Perceptual), 계획적(Planning) 제약조건을 형성하며, 한 팔의 움직임이 다른 팔이 수행할 수 있는 동작에 직접적인 영향을 준다.

인간의 물체 조작(Human Manipulation)은 두 손을 이용한 협조가 왜 강력한지를 잘 보여준다. 한 손은 물체를 안정적으로 고정하고 다른 손은 정밀한 동작을 수행할 수 있으며, 두 손이 함께 큰 물체를 운반할 수도 있다. 인간은 작업 상황에 따라 이러한 역할을 지속적으로 변경한다. 로봇 시스템 역시 관절 한계(Joint Limits), 충돌 제약(Collision Constraints), 파지 안정성(Grasp Stability), 액추에이터 성능(Actuator Capabilities), 물체 자세(Object Pose)의 불확실성을 고려하면서 유사한 협조 능력을 구현해야 한다.

양팔 로봇(Bimanual Robot)의 구성 공간(Configuration Space)은 단일 매니퓰레이터보다 상당히 크다. 두 개의 7자유도(Seven-Degree-of-Freedom) 로봇 팔만 사용하더라도 그리퍼(Gripper) 상태, 물체 자세(Object Pose), 이동 베이스(Mobile Base) 움직임, 환경 제약을 포함하기 전에 이미 14차원의 관절 공간(Joint Space)이 형성된다. 이 공간에서 직접 계획을 수행하면 수많은 팔 구성의 실현 가능성을 평가해야 하므로 계산 비용이 빠르게 증가할 수 있다.

협조 동작(Coordination)은 추가적인 기하학적 제약(Geometric Constraints)을 발생시킨다. 두 그리퍼가 동일한 물체를 강체 형태로 파지하면 말단장치(End Effector) 사이의 상대 변환(Relative Transformation)은 물체 형상과 파지 위치에 의해 제한된다. 따라서 동작 계획(Motion Planning)은 두 팔을 독립적으로 처리할 수 없다. 한쪽 팔을 위해 생성된 궤적(Trajectory)이 다른 팔의 운동학적 실현 가능성을 훼손하거나 자체 충돌(Self-Collision)을 발생시키고, 물체에 과도한 내부 힘(Internal Force)을 가할 수 있기 때문이다.

양팔 조작에서 중요한 아키텍처(Architecture) 선택 중 하나는 두 팔을 독립적으로 제어할 것인지, 계층적으로 제어할 것인지, 또는 통합 협조 제어기(Unified Coordinated Controller)를 통해 제어할 것인지 결정하는 것이다. 독립 제어(Independent Control)는 두 팔이 분리된 작업 공간에서 느슨하게 결합된 작업을 수행할 때 적합하다. 반면 강하게 결합된 조작에서는 두 매니퓰레이터를 하나의 협조 시스템으로 계획할 수 있도록 물체, 로봇 구성, 접촉 상태(Contact State), 작업 제약(Task Constraints)에 대한 공유 표현이 필요하다.

중앙집중형 아키텍처(Centralized Architecture)는 두 팔을 하나의 계획 및 제어 문제로 표현한다. 통합 계획기(Unified Planner)는 관절 한계, 팔 사이의 충돌, 물체 제약, 파지 형상, 작업 목표를 동시에 고려할 수 있다. 이러한 접근법은 강력한 협조 능력을 제공하며 협동 운반(Cooperative Carrying), 조립(Assembly), 삽입(Insertion), 대형 물체 조작에 특히 유용하지만, 시스템의 차원이 증가함에 따라 계산 복잡도(Computational Complexity)가 빠르게 증가한다.

분산형 아키텍처(Decentralized Architecture)는 조작 문제를 각 로봇 팔의 개별 제어기로 분할한다. 각 팔은 자체적으로 국부 동작(Local Motion)을 생성하면서 상대 팔과 목표 자세(Desired Pose), 힘(Force), 접촉 상태(Contact State), 작업 의도(Task Intention) 등을 교환할 수 있다. 이러한 구조는 모듈성(Modularity)과 계산 확장성(Computational Scalability)을 향상시킬 수 있지만, 두 매니퓰레이터가 공유 물체를 통해 상호작용할 경우 독립적으로 생성된 동작이 충돌할 수 있으므로 동기화(Synchronization)가 매우 중요하다.

계층형 아키텍처(Hierarchical Architecture)는 작업 수준의 협조(Task-Level Coordination)와 저수준 로봇 팔 제어(Low-Level Arm Control)를 분리하여 중앙집중형과 분산형 접근법을 결합한다. 상위 수준 조정기(High-Level Coordinator)는 조작 단계, 파지 할당(Grasp Assignment), 물체 궤적(Object Trajectory), 각 팔의 역할을 결정하며, 개별 제어기는 관절 공간(Joint Space) 또는 직교좌표 공간(Cartesian Space)의 명령을 실행한다. 이러한 분해는 모든 제어 결정을 하나의 거대한 최적화 문제로 해결하지 않고도 복잡한 작업 추론(Task Reasoning)을 가능하게 한다.

리더-팔로워 협조(Leader-Follower Coordination)는 강하게 결합된 작업에 활용할 수 있는 대표적인 아키텍처이다. 하나의 매니퓰레이터가 주요 물체 궤적(Primary Object Trajectory)을 정의하고, 다른 매니퓰레이터는 지정된 상대 자세(Relative Pose) 또는 힘 관계(Force Relationship)를 유지한다. 팔로워(Follower)가 리더(Leader)와 공유 물체 제약으로부터 자신의 움직임을 결정하므로 협조 문제를 단순화할 수 있지만, 고정된 리더-팔로워 관계는 작업 역할을 동적으로 변경해야 하는 상황에서 유연성을 제한할 수 있다.

대칭형 협동 제어(Symmetric Cooperative Control)는 두 매니퓰레이터를 물체 움직임에 동일하게 기여하는 주체로 취급한다. 목표 궤적을 물체 공간(Object Space)에서 직접 정의한 후, 역운동학(Inverse Kinematics)이나 최적화(Optimization)를 통해 두 팔에 움직임을 분배할 수 있다. 이 방식은 개별 말단장치 궤적보다 물체의 움직임 자체를 주요 제어 목표로 설정하므로 무겁거나 깨지기 쉽고 기하학적으로 제약된 물체를 운반하는 데 유용하다.

양팔 조작에서는 폐쇄 운동학 체인(Closed Kinematic Chain)에 대한 명시적인 관리도 필요하다. 두 그리퍼가 하나의 물체와 강체 접촉(Rigid Contact)을 형성하면 두 팔과 물체는 폐쇄 루프 기계 시스템(Closed-Loop Mechanical System)을 구성한다. 제어기 사이의 작은 자세 불일치도 큰 내부 힘을 발생시킬 수 있다. 따라서 안전한 협동 조작을 위해 정확한 보정(Calibration), 순응 제어(Compliant Control), 힘 센싱(Force Sensing), 제약조건을 고려한 궤적 생성이 필수적이다.

힘 협조(Force Coordination)는 접촉 중심 작업(Contact-Rich Task)에서 특히 중요하다. 제어기는 물체를 움직이기 위해 필요한 힘과 두 팔 사이에서 발생하는 내부 힘(Internal Force)을 구분해야 한다. 과도한 내부 힘은 물체를 변형시키거나 파지 실패를 유발하고 관절에 과부하를 줄 수 있다. 하이브리드 위치-힘 제어(Hybrid Position-Force Control), 임피던스 제어(Impedance Control), 어드미턴스 제어(Admittance Control), 최적화 기반 전신 제어(Optimization-Based Whole-Body Control)는 이러한 상호작용을 조절하는 주요 방법이다.

지각(Perception) 역시 중요한 과제이다. 두 매니퓰레이터가 시각적·기하학적 장면을 지속적으로 변화시키기 때문이다. 로봇 팔이 카메라를 가릴 수 있고, 물체가 한 손에서 다른 손으로 이동할 수 있으며, 접촉에 의해 물체 자세가 변하더라도 비전(Vision)만으로 이를 즉시 관측하지 못할 수 있다. 따라서 효과적인 시스템은 시각 인식과 고유수용감각(Proprioception), 힘-토크 센싱(Force-Torque Sensing), 촉각 센싱(Tactile Sensing), 접촉 상태 추정(Contact-State Estimation)을 결합하여 일관된 조작 상태를 유지한다.

파지 계획(Grasp Planning)은 개별 파지 후보가 아니라 두 파지의 조합을 고려해야 한다. 개별적으로 안정적인 파지라 하더라도 두 번째 팔이 필요한 접촉 영역에 접근하는 것을 방해한다면 양팔 작업에는 적합하지 않을 수 있다. 따라서 양팔 파지 선택(Bimanual Grasp Selection)은 도달 가능성(Reachability), 충돌 없는 접근 경로, 물체 안정성, 조작 방향, 힘 전달, 그리고 이후의 재파지(Regrasping) 또는 핸드오버(Handover) 가능성까지 함께 고려해야 한다.

역할 할당(Role Allocation)은 양팔 조작을 특징짓는 또 하나의 핵심 요소이다. 두 팔이 용기를 함께 들어 올리는 것과 같은 대칭 역할(Symmetric Role)을 사용할 수도 있고, 한쪽 팔이 부품을 고정하고 다른 팔이 삽입을 수행하는 비대칭 역할(Asymmetric Role)을 사용할 수도 있다. 실행 과정에서 역할이 변경되기도 한다. 조립 로봇은 협동 운반으로 시작하여 안정화 단계로 전환한 뒤, 마지막에는 한쪽 팔에 정밀 삽입 작업을 할당할 수 있다.

동작 동기화(Motion Synchronization)는 여러 시간 척도(Time Scale)에 걸쳐 이루어져야 한다. 대규모 운반 동작에서는 어느 정도의 시간 차이를 허용할 수 있지만, 협조 삽입, 핸드오버, 절단, 접기, 커넥터 조작 등은 정밀하게 동기화된 궤적이 필요할 수 있다. 상호작용 속도와 접촉 강성(Contact Stiffness)이 증가할수록 공유 시계(Shared Clock), 동기화된 제어 주기, 결정론적 통신(Deterministic Communication), 타임스탬프 센서 측정의 중요성이 증가한다.

충돌 회피(Collision Avoidance)는 단일 로봇 팔보다 훨씬 어렵다. 각 매니퓰레이터가 다른 매니퓰레이터에게 동적인 장애물(Dynamic Obstacle)이 되기 때문이다. 계획기는 팔-팔(Arm-Arm), 팔-물체(Arm-Object), 물체-환경(Object-Environment), 자체 충돌을 동시에 고려해야 한다. 거리 제약(Distance Constraint), 충돌 형상(Collision Geometry), 부호 거리장(Signed-Distance Field), 샘플링 기반 계획(Sampling-Based Planning), 궤적 최적화(Trajectory Optimization), 모델 예측 제어(Model-Predictive Control)를 결합하여 작업 효율성을 유지하면서 안전성을 확보할 수 있다.

여유 자유도(Redundancy)는 기회와 과제를 동시에 제공한다. 양팔 시스템은 물체 자세를 지정하는 데 필요한 것보다 훨씬 많은 자유도(Degrees of Freedom)를 갖는 경우가 많다. 이러한 여유 자유도는 특이점(Singularity) 회피, 조작성(Manipulability) 향상, 카메라 시야 유지, 관절 움직임 감소, 장애물 회피, 힘 분배 개선 등에 활용할 수 있다. 최적화 기반 제어기는 주요 조작 제약을 유지하면서 이러한 부차적 목표(Secondary Objective)를 함께 처리할 수 있다.

작업 계획(Task Planning)은 이산적인 조작 모드(Discrete Manipulation Mode)에 대해서도 추론해야 한다. 복잡한 작업은 접근, 파지, 들어 올리기, 핸드오버, 재파지, 배치, 해제 등의 과정으로 구성될 수 있다. 각각의 전환은 운동학적 구조와 실행 가능한 동작 공간을 변화시킨다. 따라서 양팔 조작은 연속 궤적 최적화(Continuous Trajectory Optimization)와 이산 작업 및 접촉 계획(Discrete Task and Contact Planning)을 결합하는 하이브리드 계획 문제(Hybrid Planning Problem)의 성격을 갖는다.

학습 기반 접근법(Learning-Based Approach)은 모든 협조 규칙을 명시적으로 설계하는 방법의 대안이 될 수 있다. 시범 학습(Learning from Demonstration), 모방 학습(Imitation Learning), 강화학습(Reinforcement Learning), 확산 정책(Diffusion Policy), 트랜스포머 기반 정책(Transformer-Based Policy), 비전-언어-행동 모델(Vision-Language-Action Model)은 관측 정보와 협조된 양팔 행동 사이의 관계를 학습할 수 있다. 이러한 방법은 접촉 동역학이나 조작 전략을 분석적 모델만으로 표현하기 어려운 작업에서 특히 유용하다.

양팔 행동 학습에는 많은 데이터가 필요하다. 행동 공간(Action Space)이 크고 성공적인 협조가 정밀한 시간적 관계에 의존하기 때문이다. 시범 데이터(Demonstration Data)는 개별 팔의 궤적뿐만 아니라 상대 움직임, 접촉 시점(Contact Timing), 힘의 상호작용, 작업 단계까지 포함해야 한다. 원격조작(Teleoperation), 모션 캡처(Motion Capture), 시뮬레이션(Simulation), 인간 시범(Human Demonstration)을 통해 수집된 데이터는 협조 조작 경험을 상호보완적으로 제공할 수 있다.

현대적인 아키텍처는 기하학적 제어(Geometric Control)를 완전히 대체하기보다는 학습과 고전 로보틱스(Classical Robotics)를 결합하는 방향으로 발전하고 있다. 학습 정책(Learned Policy)이 파지를 선택하거나 물체 움직임을 예측하고 작업 수준 행동 또는 궤적을 제안하면, 역운동학, 충돌 검사(Collision Checking), 임피던스 제어, 안전 제약이 물리적 실현 가능성을 보장한다. 이러한 하이브리드 구조(Hybrid Structure)는 순수 모델 기반 또는 순수 학습 기반 방식보다 높은 강건성(Robustness)을 제공할 수 있다.

양팔 이동 매니퓰레이터(Bimanual Mobile Manipulator)는 로봇 베이스까지 조작 시스템의 일부가 되므로 추가적인 협조 계층이 필요하다. 베이스 움직임은 작업 공간을 확장하고 로봇 팔의 조작성을 향상시키며 대형 물체 취급 중 로봇 위치를 변경할 수 있게 한다. 동시에 이동(Locomotion), 몸체 움직임, 두 매니퓰레이터, 지각 시스템, 환경 제약을 통합된 전신 프레임워크(Whole-Body Framework)에서 함께 계획해야 한다.

강건한 아키텍처(Robust Architecture)는 불확실성(Uncertainty)을 명시적으로 처리해야 한다. 보정 오차(Calibration Error), 물체 자세 불확실성, 액추에이터 백래시(Actuator Backlash), 통신 지연(Communication Latency), 파지 편차, 불완전한 접촉 모델의 오차가 두 매니퓰레이터에 걸쳐 누적될 수 있다. 따라서 피드백 제어(Feedback Control)와 온라인 상태 추정(Online State Estimation)이 필수적이며, 시스템은 예상 상태와 측정 상태를 지속적으로 비교하여 편차가 발생하면 궤적, 힘 또는 조작 모드를 수정해야 한다.

안전성(Safety)은 협조된 두 팔이 서로, 물체, 환경 사이에서 상당한 힘을 발생시킬 수 있기 때문에 특히 중요하다. 관절 토크 한계(Joint Torque Limit), 작업 공간 제약, 충돌 모니터링(Collision Monitoring), 힘 임계값(Force Threshold), 비상 정지(Emergency Stop), 순응 동작(Compliant Behavior)은 상위 수준 계획과 독립적으로 작동할 수 있어야 한다. 지각, 통신 또는 학습 정책이 잘못된 명령을 생성하더라도 안전 감독(Safety Supervision)은 계속 유효해야 한다.

확장 가능한 양팔 아키텍처는 계층화된 구성(Layered Organization)을 통해 구현하는 것이 효과적이다. 지각 계층은 장면과 접촉 상태를 유지하고, 작업 추론 계층은 조작 단계와 각 팔의 역할을 결정하며, 동작 계획 계층은 물체와 말단장치의 협조 궤적을 생성한다. 전신 최적화(Whole-Body Optimization)는 여유 자유도와 제약을 해결하고, 저수준 제어기는 위치, 속도, 토크, 상호작용 힘을 조절한다. 피드백은 이러한 모든 계층과 실제 실행을 연결한다.

궁극적으로 양팔 조작은 두 개의 독립적인 단일 팔 제어를 결합한 것으로 보기보다 협조된 물리적 상호작용(Coordinated Physical Interaction)으로 이해해야 한다. 성공적인 시스템은 공유 지각(Shared Perception), 결합 계획(Coupled Planning), 동기화 제어(Synchronized Control), 접촉 추론(Contact Reasoning), 힘 조절(Force Regulation), 적응형 작업 할당(Adaptive Task Allocation)을 통합한다. 이러한 통합적 관점은 로봇 조립, 물류, 유지보수, 가정용 조작과 점차 범용화되는 물리 지능(Physical Intelligence)을 구현하기 위한 핵심 기반이 된다.egulation, and adaptive task allocation. This integrated viewpoint provides the foundation for robotic assembly, logistics, maintenance, household manipulation, and increasingly general-purpose physical intelligence.

##  

## 08.02. Bimanual Kinematics and Workspace Analysis [w/Code]

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

Bimanual kinematics describes the geometric relationships among two manipulators, their common robot base, end effectors, manipulated objects, and surrounding workspace. Unlike single-arm kinematics, the problem must represent not only the pose of each end effector but also their relative pose and their relationship to a shared object. These coupled transformations form the mathematical foundation for coordinated dual-arm motion.

A typical bimanual robot contains two serial kinematic chains attached to a common torso, frame, or mobile platform. Each arm has a joint vector, and forward kinematics maps these joint variables to end-effector poses in a common reference frame. Expressing both manipulators in the same coordinate system is essential because collision checking, cooperative grasping, object transport, and workspace analysis depend on consistent spatial relationships.

For the left and right arms, forward kinematics can be represented as transformations from the robot base to each end effector. The relative transformation between the two end effectors is obtained by combining these transformations through frame inversion and composition. This relative pose becomes particularly important when both grippers hold the same rigid object because their separation and orientation cannot change arbitrarily during manipulation.

Inverse kinematics determines joint configurations that realize desired end-effector poses. In a bimanual system, separate inverse-kinematics solutions may be sufficient when the arms operate independently, but cooperative manipulation requires coupled solutions. The solver must simultaneously satisfy left-arm pose, right-arm pose, joint limits, collision constraints, and relationships imposed by the object or task, often producing a constrained nonlinear optimization problem.

Redundancy is common in bimanual systems. Two seven-degree-of-freedom manipulators provide fourteen arm joints while a rigid object pose contains only six independent spatial degrees of freedom. The remaining freedom can be exploited to improve manipulability, avoid obstacles and singularities, maintain comfortable postures, preserve sensor visibility, minimize energy, or distribute motion more effectively between the two arms.

The Jacobian provides the differential relationship between joint velocities and Cartesian velocities. Each manipulator has an individual Jacobian, while coordinated manipulation can be described using an augmented or combined Jacobian. Such representations allow controllers to reason simultaneously about object motion, relative end-effector motion, and redundant joint motion, providing an important basis for velocity control and whole-body optimization.

When both arms rigidly grasp one object, the robot forms a closed kinematic chain. The object links the two otherwise separate serial chains and introduces loop-closure constraints. Joint configurations must remain consistent with the fixed grasp transformations throughout motion. Even small inconsistencies can produce geometric incompatibility or large interaction forces, making accurate modeling and constraint enforcement essential.

Object-centered kinematics provides a useful representation for cooperative manipulation. Instead of commanding both end effectors independently, a desired object pose or trajectory is defined first. Fixed transformations from the object frame to the two grasp frames then determine corresponding end-effector targets. This reduces coordination ambiguity and naturally preserves the geometric relationship required for rigid dual-arm grasping.

Relative and absolute motion can also be separated mathematically. Absolute motion describes the collective movement of the two arms or the grasped object through space, while relative motion describes changes in the relationship between the end effectors. Cooperative transport typically emphasizes absolute motion while maintaining nearly constant relative geometry, whereas regrasping, stretching, folding, or opening tasks intentionally modify relative configurations.

Workspace analysis determines where a manipulator can reach and what orientations it can achieve at those locations. For bimanual robots, the individual workspace of each arm is only the starting point. The most important region for cooperative manipulation is the shared or overlapping workspace where both end effectors can simultaneously reach task-relevant poses while satisfying joint, collision, and orientation constraints.

The geometric intersection of the two arm workspaces does not automatically represent a useful bimanual workspace. A point may be reachable by both arms individually yet impossible to use simultaneously because the arms collide, required orientations are unavailable, or joint configurations are near singularities. Practical workspace analysis therefore evaluates configuration feasibility rather than relying only on Cartesian reach envelopes.

The cooperative workspace can be defined according to task-specific constraints. Carrying a large box requires pairs of reachable grasp poses separated by the object width, while assembly may require one arm to stabilize a component and the other to approach from a particular direction. Consequently, workspace quality depends on object geometry, grasp placement, tool orientation, environmental obstacles, and the intended manipulation mode.

Reachable workspace and dexterous workspace should be distinguished. The reachable workspace contains positions attainable with at least one valid orientation, whereas the dexterous workspace contains regions where a broader range of orientations can be achieved. For bimanual tasks, a shared dexterous workspace is especially valuable because coordinated assembly and reorientation frequently require substantial rotational freedom at both end effectors.

Manipulability provides a quantitative measure of how effectively joint motion can generate Cartesian motion around a configuration. Jacobian-based manipulability indices can identify configurations with strong directional capability and configurations approaching singularity. In bimanual systems, manipulability may be evaluated separately for each arm or jointly according to the motion directions required by the shared object and task.

Singularities require careful consideration because loss of mobility in either arm can compromise the entire cooperative system. A configuration that is acceptable for one manipulator may force the other near a singular posture. Coordinated planning should therefore maintain suitable singularity margins for both arms, particularly during long trajectories where object constraints restrict the ability to independently reposition either manipulator.

Joint limits further reduce the theoretical workspace. Although an end-effector pose may have a mathematical inverse-kinematics solution, the corresponding joints may approach mechanical boundaries that limit subsequent motion. Joint-limit-aware optimization introduces secondary objectives or inequality constraints that keep configurations away from problematic boundaries and preserve future maneuverability during extended bimanual operations.

Self-collision and inter-arm collision constraints reshape the usable workspace substantially. The torso, shoulders, elbows, wrists, grippers, and manipulated object can all interfere with one another. Collision-aware workspace computation excludes configurations that violate minimum separation distances, producing a more realistic representation of where coordinated manipulation can safely occur rather than merely where each arm can geometrically reach.

Object dimensions strongly influence feasible workspace. A small object allows the grippers to operate close together, while a large object may require widely separated arm configurations. Long or irregular objects can also collide with the robot body or environment during rotation. Workspace analysis should therefore include the manipulated object as part of the kinematic system rather than treating it as an independent point target.

Orientation constraints can be as important as position constraints. During tray carrying, both grippers may need to preserve a horizontal orientation, whereas connector insertion requires a precise approach axis. Welding, polishing, and tool use impose other directional requirements. Task-oriented workspace maps can encode these orientation conditions and provide more meaningful feasibility estimates than unrestricted reach calculations.

Sampling-based workspace analysis is practical for high-dimensional bimanual systems. Large numbers of joint configurations can be generated within allowable limits, transformed through forward kinematics, and evaluated for collision, manipulability, orientation, and grasp compatibility. The resulting samples can approximate reachable and cooperative regions and reveal areas where feasible configurations are dense, sparse, or absent.

Optimization-based methods provide a complementary approach. Instead of sampling the entire configuration space, an optimizer searches for joint configurations satisfying specified task poses and constraints. Multiple initial conditions can expose alternative inverse-kinematics branches. Feasibility rates, optimization costs, singularity margins, and collision distances can then be used to characterize workspace quality for particular manipulation requirements.

Workspace analysis also guides mechanical design. Shoulder spacing, torso dimensions, arm length, joint range, wrist architecture, and mounting angles directly influence the size and quality of the shared workspace. Simulation can compare candidate geometries before hardware construction, allowing designers to maximize useful overlap while avoiding excessive arm interference and preserving sufficient reach for independent manipulation.

For mobile bimanual manipulators, workspace analysis extends beyond fixed-base reachability. Base translation and rotation can reposition both arms relative to the task, creating a much larger effective workspace. The challenge becomes selecting base poses that provide favorable reachability and manipulability for both arms while maintaining navigation clearance, stability, visibility, and environmental accessibility.

Whole-body kinematics can incorporate the mobile base, torso, waist, head, and both manipulators into one generalized configuration vector. Task constraints are then expressed through multiple Jacobians and optimization objectives. This formulation allows the system to distribute motion across the entire robot, such as moving the base slightly instead of forcing an arm toward a joint limit or singular configuration.

Calibration accuracy directly affects bimanual kinematic performance. Errors in shoulder placement, joint offsets, tool-center points, camera extrinsics, or grasp transforms can produce disagreement between predicted and actual relative poses. These errors become especially significant in closed-chain manipulation because two independently small pose errors can generate substantial mechanical stress when both arms rigidly constrain the same object.

Kinematic analysis should therefore be validated using both simulation and physical measurements. Forward-kinematics accuracy, inverse-kinematics convergence, relative-pose error, manipulability, collision margins, and shared-workspace coverage can be evaluated across representative configurations. Physical validation with calibrated targets and synchronized measurements confirms whether the mathematical model accurately represents the real dual-arm system.

Ultimately, bimanual kinematics an양팔 운동학(Bimanual Kinematics)은 두 개의 매니퓰레이터(Manipulator), 공통 로봇 베이스(Robot Base), 말단장치(End Effector), 조작 대상 물체(Manipulated Object), 그리고 주변 작업 공간(Workspace) 사이의 기하학적 관계를 설명한다. 단일 팔 운동학(Single-Arm Kinematics)과 달리 각 말단장치의 자세뿐만 아니라 두 말단장치 사이의 상대 자세(Relative Pose)와 공유 물체와의 관계까지 표현해야 한다. 이러한 결합 변환(Coupled Transformation)은 협조된 양팔 동작의 수학적 기반을 형성한다.

일반적인 양팔 로봇(Bimanual Robot)은 공통 몸통(Torso), 프레임(Frame), 또는 이동 플랫폼(Mobile Platform)에 장착된 두 개의 직렬 운동학 체인(Serial Kinematic Chain)으로 구성된다. 각 로봇 팔은 관절 벡터(Joint Vector)를 가지며, 순운동학(Forward Kinematics)은 이러한 관절 변수를 공통 기준 좌표계에서 말단장치 자세로 변환한다. 두 매니퓰레이터를 동일한 좌표계로 표현하는 것은 충돌 검사(Collision Checking), 협동 파지(Cooperative Grasping), 물체 운반(Object Transport), 작업 공간 분석에 필수적이다.

왼쪽 팔과 오른쪽 팔의 순운동학은 로봇 베이스에서 각 말단장치까지의 변환(Transformation)으로 표현할 수 있다. 두 말단장치 사이의 상대 변환(Relative Transformation)은 이러한 변환에 좌표계 역변환(Frame Inversion)과 합성(Composition)을 적용하여 계산한다. 이러한 상대 자세는 두 그리퍼(Gripper)가 동일한 강체 물체를 파지할 때 특히 중요하며, 조작 과정에서 두 그리퍼 사이의 거리와 방향은 임의로 변경될 수 없다.

역운동학(Inverse Kinematics)은 원하는 말단장치 자세를 구현하는 관절 구성을 결정한다. 양팔 시스템에서 두 팔이 독립적으로 작동한다면 개별적인 역운동학 해가 충분할 수 있지만, 협동 조작(Cooperative Manipulation)에서는 결합된 해(Coupled Solution)가 필요하다. 해석기는 왼쪽 팔 자세, 오른쪽 팔 자세, 관절 한계(Joint Limits), 충돌 제약(Collision Constraints), 물체 또는 작업에서 발생하는 관계를 동시에 만족해야 하며, 이는 흔히 제약된 비선형 최적화 문제(Constrained Nonlinear Optimization Problem)가 된다.

여유 자유도(Redundancy)는 양팔 시스템에서 일반적으로 나타난다. 두 개의 7자유도(Seven-Degree-of-Freedom) 매니퓰레이터는 총 14개의 팔 관절을 제공하지만, 강체 물체의 자세는 6개의 독립적인 공간 자유도(Spatial Degrees of Freedom)만을 갖는다. 나머지 자유도는 조작성(Manipulability)을 향상시키고, 장애물과 특이점(Singularity)을 회피하며, 편안한 자세를 유지하고, 센서 가시성(Sensor Visibility)을 보존하거나 두 팔 사이의 움직임을 효과적으로 분배하는 데 활용할 수 있다.

자코비안(Jacobian)은 관절 속도(Joint Velocity)와 직교좌표 속도(Cartesian Velocity) 사이의 미분 관계를 제공한다. 각 매니퓰레이터는 개별 자코비안을 가지며, 협조 조작은 확장 자코비안(Augmented Jacobian) 또는 결합 자코비안(Combined Jacobian)을 사용하여 표현할 수 있다. 이러한 표현을 통해 제어기는 물체 움직임, 상대 말단장치 움직임, 여유 관절 움직임을 동시에 고려할 수 있으며, 속도 제어(Velocity Control)와 전신 최적화(Whole-Body Optimization)의 중요한 기반이 된다.

두 팔이 하나의 물체를 강체 형태로 파지하면 로봇은 폐쇄 운동학 체인(Closed Kinematic Chain)을 형성한다. 물체가 서로 분리되어 있던 두 직렬 체인을 연결하면서 루프 폐쇄 제약(Loop-Closure Constraint)을 발생시킨다. 관절 구성은 전체 동작 과정에서 고정된 파지 변환(Grasp Transformation)과 일관성을 유지해야 한다. 작은 불일치도 기하학적 비호환성이나 큰 상호작용 힘을 발생시킬 수 있으므로 정확한 모델링과 제약조건 적용이 중요하다.

물체 중심 운동학(Object-Centered Kinematics)은 협동 조작을 표현하는 유용한 방법이다. 두 말단장치를 각각 독립적으로 명령하는 대신 먼저 원하는 물체 자세(Object Pose) 또는 궤적(Trajectory)을 정의한다. 이후 물체 좌표계(Object Frame)에서 두 파지 좌표계(Grasp Frame)까지의 고정 변환을 이용하여 각 말단장치의 목표 자세를 결정한다. 이러한 방법은 협조 과정의 모호성을 줄이고 강체 양팔 파지에 필요한 기하학적 관계를 자연스럽게 유지한다.

상대 운동(Relative Motion)과 절대 운동(Absolute Motion)을 수학적으로 분리할 수도 있다. 절대 운동은 두 팔 또는 파지된 물체가 공간에서 전체적으로 이동하는 것을 의미하며, 상대 운동은 두 말단장치 사이의 관계 변화를 의미한다. 협동 운반(Cooperative Transport)은 일반적으로 상대적인 기하 관계를 거의 일정하게 유지하면서 절대 운동을 강조하는 반면, 재파지(Regrasping), 늘리기, 접기, 열기 작업에서는 상대 구성을 의도적으로 변화시킨다.

작업 공간 분석(Workspace Analysis)은 매니퓰레이터가 도달할 수 있는 위치와 해당 위치에서 구현 가능한 방향(Orientation)을 결정한다. 양팔 로봇에서는 각 팔의 개별 작업 공간(Individual Workspace)이 분석의 출발점일 뿐이다. 협동 조작에서 가장 중요한 영역은 두 말단장치가 관절, 충돌, 방향 제약을 만족하면서 작업에 필요한 자세에 동시에 도달할 수 있는 공유 작업 공간(Shared Workspace) 또는 중첩 작업 공간(Overlapping Workspace)이다.

두 로봇 팔 작업 공간의 기하학적 교집합이 자동으로 유용한 양팔 작업 공간을 의미하지는 않는다. 특정 위치에 두 팔이 각각 독립적으로 도달할 수 있더라도 동시에 접근하면 팔 사이에 충돌이 발생하거나 필요한 방향을 구현하지 못하고 관절 구성이 특이점에 가까워질 수 있다. 따라서 실제적인 작업 공간 분석에서는 단순한 직교좌표 도달 영역(Cartesian Reach Envelope)이 아니라 구성의 실현 가능성(Configuration Feasibility)을 평가해야 한다.

협동 작업 공간(Cooperative Workspace)은 작업별 제약조건에 따라 정의할 수 있다. 대형 상자를 운반하려면 물체 폭만큼 떨어진 두 파지 자세에 동시에 도달할 수 있어야 하며, 조립 작업에서는 한쪽 팔이 부품을 안정화하고 다른 팔이 특정 방향에서 접근해야 할 수 있다. 따라서 작업 공간의 품질은 물체 형상(Object Geometry), 파지 위치, 도구 방향, 환경 장애물, 그리고 의도된 조작 모드(Manipulation Mode)에 따라 달라진다.

도달 가능 작업 공간(Reachable Workspace)과 능숙 작업 공간(Dexterous Workspace)은 구분되어야 한다. 도달 가능 작업 공간은 최소 하나의 유효한 방향으로 접근할 수 있는 위치를 포함하며, 능숙 작업 공간은 보다 다양한 방향을 구현할 수 있는 영역을 의미한다. 양팔 작업에서는 공유 능숙 작업 공간(Shared Dexterous Workspace)이 특히 중요하다. 협조 조립과 물체 재방향 설정(Reorientation)에는 두 말단장치 모두에서 상당한 회전 자유도가 요구되기 때문이다.

조작성(Manipulability)은 특정 구성에서 관절 움직임을 직교좌표 움직임으로 얼마나 효과적으로 변환할 수 있는지를 정량적으로 나타낸다. 자코비안 기반 조작성 지표(Jacobian-Based Manipulability Index)를 사용하면 특정 방향으로 높은 운동 능력을 갖는 구성과 특이점에 접근하는 구성을 식별할 수 있다. 양팔 시스템에서는 각 팔의 조작성을 개별적으로 평가하거나 공유 물체와 작업에서 요구되는 운동 방향을 기준으로 통합하여 평가할 수 있다.

특이점(Singularity)은 어느 한쪽 팔에서 이동 능력이 손실되더라도 전체 협동 시스템의 성능을 저하시킬 수 있으므로 신중하게 고려해야 한다. 한 매니퓰레이터에는 적절한 구성이 다른 매니퓰레이터를 특이 자세에 가깝게 만들 수 있다. 따라서 협조 계획(Coordinated Planning)은 특히 물체 제약으로 각 팔을 독립적으로 재배치하기 어려운 긴 궤적에서 두 팔 모두에 적절한 특이점 여유(Singularity Margin)를 유지해야 한다.

관절 한계(Joint Limits)는 이론적인 작업 공간을 더욱 축소시킨다. 말단장치 자세에 수학적인 역운동학 해가 존재하더라도 해당 관절이 기계적 한계에 가까우면 이후의 움직임이 제한될 수 있다. 관절 한계를 고려한 최적화(Joint-Limit-Aware Optimization)는 보조 목적함수(Secondary Objective) 또는 부등식 제약(Inequality Constraint)을 도입하여 관절 구성이 문제가 되는 경계에서 벗어나도록 하고, 장시간 양팔 작업에서 향후 기동성을 유지하도록 한다.

자체 충돌(Self-Collision)과 팔 사이 충돌(Inter-Arm Collision) 제약은 실제 사용할 수 있는 작업 공간을 크게 변화시킨다. 몸통, 어깨, 팔꿈치, 손목, 그리퍼, 조작 물체가 서로 간섭할 수 있기 때문이다. 충돌 인식 작업 공간 계산(Collision-Aware Workspace Computation)은 최소 이격 거리(Minimum Separation Distance)를 위반하는 구성을 제외함으로써 각 팔이 단순히 기하학적으로 도달할 수 있는 영역이 아니라 안전하게 협조 조작을 수행할 수 있는 영역을 표현한다.

물체 크기(Object Dimensions)는 실현 가능한 작업 공간에 큰 영향을 미친다. 작은 물체에서는 두 그리퍼가 서로 가까운 위치에서 작동할 수 있지만, 큰 물체는 두 팔이 넓게 벌어진 구성을 요구할 수 있다. 길거나 불규칙한 물체는 회전 과정에서 로봇 몸체 또는 주변 환경과 충돌할 수도 있다. 따라서 작업 공간 분석에서는 조작 물체를 독립적인 점 목표(Point Target)로 취급하지 않고 운동학 시스템의 일부로 포함해야 한다.

방향 제약(Orientation Constraints)은 위치 제약만큼 중요할 수 있다. 트레이 운반에서는 두 그리퍼가 수평 방향을 유지해야 할 수 있으며, 커넥터 삽입에서는 정밀한 접근 축(Approach Axis)이 필요하다. 용접, 연마, 도구 사용 역시 각각 다른 방향 조건을 요구한다. 작업 지향 작업 공간 지도(Task-Oriented Workspace Map)는 이러한 방향 조건을 포함함으로써 제약 없는 단순 도달 범위보다 의미 있는 실현 가능성 평가를 제공한다.

샘플링 기반 작업 공간 분석(Sampling-Based Workspace Analysis)은 고차원 양팔 시스템에서 실용적인 방법이다. 허용된 관절 범위 내에서 많은 관절 구성을 생성하고 순운동학을 통해 변환한 뒤 충돌, 조작성, 방향, 파지 호환성(Grasp Compatibility)을 평가할 수 있다. 이러한 샘플은 도달 가능 영역과 협동 영역을 근사하고, 실현 가능한 구성이 밀집하거나 희박하거나 존재하지 않는 영역을 파악하는 데 활용된다.

최적화 기반 방법(Optimization-Based Method)은 이를 보완하는 접근법을 제공한다. 전체 구성 공간을 샘플링하는 대신 최적화기가 지정된 작업 자세와 제약조건을 만족하는 관절 구성을 탐색한다. 여러 초기 조건(Initial Condition)을 사용하면 서로 다른 역운동학 해의 분기(Inverse-Kinematics Branch)를 찾을 수 있다. 이후 실현 가능 비율, 최적화 비용, 특이점 여유, 충돌 거리를 이용하여 특정 조작 요구조건에 대한 작업 공간 품질을 평가할 수 있다.

작업 공간 분석은 기계 설계(Mechanical Design)를 결정하는 데에도 활용된다. 어깨 간격(Shoulder Spacing), 몸통 크기, 팔 길이, 관절 범위, 손목 구조, 장착 각도는 공유 작업 공간의 크기와 품질에 직접적인 영향을 미친다. 하드웨어 제작 전에 시뮬레이션(Simulation)을 통해 여러 후보 구조를 비교하면 팔 사이의 과도한 간섭을 줄이면서 유용한 중첩 영역을 극대화하고 독립 조작에 필요한 충분한 도달 범위를 확보할 수 있다.

이동형 양팔 매니퓰레이터(Mobile Bimanual Manipulator)의 작업 공간 분석은 고정 베이스 도달 가능성을 넘어 확장된다. 베이스의 병진 및 회전 운동을 이용하면 작업에 대해 두 팔의 위치를 재조정하여 훨씬 큰 유효 작업 공간(Effective Workspace)을 형성할 수 있다. 이때 두 팔에 유리한 도달성과 조작성을 제공하면서 이동 경로의 여유, 안정성(Stability), 가시성, 환경 접근성을 유지하는 베이스 자세(Base Pose)를 선택하는 것이 핵심 과제가 된다.

전신 운동학(Whole-Body Kinematics)은 이동 베이스, 몸통, 허리, 머리, 두 매니퓰레이터를 하나의 일반화 구성 벡터(Generalized Configuration Vector)에 포함할 수 있다. 작업 제약은 여러 자코비안과 최적화 목적함수를 통해 표현된다. 이러한 구성은 로봇 팔을 관절 한계나 특이점까지 움직이는 대신 베이스를 조금 이동시키는 것처럼 전체 로봇에 움직임을 분배할 수 있게 한다.

보정 정확도(Calibration Accuracy)는 양팔 운동학 성능에 직접적인 영향을 준다. 어깨 장착 위치, 관절 오프셋(Joint Offset), 도구 중심점(Tool-Center Point), 카메라 외부 파라미터(Camera Extrinsics), 파지 변환의 오차는 예측된 상대 자세와 실제 상대 자세 사이에 차이를 발생시킬 수 있다. 이러한 오차는 두 팔이 동일한 물체를 강체로 구속하는 폐쇄 체인 조작에서 특히 중요하며, 개별적으로 작은 자세 오차도 상당한 기계적 응력(Mechanical Stress)을 발생시킬 수 있다.

따라서 운동학 분석은 시뮬레이션과 실제 측정을 모두 이용하여 검증해야 한다. 순운동학 정확도, 역운동학 수렴성(Inverse-Kinematics Convergence), 상대 자세 오차, 조작성, 충돌 여유(Collision Margin), 공유 작업 공간 범위를 대표적인 구성 전반에서 평가할 수 있다. 보정된 표적(Calibrated Target)과 동기화 측정(Synchronized Measurement)을 이용한 실제 검증은 수학적 모델이 실제 양팔 시스템을 얼마나 정확하게 표현하는지를 확인한다.

궁극적으로 양팔 운동학 및 작업 공간 분석(Bimanual Kinematics and Workspace Analysis)은 협조 조작을 위한 기하학적 기반을 제공한다. 이를 통해 두 팔이 어디에서 함께 작업할 수 있는지, 물체를 어떻게 공동으로 구속하고 이동시킬 수 있는지, 어떤 구성이 충분한 기민성(Dexterity)과 안전성을 제공하는지를 결정할 수 있다. 결합 운동학(Coupled Kinematics), 여유 자유도 해소(Redundancy Resolution), 충돌 인식(Collision Awareness), 조작성, 물체 제약, 전신 움직임을 통합함으로써 복잡한 협동 로봇 작업을 위한 신뢰성 높은 계획이 가능해진다.d workspace analysis provide the geometric foundation for coordinated manipulation. They determine where both arms can operate, how they can jointly constrain and move objects, and which configurations provide sufficient dexterity and safety. Integrating coupled kinematics, redundancy resolution, collision awareness, manipulability, object constraints, and whole-body motion enables reliable planning for complex cooperative robotic tasks.

##  

## 08.03. Bimanual Task Decomposition and Sequencing [w/Code]

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

Bimanual task decomposition converts a complex two-handed manipulation objective into smaller actions that can be planned, coordinated, monitored, and recovered individually. Rather than treating the complete operation as one continuous trajectory, the robot identifies meaningful phases such as perception, approach, grasp, stabilization, transport, insertion, regrasping, handover, and release. This structure reduces planning complexity and exposes dependencies between the two arms.

The decomposition process begins with a task-level description of the desired outcome. A command such as assembling two components, opening a container, folding material, or carrying a large object does not directly specify robot motion. The system must transform this semantic goal into manipulation primitives with explicit object states, contact relationships, arm roles, geometric constraints, and completion conditions that can be executed by the physical platform.

Manipulation primitives provide reusable building blocks for this transformation. Typical primitives include reach, grasp, hold, lift, move, rotate, align, insert, pull, push, handover, regrasp, place, and release. Each primitive defines preconditions that must be satisfied before execution, control objectives during execution, and postconditions describing the expected resulting state. Complex bimanual behaviors can then be composed from sequences of these primitives.

Task decomposition must represent interactions between the two manipulators rather than generating two independent action lists. In an asymmetric task, one arm may stabilize an object while the other manipulates it. In a symmetric task, both arms may simultaneously transport or rotate the same object. The planner therefore identifies synchronization points where actions must start, finish, or transition together and periods where independent parallel execution is permitted.

Role assignment determines which manipulator performs each primitive. The left and right arms may have different reachability, payload capability, tool configuration, sensor visibility, or proximity to relevant objects. Role selection can therefore be based on geometric feasibility, manipulability, expected execution cost, collision risk, task convention, or learned experience rather than simply assigning actions according to fixed left-right rules.

Roles may remain fixed during simple operations but often change during complex tasks. One arm may initially grasp an object while the second clears an obstacle, after which both arms cooperatively lift it. Later, one arm may become a stabilizer while the other performs insertion. Representing role transitions explicitly allows the planner to exploit the complementary capabilities of both manipulators throughout different phases of the operation.

Sequencing defines the temporal and logical order in which manipulation primitives are executed. Some actions have strict precedence constraints: an object must be grasped before it can be lifted, and components must be aligned before insertion. Other actions can occur concurrently, such as one arm reaching toward a fixture while the other acquires a component. Efficient sequencing therefore combines serial dependencies with safe parallel execution.

A task graph provides a useful representation of these relationships. Nodes can represent manipulation states, actions, or subgoals, while edges encode valid transitions and dependencies. Directed acyclic graphs are useful when the operation has a largely predetermined progression, whereas richer state-transition graphs are appropriate when retries, alternative strategies, and recovery paths must be represented. Graph structures also make task logic easier to inspect and validate.

Finite-state machines provide a practical execution architecture for structured bimanual tasks. Each state represents a manipulation phase, and transitions occur when specified conditions are satisfied. For example, the system may progress from approach to grasp only after pose error falls below a threshold, and from grasp to lift only after contact and gripping force are verified. Failure conditions can redirect execution toward retry or recovery states.

Behavior trees offer greater flexibility for tasks containing alternatives, parallel actions, and reusable behaviors. Sequence nodes enforce ordered execution, selector nodes choose among alternative strategies, and parallel nodes coordinate simultaneous actions. Individual manipulation skills can be implemented as reusable leaves. This organization is particularly useful when a bimanual robot must adapt its action sequence according to changing perception or execution outcomes.

Planning formalisms such as Planning Domain Definition Language, symbolic planning, and task-and-motion planning can generate sequences from explicit action models. Symbolic reasoning determines which manipulation operations should occur, while geometric motion planning verifies whether those operations are physically feasible. The combination prevents a high-level planner from producing logically correct but kinematically impossible bimanual action sequences.

Task-and-motion planning is especially important because discrete manipulation decisions and continuous robot configurations are strongly coupled. Selecting which arm should grasp an object affects reachability, collision constraints, and future actions. Similarly, choosing a grasp determines whether subsequent transport or insertion is possible. Effective planners therefore exchange information between symbolic task reasoning and geometric feasibility evaluation rather than solving them independently.

Contact modes provide another important dimension of task decomposition. A manipulation sequence may transition from no contact to single-arm contact, dual-arm rigid grasp, environmental support, sliding contact, insertion contact, and release. Each mode changes the system\'s kinematic and dynamic constraints. Explicit contact-mode planning allows the controller and motion planner to apply the correct models and constraints during each phase.

Object state is often more useful than robot joint state for describing task progress. An assembly task can be represented through semantic conditions such as component acquired, component aligned, insertion started, insertion completed, and fixture released. Robot configurations remain important for execution, but object-centered state representations allow the same task structure to be reused across different robot geometries, initial poses, and motion-planning solutions.

Preconditions and postconditions create clear interfaces between manipulation skills. A grasp primitive may require that the object pose is known, the target grasp is reachable, and the gripper is open. Its postconditions may require verified contact and sufficient grasp force. These conditions allow the execution manager to determine whether a primitive can begin and whether it completed successfully instead of relying only on elapsed time.

Synchronization is essential when two primitives interact through a shared object. During cooperative lifting, both arms should establish stable grasps before vertical motion begins. During insertion, a stabilizing arm may need to maintain a fixture pose while the active arm follows a constrained trajectory. Synchronization barriers, shared events, timestamps, and coordinated state transitions provide mechanisms for enforcing these temporal relationships.

Not every bimanual action requires strict synchronization. Parallelism can reduce task completion time when the arms interact with independent objects or spatially separated regions. One arm can retrieve a component while the other prepares a fixture, provided their trajectories and workspaces remain compatible. Scheduling algorithms can exploit such independence while introducing synchronization only when physical or logical dependencies require it.

Resource conflicts must also be considered during sequencing. Both arms may require access to the same workspace, camera view, fixture, tool, or object. Executing individually feasible actions simultaneously may therefore cause collision, occlusion, or manipulation interference. Resource-aware scheduling represents these shared resources explicitly and prevents incompatible primitives from occupying them at the same time.

Regrasping introduces additional sequence complexity because an initially convenient grasp may not support the entire task. The robot may place an object temporarily, transfer it between hands, or establish a second grasp before releasing the first. Regrasp planning searches for intermediate states that preserve object stability while changing contact configuration, enabling the robot to overcome workspace, orientation, or joint-limit restrictions.

Handover is a specialized coordinated transition in which responsibility for an object moves from one manipulator to the other. The receiving arm must reach a compatible grasp before the delivering arm releases its contact. Successful sequencing therefore includes approach, synchronization, dual-contact verification, load transfer, release, and retreat. Force or tactile sensing can verify that object support has transferred safely before the sequence continues.

Error detection must be integrated into the task sequence rather than treated as an external exception. Perception may reveal that an object moved unexpectedly, grasp verification may fail, or insertion forces may exceed limits. Each primitive should expose measurable success and failure criteria so that execution can stop before an error propagates into subsequent actions. This is especially important in tightly coupled bimanual manipulation.

Recovery behaviors provide alternative transitions when normal execution fails. A failed grasp may trigger perception update and regrasping, excessive insertion force may cause withdrawal and realignment, and loss of cooperative contact may return the object to a stable support surface. Designing recovery paths together with nominal sequences produces more robust behavior than attempting to improvise recovery only after a failure occurs.

Online replanning allows the sequence to adapt when assumptions change during execution. Updated object poses, newly detected obstacles, unavailable grasps, or changing human activity may invalidate the original plan. The robot can preserve completed subgoals while regenerating only the affected future actions. This reduces unnecessary repetition and supports operation in dynamic environments where complete offline sequencing is insufficient.

Learning can complement explicit task decomposition by discovering useful primitives, predicting action success, selecting arm roles, or ranking alternative sequences. Demonstrations provide examples of how humans divide complex two-handed activities into coordinated phases. Learned models can capture contextual preferences that are difficult to encode manually, while symbolic constraints and geometric planners can continue to enforce safety and physical feasibility.

Vision-language-action models and foundation policies can provide semantic interpretation and high-level skill proposals, but reliable bimanual execution still benefits from structured sequencing. A learned model may suggest that one arm hold a container while the other removes a lid, while the execution architecture converts this proposal into verified grasp, stabilization, rotation, release, and recovery states with explicit geometric and force constraints.

A hierarchical architecture is therefore well suited to bimanual task execution. At the highest level, task reasoning interprets goals and generates subgoals. A sequencing layer assigns roles, manages dependencies, and selects manipulation skills. Motion planners generate collision-free trajectories, while low-level controllers execute position, force, impedance, or torque commands. Perception and state estimation continuously provide feedback across these layers.

Performance can be evaluated using more than final task success. Useful measures include sequence duration, number of primitive transitions, parallel execution ratio, replanning frequency, recovery success, synchronization error, collision margin, and unnecessary arm motion. Such metrics reveal whether decomposition produces not only successful but also efficient, predictable, and robust dual-arm behavior.

Ultimately, bimanual task decomposition and sequencing provide the organizational logic that connects semantic goals with coordinated physical execution. By representing manipulation through reusable primitives, arm roles, dependencies, contact modes, synchronization events, success conditions, and recovery transitions, a robot can transform complex two-handed activities into manageable operations while retaining the flexibility required for adaptive and general-purpose manipulation.

양팔 작업 분해(Bimanual Task Decomposition)는 복잡한 양손 조작 목표를 개별적으로 계획하고, 협조하며, 모니터링하고, 복구할 수 있는 작은 행동으로 변환하는 과정이다. 전체 작업을 하나의 연속 궤적(Continuous Trajectory)으로 처리하는 대신 로봇은 인식(Perception), 접근(Approach), 파지(Grasp), 안정화(Stabilization), 운반(Transport), 삽입(Insertion), 재파지(Regrasping), 핸드오버(Handover), 해제(Release)와 같은 의미 있는 단계로 구분한다. 이러한 구조는 계획 복잡도를 줄이고 두 팔 사이의 의존 관계를 명확하게 만든다.

분해 과정은 원하는 결과를 나타내는 작업 수준 설명(Task-Level Description)에서 시작된다. 두 부품의 조립, 용기 열기, 소재 접기, 대형 물체 운반과 같은 명령은 로봇의 구체적인 움직임을 직접 지정하지 않는다. 시스템은 이러한 의미적 목표(Semantic Goal)를 명시적인 물체 상태(Object State), 접촉 관계(Contact Relationship), 팔 역할(Arm Role), 기하학적 제약(Geometric Constraint), 완료 조건(Completion Condition)을 포함하는 조작 프리미티브(Manipulation Primitive)로 변환해야 한다.

조작 프리미티브(Manipulation Primitive)는 이러한 변환을 위한 재사용 가능한 기본 구성 요소를 제공한다. 대표적인 프리미티브에는 도달(Reach), 파지(Grasp), 유지(Hold), 들어 올리기(Lift), 이동(Move), 회전(Rotate), 정렬(Align), 삽입(Insert), 당기기(Pull), 밀기(Push), 핸드오버(Handover), 재파지(Regrasp), 배치(Place), 해제(Release)가 있다. 각 프리미티브는 실행 전 만족해야 하는 사전조건(Precondition), 실행 중의 제어 목표(Control Objective), 실행 결과를 정의하는 사후조건(Postcondition)을 갖는다. 복잡한 양팔 행동은 이러한 프리미티브의 연속적인 조합으로 구성할 수 있다.

작업 분해는 두 개의 독립적인 행동 목록을 생성하는 것이 아니라 두 매니퓰레이터(Manipulator) 사이의 상호작용을 표현해야 한다. 비대칭 작업(Asymmetric Task)에서는 한쪽 팔이 물체를 안정화하고 다른 팔이 조작할 수 있으며, 대칭 작업(Symmetric Task)에서는 두 팔이 동일한 물체를 동시에 운반하거나 회전시킬 수 있다. 따라서 계획기는 행동이 함께 시작하거나 종료되고 전환되어야 하는 동기화 지점(Synchronization Point)과 독립적인 병렬 실행(Parallel Execution)이 허용되는 구간을 식별해야 한다.

역할 할당(Role Assignment)은 각 프리미티브를 어느 매니퓰레이터가 수행할 것인지를 결정한다. 왼쪽 팔과 오른쪽 팔은 도달 가능성(Reachability), 가반하중(Payload Capability), 도구 구성(Tool Configuration), 센서 가시성(Sensor Visibility), 관련 물체와의 거리에서 서로 다른 특성을 가질 수 있다. 따라서 역할 선택은 단순히 고정된 좌우 규칙을 적용하는 것이 아니라 기하학적 실현 가능성, 조작성(Manipulability), 예상 실행 비용, 충돌 위험, 작업 관례 또는 학습된 경험을 기반으로 결정할 수 있다.

간단한 작업에서는 역할이 고정될 수 있지만 복잡한 작업에서는 실행 과정에서 역할이 변경되는 경우가 많다. 한쪽 팔이 처음에는 물체를 파지하고 다른 팔이 장애물을 제거한 후 두 팔이 함께 물체를 들어 올릴 수 있다. 이후 한쪽 팔은 안정화 역할로 전환되고 다른 팔은 삽입 작업을 수행할 수 있다. 역할 전환(Role Transition)을 명시적으로 표현하면 작업의 각 단계에서 두 매니퓰레이터가 가진 상호보완적 능력을 효과적으로 활용할 수 있다.

순서화(Sequencing)는 조작 프리미티브가 실행되는 시간적·논리적 순서를 정의한다. 일부 행동에는 엄격한 선행 제약(Precedence Constraint)이 존재한다. 예를 들어 물체를 들어 올리기 전에 먼저 파지해야 하며, 부품을 삽입하기 전에 정렬해야 한다. 반면 한쪽 팔이 고정구(Fixture)를 향해 이동하는 동안 다른 팔이 부품을 확보하는 것처럼 동시에 실행할 수 있는 행동도 있다. 효율적인 순서화는 직렬 의존성(Serial Dependency)과 안전한 병렬 실행을 결합해야 한다.

작업 그래프(Task Graph)는 이러한 관계를 표현하는 유용한 방법이다. 노드(Node)는 조작 상태, 행동 또는 하위 목표(Subgoal)를 나타낼 수 있으며, 에지(Edge)는 유효한 전환과 의존 관계를 표현한다. 방향성 비순환 그래프(Directed Acyclic Graph)는 작업 진행 순서가 대부분 사전에 결정된 경우 유용하며, 재시도, 대체 전략, 복구 경로를 표현해야 하는 경우에는 보다 풍부한 상태 전환 그래프(State-Transition Graph)가 적합하다. 그래프 구조는 작업 논리를 검사하고 검증하기에도 용이하다.

유한 상태 기계(Finite-State Machine)는 구조화된 양팔 작업을 실행하기 위한 실용적인 아키텍처를 제공한다. 각 상태(State)는 하나의 조작 단계를 나타내며 지정된 조건이 충족되면 상태 전환(Transition)이 발생한다. 예를 들어 자세 오차(Pose Error)가 임계값 이하로 감소한 이후에만 접근 상태에서 파지 상태로 전환하고, 접촉과 파지력이 확인된 이후에만 파지 상태에서 들어 올리기 상태로 이동할 수 있다. 실패 조건은 실행을 재시도 또는 복구 상태로 전환할 수 있다.

행동 트리(Behavior Tree)는 대체 행동, 병렬 동작, 재사용 가능한 행동을 포함하는 작업에서 더 높은 유연성을 제공한다. 시퀀스 노드(Sequence Node)는 순차 실행을 강제하고, 선택 노드(Selector Node)는 여러 대체 전략 가운데 하나를 선택하며, 병렬 노드(Parallel Node)는 동시 행동을 조정한다. 개별 조작 기술은 재사용 가능한 리프 노드(Leaf Node)로 구현할 수 있다. 이러한 구성은 변화하는 인식 결과나 실행 결과에 따라 양팔 로봇이 행동 순서를 조정해야 할 때 특히 유용하다.

계획 도메인 정의 언어(Planning Domain Definition Language), 기호 계획(Symbolic Planning), 작업-동작 계획(Task-and-Motion Planning)과 같은 계획 체계는 명시적인 행동 모델에서 실행 순서를 생성할 수 있다. 기호 추론(Symbolic Reasoning)은 어떤 조작 작업을 수행해야 하는지 결정하고, 기하학적 동작 계획(Geometric Motion Planning)은 해당 작업을 물리적으로 실행할 수 있는지를 검증한다. 이러한 결합은 상위 수준 계획기가 논리적으로는 정확하지만 운동학적으로 실행 불가능한 양팔 행동 순서를 생성하는 것을 방지한다.

작업-동작 계획(Task-and-Motion Planning)은 이산적인 조작 결정과 연속적인 로봇 구성이 강하게 결합되어 있기 때문에 특히 중요하다. 어떤 팔이 물체를 파지할 것인지에 대한 선택은 도달 가능성, 충돌 제약, 이후 행동에 영향을 준다. 마찬가지로 파지 자세의 선택은 이후 운반이나 삽입이 가능한지를 결정한다. 따라서 효과적인 계획기는 기호적 작업 추론과 기하학적 실현 가능성 평가 사이에서 정보를 교환하며 두 문제를 독립적으로 해결하지 않는다.

접촉 모드(Contact Mode)는 작업 분해에서 또 다른 중요한 차원을 제공한다. 하나의 조작 순서는 비접촉(No Contact) 상태에서 단일 팔 접촉(Single-Arm Contact), 양팔 강체 파지(Dual-Arm Rigid Grasp), 환경 지지(Environmental Support), 미끄럼 접촉(Sliding Contact), 삽입 접촉(Insertion Contact), 해제 상태로 전환될 수 있다. 각각의 모드는 시스템의 운동학적·동역학적 제약을 변화시킨다. 명시적인 접촉 모드 계획(Contact-Mode Planning)을 사용하면 각 단계에서 제어기와 동작 계획기가 적절한 모델과 제약조건을 적용할 수 있다.

작업 진행 상태를 표현할 때는 로봇의 관절 상태(Joint State)보다 물체 상태(Object State)가 더 유용한 경우가 많다. 조립 작업은 부품 확보(Component Acquired), 부품 정렬(Component Aligned), 삽입 시작(Insertion Started), 삽입 완료(Insertion Completed), 고정구 해제(Fixture Released)와 같은 의미적 조건으로 표현할 수 있다. 로봇 구성은 실제 실행에 여전히 중요하지만 물체 중심 상태 표현(Object-Centered State Representation)을 사용하면 서로 다른 로봇 구조, 초기 자세, 동작 계획 결과에서도 동일한 작업 구조를 재사용할 수 있다.

사전조건(Precondition)과 사후조건(Postcondition)은 조작 기술 사이에 명확한 인터페이스를 형성한다. 파지 프리미티브는 물체 자세가 알려져 있고 목표 파지 위치에 도달할 수 있으며 그리퍼가 열려 있을 것을 사전조건으로 요구할 수 있다. 사후조건에는 접촉 확인과 충분한 파지력 확보가 포함될 수 있다. 이러한 조건을 통해 실행 관리자(Execution Manager)는 단순히 경과 시간에 의존하지 않고 프리미티브를 시작할 수 있는지와 정상적으로 완료되었는지를 판단할 수 있다.

두 프리미티브가 공유 물체를 통해 상호작용할 경우 동기화(Synchronization)는 필수적이다. 협동 들어 올리기(Cooperative Lifting)에서는 수직 이동을 시작하기 전에 두 팔 모두 안정적인 파지를 확보해야 한다. 삽입 작업에서는 능동 팔(Active Arm)이 제약된 궤적을 따라 이동하는 동안 안정화 팔(Stabilizing Arm)이 고정구 자세를 유지해야 할 수 있다. 동기화 장벽(Synchronization Barrier), 공유 이벤트(Shared Event), 타임스탬프(Timestamp), 협조 상태 전환(Coordinated State Transition)을 통해 이러한 시간적 관계를 강제할 수 있다.

모든 양팔 행동에 엄격한 동기화가 필요한 것은 아니다. 두 팔이 서로 독립적인 물체 또는 공간적으로 분리된 영역과 상호작용하는 경우 병렬 처리(Parallelism)를 통해 전체 작업 시간을 줄일 수 있다. 한쪽 팔이 부품을 가져오는 동안 다른 팔이 고정구를 준비할 수 있으며, 두 팔의 궤적과 작업 공간이 서로 충돌하지 않는다면 동시에 실행할 수 있다. 스케줄링 알고리즘(Scheduling Algorithm)은 이러한 독립성을 활용하고 물리적 또는 논리적 의존성이 존재하는 경우에만 동기화를 적용할 수 있다.

순서화 과정에서는 자원 충돌(Resource Conflict)도 고려해야 한다. 두 팔이 동일한 작업 공간, 카메라 시야, 고정구, 도구 또는 물체를 동시에 필요로 할 수 있다. 개별적으로 실행 가능한 행동도 동시에 수행하면 충돌, 가림(Occlusion), 조작 간섭(Manipulation Interference)을 발생시킬 수 있다. 자원 인식 스케줄링(Resource-Aware Scheduling)은 이러한 공유 자원을 명시적으로 표현하고 서로 양립할 수 없는 프리미티브가 동시에 해당 자원을 점유하지 못하도록 한다.

재파지(Regrasping)는 처음 선택한 파지가 전체 작업을 지원하지 못할 수 있기 때문에 작업 순서에 추가적인 복잡성을 발생시킨다. 로봇은 물체를 일시적으로 내려놓거나 한 손에서 다른 손으로 전달하고, 첫 번째 파지를 해제하기 전에 두 번째 파지를 형성할 수 있다. 재파지 계획(Regrasp Planning)은 접촉 구성을 변경하면서 물체 안정성을 유지하는 중간 상태를 탐색하여 작업 공간, 방향, 관절 한계로 인한 제약을 극복할 수 있게 한다.

핸드오버(Handover)는 물체에 대한 책임이 한 매니퓰레이터에서 다른 매니퓰레이터로 이동하는 특수한 협조 전환 과정이다. 물체를 받는 팔은 전달하는 팔이 접촉을 해제하기 전에 적절한 파지를 확보해야 한다. 따라서 성공적인 순서에는 접근, 동기화, 양팔 접촉 확인(Dual-Contact Verification), 하중 전달(Load Transfer), 해제, 후퇴(Retreat)가 포함된다. 힘 또는 촉각 센싱(Tactile Sensing)을 이용하면 다음 단계로 진행하기 전에 물체의 지지가 안전하게 전달되었는지 확인할 수 있다.

오류 감지(Error Detection)는 작업 순서 외부의 예외 처리로 취급하기보다 작업 순서 자체에 통합해야 한다. 인식 시스템이 물체의 예상치 못한 이동을 감지하거나, 파지 검증이 실패하거나, 삽입력이 한계를 초과할 수 있다. 각 프리미티브는 측정 가능한 성공 및 실패 기준을 제공해야 하며, 이를 통해 오류가 이후 행동으로 전파되기 전에 실행을 중단할 수 있다. 이는 강하게 결합된 양팔 조작에서 특히 중요하다.

복구 행동(Recovery Behavior)은 정상적인 실행이 실패할 경우 대체 전환 경로를 제공한다. 파지 실패는 인식 정보 갱신과 재파지를 유발할 수 있고, 과도한 삽입력은 후퇴와 재정렬(Realignment)을 실행하게 할 수 있으며, 협동 접촉이 손실되면 물체를 안정적인 지지면으로 되돌릴 수 있다. 정상 실행 순서와 함께 복구 경로(Recovery Path)를 설계하면 실패가 발생한 이후에 즉흥적으로 대응하는 방식보다 훨씬 강건한 행동을 구현할 수 있다.

온라인 재계획(Online Replanning)은 실행 중 기존 가정이 변경되었을 때 행동 순서를 적응시킬 수 있게 한다. 갱신된 물체 자세, 새롭게 감지된 장애물, 사용할 수 없는 파지, 변화하는 사람의 활동은 기존 계획을 무효화할 수 있다. 로봇은 이미 완료된 하위 목표를 유지하면서 영향을 받은 이후 행동만 다시 생성할 수 있다. 이를 통해 불필요한 반복을 줄이고 완전한 오프라인 순서화(Offline Sequencing)만으로 대응하기 어려운 동적 환경에서도 작업을 수행할 수 있다.

학습(Learning)은 유용한 프리미티브를 발견하거나 행동 성공 가능성을 예측하고, 팔 역할을 선택하거나 여러 대체 순서의 우선순위를 결정함으로써 명시적 작업 분해를 보완할 수 있다. 시범(Demonstration)은 인간이 복잡한 양손 활동을 어떻게 협조된 단계로 분할하는지에 대한 사례를 제공한다. 학습 모델은 수동으로 정의하기 어려운 상황별 선호를 학습할 수 있으며, 기호적 제약과 기하학적 계획기는 계속해서 안전성과 물리적 실현 가능성을 보장할 수 있다.

비전-언어-행동 모델(Vision-Language-Action Model)과 파운데이션 정책(Foundation Policy)은 의미적 해석과 상위 수준 기술 제안을 제공할 수 있지만, 신뢰성 높은 양팔 실행에는 여전히 구조화된 순서화(Structured Sequencing)가 유용하다. 학습 모델이 한쪽 팔로 용기를 잡고 다른 팔로 뚜껑을 제거하도록 제안하면, 실행 아키텍처는 이를 명시적인 기하학적·힘 제약을 갖는 파지, 안정화, 회전, 해제, 복구 상태로 변환할 수 있다.

따라서 계층형 아키텍처(Hierarchical Architecture)는 양팔 작업 실행에 매우 적합하다. 최상위 계층에서는 작업 추론(Task Reasoning)이 목표를 해석하고 하위 목표를 생성한다. 순서화 계층(Sequencing Layer)은 역할을 할당하고 의존 관계를 관리하며 조작 기술을 선택한다. 동작 계획기(Motion Planner)는 충돌 없는 궤적을 생성하고, 저수준 제어기(Low-Level Controller)는 위치, 힘, 임피던스(Impedance), 토크 명령을 실행한다. 인식과 상태 추정(State Estimation)은 이러한 모든 계층에 지속적으로 피드백을 제공한다.

성능은 최종 작업 성공 여부만으로 평가할 필요가 없다. 유용한 평가 지표에는 전체 순서 실행 시간, 프리미티브 전환 횟수, 병렬 실행 비율(Parallel Execution Ratio), 재계획 빈도, 복구 성공률, 동기화 오차(Synchronization Error), 충돌 여유(Collision Margin), 불필요한 팔 움직임 등이 포함될 수 있다. 이러한 지표는 작업 분해가 단순히 성공적인 행동뿐만 아니라 효율적이고 예측 가능하며 강건한 양팔 행동을 생성하는지를 평가할 수 있게 한다.

궁극적으로 양팔 작업 분해 및 순서화(Bimanual Task Decomposition and Sequencing)는 의미적 목표(Semantic Goal)를 협조된 물리적 실행(Coordinated Physical Execution)과 연결하는 조직적 논리를 제공한다. 조작을 재사용 가능한 프리미티브, 팔 역할, 의존 관계, 접촉 모드, 동기화 이벤트, 성공 조건, 복구 전환으로 표현함으로써 로봇은 복잡한 양손 활동을 관리 가능한 작업으로 변환하면서 적응형 및 범용 조작(General-Purpose Manipulation)에 필요한 유연성을 유지할 수 있다.

##  

## 08.04. Object Handover Between Two Arms [w/Code]

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

Object handover between two robot arms is a coordinated manipulation process in which control and physical support of an object are transferred from a delivering manipulator to a receiving manipulator. Although the action appears simple, reliable handover requires synchronized perception, grasp planning, motion generation, contact verification, force regulation, and release timing so that object stability is preserved throughout the transfer.

Handover is useful when a single arm cannot complete an entire manipulation task because of workspace, orientation, collision, or joint-limit constraints. An object can be acquired in one region, transferred between manipulators, and then repositioned for subsequent assembly, inspection, insertion, packaging, or placement. Handover therefore extends effective workspace and enables configurations that cannot be achieved conveniently with one continuous grasp.

The handover process can be represented as a sequence of object-centered contact states. Initially, the delivering arm has exclusive control of the object. The receiving arm then approaches a feasible secondary grasp, establishes contact, and creates a temporary dual-arm grasp. Load is transferred toward the receiving arm, after which the delivering gripper releases and retreats, leaving the receiving arm with exclusive control.

A suitable handover pose must satisfy the requirements of both manipulators simultaneously. The object should be positioned within their shared workspace while maintaining feasible joint configurations, sufficient manipulability, collision clearance, and accessible grasp surfaces. Selecting the transfer location only from geometric proximity can produce awkward postures or restrict subsequent motion, so future task requirements should also influence handover-pose selection.

Object orientation at the transfer pose is equally important. The receiving arm may need a particular grasp orientation to perform the next operation, while the delivering arm must remain capable of supporting the object during transfer. Planning the handover in object space allows the system to optimize object position and orientation together rather than considering only the relative locations of the two grippers.

Grasp-pair selection determines how the two manipulators can temporarily share the object. The delivering grasp must leave sufficient surface area for the receiving gripper, and the two grippers must avoid collision with each other. Candidate grasp pairs can be evaluated using grasp stability, reachability, force closure, approach direction, manipulability, collision margin, and compatibility with the desired post-handover task.

The receiving approach trajectory requires careful collision planning because it intentionally moves one gripper close to another gripper and the held object. Standard collision avoidance must distinguish prohibited collisions from intended contact. The planner should preserve clearance between robot links while allowing the receiving fingers to enter the target grasp region and establish controlled contact with the object.

Accurate perception is necessary to estimate the actual object pose during handover. The object may differ from its expected position because of calibration errors, grasp offsets, compliance, or motion of the delivering arm. Vision can provide global pose information, while proprioception, force-torque sensing, and tactile sensing provide local information about contact and relative alignment near the transfer point.

Occlusion becomes particularly challenging as two manipulators surround the same object. Cameras may lose visibility of grasp surfaces or object features during the final approach. Multi-view perception, wrist-mounted cameras, depth sensing, tactile feedback, and model-based state estimation can maintain object-state estimates even when external vision becomes temporarily unreliable. Sensor fusion therefore improves robustness during close-range coordination.

Relative pose accuracy between the two end effectors is more important than absolute world-frame accuracy in many handover operations. Even if the object has a small global localization error, transfer may succeed when the receiving gripper can accurately determine its pose relative to the delivering gripper and object. Shared coordinate frames and precise extrinsic calibration are therefore essential for repeatable handover.

The transition into dual-arm contact creates a temporary closed kinematic chain. Once both grippers constrain the object, independent pose errors can generate internal forces. The controllers should therefore avoid rigidly commanding inconsistent Cartesian poses. Compliant strategies such as impedance or admittance control allow small relative deviations while maintaining sufficient contact forces to stabilize the object.

Force regulation becomes central during load transfer. Before the receiving arm makes contact, the delivering arm supports essentially the entire object load. After contact is established, support should gradually shift between manipulators rather than changing instantaneously. Force-torque measurements can estimate how the load is distributed and provide feedback for smoothly increasing receiving-arm support while reducing delivering-arm support.

Internal forces should be controlled independently from forces associated with object motion whenever possible. Excessive opposing forces between the grippers can deform fragile objects, cause slipping, or overload the manipulators. Cooperative control can regulate the object\'s desired motion while minimizing unnecessary internal forces, maintaining only the amount needed for stable contact during the dual-grasp phase.

Contact verification prevents premature release. Closing the receiving gripper does not guarantee that a secure grasp has been established. Verification may use finger position, motor current, tactile pressure, force-torque measurements, slip detection, or small probing motions. The delivering arm should retain support until the system confirms that the receiving grasp satisfies predefined stability conditions.

A load-transfer test provides stronger evidence than contact detection alone. The delivering arm can slightly reduce its support while the receiving arm compensates for the object weight. If measured motion, force, or slip remains within acceptable limits, the system gains confidence that the receiving grasp can safely carry the object. Otherwise, the original support can be restored without dropping the object.

Release timing is a critical event in the handover sequence. The delivering gripper should open only after the receiving grasp and load support have been verified. Release may be gradual for objects sensitive to disturbance, allowing the controller to observe changes in force distribution as contact disappears. Event-based release is generally more robust than relying on a predetermined time delay.

After release, the delivering arm must retreat without colliding with the object, receiving gripper, or surrounding environment. The retreat direction should be considered during grasp and handover planning rather than generated as an afterthought. A grasp pair that allows acquisition but traps the delivering fingers after transfer is unsuitable even if the temporary dual grasp is stable.

Handover can be performed statically or dynamically. Static handover brings the object to a nearly stationary transfer pose before the receiving arm establishes contact. This simplifies perception and synchronization and is appropriate for precision manipulation. Dynamic handover transfers the object while it is moving, potentially reducing cycle time but requiring more accurate trajectory prediction, synchronization, and compliant control.

Dynamic handover requires the receiving arm to match the object\'s position, orientation, linear velocity, and potentially angular velocity at the transfer moment. Trajectory coordination can create a moving rendezvous in which both manipulators share compatible motion for a short interval. Velocity mismatch at contact should be minimized because sudden relative motion can generate impact forces or destabilize the grasp.

Synchronization can be implemented using shared task states rather than only synchronized clocks. The receiving arm can report states such as approaching, contact detected, grasp secured, and load accepted, while the delivering arm reports supporting, transfer ready, unloading, and released. Explicit state exchange makes the protocol easier to monitor and provides clear conditions for transitions, timeout handling, and recovery.

Finite-state machines and behavior trees are well suited to handover control. States can represent transfer-pose acquisition, receiving approach, contact establishment, grasp verification, dual-arm support, load transfer, delivering release, and retreat. Failure transitions can return the system to a previous safe state, request perception updates, or place the object on a stable support surface when direct recovery is not possible.

Failures may occur because the receiving arm misses the target grasp, the object shifts during contact, insufficient gripping force is detected, or collision risk becomes excessive. Robust systems avoid committing irreversibly to the next state until critical conditions have been verified. Maintaining the delivering grasp during early failures provides a valuable recovery advantage because the object remains under controlled support.

Regrasping can be integrated with handover when the purpose is not simply to exchange ownership but to change object orientation or grasp configuration. The receiving arm can acquire the object at a new contact region, rotate or reposition it, and potentially return it to the original arm with a more favorable grasp. Repeated handovers can therefore function as an in-hand reorientation mechanism at the bimanual system level.

Handover planning should consider downstream manipulation. A receiving grasp that is stable but blocks an assembly feature or forces the arm toward a joint limit may create problems immediately after transfer. Task-aware planning evaluates candidate handovers according to both transfer success and the feasibility of future operations, reducing the need for unnecessary additional regrasping.

Learning-based methods can improve handover by predicting successful grasp pairs, transfer poses, approach trajectories, and release conditions from demonstrations or previous experience. Reinforcement learning and imitation learning can capture compliant coordination strategies that are difficult to encode analytically. Learned components can be combined with geometric planners and force-based safety constraints to preserve physical reliability.

Simulation provides a useful environment for evaluating handover strategies before deployment. Variations in object mass, friction, pose error, grasp offset, sensor noise, communication latency, and controller gains can be introduced systematically. Successful transfer rate, peak internal force, object displacement, handover duration, minimum collision distance, and recovery success can then quantify robustness.

Physical validation should progressively increase difficulty from rigid objects and static transfer poses to uncertain poses, fragile objects, asymmetric mass distributions, dynamic transfers, and cluttered environments. High-speed logging of joint states, end-effector poses, gripper states, force-torque measurements, tactile signals, and task transitions helps identify whether failures originate from perception, planning, synchronization, or control.

Ultimately, object handover is a controlled transition of contact authority rather than a simple open-and-close sequence. Reliable transfer requires the two manipulators to share object state, coordinate their trajectories, temporarily support the object together, verify the receiving grasp, regulate load distribution, and release only when physical evidence confirms stability. These principles make handover a fundamental capability for flexible bimanual manipulation.

두 로봇 팔 사이의 물체 핸드오버(Object Handover Between Two Arms)는 물체에 대한 제어권과 물리적 지지를 전달 매니퓰레이터(Delivering Manipulator)에서 수신 매니퓰레이터(Receiving Manipulator)로 이전하는 협조 조작 과정이다. 동작 자체는 단순해 보이지만 신뢰성 높은 핸드오버를 위해서는 동기화된 인식(Perception), 파지 계획(Grasp Planning), 동작 생성(Motion Generation), 접촉 검증(Contact Verification), 힘 조절(Force Regulation), 해제 시점(Release Timing)이 필요하며, 전체 전달 과정에서 물체의 안정성을 유지해야 한다.

핸드오버(Handover)는 작업 공간(Workspace), 방향(Orientation), 충돌(Collision), 관절 한계(Joint Limit) 등의 제약으로 하나의 로봇 팔이 전체 조작 작업을 완료하기 어려운 경우 유용하다. 한 영역에서 물체를 확보한 후 두 매니퓰레이터 사이에서 전달하고, 이후 조립(Assembly), 검사(Inspection), 삽입(Insertion), 포장(Packaging), 배치(Placement)에 적합한 자세로 변경할 수 있다. 따라서 핸드오버는 유효 작업 공간을 확장하고 하나의 연속적인 파지만으로 구현하기 어려운 구성을 가능하게 한다.

핸드오버 과정은 물체 중심 접촉 상태(Object-Centered Contact State)의 연속으로 표현할 수 있다. 초기에는 전달 팔(Delivering Arm)이 물체를 독점적으로 제어한다. 이후 수신 팔(Receiving Arm)이 실행 가능한 보조 파지(Secondary Grasp)에 접근하고 접촉을 형성하여 일시적인 양팔 파지(Dual-Arm Grasp)를 만든다. 하중은 점진적으로 수신 팔로 전달되고, 이후 전달 그리퍼가 물체를 해제하고 후퇴하면서 수신 팔이 물체를 독점적으로 제어하게 된다.

적절한 핸드오버 자세(Handover Pose)는 두 매니퓰레이터의 요구조건을 동시에 만족해야 한다. 물체는 공유 작업 공간(Shared Workspace) 내에 위치하면서 실행 가능한 관절 구성, 충분한 조작성(Manipulability), 충돌 여유(Collision Clearance), 접근 가능한 파지 표면을 확보해야 한다. 단순한 기하학적 근접성만으로 전달 위치를 선택하면 불편한 자세나 이후 동작의 제약이 발생할 수 있으므로 후속 작업 요구조건도 핸드오버 자세 선택에 반영해야 한다.

전달 자세에서 물체의 방향(Object Orientation) 역시 중요하다. 수신 팔은 다음 작업을 수행하기 위해 특정한 파지 방향이 필요할 수 있으며, 전달 팔은 전달 과정에서 계속 물체를 안정적으로 지지할 수 있어야 한다. 물체 공간(Object Space)에서 핸드오버를 계획하면 두 그리퍼의 상대적인 위치만 고려하는 대신 물체의 위치와 방향을 함께 최적화할 수 있다.

파지 쌍 선택(Grasp-Pair Selection)은 두 매니퓰레이터가 물체를 일시적으로 어떻게 공유할 것인지를 결정한다. 전달 파지는 수신 그리퍼가 접근할 수 있는 충분한 표면을 남겨야 하며, 두 그리퍼 사이의 충돌도 방지해야 한다. 후보 파지 쌍은 파지 안정성(Grasp Stability), 도달 가능성(Reachability), 힘 폐쇄(Force Closure), 접근 방향(Approach Direction), 조작성, 충돌 여유, 핸드오버 이후 작업과의 호환성을 기준으로 평가할 수 있다.

수신 팔의 접근 궤적(Approach Trajectory)은 한 그리퍼가 다른 그리퍼와 파지된 물체에 의도적으로 가까이 이동하기 때문에 세심한 충돌 계획(Collision Planning)이 필요하다. 일반적인 충돌 회피(Collision Avoidance)는 금지된 충돌과 의도된 접촉을 구분해야 한다. 계획기는 로봇 링크 사이의 안전거리를 유지하면서 수신 그리퍼의 손가락이 목표 파지 영역에 진입하고 물체와 제어된 접촉을 형성하도록 해야 한다.

핸드오버 중 실제 물체 자세를 추정하려면 정확한 인식(Perception)이 필요하다. 보정 오차(Calibration Error), 파지 오프셋(Grasp Offset), 순응성(Compliance), 전달 팔의 움직임으로 인해 물체가 예상 위치와 달라질 수 있다. 비전(Vision)은 전역적인 자세 정보를 제공하며, 고유수용감각(Proprioception), 힘-토크 센싱(Force-Torque Sensing), 촉각 센싱(Tactile Sensing)은 전달 지점 근처의 접촉과 상대 정렬에 관한 국부 정보를 제공한다.

두 매니퓰레이터가 동일한 물체를 둘러싸면 가림(Occlusion)이 특히 중요한 문제가 된다. 최종 접근 과정에서 카메라가 파지 표면이나 물체 특징을 관측하지 못할 수 있다. 다중 시점 인식(Multi-View Perception), 손목 장착 카메라(Wrist-Mounted Camera), 깊이 센싱(Depth Sensing), 촉각 피드백, 모델 기반 상태 추정(Model-Based State Estimation)을 활용하면 외부 비전의 신뢰성이 일시적으로 감소하더라도 물체 상태 추정을 유지할 수 있다. 따라서 센서 융합(Sensor Fusion)은 근거리 협조 과정의 강건성을 향상시킨다.

많은 핸드오버 작업에서는 절대적인 세계 좌표계 정확도보다 두 말단장치(End Effector) 사이의 상대 자세 정확도(Relative Pose Accuracy)가 더욱 중요하다. 물체의 전역 위치에 작은 오차가 있더라도 수신 그리퍼가 전달 그리퍼 및 물체에 대한 상대 자세를 정확하게 결정할 수 있다면 전달에 성공할 수 있다. 따라서 공유 좌표계(Shared Coordinate Frame)와 정밀한 외부 보정(Extrinsic Calibration)은 반복 가능한 핸드오버를 구현하는 데 필수적이다.

양팔 접촉(Dual-Arm Contact)으로 전환하면 일시적인 폐쇄 운동학 체인(Closed Kinematic Chain)이 형성된다. 두 그리퍼가 물체를 동시에 구속하면 독립적인 자세 오차가 내부 힘(Internal Force)을 발생시킬 수 있다. 따라서 제어기는 서로 일치하지 않는 직교좌표 자세(Cartesian Pose)를 강체적으로 명령하지 않아야 한다. 임피던스 제어(Impedance Control)나 어드미턴스 제어(Admittance Control)와 같은 순응 제어 전략은 물체 안정화에 필요한 접촉력을 유지하면서 작은 상대 오차를 허용한다.

힘 조절(Force Regulation)은 하중 전달(Load Transfer) 과정에서 핵심적인 역할을 한다. 수신 팔이 접촉하기 전에는 전달 팔이 사실상 물체의 전체 하중을 지지한다. 접촉이 형성된 이후에는 지지 하중이 두 매니퓰레이터 사이에서 순간적으로 변경되는 것이 아니라 점진적으로 이동해야 한다. 힘-토크 측정을 통해 하중 분배 상태를 추정하고, 전달 팔의 지지력을 감소시키면서 수신 팔의 지지력을 부드럽게 증가시키도록 피드백을 제공할 수 있다.

가능한 경우 내부 힘(Internal Force)은 물체 운동에 필요한 힘과 독립적으로 제어해야 한다. 두 그리퍼 사이에 과도한 반대 방향의 힘이 발생하면 깨지기 쉬운 물체가 변형되거나 미끄러짐이 발생하고 매니퓰레이터에 과부하가 걸릴 수 있다. 협동 제어(Cooperative Control)는 물체의 목표 움직임을 조절하면서 불필요한 내부 힘을 최소화하고, 양팔 파지 단계에서 안정적인 접촉을 유지하는 데 필요한 수준의 힘만 유지할 수 있다.

접촉 검증(Contact Verification)은 너무 이른 해제를 방지한다. 수신 그리퍼를 닫았다는 사실만으로 안정적인 파지가 형성되었다고 보장할 수 없다. 검증에는 손가락 위치(Finger Position), 모터 전류(Motor Current), 촉각 압력(Tactile Pressure), 힘-토크 측정, 미끄러짐 감지(Slip Detection), 작은 탐색 동작(Probing Motion) 등을 사용할 수 있다. 전달 팔은 수신 파지가 사전에 정의된 안정성 조건을 만족한다는 것이 확인될 때까지 물체를 계속 지지해야 한다.

하중 전달 시험(Load-Transfer Test)은 단순한 접촉 감지보다 강한 검증 정보를 제공한다. 전달 팔이 지지력을 조금 감소시키고 수신 팔이 물체의 무게를 보상하도록 할 수 있다. 이 과정에서 측정된 움직임, 힘, 미끄러짐이 허용 범위 내에 유지되면 수신 파지가 물체를 안전하게 지지할 수 있다는 신뢰도가 높아진다. 문제가 발생하면 물체를 떨어뜨리지 않고 원래의 지지 상태로 복귀할 수 있다.

해제 시점(Release Timing)은 핸드오버 순서에서 매우 중요한 이벤트이다. 전달 그리퍼는 수신 파지와 하중 지지가 모두 검증된 이후에만 열려야 한다. 외란에 민감한 물체의 경우 점진적인 해제(Gradual Release)를 사용하여 접촉이 사라지는 동안 힘 분배의 변화를 관찰할 수 있다. 일반적으로 사전에 설정된 시간 지연에 의존하는 방식보다 이벤트 기반 해제(Event-Based Release)가 더 높은 강건성을 제공한다.

해제 이후 전달 팔은 물체, 수신 그리퍼, 주변 환경과 충돌하지 않으면서 후퇴해야 한다. 후퇴 방향(Retreat Direction)은 전달이 끝난 이후 임의로 생성하기보다 파지 및 핸드오버 계획 단계에서 미리 고려해야 한다. 물체를 잡는 것은 가능하지만 전달 후 전달 그리퍼의 손가락이 빠져나올 수 없는 파지 쌍은 일시적인 양팔 파지가 안정적이더라도 적절하지 않다.

핸드오버는 정적 핸드오버(Static Handover) 또는 동적 핸드오버(Dynamic Handover) 방식으로 수행할 수 있다. 정적 핸드오버는 물체를 거의 정지된 전달 자세로 이동시킨 후 수신 팔이 접촉하도록 한다. 이는 인식과 동기화를 단순화하므로 정밀 조작에 적합하다. 동적 핸드오버는 물체가 이동하는 동안 전달하여 작업 주기 시간(Cycle Time)을 단축할 수 있지만 더욱 정확한 궤적 예측, 동기화, 순응 제어가 요구된다.

동적 핸드오버에서는 수신 팔이 전달 순간의 물체 위치, 방향, 선속도(Linear Velocity), 필요에 따라 각속도(Angular Velocity)까지 일치시켜야 한다. 궤적 협조(Trajectory Coordination)를 통해 두 매니퓰레이터가 짧은 시간 동안 호환되는 움직임을 공유하는 이동 랑데부(Moving Rendezvous)를 생성할 수 있다. 접촉 순간의 속도 불일치는 충격력을 발생시키거나 파지를 불안정하게 만들 수 있으므로 최소화해야 한다.

동기화(Synchronization)는 단순히 시계를 맞추는 방식뿐만 아니라 공유 작업 상태(Shared Task State)를 이용하여 구현할 수 있다. 수신 팔은 접근 중(Approaching), 접촉 감지(Contact Detected), 파지 확보(Grasp Secured), 하중 수용(Load Accepted)과 같은 상태를 보고할 수 있으며, 전달 팔은 지지 중(Supporting), 전달 준비(Transfer Ready), 하중 해제 중(Unloading), 해제 완료(Released) 등의 상태를 보고할 수 있다. 명시적인 상태 교환은 프로토콜을 모니터링하기 쉽게 하고 상태 전환, 시간 초과, 복구를 위한 명확한 조건을 제공한다.

유한 상태 기계(Finite-State Machine)와 행동 트리(Behavior Tree)는 핸드오버 제어에 적합하다. 상태는 전달 자세 확보, 수신 접근, 접촉 형성, 파지 검증, 양팔 지지, 하중 전달, 전달 팔 해제, 후퇴 등을 표현할 수 있다. 실패 전환(Failure Transition)은 시스템을 이전의 안전한 상태로 되돌리거나 인식 정보 갱신을 요청할 수 있으며, 직접적인 복구가 불가능하면 물체를 안정적인 지지면에 배치하도록 할 수 있다.

수신 팔이 목표 파지를 놓치거나 접촉 과정에서 물체가 이동하고, 충분한 파지력이 확보되지 않거나 충돌 위험이 지나치게 증가하는 등의 실패가 발생할 수 있다. 강건한 시스템은 중요한 조건이 검증되기 전에는 다음 상태로 비가역적으로 전환하지 않는다. 초기 실패 과정에서 전달 파지를 유지하면 물체가 계속 제어된 상태로 지지되므로 복구 측면에서 큰 장점을 제공한다.

핸드오버의 목적이 단순한 소유권 전달이 아니라 물체의 방향 또는 파지 구성을 변경하는 것이라면 재파지(Regrasping)를 핸드오버와 통합할 수 있다. 수신 팔은 새로운 접촉 영역에서 물체를 확보하고 회전 또는 재배치한 후, 필요하면 보다 유리한 파지 상태로 원래 팔에 다시 전달할 수 있다. 따라서 반복적인 핸드오버는 양팔 시스템 수준에서 손 내부 재방향 조작(In-Hand Reorientation)과 유사한 기능을 수행할 수 있다.

핸드오버 계획은 후속 조작(Downstream Manipulation)을 고려해야 한다. 안정적인 수신 파지라도 조립 부위를 가리거나 로봇 팔을 관절 한계에 가까운 자세로 만든다면 전달 직후 문제가 발생할 수 있다. 작업 인식 계획(Task-Aware Planning)은 전달 자체의 성공 가능성뿐만 아니라 이후 작업의 실현 가능성까지 기준으로 후보 핸드오버를 평가하여 불필요한 추가 재파지를 줄인다.

학습 기반 방법(Learning-Based Method)은 시범(Demonstration)이나 이전 경험으로부터 성공적인 파지 쌍, 전달 자세, 접근 궤적, 해제 조건을 예측하여 핸드오버 성능을 향상시킬 수 있다. 강화학습(Reinforcement Learning)과 모방학습(Imitation Learning)은 분석적으로 정의하기 어려운 순응 협조 전략(Compliant Coordination Strategy)을 학습할 수 있다. 학습 요소는 기하학적 계획기(Geometric Planner)와 힘 기반 안전 제약(Force-Based Safety Constraint)을 결합하여 물리적 신뢰성을 유지할 수 있다.

시뮬레이션(Simulation)은 실제 시스템에 적용하기 전에 핸드오버 전략을 평가할 수 있는 유용한 환경을 제공한다. 물체 질량, 마찰(Friction), 자세 오차, 파지 오프셋, 센서 노이즈, 통신 지연(Communication Latency), 제어기 게인(Controller Gain)을 체계적으로 변화시킬 수 있다. 전달 성공률, 최대 내부 힘, 물체 변위, 핸드오버 시간, 최소 충돌 거리, 복구 성공률 등을 측정하여 시스템의 강건성을 정량적으로 평가할 수 있다.

실제 검증(Physical Validation)은 강체 물체와 정적 전달 자세에서 시작하여 불확실한 자세, 깨지기 쉬운 물체, 비대칭 질량 분포, 동적 전달, 복잡한 환경으로 점진적으로 난이도를 높여야 한다. 관절 상태, 말단장치 자세, 그리퍼 상태, 힘-토크 측정, 촉각 신호, 작업 상태 전환을 고속으로 기록하면 실패 원인이 인식, 계획, 동기화, 제어 가운데 어디에서 발생했는지 분석할 수 있다.

궁극적으로 물체 핸드오버(Object Handover)는 단순한 그리퍼 열기와 닫기의 연속이 아니라 접촉 권한(Contact Authority)을 제어된 방식으로 전환하는 과정이다. 신뢰성 높은 전달을 위해 두 매니퓰레이터는 물체 상태를 공유하고 궤적을 협조하며, 일시적으로 물체를 함께 지지하고, 수신 파지를 검증하며, 하중 분배를 조절하고, 물리적 증거를 통해 안정성이 확인된 이후에만 물체를 해제해야 한다. 이러한 원리는 핸드오버를 유연한 양팔 조작(Bimanual Manipulation)을 위한 핵심 기능으로 만든다.

##  

## 08.05. Cooperative Bimanual Force Control [w/Code]

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

Cooperative bimanual force control enables two robotic manipulators to interact with a shared object while regulating both its motion and the forces transmitted through their contacts. Unlike independent arm control, the manipulators are mechanically coupled through the object, so an action by one arm immediately influences the other. Stable coordination therefore requires explicit treatment of object dynamics, contact forces, internal forces, compliance, and synchronization.

When both grippers rigidly grasp the same object, the arms and object form a closed mechanical chain. The desired motion must satisfy geometric constraints imposed by both grasp locations, while commanded forces must remain mutually compatible. Small pose errors, calibration offsets, or controller mismatches can create unexpectedly large interaction forces because neither arm can move independently without affecting the shared object.

A useful force-control formulation separates forces that produce object motion from internal forces that act between the two contacts without contributing to the object\'s net acceleration. Object-level forces and moments determine translation and rotation, whereas internal forces maintain grasp stability. Cooperative control should generate sufficient internal force to prevent slip while avoiding unnecessary compression, tension, or torsion.

The grasp matrix provides a mathematical relationship between contact wrenches and the resultant wrench applied to the object. Forces and moments measured or commanded at the two end effectors can be mapped into an object-centered representation. This allows the controller to determine which combinations contribute to desired object motion and which lie in the internal-force subspace and therefore do not alter the object\'s overall motion.

Internal-force regulation is especially important for fragile, compliant, or deformable objects. Excessive opposing force can crush packaging, bend components, or deform sheets, while insufficient force may allow slipping or loss of contact. Desired internal force should therefore depend on object mass, friction, grasp geometry, acceleration, material properties, and the expected disturbances encountered during manipulation.

Force closure provides a useful criterion for evaluating whether the available contacts can resist disturbances in required directions. A cooperative grasp should generate admissible contact forces while respecting friction constraints and actuator limits. Force-control planning can combine grasp geometry with friction-cone models to determine whether commanded object wrenches can be produced without violating contact stability.

Force-torque sensors mounted near the wrists provide direct measurements of interaction wrenches at the end effectors. These measurements contain contributions from object weight, inertial forces, contact reactions, and internal forces. Gravity compensation, sensor bias estimation, tool calibration, and dynamic compensation are required before measurements can be interpreted accurately for cooperative control.

Tactile sensing can complement wrist force-torque measurements by providing distributed information about local pressure, contact location, and slip. Two grasps with similar net wrench measurements may have very different local contact conditions. Tactile arrays can detect uneven loading or incipient slip and allow the controller to adjust gripping force before global object stability is compromised.

Pure position control is risky during rigid dual-arm grasping because small geometric inconsistencies can create high forces. If both manipulators attempt to track independently specified Cartesian trajectories with high stiffness, calibration and tracking errors may cause them to fight each other. Cooperative manipulation therefore commonly introduces compliance so that small relative deviations can be absorbed without generating excessive interaction forces.

Impedance control defines a desired dynamic relationship between pose error and interaction force. Each manipulator can behave like a virtual mass-spring-damper system rather than an infinitely stiff position source. Properly selected stiffness and damping allow the arms to follow the desired object trajectory while accommodating small geometric errors and external disturbances during cooperative manipulation.

Admittance control provides the complementary relationship by converting measured interaction forces into commanded motion. It is useful when the underlying robot controller accepts position or velocity commands but force-responsive behavior is required. In a bimanual system, measured contact forces can generate corrective motions that reduce excessive internal loading or allow the object to comply with environmental contact.

Hybrid position-force control separates directions that should be controlled primarily in motion from directions that should be regulated in force. During cooperative insertion, for example, the object may follow a commanded motion along the insertion axis while lateral forces are limited to support alignment. Selecting appropriate motion and force subspaces enables accurate task execution without excessive contact stress.

Object-level impedance control treats the shared object as the primary controlled body. A desired object trajectory and impedance are defined, and the required resultant wrench is distributed between the two manipulators. This approach naturally coordinates the arms because their commands originate from a common object objective rather than independently specified end-effector trajectories.

Wrench distribution determines how the required object force and moment are shared between the manipulators. Equal distribution may be suitable for symmetric grasps, but many tasks require asymmetric allocation because of different arm configurations, actuator capabilities, grasp quality, or contact geometry. Optimization can distribute loads while minimizing joint torques, avoiding saturation, and preserving desired internal forces.

Redundant force distribution creates opportunities for optimization because multiple contact-wrench combinations can produce the same resultant object wrench. Secondary objectives can minimize energy, balance actuator effort, maximize friction margin, reduce structural loading, or keep one arm lightly loaded for a subsequent action. Constraints can simultaneously enforce contact stability and force limits.

Gravity compensation is essential during cooperative transport. The combined manipulators must support the object\'s weight while generating additional forces for acceleration and disturbance rejection. If the estimated mass or center of mass is inaccurate, the arms may experience biased loads and unintended internal forces. Online estimation can improve compensation when object properties are uncertain.

Object inertia becomes increasingly important during fast motion. Translational and rotational acceleration generate dynamic loads that must be distributed through both contacts. A controller designed only for static force balance may perform poorly during rapid transport or rotation. Dynamic models can predict inertial wrenches and provide feedforward compensation while feedback corrects modeling errors.

The center of mass strongly affects load sharing. When the center of mass lies closer to one grasp, that manipulator may naturally support a larger portion of the weight or torque. Assuming symmetric load distribution in such cases can create unnecessary internal forces. Estimating the center of mass from geometry, prior models, or measured force responses allows more physically appropriate wrench allocation.

Contact with the environment introduces another layer of force coordination. During assembly, polishing, cutting, or surface following, forces arise not only between the two manipulators and object but also between the object and environment. The controller must regulate environmental interaction while maintaining stable dual-arm grasping, effectively coordinating several simultaneous contact constraints.

Cooperative insertion illustrates this challenge clearly. Both arms may hold a large component while guiding it into a fixture. The object trajectory must remain coordinated, insertion forces should remain below safe limits, and lateral contact forces can be used as alignment information. Compliance allows small corrective motion while preventing jamming caused by excessive positional stiffness.

Force control must also account for communication and control latency. If one manipulator responds to a disturbance significantly later than the other, the temporary mismatch can generate internal force oscillations. Synchronized control cycles, timestamped sensor data, deterministic communication, and consistent state estimation help both arms respond coherently to rapidly changing interaction forces.

Controller bandwidth should be selected according to robot mechanics, sensor bandwidth, communication delays, and contact stiffness. Excessively aggressive force feedback can amplify noise or excite structural resonances, while very low bandwidth produces slow disturbance rejection. Filtering and damping should reduce undesirable oscillations without introducing enough delay to destabilize contact interaction.

Passivity and stability analysis provide important tools for designing safe cooperative controllers. Two actively controlled manipulators coupled through a stiff object can exchange energy in ways that are not apparent when each arm is analyzed separately. Energy-aware control, passivity observers, damping injection, and conservative impedance settings can help prevent unstable oscillations during uncertain contact.

Safety limits should exist independently of nominal force-control objectives. Maximum contact force, joint torque, internal force, object wrench, and rate-of-force-change thresholds can trigger reduced motion, compliance increase, controlled release, or emergency stop. Such supervision is particularly important when object properties, grasp quality, or environmental contact conditions are not fully known.

State estimation combines joint measurements, force-torque sensing, tactile signals, object pose, and dynamic models to infer quantities that cannot be measured directly. Internal force, object acceleration, center-of-mass location, contact state, and slip probability can all be estimated online. Reliable estimates allow force-control policies to adapt rather than relying on fixed assumptions.

Adaptive control can modify impedance, force references, or wrench distribution as object and contact properties become known during execution. A robot may initially use conservative low-force behavior, estimate stiffness or friction from measured responses, and then adjust its control parameters. This is valuable when manipulating unfamiliar objects whose mechanical characteristics are unavailable beforehand.

Learning-based approaches can further improve cooperative force control by learning contact models, impedance parameters, load-sharing policies, or corrective actions from demonstrations and interaction data. Reinforcement learning can optimize behavior under complex contact dynamics, while imitation learning can reproduce skilled cooperative manipulation. Safety constraints remain important when learned policies command physical interaction forces.

Simulation enables systematic testing of force controllers under variations in object mass, friction, stiffness, center of mass, grasp pose, sensor noise, delay, and calibration error. Evaluation can measure object tracking error, peak internal force, load-sharing error, contact stability, energy consumption, oscillation, and recovery from disturbances before algorithms are transferred to hardware.

Physical validation should progress from quasi-static cooperative holding to transport, rotation, disturbance rejection, environmental contact, and dynamic manipulation. Controlled perturbations can test whether the two arms maintain object pose while regulating internal forces. Fragile or compliant test objects are useful for verifying that force limits and compliance mechanisms operate correctly under realistic uncertainty.

Ultimately, cooperative bimanual force control coordinates two manipulators as parts of a single physical system rather than as independent position-controlled robots. By separating object wrench from internal force, regulating contact through compliant control, distributing loads according to task and robot capability, and continuously using force and tactile feedback, dual-arm robots can manipulate shared objects safely, accurately, and robustly.

협동 양팔 힘 제어(Cooperative bimanual force control)는 두 개의 로봇 매니퓰레이터가 공유된 물체와 상호작용하는 동시에, 물체의 운동과 접촉을 통해 전달되는 힘을 모두 제어할 수 있게 합니다. 독립적인 단일 팔 제어와 달리, 매니퓰레이터들은 물체를 통해 기계적으로 결합되어 있으므로 한쪽 팔의 동작이 다른쪽 팔에 즉각적인 영향을 미칩니다. 따라서 안정적인 협동을 위해서는 물체 동역학, 접촉력, 내부력, 컴플라이언스(compliance), 그리고 동기화에 대한 명시적인 처리가 필요합니다.

두 그리퍼가 동일한 물체를 강체 파지(rigid grasp)할 때, 두 팔과 물체는 폐쇄적 기계 사슬(closed mechanical chain)을 형성합니다. 원하는 운동은 두 파지 위치에 의해 부과되는 기하학적 제약 조건을 충족해야 하며, 명령된 힘은 상호 호환성을 유지해야 합니다. 두 팔 모두 공유된 물체에 영향을 주지 않고 독립적으로 움직일 수 없기 때문에, 미세한 포즈(pose) 오차, 캘리브레이션 편차, 또는 제어기 불일치가 예상치 못한 큰 상호작용력을 유발할 수 있습니다.

유용한 힘 제어 정식화는 물체의 운동을 발생시키는 힘과, 물체의 알짜 가속도에는 기여하지 않으면서 두 접촉점 사이에서 작용하는 내부력을 분리합니다. 물체 수준의 힘과 모멘트는 병진 및 회전 운동을 결정하는 반면, 내부력은 파지 안정성을 유지합니다. 협동 제어는 미끄러짐을 방지하기에 충분한 내부력을 생성하는 동시에 불필요한 압축, 인장 또는 비틀림을 피해야 합니다.

파지 행렬(grasp matrix)은 접촉 렌치(contact wrench)와 물체에 가해지는 결과 렌치 사이의 수학적 관계를 제공합니다. 두 엔드 이펙터(end effector)에서 측정되거나 명령된 힘과 모멘트는 물체 중심 표현으로 사상(mapping)될 수 있습니다. 이를 통해 제어기는 어떤 조합이 원하는 물체 운동에 기여하고, 어떤 조합이 내부력 부공간(subspace)에 위치하여 물체의 전체 운동을 변화시키지 않는지 결정할 수 있습니다.

내부력 조절은 취약하거나, 컴플라이언트하거나, 변형되기 쉬운 물체에 특히 중요합니다. 과도한 대립력은 포장을 파손하거나, 부품을 휘게 하거나, 판재를 변형시킬 수 있는 반면, 부족한 힘은 미끄러짐이나 접촉 상실을 유발할 수 있습니다. 따라서 목표 내부력은 물체의 질량, 마찰, 파지 기하학, 가속도, 재료 특성 및 조작 중 조우할 것으로 예상되는 외란을 고려하여 설정되어야 합니다.

력 폐쇄(force closure)는 이용 가능한 접촉들이 필요한 방향의 외란에 저항할 수 있는지를 평가하는 유용한 기준을 제공합니다. 협동 파지는 마찰 제약 조건과 구동기 한계를 준수하면서 허용 가능한 접촉력을 생성해야 합니다. 힘 제어 계획은 파지 기하학과 마찰깔때기(friction cone) 모델을 결합하여 접촉 안정성을 침해하지 않고 명령된 물체 렌치를 생성할 수 있는지 여부를 결정할 수 있습니다.

손목 근처에 장착된 힘-토크 센서는 엔드 이펙터에서의 상호작용 렌치에 대한 직접적인 측정값을 제공합니다. 이러한 측정값에는 물체의 중력, 관성력, 접촉 반력 및 내부력이 포함됩니다. 협동 제어를 위해 측정값을 정확하게 해석하려면 중력 보상, 센서 바이어스 추정, 툴 캘리브레이션 및 동적 보상이 필요합니다.

촉각 감지는 손목 힘-토크 측정값을 보완하여 국소 압력, 접촉 위치 및 미끄러짐에 대한 분산된 정보를 제공합니다. 알짜 렌치 측정값이 유사한 두 파지라 하더라도 국소 접촉 상태는 매우 다를 수 있습니다. 촉각 배열은 불균일한 하중이나 초기의 미끄러짐을 감지하여 전체적인 물체 안정성이 손상되기 전에 제어기가 파지력을 조정할 수 있도록 합니다.

강체 쌍구동 파지 동안 순수 위치 제어를 사용하는 것은 미세한 기하학적 불일치가 높은 힘을 유발할 수 있으므로 위험합니다. 두 매니퓰레이터가 높은 강성(stiffness)을 가진 채 독립적으로 지정된 카테시안(Cartesian) 궤적을 추종하려고 하면, 캘리브레이션 및 추종 오차로 인해 서로 충돌하는 힘이 발생할 수 있습니다. 따라서 협동 조작은 일반적으로 컴플라이언스를 도입하여 작은 상대적 편차가 과도한 상호작용력을 발생시키지 않고 흡수되도록 합니다.

임피던스 제어(impedance control)는 포즈 오차와 상호작용력 사이의 원하는 동적 관계를 정의합니다. 각 매니퓰레이터는 무한히 강한 위치 소스보다는 가상의 질량-스프링-댐퍼 시스템처럼 동작할 수 있습니다. 적절히 선택된 강성과 감쇠(damping)를 통해 두 팔은 협동 조작 중 작은 기하학적 오차와 외부 외란을 수용하면서 원하는 물체 궤적을 추종할 수 있습니다.

어드미턴스 제어(admittance control)는 측정된 상호작용력을 명령된 운동으로 변환함으로써 상보적인 관계를 제공합니다. 이는 하위 로봇 제어기가 위치 또는 속도 명령을 받지만 힘 응답 동작이 필요한 경우에 유용합니다. 양팔 시스템에서 측정된 접촉력은 과도한 내부 하중을 줄이거나 물체가 환경 접촉에 부응(comply)하도록 하는 교정 운동을 생성할 수 있습니다.

하이브리드 위치-힘 제어(hybrid position-force control)는 주로 운동으로 제어되어야 하는 방향과 힘으로 조절되어야 하는 방향을 분리합니다. 예를 들어 협동 삽입 작업 동안, 물체는 삽입 축을 따라 명령된 운동을 추종하는 한편 정렬을 지원하기 위해 측면 힘은 제한될 수 있습니다. 적절한 운동 및 힘 부공간을 선택하면 과도한 접촉 응력 없이 정확한 작업을 수행할 수 있습니다.

물체 수준 임피던스 제어(object-level impedance control)는 공유된 물체를 주요 제어 대상 제어로 취급합니다. 원하는 물체 궤적과 임피던스가 정의되고, 필요한 결과 렌치가 두 매니퓰레이터 사이에 분배됩니다. 이 방식은 두 팔의 명령이 독립적으로 지정된 엔드 이펙터 궤적이 아닌 공통의 물체 목표로부터 시작되므로 자연스럽게 팔들을 조정합니다.

렌치 분배(wrench distribution)는 필요한 물체 힘과 모멘트가 매니퓰레이터 사이에 어떻게 공유되는지 결정합니다. 대칭적인 파지에는 균등 분배가 적합할 수 있지만, 많은 작업은 서로 다른 팔 형상, 구동기 역량, 파지 품질 또는 접촉 기하학으로 인해 비대칭 할당을 필요로 합니다. 최적화를 통해 관절 토크를 최소화하고, 포화를 방지하며, 원하는 내부력을 유지하면서 하중을 분배할 수 있습니다.

여유 렌치 분배(redundant force distribution)는 여러 접촉 렌치 조합이 동일한 결과 물체 렌치를 생성할 수 있기 때문에 최적화의 기회를 제공합니다. 보조 목표는 에너지를 최소화하고, 구동기 노력을 균형 있게 조율하며, 마찰 여유를 극대화하고, 구조적 하중을 줄이거나, 후속 동작을 위해 한쪽 팔의 하중을 가볍게 유지할 수 있습니다. 제약 조건은 접촉 안정성과 힘 한계를 동시에 강제할 수 있습니다.

중력 보상은 협동 이송 중 필수적입니다. 결합된 매니퓰레이터들은 물체의 무게를 지지하는 동시에 가속 및 외란 억제를 위한 추가적인 힘을 생성해야 합니다. 추정된 질량이나 질량 중심이 부정확하면 팔에 편향된 하중과 의도치 않은 내부력이 발생할 수 있습니다. 온라인 추정은 물체 특성이 불확실할 때 보상 성능을 향상시킬 수 있습니다.

물체 관성은 빠른 운동 중에 점점 더 중요해집니다. 병진 및 회전 가속도는 두 접촉점을 통해 분배되어야 하는 동적 하중을 발생시킵니다. 정적 힘 균형만을 위해 설계된 제어기는 급격한 이송이나 회전 중에 성능이 저하될 수 있습니다. 동역학 모델은 관성 렌치를 예측하고 피드포워드 보상을 제공할 수 있으며, 피드백은 모델링 오차를 수정합니다.

질량 중심은 하중 공유에 강한 영향을 미칩니다. 질량 중심이 한쪽 파지점에 더 가까이 위치할 때, 해당 매니퓰레이터는 자연스럽게 무게나 토크의 더 큰 부분을 지지하게 될 수 있습니다. 이러한 경우에 대칭적 하중 분배를 가정하면 불필요한 내부력이 발생할 수 있습니다. 기하학, 이전 모델, 또는 측정된 힘 응답으로부터 질량 중심을 추정하면 보다 물리적으로 적절한 렌치 할당이 가능해집니다.

환경과의 접촉은 힘 조율의 또 다른 레이어를 도입합니다. 조립, 연마, 절단 또는 표면 추종 작업 중에 힘은 두 매니퓰레이터와 물체 사이뿐만 아니라 물체와 환경 사이에서도 발생합니다. 제어기는 안정적인 양팔 파지를 유지하면서 환경 상호작용을 조절해야 하며, 여러 동시 접촉 제약 조건을 효과적으로 조정해야 합니다.

협동 삽입은 이러한 과제를 명확히 보여줍니다. 두 팔이 큰 부품을 잡고 고정구로 안내할 수 있습니다. 물체 궤적은 조율된 상태를 유지해야 하고, 삽입력은 안전한 한계 이내로 유지되어야 하며, 측면 접촉력은 정렬 정보로 활용될 수 있습니다. 컴플라이언스는 과도한 위치 강성으로 인한 잼(jamming) 현상을 방지하면서 작은 교정 운동을 가능하게 합니다.

힘 제어는 통신 및 제어 지연(latency)도 고려해야 합니다. 한 매니퓰레이터가 다른 매니퓰레이터보다 외란에 현저히 늦게 반응하면, 일시적인 불일치로 인해 내부력 진동이 발생할 수 있습니다. 동기화된 제어 주기, 타임스탬프가 지정된 센서 데이터, 확정적 통신 및 일관된 상태 추정은 두 팔이 급격히 변하는 상호작용력에 일관되게 대응하도록 돕습니다.

제어기 대역폭은 로봇의 기계적 특성, 센서 대역폭, 통신 지연 및 접촉 강성에 따라 선택되어야 합니다. 지나치게 공격적인 힘 피드백은 노이즈를 증폭하거나 구조적 공진을 노출시킬 수 있는 반면, 매우 낮은 대역폭은 느린 외란 억제를 초래합니다. 필터링과 감쇠는 접촉 상호작용을 불안정하게 만드는 지연을 유발하지 않으면서 바람직하지 않은 진동을 줄여야 합니다.

수동성(passivity) 및 안정성 분석은 안전한 협동 제어기를 설계하기 위한 중요한 도구를 제공합니다. 단단한 물체를 통해 결합된 두 개의 능동 제어 매니퓰레이터는 각 팔을 독립적으로 분석할 때는 드러나지 않는 방식으로 에너지를 교환할 수 있습니다. 에너지 인식 제어, 수동성 관측기, 감쇠 주입 및 보수적인 임피던스 설정은 불확실한 접촉 중에 발생할 수 있는 불안정한 진동을 방지하는 데 도움이 됩니다.

안전한 한계 기준은 공목 힘 제어 목표와 독립적으로 존재해야 합니다. 최대 접촉력, 관절 토크, 내부력, 물체 렌치 및 힘 변화율 임계값은 감속, 컴플라이언스 증가, 제어된 해제 또는 비상 정지를 유발할 수 있습니다. 이러한 감독은 물체 특성, 파지 품질 또는 환경 접촉 조건이 완전히 알려지지 않았을 때 특히 중요합니다.

상태 추정은 관절 측정값, 힘-토크 감지, 촉각 신호, 물체 포즈 및 동역학 모델을 결합하여 직접 측정할 수 없는 양들을 온라인으로 추론합니다. 내부력, 물체 가속도, 질량 중심 위치, 접촉 상태 및 미끄러짐 확률은 모두 온라인으로 추정될 수 있습니다. 신뢰할 수 있는 추정치는 고정된 가정에 의존하기보다 힘 제어 정책이 적응할 수 있도록 합니다.

적응 제어는 수행 중 물체 및 접촉 특성이 밝혀짐에 따라 임피던스, 힘 기준값 또는 렌치 분배를 수정할 수 있습니다. 로봇은 초기에 보수적인 저자극 동작을 사용하고, 측정된 응답으로부터 강성이나 마찰을 추정한 다음 제어 매개변수를 조정할 수 있습니다. 이는 기계적 특성을 미리 알 수 없는 익숙하지 않은 물체를 조작할 때 유용합니다.

학습 기반 접근 방식은 시연 및 상호작용 데이터로부터 접촉 모델, 임피던스 매개변수, 하중 공유 정책 또는 교정 동작을 학습함으로써 협동 힘 제어를 더욱 향상시킬 수 있습니다. 강화 학습은 복잡한 접촉 동역학 하에서 동작을 최적화할 수 있으며, 모방 학습은 숙련된 협동 조작을 재현할 수 있습니다. 학습된 정책이 물리적 상호작용력을 명령할 때도 안전 제약 조건은 여전히 중요합니다.

시뮬레이션은 물체 질량, 마찰, 강성, 질량 중심, 파지 포즈, 센서 노이즈, 지연 및 캘리브레이션 오차의 변화 하에서 힘 제어기를 체계적으로 테스트할 수 있게 합니다. 평가를 통해 알고리즘을 하드웨어로 이전하기 전에 물체 추종 오차, 피크 내부력, 하중 공유 오차, 접촉 안정성, 에너지 소비, 진동 및 외란으로부터의 복구 능력을 측정할 수 있습니다.

물리적 검증은 준정적(quasi-static) 협동 유지에서 시작하여 이송, 회전, 외란 억제, 환경 접촉 및 동적 조작으로 진행되어야 합니다. 통제된 섭동(perturbation)을 통해 두 팔이 내부력을 조절하면서 물체 포즈를 유지하는지 테스트할 수 있습니다. 취약하거나 컴플라이언트한 테스트 물체는 현실적인 불확실성 하에서 힘 한계 및 컴플라이언스 메커니즘이 올바르게 작동하는지 확인하는 데 유용합니다.

궁극적으로 협동 양팔 힘 제어는 두 매니퓰레이터를 독립적인 위치 제어 로봇이 아닌 단일 물리 시스템의 일부로 조율합니다. 물체 렌치와 내부력을 분리하고, 컴플라이언트 제어를 통해 접촉을 조절하며, 작업 및 로봇 역량에 따라 하중을 분배하고, 힘과 촉각 피드백을 지속적으로 활용함으로써, 양팔 로봇은 공유된 물체를 안전하고 정확하며 강인하게 조작할 수 있습니다.

##  

## 08.06. Bimanual Imitation Learning from Teleoperation [w/Code]

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}

Bimanual imitation learning from teleoperation enables a robot to acquire coordinated two-arm manipulation skills from demonstrations generated by a human operator. Instead of manually programming every grasp, trajectory, synchronization rule, and contact transition, the system records how an operator performs a task and learns a policy that maps observations of the environment and robot state to coordinated actions for both manipulators.

Teleoperation is particularly valuable for bimanual learning because many two-handed tasks are difficult to specify analytically. Folding material, opening containers, manipulating cables, assembling parts, or reorienting large objects may involve subtle timing and contact strategies. Human demonstrations naturally contain coordinated behaviors such as stabilizing with one hand while manipulating with the other or changing roles during execution.

A teleoperation system requires an interface that allows the operator to command both robot arms with sufficient precision and low cognitive burden. Common interfaces include dual master manipulators, motion-tracked controllers, virtual-reality devices, exoskeletons, data gloves, and vision-based hand tracking. The interface should capture not only end-effector motion but also gripper commands, relative arm coordination, and important task events.

Mapping human or master-device motion to robot motion is not trivial because human arms and robotic manipulators differ in kinematic structure, workspace, joint limits, and dexterity. Retargeting algorithms transform operator commands into feasible robot configurations while preserving task-relevant motion. Scaling, frame transformation, inverse kinematics, collision avoidance, and redundancy resolution may all be required during demonstration collection.

Cartesian teleoperation often provides an intuitive representation because the operator directly controls the desired poses of the two end effectors. Joint-space commands can then be generated through inverse kinematics or whole-body optimization. For tightly coupled tasks, object-centered teleoperation may be more effective because the operator commands shared object motion while secondary inputs adjust relative gripper pose or internal coordination.

Bilateral teleoperation can return force or tactile information from the robot to the operator. Force feedback helps the demonstrator perceive contact, object weight, insertion resistance, and grasp stability, potentially producing higher-quality demonstrations for contact-rich tasks. However, feedback loops must remain stable despite communication latency, device dynamics, scaling, and differences between master and slave systems.

The quality of imitation learning depends strongly on the demonstration dataset. Each episode should contain synchronized observations and actions from both manipulators. Typical records include RGB or depth images, joint positions and velocities, end-effector poses, gripper states, force-torque measurements, tactile signals, object poses, timestamps, and task success information. Accurate synchronization is essential because bimanual behavior often depends on precise temporal relationships.

Time alignment is especially important when sensor streams operate at different frequencies. Cameras may run slower than robot controllers, tactile sensors may provide high-rate measurements, and teleoperation commands may arrive asynchronously. Hardware timestamps or synchronized clocks allow data to be aligned onto a common temporal reference, reducing artificial delays that could otherwise be learned as part of the manipulation policy.

Demonstrations should cover meaningful variation rather than repeatedly reproducing nearly identical trajectories. Initial object pose, orientation, grasp location, environmental arrangement, execution speed, and disturbance conditions can be varied systematically. Such diversity encourages the learned policy to associate actions with task state instead of memorizing a single motion sequence and improves generalization to unseen configurations.

Successful demonstrations alone may not fully describe robust behavior. Near-failures, corrective motions, retries, and recovery examples can teach the policy how to respond when execution deviates from the nominal trajectory. Careful labeling or filtering remains important because uncontrolled failures can introduce ambiguous or unsafe actions. Dataset design should therefore balance task diversity with demonstration consistency and quality.

Behavior cloning is the most direct imitation-learning method. A policy is trained using supervised learning to predict demonstrated robot actions from observations. For bimanual manipulation, the output may include commands for both end effectors and grippers simultaneously. Joint prediction allows the model to learn correlations between the arms, such as one arm slowing down while the other establishes contact.

Independent policies for each arm can simplify the learning problem but may lose important coordination information. A shared policy with a joint action representation can model dependencies directly. Another architecture uses separate arm encoders or action heads connected through shared latent features, preserving arm-specific processing while allowing information exchange for coordinated decisions.

Observation representations determine what information the policy can use. Proprioceptive inputs describe robot configuration, while visual observations describe objects and environmental context. Force and tactile signals become important after contact. Object-centered features can improve sample efficiency when reliable pose estimation is available, whereas end-to-end visual policies reduce dependence on manually engineered perception pipelines.

History is important because many bimanual tasks are partially observable. A single image may not reveal whether an object is securely grasped, which arm currently supports its weight, or whether sliding contact has occurred. Recurrent networks, temporal convolution, transformers, and policies operating on observation windows can infer hidden task state from sequences of visual, proprioceptive, and force measurements.

Action representation strongly influences learning behavior. Policies may predict joint positions, joint velocities, end-effector poses, pose increments, forces, or impedance parameters. Absolute pose commands provide clear geometric targets, while delta actions can represent local corrections more naturally. For contact-rich manipulation, combining motion commands with stiffness or force references can provide greater adaptability.

Action chunking predicts a short sequence of future actions instead of only the next control command. This can reduce sensitivity to noisy demonstrations and improve temporal consistency. In bimanual tasks, coordinated chunks can encode meaningful synchronized motion patterns across both arms. Receding-horizon execution allows only part of each predicted chunk to be executed before observations are updated and a new prediction is generated.

Transformer-based imitation policies are well suited to multimodal bimanual datasets because they can model long temporal dependencies and interactions among vision, proprioception, language, and actions. Attention mechanisms can associate relevant visual regions with arm states and task phases. Policies can also condition on task descriptions, enabling a common architecture to represent multiple manipulation behaviors.

Diffusion policies provide another approach for learning complex action distributions. Instead of predicting a single deterministic action, a diffusion model can generate temporally coherent action sequences conditioned on current observations. This is useful when a task has multiple valid bimanual strategies, such as alternative grasp configurations or different ways of coordinating the two arms around an obstacle.

Multimodal behavior is a fundamental challenge because demonstrations may contain several correct solutions to the same observed state. Standard regression can average incompatible actions and generate poor trajectories. Mixture models, latent-variable policies, diffusion models, or discrete skill representations can preserve alternative strategies and select a coherent mode during execution rather than blending them.

Skill segmentation can divide long demonstrations into meaningful phases such as approach, grasp, stabilize, transport, regrasp, insert, and release. Segmented learning reduces the temporal complexity of long-horizon tasks and allows specialized policies to be trained for different interaction modes. A high-level policy or state machine can then select and transition between learned bimanual skills.

Relative motion features are particularly useful for learning coordination. In addition to absolute end-effector poses, the dataset can represent the transformation between the two grippers or between each gripper and the manipulated object. These features make cooperative structure explicit and can help the policy preserve appropriate spacing, orientation, and synchronization when object position changes.

Data augmentation can improve robustness without requiring every variation to be demonstrated physically. Visual augmentation can modify lighting, texture, camera noise, or background appearance, while geometric augmentation can perturb object poses and coordinate frames when mathematically valid. Care must be taken to transform actions and spatial labels consistently so that augmented examples remain physically meaningful.

Simulation can supplement real teleoperation data by providing large numbers of demonstrations under controlled variation. Domain randomization can vary object geometry, friction, mass, camera parameters, and robot dynamics. Simulated data are inexpensive to generate but may differ from physical interaction, especially in contact and tactile behavior, so real demonstrations remain important for grounding the learned policy.

Dataset aggregation can address distribution shift between demonstrations and autonomous execution. A policy trained only on expert trajectories may encounter unfamiliar states after making small errors. Interactive approaches allow the operator to provide corrective demonstrations in states visited by the learned policy. Repeated collection and retraining gradually expand coverage around the robot\'s actual execution distribution.

Safety constraints should remain outside or alongside the learned policy. Collision checking, joint limits, velocity limits, force thresholds, workspace constraints, and emergency stopping should prevent unsafe actions even if the policy predicts an incorrect command. Learned actions can also be projected into a feasible set through optimization before they are sent to low-level controllers.

Policy execution typically operates hierarchically. The imitation policy may generate target end-effector poses or short action sequences at a moderate frequency, while low-level controllers track these commands at a higher rate. Impedance control can provide physical compliance, and force monitoring can override or modify learned actions during unexpected contact. This separation improves stability and hardware safety.

Generalization should be evaluated across variations that were not explicitly demonstrated. Tests can change object pose, object instance, orientation, clutter, lighting, initial robot configuration, or task sequence. More difficult evaluation may introduce novel objects within the same category or moderate changes in geometry. Performance should distinguish interpolation within the training distribution from genuine out-of-distribution behavior.

Evaluation metrics should capture more than binary task success. Useful measures include completion time, trajectory error, grasp success, synchronization error, peak contact force, number of corrective actions, recovery rate, collision events, and robustness across initial conditions. For learned systems, performance as a function of demonstration count also reveals data efficiency and practical collection requirements.

Failure analysis is essential because imitation policies can fail for very different reasons. Errors may originate from perception, insufficient dataset coverage, poor temporal alignment, ambiguous demonstrations, action representation, controller mismatch, or physical contact uncertainty. Logging observations, predicted actions, executed commands, forces, and task states enables failures to be traced back to specific stages of the learning and execution pipeline.

A practical development cycle therefore alternates between teleoperation, dataset inspection, policy training, simulation evaluation, constrained hardware testing, and targeted recollection. Rather than collecting an enormous dataset blindly, developers can identify failure regions and add demonstrations that specifically address them. This iterative process improves data efficiency while progressively expanding task robustness.

Ultimately, bimanual imitation learning from teleoperation converts human coordination experience into reusable robotic manipulation policies. Reliable systems combine high-quality synchronized demonstrations, task-relevant representations, temporal learning, coordinated action prediction, compliant low-level control, and independent safety mechanisms. This integration provides a practical path toward teaching robots complex two-handed skills that are difficult to program explicitly.

원격조작 기반 양팔 모방학습(Bimanual Imitation Learning from Teleoperation)은 인간 조작자가 생성한 시범(Demonstration)을 통해 로봇이 협조된 양팔 조작 기술을 습득하도록 한다. 모든 파지, 궤적, 동기화 규칙, 접촉 전환을 수동으로 프로그래밍하는 대신 시스템은 조작자가 작업을 수행하는 방식을 기록하고, 환경 및 로봇 상태에 대한 관측(Observation)을 두 매니퓰레이터의 협조된 행동(Action)으로 변환하는 정책(Policy)을 학습한다.

원격조작(Teleoperation)은 많은 양손 작업을 분석적으로 정의하기 어렵기 때문에 양팔 학습에서 특히 유용하다. 소재 접기, 용기 열기, 케이블 조작, 부품 조립, 대형 물체 재방향 설정(Reorientation)에는 미묘한 타이밍과 접촉 전략이 포함될 수 있다. 인간 시범은 한 손으로 물체를 안정화하면서 다른 손으로 조작하거나 실행 과정에서 두 손의 역할을 변경하는 것과 같은 협조 행동을 자연스럽게 포함한다.

원격조작 시스템은 조작자가 낮은 인지적 부담으로 두 로봇 팔을 충분히 정밀하게 명령할 수 있는 인터페이스(Interface)를 필요로 한다. 대표적인 인터페이스에는 듀얼 마스터 매니퓰레이터(Dual Master Manipulator), 모션 추적 컨트롤러(Motion-Tracked Controller), 가상현실 장치(Virtual-Reality Device), 외골격(Exoskeleton), 데이터 글러브(Data Glove), 비전 기반 손 추적(Vision-Based Hand Tracking)이 있다. 인터페이스는 말단장치 움직임뿐만 아니라 그리퍼 명령, 팔 사이의 상대적인 협조, 중요한 작업 이벤트까지 포착해야 한다.

인간 또는 마스터 장치의 움직임을 로봇 움직임으로 매핑하는 것은 인간 팔과 로봇 매니퓰레이터의 운동학적 구조, 작업 공간, 관절 한계, 기민성(Dexterity)이 서로 다르기 때문에 단순하지 않다. 리타게팅 알고리즘(Retargeting Algorithm)은 작업에 중요한 움직임을 보존하면서 조작자 명령을 실행 가능한 로봇 구성으로 변환한다. 시범 수집 과정에서 스케일링(Scaling), 좌표계 변환(Frame Transformation), 역운동학(Inverse Kinematics), 충돌 회피(Collision Avoidance), 여유 자유도 해소(Redundancy Resolution)가 필요할 수 있다.

직교좌표 원격조작(Cartesian Teleoperation)은 조작자가 두 말단장치의 목표 자세를 직접 제어할 수 있기 때문에 직관적인 표현을 제공한다. 이후 역운동학 또는 전신 최적화(Whole-Body Optimization)를 통해 관절 공간 명령(Joint-Space Command)을 생성할 수 있다. 강하게 결합된 작업에서는 조작자가 공유 물체의 움직임을 명령하고 보조 입력을 통해 상대 그리퍼 자세 또는 내부 협조 상태를 조절하는 물체 중심 원격조작(Object-Centered Teleoperation)이 더 효과적일 수 있다.

양방향 원격조작(Bilateral Teleoperation)은 로봇에서 측정된 힘 또는 촉각 정보를 조작자에게 다시 전달할 수 있다. 힘 피드백(Force Feedback)은 조작자가 접촉, 물체 무게, 삽입 저항, 파지 안정성을 인지하도록 하여 접촉 중심 작업(Contact-Rich Task)을 위한 고품질 시범을 생성하는 데 도움을 줄 수 있다. 그러나 피드백 루프는 통신 지연, 장치 동역학, 스케일링, 마스터-슬레이브 시스템 차이가 존재하더라도 안정적으로 유지되어야 한다.

모방학습(Imitation Learning)의 품질은 시범 데이터셋(Demonstration Dataset)의 품질에 크게 의존한다. 각 에피소드(Episode)는 두 매니퓰레이터의 동기화된 관측과 행동을 포함해야 한다. 일반적인 기록 정보에는 RGB 또는 깊이 영상, 관절 위치와 속도, 말단장치 자세, 그리퍼 상태, 힘-토크 측정, 촉각 신호, 물체 자세, 타임스탬프(Timestamp), 작업 성공 정보 등이 포함된다. 양팔 행동은 정밀한 시간적 관계에 의존하는 경우가 많기 때문에 정확한 동기화가 필수적이다.

센서 스트림이 서로 다른 주파수로 동작하는 경우 시간 정렬(Time Alignment)이 특히 중요하다. 카메라는 로봇 제어기보다 낮은 속도로 작동할 수 있고, 촉각 센서는 높은 주파수의 측정값을 제공할 수 있으며, 원격조작 명령은 비동기적으로 도착할 수 있다. 하드웨어 타임스탬프(Hardware Timestamp) 또는 동기화 시계(Synchronized Clock)를 사용하면 데이터를 공통 시간 기준으로 정렬하여 인위적인 지연이 조작 정책의 일부로 잘못 학습되는 것을 줄일 수 있다.

시범은 거의 동일한 궤적을 반복하는 대신 의미 있는 변화(Variation)를 포함해야 한다. 초기 물체 위치, 방향, 파지 위치, 환경 배치, 실행 속도, 외란 조건 등을 체계적으로 변화시킬 수 있다. 이러한 다양성은 학습 정책이 하나의 동작 순서를 암기하는 대신 행동과 작업 상태 사이의 관계를 학습하도록 하며, 경험하지 않은 구성에 대한 일반화(Generalization)를 향상시킨다.

성공적인 시범만으로는 강건한 행동을 충분히 표현하지 못할 수 있다. 거의 실패한 상황(Near-Failure), 수정 동작(Corrective Motion), 재시도, 복구 사례를 포함하면 실행이 정상 궤적에서 벗어났을 때 정책이 대응하는 방법을 학습할 수 있다. 다만 통제되지 않은 실패는 모호하거나 안전하지 않은 행동을 데이터에 포함할 수 있으므로 신중한 라벨링(Labeling)과 필터링이 필요하다. 따라서 데이터셋 설계는 작업 다양성과 시범의 일관성 및 품질 사이의 균형을 유지해야 한다.

행동 복제(Behavior Cloning)는 가장 직접적인 모방학습 방법이다. 정책은 지도학습(Supervised Learning)을 통해 관측으로부터 시범에서 수행된 로봇 행동을 예측하도록 학습된다. 양팔 조작에서는 두 말단장치와 그리퍼에 대한 명령을 동시에 출력할 수 있다. 행동을 공동으로 예측하면 한쪽 팔이 접촉을 형성하는 동안 다른 팔이 속도를 낮추는 것과 같은 두 팔 사이의 상관관계를 모델이 학습할 수 있다.

각 팔에 독립적인 정책(Independent Policy)을 사용하면 학습 문제를 단순화할 수 있지만 중요한 협조 정보를 잃을 수 있다. 공동 행동 표현(Joint Action Representation)을 사용하는 공유 정책(Shared Policy)은 두 팔 사이의 의존성을 직접 모델링할 수 있다. 또 다른 구조는 공유 잠재 특징(Shared Latent Feature)을 통해 연결된 별도의 팔 인코더(Arm Encoder) 또는 행동 헤드(Action Head)를 사용하여 팔별 처리를 유지하면서 협조 의사결정을 위한 정보를 교환하도록 한다.

관측 표현(Observation Representation)은 정책이 어떤 정보를 사용할 수 있는지를 결정한다. 고유수용감각 입력(Proprioceptive Input)은 로봇 구성을 나타내고, 시각 관측(Visual Observation)은 물체와 환경 상황을 표현한다. 힘과 촉각 신호는 접촉 이후 중요성이 증가한다. 신뢰성 높은 자세 추정이 가능하다면 물체 중심 특징(Object-Centered Feature)이 데이터 효율성을 향상시킬 수 있으며, 종단간 시각 정책(End-to-End Visual Policy)은 수작업으로 설계된 인식 파이프라인에 대한 의존성을 줄일 수 있다.

많은 양팔 작업은 부분 관측 가능(Partially Observable)하기 때문에 과거 정보(History)가 중요하다. 하나의 영상만으로는 물체가 안정적으로 파지되었는지, 현재 어느 팔이 물체의 하중을 지지하는지, 미끄럼 접촉이 발생했는지를 판단하기 어려울 수 있다. 순환 신경망(Recurrent Network), 시간 합성곱(Temporal Convolution), 트랜스포머(Transformer), 관측 윈도우(Observation Window)를 사용하는 정책은 시각, 고유수용감각, 힘 측정의 시간적 연속성을 통해 숨겨진 작업 상태를 추론할 수 있다.

행동 표현(Action Representation)은 학습된 행동에 큰 영향을 준다. 정책은 관절 위치, 관절 속도, 말단장치 자세, 자세 증분(Pose Increment), 힘 또는 임피던스 파라미터(Impedance Parameter)를 예측할 수 있다. 절대 자세 명령(Absolute Pose Command)은 명확한 기하학적 목표를 제공하고, 델타 행동(Delta Action)은 국부적인 수정 동작을 자연스럽게 표현할 수 있다. 접촉 중심 조작에서는 움직임 명령과 강성(Stiffness) 또는 힘 기준값을 결합하여 적응성을 높일 수 있다.

행동 청킹(Action Chunking)은 다음 하나의 제어 명령만 예측하는 대신 짧은 미래 행동 시퀀스를 예측한다. 이는 시범 데이터의 노이즈에 대한 민감도를 낮추고 시간적 일관성(Temporal Consistency)을 향상시킬 수 있다. 양팔 작업에서는 협조된 행동 청크가 두 팔의 의미 있는 동기화 동작 패턴을 표현할 수 있다. 이동 지평 실행(Receding-Horizon Execution)을 사용하면 예측된 청크의 일부만 실행한 후 관측을 갱신하고 새로운 행동을 다시 예측할 수 있다.

트랜스포머 기반 모방 정책(Transformer-Based Imitation Policy)은 긴 시간적 의존성과 비전, 고유수용감각, 언어, 행동 사이의 상호작용을 모델링할 수 있어 다중모달 양팔 데이터셋(Multimodal Bimanual Dataset)에 적합하다. 어텐션 메커니즘(Attention Mechanism)은 관련 시각 영역을 로봇 팔 상태 및 작업 단계와 연결할 수 있다. 정책에 작업 설명(Task Description)을 조건으로 제공하면 하나의 공통 아키텍처에서 여러 조작 행동을 표현할 수도 있다.

확산 정책(Diffusion Policy)은 복잡한 행동 분포를 학습하기 위한 또 다른 접근법을 제공한다. 하나의 결정론적 행동을 예측하는 대신 확산 모델(Diffusion Model)은 현재 관측을 조건으로 시간적으로 일관된 행동 시퀀스를 생성할 수 있다. 이는 여러 파지 구성이나 장애물 주변에서 두 팔을 협조하는 다양한 방법처럼 하나의 작업에 여러 개의 유효한 양팔 전략이 존재하는 경우 특히 유용하다.

다중모달 행동(Multimodal Behavior)은 동일한 관측 상태에서도 여러 개의 올바른 해결 방법이 존재할 수 있기 때문에 중요한 문제이다. 일반적인 회귀(Regression)는 서로 호환되지 않는 행동을 평균화하여 좋지 않은 궤적을 생성할 수 있다. 혼합 모델(Mixture Model), 잠재변수 정책(Latent-Variable Policy), 확산 모델, 이산 기술 표현(Discrete Skill Representation)은 서로 다른 전략을 유지하고 실행 과정에서 이들을 혼합하지 않고 일관된 하나의 행동 모드를 선택하도록 할 수 있다.

기술 분할(Skill Segmentation)은 긴 시범을 접근, 파지, 안정화, 운반, 재파지, 삽입, 해제와 같은 의미 있는 단계로 나눌 수 있다. 분할 학습(Segmented Learning)은 장기 작업(Long-Horizon Task)의 시간적 복잡성을 감소시키고 서로 다른 상호작용 모드에 특화된 정책을 학습할 수 있게 한다. 이후 상위 수준 정책(High-Level Policy) 또는 상태 기계(State Machine)가 학습된 양팔 기술을 선택하고 기술 사이의 전환을 관리할 수 있다.

상대 운동 특징(Relative Motion Feature)은 협조 행동 학습에 특히 유용하다. 데이터셋은 각 말단장치의 절대 자세뿐만 아니라 두 그리퍼 사이의 변환 또는 각 그리퍼와 조작 물체 사이의 변환을 표현할 수 있다. 이러한 특징은 협동 구조(Cooperative Structure)를 명시적으로 나타내며 물체 위치가 변경되더라도 정책이 적절한 간격, 방향, 동기화를 유지하는 데 도움을 줄 수 있다.

데이터 증강(Data Augmentation)은 모든 변형을 실제로 시범 보이지 않고도 강건성을 향상시킬 수 있다. 시각 증강(Visual Augmentation)은 조명, 텍스처, 카메라 노이즈, 배경의 외형을 변경할 수 있으며, 기하학적 증강(Geometric Augmentation)은 수학적으로 유효한 경우 물체 자세와 좌표계를 변화시킬 수 있다. 증강된 사례가 물리적으로 의미를 유지하도록 행동과 공간 라벨(Spatial Label)도 일관되게 변환해야 한다.

시뮬레이션(Simulation)은 제어된 변화 조건에서 많은 시범을 제공함으로써 실제 원격조작 데이터를 보완할 수 있다. 도메인 무작위화(Domain Randomization)를 통해 물체 형상, 마찰, 질량, 카메라 파라미터, 로봇 동역학 등을 변화시킬 수 있다. 시뮬레이션 데이터는 저비용으로 대량 생성할 수 있지만 실제 물리적 상호작용, 특히 접촉과 촉각 거동에서 차이가 발생할 수 있으므로 학습 정책을 현실에 연결하기 위해 실제 시범이 여전히 중요하다.

데이터셋 집계(Dataset Aggregation)는 시범과 자율 실행 사이의 분포 이동(Distribution Shift)을 해결할 수 있다. 전문가 궤적만으로 학습된 정책은 작은 오류가 발생한 이후 익숙하지 않은 상태에 진입할 수 있다. 상호작용형 접근법(Interactive Approach)을 사용하면 학습 정책이 실제로 방문한 상태에서 조작자가 수정 시범(Corrective Demonstration)을 제공할 수 있다. 반복적인 데이터 수집과 재학습을 통해 실제 로봇 실행 분포 주변의 데이터 범위를 점진적으로 확장할 수 있다.

안전 제약(Safety Constraint)은 학습 정책 외부 또는 정책과 병렬로 유지해야 한다. 충돌 검사, 관절 한계, 속도 제한, 힘 임계값, 작업 공간 제약, 비상 정지(Emergency Stop)는 정책이 잘못된 명령을 예측하더라도 위험한 행동을 방지해야 한다. 학습된 행동을 저수준 제어기로 전달하기 전에 최적화를 통해 실행 가능 집합(Feasible Set)으로 투영할 수도 있다.

정책 실행(Policy Execution)은 일반적으로 계층적인 방식으로 동작한다. 모방 정책은 중간 수준의 주파수에서 목표 말단장치 자세 또는 짧은 행동 시퀀스를 생성하고, 저수준 제어기(Low-Level Controller)는 더 높은 주파수로 이러한 명령을 추종한다. 임피던스 제어(Impedance Control)는 물리적 순응성을 제공하며, 힘 모니터링(Force Monitoring)은 예상하지 못한 접촉이 발생했을 때 학습 행동을 수정하거나 우선적으로 차단할 수 있다. 이러한 분리는 안정성과 하드웨어 안전성을 향상시킨다.

일반화(Generalization)는 시범에서 명시적으로 경험하지 않은 변화 조건을 대상으로 평가해야 한다. 물체 위치, 물체 인스턴스(Object Instance), 방향, 복잡한 주변 환경(Clutter), 조명, 초기 로봇 구성, 작업 순서를 변화시킬 수 있다. 더 어려운 평가에서는 동일한 범주에 속하지만 학습에 사용되지 않은 새로운 물체나 중간 수준의 형상 변화를 도입할 수 있다. 성능 평가에서는 학습 분포 내부의 보간(Interpolation)과 실제 분포 외 행동(Out-of-Distribution Behavior)을 구분해야 한다.

평가 지표(Evaluation Metric)는 단순한 작업 성공 여부 이상을 측정해야 한다. 유용한 지표에는 완료 시간, 궤적 오차, 파지 성공률, 동기화 오차, 최대 접촉력, 수정 행동 횟수, 복구 성공률, 충돌 이벤트, 초기 조건 변화에 대한 강건성 등이 포함된다. 학습 시스템에서는 시범 데이터의 수에 따른 성능 변화도 측정하여 데이터 효율성(Data Efficiency)과 실제 데이터 수집 요구량을 평가할 수 있다.

실패 분석(Failure Analysis)은 모방 정책이 서로 다른 원인으로 실패할 수 있기 때문에 필수적이다. 오류는 인식, 부족한 데이터셋 범위, 부정확한 시간 정렬, 모호한 시범, 행동 표현, 제어기 불일치, 물리적 접촉 불확실성에서 발생할 수 있다. 관측 정보, 예측 행동, 실제 실행 명령, 힘, 작업 상태를 기록하면 학습 및 실행 파이프라인의 어느 단계에서 실패가 발생했는지를 추적할 수 있다.

실용적인 개발 주기(Development Cycle)는 원격조작, 데이터셋 검사, 정책 학습, 시뮬레이션 평가, 제약된 실제 하드웨어 시험, 목표 지향적 재수집(Targeted Recollection)을 반복하는 형태로 구성된다. 무작정 거대한 데이터셋을 수집하는 대신 개발자는 실패가 집중되는 영역을 식별하고 이를 직접 보완하는 시범을 추가할 수 있다. 이러한 반복 과정은 데이터 효율성을 향상시키면서 작업 강건성을 점진적으로 확장한다.

궁극적으로 원격조작 기반 양팔 모방학습(Bimanual Imitation Learning from Teleoperation)은 인간의 협조 경험(Human Coordination Experience)을 재사용 가능한 로봇 조작 정책으로 변환한다. 신뢰성 높은 시스템은 고품질의 동기화된 시범, 작업에 적합한 표현, 시간적 학습(Temporal Learning), 협조 행동 예측(Coordinated Action Prediction), 순응형 저수준 제어(Compliant Low-Level Control), 독립적인 안전 메커니즘을 결합한다. 이러한 통합은 명시적인 프로그래밍으로 구현하기 어려운 복잡한 양손 기술을 로봇에게 학습시키기 위한 실용적인 경로를 제공한다.

##  

## 08.07. Bimanual Policy ACT Diffusion Architecture [w/Code]

![](images/image7.png){width="7.268055555555556in" height="7.268055555555556in"}

Bimanual manipulation policies must generate temporally coordinated actions for two robot arms while responding to visual observations, proprioception, gripper states, and contact changes. Action Chunking with Transformers (ACT) and diffusion-based policies provide two influential architectures for this problem. Both predict structured action sequences rather than treating every control instant as an isolated decision, but they model those sequences in fundamentally different ways.

A bimanual policy commonly receives synchronized observations containing one or more camera images, left and right joint states, end-effector poses, gripper states, and optionally force or tactile measurements. These signals describe the shared manipulation state. The policy produces coordinated commands for both arms, allowing correlations such as simultaneous transport, asymmetric stabilization, handover, or synchronized insertion to be represented within one action model.

The action vector should preserve the coupled nature of the task. Rather than training independent left- and right-arm policies, a joint representation can concatenate both manipulators\' joint positions, velocities, Cartesian targets, or delta poses together with gripper commands. Predicting these variables jointly allows the model to learn relative timing and geometric relationships that would be difficult to maintain with separately optimized controllers.

ACT addresses long-horizon manipulation by predicting a chunk containing multiple future robot actions from the current observation. Instead of generating only the next command, the transformer predicts a short trajectory segment for both arms. This reduces the effective number of high-level decisions required during execution and allows the model to represent meaningful coordinated motion patterns over a longer temporal interval.

An ACT architecture typically combines visual features, robot state, and optional task information into transformer tokens. Camera images are processed by a visual encoder, while proprioceptive measurements are embedded into compatible latent representations. The transformer integrates these observations with latent or query tokens and predicts an ordered sequence of future bimanual actions corresponding to a fixed action horizon.

During training, ACT can use a conditional variational formulation to represent variability within demonstrations. An encoder observes demonstrated action sequences and maps them into a latent distribution, while the policy decoder predicts action chunks conditioned on observations and latent information. This allows the model to capture variation among demonstrations rather than requiring every example of a task to follow exactly the same trajectory.

At deployment, the policy predicts an action chunk from the current observation and executes either the complete chunk or only its initial portion. Replanning before the entire chunk is consumed provides feedback and reduces sensitivity to prediction errors. This receding-horizon strategy combines temporal consistency with responsiveness, which is important when objects move or contact conditions differ from those in the demonstrations.

Temporal ensembling can further smooth ACT execution. Because consecutive policy evaluations predict overlapping future actions, commands referring to the same future time can be combined using weighted averaging. Recent predictions may receive larger weights than older ones. The resulting action sequence can reduce discontinuities between chunks and provide smoother dual-arm trajectories without requiring a separate trajectory generator.

Chunk length creates an important tradeoff. Long chunks capture extended coordination and reduce inference frequency but may become inaccurate when the environment changes unexpectedly. Short chunks react quickly but provide less temporal structure and require more frequent policy decisions. The appropriate horizon depends on task dynamics, control frequency, observation uncertainty, and the degree of physical contact involved.

Diffusion policies model robot actions as samples from a conditional distribution rather than directly regressing a single trajectory. During training, noise is progressively added to demonstrated action sequences, and a neural network learns to reverse this corruption process. At inference, the policy starts from noisy action trajectories and iteratively denoises them while conditioning on current observations to produce a coherent bimanual action sequence.

This probabilistic formulation is valuable because bimanual tasks often have multiple valid solutions. An object can sometimes be grasped from different locations, passed between arms using alternative configurations, or transported around either side of an obstacle. Direct regression may average incompatible strategies, whereas a diffusion policy can represent multiple modes and generate one internally consistent trajectory.

A diffusion-policy observation encoder can combine visual features with proprioception, object state, force information, and task conditioning. The encoded observation becomes a condition for the denoising network. The action sequence may contain both arms\' Cartesian poses, joint targets, gripper states, or other control variables, allowing the denoising process to model temporal and cross-arm dependencies jointly.

The denoising network may use temporal convolution, transformer layers, or other sequence architectures. At each diffusion step, it receives a noisy action trajectory, a diffusion-time embedding, and observation conditioning. Repeated refinement transforms the initial noise into an action sequence that resembles the distribution of successful demonstrations while remaining compatible with the observed manipulation state.

Diffusion inference is generally more computationally demanding than direct action prediction because several denoising iterations may be required for every trajectory generation cycle. Real-time deployment therefore requires careful selection of action horizon, denoising steps, network size, and inference hardware. Accelerated samplers and reduced-step diffusion can decrease latency while attempting to preserve action quality.

Both ACT and diffusion policies benefit from receding-horizon execution. The model predicts more future actions than are immediately executed, the robot applies an initial segment, and the policy then observes the updated environment before generating another sequence. This structure provides closed-loop correction while preserving the smoothness and coordination advantages of multi-step prediction.

The two approaches differ in how they represent uncertainty and multimodality. ACT provides efficient sequence prediction and can incorporate latent variables to model demonstration variation, while diffusion policies explicitly learn a complex conditional action distribution through iterative generative modeling. Diffusion can be advantageous when substantially different action trajectories are valid, whereas ACT can offer lower inference complexity and direct temporal chunk generation.

Visual representation quality strongly affects either architecture. Multi-camera systems can provide global scene context, wrist views, and close-range manipulation details. Features from several cameras may be fused before entering the policy or represented as separate tokens. Data augmentation can reduce sensitivity to lighting, background, camera noise, and moderate viewpoint changes while preserving task-relevant geometry.

Proprioception provides essential information that images alone cannot reliably recover. Joint positions, velocities, gripper states, and end-effector poses tell the policy where each manipulator is and how the arms are positioned relative to one another. Normalization and consistent coordinate conventions are important because numerical scale differences or inconsistent reference frames can make training unnecessarily difficult.

Relative representations can strengthen bimanual coordination. In addition to absolute arm states, the policy may receive relative transformations between the two end effectors or between each gripper and the manipulated object. Such features expose task-relevant geometric relationships directly and can improve generalization when the entire manipulation scene is translated or rotated within the robot workspace.

Force and tactile observations are useful for tasks where visual information cannot fully determine contact state. Insertion, connector mating, deformable-object handling, and cooperative carrying may require knowledge of contact force or slip. These modalities can be encoded as additional tokens or temporal features, allowing the policy to distinguish visually similar states that require different corrective actions.

Training data should maintain accurate temporal synchronization across cameras, robot states, actions, force sensors, and gripper measurements. Misalignment can teach the model incorrect causal relationships between observations and actions. Consistent timestamps, interpolation policies, control-rate definitions, and episode boundaries are therefore important parts of the architecture even though they occur outside the neural network itself.

Dataset normalization also requires careful design. Joint angles, Cartesian positions, rotations, gripper values, and forces have different numerical ranges. Standardization or bounded scaling prevents high-magnitude variables from dominating the training loss. Rotation representations should avoid discontinuities where possible, particularly when policies predict Cartesian orientation for both end effectors.

Loss design influences what the policy prioritizes. Position, orientation, gripper, and force errors can be weighted differently according to task importance. For ACT, reconstruction losses and latent regularization may be combined. Diffusion training typically optimizes noise or velocity prediction objectives. Auxiliary losses can encourage object-state prediction, contact recognition, or consistency between the two arms.

Hierarchical policies can reduce the difficulty of very long tasks. A high-level model selects skills such as approach, grasp, stabilize, handover, insert, or release, while ACT or diffusion policies generate detailed coordinated motion within each skill. This decomposition reduces the temporal horizon that a single policy must model and allows different control structures to be used for free-space and contact-rich phases.

Language conditioning can extend the architecture to multi-task manipulation. A text instruction such as "hold the box and insert the component" can be encoded alongside visual and robot-state observations. The policy then generates bimanual actions conditioned on both physical state and task semantics. Large-scale datasets can potentially support a common policy across many coordinated manipulation behaviors.

Learned policy outputs should not bypass robot safety mechanisms. Predicted actions can be checked against joint limits, workspace boundaries, self-collision, inter-arm collision, velocity limits, and force thresholds. Optimization-based safety filters can project commands toward feasible configurations, while low-level impedance control provides compliance when prediction errors encounter physical constraints.

Inference frequency should be separated from servo frequency. ACT or diffusion inference may operate at a moderate rate and generate target trajectories, while joint or Cartesian controllers execute them at much higher frequency. Interpolation between policy outputs and low-level feedback control can maintain smooth motion. This separation allows computationally expensive policies to coexist with fast physical stabilization loops.

Evaluation should measure both task-level performance and policy behavior. Task success, completion time, grasp reliability, synchronization error, trajectory smoothness, peak force, collision rate, recovery behavior, and inference latency provide complementary information. Comparisons between ACT and diffusion policies should use identical observations, datasets, action spaces, controllers, and evaluation conditions whenever possible.

Generalization tests should vary initial object pose, object instance, clutter, lighting, robot configuration, and required coordination pattern. Tasks involving alternative valid strategies are particularly informative for evaluating multimodal policies. Robustness can also be tested through controlled perturbations during execution to determine whether receding-horizon replanning successfully returns the system toward a valid behavior.

A practical architecture may combine the strengths of both approaches rather than treating them as mutually exclusive. ACT can provide efficient skill-level action chunks for structured behaviors, while diffusion models can handle phases with strongly multimodal trajectories. A high-level coordinator can select the appropriate policy according to task phase, uncertainty, contact state, or computational constraints.

Ultimately, ACT and diffusion architectures represent two powerful approaches to learning coordinated bimanual behavior from demonstrations. ACT emphasizes efficient temporal chunk prediction, while diffusion policies emphasize expressive multimodal action generation. Combined with synchronized multimodal observations, joint action representations, closed-loop execution, compliant control, and independent safety constraints, both provide scalable foundations for increasingly general two-arm manipulation.

양팔 조작 정책(Bimanual Manipulation Policy)은 시각 관측(Visual Observation), 고유수용감각(Proprioception), 그리퍼 상태(Gripper State), 접촉 변화(Contact Change)에 대응하면서 두 로봇 팔에 대해 시간적으로 협조된 행동을 생성해야 한다. 트랜스포머 기반 행동 청킹(Action Chunking with Transformers, ACT)과 확산 기반 정책(Diffusion-Based Policy)은 이러한 문제를 해결하기 위한 대표적인 두 가지 아키텍처이다. 두 방법 모두 각각의 제어 시점을 독립적인 의사결정으로 처리하는 대신 구조화된 행동 시퀀스(Action Sequence)를 예측하지만, 이러한 시퀀스를 모델링하는 방식에는 근본적인 차이가 있다.

양팔 정책은 일반적으로 하나 이상의 카메라 영상, 왼쪽 및 오른쪽 관절 상태, 말단장치 자세(End-Effector Pose), 그리퍼 상태, 그리고 선택적으로 힘 또는 촉각 측정값을 포함하는 동기화된 관측을 입력으로 사용한다. 이러한 신호는 공유 조작 상태(Shared Manipulation State)를 표현한다. 정책은 두 팔에 대한 협조 명령을 생성하여 동시 운반, 비대칭 안정화(Asymmetric Stabilization), 핸드오버(Handover), 동기화된 삽입(Synchronized Insertion)과 같은 상관관계를 하나의 행동 모델에서 표현할 수 있도록 한다.

행동 벡터(Action Vector)는 작업의 결합된 특성을 유지해야 한다. 서로 독립적인 왼쪽 및 오른쪽 팔 정책을 학습하는 대신 공동 표현(Joint Representation)을 사용하여 두 매니퓰레이터의 관절 위치, 속도, 직교좌표 목표(Cartesian Target), 델타 자세(Delta Pose)를 그리퍼 명령과 함께 결합할 수 있다. 이러한 변수를 공동으로 예측하면 별도로 최적화된 제어기에서는 유지하기 어려운 상대적인 타이밍과 기하학적 관계를 모델이 학습할 수 있다.

ACT는 현재 관측으로부터 여러 개의 미래 로봇 행동을 포함하는 청크(Chunk)를 예측하여 장기 조작(Long-Horizon Manipulation) 문제를 처리한다. 다음 하나의 명령만 생성하는 대신 트랜스포머(Transformer)는 두 팔에 대한 짧은 궤적 구간을 예측한다. 이를 통해 실행 과정에서 필요한 상위 수준 의사결정의 실질적인 횟수를 줄이고, 모델이 더 긴 시간 구간에 걸친 의미 있는 협조 움직임 패턴을 표현할 수 있다.

ACT 아키텍처는 일반적으로 시각 특징(Visual Feature), 로봇 상태, 선택적인 작업 정보를 트랜스포머 토큰(Transformer Token)으로 결합한다. 카메라 영상은 시각 인코더(Visual Encoder)를 통해 처리되고, 고유수용감각 측정값은 호환 가능한 잠재 표현(Latent Representation)으로 임베딩된다. 트랜스포머는 이러한 관측을 잠재 토큰(Latent Token) 또는 쿼리 토큰(Query Token)과 통합하고, 고정된 행동 지평(Action Horizon)에 대응하는 미래 양팔 행동의 순차적인 시퀀스를 예측한다.

학습 과정에서 ACT는 시범(Demonstration)에 존재하는 변동성을 표현하기 위해 조건부 변분 구조(Conditional Variational Formulation)를 사용할 수 있다. 인코더는 시범 행동 시퀀스를 관측하고 이를 잠재 분포(Latent Distribution)로 매핑하며, 정책 디코더(Policy Decoder)는 관측과 잠재 정보를 조건으로 행동 청크를 예측한다. 이를 통해 모든 작업 시범이 정확히 동일한 궤적을 따르도록 요구하지 않으면서 여러 시범 사이의 변화를 모델이 표현할 수 있다.

실제 배치(Deployment)에서는 정책이 현재 관측으로부터 행동 청크를 예측하고 전체 청크 또는 초기 일부만 실행한다. 전체 청크가 모두 소비되기 전에 재계획(Replanning)을 수행하면 피드백을 활용할 수 있으며 예측 오차에 대한 민감도를 낮출 수 있다. 이러한 이동 지평 전략(Receding-Horizon Strategy)은 시간적 일관성과 반응성을 결합하며, 물체가 이동하거나 접촉 조건이 시범 데이터와 달라지는 상황에서 특히 중요하다.

시간 앙상블링(Temporal Ensembling)을 사용하면 ACT 실행을 더욱 부드럽게 만들 수 있다. 연속적인 정책 평가에서 서로 중첩되는 미래 행동이 예측되므로 동일한 미래 시점을 대상으로 하는 명령들을 가중 평균(Weighted Averaging)을 통해 결합할 수 있다. 최근의 예측에는 이전 예측보다 높은 가중치를 부여할 수 있다. 이렇게 생성된 행동 시퀀스는 별도의 궤적 생성기(Trajectory Generator)를 사용하지 않고도 행동 청크 사이의 불연속성을 줄이고 부드러운 양팔 궤적을 제공할 수 있다.

청크 길이(Chunk Length)는 중요한 절충 관계(Tradeoff)를 형성한다. 긴 청크는 장시간의 협조 관계를 표현하고 추론 빈도(Inference Frequency)를 줄일 수 있지만 환경이 예상하지 못하게 변화할 경우 부정확해질 수 있다. 짧은 청크는 빠르게 반응할 수 있지만 시간적 구조가 감소하고 더 빈번한 정책 의사결정을 요구한다. 적절한 지평은 작업 동역학(Task Dynamics), 제어 주파수, 관측 불확실성, 물리적 접촉의 정도에 따라 달라진다.

확산 정책(Diffusion Policy)은 하나의 궤적을 직접 회귀(Regression)하는 대신 로봇 행동을 조건부 분포(Conditional Distribution)에서 생성되는 샘플로 모델링한다. 학습 과정에서는 시범 행동 시퀀스에 점진적으로 노이즈를 추가하고 신경망이 이러한 오염 과정을 역으로 복원하도록 학습한다. 추론 시에는 노이즈가 포함된 행동 궤적에서 시작하여 현재 관측을 조건으로 반복적인 디노이징(Denoising)을 수행하고 일관된 양팔 행동 시퀀스를 생성한다.

이러한 확률적 구조(Probabilistic Formulation)는 양팔 작업에 여러 개의 유효한 해결 방법이 존재하는 경우 특히 유용하다. 물체를 서로 다른 위치에서 파지하거나, 다양한 구성으로 두 팔 사이에서 전달하거나, 장애물의 어느 한쪽 방향을 선택하여 운반할 수 있다. 직접적인 회귀 방식은 서로 호환되지 않는 전략을 평균화할 수 있지만 확산 정책은 여러 행동 모드(Action Mode)를 표현하고 내부적으로 일관된 하나의 궤적을 생성할 수 있다.

확산 정책의 관측 인코더(Observation Encoder)는 시각 특징을 고유수용감각, 물체 상태, 힘 정보, 작업 조건(Task Conditioning)과 결합할 수 있다. 인코딩된 관측은 디노이징 네트워크(Denoising Network)의 조건 정보가 된다. 행동 시퀀스는 두 팔의 직교좌표 자세, 관절 목표, 그리퍼 상태 또는 다른 제어 변수를 포함할 수 있으며, 이를 통해 디노이징 과정이 시간적 의존성과 팔 사이의 상호 의존성을 공동으로 모델링할 수 있다.

디노이징 네트워크는 시간 합성곱(Temporal Convolution), 트랜스포머 계층(Transformer Layer), 또는 다른 시퀀스 아키텍처를 사용할 수 있다. 각 확산 단계(Diffusion Step)에서 네트워크는 노이즈가 포함된 행동 궤적, 확산 시간 임베딩(Diffusion-Time Embedding), 관측 조건 정보를 입력으로 받는다. 반복적인 정제 과정을 통해 초기 노이즈를 성공적인 시범 분포와 유사하면서 현재 관측된 조작 상태에 적합한 행동 시퀀스로 변환한다.

확산 추론(Diffusion Inference)은 각 궤적 생성 주기마다 여러 번의 디노이징 반복이 필요할 수 있기 때문에 일반적으로 직접 행동 예측보다 계산량이 많다. 따라서 실시간 배치를 위해서는 행동 지평, 디노이징 단계 수, 네트워크 크기, 추론 하드웨어를 신중하게 선택해야 한다. 가속 샘플러(Accelerated Sampler)와 단계 수를 줄인 확산 기법을 사용하면 행동 품질을 유지하면서 지연시간(Latency)을 줄일 수 있다.

ACT와 확산 정책 모두 이동 지평 실행(Receding-Horizon Execution)을 통해 이점을 얻을 수 있다. 모델은 즉시 실행할 행동보다 더 많은 미래 행동을 예측하고, 로봇은 그중 초기 구간만 적용한 후 갱신된 환경을 다시 관측하여 새로운 시퀀스를 생성한다. 이러한 구조는 다단계 예측이 제공하는 부드러움과 협조의 장점을 유지하면서 폐루프 수정(Closed-Loop Correction)을 가능하게 한다.

두 접근법은 불확실성(Uncertainty)과 다중모달성(Multimodality)을 표현하는 방식에서 차이가 있다. ACT는 효율적인 시퀀스 예측을 제공하며 잠재변수(Latent Variable)를 사용하여 시범의 변동성을 모델링할 수 있다. 반면 확산 정책은 반복적인 생성 모델링(Generative Modeling)을 통해 복잡한 조건부 행동 분포를 명시적으로 학습한다. 크게 다른 여러 행동 궤적이 모두 유효한 경우 확산 방식이 유리할 수 있으며, ACT는 낮은 추론 복잡도와 직접적인 시간 청크 생성 측면에서 장점을 가질 수 있다.

시각 표현(Visual Representation)의 품질은 두 아키텍처 모두의 성능에 큰 영향을 준다. 다중 카메라 시스템(Multi-Camera System)은 전체 장면의 상황 정보, 손목 시점, 근거리 조작 세부 정보를 제공할 수 있다. 여러 카메라에서 추출된 특징은 정책에 입력되기 전에 융합하거나 각각 독립적인 토큰으로 표현할 수 있다. 데이터 증강(Data Augmentation)을 사용하면 작업에 중요한 기하학적 관계를 유지하면서 조명, 배경, 카메라 노이즈, 적당한 시점 변화에 대한 민감도를 낮출 수 있다.

고유수용감각(Proprioception)은 영상만으로는 신뢰성 있게 복원하기 어려운 필수 정보를 제공한다. 관절 위치, 속도, 그리퍼 상태, 말단장치 자세를 통해 정책은 각 매니퓰레이터의 현재 위치와 두 팔 사이의 상대적인 배치를 파악할 수 있다. 정규화(Normalization)와 일관된 좌표계 규칙(Coordinate Convention)이 중요하며, 수치 크기의 차이나 서로 일치하지 않는 기준 좌표계는 학습을 불필요하게 어렵게 만들 수 있다.

상대 표현(Relative Representation)을 사용하면 양팔 협조를 더욱 강화할 수 있다. 절대적인 팔 상태 외에도 두 말단장치 사이의 상대 변환(Relative Transformation) 또는 각 그리퍼와 조작 물체 사이의 상대 변환을 정책 입력에 포함할 수 있다. 이러한 특징은 작업에 중요한 기하학적 관계를 직접 제공하며 전체 조작 장면이 로봇 작업 공간 내에서 이동하거나 회전하더라도 일반화 성능을 향상시킬 수 있다.

힘 및 촉각 관측(Force and Tactile Observation)은 시각 정보만으로 접촉 상태를 완전히 판단하기 어려운 작업에서 유용하다. 삽입, 커넥터 결합(Connector Mating), 변형 물체 조작(Deformable-Object Handling), 협동 운반(Cooperative Carrying)은 접촉력 또는 미끄러짐에 대한 정보가 필요할 수 있다. 이러한 모달리티(Modality)는 추가적인 토큰이나 시간 특징으로 인코딩하여 시각적으로는 유사하지만 서로 다른 수정 행동이 필요한 상태를 정책이 구별하도록 할 수 있다.

학습 데이터는 카메라, 로봇 상태, 행동, 힘 센서, 그리퍼 측정 사이에서 정확한 시간 동기화(Temporal Synchronization)를 유지해야 한다. 시간적 정렬 오류는 관측과 행동 사이의 잘못된 인과 관계를 모델이 학습하게 만들 수 있다. 따라서 일관된 타임스탬프, 보간 정책(Interpolation Policy), 제어 주기 정의, 에피소드 경계(Episode Boundary)는 신경망 외부에서 처리되더라도 전체 아키텍처의 중요한 구성 요소이다.

데이터셋 정규화(Dataset Normalization) 역시 신중하게 설계해야 한다. 관절 각도, 직교좌표 위치, 회전, 그리퍼 값, 힘은 서로 다른 수치 범위를 갖는다. 표준화(Standardization) 또는 제한된 스케일링(Bounded Scaling)을 적용하면 큰 값을 갖는 변수가 학습 손실을 과도하게 지배하는 것을 방지할 수 있다. 특히 두 말단장치의 직교좌표 방향을 정책이 예측하는 경우 회전 표현(Rotation Representation)은 가능한 한 불연속성을 피해야 한다.

손실 함수 설계(Loss Design)는 정책이 어떤 요소를 우선적으로 학습할 것인지에 영향을 준다. 위치, 방향, 그리퍼, 힘 오차에는 작업 중요도에 따라 서로 다른 가중치를 적용할 수 있다. ACT에서는 재구성 손실(Reconstruction Loss)과 잠재 정규화(Latent Regularization)를 결합할 수 있다. 확산 학습에서는 일반적으로 노이즈 또는 속도 예측 목적함수(Velocity Prediction Objective)를 최적화한다. 보조 손실(Auxiliary Loss)을 사용하여 물체 상태 예측, 접촉 인식, 두 팔 사이의 일관성을 추가로 학습시킬 수도 있다.

계층형 정책(Hierarchical Policy)은 매우 긴 작업의 난이도를 줄일 수 있다. 상위 수준 모델은 접근, 파지, 안정화, 핸드오버, 삽입, 해제와 같은 기술(Skill)을 선택하고, ACT 또는 확산 정책은 각 기술 내부에서 세부적인 협조 움직임을 생성한다. 이러한 분해는 하나의 정책이 모델링해야 하는 시간 지평을 줄이며 자유 공간(Free-Space) 단계와 접촉 중심 단계에 서로 다른 제어 구조를 사용할 수 있도록 한다.

언어 조건화(Language Conditioning)는 아키텍처를 다중 작업 조작(Multi-Task Manipulation)으로 확장할 수 있다. 예를 들어 "상자를 잡고 부품을 삽입하라"와 같은 텍스트 명령을 시각 및 로봇 상태 관측과 함께 인코딩할 수 있다. 정책은 물리적 상태와 작업 의미(Task Semantics)를 동시에 조건으로 사용하여 양팔 행동을 생성한다. 대규모 데이터셋을 활용하면 하나의 공통 정책으로 다양한 협조 조작 행동을 표현할 가능성이 있다.

학습된 정책의 출력은 로봇 안전 메커니즘(Robot Safety Mechanism)을 우회해서는 안 된다. 예측된 행동은 관절 한계, 작업 공간 경계, 자체 충돌(Self-Collision), 팔 사이 충돌(Inter-Arm Collision), 속도 제한, 힘 임계값을 기준으로 검사할 수 있다. 최적화 기반 안전 필터(Optimization-Based Safety Filter)는 명령을 실행 가능한 구성으로 투영할 수 있으며, 저수준 임피던스 제어(Low-Level Impedance Control)는 예측 오차가 물리적 제약과 충돌할 때 순응성을 제공한다.

추론 주파수(Inference Frequency)는 서보 주파수(Servo Frequency)와 분리해야 한다. ACT 또는 확산 추론은 중간 수준의 주파수로 동작하면서 목표 궤적을 생성하고, 관절 또는 직교좌표 제어기는 훨씬 높은 주파수로 이를 실행할 수 있다. 정책 출력 사이의 보간(Interpolation)과 저수준 피드백 제어를 통해 부드러운 움직임을 유지할 수 있다. 이러한 분리는 계산량이 큰 정책과 빠른 물리적 안정화 루프를 함께 사용할 수 있도록 한다.

평가(Evaluation)는 작업 수준 성능과 정책의 행동 특성을 모두 측정해야 한다. 작업 성공률, 완료 시간, 파지 신뢰성, 동기화 오차, 궤적 부드러움(Trajectory Smoothness), 최대 힘, 충돌률, 복구 행동, 추론 지연시간은 서로 보완적인 정보를 제공한다. ACT와 확산 정책을 비교할 때는 가능한 한 동일한 관측, 데이터셋, 행동 공간, 제어기, 평가 조건을 사용해야 한다.

일반화 시험(Generalization Test)에서는 초기 물체 자세, 물체 인스턴스, 복잡한 주변 환경(Clutter), 조명, 로봇 초기 구성, 요구되는 협조 패턴을 변화시켜야 한다. 여러 개의 유효한 전략이 존재하는 작업은 다중모달 정책을 평가하는 데 특히 유용하다. 실행 중 제어된 외란(Perturbation)을 가하여 이동 지평 재계획이 시스템을 다시 유효한 행동으로 복귀시킬 수 있는지도 평가할 수 있다.

실용적인 아키텍처에서는 두 접근법을 상호 배타적으로 취급하지 않고 각각의 장점을 결합할 수도 있다. ACT는 구조화된 행동에 대해 효율적인 기술 수준 행동 청크(Skill-Level Action Chunk)를 제공하고, 확산 모델은 강한 다중모달 궤적을 갖는 단계에서 활용할 수 있다. 상위 수준 협조기(High-Level Coordinator)는 작업 단계, 불확실성, 접촉 상태, 계산 자원 제약에 따라 적절한 정책을 선택할 수 있다.

궁극적으로 ACT와 확산 아키텍처(ACT and Diffusion Architecture)는 시범으로부터 협조된 양팔 행동을 학습하기 위한 강력한 두 가지 접근법을 제공한다. ACT는 효율적인 시간 행동 청크 예측(Temporal Action Chunk Prediction)에 중점을 두며, 확산 정책은 표현력이 높은 다중모달 행동 생성(Multimodal Action Generation)에 중점을 둔다. 동기화된 다중모달 관측, 공동 행동 표현, 폐루프 실행(Closed-Loop Execution), 순응 제어(Compliant Control), 독립적인 안전 제약을 결합하면 두 방식 모두 점점 더 범용적인 양팔 조작을 위한 확장 가능한 기반을 제공할 수 있다.

##  

## 08.08. Whole Body Bimanual Humanoid Manipulation [w/Code]

![](images/image8.png){width="7.268055555555556in" height="7.268055555555556in"}

Whole-body bimanual humanoid manipulation coordinates both arms with the torso, waist, legs, and balance system so that manipulation is treated as a unified body-level task. Unlike fixed-base dual-arm robots, a humanoid must preserve dynamic stability while reaching, grasping, carrying, pushing, or assembling objects. Manipulation commands therefore interact directly with posture, support contacts, center of mass, and locomotion.

A humanoid configuration can be represented by a generalized state containing floating-base pose, leg joints, torso and waist joints, both arms, hands, and sometimes the head. This high-dimensional structure provides substantial redundancy. The robot can achieve the same hand pose through different combinations of arm extension, torso rotation, waist motion, stepping, and body translation, allowing whole-body optimization to select configurations suited to the task.

The floating base fundamentally changes the manipulation problem because the robot is not rigidly attached to the environment. Forces generated at the hands propagate through the body and must ultimately be balanced by contacts at the feet or other support surfaces. A manipulation planner must therefore consider not only whether the hands can reach a target but whether the resulting forces and postures can be physically supported without losing balance.

Support geometry provides a basic representation of stability. During quasi-static manipulation, the projected center of mass should remain within an appropriate support region formed by the feet. When the robot pushes, pulls, or carries an object, external forces shift the effective balance condition. Maintaining sufficient stability margin allows the humanoid to tolerate modeling errors, disturbances, and changes in object load.

Dynamic tasks require richer models such as the zero moment point, centroidal momentum, and contact wrench constraints. Rapid arm movement can generate reaction forces that disturb the torso and feet even when the manipulated object is lightweight. Whole-body control therefore coordinates manipulation motion with changes in center-of-mass trajectory and angular momentum rather than treating the upper body as dynamically independent from the legs.

Centroidal dynamics provides a useful abstraction for connecting manipulation and balance. Instead of modeling every joint interaction at the planning level, the controller reasons about the robot\'s total linear and angular momentum and the external wrenches acting through hands and feet. Desired hand forces can then be evaluated according to whether available support contacts can generate compensating ground reaction forces.

Bimanual tasks introduce additional coupling because both hands may constrain the same object. The two arms can create a closed kinematic chain through a rigid grasp, while the lower body simultaneously forms contact constraints with the ground. The resulting system contains multiple interconnected contact loops, making independent joint control inappropriate for tasks involving substantial object forces or precise cooperative manipulation.

Object-centered control simplifies this coordination by defining the desired pose, trajectory, or wrench of the shared object first. Grasp transformations determine corresponding targets for the left and right hands. The whole-body controller then distributes the required motion across arms, torso, waist, and legs while satisfying balance and contact constraints, allowing the object rather than individual joints to become the primary task variable.

Relative hand motion remains important even when object-centered control is used. During rigid transport, the relative transformation between the hands should remain nearly constant. During regrasping, folding, opening, or assembly, the hands may intentionally change their relative configuration. Whole-body task representations can therefore separate absolute object motion from relative bimanual motion and assign different priorities to each component.

Hierarchical whole-body control organizes competing objectives according to task importance. Balance and contact feasibility generally receive high priority, followed by object or hand motion, collision avoidance, posture regulation, and secondary optimization goals. Null-space methods or hierarchical quadratic programming can preserve high-priority constraints while using remaining degrees of freedom to improve manipulability or comfort.

Quadratic programming provides a practical framework for computing whole-body commands under multiple constraints. Optimization variables may include joint accelerations, torques, contact forces, and object wrenches. Equality constraints enforce robot dynamics and contact consistency, while inequalities represent friction limits, torque bounds, joint limits, collision margins, and support constraints. The resulting solution coordinates the complete body at each control cycle.

Task-space inverse dynamics extends this approach by converting desired Cartesian accelerations or forces into dynamically consistent whole-body commands. Hand trajectories, torso orientation, center-of-mass motion, and foot contacts can be represented simultaneously. Dynamic consistency prevents secondary posture objectives from interfering excessively with higher-priority manipulation and balance tasks.

Redundancy resolution is particularly valuable for humanoids because many body configurations can realize the same bimanual task. A robot reaching toward a distant object can extend its arms, lean the torso, rotate the waist, shift the pelvis, or step closer. Optimization can select among these alternatives according to energy, stability margin, joint-limit distance, manipulability, visibility, or expected future motion.

Manipulability should be evaluated at the whole-body level rather than only for each arm. A hand configuration that appears near an arm singularity may remain highly usable if torso or base motion can compensate. Conversely, both arms may individually have good manipulability while the complete body is poorly positioned for subsequent movement. Whole-body measures can better capture available task-space mobility.

Locomotion and manipulation become inseparable when the target lies outside the current reachable workspace. The humanoid can reposition its feet, walk while carrying an object, or take corrective steps during manipulation. Planning must determine when continuous upper-body adjustment is sufficient and when changing the support configuration through stepping provides a safer or more efficient solution.

Reachability maps can support this decision by estimating where bimanual tasks can be executed from candidate body or foot placements. Instead of checking only whether each hand can reach a point, the map can include orientation capability, shared workspace, collision feasibility, balance margin, and manipulability. Footstep planning can then choose positions that create favorable whole-body manipulation configurations.

Manipulation while walking introduces significant dynamic coupling. A carried object changes total mass distribution and may shift the combined center of mass away from the nominal humanoid model. Arm movements and object oscillation can disturb gait, while footsteps change the reference frame for the hands. Coordinated locomotion-manipulation control must therefore update object, body, and contact states continuously.

Heavy-object handling requires explicit load-aware control. Object mass and center of mass influence arm torques, torso posture, ground reaction forces, and feasible acceleration. The humanoid may widen its stance, bend its knees, bring the object closer to the torso, or distribute load asymmetrically between the arms to remain within actuator and stability limits.

External manipulation forces can also be exploited strategically. When pushing a cart or leaning against a rigid structure, hand contacts may become additional support contacts rather than merely task interactions. Multi-contact control can use feet, hands, knees, or other body surfaces to enlarge the feasible wrench region and stabilize tasks that would otherwise exceed the capabilities of a standing configuration.

Contact planning determines which body surfaces should interact with the environment and when those contacts should change. A humanoid may grasp an object with both hands, release one hand to establish environmental support, step to a new position, and then restore the bimanual grasp. Such contact transitions must preserve balance and object stability while avoiding discontinuities in force distribution.

Friction constraints are essential at every support and manipulation contact. Foot contacts must generate ground reaction forces inside feasible friction regions, while hand contacts must maintain stable grasps without slipping. Whole-body optimization can represent these limits simultaneously so that an aggressive manipulation command is reduced or modified before it causes foot slip or grasp failure.

Collision avoidance becomes more difficult as the entire body participates in manipulation. The arms may collide with the torso or each other, the knees may approach furniture, and carried objects may intersect the robot or environment. Whole-body planners require geometric models of the robot, manipulated object, and surroundings while preserving sufficient clearance for both current and future motion.

Perception must support both manipulation and body placement. Cameras and depth sensors estimate object pose, grasp regions, obstacles, support surfaces, and free space for stepping. Head and torso motion can actively improve visibility, but these motions also affect balance and manipulation geometry. Active perception can therefore be incorporated as a secondary whole-body objective when visual uncertainty limits task execution.

State estimation must provide a consistent representation of floating-base motion, joint states, foot contacts, hand contacts, object pose, and external forces. Inertial measurement units, joint encoders, cameras, force-torque sensors, and tactile sensors can be fused to estimate the complete interaction state. Errors in base orientation or contact state can directly degrade both manipulation accuracy and balance control.

Compliance is necessary when the humanoid interacts with uncertain objects or environments. High-stiffness tracking across the whole body can amplify small geometric errors into large forces. Impedance control at the hands, combined with compliant whole-body behavior, allows the robot to absorb alignment errors and disturbances while maintaining sufficient posture stiffness to remain stable.

Force distribution should consider both hand and foot contacts. A desired object wrench produces reaction forces at the hands, which must be balanced through the lower body and ground contacts. Optimization can distribute these forces according to friction margins, actuator capability, posture, and stability. This creates a direct connection between cooperative bimanual force control and whole-body balance regulation.

Predictive control can improve coordination by considering future body and object states rather than reacting only to current errors. Model predictive control can optimize center-of-mass motion, footsteps, hand trajectories, and contact forces across a finite horizon. Anticipating future manipulation loads allows the humanoid to shift posture or reposition its feet before stability becomes critical.

Learning-based methods can complement model-based whole-body control. Imitation learning can capture human-like coordination between reaching, torso motion, stepping, and bimanual manipulation, while reinforcement learning can discover robust strategies under complex dynamics. Learned policies may generate reference motions or high-level decisions while model-based controllers enforce contact, torque, balance, and safety constraints.

Simulation is essential because whole-body manipulation combines high-dimensional motion with potentially dangerous physical interaction. Randomization of object mass, friction, contact stiffness, terrain, sensor noise, actuator delay, and external disturbances can expose controller weaknesses. Simulation also allows falls, collisions, and failed grasps to be studied without damaging hardware or manipulated objects.

Sim-to-real transfer requires accurate treatment of contact dynamics and actuator behavior. Policies that rely on unrealistic friction or instantaneous torque response may fail on hardware. System identification, actuator modeling, latency randomization, compliant control, and conservative safety margins reduce this gap. Hardware validation should progressively increase object load, motion speed, environmental complexity, and contact uncertainty.

Evaluation should measure both manipulation performance and whole-body stability. Relevant metrics include task success, object pose error, hand synchronization, internal force, center-of-mass margin, foot slip, contact-force limits, joint torque utilization, energy consumption, number of corrective steps, collision events, and recovery success. A successful grasp alone is insufficient if the robot reaches it through an unstable posture.

Disturbance recovery is a defining capability of whole-body humanoid manipulation. Unexpected object motion, human contact, grasp slip, or modeling error can threaten balance. The controller may first adjust arm compliance and center-of-mass motion, then modify foot forces, and finally take a recovery step if necessary. Manipulation should degrade gracefully rather than preventing balance recovery.

Ultimately, whole-body bimanual humanoid manipulation treats the humanoid, shared object, and environment as one coupled physical system. Reliable behavior emerges from coordinating object motion, relative hand constraints, posture, balance, contact forces, stepping, perception, and compliance. Integrating these elements enables humanoid robots to perform two-handed tasks beyond fixed reach while maintaining physical feasibility, stability, and adaptability.

전신 양팔 휴머노이드 조작(Whole-Body Bimanual Humanoid Manipulation)은 두 팔을 몸통(Torso), 허리(Waist), 다리(Legs), 균형 시스템(Balance System)과 협조시켜 조작을 하나의 통합된 신체 수준 작업(Body-Level Task)으로 처리한다. 고정 베이스 양팔 로봇과 달리 휴머노이드는 물체에 접근하고, 파지하고, 운반하고, 밀거나 조립하는 동안 동적 안정성(Dynamic Stability)을 유지해야 한다. 따라서 조작 명령은 자세(Posture), 지지 접촉(Support Contact), 질량중심(Center of Mass), 보행(Locomotion)과 직접적으로 상호작용한다.

휴머노이드의 구성(Configuration)은 부유 베이스 자세(Floating-Base Pose), 다리 관절, 몸통과 허리 관절, 두 팔, 손, 경우에 따라 머리를 포함하는 일반화 상태(Generalized State)로 표현할 수 있다. 이러한 고차원 구조는 상당한 여유 자유도(Redundancy)를 제공한다. 로봇은 팔의 신장, 몸통 회전, 허리 움직임, 스텝(Stepping), 신체 병진 이동을 서로 다르게 조합하여 동일한 손 자세를 구현할 수 있으며, 전신 최적화(Whole-Body Optimization)를 통해 작업에 적합한 구성을 선택할 수 있다.

부유 베이스(Floating Base)는 로봇이 환경에 강체로 고정되어 있지 않기 때문에 조작 문제의 성격을 근본적으로 변화시킨다. 손에서 발생하는 힘은 신체 전체를 통해 전달되며 최종적으로 발 또는 다른 지지면과의 접촉을 통해 균형을 이루어야 한다. 따라서 조작 계획기(Manipulation Planner)는 손이 목표 위치에 도달할 수 있는지만 판단하는 것이 아니라, 그 결과 발생하는 힘과 자세를 균형을 잃지 않고 물리적으로 지지할 수 있는지도 고려해야 한다.

지지 기하 구조(Support Geometry)는 안정성을 표현하는 기본적인 방법을 제공한다. 준정적 조작(Quasi-Static Manipulation)에서는 투영된 질량중심이 발에 의해 형성되는 적절한 지지 영역(Support Region) 내부에 유지되어야 한다. 로봇이 물체를 밀거나 당기거나 운반하면 외력이 유효 균형 조건을 변화시킨다. 충분한 안정성 여유(Stability Margin)를 유지하면 모델링 오차, 외란(Disturbance), 물체 하중 변화에 대한 휴머노이드의 강건성을 높일 수 있다.

동적 작업(Dynamic Task)에서는 영모멘트점(Zero Moment Point), 중심동역학 운동량(Centroidal Momentum), 접촉 렌치 제약(Contact Wrench Constraint)과 같은 더욱 풍부한 모델이 필요하다. 조작 물체가 가볍더라도 빠른 팔 움직임은 몸통과 발을 교란하는 반작용력을 발생시킬 수 있다. 따라서 전신 제어(Whole-Body Control)는 상체를 다리와 동역학적으로 독립된 시스템으로 취급하지 않고, 조작 움직임을 질량중심 궤적과 각운동량(Angular Momentum)의 변화에 맞추어 협조한다.

중심동역학(Centroidal Dynamics)은 조작과 균형을 연결하는 유용한 추상화 방법을 제공한다. 계획 수준에서 모든 관절의 상호작용을 직접 모델링하는 대신 제어기는 로봇 전체의 선운동량(Linear Momentum), 각운동량, 손과 발을 통해 작용하는 외부 렌치(External Wrench)를 고려한다. 이를 통해 원하는 손의 힘이 현재의 지지 접촉에서 생성 가능한 보상 지면반력(Ground Reaction Force)으로 균형을 이룰 수 있는지 평가할 수 있다.

양팔 작업(Bimanual Task)은 두 손이 동일한 물체를 구속할 수 있기 때문에 추가적인 결합 관계를 발생시킨다. 두 팔은 강체 파지(Rigid Grasp)를 통해 폐쇄 운동학 체인(Closed Kinematic Chain)을 형성할 수 있으며, 동시에 하체는 지면과 접촉 제약(Contact Constraint)을 형성한다. 그 결과 시스템에는 서로 연결된 여러 접촉 루프가 존재하게 되므로 상당한 물체 힘이나 정밀한 협동 조작이 필요한 작업에서는 독립적인 관절 제어가 적합하지 않다.

물체 중심 제어(Object-Centered Control)는 먼저 공유 물체의 목표 자세, 궤적 또는 렌치(Wrench)를 정의하여 이러한 협조 문제를 단순화한다. 파지 변환(Grasp Transformation)을 통해 왼손과 오른손에 대응하는 목표를 결정한다. 이후 전신 제어기는 균형과 접촉 제약을 만족하면서 필요한 움직임을 팔, 몸통, 허리, 다리에 분배하며, 개별 관절이 아니라 물체가 주요 작업 변수(Task Variable)가 되도록 한다.

물체 중심 제어를 사용하더라도 상대 손 움직임(Relative Hand Motion)은 여전히 중요하다. 강체 운반에서는 두 손 사이의 상대 변환(Relative Transformation)이 거의 일정하게 유지되어야 한다. 반면 재파지(Regrasping), 접기(Folding), 열기(Opening), 조립에서는 두 손의 상대 구성을 의도적으로 변경할 수 있다. 따라서 전신 작업 표현(Whole-Body Task Representation)은 절대 물체 운동과 상대 양팔 운동을 분리하고 각 요소에 서로 다른 우선순위를 부여할 수 있다.

계층형 전신 제어(Hierarchical Whole-Body Control)는 서로 경쟁하는 목표를 작업 중요도에 따라 구성한다. 일반적으로 균형과 접촉 실현 가능성(Contact Feasibility)에 높은 우선순위를 부여하고, 그다음으로 물체 또는 손의 움직임, 충돌 회피(Collision Avoidance), 자세 조절(Posture Regulation), 보조 최적화 목표를 배치한다. 널 공간 방법(Null-Space Method) 또는 계층형 이차계획법(Hierarchical Quadratic Programming)을 사용하면 높은 우선순위 제약을 유지하면서 남은 자유도를 조작성(Manipulability)이나 자세 편의성 향상에 활용할 수 있다.

이차계획법(Quadratic Programming)은 여러 제약조건에서 전신 명령을 계산하기 위한 실용적인 프레임워크를 제공한다. 최적화 변수에는 관절 가속도, 토크, 접촉력(Contact Force), 물체 렌치(Object Wrench)가 포함될 수 있다. 등식 제약(Equality Constraint)은 로봇 동역학과 접촉 일관성을 강제하고, 부등식 제약(Inequality Constraint)은 마찰 한계, 토크 범위, 관절 한계, 충돌 여유, 지지 조건을 표현한다. 이를 통해 각 제어 주기마다 신체 전체를 협조하는 해를 계산할 수 있다.

작업 공간 역동역학(Task-Space Inverse Dynamics)은 원하는 직교좌표 가속도 또는 힘을 동역학적으로 일관된 전신 명령으로 변환하여 이러한 접근법을 확장한다. 손 궤적, 몸통 방향, 질량중심 움직임, 발 접촉을 동시에 표현할 수 있다. 동역학적 일관성(Dynamic Consistency)을 유지하면 보조 자세 목표가 더 높은 우선순위를 갖는 조작 및 균형 작업을 과도하게 방해하는 것을 방지할 수 있다.

여유 자유도 해소(Redundancy Resolution)는 동일한 양팔 작업을 구현할 수 있는 다양한 신체 구성이 존재하기 때문에 휴머노이드에서 특히 중요하다. 멀리 떨어진 물체에 접근할 때 로봇은 팔을 뻗거나, 몸통을 기울이고, 허리를 회전시키거나, 골반을 이동하고, 목표에 더 가까이 스텝을 이동할 수 있다. 최적화는 에너지, 안정성 여유, 관절 한계와의 거리, 조작성, 가시성(Visibility), 향후 예상 움직임을 기준으로 이러한 대안 중 하나를 선택할 수 있다.

조작성(Manipulability)은 각 팔에 대해서만 평가하는 것이 아니라 전신 수준에서 평가해야 한다. 팔만 고려하면 특이점(Singularity)에 가까운 손 구성이라도 몸통이나 베이스 움직임을 통해 보상할 수 있다면 충분히 유용할 수 있다. 반대로 두 팔이 개별적으로 높은 조작성을 갖더라도 전체 신체가 이후 움직임에 부적절한 자세일 수 있다. 전신 조작성 지표는 사용 가능한 작업 공간 운동 능력을 더욱 정확하게 표현할 수 있다.

목표가 현재의 도달 가능 작업 공간(Reachable Workspace) 밖에 위치하면 보행과 조작은 서로 분리할 수 없게 된다. 휴머노이드는 발의 위치를 변경하거나, 물체를 운반하면서 걷거나, 조작 도중 균형 회복을 위한 스텝을 수행할 수 있다. 계획 과정에서는 상체의 연속적인 조정만으로 충분한지 또는 스텝을 통해 지지 구성을 변경하는 것이 더 안전하고 효율적인지를 결정해야 한다.

도달 가능성 지도(Reachability Map)는 후보 신체 자세 또는 발 위치에서 양팔 작업을 수행할 수 있는지를 추정하여 이러한 결정을 지원할 수 있다. 단순히 각 손이 특정 지점에 도달할 수 있는지만 확인하는 대신 방향 구현 능력, 공유 작업 공간(Shared Workspace), 충돌 실현 가능성, 균형 여유, 조작성을 함께 포함할 수 있다. 이후 보행 계획(Footstep Planning)은 전신 조작에 유리한 구성을 형성하는 발 위치를 선택할 수 있다.

보행 중 조작(Manipulation While Walking)은 상당한 동역학적 결합(Dynamic Coupling)을 발생시킨다. 운반하는 물체는 전체 질량 분포를 변화시키고 결합된 질량중심을 기존 휴머노이드 모델의 기준 위치에서 이동시킬 수 있다. 팔 움직임과 물체의 진동은 보행을 교란할 수 있으며, 발걸음은 손의 기준 좌표계를 변화시킨다. 따라서 협조된 보행-조작 제어(Coordinated Locomotion-Manipulation Control)는 물체, 신체, 접촉 상태를 지속적으로 갱신해야 한다.

중량 물체 조작(Heavy-Object Handling)에서는 하중을 명시적으로 고려하는 제어가 필요하다. 물체 질량과 질량중심은 팔 토크, 몸통 자세, 지면반력, 실행 가능한 가속도에 영향을 준다. 휴머노이드는 액추에이터(Actuator)와 안정성 한계 내에서 동작하기 위해 보폭을 넓히고, 무릎을 굽히고, 물체를 몸통 가까이 가져오거나, 두 팔 사이에 하중을 비대칭적으로 분배할 수 있다.

외부 조작력(External Manipulation Force)은 전략적으로 활용할 수도 있다. 카트를 밀거나 강체 구조물에 기대는 경우 손 접촉은 단순한 작업 상호작용이 아니라 추가적인 지지 접촉(Additional Support Contact)이 될 수 있다. 다중 접촉 제어(Multi-Contact Control)는 발, 손, 무릎 또는 다른 신체 표면을 활용하여 실행 가능한 렌치 영역(Feasible Wrench Region)을 확장하고 일반적인 직립 자세에서는 수행하기 어려운 작업을 안정화할 수 있다.

접촉 계획(Contact Planning)은 어떤 신체 표면이 환경과 상호작용해야 하는지와 해당 접촉을 언제 변경할지를 결정한다. 휴머노이드는 두 손으로 물체를 파지한 후 한 손을 해제하여 환경 지지를 확보하고, 새로운 위치로 스텝을 이동한 다음 다시 양팔 파지를 형성할 수 있다. 이러한 접촉 전환(Contact Transition)은 힘 분배의 불연속을 방지하면서 균형과 물체 안정성을 유지해야 한다.

마찰 제약(Friction Constraint)은 모든 지지 및 조작 접촉에서 필수적이다. 발 접촉은 실행 가능한 마찰 영역 내에서 지면반력을 생성해야 하며, 손 접촉은 미끄러지지 않는 안정적인 파지를 유지해야 한다. 전신 최적화는 이러한 한계를 동시에 표현하여 공격적인 조작 명령이 발의 미끄러짐(Foot Slip)이나 파지 실패를 발생시키기 전에 명령을 감소시키거나 수정할 수 있다.

신체 전체가 조작에 참여하면 충돌 회피(Collision Avoidance)는 더욱 어려워진다. 팔은 몸통 또는 서로 충돌할 수 있고, 무릎이 가구에 접근할 수 있으며, 운반 물체가 로봇이나 주변 환경과 충돌할 수 있다. 전신 계획기는 현재 동작뿐만 아니라 이후 동작에 필요한 충분한 여유 공간을 유지하면서 로봇, 조작 물체, 주변 환경의 기하학적 모델을 함께 고려해야 한다.

인식(Perception)은 조작뿐만 아니라 신체 배치(Body Placement)까지 지원해야 한다. 카메라와 깊이 센서는 물체 자세, 파지 영역, 장애물, 지지면, 스텝을 위한 자유 공간을 추정한다. 머리와 몸통 움직임을 통해 능동적으로 가시성을 향상시킬 수 있지만 이러한 움직임 역시 균형과 조작 기하 관계에 영향을 준다. 따라서 시각적 불확실성이 작업 수행을 제한하는 경우 능동 인식(Active Perception)을 보조 전신 목표로 포함할 수 있다.

상태 추정(State Estimation)은 부유 베이스 움직임, 관절 상태, 발 접촉, 손 접촉, 물체 자세, 외력을 일관된 표현으로 제공해야 한다. 관성측정장치(Inertial Measurement Unit), 관절 엔코더(Joint Encoder), 카메라, 힘-토크 센서(Force-Torque Sensor), 촉각 센서를 융합하여 전체 상호작용 상태를 추정할 수 있다. 베이스 방향 또는 접촉 상태의 오차는 조작 정확도와 균형 제어 모두를 직접적으로 저하시킬 수 있다.

휴머노이드가 불확실한 물체 또는 환경과 상호작용할 때는 순응성(Compliance)이 필요하다. 신체 전체에 걸친 높은 강성 추종(High-Stiffness Tracking)은 작은 기하학적 오차를 큰 힘으로 증폭시킬 수 있다. 손의 임피던스 제어(Impedance Control)와 순응형 전신 행동(Compliant Whole-Body Behavior)을 결합하면 로봇이 정렬 오차와 외란을 흡수하면서 안정성을 유지하는 데 필요한 충분한 자세 강성을 확보할 수 있다.

힘 분배(Force Distribution)는 손과 발 접촉을 모두 고려해야 한다. 원하는 물체 렌치는 손에서 반작용력을 발생시키며, 이러한 힘은 하체와 지면 접촉을 통해 균형을 이루어야 한다. 최적화는 마찰 여유(Friction Margin), 액추에이터 능력, 자세, 안정성을 기준으로 이러한 힘을 분배할 수 있다. 이를 통해 협동 양팔 힘 제어(Cooperative Bimanual Force Control)와 전신 균형 조절(Whole-Body Balance Regulation)이 직접적으로 연결된다.

예측 제어(Predictive Control)는 현재 오차에만 반응하는 대신 미래의 신체 및 물체 상태를 고려하여 협조 성능을 향상시킬 수 있다. 모델 예측 제어(Model Predictive Control)는 제한된 시간 지평(Finite Horizon)에 걸쳐 질량중심 움직임, 발걸음, 손 궤적, 접촉력을 최적화할 수 있다. 미래의 조작 하중을 사전에 예측하면 안정성이 위험한 수준에 도달하기 전에 휴머노이드가 자세를 이동하거나 발 위치를 조정할 수 있다.

학습 기반 방법(Learning-Based Method)은 모델 기반 전신 제어(Model-Based Whole-Body Control)를 보완할 수 있다. 모방학습(Imitation Learning)은 접근, 몸통 움직임, 스텝, 양팔 조작 사이의 인간과 유사한 협조 관계를 학습할 수 있으며, 강화학습(Reinforcement Learning)은 복잡한 동역학 조건에서 강건한 전략을 발견할 수 있다. 학습 정책은 기준 움직임 또는 상위 수준 결정을 생성하고, 모델 기반 제어기는 접촉, 토크, 균형, 안전 제약을 강제하는 구조로 구성할 수 있다.

시뮬레이션(Simulation)은 전신 조작이 고차원 움직임과 잠재적으로 위험한 물리적 상호작용을 결합하기 때문에 필수적이다. 물체 질량, 마찰, 접촉 강성(Contact Stiffness), 지형, 센서 노이즈, 액추에이터 지연, 외부 외란을 무작위화하여 제어기의 약점을 확인할 수 있다. 또한 하드웨어나 조작 물체를 손상시키지 않고 넘어짐, 충돌, 파지 실패를 분석할 수 있다.

시뮬레이션-실환경 전이(Sim-to-Real Transfer)에서는 접촉 동역학과 액추에이터 거동을 정확하게 처리해야 한다. 비현실적인 마찰이나 즉각적인 토크 응답에 의존하는 정책은 실제 하드웨어에서 실패할 수 있다. 시스템 식별(System Identification), 액추에이터 모델링, 지연 무작위화(Latency Randomization), 순응 제어, 보수적인 안전 여유를 통해 이러한 차이를 줄일 수 있다. 실제 하드웨어 검증에서는 물체 하중, 움직임 속도, 환경 복잡도, 접촉 불확실성을 점진적으로 증가시켜야 한다.

평가(Evaluation)는 조작 성능과 전신 안정성을 모두 측정해야 한다. 관련 지표에는 작업 성공률, 물체 자세 오차, 손 동기화, 내부 힘(Internal Force), 질량중심 여유, 발 미끄러짐, 접촉력 한계, 관절 토크 사용률, 에너지 소비, 보정 스텝 횟수, 충돌 이벤트, 복구 성공률 등이 포함된다. 불안정한 자세를 통해 파지에 성공했다면 단순히 파지 성공만으로 전체 작업이 성공했다고 평가하기에는 충분하지 않다.

외란 복구(Disturbance Recovery)는 전신 휴머노이드 조작을 정의하는 핵심 능력이다. 예상하지 못한 물체 움직임, 사람과의 접촉, 파지 미끄러짐, 모델링 오차는 균형을 위협할 수 있다. 제어기는 먼저 팔의 순응성과 질량중심 움직임을 조절하고, 이후 발의 힘을 수정하며, 필요한 경우 최종적으로 균형 회복 스텝(Recovery Step)을 수행할 수 있다. 조작 작업은 균형 회복을 방해하는 것이 아니라 상황에 따라 점진적으로 성능을 낮추면서 안정성을 우선해야 한다.

궁극적으로 전신 양팔 휴머노이드 조작(Whole-Body Bimanual Humanoid Manipulation)은 휴머노이드, 공유 물체, 환경을 하나의 결합된 물리 시스템(Coupled Physical System)으로 취급한다. 신뢰성 높은 행동은 물체 움직임, 상대 손 제약, 자세, 균형, 접촉력, 스텝, 인식, 순응성을 통합적으로 협조함으로써 구현된다. 이러한 요소를 통합하면 휴머노이드 로봇은 물리적 실현 가능성, 안정성, 적응성을 유지하면서 고정된 도달 범위를 넘어서는 복잡한 양손 작업을 수행할 수 있다.

##  

## 08.09. Bimanual Safety and Self Collision Avoidance [w/Code]

![](images/image9.png){width="7.268055555555556in" height="7.268055555555556in"}

Bimanual safety and self-collision avoidance are fundamental requirements for robots that operate two manipulators within a shared workspace. Unlike single-arm systems, each arm becomes a moving obstacle for the other while both may simultaneously interact with the same object. Safe operation therefore requires continuous reasoning about robot geometry, relative motion, object occupancy, contact forces, joint limits, and environmental constraints.

The collision model should represent the complete geometry of both manipulators, grippers, tools, torso, and other relevant robot structures. Exact mesh models provide high geometric fidelity but can be computationally expensive for real-time control. Simplified primitives, convex decompositions, capsules, or bounding volumes are commonly used to accelerate distance calculations while preserving sufficient accuracy around safety-critical regions.

Self-collision checking considers potentially interfering link pairs throughout the robot rather than only the two end effectors. A left forearm may collide with the right upper arm, one wrist may intersect the opposite gripper, or both elbows may approach the torso during coordinated motion. Collision matrices can exclude permanently adjacent link pairs while retaining checks for geometrically meaningful combinations that may collide during execution.

Minimum distance provides a useful continuous safety measure. Instead of waiting until two geometric models intersect, the controller tracks the closest distance between relevant link pairs. A warning region can be defined around each body component so that corrective action begins before physical contact occurs. Larger margins may be assigned to fast-moving links, uncertain geometry, fragile tools, or areas containing exposed sensors.

Distance alone is insufficient because collision risk also depends on relative velocity. Two links separated by a moderate distance may still be dangerous if they rapidly approach one another. Relative velocity, closing speed, and predicted time to collision can therefore supplement geometric distance. Motion can be reduced or redirected when predicted separation falls below a safe threshold within the planning horizon.

Continuous collision detection examines the swept motion between consecutive robot configurations rather than checking only discrete sampled poses. This is important when control or planning intervals are relatively large, because fast links may pass through each other between two apparently collision-free samples. Swept-volume tests or interpolated checks improve confidence that the complete trajectory remains geometrically valid.

Bimanual motion planning must consider the configuration spaces of both arms simultaneously. Planning each arm independently and combining the trajectories afterward can create collisions even when each individual path is valid. Coordinated planning treats the joint configuration of both manipulators as a shared state so that relative timing, spatial separation, and mutual accessibility can be optimized together.

The dimensionality of joint planning can become large, especially for robots with seven-degree-of-freedom arms, dexterous hands, torso joints, or mobile bases. Sampling-based planners, trajectory optimization, and hierarchical methods can reduce computational difficulty. Task structure can also restrict planning to relevant degrees of freedom, while local collision avoidance handles smaller deviations during execution.

Trajectory optimization can incorporate collision distance directly into its objective or constraints. Configurations approaching forbidden regions receive increasing penalties, encouraging smooth paths with meaningful clearance rather than trajectories that barely avoid contact. Additional terms can optimize path length, joint motion, manipulability, energy, and synchronization while maintaining collision-free coordination.

Velocity obstacles and dynamic collision constraints provide another way to reason about mutual motion. Instead of considering only geometric occupancy, the controller identifies combinations of velocities that would cause future collision. Commands can then be projected outside these unsafe velocity regions while preserving as much of the desired task motion as possible, supporting responsive avoidance during online execution.

Safety becomes more complex when both arms intentionally approach each other. Handover, cooperative carrying, assembly, and shared-object manipulation require close proximity that ordinary collision avoidance might incorrectly prohibit. The system must distinguish between allowed task-related proximity and forbidden robot-robot contact. Task-specific collision masks or contact permissions can temporarily modify which interactions are considered valid.

Allowed-contact definitions should remain precise and localized. Permitting contact between two grippers for a handover should not disable collision checking between the wrists, forearms, or other links. Contact permissions should specify the relevant bodies, geometric regions, task phase, and expected interaction type. They should automatically expire when the task transitions beyond the contact-required phase.

Shared-object manipulation introduces additional collision geometry because the grasped object effectively becomes part of the robot system. The planner must check the object against both arms, torso, environment, and sometimes the grippers themselves according to the intended contact model. Updating the object\'s collision geometry from the estimated grasp transform allows its swept volume to be considered during coordinated motion.

Object uncertainty should be incorporated into safety margins. Pose estimation error, imperfect calibration, object deformation, or grasp slip can cause actual geometry to differ from the planning model. Inflating collision volumes according to uncertainty provides a conservative buffer. The margin can be reduced when high-confidence perception and precise calibration are available and increased when uncertainty becomes larger.

Inter-arm collision avoidance can be implemented as inequality constraints within inverse kinematics or whole-body optimization. A distance Jacobian describes how joint velocities influence the separation between two closest points. When distance approaches a safety boundary, the optimizer can constrain motion so that the links stop approaching or begin separating while still attempting to satisfy the primary manipulation task.

Control barrier functions provide a formal mechanism for enforcing safety constraints during control. A barrier function can represent the allowable separation between robot bodies, and the controller modifies nominal commands when necessary to keep the system inside a safe set. This approach allows learned or planned actions to operate normally until they threaten a defined safety boundary.

Hierarchical control can assign collision avoidance a priority above ordinary manipulation objectives. When sufficient clearance exists, both arms follow their desired trajectories without modification. As the distance decreases, avoidance constraints gain influence and secondary task objectives may be sacrificed. Critical safety conditions can override manipulation entirely and command stopping or separation.

Joint limits are another form of self-safety that must be considered alongside collision avoidance. Bimanual coordination can drive one arm into extreme configurations while the other remains comfortable. Soft joint-limit costs can steer the robot away from mechanical boundaries before hard limits are reached, preserving available motion for later phases and reducing the risk of abrupt controller saturation.

Singularity avoidance is closely related to safe motion generation. Near a kinematic singularity, small Cartesian commands may require large joint velocities, reducing control authority and increasing collision risk. Manipulability measures or singular-value constraints can be included in planning and control so that both arms maintain configurations capable of producing controlled corrective motion when unexpected events occur.

Velocity and acceleration limits should be enforced independently for every joint and task-space component. Even a geometrically collision-free trajectory may be unsafe if one arm moves too rapidly near the other arm, a human, or a fragile object. Context-dependent speed limits can reduce velocity when separation decreases and permit faster motion when the manipulators operate in clearly separated regions.

Force and torque monitoring provide a second safety layer when geometric prevention is insufficient. Unexpected contact may arise from modeling errors, perception failures, or objects that move unpredictably. Joint torque sensing, wrist force-torque sensors, motor current, and tactile sensing can detect abnormal interaction. Threshold violations can trigger compliant behavior, controlled retreat, protective stop, or emergency stop.

Collision detection and collision avoidance should therefore be treated as complementary capabilities. Avoidance attempts to prevent contact through planning and command modification, while detection identifies contact that nevertheless occurs. A robust bimanual system requires both because no geometric model or state estimate can perfectly represent the physical world during every manipulation task.

Compliance reduces the severity of unavoidable or intended contact. Impedance control can lower effective stiffness when manipulators operate close together or interact through a shared object. Rather than rigidly resisting every pose deviation, compliant arms can absorb small errors and limit peak contact forces. Variable impedance allows stiffness to change according to task phase and proximity.

Safe stopping requires more than immediately setting commanded velocity to zero. Physical manipulators possess momentum, controller delay, and finite braking capability. The safety system should estimate stopping distance and ensure that sufficient separation exists to decelerate before collision. Higher velocities therefore require larger protective distances, creating a direct relationship between motion speed and safety margin.

Emergency behavior should be designed according to the physical state of the task. Abruptly disabling actuators while both arms hold a heavy object may cause the object to fall and create a greater hazard. Depending on the situation, a safer response may involve controlled deceleration, maintaining grasp force, moving to a stable support pose, or placing the object before transitioning to a fully stopped state.

Human presence introduces additional safety requirements. A dual-arm robot operating near people must consider human body regions as dynamic obstacles and apply appropriate separation and speed limits. Vision, depth sensing, safety scanners, or other protective sensors can update human position estimates. Robot motion should become progressively more conservative as uncertainty or human proximity increases.

Perception latency must be included when defining protective separation. A moving arm continues traveling while sensor data are acquired, processed, communicated, and converted into a control response. The total reaction time includes sensing, inference, planning, communication, and actuator response. Safety margins should account for the distance that robot and external obstacles can travel during this interval.

Redundant safety supervision can separate high-level intelligence from low-level protection. A learned policy or task planner may generate desired actions, while an independent safety layer verifies joint limits, collision distances, speeds, forces, and workspace boundaries. Hardware-level protection can provide another layer for emergency stopping. This architecture prevents a single software failure from directly commanding unrestricted hazardous motion.

Learned bimanual policies require particular attention because neural networks may produce actions outside the distribution encountered during training. Policy outputs can be passed through a safety filter that checks predicted trajectories and modifies unsafe commands. Optimization-based projection can find the nearest feasible action satisfying collision, joint, velocity, and workspace constraints while preserving the policy intent where possible.

Predictive safety checking improves on purely reactive filtering by evaluating several future states. If a predicted action chunk from an ACT, diffusion, or other sequence policy gradually leads toward collision, the system can reject or modify it before the immediate command becomes dangerous. This is especially useful for bimanual policies because unsafe interactions may emerge from the combined future motion of both arms.

Simulation provides an efficient environment for testing collision scenarios that would be risky on physical hardware. Randomized initial poses, timing offsets, object locations, sensor errors, and unexpected obstacles can expose weaknesses in planning and safety logic. Automated stress testing can generate large numbers of near-collision cases and measure whether the controller preserves required separation.

Hardware validation should begin at low speed with generous margins and lightweight objects. Test difficulty can then increase through closer arm interaction, higher velocities, larger payloads, intentional disturbances, and controlled perception errors. High-speed logging of joint states, collision distances, predicted time to collision, forces, safety interventions, and stop events supports systematic diagnosis.

Useful evaluation metrics include minimum inter-link distance, number of safety interventions, false-positive avoidance events, collision count, stopping distance, peak contact force, task completion rate, and additional path length caused by avoidance. A good safety system should prevent hazardous contact without making normal bimanual coordination unnecessarily slow, conservative, or unreliable.

Ultimately, bimanual safety requires continuous integration of geometric reasoning, predictive motion analysis, force monitoring, compliant control, and independent supervision. The two arms must be allowed to cooperate closely when the task requires it while remaining protected from unintended self-contact and unstable configurations. Safety therefore becomes an active component of planning and control rather than a final emergency mechanism.

양팔 안전 및 자체 충돌 회피(Bimanual Safety and Self-Collision Avoidance)는 공유 작업 공간(Shared Workspace)에서 두 매니퓰레이터를 운용하는 로봇의 기본적인 요구사항이다. 단일 팔 시스템과 달리 양팔 시스템에서는 각 로봇 팔이 다른 팔에 대해 움직이는 장애물(Moving Obstacle)이 되며, 동시에 두 팔이 동일한 물체와 상호작용할 수도 있다. 따라서 안전한 동작을 위해서는 로봇 형상, 상대 운동, 물체 점유 영역(Object Occupancy), 접촉력, 관절 한계, 환경 제약을 지속적으로 고려해야 한다.

충돌 모델(Collision Model)은 두 매니퓰레이터, 그리퍼, 도구, 몸통 및 기타 관련 로봇 구조물의 전체 형상을 표현해야 한다. 정확한 메시 모델(Mesh Model)은 높은 기하학적 정밀도를 제공하지만 실시간 제어에서는 계산 비용이 커질 수 있다. 따라서 단순화된 기본 형상(Primitive), 볼록 분해(Convex Decomposition), 캡슐(Capsule), 경계 체적(Bounding Volume)을 사용하여 안전에 중요한 영역에서 충분한 정확도를 유지하면서 거리 계산을 가속하는 방법이 일반적으로 사용된다.

자체 충돌 검사(Self-Collision Checking)는 두 말단장치(End Effector)만 확인하는 것이 아니라 로봇 전체에서 서로 간섭할 가능성이 있는 링크 쌍(Link Pair)을 고려한다. 왼쪽 전완(Forearm)이 오른쪽 상완(Upper Arm)과 충돌하거나, 한쪽 손목이 반대쪽 그리퍼와 교차하거나, 협조 동작 중 두 팔꿈치가 몸통에 접근할 수 있다. 충돌 행렬(Collision Matrix)은 항상 인접해 있는 링크 쌍을 검사 대상에서 제외하면서 실제 움직임 중 충돌 가능성이 있는 기하학적으로 의미 있는 조합을 유지할 수 있다.

최소 거리(Minimum Distance)는 연속적인 안전 척도로 활용할 수 있다. 두 기하학적 모델이 실제로 교차할 때까지 기다리는 대신 제어기는 관련 링크 쌍 사이의 가장 가까운 거리를 지속적으로 추적한다. 각 신체 구성 요소 주변에 경고 영역(Warning Region)을 정의하여 물리적 접촉이 발생하기 전에 수정 동작을 시작할 수 있다. 빠르게 움직이는 링크, 형상 불확실성이 큰 영역, 깨지기 쉬운 도구, 노출된 센서 주변에는 더 큰 안전 여유(Safety Margin)를 설정할 수 있다.

거리만으로는 충돌 위험을 충분히 판단할 수 없으며 상대 속도(Relative Velocity)도 함께 고려해야 한다. 두 링크 사이에 어느 정도 거리가 있더라도 빠르게 서로 접근하고 있다면 위험할 수 있다. 따라서 상대 속도, 접근 속도(Closing Speed), 예상 충돌 시간(Time to Collision)을 기하학적 거리와 함께 사용할 수 있다. 계획 지평(Planning Horizon) 내에서 예상 분리 거리가 안전 임계값 아래로 감소할 경우 움직임을 감속하거나 다른 방향으로 수정할 수 있다.

연속 충돌 검출(Continuous Collision Detection)은 이산적으로 샘플링된 자세만 검사하는 대신 연속된 로봇 구성 사이의 이동 영역(Swept Motion)을 검사한다. 제어 또는 계획 간격이 비교적 큰 경우 빠르게 움직이는 링크가 충돌이 없는 두 샘플 자세 사이에서 서로 관통할 수 있기 때문에 이러한 방식이 중요하다. 스윕 체적 검사(Swept-Volume Test) 또는 보간 검사(Interpolated Check)를 사용하면 전체 궤적이 기하학적으로 유효한지에 대한 신뢰성을 높일 수 있다.

양팔 동작 계획(Bimanual Motion Planning)은 두 팔의 구성 공간(Configuration Space)을 동시에 고려해야 한다. 각 팔의 궤적을 독립적으로 계획한 뒤 결합하면 개별 경로는 모두 유효하더라도 두 팔 사이에 충돌이 발생할 수 있다. 협조 계획(Coordinated Planning)은 두 매니퓰레이터의 관절 구성을 하나의 공유 상태(Shared State)로 처리하여 상대적인 타이밍, 공간적 분리, 상호 접근 가능성을 함께 최적화한다.

특히 7자유도 팔(Seven-Degree-of-Freedom Arm), 다관절 손(Dexterous Hand), 몸통 관절, 모바일 베이스가 포함되면 공동 계획의 차원은 매우 커질 수 있다. 샘플링 기반 계획기(Sampling-Based Planner), 궤적 최적화(Trajectory Optimization), 계층적 방법(Hierarchical Method)을 이용하여 계산 난이도를 줄일 수 있다. 또한 작업 구조에 따라 계획에 필요한 자유도만 제한적으로 사용하고 실행 중 발생하는 작은 편차는 국부 충돌 회피(Local Collision Avoidance)로 처리할 수 있다.

궤적 최적화는 충돌 거리를 목적함수(Objective Function) 또는 제약조건에 직접 포함할 수 있다. 금지 영역에 접근하는 구성에 점차 큰 페널티를 부여하면 단순히 접촉 직전까지 접근하는 궤적보다 의미 있는 여유 공간을 유지하는 부드러운 경로를 생성할 수 있다. 동시에 경로 길이, 관절 움직임, 조작성(Manipulability), 에너지, 동기화 등을 최적화하면서 충돌 없는 협조 동작을 유지할 수 있다.

속도 장애물(Velocity Obstacle)과 동적 충돌 제약(Dynamic Collision Constraint)은 상호 움직임을 고려하는 또 다른 방법을 제공한다. 단순히 기하학적 점유 영역만 고려하는 대신 미래에 충돌을 발생시키는 속도 조합을 식별한다. 이후 명령을 이러한 위험 속도 영역 밖으로 투영하면서 원하는 작업 움직임을 가능한 한 유지하여 온라인 실행 중에도 반응성 높은 충돌 회피를 구현할 수 있다.

두 팔이 의도적으로 서로 접근해야 하는 경우 안전 문제는 더욱 복잡해진다. 핸드오버(Handover), 협동 운반(Cooperative Carrying), 조립(Assembly), 공유 물체 조작(Shared-Object Manipulation)에서는 일반적인 충돌 회피가 잘못해서 금지할 수 있는 근접 동작이 필요하다. 시스템은 작업에 필요한 허용된 근접 상태(Allowed Task-Related Proximity)와 금지된 로봇 간 접촉을 구분해야 한다. 작업별 충돌 마스크(Task-Specific Collision Mask) 또는 접촉 허용 규칙(Contact Permission)을 사용하여 어떤 상호작용을 허용할지 일시적으로 변경할 수 있다.

허용 접촉 정의(Allowed-Contact Definition)는 정확하고 국부적으로 설정해야 한다. 핸드오버를 위해 두 그리퍼 사이의 접촉을 허용한다고 해서 손목, 전완 또는 다른 링크 사이의 충돌 검사를 비활성화해서는 안 된다. 접촉 허용 규칙은 관련 신체 부위, 기하학적 영역, 작업 단계(Task Phase), 예상되는 상호작용 유형을 명시해야 한다. 또한 접촉이 필요한 작업 단계가 종료되면 해당 허용 규칙은 자동으로 해제되어야 한다.

공유 물체 조작은 파지된 물체가 사실상 로봇 시스템의 일부가 되기 때문에 추가적인 충돌 형상을 발생시킨다. 계획기는 의도된 접촉 모델에 따라 물체와 두 팔, 몸통, 환경, 경우에 따라 그리퍼 사이의 충돌을 검사해야 한다. 추정된 파지 변환(Grasp Transform)을 이용해 물체의 충돌 형상을 갱신하면 협조 움직임 동안 물체의 이동 체적(Swept Volume)까지 고려할 수 있다.

물체 불확실성(Object Uncertainty)은 안전 여유에 반영해야 한다. 자세 추정 오차(Pose Estimation Error), 불완전한 보정(Calibration), 물체 변형, 파지 미끄러짐(Grasp Slip)으로 인해 실제 형상이 계획 모델과 달라질 수 있다. 불확실성 수준에 따라 충돌 체적을 팽창시키면 보수적인 안전 버퍼를 확보할 수 있다. 신뢰도가 높은 인식과 정밀한 보정이 가능한 경우에는 여유를 줄이고 불확실성이 증가하면 다시 확대할 수 있다.

팔 사이 충돌 회피(Inter-Arm Collision Avoidance)는 역운동학(Inverse Kinematics) 또는 전신 최적화(Whole-Body Optimization) 내부의 부등식 제약(Inequality Constraint)으로 구현할 수 있다. 거리 자코비안(Distance Jacobian)은 관절 속도가 두 최근접점 사이의 거리에 어떤 영향을 주는지 표현한다. 거리가 안전 경계에 접근하면 최적화기는 주요 조작 작업을 최대한 유지하면서 링크가 더 이상 접근하지 않거나 서로 멀어지도록 움직임을 제한할 수 있다.

제어 장벽 함수(Control Barrier Function)는 제어 과정에서 안전 제약을 강제하기 위한 형식적인 방법을 제공한다. 장벽 함수는 로봇 신체 사이에 허용되는 분리 거리를 표현할 수 있으며, 시스템이 정의된 안전 집합(Safe Set) 내부에 유지되도록 필요할 때 제어기가 기준 명령을 수정한다. 이를 통해 학습 또는 계획된 행동은 안전 경계를 위협하기 전까지 정상적으로 실행될 수 있다.

계층형 제어(Hierarchical Control)는 일반적인 조작 목표보다 충돌 회피에 더 높은 우선순위를 부여할 수 있다. 충분한 여유 공간이 존재하면 두 팔은 수정 없이 원하는 궤적을 따라간다. 거리가 감소하면 회피 제약의 영향력이 증가하고 보조 작업 목표가 희생될 수 있다. 위험한 안전 조건에서는 조작 작업 전체를 무시하고 정지 또는 분리 동작을 명령할 수 있다.

관절 한계(Joint Limit)는 충돌 회피와 함께 고려해야 하는 또 다른 형태의 자체 안전(Self-Safety)이다. 양팔 협조 과정에서 한쪽 팔은 편안한 자세를 유지하는 반면 다른 팔은 극단적인 관절 구성으로 이동할 수 있다. 소프트 관절 한계 비용(Soft Joint-Limit Cost)을 사용하면 하드웨어 한계에 도달하기 전에 로봇을 기계적 경계에서 멀어지도록 유도하여 이후 작업에 필요한 움직임 여유를 확보하고 갑작스러운 제어기 포화(Controller Saturation)를 줄일 수 있다.

특이점 회피(Singularity Avoidance)는 안전한 움직임 생성과 밀접하게 관련된다. 운동학적 특이점 근처에서는 작은 직교좌표 명령이 매우 큰 관절 속도를 요구할 수 있어 제어 권한(Control Authority)이 감소하고 충돌 위험이 증가한다. 조작성 지표 또는 특이값 제약(Singular-Value Constraint)을 계획 및 제어에 포함하면 예상하지 못한 상황에서 두 팔이 제어된 수정 움직임을 수행할 수 있는 구성을 유지할 수 있다.

속도와 가속도 제한(Velocity and Acceleration Limits)은 각 관절과 작업 공간 요소에 독립적으로 적용해야 한다. 기하학적으로 충돌이 없는 궤적이라도 한쪽 팔이 다른 팔, 사람 또는 깨지기 쉬운 물체 근처에서 지나치게 빠르게 움직이면 안전하지 않을 수 있다. 상황 의존적 속도 제한(Context-Dependent Speed Limit)을 적용하면 매니퓰레이터 사이의 거리가 감소할수록 속도를 낮추고 명확하게 분리된 영역에서는 더 빠른 동작을 허용할 수 있다.

힘 및 토크 모니터링(Force and Torque Monitoring)은 기하학적 충돌 방지가 충분하지 않을 때 두 번째 안전 계층을 제공한다. 모델링 오차, 인식 실패, 예측하지 못한 물체 움직임으로 인해 예상하지 못한 접촉이 발생할 수 있다. 관절 토크 센싱, 손목 힘-토크 센서(Force-Torque Sensor), 모터 전류, 촉각 센싱(Tactile Sensing)을 통해 비정상적인 상호작용을 감지할 수 있다. 임계값을 초과하면 순응 동작, 제어된 후퇴, 보호 정지(Protective Stop), 비상 정지(Emergency Stop)를 실행할 수 있다.

따라서 충돌 검출(Collision Detection)과 충돌 회피(Collision Avoidance)는 서로 보완적인 기능으로 취급해야 한다. 충돌 회피는 계획 및 명령 수정을 통해 접촉 자체를 방지하려고 하며, 충돌 검출은 그럼에도 불구하고 발생한 접촉을 식별한다. 어떠한 기하학적 모델이나 상태 추정도 모든 조작 상황에서 실제 물리 세계를 완벽하게 표현할 수 없기 때문에 강건한 양팔 시스템에는 두 기능이 모두 필요하다.

순응성(Compliance)은 피할 수 없거나 의도된 접촉의 심각성을 감소시킨다. 임피던스 제어(Impedance Control)는 두 매니퓰레이터가 가까이 동작하거나 공유 물체를 통해 상호작용할 때 유효 강성(Effective Stiffness)을 낮출 수 있다. 모든 자세 오차에 강체적으로 저항하는 대신 순응형 팔은 작은 오차를 흡수하고 최대 접촉력을 제한할 수 있다. 가변 임피던스(Variable Impedance)를 사용하면 작업 단계와 근접 정도에 따라 강성을 변화시킬 수 있다.

안전 정지(Safe Stopping)는 단순히 명령 속도를 즉시 0으로 설정하는 것 이상을 요구한다. 실제 매니퓰레이터에는 운동량, 제어기 지연, 제한된 제동 능력이 존재한다. 안전 시스템은 정지 거리(Stopping Distance)를 추정하고 충돌 전에 감속할 수 있는 충분한 분리 거리를 확보해야 한다. 따라서 높은 이동 속도에서는 더 큰 보호 거리(Protective Distance)가 필요하며, 움직임 속도와 안전 여유 사이에는 직접적인 관계가 형성된다.

비상 동작(Emergency Behavior)은 작업의 물리적 상태에 따라 설계해야 한다. 두 팔이 무거운 물체를 들고 있는 상태에서 액추에이터를 갑자기 비활성화하면 물체가 떨어져 오히려 더 큰 위험을 발생시킬 수 있다. 상황에 따라서는 제어된 감속, 파지력 유지, 안정적인 지지 자세로 이동, 물체를 안전한 위치에 내려놓은 이후 완전 정지 상태로 전환하는 것이 더 안전할 수 있다.

사람의 존재(Human Presence)는 추가적인 안전 요구사항을 발생시킨다. 사람 근처에서 동작하는 양팔 로봇은 사람의 신체 영역을 동적 장애물(Dynamic Obstacle)로 고려하고 적절한 분리 거리와 속도 제한을 적용해야 한다. 비전, 깊이 센싱(Depth Sensing), 안전 스캐너(Safety Scanner) 또는 다른 보호 센서를 이용하여 사람의 위치 추정값을 지속적으로 갱신할 수 있다. 사람과의 거리가 감소하거나 위치 불확실성이 증가하면 로봇의 움직임은 점진적으로 더 보수적으로 변경되어야 한다.

보호 분리 거리(Protective Separation)를 정의할 때는 인식 지연(Perception Latency)도 포함해야 한다. 센서 데이터를 획득하고 처리하며 통신하고 제어 응답으로 변환하는 동안 움직이는 로봇 팔은 계속 이동한다. 전체 반응 시간에는 센싱, 추론(Inference), 계획, 통신, 액추에이터 응답이 포함된다. 따라서 안전 여유는 이 시간 동안 로봇과 외부 장애물이 이동할 수 있는 거리를 고려해야 한다.

중복 안전 감독(Redundant Safety Supervision)을 사용하면 상위 수준 지능과 저수준 보호 기능을 분리할 수 있다. 학습 정책(Learned Policy) 또는 작업 계획기(Task Planner)가 목표 행동을 생성하는 동안 독립적인 안전 계층이 관절 한계, 충돌 거리, 속도, 힘, 작업 공간 경계를 검증할 수 있다. 하드웨어 수준 보호 기능은 비상 정지를 위한 또 다른 계층을 제공한다. 이러한 구조는 하나의 소프트웨어 오류가 제한되지 않은 위험 동작으로 직접 이어지는 것을 방지한다.

학습 기반 양팔 정책(Learned Bimanual Policy)은 신경망이 학습 과정에서 경험하지 않은 분포 밖 행동(Out-of-Distribution Action)을 생성할 수 있으므로 특별한 주의가 필요하다. 정책 출력은 예측 궤적을 검사하고 위험한 명령을 수정하는 안전 필터(Safety Filter)를 통과시킬 수 있다. 최적화 기반 투영(Optimization-Based Projection)을 사용하면 정책의 의도를 가능한 한 유지하면서 충돌, 관절, 속도, 작업 공간 제약을 만족하는 가장 가까운 실행 가능 행동을 찾을 수 있다.

예측 안전 검사(Predictive Safety Checking)는 여러 미래 상태를 평가하여 단순한 반응형 필터링보다 향상된 안전성을 제공한다. ACT, 확산 정책(Diffusion Policy) 또는 다른 시퀀스 정책에서 예측된 행동 청크(Action Chunk)가 점진적으로 충돌 상태로 접근한다면 즉시 실행할 명령 자체가 위험해지기 전에 해당 청크를 거부하거나 수정할 수 있다. 양팔 정책에서는 두 팔의 결합된 미래 움직임으로 인해 위험한 상호작용이 발생할 수 있으므로 이러한 접근법이 특히 유용하다.

시뮬레이션(Simulation)은 실제 하드웨어에서 시험하기 위험한 충돌 시나리오를 평가할 수 있는 효율적인 환경을 제공한다. 초기 자세, 타이밍 오프셋(Timing Offset), 물체 위치, 센서 오차, 예상하지 못한 장애물을 무작위화하여 계획 및 안전 논리의 약점을 확인할 수 있다. 자동화된 스트레스 시험(Automated Stress Testing)을 통해 대량의 근접 충돌 사례를 생성하고 제어기가 필요한 분리 거리를 유지하는지 평가할 수 있다.

하드웨어 검증(Hardware Validation)은 낮은 속도, 충분한 안전 여유, 가벼운 물체를 사용하여 시작해야 한다. 이후 두 팔 사이의 더 가까운 상호작용, 높은 속도, 큰 페이로드(Payload), 의도적인 외란, 제어된 인식 오차를 도입하면서 시험 난이도를 점진적으로 증가시킬 수 있다. 관절 상태, 충돌 거리, 예상 충돌 시간, 힘, 안전 개입(Safety Intervention), 정지 이벤트를 고속으로 기록하면 체계적인 원인 분석이 가능하다.

유용한 평가 지표(Evaluation Metric)에는 최소 링크 간 거리(Minimum Inter-Link Distance), 안전 개입 횟수, 오탐 회피 이벤트(False-Positive Avoidance Event), 충돌 횟수, 정지 거리, 최대 접촉력, 작업 완료율, 충돌 회피로 인해 증가한 추가 경로 길이 등이 포함된다. 우수한 안전 시스템은 위험한 접촉을 방지하면서도 정상적인 양팔 협조 작업을 불필요하게 느리거나 지나치게 보수적이고 불안정하게 만들어서는 안 된다.

궁극적으로 양팔 안전(Bimanual Safety)은 기하학적 추론(Geometric Reasoning), 예측 동작 분석(Predictive Motion Analysis), 힘 모니터링, 순응 제어, 독립적인 안전 감독을 지속적으로 통합해야 한다. 작업에서 필요한 경우 두 팔이 서로 긴밀하게 협력할 수 있도록 허용하면서도 의도하지 않은 자체 접촉(Self-Contact)과 불안정한 구성으로부터 보호해야 한다. 따라서 안전은 마지막 단계의 비상 메커니즘이 아니라 계획과 제어 과정 전체에 능동적으로 포함되는 핵심 구성 요소가 된다.

##  

## 08.10. Bimanual Assembly Task Production Case

![](images/image10.png){width="7.268055555555556in" height="7.268055555555556in"}

A production bimanual assembly task uses two robotic manipulators as a coordinated work system rather than as independent arms. One arm may stabilize a housing while the other inserts a component, both may transport a large part, or their roles may change during the assembly cycle. The central engineering challenge is to coordinate perception, grasping, motion, force, tooling, and quality verification within a repeatable industrial process.

Production deployment begins by decomposing the assembly operation into physical interaction phases. A representative sequence may include part detection, acquisition, pre-alignment, cooperative positioning, insertion, fastening, inspection, and release. Each phase imposes different requirements on accuracy, stiffness, force limits, cycle time, and sensing, so the complete task should not be controlled as one undifferentiated trajectory.

The workstation layout strongly influences bimanual performance. Feeders, fixtures, cameras, tools, trays, and finished-part locations should be positioned inside favorable shared workspace rather than merely within maximum reach. Good layout reduces extreme joint configurations, inter-arm interference, unnecessary regrasping, and long transfer motions while preserving access for maintenance and human operators.

A common production case involves one arm holding a primary assembly while the second arm installs a secondary component. The holding arm functions as an adaptive fixture, controlling the workpiece pose while absorbing reaction forces from insertion or fastening. This approach can reduce dedicated mechanical fixtures and allows the same cell to handle product variants by changing robot programs, grasps, and tools.

The two arms should be assigned roles according to task geometry rather than permanently labeling one as the holder and the other as the worker. During one phase, the left arm may stabilize the component while the right performs insertion. Later, the right arm may support the assembled structure while the left acquires another part. Dynamic role allocation improves workspace utilization and can reduce unnecessary object transfers.

Part presentation is a major determinant of production reliability. Components supplied in known trays or fixtures simplify perception and grasp planning, whereas randomly oriented parts require detection, pose estimation, and potentially bin picking. A production architecture should use mechanical organization where economical and reserve sophisticated perception for variations that cannot reasonably be eliminated through workstation design.

Vision typically provides global information about part identity, pose, orientation, and assembly state. Fixed cameras can monitor the overall workstation, while wrist-mounted cameras provide close views of grasp and insertion regions. Depth sensing can support three-dimensional localization, but high-precision assembly may still require geometric features, fiducials, structured fixtures, or local visual servoing near the final mating position.

Calibration links camera coordinates, robot bases, tools, grippers, and fixtures into a consistent geometric model. Small calibration errors that are acceptable for free-space transport can cause insertion failure when tolerances are tight. Production cells therefore require repeatable tool-center-point calibration, camera extrinsic calibration, fixture registration, and procedures for detecting drift after maintenance or tool replacement.

Grasp planning should consider the entire assembly sequence rather than only initial pickup. A grasp that is stable during transport may block an insertion feature, interfere with the second arm, or prevent access by a fastening tool. Task-aware grasp selection evaluates whether each grasp supports downstream positioning, cooperative manipulation, force transmission, inspection, and final release.

Coarse positioning and fine assembly should generally use different control strategies. Motion planning can rapidly move components through free space while avoiding the other arm and workstation obstacles. As the parts approach the mating region, the system can transition to lower velocity, higher sensing sensitivity, and compliant control. This separation improves cycle time without sacrificing precision during contact.

Pre-alignment reduces the uncertainty presented to the contact controller. Visual servoing or geometric registration can align holes, shafts, connectors, edges, or mating surfaces before physical insertion begins. The goal is not necessarily to achieve perfect alignment from vision alone, but to enter a capture region in which compliant motion and force sensing can reliably complete the assembly.

Insertion is a characteristic contact-rich bimanual operation. One arm maintains the reference component while the second moves the mating component along a planned direction. Small translational or angular errors can create lateral forces and jamming. Force-torque sensing combined with compliant motion allows the robot to detect misalignment and make corrective adjustments rather than increasing force against an obstructed interface.

Impedance control is particularly useful during mating operations because it defines controlled mechanical compliance around the desired trajectory. Lower stiffness in alignment directions can allow parts to self-correct through contact geometry, while higher stiffness along selected directions preserves task progress. Stiffness and damping can change according to assembly phase, object properties, and measured contact conditions.

Search strategies can recover from residual alignment uncertainty. Spiral, raster, rotational, or small oscillatory motions can be applied while monitoring contact forces to locate holes, slots, connectors, or reference surfaces. Search amplitudes should remain bounded to prevent component damage. Successful contact signatures can trigger transition from search behavior to insertion or fastening.

Cooperative force control becomes necessary when reaction forces propagate through both manipulators. The arm holding the workpiece should not rigidly fight every force produced by the inserting arm. Object-level control can regulate the desired assembly pose while distributing support and interaction forces between both arms. Internal forces should remain sufficient for grasp stability without unnecessarily loading the product.

Fastening introduces additional torque and reaction-force requirements. If one arm operates a screwdriver, nut runner, or similar tool, the second arm may stabilize the assembly against tool torque. Tool engagement, torque buildup, final torque, angle, and seating state can be monitored to determine whether the fastening operation completed correctly rather than assuming success from commanded tool motion.

Connector insertion provides another representative case. Electrical connectors may require orientation alignment, controlled insertion force, latch engagement, and verification of final seating. Excessive force can damage pins, while insufficient insertion can produce latent quality failures. Position, force, visual, and sometimes electrical continuity information can be combined to confirm that the connector reached the intended final state.

Flexible or semi-flexible components create additional challenges. Cable routing, gasket installation, hose connection, and harness assembly require the two arms to control shape as well as pose. One arm may maintain tension or guide a deformable segment while the other performs insertion or attachment. Vision and force feedback must track configuration changes that cannot be represented by a single rigid-body pose.

The task coordinator manages transitions among perception, motion, contact, tooling, and verification states. A finite-state machine or behavior tree can encode normal production flow together with retries and recovery branches. Transitions should depend on verified events such as grasp confirmation, alignment confidence, contact detection, insertion depth, fastening torque, or inspection result rather than relying only on elapsed time.

Recovery behavior is essential because production systems must handle imperfect parts and uncertain interactions without immediately stopping the entire cell. If insertion force rises unexpectedly, the robot can retract, reacquire local pose, adjust alignment, and retry within defined limits. Repeated failure should classify the part or process condition and transfer control to an exception-handling procedure.

Self-collision avoidance remains active throughout assembly even when the arms intentionally operate close together. Task-specific contact permissions can allow the grippers or shared object to approach as required while continuing to protect wrists, forearms, elbows, tools, and the torso. Predictive collision checking should consider both arms and grasped components because future combined motion may create interference.

Cycle-time optimization should be performed after reliable task execution has been established. Free-space motions can be accelerated, perception can operate concurrently with safe robot movement, and one arm can acquire the next component while the other completes a suitable operation. Parallelization can significantly improve throughput, but dependencies involving shared workspace or contact should remain explicitly synchronized.

Production scheduling can exploit the natural parallelism of two manipulators. While one arm performs an operation that does not require the other, the second arm may prepare a tool, inspect a part, or move toward the next pickup location. The coordinator should identify which operations can overlap safely and which require strict barriers because both arms depend on the same object or workspace region.

Quality assurance should be integrated directly into the assembly sequence rather than treated only as a final inspection step. Cameras can verify orientation and presence before mating, force profiles can indicate insertion quality, fastening tools can report torque-angle signatures, and post-assembly vision can confirm geometry. Process evidence can therefore be collected continuously for each manufactured unit.

Traceability links assembly measurements to individual products or production lots. Robot programs, component identities, timestamps, force curves, torque results, inspection images, retries, and final status can be recorded in a manufacturing database. Such records support quality analysis, root-cause investigation, maintenance, and comparison of process performance across shifts or product variants.

Statistical process monitoring can detect degradation before outright failure becomes frequent. Gradual increases in insertion force, grasp correction, alignment time, or retry frequency may indicate tool wear, fixture drift, calibration error, component variation, or gripper degradation. Production data can therefore support predictive maintenance and process adjustment in addition to pass-or-fail quality control.

Product variation should be handled through parameterized task models whenever possible. Different variants may share the same assembly sequence while using different object models, grasp poses, insertion depths, force thresholds, or tool settings. Separating reusable skill logic from product-specific parameters reduces engineering effort and makes bimanual automation more scalable across mixed production.

Digital twins and simulation can validate workstation geometry and robot coordination before physical commissioning. Engineers can evaluate reachability, inter-arm collision, fixture placement, cycle time, and candidate assembly trajectories. Contact-rich processes require careful physical validation, but simulation can eliminate many geometric and sequencing problems before hardware testing begins.

Offline programming should be followed by progressive commissioning on the real cell. Initial tests can use reduced speed, generous collision margins, and lightweight or sacrificial components. Once calibration, grasping, and contact behaviors are verified, speed and production constraints can be increased systematically while monitoring forces, errors, safety events, and component quality.

Safety architecture should remain independent from nominal task success. Joint limits, speed limits, force thresholds, collision monitoring, protective stops, and emergency functions must remain effective even if perception or task logic fails. In collaborative environments, human detection and protective separation may additionally restrict robot speed or halt specific operations when personnel enter controlled regions.

Performance evaluation should combine productivity and physical quality. Relevant metrics include cycle time, first-pass yield, insertion success, fastening quality, grasp success, retry rate, peak contact force, collision interventions, downtime, and mean recovery time. Measuring only successful completion can hide unstable processes that depend on frequent retries or excessive mechanical loading.

A mature production cell should support controlled change management. Updates to perception models, robot trajectories, force thresholds, tools, or product parameters should be versioned and validated before release. Regression testing can replay representative production conditions to verify that improvements in one task phase do not introduce new failures, collisions, or cycle-time penalties elsewhere.

Ultimately, a bimanual production assembly system succeeds when two arms, sensors, tools, fixtures, controllers, and quality systems operate as one coordinated manufacturing process. The objective is not merely to reproduce human two-handed motion, but to combine complementary robotic capabilities with structured production engineering. This integration enables flexible assembly with repeatable quality, recoverable contact behavior, traceable process evidence, and scalable automation.

생산 환경의 양팔 조립 작업(Bimanual Assembly Task)은 두 로봇 매니퓰레이터를 독립적인 로봇 팔이 아니라 하나의 협조 작업 시스템(Coordinated Work System)으로 사용한다. 한쪽 팔이 하우징(Housing)을 안정적으로 지지하는 동안 다른 팔이 부품을 삽입할 수 있고, 두 팔이 대형 부품을 함께 운반하거나 조립 주기 중 서로 역할을 변경할 수도 있다. 핵심적인 엔지니어링 과제는 인식, 파지, 동작, 힘, 도구, 품질 검증을 반복 가능한 산업 공정으로 통합하는 것이다.

생산 시스템의 배치(Production Deployment)는 조립 작업을 물리적 상호작용 단계(Physical Interaction Phase)로 분해하는 것에서 시작한다. 대표적인 작업 순서는 부품 감지, 획득, 사전 정렬(Pre-Alignment), 협동 위치 결정, 삽입, 체결(Fastening), 검사, 해제를 포함할 수 있다. 각 단계는 정확도, 강성(Stiffness), 힘 제한, 사이클 타임(Cycle Time), 센싱에 서로 다른 요구사항을 가지므로 전체 작업을 하나의 구분되지 않은 궤적으로 제어해서는 안 된다.

작업 셀 배치(Workstation Layout)는 양팔 작업 성능에 큰 영향을 미친다. 피더(Feeder), 지그(Fixture), 카메라, 도구, 트레이, 완성품 위치는 단순히 최대 도달 범위 안에 배치하는 것이 아니라 양팔이 유리하게 사용할 수 있는 공유 작업 공간(Shared Workspace)에 배치해야 한다. 적절한 배치는 극단적인 관절 구성, 팔 사이의 간섭, 불필요한 재파지(Regrasping), 긴 이송 동작을 줄이면서 유지보수와 작업자의 접근성을 확보한다.

대표적인 생산 사례에서는 한쪽 팔이 주 조립체(Primary Assembly)를 잡고 다른 팔이 보조 부품(Secondary Component)을 설치한다. 물체를 잡는 팔은 적응형 지그(Adaptive Fixture)처럼 동작하여 작업물 자세를 제어하면서 삽입 또는 체결 과정에서 발생하는 반작용력을 흡수한다. 이러한 방식은 전용 기계식 지그에 대한 의존성을 줄이고 로봇 프로그램, 파지, 도구를 변경하여 동일한 셀에서 다양한 제품 변형(Product Variant)을 처리할 수 있게 한다.

두 팔의 역할은 한쪽을 항상 고정 팔, 다른 쪽을 항상 작업 팔로 정의하기보다 작업의 기하학적 특성에 따라 할당해야 한다. 한 단계에서는 왼쪽 팔이 부품을 안정화하고 오른쪽 팔이 삽입을 수행할 수 있다. 이후 오른쪽 팔이 조립된 구조를 지지하는 동안 왼쪽 팔이 다음 부품을 가져올 수도 있다. 동적 역할 할당(Dynamic Role Allocation)은 작업 공간 활용도를 향상시키고 불필요한 물체 전달을 줄일 수 있다.

부품 공급 방식(Part Presentation)은 생산 신뢰성을 결정하는 주요 요소이다. 일정한 위치의 트레이 또는 지그를 통해 공급되는 부품은 인식과 파지 계획을 단순화하지만 무작위 자세로 공급되는 부품은 검출, 자세 추정(Pose Estimation), 경우에 따라 빈 피킹(Bin Picking)이 필요하다. 생산 아키텍처는 경제적으로 가능한 경우 기계적인 정렬과 구조화를 활용하고, 작업 셀 설계만으로 제거하기 어려운 변동에 대해서만 고도화된 인식을 적용하는 것이 바람직하다.

비전(Vision)은 일반적으로 부품의 식별, 위치, 방향, 조립 상태에 대한 전역 정보를 제공한다. 고정 카메라는 전체 작업 셀을 관찰할 수 있고, 손목 장착 카메라(Wrist-Mounted Camera)는 파지 및 삽입 영역을 근거리에서 관찰한다. 깊이 센싱(Depth Sensing)은 3차원 위치 추정을 지원할 수 있지만 고정밀 조립에서는 최종 결합 위치 근처에서 기하학적 특징, 기준 마커(Fiducial), 구조화된 지그, 국부 시각 서보(Local Visual Servoing)가 추가로 필요할 수 있다.

보정(Calibration)은 카메라 좌표계, 로봇 베이스, 도구, 그리퍼, 지그를 하나의 일관된 기하학적 모델로 연결한다. 자유 공간 이송에서는 허용 가능한 작은 보정 오차도 공차가 작은 삽입 작업에서는 실패를 유발할 수 있다. 따라서 생산 셀에서는 반복 가능한 도구 중심점 보정(Tool-Center-Point Calibration), 카메라 외부 보정(Camera Extrinsic Calibration), 지그 등록(Fixture Registration), 유지보수나 도구 교체 이후 보정 드리프트(Drift)를 감지하기 위한 절차가 필요하다.

파지 계획(Grasp Planning)은 최초 픽업만 고려하는 것이 아니라 전체 조립 순서를 고려해야 한다. 운반 중 안정적인 파지라도 삽입 부위를 가리거나 두 번째 팔과 간섭하거나 체결 도구의 접근을 방해할 수 있다. 작업 인식형 파지 선택(Task-Aware Grasp Selection)은 각 파지가 후속 위치 결정, 협동 조작, 힘 전달, 검사, 최종 해제를 지원할 수 있는지를 평가한다.

대략적인 위치 결정(Coarse Positioning)과 정밀 조립(Fine Assembly)은 일반적으로 서로 다른 제어 전략을 사용해야 한다. 동작 계획(Motion Planning)을 통해 다른 팔과 작업 셀의 장애물을 회피하면서 자유 공간에서 부품을 빠르게 이동할 수 있다. 부품이 결합 영역에 접근하면 낮은 속도, 높은 센싱 민감도, 순응 제어(Compliant Control)로 전환할 수 있다. 이러한 분리는 접촉 단계의 정밀성을 유지하면서 전체 사이클 타임을 단축한다.

사전 정렬(Pre-Alignment)은 접촉 제어기(Contact Controller)가 처리해야 하는 불확실성을 줄인다. 시각 서보(Visual Servoing) 또는 기하학적 정합(Geometric Registration)을 사용하여 물리적 삽입을 시작하기 전에 구멍, 축, 커넥터, 모서리, 결합면을 정렬할 수 있다. 목표는 반드시 비전만으로 완벽한 정렬을 달성하는 것이 아니라 순응 움직임과 힘 센싱이 안정적으로 조립을 완료할 수 있는 포획 영역(Capture Region)으로 진입하는 것이다.

삽입(Insertion)은 대표적인 접촉 중심 양팔 작업(Contact-Rich Bimanual Operation)이다. 한쪽 팔이 기준 부품을 유지하고 두 번째 팔이 결합 부품을 계획된 방향으로 이동시킨다. 작은 병진 또는 각도 오차도 횡방향 힘(Lateral Force)과 걸림(Jamming)을 발생시킬 수 있다. 힘-토크 센싱(Force-Torque Sensing)과 순응 움직임을 결합하면 장애가 있는 인터페이스에 힘을 계속 증가시키는 대신 정렬 오차를 감지하고 수정할 수 있다.

임피던스 제어(Impedance Control)는 목표 궤적 주변의 기계적 순응성을 제어할 수 있기 때문에 결합 작업(Mating Operation)에 특히 유용하다. 정렬 방향에서는 낮은 강성을 적용하여 접촉 형상을 통해 부품이 스스로 위치를 보정하도록 하고, 특정 방향에서는 높은 강성을 유지하여 작업 진행을 보장할 수 있다. 강성과 감쇠(Damping)는 조립 단계, 물체 특성, 측정된 접촉 조건에 따라 변경할 수 있다.

탐색 전략(Search Strategy)은 남아 있는 정렬 불확실성으로부터 복구하는 데 사용할 수 있다. 나선형(Spiral), 래스터(Raster), 회전, 작은 진동 움직임을 적용하면서 접촉력을 모니터링하여 구멍, 슬롯, 커넥터, 기준면을 탐색할 수 있다. 부품 손상을 방지하기 위해 탐색 진폭은 제한되어야 하며, 성공적인 접촉 특징(Contact Signature)이 감지되면 탐색 동작에서 삽입 또는 체결 단계로 전환할 수 있다.

반작용력이 두 매니퓰레이터를 통해 전달되는 경우 협동 힘 제어(Cooperative Force Control)가 필요하다. 작업물을 잡고 있는 팔은 삽입 팔에서 발생하는 모든 힘에 강체적으로 저항해서는 안 된다. 물체 수준 제어(Object-Level Control)를 통해 목표 조립 자세를 유지하면서 두 팔 사이에 지지력과 상호작용력을 분배할 수 있다. 내부 힘(Internal Force)은 제품에 불필요한 하중을 가하지 않으면서 파지 안정성을 유지할 수 있는 수준으로 제한해야 한다.

체결(Fastening)은 추가적인 토크와 반작용력 요구조건을 발생시킨다. 한쪽 팔이 스크루드라이버(Screwdriver), 너트 러너(Nut Runner) 또는 유사한 도구를 사용하면 다른 팔이 도구 토크에 대해 조립체를 안정화할 수 있다. 단순히 명령된 도구 움직임만으로 성공을 가정하는 대신 도구 체결, 토크 증가, 최종 토크, 회전각, 안착 상태(Seating State)를 모니터링하여 체결 작업의 정상 완료 여부를 판단할 수 있다.

커넥터 삽입(Connector Insertion)은 또 다른 대표적인 사례이다. 전기 커넥터는 방향 정렬, 제어된 삽입력, 래치 결합(Latch Engagement), 최종 안착 확인이 필요할 수 있다. 과도한 힘은 핀을 손상시킬 수 있고 불충분한 삽입은 잠재적인 품질 문제를 발생시킬 수 있다. 위치, 힘, 비전, 경우에 따라 전기적 연속성(Electrical Continuity) 정보를 결합하여 커넥터가 목표 최종 상태에 도달했는지를 확인할 수 있다.

유연 또는 반유연 부품(Flexible or Semi-Flexible Component)은 추가적인 문제를 발생시킨다. 케이블 라우팅(Cable Routing), 개스킷 설치(Gasket Installation), 호스 연결, 하네스 조립(Harness Assembly)에서는 두 팔이 물체의 자세뿐만 아니라 형상까지 제어해야 한다. 한쪽 팔이 장력(Tension)을 유지하거나 변형 가능한 구간을 안내하는 동안 다른 팔이 삽입 또는 부착을 수행할 수 있다. 비전과 힘 피드백은 하나의 강체 자세만으로 표현할 수 없는 구성 변화를 추적해야 한다.

작업 협조기(Task Coordinator)는 인식, 움직임, 접촉, 도구 사용, 검증 상태 사이의 전환을 관리한다. 유한 상태 기계(Finite-State Machine) 또는 행동 트리(Behavior Tree)를 사용하여 정상적인 생산 흐름과 재시도 및 복구 분기를 함께 표현할 수 있다. 상태 전환은 단순한 경과 시간이 아니라 파지 확인, 정렬 신뢰도, 접촉 감지, 삽입 깊이, 체결 토크, 검사 결과와 같이 검증된 이벤트를 기준으로 이루어져야 한다.

생산 시스템은 불완전한 부품이나 불확실한 상호작용을 전체 셀의 즉각적인 정지 없이 처리해야 하므로 복구 행동(Recovery Behavior)이 필수적이다. 삽입력이 예상보다 증가하면 로봇은 후퇴하고 국부 자세를 다시 추정한 뒤 정렬을 조정하여 정의된 범위 내에서 재시도할 수 있다. 반복적인 실패가 발생하면 부품 또는 공정 상태를 분류하고 예외 처리 절차(Exception-Handling Procedure)로 제어를 전환해야 한다.

자체 충돌 회피(Self-Collision Avoidance)는 두 팔이 의도적으로 가까이 동작하는 조립 과정에서도 항상 활성 상태를 유지해야 한다. 작업별 접촉 허용(Task-Specific Contact Permission)을 통해 그리퍼 또는 공유 물체가 필요한 만큼 접근하도록 허용하면서 손목, 전완, 팔꿈치, 도구, 몸통은 계속 보호할 수 있다. 예측 충돌 검사(Predictive Collision Checking)는 두 팔과 파지된 부품을 함께 고려해야 한다. 두 팔의 미래 결합 움직임에서 간섭이 발생할 수 있기 때문이다.

사이클 타임 최적화(Cycle-Time Optimization)는 신뢰성 있는 작업 실행이 확립된 이후 수행해야 한다. 자유 공간 동작을 가속하고, 안전한 로봇 움직임과 동시에 인식을 수행하며, 한쪽 팔이 적절한 작업을 완료하는 동안 다른 팔이 다음 부품을 획득하도록 할 수 있다. 병렬화(Parallelization)는 처리량을 크게 향상시킬 수 있지만 공유 작업 공간이나 접촉과 관련된 의존 관계는 명시적으로 동기화되어야 한다.

생산 스케줄링(Production Scheduling)은 두 매니퓰레이터의 자연스러운 병렬성을 활용할 수 있다. 한쪽 팔이 다른 팔을 필요로 하지 않는 작업을 수행하는 동안 두 번째 팔은 도구를 준비하거나 부품을 검사하거나 다음 픽업 위치로 이동할 수 있다. 협조기는 어떤 작업을 안전하게 중첩할 수 있는지와 두 팔이 동일한 물체 또는 작업 공간에 의존하기 때문에 엄격한 동기화 장벽(Synchronization Barrier)이 필요한 작업을 구분해야 한다.

품질 보증(Quality Assurance)은 최종 검사 단계에서만 수행하는 것이 아니라 조립 순서 자체에 직접 통합해야 한다. 카메라는 결합 전에 방향과 부품 존재 여부를 확인하고, 힘 프로파일(Force Profile)은 삽입 품질을 나타낼 수 있으며, 체결 도구는 토크-각도 특징(Torque-Angle Signature)을 제공할 수 있다. 조립 후 비전 검사를 통해 최종 기하학적 상태를 확인할 수 있으므로 각 생산품에 대한 공정 증거(Process Evidence)를 지속적으로 수집할 수 있다.

추적성(Traceability)은 조립 과정에서 측정된 데이터를 개별 제품 또는 생산 로트(Production Lot)와 연결한다. 로봇 프로그램, 부품 식별 정보, 타임스탬프, 힘 곡선, 토크 결과, 검사 영상, 재시도 기록, 최종 상태를 제조 데이터베이스(Manufacturing Database)에 저장할 수 있다. 이러한 기록은 품질 분석, 근본 원인 분석(Root-Cause Investigation), 유지보수, 교대조 또는 제품 변형별 공정 성능 비교를 지원한다.

통계적 공정 모니터링(Statistical Process Monitoring)은 실제 실패가 빈번해지기 전에 공정 성능 저하를 감지할 수 있다. 삽입력, 파지 수정량, 정렬 시간, 재시도 빈도가 점진적으로 증가하면 도구 마모, 지그 드리프트, 보정 오차, 부품 편차, 그리퍼 성능 저하를 나타낼 수 있다. 따라서 생산 데이터는 합격 또는 불합격 판정뿐만 아니라 예지 정비(Predictive Maintenance)와 공정 조정에도 활용할 수 있다.

제품 변형(Product Variation)은 가능한 경우 파라미터화된 작업 모델(Parameterized Task Model)을 통해 처리해야 한다. 서로 다른 제품 변형도 물체 모델, 파지 자세, 삽입 깊이, 힘 임계값, 도구 설정만 다르고 동일한 조립 순서를 공유할 수 있다. 재사용 가능한 기술 로직(Skill Logic)과 제품별 파라미터를 분리하면 엔지니어링 작업량을 줄이고 혼류 생산(Mixed Production)에서 양팔 자동화의 확장성을 높일 수 있다.

디지털 트윈(Digital Twin)과 시뮬레이션(Simulation)을 이용하면 실제 시운전 전에 작업 셀의 기하 구조와 로봇 협조를 검증할 수 있다. 엔지니어는 도달 가능성, 팔 사이 충돌, 지그 배치, 사이클 타임, 후보 조립 궤적을 평가할 수 있다. 접촉 중심 공정은 세밀한 실제 검증이 필요하지만 시뮬레이션을 통해 하드웨어 시험 전에 많은 기하학적 문제와 작업 순서 문제를 제거할 수 있다.

오프라인 프로그래밍(Offline Programming) 이후에는 실제 작업 셀에서 단계적인 시운전(Progressive Commissioning)을 수행해야 한다. 초기 시험에서는 낮은 속도, 충분한 충돌 여유, 가벼운 부품 또는 시험용 부품을 사용할 수 있다. 보정, 파지, 접촉 행동이 검증되면 힘, 오차, 안전 이벤트, 부품 품질을 모니터링하면서 속도와 생산 조건을 체계적으로 높일 수 있다.

안전 아키텍처(Safety Architecture)는 정상적인 작업 성공 여부와 독립적으로 유지되어야 한다. 관절 한계, 속도 제한, 힘 임계값, 충돌 모니터링, 보호 정지(Protective Stop), 비상 기능은 인식이나 작업 로직이 실패하더라도 정상적으로 작동해야 한다. 협업 환경에서는 사람이 통제 영역에 진입하면 사람 감지(Human Detection)와 보호 분리(Protective Separation)를 통해 로봇 속도를 제한하거나 특정 작업을 정지시킬 수 있다.

성능 평가(Performance Evaluation)는 생산성과 물리적 품질을 함께 고려해야 한다. 관련 지표에는 사이클 타임, 최초 통과 수율(First-Pass Yield), 삽입 성공률, 체결 품질, 파지 성공률, 재시도율, 최대 접촉력, 충돌 회피 개입 횟수, 가동 중단 시간(Downtime), 평균 복구 시간(Mean Recovery Time)이 포함된다. 단순한 작업 완료 여부만 측정하면 빈번한 재시도나 과도한 기계적 하중에 의존하는 불안정한 공정을 발견하기 어렵다.

성숙한 생산 셀은 통제된 변경 관리(Change Management)를 지원해야 한다. 인식 모델, 로봇 궤적, 힘 임계값, 도구, 제품 파라미터에 대한 변경 사항은 배포 전에 버전 관리(Versioning)와 검증을 거쳐야 한다. 회귀 시험(Regression Testing)을 통해 대표적인 생산 조건을 반복 검증하면 특정 작업 단계의 개선이 다른 단계에서 새로운 실패, 충돌 또는 사이클 타임 증가를 발생시키지 않는지 확인할 수 있다.

궁극적으로 양팔 생산 조립 시스템(Bimanual Production Assembly System)은 두 로봇 팔, 센서, 도구, 지그, 제어기, 품질 시스템이 하나의 협조된 제조 공정(Coordinated Manufacturing Process)으로 동작할 때 성공적으로 구현된다. 목표는 단순히 인간의 양손 움직임을 모방하는 것이 아니라 상호 보완적인 로봇 능력과 구조화된 생산 엔지니어링(Production Engineering)을 결합하는 것이다. 이러한 통합을 통해 반복 가능한 품질, 복구 가능한 접촉 행동, 추적 가능한 공정 증거, 확장 가능한 자동화를 갖춘 유연한 조립 시스템을 구현할 수 있다.
