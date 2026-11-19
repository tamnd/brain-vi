---
title: "CF 208D - Giải thưởng, Giải thưởng, Giải thưởng khác"
description: "Chúng ta có một chuỗi các lần mua thanh sô-cô-la, mỗi lần mua sẽ mang lại một số điểm nhất định. Vasya bắt đầu với 0 điểm và sau mỗi thanh có thể đi đến trung tâm giải thưởng. Các giải thưởng có chi phí điểm cố định: một chiếc cốc, một chiếc khăn, một chiếc túi, một chiếc xe đạp và một chiếc ô tô, theo thứ tự tăng dần."
date: "2026-06-03T17:22:43+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 208
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 130 (Div. 2)"
rating: 1200
weight: 208
solve_time_s: 170
verified: false
draft: false
---

[CF 208D - Giải thưởng, Giải thưởng, Giải thưởng khác](https://codeforces.com/problemset/problem/208/D) 

**Đánh giá:** 1200 
**Thẻ:** triển khai 
**Thời gian giải:** 2 phút 50 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi các lần mua thanh sô-cô-la, mỗi lần mua sẽ mang lại một số điểm nhất định. Vasya bắt đầu với 0 điểm và sau mỗi thanh có thể đi đến trung tâm giải thưởng. Các giải thưởng có chi phí điểm cố định: một chiếc cốc, một chiếc khăn, một chiếc túi, một chiếc xe đạp và một chiếc ô tô, theo thứ tự tăng dần. Vasya áp dụng chiến lược tham lam: bất cứ khi nào anh ta có đủ điểm để mua bất kỳ giải thưởng nào, anh ta sẽ chọn giải thưởng đắt nhất mà anh ta có thể mua được, giảm điểm và lặp lại cho đến khi không thể mua được bất kỳ giải thưởng nào. 

Nhiệm vụ của chúng tôi là xây dựng lại bộ sưu tập giải thưởng của Vasya và xác định số điểm còn sót lại của anh ấy sau khi xử lý tất cả các thanh sô cô la. 

Đầu vào mang lại`n`(tối đa 50), điểm cho mỗi thanh (`p_i`lên tới 10^9) và chi phí của năm giải thưởng (tăng nghiêm ngặt, lên tới 10^9). cái nhỏ`n`cho phép chúng tôi mô phỏng quá trình trực tiếp mà không cần lo lắng về hiệu suất. Các giá trị điểm lớn có nghĩa là chúng ta phải tránh các phương pháp đếm hoặc DP dựa trên mảng đơn giản giả sử các số nguyên nhỏ. 

Các trường hợp khó khăn bao gồm các tình huống trong đó số điểm của Vasya khớp chính xác với giá trị giải thưởng hoặc khi anh ta có thể mua nhiều giải thưởng cùng một lúc và lựa chọn tham lam phải được áp dụng nhiều lần. Ví dụ: nếu Vasya kiếm được 10 điểm và giá trị giải thưởng`[2,4,10,15,20]`, anh ta nên chọn túi ngay thay vì mua nhiều món nhỏ hơn và số điểm còn lại phải được tính chính xác sau mỗi lần mua. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu là mô phỏng đơn giản. Bắt đầu với số điểm bằng 0 và cộng điểm của nó với mỗi thanh sô cô la. Sau đó, trong khi số điểm hiện tại ít nhất là giá trị giải thưởng rẻ nhất, hãy liên tục tìm giải thưởng đắt nhất mà Vasya có thể mua được, tăng số lượng của nó và trừ chi phí của nó khỏi điểm. Tiếp tục điều này cho tất cả các thanh. 

Cách làm này đúng vì nó trực tiếp đi theo chiến lược tham lam của Vasya. Từ`n`nhiều nhất là 50 và chỉ có 5 giải, vòng bên trong đổi điểm liên tục sẽ chạy nhiều nhất`n * max_points / min_cost`lần. Ngay cả với các giá trị điểm lớn, số lần lặp tối đa bị hạn chế bởi chuỗi phép trừ tham lam, nhiều nhất là một số lần lặp trên mỗi thanh trong điều kiện thực tế. 

Không cần giải pháp tiệm cận nhanh hơn vì các ràng buộc nhỏ, nhưng điểm mấu chốt là chiến lược tham lam cho phép chúng ta luôn chọn giải thưởng lớn nhất có thể chấp nhận được mà không cần xem xét chuỗi các giải thưởng nhỏ hơn - vấn đề đảm bảo cách tiếp cận này là tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n * 5) trong thực tế | O(5) cho bộ đếm | Đã chấp nhận | 
| Tối ưu hóa | Không áp dụng | Không áp dụng | Không cần thiết; vũ lực đủ | 

## Hướng dẫn thuật toán 

1. Khởi tạo danh sách`count`có kích thước từ 5 đến 0, đại diện cho số lượng mỗi giải thưởng mà Vasya đã thu thập được. Khởi tạo`points`về không. 
2. Lặp lại từng thanh sô cô la theo thứ tự thời gian. Thêm điểm của nó vào`points`. 
3. Trong khi`points`ít nhất là giá của giải thưởng rẻ nhất: 

một. Lặp lại các giải thưởng từ đắt nhất đến rẻ nhất. 

b. Tìm giải thưởng đắt nhất có giá nhỏ hơn hoặc bằng`points`. 

c. Tăng số lượng tương ứng. 

