Trong project này, chúng ta sẽ kết hợp nhiều khái niệm đã học từ các chương trước, bao gồm thanh menu (menu bars), thanh công cụ (toolbars), thanh trạng thái (status bars), hộp thoại (dialog boxes), tạo biểu tượng (icons), và tái triển khai các bộ xử lý sự kiện (event handlers), cùng với việc sử dụng lớp `QPainter`. Ngoài ra, chúng ta sẽ bổ sung một vài ý tưởng mới, tập trung vào cách tạo lời gợi ý (tool tips) và theo dõi vị trí của con trỏ chuột.

## <span style="color:rgb(255, 105, 97)">Cài đặt:</span>

### <span style="color:rgb(255, 179, 91)">Tạo lớp canvas để vẽ</span> 

#### <span style="color:rgb(255, 238, 140)">Khởi tạo</span>
```python
from PyQt6.QtCore import Qt, QPoint  
from PyQt6.QtGui import QPixmap  
from PyQt6.QtWidgets import QLabel  
from PyQt6.QtWidgets.QWidget import width  
  
  
# widget to be drawn on  
class Canvas(QLabel):  
    def __init__(self, parent):  
        super().__init__()  
        self.parent = parent  
        width, height = parent.width(), parent.height()  
        self.pixmap = QPixmap(width, height)  
        self.pixmap.fill(Qt.GlobalColor.white)  
  
        # keep tracking the mouse for getting mouse coordinate  
        self.mouse_track_label = QLabel()  
        self.setMouseTracking(True)  
  
        # init variables  
        self.antialiasing_status = False  
        self.eraser_selected = False  
  
        self.last_mouse_pos = QPoint()  
        self.drawing = False  
        self.pen_color = Qt.GlobalColor.black  
        self.pen_width = 2
```

note:
- `mouse_track_label`: hiển thị tọa độ con trỏ

- `eraser_selected`: kiểm tra nếu tẩy được lựa chọn

- `antialiasing_status`: kiểm tra nếu người dùng chọn tính năng khử răng cưa

- **`last_mouse_pos`**: Lưu lại vị trí cuối cùng của con trỏ chuột khi nút chuột trái được nhấn hoặc khi con trỏ chuột di chuyển. Biến này giúp theo dõi chính xác vị trí trước đó của con trỏ để vẽ liền mạch.
  
- **`drawing`**: Giá trị **True** khi nút chuột trái đang được nhấn, biểu thị rằng người dùng có thể đang vẽ. Biến này thường được sử dụng để kiểm tra xem có cần thực hiện thao tác vẽ khi con trỏ chuột di chuyển hay không.

- **`pen_color`, `pen_width`**: Các biến này lưu giá trị ban đầu của màu và độ rộng của bút vẽ (`pen`) và cọ (`brush`). Chúng sẽ xác định màu sắc và độ dày của nét vẽ khi người dùng thao tác.

#### <span style="color:rgb(255, 238, 140)">Cài đặt tùy chỉnh công cụ</span> 

```python
def selectDrawingTool(self, tool):  
    if tool == 'pencil':  
        self.eraser_selected = False  
        self.pen_width = 2  
    elif tool == 'marker':  
        self.eraser_selected = False  
        self.pen_width = 8  
    elif tool == 'eraser':  
        self.eraser_selected = True  
    elif tool == 'color':  
        self.eraser_selected = False  
        color = QColorDialog.getColor()  
        if color.isValid(): self.pen_color = color
```

#### <span style="color:rgb(255, 238, 140)">Xử lý chuột</span> 

```python

```