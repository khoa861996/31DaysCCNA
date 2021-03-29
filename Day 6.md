# NAT

## Nội dung chính trong ngày 6

- NAT cho phép mạng bên trong hợp pháp hóa địa chỉ IPv4 khi truy cập mạng. Người quản trị mạng chỉ cần 1 hoặc vài địa chỉ IPv4 cho Router để cấp cho các host, thay vì một địa chỉ IPv4 nhất duy nhất.

## Khái niệm NAT
- NAT chuyển đổi địa chỉ IPv4 bằng cách cho phép mạng đùng địa chỉ IPv4 private. NAT còn cho phép ẩn địa chi IPv4 với mạng bên ngoài

![alt text](https://i.imgur.com/mjdUEvL.png)

- **Inside local address**: Địa chỉ IP private
- **Inside global address**: Địa chỉ IP hợp lệ khi ra khỏi Router NAT
- **Outside global address**: Địa chỉ IPv4 có thể tiếp cận được đặt cho host trên mạng
- **Outside local address**: Địa chỉ IPv4 local đặt cho host trên mạng bên ngoài

![alt text](https://i.imgur.com/pposjTU.png)

1. R1 gửi gói tin đến Internet cho R1
2. R1 chuyển tiếp gói tin đến R2
3. R2 xem bảng định tuyến và xác định next hop là Router ISP. Nó kiểm tra nếu có tiêu chí đặc biệt nào để biên dịch. R2 có một ACL xác định mạng bên trong phù hợp để biên dịch. Vì thế, nó biên dịch địa chỉ IPv4 local thành địa chỉ IPv4 global
4. R2 tùy chỉnh gói tin với nguồn là IPv4 mới và gửi đến Router ISP
5. Gói tin đến đích và gửi trả lời đến địa chỉ inside global
6. Khi nhận trả lời từ đích trở lại R2, nó xem bảng NAT để trùng với địa chỉ inside global để chính xác địa chỉ inside local
87. R1 nhận gói và gửi đến PC1

### Dynamic và Static NAT

- **Dynamic NAT**: Dùng dải địa chỉ public để chỉ định chúng trong lần đầu, hoặc dùng địa chỉ public có sẵn trên interface. Khi một host với một địa chỉ IPv4 yêu cầu truy cập Internet, dynamic NAT chọn một địa chỉ IPv4 trong dải. Dynamic NAT có thể cấu hình overload
- **Static NAT**:  Dùng chỉ one-on-one địa chỉ local và global

### NAT Overload (PAT)
- Map nhiều địa chỉ IPv4 private thành 1 địa chỉ IPv4 public

![alt text](https://i.imgur.com/VyXB1np.png)
1. PC1 gửi gói đến PC2
2. Khi gói đến R2, NAT overload đổi địa chỉ nguồn thành địa chỉ IPv4 inside global và giữa bản ghi của port nguồn để xác định client mà gói tin bắt đầu gửi
3. R2 cập nhật bảng NAT
4. Khi server web trả lời, R2 dùng địa chỉ port nguồn để biên dịch gói đến chính xác client

### NAT Benefit
- NAT bảo toàn các IPv4 đã đăng ký vì với NAT overload, host bên trong có thể chia sẻ một địa chỉ IPv4 public với tất cả giao tiếp bên ngoài
- NAT cải thiện khả năng kết nối với public netwwork
- NAT cho phép bảo toàn những gì sẵn có nhưng vẫn hỗ trợ địa chỉ public mới
- NAT cung cấp một lớp bảo mật mạng vì private netwwork không quảng bá địa chỉ local

### NAT Limitations
- **Performance is degraded**: Tăng độ trễ khi chuyển mạch vì biên dịch mỗi địa chỉ IPv4 với gói header cần mất thời gian
- **End-to-end functionality is degraded**: Nhiều giao thức Internet và ứng dụng phụ thuộc trên chứng năng end-to-end
- **End-to-end IP traceability is lost**: Khó xử lý lỗi
- **Tunneling is more complicated**: 
- **Services can be disrupted**: Dịch vụ yêu cầu khởi tạo kết nối TCP từ mạng bên ngoài hoặc giao thức stateless như UDP có thể bị gián đoạn

## Cấu hình NAT tĩnh
- Là map 1-1 giữa địa chỉ bên trong và ngoài, cho phép kết nối bằng thiết bị bên ngoài truy cập vào thiết bị bên trong
- Sử dụng Cisco Packet Tracer (Đính kèm)

## Cấu hình NAT động
## Cấu hình NAT Overload