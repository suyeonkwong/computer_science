### process란?

- 의미 : 실행파일 형태로 존재하던 **program이 memory에 적재되어 CPU에 의해 실행되는 것**을 process라고 한다. 
프로세스는 Code, Data, Stack, Heap 4개의 영역으로 나누어진 memory를 각 **프로세스마다 독립적으로 할당** 받는다.

**Memory 적재**

- code : 실행한 프로그램의 코드가 저장되는 영역.
- data : 전역 변수와 static변수가 저장되는 영역.
- heap : 직접 공간을 할당/해제 하는 메모리 영역.
- stack : 함수 호출 시 생성되는 지역변수와 매개변수가 저장되는 임시 메모리 영역.

**CPU 연산과 PC register**

프로그램의 코드를 토대로 CPU가 실제로 연산을 해야만 프로그램이 실행된다고 볼 수 있다. 
어떤 코드를 읽어야 하는가를 정하는 것은 CPU내부에 있는 PC register에 저장되어 있다. **PC register는 다음에 실행될 코드의 주소값이 저장되어 있다.** 
즉, memory에 적재되어있는 process code영역의 명령어중 다음번 연산에서 읽어야할 명령어의 주소값을 PC register가 순차적으로 가리키게 되고, 
해당 명령어를 읽어와서 CPU가 연산을 수행하면 process가 실행된다. multi process 시스템에선 진행중인 process의 code영역을 PC register가 가리키다가, 
코어에 의해 다른 프로세스가 실행될 때 다른 프로세스의 code영역을 가리킨다. CPU는 PC register가 가리키는 곳에 따라 process를 변경해 가면서 명령어를 읽고 연산을 수행한다.  

### Multi Process

Multi process란 2개 이상의 process가 동시에 실행되는것을 의미한다. 

- concurrency : CPU core가 1개일때 여러 process를 짧은 시간동안 번갈아 가면서 연산을 하게 되는  시분할 시스템으로 실행되는 것이다.
- parallelism: CPU core가 여러개일 때, 각각의 core가 각각의 process를 연산함으로써 실제로 동시에 실행되는 것이다.

**concurrency는 시분할 시스템에 의해 돌아간다.**

momory의 경우에는 여러 process들이 각각의 memory영역을 차지하여 동시에 적재된다. **반면 하나의 CPU는 매 순간 하나의 process만 연산할 수 있다. 
하지만 CPU의 처리 속도가 워낙 빨라서 수 ms 이내의 짧은 시간동안 여러 process들이 CPU에서 번갈아 실행되기 때문에 사용자 입장에서는 여러 프로그램이 
동시에 실행되는 것처럼 보인다. 이처럼 CPU의 작업시간을 여러 process들이 조금씩 나누어쓰는 시스템을 시분할 시스템이라고 한다.** 

**Context And Process Control Block (PCB)**

시분할 시스템에서 process가 CPU를 점유하는 시간이 매우 짧기 때문에 다음 차례 때 이전 명령을 어디까지 수행했고 register에 어떤 값이 저장되어 있는지에 대한 정보를 알아야 한다. 이러한 정보를 context라고 하며 PCB라는 프로세스를 표현하는 형태로 사용자가 접근할 수 없는 메모리 영역안에 저장한다. PCB는 다음과 같은 정보들로 이루어진다.

- PID : 프로세스를 구분할 수 있는 고유의 번호
- Process State : 준비 / 대기 / 실행 등 프로세스의 상태를 나타내는 정보.
- Program Counter : CPU가 다음으로 실행할 명령어를 가리키는 값.
- Register
- CPU 스케줄링 정보 : 우선순위, 최종 실행시각, CPU점유시간 등에 대한 정보
- 포인터
- 프로세스 계정 정보

**Conetext switch**

한 프로세스에서 다른 프로세스로 CPU 제어권을 넘겨주는 것을 말한다. **작업 전환 시 이전의 프로세스의 상태를 PCB에 저장하여 보관하고, 
새로운 프로세스의 PCB를 읽어서 보관된 상태를 복구**하는 작업이 이루어진다. 

### Multi thread

