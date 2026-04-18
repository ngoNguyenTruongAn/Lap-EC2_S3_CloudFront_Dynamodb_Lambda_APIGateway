# 🏗️ 1. Khung yêu cầu tổng thể (Requirements Checklist)

Tài liệu này cung cấp một cái nhìn toàn diện qua **17 hạng mục quan trọng nhất** khi bắt đầu một dự án Cloud.

---

## 🎯 Chiến lược và Kinh doanh

- **Mục tiêu**: Xác định mục tiêu kinh doanh chính và cách khối lượng công việc hỗ trợ các mục tiêu đó.  
- **Tiêu chuẩn**: Nắm rõ các tiêu chuẩn tổ chức hoặc quy định bắt buộc (ví dụ: các vùng được phê duyệt, kiến trúc mẫu).  
- **Thành công**: Đo lường sự thành công của dự án thông qua các chỉ số cụ thể.  

---

## 🛡️ Bảo mật và Tuân thủ

- **Yêu cầu bảo mật**: Xác định nhu cầu về mã hóa, nơi lưu trú dữ liệu và quản lý định danh.  
- **Quản lý lỗ hổng**: Thiết lập phương pháp quản lý lỗ hổng, vá lỗi và quản lý các thông tin nhạy cảm (*secrets*) như API keys.  
- **Tuân thủ**: Xác định các tiêu chuẩn pháp lý cần tuân thủ (GDPR, HIPAA, SOC 2) và tự động hóa việc thực thi thông qua công cụ như AWS Config.  

---

## 💾 Dữ liệu và Lưu trữ

- **Loại dữ liệu**: Phân loại dữ liệu (có cấu trúc, phi cấu trúc, nhật ký) và tần suất truy cập (*Hot*, *Warm*, *Cold*).  
- **Đặc tính**: Xác định yêu cầu về độ bền vững, tính sẵn sàng và độ trễ của dữ liệu.  
- **Sao lưu**: Thiết lập chính sách lưu trữ, sao lưu và vòng đời của dữ liệu (*lifecycle transitions*).  

---

## 📈 Vận hành và Hiệu suất

- **Giám sát**: Xác định các chỉ số vận hành (*SLI*, *SLO*) và quy trình phản ứng sự cố.  
- **Khả năng phục hồi**: Thiết lập chiến lược *RTO* (Thời gian phục hồi mục tiêu) và *RPO* (Điểm phục hồi mục tiêu).  
- **Hiệu suất**: Xác định các yêu cầu về thông lượng (*Throughput*) và độ trễ tối đa có thể chấp nhận.  
- **Mở rộng (Scaling)**: Xác định các yếu tố kích hoạt mở rộng (CPU, độ dài hàng đợi) và nhu cầu mở rộng theo chiều ngang hoặc chiều dọc.  

---

# 📊 2. Câu hỏi chuyên sâu về Kiến trúc quy trình (Workflow Architecture)

Tài liệu thứ hai đi sâu vào các câu hỏi kỹ thuật về luồng dữ liệu và lưu trữ.

---

## 🔍 Phân tích dữ liệu và Khối lượng (Volumes)

- **Phân tách dữ liệu**: Xác định cách tách biệt các loại dữ liệu và số lượng khối lưu trữ (*volumes*) tối thiểu/lý tưởng cần thiết.  
- **Cấu hình khối**: Xác định kích thước khối (*block size*) và kích thước bộ nhớ đệm (*cache size*) được đề xuất.  

---

## ⚡ Hiệu suất và Truy cập

- **Mẫu truy cập**: Dữ liệu được truy cập ngẫu nhiên (*random*) hay tuần tự (*sequential*).  
- **Yêu cầu IOPS**: Xác định yêu cầu về *IOPS* và thông lượng cho từng loại dữ liệu cụ thể.  
- **Đồng thời (Concurrency)**: Đánh giá số lượng người dùng hoặc thao tác đồng thời ảnh hưởng như thế nào đến hiệu suất hệ thống.  

---

## 🕒 Tính lâu bền và Chia sẻ

- **Dữ liệu tạm thời**: Xác định dữ liệu nào không cần lưu trữ lâu dài và có cần chia sẻ giữa các dịch vụ không.  
- **Cơ chế chia sẻ**: Nếu cần chia sẻ, xác định dịch vụ nào cần quyền truy cập và chia sẻ bằng cách nào.  