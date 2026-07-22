---
title: "Amazon Bedrock AgentCore cập nhật nhiều tính năng mới giúp xây dựng AI Agent thông minh hơn"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---


Trong quá trình tìm hiểu các bài viết trên **AWS Machine Learning Blog**, mình đã đọc bài viết **"New in Amazon Bedrock AgentCore: Build agents with broader knowledge and continuous learning"**. Ban đầu mình nghĩ đây chỉ là một bản cập nhật bổ sung thêm một vài tính năng mới cho Amazon Bedrock AgentCore. Tuy nhiên, sau khi đọc, mình nhận ra điều AWS muốn hướng đến không chỉ là nâng cấp khả năng của AI Agent mà còn xây dựng một nền tảng hoàn chỉnh giúp Agent có thể làm việc hiệu quả hơn trong môi trường doanh nghiệp.

Điều mình thấy thú vị là bài viết không tập trung vào việc giới thiệu một mô hình AI mới, mà giải quyết những hạn chế mà rất nhiều AI Agent đang gặp phải hiện nay. Theo AWS, một mô hình AI dù có khả năng suy luận rất tốt vẫn khó phát huy hết tiềm năng nếu không được cung cấp đúng dữ liệu, thiếu khả năng truy cập các nguồn thông tin bên ngoài hoặc không có cơ chế cải thiện sau khi triển khai. Chính vì vậy, AgentCore được bổ sung nhiều tính năng mới nhằm giúp AI Agent có thể khai thác tri thức tốt hơn, học hỏi từ quá trình vận hành và đảm bảo an toàn khi làm việc với dữ liệu doanh nghiệp.

---

## Tổng quan bài viết

Bài viết giới thiệu những cập nhật mới của **Amazon Bedrock AgentCore**, nền tảng được AWS phát triển để xây dựng, kết nối và tối ưu các AI Agent.

Thay vì chỉ tập trung vào mô hình ngôn ngữ lớn (LLMs), AgentCore được mở rộng theo nhiều hướng khác nhau nhằm giúp Agent hoạt động hiệu quả hơn trong môi trường thực tế. Các tính năng mới tập trung vào bốn nhóm chính:

- Mở rộng khả năng truy cập tri thức.
- Hỗ trợ Agent học hỏi và cải thiện liên tục.
- Tăng cường khả năng bảo mật.
- Đơn giản hóa quá trình xây dựng và vận hành AI Agent.

Qua phần mở đầu, mình thấy AWS đang định hướng AgentCore trở thành một nền tảng đầy đủ để phát triển AI Agent thay vì chỉ là dịch vụ hỗ trợ chạy mô hình AI.

---

## Vì sao AWS tiếp tục mở rộng AgentCore?

Theo bài viết, các mô hình ngôn ngữ lớn hiện nay đã có khả năng suy luận, lập kế hoạch và tạo nội dung rất tốt. Tuy nhiên, trên thực tế nhiều AI Agent vẫn chưa thể phát huy hết khả năng của mình.

Nguyên nhân không nằm ở mô hình mà nằm ở việc Agent thiếu ngữ cảnh để đưa ra quyết định.

Ví dụ, một Agent chăm sóc khách hàng sẽ khó trả lời chính xác nếu không thể truy cập tài liệu nội bộ của doanh nghiệp. Một Agent nghiên cứu thị trường sẽ không thể tổng hợp đầy đủ thông tin nếu chỉ dựa trên dữ liệu đã được huấn luyện trước đó. Tương tự, một Agent tư vấn tài chính cũng sẽ gặp nhiều hạn chế nếu không thể truy cập dữ liệu thị trường theo thời gian thực hoặc các nguồn dữ liệu trả phí.

Qua những ví dụ này, mình nhận ra rằng sức mạnh của AI Agent không chỉ phụ thuộc vào mô hình mà còn phụ thuộc vào lượng tri thức và công cụ mà Agent có thể sử dụng.

