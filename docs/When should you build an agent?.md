## Khi nào nên xây dựng một agent?

Việc xây dựng **agent** đòi hỏi bạn phải tư duy lại cách hệ thống của mình đưa ra quyết định và xử lý sự phức tạp. Không giống như các hệ thống tự động hóa truyền thống, **agent** đặc biệt phù hợp với các quy trình làm việc mà các phương pháp quyết định theo luật lệ và logic cứng nhắc thường không đáp ứng được.

Hãy xem xét ví dụ về phân tích gian lận thanh toán. Một hệ thống quy tắc truyền thống hoạt động như một danh sách kiểm tra, gắn cờ các giao dịch dựa trên các tiêu chí được thiết lập sẵn. Ngược lại, một **agent** sử dụng mô hình ngôn ngữ lớn (LLM) hoạt động giống như một điều tra viên dày dạn kinh nghiệm, đánh giá bối cảnh, xem xét các mẫu tinh vi và phát hiện hoạt động đáng ngờ ngay cả khi không vi phạm các quy tắc rõ ràng. Chính khả năng suy luận tinh tế này là điều cho phép **agent** xử lý hiệu quả những tình huống phức tạp và mơ hồ.

Khi đánh giá nơi **agent** có thể mang lại giá trị, hãy ưu tiên các quy trình làm việc từng *kháng cự* với tự động hóa, đặc biệt là nơi các phương pháp truyền thống gặp khó khăn:

### 01. Ra quyết định phức tạp
Các quy trình cần phán đoán tinh tế, có nhiều ngoại lệ, hoặc phải đưa ra quyết định tùy theo ngữ cảnh ví dụ như quy trình phê duyệt hoàn tiền trong dịch vụ khách hàng.

### 02. Luật lệ khó duy trì
Các hệ thống trở nên cồng kềnh do có quá nhiều luật phức tạp, khiến việc cập nhật trở nên tốn kém hoặc dễ sai sót ví dụ như đánh giá bảo mật nhà cung cấp.

### 03. Phụ thuộc nhiều vào dữ liệu phi cấu trúc
Các kịch bản cần diễn giải ngôn ngữ tự nhiên, trích xuất ý nghĩa từ tài liệu, hoặc tương tác hội thoại với người dùng ví dụ như xử lý yêu cầu bồi thường bảo hiểm nhà.

---

Trước khi quyết định xây dựng một **agent**, hãy xác minh rằng trường hợp sử dụng của bạn đáp ứng rõ ràng các tiêu chí trên. Nếu không, một giải pháp quyết định theo logic cứng (*deterministic*) có thể đã đủ.
