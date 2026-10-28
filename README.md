<details>
<summary><h1>Luyện phỏng vấn C/C++ Embedded</h1></summary>

> Bản tổng hợp Câu 1–140.  

> Phần STL đã được tinh gọn theo hướng phỏng vấn: bỏ các câu đi quá sâu vào từng hàm riêng lẻ của `std::vector`.

---
<details>
<summary><strong>CÂU HỎI PHỎNG VẤN EMBEDDED / C++</strong></summary>

<details>
<summary><h2>📚 TỔNG HỢP 12 NHÓM KIẾN THỨC — EMBEDDED C/C++</h2></summary>

> Nhấn vào từng **nhóm** rồi từng **khái niệm** để xem giải thích ngắn và ví dụ.


- Trong quá trình làm việc, team có thường xuyên tổ chức các buổi training hoặc chia sẻ kiến thức, kinh nghiệm kỹ thuật giữa các thành viên không ạ?

- Trong trường hợp gặp những vấn đề kỹ thuật phức tạp cần sự hỗ trợ từ team Nhật Bản, hai bên thường phối hợp và trao đổi như thế nào ạ? Ngoài ra, các kỹ sư tại Việt Nam có cơ hội sang Nhật để làm việc trực tiếp và trao đổi kỹ thuật không ạ?

- Ngoài những kiến thức và kỹ năng đã được đề cập trong JD, anh/chị có thể chia sẻ thêm những công nghệ hoặc kiến thức chuyên môn mà team hiện đang đặc biệt quan tâm và ưu tiên không ạ?

ISO 13485:2016 : là tiêu chuẩn quản lý chất lượng thiết bị y tế
IEC 62304 : là tiêu chuẩn quản lý phát triển , bảo trì phần mềm cho thiết bị y tế

<details>
<summary><strong>NHÓM 1. C PROGRAMMING</strong></summary>

<details>
<summary><strong>Bitwise</strong></summary>

*Bitwise là các phép toán thao tác trực tiếp trên từng bit của một giá trị. quan trọng vì xử lý bi cần mà không ảnh hưởng bit khác

bit mask là giá trị dùng để chỉ ra bit nào mình muốn tác động.

OR  với 1 → BẬT bit ví dụ reg = reg | (1U << 3);

AND với 0 → TẮT bit reg = reg & ~(1U << 3);

XOR với 1 → ĐẢO bit eg = reg ^ (1U << 3);

AND với 1 → KIỂM TRA :

if (reg & (1U << 3))
{
    // bit 3 = 1
}

</details>

<details>
<summary><strong>signed / unsigned</strong></summary>

**Là gì?** `signed` biểu diễn số âm và dương; `unsigned` chỉ biểu diễn số không âm.  
**Ví dụ:** `uint8_t x = 255; int8_t y = -1;`

</details>

<details>
<summary><strong>sizeof</strong></summary>

**Là gì?** Toán tử trả về kích thước đối tượng/kiểu theo byte, kiểu trả về là `size_t`.  
**Ví dụ:** `sizeof(uint32_t)` thường là `4` nếu `CHAR_BIT == 8`.

</details>

<details>
<summary><strong>Pointer trong Embedded C — Khái niệm và ứng dụng</strong></summary>

### 1. Pointer là gì?

Pointer là biến lưu địa chỉ bộ nhớ của đối tượng hoặc hàm.

Trong Embedded, pointer được dùng để truy cập các thanh ghi phần cứng thông qua Memory-Mapped I/O.

Memory-Mapped I/O là cơ chế ánh xạ thanh ghi phần cứng vào các địa chỉ của CPU.

ánh xạ :
Gắn một địa chỉ của CPU với một thanh ghi phần cứng cụ thể.

ví dụ
Thanh ghi phần cứng GPIO
        ↕
được ánh xạ
        ↕
địa chỉ 0x40020000
        ↑
Pointer truy cập địa chỉ này

Pointer
  + Truy cập thanh ghi phần cứng
  + Truy cập/duyệt buffer dữ liệu UART, SPI, I2C, ...

tại sao phải volatile uint32_t *p = (volatile uint32_t *)0x40020000;

khi truy cập thanh ghi phần cứng?

Vì giá trị của thanh ghi phần cứng có thể thay đổi bởi phần cứng mà chương trình không trực tiếp thay đổi nó
Giúp đảm bảo chương trình thực hiện lại việc đọc thanh ghi khi code yêu cầu, thay vì compiler tối ưu theo cách có thể tái sử dụng giá trị cũ.
ví dụ về con trỏ trong nhúng
.nếu một thanh ghi GPIO được ánh xạ tại địa chỉ 0x01, ta có thể dùng một con trỏ volatile trỏ tới địa chỉ đó và dereference con trỏ để đọc hoặc ghi thanh ghi.

Nếu không có pointer trong C/C++, lập trình Embedded sẽ gặp các vấn đề lớn:
Khó truy cập trực tiếp thanh ghi phần cứng/ buffer
có thể dùng Assembly của CPU để CPU xử lý , nhưng sẽ phức tạp hơn
```c

volatile int *p = (volatile int *)0x01;
*p=1;

// p  : địa chỉ của x
// *p : giá trị 10
```

### 2. Ứng dụng trong Embedded

**Ứng dụng 1 — Truy cập thanh ghi MCU**

```c
#include <stdint.h>

// Địa chỉ register giả định
#define REG (*(volatile uint32_t *)0x40020014U)

REG |= (1U << 5);  // Set bit 5
```

- Dùng để đọc/ghi thanh ghi GPIO, UART, Timer.
- `volatile` giúp compiler không loại bỏ các truy cập register cần thiết.

**Ứng dụng 2 — Xử lý UART Buffer**

```c
uint8_t buffer[] = {10, 20, 30};
uint8_t *p = buffer;

uint8_t value = *(p + 1); // 20
```

- Dùng Pointer để truy cập từng byte dữ liệu nhận được.
- Cần kiểm tra độ dài buffer để tránh truy cập ngoài mảng.

**Ứng dụng 3 — Trả kết quả qua Pointer**

```c
void ReadSensor(int *value)
{
    *value = 100;
}

int sensor = 0;
ReadSensor(&sensor);

// sensor = 100
```

- Hàm thay đổi trực tiếp biến được truyền vào.
- Thường dùng trong các API đọc dữ liệu cảm biến.

**Ứng dụng 4 — Function Pointer / Callback**

```c
void OnReceive(void)
{
    // Xử lý dữ liệu
}

void (*callback)(void) = OnReceive;

callback();
```

- Dùng cho UART, DMA, Timer callback.
- Giúp thiết kế phần mềm theo hướng event-driven.

### 3. Các loại Pointer

| Loại | Ví dụ |
|---|---|
| Normal Pointer | `int *p = &x;` |
| NULL Pointer | `int *p = NULL;` |
| Void Pointer | `void *p = &x;` |
| Double Pointer | `int **pp = &p;` |
| Function Pointer | `void (*cb)(void);` |
| Pointer to const | `const int *p;` |
| Const Pointer | `int *const p = &x;` |
| Pointer to volatile | `volatile uint32_t *reg;` |

### 4. Trả lời phỏng vấn

Pointer là biến lưu địa chỉ bộ nhớ. Trong Embedded,
Pointer thường được sử dụng để:

1. Truy cập thanh ghi phần cứng.
2. Xử lý buffer UART/SPI/I2C.
3. Truyền địa chỉ dữ liệu giữa các hàm.
4. Triển khai callback bằng Function Pointer.
5. Quản lý bộ nhớ và cấu trúc dữ liệu.

Khi sử dụng cần tránh NULL Pointer, Dangling Pointer,
Buffer Overflow và truy cập bộ nhớ không hợp lệ.

ví dụ:
Function Pointer là con trỏ lưu địa chỉ của một hàm, cho phép gọi hàm thông qua con trỏ.

Trong Embedded C, Function Pointer thường được sử dụng để triển khai Callback giữa Driver Layer và Application Layer.

Ví dụ, khi UART Driver nhận được dữ liệu, Driver có thể gọi Callback thông qua Function Pointer để thông báo cho Application Layer xử lý.

Ưu điểm là giảm sự phụ thuộc giữa các module, giúp Driver không cần biết cụ thể hàm xử lý ở Application Layer, đồng thời tăng khả năng tái sử dụng và bảo trì phần mềm.

Driver Layer (tầng điều khiển phần cứng) là tầng phần mềm trực tiếp làm việc với các thiết bị ngoại vi của MCU.
Ví dụ: UART Driver chịu trách nhiệm cấu hình baud rate, nhận dữ liệu UART và xử lý ngắt UART.
Application Layer (tầng ứng dụng) là tầng chứa logic xử lý chức năng chính của sản phẩm.
rong thiết bị đo nhiệt độ:
Ví dụ: Trong thiết bị đo nhiệt độ:
- Driver Layer đọc dữ liệu từ cảm biến.
- Application Layer kiểm tra nhiệt độ có vượt ngưỡng không.
- Nếu vượt ngưỡng, Application yêu cầu bật đèn cảnh báo.

| Tầng | Chức năng | Ví dụ |
|---|---|---|
| **Hardware Layer** | Phần cứng thực tế | MCU, cảm biến |
| **Driver Layer** | Giao tiếp và điều khiển phần cứng | UART Driver đọc dữ liệu |
| **Application Layer** | Xử lý logic của sản phẩm | Kiểm tra dữ liệu và bật cảnh báo |

</details>

<details>
<summary><strong>Double pointer</strong></summary>

**Là gì?** Con trỏ trỏ tới một con trỏ khác.  
**Ví dụ:** `int *p=&x; int **pp=&p;` → `**pp` là `x`.
Double pointer là pointer trỏ đến một pointer khác. Nó thường được dùng khi một function cần thay đổi giá trị của pointer bên ngoài function.

</details>

<details>
<summary><strong>void pointer</strong></summary>

**Là gì?** Con trỏ có thể chứa địa chỉ nhiều kiểu; phải chuyển kiểu phù hợp trước khi giải tham chiếu.  
**Ví dụ:** `void *p=&x; int v=*(int*)p;`

</details>

<details>
<summary><strong>Pointer arithmetic</strong></summary>

**Là gì?** Pointer Arithmetic là các phép toán trên con trỏ, giúp di chuyển vị trí con trỏ theo kích thước kiểu dữ liệu.
Nó thường dùng để duyệt mảng và xử lý buffer dữ liệu trong Embedded C
**Ví dụ:** `int a[2]={3,4}; int *p=a; (*p)++;` là `3+1=4`.
`p++ thì bằng 4, giải thích p tới vị trí thứ 2 là 4`
Có 1 số chú ý như sau:
p++;       // di chuyển CON TRỎ
(*p)++;    // tăng GIÁ TRỊ mà p đang trỏ tới
*p++ tưởng đương với *(p++)      // lấy GIÁ TRỊ hiện tại, rồi di chuyển CON TRỎ
*(p + 1), thực chất tương đương a[1]

chú ý:
với biến đơn lẻ thì sao

```c
int x = 10;
int *p = &x;
p++;  // Hợp lệ, nhưng p trỏ đến vị trí ngay sau x, sẽ rơi vào trường hợp không xác định 
```

</details>

<details>
<summary><strong>Array / pointer</strong></summary>

**Là gì?** Mảng chứa nhiều phần tử liên tiếp; trong nhiều biểu thức tên mảng chuyển thành pointer tới phần tử đầu, nhưng không phải một pointer variable.  
**Ví dụ:** `int a[3]; sizeof(a)` là kích thước toàn mảng trong cùng scope.

</details>

<details>
<summary><strong>const</strong></summary>

**Là gì?** Ngăn sửa đối tượng qua đường truy cập `const`.  
**Ví dụ:** `const int x=10;` không thể viết `x=20;`.

</details>

<details>
<summary><strong>static</strong></summary>

**Là gì?** Biến local `static` giữ giá trị giữa các lần gọi; ở file scope giới hạn linkage.  
**Ví dụ:** `static int count=0; count++;`

</details>

<details>
<summary><strong>volatile</strong></summary>

**Là gì?** Báo compiler rằng giá trị có thể đổi ngoài luồng thông thường; **không** đảm bảo atomic/thread-safe.  
**Ví dụ:** `volatile uint32_t *reg=(volatile uint32_t*)ADDR;`

</details>

<details>
<summary><strong>extern</strong></summary>

**Là gì?** Khai báo một biến/hàm có định nghĩa ở nơi khác.  
**Ví dụ:** `extern int g_counter;`

</details>

<details>
<summary><strong>Stack / Heap</strong></summary>

**Là gì?** Stack thường chứa biến cục bộ và call frames; heap phục vụ cấp phát động.  
**Ví dụ:** `int x=1; int *p=malloc(sizeof *p); free(p);`

</details>

<details>
<summary><strong>.text / .data / .bss</strong></summary>

**Là gì?** `.text` thường chứa mã; `.data` dữ liệu global/static khởi tạo khác 0; `.bss` dữ liệu zero-initialized.  
**Ví dụ:** `int a=3;` → `.data`; `static int b;` → `.bss` (thông thường).

</details>

<details>
<summary><strong>malloc / calloc / realloc / free</strong></summary>

**Là gì?** calloc khởi tạo các byte của vùng cấp phát về 0, còn malloc không làm vậy 
**Ví dụ:** `int *p=malloc(5*sizeof *p); free(p);`

</details>

<details>
<summary><strong>Memory leak</strong></summary>

**Là gì?** Bộ nhớ cấp phát không được giải phóng và không còn cách thu hồi hợp lệ.  
**Ví dụ:** `p=malloc(100); p=NULL;` làm mất địa chỉ vùng cấp phát.

</details>

<details>
<summary><strong>Dangling / wild pointer</strong></summary>

**Là gì?** Dangling: trỏ tới đối tượng hết lifetime; wild: con trỏ chưa được khởi tạo hợp lệ.  
**Ví dụ:** `free(p); *p=1;` là lỗi use-after-free.

</details>

<details>
<summary><strong>Buffer overflow</strong></summary>

**Là gì?** Ghi/đọc vượt giới hạn mảng hoặc buffer.  
**Ví dụ:** `char a[4]; strcpy(a,"hello");` ghi tràn.

</details>

<details>
<summary><strong>struct / union / enum / typedef</strong></summary>

**Là gì?** `struct` gom các field; `union` chia sẻ vùng nhớ; `enum` là tập hằng số; `typedef` đặt bí danh kiểu.  
**Ví dụ:** `typedef struct {int id;} Device;`

</details>

<details>
<summary><strong>Padding / alignment</strong></summary>

**Là gì?** Compiler có thể chèn byte trống để đáp ứng căn chỉnh dữ liệu.  
**Ví dụ:** `struct {char c; int x;}` thường lớn hơn `sizeof(char)+sizeof(int)`.

</details>

<details>
<summary><strong>Bitwise / bit mask</strong></summary>

**Là gì?** Dùng `&`, `|`, `^`, `~`, `<<`, `>>` để thao tác bit.  
**Ví dụ:** `reg |= (1U<<5);` bật bit 5.

</details>

<details>
<summary><strong>Function pointer / callback</strong></summary>

**Là gì?** Biến lưu địa chỉ hàm; callback là hàm được truyền cho nơi khác để gọi khi cần.  
**Ví dụ:** `void (*cb)(void)=handler; cb();`

</details>

<details>
<summary><strong>Macro / preprocessor</strong></summary>

**Là gì?** Lệnh tiền xử lý để định nghĩa macro, include, điều kiện biên dịch.  
**Ví dụ:** `#define BIT(n) (1U << (n))`.

</details>

<details>
<summary><strong>String / memcpy / memmove</strong></summary>

**Là gì?** Chuỗi C kết thúc bằng `\0`; `memcpy` sao chép vùng không chồng lấp; `memmove` xử lý vùng chồng lấp.  
**Ví dụ:** `memmove(buf+1,buf,3);`

</details>

<details>
<summary><strong>Endianness</strong></summary>

**Là gì?** Thứ tự lưu byte của giá trị nhiều byte trong bộ nhớ.  
**Ví dụ:** `0x1234` có byte thấp `0x34` ở địa chỉ thấp trên little-endian.

</details>

<details>
<summary><strong>Undefined behavior</strong></summary>

**Là gì?** Hành vi không được chuẩn C bảo đảm, không có kết quả đáng tin.  
**Ví dụ:** `int a[2]; a[2]=1;` truy cập ngoài mảng.

</details>

<details>
<summary><strong>Reentrancy</strong></summary>

**Là gì?** Hàm có thể được gọi chồng lấp an toàn mà không làm sai trạng thái.  
**Ví dụ:** Hàm chỉ dùng biến local và không dùng shared mutable state thường dễ reentrant.

</details>

<details>
<summary><strong>Thread safety / atomic</strong></summary>

**Là gì?** Thread-safe: an toàn khi gọi đồng thời; atomic: thao tác không bị quan sát ở trạng thái dở dang theo mô hình bộ nhớ.  
**Ví dụ:** `atomic_fetch_add(&counter,1);` cho bộ đếm dùng chung.

</details>

</details>

---

<details>
<summary><strong>NHÓM 2. C++ PROGRAMMING</strong></summary>

<details>
<summary><strong>Class / Object</strong></summary>

**Là gì?** Class là bản thiết kế; object là thể hiện được tạo từ class.  
**Ví dụ:** `class Led {}; Led led;`

</details>

<details>
<summary><strong>struct vs class</strong></summary>

**Là gì?** C++ giống nhau về khả năng; khác quyền truy cập/kế thừa mặc định: `struct` public, `class` private.  
**Ví dụ:** `struct A {int x;};` → `x` public.

</details>

<details>
<summary><strong>Encapsulation</strong></summary>

**Là gì?** Encapsulation là đưa  data + method vào trong class để  bảo vệ dữ liệu

Có 3 phạm vi truy cập:

public → bên ngoài truy cập được

private → chỉ class và các thành phần được cấp quyền như friend

protected → class đó và derived class có thể truy cập

**Ví dụ:** `private: int speed; public: void setSpeed(int);`

</details>

<details>
<summary><strong>Inheritance</strong></summary>

**Là gì?** Class con kế thừa thuộc tính và phương thứ của class cha.  (biểu diễn mối quan hệ is-a.)
**Ví dụ:** `class Car : public Vehicle {};`

</details>

<details>
<summary><strong>Polymorphism</strong></summary>

**Là gì?** Cùng giao diện nhưng hành vi khác theo loại object; runtime qua hàm virtual.  
**Ví dụ:** `Base* p=&derived; p->run();`

</details>

<details>
<summary><strong>Abstraction</strong></summary>

**Là gì?** cung cấp những gì người dùng cần sử dụng, còn chi tiết bên trong thì ẩn đi.

Trong C++, abstraction thường có thể đạt được thông qua interface/abstract class

**Ví dụ:** `device.start();`

`Người dùng biết start() làm gì, không cần quan tâm nó làm như thế nào.`

Trong C++, abstraction thường được thể hiện thông qua:

Abstract class và pure virtual function

</details>

<details>
<summary><strong>Polymorphism(đa hình)</strong></summary>

**Là gì?** Cùng một interface nhưng có thể có nhiều cách thực thi khác nhau.

+ Static Polymorphism (đa hình tĩnh): thể hiện qua Overloading/Template , compiler  xác định hàm nào sẽ được gọi ngay tại compile time.


+ Dynamic Polymorphism (đa hình động): thể hiện qua Overriding ,hàm thực tế được gọi tại runtime, dựa vào object thực tế

Từ khóa  `override` dùng để xác nhận class con đang ghi đè một virtual function của class cha.

Từ khóa `virtual` để làm gi?

virtual cho phép gọi đúng hàm của class con qua pointer/reference của class cha. 

Nếu không có `virtual`?

khi gọi qua pointer/reference của class cha thì hàm của class cha sẽ được gọi


Virtual destructor để  destructor của class con được gọi , khi con trỏ của class cha xóa đối tượng class con

Nếu không có thì có khả năng à undefined behavior , và tài nguyên class con không được giải phóng đúng





</details>

<details>
<summary><strong>Overloading</strong></summary>

**Là gì?** Nhiều hàm cùng tên, khác danh sách tham số.  
**Ví dụ:** `print(int); print(float);`

</details>

<details>
<summary><strong>Overriding</strong></summary>

**Là gì?** Class con định nghĩa lại virtual function của class cha với chữ ký phù hợp.  
**Ví dụ:** `void run() override;`

</details>

<details>
<summary><strong>Virtual function</strong></summary>

**Là gì?** Cho phép gọi đúng hàm ở object dẫn xuất qua base pointer/reference.  
**Ví dụ:** `virtual void start();`

</details>

<details>
<summary><strong>Pure virtual / abstract</strong></summary>

**Là gì?** Hàm `=0` giúp tạo interface; class còn pure virtual thì abstract.  
**Ví dụ:** `virtual void draw()=0;`

</details>

<details>
<summary><strong>Virtual destructor</strong></summary>

**Là gì?** Cần khi xóa object dẫn xuất qua con trỏ base để giải phóng đầy đủ.  
**Ví dụ:** `virtual ~Base()=default;`

</details>

<details>
<summary><strong>Constructor / Destructor</strong></summary>

**Là gì?** Constructor khởi tạo object; destructor giải phóng tài nguyên khi lifetime kết thúc. 

**Ví dụ:** `Device(){} ~Device(){}`.

có 3 loai 
- Default Constructor :
    + Constructor có thể được gọi không cần truyền đối số.
- Parameterized Constructor:
    + Constructor nhận tham số để khởi tạo object.
- Copy Constructor:
    + Tạo object mới từ một object khác cùng kiểu.
- Move Constructor (c++11)
    + Nó dùng để khởi tạo object bằng cách chuyển tài nguyên từ object khác thay vì copy tài nguyên.

</details>

<details>
<summary><strong>Initialization list</strong></summary>

**Là gì?** Khởi tạo member trước thân constructor; bắt buộc cho reference/const member.  
**Ví dụ:** `Device(int x): id(x) {}`.

</details>

<details>
<summary><strong>Copy / Move</strong></summary>

**Là gì?** Copy tạo bản sao; move chuyển giao tài nguyên của object nguồn khi phù hợp.  
**Ví dụ:** `std::vector<int> b=std::move(a);`

</details>

<details>
<summary><strong>Rule of Three / Five / Zero</strong></summary>

**Là gì?** Khi tự quản lý tài nguyên, cân nhắc copy/move/destructor cùng nhau; ưu tiên Rule of Zero nhờ RAII.  
**Ví dụ:** Dùng `std::vector` thay raw `new[]`.

</details>

<details>
<summary><strong>Pointer vs reference</strong></summary>

**Là gì?** Pointer có thể null/đổi đích; reference phải được gắn với đối tượng khi khởi tạo.  
**Ví dụ:** `int *p=&x; int &r=x;`

</details>

<details>
<summary><strong>new / delete</strong></summary>

**Là gì?** Cấp phát/tạo object và hủy/giải phóng trong C++.  
**Ví dụ:** `auto p=new Device; delete p;`

</details>

<details>
<summary><strong>RAII</strong></summary>

**Là gì?** Gắn lifetime tài nguyên với lifetime của object.  
**Ví dụ:** `std::lock_guard<std::mutex> lock(m);`

</details>

<details>
<summary><strong>unique_ptr / shared_ptr / weak_ptr</strong></summary>

**Là gì?** Sở hữu duy nhất / cùng sở hữu / quan sát không tăng reference count.  
**Ví dụ:** `auto p=std::make_unique<Device>();`

</details>

<details>
<summary><strong>STL containers</strong></summary>

**Là gì?** `vector`: mảng động; `list`: linked list; `map`: cây có thứ tự; `unordered_map`: hash; `queue`: FIFO.  
**Ví dụ:** `std::vector<int> v{1,2};`

</details>

<details>
<summary><strong>Iterator / algorithm / lambda</strong></summary>

**Là gì?** Iterator duyệt container, algorithm xử lý dữ liệu, lambda định nghĩa hàm ngắn tại chỗ.  
**Ví dụ:** `std::sort(v.begin(),v.end());`

</details>

<details>
<summary><strong>Template</strong></summary>

**Là gì?** Viết hàm/class tổng quát theo kiểu dữ liệu.  
**Ví dụ:** `template<class T> T max2(T a,T b){return a>b?a:b;}`

</details>

<details>
<summary><strong>Exception</strong></summary>

**Là gì?** Cơ chế báo và xử lý lỗi qua `throw`, `try`, `catch`; cần chú ý policy trên embedded.  
**Ví dụ:** `try{f();}catch(const std::exception& e){}`.

</details>

<details>
<summary><strong>Modern C++</strong></summary>

**Là gì?** `auto` suy luận kiểu; `nullptr` null pointer; `enum class` enum mạnh kiểu; `constexpr` cho hằng tính được lúc biên dịch khi đủ điều kiện.  
**Ví dụ:** `constexpr int N=8;`

</details>

<details>
<summary><strong>std::thread / mutex / atomic</strong></summary>

**Là gì?** Tạo luồng / bảo vệ dữ liệu dùng chung / phép toán nguyên tử.  
**Ví dụ:** `std::atomic<int> count{0}; ++count;`

</details>

<details>
<summary><strong>condition_variable</strong></summary>

**Là gì?** Cho thread chờ tín hiệu và điều kiện, tránh polling liên tục.  
**Ví dụ:** `cv.wait(lock, []{return ready;});`

</details>

</details>

---

<details>
<summary><strong>NHÓM 3. EMBEDDED SYSTEMS</strong></summary>

<details>
<summary><strong>MCU vs MPU</strong></summary>

**Là gì?** MCU tích hợp CPU, RAM, Flash và ngoại vi; MPU thường cần bộ nhớ/ngoại vi ngoài, hay chạy OS phức tạp.  
**Ví dụ:** STM32 (MCU), Cortex-A SoC (MPU-based).

</details>

<details>
<summary><strong>Flash / RAM / ROM / EEPROM</strong></summary>

**Là gì?** Flash lưu firmware; RAM lưu dữ liệu khi chạy; ROM không ghi trong sử dụng bình thường; EEPROM lưu cấu hình không mất khi tắt nguồn.  
**Ví dụ:** Firmware trong Flash, biến runtime trong RAM.

</details>

<details>
<summary><strong>Memory-mapped I/O</strong></summary>

**Là gì?** Thanh ghi ngoại vi được ánh xạ vào địa chỉ bộ nhớ.  
**Ví dụ:** `*(volatile uint32_t*)REG_ADDR` truy cập register nếu địa chỉ hợp lệ.

</details>

<details>
<summary><strong>GPIO / register</strong></summary>

**Là gì?** GPIO là chân vào/ra; register chứa các bit điều khiển hoặc trạng thái.  
**Ví dụ:** `GPIO_OUT |= LED_BIT;`

</details>

<details>
<summary><strong>Polling</strong></summary>

**Là gì?** CPU liên tục kiểm tra cờ/trạng thái để biết sự kiện.  
**Ví dụ:** `while(!(STATUS&READY_BIT)) {}`.

</details>

<details>
<summary><strong>Interrupt / ISR</strong></summary>

**Là gì?** Ngoại vi phát sự kiện ngắt; ISR xử lý nhanh và thường báo task xử lý tiếp.  
**Ví dụ:** UART RX interrupt thông báo dữ liệu mới.

</details>

<details>
<summary><strong>DMA</strong></summary>

**Là gì?** Truyền dữ liệu bộ nhớ–ngoại vi với ít can thiệp CPU.  
**Ví dụ:** ADC DMA ghi mẫu vào buffer.

</details>

<details>
<summary><strong>Interrupt latency / priority</strong></summary>

**Là gì?** Latency là thời gian từ sự kiện đến khi ISR bắt đầu; priority quyết định thứ tự phục vụ theo controller.  
**Ví dụ:** Ngắt khẩn cấp ưu tiên cao.

</details>

<details>
<summary><strong>Timer / PWM</strong></summary>

**Là gì?** Timer đo hoặc tạo chu kỳ; PWM thay duty cycle để điều khiển công suất.  
**Ví dụ:** PWM 25% duty điều khiển độ sáng LED.

</details>

<details>
<summary><strong>ADC / DAC</strong></summary>

**Là gì?** ADC chuyển tín hiệu analog thành số; DAC chuyển số thành analog.  
**Ví dụ:** ADC đọc điện áp sensor.

</details>

<details>
<summary><strong>Watchdog</strong></summary>

**Là gì?** Bộ đếm thời gian yêu cầu software refresh; hết hạn sẽ kích hoạt phản ứng như reset theo cấu hình.  
**Ví dụ:** Task giám sát lỗi không kick watchdog.

</details>

<details>
<summary><strong>Bootloader / firmware</strong></summary>

**Là gì?** Bootloader chuẩn bị nạp/chạy hoặc cập nhật firmware; firmware là phần mềm trên thiết bị.  
**Ví dụ:** Bootloader nhận bản cập nhật qua UART.

</details>

<details>
<summary><strong>Startup code / linker script</strong></summary>

**Là gì?** Startup khởi tạo runtime; linker script quy định bố trí các section trong Flash/RAM.  
**Ví dụ:** Copy `.data`, zero `.bss` trước `main()`.

</details>

<details>
<summary><strong>Cross compiler / toolchain</strong></summary>

**Là gì?** Biên dịch trên host cho kiến trúc target; toolchain gồm compiler, linker, debugger...  
**Ví dụ:** `arm-none-eabi-gcc` cho ARM bare-metal.

</details>

<details>
<summary><strong>JTAG / SWD</strong></summary>

**Là gì?** Giao diện nạp và debug phần cứng.  
**Ví dụ:** SWD + ST-Link debug STM32.

</details>

<details>
<summary><strong>Bare-metal / RTOS / Embedded Linux</strong></summary>

**Là gì?** Bare-metal không có OS; RTOS lập lịch có yêu cầu thời gian; Embedded Linux giàu dịch vụ hệ thống.  
**Ví dụ:** STM32 bare-metal, FreeRTOS, OpenWrt.

</details>

<details>
<summary><strong>Sensor / actuator</strong></summary>

**Là gì?** Sensor đo đại lượng; actuator tác động vật lý.  
**Ví dụ:** Cảm biến nhiệt độ và motor.

</details>

<details>
<summary><strong>Driver / HAL</strong></summary>

**Là gì?** Driver điều khiển ngoại vi; HAL cung cấp API trừu tượng hóa phần cứng.  
**Ví dụ:** `HAL_UART_Transmit(...)`.

</details>

<details>
<summary><strong>Timeout / retry / CRC</strong></summary>

**Là gì?** Timeout giới hạn chờ; retry thử lại; CRC phát hiện lỗi dữ liệu.  
**Ví dụ:** Gói UART sai CRC bị loại bỏ.

</details>

<details>
<summary><strong>Hardware debug</strong></summary>

**Là gì?** Kết hợp register view, breakpoint, UART log, logic analyzer, oscilloscope.  
**Ví dụ:** Logic analyzer kiểm tra xung SPI.

</details>

</details>

---

<details>
<summary><strong>NHÓM 4. RTOS</strong></summary>

<details>
<summary><strong>RTOS</strong></summary>

**Là gì?** Hệ điều hành giúp xử lý tác vụ với thời gian đáp ứng có thể dự đoán theo thiết kế.  
**Ví dụ:** FreeRTOS cho MCU.

</details>

<details>
<summary><strong>G​​POS vs RTOS</strong></summary>

**Là gì?** GP​OS tối ưu khả năng sử dụng/throughput đa mục đích; RTOS tập trung deadline và độ trễ xác định.  
**Ví dụ:** Windows desktop vs FreeRTOS.

</details>

<details>
<summary><strong>Hard / Soft real-time</strong></summary>

**Là gì?** Hard: trễ deadline là lỗi nghiêm trọng; soft: trễ làm giảm chất lượng.  
**Ví dụ:** Điều khiển bảo vệ an toàn vs cập nhật số liệu giao diện.

</details>

<details>
<summary><strong>Task / thread</strong></summary>

**Là gì?** Đơn vị thực thi được scheduler quản lý.  
**Ví dụ:** Task đọc ADC và task truyền UART.

</details>

<details>
<summary><strong>Task states</strong></summary>

**Là gì?** Các trạng thái thường gặp: Running, Ready, Blocked, Suspended.  
**Ví dụ:** Task chờ queue → Blocked.

</details>

<details>
<summary><strong>Preemptive / cooperative</strong></summary>

**Là gì?** Preemptive cho phép task ưu tiên cao giành CPU; cooperative nhường CPU tự nguyện.  
**Ví dụ:** Task khẩn cấp preempt task nền.

</details>

<details>
<summary><strong>Priority / time slicing</strong></summary>

**Là gì?** Priority xác định mức ưu tiên; time slicing chia CPU giữa các task cùng ưu tiên khi được cấu hình.  
**Ví dụ:** Hai task cùng mức chạy luân phiên.

</details>

<details>
<summary><strong>Context switching</strong></summary>

**Là gì?** Lưu trạng thái task cũ và khôi phục task mới.  
**Ví dụ:** ISR đánh thức task ưu tiên cao.

</details>

<details>
<summary><strong>Tick / tickless</strong></summary>

**Là gì?** Tick là nhịp thời gian hệ thống; tickless giảm tick khi idle để tiết kiệm năng lượng.  
**Ví dụ:** `vTaskDelay(pdMS_TO_TICKS(10))`.

</details>

<details>
<summary><strong>Mutex</strong></summary>

**Là gì?** Khóa bảo vệ tài nguyên dùng chung, thường có ownership và priority inheritance.  
**Ví dụ:** Hai task dùng chung I2C bus.

</details>

<details>
<summary><strong>Binary / counting semaphore</strong></summary>

**Là gì?** Binary semaphore dùng tín hiệu/đồng bộ; counting semaphore đếm nhiều sự kiện/tài nguyên.  
**Ví dụ:** ISR give semaphore để đánh thức task.

</details>

<details>
<summary><strong>Queue</strong></summary>

**Là gì?** Truyền dữ liệu an toàn giữa các task bằng message.  
**Ví dụ:** Task ADC gửi mẫu sang task xử lý.

</details>

<details>
<summary><strong>Event group / task notification</strong></summary>

**Là gì?** Cơ chế thông báo một hoặc nhiều sự kiện tới task.  
**Ví dụ:** Thông báo `DATA_READY`.

</details>

<details>
<summary><strong>Race condition / deadlock / starvation</strong></summary>

**Là gì?** Race: kết quả phụ thuộc timing; deadlock: chờ nhau vĩnh viễn; starvation: task thiếu cơ hội chạy.  
**Ví dụ:** A giữ mutex1 chờ mutex2, B ngược lại.

</details>

<details>
<summary><strong>Priority inversion / inheritance</strong></summary>

**Là gì?** Task ưu tiên thấp giữ khóa khiến task cao phải chờ; inheritance nâng tạm priority của chủ khóa.  
**Ví dụ:** Low giữ mutex, High đợi.

</details>

<details>
<summary><strong>ISR-to-task</strong></summary>

**Là gì?** ISR nên làm ít việc, báo task thực hiện xử lý nặng.  
**Ví dụ:** `xQueueSendFromISR(...)`.

</details>

<details>
<summary><strong>Critical section</strong></summary>

**Là gì?** Vùng cần bảo vệ khỏi truy cập đồng thời; trên MCU có thể dùng khóa hoặc che ngắt thích hợp.  
**Ví dụ:** Bảo vệ cập nhật thanh ghi dùng chung.

</details>

<details>
<summary><strong>Task stack / heap fragmentation</strong></summary>

**Là gì?** Mỗi task có stack; cấp phát heap động có thể phân mảnh nếu dùng không cẩn thận.  
**Ví dụ:** Theo dõi high water mark của stack.

</details>

<details>
<summary><strong>Deadline / WCET</strong></summary>

**Là gì?** Deadline là hạn hoàn tất; WCET là thời gian chạy tối đa trong điều kiện phân tích xác định.  
**Ví dụ:** Task sensor cần hoàn thành trong 5 ms.

</details>

<details>
<summary><strong>Watchdog / monitoring</strong></summary>

**Là gì?** Giám sát sức khỏe task và phục hồi khi hệ thống không còn đáp ứng.  
**Ví dụ:** Task giám sát chỉ kick watchdog nếu các task quan trọng khỏe.

</details>

<details>
<summary><strong>FreeRTOS API</strong></summary>

**Là gì?** API tạo task, delay, queue, semaphore...  
**Ví dụ:** `xTaskCreate`, `vTaskDelay`, `xQueueSend`.

</details>

<details>
<summary><strong>Thiết kế task</strong></summary>

**Là gì?** Tách thu nhận sensor, xử lý, giao tiếp, UI; chọn priority và IPC dựa vào yêu cầu thời gian.  
**Ví dụ:** ADC ISR → queue → processing task.

</details>

</details>

---

<details>
<summary><strong>NHÓM 5. QT FRAMEWORK / GUI</strong></summary>

<details>
<summary><strong>Qt</strong></summary>

**Là gì?** Framework C++ đa nền tảng để xây dựng GUI và ứng dụng.  
**Ví dụ:** GUI thiết bị y tế bằng Qt.

</details>

<details>
<summary><strong>Qt Widgets vs Qt Quick/QML</strong></summary>

**Là gì?** Widgets dùng các widget C++; Quick/QML dùng giao diện khai báo, phù hợp animation/touch UI.  
**Ví dụ:** `QPushButton` vs QML `Button`.

</details>

<details>
<summary><strong>QObject / QWidget / QMainWindow</strong></summary>

**Là gì?** `QObject` hỗ trợ object model; `QWidget` là thành phần UI; `QMainWindow` là cửa sổ chính.  
**Ví dụ:** `class MainWindow : public QMainWindow`.

</details>

<details>
<summary><strong>Signals / Slots</strong></summary>

**Là gì?** Cơ chế phát sự kiện và kết nối hàm xử lý giữa các object.  
**Ví dụ:** `connect(button,&QPushButton::clicked,this,&MainWindow::onClick);`

</details>

<details>
<summary><strong>Event loop</strong></summary>

**Là gì?** Vòng lặp nhận và phân phối sự kiện; xử lý block dài sẽ làm UI kém phản hồi.  
**Ví dụ:** Click button → slot được gọi.

</details>

<details>
<summary><strong>QThread / worker object</strong></summary>

**Là gì?** Chạy tác vụ dài ở thread khác; pattern worker dùng QObject chuyển sang QThread.  
**Ví dụ:** Worker đọc sensor ở background.

</details>

<details>
<summary><strong>Thread affinity</strong></summary>

**Là gì?** QObject thuộc một thread; event queued được xử lý trong thread có event loop của object nhận.  
**Ví dụ:** Worker `moveToThread(&thread)`.

</details>

<details>
<summary><strong>Queued connection</strong></summary>

**Là gì?** Signal được chuyển thành event tới receiver thread, tránh gọi trực tiếp cross-thread ngoài ý muốn.  
**Ví dụ:** Worker emit `dataReady(value)` tới UI.

</details>

<details>
<summary><strong>Update UI an toàn</strong></summary>

**Là gì?** Hầu hết widget phải thao tác trong GUI thread.  
**Ví dụ:** Worker emit signal, UI slot `label->setText(...)`.

</details>

<details>
<summary><strong>QTimer</strong></summary>

**Là gì?** Phát tín hiệu timeout sau khoảng thời gian, cần event loop hoạt động.  
**Ví dụ:** Refresh UI mỗi 100 ms.

</details>

<details>
<summary><strong>QString / QByteArray / QList</strong></summary>

**Là gì?** Chuỗi Unicode / mảng byte / container danh sách trong Qt.  
**Ví dụ:** `QByteArray frame = port.readAll();`

</details>

<details>
<summary><strong>Parent-child ownership</strong></summary>

**Là gì?** Parent QObject thường tự hủy child QObject cùng lúc bị hủy.  
**Ví dụ:** `new QPushButton("OK", this)`.

