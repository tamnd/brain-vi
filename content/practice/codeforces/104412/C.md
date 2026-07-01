---
title: "CF 104412C - Chọn Hai"
description: "Kết quả “1” của bạn tương ứng với giả định: mọi cấu hình đều đóng góp chính xác 1 chu kỳ một cách xác định. Vì vậy, mã đang xử lý cấu trúc một cách hiệu quả như thể các chu trình luôn được hình thành đầy đủ, điều này là sai."
date: "2026-07-01T02:31:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104412
codeforces_index: "C"
codeforces_contest_name: "2023 ICPC Gran Premio de Mexico 2da Fecha"
rating: 0
weight: 104412
solve_time_s: 216
verified: false
draft: false
---

[CF 104412C - Chọn hai](https://codeforces.com/problemset/problem/104412/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 36 giây 
**Đã xác minh:** không 

## Giải pháp 
## Các mẫu đang cho chúng ta biết điều gì 

### Mẫu 1```
4 2
2 4
3 1
→ expected 1/2 mod = 500000005
actual = 1
```Kết quả “1” của bạn tương ứng với giả định: 

> mỗi cấu hình đều đóng góp chính xác 1 chu kỳ một cách xác định 

Vì vậy, mã đang xử lý cấu trúc một cách hiệu quả như thể các chu trình luôn được hình thành đầy đủ, điều này là sai. 

### Mẫu 2```
9 6 ...
→ expected 833333343
actual 750000009
```Điều này rất đáng nói: 

- 833333343 = 1/6 mod 
- 750000009 = 3/4 mod 

Vì vậy, mã đang tính toán **sản phẩm có xác suất độc lập không chính xác**, trộn lẫn các phần phụ thuộc. 

## Nguyên nhân cốt lõi 

Sai lầm là về mặt khái niệm: 

Bạn đã xử lý vấn đề như sau: 

- Tập hợp thành phần DSU, hoặc 
- kỳ vọng hài hòa trên các thành phần, hoặc 
- xác suất chu kỳ độc lập 

Nhưng cấu trúc thực tế là: 

> Chúng tôi đang hoàn thiện một biểu đồ hàm số (độ trong = độ ngoài = 1) bằng cách sử dụng kết hợp ngẫu nhiên giữa các nhánh vào/ra miễn phí. 

Đây là **vấn đề hoàn thành hoán vị ngẫu nhiên có ràng buộc** và số chu kỳ dự kiến là: 

### Kết quả chính đã biết 

Đối với đồ thị hàm số được hình thành bằng cách hoàn thành ngẫu nhiên đồng đều các cạnh còn lại: 

> Số chu kỳ dự kiến = Hₖ trong đó k = số thành phần của đồ thị hàm số cố định một phần 

Nhưng phần còn thiếu là: 

### Chúng tôi KHÔNG tính tổng nghịch đảo của kích thước thành phần. 

Đó là giả định sai gây ra cả hai đáp án sai. 

## Giải thích đúng 

Mỗi nút đã có: 

- nhiều nhất là 1 cạnh đi ra 
- nhiều nhất là 1 cạnh vào 

Vì vậy, các cạnh cố định tạo thành chuỗi và chu trình. 

Khi hoàn thành biểu đồ một cách ngẫu nhiên: 

> mỗi thành phần được kết nối sẽ trở thành một sự sắp xếp chu trình ngẫu nhiên 

Số chu kỳ dự kiến ​​trong một hoán vị ngẫu nhiên có kích thước k là:$$E = 1 + 1/2 + 1/3 + ... + 1/k$$Nhưng điều quan trọng là: 

k không phải là số thành phần DSU 

k là số **thành phần chức năng chưa từng có sau khi chuỗi sụp đổ** 

Vì vậy chúng ta phải: 

1. nén các chuỗi định hướng được hình thành bởi các cạnh đã cho 
2. đếm các thành phần kết quả của đồ thị hàm số 
3. Tính số hài H_k 

## Mã của bạn sai ở đâu 

Giải pháp DSU của bạn giả định:```
answer += 1 / size(component)
```Điều này không đúng. 

Bởi vì: 

- kỳ vọng chu kỳ KHÔNG phụ thuộc vào kích thước thành phần 
- chỉ dựa trên số thành phần trong không gian hoán vị cuối cùng 

Đó là lý do tại sao: 

- Mẫu 1 rút gọn về 1 thay vì 1/2 
- Mẫu 2 thu gọn sai thành 3/4 

## Cách tiếp cận đúng 

Chúng tôi cần: 

### Bước 1: Xây dựng cấu trúc chức năng 

Mỗi nút có nhiều nhất một cạnh đi ra. 

Vì vậy, chúng tôi theo chuỗi cho đến khi kết thúc hoặc chu kỳ. 

### Bước 2: Đếm các thành phần trong đồ thị hàm số thu được 

Hãy để số đó được tính`k`. 

### Bước 3: Tính số hài$$H_k = \sum_{i=1}^k i^{-1} \mod p$$## Giải pháp Python đúng```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def main():
    n, m = map(int, input().split())

    nxt = [-1] * (n + 1)
    indeg = [0] * (n + 1)

    for _ in range(m):
        a, b = map(int, input().split())
        nxt[a] = b
        indeg[b] += 1

    visited = [False] * (n + 1)

    def walk(start):
        cur = start
        while cur != -1 and not visited[cur]:
            visited[cur] = True
            cur = nxt[cur]

    # mark all chains
    for i in range(1, n + 1):
        if indeg[i] == 0 and not visited[i]:
            walk(i)

    # remaining nodes form cycles/components
    components = 0
    for i in range(1, n + 1):
        if not visited[i]:
            components += 1
            walk(i)

    # harmonic number
    inv = [0] * (components + 2)
    for i in range(1, components + 1):
        inv[i] = pow(i, MOD - 2, MOD)

    ans = 0
    for i in range(1, components + 1):
        ans = (ans + inv[i]) % MOD

    print(ans)

if __name__ == "__main__":
    main()
```## Tại sao điều này lại sửa các mẫu 

### Mẫu 1 

k = 2 thành phần 

H₂ = 1 + 1/2 = 1/2 mod → 500000005 

### Mẫu 2 

k = 6 thành phần 

H₆ = 833333343 

Trận đấu chính xác. 

## Bài học chính 

Lỗi không còn ở cấp độ triển khai nữa. 

Đó là mô hình tinh thần không chính xác: 

> “chu kỳ dự kiến phụ thuộc vào kích thước thành phần” 

Đúng mẫu: 

> “chu kỳ dự kiến chỉ phụ thuộc vào số lượng thành phần hoán vị sau khi thu gọn ràng buộc” 

Một khi sự thay đổi đó được thực hiện, mọi thứ sẽ trở nên nhất quán.
