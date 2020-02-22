# 개요  

## 데이터 설명  
**1.train, test , bus_bts 공통 사항**  
- 버스카드를 통해 결제를 한 경우에 대한 정류소 승, 하차 데이터  
- 버스에서 하차를 할 때, 버스카드를 찍지 않는 경우, 결측  
- 따라서, 승차 인원수와 하차 인원수가 동일하지 않고 다소 차이가 있음  

**2.train, test csv 공통사항**  
- 버스정류장 위경도 좌표 포함  
- 버스정류장 명은 동일하나 좌표가 다른 경우는 건너편(상행,하행)  

**3.train.csv and test.csv**  
- train.csv : 2019년 9월의 일별, 출근시간(6시-12시)의 버스 정류장별 승하차 인원,    퇴근시간(18시-20시)의 버스 정류장별 승차 인원이 기록  
- test.csv : 2019년 10월의 일별, 출근시간(6시~12시)의 버스 정류장별 승하차 인원이 기록  
- **고유값 : 일별 & 버스노선ID별 & 버스정류장(좌표)별 정보**  
- **y : 퇴근시간(18시 ~ 20시) 승차 인원 예측**  

**4.bus_bts.csv**  
- **고유값 : 버스카드별 승하차 정보 기록(출근시간(6시부터 12시)인 경우만)**

**5.submission_sample.csv**  
- test data의 ID와 목표변수인 18시~20시 승차 인원 존재  
- test.csv에서 ID와 예측값을 결합하여 제출  


## 스키마  

### train & test dataset   

| 컬럼명 | TYPE | 설명 | 비고 | 
|:---|:---|:---|:---:|
| id | NUMBER | 해당 데이터에서의 고유한 ID(train, test 중복 X) |
| date | DATE | 날짜 | PK |
| bus_route_id | NUMBER | 노선ID | PK |
| in_out | CATEGORY | 시내 or 시외 |
| station_code | NUMBER | 승하차 정류소의 ID | PK |
| station_name | VARCHER | 승하차 정류소의 이름 | PK |
| latitude | NUMBER | 버스 정류장의 위도 |
| longitude | NUMBER |	버스 정류장의 경도 |
| X~Y_ride | NUMBER | 	X:00:00 ~ Y:59:59까지 승차 인원 수 |
| X~Y_takeoff | NUMBER | X:00:00 ~ Y:59:59까지 하차 인원 수 |
| 18~20_ride | NUMBER |	18:00:00 ~ 19:59:59까지 승차 인원 수(train data에만 존재) |

### bus bts  

| 컬럼명 | Type | 설명 | 비고 |
|:---|:---|:---|:---:|
| user_card_id | NUMBER | 해당 승객의 버스카드ID |
| bus_route_id | NUMBER | 노선ID | PK |
| vhc_id | NUMBER | 차량ID |
| geton_date | DATE | 해당 승객이 탑승한 날짜 | PK |
| geton_time | TIME | 해당 승객이 탑승한 시간 | PK |
| geton_station_code | NUMBER | 승차정류소의 ID | PK |
| geton_station_name | VARCHAR | 승차정류소의 이름 | PK | 
| getoff_date | '' | 하차한 날짜 (NaN : 하차태그 X) |
| getoff_time | '' | 하차한 시간 (NaN : 하차태그 X) | 
| getoff_station_code | '' | 하차정류소의 ID (NaN : 하차태그 X)) | 
| getoff_station_name | '' | 하차정류소의 이름 (NaN : 하차태그 X) | 
| user_category | CATEGORY | 승객 구분*  | 
| user_count | NUMBER | 버스카드 결제 인수 (ex.3 : 3명이 카드 하나로 계산) |

*승객 구분(01-일반, 02-어린이, 04-청소년, 06-경로, 27-장애 일반, 28-장애 동반, 29-유공 일반, 30-유공 동반)
