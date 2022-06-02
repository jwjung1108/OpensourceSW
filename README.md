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

