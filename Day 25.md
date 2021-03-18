# Khái niệm cơ bản về định tuyến

## Nội dung chính trong ngày 26

- Khái niệm về định tuyến, gói tin được xử lý bởi các thiết bị trung chuyển như thế nào. Phương pháp định tuyến, bao gồm kết nối, định tuyến tĩnh, động

## Chuyển tiếp gói tin
- Gói tin chuyển tiếp bằng Router thông qua chức năng xác định đường dẫn và phương thức chuyển mạch. Chức năng xác định đường dẫn là quá trình Router xác định đường dẫn để chuyển tiếp một gói tin. Để xác định đường đi tốt nhất, Router tìm trong bảng định tuyến địa chỉ mạng phù hợp với địa chỉ đích của gói tin.
- Kết quả qua việc tìm kiếm này nằm ở 1 trong 3 đường dẫn:
 - **Directly connected network**: Nếu địa chỉ đích của gói tin nằm trong thiết bị kết nối trực tiếp đến Router thì gói tin được truyền trực tiếp đến thiết bị đó
 - **Remote network**: Nếu địa chỉ đích của gói tin nằm trong mạng truy cập từ xa, gói tin này sẽ được chuyển đến 1 Router khác. Mạng này chỉ đc kết nối đến bằng cách truyền gói tin qua 1 Router khác.
 - **No route determined**: Nếu địa chỉ đích của gói tin không nằm trong 2 loại trên và Router không có default route, gói tin sẽ bị hủy. Router sẽ gửi tin báo ICMP Unreachable đến địa chỉ nguồn của gói tin

