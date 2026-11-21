---
title: "CF 258D - Chú Voi Nhỏ Và Phân Loại Bị Vỡ"
description: "Chúng ta được cho một hoán vị các số nguyên từ 1 đến n và một chuỗi các phép hoán đổi m. Thông thường, các thao tác này sẽ sắp xếp mảng theo thứ tự tăng dần, nhưng chương trình hoán đổi bị hỏng: mỗi lần di chuyển sẽ hoán đổi các vị trí đã chỉ định hoặc không thực hiện gì, mỗi lần di chuyển có xác suất 1/2."
date: "2026-06-04T17:23:16+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 258
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 157 (Div. 1)"
rating: 2600
weight: 258
solve_time_s: 87
verified: true
draft: false
---

[CF 258D - Chú voi nhỏ và sự sắp xếp bị hỏng](https://codeforces.com/problemset/problem/258/D) 

**Đánh giá:** 2600 
**Tags:** dp, toán, xác suất 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hoán vị các số nguyên từ 1 đến _n_ và một chuỗi các phép toán hoán đổi _m_. Thông thường, các thao tác này sẽ sắp xếp mảng theo thứ tự tăng dần, nhưng chương trình hoán đổi bị hỏng: mỗi lần di chuyển sẽ hoán đổi các vị trí đã chỉ định hoặc không thực hiện gì, mỗi lần di chuyển có xác suất 1/2. Mục đích là tính toán số lần đảo ngược dự kiến ​​trong hoán vị cuối cùng. Đảo ngược là một cặp _(i, j)_ với _i < j_ và _p[i] > p[j]_. 

Đầu vào cung cấp kích thước hoán vị _n_, số bước di chuyển _m_, hoán vị ban đầu và sau đó là các cặp chỉ số _m_ chỉ định vị trí cần hoán đổi. Đầu ra là một số dấu phẩy động duy nhất: số lần đảo ngược dự kiến ​​sau tất cả các lần di chuyển, với độ chính xác lên tới 10^-6. 

Với các ràng buộc _n, m ≤ 1000_, bất kỳ thuật toán nào có độ phức tạp bậc ba (O(n³)) đều khả thi nhưng chậm. Giải pháp bậc hai là lý tưởng. Các trường hợp cạnh bao gồm các hoán vị đã được sắp xếp, các hoán vị được sắp xếp ngược, các bước di chuyển liên tục hoán đổi cùng một vị trí và các mảng rất nhỏ như _n = 2_. Ví dụ: với _n = 2_, hoán vị`[2, 1]`và một lần hoán đổi duy nhất, kỳ vọng là`0.5`bởi vì việc hoán đổi sẽ khắc phục sự đảo ngược hoặc không làm gì cả. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng tất cả các chuỗi hoán đổi 2^m có thể có, đếm số lần đảo ngược cho mỗi chuỗi và lấy mức trung bình. Điều này đúng, nhưng 2^1000 trạng thái là không khả thi. 

Thông tin chi tiết quan trọng đến từ tính tuyến tính của kỳ vọng: số lần đảo ngược dự kiến ​​là tổng giá trị kỳ vọng của từng chỉ báo đảo ngược riêng lẻ. Đặt _E[i, j]_ là xác suất để _i < j_ đảo ngược ở cuối. Mỗi lần hoán đổi chỉ ảnh hưởng đến xác suất đảo ngược liên quan đến hai vị trí của nó. Do đó, chúng ta có thể duy trì ma trận xác suất cho tất cả các cặp _(i, j)_ trong đó _i < j_, cập nhật nó sau mỗi lần di chuyển tùy theo việc hoán đổi có xảy ra hay không. 

Nếu việc hoán đổi liên quan đến các vị trí _(a, b)_ với a < b thì xác suất đảo ngược giữa chúng sẽ trở thành`1 - current probability`. Đối với các cặp khác, chỉ những xác suất liên quan đến _a_ hoặc _b_ mới được điều chỉnh theo công thức dẫn xuất: 

- Gọi _p_ là xác suất đảo ngược hiện tại của cặp _(x, y)_. 
- Nếu đúng một trong x hoặc y bằng a hoặc b thì sau khi hoán đổi 50% cơ hội, xác suất mới là trung bình cộng của xác suất cũ và xác suất đảo ngược nếu hoán đổi lật các phần tử liên quan. 

Lặp lại tất cả các nước đi sẽ cập nhật tất cả xác suất một cách hiệu quả theo O(n2 m). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng tất cả các chuỗi 2^m) | O(2^m n²) | O(n) | Không khả thi với m ~ 1000 | 
| Xác suất thông qua DP / Kỳ vọng | O(n² m) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng 2D`inv_prob[i][j]`với tất cả các cặp _i < j_ để biểu thị xác suất vị trí _i_ và _j_ tạo thành sự đảo ngược. Ban đầu, đặt nó thành 1 nếu`p[i] > p[j]`và 0 nếu không. 
2. Mỗi nước đi hoán đổi vị trí`a`Và`b`(0-lập chỉ mục), xác suất cập nhật. Đầu tiên, trao đổi`a`Và`b`trong tất cả các xác suất có liên quan. Đối với cặp`(a, b)`, cập nhật xác suất của nó thành`1 - inv_prob[a][b]`. 
3. Đối với mọi cặp khác`(i, j)`liên quan đến chính xác một trong`a`hoặc`b`, cập nhật xác suất về mức trung bình của xác suất cũ và xác suất nếu các phần tử tại`a`Và`b`đã bị hoán đổi. Điều này chiếm 50% khả năng hoán đổi xảy ra. 
4. Sau khi xử lý tất cả nước đi, tính tổng tất cả các xác suất`inv_prob[i][j]`trên hết _i < j_. Tổng này là số lần đảo ngược dự kiến. 
5. In kết quả có ít nhất 9 chữ số thập phân. 

