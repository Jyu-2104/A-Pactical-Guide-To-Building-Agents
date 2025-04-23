## Cấu trúc cơ bản của một agent

Ở dạng cơ bản nhất, một **agent** bao gồm ba thành phần cốt lõi:

### 01. Model  
Mô hình ngôn ngữ lớn (LLM) làm nền tảng cho khả năng suy luận và ra quyết định của agent.

### 02. Tools  
Các hàm hoặc API bên ngoài mà agent có thể sử dụng để thực hiện hành động.

### 03. Instructions  
Các hướng dẫn rõ ràng và ranh giới kiểm soát xác định cách agent hoạt động.

---

### Ví dụ: Sử dụng OpenAI Agents SDK trong Python

Bạn có thể triển khai các khái niệm này bằng thư viện mà bạn ưa thích, hoặc xây dựng hoàn toàn từ đầu. Dưới đây là một ví dụ về cách định nghĩa một agent sử dụng SDK của OpenAI:

```python
weather_agent = Agent(
    name="Weather agent",
    instructions="Bạn là một agent giúp người dùng kiểm tra thời tiết.",
    tools=[get_weather],
)
```

## Lựa chọn mô hình cho agent

Các mô hình khác nhau có những điểm mạnh và đánh đổi riêng liên quan đến độ phức tạp của nhiệm vụ, độ trễ và chi phí. Như bạn sẽ thấy trong phần tiếp theo về **Orchestration**, bạn có thể cân nhắc sử dụng nhiều loại mô hình khác nhau cho từng tác vụ trong quy trình làm việc.

Không phải tác vụ nào cũng cần đến mô hình thông minh nhất — ví dụ, các tác vụ đơn giản như truy xuất thông tin hay phân loại ý định có thể được xử lý bởi mô hình nhỏ hơn, nhanh hơn. Trong khi đó, các tác vụ phức tạp hơn như quyết định có hoàn tiền hay không có thể cần đến một mô hình mạnh mẽ hơn.

### Chiến lược hiệu quả:
- **Bắt đầu với mô hình mạnh nhất** để xây dựng nguyên mẫu agent, nhằm thiết lập một chuẩn hiệu suất ban đầu.
- Sau đó, **thử thay thế bằng các mô hình nhỏ hơn** để xem liệu vẫn đạt được kết quả chấp nhận được hay không.
- Cách tiếp cận này giúp bạn không giới hạn khả năng của agent ngay từ đầu và dễ dàng chẩn đoán điểm mạnh – yếu của từng mô hình.

---

### Tóm tắt nguyên tắc chọn mô hình:

01. **Thiết lập đánh giá (evals)** để xác định chuẩn hiệu suất ban đầu  
02. **Tập trung vào độ chính xác mục tiêu** bằng cách sử dụng mô hình tốt nhất hiện có  
03. **Tối ưu chi phí và độ trễ** bằng cách thay thế các mô hình lớn bằng mô hình nhỏ hơn khi có thể  

---

