# Day 27 — Workshop 4: Scenario Testing & Comparison

**Nhóm:** Nguyễn Thị Cẩm Nhung, Nguyễn Thanh Bình, Nguyễn Hoàng Việt Hùng
**Sản phẩm:** AI Travel Assistant

---

## 1. Volume Scenarios (Thử nghiệm quy mô)

Nhóm áp dụng chi phí Combo C (Smart Mix) **$0.032/conv** vào các kịch bản lưu lượng khách hàng khác nhau:

| Hạng mục                  | Scenario A: Low Season | Scenario B: High Season | Ghi chú                            |
| :------------------------ | :--------------------: | :---------------------: | :--------------------------------- |
| **Monthly Conversations** | 10,000                 | 50,000                  | Giả định mùa thấp điểm vs Tết      |
| **Total AI Cost**         | **$323.90**            | **$1,619.50**           | Bao gồm: LLM + Search + Human escalation residual |
| **Total Human Cost**      | **$5,000.00**          | **$25,000.00**          | Baseline: $0.50/conv (100% người)  |
| **Monthly Savings ($)**   | **$4,676.10**          | **$23,380.50**          | Số tiền tiết kiệm được thực tế     |
| **Efficiency Gain**       | **15.4x**              | **15.4x**               | AI rẻ hơn người gấp ~15.4 lần      |

---

## 2. Optimization Comparison (So sánh các kịch bản tối ưu)

Dựa trên Workshop 03, nhóm so sánh khả năng đạt mục tiêu budget **$0.030/conv**:

| Kịch bản                  | Cost / Conv | Target Met? | Đánh đổi (Trade-off)                                      |
| :------------------------ | :---------: | :---------: | :-------------------------------------------------------- |
| **Base (Combo C)**        | $0.03239    | No          | Chất lượng tốt, nhưng Human Handover chiếm 61.7% tổng chi phí vận hành. |
| **Opt 1: Strict Routing** | **$0.02489**| **YES**     | Giảm Handover xuống 2.5%. Tiết kiệm nhất, tối ưu lợi nhuận.|
| **Opt 2: Aggressive Mix** | **$0.02775**| **YES**     | 90% dùng Mini Tier. Phù hợp cho tập khách hàng Budget.     |

---

## 3. Key Economic Insights (Quan sát kinh tế)

1. **Lợi thế quy mô (Scalability):** Khi khách tăng gấp 5 lần (từ 10k lên 50k), chi phí nhân sự truyền thống sẽ tăng tuyến tính và cần tuyển thêm người ngay lập tức. Với AI, chi phí chỉ tăng theo lượng dùng API, không tốn thêm chi phí quản lý hay hạ tầng nhân sự.
2. **Điểm đòn bẩy (The Human Lever):** Mỗi 1% giảm được ở tỷ lệ Human Handover giúp tiết kiệm $0.005/conv. Đây là "nút vặn" có tác động tài chính mạnh nhất, mạnh hơn cả việc tối ưu token model.
3. **Biên lợi nhuận gộp (Gross Margin):** Nếu bán dịch vụ này cho Vietnam Airlines với giá $0.10/session, biên lợi nhuận đạt **~68%**. Nếu tối ưu theo Opt 1, biên lợi nhuận tăng lên **~75%**.
4. **Unit Economics vs Human:** Ngay cả ở kịch bản tệ nhất (Pessimistic - $0.065), AI vẫn rẻ hơn con người ($0.50) gấp gần 8 lần.

---
**Checkpoint 4: DONE**
*Toàn bộ số liệu đã được đồng bộ hóa với File 03. Sẵn sàng cho phần Khuyến nghị.*
