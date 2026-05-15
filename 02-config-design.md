# Day 27 — Workshop 2: Configuration Design

**Nhóm:** Nguyễn Thị Cẩm Nhung - 2A202600208
          Nguyễn Thanh Bình 2A202600484 
          Nguyễn Hoàng Việt Hùng - 2A202600164 
**Sản phẩm:** AI Travel Assistant

---

## 1. Detailed Combo Configuration (Chi tiết cấu hình các gói)

Dựa trên 3 chiến lược đã chọn, nhóm xác định các tham số kỹ thuật cụ thể:

### Combo A: "The Budget Saver" (Giá rẻ nhất)
| Knob | Cấu hình cụ thể | Lý do kinh tế |
| :--- | :--- | :--- |
| **Model Tier** | **GPT-4o-mini** | Chi phí cực thấp: **$0.15 (In) / $0.60 (Out)** per 1M tokens [1]. |
| **Web Search** | **OFF** | Chỉ dùng RAG nội bộ. Tiết kiệm $0.008 cho mỗi câu hỏi. |
| **History** | **Last 2 Turns** | Giữ context tối thiểu; Giả định 130 tokens history/turn [2]. |

### Combo B: "The Premium Guardian" (Chất lượng tối đa)
| Knob | Cấu hình cụ thể | Lý do kinh tế |
| :--- | :--- | :--- |
| **Model Tier** | **Claude 3.5 Sonnet** | Tier Strong: **$3.00 (In) / $15.00 (Out)** per 1M tokens [3]. |
| **Web Search** | **Smart Search + Caching** | Đảm bảo thông tin luôn mới nhất. Dùng Cache để tối ưu phí API. |
| **History** | **Full Session** | Nhớ toàn bộ thông tin John/Yuki đã nói để cá nhân hóa 100%. |

### Combo C: "The Smart PM Mix" (Đề xuất của nhóm)
| Knob | Cấu hình cụ thể | Lý do kinh tế |
| :--- | :--- | :--- |
| **Model Tier** | **Hybrid (80/20 per-session)** | Định tuyến toàn bộ session dựa trên Intent khởi đầu (80% Mini / 20% Strong). |
| **Web Search** | **Selective + Caching** | Chỉ gọi Search cho intent Real-time. Giả định **60% Cache Hit rate** [4]. |
| **History** | **Last 3 Turns** | Cân bằng UX/Cost dựa trên phân tích N=30 sessions [2]. |

---

## 2. Technical Usage Estimates (Ước tính thông số kỹ thuật)

Dựa trên hành trình của 3 khách hàng ở file 00, nhóm đưa ra các giả định con số để tính toán:

### 2.1 Token Usage per Turn (Trọng số Token)

**Ghi chú:** 1,000 tokens ≈ 750 từ tiếng Anh. Sử dụng tokenizer: `cl100k_base` (tiktoken).

| Turn Number | Input Tokens (Avg) | Output Tokens (Avg) | Ghi chú |
| :--- | :---: | :---: | :--- |
| **Turn 1** | 1,830 | 180 | Log mẫu RAG-01: Sys(500) + User(80) + RAG(1250). |
| **Turn 2** | 2,090 | 180 | History buildup: 260 ±45 tokens (N=30 on 2026-05-15) [2]. |
| **Turn 3** | 2,350 | 180 | Trung bình tích lũy lịch sử 2 lượt trước. |
| **Turn 4** | 2,610 | 180 | Trung bình tích lũy lịch sử 3 lượt trước. |
| **Turn 5** | 2,870 | 180 | Max context window (Full session) cho phép. |
| --- | --- | --- | --- |
| **Average** | **2,350** | **180** | Giá trị trung bình 5 lượt (Input/Output tính riêng). |

### 2.2 Intent-based API Calls (Tần suất gọi dịch vụ ngoài)
| Dịch vụ | Tần suất áp dụng | Chi phí đơn vị |
| :--- | :--- | :--- |
| **Search API (Tavily)** | 10% (Effective) | $0.008 / call | Tavily Pricing (Scale Plan) [5]. |
| **Human Handover** | 4% (Targeted) | $0.500 / conv | Internal HR Cost Sheet (AHT=5m, Rate=$5/hr) [6]. |

---

## 3. Data Sources & Validation (Chứng thực nguồn)

**[1] OpenAI Pricing Page:** Tra cứu ngày 15/05/2026. GPT-4o-mini (Input: $0.15/1M, Output: $0.60/1M).

**[2] Token History Audit:** Đo lường trên file `sample_conversations.csv` (N=30 sessions). Sử dụng script `tools/token_counter.py`. Kết quả: độ lệch chuẩn (stddev) +/- 45 tokens.

**[3] Anthropic Pricing Page:** Claude 3.5 Sonnet (Input: $3.00/1M, Output: $15.00/1M).

**[4] Cache Hit Assumption:** Giả định 60% dựa trên PoC giai đoạn 1 cho các câu hỏi phổ thông về Visa và Thời tiết.

**[5] Tavily API Docs:** tavily.com/pricing - Gói Scale trả phí theo lượt gọi thực tế.

**[6] Human Support Breakdown:** 
`Cost = (Avg Handle Time * Hourly Rate) * (1 + Overhead %)`. 
Giả định: AHT=5 phút; Rate=$5.00/giờ; Overhead=20% -> `$0.50 per session`.

---

## 4. Sensitivity Scenarios (Phân tích kịch bản)

| Case | Biến số thay đổi | Dự phóng Cost/Conv | Trạng thái | Hành động đề xuất |
| :--- | :--- | :---: | :--- | :--- |
| **Optimistic** | Cache Hit 80%, Handover 2% | **$0.019** | Rất an toàn | Duy trì chất lượng RAG để giữ chân người dùng. |
| **Base** | Theo thiết kế hiện tại | **$0.032** | Hơi vượt mục tiêu | Tối ưu lại System Prompt để giảm input tokens. |
| **Pessimistic** | Cache Hit 20%, Handover 10% | **$0.065** | Rủi ro lỗ | Siết chặt bộ lọc Intent, chuyển sang Model rẻ hơn. |

---

## 3. Economics Summary (Mục tiêu kinh tế)

Nhóm đặt mục tiêu cho Combo C (Smart Mix):
1. **Cost per Conversation ($/conv):** Mục tiêu < $0.03 (Rẻ hơn 15 lần so với người thật).
2. **Gross Margin:** Dự kiến đạt 85% dựa trên giá trị tiết kiệm được cho hãng.
3. **Break-even point:** Sau 10,000 cuộc hội thoại đầu tiên.

---
**Checkpoint 2: DONE**
*Sẵn sàng chuyển sang file 03-config-math.md để thực hiện phép tính thực tế.*