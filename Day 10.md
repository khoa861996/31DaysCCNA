# EtherChannel và HSRP

## Nội dung chính trong ngày 11

- Công nghệ EtherChannel giúp gộp nhiều interface vật lý thành một kênh logic để tăng băng thông trên đường dẫn point-to-point. EtherChannel cung cấp một cách để ngăn chặn hội tụ của STP khi chỉ xảy ra sự cố ở một port hoặc cáp
- Hầu hết các thiết bị đầu cuối không lưu định tuyến đến mạng truy nhập từ xa. Thay vào đó, chúng được cấu hình một default gateway để xử lý định tuyến cho chúng, Để đảm bảo một thiết bị vẫn truy cập được vào mạng, phải triển khai các loại default gateway dự phòng.

## EtherChannel
- Đây là công nghệ được triển khai bởi Cisco, có thể gộp lên đến 8 đường dẫn có tốc độ bằng nhau giữa 2 Switch
![alt text](https://i.imgur.com/Lr4oD2v.png)

- Giao thức STP thấy bó các liên kết như một interface. Nếu chỉ có 1 liên kết "up", STP không nhất thiết phải hội tụ. Điều này giúp sử dụng băng thông tốt hơn trong khi giảm thời gian hội tụ STP. Nếu không dùng EtherChannel hoặc chỉnh sửa cấu hình STP, STP sẽ chỉ để lại 1 liên kết và chặn tất cả
### Lợi ích của EtherChannel
- Khi cấu hình EtherChannel kết quả của interface ảo gọi là *port channel*:
 - Hầu hết mọi cấu hình có thể làm trên EtherChannel interface thay vì mỗi port riêng biệt
 - EtherChannel dựa vào port có sẵn của Switch để tăng cường băng thông
 - Cân bằng tải là có thể giữa các liên kết cùng một EtherChannel
 - EtherChannel tạo một tổng hợp mà STP có thể nhận diện được như một liên kết logic
 - EtherChannel cung cấp dự phòng. Sự mất mát của một liên kết vật lý không tạo ra thay đổi trong topo

### Hạn chế
- Kiểu Interface, như FastEthernet và GigabitEthernet không thể trộn lẫn vào nhau ở cùng EtherChannel
- Mỗi EtherChannel có thể có 8 port Ethernet được cấu hình tương thích
- Phần mềm Cisco IOS hiện tại chỉ hỗ trợ đến 6 EtherChannel
- Một số Server hỗ trợ chuyển đổi EtherChannel để tăng cường băng thông, tuy nhiên Server cần ít nhất 2 EtherChannel để cung cấp khả năng dự phòng vì nó có thể gửi lưu lượng đến duy nhất một Switch qua EtherChannel
- Cấu hình EtherChannel phải nhất quán ở 2 Switch. Cấu hình trunk,... phải giống nhau. Các port phải ở L2
- Tất cả các port phải ở L2 hoặc L3

## Giao thức EtherChannel
- Có thể cấu hình EtherChannel tĩnh hoặc vô điều kiện, tuy nhiên cần dùng 2 giao thức thương lượng: Port Aggregation Protocol (PAgP) và Link Aggregation Control Protocol (LACP). 2 giao thức này đảm bảo 2 bên của liên kết có cấu hình tương thích (speed,duplex,VLAN)
### Port Aggregation Protocol
- PAgP là giao thức độc quyền của Cisco tự động tạo liên kết EtherChannel. PAgP kiểm tra tính nhất quán của cấu hình và quản lý liên kết được thêm vào và hỏng giữa 2 Switch. Nó đảm bảo khi EtherChannel được tạo, tất cả các port có cùng kiểu cấu hình.
 - **On**: Chế độ này đẩy interface sang dạng channel mà không cần PAgP
 - **Desirable**: Interface bắt đầu thương lượng với interface khác bằng cách gửi các gói PAgP
 - **Auto**: Interface phản hồi gói PAgP nhận được nhưng không bắt đầu thương lượng PAgP
### Link Aggregation Control Protocol
- LACP cho phép Switch thương lượng tự động bằng cách gửi gói LACP
 - **On**: hế độ này đẩy interface sang dạng channel mà không cần LACP
 - **Active**: Interface bắt đầu thương lượng với interface khác bằng cách gửi các gói LACP
 - **Passive**: Interface phản hồi gói LACP nhận được nhưng không bắt đầu thương lượng LACP
 
## Cấu hình EtherChannel
1. Xác định các interface muốn gộp vào một liên kết **interface range interfaces**
2. Tạo port channel **channel-group identifier mode mode**. identifier từ 1->6
3. Vào cấu chế độ cấu hình interface cho port channel mới với câu lệnh **interface port-channel identifier**. Số identifier phải trùng với channel-group
4. Cấu hình trunk và VLAN

## Xử lý lỗi EtherChannel
- Tất cả các Interface trong EtherChannel phải có chung cấu hình
 - Chỉ định tất cả các port trong EtherChannel phải có cùng VLAN, hoặc cấu hình chúng là trunk. Port có native VLAN khác nhau không thể tạo thành EtherChannel
 - Khi cấu hình trunk trên EtherChannel, kiểm tra chế độ trunk trên EtherChannel. Không khuyến cáo cấu hình chế độ trunk trên các port riêng lẻ. Tuy nhiên, nên đã cấu hình thì kiểm tra xem cấu hình trunk có giống nhau ở tất cả các interface không
 - Lựa chọn thương lượng tự động cho PAgP và LACP phải được cấu hình tương thích trên 2 đầu của EtherChannel
- Cấu hình lỗi với **channel-group**:
 - Cấu hình **on** trên 1 switch và **desỉable**,**auto**, **passive**, **active**, hoặc **passive** trên các switch khác.
 - Cấu hình **auto** trên cả 2 switch dùng PAgP, nhưng cả 2 chờ nhau thương lượng

## First-Hop Redundancy Concepts
- FHRP giúp cài nhiều Router trong một mạng con để thu thập hoạt động như một Router mặc định. Những Router này chia sẻ một địa chỉ IP ảo
![alt text](https://i.imgur.com/t08iT9G.png)

- Interface g0/0 trên R1 và R2 được cấu hình với 2 địa chỉ IP, tuy nhiên cả 2 Router có địa chỉ IP ảo. Địa chỉ IP ảo này là default gateway được cấu hình trên thiết bị cuối. Một giao thức dự phòng cung cấp cơ chế xác định Router nào nên active để chuyển tiếp dữ liệu. Nó cũng xác định Router chờ để chuyển tiếp vài trò. Khả năng phục hồi của mạng sau sự cố của một thiết bị hoạt động như default gateway được gọi là first-hop redundancy

## FHRPs
- 3 lựa chọn của FHRP
 - **Hot Standby Router Protocol (HSRP)**: Công nghệ độc quyền của Cisco cho phép chuyển đổi một first-hop của thiết bị dùng IPv4. Chức năng của HSRP chờ Router giám sát trạng thái của nhóm HSRP và nhanh chóng chuyển tiếp gói tin nếu Router đang active lỗi
 - **Virtual Router Redundancy Protocol (VRRP)**: Tự động gán trách nhiệm cho một hoặc nhiều Router VRRP ảo trong LAN dùng IPv4
 - **Gateway Load Balancing Protocol (GLBP)**: Công nghệ độc quyền của Cisco bảo vệ dữ liệu từ Router hỏng, trong khi đó còn cho phép cân bằng tải

## Hoạt động của HSRP
- HSRP sử dụng mô hình hoạt động hoặc chờ trong đó một Router hoạt động đóng vai trò là default gateway của thiết bị trên subnet. Một hoặc nhiều Router ở trên cùng một subnet sau đó sẽ ở chế độ chờ. Router hoạt động có một IP ảo trùng với MAC ảo. Địa chỉ ảo này là một phần cấu hình của HSRP và thuộc cùng subnet như địa chỉ interface vật lý nhưng khác địa chỉ IP. Router tự động tạo MAC ảo. Tất cả các Router dùng HSRP biết về các địa chỉ ảo này, nhưng chỉ có Router active dùng những địa chỉ này tại bất kì thời điểm nào
- Nếu VLAN được cấu hình, Router có thể chia sẻ tải
### HSRP Priority and Preemption
- Mặc định, Router với IPv4 lớn nhất được bầu làm HSRP
- Để bầu HSRP mới, quyền ưu tiên phhải được bật bằng câu lệnh **stanby preempt**
## Cấu hình và kiểm tra HSRP
- Sử dụng Cisco Packet Tracer (Đính kèm)

[EtherChannel.zip](https://github.com/khoa861996/31DaysCCNA/files/6190522/EtherChannel.zip)
[HSRP.zip](https://github.com/khoa861996/31DaysCCNA/files/6190525/HSRP.zip)
[HSRP Load Balancing.zip](https://github.com/khoa861996/31DaysCCNA/files/6190527/HSRP.Load.Balancing.zip)



