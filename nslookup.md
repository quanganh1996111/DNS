# Tìm hiều nslookup trong Linux

## Mục lục

## 1. Câu lệnh nslookup

`nslookup` (name server lookup) là một công cụ được sử dụng để thực hiện tra cứu DNS trong Linux. Nó được sử dụng để hiển thị chi tiết DNS, chẳng hạn như địa chỉ IP của một máy tính cụ thể, bản ghi MX cho một miền hoặc máy chủ NS của một miền. Về cơ bản câu lệnh `nslookup` khá giống với câu lệnh `dig` (Có thể xem [tại đây](https://github.com/quanganh1996111/DNS/blob/master/dig-command.md)).

`nslookup` có thể được sử dụng với 2 chế độ: *interactive* và *non-interactive*. Chế độ *interactive* cho phép bạn truy vấn máy chủ định danh để biết thông tin về các máy chủ và miền khác nhau hoặc in danh sách máy chủ trong một miền. Chế độ *non-interactive* cho phép bạn chỉ in tên và thông tin được yêu cầu cho máy chủ lưu trữ hoặc miền.

## 2. Chế độ interactive

Chế độ này được sử dụng khi dùng lệnh `nslookup` mà không thêm bất cứ tùy chọn nào:

<img src="https://imgur.com/k0EXjuF.png">

- Để tìm kiếm 1 địa chỉ IP của một domain nào đó, ta chỉ cần gõ domain đó ra:

```
> google.com
Server:         8.8.8.8
Address:        8.8.8.8#53

Non-authoritative answer:
Name:   google.com
Address: 172.217.24.206
Name:   google.com
Address: 2404:6800:4005:805::200e
```

- Để thực hiện việc tra cứu ngược DNS, ta gõ địa chỉ IP:

```
> 172.217.26.142
142.26.217.172.in-addr.arpa     name = hkg12s21-in-f14.1e100.net.
142.26.217.172.in-addr.arpa     name = kul08s06-in-f14.1e100.net.

Authoritative answers can be found from:
```

- Để kiểm tra một bản ghi MX, ta thiết lập tra cứu như sau:

```
> set type=mx
> google.com
Server:         8.8.8.8
Address:        8.8.8.8#53

Non-authoritative answer:
google.com      mail exchanger = 30 alt2.aspmx.l.google.com.
google.com      mail exchanger = 10 aspmx.l.google.com.
google.com      mail exchanger = 50 alt4.aspmx.l.google.com.
google.com      mail exchanger = 20 alt1.aspmx.l.google.com.
google.com      mail exchanger = 40 alt3.aspmx.l.google.com.

Authoritative answers can be found from:
```

- Để kiểm tra một bản ghi NS, ta cũng thiết lập tra cứu:

```
> set type=ns
> google.com
Server:         8.8.8.8
Address:        8.8.8.8#53

Non-authoritative answer:
google.com      nameserver = ns1.google.com.
google.com      nameserver = ns4.google.com.
google.com      nameserver = ns2.google.com.
google.com      nameserver = ns3.google.com.

Authoritative answers can be found from:
```

## 3. Chế độ Non-interactive

Chế độ Non-interactive được sử dụng bằng cách gõ lệnh `nslookup` cùng với domain hoặc địa chỉ IP của máy chủ được tra cứu

```
[root@localhost ~]# nslookup google.com
Server:         8.8.8.8
Address:        8.8.8.8#53

Non-authoritative answer:
Name:   google.com
Address: 172.217.24.206
Name:   google.com
Address: 2404:6800:4005:805::200e
```

```
[root@localhost ~]# nslookup 172.217.24.206
206.24.217.172.in-addr.arpa     name = hkg12s13-in-f14.1e100.net.

Authoritative answers can be found from:
```

- Khác với chế độ interactive, để truy vấn một bản ghi bất kỳ (MX, NS, SOA,...), ta thêm cú pháp `-query=[type]]` vào câu lệnh. Ví dụ:

Tra cứu bản ghi MX:

```
[root@localhost ~]# nslookup -query=mx google.com
Server:         8.8.8.8
Address:        8.8.8.8#53

Non-authoritative answer:
google.com      mail exchanger = 10 aspmx.l.google.com.
google.com      mail exchanger = 50 alt4.aspmx.l.google.com.
google.com      mail exchanger = 40 alt3.aspmx.l.google.com.
google.com      mail exchanger = 20 alt1.aspmx.l.google.com.
google.com      mail exchanger = 30 alt2.aspmx.l.google.com.

Authoritative answers can be found from:
```

Tra cứu bản ghi NS:

```
[root@localhost ~]# nslookup -query=ns google.com
Server:         8.8.8.8
Address:        8.8.8.8#53

Non-authoritative answer:
google.com      nameserver = ns3.google.com.
google.com      nameserver = ns4.google.com.
google.com      nameserver = ns2.google.com.
google.com      nameserver = ns1.google.com.

Authoritative answers can be found from:
```

Tra cứu một bản ghi SOA:

```
[root@localhost ~]# nslookup -query=soa google.com
Server:         8.8.8.8
Address:        8.8.8.8#53

Non-authoritative answer:
google.com
        origin = ns1.google.com
        mail addr = dns-admin.google.com
        serial = 328298009
        refresh = 900
        retry = 900
        expire = 1800
        minimum = 60

Authoritative answers can be found from:
```

Để tra cứu một bản ghi bất kỳ, ta thêm cú pháp `type=any` như sau:

```
[root@localhost ~]# nslookup -query=any google.com
Server:         8.8.8.8
Address:        8.8.8.8#53

Non-authoritative answer:
Name:   google.com
Address: 172.217.26.142
Name:   google.com
Address: 2404:6800:4005:805::200e
google.com      mail exchanger = 30 alt2.aspmx.l.google.com.
google.com      text = "facebook-domain-verification=22rm551cu4k0ab0bxsw536tlds4h95"
google.com      text = "docusign=1b0a6754-49b1-4db5-8540-d2c12664b289"
google.com      nameserver = ns2.google.com.
google.com      nameserver = ns4.google.com.
google.com      text = "docusign=05958488-4752-4ef2-95eb-aa7ba8a3bd0e"
google.com      mail exchanger = 40 alt3.aspmx.l.google.com.
google.com
        origin = ns1.google.com
        mail addr = dns-admin.google.com
        serial = 328298009
        refresh = 900
        retry = 900
        expire = 1800
        minimum = 60
google.com      text = "v=spf1 include:_spf.google.com ~all"
google.com      rdata_257 = 0 issue "pki.goog"
google.com      mail exchanger = 10 aspmx.l.google.com.
google.com      mail exchanger = 50 alt4.aspmx.l.google.com.
google.com      nameserver = ns1.google.com.
google.com      text = "globalsign-smime-dv=CDYX+XFHUw2wml6/Gb8+59BsH31KzUr6c1l2BPvqKX8="
google.com      nameserver = ns3.google.com.
google.com      mail exchanger = 20 alt1.aspmx.l.google.com.

Authoritative answers can be found from:
```

## Tài liệu tham khảo

https://geek-university.com/linux/nslookup-command/