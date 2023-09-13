---
title: "Cấu trúc lặp while và do-while"
description: "Vòng lặp while được sử dụng để thực thi nhiều lần một đoạn chương trình, khi một điều kiện vẫn còn đúng. Vòng lặp while thường được sử dụng khi số lần lặp không được xác định trước"
chapter:
  name: "Nhập môn Java"
  slug: "chuong-02-nhap-mon-java"
category:
  name: "Java"
  slug: "java"
image: https://user-images.githubusercontent.com/29374426/125040448-de6e1b80-e0c1-11eb-935f-2d0b21cab63f.png
position: 21
---

## Vòng lặp while - do

Vòng lặp while được sử dụng để thực thi nhiều lần một đoạn chương trình, khi một điều kiện vẫn còn đúng. Vòng lặp while thường được sử dụng khi số lần lặp không được xác định trước (Không cố định).

Cú pháp:

```java
while (<Điều kiện lặp>) {
  <Khối lệnh lặp lại>
}
```

Ta có thể thấy được luồng thực hiện của chương trình thông qua sơ đồ khối sau

![cau-truc-lap-while-va-do-while](https://user-images.githubusercontent.com/29374426/125040448-de6e1b80-e0c1-11eb-935f-2d0b21cab63f.png)

Trước tiên chương trình sẽ tính giá trị của biểu thức **Điều kiện lặp**, nếu giá trị của biểu thức logic là đúng thì câu lệnh trong thân của `while` sẽ được thực hiện, sau khi thực hiện xong nó tính lại giá trị của biểu thức logic, nếu giá trị đúng nó lại tiếp tục thực hiện lệnh trong thân `while` cho đến khi giá trị của biểu thức sai.

```java
public class Thaycacac {
  public static void main(String[] args) {
    int value = 3;
    // Trong khi 'value' vẫn nhỏ hơn 10 thì vòng lặp vẫn làm việc.
    while (value < 10) {
      System.out.println("Giá trị = " + value);
      // Tăng giá trị của value lên 2 đơn vị
      value = value + 2;
    }
  }
}
```

::result

Giá trị 3<br/>
Giá trị 5<br/>
Giá trị 7<br/>
Giá trị 9

::

::alert{type="infor"}

- Biểu thức bên trong cặp ngoặc tròn phải là một biểu thức logic (biểu thức trả về giá trị true hoặc false)
- Biểu thức điều kiện phải nằm trong cặp ngoặc tròn
- Sau từ khoá while ta chỉ có thể đặt được duy nhất một lệnh, do vậy để có thể thực hiện nhiều tác vụ sau while ta phải bao chúng trong một khối lệnh
- Bên trong thân của vòng lặp while ta nên có lệnh làm thay đổi giá trị của biểu thức logic, nếu không chúng ta sẽ rơi vào vòng lặp vô hạn.
- Câu lệnh trong thân cấu trúc while có thể không được thực hiện lần nào (do biểu thức lôgic ban đầu có giá trị false )

::

## Vòng lặp do - while

```java
do {
  <Khối lệnh lặp lại>
} while(<Điều kiện lặp>)
```

sự hoạt động của cấu trúc này được thể hiện qua sơ đồ sau

![cau-truc-lap-while-va-do-while](https://user-images.githubusercontent.com/29374426/125040504-f04fbe80-e0c1-11eb-82cd-dc68da15782e.png)

Nhìn vào sơ đồ này ta thấy sự hoạt động của nó như sau:

- **Bước 1:** thực hiện lệnh
- **Bước 2:** sau khi thực hiện lệnh xong nó tính giá trị của biểu thức logic
- **Bước 3:** nếu biểu thức logic đúng nó quay trở lại bước 1, nếu sai thì bước 4
- **Bước 4** kết thúc vòng lặp và thực hiện lệnh sau do-while

Ví dụ:

```java
public class Thaycacac {
  public static void main(String[] args) {
      int value = 3;
      // Vòng lặp do-while luôn được thực thi ít nhất 1 lần.
      do {
          System.out.println("Giá trị = " + value);
          // Tăng giá trị cho 'value' thêm 3
          value = value + 3;
      } while (value < 10);
  }
}
```

::result

Giá trị 3<br/>
Giá trị 6<br/>
Giá trị 9

::

::alert{type="infor"}

- Biểu thức bên trong cặp ngoặc tròn phải là một biểu thức logic (biểu thức trả về giá trị true hoặc false)
- Biểu thức điều kiện phải nằm trong cặp ngoặc tròn
- Sau từ khoá do ta có thể đặt được nhiều lệnh
- Bên trong thân của vòng lặp do-while ta nên có lệnh làm thay đổi giá trị của biểu thức logic, nếu không chúng ta sẽ rơi vào vòng lặp vô hạn.
- Câu lệnh trong thân cấu trúc do-while được thực hiện ít nhất một lần

::
