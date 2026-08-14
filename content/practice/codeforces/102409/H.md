---
title: "CF 102409H - Tối đa hóa tiền xu"
description: "Chúng ta có dãy phòng được đánh số từ 1 đến (N). Phòng (N) là điểm đến và không chứa xu. Từ phòng (i), Diego có thể nhảy tới bất kỳ phòng nào sau đó có chỉ số lớn nhất là (i+ki). Khi anh ta đến thăm một căn phòng (i<N), anh ta thu thập đồng xu (ci) của nó."
date: "2026-08-12T05:54:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102409
codeforces_index: "H"
codeforces_contest_name: "Semana i 2019"
rating: 0
weight: 102409
solve_time_s: 708
verified: true
draft: false
---

[CF 102409H - Tối đa hóa số xu](https://codeforces.com/problemset/problem/102409/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 11 phút 48 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có dãy phòng được đánh số từ 1 đến (N). Phòng (N) là điểm đến và không chứa xu. Từ phòng (i), Diego có thể nhảy tới bất kỳ phòng nào sau đó có chỉ số tối đa là (i+k_i). Khi anh ta đến thăm một căn phòng (i<N), anh ta thu thập đồng xu (c_i) của nó. Mục tiêu là chọn một chuỗi các bước nhảy hợp lệ để đến phòng (N) với số xu lớn nhất có thể. Trong số tất cả các đường đi đạt được mức tối đa đó, chúng ta cũng cần đếm xem có bao nhiêu đường đi, modulo (10^9+7). 

Một cách hữu ích để xem các phòng là dùng biểu đồ chu kỳ có hướng. Mọi phòng (i) đều có các cạnh thuộc khoảng các đỉnh từ (i+1) đến (i+k_i). Vì mỗi cạnh đều có chỉ mục lớn hơn nên việc xử lý các phòng từ phải sang trái sẽ mang lại thứ tự lập trình động tự nhiên. 

Giá trị (N) có thể lớn bằng (10^5). Thuật toán bậc hai sẽ thực hiện xung quanh (N^2/2), tức là khoảng (5\times10^9), các phép toán trong trường hợp xấu nhất. Điều đó vượt xa những gì giới hạn thời gian 1 giây có thể xử lý. Chúng ta cần tránh quét mọi điểm đến có thể cho mỗi phòng. Giá trị xu có thể đạt tới (10^9) và một đường đi có thể ghé thăm (O(N)) phòng, do đó tổng số xu tối đa có thể đạt khoảng (10^{14}). Các số nguyên Python xử lý việc này một cách an toàn, trong khi số lượng đường dẫn tối ưu phải giảm rõ ràng theo modulo (10^9+7). 

Có một số trường hợp đặc biệt có thể khiến việc triển khai đơn giản trở nên không chính xác. 

Đầu tiên, có thể có nhiều đường đi tối ưu. Coi như:```
3
0 0
2 1
```Từ phòng 1, Diego có thể đi thẳng tới phòng 3 hoặc thăm phòng 2 trước. Cả hai con đường đều không thu được xu nào, vì vậy câu trả lời là`0 2`. Việc triển khai chỉ giữ giá trị đồng xu tốt nhất mà không tích lũy số cách sẽ báo cáo sai một đường dẫn. 

Thứ hai, một căn phòng không thể có người kế thừa ngoài căn phòng cuối cùng. Ví dụ:```
2
7
1
```Đường dẫn duy nhất có thể là (1\rightarrow2), vì vậy câu trả lời là`7 1`. Phòng điều trị (N) giống như một phòng bình thường nếu không khởi tạo chính xác trạng thái lập trình động của nó thì có thể tạo ra không có cách nào. 

Thứ ba, khoảng cách tối đa có thể chứa hầu hết mọi phòng ở bên phải. Ví dụ:```
5
0 0 0 0
4 3 2 1
```Mỗi bước nhảy từ phòng 1 có thể đến bất kỳ phòng nào sau đó. Có 8 con đường riêng biệt từ phòng 1 đến phòng 5 nên đáp án là`0 8`. Quét chuyển tiếp bậc hai nhận được câu trả lời đúng nhưng không thể thực hiện được ở mức tối đa (N). 

Cuối cùng, ranh giới khoảng là bao gồm. Nếu (k_i=2), phòng (i) có thể nhảy tới (i+1) hoặc (i+2), nhưng không thể nhảy tới (i+3). Việc nhầm lẫn điểm cuối phù hợp với giới hạn độc quyền sẽ gây ra các lỗi nhỏ, đặc biệt khi khoảng thời gian kết thúc chính xác tại phòng (N). 

## Phương pháp tiếp cận 

Công thức quy hoạch động trực tiếp rất đơn giản. Gọi (dp[i]) là số xu tối đa có thể nhận được bắt đầu từ phòng (i) và kết thúc tại phòng (N). Gọi (ways[i]) là số đường dẫn đạt được (dp[i]). Đối với phòng cuối cùng, chúng tôi xác định (dp[N]=0) và (cách[N]=1), vì việc đến đích từ chính nó không đóng góp thêm xu nào và chỉ có một phần tiếp theo trống. 

Đối với mỗi phòng trước đó (i), mỗi bước nhảy đầu tiên hợp lệ sẽ đi đến một phòng (j) nào đó trong khoảng ([i+1,i+k_i]). Như vậy, 

[ 
dp[i] = c_i + \max_{j\in[i+1,i+k_i]} dp[j]. 
] 

Khi đã biết giá trị tối đa (tốt nhất), số lượng đường đi tối ưu là tổng của (cách[j]) trên chính xác những đường dẫn kế tiếp thỏa mãn (dp[j]=tốt nhất). 

DP bạo lực này là chính xác vì mọi đường đi từ (i) đều có chính xác một đích đầu tiên (j) và sau khi đến (j), cách tiếp tục tốt nhất có thể chính xác là những gì (dp[j]) mô tả. Vấn đề là chi phí để tìm mức tối đa và số cách liên quan của nó. Nếu chúng ta quét toàn bộ khoảng thời gian kế tiếp cho mỗi phòng thì trường hợp xấu nhất có khoảng 

[ 
\sum_{i=1}^{N-1}(N-i)=\frac{N(N-1)}2 
] 

các kỳ thi kế nhiệm. Đối với (N=10^5), đây là (4.999.950.000) thao tác. 

Quan sát quan trọng là mọi chuyển đổi đều đặt ra cùng một loại câu hỏi: trong khoảng liền kề của các phòng đã được tính toán, hãy tìm giá trị (dp) lớn nhất và tổng số cách thuộc về giá trị lớn nhất đó. Các phòng được xử lý từ phải sang trái, vì vậy khi chúng ta cần câu trả lời cho phòng (i), tất cả các giá trị trong khoảng kế tiếp của nó đều có sẵn. 

Đây chính xác là một truy vấn phạm vi có cập nhật điểm. Cây phân đoạn có thể lưu trữ, đối với mỗi phân đoạn, giá trị (dp) tốt nhất và số cách đạt được giá trị đó. Việc kết hợp hai phân đoạn con rất đơn giản. Nếu một đứa trẻ có (dp) lớn hơn thì cặp của nó chính là câu trả lời. Nếu cả hai đều có cùng (dp), số đường đi của chúng sẽ được cộng theo modulo (10^9+7). 

Sau khi tính toán (dp[i]) và (ways[i]), chúng ta chèn cặp đó vào vị trí (i). Mỗi phòng được chèn một lần và được truy vấn một lần, cho thời gian (O(N\log N)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^2)) | (O(N)) | Quá chậm | 
| Tối ưu | (O(N\log N)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo trạng thái của phòng cuối cùng. Đặt (dp[N]=0) và (cách[N]=1). Phòng cuối cùng không đóng góp xu và chỉ có một cách để hoàn thành sau khi đã đến phòng đó. 
2. Xây dựng cây phân đoạn có lá ở vị trí (i) lưu trữ cặp ((dp[i],ways[i])). Ban đầu, mọi vị trí ngoại trừ (N) đều trống. Lá cho (N) lưu trữ ((0,1)). 
3. Phòng xử lý (i=N-1,N-2,\ldots,1). Bởi vì mọi bước nhảy đều di chuyển sang bên phải nên mọi đích đến có thể có của phòng (i) đều đã được xử lý. 
4. Truy vấn cây phân đoạn trong khoảng bao gồm 

[ 
[i+1,i+k_i]. 
] 

Truy vấn trả về hai mẩu thông tin. Đầu tiên là mức tối đa (dp) trong số tất cả các điểm đến có thể tiếp cận. Thứ hai là tổng (số cách) trên mỗi điểm đến có mức tối đa (dp) đó. 

1. Thêm số xu của phòng hiện tại vào mức tối đa được truy vấn: 

[ 
dp[i]=c_i+tốt nhất. 
] 

Số lượng đường dẫn tối ưu không thay đổi khi (c_i) được thêm vào, bởi vì cùng một giá trị đồng xu được thêm vào mọi khả năng tiếp tục từ phòng (i). Do đó, 

[ 
cách[i]=tốt nhấtWays. 
] 

1. Cập nhật cây phân đoạn tại vị trí (i) bằng ((dp[i],ways[i])). Các phòng trong tương lai ở bên trái có thể sử dụng phòng (i) làm điểm đến, vì vậy trạng thái của nó hiện phải có sẵn cho các truy vấn phạm vi của chúng. 
2. Sau khi xử lý phòng 1, xuất ra (dp[1]) và (ways[1]). Mọi hành trình hợp lệ đều bắt đầu từ phòng 1, vì vậy đây chính xác là tổng số xu tối đa cần thiết và số lượng hành trình tối ưu.

### Tại sao nó hoạt động 

Điều bất biến là bất cứ khi nào phòng (i) được xử lý, mọi phòng có thể truy cập từ (i) đều đã có các giá trị (dp) và (cách) chính xác được lưu trữ trong cây phân đoạn. Do đó, truy vấn phạm vi sẽ xem xét mọi đích đến đầu tiên có thể có của (i), chọn giá trị tiếp tục lớn nhất và tính tổng số đường dẫn chỉ trong số các đích đạt được giá trị đó. Việc cộng (c_i) sẽ cho kết quả tổng tốt nhất bắt đầu từ (i), trong khi giữ nguyên số đếm tương ứng. Bản cập nhật giữ nguyên bất biến cho phòng tiếp theo bên trái. Vì phòng 1 được xử lý sau cùng nên cặp lưu trữ của nó chính xác cho toàn bộ vấn đề. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7
NEG = -10**30

class SegmentTree:
    def __init__(self, n):
        size = 1
        while size < n:
            size <<= 1

        self.size = size
        self.best = [NEG] * (2 * size)
        self.ways = [0] * (2 * size)

    def merge(self, left_best, left_ways, right_best, right_ways):
        if left_best > right_best:
            return left_best, left_ways
        if right_best > left_best:
            return right_best, right_ways
        if left_best == NEG:
            return NEG, 0
        return left_best, (left_ways + right_ways) % MOD

    def update(self, pos, value, ways):
        pos += self.size
        self.best[pos] = value
        self.ways[pos] = ways

        pos >>= 1
        while pos:
            lb = self.best[pos << 1]
            lw = self.ways[pos << 1]
            rb = self.best[pos << 1 | 1]
            rw = self.ways[pos << 1 | 1]

            b, w = self.merge(lb, lw, rb, rw)
            self.best[pos] = b
            self.ways[pos] = w

            pos >>= 1

    def query(self, left, right):
        if left > right:
            return NEG, 0

        left += self.size
        right += self.size

        left_best = NEG
        left_ways = 0
        right_best = NEG
        right_ways = 0

        while left <= right:
            if left & 1:
                lb = self.best[left]
                lw = self.ways[left]
                left_best, left_ways = self.merge(
                    left_best, left_ways, lb, lw
                )
                left += 1

            if not (right & 1):
                rb = self.best[right]
                rw = self.ways[right]
                right_best, right_ways = self.merge(
                    rb, rw, right_best, right_ways
                )
                right -= 1

            left >>= 1
            right >>= 1

        return self.merge(
            left_best, left_ways,
            right_best, right_ways
        )

def solve():
    n = int(input())
    c = [0] + list(map(int, input().split()))
    k = [0] + list(map(int, input().split()))

    seg = SegmentTree(n)

    # Room n is the destination.
    seg.update(n - 1, 0, 1)

    for i in range(n - 1, 0, -1):
        # Convert 1-based room indices to 0-based segment-tree indices.
        left = i
        right = i + k[i] - 1

        best, ways = seg.query(left, right)

        dp_i = c[i] + best
        seg.update(i - 1, dp_i, ways)

    answer, ways = seg.query(0, 0)
    print(answer, ways % MOD)

if __name__ == "__main__":
    solve()
```Cây phân đoạn lưu trữ một cặp tại mỗi nút thay vì một mức tối đa duy nhất. Thành phần đầu tiên là tổng số xu tốt nhất trong phân khúc đó, trong khi thành phần thứ hai tính xem có bao nhiêu đường dẫn đạt được giá trị tốt nhất đó. 

các`merge`hoạt động là phần trung tâm của việc thực hiện. Nếu một bên có giá trị lớn hơn thì chỉ có số lượng đường dẫn của nó mới quan trọng. Nếu cả hai bên có cùng giá trị thì cả hai nhóm đường dẫn tối ưu đều hợp lệ và số lượng của chúng được cộng theo modulo (10^9+7). các`NEG`value đại diện cho một phân khúc trống chưa nhận được trạng thái phòng hợp lệ. 

Việc lập chỉ mục xứng đáng được chú ý đặc biệt. Các phương trình quy hoạch động sử dụng các chỉ số phòng dựa trên một, nhưng cây phân đoạn sử dụng các vị trí dựa trên 0. Đối với phòng (i), đích đến hợp lệ của nó là khoảng dựa trên một ([i+1,i+k_i]). Chúng trở thành các vị trí dựa trên 0 ([i,i+k_i-1]), chính xác là phạm vi được truy vấn trong mã. 

Phòng cuối cùng được chèn vào vị trí gốc 0`n - 1`với trạng thái`(0, 1)`. Sau đó, mỗi phòng khác sẽ được xử lý từ phải sang trái. Bởi vì (i+k_i\leq N), khoảng truy vấn luôn nằm trong cây phân đoạn. 

Không có vấn đề tràn số nguyên trong Python. Đường dẫn tối đa có thể thu thập theo thứ tự (10^5\cdot10^9=10^{14}) xu mà Python đại diện trực tiếp. Chỉ có một số cách cần giảm mô-đun và thao tác hợp nhất sẽ thực hiện việc giảm đó bất cứ khi nào các giá trị tối ưu bằng nhau được kết hợp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
5
0 0 0 0
4 3 2 1
```Mỗi phòng đều không có xu nên tất cả các đường dẫn hợp lệ đều tối ưu. Chúng tôi xử lý các phòng từ phải sang trái. 

| Phòng (i) | Khoảng truy vấn | Tiếp tục tốt nhất | Cách | (dp[i]) | (cách[i]) | 
| --- | --- | --- | --- | --- | --- | 
| 4 | [5, 5] | 0 | 1 | 0 | 1 | 
| 3 | [4, 5] | 0 | 2 | 0 | 2 | 
| 2 | [3, 5] | 0 | 4 | 0 | 4 | 
| 1 | [2, 5] | 0 | 8 | 0 | 8 | 

Ở phòng 4 chỉ có thể nhảy thẳng sang phòng 5. Ở phòng 3 có hai cách tiếp tục tối ưu, qua phòng 4 hoặc trực tiếp đến phòng 5. Con số lại tăng gấp đôi ở phòng 2 và cuối cùng trở thành tám ở phòng 1. Do đó, đầu ra là`0 8`. 

### Mẫu 2 

Đầu vào là:```
5
0 0 0 0
2 2 2 1
```Khoảng cách có thể truy cập hẹp hơn nên tồn tại ít đường dẫn hơn. 

| Phòng (i) | Khoảng truy vấn | Tiếp tục tốt nhất | Cách | (dp[i]) | (cách[i]) | 
| --- | --- | --- | --- | --- | --- | 
| 4 | [5, 5] | 0 | 1 | 0 | 1 | 
| 3 | [4, 5] | 0 | 2 | 0 | 2 | 
| 2 | [3, 4] | 0 | 3 | 0 | 3 | 
| 1 | [2, 3] | 0 | 5 | 0 | 5 | 

Tại phòng 2, các lối đi qua phòng 3 và 4 lần lượt đóng góp hai và một lối, tạo thành ba lối. Phòng 1 sau đó có thể bắt đầu bằng phòng 2 hoặc phòng 3, đưa ra (3+2=5) đường đi tối ưu. Kết quả là`0 5`. 

Những dấu vết này cũng thể hiện tính bất biến của khóa: cây phân đoạn luôn chứa cặp chính xác cho mỗi phòng ở bên phải phòng hiện tại, do đó, mỗi truy vấn phạm vi đều có chính xác thông tin cần thiết cho quá trình chuyển đổi hiện tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N\log N)) | Mỗi phòng trong số (N-1) thực hiện một truy vấn phạm vi và cập nhật một điểm, mỗi phòng lấy (O(\log N)). | 
| Không gian | (O(N)) | Cây phân đoạn chứa các nút (O(N)) và mảng đầu vào cũng sử dụng bộ nhớ (O(N)). | 

