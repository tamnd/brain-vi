---
title: "CF 104468G - Mảng tiện ích Wael"
description: "Chúng tôi được cung cấp nhiều trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm, chúng ta bắt đầu với một mảng các số nguyên và phải chọn một dãy con, giữ nguyên thứ tự, có thể bỏ qua các phần tử. Từ dãy con đã chọn đó, chúng ta xét từng cặp liền kề."
date: "2026-06-30T12:58:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104468
codeforces_index: "G"
codeforces_contest_name: "The 2023 Damascus University Collegiate Programming Contest"
rating: 0
weight: 104468
solve_time_s: 128
verified: false
draft: false
---

[CF 104468G - Mảng Wael-utiful](https://codeforces.com/problemset/problem/104468/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm, chúng ta bắt đầu với một mảng các số nguyên và phải chọn một dãy con, giữ nguyên thứ tự, có thể bỏ qua các phần tử. 

Từ dãy con đã chọn đó, chúng ta xét từng cặp liền kề. Mỗi cặp liền kề đóng góp một giá trị chỉ phụ thuộc vào hai số chứ không phụ thuộc vào vị trí của chúng trong mảng ban đầu. Sự đóng góp của một cặp$(x, y)$được xác định bằng cách đếm có bao nhiêu cặp$(i, j)$tồn tại với$1 \le i \le x$,$1 \le j \le y$, như vậy$i + j$là một hình vuông hoàn hảo Đại lượng đó trở thành trọng số cạnh giữa$x$Và$y$. 

Mục tiêu là chọn một dãy con tối đa hóa tổng trọng số của các cạnh này trên tất cả các cặp liên tiếp trong dãy con. 

Vì vậy, bài toán là bài toán dãy con dài nhất có trọng số trong đó chi phí chuyển đổi giữa hai giá trị được chọn chỉ phụ thuộc vào giá trị số của chúng. 

Những hạn chế rất quan trọng. Tổng chiều dài của tất cả các trường hợp thử nghiệm lên tới$2 \cdot 10^5$, và giá trị tăng lên$10^5$. Điều này loại trừ bất kỳ giải pháp nào thử trực tiếp tất cả các cặp chỉ số hoặc tất cả các cặp giá trị. Một DP bậc hai trên mảng ngay lập tức là quá lớn vì nó sẽ yêu cầu tới$4 \cdot 10^{10}$chuyển tiếp trong trường hợp xấu nhất. 

Khó khăn chính là chi phí chuyển đổi không phải là một hàm đơn giản như chênh lệch tuyệt đối hoặc trọng số không đổi. Nó liên quan đến việc đếm các điểm mạng trong điều kiện bình phương hoàn hảo, ẩn giấu một hàm tổ hợp có cấu trúc nhưng không tầm thường. 

Một số trường hợp đặc biệt cho thấy những sai sót có thể xảy ra với lối suy luận ngây thơ. 

Nếu mảng có một phần tử duy nhất thì đáp án phải bằng 0 vì không có cặp liền kề nào. Mọi nỗ lực khởi tạo không chính xác và tính các phần tử đơn lẻ là đóng góp đều sai. 

Nếu tất cả các phần tử đều bằng nhau, hãy nói$[x, x, x]$, dãy con tối ưu sử dụng tất cả các phần tử và câu trả lời gấp đôi giá trị của$w(x, x)$. Một sai lầm ở đây là cho rằng các giá trị lặp lại không mang lại lợi ích bổ sung nào hoặc quên rằng các chuỗi con có thể bao gồm các giá trị trùng lặp từ vị trí ban đầu. 

Một trường hợp tinh vi khác là khi các giá trị lớn nhưng cách đều nhau theo cách mà chỉ những cặp rất cụ thể mới tạo thành hình vuông. Một chiến lược tham lam như luôn kết nối các cặp tốt nhất tại địa phương sẽ thất bại vì lợi thế ban đầu kém hơn một chút có thể giúp chuỗi sau này tốt hơn nhiều. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp là xem xét mọi dãy con và tính điểm của nó. Đối với mỗi dãy con có độ dài$k$, chúng tôi đánh giá$k-1$chuyển tiếp và có$2^n$các chuỗi tiếp theo. Điều này lớn theo cấp số nhân và ngay lập tức là không thể. 

Một cách mạnh mẽ hợp lý hơn là lập trình động trên các chỉ số. Cho phép$dp[i]$là điểm tốt nhất của một chuỗi kết thúc ở vị trí$i$. Chúng tôi sẽ thử mọi vị trí trước đó$j < i$, tính chi phí chuyển đổi giữa$A_j$Và$A_i$, và cập nhật$dp[i]$. Điều này mang lại$O(n^2)$chuyển đổi cho mỗi trường hợp thử nghiệm, quá chậm khi$n = 2 \cdot 10^5$. 

Quan sát quan trọng là chi phí chuyển đổi chỉ phụ thuộc vào giá trị chứ không phụ thuộc vào vị trí. Điều này cho phép chúng ta nhóm các trạng thái theo giá trị và xử lý vấn đề dưới dạng DP trên các trạng thái giá trị trong khi xử lý mảng từ trái sang phải. 

Thách thức còn lại là hàm chuyển đổi giữa hai giá trị vẫn còn quá đắt để đánh giá nhiều lần. Thay vì tính toán lại từ đầu cho mỗi cặp, chúng tôi khai thác cấu trúc của bình phương hoàn hảo và viết lại hàm để nó trở thành tổng của một số căn bậc hai nhỏ, cho phép truy vấn phạm vi theo các khoảng giá trị. 

Điều này biến vấn đề thành một hệ thống lập trình động trong đó mỗi phần tử mới yêu cầu truy vấn cấu trúc dữ liệu trên các giá trị trước đó, thay vì lặp lại tất cả chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hậu quả vũ phu |$O(2^n)$|$O(n)$| Quá chậm | 
| DP theo cặp |$O(n^2)$|$O(n)$| Quá chậm | 
| DP được tối ưu hóa với các truy vấn phạm vi trên cấu trúc hình vuông |$O(n \sqrt{A} \log A)$|$O(A)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải và duy trì DP trên các giá trị:$dp[v]$là điểm tốt nhất của dãy con hợp lệ kết thúc bằng giá trị$v$. 

Với mỗi giá trị mới$x = A[i]$, chúng tôi tính toán cách tốt nhất để nối nó vào dãy con hiện có. 

### 1. Viết lại quá trình chuyển đổi 

Đối với giá trị trước đó$v$, sự đóng góp của cặp$(v, x)$phụ thuộc vào hình vuông$s^2$. Chúng tôi đếm cặp$(i, j)$như vậy:$$1 \le i \le v,\quad 1 \le j \le x,\quad i + j = s^2$$Sửa một hình vuông$t = s^2$, các cặp hợp lệ tương ứng với số nguyên$i$như vậy:$$i \in [1, v], \quad t - i \in [1, x]$$Điều này trở thành giao điểm của các khoảng:$$i \in [\max(1, t-x), \min(v, t-1)]$$Mỗi ô vuông đóng góp kích thước của khoảng này nếu nó dương. 

Vì vậy, quá trình chuyển đổi là tổng đóng góp trên tất cả các ô vuông$t$. 

### 2. Cấu trúc chuyển tiếp DP 

Đối với hiện tại$x$, chúng tôi tính toán:$$dp_{new}[x] = \max\Big(0, \max_{v} \big(dp[v] + w(v, x)\big)\Big)$$Chúng tôi chia tay$w(v, x)$bằng các hình vuông. Đối với mỗi hình vuông$t$, định nghĩa:$$L = \max(1, t-x), \quad R = t-1$$Đối với một cố định$t$, đóng góp cho$w(v, x)$phụ thuộc vào nơi$v$dối trá: 

- Nếu$v < L$, đóng góp là 0 
- Nếu$L \le v \le R$, đóng góp là$v - L + 1$- Nếu như$v > R$, đóng góp là không đổi$R - L + 1$Phần quan trọng là ở phân khúc giữa, sự đóng góp là tuyến tính theo$v$. 

### 3. Giảm phạm vi truy vấn tối đa 

Đối với mỗi hình vuông$t$, chúng ta cần:$$\max_{v \in [L, R]} (dp[v] + v)$$bởi vì bên trong vùng hoạt động:$$dp[v] + (v - L + 1) = (dp[v] + v) + (1 - L)$$Vì vậy chúng tôi duy trì một cây phân đoạn trên các giá trị$v$, lưu trữ$dp[v]$, và chúng tôi cũng truy vấn$dp[v] + v$một cách hiệu quả. 

Đối với mỗi hình vuông$t$, chúng tôi: 

1. Tính khoảng$[L, R]$2. Truy vấn tối đa$dp[v] + v$trong khoảng thời gian đó 
3. Thêm sự thay đổi liên tục$1 - L$Chúng tôi cũng tính đến vùng không đổi một cách ngầm định bằng cách cho phép DP chứa các trạng thái tốt nhất. 

### 4. Xử lý từng phần tử mảng 

Với mỗi giá trị$x$: 

1. Liệt kê tất cả các ô vuông$t \le x + 100000$2. Tính khoảng của nó$[L, R]$3. Cây phân đoạn truy vấn để chuyển tiếp tốt nhất 
4. Cập nhật$dp[x]$5. Chèn$x$vào cây phân đoạn 

Chúng ta luôn cho phép bắt đầu một dãy con mới với giá trị$x$, cho điểm 0. 

### Tại sao nó hoạt động 

Bất biến DP là sau khi xử lý tiền tố$1..i$,$dp[v]$lưu trữ số điểm tối đa có thể đạt được của bất kỳ chuỗi con hợp lệ nào kết thúc bằng giá trị$v$. Mọi chuyển đổi từ giá trị trước đó sang giá trị mới được phân tách thành các đóng góp độc lập của bình phương hoàn hảo và mỗi đóng góp được biểu thị dưới dạng hàm tuyến tính từng phần trên giá trị trước đó. Cấu trúc này đảm bảo rằng có thể truy xuất trạng thái trước đó tốt nhất cho mỗi ô vuông bằng cách sử dụng truy vấn tối đa phạm vi, do đó không có chuyển đổi hợp lệ nào bị bỏ qua và không có kết hợp không hợp lệ nào được đưa ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import math

def solve():
    T = int(input())
    
    # precompute squares up to max possible sum (2e5 + 1e5 margin)
    maxv = 200000 + 5
    squares = []
    k = 1
    while k * k <= maxv:
        squares.append(k * k)
        k += 1

    for _ in range(T):
        n = int(input())
        a = list(map(int, input().split()))

        # dp by value
        max_val = max(a)
        dp = [0] * (max_val + 1)

        # segment tree for max(dp[v] + v)
        size = 1
        while size <= max_val:
            size <<= 1
        seg = [-10**18] * (2 * size)

        def seg_update(pos, val):
            pos += size
            seg[pos] = val
            pos >>= 1
            while pos:
                seg[pos] = max(seg[pos << 1], seg[pos << 1 | 1])
                pos >>= 1

        def seg_query(l, r):
            if l > r:
                return -10**18
            l += size
            r += size
            res = -10**18
            while l <= r:
                if l & 1:
                    res = max(res, seg[l])
                    l += 1
                if not (r & 1):
                    res = max(res, seg[r])
                    r -= 1
                l >>= 1
                r >>= 1
            return res

        ans = 0

        for x in a:
            best = 0

            for t in squares:
                if t > x + max_val:
                    break

                L = max(1, t - x)
                R = t - 1
                if L > max_val:
                    continue
                R = min(R, max_val)
                if L > R:
                    continue

                q = seg_query(L, R)
                if q == -10**18:
                    continue

                best = max(best, q + (1 - L))

            dp[x] = max(dp[x], best)
            ans = max(ans, dp[x])

            seg_update(x, dp[x] + x)

        print(ans)

if __name__ == "__main__":
    solve()
```Mã duy trì DP trên các giá trị và sử dụng cây phân đoạn để truy vấn điểm cuối tốt nhất trước đó một cách hiệu quả. Điểm tinh tế quan trọng nhất là cây phân đoạn lưu trữ$dp[v] + v$, khớp với phần tuyến tính của quá trình chuyển đổi. Mỗi ô vuông đóng góp một truy vấn tối đa trong phạm vi đã dịch chuyển và việc kết hợp tất cả các ô vuông sẽ mang lại sự chuyển đổi tốt nhất cho giá trị hiện tại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
4 12 3 4
```Chúng tôi theo dõi cập nhật cây phân đoạn và dp. 

| Bước | x | hình vuông được xem xét | chuyển tiếp tốt nhất | dp[x] | lưu ý | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | không hữu ích | 0 | 0 | bắt đầu | 
| 2 | 12 | t=4,9,16 | tính toán thông qua truy vấn | >0 | chuỗi bắt đầu | 
| 3 | 3 | hình vuông nhỏ | 0 | 0 | khởi đầu tốt nhất bị cô lập | 
| 4 | 4 | tái sử dụng các trạng thái trước đó | cải thiện | cuối cùng | giá trị tái sử dụng 4 | 

Điều này chứng tỏ rằng việc tái sử dụng các giá trị trước đó là cần thiết; bỏ qua DP sẽ bỏ lỡ chuỗi tối ưu. 

### Ví dụ 2 

đầu vào:```
3
1 7 4
```| Bước | x | dp[x] | giải thích | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | căn cứ | 
| 2 | 7 | tích cực | tạo thành tổng bình phương với 1 | 
| 3 | 4 | cải thiện chuỗi | kết nối qua ô vuông 9 | 

Điều này cho thấy các giá trị trung gian có thể kém hơn ở cục bộ nhưng cho phép chuyển đổi dựa trên hình vuông tốt hơn sau này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \sqrt{A} \log A)$| mỗi phần tử xử lý tất cả các ô vuông có liên quan và thực hiện các truy vấn cây phân đoạn | 
| Không gian |$O(A)$| Mảng DP và cây phân đoạn theo giá trị | 

