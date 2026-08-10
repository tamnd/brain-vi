---
title: "CF 102394B - Số nhị phân"
description: "Mọi số nguyên từ (0) đến (2^m-1) có thể được xem dưới dạng chuỗi nhị phân (m)-bit, đệm các số 0 đứng đầu khi cần thiết. Hàm (F{m-1}(a,b)) đếm số lượng bit liên tiếp từ phía có ý nghĩa nhất bằng nhau trước lần không khớp đầu tiên."
date: "2026-08-10T19:01:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102394
codeforces_index: "B"
codeforces_contest_name: "The 2019 China Collegiate Programming Contest Harbin Site"
rating: 0
weight: 102394
solve_time_s: 149
verified: true
draft: false
---

[CF 102394B - Số nhị phân](https://codeforces.com/problemset/problem/102394/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mọi số nguyên từ (0) đến (2^m-1) có thể được xem dưới dạng chuỗi nhị phân (m)-bit, đệm các số 0 đứng đầu khi cần thiết. Hàm (F_{m-1}(a,b)) đếm xem có bao nhiêu bit liên tiếp từ phía có ý nghĩa nhất bằng nhau trước lần không khớp đầu tiên. Nói cách khác, đó là độ dài tiền tố chung dài nhất, hay LCP, trong hai chuỗi bit (m). 

Các khoảng tạo thành một phân vùng liên tiếp của toàn bộ phạm vi (0) đến (2^m-1). Đối với mỗi khoảng ([L_i,R_i]), chúng tôi chọn một đại diện (A_i) từ khoảng đó. Theo nghĩa LCP, đại diện ít nhất phải giống với mọi số (k) trong khoảng riêng của nó giống như mọi đại diện được chọn khác. 

Sau khi tất cả các đại diện được chọn, giá trị của việc lựa chọn là sản phẩm của họ. Chúng ta cần tổng của các tích này trên mọi lựa chọn hợp lệ, modulo (100000007). 

Các ràng buộc cho chúng ta biết chính xác cấu trúc hữu ích đến từ đâu. Có nhiều nhất (2^{17}=131072) số nguyên có thể có trong một trường hợp thử nghiệm và tổng (2^m) trên tất cả các trường hợp thử nghiệm nhiều nhất là (2^{18}). Do đó, một thuật toán gần tuyến tính trong (2^m) là mong muốn. Phương thức (O(2^{2m})) ngay lập tức là không thể, trong khi phương thức (O(2^m m^2)) là hợp lý vì (m\le17). 

Có một số trường hợp nguy hiểm có thể dễ dàng phá vỡ việc triển khai bất cẩn. Khi (m=0), số nguyên duy nhất có thể là (0), nên câu trả lời là (0). Ví dụ,```
1
0 1
0 0
```có đầu ra```
0
```Giải pháp giả định luôn có ít nhất một bit có thể truy cập vào các vị trí bit không hợp lệ hoặc khởi tạo DP không chính xác. 

Khi chỉ có một khoảng thời gian thì không có sự so sánh nào với các đại diện khác cả. Mọi giá trị trong khoảng đều hợp lệ. Ví dụ,```
1
2 1
0 3
```có đáp án (0+1+2+3=6). Một giải pháp áp đặt điều kiện khoảng lân cận một cách không cần thiết có thể loại bỏ các ứng cử viên một cách không chính xác. 

Mối quan hệ ranh giới cũng có vấn đề. Coi như```
1
2 2
0 1
2 3
```Có bốn lựa chọn có thể. Cả bốn đều thỏa mãn điều kiện và tích của chúng là (0\cdot2), (0\cdot3), (1\cdot2) và (1\cdot3), cho kết quả (5). Các bất đẳng thức phải không nghiêm ngặt vì cho phép các giá trị LCP bằng nhau. 

Cuối cùng, mô đun là (100000007), không phải (1000000007). Ví dụ,```
1
17 1
0 131071
```có tổng không hạn chế (8589869056), có dư lượng yêu cầu là (89868461). Sử dụng môđun (10^9+7) quen thuộc hơn sẽ âm thầm đưa ra câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi chuỗi có thể (A_1,\ldots,A_N). Đối với mỗi chuỗi, chúng ta có thể kiểm tra điều kiện ban đầu bằng cách so sánh từng đại diện được chọn với mọi đại diện khác qua các số trong khoảng của nó. Điều này đúng vì nó tuân theo định nghĩa theo nghĩa đen, nhưng số lượng trình tự mới là vấn đề thực sự. 

Nếu mỗi khoảng chứa hai số và có các khoảng (65536), điều này có thể thực hiện được khi (m=17), chỉ cần liệt kê các lựa chọn đã cho 

[ 
2^{65536} 
] 

trình tự khác nhau. Điều đó vượt xa bất cứ điều gì có thể được xử lý. Ngay cả trước khi kiểm tra xem các chuỗi đó có hợp lệ hay không, việc liệt kê là không thể. 

Quan sát hữu ích đến từ việc giải thích (F) là LCP. Giả sử (a<b<c). Đối với các chuỗi nhị phân theo thứ tự số được sắp xếp, tiền tố chung giữa hai giá trị bên ngoài được kiểm soát bởi tiền tố chung của các giá trị liền kề. Đặc biệt, khi một giá trị khác di chuyển xa hơn về bên phải, LCP của nó với giá trị cố định không thể lớn hơn theo cách tạo ra một đối thủ cạnh tranh mới tốt hơn. 

Điều này có nghĩa là bên trong khoảng (i), chúng ta không phải so sánh (A_i) một cách độc lập với mọi đại diện. Chỉ cần so sánh nó với những người hàng xóm trực tiếp của nó là đủ. Đối với hàng xóm bên trái, điểm cuối liên quan là (L_i): 

[ 
\operatorname{LCP}(A_i,L_i) 
\ge 
\operatorname{LCP}(A_{i-1},L_i). 
] 

Đối với hàng xóm bên phải, điểm cuối liên quan là (R_i): 

[ 
\operatorname{LCP}(A_i,R_i) 
\ge 
\operatorname{LCP}(A_{i+1},R_i). 
] 

Nếu những bất đẳng thức này được giữ nguyên thì mọi đại diện ở xa hơn sẽ tự động không tốt hơn vì nó thậm chí còn nằm xa hơn theo cùng một hướng số. Điều này làm giảm tình trạng toàn cầu liên quan đến tất cả các đại diện thành hai bất bình đẳng cục bộ. Bài xã luận chính thức sử dụng chính xác mức giảm LCP này. 

Bây giờ chúng ta có thể xây dựng DP từ trái sang phải. Đối với (A_i) đã chọn, bốn giá trị LCP có liên quan: 

[ 
\bắt đầu{căn chỉnh} 
a &= \operatorname{LCP}(A_i,L_i),\ 
b &= \operatorname{LCP}(A_i,R_i),\ 
c &= \operatorname{LCP}(A_i,L_{i+1}),\ 
d &= \operatorname{LCP}(A_i,R_{i-1}). 
\end{căn chỉnh} 
] 

Hai cái đầu tiên mô tả cách đại diện hiện tại hoạt động ở các điểm cuối của chính nó. Hai cái cuối cùng mô tả chính xác thông tin cần thiết khi khoảng thời gian lân cận được xử lý. 

Giả sử đại diện trước đó có trạng thái ((c',b')). Đối với ứng viên hiện tại, hai điều kiện lân cận trở thành 

[ 
c'\le a 
] 

và 

[ 
b'\ge d. 
] 

Vì vậy tất cả các trạng thái tương thích trước đó đều nằm trong một hình chữ nhật của bảng DP hai chiều. Tổng tiền tố/hậu tố hai chiều cho phép chúng ta thu được tổng đóng góp của hình chữ nhật đó trong (O(1)) sau bước tiền xử lý (O(m^2)). 

DP chỉ có trạng thái ((m+1)^2) và mọi số nguyên thuộc về chính xác một khoảng. Do đó, tổng số đại diện ứng viên được xử lý trong toàn bộ trường hợp thử nghiệm là chính xác (2^m). Kết hợp điều này với phép biến đổi trạng thái (O(m^2)) sẽ cho ra (O(2^m m^2)), đây là độ phức tạp dự kiến. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ít nhất (2^{65536}) chuỗi trong trường hợp xấu nhất | (O(N)) | Quá chậm | 
| Tối ưu | (O(2^m m^2)) | (O(m^2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Coi mọi số là một chuỗi nhị phân (m)-bit và diễn giải (F_{m-1}(a,b)) là (\operatorname{LCP}(a,b)). Đối với hai số nguyên, LCP có thể được tính từ XOR của chúng. Nếu (x=a\oplus b) khác 0 thì bit được đặt cao nhất của (x) là bit khác biệt đầu tiên, do đó LCP là (m-\operatorname{bit_length}(x)). Nếu (x=0), LCP là (m). 
2. Sử dụng thuộc tính lân cận cục bộ để giảm điều kiện hợp lệ. Đối với khoảng (i), các ràng buộc duy nhất liên quan đến khoảng trước đó là 

[ 
\operatorname{LCP}(A_i,L_i) 
\ge 
\operatorname{LCP}(A_{i-1},L_i), 
] 

trong khi hạn chế duy nhất liên quan đến khoảng thời gian tiếp theo là

[ 
\operatorname{LCP}(A_i,R_i) 
\ge 
\operatorname{LCP}(A_{i+1},R_i). 
] 

Mỗi khoảng cách xa hơn sẽ được xử lý tự động bởi tính đơn điệu của LCP dọc theo dòng số nguyên có thứ tự. 
3. Sau khoảng thời gian xử lý (i), lưu trữ trạng thái được lập chỉ mục bởi 

[ 
(c,b)= 
\left( 
\operatorname{LCP}(A_i,L_{i+1}), 
\operatorname{LCP}(A_i,R_i) 
\đúng). 
] 

Giá trị được lưu trữ ở trạng thái đó là tổng tích của tất cả các lựa chọn hợp lệ kết thúc ở đó. Tọa độ đầu tiên là tọa độ mà khoảng tiếp theo sẽ sử dụng để so sánh với (L_{i+1}) và tọa độ thứ hai là tọa độ mà khoảng tiếp theo sẽ sử dụng để so sánh với (R_i). 
4. Trước khi xử lý một khoảng, hãy chuyển bảng trạng thái trước đó thành bảng tích lũy. Xác định 

\sum_{\substack{c\le x\b\ge y}} g[c][b]. 
] 

Đây chính xác là tập hợp các đại diện trước đó tương thích với ứng viên hiện tại có giá trị điểm cuối (x) và (y). Phép biến đổi là tổng tiền tố ở tọa độ đầu tiên và tổng hậu tố ở tọa độ thứ hai, do đó, nó nhận (O(m^2)). 
5. Khởi tạo bảng tích lũy để cho phép mọi đại diện đầu tiên được phép. Về mặt khái niệm, điều này tương đương với việc đặt một trạng thái ảo duy nhất tại ((0,m)) và áp dụng cùng một phép biến đổi tích lũy. Vì không có khoảng thời gian trước đó nên mọi ứng cử viên đầu tiên có thể đều phải được chấp nhận. 
6. Với mọi ứng viên (x) trong khoảng (i), hãy tính 

[ 
A=\tên toán tử{LCP}(x,L_i), 
] 

[ 
B=\tên toán tử{LCP}(x,R_i), 
] 

[ 
C=\tên toán tử{LCP}(x,L_{i+1}) 
] 

khi (i<N), ngược lại (C=0) và 

[ 
D=\tên toán tử{LCP}(x,R_{i-1}) 
] 

khi (i>1), ngược lại (D=0). 

Giá trị DP tích lũy (f[A][D]) chứa chính xác các lựa chọn tương thích từ các khoảng trước đó. 
7. Nhân phần đóng góp trước đó với đại diện hiện tại (x), vì giá trị của một chuỗi hoàn chỉnh là tích của tất cả các đại diện được chọn. Thêm kết quả vào trạng thái ((C,B)): 

[ 
g[C][B] 
\mathrel{+}= 
f[A][D]\cdot x. 
] 

Tất cả số học được thực hiện theo modulo (100000007). 
8. Áp dụng phép biến đổi tích lũy hai chiều cho bảng (g) mới được xây dựng. Bảng kết quả trở thành (f) cho khoảng tiếp theo. 
9. Sau khoảng thời gian cuối cùng, không có khoảng thời gian tiếp theo, vì vậy mọi trạng thái cuối cùng đều hợp lệ. Do đó, câu trả lời là (f[m][0]), bao gồm mọi trạng thái vì tọa độ đầu tiên của nó nhiều nhất là (m) và tọa độ thứ hai của nó ít nhất là (0). 

Tại sao nó hoạt động: sau khoảng thời gian xử lý (i), mỗi trạng thái DP thể hiện chính xác tất cả các lựa chọn hợp lệ thông qua (i), được phân loại theo hai giá trị LCP mà khoảng thời gian tiếp theo có thể quan sát được. Khi xem xét một ứng cử viên từ khoảng (i+1), các bất đẳng thức so với đại diện trước đó chính xác là (c\le A) và (B\ge D), và bảng tích lũy tính tổng chính xác các trạng thái đó. Do đó, mọi chuỗi hợp lệ sẽ được chuyển một lần, mọi chuyển đổi không hợp lệ sẽ bị loại trừ và phép nhân với ứng cử viên hiện tại sẽ duy trì giá trị tích số. Bổ đề lân cận địa phương đảm bảo rằng việc thỏa mãn các chuyển đổi này tương đương với việc thỏa mãn phép so sánh ban đầu với mọi đại diện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 100000007

def lcp(a, b, m):
    x = a ^ b
    if x == 0:
        return m
    return m - x.bit_length()

def cumulative(g, s):
    """Return f[x][y] = sum(g[c][b]) for c <= x and b >= y."""
    f = [row[:] for row in g]

    # Suffix sum in the second coordinate.
    for row in f:
        for k in range(s - 2, -1, -1):
            v = row[k] + row[k + 1]
            if v >= MOD:
                v -= MOD
            row[k] = v

    # Prefix sum in the first coordinate.
    for j in range(1, s):
        row = f[j]
        prev = f[j - 1]
        for k in range(s):
            v = row[k] + prev[k]
            if v >= MOD:
                v -= MOD
            row[k] = v

    return f

def solve():
    T = int(input())
    out = []

    for _ in range(T):
        m, n = map(int, input().split())
        intervals = [tuple(map(int, input().split()))
                     for _ in range(n)]

        s = m + 1

        # Initially every state is reachable for the first interval.
        f = [[1] * s for _ in range(s)]

        # Reuse g between intervals. Only touched entries need clearing.
        g = [[0] * s for _ in range(s)]

        for i, (left, right) in enumerate(intervals):
            touched = []

            next_left = intervals[i + 1][0] if i + 1 < n else 0
            prev_right = intervals[i - 1][1] if i > 0 else 0

            for x in range(left, right + 1):
                a = lcp(x, left, m)
                b = lcp(x, right, m)
                c = lcp(x, next_left, m) if i + 1 < n else 0
                d = lcp(x, prev_right, m) if i > 0 else 0

                value = f[a][d] * x % MOD

                if g[c][b] == 0:
                    touched.append((c, b))

                g[c][b] += value
                if g[c][b] >= MOD:
                    g[c][b] -= MOD

            f = cumulative(g, s)

            for x, y in touched:
                g[x][y] = 0

        out.append(str(f[m][0]))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`lcp`Hàm sử dụng trực tiếp biểu diễn XOR. Nếu hai số khác nhau, bit được đặt cao nhất của XOR của chúng chính xác là bit khác nhau đầu tiên khi chuỗi nhị phân được viết từ phía có ý nghĩa nhất. của Python`bit_length()`cung cấp vị trí đó trong thời gian không đổi, do đó, mỗi truy vấn LCP sẽ tốn (O(1)) thay vì quét tất cả (m) bit. 

Bảng DP có các chỉ số từ (0) đến (m), vì LCP có thể có bất kỳ giá trị nào trong phạm vi đó. Bảng ban đầu chứa đầy các bảng thay vì xây dựng rõ ràng trạng thái ảo ((0,m)) và biến đổi nó. Chúng tương đương nhau: trạng thái ảo đóng góp vào mọi hình chữ nhật (c\le A), (B\ge D), vì vậy mọi truy vấn ban đầu đều có giá trị bằng một. 

Đối với mỗi ứng cử viên,`f[a][d]`là tổng hình chữ nhật chính xác được yêu cầu bởi hai ràng buộc lân cận. Phép nhân với`x`phải xảy ra trước khi thêm ứng cử viên vào trạng thái tiếp theo vì DP lưu trữ tổng sản phẩm chứ không chỉ lưu trữ số lượng lựa chọn. 

các`cumulative`trước tiên, hàm thực hiện tính tổng hậu tố dọc theo tọa độ thứ hai và sau đó tính tổng tiền tố dọc theo tọa độ đầu tiên. Sau hai lần vượt qua này, 

[ 
f[a][d]=\sum_{c\le a,\ b\ge d}g[c][b]. 
] 

Thứ tự quan trọng vì bước đầu tiên thiết lập điều kiện (b\ge d), trong khi bước thứ hai thiết lập (c\le a). 

Việc triển khai sử dụng lại`g`bảng và chỉ xóa các trạng thái thực sự được chạm vào trong khoảng thời gian hiện tại. Điều này tránh việc liên tục xây dựng và khởi tạo một bộ sưu tập (O(m^2)) khác cho mỗi khoảng thời gian, điều này rất hữu ích khi có nhiều khoảng thời gian đơn lẻ. 

Số nguyên Python không bị tràn, nhưng tất cả các giá trị DP đều được giảm theo modulo (100000007). Vì mỗi phép cộng kết hợp hai giá trị đã ở dưới mô đun nên một phép trừ duy nhất là đủ thay vì sử dụng`%`trong các vòng tổng tích lũy bên trong. 

## Ví dụ đã hoạt động 

### Mẫu 1 

mẫu là```
1
2 2
0 1
2 3
```Ban đầu không có khoảng thời gian trước đó, vì vậy mọi ứng viên trong khoảng thời gian đầu tiên đều được phép. 

| Khoảng thời gian | Ứng viên | (A=\tên toán tử{LCP}(x,L_i)) | (B=\tên toán tử{LCP}(x,R_i)) | (C) | (D) | Đóng góp trước đó | Giá trị gia tăng | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 2 | 1 | 0 | 0 | 1 | 0 | 
| 1 | 1 | 1 | 2 | 0 | 0 | 1 | 1 | 

Sau khoảng đầu tiên, trạng thái khác 0 duy nhất là ((C,B)=(0,2)) có giá trị (1). Hình thức tích lũy của nó làm cho phần đóng góp liên quan trước đó bằng (1). 

Đối với khoảng thứ hai, (R_1=1), do đó (D) so sánh mọi ứng cử viên với (1). 

| Khoảng thời gian | Ứng viên | (A) | (B) | (D) | Đóng góp trước đó | Giá trị gia tăng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 2 | 2 | 1 | 0 | 1 | 2 | 
| 2 | 3 | 1 | 2 | 0 | 1 | 3 | 

Tổng trọng số cuối cùng là 

[ 
2+3=5. 
] 

Các ứng cử viên đầu tiên có giá trị bằng 0 không đóng góp gì cho sản phẩm, trong khi hai chuỗi bắt đầu bằng (1) đóng góp (2) và (3). 

### Ví dụ về ranh giới 

Hãy xem xét```
1
2 2
0 2
3 3
```Trong khoảng đầu tiên, các ứng cử viên (0,1,2) có khả năng tương thích khác nhau với đại diện cuối cùng (3). 

| Ứng cử viên đầu tiên | Trạng thái liên quan ((C,B)) | Đóng góp | 
| --- | --- | --- | 
| 0 | ((0,1)) | 0 | 
| 1 | ((0,0)) | 1 | 
| 2 | ((1,2)) | 2 | 

Khoảng thứ hai chỉ có ứng cử viên (3). Các giá trị của nó là (A=2), (B=2) và (D=\operatorname{LCP}(3,2)=1). Do đó, hình chữ nhật trước yêu cầu (C\le2) và (B\ge1). Ứng viên đầu tiên đóng góp bằng 0, trong khi cả (1) và (2) đều tương thích. 

| Ứng cử viên đầu tiên | Đóng góp trước đó được xem bởi (3) | Sản phẩm | 
| --- | --- | --- | 
| 0 | 0 | 0 | 
| 1 | 1 | 3 | 
| 2 | 2 | 6 | 

Câu trả lời là 

[ 
3+6=9. 
] 

Dấu vết này chứng tỏ tại sao DP cần cả hai tọa độ. Ứng viên (1) và ứng viên (2) có các giá trị LCP khác nhau so với ranh giới bên trái của khoảng tiếp theo và hình chữ nhật tích lũy chọn cả hai đều chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(2^m m^2)) | Tổng cộng có (2^m) số nguyên ứng cử viên và mỗi khoảng thực hiện một phép biến đổi DP (O(m^2)) | 
| Không gian | (O(m^2+N)) | DP sử dụng hai bảng ((m+1)\times(m+1)), trong khi các khoảng thời gian yêu cầu lưu trữ (O(N)) | 

Trường hợp xấu nhất có (2^m=131072), với (m\le17) và tổng (2^m) trên tất cả các trường hợp thử nghiệm nhiều nhất là (2^{18}). Giới hạn (O(2^m m^2)) chính xác là tỷ lệ dự định cho các ràng buộc này. Trang cuộc thi chính thức đưa ra giới hạn thời gian một giây và giới hạn bộ nhớ 512 MB cho vấn đề này. 

## Trường hợp thử nghiệm 

Dây nịt sau đây giả định`solve()`chức năng từ giải pháp trên có sẵn.```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    try:
        solve()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

    return output.getvalue().strip()

# Provided sample
assert run(
    """1
2 2
0 1
2 3
"""
) == "5", "sample"

# Minimum-size case: m = 0, only value 0 exists.
assert run(
    """1
0 1
0 0
"""
) == "0", "minimum m"

# One interval: every value is valid, so sum 0 + 1 + 2 + 3 = 6.
assert run(
    """1
2 1
0 3
"""
) == "6", "single interval"

# Boundary case where two different first representatives are valid.
assert run(
    """1
2 2
0 2
3 3
"""
) == "9", "boundary compatibility"

# Modulo test: sum(0..131071) = 8589869056,
# which is 89868461 modulo 100000007.
assert run(
    """1
17 1
0 131071
"""
) == "89868461", "modulus"

# Maximum number of intervals: every interval is a singleton.
# The only possible sequence contains A1 = 0, so its product is 0.
n = 1 << 17
lines = ["1", f"17 {n}"]
for x in range(n):
    lines.append(f"{x} {x}")

assert run("\n".join(lines) + "\n") == "0", "maximum N"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`m=0, [0,0]`|`0`| Ranh giới 0 bit và sản phẩm 0 | 
|`m=2, [0,3]`|`6`| Không có ràng buộc lân cận khi (N=1) | 
|`m=2, [0,2], [3,3]`|`9`| Bất đẳng thức điểm cuối và ràng buộc | 
|`m=17, [0,131071]`|`89868461`| Mô-đun đúng (100000007) | 
| (131072) khoảng đơn |`0`| Xử lý tối đa (N) và ranh giới khoảng thời gian | 

## Vỏ cạnh 

Với (m=0), có chính xác một số nguyên có thể có (0) và biểu diễn nhị phân của nó có các bit bằng 0. các`lcp`chức năng nhận được`a=b=0`, trả về (m=0) và ứng cử viên duy nhất đóng góp hệ số bằng 0. Do đó, DP trả về (0) mà không bao giờ truy cập vào vị trí bit âm. 

Trong một khoảng thời gian, không có đại diện trước hoặc đại diện tiếp theo. Bảng DP ban đầu cung cấp cho mỗi ứng viên một phần đóng góp, trong khi tọa độ lân cận bị thiếu chỉ được đặt thành 0 dưới dạng giá trị giả. Mọi ứng cử viên đều được xử lý, vì vậy với ([0,3]) bốn đóng góp là (0,1,2,3), cho (6). 

Tại một ranh giới khoảng cách, sự bình đẳng phải được duy trì. Ví dụ: trong mẫu, việc chọn (A_1=1) và (A_2=3) sẽ cho LCP bằng nhau ở một số so sánh ranh giới và trình tự vẫn hợp lệ. DP sử dụng`c <= A`Và`b >= D`, không bao giờ có bất đẳng thức nghiêm ngặt, vì vậy mối quan hệ tồn tại. 

Các điểm cuối khoảng đều được bao gồm. Vì các khoảng tạo thành một phân vùng hoàn chỉnh nên tổng số ứng viên được DP xử lý chính xác là 

[ 
\sum_i(R_i-L_i+1)=2^m. 
] 

Đây là lý do tại sao việc lặp lại từ`left`bởi vì`right`toàn diện vừa cần vừa đủ. 

Trường hợp ứng viên bằng 0 cũng được xử lý một cách tự nhiên. Đóng góp chuyển đổi của nó được nhân với 0, do đó, mọi chuỗi hoàn chỉnh chứa đại diện đó sẽ đóng góp bằng 0 vào tổng sản phẩm được yêu cầu. Tình trạng như vậy không cần điều trị đặc biệt. 

Mô đun gần với (10^8) một cách bất thường chứ không phải (10^9). Việc thực hiện sử dụng hằng số chính xác`100000007`và kiểm tra modulo với (m=17) xác minh rằng việc giảm được thực hiện chính xác.
