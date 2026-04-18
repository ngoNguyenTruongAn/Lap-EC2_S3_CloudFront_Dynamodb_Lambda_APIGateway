# 🛡️ Sổ tay Kỹ thuật: AWS Identity and Access Management (IAM) & Security

**Chủ đề:** Quản lý Định danh, Ủy quyền và Bảo mật Hạ tầng Đám mây.
**Mục tiêu:** Tài liệu lưu trữ phục vụ Midterm Review và Mission: Lock It Down.

---

## 🚪 1. Cơ chế Truy cập AWS (API-Centric)
Mọi hành động trên AWS (qua Console, CLI, hay SDK) thực chất đều là một cuộc gọi **API**. Trước khi thực thi, mỗi yêu cầu phải vượt qua hai lớp kiểm soát:

* **Authentication (Xác thực - Who are you?):** Kiểm tra danh tính thông qua mật khẩu, Access Keys hoặc Tokens.
* **Authorization (Ủy quyền - What can you do?):** Kiểm tra quyền hạn dựa trên các **IAM Policies** được đính kèm.

[Image of AWS IAM authentication and authorization workflow]

---

## 🔑 2. Quản lý Thông tin Xác thực (Credentials)

| Loại chìa khóa | Đối tượng | Mục đích |
| :--- | :--- | :--- |
| **Password & MFA** | Con người | Đăng nhập giao diện AWS Management Console. |
| **Access Keys** | Máy móc/Code | Sử dụng cho CLI, SDK để gọi API (Gồm Access Key ID & Secret Access Key). |
| **Key Pairs** | Kỹ sư hệ thống | Dùng để SSH/RDP trực tiếp vào hệ điều hành của EC2. |
| **Temporary Tokens** | Ứng dụng/Roles | Cấp bởi AWS STS, có thời hạn ngắn, bảo mật cao nhất. |

---

## 👥 3. Các thực thể trong IAM

* **IAM Users:** Danh tính đại diện cho một người hoặc một dịch vụ cụ thể.
* **IAM Groups:** Tập hợp các User. Nên gán quyền theo Group dựa trên vai trò (Job Function) để dễ quản lý.
* **IAM Roles:** Danh tính tạm thời. Cực kỳ quan trọng để cho phép các dịch vụ AWS (như EC2) truy cập tài nguyên khác (như S3) mà không cần lưu trữ Access Keys trên máy chủ.
## ⚡ 4. Các dịch vụ bảo mật bổ trợ

* **Amazon Cognito:** Quản lý đăng nhập cho người dùng ứng dụng di động/web (hỗ trợ Google, Facebook).
* **AWS STS (Security Token Service):** Cấp quyền truy cập tạm thời.
* **AWS Secrets Manager:** Lưu trữ và tự động xoay vòng (rotate) các thông tin nhạy cảm như mật khẩu Database.
* **AWS Artifact:** Cổng thông tin tải các báo cáo tuân thủ (SOC, ISO, PCI) để chứng minh an ninh hạ tầng.

---

## 🏆 5. Nguyên tắc Bảo mật Thực chiến (Best Practices)

1. **Principle of Least Privilege:** Chỉ cấp quyền tối thiểu cần thiết để hoàn thành công việc.
2. **Bật MFA:** Áp dụng cho tất cả người dùng, đặc biệt là tài khoản Root.
3. **Sử dụng IAM Roles cho EC2:** Tuyệt đối không lưu Access Keys trong code hoặc file cấu hình trên server.
4. **Xoay vòng thông tin xác thực:** Thường xuyên thay đổi mật khẩu và Access Keys.
5. **Cấu hình Password Policy:** Yêu cầu mật khẩu mạnh và định kỳ thay đổi.