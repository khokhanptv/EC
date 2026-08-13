# Luyện phỏng vấn C/C++ Embedded

## Ngày 13/08/2026

### Câu 1: Biến là gì? Khai báo và khởi tạo khác nhau thế nào?1

**Trả lời phỏng vấn:** Biến là một đối tượng có tên, có kiểu dữ liệu và dùng để lưu giá trị. Khai báo giới thiệu tên và kiểu của biến; định nghĩa tạo biến và thường cấp vùng nhớ. Khởi tạo cung cấp giá trị ban đầu khi biến được định nghĩa.

### Câu 2: Biến local, global và `static` khác nhau thế nào?

**Trả lời phỏng vấn:** Biến local chỉ dùng trong khối khai báo và thường nằm trên stack. Biến global dùng được ở phạm vi toàn cục. Biến `static` tồn tại suốt chương trình. Global và static thường nằm ở Data hoặc BSS.

### Câu 3: Con trỏ là gì? `&x` và `*p` có ý nghĩa gì?

**Trả lời phỏng vấn:** Con trỏ là biến lưu địa chỉ. `&x` lấy địa chỉ của `x`; `*p` lấy giá trị tại địa chỉ mà `p` trỏ tới.

### Câu 4: Con trỏ null, con trỏ hoang và con trỏ treo khác nhau thế nào?

**Trả lời phỏng vấn:** Con trỏ null không trỏ đến đối tượng hợp lệ. Wild pointer chưa được khởi tạo. Dangling pointer trỏ đến vùng nhớ hoặc đối tượng đã hết thời gian sống.

### Câu 5: Có được trả về địa chỉ của biến local không?

**Trả lời phỏng vấn:** Không. Biến local hết thời gian sống khi hàm kết thúc, nên con trỏ trả về bị treo. Truy cập qua con trỏ đó gây undefined behavior.

### Câu 6: `malloc()` và `free()` dùng để làm gì?

**Trả lời phỏng vấn:** `malloc()` cấp phát bộ nhớ động nhưng không khởi tạo giá trị. `free()` giải phóng vùng nhớ đó. Không `free()` khi không còn sử dụng sẽ gây memory leak.

### Câu 7: Truy cập con trỏ sau khi `free()` gây ra điều gì?

**Trả lời phỏng vấn:** Sau `free()`, con trỏ trở thành dangling pointer. Truy cập vùng nhớ qua nó là lỗi use-after-free và gây undefined behavior.

### Câu 8: `const int *p` và `int *const p` khác nhau thế nào?

**Trả lời phỏng vấn:** `const int *p` là con trỏ tới hằng: đổi được địa chỉ `p`, không sửa được `*p` qua con trỏ này. `int *const p` là con trỏ hằng: không đổi được địa chỉ `p`, nhưng sửa được `*p`.

### Câu 9: `struct` là gì? Vì sao kích thước có thể lớn hơn tổng các thành viên?

**Trả lời phỏng vấn:** `struct` là kiểu dữ liệu do người dùng định nghĩa, dùng để nhóm nhiều thành viên. Kích thước của nó gồm kích thước các thành viên và padding do yêu cầu căn chỉnh bộ nhớ.

### Câu 10: `volatile` có ý nghĩa gì? Có đảm bảo atomic và thread-safe không?

**Trả lời phỏng vấn:** volatile báo cho compiler không được tối ưu , ngăn compiler dùng lại giá trị cũ trong thanh ghi;
- Biến thường: compiler có thể giữ bản sao trong thanh ghi CPU để dùng lại.
- Biến volatile: compiler không được dùng lại giá trị cũ trong thanh ghi,mỗi lần biến bị thay đổi bởi yếu tố bên ngoài compiler phải đọc lại biến, không được dùng giá trị cũ giữ trong thanh ghi.