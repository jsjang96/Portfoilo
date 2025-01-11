## 🐱 상품 속성별 매출 현황 프로그램 개발

상품 속성별로 그룹핑을 한 매출 현황 조회
1) 멀티 상품 코드, 멀티 업체 조회 기능 구현
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
| **`JAVA`** | 자바를 이용하여 프로그램 개발 |
| **`Spring`** | 웹 개발을 위한 프레임워크 |      
| **`Oracle`** | DB의 데이터 참조 |
| **`MyBatis`** | DB의 데이터를 화면에 노출시키기 위한 ORM |
| **`ExtJs`** | 화면 노출을 위한 자바스크립트 프레임워크 |


<br>

## 핵심 기능
	 이 프로그램의 핵심 기능은 상품 속성별로 매출 현황을 노출시켜주는 프로그램입니다.
  
 	 정산을 맡고 있으면서 현업 재무팀의 요청으로 개발을 하였고 다수의 상품코드, 다수의 업체코드의 상품을 볼 수 있도록 개발을 진행했습니다.
   
   	 그리고 상품의 속성인 대분류, 중분류, 소분류, 세분류마다 조건을 걸어 그에 해당되는 상품들을 노출시켜주었습니다.
   	  
   
   	 

<summary><b>상품 속성별 매출 현황 소스</b></summary>
<div align="center" markdown="1">
	<img src="https://github.com/jsjang96/images/blob/2022b8a79c4369da956540a723cfdffc35a618e9/Program.png"/>
	<img src="https://github.com/jsjang96/images/blob/2022b8a79c4369da956540a723cfdffc35a618e9/goodsPropertiesRetrieveController.png"/>
	<img src="https://github.com/jsjang96/images/blob/2022b8a79c4369da956540a723cfdffc35a618e9/retrieveGoodsPropertiesControllerJava.png"/>
	<img src="https://github.com/jsjang96/images/blob/2022b8a79c4369da956540a723cfdffc35a618e9/retrieveGoodsPropertiesService.png"/>
	<img src="https://github.com/jsjang96/images/blob/2022b8a79c4369da956540a723cfdffc35a618e9/retrieveGoodsPropertiesServiceImpl.png"/>
	<img src="https://github.com/jsjang96/images/blob/2022b8a79c4369da956540a723cfdffc35a618e9/retrieveGoodsPropertiesProcess.png"/>
	<img src="https://github.com/jsjang96/images/blob/2022b8a79c4369da956540a723cfdffc35a618e9/retrieveGoodsPropertiesDAO.png"/>
	<img src="https://github.com/jsjang96/images/blob/2022b8a79c4369da956540a723cfdffc35a618e9/mapper1.png"/>
	<img src="https://github.com/jsjang96/images/blob/2022b8a79c4369da956540a723cfdffc35a618e9/mapper2.png"/>
	<img src="https://github.com/jsjang96/images/blob/2022b8a79c4369da956540a723cfdffc35a618e9/mapper3.png"/>
	<img src="https://github.com/jsjang96/images/blob/2022b8a79c4369da956540a723cfdffc35a618e9/mapper4.png"/>
</div>
<br>

## 설명
	프로그램에서 헤더 쪽에 보면 멀티상품조회, 멀티업체조회가 있습니다. 이거는 한번에 여러개의 상품, 업체들을 조회해볼 수 있는 기능입니다.
 
 	이걸 통해서 현업들은 현재 어떤 업체가 어떤 상품을 팔고 실적을 어떻게 내고 있는지 파악할 수 있습니다.
  
	더 나아가서 현업에서 대,중,소,세분류별로도 보고 싶다고 하여 조회 조건에 추가를 하였습니다.

 	controller.js에서 헤더 조건에 해당하는 데이터를 가져와서 파라미터를 넘겨주었습니다. 그리고 응답받은 데이터를 directStoreCall 메소드를 이용하여 
  
  	해당하는 grid에 데이터를 뿌려주었습니다. 
   
	그리고 Controller.java에서는 데이터를 세팅을 해주었습니다. 보시는 것과 같이 ComUtil.injFilterToArr라는 것은 아래 사진과 같이 띄어쓰기를 하면
 
 	상품코드나 업체코드를 구별시켜주는 필터링 유틸리티입니다. 콤마 부분로 구별해주는 것은 쿼리에서 작업을 진행했습니다.
