---
published: true
category: engineering
layout: post
author: Andrew HQT
categories: Engineering
tags: English
---
Stress test Kafka Microservice
===

Trong một hệ thống gồm nhiều microservice được xây dựng xung quanh Kafka, chúng ta sẽ rất nhiều lần tự hỏi liệu các microservice của ta sẽ hoạt động như thế nào nếu một lượng lớn dữ liệu đầu vào. Liệu service có chết hay không? Vậy làm thế nào để đánh giá được mức độ chiụ tải hay đánh giá latency of processing của dịch vụ?
Ví dụ: xem xét mô hình dịch vụ phía dưới

![Figure 1](https://i.imgur.com/XEzbAUW.png)

Để có thể stress test được dịch vụ Serivce A, Chúng tôi phải xây dựng một dịch vụ test như sau:
![](https://i.imgur.com/HsgxMoQ.jpg)


SimulatorTestService sẽ làm nhiệm vụ bơm rất nhiều dữ liệu đầu vào topic1, topic2. Vài thứ cần lưu ý:
1. Dữ liệu đầu vào phải thật sự là dữ liệu gần giống với dữ liệu thật trên production sau khi đã loại bỏ các thông tin bảo mật cần thiết. 
2. Simulator Service phải có khả năng kiểm soát được tần suất bơm dữ liệu vào các topic1, topic2
3. Công việc thứ 3 của Simulator Service là cần phải lắng nghe được dữ liệu output của ServiceA để so sánh với kết quả mong đợi và tính toán hiệu suất. 
4. Simulator Service đọc dữ liệu từ csv files và chuẩn bị dữ liệu đầu vào trong bộ nhớ.

Dựa vào những phân tíc trên, tôi bắt tay vào hiện thực Simulator Service bằng cách tận dụng Spring-cloud-kafka. Vì gói Spring-cloud-kafka cung cấp tất cả các hàm cơ bản để kết nối với Kafka cluster bao gồm producer và consumer API. 
//code

Bước tiếp theo là tạo bộ kiểm soát tầng số bơm dữ liệu như mô hình bên dưới. 
![](https://i.imgur.com/PzXAA9A.jpg)

**ThreadManager** class có nhiệm vụ đọc dữ liệu test từ các file csv. Sau đó tạo các ThreadMonitor dựa vào số lượng topic mà chúng ta muốn bơm dữ liệu. 
//Code

**ThreadMonitor **là class nắm hai thông số là p, và f. P là period, thời gian cần phải bơm vào co topic thứ i đơn vị là giây. f là tầng số bơm mà SenderThread cần phải đưa vào Kafka topic trên giây. 
Ví du: (p, f) = (60,100) có nghĩa là cần bơm 100 message trên một giây vào topic liên tục trong vòng 60 giây. 
//Code

SenderThread là các thread giúp bơm dữ liệu vào Kafka độc lập. Và lựu lại số lượng message đã bơm vào Kafka. 
/Code
