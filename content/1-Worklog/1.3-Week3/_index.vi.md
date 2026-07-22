---
title: "Worklog Tuần 3"
date: 2026-05-18
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---


### Mục tiêu tuần 3:

* Đăng ký lịch lên văn phòng công ty AWS Việt Nam để giao lưu, học hỏi kinh nghiệm trực tiếp từ cán bộ hướng dẫn và các thành viên.
* Họp nhóm thảo luận, phân tích nhu cầu và lựa chọn đề tài/Workshop thực tập phù hợp.
* Bắt đầu nghiên cứu nền tảng bảo mật, phân quyền IAM và các mô hình trách nhiệm chia sẻ trên AWS để phục vụ đề tài.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| 2 | - Đăng ký lịch làm việc và tham gia giao lưu tại văn phòng công ty AWS Việt Nam <br>- Tìm hiểu mô hình trách nhiệm chia sẻ (Shared Responsibility Model) giữa AWS và người dùng | 18/05/2026 | 18/05/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 3 | - Giao lưu cùng anh chị hướng dẫn và các thành viên trong nhóm <br>- Khảo sát dịch vụ quản trị định danh AWS IAM (User, Group, Role, Policy) và nguyên tắc quyền tối thiểu | 19/05/2026 | 19/05/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 4 | - Họp nhóm thảo luận lựa chọn đề tài thực tập phù hợp với năng lực các thành viên <br>- Thực hành khởi tạo IAM User, IAM Group và gán policy quản lý truy cập dạng JSON | 20/05/2026 | 20/05/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 5 | - Thống nhất đề tài cùng nhóm và xin ý kiến định hướng từ cán bộ hướng dẫn <br>- Triển khai các lớp bảo vệ: bật MFA cho Root User, cấu hình IAM Role cho EC2 | 21/05/2026 | 21/05/2026 | <https://cloudjourney.awsstudygroup.com/> |
| 6 | - Thực hành cấu hình Customer Managed Key trong AWS KMS để mã hóa EBS/S3 <br>- Thực hiện dọn dẹp (clean-up) tài nguyên thử nghiệm và tổng hợp báo cáo tiến độ tuần 3 | 22/05/2026 | 24/05/2026 | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được tuần 3:

* **Về hoạt động tại đơn vị & Thảo luận đề tài:**
  * Hoàn thành việc đăng ký và có buổi giao lưu trực tiếp tại văn phòng công ty AWS Việt Nam.
  * Thảo luận xôi nổi cùng các thành viên trong nhóm và chốt được đề tài thực tập phù hợp dưới sự định hướng của cán bộ hướng dẫn.

* **Về kiến thức & Kỹ năng kỹ thuật:**
  * Hiểu rõ Mô hình trách nhiệm chia sẻ (Shared Responsibility Model) và nguyên tắc cấp quyền tối thiểu (Least Privilege).
  * Xây dựng thành công mô hình phân quyền quản trị bằng IAM (Users, Groups, Roles, Policies JSON) và bật xác thực đa yếu tố MFA.
  * Biết cách sử dụng AWS KMS (Customer Managed Key) để mã hóa dữ liệu ở trạng thái nghỉ cho các dịch vụ EBS và S3.
  * Duy trì quy trình dọn dẹp tài nguyên (Security Clean-up) sau khi thực hành để kiểm soát chi phí.