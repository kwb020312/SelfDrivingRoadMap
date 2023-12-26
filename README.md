# 🚗SelfDriving

자율주행이 가능한 로드맵을 구현하기
AI를 활용하여 장애물을 탐지하기

---

### 🔀Graph

핵심 개념인 Graph를 우선 구현

Class로 구현되어 `graph`, `segment(선)`, `point(점)`으로 이루어져있음

![image](https://github.com/kwb020312/SelfDrivingRoadMap/assets/46777310/991c9e77-a37b-43ee-928b-a8750e115f9b)

마우스 이벤트로 핸들링 하도록 변경

핵심 코드는 최단거리 검색 알고리즘으로 세그먼트 연결 및 포인트 드래그(이동)에 사용되었음

```javascript
function distance(p1, p2) {
  return Math.hypot(p1.x - p2.x, p1.y - p2.y);
}
```

내장 함수인 `Math.hypot()`을 사용하였음

---

### 🎞Road

Graph의 Segment 중 Intersection된 Segment는 그려져선 안된다. 그림으로 보자.

![image](https://github.com/kwb020312/SelfDrivingRoadMap/assets/46777310/cca573f1-2ac0-41e8-be9e-359c80457655)

즉 푸른 배경의 Polygon내부에 위치한 Segment를 제거하기 위해 다음과 같은 알고리즘을 사용하였다.

![image](https://github.com/kwb020312/SelfDrivingRoadMap/assets/46777310/c523e679-c7b8-4a05-830d-393bf246eb9c)

특정 Point에서 Polygon을 통과하는 직선을 그렸을 때, Outline에 짝수 회 걸치면 외부에 있음으로 판단하고, 홀수 회 걸치면 내부에 있음으로 판단한다.

![image](https://github.com/kwb020312/SelfDrivingRoadMap/assets/46777310/5623c112-ce7a-4f71-a6e9-25fa48b192f2)
