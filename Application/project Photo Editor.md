```python
import sys  
  
from PyQt6.QtCore import Qt, QSize, QRect  
from PyQt6.QtGui import QIcon, QPixmap, QAction, QTransform, QPainter  
from PyQt6.QtPrintSupport import QPrintDialog, QPrinter  
from PyQt6.QtWidgets import QMainWindow, QApplication, QLabel, QStatusBar, QToolBar, QDockWidget, QPushButton, QVBoxLayout, QWidget, QFileDialog, QMessageBox, QDialog  
  
  
class MainWindow(QMainWindow):  
    def __init__(self):  
        super().__init__()  
        self.initializeUI()  
  
    def initializeUI(self):  
        self.setWindowTitle("Photo Editor")  
        self.setWindowIcon(QIcon("images/bocchit.png"))  
        self.setFixedSize(650, 650)  
  
        self.setupMainWindow()  
        self.createToolDockWidget()  
        self.createActions()  
        self.createMenu()  
        self.createToolBar()  
        self.show()  
  
    def setupMainWindow(self):  
        """create and arrange widget in main window"""  
        self.image = QPixmap()  
  
        self.image_label = QLabel()  
        self.image_label.setAlignment(Qt.AlignmentFlag.AlignCenter)  
        self.setCentralWidget(self.image_label)  
  
        # create status bar  
        self.setStatusBar(QStatusBar())  
  
    def createToolDockWidget(self):  
        """create app dock widget"""  
        dock_widget = QDockWidget()  
        dock_widget.setWindowTitle("Edit image tools")  
        dock_widget.setAllowedAreas(  
            Qt.DockWidgetArea.LeftDockWidgetArea |  
            Qt.DockWidgetArea.RightDockWidgetArea  
        )  
  
        # button for editing  
        self.rorate90 = QPushButton("Rorate 90")  
        self.rorate90.setMinimumSize(QSize(130, 40))  
        self.rorate90.setStatusTip("Rorate 90 clockwise")  
        self.rorate90.clicked.connect(self.rorateImage90)  
  
        self.rorate180 = QPushButton("Rorate 180")  
        self.rorate180.setMinimumSize(QSize(130, 40))  
        self.rorate180.setStatusTip("Rorate 180 clockwise")  
        self.rorate180.clicked.connect(self.rorateImage180)  
  
        self.flip_horizontal = QPushButton("Flip Horizontal")  
        self.flip_horizontal.setMinimumSize(QSize(130, 40))  
        self.flip_horizontal.setStatusTip("Flip image across horizontal axis")  
        self.flip_horizontal.clicked.connect(self.flipImageHorizontal)  
  
        self.flip_vertical = QPushButton("Flip Vertical")  
        self.flip_vertical.setMinimumSize(QSize(130, 40))  
        self.flip_vertical.setStatusTip("Flip image across vertical axis")  
        self.flip_vertical.clicked.connect(self.flipImageVertical)  
  
        self.resize_half = QPushButton("Resize Half")  
        self.resize_half.setMinimumSize(QSize(130, 40))  
        self.resize_half.setStatusTip("Resize image to half the original size")  
        self.resize_half.clicked.connect(self.resizeImageHalf)  
  
        # create layout 4 dock  
        dock_vbox = QVBoxLayout()  
        dock_vbox.addWidget(self.rorate90)  
        dock_vbox.addWidget(self.rorate180)  
        dock_vbox.addStretch(1)  
        dock_vbox.addWidget(self.flip_horizontal)  
        dock_vbox.addWidget(self.flip_vertical)  
        dock_vbox.addStretch(1)  
        dock_vbox.addWidget(self.resize_half)  
        dock_vbox.addStretch(10)  
  
        # create Qwidget as container and set layout  
        tool_container = QWidget()  
        tool_container.setLayout(dock_vbox)  
        dock_widget.setWidget(tool_container)  
  
        # set initial location for dock widget  
        self.addDockWidget(Qt.DockWidgetArea.RightDockWidgetArea, dock_widget)  
  
        # handle the visibility of the dock  
        self.toggle_dock_act = dock_widget.toggleViewAction()  
  
    def createActions(self):  
        """create menu action"""  
  
        # file menu  
        self.open_act = QAction(QIcon(), "Open")  
        self.open_act.setShortcut("Ctrl+O")  
        self.open_act.setStatusTip("Open new image")  
        self.open_act.triggered.connect(self.openImage)  
  
        self.save_act = QAction(QIcon(), "Save")  
        self.save_act.setShortcut("Ctrl+S")  
        self.save_act.setStatusTip("Save image")  
        self.save_act.triggered.connect(self.saveImage)  
  
        self.print_act = QAction(QIcon(), "Print")  
        self.print_act.setShortcut("Ctrl+P")  
        self.print_act.setStatusTip("Print image")  
        self.print_act.triggered.connect(self.printImage)  
        self.print_act.setEnabled(False)  
  
        self.quit_act = QAction(QIcon(), "Quit")  
        self.quit_act.setShortcut("Ctrl+Q")  
        self.quit_act.setStatusTip("Quit program")  
        self.quit_act.triggered.connect(self.close)  
  
        # edit menu  
        self.rorate90_act = QAction("Rorate 90")  
        self.rorate90_act.setStatusTip("Rorate image 90 clockwise")  
        self.rorate90_act.triggered.connect(self.rorateImage90)  
  
        self.rorate180_act = QAction("Rorate 180")  
        self.rorate180_act.setStatusTip("Rorate image 180 clockwise")  
        self.rorate180_act.triggered.connect(self.rorateImage180)  
  
        self.flip_hor_act = QAction("Flip horizontal")  
        self.flip_hor_act.setStatusTip("Flip image horizontal axis")  
        self.flip_hor_act.triggered.connect(self.flipImageHorizontal)  
  
        self.flip_ver_act = QAction("Flip vertical")  
        self.flip_ver_act.setStatusTip("Flip image vertical axis")  
        self.flip_ver_act.triggered.connect(self.flipImageVertical)  
  
        self.resize_act = QAction("Resize half")  
        self.resize_act.setStatusTip("Resize image to half original size")  
        self.resize_act.triggered.connect(self.resizeImageHalf)  
  
        self.clear_act = QAction(QIcon(), "Clear Image")  
        self.clear_act.setShortcut("Ctrl+D")  
        self.clear_act.setStatusTip("Clear the current image")  
        self.clear_act.triggered.connect(self.clearImage)  
  
    def createMenu(self):  
        """create menu bar"""  
        self.menuBar().setNativeMenuBar(False)  
  
        # create file menu  
        file_menu = self.menuBar().addMenu("File")  
        file_menu.addAction(self.open_act)  
        file_menu.addAction(self.save_act)  
        file_menu.addSeparator()  
        file_menu.addAction(self.print_act)  
        file_menu.addSeparator()  
        file_menu.addAction(self.quit_act)  
  
        # create edit menu  
        edit_menu = self.menuBar().addMenu("Edit")  
        edit_menu.addAction(self.rorate90_act)  
        edit_menu.addAction(self.rorate180_act)  
        edit_menu.addSeparator()  
        edit_menu.addAction(self.flip_hor_act)  
        edit_menu.addAction(self.flip_ver_act)  
        edit_menu.addSeparator()  
        edit_menu.addAction(self.resize_act)  
        edit_menu.addSeparator()  
        edit_menu.addAction(self.clear_act)  
  
        # create view menu  
        view_menu = self.menuBar().addMenu("View")  
        # view_menu.addAction(self.toggle_dock_tools_act)  
  
    def createToolBar(self):  
        tool_bar = QToolBar("Photo editor toolbar")  
        tool_bar.setIconSize(QSize(24, 24))  
        self.addToolBar(tool_bar)  
  
        # add action  
        tool_bar.addAction(self.open_act)  
        tool_bar.addAction(self.save_act)  
        tool_bar.addAction(self.print_act)  
        tool_bar.addAction(self.clear_act)  
        tool_bar.addSeparator()  
        tool_bar.addAction(self.quit_act)  
  
    def openImage(self):  
        image_file, _ = QFileDialog.getOpenFileName(  
            self, "Open image", "",  
            "(*.jpeg *.jpg);;(*.png);; (*.bmp);;(*.gif)")  
        if image_file:  
            self.image = QPixmap(image_file)  
            self.image_label.setPixmap(  
                self.image.scaled(self.image_label.size(),  
                                  Qt.AspectRatioMode.KeepAspectRatio,  
                                  Qt.TransformationMode.SmoothTransformation))  
        else:  
            QMessageBox.information(self, "No image",  
                                    "No image selected.",  
                                    QMessageBox.StandardButton.Ok)  
        self.print_act.setEnabled(True)  
  
    def saveImage(self):  
        image_file, _ = QFileDialog.getSaveFileName(  
            self, "Save image", "",  
            "(*.jpeg *.jpg);;(*.png);;(*.bmp);;(*.gif)"  
        )  
        if image_file and not self.image.isNull():  
            self.image.save(image_file)  
        else:  
            QMessageBox.information(  
                self, "Not saved", "Image not saved.",  
                QMessageBox.StandardButton.Ok  
            )  
  
    def clearImage(self):  
        self.image_label.clear()  
        self.image = QPixmap()  
        self.print_act.setEnabled(False)  
  
    def rorateImage90(self):  
        if not self.image.isNull():  
            transform90 = QTransform().rotate(90)  
            pixmap = QPixmap(self.image)  
            mode = Qt.TransformationMode.SmoothTransformation  
            rorated = pixmap.transformed(transform90, mode=mode)  
  
            self.image_label.setPixmap(  
                rorated.scaled(self.image_label.size(),  
                               Qt.AspectRatioMode.KeepAspectRatio,  
                               Qt.TransformationMode.SmoothTransformation)  
            )            
            self.image = QPixmap(rorated)  
            self.image_label.repaint()  
  
    def rorateImage180(self):  
        if not self.image.isNull():  
            transform180 = QTransform().rotate(180)  
            pixmap = QPixmap(self.image)  
            mode = Qt.TransformationMode.SmoothTransformation  
            rotated = pixmap.transformed(transform180,  
                                         mode=mode)  
            self.image_label.setPixmap(  
                rotated.scaled(self.image_label.size(),  
                               Qt.AspectRatioMode.KeepAspectRatio,  
                               Qt.TransformationMode.SmoothTransformation))  
            # In order to keep from being allowed to rotate the image, set the rotated image as self.image            
            self.image = QPixmap(rotated)  
            self.image_label.repaint()  
  
    def flipImageHorizontal(self):  
        if not self.image.isNull():  
            flip = QTransform().scale(-1, 1)  
            pixmap = QPixmap(self.image)  
            flipped = pixmap.transformed(flip)  
  
            self.image_label.setPixmap(  
                flipped.scaled(  
                    self.image_label.size(),  
                    Qt.AspectRatioMode.KeepAspectRatio,  
                    Qt.TransformationMode.SmoothTransformation  
                )  
            )            
            self.image = QPixmap(flipped)  
            self.image_label.repaint()  
  
    def flipImageVertical(self):  
        if not self.image.isNull():  
            flip = QTransform().scale(1, -1)  
            pixmap = QPixmap(self.image)  
            flipped = pixmap.transformed(flip)  
  
            self.image_label.setPixmap(  
                flipped.scaled(  
                    self.image_label.size(),  
                    Qt.AspectRatioMode.KeepAspectRatio,  
                    Qt.TransformationMode.SmoothTransformation  
                )  
            )            
            self.image = QPixmap(flipped)  
            self.image_label.repaint()  
  
    def resizeImageHalf(self):  
        if not self.image.isNull():  
            resize = QTransform().scale(0.5, 0.5)  
            pixmap = QPixmap(self.image)  
            resized = pixmap.transformed(resize)  
  
            self.image_label.setPixmap(  
                resized.scaled(  
                    self.image_label.size(),  
                    Qt.AspectRatioMode.KeepAspectRatio,  
                    Qt.TransformationMode.SmoothTransformation  
                )  
            )            
            self.image = QPixmap(resized)  
            self.image_label.repaint()  
  
    def printImage(self):  
        printer = QPrinter()  
        print_dialog = QPrintDialog(printer)  
  
        if print_dialog.exec() == QDialog.DialogCode.Accepted:  
            # Qpainter -> pdf file  
            painter = QPainter()  
            painter.begin(printer)  
            # QRect to hold the painter  
            rect = QRect(painter.viewport())  
  
            # get size of image label -> set size of the viewport  
            size = QSize(self.image_label.pixmap().size())  
            size.scale(rect.size(), Qt.AspectRatioMode.KeepAspectRatio)  
            painter.setViewport(rect.x(), rect.y(), size.width(), size.height())  
            painter.setWindow(self.image_label.pixmap().rect())  
  
            # scale image label to fit the rect source  
            painter.drawPixmap(0, 0, self.image_label.pixmap())  
            painter.end()  
  
  
if __name__ == '__main__':  
    app = QApplication(sys.argv)  
    app.setAttribute(Qt.ApplicationAttribute.AA_DontShowIconsInMenus, True)  
    window = MainWindow()  
    sys.exit(app.exec())
```