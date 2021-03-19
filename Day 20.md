# OSPF

## Nội dung chính trong ngày 20

- Cơ bản về hoạt động của OSPF. OSPFv2 cho định tuyến IPv4 và OSPFv3 dùng cho định tuyến IPv6

## Single-Area OSPF Operation
### OSPF Message Format
- Phần dữ liệu của một thông báo OSPF được đóng gói vào một gói tin. Trường dữ liệu này gồm một trong 5 kiểu dữ liệu OSPF
- Phần header của gói tin OSPF bao gồm trong tất cả các gói OSPF, bất kể kiểu nào.
### Kiểu gói tin của OSPF
- 5 kiểu gói tin OSPF:
 - **Hello**: Gói tin "Hello" thiết lập và duy trì kết nối lân cận giữa các Router
 - **DBD**: Gói DBD (database description) gồm danh sách ngắn gọn cơ sở dữ liệu link-state của Router gửi. Router nhận dùng nó để kiểm tra
 - **LSR**: Router nhận có thể yêu cầu thông tin trong DBD bằng cách gửi yêu cầu link-state (LSR)
 - **LSU**: Gói tin Link-state update (LSU) trả lời cho LSR và thông báo về thông tin mới. LSU gồm 11 kiểu
 - **LSAck**: khi LSU được nhận, Router gửi một link-state Acknowledgment
### Thiết lập liên kết lân cận 
![alt text](https://i.imgur.com/uxQ90U3.png)

- **Type**: : Hello (Type 1), DBD (Type 2), LS Request (Type 3), LS Update (Type 4), LS ACK (Type 5)
- **Router ID**: ID của Router gốc
- **Area ID**: Khu vực mà bắt nguồn của gói tin
- **Network Mask**: Subnetmask liên kết với interface gửi
- **Hello Interval**: Số giây giữa gửi "hello" giữa các Router
- **Router Priority**: Lựa chọn (DR/BDR)
- **Designated Router (DR)**: ID Router của DR, nếu có
- **Backup Designated Router (BDR)**: ID Router của BDR, nếu có
- **List of Neighbors**: ID Router của Router lân cận

- Gói tin Hello được dùng để:
 - Tìm OSPF kết nối lân cận và thiết lập kết nối
 - Quảng bá tham số mà trên đó 2 Router phải cùng đồng ý để có kết nối
 - Chọn DR và BDR
- Trước khi 2 Router thiết lập kết nối, cả 2 Router phải là một phần của một mạng
### OSPF DR và BDR
- Phương án để quản lý các liên kết lân cận và truyên LSA là Designated Router (DR). Để giảm bớt lưu lượng OSPF trên mạng đa truy nhập, OSPF chọn một DR và một backup DR (BDR). DR có trách nhiệm cập nhật tất cả các Router dùng OSPF khi có thay đổi trong mạng đa truy nhập.
### Thuật toán OSPF
- OSPF sử dụng thuật toán Shortest Path First (SPF). Thuật toán này tính toán *costs* mỗi đường từ nguồn đến đích
### Quá trình định tuyến Link-State
1. Mỗi Router học chính kết nối của nó và kết nối trực tiếp của nó với mạng bằng cách phát hiện interface ở trạng thái "up"
2. Mỗi Router có trách nhiệm kiết nối lân cận với các Router lân cận trên đường kết nối trực tiếp với mạng bằng cách gửi gói hello
3. Mỗi Router xay dựng một Link-State packet (LSP) gồm trạng thái của mỗi kết nối trực tiếp
4. Mỗi Router *flood* LSP đến các Router lân cận.
5. Mỗi Router dùng cơ sở dữ liệu để xây dựng sơ đồ và tính toán đường dẫn tốt nhất đến mạng đích

## OSPFv2 và OSPFv3
|Tính năng|OSPFv2|OSPFv3|
|:-------------:|-------------|-------------|
|Advertising|Mạng IPv4|Tiền tố IPv6|
|Source|Địa chỉ IPv4 nguồn|Địa chỉ IPv6 link-local|
|Destination address|IPv4 unicast lân cận 224.0.0.5, tất cả Router OSPF dùng địa chỉ multicast|IPv6 link-local FF02::5, tất cả các Router dùng địa chỉ multicast|
||224.0.0.6, DR/BDR dùng multicast address|FF02::6, DR/BDR dùng multicast address|
|Advertising networks|Cấu hình bằng câu lệnh **network**|Cấu hình bằng câu lệnh **ipv6 ospf area**|
|IP unicast routing|IPv4 mặc định bật|Bật bằng **ipv6 unicast-routing**|
|Authentication|MD5|IPsec|

## Multiarea OSPF Operation
- Những vấn đề của single-area: 
 - **Large routing tables**: Mặc định OSPF không tổng hợp lại cập nhật định tuyến
 - **Large link-state database (LSDB)**: Mỗi Router phải duy trì một database của tất cả các kết nối đang hoạt động trong miền định tuyến
 - **Frequent SPF calculations**: Trong mạng lớn, sự thay đổi LSDB có thể làm Router sử dụng nhiều CPU để tính toán lại SPF
### Thiết kế Multiarea OSPF 
- Đặt tất cả các interface được kết nối chung một subnet trong cùng area
- Một Area nên được nối tiếp
- Một số Router có có thể nằm trong một Area, với tất cả interface đều nằm trong 1 area
- Một số Router có thể là Area Border Router (ABR) vì một số interface kết nối với lõi area và một số thì không
- Tất cả các area không phải lõi phải được kết nối với area lõi ( area 0) 

![alt text](https://i.imgur.com/F7GRHQO.png)

|Thuật ngữ|Mô tả|
|:-------------:|-------------|
|Area Border Router (ABR)|Một Router dùng OSPF kết nối với area lõi và kết nối với ít nhất một area khác|
|Backbone router|Router kết nối với area lõi|
|Internal router|Router ở 1 area|
|Autonomous System Boundary Router (ASBR)|Router có ít nhất 1 kết nối với mạng bên ngoài|
|Area|Bao gồm các Router và các kết nối chia sẻ cùng LSDB|
|Backbone area|Area đặc biệt mà tất cả các area khác phải kết nối đến (area 0)|
|Intra-area route|Tuyến đường đến một subnet ở trong cùng một area hoạt động như router|
|Interarea route|Tuyến đường đến một subnet trong một area mà Router không nằm trong đó|

### Cải thiện hiệu năng với Multiarea OSPF 
- Area càng nhỏ cần càng ít bộ nhớ
- Router dùng ít CPU để xử lý hơn, tránh quá tải CPU và cải thiện thời gian hội tụ
- Thay đổi trong mạng yêu cầu SPF tính toán ở duy nhất Router được kết nối với area nơi mà kết nối thay đổi, giảm số Router phải chạy lại SPF
- In thông tin phải quảng bá qua lại giữa các area, giảm băng thông cần để gửi LSA