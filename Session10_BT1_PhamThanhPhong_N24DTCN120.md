# BÀI 1: Phân Tích & Lựa Chọn Chiến Lược Tài Liệu Hóa

* **Họ và tên:** Phạm Thanh Phong
* **Mã sinh viên:** N24DTCN120
* **Lớp:** IT212 - AI Application in Action

---

## 1. Đáp án lựa chọn
**Đáp án: Phương án B**

---

## 2. Phân tích lý do chọn Phương án B

Phương án B tối ưu nhất vì tận dụng tối đa sức mạnh của một **AI Agent có khả năng đọc hiểu toàn bộ dự án (Repository-wide Context)**:

- **Ngữ cảnh toàn cục không bị mất (No Context Loss):** Antigravity sau khi index toàn bộ thư mục `Guai-api` có thể đọc và liên kết logic xuyên suốt nhiều file cùng lúc (Controller → Service → Repository → JWT Filter). Đây là điều quyết định vì luồng Checkout Flow không nằm ở một file duy nhất.
- **Truy vết luồng nghiệp vụ đầu cuối (End-to-End Flow Tracing):** Với một prompt được thiết kế theo cấu trúc Role + Goal + Output Format, AI có thể tự động truy vết entry point `/api/v1/checkout` đi qua tất cả lớp, phát hiện cả logic xác thực JWT, điều kiện lỗi và các trường hợp ngoại lệ — điều mà con người phải làm thủ công tốn nhiều giờ.
- **Đầu ra đúng chuẩn SRS ngay từ đầu:** Prompt yêu cầu rõ định dạng (Pre-conditions, Main flow, Exceptions) giúp AI sinh ra tài liệu sẵn sàng sử dụng, không cần mất thêm bước tổng hợp hay định dạng lại thủ công.
- **Tái sử dụng và nhất quán:** Toàn bộ ngữ cảnh được duy trì trong một phiên làm việc duy nhất, đảm bảo tài liệu SRS sinh ra nhất quán, không bị thiếu sót hay mâu thuẫn logic giữa các thành phần.

---

## 3. Phân tích lỗ hổng và rủi ro của 2 phương án bị loại trừ

### Phương án A (VSCode + GitHub Copilot Inline):
- **Thiếu ngữ cảnh đầu cuối (Severe Context Loss):** Copilot Chat chỉ hiểu đoạn code đang được bôi đen trong cửa sổ editor hiện tại. Nó không có khả năng tự động truy vết luồng từ Controller sang Service, từ Service sang Repository hay sang JWT Filter. Người dùng phải tự đọc, hiểu mối liên hệ và "kết nối" logic thủ công giữa từng lần giải thích rời rạc.
- **Tốn thời gian và dễ sai sót:** Phải lặp lại thao tác bôi đen - giải thích - ghi chú cho từng đoạn code ở từng file. Lập trình viên dễ bỏ sót các điều kiện lỗi hoặc edge case nằm ở một file khác mà họ chưa mở tới.
- **Kết quả phụ thuộc vào kỹ năng tổng hợp của người dùng:** Tài liệu SRS cuối cùng được tổng hợp thủ công từ nhiều đoạn giải thích riêng lẻ, tăng rủi ro mâu thuẫn logic, thiếu nhất quán định dạng và không đảm bảo đầy đủ.

### Phương án C (Copy-paste vào Web Chat ChatGPT/Gemini):
- **Giới hạn cửa sổ ngữ cảnh (Context Window Overflow):** Giao diện Web Chat có giới hạn token nhất định. Nếu 5 file Java có tổng dung lượng lớn, nội dung đầu file sẽ bị "quên" khi AI đọc đến cuối, dẫn đến tài liệu SRS bị thiếu hoặc mâu thuẫn.
- **Rủi ro bảo mật dữ liệu (Data Leakage Risk):** Việc copy-paste mã nguồn dự án nội bộ lên giao diện Web Chat công cộng (ChatGPT/Gemini) vi phạm nguyên tắc bảo mật thông tin doanh nghiệp. Mã nguồn có thể chứa thông tin nhạy cảm (endpoint nội bộ, logic xác thực, cấu trúc DB).
- **Không có khả năng đặt câu hỏi làm rõ ngữ cảnh:** AI không thể chủ động truy vết thêm thông tin khi cần vì không có quyền truy cập vào codebase gốc. Nếu đoạn code đã copy thiếu một phần logic quan trọng, AI sẽ tự bịa hoặc đưa ra kết luận sai.
- **Khó bảo trì tài liệu về sau:** Mỗi lần codebase thay đổi, lập trình viên phải lặp lại toàn bộ quy trình copy-paste từ đầu thay vì chỉ cần re-index và chạy lại prompt trong Antigravity.
