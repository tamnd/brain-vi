---
title: "CF 102416C - Cà phê nhanh"
description: "Chúng ta cần kiếm chính xác d đô la tiền lẻ. Các mệnh giá tiền xu có sẵn là mọi số nguyên từ a đến giới hạn trên b, bao gồm và chúng tôi có thể sử dụng bất kỳ mệnh giá nào bao nhiêu lần cũng được."
date: "2026-08-12T20:49:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102416
codeforces_index: "C"
codeforces_contest_name: "Edinburgh Competition 2019"
rating: 0
weight: 102416
solve_time_s: 152
verified: true
draft: false
---

[CF 102416C - Cà phê nhanh](https://codeforces.com/problemset/problem/102416/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 32s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần thực hiện chính xác`d`đô la thay đổi. Các mệnh giá tiền xu có sẵn là mọi số nguyên từ`a`thông qua một số giới hạn trên`b`, bao gồm và chúng tôi có thể sử dụng bất kỳ mệnh giá nào bao nhiêu lần cũng được. Nhiệm vụ là tìm số nhỏ nhất có thể`b`vì cái gì`d`có thể được biểu diễn dưới dạng tổng của những đồng tiền này. 

Đầu vào chứa`a`Và`d`, với`1 <= a <= d <= 10^9`. Vì các giá trị có thể đạt tới một tỷ nên thuật toán kiểm tra số lượng tuyến tính các giá trị có thể là quá đắt đối với giới hạn 1 giây. Chúng ta cần quy bài toán về số học theo thời gian không đổi thay vì thử từng giá trị ứng cử viên. 

Trường hợp ranh giới đầu tiên là`a = d`. Cách duy nhất để làm`d`là sử dụng đồng tiền có giá trị`d`, vậy đáp án phải là`d`. Ví dụ,`10 10`cho`10`. Một cách tiếp cận bất cẩn giả định rằng cần ít nhất hai đồng xu có thể trả về một giá trị nhỏ hơn một cách không chính xác. 

Một trường hợp nhỏ khác là`a = 1`. Ví dụ,`1 7`có câu trả lời`1`, bởi vì đồng tiền có giá trị`1`một mình có thể tạo ra mọi số tiền dương. Một tìm kiếm bắt đầu từ`a + 1`sẽ bỏ lỡ điều này ngay lập tức. 

Trường hợp ở đó`d`chia hết cho`a`cũng quan trọng. Vì`6 18`, ba đồng tiền có giá trị`6`đã thực hiện được mục tiêu rồi, vì vậy`b = 6`. Một giải pháp dựa trên làm tròn`d / floor(d / a)`số học dấu phẩy động không chính xác có thể gây ra các vấn đề về độ chính xác ở các giá trị lớn, vì vậy nên chia trần số nguyên. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử mọi mệnh giá cao hơn có thể`b`, bắt đầu từ`a`. Đối với mỗi ứng cử viên, chúng ta có thể xác định liệu`d`có thể được hình thành bằng cách sử dụng tiền từ`a`bởi vì`b`. Điều này đúng vì chúng tôi kiểm tra các ứng viên theo thứ tự tăng dần, vì vậy phương án khả thi đầu tiên nhất thiết phải là mức tối thiểu. 

Vấn đề là ở chỗ đó`b`có thể lớn như`d`, Và`d`có thể`10^9`. Trong trường hợp xấu nhất, điều này có nghĩa là kiểm tra đại khái`10^9`ứng viên. Ngay cả khi mỗi ứng viên có thể được kiểm tra trong thời gian không đổi, thì đó là khoảng một tỷ phép tính, gần như không phù hợp với giới hạn thời gian 1 giây. 

Quan sát hữu ích đến từ việc ấn định số lượng đồng xu thay vì ấn định mệnh giá lớn nhất. Giả sử chúng ta sử dụng chính xác`k`tiền xu. Tổng nhỏ nhất có thể là`ka`, bởi vì mỗi đồng xu ít nhất`a`. Tổng lớn nhất có thể là`kb`, bởi vì mỗi đồng xu nhiều nhất là`b`. 

Quan trọng hơn, bởi vì mọi giáo phái giữa`a`Và`b`tồn tại, mọi tổng số nguyên giữa`ka`Và`kb`là có thể đạt được. Như vậy`d`có thể được hình thành với chính xác`k`tiền xu chính xác khi nào`ka <= d <= kb`. 

Bất đẳng thức thứ nhất cho`k <= floor(d / a)`. Cho phép`q = floor(d / a)`. 

Nếu chúng ta có thể sử dụng nhiều nhất`q`đồng xu, sau đó chọn`q`tiền xu mang lại cho chúng tôi sự linh hoạt lớn nhất có thể ở giới hạn dưới, bởi vì`qa <= d`. Chúng tôi chỉ cần`b`đủ lớn đó`q`tiền xu có thể đạt được`d`, có nghĩa là`qb >= d`. 

Do đó nhỏ nhất có thể`b`là`ceil(d / q)`,

Ở đâu`q = floor(d / a)`. 

Không có lý do gì để xem xét ít hơn`q`tiền xu. Nếu như`k < q`, sau đó`ceil(d / k) >= ceil(d / q)`, do đó yêu cầu ít tiền hơn chỉ có thể làm cho mệnh giá tối đa cần thiết lớn hơn hoặc bằng. Việc tìm kiếm vũ phu có thể`b`do đó đã được giảm xuống còn hai phép chia số nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(d - a + 1) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`a`Và`d`. Đây là giá trị đồng xu tối thiểu có sẵn và số tiền mục tiêu. 
2. Tính toán`q = d // a`. Đây là số lượng xu tối đa có giá trị ít nhất`a`có thể phù hợp với một tổng`d`. 
3. Tính giới hạn mệnh giá nhỏ nhất cho phép`q`đồng xu đạt`d`. Chúng tôi cần`q * b >= d`, nên dùng phép chia trần số nguyên:`b = (d + q - 1) // q`. 
4. In`b`. Từ`q`là số lượng xu lớn nhất có thể, giá trị này là mệnh giá trên nhỏ nhất có thể. 

### Tại sao nó hoạt động 

hãy để`q = floor(d / a)`. Từ`qa <= d`, sử dụng`q`đồng xu không vượt mục tiêu khi tất cả chúng đều có giá trị`a`. Chúng ta cần những thứ đó`q`đồng tiền có khả năng tiếp cận`d`, đòi hỏi`qb >= d`. 

Bởi vì các mệnh giá có sẵn tạo thành khoảng số nguyên đầy đủ từ`a`ĐẾN`b`, mọi tổng giữa`qa`Và`qb`có thể đạt được một cách chính xác`q`tiền xu. Như vậy`b = ceil(d / q)`là đủ. 

Đối với bất kỳ số lượng xu nhỏ hơn`k < q`, đạt`d`sẽ yêu cầu`b >= ceil(d / k)`. Từ`k < q`, chúng tôi có`ceil(d / k) >= ceil(d / q)`. Do đó không nhỏ hơn`b`có thể làm việc với ít tiền hơn. Giá trị tính toán vừa khả thi vừa tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, d = map(int, input().split())

q = d // a
b = (d + q - 1) // q

print(b)
```giá trị`q`được tính bằng phép chia số nguyên, vì vậy nó chính xác`floor(d / a)`. Từ`a <= d`,`q`ít nhất luôn luôn là`1`, điều này làm cho phép chia tiếp theo được an toàn. 

biểu hiện`(d + q - 1) // q`thực hiện phép chia trần mà không cần số học dấu phẩy động. Điều này quan trọng vì đầu vào có thể chứa các giá trị lên tới`10^9`và số học số nguyên cho kết quả chính xác mà không cần quan tâm đến việc làm tròn. 

Số nguyên Python cũng có độ chính xác tùy ý, do đó không có vấn đề tràn số nguyên. Trong các ngôn ngữ có loại số nguyên có chiều rộng cố định, công thức tương tự vẫn an toàn cho các ràng buộc này với số nguyên 64 bit tiêu chuẩn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên,`a = 19`Và`d = 60`. 

| Biến | Giá trị | Lý do | 
| --- | --- | --- | 
|`a`| 19 | Đồng xu tối thiểu | 
|`d`| 60 | Số tiền mục tiêu | 
|`q = d // a`| 3 | Có thể sử dụng tối đa ba đồng xu | 
|`b = ceil(d / q)`| 20 | Ba đồng xu có giá trị tối đa 20 có thể đạt tới 60 | 
| Trả lời | 20 | Mệnh giá trên hợp lệ tối thiểu | 

Với tiền xu`19`Và`20`, mục tiêu chỉ đơn giản là`20 + 20 + 20 = 60`. Chỉ với mệnh giá`19`, tổng có thể có xung quanh mục tiêu không bao gồm`60`, bởi vì ba đồng xu cho`57`và bốn đồng xu cho`76`. Như vậy`20`chính xác là giới hạn trên khả thi đầu tiên. 

Đối với mẫu thứ hai,`a = 100`Và`d = 914`. 

| Biến | Giá trị | Lý do | 
| --- | --- | --- | 
|`a`| 100 | Đồng xu tối thiểu | 
|`d`| 914 | Số tiền mục tiêu | 
|`q = d // a`| 9 | Có thể sử dụng tối đa chín xu | 
|`b = ceil(d / q)`| 102 | Chín đồng tiền cần mệnh giá tối đa ít nhất là 102 | 
| Trả lời | 102 | Mệnh giá trên hợp lệ tối thiểu | 

Với các mệnh giá từ`100`bởi vì`102`, chín đồng xu có thể làm được`914`. Bắt đầu với chín`100`đồng xu cho`900`, sau đó tăng giá trị của bảy đồng tiền đó lên`2`, cho`102 + 102 + 102 + 102 + 102 + 102 + 102 + 100 + 100 = 914`. Với`b = 101`, chín đồng xu có tổng giá trị tối đa`909`, do đó giới hạn đó là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Thuật toán thực hiện một số phép toán số nguyên không đổi. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ. | 

Các ràng buộc cho phép các giá trị lên đến`10^9`, điều này làm cho bất kỳ tìm kiếm nào tỷ lệ thuận với mục tiêu đều có khả năng đạt tới một tỷ lần lặp. Giải pháp thời gian không đổi tránh được điều đó hoàn toàn và dễ dàng phù hợp trong giới hạn 1 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(data: str) -> str:
    a, d = map(int, data.split())

    q = d // a
    b = (d + q - 1) // q

    return str(b)

def run(inp: str) -> str:
    return solve(inp).strip()

# Provided samples
assert run("19 60") == "20", "sample 1"
assert run("100 914") == "102", "sample 2"

# Minimum-size input
assert run("1 1") == "1", "minimum values"

# Same minimum and target
assert run("10 10") == "10", "a equals d"

# Divisible case
assert run("6 18") == "6", "d is divisible by a"

# Small boundary case
assert run("3 10") == "5", "ceil(10 / floor(10 / 3)) = 5"

# Large values
assert run("500000000 1000000000") == "500000000", "large divisible case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Đầu vào tối thiểu có thể và`q = 1`| 
|`10 10`|`10`| Ranh giới trong đó số tiền tối thiểu bằng với mục tiêu | 
|`6 18`|`6`| Chia trần chính xác và chia trần | 
|`3 10`|`5`| Trường hợp không chia hết bắt lỗi làm tròn | 
|`500000000 1000000000`|`500000000`| Giá trị lớn và số học theo thời gian không đổi | 

## Vỏ cạnh 

cho`a = d`, coi như`10 10`. Việc tính toán mang lại`q = 10 // 10 = 1`, theo sau là`b = ceil(10 / 1) = 10`. Kết quả là đúng vì mệnh giá duy nhất có thể làm được mục tiêu là`10`chính nó. Thuật toán không vô tình cho rằng cần có nhiều đồng tiền. 

Vì`a = 1`, coi như`1 7`. Chúng tôi có được`q = 7`, Vì thế`b = ceil(7 / 7) = 1`. Điều này nói rằng giáo phái duy nhất`1`là đủ. Quả thực, bảy đồng tiền có giá trị`1`thực hiện mục tiêu một cách chính xác. Trường hợp này cũng xác nhận rằng câu trả lời có thể bằng giá trị đầu vào nhỏ nhất có thể`a`. 

Đối với một mục tiêu có thể chia chính xác, hãy xem xét`6 18`. Đây`q = 18 // 6 = 3`, Và`b = ceil(18 / 3) = 6`. Ba đồng tiền có giá trị`6`làm`18`, vì vậy không cần thiết phải có mệnh giá lớn hơn. Việc tính toán diễn ra chính xác trên`a`, đó là câu trả lời nhỏ nhất có thể. 

Đối với mục tiêu không thể chia được, hãy xem xét`3 10`. Chúng tôi có`q = 10 // 3 = 3`. Ba đồng xu phải đạt được`10`, do đó mệnh giá tối đa phải thỏa mãn`3b >= 10`, cho`b = 4`. Công thức thực tế mang lại`(10 + 3 - 1) // 3 = 12 // 3 = 4`, không`3`. Với các mệnh giá`3`Và`4`, mục tiêu là`3 + 3 + 4 = 10`. Đây là trường hợp bộc lộ việc sử dụng sai cách phân chia sàn thông thường. 

Cuối cùng, hãy xem xét mục tiêu tối đa`a = 500000000`,`d = 1000000000`. chúng tôi nhận được`q = 2`, Và`b = ceil(1000000000 / 2) = 500000000`. Hai đồng tiền có giá trị`500000000`thực hiện mục tiêu một cách chính xác. Việc tính toán duy trì thời gian không đổi ngay cả ở quy mô đầu vào lớn nhất.
