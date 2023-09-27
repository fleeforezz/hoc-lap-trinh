---
title: "DynamoDB nâng cao"
description: "Amazon DynamoDB Accelerator (DAX) là bộ nhớ đệm trong bộ nhớ, có khả năng sử dụng cao, được quản lý toàn phần dành cho Amazon DynamoDB. DAX cải thiện hiệu năng lên tới 10 lần—từ mili giây xuống micro giây—ngay cả khi có hàng triệu yêu cầu mỗi giây."
chapter:
  name: "Serverless"
  slug: "chap-11-serverless"
image: https://user-images.githubusercontent.com/29729545/155871367-1109d9c7-0746-4a63-bce0-72b56d8aafe8.png
position: 194
---

## DynamoDB Accelerator (DAX) là gì

Amazon DynamoDB Accelerator (DAX) là bộ nhớ đệm trong bộ nhớ, có khả năng sử dụng cao, được quản lý toàn phần dành cho Amazon DynamoDB. DAX cải thiện hiệu năng lên tới 10 lần từ mili giây xuống micro giây ngay cả khi có hàng triệu yêu cầu mỗi giây.

- Cache những dữ liệu thưởng xuyên được đọc
- TTL là 5 phút

![DynamoDB Accelerator (DAX)](https://user-images.githubusercontent.com/29729545/155871367-1109d9c7-0746-4a63-bce0-72b56d8aafe8.png)

## DynamoDB Streams là gì

- DynamoDB Streams là một tính năng trong DynamoDB cho phép bạn lắng nghe thay đổi trên một bảng dữ liệu nào đó (create/update/delete) và thực hiện các tác vụ đáp ứng yêu cầu nghiệp vụ trong ứng dụng của bạn.
- Stream records có thể:
  - Gửi tới **Kinesis Data Streams**
  - Đọc bởi **AWS Lambda**
  - Đọc bởi **Kinesis Client Library applications**
- Đữ liệu có thể lưu lại tối đa là 24h
- Use cases:
  - Phản ứng với thay đổi dữ liệu real-time
  - Analytics
  - Insert to ElasticSearch

![DynamoDB Streams](https://d2908q01vomqb2.cloudfront.net/887309d048beef83ad3eabf2a79a64a389ab1c9f/2021/05/06/DDB-Design-patterns-v1.3.jpg)

## DynamoDB Global Tables là gì

- DynamoDB Global Tables giúp những Table có thể truy cập vơi độ trễ thấp ở multiple-regions
- Application có thể READ/WRITE ở bất kỳ region nào
- **Để sử dụng Global Tables cần enable DynamoDB Streams**

![DynamoDB Global Tables](https://d1.awsstatic.com/product-marketing/DynamoDB/DynamoDB_Global-Tables-01.dad2508b80e8b7c544fe1a94a2abd3f770b789da.png)

## DynamoDB - Time To Live (TTL) là gì

- Bạn muốn xóa những item sau một khoảng thời gian nhất định (1 tuần, 1 tháng...)
- Use case: Giảm bớt lượng dữ liệu không cần thiết trên DynamoDB

```bash
$ aws dynamodb update-time-to-live \
               --table-name tableName \
               --time-to-live-specification \
               "Enabled=true,AttributeName=expireAtr"
```

## DynamoDB Indexes là gì

- DynamoDB cho phép bạn tạo secondary indexes trên một table
- Đây là cách query dữ liệu thay thế cho primary key
- Có 2 loại indexes:
  - **Global Secondary Index (GSI)**: Sử dụng cặp _partition key_ và _sort key_ (khác với _partition key_ và _sort key_ của table)
  - **Local Secondary Index (LSI)**: Sử dụng _partition key_ của table và một _sort key_ mới

## DynamoDB Transactions là gì

![DynamoDB Transactions](https://user-images.githubusercontent.com/29729545/155872716-c5d5eb84-fee6-450f-b0d1-d6dda472d773.png)
