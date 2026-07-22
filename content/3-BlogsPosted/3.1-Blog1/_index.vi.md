---
title: "Tối ưu chi phí khi triển khai Eclipse Dataspace Components trên AWS"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

Trong quá trình tìm hiểu các bài viết trên **AWS Architecture Blog**, mình đã đọc bài viết **"Eclipse Dataspace Components on AWS: Cost Optimization Strategies"**. Ban đầu mình nghĩ đây chỉ là một bài hướng dẫn cách giảm chi phí khi triển khai **Eclipse Dataspace Components (EDC)** trên AWS. Tuy nhiên, sau khi đọc, mình nhận ra điều AWS muốn nhấn mạnh không phải là cắt giảm chi phí, mà là thiết kế kiến trúc phù hợp với từng workload để tối ưu chi phí ngay từ đầu.

Điều mình thấy thú vị là bài viết không chỉ đưa ra các con số về chi phí mà còn giải thích vì sao từng thành phần trong kiến trúc lại ảnh hưởng đến tổng chi phí của hệ thống. Qua đó, AWS đưa ra nhiều gợi ý giúp lựa chọn cấu hình phù hợp với từng workload thay vì sử dụng cùng một kiến trúc cho mọi môi trường.

---

## Tổng quan bài viết

Đây là bài cuối cùng trong chuỗi ba bài viết về **Eclipse Dataspace Components (EDC)** trên AWS. Hai bài trước tập trung vào kiến trúc triển khai, bảo mật và độ tin cậy của hệ thống, còn bài viết này hướng đến ba trụ cột còn lại của **AWS Well-Architected Framework**, gồm:

- Performance Efficiency
- Cost Optimization
- Sustainability

Thay vì chỉ trình bày chi phí của từng dịch vụ AWS, bài viết tập trung phân tích những thành phần thực sự tạo ra chi phí trong hệ thống và so sánh hai mô hình triển khai dành cho **Business-Critical Workloads** và **Non-Critical Workloads**.

---

## Kiến trúc triển khai EDC trên AWS

Bài viết sử dụng một kiến trúc tham chiếu để triển khai **Eclipse Dataspace Components (EDC)** trên AWS.

Trong kiến trúc này:

- Amazon API Gateway tiếp nhận các yêu cầu từ bên ngoài.
- Network Load Balancer phân phối lưu lượng truy cập đến các dịch vụ EDC.
- Amazon ECS triển khai hai thành phần chính là **EDC Control Plane** và **EDC Data Plane**.
- Amazon Aurora PostgreSQL lưu trữ dữ liệu của hệ thống.
- AWS Secrets Manager quản lý các thông tin nhạy cảm.
- Amazon Cognito hỗ trợ xác thực thông qua OAuth 2.0.
- Amazon S3 được sử dụng để lưu trữ dữ liệu và tích hợp với các nguồn dữ liệu bên ngoài.

Kiến trúc này được thiết kế theo hướng tách biệt từng thành phần, giúp hệ thống dễ mở rộng, thuận tiện cho việc quản lý cũng như tối ưu hiệu năng và chi phí vận hành.

**Hình 1. Kiến trúc triển khai Eclipse Dataspace Components trên AWS**

![Kiến trúc triển khai Eclipse Dataspace Components trên AWS](/images/edc-architecture.png)

*Hình 1. Kiến trúc tham chiếu triển khai Eclipse Dataspace Components (EDC) trên AWS.*

Quan sát sơ đồ trên, mình thấy Amazon API Gateway đóng vai trò tiếp nhận các yêu cầu từ bên ngoài trước khi chuyển đến Network Load Balancer. Các dịch vụ EDC được triển khai trên Amazon ECS và sử dụng Amazon Aurora PostgreSQL để lưu trữ dữ liệu. Bên cạnh đó, Amazon Cognito hỗ trợ xác thực người dùng, AWS Secrets Manager quản lý thông tin bảo mật và Amazon S3 hỗ trợ lưu trữ cũng như trao đổi dữ liệu với các hệ thống bên ngoài.

---

## Phân loại Workload

Một điểm mình thấy khá hay là AWS chia workload thành hai nhóm với mục tiêu sử dụng khác nhau.

### Business-Critical Workloads

Đây là các hệ thống yêu cầu:

- High Availability
- Reliability
- Hiệu năng cao
- Hoạt động liên tục

Các workload này thường phục vụ các chức năng quan trọng của doanh nghiệp nên được triển khai với cấu hình mạnh nhằm đảm bảo tính ổn định và khả năng xử lý.

### Non-Critical Workloads

Đây là các môi trường như:

- Development
- Testing
- Proof of Concept
- Hoặc các workload có thể chấp nhận gián đoạn trong thời gian ngắn.

Qua đó mình nhận ra rằng không phải môi trường nào cũng cần triển khai cùng một cấu hình như Production. Nếu sử dụng hạ tầng mạnh cho cả môi trường Development hoặc Testing thì sẽ làm tăng chi phí mà không mang lại nhiều giá trị.

