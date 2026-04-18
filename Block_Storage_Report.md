# 📦 AWS Storage Deep Dive: Block Storage (EBS vs. Instance Store)
---
## ⚡ 1. EC2 Instance Store (Lưu trữ tạm thời)
**Định nghĩa:** Là ổ cứng được gắn vật lý trực tiếp vào máy chủ vật lý (Host) đang chạy Instance của bạn. 
### 🎯 Trường hợp sử dụng lý tưởng:
> Dành cho các dữ liệu thay đổi thường xuyên nhưng không cần lưu giữ lâu dài.
* **Buffers & Caches:** Tăng tốc độ truy xuất dữ liệu tạm thời.
* **Scratch data:** Các file nháp xử lý trong quá trình tính toán.
* **Replicated Data:** Dữ liệu đã được sao lưu ở nhiều nơi khác (như cụm Load Balanced hoặc NoSQL database có cơ chế replication).
---
## 🛡️ 2. Amazon EBS (Elastic Block Store)
**Định nghĩa:** Là dịch vụ lưu trữ khối bền vững, hoạt động độc lập với vòng đời của EC2. Bạn có thể coi nó như một ổ cứng mạng (Network Drive) hiệu năng cao.
### 🎯 Trường hợp sử dụng lý tưởng:
* **Hệ điều hành (Boot volumes):** Nơi cài đặt Windows/Linux.
* **Cơ sở dữ liệu (Databases):** SQL Server, MySQL, SAP HANA cần độ bền vững cao.
* **Doanh nghiệp (Enterprise Apps):** Các ứng dụng quan trọng cần khả năng mở rộng linh hoạt.
=> "Lift and Shift" + "Database" = "EBS"
- Loại hình Volume của EBS (Tóm lược)
=> Để tối ưu hóa giữa Giá cả và Hiệu năng, AWS cung cấp 6 loại Volume được chia thành 2 nhóm lớn:
1. SSD-backed (Tối ưu IOPS): Dành cho các tác vụ cần tốc độ đọc/ghi ngẫu nhiên nhanh (Database, Boot volume).
**AWS chỉ hỗ trợ Multi-Attach cho các loại ổ đĩa thuộc dòng Provisioned IOPS SSD (io1 và io2)**.
2. HDD-backed (Tối ưu Throughput): Dành cho các tác vụ cần băng thông lớn, đọc/ghi tuần tự (Big Data, Log processing).
| Loại ổ đĩa          | Tên mã        | Ưu tiên                 | Trường hợp sử dụng tốt nhất           |
| :---                | :---          | :---                    | :---                                  |
| General Purpose     | gp3           | Giá/Hiệu năng           | Web server, Môi trường Dev/Test, OS   |
| Highest Perf        | ios           | Độ trễ/Độ bền           | Database cực lớn, ứng dụng cốt lõi    |
| Big Data            | st1           | Băng thông              | Phân tích dữ liệu, Log processing     |
| Cheap Archive       | sc1           | Giá rẻ nhất             | Dữ liệu lưu trữ ít dùng               |   
- Ba câu hỏi "Vàng" để chọn đúng loại đĩa:
Bạn hãy đối chiếu khối lượng công việc với sơ đồ ra quyết định sau:
1. Nhu cầu là gì (IOPS hay Throughput)?
- Nếu cần đọc/ghi ngẫu nhiên nhanh (Database) -> Chọn SSD (gp3, io2).
- Nếu cần xử lý file lớn, tuần tự (Big Data, Logs) -> Chọn HDD (st1, sc1).
2. Độ nhạy cảm với độ trễ (Latency)?
- Cần dưới 1ms (Cực nhanh) -> Chọn io2. (64,000 IOPS and 1,000 MB/s throughput)
- Chấp nhận vài ms đến 10ms (Nhanh) -> Chọn gp3.
3. Ưu tiên Giá hay Hiệu năng?
- Dùng Elastic Volumes để bắt đầu với loại rẻ nhất (gp3), sau đó nâng cấp lên nếu cần mà không làm gián đoạn hệ thống.                     |
---

## 📊 3. Bảng so sánh "Liền mạch"

| Đặc tính                | EC2 Instance Store                   | Amazon EBS                           |
| :-----------------------| :------------------------------------| :------------------------------------|
| **Bền vững dữ liệu**    | **Tạm thời** (Mất khi Instance Stop) | **Vĩnh viễn** (Độc lập với Instance) |
| **Vị trí vật lý**       | Gắn trực tiếp vào Host vật lý        | Kết nối qua mạng nội bộ AWS          |
| **Độ trễ (Latency)**    | Cực thấp (Dưới 1ms)                  | Thấp (Vài ms)                        |
| **Khả năng Snapshot**   | Không hỗ trợ                         | Có (Lưu vào S3)                      |
| **Khả năng thay đổi**   | Cố định theo loại Instance           | Linh hoạt (Elastic Volumes)          |
| **Độ bền (Durability)** | Phụ thuộc vào ổ đĩa vật lý           | Tự động nhân bản trong 1 AZ          |
=> Amazon EC2 instances and Amazon EBS volumes must be in the same Availability Zone.
---

## 🏛️ 4. Chiến lược tối ưu hóa (Well-Architected)
=> **Lời kết:** Không có giải pháp lưu trữ nào là "tốt nhất" cho mọi trường hợp. Sử dụng **Instance Store** khi bạn cần tốc độ tối đa cho dữ liệu tạm, và sử dụng **EBS** như một "xương sống" vững chắc cho toàn bộ hệ thống của bạn.
---
