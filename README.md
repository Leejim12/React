## MySQL 정리


#SQL문 정리

-. MySQL Login
```
루트 로그인 : mysql -u root -p
정상 로그인 : mysql -h ip주소 -u 계정 -p;
```

## 기본 문법
-. 유저 보여주기
```
select user, host from mysql.user;
```
-. 유저 생성
```
create users '유저명'@'localhost' identified by 'somepassword';
```
-. 유저 삭제
```
Drop user 'someuser'@'localhost';
```
-. 접근권한 부여
```
Grant All PRIVILEGES ON *.* TO 'someuser'@'localhost';
FLUSH PRIVILEGES;
```

-. 사용자의 권한 확인.
```
Show Grants for 'someuser'@'localhost';
```

-. 권한 회수
```
REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'someuser'@'localhost';
( REVOKE UPDATE )
```
-. 데이터베이스 보기
```
Show Databases
```
-. DB 생성
```
Create Database dbname;
```
-. DB 선택
```
use dbname;
```
-. 외래키 설정
1. 이미 있는 테이블
```
ALTER TABLE Reservation
ADD CONSTRAINT CustomerID
FOREIGN KEY (ID)
REFERENCES Customer (ID);
```
2. 테이블 생성 시 애초에 
```
CREATE TABLE Test2
(
    ID INT,
    ParentID INT,
    FOREIGN KEY (ParentID)
    REFERENCES Test1(ID) ON UPDATE CASCADE
);
```

## 테이블 관련
-. 테이블 생성
```
CREATE TABLE users( id INT AUTO_INCREMENT,
first_name VARCHAR(100),
last_name VARCHAR(100),
email VARCHAR(50),
password VARCHAR(20),
location VARCHAR(100),
dept VARCHAR(100),
is_admin TINYINT(1),
register_date DATETIME,
PRIMARY KEY(id)
);
```
-. 인덱스 생성
```
CREATE INDEX idx_location On users(location);
DROP INDEX idx_location ON users;
```
-. 외래키와 함께 테이블 생성
```
CREATE TABLE posts(
id INT AUTO_INCREMENT,
user_id INT,
title VARCHAR(100),
body TEXT,
publish_date DATETIME DEFAULT CURRENT_TIMESTAMP,
PRIMARY KEY(id),
FOREIGN KEY (user_id) REFERENCES users(id)
);
```

-. 외래키(2개)와 함께 테이블 생성
```
CREATE TABLE comments(
id INT AUTO_INCREMENT,
post_id INT,
user_id INT,
body TEXT,
publish_date DATETIME DEFAULT CURRENT_TIMESTAMP,
PRIMARY KEY(id),
FOREIGN KEY(user_id) references users(id),
FOREIGN KEY(post_id) references posts(id)
);
```


--------------------------------------------------
## Join

-. 내부조인
```
Select
users.first_name,
users.last_name,
posts.title,
posts.publish_date
from users u
inner join posts p
On u.id = p.user_id
Order by p.title; 
```
-. Left Join 
* left join : 일치하는 것 만
```
SELECT
comments.body,
posts.title
FROM comments
LEFT JOIN posts ON posts.id = comments.post_id
ORDER BY posts.title;
```
-. 다중 join
```
SELECT
comments.body,
posts.title,
users.first_name,
users.last_name
FROM comments
INNER JOIN posts on posts.id = comments.post_id
INNER JOIN users on users.id = comments.user_id
ORDER BY posts.title;
```

--------------------------------------------------
## Select 관련
-. select 기본
```
SELECT * FROM users;
SELECT first_name, last_name FROM users;
```

-. select where
```
SELECT * FROM users WHERE location='Massachusetts';
SELECT * FROM users WHERE location='Massachusetts' AND dept='sales';
SELECT * FROM users WHERE is_admin = 1;
SELECT * FROM users WHERE is_admin > 0;
```

-. Order by
```
SELECT * FROM users ORDER BY last_name ASC;
SELECT * FROM users ORDER BY last_name DESC;
```

-. Concatenate Columns
```
SELECT CONCAT(first_name,' ', last_name) AS 'Name', dept FROM users;
```
-. in Range
```
SELECT * FROM users WHERE age BETWEEN 20 AND 25;
```
-. Like
```
SELECT * FROM users WHERE dept LIKE 'd%';
SELECT * FROM users WHERE dept LIKE 'dev%';
SELECT * FROM users WHERE dept LIKE '%t';
SELECT * FROM users WHERE dept LIKE '%e%';
```
-. Not Like
```
SELECT * FROM users WHERE dept NOT LIKE 'd%';
```
-. Group by + Having
```
SELECT age FROM users GROUP BY age;
SELECT age FROM users WHERE age > 20 GROUP BY age;
SELECT age, avg(age) FROM users GROUP BY age HAVING avg(age) >=2;
```

-. SubQuery
```
SELECT * FROM users WHERE location=
(SELECT location FROM users WHERE email = 'sara@gmail.com’);
```
--------------------------------------------------------
## 집계함수
```
SELECT COUNT(id) FROM users;
SELECT MAX(age) FROM users;
SELECT MIN(age) FROM users;
SELECT SUM(age) FROM users;
SELECT UCASE(first_name), LCASE(last_name) FROM users;
```
-----------------------------------------------------------
서브쿼리
1. 평균급여보다 높은사람 조회
```
SELECT EMPNO,ENAME,SAL FROM EMP WHERE SAL > (SELECT AVG(SAL) FROM EMP)
ORDER BY 3 DESC;
```

2. 부서번호 10인 사원 중 최대급여 받는 사원과 동일한 급여를 받는 사원
```
SELECT EMPNO,ENAME
FROM EMP
WHERE SAL = (SELECT MAX(SAL) FROM EMP WHERE DEPTNO = 10);
```


-------------------------------------------------------------------


## ERD 관련
-. ERD 그리는 방법
1. SQL → ERD
2. ERD → SQL
  : 이게 맞다.

-----------------------------------------------------------------
## DB 관련
1. 특징
### 실시간 접근
```
사용자수가 많아져도 실시간 응답 필요.
```
### 변화가능성
```
현실세계 반영해야함.
```
### 동시공용
```
여러 사용자 동시 사용 가능해야함.
```
### 내용 기반 참조
```
저장 위치 알 필요 없음. --> 값만을 이용해서 데이터 접근이 가능해야함.
```

2. 설계 단계
-. 단계별 주요 작업 내용
```

```

ㅁ. 관계형 데이터 모델 구조
```
1. 릴레이션
2. 2차원 테이블 형태 
-. 열 == column == Attribute ** Domain : 속성이 가질 수 있는 값 범위
-. 행 == Row == Tuple
```

#### 릴레이션 특징
```
1. 유일성 : 모든 튜플은 다른 값
2. 무순서성 : 순서없음.
3. 원자성 : 분해불가
```

-----------------------------------------------------------------

"# React" 
