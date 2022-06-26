# Clean Code - 8. 경계

Created: June 17, 2022
Created by: Anonymous
Tags: Clean Code

- 시스템에 들어가는 모든 소프트웨어를 다 직접 만들지는 않는다. 그렇다면 어떤 방식으로 소프트웨어를 작성하고 오픈소스나 패키지를 활용하는게 좋을까?

## 외부 코드 사용하기

- Before
    
    ```java
    Map<String, Sensor> sensors = new HashMap<Sensor>();
    ...
    Sensor s = sensors.get(sensorId);
    ```
    
- After
    
    ```java
    public class Sensor {
    		// 제네릭을 활용한 형변환
        private Map sensors = new HashMap();
    
        public Sensor get(String sensorId) {		
            return (Sensor) sensors.get(sensorId);
        }
    }
    ```
    
    - 이런 경우엔 Map interface가 변경되더라도 다른 코드들이 영향을 받지 않는다. Why? Map을 sensors 안으로 숨겼기에 가능!
    - **Map을 여기저기에 넘기지 않도록 작성하는 것이 포인트!**
    
    ## 경계 살피고 익히기
    
    - 외부 라이브러리를 활용하려고 할 적에 대개 문서를 기반으로 분석하여 적용하려고 한다.
    - ***학습 테스트*** 작성으로 프로젝트에서 사용하려는 방식대로 외부 API 호출을 해보면서 알아가기. → 외부 코드를 익히는 데 도움이 된다.
        - e.g.) log4j: 아파치 로깅 라이브러리
            - 여러 테스트 케이스 코드를 작성하며 활용법과 동작 원리 익히고 어떻게 설계할지를 감을 잡을 수 있음.
    - 학습 테스트는 새로운 패키지가 나올 때마다 돌려보는게 좋음. Why? 우리가 알고 있는 방식대로 동작하는지 점검할 수 있다. → 학습 테스트가 있다면 좀 더 수월하게 새로운 패키지 버전으로 이전할 수 있음.
    
    ## 아직 존재하지 않는 코드 사용하기
    
    - 이거 뭔가 현재 CMS 프로젝트 진행 방식과 유사해 보인다…!실제로 가상의 기획을 기반으로 예측하며 작업중인지라!

![Fake Transmitter 클래스로 테스트할 수 있도록 설계하기. Interface로 입력 받아서 코드에서 통제할 수 있도록 만들기.]![IMG_1555](https://user-images.githubusercontent.com/15176192/175812776-f23fcb80-23b7-426f-859e-fe5df6357916.jpg)

Fake Transmitter 클래스로 테스트할 수 있도록 설계하기. Interface로 입력 받아서 코드에서 통제할 수 있도록 만들기.

## 깨끗한 경계

- 외부 패키지의 직접적인 사용을 자제하고 경계를 감싸거나 Adapter 패턴을 사용해 우리가 원하는 인터페이스를 패키지가 제공하는 인터페이스로 변환하자!