# Khái niệm ACL

## Nội dung chính trong ngày 9

- Một trong những kĩ năng quan trọng của quản trị mạng là kiểm soát truy cập control access list (ACL). ACL dùng để chặn lưu lượng hoặc cho phép một số lưu lượng cụ thể

## Hoạt động của ACL
- Mặc định Router hoạt động cho phép chuyển tiếp tất cả các gói tín. ACL có thể trợ giúp cơ bản về bảo mật. ACL tăng độ trễ của Router. Nếu doanh nghiệp lớn, với Router quản lý nhiều người dùng, người quản trị mạng dùng các biện pháp bảo mật riêng
### Xác dịnh ACL
- ACL là một Router cấu hình một tập lệnh kiểm soát Router đồng ý hoặc từ chối gói tin truyền qua, dựa trên header. Để xác định gói tin được truyền qua hay từ chối, nó kiểm tra ACL theo tuần tự. Khi một ACL khớp nó không kiểm nữa, gói tin được truyền qua hoặc từ chối. deny any là ngầm hiểu ở cuối ACL. Nếu gói không trung với điều kiện nào trong ACL, nó sẽ bị bỏ
### Processing Interface ACLs
- ACL có thể áp dụng trên interface với lưu lượng đến hoặc đi. Tuy nhiên cần ACL khác nhau cho 2 hướng
### List Logic with IP ACLs
- Một ACL là một danh sách câu lệnh được xử lý từ trên xuống dưới. Nếu gói trung với 1 dòng trong ACL, Router áp dụng kịch bản được cấu hình cho gói đó và từ chối tất cả các ACL còn lại
![alt text](https://i.imgur.com/9EGcGoj.png)

## Sử dụng ACL
- Vì ACL có thể lọc lưu lượng nên phải có kế hoạch cụ thể trước khi cấu hình
### Kiểu ACL
- **Standard IPv4 ACLs**: Lọc lưu lượng dựa trên địa chỉ nguồn
- **Extended IPv4 and IPv6 ACLs**: Có thể lọc dựa trên địa chỉ nguồn và đích, giao thức, nguồn và đích của TCP và UDP port
- **Numbered IPv4 ACLs**:Sử dụng số để nhận dạng
- **Named IPv4 and IPv6 ACLs**: Sử dụng tên để nhận dạng
### ACL Identification

| Giao thức           | Phạm vi        |
|:-------------:|:-------------:|
| IP   |      1-99         |
| Extended IP  |  100-199  |
| Standard IP (expanded)       |    1300-1999           |
| Extended IP (expanded)     | 2000–2699     |

- Dùng ACL theo tiên linh hoạt, dễ nhớ hơn số

### Nguyên tắc thiết kế ACL
- Dựa trên điều kiện kiểm tra
- Chỉ 1 ACL được cho phép cho 1 giao thức, 1 hướng, 1 interface
- Tổ chức ACL xử lý từ trên xuống dưới
- Mọi ACL ngầm định deny any ở cuối
- Tạo ACL trước khi đặt vào interface
- Đặt extended ACL gần nguồn lưu lượng mà muốn deny. 