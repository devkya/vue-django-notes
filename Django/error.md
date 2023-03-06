# ERROR 해결

## NameError: name '\_mysql' is not defined

- error log : `NameError: name '_mysql' is not defined`
- solution

  1. `pipenv install pymysql`
  2. `settings.py` 아래 내용 추가

  ```python
  import pymysql

  pymysql.install_as_mySQLdb()
  ```

---
