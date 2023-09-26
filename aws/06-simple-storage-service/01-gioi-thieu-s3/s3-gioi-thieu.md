---
title: "Giới thiệu về S3"
description: "Amazon S3 (Amazon Simple Storage Service) là dịch vụ lưu trữ dữ liệu đơn giản của Amazon cung cấp."
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
image: https://user-images.githubusercontent.com/29729545/147766077-fce2bc9e-0852-4d72-b89e-b30b67d78eb0.png
position: 50
---

## AWS S3 là gì

Amazon S3 (Amazon Simple Storage Service) là dịch vụ lưu trữ dữ liệu đơn giản của Amazon cung cấp. Amazon S3 cung cấp khả nẳng mở rộng, tính khả dụng của dữ liệu, bảo mật cao.

## S3 Buckets và Object trong AWS

Amazon S3 cho phép người dùng có thể lưu trữ Objects (files) trong Buckets (directories).

### S3 Buckets là gì
- Bucket có scope là Region
- Buckets trong aws cần có tên là duy nhất trên toàn cầu
- Khi tạo buckets cần chọn region
- Quy tắc đặt tên:
  - Chỉ chưa ký tự viết thường, số, dấu chấm (.), gạch ngang (-)
  - Độ dài từ 3-63 ký tự
  - Không được có format của IP
  - Phải bắt đầu bằng ký tự thường hoặc số

### S3 Objects là gì

Object giống như một file dữ liệu của chúng ta. Object có **Key** chính là path tên object trong bucket.

- Key có thể là kết hợp của: **prefix + object_name**

![Key trong object](https://user-images.githubusercontent.com/29729545/147766077-fce2bc9e-0852-4d72-b89e-b30b67d78eb0.png)

(prefix: _path1/path2_, object*name: \_file-name.txt*)

- Object value là nội dung của body
  - Object size tối đa là: 5TB (5000GB)
  - Nếu muốn upload nhiều hơn 5GB, cần dùng **"multi-path upload"** để chia nhỏ upload nhiều phần.

## S3 versioning là gì

- Chúng ta có thể tạo các version của file
- Tính năng này được enable ở **"bucket level"**
- Khi 1 file có chung key, version sẽ tự động tạo ra
- Khi đánh version cho 1 file, chúng ta có thể dễ dàng phục hồi các version của 1 file

::alert{type="infor"}
Nếu enable versioning của một bucket thì những file đã tồn tại trước đó sẽ có <b>version ID = null</b>
::
