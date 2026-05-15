# Day 27 — Workshop 3: Unit Economics Calculation

**Nhóm:** Nguyễn Thị Cẩm Nhung, Nguyễn Thanh Bình, Nguyễn Hoàng Việt Hùng
**Sản phẩm:** AI Travel Assistant
**Cấu hình trọng tâm:** Combo C (Smart PM Mix)

---

## 1. Cost Parameters (Bảng giá tham chiếu)

| Thành phần | Đơn giá (Unit Cost) | Ghi chú |
| :--- | :--- | :--- |
| **Mini Tier (Input)** | $0.15 / 1M tokens | GPT-4o-mini |
| **Mini Tier (Output)** | $0.60 / 1M tokens | GPT-4o-mini |
| **Strong Tier (Input)** | $3.00 / 1M tokens | Claude Sonnet 4.6 |
| **Strong Tier (Output)** | $15.00 / 1M tokens | Claude Sonnet 4.6 |
| **Search API** | $0.008 / call | Tavily API |
| **Human Handover** | $0.500 / conv | Chi phí nhân sự hỗ trợ |

---

## 2. Token Usage per Session (Ước tính Tokens)

Giả định session trung bình **5 lượt (turns)**:
- **Total Input:** 2,350 tokens/turn × 5 = **11,750 tokens**
- **Total Output:** 180 tokens/turn × 5 = **900 tokens**

---

## 3. Component Costing (Tính toán chi tiết cho Combo C)

### 3.1 Expected Model Cost (Chi phí mô hình trọng số)
*Áp dụng tỷ lệ Hybrid: 80/20 per-session split (Xác định tại Turn 1)*

1. **Cost if all Mini:** (0.01175 × $0.15) + (0.0009 × $0.60) = **$0.00230**
2. **Cost if all Strong:** (0.01175 × $3.00) + (0.0009 × $15.00) = **$0.04875**
3. **Weighted Average:** (0.8 × 0.00230) + (0.2 × 0.04875) = **$0.01159**

### 3.2 Expected Search Cost (Chi phí tìm kiếm)
*Công thức: Effective_Calls = IntentRate (47%) * (1 - CacheHit 60%) * SelectiveFactor*
- **Cost:** 0.1 × $0.008 = **$0.00080**

### 3.3 Expected Human Cost (Chi phí nhân sự)
*Tỷ lệ chuyển người thật: 4%*
- **Cost:** 0.04 × $0.50 = **$0.02000**

---

## 4. Total Unit Economics Summary

### 3.2 Cấu trúc chi phí (Cost Structure Analysis)

| Thành phần | Chi phí / Hội thoại | Tỷ trọng (%) | Ghi chú |
| :--- | :---: | :---: | :--- |
| **Model (LLM)** | $0.01159 | 35.8% | GPT-4o-mini & RAG Input |
| **Search API** | $0.00080 | 2.5% | Tần suất 0.4 search/conv |
| **Human Handover** | **$0.02000** | **61.7%** | Lương NV trực + Ops |
| **TỔNG CỘNG** | **$0.03239** | **100%** | |

---

> **Kết luận:** Hiện tại mức phí **$0.032** đang cao hơn mục tiêu **$0.030** (~8%).
> 
> **Điểm nghẽn (Bottleneck):** Chi phí nhân sự hỗ trợ (**Human Handover**) chiếm hơn **60%** tổng chi phí vận hành. Dù chỉ có 20-25% số ca chuyển cho người thật, nhưng giá trị tuyệt đối của một ca hỗ trợ người đóng góp rất lớn vào tổng chi phí trung bình.

---

## 5. Optimization Roadmap (Kế hoạch tối ưu để đạt < $0.03)

Để đưa chi phí về ngưỡng an toàn, nhóm đề xuất 2 phương án điều chỉnh cấu hình (áp dụng độc lập):

### Phương án 1: Thắt chặt điều hướng (Strict Routing)
- Hành động: Giảm tỷ lệ Human Handover từ **4% → 2.5%**.
- Phép tính: $0.01159 (Model) + $0.0008 (Search) + $0.0125 (Human: 2.5% * $0.5)
- **Kết quả: $0.02489/conv** (Đạt mục tiêu).

### Phương án 2: Tối ưu Model Mix (Aggressive Hybrid)
- Hành động: Chuyển dịch tỷ lệ dùng Strong tier từ **20% → 10%**.
- Phép tính: $0.00695 (Weighted Model: 90/10 split) + $0.0008 (Search) + $0.02 (Human: 4%)
- **Kết quả: $0.02775/conv** (Đạt mục tiêu).

---

## 6. Nguồn dữ liệu & Assumptions (Data Integrity)

### 5. Nguồn dữ liệu & Giả định (Data Sources & Assumptions)

| Hạng mục | Nguồn / Tài liệu tham chiếu | Trạng thái |
| :--- | :--- | :---: |
| **Model Pricing** | [OpenAI & Anthropic Pricing Pages - 2026-05-15] | Verified |
| **Token Measurement** | `sample_conversations.csv` (N=30), script `token_counter.py` | Verified |
| **RAG Context** | `vietnamairlines_public_policies.json` (Avg chunk = 250 tokens) | Verified |
| **Search & Cache** | Công thức: `Effective_Rate (10%) = Global_Intent (47%) * (1 - Cache_Hit 60%) * Selective_Filter (0.53)` | **Assumption** |
| **Human Handover** | `support_cost_calc.xlsx` (AHT 5m, Rate $5/hr) | Estimated |
| **Split Logic** | Áp dụng 80/20 theo tổng lưu lượng truy cập (Per-session) | Verified |
---
**Checkpoint 3: DONE**
*Sẵn sàng chuyển sang file 04-scenario-testing.md để Stress-test với quy mô khách hàng lớn.*