Theo AWS, để một AI Agent hoạt động hiệu quả trong môi trường doanh nghiệp thì ngoài khả năng suy luận còn cần ba yếu tố quan trọng:

- Có quyền truy cập vào nguồn tri thức phù hợp.
- Có khả năng sử dụng các công cụ cần thiết.
- Có cơ chế liên tục đánh giá và cải thiện sau khi được triển khai.

Đây cũng chính là hướng phát triển mà Amazon Bedrock AgentCore đang theo đuổi trong bản cập nhật lần này.

---

## AgentCore bổ sung ba lớp kiến thức cho AI Agent

Điểm mình thấy nổi bật nhất trong bài viết là AWS chia nguồn dữ liệu của AI Agent thành ba lớp kiến thức khác nhau. Điều này giúp Agent không còn phụ thuộc hoàn toàn vào dữ liệu đã được huấn luyện mà có thể khai thác thêm nhiều nguồn thông tin để phục vụ quá trình suy luận và ra quyết định.

### Lớp kiến thức nội bộ (Managed Knowledge Base)

Lớp đầu tiên là **Amazon Bedrock Managed Knowledge Base**.

Theo bài viết, dữ liệu quan trọng của doanh nghiệp thường nằm trên nhiều hệ thống khác nhau như SharePoint, Google Drive, Confluence hoặc Amazon S3. Trước đây, để AI Agent truy cập các nguồn dữ liệu này, nhà phát triển thường phải tự xây dựng hệ thống RAG, quản lý Vector Database và tối ưu quá trình truy xuất dữ liệu.

Với AgentCore, AWS quản lý phần lớn công việc này. Người phát triển chỉ cần kết nối nguồn dữ liệu, còn AgentCore sẽ xử lý việc lập chỉ mục, truy xuất, xếp hạng kết quả và tối ưu tìm kiếm. Theo mình, điều này giúp rút ngắn đáng kể thời gian phát triển AI Agent và giúp Agent có thể trả lời dựa trên chính dữ liệu của doanh nghiệp thay vì chỉ dựa vào dữ liệu huấn luyện.

### Lớp kiến thức toàn cầu (Web Search)

Lớp thứ hai là **Web Search**.

Nếu dữ liệu nội bộ giúp Agent hiểu doanh nghiệp thì Web Search giúp Agent tiếp cận các thông tin mới nhất trên Internet.

Theo AWS, Web Search được xây dựng trên cùng hạ tầng tìm kiếm của Amazon và kết hợp với Knowledge Graph để cung cấp các dữ liệu đã được xác minh như thông tin doanh nghiệp, giá cổ phiếu hoặc các sự kiện đang diễn ra.

Điều mình thấy khá hay là toàn bộ quá trình tìm kiếm vẫn diễn ra trong môi trường AWS nên không cần tích hợp thêm nhiều dịch vụ bên ngoài. Điều này giúp vừa đảm bảo khả năng cập nhật thông tin theo thời gian thực, vừa đáp ứng các yêu cầu về bảo mật và quản trị dữ liệu.
### Lớp kiến thức trả phí (AgentCore Identity và Payment)

Lớp kiến thức cuối cùng mà AWS giới thiệu là **AgentCore Identity và Payment**.

Theo bài viết, không phải nguồn dữ liệu nào cũng được cung cấp miễn phí. Trong nhiều lĩnh vực như tài chính, nghiên cứu khoa học hoặc phân tích thị trường, các dữ liệu có giá trị thường nằm sau các dịch vụ trả phí hoặc API thương mại. Nếu AI Agent không thể truy cập các nguồn dữ liệu này thì kết quả đưa ra có thể chưa đầy đủ hoặc chưa chính xác.

