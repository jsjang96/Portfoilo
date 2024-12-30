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
이 프로그램의 핵심 기능은 **접근 권한 관리 자동화 프로그램**입니다. <br>
특정 프로그램에 접근한지 90일이 지난 사용자를 조회하여 권한을 자동으로 삭제를 해주도록 하였습니다. <br>
리눅스의 크론탭을 이용하여 매일 18시에 배치가 돌 수 있도록 하였고 배치 후 모니터링까지 진행했습니다.<br>

프로그램별 사용자 접근 조회 및 최근 접속 날짜 업데이트는 Chunk방식을 이용하여 개발했습니다. 오류가 떴을 때 통째로 rollback을 할 필요가 없다고 판단했기 때문에 건마다 commit을 찍어주었습니다. <br>
90일 이상 미접속 사용자 조회 후 접근 권한 삭제는 데이터의 정합성 때문에 하나의 오류가 떴을 때 통째로 rollback을 시켜주는 tasklet 방식을 이용하여 개발했습니다.

<details>
<summary><b>전체 아키텍처 펼치기</b></summary>
<div align="center" markdown="1">
	<img src="https://github.com/jsjang96/images/blob/master/%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%981.png"/>
	<img src="https://github.com/jsjang96/images/blob/master/%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%982.png"/>
	<img
src="https://github.com/jsjang96/images/blob/master/%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%983.png"/>
</div>
</details>
<br>

## 트러블 슈팅

### 1. 크롤링
- 데이터를 모으는 과정에 있어서 일정 데이터 개수가 넘어가면 403오류가 떠서 홈페이지에 안들어가짐.(ip 테더링을 해도 되지 않음)
<img src="https://github.com/jsjang96/images/blob/master/%ED%81%AC%EB%A1%A4%EB%A7%81%20403%EC%98%A4%EB%A5%98%20-%20%EB%AC%B8%EC%A0%9C%EB%B0%9C%EC%83%9D.png"/>
<br>

#### 1-1. 팀원들과 분산작업 (에러 해결)
<img src="https://github.com/jsjang96/images/blob/master/%ED%8C%80%EC%9B%90%20%EB%B6%84%EC%82%B0%20%ED%81%AC%EB%A1%A4%EB%A7%81%20-%20%EB%AC%B8%EC%A0%9C%ED%95%B4%EA%B2%B0.png"/>
<br>

### 2. Elasticsearch - mongoDB 연동 실패
- mysql, PostgreSQL - Elasticsearch 연동은 잘 됐음. ( 관계형 DB와 elasticserach의 연동은 문제 없음 )
- Mongodb(Nosql) - Elasticsearch 연동 실패
<br>

### 3. Cloud에 띄워져 있는 mongoDB 해킹
- Cloud에 mongoDB를 띄워서 데이터를 넣었으나 시간이 지나면 계속 데이터가 사라지는 문제점을 확인.
<br>

#### 3-1. 팀원들과 분산작업 (에러 해결)
- 조사 끝에 누군가가 해킹을 해서 침입을 한 것으로 파악
- 원인 : 방화벽 설정을 안함
- 방화벽 설정을 통하여 ip설정을 하여 침입을 방지
<br>

### 4. Kafka에서 connenctor 실패
- kafka와 logstash와 연동은 됐지만 싱크 커넥터를 통해 BigQuery에 데이터가 안들어감.
<br>

#### 4-1. python 코드로 대체 (문제 해결)
- python에서 임의로 꺼내줄 수 있는 코드를 짜서 BigQuery에 보내줌.
<br>

## 내가 맡은 큰 틀
<br>