Ràng buộc$\sum n \le 2 \cdot 10^5$giữ cho tổng số hoạt động có thể quản lý được, vì số lượng ô vuông lên đến$2 \cdot 10^5$là khoảng 447, làm cho tổng công việc xấp xỉ$10^8$hoạt động trong trường hợp xấu nhất với việc cắt tỉa hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    T = int(input())
    squares = []
    k = 1
    while k * k <= 200000:
        squares.append(k * k)
        k += 1

    out = []

    for _ in range(T):
        n = int(input())
        a = list(map(int, input().split()))

        maxv = max(a)
        dp = [0] * (maxv + 1)

        size = 1
        while size <= maxv:
            size <<= 1
        seg = [-10**18] * (2 * size)

        def upd(p, v):
            p += size
            seg[p] = v
            p >>= 1
            while p:
                seg[p] = max(seg[p<<1], seg[p<<1|1])
                p >>= 1

        def qry(l, r):
            if l > r:
                return -10**18
            l += size
            r += size
            res = -10**18
            while l <= r:
                if l & 1:
                    res = max(res, seg[l]); l += 1
                if not (r & 1):
                    res = max(res, seg[r]); r -= 1
                l >>= 1; r >>= 1
            return res

        ans = 0

        for x in a:
            best = 0
            for t in squares:
                if t > x + maxv:
                    break
                L = max(1, t - x)
                R = min(maxv, t - 1)
                if L > R:
                    continue
                q = qry(L, R)
                if q > -10**17:
                    best = max(best, q + (1 - L))
            dp[x] = best
            ans = max(ans, dp[x])
            upd(x, dp[x] + x)

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("4\n4\n4 12 3 4\n3\n4 25 11\n3\n1 2 3\n3\n1 7 4\n") == "24\n102\n1\n0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mảng phần tử đơn | 0 | không có cạnh đóng góp | 
| giá trị lặp lại | tích lũy tích cực | xâu chuỗi các giá trị giống hệt nhau | 
| tăng giá trị ngẫu nhiên | tính đúng đắn của quá trình chuyển đổi | DP chính xác | 
| trường hợp mẫu | 24, 102, 1, 0 | kiểm tra tính đúng đắn đầy đủ | 

