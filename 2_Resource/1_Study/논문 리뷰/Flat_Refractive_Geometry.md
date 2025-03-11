---
tags: #Study/springer journal, SLAM
aliases: 
-"loop closure method"
-"algorithm"
-"visual SLAM"
---



**단일 시점 카메라**(Single Viewpoint Camera)**는 카메라 렌즈를 통해 들어온 빛이 한 점, 즉 **단일 시점**에서 나온 것처럼 이미지를 형성하는 카메라 모델입니다. 이는 일반적인 카메라의 이상적인 모델로, 모든 광선이 한 점에서 시작한다고 가정합니다. 하지만 실제로는 렌즈의 **굴절**이나 다른 요인 때문에 이 가정이 항상 성립하지는 않습니다. 이 논문에서는 **평면 인터페이스**를 통해 **매질**을 관찰하는 카메라 시스템에서는 이 **SVP 모델**이 유효하지 않음을 지적하며, 이는 기하학적 오류의 원인이 될 수 있다고 설명합니다.

- 수중에서 카메라를 사용할 때, **공기-물 경계면(Air-Water Interface)**에서 **광학 왜곡(Optical Distortion)**이 발생함.
- 기존의 수중 3D 복원 기법은 **다중 뷰(Multi-view)나 구조광(Structured Light)**을 활용하지만, 물리적인 굴절 오류가 발생하는 문제가 있음.
- **Flat Interface 기반 SVP 시스템**은 **고정된 평면 경계면을 활용하여 3D 복원 정확도를 향상**시키는 방식.

