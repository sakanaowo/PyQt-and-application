Dùng để thu thập, sửa đổi dữ liệu. Còn có thể ẩn văn bản khi đã enter bằng placeholder.
note: nếu cần nhập nhiều dòng dữ liệu thì nên dùng QTextEdit


![](https://github.com/sakanaowo/PyQt-and-application/blob/main/Image/Pasted%20image%2020240714203212.png?raw=true)

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

Sử dụng phương thức từ QWidget là setMaximumSize() để thiết lập vị trí và kích thước của cửa sổ.
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

![](https://github.com/sakanaowo/PyQt-and-application/blob/main/Image/Pasted%20image%2020240714203221.png?raw=true)
