# QoS, Cloud, SDN

## Nội dung chính trong ngày 3

- Quality of Service (QoS) là công cụ và kỹ thuật mà người quản trị mạng dùng để cải thiện lưu lượng trong mạng
- Điện toán đám mây là một dịch vụ tăng cường quan trọng cung cấp các lựa chọn đến người dùng cuối. Các mạng doanh nghiệp có thể dùng nhiều dịch vụ mà điện toán đám mây đem lại để tăng năng suất và giảm chi phí
- Software-Defined Networking (SDN) trở thành thành phần tích hợp của mạng doanh nghiệp vì người quản trị có thể nhanh chóng quản lý hàng nghìn thiết bị mạng

## QoS
- Các công cụ QoS được dùng để phân loại lưu lượng dựa trên:
 - **Latency (Delay)**: Độ trễ, là thời gian để dữ liệu có thể gửi đến bên nhận. Công cụ QoS có thể giảm độ trễ các gói như thoại và video
 - **Jitter**: Jitter là phương sai trong độ trễ của gói. Công cụ QoS có thể thoát khỏi độ trễ của gói tin để cải thiện trải nghiệm người dùng
 - **Loss**: Số lượng tin bị mất, thường là phần trăm của gói tin được gửi. Công cụ QoS giảm packet loss
 - **Bandwidth**: Băng thông là thước đo của dữ liệu của một interface có thể gửi mỗi giây. Công cụ QoS có thể quản lý kiểu lưu lượng dùng băng thông
 