</details>

<details>
<summary><strong>Qt Designer / layout</strong></summary>

**Là gì?** Designer dựng giao diện; layout sắp xếp widget co giãn.  
**Ví dụ:** `QVBoxLayout`.

</details>

<details>
<summary><strong>QSerialPort / QTcpSocket</strong></summary>

**Là gì?** API Qt cho UART serial và TCP networking.  
**Ví dụ:** `port.setBaudRate(115200);`

</details>

<details>
<summary><strong>Model/View</strong></summary>

**Là gì?** Tách dữ liệu model khỏi cách hiển thị view.  
**Ví dụ:** `QTableView` với `QAbstractTableModel`.

</details>

<details>
<summary><strong>Qt vs MFC</strong></summary>

**Là gì?** Cả hai event-driven; MFC thường dựa Windows message map, Qt dùng signals/slots và đa nền tảng.  
**Ví dụ:** `ON_BN_CLICKED` so với `connect`.

</details>

<details>
<summary><strong>Blocking vs non-blocking</strong></summary>

**Là gì?** Blocking ở GUI thread làm treo hiển thị; non-blocking/asynchronous giữ UI phản hồi.  
**Ví dụ:** Dùng worker thay vì sleep trong slot.

</details>

<details>
<summary><strong>Hiển thị sensor / alarm</strong></summary>

**Là gì?** Tách dữ liệu, kiểm tra validity/timestamp, hiển thị cảnh báo rõ ràng theo yêu cầu an toàn.  
**Ví dụ:** Mất sensor → hiển thị trạng thái invalid.

</details>

<details>
<summary><strong>UI lag / freeze</strong></summary>

**Là gì?** Kiểm tra blocking operation, paint quá dày, lock contention; giới hạn tần số refresh.  
**Ví dụ:** Sensor 1 kHz nhưng UI refresh 20 Hz.

</details>

</details>

---

<details>
<summary><strong>NHÓM 6. COMMUNICATION PROTOCOLS</strong></summary>

<details>
<summary><strong>UART</strong></summary>

**Là gì?** Giao tiếp nối tiếp bất đồng bộ, thường qua TX/RX, baud rate, parity, stop bit.  
**Ví dụ:** MCU gửi log UART 115200.

</details>

<details>
<summary><strong>SPI</strong></summary>


# SPI (Serial Peripheral Interface)

- Là giao tiếp **nối tiếp, đồng bộ, Full-Duplex**.
- Thường sử dụng **4 dây**:
  - **SCLK/SCK:** Clock do Master tạo.
  - **MOSI:** Master → Slave.
  - **MISO:** Slave → Master.
  - **CS/SS:** Master chọn Slave.

## 1. Tốc độ

- Tốc độ phổ biến: **1 Mbps, 10 Mbps, 20 Mbps, 50 Mbps** hoặc cao hơn.

## 2. Khung truyền

SPI **không có khung truyền cố định** như I2C.

```text
CS = 0 → [DATA] → CS = 1
```

- **CS = 0:** Chọn Slave, bắt đầu giao tiếp.
- Truyền/nhận Data theo xung **SCLK**.
- **CS = 1:** Bỏ chọn Slave, kết thúc giao tiếp.
- Data thường là **8 bit hoặc 16 bit**, tùy thiết bị/cấu hình.

## 3. Full-Duplex

SPI có 2 đường dữ liệu riêng:

```text
MOSI: Master → Slave
MISO: Master ← Slave
```

→ Có thể **truyền và nhận đồng thời**.

## 4. Nhiều Slave

Thông thường mỗi Slave có một chân CS riêng:

```text
Master
 ├── CS1 → Slave 1
 ├── CS2 → Slave 2
 └── CS3 → Slave 3
```

mẹo nếu 1 M- 1 S, thì số dây là 4 , nếu N slayer thì số dây là 3+N
nếu quá nhiều dây thì dùng decoder (74HC138) từ 3 sang 8

## 5. SPI Mode

Có **4 Mode** dựa trên **CPOL và CPHA**.

- **CPOL (Clock Polarity):** Xác định mức logic của SCLK khi không hoạt động.
- **CPHA (Clock Phase):** Xác định thời điểm lấy mẫu dữ liệu theo cạnh clock.

Mục đích là xác định trạng thái clock và thời điểm lấy mẫu dữ liệu.

Master và Slave phải cấu hình SPI Mode tương thích để truyền/nhận chính xác.

## 6. Điểm cần nhớ

- Không có **Address** như I2C.
- Không có **ACK/NACK bắt buộc** như I2C.
- Không cần **Pull-up** trên các đường SPI theo kiểu bắt buộc như I2C.
- Chọn Slave bằng **CS**.
- Hỗ trợ **Full-Duplex** nhờ MOSI và MISO.

## 7. Trả lời phỏng vấn

> SPI là giao thức truyền thông nối tiếp, đồng bộ và Full-Duplex. Nó thường sử dụng 4 dây SCLK, MOSI, MISO và CS. Master sử dụng CS để chọn Slave và tạo SCLK để đồng bộ truyền dữ liệu. SPI không sử dụng Address và ACK/NACK bắt buộc như I2C.

</details>

<details>
<summary><strong>I2C</strong></summary>


# I2C (Inter-Integrated Circuit)

- I2C là giao thức truyền thông **đồng bộ, bán song công (Half-Duplex), nối tiếp (Serial)**.
- Sử dụng 2 dây:
  - **SDA (Serial Data):** Truyền dữ liệu.
  - **SCL (Serial Clock):** Truyền xung clock.

## 1. Tốc độ truyền I2C

| Chế độ | Tốc độ |
|---|---|
| Standard Mode | 100 kbps |
| Fast Mode | 400 kbps |
| Fast Mode Plus | 1 Mbps |
| High-Speed Mode | 3.4 Mbps |

- Đơn vị tốc độ: **bps (bit/giây)**.
- Tần số clock SCL thường được biểu diễn bằng **Hz, kHz hoặc MHz**.

## 2. Khung truyền I2C

```text
START → [7-bit Address + 1-bit R/W] → ACK → [8-bit Data] → ACK → STOP
                 8 bit                1 bit      8 bit       1 bit
```

### START và STOP

- **START:** SDA chuyển từ `1 → 0` khi SCL = 1.
- **STOP:** SDA chuyển từ `0 → 1` khi SCL = 1.

### Address

- Thường sử dụng địa chỉ **7 bit**, ngoài ra còn có chế độ **10 bit**.
- Mỗi Slave trên cùng bus cần có địa chỉ riêng để Master phân biệt.
- Với địa chỉ 7 bit, không phải tất cả 128 giá trị đều dùng được cho Slave thông thường vì có các địa chỉ dành riêng.

### R/W (Read/Write)

Gồm **1 bit**:

- `0`: Write – Master ghi dữ liệu.
- `1`: Read – Master đọc dữ liệu.

### ACK/NACK

Gồm **1 bit**:

- **ACK (0):** Thiết bị xác nhận đã nhận byte.
- **NACK (1):** Thiết bị không xác nhận hoặc không muốn nhận tiếp.

Trong khung ghi dữ liệu từ Master đến Slave:

- **ACK thứ 1:** Slave xác nhận địa chỉ phù hợp và sẵn sàng giao tiếp.
- **ACK thứ 2:** Slave xác nhận đã nhận byte Data.

**Lưu ý:** Khi Master đọc dữ liệu từ Slave, Master sẽ gửi ACK/NACK sau mỗi byte dữ liệu nhận được.

### Data

- Mỗi lần truyền **8 bit = 1 byte**.
- Có thể truyền nhiều byte liên tiếp.
- Sau mỗi byte sẽ có thêm 1 bit ACK/NACK.

## 3. Vì sao I2C cần điện trở kéo lên (Pull-up)?

- SDA và SCL sử dụng cấu trúc **Open-Drain**.
- Thiết bị chỉ chủ động kéo đường tín hiệu xuống mức `0`.
- Khi muốn tín hiệu lên mức `1`, thiết bị nhả đường tín hiệu và điện trở **Pull-up** kéo tín hiệu lên mức cao.

**Kết luận:**

- Mức `0`: Thiết bị kéo xuống GND.
- Mức `1`: Điện trở Pull-up kéo lên VCC.
- Pull-up giúp nhiều thiết bị có thể dùng chung bus I2C mà không xảy ra xung đột khi một thiết bị kéo xuống mức thấp.

</details>

<details>
<summary><strong>Ethernet</strong></summary>


# Ethernet (IEEE 802.3)

- Ethernet là công nghệ truyền thông mạng có dây, chủ yếu hoạt động ở **Layer 1 (Physical)** và **Layer 2 (Data Link)** trong mô hình OSI.
- Sử dụng **MAC Address** để xác định thiết bị nguồn và đích trong mạng LAN.
- MAC Address là địa chỉ vật lý (logic Layer 2) của giao diện mạng, thường dài 48 bit, được sử dụng để xác định nguồn và đích khi truyền Ethernet Frame.
- IP Address: Địa chỉ logic, dùng để gửi dữ liệu giữa các mạng.
- Dữ liệu được truyền dưới dạng **Ethernet Frame**.
- Ethernet hiện đại thường hoạt động ở chế độ **Full-Duplex**.
- Ứng dụng: **LAN, Embedded Linux, Automotive Ethernet, ECU, IoT**.


## 1. Tốc độ Ethernet

| Loại | Tốc độ |
|---|---|
| Ethernet | 10 Mbps |
| Fast Ethernet | 100 Mbps |
| Gigabit Ethernet | 1 Gbps |
| 10 Gigabit Ethernet | 10 Gbps |

- Đơn vị tốc độ: **Mbps, Gbps**.

## 2. Ethernet Frame

```text
[Destination MAC] → [Source MAC] → [Type/Length] → [Data] → [FCS]
      6 byte           6 byte         2 byte       46–1500     4 byte
```

- **Destination MAC:** Địa chỉ MAC của thiết bị nhận.
- **Source MAC:** Địa chỉ MAC của thiết bị gửi.
- **Type/Length:** Xác định loại giao thức phía trên hoặc độ dài dữ liệu.
- **Data:** Dữ liệu cần truyền.
- **FCS:** Kiểm tra lỗi bằng CRC-32.

## 3. MAC Address và IP Address

- **MAC Address:** Địa chỉ ở Layer 2, thường dài 48 bit (6 byte).
- **IP Address:** Địa chỉ ở Layer 3, dùng để định tuyến dữ liệu giữa các mạng.
- **ARP:** Dùng để tìm MAC Address tương ứng với địa chỉ IPv4 trong mạng cục bộ.

## 4. MAC và PHY trong Embedded

- **MAC (Media Access Control):** Xử lý Ethernet Frame, địa chỉ MAC, truyền và nhận dữ liệu.
- **PHY (Physical Layer):** Chuyển đổi dữ liệu giữa MAC và tín hiệu vật lý trên đường truyền.
- **MII/RMII/RGMII:** Các giao tiếp phổ biến giữa MAC và PHY.

## 5. Điểm cần nhớ

- Ethernet sử dụng **MAC Address**, không sử dụng địa chỉ Slave như I2C.
- Ethernet Frame có **FCS (CRC-32)** để phát hiện lỗi.
- Ethernet có thể truyền dữ liệu qua Switch.
- Ethernet là công nghệ Layer 1 và Layer 2; **TCP/IP** là các giao thức được sử dụng phía trên Ethernet.
- Ethernet không quy định tốc độ truyền cố định mà có nhiều chuẩn tốc độ khác nhau.

## 6. Trả lời phỏng vấn

> Ethernet là công nghệ truyền thông mạng có dây, chủ yếu hoạt động ở Layer 1 và Layer 2 của mô hình OSI. Ethernet truyền dữ liệu dưới dạng Frame, sử dụng MAC Address để xác định nguồn và đích. Trong Embedded, Ethernet thường gồm MAC Controller(MAC = Xử lý dữ liệu.) và PHY(Truyền/nhận tín hiệu vật lý.), hỗ trợ các tốc độ như 10 Mbps, 100 Mbps và 1 Gbps.



</details>

<details>
<summary><strong>TCP/IP</strong></summary>

TCP/IP là bộ giao thức mạng (Protocol Suite), bao gồm nhiều giao thức như TCP, UDP, IP, HTTP, MQTT...

IP: Định địa chỉ và định tuyến dữ liệu đến thiết bị đích.

TCP: Truyền dữ liệu tin cậy, đúng thứ tự.TCP phải thiết lập kết nối trước khi truyền dữ liệu.Ví dụ: Client gửi dữ liệu đến Server.

UDP: Truyền dữ liệu nhanh, không đảm bảo dữ liệu đến nơi.UDP không cần thiết lập kết nối trước khi gửi dữ liệu.

TCP và UDP là hai giao thức cùng thuộc tầng Transport.

</details>

<details>
<summary><strong>TCP vs UDP</strong></summary>

**Là gì?** TCP có kết nối, thứ tự và retransmission; UDP là datagram không tự bảo đảm giao hàng.  
**Ví dụ:** TCP cho cấu hình; UDP cho streaming khi phù hợp.

</details>

<details>
<summary><strong>Socket programming</strong></summary>

**Là gì?** API tạo endpoint để gửi/nhận dữ liệu TCP/UDP.  
**Ví dụ:** `socket()`, `connect()`, `send()`.

</details>

<details>
<summary><strong>USB</strong></summary>

> USB là chuẩn giao tiếp nối tiếp, hỗ trợ truyền dữ liệu và cấp nguồn, hoạt động theo mô hình Host – Device. USB 2.0 thường sử dụng 4 dây gồm VBUS, D+, D- và GND. Khi thiết bị được kết nối, Host thực hiện Enumeration để nhận diện và cấu hình thiết bị. USB hỗ trợ 4 kiểu truyền dữ liệu là Control, Bulk, Interrupt và Isochronous.- USB 2.0 High-Speed đạt tối đa **480 Mbps**, 3.0 thì 5Gbs

Các kiểu truyền dữ liệu USB

| Kiểu truyền | Ứng dụng |
|---|---|
| Control | Cấu hình và điều khiển thiết bị |
| Bulk | USB Flash, truyền file |
| Interrupt | Bàn phím, chuột |
| Isochronous | Audio, video thời gian thực |

</details>

<details>
<summary><strong>Bluetooth / BLE</strong></summary>

> Bluetooth là công nghệ truyền thông không dây khoảng cách ngắn, hoạt động ở băng tần 2.4 GHz. Bluetooth có hai loại phổ biến là Classic và BLE. Classic thường dùng cho âm thanh và truyền dữ liệu liên tục, còn BLE được tối ưu để tiết kiệm năng lượng, phù hợp với cảm biến và thiết bị IoT.  tốc đố tầm 2 tới 3Mbps

</details>

<details>
<summary><strong>CAN</strong></summary>

**Là gì?** Bus truyền thông ưu tiên theo identifier; bit dominant thắng recessive khi arbitration.  
**Ví dụ:** ECU gửi CAN frame ID 0x100.

</details>

<details>
<summary><strong>CRC / checksum</strong></summary>

**Là gì?** Giá trị kiểm tra phát hiện lỗi truyền; CRC dựa phép toán đa thức.  
**Ví dụ:** Receiver tính lại CRC và so sánh.

</details>

<details>
<summary><strong>Ring buffer</strong></summary>

**Là gì?** Buffer vòng cho producer/consumer, thường dùng UART RX.  
**Ví dụ:** ISR ghi byte, task đọc byte.

</details>

<details>
<summary><strong>Packet parsing</strong></summary>

**Là gì?** Xác định frame, độ dài, payload và kiểm tra dữ liệu.  
**Ví dụ:** `[SOF][LEN][DATA][CRC]`.

</details>

<details>
<summary><strong>Timeout / retry / reconnect</strong></summary>

**Là gì?** Giới hạn thời gian chờ, thử lại có kiểm soát và thiết lập lại kết nối.  
**Ví dụ:** TCP mất kết nối → backoff rồi reconnect.

</details>

<details>
<summary><strong>Sensor disconnect</strong></summary>

**Là gì?** Phát hiện mất tín hiệu và đánh dấu dữ liệu invalid, không coi dữ liệu cũ là dữ liệu hiện tại.  
**Ví dụ:** Không nhận gói trong 2 s → báo lỗi.

</details>

</details>

---

<details>
<summary><strong>NHÓM 7. MULTITHREADING & CONCURRENCY</strong></summary>

<details>
<summary><strong>Process vs Thread</strong></summary>

**Là gì?** Process có address space riêng; thread cùng process chia sẻ address space.  
**Ví dụ:** Hai threads cùng sửa biến toàn cục.

</details>

<details>
<summary><strong>Thread lifecycle</strong></summary>

**Là gì?** Tạo, chạy, chờ, kết thúc; tài nguyên thread cần được quản lý.  
**Ví dụ:** `std::thread t(f); t.join();`

</details>

<details>
<summary><strong>Race condition</strong></summary>

**Là gì?** Kết quả phụ thuộc timing/interleaving, có thể sai do thiếu đồng bộ.  
**Ví dụ:** Hai thread cùng tăng counter.

</details>

<details>
<summary><strong>Data race</strong></summary>

**Là gì?** Truy cập đồng thời cùng memory, ít nhất một ghi, thiếu synchronization theo C++; là undefined behavior.  
**Ví dụ:** Hai thread viết `int x` thường.

</details>

<details>
<summary><strong>Mutex</strong></summary>

**Là gì?** Khóa dùng bảo vệ shared state.  
**Ví dụ:** `std::lock_guard<std::mutex> g(m);`

</details>

<details>
<summary><strong>Semaphore</strong></summary>

**Là gì?** Bộ đếm/tín hiệu phối hợp các thread hoặc task.  
**Ví dụ:** Producer signal khi có data.

</details>

<details>
<summary><strong>Deadlock / livelock / starvation</strong></summary>

**Là gì?** Chờ khóa vĩnh viễn / liên tục đổi trạng thái nhưng không tiến triển / bị bỏ đói CPU hoặc tài nguyên.  
**Ví dụ:** Hai thread khóa mutex theo thứ tự ngược.

</details>

<details>
<summary><strong>Condition variable</strong></summary>

**Là gì?** Cho thread ngủ cho tới khi điều kiện đúng và được thông báo.  
**Ví dụ:** `cv.wait(lock, pred);`

</details>

<details>
<summary><strong>Producer-consumer</strong></summary>

**Là gì?** Một thread tạo dữ liệu, thread khác lấy ra từ queue an toàn.  
**Ví dụ:** Sensor producer → logger consumer.

</details>

<details>
<summary><strong>Atomic vs volatile</strong></summary>

**Là gì?** Atomic hỗ trợ đồng bộ truy cập; volatile không thay thế mutex/atomic.  
**Ví dụ:** `std::atomic<bool> stop{false};`

</details>

<details>
<summary><strong>Critical section</strong></summary>

**Là gì?** Khối code cần bảo vệ khi truy cập tài nguyên chung.  
**Ví dụ:** Cập nhật danh sách dùng chung.

</details>

<details>
<summary><strong>Thread-safe queue</strong></summary>

**Là gì?** Queue có cơ chế đồng bộ an toàn giữa nhiều thread.  
**Ví dụ:** Mutex + condition_variable + queue.

</details>

<details>
<summary><strong>UI thread vs worker</strong></summary>

**Là gì?** UI thread xử lý event, worker thực hiện công việc lâu.  
**Ví dụ:** Download ở worker, báo UI bằng signal.

</details>

<details>
<summary><strong>Cancel mechanism</strong></summary>

**Là gì?** Worker kiểm tra cờ dừng và kết thúc an toàn tại điểm phù hợp.  
**Ví dụ:** `if (stopRequested) break;`

</details>

<details>
<summary><strong>Shared state / lifetime</strong></summary>

**Là gì?** Dữ liệu chung cần đồng bộ và quản lý lifetime để tránh use-after-free.  
**Ví dụ:** Không capture reference của object đã hủy.

</details>

<details>
<summary><strong>Debug intermittent bug</strong></summary>

**Là gì?** Ghi log có timestamp/thread ID, kiểm tra lock order và truy cập chung.  
**Ví dụ:** Bug chỉ xảy ra khi hai thao tác gần đồng thời.

</details>

</details>

---

<details>
<summary><strong>NHÓM 8. MEDICAL DEVICE SOFTWARE & SAFETY</strong></summary>

<details>
<summary><strong>IEC 62304</strong></summary>

**Là gì?** Tiêu chuẩn quy trình vòng đời phát triển/bảo trì software thiết bị y tế.  
**Ví dụ:** Requirements → design → verification → maintenance.

</details>

<details>
<summary><strong>Safety class A/B/C</strong></summary>

**Là gì?** Phân loại mức an toàn phần mềm theo khả năng góp phần gây hại; yêu cầu quy trình tăng theo rủi ro.  
**Ví dụ:** Class C áp dụng yêu cầu nghiêm ngặt hơn A.

</details>

<details>
<summary><strong>Software lifecycle</strong></summary>

**Là gì?** Từ requirements, architecture, design, coding, test đến maintenance.  
**Ví dụ:** Quản lý thay đổi có impact analysis.

</details>

<details>
<summary><strong>Verification vs Validation</strong></summary>

**Là gì?** Verification: xây đúng theo đặc tả? Validation: sản phẩm đáp ứng nhu cầu sử dụng dự định?  
**Ví dụ:** Test đúng requirement vs đánh giá use case lâm sàng.

</details>

<details>
<summary><strong>Requirement traceability</strong></summary>

**Là gì?** Liên kết requirement với thiết kế, code và test để kiểm chứng mức bao phủ.  
**Ví dụ:** REQ-10 → TC-10.

</details>

<details>
<summary><strong>ISO 14971</strong></summary>

**Là gì?** Tiêu chuẩn quản lý rủi ro của thiết bị y tế.  
**Ví dụ:** Xác định hazard, đánh giá và kiểm soát risk.

</details>

<details>
<summary><strong>IEC 60601</strong></summary>

**Là gì?** Họ tiêu chuẩn an toàn cơ bản và hiệu năng thiết yếu cho thiết bị điện y tế.  
**Ví dụ:** Xem xét safety/electrical design.

</details>

<details>
<summary><strong>IEC 62366-1</strong></summary>

**Là gì?** Tiêu chuẩn quy trình usability engineering cho thiết bị y tế.  
**Ví dụ:** UI hạn chế nhầm thao tác khi cấp cứu.

</details>

<details>
<summary><strong>Risk management</strong></summary>

**Là gì?** Nhận diện nguy cơ, ước lượng/đánh giá rủi ro, kiểm soát và theo dõi.  
**Ví dụ:** Sensor sai có thể tạo alarm sai.

</details>

<details>
<summary><strong>Fail-safe / recovery</strong></summary>

**Là gì?** Khi phát hiện lỗi, chuyển sang trạng thái giảm rủi ro theo thiết kế.  
**Ví dụ:** Không sử dụng số đo invalid để điều khiển.

</details>

<details>
<summary><strong>Sensor failure handling</strong></summary>

**Là gì?** Kiểm tra kết nối, timestamp, giá trị và lỗi; cảnh báo theo risk control.  
**Ví dụ:** Mất tín hiệu SpO₂ → báo sensor disconnected.

</details>

<details>
<summary><strong>Alarm handling</strong></summary>

**Là gì?** Phân loại, ưu tiên và hiển thị cảnh báo phù hợp; tránh mất alarm quan trọng.  
**Ví dụ:** Alarm không phụ thuộc hoàn toàn vào UI animation.

</details>

<details>
<summary><strong>Patient data integrity</strong></summary>

**Là gì?** Bảo đảm dữ liệu đúng, nhất quán và không gắn nhầm bệnh nhân.  
**Ví dụ:** Kiểm tra Patient ID khi lưu kết quả.

</details>

<details>
<summary><strong>Testing levels</strong></summary>

**Là gì?** Unit kiểm hàm/module; integration kiểm giao tiếp; system kiểm toàn hệ thống; regression kiểm không tái phát.  
**Ví dụ:** Sửa bug → chạy test liên quan.

</details>

<details>
<summary><strong>Impact analysis</strong></summary>

**Là gì?** Đánh giá sửa đổi ảnh hưởng thành phần, yêu cầu, rủi ro và test nào.  
**Ví dụ:** Thay đổi xử lý sensor phải kiểm tra alarm.

</details>

</details>

---

<details>
<summary><strong>NHÓM 9. DEBUGGING, TESTING & DESIGN</strong></summary>

<details>
<summary><strong>Unit / Integration / System test</strong></summary>

**Là gì?** Kiểm một module / các module kết hợp / toàn hệ thống.  
**Ví dụ:** Test parser → parser + UART → thiết bị.

</details>

<details>
<summary><strong>Regression test</strong></summary>

**Là gì?** Chạy lại test để phát hiện lỗi do sửa đổi.  
**Ví dụ:** Sửa nút Cancel rồi test Search.

</details>

<details>
<summary><strong>Test case</strong></summary>

**Là gì?** Gồm điều kiện đầu, input, bước thực hiện và expected result.  
**Ví dụ:** Input timeout → expected error.

</details>

<details>
<summary><strong>Boundary / negative test</strong></summary>

**Là gì?** Kiểm ngưỡng biên và input lỗi.  
**Ví dụ:** Buffer size 0, 1, MAX, MAX+1.

</details>

<details>
<summary><strong>Mock / stub / harness</strong></summary>

**Là gì?** Mock kiểm tương tác; stub trả kết quả giả; harness cung cấp môi trường chạy test.  
**Ví dụ:** Mock `HAL_UART_Transmit`.

</details>

<details>
<summary><strong>Unity / CMock / Ceedling</strong></summary>

**Là gì?** Unity viết assertion; CMock tạo mocks; Ceedling build/run unit tests cho C.  
**Ví dụ:** `TEST_ASSERT_EQUAL(5,actual);`

</details>

<details>
<summary><strong>Coverage / traceability</strong></summary>

**Là gì?** Coverage đo mức test chạy qua cấu trúc/code; traceability liên kết yêu cầu với test.  
**Ví dụ:** Mỗi REQ có ít nhất một TC thích hợp.

</details>

<details>
<summary><strong>Root cause analysis</strong></summary>

**Là gì?** Xác định nguyên nhân gốc thay vì chỉ xử lý triệu chứng.  
**Ví dụ:** Treo UI do synchronous DB query.

</details>

<details>
<summary><strong>Log / crash dump</strong></summary>

**Là gì?** Log ghi sự kiện; dump lưu trạng thái tại thời điểm crash.  
**Ví dụ:** Stack trace cho biết hàm lỗi.

</details>

<details>
<summary><strong>Memory leak detection</strong></summary>

**Là gì?** Theo dõi cấp phát và tài nguyên không được thu hồi.  
**Ví dụ:** RAM process tăng dần sau nhiều lần mở màn hình.

</details>

<details>
<summary><strong>Software architecture</strong></summary>

**Là gì?** Phân chia hệ thống thành thành phần, interface và luồng dữ liệu.  
**Ví dụ:** Sensor driver → processing → UI.

</details>

<details>
<summary><strong>State machine</strong></summary>

**Là gì?** Mô hình hệ thống qua state và transition.  
**Ví dụ:** IDLE → RUNNING → ERROR.

</details>

<details>
<summary><strong>UML diagrams</strong></summary>

**Là gì?** Class: quan hệ kiểu; Sequence: trao đổi theo thời gian; Activity: luồng công việc.  
**Ví dụ:** Sequence diagram cho thao tác Send.

</details>

<details>
<summary><strong>Basic vs Detailed design</strong></summary>

**Là gì?** Basic/high-level mô tả thành phần và interface lớn; detailed mô tả logic, data structure, xử lý lỗi cụ thể.  
**Ví dụ:** Thiết kế module vs thuật toán parse frame.

</details>

<details>
<summary><strong>Code review</strong></summary>

**Là gì?** Kiểm code về logic, độ an toàn, maintainability và tiêu chuẩn.  
**Ví dụ:** Review lỗi null pointer.

</details>

<details>
<summary><strong>Static analysis</strong></summary>

**Là gì?** Phân tích mã mà không cần chạy nhằm phát hiện loại lỗi nhất định.  
**Ví dụ:** Cảnh báo uninitialized variable.

</details>

<details>
<summary><strong>Impact analysis</strong></summary>

**Là gì?** Xác định code/test/tài liệu liên quan khi sửa đổi.  
**Ví dụ:** Sửa DB schema → kiểm tra migration.

</details>

</details>

---

<details>
<summary><strong>NHÓM 10. FUJIFILM HEALTHCARE</strong></summary>

<details>
<summary><strong>Data Management</strong></summary>

**Là gì?** Chức năng quản lý thông tin bệnh nhân, ca khám và dữ liệu liên quan.  
**Ví dụ:** Tìm kiếm bệnh nhân/Exam.

</details>

<details>
<summary><strong>Store / Print</strong></summary>

**Là gì?** Chức năng lưu/truyền dữ liệu và in trong workflow máy siêu âm.  
**Ví dụ:** Gửi ảnh tới hệ thống DICOM.

</details>

<details>
<summary><strong>MFC / event-driven</strong></summary>

**Là gì?** MFC xây ứng dụng Windows dựa trên message và handler.  
**Ví dụ:** Click nút → message handler chạy.

</details>

<details>
<summary><strong>Message handling</strong></summary>

**Là gì?** Xử lý Windows message hoặc custom message để điều khiển UI.  
**Ví dụ:** `PostMessage(WM_APP+1,...)`.

</details>

<details>
<summary><strong>Multithreading</strong></summary>

**Là gì?** Thực hiện tác vụ nền song song để UI tiếp tục phản hồi.  
**Ví dụ:** Auto Delete chạy background.

</details>

<details>
<summary><strong>Background Auto Delete</strong></summary>

**Là gì?** Tác vụ xóa dữ liệu nền theo chính sách/điều kiện hệ thống.  
**Ví dụ:** Worker xử lý, UI hiển thị trạng thái.

</details>

<details>
<summary><strong>Cancel mechanism</strong></summary>

**Là gì?** Yêu cầu dừng công việc nền theo trạng thái an toàn; cần xử lý hoàn tất trước khi đóng luồng.  
**Ví dụ:** Cancel gửi cờ, worker kiểm tra rồi báo complete.

</details>

<details>
<summary><strong>PostMessage</strong></summary>

**Là gì?** Đưa message vào queue của thread sở hữu cửa sổ mà không chờ handler chạy xong.  
**Ví dụ:** Worker `PostMessage` để UI cập nhật.

</details>

<details>
<summary><strong>SQLite</strong></summary>

**Là gì?** CSDL quan hệ nhúng, lưu thông tin ngay trong file.  
**Ví dụ:** `SELECT ... FROM Patient;`

</details>

<details>
<summary><strong>DICOM</strong></summary>

**Là gì?** Chuẩn trao đổi và quản lý thông tin/hình ảnh y tế.  
**Ví dụ:** Gửi ảnh siêu âm đến PACS.

</details>

<details>
<summary><strong>MPPS</strong></summary>

**Là gì?** Modality Performed Procedure Step: trao đổi trạng thái thủ thuật được thực hiện.  
**Ví dụ:** Cập nhật trạng thái examination.

</details>

<details>
<summary><strong>Storage Commitment</strong></summary>

**Là gì?** DICOM service yêu cầu hệ thống nhận xác nhận trách nhiệm lưu trữ instance.  
**Ví dụ:** Thiết bị yêu cầu xác nhận ảnh đã được lưu.

</details>

<details>
<summary><strong>Root cause analysis</strong></summary>

**Là gì?** Dựa vào logs, source, event flow và debugging để xác định gốc lỗi.  
**Ví dụ:** Message đến khi view ở trạng thái không phù hợp.

</details>

<details>
<summary><strong>Impact / regression</strong></summary>

**Là gì?** Xác định phần bị ảnh hưởng rồi test lại các luồng liên quan.  
**Ví dụ:** Sửa Delete → test Find, Tile, Patient.

</details>

<details>
<summary><strong>QA / khách hàng Nhật</strong></summary>

**Là gì?** Làm rõ yêu cầu, báo cáo điều tra và phối hợp xác nhận fix.  
**Ví dụ:** Bug report gồm nguyên nhân, sửa đổi, test result.

</details>

</details>

---

<details>
<summary><strong>NHÓM 11. AUTOSAR & FPT TELECOM</strong></summary>

<details>
<summary><strong>AUTOSAR Classic</strong></summary>

**Là gì?** Kiến trúc chuẩn hóa phần mềm ECU với Application, RTE, BSW.  
**Ví dụ:** Automotive ECU software.

</details>

<details>
<summary><strong>Application / RTE / BSW / MCAL</strong></summary>

**Là gì?** App xử lý chức năng; RTE kết nối component; BSW cung cấp dịch vụ; MCAL truy cập ngoại vi MCU.  
**Ví dụ:** Eth driver nằm lớp thấp.

</details>

<details>
<summary><strong>EthIf</strong></summary>

**Là gì?** Ethernet Interface của AUTOSAR, kết nối upper-layer với Ethernet Driver/Transceiver abstraction.  
**Ví dụ:** Test `EthIf_SetControllerMode`.

</details>

<details>
<summary><strong>SWS / ECUC / ARXML</strong></summary>

**Là gì?** SWS quy định hành vi; ECUC mô tả tham số cấu hình; ARXML lưu mô hình/cấu hình AUTOSAR.  
**Ví dụ:** Viết test theo SWS requirement.

</details>

<details>
<summary><strong>mSQT test case</strong></summary>

**Là gì?** Test tự động gồm precondition, action, expected result, postcondition.  
**Ví dụ:** Gọi EthIf API rồi assert kết quả.

</details>

<details>
<summary><strong>DET / API verification</strong></summary>

**Là gì?** DET báo lỗi phát triển theo AUTOSAR; verify API so sánh hành vi với specification.  
**Ví dụ:** Invalid parameter → expected DET error.

</details>

<details>
<summary><strong>AURIX TC397 / TASKING / Mobilgene</strong></summary>

**Là gì?** TC397 là MCU; TASKING là compiler; Mobilgene dùng cấu hình/generate BSW.  
**Ví dụ:** Compile ECU code bằng TASKING.

</details>

<details>
<summary><strong>OpenWrt</strong></summary>

**Là gì?** Embedded Linux distribution dành cho router và thiết bị mạng.  
**Ví dụ:** Chạy service C/C++ trên router.

</details>

<details>
<summary><strong>Embedded Linux</strong></summary>

**Là gì?** Linux triển khai trên thiết bị có tài nguyên và nhu cầu cụ thể.  
**Ví dụ:** Process đọc trạng thái mạng.

</details>

<details>
<summary><strong>Wi-Fi</strong></summary>

**Là gì?** Công nghệ kết nối mạng không dây IEEE 802.11.  
**Ví dụ:** Router cung cấp WLAN.

</details>

<details>
<summary><strong>MQTT</strong></summary>

**Là gì?** Giao thức nhẹ publish/subscribe qua broker.  
**Ví dụ:** Device publish telemetry lên topic.

</details>

<details>
<summary><strong>Device connectivity</strong></summary>

**Là gì?** Quản lý kết nối, trạng thái, lỗi và phục hồi kết nối thiết bị.  
**Ví dụ:** Reconnect khi mất TCP/MQTT.

</details>

<details>
<summary><strong>Embedded debugging</strong></summary>

**Là gì?** Phân tích log, memory, process, giao tiếp và điều kiện tái hiện.  
**Ví dụ:** SSH xem log rồi đối chiếu code.

</details>

</details>

---

<details>
<summary><strong>NHÓM 12. HR & BEHAVIORAL INTERVIEW</strong></summary>

<details>
<summary><strong>Giới thiệu bản thân</strong></summary>

**Là gì?** Tóm tắt kinh nghiệm, ba dự án chính và lý do phù hợp.  
**Ví dụ:** “Tôi có hơn 5 năm kinh nghiệm C/C++, từng làm FUJIFILM, AUTOSAR và IoT.”

</details>

<details>
<summary><strong>Giới thiệu dự án</strong></summary>

**Là gì?** Nêu sản phẩm, vai trò thực tế, công nghệ, một đóng góp tiêu biểu.  
**Ví dụ:** “FUJIFILM: C++/MFC, debug UI và multithreading.”

</details>

<details>
<summary><strong>Vì sao Nihon Kohden?</strong></summary>

**Là gì?** Kết nối kinh nghiệm thiết bị y tế với mục tiêu phát triển embedded an toàn.  
**Ví dụ:** “Tôi muốn tiếp tục lĩnh vực medical software.”

</details>

<details>
<summary><strong>Vì sao medical device?</strong></summary>

**Là gì?** Thích bài toán phần mềm tin cậy, có tác động tới chăm sóc bệnh nhân.  
**Ví dụ:** Nhấn mạnh safety và testing.

</details>

<details>
<summary><strong>Chưa có Qt / RTOS</strong></summary>

**Là gì?** Nói rõ phạm vi chưa có kinh nghiệm trực tiếp, nêu kiến thức liên quan và kế hoạch học.  
**Ví dụ:** “Tôi có MFC/event-driven và đang tìm hiểu Qt.”

</details>

<details>
<summary><strong>Bug khó đã xử lý</strong></summary>

**Là gì?** Trình bày Situation–Task–Action–Result, ưu tiên bug thật.  
**Ví dụ:** “UI không cập nhật do thứ tự callback…” nếu đúng dự án.

</details>

<details>
<summary><strong>Root cause method</strong></summary>

**Là gì?** Reproduce → log → isolate → fix → regression.  
**Ví dụ:** Bug crash chỉ khi Cancel.

</details>

<details>
<summary><strong>Làm việc Hardware / QA</strong></summary>

**Là gì?** Thống nhất cách tái hiện, log/tín hiệu, expected result và phân công xử lý.  
**Ví dụ:** Software + Hardware kiểm tra UART frame.

</details>

<details>
<summary><strong>Khách hàng Nhật</strong></summary>

**Là gì?** Trao đổi rõ yêu cầu, bằng chứng kỹ thuật, tiến độ, kết quả test.  
**Ví dụ:** Báo cáo investigation và impact analysis.

</details>

<details>
<summary><strong>Safety vs deadline</strong></summary>

**Là gì?** Ưu tiên an toàn; báo rủi ro sớm, đề xuất phạm vi và kế hoạch kiểm chứng.  
**Ví dụ:** Không bỏ test safety-critical để kịp lịch.

</details>

<details>
<summary><strong>Hỏi lại nhà tuyển dụng</strong></summary>

**Là gì?** Hỏi phạm vi Qt/RTOS, sản phẩm, quy trình IEC 62304, môi trường kỹ thuật.  
**Ví dụ:** “Team đang dùng RTOS nào?”

</details>

</details>

---

</details>




# LUYỆN PHỎNG VẤN EMBEDDED C/C++ – NIHON KOHDEN

Nihon Kohden là công ty của Nhật Bản, được thành lập vào năm 1951, chuyên nghiên cứu, phát triển và sản xuất các thiết bị điện tử y tế.

Một số sản phẩm tiêu biểu :máy điện tim , máy điện não  và máy khử rung tim .

Về vcông việc của tôi tập trung vào phát triển phần mềm nhúng bằng C/C++, phát triển giao diện Qt, điều khiển trên RTOS và xử lý dữ liệu từ cảm biến.

## PHẦN 1. GIỚI THIỆU BẢN THÂN VÀ KINH NGHIỆM

Tôi có hơn 5 năm kinh nghiệm phát triển phần mềm C/C++, bao gồm Embedded Software và Windows Desktop Application.

Trước đây, tôi từng làm việc tại FPT Software và FPT Telecom, tham gia các dự án liên quan đến thiết bị y tế, Automotive và IoT.

**Dự án gần đây nhất của tôi là FUJIFILM Healthcare**, phát triển và bảo trì phần mềm cho hệ thống máy siêu âm.

