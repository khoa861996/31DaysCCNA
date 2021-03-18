# Cấu hình định tuyến mặc định và định tuyến tĩnh 

## Nội dung chính trong ngày 23

- Cấu hình static và default route cho IPv4 và IPv6. Static route có thể dùng để đẩy lưu lượng theo một đường cụ thể hoặc kết nối default route ra ngoài. Người quản trị mạng cấu hình cứng static route vào bảng định tuyến và phải giám sát duy trì để đảm bảo kết nối

## Default Route và Static Route
- Định tuyến tĩnh (Static Route) được xử dụng trong:
 - Một mạng nhỏ cần định tuyến đơn giản
 - Topo mạng hub-and-spoke
 - Khi muốn tạo một định tuyến nhanh chóng
 - Như một định tuyến dự phòng

## Cấu hình Static Route IPv4
**Router(config)# ip route network-address subnet-mask {ip-address | exit-interface} [administrative-distance]**
- Trong đó: 
 - **network-address**: Địa chỉ đích của mạng truy nhập từ xa được thêm vào bảng định tuyến  
 - **subnet-mask:**: SubnetMask của mạng truy nhập từ xa được thêm vào bảng định tuyến
 - **ip-address**: Thường là địa chỉ IP của Router tiếp theo
 - **exit-interface**: Interface dùng để chuyển tiếp gói tin đến mạng đích 
 
- Dùng Cisco Packet Tracer (Đính kèm)

![alt text](https://i.imgur.com/Wr4kNGf.png)
- R1 không biết các mạng 
 - 172.16.1.0/24: LAN trên R2
 - 192.168.0.0/24: Kết nối mạng serial giữa R2 và R3 
 - 192.168.1.0/24: LAN trên R3
 - 10.10.10.0/24: Kết nối mạng serial giữa R2 và HQ 
 - 0.0.0.0/0: Các kết nối mạng khác thông qua HQ
### Cấu hình Static Route IPv4 sử dụng tham số Next-Hop
### Cấu hình Static Route IPv4 sử dụng tham số Exit Interface
### Cấu hình Default Route
- Default Route là một kiểu định tuyến tĩnh đặc biệt đại diện cho mọi định tuyến
- Sau khi định tuyến, R2 hoạt động như Router hub của R1 và R3 vì thế nó cần 2 định tuyến tĩnh chỉ đến R1 và R3
- Mạng 172.16 và 192.168 có thể tổng hợp lại một tuyến
 - Mạng 172.16 (192.168 tương tự):
  - 172.16.1.0: 10101100.00010000.00000001.00000000
  - 172.16.2.0: 10101100.00010000.00000010.00000000
  - 172.16.2.0: 10101100.00010000.00000011.00000000
  
  => Tổng hợp lại có 22 bit giống nhau cho mạng 172.16 => 172.168.0.0/22

## Cấu hình Static Route IPv6
**Router(config)# ipv6 route ipv6-prefix/prefix-length {ipv6-address | exit-interface} [administrative-distance]**

[Static,default route( IPv4,IPv6).zip](https://github.com/khoa861996/31DaysCCNA/files/6165539/Static.default.route.IPv4.IPv6.zip)
