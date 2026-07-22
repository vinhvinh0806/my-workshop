---
title: "Tối ưu chi phí và đơn giản hóa vận hành với Writable Warm Storage trên Amazon OpenSearch Service"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

Trong quá trình tìm hiểu các bài viết trên **AWS Big Data Blog**, mình đã đọc bài viết **"Cut costs and simplify operations with writable warm storage in Amazon OpenSearch Service"**. Ban đầu mình nghĩ đây chỉ là một bản cập nhật nhỏ dành cho Amazon OpenSearch Service nhằm cải thiện hiệu năng lưu trữ. Tuy nhiên, sau khi đọc kỹ bài viết, mình nhận ra AWS đang giải quyết một vấn đề mà rất nhiều hệ thống phân tích dữ liệu lớn gặp phải: làm sao vừa giảm chi phí lưu trữ, vừa vẫn có thể cập nhật dữ liệu lịch sử khi cần.

Theo bài viết, nhiều hệ thống như quản lý log, giám sát ứng dụng hay phân tích sự kiện bảo mật thường lưu trữ khối lượng dữ liệu rất lớn, từ hàng terabyte đến hàng petabyte. Khi dữ liệu ngày càng tăng, việc giữ toàn bộ dữ liệu ở tầng lưu trữ hiệu năng cao sẽ khiến chi phí tăng đáng kể. Ngược lại, nếu chuyển toàn bộ dữ liệu sang tầng lưu trữ giá rẻ thì khả năng cập nhật dữ liệu lịch sử lại bị hạn chế. Đây chính là bài toán mà AWS muốn giải quyết thông qua Writable Warm Storage.

## Tổng quan bài viết

Bài viết giới thiệu tính năng **Writable Warm Storage** trên **Amazon OpenSearch Service**, được hỗ trợ từ phiên bản **3.3** trở lên. Đây là một cải tiến giúp tầng lưu trữ Warm không còn chỉ hỗ trợ đọc như UltraWarm trước đây mà có thể thực hiện cả thao tác ghi và cập nhật dữ liệu trực tiếp.

Theo AWS, thay vì phải di chuyển chỉ mục từ tầng Warm về tầng Hot mỗi khi cần chỉnh sửa dữ liệu, Writable Warm Storage cho phép cập nhật trực tiếp ngay tại tầng Warm. Điều này giúp giảm đáng kể thời gian xử lý, đơn giản hóa quá trình vận hành và tối ưu chi phí hạ tầng.

Qua phần giới thiệu này, mình thấy AWS không chỉ bổ sung thêm một tính năng mới mà còn thay đổi cách quản lý dữ liệu lịch sử trong OpenSearch Service theo hướng linh hoạt và hiệu quả hơn.

---

## Thách thức của mô hình lưu trữ phân tầng truyền thống

Amazon OpenSearch Service sử dụng mô hình lưu trữ phân tầng để cân bằng giữa hiệu năng và chi phí. Theo bài viết, dữ liệu thường được chia thành ba tầng chính:

- **Hot**: Lưu trữ dữ liệu mới với hiệu năng cao nhất, sử dụng ổ đĩa gắn trực tiếp vào máy chủ để phục vụ việc lập chỉ mục và tìm kiếm theo thời gian thực.
- **UltraWarm**: Lưu trữ dữ liệu ít được truy cập hơn với chi phí thấp nhờ sử dụng Amazon S3 kết hợp bộ nhớ đệm cục bộ.
- **Cold**: Lưu trữ dữ liệu rất ít khi truy cập với chi phí thấp nhất và cần chuyển trở lại Warm hoặc Hot trước khi sử dụng.

Đối với các dữ liệu chỉ đọc như nhật ký hệ thống, mô hình này hoạt động rất hiệu quả. Tuy nhiên, trong thực tế vẫn có nhiều trường hợp dữ liệu đến muộn hoặc cần chỉnh sửa để đáp ứng các yêu cầu về tuân thủ. Khi đó, UltraWarm trở thành một điểm hạn chế vì chỉ hỗ trợ đọc.

Theo AWS, để cập nhật chỉ một bản ghi trong UltraWarm, toàn bộ chỉ mục phải được chuyển về tầng Hot, thực hiện cập nhật rồi lại chuyển trở về UltraWarm. Quá trình này bao gồm nhiều bước như hợp nhất phân đoạn, tạo snapshot và di chuyển dữ liệu, có thể mất khoảng **130 phút cho một chỉ mục 100 GB**, đồng thời tiêu tốn đáng kể tài nguyên CPU, RAM và dung lượng ổ đĩa của các nút Hot.

