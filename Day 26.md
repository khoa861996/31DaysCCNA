# IPv6

## Nội dung chính trong ngày 26
- Giao thức IPv6 và kiểu địa chỉ của IPv6, thực hiện đặt địa chỉ, chia mạng con, tự động cấu hình cho host và cấu hình chạy cả IPv4 và IPv6 

## Tổng quan và lợi ích của IPv6
- IPv6 đáp ứng nhu cầu ngày càng phức tạp trong việc phân cấp địa chỉ mà IPv4 không làm được.
- Lợi ích:
 - **Extended address space**: với 128 bit địa chỉ cung cấp 340 nghìn nghìn nghìn tỷ địa chỉ
 - **Stateless address autoconfiguration**: Cho phép các host tạo phương thức định tuyến IPv6.
 - **Eliminates the need for NAT/PAT**: Với IPv6 việc cạn kiệt IP không còn là vấn đề. NAT64 cung cấp khả năng tương thích ngược với IPv4
 - **Simpler header**: Header đơn giản cung cấp nhiều lợi thế hơn IPv4
  - Hiệu quả định tuyến tốt hơn và mở rộng tốc độ chuyển tiếp
  - Không có bản tin broadcasts, không bị đe dọa bởi bão các broadcasts
  - Không cần thiết xử lý checksums
 - **Mobility and security**: Tính di động và bảo mật giúp đảm bảo các tiêu chuẩn IP động và IPsec:
  - IPv4 không tự động cho phép thiết bị di động di chuyển mà không làm ngắt kết nối
  - IPv6 xây dựng sẵn tính di động
  - IPsec được kích hoạt trên mọi nút của IPv6 khi được sử dụng, tăng cường bảo mật
 - **Transition strategies**: Tăng cường khả năng hoạt động của IPv4 với các tính năng của IPv6:
  - Sử dụng cả IPv4 và IPv6 trên cùng một thiết bị mạng
  - Sử dụng Tunnel
 
## Các kiểu địa chỉ IPv6
- IPv6 sử dụng unicast, multicast, anycast