---

## Những thành phần tạo ra chi phí nhiều nhất

Trước khi đọc bài viết, mình nghĩ Amazon API Gateway hoặc Amazon S3 sẽ là những dịch vụ chiếm phần lớn chi phí.

Tuy nhiên, AWS chỉ ra rằng phần lớn ngân sách lại đến từ:

- Amazon Aurora PostgreSQL
- Amazon ECS chạy trên AWS Fargate
- Network Load Balancer

Trong khi đó, các dịch vụ như Amazon API Gateway, Amazon S3 và Amazon ECR chỉ chiếm một tỷ lệ nhỏ trong tổng chi phí.

Điều này cho thấy phần lớn chi phí đến từ Database, Compute và Load Balancer thay vì các dịch vụ sử dụng theo lưu lượng. Nhờ đó mình hiểu rằng muốn tối ưu chi phí thì trước tiên cần xác định đúng **Cost Drivers**, từ đó mới lựa chọn phương án tối ưu phù hợp.

---

## So sánh chi phí giữa hai mô hình

Để minh họa, AWS đưa ra hai kịch bản triển khai.

### Business-Critical

Đối với các hệ thống yêu cầu tính sẵn sàng cao, AWS sử dụng:

- Amazon Aurora PostgreSQL db.r6g.large
- Amazon ECS với AWS Fargate
- Network Load Balancer

Chi phí ước tính khoảng **387 USD/tháng**.

### Non-Critical

Đối với môi trường Development hoặc Testing, AWS đề xuất:

- Aurora db.t4g.medium
- AWS Fargate Spot

Chi phí giảm xuống còn khoảng **164 USD/tháng**, tương đương tiết kiệm khoảng **58%** so với mô hình Business-Critical.

Qua ví dụ này, mình hiểu rằng tối ưu chi phí không có nghĩa là cắt giảm dịch vụ. Quan trọng hơn là lựa chọn cấu hình phù hợp với mức độ quan trọng của workload để vừa đáp ứng yêu cầu vận hành vừa tránh lãng phí tài nguyên.

---

## Một số chiến lược tối ưu chi phí

Ngoài việc lựa chọn đúng cấu hình, AWS còn đưa ra nhiều khuyến nghị nhằm giảm chi phí vận hành như:

- Sử dụng **AWS Fargate Spot** đối với các workload có thể chấp nhận gián đoạn.
- Thiết lập **Amazon S3 Lifecycle Policies** để tự động chuyển dữ liệu sang các lớp lưu trữ có chi phí thấp hơn.
- Theo dõi chi phí bằng **AWS Cost Explorer** và **AWS Budgets**.
- Cân nhắc sử dụng **Savings Plans** đối với các workload hoạt động ổn định trong thời gian dài.

Theo mình, đây đều là những thực hành khá hữu ích và hoàn toàn có thể áp dụng cho nhiều hệ thống chạy trên AWS, kể cả những dự án không sử dụng Eclipse Dataspace Components.

---

## Bài học rút ra

Sau khi đọc bài viết, mình rút ra một số điều đáng chú ý:

- Không phải môi trường nào cũng cần cấu hình giống Production.
- Database, Compute và Network Load Balancer thường là ba thành phần chiếm phần lớn chi phí khi triển khai hệ thống.
- Cần xác định đúng mức độ quan trọng của workload trước khi lựa chọn tài nguyên.
- Theo dõi chi phí thường xuyên sẽ giúp phát hiện sớm các khoản chi bất thường.
- Thiết kế kiến trúc phù hợp ngay từ đầu sẽ giúp tiết kiệm chi phí trong suốt quá trình vận hành.

---

## Kết luận

Sau khi đọc bài viết, mình hiểu rằng tối ưu chi phí không chỉ là giảm tài nguyên mà còn là quá trình thiết kế kiến trúc phù hợp với từng workload ngay từ đầu. Khi lựa chọn đúng cấu hình và thường xuyên theo dõi chi phí, doanh nghiệp có thể vừa đảm bảo hiệu năng, vừa sử dụng tài nguyên hiệu quả và giảm đáng kể chi phí vận hành.

Theo mình, đây là một bài viết rất hữu ích đối với những bạn đang tìm hiểu về **AWS Architecture**, **AWS Well-Architected Framework** hoặc **FinOps**, bởi nó giúp mình có cái nhìn thực tế hơn về cách thiết kế hệ thống vừa đáp ứng yêu cầu vận hành, vừa tối ưu chi phí trên AWS.

---

## Tài liệu tham khảo

- AWS Architecture Blog: *Eclipse Dataspace Components on AWS: Cost Optimization Strategies*
- https://aws.amazon.com/blogs/architecture/eclipse-dataspace-components-on-aws-cost-optimization-strategies/