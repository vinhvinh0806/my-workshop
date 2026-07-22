---
title: "Worklog Tuần 5"
date: 2026-06-01
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---



### Mục tiêu tuần 5:

* Tự học và thực hành chuyên sâu về Amazon VPC (Virtual Private Cloud), quy hoạch mạng an toàn cho đề tài.
* Phân tích rõ các yêu cầu kỹ thuật, phạm vi ứng dụng và mục tiêu cần đạt được của đề tài thực tập.
* Thiết kế và xây dựng sơ đồ kiến trúc tổng quan (Architecture Diagram) ban đầu cho hệ thống.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Nghiên cứu các trụ cột vận hành xuất sắc (Operational Excellence) và tự học Amazon VPC <br>- Thiết lập tự động tắt máy chủ bằng AWS Lambda và tạo Dashboard giám sát với Amazon CloudWatch | 01/06/2026 | 02/06/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Phân tích phạm vi đề tài: Quản lý quyền EC2 qua Tag, tự động hóa tác vụ bằng AWS Systems Manager <br>- Tìm hiểu Session Manager và viết kịch bản hạ tầng dạng mã (IaC) với AWS CloudFormation | 03/06/2026 | 03/06/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - Xây dựng sơ đồ kiến trúc ban đầu của hệ thống (VPC, Subnets, Security Group, Transit Gateway) <br>- Thiết lập bảo mật ứng dụng với AWS WAF, IAM Condition và đánh giá qua AWS Security Hub | 04/06/2026 | 04/06/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - Thử nghiệm đóng gói ứng dụng đề tài bằng Docker và triển khai Container trên Amazon ECS <br>- Khởi tạo quy trình CI/CD tự động hóa việc triển khai ứng dụng qua AWS CodePipeline | 05/06/2026 | 05/06/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 6 | - Phân tích chi phí và tối ưu hóa tài nguyên mạng/máy chủ cho sơ đồ kiến trúc (Savings Plans, Athena) <br>- Tổng hợp sơ đồ kiến trúc ban đầu, hoàn thiện tài liệu đề xuất và nộp báo cáo tuần 5 | 06/06/2026 | 07/06/2026 | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được tuần 5:

* **Về phân tích & Thiết kế hệ thống:**
  * Xác định rõ phạm vi, mục tiêu và danh sách các dịch vụ AWS cốt lõi cần triển khai cho đề tài.
  * Hoàn thành bản vẽ sơ đồ kiến trúc hệ thống ban đầu (Initial System Architecture Diagram) đáp ứng tiêu chí cao về tính sẵn sàng và bảo mật.

* **Về kỹ năng hạ tầng & Vận hành:**
  * Nắm vững cách thiết lập Amazon VPC (Public/Private Subnet, Route Table, NAT Gateway, Security Group).
  * Thực hành tự động hóa vận hành bằng AWS Lambda, Systems Manager Session Manager và Infrastructure as Code (CloudFormation).
  * Triển khai giải pháp Containerization cho ứng dụng với Docker và Amazon ECS kết hợp luồng tự động CI/CD (AWS CodePipeline).
  * Làm chủ các công cụ kiểm soát bảo mật (AWS WAF, Security Hub) và phân tích chi phí (AWS Glue, Amazon Athena).