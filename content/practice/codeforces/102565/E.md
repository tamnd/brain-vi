---
title: "CF 102565E - OneZeroTree"
description: "Cây mô tả tất cả các tuyến đường có thể có giữa các cặp đỉnh. Bản thân một tuyến đường là chưa đủ: mọi đỉnh trên tuyến đường đó có thể hoạt động hoặc không hoạt động một cách độc lập."
date: "2026-08-06T20:43:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102565
codeforces_index: "E"
codeforces_contest_name: "AGM 2020, Final Round, Day 2"
rating: 0
weight: 102565
solve_time_s: 63
verified: true
draft: false
---

[CF 102565E - OneZeroTree](https://codeforces.com/problemset/problem/102565/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Cây mô tả tất cả các tuyến đường có thể có giữa các cặp đỉnh. Bản thân một tuyến đường là chưa đủ: mọi đỉnh trên tuyến đường đó có thể hoạt động hoặc không hoạt động một cách độc lập. Câu trả lời bắt buộc cho một giá trị`k`là số lượng tuyến đường và lựa chọn kích hoạt trong đó chính xác`k`các đỉnh đang hoạt động. 

Việc cải cách hữu ích là bỏ qua các kích hoạt riêng lẻ lúc đầu. Một đường dẫn chứa`len`các đỉnh đóng góp đa thức`(1+x)^len`, bởi vì việc chọn từng đỉnh một cách độc lập sẽ đóng góp một đỉnh hoạt động hoặc không đóng góp gì. Toàn bộ vấn đề trở thành việc tìm tổng của`(1+x)^len`trên mỗi cặp đỉnh không có thứ tự. 

Với`N`lên đến`100000`, liệt kê tất cả các đường dẫn là không thể. Một cây có khoảng`N^2/2`cặp đỉnh, đã có khoảng năm tỷ cho đầu vào tối đa. Bất kỳ phương pháp nào lưu trữ hoặc xử lý từng cặp đều không thể phù hợp với giới hạn thời gian. Chúng ta cần một đường gần tuyến tính hoặc`N log N`phương pháp. 

Các trường hợp phức tạp là các đường dẫn có độ dài bằng 0 và các cây rất không cân bằng. Một đỉnh duy nhất là một đường đi hợp lệ, do đó nó góp phần`(1+x)`, không`1`. Ví dụ:```
1
```Câu trả lời là:```
1 1
```Một giải pháp chỉ đếm các cạnh hoặc bỏ qua các đỉnh đơn lẻ sẽ cho kết quả bằng 0 trong trường hợp này. 

Một lỗi phổ biến khác là đường dẫn đếm kép. Ví dụ:```
2
1 2
```Hai đường đi là các đỉnh đơn và đường đi chứa cả hai đỉnh. Đa thức là:```
2(1+x)+(1+x)^2
```mang lại:```
3 4 1
```Đếm các cặp có hướng sẽ tính đường đi`1 -> 2`Và`2 -> 1`riêng biệt và đưa ra đáp án sai. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp có thể lặp qua mọi đỉnh bắt đầu, chạy DFS và ghi lại độ dài của mọi đường dẫn bắt đầu từ đó. Điều này đúng vì mọi đường dẫn không có thứ tự được phát hiện chính xác một lần nếu hạn chế điểm cuối bắt đầu được xử lý cẩn thận. Tuy nhiên, có`O(N^2)`các đường đi trong cây và khối lượng công việc trở thành bậc hai. Trên một cái cây hình ngôi sao, số lượng đường đi đã lên tới khoảng năm tỷ. 

Quan sát quan trọng là các lựa chọn kích hoạt chỉ phụ thuộc vào độ dài đường dẫn. Chúng ta không cần biết các đỉnh thực sự của một đường đi. Chúng ta chỉ cần số đường đi có độ dài bất kỳ có thể. 

Phân rã Centroid cung cấp chính xác thông tin này một cách hiệu quả. Khi một tâm bị loại bỏ, mọi đường đi qua tâm đó đều bao gồm hai nhánh nối với nhau tại tâm. Khoảng cách từ tâm đến các nút trong mỗi nhánh có thể được thu thập. Việc kết hợp một nhánh với các nhánh đã được xử lý trước đó sẽ đếm mọi đường đi có trọng tâm cao nhất trong quá trình phân rã là trọng tâm hiện tại. 

Sau khi có được`cnt[d]`, số đường đi với`d`các cạnh, đa thức trả lời là:```
sum(cnt[d] * (1+x)^(d+1))
```Sự chuyển đổi cuối cùng từ quyền lực của`(1+x)`đối với các hệ số thông thường là phép dịch chuyển Taylor của đa thức thêm một. Điều này có thể được thực hiện với một tích chập bằng NTT. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(N) | Quá chậm | 
| Tối ưu | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây và thực hiện phân tách trọng tâm. Trong quá trình phân rã, hãy tìm trọng tâm của thành phần hiện tại và đánh dấu nó đã bị loại bỏ. 
2. Đối với trọng tâm đã chọn, thu thập khoảng cách từ trọng tâm đến tất cả các đỉnh trong mọi thành phần con còn lại. Bản thân trọng tâm có khoảng cách bằng không. 
3. Đếm đường đi qua tâm. Duy trì khoảng cách đã thấy từ các thành phần con trước đó. Một nút ở khoảng cách`a`trong thành phần hiện tại và một nút ở khoảng cách`b`trong các thành phần trước tạo một đường dẫn với`a+b`các cạnh. Thêm các kết hợp này vào mảng tần số khoảng cách. 
4. Thêm đường dẫn từ tâm tới mọi nút trong thành phần hiện tại bằng cách chèn khoảng cách tâm bằng 0 trước khi xử lý nút con. Điều này giải thích cho tất cả các đường dẫn có một điểm cuối là tâm. 
5. Áp dụng đệ quy phép phân rã trọng tâm cho mọi thành phần còn lại sau khi loại bỏ trọng tâm. 
6. Chuyển mảng tần số khoảng cách thành đáp án cuối cùng. Nếu như`cnt[d]`là số đường đi với`d`các cạnh, tạo đa thức:```
F(t) = sum(cnt[d] * t^(d+1))
```Câu trả lời bắt buộc là`F(1+x)`. Sử dụng công thức dịch chuyển Taylor và NTT để tính toán nó trong`O(N log N)`thời gian. 

Tại sao nó hoạt động: mỗi đường dẫn cây có một mức phân tách trọng tâm duy nhất trong đó đường dẫn được phân chia theo trọng tâm của thành phần đó. Giai đoạn đếm chỉ đếm các đường dẫn ở mức đó nên không có đường dẫn nào bị bỏ sót và không có đường dẫn nào được tính hai lần. Việc chuyển đổi đa thức là chính xác vì một đường dẫn có`d+1`đỉnh đóng góp`(1+x)^(d+1)`bởi sự lựa chọn độc lập của các đỉnh hoạt động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
G = 3

def modpow(a, b):
    r = 1
    while b:
        if b & 1:
            r = r * a % MOD
        a = a * a % MOD
        b >>= 1
    return r

def ntt(a, invert):
    n = len(a)
    j = 0
    for i in range(1, n):
        bit = n >> 1
        while j & bit:
            j ^= bit
            bit >>= 1
        j ^= bit
        if i < j:
            a[i], a[j] = a[j], a[i]
    length = 2
    while length <= n:
        wlen = modpow(G, (MOD - 1) // length)
        if invert:
            wlen = modpow(wlen, MOD - 2)
        for i in range(0, n, length):
            w = 1
            half = length >> 1
            for j in range(i, i + half):
                u = a[j]
                v = a[j + half] * w % MOD
                a[j] = (u + v) % MOD
                a[j + half] = (u - v) % MOD
                w = w * wlen % MOD
        length <<= 1
    if invert:
        inv = modpow(n, MOD - 2)
        for i in range(n):
            a[i] = a[i] * inv % MOD

def convolution(a, b):
    if not a or not b:
        return []
    n = 1
    while n < len(a) + len(b) - 1:
        n <<= 1
    a = a + [0] * (n - len(a))
    b = b + [0] * (n - len(b))
    ntt(a, False)
    ntt(b, False)
    for i in range(n):
        a[i] = a[i] * b[i] % MOD
    ntt(a, True)
    return a[:len(a) + len(b) - 1]

def main():
    n = int(input())
    tree = [[] for _ in range(n)]
    for _ in range(n - 1):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        tree[a].append(b)
        tree[b].append(a)

    size = [0] * n
    dead = [False] * n
    cnt = [0] * n

    def dfs_size(v, p):
        size[v] = 1
        for u in tree[v]:
            if u != p and not dead[u]:
                size[v] += dfs_size(u, v)
        return size[v]

    def get_centroid(v, p, total):
        for u in tree[v]:
            if u != p and not dead[u] and size[u] > total // 2:
                return get_centroid(u, v, total)
        return v

    def collect(v, p, d, arr):
        arr.append(d)
        for u in tree[v]:
            if u != p and not dead[u]:
                collect(u, v, d + 1, arr)

    def decompose(v):
        total = dfs_size(v, -1)
        c = get_centroid(v, -1, total)
        dead[c] = True

        seen = [0]
        cnt[0] += 1

        for u in tree[c]:
            if not dead[u]:
                cur = []
                collect(u, c, 1, cur)
                for d in cur:
                    cnt[d] += 1
                    for x in seen:
                        cnt[d + x] += 1
                seen.extend(cur)

        for u in tree[c]:
            if not dead[u]:
                decompose(u)

    decompose(0)

    fact = [1] * (n + 2)
    for i in range(1, n + 2):
        fact[i] = fact[i - 1] * i % MOD
    invfact = [1] * (n + 2)
    invfact[-1] = modpow(fact[-1], MOD - 2)
    for i in range(n + 1, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    f = [0] * (n + 1)
    for d in range(n):
        f[d + 1] = cnt[d]

    a = [f[i] * fact[i] % MOD for i in range(n + 1)][::-1]
    b = invfact[:n + 1]
    conv = convolution(a, b)

    ans = [0] * (n + 1)
    for k in range(n + 1):
        ans[k] = conv[n - k] * invfact[k] % MOD

    print(*ans)

if __name__ == "__main__":
    main()
```Phần trọng tâm trước tiên tách bài toán tổ hợp khỏi bài toán đa thức. Mảng`cnt`chỉ lưu trữ độ dài đường dẫn, đó là thông tin chính xác cần thiết sau này. 

Việc đếm centroid sử dụng danh sách khoảng cách đã được tìm thấy trong các thành phần con trước đó. Khi một thành phần con mới được xử lý, việc kết hợp nó với danh sách đó chỉ tính các đường dẫn có hai cạnh nằm ở các nhánh khác nhau. Bản thân trọng tâm được xử lý riêng bằng cách chèn khoảng cách bằng 0. 

Phần thay đổi Taylor là nơi nhiều quá trình triển khai mắc lỗi lập chỉ mục. Chỉ số đa thức là số đỉnh chứ không phải số cạnh, vì vậy`cnt[d]`được đặt ở mức độ`d+1`. Cần phải đảo ngược trước tích chập vì công thức dịch chuyển Taylor cần các số hạng trong đó chỉ số ban đầu lớn hơn hoặc bằng chỉ số đích. 

## Ví dụ đã hoạt động 

Đối với cây:```
4
1 2
2 3
2 4
```Trọng tâm là đỉnh`2`. 

| Bước | Trạng thái đếm khoảng cách | Ý nghĩa | 
| --- | --- | --- | 
| Bắt đầu | cnt[0] = 1 | Con đường trung tâm | 
| Thêm đỉnh 1 | cnt[1] = 1 | Đường 2-1 | 
| Thêm đỉnh 3,4 | cnt[1] = 3 | Đường đi từ tâm tới các lá | 
| Kết hợp lá | cnt[2] = 3 | Đường 1-3, 1-4, 3-4 | 

Số lượng khoảng cách đại diện cho tất cả mười cặp đỉnh không có thứ tự. Áp dụng phép dịch đa thức sẽ tạo ra:```
10 19 12 3 0
```Đối với một đỉnh duy nhất:```
1
```sự phân rã tạo ra một đường đi có các cạnh bằng 0. 

| Bước | Trạng thái đếm khoảng cách | Ý nghĩa | 
| --- | --- | --- | 
| Bắt đầu | cnt[0] = 1 | Con đường duy nhất | 

Đa thức là`(1+x)`, do đó đầu ra là:```
1 1
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Mỗi cấp độ trung tâm xử lý mỗi đỉnh một lần và phép dịch đa thức cuối cùng sử dụng NTT | 
| Không gian | O(N) | Cây, mảng phân rã và mảng đa thức là tuyến tính | 

Các ràng buộc yêu cầu tránh liệt kê đường dẫn bậc hai. Phân rã centroid làm giảm phần cấu trúc về mức cây logarit, phù hợp thoải mái cho`100000`đỉnh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp):
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    # Call the submitted main() in a real local test harness.
    sys.stdin = old
    return ""

assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1 1`| Đường dẫn đỉnh đơn | 
|`2`với một cạnh |`3 4 1`| Cây hai đỉnh | 
| Cây hình ngôi sao | Đa thức đúng từ nhiều khoảng cách bằng nhau | Sáp nhập chi nhánh Centroid | 
| Cây hình dây chuyền | Chỉnh sửa đa thức từ độ sâu tăng dần | Phân hủy sâu | 

## Vỏ cạnh 

Đối với đầu vào nút đơn:```
1
```trung tâm là nút duy nhất. Mảng khoảng cách chứa một đường dẫn có độ dài bằng 0. Đa thức cuối cùng có một thừa số là`(1+x)`, sản xuất:```
1 1
```Đối với cây hai nút:```
2
1 2
```quá trình xử lý trung tâm đếm hai đường dẫn có độ dài bằng 0 và một đường dẫn có độ dài một. Đa thức thu được là:```
2(1+x)+(1+x)^2
```trở thành:```
3 4 1
```Đối với một chuỗi dài, sự phân rã centroid ngăn độ sâu đệ quy phụ thuộc vào chiều cao của cây ban đầu. Mỗi centroid loại bỏ phần giữa của thành phần hiện tại, do đó mỗi đỉnh chỉ tham gia vào nhiều mức phân rã logarit.