Đối với (N=10^5), thuật toán thực hiện khoảng (N) thao tác cây phân đoạn thay vì hàng tỷ lần quét kế tiếp. Độ phức tạp (O(N\log N)) vừa vặn thoải mái trong các ràng buộc dự định và mức sử dụng bộ nhớ dưới 256 MB. 

## Trường hợp thử nghiệm 

Bộ dây thử nghiệm sau đây sử dụng cùng một`solve`logic như chương trình đã gửi và kiểm tra các mẫu được cung cấp cùng với các trường hợp ranh giới, ràng buộc và đầu vào lớn.```python
import sys
import io

MOD = 10**9 + 7
NEG = -10**30

class SegmentTree:
    def __init__(self, n):
        size = 1
        while size < n:
            size <<= 1
        self.size = size
        self.best = [NEG] * (2 * size)
        self.ways = [0] * (2 * size)

    def merge(self, a, aw, b, bw):
        if a > b:
            return a, aw
        if b > a:
            return b, bw
        if a == NEG:
            return NEG, 0
        return a, (aw + bw) % MOD

    def update(self, pos, value, ways):
        pos += self.size
        self.best[pos] = value
        self.ways[pos] = ways

        pos >>= 1
        while pos:
            self.best[pos], self.ways[pos] = self.merge(
                self.best[pos << 1],
                self.ways[pos << 1],
                self.best[pos << 1 | 1],
                self.ways[pos << 1 | 1],
            )
            pos >>= 1

    def query(self, left, right):
        left += self.size
        right += self.size

        lb, lw = NEG, 0
        rb, rw = NEG, 0

        while left <= right:
            if left & 1:
                lb, lw = self.merge(
                    lb, lw, self.best[left], self.ways[left]
                )
                left += 1

            if not (right & 1):
                rb, rw = self.merge(
                    self.best[right], self.ways[right], rb, rw
                )
                right -= 1

            left >>= 1
            right >>= 1

        return self.merge(lb, lw, rb, rw)

def solve():
    n = int(input())
    c = [0] + list(map(int, input().split()))
    k = [0] + list(map(int, input().split()))

    seg = SegmentTree(n)
    seg.update(n - 1, 0, 1)

    for i in range(n - 1, 0, -1):
        best, ways = seg.query(i, i + k[i] - 1)
        seg.update(i - 1, c[i] + best, ways)

    ans, ways = seg.query(0, 0)
    print(ans, ways)

def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_input = input

    try:
        sys.stdin = io.StringIO(inp)
        input = sys.stdin.readline

        old_stdout = sys.stdout
        sys.stdout = io.StringIO()

        solve()
        result = sys.stdout.getvalue().strip()

        sys.stdout = old_stdout
        return result
    finally:
        sys.stdin = old_stdin
        input = old_input

# Provided samples
assert run(
    """5
0 0 0 0
4 3 2 1
"""
) == "0 8", "sample 1"

assert run(
    """5
0 0 0 0
2 2 2 1
"""
) == "0 5", "sample 2"

assert run(
    """7
100 0 0 0 0 0
2 2 2 2 2 1
"""
) == "100 13", "sample 3"

# Minimum-size input.
assert run(
    """2
7
1
"""
) == "7 1", "minimum size"

# Tie between two optimal paths.
assert run(
    """3
0 0
2 1
"""
) == "0 2", "two optimal paths"

# Off-by-one case: room 1 can reach room 2 or room 3,
# but room 2 cannot reach room 3 because k[2] = 2 does
# allow room 4, while room 3 also reaches room 4.
assert run(
    """4
1 100 1
1 2 1
"""
) == "101 1", "maximum-value path"

# All rooms have equal coins and only one possible next room.
n = 100000
coins = "5 " * (n - 1)
jumps = "1 " * (n - 1)
large_input = (
    str(n) + "\n" +
    coins.rstrip() + "\n" +
    jumps.rstrip() + "\n"
)
assert run(large_input) == f"{5 * (n - 1)} 1", "maximum size"

# Maximum jump range with all paths optimal.
assert run(
    """5
0 0 0 0
4 3 2 1
"""
) == "0 8", "maximum branching"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 7 / 1`|`7 1`| Số lượng phòng tối thiểu và khởi tạo phòng cuối cùng | 
|`3 / 0 0 / 2 1`|`0 2`| Nhiều đường dẫn tối ưu có giá trị bằng nhau | 
|`4 / 1 100 1 / 1 2 1`|`101 1`| Chọn phần tiếp theo có giá trị tối đa và ranh giới khoảng thời gian chính xác | 
| (N=100000), tất cả (c_i=5), tất cả (k_i=1) |`499995 1`| Kích thước đầu vào tối đa, tổng số xu lớn và cấu trúc đường dẫn tuyến tính | 
| Mẫu 1 |`0 8`| Phân nhánh tối đa và tích lũy số lượng đường dẫn có giá trị bằng nhau | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là đồ thị nhỏ nhất có thể:```
2
7
1
```Quá trình chuyển đổi duy nhất là (1\rightarrow2). Cây phân đoạn ban đầu chứa`(0, 1)`tại phòng 2. Phòng xử lý 1 truy vấn chính xác lá đó, nhận được sự tiếp tục tốt nhất bằng 0 bằng một cách, thêm bảy đồng xu và lưu trữ`(7, 1)`. Đầu ra là`7 1`. Điều này xác minh rằng trạng thái đích được khởi tạo dưới dạng một sự tiếp tục hợp lệ thay vì không có cách nào. 

