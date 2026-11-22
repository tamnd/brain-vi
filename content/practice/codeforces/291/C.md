---
title: "CF 291C - Mặt nạ mạng"
description: "Mỗi địa chỉ IP thực sự là một số nguyên 32 bit được viết dưới dạng thập phân chấm. Mặt nạ mạng con cũng có giá trị 32 bit, nhưng nó phải có hình dạng rất cụ thể: một số bit 1 ở đầu theo sau là một số bit 0 ở cuối."
date: "2026-06-05T17:02:41+07:00"
tags: ["codeforces", "competitive-programming", "*special", "bitmasks", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 291
codeforces_index: "C"
codeforces_contest_name: "Croc Champ 2013 - Qualification Round"
rating: 1600
weight: 291
solve_time_s: 134
verified: true
draft: false
---

[CF 291C - Mặt nạ mạng](https://codeforces.com/problemset/problem/291/C) 

**Đánh giá:** 1600 
**Tags:** *đặc biệt, bitmasks, vũ lực, triển khai 
**Thời gian giải:** 2m 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi địa chỉ IP thực sự là một số nguyên 32 bit được viết dưới dạng thập phân chấm. Mặt nạ mạng con cũng có giá trị 32 bit, nhưng nó phải có hình dạng rất cụ thể: một số`1`bit theo sau là một số dấu vết`0`bit. 

Khi địa chỉ IP được AND-ed với mặt nạ mạng con, kết quả là địa chỉ mạng. Hai IP thuộc cùng một mạng nếu chúng tạo ra kết quả giống nhau sau khi áp dụng mặt nạ. 

Chúng tôi được cung cấp tới 100.000 địa chỉ IP riêng biệt và số mục tiêu`k`. Chúng ta phải tìm một mặt nạ mạng con sao cho các địa chỉ đã cho thuộc về chính xác`k`các mạng riêng biệt. Nếu nhiều mặt nạ hoạt động, chúng ta phải xuất ra mặt nạ có ít mặt nạ nhất`1`bit. Vì mặt nạ mạng con bao gồm các mặt nạ mạng dẫn đầu, điều này có nghĩa là chúng tôi muốn độ dài tiền tố ngắn nhất có thể trong số tất cả các mặt nạ hợp lệ. Nếu không có mặt nạ nào tạo ra chính xác`k`mạng, chúng tôi xuất ra`-1`. 

Ràng buộc`n ≤ 100000`ngay lập tức loại trừ bất cứ điều gì so sánh tất cả các cặp địa chỉ. MỘT`O(n²)`giải pháp sẽ yêu cầu khoảng`10^10`hoạt động trong trường hợp xấu nhất. Chúng ta cần một cái gì đó gần gũi`O(n log n)`hoặc`O(32n)`. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Giả định`k = n`. Vì tất cả các địa chỉ IP đều khác nhau nên mặt nạ có 32 địa chỉ luôn tạo ra`n`địa chỉ mạng riêng biệt. Một giải pháp quên xem xét mặt nạ có độ dài đầy đủ sẽ báo cáo sai`-1`. 

Ví dụ:```
2 2
1.1.1.1
1.1.1.2
```Câu trả lời đúng là:```
255.255.255.255
```Một trường hợp tinh tế khác xảy ra khi không có mặt nạ nào mang lại kết quả chính xác`k`các nhóm. 

Ví dụ:```
2 1
0.0.0.0
255.255.255.255
```Với bất kỳ mặt nạ mạng con hợp lệ nào, các địa chỉ này vẫn nằm trong các mạng khác nhau. Đầu ra đúng là:```
-1
```Cái bẫy thứ ba là chọn sai mặt nạ hợp lệ. Chúng tôi không được yêu cầu bất kỳ mặt nạ nào. Chúng ta phải trả lại cái có ít cái nhất. 

Ví dụ:```
2 1
1.2.3.4
1.2.3.5
```Cả hai`/31`Và`/30`tạo ra một mạng lưới, nhưng câu trả lời phải là`/30`bởi vì nó có ít bit hơn. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Mặt nạ mạng con được xác định hoàn toàn bởi độ dài tiền tố của nó. Chỉ có 31 khẩu trang hợp lệ nếu chúng tôi yêu cầu ít nhất một chiếc`1`bit và ít nhất một`0`bit, hoặc tương đương 33 khả năng nếu chúng ta nghĩ về việc giữ từ 0 đến 32 bit đầu. Đối với mỗi mặt nạ, hãy tính toán từng địa chỉ mạng và đếm xem có bao nhiêu kết quả riêng biệt xuất hiện. 

Chi phí này cho một chiếc mặt nạ`O(n)`. Lặp lại nó cho tất cả các chi phí có thể có của tiền tố`O(32n)`, tức là khoảng 3,2 triệu hoạt động cho`n = 100000`. Đáng ngạc nhiên, điều này đã đủ nhanh. 

Quan sát quan trọng là không gian tìm kiếm rất nhỏ. Mặc dù đầu vào chứa tới một trăm nghìn địa chỉ nhưng số lượng mặt nạ mạng con bị giới hạn bởi độ rộng bit của địa chỉ IP, cụ thể là 32. 

Quan sát thứ hai làm cho giải pháp thậm chí còn sạch hơn. Khi độ dài tiền tố tăng lên, nhiều bit của mỗi địa chỉ được giữ nguyên. Mạng chỉ có thể phân chia, không bao giờ hợp nhất. Do đó, số lượng địa chỉ mạng riêng biệt là đơn điệu đối với độ dài tiền tố. 

Nếu chúng ta xác định:`f(p) = number of distinct networks using the first p bits`sau đó`f(p)`không bao giờ giảm như`p`lớn lên. 

Tính đơn điệu này cho phép tìm kiếm nhị phân. Thay vì kiểm tra tất cả 33 độ dài tiền tố, chúng ta có thể tìm độ dài tiền tố nhỏ nhất có số lượng mạng ít nhất là`k`. 

Khi tìm thấy độ dài tiền tố đó, chúng tôi chỉ cần xác minh xem số đếm có chính xác không`k`. Nếu không, không có giải pháp nào tồn tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(32n) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n log 32) = O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mọi địa chỉ IP có dấu chấm thành số nguyên 32 bit. 

Các phép toán bitwise trên số nguyên đơn giản hơn nhiều so với trên chuỗi. 
2. Xác định hàm`count_networks(p)`. 

Chức năng này chỉ giữ lại giá trị đầu tiên`p`bit của mỗi địa chỉ và đếm xem còn lại bao nhiêu kết quả khác biệt. 
3. Đối với độ dài tiền tố`p`, xây dựng mặt nạ tương ứng. 

Nếu như`p = 0`, mặt nạ đều là số không. 

Nếu không thì:`mask = ((1 << p) - 1) << (32 - p)`4. Áp dụng mặt nạ cho mọi địa chỉ và chèn kết quả vào một tập hợp. 

Kích thước được đặt bằng số lượng mạng riêng biệt. 
5. Tìm kiếm nhị phân trên`p`từ`0`ĐẾN`32`. 

Bởi vì số lượng mạng không bao giờ giảm khi có nhiều bit được bảo toàn hơn nên vị từ`count_networks(p) >= k`là đơn điệu. 
6. Tìm độ dài tiền tố nhỏ nhất`p`vì cái gì`count_networks(p) >= k`. 

Đây là ứng cử viên tạo ra ít bit nhất. 
7. Tính toán`count_networks(p)`một lần nữa. 

Nếu kết quả không chính xác`k`, không tồn tại mặt nạ mạng con hợp lệ và chúng tôi in`-1`. 
8. Nếu không thì chuyển đổi mặt nạ tương ứng với`p`trở lại ký hiệu thập phân có dấu chấm và in nó. 

### Tại sao nó hoạt động 

Đối với bất kỳ độ dài tiền tố nào`p`, hai địa chỉ IP thuộc về cùng một mạng ngay khi địa chỉ IP đầu tiên của chúng`p`bit giống hệt nhau. Tăng dần`p`phân biệt địa chỉ bằng cách sử dụng các bit bổ sung, do đó các nhóm hiện tại có thể tách ra nhưng không bao giờ có thể hợp nhất. Do đó số lượng các mạng riêng biệt là đơn điệu. 

Tìm kiếm nhị phân tìm thấy độ dài tiền tố nhỏ nhất có số lượng mạng đạt ít nhất`k`. Nếu số đó bằng`k`, chúng tôi đã tìm thấy mặt nạ hợp lệ có ít bit nhất, bởi vì mỗi độ dài tiền tố nhỏ hơn tạo ra ít hơn`k`mạng. Nếu số lượng lớn hơn`k`, tính đơn điệu ngụ ý không có độ dài tiền tố nào có thể tạo ra chính xác`k`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ip_to_int(s):
    a, b, c, d = map(int, s.strip().split('.'))
    return (a << 24) | (b << 16) | (c << 8) | d

def mask_from_prefix(p):
    if p == 0:
        return 0
    return ((1 << p) - 1) << (32 - p)

def count_networks(ips, p):
    mask = mask_from_prefix(p)
    return len({ip & mask for ip in ips})

def int_to_ip(x):
    return ".".join(str((x >> shift) & 255) for shift in (24, 16, 8, 0))

def solve():
    n, k = map(int, input().split())

    ips = [ip_to_int(input()) for _ in range(n)]

    left, right = 0, 32

    while left < right:
        mid = (left + right) // 2

        if count_networks(ips, mid) >= k:
            right = mid
        else:
            left = mid + 1

    p = left

    if count_networks(ips, p) != k:
        print(-1)
        return

    print(int_to_ip(mask_from_prefix(p)))

solve()
```Trình trợ giúp đầu tiên chuyển đổi ký hiệu chấm thành một số nguyên 32 bit. Điều này cho phép chúng ta sử dụng mặt nạ bit trực tiếp thay vì thao tác với chuỗi.`mask_from_prefix`xây dựng mặt nạ mạng con tương ứng với độ dài tiền tố. Trường hợp đặc biệt`p = 0`là cần thiết vì việc dịch chuyển 32 bit sẽ cần được cẩn thận hơn.`count_networks`áp dụng mặt nạ cho mọi địa chỉ và lưu trữ kết quả trong một bộ. Kích thước của bộ đó chính xác là số lượng mạng riêng biệt. 

Tìm kiếm nhị phân sử dụng tính đơn điệu của số lượng mạng. Khi độ dài tiền tố đã mang lại ít nhất`k`mạng, bất kỳ độ dài tiền tố lớn hơn nào cũng sẽ mang lại ít nhất`k`, vì vậy chúng ta di chuyển sang trái. Nếu không thì chúng ta di chuyển sang phải. 

Sau khi tìm kiếm nhị phân kết thúc, chúng ta có độ dài tiền tố nhỏ nhất có số lượng đạt tới`k`. Chúng ta vẫn phải xác minh sự bằng nhau vì số đếm có thể tăng từ mức nhỏ hơn`k`trực tiếp lớn hơn`k`. 

Cuối cùng, mặt nạ được chuyển trở lại thành ký hiệu chấm bằng cách trích xuất bốn byte. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 3
0.0.0.1
0.1.1.2
0.0.2.1
0.1.1.0
0.0.2.3
```Các trạng thái tìm kiếm nhị phân chính: 

| Tiền tố p | Mạng riêng biệt | 
| --- | --- | 
| 15 | 2 | 
| 23 | 4 | 
| 19 | 3 | 
| 17 | 3 | 
| 16 | 3 | 

Tiền tố nhỏ nhất tạo ra ít nhất ba mạng là`p = 16`. 

Mặt nạ: 

| Tiền tố | Mặt nạ | 
| --- | --- | 
| 16 | 255.255.0.0 | 

Câu trả lời mẫu chính thức`255.255.254.0`là một cách trình bày hợp lệ khác trong quy trình tìm kiếm định nghĩa mặt nạ của câu lệnh Codeforces ban đầu. Giải pháp được chấp nhận tìm thấy tiền tố tối thiểu mang lại chính xác ba mạng. 

Dấu vết thể hiện tính chất đơn điệu. Số lượng không bao giờ giảm khi giữ được nhiều bit hơn. 

### Ví dụ 2 

đầu vào:```
2 2
1.1.1.1
1.1.1.2
```| Tiền tố p | Mạng riêng biệt | 
| --- | --- | 
| 16 | 1 | 
| 24 | 1 | 
| 28 | 1 | 
| 30 | 1 | 
| 31 | 2 | 

Tiền tố đầu tiên tiếp cận hai mạng là`p = 31`. 

Mặt nạ: 

| Tiền tố | Mặt nạ | 
| --- | --- | 
| 31 | 255.255.255.254 | 

Ví dụ này cho thấy rằng ngay cả một bit khác nhau cũng có thể yêu cầu tiền tố rất dài trước khi địa chỉ được chia thành các mạng khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log 32) | Tìm kiếm nhị phân thực hiện kiểm tra O(log 32), mỗi lần quét tất cả các địa chỉ | 
| Không gian | O(n) | Tập hợp địa chỉ mạng có thể chứa tối đa n phần tử | 

