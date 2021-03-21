# EIGRP

## Nội dung chính trong ngày 16

- Enhanced Interior Gateway Routing Protocol (EIGRP) là giao thức định tuyến kiểu distance vector dạng classless

## Tổng quan
- Các tính năng:
 - Giao thức truyền vận tin cậy (RTP)
 - Cập nhật có giới hạn
 - Thuật toán cập nhật khuếch tán (DUAL)
 - Khả năng thiết lập lân cận
- Mặc dù EIGRP có thể hoạt động như định tuyến link-state, nó vẫn là định tuyến distance vector

## Đặc trưng của EIGRP
### PDMs
- EIGRP có khả năng định tuyến với nhiều giao thức khác nhau, gồm cả IPv4 và IPv6, dùng protocol-denpendent modules (PDM). PDM chịu trách nhiệm cho nhiệm vụ định tuyến cụ thể cho mỗi lớp Network:
 - Đảm bảo bảng Router liền kề và bảng topo của Router EIGRP thuọc về bộ giao thức EIGRP
 - Xây dựng và biên dịch gói tin với giao thức cụ thể cho DUAL
 - Interface DUAL với bảng định tuyến giao thức cụ thể
 - Tính toán metric và truyền dữ liệu đến DUAL
 - Thực hiện chức năng phân phối lại đến và từ các giao thức định tuyến khác
 - Phân phối lại các tuyến đã được học bởi giao thức định tuyến khác
### RTP
- RTP cho phép PDM hoạt động, EIGRP dùng RTP thay cho TCP để thực hiện truyền và nhận gói EIGRP

![alt text](https://i.imgur.com/B6sZ5LT.png)

- EIGRP không thể dùng dịch vụ của UDP và TCP vì IPX và AppleTalk không dùng giao thức từ giao thức TCP/IPX
### Kiểu gói tin EIGRP
- RTP có thể truyền gói EIGRP dưới dạng unicast hoặc multicast
 - Gói Multicast EIGRP cho IPv4 dùng dành riêng cho IPv4 multicast 224.0.0.10
 - Gói Multicast EIGRP cho IPv6 gửi cho địa chỉ IPv6 multicast FF02::A.
- EIGRP dùng 5 kiểu gói tin:
 - **Hello**: EIGRP dùng gói tin "hello" để tìm các Router lân cận để thiết lập kết nối. Gói tin "hello" là multicast và truyền không tin cậy, nên không cần phản hồi từ bên nhận
 - **Update**: EIGRP không gửi cập nhật định kỳ. Gói cập nhật chỉ được gửi khi cần thiết. Gói tin cập nhật dùng các truyền tin cậy
 - **Acknowledgment**: EIGRP gửi gói ACK không tin cậy
 - **Query**: DUAL dùng gói tin truy vấn (query) khi tìm kiếm mạng, dùng cách truyền tin cậy và có thể sử dụng cả multicast và unicast
 - **Reply**: Gói trả lời (reply) được gửi trong gói phản hồi đến gói query cho dù Router có trả lời về thông tin được truy vấn hay không

## Quá trình xử lý EIGRP
- EIGRP dùng phương thức hội tụ riêng biệt gồm một hỗn hợp mertric và DUAL
### EIGRP Convergence

![alt text](https://i.imgur.com/KDNOZU5.png)

1. Router trao đổi "hello" để thiết lập kết nối và cập nhật bảng Router lân cận
2. Router trao đổi tất cẩ thông tin về định tuyến và cập nhật bảng topo
3. Router chọn tuyến tốt nhất và cập nhật bảng định tuyến
4. Router duy trì liên kết với "hello"
5. Khi có thay đổi trong topo, Router phản hồi với các bản cập nhật và kết nối lại về thông tin mới
### EIGRP Composite Metric
- EIGRP dùng giá trị băng thông, độ trễ, tính tin cậy, và tải trong hỗn hợp metric để tính toán đường dẫn hợp lý đến một mạng
### Khái niệm DUAL
- Cơ chế tránh vòng lặp của DUAL
 - **Successor**: Một Router lân cận được dùng để chuyển tiếp gói tin là tuyến least-cost đến mạng đích
 - **Feasible distance (FD)**: Metric được tính toán thấp nhất để đến mạng đích
 - **Feasible successor (FS)**: Router lân cận có một đường dự phòng không lặp đến cùng một mạng
 - **Reported distance (RD) or advertised distance (AD)**: Khoảng khả thi được báo bởi Router lân cận
 - **Feasible condition or feasibility condition (FC)**: Điều kiện gặp khi một Router lân cận reported distance (RD) đến một mạng nhỏ hơn khoảng cách khả thi của Router nội bộ đến cùng một mạng đích
 