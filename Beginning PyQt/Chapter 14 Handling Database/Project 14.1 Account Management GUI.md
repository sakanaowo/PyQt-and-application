
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

#### <span style="color:rgb(255, 238, 140)">convert .db to .sql</span>
- Tải SQLITE  tool về máy: [link](https://www.sqlite.org/download.html)
- [Thêm sqlite vào PATH](https://www.linkedin.com/pulse/part-5-how-install-sqlite-your-machine-windows-linux-mac-julles/)
- Chuyển file .db -> .sql:
	```css
	sqlite3 file.db .dump >> file.sql
	```

---
NOTE: 
-  **Một kết nối được tham chiếu bằng tên kết nối, không phải tên của DB**
- Nếu muốn đặt tên cho DB, phải truyền tên đó làm đối số sau driver trong `addDatabase()`. Nếu không có tên nào được chọn thì kết nối mặc định sẽ được sử dụng
- `prepare()` chuẩn bị truy vấn để thực thi, được chuẩn bị thành công thì các giá trị có thể được liên kết với các _field_ bằng `addBindValue()`
---

## <span style="color:rgb(255, 105, 97)">Working with the QSqlTableModel class</span> 

Hiển thị DB dưới dạng table trong python:
```python
# table_model.py
import sys  
  
from PyQt6.QtSql import QSqlDatabase, QSqlTableModel  
from PyQt6.QtWidgets import QWidget, QMessageBox, QTableView, QHeaderView, QVBoxLayout, QApplication  
  
  
class MainWindow(QWidget):  
    def __init__(self):  
        super().__init__()  
        self.initUI()  
  
    def initUI(self):  
        self.setMinimumSize(1000, 500)  
        self.setWindowTitle("SQL Table Model")  
  
        self.createConnection()  
        self.setup()  
        self.show()  
  
    def createConnection(self):  
        """set up the connnection to the DB  
        check for table needed"""        
        database = QSqlDatabase.addDatabase('QSQLITE')  
        database.setDatabaseName('files/accounts.db')  
  
        if not database.open():  
            print("Unable to open database")  
            sys.exit(1)  
  
        table_needed = {"accounts"}  
        table_not_found = table_needed - set(database.tables())  
  
        if table_not_found:  
            QMessageBox.critical(  
                None, "Error",  
                f"""<p> The following tables are missing in the database: {table_not_found}</p>"""  
            )  
            sys.exit(1)  
  
    def setup(self):  
        model = QSqlTableModel()  
        model.setTable("accounts")  
  
        table_view = QTableView()  
        table_view.setModel(model)  
        table_view.horizontalHeader().setSectionResizeMode(  
            QHeaderView.ResizeMode.Stretch)  
  
        model.select()  # select all
  
        main_vbox = QVBoxLayout()  
        main_vbox.addWidget(table_view)  
  
        self.setLayout(main_vbox)  
  
  
if __name__ == "__main__":  
    app = QApplication(sys.argv)  
    window = MainWindow()  
    sys.exit(app.exec())
```

## <span style="color:rgb(255, 105, 97)">Working with QSqlRelationalTableModel</span> 

Lớp **QSqlRelationalTableModel** để làm việc với cơ sở dữ liệu quan hệ. Lớp **QSqlRelationalTableModel** cung cấp một model để quản lý và chỉnh sửa dữ liệu trong bảng SQL, đồng thời hỗ trợ thêm cho việc sử dụng các **khóa ngoại**. Khóa ngoại là một ràng buộc SQL được sử dụng để liên kết các bảng với nhau.

```python
# relational_model.py
import sys  
  
from PyQt6.QtSql import QSqlDatabase, QSqlRelationalTableModel, QSqlRelationalDelegate, QSqlRelation  
from PyQt6.QtWidgets import QWidget, QApplication, QMessageBox, QTableView, QHeaderView, QVBoxLayout  
  
  
class MainWindow(QWidget):  
    def __init__(self):  
        super().__init__()  
        self.initUI()  
  
    def initUI(self):  
        self.setWindowTitle("Relational Table Model")  
        self.setMinimumSize(1000, 500)  
  
        self.createConnection()  
        self.setup()  
        self.show()  
  
    def createConnection(self):  
        """setup the connnection to the DB"""  
        database = QSqlDatabase.addDatabase("QSQLITE")  
        database.setDatabaseName("files/accounts.db")  
  
        if not database.open():  
            print("Database connection failed")  
            sys.exit(1)  
  
        table_needed = {"accounts", "countries"}  
        table_not_found = table_needed - set(database.tables())  
        if table_not_found:  
            QMessageBox.critical(  
                None, "Error",  
                f"""<p>The following tables are missing from the databse:   
{table_not_found}  
                </p>"""            )  
            sys.exit(1)  
  
    def setup(self):  
        model = QSqlRelationalTableModel()  
        model.setTable("accounts")  
        # set up relationship for foreign key  
        model.setRelation(model.fieldIndex("country_id"),  
                          QSqlRelation("countries", "id", "country"))  
  
        table_view = QTableView()  
        table_view.setModel(model)  
        table_view.horizontalHeader().setSectionResizeMode(  
            QHeaderView.ResizeMode.Stretch  
        )  
  
        model.select()  
  
        # instantiate the delegate  
        delegate = QSqlRelationalDelegate()  
        table_view.setItemDelegate(delegate)  
  
        main_vbox = QVBoxLayout()  
        main_vbox.addWidget(table_view)  
        self.setLayout(main_vbox)  
  
  
if __name__ == '__main__':  
    app = QApplication(sys.argv)  
    window = MainWindow()  
    sys.exit(app.exec())
```
---
NOTE:
 **`model.setRelation(...)`**: Thiết lập quan hệ giữa bảng `accounts` và `countries` thông qua khóa ngoại `country_id`. `QSqlRelation` chỉ định rằng trường `country_id` của bảng `accounts` liên quan đến trường `id` của bảng `countries`, và `country` là cột cần hiển thị từ bảng `countries`.

---
## <span style="color:rgb(255, 105, 97)">Account  management GUI</span> 

Ứng dụng này cho phép người dùng thêm, xóa, và sắp xếp nội dung của bảng. Khi thêm hoặc xóa hàng, dữ liệu trong cơ sở dữ liệu cũng sẽ được cập nhật.

```python
# account manager.py
import os  
import sys  
  
from PyQt6.QtCore import Qt  
from PyQt6.QtGui import QIcon  
from PyQt6.QtSql import QSqlDatabase, QSqlTableModel, QSqlRelationalTableModel, QSqlRelation, QSqlRelationalDelegate, \  
    QSqlQuery  
from PyQt6.QtWidgets import QWidget, QMessageBox, QLabel, QSizePolicy, QPushButton, QComboBox, QHBoxLayout, QTableView, \  
    QHeaderView, QAbstractItemView, QVBoxLayout, QApplication  
  
  
  
class MainWindow(QWidget):  
    def __init__(self):  
        super().__init__()  
        self.initUI()  
  
    def initUI(self):  
        self.setWindowTitle("Account Manager")  
        self.setMinimumSize(1000, 600)  
  
        self.createConnection()  
        self.createModel()  
        self.setup()  
        self.show()  
  
    def createConnection(self):  
        database = QSqlDatabase.addDatabase("QSQLITE")  
        database.setDatabaseName("files/accounts.db")  
  
        if not database.open():  
            print("Error opening database")  
            sys.exit(1)  
  
        table_needed = {"accounts", "countries"}  
        table_not_found = table_needed - set(database.tables())  
        if table_not_found:  
            QMessageBox.critical(  
                None, "Error",  
                f"""<p>The following tables are missing: {table_not_found}</p>"""  
            )  
            sys.exit(1)  
  
    def createModel(self):  
        self.model = QSqlRelationalTableModel()  
        self.model.setTable("accounts")  
        self.model.setRelation(  
            self.model.fieldIndex("country_id"),  
            QSqlRelation("countries", "id", "country"))  
  
        self.model.setHeaderData(  
            self.model.fieldIndex("id"),  
            Qt.Orientation.Horizontal, "ID")  
        self.model.setHeaderData(  
            self.model.fieldIndex("employee_id"),  
            Qt.Orientation.Horizontal, "Employee ID")  
        self.model.setHeaderData(  
            self.model.fieldIndex("first_name"),  
            Qt.Orientation.Horizontal, "First")  
        self.model.setHeaderData(  
            self.model.fieldIndex("last_name"),  
            Qt.Orientation.Horizontal, "Last")  
        self.model.setHeaderData(  
            self.model.fieldIndex("email"),  
            Qt.Orientation.Horizontal, "E-mail")  
        self.model.setHeaderData(  
            self.model.fieldIndex("department"),  
            Qt.Orientation.Horizontal, "Dept.")  
        self.model.setHeaderData(  
            self.model.fieldIndex("country_id"),  
            Qt.Orientation.Horizontal, "Country")  
  
        self.model.select()  
  
    def setup(self):  
        icon_path = "icons"  
  
        title = QLabel("Account Management System")  
        title.setSizePolicy(QSizePolicy.Policy.Fixed,  
                            QSizePolicy.Policy.Fixed)  
        title.setStyleSheet("font: bold 24px")  
  
        add_product_button = QPushButton("Add Employee")  
        add_product_button.setIcon(  
            QIcon(os.path.join(icon_path, "add_user.png")))  
        add_product_button.setStyleSheet("padding: 10px")  
        add_product_button.clicked.connect(self.addItem)  
  
        del_products_button = QPushButton("Delete")  
        del_products_button.setIcon(  
            QIcon(os.path.join(icon_path, "trash_can.png")))  
        del_products_button.setStyleSheet("padding: 10px")  
        del_products_button.clicked.connect(self.deleteItem)  
  
        # set up sorting combobox  
        sorting_option = [  
            "Sort by ID", "Sort by Employee ID",  
            "Sort by First Name", "Sort by Last Name",  
            "Sort by Department", "Sort by Country"]  
        sort_combo = QComboBox()  
        sort_combo.addItems(sorting_option)  
        sort_combo.currentTextChanged.connect(self.setSortingOrder)  
  
        button_hbox = QHBoxLayout()  
        button_hbox.addWidget(add_product_button)  
        button_hbox.addWidget(del_products_button)  
        button_hbox.addStretch()  
        button_hbox.addWidget(sort_combo)  
  
        edit_container = QWidget()  
        edit_container.setLayout(button_hbox)  
  
        # create table view and set model  
        self.table_view = QTableView()  
        self.table_view.setModel(self.model)  
  
        horizontal = self.table_view.horizontalHeader()  
        horizontal.setSectionResizeMode(  
            QHeaderView.ResizeMode.Stretch)  
        vertical = self.table_view.verticalHeader()  
        vertical.setSectionResizeMode(  
            QHeaderView.ResizeMode.Stretch)  
  
        self.table_view.setSelectionMode(  
            QAbstractItemView.SelectionMode.SingleSelection)  
        self.table_view.setSelectionBehavior(  
            QAbstractItemView.SelectionBehavior.SelectRows)  
  
        delegate = QSqlRelationalDelegate()  
        self.table_view.setItemDelegate(delegate)  
  
        main_vbox = QVBoxLayout()  
        main_vbox.addWidget(title, Qt.AlignmentFlag.AlignLeft)  
        main_vbox.addWidget(edit_container)  
        main_vbox.addWidget(self.table_view)  
        self.setLayout(main_vbox)  
  
    def addItem(self):  
        last_row = self.model.rowCount()  
        self.model.insertRow(last_row)  
  
        query = QSqlQuery()  
        query.exec("Select max (id) from accounts")  
        if query.next():  
            int(query.value())  
  
    def deleteItem(self):  
        currentItem = self.table_view.selectedIndexes()  
        for index in currentItem:  
            self.model.removeRow(index.row())  
        self.model.select()  
  
    def setSortingOrder(self, text):  
        if text == "Sort by ID":  
            self.model.setSort(self.model.fieldIndex("id"),  
                               Qt.SortOrder.AscendingOrder)  
        elif text == "Sort by Employee ID":  
            self.model.setSort(  
                self.model.fieldIndex("employee_id"),  
                Qt.SortOrder.AscendingOrder)  
        elif text == "Sort by First Name":  
            self.model.setSort(  
                self.model.fieldIndex("first_name"),  
                Qt.SortOrder.AscendingOrder)  
        elif text == "Sort by Last Name":  
            self.model.setSort(  
                self.model.fieldIndex("last_name"),  
                Qt.SortOrder.AscendingOrder)  
        elif text == "Sort by Department":  
            self.model.setSort(  
                self.model.fieldIndex("department"),  
                Qt.SortOrder.AscendingOrder)  
        elif text == "Sort by Country":  
            self.model.setSort(  
                self.model.fieldIndex("country"),  
                Qt.SortOrder.AscendingOrder)  
        self.model.select()  
  
  
if __name__ == "__main__":  
    app = QApplication(sys.argv)  
    window = MainWindow()  
    sys.exit(app.exec())
```

