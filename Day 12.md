# LAN Security và Device Hardening 

## Nội dung chính trong ngày 12

- Cấu hình Port Security, giảm thiểu các mỗi đe dọa đến LAN.

## Cấu hình Port Security
- Nếu biết thiết bị nên dùng cáp nào và kết nối cụ thể đến interface nào trên một Switch, có thể dùng Port Security để hạn chế interface đó để chỉ có một số thiết bị có thể dùng. Điều này giúp giảm khả năng tiếp xúc với một số kiểu tấn công mà tin tặc kết nối một laptop đến một socket hoặc dùng cáp gắn vào thiết bị cuối để truy cập vào mạng
- Cấu hình Port Security gồm nhiều bước. Cơ bản cần đặt một access port (không dùng VLAN, trunking), sau đó bật Port Security và cấu hình địa chỉ MAC của thiết bị được truy cập vào Port đó
1. Cấu hình interface chế độ truy cập tĩnh **switchport mode access**
2. Bật Port Security **switchport port-security**
3. Ghi đè số lượng giới hạn của địa chỉ MAC được cho phép với interface ** switchport port-security maximum number**
4. Ghi đè hoạt động mặc định khi có xâm nhập **switchport port-security violation {protect | restrict | shutdown}**
5. Xác định trước có MAC nguồn nào được cho phép xử dụng interface này **switchport port-security mac-address mac-address**
6. Thay vì bước 5. cấu hình interface tự động học và cấu hình địa chỉ MAC của kết nối hiện thời **switchport port-security mac-address sticky**

- Khi một thiết bị trái phép cố gắng gửi các frame đến interface của Switch, Switch có thể đưa ra các thông báo cung cấp thông tin, hủy các frame từ các thiết bị đó, hoặc thậm chí hủy tất cả các frame khỏi các thiết bị bằng các tắt interface

|Tùy chọn lệnh **switchport port-security violation** |protect|restrict|shutdown|
|:-------------:|:-------------:|:-------------:|:-------------:|
|Hủy bỏ lưu lượng xâm nhập |Có|Có|Có|
|Gửi log và thông báo SNMP|Không|Có|Có|
|Tắt interface, hủy mọi lưu lượng|Không|Không|Có|

