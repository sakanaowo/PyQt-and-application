
Trong dự án của chương này, chúng ta sẽ xem xét việc tạo một **widget** tùy chỉnh và có chức năng trong **PyQt**. Mặc dù **PyQt** cung cấp nhiều **widget** để xây dựng giao diện đồ họa người dùng (**GUI**), đôi khi bạn có thể thấy mình cần phải thiết kế và xây dựng **widget** riêng. Một trong những lợi ích của việc tạo **widget** tùy chỉnh là bạn có thể tạo ra một **widget** chung có thể được sử dụng bởi nhiều ứng dụng khác nhau hoặc tạo một **widget** dành riêng cho ứng dụng, giúp bạn giải quyết một vấn đề cụ thể.

Có khá nhiều kỹ thuật mà bạn có thể sử dụng để tạo **widget** của riêng mình, hầu hết chúng ta đã thấy trong các ví dụ trước đây:
- Sửa đổi các thuộc tính của các **widget** trong **PyQt** bằng cách sử dụng các phương thức tích hợp sẵn, chẳng hạn như **setAlignment()**, **setTextColor()**, và **setRange()**.
- Tạo các **style sheet** để thay đổi hành vi và giao diện hiện tại của một **widget**.
- Tạo các **subclass** cho **widget** và cài đặt lại các trình xử lý sự kiện, hoặc thêm thuộc tính động vào các lớp **QWidget**.
- Tạo các **widget** tổng hợp được cấu thành từ hai hoặc nhiều loại **widget** và sắp xếp chúng lại với nhau bằng một **layout**.
- Thiết kế một **widget** hoàn toàn mới, kế thừa từ **QWidget** và có các thuộc tính và giao diện riêng biệt.

![](Pasted%20image%2020240928151610.png)
## <span style="color:rgb(255, 105, 97)">PyQt image handling</span>

Trong các ví dụ trước, chúng ta đã làm việc với **QPixmap** để xử lý dữ liệu hình ảnh. **Qt** thực tế cung cấp bốn lớp khác nhau để làm việc với hình ảnh, mỗi lớp đều có mục đích riêng biệt.

**QPixmap** là lựa chọn hàng đầu để hiển thị hình ảnh trên màn hình. Pixmap có thể được hiển thị trên nhiều **widget** có thể hiển thị biểu tượng, bao gồm **QLabel** và **QPushButton**. **QImage** được tối ưu hóa cho việc đọc, ghi và xử lý hình ảnh, rất hữu ích nếu bạn cần truy cập trực tiếp và chỉnh sửa dữ liệu pixel của hình ảnh. **QImage** cũng có thể hoạt động như một thiết bị vẽ (**paint device**). Một thiết bị vẽ (được tạo bởi lớp **QPaintDevice**) là một bề mặt hai chiều có thể được vẽ lên bằng **QPainter**. Cũng cần lưu ý rằng **QImage** kế thừa từ **QPaintDevice**.

Chuyển đổi giữa **QImage** và **QPixmap** cũng có thể thực hiện được. Một cách sử dụng khả thi cho việc kết hợp hai lớp này là tải tệp hình ảnh bằng **QImage**, xử lý dữ liệu hình ảnh, sau đó chuyển đổi nó thành **pixmap** trước khi hiển thị trên màn hình. **Widget** thanh trượt RGB cung cấp một ví dụ về việc chuyển đổi giữa hai lớp này.

**QBitmap** là một lớp con của **QPixmap** và cung cấp **pixmap** đơn sắc (độ sâu 1-bit). **QPicture** là một thiết bị vẽ có khả năng phát lại các lệnh của **QPainter**, nghĩa là bạn có thể tạo một bức tranh từ bất kỳ định dạng hình ảnh nào mà bạn đang vẽ. Các hình ảnh được tạo bởi **QPicture** là độc lập với độ phân giải, hiển thị giống nhau dù bạn sử dụng định dạng hình ảnh nào, chẳng hạn như **png**, **svg**, hay **pdf**.

Thanh trượt RGB sử dụng hai loại **widget** để chọn giá trị RGB: **QSpinBox**, đã được giới thiệu trong Chương 4, và một **widget** mới.

## <span style="color:rgb(255, 105, 97)">QSlider widget</span>

Lớp **QSlider** cung cấp cho nhà phát triển một công cụ để chọn các giá trị nguyên trong một phạm vi giới hạn. **Sliders** cung cấp cho người dùng một cách tiện lợi để nhanh chóng chọn giá trị hoặc thay đổi các cài đặt chỉ bằng cách kéo một tay cầm đơn giản. Mặc định, các **slider** được sắp xếp theo chiều dọc (được chỉ định bởi **Qt.Orientation.Vertical**), nhưng có thể thay đổi bằng cách truyền cờ **Qt.Orientation.Horizontal** vào hàm khởi tạo.

