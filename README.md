<details>
<summary><h1>Luyện phỏng vấn C/C++ Embedded</h1></summary>

> Bản tổng hợp Câu 1–140.  

> Phần STL đã được tinh gọn theo hướng phỏng vấn: bỏ các câu đi quá sâu vào từng hàm riêng lẻ của `std::vector`.

---

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

Trong bộ kiến thức bạn cung cấp, **Abstraction được liệt kê là một khái niệm chính của OOP nhưng chưa có phần giải thích chi tiết riêng**.

Vì vậy hiện tại ghi nhận:

```text
Các khái niệm chính:

Class
Object
Inheritance
Polymorphism
Abstraction
Encapsulation
```

Phần Abstraction chi tiết chưa được bổ sung để tránh trộn thêm kiến thức ngoài nội dung bạn đang học.

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
<summary><strong>Câu 26: Hàm thuần ảo và abstract class là gì?</strong></summary>

**Trả lời phỏng vấn:** Hàm thuần ảo là hàm `virtual` khai báo bằng `= 0`. Class chứa hàm thuần ảo chưa được cài đặt là abstract class và không thể tạo object trực tiếp.

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

**Trả lời phỏng vấn:** Mảng là vùng nhớ liên tiếp chứa nhiều phần tử cùng kiểu và có kích thước cố định. Con trỏ là biến lưu địa chỉ. Trong hầu hết biểu thức, tên mảng tự chuyển thành con trỏ tới phần tử đầu tiên.

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
<summary><strong>Câu 50: Wild pointer là gì?</strong></summary>

**Trả lời phỏng vấn:** Wild pointer là con trỏ chưa được khởi tạo, nên chứa địa chỉ không xác định. Nên khởi tạo con trỏ bằng `nullptr` nếu chưa có địa chỉ hợp lệ.

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

> Cho phép thao tác đơn giản trên dữ liệu được thực hiện an toàn giữa nhiều thread mà không cần mutex trong nhiều trường hợp.
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

| Loại test | Kiểm tra gì? | Ví dụ |
|---|---|---|
| Unit Test | Hàm/class riêng lẻ | Test hàm parse MQTT payload |
| Functional Test | Một chức năng theo yêu cầu | Device nhận `ON` và bật thiết bị |
| Integration Test | Nhiều thành phần kết hợp | Server → MQTT → device → trạng thái phản hồi |
| System Test | Toàn bộ hệ thống trong môi trường gần thực tế | App, backend, broker, network và device hoạt động cùng nhau |

---

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


<!--
MỞ RỘNG SAU NÀY:
Có thể thêm một nhóm mới bằng cách sao chép cấu trúc <details> cấp ngoài, ví dụ:

<details>
<summary><h1>Luyện phỏng vấn C/C++ Embedded_1</h1></summary>

... các câu hỏi dạng <details> lồng bên trong ...

</details>
-->
