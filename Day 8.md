# Triển khai ACL

## Nội dung chính trong ngày 8

- Cấu hình, kiểm tra, xử lý lỗi ACL

## Cấu hình ACL IPv4 tiêu chuẩn theo số
- Standard IPv4 ACL nằm trong phạm vi 1-99 và 1300-1999 hoặc ACL được đặt tên, lọc dữ liệu dựa trên địa chỉ nguồn và mask
### Deny a Specific Host
### Deny a Specific Subnet
### Deny Telnet or SSH Access to the Router
- Với lưu lượng vào và ra của Router, lọc Telnet hoặc SSH trên Router bằng cách áp dụng ACL vào port vty
- Sử dụng Cisco Packet Tracer (Đính kèm)

## Cấu hình ACL IPv4 mở rộng theo số
- Extended IPv4 ACL nằm trong phạm vi 100-199 và 2000-2699. Extended ACL kiểm tra địa chỉ nguồn và đích
### Deny FTP from Subnets

## Cấu hình ACL IPv4 theo tên
1. Đặt tên ACL **Router(config)# ip access-list standard name**
2. Tạo ACL **Router(config-std-nacl)# [sequence-number] {permit | deny} source source-wildcard**
3. Áp dụng ACL **Router(config-if)# ip access-group name [in | out]**
### Deny a Single Host from a Given Subnet

## Cấu hình ACL IPv6
- Cấu hình giống IPv4 nhưng không bao gồm TCP,UDP
- Áp dụng vào interface dùng

**Router(config-if)# ipv6 traffic-filter access-list-name { in | out }**

## Xử lý lỗi ACL
- ACL có thể chặn ping, traceroute,... 
1. Cấu hình ACL không thể gây lỗi nếu chưa áp dụng lên interface
2. Xác định cấu hình ACL
3. Phân tích ACL để xác định gói tin

[Standard ACL.zip](https://github.com/khoa861996/31DaysCCNA/files/6190543/Standard.ACL.zip)
[Extended ACL.zip](https://github.com/khoa861996/31DaysCCNA/files/6190544/Extended.ACL.zip)
[IPv6 ACL.zip](https://github.com/khoa861996/31DaysCCNA/files/6190546/IPv6.ACL.zip)


