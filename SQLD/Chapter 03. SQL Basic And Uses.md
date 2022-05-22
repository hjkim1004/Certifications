# Chapter 03. SQL 기본과 활용

## ⭐ Introduction

### 📌 Aims

* 관계형 데이터베이스와 SQL의 이해 관계를 이해하고, 원하는 데이터를 조회하기 위한 SELECT 문장을 구성하는 WHERE절, GROUP BY 절, HAVING 절, ORDER BY 절의 기능과 내장함수를 알아본다.
* 집합 간 관계를 연결하기 위한 WHERE 절 기반의 기본적인 조인(JOIN)을 이해하고, ANSI/ISO 표준 조인 문장도 알아본다.

<br>

### 📌 Summary

1. 관계형 데이터베이스 개요
2. SELECT 문
3. 함수
4. WHERE 절
5. GROUP BY, HAVING 절
6. ORDER BY 절
7. 조인
8. 표준 조인

<br>



## ⭐ Contents

### 📌 1. 관계형 데이터베이스 개요

#### ✒ 데이터베이스

* 데이터베이스란? 특정 기업이나 조직, 개인이 필요에 따라(예, 부가가치가 발생하는) 데이터를 일정한 형태로 저장해놓은 것을 의미한다.
* DBMS(Database Management System): 효율적인 데이터베이스 관리 및 데이터 손상 회피 및 복구를 지원하는 관리도구
* 데이터베이스의 발전

| 시대     | 개요                                                         |
| -------- | ------------------------------------------------------------ |
| 1960년대 | - 플로우 차트 중심의 개발 방법을 사용<br />- 파일 구조로 데이터를 저장 및 관리 |
| 1970년대 | - 데이터베이스 관리 기법이 태동했던 시기<br />- 계층형(Hierarchical), 망형(Network) 데이터베이스 같은 제품들이 상용화 |
| 1980년대 | - 현재 대부분의 기업에서 사용되는 관계형 데이터베이스가 상용화<br />- Oracle, Sybase, DB2 |
| 1990년대 | - 많은 제품이 더향상된 기능으로 정보시스템의 핵심 솔루션으로 자리 잡음<br />- Oracle, Sybase, Informix, DB2, Teradata, SQL Server<br />- 객체 지향 정보를 지원하기 위해 객체 관겨형 데이터베이스로 발전 |

<br>

#### ✒ SQL

* SQL(Structed Query Language): 관계형 데이터베이스에서 데이터 정의, 데이터 조작, 데이터를 제어하기 위해 사용하는 언어이다.
* SQL 문장들의 종류

| 종류                  | 명령어                      | 설명                                                         |
| --------------------- | --------------------------- | ------------------------------------------------------------ |
| 데이터 조작어 (DML)   | SELECT                      | 데이터베이스에 들어있는 데이터를 조회하거나 검색하기 위한 명령어로 RETRIEVE라고도 한다. |
| 데이터 조작어 (DML)   | INSERT, UPDATE, DELETE      | 데이터베이스 테이블에 들어있는 데이터에 변형을 가하는 종류의 명령어 |
| 데이터 정의어 (DDL)   | CREATE, ALTER, DROP, RENAME | - 테이블과 같은 데이터 구조를 정의하는 데 사용되는 명령어들이다.<br />- 구조를 생성/변경/삭제하거나 이름을 바꾸는 데이터 구조와 관련된 명령어 |
| 데이터 제어어 (DCL)   | GRANT, REVOKE               | 데이터베이스에 접근하고 객체들을 사용하도록 권한을 주고, 회수하는 명령어 |
| 트랜잭션 제어어 (TCL) | COMMIT, ROLLBACK            | 논리적인 작업 단위를 묶어서 DML에 의해 조작된 결과(트랜잭션)별로 제어하는 명령어 |

<br>

#### ✒ 추가된 ANSI/ISO 표준 SQL 기능

