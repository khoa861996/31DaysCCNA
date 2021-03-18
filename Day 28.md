# Khái niệm về VLAN, Trunking và cấu hình chúng

## Nội dung chính trong ngày 28
- Các mạng lớn hiện nay đều triển khai mạng mạng cục bộ ảo (VLANs). Nếu không có VLANs, Switch sẽ coi các port đều ở trong cùng một miền quảng bá. Port của Switch sẽ được chia làm các nhóm trong các VLANs khác nhau (chia đoạn miền quảng bá)
- Tìm hiểu về khái niệm VLAN, kiểu truyền dữ liệu, các kiểu VLAN, khái niệm trunking, gồm Dynamic Trunking Protocol (DTP). Câu lệnh cấu hình VLANs và trunking

## Khái niệm VLAN
- Switch ban đầu mặc định có 1 VLAN, tuy nhiên có thể tự cấu hình thành nhiều VLAN khác nhau. Điều này sẽ tạo ra nhiều miền quảng bá bằng cách đặt nhiều giao diện vào 1 VLAN.
- Những lý do để sử dụng VLAN:
 - Nhóm các người dùng lại khu mà không cần dựa trên khu vực vật lý.
 - Chia đoạn các thiết bị thành nhiều LAN nhỏ giảm thiểu việc xử lý cho tất cả các thiết bị trên LAN
 - Giảm thiểu hoạt động của STP bằng cách giới hạn 1 VLAN đến 1 Switch
 - Tăng cường bảo mật bằng cách tách riêng dữ liệu nhạy cảm thành một VLAN riêng biệt
 - Tách riêng lưu lượng thoại với lưu lượng dữ liệu
 - Giúp xử lý lỗi bằng cách giảm kích cỡ của miền lỗi
- Lợi ích khi sử dụng VLAN:
 - **Security**: Dữ liệu nhạy cảm sẽ được tách riêng ra 1 VLAN
 - **Cost reduction**: Giảm thiểu chi phí nâng cấp mạng, sử dụng hiệu quả hơn băng thông hiện có
 - **Higher performance**: Chia L2 Network thành nhiều miền quảng bá một cách logic, giảm thiểu lưu lượng không cần thiết trong mạng và tăng hiệu năng
 - **Broadcast storm mitigation**: Chia đoạn VLAN ngăn chặn việc lượng tin quảng bá lan truyền trên toàn hệ thống mạng
 - **Ease of management and troubleshooting**: Chia các địa chỉ mạng thành nhóm theo liền kề. Việc này sẽ giúp dễ dàng xác định vị trí xảy ra lỗi, xử lý sự cố hiệu quả hơn.
### Các loại lưu lượng
| Loại lưu lượng        | Mô tả        |
|:-------------:|:-------------:|
| Network management   |Các nhà thiết kế chia ra một VLAN riêng biệt để quản lý lưu lượng   |
| IP telephony  |2 loại điện thoại IP: thông tin tín hiệu giữa các thiết bị cuối và gói dữ liệu của cuộc gọi thoại. Nhà thiết kế thường cấu hình dữ liệu từ IP của điện thoại trên 1 VLAN được thiết kế cho lưu lượng thoại để có chất lượng cao|
| IP multicast        |    Lưu lượng đa hướng có thể tạo ra một lượng dữ liệu lớn truyền trực tiếp qua mạng. Switch phải được cấu hình để giữ lưu lượng không bị tràn sang thiết vị không yêu cầu nó, và Router phải được cấu hình để đảm bảo lưu lượng đa hướng được chuyển tiếp sang vùng mạng được yêu cầu |
| Normal data     | Dữ liệu mạng bình thường là các file, dịch vụ in, email, trình duyệt, truy cập database,...    |
| Scavenger class       | Lớp Scavenger gồm tất cả các lưu lượng có giao thức hoặc biểu mẫu vượt quá luồng lưu lượng bình thường. Các ứng dụng được chỉ định cho lớp này hướng đến đối tượng giải trí|
### Các loại VLAN
- Xác định theo kiểu lưu lượng hoặc xác định theo chức năng
- Các loại VLAN chủ yếu:
 - **Data VLAN**: Cấu hình để vận chuyển lưu lượng do người dùng tạo ra, đảm bảo rằng lưu lượng thoại mà lưu lượng quản lý được tách riêng ra khỏi lưu lượng dữ liệu
 - **Default VLAN**: Tất cả các port của Switch đều nằm trong VLAN mặc định (VLAN 1). VLAN này không thể đổi tên và xóa. Nên hạn chế chỉ cho VLAN 1 dùng làm đường dẫn cho việc kiểm soát lưu lượng ở L2.
 - **Black hole VLAN**: Phương pháp bảo mật tốt nhất là xác định *Black Hole VLAN* để trở thành 1 VLAN giả khác với các VLAN trong LAN. Các port không dùng của Switch sẽ được thiết lập *Black Hole VLAN* để các thiết bị kết nối trái phép sẽ được ngăn chặn giao tiếp ra ngoài Switch
 - **Native VLAN**: Loại VLAN này đóng vai trò như một định danh chung trên các đầu đối nhau của liên kết trung kế. Phương pháp bảo mật tốt nhất là xác định một VLAN gốc làm VLAN giả riêng biệt
 - **Management VLAN**: Người quản trị mạng định nghĩa VLAN này như là 1 VLAN để truy cập quản trị Switch (Mặc định là VLAN 1)
 - **Voice VLANs**: VLAN thoại kích hoạt port của Switch mang lưu lượng thoại từ thiết bị di động.
 
