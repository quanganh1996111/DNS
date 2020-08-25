# Tìm hiểu về câu lệnh dig trong linux

## 1. Dig là gì ?

**DIG** là iết tắt của Domain Information Groper là một công cụ dòng lệnh quản trị mạng được dùng để truy vấn DNS name servers. Với lệnh `dig`, bạn có thể truy vấn thông tin về các bản ghi DNS, bao gồm: host addresses, mail exchanges, và name servers. Đây là công cụ được các quản trị viên hệ thống sử dụng phổ biến nhất để khắc phục sự cố DNS vì tính linh hoạt và dễ sử dụng của nó.

## 2. Cài đặt dig trên Linux

- Cài đặt trên Ubuntu:

`apt-get install dnsutils`

- Cài đặt trên CentOS7:

`yum install bind-utils`

- Kiểm tra version của `dig`:

```
dig -v
DiG 9.11.4-P2-RedHat-9.11.4-16.P2.el7_8.6
```

## 3. Cú phát sử dụng câu lệnh dig

`dig [server] [name] [type]`

Trong đó:

- `[server]` – địa chỉ IP hoặc hostname của name server sẽ dùng để thực hiện truy vấn.

    - Nếu bạn cung cấp cho đối số server thông tin về hostname thì nó sẽ giải quyết hostname trước khi tiếp tục truy vấn name server.

    - Đây là tùy chọn nên bạn cũng có thể không khai báo ở đây, trong trường hợp không khai báo thì `dig` sẽ lấy thông tin này trong file `/etc/resolv.conf`.

- `[name]` – tên của bản ghi resource sẽ được truy vấn.

- `[type]` – loại truy vấn được yêu cầu bởi dig. Nó có thể là 1 trong số các bản ghi: A, MX, SOA,…Nếu không có bản ghi nào được chỉ định thì dig sẽ mặc định đó là bản ghi A.

## 4. Cách sử dụng câu lệnh dig

### 4.1. Truy vấn DNS cơ bản

Ví dụ: Truy vấn tên miền `google.com`

```
[root@anhtq-test ~]# dig google.com

; <<>> DiG 9.11.4-P2-RedHat-9.11.4-16.P2.el7_8.6 <<>> google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 59757
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;google.com.                    IN      A

;; ANSWER SECTION:
google.com.             299     IN      A       172.217.174.206

;; Query time: 66 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Tue Aug 25 13:55:04 +07 2020
;; MSG SIZE  rcvd: 55
```

Kết quả nhận được bao gồm các phần sau:

- Dòng đầu tiên của đầu ra hiển thị version đã cài đặt và truy vấn được gọi. Dòng thứ hai hiển thị các tùy chọn(theo mặc định của câu lệnh).

```
; <<>> DiG 9.11.4-P2-RedHat-9.11.4-16.P2.el7_8.6 <<>> google.com
;; global options: +cmd
```

- Phần tiếp thep, dòng đầu tiên của phần này là tiêu đề, bao gồm opcode và trạng thái của hành động. Trong trường hợp này, trạng thái NOERROR có nghĩa là yêu cầu truy vấn truy vấn DNS không gặp lỗi.

```
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 59757
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1
```

- Phần này được hiển thị theo mặc định chỉ trên các phiên bản mới hơn.

```
;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
```

- Đây là phần mà lệnh `dig` hiển thị truy vấn của chúng ta. Theo mặc định, `dig` sẽ yêu cầu bản ghi A.

```
;; QUESTION SECTION:
;google.com.                    IN      A
```

- Phần trả lời cung cấp cho chúng ta tên miền google.com. trỏ đến địa chỉ IP 172.217.174.206

```
;; ANSWER SECTION:
google.com.             299     IN      A       172.217.174.206
```

- Đây là phần cuối cùng của đầu ra bao gồm số liệu thống kê về truy vấn.

    - Query time: Cho biết thời gian của kết nối tốn bao lâu.

    - SERVER: 8.8.8.8: Cho biết name resolver đang dùng.

    - WHEN: Thời gian thực hiện truy vấn DNS.

    - MSG SIZE rcvd: Kích thước gói tin trả lời truy vấn DNS.

