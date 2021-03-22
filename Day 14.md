# Fine-Tuning và xử lý lỗi EIGRP

## Nội dung chính trong ngày 14

- Tinh chỉnh, xử lý lỗi EIGRP. Tự động tổng hợp mạng, thay đổi *cost*, thiết lập default route, thay đổi bộ đếm

## Tùy chỉnh cấu hình EIGRP cho IPv4
### Automatic Summarization
- Mặc định tính năng này tắt. Tổng hợp định tuyến cho phép Router tập hợp lại các mạng và quảng bá chúng như một nhóm lớn dùng một định tuyến tổng hợp

![alt text](https://i.imgur.com/nnlH8im.png)

- R1 và R2 được cấu hình dùng IPv4. R1 có 3 mạng con trong bảng định tuyến: 172.16.1.0/24, 172.16.2.0/24, 172.16.3.0/24. Ở mạng dạng Classful, các mạng này là một phần của mạng lớn loại B: 172.16.0.0/16
- Câu lệnh: **auto-summary** (nên tắt)
### Propagating an IPv4 Default Route

**R2(config)# ip route 0.0.0.0 0.0.0.0 Serial0/1/0**
**R2(config)# router eigrp 1**

**R2(config-router)# redistribute static**

**redistribute static**: thông báo cho EIGRP thêm định tuyến tĩnh này vào bản cập nhật đến các Router khác


![alt text](https://i.imgur.com/2hAkyHv.png)
- D*EX:
 - **D**: Tuyến được học qua EIGRP 
 - *****: Tuyến đang là default route  
 - **EX**: Đây là tuyến bên ngoài với AD=170

### Tùy chỉnh EIGRP metric

**Router(config-if)# bandwidth kilobits**

### Tùy chỉnh khoảng "Hello" và "HoldTimes"
- 2 tham số này được cấu hình trên mỗi interface và không cần giống nhau giữa các router dùng EIGRP để thiết lập kết nối

**Router(config-if)# ip hello-interval eigrp as-number seconds**

- Nếu thay đổi "hello" thì phải thay đổi hold-time với giá trị bằng hoặc lớn hơn "hello"

**Router(config-if)# ip hold-time eigrp as-number seconds**

## Tùy chỉnh cấu hình EIGRP cho IPv6
- Tương tự với IPv4
### Modifying Bandwidth Utilization
- Mặc định EIGRP dùng đến 50% băng thông của một interface
**Router(config-if)# ipv6 bandwidth-percent eigrp as-number percent**

## Discontiguous Networks
- EIGRP có một vấn đề duy nhất vì có thể cấu hình EIGRP để tự động hợp mạng ở các đoạn ngăn cách. Automatic summarization sẽ không xảy ra vấn đề gì, miễn là mạng được tổng hợp liền kề nhau

[EIGRP.zip](https://github.com/khoa861996/31DaysCCNA/files/6183121/EIGRP.zip)
