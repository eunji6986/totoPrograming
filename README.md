# totoPrograming
### MY SQL
- oracle의 기본제공 hr 스키마를 mysql로 가져오기, 연동시키기 
- 프로그래머스 SQL  


### 문법
- 곱집합이 되어버리는 카티션 프로덕트
n개의 테이블에서 조회할 때, where절에서 연결해 주지 않으면 
A table row count * B table row count 가 발생한다.
    
    ```sql
    select * from employees e, departments d 
    where e.departmant_id = d.department_id;
    
    -> where절에는 테이블 갯수 -1개의 연결점이 필요(다른 테이블과의 관계성이므로 ) 
    ```
    
- 조건지정 : Oracle의 decode = Mysql case when then end
    - group by 조건에 대해 count를 여러개 구할때
    
    ```sql
    SELECT 
        컬럼명 as example
        ,count(case when 조건1 and 조건2 then 1 end) as alias1
        ,count(case when 조건3 then 1 end) as alias2
        ,count(case when 조건4 then 1 end) as alias3
        ,count(case when 조건5 then 1 end) as alias4
    FROM 
        테이블명
    group by 
        example
    ```
    
- mysql에서 rownum쓰는 법 [https://developer-jjun.tistory.com/23](https://developer-jjun.tistory.com/23) **SET변수**
- order by 기준이 2개일 때
    
    ```sql
    order by example1 desc, example2; 
    (example 1이 우선 적용 정렬된다)
    ```
    
- 상위n개 조회 : limit 구문
    
    ```sql
    SELECT NAME
    FROM ANIMAL_INS
    ORDER BY DATETIME
    **LIMIT 1 (최상위 1개)**
    ```
    
- 그룹화 하기 (종류별 그룹화 해서 count 세는 문제)
    
    ```sql
    - 컬럼 그룹화
    select 컬럼 from 테이블 group by 그룹화할 컬럼;
    
    - 조건처리 후 그룹화
    select 컬럼 from 테이블 where 조건식 group by 그룹화할 컬럼;
    
    - 그룹화 후 조건달기 
    select 컬럼 from 테이블 group by 그룹화할 컬럼 having 조건식;
    
    - 조건처리 후 그룹화 후 조건달기
    select 컬럼 from 테이블 where 조건식 group by 그룹화할 컬럼 having 조건식;
    
    - 그룹화 해도 여전히 order by는 맨 뒤.
    select 컬럼 
    from 테이블
    [where] 조건식
    group by 그룹화할 컬럼
    [having] 조건식
    order by 컬럼1 [,컬럼2,컬럼3..]
    ```
    
- COUNT(*) : Null 값을 포함하여 카운트
COUNT(column) : Null 값을 제외하고 카운트
- count 값이 n건 이상인 것만 보여주기
    
    ```sql
    group by example
    having n <= count(example) 
    ```
    
- 시간포맷 
MySQL에서 `DATETIME`타입은 YYYY-MM-DD hh:mm:ss
시분초가 필요없을때는 처음부터 타입을 `DATE`형(YYYY-MM-DD)으로 지정
(`DATE`를 `DATE_FORMAT`으로 `%Y-%m-%d %h:%m:%s` 형식을 지정하면 시분초값은 0)
    
    ```sql
    FORMAT	설명
    %M	Month 월(Janeary, February ...)
    %m	Month 월(01, 02, 03 ...)
    %W	Day of Week 요일(Sunday, Monday ...)
    %D	Month 월(1st, 2dn, 3rd ...)
    %Y	Year 연도(1999, 2000, 2020)
    %y	Year 연도(99, 00, 20)
    %X	Year 연도(1999, 2000, 2020) %V와 같이쓰임
    %x	Year 연도(1999, 2000, 2020) %v와 같이쓰임
    %a	Day of Week요일(Sun, Mon, Tue ...)
    %d	Day 일(00, 01, 02 ...)
    %e	Day 일(0, 1, 2 ..)
    %c	Month(1, 2, 3 ..)
    %b	Month(Jen Feb ...)
    %j	n번째 일(100, 365)
    %H	Hour 시(00, 01, 24) 24시간 형태
    %h	Hour 시(01, 02, 12) 12시간 형태
    %I(대문자 아이)	Hour 시(01, 02 12) 12시간 형태
    %l(소문자 엘)	Hour 시(1, 2, 12) 12 시간 형태
    %i	Minute 분(00, 01 59)
    %r	hh:mm:ss AP
    %T	hh:mm:ss
    %S, %s	Second 초
    %p	AP, PM
    %w	Day Of Week (0, 1, 2) 0부터 일요일
    %U	Week 주(시작: 일요일)
    %u	Week 주(시작 월요일)
    %V	Week 주(시작: 일요일)
    %v	Week 주(시작:월요일)
    
    ---
    SELECT DATE_FORMAT(date컬럼,'%y-%m-%d') AS aliasExample  
    FROM test
    
    +-------------+
    | CREATE_DATE |
    +-------------+
    | 20-03-16    |
    +-------------+
    
    ---
    
    SELECT
        (CASE
             WHEN INSTR(DATE_FORMAT(CREATE_DATE, '%Y-%m-%d %p %h:%i'), 'PM') > 0
             THEN REPLACE(DATE_FORMAT(CREATE_DATE, '%Y-%m-%d %p %h:%i'), 'PM', '오후')
             ELSE REPLACE(DATE_FORMAT(CREATE_DATE, '%Y-%m-%d %p %h:%i'), 'AM', '오전')    
             END) AS CREATE_DATE
    FROM test
    
    +----------------------+
    |      CREATE_DATE     |
    +----------------------+
    | 2020-03-16 오후 06:20 |
    +----------------------+
    ```
    
- 한 컬럼에서 중복을 제거한 후 갯수 뽑는 법 
한 컬럼에서 중복을 제거한 후 조건을 걸어 갯수 뽑는 법
    
    ```sql
    select count(distinct 컬럼명) as alias
    from 테이블명
    
    count(distinct(case when then end)) 이렇게 distinct에 조건을 걸 수 도 있음.
    ```
    

### MYSQL 변수

프로그래머스 입양시각구하기(2) 문제를 풀며 정리 

```sql
입양시각 구하기(2) 정답

set @hour := -1;

SELECT (@hour := @hour+1) as hour,
		(select count(datetime) 
		 from animal_outs 
	 	 where @hour = date_format(datetime,'%H')) as count
from animal_outs
where @hour < 23 

원리는 이해 못했지만 rounum을 만들때처럼 
(@hour := @hour+1) 선언을 해주면 반복적으로 ++처리가 된다.
hour는 0부터 나와야 한다. +1했을때 0이 나오게끔 초기화를 -1로 한다.

```

```sql
(변수 선언+초기화 : 초기화 하지 않으면 null)
SET @변수이름 := 대입값; 
SET @변수이름 = 대입값; (set이외에서는 = 는 비교연산자임을 참고)

select @변수이름 := 대입값;
select @변수이름 := @변수이름+1 -> ++효과
```

- **UNION** 2개 이상의 테이블을 결합해서 결과값을 출력할 때, 중복된 값을 제거할 수 있는 쿼리. 컬럼의 갯수가 같아야 하는 점 참고.
- JOIN : [https://yoo-hyeok.tistory.com/98](https://yoo-hyeok.tistory.com/98) (다이어그램 이미지로 설명)
    - 😒❓ 두개 테이블을 조회해서 where절에서 묶어주는 것과, 조인해야 할 때를 어떻게 구분하면 될까?
    - 해답 : [https://yeo-computerclass.tistory.com/m/41](https://yeo-computerclass.tistory.com/m/41)
    
- unix_timestamp  함수

```sql
select distinct(customer_id)
from rental
where unix_timestamp(rental_date) < unix_timestamp('2005-06-01')

'2005-06-01'이전의 값에 대해서만 선택하기.
```

- case when then else end 로 replace없이 간단히 문자 치환하기

```sql
동물의 아이디와 이름, 중성화 여부를 아이디 순으로 조회하는 SQL문을 작성해주세요. 
이때 중성화가 되어있다면 'O', 아니라면 'X'라고 표시해주세요.

(case when sex_upon_intake like '%intact%' then 'X' else 'O' end) as 중성화
```

- 날짜 간격 계산

```sql
단순 일 차이 날짜1-날짜2 : DATEDIFF(날짜1,날짜2)
연 분기 월 주 일 시 분 초 지정해서 가져올 떄 : TIMESTAMPDIFF(단위, 날짜1, 날짜2);
SECOND : 초
MINUTE : 분
HOUR : 시
DAY : 일
WEEK : 주
MONTH : 월
QUARTER : 분기
YEAR : 연
 
```

- 중복된 값만 보여주며, 중복 리스트를 distinct 하지 않고 보여줄 때의 쿼리

```sql
-- 코드를 입력하세요
SELECT col1,col2,컬럼
from 테이블
where 컬럼 in (select 컬럼 from 테이블 group by 컬럼  having count(컬럼)>1)

```