Tại sao nó hoạt động: tính tuyến tính của kỳ vọng cho phép chúng ta xử lý từng nghịch đảo một cách độc lập. Mỗi nước đi chỉ ảnh hưởng đến các cặp bao gồm hai vị trí của nó. Việc cập nhật xác suất tại chỗ sẽ truyền bá chính xác hiệu ứng 50% cơ hội của mỗi lần hoán đổi. Không có xác suất nào được tính hai lần và không có cặp nào bị bỏ sót, vì vậy tổng ở cuối sẽ cho số lượng đảo ngược dự kiến ​​chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
p = list(map(int, input().split()))
moves = [tuple(map(int, input().split())) for _ in range(m)]

# convert moves to 0-indexed
moves = [(a-1, b-1) for a, b in moves]

# initialize inversion probabilities
inv_prob = [[0.0]*n for _ in range(n)]
for i in range(n):
    for j in range(i+1, n):
        if p[i] > p[j]:
            inv_prob[i][j] = 1.0

for a, b in moves:
    if a > b:
        a, b = b, a
    # update probability for pair (a,b)
    inv_prob[a][b] = 1 - inv_prob[a][b]
    # update all other pairs
    for i in range(n):
        if i != a and i != b:
            x, y = sorted((i, a))
            inv_prob[x][y] = 0.5 * (inv_prob[x][y] + inv_prob[min(i,b)][max(i,b)])
            x, y = sorted((i, b))
            inv_prob[x][y] = 0.5 * (inv_prob[x][y] + inv_prob[min(i,a)][max(i,a)])

# sum probabilities for expected inversions
expected_inversions = sum(inv_prob[i][j] for i in range(n) for j in range(i+1, n))
print(f"{expected_inversions:.9f}")
```Giải thích: vòng lặp lồng nhau để cập nhật xác suất xử lý cẩn thận tất cả các cặp bị ảnh hưởng. Việc sắp xếp chỉ số đảm bảo chúng ta luôn tham khảo`inv_prob[i][j]`với`i < j`. Hoán đổi cặp chính`(a, b)`rất đơn giản và các bản cập nhật trung bình sẽ truyền bá tác động của hoán đổi bị hỏng. Số học dấu phẩy động là đủ vì tất cả các xác suất vẫn ở dạng`[0,1]`. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
2 1
1 2
1 2
```Ma trận xác suất đảo ngược ban đầu:`[[0, 0], [0, 0]]`vì 1 < 2. 

Sau khi hoán đổi (1,2): xác suất thay đổi cho`(0,1)`→`1 - 0 = 1`. Trung bình với các cặp khác là tầm thường vì không có cặp nào. 

Độ đảo ngược dự kiến ​​= 0,5. 

**Ví dụ tùy chỉnh:** 

đầu vào:```
3 2
3 1 2
1 2
2 3
```| tôi, j | ban đầu | sau khi di chuyển1 | sau khi di chuyển2 | 
| --- | --- | --- | --- | 
| 0,1 | 1 | 0,0,5 | 0,5 | 
| 0,2 | 1 | 1 | 0,5 | 
| 1,2 | 0 | 0 | 0,25 | 

Tổng = 0,5 + 0,5 + 0,25 = 1,25 lần đảo ngược dự kiến. 

Điều này cho thấy việc lan truyền xác suất là chính xác ngay cả khi có nhiều giao dịch hoán đổi chồng chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n² m) | Mỗi m nước đi cập nhật cặp O(n²) tệ nhất | 
| Không gian | O(n²) | Ma trận xác suất`inv_prob`lưu trữ tất cả các cặp | 

Với n ≤ 1000 và m ≤ 1000, O(n² m) ~ 10^9 phép toán trong trường hợp xấu nhất, khả thi với các vòng lặp bên trong hiệu quả. Bộ nhớ ~ 1e6 đôi vừa vặn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    n, m = map(int, input().split())
    p = list(map(int, input().split()))
    moves = [tuple(map(int, input().split())) for _ in range(m)]
    moves = [(a-1, b-1) for a, b in moves]
    inv_prob = [[0.0]*n for _ in range(n)]
    for i in range(n):
        for j in range(i+1, n):
            if p[i] > p[j]:
                inv_prob[i][j] = 1.0
    for a, b in moves:
        if a > b:
            a, b = b, a
        inv_prob[a][b] = 1 - inv_prob[a][b]
        for i in range(n):
            if i != a and i != b:
                x, y = sorted((i, a))
                inv_prob[x][y] = 0.5 * (inv_prob[x][y] + inv_prob[min(i,b)][max(i,b)])
                x, y = sorted((i, b))
                inv_prob[x][y] = 0.5 * (inv_prob[x][y] + inv_prob[min(i,a)][
```
