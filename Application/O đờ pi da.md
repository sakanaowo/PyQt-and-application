Tạo một GUI order pizza và đồ ăn kèm(ở đây là cánh gà) có giao diện như hình dưới

![](Pasted%20image%2020240821215611.png)

![](Pasted%20image%2020240821215622.png)


## <span style="color:rgb(255, 105, 97)">Phân tích mẫu</span>

![](Pasted%20image%2020240821215719.png)

Chia ra 2 phần là :
- Giao diện đặt đồ gồm 2 tab bên trái:
	- Tab1 gồm 3 phần sử dụng QVBoxLayout:
		- Hiển thị mẫu, thông tin về bánh pizza
		- Nhóm [QRadioButton](obsidian://open?vault=Pyqt%20and%20application&file=Beginning%20PyQt%2FChapter%206%20Style%2F2.%20T%E1%BB%95%20ch%E1%BB%A9c%20widget%20v%E1%BB%9Bi%20Container%20v%C3%A0%20Tab) hiển thị các lựa chọn vỏ bánh (chỉ được chọn 1)
		- Nhóm [QGroupBox](obsidian://open?vault=Pyqt%20and%20application&file=Beginning%20PyQt%2FChapter%206%20Style%2F2.%20T%E1%BB%95%20ch%E1%BB%A9c%20widget%20v%E1%BB%9Bi%20Container%20v%C3%A0%20Tab) để hiển thị các lựa chọn topping (có thể chọn nhiều)
	- Tab2 gồm 2 phần:
		- Hiển thị mẫu
		- Nhóm [QRadioButton](obsidian://open?vault=Pyqt%20and%20application&file=Beginning%20PyQt%2FChapter%206%20Style%2F2.%20T%E1%BB%95%20ch%E1%BB%A9c%20widget%20v%E1%BB%9Bi%20Container%20v%C3%A0%20Tab) để hiển thị các lựa chọn cánh gà
	- Cả 2 tab đều có [QPushButton](obsidian://open?vault=Pyqt%20and%20application&file=Beginning%20PyQt%2FChapter%203%20Th%C3%AAm%20ch%E1%BB%A9c%20n%C4%83ng%20v%C3%A0%20ti%E1%BB%87n%20%C3%ADch%2F2.%20QButton) để thêm vào order  
- Sidebar bên phải hiển thị đồ đã order gồm 2 phần:
	- 1 QLabel "Order"
	- 1 QWidget hiển thị 3 QLabel thông tin về order gồm:
		- pizza type
		- topping
		- extra 
	- Các phần được bố trí bằng QGridLayout

## <span style="color:rgb(255, 105, 97)">Code</span> 
```python
import sys  
  
from PyQt6.QtCore import Qt  
from PyQt6.QtGui import QPixmap  
from PyQt6.QtWidgets import QWidget, QApplication, QTabBar, QTabWidget, QLayout, QLabel, QGridLayout, QVBoxLayout, \  
    QHBoxLayout, QGroupBox, QButtonGroup, QRadioButton, QPushButton  
  
style_sheet = """  
    QWidget {        background-color: #C92108;    }    QWidget#Tabs {        background-color: #FCEBCD;        border-radius: 4px;    }    QWidget#ImageBorder{        background-color: #FCF9F3;        border-width: 2px;        border-style: solid;        border-color: #FABB4C;    }    QWidget#Side{     
        background-color: #EFD096;  
        border-radius: 4px;    }    QLabel{        background-color: #EFD096;        border-width: 2px;        border-style: solid;        border-radius: 4px;        border-color: #EFD096;    }    QLabel#Header{        background-color: #EFD096;        border-width: 2px;        border-style: solid;        border-radius: 4px;        border-color: #EFD096;        padding-left: 10px;        color: #961A07;    }    QLabel#ImageInfo{        background-color: #FCF9F3;        border-radius: 4px;    }    QGroupBox{        background-color: #FCEBCD;        color: #961A07;    }    QRadioButton{        background-color: #FCF9F3;    }    QPushButton{        background-color: #C92108;        border-radius: 4px;        padding: 6px;        color: #FFFFFF;    }    QPushButton:pressed{        background-color: #C86354;        border-radius: 4px;        padding: 6px;        color: #DFD8D7;    }"""  
  
  
class MainWindow(QWidget):  
    def __init__(self):  
        super().__init__()  
        self.initializeUI()  
  
    def initializeUI(self):  
        self.setWindowTitle('Food Ordering GUI')  
        self.setMinimumSize(700, 700)  
  
        self.setupMainWindow()  
        self.show()  
  
    def setupMainWindow(self):  
        """create and arrange widget in main window"""  
        """part1"""  
        # create tab bar  
        self.tab_bar = QTabWidget()  
  
        self.pizza_tab = QWidget()  
        self.pizza_tab.setObjectName("Tabs")  
  
        self.wings_tab = QWidget()  
        self.wings_tab.setObjectName("Tabs")  
  
        self.tab_bar.addTab(self.pizza_tab, "Pizza")  
        self.tab_bar.addTab(self.wings_tab, "Wings")  
  
        # call methods that contain widget for each tab  
        self.pizzaTab()  
        self.wingsTab()  
  
        """part 2"""  
        # create sidebar in main window  
        self.side_widget = QWidget()  
        self.side_widget.setObjectName("Tabs")  
  
        order_label = QLabel("Order")  
        order_label.setObjectName("Header")  
  
        items_box = QWidget()  
        items_box.setObjectName("Side")  
  
        pizza_label = QLabel("Pizza Type: ")  
        self.display_pizza_label = QLabel("")  
  
        toppings_label = QLabel("Toppings: ")  
        self.display_toppings_label = QLabel("")  
  
        extra_label = QLabel("Extra: ")  
        self.display_wings_label = QLabel("")  
  
        # set grid layout 4 object in side widget  
        items_grid = QGridLayout()  
        items_grid.addWidget(pizza_label, 0, 0, Qt.AlignmentFlag.AlignRight)  
        items_grid.addWidget(self.display_pizza_label, 0, 1)  
        items_grid.addWidget(toppings_label, 1, 0, Qt.AlignmentFlag.AlignRight)  
        items_grid.addWidget(self.display_toppings_label, 1, 1)  
        items_grid.addWidget(extra_label, 2, 0, Qt.AlignmentFlag.AlignRight)  
        items_grid.addWidget(self.display_wings_label, 2, 1)  
        items_box.setLayout(items_grid)  
  
        """part3"""  
        # set main layout for side widget  
        side_vbox = QVBoxLayout()  
        side_vbox.addWidget(order_label)  
        side_vbox.addWidget(items_box)  
        side_vbox.addStretch()  
        self.side_widget.setLayout(side_vbox)  
  
        # add widget to main window and set layout  
        main_hbox = QHBoxLayout()  
        main_hbox.addWidget(self.tab_bar, 1)  
        main_hbox.addWidget(self.side_widget)  
        self.setLayout(main_hbox)  
  
    def pizzaTab(self):  
        """part1"""  
        """create pizza tab and allow to sellect pizza type"""  
        # setup widget and layout to display information to user  
        tab_pizza_label = QLabel("Build ur piza")  
        tab_pizza_label.setObjectName("Header")  
  
        description_box = QWidget()  
        description_box.setObjectName("ImageBorder")  
  
        pizza_img_path = "images/pizza.png"  
        pizza_img = self.loadImage(pizza_img_path)  
  
        pizza_desc = QLabel()  
        pizza_desc.setObjectName("ImageInfor")  
        pizza_desc.setText(  
            """<p> Build a custom pizza. 
            Start with ur fav crust and add any topping.</p>"""  
        )  
        pizza_desc.setWordWrap(True)  
        pizza_desc.setContentsMargins(10, 10, 10, 10)  
  
        pizza_hbox = QHBoxLayout()  
        pizza_hbox.addWidget(pizza_img)  
        pizza_hbox.addWidget(pizza_desc, 1)  
  
        description_box.setLayout(pizza_hbox)  
  
        # create group box contain crust choices  
        crust_gbox = QGroupBox()  
        crust_gbox.setTitle("Choose ur crust")  
  
        # groupbox to group the widget together while button grp used to get in4 about which radio button is checked  
        self.crust_group = QButtonGroup()  
        gb_vbox = QVBoxLayout()  
        crust_list = ["Hand-Tossed", "Flat", "Stuffed"]  
  
        # create radio button 4 different crust and add to layout  
        for cr in crust_list:  
            crust_rb = QRadioButton(cr)  
            gb_vbox.addWidget(crust_rb)  
            self.crust_group.addButton(crust_rb)  
        crust_gbox.setLayout(gb_vbox)  
  
        """part2"""  
        # create grpbox contain topping choices  
        toppings_gbox = QGroupBox()  
        toppings_gbox.setTitle("Choose ur topping")  
  
        # setup button grp 4 topping  
        self.toppings_group = QButtonGroup()  
        gb_vbox = QVBoxLayout()  
  
        toppings_list = ["Pepperoni", "Sausage", "Bacon",  
                         "Canadian Bacon", "Beef", "Pineapple",  
                         "Olive", "Tomato", "Green Pepper",  
                         "Mushroom", "Onion", "Spinach",  
                         "Cheese"]  
        # create radio button 4 different topping  
        for top in toppings_list:  
            topping_rb = QRadioButton(top)  
            gb_vbox.addWidget(topping_rb)  
            self.toppings_group.addButton(topping_rb)  
        self.toppings_group.setExclusive(False)  
        toppings_gbox.setLayout(gb_vbox)  
  
        # create button to add in4 to side widget  
        add_to_order_button1 = QPushButton("Add Order")  
  
        add_to_order_button1.clicked.connect(self.displayPizzaInOrder)  
  
        # create layout for pizaa tab  
        page1_vbox = QVBoxLayout()  
        page1_vbox.addWidget(tab_pizza_label)  
        page1_vbox.addWidget(description_box)  
        page1_vbox.addWidget(crust_gbox)  
        page1_vbox.addWidget(toppings_gbox)  
        page1_vbox.addStretch()  
        page1_vbox.addWidget(add_to_order_button1,
					        alignment=Qt.AlignmentFlag.AlignRight)  
  
        self.pizza_tab.setLayout(page1_vbox)  
  
    def wingsTab(self):  
        """create wings tab and allow to sellect wings type"""  
        """part 1"""  
        tab_wings_label = QLabel("Build ur wings")  
        tab_wings_label.setObjectName("Header")  
  
        description_box = QWidget()  
        description_box.setObjectName("ImageBorder")  
  
        wings_img_path = "images/wings.png"  
        wings_img = self.loadImage(wings_img_path)  
  
        wings_desc = QLabel()  
        wings_desc.setObjectName("ImageInfor")  
        wings_desc.setText(  
            """<p>6 pieces of rich-tasting, 
            white meat chicken that will have you coming back for more.</p>"""
        )  
        wings_desc.setWordWrap(True)  
        wings_desc.setContentsMargins(10, 10, 10, 10)  
  
        wings_hbox = QHBoxLayout()  
        wings_hbox.addWidget(wings_img)  
        wings_hbox.addWidget(wings_desc, 1)  
  
        description_box.setLayout(wings_hbox)  
  
        """part 2"""  
        wings_gbox = QGroupBox()  
        wings_gbox.setTitle("Choose ur wings")  
  
        self.wings_group = QButtonGroup()  
        gb_vbox = QVBoxLayout()  
        flavor_list = ["Buffalo", "Sweet-Sour", "Teriyaki", "Barbecue"]  
  
        # create radio button for different flavor  
        for flavor in flavor_list:  
            flavor_rb = QRadioButton(flavor)  
            gb_vbox.addWidget(flavor_rb)  
            self.wings_group.addButton(flavor_rb)  
        wings_gbox.setLayout(gb_vbox)  
  
        # creaete button to add in4 to side widget when clicked  
        add_to_order_button2 = QPushButton("Add Order")  
        add_to_order_button2.clicked.connect(self.displayWingsInOrder)  
  
        # create layout for wings tab  
        page2_vbox = QVBoxLayout()  
        page2_vbox.addWidget(tab_wings_label)  
        page2_vbox.addWidget(description_box)  
        page2_vbox.addWidget(wings_gbox)  
        page2_vbox.addWidget(add_to_order_button2,
					        alignment=Qt.AlignmentFlag.AlignRight)  
        page2_vbox.addStretch()  
  
        self.wings_tab.setLayout(page2_vbox)  
  
    def loadImage(self, img_path):  
        aspect = Qt.AspectRatioMode.KeepAspectRatioByExpanding  
        transform = Qt.TransformationMode.SmoothTransformation  
        try:  
            with open(img_path):  
                image = QLabel(self)  
                image.setObjectName("ImageInfor")  
                pixmap = QPixmap(img_path)  
                image.setPixmap(pixmap.scaled(image.size(), aspect, transform))  
                return image  
        except FileNotFoundError as error:  
            print(f"Image {img_path} not found. Error: {error}")  
  
    def displayPizzaInOrder(self):  
        if self.crust_group.checkedButton():  
            text = self.crust_group.checkedButton().text()  
            self.display_pizza_label.setText(text)  
  
            toppings = self.collectToppingInList()  
            toppings_str = '\n'.join(toppings)  
            self.display_toppings_label.setText(toppings_str)  
            self.update()  
  
    def displayWingsInOrder(self):  
        if self.wings_group.checkedButton():  
            text = self.wings_group.checkedButton().text() + " Wings"  
            self.display_wings_label.setText(text)  
            self.update()  
  
    def collectToppingInList(self):  
        toppings_list = [button.text() for i, button in
				        enumerate(self.toppings_group.buttons()) 
				        if button.isChecked()]  
        return toppings_list  
  
  
if __name__ == '__main__':  
    app = QApplication(sys.argv)  
    app.setStyleSheet(style_sheet)  
    window = MainWindow()  
    sys.exit(app.exec())
```