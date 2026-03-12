# PRD — GRAD LMS

## 1. Problem Statement

Các đơn vị đào tạo online (cá nhân, tổ chức, marketplace nhiều giảng viên) thường phải dùng nhiều công cụ rời rạc cho tạo khóa học, bán hàng, live class, quản lý học viên, thanh toán và vận hành. Điều này làm tăng chi phí vận hành, khó mở rộng và tạo trải nghiệm học tập không liền mạch.

GRAD LMS giải quyết bài toán này bằng một nền tảng LMS hợp nhất, cho phép quản trị toàn bộ vòng đời đào tạo và kinh doanh khóa học trên một hệ thống.

## 2. Business Context

- Sản phẩm: GRAD LMS (learning management system cho online education/training/course marketplace).
- Phân khúc chính: giảng viên cá nhân, tổ chức đào tạo, mô hình multi-instructor marketplace.
- Giá trị kinh doanh: gom vận hành học tập + thương mại (subscriptions, sales, payout, affiliate) vào một hệ thống duy nhất để tăng tốc go-to-market.
- Môi trường triển khai mục tiêu: shared hosting/VPS tương thích Laravel, có thể nâng cấp dần khi tăng tải.

## 3. Goals

- Cung cấp khả năng tạo và quản lý khóa học không giới hạn (video/tập trung/live).
- Hỗ trợ học tập đầy đủ: kiểm tra, bài tập, chứng chỉ, review, học trực tuyến.
- Hỗ trợ vận hành kinh doanh đào tạo: subscriptions, sales, discount codes, payout, affiliate.
- Hỗ trợ đa vai trò người dùng: admin, instructor, student, organization.
- Dễ mở rộng qua combo, mobile app.

## 4. Non-Goals (Out of Scope)

- Không xây mới LMS từ đầu trong tài liệu này; đây là PRD cho triển khai/vận hành GRAD LMS.
- Không bao gồm custom feature sâu ngoài khả năng hiện có của GRAD LMS core.
- Không bao gồm migration dữ liệu phức tạp từ hệ thống legacy (nếu có sẽ có project riêng).
- Không cam kết auto payout; quy trình payout hiện tại là xác nhận và chi trả thủ công bởi admin/staff.

## 5. User Stories

### Admin

- Là admin, tôi muốn cấu hình hệ thống (general, financial, payment gateways, SMTP, security, SEO) để nền tảng vận hành ổn định.
- Là admin, tôi muốn duyệt lớp học và quản lý nội dung để đảm bảo chất lượng học liệu.
- Là admin, tôi muốn xử lý payout request của giảng viên để kiểm soát rủi ro tài chính.
- Là admin, tôi muốn quản lý user roles/groups để áp dụng commission/discount linh hoạt.

### Instructor

- Là giảng viên, tôi muốn tạo và xuất bản khóa học (video/text/live) để bán nội dung của mình.
- Là giảng viên, tôi muốn nhận đánh giá học viên và theo dõi doanh thu để tối ưu khóa học.
- Là giảng viên, tôi muốn gửi yêu cầu rút tiền khi đủ ngưỡng tối thiểu để nhận thu nhập.

### Student

- Là học viên, tôi muốn tìm kiếm/đăng ký khóa học và học mượt trên web/mobile.
- Là học viên, tôi muốn làm quiz/assignment và nhận certificate để xác nhận kết quả học tập.
- Là học viên, tôi muốn nhận thông báo/noticeboard để theo dõi lịch học và cập nhật quan trọng.

### Organization

- Là tổ chức, tôi muốn quản lý người dùng tổ chức và lớp học nội bộ để triển khai đào tạo quy mô lớn.
- Là tổ chức, tôi muốn dùng phân quyền và báo cáo để kiểm soát hiệu quả đào tạo.

## 6. Acceptance Criteria

### Core Platform

- Có thể tạo, chỉnh sửa, publish/unpublish khóa học từ admin/instructor panel.
- Học viên có thể mua/đăng ký và truy cập nội dung khóa học thành công.
- Quiz/certificate hoạt động và lưu được kết quả theo user.

