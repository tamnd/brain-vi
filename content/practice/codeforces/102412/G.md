---
title: "CF 102412G - Vấn đề về chất lượng AtCoder"
description: "Chúng ta có một tập hợp (S) với (n) phần tử và mọi tập hợp con của (S) phải được tô màu đỏ hoặc xanh. Đối với tập hợp con (T), tô màu đỏ có giá (RT), trong khi tô màu xanh lam có giá (BT). Chi phí có thể âm, vì vậy mục tiêu không chỉ đơn giản là chọn màu rẻ hơn một cách độc lập."
date: "2026-08-10T14:08:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102412
codeforces_index: "G"
codeforces_contest_name: "MEX Foundation Contest (supported by AIM Tech)"
rating: 0
weight: 102412
solve_time_s: 245
verified: true
draft: false
---

[CF 102412G - Vấn đề về chất lượng AtCoder](https://codeforces.com/problemset/problem/102412/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp (S) với (n) phần tử và mọi tập hợp con của (S) phải được tô màu đỏ hoặc xanh. Đối với một tập hợp con (T), tô màu đỏ có giá (R_T), trong khi tô màu xanh lam có giá (B_T). Chi phí có thể âm, vì vậy mục tiêu không chỉ đơn giản là chọn màu rẻ hơn một cách độc lập. 

Màu sắc có một hạn chế về cấu trúc. Bất cứ khi nào hai tập con (A) và (B) có cùng màu thì liên kết của chúng (A\cup B) cũng phải có cùng màu đó. Nói cách khác, các tập con của mỗi màu phải đóng lại khi lấy hợp. 

Đầu vào đại diện cho các tập hợp con bằng mặt nạ bit. Mặt nạ (i) đại diện cho tập hợp con chứa chính xác các phần tử có bit tương ứng được đặt. Do đó, hai mảng chứa (2^n) chi phí mỗi mảng. Chúng ta cần tổng chi phí sơn đã chọn tối thiểu trên tất cả (2^n) tập hợp con. 

Giới hạn (n\le20) là tín hiệu chính. Có nhiều nhất (2^{20}=1.048.576) tập hợp con, vì vậy giải pháp (O(2^n)) hoặc (O(n2^n)) là thực tế. Với giới hạn hai giây, mọi thứ như (O(3^n)) đã quá lớn ở mức (n=20), vì (3^{20}=3,486,784,401). Giới hạn bộ nhớ của (256) MiB cũng có nghĩa là chúng ta chỉ nên giữ một vài mảng có kích thước (2^n), thay vì lưu trữ một bảng DP lớn được lập chỉ mục bởi một số tập hợp con. Các ràng buộc và giới hạn chính thức là (n\le20), chi phí từ (-10^9) đến (10^9), hai giây và (256) MiB. 

Có một số trường hợp khó xử lý. Đầu tiên, (n=0) chỉ để lại tập con trống. Ví dụ,```
0
-129363358
227605714
```có câu trả lời`-129363358`, vì có chính xác một tập hợp con và chúng ta chỉ cần chọn màu rẻ hơn của nó. Mẫu này được bao gồm trong tuyên bố ban đầu. 

Chi phí âm là một nguồn sai lầm khác. Một giải pháp giả định chi phí không âm có thể cố gắng diễn giải vấn đề như một cách tô màu chi phí tối thiểu thông thường, nhưng chi phí âm có thể làm cho một màu có vẻ đắt tiền được ưa chuộng hơn trên toàn cầu vì hạn chế kết hợp nhiều tập hợp con. 

Cuối cùng, tập hợp con trống phải được coi là tập hợp con thông thường. Với (n=1), có chính xác hai tập con, tập rỗng và tập đơn. Tập trống không có ngoại lệ đặc biệt nào đối với quy tắc tô màu. Việc quên đưa chi phí của nó vào trường hợp cơ sở sẽ tạo ra kết quả sai lệch chính xác bằng một chi phí tập hợp con. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về vấn đề này là xác định cách tô màu hợp lệ cho mọi tập hợp con. Điều đó có nghĩa là xem xét (2^{2^n}) các cách tô màu có thể có và kiểm tra điều kiện hợp. Điều này đã là vô vọng đối với (n) rất nhỏ nên chúng ta cần khai thác cấu trúc đặc biệt của việc đóng hợp. 

Một cách tiếp cận lập trình động lực lượng vũ phu hữu ích hơn xem xét một tập hợp cố định (S). Giả sử (S) có màu đỏ. Trong số tất cả các tập con màu xanh của (S), lấy hợp của chúng và gọi nó là (V). Bởi vì các tập con màu xanh là tập đóng dưới phép hợp nên (V) bản thân nó có màu xanh và là tập con màu xanh lớn nhất duy nhất. Mọi tập con không chứa trong (V) khi đó phải có màu đỏ, vì nếu không nó sẽ là một tập con màu xanh khác có hợp với (V) sẽ lớn hơn (V). 

Điều này mang lại một DP liệt kê (V\subseteq S). Đối với mỗi (S), chúng ta có thể thử mọi (V) có thể, giải bài toán tô màu bên trong (V) và trả tiền cho tất cả các tập con bên ngoài (V) bằng màu đỏ. Thực hiện việc này trên mỗi cặp (V\subseteq S) mất (O(3^n)) thời gian. Tại (n=20), đó là khoảng (3,49) tỷ mối quan hệ tập hợp con, vượt xa giới hạn. Lý do tương tự cũng được áp dụng nếu (S) có màu xanh lam. Cấu trúc DP này đúng nhưng nó xem xét quá nhiều khả năng. Quan điểm (O(3^n)) cũng là một cách hữu ích để khám phá sự tái diễn nhanh hơn. 

Quan sát quan trọng là chúng ta thực sự không cần xác định tập hợp con có màu đối lập lớn nhất. Giả sử (S) có màu đỏ. Phải tồn tại một số phần tử (e\in S) sao cho mọi tập con (T) thỏa mãn (e\in T\subseteq S) đều có màu đỏ. Để biết lý do tại sao, hãy giả sử điều ngược lại. Khi đó với mỗi phần tử (e), sẽ có một số tập con màu xanh chứa (e). Lấy hợp tất cả các tập con màu xanh đó sẽ được (S). Vì các tập con màu xanh là tập đóng liên hợp nên bản thân (S) sẽ phải có màu xanh, mâu thuẫn với việc (S) có màu đỏ. 

Khi phần tử (e) như vậy được chọn, tất cả các tập hợp con chứa (e) sẽ có màu đỏ. Các tập con còn lại chính xác là tập con của (S\setminus{e}) và chúng có thể được tô màu tối ưu như một thể hiện nhỏ hơn độc lập. Điều này biến việc quan sát cấu trúc thành một sự lặp lại trên mặt nạ. 

Đặt (R(S)) biểu thị tổng chi phí đỏ của mọi tập hợp con của (S) và xác định (B(S)) tương tự. Nếu chúng ta loại bỏ (e) khỏi (S), thì (R(S)-R(S\setminus{e})) chính xác là tổng chi phí màu đỏ của tất cả các tập con của (S) chứa (e). Điều tương tự cũng xảy ra với màu xanh lam. 

Nếu (dp[S]) biểu thị chi phí tối thiểu để tô màu tất cả các tập con của (S), mà không cố định màu của (S), thì 

\min_{e\in S} 
\left( 
dp[S\setminus{e}] 
+ 
\min\left( 
R(S)-R(S\setminus{e}), 
B(S)-B(S\setminus{e}) 
\phải) 
\đúng). 
] 