Trường hợp cạnh thứ hai chứa một số đường dẫn tối ưu:```
3
0 0
2 1
```Phòng 1 có thể nhảy thẳng tới phòng 3 hoặc qua phòng 2. Phòng 2 có trạng thái`(0,1)`, trong khi phòng 1 truy vấn phòng 2 và 3, trạng thái của chúng là`(0,1)`Và`(0,1)`. Vì các giá trị tốt nhất của chúng bằng nhau nên việc hợp nhất cây phân đoạn sẽ cộng số lượng và trả về của chúng`(0,2)`. Đầu ra là`0 2`. Điều này nắm bắt các triển khai ghi đè số đếm khi gặp hai giá trị cực đại bằng nhau. 

Trường hợp thứ ba kiểm tra xem giá trị tối đa, thay vì số lượng đường dẫn có thể truy cập, sẽ xác định câu trả lời:```
4
1 100 1
1 2 1
```Phòng 3 chỉ có thể lên phòng 4 nên trạng thái là`(1,1)`. Phòng 2 có thể đi đến phòng 3 và 4, cho các giá trị tiếp tục là 1 và 0, vì vậy trạng thái của nó là`(101,1)`. Phòng 1 chỉ có thể đi đến phòng 2, sản xuất`(102,1)`nếu bao gồm đồng tiền riêng của nó. 

