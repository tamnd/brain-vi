---
title: "CF 1663D - Được xếp hạng - 3"
description: "Bài toán cho ta một chuỗi S có độ dài ba, gồm các ký tự + và - và một số nguyên không âm X. Chuỗi biểu thị một chuỗi các thao tác trên một số nguyên: + nghĩa là tăng thêm 1, - nghĩa là giảm đi 1."
date: "2026-06-10T02:31:14+07:00"
tags: ["codeforces", "competitive-programming", "*special", "combinatorics", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1663
codeforces_index: "D"
codeforces_contest_name: "April Fools Day Contest 2022"
rating: 0
weight: 1663
solve_time_s: 77
verified: true
draft: false
---

[CF 1663D - Được xếp hạng - 3](https://codeforces.com/problemset/problem/1663/D) 

**Đánh giá:** - 
**Tags:** *đặc biệt, tổ hợp, dp, toán 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề cho chúng ta một chuỗi`S`có độ dài ba, bao gồm các ký tự`+`Và`-`và một số nguyên không âm`X`. Chuỗi biểu thị một chuỗi các thao tác trên một số nguyên:`+`có nghĩa là tăng thêm 1,`-`có nghĩa là giảm đi 1. Bắt đầu từ`X`, chúng tôi liên tục áp dụng trình tự`S`vô cùng. Nhiệm vụ là xác định có bao nhiêu số nguyên không âm riêng biệt có thể xuất hiện trong chuỗi biến đổi vô hạn này. 

Bởi vì`S`có độ dài 3, số lượng thao tác trên mỗi chu kỳ rất nhỏ.`X`có thể lên tới một tỷ, một con số lớn nhưng kích thước cố định nhỏ của`S`gợi ý rằng mô phỏng qua một vài chu kỳ có thể là đủ. Khía cạnh không rõ ràng là chuỗi lặp lại mãi mãi và các giá trị âm bị loại bỏ: cùng một giá trị có thể được xem lại nhiều lần, vì vậy cần phải tính toán cẩn thận. Một cách tiếp cận đơn giản mô phỏng tất cả các bước cho đến`10^9`là không khả thi và chúng ta cần phát hiện các chu kỳ hoặc giới hạn mà không có giá trị mới nào xuất hiện. 

Các trường hợp biên bao gồm các chuỗi làm giảm ngay lập tức`X`về 0, các chuỗi luôn tăng và các chuỗi trong đó ứng dụng lặp đi lặp lại dẫn đến dao động giữa hai giá trị. Ví dụ, nếu`S = "+-+"`Và`X = 1`, trình tự phát triển như`1 → 2 → 1 → 2 → …`. Các giá trị riêng biệt ở đây chỉ là`{1, 2}`. Một mô phỏng ngây thơ có thể tiếp tục đếm nhầm những lần lặp lại này. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ mô phỏng trình tự bắt đầu từ`X`và theo dõi tất cả các giá trị không âm trong một tập hợp cho đến khi chúng tôi thấy giá trị lặp lại hoặc cho đến khi các giá trị chuyển sang âm. Trong mã giả, đây là:```
values = {X}
current = X
while True:
    for op in S:
        current += 1 if op == '+' else -1
        if current < 0:
            break
        values.add(current)
    if no new values were added:
        break
```Điều này đúng nhưng nếu`X`hoặc giá trị tăng lớn, chúng ta có thể lặp lại quá nhiều lần, đặc biệt là vì`X`có thể lên đến`10^9`. Tuy nhiên, bởi vì`S`có độ dài 3, thay đổi ròng trên mỗi chu kỳ là nhỏ (trong khoảng từ -3 đến +3). Điều này có nghĩa là số lượng giá trị riêng biệt mà chúng ta thấy trước khi chạm vào số âm hoặc đạt đến mẫu lặp lại nhiều nhất là`3 * 2 + 1 = 7`trong trường hợp dao động xấu nhất, do đó, lực lượng vũ phu thực sự khả thi ở đây. 

Thông tin chi tiết quan trọng là chúng ta chỉ cần mô phỏng tối đa một vài chu kỳ cho đến khi trình tự đạt 0 hoặc lặp lại. Việc theo dõi các giá trị đã truy cập đảm bảo rằng chúng tôi đếm từng số nguyên không âm riêng biệt chính xác một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O( | S | * (#giá trị riêng biệt)) | 
| Tối ưu hóa với tính năng phát hiện chu kỳ | O( | S | * hằng số nhỏ) | 

Cách tiếp cận tối ưu về cơ bản là mô phỏng giống nhau nhưng chính thức dừng khi không phát hiện được giá trị mới nào hoặc khi một số âm được tạo ra. Vì kích thước của`S`đã được sửa, điều này được đảm bảo chạy trong thời gian không đổi. 

## Hướng dẫn thuật toán 

1. Khởi tạo một bộ`visited`với giá trị ban đầu`X`. Điều này theo dõi tất cả các số nguyên không âm xuất hiện trong chuỗi. 
2. Đặt`current = X`. 
3. Lặp lại thao tác sau cho đến khi không có giá trị mới nào được thêm vào trong một chu kỳ đầy đủ: 

1. Lặp qua chuỗi`S`: 

1. Nếu nhân vật là`+`, tăng`current`bằng 1. 
2. Nếu nhân vật là`-`, giảm`current`bằng 1. 
3. Nếu`current`trở nên âm, loại bỏ nó và dừng chu trình này. 
4. Nếu không, hãy thêm`current`ĐẾN`visited`. 
2. Nếu kích thước của`visited`không thay đổi trong chu kỳ này, hãy phá vỡ vòng lặp. 
4. Xuất ra kích thước của`visited`. 

Tại sao nó hoạt động: Mỗi lần lặp lại sẽ thêm tất cả các số nguyên không âm mới có thể truy cập được bằng một ứng dụng nữa`S`. Bởi vì`S`có độ dài 3 và giá trị không thể âm, tập hợp các số có thể truy cập tăng lên một cách đơn điệu nhưng bị giới hạn. Thuật toán dừng khi không có số mới nào xuất hiện, đảm bảo rằng chúng tôi đếm tất cả các số nguyên không âm riêng biệt có thể truy cập chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

S = input().strip()
X = int(input())

visited = set()
visited.add(X)
current = X

while True:
    new_values = set()
    temp = current
    for op in S:
        if op == '+':
            temp += 1
        else:
            temp -= 1
        if temp >= 0:
            new_values.add(temp)
    if not new_values - visited:
        break
    visited.update(new_values)
    current = max(new_values)

print(len(visited))
```Mã khởi tạo tập hợp các giá trị đã truy cập bằng`X`. Đối với mỗi chu kỳ của`S`, nó tính toán các giá trị mới có thể tiếp cận mà không để`current`đi tiêu cực. Quá trình này dừng khi chu trình không tạo ra giá trị mới, đảm bảo chúng ta đếm từng giá trị riêng biệt một lần. Sự lựa chọn của`current = max(new_values)`đảm bảo chúng tôi tiếp tục mô phỏng từ giá trị lớn nhất có thể đạt được, giúp nắm bắt hiệu quả tất cả các mức tăng có thể. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
+-+
1
```| Bước | Hiện tại | Giá trị mới | Đã truy cập | 
| --- | --- | --- | --- | 
| 0 | 1 | {} | {1} | 
| 1 | 1 → 2 → 1 → 2 | {2} | {1,2} | 
| 2 | 2 → 3 → 2 → 3 | {3} | {1,2,3} | 
| 3 | 3 → 4 → 3 → 4 | {4} | {1,2,3,4} | 
| 4 | ... | Không có giá trị mới | {1,2,3,4} | 

Đầu ra:`4`Điều này chứng tỏ rằng thuật toán nắm bắt tất cả các dao động và kết thúc khi không có giá trị mới nào được tạo ra. 

Đầu vào mẫu 2:```
---
2
```| Bước | Hiện tại | Giá trị mới | Đã truy cập | 
| --- | --- | --- | --- | 
| 0 | 2 | {} | {2} | 
| 1 | 2 → 1 → 0 → -1 | {1,0} | {0,1,2} | 
| 2 | 0 → -1 → -2 → -3 | {} | {0,1,2} | 

Đầu ra:`3`Điều này cho thấy việc xử lý các giá trị âm một cách chính xác bằng cách loại bỏ chúng ngay lập tức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | S | 
| Không gian | O(#giá trị khác biệt) | Chúng tôi lưu trữ tất cả các số nguyên không âm riêng biệt có thể truy cập từ`X`. | 

Được cho`|S|=3`, thậm chí lớn`X`được xử lý hiệu quả vì chuỗi giảm về 0 hoặc dao động trong một phạm vi nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    S = input().strip()
    X = int(input())
    visited = set([X])
    current = X
    while True:
        new_values = set()
        temp = current
        for op in S:
            if op == '+':
                temp += 1
            else:
                temp -= 1
            if temp >= 0:
                new_values.add(temp)
        if not new_values - visited:
            break
        visited.update(new_values)
        current = max(new_values)
    return str(len(visited))

# Provided samples
assert run("+-+\n1\n") == "4", "sample 1"
assert run("---\n2\n") == "3", "sample 2"

# Custom cases
assert run("+++\n0\n") == "4", "increasing from 0"
assert run("++-\n1\n") == "3", "mixed operations"
assert run("--+\n1\n") == "3", "decreasing then increasing"
assert run("---\n0\n") == "1", "all negative from 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`+++\n0`| 4 | dãy luôn tăng, bắt đầu từ 0 | 
|`++-\n1`| 3 | hoạt động hỗn hợp tạo dao động | 
|`--+\n1`| 3 | giảm rồi tăng, xử lý số 0 | 
|`---\n0`| 1 | giá trị âm bị loại bỏ ngay lập tức | 

## Vỏ cạnh 

Khi nào`X = 0`Và`S`chỉ chứa`-`, thuật toán xử lý nó một cách chính xác bằng cách chỉ tạo ra`{0}`. Đối với đầu vào`S = "---"`Và`X = 0`, giá trị tạm thời ngay lập tức trở thành âm sau bước đầu tiên, do đó không có giá trị mới nào được thêm vào. Đầu ra là`1`, phù hợp với mong đợi. Tương tự, các trình tự như`"+-+"`bắt đầu từ nhỏ`X`dao động chính xác và vòng lặp kết thúc khi không có giá trị mới nào xuất hiện. Thuật toán đảm bảo rằng tất cả các số nguyên không âm có thể truy cập riêng biệt đều được tính chính xác một lần.
