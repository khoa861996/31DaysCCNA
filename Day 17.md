# Fine-Tuning và xử lý lỗi OSPF

## Nội dung chính trong ngày 17

- Fine-Tuning liên quan đến xử lý bộ hẹn giờ, bầu DR/BDR, quảng bá một default route, xử lý sự cố về OSPF

## Cấu hình OSPF mẫu
- Sử dụng Cisco Packet Tracer (Đính kèm)
## Chỉnh sửa cấu hình OSPFv2
- Khái niệm và câu lệnh để phân phối lại default route, điều chỉnh OSPF interface, thao tác quy trình bầu DR/BDR 
### Phân phối lại một Default Route
- R1 có một kết nối Internet nên R1 là Autonomous System Boundary Router (ASBR). Vì thế cấu hình default route đến Internet và phân phối lại default route cho R2 và R3 với câu lệnh default-information orginate
**R1(config)# ip route 0.0.0.0 0.0.0.0 Serial 0/1/0**
**R1(config)# router ospf 10**
**R1(config-router)# default-information originate**
- R2 và R3 sẽ có default route với mã ** O*E2 **

### Chỉnh sửa khoảng thời gian "Hello" và "Dead"
- Mặc định khoảng thời gian mặc định của "hello" là 10s trên Multiaccess và Point-to-Point. Nonbroadcast multiaccess (NBMA) là 30s. Khoảng "Dead" sẽ gấp 4 lần khoảng "hello"
- Thay đổi thông số này để Router có thể kiểm tra mạng không kết nối được trong thời gian ngắn hơn. Cải thiện lưu lượng.
**Router(config-if)# ip ospf hello-interval seconds**
**Router(config-if)# ip ospf dead-interval seconds**
- Chú ý: với OSPF khoảng "hello" và "dead" phải tương đương với nhau giữa các Router
### Kiểu mạng OSPF
- 5 loại:
 - **Point-to-point**: 2 Router kết nối qua một liên kết. Không có Router nào trên cùng liên kết đó
 - **Broadcast multiaccess**: Nhiều Router kết nối với nhau qua mạng Ethernet
 - **NBMA**: Nhiều Router kết nối trong một mạng không cho phép Broadcast (Frame Relay)
 - **Point-to-multipoint**: Nhiều Router kết nối với nhau trong topo hub-and-spoke qua mạng NBMA
 - **Virtual links**: Mạng OSPF đặc biệt kết nối khoảng cách xa đến area cốt lõi
### Bầu DR/BDR
- Biện pháp quản lý số lượng các Router lân cận và truyền LSA trên mạng multiaccess là designated router (DR). Để giảm thiểu lưu lượng ở mạng multiaccess, OSPF chọn DR và BDR. DR chịu trách nhiệm cập nhật tất cả các Router khác khi có thay đổi trong mạng multiaccess. BDR giám sát DR và trở thành DR khi DR lỗi
- Tiêu chí lựa chọn DR và BDR:
1. Router DR với interface có *priority* cao nhất
2. Router BDR là Router với *priority* cao thứ 2
3. Nếu các interface có *priority* giống nhau, thì sẽ lựa chọn theo ID Router
- Khi DR được bầu, nó tồn tại đến khi:
 - DR lỗi
 - Quá trình xử lý OSPF trên DR lỗi
- Nếu DR lỗi, BDR lên làm DR và tổ chức chọn BDR mới. Nếu một Router mới tham gia vào mạng sau khi DR/BDR được bầu, nó sẽ không trở thành DR/BDR cho dù có *priority* hoặc ID Router cao nhất.
### Kiểm soát việc bầu DR/BDR
- Vì DR là đầu mối để tập hợp và phân phối LSA, Router này phải có CPU và bộ nhớ đủ để hoạt động. Thay vì dùng Router ID có thể dùng *priority* với câu lệnh **ip ospf priority**
**Router(config-if)# ip ospf priority {0 - 255}**
- Mặc định là 1 ở mọi interface. Đổi thành giá trị lớn hơn thì Router đó sẽ trở thành DR, Nếu = 0 thì Router đó không thể thành DR/BDR

## Xử lý lỗi OSPF
### OSPF State
![alt text](https://i.imgur.com/GZfRBh5.png)
### OSPF Adjacency
- Các kết nối OSPF phải có cùng nhiều cài đặt để có thể giao tiếp. Liên kết OSPF sẽ không hình thành khi:
 - Interface không cùng mạng
 - kiểu mạng OSPF không giống nhau 
 - Khác khoảng "hello" hoặc "dead"

[Redistribute, Interval OSPF.zip](https://github.com/khoa861996/31DaysCCNA/files/6177480/Redistribute.Interval.OSPF.zip)

 