Trong dự án này, tôi chủ yếu sử dụng C++, MFC, Win32 API và SQLite. Công việc chính của tôi là phân tích source code, điều tra và sửa lỗi phần mềm, đặc biệt là các lỗi liên quan đến UI, multithreading, database và xử lý dữ liệu.

Ngoài ra, tôi cũng viết tài liệu kỹ thuật, test case, phân tích phạm vi ảnh hưởng sau khi sửa lỗi và thực hiện regression testing.

**Trước đó, tôi tham gia dự án Automotive của Hyundai AutoEver**, phát triển và kiểm thử phần mềm ECU theo chuẩn AUTOSAR Classic.

Tôi phụ trách thiết kế và thực hiện test case cho module EthIf dựa trên tài liệu SWS và ECUC, kiểm tra hoạt động của API, phân tích các trường hợp test fail và thực hiện regression testing.

Công nghệ sử dụng gồm Embedded C/C++, AUTOSAR Classic, mSQT, Infineon AURIX TC397, TASKING Compiler và Mobilgene.

**Trước khi làm việc tại FPT Software, tôi làm việc tại FPT Telecom**, tham gia các dự án phần mềm nhúng cho thiết bị mạng và IoT như Router, Modem, FPT Play Box và Smart Home.

Ở FPT Telecom, em chủ yếu phát triển phần mềm Embedded trên nền tảng Linux/OpenWrt bằng C/C++, cho các thiết bị như FPT Play Box và Smart Home.

Công việc của em tập trung vào xử lý kết nối Wi-Fi, MQTT, nhận lệnh điều khiển từ server và gửi trạng thái thiết bị lên hệ thống.

Các công nghệ tôi đã sử dụng gồm C/C++, OpenWrt, Linux, Wi-Fi và MQTT. Công việc liên quan đến phát triển chức năng kết nối, điều khiển, giám sát thiết bị, phân tích lỗi và kiểm thử.

Thông qua các dự án này, tôi đã tích lũy kinh nghiệm về C/C++, debugging, multithreading, communication protocols và software testing.

Tôi mong muốn tiếp tục phát triển chuyên môn trong lĩnh vực Embedded Software, đặc biệt là các sản phẩm thiết bị y tế đòi hỏi độ an toàn và độ tin cậy cao.

---

# PHẦN 2. CÂU HỎI PHỎNG VẤN DỰ ÁN FPT TELECOM – IoT

## Nhóm 1. Giới thiệu dự án IoT tại FPT Telecom

<details>
<summary><strong>1. Bạn đã làm những dự án gì tại FPT Telecom?</strong></summary>

Tôi từng tham gia phát triển phần mềm nhúng cho các thiết bị FPT Play Box, Smart Home, Router và Modem. Công việc liên quan đến phát triển chức năng kết nối, điều khiển và giám sát thiết bị bằng C/C++.

</details>

<details>
<summary><strong>2. Vai trò chính của bạn trong các dự án là gì?</strong></summary>

Ở giai đoạn đầu, tôi làm Embedded Software Engineer, tham gia phát triển chức năng, tích hợp, kiểm thử và debug. Giai đoạn sau, tôi đảm nhận vai trò Project Manager, quản lý tiến độ và phối hợp giữa các nhóm phát triển, kiểm thử.

</details>

<details>
<summary><strong>3. Dự án sử dụng những công nghệ gì?</strong></summary>

Chủ yếu sử dụng C/C++, OpenWrt, GCC Toolchain, OpenWrt SDK, Wi-Fi và MQTT. Tôi cũng sử dụng Git, SVN để quản lý source code.

</details>

<details>
<summary><strong>4. IoT là gì?</strong></summary>

IoT (Internet of Things) là hệ thống các thiết bị có khả năng kết nối mạng để thu thập dữ liệu, trao đổi thông tin, giám sát hoặc điều khiển từ xa.

</details>

<details>
<summary><strong>5. Kiến trúc cơ bản của một hệ thống IoT như thế nào?</strong></summary>

Thông thường gồm ba thành phần: thiết bị IoT, hệ thống mạng truyền thông và server/cloud. Thiết bị thu thập dữ liệu hoặc thực hiện lệnh điều khiển, sau đó giao tiếp với server qua các giao thức như MQTT hoặc HTTP.

</details>

---

## Nhóm 2. Embedded C/C++ và OpenWrt

<details>
<summary><strong>6. OpenWrt là gì?</strong></summary>

OpenWrt là hệ điều hành mã nguồn mở dựa trên Linux, thường được sử dụng cho Router và các thiết bị mạng nhúng. Nó hỗ trợ quản lý mạng, cấu hình thiết bị và phát triển ứng dụng C/C++.

</details>

<details>
<summary><strong>7. OpenWrt khác RTOS như thế nào?</strong></summary>

OpenWrt sử dụng Linux, chủ yếu phục vụ các thiết bị mạng và ứng dụng cần nhiều chức năng hệ thống. RTOS tập trung vào khả năng đáp ứng thời gian thực với độ trễ có thể dự đoán được.

</details>

<details>
<summary><strong>8. Bạn biên dịch chương trình C/C++ cho thiết bị OpenWrt như thế nào?</strong></summary>

Sử dụng cross compiler và OpenWrt SDK để biên dịch chương trình trên máy phát triển, tạo file thực thi phù hợp với kiến trúc CPU của thiết bị đích.

</details>

<details>
<summary><strong>9. Cross compiler là gì?</strong></summary>

Cross compiler là trình biên dịch chạy trên một nền tảng nhưng tạo chương trình thực thi cho nền tảng khác. Ví dụ, biên dịch trên máy x86 để tạo chương trình chạy trên thiết bị ARM.

</details>

<details>
<summary><strong>10. Bạn debug phần mềm trên thiết bị Embedded Linux như thế nào?</strong></summary>

Có thể sử dụng log, SSH, GDB hoặc phân tích trạng thái process. Khi gặp lỗi, tôi ưu tiên kiểm tra log, tái hiện lỗi và xác định nguyên nhân trước khi sửa code.

</details>

<details>
<summary><strong>11. Process và Thread khác nhau như thế nào?</strong></summary>

Process có không gian địa chỉ riêng, còn các thread trong cùng process chia sẻ bộ nhớ và tài nguyên của process. Thread giao tiếp nhanh hơn nhưng cần đồng bộ khi truy cập dữ liệu chung.

</details>

<details>
<summary><strong>12. Nếu một ứng dụng trên OpenWrt bị crash, bạn sẽ xử lý thế nào?</strong></summary>

Tôi kiểm tra log và điều kiện xảy ra lỗi, tìm cách tái hiện, sau đó sử dụng debugger hoặc core dump nếu có để xác định vị trí lỗi. Cuối cùng, sửa code và thực hiện regression test.

</details>

---

## Nhóm 3. MQTT và giao tiếp IoT

<details>
<summary><strong>13. MQTT là gì?</strong></summary>

MQTT là giao thức truyền thông nhẹ dựa trên mô hình Publish/Subscribe, thường sử dụng TCP và phù hợp với các thiết bị IoT có tài nguyên hoặc băng thông hạn chế.

</details>

<details>
<summary><strong>14. MQTT hoạt động như thế nào?</strong></summary>

MQTT gồm Broker, Publisher và Subscriber. Publisher gửi message đến một topic trên Broker, sau đó Broker chuyển message đến các client đã subscribe topic đó.

**Mô hình:**

`IoT Device (Publisher) → MQTT Broker → Application (Subscriber)`

</details>

<details>
<summary><strong>15. MQTT khác HTTP như thế nào?</strong></summary>

HTTP chủ yếu sử dụng mô hình Request/Response, còn MQTT sử dụng Publish/Subscribe. MQTT phù hợp với các ứng dụng IoT cần trao đổi message thường xuyên và có độ overhead thấp.

</details>

<details>
<summary><strong>16. MQTT có những mức QoS nào?</strong></summary>

- **QoS 0:** At most once — gửi tối đa một lần.
- **QoS 1:** At least once — đảm bảo nhận ít nhất một lần, có thể trùng.
- **QoS 2:** Exactly once — đảm bảo message được nhận đúng một lần theo cơ chế MQTT.

</details>

<details>
<summary><strong>17. Nếu thiết bị mất kết nối MQTT thì xử lý thế nào?</strong></summary>

Phát hiện mất kết nối, ghi log, thực hiện reconnect với khoảng chờ phù hợp và khôi phục subscription khi cần. Với dữ liệu quan trọng, cần có cơ chế lưu tạm hoặc đồng bộ lại sau khi kết nối phục hồi.

</details>

<details>
<summary><strong>18. MQTT Broker là gì?</strong></summary>

Broker là server trung gian nhận message từ Publisher và phân phối đến các Subscriber dựa trên topic mà họ đăng ký.

</details>

<details>
<summary><strong>19. Làm sao để bảo mật giao tiếp MQTT?</strong></summary>

Có thể sử dụng TLS để mã hóa đường truyền, xác thực client và cấu hình quyền Publish/Subscribe theo từng topic.

</details>

---

## Nhóm 4. Wi-Fi, Networking và Communication

<details>
<summary><strong>20. TCP và UDP khác nhau như thế nào?</strong></summary>

TCP là giao thức hướng kết nối, cung cấp truyền dữ liệu tin cậy và đúng thứ tự. UDP không thiết lập kết nối, overhead thấp hơn nhưng không tự đảm bảo dữ liệu đến nơi hoặc đúng thứ tự.

</details>

<details>
<summary><strong>21. Socket là gì?</strong></summary>

Socket là điểm cuối giao tiếp mạng, cho phép các process trao đổi dữ liệu thông qua các giao thức như TCP hoặc UDP.

</details>

<details>
<summary><strong>22. Nếu thiết bị kết nối Wi-Fi nhưng không giao tiếp được với server, bạn sẽ kiểm tra gì?</strong></summary>

Tôi kiểm tra IP, gateway, DNS, khả năng kết nối mạng, trạng thái TCP connection và log ứng dụng để xác định lỗi nằm ở mạng hay phần mềm.

</details>

<details>
<summary><strong>23. IP Address và MAC Address khác nhau thế nào?</strong></summary>

IP Address là địa chỉ logic dùng để định tuyến trong mạng IP. MAC Address là địa chỉ của giao diện mạng, thường được sử dụng tại tầng liên kết dữ liệu.

</details>

<details>
<summary><strong>24. Nếu server không phản hồi, ứng dụng Embedded nên xử lý thế nào?</strong></summary>

Sử dụng timeout, kiểm tra trạng thái kết nối và retry có giới hạn. Không nên để chương trình chờ vô thời hạn vì có thể làm ảnh hưởng đến các chức năng khác.

</details>

<details>
<summary><strong>25. Làm thế nào để kiểm tra thiết bị có kết nối Internet?</strong></summary>

Có thể kiểm tra IP, gateway, DNS và thử kết nối đến endpoint của dịch vụ. Chỉ ping gateway thành công chưa đủ để khẳng định thiết bị có Internet.

</details>

---

## Nhóm 5. Memory, Multithreading và Debugging

<details>
<summary><strong>26. Memory leak là gì?</strong></summary>

Memory leak xảy ra khi chương trình cấp phát bộ nhớ nhưng không giải phóng sau khi không còn sử dụng, khiến lượng bộ nhớ khả dụng giảm dần.

</details>

<details>
<summary><strong>27. Làm sao phát hiện memory leak trên Embedded Linux?</strong></summary>

Có thể theo dõi mức sử dụng RAM của process qua `/proc`, `top`, `ps` hoặc sử dụng công cụ như Valgrind nếu môi trường hỗ trợ.

</details>

<details>
<summary><strong>28. Race condition là gì?</strong></summary>

Race condition xảy ra khi kết quả chương trình phụ thuộc vào thứ tự hoặc thời điểm các luồng thực thi. Nếu nhiều thread truy cập và thay đổi dữ liệu chung không được đồng bộ, có thể dẫn đến kết quả sai.

</details>

<details>
<summary><strong>29. Mutex dùng để làm gì?</strong></summary>

Mutex dùng để bảo vệ tài nguyên dùng chung, đảm bảo chỉ một thread sở hữu mutex và truy cập vùng critical section được bảo vệ tại một thời điểm.

</details>

<details>
<summary><strong>30. Nếu thiết bị chạy nhiều ngày rồi bị treo, bạn xử lý như thế nào?</strong></summary>

Tôi sẽ kiểm tra log và tài nguyên hệ thống, đặc biệt là RAM, CPU, thread và file descriptor. Sau đó xem xét các nguyên nhân như memory leak, deadlock hoặc lỗi giao tiếp, tìm cách tái hiện và sửa nguyên nhân gốc.

</details>

---

## Nhóm 6. Câu hỏi tình huống liên quan đến Nihon Kohden

<details>
<summary><strong>31. Kinh nghiệm IoT của bạn có thể áp dụng vào thiết bị y tế như thế nào?</strong></summary>

Tôi có thể áp dụng kiến thức Embedded C/C++, xử lý dữ liệu, giao tiếp thiết bị, debugging và kiểm thử. Tuy nhiên, thiết bị y tế có các yêu cầu an toàn và độ tin cậy đặc thù mà tôi cần tiếp tục tìm hiểu.

</details>

<details>
<summary><strong>32. Nếu cảm biến IoT ngừng gửi dữ liệu thì bạn xử lý ra sao?</strong></summary>

Tôi sẽ kiểm tra timeout, trạng thái kết nối và tính hợp lệ của dữ liệu. Khi phát hiện lỗi, hệ thống cần thông báo trạng thái phù hợp và thực hiện cơ chế phục hồi theo yêu cầu thiết kế.

</details>

<details>
<summary><strong>33. Nếu nhiều thiết bị cùng gửi dữ liệu về server thì làm sao xử lý?</strong></summary>

Có thể sử dụng kiến trúc message-based, MQTT Broker và các cơ chế buffering hoặc queue để tiếp nhận, phân phối và xử lý dữ liệu. Cần đánh giá thêm throughput và tài nguyên hệ thống.

</details>

<details>
<summary><strong>34. Nếu nhận được dữ liệu cảm biến sai, bạn xử lý thế nào?</strong></summary>

Tôi sẽ kiểm tra định dạng gói tin, độ dài, checksum hoặc CRC nếu có, đồng thời kiểm tra dữ liệu nằm trong phạm vi hợp lệ. Không nên sử dụng dữ liệu lỗi để thực hiện điều khiển.

</details>

<details>
<summary><strong>35. Theo bạn, yếu tố nào quan trọng nhất khi phát triển phần mềm thiết bị y tế?</strong></summary>

Tôi cho rằng an toàn và độ tin cậy là những yếu tố quan trọng hàng đầu. Phần mềm cần xử lý lỗi có kiểm soát, bảo vệ dữ liệu và được kiểm thử đầy đủ theo yêu cầu và mức độ rủi ro của thiết bị.

</details>

---

# PHẦN 3. CÁC CÂU HỎI CẦN ƯU TIÊN ÔN TẬP

| STT | Câu hỏi cần chuẩn bị |
|---|---|
| 1 | Tại FPT Telecom, bạn trực tiếp phát triển những chức năng Embedded C/C++ nào? |
| 2 | Bạn sử dụng MQTT để giải quyết bài toán gì trong hệ thống IoT? |
| 3 | OpenWrt là gì và khác RTOS như thế nào? |
| 4 | Bạn đã từng gặp lỗi kỹ thuật nào khó trong dự án IoT và xử lý ra sao? |
| 5 | Kinh nghiệm phát triển thiết bị IoT có thể áp dụng như thế nào vào thiết bị y tế? |

**Lưu ý khi phỏng vấn:** Cần chuẩn bị ít nhất một chức năng Embedded C/C++ đã trực tiếp lập trình và một lỗi thực tế từng xử lý tại FPT Telecom, bao gồm nguyên nhân, cách khắc phục và kết quả.


<details>
<summary><strong>Câu 0: Qúa trình biên dịch</strong></summary>

**Trả lời phỏng vấn:** Trả lời phỏng vấn:
Quá trình biên dịch C/C++ gồm 4 bước:

**Preprocessing → Compilation → Assembly → Linking**


- Preprocessor :tạo file main.i.
    - 3 việc xảy ra trong quá trình tiền xử lý:
        - #include file header
        - Xóa bỏ comment.
        - Triển khai macro
- Compiler chuyển code sang Assembly.(file .i sang file ngôn ngữ assembly là file .s)
- Assembler: compiler sẽ Biên dịch ngôn ngữ Assembly sang ngôn ngữ máy (0 và 1). Và tạo ra tệp tin Object .o
- Linker ghép các Object File và thư viện để tạo file thực thi.


main.c
  ↓ Preprocessing
main.i
  ↓ Compilation
main.s
  ↓ Assembly
main.o
  ↓ Linking
Executable

</details>

<details>
<summary><strong>Câu 0.1: sự khác biệt giữa vi điều khiển và vi xử lý</strong></summary>

**Trả lời phỏng vấn:** Trả lời phỏng vấn:
### Sự khác biệt giữa vi điều khiển và vi xử lý

- **Vi điều khiển (MCU):** tích hợp CPU, bộ nhớ và ngoại vi trên cùng một chip, thường dùng cho tác vụ chuyên biệt.
- **Vi xử lý (MPU):** chủ yếu là CPU, cần RAM, bộ nhớ và ngoại vi bên ngoài, thường dùng cho hệ thống đa năng.

### Các thành phần của vi điều khiển

Gồm: **CPU, RAM, ROM/Flash và các ngoại vi** như GPIO, UART, SPI, I2C, ADC, DAC, PWM, Timer/Counter.

### Thư viện tĩnh và thư viện động

- **Static Library:** được liên kết vào chương trình khi build → file thực thi lớn hơn nhưng không cần thư viện riêng khi chạy.
- **Dynamic Library:** được liên kết/nạp khi chạy → file thực thi nhỏ hơn, dễ cập nhật nhưng cần thư viện tương ứng khi chạy.

</details>

<details>
<summary><strong>Câu 0.2: Thư viện tĩnh và thư viện động</strong></summary>

**Trả lời phỏng vấn:** Trả lời phỏng vấn:
### Thư viện tĩnh và thư viện động

- **Static Library:** được liên kết vào chương trình khi build → file thực thi lớn hơn nhưng không cần thư viện riêng khi chạy.
- **Dynamic Library:** được liên kết/nạp khi chạy → file thực thi nhỏ hơn, dễ cập nhật nhưng cần thư viện tương ứng khi chạy.

</details>



<details>
<summary><strong>Câu 1: Quy trình xử lý interrupt diễn ra như thế nào?</strong></summary>

**Trả lời phỏng vấn:** Khi có interrupt, CPU tạm dừng chương trình hiện tại, lưu trạng thái đang chạy vào stack, thực thi hàm xử lý ngắt `ISR`, sau đó khôi phục trạng thái và tiếp tục chương trình trước đó.

</details>

<details>
<summary><strong>Câu 2: SPI có bao nhiêu dây?</strong></summary>

**Trả lời phỏng vấn:** SPI với N Slave thường cần 3 + N dây: 3 dây dùng chung là SCLK, MOSI, MISO, và mỗi Slave có một dây CSS riêng
- SPI có tốc độ cao, thường nhanh hơn UART và I2C. Ưu điểm là truyền full-duplex và giao thức đơn giản. Nhược điểm là tốn nhiều chân, đặc biệt khi có nhiều Slave vì mỗi Slave thường cần một chân CS riêng.
</details>

<details>
<summary><strong>Câu 3: Stack và Queue khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** `Stack` hoạt động theo nguyên tắc `LIFO` - vào sau ra trước. `Queue` hoạt động theo nguyên tắc `FIFO` - vào trước ra trước.


</details>

<details>
<summary><strong>Câu 4: Bạn đã dùng RTOS nào trong công việc?</strong></summary>

**Trả lời phỏng vấn:** Tôi chưa trực tiếp làm nhiều với FreeRTOS, chủ yếu làm OpenWrt và AUTOSAR, nhưng tôi có kiến thức cơ bản về task, scheduling, mutex và semaphore.

- `RTOS` = `Real-Time Operating System`: hệ điều hành thời gian thực, dùng để quản lý các task và đảm bảo task được xử lý đúng thời gian yêu cầu.
- `FreeRTOS`: một RTOS cụ thể, mã nguồn mở, rất phổ biến trong embedded.
- `Task`: một công việc hoặc chương trình nhỏ được RTOS quản lý và cho chạy.
- `Scheduling`: cơ chế RTOS quyết định task nào được chạy trước, thường dựa vào `priority`.

</details>

<details>
<summary><strong>Câu 5: Trong ASPICE, Software Requirement liên kết với gì?</strong></summary>

**Trả lời phỏng vấn:** 
## ASPICE là gì?

`ASPICE` = `Automotive SPICE`.

Là mô hình chuẩn hóa và đánh giá quy trình phát triển phần mềm ô tô.

Flow cơ bản:

`Requirement → Design → Coding → Testing → Traceability`

---

## Các thành phần chính

- `System Requirement`: yêu cầu hệ thống.
- `Software Requirement`: yêu cầu phần mềm.
- `Software Design`: thiết kế phần mềm.
- `Implementation`: coding.
- `Unit Test`: test từng phần nhỏ.
- `Integration Test`: test các module khi kết hợp.
- `Traceability`: truy vết giữa Requirement, Design và Test.

---

## Software Requirement liên kết với gì?

Thường liên kết với:

`System Requirement → Software Requirement → Software Design → Test Case`

---

## Software Requirement là gì?

`Software Requirement` = **Yêu cầu phần mềm**.

Mô tả phần mềm cần làm gì để đáp ứng yêu cầu của hệ thống.

Ý nghĩa:
- Là cơ sở để design.
- Là cơ sở để coding.
- Là cơ sở để tạo test case.

</details>

<details>
<summary><strong>Câu 6: Tessy và Polyspace là gì?</strong></summary>

**Trả lời phỏng vấn:** `Tessy` là tool dùng cho Unit Test và Integration Test trong embedded. `Polyspace` là tool dùng cho static code analysis.
## Unit Test, Integration Test và Static Code Analysis là gì?

- `Unit Test`: kiểm tra từng function hoặc module nhỏ một cách độc lập.
- `Integration Test`: kiểm tra các module khi kết hợp với nhau có hoạt động đúng không.
- `Static Code Analysis`: phân tích source code mà không cần chạy chương trình, để tìm lỗi, coding rule violation hoặc vấn đề tiềm ẩn.

</details>

<details>
<summary><strong>Câu 7: MCAL Timer liên quan đến gì?</strong></summary>

**Trả lời phỏng vấn:** Trong AUTOSAR, module thường gặp liên quan đến timer là `GPT`. Flow cơ bản là init timer, start timer, timer chạy, timeout rồi xử lý bằng interrupt hoặc callback.

</details>

<details>
<summary><strong>Câu 8: Khi communication không hoạt động thì kiểm tra gì?</strong></summary>

**Trả lời phỏng vấn:** Tôi sẽ kiểm tra :
- Kết nối phần cứng.
- Cấu hình hai bên có giống nhau không.
- `Clock`.
- `Driver` đã được init chưa.
- `TX/RX` có dữ liệu không.
- Nếu cần, dùng `Logic Analyzer` hoặc `Oscilloscope` để kiểm tra signal.

</details>

<details>
<summary><strong>Câu 9: UDS Security Access hoạt động thế nào?</strong></summary>

**Trả lời phỏng vấn:** `SecurityAccess` là service `0x27`. Tester xin `Seed` từ ECU, dùng Seed để tính `Key`, gửi Key lại ECU. ECU kiểm tra Key, nếu đúng thì unlock security level.
- UDS Security Access là cơ chế xác thực để mở quyền truy cập vào các chức năng bị bảo vệ trên ECU.
</details>

<details>
<summary><strong>Câu 10: UART không hoạt động thì kiểm tra gì?</strong></summary>

## UART là gì?

`UART` = `Universal Asynchronous Receiver/Transmitter`.
Là giao tiếp nối tiếp không đồng bộ, dùng để truyền và nhận dữ liệu giữa hai thiết bị qua `TX` và `RX`.
`USART` : hỗ trợ cả không đồng bộ và đồng bộ.USART có thêm tín hiệu Clock để hai thiết bị đồng bộ dữ liệu.
---

## Ưu điểm

- Đơn giản, dễ sử dụng.
- Chỉ cần ít dây: `TX`, `RX`, `GND`.
- Phù hợp để debug, log và giao tiếp giữa các vi điều khiển hoặc module.

---

## Nhược điểm

- Tốc độ thường thấp hơn `SPI`.
- Chủ yếu dùng giao tiếp điểm-điểm giữa hai thiết bị.
- Hai bên phải cấu hình giống nhau về baud rate, parity, data bit và stop bit.
- Không có clock riêng nên nếu cấu hình timing sai có thể gây lỗi dữ liệu.

**Trả lời phỏng vấn:** Nếu UART không hoạt động, tôi sẽ kiểm tra theo 3 nhóm:

- **Phần cứng:**
  - Kiểm tra dây `TX`, `RX`, `GND` có nối đúng không.
  - Kiểm tra nguồn và mức điện áp có đúng không.

- **Phần mềm:**
  - Kiểm tra cấu hình UART như baud rate, parity, stop bit.
  - Kiểm tra `clock`, pin configuration và driver đã init chưa.
  - Kiểm tra dữ liệu `TX/RX` có được gửi và nhận đúng không.

- **Logic Analyzer / Oscilloscope:**
  - Dùng để kiểm tra tín hiệu thực tế trên chân `TX/RX`.
  - Giúp xác định có signal hay không và baud rate/timing có đúng không.

</details>

<details>
<summary><strong>Câu 11: FreeRTOS có bao nhiêu trạng thái task?</strong></summary>

**Trả lời phỏng vấn:** Có 4 trạng thái chính: `Running`, `Ready`, `Blocked` và `Suspended`.

</details>

<details>
<summary><strong>Câu 12: Process và Thread khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** `Process` có vùng memory riêng. `Thread` nằm trong một process và share memory với các thread khác, nhưng mỗi thread có stack và execution context riêng.

</details>

<details>
<summary><strong>Câu 13: `.bss` và `.data` khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** `.data` chứa các biến global/static đã được khởi tạo. `.bss` chứa các biến global/static chưa khởi tạo hoặc được khởi tạo bằng `0`.

</details>


</details>



<details>
<summary><strong>Câu 0: cấu trúc dữ liệu và giải thuật</strong></summary>

## Các cấu trúc dữ liệu phổ biến

- `Array`: mảng, lưu các phần tử liên tiếp trong bộ nhớ.
- `Linked List`: danh sách liên kết, mỗi node trỏ tới node tiếp theo.
- `Stack`: LIFO – vào sau ra trước. ví dụ call stack
- `Queue`: FIFO – vào trước ra trước. ví dụ truyền msg
- `Deque`: có thể thêm/xóa ở cả hai đầu.
- `Heap`: cây dùng để lấy phần tử lớn nhất/nhỏ nhất nhanh.

---


## Các giải thuật phổ biến trong C/C++

### 1. Linear Search (Tìm kiếm tuyến tính)
- Duyệt từng phần tử từ đầu đến cuối để tìm giá trị cần tìm.
- Không yêu cầu dữ liệu được sắp xếp.
- Độ phức tạp: `O(n)`.



### 2. Bubble Sort (Sắp xếp nổi bọt)
- So sánh hai phần tử liền kề, sai thứ tự thì hoán đổi.
- Dễ hiểu nhưng không hiệu quả với dữ liệu lớn.
- 2 vòng for
- Độ phức tạp: `O(n²)`.




---

## Cách nhớ nhanh

- `Linear Search` → Tìm từng phần tử.
- `Bubble Sort` → So sánh hai phần tử cạnh nhau rồi đổi chỗ.
 


**Thuật ngữ:**
- `O(n)`: Số thao tác tăng tuyến tính theo lượng dữ liệu.
- `O(log n)`: Số thao tác tăng chậm khi dữ liệu tăng.
- `O(n²)`: Số thao tác tăng theo bình phương lượng dữ liệu.
- `V`: Số đỉnh trong đồ thị.
- `E`: Số cạnh nối các đỉnh.


</details>
<details>
<summary><strong>Câu 1: Biến là gì? Khai báo và khởi tạo khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** Biến là một đối tượng có tên, có kiểu dữ liệu và dùng để lưu giá trị. Khai báo giới thiệu tên và kiểu của biến; định nghĩa tạo biến và thường cấp vùng nhớ. Khởi tạo cung cấp giá trị ban đầu khi biến được định nghĩa.

</details>

<details>
<summary><strong>Câu 2: Biến local, global và `static` khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** Biến local chỉ dùng trong khối khai báo và thường nằm trên stack. Biến global dùng được ở phạm vi toàn cục. Biến `static` tồn tại suốt chương trình. Global và static thường nằm ở Data hoặc BSS.

</details>

<details>
<summary><strong>Câu 3: Con trỏ là gì? `&x` và `*p` có ý nghĩa gì?</strong></summary>

**Trả lời phỏng vấn:** Con trỏ là biến lưu địa chỉ. `&x` lấy địa chỉ của `x`; `*p` lấy giá trị tại địa chỉ mà `p` trỏ tới.

</details>

<details>
<summary><strong>Câu 4: Con trỏ null, con trỏ hoang và con trỏ treo khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** Con trỏ null không trỏ đến đối tượng hợp lệ. Wild pointer chưa được khởi tạo. Dangling pointer trỏ đến vùng nhớ hoặc đối tượng đã hết thời gian sống.

</details>

<details>
<summary><strong>Câu 5: Có được trả về địa chỉ của biến local không?</strong></summary>

**Trả lời phỏng vấn:** Không. Biến local hết thời gian sống khi hàm kết thúc, nên con trỏ trả về bị treo. Truy cập qua con trỏ đó gây undefined behavior.

</details>

<details>
<summary><strong>Câu 6: `malloc()` và `free()` dùng để làm gì?</strong></summary>

**Trả lời phỏng vấn:** `malloc()` cấp phát bộ nhớ động nhưng không khởi tạo giá trị. `free()` giải phóng vùng nhớ đó. Không `free()` khi không còn sử dụng sẽ gây memory leak.

</details>

<details>
<summary><strong>Câu 7: Truy cập con trỏ sau khi `free()` gây ra điều gì?</strong></summary>

**Trả lời phỏng vấn:** Sau `free()`, con trỏ trở thành dangling pointer. Truy cập vùng nhớ qua nó là lỗi use-after-free và gây undefined behavior.

</details>

<details>
<summary><strong>Câu 8: `const int *p` và `int *const p` khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** `const int *p` là con trỏ tới hằng: đổi được địa chỉ `p`, không sửa được `*p` qua con trỏ này. `int *const p` là con trỏ hằng: không đổi được địa chỉ `p`, nhưng sửa được `*p`.

</details>

<details>
<summary><strong>Câu 9: `struct` là gì? Vì sao kích thước có thể lớn hơn tổng các thành viên?</strong></summary>

**Trả lời phỏng vấn:** `struct` là kiểu dữ liệu do người dùng định nghĩa, dùng để nhóm nhiều thành viên. Kích thước của nó gồm kích thước các thành viên và padding do yêu cầu căn chỉnh bộ nhớ.

</details>

<details>
<summary><strong>Câu 10: `volatile` có ý nghĩa gì? Có đảm bảo atomic và thread-safe không?</strong></summary>

**Trả lời phỏng vấn:** `volatile` cho compiler biết giá trị có thể thay đổi ngoài luồng thực thi thông thường, nên các lần truy cập không được loại bỏ tùy ý. Nó thường dùng với thanh ghi phần cứng và biến được ISR thay đổi. `volatile` không đảm bảo atomic hoặc thread-safe.

</details>

<details>
<summary><strong>Câu 11: Stack và Heap khác nhau như thế nào?</strong></summary>

**Trả lời phỏng vấn:** Stack được quản lý tự động, thường chứa biến local và thông tin gọi hàm; truy cập nhanh nhưng kích thước giới hạn, có thể stack overflow. Heap dùng cho cấp phát động bằng `malloc`/`new`; linh hoạt và thường lớn hơn nhưng có thể gây memory leak, phân mảnh và phải quản lý thủ công.

</details>

<details>
<summary><strong>Câu 12: Vùng Data và BSS khác nhau như thế nào?</strong></summary>

**Trả lời phỏng vấn:** Data thường chứa biến global/static được khởi tạo khác 0. BSS thường chứa biến global/static chưa khởi tạo hoặc khởi tạo bằng 0.

</details>

<details>
<summary><strong>Câu 13: Memory leak và memory fragmentation khác nhau như thế nào?</strong></summary>

**Trả lời phỏng vấn:** Memory leak là bộ nhớ không được giải phóng khi không còn sử dụng. Fragmentation là bộ nhớ trống bị chia nhỏ, khiến cấp phát khối lớn có thể thất bại dù tổng dung lượng trống vẫn đủ.

</details>

<details>
<summary><strong>Câu 14: Function pointer là gì và được dùng khi nào?</strong></summary>

**Trả lời phỏng vấn:** Function pointer là con trỏ lưu địa chỉ của hàm, dùng để gọi hàm gián tiếp. Nó thường được dùng cho callback, bảng chọn hàm và thay đổi hành vi khi chạy.

</details>

<details>
<summary><strong>Câu 15: Truyền tham trị và truyền tham chiếu khác nhau như thế nào?</strong></summary>

**Trả lời phỏng vấn:** Truyền tham trị tạo bản sao của đối số. Truyền tham chiếu cho phép thao tác trên đối tượng gốc. C chỉ có truyền tham trị và dùng con trỏ để mô phỏng; C++ hỗ trợ tham chiếu trực tiếp.

</details>

<details>
<summary><strong>Câu 16: Trong C++, `class` và `struct` khác nhau ở điểm nào?</strong></summary>

**Trả lời phỏng vấn:** Trong C++, `class` và `struct` gần như giống nhau. `class` mặc định truy cập và kế thừa `private`, còn `struct` mặc định là `public`.

</details>

<details>
<summary><strong>Câu 17: Bốn tính chất chính của lập trình hướng đối tượng là gì?</strong></summary>

**Trả lời phỏng vấn:** Bốn tính chất chính của OOP là **Encapsulation (Đóng gói)**, **Inheritance (Kế thừa)**, **Polymorphism (Đa hình)** và **Abstraction (Trừu tượng)**.

---

# Tổng hợp kiến thức OOP

## 1. OOP là gì?

**OOP – Object-Oriented Programming** là lập trình hướng đối tượng.

Ý tưởng chính là tổ chức chương trình dựa trên:

- **Class** → khuôn mẫu.
- **Object** → đối tượng được tạo từ class.
- Object có:
  - **Attribute / Property** → dữ liệu, trạng thái.
  - **Method** → hành vi.

Ví dụ ngoài đời:

```text
Class: Car
```

Thuộc tính:

- color
- speed

Hành vi:

- start()
- stop()

Từ `Car` có thể tạo nhiều object:

```text
car1
car2
car3
```

Mỗi object có dữ liệu riêng.

### Cách trả lời phỏng vấn

> OOP là phương pháp lập trình tổ chức chương trình dựa trên các object. Object chứa dữ liệu và các method xử lý dữ liệu đó.

---

## 2. Class.

### Class là gì?

**Class là bản thiết kế/khuôn mẫu dùng để tạo object.**

Class định nghĩa:

```text
Class
 ├── Attribute
 └── Method
```

Ví dụ C++:

```cpp
class Student
{
public:
    int id;
    string name;

    void display()
    {
        cout << id << " " << name;
    }
};
```

Ở đây:

- `Student` là **class**.
- `id`, `name` là **attribute/member variable**.
- `display()` là **method/member function**.

Điểm cần nhớ:

- Class là kiểu dữ liệu do người dùng định nghĩa.
- Class là bản thiết kế để tạo object.
- Có thể tạo nhiều object từ một class.
- Bộ nhớ cho dữ liệu của object được tạo khi object tồn tại.

### Cách nhớ

> **Class = bản thiết kế.**

```text
Bản thiết kế xe → Class
Chiếc xe thật   → Object
```

### Trả lời phỏng vấn

> Class là khuôn mẫu định nghĩa dữ liệu và các method mà object của class đó sẽ có.

---

## 3. Object

### Object là gì?

**Object là một instance/thể hiện cụ thể của class.**

Ví dụ:

```cpp
Student s1;
Student s2;
```

`Student` là class.

`s1`, `s2` là hai object.

Ta có thể:

```cpp
s1.id = 101;
s1.name = "An";

s2.id = 102;
s2.name = "Linh";
```

Mặc dù cùng một class, mỗi object có dữ liệu riêng:

```text
Student
   |
   +---- s1
   |     id = 101
   |     name = An
   |
   +---- s2
         id = 102
         name = Linh
```

### Cách nhớ

> **Class = khuôn. Object = sản phẩm được tạo từ khuôn.**

### Trả lời phỏng vấn

> Object là một instance của class. Mỗi object có trạng thái dữ liệu riêng và có thể gọi các method được định nghĩa trong class.

---

## 4. Method

### Method là gì?

Method là **hàm nằm trong class**, dùng để mô tả hành vi của object.

Ví dụ:

```cpp
class Calculator
{
public:
    int add(int a, int b)
    {
        return a + b;
    }
};
```

Tạo object:

```cpp
Calculator calc;
int result = calc.add(5, 3);
```

Quá trình có thể hiểu:

```text
calc.add(5,3)
      ↓
a = 5, b = 3
      ↓
a + b
      ↓
return 8
```

Method:

- có thể nhận tham số;
- có thể trả về giá trị;
- cũng có thể không trả về giá trị (`void`);
- dùng để mô tả hành vi của object.

### Cách nhớ

```text
Attribute = object CÓ GÌ
Method    = object LÀM GÌ
```

Ví dụ:

```text
Car

Attribute:
speed

Method:
start()
stop()
```

### Trả lời phỏng vấn

> Method là hàm thành viên của class, dùng để định nghĩa hành vi hoặc xử lý dữ liệu của object.

---

## 5. Constructor

### Constructor là gì?

Constructor là một **hàm đặc biệt dùng để khởi tạo object**.

Trong C++:

- tên constructor giống tên class;
- không có kiểu trả về;
- được gọi tự động khi object được tạo.

Ví dụ:

```cpp
class Student
{
public:
    int id;
    string name;

    Student(int i, string n)
    {
        id = i;
        name = n;
    }
};
```

Khi:

```cpp
Student s1(101, "An");
```

thì có thể hiểu:

```text
Tạo s1
   ↓
Constructor Student(...) được gọi
   ↓
id = 101
name = "An"
   ↓
Object được khởi tạo
```

Một class có thể có nhiều constructor với tham số khác nhau.

Ví dụ:

```cpp
class Student
{
public:
    Student()
    {
    }

    Student(int id)
    {
    }

    Student(int id, string name)
    {
    }
};
```

Đây chính là **constructor overloading**.

### Cách nhớ

> Constructor = hàm chạy khi object được tạo để thiết lập trạng thái ban đầu.

### Trả lời phỏng vấn

> Constructor là hàm đặc biệt của class, được gọi tự động khi object được tạo và thường dùng để khởi tạo dữ liệu cho object.

---

## 6. Inheritance – Kế thừa

Inheritance cho phép **class con kế thừa thuộc tính và method của class cha**.

Ví dụ:

```cpp
class Animal
{
public:
    void eat()
    {
        cout << "eat";
    }
};

class Dog : public Animal
{
public:
    void bark()
    {
        cout << "bark";
    }
};
```

`Dog` có:

```text
eat()   ← kế thừa từ Animal
bark()  ← của riêng Dog
```

Do đó:

```cpp
Dog d;
d.eat();
d.bark();
```

Quan hệ này thường gọi là:

> **IS-A**

