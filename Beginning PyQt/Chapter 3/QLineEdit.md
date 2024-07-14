Dùng để thu thập, sửa đổi dữ liệu

Còn có thể ẩn văn bản khi đã enter bằng placeholder 

note: nếu cần nhập nhiều dòng dữ liệu thì nên dùng QTextEdit

![](https://private-user-images.githubusercontent.com/131884955/348526232-707988d6-7aec-4831-8a12-5f58f7268476.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjA5NDc3NzUsIm5iZiI6MTcyMDk0NzQ3NSwicGF0aCI6Ii8xMzE4ODQ5NTUvMzQ4NTI2MjMyLTcwNzk4OGQ2LTdhZWMtNDgzMS04YTEyLTVmNThmNzI2ODQ3Ni5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNzE0JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDcxNFQwODU3NTVaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT05MzAxMGUwM2UxOGFlMTBjNDdjMDNlMzZhYmMxZWNlNTY1ZjM5NWI3MDdmNGU4ODE2ZmI1ZDkxNjVkZjllYjM2JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.e0gflzntRWgcioleF7GPogTxl0cUptVmhfCvqEe3GtQ)

### <span style="color:rgb(0, 176, 240)">Thiết lập MainWindow </span>
```python
import sys  
from PyQt6.QtWidgets import (QApplication, QWidget, QLabel, QLineEdit, QPushButton)  
from PyQt6.QtCore import Qt  
  
  
class MainWindow(QWidget):  
    def __init__(self):  
        super().__init__()  
	    self.initializeUI()  
  
    def initializeUI(self):  
        """Set up the application's GUI."""  
        self.setMaximumSize(310, 130)  
        self.setWindowTitle("QLineEdit Example")  
        self.setUpMainWindow()  
        self.show()  
  
  
if __name__ == '__main__':  
    app = QApplication(sys.argv)  
    window = MainWindow()  
    sys.exit(app.exec())
```

Sử dụng phương thức từ QWidget là setMaximumSize() để thiết lập vị trí và kích thước của cửa sổ
note: Các phương thức thường dùng:
- `setMinimumSize()`
- `setMinimumHeight()` và `setMinimumWidth()`
- `setFixedSize()`: đặt lại maximum và minimum size cho widget, ngăn việc thay đổi kích thước
### <span style="color:rgb(0, 176, 240)">setUpMainWindow():</span>
```python
def setUpMainWindow(self):  
    """Create and arrange widgets in the main window."""  
    QLabel("Please enter your name below.",  
           self).move(70, 10)  
      
    name_label = QLabel("Name:", self)  
    name_label.move(20, 50)  
      
    self.name_edit = QLineEdit(self)  
    self.name_edit.resize(210, 20)  
    self.name_edit.move(70, 50)  
      
    clear_button = QPushButton("Clear", self)  
    clear_button.move(140, 90)  
    clear_button.clicked.connect(self.clearText)  
      
    accept_button = QPushButton("OK", self)  
    accept_button.move(210, 90)  
    accept_button.clicked.connect(self.acceptText)
```
note: có thể tạo widget và sắp xếp chúng trong GUI mà không cần khai báo 

### <span style="color:rgb(0, 176, 240)">clearText() và acceptText():</span>
```python
def clearText(self):  
    """Clear the QLineEdit input field."""  
    self.name_edit.clear()  
  
def acceptText(self):  
    """Accept the user's input in the QLineEdit  
    widget and close the program."""    
    
    print(self.name_edit.text())  
    self.close()
```

## Full code và kết quả:
```python
import sys  
from PyQt6.QtWidgets import (QApplication, QWidget, QLabel, QLineEdit, QPushButton)  
from PyQt6.QtCore import Qt  
  
  
class MainWindow(QWidget):  
    def __init__(self):  
        super().__init__()  
        self.initializeUI()  
  
    def initializeUI(self):  
        """Set up the application's GUI."""  
        self.setMaximumSize(310, 130)  
        self.setWindowTitle("QLineEdit Example")  
        self.setUpMainWindow()  
        self.show()  
  
    def setUpMainWindow(self):  
        """Create and arrange widgets in the main window."""  
        QLabel("Please enter your name below.",  
               self).move(70, 10)  
  
        name_label = QLabel("Name:", self)  
        name_label.move(20, 50)  
  
        self.name_edit = QLineEdit(self)  
        self.name_edit.resize(210, 20)  
        self.name_edit.move(70, 50)  
  
        clear_button = QPushButton("Clear", self)  
        clear_button.move(140, 90)  
        clear_button.clicked.connect(self.clearText)  
  
        accept_button = QPushButton("OK", self)  
        accept_button.move(210, 90)  
        accept_button.clicked.connect(self.acceptText)  
  
    def clearText(self):  
        """Clear the QLineEdit input field."""  
        self.name_edit.clear()  
  
    def acceptText(self):  
        """Accept the user's input in the QLineEdit  
        widget and close the program."""  
        print(self.name_edit.text())  
        self.close()  
  
  
if __name__ == '__main__':  
    app = QApplication(sys.argv)  
    window = MainWindow()  
    sys.exit(app.exec())
```

![](https://private-user-images.githubusercontent.com/131884955/348527717-d6d31166-481f-4951-8c84-32db106330f6.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjA5NDk2NDUsIm5iZiI6MTcyMDk0OTM0NSwicGF0aCI6Ii8xMzE4ODQ5NTUvMzQ4NTI3NzE3LWQ2ZDMxMTY2LTQ4MWYtNDk1MS04Yzg0LTMyZGIxMDYzMzBmNi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjQwNzE0JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI0MDcxNFQwOTI5MDVaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT0xNmY0ZGE5OTRlNGVkYzc5ZjI3ZjE1ZTg1YmEyZmFhM2Q0MTM0ZDU1YmNjZTI3MjQ4YWNlOGU0YmQyYTA5N2YzJlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCZhY3Rvcl9pZD0wJmtleV9pZD0wJnJlcG9faWQ9MCJ9.UuhBJBwbpEHtBy6oIMdIE0zlUnrjAjH91_kCvyrGukM)