* STANDARD JOIN 기능 추가(CROSS, OUTER JOIN 등 새로운 FROM 절 JOIN 기능들)
* SCALAR SUBQUERY, TOP-N QUERY 등의 새로운 SUBQUERY 기능들
* ROLLUP, CUBE, GROUPING SETS 등의 새로운 리포팅 기능
* WINDOW FUNCTION 같은 새로운 개념의 분석 기능들

<br>

#### ✒ 집합연산자

* 일반 집합연산자

1. UNION 연산: 합집합, UNION과 UNION ALL의 결과가 같을때 UNION ALL을 사용한다.
2. INTERACTION 연산: 교집합
3. DIFFERENCE 연산: 차집합, EXCEPT로 구현(Oracle은 MINUS로 구현)
4. PRODUCT 연산: CROSS JOIN 기능으로 구현, 양쪽 집합의 M*N건의 데이터 조합이 이뤄지며, CARTESIAN PRODUCT라고도 표현한다.

<br>

* 순수관계 연산자

1. SELECT 연산: WHERE 절로 구현
2. PROJECT 연산: SELECT 절로 구현
3. JOIN 연산: 다양한 JOIN 기능으로 구현
4. DIVIDE 연산: 나눗셈과 비슷한 개념. 현재는 사용되지 않음

<br>

#### ✒ 테이블

* 테이블(Table)이란?  데이터를 저장하는 객체(Object)로서 관계형 데이터베이스의 기본 단위이다.
* 관계형 데이터베이스에서는 모든 데이터를 컬럼과 행이라는 2차원 구조로 나타낸다.
* 세로방향을 컬럼(Column), 가로 방향을 행(Row)이라고 한다.

| 용어    | 설명                                                         |
| ------- | ------------------------------------------------------------ |
| 테이블  | 행과 열의 2차원 구조를 가진 데이터의 저장 장소이며, 데이터베이스의 가장 기본적인 개념 |
| 컬럼/열 | 2차원 구조를 가진 테이블에서 세로 방향으로 이루어진 특정 속성(더이상 나눌수 없는 특성) |
| 행      | 2차원 구조를 가진 테이블에서 가로 방향으로 연결된 데이터     |

<br>

#### ✒ 데이터 유형

* 데이터베이스의 테이블에 특정 자료를 입력할 때, 그 자료를 받아들일 공간을 유형별로 나누는 기준
* 자주 쓰이는 데이터 유형

| 데이터 유형  | 설명                                                         |
| ------------ | ------------------------------------------------------------ |
| CHARACTER(s) | - 고정 길이 문자열 정보<br />- s는 기본 길이 1바이트, 최대 길이 2000(Oracle), 8000(SQL Server)<br />- s만큼 최대 고정 길이를 갖고있으므로, 할당된 변수값이 s보다 작을 경우 그차이만큼 공간이 채워짐 |
| VARCHAR(s)   | - CHARACTER VARYING의 약자로 가변 길이 문자열 정보<br />- Oracle 은 VARCHAR2로 표현, SQL Server는 VARCHAR로 표현<br />- s의 최소 길이 1바이트, 최대 길이 4000(Oracle), 8000(SQL Server)<br />- s만큼의 최대 길이를 갖지만 가변 길이로 조정이 되기 때문에 할당된 변수 값의 바이트만 적용된다. (Limit개념) |
| NUMERIC      | - 정수, 실수 등 숫자 정보(Oracle은 NUMBER로, SQL Server는 10가지 이상의 숫자 타입을 갖고있음)<br />- Oracle은 처음에 전체 자리수를 지정하고, 그 다음 소수 부분의 자리수를 지정한다.<br /> ex) 정수 부분 6자리, 소수부분 2자리 => NUMBER(8, 2) |
| DATATIME     | - 날짜와 시각 정보 (Oracle은 DATE로 표현, SQL Server는 DATETIME으로 표현)<br />- Oracle은 1초 단위, SQL Server는 3.33ms 단위 관리 |

<br>

### 📌 2. SELECT 문

#### ✒ 1. SELECT

* 사용자가 입력한 데이터를 조회한다.

