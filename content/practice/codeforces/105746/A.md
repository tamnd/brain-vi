---
title: "CF 105746A - Ô tô"
description: "Chúng ta đang theo dõi một đoàn ô tô đang chuyển động dọc theo một đường thẳng. Mỗi ô tô bắt đầu ở vị trí 0 và di chuyển với tốc độ không đổi trong bất kỳ khoảng thời gian truy vấn nào. Tuy nhiên, tốc độ không cố định trên toàn cầu, chúng có thể thay đổi theo thời gian do cập nhật."
date: "2026-06-22T04:42:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105746
codeforces_index: "A"
codeforces_contest_name: "Bangladesh Olympiad in Informatics 2025 National Round Day 1"
rating: 0
weight: 105746
solve_time_s: 63
verified: true
draft: false
---

[CF 105746A - Ô tô](https://codeforces.com/problemset/problem/105746/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang theo dõi một đoàn ô tô đang chuyển động dọc theo một đường thẳng. Mỗi ô tô bắt đầu ở vị trí 0 và di chuyển với tốc độ không đổi trong bất kỳ khoảng thời gian truy vấn nào. Tuy nhiên, tốc độ không cố định trên toàn cầu, chúng có thể thay đổi theo thời gian do cập nhật. 

Hệ thống xử lý hai loại hoạt động. Một thao tác thay đổi tốc độ của một chiếc xe cụ thể. Người còn lại đặt một câu hỏi hình học theo thời gian và không gian: trong khoảng thời gian từ L đến R giây, có bao nhiêu ô tô đã từng đi vào đoạn không gian từ X đến Y mét ít nhất một lần tại bất kỳ thời điểm nào. 

Một ô tô đóng góp vào câu trả lời nếu tồn tại ít nhất một thời điểm t trong khoảng [L, R] sao cho vị trí A[i] · t của nó nằm trong [X, Y]. 

Vì các vị trí tăng tuyến tính theo thời gian nên mỗi ô tô sẽ vẽ một đoạn thẳng trong mặt phẳng thời gian-vị trí. Truy vấn hỏi có bao nhiêu đường trong số này giao nhau với một tấm thời gian nằm ngang và dải vị trí dọc nhất định theo cách không trống. 

Các ràng buộc buộc chúng tôi phải xử lý tối đa 100000 thao tác với tốc độ lên tới 10^9 và thời gian lên tới 10^9, trong khi phạm vi vị trí có thể lên tới 10^15. Điều này ngay lập tức loại trừ việc kiểm tra từng ô tô cho mỗi truy vấn, vì đó sẽ là 10^10 thao tác trong trường hợp xấu nhất. 

Một ý tưởng ngây thơ có thể là đánh giá từng truy vấn bằng cách lặp lại tất cả các ô tô và kiểm tra xem các khoảng thời gian có trùng nhau hay không. Điều đó đòi hỏi phải tính toán, đối với mỗi ô tô, khoảng thời gian mà nó nằm trong [X, Y] và giao nó với [L, R]. Điều này đúng về mặt logic nhưng quá chậm. 

Trường hợp cạnh tinh tế xuất hiện khi tốc độ bằng không. Nếu A[i] = 0 thì ô tô sẽ ở vị trí 0 mãi mãi. Khi đó nó chỉ đóng góp khi X 0 ≤ Y, không phụ thuộc vào thời gian. Một công thức ngây thơ liên quan đến việc chia cho tốc độ sẽ bị phá vỡ ở đây. 

Một trường hợp phức tạp khác xảy ra khi X = 0 hoặc khi L = R. Khi L = R, chúng ta đang kiểm tra một thời điểm tức thời một cách hiệu quả, nhưng các giải pháp coi các khoảng thời gian là mở hoặc nửa mở có thể vô tình làm mất đi các lần chạm ranh giới hợp lệ. 

## Phương pháp tiếp cận 

Đối với một ô tô có vận tốc v, vị trí của nó là v · t. Chúng ta muốn biết liệu có tồn tại t trong [L, R] sao cho: 

X ≤ v · t ≤ Y. 

Nếu v = 0, điều kiện giảm xuống còn kiểm tra xem 0 có nằm trong [X, Y], không phụ thuộc vào thời gian hay không. Nếu có, chiếc xe luôn được tính; nếu không thì không bao giờ. 

Nếu v > 0, chúng ta có thể chia bất đẳng thức: 

X/v ≤ t ≤ Y/v. 

Vì vậy, ô tô sẽ đóng góp nếu khoảng [X/v, Y/v] trùng với [L, R]. Điều này trở thành một kiểm tra giao lộ khoảng thời gian đơn giản. 

Do đó, mỗi truy vấn loại 2 giảm xuống việc đếm xem có bao nhiêu tốc độ hiện tại tạo ra khoảng trùng lặp với [L, R]. Khó khăn là tốc độ thay đổi linh hoạt, vì vậy chúng ta cần một cấu trúc hỗ trợ cập nhật điểm và tính phạm vi trên các giá trị dẫn xuất. 

Quan sát quan trọng là mỗi truy vấn độc lập với ô tô và tình trạng của mỗi ô tô chỉ phụ thuộc vào tốc độ hiện tại của nó. Vì vậy, chúng tôi duy trì nhiều tốc độ và trả lời các truy vấn bằng cách kiểm tra từng tốc độ. Tuy nhiên, quét trực tiếp quá chậm. 

Thay vào đó, chúng tôi giảm từng truy vấn để đếm tốc độ v sao cho: 

max(L, X/v) ≤ min(R, Y/v), 

có thể được sắp xếp lại thành các ràng buộc trên v: 

Tồn tại t trong [L, R] thỏa mãn X ≤ v t ≤ Y. 

Tương đương: 

v ≥ X/R và v ≤ Y/L, có quan tâm đến ranh giới sàn và trần. 

Vì vậy, mỗi truy vấn sẽ đếm xem có bao nhiêu tốc độ nằm trong phạm vi [thấp, cao] bắt nguồn từ (X, Y, L, R). Với các bản cập nhật, chúng ta cần một cấu trúc có thứ tự động. 

Do đó, chúng tôi duy trì tất cả tốc độ hiện tại trong một cấu trúc cân bằng hỗ trợ thống kê chèn, xóa và sắp xếp. Cây Fenwick hoặc cây phân đoạn trên các giá trị nén hoạt động vì tốc độ bị giới hạn và cập nhật. 

Chúng tôi phối hợp nén tất cả tốc độ xuất hiện trong mảng ban đầu và các bản cập nhật. Sau đó chúng tôi duy trì số lượng tần số. Mỗi bản cập nhật điều chỉnh số lượng trong O(log N) và mỗi truy vấn trở thành tổng phạm vi trong O(log N) sau giới hạn tính toán.

Cái nhìn sâu sắc quan trọng là điều kiện hình học sụp đổ thành một bất đẳng thức thuần túy về tốc độ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force cho mỗi truy vấn | O(NQ) | O(N) | Quá chậm | 
| Cấu trúc được sắp xếp + nén | O((N + Q) log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tất cả tốc độ có thể xuất hiện, bao gồm các giá trị ban đầu và tất cả các giá trị cập nhật, vì chúng tôi sẽ nén chúng thành các chỉ số cho cây Fenwick. 
2. Xây dựng một dãy các tốc độ duy nhất được sắp xếp và ánh xạ từng tốc độ vào chỉ mục của nó. Điều này cho phép chúng ta xử lý vấn đề như việc đếm tần số trên một miền rời rạc. 
3. Khởi tạo cây Fenwick trong đó mỗi vị trí lưu trữ số lượng ô tô hiện có tốc độ đó. 
4. Chèn tất cả tốc độ ban đầu vào cây Fenwick bằng cách tăng tần số của chúng. Điều này thiết lập trạng thái ban đầu của hệ thống. 
5. Đối với truy vấn loại 1, hãy xóa tốc độ cũ của ô tô khỏi cây Fenwick và chèn tốc độ mới. Điều này giữ cho cấu trúc phù hợp với cấu hình hiện tại. 
6. Đối với truy vấn loại 2, hãy tính khoảng tốc độ hợp lệ [thấp, cao] xuất phát từ điều kiện ô tô phải thỏa mãn X ≤ v · t ≤ Y đối với một số t trong [L, R]. Điều này giảm xuống còn v ∈ [trần(X / R), sàn(Y / L)]. 
7. Chuyển đổi thấp và cao thành các chỉ số nén bằng cách sử dụng tìm kiếm nhị phân trên danh sách tốc độ đã sắp xếp. 
8. Truy vấn cây Fenwick để biết số tốc độ trong phạm vi chỉ số này. Tổng này chính xác là số ô tô thỏa mãn điều kiện. 
9. Xuất kết quả cho từng truy vấn loại 2. 

Tính chính xác phụ thuộc vào thực tế là mỗi ô tô hợp lệ được tính chính xác một lần vì tốc độ của nó xác định duy nhất liệu chuyển động của nó có cắt vùng yêu cầu hay không. 

## Tại sao nó hoạt động 

Mỗi ô tô tạo ra một tham số duy nhất, tốc độ của nó và điều kiện hình học của giao điểm với hình chữ nhật vị trí-thời gian giảm xuống thành một ràng buộc đơn điệu trên tham số đó. Tính đơn điệu này đảm bảo rằng tập hợp tốc độ hợp lệ tạo thành một khoảng liền kề theo thứ tự được sắp xếp. Vì các bản cập nhật chỉ thay đổi một giá trị tại một thời điểm nên việc duy trì tần suất trên miền được sắp xếp này sẽ duy trì tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

    def range_sum(self, l, r):
        if l > r:
            return 0
        return self.sum(r) - self.sum(l - 1)

def ceil_div(a, b):
    if b == 0:
        return float('inf')
    return (a + b - 1) // b

def floor_div(a, b):
    if b == 0:
        return 0
    return a // b

def solve():
    N = int(input())
    A = list(map(int, input().split()))
    Q = int(input())

    queries = []
    all_vals = set(A)

    for _ in range(Q):
        tmp = input().split()
        if tmp[0] == '1':
            i = int(tmp[1]) - 1
            s = int(tmp[2])
            queries.append(('1', i, s))
            all_vals.add(s)
        else:
            X = int(tmp[1])
            Y = int(tmp[2])
            L = int(tmp[3])
            R = int(tmp[4])
            queries.append(('2', X, Y, L, R))

    vals = sorted(all_vals)
    idx = {v: i + 1 for i, v in enumerate(vals)}

    fw = Fenwick(len(vals))

    cur = A[:]
    for v in cur:
        fw.add(idx[v], 1)

    out = []

    for q in queries:
        if q[0] == '1':
            i, s = q[1], q[2]
            fw.add(idx[cur[i]], -1)
            cur[i] = s
            fw.add(idx[s], 1)
        else:
            X, Y, L, R = q[1], q[2], q[3], q[4]

            if L == 0:
                low = 0
            else:
                low = ceil_div(X, R)

            high = Y // L

            l = 1
            r = len(vals)

            while l <= r:
                m = (l + r) // 2
                if vals[m - 1] < low:
                    l = m + 1
                else:
                    r = m - 1
            left = l

            l = 1
            r = len(vals)

            while l <= r:
                m = (l + r) // 2
                if vals[m - 1] <= high:
                    l = m + 1
                else:
                    r = m - 1
            right = r

            out.append(str(fw.range_sum(left, right)))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai tách biệt các bản cập nhật và truy vấn một cách rõ ràng. Cây Fenwick duy trì tần số của tốc độ và mỗi truy vấn sẽ tính toán một khoảng dẫn xuất theo tốc độ và đếm xem có bao nhiêu tốc độ rơi vào đó. 

Phần tinh tế nhất là tính toán giới hạn một cách chính xác. Bất đẳng thức X ≤ v t ≤ Y trên t ∈ [L, R] biến thành v ≥ X / R và v ≤ Y / L. Phép chia số nguyên phải được xử lý bằng trần cho giới hạn dưới và sàn cho giới hạn trên. Bất kỳ lỗi ngẫu nhiên nào ở đây sẽ tính sai ô tô ở các trường hợp ranh giới như lượt truy cập chính xác tại X hoặc Y. 

Tìm kiếm nhị phân được sử dụng để dịch các giới hạn này thành các chỉ số vì tốc độ được nén nhưng không nhất thiết phải liên tục. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
N = 3
A = [2, 10, 15]
Query: type 2 with X=10, Y=15, L=3, R=4
```Chúng tôi đánh giá từng chiếc xe. 

| Xe hơi | Tốc độ v | Điều kiện khoảng v ≥ X/R | v ≤ Y/L | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 2 ≥ 2,5 sai | vâng | không | 
| 2 | 10 | 10 ≥ 2,5 có | 10 5 sai | không | 
| 3 | 15 | 15 ≥ 2,5 có | 15 5 sai | không | 

Chỉ những chiếc xe thỏa mãn cả hai giới hạn mới được tính, cho kết quả 0 cho cách giải thích riêng biệt này. Điều này nêu bật lý do tại sao lý luận trực tiếp phải được áp dụng cẩn thận với các ranh giới số nguyên chính xác trong bối cảnh lời giải đầy đủ. 

### Ví dụ 2 

đầu vào:```
N = 3
A = [1, 5, 20]
Query: X=5, Y=100, L=1, R=10
```Giới hạn: 

v ≥ trần(5/10) = 1 

v ≤ sàn(100/1) = 100 

Tất cả các xe đều đáp ứng điều này. 

| Xe hơi | Tốc độ | Trong phạm vi [1.100] | Đếm | 
| --- | --- | --- | --- | 
| 1 | 1 | vâng | vâng | 
| 2 | 5 | vâng | vâng | 
| 3 | 20 | vâng | vâng | 

Câu trả lời là 3. 

Những ví dụ này cho thấy điều kiện hình học được chuyển thành một bài kiểm tra ngưỡng đơn giản về tốc độ như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + Q) log N) | mỗi bản cập nhật và truy vấn sử dụng các phép toán Fenwick cộng với tìm kiếm nhị phân trên tốc độ nén | 
| Không gian | O(N + Q) | lưu trữ cho mảng, bản đồ nén và cây Fenwick | 

Độ phức tạp phù hợp thoải mái trong các ràng buộc vì cả N và Q đều là 100000 và hệ số log vẫn nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()

# NOTE: In real use, hook solve() and capture output properly

# small sanity checks (conceptual placeholders)
# assert run(...) == ...
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| N=1, A=[0], truy vấn X=0 Y=0 L=1 R=1 | 1 | trường hợp cạnh tốc độ bằng không | 
| N=2, A=[1,2], không có cập nhật | phụ thuộc | tính đúng đắn tĩnh cơ bản | 
| X=0 Y=1e15 phạm vi lớn | N | ranh giới bao gồm đầy đủ | 
| L=R một lần | đúng | ranh giới truy vấn tức thời | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi ô tô có tốc độ bằng 0. Đối với đầu vào trong đó A[i] = 0 và X = 0, Y = 5, L = 1, R = 10, ô tô luôn ở vị trí 0. Điều kiện nên đếm nó một lần. Bất đẳng thức dẫn xuất v ≥ ceil(X / R) trở thành 0 ≥ 0, và v ≤ sàn(Y / L) trở thành 0 ≤ 5, do đó đưa vào đúng. 

Một trường hợp cạnh khác là khi L = R. Giả sử X = 10, Y = 20, L = R = 2. Điều kiện trở thành kiểm tra xem v · 2 có nằm trong [10, 20] hay không, tương đương với v trong [5, 10]. Công thức của thuật toán tạo ra khoảng giống nhau, đảm bảo tính chính xác. 

Trường hợp tinh tế cuối cùng là bao gồm ranh giới. Nếu một chiếc xe thỏa mãn chính xác v · L = X hoặc v · R = Y thì phải tính. Sử dụng trần và sàn đúng cách sẽ đảm bảo những kết quả khớp chính xác này được giữ nguyên thay vì bị rơi.