### 1. 영화, TV show 대량 데이터 수집 및 전처리
<br>

	영화, TV show의 데이터를 수집을 하였습니다. 크롤링 하는 것에 있어서 예상치 못한 문제들이 상당히 많았습니다.
 	OTT별로도 데이터를 수집해보고 "키노라이츠"라는 사이트에서도 데이터들을 모았습니다.
  	크롤링을 하는데 계속 403 error가 떴고 ip를 테더링해서 몇 번을 다시 시도 했지만 크롤링 되는 중간에 중단이 됐고 그 후로는 403 error가 떴습니다.
 
 	처음 겪는 문제이고 어떤게 문제점인지 점검하는 과정에 있어서 보안 문제임을 알게 됐습니다.
    혼자서 여러 날을 나눠서 크롤링을 할 수 있었지만 빠르게 데이터를 모으기 위해 팀원들과 소통을 하여 협업을 했습니다.
	영화 고유 id값들을 6분할을 해서 팀원들에게 나눠줬고 각자 다른 ip에서 크롤링을 할 수 있도록 했습니다.
 	그것을 json파일로 저장하여 병합하고 전처리를 하였습니다.
  
 	각자마다 홈페이지를 만들 때 필요한 데이터 형식이 달라서 각자에게 맞는 데이터 형식으로 주기 위해 전처리를 했습니다.
	데이터를 대량 수집하고 전처리를 하며 알게 된 것이 있습니다.

 	첫째, 403 error가 뜨면 보안 문제임을 알게 됐습니다.
   
   	둘째, 프로젝트를 하며 전처리를 하는 것이 꽤나 많은 시간이 걸린다는 것을 알게 됐습니다.
    
    셋째, 팀원들과 소통을 하며 협업하는 것을 어떻게 하는지 몰랐는데 이 작업을 통해서 알게 됐고 더 적극적으로 소통을 하게 됐습니다.
    
    넷째, 데이터 수집, 전처리가 재밌다는 것을 알 수 있었습니다.


### 2. mongoDB에 데이터 적재, mongoDB와 django연동
  <br>
	
  	영화 데이터를 json 형식으로 저장하기 위해 mongoDB를 사용했습니다.
   	OTT별로 collection을 나눠주고 분류했습니다.
    
   	이 mongoDB를 django와 연동하였습니다.
    서버를 띄우기 전 local에서 mongoDB를 썼을 때 pymongo를 통해 django와 연동을 했습니다.
    mongoDB를 서버에 띄우고 django와 연동하는 것은 local에서 하는 것보다 훨신 수월했습니다.
    	
### 3. django에서 영화 리스트 페이지 구현
  <br>

  	영화, TV show 라는 media type을 선택하면 영화, TV show에 해당하는 페이지가 나올 수 있도록 구현했습니다.
   	mongoDB에 있는 데이터들을 django에 구현을 하였고 OTT와 장르별로 필터링을 진행했습니다. 페이지에 나오는 data들을 random하게 나올 수 있도록 하였습니다.
    페이징과 장르는 중복 선택이 가능할 수 있도록 했습니다.

	처음에는 mongoDB에 있는 미디어 데이터들을 구현하는 것이 쉽지 않았지만 문제점을 차근차근히 파악을 했고 해결했습니다.
 	django에 대해 자세히 알게 됐고 data가 어떻게 흘러가는지에 대해서도 알게 됐습니다.

### 4. Airflow를 통한 ETL 파이프라인 구축
<br>   	
	
 	Airflow를 통해 MEGABOX, CGV, DAUM영화 에서 데이터를 크롤링(수집)하고 전처리를 하였습니다.
  	영화에 대한 poster image들을 Google Cloud Storage에 적재하고 
 	필요한 데이터를 전처리 후 mongoDB로 전송하는 일련의 과정을 자동화하였습니다.

  	django에서 사용자가 머물러 있는 시간을 기준으로 log data가져왔습니다.
   	django에서 로그가 생성되게 코드를 짰고 파일로 만들었습니다. logstash가 그 파일을 바라보게끔 연동을 했습니다.
    필요한 data만 빼고 dict 형태로 바꾸고 kafka로 보냈습니다.
   
    Kafka에 log data를 저장했지만 이것을 커넥터를 통해 redis와 BigQuery에 보내줘야하는데 커넥터 오류가 떴습니다.
    그래서 python을 통해 connector 역할을 할 수 있는 코드를 짜서 보내줬습니다.

 	BigQuery를 쓴 이유는 cloud ML을 돌리려고 사용했으나 무료 크레딧 영역에서 모델을 돌릴 수가 없었습니다.
  	해서 모델은 jupyter로 돌렸습니다.

   	redis를 사용해 사용자들이 자주 보는 데이터들을 보여줄 수 있도록 하였습니다.
   	log data로 ML을 돌리고 그것을 redis에 보내줘서 빠르게 20~30개의 추천 데이터를 보여주었습니다.

 	즉, 모든 일련의 과정이 자동화되어 지속적으로 서비스가 제공될 수 있게 설계하였습니다.

  ### 5. ELK스택을 사용한 자동화
