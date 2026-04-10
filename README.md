# Today I Learn 페이지 만들기

# DB 실습 과제

### 문제 1: 테이블 생성하기 (CREATE TABLE)
``` SQL
SELECT DISTINCT crew_id, nickname
FROM attendance
ORDER BY crew_id;

CREATE TABLE crew (
  crew_id INT NOT NULL,
  nickname VARCHAR(50) NOT NULL,
  PRIMARY KEY (crew_id)
);


INSERT INTO crew (crew_id, nickname)
SELECT DISTINCT crew_id, nickname
FROM attendance
ORDER BY crew_id;
```

### 문제 2: 테이블 컬럼 삭제하기 (ALTER TABLE)
``` SQL
ALTER TABLE attendance
DROP COLUMN nickname;
```

### 문제 3: 외래키 설정하기
``` SQL
ALTER TABLE attendance
ADD CONSTRAINT fk_attendance_crew
FOREIGN KEY (crew_id) REFERENCES crew (crew_id);
```

### 문제 4: 유니크 키 설정
``` SQL
ALTER TABLE crew
ADD CONSTRAINT uq_crew_nickname
UNIQUE (nickname);
```

### 문제 5: 크루 닉네임 검색하기 (LIKE)
``` SQL
SELECT nickname, attendance_date, start_time
FROM attendance
WHERE nickname LIKE '디%'
  AND attendance_date = '2025-03-04';
```

### 문제 6: 출석 기록 확인하기 (SELECT + WHERE)
``` SQL
SELECT *
FROM attendance
WHERE nickname = '어셔'
  AND attendance_date = '2025-03-06';
```

### 문제 7: 누락된 출석 기록 추가 (INSERT)
``` SQL
INSERT INTO attendance (crew_id, nickname, attendance_date, start_time, end_time)
VALUES (13, '어셔', '2025-03-06', '09:31', '18:01');
```

### 문제 8: 잘못된 출석 기록 수정 (UPDATE)
``` SQL
UPDATE attendance
SET start_time = '10:00'
WHERE nickname = '주니'
  AND attendance_date = '2025-03-12';
```

### 문제 9: 허위 출석 기록 삭제 (DELETE)
``` SQL
DELETE FROM attendance
WHERE nickname = '아론'
  AND attendance_date = '2025-03-12';
```

### 문제 10: 출석 정보 조회하기 (JOIN)
``` SQL
SELECT c.nickname, a.attendance_date, a.start_time, a.end_time
FROM crew AS c
INNER JOIN attendance AS a ON c.crew_id = a.crew_id
ORDER BY c.crew_id, a.attendance_date;
```

### 문제 11: nickname으로 쿼리 처리하기 (서브 쿼리)
``` SQL
SELECT *
FROM attendance
WHERE crew_id = (
  SELECT crew_id
  FROM crew
  WHERE nickname = '검프'
);
```

### 문제 12: 가장 늦게 하교한 크루 찾기
``` SQL
SELECT c.nickname, a.end_time
FROM crew AS c
INNER JOIN attendance AS a ON c.crew_id = a.crew_id
WHERE a.attendance_date = '2025-03-05'
ORDER BY a.end_time DESC
LIMIT 1;
```

### 문제 13: 크루별로 '기록된' 날짜 수 조회
``` SQL
SELECT crew_id, COUNT(attendance_date) AS recorded_days
FROM attendance
GROUP BY crew_id
ORDER BY crew_id;
```

### 문제 14: 크루별로 등교 기록이 있는(start_time IS NOT NULL) 날짜 수 조회
``` SQL
SELECT crew_id, COUNT(start_time) AS attended_days
FROM attendance
GROUP BY crew_id
ORDER BY crew_id;
```

### 문제 15: 날짜별로 등교한 크루 수 조회
``` SQL
SELECT attendance_date, COUNT(crew_id) AS crew_count
FROM attendance
WHERE start_time IS NOT NULL
GROUP BY attendance_date
ORDER BY attendance_date;
```

### 문제 16: 크루별 가장 빠른 등교 시각(MIN)과 가장 늦은 등교 시각(MAX)
``` SQL
SELECT
  crew_id,
  MIN(start_time) AS earliest_start,
  MAX(start_time) AS latest_start
FROM attendance
GROUP BY crew_id
ORDER BY crew_id;
```
