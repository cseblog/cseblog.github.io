---
published: true
layout: post
author: Andrew HQT
categories: Engineering
tags: English
---
Stress Test Kafka Microservice
===

Trong một hệ thống gồm nhiều microservice được xây dựng xung quanh Kafka, chúng ta rất nhiều lần sẽ tự hỏi liệu các microservice hoạt động như thế nào, nếu xảy ra trường hợp tải dữ liệu đầu vào đột ngột tăng cao. Liệu các service có chết hay không? Vậy làm thế nào để đánh giá được mức độ chiụ tải hay đánh giá độ trễ (latency) của các microservice.

**Ví dụ:** Xem xét mô hình dịch vụ phía dưới
![Figure 1](https://i.imgur.com/XEzbAUW.png)

Để có thể stress test được dịch vụ Serivce A, Tôi phải xây dựng một dịch vụ test như sau:
![Simulator Service](https://i.imgur.com/HsgxMoQ.jpg)


### Simulator Service sẽ làm nhiệm vụ nạp rất nhiều dữ liệu vào các topic1, topic2... Sau đây là vài điểm cần lưu ý:
1. Dữ liệu đầu vào phải thật sự là dữ liệu gần giống với dữ liệu thật trên production sau khi đã loại bỏ các thông tin bảo mật cần thiết. 
2. Simulator Service phải có khả năng kiểm soát được tầng suất nạp dữ liệu vào các topic1, topic2...
3. Công việc thứ 3 của Simulator Service là cần phải lắng nghe được dữ liệu output của ServiceA để so sánh với kết quả mong đợi và tính toán hiệu suất. 
4. Simulator Service đọc dữ liệu từ csv files và chuẩn bị dữ liệu đầu vào trong bộ nhớ.

Dựa vào những yêu cầu trên, tôi bắt tay vào hiện thực Simulator Service bằng cách tận dụng thư viện Spring-cloud-kafka. Vì gói Spring-cloud-kafka cung cấp tất cả các hàm cơ bản để kết nối với Kafka cluster bao gồm producer và consumer API. 
//code

Bước tiếp theo là tạo bộ kiểm soát tầng số bơm dữ liệu như mô hình bên dưới. 
![](https://i.imgur.com/PzXAA9A.jpg)

**ThreadManager** Class có nhiệm vụ đọc dữ liệu test từ các file csv. Sau đó tạo các ThreadMonitor dựa vào số lượng topic đầu vào mà chúng ta muốn bơm dữ liệu. 
//Code

**ThreadMonitor** là Class nhận hai thông số là p, và f. Ở đây p là **period**, thời gian cần phải nạp vào cho topic thứ i, đơn vị là giây. f là tầng số bơm mà SenderThread cần phải đưa vào Kafka topic trong giây.

**Ví dụ:** (p, f) = (60,100) có nghĩa là cần bơm 100 message trên một giây vào topic liên tục trong vòng 60 giây. 
//Code

SenderThread là các thread giúp nạp dữ liệu vào Kafka một cách độc lập. Và lựu lại số lượng message đã bơm vào Kafka. 
/Code
