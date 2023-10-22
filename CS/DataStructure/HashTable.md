# HashTable

> Dictionary: Abstract Data Type (ADT)
> 
- a set of items, each items has a key. we can insert(item), delete(item), search(key)
- 한 키에 하나의 아이템이 매칭. 기존 키에 insert하면 새로운 item으로 overwrite됨

## 사용처

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