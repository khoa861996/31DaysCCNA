# Triển khai WAN

## Nội dung chính trong ngày 4

- PPP, PPPoE, GRE, eBGP

## Khái niệm PPP 

- PPP cung cấp nhiều chức năng cơ bản hữu dụng cho kết nối giữa 2 thiết bị:
 - Xác định header và trailer cho phép phần phối frame của đường dẫn
 - Hỗ trợ liên kết đồng bộ và không đồng bộ
 - Cho phép nhiều giao thức L3 truyền qua một đường dẫn
 - Cơ chế xác thực
 - Kiểm soát giao thức
 
### Định dạng Frame của PPP

- PPP xác định tin kiểm soát tin L2
- Link Control Protocol (LCP) kiểm soát chức năng hoạt động giống nhau của giao thức L3

#### Looped-Link Detection

- LCP nhanh chóng thông đường dẫn bị lặp dùng tính năng gọi là magic number. PPP giúp Router nhận ra đường dẫn bị lặp. Nếu Router nhận ra vòng lặp, nó có thể đặt interface ở trạng thái down

#### Enhanced Error Detection
- Khi mạng có đường dự phòng, có thể dùng PPP để giám sát tần số các frame nhận bị lỗi

#### PPP Multilink

- Trong cấu hình dự phòng giũa 2 Router, Router dùng cân bằng tải L3, xem kẽ lưu lượng giữa 2 đường dẫn. Multilink PPP cho phép bảng định tuyến L3 dùng một tuyến để tổng hợp các đường dẫn, giữ bảng định tuyến nhỏ nhất

#### PPP Authentication

- PAP và CHAP xác thực điểm cuối trên một trong 2 đầu của liên kết serial point-to-point. CHAP dùng thuật toán MD5 an toàn hơn mật khẩu bằng chữ gửi bởi PAP

## Cấu hình PPP

- Sử dụng Cisco Packet Tracer (Đính kèm)

## PPPoE
- PPP có thể dùng trên tất cả các liên kết Serial
- ISP thường dùng PPP như một giao thức data-link 
## Cấu hình PPPoE
1. Tạo PPP tunnel dùng interface dialer
2. Cấu hình PPP CHAP để xác thực với ISP
3. Bật PPPoE ở interface vật lý **pppoe enable**
4. Đặt tối đa MTU xuống 1492 để phòng tránh phân mảnh gói tin làm trễ truyền

## GRE Tunnel
- Generic routing encapsulation là một ví dụ cơ bản, không bảo mật

### Cấu hình GRE
- Sử dụng Cisco Packet Tracer (Đính kèm)

## Khái niệm BGP
- BGP trao đổi thông tin định tuyến giữa các Router như OSPF và EIGRP

## Cấu hình eBGP

[GRE.zip](https://github.com/khoa861996/31DaysCCNA/files/6221802/GRE.zip)
[PPP.zip](https://github.com/khoa861996/31DaysCCNA/files/6221803/PPP.zip)
[BGP.zip](https://github.com/khoa861996/31DaysCCNA/files/6221804/BGP.zip)
