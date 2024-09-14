~ excel ?

![](Pasted%20image%2020240914232941.png)

## <span style="color:rgb(255, 105, 97)">Cài đặt</span> 
```python
import sys  
  
from PyQt6.QtGui import QAction  
from PyQt6.QtWidgets import QMainWindow, QApplication, QTableWidget, QMenu, QInputDialog, QTableWidgetItem  
  
  
class MainWindow(QMainWindow):  
    def __init__(self):  
        super().__init__()  
        self.initUI()  
  
    def initUI(self):  
        self.setMinimumSize(1000, 500)  
        self.setWindowTitle("SpreadSheet")  
  
        self.item_text = None  
  
        self.setup()  
        self.createActions()  
        self.createMenu()  
        self.show()  
  
    def setup(self):  
        self.table_widget = QTableWidget()  
        # Khởi tạo table 10x10
        self.table_widget.setRowCount(10)  
        self.table_widget.setColumnCount(10)  
  
        # cell bắt đầu tại góc trên trái
        self.table_widget.setCurrentCell(0, 0)  
  
        # cho phép đổi header khi doubleclick
        h_header = self.table_widget.horizontalHeader()  
        h_header.sectionDoubleClicked.connect(self.changeHeader)  
  
        self.setCentralWidget(self.table_widget)  
  
    # tạo action trong menu bar và menu ngữ cảnh (click chuột phải)
    def createActions(self):  
        self.quit_act = QAction("Quit", self)  
        self.quit_act.setShortcut("Ctrl+Q")  
        self.quit_act.triggered.connect(self.close)  
  
        self.add_row_above = QAction("Add Row Above", self)  
        self.add_row_above.triggered.connect(self.addRowAbove)  
  
        self.add_row_below = QAction("Add Row Below", self)  
        self.add_row_below.triggered.connect(self.addRowBelow)  
  
        self.add_col_before = QAction("Add Column Before", self)  
        self.add_col_before.triggered.connect(self.addColBefore)  
  
        self.add_col_after = QAction("Add Column After", self)  
        self.add_col_after.triggered.connect(self.addColAfter)  
  
        self.del_row = QAction("Delete Row", self)  
        self.del_row.triggered.connect(self.deleteRow)  
  
        self.del_col = QAction("Delete Column", self)  
        self.del_col.triggered.connect(self.deleteCol)  
  
        self.clearALL = QAction("Clear All", self)  
        self.clearALL.triggered.connect(self.clearTable)  
  
    # menuBar chỉ chứa "File" và "Table"
    def createMenu(self):  
        self.menuBar().setNativeMenuBar(False)  
  
        file_menu = self.menuBar().addMenu("File")  
        file_menu.addAction(self.quit_act)  
  
        table_menu = self.menuBar().addMenu("Table")  
        table_menu.addAction(self.add_row_above)  
        table_menu.addAction(self.add_row_below)  
        table_menu.addSeparator()  
        table_menu.addAction(self.add_col_before)  
        table_menu.addAction(self.add_col_after)  
        table_menu.addSeparator()  
        table_menu.addAction(self.del_row)  
        table_menu.addAction(self.del_col)  
        table_menu.addSeparator()  
        table_menu.addAction(self.clearALL)  
  
    def changeHeader(self):  
        col = self.table_widget.currentRow()  
  
        text, check = QInputDialog.getText(self, "Enter Header", "Header text:")  
        if check and text != "":  
            self.table_widget.setHorizontalHeaderItem(col, QTableWidgetItem(text))  
  
    def addRowAbove(self):  
        current_row = self.table_widget.currentRow()  
        self.table_widget.insertRow(current_row)  
  
    def addRowBelow(self):  
        current_row = self.table_widget.currentRow()  
        self.table_widget.insertRow(current_row + 1)  
  
    def addColBefore(self):  
        current_col = self.table_widget.currentColumn()  
        self.table_widget.insertColumn(current_col)  
  
    def addColAfter(self):  
        current_col = self.table_widget.currentColumn()  
        self.table_widget.insertColumn(current_col + 1)  
  
    def deleteRow(self):  
        current_row = self.table_widget.currentRow()  
        self.table_widget.removeRow(current_row)  
  
    def deleteCol(self):  
        current_col = self.table_widget.currentColumn()  
        self.table_widget.removeColumn(current_col)  
  
    def clearTable(self):  
        self.table_widget.clear()  
  
    def contextMenuEvent(self, event):  
        contextMenu = QMenu(self)  
        contextMenu.addAction(self.add_row_above)  
        contextMenu.addAction(self.add_row_below)  
        contextMenu.addSeparator()  
        contextMenu.addAction(self.add_col_before)  
        contextMenu.addAction(self.add_col_after)  
        contextMenu.addSeparator()  
        contextMenu.addAction(self.del_row)  
        contextMenu.addAction(self.del_col)  
        contextMenu.addSeparator()  
  
        copy_act = contextMenu.addAction("Copy")  
        paste_act = contextMenu.addAction("Paste")  
        contextMenu.addSeparator()  
        contextMenu.addAction(self.clearALL)  
  
        action = contextMenu.exec(self.mapToGlobal(event.pos()))  # popup context menu when right click in GUI  
        if action == copy_act:  
            self.copyItem()  
        if action == paste_act:  
            self.pasteItem()  
  
    def copyItem(self):  
        if self.table_widget.currentItem() != None:  
            text = self.table_widget.currentItem().text()  
            self.item_text = text  
  
    def pasteItem(self):  
        if self.item_text != None:  
            row = self.table_widget.currentRow()  
            col = self.table_widget.currentColumn()  
            self.table_widget.setItem(row, col, QTableWidgetItem(self.item_text))  
  
  
if __name__ == "__main__":  
    app = QApplication(sys.argv)  
    window = MainWindow()  
    sys.exit(app.exec())
```