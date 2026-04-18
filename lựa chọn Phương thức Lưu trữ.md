I. Check-list 5 câu hỏi "Vàng" để lựa chọn Phương thức Lưu trữ:
1. Tần suất truy cập: Bạn cần lấy dữ liệu hàng giây (Online) hay vài tháng một lần (Archive)?
2. Hiệu suất yêu cầu: Bạn cần băng thông rộng (Throughput) hay tốc độ đọc ghi dữ liệu nhỏ lẻ (IOPS)?
3. Giao thức kết nối: Ứng dụng của bạn dùng Linux (NFS) hay Windows (SMB)?
4. Độ bền vững: Dữ liệu này mất đi có sao không (Transient) hay phải tồn tại vĩnh viễn (Durable)?
5. Ngân sách: Bạn sẵn sàng trả bao nhiêu tiền để duy trì lượng dữ liệu này?
II.Quy trình Tối ưu hóa
- Thực hiện vòng lặp: Đo lường -> Phân tích -> Cải tiến
+ Giám sát (Monitoring): Sử dụng Amazon CloudWatch để theo dõi các chỉ số như IOPS thực tế và dung lượng đang dùng.
+ Thử nghiệm (Benchmarking): Chạy thử tải (Load testing) để xem ở mức nào thì ổ cứng của bạn bị "nghẽn".
+ Tự động hóa: Thiết lập cảnh báo và tự động tăng dung lượng hoặc thay đổi loại lưu trữ khi đạt ngưỡng giới hạn.
