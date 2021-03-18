# Bộ chuyển mạch Ethernet (Ethernet Switching)

## Nội dung chính trong ngày 30

- Khái niệm về Ethernet switching, lịch sử, phương thức hoạt động và tính năng của chúng.
- Cách truy cập các thiết bị của Cisco, các câu lệnh IOS để điều hướng các câu lệnh (CLI), và chi tiết về cách hoạt động của Ethernet

## Evolution to Switching (Lịch sử)
- Ban đầu các thiết bị sử dụng kết nối vật lý bằng cáp đồng trục. Với sự ra đời của cáp 10BASE-T và UTP, hub trở nên phổ biến với mức giá rẻ và dễ dàng để kết nối các thiết bị. Tuy nhiên cáp 10BASE-T có những giới hạn:
 - Một frame ở thiết bị này có thể xung đột với một frame ở thiết bị khác trên cùng LAN. Các thiết bị ở cùng miền xung đột(collision domain) chia sẻ cùng băng thông.
 - Các thiết bị trong cùng LAN có thể nhận và xử lý Broadcasts(quảng bá tin từ 1 điểm đến tất cả các điểm khác cùng mạng) được gửi từ 1 thiết bị.
- Ethernet bridges ra đời để giải quyết các vấn đề trong chia sẻ LAN. Bridge chia 1 LAN thành 2 miền xung đột giúp giảm thiểu số miền xung đột trong một LAN. Điều này làm tăng hiệu năng của mạng bằng cách giảm lưu lượng truy cập không cần thiết.
- Switches ra đời với những tình năng có sẵn của Bridge và bổ sung thêm:
 - Số lượng lớn lớn giao diện để chia các miền xung đột làm nhiều đoạn hơn.
 - Chuyển mạch dựa trên phần cứng để quyết định thay vì phần mềm.
- Mỗi máy tính cắm vào các cổng riêng biệt của switch sẽ có một miền xung đột riêng biệt và chúng sẽ có một đoạn riêng làm tăng thông lượng của mạng một cách đáng kể. Lý do:
 - Mỗi cổng sẽ có một băng thông riêng biệt
 - Tạo ra môi trường riêng biệt không xảy ra xung đột
 - Hoạt động song công
 
## Switching Logic
- Ethernet switch chọn lọc các frame riêng lẻ từ cổng nhận đến cổng đích. Trong quá trình này Switch sẽ tạo một băng thông toàn phần, có logic, kết nối điểm-điểm giữa 2 nút.
- Switch tạo ra một kết nối logic dựa trên địa chỉ MAC nguồn và đích trên header của Ethernet. Nhiệm vụ chính của Switch trong LAN là sẽ nhận các Ethernet frame và quyết định chuyển tiếp hoặc từ chối chúng. Để thực hiện điều này, Switch thực hiện qua 3 bước:
1. Quyết định khi nào chuyển tiếp một frame hoặc lọc frame dựa trên địa chỉ MAC đích.
2. Tìm địa chỉ MAC bằng cách kiểm tra địa chỉ MAC nguồn của mỗi frame mà Swtich nhận được
3. Tạo một môi trường không lặp với các Switch khác bằng cách sử dụng Spanning Tree Protocol (STP)
- Để quyết định chuyển tiếp hay lọc, Switch sử dụng bảng địa chỉ MAC được xây dựng tự động để so sánh các frame của địa chỉ MAC đích với các trường trên bảng
- Thêm vào đó để chuyển tiếp hoặc lọc các frame, Switch sẽ làm mới thời gian ở MAC nguồn của frame

## Collision and Broadcast Domains
- Một miền xung đột là tập hợp các giao diện LAN mà có các frame có thể xung đột với nhau(Vd: hub). Khi một host được kết nốt với switch, switch sẽ tạo ra mọt kết nối riêng biệt để loại bỏ xung đột. Swtich giảm xung đột và cải thiện băng thông dùng trong các phân đoạn mạng vì chúng cung cấp băng thông riêng biệt cho mỗi đoạn mạng.
- Switch không thể giúp giảm lưu lượng từ broadcast. Một tập hợp các kết nối với Switch tạo thành một miền quảng bá(broadcast domain). Nếu một frame với địa chỉ đích là FFFF.FFFF.FFFF đi qua switch, Switch đó phải đẩy frame này đến tất cả các cổng đang được sử dụng. Mỗi thiết bị phần cứng phải xử lý broadcast frame ít nhất đến lớp Netwrok. Router và VLAN dùng để phân đoạn miền quảng bá

