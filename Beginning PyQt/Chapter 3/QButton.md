Thường được sử dụng để thực hiện các hành động và đưa ra các lựa chọn.
Khi click vào QButton widget, tín hiệu được gửi để kết nối tới hàm, đưa ra các nút 'OK','Next', ... nhưng ta cũng có thể tạo ra các nút riêng với icon riêng, text riêng

---
note:
Có nhiều class button khác nhau như QToolButton để lựa chọn các item trong toolbar hoặc QRadioButton để tạo nhóm nút một lựa chọn

---

Ví dụ 1:
Tạo QButton mà khi click sẽ sử dụng signals and slots để thay đổi văn bản của một QLabel và hiển thị cách xử lý việc đóng cửa sổ :
#### set up main window for using QPushButton :
```python
import sys  
from PyQt6.QtWidgets import QApplication, QWidget, QLabel, QPushButton  
from PyQt6.QtGui import QFont, QPixmap  
from PyQt6.QtCore import Qt

class MainWindow(QWidget):
	def __init__(self):
		super().__init__()
		self.initializeUI()

	def initializeUI(self):
		""" set up the application's GUI """
		self.setGeometry(100,100,250,150)
		self.setWindowTitle("QButton Example")

		self.setUpMainWindow()
		self.show()
if __name__ == '__main__':
	app = QApplication(sys.argv)
	window = MainWindow()
	sys.exit(app.exec())
```
#### setUpMainWindow():
```python
def setUpMainWindow(self):  
    """ create and arrange widgets in the main window"""  
    self.times_pressed = 0  
    self.name_label = QLabel("Don't push", self)  
    self.name_label.setAlignment(  
        Qt.AlignmentFlag.AlignCenter  
    )  
    self.name_label.move(60, 30)  
  
    self.button = QPushButton("push", self)  
    self.button.move(80, 70)  
    self.button.clicked.connect(self.buttonClicked)
```
- `time_pressed`: đếm số lần button được ấn 
- Cửa sổ này chỉ chứa một QLabel và một QPushButton 
- Thay vì sử dụng `setText()` ta có thể hiển thị luôn văn bản khi khởi tạo QLabel cho `name_label`
- Nội dung của widget văn bản có thể được căn chỉnh:
	- AlignLeft: căn lề trái
	- AlignRight: căn lề phải
	- AlignHCenter và AlignVCenter: căn giữa theo chiều ngang và dọc
	- AlignTop và AlignBottom: căn trên và dưới
#### Code for buttonClicked():
```python
def buttonClicked(self):  
    """ handle when the button is clicked  
        Demonstrates how to change text for widgets, update,theirs sizes and locations, and how to close the window    """    
    self.times_pressed += 1  
    if self.times_pressed == 1:  
        self.name_label.setText("Why pressed?")  
    if self.times_pressed == 2:  
        self.name_label.setText("Warning")  
        self.button.setText("Feeling lucky?")  
        self.button.adjustSize()  
        self.button.move(70, 70)  
    if self.times_pressed == 3:  
        print("da window has been closed")  
        self.close()
```
### Full code:
```python
import sys  
from PyQt6.QtWidgets import (QApplication, QWidget, QLabel, QPushButton)  
from PyQt6.QtCore import Qt  
  
  
class MainWindow(QWidget):  
    def __init__(self):  
        super().__init__()  
        self.initializeUI()  
  
    def initializeUI(self):  
        """ set up the application's GUI """  
        self.setGeometry(100, 100, 250, 150)  
        self.setWindowTitle("QButton Example")  
  
        self.setUpMainWindow()  
        self.show()  
  
    def setUpMainWindow(self):  
        """ create and arrange widgets in the main window"""  
        self.times_pressed = 0  
        self.name_label = QLabel("Don't push", self)  
        self.name_label.setAlignment(  
            Qt.AlignmentFlag.AlignCenter  
        )  
        self.name_label.move(60, 30)  
  
        self.button = QPushButton("push", self)  
        self.button.move(80, 70)  
        self.button.clicked.connect(self.buttonClicked)  
  
    def buttonClicked(self):  
        """ handle when the button is clicked  
            Demonstrates how to change text for widgets, update,theirs sizes and locations, and how to close the window        """        self.times_pressed += 1  
  
        if self.times_pressed == 1:  
            self.name_label.setText("Why pressed?")  
        if self.times_pressed == 2:  
            self.name_label.setText("Warning")  
            self.button.setText("Feeling lucky?")  
            self.button.adjustSize()  
            self.button.move(70, 70)  
        if self.times_pressed == 3:  
            print("da window has been closed")  
            self.close()  
  
  
if __name__ == '__main__':  
    app = QApplication(sys.argv)  
    window = MainWindow()  
    sys.exit(app.exec())
```
