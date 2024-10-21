

# SOLID
- 객체지향으로 프로그래밍을 개발할 때 지켜야 하는 5가지 원칙
- 앞 글자를 따서 SOLID라고 지었음
  
1. S - Single Responsibility Principle
   - A class should have one, and only on reason to change
   - 하나의 액터에 의해서만 변경되어야 한다. 
3. O - Open-Closed Principle
   - 확장에는 열러있고 변경에는 닫혀있다.
   - 변경하게 되면 연관된 객체들까지 다 변경해야하므로 추가하는 방식으로 변경사항을 반영해라
5. L - Liscov Substitution Principle
   - 서브타입은 언제나 슈퍼타입으로 치환될 수 있다.
   - 슈퍼타입의 정의를 거스르지 마라.
7. I - Interface Segregation Principle
   - 사용되지 않는 메서드는 인터페이스를 분리하라.
9. D - Dependency Inversion Principle
    - 구체적인 객체가 일반적인 객체에 의존하게 하지 마라
    - 상대적으로 변경되기 쉬운 객체를 의존하면 같이 변경되어야 할 일이 많을 수 있다.
  

# Clean Code
- 가독성이 높은 코드
- 이해하기 쉬운 네이밍, 오류,중복 없는 코드와 같은 방법이 존재

## 리팩터링 (refactoring)
- 외부 동작은 그대로 둔 채 내부 구현을 가독성이 좋게 정리해 나가는 것
- 테스트가 깨지지 않아야 한다.
- 리팩터링을 통해 클린 코드로 나아가는 것
