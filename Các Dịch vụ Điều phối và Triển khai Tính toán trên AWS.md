Các Dịch vụ Điều phối và Triển khai Tính toán trên AWS
## 🎼 1. AWS Step Functions - "Người nhạc trưởng" Workflow
Step Functions giúp bạn kết nối nhiều dịch vụ AWS thành các luồng công việc (workflows) trực quan.

* **Cơ chế:** Điều phối các thành phần (như Lambda, ECS, DB) theo các bước logic (First X, then Y).
* **Ưu điểm:** Tự động xử lý lỗi (Retry), theo dõi trạng thái của từng bước và giảm bớt mã nguồn kết nối (plumbing code).
* **Thực chiến:** Rất hữu ích cho quy trình Backup tự động: *Chụp Snapshot -> Kiểm tra lỗi -> Copy sang Region khác -> Gửi báo cáo Slack.*

---

## 🚀 2. Các giải pháp triển khai đặc thù

### 🔹 AWS Elastic Beanstalk (PaaS)
* **Mục tiêu:** Giúp lập trình viên đưa ứng dụng lên mây nhanh nhất mà không cần quản lý hạ tầng.
* **Tự động hóa:** Tự động xử lý việc cấp phát tài nguyên, cân bằng tải (Load Balancing), và giám sát sức khỏe ứng dụng.
* **Kiểm soát:** Bạn vẫn có thể truy cập vào các instance EC2 bên dưới để tinh chỉnh nếu cần.

### 🍱 AWS Batch (Xử lý hàng loạt)
* **Mục tiêu:** Chạy hàng nghìn công việc tính toán quy mô lớn một cách hiệu quả.
* **Cơ chế:** Tự động chọn loại máy (EC2 hoặc Fargate) tối ưu nhất về chi phí và hiệu năng để hoàn thành các "Job" được giao.

### 🕯️ Amazon Lightsail (VPS đơn giản)
* **Mục tiêu:** Cung cấp gói dịch vụ "tất cả trong một" (Compute, Storage, Networking) với giá cố định.
* **Ứng dụng:** Phù hợp cho website WordPress đơn giản, môi trường dev cá nhân hoặc các dự án nhỏ bắt đầu trên AWS.

---

## 🎯 3. Chiến lược lựa chọn Compute (Decision Guide)

Dựa trên đặc thù của đội ngũ và dự án, hãy chọn hướng đi phù hợp:

1.  **Hướng Serverless (Tối ưu tốc độ & chi phí):**
    * Dành cho team xây dựng từ đầu, muốn tập trung 100% vào Business Logic.
    * *Dịch vụ:* Lambda, API Gateway, S3, Step Functions.
2.  **Hướng Container (Tối ưu tính nhất quán):**
    * Dành cho team đã quen với Docker hoặc có ứng dụng cần chạy lâu hơn 15 phút.
    * *Dịch vụ:* ECS/EKS chạy trên Fargate.
3.  **Hướng Truyền thống/Monolith (Tối ưu quyền kiểm soát):**
    * Dành cho các ứng dụng web phức tạp, cần truy cập sâu vào Server.
    * *Dịch vụ:* Elastic Beanstalk hoặc EC2 thuần túy.