---
title: 'MySql 시작하기'
date: '2020-12-20'
tags: ['MySql']
draft: false
summary: 'MySql 에 대해 학습한 내용을 정리 했습니다.'
---

# **들어가기 전에**

> 교육원에서 Oracle을 이용해서 두 개의 프로젝트를 진행했었다.  
> 며칠 전, 좋은 기회로 스타트업 인턴으로 함께 할 수 있게 되었고, 회사에서 MySQL을 사용하기에 공부를 하고 첫 출근하면 그래도 조금 더 수월하게 일을 할 수 있을 것 같아 기록을 남기려고 한다.

---

# **Install MySQL**

- 맥을 사용하다보니 손쉽게 MySQL을 설치할 수 있는 Homeblew를 이용해 설치하기로 했다. 아래의 게시글을 참고했다.

[MacOS 에서 MySQL 설치](https://ssungkang.tistory.com/entry/MySQL-MacOS-%EC%97%90%EC%84%9C-MySQL-%EC%84%A4%EC%B9%98)

---

# **Command**

[mysql.server start 시 발생한 에러와 해결 방법](https://velog.io/@04_miffy/mysql.server-start-error-and-solution)

`ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)`

**서버의 방화벽이 풀려 있는지 확인을 해야한다.** ㅇMySQL은 설치 후, 데몬(mysqld)이 돌아가고, 해당 데몬을 통해 MySQL 데이터베이스에 접속하는 방식이다. 즉, 데이터베이스에 접근하기 위해서는 데몬이 실행되고 있어야한다.

- **brew services를 이용해 해당 접속 문제를 해결할 수 있다.**

```
// services 종료
brew services stop mysql

// services 시작
brew services start mysql

// MySQL root 실행
mysql -u -root -p
```

- 먼저 앞서 말한 것 처럼 Oracle을 주로 사용하다보니 MySQL의 권한 부여라던지 `user` 를 만들고, 해당 `user` 가 어떤 Database를 사용할 것인지를 명령해줘야 하는 부분에서 많이 막혔다.

### **MySQL 접속**

```
mysql -u {user 이름} -p
```

### **USER 생성, 삭제 및 권한 부여**

- USER 생성은 `root`에서 진행해야 하기 때문에 위의 과정을 `root`로 접속해야 한다.
- `%` 를 host 영역에서 입력하여 USER를 생성할 경우, 외부에서도 해당 USER에 접근 할 수 있다.
  - 나는 USER NAME : spring, PW : spring으로 만들었다.

```
// USER 생성하기
create user '유저명'@'% 혹은 localhost' identified by '패스워드';

// USER 삭제하기
delete from user where user = '유저명';

// USER 조회
select user, host from user;

// 생성된 USER 권한 부여하기
grant all privileges on 데이터베이스 명.* to '유저명'@'해당 유저의 host';
```

### **Database 생성 및 삭제, 사용 명령, 조회**

- MySQL은 어떤 데이터베이스를 사용할 것인지 명령을 내려주어야 한다.

```
// 생성
create database 데이터베이스 명;

// 삭제
drop database 데이터베이스 명;

// 사용 명령
use 데이터베이스 명;

// 조회
show databases;
```

### **번경된 내용 메모리에 반영**

```
flush privileges;
```

### **체크해야 할 부분들**

- DBeaver
  - DBeaver에서 MySQL 연결 시, **Driver properties → allowPublicKeyRetrieval**을 `true`로 변경해주어야 한다.
- URL 설정
  - MySQL 특정 버전 이후로는 KST를 지원하지 않아 URL에 직접 지정해줘야 한다.
  - URL 설정 시, 사용하고자 하는 데이터베이스를 파라미터로 넘겨주어야 한다.
- `jdbc:mysql://localhost:3306/DB이름?serverTimezone=Asia/Seoul&useSSL=false&characterEncoding=UTF-8`