Thuật ngữ đầu tiên tô màu đệ quy tất cả các tập hợp con không chứa (e). Số hạng thứ hai chọn xem tất cả các tập con chứa (e) có màu đỏ hay tất cả đều có màu xanh. 

Chúng ta chỉ cần tính (R(S)) và (B(S)), là tổng tập con tiêu chuẩn hoặc SOS DP, trong (O(n2^n)). Phép truy toán cuối cùng cũng xem xét từng phần tử được đặt một lần, do đó tổng số vẫn còn (O(n2^n)). Đây là cách tiếp cận tối ưu được sử dụng bởi các giải pháp được chấp nhận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đối với các lựa chọn cấu trúc hợp lệ | (O(3^n)) | (O(2^n)) | Quá chậm | 
| SOS DP + mặt nạ tối ưu DP | (O(n2^n)) | (O(2^n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc hai mảng chi phí. (Các) mặt nạ đại diện cho một tập hợp con của tập hợp ban đầu, do đó có (2^n) mặt nạ. 
2. Chuyển mảng màu đỏ thành (R[s]), tổng chi phí màu đỏ trên mỗi mặt nạ con của (s). Thực hiện phép biến đổi tương tự trên mảng màu xanh để thu được (B[s]). Đây là SOS DP tiêu chuẩn và sau khi nó kết thúc, chênh lệch (R[s]-R[t]) của (t=s\setminus{e}) chính xác là tổng chi phí màu đỏ của các tập hợp con của (s) chứa (e). 
3. Đặt (dp[0]=\min(R[0],B[0])). Tập rỗng không có phần tử nào cần loại bỏ nên đây là trường hợp cơ bản. Vì nó là tập hợp con duy nhất khi (n=0), nên điều này cũng ngay lập tức xử lý trường hợp nhỏ nhất có thể. 
4. Xử lý mọi mặt nạ không trống theo thứ tự số tăng dần. Với mỗi tập bit (e) của (s), hãy đặt (t=s\setminus{e}). Bởi vì (t<s), giá trị DP của nó đã được tính toán. 
5. Giả sử phần tử được chọn là (e). Mọi tập con của (các) chứa (e) phải nhận một màu chung. Nếu màu đó là màu đỏ thì tổng đóng góp của chúng là (R[s]-R[t]). Nếu nó có màu xanh lam thì đóng góp của họ là (B[s]-B[t]). Giá trị nhỏ hơn trong hai giá trị này là lựa chọn tốt nhất cho (e) cụ thể này. 
6. Cập nhật 

\min\left( 
dp[s], 
dp[t]+\min(R[s]-R[t],B[s]-B[t]) 
\đúng). 
] 

1. Sau khi tất cả các mặt nạ đã được xử lý, hãy xuất (dp[2^n-1]), vì mặt nạ này đại diện cho tập hợp hoàn chỉnh (S), nên tất cả các tập hợp con của tập hợp ban đầu đều đã được xem xét. 

### Tại sao nó hoạt động 

Bất biến là (dp[S]) là chi phí tối thiểu của việc tô màu hợp lệ cho mọi tập hợp con của (S). Hãy xem xét bất kỳ màu tối ưu nào của một (S) khác trống và giả sử (S) là màu đỏ. Điều kiện đóng hợp đảm bảo rằng một phần tử nào đó (e\in S) có đặc tính là mọi tập con chứa (e) đều có màu đỏ. Việc loại bỏ (e) để lại chính xác các tập con của (S\setminus{e}), có chi phí tối ưu là (dp[S\setminus{e}]). Tất cả các tập con còn lại chứa (e) buộc phải chuyển sang màu đỏ và đóng góp (R[S]-R[S\setminus{e}]). Lập luận tương tự được áp dụng khi (S) có màu xanh lam. Do đó, mọi màu sắc tối ưu được thể hiện bằng một trong các chuyển đổi của chúng tôi, trong khi mọi chuyển đổi đều tạo ra màu hợp lệ. Lấy mức tối thiểu sẽ cho kết quả chính xác (dp[S]). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve(data=None):
    if data is None:
        n = int(input())
        m = 1 << n
        red = list(map(int, input().split()))
        blue = list(map(int, input().split()))
    else:
        it = iter(map(int, data.split()))
        n = next(it)
        m = 1 << n
        red = [next(it) for _ in range(m)]
        blue = [next(it) for _ in range(m)]

    # SOS DP:
    # red[s] = sum(red[t]) for all t subset of s.
    # blue[s] = sum(blue[t]) for all t subset of s.
    step = 1
    while step < m:
        block = step << 1
        for base in range(0, m, block):
            end = base + step
            for s in range(base, end):
                red[s + step] += red[s]
                blue[s + step] += blue[s]
        step <<= 1

    INF = 10**30
    dp = [INF] * m
    dp[0] = min(red[0], blue[0])

    for s in range(1, m):
        best = INF
        x = s
        while x:
            bit = x & -x
            t = s ^ bit

            add = red[s] - red[t]
            other = blue[s] - blue[t]
            if other < add:
                add = other

            cand = dp[t] + add
            if cand < best:
                best = cand

            x ^= bit

        dp[s] = best

    return str(dp[m - 1])

if __name__ == "__main__":
    print(solve())
```Phần đầu tiên lưu trữ hai mảng chi phí ban đầu. Sau đó, phép biến đổi SOS thay đổi ý nghĩa của chúng từ “chi phí của chính xác tập hợp con này” thành “tổng chi phí của tất cả các mặt nạ con”. Biến đổi tại chỗ an toàn vì mỗi bit được xử lý một lần và khi một bit được thêm vào mặt nạ, giá trị không có bit đó đã được hoàn thiện cho giai đoạn hiện tại. 

Trường hợp cơ bản sử dụng`min(red[0], blue[0])`, bởi vì sau khi SOS biến đổi cả hai`red[0]`Và`blue[0]`vẫn đại diện cho chi phí của riêng tập hợp con trống. 

Đối với mỗi mặt nạ,`x & -x`trích xuất một tập bit. Loại bỏ bit đó mang lại`t = s ^ bit`. Việc lặp qua các bit đã đặt thay vì tất cả (n) vị trí sẽ tránh được các bước kiểm tra không cần thiết và truy cập chính xác các chuyển đổi (\operatorname{popcount}(s)). 

biểu thức`red[s] - red[t]`là tổng chi phí màu đỏ cho chính xác các mặt nạ con đó của`s`chứa phần tử bị loại bỏ. Biểu thức màu xanh tương ứng có cách giải thích tương tự. Vì số nguyên Python có độ chính xác tùy ý nên tổng chi phí có thể đạt khoảng (2^{20}\cdot10^9), không yêu cầu xử lý tràn đặc biệt. 

Thứ tự số của mặt nạ cũng đủ cho thứ tự DP. Việc loại bỏ một bit đã đặt luôn tạo ra mặt nạ nhỏ hơn, vì vậy`dp[t]`có sẵn trước đây`dp[s]`được đánh giá. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
2
-5 9 9 -5
10 -8 -6 3
```Sau khi chuyển đổi SOS, chi phí tích lũy là 

| (Các) mặt nạ | (R[s]) | (B[s]) | 
| --- | --- | --- | 
| 0 | -5 | 10 | 
| 1 | 4 | 2 | 
| 2 | 4 | 4 | 
| 3 | 8 | -1 | 

DP tiến hành như sau. 

| Mặt nạ | Đã xóa bit | Mặt nạ trước | Chi phí bổ sung | Ứng viên | (dp) | 
| --- | --- | --- | --- | --- | --- | 
| 0 | | | | | -5 | 
| 1 | 1 | 0 | (\min(9,-8)=-8) | -13 | -13 | 
| 2 | 2 | 0 | (\min(9,-6)=-6) | -11 | -11 | 
| 3 | 1 | 2 | (\min(4,-5)=-5) | -16 | -16 | 
| 3 | 2 | 1 | (\min(4,-3)=-3) | -16 | -16 | 

Câu trả lời cuối cùng là`-16`, phù hợp với mẫu 

Phần thú vị là mặt nạ`3`. Có hai lựa chọn có thể có cho phần tử đặc biệt. Cả hai đều dẫn đến sự tối ưu như nhau ở đây, nhưng sự tái diễn chỉ cần sự chuyển đổi tốt hơn. 

### Mẫu 2 

Đầu vào là```
3
-15 19 19 -5 30 -3 -16 13
29 -6 -14 -7 24 -5 18 11
```Sau SOS DP, chúng tôi nhận được 

| Mặt nạ | (R[mặt nạ]) | (B[mặt nạ]) | 
| --- | --- | --- | 
| 0 | -15 | 29 | 
| 1 | 4 | 23 | 
| 2 | 4 | 15 | 
| 3 | 18 | 2 | 
| 4 | 15 | 53 | 
| 5 | 31 | 42 | 
| 6 | 18 | 57 | 
| 7 | 42 | 50 | 

Bây giờ các trạng thái DP quan trọng là 

| Mặt nạ | Đã xóa bit | Mặt nạ trước | Chi phí bổ sung | Ứng viên | (dp) | 
| --- | --- | --- | --- | --- | --- | 
| 0 | | | | | -15 | 
| 1 | 1 | 0 | -6 | -21 | -21 | 
| 2 | 2 | 0 | -14 | -29 | -29 | 
| 3 | 1 | 2 | -13 | -42 | -42 | 
| 3 | 2 | 1 | -21 | -42 | -42 | 
| 4 | 4 | 0 | 24 | 9 | 9 | 
| 5 | 1 | 4 | -11 | -2 | -2 | 
| 5 | 4 | 1 | 19 | -2 | -2 | 
| 6 | 2 | 4 | 4 | 13 | -15 | 
| 6 | 4 | 2 | 28 | -1 | -15 | 
| 7 | 1 | 6 | -7 | -22 | -22 | 
| 7 | 2 | 5 | 8 | 6 | -22 | 
| 7 | 4 | 3 | 48 | 6 | -22 | 

Giá trị cuối cùng là`-22`, đó là câu trả lời mẫu. 

Dấu vết này chứng tỏ tại sao DP phải xem xét mọi phần tử có thể bị loại bỏ. Sự chuyển đổi tốt nhất thành một tập hợp không cần phải tương ứng với bất kỳ vị trí bit cố định nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n2^n)) | SOS DP thực hiện (n2^{n-1}) phép cộng và mặt nạ DP kiểm tra tối đa (n) bit trên mỗi mặt nạ | 
| Không gian | (O(2^n)) | Hai mảng chi phí được chuyển đổi và một mảng DP chứa (2^n) giá trị mỗi mảng | 

