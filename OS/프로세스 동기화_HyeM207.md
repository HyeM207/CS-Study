# 프로세스 동기화 (Process Synchronization)
## #️⃣ 프로세스 동작 관련 배경 지식
- 프로세스는 동시에 실행될 수 있다. 
    - 언제든 중단될 수 있고, 부분적으로 실행을 완료할 수 있다.
- 공유된 데이터에 동시 접근시, 데이터 불일치(data inconsistency) 발생할 수 있다. 
- 데이터 일관성(data consistency)를 유지하기 위해서는 프로세스의 순서된 실행을 보장하는 메커니즘이 필요하다.

<br>


# 1️⃣ Race Condition
> 공유 자원에 대해 여러 프로세스가 동시에 접근할 때, 결과값에 영향을 줄 수 있는 상태  

여러개의 프로세스가 동일한 데이터에 대해 <B>동시에 접근</b>하거나 조작하여, `실행 결과가 접근 순서에 따라 바뀌는 것`을 Race Condition이라고 한다.   
그렇기에 한 타임에 오로지 한 프로세서만 접근/조작하도록 하도록 보장해야된다. 

### [ Race Condition이 발생하는 경우 ]
1. kernel 작업 수행 중 인터럽트 발생 
    - 문제점 :  kernel 모드에서 데이터를 로드하여 작업 수행하다가 인터럽트가 발생하여, 작업중이던 같은 데이터를 조작하는 경우 문제가 된다.
    - 해결법 : Kernel모드에서 작업을 수행하는 동안, `인터럽트를 disable`하여 CPU 제어권을 가져가지 못하게 한다.

2. 프로세스가 `'System Call'`하여 커널 모드로 진입하여 작업을 수행하는 도중, `Context Switching`이 발생할 때 
    - 문제점 : 프로세스 p1이 커널모드에서 데이터 조작 도중, <u>시간이 초과되어</u> cpu 제어권이 프로세스 p2로 넘어가 같은 데이터를 조작하는 경우 문제가 된다. 이러면 p2가 작업에 반영되지 않는다.
    - 해결법 : 프로세스가 커널 모드에서 작업을 하는 경우 `시간이 초과되어도 cpu 제어권이 다른 프로세스에 넘어가지 않도록` 해야한다. 

3. 멀티 프로세서 환경에서 공유 메모리 내의 커널 데이터 접근할 때
    - 문제점 : 멀티 프로세서 환경에서 `cpu 2개가 동시에 커널 내부 데이터에 접근하여 조작`하는 경우
    - 해결법 : 커널 내부에 있는 각 공유데이터에 접근할 때 마다, 그 데이터에 대한 `lock/unlock`을 한다.

<br><br>


# 2️⃣ Critical Section Problem (임계 구역 문제)
각각의 프로세스는 `'critical section'(임계 구역)'`라고 하는 코드 세그먼트가 있다.
- 'critical section'는 프로세스는 한 개 이상의 다른 프로세스와 공유되는 데이터에 접근하고 수정할 수 있는 공간이다. 
- 한 프로세스가 'critical section'에서 실행될 때, 다른 프로세스는 해당 구역에서 실행될 수 없다.  
    (즉, 'critical section'에서는 두개의 프로세스가 동시에 실행되지 않는다)  
<Br>

<b>'Critical section problem'은 프로세스간에 데이터를 협력적으로 잘 공유하기 위해 활동을 동기화하는 프로토콜을 설계하는 것을 의미한다. </b>
- 각각의 프로세스는 entry section에서  critical section 진입하기 위해서 permission이 있어야하며, 'critical section' -> 'exit section' -> `remainder section` 을 순서로 들어갈 수 있다. (아래 코드 블록 참고)

```
// 일반적인 프로세스 구조

while (true) {
    'entry section'
        critical section
    'exit section'
        remainder section
}
```

<br><br>

# 3️⃣ Solution to Critical Section Problem (임계 구역 문제 해결책)
### [ 임계 문제 해결책의 필수 조건 3가지 ]
임계 문제 해결책은 아래의 3가지 조건은 모두 만족해야 된다.
1. `Mutual Exclusion`
    - 프로세스 p1이 critical section에서 실행 중이라면, <u>다른 프로세스는 해당 구역에서 실행될 수 없다.</u>
