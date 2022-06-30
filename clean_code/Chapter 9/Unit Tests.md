# Unit Test
- Isolate my code from the operating system rather than just calling the standard timing functions
- mock out those timing functions so that I had absolute control over the time.
- schedule commands that set boolean flags, and then I would step the time forward, watching
those flags and ensuring that they went from false to true just as I changed the time to the
right value.
- Once I got a suite of tests to pass, I would make sure that those tests were convenient
to run for anyone else who needed to work with the code. 
- I would ensure that the tests and the code were checked in together into the same source package.

## The Three Laws of TDD
tip of the iceberg: TDD - write unit tests first, before production code.

`Three Laws` (Cycle)  
- The tests
and the production code are written together, with the tests just a few seconds ahead of the
production code

1 You may not write production code until you have written a failing unit test.

2 You may not write more of a unit test than is sufficient to fail, and not compiling is failing.

3 You may not write more production code than is sufficient to pass the currently failing test.


## Keeping Tests Clean 
- you might decide that having dirty tests is better than having no tests
    - having dirty tests is equivalent to, if not worse
than, having no tests.
    - production code를 바꾸게 되면 dirty test code를 맞춰 변경하기 어렵다.

- Test suite 없이는 code base가 예상한대로 동작하지 않을 수 있다 = Test suite 없이는 변경한 내용이 시스템의 다른 부분을 망가뜨리지 않을 것임을 확신할 수 없다 -> 변경하기 두려워진다 -> 변경이 이점보다 단점이 많을 것 같아 production code를 깨끗하게 하는 것을 멈춘다 -> production code가 썩는다 -> In the end they were left with no tests,
tangled and bug-riddled production code, frustrated customers, and the feeling that their
testing effort had failed them

- Test code is just as important as production code. It must be kept as clean
as production code.
### Tests Enable the -ilities
- Test를 깨끗하게 유지하지 않으면 잃게 된다 -> production code를 flexible, maintainable, reusable하게 유지할 수 없다
    - Tests enable all the -ilities, 
because tests enable change.

- 코드를 얼마나 잘 작성하고 구조를 잘 만들었는지와 상관없이 Test가 없으면 모든 변경사항은 잠재적 버그다
- The higher your test coverage, the less
your fear. 
- The dirtier your tests, the
dirtier your code becomes. Eventually you lose the tests, and your code rots.
## Clean Tests
- What makes a clean test? **Readability**
    - perhaps even more important in unit tests than it is in production code. 
What makes tests readable? The same thing that makes all code readable: clarity, simplicity,
and density of expression. 
In a test you want to say a lot with as few expressions as
possible.
Notice that the vast majority of annoying detail has been eliminated. The tests get
right to the point and use only the data types and functions that they truly need. Anyone
who reads these tests should be able to work out what they do very quickly, without being
misled or overwhelmed by details.
### Domain-Specific Testing Language
The tests in Listing 9-2 demonstrate the technique of building a domain-specific language

for your tests. Rather than using the APIs that programmers use to manipulate the sys-
tem, we build up a set of functions and utilities that make use of those APIs and that

make the tests more convenient to write and easier to read. These functions and utilities

become a specialized API used by the tests. They are a testing language that program-
mers use to help themselves to write their tests and to help those who must read those

tests later on.

This testing API is not designed up front; rather it evolves from the continued refac-
toring of test code that has gotten too tainted by obfuscating detail. Just as you saw me

refactor Listing 9-1 into Listing 9-2, so too will disciplined developers refactor their test
code into more succinct and expressive forms.
### A Dual Standard
After all, it runs in a test environment, not a production environment, and
those two environment have very different needs.
There are things that you might never do in a
production environment that are perfectly fine in a test environment. Usually they involve
issues of memory or CPU efficiency. But they never involve issues of cleanliness.
```java
//from this
@Test
public void turnOnLoTempAlarmAtThreashold() throws Exception {
    hw.setTemp(WAY_TOO_COLD);
    controller.tic();
    assertTrue(hw.heaterState());
    assertTrue(hw.blowerState());
    assertFalse(hw.coolerState());
    assertFalse(hw.hiTempAlarm());
    assertTrue(hw.loTempAlarm());
}

//to this
@Test
public void turnOnLoTempAlarmAtThreshold() throws Exception {
    wayTooCold();
    assertEquals("HBchL", hw.getState());
}
```
## One Assert per Test 
- test should have one and only one assert statement.
- ex. splitting the tests as shown results in a lot of duplicate code.. We can eliminate the duplication by using some skills
### Single Concept per Test
- Perhaps a better rule is that we want to test a single concept in each test function.
- Probably the best rule is that you should minimize the number of asserts per concept and test just one concept per test function.
## F.I.R.S.T
Clean tests follow five other rules:  
`Fast` Tests should run quickly. When tests run slow, you won’t want
to run them frequently. If you don’t run them frequently, you won’t find problems early
enough to fix them easily. You won’t feel as free to clean up the code. => code
will begin to rot.

`Independent` One test should not set up the conditions for the next test. You should be able to run each test independently and run the tests in any order you like.   
When tests depend on each other,  
- the first one to fail causes a cascade of downstream failures
- making diagnosis difficult and hiding downstream defects.

`Repeatable` Should be able to run the
tests in any environment like production environment, QA environment, and on your laptop without a network. If your tests aren’t repeatable in any environment, then you’ll always have an excuse for why they fail. You’ll also find yourself unable to run the tests when the environment isn’t available.

`Self-Validating` The tests should have a boolean output. Either they pass or fail. If the tests aren’t
self-validating, then failure can become subjective and running the tests can require a long
manual evaluation.

`Timely` Written in a timely fashion. Unit tests should be written just
before the production code that makes them pass. If you write tests after the production
code, then you may find the production code to be hard to test. You may decide that some
production code is too hard to test. You may not design the production code to be testable.

## Conclusion
Perhaps Tests are even more important than the production code, because tests preserve and enhance the flexibility, maintainability, and reusability of the production code. 
- Keep your tests constantly clean. 
- Work to make them expressive and succinct. 
- Invent testing APIs that act as domain-specific language that helps you write the tests.
- If you let the tests rot, then your code will rot too. Keep your tests clean.