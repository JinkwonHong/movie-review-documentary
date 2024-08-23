# 📽MovieReview🎞

## 🎬 프로젝트 소개
영화 정보 및 리뷰 등을 공유하는 영화 관련 커뮤니티 개발 프로젝트

## ⚙ 사용 개발 환경
|           |                                                             Tool & Version                                                             |
|:---------:|:--------------------------------------------------------------------------------------------------------------------------------------:|
| Language  |              <img src="https://img.shields.io/badge/Kotlin-ver 1.9.24-7F52FF?style=flat-squre&logo=Kotlin&logoColor=white"/>              |
|    IDE    |            <img src="https://img.shields.io/badge/Intellij%20IDEA-000000?style=flat-squre&logo=intellijidea&logoColor=white"/>            |
|    SDK    | <img src="https://img.shields.io/badge/Eclipse%20Temurin-ver 17.0.11-FF1464?style=flat-squre&logo=eclipseadoptium&logoColor=white"/> | 
| Framework |       <img src="https://img.shields.io/badge/Spring%20Boot-ver 3.3.1-6DB33F?style=flat-squre&logo=springboot&logoColor=white"/>        |
|    JWT    |         <img src="https://img.shields.io/badge/jjwt-ver 0.12.3-000000?style=flat-square&logo=jsonwebtokens&logoColor=white"/>          |
| Database  |         <img src="https://img.shields.io/badge/Supabase-3FCF8E?style=flat-square&logo=supabase&logoColor=white"/>          |
|   Cache   |         <img src="https://img.shields.io/badge/Redis-FF4438?style=flat-square&logo=redis&logoColor=white"/>          |
|   ORM   |         <img src="https://img.shields.io/badge/Spring Data JPA-43B02AF?style=flat-square&logo=&logoColor=white"/>          |
| Query Builder |         <img src="https://img.shields.io/badge/Querydsl-ver 5.0.0-0085CA?style=flat-square&logo=&logoColor=white"/>          |
|Performance Testing Tool|         <img src="https://img.shields.io/badge/nGrinder-FF9E0F?style=flat-square&logo=&logoColor=white"/>          |


