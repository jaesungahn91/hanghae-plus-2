# 장애 대응 훈련
## 시나리오
```
1. 장애 탐지 (시스템 알람, 고객센터, QA팀, 사내제보)
2. 장애 공지 (Slack, KakaoTalk 등 실시간 알람을 받을 수 있는 SNS 활용)
3. 장애 전파 (사내 내부 조직, 팀원, 고객 등)
4. 장애 복구
5. 장애 후속 조치
6. 장애 회고 (왜 장애가 발생했으며, 앞으로 어떻게 처리해야 할지 대응책 마련)
```
### 장애 탐지
#### 시스템 알림을 통한 탐지
- 시스템 지표: CPU, Memory, latency, 5xx error count 등
- 비즈니스 지표: 가게 상세 진입률, 주문 추이 등
- 외부 연동 시스템 지표: 연동시스템 주문 전달 실패, 프랜차이즈 시스템 오류 등
#### 고객 센터로 인입되는 문의

### 장애 공지
- 장애등급장애기간
- 고객이 받는 영향
- 논의채널
- 장애원인
- 조치내용
**확인된 최소의 정보만 가지고 빠르게 공지**

### 장애 전파
**장애를 해결하는 것만큼이나 장애 상황과 해결 방안을 잘 전파하는 것도 중요**
- 첫째, 장애 복구와 장애 전파를 분리 운영
- 둘째, 고객이 알고 싶어하는 내용을 전파

### 장애 복구
**장애 공지 후에 장애 전파와 장애 복구가 동시에 진행**
- 서비스 정상화는 원인 파악보다 우선
- 일반적으로 많이 사용하는 장애 복구 방법
	- 장비 증설
	- 롤백
	- 핫픽스
	- 장비 교체

### 장애 후속 조치
**장애 복구가 완료되고, 서비스가 정상화되면 원인 파악과 재발 방지 대책 수립을 위해서 장애 리뷰를 진행**
장애보고서에는 장애 발생 시각, 탐지 시각, 종료 시각, 장애 탐지 방법, 장애 발생 지점, 장애 복구 방법, 대응 과정 중의 시간별 action, 장애 원인, 재발 방지 대책 등이 포함
- 장애 원인 분석
- 재발 방지 대책 수립
- 장애 리뷰

---
## 부하테스트
**부하**란, **처리를 실행하려고 해도 실행할 수 없어서 대기하고 있는 프로세스의 수**를 의미
### 처리량
![tps.png](tps.png)
- User 증가 시 TPS는 어느 정도 증가하다가 더 이상 증가하지 않게 되며, Time은 일정하게 유지되다 점차적으로 증가합니다. 반면, 부하가 증가할 경우(TPS가 증가) 지연시간은 변곡점에 이르기도 하는데, 이 경우 시스템 리소스가 누수되고 있는 것은 아닌지 확인해봐야 합니다.
- Time과 달리, TPS (Transaction Per Seconds)는 Scale out 혹은 Scale up을 통해 증가시킬 수 있습니다. 보통 테스트 시에 단순히 응답시간을 기준으로 종료시키지 말고, TPS나 DB Connection, CPU 등을 종합적으로 확인하고 중단시켜야 합니다.

### 주의 사항
- 성능 테스트는 **실제 사용자가 접속하는 환경**에서 진행하여야 합니다. 내부 네트워크에서 부하를 발생시킬 경우 응답시간에 차이가 발생할 수 있습니다.
- 부하 테스트에서는 클라이언트 내부 처리시간이 배제되어 있음을 염두해두어야 합니다.
- 테스트 DB에 들어 있는 데이터의 양이 **실제 운영 DB와 동일**하여야 합니다. 통상 전체 성능의 70% 이상이 DB에 좌우되는데, 테스트 대상이 되는 테이블의 데이터 양이 다르면 쿼리의 실행계획이 달라져 성능이 다르게 나타날 수 있습니다. 또한 데이터가 소량이면 디스크 입출력이 일어나야 하는데 모두 메모리에 로드되어 성능이 빠른 것으로 착각할 수 있습니다.
- 운영환경의 경우, 서비스 요청 외에 별도로 수행되는 배치나 후속작업으로 인한 부하가 있을 수 있습니다. 서버에 일정하게 발생하는 부하가 있다면 성능 테스트 시나리오에도 포함해야 합니다.
- 외부 요인(결제 등)의 경우 **시스템과 분리된 별도의 서버로 구성**해야 합니다. 객체를 Mocking하는 경우 Http Connection Pool, Connection Thread 등을 미사용하게 되고 IO가 발생하지 않습니다. 같은 애플리케이션에 Dummy Controller를 구성하는 경우 테스트 시스템의 자원과 리소스를 같이 사용하므로 테스트의 신뢰성이 떨어집니다.

### 테스트 계획하기
#### * 전제 조건 정리
- 테스트하려는 **Target 시스템의 범위**를 정해야 합니다.
- 부하 테스트시에 저장될 데이터 건수와 크기를 결정하세요. 서비스 이용자 수, 사용자의 행동 패턴, 사용 기간 등을 고려하여 계산합니다.
- 목푯값에 대한 성능 유지기간을 정해야 합니다.
- 서버에 같이 동작하고 있는 다른 시스템, 제약 사항 등을 파악합니다.
#### * 목푯값 설정
1. 우선 예상 1일 사용자 수(DAU)를 정해봅니다.
2. 피크 시간대의 집중률을 예상해봅니다. (최대 트래픽 / 평소 트래픽)
3. 1명당 1일 평균 접속 혹은 요청수를 예상해봅니다.
4. 이를 바탕으로 Throughput을 계산합니다.