Ví dụ:

```text
Dog IS-A Animal
```

Lợi ích:

- tái sử dụng code;
- tránh viết lại;
- class con có thể mở rộng class cha;
- hỗ trợ polymorphism.

### Cách nhớ

> **Inheritance = class con lấy lại những gì class cha đã có.**

### Trả lời phỏng vấn

> Inheritance cho phép class con kế thừa dữ liệu và method từ class cha để tái sử dụng và mở rộng code.

---

## 7. Polymorphism – Đa hình

Polymorphism nghĩa là:

> **Một hành động có thể có nhiều cách thực hiện tùy object.**

Có hai loại chính.

### Compile-time Polymorphism

Thường thông qua **overloading**.

Ví dụ:

```cpp
int add(int a, int b);
int add(int a, int b, int c);
double add(double a, double b);
```

Cùng tên:

```text
add()
```

nhưng khác tham số.

Compiler quyết định gọi hàm nào khi compile.

### Runtime Polymorphism

Thông qua **overriding**.

Ý tưởng:

```text
Animal
  |
  +-- Dog
  |
  +-- Cat
```

Cùng hành động:

```text
sound()
```

nhưng:

```text
Dog → gâu gâu
Cat → meo meo
```

Trong C++ thường kết hợp với `virtual`:

```cpp
class Animal
{
public:
    virtual void sound()
    {
        cout << "Animal";
    }
};

class Dog : public Animal
{
public:
    void sound() override
    {
        cout << "Dog";
    }
};
```

### Nhớ nhanh

```text
Overloading
→ cùng tên
→ khác tham số
→ compile time

Overriding
→ class con ghi đè method class cha
→ runtime
```

### Trả lời phỏng vấn

> Polymorphism cho phép cùng một interface hoặc lời gọi hàm nhưng có hành vi khác nhau tùy đối tượng. Compile-time thường dùng overloading, runtime thường dùng virtual function và overriding.

---

## 8. Encapsulation – Đóng gói

Encapsulation nghĩa là:

> **Đóng dữ liệu và các method xử lý dữ liệu vào trong class, đồng thời hạn chế truy cập trực tiếp dữ liệu.**

Ví dụ:

```cpp
class Student
{
private:
    int id;

public:
    void setId(int value)
    {
        id = value;
    }

    int getId()
    {
        return id;
    }
};
```

Bên ngoài không được:

```cpp
Student s;
// s.id = 10;   // không được vì private
```

Mà truy cập thông qua method:

```cpp
s.setId(10);
cout << s.getId();
```

Cơ chế:

```text
Bên ngoài
    |
    | setId()
    v
+----------------+
| Student        |
|                |
| private:       |
|   id           |
|                |
| public:        |
|   setId()      |
|   getId()      |
+----------------+
```

Lợi ích:

- bảo vệ dữ liệu;
- hạn chế truy cập trái phép;
- kiểm soát cách dữ liệu bị thay đổi;
- code dễ bảo trì hơn.

### Cách nhớ

> **Encapsulation = giấu dữ liệu bên trong, bên ngoài thao tác thông qua interface.**

### Trả lời phỏng vấn

> Encapsulation là đóng gói dữ liệu và method trong class, đồng thời giới hạn truy cập trực tiếp vào dữ liệu, thường bằng `private` và cung cấp public method để thao tác.

---

## 9. Abstraction – Trừu tượng

- Abstraction là ẩn chi tiết xử lý bên trong và chỉ cung cấp chức năng cần thiết cho người dùng.

---

## 10. Sơ đồ tổng hợp dễ nhớ

```text
                    OOP
                     |
        +------------+------------+
        |                         |
      Class                    Object
    Khuôn mẫu              Thể hiện của class
        |
        +---- Attribute → dữ liệu
        |
        +---- Method    → hành vi
        |
        +---- Constructor
              khởi tạo object

4 khái niệm thường gặp
        |
        +---- Encapsulation
        |     Đóng gói / bảo vệ dữ liệu
        |
        +---- Inheritance
        |     Class con kế thừa class cha
        |
        +---- Polymorphism
        |     Một interface, nhiều hành vi
        |
        +---- Abstraction
              Trừu tượng
```

</details>

<details>
<summary><strong>Câu 18: Overloading và overriding khác nhau như thế nào?</strong></summary>

**Trả lời phỏng vấn:** Overloading là nhiều hàm cùng tên nhưng khác danh sách tham số, được chọn lúc biên dịch. Overriding là class con định nghĩa lại hàm `virtual` của class cha, được chọn lúc chạy.

</details>

<details>
<summary><strong>Câu 19: Shallow copy và deep copy khác nhau như thế nào?</strong></summary>

**Trả lời phỏng vấn:** Shallow copy sao chép trực tiếp các thành viên; nếu có con trỏ, hai đối tượng sẽ cùng trỏ đến một vùng nhớ. Deep copy tạo vùng nhớ riêng và sao chép cả dữ liệu được trỏ tới.

</details>

<details>
<summary><strong>Câu 20: Rule of Three trong C++ là gì?</strong></summary>

**Trả lời phỏng vấn:** Nếu class tự quản lý tài nguyên và cần tự viết destructor, thì thường cũng cần copy constructor và copy assignment operator. Ba hàm này giúp sao chép và giải phóng tài nguyên đúng cách, tránh memory leak và double free.

</details>

<details>
<summary><strong>Câu 21: RAII trong C++ là gì?</strong></summary>

**Trả lời phỏng vấn:** RAII gắn tài nguyên với vòng đời object: constructor nhận tài nguyên, destructor tự động giải phóng khi object hết thời gian sống. Nhờ đó hạn chế rò rỉ tài nguyên, kể cả khi hàm kết thúc sớm hoặc có exception.

</details>

<details>
<summary><strong>Câu 22: `unique_ptr`, `shared_ptr` và `weak_ptr` khác nhau như thế nào?</strong></summary>

**Trả lời phỏng vấn:** `unique_ptr` có một chủ sở hữu và không được copy. `shared_ptr` cho phép nhiều chủ sở hữu thông qua bộ đếm tham chiếu. `weak_ptr` không sở hữu tài nguyên, dùng để quan sát object do `shared_ptr` quản lý và tránh vòng tham chiếu.

</details>

<details>
<summary><strong>Câu 23: Move semantics là gì? `std::move` có thực sự di chuyển dữ liệu không?</strong></summary>

**Trả lời phỏng vấn:** Move semantics chuyển quyền sở hữu tài nguyên thay vì sao chép toàn bộ dữ liệu. `std::move` không tự di chuyển; nó chỉ chuyển biểu thức thành rvalue để move constructor hoặc move assignment có thể được chọn.

</details>

<details>
<summary><strong>Câu 24: Lvalue và rvalue khác nhau như thế nào?</strong></summary>

**Trả lời phỏng vấn:** Lvalue đại diện cho object có danh tính và vị trí xác định. Rvalue thường là giá trị tạm thời, phù hợp để chuyển tài nguyên bằng move semantics.

</details>

<details>
<summary><strong>Câu 25: Vì sao destructor class cha cần là `virtual` khi dùng đa hình?</strong></summary>

**Trả lời phỏng vấn:** Destructor class cha cần `virtual` để khi xóa object class con qua con trỏ class cha, cả destructor class con và class cha đều được gọi, giúp giải phóng đầy đủ tài nguyên.

</details>

<details>
<summary><strong>Câu 26: hàm ảo ,Hàm thuần ảo và abstract class là gì?</strong></summary>

**Trả lời phỏng vấn:** 
- **Hàm ảo:** Hàm ảo là hàm khai báo bằng virtual ở class cha , cho phép class con override và hỗ trợ đa hình runtime.
- Đa hình runtime nghĩa là: chương trình quyết định sẽ gọi hàm nào khi chương trình đang chạy, dựa trên object thực tế.
-  Khi dùng con trỏ/reference của class cha trỏ tới object class con, chương trình sẽ gọi hàm override của class con.
```c++
class A {
public:
    virtual void f() {}
};

class B : public A {
public:
    void f() override {}
};

A* p = new B();
p->f();   // gọi B::f()sound();

//A* là con trỏ class cha, nhưng đang trỏ tới object B → nhờ virtual nên gọi B::f().
```

- **Hàm thuần ảo:** là hàm `virtual` khai báo bằng `= 0`.Class con phải override hàm thuần ảo nếu muốn tạo object.(class cha đưa ra yêu cầu, class con phải tự cài đặt cách thực hiện.)

```c++
class Animal {
public:
    virtual void sound() = 0;
};
class Dog : public Animal {
public:
    void sound() override {
        cout << "Woof";
    }
};
```

- **Abstract class:** là class có ít nhất một hàm thuần ảo  và không thể tạo object trực tiếp từ class này
- Constructor không thể là virtual.
- Destructor nên là virtual ,Vì nếu xóa object class con thông qua con trỏ class cha,  để destructor của class cha và  class con cũng được gọi.


</details>

<details>
<summary><strong>Câu 27: Object slicing là gì?</strong></summary>

**Trả lời phỏng vấn:** Object slicing xảy ra khi copy hoặc truyền object class con theo giá trị vào object class cha, làm mất phần dữ liệu và hành vi riêng của class con.

</details>

<details>
<summary><strong>Câu 28: `static_cast` và `dynamic_cast` khác nhau như thế nào?</strong></summary>

**Trả lời phỏng vấn:** `static_cast` được kiểm tra khi biên dịch nhưng không kiểm tra kiểu thật của object lúc chạy. `dynamic_cast` kiểm tra kiểu lúc runtime, thường dùng khi ép kiểu trong hệ thống kế thừa đa hình.

</details>

<details>
<summary><strong>Câu 29: Undefined, unspecified và implementation-defined behavior khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** Undefined behavior là hành vi mà chuẩn không đảm bảo kết quả. Unspecified behavior cho phép một trong nhiều kết quả hợp lệ nhưng compiler không cần ghi rõ lựa chọn. Implementation-defined behavior do compiler hoặc nền tảng lựa chọn và phải có tài liệu mô tả.

</details>

<details>
<summary><strong>Câu 30: Rule of Five và Rule of Zero là gì?</strong></summary>

**Trả lời phỏng vấn:** Rule of Five là Rule of Three cộng thêm move constructor và move assignment. Rule of Zero khuyên dùng các kiểu tự quản lý tài nguyên để không phải tự viết năm hàm đặc biệt này.

</details>

<details>
<summary><strong>Câu 31: `const` và `constexpr` khác nhau như thế nào?</strong></summary>

**Trả lời phỏng vấn:** `const` nghĩa là giá trị không được thay đổi sau khi khởi tạo, nhưng giá trị khởi tạo có thể chỉ biết lúc runtime. `constexpr` yêu cầu giá trị có thể được tính tại compile time và đồng thời cũng là `const`.

</details>

<details>
<summary><strong>Câu 32: Từ khóa `inline` có ý nghĩa gì?</strong></summary>

**Trả lời phỏng vấn:** `inline` cho phép định nghĩa hàm giống nhau trong nhiều translation unit, nên thường dùng khi định nghĩa hàm trong header. Nó không bắt buộc chèn mã tại nơi gọi; compiler tự quyết định việc tối ưu này.

</details>

<details>
<summary><strong>Câu 33: Macro và hàm `inline` khác nhau như thế nào?</strong></summary>

**Trả lời phỏng vấn:** Macro được tiền xử lý thay thế văn bản trước khi biên dịch, không kiểm tra kiểu và dễ gây lỗi do tham số bị đánh giá nhiều lần. Hàm `inline` vẫn là hàm C++ bình thường, có kiểm tra kiểu và phạm vi rõ ràng; compiler tự quyết định có chèn mã hay không.

</details>

<details>
<summary><strong>Câu 34: Template trong C++ là gì?</strong></summary>

**Trả lời phỏng vấn:** Template là khuôn mẫu để tạo hàm hoặc class cho nhiều kiểu dữ liệu. Compiler sinh phiên bản phù hợp cho kiểu được sử dụng, nên đây là một dạng đa hình tại compile time.

</details>

<details>
<summary><strong>Câu 35: `new/delete` và `malloc/free` khác nhau như thế nào?</strong></summary>

**Trả lời phỏng vấn:** Cả hai đều cấp phát lúc runtime. `new` cấp phát và gọi constructor; `delete` gọi destructor rồi giải phóng. `malloc()` và `free()` chỉ quản lý vùng nhớ thô. Không được dùng lẫn các cặp này.

</details>

<details>
<summary><strong>Câu 36: Mảng và con trỏ khác nhau như thế nào?</strong></summary>

**Trả lời phỏng vấn:** 
- Mảng (array): là một vùng nhớ chứa nhiều phần tử cùng kiểu, nằm liên tiếp nhau.
- Con trỏ (pointer): là biến dùng để lưu địa chỉ của một biến hoặc vùng nhớ khác.
- Tên mảng  là  con trỏ tới phần tử đầu tiên của mảng
```cpp
int arr[3] = {10, 20, 30};
int *p = arr;      // tương đương với:
int *p = &arr[0];
```

</details>

<details>
<summary><strong>Câu 37: `sizeof(mảng)` và `sizeof(con_trỏ)` khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** `sizeof(mảng)` trả về tổng số byte của toàn bộ mảng. `sizeof(con_trỏ)` chỉ trả về kích thước của bản thân con trỏ, không liên quan đến số phần tử được trỏ tới.

</details>

<details>
<summary><strong>Câu 38: Đa hình compile time và runtime khác nhau như thế nào?</strong></summary>

**Trả lời phỏng vấn:** Đa hình compile time được quyết định khi biên dịch, thường qua overloading và template. Đa hình runtime được quyết định khi chương trình chạy, thông qua hàm `virtual` và overriding.

</details>

<details>
<summary><strong>Câu 39: `vtable` và `vptr` là gì?</strong></summary>

**Trả lời phỏng vấn:** `vtable` thường là bảng chứa địa chỉ các hàm virtual. `vptr` thường là con trỏ ẩn trong object, trỏ đến `vtable` tương ứng. Đây là cách triển khai phổ biến, không phải yêu cầu bắt buộc của chuẩn C++.

</details>

<details>
<summary><strong>Câu 40: Vì sao không nên mong đợi đa hình khi gọi hàm `virtual` trong constructor hoặc destructor?</strong></summary>

**Trả lời phỏng vấn:** Trong constructor, phần class con chưa được tạo hoàn chỉnh; trong destructor, phần class con đã bị hủy. Vì vậy, lời gọi `virtual` chỉ chạy phiên bản của class đang được tạo hoặc hủy, không gọi bản override của class con.

---

</details>

<details>
<summary><strong>Câu 41: `static` trong C/C++ có những cách sử dụng nào?</strong></summary>

**Trả lời phỏng vấn:** `static` ngoài hàm giới hạn symbol trong file hiện tại; `static` local chỉ khởi tạo một lần và giữ giá trị giữa các lần gọi hàm; `static` member trong class được dùng chung cho tất cả object của class.Ưu điểm là tiết kiệm bộ nhớ và phù hợp để lưu trạng thái chung. Nhược điểm là hoạt động giống global state, khó kiểm soát và cần đồng bộ khi sử dụng đa luồng.
| Loại | Nội dung | Giải thích |
|---|---|---|
| Ưu điểm | Dùng chung dữ liệu | Tất cả object cùng sử dụng một biến duy nhất |
| Ưu điểm | Tiết kiệm bộ nhớ | Không tạo một bản riêng cho từng object |
| Ưu điểm | Truy cập không cần object | Có thể truy cập bằng `TênClass::tênBiến` |
| Ưu điểm | Quản lý thông tin chung | Phù hợp để đếm số object hoặc lưu trạng thái chung của class |
| Nhược điểm | Hoạt động giống biến global | Có thể bị thay đổi từ nhiều nơi nên khó kiểm soát |
| Nhược điểm | Khó kiểm thử | Các bài test có thể ảnh hưởng lẫn nhau vì cùng dùng chung dữ liệu |
| Nhược điểm | Không lưu dữ liệu riêng | Không phù hợp nếu mỗi object cần một giá trị khác nhau |
| Nhược điểm | Có thể xảy ra data race | Nhiều thread cùng thay đổi thì cần `mutex` hoặc `atomic` bảo vệ |
| Nhược điểm | Tồn tại lâu | Biến thường tồn tại trong suốt thời gian chương trình chạy |
ví dụ:

```c++
class Test {
public:
    static int x;
};

int Test::x = 0;

int main() {
    Test a;
    Test b;

    a.x = 10;

    cout << b.x;  // Kết quả: 10
}
```



# Singleton là một design pattern(giải pháp thiết kế) đảm bảo:
👉 Trong chương trình chỉ tồn tại duy nhất 1 object của class.
| Tiêu chí | Static data member | Singleton |
|---|---|---|
| Cái duy nhất | Một biến của class | Một object của class |
| Số lượng | Mỗi static member chỉ có một bản | Class chỉ cho phép sử dụng một object |
| Phạm vi chia sẻ | Chia sẻ một giá trị | Chia sẻ toàn bộ trạng thái và hành vi của object |
| Cách truy cập | `ClassName::member` | `ClassName::GetInstance()` |
| Ví dụ | `static int count` | `Singleton Logger` |

> Ưu điểm là dễ quản lý tài nguyên chung; nhược điểm là tạo global state, tăng phụ thuộc, khó kiểm thử và cần đồng bộ khi dùng đa luồng.>
```c++
ví dụ:
class Logger {
private:
    Logger() {} // không cho tạo từ bên ngoài

public:
    static Logger& getInstance() {
        static Logger instance; // chỉ tạo 1 lần
        return instance;
    }
};

int main() {
    Logger& a = Logger::getInstance();
    Logger& b = Logger::getInstance();
}

```

</details>

<details>
<summary><strong>Câu 42: `extern` trong C/C++ dùng để làm gì?</strong></summary>

**Trả lời phỏng vấn:** `extern` dùng để khai báo rằng biến hoặc hàm đã được định nghĩa ở nơi khác, thường là file khác, để file hiện tại có thể sử dụng.

</details>

<details>
<summary><strong>Câu 43: Declaration và definition khác nhau như thế nào?</strong></summary>

**Trả lời phỏng vấn:** Declaration giới thiệu tên và kiểu cho compiler. Definition thực sự tạo object hoặc cung cấp phần cài đặt và thường cấp vùng nhớ.

</details>

<details>
<summary><strong>Câu 44: `const` khác gì với `#define`?</strong></summary>

**Trả lời phỏng vấn:** `const` là hằng có kiểu dữ liệu và được compiler kiểm tra kiểu. `#define` là macro được preprocessor thay thế trước khi biên dịch và không có kiểu dữ liệu.

</details>

<details>
<summary><strong>Câu 45: `typedef` và `using` dùng để làm gì?</strong></summary>

**Trả lời phỏng vấn:** Cả `typedef` và `using` đều dùng để tạo alias cho kiểu dữ liệu. `using` là cú pháp hiện đại hơn trong C++ và thuận tiện hơn với template.

</details>

<details>
<summary><strong>Câu 46: `enum` là gì? `enum class` khác `enum` thường như thế nào?</strong></summary>

**Trả lời phỏng vấn:** `enum` dùng để định nghĩa một tập các giá trị có tên. `enum class` an toàn hơn vì các giá trị nằm trong phạm vi của enum và không tự động chuyển sang `int`.

</details>

<details>
<summary><strong>Câu 47: `namespace` trong C++ dùng để làm gì?</strong></summary>

**Trả lời phỏng vấn:** `namespace` dùng để tổ chức code và tránh xung đột tên giữa biến, hàm hoặc class. `using namespace std;` cho phép dùng thành phần trong `std` mà không cần viết tiền tố `std::`.

</details>

<details>
<summary><strong>Câu 48: `nullptr` khác gì với `NULL`?</strong></summary>

**Trả lời phỏng vấn:** `NULL` thường là macro đại diện cho giá trị null kiểu số nguyên, còn `nullptr` là null pointer literal có kiểu riêng `std::nullptr_t`. Trong C++11 trở lên nên dùng `nullptr`.

</details>

<details>
<summary><strong>Câu 49: Dangling pointer là gì?</strong></summary>

**Trả lời phỏng vấn:** Dangling pointer là con trỏ vẫn giữ địa chỉ của vùng nhớ hoặc object đã không còn hợp lệ, ví dụ sau khi `delete`/`free` hoặc object hết lifetime.

</details>

<details>
<summary><strong>Câu 50: Interface là gì?</strong></summary>

**Trả lời phỏng vấn:** Interface là bản hợp đồng quy định class phải cung cấp những chức năng nào. C++ không có từ khóa interface, nên thường dùng abstract class chứa các pure virtual function để tạo interface.
ví dụ:
 ta muốn các class con đều phải có hàng read
</details>

---

### STL – các câu trọng tâm phỏng vấn

<details>
<summary><strong>Câu 51: STL là gì?</strong></summary>

**Trả lời phỏng vấn:** STL là thư viện chuẩn C++ cung cấp các cấu trúc dữ liệu và thuật toán dùng sẵn. Ba thành phần chính thường được nhắc đến là Container, Algorithm và Iterator.
### 1. Container là gì?

**Container** là cấu trúc dùng để lưu trữ và quản lý một tập hợp các phần tử. Mỗi container có cách tổ chức dữ liệu, tốc độ truy cập, chèn, xóa và tìm kiếm khác nhau.

#### Nhóm Sequence Container

Các phần tử được tổ chức theo thứ tự tuyến tính.

| Container | Cấu tạo / đặc điểm | Khi nào dùng? |
|---|---|---|
| `array` | Mảng có kích thước cố định; phần tử liên tiếp | Biết kích thước ngay từ compile time |
| `vector` | Mảng động; phần tử liên tiếp | Cần truy cập nhanh theo index và chủ yếu thêm ở cuối |
| `deque` | Hàng đợi hai đầu; không bảo đảm toàn bộ phần tử liên tiếp | Cần thêm hoặc xóa nhanh ở cả đầu và cuối |
| `list` | Danh sách liên kết kép | Cần chèn hoặc xóa thường xuyên tại vị trí đã biết qua iterator |
| `forward_list` | Danh sách liên kết đơn | Cần danh sách một chiều và muốn giảm chi phí bộ nhớ so với `list` |

#### Nhóm Associative Container

Dữ liệu được sắp xếp theo key; thường được triển khai bằng cây cân bằng.

| Container | Dữ liệu lưu trữ | Trùng lặp | Khi nào dùng? |
|---|---|---|---|
| `set` | Value | Không trùng | Cần value duy nhất và có thứ tự |
| `multiset` | Value | Được trùng | Cần value có thứ tự và cho phép trùng |
| `map` | Key–value | Key không trùng | Cần tra cứu theo key và duyệt theo thứ tự key |
| `multimap` | Key–value | Key được trùng | Một key cần liên kết với nhiều value và vẫn có thứ tự |

#### Nhóm Unordered Associative Container

Dữ liệu thường được tổ chức bằng hash table và không bảo đảm thứ tự.

| Container | Dữ liệu lưu trữ | Trùng lặp | Khi nào dùng? |
|---|---|---|---|
| `unordered_set` | Value | Không trùng | Cần kiểm tra value tồn tại nhanh, không cần thứ tự |
| `unordered_multiset` | Value | Được trùng | Cần lưu value trùng và không cần thứ tự |
| `unordered_map` | Key–value | Key không trùng | Cần tìm kiếm theo key nhanh trung bình, không cần thứ tự |
| `unordered_multimap` | Key–value | Key được trùng | Một key có nhiều value và không cần thứ tự |

#### Nhóm Container Adapter

Container adapter cung cấp một giao diện sử dụng đặc biệt trên một container bên dưới.

| Container adapter | Nguyên tắc | Khi nào dùng? |
|---|---|---|
| `stack` | LIFO – vào sau ra trước | Undo/redo, DFS, xử lý theo thứ tự ngược |
| `queue` | FIFO – vào trước ra trước | Task queue, message queue, xử lý tuần tự |
| `priority_queue` | Phần tử có độ ưu tiên cao nhất được lấy trước | Lập lịch, xử lý công việc theo độ ưu tiên |

**Trả lời ngắn về Container:**

> Container là cấu trúc STL dùng để lưu và quản lý một tập hợp phần tử. Việc chọn container phụ thuộc vào yêu cầu truy cập, tìm kiếm, chèn, xóa và thứ tự dữ liệu.

### 2. Algorithm là gì?

**Algorithm** là các hàm có sẵn dùng để xử lý dữ liệu, chẳng hạn tìm kiếm, sắp xếp, đếm, sao chép hoặc biến đổi. Algorithm thường không làm việc trực tiếp với một container cụ thể mà làm việc trên một khoảng iterator dạng `[first, last)`.

| Nhóm | Algorithm phổ biến | Tác dụng |
|---|---|---|
| Tìm kiếm | `find`, `find_if`, `binary_search` | Tìm value hoặc phần tử thỏa điều kiện |
| Đếm và kiểm tra | `count`, `count_if`, `all_of`, `any_of`, `none_of` | Đếm hoặc kiểm tra điều kiện trên các phần tử |
| Sắp xếp | `sort`, `stable_sort`, `partial_sort` | Sắp xếp toàn bộ hoặc một phần dữ liệu |
| Xác định vị trí | `lower_bound`, `upper_bound`, `equal_range` | Tìm vị trí trong một range đã sắp xếp |
| Sao chép và di chuyển | `copy`, `copy_if`, `move` | Sao chép hoặc di chuyển phần tử sang vùng khác |
| Biến đổi | `transform`, `replace`, `replace_if` | Thay đổi hoặc tạo dữ liệu từ các phần tử hiện có |
| Loại bỏ logic | `remove`, `remove_if`, `unique` | Dồn các phần tử cần giữ lại; thường kết hợp `erase()` để xóa thật khỏi container |
| Giá trị nhỏ/lớn | `min_element`, `max_element`, `minmax_element` | Tìm phần tử nhỏ nhất hoặc lớn nhất |
| Duyệt | `for_each` | Thực hiện một thao tác trên từng phần tử |
| Tính toán | `accumulate`, `iota` | Tính tổng hoặc tạo dãy giá trị; thuộc header `<numeric>` |
| Heap | `make_heap`, `push_heap`, `pop_heap`, `sort_heap` | Tạo và thao tác với cấu trúc heap |

**Trả lời ngắn về Algorithm:**

> Algorithm là các hàm xử lý dữ liệu dùng sẵn của STL. Chúng thường nhận một range iterator nên có thể tái sử dụng với nhiều container khác nhau.

### 3. Iterator là gì?

**Iterator** là đối tượng dùng để xác định vị trí và duyệt phần tử trong container. Iterator tạo cầu nối giữa Container và Algorithm.

| Loại iterator | Khả năng chính | Ví dụ thường gặp |
|---|---|---|
| Input iterator | Đọc và tiến về phía trước | Iterator đọc dữ liệu đầu vào |
| Output iterator | Ghi và tiến về phía trước | `back_inserter` |
| Forward iterator | Đọc/ghi, đi một chiều nhiều lần | `forward_list`, `unordered_map` |
| Bidirectional iterator | Đi tới và đi lùi | `list`, `set`, `map` |
| Random-access iterator | Nhảy đến vị trí bất kỳ, hỗ trợ phép toán khoảng cách | `vector`, `deque` |
| Contiguous iterator | Random access và các phần tử nằm liên tiếp trong bộ nhớ | `array`, `vector` |

**Trả lời ngắn về Iterator:**

# Iterator là abstraction dùng để truy cập và duyệt phần tử trong container. Algorithm nhận iterator để có thể hoạt động với nhiều loại container.Hiểu đơn giản Iterator là một biến dùng để xác định vị trí và duyệt qua các phần tử trong container.

### Mối quan hệ giữa ba thành phần

> **Container lưu dữ liệu → Iterator truy cập dữ liệu → Algorithm xử lý dữ liệu.**

</details>

<details>
<summary><strong>Câu 52: `std::vector` là gì?</strong></summary>

**Trả lời phỏng vấn:** `std::vector` là mảng động, có thể thay đổi số lượng phần tử trong runtime. Các phần tử được lưu liên tiếp trong bộ nhớ nên truy cập theo index nhanh.

</details>

<details>
<summary><strong>Câu 53: `std::vector` khác mảng thông thường như thế nào?</strong></summary>

**Trả lời phỏng vấn:** Mảng có kích thước cố định, còn `vector` có thể thay đổi kích thước trong runtime. Cả hai đều lưu phần tử liên tiếp trong bộ nhớ.

</details>

<details>
<summary><strong>Câu 54: `std::vector` và `std::list` khác nhau như thế nào?</strong></summary>

**Trả lời phỏng vấn:** `vector` lưu liên tiếp trong bộ nhớ nên truy cập ngẫu nhiên theo index nhanh. `list` là danh sách liên kết kép, phù hợp khi cần chèn/xóa thường xuyên ở vị trí đã biết qua iterator nhưng không hỗ trợ truy cập trực tiếp bằng index.

</details>

<details>
<summary><strong>Câu 55: `std::map` là gì?</strong></summary>

**Trả lời phỏng vấn:** `std::map` lưu dữ liệu dạng key-value. Mỗi key là duy nhất và các phần tử được sắp xếp theo key.

</details>

<details>
<summary><strong>Câu 56: `std::map` và `std::unordered_map` khác nhau như thế nào?</strong></summary>

**Trả lời phỏng vấn:** Cả hai đều lưu key-value với key duy nhất. `map` có thứ tự theo key, còn `unordered_map` không đảm bảo thứ tự và thường dùng hash table nên tìm kiếm trung bình nhanh hơn.

</details>

<details>
<summary><strong>Câu 57: `std::set` là gì? Khác `std::map` thế nào?</strong></summary>

**Trả lời phỏng vấn:** `set` lưu các value duy nhất và có thứ tự. `map` lưu theo cặp key-value, còn `set` chỉ lưu value.

</details>

<details>
<summary><strong>Câu 58: `std::multiset` là gì?</strong></summary>

**Trả lời phỏng vấn:** `multiset` giống `set` nhưng cho phép các value bị trùng nhau và vẫn giữ thứ tự.

</details>

<details>
<summary><strong>Câu 59: `std::multimap` là gì?</strong></summary>

**Trả lời phỏng vấn:** `multimap` lưu key-value giống `map` nhưng cho phép nhiều phần tử có cùng key.

</details>

<details>
<summary><strong>Câu 60: `std::unordered_set` là gì?</strong></summary>

**Trả lời phỏng vấn:** `unordered_set` lưu các value duy nhất, không đảm bảo thứ tự và thường dựa trên hash table. Phù hợp khi cần kiểm tra một value có tồn tại nhanh.

</details>

<details>
<summary><strong>Câu 61: `std::unordered_multiset` là gì?</strong></summary>

**Trả lời phỏng vấn:** `unordered_multiset` lưu value, cho phép trùng và không đảm bảo thứ tự.

</details>

<details>
<summary><strong>Câu 62: `std::unordered_multimap` là gì?</strong></summary>

**Trả lời phỏng vấn:** `unordered_multimap` lưu key-value, cho phép trùng key và không đảm bảo thứ tự.

</details>

<details>
<summary><strong>Câu 63: `std::queue` là gì?</strong></summary>

**Trả lời phỏng vấn:** `std::queue` là container adapter hoạt động theo FIFO – First In, First Out. Phần tử vào trước sẽ được lấy ra trước. Thường dùng cho task queue, message queue hoặc request chờ xử lý.

</details>

<details>
<summary><strong>Câu 64: `std::deque` là gì? Khác `vector` thế nào?</strong></summary>

**Trả lời phỏng vấn:** `deque` là double-ended queue, hỗ trợ thêm/xóa hiệu quả ở cả đầu và cuối. `vector` lưu liên tiếp toàn bộ và phù hợp khi chủ yếu truy cập index hoặc thêm cuối; `deque` không đảm bảo toàn bộ phần tử nằm liên tiếp.

</details>

<details>
<summary><strong>Câu 65: `std::stack` là gì?</strong></summary>

**Trả lời phỏng vấn:** `std::stack` là container adapter hoạt động theo LIFO – Last In, First Out. Phần tử thêm sau sẽ được lấy ra trước. Thường dùng cho undo/redo, DFS, kiểm tra ngoặc hoặc các bài toán cần xử lý theo thứ tự ngược.

</details>

<details>
<summary><strong>Câu 66: `std::sort()` dùng để làm gì?</strong></summary>

**Trả lời phỏng vấn:** `std::sort()` dùng để sắp xếp một khoảng phần tử. Nó thường dùng với `vector`, `array`, `deque` hoặc các range hỗ trợ random-access iterator. `map` đã tự sắp xếp theo key nên không dùng `std::sort()` trực tiếp.

</details>

<details>
<summary><strong>Câu 67: `std::find()` dùng để làm gì?</strong></summary>

**Trả lời phỏng vấn:** `std::find()` dùng để tìm một giá trị trong một khoảng `[begin, end)`. Nếu tìm thấy, nó trả về iterator tới phần tử; nếu không tìm thấy, kết quả bằng `end()`.

</details>

<details>
<summary><strong>Câu 68: Iterator trong STL là gì?</strong></summary>

**Trả lời phỏng vấn:** Iterator là đối tượng dùng để truy cập và duyệt các phần tử trong container STL, hoạt động tương tự con trỏ.

</details>

<details>
<summary><strong>Câu 69: `begin()` và `end()` khác nhau như thế nào?</strong></summary>

**Trả lời phỏng vấn:** `begin()` trả về iterator tới phần tử đầu tiên. `end()` trả về iterator tới vị trí ngay sau phần tử cuối cùng, không phải phần tử cuối.

</details>

<details>
<summary><strong>Câu 70: Iterator khác con trỏ thông thường như thế nào?</strong></summary>

**Trả lời phỏng vấn:** Con trỏ là địa chỉ bộ nhớ thực tế. Iterator là abstraction dùng để duyệt container; cách dùng có thể giống con trỏ nhưng khả năng thao tác phụ thuộc loại container.

</details>

<details>
<summary><strong>Câu 71: Iterator invalidation là gì?</strong></summary>

**Trả lời phỏng vấn:** Iterator invalidation xảy ra khi thao tác trên container làm iterator cũ không còn hợp lệ. Với `vector`, reallocation là nguyên nhân rất hay gặp.

</details>

<details>
<summary><strong>Câu 72: Độ phức tạp quan trọng của `std::vector` là gì?</strong></summary>

| Thao tác | Độ phức tạp |
|---|---:|
| Truy cập theo index | `O(1)` |
| Tìm kiếm tuyến tính | `O(n)` |
| Thêm cuối | `O(1)` trung bình |
| Chèn/xóa ở giữa hoặc đầu | `O(n)` |

**Trả lời phỏng vấn:** `vector` truy cập theo index `O(1)`, tìm kiếm `O(n)`, thêm cuối trung bình `O(1)`, còn chèn/xóa ở giữa thường `O(n)` vì phải di chuyển các phần tử phía sau.

</details>

<details>
<summary><strong>Câu 73: Độ phức tạp quan trọng của `std::map` là gì?</strong></summary>

**Trả lời phỏng vấn:** `map` thường được triển khai bằng cây tìm kiếm cân bằng. Tìm kiếm, insert và erase thường có độ phức tạp `O(log n)`.

</details>

<details>
<summary><strong>Câu 74: Độ phức tạp quan trọng của `std::unordered_map` là gì?</strong></summary>

**Trả lời phỏng vấn:** `unordered_map` thường dùng hash table. Tìm kiếm, insert và erase trung bình là `O(1)`, nhưng trường hợp xấu có thể là `O(n)`.

</details>

<details>
<summary><strong>Câu 75: Khi nào nên dùng `map`, khi nào nên dùng `unordered_map`?</strong></summary>

**Trả lời phỏng vấn:** Nếu cần key có thứ tự hoặc cần duyệt theo thứ tự key thì dùng `map`. Nếu không cần thứ tự và ưu tiên tốc độ tìm kiếm trung bình thì dùng `unordered_map`.

</details>

<details>
<summary><strong>Câu 76: Cách nhớ nhanh nhóm `set/map`, `multi` và `unordered`?</strong></summary>

**Trả lời phỏng vấn:**

- `set` → chỉ lưu value.

- `map` → lưu key-value.

- Có `multi` → cho phép trùng.

- Có `unordered` → không đảm bảo thứ tự và thường dùng hash table.

</details>

<details>
<summary><strong>Câu 77: Chọn container STL nào trong các tình huống thường gặp?</strong></summary>

| Container | Định nghĩa / cấu tạo | Tác dụng / Khi nào dùng |
|---|---|---|
| `vector` | Mảng động, phần tử liên tiếp | Cần truy cập nhanh theo index, chủ yếu thêm ở cuối |
| `list` | Danh sách liên kết kép | Cần chèn/xóa thường xuyên ở vị trí đã biết |
| `map` | Key-value, key duy nhất, có thứ tự | Cần tra cứu theo key và cần thứ tự |
| `unordered_map` | Key-value, hash-based | Cần tìm kiếm theo key nhanh, không cần thứ tự |
| `set` | Value duy nhất, có thứ tự | Cần loại bỏ phần tử trùng và giữ thứ tự |
| `multiset` | Value có thể trùng, có thứ tự | Cần lưu nhiều giá trị giống nhau |
| `multimap` | Key-value, key có thể trùng | Một key cần gắn với nhiều value |
| `unordered_set` | Value duy nhất, hash-based | Cần kiểm tra tồn tại nhanh, không cần thứ tự |
| `queue` | FIFO | Task queue, message queue, request chờ xử lý |
| `deque` | Double-ended queue | Cần thêm/xóa nhanh ở cả đầu và cuối |
| `stack` | LIFO | Undo/redo, DFS, xử lý theo thứ tự ngược |

</details>

---

### Ghi nhớ nhanh STL

- `vector` → truy cập index nhanh.

- `list` → chèn/xóa thuận tiện ở vị trí đã biết.

- `map` → key-value + có thứ tự.

- `unordered_map` → key-value + tìm nhanh trung bình + không thứ tự.

- `set` → value duy nhất.

- `queue` → FIFO.

- `stack` → LIFO.

- `deque` → thao tác tốt ở cả hai đầu.

</details>

<details>
<summary><h1>Đa luồng C++ — Câu 78–120</h1></summary>

<details>
<summary><strong>Câu 78: Process là gì, thread là gì và chúng khác nhau như thế nào?</strong></summary>

**Trả lời phỏng vấn:** Process là một chương trình đang chạy, có vùng nhớ và tài nguyên riêng. Thread là đơn vị thực thi bên trong process. Các thread trong cùng process chia sẻ code, biến global và heap, nhưng mỗi thread có stack và trạng thái thực thi riêng. Việc tạo và chuyển đổi thread thường nhẹ hơn process.

**Cách nhớ:** Process chứa tài nguyên, thread thực hiện công việc.

</details>

<details>
<summary><strong>Câu 79: Concurrency và parallelism khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** Concurrency là nhiều công việc cùng tiến triển trong một khoảng thời gian nhưng không nhất thiết chạy cùng một thời điểm. Parallelism là nhiều công việc thực sự chạy đồng thời trên nhiều CPU core.

</details>

<details>
<summary><strong>Câu 80: `std::thread`, `join()` và `detach()` là gì?</strong></summary>

**Trả lời phỏng vấn:** `std::thread` đại diện cho một luồng thực thi. `join()` làm thread hiện tại chờ thread kia kết thúc, còn `detach()` cho thread chạy độc lập. Nếu một object `std::thread` vẫn còn `joinable` khi bị hủy, chương trình gọi `std::terminate()`, nên cần `join()` hoặc `detach()` trước đó.

</details>

<details>
<summary><strong>Câu 81: Context switch là gì?</strong></summary>

