# Các loại bản ghi trong DNS

## Mục lục

## 1. SOA (Start of Authority)

Trong mỗi tập tin cơ sở dữ liệu DNS phải có một và chỉ một record SOA (Start of Authority). Bao gồm các thông tin về domain trên DNS Server, thông tin về zone transfer.

```
$TTL 86400
@       IN SOA  masterdns.tuanda.com.     admin.tuanda.com. (
                                  2014090401    ; serial
                                        3600    ; refresh
                                        1800    ; retry
                                      604800    ; expire
                                       86400 )  ; TTL
```

Trong đó:

- `masterdns.tuanda.com.`:giá trị DNS chính của tên miền hoặc máy chủ.

- `admin.tuanda.com.`: chuyển đổi từ dạng admin@tuanda.com, thể hiện chủ thể sở hữu tên miền này.

- `Serial` : áp dụng cho mọi dữ liệu trong zone và có định dạng YYYYMMDDNN với YYYY là năm, MM là tháng, DD là ngày, NN là số lần sửa đổi dữ liệu zone trong ngày. Luôn luôn phải tăng số này lên mỗi lần sửa đổi dữ liệu zone. Khi máy chủ Secondary liên lạc với máy chủ Primary, trước tiên nó sẽ hỏi số serial. Nếu số serial của máy Secondary nhỏ hơn số serial của máy Primary tức là dữ liệu zone trên Secondary đã cũ và sao đó máy Secondary sẽ sao chép dữ liệu mới từ máy Primary thay cho dữ liệu đang có.

- `Refresh` : chỉ ra khoảng thời gian máy chủ Secondary kiểm tra sữ liệu zone trên máy Primary để cập nhật nếu cần. Giá trị này thay đổi tùy theo tuần suất thay đổi dữ liệu trong zone.

- `Retry` : nếu máy chủ Secondary không kết nối được với máy chủ Primary theo thời hạn mô tả trong refresh (ví dụ máy chủ Primary bị shutdown vào lúc đó thì máy chủ Secondary phải tìm cách kết nối lại với máy chủ Primary theo một chu kỳ thời gian mô tả trong retry. Thông thường, giá trị này nhỏ hơn giá trị refresh).

- `Expire` : nếu sau khoảng thời gian này mà máy chủ Secondary không kết nối được với máy chủ Primary thì dữ liệu zone trên máy Secondary sẽ bị quá hạn. Khi dữ liệu trên Secondary bị quá hạn thì máy chủ này sẽ không trả lời mỗi truy vấn về zone này nữa. Giá trị expire này phải lớn hơn giá trị refresh và giá trị retry.

- `TTL (time to live)` : giá trị này áp dụng cho mọi record trong zone và được đính kèm trong thông tin trả lời một truy vấn. Mục đích của nó là chỉ ra thời gian mà các máy chủ name server khác cache lại thông tin trả lời.

## 2. Name Server

Record tiếp theo cần có trong zone là NS (name server) record. Mỗi name server cho zone sẽ có một NS record. Chứa địa chỉ IP của DNS Server cùng với các thông tin về domain đó.

Ví dụ:

```
cloud365.vn. IN NS ns1.zonedns.vn.
cloud365.vn. IN NS ns2.zonedns.vn.
```

## 3. Record A

Là một record căn bản và quan trọng, dùng để ánh xạ từ một domain thành địa chỉ IP cho phép có thể truy cập website. Đây là chức năng cốt lõi của hệ thống DNS. Record A có dạng như sau:

`cloud365.vn   A    103.101.161.201`

Tên miền subdomain (tên miền con):

`sub.cloud365.vn   A   103.101.161.201`

## 4. Record AAAA

Có nhiệm vụ tương tự như bản ghi A, nhưng thay vì địa chỉ IPv4 sẽ là địa chỉ IPv6.

## 5. Record PTR

Hệ thống tên miền thông thường cho phép chuyển đổi từ tên miền sang địa chỉ IP. Trong thực tế, một số dịch vụ Internet đòi hỏi hệ thống máy chủ DNS phải có chức năng chuyển đổi từ địa chỉ IP sang tên miền. Tên miền ngược thường được sử dụng trong một số trường hợp xác thực email gửi đi.

Ví dụ:

- Đối với IPv4:

`90.163.101.103.in-addr.arpa       IN PTR     masterdns.tuanda.com.`

- Đối với IPv6:

`0.0.0.0.0.0.0.0.0.0.0.0.d.c.b.a.4.3.2.1.3.2.1.0.8.c.d.0.1.0.0.2.ip6.arpa  IN PTR masterdns.tuanda.com.`

## 6. Record SRV

Bản ghi SRV được sử dụng để xác định vị trí các dịch vụ đặc biệt trong 1 domain, ví dụ tên máy chủ và số cổng của các máy chủ cho các dịch vụ được chỉ định.

Ví dụ: 

`_ldap._tcp.example.com. 3600  IN  SRV  10  0  389  ldap01.example.com.`

Một Client trong trường hợp này có thể nhờ DNS nhận ra rằng, trong tên miền example.com có LDAP Server ldap01, mà có thể liên lạc qua cổng TCP Port 389 .

Trong đó:

- **Service**: có giá trị _ldap.

- **Protocol**: có giá trị  _tcp.

- **example.com.**: Tên miền (domain name).

- **TTL**: Thời gian RR được giữ trong cache, giá trị trong ví dụ là 3600.

- **Class**: standard DNS class, luôn là IN.

- **Priority**: ưu tiên của host, số càng nhỏ càng ưu tiên.

- **Weight**: khi cùng bậc ưu tiên, thì trọng lượng 3 so với trọng lượng 2 sẽ được lựa chọn 60% (hỗ trợ load balancing).

- **Port**: của dịch vụ (tcp hay udp).

- **Target**: chỉ định FQDN cho host hỗ trợ dịch vụ.

## Tài liệu tham khảo

https://blog.cloud365.vn/linux/dns-record/