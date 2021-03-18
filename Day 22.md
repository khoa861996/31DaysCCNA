# RIPv2

## Nội dung chính trong ngày 22

- RIP (Routing Information Protocol) là định tuyến cơ bản nhất, RIP ít được dùng trong hiện tại
- Cách hoạt động, cấu hình, kiểm tra, xử lý lỗi RIP

## Khái niệm RIP
### RIPv1
- RIPv1 là định tuyến *classful distance vector* sử dụng cho IPv4. Nó dùng hop-count là metric để chọn đường đi, nếu lớn hơn 15 được coi là không tiếp cận được. RIPv1 đóng gói tin vào một đoạn UDP dùng port 520
### Cách thức hoạt động
- Thông báo của RIPv1 dùng 2 trường: Yêu cầu và phản hồi
- Mỗi interface được cấu hình RIP gửi một thông báo yêu cầu khi khởi động, nó yêu cầu tất cả các Router lân cận khác dùng RIP gửi bảng định tuyến. RIP ở các Router lân cận gửi thông báo phản hồi. Khi Router yêu cần nhận được phản hồi, nó đánh giá từng tuyến đường. Nếu tuyến đường là mới, Router nhận thêm tuyến đó vào bảng định tuyến. Nếu tuyến đó đã có trong bảng định tuyến, thì tuyến có sẵn được thay thế nếu tuyến mới có hop-count tốt hơn
- RIPv1 không gửi thông tin về Subnetmask. Vì giới hạn này, RIPv1 không được dùng nữa vì không dùng được VLSM

## Cấu hình RIPv1
- Dùng Cisco Packet Tracer (Đính kèm)
- Cấu hình trên R1 (Tương tự với R2, R3):

 **R1(config)# router rip**
 
 **R1(config-router)# network 192.168.1.0**
 
 **R1(config-router)# network 192.168.2.0**

## Kiểm tra và xử lý lỗi RIPv1
- Câu lệnh kiểm tra:
 - **show ip route**
 - **show ip protocols**
 - **debug ip rip**

- RIP route được học bởi R2:
R 192.168.5.0/24 [120/1] via 192.168.4.1, 00:00:23, Serial0/0/1

| Đầu ra           | Mô tả      |
|:-------------:|:-------------:|
| R     |Xác định nguồn tuyến dùng RIP|  
| 192.168.5.0  |  Xác định địa chỉ của mạng truy nhập từ xa  |
| /24   |Xác định Subnetmask cho mạng này|          
| [120/1]    | AD (120) và mertic (1 hop)     |
| via 192.168.4.1      | Địa chỉ của Next-hop router (R3) để gửi lưu lượng đến mạng đó      |
| 00:00:01      |  Thời gian sau khi định tuyến được cập nhật|
| Serial0/0/1    |       Xác định interface có thể tiếp cận được        |

![alt text](https://i.imgur.com/ElHaSFy.png)
1. Dòng đầu xác định định tuyến kiểu RIP được cấu hình trên R2
2. ...CCNP
3. Thời gian hiện khi cấp nhật tiếp theo được gửi từ Router này 
4. Đầu ra gồm thông tin của phiên bản RIP được cấu hình và interface nào tham gia vào việc cập nhật RIP. 
5. Hiển thị R2 đang ở trạng tổng hợp ở mạng kiểu classful, cân bằng tải
6. Mạng Classful được cấu hình với câu lệnh được liệt kê tiếp theo. Nếu R2 có interface được cấu hình mạng, R2 sẽ bao gồi mạng khi RIP cập nhật
7. RIP lân cận được liệt kê. Gateway là địa chỉ IP tiếp theo của các Router lân cận gửi cập nhật đến R2

### Automatic Summarization
- Dùng Cisco Packet Tracer (Đính kèm)
- Ở cấu hình RIP ở tất cả các Router, mạng loại classful được nhập thay vì mỗi mạng con
- Khi R2 gửi cập nhật đến R1, nó gửi mạng 172.30.3.0 vì Serial 0/0/0 dùng /24. Nó sẽ tổng hợp mạng con 192.168.4.8/22 vào 192.168.4.0 trước khi gửi cập nhật cho R1 vì R1 được áp dụng kiểu classful. R2 là Router ngăn cách cho mạng 192.168.4.0. Khi nó cập nhật đến R3, R2 sẽ tổng hợp mạng con 172.30.1.0, 172.30.2.0, 172.30.3.0 thành 172.30.0.0 vì R2 là Router ngăn cách cho mạng 172.30.0.0 và R3 không có cách nào để đến được mạng 172.30.0.0
### Default Routing
- Có thể cấu hình R1 với default route đến R2 tuy nhiên có thể dùng câu lệnh

 **default-information originate** để lan truyền default route đến R1

 **R2(config)# router rip**

 **R2(config-router)# default-information originate** 

## RIPv2
- RIPv2 có thêm trường Subnetmask => có thể dùng VLSM
- Mặc định RIPv2 tự động tổng hợp mạng như RIPv1, để có thể hỗ trợ mạng con không liên tục và VLSM, phải cấu hình **no auto-summary** ở RIPv2

### Kiểm tra và sửa lỗi RIPv2
- Đảm bảo rằng các đường dẫn ở trạng thái *up*
- Kiểm tra kết nối
- Đảm bảo IP và subnetmask đúng
- **Version**: Kiểm tra đã có lệnh version 2 chưa
- **Network statements**: Cấu hình Network không đúng hoặc thiếu
- **Automatic summarization**: đảm bảo rằng đã dùng câu lệnh **no auto-summary**

[RIP.zip](https://github.com/khoa861996/31DaysCCNA/files/6165547/RIP.zip)
