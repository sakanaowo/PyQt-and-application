Giao diện login cơ bản:
![](https://github.com/sakanaowo/PyQt-and-application/blob/main/Image/Pasted%20image%2020240719152526.png?raw=true)

Giao diện đăng ký cơ bản:
![](https://github.com/sakanaowo/PyQt-and-application/blob/main/Image/Pasted%20image%2020240719152644.png?raw=true)

### <span style="color:rgb(0, 176, 240)">Thiết kế GUI login và registration</span>
- Khi thiết kế GUI: 
	- cần tạo sự rõ ràng giữa giao diện đăng nhập và đăng ký
	- Giúp user điều hướng nếu gặp các lỗi tiềm ẩn, ví dụ như :
		- Caps lock
		- Lỗi tên người dùng
		- blah blah
- Một số thành phần chính:
	- Vùng nhập tên và mật khẩu
	- Checkbox ghi nhớ thông tin đăng nhập
	- Button tạo tài khoản hoặc đăng ký mới

- Trong cửa sổ đăng nhập:
	- 2 widget QLineEdit để nhập tài khoản và mật khẩu
		- Khi click vào Login button, một tín hiệu được truyền đến slot kiểm tra nếu xác thực được tài khoản
		- Hộp thoại QMessageBox hiển thị nếu tài khoản có tồn tại
		- Nếu người dùng tồn tại thì sẽ được chuyển đến giao diện chính của app
	- 1 checkbox để hiển thị mật khẩu
	- 2 QPushButton widget để login hoặc tạo tài khoản mới
		- Click vào Sign up button để tạo tài khoản mới
		- Người dùng không thể thao tác với giao diện Login nếu chưa đóng giao diện tạo tài khoản
	- Khi đóng cửa sổ đăng nhập, thay vì đóng ứng dụng thì sẽ dẫn đến `closeEvent()` để tùy chỉnh cách đóng ứng dụng
## <span style="color:rgb(0, 176, 240)">Tạo Login GUI</span> 
### <span style="color:rgb(224, 187, 228)">import các thư viện cần thiết</span>:
```python
import sys  
from PyQt6.QtWidgets import (QApplication, QWidget, QLabel, QLineEdit, QPushButton, QCheckBox, QMessageBox)  
from PyQt6.QtGui import QFont, QPixmap  
from PyQt6.QtCore import Qt  
from registration import NewUserDialog
```
note: Ở đây, tạo một file .py tên "registration.py" để tạo thư viện registration
### <span style="color:rgb(224, 187, 228)">Tạo cửa sổ Login</span>:
```python
class LoginWindow(QWidget):  
    def __init__(self):  
        super().__init__()  
        self.initializeUI()  
  
    def initializeUI(self):  
        """Set up the application's GUI."""  
        self.setFixedSize(360, 220)  
        self.setWindowTitle("3.1 – Login GUI")  
        self.setUpWindow()  
        self.show()
```
note:
- Lớp này không phải là cửa sổ chính cho app 
- `setFixedSize()` ngăn chặn tùy chỉnh cửa sổ
### <span style="color:rgb(224, 187, 228)">setUpMainWindow cho Login GUI:</span> 
#### nửa trên:
```python
def setUpWindow(self):  
    """Create and arrange widgets in the main window."""  
    self.login_is_successful =  False
  
    login_label = QLabel("Login", self)  
    login_label.setFont(QFont("Arial", 20))  
    login_label.move(160, 10)  
  
    # Create widgets for username and password  
    username_label = QLabel("Username:", self)  
    username_label.move(20, 54)  
  
    self.username_edit = QLineEdit(self)  
    self.username_edit.resize(250, 24)  
    self.username_edit.move(90, 50)  
  
    password_label = QLabel("Password:", self)  
    password_label.move(20, 86)  
  
    self.password_edit = QLineEdit(self)  
    self.password_edit.setEchoMode(QLineEdit.EchoMode.Password)  
    self.password_edit.resize(250, 24)  
    self.password_edit.move(90, 82)
```
note:
- `login_is_successfull` lưu trạng thái đăng nhập hoặc không
- `login_label`:
	- tạo nhãn với văn bản "Login"
	- thiết lập font cho nhãn: `setFont`
	- căn chỉnh vị trị tại (160,10)
- Tạo và sắp xếp label username:
	- `username_label`:
		- tạo nhãn và để tên là "Username"
	- `username_edit`:
		- tạo ô nhập dữ liệu bằng QLineEdit 
		- căn chỉnh ô nhập liệu bằng `resize` và `move`
- Tạo và sắp xếp label password:
	- `password_label`: 
	- `password_edit`:
		- tạo ô nhập liệu bằng QLineEdit
		- ẩn văn bản bằng `setEchoMode`: .setEchoMode(QLineEdit.EchoMode.Password)
		- căn chỉnh bằng `move` và `resize` 
#### nửa dưới
```python
self.show_password_cb = QCheckBox("Show Password", self)  
self.show_password_cb.move(90, 110)  
self.show_password_cb.toggled.connect(self.displayPasswordIfChecked)  
  
# Create QPushButton for signing in  
login_button = QPushButton("Login", self)  
login_button.resize(320, 34)  
login_button.move(20, 140)  
login_button.clicked.connect(self.clickLoginButton)  
# Create sign up QLabel and QPushButton  
not_member_label = QLabel("Not a member?", self)  
not_member_label.move(20, 186)  
sign_up_button = QPushButton("Sign Up", self)  
sign_up_button.move(120, 180)  
sign_up_button.clicked.connect(self.createNewUser)
```
note:
- Tạo checkbox `show_password_cb` để hiển thị mật khẩu:
	- sử dụng QCheckBox
	- căn chỉnh vị trí bằng `move`
	- kết nối tới phương thức `displayPassswordIfChecked`
- Tạo nút đăng nhập `login_button`
	- Tạo bằng QPushButton
	- kết nối tới `clickLoginButton` nếu button đã được `clicked`
- Tạo label đăng ký:
	- Tạo QLabel "not a member?" và căn chỉnh bằng `move`
	- Tạo nút QPushButton "Sign up" để đăng ký
	- Nếu nút "Sign up" được `clicked` thì kết nối tới `createNewUser`
### <span style="color:rgb(224, 187, 228)">slot</span> `clickLoginButton()`:
Kiểm tra nếu tài khoản-mật khẩu có trong file users.txt
Nếu có, hiển thị một QMessage và đóng app. Nếu không, hiển thị cảnh báo và đóng app
#### nửa trên:
```python
def clickLoginButton(self):  
    users = {}  # Dictionary to store user information  
    file = "files/users.txt"  
    try:  
        with open(file, "r") as f:  
            for line in f:  
                user_info = line.split(" ")  
                username_info = user_info[0]  
                password_info = user_info[1].strip("\n")  
                users[username_info] = password_info  
  
        # Collect user and password information  
        username = self.username_edit.text()  
        password = self.password_edit.text()
```
note:
- mở file users.txt 
	- lấy username bằng `user_info[0]`
	- lấy password bằng `user_info[1]`, `.strip("\n")` để ngắt dấu xuống dòng
	- nạp mật khẩu và tài khoản vào từ điển _users_
- lấy username và password người dùng đã nhập bằng `.text()`
#### nửa sau:
```python
		if (username, password) in users.items():  
	        QMessageBox.information(self,  
	                                "Login Successful!",  
	                                "Login Successful!",  
	                                QMessageBox.StandardButton.Ok,  
	                                QMessageBox.StandardButton.Ok)  
	        self.login_is_successful = True  
	        self.close()  # Close the login window  
	        self.openApplicationWindow()  
	    else:  
	        QMessageBox.warning(self, "Error Message",  
	                            "The username or password is incorrect.",  
	                            QMessageBox.StandardButton.Close,  
	                            QMessageBox.StandardButton.Close)  
	except FileNotFoundError as error:  
	    QMessageBox.warning(self, "Error",  
	                        f"""<p>File not found.</p>  
	 <p>Error: {error}</p>""",  
	                        QMessageBox.StandardButton.Ok)  
	    # Create file if it doesn't exist  
	    f = open(file, "w")
```
note:
- Kiểm tra thông tin đăng nhập:
	- Nếu người dùng có trong DB
- Xử lý nếu file không tồn tại:
	- Thông báo lỗi "File not found"
	- Tạo mới file `f = open(file,"w")`
### slot `displayPasswordIfChecked()`:

Nếu QCheckButton được bật thì hiển thị mật khẩu

```python
def displayPasswordIfChecked(self, checked):  
    """If QCheckButton is enabled, view the password.  
    Else, mask the password so others can not see it."""    
    if checked:  
        self.password_edit.setEchoMode(  
            QLineEdit.EchoMode.Normal)  
    elif not checked:  
        self.password_edit.setEchoMode(  
            QLineEdit.EchoMode.Password)
```
note:
- Nếu `show_password_cb` đã được bật thì mật khẩu sẽ được hiển thị sử dụng `setEchoMode()` thông qua QLineEdit:
### <span style="color:rgb(224, 187, 228)">Tạo cửa sổ hoặc hộp thoại đăng ký</span>
#### <span style="color:rgb(20, 225, 191)">Tạo cửa sổ đăng ký mới</span>
```python
def createNewUser(self):  
    """Open a dialog for creating a new account."""  
    self.create_new_user_window = NewUserDialog()  
    self.create_new_user_window.show()
```

#### <span style="color:rgb(20, 225, 191)">Tạo cửa sổ ứng dụng</span>
```python
def openApplicationWindow(self):  
    """Open a mock main window after the user logs in."""  
  
    self.main_window = MainWindow()  
    self.main_window.show()
```

### <span style="color:rgb(224, 187, 228)">Event Handler cho đóng cửa sổ</span> 
```python
def closeEvent(self, event):  
    """Reimplement the closing event to display a QMessageBox before closing."""    
    if self.login_is_successful == True:  
        event.accept()  
    else:  
        answer = QMessageBox.question(  
            self, "Quit Application?",  
            "Are you sure you want to QUIT?",  
            QMessageBox.StandardButton.No | \  
            QMessageBox.StandardButton.Yes,  
            QMessageBox.StandardButton.Yes)  
        if answer == QMessageBox.StandardButton.Yes:  
            event.accept()  
        if answer == QMessageBox.StandardButton.No:  
            event.ignore()
```
note: 
- Nếu đã login thì cửa sổ sẽ đóng
- Ngược lại, hiển thị hộp thoại có câu hỏi với hai nút _No_ và _Yes_, mặc định là _Yes_
	- Nếu chọn _Yes_ thì sẽ đóng cửa sổ
	- Ngược lại thì cửa sổ sự kiện bị bỏ qua và cửa sổ login sẽ không đóng lại

## <span style="color:rgb(186, 225, 255)">Tạo MainWindow</span> 
```python
class MainWindow(QWidget):  
    def __init__(self):  
        super().__init__()  
        self.initializeUI()  
  
    def initializeUI(self):  
        self.setMinimumSize(640, 426)  
        self.setWindowTitle("Main Window hehe")  
        self.setUpMainWindow()  
  
    def setUpMainWindow(self):  
        image = "image/background.jpg"  
  
        try:  
            with open(image):  
                main_label = QLabel(self)  
                pixmap=QPixmap(image)  
                main_label.setPixmap(pixmap)  
                main_label.move(0,0)  
        except FileNotFoundError as error:  
            print(f"Image not found. \nError: {error}")
```
note:
- Không có `show()` vì Main Window chỉ hiển thị khi đã login

### <span style="color:rgb(224, 187, 228)">Tạo thư viện registration</span>

```python
import sys  
from PyQt6.QtWidgets import (QApplication, QDialog, QLabel, QPushButton, QLineEdit, QMessageBox)  
from PyQt6.QtGui import QFont, QPixmap  
  
  
class NewUserDialog(QDialog):  
    def __init__(self):  
        super().__init__()  
        self.setModal(True)  
        self.initializeUI()  
  
    def initializeUI(self):  
        """Set up the application's GUI."""  
        self.setFixedSize(360, 320)  
        self.setWindowTitle("3.1 - Registration GUI")  
        self.setUpWindow()  
  
    def setUpWindow(self):  
        login_label = QLabel("Create New Account", self)  
        login_label.setFont(QFont("Arial", 20))  
        login_label.move(90, 20)  
  
        # label 4 image  
  
        user_image = "images/new_user_icon.png"  
  
        try:  
            with open(user_image):  
                user_label = QLabel(self)  
                pixmap = QPixmap(user_image)  
                user_label.setPixmap(pixmap)  
                user_label.move(150, 60)  
        except FileNotFoundError as error:  
            print(f"Image not found :< \nError: {error}")  
  
        # Create name QLabel and QLineEdit widget  
        name_label = QLabel("User name:", self)  
        name_label.move(20, 144)  
  
        self.name_edit = QLineEdit(self)  
        self.name_edit.resize(250, 24)  
        self.name_edit.move(90, 144)  
  
        fullname_label = QLabel("Full Name:", self)  
        fullname_label.move(20, 174)  
  
        fullname_edit = QLineEdit(self)  
        fullname_edit.resize(250, 24)  
        fullname_edit.move(90, 170)  
  
        # create password  
        newpws_label = QLabel("Password:", self)  
        newpws_label.move(20, 204)  
  
        self.newpws_edit = QLineEdit(self)  
        self.newpws_edit.setEchoMode(QLineEdit.EchoMode.Password)  
        self.newpws_edit.resize(250, 24)  
        self.newpws_edit.move(90, 200)  
  
        confirm_label = QLabel("Confirm:", self)  
        confirm_label.move(20, 234)  
  
        self.confirm_edit = QLineEdit(self)  
        self.confirm_edit.setEchoMode(QLineEdit.EchoMode.Password)  
        self.confirm_edit.resize(250, 24)  
        self.confirm_edit.move(90, 230)  
  
        # create signup button  
        signup_button = QPushButton("Sign Up", self)  
        signup_button.resize(320, 32)  
        signup_button.move(20, 270)  
        signup_button.clicked.connect(self.confirmSignUp)  
  
    def confirmSignUp(self):  
        name_text = self.name_edit.text()  
        pswd_text = self.newpws_edit.text()  
        confirm_text = self.confirm_edit.text()  
  
        if name_text == "" or pswd_text == "":  
            QMessageBox.warning(self, "Error", "Invalid username or password.",  
                                QMessageBox.StandardButton.Close,  
                                QMessageBox.StandardButton.Close)  
        elif pswd_text != confirm_text:  
            QMessageBox.warning(self, "Error", "The password you entered do not match.",  
                                QMessageBox.StandardButton.Close,  
                                QMessageBox.StandardButton.Close)  
        else:  
            with open("users.txt", "w") as f:  
                f.write("\n" + name_text + " ")  
                f.write(pswd_text)  
            self.close()
```

## Full code:
Login GUI:
```python
import sys  
from PyQt6.QtWidgets import (QApplication, QWidget, QLabel, QLineEdit, QPushButton, QCheckBox, QMessageBox)  
from PyQt6.QtGui import QFont, QPixmap  
from PyQt6.QtCore import Qt  
from registration import NewUserDialog  
  
  
class LoginWindow(QWidget):  
    def __init__(self):  
        super().__init__()  
        self.initializeUI()  
  
    def initializeUI(self):  
        """Set up the application's GUI."""  
        self.setFixedSize(360, 220)  
        self.setWindowTitle("3.1 – Login GUI")  
        self.setUpWindow()  
        self.show()  
  
    def setUpWindow(self):  
        """Create and arrange widgets in the main window."""  
        self.login_is_successful = False  
  
        login_label = QLabel("Login", self)  
        login_label.setFont(QFont("Arial", 20))  
        login_label.move(160, 10)  
  
        # Create widgets for username and password  
        username_label = QLabel("Username:", self)  
        username_label.move(20, 54)  
  
        self.username_edit = QLineEdit(self)  
        self.username_edit.resize(250, 24)  
        self.username_edit.move(90, 50)  
  
        password_label = QLabel("Password:", self)  
        password_label.move(20, 86)  
  
        self.password_edit = QLineEdit(self)  
        self.password_edit.setEchoMode(QLineEdit.EchoMode.Password)  
        self.password_edit.resize(250, 24)  
        self.password_edit.move(90, 82)  
  
        self.show_password_cb = QCheckBox("Show Password", self)  
        self.show_password_cb.move(90, 110)  
        self.show_password_cb.toggled.connect(self.displayPasswordIfChecked)  
  
        # Create QPushButton for signing in  
        login_button = QPushButton("Login", self)  
        login_button.resize(320, 34)  
        login_button.move(20, 140)  
        login_button.clicked.connect(self.clickLoginButton)  
        # Create sign up QLabel and QPushButton  
        not_member_label = QLabel("Not a member?", self)  
        not_member_label.move(20, 186)  
        sign_up_button = QPushButton("Sign Up", self)  
        sign_up_button.move(120, 180)  
        sign_up_button.clicked.connect(self.createNewUser)  
  
    def clickLoginButton(self):  
        users = {}  # Dictionary to store user information  
        file = "users.txt"  
        try:  
            with open(file, "r") as f:  
                for line in f:  
                    user_info = line.split(" ")  
                    username_info = user_info[0]  
                    password_info = user_info[1].strip("\n")  
                    users[username_info] = password_info  
  
            # Collect user and password information  
            username = self.username_edit.text()  
            password = self.password_edit.text()  
  
            if (username, password) in users.items():  
                QMessageBox.information(self,  
                                        "Login Successful!",  
                                        "Login Successful!",  
                                        QMessageBox.StandardButton.Ok,  
                                        QMessageBox.StandardButton.Ok)  
                self.login_is_successful = True  
                self.close()  # Close the login window  
                self.openApplicationWindow()  
            else:  
                QMessageBox.warning(self, "Error Message",  
                                    "The username or password is incorrect.",  
                                    QMessageBox.StandardButton.Close,  
                                    QMessageBox.StandardButton.Close)  
        except FileNotFoundError as error:  
            QMessageBox.warning(self, "Error",  
                                f"""<p>File not found.</p>  
         <p>Error: {error}</p>""",  
                                QMessageBox.StandardButton.Ok)  
            # Create file if it doesn't exist  
            f = open(file, "w")  
  
    def displayPasswordIfChecked(self, checked):  
        """If QCheckButton is enabled, view the password.  
        Else, mask the password so others can not see it."""        
        if checked:  
            self.password_edit.setEchoMode(  
                QLineEdit.EchoMode.Normal)  
        elif not checked:  
            self.password_edit.setEchoMode(  
                QLineEdit.EchoMode.Password)  
  
    def createNewUser(self):  
        """Open a dialog for creating a new account."""  
        self.create_new_user_window = NewUserDialog()  
        self.create_new_user_window.show()  
  
    def openApplicationWindow(self):  
        """Open a mock main window after the user logs in."""  
  
        self.main_window = MainWindow()  
        self.main_window.show()  
  
    def closeEvent(self, event):  
        """Reimplement the closing event to display a  
        QMessageBox before closing."""        if self.login_is_successful:  
            event.accept()  
        else:  
            answer = QMessageBox.question(  
                self, "Quit Application?",  
                "Are you sure you want to QUIT?",  
                QMessageBox.StandardButton.No | \  
                QMessageBox.StandardButton.Yes,  
                QMessageBox.StandardButton.Yes)  
  
            if answer == QMessageBox.StandardButton.Yes:  
                event.accept()  
            if answer == QMessageBox.StandardButton.No:  
                event.ignore()  
  
  
class MainWindow(QWidget):  
    def __init__(self):  
        super().__init__()  
        self.initializeUI()  
  
    def initializeUI(self):  
        self.setMinimumSize(640, 426)  
        self.setWindowTitle("Main Window hehe")  
        self.setUpMainWindow()  
        self.show()  
  
    def setUpMainWindow(self):  
        image = "images/background.jpg"  
  
        try:  
            with open(image):  
                main_label = QLabel(self)  
                pixmap = QPixmap(image)  
                main_label.setPixmap(pixmap)  
                main_label.move(0, 0)  
        except FileNotFoundError as error:  
            print(f"Image not found. \nError: {error}")  
  
  
if __name__ == '__main__':  
    app = QApplication(sys.argv)  
    window = LoginWindow() 
    sys.exit(app.exec())
```