**Trả lời phỏng vấn:** Context switch là quá trình CPU tạm dừng một thread hoặc process, lưu trạng thái của nó rồi khôi phục trạng thái của thread hoặc process khác. Quá trình này tạo ra chi phí nên quá nhiều thread có thể làm giảm hiệu năng.

</details>

<details>
<summary><strong>Câu 82: Shared resource và critical section là gì?</strong></summary>

**Trả lời phỏng vấn:** Shared resource là tài nguyên được nhiều thread sử dụng chung, chẳng hạn biến global, object hoặc file. Critical section là đoạn code truy cập tài nguyên chung và thường phải được đồng bộ để tránh lỗi dữ liệu.

</details>

<details>
<summary><strong>Câu 83: Race condition và data race khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** Race condition là lỗi logic khi kết quả phụ thuộc vào thứ tự hoặc thời điểm các thread thực thi. Data race xảy ra khi nhiều thread truy cập cùng một vùng nhớ, có ít nhất một thao tác ghi và không có đồng bộ phù hợp. Trong C++, data race gây undefined behavior.

</details>

<details>
<summary><strong>Câu 84: Mutex là gì?</strong></summary>

**Trả lời phỏng vấn:** Mutex là cơ chế loại trừ lẫn nhau, dùng để bảo vệ tài nguyên dùng chung bằng cách chỉ cho một thread truy cập critical section tại một thời điểm.


# MULTITHREADING SYNCHRONIZATION

## 1. `mutex`

> Cơ chế đồng bộ dùng để bảo vệ tài nguyên dùng chung, đảm bảo chỉ một thread truy cập tại một thời điểm.
>
> **Ví dụ:** Hai thread cùng cập nhật `balance`, `mutex` đảm bảo chỉ một thread sửa tại một thời điểm.
>
> **Dùng để tránh:** Data Race, dữ liệu sai.

---

## 2. `lock()`

> Hàm dùng để khóa `mutex`.
>
> Nếu mutex đang bị thread khác giữ thì thread hiện tại phải chờ.
>
> **Rủi ro:** Quên `unlock()` có thể gây treo hoặc Deadlock.

---

## 3. `unlock()`

> Hàm dùng để mở khóa `mutex`.
>
> Sau khi xử lý xong tài nguyên dùng chung, thread gọi `unlock()` để thread khác có thể truy cập.
>
> **Khuyến nghị:** Ưu tiên `lock_guard` hoặc `unique_lock` để tránh quên `unlock()`.

---

## 4. `lock_guard`

> Class quản lý `mutex` tự động theo RAII.
>
> Tạo object → tự `lock`.
>
> Ra khỏi scope → tự `unlock`.
>
> **Ưu điểm:** Đơn giản, an toàn, tránh quên `unlock()`.
>
> → Dùng khi chỉ cần lock từ đầu đến cuối scope.

---

## 5. `unique_lock`

> Class quản lý `mutex` linh hoạt hơn `lock_guard`.
>
> Có thể `lock`, `unlock`, rồi `lock` lại khi cần.
>
> **Thường dùng với:** `condition_variable`.
>
> → Dùng khi cần quản lý lock linh hoạt.

---

## 6. `condition_variable`

> Cho phép một thread `wait` đến khi thread khác `notify`.
>
> Thường đi với:
>
> `condition_variable + unique_lock + mutex`
>
> **Ví dụ:** Consumer chờ queue có dữ liệu. Producer thêm dữ liệu rồi gọi `notify_one()`.
>
> Khi `wait()`:
>
> - Mutex được tạm `unlock`.
> - Thread chuyển sang chờ.
> - Khi được `notify`, mutex được `lock` lại.
>
> → Thường dùng trong mô hình **Producer - Consumer**.

---

## 7. `shared_mutex`

> Cho phép nhiều thread cùng đọc nhưng chỉ một thread được ghi tại một thời điểm.
>
> **Ví dụ:** Nhiều thread cùng đọc cấu hình, nhưng khi một thread sửa cấu hình thì phải độc quyền.
>
> → **Multiple Readers - Single Writer**.

---

## 8. `shared_lock`

> Wrapper dùng với `shared_mutex` cho thao tác đọc.
>
> Nhiều thread có thể giữ `shared_lock` cùng lúc.
>
> **Ví dụ:** Thread A, B, C cùng đọc danh sách user.
>
> → `shared_lock` = **khóa đọc**.

---

## 9. Exclusive Lock / Write Lock

> Khóa độc quyền dùng khi một thread cần ghi dữ liệu với `shared_mutex`.
>
> Khi Writer giữ lock:
>
> - Thread khác không được đọc.
> - Thread khác không được ghi.
>
> Thường dùng:
>
> `unique_lock<shared_mutex>`
>
> → Exclusive Lock = **khóa ghi**.

---

## 10. `atomic`

> Atomic đảm bảo tại một thời điểm, một thao tác cập nhật biến được thực hiện trọn vẹn, không bị thao tác của thread khác chen vào giữa.
>
> **Ví dụ:** Nhiều thread cùng tăng biến `counter`.
>
> Thường dùng cho:
>
> - Counter
> - Flag
> - Trạng thái đơn giản
>
> **Hạn chế:** Không phù hợp với logic phức tạp gồm nhiều biến.

---

## 11. Race Condition

> Kết quả chương trình phụ thuộc vào thứ tự hoặc timing thực thi của các thread.
>
> **Ví dụ:** A chạy trước cho kết quả khác B chạy trước.
>
> **Hậu quả:**
>
> - Kết quả không ổn định.
> - Bug khó tái hiện.
>
> **Xử lý:**
>
> - Dùng cơ chế đồng bộ phù hợp như `mutex`, `atomic`, `condition_variable`.

---

## 12. Data Race

> Nhiều thread truy cập cùng một vùng dữ liệu, có ít nhất một thread ghi và không được đồng bộ đúng cách.
>
> **Ví dụ:** Hai thread cùng tăng `counter` từ `10`, nhưng kết quả cuối là `11` thay vì `12`.
>
> **Hậu quả:**
>
> - Dữ liệu sai.
> - Mất update.
> - Có thể crash.
> - Trong C++ có thể gây **Undefined Behavior**.
>
> **Xử lý:**
>
> - Dùng `mutex`.
> - Dữ liệu đơn giản có thể dùng `atomic`.
>
> → Data Race = **cùng truy cập dữ liệu, có ghi nhưng không được bảo vệ**.

---

## 13. Deadlock

> Hai hoặc nhiều thread chờ tài nguyên của nhau và không thread nào tiếp tục được.
>
> **Ví dụ:**
>
> Thread A giữ Mutex 1, chờ Mutex 2.
>
> Thread B giữ Mutex 2, chờ Mutex 1.
>
> **Hậu quả:**
>
> - Thread bị treo.
> - Chương trình có thể không phản hồi.
>
> **Xử lý / phòng tránh:**
>
> - Lock mutex theo cùng một thứ tự.
> - Giữ thời gian lock ngắn.
> - Có thể dùng `std::scoped_lock` khi lock nhiều mutex.
>
> → Deadlock = **đứng im chờ nhau**.

---

## 14. Livelock

> Các thread vẫn chạy nhưng liên tục nhường hoặc retry nên không thread nào hoàn thành công việc.
>
> **Ví dụ:** Thread A nhường B, B lại nhường A, lặp lại liên tục.
>
> **Hậu quả:**
>
> - Công việc không hoàn thành.
> - CPU vẫn bị sử dụng.
>
> **Xử lý / phòng tránh:**
>
> - Thêm delay hoặc random backoff.
> - Giới hạn số lần retry.
> - Thiết lập ưu tiên rõ ràng.
>
> → Livelock = **vẫn chạy nhưng không tiến triển**.

---

## 15. Starvation

> Một thread phải chờ rất lâu vì các thread khác liên tục được ưu tiên sử dụng tài nguyên.
>
> **Ví dụ:** Reader liên tục lấy lock khiến Writer mãi không được ghi.
>
> **Hậu quả:**
>
> - Thread chờ quá lâu.
> - Task có thể gần như không được thực hiện.
>
> **Xử lý / phòng tránh:**
>
> - Giữ lock ngắn.
> - Sử dụng cơ chế scheduling hoặc lock công bằng hơn.
>
> → Starvation = **mãi không tới lượt**.

---

## 16. Critical Section

> Đoạn code truy cập hoặc sửa tài nguyên dùng chung và cần được bảo vệ.
>
> **Ví dụ:** Đọc `balance` → cộng tiền → ghi lại `balance`.
>
> **Nếu không bảo vệ:**
>
> - Có thể xảy ra Data Race.
> - Dữ liệu có thể sai.
>
> **Xử lý:**
>
> - Dùng `mutex`.
> - Với dữ liệu đơn giản có thể dùng `atomic`.
> - Giữ Critical Section càng ngắn càng tốt.
>
> → Critical Section = **đoạn code cần được bảo vệ**.

---

# Ghi nhớ nhanh

| Khái niệm | Hiểu đơn giản |
|---|---|
| `mutex` | Khóa bảo vệ tài nguyên |
| `lock()` | Khóa mutex |
| `unlock()` | Mở mutex |
| `lock_guard` | Tự lock, tự unlock |
| `unique_lock` | Lock linh hoạt |
| `condition_variable` | Wait / Notify giữa các thread |
| `shared_mutex` | Nhiều Reader, một Writer |
| `shared_lock` | Khóa đọc |
| Exclusive Lock | Khóa ghi |
| `atomic` | Thao tác đơn giản an toàn giữa thread |
| Race Condition | Kết quả phụ thuộc timing |
| Data Race | Có ghi dữ liệu nhưng không đồng bộ |
| Deadlock | Đứng im chờ nhau |
| Livelock | Vẫn chạy nhưng không tiến triển |
| Starvation | Mãi không tới lượt |
| Critical Section | Đoạn code cần bảo vệ |
</details>

<details>
<summary><strong>Câu 85: `lock_guard`, `unique_lock` và `scoped_lock` khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** Cả ba đều quản lý mutex theo RAII. `lock_guard` đơn giản, khóa khi được tạo và tự mở khóa khi ra khỏi scope. `unique_lock` linh hoạt hơn, cho phép khóa hoặc mở khóa thủ công và dùng được với condition variable. `scoped_lock` phù hợp khi cần khóa nhiều mutex an toàn.

</details>

<details>
<summary><strong>Câu 86: Deadlock là gì và phòng tránh thế nào?</strong></summary>

**Trả lời phỏng vấn:** Deadlock xảy ra khi các thread chờ tài nguyên của nhau vô thời hạn nên không thread nào tiếp tục được. Có thể hạn chế bằng cách thống nhất thứ tự lấy khóa, giữ khóa trong thời gian ngắn và dùng `scoped_lock` khi cần khóa nhiều mutex.

</details>

<details>
<summary><strong>Câu 87: Deadlock, livelock và starvation khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** Deadlock là các thread bị chặn và chờ nhau. Livelock là các thread vẫn hoạt động nhưng liên tục phản ứng với nhau nên không tiến triển. Starvation là một thread không được cấp CPU hoặc tài nguyên trong thời gian dài.

</details>

<details>
<summary><strong>Câu 88: `condition_variable` dùng để làm gì?</strong></summary>

**Trả lời phỏng vấn:** `condition_variable` cho phép một thread ngủ và chờ đến khi điều kiện được thỏa mãn thay vì liên tục kiểm tra gây tốn CPU. Nó thường được dùng cùng mutex và `unique_lock`; thread khác gọi `notify_one()` hoặc `notify_all()` để đánh thức thread đang chờ.

</details>

<details>
<summary><strong>Câu 89: Spurious wakeup là gì? Vì sao cần predicate?</strong></summary>

**Trả lời phỏng vấn:** Spurious wakeup là trường hợp thread thức dậy dù điều kiện chưa thỏa mãn. Vì vậy, khi dùng condition variable phải luôn kiểm tra lại điều kiện bằng predicate sau khi thread thức dậy.

</details>

<details>
<summary><strong>Câu 90: Producer–consumer là bài toán gì?</strong></summary>

**Trả lời phỏng vấn:** Producer–consumer là mô hình trong đó một nhóm thread tạo dữ liệu và nhóm khác xử lý dữ liệu thông qua hàng đợi dùng chung. Hàng đợi thường được bảo vệ bằng mutex, còn condition variable được dùng để thông báo khi có dữ liệu hoặc có chỗ trống.

</details>

<details>
<summary><strong>Câu 91: `std::atomic` là gì?</strong></summary>

**Trả lời phỏng vấn:** `std::atomic` cho phép nhiều thread thao tác trên một biến bằng các phép toán nguyên tử mà không gây data race. Nó phù hợp với dữ liệu và thao tác đơn giản như counter hoặc flag.

</details>

<details>
<summary><strong>Câu 92: Atomic và mutex khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** Atomic phù hợp với thao tác đơn giản trên một biến. Mutex phù hợp khi cần bảo vệ nhiều dữ liệu hoặc nhiều thao tác phải được thực hiện như một khối thống nhất. Một biến atomic không tự động làm cho toàn bộ thuật toán trở thành thread-safe.

</details>

<details>
<summary><strong>Câu 93: `volatile` và `atomic` khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** `volatile` yêu cầu compiler không loại bỏ tùy ý các lần truy cập nhưng không bảo đảm atomic hoặc thread-safe. `std::atomic` cung cấp thao tác nguyên tử và cơ chế đồng bộ giữa các thread. Không dùng `volatile` để thay thế `atomic` trong đa luồng.

</details>

<details>
<summary><strong>Câu 94: Memory ordering và happens-before là gì?</strong></summary>

**Trả lời phỏng vấn:** Memory ordering quy định cách các thao tác bộ nhớ được quan sát giữa các thread. Nếu thao tác A happens-before thao tác B thì ảnh hưởng của A phải được B nhìn thấy. Quan hệ này được tạo bởi những cơ chế đồng bộ như mutex hoặc atomic với memory order phù hợp.

</details>

<details>
<summary><strong>Câu 95: Thread-safe và reentrant khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** Thread-safe nghĩa là hàm hoặc đối tượng có thể được nhiều thread sử dụng đồng thời mà vẫn đúng. Reentrant nghĩa là hàm có thể bị gọi lại trước khi lần gọi trước hoàn thành mà vẫn an toàn. Reentrant thường có yêu cầu chặt chẽ hơn thread-safe.

</details>

<details>
<summary><strong>Câu 96: `future`, `promise` và `async` là gì?</strong></summary>

**Trả lời phỏng vấn:** `promise` dùng để cung cấp kết quả hoặc exception, `future` dùng để chờ và nhận kết quả đó, còn `std::async` hỗ trợ thực hiện một công việc bất đồng bộ và trả kết quả qua `future`.

</details>

<details>
<summary><strong>Câu 97: Semaphore là gì? Khác mutex thế nào?</strong></summary>

**Trả lời phỏng vấn:** Mutex cung cấp quyền sở hữu độc quyền để bảo vệ critical section. Semaphore quản lý một bộ đếm và giới hạn số thread được truy cập tài nguyên cùng lúc. Semaphore không có khái niệm chủ sở hữu giống mutex.

</details>

<details>
<summary><strong>Câu 98: Thread pool là gì?</strong></summary>

**Trả lời phỏng vấn:** Thread pool là tập hợp các worker thread được tạo sẵn để xử lý công việc trong hàng đợi. Nó giảm chi phí liên tục tạo và hủy thread, đồng thời giúp giới hạn số thread chạy đồng thời.

</details>

<details>
<summary><strong>Câu 99: Priority inversion là gì?</strong></summary>

**Trả lời phỏng vấn:** Priority inversion xảy ra khi thread ưu tiên cao phải chờ tài nguyên do thread ưu tiên thấp giữ, trong khi thread ưu tiên trung bình tiếp tục chiếm CPU. Một giải pháp phổ biến là priority inheritance.

</details>

<details>
<summary><strong>Câu 100: Khi thiết kế chương trình đa luồng cần lưu ý gì?</strong></summary>

**Trả lời phỏng vấn:** Cần hạn chế dữ liệu dùng chung, đồng bộ đúng critical section, tránh deadlock, giữ khóa trong thời gian ngắn, dùng RAII và quản lý vòng đời thread rõ ràng. Chỉ nên dùng đa luồng khi lợi ích lớn hơn chi phí đồng bộ và context switch.

</details>

<details>
<summary><strong>Câu 101: `shared_mutex` là gì?</strong></summary>

**Trả lời phỏng vấn:** `shared_mutex` phù hợp với dữ liệu được đọc nhiều và ghi ít. Nhiều reader có thể cùng giữ shared lock, nhưng writer cần quyền truy cập độc quyền nên khi ghi không thread nào khác được đọc hoặc ghi.

</details>

<details>
<summary><strong>Câu 102: `recursive_mutex` và `timed_mutex` là gì?</strong></summary>

**Trả lời phỏng vấn:** `recursive_mutex` cho phép cùng một thread khóa mutex nhiều lần và phải mở khóa đúng số lần. `timed_mutex` cho phép thử lấy khóa với giới hạn thời gian thay vì chờ vô hạn.

</details>

<details>
<summary><strong>Câu 103: `thread_local` là gì?</strong></summary>

**Trả lời phỏng vấn:** `thread_local` tạo một phiên bản biến riêng cho mỗi thread. Thay đổi ở thread này không ảnh hưởng đến phiên bản của thread khác, và mỗi phiên bản tồn tại trong suốt thời gian sống của thread tương ứng.

</details>

<details>
<summary><strong>Câu 104: Compare-and-swap — CAS là gì?</strong></summary>

**Trả lời phỏng vấn:** CAS là thao tác nguyên tử so sánh giá trị hiện tại với giá trị mong đợi và chỉ cập nhật thành giá trị mới nếu chúng bằng nhau. Đây là nền tảng của nhiều thuật toán lock-free.

</details>

<details>
<summary><strong>Câu 105: Lock-free và wait-free khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** Lock-free bảo đảm toàn hệ thống luôn có ít nhất một thread tiến triển nhưng một thread cụ thể vẫn có thể chờ lâu. Wait-free mạnh hơn vì bảo đảm mỗi thread hoàn thành thao tác sau một số bước hữu hạn.

</details>

<details>
<summary><strong>Câu 106: ABA problem là gì?</strong></summary>

**Trả lời phỏng vấn:** ABA xảy ra khi giá trị thay đổi từ A sang B rồi quay lại A, khiến CAS nhìn thấy A và cho rằng dữ liệu chưa thay đổi. Có thể hạn chế bằng version counter, tagged pointer hoặc kỹ thuật quản lý bộ nhớ phù hợp.

</details>

<details>
<summary><strong>Câu 107: False sharing là gì?</strong></summary>

**Trả lời phỏng vấn:** False sharing xảy ra khi các thread sửa những biến khác nhau nhưng các biến nằm trên cùng một cache line. CPU phải liên tục đồng bộ và vô hiệu hóa cache line, làm giảm hiệu năng dù dữ liệu không thực sự được dùng chung.

</details>

<details>
<summary><strong>Câu 108: Cache coherence có làm chương trình thread-safe không?</strong></summary>

**Trả lời phỏng vấn:** Không. Cache coherence giúp các CPU core duy trì dữ liệu cache nhất quán nhưng không tự bảo đảm atomicity, memory ordering hoặc loại bỏ data race. Chương trình vẫn cần mutex, atomic hoặc cơ chế đồng bộ thích hợp.

</details>

<details>
<summary><strong>Câu 109: `memory_order` trong C++ gồm những loại nào?</strong></summary>

**Trả lời phỏng vấn:** Các loại gồm `relaxed`, `consume`, `acquire`, `release`, `acq_rel` và `seq_cst`. Chúng quy định mức độ sắp xếp và đồng bộ của thao tác atomic. `seq_cst` mạnh và dễ hiểu nhất; `relaxed` chỉ bảo đảm tính nguyên tử; `consume` hiếm khi được sử dụng trực tiếp.

</details>

<details>
<summary><strong>Câu 110: `memory_order_relaxed` bảo đảm điều gì?</strong></summary>

**Trả lời phỏng vấn:** `memory_order_relaxed` bảo đảm thao tác trên chính biến atomic là nguyên tử nhưng không thiết lập thứ tự hoặc đồng bộ với các dữ liệu khác giữa các thread.

</details>

<details>
<summary><strong>Câu 111: Acquire và release hoạt động thế nào?</strong></summary>

**Trả lời phỏng vấn:** Release công bố các thao tác xảy ra trước nó, còn acquire tiếp nhận các thay đổi đó. Khi một acquire đọc được giá trị từ release tương ứng, các thao tác trước release sẽ được thread acquire nhìn thấy.

</details>

<details>
<summary><strong>Câu 112: `memory_order_seq_cst` là gì?</strong></summary>

**Trả lời phỏng vấn:** `memory_order_seq_cst` cung cấp thứ tự nhất quán tuần tự, làm các thao tác atomic `seq_cst` được quan sát theo một thứ tự chung. Đây là memory order mặc định và dễ sử dụng nhất nhưng có thể hạn chế tối ưu hóa.

</details>

<details>
<summary><strong>Câu 113: Memory fence là gì?</strong></summary>

**Trả lời phỏng vấn:** Memory fence là rào cản dùng để thiết lập thứ tự giữa các thao tác bộ nhớ, hạn chế compiler hoặc CPU sắp xếp một số thao tác qua rào cản đó. Đây là cơ chế nâng cao và cần kết hợp đúng với atomic để tạo đồng bộ hợp lệ.

</details>

<details>
<summary><strong>Câu 114: `std::jthread` khác `std::thread` thế nào?</strong></summary>

**Trả lời phỏng vấn:** `std::jthread` được thêm từ C++20, tự động yêu cầu dừng rồi `join()` khi bị hủy và hỗ trợ cooperative cancellation qua `stop_token`. `std::thread` yêu cầu lập trình viên tự `join()` hoặc `detach()`.

</details>

<details>
<summary><strong>Câu 115: `stop_token` có buộc thread dừng ngay không?</strong></summary>

**Trả lời phỏng vấn:** Không. `stop_token` chỉ truyền yêu cầu dừng theo cơ chế cooperative cancellation. Thread phải chủ động kiểm tra yêu cầu, dọn dẹp tài nguyên và tự kết thúc an toàn.

</details>

<details>
<summary><strong>Câu 116: `latch` và `barrier` khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** `latch` là điểm đồng bộ sử dụng một lần; khi bộ đếm giảm về 0, các thread được tiếp tục. `barrier` đồng bộ các thread theo nhiều giai đoạn và có thể tự tái sử dụng sau mỗi giai đoạn.

</details>

<details>
<summary><strong>Câu 117: `launch::async` và `launch::deferred` khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** `launch::async` yêu cầu công việc được thực hiện bất đồng bộ. `launch::deferred` trì hoãn công việc cho đến khi gọi `get()` hoặc `wait()` trên `future`. Nếu không chỉ định chính sách, implementation có thể lựa chọn.

</details>

<details>
<summary><strong>Câu 118: Điều gì xảy ra nếu exception thoát khỏi hàm của `std::thread`?</strong></summary>

**Trả lời phỏng vấn:** Nếu exception thoát khỏi hàm chạy bởi `std::thread` mà không được bắt, chương trình gọi `std::terminate()`. Vì vậy, thread nên bắt exception và truyền lỗi về thread quản lý qua `promise`, `future` hoặc `exception_ptr`.

</details>

<details>
<summary><strong>Câu 119: `std::call_once` và `once_flag` dùng để làm gì?</strong></summary>

**Trả lời phỏng vấn:** `std::call_once` kết hợp với `once_flag` bảo đảm một thao tác chỉ được thực hiện thành công đúng một lần dù nhiều thread cùng gọi. Nó thường dùng để khởi tạo tài nguyên dùng chung hoặc cài đặt thread-safe Singleton.

</details>

<details>
<summary><strong>Câu 120: Làm thế nào để debug và tối ưu chương trình đa luồng?</strong></summary>

**Trả lời phỏng vấn:** Khi debug, cần kiểm tra data race, deadlock, thứ tự lấy khóa, vòng đời thread và ghi log kèm thread ID. Khi tối ưu, cần giảm dữ liệu dùng chung, giữ khóa ngắn, giới hạn số thread, kiểm tra false sharing và luôn đo hiệu năng trước khi thay đổi. CPU affinity chỉ nên dùng khi có lý do và số liệu rõ ràng.

</details>

---

### Ghi nhớ nhanh đa luồng

- Process chứa tài nguyên; thread thực hiện công việc.
- Dữ liệu dùng chung phải được đồng bộ đúng cách.
- Mutex bảo vệ một khối thao tác; atomic phù hợp với thao tác đơn giản.
- `volatile` không thay thế `atomic`.
- Condition variable phải luôn kiểm tra điều kiện bằng predicate.
- Thống nhất thứ tự lấy khóa để hạn chế deadlock.
- Dùng RAII để khóa được tự động giải phóng.
- Quản lý rõ vòng đời và cách kết thúc thread.

</details>

<details>
<summary><h1>IPC — Giao tiếp giữa các process — Câu 121–140</h1></summary>

<details>
<summary><strong>Câu 121: IPC là gì và tại sao process cần IPC?</strong></summary>

**Trả lời phỏng vấn:** IPC là tập hợp các cơ chế cho phép những process có vùng nhớ riêng trao đổi dữ liệu và đồng bộ với nhau. IPC cần thiết vì một process thông thường không thể trực tiếp truy cập biến nằm trong không gian địa chỉ của process khác.

</details>

<details>
<summary><strong>Câu 122: Những cơ chế IPC phổ biến là gì?</strong></summary>

**Trả lời phỏng vấn:** Các cơ chế IPC phổ biến gồm pipe, named pipe, shared memory, message queue, socket, signal, semaphore, memory-mapped file và RPC. Cơ chế phù hợp phụ thuộc vào phạm vi giao tiếp, lượng dữ liệu, hiệu năng và độ phức tạp chấp nhận được.

| Cơ chế IPC | Là gì? | Dùng khi nào? | Điểm cần nhớ |
|---|---|---|---|
| **Pipe (anonymous pipe)** | Kênh truyền byte không có tên, thường nối process cha và process con | Truyền dữ liệu đơn giản giữa các process có quan hệ cha–con; chuyển hướng `stdin`/`stdout` | Thường một chiều; muốn hai chiều thường cần hai pipe |
| **Named pipe** | Pipe có tên do hệ điều hành quản lý | Giao tiếp giữa các process độc lập trên cùng máy; mô hình client–server | Có thể hỗ trợ hai chiều; Windows có byte mode và message mode |
| **Shared memory** | Nhiều process cùng ánh xạ và truy cập một vùng nhớ | Truyền dữ liệu lớn hoặc cần độ trễ thấp | Thường rất nhanh nhưng phải dùng cơ chế đồng bộ riêng |
| **Message queue** | Hàng đợi truyền dữ liệu theo từng message | Producer–consumer, xử lý bất đồng bộ, các bên không cần hoạt động cùng lúc | Bảo toàn ranh giới message nhưng thường có thêm overhead |
| **Socket** | Hai endpoint trao đổi dữ liệu trên cùng máy hoặc qua mạng | Giao tiếp client–server, giữa các máy hoặc khi cần giao thức mạng | TCP là byte stream; ứng dụng phải tự xử lý ranh giới message |
| **Signal / event** | Cơ chế thông báo rằng một sự kiện đã xảy ra | Báo dừng, đánh thức hoặc thông báo thay đổi trạng thái | Phù hợp để báo hiệu, không phù hợp truyền dữ liệu lớn |
| **Semaphore / named mutex** | Cơ chế đồng bộ có thể được nhiều process cùng sử dụng | Bảo vệ shared memory hoặc giới hạn số process truy cập tài nguyên | Chủ yếu dùng để đồng bộ, không phải để truyền nội dung dữ liệu |
| **Memory-mapped file** | Ánh xạ file hoặc vùng mapping vào không gian địa chỉ của process | Chia sẻ dữ liệu lớn, xử lý file lớn hoặc cần dữ liệu có thể lưu lại | Nhiều process cùng ghi thì vẫn phải đồng bộ |
| **RPC** | Gọi một hàm hoặc dịch vụ nằm trong process hay máy khác | Kiến trúc dịch vụ, request–response, ứng dụng phân tán | Cần serialization; có độ trễ, timeout và khả năng thất bại |

**Lưu ý:** Thư viện chuẩn C++ chưa cung cấp đầy đủ IPC tổng quát; chương trình thường dùng API hệ điều hành hoặc thư viện bên ngoài.

</details>

<details>
<summary><strong>Câu 123: Anonymous pipe là gì?</strong></summary>

**Trả lời phỏng vấn:** Anonymous pipe là kênh IPC không có tên, thường dùng giữa process cha và process con. Nó truyền dữ liệu dạng byte stream và thường hoạt động một chiều; muốn giao tiếp hai chiều thường cần hai pipe.

</details>

<details>
<summary><strong>Câu 124: Named pipe là gì?</strong></summary>

**Trả lời phỏng vấn:** Named pipe là pipe có tên, cho phép các process độc lập không có quan hệ cha–con giao tiếp với nhau. Nó thường dùng cho IPC trên cùng máy và có thể hỗ trợ mô hình client–server hoặc giao tiếp hai chiều tùy hệ điều hành và cấu hình.

</details>

<details>
<summary><strong>Câu 125: Pipe có bảo toàn ranh giới message không?</strong></summary>

**Trả lời phỏng vấn:** Không nên mặc định pipe luôn bảo toàn ranh giới message. Với pipe dạng byte stream, một lần ghi có thể phải đọc nhiều lần hoặc nhiều lần ghi có thể được đọc chung. Ứng dụng cần tự thiết kế message framing, chẳng hạn gửi độ dài trước dữ liệu. Một số named pipe trên Windows có thể được cấu hình theo message mode.

</details>

<details>
<summary><strong>Câu 126: Shared memory là gì?</strong></summary>

**Trả lời phỏng vấn:** Shared memory ánh xạ cùng một vùng nhớ vào nhiều process để các process đọc và ghi trực tiếp. Nó có hiệu năng cao và hạn chế sao chép dữ liệu, nhưng chương trình phải tự đồng bộ để tránh race condition và hỏng dữ liệu.

</details>

<details>
<summary><strong>Câu 127: Đồng bộ shared memory như thế nào?</strong></summary>

**Trả lời phỏng vấn:** Shared memory chỉ cung cấp vùng dữ liệu chung, không tự đồng bộ. Cần dùng semaphore, mutex liên process, process-shared mutex hoặc cơ chế atomic phù hợp để bảo vệ dữ liệu. Không nên chỉ dùng một biến `bool` thông thường làm cờ đồng bộ.

</details>

<details>
<summary><strong>Câu 128: Message queue là gì?</strong></summary>

**Trả lời phỏng vấn:** Message queue truyền dữ liệu theo từng message thông qua một hàng đợi do hệ thống hoặc middleware quản lý. Nó phù hợp với giao tiếp bất đồng bộ, bảo toàn ranh giới message và giúp tách biệt bên gửi với bên nhận.

</details>

<details>
<summary><strong>Câu 129: Shared memory và message queue khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** Shared memory thường nhanh hơn và phù hợp với dữ liệu lớn nhưng phức tạp về đồng bộ và quản lý cấu trúc dữ liệu. Message queue dễ tổ chức hơn, có ranh giới message rõ ràng nhưng thường có thêm overhead sao chép và quản lý.

</details>

<details>
<summary><strong>Câu 130: Socket là gì?</strong></summary>

**Trả lời phỏng vấn:** Socket là một endpoint giao tiếp hai chiều, cho phép các process trao đổi dữ liệu trên cùng máy hoặc qua mạng. Socket thường được sử dụng theo mô hình client–server.

</details>

<details>
<summary><strong>Câu 131: Local socket và network socket khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** Local socket chỉ dùng giữa các process trên cùng máy, còn network socket dùng địa chỉ mạng và port để giao tiếp trên cùng máy hoặc giữa các máy. Khi không cần qua mạng, local socket thường có overhead thấp hơn và phạm vi truy cập dễ kiểm soát hơn.

</details>

<details>
<summary><strong>Câu 132: TCP có bảo toàn ranh giới message không?</strong></summary>

**Trả lời phỏng vấn:** Không. TCP cung cấp một byte stream liên tục nên một lần `send()` không nhất thiết tương ứng với một lần `recv()`. Chương trình phải xử lý partial send, partial receive và tự thiết kế message framing bằng độ dài, ký hiệu kết thúc hoặc message có kích thước cố định.

</details>

<details>
<summary><strong>Câu 133: Signal là gì?</strong></summary>

**Trả lời phỏng vấn:** Signal là cơ chế thông báo sự kiện bất đồng bộ cho một process, chẳng hạn yêu cầu dừng hoặc báo một sự kiện hệ thống. Nó phù hợp cho tín hiệu điều khiển đơn giản nhưng không phải kênh truyền lượng dữ liệu lớn.

</details>

<details>
<summary><strong>Câu 134: Semaphore, mutex và event liên process khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** Mutex dùng cho quyền sở hữu độc quyền một tài nguyên. Semaphore dùng bộ đếm để giới hạn số process hoặc thread được truy cập đồng thời. Event dùng để báo hiệu rằng một sự kiện hoặc trạng thái đã xảy ra.

</details>

<details>
<summary><strong>Câu 135: Memory-mapped file là gì?</strong></summary>

**Trả lời phỏng vấn:** Memory-mapped file ánh xạ nội dung file vào không gian địa chỉ để chương trình truy cập giống như vùng nhớ. Nó có thể dùng để xử lý file lớn hoặc chia sẻ dữ liệu giữa các process, nhưng vẫn cần đồng bộ khi có nhiều bên cùng ghi.

</details>

<details>
<summary><strong>Câu 136: RPC là gì?</strong></summary>

**Trả lời phỏng vấn:** RPC cho phép một process gọi thủ tục hoặc dịch vụ nằm trong process hoặc máy khác như một lời gọi hàm. Bên dưới vẫn phải đóng gói dữ liệu, truyền request, thực hiện xử lý và trả response. RPC có độ trễ và khả năng thất bại nên không thể xem hoàn toàn giống lời gọi cục bộ.

</details>

<details>
<summary><strong>Câu 137: Serialization và deserialization là gì?</strong></summary>

**Trả lời phỏng vấn:** Serialization chuyển object hoặc cấu trúc dữ liệu thành định dạng có thể truyền hoặc lưu trữ; deserialization khôi phục dữ liệu ở phía nhận. Hai phía phải thống nhất định dạng, kích thước kiểu, byte order và phiên bản giao thức.

**Lưu ý:** Không nên gửi trực tiếp toàn bộ byte của một C/C++ `struct` nếu chưa xử lý padding, con trỏ và byte order.

</details>

<details>
<summary><strong>Câu 138: Blocking, non-blocking, synchronous và asynchronous khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** Blocking và non-blocking mô tả lời gọi có làm thread chờ hay trả về ngay. Synchronous và asynchronous mô tả cách tổ chức công việc và nhận kết quả. Hai cặp khái niệm có liên quan nhưng không hoàn toàn giống nhau.

</details>

<details>
<summary><strong>Câu 139: IPC cần lưu ý gì về bảo mật và độ tin cậy?</strong></summary>

**Trả lời phỏng vấn:** Không được mặc định tin tưởng dữ liệu IPC. Cần kiểm tra quyền truy cập, danh tính bên kết nối, định dạng và kích thước dữ liệu; đồng thời xử lý timeout, mất kết nối, partial read/write, process bị treo, phiên bản giao thức và việc dọn dẹp tài nguyên.

</details>

<details>
<summary><strong>Câu 140: Chọn cơ chế IPC như thế nào?</strong></summary>

| Nhu cầu | Cơ chế phù hợp |
|---|---|
| Process cha–con, dữ liệu đơn giản | Anonymous pipe |
| Process độc lập trên cùng máy | Named pipe hoặc local socket |
| Dữ liệu lớn, yêu cầu tốc độ cao | Shared memory |
| Truyền từng message bất đồng bộ | Message queue |
| Giao tiếp qua mạng | Socket |
| Chia sẻ hoặc xử lý file lớn | Memory-mapped file |
| Gọi dịch vụ ở process hoặc máy khác | RPC |
| Thông báo sự kiện đơn giản | Signal hoặc event |

**Trả lời phỏng vấn:** Tôi chọn IPC dựa trên phạm vi giao tiếp, lượng dữ liệu, yêu cầu hiệu năng, mô hình message, khả năng đồng bộ, bảo mật và độ phức tạp. Shared memory nhanh nhưng khó đồng bộ; pipe và message queue dễ tổ chức hơn; socket phù hợp khi cần giao tiếp qua mạng.

**Trên Windows C++:** Các cơ chế thường gặp gồm Named Pipe, socket, named mutex/semaphore/event, `CreateFileMapping()` kết hợp `MapViewOfFile()`, và `WM_COPYDATA` cho dữ liệu nhỏ giữa các ứng dụng có cửa sổ.

</details>

---

### Ghi nhớ nhanh IPC

- Pipe phù hợp với luồng dữ liệu đơn giản.
- Shared memory nhanh nhưng phải tự đồng bộ.
- Message queue truyền theo từng message.
- Socket dùng được trên cùng máy hoặc qua mạng.
- TCP là byte stream, không bảo toàn ranh giới message.
- Serialization phải xử lý padding, con trỏ, byte order và phiên bản.
- Luôn xử lý timeout, partial read/write, mất kết nối và dọn dẹp tài nguyên.

</details>


<details>
<summary><h1>C/C++ BỔ SUNG — Câu 141–165</h1></summary>

> Phần này bổ sung các kiến thức C/C++ cốt lõi còn thiếu. Các chủ đề Embedded chuyên sâu sẽ được thêm sau.

<details>
<summary><strong>Câu 141: Exception là gì? `try`, `throw` và `catch` dùng để làm gì?</strong></summary>

Exception là cơ chế báo và xử lý lỗi trong C++.

- `try`: chứa đoạn code có thể phát sinh lỗi.
- `throw`: phát ra exception.
- `catch`: nhận và xử lý exception.

```cpp
try {
    throw 10;
} catch (int error) {
    std::cout << error;
}
```

**Trả lời phỏng vấn:** Exception là cơ chế xử lý lỗi trong C++. Code có khả năng lỗi được đặt trong `try`, lỗi được phát ra bằng `throw` và được xử lý trong `catch`.

</details>

<details>
<summary><strong>Câu 142: Stack unwinding là gì? RAII liên quan thế nào đến exception?</strong></summary>

Khi exception xảy ra, chương trình rời khỏi các hàm đang gọi để tìm `catch` phù hợp. Trong quá trình đó, destructor của các object local đã được tạo sẽ tự động chạy. Quá trình này gọi là **stack unwinding**.

RAII giúp tài nguyên được giải phóng qua destructor, kể cả khi exception xảy ra.

**Trả lời phỏng vấn:** Stack unwinding là quá trình C++ thoát dần khỏi các function call để tìm `catch` và gọi destructor của object local. Vì vậy, RAII giúp dọn dẹp tài nguyên an toàn khi có exception.

</details>

<details>
<summary><strong>Câu 143: Reference và pointer khác nhau như thế nào?</strong></summary>

| Tiêu chí | Reference | Pointer |
|---|---|---|
| Khởi tạo | Phải tham chiếu đến object khi khai báo | Có thể chưa trỏ đến object |
| Giá trị null | Thông thường không được null | Có thể là `nullptr` |
| Đổi đối tượng đích | Không thể đổi sau khi khởi tạo | Có thể trỏ sang object khác |
| Truy cập | Dùng trực tiếp như biến thường | Phải dereference bằng `*` hoặc `->` |
| Phép toán địa chỉ | Không hỗ trợ pointer arithmetic | Có hỗ trợ |

**Trả lời phỏng vấn:** Reference là bí danh của một object và phải được khởi tạo ngay. Pointer lưu địa chỉ, có thể là `nullptr` và có thể đổi sang địa chỉ khác.

