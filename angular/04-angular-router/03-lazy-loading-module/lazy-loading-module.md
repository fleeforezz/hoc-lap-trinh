---
title: "Lazy Loading Modules"
description: "Tiếp tục với Angular Router, các bạn đã biết cách tách phần routing ra thành feature tương ứng. Từ đó code của chúng ta đã hoạt động khá riêng biệt, nếu bạn cần reuse module nào thì có thể copy nguyên phần code đó sang app Angular khác và import vào `AppModule` là được. Vẫn tiếp tục là ứng dụng hiển thị danh sách các bài viết hôm trước. Nhưng bây giờ mình sẽ có thêm một phần nữa để quản lý bài viết nằm ở đường dẫn `/admin`. Phần code này sẽ được đặt trong `AdminModule`."
chapter:
  name: "Angular Router"
  slug: "chuong-04-angular-router"
category:
  name: "Angular"
  slug: "angular"
image: https://kungfutech.edu.vn/thumbnail.png
position: 3
---

Cùng ôn lại bài cũ tý nhé, ở bài trước chúng ta đã biết cách config một feature module có tên là `ArticleModule`

```ts
@NgModule({
  imports: [CommonModule, ArticleRoutingModule],
  declarations: [
    ArticleComponent,
    ArticleListComponent,
    ArticleDetailComponent,
  ],
})
export class ArticleModule {}
```

```ts
const routes: Routes = [
  {
    path: "article",
    component: ArticleComponent,
    children: [
      {
        path: "",
        component: ArticleListComponent,
      },
      {
        path: ":slug",
        component: ArticleDetailComponent,
      },
    ],
  },
];

@NgModule({
  imports: [CommonModule, RouterModule.forChild(routes)],
  declarations: [],
  exports: [RouterModule],
})
export class ArticleRoutingModule {}
```

Và cuối cùng là import module này vô `AppModule`

