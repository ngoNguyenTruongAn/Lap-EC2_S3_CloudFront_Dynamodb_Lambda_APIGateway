## 🏗️ 1. Định nghĩa về Tài nguyên Tính toán (Compute)

Tài nguyên tính toán là "bộ não" của ứng dụng, chịu trách nhiệm xử lý các thuật toán và dữ liệu.
* **CPU & RAM:** Thành phần cốt lõi để thực hiện tính toán.
* **Cloud Benefit:** Khả năng truy cập lượng tài nguyên lớn hơn nhiều so với máy chủ vật lý đơn lẻ và tính đàn hồi (**Elasticity**) để đáp ứng các đợt truy cập đột biến.

---

## 🖥️ 2. Amazon EC2 - Virtual Machines (Máy ảo)

Đây là lựa chọn phổ biến nhất cho các kiến trúc truyền thống và cần quyền kiểm soát cao.

* **Instance Types:** Đa dạng cấu hình (CPU, RAM, Storage) phù hợp với từng loại ứng dụng (Compute-intensive, Memory-intensive).
* **Kiểm soát:** Bạn quản lý từ lớp Hệ điều hành (OS) trở lên.
* **Ứng dụng:** Lý tưởng cho SQL Server và các ứng dụng đòi hỏi quyền Admin để cài đặt phần mềm bên thứ ba.

---

## 📦 3. Container Services (Dịch vụ Container)

Container hóa giúp đơn giản hóa quy trình đóng gói và triển khai ứng dụng (CI/CD).

* **Amazon ECS:** Đơn giản, tích hợp sâu với các dịch vụ AWS khác.
* **Amazon EKS:** Chạy Kubernetes tiêu chuẩn, linh hoạt cho việc quản lý quy mô lớn và đa đám mây (Multi-cloud).
* **Lợi ích:** Đảm bảo ứng dụng chạy giống hệt nhau trên mọi môi trường từ Dev đến Production.

---

## ⚡ 4. Serverless với AWS Lambda

Cách tiếp cận hiện đại để xây dựng ứng dụng theo hướng sự kiện (Event-driven).

* **Không quản lý Server:** Bạn chỉ tập trung vào viết mã nguồn.
* **Tối ưu chi phí:** Không tính phí khi mã không chạy. Trả tiền theo từng lượt request và thời gian xử lý.
* **Ứng dụng:** Rất tốt cho các tác vụ tự động hóa, xử lý ảnh, gửi email hoặc các vi dịch vụ (Microservices).

---

## 📊 5. Bảng so sánh lựa chọn Compute

| Tiêu chí | Amazon EC2 | Containers (ECS/EKS) | AWS Lambda |
| :--- | :--- | :--- | :--- |
| **Quản trị hạ tầng** | Khách hàng quản lý | AWS quản lý một phần | AWS quản lý hoàn toàn |
| **Tính đàn hồi** | Cần cấu hình Auto Scaling | Nhanh chóng | Tự động hoàn toàn |
| **Thời gian chạy** | 24/7 hoặc theo lịch | Theo trạng thái Container | Ngắn hạn (Dưới 15 phút) |

---

## 💡 Lời khuyên thực chiến từ Mentor

Khi thiết kế cho 5.000 người dùng trong dự án **OpenAutomate**:
1.  **Sử dụng EC2 cho SQL Server:** Để đảm bảo sự ổn định và quyền kiểm soát cấu hình đĩa EBS.
2.  **Cân nhắc Lambda cho Automation:** Các tác vụ tự động hóa nhỏ có thể được chuyển sang Lambda để tiết kiệm chi phí vận hành máy chủ 24/7.
3.  **An ninh (IAM Role):** Mỗi tài nguyên tính toán phải có IAM Role riêng biệt với quyền hạn tối thiểu. Không bao giờ lưu trữ Access Keys bên trong máy chủ EC2 hay mã nguồn.

---