```sql
-- ALL/DISTINCT 속성
SELECT [ALL/DISTINCT] s.name, s.age, ...
FROM STUDENT s;

-- 애스터리스크(*) 사용
SELECT *
FROM STUDENT;

-- 별명(ALIAS) 부여하기
SELECT s.name AS '이름', s.age AS '나이', ...
FROM STUDENT s;
```

<br>

#### ✒ 산술연산자

* NUMBER와 DATE 자료형에 대해 적용되며, 일반적으로 수학의 4사칙 연산과 동일하다.
* 일반적으로 산술 연산을 하거나 특정 함수를 적용하면 컬럼의 레이블이 길어지고, 기존의 컬럼에 대해 새로운의미를 부여한 것이므로 적절한 ALIAS를 새롭게 부여하는 것이 좋다.

| 산술 연산자 | 설명                                   |
| ----------- | -------------------------------------- |
| ()          | 연산자의 우선순위를 변경하기 위한 괄호 |
| *           | 곱하기                                 |
| /           | 나누기                                 |
| +           | 더하기                                 |
| -           | 빼기                                   |

<br>

#### ✒ 합성 연산자

* 문자와 문자를 연결하는 합성(CONCATENATION) 연산자
* Oracle에서는 2개의 수직 바(||)를 이용한다
* SQL Server에서는 + 표시를 이용한다
* CONCAT 함수를 사용할 수 있다.

<br>

### 📌 3. 함수

#### ✒ 내장 함수 개요

* 함수(Function)는 다양한 기준으로 분류할 수 있는데, 벤더에서 제공하는 함수인 내장함수(Built-in Function), 사용자 정의 함수(User Defined Function)으로 나눌 수 있다
* 내장함수는 단일행 함수, 다중행 함수로 나눌수 있다.
* 다중행 함수는 다시 집계함수, 그룹 함수, 윈도우 함수로 나눌 수 있다.

<br>

#### ✒ 단일행 함수

* 종류

| 종류           | 함수의 예                                                    |
| -------------- | ------------------------------------------------------------ |
| 문자형 함수    | LOWER, UPPER, ASCII, CHR/CHAR, CONCAT, SUBSTR/SUBSTRING, LENGTH/LEN, LTRIM, RTRIM, TRIM |
| 숫자형 함수    | ABS, SIGN, MOD,CEIL/CEILING, FLOOR, ROUND, TRUNC, SIN, COS, TAN, EXP, POWER, SQRT, LOG, LN |
| 날짜형 함수    | SYSDATE/GETDATE, EXTRACT/DATEPART<br />TO_NUMBER(TO_CHAR(d, 'YYYY'\|'MM'\|'DD'))/YEAR\|MONTH\|DAY |
| 변환형 함수    | (CAST, TO_NUMBER, TO_CHAR, TO_DATE)/<br />(CAST, CONVERT)    |
| NULL 관련 함수 | NVL/ISNULL, NULLIF, COALESCE                                 |

<br>

* 중요 특징

1. SELECT, WHERE, ORDER BY 절에 사용 가능하다.
2. 각 행들에 대해 개별적으로 작용해 데이터 값들을 조작하고, 각각의 행에 대한 조작 결과를 리턴한다.
3. 여러 인수를 가질 수 있다.
4. 특별한 경우가 아니면 함수의 인자로 함수를 사용하는 함수의 중첩이 가능하다

<br>

#### ✒ CASE 표현

* CASE 표현은 IF-THEN-ELSE 논리와 유사한 방식으로 표현식을 작성해 SQL의 비교 연산 기능을 보완하는 역할을 한다

* 예시

```sql
SELECT ENAME
	,CASE
		WHEN SAL > 2000 THEN SAL
		ELSE 2000
	END AS REVISED_SALARY
FROM EMP;
```

<br>

* 1) Simple Case Expression: case 다음 바로 조건에 사용되는 칼럼이나 표현식

