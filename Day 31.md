# Mô hình mạng, thiết bị mạng, và các thành phần của chúng

## Nội dung chính trong ngày 31

- Mô hình kết nối các hệ thống mở (OSI) và giao thức điều khiển truyền vận/giao thức internet (TCP/IP) là 2 mô hình mạng quan trọng trong bộ khung các khái niệm để hiểu về hoạt động của mạng.
- Tìm hiểu về các lớp, chức năng của các mô hình.
- Tìm hiểu về các thiết bị mạng đang được sử dụng, các phương thức được sử dụng để kết nối các thiết bị đó với nhau và sự khác nhau của các cấu trúc liên kết mạng

## Mô hình OSI và TCP/IP
| OSI           | TCP/IP        |
|:-------------:|:-------------:|
| Application   |               |
| Presentation  |  Application  |
| Session       |               |
| Transport     | Transport     |
| Network       | Internet      |
| Data Link     | Network Access|
| Physical      |               |

- Note:
 - OSI cho lớp của một mô hình.
 - TCP/IP cho các giao thức.

# Các lớp của mô hình OSI

| Lớp (Layer)  | Chức năng |
| :-------------: |-------------|
| Ứng dụng (Application)| Giao diện giữa mạng và ứng dụng. Bao gồm các dịch vụ xác thực|
| Trình diễn (Presentation)|Xác định định dạng và cách tổ chức dữ liệu. Bao gồm mã hóa|
| Phiên (Session)|Thiết lập, duy trì luồng kết nối 2 chiều giữa các thiết bị cuối|
| Giao vận (Transport)| Quản lý các luồng dữ liệu trao đổi qua lại. Cung cấp các dịch vụ giữa 2 máy chủ, gồm thiết lập và kết thúc kết nối, kiểm soát luồng, khôi phục lỗi, phân đoạn các khối dữ liệu lớn thành các phần nhỏ để truyền|
| Mạng (Network)|Xác định địa chỉ, định tuyến, xác định đường dẫn|
| Liên kết dữ liệu (Data-Link)| Định dạng dữ liệu thành các kiểu phù hợp để truyền vào các thiết bị vật lý. Xác định quy tắc về thời điểm có thể sử dụng các thiết bị. Xác định biện pháp để nhận ra lỗi khi truyền|
| Vật lý (Physical )| Xác định các thiết bị điện,quang học,cáp nối,đầu nối và quy trình cần thiết để truyền các bit dưới dạng năng lượng truyền qua môi trường vật lý|

- Note:
 - **A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocesing
 
# Các lớp của mô hình TCP/IP và giao thức của chúng

| Lớp (Layer) | Chức năng| Ví dụ giao thức|
| :-------------: |-------------|------------|
| Application|Trình bày dữ liệu cho người dùng và kiểm soát các hội thoại|DNS,DHCP,POP3,...|
| Transport|Hỗ trợ giao tiếp giữa các thiết bị khác nhau qua nhiều mạng khác nhau|TCP,UDP|
| Internet|Xác định đường dẫn phù hợp qua mạng|IP,ARP,ICMP|
| Network Access|Kiểm soát các thiết bị phần cứng và phương tiện cấu thành nên mạng|Ethernet,Frame Relay|


| Protocol  | Chức năng |
| ------------- |-------------|
| Domain Name System (DNS)| Cung cấp địa chỉ IP của 1 website hoặc tên miền để máy chủ có thế kết nối đến|
| Telnet |Cho phép quản trị viên đăng nhập vào máy chủ từ xa.|
| Simple Mail Transfer Protocol (SMTP), Post Office Protocol (POP3),Internet Message Access Protocol (IMAP)|Các giao thức thuận tiện cho việc gửi mail giữa máy khách và máy chủ|
| Dynamic Host Configuration Protocol (DHCP)| Cấp phát địa chỉ IP tự động cho máy khách|
| Hypertext Transfer Text Protocol (HTTP)| Chuyển đổi thông tin giữa web của khách và web của máy chủ|
| File Transfer Protocol (FTP)|Giao thức thuận tiện cho việc download và upload file giữa máy khách và máy chủ |
| Simple Network Management Protocol (SNMP)|Bật hệ thống quản lý mạng để kiểm tra thiết bị được gắn trên mạng|
| Transmission Control Protocol (TCP)|Hỗ trợ kết nối ảo giữa các máy chủ trong mạng để cung cấp dữ liệu an toàn|
| User Datagram Protocol (UDP)|Hỗ trợ truyền dữ liệu nhanh, không an toàn các dữ liệu nhẹ hoặc các dữ liệu cần truyền nhanh trong khoảng thời gian ngắn|
| Internet Protocol (IP)|Cung cấp một địa chỉ duy nhất đến máy tính để giao tiếp qua mạng|
| Address Resolution Protocol (ARP)|Tìm địa chỉ phần cứng của máy chỉ khi chỉ biết IP|
| Internet Control Message Protocol (ICMP)|Gửi thông báo lỗi và kiểm soát chúng, bao gồm khả năng tiếp cận với 1 máy chủ khác và tình khả dụng của các dịch vụ|
| Ethernet|Tiêu chuẩn LAN phổ biến nhất để đóng gói và truyền dữ liệu lên các phương tiện|

