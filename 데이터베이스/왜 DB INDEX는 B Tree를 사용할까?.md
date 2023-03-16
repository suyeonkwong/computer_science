## 왜 DB INDEX는 B Tree를 사용할까?
### AVL Tree 

![AVL tree PNG](https://user-images.githubusercontent.com/80368511/225526438-85903c74-7249-40f4-ae6a-1e6de31ed9da.png)
![AVL tree PNG](https://user-images.githubusercontent.com/80368511/225526499-abe74c7a-14e4-4f2f-8b4b-1f2f3ec164c5.png)

자가 균형 이진트리, 스스로 균형을 잡기때문에 한쪽으로 취우쳐지는 현상을 방지 할 수 있음.  평범한 이진트리처럼 보이나 10을 추가할 경우 오른쪽으로 취우치는 현상을 막기 위해 왼쪽으로 회전된 모습을 볼 수 있음. 만약 오른쪽그림에서 5에 접근하기 위해선 4번의 접근이 필요하다는것을 알 수 있음. 

### B Tree

BST이진 탐색 트리는 최대2개까지의 자식노드를 가질수 있다. 그러나 B tree는 **2개 이상의 자식노드**를 가질수 있다. 
![B tree PNG](https://user-images.githubusercontent.com/80368511/225526812-851990e5-c5ab-4e51-83bd-c5be4fbf4377.png)

M이라는 기준 을 두고 몇 차 인지 생각해보자.

M : 각 노두의 최대 자녀 노드 수

M-1 : 각 노드의 최대 key 수

M / 2 : 각 노드의 최소 자녀 노드 수

[M / 2] -1 : 각 노드의 최소 key 수 

만약 5에 접근하기 위해 3번 접근해야함.

이진트리같은 경우 탐색범위를 최대 1/2로 줄일 수 있지만 B tree같은 경우 최대 3-5개의 자녀 노드를 가지기 때문에 탐색범위를 1/3 또는 1/5로 줄일수 있음. 즉 데이터를 찾을 때 탐색 범위를 빠르게 좁힐 수 있다. 또한 노드의 최대 block이 이진트리는 데이터 1개이지만 B tree같은 경우 한개의 노드에 여러 데이터가 저장되므로 block단위를 읽을 때도 더 많은 데이터를 가져올 수 있다. 

- DB는 기본적으로 secondary storage에 저장된다.
- B tree index는 self-balancing BST에 비해 secondary storage 접근을 적게 한다.
- B tree 노드는 block 단위의 저장공간을 알차게 사용할 수 있다.

### Hash Index

- hash table을 사용하여 index를 구현
- 삽입/삭제/조회 시간복잡도 O(1)의 성능
- 해시 테이블의 사이즈를 늘리는 rehasing에 대한 부담.
- equality 비교만 가능, range 비교 불가능
- 다중컬럼 Index의 경우 전체 attribute에 대한 조회만 가능. 일부분만 사용하는것이 불가능하다.