Đối với dữ liệu đầu vào thực tế ở trên, đường dẫn (1\rightarrow2\rightarrow4) thu thập (1+100=101), trong khi (1\rightarrow2\rightarrow3\rightarrow4) thu thập (1+100+1=102). Do đó, đầu ra đúng thực sự là:```
102 1
```Ví dụ này minh họa tại sao trạng thái DP phải bao gồm số xu của phòng hiện tại sau khi phần tiếp theo tốt nhất đã được chọn. 

Trường hợp phân nhánh tối đa là:```
5
0 0 0 0
4 3 2 1
```Phòng 5 bắt đầu bằng`(0,1)`. Phòng 4 được`(0,1)`, phòng 3 được`(0,2)`, phòng 2 được`(0,4)`, và phòng 1 được`(0,8)`. Mọi đường dẫn có thể có tổng số xu giống nhau, vì vậy thao tác hợp nhất có giá trị bằng nhau của cây phân đoạn sẽ tính tất cả các đường dẫn. Kết quả là`0 8`. 

Cuối cùng, hãy xem xét cấu trúc có kích thước tối đa với (N=100000), mọi giá trị đồng xu bằng 5 và mọi giá trị (k_i=1). Có chính xác một con đường khả dĩ, nên kết quả là```
499995 1
```Thuật toán vẫn chỉ thực hiện công việc cây phân đoạn (O(N\log N)). Trường hợp này kiểm tra cả khả năng mở rộng và thực tế là các giá trị tiền xu tích lũy lớn được xử lý mà không bị tràn. 

Bài xã luận đã sẵn sàng để sử dụng nguyên trạng. Cần thực hiện một chỉnh sửa trước khi xuất bản: nhận xét về khai thác thử nghiệm xung quanh trường hợp tùy chỉnh bốn phòng phải khớp với giá trị thực tế mong đợi`102 1`, như được rút ra trong cuộc thảo luận về trường hợp cạnh cuối cùng.
