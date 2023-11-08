# 오픈소스 프로젝트
## 특강
- 모듈/패키지
	- 폴더 structure
	- project
- 정리 -> 인지 레벨
- MSA
	- SaaS
	- service 끼리 end to end
	- transaction관리
		- 2pc 
		- saga

- 모노레포
	- 예) 구글
- 멀티레포
	- 자동화가 잘될 수록 좋음
- 팀단위 분리의 멀티레포를 선호
=> 분리시 경계선 설정 중요(정의 명확 필요)

- 패키지
	- 분리할 수 없는는 단위 묶음?
		- 분리되면 안되는 단위?
		- ex)
			- controller
			- domain
			- facade or application <= 도메인 로직을 응용 혹은 조합
			- infrastructure
- 모듈
	- 조합할 수 있는 단위?
	- 분리가능하고 
	- ex)
		- api-service-name
		- support
			- 비가역적 모듈
			- 라이브러리?
		- module(infra)
			- main-db(mysql)
			- redis(spring-data-redis)
			- jpa
				- entity
			- client(외부 api 호출)
boot 단위
-> bootstrap
	- bootstrap-api
	- bootstrap-batch


- 도메인 validation
	- service에 validation 만드는거
	- user domain 생성시 검증
	- user 도메인에 확인하기
		- ex) user.validation()

---
## 오픈소스 SW 생성
라이선싱 및 
- 
- CONTRIBUTING.adoc