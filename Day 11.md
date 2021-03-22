# STP

## Nội dung chính trong ngày 11

- Phương thức hoạt động và cấu hình Spanning Tree Protocol (STP)

## Khái niệm và hoạt động STP
- Điểm chính của một mạng truyền thông được xây dựng tốt là khả năng phục hồi. Có nghĩa là mạng cần có xử lý thiết bị hoặc xử lý liên kết thất bại qua đường dự phòng. STP hỗ trợ phòng chống lặp trong mạng chuyển mạch dự phòng
- Không có STP có thể dẫn đến 1 số lỗi: 
 - **Broadcast storms**: Mỗi Switch truyền tin quảng bá không ngừng
 - **Multiple frame transmission**: Nhiều bản sao của các frame được gửi đến đích
 - **MAC database instability**: Không ổn định bảng địa chỉ MAC

## Thuật toán STP
- STP đặt trên các port nhất định trong trạng thái block vì thế nó không thể nghe, chuyển tiếp hay truyền dữ liệu. STP tạo ra một hệ cây đảm bảo chỉ có 1 đường dẫn tồn tại cho mỗi đoạn mạng. Nếu đoạn nào gặp gián đoạn trong kết nối, STP xây dựng lại một hệ cây mới bằng cách kích hoạt đường dự phòng
- Thuật toán STP chọn interface ở trạng thái đang chuyển tiếp. 
- Switch trao đổi thông tin cấu hình STP mỗi 2 giây, dùng multicast frame gọi là birdge protocol data unit (BPDU). Chặn port nghe để cho BPDU xác định xem phía bên kia có đường dẫn bị "down"

## Hội tụ STP
- Quá trình hội tụ STP là quá trình Switch thu thập xem có gì thay đổi trong topo. Switch xác định xem chúng cần thay đổi port nào chặn, port nào chuyển tiếp
1. Bầu root bridge (Switch với BID thấp nhất). Mọi port ở RB đều ở trạng thái chuyển tiếp
2. Bầu root port. Root port là port mà không phải root bridge có đường dẫn tốt nhất đến root bridge
3. Bầu designed port cho mỗi đoạn
4. root port và designed port chuyển thành trạng thái forwarding

|Đặc điểm của Port|Trạng thái của Port|Mô tả|
|:-------------:|:-------------:|-------------|
|Tất cả các root port của Switch|Forwarding|Root Switch luôn luôn là designed switch trên tất cả các đoạn được kết nối|
|Port không phải root của Swtich|Forwarding|Port này mà Switch có *cost* thấp nhất để đến root switch|
|Designed port của mỗi LAN|Forwarding|Switch chuyển thiếp BPDU có *cost* thấp nhất trên đoạn|
|Các port khác|Blocking|Port không dùng để chuyển tiếp frame|

## Các loại STP
- **STP**: Nguyên gốc của STP, cung cấp một topo không lặp trong một mạng với các đường dự phòng
- **PVST+**: Per-VLAN Spanning Tree Plus là một nâng cấp của Cisco cung cấp khả năng cấu hình cho mỗi VLAN trong mạng
- **RSTP**: Rapid STP (802.1w), là sự phát triển của STP cung cấp khả năng hội tụ nhanh hơn
- **Rapid PVST+**: Nâng cấp RSTP sử dụng PVST+
- **Multiple Spanning Tree Protocol**: MSTP đặt nhiều VLAN vào cùng một hệ cây

|Giao thức|Tiêu chuẩn|Tài nguyên cần|Hội tụ|Tính toán hệ cây|
|:-------------:|:-------------:|-------------|-------------|-------------|
|STP|802.1D|Thấp|Chậm|Tất cả VLAN|
|PVST+|Cisco|Cao|Chậm|Mỗi VLAN|
|RSTP|802.1w|Trung bình|Nhanh|Tất cả VLAN|
|Rapid PVST+|Cisco|Rất cao|Nhanh|Mỗi VLAN|
|MSTP|802.1s, Cisco|Trung bình hoặc cao|Nhanh|Mọi trường hợp|

## PVST
- PVST+ là cài đặt mặc định của tất cả các thiết bị Cisco. Trong môi trường PVST+, có thể tinh chỉnh các tham số spanning-tree để nửa số VLAN chuyển tiếp trên mỗi đường trunk. Cấu hình 1 Switch là root bridge cho nửa số VLAN và 1 switch là root bridge cho nửa số VLAN còn lại

