---
title: "Vòng lặp th:each trong Thymeleaf"
description: "Thymeleaf cung cấp cho bạn vòng lặp each, và bạn có thể sử dụng nó thông qua thuộc tính (attribute) th:each. Đây là vòng lặp duy nhất được hỗ trợ trong Thymeleaf.."
chapter:
  name: "Thymeleaf"
  slug: "chuong-04-thymeleaf"
category:
  name: "Spring Boot"
  slug: "spring-boot"
image: https://kungfutech.edu.vn/thumbnail.png
position: 6
---

## Vòng lặp th:each

**Thymeleaf** cung cấp cho bạn vòng lặp **each**, và bạn có thể sử dụng nó thông qua thuộc tính (attribute) **th:each**. Đây là vòng lặp duy nhất được hỗ trợ trong Thymeleaf.
Vòng lặp này chấp nhận một vài loại dữ liệu như:

- Các đối tượng thực hiện (implements) interface java.util.Iterable.
- Các đối tượng thực hiện (implements) interface java.util.Map.
- Các mảng (Arrays)

```html
<someHtmlTag th:each="item : ${items}"> .... </someHtmlTag>
```

Thẻ **<th:block>** là một thẻ ảo trong Thymeleaf, nó không tương ứng với bất kỳ thẻ nào của HTML, nhưng nó rất có ích trong nhiều trường hợp, chẳng hạn bạn có thể đặt thuộc tính (attribute) **th:each** trong thẻ này.

```html
<th:block th:each="item : ${items}"> .... </th:block>
```

Ví dụ đơn giản với vòng lặp th:each
<content-example>
CONTROLLER
</content-example>

```java
@RequestMapping("/loop-simple-example") public String loopExample1(Model model) {
  String[] flowers = new String[] { "Rose", "Lily", "Tulip", "Carnation", "Hyacinth" };
  model.addAttribute("flowers", flowers);
  return "loop-simple-example";
}
```

VIEW

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8" />
    <title>Loop</title>
  </head>
  <body>
    <h1>th:each</h1>
    <ul>
      <th:block th:each="flower : ${flowers}">
        <li th:utext="${flower}">..</li>
      </th:block>
    </ul>
  </body>
</html>
```

OUTPUT

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Loop</title>
  </head>
  <body>
    <h1>th:each</h1>
    <ul>
      <li>Rose</li>
      <li>Lily</li>
      <li>Tulip</li>
      <li>Carnation</li>
      <li>Hyacinth</li>
    </ul>
  </body>
</html>
```

Cú pháp đầy đủ của th:each bao gồm 2 biến, biến phần tử (item variable) và biến trạng thái (state variable).

```html
<someHtmlTag th:each="item, iState : ${items}"> ..... </someHtmlTag>
```

<!-- OR: -->

```html
<th:block th:each="item, iState : ${items}"> ..... </th:block>
```

Biến trạng thái (State variable) là một đối tượng hữu ích, nó chứa các thông tin cho bạn biết trạng thái hiện tại của vòng lặp, chẳng hạn như số phần tử của vòng lặp, chỉ số hiện tại của vòng lặp,...
Dưới đây là danh sách các thuộc tính (property) của biến trạng thái (state variable):

![property](https://scontent.fhan2-3.fna.fbcdn.net/v/t1.15752-9/279280891_355385463240783_5244729283221754305_n.png?_nc_cat=108&ccb=1-6&_nc_sid=ae9488&_nc_ohc=L6CtSZfuW0MAX-mFMKA&_nc_ht=scontent.fhan2-3.fna&oh=03_AVKhm-EcGf3r95ekALm8UoPXpGvuy7HnQZ0lm5cIdkA3aA&oe=62A1DA5B)
