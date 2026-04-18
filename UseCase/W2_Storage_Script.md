# W2 Presentation Script (Ngắn gọn cho nhóm)

## 1. Mục tiêu W2
Tập trung vào Storage và bảo mật dữ liệu. Câu hỏi chính cần trả lời là dữ liệu nằm ở đâu, ai được truy cập, và vì sao.

## 2. Kiến trúc tổng quan
Luồng chính của hệ thống:
User -> Internet -> Route 53 -> Security Layer (CloudFront + WAF + ACM) -> Internet Gateway -> ALB -> ECS -> RDS MySQL.
* Sẽ có 1 DataLayer chứa các S3 bucket.

Ngoài luồng chính, hệ thống có thêm lớp quan sát và kiểm toán:
- CloudWatch: theo dõi metric, log runtime của ECS, tạo cảnh báo khi hệ thống bất thường.
- CloudTrail: ghi lại toàn bộ API actions trong AWS để audit ai làm gì, khi nào.

Backend chạy trên ECS theo mô hình stateless, nên dữ liệu bền vững không lưu trong container.

## 3. Storage design (W2)
Nhóm tách dữ liệu thành 3 S3 bucket:
- user-media: avatar, ảnh sản phẩm, ảnh shop, ảnh review.
- logs-audit: log từ ALB, CloudFront và log kiểm toán từ CloudTrail.
- backup-archive: backup và snapshot export của database.

Việc tách bucket giúp phân quyền rõ ràng, giảm blast radius, và tối ưu lifecycle theo từng loại dữ liệu.

## Luồng quan sát và kiểm toán:
- CloudWatch thu thập log/metric theo thời gian thực để vận hành và cảnh báo sự cố.
- CloudTrail tập trung vào audit bảo mật và truy vết thay đổi cấu hình, quyền truy cập.
- Dữ liệu log cần lưu trữ dài hạn được đưa vào logs-audit để phục vụ điều tra sau này.

## 4. KMS và quyền truy cập
Mỗi bucket dùng SSE-KMS với key riêng:
- key-media cho user-media.
- key-logs cho logs-audit.
- key-backup cho backup-archive.

Phân quyền theo role:
- ECS Task Role: chỉ được dùng key-media để đọc/ghi media.
- Role ghi log: chỉ được Encrypt vào logs-audit, không có Decrypt.
- Backup role: ghi backup bằng key-backup.
- DBA/Ops role: chỉ role này mới có Decrypt key-backup để restore.

Muốn đọc dữ liệu mã hóa phải qua 2 lớp đồng thời:
- Quyền S3.
- Quyền KMS.

## 5. Vì sao phải dùng KMS
- Kiểm soát truy cập mã hóa theo role, không mở rộng vô tội vạ.
- Có thể thu hồi quyền nhanh khi có sự cố.
- Có audit sử dụng key để phục vụ bảo mật và compliance.
- Cô lập rủi ro giữa media, logs và backup.

## 6. Lựa chọn storage cho DB
RDS MySQL dùng gp3 vì cân bằng tốt giữa hiệu năng và chi phí cho workload hiện tại.
Khi IOPS và latency trở thành điểm nghẽn mới cần xem xét nâng cấp io1/io2.

## 7. Kết luận
Thiết kế hiện tại đáp ứng 4 mục tiêu:
1. Bảo mật dữ liệu.
2. Tối ưu chi phí lưu trữ.
3. Sẵn sàng mở rộng cho các tuần tiếp theo.
4. Quan sát vận hành và kiểm toán bảo mật rõ ràng (CloudWatch + CloudTrail).