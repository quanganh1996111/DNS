# Chuyển đổi NameServer sang CloudFlare

## Các bước thực hiện

### Bước 1: Đăng ký tài khoản CloudFlare

Để đăng ký tài khoản CloudFlare, ta truy cập vào trang: https://www.cloudflare.com/a/sign-up

### Bước 2: Nhập Domain mà ta muốn đăng ký lên CloudFlare

<img src="https://imgur.com/SkME7Bw.png">

### Bước 3: Thêm các bản ghi DNS vào CloudFlare

Tại đây có thể cấu hình thiết lập các record DNS đã có sẵn sau khi quá trình scan hoàn tất hoặc thêm các record DNS khác cho domain cũng như cho phép các record DNS của domain tương ứng phân giải thông qua CloudFlare (fake IP) và chọn Continue.

<img src="https://imgur.com/1eDkAvA.png">

Một số Trạng thái cần chú ý:

<img src="https://imgur.com/UmzX3Ps.png">

### Bước 3: Tiến hành thay đổi Nameserver sang Nameserver của CloudFlare

Thông báo thay đổi Nameserver mặc định sang Nameserver của CloudFlare

<img src="https://imgur.com/2FfMX70.png">

<img src="https://imgur.com/T0MmlLn.png">

### Bước 4: Truy cập vào trang quản lý tên miền cũ để thay đổi Nameserver

Truy cập vào https://zonedns.vn/ và đăng nhập với tên miền muốn thay đổi

<img src="https://imgur.com/EDeNGFa.png">

### Bước 5: Xác nhận bên CloudFlare đã thay đổi Nameserver

Ta nên đợi một lúc để các thay đổi được hoàn thành

<img src="https://imgur.com/ItsnRfH.png">

## Kết quả nhận được

Thông báo CloudFlare xác nhận đã cập nhật tên miền thành công

<img src="https://imgur.com/Dtwu3D4.png">

Kiểm tra các bản ghi DNS và Nameserver của CloudFlare

<img src="https://imgur.com/dp4L77p.png">

### Chú ý

Đối với các Webserver đang sử dụng tên miền ta vừa trỏ về CloudFlare, ta cũng phải cấu hình trỏ về Nameserver của CloudFlare

Ví dụ: Cấu hình trỏ tên miền về Nameserver CloudFlare trên Linux

`vi /etc/resolv.conf`

```
nameserver autumn.ns.cloudflare.com
nameserver morgan.ns.cloudflare.com
```

### Chúc các bạn thành công