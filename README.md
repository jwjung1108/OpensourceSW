# **OpenSource SW**

#### **리눅스 top 정리 및 설명**

##### ***top***

* 시스템의 상태를 전반적으로 가장 빠르게 파악 가능(CPU, Memory, Process)
* 옵션 없이 입력하면 interval 간격(기본 3초)으로 화면을 갱신하며 정보를 보여줌
##### ***top 실행 전 옵션***
  * 순간의 정보를 확인하려면 `-b`옵션 추가(batch 모드)
  * '-n': top 실행 주기 설정(반복 횟수)

##### ***top 실행 후 명령어***
  * shitf + p : CPU 사용률 내림차순
  * shitf + m : 메모리 사용률 내림차순
  * shitf + t : 프로세스가 돌아가고 있는 시간 순
  * k : kill.k 입력 후 PID 번호 작성. signal은 9
  * f : sort field 선택화면 => q 누르면 RES순으로 정렬
  * a : 메모리 사용량에 따라 정렬
  * b : Batch 모드로 작동
  * 1 : CPU Core별로 사용량 보여줌

##### `top`
<img width="868" alt="top" src="https://user-images.githubusercontent.com/58600616/171654016-2ea12488-74fe-4746-9099-55b6685f9f8b.png">

##### `top -b`
<img width="858" alt="top -b" src="https://user-images.githubusercontent.com/58600616/171654508-8116beb8-e222-453b-a041-f164e217c3f7.png">

---
###### ***사진을 기준으로 첫번째 줄 부터 보면된다.***
* top - 23:36:22 up **2:41** : 2시간 41분 전에 서버가 구동
* **load average : 0.12 0.03 0.01** : 현재 시스템이 얼마나 일을 하는지를 나타냄. 3개의 숫자는 1분, 5분, 15분 간의 평균 실행/대기 중인 프로세스의 수. CPU 코어수 보다 적으면 문제 없음
* Tasks : 프로세스 개수
* MiB Mem, Swap : 각 메모리의 사용량
* PR : 실행 우선순위
* VIRT, RES, SHR : 메모리 사용량 => 누수 check 가능
* S : 프로세스 상태(작업중, I/O 대기, 유휴 상태 등)

##### ***VIRT,RES,SHR***
* *현재 프로세스가 사용하고 있는 메모리*
* **VIRT**
  - 프로세스가 사용하고 있는 virtual memory의 전체 용량
  - 프로세스에 할당된 가상 메모리 전체
  - **SWAP + RES**
* **RES**
  - 현재 프로세스가 사용하고 있는 물리 메모리의 양
  - 실제로 메모리에 올려서 사용하고 있는 물리 메모리
  - **실제로 메모리를 쓰고 있는 RES가 핵심!**
* **SHR**
  - 다른 프로세스와 공유하고 있는 shared memory의 양
  - 예시로 라이브러리를 들 수 있음. 대부분의 리눅스 프로세스는 glibc라는 라이브러리를 참고하기에 이런 라이브러리를 공유 메모리에 올려서 사용

##### ***프로세스 상태***
* SHR 옆에 있는 S 항목으로 볼 수 있음
   - D : Uninterruptiable sleep. 디스크 혹은 네트워크 I/O를 대기
   - R : 실행 중(CPU 자원을 소모)
   - S : Sleeping 상태, 요청한 리소스를 즉시 사용 가능
   - T : Traced or Stopped. 보통의 시스템에서 자주 볼 수 없는 상태
   - Z : zombie. 부모 프로세스가 죽은 자식 프로세스
---

##### ***ps***

* Process Status의 약자. 현재 실행중인 프로세스 목록과 상태를 보여줌
* ps의 옵션은 전통적인 유닉스인 System V, BSD, GNU에 따라 결과와 표기법이 상이함

##### ***ps 사용법***