```sql
SELECT LOC
	,CASE LOC
		WHEN 'NEW YORK' THEN 'EAST'
		WHEN 'BOSTON' THEN 'EAST'
		WHEN 'CHICAGO' THEN 'CENTER'
		WHEN 'DALLAS' THEN 'CENTER'
		ELSE 'ETC'
	END AS AREA
FROM DEPT;
```

<br>

* 2) Searched Case Expression: 컬럼이나 표현식을 표시하시 않고, 다음 ㅈwhen 절에서 조건절을 사용할 수 있는 방식

```sql
SELECT ENAME
	,CASE
		WHEN SAL >= 3000 THEN 'HIGH'
		WHEN SAL >= 1000 THEN 'MID'
		ELSE 'LOW'
	END AS SALARY_GRADE
FROM EMP;
```



### 📌 4. WHERE 절

#### ✒ WHERE 조건절 개요

* WHERE 절에는 두 개 이상의 테이블에 대한 조인 조건을 기술하거나 결과를 제한하기 위한 조건을 기술 할 수 있다.
* 불필요하게 많은 자료를 요청하는 SQL 문장은 서버의 CPU나 메모리 등의 시스템 자원을 과다하게 사용한다.
* Where 조건절이 없는 FTS(Full Table Scan) 문장은 SQL 튜닝의 1차적인 검토 대상이 된다.

* 형태

```sql
SELECT [DISTINCT/ALL] 컬럼명 [ALIAS 명]
FROM 테이블명
WHERE 조건식;
```

<br>

#### ✒ 연산자의 종류

* 비교연산자: =, >, >=, <, <=
* SQL 연산자: BETWEEN a AND b, IN (list), LIKE '문자열', IS NULL
* 논리 연산자: AND, OR, NOT
* 부정 비교 연산자: !=, ^=, <>

<br>

### 📌 5. GROUP BY, HAVING 절

#### ✒ 집계함수

* 집계함수(Aggregate Function): 여러 행들의 그룹이 모여서 그룹당 단 하나의 결과를 돌려주는 다중행 함수
* 집계함수의 종류

| 집계함수                               | 사용 목적                                                    |
| -------------------------------------- | ------------------------------------------------------------ |
| COUNT(*)                               | NULL 값을 포함한 행의 수를 출력한다                          |
| COUNT(표현식)                          | 표현식의 값이  NULL 값인 것을 제외한 행 수를 출력한다.       |
| SUM([DISTINCT \| ALL] 표현식)          | 표현식의 NULL 값을 제외한 합계를 출력한다                    |
| AVG ([DISTINCT \| ALL] 표현식)         | 표현식의 NULL 값을 제외한 평균을 출력한다                    |
| MAX ([DISTINCT \| ALL] 표현식)         | 표현식의 최대값을 출력한다.<br />(문자, 날짜 데이터 타입도 사용가능) |
| MIN ([DISTINCT \| ALL] 표현식)         | 표현식의 최소값을 출력한다.<br />(문자, 날짜 데이터 타입도 사용가능) |
| STDDEV([DISTINCT \| ALL] 표현식)       | 표현식의 표준 편차를 출력한다.                               |
| VARIANCE/VAR([DISTINCT \| ALL] 표현식) | 표현식의 분식을 출력한다.                                    |
| 기타 통계 함수                         | 벤더별로 다양한 통계식을 제공한다.                           |

<br>

#### ✒ GROUP BY/HAVING 절

```sql
SELECT [DISTINCT] 컬럼명 [ALIAS 명]
FROM 테이블명
[WHERE 조건식]
[GROUP BY 컬럼(column)이나 표현식]
[HAVING 그룹조건식]
```



#### ✒ CASE 표현을 활용한 월별 데이터 집계



### 📌 6. ORDER BY 절

#### ✒ ORDER BY 정렬

- 다양한 목적에 맞게 특정 컬럼을 기준으로 정렬/출력하는데 사용한다.
- 컬럼(Column) 대신에 SELECT 문에서 사용한 ALIAS 명이나 컬럼 순서를 나타내는 정수도 사용가능하다
- 정렬방식을 지정하지 않으면 기본으로 오름차순 정렬한다. ASC(Ascending)/DESC(Descending)