### Trunking VLANs
- Một VLAN trung kế là 1 Ethernet liên kết điểm-điểm giữa giao diện Ethernet của Switch và giao diện Ethernet của thiết bị mạng khác (như Router và Switch), mang lưu lượng của nhiều VLAN qua 1 số ít đường dẫn. Đường trung kế của VLAN cho phép mở rộng VLAN qua toàn bộ mạng. Đường trunk kế VLAN không thuộc một VLAN cụ thể nào, thay vào đó nó sẽ hoạt động như một liên kết VLAN giữa các Switch

![alt text](https://i.imgur.com/5btrmVk.png)
- Khi một frame đặt trong đường trung kế, thông tin về VLAN nó thuộc về phải được thêm vào frame. Điều này được thực hiện bởi tiêu chuẩn IEEE 802.1Q. Khi Switch nhận frame ở một cổng được cấu hình *access mode* và được định sẵn cho việc kết nối thiết bị từ xa qua đường trung kế, Switch sẽ tách frame ra và chèn thẻ VLAN vào đó, tình toàn lại FCS(frame check sequence) và gửi frame được gắn thẻ qua port trung kế.

![alt text](https://i.imgur.com/C8hDPVU.png)
- Thẻ VLAN gồm:
 - **3 bits of user priority**: Cung cấp khả năng truyền nhanh các frame ở L2 như thoại hoặc lưu lượng
 - **1 bit of Canonical Format Identifier (CFI)**: Kích hoạt các *Token Ring frame* để dễ dàng truyền qua kết nối Ethernet
 - **12 bits of VLAN ID (VID)**: Cung cấp khả năng đánh số VLAN

### Dynamic Trunking Protocol
- DTP là một giao thức của Cisco cho phép thương lượng cả trạng thái của trunk port và trunk encapsulation của trunk port. DTP chỉ quản lý thương lượng đường trunk nếu port của Switch được cấp hình chế độ DTP
- Các chế độ hoạt động:
 - Nếu Switch được cấu hình với câu lệnh **switch mode trunk** thì port của switch sẽ gửi định kỳ bản tin DTP đến port điều khiển, quảng bá rằng nó đang ở chế độ trunk
 - Nếu Switch được cấu hình với câu lệnh **switch mode trunk dynamic auto** thì port của switch nội bộ sẽ quảng bá đến port của switch điều khiển là có thể kết nối trunk nhưng không yêu cầu chuyển sang chế độ trunk. Sau khi thương lượng một DTP, port nội bộ sẽ ở chế độ trunk khi chế độ ở port trunk điều khiển từ xa được cấu hình với trạng thái **on** hoặc **desirable**. Nếu cả 2 port của Switch đặt **auto** thì chúng sẽ không thương lượng để có chế độ trunk. Việc thương lượng này phải ở chế độ *access mode*
 - Nếu Switch được cấu hình với câu lệnh **switch mode trunk dynamic desirable** thì port của switch nội bộ sẽ quảng bá đến port của switch điều khiển là có thể kết nối trunk và hỏi port của Switch điều khiển chuyển sang chế độ trunk. Nếu port nội bộ nhận thấy bên điều khiển cấu hình **on**,**desirable**,**auto** thì sẽ chuyển sang chế độ trunk, còn nếu ở **nonegotiate** thì sẽ không chuyển sang trunk
 - Nếu Switch được cấu hình với câu lệnh **switch nonegotiate** thì port nội bộ sẽ được coi là ở chế độ trunk
 
## Cấu hình và kiểm tra VLAN
- Dùng Cisco Packet Tracer

### VLAN mở rộng
| Kiểu        | Định nghĩa       |
|:-------------:|:-------------:|
| Normal range VLANs  |Dùng trong quy mô nhỏ   |
||ID từ 1-1005|
||ID 1 và từ 1002-1005 được tạo tự động và không thể xóa|
||Cấu hình được lưu trong cơ sở dữ liệu của VLAN gọi là vlan.dat|
| Extended range VLANs  |Phục vụ các doanh nghiệp lớn để mở rộng cơ sở hạ tầng với nhiều khách hàng|
||ID từ 1006-4094|
||Tính năng ít hơn VLAN thông thường|
||Cấu hình được lưu ở file running configuration|

- Swich 2960 của Cisco phải ở chế độ VTP transparent để có thể cấu hình VLAN mở rộng

### Cấu hình và kiểm tra Trunking
- Dùng Ciso Packet Tracer

## Xử lý sự cố trong VLAN
![alt text](https://i.imgur.com/9dJGquk.png)
1. Dùn câu lênh **show vlan** để kiểm tra port có nằm trong VLAN đó không, nếu sai dùng ** switchport access vlan** để sửa
2. Nếu VLAN của port được chỉ định bị xóa, port đó thành port không hoạt động. Dùng **show vlan** và **show interfaces switchport** để kiểm tra lỗi với VLAN bị xóa. Nếu port không hoạt động, nó sẽ không có chức năng đến khi VLAN bị mất đó được tạo lại bằng câu lệnh **vlan** *vlan_id*

| Câu lệnh        | Mô tả |
|:-------------:|:-------------:|
| show vlan  |Hiển thị danh sách mỗi VLAN và các giao diện được gán vào VLAN đó   |
| show vlan brief       |  |
| show vlan id *num*        | Danh sách các port mode access và trunk trong VLAN|
| show interfaces switchport        | Xác định giao diện access của VLAN và voice VLAN, cấu hình và mode đang hoạt động(access/trunk), trạng thái của port(up/down) |
|show interfaces *type number* switchport||
| show mac address-table        | Hiển thị bảng danh sách MAC, gồm các liên kiết với VLAN |
| show interface status        | Tổng hợp các trạng thái của giao diện |

## Xử lý sự cố Trunking
1. Xác định tất cả các interface và các VLAN chúng được truy cập, sau đó gán lại chúng cho các VLAN phù hợp
2. Xác định VLAN có tồn tại hoặc hoạt động ở từng Switch hay không.
3. Kiểm tra danh sách VLAN được cho phép trên Switch ở 2 đầu trunk và đảm bảo chúng có cho phép cùng VLAN.
4. Đảm bảo rằng các liên kết phải dùng đường trunk
### Kiểm tra 2 đầu của đường trunk
![alt text](https://i.imgur.com/1oc64kH.png)
- Nếu cấu hình VLAN không giống nhau giữa 2 đầu, đường trunk sẽ không cho phép lưu lượng đi qua VLAN đó (vlan 10)
- Để thêm vlan 10 vào đường trunk của S2:
 - S2(config)# interface g0/2
 - S2(config-if)# switchport trunk allowed vlan add 10
### Kiểm tra trạng thái hoạt động
- Cấu hình trunk có thể bị nhầm lẫn. Câu lệnh **switchport mode dynamic auto** ở 2 phía của switch sẽ không tự động tạo đường trunk. Thay vào đó 2 đầu đợi thiết bị trên đường dẫn bắt đầu thương lượng