![alt text](https://i.imgur.com/OoMGtiw.png)
- Từ S2, Một port chuyển tiếp hay chặn phụ thuộc vào VLAN. Sau khi hội tụ, port f0/2 sẽ chuyển tiếp frame từ VLAN 10 và chặn frame từ VLAN 20. Port f0/3 sẽ chuyển tiếp frame từ VLAN 20 và chặn frame từ VLAN 10
- Đặc trưng của PVST+
 - Cấu hình PVST mỗi VLAN cho phép các đường dẫn dự phòng được tận dụng tối đang
 - Mỗi spanning-tree thêm vào cho một VLAN sẽ tốn nhiều CPU
 
### Port States
- Spanning-tree được xác định ngay khi Switch bật lên

|Cho phép hoạt động|Blocking|Listening|Learning|Forwarding|Disabled|
|:-------------:|:-------------:|-------------|-------------|-------------|-------------|
|Có thể nhận và xử lý BPDU|Có|Có|Có|Có|Không|
|Có thể chuyển tiếp dữ liệu nhận trên interface|Không|Không|Không|Có|Không|
|Có thể chuyển tiếp dữ liệu chuyển từ interface khác|Không|Không|Không|Có|Không|
|Có thể học địa chỉ MAC|Không|Không|Có|Có|Không|

## Rapid PVST+
- Phiên bản của RSTP chạy cho mỗi VLAN
- Với RSTP, IEEE cải thiện khả năng hội tụ của STP từ 50 đến ít hơn 10 giây
### RSTP Interface Behavior
- Thay đổi chính của RSTP có thể thấy khi có thay đổi trong mạng. RSTP hoạt động khác nhau trên một số interface dựa trên kết nối với interface:
 - **Edge-type behavior and PortFast**: RSTP cải thiện hội tụ bằng cách ngay lập tức đặt port ở trạng thái forwarding 
 - **Link-type shared**: RSTP hoạt động giống STP trên chia sẻ link-type
 - **Link-type point-to-point**: RSTP cải thiện hội tụ đường dẫn song công giữa các Switch.
### RSTP Port Roles
![alt text](https://i.imgur.com/cfX0mK2.png)

|RSTP Role|STP Role|Định nghĩa|
|:-------------:|:-------------:|-------------|
|Root port|Root port|Một port ở switch không phải root nghe BPDU tốt nhất trong tất cả các BPDU đã nhận|
|Designated port|Designated port|Tất cả các port của switch trên tất cả các switch gắn trên cùng 1 đoạn/ miền xung đột, port quảng bá BPDU tốt nhất|
|Alternate port||Port trên Switch mà nhận BPDU ở mức dưới mức tối ưu|
|Backup port||Port không phải designed trên switch mà gắn vào cùng một đoạn như một port khác trên cùng một switch|
|Disabled||Port bị tắt hoặc không phù hợp để hoạt động|

### Edge Ports
![alt text](https://i.imgur.com/jpZ471M.png)
- RSTP dùng khái niệm edge port tương ứng với PortFast của PVST+. Edge port chuyển trạng thái sang forwarding

## Cấu hình và kiểm tra STP
- Sử dụng Cisco Packet Tracer (Đính kèm)
### Cấu hình PortFast và BPDU Guard
- Để cấu hình PortFast có hiệu lực, BPDU không bao giờ được nhận vì điều đó chỉ ra răng một bridge hoặc switch khác đang kết nối với port, gây lặp. Khi được bật BPDU guard đưa port vào trạng thái err-disabled khi nhận được một BPDU. Tính năng BPDU guard cung cấp khả năng bảo mật với những cấu hình không phù hợp
### Cấu hình Rapid PVST+
- PVST+ là mặc định của Switch Cisco, để đổi thành Rapid PVST+, dùng cầu lệnh **spanning-tree mode rapid-pvst**

## Switch Stacking
- Xếp chồng nhiều lớp Switch giúp người quản trị mạng quản lý nhiều Switch như 1 Switch
- Điều này cải thiện hiệu quả quản lý và giao thức theo các cách:
 - Stack có một IP quản lý
 - Một file cấu hình bao gồm tất cả các interface
 - STP,CDP,VTP chạy trên 1 Swtich
 - Port của Switch xuất hiện như trên cùng một Switch
 - Một bảng địa chỉ MAC
 
![alt text](https://i.imgur.com/I75L22Y.png)
- Switch Stacking giảm thiểu đường kính của STP

[STP.zip](https://github.com/khoa861996/31DaysCCNA/files/6183107/STP.zip)

