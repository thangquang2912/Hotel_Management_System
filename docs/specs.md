# HotelMind — PA#1 Proposal and Planning

**Môn:** CSC13114  
**Nhóm:** Trà Nguyễn Quang Thắng (23120353) — RAG/evaluation; Nguyễn Anh Tuấn (23120394) — backend/data; Phạm Quang Tuấn (23120396) — frontend/UX  
**Repository:** https://github.com/thangquang2912/Hotel_Management_System

## Giới thiệu ngắn

HotelMind là trợ lý đặt phòng AI trên website dành cho khách sạn mini, giúp khách hỏi chính sách, xem giá và phòng trống ngay cả khi lễ tân đang bận. Hệ thống tra cứu tài liệu khách sạn để trả lời kèm nguồn, lấy giá và tình trạng phòng từ API, đồng thời tạo booking nháp để lễ tân duyệt. Khi thiếu dữ liệu hoặc có thông tin mâu thuẫn, trợ lý chuyển câu hỏi cho lễ tân thay vì tự xác nhận đặt phòng.

## 1. Problem and users

**Người dùng mục tiêu:** chị Lan, lễ tân ca đêm (22:00–06:00) làm việc một mình tại một khách sạn mini 30 phòng ở TP.HCM.

**Vấn đề:** khi chị Lan đang check-in cho khách đến muộn, chị không thể đồng thời trả lời nhanh các câu hỏi trên website về giá, phòng trống và chính sách hủy; khách phải chờ và có thể đặt nơi khác.

Hiện nay, chị Lan trả lời thủ công trên điện thoại, tra giá và phòng trống trong Excel, tra chính sách trong Word rồi ghi yêu cầu đặt phòng lại vào Excel. Trong tuần đầu, nhóm sẽ phỏng vấn một lễ tân khách sạn mini để kiểm tra quy trình và giả định này; nếu không tiếp cận được, nhóm sẽ ghi rõ đây là persona giả định và kiểm tra bằng walkthrough với giảng viên.

## 2. The LLM feature and the cost of being wrong

**Một tính năng LLM cốt lõi:** trợ lý đặt phòng có căn cứ trên website. Trong một cuộc hội thoại, trợ lý trả lời chính sách từ tài liệu khách sạn, gọi API nội bộ để lấy giá/phòng trống và có thể tạo **booking nháp** để lễ tân duyệt. Mỗi câu trả lời về chính sách phải kèm đoạn nguồn; thiếu nguồn hoặc dữ liệu mâu thuẫn thì chuyển cho lễ tân. LLM không tự tính giá, tự suy đoán phòng trống hay xác nhận booking.

| Sai ở đâu | Ai chịu ảnh hưởng và chi phí | Khả năng khắc phục |
|---|---|---|
| Nói sai “được hủy miễn phí” | Khách có thể mất cọc; khách sạn có thể phải hoàn/giảm **0,8–1,5 triệu VND** cho một đêm và chịu review xấu | Khó đảo ngược sau khi khách đã quyết định dựa trên câu trả lời |
| Báo sai giá hoặc phòng trống | Khách mất thời gian; lễ tân phải xử lý khiếu nại và khách sạn có thể mất tối đa **1,5 triệu VND** doanh thu một đêm | Có thể chặn trước khi phát sinh giao dịch vì booking chỉ là bản nháp |

Hệ thống lưu câu hỏi, câu trả lời, citation, tool call và quyết định handoff. Nhóm xây bộ 50 câu hỏi có đáp án chuẩn; trước demo, **100% câu chính sách phải có citation đúng**, **100% câu giá/phòng phải dùng tool**, và **không có câu sai nghiêm trọng** trong bộ eval. Lễ tân có nút đánh dấu câu trả lời sai để nhóm phát hiện lỗi ngoài bộ eval.

## 3. Scope: in and out

**Trong học kỳ:** dữ liệu tối thiểu cho 10 phòng/3 loại phòng, bảng giá và booking nháp; tài khoản lễ tân; nhập tài liệu chính sách; tìm kiếm RAG; chat widget Việt–Anh; tool kiểm tra giá/phòng trống và tạo booking nháp; handoff; màn hình duyệt booking; log và bộ eval 50 câu.

