GUI phản hồi các sự kiện do người dùng tạo ra. Trong Qt, có một số [sự kiện](obsidian://open?vault=Pyqt%20and%20application&file=Beginning%20PyQt%2FUntitled%2Fs%E1%BB%B1%20ki%E1%BB%87n) được tạo ra để xử lý giao tiếp giữa các widget. App cần nhận biết và phản hồi sự kiện đó một cách thích hợp => event handling - xử lý sự kiện.
Khi hàm `exec()` được gọi thì app sẽ nhận dạng và xử lý cho đến khi chương trình được đóng
Trong PyQt, event handling - xử lý sự kiện sẽ được xử lý theo 1 trong 2 cách:
- Event handler:
	- Quản lý các sự kiện
- Signal and slot 