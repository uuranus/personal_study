# Collection 함수형 API

분류: API

## Collection

- 코틀린은 자바의 Collection Framework를 사용
- 데이터를 효율적으로 관리할 수 있는 자료구조들을 모아놓은 라이브러리
    - Queue, List, HashMap, 등등이 존재

## 함수형 컬렉션 API

- 함수형 프로그래밍 스타일로 컬렉션을 다루는 함수이다.
- API의 결과값으로 새로운 컬렉션을 리턴한다.

## filter,map

- 대부분의 컬렉션 연산을 표현할 수 있다.

filter (원소 제거)

```kotlin
data class Person(val name: String, val age: Int)

val list = listOf(1,2,3,4)
println(list.filter { it % 2 == 0 } 
//[2,4]

val people = listOf(Person("Alice", 29), Person("Bob", 31))
println(people.filter { it. age > 30 }
//[Person(name=Bob, age=31)]
```

filter는 기존 요소를 변형할 수 없다.

map (원소 변환)

```kotlin
val list = listOf(1,2,3,4)
println(list.map { it * it }
//[1,4,9,16]

val people = listOf(Person("Alice", 29), Person("Bob", 31))
println(people.map { it.name }
println(people.map(Person::name)) // 멤버 참조
//[Alice, Bob]
```

연쇄호출도 가능하다

```kotlin
people.filter { it.age > 30 }.map(Person::name)
```

map의 경우에는 Key, Value 따로따로 존재한다.

```kotlin
val numbers = mapOf(0 to "zero", 1 to "one")
println(numbers.mapValues { it.value.toUpperCase() })
//{0=ZERO, 1=ONE}
```

mapKeys,mapValues, filterKeys,filterValues 다 존재

## all, any, count, find

- 컬렉션의 모든 원소가 어떤 조건을 만족하는지 또는 그런 조건을 만족하는 원소가 존재하는지 판단하는 연산

**all** (모든 원소가 만족하는지) 

```kotlin
val canBeInClub27 = {p: Person -> p.age <= 27 }

val people = listOf(Person("Alice", 29), Person("Bob", 31))
println(people.all(canBeInClub27)
//false
```

any의 술어를 !붙인 것과 동일하다

```kotlin
println(people.any{ p:Person -> p.age > 27 })
```

**any** (어떤 조건을 만족하는 요소가 하나라도 있는지) 

```kotlin
println(people.any(canBeInClub27)
//true
```

!all과 동일하다. !를 놓치기 쉽기 때문에 비추

```kotlin
println(!people.all(canBeInClub27)
//true
```

**count** (어떤 조건을 만족하는 원소들의 개수)

```kotlin
println(people.count(canBeInClub27)
//1
```

**find** (어떤 조건을 만족하는 첫번째 원소)

```kotlin
println(people.find(canBeInBlub27)
//Person(name=Alice,age=27)
//찾는 원소가 없으면 null
```

firstOrNull과 동일하다.

## groupBy

- 리스트를 여러 그룹으로 이루어진 맵으로 변경

```kotlin
val people = listOf(Person("Alice",31), Person("Bob", 29), Person("Carol", 31))
println(people.groupBy { it.age })
//{29=[Person(name=Bob,age=29)],
	//31=[Person(name=Alice,age=31), Person(name=Carol, age=31)]
```

## flatMap, flatten

- 중첩된 컬렉션 안의 원소를 처리

flatMap

```kotlin
val strings = listOf("abc", "def")
println(strings.flatMap { it.toList() }) //[["a","b","c"], ["d","e","f"]]
//[a,b,c,d,e,f]

data class Book(val title: String, val authors: List<String>)

val books = listOf(Book("Thursday Next", listOf("Jasper Fforde")),
		Book("Mort", listOf("Terry Pratchett")),
		Book("Good Omens", listOf("terry Pratchett", "Neil Gaiman")))

println(books.flatMap { it.authors }.toSet())
//[Jasper Fforde, Terry Pratchett, Neil Gaiman
//authors의 리스트들을 한데로 모아서 펼치고 toSet()으로 중복을 제거한다.
```

람다의 결과로 얻은 리스트를 한데 모아서 펼친다.

flatten

- 람다 없이 flatten()으로 호출한다.
- 람다를 통해서 딱히 요소를 변형해야 할 필요가 없이 평평하게 펼치기만 하면 될 때 사용한다.

```kotlin
val list = listOf(listOf(1,2,3), listOf(4,5,6))

println(list.flatten())
//[1,2,3,4,5,6]
```