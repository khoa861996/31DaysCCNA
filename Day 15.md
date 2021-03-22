# Triển khai EIGRP

## Nội dung chính trong ngày 15

- Cấu hình, kiểm tra EIGRP cho IPv4, IPv6

## Cấu hình EIGRP cho IPv4
- Sử dụng Cisco Packet Tracer (Đính kèm)

- Car EIGRP và OSPF đề dùng Router ID, tuy nhiên ở OSPF quan trọng hơn

## Kiểm tra EIGRP cho IPv4
### Kiểm tra chi tiết giao thức
![alt text](https://i.imgur.com/m14iwXB.png)

- EIGRP là giao thức định tuyến động được cấu hình trên R1 với AS là 1
- Router ID: 1.1.1.1
- AD của EIGRP trên R1 là internal = 90 và external = 170 (giá trị mặc định)
- Mặc định EIGRP không tự tổng hợp các mạng. Mạng con được bao gồm trong cập nhật định tuyến
- Router EIGRP lân cận mà R1 có với các Router khác được dùng để nhận các cập nhật định tuyến EIGRP
### Kiểm tra bảng Router lân cận
![alt text](https://i.imgur.com/4BRpZvP.png)

- **Cột H**: Danh sách Router lân cận mà chúng học được
- **Address**: Địa chỉ IPv4 của các Router lân cận
- **Interface**: Interface mà gói "hello" nhận
- **Hold**: Khi gói "hello" được nhận, giá trị này dặt lại thành giá trị thời gian giữ cao nhất và đếm ngược về 0. Nếu về 0, Router lân cận được đánh giá là không hoạt động
- **Uptime**: Hiển thị thời gian mà Router lân cận này được thêm vào bảng
- **Smooth round trip timer (SRTT) and retransmission timeout (RTO)**: Dùng bởi RTP để quản lý độ tin cậy của gói EIGRP
- **Queue count**: Nên là 0. Nếu lớn hơn 0, gói EIGRP sẽ đợi để được gửi
- **Sequence number**: Theo dõi cập nhật, truy vấn và trả lời gói tin

## Cấu hình EIGRP cho IPv6
- Sử dụng Cisco Packet Tracer (Đính kèm)

[EIGRP.zip](https://github.com/khoa861996/31DaysCCNA/files/6183123/EIGRP.zip)