![141115474-7a979112-1565-4b0f-81ac-16d73d0c3475](https://user-images.githubusercontent.com/80368511/216668801-f6eedf46-8b8d-4d0b-b09d-64476a6097bd.png)

thread는 process내부에서 실행되는 동작의 단위이다. 프로세스 속 스레드들은 독립적인 stack 메모리와 PC register를 가지게 되고, code, data, heap영역을 공유한다. 
Multi thread를 사용하는 이유는 하나의 process가 동시에 여러개의 task를 수행하도록 병렬처리 하기 위해서다. 

각 스레드들은 stack을 통해 독립적인 함수 호출을 하며 thread끼리의 Context Switch를 위해 code address가 저장되야 하므로 thread별로 독립적인 pc register가 필요하다. 

### Multi process vs Multi thread

<aside>
multi thread는 multi process보다 적은 메모리 공간을 차지하며 context switching시 캐시 메모리를 초기화 할 필요가 없기 때문에 작업 전환이 빠르다는 장점이 있지만 
  동기화 문제화 하나의 thread 장애로 인해 전체 thread가 종료될 위험이 있다.
</aside>

<aside>
multi process는 multi thread보다 더 많은 메모리 공간과 CPU시간을 차지하지만 하나의 process가 죽더라도 다른 process에 영향을 주지않아 안전하다.
</aside>

### Process간 통신

process는 독립적인 주소 공간을 갖기 때문에, 다른 process의 주소 공간을 참조할 수 없다. 경우에 따라 운영체제가 프로세스 간 통신 IPC(Inter Process Communication)를 제공한다. 

**공유메모리(shared memory)**

프로세스들이 일부 주소공간을 공유하며 공유한 메모리 영역에 read/write하는 방법이다. 어떤 프로세스가 커널에 공유 메모리 할당을 요청하면 커널이 메모리 공간을 할당해주며 
이후에 해당 메모리에 대한 모든 접근이 일반적인 메모리처럼 취급되기 때문에 커널의 도움 없이 각 프로세스들이 해당 메모리에 접근이 가능하여 속도가 빠르다는 장점이 있다. 
이 방식은 프로세스 간의 통신을 수월하게 하지만 동시에 같은 메모리에 접근할 때 일관성 문제가 발생하며 따라서 
프로세스들 끼리 직접 공유 메모리에 대한 동기화 문제를 책임져야 한다.  

**메시지 전달(message passing)**

커널이 send와 receive라는 두가지 연산을 통해 프로세스들 간의 메세지를 전달해주는 방식이다. 커널을 통해 데이터를 주고받기 때문에 메모리 굥유보다 속도가 느리지만, 
커널이 동기화를 제공하며 안전하는 특징이 있으므로 적은 양의 데이터를 교환하는데 유용하다. 또 구현하기 쉽다는 장점이 있다. pipe, socket, message queue등이 있다. 

### MultiProcess/MultiThread 에서 동기화 문제를 해결하는 방법

**동기화 문제**

- 동기화문제란 서로 다른 thread가 메모리 영역을 공유하는 상황에서 동일한 자원에 동시에 접근하여 의도와 다르게 다른 값을 읽거나 수정하게 되는 문제이다. 이것을 해결하기 위한 방법에는 mutex, semaphore가 있다.
- CPU는 atomic operation연산을 한다. 따라서 count++작업도 count++변수의 값 가져오기 / count 변수 값 1 증가시키기 / 변경된 count값 저장하기 와 같은 3번의 연산을 수행한다. 시분할 시스템으로 작동하는 multi thread에서 두 개의 thread가 동일한 데이터인 count에 동시에 접근하여 조작하는 상황을 가정하면, thread1에서 count++연산을 수행하고 thread2에서 count++연산을 수행하면 **실행 결과가 접근이 발생한 순서에 따라 달라질 수 있다**. 이것을 **경쟁상황**이라고 한다.

**임계 영역**

- 둘 이상의 thread가 동시에 동일한 자원에 접근하도록 하는 프로그램 코드 부분을 의미한다. 임계영역의 코드는 **원자적으로 실행**되어야 하기 때문에 thread가 임계구역에서 수행하는 동안 다른 thread는 임계구역에 들어갈 수 없어야 한다.

**Mutex**

- Mutex란 1개의 스레드만이 공유 자원에 접근할 수 있도록 하여, 경쟁 상황을 방지하는 기법이다. 공유 자원을 점유하는  thread가 lock을 걸면, 다른 thread는 unlock 상태가 될 때까지 해당 자원에 접근 할 수 없다.

**Semaphore**

- Semaphore란 S개의 thread만이 공유 자원에 접근 할 수 있도록 제어하는 동기화 기법이다. Semaphore기법에서는 정수형 변수 S(세마포)값을 자원의 수로 초기화하고, 자원에 접근할 때는 S- -연산을 수행해서 세마포 값을 감소시키고 자원을 방출할 때는 S++연산을 수행하여 세마포 값을 증가시킨다. 이 때 세마포 값이 0이 되면 모든 자원이 사용 중임을 의미하고, 이후 자원을 사용하려는 프로세스는 서마포 값이 0보다 커질때까지 block 상태가 된다.

### 교착상태(Deadlock)에 대해 설명해라.

**Dead lock**

- 둘 이상의 thread가 각기 다른 thread가 점유하고 있는 자원을 서로 기다릴 때, ***무한 대기*에 빠지는 상황**을 말한다.

**발생조건**

- 상호배제 : 동시에 한 thread만 자원을 점유할 수 있는 상황을 말하며 다른 thread가 자원을 사용하려면 자원이 방출될 때까지 기다려야한다.
- 점유대기 : thread가 자원을 보유한 상태에서 다른 thread가 보유한 자원을 추가로 기다리는 상황이다.
- 비선점 : 다른 thread가 사용 중인 자원을 강제로 선점할 수 없는 상황이며 자원을 점유하고 있는 thread에 의해서만 자원이 방출된다.
- 순환 대기 : 대기 중인 thread들이 순환 형태로 자원을 대기하고 있는 상황이다.