**Cố ý không làm:** booking tự xác nhận; thanh toán; dynamic pricing; giao diện lịch kéo-thả; nghiệp vụ buồng phòng/F&B/hóa đơn; channel manager và đa khách sạn; Zalo/Messenger/voice/OCR; AI agent cho nhân viên; MCP, Text-to-SQL và hạ tầng Kubernetes/Celery/Redis.

## 4. Plan and ownership

| Checkpoint | Hạn | Kết quả kiểm tra được | Owner |
|---|---:|---|---|
| CP1 | 20/10/2026 | Persona/interview, luồng chính, repo/CI, schema và 20 câu eval đầu tiên | Trà Nguyễn Quang Thắng |
| CP2 | 03/11/2026 | Dữ liệu phòng/giá, booking nháp, đăng nhập lễ tân và test chống double-booking | Nguyễn Anh Tuấn |
| CP3 | 17/11/2026 | Nhập tài liệu, retrieval và câu trả lời chính sách có citation | Trà Nguyễn Quang Thắng |
| CP4 | 01/12/2026 | Tool giá/phòng/booking kết nối với chat widget và màn hình duyệt | Phạm Quang Tuấn |
| CP5 | 15/12/2026 | Handoff, audit log, 50 câu eval và sửa các lỗi nghiêm trọng | Trà Nguyễn Quang Thắng |
| CP6 | 29/12/2026 | Docker Compose, demo end-to-end, báo cáo kết quả và chi phí | Nguyễn Anh Tuấn |

Owner chịu trách nhiệm giao kết quả; hai thành viên còn lại review và hỗ trợ tích hợp. Nhóm sẽ đổi ngày ngay khi lịch checkpoint chính thức của môn học khác các mốc trên.

## 5. Risks

1. **Câu trả lời sai nhưng nghe thuyết phục làm mất niềm tin và khiến feature không thể demo.** Ngay trong CP1, Thắng viết 20 câu eval; nhóm bắt buộc citation cho policy, bắt buộc tool cho dữ liệu giao dịch và handoff khi không có nguồn. Nếu còn bất kỳ lỗi nghiêm trọng ở CP5, demo chỉ cho phép trả lời nhóm câu đã đạt eval.
2. **Phần quản lý khách sạn trễ khiến LLM không có dữ liệu thật để gọi.** Ngay tuần đầu, Tuấn tạo seed 10 phòng, 3 loại phòng và API contract giả. Nếu CP2 trễ quá 7 ngày, nhóm bỏ CRUD quản trị đầy đủ, nhập seed trực tiếp và giữ API đọc/tạo booking nháp cần cho feature LLM.

## 6. Technology choices

| Hạng mục | Lựa chọn và lý do |
|---|---|
| Backend | **Python 3.12 + FastAPI + SQLAlchemy/Alembic:** cùng hệ sinh thái với RAG, có API typed và streaming, giảm số công nghệ nhóm phải học. |
| Database | **PostgreSQL 16 + pgvector:** lưu booking và vector trong một DB; exclusion constraint giúp chống double-booking. |
| LLM | **Gemini API, `gemini-3.5-flash-lite`:** hỗ trợ function calling với độ trễ/chi phí thấp cho chat và tool use. Free tier: **$0** trong quota; paid Standard: **$0.30/1M input tokens, $2.50/1M output tokens** (giá kiểm tra ngày 07/10/2026). Nhóm không bật billing và đặt ngân sách học kỳ **$0**. |
| Embedding | **Gemini `gemini-embedding-2`, 768 chiều:** một embedding đa ngôn ngữ cho tài liệu Việt–Anh; free tier $0, paid Standard **$0.20/1M text tokens**. |
| Frontend | **React + Vite + TypeScript + Tailwind:** nhóm đã quen và có thể dùng chung component cho chat widget và màn hình lễ tân. |
| Testing/evaluation | **pytest + Testcontainers + FakeLLM:** kiểm tra PostgreSQL và tool contract thật nhưng không tốn token hoặc làm test phụ thuộc API. |
| Deployment | **Docker Compose + GitHub Actions:** chạy toàn bộ demo bằng một lệnh và kiểm tra lint/test trên mỗi pull request. |