# Protocol Data Unit and Encapsulation
- Các dữ liệu truyền xuống qua các giao thức xếp chồng được thêm các thông tin khác nhau ở mỗi lớp hay còn được gọi là quá trình đóng gói (encapsulation process). Cấu trúc dữ liệu ở mỗi lớp được gọi là giao thức đơn vị dữ liệu (protocol data unit)

| OSI Layer         | PDU        |
|:-------------:|:-------------:|
| Application   |Data   |
| Presentation  |Data|
| Session        |    Data |
| Transport     | Segment (phân đoạn)     |
| Network       |Packet (đóng gói)    |
| Data Link     | Frame|
| Physical      |      Bits         |

- Quá trình giao tiếp giữa từ nguồn đến đích:
1. Dữ liệu được tạo ở lớp ứng dụng của thiết bị nguồn.
2. Dữ liệu được đưa xuống qua các giao thức xếp trồng trong thiết bị nguồn, nó sẽ được phân đoạn và đóng gói.
3. Dữ liệu được tạo ra trên thiết bị ở lớp Network access
4. Dữ liệu được truyền qua mạng internet, bao gồm các thiết bị kết nối mạng và các thiết bị trung gian.
5. Thiết bị đích nhận dữ liệu ở lớp Network access.
6. Dữ liệu truyền qua các lớp giao thức xếp chồng của thiết bị đích sẽ được mở ra và tập hợp lại.
7. Dữ liệu được truyền đến ứng dụng đích ở lớp ứng dụng của thiết bị đích.

# Chi tiết các lớp của giao thức TCP/IP
## Application
- Lớp này cung cấp giao diện giữa phần mềm như trình duyệt và mạng lưới. Quá trình yêu cầu và nhận một trang web được hoạt động như sau:
1. Một giao thức HTTP được gửi đi, gồm chỉ dẫn để lấy lấy file.
2. Một giao thức HTTP phản hồi từ web server với đoạn mã được gắn trong header. 200(suceeded) 404(not found).
- Giao thức HTTP yêu cầu và phản hồi được đóng gói lại trong headers.
- Nội dung của header cho phép các lớp ứng dụng của các thiết bị đầu cuối giao tiếp với nhau.
- Các giao thức lớp ứng dụng đều có quy trình để giao tiếp giống nhau giữa các thiết bị cuối.

## Transport
- Lớp này thông qua giao thức TCP cung cấp một cơ chế để đảm bảo dữ liệu truyền qua mạng.
- TCP hỗ trợ khôi phục lỗi cho lớp ứng dụng thông qua việc dùng cơ chế xác nhận.
- Cơ chế hoạt động:
1. Web client gửi yêu cầu HTTP cho một web server bất kỳ xuống lớp transport.
2. TCP đóng gói yêu cầu HTTP với một header chứa địa chỉ port cho HTTP.
3. Các lớp dưới sẽ xử lý và gửi yêu cầu đến web server.
4. Web server nhận yêu cầu HTTP và gửi lại một tin báo nhận trở về web client.
5. Web server gửi phản hồi HTTP xuống lớp transport.
6. TCP đóng gói dữ liệu HTTP với một TCP header.
7. Các lớp dưới xử lý và gửi phản hồi đến web client.
8. Web client gửi lại một tin báo nhận đến web server.
- Nếu dữ liệu bị mất trong quá trình truyền, TCP phải phục hồi dữ liệu đó.
- Lớp này còn cung cấp UDP, giao thức này cần ít kết nối, không đảm bảo dữ liệu khi truyền.

