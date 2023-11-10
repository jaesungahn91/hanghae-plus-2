# 오픈소스 프로젝트
## 특강
### 모듈과 패키지(하헌우 코치)
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
## 오픈소스 개발
> 리포지토리 라이선싱 및 기여자 지침 설정
- LICENSE.txt, LICENSE.md, README.md
- CONTRIBUTING.adoc
[licensing-a-repository](https://docs.github.com/ko/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/licensing-a-repository)
[setting-guidelines-for-repository-contributors](https://docs.github.com/ko/communities/setting-up-your-project-for-healthy-contributions/setting-guidelines-for-repository-contributors)
[Github license의 종류와 나에게 맞는 라이선스 선택하기](https://flyingsquirrel.medium.com/github-license%EC%9D%98-%EC%A2%85%EB%A5%98%EC%99%80-%EB%82%98%EC%97%90%EA%B2%8C-%EB%A7%9E%EB%8A%94-%EB%9D%BC%EC%9D%B4%EC%84%A0%EC%8A%A4-%EC%84%A0%ED%83%9D%ED%95%98%EA%B8%B0-ae29925e8ff4)
### npm repository 등록
- 오픈소스 등록(Maven Central Repository vs NPM Repository)
- 





---
- [오픈소스활동을 시작하기위한 작은가이드](https://deview.kr/data/deview/session/attach/1300_T3_%EA%B3%A0%EC%83%81%EC%9A%B0_%EC%98%A4%ED%94%88%20%EC%86%8C%EC%8A%A4%20%ED%99%9C%EB%8F%99%EC%9D%84%20%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0%20%EC%9C%84%ED%95%9C%20%EC%9E%91%EC%9D%80%20%EA%B0%80%EC%9D%B4%EB%93%9C.pdf)
- [깃허브(GitHub)에서의 오픈 소스 프로젝트 기여를 위한 초보자 가이드](https://seongjin.me/how-to-contribute-to-open-source/)