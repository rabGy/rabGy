# 리눅스 명령어 조사

## top 명령어
+ 시스템의 상태를 전반적으로 ***가장 빠르게*** 파악 가능(CPU, Memory, Process)
+ 옵션 없이 입력하면 interval 간격(기본 3초)으로 화면을 갱신하며 정보를 보여줌
+ top 실행 전 옵션
  - 순간의 정보를 확인하려면 -b 옵션 추가(batch 모드)
  - -n : top 실행 주기 설정(반복 횟수)

### + top 실행 후 명령어
  - shift + p : CPU 사용률 내림차순
  - shit + m : 메모리 사용률 내림차순
  - shift + t : 프로세스가 돌아가고 있는 시간 순
  - k : kill. k 입력 후 PID 번호 작성. signal은 9
  - f : sort field 선택 화면 -> q 누르면 RES순으로 정렬
  - a : 메모리 사용량에 따라 정렬
  - b : Batch 모드로 작동
  - 1 : CPU Core별로 사용량 보여줌
### + top -b -n 1
![image](https://user-images.githubusercontent.com/106803178/171900382-691d3753-5040-4d8f-abe7-8d5a8ccc85f6.png)

+ 3:58 : 3시간 58분 전에 서버가 구동
+ load average : 현재 시스템이 얼마나 일을 하는지를 나타냄. 3개의 숫자는 1분, 5분, 15분 간의 평균 + 실행/대기 중인 프로세스의 수. CPU 코어수 보다 적으면 문제 없음
+ Tasks : 프로세스 개수
+ KiB Mem, Swap : 각 메모리의 사용량
+ PR : 실행 우선순위
+ VIRT, RES, SHR : 메모리 사용량 => 누수 check 가능
+ S : 프로세스 상태(작업중, I/O 대기, 유휴 상태 등)

----------------------------

## ps 명령어
+ 현재 실행중인 ***프로세스 목록과 상태***를 보여줌
+ ***Unix, BSD, GNU***에 따라 결과가 다르게 나타남

+ Unix Option : 앞에 '-' (dash)가 붙는 옵션 표기방법 
+ BSD Option : '-' 를 붙이지 않음
+ GNU Option : 명령어 앞에 '--' (double dash)를 붙임

### 기본 ps 명령어 구성
+ ps -e, -A : 모든 프로세스를 보여줌 
+ ps -a : 세션 리더와 터미널과 연관된 프로세스들을 제외한 모든 프로세스를 보여줌
+ ps -d : 세션 리더를 제외한 모든 프로세스를 보여줌
+ ps -f : full format으로 세션의 정보를 표시함 
+ ps -ef : -e와 -f의 옵션 조합인데, 모든 프로세스를 full format으로 보여줌 
![image](https://user-images.githubusercontent.com/106803178/171902418-775aa211-ef7e-4b2f-8e14-5704551375fe.png)
↑ ps -ef의 결과 1
![image](https://user-images.githubusercontent.com/106803178/171902862-34eb6517-0695-491f-9f2c-33e16e0572a6.png)
↑ ps -ef 결과 2

+ ps -u userlist : EUID 혹은 유저 이름으로 프로세스를 고름. 이때 여러 uid를 줄수 있는데 ','(comma)로 구분하여 명시해줌. euid는 프로세스가 수행할때 갖는 유저 권한을 말함.
+ ps -U userlist : -u 옵션과는 동일하나 RUID가 갖는 프로세스만을 찾아냄. ruid는 real user id라는 것으로 실제 프로그램을 실행한 uid를 의미함. 이때도 쉼표로 여러 uid를 지정할 수 있슴.
+ ps -p pidlist : 프로세스 id가 일치하는 프로세스를 출력함. 여러 pid들을 뽑아내고 싶다면 마찬가지로 ','(comma)로 pid를 구분하여 명시해줄 수 있음. 이 명령은 ps --pid pidlist와 같음.
+ ps --ppid pidlist : 부모 프로세스 id와 일치하는 프로세스를 출력함. 역시 여러 ppid를 ','(comma)로 구분가능함.
+ ps -t ttylist : tty와 일치하는 프로세스들을 출력해줌. 이 명령은 t 혹은 --tty 옵션과 같음. 
+ ps -o format : 사용자가 지정한 format대로 출력함. format에 대해서는 설명이 길지만 간략하게 원하는 column만 보여준다고 기억하시면 됨. 예를 들어 사용자가 임의로 pid, ppid, cmd, uid 등을 표시할 수 있음.
+ ps aux : BSD 문법으로 실행중인 모든 프로세스를 나타냄. ps -aux와는 다른 옵션

### ps | grep - 원하는 프로세스만 추출
+ ps -ef | grep sshd
![image](https://user-images.githubusercontent.com/106803178/171904468-7c20c6da-e874-4abd-bb3b-62aea81e87ad.png)
+ pstree : 트리 형식으로 실행중인 프로세스를 보여줌.

--------------------

## jobs 명령어
+ **작업이 중지된 상태, 백그라운드로 진행 중인 작업 상태, 변경 되었지만 보고되지 않은 상태** 등을 표시하는 명령어

|상태|설명|
|:--:|:--:|
|Runnig|작업이 계속 진행중임|
|Done|작업이 완료되어 0을 반환|
|Done(code)|작업이 종료되었으며 0이 아닌 코드를 반환|
|Stopped|작업이 일시 중단|
|Stopped(SIGTSTP)|SIGTSTP 시그널이 작업을 일시 중단|
|Stopped(SIGTSTOP)|SIGSTOP 시그널이 작업을 일시 중단|
|Stopped(SIGTTIN)|SIGSTTIN 시그널이 작업을 일시 중단|
|Stopped(SIGTTOU)|SIGTTOU 시그널이 작업을 일시 중단|





|옵션|설명|
|:--:|:--:|
|-l|프로세스 그룹 ID를 state 필드 앞에 출력|
|-n|프로세스 그룹 중에 대표 프로세스 ID를 출력|
|-p|각 프로세스 ID에 대해 한 행씩 출력|
|command|지정한 명령어를 실행|

------------------

## kill 명령어
+ 프로세스에 특정한 signal을 보내는 명령어
+ 일반적으로 ***종료되지 않는 프로세스***를 종료 시킬 때 많이 사용함

### 사용 예
+ kill [옵션 or 시그널(번호 또는 이름)] PID
$ kill -9 1234
$ kill -SIGKILL 1234

### Signal의 종류
![rhkw](https://user-images.githubusercontent.com/106803178/171917212-71ee0da2-3482-4c00-b40d-730e96310694.PNG)

-------------------------
# vim 에디터 매크로 사용 방법

1. 매크로 시작= q[name]
2. ....(매크로 입력)
3. 매크로 종료 = q
4. 매크로 실행 = @[name]
5. 매크로 여러번 실행 = [number]@[name] 

+ :u 작업 되돌리기
