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



