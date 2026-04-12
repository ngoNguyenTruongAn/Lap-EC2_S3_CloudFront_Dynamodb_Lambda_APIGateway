AWS Hands-on Labs: From Infrastructure to Serverless
Dự án này tổng hợp chuỗi 6 bài Lab thực hành về các dịch vụ cốt lõi của Amazon Web Services (AWS). Mục tiêu là xây dựng nền tảng vững chắc về hạ tầng điện toán đám mây và triển khai các kiến trúc hướng sự kiện (Event-driven).

🛠 Tech Stack
Compute: Amazon EC2, AWS Lambda
Storage: Amazon S3
Database: Amazon DynamoDB (NoSQL)
Networking & Content Delivery: Amazon CloudFront, Amazon API Gateway
Monitoring: Amazon CloudWatch

📝 Nội dung tóm tắt các bài Lab
1. Amazon EC2 (Elastic Compute Cloud)
Khởi tạo và cấu hình máy chủ ảo Linux.
Quản lý bảo mật với Security Groups (Mở cổng HTTP 80).
Thực hiện Scaling (thay đổi Instance Type) và bảo vệ máy chủ với Termination Protection.

2. Amazon S3 (Simple Storage Service)
Quản lý lưu trữ đối tượng (Object Storage).
Phân quyền truy cập qua Bucket Policy và ACL.
Triển khai Versioning để bảo vệ dữ liệu chống xóa nhầm/ghi đè.

3. Amazon CloudFront (CDN)
Thiết lập mạng phân phối nội dung (Content Delivery Network).
Sử dụng OAI (Origin Access Identity) để bảo mật S3 Bucket gốc.
Tối ưu hóa tốc độ tải trang thông qua cơ chế Caching tại các Edge Locations.

4. Amazon DynamoDB
Thiết lập cơ sở dữ liệu NoSQL không máy chủ.
Trải nghiệm tính linh hoạt của Schema-less (mỗi bản ghi có thể có thuộc tính khác nhau).
Phân biệt hiệu năng giữa thao tác Query (tối ưu) và Scan (tốn kém).

5. AWS Lambda
Xây dựng hàm xử lý ảnh tự động bằng Python.
Thiết lập Trigger để Lambda tự động "thức dậy" khi có file mới tải lên S3.
Tự động hóa quy trình Resize ảnh (Thumbnail) mà không cần quản lý server.

6. Amazon API Gateway
Xây dựng RESTful API làm cầu nối giữa Internet và AWS Lambda.
Triển khai hệ thống FAQ tự động trả về dữ liệu JSON.
Giám sát và Debug hệ thống thông qua CloudWatch Logs.

💡 Kết quả đạt được
Qua chuỗi bài Lab này, tôi đã nắm vững cách kết nối các dịch vụ AWS lại với nhau để tạo thành một hệ thống hoàn chỉnh:
1.Người dùng gọi API Gateway.
2.API Gateway kích hoạt Lambda.
3.Lambda truy xuất dữ liệu từ DynamoDB hoặc lưu trữ file vào S3.
4.Dữ liệu được phân phối tốc độ cao đến người dùng cuối qua CloudFront.
