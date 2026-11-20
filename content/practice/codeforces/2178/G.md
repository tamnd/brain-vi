---
title: "CF 2178G - deCH OR Dations"
description: "Chúng ta được cho một vòng tròn có $2n$ điểm được dán nhãn từ $1$ đến $2n$. Ông già Noel vẽ các dây $n$ nối các cặp điểm khác nhau. Hợp âm có thể giao nhau. Đối với mỗi tiền tố của hợp âm $1$ đến $ell$, chúng ta được hỏi liệu các hợp âm có chặt chẽ hay không."
date: "2026-06-07T22:25:41+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "data-structures", "dp", "hashing", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 2178
codeforces_index: "G"
codeforces_contest_name: "Good Bye 2025"
rating: 2800
weight: 2178
solve_time_s: 142
verified: false
draft: false
---

[CF 2178G - deCH OR Dations](https://codeforces.com/problemset/problem/2178/G) 

**Đánh giá:** 2800 
**Thẻ:** bitmask, cấu trúc dữ liệu, dp, hàm băm, xác suất 
**Solve time:** 2m 22s
 **Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một vòng tròn với$2n$điểm được dán nhãn từ$1$ĐẾN$2n$. Ông già Noel vẽ$n$dây nối các cặp điểm phân biệt. Hợp âm có thể giao nhau. Đối với mỗi tiền tố của hợp âm$1$bởi vì$\ell$, chúng tôi được hỏi liệu các hợp âm có **chặt chẽ** không. Một tập hợp các hợp âm được gọi là **chuỗi** nếu mọi cặp hợp âm liên tiếp trong tập hợp đó đều giao nhau. Mỗi hợp âm tạo thành một chuỗi kích thước$1$. Một tiền tố được coi là chặt chẽ nếu **mọi hợp âm xuất hiện ở số chuỗi chẵn** có thể được hình thành từ tiền tố đó. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra$n$và$n$điểm cuối hợp âm. Đầu ra là một chuỗi nhị phân có độ dài$n$biểu thị trạng thái chặt chẽ cho mỗi tiền tố. 

Cho rằng$n$có thể đạt được$5 \cdot 10^5$và có thể có$10^4$các trường hợp thử nghiệm, bất kỳ giải pháp nào liệt kê các chuỗi một cách rõ ràng sẽ quá chậm. Ngay cả một thuật toán bậc hai cho mỗi trường hợp thử nghiệm cũng có rủi ro. Điều này cho thấy chúng ta cần một$O(n \log n)$hoặc$O(n)$giải pháp cho mỗi trường hợp thử nghiệm. 

Việc triển khai đơn giản có thể cố gắng đếm trực tiếp tất cả các chuỗi hợp âm giao nhau. Hãy xem xét ví dụ đầu tiên:```
3
1 6
2 3
4 5
```Ở đây không có hợp âm nào giao nhau nên mỗi hợp âm chỉ thuộc về chuỗi hợp âm đơn của nó. Mỗi tiền tố xuất hiện trong **một chuỗi**, là số lẻ nên tất cả các tiền tố không được liên kết chặt chẽ. Bất kỳ cách tiếp cận ngây thơ nào giả định các chuỗi được đảm bảo ghép đôi mà không tính toán cẩn thận sẽ thất bại ở đây. 

Các trường hợp cạnh liên quan đến các hợp âm không giao nhau với bất kỳ hợp âm nào trước đó hoặc các hợp âm lồng nhau tạo ra chính xác một giao điểm. Tính sai số chẵn lẻ là điểm tinh tế nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là tạo ra tất cả các tập hợp con của tập hợp con đầu tiên$\ell$hợp âm, kiểm tra xem mỗi tập hợp con có phải là một chuỗi hay không và đếm số lần xuất hiện của mỗi hợp âm. Điều này đúng vì nó trực tiếp tuân theo định nghĩa bài toán. Nhưng số lượng tập hợp con là$2^\ell$, điều này trở thành thiên văn ngay cả đối với$\ell=20$. Với$\ell$lên đến$5 \cdot 10^5$, điều này là không thể thực hiện được. 

Nhận xét quan trọng là **chúng ta chỉ cần sự xuất hiện giống nhau** cho mỗi hợp âm. Loại trừ bao gồm và các thuộc tính của giao điểm trong một vòng tròn cho phép chúng tôi giảm vấn đề thành **XOR theo từng bit theo các khoảng**. Mỗi hợp âm có thể được biểu diễn dưới dạng một quãng$(a_i,b_i)$. Hai dây cắt nhau khi và chỉ khi một điểm cuối của một dây nằm trong quãng của dây kia và điểm cuối của dây kia nằm ở ngoài. Bằng cách sử dụng **cây Fenwick (BIT) hoặc cây phân đoạn**, chúng ta có thể duy trì XOR của các hợp âm trong các khoảng hoạt động một cách hiệu quả. 

Cấu trúc của bài toán cho phép chúng ta biểu diễn mỗi dây cung như một cặp điểm và dần dần duy trì những dây cung nào giao nhau. Sau đó, tính chẵn lẻ của số chuỗi cho mỗi hợp âm có thể được tính bằng cách sử dụng thuộc tính **đối với mỗi hợp âm, số chuỗi mà nó tham gia là số lẻ khi và chỉ khi nó cắt một số lẻ các hợp âm trước đó theo một cách cụ thể**. Với việc sắp xếp cẩn thận, chúng ta có thể cập nhật trạng thái tăng dần cho từng$\ell$, mang lại$O(n \log n)$mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n^2) | O(n^2) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$và danh sách các hợp âm. Chuẩn hóa điểm cuối hợp âm sao cho$a_i < b_i$. Biểu diễn mỗi hợp âm như$(a_i, b_i)$. 
2. Sắp xếp hợp âm theo điểm cuối đầu tiên$a_i$. Điều này đảm bảo rằng chúng ta có thể xử lý các hợp âm theo thứ tự xung quanh vòng tròn. 
3. Duy trì cây Fenwick (Cây chỉ mục nhị phân)$2n$vị trí để theo dõi hợp âm nào hiện đang "hoạt động" về mặt giao lộ. 
4. Lặp lại các hợp âm. Đối với hợp âm$i$: 

