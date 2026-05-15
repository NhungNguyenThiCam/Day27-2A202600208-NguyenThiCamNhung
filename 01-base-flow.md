# Day 27 — Workshop 1: Base Flow & Technical Knobs

**Nhóm:** Nguyễn Thị Cẩm Nhung - 2A202600208
          Nguyễn Thanh Bình 2A202600484 
          Nguyễn Hoàng Việt Hùng - 2A202600164 
**Sản phẩm:** AI Travel Assistant

---

## 1. The Base Flow (Luồng vận hành cơ sở)

Mô tả cách một tin nhắn của John, Yuki hay Min-ho đi qua hệ thống:

```text
[User Input] 
      │
      ▼
[Intent Classifier] ──────────┐
      │                       │
      ▼                       ▼
<Branching Logic>       [High Risk / Transactional] ──► [Human Handover] ($0.50/conv)
      │
      ▼
[Context Assembly]
      ├─ Memory Layer (Sở thích khách hàng)
      ├─ RAG (Chính sách VNA nội bộ)
      └─ Web Search (Nếu Real-time/Freshness req)
      │
      ▼
[LLM Generation] (Model Tier selection)
      │
      ▼
[Response + Citation]
```

---

## 2. The 3 Knobs (3 Nút vặn kinh tế)

Nhóm thống nhất các mức độ cho từng "nút vặn" để điều chỉnh chi phí:

| Knob | Cấp độ Thấp (Rẻ) | Cấp độ Cao (Đắt) |
| :--- | :--- | :--- |
| **1. Model Tier** | **Flash/Mini**: GPT-4o-mini hoặc Gemini 2.5 Flash. Tốc độ nhanh, cực rẻ. | **Strong/Pro**: GPT-4o hoặc Gemini 2.5 Pro. Thông minh, hiểu luật (Compliance) tốt. |
| **2. Web Search** | **Off/Optional**: Chỉ gọi API Search khi AI không tìm thấy trong RAG. | **Smart Search + Caching**: Luôn kiểm tra Cache trước; chỉ gọi API Search nếu Cache miss để đảm bảo độ tươi mới mà vẫn tối ưu chi phí. |
| **3. Context Window** | **Short**: Chỉ nhớ 2 lượt chat gần nhất. | **Full**: Nhớ toàn bộ session (5-7 lượt). |

---

## 3. Combo Design (Thiết kế các gói cấu hình)

Nhóm thiết kế 3 phương án đánh đổi giữa **Chi phí** và **Chất lượng**:

### Combo A: "The Budget Saver" (Ưu tiên tiết kiệm)
*Mục tiêu: Phục vụ khách vãng lai hỏi thông tin cơ bản (John style).*
- **Model:** 100% Flash/Mini.
- **Search:** Tắt mặc định, chỉ dùng RAG dữ liệu tĩnh.
- **History:** 2 lượt gần nhất.
- **Kỳ vọng:** Chi phí cực thấp, nhưng có thể bị lỗi "Freshness" nếu chính sách Visa đổi đột ngột.

### Combo B: "The Premium Guardian" (Ưu tiên an toàn)
*Mục tiêu: Phục vụ khách VIP, yêu cầu độ chính xác tuyệt đối (Yuki style).*
- **Model:** 100% Strong/Pro.
- **Search:** Smart Search + Caching (Ưu tiên cache, gọi API cho dữ liệu mới nhất từ nguồn Chính phủ/VNA).
- **History:** Toàn bộ session.
- **Kỳ vọng:** Chất lượng cao nhất, cực kỳ an toàn cho thương hiệu nhưng chi phí API sẽ rất "rát".

### Combo C: "The Smart PM Mix" (Tối ưu hóa bởi PM)
*Mục tiêu: Cân bằng kinh tế (Cấu hình nhóm đề xuất).*
- **Classifier:** Dùng model Flash để phân loại Intent (Rẻ).
- **Execution:** 
    - **Informational/Real-time:** Dùng model Flash + Search chỉ khi cần.
    - **Compliance/Emergency:** Dùng model Pro + Smart Search + Caching + Human Handover.
- **History:** 3 lượt gần nhất + Memory Layer (chỉ lưu key preferences như "sensitive stomach").
- **Kỳ vọng:** Giảm 60% chi phí so với bản Premium nhưng vẫn giữ được 95% độ chính xác ở các task quan trọng.

---

## 4. Economic Assumptions (Giả định kinh tế)

Để chuẩn bị cho file tính toán (03-config-math), nhóm đưa ra các giả định:
1. **Turn count:** Trung bình 5 turns/session.
2. **Search API:** Giá $0.008 mỗi lượt gọi thành công (Tavily).
3. **Human handover:** Chi phí cơ hội/lương nhân viên là $0.50 mỗi cuộc hội thoại chuyển sang.
4. **Token ratio:** Lượt chat thứ 5 sẽ tốn gấp 3 lần lượt chat thứ 1 do tích lũy history.

---
**Checkpoint 1: DONE**
*Sẵn sàng chuyển sang file 02-config-design.md để chi tiết hóa các tham số tính tiền.*