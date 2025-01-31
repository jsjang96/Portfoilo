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

## <b>프로그램별 사용자 접근자 조회 및 최근 접속 날짜 업데이트</b>
<br>

## 설명
  	 해당 Job의 Step에서 chunk 방식으로 개발을 진행했습니다. 오류가 났을 때 통째로 rollback을 할 필요가 없다고 판단했기 때문에
  	 chunk방식을 선택을 했고 건마다 commit을 찍어주었습니다.

  	 chunk방식 중 조회를 담당하는 reader을 통하여서 프로그램별 사용자 접근 날짜를 조회하여 writer로 업데이트를 시켜주는 자동화 프로그램입니다.
	 옵티마이저가 인덱스를 잘 찾아서 갈 수 있도록 힌트를 주었고 각 사이트 별로 최근 접속 이력을 조회하고 업데이트를 진행했습니다.

	 매일 18시에 도는 스케줄 잡에 해당 Step을 Flow로 등록을 하였습니다. 
   	 18시에 도는 이 job은 크론탭을 통해 스케줄 등록을 하였고 리눅스를 통해 모니터링을 진행했습니다.
	
 	 배치 개발을 처음 진행했던 개발건이라 의미가 있고 공부를 많이 했던 기억이 납니다. chunk가 무엇인지, tasklet이 무엇인지
 	 그리고 배치가 어떻게 실행이 되고 메타 테이블에 어떻게 데이터가 들어가는지도 파악을 하면서 진행을 했습니다.
 	 이 개발을 통하여서 제가 배치 개발에 대한 재미를 느낄 수 있었고 새로운 것을 알아가는 것에 대한 쾌감을 느낄 수 있었습니다.
<br>
<br>

## <b>90일 이상 미접속 사용자 조회 후 접근 권한 삭제</b>
<br>

## 설명
	해당 Job의 Step에서 tasklet 방식으로 개발을 진행했습니다. 데이터 정합성 때문에 오류가 났을 때 통쨰로 rollback을 해줄 필요가 있다고
 	판단을 했기 때문에 tasklet 방식을 선택을 했고 통 commit을 해주었습니다.

  	DB에서 "useYn" 을 DB에서 가져와서 "1"이면 배치를 실행하고 "0"이면 SKIP을 할 수 있도록 진행했습니다.
   	이 배치를 배포를 했는데 배치를 실행하고 싶지 않으면 DB에서 "1"을 "0"으로 바꿔주면 SKIP할 수 있습니다.

	이 배치에서는 90일 이상 접속하지 않은 사용자들을 조회를 한 후에 각 프로그램에 접근할 수 있는 권한을 삭제하는 배치입니다.

	이 배치도 매일 18시에 돌 수 있도록 스케줄 잡에 등록을 하였고 리눅스를 통해 모니터링을 진행했습니다.

	이번 배치 개발을 하면서 내가 전에 배치를 개발하면서 공부했던 것들을 토대로 개발을 진행하였을 때 성취감이 있었고 
 	다른 방법으로 배치를 개발을 하니 더 재밌었고 시간 흐르는지도 모른채 개발을 했었습니다.
<br>

## 트러블 슈팅

### 1. 쿼리 처리 속도 이슈
	개발 후 배포를 하였는데 예상 시간보다 훨씬 오래동안 배치가 돌았습니다. 그래서 오류 문자가 오게 되었고 세션이 끊기게 되어서 FAILED처리가 되었습니다.
 
 	개발을 하고 TEST를 할 때는 인덱스를 잘 타고 속도도 빨랐지만 서버에 배포 후에 옵티마이저가 인덱스를 찾지 못하는 현상이 발견돼서 처리 속도가 느려졌습니다.

  	쿼리에 인덱스 힌트를 추가를 하였고 핫배포를 진행했습니다. 이것을 통하여서 알게된 것은 개발을 할 때 인덱스가 탈 수 있지만 서버에서는 옵티마이저가 인덱스를

   	찾지 못하는 현상이 발견될 수 있다는 것을 알게 됐고 다음에 개발할 때는 힌트를 추가하는게 좋다는 것으로 판단되었습니다.


## 느낀점

### 1. 스프링배치에 대한 관심과 호기심
	스프링배치에 대해서 모르는 것이 많았고 처음 개발한 것이었습니다. 무엇이든 처음하는 것, 모르는 것을 하는 것에 있어서 막연한 두려움이 있었습니다.
 
 	하지만 스프링배치 관련 책, 자료들을 보며 어떤 구조로 되어 있고 어떤 흐름으로 배치가 실행이 되는지 공부하면서 배치 개발을 진행했습니다.

  	Connection은 어디서 맺는지, Job과 Step의 상태는 어디에 저장이 되는지 등등 알 수 있었습니다. 모르는 것을 알게 됐고 직접 개발을 진행함으로써

    배치에 대해서 더욱 알고 싶어졌고 배치 개발을 더 하고 싶었습니다. 이러한 부분을 부장님께 말씀을 드렸더니 배치 담당자가 되었습니다. 다른 부서에서

    개발한 배치에 오류가 나면 원인 파악을 해주고 보고를 하였고 매주 배치 배포를 진행하였습니다. 
  	

### 2. 디버깅 속도와 판단력
	쿼리 처리 속도가 늦어져서 배치가 안 끝난 것에 있어서 굉장한 당황을 했었습니다. 개발했을 때에는 처리가 늦지 않았는데 배포를 하고 나서는 늦어짐으로 인해
 
	주말에 원격을 붙어서 디버깅을 했었습니다. 개발자는 얼마나 디버깅을 빨리하는지가 정말 중요하다는 것을 느끼게 해준 개발이었습니다. 소스상으로 봤을 때

  	이상이 없는 것을 확인한 후에 DB에서도 INDEX를 타고 있는지 확인을 하였는데 INDEX도 타고 해당 쿼리를 조회해보니 처리 속도도 빨랐습니다. 구글링을 통

   	하여서 서버 배포 후 옵티마이저가 인덱스를 찾지 못하는 경우도 있다고 하여 이러한 부분을 부장님께 말씀 드리고 맞는지 확인을 하였습니다. 그 후에

    바로 힌트를 주어서 배포를 하고 해결을 진행했습니다. 원인을 알았다면 그 일에 대해서 어떻게 해야할지 판단력을 키우는 것이 중요하다는 것을 알게 됐습니다. 
    
    작지만 이러한 경험들이 나중에 많은 개발을 할 때 도움이 될 것이라는 생각을 가지게 됐고 좋은 경험이었습니다.
  	