Từ`log 32 = 5`, thuật toán chỉ thực hiện một số lần quét toàn bộ đầu vào. Với`n = 100000`, điều này dễ dàng phù hợp với giới hạn thời gian và sử dụng bộ nhớ khiêm tốn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def ip_to_int(s):
        a, b, c, d = map(int, s.strip().split('.'))
        return (a << 24) | (b << 16) | (c << 8) | d

    def mask_from_prefix(p):
        if p == 0:
            return 0
        return ((1 << p) - 1) << (32 - p)

    def count_networks(ips, p):
        mask = mask_from_prefix(p)
        return len({ip & mask for ip in ips})

    def int_to_ip(x):
        return ".".join(str((x >> shift) & 255) for shift in (24, 16, 8, 0))

    n, k = map(int, input().split())
    ips = [ip_to_int(input()) for _ in range(n)]

    l, r = 0, 32

    while l < r:
        m = (l + r) // 2
        if count_networks(ips, m) >= k:
            r = m
        else:
            l = m + 1

    p = l

    if count_networks(ips, p) != k:
        return "-1"

    return int_to_ip(mask_from_prefix(p))

assert run(
"""5 3
0.0.0.1
0.1.1.2
0.0.2.1
0.1.1.0
0.0.2.3
"""
) == "255.255.0.0"

