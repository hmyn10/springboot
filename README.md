## Spring Boot & JPA 사용 API



+ #### 개발환경

  - Lang : Java 8 
  - Build : Gradle
  - Framework : Spring Boot 2
  - Test : JUnit 4
  - Tech : JPA, Lombok, JPQL
  - Request & Reponse tool : Postman



- #### 빌드 및 실행방법

  ```
  1. github 소스 다운로드
  2. 프로젝트 마우스 우클릭 > Gradle > Refresh Gradle Project
  3. 프로젝트 마우스 우클릭 > Run As > Spring Boot App 실행 
  4. Token 생성을 위해 http://localhost:7979/user/signUp 호출과 
     http://localhost:7979/user/signIn 호출을 차례대로 실행해서 토큰 발급
  5. 데이터의 조회 전 http://localhost:7979/api/saveBank 호출하여 데이터 저장
  6. 저장 완료 후 각 API 호출
  ```



+ #### 엔티티 관계 ( 1(Bank) : 多(BankMoney) )

  >  Bank
  
  | Key  |    Column     |     Type      |
  | :--: | :-----------: | :-----------: |
  |  PK  | instituteCode |    NUMBER     |
  |      | instituteName | VARCHAR2(100) |
  
  >  BankMoney
  
  | Key  |    Column     |  Type  |
  | :--: | :-----------: | :----: |
  |  PK  |     year      | NUMBER |
  |  PK  |     month     | NUMBER |
  |  FK  | instituteCode | NUMBER |
  |      |     money     | NUMBER |
  
  

+ #### API 호출 URL (http://localhost:7979) 

  1. ##### POST
  
     - _/user/signUp_: 계정 생성 시, Token 생성 API
  
       > 요청 파라미터 (/user/signUp?username=kakao&password=pay1234)
  
       ~~~ 
       {
           "username" : "kakao",
           "password" : "pay1234"
       }
       ~~~
       
       > 응답
       
       ~~~ 
       {
           "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE1NjY0NDg4MjgsInVzZXJuYW1lIjoia2FrYW8ifQ.6jnSJDoSCVJAIjBdvcDQhJDuHA8Lkan3O1jwvUFodOg",
           "refresh_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE1NjY0NDkxMjgsInVzZXJuYW1lIjoia2FrYW8ifQ.dzwymLo4QNAcXV_CrzBkYuxnl_Ydka7Do9-pUNilMWU",
           "token_type": "bearer"
       }
       ~~~
       
     - _/user/signIn_ : 로그인 시, Token 발급 API
  
       > 요청 파라미터 (/user/signIn?username=kakao&password=pay1234)
  
       ~~~
       {
           "username" : "kakao",
           "password" : "pay1234"
       }
       ~~~
       
       > 응답
       
       ~~~
       {
           "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE1NjY0NDg4NzEsInVzZXJuYW1lIjoia2FrYW8ifQ.GfWHdn6igqa_pK9n_iKcS4cVwGjJz_BO5G203SEVXK0",
           "refresh_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE1NjY0NDkxNzEsInVzZXJuYW1lIjoia2FrYW8ifQ.HfjaJ7-5gz2k4aP-cC0LOdwDDs3TJEMjqJ4x3q2rIJA",
           "token_type": "bearer"
       }
       ~~~
       
     - _/api/saveBank_ : csv 파일 레코드 저장 API
  
       > 응답
  
       ~~~ 
       true
       ~~~
  
  2. ##### GET
  
     - _/user/refreshToken_ : refresh Token 재발급 API
  
       > 응답
  
       ~~~
       {
           "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE1NjI1MDQxNTYsInVzZXJuYW1lIjoidGVzdCJ9.Fp9k9MGai41mle6HY8lB5CpAOd2FCEfwQyp_lPHqbMw",
           "refresh_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE1NjI1MDQzOTYsInVzZXJuYW1lIjoidGVzdCJ9.YmFKlVD5oS_-i1ntoOKWTzBznoQIMd001qQVF0NstJk",
           "token_type": "bearer"
       }
       ~~~
       
     - _/api/findBankNameList_ : 은행명 조회 API
  
       > 응답
  
       ~~~ 
       [
           "주택도시기금",
           "국민은행",
           "우리은행",
           "신한은행",
           "한국시티은행",
           "하나은행",
           "농협은행/수협은행",
           "외환은행",
           "기타은행"
       ]
       ~~~
  
     - _/api/findTotalAmount_ : 년도별 각 금융기관의 지원금액 합계 및 총계 조회 API
  
       > 응답
  
       ~~~ 
       {
           "name": "주택금융 공급현황",
           "data": [
               {
                   "year": "2005",
                   "total": 48016,
                   "detail": {
                       "농협은행/수협은행": 1486,
                       "하나은행": 3122,
                       "우리은행": 2303,
                       "국민은행": 13231,
                       "신한은행": 1815,
                       "외환은행": 1732,
                       "주택도시기금": 22247,
                       "기타은행": 1376,
                       "한국시티은행": 704
                   }
               },
               {
                   "year": "2006",
                   "total": 41210,
                   "detail": {
                       "농협은행/수협은행": 2299,
                       "하나은행": 3443,
                       "우리은행": 4134,
                       "국민은행": 5811,
                       "신한은행": 1198,
                       "외환은행": 2187,
                       "주택도시기금": 20789,
                       "기타은행": 1061,
                       "한국시티은행": 288
                   }
               },
         (... 생략 ...) 
               },
               {
                   "year": "2017",
                   "total": 295126,
                   "detail": {
                       "농협은행/수협은행": 26969,
                       "하나은행": 35629,
                       "우리은행": 38846,
                       "국민은행": 31480,
                       "신한은행": 40729,
                       "외환은행": 0,
                       "주택도시기금": 85409,
                       "기타은행": 36057,
                       "한국시티은행": 7
                   }
               }
           ]
       }
        
       ~~~
  
     - _/api/findGodBank_ : 가장 큰 금액 지원 은행과 금액 출력 API
  
       >  응답 
  
       ~~~
       {
           "bank": "주택도시기금",
           "year": "2014"
       }
       ~~~
  
     - _/api/findKebMaxAndMin_ : 2005~2016년 외환은행 최소 평균 금액 (년도)과 최대 평균 금액(년도) 조회API
  
       > 응답
  
       ~~~ 
       [
           {
               "money": "78.4",
               "year": "2008",
               "part": "외환 은행 평균 최소 지원 금액"
           },
           {
               "money": "1701.8",
               "year": "2015",
               "part": "외환 은행 평균 최대 지원 금액"
           }
       ]
       ~~~
  
     - _/api/findPredictionAmount?bank={bank}&month={month}_
  
       : 2018년 특정 은행의 특정 달 지원 금액 예측 API
  
       > 요청 파라미터 (/findPredictionAmount?bank=국민은행&month=2)
  
       ~~~ 
       {
           "bank" : "국민은행",
           "month" : "2"
       }
       ~~~
  
       > 응답
  
       ~~~ 
       {
           "bank": 2,
           "year": 2018,
           "month": 2,
           "amount": 3831
       }
       ~~~