```ts
@NgModule({
  imports: [BrowserModule, FormsModule, ArticleModule, AppRoutingModule],
  declarations: [AppComponent],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

Bây giờ mình sẽ run `npm run build` để build ra file JS. Mình dùng tool [source-map-explorer][sourcemap] để phân tích bundle JS đã build. Có khá nhiều tool tương ứng, trong đó nổi nhất gần đây là [webpack-bundle-analyzer][webpackbundle] trong màu mè hơn. Nhưng dù sao mình cũng dùng quen `source-map-explorer` rồi. Nếu bạn chưa cài cắm thì có thể chạy câu lệnh đơn giản là `npm i -g source-map-explorer`.

Sau khi `npm run build` chạy xong, mình sẽ mở folder dist và check bundle `main.js` bằng câu lệnh `source-map-explorer main.js`. Đây là bundle JS chính sẽ load khi ta load page.

![Step 1][step01]

Sau khi câu lệnh trên được chạy, các bạn sẽ nhìn thấy chi tiết bundle main.js bao gồm những gì. Đại ý là khi load `main.js`, cả AppModule và ArticleModule sẽ đều được load. Điều này trong kì vọng của chúng ta.

![Step 2][step02]

## Thêm một feature module nữa.

Bây giờ mình sẽ thêm AdminModule vào, đại ý là mình sẽ hiển thị danh sách article ở dạng bảng và có thể edit/sửa/xóa article. Vì làm ví dụ nên mình chỉ để button thôi, ko implement mấy cái chức năng này.

Phần UI đại ý sau khi code sẽ nhìn như sau.

![Step 3][step03]

Giờ chúng ta bắt tay vào code nhé. Mình cũng sẽ tạo ra một `AdminModule` có chứa hai component:

- AdminComponent: phần layout, như ArticleComponent
- AdminArticleListComponent: phần component hiển thị article ở dạng bảng để thực hiện các tác vụ.

Đây là `AdminRoutingModule`

```ts
const routes: Routes = [
  {
    path: "admin",
    component: AdminComponent,
    children: [
      {
        path: "",
        component: AdminArticleListComponent,
      },
    ],
  },
];

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule],
})
export class AdminRoutingModule {}
```

Tuy nhiên bởi vì mình sẽ cần render table, và phải edit Article nữa nên mình đã import thêm `ReactiveFormsModule` trong `AdminModule`

```ts
@NgModule({
  declarations: [AdminComponent, AdminArticleListComponent],
  imports: [
    CommonModule,
    ReactiveFormsModule, // <-- Import thêm để dùng forms sau này
    AdminRoutingModule,
  ],
})
export class AdminModule {}
```

Xong đâu đấy thì mình import `AdminRoutingModule` vào `AdminModule`. Và `AdminModule` vào `AppModule`

```ts
@NgModule({
  imports: [BrowserModule, AdminModule, ArticleModule, AppRoutingModule],
  declarations: [AppComponent],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

Giờ khi run `npm start`, anh em sẽ thấy UI trong như cái ảnh động ở trên. Mình ko show lại ở dưới này nữa nhé. Phần hay ho giờ lại liên quan đến JS Bundle. Mình tiếp tục dùng `source-map-explorer` để check hàng `main.js` thì thấy giờ đã có thêm `AdminModule` trong bundle. Thế còn đống `Angular Core` rồi `ReactiveFormsModule` thì nằm ở đâu nhỉ? Câu trả lời là nó nằm ở trong một file có tên là `vendor.js`. Cùng check hàng file này nhé, chạy câu lệnh `source-map-explorer vendor.js` và chúng ta có kết quả như sau.

![Step 4][step04]

Forms nằm ở đây chứ đâu, phần bôi đỏ đó.

![Step 5][step05]

Kết luận lại là nếu dùng Feature Module thì khi load ứng dụng, toàn bộ thư viện, component bạn dùng ở trong TẤT CẢ các feature module sẽ đều được Angular load. App nhỏ thì ko nói, nhưng app lớn chắc chắn sẽ rất ảnh hưởng đến trải nghiệm của người dùng.

Giờ mình mới đến phần quan trọng của bài viết hôm nay - Lazy Load Module. Khi đã áp dụng được kĩ thuật này, thì riêng phần `AdminModule` và `ReactiveFormsModule` đi kèm chỉ được load khi bạn mở url `/admin` thôi.

## Lazy load module

Để hỗ trợ lazy load module trong Angular, đầu tiên mình cần làm một số việc.

1. Xóa `AdminModule` trong mảng import của `AppModule` đi. Vì nếu mình import vô đây thì khi bundle toàn bộ phần code của `AdminModule` sẽ được load cùng `AppModule` như mình đã thấy ở trên
   Note: nhớ xóa cả phần `import` ở trên đi nhá, không thì sẽ ko giải quyết vấn đề gì đâu :))

![Step 6][step06]

2. Config lại route cho `AdminModule`. Mình cần bỏ phần path `admin` ở trong `AdminRoutingModule` đi. Lý do thì các bạn xem step 3 sẽ rõ.

![Step 7][step07]

3. Bước quan trọng nhất, trong `AppRoutingModule`, chúng ta cần config để load admin module theo cú pháp của lazy loading module khi mở ứng dụng ở path có dạng `/admin`

![Step 8][step08]

Vẫn là định nghĩa là path `admin`, nhưng thay vì component, mình dùng `loadChildren` với cú pháp `() => import('./admin/admin.module').then((m) => m.AdminModule)`, tức là một function có return lại một dynamic import module.

> Notice that the lazy-loading syntax uses loadChildren followed by a function that uses the browser's built-in `import('...')` syntax for dynamic imports. The import path is the relative path to the module.

Vậy là xong rồi đấy. Giờ mình mở sẽ run `npm run build` một lần nữa. Giờ CLI đã tạo ra một file JS mới dành riêng cho `AdminModule` tên `admin-admin-module.js`.

![Step 9][step09]

Mình sẽ mở app lên xem thử, các bạn sẽ thấy một sự khác bọt khá lớn.