</details>

<details>
<summary><strong>Câu 144: Vì sao thường truyền object bằng `const T&`?</strong></summary>

```cpp
void Print(const std::string& text);
```

- `&` tránh sao chép object.
- `const` không cho hàm thay đổi object.
- Phù hợp với object lớn như `std::string`, `vector` hoặc class.

**Trả lời phỏng vấn:** Truyền bằng `const T&` giúp tránh chi phí copy và bảo đảm hàm không sửa object. Với kiểu nhỏ như `int` hoặc `char`, truyền tham trị thường đơn giản hơn.

</details>

<details>
<summary><strong>Câu 145: `public`, `private` và `protected` khác nhau thế nào?</strong></summary>

| Access specifier | Bên trong class | Class con | Code bên ngoài |
|---|:---:|:---:|:---:|
| `public` | Có | Có | Có |
| `protected` | Có | Có | Không |
| `private` | Có | Không truy cập trực tiếp | Không |

**Trả lời phỏng vấn:** `public` cho phép truy cập từ bên ngoài; `protected` chỉ cho class hiện tại và class con; `private` chỉ cho chính class và friend truy cập.

</details>

<details>
<summary><strong>Câu 146: Con trỏ `this` là gì?</strong></summary>

`this` là con trỏ ngầm định trỏ đến object đang gọi hàm thành viên.

```cpp
class Student {
private:
    int age;
public:
    void SetAge(int age) {
        this->age = age;
    }
};
```

**Trả lời phỏng vấn:** `this` trỏ đến object hiện tại. Nó thường dùng để phân biệt member với parameter cùng tên hoặc trả về chính object bằng `*this`.

</details>

<details>
<summary><strong>Câu 147: Hàm thành viên `const` là gì?</strong></summary>

```cpp
int GetValue() const {
    return value;
}
```

Từ khóa `const` sau tên hàm bảo đảm hàm không thay đổi trạng thái thông thường của object và cho phép gọi hàm trên object `const`.

**Trả lời phỏng vấn:** Hàm thành viên `const` cam kết không thay đổi object, ngoại trừ member `mutable`. Object `const` chỉ gọi được các hàm thành viên `const`.

</details>

<details>
<summary><strong>Câu 148: Các loại constructor thường gặp là gì?</strong></summary>

| Loại | Mục đích |
|---|---|
| Default constructor | Tạo object không cần đối số |
| Parameterized constructor | Tạo object bằng các giá trị truyền vào |
| Copy constructor | Tạo object mới từ một lvalue object khác |
| Move constructor | Tạo object mới bằng cách nhận tài nguyên từ rvalue object khác |

**Trả lời phỏng vấn:** Constructor dùng để khởi tạo object. Các loại thường gặp là default, parameterized, copy và move constructor.

</details>

<details>
<summary><strong>Câu 149: Copy constructor và copy assignment khác nhau thế nào?</strong></summary>

```cpp
Test b = a;  // Copy constructor: b đang được tạo
b = a;       // Copy assignment: b đã tồn tại
```

**Trả lời phỏng vấn:** Copy constructor tạo một object mới từ object khác. Copy assignment gán dữ liệu cho một object đã tồn tại và phải xử lý tài nguyên cũ của object đích.

</details>

<details>
<summary><strong>Câu 150: Move constructor và move assignment khác nhau thế nào?</strong></summary>

- Move constructor khởi tạo object mới bằng tài nguyên của object nguồn.
- Move assignment chuyển tài nguyên vào object đã tồn tại và phải giải phóng tài nguyên cũ của object đích.
- Object nguồn vẫn hợp lệ nhưng trạng thái sau move thường không được xác định cụ thể.

**Trả lời phỏng vấn:** Move constructor dùng khi object đích đang được tạo; move assignment dùng khi object đích đã tồn tại. Cả hai thường chuyển quyền sở hữu tài nguyên thay vì sao chép toàn bộ dữ liệu.

</details>

<details>
<summary><strong>Câu 151: `= default` và `= delete` có ý nghĩa gì?</strong></summary>

```cpp
Test() = default;
Test(const Test&) = delete;
Test& operator=(const Test&) = delete;
```

- `= default`: yêu cầu compiler tạo implementation mặc định.
- `= delete`: cấm sử dụng function đó.

**Trả lời phỏng vấn:** `= default` yêu cầu compiler sinh hàm mặc định; `= delete` làm hàm không thể được gọi, thường dùng để cấm copy object.

</details>

<details>
<summary><strong>Câu 152: Bốn phép cast trong C++ khác nhau thế nào?</strong></summary>

| Cast | Công dụng chính |
|---|---|
| `static_cast` | Chuyển đổi kiểu có quan hệ rõ ràng và được kiểm tra ở compile time |
| `dynamic_cast` | Downcast an toàn trong hệ phân cấp đa hình; kiểm tra ở runtime |
| `const_cast` | Thêm hoặc bỏ `const`/`volatile` |
| `reinterpret_cast` | Diễn giải lại bit/địa chỉ thành kiểu khác; rủi ro cao |

**Trả lời phỏng vấn:** Nên dùng cast C++ theo đúng mục đích. `static_cast` cho chuyển đổi thông thường, `dynamic_cast` cho đa hình runtime, `const_cast` thay đổi constness và `reinterpret_cast` cho chuyển đổi mức thấp.

</details>

<details>
<summary><strong>Câu 153: Lambda expression là gì?</strong></summary>

Lambda là hàm không cần đặt tên, thường dùng làm callback hoặc truyền vào thuật toán STL.

```cpp
int factor = 2;
auto multiply = [factor](int value) {
    return value * factor;
};
```

- `[=]`: capture biến bên ngoài theo value.
- `[&]`: capture theo reference.
- `[x]` hoặc `[&x]`: capture một biến cụ thể.

**Trả lời phỏng vấn:** Lambda là function object được viết ngắn gọn ngay tại nơi sử dụng. Capture list quy định cách lambda sử dụng biến bên ngoài.

</details>

<details>
<summary><strong>Câu 154: Operator overloading là gì?</strong></summary>

Operator overloading cho phép định nghĩa cách toán tử hoạt động với kiểu dữ liệu do người dùng tạo.

```cpp
Point operator+(const Point& other) const;
```

Không thể tạo toán tử mới hoặc thay đổi độ ưu tiên và số lượng toán hạng của toán tử.

**Trả lời phỏng vấn:** Operator overloading cho phép toán tử như `+`, `==` hoặc `<<` làm việc tự nhiên với class. Chỉ nên overload khi ý nghĩa của toán tử rõ ràng và dễ hiểu.

</details>

<details>
<summary><strong>Câu 155: C string và `std::string` khác nhau thế nào?</strong></summary>

| C string | `std::string` |
|---|---|
| Mảng `char` kết thúc bằng `\0` | Class quản lý chuỗi tự động |
| Phải quản lý kích thước cẩn thận | Tự quản lý bộ nhớ |
| Dùng `strlen`, `strcmp`, `strcpy`... | Có `size`, `find`, `append`, toán tử `+`... |
| Dễ xảy ra buffer overflow | An toàn và thuận tiện hơn trong C++ |

**Trả lời phỏng vấn:** C string là mảng `char` kết thúc bằng null character, còn `std::string` là class quản lý chuỗi và bộ nhớ tự động. Trong C++ nên ưu tiên `std::string` khi không bị giới hạn bởi API C.

</details>

<details>
<summary><strong>Câu 156: Những lỗi bộ nhớ phổ biến trong C/C++ là gì?</strong></summary>

| Lỗi | Ý nghĩa |
|---|---|
| Memory leak | Cấp phát nhưng không giải phóng |
| Dangling pointer | Con trỏ trỏ tới vùng nhớ không còn hợp lệ |
| Use-after-free | Truy cập vùng nhớ sau khi đã giải phóng |
| Double free | Giải phóng cùng vùng nhớ nhiều lần |
| Buffer overflow | Ghi vượt giới hạn buffer |
| Out-of-bounds | Truy cập ngoài phạm vi mảng/container |
| Null dereference | Dereference con trỏ null |

**Trả lời phỏng vấn:** Các lỗi phổ biến gồm leak, use-after-free, double free, buffer overflow, out-of-bounds và null dereference. Nên dùng RAII, smart pointer, kiểm tra giới hạn và công cụ phân tích bộ nhớ để phòng tránh.

</details>

<details>
<summary><strong>Câu 157: Include guard và `#pragma once` dùng để làm gì?</strong></summary>

Chúng ngăn một header bị include nhiều lần trong cùng translation unit.

```cpp
#ifndef MY_CLASS_H
#define MY_CLASS_H

class MyClass {};

#endif
```

Hoặc:

```cpp
#pragma once
```

**Trả lời phỏng vấn:** Include guard và `#pragma once` ngăn lỗi định nghĩa lặp khi một header được include nhiều lần. Include guard là cách chuẩn và portable; `#pragma once` ngắn gọn và được nhiều compiler hỗ trợ.

</details>

<details>
<summary><strong>Câu 158: Composition và inheritance khác nhau thế nào?</strong></summary>

- Inheritance biểu diễn quan hệ **is-a**: `Dog` là một `Animal`.
- Composition biểu diễn quan hệ **has-a**: `Car` có một `Engine`.

**Trả lời phỏng vấn:** Inheritance phù hợp khi class con thực sự là một dạng của class cha và cần đa hình. Composition ghép object từ các thành phần nhỏ, giảm phụ thuộc và thường linh hoạt hơn.

</details>

<details>
<summary><strong>Câu 159: Interface trong C++ được biểu diễn như thế nào?</strong></summary>

C++ không có từ khóa `interface`. Interface thường được biểu diễn bằng abstract class chứa các pure virtual function và virtual destructor.

```cpp
class ILogger {
public:
    virtual ~ILogger() = default;
    virtual void Log(const std::string& message) = 0;
};
```

**Trả lời phỏng vấn:** Interface trong C++ thường là abstract class chỉ khai báo hành vi qua pure virtual function. Nó giúp code phụ thuộc vào abstraction thay vì implementation cụ thể.

</details>

<details>
<summary><strong>Câu 160: Diamond problem và virtual inheritance là gì?</strong></summary>

Diamond problem xảy ra khi một class kế thừa từ hai class và cả hai class đó cùng kế thừa từ một base class, làm object có thể chứa hai bản sao của base.

Virtual inheritance giúp các nhánh dùng chung một base subobject:

```cpp
class B : virtual public A {};
class C : virtual public A {};
```

**Trả lời phỏng vấn:** Diamond problem gây trùng lặp và nhập nhằng base class trong multiple inheritance. Virtual inheritance bảo đảm class cuối chỉ có một base subobject dùng chung.

</details>

<details>
<summary><strong>Câu 161: `friend` trong C++ là gì?</strong></summary>

Function hoặc class được khai báo `friend` có thể truy cập member `private` và `protected`.

**Trả lời phỏng vấn:** `friend` cấp quyền truy cập đặc biệt cho một function hoặc class khác. Nó hữu ích trong một số trường hợp như overload toán tử, nhưng nên hạn chế vì làm tăng phụ thuộc và giảm tính đóng gói.

</details>

<details>
<summary><strong>Câu 162: Factory pattern là gì?</strong></summary>

Factory tách logic tạo object khỏi nơi sử dụng object.

```text
CreateDevice("wifi") → WifiDevice
CreateDevice("mqtt") → MqttDevice
```

**Trả lời phỏng vấn:** Factory là design pattern đóng gói việc tạo object và thường trả object thông qua interface/base class. Nó giúp code sử dụng không phụ thuộc trực tiếp vào class cụ thể.

</details>

<details>
<summary><strong>Câu 163: Observer pattern là gì?</strong></summary>

Observer cho phép một object thông báo cho nhiều object khác khi trạng thái thay đổi.

```text
NetworkManager mất kết nối
        ↓ thông báo
Logger, UI, ReconnectManager
```

**Trả lời phỏng vấn:** Observer thiết lập quan hệ một-nhiều. Khi subject thay đổi, các observer đã đăng ký sẽ được thông báo; thường dùng cho event, callback và cập nhật trạng thái.

</details>

<details>
<summary><strong>Câu 164: `auto` và range-based `for` dùng để làm gì?</strong></summary>

```cpp
std::vector<int> numbers = {1, 2, 3};

for (const auto& number : numbers) {
    std::cout << number;
}
```

- `auto`: compiler suy luận kiểu từ biểu thức khởi tạo.
- Range-based `for`: duyệt các phần tử trong container ngắn gọn hơn.

**Trả lời phỏng vấn:** `auto` giúp tránh lặp lại tên kiểu dài nhưng không làm C++ trở thành ngôn ngữ dynamic type. Range-based `for` giúp duyệt container rõ ràng; dùng `const auto&` để tránh copy khi chỉ đọc.

</details>

<details>
<summary><strong>Câu 165: `size`, `capacity`, `reserve`, `resize`, `push_back` và `emplace_back` của `vector` khác nhau thế nào?</strong></summary>

| Thành phần | Ý nghĩa |
|---|---|
| `size()` | Số phần tử đang có |
| `capacity()` | Số phần tử có thể chứa trước khi phải cấp phát lại |
| `reserve(n)` | Tăng capacity, không tạo thêm phần tử |
| `resize(n)` | Thay đổi số phần tử thực tế |
| `push_back(x)` | Thêm một object đã có vào cuối |
| `emplace_back(...)` | Tạo object trực tiếp ở cuối từ các đối số |

**Trả lời phỏng vấn:** `size` là số phần tử, còn `capacity` là dung lượng đã cấp phát. `reserve` chuẩn bị bộ nhớ nhưng không đổi size; `resize` làm thay đổi số phần tử. `emplace_back` có thể tạo phần tử trực tiếp trong container.

</details>

### Ghi nhớ nhanh C/C++ bổ sung

- Exception: `try → throw → catch`.
- Reference là bí danh; pointer lưu địa chỉ.
- `const T&` tránh copy và không cho sửa object.
- Copy/move constructor tạo object mới; assignment làm việc với object đã tồn tại.
- Ưu tiên RAII và smart pointer để quản lý tài nguyên.
- Composition là **has-a**; inheritance là **is-a**.
- Factory dùng để tạo object; Observer dùng để thông báo sự kiện.

</details>


<details>
<summary><h1>FPT Telecom</h1></summary>

## 1. Công việc chính

Phát triển và maintain embedded software cho **FPT Play Box** và **Smart Home**, tập trung vào:

| Công việc | Ví dụ đơn giản |
|---|---|
| **Xử lý kết nối** | Kiểm tra thiết bị có kết nối Wi-Fi và kết nối với máy chủ của FPT hay không; nếu mất kết nối thì kết nối lại |
| **Điều khiển thiết bị** | Nhận lệnh bật/tắt từ máy chủ của FPT rồi thực hiện bật/tắt thiết bị |
| **Theo dõi trạng thái thiết bị** | Kiểm tra thiết bị đang bật, tắt, online hay offline rồi gửi trạng thái đó lên máy chủ của FPT |
| **Kết nối thiết bị với hệ thống máy chủ** | Nhận dữ liệu hoặc lệnh từ máy chủ, đọc nội dung lệnh, xử lý rồi gửi kết quả hoặc trạng thái của thiết bị ngược lại |

Ngoài ra còn:

- **Debugging**: tìm và sửa lỗi trong chương trình, thường dựa vào log hoặc dùng GDB/gdbserver để kiểm tra process, breakpoint, biến và call stack.
- **Functional Test**: kiểm tra **từng chức năng riêng lẻ có hoạt động đúng yêu cầu hay không**.  
  Ví dụ: gửi lệnh `ON` thì thiết bị có thực sự bật hay không; mất Wi-Fi thì thiết bị có tự kết nối lại hay không.
- **Integration Test**: kiểm tra **nhiều thành phần khi kết nối với nhau có hoạt động đúng hay không**.  
  Ví dụ: máy chủ gửi lệnh qua MQTT → thiết bị nhận lệnh → bật thiết bị → gửi trạng thái `ON` ngược lại cho máy chủ.
- **Technical Document**: viết tài liệu kỹ thuật như cách build, cách cài đặt, cách test, mô tả luồng xử lý hoặc hướng dẫn debug.

---

## 2. Ngôn ngữ

- C
- C++

---

## 3. Công nghệ

- OpenWrt
- Wi-Fi
- MQTT
- Git
- SVN

---

# KIẾN THỨC LIÊN QUAN

## 4. Linux Kernel là gì?

**Linux Kernel** là phần lõi của hệ điều hành Linux.

Nó quản lý:

- CPU
- RAM
- Process
- File system
- Driver
- Thiết bị phần cứng

Dễ nhớ:

> Kernel = phần trung tâm quản lý toàn bộ hệ thống.

---

## 5. User Space và Kernel Space

### User Space

Là nơi các application chạy.

Ví dụ:

```text
my_app
MQTT client
Web service
```

### Kernel Space

Là nơi Linux Kernel và driver chạy.

Dễ nhớ:

```text
User Space   → Application
Kernel Space → Kernel + Driver
```

---

## 6. Process và Thread

### Process

Là một chương trình đang chạy.

Ví dụ:

```text
my_app
```

khi được chạy sẽ trở thành một process.

### Thread

Là một luồng thực thi bên trong process.

Một process có thể có nhiều thread.

Dễ nhớ:

```text
Process
├── Thread 1
├── Thread 2
└── Thread 3
```

---

## 7. OpenWrt là gì?

**OpenWrt** là hệ điều hành Linux dành cho thiết bị mạng và embedded.

Ví dụ:

- Router
- Gateway
- Smart Home device
- Access Point

Dễ nhớ:

> OpenWrt = Linux dành cho thiết bị mạng/embedded.

---

## 8. Embedded Linux khác Bare-metal MCU thế nào?

### Embedded Linux

Có:

- Linux Kernel
- Process
- Thread
- File system
- Driver
- Service

### Bare-metal MCU

Code chạy trực tiếp trên vi điều khiển.

Thường không có hệ điều hành đầy đủ.

Dễ nhớ:

```text
Embedded Linux
→ Có Linux

Bare-metal MCU
→ Code chạy trực tiếp trên MCU
```

---

## 9. Application / Service chạy trên OpenWrt là gì?

### Application

Là chương trình C/C++ chạy trên thiết bị.

Ví dụ:

```text
my_app
```

có thể dùng để:

- xử lý Wi-Fi
- xử lý MQTT
- nhận lệnh
- điều khiển thiết bị
- gửi trạng thái

### Service

Là chương trình thường chạy nền và có thể tự khởi động khi thiết bị bật.

---

## 10. File System Linux cơ bản

Một số thư mục thường gặp:

| Thư mục | Ý nghĩa |
|---|---|
| `/etc` | File cấu hình |
| `/tmp` | File tạm |
| `/usr` | Program và library |
| `/dev` | Thiết bị |
| `/proc` | Thông tin process và kernel |

---

## 11. Process / Service trên Device

### Process

Là chương trình đang chạy.

Có thể kiểm tra bằng:

```bash
ps
```

hoặc:

```bash
top
```

### Service

Là chương trình chạy nền.

Ví dụ service có thể tự chạy khi thiết bị OpenWrt khởi động.

---

## 12. SSH vào Device

SSH dùng để truy cập terminal của thiết bị từ laptop qua mạng.

Ví dụ:

```bash
ssh root@192.168.1.1
```

Sau khi SSH vào device có thể chạy:

```bash
ps
top
logread
ls
```

Dễ nhớ:

> SSH = vào terminal của thiết bị từ xa.

---

# BUILD / TOOLCHAIN

## 13. Compiler là gì?

Compiler dùng để dịch code C/C++ thành object code.

Ví dụ:

```text
main.cpp
   ↓
Compiler
   ↓
main.o
```

---

## 14. Linker là gì?

Linker dùng để ghép:

- các file `.o`
- các library

thành một chương trình hoàn chỉnh.

Ví dụ:

```text
main.o
network.o
library
   ↓
Linker
   ↓
my_app
```

---

## 15. Toolchain là gì?

Toolchain là bộ công cụ dùng để build chương trình.

Thường gồm:

- Compiler
- Linker
- Assembler
- Debugger

Dễ nhớ:

> Toolchain = bộ công cụ build software.

---

## 16. GCC là gì?

GCC là bộ compiler/toolchain phổ biến dùng để compile C/C++.

Dễ nhớ:

> GCC = công cụ compile C/C++.

---

## 17. Cross-compile là gì?

Cross-compile là:

> Build chương trình trên một máy nhưng chương trình tạo ra chạy trên một máy khác.

Ví dụ:

```text
Laptop x86
   ↓ build

Router ARM
   ↓
chạy chương trình
```

---

## 18. Vì sao phải Cross-compile?

Vì thiết bị embedded:

- tài nguyên hạn chế
- CPU khác laptop
- thường không có môi trường build đầy đủ

Nên ta build trên laptop rồi deploy lên thiết bị.

---

## 19. GCC Toolchain dùng để làm gì?

Dùng để compile và link code C/C++.

```text
Source C/C++
    ↓
GCC Toolchain
    ↓
Executable
```

---

## 20. OpenWrt SDK là gì?

OpenWrt SDK là bộ công cụ dùng để build software/package cho thiết bị chạy OpenWrt.

Flow:

```text
Source C/C++
    ↓
Makefile
    ↓
OpenWrt SDK
    ↓
GCC Cross-Compiler
    ↓
Executable / .ipk
```

---

## 21. Makefile dùng để làm gì?

Makefile mô tả cách build chương trình.

Ví dụ nó cho biết:

- compile file nào
- link library nào
- output tên gì
- cách install package

Dễ nhớ:

> Makefile = hướng dẫn cách build.

---

## 22. `.o`, Executable và `.ipk`

### `.o`

File trung gian sau khi compile.

### Executable

File chương trình có thể chạy.

### `.ipk`

Package dùng để cài phần mềm trên OpenWrt.

Flow:

```text
main.cpp
   ↓
Compiler
   ↓
main.o
   ↓
Linker
   ↓
my_app
   ↓
đóng package
   ↓
my_app.ipk
```

---

# NETWORK CƠ BẢN

## 23. IP Address

Là địa chỉ của một thiết bị trong mạng.

Dễ nhớ:

> IP = địa chỉ của thiết bị.

---

## 24. TCP/IP cơ bản

Là bộ giao thức giúp các thiết bị giao tiếp với nhau qua mạng.

- **IP**: xác định địa chỉ thiết bị.
- **TCP/UDP**: dùng để truyền dữ liệu.

---

## 25. TCP và UDP khác nhau

### TCP

- Có kết nối.
- Đảm bảo dữ liệu đến.
- Đảm bảo đúng thứ tự.
- Tin cậy hơn.

### UDP

- Không cần tạo kết nối trước.
- Nhanh hơn.
- Không đảm bảo dữ liệu đến đầy đủ.

---

## 26. Client và Server

### Client

Là bên gửi yêu cầu hoặc chủ động tạo kết nối.

### Server

Là bên nhận và xử lý yêu cầu.

---

## 27. Socket là gì?

Socket là điểm giao tiếp giữa hai chương trình qua mạng.

Ví dụ:

```text
Client
   ↓
Socket
   ↓
Network
   ↓
Socket
   ↓
Server
```

---

## 28. Port là gì?

Port dùng để xác định application/service cụ thể trên một thiết bị.

Dễ nhớ:

```text
IP   = địa chỉ ngôi nhà
Port = số phòng
```

---

## 29. DNS là gì?

DNS dùng để chuyển tên miền thành IP address.

Ví dụ:

```text
example.com
   ↓
DNS
   ↓
IP Address
```

---

## 30. Wi-Fi Connection cơ bản

Luồng đơn giản:

```text
Device
   ↓
Tìm Wi-Fi
   ↓
Kết nối Access Point
   ↓
Nhận IP
   ↓
Có thể giao tiếp mạng
```

---

## 31. Reconnect khi mất Network

Khi phát hiện mất Wi-Fi hoặc mất kết nối với server, chương trình sẽ thử kết nối lại.

Ví dụ:

```text
Đang kết nối
   ↓
Mất mạng
   ↓
Phát hiện mất kết nối
   ↓
Chờ một khoảng thời gian
   ↓
Kết nối lại
```

---

## 32. Timeout

Timeout là thời gian tối đa mà chương trình chờ một thao tác hoặc phản hồi.

Ví dụ:

> Chờ server 5 giây. Nếu sau 5 giây không có phản hồi thì xem là timeout.

---

## 33. Retry

Retry nghĩa là khi thao tác thất bại thì thử thực hiện lại.

Ví dụ:

```text
Gửi dữ liệu
   ↓
Thất bại
   ↓
Retry
   ↓
Gửi lại
```

---

# MQTT

## 34. MQTT là gì?

MQTT là giao thức truyền tin nhẹ, thường dùng trong:

- IoT
- Smart Home
- Embedded device

Dùng để thiết bị giao tiếp với hệ thống máy chủ.

---

## 35. Broker

Broker là máy chủ trung gian của MQTT.

Nó:

- nhận message
- xác định topic
- chuyển message cho các bên đã đăng ký topic đó

---

## 36. Publisher

Publisher là bên gửi message.

Ví dụ:

```text
Device
↓
Publish trạng thái
```

---

## 37. Subscriber

Subscriber là bên đăng ký để nhận message.

Ví dụ:

```text
Device
↓
Subscribe topic điều khiển
↓
Nhận lệnh ON/OFF
```

---

## 38. Topic

Topic là tên/kênh dùng để phân loại message.

Ví dụ:

```text
home/light/control
home/light/status
```

---

## 39. Publish / Subscribe

### Publish

Gửi message lên một topic.

### Subscribe

Đăng ký nhận message từ một topic.

Ví dụ:

```text
Máy chủ
   ↓ Publish "ON"
home/light/control
   ↓
MQTT Broker
   ↓
Device đã Subscribe
   ↓
Nhận "ON"
```

---

## 40. QoS 0 / 1 / 2

### QoS 0

- Gửi một lần.
- Không đảm bảo bên nhận nhận được.

### QoS 1

- Đảm bảo nhận ít nhất một lần.
- Có thể nhận trùng.

### QoS 2

- Đảm bảo nhận đúng một lần.
- Phức tạp hơn.

Dễ nhớ:

```text
QoS 0 → nhanh, không đảm bảo
QoS 1 → ít nhất 1 lần
QoS 2 → đúng 1 lần
```

---

## 41. Retained Message

Broker giữ lại message cuối cùng của một topic.

Khi subscriber mới subscribe topic đó, broker có thể gửi ngay message cuối cùng cho subscriber.

---

## 42. Keep Alive

Keep Alive dùng để kiểm tra client và MQTT Broker còn kết nối với nhau hay không.

Nếu quá lâu không có giao tiếp, kết nối có thể được xem là đã mất.

---

## 43. MQTT Reconnect

Khi kết nối MQTT bị mất:

```text
Phát hiện mất MQTT
   ↓
Chờ
   ↓
Kết nối lại Broker
   ↓
Subscribe lại các topic
   ↓
Tiếp tục hoạt động
```

---

# DEVICE GIAO TIẾP VỚI HỆ THỐNG MÁY CHỦ

## 44. Device giao tiếp với máy chủ như thế nào?

Ví dụ với Smart Home:

```text
Mobile App
    ↓
Máy chủ của FPT
    ↓
MQTT Broker
    ↓
Device chạy OpenWrt
    ↓
Application C/C++
```

Ví dụ điều khiển:

```text
Người dùng bấm ON trên App
        ↓
Máy chủ gửi lệnh ON
        ↓
MQTT Broker
        ↓
Device nhận lệnh
        ↓
Application C/C++ xử lý
        ↓
Bật thiết bị
        ↓
Device gửi trạng thái ON
        ↓
Máy chủ nhận trạng thái
```

Dễ nhớ:

> Device kết nối mạng qua Wi-Fi, giao tiếp với máy chủ bằng MQTT, nhận lệnh điều khiển và gửi trạng thái thiết bị ngược lại.

---

# DEBUG

## 45. GDB / gdbserver là gì?

### GDB

Chạy trên máy phát triển, dùng để điều khiển quá trình debug.

Có thể:

- đặt breakpoint
- xem biến
- xem call stack
- chạy từng bước

### gdbserver

Chạy trên thiết bị OpenWrt.

Nó cho phép GDB trên máy phát triển kết nối tới chương trình đang chạy trên device.

Flow:

```text
Laptop
GDB
  │
  │ kết nối qua mạng
  ↓
OpenWrt Device
gdbserver
  │
  ↓
Application C/C++
```

Dễ nhớ:

> GDB ở laptop, gdbserver ở device.

---

# TÓM TẮT CÔNG VIỆC FPT TELECOM

## 46. Cách nhớ ngắn gọn

```text
C/C++
  ↓
GCC Toolchain + OpenWrt SDK
  ↓
Build
  ↓
Executable / .ipk
  ↓
Deploy lên device OpenWrt
  ↓
Device kết nối Wi-Fi
  ↓
Giao tiếp máy chủ qua MQTT
  ↓
Nhận lệnh / điều khiển / gửi trạng thái
  ↓
Test + Debug
```

## 47. Câu trả lời phỏng vấn ngắn

> Em phát triển application C/C++ chạy trên OpenWrt cho FPT Play Box và Smart Home. Công việc chính của em là xử lý kết nối Wi-Fi/MQTT, nhận lệnh điều khiển từ máy chủ, thực hiện trên thiết bị và gửi trạng thái thiết bị ngược lại. Software được cross-compile bằng GCC Toolchain và OpenWrt SDK, sau đó deploy lên device để functional test, integration test và debug.

---

# LINUX / OPENWRT THỰC TẾ

## 48. Các lệnh Linux cơ bản thường dùng

| Lệnh | Công dụng |
|---|---|
| `ls`, `cd`, `pwd` | Xem và di chuyển giữa các thư mục |
| `cp`, `mv`, `rm` | Sao chép, di chuyển và xóa file |
| `find` | Tìm file |
| `grep` | Tìm nội dung trong text/log |
| `ps`, `top` | Xem process và mức sử dụng tài nguyên |
| `df`, `du`, `free` | Kiểm tra disk và RAM |
| `ip addr`, `ip route` | Xem IP và routing table |
| `ping` | Kiểm tra khả năng kết nối mạng |
| `logread`, `dmesg` | Đọc system log và kernel log |

**Trả lời phỏng vấn:** Khi debug thiết bị Linux/OpenWrt, em thường kiểm tra process, log, CPU/RAM, IP, route và khả năng kết nối mạng bằng các lệnh Linux cơ bản.

---

## 49. Quyền truy cập file trong Linux

Linux có ba quyền cơ bản:

- `r`: read – đọc.
- `w`: write – ghi.
- `x`: execute – thực thi.

Ba nhóm người dùng:

- Owner.
- Group.
- Others.

```bash
chmod +x my_app
chmod 755 my_app
```

> Nếu executable không có quyền `x`, chương trình không thể chạy trực tiếp.

---

## 50. Signal và graceful shutdown

Signal là cơ chế hệ điều hành gửi thông báo bất đồng bộ cho process.

| Signal | Ý nghĩa |
|---|---|
| `SIGINT` | Thường phát sinh khi nhấn `Ctrl+C` |
| `SIGTERM` | Yêu cầu process kết thúc an toàn |
| `SIGKILL` | Buộc process dừng ngay, không thể bắt hoặc xử lý |
| `SIGSEGV` | Process truy cập bộ nhớ không hợp lệ |

Graceful shutdown nghĩa là process nhận yêu cầu dừng, ngừng nhận công việc mới, đóng socket/file, lưu trạng thái cần thiết rồi thoát.

**Trả lời phỏng vấn:** Nên ưu tiên `SIGTERM` để application có cơ hội dọn dẹp tài nguyên. `SIGKILL` chỉ dùng khi process không thể dừng theo cách thông thường.

---

## 51. Daemon, service và `procd`

- **Daemon**: process chạy nền, thường hoạt động lâu dài.
- **Service**: chương trình nền được hệ thống quản lý.
- **`procd`**: trình quản lý process/service của OpenWrt.

Các thao tác thường dùng:

```bash
/etc/init.d/my_app start
/etc/init.d/my_app stop
/etc/init.d/my_app restart
/etc/init.d/my_app enable
```

`enable` cho phép service tự chạy khi thiết bị khởi động.

---

## 52. `logread` và `dmesg` khác nhau thế nào?

| Công cụ | Nội dung chính |
|---|---|
| `logread` | Đọc system log và log do service/application gửi vào hệ thống log |
| `dmesg` | Đọc kernel ring buffer, thường dùng cho driver, boot và lỗi kernel |

Ví dụ:

```bash
logread -f
dmesg | grep wlan
```

> Debug application/service thường xem `logread`; debug kernel, driver hoặc thiết bị phần cứng thường xem `dmesg`.

---

## 53. `opkg` là gì?

`opkg` là package manager thường dùng trên OpenWrt.

```bash
opkg update
opkg install my_app.ipk
opkg remove my_app
opkg list-installed
```

**Trả lời phỏng vấn:** `opkg` dùng để cài, gỡ và quản lý package `.ipk` trên OpenWrt.

---

## 54. UCI là gì?

UCI – Unified Configuration Interface – là hệ thống cấu hình thống nhất của OpenWrt.

Các file cấu hình thường nằm trong:

```text
/etc/config/
```

Ví dụ:

```bash
uci get network.lan.ipaddr
uci set my_app.main.enable='1'
uci commit my_app
```

> `uci set` thay đổi cấu hình trong bộ nhớ; `uci commit` lưu thay đổi xuống file cấu hình.

---

## 55. `ubus` là gì?

`ubus` là cơ chế IPC của OpenWrt, cho phép các service gọi method và trao đổi dữ liệu với nhau.

```bash
ubus list
ubus call system board
```

**Trả lời phỏng vấn:** `ubus` là message bus nội bộ của OpenWrt. Application có thể dùng nó để gọi service, lấy trạng thái hoặc phát sự kiện mà không phải tự xây dựng toàn bộ giao thức IPC.

---

## 56. `netifd` là gì?

`netifd` là daemon quản lý network interface trong OpenWrt.

Nó xử lý:

- Interface lên hoặc xuống.
- Cấu hình IP.
- DHCP client.
- Thay đổi trạng thái mạng.

> UCI lưu cấu hình mạng; `netifd` đọc cấu hình và áp dụng vào hệ thống.

---

## 57. Overlay filesystem trong OpenWrt là gì?

OpenWrt thường có:

- `/rom`: firmware gốc, chỉ đọc.
- `/overlay`: lưu các thay đổi có thể ghi.
- `/`: kết quả kết hợp giữa `/rom` và `/overlay`.

**Trả lời phỏng vấn:** Overlay filesystem giúp firmware gốc vẫn giữ nguyên trong `/rom`, còn cấu hình và package người dùng cài thêm được lưu ở `/overlay`.

---

# BUILD / DEPLOY THỰC TẾ

## 58. Package Makefile của OpenWrt dùng để làm gì?

Package Makefile mô tả:

- Tên và phiên bản package.
- Phụ thuộc vào library/package nào.
- Cách configure và compile.
- File nào được cài vào package `.ipk`.
- Application được đặt vào thư mục nào trên device.

> Makefile thông thường build source; package Makefile của OpenWrt còn tích hợp source đó vào hệ thống build và đóng gói `.ipk`.

---

## 59. Target architecture, ABI, sysroot và `staging_dir`

| Khái niệm | Ý nghĩa |
|---|---|
| Target architecture | Kiến trúc CPU của thiết bị như ARM, MIPS, AArch64 |
| ABI | Quy ước binary giữa chương trình, compiler, library và hệ điều hành |
| Sysroot | Cây thư mục mô phỏng root filesystem của target để compiler tìm header/library |
| `staging_dir` | Nơi OpenWrt build system đặt toolchain, header và library đã chuẩn bị cho target |

> Chương trình phải được build đúng architecture và ABI của target. Đúng ARM nhưng sai ABI hoặc sai C library vẫn có thể không chạy.

---

## 60. Static library và shared library khác nhau thế nào?

| Static library | Shared library |
|---|---|
| Thường có đuôi `.a` | Thường có đuôi `.so` |
| Code được đưa vào executable khi link | Được nạp khi chương trình chạy |
| Executable lớn hơn | Executable nhỏ hơn |
| Ít phụ thuộc library trên device | Device phải có đúng shared library tương thích |

Kiểm tra shared library:

```bash
ldd my_app
```

---

## 61. Quy trình build và deploy package OpenWrt

```text
Source C/C++
    ↓
OpenWrt SDK + package Makefile
    ↓
Cross-compile
    ↓
my_app.ipk
    ↓ scp
OpenWrt device
    ↓ opkg install
Start/restart service
    ↓
Kiểm tra process + log + chức năng
```

Ví dụ:

```bash
scp my_app.ipk root@192.168.1.1:/tmp/
ssh root@192.168.1.1
opkg install /tmp/my_app.ipk
/etc/init.d/my_app restart
ps | grep my_app
logread -f
```

---

## 62. Vì sao executable đã copy sang device nhưng không chạy?

Các nguyên nhân thường gặp:

- Sai CPU architecture.
- Sai ABI hoặc C library.
- Thiếu quyền execute.
- Thiếu shared library.
- Dynamic loader không tồn tại.
- File bị hỏng hoặc copy chưa hoàn chỉnh.
- Chương trình cần file cấu hình nhưng không tìm thấy.

Các lệnh kiểm tra:

```bash
file my_app
chmod +x my_app
ldd my_app
```

---

# SOCKET / NETWORK PROGRAMMING

## 63. Luồng hoạt động của TCP client

```text
socket()
   ↓
connect()
   ↓
send() / recv()
   ↓
close()
```

**Trả lời phỏng vấn:** TCP client tạo socket, chủ động kết nối tới IP và port của server, trao đổi dữ liệu rồi đóng socket. Chương trình thực tế phải xử lý timeout, partial read/write và mất kết nối.

---

## 64. Luồng hoạt động của TCP server

```text
socket()
   ↓
bind()
   ↓
listen()
   ↓
accept()
   ↓
recv() / send()
   ↓
close()
```

**Trả lời phỏng vấn:** TCP server bind socket với địa chỉ/port, listen kết nối đến và dùng `accept()` tạo socket riêng để giao tiếp với từng client.

---

## 65. Partial `send()` và `recv()` là gì?

TCP là byte stream. Một lần `send()` có thể gửi ít hơn số byte yêu cầu và một lần `recv()` có thể nhận chưa đủ một message.

Do đó cần:

- Kiểm tra giá trị trả về.
- Lặp cho đến khi gửi/nhận đủ dữ liệu cần thiết.
- Tự thiết kế ranh giới message bằng length field, delimiter hoặc protocol.

> Không được giả định một lần `send()` tương ứng chính xác với một lần `recv()`.

---

## 66. Blocking và non-blocking socket khác nhau thế nào?

| Blocking | Non-blocking |
|---|---|
| Lệnh có thể chờ cho đến khi có dữ liệu/kết quả | Lệnh trả về ngay nếu chưa thể thực hiện |
| Code đơn giản hơn | Phải quản lý trạng thái và sự kiện |
| Có thể làm thread bị kẹt nếu không có timeout | Phù hợp khi xử lý nhiều kết nối |

**Trả lời phỏng vấn:** Blocking socket dễ viết nhưng phải có timeout hoặc thread riêng. Non-blocking socket không giữ thread chờ nhưng cần event loop như `select`, `poll` hoặc `epoll`.

---

## 67. `select`, `poll` và `epoll` dùng để làm gì?

Chúng giúp một thread theo dõi nhiều file descriptor/socket.

| Cơ chế | Điểm chính |
|---|---|
| `select` | Phổ biến nhưng giới hạn số descriptor và phải tạo lại tập descriptor |
| `poll` | Không có giới hạn nhỏ cố định như `select`, nhưng vẫn duyệt toàn bộ danh sách |
| `epoll` | Linux-specific, hiệu quả hơn khi quản lý nhiều kết nối |