<img src="https://github.com/jsjang96/images/blob/5d452e8e9f6e5944b917fb27b5d8906d651eb1cd/multientpretrieve.png"/>
<img src="https://github.com/jsjang96/images/blob/5d452e8e9f6e5944b917fb27b5d8906d651eb1cd/multigoodsproperties.png"/>

 	그 후로 Service, ServiceImpl(구현체)를 타고 갑니다. ServiceImpl에서 트랜잭션을 관리하는 만약 조회기능이다 하면 @ReadTx 트랜잭션을 붙여줬고
  	I,U,D기능이다 하면 @ProcessTx를 붙여줘서 트랜잭션 관리를 해주었습니다.

   	그 뒤로는 Process, DAO, mapper을 거치게 됩니다.
    
    mapper에서의 조건들을 보면 if문을 통하여서 넘겨진 파라미터 중에서 해당되는 데이터가 있으면 쿼리가 타게끔 진행을 해주었습니다.
     
    그리고 foreach함수에서 위에서 말씀드린 것처럼 멀티상품, 밀터업체 조회할 때 콤마로 구별하기 위해서 seperator을 콤마로 지정해두었습니다.
<br>
<br>

## 트러블 슈팅

### 1. 멀티상품코드, 멀티업체코드 조회 후 다시 클릭 시 조회했던 코드들이 그대로 존재
	멀티상품코드, 멀티업체코드를 이용하요 조회를 한 후에 다시 이용하기 위해서 버튼을 눌렀을 때 이전에 조회했던 코드들이 계속 남아있었습니다.
 
 	Controller.js단에서 파라미터를 안넘기게끔 하는 것이 맞다는 판단 하에 directStoreCall후에 후처리를 해주는 callback함수에서

  	조회한 후에 해당 팝업에 코드들을 clear해주는 작업을 진행했습니다.


## 느낀점

### 1. 성취감
	스프링배치에 대해서 모르는 것이 많았고 처음 개발한 것이었습니다. 무엇이든 처음하는 것, 모르는 것을 하는 것에 있어서 막연한 두려움이 있었습니다.
 
 	하지만 스프링배치 관련 책, 자료들을 보며 어떤 구조로 되어 있고 어떤 흐름으로 배치가 실행이 되는지 공부하면서 배치 개발을 진행했습니다.

  	Connection은 어디서 맺는지, Job과 Step의 상태는 어디에 저장이 되는지 등등 알 수 있었습니다. 모르는 것을 알게 됐고 직접 개발을 진행함으로써

    배치에 대해서 더욱 알고 싶어졌고 배치 개발을 더 하고 싶었습니다. 이러한 부분을 부장님께 말씀을 드렸더니 배치 담당자가 되었습니다. 다른 부서에서

    개발한 배치에 오류가 나면 원인 파악을 해주고 보고를 하였고 매주 배치 배포를 진행하였습니다. 
  	

### 2. 공부의 필요성
	쿼리 처리 속도가 늦어져서 배치가 안 끝난 것에 있어서 굉장한 당황을 했었습니다. 개발했을 때에는 처리가 늦지 않았는데 배포를 하고 나서는 늦어짐으로 인해
 
	주말에 원격을 붙어서 디버깅을 했었습니다. 개발자는 얼마나 디버깅을 빨리하는지가 정말 중요하다는 것을 느끼게 해준 개발이었습니다. 소스상으로 봤을 때

  	이상이 없는 것을 확인한 후에 DB에서도 INDEX를 타고 있는지 확인을 하였는데 INDEX도 타고 해당 쿼리를 조회해보니 처리 속도도 빨랐습니다. 구글링을 통

   	하여서 서버 배포 후 옵티마이저가 인덱스를 찾지 못하는 경우도 있다고 하여 이러한 부분을 부장님께 말씀 드리고 맞는지 확인을 하였습니다. 그 후에

    바로 힌트를 주어서 배포를 하고 해결을 진행했습니다. 원인을 알았다면 그 일에 대해서 어떻게 해야할지 판단력을 키우는 것이 중요하다는 것을 알게 됐습니다. 
    
    작지만 이러한 경험들이 나중에 많은 개발을 할 때 도움이 될 것이라는 생각을 가지게 됐고 좋은 경험이었습니다.
  	
