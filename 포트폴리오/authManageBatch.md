## 🐱 사용자 접근에 따른 권한 관리 배치 개발

프로그램별 6개월 이상 접근하지 않은 사용자의 접근 권한 삭제 및 관리 배치
1) 프로그램별 사용자 접근자 조회 및 최근 접속 날짜 업데이트
2) 90일 이상 미접속 사용자 조회 후 접근 권한 삭제 
3) 매일 18시에 돌 수 있도록 스케줄 작업 진행

<br>

## 🧑‍🤝‍🧑 제작 기간 & 참여 인원

  	-  제작 기간 : 2024.05.22 ~ 2024.06.10
  	-  참여 인원 : 1명
<br>

## ⚙️ 사용 기술 스택

<br>

| ⚙️ 기술 스택 | 👇🏻 사용 목적 |
|--|--|
| **`JAVA`** | JAVA를 이용한 로직 구현 |
| **`Spring Batch`** | 스케줄링 및 배치 개발 |      
| **`Oracle`** | 인덱스 힌트를 이용한 데이터베이스 활용 |
| **`Linux`** | 배치 모니터링, 로그 확인 및 크론탭을 이용한 스케줄링 |


<br>

## 핵심 기능
	 이 프로그램의 핵심 기능은 접근 권한 관리 자동화 프로그램입니다.
 	 특정 프로그램에 접근한지 90일이 지난 사용자를 조회하여 권한을 자동으로 삭제를 해주도록 하였습니다.
   	 리눅스의 크론탭을 이용하여 매일 18시에 배치가 돌 수 있도록 하였고 배치 후 모니터링까지 진행했습니다.

<summary><b>프로그램별 사용자 접근자 조회 및 최근 접속 날짜 업데이트</b></summary>
<div align="center" markdown="1">
	<img src="https://github.com/jsjang96/images/blob/84946fb44339b9f6cc1c0460935c5d4835cf3362/UserAccessHisMngConfig_1.PNG"/>
	<img src="https://github.com/jsjang96/images/blob/84946fb44339b9f6cc1c0460935c5d4835cf3362/UserAccessHisMngConfig_2.PNG"/>
	<img src="https://github.com/jsjang96/images/blob/84946fb44339b9f6cc1c0460935c5d4835cf3362/selectUserAccessHisMng.png"/>
	<img src="https://github.com/jsjang96/images/blob/84946fb44339b9f6cc1c0460935c5d4835cf3362/updateUserAccessHisMng.png"/>
</div>
<br>

## 설명
	 해당 Job의 Step에서 chunk 방식으로 개발을 진행했습니다. 오류가 났을 때 통째로 rollback을 할 필요가 없다고 판단했기 때문에
  	 chunk방식을 선택을 했고 건마다 commit을 찍어주었습니다.

  	 위에서 보면 reader을 통하여서 프로그램별 사용자 접근 날짜를 조회하여 writer로 업데이트를 시켜주는 자동화 프로그램입니다.  
	 옵티마이저가 인덱스를 잘 찾아서 갈 수 있도록 힌트를 주었고 각 사이트 별로 최근 접속 이력을 조회하고 업데이트를 진행했습니다.

   	 매일 18시에 이 배치가 돌 수 있도록 스케줄 잡에 등록을 하였고 리눅스를 통해 모니터링을 진행했습니다.
	
 	 배치 개발을 처음 진행했던 개발건이라 의미가 있고 공부를 많이 했던 기억이 납니다. chunk가 무엇인지, tasklet이 무엇인지
 	 그리고 배치가 어떻게 실행이 되고 메타 테이블에 어떻게 데이터가 들어가는지도 파악을 하면서 진행을 했습니다.
 	 이 개발을 통하여서 제가 배치 개발에 대한 재미를 느낄 수 있었고 새로운 것을 알아가는 것에 대한 쾌감을 느낄 수 있었습니다.
<br>
<br>

<summary><b>90일 이상 미접속 사용자 조회 후 접근 권한 삭제</b></summary>
<div align="center" markdown="1">
	<img src="https://github.com/jsjang96/images/blob/84946fb44339b9f6cc1c0460935c5d4835cf3362/UserAccessAuthMngConfig.png"/>
	<img src="https://github.com/jsjang96/images/blob/84946fb44339b9f6cc1c0460935c5d4835cf3362/UserAccessAuthMngTasklet.png"/>
</div>
<br>

## 설명
	해당 Job의 Step에서 tasklet 방식으로 개발을 진행했습니다. 데이터 정합성 때문에 오류가 났을 때 통쨰로 rollback을 해줄 필요가 있다고
 	판단을 했기 때문에 tasklet 방식을 선택을 했고 통 commit을 해주었습니다.

  	위에서 보면 DB에서 "useYn" 을 DB에서 가져와서 "1"이면 배치를 실행하고 "0"이면 SKIP을 할 수 있도록 진행했습니다.
   	이 배치를 배포를 했는데 배치를 실행하고 싶지 않으면 DB에서 "1"을 "0"으로 바꿔주면 SKIP할 수 있습니다.

	이 배치에서는 90일 이상 접속하지 않은 사용자들을 조회를 한 후에 각 프로그램에 접근할 수 있는 권한을 삭제하는 배치입니다.

	이 배치도 매일 18시에 돌 수 있도록 스케줄 잡에 등록을 하였고 리눅스를 통해 모니터링을 진행했습니다.

	이번 배치 개발을 하면서 내가 전에 배치를 개발하면서 공부했던 것들을 토대로 개발을 진행하였을 때 성취감이 있었고 
 	다른 방법으로 배치를 개발을 하니 더 재밌었고 시간 흐르는지도 모른채 개발을 했었습니다.
<br>
