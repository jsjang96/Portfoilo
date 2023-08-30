<img src="https://capsule-render.vercel.app/api?type=rounded&color=auto&height=200&section=header&text=MOOCHU&fontSize=90" />

## 🐱 MOOCHU 이란?

**`MOOCHU`** 는 **OTT, 영화관의 영화들의 데이터들을 한 곳으로 모아서**, <br>
**저장된 영화를 기반으로 사용자마다 좋아하는 영화, TV show를 추천**을 받을 수 있는<br> 
**`영화 추천 시스템 웹 서비스`** 입니다. <br>

<br>

<details>
<summary>🎤 MOOCHU 바로가기</summary>
<div markdown="1">

<br>
<a href="http://34.64.147.118:8000/moochu" target="_blank" rel="noopener noreferrer">👉🏻Click👈🏻</a>
<br>
</div>
</details>

<br>

## 🧑‍🤝‍🧑 제작 기간 & 참여 인원

  	-  제작 기간 : 2023.06.12 ~ 2023.08.11 (약 2개월)
	-  참여 인원 : 6명
<br>

## ⚙️ 사용 기술 스택

<br>

| ⚙️ 기술 스택 | 👇🏻 사용 목적 |
|--|--|
| **`django`** | MOOCHU 웹 서비스 구현 |
| **`fastapi`** | 추천 모델 serving |
| **`Google Cloud Storage`** | Data Warehouse, 학습된 모델 파일 적재 |
| **`Google BigQuery`** | Data Warehouse, 학습된 모델 파일 적재 |
| **`airflow`** | CGV, Lotte, MegaBox, OTT 개봉예정작 데이터 자동화|
| **`Kafka`** , **`redis`** | 트래픽 분산, 로그 적재 |
| **`Mysql`**, **`mongoDB`** | 데이터베이스 활용 |
| **`nginx`**, **`gunicorn`** | Web서버와 WAS 분리 |
| **`GCP(Google Cloud Platform)`** | 배포를 위한 클라우드 서비스 활용 |
| **`docker`**, **`docker compose`** | 배포를 위한 작업 환경 도커라이징 |
| **`elastic search`**, **`logstash`**, **`kibana`** | 검색 기능 고도화, 데이터 시각화 |

<br>

## ERD 설계
	
![DB ERD](https://github.com/jsjang96/images/blob/master/MOOCHU_erd.png)

<br>
## 핵심 기능
이 서비스의 핵심 기능은 **추천 서비스 기능**입니다. <br>
사용자가 즐겨서 보는 영화 장르, 영화 리스트를 기반으로 새로운 영화를 추천해주는 서비스입니다. <br>
또한, 영화들을 사용자들끼리 추천을 해주고 미니홈피를 만들어 소통도 할 수 있도록 했습니다. <br>
그리고, 장르와 OTT별로 본인이 좋아하는 영화들을 필터링하여 볼 수 있습니다.<br>
아래는 **전체 아키텍처**에 대한 첨부파일입니다.

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

### 4-1. python 코드로 대체 (문제 해결)
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
    	
	실제 서비스를 위한 데이터, 사용자에 대한 데이터 적재 => PostgreSQL
	로그 데이터 임시 저장 => Redis
	로그 데이터를 모두 적재 => MongoDB

 	저희 서비스는 도커라이징을 통한 배포를 하였으나, DB 서버는 보수적인 관리를 위해 도커라이징을 하지 않았습니다.

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
