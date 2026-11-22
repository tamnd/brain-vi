---
title: "CF 306A - Kẹo"
description: "Polycarpus có một số kẹo nhất định và một số bạn bè. Anh ấy muốn phân phát tất cả số kẹo cho những người bạn của mình theo cách sao cho mỗi người bạn đều nhận được số kẹo dương và sự khác biệt giữa người bạn nhận được nhiều kẹo nhất và người bạn…"
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 306
codeforces_index: "A"
codeforces_contest_name: "Testing Round 6"
rating: 800
weight: 306
solve_time_s: 200
verified: false
draft: false
---

[CF 306A - Kẹo](https://codeforces.com/problemset/problem/306/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 3 phút 20s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Polycarpus có một số kẹo nhất định và một số bạn bè. Anh ấy muốn phân phát tất cả số kẹo cho những người bạn của mình sao cho mỗi người bạn đều nhận được số kẹo dương và sự khác biệt giữa người bạn nhận được nhiều kẹo nhất và người bạn nhận được ít kẹo nhất là càng nhỏ càng tốt. Đầu vào cho chúng ta hai số nguyên: tổng số kẹo`n`và số lượng bạn bè`m`. Đầu ra phải là một chuỗi các`m`các số nguyên dương có tổng bằng`n`và sự khác biệt giữa số nguyên lớn nhất và nhỏ nhất phải được giảm thiểu. 

Với những hạn chế,`n`Và`m`cả hai đều nhiều nhất là 100. Con số này đủ nhỏ để bất kỳ chiến lược phân phối đơn giản nào cũng sẽ chạy hiệu quả, do đó chúng ta không phải lo lắng về hiệu suất với các thuật toán có độ phức tạp cao. Thách thức chính là lý luận về bản thân sự phân phối. 

Một trường hợp cạnh quan trọng là khi`n`không chia hết cho`m`. Ví dụ: nếu Polycarpus có 10 chiếc kẹo và 3 người bạn, một cách tiếp cận ngây thơ có thể cố gắng tặng cho mỗi người bạn`n // m = 3`kẹo, nhưng khi đó chúng ta sẽ còn dư 1 viên kẹo. Cách tiếp cận đúng là tặng thêm một số bạn bè một viên kẹo để cân bằng việc phân phối:`[4, 3, 3]`. Một trường hợp cạnh khác là khi`n`bằng`m`; mỗi người bạn chỉ cần nhận được một viên kẹo. Một giải pháp bất cẩn không xử lý phần còn lại một cách chính xác sẽ thất bại đối với bất kỳ đầu vào nào trong đó`n % m != 0`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là thử mọi cách phân phối có thể của`n`kẹo trong số`m`bạn bè và kiểm tra sự khác biệt giữa giá trị tối đa và tối thiểu. Điều này được đảm bảo để tìm ra giải pháp tối ưu, nhưng số lượng kết hợp tăng theo cấp số nhân với`n`Và`m`. Cụ thể, số lượng phân vùng của`n`vào trong`m`các số nguyên dương có tính chất tổ hợp và có thể dễ dàng vượt quá hàng nghìn, điều này là không cần thiết do cấu trúc đơn giản của bài toán. 

Cái nhìn sâu sắc quan trọng để tối ưu hóa điều này là sự phân phối công bằng nhất luôn là sự phân phối gần như đồng đều. Mỗi người bạn sẽ nhận được một trong hai`floor(n / m)`hoặc`ceil(n / m)`kẹo. Cho phép`q = n // m`là số kẹo cơ bản cho mỗi người bạn và`r = n % m`là phần còn lại. Chính xác`r`bạn bè nên có được`q + 1`kẹo và phần còn lại`m - r`bạn bè có được`q`kẹo. Điều này đảm bảo rằng chênh lệch tối đa là 1, mức chênh lệch tối thiểu và không thể cải thiện vì nếu tất cả các giá trị đều bằng nhau và`n % m != 0`, chúng ta sẽ không đạt được tổng số tiền`n`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổ hợp) | O(m) | Quá chậm | 
| Tối ưu | O(m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`Và`m`từ đầu vào. Chúng đại diện cho tổng số kẹo và số lượng bạn bè. 
2. Tính toán`q = n // m`là số kẹo cơ bản mà mỗi người bạn sẽ nhận được. Điều này đảm bảo mọi người đều nhận được ít nhất số tiền tối thiểu như nhau. 
3. Tính toán`r = n % m`vì số kẹo còn lại vẫn cần được phân phát. Những viên kẹo bổ sung này sẽ được tặng cho mỗi người bạn một chiếc để giảm thiểu sự khác biệt tối đa. 
4. Tạo danh sách kích thước`m`nơi đầu tiên`r`các yếu tố là`q + 1`và phần còn lại`m - r`các yếu tố là`q`. Điều này đảm bảo rằng chính xác`r`bạn bè nhận thêm một viên kẹo, cân bằng việc phân phối. 
5. In danh sách dưới dạng số nguyên được phân tách bằng dấu cách. 

Tại sao nó hoạt động: Điều bất biến là mỗi người bạn nhận được ít nhất`q`kẹo, và chính xác`r`bạn bè nhận được thêm một viên kẹo. Tổng của tất cả các giá trị là`q*(m-r) + (q+1)*r = n`, đảm bảo tính đúng đắn. Sự khác biệt giữa giá trị lớn nhất và nhỏ nhất nhiều nhất là 1, là mức tối thiểu vì việc phân phối phần còn lại yêu cầu một số bạn bè nhận thêm kẹo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
q, r = divmod(n, m)
result = [q + 1] * r + [q] * (m - r)
print(' '.join(map(str, result)))
```Mã đầu tiên tính thương số và số dư bằng cách sử dụng`divmod`, sạch hơn các phép toán chia và modulo riêng biệt. Danh sách sau đó được xây dựng bằng cách lặp lại`q + 1`chính xác`r`lần và`q`chính xác`m - r`lần. sử dụng`join`chuyển đổi các số nguyên thành chuỗi và in chúng theo định dạng được yêu cầu. Tránh được từng lỗi một bằng cách xử lý cẩn thận phần còn lại`r`và đảm bảo tất cả bạn bè đều nhận được số nguyên dương. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`12 3`| Bước | q | r | Danh sách kết quả | 
| --- | --- | --- | --- | 
| Tính q,r | 4 | 0 | [] | 
| Xây dựng danh sách | 4_0 + 4_3 | 0 | [4, 4, 4] | 

Tất cả bạn bè đều nhận được số kẹo như nhau. Chênh lệch tối đa là 0. 

### Ví dụ 2 

đầu vào:`10 3`| Bước | q | r | Danh sách kết quả | 
| --- | --- | --- | --- | 
| Tính q,r | 3 | 1 | [] | 
| Xây dựng danh sách | [3+1]*1 + [3]*2 | 1 | [4, 3, 3] | 

Một người bạn nhận thêm một chiếc kẹo để chia phần còn lại. Sự khác biệt tối đa là 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | Chúng tôi tạo một danh sách có độ dài`m`và in nó. Mỗi hoạt động là tuyến tính trong`m`. | 
| Không gian | O(m) | Chúng tôi lưu trữ danh sách kích thước kết quả`m`. | 

Được cho`m ≤ 100`, điều này cực kỳ nhanh và tiết kiệm bộ nhớ. Giải pháp dễ dàng chạy trong vòng 1 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    q, r = divmod(n, m)
    result = [q + 1] * r + [q] * (m - r)
    return ' '.join(map(str, result))

# provided samples
assert run("12 3\n") == "4 4 4", "sample 1"
assert run("10 3\n") == "4 3 3", "remainder distribution"

# custom cases
assert run("1 1\n") == "1", "minimum input"
assert run("100 1\n") == "100", "single friend, all candies"
assert run("100 100\n") == "1 "*100[:-1], "each friend gets one candy"
assert run("7 3\n") == "3 2 2", "small uneven distribution"
assert run("15 4\n") == "4 4 4 3", "uneven with remainder less than half"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | đầu vào tối thiểu | 
| 100 1 | 100 | người bạn độc thân nhận được tất cả kẹo | 
| 100 100 | 1 1 ... 1 | bạn bè tối đa, phân phối tối thiểu | 
| 7 3 | 3 2 2 | độ chính xác phân phối phần dư | 
| 15 4 | 4 4 4 3 | phân bố số dư khi số dư < m/2 | 

## Vỏ cạnh 

Khi nào`n`bằng`m`, chẳng hạn như`5 5`, mỗi người bạn nhận được đúng 1 viên kẹo. Thuật toán tính toán`q = 1`Và`r = 0`. Danh sách kết quả là`[1, 1, 1, 1, 1]`. Khi`n`lớn hơn một chút so với`m`, chẳng hạn như`7 3`, chúng tôi tính toán`q = 2`Và`r = 1`. đầu tiên`r`người bạn nhận được`q + 1 = 3`kẹo và hai người còn lại nhận được`2`từng viên kẹo. Điều này đảm bảo rằng chênh lệch tối đa chính xác là 1, không thể giảm được. Thuật toán xử lý khéo léo cả hai trường hợp mà không cần thêm điều kiện.