<br>   	
	
 	Airflow로 영화3사, Daum의 영화 데이터들을 bigquery, mongoDB, elasticsearch에 각자 넣으려고 하였습니다.
  	하지만 Airflow가 할 일이 많아지고 복잡해져서 Airflow가 mongoDB를 바라보게 하려고 Logstash를 썼습니다.
   	그 과정에 있어서 데이터가 흐르지 않는 것을 Kibana를 통해 확인을 했습니다.
    안되는 원인 : MongoDB가 비정형데이터라 안되는 것으로 추정됩니다.

 <div align=center><h1>📚 STACKS</h1></div>

<div align=center> 
  <img src="https://img.shields.io/badge/python-3776AB?style=for-the-badge&logo=python&logoColor=white"> 
  <br>
  
  <img src="https://img.shields.io/badge/django-092E20?style=for-the-badge&logo=django&logoColor=white">
  <img src="https://img.shields.io/badge/gunicorn-499848?style=for-the-badge&logo=gunicorn&logoColor=white">
  <img src="https://img.shields.io/badge/NGINX-009639?style=for-the-badge&logo=NGINX&logoColor=white">
  <br>
  
  <img src="https://img.shields.io/badge/mysql-4479A1?style=for-the-badge&logo=mysql&logoColor=white"> 
  <img src="https://img.shields.io/badge/mongoDB-47A248?style=for-the-badge&logo=MongoDB&logoColor=white">
  <br>
  
  <img src="https://img.shields.io/badge/docker-2496ED?style=for-the-badge&logo=docker&logoColor=white">
  <img src="https://img.shields.io/badge/elasticsearch-005571?style=for-the-badge&logo=elasticsearch&logoColor=white">
  <img src="https://img.shields.io/badge/logstash-005571?style=for-the-badge&logo=logstash&logoColor=white">
  <br>
  
  <img src="https://img.shields.io/badge/redis-DC382D?style=for-the-badge&logo=redis&logoColor=white">
  <img src="https://img.shields.io/badge/apachekafka-231F20?style=for-the-badge&logo=apachekafka&logoColor=white">
  <img src="https://img.shields.io/badge/apacheairflow-017CEE?style=for-the-badge&logo=apacheairflow&logoColor=white">
  <br>
    
  <img src="https://img.shields.io/badge/ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white"> 
  <img src="https://img.shields.io/badge/googlecloud-4285F4?style=for-the-badge&logo=googlecloud&logoColor=white"> 
  <br>
  
  <img src="https://img.shields.io/badge/html5-E34F26?style=for-the-badge&logo=html5&logoColor=white"> 
  <img src="https://img.shields.io/badge/css-1572B6?style=for-the-badge&logo=css3&logoColor=white"> 
  <img src="https://img.shields.io/badge/javascript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black"> 
  <br>

  <img src="https://img.shields.io/badge/github-181717?style=for-the-badge&logo=github&logoColor=white">
  <img src="https://img.shields.io/badge/git-F05032?style=for-the-badge&logo=git&logoColor=white">
  <img src="https://img.shields.io/badge/fontawesome-339AF0?style=for-the-badge&logo=fontawesome&logoColor=white">
  <img src="https://img.shields.io/badge/bootstrap-7952B3?style=for-the-badge&logo=bootstrap&logoColor=white">
  <br>
</div>