![Step 10][step10]

Khi mình mới load page, thì app có load các file cần thiết và `main.js`. Tuy nhiên khi mình ấn vào Admin để đi đến `/admin`, lúc đó `admin-admin-module.js` mới được load. Lazy load chính là như thế, tức là không load ngay từ đầu, mà chỉ load khi cần đến.

Giờ chúng ta cùng xem bundle của `main.js` và `admin.module.js` nhé.

```
source-map-explorer main.js
source-map-explorer admin-admin-module.js
```

![Step 11][step11]

Phần bundle của `main.js` giờ đã không còn AdminModule nữa rồi, chỉ còn ArticleModule thôi.

![Step 12][step12]

Còn đây là phân tích của `admin-admin-module.js`. Phần import của Form và các component của AdminModule đã nằm trong bundle này. Quá xuất sắc.

## Lazy load syntax

`import('...')` syntax được khuyến cáo sử dụng từ Angular version 8.

Ngoài cách dùng `import('...')` syntax, chúng ta có một cách dùng từ Angular version 7 trở xuống như sau: `loadChildren: './admin/admin.module#AdminModule'`. Đó là một magic string, để chỉ ra file path đến file mà chứa NgModule có kèm Router cần load.

### Preloading Lazy Module

Việc tách ra thành các lazy loading module rất có lợi cho lần tải trang của user. Với việc browser phải download file JS nhỏ hơn, nên thời gian chờ đợi để tương tác với website sẽ giảm. Tuy nhiên nhiều khi các lazy loading module có thể có size rất lớn. Dẫn đến việc khi user bấm vào một link, sẽ mất thời gian để tải. Ví dụ vẫn là lazy loading `AdminModule` ở trên nhưng giờ mình giả lập là mạng điện thoại, xem tình hình thế nào nhé.

![Step 13][step13]

Anh em thấy đấy, thời gian từ khi bấm vào link Admin cho đến khi UI hiện ra có khi cả 10s. Quá chậm.

Có một số module mà mình biết rằng thường là khi user mở ứng dụng của mình ra, họ sẽ click vào đó đầu tiên. Chúng ta có thể dùng kĩ thuật Preloading để load những bundle ở background.

Để enable preloading cho tất cả các lazy loaded modules, các bạn cần import `PreloadAllModules` từ package `@angular/router` và cấu hình nó ở trong AppRoutingModule, đoạn forRoot.

```ts
import { PreloadAllModules } from "@angular/router";

const routes: Routes = [
  {
    path: "admin",
    loadChildren: () =>
      import("./admin/admin.module").then((m) => m.AdminModule),
  },
  {
    path: "",
    redirectTo: "article",
    pathMatch: "full",
  },
];

@NgModule({
  imports: [
    RouterModule.forRoot(routes, {
      preloadingStrategy: PreloadAllModules,
    }),
  ],
  exports: [RouterModule],
})
export class AppRoutingModule {}
```

Bây giờ chạy lại app nào.

![Step 14][step14]

Great. Đã hoạt động như kì vọng. Ngay sau khi trang chủ được load, thì app đã tự động download `admin-admin-module.js` về. Do mình ko dùng `ng build` với flag `--prod=true` nên dung lượng file hơi lớn. Chứ khi build có flag prod thì file size sẽ nhẹ hơn rất nhiều nhé.

Để có thể preload các module theo ý muốn, thay vì toàn bộ các lazy loading module, các bạn tham khảo thêm ở [đây][preloading]. Do khuôn khổ bài viết có hạn nên mình ko đưa vào. Ai có nhu cầu thì tìm đọc thêm nhé.

## Tổng kết

Hy vọng các bạn đã thấy được lợi ích của Lazy Loading và biết cách config một Lazy loading module cho Router trong Angular qua bài viết này. Để luyện tập thì mình nghĩ anh em nên thử convert cái ArticleModule qua lazy loading module xem sao.

## Code example

https://stackblitz.com/edit/angular-100-days-of-code-day-29-router-lazy