📘 Bạn có thể tìm thấy hướng dẫn chi tiết về cách lựa chọn mô hình OpenAI tại [đây](https://platform.openai.com/docs/guides/gpt).


## Định nghĩa công cụ (tools)

**Tools** mở rộng khả năng của **agent** bằng cách sử dụng các API từ các ứng dụng hoặc hệ thống nền tảng. Đối với các hệ thống cũ không có API, **agent** có thể dựa vào các mô hình tương tác máy tính để thao tác trực tiếp với giao diện web hoặc ứng dụng — giống như con người thực hiện.

Mỗi công cụ nên được định nghĩa một cách chuẩn hóa, giúp thiết lập mối quan hệ linh hoạt, nhiều-nhiều giữa công cụ và agent. Các công cụ được ghi chép kỹ, kiểm thử đầy đủ, và có thể tái sử dụng sẽ:
- Dễ dàng được tìm thấy lại (discoverability)
- Đơn giản hóa việc quản lý phiên bản
- Tránh việc định nghĩa lại không cần thiết

---

### Ba loại công cụ chính mà agent cần:

| **Loại**        | **Mô tả**                                                                 | **Ví dụ**                                                                 |
|------------------|--------------------------------------------------------------------------|---------------------------------------------------------------------------|
| **Data**         | Giúp agent truy xuất ngữ cảnh và thông tin cần thiết cho quy trình.       | Truy vấn cơ sở dữ liệu giao dịch, hệ thống CRM, đọc tài liệu PDF, tìm kiếm web. |
| **Action**       | Cho phép agent tương tác với hệ thống để thực hiện hành động cụ thể.      | Gửi email/tin nhắn, cập nhật bản ghi CRM, chuyển yêu cầu hỗ trợ đến nhân viên. |
| **Orchestration**| Agent có thể sử dụng các agent khác như một công cụ (xem mẫu "Manager"). | Refund agent, Research agent, Writing agent.                             |

---

### Ví dụ thực tế: Định nghĩa công cụ cho agent bằng OpenAI Agents SDK

```python
from agents import Agent, WebSearchTool, function_tool

@function_tool
def save_results(output):
    db.insert({
        "output": output,
        "timestamp": datetime.time()
    })
    return "File saved"

search_agent = Agent(
    name="Search agent",
    instructions="Help the user search the internet and save results if asked.",
    tools=[WebSearchTool(), save_results],
)
```

👉 Khi số lượng công cụ cần thiết tăng lên, hãy cân nhắc chia nhỏ công việc thành nhiều agent khác nhau (xem phần Orchestration).


## Cấu hình hướng dẫn (Configuring instructions)

Hướng dẫn chất lượng cao là yếu tố thiết yếu đối với bất kỳ ứng dụng sử dụng LLM nào, nhưng đặc biệt quan trọng đối với các **agent**.  
Hướng dẫn rõ ràng giúp giảm sự mơ hồ và cải thiện khả năng ra quyết định của agent, từ đó thực hiện quy trình làm việc mượt mà hơn và ít lỗi hơn.

---

### Các thực tiễn tốt nhất khi viết hướng dẫn cho agent

#### Sử dụng tài liệu hiện có  
Khi tạo các quy trình, hãy sử dụng các quy trình vận hành hiện có, kịch bản hỗ trợ hoặc tài liệu chính sách để xây dựng các quy trình thân thiện với LLM.  
Trong lĩnh vực dịch vụ khách hàng chẳng hạn, các quy trình này có thể tương ứng với từng bài viết trong cơ sở tri thức của bạn.

#### Hướng dẫn agent chia nhỏ nhiệm vụ  
Việc cung cấp các bước nhỏ hơn, rõ ràng hơn từ những tài nguyên dày đặc giúp giảm sự mơ hồ và giúp mô hình dễ dàng làm theo hướng dẫn hơn.

#### Xác định hành động rõ ràng  
Đảm bảo mỗi bước trong quy trình tương ứng với một hành động hoặc đầu ra cụ thể.  
Ví dụ: một bước có thể yêu cầu agent hỏi người dùng mã đơn hàng của họ hoặc gọi một API để lấy thông tin tài khoản.  
Việc diễn đạt rõ ràng hành động (và thậm chí cả lời nhắn gửi tới người dùng) sẽ giảm thiểu sai sót trong quá trình hiểu và thực hiện.

#### Xử lý các trường hợp ngoại lệ  
Tương tác trong thế giới thực thường tạo ra các điểm cần ra quyết định, chẳng hạn như cách xử lý khi người dùng cung cấp thông tin không đầy đủ hoặc đưa ra câu hỏi bất ngờ.  
Một quy trình mạnh mẽ cần dự đoán các biến thể phổ biến và bao gồm hướng dẫn cách xử lý chúng bằng các bước điều kiện hoặc nhánh xử lý, chẳng hạn như bước thay thế nếu thiếu một thông tin bắt buộc.

---

Bạn có thể sử dụng các mô hình nâng cao như `o1` hoặc `o3-mini` để tự động tạo hướng dẫn từ tài liệu hiện có. Dưới đây là một ví dụ về prompt minh họa cách tiếp cận này:

```plaintext
Bạn là một chuyên gia trong việc viết hướng dẫn cho LLM agent. Hãy chuyển đổi tài liệu trung tâm trợ giúp sau thành một bộ hướng dẫn rõ ràng, được viết dưới dạng danh sách đánh số.  
Tài liệu này là một chính sách mà LLM cần tuân theo. Hãy đảm bảo không có sự mơ hồ, và các hướng dẫn được viết như những chỉ dẫn dành cho agent.  
Tài liệu trung tâm trợ giúp cần chuyển đổi là: {{help_center_doc}}
```
