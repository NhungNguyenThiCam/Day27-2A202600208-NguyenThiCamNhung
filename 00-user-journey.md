# Day 27 — Workshop 0: User Journey Simulation

**Nhóm:** Nguyễn Thị Cẩm Nhung - 2A202600208 
          Nguyễn Thanh Bình - 2A202600484 
          Nguyễn Hoàng Việt Hùng - 2A202600164 
**Sản phẩm:** AI Travel Assistant (Vietnam Airlines/Tourism Focus)

---

## 1. Persona Setup (Thân phận khách hàng)

| Đặc điểm | Tourist #1 (John - Mỹ) | Tourist #2 (Yuki - Nhật) | Tourist #3 (Min-ho - Hàn) |
| :--- | :--- | :--- | :--- |
| **Quốc tịch** | Mỹ | Nhật Bản | Hàn Quốc |
| **Mục đích & Style**| Solo budget backpacker | Premium Anniversary | Tech-savvy Business/Golf |
| **Tech Literacy**   | Medium | Low | High |
| **Emotional Trigger**| Fear of scams (Lo bị lừa) | Anxiety about hygiene (Lo vệ sinh) | Stress during emergency (Áp lực) |
| **Ngân sách**       | Medium | High | High |
| **Success Metric**  | Thông tin Visa < 2 phút | An tâm đặt dịch vụ cao cấp | Giải quyết sự cố < 10 phút |

---

## 2. Chat Session Simulation (Mô phỏng hội thoại)

### Tourist #1: John (USA) - State: Cautious | Style: Budget
1. "Do US citizens need a visa for a 15-day trip to Vietnam?" -> **[Informational][Compliance Risk]**
2. "What are the latest safety tips for solo travelers in Ho Chi Minh City?" -> **[Informational][Information Freshness Risk]**
3. "How can I apply for an e-visa online officially?" -> **[Informational][Compliance Risk]**
4. "Can I pay with US dollars in local markets?" -> **[Informational][Information Freshness Risk]**
5. "Is there a direct flight from San Francisco to Hanoi?" -> **[Navigation][Information Freshness Risk]**

### Tourist #2: Yuki (Japan) - State: Anxious | Style: Premium
1. "Which street foods are safe for sensitive stomachs in Hanoi?" -> **[Recommendation][User Harm Risk: Low]**
2. "Are there any 5-star Japanese-speaking guides in Da Nang?" -> **[Transactional][Human Handover Priority]**
3. "Can you recommend a luxury cruise in Ha Long Bay for a couple?" -> **[Recommendation][Information Freshness Risk]**
4. "What is the best way to travel from the airport safely at night?" -> **[Navigation][User Harm Risk: Low]**
5. "I want to book a private car for 3 days, can you help me?" -> **[Transactional][Human Handover]**

### Tourist #3: Min-ho (Korea) - State: Urgent | Style: Tech-Business
1. "Where can I buy a reliable 5G eSIM at Da Nang airport?" -> **[Real-time][Information Freshness Risk]**
2. "What is the weather forecast for golf courses in Da Lat?" -> **[Real-time][Information Freshness Risk]**
3. "Is there a Korean hospital or clinic near Hoan Kiem lake?" -> **[Emergency][User Harm Risk: High]**
4. "Are golf clubs available for rent at most resorts?" -> **[Informational][Information Freshness Risk]**
5. "I lost my passport, what is the contact for the Embassy?" -> **[Emergency][User Harm Risk: High -> Human]**

---

## 3. Bảng thống kê ý định (Intent Statistics)

| Intent Type | Routing Policy | Search Need | Trusted Source | Count |
| :--- | :--- | :--- | :--- | :---: |
| **Informational** | AI Only | Low | Government Docs / VNA Policy | 5 |
| **Recommendation** | AI Assisted | Optional | Verified Travel Partners | 2 |
| **Real-time** | AI + API | Mandatory | Official Weather/Airport APIs | 2 |
| **Navigation** | AI Only | Medium | Official Schedules | 2 |
| **Transactional** | AI -> Human | Mandatory | Internal CRM / Staff | 2 |
| **Emergency** | Human Priority | Mandatory | Verified Healthcare/Embassy DB | 2 |
| **Total** | | | | **15** |

---

## 4. Kết luận cho bài toán Kinh tế (Economics Insight)

Dựa trên mô phỏng trên, nhóm rút ra các chỉ số cơ sở để tính toán ở file sau:

1. **Token Density:** Session trung bình **5 turns**. Các turn về Emergency/Transactional tốn nhiều context hơn do cần nạp dữ liệu từ Trusted Sources.
2. **Search API Frequency:** ~40-50% yêu cầu dữ liệu thời gian thực (Web Search).
3. **Escalation Rate:** ~25% cần con người. Chi phí hỗ trợ ước tính (**Estimated assisted-support cost: $0.50/conv**) tập trung vào các intent Emergency và Premium Transactional.
4. **Strategic Observation:** High-risk intents (Emergency/Transactional) tuy có tần suất thấp nhưng chi phí sai lệch/vận hành rất cao -> Cần cơ chế điều hướng nghiêm ngặt (Strict Routing) thay vì tự động hóa hoàn toàn.

---

## 5. Failure Simulation & Mitigation

| Tình huống | Lỗi AI tiềm ẩn | Chiến lược khắc phục |
| :--- | :--- | :--- |
| Chính sách Visa | Quy định cũ (Outdated) | Bắt buộc Web Search + Link dẫn nguồn |
| Y tế/Khẩn cấp | Địa chỉ sai/Ảo giác | Hard-coded danh sách liên hệ (Không để AI tự sinh) |
| Đặt dịch vụ | Hứa hẹn sai thẩm quyền | Tự động ngắt luồng (Escalation) khi thấy từ khóa "Book" |

---

## 6. System Design Implication

| Observation | Product/System Impact |
| :--- | :--- |
| 47% queries require live data | Cần Search Orchestration Layer (Tavily/Perplexity API) |
| 20-30% escalation rate | Cần tích hợp CRM + Hệ thống định tuyến nhân viên (Agent Routing) |
| Emergency intents exist | Ưu tiên định tuyến ngay khi thấy từ khóa "hộ chiếu", "bệnh viện" |
| Personalized Economics | User memory giúp giảm việc tra cứu lại ngữ cảnh, tối ưu token |
| **System Component** | **Mục đích** |
| Lightweight Memory Layer | Ghi nhớ sở thích (Premium vs Budget) & giảm trùng lặp context |

---
**Checkpoint 0: DONE**
*Sẵn sàng chuyển sang file 01-base-flow.md để thiết kế cấu hình tối ưu chi phí.*
