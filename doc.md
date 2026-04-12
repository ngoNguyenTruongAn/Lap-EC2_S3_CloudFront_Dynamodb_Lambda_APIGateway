                                INTRODUCTION TO AMAZON EC2
Task 1: Launch Your Amazon EC2 Instance
- Ở AWS Management Console, tìm kiếm và chọn EC2
- Chọn Launch instance để khởi tạo 
- Name and tags: Đặt tên là Web Server
- Application and OS Images (AMI): Chọn Amazon Linux 2023 AMI (Chọn Hệ điều hành)
- Instance type: Chọn t3.micro (2 vCPU và 1 GiB Memmory) (Cấu hình RAM/CPU)
- Key pair -> Proceed without a key pair
- Network settings -> Edit (để thiết lập VPC)
- Chọn VPC có tên Lab VPC
- Subnet -> Chọn Public Subnet 1
- Firewall (Security Groups): Chọn Select existing security group -> Web Server security group (Chọn áp dụng một bộ quy tắc bảo mật đã có sẵn)
- Click vào Advanced details, tìm Termination protection -> Enable (Tạo 1 chôt an toàn khi lỡ như xóa máy chủ)
- User data dán đoạn script đã được cung cấp: 
#!/bin/bash
dnf -y install httpd
systemctl enable httpd
systemctl start httpd
echo '<html><h1>Hello From Your Web Server!</h1></html>' > /var/www/html/index.html
![DoneLaunchEC2](./evidence/Hoàn%20thành%20Tạo%20EC2.jpg)
Task 2: Monitor Your Instance
- Status and alarms: Kiểm tra tính sẵn sàng của hệ thống (System/Instance/EBS reachability).
![Status and alarms](./evidence/StatusCheck.jpg)
- Monitoring : Xem các biểu đồ CloudWatch (CPU, Network, Disk)
- Actions -> Monitor and troubleshoot -> Get system log (Dùng để kiểm tra quá trình khởi động và xác nhận script User Data đã chạy thành công)
![System Log](./evidence/SystemLog.jpg)
- Actions -> Monitor and troubleshoot -> Get instance screenshot (Xem hình ảnh thực tế của màn hình console máy chủ)
![Instance screenshot](./evidence/Instance%20Sc.jpg)
Task 3: Update Your Security Group and Access the Web Server
- Cập nhật Security Group:
+ Truy cập mục Security Groups ở Phần Network & Security
+ Chọn Web Server security group
![Web Server security group](./evidence/Web%20Server%20security%20group.jpg)
+ Tại tab Inbound rules, chọn Edit inbound rules
+ Thêm rule mới: Type HTTP, Source Anywhere-IPv4 (0.0.0.0/0) -> Save rules
![Edit inbound rules](./evidence/Edit%20Inbound%20rules.jpg)
- Thử truy cập: Copy Public IPv4 address của instance và dán vào trình duyệt với tiền tố http:// 
![Thử truy cập](./evidence/Thử%20truy%20cập.jpg)
Task 4: Resize Your Instance (Type & EBS Volume)
- Stop Instance: Chọn Web Server -> Instance state -> Stop instance (Bắt buộc phải dừng máy chủ trước khi đổi loại instance)
![Stop Instance](./evidence/Stop%20Instance.jpg)
- Actions -> Instance settings -> Change instance type (Đổi cấu hình Instance)
- Chọn t3.small và nhấn Change
![Stop Instance](./evidence/Đổi%20cấu%20hình%20Instance.jpg)
- Thay đổi kích thước ổ đĩa (EBS Volume) -> mục Volumes (phần Elastic Block Store)
- Chọn Volume đang gắn vào Web Server -> Actions -> Modify volume
- Thay đổi Size từ 8 GiB thành 10 GiB. Nhấn Modify
![Modify volume](./evidence/Modify%20volume.jpg)
- Quay lại mục Instances, chọn Web Server -> Instance state -> Start instance
Task 5: Test Termination Protection
- Chọn Web Server > Instance state > Terminate instance (thử xóa)
![Modify volume](./evidence/Test%20Termination%20Protection.jpg)
- Actions > Instance settings > Change termination protection. Bỏ chọn Enable và Save (Tắt bảo vệ)
- Web Server -> Instance state -> Terminate instance => Trạng thái chuyển sang Terminated
![Terminated Instance](./evidence/Terminated%20Instance.jpg)

                                Storage Service (S3)
