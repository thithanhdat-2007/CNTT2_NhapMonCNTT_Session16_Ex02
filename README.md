# Bài tập Session 16 – Ex02: Phân quyền và xử lý chia sẻ nhầm dữ liệu ứng viên

Họ và tên: Thi Thành Đạt  
MSSV: N26DTCN041  
Lớp: CNTT2  
Môn học: Nhập môn Công nghệ thông tin  
Email: thithanhdat111@gmail.com

## 1. Xác định mức độ nhạy cảm

Danh sách CV có số điện thoại và mức lương kỳ vọng là tài liệu nhạy cảm. Theo **Quy tắc Cờ Đỏ Bảo mật**, em đặt phạm vi chia sẻ là **Restricted**, chỉ thêm đúng tài khoản của người cần làm việc; tuyệt đối không chọn **Anyone with the link**. Link đã gửi ra ngoài không phải là bằng chứng rằng dữ liệu đã bị tải về, nhưng phải xử lý như một sự cố có nguy cơ rò rỉ.

Ba mức quyền có ý nghĩa khác nhau: **Viewer** chỉ xem, **Commenter** xem và để lại nhận xét, còn **Editor** có thể sửa nội dung. Vì thế không cấp Editor cho người chỉ cần đọc hoặc góp ý.

## 2. Bản thiết kế phân quyền

| Đối tượng | Tài liệu được truy cập | Mức quyền | Phạm vi | Lý do |
| --- | --- | --- | --- | --- |
| Trưởng phòng và nhân viên nhân sự trực tiếp quản lý tuyển dụng | Danh sách ứng viên gốc | Editor | Restricted, theo tài khoản được chỉ định | Cần cập nhật hồ sơ và chịu trách nhiệm duyệt nội dung. |
| Trưởng bộ phận tham gia tuyển người | Danh sách ứng viên gốc | Commenter | Restricted, theo tài khoản được chỉ định | Cần xem và nhận xét, không được sửa số điện thoại hay mức lương. |
| Người được giao xem báo cáo tuyển dụng nhưng không xử lý hồ sơ | Bản tóm tắt đã bỏ SĐT và lương | Viewer | Restricted, theo tài khoản được chỉ định | Chỉ cần đọc kết quả; không cần thấy dữ liệu cá nhân trong bản gốc. |
| Ứng viên bên ngoài và nhân viên không liên quan | Không được truy cập danh sách gốc | Không cấp quyền | Không chia sẻ | Không có nhiệm vụ cần xem dữ liệu của các ứng viên khác. |

Chủ file kiểm tra thêm quyền được thừa kế từ thư mục chứa file. Trong phần cài đặt chia sẻ, em tắt quyền để Editor tự đổi quyền hoặc mời người khác, và hạn chế tải xuống, sao chép, in đối với người không cần các thao tác đó. Các hạn chế trên màn hình không thể ngăn hoàn toàn việc chụp ảnh hoặc ghi lại nội dung đã xem.

## 3. Góp ý bằng Suggesting Mode

Nếu danh sách được trình bày trong **Google Docs**, trưởng bộ phận có quyền **Commenter** có thể chọn **Suggesting** và đề xuất sửa câu chữ. Đề xuất hiện riêng, chỉ trở thành nội dung chính khi nhân sự chấp nhận. Nhân sự vẫn là người duyệt và cập nhật dữ liệu gốc.

Nếu danh sách là **Google Sheets**, em không gọi thao tác nhận xét trên bảng tính là Suggesting Mode. Trưởng bộ phận dùng **Comment** tại ô cần góp ý; nếu cần đề xuất sửa bằng Suggesting Mode, nhóm lập một Google Docs riêng để nhận xét theo mã ứng viên, không chép SĐT và lương sang tài liệu đó. Như vậy bảng dữ liệu gốc không bị sửa trực tiếp.

## 4. Kịch bản xử lý sự cố trong 3 bước

1. **Thu hồi quyền ngay:** Mở **Share / Manage access** của file, đổi **General access** thành **Restricted**, xóa quyền của ứng viên nếu tài khoản của họ đã được thêm trực tiếp, rồi kiểm tra quyền từ thư mục cha và các tài khoản bên ngoài khác. Kiểm tra lại bằng tài khoản không được cấp quyền để chắc link cũ không mở được.
2. **Rà soát dấu vết truy cập:** Ghi lại thời điểm chia sẻ nhầm và mã file. Nếu công ty dùng Google Workspace và có quyền quản trị, vào **Admin console → Reporting → Audit and investigation → Drive log events**, lọc theo file, khoảng thời gian và tài khoản liên quan; tìm các sự kiện **View**, **Download**, **Source Copy**, chỉnh sửa và thay đổi quyền. Nếu không có quyền quản trị, nhờ quản trị viên kiểm tra; mục Activity thông thường không đủ để khẳng định ứng viên đã hay chưa tải bản sao.
3. **Xử lý hậu quả và ngăn lặp lại:** Báo trưởng phòng nhân sự và người phụ trách an toàn dữ liệu về kết quả rà soát, kiểm tra nội dung file có bị sửa hay không và khôi phục bản đúng nếu cần. Nếu thấy dấu hiệu dữ liệu đã ra ngoài, xử lý theo quy trình sự cố của công ty. Sau đó rà soát lại toàn bộ quyền, chỉ giữ các tài khoản trong bảng phân quyền và kiểm tra định kỳ.

Việc thu hồi quyền chỉ chặn truy cập **từ thời điểm thu hồi trở đi**. Nhật ký có thể cho thấy nhiều lượt xem, tải hoặc sao chép, nhưng không ghi được mọi cách lấy thông tin; không thấy sự kiện cũng không chứng minh chắc chắn là dữ liệu chưa bị lưu lại.
