# Cấu hình cơ bản cho Switch

## Nội dung chính trong ngày 29
- Tìm hiểu cơ bản Cisco IOS, các câu lệnh cơ bản để cấu hình Switch
- Tìm hiểu các kỹ thuật xác minh như **ping**,**traceroute**,**show**

## Accessing and Navigating the Cisco IOS
### Kết nối với thiết bị Cisco
![alt text](https://i.imgur.com/XfsQ9mU.png)
- Có thể truy cập vào các thiết bị này trực tiếp hoặc kết nối từ xa
- 2 cách để cấu hình thiết bị Cisco:
 - **Console terminal**: dùng cáp 2 đầu RJ-45 và máy tính sử dụng phần mềm để giao tiếp với thiết bị(Hyperterminal, Tera Term) để thiết lập liên kết trực tiếp. Hoặc sử dụng miniUSB nếu có
 - **Remote terminal**: dùng modem bên ngoài kết nối với cổng phụ của Router để cấu hình từ xa
- Sau khi cấu hình, có thể truy cập thiết bị qua 3 phương thức:
 - Kết nối với thiết bị đầu cuối sử dụng Telnet
 - Cấu hình thiết bị qua kết nối hiện thời, hoặc tải cấu hình có sẵn từ TFTP server trong mạng lưới
 - Tải file cấu hình sử dụng phần mềm quản trị mạng như CiscoWorks
### Các phiên CLI EXEC
- Cisco IOS chia phiên EXEC ra 2 cấp truy cập cơ bản:
 - **User EXEC mode**: Truy cập 1 số câu lệnh giám sát và kiểm tra lỗi cơ bản (ping,show)
 - **Privileged EXEC mode**: Toàn quyền truy cập để cấu hình và quản trị thiết bị
### Câu lệnh hiện lịch sử
- Cisco IOS lưu 10 câu lệnh được cấu hình mới nhật vào bộ đệm
| Cú pháp           | Mô tả        |
|:-------------:|:-------------:|
| switch# show history |Hiện các câu lệnh lưu trong bộ đệm|      
| switch# terminal history  |  Bật chế độ lịch sử  |
| switch# terminal history size 50             |Cấu hình số lượng câu lệnh lưu lại trong bộ đệm|
| switch# terminal no history size     |Đặt lại mặc định số câu lệnh được lưu  |
| switch# terminal no history       | Tắt chế độ lưu lịch sử |

### Câu lệnh kiểm tra IOS
![alt text](https://i.imgur.com/wk7xXDQ.png)
- Câu lệnh dùng cho Cisco IOS lưu trong RAM
- Câu lệnh áp dụng vào file cấu hình dự phòng lưu ở NVRAM
- Câu lệnh áp dụng vào Flash hoặc một giao diện riêng nào đó
### Truy cập chế độ cấu hình con
- Để truy cập chế độ cấu hình chung, nhập **configure terminal**. Cisco IOS cung cấp nhiều chế độ cấu hình con khác nhau

| Prompt           | Tên chế độ       |Câu lệnh truy cập|
|:-------------:|:-------------:|----------|
| hostname(config)#       |Global|configure terminal|
| hostname(config-line)#  |  Line  |line console 0, line vty 0 15|
| hostname(config-if)#             |Interface|interface fastethernet 0/0|
| hostname(config-router)#     | Router     |router rip, router eigrp 100,...|

## Cấu hình Switch cơ bản

| Mô tả | Cú pháp        |
|:-------------:|:-------------:|
|Truy cập chế độ cấu hình chung|Switch# configure terminal|
|Đặt tên cho thiết bị|Switch(config)# hostname S1|
|Truy cập giao diện cấu hình VLAN 1|S1(config)# interface vlan 1|
|Cấu hình địa chỉ IP|S1(config-if)# ip address 172.17.99.11 255.255.255.0|
|Bật giao diện|S1(config-if)# no shutdown|
|Thoát về chế độ cấu hình chung|S1(config-if)# exit|
|Nhập giao diện cổng để đặt VLAN|S1(config)# interface fastethernet 0/6| 
|Xác định VLAN là thành viên cho port |S1(config-if)# switchport mode access |
|Gán port cho VLAN|S1(config-if)# switchport access vlan 123|
|Cấu hình chế độ song công tự động|S1(config-if)# duplex auto|
|Cấu hình chế độ tốc độ tự động|S1(config-if)# speed auto|
|Bật auto-MDIX|S1(config-if)# mdix auto|
|Đặt gateway mặc định|S1(config)# ip default-gateway 172.17.50.1|
|Cấu hình máy chủ HTTP để kích hoạt sử dụng mật khẩu|S1(config)# ip http authentication enable|
|Bật máy chủ HTTP|S1(config)# ip http server|
|Chuyển sang chế độ cấu hình đường truyền|S1(config)# line console 0|
|Đặt mật khẩu "cisco"|S1(config-line)# password cisco|
|Yêu cầu mật khẩu đăng nhập|S1(config-line)# login|
|Thoát về chế độ cấu hình chung|S1(config-if)# exit|
|Chuyển sang chế độ cấu hình đường truyền cho các thiết bị vty 0-15|S1(config)# line vty 0 15|
|Đặt mật khẩu "cisco"|S1(config-line)# password cisco|
|Yêu cầu mật khẩu đăng nhập đường truyền vty|S1(config-line)# login|
|Thoát về chế độ cấu hình chung|S1(config-line)# exit|
|Đặt mật khẩu "cisco" để truy cập chế độ EXEC|S1(config)# enable password cisco|
|Cấu hình class để bật chế độ mật khẩu khi truy cập chế độ EXEC|S1(config)# enable secret class|
|Mã hóa tất cả các mật khẩu|S1(config)# service password-encryption|
|Đặt khẩu hiệu đăng nhập với tin nhắn(phải có # đầu và cuối)|S1(config)# banner motd #Device maintenance will be occurring on Friday!#|
|Trở về chế độ EXEC|S1(config)# end|
|Lưu cấu hình|S1# copy running-config startup-config|

## Bán song công, song công, và tốc độ Port
- Giao tiếp bán song công là 1 luồng dữ liệu đơn hướng trong đó thiết bị có thể gửi hoặc nhận trên mạng LAN Ethernet nhưng khong phải cùng một lúc
- Các thiết bị mạng LAN và các NIC hoạt động song công khi thiết bị còn được kết nối với thiết bị khác tương thích với giao tiếp song công. Giao tiếp song công tăng cường hiệu quả của băng thông bằng cách cho phép truyền và nhận dữ liệu cùng lúc, không bị xung đột. Gigabit Ethernet và 10-Gbps NIC yêu cầu kết nối song công để hoạt động. Tốc độ port: 100 Mbps, 1 Gbps, 10Gbps.
### Automatic Medium-Dependent Interface Crossover(auto-MDIX)
- Kích hoạt auto-MDIX sẽ tự động nhận diện loại cáp cho các kiểu kết nối và cấu hình phù hợp cho chúng
- Khi sử dụng auto-MDIX: tốc độ và chế độ song công phải được để tự động

## Kiểm tra kết nối mạng
- Sử dụng công cụ kiểm tra để xác định nguyên nhân xảy ra kết nối mạng. Câu lệnh ping có thể kiểm tra có hệ thống kết nối theo 3 cách thức.
 - Thiết bị đầu cuối có thể tự ping không?
 - Thiết bị đầu cuối có thể ping đến default gateway không
 - Thiết bị đầu cuối có thể ping đến đích không?
- Kết nối nội bộ sẽ không có lỗi khi thiết bị đầu cuối có thể ping đến default gateway. Dùng lệnh traceroute có thể giúp cô lập điểm trong đường dẫn từ nguồn đến đích khi lưu lượng bị dừng

![alt text](https://i.imgur.com/mi5y1JZ.png)
- Kiểm tra hoạt động của TCP/IP trong nội bộ bằng cách ping đến địa chỉ loopback (127.0.0.1)
- Ping đến địa chỉ 127.0.0.1 sẽ thành công bất kể host có kết nối mạng hay không

![alt text](https://i.imgur.com/QDq7Yxn.png)
- Kiểm tra kết nối đến default gateway
- Nếu không ping được có thể do một số vấn đề, mỗi vấn đề phải được kiểm tra theo một trình tự có hệ thống.
1. Cáp kết nối từ PC đến Switch có chính xác không? Đèn liên kết có sáng không?
2. Cấu hình mạng cho PC có đúng theo logic của mạng không?
3. Các giao diện kết nối của Switch dẫn đến sự cố này? Không khớp khi ghép kênh, tốc độ, auto-MDIX không giống nhau? Cấu hình VLAN sai?
4. Cáp kết nối từ Switch đến Router có chính xác không? Đèn liên kết có sáng không?
5. Cấu hình trên Router có đúng không?

![alt text](https://i.imgur.com/ah0WalU.png)
- Kiểm tra kết nối đến đích
- Nếu có lỗi ở đây thì sẽ là lỗi bên ngoài default gateway vì đã ping thành công đến default gateway

![alt text](https://i.imgur.com/MJpY6Vr.png)
- Kiểm tra điểm bị đứt đoạn trong đường dẫn bằng câu lệnh **tracert**

## Kiểm tra lỗi của giao diện kết nối và cáp nối
- Lớp vật lý thường là lý do dẫn đến lỗi mạng, quá tải, mất kết nối cáp, thiết bị điện, hỏng phần cứng,...
### Lỗi phương tiện
- Thiết bị mới lắp vào gây nhiễu điện từ đến môi trường.
- Cáp được đi quá gần với các loại động cơ mạnh (thang máy,...)
- Quản lý kết nối dây kém sẽ ảnh hưởng đến kết nối RJ-45, khiến dây bị đứt.
- Ứng dụng mới thay đổi lưu lượng.
- Khi thiết bị mới kết nối với Switch, kết nối này sẽ hoạt động ở chế động bán song công hoặc xảy ra không khớp song công, dẫn đến xung đột.

## Trạng thái kết nối giao diện và cấu hình Switch
- Trạng thái "up" hoặc "down"
| Trạng thái đường truyền           | Trạng thái giao thức        | Trạng thái giao diện|Nguyên nhân|
|:-------------:|:-------------:|-------------|-------------|
| Administratively Down    |Down  | disable|Giao diện được cấu hình **shutdown**|
| Down |  Down | notconnect|Không có cáp kết nối, cáp chất lượng thấp, sai cáp, 2 thiết bị không cùng tốc độ,...|
| Up          |Down| notconnect|Một giao diện ở trạng thái up/down không mong muốn trên giao diện của Switch, đây là vấn đề ở L2 trên thiết bị L3 |
| Down  | Down(err-disabled) | err-disabled|Port security bị tắt|
| Up  | Up     | connect|Giao diện đang hoạt động|

### Song công và kết nối khác tốc động
![alt text](https://i.imgur.com/8jDHYtb.png)
- Câu lệnh: **show interfaces status** và **show interfaces**
 - **show interfaces status** hay được dùng để xử lý song công và tốc độ truyền. **a-full** biểu thị là switch thương lượng song công tự động. **a-100** biểu thị Switch tự động thương lượng tốc độ là 100 Mbps.
- Tìm sự khác nhau ở chế độ song công sẽ khó hơn khác nhau về tốc độ. Ở vấn đề này, giao diện vẫn sẽ hoạt động tuy nhiên mạng sẽ hoạt động với hiệu năng thấp, xảy ra vấn đề về giao tiếp.
### Lỗi thường xảy ra ở L1 khi giao diện đang "Up"
| Kiểu lỗi           | Giá trị chỉ ra lỗi       |Nguyên nhân|
|:-------------:|:-------------:|----------|
| Mức độ nhiễu quá mức       |Nhiều đầu vào lỗi, xung đột|Sai cáp nối, hỏng cáp, ...|
| Xung đột  |  Nhiều hơn 1% của tất cả các frame xung đột  |Không cùng chế độ song công|
| Late collision (kích thước mạng quá lớn)     |Tăng late collision|Miền xung đột hoặc cáp quá dài, không cùng chế độ song công|
- Cisco kiểm tra để giúp xác định lỗi có thể xảy ra kể cả khi giao diện này đang được kết nối