## Frame Forwarding
- Switch có nhiều phương pháp chuyển tiếp frames. Chúng có thể khác nhau về phương pháp truyền, tốc độ truyền qua cổng, bộ nhớ đệm, và lớp OSI được sử dụng để lựa chọn chuyển tiếp
### Switch Forwarding Methos
- **Store-and-forward switching**: Switch lưu các frame nhận được trong bộ nhớ đệm, phân tích mỗi frame để biết được đích, và đánh giá tính toàn vẹn dữ liệu bằng cách sử dụng CRC (Cyclic Redundancy Check) trong phần đuôi của frame. Tất cả frame sẽ đc lưu và kiểm tra tính toàn vẹn trước khi được chuyển tiếp.
- **Cut-through switching**: Switch sẽ lưu đệm vừa đủ frame để đọc địa chỉ đích để nó có thể xác định cổng nào để chuyển tiếp dữ liệu
- **Fragment-free**: Switch đợi *collision window* đi qua trước khi chuyển tiếp frame. Có nghĩa là mỗi frame sẽ đc kiểm tra trường dữ liệu để chắc chắn không xảy ra phân mảnh. Chế độ Fragment-free cung cấp khả năng kiểm tra lỗi tốt hơn kiểu Cut-Through và hầu như không tăng độ trễ
### Symmetric and Asymmetric Switching
- Chuyển mạch đối xứng cung cấp kết nối chuyển mạch giữa các cổng có cùng băng thông
- Chuyển mạch không đối xứng cung cấp kết nối chuyển mạch giữa các cổng khác băng thông
### Memory Buffering
- Switch lưu các frame ở bộ nhớ đệm trong một khoảng thời gian ngắn
- 2 phương pháp: 
 - **Port-based memory**: Các frame được lưu trong hàng chờ được liên kết đến một cổng cụ thể
 - **Shared memory**: Các frame được gửi vào một bộ nhớ đệm chung mà tất cả các switch chia sẻ cho nhau
### Layer 2 and Layer 3 Switching
- Switch layer 2 thực hiện chuyển mạch và lọc chỉ dựa trên địa chỉ MAC.
- Switch layer 3 có những tính năng của Switch L2 và còn dùng cả thông tin về địa chỉ IP. Switch L3 có thể thực hiện định tuyến, giảm thiểu số lượng Router trong LAN. Vì Switch L3 có khả năng chuyên về chuyển mạch ở phần cứng, chúng có thể định tuyến dữ liệu nhanh như chuyển mạch dữ liệu.

## Ethernet
- 802.3 là tiêu chuẩn IEEE cho Ethernet. 802.3 và Ethernet đều xác định lớp Physical và Data Link của công nghệ LAN