![alt text](https://i.imgur.com/CGmvmwz.png)
- Địa chỉ Unicast xác định duy nhất một giao diện trên thiết bị sử dụng IPv6. Một gói tin được gửi đến địa chỉ Unicast được nhận bởi giao diện được chỉ định cho địa chỉ đó.
### Global Unicast Address
- Địa chỉ IPv6 Unicast là duy nhất trên toàn cầu. Gồm 48 bit định tuyến toàn cầu, 16 bit cho ID của subnet, 64 bit cho ID của interface.
- Trong IPv6 một giao diện có thể được cấu hình với nhiều địa chỉ global unicast ở cùng hoặc khác subnet, một giao diện có thể không cần cấu hình địa chỉ global unicast nhưng phải có địa chỉ link-local
#### Link-Local Address
- Địa chỉ Link-Local là một kiểu của địa chỉ Unicast. Địa chỉ Link-Local được giới hạn trong 1 liên kết duy nhất. Nó phải là duy nhất cho liên kết đó vì gói tin với địa chỉ link-local nguồn hay đích không thể định tuyến ngoài liên kết
- Địa chỉ Link-Local được cấu hình theo 3 cách:
 - Tự động, dùng EUI-64
 - Tạo ngẫu nhiên 
 - Tĩnh, tự tạo bằng tay
- Địa chỉ Link-Local cung cấp lợi thế cho IPv6. 1 thiết bị có thể tự tạo Link-Local. Địa chỉ Unicast của Link-Local nằm trong đoạn FE80::/10 đến FEBF::/10
#### Loopback Address
- Địa chỉ loopback của IPv6 là tất cả địa chỉ 0 ngoại trừ bit cuối được đặt là 1. Dùng để kiểm tra giao thức TCP/IP như IPv4
#### Unspecified Address
- Địa chỉ Unicast không xác định là tất cả địa chỉ 0 (::). Nó không thể được gán vào interface nhưng cần để giao tiếp khi thiết bị gửi chưa có IPv6
#### Unique Local Address
- Đây là địa chỉ riêng tư. Tuy nhiên với IPv6 nó chỉ có duy nhất trên toàn cầu.
- Đặc điểm:
 - Sở hữu tiền tố duy nhất trên toàn cầu hoặc các xác suất cao là duy nhất
 - Cho phép các trang web được kết hợp hoặc kết nối với nhau một cách riêng biệt mà không có xung đột địa chỉ.
 - Độc lập với bất kỳ ISP nào và có thể sử dụng trong một trang web mà không cần kết nối mạng
 - Nếu bị dò rỉ ra ngoài một trang web do định tuyến hoặc do hệ thống tên miền thì không xảy ra xung đột với bất kỳ địa chỉ nào.
 - Có thể chỉ là một địa chỉ Unicast toàn cầu
#### IPv4 Embedded Address
- Phải dùng phương pháp NAT64 để biên dịch giữa IPv4 và IPv6. IPv4 biên dịch IPv6 trên các host và router để tạo một tunnel truyền gói tin IPv6 qua mạng sử dụng IPv4

### Multicast
- Multicast là công nghệ dùng cho thiết bị để gửi một gói tin đến nhiều địa chỉ ngay lập tức (FF00:/8) gồm 2 loại:
 - Assigned Multicast
 - Solicited-Node Multicast

### Anycast
- Địa chỉ này có thể được chỉ định ở nhiều thiết bị hoặc interface. Gói tin sử dụng anycast sẽ được định tuyến đến thiết bị gần nhất được cấu hình anycast

## Giới thiệu về địa chỉ IPv6
### Quy ước để viết địa chỉ IPv6
- IPv6 dùng 32 chữ số thập lục phân
- VD: 2340:1111:AAAA:0001:1235:5678:9ABC
 - Bỏ qua số 0 ở bất kì hextet nào
 - Bỏ qua hextet có tất cả là số 0
- VD 1: FE00:0000:0000:0001:0000:0000:0000:0056
 => Viết ngắn lại: FE00::1:0:0:0:56 (**Không được dùng hai lần ::)
### Quy ước để viết tiền tố IPv6
- Tiền tố của IPv6 đại diện cho một dải hoặc một khối các địa chỉ IPv6 liên tiếp. Số đại diện cho dải địa chỉ gọ là prefix, thường được thấy trong bảng định tuyến
- Cũng như IPv4 khi viết tiền tố cho IPv6, bit vượt quá độ dài của tiền tố đều là số 0
- VD:  2000:1235:5678:9ABC:1235:5678:9ABC:1111/64
 - Tiền tố của địa chỉ: 2000:1235:5678:9ABC:**0000:0000:0000:0000**/64
 - Viết ngắn lại: 2000:1235:5678:9ABC::/64
- Nếu độ dài của tiền tố không nằm trong đoạn ngăn cách giữa các hextet, giá trị của tiền tố phải liệt kê toàn bộ các giá trị của hextet cuối
- VD: 2000:1235:5678:9ABC:1235:5678:9ABC:1111/56
 - Viết ngắn lại: 2000:1235:5678:9A00::/64
- Chú ý khi viết tiền tố IPv6:
 - Tiền tố có cùng giá trị như địa chỉ IP trong nhóm cho các số đầu tiên của các bit
 - Các bit sau phần tiền tố đều là số 0
 - Tiền tố có thể viết ngắn lại theo quy tắc của địa chỉ IPv6
 - Nếu độ dài tiền tố không nằm trên đoạn ngăn cách giữa các hextet, phải viết xuống tất cả các giá trị của toàn bộ hextet
 
## IPv6 Subnetting
 - Một trang web được đặt một địa chỉ IPv6 với độ dài prefix là /48, 16 bit subnet ID và /64 subnet prefix
 VD:  2001:0DB8:000A::/48 (2001:DB8:A::/48) gồm 2^16 subnet và 2^64 địa chỉ interface
### Subnetting the Subnet ID
 - Với các công ty nhỏ, chỉ cần tăng bit cuối của Subnet ID lên
### Subnetting into the Interface ID
 - Nên chia mạng con ở các khoảng ngăn cách

## Stateless Address Autoconfiguration
- **Stateless Address Autoconfiguration (SLAAC)**: Host tự động nhận prefix /64 qua IPv6 Neighbor Discovery Protocol (NDP) và tính toán tất cả cácc địa chỉ bằng cách dùng EUI-64
- **DHCPv6**: Hoạt động giống DHCP của IPv4