## 🖼 ERD
![image](https://github.com/imseongwoo/MovieReview/assets/162969955/0ce814e6-97a4-47b7-86d3-9d5e0cc0b8a1)

## 🕹 주요 기능
#### 1. 인기 검색어 기능 (`/api/버전/keywords`)
- QueryDSL 활용 1시간, 24시간을 기준으로 `Between` 구문을 사용하여 Keyword 테이블에서 해당 조건에 맞는 keyword를 반환하는 API
  - 최근 1시간 인기 검색어 조회 기능 (`/api/버전/keywords/last-hour`)
  - 최근 24시간 인기 검색어 조회 기능 (`/api/버전/keywords/last-day`)
- 관련 Issue: [#2](https://github.com/imseongwoo/MovieReview/issues/2)
- 관련 Pull Request: [#29](https://github.com/imseongwoo/MovieReview/pull/29)

#### 2. 검색 기능 (`/api/버전/posts/search`)
- Post 테이블 내의 title, content Column 기준으로 `LIKE` 구문 사용 검색 API
  - QueryDSL 사용 SQL 쿼리문에 `LIKE` 구문 포함
- Pageable을 통한 Paging 처리를 통해 검색 API에 결과 페이지 단위로 조회
- 관련 Issue: [#48](https://github.com/imseongwoo/MovieReview/issues/48)
- 관련 Pull Request: [#51](https://github.com/imseongwoo/MovieReview/pull/51)

#### 3. 캐싱 기능 적용
(Version 1) 기본 검색 API
- Cache를 적용하지 않은 기본 검색 API
- `/api/v1/posts/search`
- 관련 Issue: [#48](https://github.com/imseongwoo/MovieReview/issues/48)
- 관련 Pull Request: [#51](https://github.com/imseongwoo/MovieReview/pull/51)

(Version 2) In-memory Cache(Local Memory Cache) 적용 API
- In-memory Cache(Local Memory Cache)를 적용한 검색 API
  - `spring-boot-starter-cache` 의존성을 사용하여 Local Memory Cache를 적용한 검색 API
  - Spring AOP 방식으로 동작하는 `@Cacheable` 어노테이션을 활용하여 캐시 적용
  - `/api/v2/posts/search`
- In-memory Cache(Local Memory Cache)를 적용한 인기 검색어 API
  - `spring-boot-starter-cache` 의존성을 사용하여 Local Memory Cache를 적용한 인기 검색어 API
  - Spring AOP 방식으로 동작하는 `@Cacheable` 어노테이션을 활용하여 캐시 적용
  - `/api/v2/keywords`
- 관련 Issues: [#34](https://github.com/imseongwoo/MovieReview/issues/34), [#54](https://github.com/imseongwoo/MovieReview/issues/54)
- 관련 Pull Request: [#52](https://github.com/imseongwoo/MovieReview/pull/52), [#53](https://github.com/imseongwoo/MovieReview/pull/53), [#55](https://github.com/imseongwoo/MovieReview/pull/55)

(Version 3) Redis를 활용한 Remote Cache 적용 API
- Redis를 활용한 Remote Cache를 적용한 검색 API
  - `/api/v3/posts/search`
- Redis를 활용한 Remote Cache를 적용한 인기 검색어 API
  - `/api/v3/keywords`
- 관련 Issue: [#35](https://github.com/imseongwoo/MovieReview/issues/35)
- 관련 Pull Request: [#56](https://github.com/imseongwoo/MovieReview/pull/56)

## 🧪 Redis Cache를 적용한 API 성능 테스트
<details>
  <summary><b style="font-size: 14px;">테스트 환경</b></summary>
  <div style="font-size: 12px;">
    CPU: Apple M1<br>
    RAM: 16GB<br>
    Database: Supabase<br>
    테스트 도구: nGrinder<br>
  </div>
</details>

- 최근 1시간 인기 검색어 API
  - (Version 1) 캐시 미적용
    <img width="1000" alt="last-hour-version-1" src="https://github.com/imseongwoo/MovieReview/assets/162969955/3b26a708-33d9-4b2f-80d7-2201b86cfd7d">
    - TPS: 288.8
    - Peak TPS: 321
    - Mean TPS Time: 328.30ms
    - Executed Tests: 33,078
    - Run Time: 2 Minutes

  - (Version 2) In-Memory Cache(Local Memory Cache) 적용
    <img width="1000" alt="last-hour-version-2" src="https://github.com/imseongwoo/MovieReview/assets/162969955/46aac7ef-7b9c-4229-a320-97321058a76d">
    - TPS: 3625.8
    - Peak TPS: 4675
    - Mean TPS Time: 14.16ms
    - Executed Tests: 413,014
    - Run Time : 2 Minutes

  - (Version 3) Redis Cache(Remote Cache) 적용
    <img width="1000" alt="last-hour-version-3" src="https://github.com/imseongwoo/MovieReview/assets/162969955/c6199c51-6658-4aa3-a82c-a99426396c15">
    - TPS: 3021.1
    - Peak TPS: 4313
    - Mean TPS Time: 15.93ms
    - Executed Tests: 343,392
    - Run Time : 2 Minutes

- 최근 24시간 인기 검색어 API
  - (Version 1) 캐시 미적용
    <img width="1000" alt="last-day-version-1" src="https://github.com/imseongwoo/MovieReview/assets/162969955/e07403f8-9249-48bc-a020-185d31658554">
    - TPS: 54.4
    - Peak TPS: 67
    - Mean TPS Time: 1766.97ms
    - Executed Tests: 6229
    - Run Time: 2 Minuts
  
  - (Version 2) In-Memory Cache(Local Memory Cache) 적용
    <img width="1000" alt="last-day-version-2" src="https://github.com/imseongwoo/MovieReview/assets/162969955/712d4726-0794-4feb-9485-f5d94bdafe8e">
    - TPS: 3522.1
    - Peak TPS: 4735
    - Mean TPS Time: 15.14ms
    - Executed Tests: 408,792
    - Run Time: 2 Minutes
      
  - (Version 3) Redis Cache(Remote Cache) 적용
    <img width="1000" alt="last-day-version-3" src="https://github.com/imseongwoo/MovieReview/assets/162969955/8b611485-4e23-4070-bb0c-d96c4c3b9bf3">
    - TPS: 2728.9
    - Peak TPS: 3948
    - Mean TPS Time: 16.92ms
    - Executed Tests: 315,504
    - Run Time: 2 Minutes


      
- 검색 API
  - (Version 1) 캐시 미적용
    ![search_post_v1](https://github.com/imseongwoo/MovieReview/assets/162969955/0de2c476-d2e7-481f-81a3-ff6b6a7fd08a)
    - TPS: 33.6
    - Peak TPS: 38.5
    - Mean TPS Time: 1487.39ms
    - Executed Tests: 9960
    - Run Time: 5 Minutes
    - vUsers: 50
    
  - (Version 2) In-Memory Cache(Local Memory Cache) 적용
    ![search_post_v2](https://github.com/imseongwoo/MovieReview/assets/162969955/05f764dc-bfd8-4b3e-a891-da94beae4e57)
    - TPS: 8627.7
    - Peak TPS: 10844
    - Mean TPS Time: 7.58ms
    - Executed Tests: 2,558,807
    - Run Time: 5 Minutes
    - vUsers: 100

  - (Version 3) Redis Cache(Remote Cache) 적용
    ![search_post_v3](https://github.com/imseongwoo/MovieReview/assets/162969955/49145fbd-b344-4fc7-b901-308f5d3a883b)
    - TPS: 6450.7
    - Peak TPS: 8622.5
    - Mean TPS Time: 10.39ms
    - Executed Tests: 1,913,411
    - Run Time: 5 Minutes
    - vUsers: 100

## 💾 캐시 적용 이유
- In-Memory Cache(Local Memory Cache)
  - 캐시된 데이터를 메모리에서 가져오기 때문에 응답 시간 단축
  - 별도의 캐시 서버를 구축할 필요 없이 코드 내에서 어노테이션 사용으로 쉽게 구현 가능
  - 애플리케이션의 내부 메모리를 사용하기 때문에 네트워크를 이용할 필요 없이 빠르게 데이터 전송 가능
  - 같은 프로세스 내에서 데이터를 캐싱하기 때문에 데이터 일관성 문제에서 안정적
  - 단일 서버나 소규모 프로젝트 같은 단일 인스턴스에선 로컬 메모리 캐시 활용이 보다 효율적
- Redis Cache
  - 여러 서버에 데이터를 분산 저장할 수 있어 수평 확장(Scale-Out)이 가능
  - 여러 애플리케이션이 하나의 Redis Cache 서버를 사용하여 다중 클라이언트 지원이 가능
  - 데이터를 단순하게 키 값으로 저장하는 것 이외의 리스트,셋 등 다양한 데이터 구조를 지원하여 효율적으로 데이터 관리 가능
  - Memcached에 비해 Spring Redis에서 제공하는 API를 활용하여 손쉬운 구축 가능

## 📈 Caffeine Cache
- Caffeine은 스프링 공식 라이브러리는 아니지만, 높은 신뢰도를 자랑하는 캐시 라이브러리
  Github에서 14.1K 이상의 별점을 받고 있으며(2023.08.14. 기준), Java 8 이후로는 Spring Boot에서도 auto-configuration을 지원할 정도로 널리 사용

#### Caffeine이 제공하는 주요 기능
- 자동 로딩: 캐시에 항목을 자동으로 로딩하며, 선택적으로 비동기 로딩도 지원
- 크기 기반 제거: 빈도와 최근 사용도를 기준으로 최대치를 초과할 경우 항목을 제거
- 시간 기반 만료: 마지막 접근 또는 마지막 쓰기 이후로 경과된 시간에 따라 항목을 만료
- 비동기 새로 고침: 항목에 대한 첫 번째 요청이 발생할 때, 비동기적으로 항목을 새로 고침 가능

  
기존의 Spring Cache를 사용하는 경우, 별도의 Config 설정 없이 기본적으로 ConcurrentMap을 이용해 In-Memory 캐시를 관리하게 되나 해당 방법의 경우 특정 기준에 따른 만료 등의 기능 구현의 어려움
반면 Caffeine은 라이브러리의 추상화 덕분에 이러한 기능들을 간편하게 사용 가능

#### 사용 이유
- 프로젝트에서는 현재 읽기 기능에만 캐시를 적용
  Caffeine은 초당 데이터 처리량 측면에서 매우 우수한 성능을 보여주며, 추가적인 기능이 필요 없는 상황에서 읽기 성능 최적화를 위해 가장 적절한 선택이라 판단하여 본 프로젝트에서는 Caffeine Cache를 사용하기로 결정
  ![image](https://github.com/user-attachments/assets/b3643002-4f95-4b66-9bae-b82929110312)
