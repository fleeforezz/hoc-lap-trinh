---
title: "Bảo mật trong S3"
description: "S3 security là quản lý quyền truy cập dữ liệu trong Amazon S3. S3 là dịch vụ lưu trữ đối tượng duy nhất cho phép bạn chặn quyền truy cập công cộng vào tất cả đối tượng trong bộ chứa hoặc ở cấp tài khoản bằng tính năng Chặn quyền truy cập công cộng S3."
author:
  fullname: Phan Văn Đức
  username: ducpv
  avatar: "/configs/author/ducpv.jpg"
category:
  name: "Khóa học AWS từ cơ bản đến nâng cao"
  slug: "aws"
chapter:
  name: "Simple Storage Serivce"
  slug: "chap-06-s3"
image: https://user-images.githubusercontent.com/29729545/147956862-919f469d-1203-4478-a698-ae6bd5790ebb.png
position: 53
---

## S3 Security trong AWS

S3 security là quản lý quyền truy cập dữ liệu trong Amazon S3. Chúng ta có 3 phương thức để quản lý truy cập, đó là:

- **IAM policies**: Cấp quyền truy cập cho user nhất định
- **ACLs**:
  - Bucket ALC: Quản lý cấp độ bucket
  - Objects ALC: Quản lý cấp độ Objects
- **Bucket policies**: Có thể add/deny quyền truy cập một cách linh hoạt được đinh nghĩa trong file JSON.

## S3 Bucket Policies trong AWS

![S3 Bucket Policies](https://user-images.githubusercontent.com/29729545/147956862-919f469d-1203-4478-a698-ae6bd5790ebb.png)

- S3 bucket policies được định nghĩa dưới dạng file JSON
- **Resource**: Tài nguyên thực thi (Bucket hoặc Objects)
- **Effect**: Allow hoặc Deny
- **Principal**: Account hay user được apply (\* là tất cả user)
- **Action**: Quền thực thi (GetObject, Put, Delete...)

Ví dụ như trên hình vẽ đang định nghĩa policy: Tất cả user có thể đọc được tất cả object trong bucket foobucket.

**Sử dụng S3 bucket policies thường cho:**

- Cấp quyền truy cập đến bucket, object
- Bắt buộc object cần được mã hóa trước khi upload lên S3
- Cấp quyền truy cập cho account khác (cross account)

## Block public access

Mặc định khi tạo một bucket trên S3, Amazon sẽ block tất cả public access. Có nghĩa là khi bạn upload một file ảnh lên bucket đó, bạn sẽ không thể view file đó được luôn. Để có thể xem được file ảnh đó, bạn cần cấp quyền để có thể xem từ mọi nơi.

## Bảo mật người dùng trong S3

- **MFA Delete**: Chúng ta có thể anable MFA (multi factor authentication) khi muốn delete object. Việc này đảm bảo không phải ai cũng có thể xóa dữ liệu của bạn
- **Pre-Signed URLs**: Ví dụ video của bạn là premium (giới hạn cho những user đã trả phí). Khi tạo Pre-Signed URLs cho object đó, urls sẽ có expire.
