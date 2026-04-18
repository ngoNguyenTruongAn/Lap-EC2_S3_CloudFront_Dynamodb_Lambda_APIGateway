AWS Backup
- AWS Backup là một dịch vụ bảo vệ dữ liệu được quản lý hoàn toàn, giúp bạn tập trung hóa và tự động hóa việc sao lưu dữ liệu trên toàn bộ các dịch vụ của AWS (như EC2, EBS, S3, RDS) và cả hạ tầng tại chỗ (On-premises), giúp loại bỏ nhu cầu viết script thủ công và đảm bảo tuân thủ các quy định về an toàn dữ liệu.
- Các tính năng nổi bật (Key Features):
1.Centralized backup management: Tất cả nằm tại một bảng điều khiển duy nhất(RDS, EBS, S3).
2.Policy-based backup: chỉ cần định nghĩa một lần: "Sao lưu lúc 2h sáng hàng ngày, giữ trong 30 ngày". Hệ thống sẽ tự áp dụng cho mọi tài nguyên có gắn Tag tương ứng
3.Automated backup scheduling: AWS Backup sẽ tự động sao lưu tài nguyên AWS của bạn theo các chính sách và lịch trình mà bạn xác định.
4.Automated retention management: Giảm thiểu chi phí lưu trữ bản sao lưu bằng cách chỉ giữ lại các bản sao lưu trong thời gian cần thiết.
5.Lifecycle Management: Tự động chuyển các bản sao lưu cũ sang bộ nhớ lưu trữ giá rẻ (Cold Storage) để tối ưu chi phí.
6.Incremental Backups: Chỉ lưu trữ những phần dữ liệu bị thay đổi, giúp tiết kiệm dung lượng và chi phí đáng kể.
Các dịch vụ cụ thể |
| :---             | :---                                                        |
| **Compute**      | Amazon EC2 (bao gồm cả Instance Store-backed)               |
| **Storage**      | Amazon EBS, EFS, S3, FSx (Lustre, Windows), Storage Gateway |
| **Databases**    | Amazon RDS, Aurora, DynamoDB, Neptune, DocumentDB           |

Native Service Snapshots
- Snapshots là tính năng sao lưu tức thì của các dịch vụ AWS, cho phép tạo ra các bản sao dữ liệu tức thời mà không làm gián đoạn quá trình hệ thống đang hoạt động.

- Amazon EBS Snapshots (Ổ cứng EC2)
+ Với cơ chế cực:
* **Incremental:** Chỉ lưu những khối dữ liệu (blocks) bị thay đổi sau lần chụp gần nhất. Điều này giúp bạn **tiết kiệm tiền** và thời gian chụp cực nhanh.
* **Lưu trữ tại S3:** Dù bạn chụp ổ EBS, nhưng bản Snapshot được cất giấu an toàn tại S3.
* **Khôi phục tức thì:** Khi tạo ổ đĩa mới từ Snapshot, bạn có thể **dùng được ngay**. Dữ liệu sẽ được AWS tải ngầm từ S3 lên ổ đĩa trong lúc bạn đang làm việc.
- Amazon FSx for Lustre Snapshots
+ Trong dịch vụ FSx for Lustre, Snapshots thường được gọi là **Backups**
