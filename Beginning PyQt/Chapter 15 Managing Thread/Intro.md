Chúng ta đều từng gặp trường hợp khi chạy một quy trình, chẳng hạn như sao chép tệp giữa các thư mục hoặc khởi chạy một ứng dụng mới, chương trình bị lag trong chốc lát và đôi khi bị treo hoàn toàn. Trong những trường hợp như vậy, chúng ta phải chờ cho tác vụ hiện tại hoàn thành hoặc **Ctrl+Alt+Delete** để thoát. Khi tạo giao diện người dùng (GUI), cần chú ý cách xử lý và dự đoán để tránh những tình huống này.

Trong chương này, chúng ta sẽ:
- Xem xét các kỹ thuật xử lý quy trình tốn thời gian trong PyQt
- Học cách triển khai **threading** trong GUI với **QThread**
- Sử dụng widget **QProgressBar** để cung cấp phản hồi trực quan về tiến độ của một tác vụ

Động lực của chương này có hai mặt: giúp bạn thiết kế các ứng dụng GUI mạnh mẽ hơn và cung cấp kiến thức về cách xử lý các tình huống khi ứng dụng cần chạy các quy trình kéo dài. Bất kỳ hành động nào khiến quá trình xử lý sự kiện bị đình trệ đều gây ảnh hưởng xấu đến trải nghiệm người dùng.

## <span style="color:rgb(255, 105, 97)">Giới thiệu về Threading (Luồng)</span>

Hiệu suất của máy tính có thể được đo lường bằng độ chính xác, hiệu quả, và tốc độ mà nó thực thi các lệnh chương trình. Các máy tính hiện đại có thể tận dụng bộ vi xử lý đa lõi (multicore) để chạy các lệnh song song, từ đó tăng cường hiệu suất của các ứng dụng được viết để khai thác kiến trúc đa lõi. Việc thực hiện các tác vụ theo cách đồng bộ, nghĩa là chỉ xử lý một tác vụ tại một thời điểm cho đến khi hoàn thành trước khi chuyển sang tác vụ tiếp theo, có thể trở nên không hiệu quả, đặc biệt với các hoạt động lớn. Điều chúng ta cần là một cách để thực hiện các tác vụ song song, và đó là lý do tại sao **threads** (luồng) và **processes** (quá trình) được sử dụng.

### <span style="color:rgb(255, 179, 91)">Threads và Processes khác nhau như thế nào?</span>

- **Process** là một phiên bản của một ứng dụng yêu cầu bộ nhớ và tài nguyên máy tính để chạy. Ví dụ, khi mở trình soạn thảo văn bản trên máy tính để viết một bài luận, đó là một quá trình. Khi bạn cần tìm kiếm thông tin trên Internet trong khi viết bài, bạn có thêm một quá trình độc lập khác chạy song song với quá trình đầu tiên. Các quá trình này hoạt động độc lập và không ảnh hưởng đến nhau.
  
- **Thread** là yếu tố cốt lõi giúp thực hiện tính đồng thời bên trong một quá trình. Khi một quá trình bắt đầu, nó chỉ có một luồng, nhưng nhiều luồng có thể được bắt đầu trong cùng một quá trình. Những luồng này, giống như các quá trình, được quản lý bởi **CPU**. **Multithreading** xảy ra khi CPU có thể xử lý nhiều luồng cùng lúc trong một quá trình. Các luồng này độc lập nhưng cũng chia sẻ tài nguyên của quá trình. **Multithreading** giúp các ứng dụng phản hồi tốt hơn với các đầu vào của người dùng trong khi các hoạt động khác đang diễn ra trong nền, và tận dụng tốt hơn các tài nguyên của hệ thống.

Trên một hệ thống với CPU một lõi, **parallelism** thực sự là không thể đạt được. Trong trường hợp này, CPU được chia sẻ giữa các quá trình hoặc các luồng. **Context switches** được sử dụng để chuyển đổi giữa các luồng, làm gián đoạn luồng hiện tại, lưu trạng thái của nó và khôi phục trạng thái của luồng tiếp theo. Điều này tạo ra cảm giác song song giả.

Để đạt được tính song song thực sự, một bộ vi xử lý đa lõi sẽ cho phép các luồng trong một ứng dụng đa luồng được gán cho các lõi xử lý khác nhau, giúp hệ thống thực sự đồng thời.

## <span style="color:rgb(255, 105, 97)">Thread in PyQt</span> 

Khi phát triển ứng dụng dựa trên Qt, một luồng chính sẽ được tạo ra để quản lý tất cả các sự kiện trong giao diện người dùng. Để tăng hiệu suất và tránh làm giao diện bị đơ khi xử lý các tác vụ nặng, chúng ta có thể sử dụng các **luồng phụ** để xử lý song song các tác vụ mà không làm gián đoạn luồng chính. 

PyQt cung cấp cơ chế **signals và slots** để giao tiếp giữa luồng chính và luồng phụ, giúp báo cáo tiến trình hoặc kết thúc nhiệm vụ. Tuy nhiên, cần chú ý tránh các vấn đề như **tranh chấp tài nguyên** hoặc **deadlock** khi nhiều luồng truy cập cùng một tài nguyên. Các lớp như **QMutex**, **QReadWriteLock**, và **QSemaphore** có thể được sử dụng để đồng bộ hóa và quản lý tài nguyên giữa các luồng một cách an toàn.

## <span style="color:rgb(255, 105, 97)">Method for processing Long Event in PyQt</span> 

Có nhiều cách để xử lý các sự kiện dài trong PyQt mà không làm giao diện người dùng (GUI) bị đơ, bao gồm cả việc sử dụng luồng (threading). Dưới đây là các phương pháp chính để xử lý các tác vụ này:

1. **Xử lý theo từng bước nhỏ**: Nếu tác vụ có thể chia nhỏ, thực hiện từng bước tuần tự và gọi `QApplication.processEvents()` để xử lý các sự kiện đang chờ.
   
2. **Sử dụng QTimer**: Lên lịch các tác vụ thực hiện ở khoảng thời gian nhất định bằng QTimer, kết hợp với signals và slots.

3. **Sử dụng QThread**: Tạo luồng phụ để xử lý các tác vụ dài mà không chặn luồng chính. Reimplement phương thức `run()` và sử dụng signals/slots để giao tiếp với luồng chính.

4. **Sử dụng QThreadPool và QRunnable**: Phân chia công việc và sử dụng tất cả các lõi CPU. Tạo một lớp con từ QRunnable, reimplement `run()`, và sử dụng QThreadPool để quản lý và thực thi các tác vụ.

Chọn phương pháp phù hợp tùy thuộc vào yêu cầu của ứng dụng, và cần cẩn thận khi sử dụng luồng để tránh các vấn đề về hiệu suất và lỗi tiềm ẩn.