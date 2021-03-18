# Cấu hình Router cơ bản

## Nội dung chính trong ngày 24

- Cấu hình và kiểm tra các cài đặt ban đầu, bao gồm IPv4, IPv6

## Cấu hình cơ bản IPv4
![alt text](https://i.imgur.com/LLM6RHh.png)
- Sử dụng Cisco Packet Tracer (Đính kèm)

### Ý nghĩa các câu lệnh:
- Truy cập chế độ EXEC và vào chế độ cấu hình chung:

 **Router> enable**
 
 **Router# config t**
- Đặt tên Router và đặt mật khẩu mã hóa:

 **Router(config)# hostname R1**
 
 **R1(config)# enable secret class**
 
- Đặt mật khẩu console và yêu cầu phải login

 **R1(config)# line console 0**
 
 **R1(config-line)# password cisco**
 
 **R1(config-line)# login**
 
- Cấu hình SSH cho truy cập để tăng cường bảo mật.

 **R1(config)# line vty 0 15**
 
 **R1(config-line)# transport input ssh**
 
 **R1(config-line)# login local**
 
 **R1(config-line)# exit**
 
 **R1(config)# username cisco password cisco**
 
- Cấu hình các interface của Router với địa chỉ IP và Subnetmask (R1, R2 tương tự)

 **R1(config)# interface Serial0/0/0**
 
 **R1(config-if)# ip address 192.168.2.1 255.255.255.0**
 
 **R1(config-if)# no shutdown**
 
 **R1(config)# interface g0/0**
 
 **R1(config-if)# ip address 192.168.1.1 255.255.255.0**
 
 **R1(config-if)# no shutdown**
 
- Dùng định tuyến tĩnh (Static Route) để kết nối 2 bên Router R1, R2:

 **R1(config)# ip route 192.168.3.0 255.255.255.0 Serial 0/0/0**
 
 **R1(config)# ip route 192.168.1.0 255.255.255.0 Serial 0/0/0**

### Kiểm tra cấu hình
- Các câu lệnh:
 **show running-config**: Kiểm tra tất cả các cấu hình hiện có trên Router
 **show ip route**: Kiểm tra bảng định tuyến
 **show ip interface brief**: Kiểm tra trạng thái của các interface
 **show interface *xxx* **: Kiểm tra một interface cụ thể

![alt text](https://i.imgur.com/0fze7w9.png)

| Hiển thị           | Mô tả        |
|:-------------:|:-------------:|
|GigabitEthernet…is {up|down|administratively down}|Phần cứng của các interface đang ở trạng thái bật, ngắt hoặc người quản trị đã ngắt nó|      
|line protocol is {up | down}|Phần mềm xử lý giao thức đó có dùng được không |
|Hardware |Kiểu phần cứng và địa chỉ|
|Internet address|Địa chỉ IP kèm theo đó là subnetmask|
|MTU|Số lượng đơn vị truyền tải được nhiều nhất của một interface|
|BW|Băng thông của interface|
|DLY|Độ trễ của interface|
|rely|Độ tin cậy của interface|
|load|Tải của interface|
|Encapsulation|Phương thức đóng gói|
|Loopback|Địa chỉ loopback có được đặt hay không. Có thể xảy ra sự cố với nhà cung cấp dịch vụ |
|Keepalive|Thông số keepalive có được đặt không|
|ARP type|Kiểu của Address Resolution Protocol|
|Last input|Giờ phút giây từ khi gói tin cuối được nhận bởi interface. |
|output|Giờ phút giây từ khi gói tin cuối được truyền bởi interface|
|output hang|Giờ phút giây từ khi interface lần cuối khởi động lại vì quá trình truyền quá lâu|
|Output queue, input queue, drops queue|Số lượng gói tin ra và vào đang chờ|
|Five minute input rate, Five minute output rate|Trung bình các bit và gói tin được truyền trong mỗi dây trong 5 phút|
|packets input|Tổng số các gói tin không lỗi mà hệ thống đã nhận|
|bytes input|Tổng số byte gồm dữ liệu và địa chỉ MAC được đóng gói|
|no buffers|Số lượng các gói tin nhận bị hủy bởi hệ thống không còn bộ đệm|
|Received…broadcasts|Tổng số tin quảng bá hoặc multicast được nhận bởi interface|
|runts|Số lượng Ethernet frame bị hủy bởi chúng nhỏ hơn mức nhỏ nhất của Ethernet frame|
|input error|Runts,giants,CRC,...|
|giants|Số lượng Ethernet frame bị hủy bởi chúng lớn hơn mức nhỏ nhất của Ethernet frame|
|CRC|CRC được tạo bởi LAN không trùng với checksum được tính toán bởi nơi nhận dữ liệu|
|frame|Số lượng gói tin được nhận không chính xác với CRC bị lỗi|
|overrun|Thời gian mà thiết bị nhận không thể nhận thủ công dữ liệu đến bộ đệm của phần cứng |
|ignored|Số gói tin nhận bị từ chối bởi interface vì interface gần hết bộ đệm|
|input packets with dribble condition detected|Lỗi do frame quá dài|
|packets output|Số lượng tin được truyền bởi hệ thống|
|bytes|Số lượng byte gồm dữ liệu và địa chỉ MAC được đóng gói đã truyền trong hệ thống|
|underruns|Số thời gian mà thiết bị truyền chạy nhanh hơn khả năng của Router|
|output errors|Tổng số các lỗi được ngăn chặn việc truyền dữ liệu ra khỏi interface|
|collisions|Số lượng tin được truyền lại vì xung đột Ethernet|
|interface resets|Số lần interface được khởi động lại hoàn toàn|

## Cấu hình cơ bản IPv6

- Sử dụng Cisco Packet Tracer (Đính kèm)

- Bật chế độ định tuyến IPv6

 **R1(config)# ipv6 unicast-routing**
- Cấu hình địa chỉ cho các interface: Có thể dùng EUI-64 hoặc link-local

 **R1(config)# interface g0/0**
 
 **R1(config-if)# ipv6 address 2001:db8:acad:1::/64 eui-64**
 
hoặc

 **R1(config-if)# interface g0/0**

 **R1(config-if)# ipv6 address 2001:db8:acad:1::1/64**

 **R1(config-if)# ipv6 address fe80::1 link-local**

- Để giảm sự phức tạp khi cấu hình Router và xử lý lỗi cần cấu hình địa chỉ IPv6 và địa chỉ link-local thủ công 
- Chú ý: Nếu không loại bỏ địa chỉ Unicast đã nhập trước đó thì một Interface sẽ có 2 địa chỉ IPv6

## Kiểm tra các kết nối IPv4 và IPv6
![alt text](https://i.imgur.com/sEV57F9.png)
- Kiểm tra bằng câu lệnh **ping**, **traceroute** trên Router đến các PC
- Dùng Telnet hoặc SSH để truy cập từ xa đến thiết bị khác để kiểm tra kết nối, điều này giúp quản trị tốt hơn

## Xử lý lỗi về IP cơ bản
### Default Gateway
- Cấu hình sai default gateway
- Default Gateway được dùng khi host muốn gửi gói tin từ thiết bị đến một mạng khác. Địa chỉ default gateway thường là địa chỉ interface của router gắn vào mạng nội bộ nơi mà các host được kết nối
- Để xử lý: kiểm tra lại topo và địa chỉ của Router gắn trên cùng LAN
- Chú ý: Cấu hình sai DHCP server cũng có thể dẫn đến lỗi về default gateway
### Trùng địa chỉ IP
- Trùng địa chỉ IP có thể xảy ra giữa các thiết bị được cấu hình tĩnh và PC lấy thông tin địa chỉ từ DHCP server
- Để xử lý: 
 - Chuyển mạng của thiết bị với địa chỉ tĩnh với các DHCP client
 - Ở DHCP server, loại bỏ địa chỉ tĩnh của thiết bị cuối trong dải IP được cấu hình trên đây

[Basic Router Config IPv4.zip](https://github.com/khoa861996/31DaysCCNA/files/6165534/Basic.Router.Config.IPv4.zip)
[Basic Router Config IPv6.zip](https://github.com/khoa861996/31DaysCCNA/files/6165535/Basic.Router.Config.IPv6.zip)
