---
title: "LinkedHashSet trong Java"
description: "LinkedHashSet chỉ chứa các phần tử duy nhất, không chấp nhận 2 phần tử trùng nhau,linkedHashSet đảm bảo thứ tự được thêm vào, ngoài ra nó sử dụng đối tượng LinkedHashMap nội bộ để lưu trữ và xử lý các phần tử của nó..."
chapter:
  name: "Java collections"
  slug: "chuong-04-java-collections"
image: https://user-images.githubusercontent.com/29374426/144807571-408057c0-4e0c-4f29-85d4-5c928a669ca3.png
position: 8
---

## Tập hợp `LinkedHashSet` trong Java là gì?

Lớp `LinkedHashSet` trong Java kế thừa `HashSet` và triển khai `Set Interface`. Nó tạo một collection mà sử dụng một Linked List để lưu giữ các phần tử theo thứ tự chúng đã được chèn.

![Tập hợp LinkedHashSet trong Java](https://user-images.githubusercontent.com/29374426/144807571-408057c0-4e0c-4f29-85d4-5c928a669ca3.png)

Các điểm quan trọng về lớp LinkedHashSet trong java là:

- **LinkedHashSet** chỉ chứa các phần tử duy nhất, không chấp nhận 2 phần tử trùng nhau.
- **LinkedHashSet** đảm bảo thứ tự được thêm vào.
- **LinkedHashSet** sử dụng đối tượng LinkedHashMap nội bộ để lưu trữ và xử lý các phần tử của nó.
- **LinkedHashSet** cho phép chứa phần tử NULL.
- **LinkedHashSet** không được đồng bộ. Để có LinkedHashSet đồng bộ, hãy sử dụng phương thức `Collections.synchronizedSet()`.

## Khởi tạo `LinkedHashSet` trong Java

- `LinkedHashSet()`: khởi tạo một danh sách mảng trống.
- `LinkedHashSet(Collection c)`: khởi tạo một danh sách với các phần tử của collection c.

```java
LinkedHashSet<Integer> numbers = new LinkedHashSet<>(8, 0.75);
```

Ở đây, chúng ta đã tạo ra một `LinkedHashSet` có tên là numbers. Chú ý phần code `new LinkedHashSet<>(8, 0.75)`. Ở đây, tham số đầu tiên là capacity và tham số thứ hai là loadFactor.

- **capacity** – Dung lượng của HashSet này là 8. Ý nghĩa, nó có thể lưu trữ 8 phần tử.
- **loadFactor** – Hệ số tải của HashSet này là 0,6. Điều này có nghĩa là: khi nào hash table được lấp đầy 60%, các phần tử mới được nhập vào được chuyển sang hash table mới có kích thước gấp đôi hash table ban đầu.

Có thể tạo một LinkedHashSet mà không cần xác định công suất và hệ số tải của nó.

```java
LinkedHashSet<Integer> numbers1 = new LinkedHashSet<>();
```

Theo mặc định:

- Dung lượng của LinkedHashSet sẽ là 16
- Hệ số tải sẽ là 0,75

## Tạo `LinkedHashSet` từ các collection khác

Chúng ta có thể tạo một LinkedHashSet có chứa tất cả các phần tử của các collection khác theo cách sau đây.

```java
import java.util.LinkedHashSet;
import java.util.ArrayList;

class Main {
    public static void main(String[] args) {
        // Creating an arrayList of even numbers
        ArrayList<Integer> evenNumbers = new ArrayList<>();
        evenNumbers.add(2);
        evenNumbers.add(4);
        System.out.println("ArrayList: " + evenNumbers);

        // Creating a LinkedHashSet from an ArrayList
        LinkedHashSet<Integer> numbers = new LinkedHashSet<>(evenNumbers);
        System.out.println("LinkedHashSet: " + numbers);
    }
}
```

::result

ArrayList: [2, 4]<br/>
LinkedHashSet: [2, 4]

::

## Chèn các phần tử vào `LinkedHashSet`

- `add()` – chèn phần tử được chỉ định vào LinkedHashSet
- `addAll()` – chèn tất cả các phần tử của collection đã chỉ định vào LinkedHashSet

```java
import java.util.LinkedHashSet;

class Main {
    public static void main(String[] args) {
        LinkedHashSet<Integer> evenNumber = new LinkedHashSet<>();

        // Using add() method
        evenNumber.add(2);
        evenNumber.add(4);
        evenNumber.add(6);
        System.out.println("LinkedHashSet: " + evenNumber);

        LinkedHashSet<Integer> numbers = new LinkedHashSet<>();

        // Using addAll() method
        numbers.addAll(evenNumber);
        numbers.add(5);
        System.out.println("New LinkedHashSet: " + numbers);
    }
}
```

::result

LinkedHashSet: [2, 4, 6]<br/>
New LinkedHashSet: [2, 4, 6, 5]

::

## Duyệt qua các phần tử trong `LinkedHashSet`

Để truy cập các phần tử của LinkedHashSet, chúng ta có thể sử dụng hàm `iterator()`. Để sử dụng hàm này, chúng ta phải import gói `java.util.Iterator`.

```java
import java.util.LinkedHashSet;
import java.util.Iterator;

class Main {
    public static void main(String[] args) {
        LinkedHashSet<Integer> numbers = new LinkedHashSet<>();
        numbers.add(2);
        numbers.add(5);
        numbers.add(6);
        System.out.println("LinkedHashSet: " + numbers);

        // Calling the iterator() method
        Iterator<Integer> iterate = numbers.iterator();

        System.out.print("LinkedHashSet using Iterator: ");

        // Accessing elements
        while(iterate.hasNext()) {
            System.out.print(iterate.next());
            System.out.print(", ");
        }
    }
}
```

::result

LinkedHashSet: [2, 5, 6]<br/>
LinkedHashSet using Iterator: 2, 5, 6,

::

Lưu ý:

- `hasNext()` trả về true nếu có một phần tử tiếp theo trong `LinkedHashSet`
- `next()` trả về phần tử tiếp theo trong `LinkedHashSet`

## Xóa các phần tử khỏi `LinkedHashSet`

- `remove()` – xóa phần tử đã chỉ định khỏi LinkedHashSet
- `removeAll()` – loại bỏ tất cả các phần tử khỏi LinkedHashSet

```java
import java.util.LinkedHashSet;

class Main {
    public static void main(String[] args) {
        LinkedHashSet<Integer> numbers = new LinkedHashSet<>();
        numbers.add(2);
        numbers.add(5);
        numbers.add(6);
        System.out.println("LinkedHashSet: " + numbers);

        // Using the remove() method
        boolean value1 = numbers.remove(5);
        System.out.println("Is 5 removed? " + value1);

        boolean value2 = numbers.removeAll(numbers);
        System.out.println("Are all elements removed? " + value2);
    }
}
```

::result

LinkedHashSet: [2, 5, 6]<br/>
Is 5 removed? true<br/>
Are all elements removed? true

::

## Lấy phần hợp của các set

Để Lấy phần hợp của các set, chúng ta có thể sử dụng hàm `addAll()`.

```java
import java.util.LinkedHashSet;

class Main {
    public static void main(String[] args) {
        LinkedHashSet<Integer> evenNumbers = new LinkedHashSet<>();
        evenNumbers.add(2);
        evenNumbers.add(4);
        System.out.println("LinkedHashSet1: " + evenNumbers);

        LinkedHashSet<Integer> numbers = new LinkedHashSet<>();
        numbers.add(1);
        numbers.add(3);
        System.out.println("LinkedHashSet2: " + numbers);

        // Union of two set
        numbers.addAll(evenNumbers);
        System.out.println("Union is: " + numbers);
    }
}
```

::result

LinkedHashSet1: [2, 4]<br/>
LinkedHashSet2: [1, 3]<br/>
Union is: [1, 3, 2, 4]

::

## Lấy phần giao của các set

Để Lấy phần giao của các set, chúng ta có thể sử dụng hàm `retainAll()`

```java
import java.util.LinkedHashSet;

class Main {
    public static void main(String[] args) {
        LinkedHashSet<Integer> primeNumbers = new LinkedHashSet<>();
        primeNumbers.add(2);
        primeNumbers.add(3);
        System.out.println("LinkedHashSet1: " + primeNumbers);

        LinkedHashSet<Integer> evenNumbers = new LinkedHashSet<>();
        evenNumbers.add(2);
        evenNumbers.add(4);
        System.out.println("LinkedHashSet2: " + evenNumbers);

        // Intersection of two sets
        evenNumbers.retainAll(primeNumbers);
        System.out.println("Intersection is: " + evenNumbers);
    }
}
```

::result

LinkedHashSet1: [2, 3]<br/>
LinkedHashSet2: [2, 4]<br/>
Intersection is: [2]

::

## Tìm hiệu của hai set

Để tìm hiệu của hai set, chúng ta có thể sử dụng hàm `removeAll()`.

```java
import java.util.LinkedHashSet;

class Main {
    public static void main(String[] args) {
        LinkedHashSet<Integer> primeNumbers = new LinkedHashSet<>();
        primeNumbers.add(2);
        primeNumbers.add(3);
        primeNumbers.add(5);
        System.out.println("LinkedHashSet1: " + primeNumbers);

        LinkedHashSet<Integer> oddNumbers = new LinkedHashSet<>();
        oddNumbers.add(1);
        oddNumbers.add(3);
        oddNumbers.add(5);
        System.out.println("LinkedHashSet2: " + oddNumbers);

        // Difference between LinkedHashSet1 and LinkedHashSet2
        primeNumbers.removeAll(oddNumbers);
        System.out.println("Difference : " + primeNumbers);
    }
}
```

::result

LinkedHashSet1: [2, 3, 5]<br/>
LinkedHashSet2: [1, 3, 5]<br/>
Difference: [2]

::

## Kiểm tra có phải là tập con hay không

Để kiểm tra xem một set có phải là tập con của một set khác hay không, chúng ta có thể sử dụng hàm `containsAll()`

```java
import java.util.LinkedHashSet;

class Main {
    public static void main(String[] args) {
        LinkedHashSet<Integer> numbers = new LinkedHashSet<>();
        numbers.add(1);
        numbers.add(2);
        numbers.add(3);
        numbers.add(4);
        System.out.println("LinkedHashSet1: " + numbers);

        LinkedHashSet<Integer> primeNumbers = new LinkedHashSet<>();
        primeNumbers.add(2);
        primeNumbers.add(3);
        System.out.println("LinkedHashSet2: " + primeNumbers);

        // Check if primeNumbers is a subset of numbers
        boolean result = numbers.containsAll(primeNumbers);
        System.out.println("Is LinkedHashSet2 is subset of LinkedHashSet1? " + result);
    }
}
```

::result

LinkedHashSet1: [1, 2, 3, 4]<br/>
LinkedHashSet2: [2, 3]<br/>
Is LinkedHashSet2 is a subset of LinkedHashSet1? true

::

## Một số hàm khác của `LinkedHashSet`

| Hàm          | Mô tả                                                                      |
| ------------ | -------------------------------------------------------------------------- |
| `clone()`    | Tạo một bản sao của LinkedHashSet                                          |
| `contains()` | Tìm kiếm phần tử đã chỉ định trong LinkedHashSet và trả về kết quả boolean |
| `isEmpty()`  | Kiểm tra xem LinkedHashSet có trống không                                  |
| `size()`     | Trả về kích thước của LinkedHashSet                                        |
| `clear()`    | Loại bỏ tất cả các phần tử khỏi LinkedHashSet                              |

## So sánh `LinkedHashSet` và HashSet

Cả `LinkedHashSet` và `HashSet` đều triển khai `Set interface`. Tuy nhiên, có một số khác biệt giữa chúng.

- `LinkedHashSet` duy trì một `LinkedList` trong nội bộ Do đó, nó duy trì thứ tự chèn các phần tử của nó.
- Class `LinkedHashSet` đòi hỏi nhiều không gian lưu trữ hơn `HashSet`. Đó là do `LinkedHashSet` duy trì `LinkedList` trong nội bộ.
- Hiệu suất của `LinkedHashSet` chậm hơn `HashSet`. Bởi vì `LinkedList` có ở trong `LinkedHashSet`.

## So sánh LinkedHashSet và TreeSet

Dưới đây là những sự khác biệt chính giữa `LinkedHashSet` và `TreeSet`:

- Class `TreeSet` triển khai SortedSet interface. Đó là lý do tại sao các phần tử trong một `TreeSet` được sắp xếp theo thứ tự. Tuy nhiên, class - `LinkedHashSet` chỉ duy trì thứ tự chèn vào của các phần tử của nó.
- `TreeSet` thường hoạt động chậm hơn `LinkedHashSet`. Bởi vì khi một phần tử được thêm vào `TreeSet`, cần phải thực hiện thêm cả thao tác sắp xếp.
- `LinkedHashSet` cho phép ta chèn các giá trị `null`. Tuy nhiên, ta không thể chèn giá trị null vào `TreeSet`.
