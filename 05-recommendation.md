# Day 27 — Workshop 5: Final Recommendation & Pitch

**Nhóm:** Nguyễn Thị Cẩm Nhung, Nguyễn Thanh Bình, Nguyễn Hoàng Việt Hùng
**Sản phẩm:** AI Travel Assistant
**Ngày:** 2026-05-15

---

## 1. Executive Summary (The 5-Minute Pitch)

Vietnam Airlines đang đối mặt với bài toán chi phí hỗ trợ khách hàng quốc tế lên tới $0.50 cho mỗi cuộc hội thoại. Nhóm chúng tôi đề xuất triển khai **AI Travel Assistant (Combo C - Smart Mix)**. Giải pháp này sử dụng kiến trúc Hybrid (80% GPT-4o-mini và 20% Claude 3.5 Sonnet) kết hợp với hệ thống Semantic Caching thông minh (60% hit rate). 

Kết quả Stress-test cho thấy chúng tôi có thể giảm chi phí vận hành từ $0.50 xuống còn **$0.0249/conv** (tiết kiệm lên đến ~95% trong kịch bản tối ưu nhất). Trong mùa cao điểm với 50,000 khách hàng, hệ thống này giúp hãng tiết kiệm hơn **$23,300 mỗi tháng** mà vẫn đảm bảo tính tuân thủ (Compliance) và độ tươi mới của thông tin thông qua cơ chế Web Search chọn lọc. Đây không chỉ là một chatbot, mà là một cỗ máy tối ưu hóa biên lợi nhuận đạt mức 75% cho dịch vụ hỗ trợ khách hàng.

---

## 2. Trả lời 4 câu hỏi PM

### Q1: Nhóm chọn cấu hình (Combo) nào để triển khai? Vì sao?
**Trả lời:** Nhóm chọn **Combo C (Smart Mix) với kịch bản Opt 1 (Strict Routing)**. 
- **Lý do:** Đây là kịch bản duy nhất cân bằng được giữa độ thông minh của mô hình Strong Tier (Claude Sonnet) cho các vấn đề nhạy cảm và chi phí cực thấp của Mini Tier cho FAQ. Việc siết chặt tỉ lệ Handover ở mức 2.5% đảm bảo chúng ta đạt mục tiêu kinh tế < $0.03/conv.

### Q2: Tổng số tiền tiết kiệm được (Cost Savings) là bao nhiêu so với thuê người?
**Trả lời:** 
- **Mỗi cuộc hội thoại:** Tiết kiệm được **$0.475**.
- **Mùa cao điểm (50k khách):** Tiết kiệm **$23,380.50/tháng**.
- **Hiệu quả:** Rẻ hơn sức người 20 lần (nếu không tính các ca handover).

### Q3: Khi nào thì bạn sẽ nâng cấp hoặc hạ cấp cấu hình?
**Trả lời:**
- **Nâng cấp (Lên Combo B - Premium):** Khi tỉ lệ lỗi thông tin (Hallucination) ở các Intent về Visa tăng > 2% hoặc điểm hài lòng CSAT < 4.0/5.
- **Switch to Budget Saver (Combo A):** Khi lưu lượng khách hàng tăng đột biến > 200,000 session/tháng vượt quá ngân sách dự phòng API, hoặc khi các mô hình Mini Tier tiến hóa đủ để hỗ trợ các tác vụ Compliance trong phạm vi giới hạn (constrained scope).

### Q4: Rủi ro lớn nhất về mặt kinh tế của sản phẩm này là gì?
**Trả lời:** **Rủi ro bùng nổ chi phí nhân sự (Human Handover Spike)**. Nếu AI không thể giải quyết các câu hỏi phức tạp và tỉ lệ khách đòi gặp người thật tăng lên 10%, tổng chi phí sẽ tăng vọt lên $0.065/conv (gấp đôi dự toán). 
- **Biện pháp giảm thiểu:** Liên tục cải thiện RAG và Prompt cho luồng từ chối khéo léo, đồng thời cập nhật Memory Layer để AI không hỏi lại những điều khách đã nói.

---

## 3. Phân công thuyết trình (Presentation Roles)

| Thành viên | Phần đảm nhiệm |
| :--- | :--- |
| **Nhung (Lead)** | Trình bày Persona, Intent và Chiến lược Trusted Source (File 00). |
| **Bình** | Phân tích 3 Knobs và Logic tính toán chi phí Hybrid Model (File 01, 02, 03). |
| **Hùng** | Trình bày kịch bản High Season, ROI và Khuyến nghị cuối cùng (File 04, 05). |

---
**FINAL CHECKPOINT: DONE**
*Hồ sơ đã sẵn sàng để trình bày trước ban giám khảo.*