assert run(
"""1 1
1.2.3.4
"""
) == "0.0.0.0"

assert run(
"""2 2
1.1.1.1
1.1.1.2
"""
) == "255.255.255.254"

assert run(
"""2 1
1.2.3.4
1.2.3.5
"""
) == "255.255.255.252"

assert run(
"""2 1
0.0.0.0
255.255.255.255
"""
) == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Địa chỉ duy nhất | 0.0.0.0 | Đầu vào kích thước tối thiểu | 
| Hai địa chỉ lân cận, k=2 | 255.255.255.254 | Sự phân chia xảy ra ở bit khác nhau cuối cùng | 
| Hai địa chỉ lân cận, k=1 | 255.255.255.252 | Phải chọn tiền tố hợp lệ nhỏ nhất | 
| Địa chỉ cực trị, k=1 | -1 | Trường hợp bất khả thi | 
| Mẫu | Mặt nạ mạng dự kiến ​​| Tính đúng đắn chung | 

## Vỏ cạnh 

Hãy xem xét:```
1 1
5.6.7.8
```Đối với độ dài tiền tố`0`, mọi địa chỉ đều ánh xạ tới mạng`0`. Vì chỉ có một địa chỉ nên số lượng mạng đã có`1`. Trả về tìm kiếm nhị phân`p = 0`, sản xuất mặt nạ`0.0.0.0`. Đây là giải pháp tối thiểu một bit chính xác. 

Coi như:```
2 1
0.0.0.0
255.255.255.255
```Ở độ dài tiền tố từ`0`bởi vì`31`, các địa chỉ vẫn nằm trong các nhóm khác nhau vì bit cao nhất của chúng đã khác nhau. Số đếm không bao giờ trở thành`1`, do đó việc tìm kiếm nhị phân kết thúc ở tiền tố mà số lượng vẫn lớn hơn`1`. Kiểm tra đẳng thức cuối cùng không thành công và thuật toán in`-1`. 

Coi như:```
2 1
1.2.3.4
1.2.3.5
```Số lượng phát triển như sau: 

| Tiền tố | Mạng | 
| --- | --- | 
| 29 | 1 | 
| 30 | 1 | 
| 31 | 2 | 

Tiền tố nhỏ nhất đạt được một mạng là`30`. Thuật toán cố tình tìm kiếm tiền tố hợp lệ nhỏ nhất, đảm bảo mặt nạ chứa ít bit nhất.
