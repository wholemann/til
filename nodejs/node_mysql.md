### Node MySQL 연동

아래와 같은 에러가 나는 경우

```
ERROR: Client does not support authentication protocol requested by server; consider upgrading MySQL client
```

사용자를 등록할 때 비밀번호를 설정하는데 caching_sha2_password는 대소문자특수문자숫자가 포함 된 복잡한 새 암호화 방식이고 mysql_native_password는 예전의 해싱 방식이다.

아래와 같이 my.cnf를 변경한다.

```
my.cnf
---------

[mysqld]
# The default authentication plugin to be used when connecting to the server
default_authentication_plugin=mysql_native_password
```

그 후 mysql에 접속하여 아래 쿼리를 입력한다.

```
mysql> ALTER USER 'username'@'%' IDENTIFIED WITH mysql_native_password BY 'password';
```