|옵션  |내용|
|:--:|:--:|
|-A|writes to standard output information about all processes.<br>모든 프로세스를 출력</br>|
|a(BSD계열)   |터미널과 연관된 프로세스를 출력하는 옵션 <br>보통 x옵션과 연계하여 모든 프로세스를 출력할 때 사용함</br>|
|-a|세션 리더(일반적으로 로그인 셸)을 제외하고 데몬 프로세스처럼 터미널에 종속되지 않은 모든 프로세스를 출력함|
|-e|writes information to standard output about all processes, except kernal processes. <br>커널 프로세스를 제외한 모든 프로세스를 출력해 줌</br>|
|-f|풀 포맷으로 보여준다 (Generates a full listing) <br>유닉스 스타일로 출력해주는 옵션 UID, PID, PPID등이 함께 표시됨</br>|
|-l(sys V)<br>I (BSD계열)</br>|긴 포맷으로 보여준다. (Generates a long listing)<br>프로세스의 정보를 길게 보여주는 옵션, 우선순위와 관련된 PRI와 NI값을 확인할 수 있음|
|-o 값|출력 포맷을 지정하는 옵션, 값으로는 pid, tty, time, cmd 등을 지정할 수 있음|
|-M|64비트 프로세스들을 보여줌|
|-M|프로세스들 뿐만 아니라 커널 스레드들도 보여준다.|
|-p|특정 PID를 지정할 때 사용함|
|-r|현재 실행 중인 프로세스를 보여줌|
|u(BSD계열)|프로세스의 소유자를 기준으로 출력(ps ax만 하면 USER 기준의 정보가 뜨지않음, 따라서 aux랑 같이 써줌)|
|-u|특정 사용자의 프로세스 정보를 확인할 때 사용. 사용자를 지정하지 않으면 현재 사용자를 기준으로 정보를 출력함|
|x(BSD계열)|데몬 프로세스처럼 터미널에 종속되지 않는 프로세스를 출력함. 보통 a옵션과 결합하여 모든 프로세스를 출력할 때 사용|
|-x|로그인 상태에 있는 동안 아직 완료되지 않은 프로세스들을 보여줌. 유닉스 시스템은 사용자가 로그아웃 한 후에도 임의의 프로세스가 계속 동작하게 할 수 있음 <br>그러면 그 프로세스는 자신을 실행시킨 셸이 없어도 계속 자신의 일을 수행 -> 이러한 프로세스는 일반적인 ps 명령으로 확인 할 수 없음</br> 이 때 -x옵션을 사용하면 자신의 터미널이 없는 프로세스들을 확인 할 수 있음|


##### `ps aux`
<img width="865" alt="ps aux" src="https://user-images.githubusercontent.com/58600616/171673484-76cbb203-c7db-4914-b4f9-7230190dd71b.png">

---
##### ***ps 항목***
|항목|의미|
|:--:|:--:|
|USER|BSD계열에서 나타나는 항목으로 프로세스 소유자의 이름|
|UID|SYSTEM V계열에서 나타나는 항목으로 프로세스 소유자의 이름|
|PID|프로세스의 식별번호|
|PPID|부모 프로세스 ID|
|%CPU|CPU 사용 비율의 추정치(BSD)|
|%MEM|메모리의 사용 비율의 추정치(BSD)|
|VSZ|K단위 또는 페이지 단위의 가상메모리 사용량|
|RSS|실제 메모리 사용량(Resident Set Size)|
|TTY|프로세스와 연결된 터미널|
|S, STAT|현재 프로세스의 상태 코드(S: Sys V, STAT:BSD)|
|TIME|총 CPU 사용 시간|
|COMMAND|프로세스이 실행 명령행|
|STIME|프로세스가 시작된 시간 혹은 날짜|
|C, CP|짧은 기간 동안의 CPU 사용률(C: Sys v, CP:BSD)|
|F|프로세스의 플래그|
|PRI|실제 실행 우선순위|
|NI|NICE 우선순위 번호|

---

##### ***jobs***

 * 작업의 상태를 표시하는 명령어
 * 현재 쉘 세션에서 실행시킨 백그라운드 작업의 목록이 출력, 각 작업에는 번호가 붙어 있어 kill 명령어 뒤에 '% 번호'등으로 사용할 수 있음

