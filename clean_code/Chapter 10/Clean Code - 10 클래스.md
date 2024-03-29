# Clean Code - 10. 클래스

Created: June 26, 2022
Created by: Anne Lee
Tags: Clean Code

## 클래스 체계

- 클래스를 정의하는 표준 자바 관례에 따르면, 맨 먼저 나오는 것은 변수 목록. 그 다음은 공개 함수가 나온다. 공개 함수에서 호출하는 비공개 함수가 바로 다음에 나오는 것으로 마치 하나의 기사를 읽듯이 읽혀야 한다. 정리하면, **변수 목록 → 공개 함수 → 비공개 함수**순으로 정리한다.
- 유틸리티 함수와 변수의 경우 공개하지 않는 편이 좋지만 그렇다고 반드시 공개하지 않는 것도 아니다. **테스트 코드 작성을 위해 protected로 선언하는 경우도 있다.** 이때, 함수 공개 여부는 신중하게 결정해야 한다.

## 클래스는 작아야 한다!

- 클래스의 크기는 함수와 마찬가지로 작아야 한다. 그렇다면 여기서 드는 질문은 ***과연 얼만큼 작아야 하냐***는 것이다.
- 함수는 물리적인 행의 길이, 클래스는 맡은 **책임**!
- 클래스 이름은 해당 클래스 책임을 기술해야 한다. 단순히 메서드 개수가 적다고 해서 최적인 것은 아니다!
- 여기서 중요한 포인트는 만약 작명하면서 좋은 이름이 떠오르지 않는다? 이건 클래스에 많은 책임이 부여되었기 때문이라고 봐도 무방하다! 또한, 클래스 설명은 접속어(and, but, if etc…)를 사용하지 않고 설명이 가능해야 한다.
- 클래스 설계에 있어 SOLID 원칙 중 SRP(단일책임원칙) 적용이 가장 중요하다. 근데 우리는 흔히 이걸 적용하지 않고 있다. Why? 프로그래머는 대개 **돌아가는** 프로그램에만 신경쓴다. 유지보수가 가능한 코드를 추구해야함을 잘 느끼지 못한다.
- 유지보수와 변경에 유연한 클래스를 위해선 무조건 잘게 쪼개서 작업하는게 필수! 이건 왠지 나의 Jira Ticket을 관리하는 방법과도 유사해 보이는구만…ㅎㅎㅎ
- 우리는 보통 응집도가 높은 클래스를 선호하는데 이런 클래스를 잘게 쪼개면 여러 개의 클래스가 나온다. → *그렇다면 응집도가 높은 클래스를 잘게 쪼개야 하는 시그널은 무엇일까?*
    
    ```java
    // 목록 10-5 PrintPrimes.java
    
    package literatePrimes;
    
    public class PrintPrimes {
    public static void main(String[] args) {
    
    final int M = 1000;
    final int RR = 50;
    final int CC = 4;
    final int W = 10;
    final int ORDMAX = 30;
    int P[] = new int[M + 1];
    int PAGENUMBER;
    int PAGEOFFSET;
    int ROWOFFSET;
    int C;
    int J;
    int K;
    boolean JPRIME;
    int ORD;
    int SQUARE;
    int N;
    int MULT[] = new int [ORDMAX + 1];
    ...
    }
    ```
    
- 해당 클래스를 잘게 쪼개면 3개로 나뉠 수 있는데 해당 클래스는 좀 더 서술적인 네이밍을 통해 가독성을 높혔다는 부분이 가장 큰 장점!

## 변경하기 쉬운 클래스

- 클래스를 체계적으로 정리해놓으면 시스템 변경에 위험 수반이 덜하다.
- **클래스 일부에서만 사용되는 비공개 메서드는 코드를 개선할 잠재적인 여지를 시사한다.** 👀
- 새 기능을 수정하거나 기존 기능을 변경할 때 건드릴 코드가 최소인 시스템 구조가 바람직하다. → 그렇다면 이 기준을 어떻게 측정할 수 있을까?
- 테스트가 가능할 정도로 시스템의 결합도를 낮추면 유연성과 재사용성이 높아진다!