```sql
SELECT 컬럼명 [ALIAS 명]
FROM 테이블 명
[WHERE 조건식]
[GROUP BY 컬럼이나 표현식]
[HAVING 그룹조건식]
[ORDER BY 컬럼이나 표현식 [ASC | DESC]]
```

<br>

#### ✒ SELECT 문장 실행 순서

```sql
5. SELECT 컬럼명 [ALIAS 명]
1. FROM 테이블 명
2. [WHERE 조건식]
3. [GROUP BY 컬럼이나 표현식]
4. [HAVING 그룹조건식]
6. [ORDER BY 컬럼이나 표현식 [ASC | DESC]]
```

1. 발췌 대상 테이블을 참조한다(FROM)
2. 발췌 대상 데이터가 아닌 것은 제거한다(WHERE)
3. 행들을 소그룹화한다(GROUP BY)
4. 그룹핑된 값의 조건에 맞는 것만 출력한다(HAVING)
5. 데이터 값을 출력/계산 한다(SELECT)
6. 데이터를 정렬한다(ORDER BY)

<br>

### 📌 7. 조인

#### ✒ 조인 개요

* 조인이란? 두 개 이상의 테이블들을 연결해 데이터를 출력하는 것
* 일반적인 행들은 PK(PRIMARY KEY)나 FK(FOREIGN KEY) 값의 연관에 의해 조인이 성립한다.
* FROM 절에 여러 테이블이 나열되더라도 SQL에서 데이터를 처리할때는 단 두개의 집합 간에만 조인이 일어난다.
* 테이블의 조인 순서는 내부적으로 DBMS(옵티마이저)에 의해 결정된다.

<br>

#### ✒ 1. EQUI JOIN

* EQUI(등가) JOIN은 두 개의 테이블 간에 컬럼 값들이 서로 정확하게 일치하는 경우에 사용되는 방법.
* 대부분 PK <-> FK의 관계를 기반으로 한다.
* JOIN의 조건은 WHERE 절에 기술하게 되는데 "="연산자를 사용해서 표현한다.

<br>

#### ✒ 2. NON-EQUI JOIN

* NON-EQUI(비등가) JOIN은 두 개의 테이블 간에 논리적인 연관 관계를 갖고 있으나, 컬럼 값들이 서로 일치하지 않는 경우에 사용된다.
* "="연산자가 아닌 다른(Between, >, >=, <, <= 등) 연산자들을 사용해 JOIN을 수행한다.
* 예제

```sql
SELECT A.ENAME, A.JOB, A.SAL, B.GRADE
FROM EMP A, SALGRADE B
WHERE A.SAL BETWEEN B.LOSAL AND B.HISAL;
```

<br>

#### ✒ 3. 3개 이상의 TABLE JOIN

* 예시

```sql
SELECT A.PLAY_NAME AS 선수명, A.POSITION AS 포지션
	,B.REGION_NAME AS 연고지, B.TEAM_NAME AS 팀명
	,C.STADIUM_NAME AS 구장명
FROM PLAYER A, TEAM B, STADIUM C
WHERE B.TEAM_ID = A.TEAM_ID
	AND C.STADIUM_ID = B.STADIUM_ID
ORDER BY 선수명;
```

<br>

#### ✒ 4. OUTER JOIN

* EQUI, NON EQUI 조인은 모두 조인 조건의 결과가 참(TRUE)인 행들만 반환하는 INNER 조인이다. OUTER JOIN은 조인 조건을 만족하지 않는 행들도 함께 반환할 때 사용한다.
* Oracle 의 조인 기호: (+)

```sql
SELECT T1.column1, T2.column2, ...
FROM Table T1, Table T2
WHERE T1.column1(+) = Table2.column2
```

* 주의점: (+) 기호의 위치, (+) 반대편에 있는 테이블이 OUTER TABLE의 기준이 된다.