`jobs [옵션][작업번호]`

 * jobs 명령어는 현재 쉘 프로세스의 자식 백그라운드 프로세스들을 보여준다고 생각하면 됨

<img width="576" alt="jobs" src="https://user-images.githubusercontent.com/58600616/171676528-6e995526-e1d8-4173-810d-a53a7596c5c3.png">

---

|상태|설명|
|:--:|:--:|
|Running|작업이 계속 진행중임
|Done|작업이 완료되어 0을 반환|
|Done(cod)|작업이 종료되었으며 0이 아닌 코드를 반환|
|Stopped|작업이 일시중단|
|Stopped(SIGTSTP)|SIGTSTP 시그널이 작업을 일시 중단|
|Stopped(SIGSTOP)|SIGSTOP 시그널이 작업을 일시 중단|
|Stopped(SIGTTIN)|SIGTTIN 시그널이 작업을 일시 중단|
|Stopped(SIGTTOU)|SIGTTOU 시그널이 작업을 일시 중단|

##### ***옵션***
|옵션|설명|
|:--:|:--:|
|-l|프로세스 그룹 ID를 state 필드 앞에 출력|
|-n|프로세스 그룹 중에 대표 프로세스 ID를 출력|
|-p|각 프로세스 ID에 대해 한 행씩 출력|
|command| 지정한 명령어를 실행|

---

##### ***kill***

* 프로세스에 특정한 signal을 보내는 명령
* 보통 실행죽인 프로세스에 종료 신호를 보내며, 중지시킬 수 없는 프로세스를 종료시킬 때 많이 사용함

##### ***사용 방식***
`kill [option][-시그널번호 or -시그널이름] PID`
kill 명령 뒤에 어떤 프로세스의 PID(Process ID)를 적어주면 그 프로세스에 종료시그널을 보냄

##### ***signal_number와 이름***
|signal_number|이름|설명|
|:--:|:--:|:--:|
|1| SIGHUP(HUP)|hang up의 약자로 프로세스를 재시작시키는 시그널|
|2| SIGINT(INT)|인터럽트.실행을 중지시킨다. `[CTRL] + [C]`를 눌렀을 때 보내지는 시그널|
|3| QUIT| 실행중지|
|9| SIGKILL(KILL)|무조건 종료, 즉 강제 종료시키는 시그널|
|15| SIGTERM(TERM)|Terminate의 약자, 가능한 정상 종료시키는 시그널로 kill 명령의 기본 시그널|
|18| CONT|Continue, STOP등에 의해 정지된 프로세스를 다시 실행시킴|
|19| STOP|무조건적, 즉각적 정지}
|20| TSTP|실행 정지후 다시 실행을 계속하기 위하여 대기시키는 시그널 `[CTRL] + [Z]` 를 눌렀을 때 보내지는 시그널|

---

##### ***vim에디터에서의 매크로(Macro)***

* 특정한 움직임 또는 입력을 키에 저장함으로써, 단순하면서 반복되는 동작을 쉽고 빠르게 해주는 것을 말함

***사용 방법***
1) q + a   =>   a키에 recording 시작
2) 반복을 위한 내가 원하는 동작 <br>`print("Hello World!")`</br>
3) q       =>   recording종료
4) @a      =>   1회 실행 <br>```print("Hello World!") print("Hello World!")```</br>
5) @@      =>   방금 실행한 매크로 실행 <br> ```print("Hello World!") print("Hello World!") print("Hello World!")```</br>
6) 10@a    =>   매크로 10회 실행 <br>```print("Hello Wolrd!") print("Hello Wolrd!") print("Hello Wolrd!") print("Hello Wolrd!") print("Hello Wolrd!") print("Hello Wolrd!") print("Hello Wolrd!") print("Hello Wolrd!") print("Hello Wolrd!") print("Hello Wolrd!")```</br>