Task 1: Create a Bucket
- Copy Account ID
- Ở AWS Management Console, tìm kiếm và chọn S3 -> Create bucket
- Đặt tên Bucket (reportbucket-ID)
- Object Ownership -> ACLs enabled kích hoạt tính năng phân quyền Access Control List giup cấp quyền cho các file riêng 
![Bucket S3](./evidence/Bucket%20S3.jpg)
Task 2: Upload an Object 
- Upload file -> Chọn new-report.png (Để đưa dữ liệu thực tế lên Cloud)
![Upload file new-report.png](./evidence/Upload%20file%20-%20new-report.jpg)
Task 3: Make an Object Public
- Truy cập Object URL -> Bị Access Denied (cơ chế bảo mật mặc định của AWS -> Phải có sự cho phép phân quyền)
![Access Denied](./evidence/Access%20Denied.jpg)
- Deselect "Block all public access" (Mở "khóa tổng" của Bucket - Nếu khóa này vẫn bật,không thể làm bất kỳ file nào bên trong trở nên công khai)
![Deselect Block](./evidence/Deselect%20Block.jpg)
- Make public using ACL -> Để cấp quyền "Read" cho mọi người trên Internet đối với file ảnh cụ thể
Task 4: Test Connectivity từ EC2
- Vào EC2 -> Chọn Bastion Host -> Connect (dùng Session Manager)
- Gõ lệnh aws s3 ls (Kiểm tra xem máy chủ đã có quyền liệt kê các bucket trong tài khoản chưa)
- Gõ lệnh aws s3 cp report-test1.txt s3://<tên-bucket> -> Lỗi (Không có quyền Ghi dữ liệu vòa bucket)
Task 5: Create a Bucket Policy
- Vào IAM -> Roles -> Tìm và copy ARN của EC2InstanceProfileRole
![EC2InstanceProfileRole](./evidence/EC2InstanceProfileRole.jpg)
- Mở AWS Policy Generator -> Type: S3 Bucket Policy -> Effect: Allow -> Principal: (Dán Role ARN) -> Actions: GetObject, PutObject ->  Add Statement -> Generate Policy
- nó sẽ ra JSON format, hãy thêm /* ở Resource -> như thế này: 
{
    "Version": "2012-10-17",
    "Id": "Policy1604361694227",
    "Statement": [
        {
            "Sid": "Stmt1604361692117",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::416159072693:role/EC2InstanceProfileRole"
            },
            "Action": [
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": "arn:aws:s3:::reportbucket987987/*"
        }
    ]
}
-> Copy JSON format -> Save changes
- Dán Policy JSON format đã copy vào phần "Bucket Policy" trong tab Permissions
Task 6: Explore Versioning
- Properties tab -> Bucket Versioning -> Edit -> Enable (Bật tính năng lưu lại lịch sử. Kể từ giờ, mỗi khi bạn sửa hay xóa file, bản cũ vẫn được AWS giữ lại)
- Upload đè file sample-file.txt mới
- nút "Show versions" (Để nhìn thấy tất cả các phiên bản (Version ID) của cùng một file)
- Tắt nút Show versions -> xóa file -> file không bị xóa chỉ hiện Delete Marker (S3 không xóa file thật mà chỉ dán một cái nhãn "đã xóa" lên trên cùng để ẩn file đi)
- Xóa nhãn "Delete Marker" (Phục hồi (Restore) file về trạng thái trước khi xóa)

                                AMAZON CLOUDFRONT
Task 1: Creating an S3 bucket and storing an image file
- Ở AWS Management Console, tìm kiếm và chọn S3 -> Create bucket
- Bucket name -> nhập cftan2907
- setting còn lại để default -> Create Bucket
![Create Bucket-CloudFront](./evidence/Create%20Bucket-CloudFront.jpg)
- Upload file -> Ảnh .png vừa tải về máy -> Upload
![Upload file png](./evidence/Upload%20file%20png.jpg)
- Chọn object mình vừa upload -> Copy URL -> dán vào 1 tab trình duyệt mới -> Lỗi Access Denied (Chưa có quyền truy cập)
![Access Denied png](./evidence/Access%20Denied%20png.jpg)
Task 2: Creating a CloudFront web distribution
- Search "CloudFront" -> Create distribution  -> Chọn plan Pay as you go (Dùng bao nhiêu trả bấy nhiêu) -> Next
- Distribution name -> Nhập cloudfront-lab-distribution -> Next
- Origin type -> Amazon S3
- S3 origin -> Browse S3 -> Chọn bucket S3 mà mình vừa tạo -> Còn lại để default -> Next
![Origin type](./evidence/Origin%20type.jpg)
- Web Application Firewall (WAF) -> Do not enable security protections -> Next -> Create distribution
![new distribution](./evidence/new%20distribution.jpg)
Task 3: Testing the distribution
- Copy Distribution domain name ở Distribution vừa tạo
- Tạo 1 file myimage.html và copy nội dung đặt vào file đó: 
<html>
<head>My CloudFront Test</head>
<body>
<p>My text content goes here.</p>
<img src="https://DOMAIN/OBJECT" alt="my test image">
</body>
</html>
- Thay DOMAIN = Distribution domain name bạn vừa copy ()
- Thay OBJECT = Tên file mà bạn vừa upload lên bucket S3 vừa tạo -> nội dung sau khi replace thì sẽ như thế này:
<html>
<head>My CloudFront Test</head>
<body>
<p>My text content goes here.</p>
<img src="https://d1hscfftwuvn12.cloudfront.net/png-transparent-corgi-happy-cute-corgi-illustration.png " alt="my test image">
</body>
</html>

- Mở file HTML bằng trình duyệt file bạn vừa mới tạo và chỉnh sửa 
![myimage.html](./evidence/myimage.html-png.jpg)
=> Hiển thị thành công hình ảnh

                                AMAZON DYNAMODB
Task 1: Create a New Table
- Search "DynamoDB" -> Create table (Tiến hành tạo)
- Table name -> Music ,Partition key -> Artist (String) ,Sort key - optional -> Song (String)
![Done Create table](./evidence/Done%20Create%20table.jpg)
Task 2: Add Data
- Click Explore table items -> Create item và nhập data:
+ Artist Value: Pink Floyd
+ Song Value : Money
- Click Add new attribute -> String -> Đặt Atribute name : Album , Value: The Dark Side of the Moon
- Tương tự tạo 1 Attribute mới -> Number -> Name: Year , Value: 1973
-> Create item
![Done Create item](./evidence/Done%20Create%20item.jpg)
- Tương tự tạo thêm 1 item thứ 2:
1.Artist : John Lennon (String)
2.Song : Imagine(String)
3.Album: Imagine(String)
4.Year: 1971(Number)
5.Genre: Soft rock (String)
- Tương tự tạo thêm 1 item thứ 3:
1.Artist : Psy (String)
2.Song : Gangnam Style(String)
3.Album: Psy 6 (Six Rules), Part 1(String)
4.Year: 2011(Number)
5.LengthSeconds: 219 (Number)