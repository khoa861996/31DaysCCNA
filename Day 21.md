# VTP và cấu hình định tuyến Inter-VLAN

## Nội dung chính trong ngày 21

- Cấu hình thủ công các VLAN qua các Switch trong một mạng doanh nghiệp trở nên khó có thể quản lý. Vì thế Cisco đã phát triển VLAN Trunking Protocol (VTP) để giúp người quản trị mạng có thể triển khai VLAN trong mạng lớn

## Khái niệm VTP
- VTP là một giao thức thông báo ở L2 duy trì tính nhất quán của cấu hình VLAN bằng cách thêm, xóa, đổi tên VLAN qua các mạng
- VTP cho phép người quản trị quản trị VLAN trên một Switch như một VTP server. VTP server phân bổ và đồng bộ thông tin VLAN qua các đường trunk
- Các thành phần của VTP:

| Thành phần           | Ý nghĩa      |
|:-------------:|-------------|
| VTP domain     |Một miền VTP gồm 1 hoặc nhiều Switch kết nối với nhau. |
||Mọi Switch nằm trong miền chia sẻ cấu hình VLAN với nhau sử dụng VTP advertisements|  
||Switch ở các VTP domain khác nhau không trao đổi VTP với nhau|
||Router hoặc Switch L3 xác định ranh giới giữa các miền|
| VTP advertisements  | Mỗi Switch trong VTP domain gửi cấu hình quảng bá định kỳ từ mỗi cổng trunk đến một địa chỉ multicast  |
||Các Switch lân cần nhận những quảng bá này và cập nhật VTP của nó và VLAN nếu cần thiết|
| VTP modes  |Một Switch có thể được cấu hình với 1 trong 3 chế độ: Server, Client, Tranparent|          
| VTP password    |Cấu hình mật khẩu cho Switch trong miền VTP|

- Các chế độ:

| Chế độ           | Ý nghĩa      |
|:-------------:|-------------|
|VTP server|VTP server quảng bá thông tin các VLAN trong miền đến các Switch đã kích hoạt VTP trong cùng một miền|
||VTP server lưu trữ thông tin về VLAN cho toàn bộ miền trong NVRAM|
||VTP server là nơi tạo, xóa, đổi tên VLAN|
|VTP Client|Chức năng giống như VTP server nhưng không thể tạo, xóa, đổi tên VLAN|
||VTP Client lưu thông tin của VLAN cho tất cả các miền chỉ khi Switch được bật|
||Phải cấu hình chế độ Client ở Switch|
|VTP transparent|Tranparent Switch không liên quan đến VTP ngoại trừ để chuyển tiếp quảng bá VTP đến VTP Client và VTP server|

- Chức năng các chế độ:

| Chức năng          | Server      | Client | Tranparent
|:-------------:|:-------------:|:-------------:|:-------------:|
|Gửi thông báo VTP chỉ qua ISL hoặc 802.1Q|Có|Có|Có|
|Cho phép cấu hình CLI|Có|Không|Có|
|Dùng phạm vi bình thường của VLAN (1-1005)|Có|Có|Có|
|Dùng phạm vi mở rộng của VLAN (1-1005)|Không|Không|Có|
|Đồng bộ bằng chính cấu hình cơ sở dữ liệu khi nhận thông tin VTP với số Revison cao hơn|Có|Có|Không|
|Tạo và gửi cập nhật VTP định kỳ mỗi 5 phút|Có|Có|Không|
|Không xử lý cập nhật VTP nhưng cho phép nhận cập nhật VTP ra các trunk khác|Không|Không|Có|

## Cấu hình và kiểm tra VTP
- Sử dụng Cisco Packet Tracer (Đính kèm)

1. Cấu hình chế độ VTP sử dụng **vtp mode**
2. Cấu hình mật khẩu ** vtp password *xxx* **
3. Cấu hình tên của miền **vtp domain**
4. Cấu hình VTP pruning trên VTP server **vtp pruning**
5. Bật VTP ver2 **vtp version 2**
6. Cấu hình trunk giữa các Switch

## Khái niệm định tuyến Inter-VLAN
- Giao tiếp này phải xử lý ở các thiết bị L3 
### Legacy Inter-VLAN Routing
- Định tuyến này yêu cầu nhiều interface vật lý ở cả Router và Switch. Khi sử dung Router để tạo định tuyến inter-VLAN, interface của Router có thể kết nối với các VLAN riêng biệt. Thiết bị ở các VLAN đó gửi lưu lượng thông qua Router đến các VLAN khác

![alt text](https://i.imgur.com/8xsWqzT.png)
- Mỗi interface của S2 nối với R1 được cấu hình với 1 VLAN. Điều này sẽ dẫn đến việc cạn kiệt các interface.
### Router on a Stick
- Các phần mềm của Router hiện nay cho phép cấu hình một interface với nhiều đường trunk bằng cách sử dụng subinterface

![alt text](https://i.imgur.com/eq0KULm.png)
- Interface vật lý G0/0 chia nhỏ thành 2 interface logic. Cấu hình trunk cho cả VLAN 10 và VLAN 30, và mỗi subinterface được cấu hình với các VLAN khác nhau
### Multilayer Switch
- Đây là giải pháp mở rộng cho mạng doanh nghiệp hiện nay sử dụng một multilayer switch để thay thế Router và Switch. Nó sẽ thực hiện được cả việc chuyển mạch dữ liệu cùng 1 VLAN và định tuyến dữ liệu giữa các VLAN

![alt text](https://i.imgur.com/goYdGtk.png)
- Router có giới hạn về số lượng interface để kết nối mạng
- Giới hạn lưu lượng có thể được đáp ứng ở trên liên kết vật lý tại một thời điểm
- Với Multilayer Switch, gói tin có thể chuyển tiếp với một đường trunk

## Cấu hình Router on a Stick
- Khi dùng Router on a Stick, interface vật lý của Router phải được kết nối với đường trunk với Switch liền kề. Trên Router, subinterface được tạo cho mỗi VLAN trong mạng. Mỗi subinterface được định một địa chỉ IP riêng cho subnet hoặc VLAN
- Sử dụng Cisco Packet Tracer (Đính kèm)
1. Kích hoạt interface vật lý sử dụng trunk với Switch kết nối với nó bằng **no shutdown**
2. Nhập chế độ cấu hình subinterface **interface g0/1.10** cho VLAN 10
3. Cấu hình kiểu đóng gói trunk **encapsulation {dot1q|isl} *vlan-number* [native]**
4. Cấu hình địa chỉ IP và subnetmask

## Cấu hình và kiểm tra định tuyến Inter-VLAN dùng Multilayer Switch
- Các Multilayer Switch hỗ trợ 2 kiểu L3:
 - **Switch virtual interface (SVI)**: interface ảo của VLAN dùng để định tuyến inter-VLAN
 - **Routed port**: Gần giống như  interface vật lý ở Router
### Tạo thêm SVI
- Sử dụng Cisco Packet Tracer (Đính kèm)
- Tạo SVI dùng câu lệnh **interface vlan *vlan-id* **
- Switch phải được cấu hình **ip routing** để thực hiện định tuyến ở L3

[VTP mode.zip](https://github.com/khoa861996/31DaysCCNA/files/6172401/VTP.mode.zip)
[Router on a Stick.zip](https://github.com/khoa861996/31DaysCCNA/files/6172403/Router.on.a.Stick.zip)
[Multilayer Switch.zip](https://github.com/khoa861996/31DaysCCNA/files/6172406/Multilayer.Switch.zip)


