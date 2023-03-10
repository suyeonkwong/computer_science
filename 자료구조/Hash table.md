## Hash table

hash table은 **빠른 탐색**을 위한 자료구조로써 key-value쌍의 데이터를  입력받는다. hash function h에 key값을 입력으로 넣어 얻은 **해시값을 h(k)를 위치로 지정**하여 
key-value 데이터 쌍을 저장한다. 저장, 삭제, 검색의 시간복잡도가 모두 O(1)이다. 

- 좋은 hash function의 조건은 상황에 따라 달라 질 수 있으나 연산 속도가 빨라야 하고, 해시값이 고르게 분포되어 최대한 해시값이 겹치지 않아야 한다.

### Direct-address Table

의미

- 직접 주소화 테이블이란, key값으로 k를 갖는 원소는 index k에 저장하는 방식이다.

단점  

- 직접 주소화 방법으로 key-value 쌍의 데이터를 저장할 경우 key가 index를 고르게 사용하지 않게 되면 **불필요한 공간이 낭비** 된다.
- key가 다양한 자료형을 담을 수 없게 된다.

### Hash table

key-value 데이터 쌍을 저장하기 위해 직접 주소화 방법은 적합하지 않다. hash table은 hash function h를 이용해서 key-value를 index h(k)에 저장한다. 
이 때, ‘키 k값을 갖는 원소가 위치 h(k)에 hash된다’ 또는 ‘h(k)는 키 k값의 해시값이다.’라고 표한한다. key는 무조건 존재해야 하며, 중복되는 key가 있어서는 안된다. 
hash table을 구성하는 key-value데이터를 저장할 수 있는 각각의 공간을 slot또는 bucket이라고 한다. 

### Collision

**서로 다른 key의 해시값이 똑같을 때**를 말한다. 중복되는 key는 없지만 해시 값이 중복 될 수 있는데 이 때 collision이 발생했다고 한다. 
따라서 collision이 최대한 적게 나도록 hash function을 잘 설계해야 한다.

### 시간복잡도와 공간효율

- 시간복잡도는 저장, 삭제, 검색 모두 O(1)이지만, collistion이 발생할 경우 최악으로 O(n)이 될 수 있다.
- 공간효율성은 떨어지는데, 데이터가 저장되기 전에 미리 저장공간(slot, bucket)을 확보해야 한다. 따라서 저장공간이 부족하거나 채워지지 않은 부분이 많은 경우가 생길 수 있다.

### Collision해결

### **Separate chaining**

***linked list***를 이용하여 collision을 해결한다. 충돌이 발생하면 linked list에 노드(slot)를 추가하여 데이터를 저장한다. n개의 모든 key가 동일한 해시값을 갖게 되면 
길이 n의 linked list가 생성되며 이때의 검색의 시간복잡도는 O(n)이다. separate chaining은 기본적으로 linked list를 이용하여 데이터를 저장하지만 충돌이 많이 발생하여 
linked list의 길이가 너무 길어지면 BST 자료구조를 이용하여 데이터를 저장하기도 한다. 따라서 최악의 시간복잡도를 O(n)에서 O(*log*n)으로 낮출 수 있다.

- 삽입 : 서로 다른 두key가 같은 해시 값을 갖게 되면 linked list에 node를 **추가**하여 key-value를 저장한다. 삽입의 시간복잡도는 O(1)이다.
- 검색 : 기본적으로 O(1)의 시간복잡도이지만 최악의 경우 O(n)의 시간복잡도를 가진다.
- 삭제 : 삭제를 위해 검색을 먼저 해야하므로 검색의 시간복잡도와 동일하다. 기본적으로 O(1)의 시간복잡도이지만 최악의 경우 O(n)의 시간복잡도를 가진다.
    
### **Open addressing**

collision이 발생하면 미리 정한 규칙에 따라 hash table의 비어있는 solt를 찾는다. 추가적인 메모리를 사용하지 않으므로 linked list또는 tree자료 구조를 통해 
추가로 메모리를 할당하는 separate chaining방식에 비해 ***메모리를 적게 사용***한다. 

- Linear Probing(선형 조사법) : 충돌이 발생한 해시값으로 부터 일정한 값만큼 (+1, +2, +3…) 건너 뛰어, 비어 있는 slot에 데이터를 저장한다.
- Quadratic Probing(이차 조사법) : 제곱수 (+1^2, +2^2, +3^2, …)로 건너 뛰어, 비어 있는 slot에 저장한다.

충돌이 여러번 발생하면 여러번 건너 뛰어 빈 slot을 찾는 방식인데 두 방식 모두 충돌 횟수가 많아지면 특정 영역에 데이터가 집중적으로 몰리는 클러스터링 현상이 발생한다. 
클러트링 현상이 발생할 경우 평균 탐색 시간이 증가한다.

- Double Hashing(이중해시) : 이중해싱은 probing하는 방식이다. 선형조사법이나 이차 조사법은 탐사 이동폭이 같기 때문에 클러스터링 문제가 발생하는데 
클러스터링 문제를 해결하기 위해 2개의 해시함수를 사용해서 **하나는 최초의 해시값을 얻을 때 사용하고, 
또 다른 하나는 해시 충돌이 발생할 때 탐사 이동폭을 얻기 위해 사용**한다.
