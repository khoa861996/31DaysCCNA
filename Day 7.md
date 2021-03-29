# DHCP và DNS

## Nội dung chính trong ngày 7

- Cấp phát địa chỉ IP tự động, hệ thống tên miền

## DHCPv4

- DHCPv4 cho phét host có được IP một cách tự động khi kết nối với mạng
- Khi thiết bị được cấu hình DHCPv4 khởi động hoặc kết nối vào mạng, Client quảng bá một gói DHCPDISCOVER để xem có server DHCPv4 nào trong mạng không. Server DHCPv4 gửi trả lời với DHCPOFFER với địa chỉ IP, subnetmask, DNS server, default gateway
- Client có thể nhận nhiều gói DHCPOFFER nếu có nhiều Server DHCPv4. Client chọn đề nghị đầu tiên và quảng bá gói DHCPREQUEST để xác định rõ server
- Nếu địa chỉ IP vẫn hợp lệ, Server được chọn trả lại tin DHCPPACK. Nếu không nó trả lại tin DHCPNAK. Nếu như thiết bị tắt hoặc ra ngoài mạng, địa chỉ sẽ được trả lại để tái sử dụng

## Cấu hình DHCPv4
### Cấu hình Router như một Server DHCPv4
### Cấu hình Router trả lời yêu cầu DHCPv4
- Sử dụng Cisco Packet Tracer (Đính kèm)

## DHCPv6
- IPv6 dùng 2 phương thức để lấy địa chỉ unicast
 - Stateless address autoconfiguration (SLAAC)
 - Stateful DHCPv6
 
### SLAAC
- SLAAC dùng ùng ICMPv6 Router Solicitation (RS) và Router Advertisement (RA) để cung cấp địa chỉ và các cấu hình khác
 - **Router Solicitation (RS) message**: Khi Client được cấu hình lấy địa chỉ tự động dùng SLAAC, Client gửi tin RS đến router. Tin RS được gửi đến tất cả địa chỉ multicast của Router (FF02::2)
 - **Router Advertisement (RA) message**: Client dùng thông tin này để tạo địa chỉ IPv6 unicast. Một Router gửi một tin RA định kỳ hoặc phản hồi đến một tin RS. Tin RA gồm tiền tố và độ dài tiền tố của các đoạn trong nội bộ.
 - **Neighbor Solicitation (NS) message**: Một tin NS (Neighbor Solicitation) được dùng để học dữ liệu địa chỉ lân cận trong một mạng. Trong quá trình xử lý SLAAC, một host dùng Duplicate Address Detection (DAD) bằng cách gán địa chỉ IPv6 của nó là địa chỉ đích trong NS. NS được gửi ra ngoài mạng để xác nhận địa chỉ IPv6 mới là duy nhất
 
### Stateless DHCPv6
- Client dùng tin RA từ Router để tạo ra địa chỉ unicast global

**Router(config-if)# ipv6 nd other-config-flag**

### Stateful DHCPv6
- Tin RA cho Client biết nó thu được tất cả thông tin địa chỉ từ DHCPv6 Server

**Router(config-if)# ipv6 nd managed-config-flag**

![alt text](https://i.imgur.com/ypKfVZZ.png)

1. PC gửi một RS bắt đầu xử lý lấy địa chỉ IPv6
2. R1 trả lời với một RA. Nếu cờ M (Managed Address Configuration flag) hoặc O ( Other Configuration flag) không được cài, PC1 dùng SLAAC. Nếu một trong 2 cờ M hoặc O được đặt, PC1 bắt đầu xử lý DHCPv6
3. PC1 gửi một tin DHCPv6 SOLICIT đến tất cả server DHCPv6 
4. Một Server DHCPv6 phản hồi với tin DHCPv6 ADVERTISE
5. Client gửi DHCPv6 REQUEST hoặc DHCPv6 INFORMATION-REQUEST 
6. Server trả lời với thông tin được yêu cầu

## Cấu hình DHCPv6
### Cấu hình Router là một Server DHCPv6 Stateless
### Cấu hình Router là một Server DHCPv6 Stateful

## DNS
![alt text](https://i.imgur.com/zYiUcvf.png)

- **A**: Địa chỉ IPv4 thiết bị đầu cuối
- **NS**: Server định danh
- **AAAA**: Địa chỉ IPv6 thiết bị đầu cuối
- **MX**: Bản ghi trao đổi mail
 - Khi một client truy vấn, DNS server xử lý bằng cách xem chính bản ghi để giải mã tên. Nếu không giải mã được tên nó liên hệ với Server khác để xử lý
- **.com**: Thương mại
- **.edu**: Giáo dục
- **.gov**: Chính phủ
- **.mil**: Quân đội
- **.org**: Phi thương mại
- **.net**: ISP

[DHCP.zip](https://github.com/khoa861996/31DaysCCNA/files/6221815/DHCP.zip)