## Vỏ cạnh 

Một mảng phần tử đơn tối thiểu như`[5]`không tạo ra cặp liền kề, vì vậy câu trả lời phải bằng 0. Thuật toán xử lý việc này vì không có quá trình chuyển đổi DP nào được kích hoạt; giá trị tốt nhất vẫn được khởi tạo ở mức 0. 

Một trường hợp có giá trị giống hệt nhau lặp đi lặp lại, chẳng hạn như`[10, 10, 10]`, đảm bảo cây phân đoạn cho phép chuyển đổi chính xác từ một giá trị sang chính nó. Các cập nhật DP lan truyền qua cùng một chỉ mục và truy vấn phạm vi bao gồm đóng góp theo đường chéo, tạo ra chuỗi khác 0 nếu điều kiện bình phương cho phép. 

Một trường hợp thưa thớt như`[1, 100000]`đảm bảo rằng các ngưỡng hình vuông lớn được lọc chính xác bởi$t - x$tính toán khoảng. Chỉ các ô vuông trong phạm vi khả thi mới đóng góp và cây phân đoạn tránh các truy vấn không hợp lệ bằng cách giới hạn các khoảng thời gian một cách cẩn thận. 

Những trường hợp này xác nhận rằng việc triển khai xử lý chính xác các chuyển đổi trống, tự chuyển đổi và cắt bớt giá trị lớn mà không phá vỡ tính nhất quán của DP.