Với (n=20), chỉ có (1.048.576) tập con. Thuật toán thực hiện một số thao tác tuyến tính trên mỗi tập hợp con, thay vì xem xét các cặp tập hợp con. Đó là sự khác biệt giữa khoảng hàng chục triệu hoạt động và (3,49) tỷ mối quan hệ tập hợp con của phương pháp tiếp cận vũ phu (O(3^n)). Giới hạn bộ nhớ chính thức là (256) MiB và giới hạn thời gian là hai giây. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây sử dụng tương tự`solve`hoạt động như giải pháp được gửi. Thử nghiệm kích thước tối đa sử dụng (n=20) với chi phí giống nhau, rất hữu ích để kiểm tra cả số lượng tập hợp con và ranh giới hiệu suất.```
# Save the submitted solution in solution.py first.
from solution import solve

def run(inp: str) -> str:
    return solve(inp).strip()

# Provided sample 1
assert run(
    """2
-5 9 9 -5
10 -8 -6 3
"""
) == "-16", "sample 1"

# Provided sample 2
assert run(
    """3
-15 19 19 -5 30 -3 -16 13
29 -6 -14 -7 24 -5 18 11
"""
) == "-22", "sample 2"

# Provided sample 3, minimum-size instance
assert run(
    """0
-129363358
227605714
"""
) == "-129363358", "empty ground set"

# Custom: n = 1, negative costs
assert run(
    """1
-5 -10
0 -1
"""
) == "-15", "negative costs"

# Custom: all costs equal
assert run(
    """2
5 5 5 5
7 7 7 7
"""
) == "20", "all-equal costs"

# Custom: n = 20, maximum number of subsets.
# Every subset costs 0 in red and 1 in blue, so painting everything red is optimal.
n = 20
cnt = 1 << n
maximum_input = (
    str(n) + "\n" +
    ("0 " * (cnt - 1)) + "0\n" +
    ("1 " * (cnt - 1)) + "1\n"
)
assert run(maximum_input) == "0", "maximum-size instance"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`, một màu đỏ và một màu xanh giá |`-129363358`| Bộ đất trống và hộp đựng | 
|`1`, chi phí âm hỗn hợp |`-15`| Giá trị âm và ranh giới đơn lẻ | 
|`2`, mọi chi phí màu đỏ`5`, mỗi giá xanh`7`|`20`| Chi phí bằng nhau cho mỗi màu và tô màu toàn màu đỏ không hạn chế | 
|`20`, tất cả chi phí màu đỏ`0`, tất cả chi phí màu xanh`1`|`0`| Số lượng tập hợp con tối đa và ranh giới lập chỉ mục (2^n) | 