Đoạn mã sau đây minh họa cách tạo một instance của **QSlider**, thiết lập giá trị phạm vi tối đa của slider, và kết nối với tín hiệu **valueChanged** sẽ được phát ra mỗi khi giá trị của **slider** thay đổi.

```python
slider = QSlider(Qt.Horizontal, self) # Giá trị mặc định là từ 0 đến 99
slider.setMaximum(200) 
slider.valueChanged.connect(self.printSliderValue) 

def printSliderValue(self, value): 
	print(value)
```

Ở đây, giá trị phạm vi tối đa của **slider** là 200, và giá trị của nó sẽ được in ra **shell** mỗi khi vị trí của **slider** thay đổi.

## <span style="color:rgb(255, 105, 97)">Cài đặt project</span> 

```python
import sys  
from PyQt6.QtWidgets import (QApplication, QWidget, QLabel, QSlider, QSpinBox, QHBoxLayout, QVBoxLayout, QGridLayout)  
from PyQt6.QtGui import QImage, QPixmap, QColor, qRgb, QFont  
from PyQt6.QtCore import Qt  
  
style_sheet = """  
    QSlider:groove:horizontal {        
	    border: 1px solid #000000;        
	    background: white;        
	    height: 10px;        
	    border-radius: 4px;    
	}    
	QSlider#Red:sub-page:horizontal {        
		background: qlineargradient(x1:1,y1:0,x2:0,y2:1,stop: 0 #FF4242,stop: 1#1C1C1C);        
		border: 1px solid #4C4B4B;        
		height: 10px;        
		border-radius: 4px;    
	}    
	QSlider::add-page:horizontal {        
		background: #FFFFFF;        
		border: 1px solid #4C4B4B;        
		height: 10px;        
		border-radius: 4px;    
	}    
	QSlider::handle:horizontal {        
		background: qlineargradient(x1:0, y1:0, x2:1, y2:1,stop: 0 #EEEEEE, stop: 1 #CCCCCC);        
		border: 1px solid #4C4B4B;        
		width: 13px;        
		margin-top: -3px;        
		margin-bottom: -3px;        
		border-radius: 4px;    
	}    
	QSlider::handle:horizontal:hover {        
		background: qlineargradient(x1:0, y1:0, x2:1, y2:1, stop: 0 #FFFFFF, stop: 1 #DDDDDD);        
		border: 1px solid #393838;        
		border-radius: 4px;    
	}    
	QSlider#Green:sub-page:horizontal {        
		background: qlineargradient(x1:1, y1:0, x2:0, y2:1,stop: 0 #FF4242, stop: 1 #1C1C1C);        
		background: qlineargradient(x1:0, y1:1, x2:1, y2:1,stop: 0 #1C1C1C, stop: 1 #00FF00);        
		border: 1px solid #4C4B4B;        
		height: 10px;        
		border-radius: 4px;    
	}  
    QSlider#Blue:sub-page:horizontal{        
	    background: qlineargradient(x1:1, y1:0, x2:0, y2:1,stop: 0 #FF4242, stop: 1 #1C1C1C);        
	    background: qlineargradient(x1:0, y1:1, x2:1, y2:1,stop: 0 #1C1C1C, stop: 1 #0000FF);        
	    border: 1px solid #4C4B4B;        
	    height: 10px;        
	    border-radius: 4px;    
	}
"""  
  
  
class RGBSlider(QWidget):  
    def __init__(self, _image=None, *args, **kwargs):  
        super().__init__(*args, **kwargs)  
        self._image = _image  
        self.initUI()  
  
    def initUI(self):  
        self.setMinimumSize(225, 330)  
        self.setWindowTitle("RGB slider")  
  
        self.current_val = QColor()  
  
        self.setup()  
        self.show()  
  
    def setup(self):  
        self.color_display = QImage(100, 100, QImage.Format.Format_RGBX64)  
        self.color_display.fill(Qt.GlobalColor.black)  
  
        self.cd_label = QLabel()  
        self.cd_label.setScaledContents(True)  
  
        red_label = QLabel("Red")  
        red_label.setFont(QFont('Helvetica', 14))  
  
        self.red_slider = QSlider(Qt.Orientation.Horizontal)  
        self.red_slider.setObjectName("Red")  
        self.red_slider.setMaximum(225)  
  
        self.red_spinbox = QSpinBox()  
        self.red_spinbox.setMaximum(255)  
  
        green_label = QLabel("Green")  
        green_label.setFont(QFont('Helvetica', 14))  
        self.green_slider = QSlider(Qt.Orientation.Horizontal)  
        self.green_slider.setObjectName("Green")  
        self.green_slider.setMaximum(225)  
  
        self.green_spinbox = QSpinBox()  
        self.green_spinbox.setMaximum(255)  
  
        blue_label = QLabel("Blue")  
        blue_label.setFont(QFont('Helvetica', 14))  
        self.blue_slider = QSlider(Qt.Orientation.Horizontal)  
        self.blue_slider.setObjectName("Blue")  
        self.blue_slider.setMaximum(225)  
  
        self.blue_spinbox = QSpinBox()  
        self.blue_spinbox.setMaximum(255)  
  
        hex_label = QLabel("Hex color")  
        self.hex_value_label = QLabel()  
        hex_hbox = QHBoxLayout()  
        hex_hbox.addWidget(hex_label, Qt.AlignmentFlag.AlignRight)  
        hex_hbox.addWidget(self.hex_value_label, Qt.AlignmentFlag.AlignLeft)  
        hex_container = QWidget()  
        hex_container.setLayout(hex_hbox)  
  
        grid = QGridLayout()  
        grid.addWidget(red_label, 0, 0, Qt.AlignmentFlag.AlignLeft)  
        grid.addWidget(self.red_slider, 1, 0)  
        grid.addWidget(self.red_spinbox, 1, 1)  
  
        grid.addWidget(green_label, 2, 0, Qt.AlignmentFlag.AlignLeft)  
        grid.addWidget(self.green_slider, 3, 0)  
        grid.addWidget(self.green_spinbox, 3, 1)  
  
        grid.addWidget(blue_label, 4, 0, Qt.AlignmentFlag.AlignLeft)  
        grid.addWidget(self.blue_slider, 5, 0)  
        grid.addWidget(self.blue_spinbox, 5, 1)  
  
        grid.addWidget(hex_container, 6, 0, 1, 0)  
  
        self.red_slider.valueChanged.connect(self.updateRedSpinbox)  
        self.red_spinbox.valueChanged.connect(self.updateRedSlider)  
  
        self.green_slider.valueChanged.connect(self.updateGreenSpinBox)  
        self.green_spinbox.valueChanged.connect(self.updateGreenSlider)  
  
        self.blue_slider.valueChanged.connect(self.updateBlueSpinBox)  
        self.blue_spinbox.valueChanged.connect(self.updateBlueSlider)  
  
        rgb_widget = QWidget()  
        rgb_widget.setLayout(grid)  
  
        main_vbox = QVBoxLayout()  
        main_vbox.addWidget(self.cd_label)  
        main_vbox.addWidget(rgb_widget)  
  
        self.setLayout(main_vbox)  
  
    def updateRedSpinbox(self, value):  
        self.red_spinbox.setValue(value)  
        self.redValue(value)  
  
    def updateRedSlider(self, value):  
        self.red_slider.setValue(value)  
        self.redValue(value)  
  
    def updateGreenSpinBox(self, value):  
        self.green_spinbox.setValue(value)  
        self.greenValue(value)  
  
    def updateGreenSlider(self, value):  
        self.green_slider.setValue(value)  
        self.greenValue(value)  
  
    def updateBlueSpinBox(self, value):  
        self.blue_spinbox.setValue(value)  
        self.blueValue(value)  
  
    def updateBlueSlider(self, value):  
        self.blue_slider.setValue(value)  
        self.blueValue(value)  
  
    def redValue(self, value):  
        new_color = qRgb(value, self.current_val.green(), self.current_val.blue())  
        self.updateColorINfo(new_color)  
  
    def greenValue(self, value):  
        new_color = qRgb(self.current_val.red(), value, self.current_val.blue())  
        self.updateColorINfo(new_color)  
  
    def blueValue(self, value):  
        new_color = qRgb(self.current_val.red(), self.current_val.green(), value)  
        self.updateColorINfo(new_color)  
  
    def updateColorINfo(self, color):  
        self.current_val = QColor(color)  
        self.color_display.fill(color)  
        self.cd_label.setPixmap(QPixmap.fromImage(self.color_display))  
        self.hex_value_label.setText(f"{self.current_val.name()}")  
  
    def getPixelValue(self, event):  
        x = int(event.position().x())  
        y = int(event.position().y())  
  
        if self._image.valid(x, y):  
            self.current_val = QColor(self._image.pixel(x, y))  
  
            red_val = self.current_val.red()  
            green_val = self.current_val.green()  
            blue_val = self.current_val.blue()  
  
            self.updateRedSpinbox(red_val)  
            self.updateGreenSpinBox(green_val)  
            self.updateBlueSpinBox(blue_val)  
  
            self.updateRedSlider(red_val)  
            self.updateGreenSlider(green_val)  
            self.updateGreenSlider(blue_val)  
  
  
if __name__ == "__main__":  
    app = QApplication(sys.argv)  
    app.setStyleSheet(style_sheet)  
    window = RGBSlider()  
    sys.exit(app.exec())
```