Để giải quyết vấn đề này, AgentCore bổ sung cơ chế cho phép Agent xác thực danh tính, truy cập các dịch vụ trả phí và thực hiện thanh toán một cách an toàn ngay trong quá trình xử lý. Đồng thời, AWS cũng cung cấp cơ chế giúp các nhà cung cấp nội dung kiểm soát việc AI Agent được phép truy cập, từ đó tạo ra một hệ sinh thái đáng tin cậy cho cả người sử dụng và nhà cung cấp dữ liệu.

Theo mình, đây là một điểm khá mới vì nó mở ra khả năng xây dựng các AI Agent có thể làm việc với nhiều nguồn dữ liệu chuyên ngành thay vì chỉ sử dụng các nguồn dữ liệu công khai như trước đây.

**Hình 1. Ba lớp kiến thức của Amazon Bedrock AgentCore**

![Ba lớp kiến thức của Amazon Bedrock AgentCore](/images/agentcore-knowledge-layers.png)

*Hình 1. AgentCore mở rộng khả năng của AI Agent thông qua ba lớp kiến thức gồm dữ liệu nội bộ, dữ liệu trên Internet và các nguồn dữ liệu trả phí.*

---

## AI Agent không chỉ biết nhiều hơn mà còn học hỏi tốt hơn

Một điểm mình thấy khá ấn tượng trong bài viết là AWS không chỉ giúp AI Agent truy cập nhiều dữ liệu hơn mà còn bổ sung các công cụ để theo dõi và cải thiện chất lượng Agent sau khi được triển khai.

Theo AWS, nhiều lỗi của AI Agent rất khó phát hiện vì chúng không tạo ra thông báo lỗi rõ ràng. Chẳng hạn, Agent có thể xác nhận một thao tác chưa từng được thực hiện hoặc trả lời sai khi một API gặp sự cố nhưng hệ thống vẫn ghi nhận phiên làm việc thành công. Những lỗi này thường chỉ được phát hiện khi người dùng phản hồi hoặc khi đã ảnh hưởng đến nhiều phiên làm việc.

Để giải quyết vấn đề này, AgentCore bổ sung các công cụ phân tích giúp thu thập và đánh giá dữ liệu từ quá trình vận hành thực tế. Hệ thống có thể phát hiện các lỗi lặp lại, phân tích ý định của người dùng, theo dõi cách Agent xử lý từng tác vụ và xác định những điểm cần cải thiện. Dựa trên các kết quả này, AgentCore còn đưa ra đề xuất điều chỉnh Prompt hoặc mô tả công cụ, đồng thời hỗ trợ đánh giá hàng loạt và A/B Testing trước khi áp dụng vào môi trường sản xuất.

Theo mình, cách tiếp cận này giúp việc tối ưu AI Agent trở nên có cơ sở hơn. Thay vì điều chỉnh theo cảm tính, nhà phát triển có thể dựa trên dữ liệu thực tế để cải thiện chất lượng Agent một cách liên tục và có hệ thống.

**Hình 2. Quy trình cải thiện AI Agent trên AgentCore**

![Quy trình cải thiện AI Agent trên AgentCore](/images/agentcore-optimization-loop.png)

*Hình 2. AgentCore hỗ trợ phân tích dữ liệu vận hành, đưa ra đề xuất cải thiện và đánh giá kết quả trước khi triển khai.*

---

## AWS tiếp tục tăng cường khả năng bảo mật

Khi AI Agent ngày càng có khả năng truy cập nhiều nguồn dữ liệu và thực hiện nhiều hành động hơn, vấn đề bảo mật cũng trở nên quan trọng hơn.

Trong bản cập nhật này, AWS tích hợp **Amazon Bedrock Guardrails** trực tiếp vào AgentCore nhằm kiểm soát các hành động của Agent, phát hiện Prompt Injection, ngăn chặn nội dung độc hại và giảm nguy cơ truy cập hoặc tiết lộ dữ liệu nhạy cảm.

