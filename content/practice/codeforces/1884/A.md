---
title: "CF 1884A - Thiết Kế Đơn Giản"
description: "Đối với mỗi trường hợp thử nghiệm, chúng ta được cấp một số bắt đầu x và một số nguyên k. Một số được coi là đẹp nếu tổng các chữ số thập phân của nó chia hết cho k. Nhiệm vụ là tìm số nguyên y nhỏ nhất sao cho y ≥ x và tổng các chữ số của y chia hết cho k."
date: "2026-06-08T22:22:58+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1884
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 904 (Div. 2)"
rating: 800
weight: 1884
solve_time_s: 104
verified: true
draft: false
---

[CF 1884A - Thiết kế đơn giản](https://codeforces.com/problemset/problem/1884/A) 

**Đánh giá:** 800 
**Tags:** vũ phu, tham lam, toán học 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đối với mỗi trường hợp thử nghiệm, chúng tôi được cấp một số bắt đầu`x`và một số nguyên`k`. Một số được coi là đẹp nếu tổng các chữ số thập phân của nó chia hết cho`k`. 

Nhiệm vụ là tìm số nguyên nhỏ nhất`y`như vậy`y ≥ x`và tổng các chữ số của`y`chia hết cho`k`. 

Các ràng buộc nhỏ một cách đáng ngạc nhiên từ góc độ thuật toán. Giá trị của`x`có thể đạt được`10^9`, Nhưng`k`nhiều nhất là`10`. Số lượng ca kiểm thử có thể lớn tới mức`10^4`, vì vậy giải pháp cho một trường hợp thử nghiệm sẽ cực kỳ rẻ. 

Nhận xét quan trọng từ các ràng buộc là tổng các chữ số nhỏ. Ngay cả đối với những con số xung quanh`10^9`, tổng các chữ số nhiều nhất là`9 × 10 = 90`. Từ`k ≤ 10`, chúng ta không cần phải tìm kiếm quá xa trước khi tìm được câu trả lời hợp lệ. Điều này làm cho việc tìm kiếm trực tiếp trở nên thực tế. 

Một sai lầm dễ mắc phải là cho rằng câu trả lời phải lớn hơn`x`. Coi như:```
x = 777, k = 3
```Tổng các chữ số là`7 + 7 + 7 = 21`, vốn đã chia hết cho`3`. Câu trả lời đúng là:```
777
```Một lỗi phổ biến khác là kiểm tra tính chia hết của chính số đó thay vì tổng các chữ số của nó. Ví dụ:```
x = 17, k = 8
```số`17`không chia hết cho`8`, nhưng tổng chữ số của nó là`1 + 7 = 8`, chia hết cho`8`. Câu trả lời đúng là:```
17
```Trường hợp tinh tế thứ ba xảy ra khi một số số liên tiếp bị lỗi trước khi số hợp lệ đầu tiên xuất hiện.```
x = 1, k = 10
```Tổng các chữ số là:```
1 -> 1
2 -> 2
...
18 -> 9
19 -> 10
```Câu trả lời là:```
19
```Việc triển khai bất cẩn chỉ kiểm tra một vài số gần đó có thể bỏ sót ứng cử viên hợp lệ đầu tiên. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là bắt đầu từ`x`và liên tục kiểm tra từng số một. 

Đối với số ứng viên`cur`, tính tổng các chữ số của nó. Nếu số tiền đó chia hết cho`k`, chúng ta đã tìm được câu trả lời hợp lệ nhỏ nhất vì chúng ta đã kiểm tra các số theo thứ tự tăng dần. Ngược lại thì tăng`cur`và tiếp tục. 

Lực lượng vũ phu là chính xác bởi vì mọi số lớn hơn hoặc bằng`x`được kiểm tra theo thứ tự và số đẹp đầu tiên gặp phải là đáp án nhỏ nhất có thể. 

Thông thường, tìm kiếm tuyến tính trên số nguyên sẽ nguy hiểm. Nếu khoảng cách giữa các câu trả lời hợp lệ có thể rất lớn thì thời gian chạy trong trường hợp xấu nhất sẽ trở nên không thể chấp nhận được. 

Quan sát quan trọng đó là`k ≤ 10`. Tổng chữ số thay đổi thường xuyên và trong số bất kỳ phạm vi số nhỏ hợp lý nào, chúng ta sẽ gặp một tổng chữ số chia hết cho bất kỳ giá trị nào từ`1`ĐẾN`10`. Trong thực tế, việc tìm kiếm không bao giờ đi xa. Đây chính xác là giải pháp dự định cho vấn đề. 

Việc tính tổng các chữ số chỉ mất một vài thao tác vì các số có tối đa mười chữ số. Ngay cả với`10^4`trường hợp thử nghiệm, tổng công việc là rất nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm vũ phu | O(g · d) | O(1) | Đã chấp nhận | 
| Tối ưu (cùng quan sát) | O(g · d) | O(1) | Đã chấp nhận | 

Đây`g`là số lượng ứng viên được chọn cho đến khi tìm được câu trả lời, và`d`là số chữ số. Từ`k ≤ 10`,`g`vẫn còn rất nhỏ. 

## Hướng dẫn thuật toán 

1. Đọc`x`Và`k`. 
2. Đặt`cur = x`. 
3. Tính tổng các chữ số của`cur`. 
4. Nếu tổng các chữ số chia hết cho`k`, đầu ra`cur`và ngừng xử lý trường hợp thử nghiệm này. 
5. Nếu không thì tăng`cur`bởi một. 
6. Lặp lại từ bước 3. 

Việc tìm kiếm tiến hành theo thứ tự số tăng dần. Số hợp lệ đầu tiên gặp phải tự động là câu trả lời hợp lệ nhỏ nhất. 

### Tại sao nó hoạt động 

Tại mỗi lần lặp, tất cả các số nguyên từ`x`bởi vì`cur - 1`đã được kiểm tra và bị loại vì tổng chữ số của chúng không chia hết cho`k`. 

Khi thuật toán dừng ở một giá trị nào đó`cur`, tổng các chữ số của nó chia hết cho`k`. Vì mọi số nhỏ hơn đều lớn hơn hoặc bằng`x`đã được kiểm tra và thấy không hợp lệ, không có câu trả lời hợp lệ nào nhỏ hơn tồn tại. Như vậy`cur`chính xác là số đẹp nhỏ nhất không nhỏ hơn`x`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def digit_sum(n):
    s = 0
    while n:
        s += n % 10
        n //= 10
    return s

t = int(input())

for _ in range(t):
    x, k = map(int, input().split())

    cur = x
    while True:
        if digit_sum(cur) % k == 0:
            print(cur)
            break
        cur += 1
```Hàm trợ giúp tính tổng các chữ số bằng cách sử dụng các phép chia modulo và chia lặp lại. Điều này tránh chuyển đổi số thành chuỗi, mặc dù cả hai phương pháp đều đủ nhanh. 

Đối với mỗi trường hợp thử nghiệm, chương trình bắt đầu lúc`x`và kiểm tra từng ứng viên một. Vòng lặp kết thúc ngay khi tổng chữ số chia hết cho`k`được tìm thấy. 

Một điều kiện biên tinh tế là khi`x`bản thân nó đã đẹp rồi. Thuật toán xử lý việc này một cách tự nhiên vì`cur`bắt đầu lúc`x`, vì vậy lần kiểm tra đầu tiên bao gồm số bắt đầu. 

Không có lo ngại về tràn vì số nguyên Python không bị giới hạn và các giá trị bài toán vốn đã rất nhỏ so với các tiêu chuẩn lập trình cạnh tranh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
x = 10, k = 8
```| Số hiện tại | Tổng chữ số | Tổng chữ số % 8 | Hành động | 
| --- | --- | --- | --- | 
| 10 | 1 | 1 | Tiếp tục | 
| 11 | 2 | 2 | Tiếp tục | 
| 12 | 3 | 3 | Tiếp tục | 
| 13 | 4 | 4 | Tiếp tục | 
| 14 | 5 | 5 | Tiếp tục | 
| 15 | 6 | 6 | Tiếp tục | 
| 16 | 7 | 7 | Tiếp tục | 
| 17 | 8 | 0 | Dừng lại | 

Đầu ra:```
17
```Ví dụ này cho thấy bản thân con số đó không có ý nghĩa gì. Chỉ có tổng chữ số quan trọng. 

### Ví dụ 2 

đầu vào:```
x = 1235, k = 10
```| Số hiện tại | Tổng chữ số | Tổng chữ số % 10 | Hành động | 
| --- | --- | --- | --- | 
| 1235 | 11 | 1 | Tiếp tục | 
| 1236 | 12 | 2 | Tiếp tục | 
| 1237 | 13 | 3 | Tiếp tục | 
| 1238 | 14 | 4 | Tiếp tục | 
| 1239 | 15 | 5 | Tiếp tục | 
| 1240 | 7 | 7 | Tiếp tục | 
| 1241 | 8 | 8 | Tiếp tục | 
| 1242 | 9 | 9 | Tiếp tục | 
| 1243 | 10 | 0 | Dừng lại | 

Đầu ra:```
1243
```Dấu vết này chứng tỏ tại sao việc kiểm tra các số một cách tuần tự là cần thiết. Hoạt động mang có thể thay đổi đáng kể tổng chữ số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(g · d) |`g`ứng viên đã được kiểm tra,`d`tính toán tổng chữ số | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Các số có nhiều nhất là mười chữ số, vì vậy`d`thực tế là không đổi. Khoảng cách tìm kiếm`g`cũng rất nhỏ vì`k ≤ 10`. Ngay cả với`10^4`trường hợp thử nghiệm, giải pháp dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    def digit_sum(n):
        s = 0
        while n:
            s += n % 10
            n //= 10
        return s

    t = int(input())
    ans = []

    for _ in range(t):
        x, k = map(int, input().split())
        cur = x

        while True:
            if digit_sum(cur) % k == 0:
                ans.append(str(cur))
                break
            cur += 1

    print("\n".join(ans))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue()

# provided sample
assert run(
"""6
1 5
10 8
37 9
777 3
1235 10
1 10
"""
) == """5
17
45
777
1243
19
"""

# minimum values
assert run(
"""1
1 1
"""
) == """1
"""

# already beautiful
assert run(
"""1
999999999 9
"""
) == """999999999
"""

# carry changes digit sum drastically
assert run(
"""1
19 10
"""
) == """19
"""

# near upper bound
assert run(
"""1
1000000000 10
"""
) == """1000000009
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Đầu vào nhỏ nhất có thể | 
|`999999999 9`|`999999999`| Câu trả lời có thể bằng giá trị bắt đầu | 
|`19 10`|`19`| Chia hết chính xác ở số bắt đầu | 
|`1000000000 10`|`1000000009`| Giá trị lớn và tìm kiếm lặp đi lặp lại | 

## Vỏ cạnh 

Hãy xem xét:```
1
777 3
```Thuật toán bắt đầu với`cur = 777`. Tổng các chữ số là`21`, Và`21 % 3 = 0`. Vòng lặp dừng ngay lập tức và xuất ra:```
777
```Điều này xác nhận rằng câu trả lời có thể chính xác`x`. 

Coi như:```
1
17 8
```Tổng các chữ số là`1 + 7 = 8`. Từ`8 % 8 = 0`, thuật toán đầu ra:```
17
```Điều này chứng tỏ rằng tính chất phụ thuộc vào tổng các chữ số chứ không phụ thuộc vào khả năng chia hết của chính số đó. 

Coi như:```
1
1 10
```Thuật toán kiểm tra các số một cách tuần tự:```
1, 2, 3, ..., 18, 19
```Tổng chữ số đầu tiên chia hết cho`10`xảy ra tại`19`, có tổng chữ số là`10`. Đầu ra là:```
19
```Trường hợp này cho thấy tại sao việc tìm kiếm phải tiếp tục cho đến khi tìm thấy tổng chữ số hợp lệ, ngay cả khi nhiều số liên tiếp không thành công.
