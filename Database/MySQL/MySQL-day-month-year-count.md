# MySQL(MariaDB) 일별, 월별, 년별 통계

## 데이터가 존재하는 경우
```sql
SELECT
    DATE_FORMAT(joindate,'%Y-%m-%d'), COUNT(*)
FROM user
GROUP BY
    DATE_FORMAT(joindate,'%Y-%m-%d')
ORDER BY
    DATE_FORMAT(joindate,'%Y-%m-%d');
```
```
%Y-%m-%d 일별

%Y-%m 월별

%Y 년별
```

## 일별 데이터가 존재하지 않는 경우(0인 경우)