> Với số lượng socket nhỏ, `select` hoặc `poll` có thể đủ; `epoll` phù hợp server Linux có nhiều kết nối.

---

## 68. Heartbeat, timeout và exponential backoff

- **Heartbeat**: message định kỳ để kiểm tra phía bên kia còn hoạt động.
- **Timeout**: thời gian chờ tối đa.
- **Retry**: thử lại sau khi thất bại.
- **Exponential backoff**: thời gian chờ tăng dần sau mỗi lần thất bại.
- **Jitter**: thêm độ trễ ngẫu nhiên để nhiều device không reconnect cùng lúc.

Ví dụ:

```text
Retry sau: 1s → 2s → 4s → 8s → tối đa 60s
```

> Không nên retry liên tục không có delay vì sẽ tốn CPU, mạng và tạo tải lớn lên server.

---

## 69. Wi-Fi STA, AP và DHCP là gì?

| Khái niệm | Ý nghĩa |
|---|---|
| STA – Station | Device kết nối vào một Access Point |
| AP – Access Point | Thiết bị phát Wi-Fi cho client khác kết nối |
| DHCP client | Xin địa chỉ IP từ DHCP server |
| DHCP server | Cấp IP, gateway và DNS cho client |

Luồng STA cơ bản:

```text
Scan AP → Authentication/Association → DHCP → Có IP → Kết nối server
```

> Kết nối được Wi-Fi chưa chắc đã có IP hoặc truy cập được Internet/server.

---

# MQTT THỰC TẾ

## 70. Last Will and Testament – LWT là gì?

LWT là message được client đăng ký với broker khi kết nối. Nếu client mất kết nối bất thường, broker publish message này thay client.

Ví dụ:

```text
Topic: device/123/status
Payload: offline
```

**Trả lời phỏng vấn:** LWT giúp hệ thống phát hiện và thông báo device offline khi device mất kết nối đột ngột, không kịp tự gửi trạng thái.

---

## 71. Clean session/Clean start và persistent session

- **Clean session/Clean start**: client yêu cầu bắt đầu phiên mới, không dùng trạng thái subscription cũ.
- **Persistent session**: broker giữ một số trạng thái phiên như subscription và message phù hợp khi client mất kết nối, tùy phiên bản MQTT và QoS.

**Trả lời phỏng vấn:** Nếu cần nhận lại dữ liệu sau khi reconnect, có thể dùng persistent session. Nếu luôn muốn khởi tạo sạch và subscribe lại, dùng clean session/start phù hợp.

---

## 72. Wildcard `+` và `#` trong MQTT topic

- `+`: khớp đúng một cấp topic.
- `#`: khớp nhiều cấp còn lại và phải nằm ở cuối filter.

```text
home/+/status   → home/light/status, home/fan/status
home/#          → mọi topic bắt đầu bằng home/
```

---

## 73. Bảo mật MQTT

Các biện pháp cơ bản:

- Username/password hoặc certificate để xác thực.
- TLS để mã hóa đường truyền.
- Phân quyền topic – ACL.
- Kiểm tra payload và giới hạn kích thước message.
- Không hard-code secret trong source code.

**Trả lời phỏng vấn:** MQTT không tự động an toàn chỉ vì dùng broker. Cần xác thực, mã hóa TLS, phân quyền topic và kiểm tra dữ liệu nhận được.

---

## 74. Vì sao QoS 1 có thể làm lệnh được xử lý hai lần?

QoS 1 bảo đảm **at least once**. Nếu sender không nhận được ACK, message có thể được gửi lại nên subscriber có thể nhận trùng.

Giải pháp:

- Mỗi command có `message_id` hoặc `request_id`.
- Device lưu/kiểm tra ID đã xử lý.
- Thiết kế thao tác idempotent nếu có thể.

> Idempotent nghĩa là thực hiện cùng một lệnh nhiều lần vẫn tạo ra kết quả cuối giống nhau.

---

## 75. Luồng MQTT reconnect an toàn

```text
Phát hiện mất MQTT
        ↓
Đánh dấu trạng thái disconnected
        ↓
Chờ bằng exponential backoff + jitter
        ↓
Kiểm tra Wi-Fi/IP/DNS
        ↓
Kết nối lại broker
        ↓
Xác thực + subscribe lại nếu cần
        ↓
Đồng bộ trạng thái device
        ↓
Tiếp tục publish/subscribe
```

> Wi-Fi còn kết nối không có nghĩa MQTT vẫn còn kết nối. Hai trạng thái phải được quản lý riêng.

---

## 76. JSON, serialization và kiểm tra payload

Device thường nhận payload dạng JSON hoặc định dạng nhị phân.

Khi xử lý cần kiểm tra:

- Message có parse được không.
- Field bắt buộc có tồn tại không.
- Kiểu dữ liệu và phạm vi giá trị.
- Kích thước payload.
- Phiên bản protocol.
- Command có được phép thực hiện không.

**Trả lời phỏng vấn:** Không nên tin trực tiếp dữ liệu từ server hoặc broker. Application phải parse có kiểm soát, validate field và xử lý message lỗi mà không làm process crash.

---

# TEST / DEBUG / SOURCE CONTROL

## 77. Unit Test, Functional Test, Integration Test và System Test

| Loại test | Kiểm tra gì? | Ví dụ | Framework/công cụ thường dùng |
|---|---|---|---|
| Unit Test | Kiểm tra từng hàm hoặc class riêng lẻ | Test hàm parse MQTT payload | C: Unity, CMock, Ceedling. C++: GoogleTest, GoogleMock |
| Functional Test | Kiểm tra một chức năng có đúng yêu cầu không | Device nhận lệnh `ON` và bật thiết bị | Python, Pytest, Robot Framework, shell script hoặc công cụ test nội bộ |
| Integration Test | Kiểm tra nhiều thành phần kết hợp với nhau | Server → MQTT Broker → Device → gửi trạng thái phản hồi | Robot Framework, Pytest, Mosquitto client, Paho MQTT, test script |
| System Test | Kiểm tra toàn bộ hệ thống trong môi trường gần thực tế | App, backend, broker, network và device hoạt động cùng nhau | Thiết bị thật, test bench, hệ thống backend test và công cụ mô phỏng network |
Công cụ Unit Test cho C

Công cụ	Dùng để làm gì?
Unity	Framework viết và chạy Unit Test cho C
CMock	Tự động tạo mock/stub cho các function C phụ thuộc
Ceedling	Quản lý build và chạy test; tích hợp Unity và CMock
Unity: dùng để viết test và kiểm tra kết quả bằng assertion.
CMock: giả lập các thành phần phụ thuộc như MQTT, Wi-Fi hoặc hardware API.
Ceedling: quản lý việc build, chạy và tổng hợp kết quả test.

Unity được thiết kế riêng cho Unit Test C và phù hợp với embedded; CMock tạo mock/stub cho C; Ceedling kết hợp Unity, CMock và hệ thống build test. Unity, CMock, Ceedling

Công cụ Unit Test cho C++
Công cụ	Dùng để làm gì?
GoogleTest	Framework viết và chạy Unit Test cho C++
GoogleMock	Tạo mock object để giả lập các class phụ thuộc

GoogleTest là framework Unit Test C++; GoogleMock được tích hợp cùng dự án và hỗ trợ tạo mock class. GoogleTest/GoogleMock

Ví dụ liên quan FPT Telecom
Hàm cần test:
ParseMqttCommand("{\"command\":\"ON\"}")

Unit Test kiểm tra:
- Payload hợp lệ → trả về lệnh ON.
- Thiếu field command → báo lỗi.
- Payload sai định dạng → không làm process crash.
- Command không hỗ trợ → từ chối xử lý.
Trả lời phỏng vấn

Unit Test kiểm tra từng hàm hoặc class riêng lẻ. Với C có thể dùng Unity, CMock và Ceedling; với C++ có thể dùng GoogleTest và GoogleMock. Functional Test kiểm tra một chức năng, Integration Test kiểm tra nhiều thành phần kết hợp và System Test kiểm tra toàn bộ hệ thống trong môi trường gần thực tế.

Lưu ý: đây là các công cụ thường dùng, chưa thể khẳng định dự án FPT Telecom thực tế dùng chính xác framework nào nếu team chưa cung cấp thông tin.
## 78. Các công cụ debug Linux ngoài GDB

| Công cụ | Công dụng |
|---|---|
| Log | Theo dõi flow, trạng thái và lỗi runtime |
| `strace` | Theo dõi system call của process |
| Core dump | Lưu trạng thái process khi crash để phân tích sau |
| `addr2line` | Chuyển địa chỉ crash thành file và dòng source khi có symbol |
| `ldd` | Kiểm tra shared library phụ thuộc |
| AddressSanitizer | Phát hiện out-of-bounds, use-after-free và lỗi bộ nhớ khi build phù hợp |
| Valgrind | Phát hiện leak/lỗi bộ nhớ nếu target hoặc môi trường test hỗ trợ |

> Trên thiết bị tài nguyên hạn chế, thường ưu tiên log, core dump và remote GDB; sanitizer/Valgrind có thể chạy trên môi trường test tương thích.

---

## 79. Cách kiểm tra process dùng CPU hoặc RAM cao

Quy trình cơ bản:

1. Dùng `top` hoặc `ps` xác định process.
2. Xem log có vòng lặp retry liên tục hay không.
3. Kiểm tra số thread, socket và file descriptor.
4. Kiểm tra memory leak hoặc queue tăng không giới hạn.
5. Dùng profiler/debugger nếu môi trường hỗ trợ.

Nguyên nhân thường gặp:

- Busy loop không có sleep/wait.
- Reconnect liên tục.
- Thread không thoát.
- Memory leak.
- Message queue không được tiêu thụ kịp.

---

## 80. Git và SVN cơ bản

| Git | SVN | Công dụng |
|---|---|---|
| `git clone` | `svn checkout` | Lấy source lần đầu |
| `git pull` | `svn update` | Cập nhật source |
| `git status` | `svn status` | Xem file thay đổi |
| `git diff` | `svn diff` | Xem nội dung thay đổi |
| `git commit` | `svn commit` | Ghi nhận thay đổi |
| `git revert` | `svn revert` | Hoàn tác theo cơ chế tương ứng |

**Khác nhau cơ bản:** Git là hệ thống version control phân tán; mỗi developer thường có repository local đầy đủ. SVN là hệ thống tập trung, phụ thuộc nhiều hơn vào repository server trung tâm.

---

## 81. Tình huống: Device mất Wi-Fi thì xử lý thế nào?

**Cách trả lời phỏng vấn:**

> Em tách trạng thái Wi-Fi, IP và MQTT. Khi mất Wi-Fi, application đánh dấu offline, dừng hoặc đưa các thao tác mạng vào hàng chờ, sau đó reconnect bằng backoff để tránh retry liên tục. Khi Wi-Fi kết nối lại và nhận được IP, em reconnect MQTT, subscribe lại nếu cần, đồng bộ trạng thái mới nhất với server rồi tiếp tục xử lý message. Trong quá trình này cần có log, timeout và giới hạn kích thước hàng chờ.

---

## 82. Tình huống: Service bị crash hoặc không tự chạy sau reboot

Quy trình kiểm tra:

1. Dùng `ps` kiểm tra process có chạy không.
2. Kiểm tra `/etc/init.d/my_app` và trạng thái `enable`.
3. Đọc `logread` và core dump nếu có.
4. Kiểm tra executable, quyền chạy, library và file cấu hình.
5. Chạy application thủ công để tái hiện lỗi.
6. Kiểm tra cấu hình `procd` có respawn service khi cần không.

**Trả lời phỏng vấn:** Em xác định service chưa start hay đã start rồi crash, sau đó kiểm tra init script, log, dependency, quyền, library và cấu hình. Nếu service quan trọng, có thể cấu hình `procd` respawn nhưng vẫn phải tìm và sửa nguyên nhân crash, không chỉ restart vô hạn.

---

# TÓM TẮT FPT TELECOM MỞ RỘNG

```text
Viết C/C++
    ↓
Cross-compile đúng architecture + ABI
    ↓
Đóng gói .ipk và cài bằng opkg
    ↓
Chạy service bằng procd/init script
    ↓
Đọc cấu hình UCI, giao tiếp nội bộ qua ubus
    ↓
Quản lý Wi-Fi/IP/MQTT bằng state machine
    ↓
Nhận command, kiểm tra payload, điều khiển device
    ↓
Publish trạng thái về server
    ↓
Xử lý timeout, retry, duplicate và reconnect
    ↓
Test + log + GDB/core dump + theo dõi CPU/RAM
```

</details>


<!-- BEGIN: KIEN THUC 1-125 NHUNG -->

<details>
<summary><strong>Kiến thức 1–125 (Nhúng) — C/C++, RTOS, MCU, Qt, Linux, GUI</strong></summary>

> Nhóm câu hỏi bổ sung phục vụ phỏng vấn Embedded/Medical Device. Mỗi câu có thể bấm để đóng hoặc mở. Số thứ tự trong nhóm này độc lập với các mục hiện có trong README.


### C/C++ Embedded cơ bản

<details>
<summary><strong>Câu 1: Giới thiệu kinh nghiệm Embedded C/C++</strong></summary>

**Trả lời phỏng vấn:** Tôi có kinh nghiệm C/C++ trong dự án AUTOSAR Automotive và phần mềm máy siêu âm; đã làm việc với giao diện, đa luồng, giao tiếp và debug.

**Giải thích / ví dụ:** Embedded: hệ thống nhúng; debug: tìm và xử lý lỗi; multithreading: lập trình đa luồng.

</details>

<details>
<summary><strong>Câu 2: Bạn từng làm dự án nhúng nào?</strong></summary>

**Trả lời phỏng vấn:** Tôi từng tham gia kiểm thử EthIf trong AUTOSAR Classic cho ECU dùng Infineon AURIX TC39x và phát triển phần mềm C++ cho máy siêu âm.

**Giải thích / ví dụ:** ECU: bộ điều khiển điện tử; MCU: vi điều khiển; EthIf: giao diện Ethernet của AUTOSAR.

</details>

<details>
<summary><strong>Câu 3: Embedded System là gì?</strong></summary>

**Trả lời phỏng vấn:** Hệ thống nhúng là hệ thống máy tính được thiết kế cho chức năng cụ thể, thường chịu giới hạn về bộ nhớ và tài nguyên.

**Giải thích / ví dụ:** Ví dụ: ECU ô tô, máy đo huyết áp, bộ điều khiển cảm biến.

</details>

<details>
<summary><strong>Câu 4: C và C++ khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** C chủ yếu lập trình thủ tục; C++ hỗ trợ thêm class, lập trình hướng đối tượng, template và thư viện chuẩn.

**Giải thích / ví dụ:** OOP: lập trình hướng đối tượng; class: khuôn mẫu tạo đối tượng.

</details>

<details>
<summary><strong>Câu 5: Pointer là gì?</strong></summary>

**Trả lời phỏng vấn:** Pointer (con trỏ) là biến lưu địa chỉ bộ nhớ của đối tượng hoặc hàm; `&a` lấy địa chỉ, `*p` truy cập giá trị mà `p` trỏ tới.

**Giải thích / ví dụ:** Ví dụ: `int a=10; int *p=&a;` thì `*p` là 10.

</details>

<details>
<summary><strong>Câu 6: Pointer và Reference khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** Pointer lưu địa chỉ, có thể là `nullptr` và đổi đối tượng được trỏ tới; reference là bí danh gắn với đối tượng khi khởi tạo.

**Giải thích / ví dụ:** Reference: tham chiếu; `nullptr`: giá trị con trỏ rỗng.

</details>

<details>
<summary><strong>Câu 7: Stack và Heap khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** Stack thường chứa thông tin gọi hàm và biến cục bộ; heap dùng để cấp phát động và cần quản lý vòng đời tài nguyên.

**Giải thích / ví dụ:** Stack: ngăn xếp; heap: vùng cấp phát động; memory leak: rò rỉ bộ nhớ.

</details>

<details>
<summary><strong>Câu 8: Memory leak là gì?</strong></summary>

**Trả lời phỏng vấn:** Memory leak là tình trạng vùng nhớ đã cấp phát không được giải phóng khi không còn sử dụng, khiến bộ nhớ khả dụng giảm dần.

**Giải thích / ví dụ:** Ví dụ: gọi `malloc()` liên tục mà quên `free()`.

</details>

<details>
<summary><strong>Câu 9: Xử lý memory leak như thế nào?</strong></summary>

**Trả lời phỏng vấn:** Kiểm tra vị trí cấp phát và giải phóng, sử dụng RAII và smart pointer khi phù hợp; dùng công cụ theo dõi bộ nhớ để xác định lỗi.

**Giải thích / ví dụ:** RAII: quản lý tài nguyên theo vòng đời đối tượng; smart pointer: con trỏ thông minh.

</details>

<details>
<summary><strong>Câu 10: `volatile` dùng để làm gì?</strong></summary>

**Trả lời phỏng vấn:** `volatile`   báo cho compiler biết rằng giá trị của biến có thể thay đổi bất ngờ từ bên ngoài luồng thực thi hiện tại( ngắt hoặc thanh ghi phần cứng), nên mỗi lần đọc/ghi phải truy cập giá trị thực trong memory, không được tự ý tối ưu bằng cách giữ giá trị cũ trong register.

**Giải thích / ví dụ:** Compiler: trình biên dịch; hardware register: thanh ghi phần cứng; ISR: hàm xử lý ngắt.

</details>

<details>
<summary><strong>Câu 11: `volatile` có đảm bảo an toàn đa luồng không?</strong></summary>

**Trả lời phỏng vấn:** Không. `volatile` không bảo đảm thao tác nguyên tử hoặc đồng bộ giữa các luồng; cần atomic, mutex hoặc cơ chế phù hợp.

**Giải thích / ví dụ:** Thread-safe: an toàn đa luồng; atomic: thao tác nguyên tử; mutex: khóa loại trừ lẫn nhau.

</details>

<details>
<summary><strong>Câu 12: `const` và `constexpr` khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** `const` ngăn sửa giá trị qua biến đó; `constexpr` cho phép và yêu cầu khởi tạo phù hợp với biểu thức hằng tại thời điểm biên dịch.

**Giải thích / ví dụ:** Compile time: thời điểm biên dịch; runtime: khi chương trình chạy.

</details>

<details>
<summary><strong>Câu 13: `struct` và `class` trong C++ khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** Hai loại có khả năng gần giống nhau, nhưng `struct` mặc định thành viên và kế thừa là `public`, còn `class` mặc định là `private`.

**Giải thích / ví dụ:** Public: truy cập công khai; private: truy cập giới hạn.

</details>

<details>
<summary><strong>Câu 14: OOP gồm những đặc điểm nào?</strong></summary>

**Trả lời phỏng vấn:** Bốn đặc điểm chính: đóng gói, kế thừa, đa hình và trừu tượng.

**Giải thích / ví dụ:** Encapsulation: đóng gói; inheritance: kế thừa; polymorphism: đa hình; abstraction: trừu tượng.

</details>

<details>
<summary><strong>Câu 15: Polymorphism là gì?</strong></summary>

**Trả lời phỏng vấn:** Đa hình cho phép cùng một lời gọi qua giao diện chung có hành vi khác nhau theo loại đối tượng thực tế.

**Giải thích / ví dụ:** Ví dụ: `Animal::sound()` được `Dog` và `Cat` ghi đè khác nhau.

</details>

<details>
<summary><strong>Câu 16: Virtual function là gì?</strong></summary>

**Trả lời phỏng vấn:** Hàm ảo `virtual` cho phép gọi đúng bản ghi đè ở class con thông qua con trỏ hoặc tham chiếu class cha khi chạy.

**Giải thích / ví dụ:** Override: ghi đè; runtime: khi chương trình đang chạy.

</details>

<details>
<summary><strong>Câu 17: Virtual destructor để làm gì?</strong></summary>

**Trả lời phỏng vấn:** Bảo đảm xóa đối tượng class con qua con trỏ class cha sẽ gọi đúng các destructor liên quan.

**Giải thích / ví dụ:** Destructor: hàm hủy để dọn dẹp tài nguyên.

</details>

<details>
<summary><strong>Câu 18: Smart pointer là gì?</strong></summary>

**Trả lời phỏng vấn:** Con trỏ thông minh hỗ trợ quản lý vòng đời đối tượng: `unique_ptr` sở hữu duy nhất, `shared_ptr` chia sẻ sở hữu, `weak_ptr` không sở hữu.

**Giải thích / ví dụ:** RAII: tự dọn tài nguyên khi đối tượng quản lý kết thúc vòng đời.

</details>


### RTOS và tác vụ thời gian thực

<details>
<summary><strong>Câu 19: RTOS là gì?</strong></summary>

**Trả lời phỏng vấn:** RTOS là hệ điều hành thời gian thực, tập trung vào việc đáp ứng các giới hạn thời gian cần thiết cho các tác vụ.

**Giải thích / ví dụ:** RTOS: Real-Time Operating System; deadline: thời hạn; deterministic: có thể dự đoán/giới hạn thời gian đáp ứng.

</details>

<details>
<summary><strong>Câu 20: RTOS khác hệ điều hành thông thường thế nào?</strong></summary>

**Trả lời phỏng vấn:** RTOS nhấn mạnh việc đáp ứng thời hạn và thời gian phản hồi có giới hạn; hệ điều hành đa dụng thường tập trung hiệu suất tổng thể và trải nghiệm sử dụng.

**Giải thích / ví dụ:** Throughput: lượng công việc xử lý trong một đơn vị thời gian; user experience: trải nghiệm người dùng.

</details>

<details>
<summary><strong>Câu 21: Task trong RTOS là gì?</strong></summary>

**Trả lời phỏng vấn:** Task là đơn vị công việc được RTOS lập lịch, có ngăn xếp, mức ưu tiên và trạng thái riêng.

**Giải thích / ví dụ:** Task priority: mức ưu tiên giúp hệ điều hành quyết định tác vụ nào chạy trước.

</details>

<details>
<summary><strong>Câu 22: Các trạng thái Task trong RTOS là gì?</strong></summary>

**Trả lời phỏng vấn:** Running: đang chạy; Ready: sẵn sàng nhưng chờ CPU; Blocked: chờ sự kiện/thời gian; Suspended: bị tạm dừng.

**Giải thích / ví dụ:** Các tên và cơ chế cụ thể phụ thuộc RTOS; đây là bốn trạng thái thường gặp.

</details>

<details>
<summary><strong>Câu 23: Scheduler là gì?</strong></summary>

**Trả lời phỏng vấn:** Scheduler là bộ lập lịch, chọn task được CPU thực thi theo quy tắc và mức ưu tiên.

**Giải thích / ví dụ:** Scheduling policy: quy tắc lập lịch; priority: mức ưu tiên.

</details>

<details>
<summary><strong>Câu 24: Preemptive scheduling là gì?</strong></summary>

**Trả lời phỏng vấn:** Lập lịch có quyền ngắt: khi task ưu tiên cao sẵn sàng, nó có thể giành CPU từ task ưu tiên thấp đang chạy.

**Giải thích / ví dụ:** Task priority: độ ưu tiên của tác vụ; preemptive: cho phép tạm dừng task hiện tại.

</details>

<details>
<summary><strong>Câu 25: Context switch là gì?</strong></summary>

**Trả lời phỏng vấn:** Context switch là việc lưu trạng thái thực thi của task hiện tại và khôi phục trạng thái của task khác để chuyển CPU.

**Giải thích / ví dụ:** Context: các thanh ghi, con trỏ stack và trạng thái cần thiết để tiếp tục thực thi.

</details>

<details>
<summary><strong>Câu 26: Mutex và Semaphore khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** Mutex dùng để bảo vệ tài nguyên chung với cơ chế sở hữu khóa; semaphore là tín hiệu/bộ đếm để đồng bộ hoặc giới hạn tài nguyên.

**Giải thích / ví dụ:** Mutex: khóa loại trừ; semaphore: cơ chế đồng bộ bằng tín hiệu/bộ đếm.

</details>

<details>
<summary><strong>Câu 27: Khi nào dùng mutex?</strong></summary>

**Trả lời phỏng vấn:** Khi nhiều task cùng truy cập tài nguyên hoặc dữ liệu dùng chung và cần ngăn thao tác đồng thời gây sai dữ liệu.

**Giải thích / ví dụ:** Shared resource: tài nguyên dùng chung; critical section: đoạn mã cần bảo vệ.

</details>

<details>
<summary><strong>Câu 28: Race condition là gì?</strong></summary>

**Trả lời phỏng vấn:** Race condition xảy ra khi kết quả xử lý phụ thuộc không mong muốn vào thứ tự hoặc thời điểm các tác vụ thực thi.

**Giải thích / ví dụ:** Data race: truy cập xung đột bộ nhớ giữa các luồng không được đồng bộ đúng cách.

</details>

<details>
<summary><strong>Câu 29: Deadlock là gì?</strong></summary>

**Trả lời phỏng vấn:** Deadlock xảy ra khi các task chờ tài nguyên do nhau giữ và không task nào tiến triển.

**Giải thích / ví dụ:** Ví dụ: A giữ khóa 1 chờ khóa 2, B giữ khóa 2 chờ khóa 1.

</details>

<details>
<summary><strong>Câu 30: Cách phòng tránh Deadlock?</strong></summary>

**Trả lời phỏng vấn:** Giữ thứ tự khóa nhất quán, hạn chế thời gian giữ khóa, tránh khóa lồng nhau không cần thiết và dùng timeout khi phù hợp.

**Giải thích / ví dụ:** Timeout: thời hạn chờ tối đa; lock: khóa tài nguyên.

</details>

<details>
<summary><strong>Câu 31: Priority inversion là gì?</strong></summary>

**Trả lời phỏng vấn:** Đảo ngược ưu tiên: task ưu tiên cao bị chặn bởi task ưu tiên thấp đang giữ tài nguyên; có thể giảm bằng kế thừa ưu tiên.

**Giải thích / ví dụ:** Priority inheritance: tạm nâng mức ưu tiên của task đang giữ khóa.

</details>

<details>
<summary><strong>Câu 32: ISR là gì?</strong></summary>

**Trả lời phỏng vấn:** ISR là hàm xử lý ngắt, thực thi để phản ứng với sự kiện ngắt của phần cứng hoặc hệ thống.

**Giải thích / ví dụ:** ISR: Interrupt Service Routine; interrupt: ngắt.

</details>

<details>
<summary><strong>Câu 33: Cần chú ý gì trong ISR?</strong></summary>

**Trả lời phỏng vấn:** ISR phải ngắn, tránh hàm chờ lâu hoặc xử lý nặng; nên chuyển công việc tiếp theo cho task phù hợp.

**Giải thích / ví dụ:** Blocking: phải chờ; interrupt latency: độ trễ xử lý ngắt.

</details>

<details>
<summary><strong>Câu 34: ISR có nên dùng Mutex không?</strong></summary>

**Trả lời phỏng vấn:** Không dùng mutex thông thường có thể chờ trong ISR. Nên dùng API được RTOS cho phép gọi từ ngắt như queue/semaphore dạng FromISR.

**Giải thích / ví dụ:** FromISR: các hàm riêng để gọi từ hàm xử lý ngắt trong FreeRTOS.

</details>


### Đa luồng

<details>
<summary><strong>Câu 35: Process và Thread khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** Process là tiến trình với không gian địa chỉ riêng; các thread cùng process chia sẻ nhiều tài nguyên nhưng có stack và luồng thực thi riêng.

**Giải thích / ví dụ:** Process: tiến trình; thread: luồng thực thi; shared memory: bộ nhớ dùng chung.

</details>

<details>
<summary><strong>Câu 36: Bạn có kinh nghiệm multithreading không?</strong></summary>

**Trả lời phỏng vấn:** Trong dự án phần mềm máy siêu âm tôi từng xử lý các chức năng UI, cơ sở dữ liệu và tác vụ nền liên quan đa luồng, chú ý đồng bộ và quản lý tài nguyên.

**Giải thích / ví dụ:** UI thread: luồng giao diện; background task: tác vụ nền.

</details>

<details>
<summary><strong>Câu 37: Truyền dữ liệu giữa các thread bằng cách nào?</strong></summary>

**Trả lời phỏng vấn:** Có thể dùng dữ liệu chung kết hợp mutex, hàng đợi thông điệp, tín hiệu sự kiện hoặc condition variable.

**Giải thích / ví dụ:** Message queue: hàng đợi thông điệp; synchronization: đồng bộ.

</details>


### UART, SPI, I2C và Ethernet

<details>
<summary><strong>Câu 38: UART là gì?</strong></summary>

**Trả lời phỏng vấn:** UART là giao tiếp nối tiếp không đồng bộ; thường dùng TX để truyền, RX để nhận, có nối đất chung phù hợp và không có đường clock riêng.

**Giải thích / ví dụ:** UART: Universal Asynchronous Receiver/Transmitter; TX: phát; RX: nhận.

</details>

<details>
<summary><strong>Câu 39: UART cần cấu hình gì?</strong></summary>

**Trả lời phỏng vấn:** Cần thống nhất baud rate, số bit dữ liệu, parity và số bit dừng giữa hai thiết bị.

**Giải thích / ví dụ:** Baud rate: tốc độ ký hiệu; parity: bit kiểm tra chẵn lẻ; stop bit: bit dừng.

</details>

<details>
<summary><strong>Câu 40: UART và SPI khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** UART không đồng bộ; SPI đồng bộ bằng clock riêng và thường hỗ trợ truyền nhận hai chiều cùng lúc.

**Giải thích / ví dụ:** SPI: Serial Peripheral Interface; SCLK: xung nhịp; MOSI/MISO: đường dữ liệu; CS: chọn thiết bị.

</details>

<details>
<summary><strong>Câu 41: SPI và I2C khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** SPI thường nhanh, dùng nhiều dây; I2C sử dụng hai dây SDA/SCL và địa chỉ để kết nối nhiều thiết bị.

**Giải thích / ví dụ:** I2C: Inter-Integrated Circuit; SDA: dữ liệu; SCL: nhịp đồng hồ.

</details>

<details>
<summary><strong>Câu 42: UART không nhận được dữ liệu thì debug thế nào?</strong></summary>

**Trả lời phỏng vấn:** Kiểm tra dây TX/RX/GND, mức điện áp, baud rate, parity, cấu hình chân/clock/driver, ngắt hoặc DMA và buffer nhận.

**Giải thích / ví dụ:** DMA: truyền dữ liệu trực tiếp; buffer: bộ đệm; logic analyzer: máy phân tích tín hiệu số.

</details>

<details>
<summary><strong>Câu 43: Ethernet hoạt động cơ bản thế nào?</strong></summary>

**Trả lời phỏng vấn:** Ethernet là công nghệ mạng cục bộ; driver xử lý giao tiếp phần cứng, còn các giao thức như IP/TCP/UDP thực hiện truyền dữ liệu ở tầng trên.

**Giải thích / ví dụ:** LAN: mạng cục bộ; driver: phần mềm điều khiển phần cứng.

</details>

<details>
<summary><strong>Câu 44: TCP và UDP khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** TCP tạo kết nối, cung cấp luồng byte tin cậy và đúng thứ tự; UDP gửi datagram không bảo đảm nhận được hay đúng thứ tự.

**Giải thích / ví dụ:** Packet/datagram: gói dữ liệu; overhead: phần chi phí xử lý bổ sung.

</details>

<details>
<summary><strong>Câu 45: Truyền dữ liệu quan trọng nên chọn TCP hay UDP?</strong></summary>

**Trả lời phỏng vấn:** Nếu cần truyền tin cậy và đúng thứ tự thường chọn TCP; nếu ưu tiên độ trễ và tự xử lý mất gói có thể chọn UDP.

**Giải thích / ví dụ:** Latency: độ trễ; reliability: độ tin cậy. TCP vẫn cần xử lý lỗi, timeout ở tầng ứng dụng.

</details>


### Cảm biến và cơ cấu chấp hành

<details>
<summary><strong>Câu 46: Sensor và Actuator khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** Sensor thu thập thông tin từ môi trường; actuator nhận lệnh và tạo tác động vật lý.

**Giải thích / ví dụ:** Sensor: cảm biến; actuator: cơ cấu chấp hành. Ví dụ: cảm biến nhiệt độ và động cơ.

</details>

<details>
<summary><strong>Câu 47: Xử lý dữ liệu sensor thế nào?</strong></summary>

**Trả lời phỏng vấn:** Đọc qua giao tiếp phần cứng, kiểm tra hợp lệ, hiệu chuẩn/lọc nhiễu khi cần rồi chuyển dữ liệu cho chức năng xử lý.

**Giải thích / ví dụ:** Calibration: hiệu chuẩn; filter: lọc nhiễu; validation: kiểm tra hợp lệ.

</details>

<details>
<summary><strong>Câu 48: Sensor trả giá trị bất thường thì làm gì?</strong></summary>

**Trả lời phỏng vấn:** Kiểm tra giới hạn dữ liệu, trạng thái cảm biến, giao tiếp và timeout; bỏ dữ liệu sai và xử lý an toàn theo yêu cầu hệ thống.

**Giải thích / ví dụ:** Range: khoảng giá trị; timeout: hết thời gian chờ.

</details>

<details>
<summary><strong>Câu 49: Actuator không phản hồi thì làm gì?</strong></summary>

**Trả lời phỏng vấn:** Kiểm tra lệnh gửi, kết nối, nguồn, phản hồi và timeout; báo lỗi hoặc đưa hệ thống về trạng thái an toàn phù hợp.

**Giải thích / ví dụ:** Feedback: tín hiệu phản hồi; fail-safe: chuyển sang trạng thái an toàn khi lỗi.

</details>


### Qt và giao diện người dùng

<details>
<summary><strong>Câu 50: Bạn có kinh nghiệm Qt không?</strong></summary>

**Trả lời phỏng vấn:** Tôi có kinh nghiệm GUI bằng C++/MFC; chưa có nhiều kinh nghiệm Qt thực tế nhưng đã quen cơ chế sự kiện và xử lý đa luồng nên có nền tảng học Qt.

**Giải thích / ví dụ:** Qt: framework C++; GUI: giao diện đồ họa; MFC: thư viện GUI cho Windows.

</details>

<details>
<summary><strong>Câu 51: Signal và Slot trong Qt là gì?</strong></summary>

**Trả lời phỏng vấn:** Signal phát thông báo khi có sự kiện; slot là hàm xử lý tín hiệu đã kết nối.

**Giải thích / ví dụ:** Ví dụ: bấm nút phát signal, slot cập nhật màn hình.

</details>

<details>
<summary><strong>Câu 52: Event-driven programming là gì?</strong></summary>

**Trả lời phỏng vấn:** Lập trình hướng sự kiện: chương trình phản ứng khi xảy ra sự kiện như nhấn nút, timer hết thời gian hoặc nhận dữ liệu.

**Giải thích / ví dụ:** Event: sự kiện; handler: hàm xử lý sự kiện.

</details>

<details>
<summary><strong>Câu 53: Có nên xử lý nặng trong UI thread không?</strong></summary>

**Trả lời phỏng vấn:** Không, vì có thể làm giao diện không phản hồi. Chuyển xử lý nặng sang worker thread và cập nhật GUI bằng cơ chế an toàn cho luồng giao diện.

**Giải thích / ví dụ:** UI thread: luồng giao diện; worker thread: luồng xử lý phụ; freeze: đứng giao diện.

</details>

<details>
<summary><strong>Câu 54: Thiết kế UI cho AED thế nào?</strong></summary>

**Trả lời phỏng vấn:** UI phải rõ ràng, ít thao tác, chỉ dẫn dễ hiểu và cảnh báo nổi bật để người dùng thao tác đúng trong tình huống khẩn cấp.

**Giải thích / ví dụ:** AED: máy khử rung tim tự động bên ngoài; usability: tính dễ sử dụng.

</details>


### Thiết bị y tế và an toàn

<details>
<summary><strong>Câu 55: Bạn có kinh nghiệm Medical Device không?</strong></summary>

**Trả lời phỏng vấn:** Tôi từng làm phần mềm C++ cho máy siêu âm, liên quan GUI, cơ sở dữ liệu, đa luồng và giao tiếp DICOM.

**Giải thích / ví dụ:** DICOM: chuẩn dữ liệu/hình ảnh y tế; medical device: thiết bị y tế.

</details>

<details>
<summary><strong>Câu 56: Medical Device khác phần mềm thông thường ở đâu?</strong></summary>

**Trả lời phỏng vấn:** Phần mềm thiết bị y tế đòi hỏi chú trọng an toàn, độ tin cậy, đánh giá rủi ro, kiểm thử, tài liệu và truy vết.

**Giải thích / ví dụ:** Safety: an toàn; reliability: độ tin cậy; traceability: truy vết yêu cầu tới thiết kế và test.

</details>

<details>
<summary><strong>Câu 57: IEC 62304 là gì?</strong></summary>

**Trả lời phỏng vấn:** IEC 62304 là tiêu chuẩn về quy trình vòng đời phần mềm thiết bị y tế, gồm phát triển, bảo trì, quản lý cấu hình và xử lý vấn đề.

**Giải thích / ví dụ:** Software lifecycle: vòng đời phần mềm; configuration management: quản lý cấu hình.

</details>

<details>
<summary><strong>Câu 58: Vì sao Reliability quan trọng với AED?</strong></summary>

**Trả lời phỏng vấn:** Thiết bị cấp cứu phải hoạt động đúng, ổn định và sẵn sàng khi cần; lỗi phần mềm có thể ảnh hưởng trực tiếp đến bệnh nhân.

**Giải thích / ví dụ:** Reliability: độ tin cậy; availability: độ sẵn sàng; safety-critical: liên quan an toàn nghiêm trọng.

</details>

<details>
<summary><strong>Câu 59: Fail-safe là gì?</strong></summary>

**Trả lời phỏng vấn:** Fail-safe là cơ chế đưa hệ thống về trạng thái an toàn phù hợp khi phát hiện lỗi để giảm nguy cơ gây hại.

**Giải thích / ví dụ:** Safe state: trạng thái an toàn, phụ thuộc thiết kế và phân tích rủi ro của thiết bị.

</details>

<details>
<summary><strong>Câu 60: Watchdog Timer dùng để làm gì?</strong></summary>

**Trả lời phỏng vấn:** Watchdog giám sát tiến độ chương trình; nếu không được cập nhật đúng hạn, nó có thể kích hoạt reset hoặc biện pháp phục hồi đã thiết kế.

**Giải thích / ví dụ:** Watchdog: bộ định thời giám sát; reset: khởi động lại.

</details>

<details>
<summary><strong>Câu 61: Tăng reliability của Embedded Software thế nào?</strong></summary>

**Trả lời phỏng vấn:** Kiểm tra đầu vào, xử lý lỗi/timeout, thiết kế trạng thái rõ ràng, giám sát hoạt động và kiểm thử đầy đủ.

**Giải thích / ví dụ:** Input validation: kiểm tra dữ liệu; state machine: máy trạng thái.

</details>

<details>
<summary><strong>Câu 62: Hàm quan trọng trả lỗi thì làm gì?</strong></summary>

**Trả lời phỏng vấn:** Kiểm tra mã lỗi, ghi log, xác định nguyên nhân, phục hồi nếu phù hợp và không để hệ thống tiếp tục ở trạng thái mất an toàn.

**Giải thích / ví dụ:** Error code: mã lỗi; log: bản ghi hoạt động; recovery: phục hồi.

</details>

<details>
<summary><strong>Câu 63: Điều gì quan trọng nhất khi phát triển thiết bị y tế?</strong></summary>

