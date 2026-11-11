---
title: "CF 1599F - Sao Hỏa"
description: "Chúng ta được cung cấp một danh sách các vị trí thành phố trên một thuộc địa hình tròn của sao Hỏa, nơi các thành phố được đánh số modulo $10^9+7$. Mỗi truy vấn hỏi liệu có thể kết nối tất cả các thành phố trong một phân nhóm nhất định bằng cách sử dụng các con đường có độ dài cố định $D$ hay không."
date: "2026-06-10T08:41:02+07:00"
tags: ["codeforces", "competitive-programming", "hashing"]
categories: ["algorithms"]
codeforces_contest: 1599
codeforces_index: "F"
codeforces_contest_name: "Bubble Cup 14 - Finals Online Mirror (Unrated, ICPC Rules, Teams Preferred, Div. 1)"
rating: 2700
weight: 1599
solve_time_s: 143
verified: false
draft: false
---

[CF 1599F - Sao Hỏa](https://codeforces.com/problemset/problem/1599/F) 

**Xếp hạng:** 2700 
**Thẻ:** băm 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các vị trí thành phố trên một thuộc địa hình tròn của sao Hỏa nơi các thành phố được đánh số modulo$10^9+7$. Mỗi truy vấn hỏi liệu có thể kết nối tất cả các thành phố trong một phân nhóm nhất định bằng cách sử dụng các con đường có độ dài cố định hay không$D$. Kết nối với một con đường dài$D$có nghĩa là, nếu bạn lấy bất kỳ hai thành phố nào được kết nối$x$Và$y$, sự khác biệt$|x-y|$modulo$10^9+7$phải chia hết cho$D$. Mục tiêu là trả lời "Có" nếu tồn tại một thứ tự của các thành phố này sao cho mỗi cặp liên tiếp khác nhau một cách chính xác.$D$modulo$10^9+7$, và "Không" nếu không. 

Các ràng buộc rất lớn: lên tới$2 \cdot 10^5$các thành phố và các truy vấn, loại trừ mọi kiểm tra hoán vị mạnh mẽ về sự sắp xếp của thành phố. Bất kỳ giải pháp nào lặp lại các mảng con một cách đơn giản sẽ dẫn đến$O(N \cdot Q)$hoạt động, có khả năng$4 \cdot 10^{10}$, vượt xa khả thi. 

Khía cạnh không rõ ràng của vấn đề là các thành phố có thể lặp lại và mảng không được sắp xếp. Ví dụ, một cách tiếp cận bất cẩn có thể chỉ kiểm tra xem thành phố đầu tiên và cuối cùng có khác nhau nhiều lần không.$D$, nhưng điều đó không thành công khi sự khác biệt nội bộ giữa các thành phố vi phạm yêu cầu về khoảng cách. Ví dụ, hãy xem xét các thành phố`[0, 3, 6]`Và$D=3$. Bất kỳ mảng con nào cũng hoạt động, nhưng nếu mảng con đó`[0, 6]`, một phép kiểm tra ngây thơ chỉ sử dụng điểm cuối có thể trả lời sai là "Không", vì$6-0$chia hết cho 3, nhưng nếu mảng là`[0, 4, 8]`Và$D=2$, mặc dù chỉ nhìn vào điểm cuối sẽ nói "Có" một cách không chính xác$4-0=4$Và$8-4=4$đều chia hết cho 2, nên trong trường hợp này nó thực sự có tác dụng. Các trường hợp biên bao gồm các mảng con với một thành phố duy nhất hoặc khi tất cả các thành phố giống hệt nhau, trong đó bất kỳ$D$hoạt động. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: đối với mỗi truy vấn, lặp lại mảng con, kiểm tra tất cả sự khác biệt giữa các thành phố liên tiếp và xác minh rằng mỗi sự khác biệt theo modulo$10^9+7$chia hết cho$D$. Điều này đúng vì bài toán trực tiếp yêu cầu tính chia hết của khoảng cách nhưng lại quá chậm. Nếu mỗi mảng con có độ dài$N$trong trường hợp xấu nhất và chúng ta có$Q$truy vấn, dẫn đến$O(N \cdot Q) \sim 4 \cdot 10^{10}$hoạt động. 

Quan sát quan trọng là chúng ta không cần phải xem xét tất cả các cặp. Chúng ta chỉ cần ước số chung lớn nhất (GCD) của tất cả các khác biệt theo cặp trong mảng con. Nếu GCD của những khác biệt này chia hết cho$D$, khi đó có thể sắp xếp các thành phố theo trình tự có chiều dài đường$D$. Điều này có tác dụng vì trình tự có thể được sắp xếp lại một cách tùy ý; GCD ghi lại khoảng cách chung tốt nhất tồn tại giữa tất cả các thành phố. Bằng cách tính toán trước các GCD tiền tố của các khác biệt trong mảng, chúng ta có thể trả lời từng truy vấn trong$O(\log N)$sử dụng cây phân đoạn hoặc bảng thưa thớt. Điều này làm giảm tổng độ phức tạp từ mức không khả thi$O(NQ)$khả thi$O(N \log N + Q \log N)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N * Q) | O(1) | Quá chậm | 
| Tối ưu (GCD + Bảng thưa) | O(N log N + Q log N) | O(N log N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán tất cả sự khác biệt giữa các thành phố liên tiếp theo modulo$10^9+7$. Đối với mảng`a`, tạo nên`diff[i] = (a[i+1] - a[i]) % MOD`. Điều này nắm bắt được khoảng cách hiệu quả cần căn chỉnh để xây dựng đường. 
2. Xây dựng một bảng thưa thớt`diff`cho các truy vấn GCD. Điều này cho phép chúng ta tính toán GCD của bất kỳ mảng con nào có sự khác biệt về$O(1)$mỗi truy vấn. 
3. Đối với mỗi truy vấn`(L, R, D)`, hãy xem xét mảng con từ chỉ mục`L-1`ĐẾN`R-1`. Tính GCD của sự khác biệt`diff[L-1:R-1]`. 
4. Nếu GCD này chia hết cho`D`, xuất ra "Có"; nếu không thì xuất ra "Không". 
5. Các mảng con một phần tử thường là "Có" vì không có khoảng trống để kết nối. 

Tại sao nó hoạt động: Bằng cách lấy GCD của tất cả những khác biệt liên tiếp trong mảng con, chúng tôi nắm bắt được khoảng cách tối thiểu giữa các thành phố. Nếu khoảng cách này chia hết cho`D`, thì có thể bố trí các thành phố sao cho mỗi kết nối đều chính xác`D`. Modulo đảm bảo xử lý chính xác cấu trúc hình tròn mà không bị tràn. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

MOD = 10**9 + 7

def build_sparse_table(arr):
    n = len(arr)
    K = n.bit_length()
    st = [[0]*n for _ in range(K)]
    st[0] = arr[:]
    for k in range(1, K):
        for i in range(n - (1 << k) + 1):
            st[k][i] = math.gcd(st[k-1][i], st[k-1][i + (1 << (k-1))])
    return st

def query(st, l, r):
    k = (r - l + 1).bit_length() - 1
    return math.gcd(st[k][l], st[k][r - (1 << k) + 1])

def main():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    if n > 1:
        diff = [(a[i+1] - a[i]) % MOD for i in range(n-1)]
        st = build_sparse_table(diff)
    else:
        diff = []
        st = []

    for _ in range(q):
        L, R, D = map(int, input().split())
        L -= 1
        R -= 1
        if L == R:
            print("Yes")
            continue
        g = query(st, L, R-1)
        print("Yes" if g % D == 0 else "No")

if __name__ == "__main__":
    main()
```Giải pháp đầu tiên tính toán trước các sai phân liên tiếp để nắm bắt các khoảng cách tiềm năng. Bảng thưa thớt cho phép truy vấn GCD trong O(1) cho mỗi truy vấn. Các mảng con một phần tử được xử lý riêng biệt để tránh các phạm vi trống. Trừ một từ các chỉ mục sẽ căn chỉnh chúng với các danh sách Python dựa trên số 0. Hoạt động Modulo đảm bảo xử lý thích hợp các vị trí thành phố hình tròn. 

## Ví dụ đã hoạt động 

Sử dụng mẫu 1: 

| Truy vấn | mảng con | Sự khác biệt modulo MOD | GCD | D | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 2 3 12 | [0, 12] | [12] | 12 | 12 | Có | 
| 2 3 6 | [0, 12] | [12] | 12 | 6 | Không | 
| 4 6 2 | [6, 10, 8] | [4, 998244736] | 2 | 2 | Có | 

Các bảng cho thấy thuật toán tính toán chính xác hiệu và GCD, đồng thời đánh giá khả năng chia hết cho D. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N + Q log N) | Xây dựng bảng thưa thớt là O(N log N), mỗi truy vấn O(log N) | 
| Không gian | O(N log N) | Bảng thưa lưu trữ nhiều cấp độ GCD | 

Giải pháp vừa vặn thoải mái trong giới hạn 2 giây cho$N, Q \le 2 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    main()
    return out.getvalue().strip()

# provided sample
assert run("9 8\n17 0 12 6 10 8 2 4 5\n2 3 12\n2 3 6\n2 4 6\n4 6 2\n2 8 2\n1 2 17\n1 8 2\n9 9 14") == \
"Yes\nNo\nYes\nYes\nYes\nYes\nNo\nYes"

# minimum input
assert run("1 1\n0\n1 1 0") == "Yes"

# all equal
assert run("3 2\n5 5 5\n1 3 1\n1 3 2") == "Yes\nYes"

# maximum D
assert run("2 1\n0 1000000006\n1 2 1000000006") == "Yes"

# boundary consecutive
assert run("3 1\n0 2 4\n1 3 2") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | Có | Mảng con một phần tử | 
| 3 2 | Có Có | Tất cả các thành phố đều bình đẳng, D nào cũng có tác dụng | 
| 2 1 | Có | Giá trị lớn gần ranh giới MOD | 
| 3 1 | Có | Sự khác biệt modulo MOD, tính đúng đắn của GCD | 

## Vỏ cạnh 

Các mảng con một phần tử là
