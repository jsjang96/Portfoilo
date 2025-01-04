## 🐱 DLC(Data Life Cycle)로 인한 5년 이전 데이터 삭제 프로시저 개발

- 방송통신 법으로 인하여서 현재로부터 5년 이전 데이터 삭제 프로시저 개발 <br>
- 담당자에게 매일 DLC진척율을 이메일로 받을 수 있도록 프로그램 개발 및 템플릿 작성 <br>
- 현재 삭제 진행중인 년/월을 기준으로 12개월 전부터 2개월 후의 데이터까지 노출 <br>
- 서버 로그를 통한 모니터링 진행

<br>

## 🧑‍🤝‍🧑 제작 기간 & 참여 인원

  	-  제작 기간 : 2024.04.01 ~ 진행중 
  	-  참여 인원 : 2명
<br>

## ⚙️ 사용 기술 스택

<br>

| ⚙️ 기술 스택 | 👇🏻 사용 목적 |
|--|--|
| **`ORACLE`** | 프로시저 개발 |
| **`REST API`** | 데이터 삭제 진척율 이메일 발송 |
| **`linux`** | 프로시저 스케줄링 및 이메일 발송 스케줄링 |

<br>

## 핵심 기능
이 서비스의 핵심 기능은 **5년 전 데이터 삭제 배치**입니다. <br>

방송통신 법으로 인하여서 5년 전 데이터를 삭제를 해야하고 총 30개의 테이블을 대상으로 하여 5년 전 데이터를 삭제합니다. <br>

데이터가 어느 기간의 몇 개의 데이터가 삭제가 됐는지 진척율을 REST API를 통해 템플릿을 짜고 전송해주었습니다. <br>

아래는 **DLC 개발**에 대한 첨부파일입니다.

<summary><b>DLC 프로세스</b></summary> <br>
<div align="center" markdown="1">
	<img src="https://github.com/jsjang96/images/blob/0e352c49eac92d566f1e8dc58feeb66604784e10/CUR_TG_DATA.png"/>
	<img src="https://github.com/jsjang96/images/blob/0e352c49eac92d566f1e8dc58feeb66604784e10/DLCCheck.png"/>
	<img src="https://github.com/jsjang96/images/blob/0e352c49eac92d566f1e8dc58feeb66604784e10/DLC_KEY_DT_COLLECT.png"/>
	<img src="https://github.com/jsjang96/images/blob/d04859a68b125e8748551b29bc58c77119195aae/CUR_DL_DATA.png"/>
</div>

위 사진들을 통해 봤을 때 주문 날짜를 기준으로 5년 전의 데이터들을 여러 검증을 한 후에 MNT_COL_DT라는 테이블에 INSERT를 하여 수집을 하였습니다. <br>

후에 MNT_COL_DT에 있는 수집한 데이터들을 MOD함수를 통하여서 개수를 분산화하여 삭제를 진행해주는 커서를 작성하였습니다. <br>

테이블 데이터 삭제를 하기 전에 SALE_TEST라는 계정에 삭제 대상 테이블을 똑같이 생성하여 백업을 시킨 후에 실제로 쓰이고 있는 테이블의 데이터를 삭제했습니다. <br>

이 코드는 주문번호(ORDER_NO)를 쓰고있는 테이블들의 데이터를 지운것이고 배송식별번호, 임시상품코드를 기준으로 하여서도 삭제 프로시저를 개발하여 삭제했습니다.

다음 사진들은 DLC의 진척율을 매일 담당자에게 이메일을 보내는 REST API 개발 건이고 소스와 실제 이메일을 보내는 템플릿입니다.

<div align="center" markdown="1">
	<img src="https://github.com/jsjang96/images/blob/d04859a68b125e8748551b29bc58c77119195aae/DLCProgressSource.png"/>
	<img src="https://github.com/jsjang96/images/blob/d04859a68b125e8748551b29bc58c77119195aae/DLCProgressSource2.jpg"/>
	<img src="https://github.com/jsjang96/images/blob/d04859a68b125e8748551b29bc58c77119195aae/DLCProgressSource3.png"/>
	<img src="https://github.com/jsjang96/images/blob/d04859a68b125e8748551b29bc58c77119195aae/DLCProgressSourceQuery.png"/>
	<img src="https://github.com/jsjang96/images/blob/d04859a68b125e8748551b29bc58c77119195aae/DLCProgressEmail.png"/>
</div>

<br>

## 트러블 슈팅

### 1. I/O 오류
- 커넥션 수도 늘려보고 한번의 트랜잭션에서 처리하는 데이터를 줄여봤지만 여전히 I/O오류가 났습니다. <br>
- 원인은 DB스펙의 문제였고 DB스펙을 향상시키는데는 현실적으로 어려움이 있기 때문에 MOD함수를 써서 sequnce를 6으로 나눠서 나머지가 0,1,2,3,4,5로 나누어서 <br>
  프로시저를 각각 개발을 하였고 시간이 최대한 겹치지 않게 스케줄링을 하여 실행 시켰습니다. <br>
- 추후 방향은 프로시저 개발한 것을 다른 DB로 이관하는 방향으로 논의가 됐습니다.
