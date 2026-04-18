# Bao cao lua chon service AWS cho mini-e_web (W1 + W2 Storage)

## 1. Muc tieu
Tai lieu nay tong hop cac service dang dung cho kien truc mini-e_web va ly do lua chon tung service. Pham vi duoc chot:
- Backend chay tren ECS Fargate
- Database dung RDS MySQL
- Frontend host tren S3 (thay cho Amplify)
- W2 bo sung lop Storage gom user-media, logs-audit, backup-archive

## 2. Luong tong quan (main flow)
User -> Internet -> Route 53 -> Security Layer (CloudFront + WAF + ACM) -> Internet Gateway -> ALB -> ECS -> RDS MySQL

Luong storage bo sung:
- ECS <-> S3 user-media (doc/ghi file)
- RDS -> S3 backup-archive (backup/snapshot export)
- ALB/CloudFront -> S3 logs-audit (log luu tru)
- KMS ma hoa du lieu at-rest cho S3 va RDS

## 3. Service trong W1 va ly do su dung

### 3.1 Route 53
- Vai tro: DNS cho web va API.
- Ly do chon:
  - Quan ly domain on dinh, tich hop tot voi CloudFront/ALB.
  - Ho tro route policy, health check cho mo rong sau nay.

### 3.2 CloudFront
- Vai tro: CDN phan phoi noi dung frontend va co the cache mot phan noi dung API.
- Ly do chon:
  - Giam do tre cho nguoi dung o nhieu khu vuc.
  - Giam tai cho origin (S3/ALB), toi uu chi phi va hieu nang.

### 3.3 AWS WAF
- Vai tro: Loc va chan request doc hai truoc khi vao he thong.
- Ly do chon:
  - Giam rui ro cac tan cong pho bien (bot, injection pattern, rate abuse).
  - Tao bien gioi bao mat som tai edge.

### 3.4 ACM
- Vai tro: Cap va quan ly TLS certificate.
- Ly do chon:
  - Bat buoc cho HTTPS an toan.
  - Tu dong gia han, giam sai sot van hanh.

### 3.5 VPC
- Vai tro: Phan tach mang rieng cho ung dung.
- Ly do chon:
  - Tao ranh gioi mang ro rang giua public va private.
  - De ap dung Security Group theo nguyen tac least privilege.

### 3.6 Internet Gateway
- Vai tro: Ket noi subnet public ra Internet.
- Ly do chon:
  - Can thiet de ALB nhan traffic cong khai.

### 3.7 ALB
- Vai tro: Can bang tai Layer 7 vao ECS service.
- Ly do chon:
  - Dinh tuyen HTTP/HTTPS tot cho API.
  - Ho tro health check, path-based routing, kha nang scale.

### 3.8 ECS Fargate
- Vai tro: Chay backend container theo mo hinh stateless.
- Ly do chon:
  - Khong can quan ly server EC2, giam ganh van hanh.
  - De scale ngang theo tai va trien khai rolling update.

### 3.9 RDS MySQL (Multi-AZ)
- Vai tro: Luu tru du lieu giao dich he thong.
- Ly do chon:
  - Managed database, giam cong viec quan tri DB.
  - Co kha nang backup, monitoring, failover tot hon tu host.
  - Phu hop workload giao dich cua ung dung e-commerce.

### 3.10 S3 Frontend Static Site
- Vai tro: Luu file frontend build static.
- Ly do chon:
  - Don gian, ben vung, chi phi thap cho static content.
  - Ket hop CloudFront de toi uu phan phoi toan cau.

## 4. Service bo sung W2 (Storage) va ly do su dung

### 4.1 S3 user-media
- Du lieu: avatar, anh san pham, anh shop, anh review.
- Ly do tach rieng:
  - De dat policy truy cap rieng cho app role.
  - Vong doi du lieu khac voi log va backup.
  - Han che blast radius neu xay ra su co sai quyen.

### 4.2 S3 logs-audit
- Du lieu: ALB access logs, CloudFront logs.
- Ly do tach rieng:
  - Log can retention va lifecycle khac media.
  - De audit, truy vet su co, phuc vu bao cao bao mat.

### 4.3 S3 backup-archive
- Du lieu: backup DB, snapshot export, ban luu tru dai han.
- Ly do tach rieng:
  - Do nhay cam cao, can policy nghiem ngat hon.
  - De ap dung retention dai han va toi uu chi phi luu tru.

### 4.4 AWS KMS
- Vai tro: Quan ly khoa ma hoa cho du lieu at-rest.
- Ly do chon:
  - Kiem soat ma hoa tap trung, co audit su dung key.
  - Tach quyen truy cap du lieu va quyen su dung key.
  - Ap dung cho SSE-KMS tren S3 va encryption tren RDS.

## 5. Storage class va lifecycle de xuat

### 5.1 user-media
- 0-30 ngay: S3 Standard
- 31-90 ngay: Standard-IA
- >90 ngay: Glacier Instant Retrieval
- Ly do: anh moi truy cap cao, anh cu truy cap giam dan.

### 5.2 logs-audit
- 0-30 ngay: S3 Standard
- 31-180 ngay: Glacier Flexible Retrieval
- >180 ngay: Glacier Deep Archive
- Ly do: log chu yeu de dieu tra, tan suat doc thap theo thoi gian.

### 5.3 backup-archive
- 0-30 ngay: Standard-IA
- 31-365 ngay: Glacier Flexible Retrieval
- >365 ngay: Deep Archive
- Ly do: toi uu chi phi cho backup dai han, van dam bao kha nang khoi phuc.

## 6. Lua chon storage cho RDS MySQL
- Lua chon khuyen nghi: GP3.
- Ly do:
  - Can bang tot giua chi phi va hieu nang cho workload e-commerce vua.
  - Linh hoat scale theo nhu cau thuc te.
  - Tranh over-provision som nhu io2 neu chua co bang chung bottleneck.

Khi can xem xet nang cap io1/io2:
- IOPS cao lien tuc, latency rat nhay cam, workload database nang on dinh.

## 7. Ket luan
Kien truc W1 + W2 nay dap ung 3 muc tieu:
1. Van hanh on dinh va de mo rong (ECS + ALB + RDS Multi-AZ).
2. Bao mat theo lop (Security Layer + private subnet + KMS).
3. Quan tri du lieu dung muc dich (tach bucket theo media, log, backup + lifecycle toi uu).

Day la nen tang phu hop de buoc sang cac tuan tiep theo ve CI/CD, observability va hardening bao mat.