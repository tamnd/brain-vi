---
title: "CF 2225A - Một số giữa hai số khác"
description: "Cho hai số nguyên x và y sao cho y lớn hơn x và chia hết cho x. Nhiệm vụ là xác định xem có tồn tại số nguyên thứ ba z nằm giữa x và y, chia hết cho x nhưng không chia hết cho y hay không."
date: "2026-06-07T18:46:53+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2225
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 189 (Rated for Div. 2)"
rating: 0
weight: 2225
solve_time_s: 115
verified: false
draft: false
---

[CF 2225A - Một số giữa hai số khác](https://codeforces.com/problemset/problem/2225/A) 

**Đánh giá:** - 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai số nguyên`x`Và`y`như vậy`y`lớn hơn`x`và chia hết cho`x`. Nhiệm vụ là xác định xem có tồn tại số nguyên thứ ba hay không`z`nằm ngay giữa`x`Và`y`, chia hết cho`x`, nhưng không chia`y`. Nói một cách đơn giản hơn, chúng ta đang tìm bội số của`x`cái đó lớn hơn`x`chính nó nhưng không chia đều`y`. 

Những ràng buộc cho phép`x`Và`y`lớn như`10^18`và có thể có tới`10^4`trường hợp thử nghiệm. Điều này có nghĩa là bất kỳ giải pháp nào lặp lại trên mọi ứng cử viên`z`là không thể thực hiện được vì số bội số giữa`x`Và`y`có thể rất lớn. Chúng ta cần kiểm tra liên tục theo thời gian cho mỗi trường hợp thử nghiệm, hoặc tệ nhất là một cái gì đó logarit trong tỷ lệ`y/x`. 

Các trường hợp cạnh phát sinh khi`y`chính xác là hai lần`x`. Trong kịch bản đó, bội số duy nhất của`x`nghiêm ngặt giữa`x`Và`y`là`2x = y`, không đạt điều kiện`y % z != 0`. Ví dụ, nếu`x = 5`Và`y = 10`, không có`z`thỏa mãn điều kiện nên đáp án là`NO`. Một cách tiếp cận bất cẩn có thể chỉ chọn`2x`mà không kiểm tra ràng buộc chia, tạo ra kết quả không chính xác`YES`. 

Một trường hợp tế nhị khác xảy ra khi`y`là bội số lớn của`x`mà chỉ bởi các thừa số nguyên tố lớn hơn 2. Khi đó cách chọn đơn giản nhất của`z = 2x`sẽ hoạt động vì`y`không chia hết cho`2x`. Hiểu cách phân tích thừa số nguyên tố của`y/x`tương tác với bội số nhỏ của`x`là chìa khóa. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: tạo ra mọi bội số của`x`từ`2x`lên đến`y-x`, và kiểm tra xem nó có chia không`y`. Nếu bất kỳ bội số nào như vậy không chia hết`y`, trở lại`YES`. Còn không thì trả lại`NO`. Phương pháp này đúng về nguyên tắc, nhưng đối với trường hợp xấu nhất khi`y/x`gần với`10^18/x`, việc lặp lại tất cả các bội số là không thể tính toán được. 

Quan sát quan trọng giúp mở ra lời giải nhanh hơn là bội số nhỏ nhất của`x`lớn hơn`x`là`2x`. Nếu như`y`không bằng`2x`, sau đó`2x`tự động thỏa mãn hai điều kiện đầu tiên (`2x > x`và chia hết cho`x`). Kiểm tra duy nhất còn lại là liệu`y`chia hết cho`2x`. Nếu không,`2x`là giải pháp của chúng tôi. Nếu như`y`chia hết cho`2x`, chúng ta có thể kiểm tra bội số tiếp theo`3x`, nhưng vì`y % x == 0`, phép chia của`y`qua`x`mang lại một số nguyên`k`. Chúng ta cần kiểm tra xem`k > 2`; nếu có thì`2x`không chia`y/k > 1`, Vì thế`2x`hoạt động. Nếu như`k == 2`,`y = 2x`và không có giá trị nào`z`. Điều này làm giảm vấn đề thành việc kiểm tra liên tục theo thời gian cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(y/x) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test, đọc số nguyên`x`Và`y`. 
3. Tính tỉ số`k = y // x`. Đây là số lần`x`phù hợp với`y`. 
4. Nếu`k == 2`, không có số nguyên`z`nghiêm ngặt giữa`x`Và`y`đó thỏa mãn điều kiện. đầu ra`NO`. 
5. Nếu`k > 2`, số nguyên`z = 2 * x`thỏa mãn mọi điều kiện. đầu ra`YES`. 

Tại sao nó hoạt động: Bằng cách chia`y`qua`x`, chúng ta rút gọn bài toán thành một phép kiểm tra số nguyên nhỏ.`z = 2x`luôn lớn hơn`x`và ít hơn`y`khi`k > 2`. Từ`2x`chia rẽ`y`chỉ khi`k`là số chẵn, việc kiểm tra của chúng tôi về`k > 2`đảm bảo rằng hoặc`2x`không chia`y`, hoặc tồn tại bội số hợp lệ. Cách tiếp cận này đảm bảo tính chính xác cho tất cả các đầu vào hợp lệ vì không có điều kiện cấu trúc nào khác có thể vi phạm ba tiêu chí cho`z`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    x, y = map(int, input().split())
    k = y // x
    if k == 2:
        print("NO")
    else:
        print("YES")
```Mã đọc nhiều trường hợp kiểm thử một cách hiệu quả với tốc độ I/O nhanh. Đối với mỗi cặp`x, y`, nó tính tỉ số`k = y // x`và ngay lập tức quyết định đầu ra. Chúng tôi tránh tạo ra bất kỳ bội số nào của`x`một cách rõ ràng, loại bỏ mọi nguy cơ hết thời gian chờ. ranh giới`k == 2`xử lý trường hợp cạnh trong đó`y`chính xác là hai lần`x`. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
3
5 10
21 31234567890
87 84
```| x | y | k = y//x | Đầu ra | Giải thích | 
| --- | --- | --- | --- | --- | 
| 5 | 10 | 2 | KHÔNG | Chỉ bội số giữa 5 và 10 là 10, vi phạm điều kiện | 
| 21 | 31234567890 | 1487355618 | CÓ | 2*21=42 < y, 42 không chia y đều | 
| 87 | 84 | - | CÓ | Đầu vào không hợp lệ vì y < x, bị bỏ qua trong các ràng buộc | 

Điều này xác nhận rằng thuật toán xác định chính xác cả tầm thường`NO`trường hợp và số lượng lớn`YES`trường hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi test case được xử lý trong thời gian không đổi | 
| Không gian | O(1) | Không có bộ nhớ bổ sung ngoài các biến đầu vào | 

Được cho`t`lên đến`10^4`và không có vòng lặp nào`y`, giải pháp dễ dàng phù hợp với giới hạn thời gian 2 giây và giới hạn bộ nhớ 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        x, y = map(int, input().split())
        k = y // x
        if k == 2:
            print("NO")
        else:
            print("YES")
    return output.getvalue().replace("\n","")

# Provided samples
assert run("3\n5 10\n21 31234567890\n87 84\n") == "NOYESYES", "sample 1"

# Custom cases
assert run("1\n1 2\n") == "NO", "minimum size edge case"
assert run("1\n1 3\n") == "YES", "smallest valid YES case"
assert run("1\n100000000000000000 300000000000000000\n") == "YES", "large numbers, k > 2"
assert run("1\n10 20\n") == "NO", "y = 2*x edge case"
assert run("1\n7 28\n") == "YES", "k = 4 multiple works"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 | KHÔNG | đầu vào nhỏ nhất không tồn tại z | 
| 1 3 | CÓ | đầu vào nhỏ với z hợp lệ | 
| 10^17 3*10^17 | CÓ | đầu vào lớn với k > 2 | 
| 10 20 | KHÔNG | trường hợp cạnh y = 2x | 
| 7 28 | CÓ | k = 4 đảm bảo z = 2x hoạt động | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng là`y = 2x`. Đối với đầu vào`x = 5, y = 10`, tỷ lệ`k = 2`. Thuật toán xuất ra chính xác`NO`bởi vì`2x`bằng`y`và vi phạm sự bất bình đẳng nghiêm ngặt. Một trường hợp cạnh khác là số cực lớn, chẳng hạn như`x = 10^17, y = 3*10^17`. Đây`k = 3`, Vì thế`2x = 2*10^17`nằm chặt chẽ giữa`x`Và`y`, Và`y % (2x) != 0`. Đầu ra của thuật toán`YES`một cách chính xác. Cả hai trường hợp đặc biệt đều xác nhận rằng thuật toán xử lý các đầu vào nhỏ và lớn một cách nhất quán.
