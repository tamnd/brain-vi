---
title: "CF 1411A - Trò chuyện trong trò chơi"
description: "Chúng tôi nhận được một số tin nhắn trò chuyện. Mỗi tin nhắn là một chuỗi chứa các chữ cái tiếng Anh viết thường và ký tự )."
date: "2026-06-11T07:28:47+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1411
codeforces_index: "A"
codeforces_contest_name: "Technocup 2021 - Elimination Round 3"
rating: 800
weight: 1411
solve_time_s: 83
verified: true
draft: false
---

[CF 1411A - Trò chuyện trong trò chơi](https://codeforces.com/problemset/problem/1411/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi nhận được một số tin nhắn trò chuyện. Mỗi tin nhắn là một chuỗi chứa các chữ cái tiếng Anh viết thường và ký tự`)`. 

Một tin nhắn được coi là xấu nếu số lượng liên tiếp`)`các ký tự ở cuối chuỗi hoàn toàn lớn hơn số lượng tất cả các ký tự còn lại trước chúng. 

Ví dụ, trong chuỗi`ab))))`, có 4 dấu ngoặc đơn đóng ở cuối và 2 ký tự khác trước chúng. Từ`4 > 2`, tin nhắn xấu. 

Đối với mỗi trường hợp thử nghiệm, chúng ta phải xác định xem thông báo có sai hay không và in ra`"Yes"`hoặc`"No"`. 

Những hạn chế là cực kỳ nhỏ. Mỗi chuỗi có độ dài tối đa là 100 và có tối đa 100 trường hợp kiểm thử. Ngay cả một thuật toán quét mọi ký tự nhiều lần cũng đủ nhanh. Mục tiêu không phải là tối ưu hóa mà là xác định chính xác hậu tố của dấu ngoặc đơn đóng và so sánh kích thước của nó với phần còn lại của chuỗi. 

Một sai lầm dễ mắc phải là đếm tất cả`)`các ký tự trong chuỗi thay vì chỉ các ký tự liên tiếp ở cuối. 

Coi như:```
7
a)b)c))
```Hậu tố liên tiếp`)`ký tự có độ dài 2. Phần còn lại có độ dài 5. Vì`2 > 5`là sai, câu trả lời là`"No"`. 

Một giải pháp có tính đến mọi`)`sẽ nhận được 4 và kết luận sai rằng tin nhắn đó là xấu. 

Một trường hợp cạnh khác là khi toàn bộ chuỗi bao gồm các dấu ngoặc đơn đóng.```
3
)))
```Độ dài hậu tố là 3 và độ dài còn lại là 0. Vì`3 > 0`, câu trả lời là`"Yes"`. 

Trường hợp tế nhị thứ ba là sự bình đẳng.```
6
abc)))
```Độ dài hậu tố là 3 và độ dài còn lại cũng là 3. Định nghĩa yêu cầu số đếm lớn hơn, vì vậy câu trả lời đúng là`"No"`. 

Việc thực hiện bất cẩn bằng cách sử dụng`>=`sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Giải pháp brute-force trước tiên sẽ xác định mọi hậu tố có thể có của chuỗi, kiểm tra xem nó có bao gồm toàn bộ`)`ký tự rồi tính độ dài của nó. Vì độ dài chuỗi tối đa là 100 nên ngay cả một phương pháp kém hiệu quả như vậy vẫn có thể chạy thoải mái trong giới hạn. 

Cấu trúc vấn đề đưa ra một quan sát đơn giản hơn nhiều. Chỉ có khối liên tiếp cuối cùng của`)`vấn đề. Mọi thứ trước khối đó thuộc về số lượng "ký tự còn lại". 

Thay vì kiểm tra nhiều hậu tố, chúng ta có thể bắt đầu từ cuối chuỗi và đếm xem có bao nhiêu hậu tố liên tiếp.`)`các ký tự xuất hiện. Khoảnh khắc chúng tôi gặp bất kỳ nhân vật nào khác, chúng tôi dừng lại. Số lượng đó chính xác là kích thước hậu tố được yêu cầu bởi câu lệnh. 

Nếu hậu tố chứa`cnt`trong ngoặc đơn thì phần còn lại chứa`n - cnt`nhân vật. Tin nhắn xấu chính xác khi:```
cnt > n - cnt
```Phương pháp brute-force hoạt động vì cuối cùng nó tìm thấy khối kết thúc, nhưng quét ngược một lần sẽ cung cấp thông tin tương tự một cách trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc độ dài`n`và chuỗi`s`. 
2. Bắt đầu từ ký tự cuối cùng của chuỗi và di chuyển sang trái. 
3. Đếm xem có bao nhiêu liên tiếp`)`ký tự xuất hiện ở cuối. Lưu số đếm này vào`cnt`. 
4. Dừng lại ngay khi có ký tự khác với`)`gặp phải, bởi vì chỉ có khối liên tiếp cuối cùng mới quan trọng. 
5. Tính số ký tự còn lại là`n - cnt`. 
6. Nếu`cnt > n - cnt`, in`"Yes"`. 
7. Nếu không, hãy in`"No"`. 

### Tại sao nó hoạt động 

Định nghĩa của một thông báo xấu chỉ phụ thuộc vào dấu ngoặc đơn đóng liên tiếp ở cuối chuỗi. Bằng cách quét ngược, chúng tôi tính toán chính xác số lượng đó. 

Mọi ký tự không thuộc khối kết thúc này đều là một phần của phần còn lại của chuỗi. Như vậy`n - cnt`chính xác là số ký tự còn lại được đề cập trong câu lệnh. 

Thuật toán kiểm tra trực tiếp điều kiện từ định nghĩa, vì vậy câu trả lời của nó luôn đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        s = input().strip()

        cnt = 0
        i = n - 1

        while i >= 0 and s[i] == ')':
            cnt += 1
            i -= 1

        if cnt > n - cnt:
            print("Yes")
        else:
            print("No")

solve()
```Việc thực hiện tuân theo thuật toán một cách chính xác. 

Biến`cnt`lưu trữ độ dài của hậu tố bao gồm liên tiếp`)`nhân vật. Vòng lặp bắt đầu ở chỉ mục cuối cùng và tiếp tục di chuyển sang trái trong khi ký tự hiện tại`)`. 

Khi một ký tự khác xuất hiện, hậu tố đã kết thúc và không cần phải tiếp tục quét. Số lượng ký tự còn lại chỉ đơn giản là`n - cnt`. 

Sự so sánh cuối cùng sử dụng`>`còn hơn là`>=`. Chi tiết này rất quan trọng vì số lượng bằng nhau không làm cho thông điệp trở nên tồi tệ. 

Vì độ dài chuỗi rất nhỏ nên các vấn đề về tràn số nguyên và bộ nhớ không tồn tại ở đây. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chuỗi đầu vào:```
gege)))))
```| Bước | Vị trí | Nhân vật | cnt | 
| --- | --- | --- | --- | 
| Bắt đầu | 8 | ) | 0 | 
| 1 | 8 | ) | 1 | 
| 2 | 7 | ) | 2 | 
| 3 | 6 | ) | 3 | 
| 4 | 5 | ) | 4 | 
| 5 | 4 | ) | 5 | 
| Dừng lại | 3 | e | 5 | 

Sau khi quét: 

| Giá trị | Kết quả | 
| --- | --- | 
| n | 9 | 
| cnt | 5 | 
| n - cnt | 4 | 
| Tình trạng | 5 > 4 | 

Đầu ra:```
Yes
```Ví dụ này hiển thị hậu tố dài hơn tiền tố còn lại, do đó thông báo không hợp lệ. 

### Ví dụ 2 

Chuỗi đầu vào:```
gl))hf))))))
```| Bước | Vị trí | Nhân vật | cnt | 
| --- | --- | --- | --- | 
| Bắt đầu | 11 | ) | 0 | 
| 1 | 11 | ) | 1 | 
| 2 | 10 | ) | 2 | 
| 3 | 9 | ) | 3 | 
| 4 | 8 | ) | 4 | 
| 5 | 7 | ) | 5 | 
| 6 | 6 | ) | 6 | 
| Dừng lại | 5 | f | 6 | 

Sau khi quét: 

| Giá trị | Kết quả | 
| --- | --- | 
| n | 12 | 
| cnt | 6 | 
| n - cnt | 6 | 
| Tình trạng | 6 > 6 | 

Đầu ra:```
No
```Dấu vết này thể hiện sự so sánh chặt chẽ. Bình đẳng là chưa đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được kiểm tra tối đa một lần trong khi quét từ cuối | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Với`n ≤ 100`và tối đa 100 trường hợp thử nghiệm, giải pháp thực hiện tối đa vài nghìn lần kiểm tra ký tự. Nó dễ dàng phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    out = []

    t = int(input())

    for _ in range(t):
        n = int(input())
        s = input().strip()

        cnt = 0
        i = n - 1

        while i >= 0 and s[i] == ')':
            cnt += 1
            i -= 1

        out.append("Yes" if cnt > n - cnt else "No")

    return "\n".join(out)

# provided sample
assert run(
"""5
2
))
12
gl))hf))))))
9
gege)))))
14
)aa))b))))))))
1
)
"""
) == "\n".join([
    "Yes",
    "No",
    "Yes",
    "Yes",
    "Yes"
]), "sample 1"

# minimum size
assert run(
"""1
1
)
"""
) == "Yes", "single parenthesis"

# equality boundary
assert run(
"""1
6
abc)))
"""
) == "No", "suffix equals remaining length"

# no ending dominance
assert run(
"""1
7
a)b)c))
"""
) == "No", "only ending block counts"

# all parentheses
assert run(
"""1
10
))))))))))
"""
) == "Yes", "entire string is suffix"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`")"`|`Yes`| Đầu vào hợp lệ nhỏ nhất | 
|`"abc)))"`|`No`| Bình đẳng thôi chưa đủ | 
|`"a)b)c))"`|`No`| Chỉ có dấu ngoặc đơn ở cuối mới quan trọng | 
|`"()))))))))"`(tất cả các dấu ngoặc đơn) |`Yes`| Toàn bộ chuỗi có thể là hậu tố | 

## Vỏ cạnh 

### Bình đẳng giữa cả hai phần 

đầu vào:```
1
6
abc)))
```Số lần quét ngược`cnt = 3`. Các ký tự còn lại là`6 - 3 = 3`. 

Sự so sánh trở thành:```
3 > 3
```đó là sai, vì vậy đầu ra là:```
No
```Điều này xác nhận rằng điều kiện là hoàn toàn lớn hơn. 

### Toàn bộ chuỗi bao gồm dấu ngoặc đơn 

đầu vào:```
1
3
)))
```Quá trình quét đến đầu chuỗi và đếm`cnt = 3`. 

Phần còn lại có:```
3 - 3 = 0
```Từ:```
3 > 0
```đầu ra là:```
Yes
```Thuật toán xử lý việc này một cách tự nhiên mà không có bất kỳ trường hợp đặc biệt nào. 

### Dấu ngoặc đơn xuất hiện sớm hơn trong chuỗi 

đầu vào:```
1
7
a)b)c))
```Quét từ cuối chỉ tính khối cuối cùng:```
cnt = 2
```Các dấu ngoặc đơn trước đó bị bỏ qua vì chúng không phải là một phần của hậu tố kết thúc. 

Độ dài còn lại là:```
7 - 2 = 5
```Từ:```
2 > 5
```là sai, câu trả lời là:```
No
```Điều này xác minh rằng thuật toán tuân theo định nghĩa chính xác của vấn đề thay vì đếm tất cả các dấu ngoặc đơn trong chuỗi.
