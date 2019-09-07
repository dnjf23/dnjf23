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

문제풀이
1. 거래내역에서 등록된 년도리스트를 추출한다
2. ServiceImple단에서 앞에서 추출한 년도값을 파라미터로 년도별로 합계금액이 큰 순서대로 list값을 호출 후 리턴값을 tuple에 {년도, 합계금액 List} 구조로 담는다.

수행 결과
[
    {
        "year": 2018,
        "dataList": [
            {
                "brName": "분당점",
                "brCode": "B",
                "sumAmt": 38500000
            },
            {
                "brName": "판교점",
                "brCode": "A",
                "sumAmt": 20510000
            },
            {
                "brName": "강남점",
                "brCode": "C",
                "sumAmt": 20234567
            },
            {
                "brName": "잠실점",
                "brCode": "D",
                "sumAmt": 14000000
            }
        ]
    },
    {
        "year": 2019,
        "dataList": [
            {
                "brName": "판교점",
                "brCode": "A",
                "sumAmt": 66800000
            },
            {
                "brName": "분당점",
                "brCode": "B",
                "sumAmt": 45400000
            },
            {
                "brName": "강남점",
                "brCode": "C",
                "sumAmt": 19500000
            },
            {
                "brName": "잠실점",
                "brCode": "D",
                "sumAmt": 6000000
            }
        ]
    }
]

Q4. 분당점과 판교점을 통폐합하여 판교점으로 관리점 이관을 하였습니다. 지점명을 입력하면 해당지점의 거래금액 합계를 출력하는 API 개발( 취소여부가 ‘Y’ 거래는 취소된 거래임,)

문제풀이
1."분당점"입력 시 404 exception이 호출되는 로직 추가한다.(실제 프로젝트에서 작업할때는 분당점과 판교점을 통합하는 db작업 후 분당점 입력 시 리턴결과가 null일때 처리 했겠지만 현 과제의 조건에는 명시되어 있지 않았으므로 입력값에 대해 exception 발생하게 처리)
2. 입력값을 파라미터로 데이터 조회 후 리턴값을 출력한다.

수행 결과
{
"brName":"판교점"
}
{
    "brName": "판교점",
    "brCode": "A",
    "sumAmt": 171210000
}

{
"brName":"분당점"
}
http status : 404
{"code":"404","메세지":"br code not found error"}

{
"brName":"강남점"
}
{
    "brName": "강남점",
    "brCode": "C",
    "sumAmt": 39734567
}

{
"brName":"잠실점"
}
{
    "brName": "잠실점",
    "brCode": "D",
    "sumAmt": 20000000
}
```

# 빌드 및 실행 방법
 1. [소스다운로드](https://drive.google.com/drive/folders/1rk377wCzRdE_8vXR4Bdj-_cy-n9fY5n4) 에서 파일을 내려 받는다.
 2. 로컬 db 셋팅을 한다. (테이블 생성 정보, 데이터 insert 스크립트 "DB_setup" 테이블 참조)
 3. application.properties 에 들어가서 spring.datasource.* 부분을 테스트환경의 로컬정보에 맞는 값으로 변경한다. 
 4. STS 기준으로 Boot Dashboard에서 서버 기동버튼을 클릭한다.
 
 테스트 방법
- Q1. http://localhost:8080/praticeNo1 호출
- Q2. http://localhost:8080/praticeNo2 호출
- Q3. http://localhost:8080/praticeNo3 호출
- Q4. http://localhost:8080/praticeNo4 호출
postman 실행 > post방식으로 선택 변경 > url 입력 > 파라미터 Body에 {"brName":"관리지점"} 입력 > send

 
 
 
 
 
 
 
 
 
 
 
 
 
 
