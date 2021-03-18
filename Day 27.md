# IPv4

## Nội dung chính trong ngày 27

- Cấu trúc địa chỉ IPv4, các loại, IPv4 công khai và riêng tư, chia mạng con IPv4

## Địa chỉ IPv4
### Header Format

![alt text](https://i.imgur.com/efOr5Oc.png)
- Mỗi gói IP mang theo một header bao gồm IP nguồn và đích
- Một địa chỉ IP chia làm 2 phần:
 - Các bit bậc cao hoặc ngoài cùng bên trái xác định thành phần địa chỉ mạng.
 - Các bit bạc thấp hoặc ngoài cùng bên phải xác định các host.

### Các loại địa chỉ IPv4

![alt text](https://i.imgur.com/h04cGFv.png)
- IPv4 được thiết kế với cấu trúc: A,B,C,D,E. Loại D dùng cho multicast, loại E dùng để nghiên cứu. Loại A,B,C định ra các host trong mạng. 3 loại này sẽ chia ra làm phần Network và phần host
- Thiết bị hoạt động ở L3 có thể định ra loại của IP dựa trên các bit đầu của octet đầu tiên

| Loại | Khoảng số trong octet đầu | Các bit đầu của octet |Phần Network(N) và Host(H) |Subnetmask mặc định|Số lượng Network và Host có thể tồn tại|
|:-------------:|:-------------:|-------------|-------------|------------|------------|
|A|1-127|00000000–01111111|N.H.H.H|255.0.0.0|2^7 Network, 2^24-2 Host|
|B|128-191|10000000–10111111|N.N.H.H|255.255.0.0|2^14 Network, 2^16-2 Host|
|C|192-223|11000000–11011111|N.N.N.H|255.255.255.0|2^21 Network, 2^8-2 Host|
|D|224-239|11100000–11101111|Không dùng để chia host|
|E|240-255|11110000–11111111|Không dùng để chia host|

- 2 Host bị trừ đi ở mỗi mạng là địa chỉ mạng và địa chỉ quảng bá của mạng con đó.

## IP công khai và riêng tư
 - Class A: 10.0.0.0/8 (10.0.0.0–10.255.255.255)
 - Class B: 172.16.0.0/12 (172.16.0.0–172.31.255.255)
 - Class C: 192.168.0.0/16 (192.168.0.0–192.168.255.255)
- Việc chia địa chỉ IP như trên giúp các doanh nghiệp tận dụng được toàn bộ địa chỉ của loại A (10.0.0.0/8). Chuyển tiếp lưu lượng ra Internet cần biên dịch địa chỉ công khai bằng Network Address Translation (NAT). Bằng cách sử dụng nhiều địa chỉ riêng tư, một công ty chỉ cần một địa chỉ IP công khai

## Chia IP
1. Xác định có bao nhiêu bit mượn dùng dựa trên yêu cầu về số Host
2. Xác định Subnetmask mới
3. Xác định số bước nhảy
4. Liệt kê các mạng con.

- VD: 192.168.1.0/24, 30 host mỗi subnet
 - Có 8 bit cho host
 - Host Bits = Bit Borrowed + Bits Left
 - 30 host phải để lại 5 bit: 2^5-2 = 30
 - Vậy còn 3 bit phần host, mượn 3 bit này để tạo ra nhiều mạng con nhất có thể để tạo thêm 8 mạng con: 2^3=8
 - Subnetmask mới là 24 bit ban đầu cộng 3 bit mượn = 27 => /27
 => Vậy có 8 mạng con với mỗi mạng con gồm 30 host
- VD 1: 172.16.0.0/16, 80 host mỗi subnet
 - Có 16 bit cho host
 - 80 host phải để lại 7 bit: 2^7-2 = 126 host
 - Còn 9 bit mượn: 2^9 = 512 subnet
 - Subnetmask mới: 16+9 = 25 (255.255.255.254)
 => Vậy có 512 mạng con mỗi mạng con gồm 126 host
- VD 2: 172.16.0.0/16, ít nhất 80 subnet
 - Mượn 7 bit để làm subnet: 2^7 = 128
 - Còn 9 bit host: 2^9-2= 510
 - Subnetmask mới: 16+7=23 (255.255.254.0)
- VD 3: 172.16.10.0/23, 60 host 
 - 9 bit host
 - 60 host phải để lại 6 bit: 2^6-2 = 62
 - Còn 3 bit mượn => số subnet = 2^3 = 8
 - Subnetmask mới: 23+3=26 (255.255.255.192)

## VLSM
- VLSM giúp tùy chỉnh mạng con phù hợp
![alt text](https://i.imgur.com/x7ltbfM.png)
- 172.30.4.0/22
- LAN 1: 60 Host
- LAN 2: 10 Host
- LAN 3: 250 Host
- LAN 4: 100 Host

- Có 10 bit host

- LAN 3: 250 host: để lại 8 bit => 2^8-2 = 254 host
- Còn 2 bit cho subnet => 2^2=4 subnet
- Subnetmask mới: 22+2 = 24 (255.255.255.0)
- Bước nhảy = 256 - 255 = 1
=> Chia làm 4 subnet mới:
- 172.30.4.0/24
- 172.30.5.0/24
- 172.30.6.0/24
- 172.30.7.0/24
- Cho 172.30.4.0/24 là LAN 3

- Xét 172.30.5.0/24 chia cho LAN 4: 100 host: để lại 7 bit => 2^7-2=124 host
- Còn 1 bit cho subnet => 2^1=2 subnet
- Subnetmask mới 24+1 =25 (255.255.255.128)
- Bước nhảy = 256 - 128 = 128
=> Chia làm 2 subnet mới:
- 172.30.5.0/25
- 172.30.5.128/25
- Cho 172.30.5.0/25 là LAN 4

- Xét 172.30.5.128/25 chia cho LAN 1: 60 host: để lại 6 bit = >2^6-2=62 host
- Còn 1 bit cho subnet => 2^1=2 subnet
- Subnetmask mới 25+1 =26 (255.255.255.192)
- Bước nhảy = 256 - 192 = 64
=> Chia làm 2 subnet mới:
- 172.30.5.128/26
- 172.30.5.192/26
- Cho 172.30.5.128/26 là LAN 1

- Xét 172.30.5.192/26 chia cho LAN 2: 10 host: để lại 4 bit => 2^4-2=14 host
- Còn 2 bit cho subnet => 2^2=4 subnet
- Subnetmask mới: 26+2 = 28 (255.255.255.240)
- Bước nhảy = 256 - 240 = 16
=> Chia làm 4 subnet mới:
- 172.30.5.192/28
- 172.30.5.208/28
- 172.30.5.224/28
- 172.30.5.240/28
- Cho 172.30.5.192/28 là LAN 2
 
- Xét 172.30.5.208/28 chia cho đường WAN ( cần 2 host): để lại 2 bit 2^2-2=2 host
- Còn 2 bit cho subnet => 2^2=4 subnet 
- Subnetmask mới 28+2 =30 (255.255.255.252)
- Bước nhảy = 256 - 252 = 4
=> Chia làm 4 subnet mới:
- 172.30.5.208/30
- 172.30.5.212/30
- 172.30.5.216/30
- 172.30.5.220/30
- Cho 172.30.5.208/30 là WAN

=> Vậy ta còn thừa 3 subnet /30, 2 subnet /28, 2 subnet /24