![alt text](https://i.imgur.com/ixNMjBD.png)
1. PC1 gửi gói tin đến PC2. Sử dụng chức năng AND trên địa chỉ IP đích và Subnetmask của PC1, PC1 xác định được IP nguồn và đích nằm ở một mạng khác. PC1 kiểm tra bảng ARP địa chỉ default gateway và liên kết MAC của nó. Sau đó đóng gói gói tin vào Ethernet header và chuyển tiếp đến R1
2. Router R1 nhận Ethernet frame. R1 kiểm tra địa chỉ MAC đích. Sau đó R1 sao chép frame vào bộ đệm để xử lý.
   R1 mở gói Ethernet frame và đọc địa chỉ IP đích. Và bởi vì nó không trùng với bất kỳ kết nối nào của R1 nên R1 sẽ xem bảng định tuyến để định tuyến gói tin này
   R1 kiểm tra bảng định tuyến để tìm ra mạng có địa chỉ IP đích như một host trong mạng. R1 đóng gói gói tin lại với định dạng frame phù hợp để gói tin đi ra khỏi interface và chuyển frame đó đến interface (G0/1). Interface này sẽ chuyển tiếp gói tin đến chặng tiếp theo
3. R2 nhận gói tin và xử lý giống R1 tuy nhiên gói tin truyền qua interface với kết nối serial. Vì thế R2 đóng gói gói tin lại với định dạng frame phù hợp để gửi đến R3 (serial interface không dùng địa chỉ MAC)
4. R3 nhận gói tin. R3 mở gói tin. Tìm trong bảng định tuyến một mạng kết nối trực tiếp đến R3. R3 tìm đến địa chỉ IP đích trong bộ đệm ARP. Nếu nó không tồn tại trong bộ đệm ARP, R3 sẽ yêu cầu ARP qua G0/0
   PC2 gửi lại ARP với MAC của nó. R3 cập nhật bộ đệm ARP với thông tin địa chỉ đích và địa chỉ MAC.
   Gói tin được đóng gói lại và gửi qua G0/0
5. Ethernet frame với gói tin được đóng gói đến PC2.
   PC 2 kiểm tra địa chỉ MAC đích, trùng với địa chỉ địa chỉ MAC của interface nhận. PC2 sẽ sao chép lại tất cả frame. PC2 thấy kiểu trường Ethernet là 0x800, có nghĩa là Ethernet frame này gồm gói tin trong phần dữ liệu của frame. PC2 mở gói và truyền gói tin này cho hệ thống xử lý IP xử lý

## Phương thức định tuyến
- Router có học định tuyến từ 3 nguồn cơ bản:
 - **Directly connected routes**: Tự động điền vào bảng định tuyến khi một interface hoạt động với một địa chỉ IP
 - **Static routes**: Cấu hình thủ công bở người quản trị mạng và điền vào bảng định tuyến nếu đầu ra của interface được định tuyến tĩnh
 - **Dynamic routes**: Học bởi Router qua việc chia sẻ định tuyến với Router khác dùng chung giao thức định tuyến
- Trong nhiều trường hợp, độ phức tạp của topo, số lượng network, cần dùng định tuyến tự động.

## Các giao thức định tuyến động
### IGP và EGP
- Một hệ thống tự quản trị là một tập hợp của các Router dưới sự điều hành của người quản trị đưa ra chính sách định tuyến chung với Internet. 
- 2 giao thức định tuyến cần dùng:
 - **Interior gateway protocols (IGP)**: Sử dụng để định tuyến trong hệ thống tự quản trị, gọi là định tuyến trong một hệ thống tự trị (AS)
 - **Exterior gateway protocols (EGP)**: Sử dụng để định tuyến liên hệ thống tự quản trị, gọi là định tuyến giữa các hệ thống tự trị (AS)
### Distance Vector Routing Protocols
- *Distance vector* có nghĩa là định tuyến đó được quảng bá như các vector của hướng và khoảng cách. Khoảng cách được xác định theo giá trị như số bước nhảy, và hướng là next-hop Router hoặc interface mà nó đi ra. Giao thức Distance Vector dùng thuật toán Bellman-Ford để xác định đường đi tốt nhất cho định tuyến
- Một số giao thức Distance Vector gửi bảng định tuyến theo định kỳ đến tất cả các kết nối lân cận.
- Mặc dù thuật toán Bellman-Ford đủ để duy trì cơ sở dữ liệu của các mạng có thể truy cập, thuật toán này không cho phép Router biết chính xác cấu trúc của kết nối Internet. Router chỉ biết thông tin định tuyến từ các Router lân cận
- Giao thức này dùng Router như một bảng chỉ dẫn theo đường dẫn đến đích. Giao thức này không có một sơ đồ cụ thể về topo mạng
- Sử dụng giao thức Distance Vector tốt nhất trong:
 - Mạng đơn giản, phẳng và không yêu cầu thiết kế phân cấp
 - Người quản trị mạng không đủ hiểu biết để cấu hình và xử lý lỗi của giao thức Link-state
 - Các loại mạng cụ thể như hub-and-spoke được triển khai dạng này
### Link-State Routing Protocols
- Giao thức định tuyến này có thể tạo ra cái nhìn tổng thể về topo bằng cách tập hợp các thông tin từ tất cả các Router. Bảng chỉ dẫn từ nguồn đến đích là không cần thiết bởi các Router dùng Link-State dùng một sơ đồ giống nhau. Router dùng Link-State tạo một topo để chọn đường dẫn tốt nhất đến tất cả các đích.
- Khi các mạng hội tụ, Link-State sẽ được gửi chỉ khi topo có sự thay đổi
- Sử dụng giao thức Link-State tốt nhất trong:
 - Mạng có sự phân cấp, xử lý các mạng lớn
 - Người quản trị mạng có hiểu biết tốt để triển khai định tuyến bằng giao thức Link-State
 - Hội tụ nhanh trong mạng là điều quan trọng
### Classful Routing Protocols
- Giao thức định tuyến Classful không gửi thông tin về Subnetmask trong khi cập nhật định tuyến. Routing Information Protocol (RIP) là định tuyến kiểu Classful.
- Giao thức này vẫn có thể sử dụng trong mạng ngày nay nhưng chúng không bao gồm Subnetmask. Không thể dùng giao thức này khi dùng VLSM
### Classless Routing Protocols
- Giao thức định tuyến Classless bao gồm cả thông tin về Subnetmask trong khi cập nhật định tuyến. Mạng ngày nay không còn được phân bổ dựa trên các loại A,B,C và không thể xác định Subnetmask bằng cách xác định giá trị của octet đầu tiên.
- Giao thức này phải có trong hầu hết các mạng hiện thời vì nó hỗ trợ VLSM, các định tuyến dùng Classless (RIPv2,EIGRP,OSPF,IS-IS,BGP)

## Dynamic Routing Metrics
- Trong 1 số trường hợp, giao thức định tuyến học nhiều hơn một tuyến đường đến cùng một đích từ cùng một nguồn định tuyến. Để chọn đường đi tốt nhất, giao thức định tuyến phải có khả năng đánh giá và phân biệt các đường đi. *metric* được dùng trong trường hợp này. 2 giao thức định tuyến khác nhau có thể chọn đường đi khác nhau để đến cùng một đích bởi chúng dùng *metric* khác nhau:
 - **RIP—Hop count**: Đường đi tốt nhất được chọn là tuyến đường đi qua ít Router nhất
 - **IGRP and EIGRP—Bandwidth, delay, reliability, and load**: Đường đi tốt nhất được chọn là tuyến đường với giá trị metric nhỏ nhất được tính toán từ nhiều tham số
 - **IS-IS and OSPF—Cost**: Đường đi tốt nhất được chọn là tuyến đường có *cost* nhở nhất (ở đây là băng thông)
- Tất cả các giao thức định tuyến đều có khả năng cân bằng tải

## Administrative Distance
- Một Router học nhiều tuyến đường để truy cập mạng từ xa.
- Ở một mạng có thể có nhiều giao thức định tuyến động
- AD xác định ưu tiên của nguồn định tuyến. Mỗi định tuyến nguồn bao gồm các giao thức định tuyến riêng biệt. Router Cisco dùng AD để xác định đường đi tốt nhất khi chúng học về mạng đích từ 2 hoặc nhiều hơn các định tuyến nguồn.
- Giá trị của AD từ 0-255. Giá trị càng thấp, định tuyến nguồn càng được ưu tiên.

| Định tuyến nguồn           | AD      |
|:-------------:|:-------------:|
| Connected     |0|  
| Static  |  1  |
| EIGRP summary route   |5|          
| External BGP     | 20     |
| Internal EIGRP       | 90      |
| IGRP      |  100|
| OSPF      |       110        |
| IS-IS      | 115|
| RIP      |     120          |
| External EIGRP      | 170|
| Internal BGP      |    200           |

## Ngăn chặn vòng lặp khi định tuyến
- Nếu không có biện pháp ngăn chặn, phương pháp định tuyến Distance Vector có thể gây ra lặp. Một định tuyến lặp là khi các gói tin được truyền đi truyền lại ở 1 loạt các Router mà không đến được mạng đích. Điều này xảy ra khi 2 hoặc nhiều Router có sự không chính xác về thông tin định tuyến đến mạng đích.
 - **A maximum metric to prevent count to infinity**: để dừng việc metric tăng lên do lặp định tuyến, giá trị metric được xác định là vô cực
 - **Hold-down timers**: Router được chỉ dẫn để dữ các thay đổi có thể tác động đến đường truyền trong một khoảng thời gian nhất định. Nếu đường truyền được xác định là đã ngắt hoặc có thể đã ngắt, các thông tin về tuyến đường đó sẽ bị từ trối trong một khoảng thời gian, để mạng có thời gian để hội tụ
 - **Split horizon**: Ngăn chặn lặp bằng cách không cho pháp tin quảng bá được gửi lại qua interface tạo ra nó. Ngăn chặn Router tăng metric.
 - **Route poisoning or poison reverse**: Tuyến đường được đánh dấu là không đến được trong bảng cập nhật định tuyến được gửi đến các Router.
 - **Triggered updates**: Bảng định tuyến cập nhật sẽ gửi ngay phản hồi về sự thay đổi định tuyến. Cập nhật được kích hoạt không đợi bộ hẹn giờ hết thời gian. Router phát hiện ngay lập tức gửi thông báo cập nhật đến các Router liền kề
 - **TTL field in the IP header**: Trường TimeToLive (TTL) tránh trường hợp trong đó có gói không gửi được tồn tại không ngừng trên mạng. Với TTL, thiết bị nguồn của gói tin đặt trường 8 bit với 1 giá trị, giá trị này giảm đi 1 sau mỗi Router trong đường đi đến khi gói tin đến đích, Nếu TTL giảm xuống 0 trước khi gói tin đến đích, gói tin sẽ bị hủy và Router gửi một tin báo ICMP về nguồn của gói tin

## Các tính năng của giao thức định tuyến Link-State
### Building the LSDB
- Router với Link-State cung cấp thông tin chi tiết về kết nối Internet đến tất cả các Router khác. Router sử dụng Link-State database (LSDB) tính toán tuyến đường tốt nhất đến các mạng connected
- Giao thức định tuyến Link-State sẽ quảng bá thông tin về cập nhật định tuyến gọi là link-state advertisements (LSA)

![alt text](https://i.imgur.com/Nk8Mi8X.png)
- R8 gửi quảng bá về cập nhật định tuyến đến tất cả các Router, nó còn gửi đến chính nó về cập nhật này. Các Router khác sẽ tiếp tục gửi LSA đến khi tất cả các Router có bản sao.
- Mặc định sau 30 phút giao thức Link-State lại tiếp tục gửi LSA khi không có thay đổi

### Tính toán thuật toán Dijkstra
- Giao thức Link-State phải tìm và thêm tuyến đường vào bảng định tuyến sử dụng thuật toán Dijkstra Shortest Path First (SPF)
- Thuật toán SPF hoạt động trên LSDB để tạo một cây SPF. LSDB giữ thông tin của tất cả các Router và đường dẫn có thể. Mỗi Router là điểm bắt đầu và mỗi Subnet là điểm đích dùng thuật toán SPF để xây dựng cây SPF giúp chọn tuyến đường tốt nhất đến mỗi Subnet

![alt text](https://i.imgur.com/Ps1uioO.png)
- Để chọn tuyến đường tốt nhất, Thuật toán SPF của Router thêm *cost* với mỗi liên kết giữa nó và Subnet đích.
- Theo hình có thể thấy mức *cost* tốt nhất là đi qua R5

### Hội tụ với giao thức Link-State
- Khi LSA thay đổi, giao thức Link-State xử lý và hội tụ mạng lại một cách nhanh chóng. Khi liên kết giữa 2 Router không thành công thì 2 Router đó sẽ gửi LSA báo rằng interface đã ở trạng thái ngắt. Các Router chạy lại thuật toán SPF để xem có thay đổi gì trong tuyến đường. Sau đó tất cả các Router thay đổi các tuyến đường theo kết quả SPF