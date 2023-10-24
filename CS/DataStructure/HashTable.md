# HashTable

> Dictionary: Abstract Data Type (ADT)
> 
- a set of items, each items has a key. we can insert(item), delete(item), search(key)
- 한 키에 하나의 아이템이 매칭. 기존 키에 insert하면 새로운 item으로 overwrite됨
- (k,v) 형식의 entries들을 k를 인덱스로 이용해서 hash table이라는 배열에 저장하는 자료구조
- k를 integer한 형태의 key로 변환하는 hash function과 변환된 hashCode를 배열의 범위 0~n-1 사이의 값으로 변환하는 compression function이 존재

<img width="792" alt="hashing 과정" src="https://github.com/uuranus/personal_study/assets/72340294/ec8cafeb-420e-4baf-8bd0-26eebc644143">

출처: Data Structures and Algorithms in Java 6th Editiion, 411p

### 사용처

- docdist
    - 단어 counting, 두 doc간 공통 단어 세기 등등
- database
- compiler & interpreters
    - 변수 이름 → 주소값과 매칭
- network
    - 라우터
    - server - socket
- substring search
    - ctrl + f
- string commonalities
- file and directory synchronization
- cryptography

## 주요 메서드

- insert
- put
- delete
- map과 동일

## Hash Function

- int형의 hashCode를 반환하는 함수. 배열의 크기에 상관없는 값을 리턴하기 때문에 음수값도 가능하다.
- collision이 최소화되게 맵핑하는 게 중요
- 해시함수 예시
    - bit representation?
    - polynominal
    - cyclic-shift
- 한 방법들이 존재
- In Java
    - hashCode는 객체의 주소값
    - equals()로 동일하게 취급되는 객체의 hashCode값은 같아야 한다.
        - equals라면, 두 객체를 Map에 insert했을 때, 동일한 키값에서 저장이 되어야 한다. 이 때, hashCode를 통해서 저장되기 때문에 동일하게 해줘야 한다.
        - if x.equals(y) then x.hashCode() == y.hashCode().

## Compression Function

- hash function이 반환한 int 값을 배열의 범위 [0,n-1]에 맞춰서 리턴해주는 함수
- division method
- the MAD mthod (multiply-add-and-divide)
- 방식이 존재

## Collision-Handling

- 아무리 좋은 hash function, compression function을 선택하여도 서로 다른 두 키값이 동일한 해시값으로 변환되어서 같은 배열의 인덱스에 저장될 수 있다.
    - 이를 collision이라고 한다.
- 이 때, 충돌된 두 키의 value를 어떻게 저장할 것인지가 collision-handling이다.
- separate chaining
- open addressing
    - linear probing
    - quadratic probing
    - double hashing
- 방법이 존재

## 시간 복잡도

O(1)

## simple approach

- direct-access table
    - integer가 key인 거대한 array로 생각
    - 문제
        - key가 integer가 아닐 수 있다.
            - 해결: prehash - key들을 non negative integer로 매핑
        - 메모리 낭비가 심함 (미리 메모리를 선점해서 빈 공간이 많다)
            - 해결: hashing - hash는 pieces로 잘라서 섞는 것 (해시 브라운 ㅋㅋㅋ)
                - 모든 키들을 small set of integers로 줄이는 것
                - 가능한 키들의 집합은 giant한데, 이를 hash function을 통해서 작은 사이즈의 hash table로 줄여서 저장할 수 있게 함 → 기존 키 집합들의 빈 공간을 최대한 줄이자.
                - collision 발생 - pigeonhole problem
                    - chaining, open addressing으로 해결 가능

### chaining

- 동일한 해시값을 가지는 기존의 다른 두 키값이 충돌될 때, item 부분을 linked list로 구현하여 두 키값의 item을 다 저장하는 방법
- 하나만 매핑되고 아이템 하나만 있는 linkedList가 되는 것
- worst case
    - 모든 키들이 다 한 해시값으로 맵핑되서 그냥 줄줄이 linked list에 연결되는 것
    - 이후 item을 insert, delete, search하는 메서드는 O(n)이 걸린다.

### Simple Uniform Hashing

- false assumption
1. uniformity
    1. random하게 해시값을 선택
2. independent
    1. 앞에 선택된 키값에 상관없이 해당 키값은 hash 키값을 random하게 선택

analysis

- expected length of chain
    - n keys, m slots → n/m
    - running time = O(1+ chain 길이)

### hash functions

1. division method
    1. h(k) = k mod m
    2. 충돌 가능성 높음
2. multiplication method
    1. h(k) = [(a*k) mod 2^w] >> (w-r), w bit
        1. a*k를 하면 2w의 길이가 된다. mod 2^w면 하위 w비트만 쓰겠다.
        2. shift right (w-r)는 상위 r비트만 쓰겠다. m = 2^r
        3. a*k 하는 과정이 hashing. 그리고 그 중 middle 값이 제일 잘 섞이는 부분
3. universal hashing (good)
    1. h(k) = [(ak+b) mod p] mod m
    2. a,b는 랜덤 숫자 (0~p-1 사이), p는 전체 키들 집합 크기보다 큰 소수
    3. worst case - 다른 키 k1,k2가 충돌될 경우는 1/m! a,b를 랜덤하게 뽑는다면
    

## table doubling

- hashtable에 m개의 슬롯을 만들어 n개의 키를 저장한다.
- 그럼 n이 커지면?? 적당한 m은 어떻게 찾는가
    - 우리는 m = Theta(n)을 원한다.
- n>m 이면, grow table
    - 새로운 사이즈 m’의 테이블만큼을 reallocate하고 rehash (new hash function이 필요)
    - O(n+m+m’)
    - m’은 어느정도가 좋은가 = 2*m
        - cost of n inserts  = O(1 +2 + 4 + 8 + .. + n) = O(n)
        - amortization에 의한 table dobling
- deletion
    - if  m== n/2, shrink table to m/2
        - slow → 2배로 늘렸다가 삭제하면 다시 줄이고 다시 삽입하면 2배로 늘리고 반복할수도 있다.
    - if m = n / 4 shrink table to m/2
        - amortized time → O(1) 왜냐하면 n≤m ≤4n

### amortization

- operation takes “T(n) amortized” if k operations take ≤ k*T(n) time
- think of meaning “T(n) on average” where average over all operations
- 즉, table doubling에 대해 k inserts take O(n) times → O(1) amortized/insert
