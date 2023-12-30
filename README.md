# 🚗SelfDriving

[자율주행이 가능한 로드맵을 구현](https://self-driving-road-map.vercel.app/)하기
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

---

### 💥Save & Load, Editor Enable & Disable

![image](https://github.com/kwb020312/SelfDrivingRoadMap/assets/46777310/093aa731-9f16-42f7-beba-e813724fb59a)

---

### 🔱Marks

정지 마크

![image](https://github.com/kwb020312/SelfDrivingRoadMap/assets/46777310/c9a6e3bb-c1ab-464e-a9c0-7ef8d0c3f920)

횡단보도

![image](https://github.com/kwb020312/SelfDrivingRoadMap/assets/46777310/af21690b-0597-4ccf-8e7d-1f1696ebb0dc)

ETC

![image](https://github.com/kwb020312/SelfDrivingRoadMap/assets/46777310/5211de6f-24f8-4314-879e-ed0dd9eebfc9)

---

### 😱AI

라이브러리를 사용하지 않고 다음과 같은 JS 코드를 사용하였다.

신경망(Neural Network)을 구현한 것이며
신경망은 머신러닝의 핵심 구성 요소로, 입력, 가중치, 편향 등을 처리하여 출력을 생성한다.
이 코드에서는 두 개의 클래스, `NeuralNetwork`와 `Level`이 정의되어 있음

`NeuralNetwork` 클래스는 신경망 전체를 표현하며, 각각의 `Level` 인스턴스를 포함 `Level`은 신경망의 한 층을 나타내며, 각 층은 입력 뉴런, 출력 뉴런, 가중치, 편향을 갖는다.

`NeuralNetwork` 클래스의 주요 메소드는 다음과 같음
- `constructor(neuronCounts)`: 신경망의 각 층에 대한 뉴런 수를 인자로 받아 신경망을 생성 이때 각 층은 `Level` 인스턴스로 생성
- `feedForward(givenInputs, network)`: 주어진 입력에 대해 신경망을 통해 출력을 생성하는 과정을 수행 이 과정은 각 층마다 입력을 받아 출력을 생성하고, 그 출력을 다음 층의 입력으로 사용하는 방식으로 진행
- `mutate(network, amount=1)`: 신경망의 가중치와 편향을 무작위로 변형하는 메소드 이는 학습 과정에서 신경망의 성능을 향상시키는 데 사용
  
`Level` 클래스의 주요 메소드
- `constructor(inputCount, outputCount)`: 각 층의 입력 뉴런 수와 출력 뉴런 수를 인자로 받아 층을 생성 가중치와 편향은 무작위로 초기화
- `#randomize(level)`: 각 층의 가중치와 편향을 무작위로 설정하는 메소드
- `feedForward(givenInputs, level)`: 주어진 입력에 대해 층을 통해 출력을 생성하는 과정을 수행 이 과정은 각 입력 뉴런과 해당 가중치를 곱한 값의 합이 편향보다 크면 출력을 1로, 그렇지 않으면 0으로 설정

자동차의 센서 입력을 신경망의 입력으로 사용하고, 신경망의 출력을 자동차의 조향 및 가속 조작으로 사용 `mutate` 메소드를 사용해 신경망을 지속적으로 학습시켜 자동차의 주행 성능을 향상시킴

```javascript
class NeuralNetwork{
    constructor(neuronCounts){
        this.levels=[];
        for(let i=0;i<neuronCounts.length-1;i++){
            this.levels.push(new Level(
                neuronCounts[i],neuronCounts[i+1]
            ));
        }
    }

    static feedForward(givenInputs,network){
        let outputs=Level.feedForward(
            givenInputs,network.levels[0]);
        for(let i=1;i<network.levels.length;i++){
            outputs=Level.feedForward(
                outputs,network.levels[i]);
        }
        return outputs;
    }

    static mutate(network,amount=1){
        network.levels.forEach(level => {
            for(let i=0;i<level.biases.length;i++){
                level.biases[i]=lerp(
                    level.biases[i],
                    Math.random()*2-1,
                    amount
                )
            }
            for(let i=0;i<level.weights.length;i++){
                for(let j=0;j<level.weights[i].length;j++){
                    level.weights[i][j]=lerp(
                        level.weights[i][j],
                        Math.random()*2-1,
                        amount
                    )
                }
            }
        });
    }
}

class Level{
    constructor(inputCount,outputCount){
        this.inputs=new Array(inputCount);
        this.outputs=new Array(outputCount);
        this.biases=new Array(outputCount);

        this.weights=[];
        for(let i=0;i<inputCount;i++){
            this.weights[i]=new Array(outputCount);
        }

        Level.#randomize(this);
    }

    static #randomize(level){
        for(let i=0;i<level.inputs.length;i++){
            for(let j=0;j<level.outputs.length;j++){
                level.weights[i][j]=Math.random()*2-1;
            }
        }

        for(let i=0;i<level.biases.length;i++){
            level.biases[i]=Math.random()*2-1;
        }
    }

    static feedForward(givenInputs,level){
        for(let i=0;i<level.inputs.length;i++){
            level.inputs[i]=givenInputs[i];
        }

        for(let i=0;i<level.outputs.length;i++){
            let sum=0
            for(let j=0;j<level.inputs.length;j++){
                sum+=level.inputs[j]*level.weights[j][i];
            }

            if(sum>level.biases[i]){
                level.outputs[i]=1;
            }else{
                level.outputs[i]=0;
            } 
        }

        return level.outputs;
    }
}
```