- **Throughput : 1일 평균 rps ~ 1일 최대 rps**
    - 1일 사용자 수(DAU) x 1명당 1일 평균 접속 수 = 1일 총 접속 수
    - 1일 총 접속 수 / 86,400 (초/일) = 1일 평균 rps
    - 1일 평균 rps x (최대 트래픽 / 평소 트래픽) = 1일 최대 rps
- **Latency : 일반적으로 50~100ms이하**로 잡는 것이 좋습니다.
- 사용자가 검색하는 데이터의 양, 갱신하는 데이터의 양 등을 파악해둡니다.
#### * 시나리오 대상
##### - 접속 빈도가 높은 기능
- 홈페이지 등
##### - 서버 리소스 소비량이 높은 기능
- CPU
    - 이미지, 동영상 변환
    - 인증
    - 파일 압축/해제
- Network
    - 응답 컨텐츠 크기가 큰 페이지
    - 이미지, 동영상 업로드/다운로드
- Disk
    - 로그가 많은 페이지
##### - DB를 사용하는 기능
- 많은 리소스를 조합하여 결과를 보여주는 페이지
- 여러 사용자가 같은 리소스를 갱신하는 페이지
##### - 외부 시스템과 통신하는 기능
- 결제 기능
- 알림 기능
- 인증/인가

### 테스트 설정값 구하기
#### 1. 목표 rps 구하기
> RPS(초당 요청 수) 또는 처리량은 **부하 테스트가 초당 생성하는 서버 애플리케이션에 대한 총 요청 수**

a. 우선 예상 1일 사용자 수(DAU)를 정해봅니다.  
b. 피크 시간대의 집중률을 예상해봅니다. (최대 트개픽 / 평소 트래픽)  
c. 1명당 1일 평균 접속 혹은 요청수를 예상해봅니다.  
d. 이를 바탕으로 Throughput을 계산합니다.

- **Throughput : 1일 평균 rps ~ 1일 최대 rps**
    - 1일 사용자 수(DAU) x 1명당 1일 평균 접속 수 = 1일 총 접속 수
    - 1일 총 접속 수 / 86,400 (초/일) = 1일 평균 rps
    - 1일 평균 rps x (최대 트래픽 / 평소 트래픽) = 1일 최대 rps
#### 2. VUser 구하기
- Request Rate: measured by the number of requests per second (RPS)
- VU: the number of virtual users
- R: the number of requests per VU iteration
- T: a value larger than the time needed to complete a VU iteration
```plaintext
T = (R * http_req_duration) (+ 1s) ; 내부망에서 테스트할 경우 예상 latency를 추가한다
VUser = (목표 rps * T) / R
```
- 가령, 두개의 요청 (**R=2**)이 있고, 왕복시간이 0.5s, 지연시간이 1초라고 가정할 때 (**T=2**), 계산식은 아래와 같다.
> VU = (300 * 2) / 2 = 300
#### 3. 테스트 기간
- 일반적으로 Load Test는 보통 30분 ~ 2시간 사이로 권장합니다. 부하가 주어진 상황에서 DB Failover, 배포 등 여러 상황을 부여하며 서비스의 성능을 확인합니다.

---
## 부하테스트 툴
- K6
- A

---
## 장애 시나리오
### 상품
> 상품 목록 조회 요청 대량 발생 시에 따른 장애 대응 전략
- 테스트 하는 상황 (유저 시나리오와 최대한 비슷하게)
	- 상황 벤치마킹 - 무신사 여름 세일
	- DAU 165만
	- https://www.sedaily.com/NewsView/29RZ4DX1BH
- 현재 DB에 적재되어있는 데이터의 수
	- 브랜드 2,063개
	- 할인상품 223,986개
- 동시 요청의 수
	- 1일 사용자 수(DAU) x 1명당 1일 평균 접속 수 = 1일 총 접속 수
		- 1650000 x 2 = 3300000
	- 1일 총 접속 수 / 86,400 (초/일) = 1일 평균 rps
		- 3300000 / 86,400 = 38 rps
- 우리가 기대하는 응답값 (P99 등)
	- Latency : 일반적으로 50~100ms이하로
	- 부하테스트별 기대 응답값
		- smoke test
			- P95 - 15ms
		- load test
			- P95 - 35ms
		- stress test
			- 부하지점 확인

---
🗓️ **Weekly Schedule Summary: 이번 챕터의 주간 일정**
- `10/27(금)`
    - 부하테스트 시나리오 및 진행 문서 제출
        - 아래내용을 포함
            - 테스트 하는 상황 (유저 시나리오와 최대한 비슷하게)
	            - 할인 이벤트 기간동안 평소보다 많은 유저가 상품 주문을 하는 경우
            - 현재 DB에 적재되어있는 데이터의 수
	            - 주의 사항
            - 동시 요청의 수
	            - 테스트 별다르게 
	            - 목표 rps 구하기
            - 우리가 기대하는 응답값 (P99 등)
	            - 테스트 별로 다르게
    - 서버의 확장, 배포과정에서 발생할 수 있는 장애 및 SPOF 에 대한 분석 레포트 제출
        - 아래 내용을 포함
            - 서버 배포 실패에 대한 대응방안
            - 우리 서비스의 외부 의존성 파악
            - 배포시의 하위호환성 보장 전략
	            - 
            - 롤백에 대한 배포전략
- `11/3(금)` Chapter4 종료
    - 부하테스트 결과와 대응 방안 레포트
    - 단일 장애 지점을 해소하기 위한 서킷브레이커, 이중화 등을 적용한 대응 레포트 제출
- `11/4(토)` 중간발표회
    - 소개 및 트러블슈팅 중심의 프로젝트 발표