Ngoài ra, AgentCore còn được thiết kế để có thể tích hợp với nhiều giải pháp bảo mật của các đối tác như Check Point, Zscaler, Rubrik, Netskope và SentinelOne. Điều này giúp doanh nghiệp xây dựng các chính sách bảo mật thống nhất ngay cả khi Agent sử dụng nhiều nguồn dữ liệu hoặc nhiều công cụ khác nhau.

Theo mình, đây là một tính năng rất cần thiết vì khi AI Agent ngày càng thông minh hơn thì các cơ chế kiểm soát cũng cần được tăng cường để hạn chế các rủi ro trong quá trình vận hành.

---

## AgentCore Harness giúp xây dựng AI Agent nhanh hơn

Một tính năng khác được AWS giới thiệu là **AgentCore Harness**.

Theo bài viết, thay vì phải tự xây dựng toàn bộ vòng đời của AI Agent, nhà phát triển chỉ cần khai báo các thông tin như mô hình AI, công cụ, kỹ năng và hướng dẫn hoạt động thông qua cấu hình.

Sau đó, AgentCore sẽ tự động tạo môi trường thực thi, quản lý bộ nhớ giữa các phiên làm việc, điều phối công cụ, duy trì trạng thái của Agent và cung cấp sẵn nhiều thành phần cần thiết để Agent có thể hoạt động.

Theo mình, cách tiếp cận này giúp giảm đáng kể thời gian phát triển AI Agent, đặc biệt đối với những nhóm mới bắt đầu xây dựng ứng dụng Generative AI. Đồng thời, khi hệ thống mở rộng, nhà phát triển vẫn có thể tiếp tục sử dụng cùng một nền tảng mà không cần thiết kế lại toàn bộ kiến trúc.

---

## Bài học rút ra

Sau khi đọc bài viết, mình rút ra một số điểm đáng chú ý:

- AI Agent không chỉ cần một mô hình AI mạnh mà còn cần được cung cấp đúng dữ liệu và công cụ để làm việc hiệu quả.
- Việc kết hợp dữ liệu nội bộ, dữ liệu trên Internet và các nguồn dữ liệu trả phí giúp Agent có nhiều ngữ cảnh hơn khi đưa ra quyết định.
- Theo dõi quá trình vận hành và cải thiện Agent liên tục là yếu tố quan trọng để nâng cao chất lượng của hệ thống.
- Các cơ chế bảo mật như Guardrails đóng vai trò quan trọng khi AI Agent ngày càng có nhiều quyền truy cập vào dữ liệu và tài nguyên của doanh nghiệp.
- AgentCore giúp đơn giản hóa quá trình xây dựng, triển khai và mở rộng AI Agent trên AWS.

---

## Kết luận

Sau khi đọc bài viết, mình hiểu rằng Amazon Bedrock AgentCore không chỉ bổ sung thêm các tính năng mới mà còn hướng đến việc xây dựng một nền tảng hoàn chỉnh cho AI Agent trong môi trường doanh nghiệp. Thay vì chỉ tập trung vào mô hình AI, AWS mở rộng AgentCore theo nhiều hướng như truy cập tri thức, tìm kiếm thông tin theo thời gian thực, học hỏi từ dữ liệu vận hành và tăng cường các cơ chế bảo mật.

Theo mình, đây là một bài viết rất hữu ích đối với những bạn đang tìm hiểu về **Generative AI**, **Amazon Bedrock** hoặc phát triển **AI Agent** trên AWS, bởi nó giúp mình hiểu rõ hơn những thành phần cần thiết để xây dựng một AI Agent có thể hoạt động hiệu quả trong thực tế thay vì chỉ dừng lại ở khả năng sinh nội dung.

---

## Tài liệu tham khảo

- AWS Machine Learning Blog: *New in Amazon Bedrock AgentCore: Build agents with broader knowledge and continuous learning*
- https://aws.amazon.com/blogs/machine-learning/new-in-amazon-bedrock-agentcore-build-agents-with-broader-knowledge-and-continuous-learning/