### TCP Header
- TCP ngốn nhiều băng thông hơn cho việc phục hồi lỗi, và tốn nhiều chu trình xử lý hơn UDP.
- UDP và TCP dựa vào IP để giao tiếp giữa 2 thiết bị.

### Port Numers
- Port numbers cung cấp khả năng ghép các ứng dụng trên cùng một máy tính.
- Mỗi lần yêu 1 website, TCP sẽ chỉ định một địa chỉ port khác từ nguồn (1024 -> 65535)

| Port Number         | Protocol        |Application|
|:-------------:|:-------------:|-------------|
|20| TCP| FTP data|
|21 |TCP |FTP control|
|22| TCP| SSH|
|23| TCP| Telnet|
|25| TCP| SMTP|
|53| UDP, TCP| DNS|
|67, 68| UDP| DHCP|
|69| UDP| TFTP|
|80| TCP| HTTP (WWW)|
|110| TCP| POP3|
|161| UDP| SNMP|
|443| TCP| HTTPS (SSL)|
|16384–32767| UDP| RTP-based voice (VoIP) and video|

### Error Recovery
- 2 Trường Sequence (trình tự) và Acknowledgment trong TCP header sẽ kiểm tra từng byte của dữ liệu để đảm bảo dữ liệu bị mất sẽ được truyền tiếp.

