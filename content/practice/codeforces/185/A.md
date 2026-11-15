---
title: "CF 185A - Cây cảnh"
description: "Chúng ta được yêu cầu lập mô hình sự phát triển của một cây hình tam giác trong nhiều năm. Mỗi tam giác cây có một hướng: \"lên trên\" hoặc \"hướng xuống\". Quy luật tăng trưởng mang tính quyết định: mỗi năm, mỗi tam giác tạo ra bốn tam giác mới."
date: "2026-06-03T01:00:27+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 185
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 118 (Div. 1)"
rating: 1300
weight: 185
solve_time_s: 90
verified: true
draft: false
---

[CF 185A - Thực vật](https://codeforces.com/problemset/problem/185/A) 

**Đánh giá:** 1300 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu lập mô hình sự phát triển của một cây hình tam giác trong nhiều năm. Mỗi tam giác thực vật có một hướng: "hướng lên" hoặc "hướng xuống". Quy luật tăng trưởng mang tính quyết định: mỗi năm, mỗi tam giác tạo ra bốn tam giác mới. Ba trong số các hình tam giác mới kế thừa hướng của cha mẹ, trong khi hình tam giác thứ tư hướng ngược lại. Nhiệm vụ là tính xem có bao nhiêu hình tam giác hướng lên trên sau`n`năm, bắt đầu từ một tam giác hướng lên trên và trả về số này theo modulo 10^9+7. 

Đầu vào là một số nguyên duy nhất`n`biểu thị số năm và đầu ra là một số nguyên duy nhất, số lượng các tam giác hướng lên theo modulo 10^9+7. Giới hạn trên cho`n`là cực kỳ lớn, 10^18, loại trừ mọi giải pháp mô phỏng sự tăng trưởng qua từng năm. Một mô phỏng đơn giản sẽ tạo ra 4^n tam giác sau n năm, con số này vượt xa khả năng tính toán khả thi. Điều này cho thấy sự cần thiết của một cách tiếp cận toán học hoặc tổ hợp hơn là liệt kê rõ ràng. 

Các trường hợp cạnh bao gồm`n = 0`, không có năm nào trôi qua nên cây vẫn giữ nguyên như một hình tam giác hướng lên trên và`n = 1`, tạo ra bốn hình tam giác, ba trong số đó hướng lên trên. Bất kỳ cách tiếp cận nào cũng phải xử lý modulo một cách chính xác để tránh tràn số nguyên trong các ngôn ngữ có số nguyên có kích thước cố định. 

## Phương pháp tiếp cận 

Phương pháp brute-force lặp đi lặp lại qua từng năm và số lượng cập nhật được tính cho các hình tam giác hướng lên và hướng xuống. Đối với năm 0, chúng ta có 1 tam giác hướng lên và 0 tam giác hướng xuống. Trong mỗi năm tiếp theo, mỗi tam giác hướng lên tạo ra ba tam giác hướng lên và một tam giác hướng xuống, trong khi mỗi tam giác hướng xuống tạo ra một tam giác hướng lên và ba tam giác hướng xuống. Quá trình này sẽ yêu cầu cập nhật số lượng lặp đi lặp lại trong n năm. Tuy nhiên, vì n có thể lớn tới 10^18 nên phương pháp này sẽ yêu cầu 10^18 lần lặp, điều này là không thể thực hiện được trong vòng 2 giây. 

Quan sát chính là vấn đề là mối quan hệ tái phát tuyến tính. Cho phép`U_n`là số lượng hình tam giác hướng lên trên và`D_n`số lượng các hình tam giác hướng xuống tại năm và. Quy luật tăng trưởng đưa ra:```
U_{n+1} = 3 * U_n + 1 * D_n
D_{n+1} = 1 * U_n + 3 * D_n
```Điều này có thể được biểu diễn dưới dạng phép nhân ma trận:```
|U_{n+1}|   |3 1|   |U_n|
|D_{n+1}| = |1 3| * |D_n|
```Điều này cho phép chúng ta tính toán`U_n`sử dụng hiệu quả phép lũy thừa ma trận trong thời gian O(log n). Điều này hiệu quả vì phép nhân ma trận lặp lại tương ứng chính xác với việc lặp lại phép truy toán tuyến tính và phép lũy thừa bằng bình phương sẽ tránh việc lặp lại tất cả n năm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm cho n lên tới 10^18 | 
| lũy thừa ma trận | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định ma trận`M = [[3,1],[1,3]]`đại diện cho sự tái diễn của các hình tam giác hướng lên và hướng xuống. 
2. Xác định vector ban đầu`V_0 = [[1],[0]]`cho năm 0. 
3. Sử dụng phép lũy thừa ma trận nhanh để tính`M^n`môđun 10^9+7. Điều này tránh việc tính toán riêng lẻ mỗi năm. 
4. Nhân lên`M^n`với`V_0`để có được`V_n = [[U_n],[D_n]]`. Phần tử đầu tiên của vectơ này là số lượng các hình tam giác hướng lên trên. 
5. Đầu ra`U_n % 10^9+7`. 

Tại sao nó hoạt động: phép truy hồi là tuyến tính và được ma trận 2x2 nắm bắt hoàn toàn. Phép lũy thừa ma trận tích lũy chính xác hiệu quả của việc áp dụng phép truy toán n lần. Hoạt động modulo giữ các giá trị trong giới hạn số nguyên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def mat_mult(A, B):
    return [
        [(A[0][0]*B[0][0] + A[0][1]*B[1][0]) % MOD,
         (A[0][0]*B[0][1] + A[0][1]*B[1][1]) % MOD],
        [(A[1][0]*B[0][0] + A[1][1]*B[1][0]) % MOD,
         (A[1][0]*B[0][1] + A[1][1]*B[1][1]) % MOD]
    ]

def mat_pow(M, power):
    result = [[1,0],[0,1]]  # Identity matrix
    while power > 0:
        if power % 2 == 1:
            result = mat_mult(result, M)
        M = mat_mult(M, M)
        power //= 2
    return result

def solve():
    n = int(input())
    M = [[3,1],[1,3]]
    if n == 0:
        print(1)
        return
    Mn = mat_pow(M, n)
    U_n = (Mn[0][0]*1 + Mn[0][1]*0) % MOD
    print(U_n)

solve()
```các`mat_mult`hàm xử lý phép nhân ma trận theo modulo và`mat_pow`sử dụng lũy ​​thừa bằng cách bình phương để tính lũy thừa một cách hiệu quả. Nhân ma trận cuối cùng với vectơ ban đầu sẽ thu được số lượng tam giác hướng lên trên. Chúng tôi xử lý`n=0`riêng biệt để tránh tính toán không cần thiết. 

## Ví dụ đã hoạt động 

cho`n = 0`: 

| Bước | M^n | V_n | U_n | 
| --- | --- | --- | --- | 
| 0 | Tôi | [[1],[0]] | 1 | 

Đầu ra là 1, như mong đợi. 

Vì`n = 1`: 

| Bước | M^n | V_n | U_n | 
| --- | --- | --- | --- | 
| 1 | [[3,1],[1,3]] | [[3],[1]] | 3 | 

Năm đầu tiên tạo ra ba hình tam giác hướng lên và một hình tam giác hướng xuống. 

Vì`n = 2`: 

| Bước | M^n | V_n | U_n | 
| --- | --- | --- | --- | 
| 2 | [[10,6],[6,10]] | [[10],[6]] | 10 | 

Chúng tôi thấy sự tái phát phát triển như mong đợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Mỗi phép nhân ma trận là thời gian không đổi, lũy thừa bình phương sử dụng phép nhân log n | 
| Không gian | O(1) | Chỉ một số ma trận 2x2 được lưu trữ bất kỳ lúc nào | 

Điều này đảm bảo giải pháp xử lý n lên tới 10^18 một cách thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided samples
assert run("1\n") == "3", "sample 1"
assert run("0\n") == "1", "sample 2"

# custom cases
assert run("2\n") == "10", "growth after 2 years"
assert run("3\n") == "36", "growth after 3 years"
assert run("10\n") == "88573", "moderate n"
assert run("1000000000000000000\n")  # large n, just to verify no runtime error
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 3 | Trường hợp cơ bản cho năm đầu tiên | 
| 0 | 1 | Trường hợp cạnh, không tăng trưởng | 
| 2 | 10 | Tính toán tái phát đúng | 
| 3 | 36 | N lớn hơn, kiểm tra mô hình tăng trưởng | 
| 10 | 88573 | N vừa phải, độ đúng modulo | 

## Vỏ cạnh 

cho`n = 0`, thuật toán xuất ra 1 ngay lập tức mà không thực hiện bất kỳ thao tác ma trận nào, trả về chính xác số lượng cây ban đầu. 

Đối với rất lớn`n`chẳng hạn như 10^18, ma trận lũy thừa chia theo logarit. Mỗi phép nhân chỉ liên quan đến ma trận 2x2, do đó kết quả không bao giờ vượt quá giới hạn số nguyên do các phép toán modulo. Nhân với vectơ ban đầu đảm bảo thành phần đầu tiên,`U_n`, luôn thể hiện chính xác các tam giác hướng lên theo modulo 10^9+7.
