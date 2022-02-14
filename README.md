# 항공사 고객 만족도 예측

## 데이터 셋

- id : 샘플 아이디
- Gender : 성별
- Customer Type : Disloyal 또는 Loyal 고객
- Age : 나이
- Type of Travel : Business 또는 Personal Travel
- Class : 등급
- Flight Distance : 비행 거리
- Seat comfort : 좌석 만족도
- Departure/Arrival time convenient : 출발/도착 시간 편의성 만족도
- Food and drink : 식음료 만족도
- Gate location : 게이트 위치 만족도
- Inflight wifi service : 기내 와이파이 서비스 만족도
- Inflight entertainment : 기내 엔터테인먼트 만족도
- Online support : 온라인 지원 만족도
- Ease of Online booking : 온라인 예매 편리성 만족도
- On-board service : 탑승 서비스 만족도
- Leg room service : Leg room 서비스 만족도
- Baggage handling : 수하물 처리 만족도
- Checkin service : 체크인 서비스 만족도
- Cleanliness : 청결도 만족도
- Online boarding : 온라인보딩 만족도
- Departure Delay in Minutes : 출발 지연 시간
- Arrival Delay in Minutes : 도착 지연 시간
- target : 만족 여부

## EDA 결과 해석

- 결측 데이터 없음

- 수치형 데이터 : 'Age', 'Flight Distance', 'Departure Delay in Minutes', 'Arrival Delay in Minutes'

- 범주형 데이터의 경우 전반적으로 데이터 분포가 균일하지 않음. data set split을 할 때 주의 필요함

- 범주형 데이터 Label 0 이슈

  - Lable 0이 존재하는 범주형 데이터 8개 존재함
  - 1) Seat comfort
    2) Departure/Arrival time convenient (무응답 비율이 많고 target과의 상관관계도 매우 낮아 데이터 삭제)
    3)  Food and drink 
    4) Inflight wifi service
    5) Inflight enteratinment
    6) Ease of Online booking
    7) Leg room service
    8)  Online boarding
  - 0 을 매우 불만족으로 해석할 경우 Target 값이 1인 경우가 우세한 것이 설명하기 어려움
  - 또한 Inflight wifi service, Ease of Online Booking, Online Boarding 의 경우 Lable 0인 데이터가 1~2개 정도로 매우 적음
  - 0은 무응답으로 가정함
  - target 값이 주로 1에 몰려있는 것으로 보아 무응답이 Random하게 발생한 것으로 보이지는 않음.
    - 첫번째 방법 : 다른 Feature를 통해 예측 (어려움, bias)
    - 두번째 방법 :  onehot encoding을 통하여 order를 고려하지 않음으로서 무응답도 하나의 category로 사용 (변수가 너무 많아짐 > 차원축소로 해결 가능)

- 다중 공산성

  - 차원 축소로 해결할 예정

- 데이터 특징 정리

  - id
    - 분석에 불필요한 데이터
    - 데이터 삭제

  - Gender  
    - 특이사항 보이지 않음
    - target과의 상관관계 good
    - onehot encoding

  - Customer Type 
    - Loyal Customer 데이터가 약 83% 차지함
    -  target과의 상관관계 good
    -  onehot encoding

  - Age 
    - target과의 상관관계 bad
    - 데이터 삭제 고려 대상

  - Type of Travel
    - 특이사항 없음 
    - onehot encoding

  - Class
    - Eco plus의 데이터 적음
    - 특이사항 없음 
    - onehot encoding

  - Flight Distance
    - target과의 상관계수 매우 낮음
    - 데이터 삭제 고려 대상

  - Seat comfort
    - 0일때 target값이 1에 치우쳐 있음 (이상값)
    - 0은 매우 불편함을 뜻하는 것이 아닐 것으로 보임 떄문에 Label에 순서가 있다고 보기 어려움
    - Food and drink와 다중 공산성을 보임
    - onehot encoding

  - Departure/Arrival time convenient
    - Label 0 무응답 수가 많음
    - target과의 상관관계가 매우 낮음
    - 데이터 삭제

  - Food and drink
    - 0일때 target값이 1에 치우쳐 있음 (이상값)
    - Seat comfort와 다중 공산성을 보임
    - onehot encoding

  - Gate location
    - target과 상관관계 매우 낮음
    - 데이터 삭제 고려 대상

  - Inflight wifi service
    - Label이 0인 데이터가 단 2개 존재함, target값은 모두 0
    - 결측 데이터일 가능성이 있지만 그 수가 매우 적고 Label을 ordinal 하게 보는데 문제가 없다고 판단.
    - onehot encoding / Label encoding

  - Inflight enteratinment
    - Label이 0인 데이터 target값이 1에 치우침(이상값)
    - onehot encoding

  - Online support
    - 특이사항 없음
    - Label encoding

  - Ease of Online booking
    - Label이 0인 데이터가 단 1개 존재함, target값은 0
    - onehot encoding / Label encoding

  - On-board service
    - 특이사항 없음
    - Label encoding

  - Leg room service
    - Label 0인 데이터 존재, 역시 이상값으로 보임
    - onehot encoding

  - Baggage handling
    - 특이사항 없음
    - Label encoding

  - Checkin service
    - 특이사항 없음
    - Label encoding

  - Cleanliness
    - 특이사항 없음
    - Label encoding

  - Online boarding
    - Label 0 데이터 1개 존재
    - onehot encoding / Label encoding

  - Departure Delay in Minutes
    - Arrival Delay in Minutes와 매우 강한 다중 공산성 보임
    - 왜도가 매우 큼
    - Log 변환

  - Arrival Delay in Minutes
    - Departure Delay in Minutes와 매우 강한 다중 공산성 보임
    - 왜도가 매우 큼 
    - Log 변환 필요함

  - target
    - target 값이 0인 데이터가 많지만 큰 불균형은 보이지 않음
    - 특이사항 없음




## 데이터 전처리



## 분석 모델



## 분석 결과 요약 



