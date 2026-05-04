# Lab 18 Reflection - Lakehouse Architecture

**Câu hỏi:** Anti-pattern nào trong thiết kế Data Lakehouse mà team bạn dễ mắc phải nhất? Tại sao?

**Trả lời:**

Anti-pattern mà nhóm mình dễ vướng phải nhất là **"Lạm dụng Time Travel và thiếu chiến lược VACUUM"**. 

**Lý do:**
Tính năng Time Travel của Delta Lake rất quyền năng, cho phép truy vấn lại dữ liệu cũ và khôi phục lỗi (như đã thực hành trong NB3). Tuy nhiên, nhóm dễ mắc sai lầm khi giữ lại lịch sử quá lâu mà không có kế hoạch dọn dẹp định kỳ. 

Trong kiến trúc Lakehouse, các file dữ liệu cũ không bị xóa vật lý ngay lập tức sau khi Update/Delete mà vẫn tồn tại để phục vụ Time Travel. Nếu không chạy lệnh `VACUUM` để dọn dẹp các tập tin đã lỗi thời, dung lượng lưu trữ trên Cloud (S3/GCS) sẽ tăng vọt theo thời gian, dẫn đến chi phí vận hành (FinOps) ngoài tầm kiểm soát. Việc cân bằng giữa "khả năng phục hồi dữ liệu" và "tối ưu chi phí lưu trữ" là một bài toán quản trị mà nhóm cần đặc biệt lưu ý khi triển khai thực tế.
