# 개발 프레임웍크
 - JAVA 1.8.0_221
 - Spring Boot 2.0.2
 - mybatis 3.4.2
 - mysql 8.0.11
 - maven
# 문제해결 방법
```sh
Q1. 2018년, 2019년 각 연도별 합계 금액이 가장 많은 고객을 추출하는 API 개발.(단, 취소여부가 ‘Y’ 거래는 취소된 거래임, 합계 금액은 거래금액에서 수수료를 차감한 금액임)

문제풀이
1. group by 를 이용해 년도별 가장 많은 금액을 추출 후 테이블 t1 명으로 생성한다. 
2. 년도별 고객별 합계값을 추출 후 테이블 t2 명으로 생성한다.
3. 위에 만든 두 테이블의 년도, 합계금액을 조인해 결과를 얻는다.

테스트 방법
1. Postman or url직접 입력
2. http://localhost:8080/praticeNo1 호출

수행 결과
[
    {
        "year": 2018,
        "name": "테드",
        "acctNo": 11111114,
        "sumAmt": 28992000
    },
    {
        "year": 2019,
        "name": "에이스",
        "acctNo": 11111112,
        "sumAmt": 40998400
    }
]

Q2. 2018년 또는 2019년에 거래가 없는 고객을 추출하는 API 개발.
(취소여부가 ‘Y’ 거래는 취소된 거래임)

문제풀이
1. 거래내역의 년도정보 계좌정보 테이블의 크로스 조인을 통해 등록된 년도별 마스터 테이블인 accountMst 생성한다.
2. 거래내역의 년도별 거래 이력을 group by를 통해 추출해 dealHist 을 생성한다.
3. 두 테이블을 accountMst 기준으로 left outer join을 한 후 dealHist쪽에 값이 비는 데이터를 추출 한다.

테스트 방법
1. Postman or url직접 입력
2. http://localhost:8080/praticeNo2 호출

수행 결과
[
    {
        "year": 2018,
        "name": "사라",
        "acctNo": 11111115
    },
    {
        "year": 2018,
        "name": "제임스",
        "acctNo": 11111118
    },
    {
        "year": 2019,
        "name": "테드",
        "acctNo": 11111114
    },
    {
        "year": 2019,
        "name": "제임스",
        "acctNo": 11111118
    }
]

Q3. 연도별 관리점별 거래금액 합계를 구하고 합계금액이 큰 순서로 출력하는 API 개발.( 취소여부가 ‘Y’ 거래는 취소된 거래임)






```
