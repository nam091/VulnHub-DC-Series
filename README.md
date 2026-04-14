# Tổng hợp Báo cáo Lab An toàn thông tin & Write-ups VulnHub

Dự án này là tập hợp các báo cáo chi tiết về quá trình kiểm thử xâm nhập (Penetration Testing) và giải quyết các thử thách CTF. Các tài liệu được định dạng chuyên nghiệp, tập trung vào phương pháp tiếp cận, công cụ và kỹ thuật khai thác thực tế.

## 📂 Cấu trúc Repository

Nội dung được chia làm hai phần chính:

### 1. Báo cáo Lab Core (`LabX.md`)
Các báo cáo tập trung vào các khái niệm nền tảng trong an toàn thông tin:
- **Truy cập ban đầu (Initial Access)**: Tấn công đoán mật khẩu (Password Guessing), tấn công từ điển và Password Spraying.
- **Payloads & C2**: Sử dụng các framework mạnh mẽ như Metasploit, Sliver, và Empire.
- **Nhận thức tình huống (Situational Awareness)**: Dò quét hệ thống và trinh sát mạng nội bộ.
- **Leo thang đặc quyền (Privilege Escalation)**: Vượt qua UAC, khai thác dịch vụ hệ thống và thao tác với token.

### 2. Series Write-up DC (`DC-X.md`)
Hướng dẫn chi tiết cách khai thác các máy ảo dễ bị tổn thương thuộc **Series DC** từ VulnHub:
- **DC-1 đến DC-9**: Lộ trình khai thác toàn diện bao gồm Drupal CMS, WordPress, leo thang đặc quyền Linux (SUID, git, vi), và các cuộc tấn công Kerberos (Vé vàng - Golden Ticket).

---

## 🛠️ Công cụ & Kỹ thuật đã sử dụng

Các báo cáo minh họa việc sử dụng thành thạo nhiều công cụ bảo mật tiêu chuẩn:

- **Trinh sát**: `nmap`, `netdiscover`, `cewl`, `dirb`.
- **Khai thác**: `Metasploit Framework`, `searchsploit`, `sqlmap`, `Impacket`.
- **Điều khiển (C2)**: `Sliver`, `PowerShell Empire`, `Meterpreter`.
- **Tấn công xác thực**: `Hydra`, `John the Ripper`, `Hashcat`, `Responder`.
- **Hậu khai thác**: `PowerUp`, `SharpWMI`, `Mimikatz`, `SecretsDump`.

---

## 🌟 Điểm nổi bật

- **Định dạng chuyên nghiệp**: Tối ưu hóa cho việc đọc với hệ thống đề mục rõ ràng và có Mục lục (Table of Contents) tự động.
- **Tài sản thống nhất**: Toàn bộ ảnh minh họa được tập trung vào thư mục `images/` để dễ quản lý.
- **Đảm bảo riêng tư**: Mọi thông tin định danh cá nhân và mã sinh viên đã được loại bỏ hoàn toàn sạch sẽ.

---

> [!NOTE]
> Các báo cáo này chỉ phục vụ mục đích học tập và nghiên cứu đạo đức. Mọi hoạt động kiểm thử đều được thực hiện trong môi trường Lab hoặc trên các mục tiêu được cho phép.
