# WAN

## Nội dung chính trong ngày 5

- Tìm hiểu về WAN, kết nối, VPN

## WAN Topologies

![alt text](https://i.imgur.com/92H1oGj.png)

- **Point-to-point**: Thường dùng kết nối leased-line như T1/E1
- **Hub-and-spoke**: Single-homed, Point-to-multipoint topo với một interface trên Router hub có thể chia sẻ với nhiều Router spoke qua interface ảo
- **Full mesh**: Cho mỗi Router một kết nối đến tất cả các Router khác. Yêu cầu nhiều interface ảo
- **Dual-homed**: Cung cấp dự phòng cho Single-homed, Point-to-multipoint topo bằng cách cung cấp một hub thứ 2 để kết nối với Router spoke

- Một tổ chức có thể chọn triển khai với các topo này. Enterprise dùng full mesh, sau đó dùng hub-and-spoke giữa các vùng đầu não. Nếu các văn phòng giao tiếp thường xuyên, người quản trị mạng dùng point-to-point 

## Kết nối WAN
### Lựa chọn kết nối
- Còn được gọi là leased-line. leased-line thường đắt hơn dịch vụ chuyển mạch vì tính chuyên dụng

### Lựa chọn kết nối chuyển mạch
- 2 lựa chọn chuyển mạch là analog dialup và ISDN
 - Analog dialup dùng để trao đổi mua bán, email, giá cả,...
 - ISDN chuyển vòng lặp nội bộ thành kết nối kỹ thuật số TDM
  - **Basic Rate Interface (BRI)**: Cung cấp 2 kênh B 64kbps cho thoại và hoặc dữ liệu và kênh D 16kbps được dùng để kiểm soát tín hiệu
  - **Primary Rate Interface (PRI)**: Cung cấp 23 kênh B 64kbps và 1 kênh D 64kbps ở Bắc Mỹ,...
  
### Lựa chọn chuyển mạch gói

#### Metro Ethernet
- **Reduced expenses and administration**: Giảm bớt chi phi
- **Easy integration with existing networks**: Dễ kết nối với Ethernet LAN có sẵn
- **Enhanced business productivity**: Tận dụng lợi thế nâng cao ứng dụng IP

#### MPLS
- **Multiprotocol**: MPLS có thể mang theo tất cả loại tải nào
- **Labels**: Dùng các label bên trong nhà cung cấp dịch vụ mạng để xác định đường dẫn giữa các Router thay vì giữa các điểm cuối
- **Switching**: MPLS định tuyến gói IPv4 và IPv6, nhưng mọi thứ khác đều chuyển mạch

### Lựa chọn kết nối Internet
#### DSL
- Là một công nghệ kết nối thường trực dùng đường di động xoắn đôi để truyền dữ liệu có băng thông cao và cung cấp dịch vụ IP cho người đăng ký

#### Cable Modem
- Cung cấp một kết nối thường trực với kết nối đơn giản

#### Wireless

- **Municipal Wi-Fi**
- **WiMAX**
- **Satellite Internet**:
- **Cellular service**

### Lựa chọn liên kết WAN

| Lựa chọn           | Mô tả        |Lợi thế|Nhược điểm|Giao thức|
|:-------------:|:-------------:|:-------------:|:-------------:|:-------------:|
| Leased line   |  Kết nối điểm điểm giữa 2 LAN             | An toàn nhất|Đắt|PPP,HDLC,SDLC|
|Circuit switching|Đường mạch chuyên biệt giữa 2 điểm cuối|Rẻ hơn|Cài đặt để gọi| PPP,ISDN|
| Packet switching  |  Thiết bị truyền gói bằng một chia sẻ point-to-point or point-to-multipoint   |Hiệu quả cao dùng băng thông|Chia sẻ media qua đường dẫn| Frame Relay, MetroE
| Internet       | Gói chuyển mạch không kết nối dùng Internet              |Rẻ nhất| Không an toàn| DSL,wireless|

## Công nghệ VPN
- VPN là kết nối mã háo giữa 2 mạng riêng qua mạng chung. Thay vì dùng thiết bị L2 chuyên biệt như leased-line, một VPN dùng kết nối ảo gọi là VPN tunnels, định tuyến qua Internet từ mạng riêng của công ty đến bên truy cập từ xa

### Lợi ích của VPN
- **Cost savings**: Loại bỏ yêu cầu về các kết nối WAN chuyên dụng 
- **Security**: Dùng mã hóa cấp cao và xác thực giao thức bảo vệ dữ liệu từ các truy cập chưa xác thực
- **Scalability**: Có thể thêm lượng lớn dung lượng mà không cần thêm cơ sở hạ tầng 
- **Compatibility with broadband technology**: Hỗ trợ bởi nhà cung cấp dịch vụ, vì vậy nhân viên di động và viễn thông có thể tận dụng dịch vụ Internet tốc độ cao tại nhà để truy cập mạng công ty

### Kiểu của truy cập VPN

- **Site-to-site VPNs**: Kết nối tất cả mạng này với tất cả mạng khác
- **Remote-access VPNs**: Dùng cho các host riêng lẻ để kết nối từ xa đến mạng của công ty qua Internet
- **Dynamic Multipoint VPN**: Công nghệ của Cisco để xây dựng VPN một cách dễ dàng