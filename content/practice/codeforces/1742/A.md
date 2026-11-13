---
title: "CF 1742A - Tổng"
description: "Đối với mỗi trường hợp thử nghiệm, chúng tôi nhận được ba số nguyên. Nhiệm vụ là xác định xem liệu một trong ba số này có thể được biểu diễn dưới dạng tổng của hai số còn lại hay không."
date: "2026-06-09T16:12:51+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1742
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 827 (Div. 4)"
rating: 800
weight: 1742
solve_time_s: 119
verified: true
draft: false
---

[CF 1742A - Tổng](https://codeforces.com/problemset/problem/1742/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đối với mỗi trường hợp thử nghiệm, chúng tôi nhận được ba số nguyên. Nhiệm vụ là xác định xem liệu một trong ba số này có thể được biểu diễn dưới dạng tổng của hai số còn lại hay không. 

Vì chỉ có ba giá trị nên có chính xác ba cách để điều kiện có thể đúng: 

Nếu`a + b = c`, hoặc`a + c = b`, hoặc`b + c = a`. 

Những hạn chế là cực kỳ nhỏ. Ngay cả số lượng ca kiểm thử tối đa cũng chỉ là 9261 và mỗi ca kiểm thử chỉ chứa ba số nguyên từ 0 đến 20. Điều này có nghĩa là chúng ta có đủ khả năng để kiểm tra trực tiếp mọi mối quan hệ có thể có. Tổng khối lượng công việc rất nhỏ, chỉ một vài phép tính số học cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh tế xuất hiện khi có liên quan đến số 0. 

Coi như:```
0 0 0
```Câu trả lời đúng là`YES`bởi vì`0 + 0 = 0`. Một giải pháp bất cẩn cho rằng tổng phải lớn hơn số cộng sẽ trả về không chính xác`NO`. 

Một trường hợp cạnh khác xảy ra khi tất cả các số đều bằng nhau nhưng khác 0.```
20 20 20
```Câu trả lời đúng là`NO`. Không có số nào bằng tổng của hai số còn lại vì`20 + 20 = 40`. Một giải pháp bất cẩn chỉ kiểm tra xem hai giá trị có bằng nhau hay không có thể trả về nhầm`YES`. 

Một trường hợp nữa là khi số lớn nhất không được đưa ra ở một vị trí cụ thể.```
15 7 8
```Câu trả lời đúng là`YES`bởi vì`7 + 8 = 15`. Một giải pháp chỉ kiểm tra`a + b == c`sẽ bỏ lỡ cấu hình hợp lệ này. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ trực tiếp nhất là kiểm tra mọi lựa chọn có thể có của một con số mục tiêu. Vì chỉ có ba số nên chúng ta có thể kiểm tra xem số thứ nhất có bằng tổng của hai số còn lại hay không, số thứ hai có bằng tổng của hai số còn lại và số thứ ba có bằng tổng của hai số còn lại hay không. 

Cách tiếp cận này đúng vì ba phép kiểm tra này bao hàm chính xác mọi cách mà một số có thể là tổng của hai số còn lại. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi thực hiện ba phép cộng và ba phép so sánh. Ngay cả ở mức tối đa 9261 trường hợp thử nghiệm, đây cũng chỉ là vài chục nghìn thao tác, điều này không đáng kể. 

Quan sát quan trọng là bản thân vấn đề chỉ có ba trường hợp có thể xảy ra. Không cần phải sắp xếp, tìm kiếm hoặc bất kỳ kỹ thuật phức tạp nào. Toàn bộ nhiệm vụ giảm xuống còn việc đánh giá trực tiếp ba phương trình hợp lệ. 

Các giải pháp mạnh mẽ và tối ưu ở đây thực sự giống nhau vì không gian tìm kiếm là không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi test case, hãy đọc`a`,`b`, Và`c`. 
3. Kiểm tra xem`a + b == c`. 

Nếu điều này đúng thì một số là tổng của hai số còn lại, vậy câu trả lời là`YES`. 
4. Kiểm tra xem`a + c == b`. 

Điều này bao gồm trường hợp số thứ hai là tổng. 
5. Kiểm tra xem`b + c == a`. 

Điều này bao gồm khả năng còn lại trong đó số đầu tiên là tổng. 
6. Nếu ít nhất một trong ba kiểm tra là đúng, hãy in`YES`. Nếu không thì in`NO`. 

### Tại sao nó hoạt động 

Chính xác một trong ba số chỉ có thể là tổng của hai số còn lại ở một trong ba phương trình:`a + b = c`,`a + c = b`, hoặc`b + c = a`. 

Thuật toán kiểm tra cả ba khả năng. Nếu tồn tại bất kỳ mối quan hệ hợp lệ nào, một trong những so sánh này thành công và thuật toán sẽ in ra`YES`. Nếu cả ba phép so sánh đều thất bại thì không có số nào bằng tổng của hai số còn lại, vì vậy câu trả lời đúng là`NO`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    a, b, c = map(int, input().split())

    if a + b == c or a + c == b or b + c == a:
        print("YES")
    else:
        print("NO")
```Dòng đầu tiên ghi số lượng test case. Mỗi lần lặp đọc ba số nguyên. 

Tình trạng bên trong`if`câu lệnh trực tiếp thực hiện ba phương trình được thảo luận trong thuật toán. Sử dụng logic OR có nghĩa là câu trả lời trở thành`YES`ngay khi tìm thấy bất kỳ mối quan hệ tổng hợp lệ nào. 

Không có mối quan tâm nào về điều kiện biên ngoài việc kiểm tra cả ba khả năng. Các con số rất nhỏ nên việc tràn số nguyên là không thể trong Python. Thứ tự kiểm tra không quan trọng vì mọi cấu hình hợp lệ đều được kiểm tra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
1 4 3
```| một | b | c | a+b==c | a+c==b | b+c==a | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 3 | Sai | Đúng | Sai | CÓ | 

Đây,`1 + 3 = 4`, vậy là điều kiện thứ hai thành công. Ví dụ này cho thấy tại sao chúng ta phải kiểm tra cả ba phương trình thay vì chỉ`a + b == c`. 

### Ví dụ 2 

đầu vào:```
1
20 20 20
```| một | b | c | a+b==c | a+c==b | b+c==a | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 20 | 20 | 20 | Sai | Sai | Sai | KHÔNG | 

Tất cả các số đều bằng nhau, nhưng không có số nào bằng tổng của hai số còn lại. Điều này chứng tỏ rằng chỉ bình đẳng thôi là chưa đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Ba so sánh cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Công việc được thực hiện cho mỗi trường hợp kiểm thử là không đổi và độc lập với các giá trị đầu vào. Ngay cả ở số lượng trường hợp thử nghiệm tối đa, thời gian chạy vẫn thấp hơn nhiều so với giới hạn và mức sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        a, b, c = map(int, input().split())

        if a + b == c or a + c == b or b + c == a:
            out.append("YES")
        else:
            out.append("NO")

    return "\n".join(out)

# provided sample
assert run(
"""7
1 4 3
2 5 8
9 11 20
0 0 0
20 20 20
4 12 3
15 7 8
"""
) == """YES
NO
YES
YES
NO
NO
YES""", "sample 1"

# minimum values
assert run(
"""1
0 0 0
"""
) == "YES", "all zeros"

# maximum values
assert run(
"""1
20 20 20
"""
) == "NO", "all maximum values"

# largest value is first
assert run(
"""1
15 7 8
"""
) == "YES", "must check all permutations"

# simple negative case
assert run(
"""1
4 12 3
"""
) == "NO", "no valid sum relation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 0`|`YES`| Giá trị 0 vẫn thỏa mãn điều kiện | 
|`20 20 20`|`NO`| Các giá trị bằng nhau không tự động hoạt động | 
|`15 7 8`|`YES`| Giá trị tổng hợp có thể xuất hiện ở bất kỳ vị trí nào | 
|`4 12 3`|`NO`| Sửa lỗi từ chối khi không có phương trình nào đúng | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1
0 0 0
```Thuật toán đánh giá:`0 + 0 == 0`, đó là sự thật. 

Vì một trong các lần kiểm tra thành công, nó sẽ in`YES`. Điều này xử lý chính xác trường hợp đặc biệt trong đó tất cả các giá trị bằng 0. 

Hãy xem xét đầu vào:```
1
20 20 20
```Thuật toán đánh giá:`20 + 20 == 20`→ sai`20 + 20 == 20`→ sai`20 + 20 == 20`→ sai 

Tất cả các kiểm tra đều thất bại, vì vậy nó in`NO`. Điều này xử lý chính xác các giá trị khác không bằng nhau. 

Hãy xem xét đầu vào:```
1
15 7 8
```Thuật toán đánh giá:`15 + 7 == 8`→ sai`15 + 8 == 7`→ sai`7 + 8 == 15`→ đúng 

Điều kiện thứ ba thành công nên câu trả lời là`YES`. Điều này cho thấy tại sao chỉ kiểm tra một đơn hàng sẽ không chính xác.