![alt text](https://i.imgur.com/9oMHQPU.png)
 - Ở hình trên 3000 bytes dữ liệu đã được gửi thành công. Web browser đã nhận đủ dữ liệu và sẽ trả lại trường với Acknowledgment = 4000 với ngụ ý rằng số byte sẽ nhận tiếp theo là 4000.

![alt text](https://i.imgur.com/iXAYEvI.png)
 - Ở hình trên web browser được gửi đến 3000 bytes dữ liệu tuy nhiên ở đoạn dữ liệu 2000-2999 bị lỗi và không nhận được. Web browser sẽ gửi lại một trường ACK yêu cầu gửi lại dữ liệu trong đoạn từ 2000-2999. Sau khi nhận đủ tất cả các bytes web browser sẽ tiếp tục xử lý như bình thường

- Web server sẽ tạo một khoảng thời gian truyền dữ liệu và đợi tin báo để đề phòng khi tin báo bị mất hoặc tất cả các đoạn dữ liệu bị mất. Khi thời gian hết dữ liệu sẽ được gửi lại từ đầu

### Flow Control
- TCP kiểm soát luồng thông qua một trình gọi là *windowing*
- 2 thiết bị đầu cuối sẽ thương lượng kích thước của *window* khi thiết lập kết nốt ban đầu và tự động thương lượng kích thước dó trong suốt quá trình kết nối, tăng kích thước của chúng đến khi max kích thước của *window* = 65535 hoặc đến khi gặp lỗi. 
- Sau khi gửi dữ liệu trong kích thước cho phép của *window*, nguồn sẽ phải nhận một tin báo nhận trước khi gửi *window* tiếp theo.

### Connection Establishment and Termination
- Thiết lập kết nối là quá trình khởi tạo các trường Sequence và Acknowledgment, thống nhất về port number và kích thước của *window*
SYN,ACK: đại diện cho 1 bit cờ trong TCP header để báo hiệu kết nối

![alt text](https://i.imgur.com/uv0TLiz.png)
- TCP khởi tạo Sequence Number ngẫu nhiên trong 32bit số với mỗi lần truyền mới. Acknowledgment Number được nhận lại và tăng Sequence Number lên 1
1. Web browser với source port được tạo tự động SPORT=1027 gửi request với SEQ=200 được tạo ngẫu nhiên có cờ SYN đến DPORT=80(HTTP)
2. Từ Web server gửi lại web browser với SPORT=80 với SEQ=1450 được tạo ngẫu nhiên có cờ SYN,ACK
3. Web browser nhận Acknowledgment Number và tăng nó lên 1

![alt text](https://i.imgur.com/9etIEEV.png)
 - Khi hoàn thành quá trình truyền, trình kết thúc 4 bước xảy ra sử dụng cờ FIN (không gửi thêm số liệu)

### UDP
 - Khác với TCP, UDP không có độ tin cậy, không có *window*, không hệ thống lại dữ liệu
 - UDP với ít byte hơn và xử lý ít khâu hơn TCP
 - Ứng dụng dùng UDP sẽ có khả năng mất dữ liệu để có độ trễ thấp

## Internet
- Lớp này xác định địa chỉ IP khác nhau cho mỗi máy tính.
- Lớp này xử lý định tuyến để bộ định tuyến có thể xác định đường dẫn tốt nhất để gửi gói tin đến đích.
- IP còn xử lý định tuyến dữ liệu từ nguồn đến đích.
1. Web client gửi yêu cầu HTTP.
2. TCP đóng gói yêu cầu HTTP.
3. IP đóng gói các đoạn truyền tải thành một gói, thêm vào đó địa chỉ nguồn và đích.
4. Các lớp dưới xứ lý và gửi yêu cầu đến Web server.
5. Web server nhận yêu cầu HTTP và gửi một tin báo nhận trả về cho web client.
6. Web server gửi phản hồi HTTP xuống lớp transport.
7. TCP đóng gói dữ liệu HTTP.
8. IP đóng gói các đoạn truyền tải thành một gói, thêm vào đó địa chỉ nguồn và đích.
9. Các lớp dưới xử lý và gửi phản hồi lại cho web client.
10. Web client gửi một tin báo nhận về web server.


## Network Access
- IP phụ thuộc vào lớp này để chuyển các gói tin qua tầng vật lý
- Lớp này xác định giao thức và phần cứng cần thiết để truyền dữ liệu qua các thiết bị mạng vật lý bằng cách chỉ định cách kết nối các thiết bị mạng với nhau để dữ liệu có thể truyền qua đó
- Lớp này cung cấp nhiều giao thức khác nhau để xử lý các kiểu thiết bị khác nhau để dữ liệu có thể truyền từ nguồn đến đích.
- Một số trường hợp, địa chỉ liên kết cục bộ là cần thiết để truyền dữ liệu từ bước này đến bước tiếp theo.

- Tổng hợp quá trình yêu cầu và gửi đi của 1 trang web:
1. Web client gửi yêu cầu HTTP.
2. TCP đóng gói yêu cầu HTTP.
3. IP đóng gói các đoạn truyền tải thành một gói, thêm vào đó địa chỉ nguồn và đích.
4. Lớp Network Access đóng gói các gói tin thành một frame, xác định địa chỉ của nó cho liên kết nội bộ
5. Lớp Network Access gửi các frame dưới dạng bit trên các thiết bị.
6. Các thiết bị trung gian xử lý các bit ở lớp Network Access và lớp Internet, sau đó chuyển tiếp các dữ liệu đến đích.
7. Web server nhận các bit trên các thiết bị vật lý và gửi nó lên qua lớp Network Access và lớp Internet.
8. Web server gửi một tin báo nhận về web client.
9. Web server gửi phản hồi HTTP xuống lớp transport.
10. TCP đóng gói dữ liệu HTTP.
11. IP đóng gói các đoạn truyền tải thành một gói, thêm vào đó địa chỉ nguồn và đích.
12. Lớp Network Access đóng gói các gói tin thành một frame, xác định địa chỉ của nó cho liên kết nội bộ.
13. Lớp Network Access gửi các frame dưới dạng bit trên các thiết bị.
14. Các lớp dưới xử lý và gửi phản hồi đến web client.
15. Phản hồi được trả lại nguồn qua các liên kết dữ liệu khác nhau.
16. Web client nhận phản hồi trên các thiết bị vật lý và gửi dữ liệu lên qua lớp Network Access và lớp Internet.
17. Web client gửi một tin báo nhận về web server.
18. Trang web được hiển thị trên trình duyệt của thiết bị.

## Data Encapsulation
- Các lớp khác nhau của mô hình TCP/IP sẽ được đóng gói với một header mới.
1. Tạo và đóng gói dữ liệu với header của lớp Application
2. Đóng gói dữ liệu được cấp bởi lớp Application trong header lớp Transport. TCP hoặc UDP header thường được sử dụng ở các ứng dụng người dùng cuối.
3. Đóng gói dữ liệu được cấp bởi lớp Transport trong header lớp Internet. 
4. Đóng gói dữ liệu được cấp bởi lớp Internet trong header và trailer(phần đuôi) lớp Network Access.
5. Truyền các bit. Lớp vật lý mã hóa tín hiệu vào phương tiện để truyền các frame.

## Devices
| Thiết bị        | Mô tả        |
|:-------------:|:-------------:|
| Hub    | Là lựa chọn cơ bản cho các LAN nhỏ, trong đó việc sử dụng băng thông không phải là một vấn đề hoặc chi phí là một yếu tố   |
| Switches    |Switches thay thế hub làm các thiết bị trung gian trong LAN vì switch có thể phân đoạn các miền xung đột (conllision domains) và cung cấp thêm khả năng bảo mật |

### Switches
#### Access Layer Switches
- Access Layer Switches tạo kết nối các thiết bị đầu cuối với mạng lưới.
 - Tính năng:
  - Port security
  - VLANs
  - Fast Ethernet/Gigabit Ethernet
  - Power over Ethernet (PoE)
  - Link aggregation
  - Quality of service (QoS)
#### Distribution Layer Switches
- Distribution Layer Switches nhận dữ liệu từ Access Layer Switches và truyền nó đến Core Layer Switches
 - Tính năng:
  - Layer 3 support
  - High forwarding rate
  - Gigabit Ethernet/10 Gigabit Ethernet
  - Redundant components
  - Security policies/access control lists
  - Link aggregation
  - QoS
#### Core Layer Switches
- Core Layer Switches tạo thành cốt lõi(backbone) và chịu trách nhiệm xử lý phần lớn các Switches trong LAN.
 - Tính năng:
  - Layer 3 support
  - Very high forwarding rate
  - Gigabit Ethernet/10 Gigabit Ethernet
  - Redundant components
  - Link aggregation
  - QoS

### Routers
- Routers là thiết bị chính được sử dụng để liên kết các mạng với nhau - LAN,WAN,WLAN

### Specialty Devices
#### Firewalls
- Firewall là một thiết bị mạng gồm cả phần cứng hoặc phần mềm, kiểm soát truy cập trong mạng trong các tổ chức. Bảo vệ dữ liệu từ các mối nguy hại bên ngoài.
- Các tổ chức triển khai firewall về phần mềm qua các hệ thống như Windows server, Linux,...
- Firewall được cấu hình cho phép chặn các kiểu dữ liệu không mong muốn.
- Firewall thường là các thiết bị mạng chuyên dụng.

#### IDS and IPS
- Đây là 2 hệ thống phát hiện xâm nhập (Intrusion Detection Systems) và ngăn chặn xâm nhập (Intrusion Prevention Systems) có thể phát hiện tấn công.
- IDS nhận một bản sao các lưu lượng để phân tích. Đây là một hệ thống phát hiện gián tiếp. Nó có thể phát hiện sự hiện diện của cuộc tấn công, ghi lại log và gửi cảnh báo.
- IPS có cùng chức năng như IDS. IPS hoạt động liên tục để quét trong mạng lưới để tìm kiếm các hoạt động trái phép. Nó có thể chặn bất cứ khả năng đe dọa nào đến hệ thống.

### Access Points and Wireless LAN Controllers
- WLAN là một phần quan trọng của kết nối mạng. Người dùng có thể kết nối khi di chuyển từ vị trí này đến vị trí khác khi ở trong một khu vực. Để có kết nối này, người quản trị mạng phải tập hợp các điểm truy cập không dây (Access Points) và các bộ điều khiển mạng LAN không dây (Wireless LAN Controllers)

![alt text](https://i.imgur.com/iiq0waY.png)
- AP có cổng Ethernet kết nối với cổng switch. AP có thể có thể trở thành 1 wireless router không có các tính năng của layer 3
- AP còn có thể dùng khi khu vực phủ sóng của WLAN cần mở rộng.

![alt text](https://i.imgur.com/FxsD8Ak.png)
- Trông các mô hình mạng lớn hơn cần sử dụng một WLC để quản lý các AP
- Dùng WLC,VLAN có thể cấp IP cho các thiết bị kết nối không dây từ các mạng con.

## Physical
- Trước khi bất kì kết nối mạng nào được diễn ra, một kết nối có dây hoặc không dây phải được thiết lập. Kiểu của kết nối này phụ thuộc vào cách thiết lập mạng. Ở hệ thống mạng lớn hơn, Switches và AP thường được chia ra các thiết bị chuyên dụng khác nhau
- Có 3 loại chính:
 - Copper cable(cáp đồng): Tín hiệu là các mẫu của xung điện
 - Fiber-optic cable(cáp quang): Tín hiện là các mẫu của ánh sáng 
 - Wirless(không dây): Tín hiệu là các mẫu truyền của vi sóng
 
## LAN Device Connection Guidelines
- Kết nối các thiết bị trong LAN thường dùng cáp xoắn hở (UTP). Các thiết bị mới hơn có khả năng kích hoạt cả cáp thẳng hoặc cáp chéo
- Quy luật kết nối:
 - Cáp thẳng:
  - Switch nối đến cổng Ethernet của router
  - Máy tính nối đến switch
  - Máy tính nối đến hub
 - Cáp chéo:
  - Switch nối đến switch
  - Switch nối đến hub
  - Hub nối đến hub
  - Router nối đến router
  - Máy tính đến máy tính
  - Máy tính đến cổng Ethernet của router

## LAN and WAN
- Local Area Network(LAN) là một mạng của các máy tính và các thành phần liên quan nằm gần nhau trong một khu vực. LAN có thể thay đổi độ rộng bên trong nó, từ 1 máy tính đến router, hay đến hàng trăm máy tính khác. Tuy nhiên LAN có hạn chế về mặt địa lý.
- Các thành phần cơ bản cấu thành nên một LAN gồm
 - Máy tính (Computers).
 - Các kết nối (Interconnections)
 - Thiết bị mạng (Networking devices)
 - Các giao thức (Protocols)
- Wide Area Network(WAN) về cơ bản sẽ kết nối các LAN ở các khu vực địa lý khác nhau lại
- Tập hợp các LAN kết nối bởi 1 hay nhiều WAN được gọi là Internet
- Tùy theo kiểu của dịch vụ, kết nối với WAN thường hoạt động theo 4 cách:
 - RJ-11 nối với các thiết bị nối nhất thời hoặc DSL modem
 - Cáp đồng trục nối với cáp modem
 - Kết nối nối tiếp cáp 60-pin với CSU/DSU
 - Bộ điều khiển RJ-45 T1 nối với CSU/DSU
- Công nghệ điều khiển kết nối hỗ trợ người dùng từ xa theo:
 - Các công nghệ WAN truyền thống, gồm Frame Relay, ATM, leased lines(đường kết nối riêng).
 - Mạng riêng ảo (VPN)
 - Điều khiển truy cập VPN qua một kết nối có băng thông rộng ra mạng Internet
 
## Networking Icons
![alt text](https://i.imgur.com/w87QsCR.png)
- Biểu tượng các thiết bị mạng

## Physical and Logical Topologies
- Sơ đồ mạng thường được gọi là cấu trúc liên kết(topologies). Một cấu trúc liên kết bằng đồ thị sẽ hiển thị các phương pháp kết nối giữa các thiết bị.

![alt text](https://i.imgur.com/1Tb1WmN.png)
 - Điểm đến điểm (Point-to-Point)
 - Hình bus (Bus)
 - Hình vòng (Ring)
 - Hình sao (Star)
- Theo logic, Ethernet hoạt động theo cấu trúc hình bus. Tuy nhiên mạng Ethernet hầu như được thiết kế dạng hình sao hoặc hình sao mở rộng (star extended)
- Các phương thức truy cập khác thường dùng cấu trúc hình vòng (Ring)

## Hierarchical Campus Designs
- Thiết kế phân cấp hay hình cây chia mậng thành các lớp rời rạc. Mỗi lớp cung cấp một chức năng khác nhau. Điều này khiến thiết kết mạng trở thành các modun, Điều này làm tăng khả năng mở rộng và tăng hiệu năng
- Thiết kế phân cấp chia làm 3 lớp: 

![alt text](https://i.imgur.com/lPNSoj1.png)
 - Access layer: Cung cấp quyền truy cập trong nội bộ và người dùng từ xa
 - Distribution layer: Kiểm soát lưu lượng dữ liệu giữa lớp truy cập vào lớp lõi
 - Core layer: Hoạt động như mội cốt lõi(backbone) dự phòng với tốc độ cao
 
![alt text](https://i.imgur.com/cWvg270.png)
 - Với mô hình mạng nhỏ hơn Core layer thường được thu gọn thành Distribution layer để có được thiết kế 2 tầng
 - Với thiết kế 2 tầng này giải quyết được các vấn đề thiết kế:
  - Cung cấp một nơi để kết nối các thiết bị người dùng cuối
  - Kết nối các switch với một số lượng cáp phù hợp bằng cách kết nối tất cả các access switch đến distribution switch