### Commerce & Finance

- Discount code áp dụng đúng theo rule.
- Sales list và balances hiển thị được dữ liệu giao dịch.
- Instructor gửi payout request được khi số dư >= minimum payout amount.
- Admin/staff có thể duyệt/từ chối payout request; sau khi duyệt, trạng thái ví thu nhập cập nhật đúng.

### Operations

- Notification templates và noticeboard gửi/hiển thị đúng theo cấu hình.
- Các cấu hình hệ thống chính (SMTP, payment gateways, timezone, language, file manager) có thể cập nhật từ admin panel.

### Installation Readiness

- Môi trường đạt yêu cầu: PHP 8.x, MySQL 5.7+ hoặc MariaDB 10.2+, hosting tương thích Laravel.

## 7. Scope

### In Scope (MVP triển khai)

- Cài đặt và kích hoạt license.
- Thiết lập module nội dung: classes, categories, filters, quizzes, certificates, meetings.
- Thiết lập module người dùng: roles, groups, become instructor, organization users.
- Thiết lập module CRM/marketing/financial cơ bản: notifications, reports, discount, payout, sales.
- Thiết lập tích hợp cốt lõi: payment gateway, SMTP, Zoom, S3 (nếu dùng).

### Out of Scope (giai đoạn sau)

- Tùy biến sâu theme/code ngoài admin settings.
- Phát triển plugin riêng chưa có trong bundle.
- Triển khai mobile app nếu chưa có nhu cầu go-live mobile.

## 8. Dependencies

- Hosting/VPS tương thích Laravel.
- Runtime và DB: PHP 8.1, MySQL/MariaDB theo yêu cầu.
- License file và kích hoạt support qua CRM.
- Tài khoản dịch vụ bên thứ ba (payment provider, SMTP, Zoom, S3).
- Quy trình vận hành tài chính nội bộ (đặc biệt cho payout thủ công).

## 9. Risks

- Sai cấu hình hosting/PHP extension dẫn đến lỗi cài đặt hoặc hiệu năng kém.
- Quy trình payout thủ công gây chậm SLA thanh toán nếu không có SOP rõ.
- Cấu hình sai payment/SMTP làm gián đoạn giao dịch hoặc thông báo.
- Mở rộng nhanh nhưng chưa nâng cấp hạ tầng gây suy giảm trải nghiệm.

## 10. Success Metrics (KPIs)

- Time-to-launch: thời gian từ mua script đến go-live production.
- Activation rate: % instructor hoàn tất profile + tạo khóa học đầu tiên.
- Learner conversion: % visitor -> học viên trả phí/đăng ký.
- Learning completion: % học viên hoàn thành khóa + quiz.
- Financial ops SLA: thời gian xử lý payout request trung bình.
- System reliability: tỉ lệ lỗi cài đặt/cấu hình sau go-live.

## 11. Rollout Plan

### Phase 1 — Setup & Compliance

- Chuẩn hóa môi trường theo requirements.
- Cài đặt và kiểm tra truy cập admin panel.

### Phase 2 — Core Configuration

- Cấu hình general/financial/payment/security.
- Cấu hình roles/groups/organization flow.
- Tạo dữ liệu mẫu: categories, classes, quizzes.

### Phase 3 — Pilot Launch

- Onboard nhóm instructor/học viên pilot.
- Chạy end-to-end flow: enroll -> học -> đánh giá -> payout.
- Thu thập lỗi vận hành và tối ưu SOP.

### Phase 4 — Scale

- Bật thêm module nâng cao (plugins, mobile app, theme builder nếu cần).
- Nâng cấp hạ tầng theo tăng trưởng traffic/doanh thu.

## 12. Open Questions

- Mô hình kinh doanh ưu tiên: marketplace đa giảng viên hay academy nội bộ?
- Có yêu cầu pháp lý địa phương nào cho chứng chỉ/thanh toán cần bổ sung không?
- SLA mong muốn cho xử lý payout là bao lâu?
- Có cần go-live mobile app ngay giai đoạn đầu không?
- Ngưỡng KPI tối thiểu cho “pilot thành công” là gì?
