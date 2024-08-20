
## <span style="color:rgb(255, 105, 97)">QRadioButton widget</span> 
Lớp `QRadioButton` trong PyQt cho phép tạo các nút tùy chọn mà có thể được bật (checked) hoặc tắt (unchecked). Nút radio bao gồm một nút tròn và một nhãn hoặc biểu tượng tương ứng, và chúng rất hữu ích trong các tình huống cần cung cấp cho người dùng nhiều lựa chọn, nhưng chỉ một lựa chọn có thể được chọn tại một thời điểm.

Khi người dùng chọn một nút radio mới, các nút radio khác sẽ tự động bị bỏ chọn. Để làm điều này, bạn cần đặt nhiều nút radio vào một widget cha. Khi đó, các nút này sẽ trở thành **autoexclusive**, có nghĩa là chúng tự động trở thành phần tử của một nhóm độc quyền lẫn nhau. Nếu một nút radio được chọn trong nhóm, tất cả các nút còn lại sẽ tự động bị bỏ chọn. Thay đổi tính năng này bằng cách đặt giá trị của phương thức `setAutoExclusive()` của `QRadioButton` thành `False`.

Ngoài ra, có thể có nhiều nhóm nút radio độc quyền trong cùng một widget cha bằng cách sử dụng lớp `QButtonGroup` để tách biệt và quản lý các nhóm khác nhau.

Nút radio tương tự như lớp `QCheckBox` về cách phát ra các tín hiệu. Một nút radio sẽ phát ra tín hiệu `toggled` khi nó được bật hoặc tắt, và bạn có thể kết nối tín hiệu này với một slot để thực hiện các hành động khi trạng thái của nút radio thay đổi.

## <span style="color:rgb(255, 105, 97)">QGroupBox class</span> 

`QGroupBox` là một khung hình chữ nhật được sử dụng để nhóm các widget lại với nhau trong PyQt. Một group box có viền với tiêu đề nằm ở phía trên. Tiêu đề của group box có thể có thuộc tính checkable, cho phép bật hoặc tắt các widget con bên trong group box khi tiêu đề được chọn (checked) hoặc bỏ chọn (unchecked).

`QGroupBox` có thể chứa bất kỳ loại widget nào. Tuy nhiên, do `QGroupBox` không tự động sắp xếp các widget con của nó, nên cần áp dụng một layout manager để tổ chức các widget bên trong. Điều này giúp đảm bảo rằng các widget con được sắp xếp đúng cách và có giao diện gọn gàng, rõ ràng.

ví dụ:
```python
effects_gb = QGroupBox("Effect")#title

effect1_rb = QRadioButton("Strikethrough")
effect2_rb = QRadioButton("Outline")

gb_hbox = QHBoxLayout()
gb_hbox.addWidget(effect1_rb)
gb_hbox.addWidget(effect2_rb)

effects_gb.setLayout(gb_hbox)
```

## <span style="color:rgb(255, 105, 97)">QTabWidget class</span> 

Thường được sử dụng để tổ chức thông tin liên quan vào các page riêng biệt trông một cửa sổ nhằm tránh GUI bị lộn xộn. Class này cung cấp một thanh tab ở trên cùng và một vùng bên dưới tab để hiện thị thông tin liên quan đến tab và các widget đi kèm. Mỗi lần click chỉ hiển thị một page tương ứng

Có nhiều cách để tương tác với tab, ví dụ:
- switch tab: khi đổi tab, tín hiệu `currentChanged` được gửi đi 
- Enabling/Disabling tab: điều hướng tab thành bật/tắt bằng `setTabEnabled()`


## <span style="color:rgb(255, 105, 97)">Ứng dụng </span> 

![](Pasted%20image%2020240820131514.png)

```python
import sys  
  
from PyQt6.QtWidgets import QWidget, QApplication, QTabWidget, QHBoxLayout, QLineEdit, QLabel, QRadioButton, \  
    QVBoxLayout, QGroupBox  
  
  
class MainWindow(QWidget):  
    def __init__(self):  
        super().__init__()  
        self.initializeUI()  
  
    def initializeUI(self):  
        self.setMinimumSize(400, 300)  
        self.setWindowTitle("Container ex")  
  
        self.setupMainWindow()  
        self.show()  
  
    def setupMainWindow(self):  
        """create and arrange widget in main window"""  
        """setup tab bar"""  
  
        # create tab bar and page containers  
        tab_bar = QTabWidget(self)  
        self.prof_details_tab = QWidget()  
        self.background_tab = QWidget()  
  
        tab_bar.addTab(self.prof_details_tab, "profile details")  
        tab_bar.addTab(self.background_tab, "Background")  
  
        # Call method to create page  
        self.profileDetailTab()  
        self.backgroundTab()  
  
        # set layout  
        main_hbox = QHBoxLayout()  
        main_hbox.addWidget(tab_bar)  
        self.setLayout(main_hbox)  
  
    def profileDetailTab(self):  
        """profile page allow user to enter their name,bla bla"""  
        # setup label        name_label = QLabel("Name")  
        name_edit = QLineEdit()  
  
        address_label = QLabel("Address")  
        address_edit = QLineEdit()  
  
        # create radio button  
        male_rb, female_rb = QRadioButton("Male"), QRadioButton("Female")  
  
        gender_hbox = QHBoxLayout()  
        gender_hbox.addWidget(male_rb)  
        gender_hbox.addWidget(female_rb)  
  
        # create group to contain radiobutton  
        gender_gb = QGroupBox("Gender")  
        gender_gb.setLayout(gender_hbox)  
  
        # add all widget to the profile details  
        tab_vbox = QVBoxLayout();  
        tab_vbox.addWidget(name_label)  
        tab_vbox.addWidget(name_edit)  
        tab_vbox.addStretch()  
        tab_vbox.addWidget(address_label)  
        tab_vbox.addWidget(address_edit)  
        tab_vbox.addStretch()  
        tab_vbox.addWidget(gender_gb)  
  
        # set layout 4 profile tab  
        self.prof_details_tab.setLayout(tab_vbox)  
  
    def backgroundTab(self):  
        """let user select their educational background"""  
        # layout 4 education_gb        ed_v_box = QVBoxLayout()  
  
        # Create and add radio buttons to ed_v_box  
  
        education_list = ["1", "2", "3", "4", "5", "6", "7", "8", "9"]  
  
        for ed in education_list:  
            self.education_rb = QRadioButton(ed)  
            ed_v_box.addWidget(self.education_rb)  
  
        # Set up group box to hold radio buttons  
        self.education_gb = QGroupBox(  
            "Highest Level of Education")  
        self.education_gb.setLayout(ed_v_box)  
  
        # Create and set for background tab  
        tab_v_box = QVBoxLayout()  
        tab_v_box.addWidget(self.education_gb)  
  
        # Set layout for background tab  
        self.background_tab.setLayout(tab_v_box)  
  
  
if __name__ == '__main__':  
    app = QApplication(sys.argv)  
    window = MainWindow()  
    sys.exit(app.exec())
```