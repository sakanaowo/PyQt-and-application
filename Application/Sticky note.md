project tạo sticky note GUI cho phép mở nhiều cửa sổ để note độc lập, có thể thay đổi màu sắc và paste nội dung từ clipboard

![](https://github.com/sakanaowo/PyQt-and-application/blob/main/Image/Pasted%20image%2020240907152506.png?raw=true)

sticky note là một dạng SDI - single document interface (giao diện tài liệu đơn)

Cấu trúc:
- chứa 1 QTextEdit ở trung tâm 
- Một menubar để tạo mới note, clear text, thoát, đổi màu nền, dán nội dung

## <span style="color:rgb(255, 105, 97)">Code:</span> 

```python
import sys  
  
from PyQt6.QtGui import QAction  
from PyQt6.QtWidgets import QMainWindow, QApplication, QTextEdit  
  
  
class StickyNote(QMainWindow):  
    # Class variables shared by all instances  
    note_id = 1  
    notes = []  
  
    def __init__(self, note_ref=str()):  
        super().__init__()  
        self.initializeUI()  
  
    def initializeUI(self):  
        """Set up the application's GUI."""  
        self.setMinimumSize(250, 250)  
        self.setWindowTitle("Sticky Note")  
        self.setUpMainWindow()  
  
        self.createActions()  
        self.createMenu()  
        self.createClipboard()  
  
        self.show()  
  
    def setUpMainWindow(self):  
        self.notes.append(self)  
        self.central_tedit = QTextEdit()  
        self.setCentralWidget(self.central_tedit)  
  
    def createActions(self):  
        self.new_note_act = QAction("New Note", self)  
        self.new_note_act.setShortcut("Ctrl+N")  
        self.new_note_act.triggered.connect(self.newNote)  
  
        self.close_act = QAction("Clear", self)  
        self.close_act.setShortcut("Ctrl+W")  
        self.close_act.triggered.connect(self.clearNote)  
  
        self.quit_act = QAction("Quit", self)  
        self.quit_act.setShortcut("Ctrl+Q")  
        self.quit_act.triggered.connect(self.close)  
  
        self.yellow_act = QAction("Yellow", self)  
        self.yellow_act.triggered.connect(  
            lambda: self.changeBackground(self.yellow_act.text()))  
  
        self.blue_act = QAction("Blue", self)  
        self.blue_act.triggered.connect(  
            lambda: self.changeBackground(self.blue_act.text()))  
  
        self.green_act = QAction("Green", self)  
        self.green_act.triggered.connect(  
            lambda: self.changeBackground(self.green_act.text()))  
  
        self.paste_act = QAction("Paste", self)  
        self.paste_act.setShortcut("Ctrl+V")  
        self.paste_act.triggered.connect(self.pasteToClipboard)  
  
    def createMenu(self):  
        self.menuBar().setNativeMenuBar(False)  
  
        file_menu = self.menuBar().addMenu("File")  
        file_menu.addAction(self.new_note_act)  
        file_menu.addAction(self.close_act)  
        file_menu.addSeparator()  
        file_menu.addAction(self.quit_act)  
  
        color_menu = self.menuBar().addMenu("Color")  
        color_menu.addAction(self.yellow_act)  
        color_menu.addAction(self.blue_act)  
        color_menu.addAction(self.green_act)  
  
        paste_menu = self.menuBar().addMenu("Paste")  
        paste_menu.addAction(self.paste_act)  
  
    def createClipboard(self):  
        self.clipboard = QApplication.clipboard()  
        self.clipboard.dataChanged.connect(self.copyToClipboard)  
        self.mime_data = self.clipboard.mimeData()  
  
    def copyToClipboard(self):  
        self.mime_data = self.clipboard.mimeData()  
  
    def newNote(self):  
        StickyNote().show()  
        self.note_id += 1  
  
    def clearNote(self):  
        self.central_tedit.clear()  
  
    def changeBackground(self, text):  
        if text == 'Yellow':  
            self.central_tedit.setStyleSheet("background-color: yellow")  
        elif text == 'Blue':  
            self.central_tedit.setStyleSheet("background-color: blue")  
        elif text == 'Green':  
            self.central_tedit.setStyleSheet("background-color: green")  
  
    def pasteToClipboard(self):  
        if self.mime_data.hasText(): self.central_tedit.paste()  
  
  
if __name__ == '__main__':  
    app = QApplication(sys.argv)  
    window = StickyNote()  
    sys.exit(app.exec())
```