![alt text](https://i.imgur.com/oXckDAU.png)

- **Classification and marking**: Giám sát lưu lượng và phân chia gói dựa trên nội dung header
- **Congestion avoidance**: Khi lưu lượng vượt quá số lượng cho phép của tài nguyên mạng, một số lưu lượng có thể bị bỏ, trễ, hoặc đánh dấu lại để tránh tắc nghẽn
- **Congestion management**: Quản lý lịch trình lưu lượng khi gói tin chờ đến lượt để ra khỏi interface

### Classification and Marking

- Classification là quá trình xử lý trường phù hợp trong header để lấy 1 số kiểu của QoS trên gói tin
- Marking là quá trình xử lý thay đổi giá trị bit trong ToS hoặc trường Traffic Class

#### DSCP và IPP
![alt text](https://i.imgur.com/JM5BT35.png)

- Với các đường trunk L2, byte thứ 3 của 4 byte 802.1Q header là cho Class of Service (CoS), và công cụ QoS để đánh dấu frame. Trường này chỉ tồn tại khi frame đi qua đường trunk

#### EF và AF
- Expedited Forwarding (EF) là giá trị thập phân DSCP của 46 được gợi lý để dùng với các gói yêu cầu độ trễ, jitter và loss thấp. Dùng để mark gói thoại

![alt text](https://i.imgur.com/YqF9Ebj.png)

- Assured Forwarding (AF) xác định một bộ 12 giá trị DSCP được xếp vào một ma trận

### Congestion Management
- Quản lý tắc nghẽn là công cụ QoS để quản lý hàng chờ như gói tin ra khỏi interface
- Công cụ Class-Based Weighted Fair Queueing (CBWFQ), chỉ định class cho lưu lượng chờ và đảm bảo băng thông nhỏ nhất cho hàng chờ

![alt text](https://i.imgur.com/sgfXLSr.png)

- CBWFQ một mình sẽ không thỏa được về nhu cầu thời gian. Giải pháp là thêm Low Latency Queueing vào CBWFQ

![alt text](https://i.imgur.com/cr3btO9.png)

### Policing, Shaping, và TCP Discards
- 2 công cụ để tránh tắc nghẽn là policing và shaping. 2 công cụ này giữ bit rate ở mức cùng hoặc dưới tốc độ cho phép.

![alt text](https://i.imgur.com/lGkChaP.png)

- Nhà cung cấp dịch vụ dùng chính sách để trung với Committed Information Rate (CIR). Nếu người dùng nhiều hơn 200 Mbps CIR, SP có thể bỏ các gói tin thêm hoặc đánh dấu các gói này nhưng vẫn cho phép chúng thông qua
 - Đo lượng lương lượng qua thời gian để so sánh với chính sách đã được cấu hình
 - Cho phép burst lưu lượng trong thời gian ngắn
 - Hủy tin thừa hoặc đánh dấu lại để loại bỏ nó sau nếu xảy ra tắc nghẽn
 
![alt text](https://i.imgur.com/VGVq6L0.png)

- Shaper giảm lưu lượng bằng cách cho các gói vào hàng chờ và lên lịch cho các gói dựa trên shaping rate
- Shaping không thể làm chậm tốc độ vật lý của interface. Thay vào đó nó gửi và chờ
 - Đo lượng lương lượng qua thời gian để so sánh với shaping rate đã cấu hình 
 - Cho phép burst lưu lượng trong thời gian ngắn
 - Làm chậm gói tin bằng cách cho chúng vào hàng chờ
 
### QoS và TCP

![alt text](https://i.imgur.com/dxw4hSm.png)

- Khi hàng chờ phía dưới đầy, gói tin nhận cuối cùng sẽ bị bỏ
- Dịch vụ TCP connection-oriented giúp công cụ QoS giảm drop. Kết nối TCP sẽ bị chậm đi, giảm nghẽn, tránh drop

![alt text](https://i.imgur.com/mk0UAft.png)

## Cloud Computing
- Điện toán đám mây liên quan đến một số lượng lớn các máy tính kết nối qua một mạng đặt ở bất cứ đâu. Lợi ích của Cloud Computing
 - Cho phép truy cập dữ liệu của tổ chức mọi lúc mọi nơi
 - Chỉ đăng ký các dịch vụ cần thiết
 - Không cần hoặc giảm yêu cầu nhân viên IT để quản lý, giám sát 
 - Giảm chi phí, năng lượng, thiết bị vật lý và tranning nhân viên
 - Cho phép đáp ứng nhanh các yêu cầu về khối lượng dữ liệu ngày càng tăng
- Nhà cung cấp Cloud dựa vào ảo hóa để cung cấp các giải pháp cho khách hàng

### Server Virtualization
- Ảo hóa Server tận dụng lợi thế của các nguồn tài nguyên rảnh dỗi và hợp nhất số Server được yêu cầu. Ảo hóa phân chia OS từ phần cứng. Điều này cho phép nhiều OS tồn tại trên một phần cứng. Mỗi thành phần của OS gọi là máy ảo (VM)
- Một Server với nhiều VM dùng một người giám sát để quản lý truy cập phần vật lý của Server

![alt text](https://i.imgur.com/fx6Pia3.png)

- Một phương pháp khác để quản lý VM trên Server, nhất là trong môi trường data center là dùng Switch ảo kết nối VM đến NIC vật lý

![alt text](https://i.imgur.com/Mk8MBwO.png)

- Ở data center, nhiều Server được đặt trong một rack. 2 NIC vật lý được gắn vào 2 Switch dự phòng. Rack được đặt theo hàng và được quản lý bởi 2 Switch dự phòng ở cuối hàng

### Dịch vụ Cloud Computing
1. Người dùng yêu câu một VM hoặc một nhóm các VM
2. Kỹ sư ở data center cấu hình ảo hóa phần mềm
3. Phần mềm ảo hóa tạo ra VM

- **On-demand self-service**: Người dùng có thể lựa chọn, thay đổi và thôi dịch vụ mà không cần tác động của con người
- **Broad network access**: Dịch vụ có thể truy cập ở bất cứ thiết bị nào trên mọi mạng
- **Resource pooling**: Nhà cung cấp có một nguồn tài nguyên có thể tự động phân phối cho người dùng, Người dùng không cần quan tâm về vị trí phần cứng
- **Rapid elasticity**: Các nguồn tài nguyên có thể mở rộng hoặc thu lại nếu cần
- **Measured service**: Nhà cung cấp có thể đo đạc mức độ sử dụng và báo lại cho khách hàng
- Các dịch vụ
 - **Software as a Service (SaaS**: Nhà cung cấp có trách nhiệm cung cấp các dịch vụ truy cập qua Internet như email, office 365,... Người dùng chỉ cần cung cấp dữ liệu của họ
 - **Platform as a Service (PaaS)**: Nhà cung cấp có trách nhiệm cung cấp truy cập các công cụ phát triển và dịch vụ được dùng để truyền ứng dụng. Người dùng có thể thay đổi phần cứng ảo hóa
 - **Infrastructure as a Service (IaaS)**: Nhà cung cấp có trách nhiệm cung cấp truy cập vào trang thiết bị mạng, dịch vụ ảo hóa mạng, và cơ sở hạ tầng mạng
- 4 mô hình Cloud: 
 - **Public clouds**: Ứng dụng và dịch vụ dựa trên Cloud được cung cấp bởi một Cloud công cộng
 - **Private clouds**: Ứng dụng và dịch vụ dựa trên Cloud trong một Cloud private được hướng đến một tổ chức cụ thể, như chính phủ,...
 - **Hybrid clouds**: hybird Cloud được tạo bởi 2 hoặc nhiều Cloud. Mỗi phần là mội đối tượng khác nhau nhưng đều kết nối bằng một kiểu kiến trúc
 - **Community clouds**: Cloud cộng đồng được tạo cho một cộng đồng riêng lẻ
 
### Virual Network Infrastructure
- Một cơ sở hạ tầng mạng ảo gồm tập hợp các chức năng mạng ảo (VNF), gồm Switch, Server load balancers(SLB), Router, Firewall ảo

![alt text](https://i.imgur.com/vypyPbf.png)

## Software-Defined Networking
### Data, Control, and Management Planes
- Một thiết bị mạng truyền thống gồm 2 plan. Phần data plan có trách nhiện chuyển tiếp dữ liệu nhanh nhất có thể:
 - L2 và L3 đóng và mở gói
 - Thêm hoặc loại bỏ 802.1Q trunking header
 - Tìm kiếm bảng địa chỉ MAC
 - Tìm kiếm bảng định tuyến
 - Mã hóa dữ liệu và thêm một IP header mới (VPN)
 - Thay đổi địa chỉ IP nguồn và đích (NAT)
 - Loại bỏ tin dựa và bộ lọc (ACL, port security)
 
### Controllers
- Control plane từng là một phần của thiết bị và được phân phối qua các thiết bị. Thiết bị phải tính toán tài nguyên và bảo trì cấu trúc dữ liệu L2 và L3 (bảng ARP, định tuyến,...)
- Trong SDN, chức năng của control plane có thể loại bỏ ở thiết bị vật lý và đặt trong trung tâm ứng dụng gọi là một controller
### APIC-EM and ACLs
- APIC-EM controller có khả năng quản lý chính sách trên toàn mạng. , ACL Analysis và Path Trace cho phép người quản trị dễ dàng hình dung lưu lượng và tìm xung đột