**Trả lời phỏng vấn:** Tôi ưu tiên an toàn và độ tin cậy, sau đó bảo đảm khả năng kiểm thử, bảo trì, truy vết và dễ sử dụng.

**Giải thích / ví dụ:** Maintainability: khả năng bảo trì; testability: khả năng kiểm thử.

</details>


### Kiểm thử và debug

<details>
<summary><strong>Câu 64: Bạn thường debug C/C++ thế nào?</strong></summary>

**Trả lời phỏng vấn:** Tái hiện lỗi, đọc log, đặt breakpoint, theo dõi call stack và giá trị biến, xác định nguyên nhân gốc rồi kiểm thử lại sau khi sửa.

**Giải thích / ví dụ:** Breakpoint: điểm dừng; call stack: danh sách lời gọi hàm; root cause: nguyên nhân gốc.

</details>

<details>
<summary><strong>Câu 65: Unit Test là gì?</strong></summary>

**Trả lời phỏng vấn:** Unit Test kiểm tra một đơn vị nhỏ như hàm hoặc module riêng lẻ có hoạt động đúng yêu cầu hay không.

**Giải thích / ví dụ:** Test case: trường hợp kiểm thử; assertion: điều kiện kiểm tra kết quả.

</details>

<details>
<summary><strong>Câu 66: Stub và Mock là gì?</strong></summary>

**Trả lời phỏng vấn:** Stub cung cấp hành vi giả để thay dependency; mock thường còn cho phép kiểm tra lời gọi và tương tác với dependency.

**Giải thích / ví dụ:** Dependency: thành phần phụ thuộc; mock/stub: đối tượng hoặc hàm giả lập khi test.

</details>

<details>
<summary><strong>Câu 67: Bạn đã sử dụng công cụ Unit Test nào?</strong></summary>

**Trả lời phỏng vấn:** Tại dự án AUTOSAR tôi sử dụng mSQT để thiết kế và thực hiện các test case cho API của EthIf.

**Giải thích / ví dụ:** API: giao diện lập trình; EthIf: Ethernet Interface; mSQT: công cụ/framework kiểm thử dự án.

</details>

<details>
<summary><strong>Câu 68: Boundary Value Testing là gì?</strong></summary>

**Trả lời phỏng vấn:** Kiểm thử các giá trị ở và quanh ranh giới cho phép, ví dụ giới hạn 0–100 thì thử -1, 0, 1, 99, 100, 101.

**Giải thích / ví dụ:** Boundary: biên; range: khoảng hợp lệ.

</details>

<details>
<summary><strong>Câu 69: Regression Test là gì?</strong></summary>

**Trả lời phỏng vấn:** Kiểm thử hồi quy là chạy lại các test cần thiết sau khi sửa lỗi hoặc thêm chức năng để phát hiện tác động đến phần cũ.

**Giải thích / ví dụ:** Regression: lỗi tái phát hoặc tác động không mong muốn sau thay đổi.

</details>


### Thiết kế phần mềm

<details>
<summary><strong>Câu 70: State Machine là gì?</strong></summary>

**Trả lời phỏng vấn:** State Machine mô hình hóa hành vi hệ thống bằng các trạng thái và điều kiện chuyển giữa các trạng thái.

**Giải thích / ví dụ:** State: trạng thái; transition: chuyển trạng thái; event: sự kiện.

</details>

<details>
<summary><strong>Câu 71: Vì sao State Machine dùng nhiều trong Embedded?</strong></summary>

**Trả lời phỏng vấn:** Thiết bị thường có trạng thái rõ ràng như Init, Idle, Running, Error nên máy trạng thái giúp logic dễ kiểm soát và kiểm thử.

**Giải thích / ví dụ:** Init: khởi tạo; Idle: chờ; Running: hoạt động; Error: lỗi.

</details>

<details>
<summary><strong>Câu 72: Thiết kế module Embedded thế nào?</strong></summary>

**Trả lời phỏng vấn:** Chia phần mềm thành các module có nhiệm vụ và giao diện rõ ràng, tách logic ứng dụng khỏi phần phụ thuộc phần cứng khi phù hợp.

**Giải thích / ví dụ:** Module: thành phần; interface: giao diện kết nối; abstraction: trừu tượng.

</details>

<details>
<summary><strong>Câu 73: Low Coupling có lợi gì?</strong></summary>

**Trả lời phỏng vấn:** Các module ít phụ thuộc nhau sẽ dễ sửa đổi, kiểm thử và bảo trì mà giảm ảnh hưởng dây chuyền.

**Giải thích / ví dụ:** Coupling: độ phụ thuộc giữa các module; low coupling: phụ thuộc thấp.

</details>

<details>
<summary><strong>Câu 74: Software Design Document là gì?</strong></summary>

**Trả lời phỏng vấn:** Tài liệu thiết kế phần mềm mô tả kiến trúc, module, giao diện, luồng dữ liệu và cách xử lý chức năng.

**Giải thích / ví dụ:** Basic Design: thiết kế cơ bản; Detailed Design: thiết kế chi tiết.

</details>


### Kinh nghiệm dự án

<details>
<summary><strong>Câu 75: Trong AUTOSAR bạn đã làm gì?</strong></summary>

**Trả lời phỏng vấn:** Tôi kiểm thử module EthIf: đọc SWS/ECUC, viết test case, cấu hình stub, chạy test và so sánh kết quả.

**Giải thích / ví dụ:** SWS: đặc tả phần mềm; ECUC: cấu hình ECU; EthIf: Ethernet Interface.

</details>

<details>
<summary><strong>Câu 76: EthIf nằm ở đâu trong AUTOSAR?</strong></summary>

**Trả lời phỏng vấn:** EthIf là lớp giao diện giữa các module phía trên như TcpIp/EthSM và phần Ethernet Driver/Transceiver ở dưới.

**Giải thích / ví dụ:** EthSM: bộ quản lý trạng thái Ethernet; transceiver: bộ thu phát.

</details>

<details>
<summary><strong>Câu 77: MCU dự án Automotive là gì?</strong></summary>

**Trả lời phỏng vấn:** Dự án chủ yếu dùng Infineon AURIX TC39x, compiler TASKING và công cụ Mobilgene để cấu hình BSW.

**Giải thích / ví dụ:** BSW: phần mềm nền tảng AUTOSAR; compiler: trình biên dịch.

</details>

<details>
<summary><strong>Câu 78: Bạn từng làm giao thức truyền thông nào?</strong></summary>

**Trả lời phỏng vấn:** Tôi từng làm kiểm thử Ethernet/EthIf trong AUTOSAR, có kiến thức CAN và các giao tiếp nhúng; dự án thiết bị y tế có các chức năng DICOM.

**Giải thích / ví dụ:** CAN: mạng điều khiển trên ô tô; DICOM: tiêu chuẩn dữ liệu y tế.

</details>

<details>
<summary><strong>Câu 79: Trong dự án máy siêu âm bạn làm gì?</strong></summary>

**Trả lời phỏng vấn:** Tôi phát triển và sửa lỗi ứng dụng C++ máy siêu âm liên quan UI, đa luồng, cơ sở dữ liệu và DICOM.

**Giải thích / ví dụ:** UI: giao diện người dùng; database: cơ sở dữ liệu.

</details>

<details>
<summary><strong>Câu 80: Ví dụ một bug khó bạn từng xử lý?</strong></summary>

**Trả lời phỏng vấn:** Tôi xử lý lỗi hộp thoại bị đóng khi callback gửi dữ liệu vẫn đang chạy, bằng cách quản lý trình tự hoàn tất callback trước khi đóng tài nguyên liên quan.

**Giải thích / ví dụ:** Callback: hàm gọi lại; lifecycle: vòng đời tài nguyên; synchronization: đồng bộ.

</details>


### Tình huống kỹ thuật

<details>
<summary><strong>Câu 81: Task RTOS chạy quá lâu thì sao?</strong></summary>

**Trả lời phỏng vấn:** Các task khác có thể bị trễ hoặc lỡ deadline; cần đo thời gian chạy, kiểm tra mức ưu tiên và giảm thao tác gây chờ.

**Giải thích / ví dụ:** Execution time: thời gian chạy; missed deadline: trễ thời hạn.

</details>

<details>
<summary><strong>Câu 82: Sensor phải đọc mỗi 10 ms nhưng task mất 20 ms thì sao?</strong></summary>

**Trả lời phỏng vấn:** Phân tích thời gian thực thi và ưu tiên; tối ưu hoặc tách đọc cảm biến ra khỏi xử lý nặng để đáp ứng chu kỳ 10 ms.

**Giải thích / ví dụ:** Periodic task: tác vụ định kỳ; acquisition: lấy mẫu dữ liệu.

</details>

<details>
<summary><strong>Câu 83: Hai Task cùng truy cập Buffer thì làm sao?</strong></summary>

**Trả lời phỏng vấn:** Bảo vệ buffer chung bằng mutex, hoặc truyền dữ liệu qua queue để hạn chế truy cập chung trực tiếp.

**Giải thích / ví dụ:** Buffer: bộ đệm; queue: hàng đợi.

</details>

<details>
<summary><strong>Câu 84: Ethernet mất kết nối thì xử lý sao?</strong></summary>

**Trả lời phỏng vấn:** Phát hiện link-down hoặc timeout, cập nhật trạng thái, thử kết nối lại theo chính sách và không dùng dữ liệu đã không hợp lệ.

**Giải thích / ví dụ:** Link-down: mất liên kết; reconnect: kết nối lại.

</details>

<details>
<summary><strong>Câu 85: UI bị đứng nhưng hệ thống điều khiển vẫn chạy thì debug gì?</strong></summary>

**Trả lời phỏng vấn:** Kiểm tra UI thread có bị chặn do tác vụ nặng, mutex hoặc chờ thread khác; xem log, call stack và trao đổi sự kiện.

**Giải thích / ví dụ:** Freeze: không phản hồi; blocking: bị chặn/chờ.

</details>

<details>
<summary><strong>Câu 86: Chương trình crash ngẫu nhiên sau vài giờ thì nghĩ đến gì?</strong></summary>

**Trả lời phỏng vấn:** Kiểm tra rò rỉ bộ nhớ/tài nguyên, tràn bộ đệm, con trỏ treo, lỗi đa luồng và dữ liệu đầu vào bất thường.

**Giải thích / ví dụ:** Crash: dừng bất thường; dangling pointer: con trỏ trỏ tới đối tượng hết hạn.

</details>

<details>
<summary><strong>Câu 87: Hardware trả dữ liệu sai thì Software làm gì?</strong></summary>

**Trả lời phỏng vấn:** Kiểm tra dữ liệu, giới hạn và trạng thái giao tiếp; phát hiện lỗi, không đưa dữ liệu sai vào xử lý nguy hiểm.

**Giải thích / ví dụ:** Hardware: phần cứng; validation: xác thực dữ liệu.

</details>

<details>
<summary><strong>Câu 88: Software và Hardware không thống nhất nguyên nhân lỗi thì làm gì?</strong></summary>

**Trả lời phỏng vấn:** Thu thập log, tái hiện vấn đề, xác nhận hợp đồng giao tiếp và đo tín hiệu thực tế cùng nhóm phần cứng.

**Giải thích / ví dụ:** Evidence: bằng chứng; interface: giao diện giao tiếp.

</details>


### IDE, vi điều khiển và trình biên dịch

<details>
<summary><strong>Câu 89: SEGGER Embedded Studio là gì?</strong></summary>

**Trả lời phỏng vấn:** SEGGER Embedded Studio là IDE lập trình C/C++ cho hệ thống nhúng, hỗ trợ biên dịch, nạp và debug các vi điều khiển được hỗ trợ.

**Giải thích / ví dụ:** IDE: môi trường phát triển tích hợp; SEGGER: nhà sản xuất công cụ.

</details>

<details>
<summary><strong>Câu 90: IAR Embedded Workbench là gì?</strong></summary>

**Trả lời phỏng vấn:** IAR Embedded Workbench là môi trường phát triển nhúng có compiler, linker và debugger phục vụ chương trình C/C++ trên MCU.

**Giải thích / ví dụ:** Compiler: biên dịch; linker: liên kết; debugger: công cụ sửa lỗi.

</details>

<details>
<summary><strong>Câu 91: IDE là gì và bạn dùng IDE nào?</strong></summary>

**Trả lời phỏng vấn:** IDE là môi trường tích hợp để viết, build và debug; tôi từng sử dụng Visual Studio cho C++ máy siêu âm và Eclipse CDT cho AUTOSAR.

**Giải thích / ví dụ:** IDE: Integrated Development Environment; build: tạo chương trình.

</details>

<details>
<summary><strong>Câu 92: Vi điều khiển khác vi xử lý thế nào?</strong></summary>

**Trả lời phỏng vấn:** MCU tích hợp CPU, RAM, Flash và ngoại vi trên chip; MPU tập trung vào năng lực xử lý và thường kết hợp bộ nhớ, ngoại vi ngoài.

**Giải thích / ví dụ:** MCU: Microcontroller Unit (vi điều khiển); MPU: Microprocessor Unit (vi xử lý).

</details>

<details>
<summary><strong>Câu 93: nRF52840 là gì?</strong></summary>

**Trả lời phỏng vấn:** nRF52840 là vi điều khiển/SoC không dây của Nordic Semiconductor, có lõi ARM Cortex-M4, 1 MB Flash, 256 KB RAM và hỗ trợ Bluetooth Low Energy.

**Giải thích / ví dụ:** Cortex-M4: lõi CPU; BLE: Bluetooth tiết kiệm năng lượng; SoC: hệ thống trên chip.

</details>

<details>
<summary><strong>Câu 94: nRF52840 dùng để làm gì? Có trong Router không?</strong></summary>

**Trả lời phỏng vấn:** nRF52840 dùng cho thiết bị IoT, cảm biến, thiết bị đeo và đồ dùng Bluetooth. Arduino Nano 33 BLE Sense, Adafruit Feather nRF52840, Nordic nRF52840 DK là các bo mạch dùng chip này. Router Wi-Fi thông thường không dùng nó làm bộ xử lý chính, nhưng một số thiết bị có thể dùng nó làm chip không dây phụ.

**Giải thích / ví dụ:** nRF52840 là MCU, không phải MPU; router thường có SoC mạng riêng; Thread/Zigbee/BLE là các công nghệ không dây.

</details>

<details>
<summary><strong>Câu 95: Cross Compiler là gì?</strong></summary>

**Trả lời phỏng vấn:** Trình biên dịch chéo chạy trên máy phát triển nhưng tạo chương trình cho kiến trúc hoặc môi trường đích khác.

**Giải thích / ví dụ:** Host: máy biên dịch; target: thiết bị chạy; ví dụ `arm-none-eabi-gcc`.

</details>

<details>
<summary><strong>Câu 96: IDE khác Compiler thế nào?</strong></summary>

**Trả lời phỏng vấn:** IDE là môi trường soạn thảo và quản lý dự án; compiler là công cụ dịch mã nguồn thành mã đích.

**Giải thích / ví dụ:** Ví dụ: Visual Studio là IDE; MSVC hoặc GCC là trình biên dịch.

</details>

<details>
<summary><strong>Câu 97: SEGGER Embedded Studio khác IAR thế nào?</strong></summary>

**Trả lời phỏng vấn:** Cả hai đều là IDE nhúng; khác về compiler, debugger, khả năng tối ưu, hệ sinh thái công cụ và kiến trúc hỗ trợ.

**Giải thích / ví dụ:** SEGGER thường kết hợp J-Link; IAR có compiler và debugger C-SPY.

</details>

<details>
<summary><strong>Câu 98: J-Link là gì?</strong></summary>

**Trả lời phỏng vấn:** J-Link là đầu dò nạp và debug của SEGGER, giúp máy tính kết nối MCU để nạp chương trình, đặt breakpoint, xem thanh ghi và bộ nhớ.

**Giải thích / ví dụ:** SWD: giao tiếp debug hai tín hiệu chủ yếu; JTAG: giao tiếp kiểm thử và debug.

</details>

<details>
<summary><strong>Câu 99: Bare-metal và RTOS khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** Bare-metal chạy code trực tiếp không có hệ điều hành; RTOS quản lý task, mức ưu tiên và đồng bộ cho tác vụ thời gian thực.

**Giải thích / ví dụ:** Bare-metal: không OS; scheduling: lập lịch thực thi.

</details>

<details>
<summary><strong>Câu 100: Quy trình phát triển firmware trên MCU?</strong></summary>

**Trả lời phỏng vấn:** Viết mã C/C++, cấu hình build, biên dịch, liên kết, tạo firmware, nạp xuống MCU rồi kiểm thử/debug trên thiết bị.

**Giải thích / ví dụ:** Compile: biên dịch; link: liên kết; flash: nạp vào bộ nhớ chip.

</details>


### TRACE32, router và GDB

<details>
<summary><strong>Câu 101: TRACE32 là gì?</strong></summary>

**Trả lời phỏng vấn:** TRACE32 là bộ công cụ debug và phân tích hệ thống nhúng của Lauterbach; cho phép xem thanh ghi, bộ nhớ, breakpoint và thông tin thực thi trên chip phù hợp.

**Giải thích / ví dụ:** Trace: thông tin theo dõi thực thi; JTAG/SWD: giao tiếp debug phần cứng.

</details>

<details>
<summary><strong>Câu 102: TRACE32 khác J-Link thế nào?</strong></summary>

**Trả lời phỏng vấn:** TRACE32 là hệ thống công cụ debug và phân tích chuyên sâu của Lauterbach; J-Link là dòng đầu dò nạp/debug của SEGGER.

**Giải thích / ví dụ:** Cả hai phụ thuộc vào mức hỗ trợ của chip và giao tiếp debug.

</details>

<details>
<summary><strong>Câu 103: Trong Router thường debug bằng gì?</strong></summary>

**Trả lời phỏng vấn:** Với OpenWrt/Linux, thường dùng SSH, `logread`, `dmesg`, GDB/gdbserver, UART console; khi cần mức thấp có thể dùng JTAG.

**Giải thích / ví dụ:** SSH: truy cập từ xa; UART console: cổng lệnh nối tiếp; `tcpdump`: bắt gói mạng.

</details>

<details>
<summary><strong>Câu 104: GDB và GDB Server là gì?</strong></summary>

**Trả lời phỏng vấn:** GDB là trình debug C/C++; gdbserver chạy trên thiết bị đích để GDB từ máy phát triển có thể kết nối và debug từ xa.

**Giải thích / ví dụ:** GDB: GNU Debugger; remote debugging: debug trên thiết bị khác.

</details>

<details>
<summary><strong>Câu 105: Debug bằng SSH khác JTAG thế nào?</strong></summary>

**Trả lời phỏng vấn:** SSH cần hệ điều hành/dịch vụ mạng hoạt động và thích hợp xem log, chạy công cụ; JTAG cho phép debug CPU/firmware ở mức phần cứng nếu chip và bo mạch hỗ trợ.

**Giải thích / ví dụ:** SSH: kết nối terminal qua mạng; JTAG: giao diện kiểm tra/debug phần cứng.

</details>

<details>
<summary><strong>Câu 106: GDB là gì? Ví dụ debug Router?</strong></summary>

**Trả lời phỏng vấn:** GDB cho phép đặt breakpoint, chạy từng bước, xem biến và call stack. Với OpenWrt có thể chạy `gdbserver :1234 ./app` trên router, dùng GDB phù hợp kiến trúc trên PC và `target remote 192.168.1.1:1234` để kết nối.

**Giải thích / ví dụ:** `break main`: đặt điểm dừng; `next`: chạy qua dòng; `step`: vào hàm; `print x`: xem biến; `backtrace`: xem các hàm đang gọi. Cần chương trình đúng kiến trúc và có debug symbol.

</details>


### Qt/QML và thiết kế GUI

<details>
<summary><strong>Câu 107: Qt là gì?</strong></summary>

**Trả lời phỏng vấn:** Qt là framework ứng dụng C++, hỗ trợ GUI và phát triển trên nhiều nền tảng như Windows, Linux và Embedded Linux.

**Giải thích / ví dụ:** GUI: giao diện đồ họa; framework: bộ thư viện/công cụ; cross-platform: đa nền tảng.

</details>

<details>
<summary><strong>Câu 108: QML là gì, khác C++ thế nào?</strong></summary>

**Trả lời phỏng vấn:** QML là ngôn ngữ khai báo của Qt để mô tả giao diện và tương tác; C++ thường xử lý dữ liệu, chức năng và giao tiếp tầng dưới.

**Giải thích / ví dụ:** QML: Qt Modeling Language; declarative: khai báo giao diện mong muốn.

</details>

<details>
<summary><strong>Câu 109: MOC trong Qt là gì?</strong></summary>

**Trả lời phỏng vấn:** MOC (Meta-Object Compiler) đọc các khai báo mở rộng trong class C++ của Qt, ví dụ `Q_OBJECT`, và sinh mã C++ hỗ trợ meta-object, signal/slot; không phải trình chuyên chuyển QML thành C++.

**Giải thích / ví dụ:** Q_OBJECT: macro Qt; signal: tín hiệu; slot: hàm nhận tín hiệu. Công cụ QML khác gồm qmlcachegen/qmlsc.

</details>

<details>
<summary><strong>Câu 110: RCC trong Qt là gì?</strong></summary>

**Trả lời phỏng vấn:** RCC (Resource Compiler) đóng gói hình ảnh, icon, font và file tài nguyên được khai báo để ứng dụng Qt truy cập thuận tiện.

**Giải thích / ví dụ:** Resource: tài nguyên; QRC: file mô tả tài nguyên.

</details>

<details>
<summary><strong>Câu 111: Figma là gì?</strong></summary>

**Trả lời phỏng vấn:** Figma là công cụ thiết kế giao diện, bố cục và bản mô phỏng tương tác trước khi lập trình.

**Giải thích / ví dụ:** UI: giao diện người dùng; UX: trải nghiệm; prototype: bản mẫu mô phỏng.

</details>

<details>
<summary><strong>Câu 112: Adobe XD là gì?</strong></summary>

**Trả lời phỏng vấn:** Adobe XD là công cụ thiết kế UI/UX và prototype, có mục đích tương tự Figma, không phải compiler hay driver đồ họa.

**Giải thích / ví dụ:** Prototype: mô hình tương tác; design: thiết kế.

</details>


### OpenGL, SDL và hiển thị

<details>
<summary><strong>Câu 113: OpenGL là gì, có phải Driver không?</strong></summary>

**Trả lời phỏng vấn:** OpenGL là API đồ họa cho tác vụ vẽ 2D/3D; không phải driver phần cứng, mà được implementation/driver đồ họa thực thi.

**Giải thích / ví dụ:** API: giao diện lập trình; GPU: bộ xử lý đồ họa; rendering: quá trình tạo hình.

</details>

<details>
<summary><strong>Câu 114: SDL là gì, có phải Driver không?</strong></summary>

**Trả lời phỏng vấn:** SDL (Simple DirectMedia Layer) là thư viện đa phương tiện C/C++ hỗ trợ cửa sổ, sự kiện đầu vào, âm thanh và vẽ hình; không phải GPU driver.

**Giải thích / ví dụ:** Event: sự kiện; window: cửa sổ; SDL có thể kết hợp OpenGL.

</details>

<details>
<summary><strong>Câu 115: OpenGL và SDL khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:** OpenGL tập trung vào API vẽ đồ họa; SDL giúp quản lý cửa sổ, nhập liệu, âm thanh và cung cấp API vẽ 2D, có thể kết hợp OpenGL.

**Giải thích / ví dụ:** Graphics API: giao diện đồ họa; multimedia: đa phương tiện.

</details>

<details>
<summary><strong>Câu 116: Vì sao cài GUI thành công nhưng màn hình không lên hình?</strong></summary>

**Trả lời phỏng vấn:** Ứng dụng có thể thiếu hoặc chọn sai driver, Qt Platform Plugin hay hệ thống hiển thị tương ứng; cần kiểm tra log, đầu ra đồ họa và tính tương thích phần cứng.

**Giải thích / ví dụ:** Platform Plugin: thành phần kết nối Qt với hệ hiển thị; EGLFS/LinuxFB: plugin phổ biến trên nhúng.

</details>


### CMake, GCC và build đa nền tảng

<details>
<summary><strong>Câu 117: CMake là gì?</strong></summary>

**Trả lời phỏng vấn:** CMake là công cụ cấu hình và tạo hệ thống build cho C/C++; xác định mã nguồn, thư viện, compiler và tùy chọn build.

**Giải thích / ví dụ:** `CMakeLists.txt`: file cấu hình; CMake không tự thay thế compiler.

</details>

<details>
<summary><strong>Câu 118: GCC là gì, khác CMake thế nào?</strong></summary>

**Trả lời phỏng vấn:** GCC (GNU Compiler Collection) là bộ trình biên dịch; CMake là công cụ tổ chức và tạo cấu hình build để gọi compiler và công cụ liên quan.

**Giải thích / ví dụ:** GCC: bộ công cụ biên dịch GNU; build: tạo chương trình.

</details>

<details>
<summary><strong>Câu 119: Cross-platform Build là gì?</strong></summary>

**Trả lời phỏng vấn:** Xây dựng phần mềm để có thể biên dịch trên nhiều hệ điều hành/kiến trúc qua cách cấu hình và mã nguồn phù hợp.

**Giải thích / ví dụ:** Cross-platform: đa nền tảng; cross-compilation: biên dịch từ host cho target khác.

</details>

<details>
<summary><strong>Câu 120: Chỉ thị tiền xử lý là gì, dùng cho nhiều nền tảng thế nào?</strong></summary>

**Trả lời phỏng vấn:** Các chỉ thị bắt đầu bằng `#` được xử lý trước biên dịch, ví dụ `#include`, `#define`, `#ifdef`, `#elif`, `#endif` để chọn mã theo nền tảng.

**Giải thích / ví dụ:** Preprocessor: bộ tiền xử lý; conditional compilation: biên dịch có điều kiện.

</details>


### Driver hiển thị trên Embedded Linux

<details>
<summary><strong>Câu 121: Framebuffer là gì?</strong></summary>

**Trả lời phỏng vấn:** Framebuffer là vùng bộ nhớ chứa dữ liệu điểm ảnh để hiển thị; Linux truyền thống thường có giao diện thiết bị `/dev/fb0`.

**Giải thích / ví dụ:** Pixel: điểm ảnh; framebuffer không đồng nghĩa với driver đồ họa.

</details>

<details>
<summary><strong>Câu 122: GPU Driver là gì, khác Framebuffer thế nào?</strong></summary>

**Trả lời phỏng vấn:** GPU driver điều khiển phần cứng đồ họa và có thể hỗ trợ tăng tốc; framebuffer là vùng nhớ chứa hình ảnh, không phải hai loại driver đối lập tuyệt đối.

**Giải thích / ví dụ:** Hardware acceleration: tăng tốc bằng phần cứng; GPU: bộ xử lý đồ họa.

</details>

<details>
<summary><strong>Câu 123: DRM/KMS trên Linux là gì?</strong></summary>

**Trả lời phỏng vấn:** DRM/KMS là hệ thống Linux quản lý thiết bị đồ họa, tài nguyên hiển thị và cấu hình chế độ màn hình.

**Giải thích / ví dụ:** DRM: Direct Rendering Manager; KMS: Kernel Mode Setting; kernel: nhân Linux.

</details>

<details>
<summary><strong>Câu 124: Qt hiển thị GUI trên Embedded Linux bằng cách nào?</strong></summary>

**Trả lời phỏng vấn:** Qt kết nối với hệ thống hiển thị qua Platform Plugin như `linuxfb`, `eglfs` hoặc Wayland theo driver và phần cứng.

**Giải thích / ví dụ:** EGLFS: plugin Qt sử dụng EGL/OpenGL ES; LinuxFB: plugin framebuffer; Wayland: giao thức hiển thị.

</details>

<details>
<summary><strong>Câu 125: Qt chạy nhưng màn hình đen, debug thế nào?</strong></summary>

**Trả lời phỏng vấn:** Xem log và trạng thái ứng dụng, kiểm tra driver/thiết bị `/dev/fb0` hoặc `/dev/dri`, plugin Qt, EGL/GPU và cấu hình màn hình; có thể bật `QT_DEBUG_PLUGINS=1`.

**Giải thích / ví dụ:** `dmesg`: log kernel; DRM/KMS: quản lý hiển thị; không có `/dev/fb0` chưa chắc là lỗi nếu dùng DRM.

</details>



# Kiến thức bổ sung 126–145: Sensor, Actuator và Communication

## I. Sensor – Xử lý dữ liệu cảm biến

<details>
<summary><strong>Câu 126. Sensor là gì?</strong></summary>

**Trả lời phỏng vấn:**

Sensor (cảm biến) là linh kiện dùng để đo các đại lượng vật lý như nhiệt độ, áp suất, nhịp tim hoặc nồng độ oxy trong máu, sau đó chuyển thành tín hiệu để hệ thống xử lý.

**Ví dụ:** Cảm biến nhiệt độ trong máy theo dõi sinh hiệu.

</details>

<details>
<summary><strong>Câu 127. ADC là gì? Tại sao cần ADC?</strong></summary>

**Trả lời phỏng vấn:**

ADC (Analog-to-Digital Converter) là bộ chuyển đổi tín hiệu analog (tương tự) thành giá trị digital (số) để MCU có thể xử lý.

**Ví dụ:** MCU sử dụng ADC để đọc điện áp từ cảm biến nhiệt độ analog.

</details>

<details>
<summary><strong>Câu 128. MCU đọc dữ liệu cảm biến bằng những cách nào?</strong></summary>

**Trả lời phỏng vấn:**

MCU có thể đọc dữ liệu cảm biến bằng:
- **ADC:** Đọc tín hiệu analog.
- **I2C:** Giao tiếp với cảm biến digital.
- **SPI:** Giao tiếp tốc độ cao với cảm biến.
- **UART:** Nhận dữ liệu từ module cảm biến.

Tùy loại cảm biến mà lựa chọn giao tiếp phù hợp.

</details>

<details>
<summary><strong>Câu 129. Polling và Interrupt khác nhau như thế nào?</strong></summary>

**Trả lời phỏng vấn:**

- **Polling:** MCU chủ động kiểm tra trạng thái cảm biến liên tục hoặc định kỳ.
- **Interrupt:** Phần cứng phát tín hiệu ngắt để MCU xử lý sự kiện.

Interrupt giúp tránh kiểm tra liên tục nhưng ISR cần được xử lý ngắn gọn.

</details>

<details>
<summary><strong>Câu 130. Làm thế nào để giảm nhiễu dữ liệu cảm biến?</strong></summary>

**Trả lời phỏng vấn:**

Một số phương pháp:
- Moving Average: Lấy trung bình nhiều mẫu.
- Low-pass Filter: Bộ lọc thông thấp.
- Loại bỏ hoặc đánh dấu các giá trị bất thường.
- Lọc nhiễu bằng phần cứng.

**Ví dụ:** Lấy trung bình 10 mẫu nhiệt độ để giảm dao động do nhiễu.

</details>

<details>
<summary><strong>Câu 131. Sampling Rate là gì?</strong></summary>

**Trả lời phỏng vấn:**

Sampling Rate là tần số lấy mẫu, tức số lần đọc tín hiệu trong một giây.

**Ví dụ:** 100 Hz nghĩa là lấy 100 mẫu mỗi giây.

Tần số lấy mẫu phải phù hợp với đặc tính tín hiệu để tránh mất thông tin và hiện tượng aliasing (chồng phổ).

</details>

<details>
<summary><strong>Câu 132. Nếu cảm biến trả về dữ liệu không hợp lệ, xử lý thế nào?</strong></summary>

**Trả lời phỏng vấn:**

- Kiểm tra trạng thái kết nối và timeout.
- Kiểm tra checksum hoặc CRC nếu có.
- Kiểm tra giá trị có nằm trong phạm vi hợp lệ không.
- Kiểm tra dữ liệu có còn mới không.
- Báo lỗi và đưa hệ thống về trạng thái an toàn phù hợp khi cần.

**Ví dụ:** Nếu mất tín hiệu cảm biến SpO2, thiết bị cần thông báo lỗi đo thay vì tiếp tục hiển thị giá trị cũ như dữ liệu hiện tại.

</details>

---

## II. Actuator – Điều khiển cơ cấu chấp hành

<details>
<summary><strong>Câu 133. Actuator là gì? Cho ví dụ.</strong></summary>

**Trả lời phỏng vấn:**

Actuator (cơ cấu chấp hành) là bộ phận nhận lệnh điều khiển và tạo ra tác động vật lý.

**Ví dụ:**
- Motor: Động cơ.
- Relay: Rơ-le đóng/ngắt mạch.
- Solenoid Valve: Van điện từ.
- Buzzer: Còi cảnh báo.

</details>

<details>
<summary><strong>Câu 134. GPIO điều khiển Actuator như thế nào?</strong></summary>

**Trả lời phỏng vấn:**

MCU sử dụng GPIO để xuất tín hiệu HIGH hoặc LOW nhằm điều khiển mạch driver, transistor hoặc relay.

**Ví dụ:** MCU xuất HIGH để kích hoạt còi cảnh báo thông qua mạch điều khiển.

**Lưu ý:** Không nên nối trực tiếp tải công suất lớn vào GPIO.

</details>

<details>
<summary><strong>Câu 135. PWM là gì? Ứng dụng của PWM?</strong></summary>

**Trả lời phỏng vấn:**

PWM (Pulse Width Modulation) là phương pháp điều chế độ rộng xung.

Thay đổi Duty Cycle (tỷ lệ thời gian tín hiệu ở mức HIGH trong một chu kỳ) để điều chỉnh công suất trung bình đưa tới tải.

**Ứng dụng:**
- Điều chỉnh tốc độ động cơ.
- Điều chỉnh độ sáng LED.
- Điều khiển một số loại cơ cấu chấp hành.

</details>

<details>
<summary><strong>Câu 136. Tại sao cần mạch Driver khi điều khiển động cơ?</strong></summary>

**Trả lời phỏng vấn:**

GPIO của MCU chỉ có khả năng cấp dòng điện nhỏ, trong khi động cơ thường cần dòng điện lớn hơn.

Mạch Driver giúp:
- Cấp công suất phù hợp cho động cơ.
- Điều khiển tốc độ hoặc chiều quay tùy loại driver.
- Bảo vệ MCU khỏi các tác động điện không mong muốn.

</details>

<details>
<summary><strong>Câu 137. Fail-safe là gì trong thiết bị y tế?</strong></summary>

**Trả lời phỏng vấn:**

Fail-safe là thiết kế để khi hệ thống gặp lỗi, thiết bị chuyển về trạng thái an toàn đã được xác định.

**Ví dụ:** Khi phát hiện lỗi điều khiển, hệ thống ngừng một cơ cấu truyền động có nguy cơ gây nguy hiểm và phát cảnh báo.

Trạng thái an toàn phải được xác định theo yêu cầu và phân tích rủi ro của từng thiết bị.

</details>

---

## III. Communication – Giao tiếp và truyền thông

<details>
<summary><strong>Câu 138. USB là gì? Ứng dụng trong Embedded?</strong></summary>

**Trả lời phỏng vấn:**

USB (Universal Serial Bus) là chuẩn kết nối và truyền dữ liệu giữa các thiết bị.

**Ứng dụng:**
- Kết nối thiết bị y tế với máy tính.
- Xuất dữ liệu đo.
- Cập nhật firmware.
- Kết nối thiết bị ngoại vi.

</details>

<details>
<summary><strong>Câu 139. USB Host và USB Device khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:**

- **USB Host:** Quản lý quá trình kết nối và giao tiếp với thiết bị USB.
- **USB Device:** Thiết bị ngoại vi giao tiếp dưới sự quản lý của Host.

**Ví dụ:** Máy tính là USB Host, thiết bị đo kết nối với máy tính có thể hoạt động như USB Device.

</details>

<details>
<summary><strong>Câu 140. Ethernet là gì? Ứng dụng trong thiết bị y tế?</strong></summary>

**Trả lời phỏng vấn:**

Ethernet là công nghệ kết nối mạng có dây.

**Ứng dụng:**
- Truyền dữ liệu đo đến máy tính.
- Kết nối với máy chủ.
- Kết nối hệ thống giám sát.
- Truyền dữ liệu trong mạng nội bộ bệnh viện.

</details>

<details>
<summary><strong>Câu 141. TCP và UDP khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:**

- **TCP:** Giao thức hướng kết nối, bảo đảm dữ liệu được truyền theo đúng thứ tự và có cơ chế truyền lại dữ liệu bị mất.
- **UDP:** Giao thức không kết nối, overhead thấp hơn nhưng không bảo đảm dữ liệu đến nơi hoặc đúng thứ tự.

**Ví dụ:**
- TCP: Truyền dữ liệu cần tính toàn vẹn và thứ tự.
- UDP: Truyền dữ liệu thời gian thực khi ứng dụng có thể chấp nhận một số gói bị mất.

</details>

<details>
<summary><strong>Câu 142. Bluetooth Classic và BLE khác nhau thế nào?</strong></summary>

**Trả lời phỏng vấn:**

- **Bluetooth Classic:** Thường phù hợp với kết nối truyền dữ liệu liên tục như âm thanh.
- **BLE (Bluetooth Low Energy):** Được thiết kế để tiết kiệm năng lượng, phù hợp với cảm biến và thiết bị đeo.

**Ví dụ:** Thiết bị đo sức khỏe đeo tay có thể sử dụng BLE để truyền dữ liệu đến điện thoại.

</details>

<details>
<summary><strong>Câu 143. BLE Central và Peripheral là gì?</strong></summary>

**Trả lời phỏng vấn:**

- **Central:** Thiết bị thường thực hiện tìm kiếm và khởi tạo kết nối.
- **Peripheral:** Thiết bị thường phát quảng bá (advertising) và chấp nhận kết nối.

**Ví dụ:** Điện thoại đóng vai trò Central, cảm biến nhịp tim đóng vai trò Peripheral.

</details>

<details>
<summary><strong>Câu 144. Timeout trong giao tiếp là gì? Tại sao cần Timeout?</strong></summary>

**Trả lời phỏng vấn:**

Timeout là giới hạn thời gian chờ phản hồi từ thiết bị hoặc hệ thống khác.

Nếu hết thời gian mà không nhận được phản hồi, chương trình sẽ báo lỗi hoặc thực hiện cơ chế xử lý phù hợp.

**Ví dụ:** MCU gửi lệnh đọc cảm biến nhưng không nhận được phản hồi trong thời gian quy định thì báo timeout.

**Mục đích:** Tránh chương trình chờ vô thời hạn.

</details>

<details>
<summary><strong>Câu 145. Làm thế nào để kiểm tra dữ liệu truyền nhận có bị lỗi?</strong></summary>

**Trả lời phỏng vấn:**

Một số phương pháp kiểm tra:
- **Checksum:** Kiểm tra tổng kiểm tra dữ liệu.
- **CRC (Cyclic Redundancy Check):** Phát hiện lỗi dữ liệu khi truyền.
- **Length:** Kiểm tra độ dài gói tin.
- **Timeout:** Kiểm tra quá thời gian chờ.
- **Sequence Number:** Kiểm tra thứ tự gói tin nếu giao thức hỗ trợ.

**Ví dụ:** Khi nhận gói tin từ cảm biến, MCU kiểm tra CRC trước khi sử dụng dữ liệu.

</details>

</details>

<!-- END: KIEN THUC 1-125 NHUNG -->
<!--
MỞ RỘNG SAU NÀY:
Có thể thêm một nhóm mới bằng cách sao chép cấu trúc <details> cấp ngoài, ví dụ:

<details>
<summary><h1>Luyện phỏng vấn C/C++ Embedded_1</h1></summary>

... các câu hỏi dạng <details> lồng bên trong ...

</details>
-->