2. `Progress`
    - critical section에서 실행중인 프로세스가 없고, critical section에 들어가길 원하는 다른 프로세스들이 있다면, <u>critical section에 들어갈 프로세스를 선택하는 과정을 미룰 수 없이 꼭 선택해야한다. </u>
3. `Bounded Waiting`
    - 프로세스가 critical section 진입을 요청한 후, permission을 받기 전까지 <u>다른 프로세스가 critical section에 진입하는 것을 허용하는 횟수에 경계</u>가 존재해야한다.  
        - 각 프로세스는 수행 속도가 0이 아닌 상태로 실행된다는 가정이다.
        - 프로세스들에 대한 상대적인 속도는 가정하지 않는다. 

### [ 종류 ]  
임계 구역 문제의 해결책은 크게 하드웨어적 솔루션과 소프트웨어적 솔루션을 나눌 수 있다. 하드웨어적 솔루션은 복잡하고 일반적으로 애플리케이션 개발자가 액세스할 수 없어서, 소프트웨어 차원의 mutex나 semaphore등이 쓰이고 있다. 

<br><Br>

## 3️⃣-1. Mutex (뮤텍스)
> critical section을 가진 스레드들의 실행시간이 서로 겹치지 않고 각각 단독으로 실행되게 하는 기술

`mutex`는 mutual exclusion의 약자로, mutext lock은 <u>critical section 보호와 race condition을 예방하기 위한 방법</u>이다.

접근 조율을 위해 `lock`과 `unlock`을 사용한다. 
- lock : 현재 critical section에 들어갈 권한을 얻어옴
    - 만약 다른 프로세스/스레드가 해당 섹션에서 실행 중이면 종료할 때까지 대기함
- unlock : 현재 critical section을 모두 사용했음을 알림 
    - 대기 중인 다른 프로세스/스레드가 해당 구역에 진입할 수 있게됨 

코드상으로 표현하면 다음과 같다.
```
   acquire() {
        while (!available) 
          ; /* busy wait */ 
        available = false; 
    } 
   release() { 
       available = true; 
    } 
    do { 
    acquire lock
       critical section
    release lock 
      remainder section 
 } while (true); 
```
mutex lock 은 'available'이름의 boolean 변수에 lock이 가능한지 불가능한지를 저장하고, acquire()함수와 release()함수에 값이 변경된다.  
- acquire 함수는 lock을 요청하고, release는 lock을 해제할 것을 요청하는 함수이다.
<br><Br>

## 3️⃣-2. Semaphore (세마포어)
> 멀티 프로그래밍 환경에서 공유 자원에 대한 접근을 제한하는 방법  
### ✅ 정의 및 이론 
mutex보다 더 정교화되게 프로세스 활동을 동기화 방법이다. 

semaphore s는 integer 변수로, 두개의 나눌 수 없는(atomic) 연산인 wait와 signal이 있다. 보통 각각을 P()와 V()라고 부른다.

- S : Semaphore; interger 변수로, 자원 개수
- P() : (= wait()) 임계구역 들어가기 전에 수행 
    - 프로세스 진입 여부를 자원 개수(S)를 통해 결정 
- V() : (= signal()) 임계구역에서 나올 때 수행 
    - 자원 반납 알림, 대기 중인 프로세스를 깨우는 신호


```
wait(S) { 
    while (S <= 0)
       ; // busy wait
    S--; // 자원 사용 
}

signal(S) { 
    S++; // 자원 해제
}
```
<br>

### ✅ 세마포어 이용
OS는 counting 과 binary 세마포어를 구분한다. 
1. `counting semaphore` 
    - 정해진 수의 인스턴스에 대한 접근 권한을 조절하기 위해 사용함
    - <b>초기화는 자원 수로 되고, <u>리소스를 사용할 프로세스는 wait()함수를 호출하고, 자원을 해제할 때는 signal()함수</u>를 호출</b>한다.
    - 만약 semaphore 변수가 0이 되면, 모든 자원이 사용 중임을 의미한다.   
    이때 자원 사용을 대기중인 프로세스는 semaphore 변수값이 0이상이 될때까지 block되어 대기상태를 유지한다. 
2. `binary semaphore (이진 세마포어)`
    - 오직 0과 1만 가지고 있어, `mutex 와 유사하다`

<br><br>

# 4️⃣ liveness failure
critcal section 접근 조정을 위해 <u>뮤텍스와 세마포어와 같은 동기화 도구를 사용함으로써, 임계 구역에 진입하려는 프로세스가 무한정 대기할 가능성이 생겨 `liveness failure`를 초래</u>할 수 있다.
 
