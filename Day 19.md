# Single-Area OSPF

## Nội dung chính trong ngày 19

- Triển khai OSPFv2 và OSPFv3 cho thiết kế mạng kiểu single-area

## Cấu hình Single-Area OSPFv2
- Sử dụng Cisco Packet Tracer (Đính kèm)
- Kích hoạt bằng câu lệnh **router ospf *process-id* **
 - R1(config)# router ospf process-id
### Router ID
1. Router dùng địa chỉ IP được cấu hình với câu lệnh **router-id**
2. Nếu Router ID không được cấu hình, Router chọn địa chỉ IP cao nhất của mọi loopback interface
3. Nếu không có loopback interface, Router chọn địa chỉ IP cao nhất đang hoạt động của tất cả các interface vật lý
- Vì người quản trị có thể kiểm soát được **router-id** và vì loopback interface gây lộn xộn bảng định tuyến, tốt nhất là nên cấu hình **router-id**.
### Câu lệnh Network
**Router(config-router)# network network-address wildcard-mask area area-id**
- Là tổng hợp của *network-address* và *wildcard-mask*
- *Wildcard mask* là cấu hình tùy chỉnh như là đảo ngược của subnetmask
- **area *area-id* ** là câu lệnh tập hợp tất cả các Router chia sẻ cùng link-state. Thông thường area là 0
- Một cách thay thế khác, OSPFv2 có thể dùng bở câu lệnh ** network *intf-ip-address* *0.0.0.0 area* area-id**
### Passive Interfaces
- Mặc định thông báo của OSPF truyền qua tất cả các interface OSPF
- Dùng câu lệnh **passive-interface** để ngăn gửi OSPF cập nhật qua các interface

## Cấu hình Single-Area OSPFv3
- Tương tự OSPFv2
- Sử dụng Cisco Packet Tracer (Đính kèm)