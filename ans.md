# 1. 1.2 节 SQL 的查询部分，23467 题的查询代码

2. 返回所有名字以 S 开头的船员

```sql
SELECT sid, sname FROM sailors
WHERE sname LIKE 'S_%';
```

3. 输出 Sailors 中年龄大于等于 29 的船员，输出他们的名字，并按他们的 rating 降序排序，仅输出前两个

```sql
SELECT sname
FROM Sailors
WHERE age >= 29
ORDER BY rating DESC
LIMIT 2;
```

4. 找到 rating 比 sid 为 7 的船员高的船员，返回他们的 age 除以 rating 的值

```sql
SELECT sname, age / rating AS age_per_rating
FROM Sailors
WHERE rating > (SELECT rating FROM Sailors WHERE sid = 7);
```

6. 返回预订过所有船的船员

```sql
SELECT s.sid, s.sname
FROM Sailors s
WHERE NOT EXISTS (
    SELECT 1
    FROM Boats b
    WHERE NOT EXISTS (
        SELECT 1
        FROM Reserves r
        WHERE r.sid = s.sid AND r.bid = b.bid
    )
);
```

7. 将 Sailors 中的船员按 rating>5 和 rating<=5 分成两组，分别返回他们的年龄的平均值。 用 CREATE VIEW 去重新实现上面的第二个例子的写法。

```sql
CREATE VIEW high_low_rating_avg_age AS
WITH high_rating AS (
    SELECT AVG(age) AS avg_age
    FROM Sailors
    WHERE rating > 5
),
low_rating AS (
    SELECT AVG(age) AS avg_age
    FROM Sailors
    WHERE rating <= 5
)
SELECT
    (SELECT avg_age FROM high_rating) AS high_rating_avg_age,
    (SELECT avg_age FROM low_rating) AS low_rating_avg_age;
```

# 2. 1.3 节

```sql


CREATE TABLE time_slot (
    slot_id CHAR(1),
    date_day day,
    start_hour INT,
    start_minute INT,
    end_hour INT,
    end_minute INT
);

CREATE TABLE classroom (
    building_name VARCHAR(100),
    room_number INT,
    capacity INT
);

CREATE TABLE department (
    department_name VARCHAR(100),
    head_of_department VARCHAR(100),
    budget DECIMAL(10, 2)
);

CREATE TABLE course (
    course_id VARCHAR(10) PRIMARY KEY,
    course_name VARCHAR(100),
    department VARCHAR(50),
    credits INT
);

CREATE TABLE instructor (
    instructor_id VARCHAR(5) PRIMARY KEY,
    name VARCHAR(100),
    department VARCHAR(50),
    salary DECIMAL(10, 2)
);

CREATE TABLE section (
    section_id VARCHAR(5) PRIMARY KEY,
    course_id VARCHAR(5),
    semester VARCHAR(10),
    date_year YEAR,
    building VARCHAR(50),
    room_number VARCHAR(5),
    instructor_id VARCHAR(5)
);

CREATE TABLE student (
    student_id VARCHAR(10) PRIMARY KEY,
    student_name VARCHAR(100),
    department_name VARCHAR(100),
    age INT
);

CREATE TABLE takes (
    student_id VARCHAR(10),
    course_id VARCHAR(10),
    section_id VARCHAR(10),
    semester VARCHAR(10),
    date_year year,
    grade VARCHAR(2),
    PRIMARY KEY (student_id, course_id, section_id, semester, date_year)
);

```