[Github](https://imgur.com/j2O2H9z)
- Ethernet chia chức năng của lớp Data Link thành lớp con riêng biệt:
 - **Logical Link Control (LLC) sublayer**: Được xác định trong tiêu chuẩn 802.2
 - **Media Access Control (MAC) sublayer**: Được xác định trong tiêu chuẩn 802.3
- Lớp con LLC xử lý giao tiếp giữa lớp Netwrok và lớp con MAC. LLC cung cấp cách xác định giao thức chuyển từ lớp Data Link đến lớp Netwrok.
- Lớp con MAC có 2 nhiệm vụ chính
 - **Data encapsulation**: Bao gồm tập hợp các frame trước khi truyền, phân tích các frame khi tiếp nhận. Lớp Data Link đánh địa chỉ MAC và kiểu tra lỗi.
 - **Media Access Control** Vì Ethernet là thiết bị được chia sẻ và tất cả các thiết bị có thể truyền qua bất cứ lúc nào, nên việc truy cập các phương tiện được kiểm soát bởi một phương thức gọi là Carrier Sense Multiple Access with Collision Detection (CSMA/CD) khi hoạt động bán song công.
 - Ở lớp Physical, Ethernet chỉ định và triển khai mã hóa và giải mã cho phép các frame truyền dưới dạng tín hiệu qua cáp UTP và cáp quang.
 
## Legacy Ethernet Technologies
[Github](https://imgur.com/crlqM02)
- Một chuỗi các cáp tạo ra một mạch điện, gọi là Bus, được chia sẻ giữa các thiết bị trên Ethernet. Khi 1 máy tính muốn gửi 1 số bit đến máy tính khác trên Bus, nó gửi một tín hiệu điện đến tất cả các thiết bị trên Ethernet
[Github](https://imgur.com/xYkpaQP)
- Bất cứ sự thay đổi nào trong cấu trúc liên kết vật lý từ kiểu bus sang kiểu star(sao), hub sẽ hoạt động một cách logic tương tự như cấu trúc liên kiết hình bus và yêu cầu sử dụng CSMA/CD

### CSMA/CD
- Ethernet chỉ xác định duy nhất một thiết bị gửi lưu lượng tại một thời điểm. Thuật toán CSMA/CD xác định cách truy cập Ethernet bus.
- CSMA/CD giúp phòng ngừa xung đột và xác định cách xử lý khi xảy ra xung đột
- Thuật toán CSMA/CD hoạt động như sau:
1. Thiếtt bị với một frame gửi tín hiệu nghe cho đến khi Ethernet rảnh
2. Thiết bị bắt đầu gửi frame
3. Bên gửi tiếp tục nghe tín hiệu để đảm bảo không có xung đột
4. Nếu xảy ra xung đột, Thiết bị gửi frame đi sẽ gửi tín hiệu gây nhiễu để đảm bảo các trạm nhận ra xung đột.
5. Khi hoàn tất việc gây nhiễu, Bên gửi chọn ngẫu nhiên một bộ đếm và đợi trong khoảng thời gian đó trước khi gửi lại các frame bị xung đột
6. Khi thời gian bộ đếm hết, quá trình xử lý bắt đầu lại từ đầu
- Khi CSMA/CD hoạt động, Card mạng (NIC) hoạt động bán song công, gửi hoặc nhận frame. CSMA/CD bị tắt khi NIC phát hiện hoạt động để chế độ song công.

## Current Ethernet Technologies
| Tên           | Tốc độ        | Tên thay thế|Tên trong tiêu chuẩn IEEE|Loại cáp,độ dài|
|:-------------:|:-------------:|-------------|-------------|------------|
| Ethernet    |10 Mbps   | 10BASE-T|802.3|Đồng, 100m|
| Fast Ethernet  |  100 Mbps  | 100BASE-TX|802.3u|Đồng, 100m|
| Gigabit Ethernet           |1000 Mbps| 1000BASE-LX|802.3z|Quang, 550m|
| Gigabit Ethernet  | 1000 Mbps     | 1000BASE-T|802.3ab|Đồng, 100m|
| 10GigE  | 10 Gbps      | 10GBASE-T|802.3an|Đồng, 100m|

## UTP Cabling
- 10BASE-T, 1000BASE-T, 100BASE-TX là 3 tiêu chuẩn Ethernet thường dùng sử dụng cáp UTP. Chúng khác nhau ở số dây và kiểu cáp
- Cáp UTP gồm 2 hoặc 4 cặp dây, dùng kết nối RJ-45 gồm 8 dây gọi là pin

## Benefits of Using Switches
- Dùng CSMA/CD để phát hiện và giải quyết xung đột
- Switch trong LAN được giảm thiểu, hoặc triệt tiêu xung đột trong LAN
 - Switch xem các bit khi nhận các frame để có thể gửi các frame này ra một cổng theo yêu cầu chứ không phải tất cả các cổng.
 - Nếu Switch cần chuyển tiếp nhiều frame từ 1 cổng, Switch sẽ lưu các frame đấy vào bộ đệm và gửi từ frame một để tránh sung đột.

## Ethernet Addressing
- Tiêu chuẩn IEEE xác định định dạng và chỉ định địa chỉ của LAN. Để đảm bảo địa chỉ MAC là đọc nhất, nửa đầu của địa chỉ để nhận diện nhà sản xuất Card. Nửa sau của địa chỉ được chỉ định bảo nhà sản xuất và không bao giờ giống nhau ở các Card
[Github](https://imgur.com/nFvNonP)
- Nhóm địa chỉ cho Ethernet:
 - **Broadcast addresses**: Mọi thiết bị đều xử lý frame (FFFF.FFFF.FFFF)
 - **Multicast addresses**: Cho phép mạng con của thiết bị trong LAN giao tiếp với nhau (0100.5exx.xxx)

## Ethernet Framing
- Lớp vật lý giúp ta nhận được 1 chuỗi các bit từ thiết bị này đến thiết bị khác. Thuật ngữ *framing* cho phép các thiết bị nhận dịch các bit. Thuật ngữ *framing* xác định ý nghĩa các bit được truyền và nhận qua mạng

[Github](https://imgur.com/uEDZfdj)

| Trường           | Độ dài trường (Bytes)        |Mô tả|
|:-------------:|:-------------:|-----------|
| Preamble       |7|Đồng bộ |
| Start Frame Delimiter(SFD)  |  1  |Báo hiệu byte tiếp theo bắt đầu trường MAC đích|
| Destination MAC Address             |6|Xác nhận địa chỉ nhận của frame|
| Source MAC Addres     | 6    |Xác định thiết bị gửi frame|
| Length     | 2     |Xác định độ dài của trường dữ liệu của frame|
| Type     | 2|Xác định kiểu giao thức được liệt kê trong frame|
| Data and Pad     | 46-1500|Giữ dữ liệu từ lớp cao hơn, L3, thường là gói IP |
| Frame Check Sequence (FCS)      |   4            |Cung cấp phương thức để NIC nhận có thể xác định xem frame có bị lỗi truyền nào hay không |

## The Role of the Physical Layer
- Lớp Physical của mô hình OSI chấp nhận một frame hoàn chỉnh từ lớp Data Link và mã hóa chúng như một chuỗi tín hiệu truyền vào các phương tiện cục bộ
- Việc phân phối các frame qua các phương tiên cục bộ yêu cầu các yêu cầu vật lý sau: 
 - Các phương tiện vật lý và các kết nối cần thiết
 - Biểu diễn các bit trên các phương tiện này.
 - Mã hóa dữ liệu và kiểm soát thông tin.
 - Mạch phát và thu ở trên thiết bị mạng.
- Dữ liệu được biểu diễn trên 3 dạng cơ bản của thiết bị mạng:
 - Cáp đồng
 - Cáp quang
 - Không dây (802.11)
- Bit được biểu diễn trên các thiết bị bằng các đổi một hay nhiều các đặc điểm của tín hiệu:
 - Biên độ
 - Tần số
 - Chu kỳ