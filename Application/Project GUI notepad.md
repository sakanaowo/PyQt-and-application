```python
from PyQt6.QtCore import Qt  
from PyQt6.QtGui import QAction, QIcon, QTextCursor, QColor  
from PyQt6.QtWidgets import QMainWindow, QApplication, QTextEdit, QMessageBox, QFileDialog, QInputDialog, QFontDialog, \  
    QColorDialog  
import sys  
  
  
class MainWindow(QMainWindow):  
    def __init__(self):  
        super().__init__()  
        self.initializeUI()  
  
    def initializeUI(self):  
        self.setMinimumSize(400, 500)  
        self.setWindowTitle("Notepad GUI")  
  
        self.setupMainWindow()  
        self.createActions()  
        self.createMenu()  
        self.show()  
  
    def setupMainWindow(self):  
        self.text_edit = QTextEdit()  
        self.text_edit.textChanged.connect(self.removeHighlights)  
        self.setCentralWidget(self.text_edit)  
  
    def createActions(self):  
        """ create app menu action"""  
  
        # action 4 file menu  
        self.new_act = QAction(QIcon("images/cirno.png"), "New")  
        self.new_act.triggered.connect(self.clearText)  
  
        self.open_act = QAction(QIcon("images/migu.png"), "Open")  
        self.open_act.setShortcut("Ctrl+O")  
        self.open_act.triggered.connect(self.openFile)  
  
        self.save_act = QAction(QIcon("images/yuyuko.png"), "Save")  
        self.save_act.setShortcut("Ctrl+S")  
        self.save_act.triggered.connect(self.saveToFile)  
  
        self.quit_act = QAction(QIcon("images/reimu.png"), "Quit")  
        self.quit_act.setShortcut("Ctrl+Q")  
        self.quit_act.triggered.connect(self.close)  
  
        self.undo_act = QAction(QIcon("images/bocchit.png"), "Undo")  
        self.undo_act.setShortcut("Ctrl+Z")  
        self.undo_act.triggered.connect(self.text_edit.undo)  
  
        self.redo_act = QAction(QIcon("images/bocchir.png"), "Redo")  
        self.redo_act.setShortcut("Ctrl+Shift+Z")  
        self.redo_act.triggered.connect(self.text_edit.redo)  
  
        self.cut_act = QAction(QIcon("images/cut.png"), "Cut")  
        self.cut_act.setShortcut("Ctrl+X")  
        self.cut_act.triggered.connect(self.text_edit.cut)  
  
        self.copy_act = QAction(QIcon("images/copy.png"), "Copy")  
        self.copy_act.setShortcut("Ctrl+C")  
        self.copy_act.triggered.connect(self.text_edit.copy)  
  
        self.paste_act = QAction(QIcon("images/paste.png"), "Paste")  
        self.paste_act.setShortcut("Ctrl+V")  
        self.paste_act.triggered.connect(self.text_edit.paste)  
  
        self.find_act = QAction(QIcon("images/find.png"), "Find")  
        self.find_act.setShortcut("Ctrl+F")  
        self.find_act.triggered.connect(self.searchText)  
  
        # create action 4 tool menu  
        self.font_act = QAction(QIcon("images/font.png"), "Font")  
        self.font_act.setShortcut("Ctrl+T")  
        self.font_act.triggered.connect(self.chooseFont)  
  
        self.color_act = QAction(QIcon("images/color.png"), "Color")  
        self.color_act.setShortcut("Ctrl+Shift+C")  
        self.color_act.triggered.connect(self.chooseFontColor)  
  
        self.highlight_act = QAction(QIcon("images/highlight.png"), "Highlight")  
        self.highlight_act.setShortcut("Ctrl+Shift+H")  
        self.highlight_act.triggered.connect(self.chooseFontBackgroundColor)  
  
        # Create actions for Help menu  
        self.about_act = QAction("About")  
        self.about_act.triggered.connect(self.aboutDialog)  
  
    def createMenu(self):  
        """create app's menu bar"""  
        self.menuBar().setNativeMenuBar(False)  
  
        # create File menu  
        file_menu = self.menuBar().addMenu("File")  
        file_menu.addAction(self.new_act)  
        file_menu.addSeparator()  
        file_menu.addAction(self.open_act)  
        file_menu.addAction(self.save_act)  
        file_menu.addSeparator()  
        file_menu.addAction(self.quit_act)  
  
        # create edit menu  
        edit_menu = self.menuBar().addMenu("Edit")  
        edit_menu.addAction(self.undo_act)  
        edit_menu.addAction(self.redo_act)  
        edit_menu.addSeparator()  
        edit_menu.addAction(self.cut_act)  
        edit_menu.addAction(self.copy_act)  
        edit_menu.addAction(self.paste_act)  
        edit_menu.addSeparator()  
        edit_menu.addAction(self.find_act)  
  
        # create tool menu  
        tool_menu = self.menuBar().addMenu("Tools")  
        tool_menu.addAction(self.font_act)  
        tool_menu.addAction(self.color_act)  
        tool_menu.addAction(self.highlight_act)  
  
        # create help menu  
        help_menu = self.menuBar().addMenu("Help")  
        help_menu.addAction(self.about_act)  
  
    def clearText(self):  
        answer = QMessageBox.question(self, "Clear text?",  
                                      "Muốn buông bỏ hết sao~ ",  
                                      QMessageBox.StandardButton.No | \  
                                      QMessageBox.StandardButton.Yes,  
                                      QMessageBox.StandardButton.Yes)  
        if answer == QMessageBox.StandardButton.Yes: self.text_edit.clear()  
  
    def openFile(self):  
        file_name, _ = QFileDialog.getOpenFileName(  
            self, "Open File", "",  
            "HTML File (*.html);;Text Files (*.txt)"  
        )  
  
        if file_name:  
            with open(file_name, "r") as f:  
                notepad_text = f.read()  
            self.text_edit.setText(notepad_text)  
  
    def saveToFile(self):  
        file_name, _ = QFileDialog.getSaveFileName(  
            self, "Save File", "",  
            "HTML Files (*.html);;Text Files (*.txt)"  
        )  
  
        if file_name.endswith(".txt"):  
            notepad_text = self.text_edit.toPlainText()  
            with open(file_name, "w") as f:  
                f.write(notepad_text)  
        elif file_name.endswith(".html"):  
            notepad_richtext = self.text_edit.toHtml()  
            with open(file_name, "w") as f:  
                f.write(notepad_richtext)  
        else:  
            QMessageBox.information(  
                self, "Not Saved", "Text not saved.",  
                QMessageBox.StandardButton.Ok)  
  
    def searchText(self):  
        find_text, ok = QInputDialog.getText(  
            self, "Search", "Find:"  
        )  
        if ok:  
            extra_selection = []  
  
            self.text_edit.moveCursor(  
                QTextCursor.MoveOperation.Start  
            )  
            color = QColor(Qt.GlobalColor.cyan)  
  
            while (self.text_edit.find(find_text)):  
                # Use ExtraSelection() to mark the text you are searching for as gray  
                selection = QTextEdit.ExtraSelection()  
                selection.format.setBackground(color)  
  
                # set the cursor of the selection  
                selection.cursor = self.text_edit.textCursor()  
                extra_selection.append(selection)  
  
            # highlight all selection in Qtext widget  
            self.text_edit.setExtraSelections(extra_selection)  
  
    def removeHighlights(self):  
        self.text_edit.setExtraSelections([])  
  
    def chooseFont(self):  
        current = self.text_edit.currentFont()  
        opt = QFontDialog.FontDialogOption.DontUseNativeDialog  
        font, ok = QFontDialog.getFont(current, self, options=opt)  
        if ok:  
            self.text_edit.setCurrentFont(font)  
  
    def chooseFontColor(self):  
        color = QColorDialog.getColor()  
        if color.isValid():  
            self.text_edit.setTextColor(color)  
  
    def chooseFontBackgroundColor(self):  
        color = QColorDialog.getColor()  
        if color.isValid():  
            self.text_edit.setTextBackgroundColor(color)  
  
    def aboutDialog(self):  
        QMessageBox.about(self, "About notepad"  
                                """<p> beginner practical guide</p>                                <p> project notepad GUI</p>""")  
  
  
if __name__ == '__main__':  
    app = QApplication(sys.argv)  
    window = MainWindow()  
    sys.exit(app.exec())
```


---
[project](obsidian://open?vault=Pyqt%20and%20application&file=Application%2Fproject%20Photo%20Editor): [project Photo Editor](project%20Photo%20Editor.md)