Theo mình, đây là nguyên nhân khiến nhiều doanh nghiệp phải giữ dữ liệu ở tầng Hot lâu hơn mức cần thiết hoặc đầu tư thêm tài nguyên chỉ để phục vụ các lần cập nhật không thường xuyên. Điều này làm tăng chi phí vận hành và khiến việc quản lý hệ thống trở nên phức tạp hơn.

---

## Writable Warm Storage hoạt động như thế nào?

Để giải quyết những hạn chế của UltraWarm, AWS giới thiệu **Writable Warm Storage** sử dụng các phiên bản **OpenSearch Optimized (OI2)**.

Theo bài viết, cả tầng Hot và Writable Warm đều sử dụng Amazon S3 làm bộ lưu trữ bền vững. Vì vậy, khi dữ liệu được chuyển giữa hai tầng, hệ thống chỉ cần thực hiện việc di chuyển phân đoạn nhẹ thay vì sao chép toàn bộ dữ liệu như trước đây.

Bên cạnh đó, công cụ tìm kiếm Lucene hoạt động giống nhau trên cả hai tầng nên Writable Warm vẫn hỗ trợ đầy đủ các thao tác như ghi dữ liệu, hợp nhất nền và làm mới chỉ mục tương tự tầng Hot. Điều này giúp các dữ liệu đến muộn hoặc các bản ghi cần chỉnh sửa có thể được cập nhật trực tiếp chỉ trong vài giây mà không cần chuyển trở lại tầng Hot.

**Hình 1. So sánh luồng dữ liệu giữa UltraWarm và Writable Warm**

![So sánh UltraWarm và Writable Warm](/images/writable-warm-storage.png)

*Hình 1. Writable Warm Storage cho phép ghi trực tiếp vào tầng Warm, loại bỏ quy trình chuyển chỉ mục giữa Hot và Warm, giúp giảm thời gian cập nhật dữ liệu và đơn giản hóa quá trình vận hành.*
---

## Lợi ích về chi phí, vận hành và tính linh hoạt

Theo AWS, Writable Warm Storage không chỉ giúp cập nhật dữ liệu thuận tiện hơn mà còn mang lại nhiều lợi ích về chi phí, vận hành và khả năng mở rộng hệ thống. Đây cũng là những điểm khiến tính năng này trở thành một lựa chọn phù hợp đối với các doanh nghiệp đang lưu trữ lượng dữ liệu lớn trên Amazon OpenSearch Service.

### Tối ưu chi phí hạ tầng

Một điểm mình thấy khá ấn tượng trong bài viết là Writable Warm Storage hỗ trợ **Reserved Instances (RI)** trên các phiên bản **OpenSearch Optimized (OI2)**. Trước đây, UltraWarm chỉ hỗ trợ mô hình tính phí theo nhu cầu (On-Demand), trong khi Writable Warm cho phép doanh nghiệp cam kết sử dụng trong thời gian dài để nhận mức giá ưu đãi hơn.

Theo AWS, doanh nghiệp có thể tiết kiệm khoảng **36%** khi sử dụng Reserved Instance trong thời hạn một năm và lên đến **48%** khi sử dụng Reserved Instance trong ba năm. Ngoài ra, do dữ liệu có thể được chuyển sang tầng Warm sớm hơn thay vì phải giữ lâu ở tầng Hot để chờ các bản cập nhật, tổng chi phí lưu trữ cũng giảm đáng kể.

Theo mình, đây là một cải tiến rất hữu ích đối với các hệ thống lưu trữ log hoặc dữ liệu phân tích trong thời gian dài vì chi phí hạ tầng thường chiếm tỷ trọng khá lớn trong quá trình vận hành.

### Đơn giản hóa quá trình vận hành

Một lợi ích khác được AWS nhấn mạnh là Writable Warm Storage giúp giảm đáng kể sự phức tạp trong quản trị hệ thống.

Trước đây, mỗi lần cập nhật dữ liệu lịch sử đều phải trải qua nhiều bước như hợp nhất phân đoạn (force merge), tạo snapshot và di chuyển dữ liệu giữa các tầng lưu trữ. Những thao tác này vừa tiêu tốn tài nguyên CPU, RAM, ổ đĩa vừa cần được thực hiện vào các khoảng thời gian phù hợp để tránh ảnh hưởng đến hoạt động của hệ thống.

Với Writable Warm Storage, toàn bộ quá trình trên được thay thế bằng việc di chuyển phân đoạn nhẹ và ghi dữ liệu trực tiếp trên tầng Warm. Điều này giúp giảm tải cho các nút Hot, đơn giản hóa các chính sách **Index State Management (ISM)** và loại bỏ nhiều quy trình quản trị phức tạp trước đây.

Theo mình, việc đơn giản hóa quy trình vận hành sẽ giúp các nhóm quản trị tiết kiệm nhiều thời gian hơn và hạn chế các rủi ro phát sinh trong quá trình cập nhật dữ liệu.

