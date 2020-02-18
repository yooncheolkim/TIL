## 01. null 관련 하여

### select 문에서 null 값 비교,

~~~sql
SELECT * FROM TMSTUSER T
WHERE T.USERNAME <> 'YY'
~~~
- 위와 같이 하면, USERNAME 컬럼이 NULL 인것 또한 조회 되지 않는다.

~~~SQL
SELECT * FROM TMSTUSER T
WHERE 
(T.USERNAME <> 'YY' OR T.USERNAME IS NULL)
~~~
- 이렇게 해야, USERNAME 이 NULL인것도 포함하여 조회 해준다.

</BR>
</BR>

### IF 조건에서 NULL 사용

~~~SQL
IF CURR.USERNAME <> 'A' THEN

END IF
~~~
- 이렇게 하면, NULL인것은 조건에 부합되지 않아 IF문을 빠져나가게 된다.

~~~SQL
IF CURR.USERNAME <> 'A' OR CURR.USERNAME IS NULL THEN 

END IF
~~~
- 이렇게 해야 NULL을 포함하여 조건안에 들어가게 된다.




</BR>
</BR>

### SELECT INTO 할때 조심해야 할점

~~~SQL
SELECT T.USERID
INTO V_USERID
 FROM TMSTUSER T
WHERE T.USERNAME  = 'AA'
~~~

- 조회 결과가 없을 경우 에러가 발생한다.

~~~SQL
SELECT MAX(T.USERID)
INT V_USERID
FROM TMSTUSER T
WHERE T.USERNAME = 'AA'
~~~
- 이렇게 하면, 그룹함수로 한 행만 가져오기 때문에, V_USERID에 최소한 NULL이 들어가는걸 보장할 수 있다.