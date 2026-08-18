<details>
<summary><h1>Luyện phỏng vấn C/C++ Embedded</h1></summary>

> Bản tổng hợp Câu 1–77.  

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

**Trả lời phỏng vấn:** `static` ngoài hàm giới hạn symbol trong file hiện tại; `static` local chỉ khởi tạo một lần và giữ giá trị giữa các lần gọi hàm; `static` member trong class được dùng chung cho tất cả object của class.

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

<!--
MỞ RỘNG SAU NÀY:
Có thể thêm một nhóm mới bằng cách sao chép cấu trúc <details> cấp ngoài, ví dụ:

<details>
<summary><h1>Luyện phỏng vấn C/C++ Embedded_1</h1></summary>

... các câu hỏi dạng <details> lồng bên trong ...

</details>
-->