## Vỏ cạnh 

Với (n=0), không gian mặt nạ chỉ chứa mặt nạ`0`. Biến đổi SOS không có gì để làm và DP khởi tạo`dp[0]`với mức giá rẻ hơn trong hai chi phí. Đối với đầu vào bê tông```
0
-129363358
227605714
```thuật toán tính toán`dp[0] = min(-129363358, 227605714)`, cho`-129363358`. Đây chính xác là hành vi bắt buộc đối với tập hợp con trống duy nhất. 

Đối với chi phí âm, hãy xem xét```
1
-5 -10
0 -1
```Tập con trống có giá trị rẻ nhất là màu đỏ, tại`-5`, và singleton cũng có màu đỏ rẻ nhất, ở mức`-10`, vậy câu trả lời là`-15`. DP bắt đầu với`dp[0] = -5`. Đối với mặt nạ`1`, gỡ bỏ mặt nạ lá duy nhất của nó`0`; mức tăng màu đỏ là`-10`, trong khi mức tăng màu xanh lam là`-1`. Mức tăng tối thiểu là`-10`, sản xuất`dp[1] = -15`. Không có giả định nào về tính tích cực của chi phí xuất hiện ở bất kỳ đâu trong sự tái diễn. 

Với mọi chi phí bằng nhau, hãy xem xét```
2
5 5 5 5
7 7 7 7
```Mỗi một trong bốn tập hợp con có giá`5`màu đỏ và`7`màu xanh lam. Tô màu mọi tập hợp con màu đỏ là hợp lệ vì họ màu đỏ là toàn bộ tập hợp lũy thừa và đóng liên minh một cách tầm thường. Câu trả lời là do đó`4*5 = 20`. Phép truy hồi tìm thấy kết quả tương tự vì mọi chuyển đổi đều chọn mức tăng màu đỏ của`5`trên mỗi tập hợp con mới bị ép buộc thay vì mức tăng màu xanh của`7`. 

Ranh giới kích thước tối đa là (n=20), trong đó mặt nạ`2^20-1 = 1,048,575`là chỉ số hợp lệ lớn nhất. Một lỗi triển khai phổ biến là vô tình phân bổ`2^n-1`các mục nhập hoặc sử dụng một vòng lặp bao gồm truy cập chỉ mục`2^n`. Giải pháp phân bổ chính xác`1 << n`mục nhập và xử lý mặt nạ từ`1`bởi vì`m-1`, do đó, mặt nạ lớn nhất được xử lý chính xác một lần mà không có quyền truy cập ngoài giới hạn. 

Trường hợp cạnh logic chính là một tập hợp có màu tối ưu chứa cả hai màu. Phép truy toán không cho rằng toàn bộ tập hợp là đơn sắc. Thay vào đó, nó chỉ dựa vào sự tồn tại của một phần tử mà toàn bộ chứa một nửa mạng tập hợp con có cùng màu với tập hợp đầy đủ. Khi phần tử đó được cố định, nửa còn lại sẽ được tối ưu hóa đệ quy. Đây chính xác là điều làm cho điều kiện đóng hợp trở nên hữu ích mà không cần phải liệt kê các màu hoàn chỉnh.