<br>

### 📌 8. 표준 조인

#### ✒ FROM 절 조인 형태

* ANSI/ISO에서 표시하는 FROM 절의 조인 형태

1. INNER JOIN: 가장 기본적인(DEFAULT) JOIN으로 조인 조건을 만족하는 행들만 반환한다. DEFAULT 옵션이므로 생략 가능.
2. NATURAL JOIN: INNER JOIN의 하위 개념, 두 테이블 간에 동일한 이름을 갖는 모든 컬럼들에 대해 EQUI(=) JOIN 수행
3. USING 조건절 (Oracle 제한)
4. ON 조건절: JOIN조건 명시
5. CROSS JOIN
6. OUTER JOIN

<br>

#### ✒ 1. INNER JOIN

* INNER JOIN은 내부조인이라고 하며, 조인 조건에 만족하는 행들만 반환한다.
* INNER JOIN 표시는 전통적인 방식의 조인 문법에서는 WHERE 절에 기술하던 조인 조건을 FROM절에 정의 하겠다는 표시 이므로 USING 조건절이나 ON 조건절을 필수적으로 사용해야한다.

```sql
SELECT A.EMPNO, A.ENAME, B.DEPTNO, B.DNAME
FROM EMP A
	[INNER] JOIN DEPT B ON B.DETPNO = A.DEPTNO;
```

<br>

#### ✒ 2. NATURAL JOIN

* NATURAL JOIN은 두 테이블 간에 동일한 이름을 갖는 모든 컬럼들에 대해 EQUI(=) JOIN을 수행한다.
* NATURAL JOIN이 명시되면 추가로 USING 조건절, ON 조건절, WHERE 조인조건 등 조인조건절을 사용할 수 없다. 그리고 SQL Server에서는 지원하지 않는 기능이다.

```sql
SELECT A.EMPNO, A.ENAME, DEPTNO, B.DNAME
FROM EMP A
	NATURAL JOIN DEPT B;
```

<br>

#### ✒ 3. USING 조건절

* NATURAL JOIN에서는 같은 이름을 가진 모든 컬럼들에 대해 조인이  이루어지지만, FROM 절의 USING 조건절을 이용하면 같은 이름을 가진 컬럼들 중에서 원하는 컬럼에 대해서만 선택적으로 EQUI JOIN을 할수 있다.
* 이 기능은 SQL Server에서는 지원하지 않는다.

```sql
SELECT *
FROM DEPT A JOIN DEPT_TEMP B
USING (DEPTNO);
```

<br>

#### ✒ 4. ON 조건절

* 조인 서술부(ON 조건절)과 비 조인 서술부(WHERE 조건절)를 분리해 이해가 쉬우며, 컬럼명이 달라도 조인조건을 기술할 수 있는 장점이 있다.
* ON 조건절에 JOIN조건 외에도 데이터 검색 조건을 추가할 수 있으나, 검색조건 목적인 경우는 WHERE절을 사용할 것을 권고한다. (다만 OUTER JOIN에서 조인의 대상을 제한하기 위한 목적으로 사용되는 추가 조건의 경우에는 ON 절에 표기되야 한다.)

<br>

#### ✒ 5. CROSS JOIN

* E.F.CODD 박사가 언급한 일반 집합 연산자의 PRODUCT 개념으로 테이블 간 조인 조건이 없는 경우 생길 수 있는 모든 데이터의 조합을 말한다.

```sql
SELECT A.ENAME, B.DNAME
FROM EMP A CROSS JOIN DEPT B
ORDER BY A.ENAME;
```

<br>

#### ✒ 6. OUTER JOIN

* 전통적인 방식에서는 OUTER JOIN 문법으로 (+) 기호를 사용했으나, 여러 문제점으로 새로운 OUTER JOIN 문법을 사용하게 되었다.

* OUTER 문법: LEFT OUTER JOIN(LEFT JOIN), RIGHT OUTER JOIN(RIGHT JOIN), FULL OUTER JOIN(FULL JOIN)