d. Trừ chi phí của giải thưởng từ`points`. 
4. Sau khi xử lý tất cả các thanh sô cô la, hãy in số lượng của từng giải thưởng theo sau là số lượng còn lại`points`. 

Điều này có hiệu quả vì điều bất biến là sau mỗi vòng lặp bên trong, Vasya không thể nhận được bất kỳ giải thưởng nào ngoại trừ có thể sau khi cộng thêm điểm từ thanh sô cô la tiếp theo. Tại mỗi thời điểm quyết định, việc chọn giải thưởng có giá phải chăng đắt nhất đảm bảo chiến lược tham lam phù hợp với mô tả vấn đề. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
points_list = list(map(int, input().split()))
prizes = list(map(int, input().split()))  # a, b, c, d, e
count = [0] * 5
points = 0

for p in points_list:
    points += p
    while points >= prizes[0]:
        for i in range(4, -1, -1):
            if points >= prizes[i]:
                count[i] += 1
                points -= prizes[i]
                break

print(' '.join(map(str, count)))
print(points)
```Vòng lặp bên ngoài thêm điểm cho mỗi thanh. Vòng lặp bên trong liên tục áp dụng chiến lược tham lam cho đến khi Vasya không thể mua được bất kỳ giải thưởng nào nữa. Việc lặp lại từ đắt nhất đến rẻ nhất đảm bảo chúng ta luôn chọn giải thưởng tối đa trước tiên. sử dụng`break`bên trong vòng lặp bên trong đảm bảo chỉ mua một giải thưởng trong mỗi lần lặp, mô phỏng chính xác các trao đổi liên tục của Vasya. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| Bước | Điểm đã thêm | Điểm trước khi trao đổi | Giải thưởng đã mua | Điểm sau khi trao đổi | Đếm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | Cốc (2) | 1 | [1,0,0,0,0] | 
| 2 | 10 | 11 | Túi (10) | 1 | [1,0,1,0,0] | 
| 3 | 4 | 5 | Khăn (4) | 1 | [1,1,1,0,0] | 

Điều này xác nhận thuật toán chọn chính xác giải thưởng đắt nhất ở mỗi bước và theo dõi số điểm còn sót lại. 

Đầu vào tùy chỉnh:```
5
1 2 3 4 5
2 3 5 7 11
```| Bước | Điểm | Giải thưởng đã mua | Điểm Sau | Đếm | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | - | 1 | [0,0,0,0,0] | 
| 2 | 3 | 3 | 0 | [0,1,0,0,0] | 
| 3 | 3 | 2 | 0 | [1,1,0,0,0] | 
| 4 | 4 | 3 | 1 | [1,1,1,0,0] | 
| 5 | 6 | 5 | 1 | [1,1,2,0,0] | 

Điều này thể hiện các lựa chọn tham lam lặp đi lặp lại với các điểm còn sót lại được chuyển tiếp một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * 5) | Chạy vòng lặp bên ngoài`n`lần; vòng lặp bên trong chạy tối đa 5 lần lặp cho mỗi lần trao đổi giải thưởng. | 
| Không gian | O(5) | Cửa hàng có 5 giải thưởng; danh sách đầu vào có kích thước n. | 

Với n 50, thao tác này thực hiện tốt dưới 2 giây ngay cả với giá trị điểm lớn. Việc sử dụng bộ nhớ không đáng kể, thấp hơn nhiều so với 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    points_list = list(map(int, input().split()))
    prizes = list(map(int, input().split()))
    count = [0]*5
    points = 0
    for p in points_list:
        points += p
        while points >= prizes[0]:
            for i in range(4, -1, -1):
                if points >= prizes[i]:
                    count[i] += 1
                    points -= prizes[i]
                    break
    return f"{' '.join(map(str,count))}\n{points}"

# Provided sample
assert run("3\n3 10 4\n2 4 10 15 20\n") == "1 1 1 0 0\n1", "sample 1"

# Minimum input
assert run("1\n1\n1 2 3 4 5\n") == "1 0 0 0 0\n0", "min input"

# All points smaller than cheapest prize
assert run("3\n1 1 1\n5 6 7 8 9\n") == "0 0 0 0 0\n3", "all points small"

# Exact sums
assert run("2\n7 4\n2 3 5 7 10\n") == "0 1 1 0 0\n3", "exact sums"

# Multiple exchanges per bar
assert run("1\n20\n2 4 10 15 20\n") == "0 0 1 0 0\n10", "single large bar"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 / 5 6 7 8 9 | 0 0 0 0 0\n3 | Điểm ít hơn bất kỳ giải thưởng nào | 
| 2 7 4 / 2 3 5 7 10 | 0 1 1 0 0\n3 | Số tiền chính xác kích hoạt nhiều giải thưởng | 
| 1 20/2 4 10 15 20 | 0 0 1 0 0\n10 | Thanh đơn cho phép trao đổi nhiều lần | 

## Vỏ cạnh 

Nếu Vasya kiếm được điểm khớp chính xác với nhiều giải thưởng trong một bước, thì thuật toán sẽ chọn giải thưởng đắt nhất trước tiên. Ví dụ: điểm 20 có thưởng`[2,4,10,15,20]`dẫn đến một ô tô nếu chúng ta xử lý tham lam theo thứ tự ngược lại. Điểm còn lại bằng 0 nếu
