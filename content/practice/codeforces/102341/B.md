---
title: "CF 102341B - Bóng đèn"
description: "Đồ thị bao gồm (n) lớp, mỗi lớp chứa chính xác (k) đỉnh. Các cạnh chỉ đi từ lớp (i) đến lớp (i+1). Dây leo là một đường đi có hướng và hai dây leo không thể có chung một đỉnh hoặc một cạnh."
date: "2026-08-14T05:18:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102341
codeforces_index: "B"
codeforces_contest_name: "Radewoosh+mnbvmar Contest (supported by AIM Tech)"
rating: 0
weight: 102341
solve_time_s: 331
verified: true
draft: false
---

[CF 102341B - Bulbasaur](https://codeforces.com/problemset/problem/102341/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 31 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đồ thị bao gồm (n) lớp, mỗi lớp chứa chính xác (k) đỉnh. Các cạnh chỉ đi từ lớp (i) đến lớp (i+1). Dây leo là một đường đi có hướng và hai dây leo không thể có chung một đỉnh hoặc một cạnh. Đối với hai lớp (i<j), (f(i,j)) là số lượng tối đa các đường dẫn có hướng tách rời đỉnh từ lớp (i) đến lớp (j). Nhiệm vụ là tính tổng giá trị này trên mỗi cặp lớp riêng biệt. 

Đối với (i,j) cố định, đây là bài toán luồng cực đại với dung lượng đỉnh bằng một. Chia mỗi đỉnh thành một đỉnh trong và một đỉnh ngoài với cạnh chứa một cạnh ở giữa chúng. Các đường hầm nhận được dung lượng vô hạn và một siêu nguồn được kết nối với mọi đỉnh của lớp (i), trong khi mọi đỉnh của lớp (j) đều kết nối với một siêu nguồn. Luồng tối đa thu được chính xác là (f(i,j)). 

Ràng buộc (k\leq 9) là chìa khóa. Số lượng lớp có thể lớn tới (40000), do đó, bất kỳ số bậc hai nào trong (n) đều đã quá lớn. Có khoảng (8\cdot 10^8) cặp lớp khi (n=40000), vì vậy việc giải quyết rõ ràng vấn đề luồng cho mỗi cặp là không thể. Mặt khác, (2^k\leq512), làm cho lập trình động tập hợp con trở nên thiết thực. Cách tiếp cận được chấp nhận khai thác chính xác sự bất đối xứng này, thay thế số khoảng bậc hai bằng (n) chuyển tiếp trên (2^k) tập hợp con. Độ phức tạp thu được là (O(nk^2 2^k)). 

Có một số trường hợp ranh giới rất dễ bị xử lý sai. Với (k=1), một đường hầm bị thiếu sẽ ngay lập tức khiến mỗi khoảng thời gian đi qua nó có lưu lượng bằng 0. Ví dụ,```
2 1
0
```có câu trả lời`0`. Một giải pháp giả định mỗi cặp lớp lân cận đóng góp ít nhất một đường dẫn sẽ trả về không chính xác`1`. 

Trường hợp ngược lại cũng hữu ích. Với (k=1) và mọi đường hầm hiện diện,```
3 1
1
1
```mỗi cặp lớp có một đường dẫn, vì vậy câu trả lời là`3`. Một giải pháp chỉ đếm các đường đi có độ dài tối đa sẽ bỏ lỡ khoảng thời gian từ lớp 1 đến lớp 2 và quay trở lại`1`. 

Trường hợp tinh tế thứ hai xảy ra khi một đỉnh trong lớp trung gian không thể sử dụng được. Trong mẫu đầu tiên, ma trận cuối cùng chứa một hàng bằng 0, do đó chỉ có ba đường dẫn có thể đi qua từ lớp 3 đến lớp 4. Do đó (f(3,4)=3), mặc dù các lớp trước đó chấp nhận bốn đường dẫn rời nhau. Việc xử lý từng lớp một cách độc lập và lấy số đỉnh tối thiểu mà không kiểm tra kết nối sẽ không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xem xét mọi khoảng ([i,j]), xây dựng mạng luồng có công suất đỉnh của nó và chạy thuật toán luồng cực đại. Điều này đúng vì định lý Menger xác định số lượng tối đa các đường đi từ đỉnh đến điểm cắt đỉnh tối thiểu. Sau khi tách các đỉnh, bài toán trở thành luồng có điện dung cạnh thông thường. 

Vấn đề là số lượng khoảng thời gian. Có (\Theta(n^2)) trong số đó. Một khoảng có độ dài (L) chứa (O(Lk)) đỉnh và (O(Lk^2)) các cạnh đường hầm có thể có. Ngay cả việc triển khai theo phong cách Ford-Fulkerson cũng cần tới (k) lần tăng cường, mang lại (O(Lk^3)) công việc trong một khoảng thời gian. Tổng tất cả các khoảng, tổng số là (O(n^3k^3)). Đối với (n=40000), riêng tổng độ dài các khoảng là 

[ 
\sum_{1\leq i<j\leq n}(j-i) 
=\frac{n(n-1)(n+1)}6, 
] 

tức là khoảng (1,07\cdot10^{13}). Với (k=9), hệ số (k^3) tương ứng sẽ cho ra khoảng (7,8\cdot10^{15}) đơn vị xử lý cạnh trước cả khi tính đến chi phí luồng. 

Lực lượng vũ phu hoạt động vì mọi giá trị luồng riêng lẻ đều nhỏ, nhiều nhất là (k), nhưng nó không thành công vì về cơ bản nó giải quyết cùng một vấn đề nhỏ riêng biệt cho mỗi cặp lớp. 

Quan sát chính là sử dụng công thức cắt nhỏ thay vì tính toán trực tiếp giá trị luồng. Đối với điểm cuối bên phải cố định (r), hãy xác định, với mọi (c\in[1,k]), lớp lớn nhất (L_c) sao cho 

[ 
f(L_c,r)\geq c. 
] 

Bởi vì việc mở rộng một khoảng sang trái chỉ có thể làm cho việc tập hợp các đường đi rời rạc trở nên khó khăn hơn nên tập hợp các điểm cuối bên trái hỗ trợ các đường dẫn (c) là một hậu tố. Do đó, số lượng điểm cuối bên trái (i<r) với (f(i,r)\geq c) chỉ đơn giản là (r-L_c). Kể từ khi 

[ 
f(i,r)=\sum_{c=1}^{k[f(i,r)\geq c], 
] 

chúng tôi nhận được 

\sum_{c=1}^{k}(r-L_c). 
] 

Vì vậy, toàn bộ vấn đề trở thành việc duy trì các vị trí ngưỡng (k) này trong khi quét biểu đồ từ trái sang phải. 

Khó khăn còn lại là mô tả đường cắt tối thiểu mà không lưu trữ rõ ràng tất cả các đỉnh của nó. Vì mỗi lớp chỉ chứa (k\leq9) đỉnh, biểu thị các đỉnh vẫn có thể truy cập được trong lớp hiện tại bằng mặt nạ bit (S). Đối với mỗi tập hợp con (S) và mỗi ngưỡng cắt (c), chúng tôi duy trì điểm cuối bên trái lớn nhất có thể mà phần cắt có kích thước yêu cầu để lại chính xác (S). Di chuyển qua một ma trận sẽ biến đổi (S) thành vùng lân cận của nó và việc xóa một đỉnh khỏi tập hợp có thể tiếp cận sẽ làm tăng số lượng đỉnh được sử dụng để cắt. Cả hai thao tác có thể được thực hiện với tập hợp con DP. 

Đây là tính năng nén trạng thái có chiều rộng nhỏ tương tự đằng sau giải pháp tiêu chuẩn được chấp nhận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^3k^3)) | (O(nk^2)) cho mạng một luồng | Quá chậm | 
| Tối ưu | (O(nk^2 2^k)) | (O(k2^k)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu thị mọi tập hợp con của lớp hiện tại bằng mặt nạ bit (k). Bit được đặt có nghĩa là đỉnh tương ứng vẫn có thể tiếp cận được từ phía bên trái của vết cắt. 
2. Đối với mọi mặt nạ (S) và mọi (c\in[0,k]), hãy duy trì`dp[S][c]`. Giá trị của nó là một chỉ mục lớp. Nó đại diện cho điểm cuối xa nhất bên trái mà sau khi trả chi phí cắt giảm tương ứng, bộ có thể truy cập hiện tại có thể chính xác là (S) trong khi vẫn giữ lại ít nhất (c) đơn vị kết nối. Thông tin duy nhất chúng ta cần từ trạng thái này là điểm cuối bên trái lớn nhất có thể, bởi vì đó chính xác là yếu tố quyết định số lượng khoảng đóng góp cho câu trả lời. 
3. Ban đầu chúng ta đang ở lớp 1. Nếu chính xác các đỉnh trong (S) vẫn có thể tiếp cận được thì các đỉnh (k-|S|) khác có thể bị loại bỏ. Điều này đưa ra ngưỡng ban đầu 
[ 
dp[S][c]=1 
] 
bất cứ khi nào (c\leq k-|S|). Ngưỡng lớn hơn là không thể, vì vậy những mục đó nhận được giá trị trọng điểm (n+1). 
4. Đọc ma trận giữa lớp hiện tại và lớp tiếp theo. Đối với mỗi đỉnh của lớp cũ, hãy lưu trữ mặt nạ bit của các lớp lân cận đi của nó. Đối với một tập con (S), toàn bộ vùng lân cận của nó là hợp của các mặt nạ đó. 
5. Trước khi xóa các đỉnh trong lớp mới, hãy chuyển mọi trạng thái từ (S) sang vùng lân cận của nó (N(S)). Khi một số tập hợp con cũ tạo ra cùng một tập hợp con mới, trạng thái của chúng mô tả các cách khác nhau để đạt được cùng một tập hợp có thể truy cập được, vì vậy chúng tôi hợp nhất chúng bằng cách giữ ngưỡng tốt nhất cho mọi (c). 
6. Bây giờ xử lý việc xóa đỉnh bên trong lớp mới. Nếu tập có thể truy cập là (S), việc xóa một đỉnh (u\in S) sẽ thay đổi trạng thái thành (S\setminus{u}) và tăng số đỉnh bị xóa lên một. Trong DP đây là quá trình chuyển đổi tập hợp con một bit: 
[ 
dp[S\setminus{u}][c] 
\leftarrow 
\max(dp[S\setminus{u}][c],dp[S][c-1]). 
] 
Việc áp dụng điều này cho mỗi bit đã đặt sẽ thực hiện toàn bộ quá trình cắt cho lớp mới. 
7. Tập trống là trạng thái không có đỉnh nào trong lớp hiện tại có thể truy cập được từ điểm cuối bên trái. Như vậy`dp[0][c]`chính xác là điểm cuối bên trái lớn nhất mà việc cắt tương ứng với ngưỡng (c) có thể ngắt kết nối điểm cuối đó khỏi lớp hiện tại. 
8. Với mọi (c=1,\ldots,k), hãy cộng 
[ 
r-dp[0][c] 
] 
để trả lời. Điều này đếm các điểm cuối bên trái (i<r) mà tại đó tồn tại ít nhất (c) đường dẫn rời nhau. Tổng kết này (c) chính xác là tổng của các giá trị luồng kết thúc ở lớp (r). 
9. Tiếp tục đi qua tất cả (n-1) ma trận. Chỉ cần hai lớp DP, do đó mức tiêu thụ bộ nhớ vẫn còn (O(k2^k)). 

Bất biến về tính đúng đắn là sau lớp xử lý (r), mọi trạng thái`dp[S][c]`lưu trữ điểm cuối bên trái xa nhất tương thích với đường cắt để lại chính xác (S) có thể truy cập được ở lớp (r) và vẫn hỗ trợ ngưỡng (c). Quá trình chuyển đổi vùng lân cận chính xác là hiệu ứng của việc vượt qua lớp tiếp theo mà không cắt một đỉnh, trong khi quá trình chuyển đổi tập hợp con liệt kê mọi lựa chọn có thể có của các đỉnh cắt trong lớp đó. Do đó, trạng thái tập trống xem xét mọi lần cắt đỉnh có thể. Theo định lý cắt cực tiểu luồng cực đại, ngưỡng của nó tương đương với số đường đi tách đỉnh tương ứng. Cuối cùng, tính đơn điệu ở điểm cuối bên trái biến mỗi ngưỡng thành số đếm đơn giản (r-dp[0][c]), do đó mỗi (f(i,r)) được tính chính xác một lần cho mỗi đơn vị luồng mà nó đóng góp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, k = map(int, input().split())

    size = 1 << k
    inf = n + 1

    popcnt = [0] * size
    for mask in range(1, size):
        popcnt[mask] = popcnt[mask >> 1] + (mask & 1)

    bits = [[] for _ in range(size)]
    for mask in range(1, size):
        x = mask
        cur = bits[mask]
        while x:
            b = x & -x
            cur.append(b.bit_length() - 1)
            x ^= b

    dp = [[inf] * (k + 1) for _ in range(size)]

    for mask in range(size):
        lim = k - popcnt[mask]
        row = dp[mask]
        for c in range(lim + 1):
            row[c] = 1

    ans = 0

    def merge(dst, src):
        a0 = dst[0]
        b0 = src[0]

        if a0 <= b0:
            base = a0
            for c in range(1, k + 1):
                x = src[c]
                if x == b0:
                    x = base
                if x > dst[c]:
                    dst[c] = x
        else:
            base = b0
            old0 = a0
            for c in range(1, k + 1):
                x = dst[c]
                if x == old0:
                    x = base
                y = src[c]
                if y > x:
                    x = y
                dst[c] = x
            dst[0] = base

    def merge_shift(dst, src):
        a0 = dst[0]
        b0 = src[0]

        if a0 <= b0:
            base = a0
            for c in range(1, k + 1):
                x = src[c - 1]
                if x == b0:
                    x = base
                if x > dst[c]:
                    dst[c] = x
        else:
            base = b0
            old0 = a0
            for c in range(1, k + 1):
                x = dst[c]
                if x == old0:
                    x = base
                y = src[c - 1]
                if y > x:
                    x = y
                dst[c] = x
            dst[0] = base

    for layer in range(2, n + 1):
        to = [0] * k

        for u in range(k):
            s = input().strip()
            while not s:
                s = input().strip()

            mask = 0
            for v, ch in enumerate(s):
                if ch == '1':
                    mask |= 1 << v
            to[u] = mask

        # neigh[mask] = union of all outgoing neighbors of vertices in mask.
        neigh = [0] * size
        for mask in range(1, size):
            b = mask & -mask
            u = b.bit_length() - 1
            neigh[mask] = neigh[mask ^ b] | to[u]

        nxt = [[inf] * (k + 1) for _ in range(size)]

        # Cross the current matrix without deleting a vertex.
        for mask in range(size):
            merge(nxt[neigh[mask]], dp[mask])

        dp = nxt

        # Delete vertices in the new layer.
        for mask in range(size - 1, 0, -1):
            src = dp[mask]
            for u in bits[mask]:
                merge_shift(dp[mask ^ (1 << u)], src)

        # A cut cannot need a left endpoint beyond the current layer.
        for mask in range(size):
            lim = k - popcnt[mask]
            row = dp[mask]
            for c in range(lim + 1):
                if row[c] > layer:
                    row[c] = layer

        empty = dp[0]
        for c in range(1, k + 1):
            ans += layer - empty[c]

    print(ans)

if __name__ == "__main__":
    main()
```Phần đầu tiên của việc thực hiện tính toán trước`popcnt`và các bit được thiết lập của mỗi mặt nạ. Vì cấu trúc tập hợp con giống nhau được sử dụng ở mọi lớp nên thực hiện việc này một lần sẽ tránh được các mặt nạ giải mã lặp đi lặp lại. 

các`to`mảng lưu trữ các lân cận đi của một đỉnh dưới dạng bitmask. các`neigh`mảng sau đó được xây dựng theo phép lặp bit có ý nghĩa nhỏ nhất tiêu chuẩn. Nếu như`b`là một chút`mask`, lân cận của`mask`là lân cận của`mask ^ b`hợp các lân cận của đỉnh đó. Điều này làm giảm tính toán lân cận cho tất cả (2^k) tập hợp con xuống (O(2^k)) mỗi lớp. 

các`merge`thường nhật đáng được quan tâm đặc biệt. Hàng DP không chỉ đơn giản là tập hợp các giá trị độc lập. Mục nhập thứ 0 của nó xác định ranh giới của khoảng ngưỡng được biểu thị. Khi kết hợp hai cách để có được cùng một tập hợp con, các mục bằng ranh giới thứ 0 của hàng nguồn phải được chuyển sang ranh giới tốt hơn trước khi lấy mức tối đa theo thành phần. Đây chính xác là quá trình chuẩn hóa được thực hiện bởi quá trình chuyển đổi tham chiếu.`merge_shift`là thao tác tương tự sau khi xóa một đỉnh. Thay vì xây dựng một mảng dịch chuyển tạm thời, nó xử lý trực tiếp mục nhập nguồn`src[c - 1]`như mục nhập của điểm đến`c`. Việc tránh phân bổ hàng tạm thời là vấn đề quan trọng vì quá trình chuyển đổi được thực hiện cho mọi tập hợp con và mọi bit được đặt. 

Thứ tự mặt nạ giảm dần trong giai đoạn xóa là có chủ ý. Một trạng thái cho`mask`phải được sử dụng trước khi các trạng thái đại diện cho tập hợp con lớn hơn có thể ghi đè thông tin cần thiết cho quá trình chuyển đổi hiện tại. Hoạt động tương đương của mã nguồn xử lý các mặt nạ từ mặt nạ đầy đủ trở xuống. 

Câu trả lời được lưu trữ dưới dạng số nguyên có độ chính xác tùy ý của Python, do đó không có vấn đề tràn. Câu trả lời lớn nhất có thể là (k\binom n2), có thể dễ dàng biểu diễn bằng số nguyên Python. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Ba ma trận là```
1000
1100
0110
0011

0100
1100
0010
0001

1000
1100
0000
0011
```Các giá trị lưu lượng thực tế là 

[ 
f(1,2)=4,\quad f(1,3)=4,\quad f(1,4)=3, 
] 

[ 
f(2,3)=4,\quad f(2,4)=3,\quad f(3,4)=3. 
] 

DP không cần lưu trữ sáu giá trị này một cách riêng lẻ. Đối với mỗi điểm cuối bên phải (r), nó lưu trữ ranh giới ngưỡng cho mọi (c). 

| Lớp bên phải (r) | (c) | Điểm cuối bên trái với (f(i,r)\ge c) |`dp[0][c]`| Đóng góp (r-dp[0][c]) | 
| --- | --- | --- | --- | --- | 
| 2 | 1 | 1 | 1 | 1 | 
| 2 | 2 | 1 | 1 | 1 | 
| 2 | 3 | 1 | 1 | 1 | 
| 2 | 4 | 1 | 1 | 1 | 
| 3 | 1 | 1,2 | 1 | 2 | 
| 3 | 2 | 1,2 | 1 | 2 | 
| 3 | 3 | 1,2 | 1 | 2 | 
| 3 | 4 | 1,2 | 1 | 2 | 
| 4 | 1 | 1,2,3 | 1 | 3 | 
| 4 | 2 | 1,2,3 | 1 | 3 | 
| 4 | 3 | 1,2,3 | 1 | 3 | 
| 4 | 4 | không | 4 | 0 | 

Những đóng góp là (4+8+9=21). Hàng cuối cùng thực hiện ranh giới nơi bốn con đường rời rạc không còn tồn tại, trong khi ba con đường vẫn tồn tại. 

### Mẫu 2 

Các ma trận là```
000
100
010

000
100
010

010
101
010

010
101
010
```Các giá trị dòng kết quả là 

[ 
f(1,2)=2, 
] 

[ 
f(1,3)=1,\quad f(2,3)=2, 
] 

[ 
f(1,4)=1,\quad f(2,4)=2,\quad f(3,4)=2, 
] 

[ 
f(1,5)=1,\quad f(2,5)=2,\quad f(3,5)=2,\quad f(4,5)=2. 
] 

| Lớp bên phải (r) | (c) |`dp[0][c]`| Đóng góp | Tổng cho điều này (r) | 
| --- | --- | --- | --- | --- | 
| 2 | 1 | 1 | 1 | | 
| 2 | 2 | 1 | 1 | 2 | 
| 2 | 3 | 2 | 0 | | 
| 3 | 1 | 1 | 2 | | 
| 3 | 2 | 2 | 1 | | 
| 3 | 3 | 3 | 0 | 3 | 
| 4 | 1 | 1 | 3 | | 
| 4 | 2 | 2 | 2 | | 
| 4 | 3 | 4 | 0 | 5 | 
| 5 | 1 | 1 | 4 | | 
| 5 | 2 | 2 | 3 | | 
| 5 | 3 | 5 | 0 | 7 | 

Tổng số là (2+3+5+7=17). Ví dụ này chứng minh tại sao việc xây dựng ngưỡng lại hữu ích: luồng cho điểm cuối bên phải cố định có thể giảm từ hai đường xuống một khi điểm cuối bên trái di chuyển trước đó và DP nắm bắt đồng thời tất cả các ranh giới đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(nk^2 2^k)) | Có (2^k) trạng thái tập hợp con, mỗi lần chuyển đổi xem xét tối đa (k) đỉnh và (k) giá trị ngưỡng. | 
| Không gian | (O(k2^k)) | Chỉ có lớp trạng thái tập hợp con hiện tại và trước đó là bắt buộc. | 

Với (k\leq9), (2^k\leq512), do đó phần mũ chỉ phụ thuộc vào độ rộng nhỏ của lớp chứ không phụ thuộc vào (n). Số lượng lớp có thể đạt tới (40000), nhưng mỗi lớp thực hiện cùng một quá trình chuyển đổi có chiều rộng giới hạn. Đây chính xác là lý do tại sao tập con DP lại khả thi đối với các ràng buộc ban đầu. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây giả định`main`hàm từ giải pháp nằm trong cùng một mô-đun Python. Trình trợ giúp thay thế đầu vào tiêu chuẩn và ghi lại đầu ra tiêu chuẩn.```python
import sys
import io
from contextlib import redirect_stdout

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    try:
        with redirect_stdout(out):
            main()
    finally:
        sys.stdin = old_stdin
    return out.getvalue().strip()

# Provided sample 1
assert run("""\
4 4
1000
1100
0110
0011

0100
1100
0010
0001

1000
1100
0000
0011
""") == "21", "sample 1"

# Provided sample 2
assert run("""\
5 3
000
100
010

000
100
010

010
101
010

010
101
010
""") == "17", "sample 2"

# Minimum-size graph, no tunnel.
assert run("""\
2 1
0
""") == "0", "minimum size with no path"

# Minimum-size graph, one tunnel.
assert run("""\
2 1
1
""") == "1", "minimum size with one path"

# Two vertices and a complete matching between the layers.
assert run("""\
2 2
10
01
""") == "2", "two disjoint paths"

# A path exists only across the first boundary.
assert run("""\
3 1
1
0
""") == "1", "boundary between usable and unusable layers"

# All possible paths exist for k=1.
assert run("""\
3 1
1
1
""") == "3", "all-equal connected layers"

# Maximum-size structural test for k=1.
n = 40000
maximum_case = str(n) + " 1\n" + "1\n" * (n - 1)
assert run(maximum_case) == str(n * (n - 1) // 2), "maximum n, k=1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1`với đường hầm`0`|`0`| Kích thước tối thiểu và ranh giới bị ngắt kết nối | 
|`2 1`với đường hầm`1`|`1`| Kích thước tối thiểu và câu trả lời tích cực nhỏ nhất | 
|`2 2`với ma trận nhận dạng |`2`| Nhiều đường dẫn rời nhau và (k>1) | 
|`3 1`với ma trận`1`,`0`|`1`| Hành vi khác nhau vượt qua ranh giới bị phá vỡ | 
|`3 1`với ma trận`1`,`1`|`3`| Mỗi khoảng thời gian đều đóng góp | 
|`n=40000, k=1`, mọi ma trận`1`|`799980000`| Câu trả lời tối đa (n) và tích lũy tối đa | 

## Vỏ cạnh 

Đối với trường hợp bị ngắt kết nối nhỏ nhất,```
2 1
0
```không có đường hầm từ lớp 1 đến lớp 2. DP bắt đầu với một tập hợp con bit và xử lý một ma trận có vùng lân cận trống. Trạng thái có thể truy cập trống đạt đến ngưỡng cho một đường dẫn ngay lập tức, do đó`layer - dp[0][1]`là số không. Câu trả lời cuối cùng là`0`. 

Đối với trường hợp được kết nối nhỏ nhất,```
2 1
1
```đỉnh duy nhất trong lớp 1 chạm đến đỉnh duy nhất trong lớp 2. Ngưỡng tập hợp trống vẫn ở ranh giới trước đó, tạo ra một điểm cuối bên trái với một đường dẫn khả dụng. Câu trả lời là`1`. 

Đối với hai đường dẫn đồng thời,```
2 2
10
01
```đỉnh đầu tiên kết nối với đỉnh thứ nhất và đỉnh thứ hai kết nối với đỉnh thứ hai. Trạng thái có thể truy cập hai bit đầy đủ vẫn tồn tại trong quá trình chuyển đổi, do đó ngưỡng cho (c=1) và (c=2) đều tính khoảng duy nhất. Đóng góp của họ là (1+1=2). 

Đối với trường hợp ranh giới```
3 1
1
0
```khoảng đầu tiên có một đường đi, nhưng ranh giới thứ hai bị ngắt kết nối. Ở lớp 2, ngưỡng cho một đường dẫn sẽ tính điểm bắt đầu ở lớp 1, trong khi ở lớp 3, nó không tính khoảng thời gian hợp lệ. Tổng số chính xác là`1`, cho thấy lý do tại sao DP phải xử lý mọi ranh giới một cách độc lập thay vì cho rằng kết nối vẫn tồn tại. 

Đối với trường hợp một đỉnh được kết nối đầy đủ,```
3 1
1
1
```mỗi trong ba khoảng có một đường đi. Tại mỗi điểm cuối bên phải, ngưỡng cho một đường dẫn bao gồm mọi lớp trước đó, do đó các đóng góp là (1) cho lớp 2 và (2) cho lớp 3. Câu trả lời cuối cùng là`3`. 

Đối với trường hợp kích thước tối đa với (n=40000) và (k=1), mỗi khoảng có một luồng, vì vậy câu trả lời là 

[ 
\binom{40000}{2}=799980000. 
] 

DP không bao giờ lưu trữ tất cả các khoảng thời gian. Nó chỉ giữ lại ranh giới ngưỡng hiện tại, đó là lý do tại sao câu trả lời có thể được tích lũy mà không cần bộ nhớ bậc hai hoặc thời gian.
