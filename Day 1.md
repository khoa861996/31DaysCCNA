# Công cụ và phương pháp khắc phục sự cố

## Nội dung chính trong ngày 1

- Xử lý lỗi

## Troubleshooting Documentation
- Để giám sát xử lý lỗi một mạng, người quản trị phải có tài liệu về mạng đang dùng:
 - File cấu hình
 - Thiết bị vật lý và logic
 - Hiệu suất cơ bản
 
## Configuration Files
- Kiểu thiết bị và thiết kế
- IOS image
- Tên của thiết bị mạng
- Vị trí của thiết bị
- Các kiểu module và vị trí module và nơi đặt chúng
- Địa chỉ lớp Data-link
- Địa chỉ lớp Network

## Troubleshooting Methods

![alt text](https://i.imgur.com/IirIWXM.png)

## Troubleshooting at Each Layer
### Physical Layer
- Vì lớp trên của mô hình OSI phụ thuộc vào lớp Physical để có chức năng, người quản trị phải biết tách riêng và xử lý lỗi lớp này

![alt text](https://i.imgur.com/9wrs2sY.png)

### Data Link Layer
- Lỗi L2 dẫn đến các triệu chứng cụ thể khi được nhận biết, giúp xác định một cách nhanh chóng 

![alt text](https://i.imgur.com/9SRnNbA.png)

### Network Layer

- Lớp Network lỗi gồm các lỗi liên quan đến giao thức L3, gồm giao thức định tuyến

![alt text](https://i.imgur.com/MivMA5e.png)

### Transport Layer
- Lỗi mạng có thể nảy sinh từ lớp transport trên Router

![alt text](https://i.imgur.com/3q2iLAX.png)

- Lỗi phổ biến nhất là cấu hình ACL không đúng 

![alt text](https://i.imgur.com/MYteV0B.png)

- NAT có thể gây lỗi, gồm cấu hình sai NAT inside, outside, hoặc cấu hình sai ACL

![alt text](https://i.imgur.com/bvkNKVC.png)

### Application Layer
- Lỗi lớp Application ngăn chặn dịch vụ được cung cấp đến ứng dụng. Lỗi lớp này có thể dẫn đến không tiếp cận được tài nguyên khi lớp Physical,Data-link,Network,Transport có chức năng. Nó có thể có kết nối mạng nhưng ứng dụng không cung cấp dữ liệu

![alt text](https://i.imgur.com/75lZIOX.png)

### Bottom-Up Method and the Layers
1. Kiểm tra kết nối vật lý
2. Kiểm tra chế độ song công
3. Kiểm tra lớp Data-link và Network
4. Kiểm tra default gateway
5. chắc chắn thiết vị xác định đúng đường từ nguồn đến đích
6. Xác định lớp transport hoạt động.
7. Xác định không có ACL chặn lưu lượng
8. Đảm bảo cài đặt DNS đúng

## Troubleshooting with IP Service Level Agreement
- IP service level agreement (SLA) là tính năng trên Router Cisco giúp tính toán hiệu năng
- Kỹ sư mạng có thể dùng e IP SLA ICMP Echo để kiểm tra thiết bị