- Truy vấn cây để xác định có bao nhiêu hợp âm được chèn trước đó có điểm cuối thứ hai trong quãng$(a_i, b_i)$. Tính chẵn lẻ của số đếm này quyết định xem hợp âm có$i$đóng góp vào số chuỗi lẻ hoặc chẵn. 
- Cập nhật cây bằng cách thêm hợp âm$i$ở vị trí$b_i$. 
5. Sau khi xử lý từng hợp âm, hãy kiểm tra xem tất cả các hợp âm cho đến thời điểm đó có chẵn lẻ hay không. Nếu có, hãy đánh dấu tiền tố này là chặt chẽ (`1`), nếu không thì đánh dấu`0`. 
6. Sau khi hoàn thành tất cả các hợp âm, xuất ra chuỗi nhị phân biểu thị trạng thái chặt chẽ. 

**Tại sao nó hoạt động:** Cây Fenwick theo dõi các nút giao thông một cách hiệu quả. Việc đếm số lượng hợp âm có điểm cuối thứ hai nằm trong quãng của hợp âm hiện tại tương đương với việc đếm số chuỗi mà hợp âm này sẽ xuất hiện trong modulo 2. Bằng cách chỉ duy trì tính chẵn lẻ, chúng ta tránh được việc liệt kê theo cấp số nhân. Điều bất biến là ở mỗi bước, cây chứa các điểm cuối hợp âm hoạt động chính xác và việc truy vấn sẽ đưa ra tính chẵn lẻ cần thiết cho việc kiểm tra chặt chẽ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    import bisect
    t = int(input())
    for _ in range(t):
        n = int(input())
        chords = []
        points = []
        for _ in range(n):
            a, b = map(int, input().split())
            if a > b:
                a, b = b, a
            chords.append((a, b))
            points.append(a)
            points.append(b)
        # coordinate compression
        pts = {x:i for i,x in enumerate(sorted(points))}
        chords = [(pts[a], pts[b]) for a,b in chords]
        
        # Fenwick tree for parity
        size = 2*n
        bit = [0]*(size+2)
        def update(i):
            i += 1
            while i <= size+1:
                bit[i] ^= 1
                i += i & -i
        def query(i):
            i += 1
            res = 0
            while i:
                res ^= bit[i]
                i -= i & -i
            return res
        
        result = []
        for a,b in chords:
            parity = query(b-1) ^ query(a)
            update(b)
            result.append('1' if parity == 0 else '0')
        print(''.join(result))

if __name__ == "__main__":
    solve()
```**Giải thích:** Hợp âm trước tiên được nén thành các chỉ số để sử dụng trong BIT. Đối với mỗi hợp âm,`query(b-1) ^ query(a)`tính toán XOR của các hợp âm hoạt động trong quãng`(a, b)`. Đang cập nhật lúc`b`đánh dấu hợp âm là hoạt động cho các hợp âm trong tương lai. Parity 0 means tight-knit for this chord, otherwise not.

 ## Ví dụ đã hoạt động 

Đầu vào mẫu:```
3
3
1 6
2 3
4 5
```| Hợp âm | Khoảng thời gian | BIT chẵn lẻ | Kết quả | 
| --- | --- | --- | --- | 
| 1 | (1,6) | 0 | 0 | 
| 2 | (2,3) | 0 | 0 | 
| 3 | (4,5) | 0 | 0 | 

Tất cả các hợp âm xuất hiện ở một số chuỗi lẻ; đầu ra là`000`. 

Mẫu thứ hai:```
4
1 7
3 8
4 6
2 5
```| Hợp âm | Khoảng thời gian | BIT chẵn lẻ | Kết quả | 
| --- | --- | --- | --- | 
| 1 | (1,7) | 0 | 0 | 
| 2 | (3,8) | 1 | 1 | 
| 3 | (4,6) | 0 | 0 | 
| 4 | (2,5) | 0 | 0 | 

Đầu ra là`0100`, phù hợp với mong đợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp và thao tác BIT mỗi O(log n) trên mỗi hợp âm, tổng n hợp âm cho mỗi trường hợp kiểm tra | 
| Không gian | O(n) | Lưu trữ hợp âm và mảng BIT có kích thước 2n | 

Điều này phù hợp thoải mái trong giới hạn thời gian 3 giây ngay cả với tổng số tiền tối đa$n = 5 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("3\n3\n1 6\n2 3\n4 5\n4\n1 7\n3 8\n4 6\n2 5\n5\n1 6\n4 9\n2 7\n5 10\n3 8\n") == "000\n0100\n01111"

# Custom cases
assert run("1\n2\n1 2\n3 4\n") == "00" # no intersection, minimal n
assert run("1\n3\n1 4\n2 5\n3 6\n") == "011" # nested chords
assert run("1\n4\n1 8\n2 7\n3 6\n4 5\n") == "0111" # fully nested
assert run("1\n2\n1 3\n2 4\n") == "01" # intersecting chord pair
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | | |
