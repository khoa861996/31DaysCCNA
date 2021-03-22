# CDP và LLDP

## Nội dung chính trong ngày 13

- Cisco Discovery Protocol (CDP) là tính năng độc quyền của Cisco được dùng để tập hợp thông tin của các thiết bị ở cùng data-link. Thiết bị Cisco còn hỗ trợ Link Layer Discovery Protocol (LLDP), là giao thức tìm kiếm lân cận tương tự CDP

## Tổng quan CDP
![alt text](https://i.imgur.com/iNv3IGI.png)

- CDP gửi quảng bá đến thiết bị trực tiếp kết nối
- CDP chạy trên mọi thiết bị của Cisco. Nó tập hợp địa chỉ giao thức của các thiết bị lân cận và tìm ra nền tảng của thiết bị đó. CDP chạy ở lớp data-link
- CDP có thể hỗ trợ tìm và xử lý lỗi với các thông tin:
 - **Device ID**: tên của thiết bị lân cận
 - **Addresses**: Địa chỉ IPv4 và IPv6 được dùng bởi thiết bị
 - **Port ID**: Tên của port local và remote 
 - **Capabilities**: Thiết bị là Router hay Switch hoặc chức năng khác
 - **Version**: Phiên bản CDP đang chạy trên thiết bị
 - **Platform**: Nền tảng phần cứng của thiết bị

## Cấu hình CDP
- CDP mặc định chạy trên tất cả các interface

![alt text](https://i.imgur.com/1AvfBSd.png)
- Interface không cần cấu hình địa chỉ L3 để gửi quảng bá CDP, chỉ cần câu lệnh **no shut**
- Để tắt CDP, dùng câu lệnh **no cdp run**
- CDP có thể được tắt ở từng interface. Đảm bảo an toàn tránh các interface kết nối với mạng untrusted.

**Router(config)# interface s0/0/0**

**Router(config-if)# no cdp enable**

- Cấu hình thời gian để quảng bá CDP và hold-time

**Router(config)# cdp timer *seconds*** (5-254) (default 60)

**Router(config)# cdp holdtime *seconds*** (10-255) (default 180)

## Kiểm tra CDP
![alt text](https://i.imgur.com/I7vHeH7.png)

## Tổng quan LLDP 
- LLDP hoạt động với Router,Switch,Wirless LAN AP. Cũng như CDP, LLDP là một giao thức dùng cho thiết bị để quảng bá thông tin của nó đến các thiết bị khác trong mạng

## Cấu hình LLDP
**Router(config)# lldp run**
- Khi LLDP được bật, nó sẽ hoạt động ở tất cả interface. Để tắt LLDP ở một interface dùng câu lệnh **no lldp transmit** và **no lldp receive**
- Cấu hình thời gian để quảng bá LLDP và hold-time cũng tương tự CDP tuy nhiên default lần lượt là 30 và 120
