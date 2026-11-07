---
title: "CF 1360E - Đa giác"
description: "Chúng ta có một ma trận vuông $n nhân n$ bắt đầu hoàn toàn chứa đầy các số 0. Dọc theo cạnh trên, có các khẩu pháo $n$, một khẩu ở phía trên mỗi cột, và dọc theo mép trái, có các khẩu pháo $n$, một khẩu ở bên trái của mỗi hàng."
date: "2026-06-11T12:51:43+07:00"
tags: ["codeforces", "competitive-programming", "dp", "graphs", "implementation", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1360
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 644 (Div. 3)"
rating: 1300
weight: 1360
solve_time_s: 129
verified: true
draft: false
---

[CF 1360E - Đa giác](https://codeforces.com/problemset/problem/1360/E) 

**Đánh giá:** 1300 
**Tags:** dp, đồ thị, triển khai, đường dẫn ngắn nhất 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$ma trận vuông bắt đầu hoàn toàn chứa đầy số không. Dọc theo mép trên có$n$đại bác, một khẩu phía trên mỗi cột, và dọc theo mép trái, có$n$đại bác, một khẩu ở bên trái mỗi hàng. Mỗi khẩu pháo bắn một số "1" theo đường thẳng sang phải (pháo hàng) hoặc hướng xuống (pháo cột) cho đến khi chạm vào số 1 khác hoặc cạnh của ma trận. Khi số 1 dừng lại, nó sẽ tồn tại vĩnh viễn trong ô đó. 

Nhiệm vụ là xác định xem liệu cấu hình cuối cùng nhất định gồm 0 và 1 có thể được tạo ra bởi một số chuỗi phát bắn từ những khẩu pháo này hay không. Đầu vào là một loạt các ca kiểm thử, mỗi ca có kích thước$n$và$n \times n$ma trận và đầu ra phải là "CÓ" nếu có thể cấu hình và "KHÔNG" nếu ngược lại. 

Những hạn chế$1 \le n \le 50$và tổng số ô trên tất cả các trường hợp thử nghiệm$\le 10^5$đề nghị rằng một$O(n^2)$cách tiếp cận trên mỗi ma trận là có thể chấp nhận được. Các trường hợp không rõ ràng là những trường hợp mà số 1 ở vị trí mà nếu không có số 1 nào khác chặn đường đi của nó thì sẽ tiếp tục di chuyển qua mép. Ví dụ: số 1 ở góc dưới bên phải luôn ổn nhưng số 1 ở vị trí$(1,1)$trong ma trận 2x2 mà không có bất kỳ số 1 nào ở hàng hoặc cột đầu tiên là không thể vì không gì có thể ngăn chặn sớm một cú đánh ở đó. Việc triển khai ngây thơ không tính đến quy tắc "chặn bởi hàng xóm" này sẽ chấp nhận sai các cấu hình như vậy. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng mọi chuỗi cảnh quay có thể có và xem liệu nó có khớp với ma trận cuối cùng hay không. Mỗi khẩu pháo có thể bắn nhiều lần và có$2n$đại bác. Việc thử tất cả các chuỗi có tính chất tổ hợp và không khả thi, ngay cả đối với$n=10$. 

Cái nhìn sâu sắc quan trọng là làm việc lạc hậu. Số 1 trong ma trận phải ở cạnh dưới, cạnh phải hoặc có số 1 ngay bên phải hoặc bên dưới. Nếu số 1 không thỏa mãn tính chất này thì không có chuỗi phát đại bác nào có thể đặt nó ở đó mà không vi phạm quy tắc va chạm. Quan sát này cho phép chúng tôi kiểm tra từng ô một cách độc lập trong$O(n^2)$thời gian: đối với mỗi số 1 không ở hàng hoặc cột cuối cùng, chúng tôi đảm bảo rằng ô bên dưới hoặc bên phải cũng là số 1. Điều này thể hiện ràng buộc thiết yếu là số 1 dừng lại khi nó chạm đến ranh giới ma trận hoặc số 1 khác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((2n)^k) cho k ảnh | O(n^2) | Quá chậm | 
| Tối ưu | O(n^2) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại từng trường hợp kiểm thử và đọc kích thước$n$và ma trận. 
2. Đối với mỗi ô$(i, j)$trong ma trận, kiểm tra xem nó có chứa số 1 không. 
3. Nếu ô chứa số 1 và nó **không** ở hàng cuối cùng hoặc cột cuối cùng, hãy kiểm tra hai ô có thể chặn ảnh của nó:$(i+1, j)$(bên dưới) và$(i, j+1)$(Phải). 
4. Nếu cả ô bên dưới và ô bên phải đều bằng 0 thì không thể cấu hình. Ngay lập tức đánh dấu test case này là “NO” và chuyển sang test case tiếp theo. 
5. Nếu tất cả các số 1 đều vượt qua kiểm tra, thì có thể cấu hình được, vì vậy hãy đánh dấu trường hợp kiểm tra là "CÓ". 
6. Xuất kết quả cho tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: Theo cách xây dựng, bất kỳ số 1 nào ở vị trí không có ranh giới đều phải có bộ chặn ngay bên dưới hoặc ngay bên phải để có thể dừng ở đó. Bất biến này đảm bảo rằng mỗi 1 có thể bắt nguồn từ một phát đại bác và dừng lại một cách thích hợp. Nếu bất kỳ 1 nào vi phạm thuộc tính này thì không có chuỗi cảnh quay nào có thể tạo ra nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
results = []

for _ in range(t):
    n = int(input())
    grid = [input().strip() for _ in range(n)]
    possible = True
    for i in range(n):
        for j in range(n):
            if grid[i][j] == '1':
                if i < n - 1 and j < n - 1:
                    if grid[i+1][j] != '1' and grid[i][j+1] != '1':
                        possible = False
                        break
        if not possible:
            break
    results.append("YES" if possible else "NO")

print("\n".join(results))
```Giải pháp này lặp lại trên từng ô. Vòng lặp lồng nhau chỉ kiểm tra các ô không nằm ở hàng cuối cùng hoặc cột cuối cùng. Việc nghỉ sớm sẽ tránh được việc kiểm tra không cần thiết khi phát hiện thấy vi phạm. Lưới được lưu trữ dưới dạng danh sách các chuỗi để đơn giản hóa việc lập chỉ mục và so sánh với`'1'`. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
4
0010
0011
0000
0000
```| tôi | j | lưới[i][j] | tôi<n-1 & j<n-1? | lưới[i+1][j] | lưới[i][j+1] | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 1 | Đúng | 0 | 1 | được | 
| 1 | 2 | 1 | Đúng | 0 | 1 | được | 
| 1 | 3 | 1 | Sai | - | - | được | 

Tất cả các số 1 đều thỏa mãn quy tắc chặn, đầu ra là CÓ. 

Đầu vào mẫu 2:```
2
10
01
```| tôi | j | lưới[i][j] | tôi<n-1 & j<n-1? | lưới[i+1][j] | lưới[i][j+1] | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | Đúng | 0 | 0 | Thất bại | 

Giá trị 1 tại (0,0) không có hàng xóm bên phải hoặc phía dưới là 1, không thể cấu hình, đầu ra là NO. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) cho mỗi trường hợp thử nghiệm | Mỗi ô được kiểm tra tối đa một lần, tổng số ô ≤ 10^5 | 
| Không gian | O(n^2) | Lưu trữ lưới cho từng trường hợp thử nghiệm, không có cấu trúc bổ sung | 

Với những hạn chế, giải pháp này chạy thoải mái trong giới hạn 2 giây và sử dụng tốt bộ nhớ dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(input())
    results = []
    for _ in range(t):
        n = int(input())
        grid = [input().strip() for _ in range(n)]
        possible = True
        for i in range(n):
            for j in range(n):
                if grid[i][j] == '1':
                    if i < n - 1 and j < n - 1:
                        if grid[i+1][j] != '1' and grid[i][j+1] != '1':
                            possible = False
                            break
            if not possible:
                break
        results.append("YES" if possible else "NO")
    return "\n".join(results)

# Provided samples
assert run("5\n4\n0010\n0011\n0000\n0000\n2\n10\n01\n2\n00\n00\n4\n0101\n1111\n0101\n0111\n4\n0100\n1110\n0101\n0111\n") == "YES\nNO\nYES\nYES\nNO"

# Custom cases
assert run("1\n1\n1\n") == "YES"  # single-cell 1
assert run("1\n2\n11\n11\n") == "YES"  # full 2x2 grid
assert run("1\n3\n100\n010\n001\n") == "NO"  # diagonal without blockers
assert run("1\n3\n110\n111\n011\n") == "YES"  # complex valid pattern
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1x1 với 1 | CÓ | Trường hợp tầm thường đơn bào | 
| 2x2 đầy đủ 1 giây | CÓ | Lưới nhỏ hoàn chỉnh | 
| 3x3 đường chéo 1s | KHÔNG | Vị trí không thể thực hiện được nếu không có trình chặn | 
| khu phức hợp 3x3 | CÓ | Nhiều số 1 với bộ chặn chính xác | 

## Vỏ cạnh 

Đối với ma trận một ô, số 1 bất kỳ luôn hợp lệ vì nó nằm ở cả hàng và cột cuối cùng. 

Đối với ma trận 2x2 trong đó số 1 ở (0,0) và (1,1) nhưng (0,1) và (1,0) là 0, thuật toán xác định rằng số 1 ở (0,0) không có bộ chặn nào để dừng nó, tạo ra NO. 

Đối với một ma trận chứa toàn số 1, mỗi số 1 đều có một hàng xóm ở bên phải hoặc bên dưới hoặc nằm trên biên, tạo ra CÓ. 

Thuật toán xử lý tất cả các tình huống này một cách tự nhiên mà không cần thêm logic có điều kiện, xác nhận tính chính xác.
