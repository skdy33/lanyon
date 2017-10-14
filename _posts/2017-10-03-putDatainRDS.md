---
layout: post
title: RDS에 데이터 넣기.
---

## 1. 로컬 to RDS
방대한 양의 데이터가 로컬 워크스테이션에 산개해 있다. <br>
이를 RDS에 하나하나 넣어야 하는데, postgresql 이 손에 안익기도 하고... <br>
그래서 과정을 나열해볼 까 한다. <br>
*어플리케이션이 다 파이썬 기반이니 파이썬 기반으로...* <br>

### SPEC
* **ubuntu**
```zsh
❯ lsb_release -a                                        
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 16.04.3 LTS
Release:	16.04
Codename:	xenial
```
* ```PostgreSQL 9.6.3 ```

* python
```zsh
❯ python -V                                             
Python 3.6.1 :: Anaconda 4.4.0 (64-bit)
```

## 2 Python-PostgreSQL Database Adapter 소개
일단 파이썬에서 DB와 통신하는 모듈을 설치하자. <br>
이름은 **psycopg2**
```zsh
pip install psycopg2
```

설치 끝! <br>

## 3. psycopg2 사용예
psycopg2가 엄청난게 단순히 *string* 형태로 통신이 가능하다. <br>
여러 args(변수)들을 더럽게 추가하고 던지고 할 필요가 없는 것이다. <br>

그렇다면 우리가 할 것은? 바로 DB server와 Python client를 연결하는 것 뿐!<br>

연결하는 법 또한 너무 쉽다. <br>

### 3.1 커넥트!

postgreSQL server에 접근하는 방법은 다 아리라 믿는다. <br>
모르면 [컴히어](aws RDS 구동)
```python
import psycopg2

# 여기서 핵심은 쌍 따옴표를 쓰면 안된다는 것이다. 그냥 따옴표 써야한다.
conn_string = """
host = ''
user = ''
dbname = ''
password = ''
"""
# 이제 엔터 값을 없애야 한다.
conn_string = conn_string.replace('\n',' ').strip()

# connect!
conn = psycopg2.connect(conn_string)
```
자 이제 connect 완료!

### 3.2 테이블 만들어보기
**쿼리는 string으로**. <br>
이것만 기억하면 된다. <br>

나는 


## 참고자료
* [Python에서 PostgreSQL 사용하기](http://sanghun.xyz/python-postgres/)
