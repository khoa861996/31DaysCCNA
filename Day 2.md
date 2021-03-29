# Giám sát, quản lý, bảo trì thiết bị

## Nội dung chính trong ngày 2

- Công cụ giám sát, quản lý, bảo trì Router và Switch. Cấu hình giám sát thiết bị Simple Network Management Protocol (SNMP), syslog và Network Time Protocol (NTP).

## SNMP
- SNMP là giao thức lớp application cung cấp định dạng tin để giao tiếp giữa quản lý các đại lý

### Thành phần SNMP
 - SNMP manager
 - SNMP agents (managed node)
 - Management Information Base (MIB)
 
### SNMP Messages

- Người quản lý SNMP là một phần của hệ thống quản lý (NMS) và dùng phần mềm quản lý SNMP. SNMP agents quản lý thiết bị. MIB lưu giá trị SNMP.

### SNMP Versions
- **SNMPv1**
- **SNMPv2c** 
- **SNMPv3**
- SNMPv1 và SNMPv2 dùng community strings để kiểm soát truy cập vào MIB
 - **Read-only (ro)**: Cung cấp truy cập vào MIB nhưn không cho phép thay đổi các giá trị này
 - **Read-write (rw)**: Cung cấp đọc và viết trong MIB
 
### The Management Information Base
- MIB tổ chức giá trị theo phân cấp

![alt text](https://i.imgur.com/q8KkCLp.png)

## Cấu hình SNMP
1. Cấu hình community strings và access level **snmp-server community string {RO|RW}**
2. Ghi lại location của thiết bị **snmp-server location text-describing-location **
3. Ghi lại location của thiết bị ** snmp-server contact contact-name**
4. Hạn chế truy cập SNMP đến NMS host được đồng ý bằng ACL **snmp-server community string acl**

## Syslog
### Syslog Operation
- Dùng UDP port 514 để gửi thông báo qua IP để thu thập tin

![alt text](https://i.imgur.com/0gmSbpB.png)

- Dịch vụ Syslog gồm 3 khả năng:
 - Thu thập thông tin nhật ký (log) để giám sát và xử lý lỗi
 - Chọn kiểu thông tin log được ghi lại
 - Chỉ định đích của các thông báo được ghi lại

## Network Time Protocol
- Hầu hết các log có danh sách ngày và giờ là một phần của tin 
- NTP cung cấp khả năng đồng bộ thời gian

## Cisco IOS File System and Devices
- Thiết bị Cisco cung cấp tính năng gọi là Cisco IOS Integrated File System (IFS).Hệ thống này cho phép tạo, điều hướng và thao tác các thư mục của thiết bị Cisco
### URL Prefixes for Specifying File Locations
![alt text](https://i.imgur.com/MaXbKBQ.png)

- tftp: là tiền tố xác định giao thức
- Sau (//) là vị trí file
- 192.168.20.254 là vị trí của Server TFTP
- Cấu hình thư mục chính của Server TFTP
- backup-config là file ví dụ