```
;; Query time: 66 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Tue Aug 25 13:55:04 +07 2020
;; MSG SIZE  rcvd: 55
```
 
### 4.2. Truy vấn địa chỉ IP

Để truy vấn ngắn gọn kết quả, ta sử dụng thêm tùy chọn `+short` trong câu lệnh. Kết quả chỉ bao gồm các địa chỉ IP của bản ghi A:

```
[root@anhtq-test ~]# dig amazon.com +short
205.251.242.103
176.32.103.205
176.32.98.166
```

Để có kết quả chi tiết hơn bằng cách sử dụng các tùy chọn `+noall` và tùy chọn `+answer`:

```
[root@anhtq-test ~]# dig amazon.com +noall +answer

; <<>> DiG 9.11.4-P2-RedHat-9.11.4-16.P2.el7_8.6 <<>> amazon.com +noall +answer
;; global options: +cmd
amazon.com.             29      IN      A       205.251.242.103
amazon.com.             29      IN      A       176.32.103.205
amazon.com.             29      IN      A       176.32.98.166
```

### 4.3. Truy vấn bản ghi MX cho tên miền

Để truy vấn các bản ghi MX, ta chỉ việc thêm tùy chọn `MX` vào câu lệnh. Sử dụng thêm tùy chọn `+noall` và `+answer` để làm ngắn gọn kết quả in ra:

```
[root@anhtq-test ~]# dig MX google.com +noall +answer

; <<>> DiG 9.11.4-P2-RedHat-9.11.4-16.P2.el7_8.6 <<>> MX google.com +noall +answer
;; global options: +cmd
google.com.             320     IN      MX      30 alt2.aspmx.l.google.com.
google.com.             320     IN      MX      40 alt3.aspmx.l.google.com.
google.com.             320     IN      MX      10 aspmx.l.google.com.
google.com.             320     IN      MX      50 alt4.aspmx.l.google.com.
google.com.             320     IN      MX      20 alt1.aspmx.l.google.com.
```

### 4.4. Truy vấn bản ghi SOA cho tên miền

Tùy chọn truy vấn cũng giống như truy vấn bản ghi `MX`, ta thêm tùy chọn `SOA` vào câu lệnh:

```
[root@anhtq-test ~]# dig SOA google.com +noall +answer

; <<>> DiG 9.11.4-P2-RedHat-9.11.4-16.P2.el7_8.6 <<>> SOA google.com +noall +answer
;; global options: +cmd
google.com.             20      IN      SOA     ns1.google.com. dns-admin.google.com. 328106856 900 900 1800 60
```

### 4.5. Truy vấn Bản ghi TTL cho tên miền

```
[root@anhtq-test ~]# dig TTL google.com +noall +answer

; <<>> DiG 9.11.4-P2-RedHat-9.11.4-16.P2.el7_8.6 <<>> TTL google.com +noall +answer
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 33815
;; flags: qr rd ra ad; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;TTL.                           IN      A

;; AUTHORITY SECTION:
.                       86395   IN      SOA     a.root-servers.net. nstld.verisign-grs.com. 2020082500 1800 900 604800 86400

;; Query time: 41 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Tue Aug 25 14:17:55 +07 2020
;; MSG SIZE  rcvd: 107

google.com.             299     IN      A       172.217.174.206
```

### 4.6. Tra cứu DNS ngược

Để truy vấn tên máy chủ được liên kết với một địa chỉ IP cụ thể, hãy sử dụng tùy chọn `-x`.

```
[root@anhtq-test ~]# dig -x 172.217.174.206 +noall +answer

; <<>> DiG 9.11.4-P2-RedHat-9.11.4-16.P2.el7_8.6 <<>> -x 172.217.174.206 +noall +answer
;; global options: +cmd
206.174.217.172.in-addr.arpa. 20847 IN  PTR     hkg07s34-in-f14.1e100.net.
```