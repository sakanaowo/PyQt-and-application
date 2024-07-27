import sys  
  
from PyQt6.QtCore import Qt  
from PyQt6.QtGui import QFont, QIcon  
from PyQt6.QtWidgets import QWidget, QLabel, QApplication, QButtonGroup, QPushButton, QVBoxLayout, QCheckBox, QLineEdit, \  
    QMessageBox  
  
  
class inputAge(QWidget):  
    def __init__(self):  
        super().__init__()  
        self.initializeUI()  
  
    def initializeUI(self):  
        self.setFixedSize(360, 220)  
        self.setWindowTitle("Máy tính tuổi thông minh")  
        self.setWindowIcon(QIcon("images\IMG_3901.png"))  
        self.setUpMainWindow()  
        self.show()  
  
    def setUpMainWindow(self):  
        header = QLabel("Máy tính tuổi thông minh!!!", self)  
        header.setFont(QFont("Arial", 20))  
        header.move(16, 10)  
  
        hehe = QLabel("Nhập vào tuổi của bạn:", self)  
        hehe.move(20, 54)  
  
        self.hehe_edit = QLineEdit(self)  
        self.hehe_edit.resize(100, 24)  
        self.hehe_edit.move(150, 50)  
  
        confirm_button = QPushButton("Xác Nhận", self)  
        confirm_button.resize(320, 34)  
        confirm_button.move(20, 140)  
        confirm_button.clicked.connect(self.clickedConfirm)  
  
    def clickedConfirm(self):  
        tmp = self.hehe_edit.text()  
        if tmp:  
            QMessageBox.information(  
                self,  
                "hehe",  
                f"Tuổi của bạn là {tmp} !!!",  
                QMessageBox.StandardButton.Ok,  
                QMessageBox.StandardButton.Ok  
            )  
            self.close()  
        else:  
            QMessageBox.warning(self,  
                                "not hehe",  
                                "Vui lòng nhập tuổi của bạn~",  
                                QMessageBox.StandardButton.Close,  
                                QMessageBox.StandardButton.Close)  
  
  
if __name__ == '__main__':  
    app = QApplication(sys.argv)  
    window = inputAge()  
    sys.exit(app.exec())