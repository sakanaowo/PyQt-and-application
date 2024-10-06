
## <span style="color:rgb(255, 105, 97)">Explaination for Working with the QtSql module</span> 

Trong ví dụ đầu tiên, ta sẽ sử dụng `QSqlQuery` để tạo một database nhỏ để quan sát GUI quản lý tài khoản. DB chứa 2 bảng là `accounts` và `countries`, liên kết với nhau bằng `country_ID` trong `accounts` và `id` trong `countries`

![](Pasted%20image%2020241006151459.png)

### <span style="color:rgb(255, 179, 91)">Tạo kết nối tới DB</span> 

```python
# create_DB.py
import random  
import sys  
  
from PyQt6.QtCore import QCoreApplication  
from PyQt6.QtSql import QSqlDatabase, QSqlQuery  
  
  
class CreateEmployeeData:  
    """create a simple db for the project  
    demonstrate how to connect to a DB, create query, tables and records in table    """  
    # create connection to DB. If DB not exist, create new Db file    # using SQLite ver3 driver    
    def __init__(self):  
        self.database = QSqlDatabase.addDatabase("QSQLITE")  
        self.database.setDatabaseName("files/accounts.db")  
  
        if not self.database.open():  
            print("Error opening database")  
            sys.exit(1)  
  
        self.query = QSqlQuery()  
        # erase database contents  
        self.query.exec("DROP TABLE accounts")  
        self.query.exec("DROP TABLE countries")  
  
        self.query.exec("CREATE TABLE accounts (\n"  
                        "        id INTEGER PRIMARY KEY AUTOINCREMENT UNIQUE NOT NULL,\n"  
                        "        employee_id INTEGER NOT NULL,\n"  
                        "        first_name VARCHAR(30) NOT NULL,\n"  
                        "        last_name VARCHAR(30) NOT NULL,\n"  
                        "        email VARCHAR(30) NOT NULL,\n"  
                        "        department VARCHAR(30) NOT NULL,\n"  
                        "        country_id VARCHAR(30) REFERENCES countries(id))\n"  
                        "    ")  
        # positional binding  
        self.query.prepare(" INSERT INTO accounts(\n"  
                           "        employee_id,first_name,last_name,email,department,country_id)\n"  
                           "        VALUES(?,?,?,?,?,?)\n"  
                           "    ")  
  
        first_names = ["Emma", "Olivia", "Ava", "Isabella",  
                       "Sophia", "Mia", "Charlotte", "Amelia", "Evelyn",  
                       "Abigail", "Valorie", "Teesha", "Jazzmin", "Liam",  
                       "Noah", "William", "James", "Logan", "Benjamin",  
                       "Mason", "Elijah", "Oliver", "Jason", "Lucas",  
                       "Michael"]  
  
        last_names = ["Smith", "Johnson", "Williams", "Brown",  
                      "Jones", "Garcia", "Miller", "Davis", "Rodriguez",  
                      "Martinez", "Hernandez", "Lopez", "Gonzalez",  
                      "Wilson", "Anderson", "Thomas", "Taylor", "Moore",  
                      "Jackson", "Martin", "Lee", "Perez", "Thompson",  
                      "White", "Harris"]  
  
        # create data for first table  
        employee_ids = random.sample(range(1000, 2500), len(first_names))  
        countries = {"USA": 1, "India": 2, "China": 3,  
                     "France": 4, "Germany": 5}  
        country_names = list(countries.keys())  
        country_codes = list(countries.values())  
  
        departments = ["Production", "R&D", "Marketing", "HR",  
                       "Finance", "Engineering", "Managerial"]  
  
        for f_name in first_names:  
            l_name = last_names.pop()  
            email = (l_name + f_name[0]).lower() + "@gmail.com"  
            country_id = random.choice(country_codes)  
            dept = random.choice(departments)  
            employee_id = employee_ids.pop()  
            self.query.addBindValue(employee_id)  
            self.query.addBindValue(f_name)  
            self.query.addBindValue(l_name)  
            self.query.addBindValue(email)  
            self.query.addBindValue(dept)  
            self.query.addBindValue(country_id)  
            self.query.exec()  
  
        self.country_query = QSqlQuery()  
        self.country_query.exec("\n"  
                                "    CREATE TABLE countries (\n"  
                                "    id INTEGER PRIMARY KEY AUTOINCREMENT UNIQUE NOT NULL,\n"  
                                "    country VARCHAR(30) NOT NULL\n"  
                                "    )\n"  
                                "    ")  
        self.country_query.prepare(" INSERT INTO countries(country) values (?)")  
        for name in country_names:  
            self.country_query.addBindValue(name)  
            self.country_query.exec()  
        print("Database created successfully")  
  
  
if __name__ == "__main__":  
    app = QCoreApplication(sys.argv)  
    CreateEmployeeData()  
    sys.exit(app.exec())
```

---
NOTE: 
-  **Một kết nối được tham chiếu bằng tên kết nối, không phải tên của DB**
- Nếu muốn đặt tên cho DB, phải truyền tên đó làm đối số sau driver trong `addDatabase()`. Nếu không có tên nào được chọn thì kết nối mặc định sẽ được sử dụng
- `prepare()` chuẩn bị truy vấn để thực thi, được chuẩn bị thành công thì các giá trị có thể được liên kết với các _field_ bằng `addBindValue()`
---

