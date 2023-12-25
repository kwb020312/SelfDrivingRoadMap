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
