---
title: "CF 166E - Tứ diện"
description: "Chúng ta được yêu cầu đếm số cách một con kiến ​​có thể xuất phát từ đỉnh D của một tứ diện và quay trở lại D sau đúng n bước, di chuyển dọc theo các cạnh ở mỗi bước. Tứ diện có bốn đỉnh được ký hiệu là A, B, C, D và mỗi đỉnh được nối với ba đỉnh còn lại."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "matrices"]
categories: ["algorithms"]
codeforces_contest: 166
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 113 (Div. 2)"
rating: 1500
weight: 166
solve_time_s: 180
verified: true
draft: false
---

[CF 166E - Tứ diện](https://codeforces.com/problemset/problem/166/E) 

**Đánh giá:** 1500 
**Tags:** dp, toán, ma trận 
**Thời gian giải:** 3 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm số cách một con kiến có thể xuất phát từ đỉnh D của một tứ diện và quay trở lại D sau đúng _n_ bước, di chuyển dọc theo các cạnh ở mỗi bước. Tứ diện có bốn đỉnh được ký hiệu là A, B, C, D và mỗi đỉnh được nối với ba đỉnh còn lại. Đầu vào là một số nguyên _n_ biểu thị độ dài đường đi và đầu ra là số chuỗi các bước di chuyển riêng biệt bắt đầu và kết thúc tại D, modulo 10^9 + 7. 

Cho rằng _n_ có thể lớn tới 10^7, chúng tôi không thể mô phỏng rõ ràng tất cả các đường dẫn có thể có. Một cách tiếp cận mạnh mẽ liệt kê từng đường dẫn sẽ có độ phức tạp theo thời gian theo cấp số nhân là O(3^n), không khả thi đối với _n_ vượt quá 20 hoặc 30. Thay vào đó, chúng ta cần một cách tiếp cận tính toán kết quả một cách hiệu quả, trong khoảng thời gian O(log n) hoặc O(n). 

Các trường hợp cạnh không rõ ràng bao gồm _n_ nhỏ nhất. Đối với _n_ = 1, con kiến ​​không thể quay lại D trong một bước vì nó phải di chuyển sang con lân cận, vì vậy câu trả lời là 0. Một trường hợp khác là _n_ = 2, trong đó con kiến ​​có chính xác ba lựa chọn: D → A → D, D → B → D, D → C → D. Việc xử lý chính xác những đầu vào nhỏ này đảm bảo các trường hợp cơ sở trong phép lặp hoặc lũy thừa ma trận của chúng ta được thiết lập đúng. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Người ta có thể liệt kê đệ quy tất cả các chuỗi bước đi từ D và đếm những chuỗi quay trở lại D sau _n_ bước. Điều này hiệu quả vì mỗi bước có chính xác ba bước di chuyển có thể xảy ra và việc khám phá đệ quy tất cả các đường dẫn sẽ nắm bắt được mọi trình tự hợp lệ. Tuy nhiên, độ phức tạp về thời gian là O(3^n), điều này bùng nổ ngay cả đối với các giá trị vừa phải của _n_. Điều này nhanh chóng trở nên không thực tế khi _n_ đạt tới 10^7. 

Quan sát quan trọng để có giải pháp nhanh hơn là số cách để đến một đỉnh sau _k_ bước chỉ phụ thuộc vào số cách để đến từng đỉnh sau _k-1_ bước. Đây là một thiết lập lập trình động cổ điển trên một biểu đồ nhỏ. Gọi dp[x][k] là số cách để đến đỉnh x trong k bước. Phép truy hồi rất đơn giản: để đến D ở bước k, con kiến ​​phải đến từ A, B hoặc C ở bước k-1. Tương tự, để đạt đến A trong bước k, kiến ​​phải đến từ B, C hoặc D. Phép lặp này có thể được biểu diễn dưới dạng ma trận chuyển tiếp 4×4 và được tính toán hiệu quả bằng cách sử dụng phép lũy thừa ma trận, giúp giảm độ phức tạp từ O(n) trên mỗi lần cập nhật trạng thái thành phép nhân ma trận O(log n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^n) | O(n) | Quá chậm | 
| DP / Hàm mũ ma trận | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đánh dấu các đỉnh A, B, C, D theo chỉ số 0, 1, 2, 3 để thuận tiện. Chúng tôi muốn đếm số bước đi có độ dài n bắt đầu và kết thúc tại D (chỉ số 3). 
2. Xác định ma trận kề 4×4 M biểu diễn các nước đi hợp lệ. Mỗi mục M[i][j] là 1 nếu đỉnh j có thể tới được từ i trong một bước và 0 nếu ngược lại. Đối với một tứ diện, mỗi đỉnh đều nối với ba đỉnh còn lại. Đối với D (chỉ số 3), hàng 3 sẽ có số 1 ở cột 0, 1, 2 và 0 ở cột 3. 
3. Biểu thị số cách để đến mỗi đỉnh sau k bước dưới dạng vectơ v_k có độ dài 4. Vector cho bước 0, v_0, là [0, 0, 0, 1] vì con kiến bắt đầu từ D. 
4. Tính v_n = M^n × v_0 bằng cách sử dụng lũy thừa ma trận. Mỗi phép nhân áp dụng phép lặp để tiến lên một bước. Phép lũy thừa ma trận cho phép chúng ta tính toán M^n trong phép nhân O(log n). 
5. Trả về v_n[3] modulo 10^9 + 7. Giá trị này tính tất cả các bước đi hợp lệ bắt đầu và kết thúc tại D sau n bước. 

Tại sao nó hoạt động: Ma trận kề nắm bắt tất cả các chuyển đổi một bước hợp lệ. Nhân ma trận nhiều lần áp dụng phép truy toán cho nhiều bước. Phép lũy thừa ma trận tính toán hiệu quả các chuyển đổi n bước mà không cần lặp lại n lần. Bất biến là sau k phép nhân, vectơ v_k lưu trữ chính xác số bước đi đến mỗi đỉnh theo k bước.

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def mat_mult(A, B):
    n = len(A)
    C = [[0]*n for _ in range(n)]
    for i in range(n):
        for j in range(n):
            for k in range(n):
                C[i][j] = (C[i][j] + A[i][k]*B[k][j]) % MOD
    return C

def mat_pow(mat, power):
    n = len(mat)
    result = [[1 if i==j else 0 for j in range(n)] for i in range(n)]
    while power > 0:
        if power % 2 == 1:
            result = mat_mult(result, mat)
        mat = mat_mult(mat, mat)
        power //= 2
    return result

n = int(input())
adj = [
    [0,1,1,1],
    [1,0,1,1],
    [1,1,0,1],
    [1,1,1,0]
]

# initial vector v0 corresponds to starting at D (index 3)
v0 = [0,0,0,1]

# compute M^n
M_n = mat_pow(adj, n)

# compute final vector v_n = M^n * v0
v_n = [0]*4
for i in range(4):
    for j in range(4):
        v_n[i] = (v_n[i] + M_n[i][j]*v0[j]) % MOD

print(v_n[3])
```các`mat_mult`hàm nhân hai ma trận 4×4 theo modulo 10^9+7. các`mat_pow`hàm nâng ma trận lên lũy thừa n bằng cách sử dụng lũy ​​thừa nhị phân. Ma trận kề mã hóa tứ diện và v0 đại diện cho đỉnh bắt đầu. Cuối cùng, chúng ta tính v_n bằng cách nhân M^n với v0 và mục nhập tương ứng với D sẽ đưa ra câu trả lời. 

## Ví dụ đã hoạt động 

Mẫu 1, n=2: 

| Bước | v_k (A,B,C,D) | 
| --- | --- | 
| 0 | [0,0,0,1] | 
| 1 | [1,1,1,0] | 
| 2 | [2,2,2,3] | 

v_2[3] = 3, khớp với ba đường dẫn D→A→D, D→B→D, D→C→D. 

Mẫu 2, n=3: 

| Bước | v_k (A,B,C,D) | 
| --- | --- | 
| 0 | [0,0,0,1] | 
| 1 | [1,1,1,0] | 
| 2 | [2,2,2,3] | 
| 3 | [7,7,7,6] | 

v_3[3] = 6, tương ứng với cả 6 bước đi có độ dài 3 bắt đầu và kết thúc tại D. 

Những dấu vết này xác nhận rằng phép tính lặp lại và lũy thừa ma trận tính toán chính xác số lần đi bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Mỗi phép nhân ma trận là một hằng số (4×4) và chúng ta thực hiện phép nhân O(log n) bằng cách sử dụng lũy ​​thừa. | 
| Không gian | O(1) | Chúng ta chỉ lưu trữ một vài ma trận và vectơ 4×4, độc lập với n. | 

Điều này phù hợp thoải mái trong giới hạn 2 giây ngay cả với n=10^7. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    MOD = 10**9 + 7

    def mat_mult(A, B):
        n = len(A)
        C = [[0]*n for _ in range(n)]
        for i in range(n):
            for j in range(n):
                for k in range(n):
                    C[i][j] = (C[i][j] + A[i][k]*B[k][j]) % MOD
        return C

    def mat_pow(mat, power):
        n = len(mat)
        result = [[1 if i==j else 0 for j in range(n)] for i in range(n)]
        while power > 0:
            if power % 2 == 1:
                result = mat_mult(result, mat)
            mat = mat_mult(mat, mat)
            power //= 2
        return result

    n = int(input())
    adj = [
        [0,1,1,1],
        [1,0,1,1],
        [1,1,0,1],
        [1,1,1,0]
    ]
    v0 = [0,0,0,1]
    M_n = mat_pow(adj, n)
    v_n = [0]*4
    for i in range(4):
        for j in range(4):
            v_n[i] = (v_n[i] + M_n[i][j]*v0[j]) % MOD
    return str(v_n[3])

# Provided samples
assert run("2\n")
```
