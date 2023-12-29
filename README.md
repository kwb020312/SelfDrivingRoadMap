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

---

### 🏕🏞Tree & Building

사용자 시점의 원형을 중심으로 거리에 맞게 Polygon을 눕히거나 세워 3D 처럼 보이게 만드는 것이다.

중요한 함수는 워낙 많은데 핵심은 `비율`이다.

```javascript
function lerp(a, b, t) {
  return a + (b - a) * t;
}
```

위 내용에서 lerp 함수는 선형 보간법을 활용하는 함수인데, 쉽게 풀이하자면 `비율`을 반환하는 함수다.

a=10, b=20, t=0.5 라면, 15가 반환되는 형식이며 애니메이션, 그래픽, 게임 프로그래밍 등 다양한 분야에서 중간 값을 계산할 때 많이 활용된다.

사용자 시점에 따른 거리에 맞게 비율을 늘리거나 줄이게 되는 것으로 3D 처럼 보이게 만드는 것

![image](https://github.com/kwb020312/SelfDrivingRoadMap/assets/46777310/5dcd7d17-fd0e-4ec3-bff7-960695d1e254)

![image](https://github.com/kwb020312/SelfDrivingRoadMap/assets/46777310/738120cc-021a-47d2-a805-78ed3ae1e4a3)