### Linh hoạt hơn khi mở rộng hệ thống

Theo bài viết, Writable Warm Storage còn cung cấp nhiều lựa chọn cấu hình hơn so với UltraWarm.

Trong khi UltraWarm chỉ hỗ trợ hai loại instance thì Writable Warm hỗ trợ nhiều kích thước từ **oi2.large** đến **oi2.16xlarge**, giúp doanh nghiệp lựa chọn cấu hình phù hợp với từng quy mô dữ liệu cũng như nhu cầu xử lý khác nhau.

Theo mình, việc có nhiều lựa chọn cấu hình sẽ giúp doanh nghiệp mở rộng hệ thống linh hoạt hơn mà không cần thay đổi toàn bộ kiến trúc lưu trữ khi khối lượng dữ liệu tăng lên.

---

## Hiệu năng tìm kiếm và khi nào nên sử dụng Writable Warm

Ngoài việc tối ưu chi phí và vận hành, AWS cũng tiến hành đánh giá hiệu năng của Writable Warm Storage bằng bộ dữ liệu chuẩn **NYC Taxi**.

Kết quả cho thấy Writable Warm đạt hiệu năng tương đương hoặc tốt hơn UltraWarm trong **6 trên 7** loại truy vấn được thử nghiệm. Các truy vấn lọc dữ liệu, truy vấn theo phạm vi, sắp xếp dữ liệu và tổng hợp theo thời gian đều đạt kết quả rất tốt. Điều này cho thấy việc bổ sung khả năng ghi dữ liệu không làm giảm hiệu năng tìm kiếm mà vẫn đảm bảo khả năng truy vấn ổn định.

Theo AWS, Writable Warm phù hợp với những hệ thống thường xuyên cần cập nhật dữ liệu lịch sử, muốn tận dụng Reserved Instances để giảm chi phí hoặc cần nhiều lựa chọn cấu hình phần cứng. Trong khi đó, UltraWarm vẫn là lựa chọn phù hợp đối với các hệ thống chỉ lưu trữ dữ liệu bất biến hoặc cần kết hợp với Cold Storage.

Qua phần này, mình nhận ra rằng việc lựa chọn giữa Writable Warm và UltraWarm không phụ thuộc vào việc công nghệ nào tốt hơn, mà phụ thuộc vào đặc điểm dữ liệu và nhu cầu vận hành của từng hệ thống.

---

## Bài học rút ra

Sau khi đọc bài viết, mình rút ra một số điểm đáng chú ý:

- Mô hình lưu trữ phân tầng giúp cân bằng giữa hiệu năng và chi phí khi quản lý lượng dữ liệu rất lớn.
- Writable Warm Storage khắc phục hạn chế chỉ đọc của UltraWarm bằng cách cho phép ghi và cập nhật dữ liệu trực tiếp trên tầng Warm.
- Việc loại bỏ quy trình chuyển dữ liệu giữa Hot và Warm giúp giảm thời gian xử lý và đơn giản hóa quá trình vận hành.
- Reserved Instances trên các phiên bản OI2 giúp doanh nghiệp tối ưu chi phí khi triển khai Amazon OpenSearch Service trong thời gian dài.
- Việc lựa chọn giữa Writable Warm và UltraWarm nên dựa trên nhu cầu cập nhật dữ liệu cũng như mô hình triển khai thực tế của từng hệ thống.

---

## Kết luận

Sau khi đọc bài viết, mình hiểu rằng Writable Warm Storage không chỉ là một tính năng mới của Amazon OpenSearch Service mà còn là một cải tiến quan trọng trong cách quản lý dữ liệu lịch sử. Thay vì phải liên tục chuyển dữ liệu giữa các tầng lưu trữ để thực hiện cập nhật, doanh nghiệp có thể ghi trực tiếp vào tầng Warm, từ đó giảm chi phí, đơn giản hóa quá trình vận hành và vẫn duy trì hiệu năng tìm kiếm tốt.

Theo mình, đây là một tính năng rất hữu ích đối với các hệ thống quản lý log, phân tích dữ liệu hoặc giám sát hạ tầng trên AWS vì vừa giúp tối ưu ngân sách vừa giúp việc cập nhật dữ liệu lịch sử trở nên nhanh chóng hơn. Nếu doanh nghiệp đang sử dụng Amazon OpenSearch Service với khối lượng dữ liệu lớn và thường xuyên phát sinh các bản cập nhật muộn, Writable Warm Storage là một giải pháp rất đáng để cân nhắc.

---

## Tài liệu tham khảo

- AWS Big Data Blog: *Cut costs and simplify operations with writable warm storage in Amazon OpenSearch Service*
- https://aws.amazon.com/vi/blogs/big-data/cut-costs-and-simplify-operations-with-writable-warm-storage-in-amazon-opensearch-service/