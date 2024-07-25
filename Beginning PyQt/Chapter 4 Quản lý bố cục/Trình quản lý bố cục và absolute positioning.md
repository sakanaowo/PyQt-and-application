### <span style="color:rgb(255, 105, 97)">Layout Manager - quản lý bố cục</span>
- Khi triển khai các widget thì cần xem xét các trường hợp như:
	- Kích thước và vị trí của widget này với widget khác
	- Xử lý khi cửa sổ bị thay đổi
	- Xử lý khi widget được thêm hoặc xóa
- Class quản lý bố cục giúp tổ chức bố cục trở nên đơn giản và các widget có thể giao tiếp với nhau 
#### Ví dụ:  sắp xếp một QLabel widget nằm trên một QLineEdit widget:
- Tạo widget :
	```python
	label = QLabel("Name")
	line_edit = QLineEdit()
	```
	note:
	- widget gốc lẫn tham số self đều không tự chuyển thành đối số (argument) khi tạo các widget
	- Trình quản lý bố cục sẽ tự động gán lại các widget để liên kết với widget gốc
	-> Trình quản lý bố cục sẽ thiết lập các widget gốc cho các widget con

- Tạo một trình quán lý bố cục cho widget theo chiều dọc bằng `QVBoxLayout()`
	```python
	v_box = QVBoxLayout()  
	v_box.addWidget(label)  
	v_box.addWidget(line_edit)
	parent_widget.setLayout(v_box)
	```
	note :
	- Widget gốc có thể là :
		- một widget
		- cửa sổ
		- hộp thoại
	- <span style="color:rgb(20, 225, 191)">Trình quản lý không phải là widget gốc, chỉ widget mới là widget gốc cho widget khác</span> 
	Hoặc thay vì sử dụng `setLayout()`, ta có thể dùng:
	```python
	v_box = QVBoxLayout(parent_widget)
	```

- Thêm bố cục vào các bố cục khác để tạo _bố cục lồng nhau_ - _nested layout_ :
	```python
	h_box = QHBoxLayout()
	v_box.addLayout(h_box)
	```
	note: 
	- `h_box` là bố cục con của bố cục gốc `v_box` 
	- `h_box` là bố cục bên trong

### <span style="color:rgb(255, 105, 97)">Absolute Positioning</span> 
- Một phương pháp sắp xếp các widget trong giao diện là Aboslute positioning, liên quan đến việc chỉ định các giá trị kích thước và vị trí cho widget con trong widget gốc
- Ý tưởng này đã được giới thiệu và sử dụng trong chap 2 và 3 bằng cách sử dụng phương thức move() của QWidget.
- Aboslute positioning có thể hữu ích để thiết lập các vị trí và kích thước của các widget nằm trong các widget khác, hoặc có thể là để định vị lại vị trí của cửa sổ trên màn hình.
- một số nhược điểm cần lưu ý. 
  - thay đổi kích thước cửa sổ chính sẽ không làm cho các widget trong nó thay đổi kích thước hoặc vị trí của chúng.
  - sự khác biệt giữa các hệ điều hành, chẳng hạn như phông chữ và kích thước phông chữ, có thể thay đổi đáng kể giao diện và bố cục của các widget trên các nền tảng khác nhau.
  - việc sử dụng Aboslute positioning tốn nhiều thời gian, vì cần tính toán chính xác kích thước và vị trí của từng widget cũng như xử lý các vấn đề như thay đổi kích thước cửa sổ và thêm hoặc xóa các widget trong GUI.