![alt text](https://i.imgur.com/26WY2o9.png)

- Cấu hình Port Security cho mỗi interface chỉ cho phép 3 địa chỉ MAC. Nếu có địa chỉ MAC thứ 4, chỉ thiết bị với lưu lượng đó bị hủy. Nếu lựa chọn violation không rõ ràng, lưu lượng của thiết bị được cho phép trên port cũng bị hủy vì port mặc định ở shutdown

## Khôi phục lại Port sau một cuộc tấn công
- Khi Port Security được bật ở một interface, mặc định sẽ shutdown port đó.
- Xử lý bằng 2 cách:
 - Giới hạn địa chỉ MAC cần bảo mật được thêm vào bảng địa chỉ của interface đó, và 1 bảng địa chỉ không trong bảng cho phép truy cập vào interface đó
 - Một địa chỉ được học hoặc được cấu hình trên một interface bảo mật được thấy bởi một interface bảo mật khác trong cùng VLAN
- Khi xảy ra xâm nhập, một thông báo syslog được gửi đến console là interface đó đang ở trạng thái **err-disable**

## Giảm thiểu mối đe dọa đến LAN
### DHCP Snooping
- Đây là nền tảng cấu hình bảo mật của Switch để phòng chống man-in-the-middle

![alt text](https://i.imgur.com/iHW9rAv.png)

- R1 được cấu hình để trả lời yêu cầu DHCP đến DHCP Server gắn với R2
- Tuy nhiên DHCP Server giả mạo gắn ở SW1 trả lời yêu cầu DHCP từ PC1 trước. PC1 đồng ý địa chỉ IP đừa Server này, đặt DHCP Server này làm default gateway
- Để phòng trừ kiểu tấn công này, DHCP Snooping dùng khá niệm trusted và untrusted port

![alt text](https://i.imgur.com/CsDeBHs.png)
- Các tính năng quan trọng trong cấu hình DHCP Snooping:
 - **Trusted ports**: Port được tin cậy cho phép tất cả các tin DHCP
 - **Untrusted ports, server messages**: Port không tin cậy hủy mọi tin đến được cho rằng từ Server gửi
 - **Untrusted ports, client messages**: Port không tin cậy áp dụng logic phức tạp hơn cho các thông báo được cho là tin từ Client. Chúng kiểm tra mỗi tin DHCP đến xung đột với bảng DHCP có sẵn
 - **Rate limiting**: Tính năng này giới hạn số lượng tin DHCP nhận mỗi giây, mỗi port
### Sửa đổi VLAN gốc và quản lý VLAN
- IEEE 802.1Q xác định một VLAN gốc để duy trì khả năng tương thích ngược với các lưu lượng không được gắn thẻ. Một VLAN gốc đóng vai trò như một định danh chung trên các đầu cuối của một liên kết trunk 
- Nên cấu hình 1 VLAN khác biệt với VLAN 1 và các VLAN khác 

![alt text](https://i.imgur.com/o7jHDf4.png)
- Tạo một VLAN gốc để quản lý, kích hoạt interface VLAN 86. Cấu hình trunk và đặt VLAN 86 là native
### Switch Port Hardening
- Interface của Router phải được kích hoạt với câu lệnh **no shutdown** trước khi có thể hoạt động. Thiết bị Cisco chọn cấu hình mặc định bao gồm cho phép interface hoạt động mà không cần cấu hình, điều này dẫn đến đe dọa về bảo mật
 - Tắt các interface với câu lệnh **shutdown**
 - Đề phòng kết nối Trunk bằng cách dùng lệnh **switchport mode**
 - Chỉ định port đến 1 VLAN chưa được dùng với câu lệnh **switchport access vlan number** 
 - Đặt VLAN gốc khác VLAN 1, nhưng thay vì dùng VLAN chưa được dùng, dùng câu lệnh **switchport trunk native vlan vlan-id**
- Cách tốt nhất là dùng "black hole" VLAN, đặt tất cả các port không dùng vào VLAN đóng
### AAA
- Cấu hình tên đăng nhập và mật khẩu trên tất cả các thiết bị mạng khó có thể mở rộng. Các tốt nhất là dùng một server lưu trữ các tên đang nhập và mật khẩu. Để giải quyết vấn đề này, thiết bị Cisco hỗ trợ Authentication, Authorization, Accounting (AAA) để giúp bảo mật khi truy cập thiết bị
- 2 giao thức xác thực được hỗ trợ trên các thiết bị Cisco:
 - Terminal Access Controller Access-Control System Plus (TACACS+)
 - Remote Authentication Dial-In User Service (RADIUS)
- Chọn TACACS+ hay RADIUS phụ thuộc vào doanh nghiệp. Một ISP lớn nên dùng RADIUS vì nó hỗ trợ chi tiết tài khoản cho các người dùng trả phí. Các doanh nghiệp với các nhóm người dùng nên dùng TACACS+ vì nó yêu cầu chính sách ủy quyền để áp dụng vào mỗi người dùng, mỗi nhóm
### 802.1X
- IEEE 802.1X là giao thức xác thực và kiểm soát truy cập dựa trên port

![alt text](https://i.imgur.com/w3PAKuI.png)
- **Client (supplicant)**: Đây thường là port 8202.1X kích hoạt trên các thiết bị yêu cầu truy cập LAN và dịch vụ của Switch và trả lời yêu cầu từ Switch
- **Switch (authenticator)**: Switch kiểm soát truy cập vật lý và mạng. Switch hoạt động như 1 proxy giữa client và server xác thực
- **Authentication server**: Server xác thực hoạt động để xác thực thông tin của client xác nhận danh tính của Client và thông báo cho Switch là client có quyền truy cập

![alt text](https://i.imgur.com/diy3Z3J.png)

- Quá trình xử lý 802.1X
 - Server xác thực RADIUS được cấu hình với tên đăng nhập và mật khẩu
 - Switch trong LAN kích hoạt xác thực 802.1X, được cấu hình với địa chỉ IP của Server xác thực và có 802.1X kích hoạt trên tất cả các port yêu cầu
 - Người dùng kết nối với thiết bị qua port dùng 802.1X phải biết tên đăng nhập và mật khẩu trước khi truy cập vào mạng
 
### Cấu hình SSH

- Secure Shell (SSH) được khuyên dùng như một giao thức dùng để quản lý thiết bị từ xa

![alt text](https://i.imgur.com/hnsQkMq.png)
1. Kiểm tra xem Switch có hỗ trợ SSH không **show ip ssh**
2. Cấu hình một DNS **ip domain-name**
3. Cấu hình **crypto key generate rsa** để tạo 1 khóa RSA
4. Đổi đường vty để dùng tên đăng nhập 
5. Cấu hình Switch chỉ cho phép kết nối SSH **transport input ssh**
6. Thêm 1 hoặc nhiều tên đăng nhập, mật khẩu
7. Có thể thay đổi SSH thành phiên bản 2.0
8. Kiểm tra SSH bằng câu lệnh **show ip ssh**