- 무한 대기는 '임계 문제 해결책의 필수 조건 3가지' 중 `progress`와 ` bounded-waiting` 을 위반한다. 

- `Liveness` 는 시스템이 실행 주기 동안 프로세스가 진행되도록 보장하기 위해 충족해야 하는 일련의 속성으로, 무한정 대기 하는 상황은 `liveness failure`의 한 예시이다. 

- `liveness failure` 사례로는 크게 `Deadlock`과 `Priority Inversion`가 있다.
    - `Deadlock` : <u>2개 이상의 프로세스가</u> 대기 프로세스 중 <b>하나만 발생할 수 있는 이벤트를 무한 대기</b>하는 상황
    - `Priority Inversion` : 우선순위 낮은 프로세스가 우선순위가 더 높은 프로세스에 필요한 lock을 유지하는 경우 발생하는 스케쥴링 문제


<br><br>

# 5️⃣ 주요 개념 정리
### ⭐ 뮤텍스, 세마포어가 뭔지, 차이점은?
> 뮤텍스 

뮤텍스(mutext)란 mutual exclusion의 약자로, 상호배제를 의미한다.  
임계 구역인 critical section에 대해 `lock`과 `unlock`함수를 이용하여 <u>하나의 프로세스만 접근 허용하는 방식이다. </u>  

lock을 통해 해당 제한 구역에 들어갈 권한을 얻고, unlock을 통해 구역 권한을 반납하는데, 이는 해당 섹션에 접근하려는 다른 스레드들을 강제적으로 막음으로써 첫번재 스레드가 해당 섹션을 빠져나올 때까지 기다리는 구조이다.  


> 세마포어

세마포어(semaphore)란, 운영체제에서 <b>공유 자원에 대한 접속을 제어하기 위해 사용되는 동기화 기법</b> 중에 하나로, 공유 자원에 접근할 수 있는 최대 허용치만큼 프로세스가 동시에 접근가능하다.  

스레드들은 리소스 접근 요청을 할 수 있는데, <u>counting semaphore의 경우 정수형 변수를 이용하여 초기화는 자원수로 한다.   
해당 변수는 리소스 사용시 카운트가 하나씩 줄고, 리소스 반납시엔 카운트가 하나씩 증가한다. 만약 리소스가 모두 사용중인 경우라면, 즉 카운트가 0인 경우엔 다음 작업은 대기를 하게 되는 방식이다. </u>


> 차이점  

- `세마포어는 뮤텍스가 될 수 있지만, 뮤텍스는 세마포어가 될 수 없다.`
    - (뮤텍스는 이진 세마포어와 같음)
- `뮤택스는 자원 소유가 가능하지만, 세마포어는 소유 불가능하다.` 
    - 뮤텍스는 한 프로세스에 의해 소유될 수 있는 <b>Key를 기반으로 한 상호배제 기법이고 Key에 해당하는 어떤 객체(Object)가 있으며, 이 객체를 소유한 스레드/프로세스만이 공유자원에 접근할 수 있다 </b>
- `뮤텍스는 동기화 대상이 오직 1개일 때 사용하며, 세마포어는 동기화 대상이 1개 이상일 때 사용한다.`
    - 동기화의 개수가 다름 (뮤텍스는 1개, 세마포어는 설정한 자원수만큼 가능함)

- `세마포어는 <u>시스템 범위</u>에 걸쳐 있고, 파일 시스템 상의 파일로 존재한다. 반면, Mutex는 <u>프로세스의 범위</u>를 가지며 프로세스 종료될 때 자동으로 Clean up 된다.`

<br><Br>

### 참고 
[공룡책_Operating System Concepts-3rd Edition](https://os.ecci.ucr.ac.cr/slides/Abraham-Silberschatz-Operating-System-Concepts-10th-2018.pdf)

[공룡책_Operating System Concepts-9th Edition_ch5 슬라이드](https://www.os-book.com/OS9/slide-dir/index.html)

[gyoogle의 Tech Interview 중 OS 파트](https://gyoogle.dev/)

[프로세스 동기화 관련 정리 블로그](https://systorage.tistory.com/entry/CS-%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-%EB%8F%99%EA%B8%B0%ED%99%94) 

[뮤텍스와 세마포어 차이 정리 블로그](https://heeonii.tistory.com/14)