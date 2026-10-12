---
title: "CF 105023D - Thuật toán sân bay"
description: "Mỗi máy bay chiếm một cổng trong khoảng thời gian liên tục từ lúc đến đến lúc khởi hành. Mặc dù nó có mặt ở sân bay nhưng nó chặn một cổng, do đó, các khoảng thời gian chồng chéo tương ứng với việc sử dụng cổng đồng thời."
date: "2026-06-28T01:44:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105023
codeforces_index: "D"
codeforces_contest_name: "HPI 2024 Novice"
rating: 0
weight: 105023
solve_time_s: 81
verified: false
draft: false
---

[CF 105023D - Thuật toán sân bay](https://codeforces.com/problemset/problem/105023/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi máy bay chiếm một cổng trong khoảng thời gian liên tục từ lúc đến đến lúc khởi hành. Mặc dù nó có mặt ở sân bay nhưng nó chặn một cổng, do đó, các khoảng thời gian chồng chéo tương ứng với việc sử dụng cổng đồng thời. 

Đối với một khoảng truy vấn nhất định$[l, r]$, chúng ta được yêu cầu chỉ xem xét những mặt phẳng có liên quan trong cửa sổ thời gian này. Một máy bay góp phần gây ra tắc nghẽn trong quá trình truy vấn nếu sự hiện diện của nó tại sân bay chồng lên khoảng thời gian truy vấn theo cách nó “hoạt động” tại một thời điểm nào đó bên trong nó, với một quy tắc đặc biệt ở ranh giới: các máy bay khởi hành chính xác vào lúc$l$hoặc$r$bị loại trừ, trong khi máy bay đến đúng lúc$l$hoặc$r$được bao gồm. 

Nhiệm vụ của mỗi truy vấn là xác định số lượng mặt phẳng tối đa có mặt đồng thời tại bất kỳ thời điểm nào trong khoảng$[l, r]$, vì mức tối đa đó bằng số lượng cổng tối thiểu cần thiết để tránh xung đột. 

Ràng buộc$N \le 10^5$ngay lập tức loại trừ bất kỳ giải pháp nào tính toán lại các phần chồng chéo trên mỗi truy vấn bằng cách quét tất cả các mặt phẳng. Thậm chí$Q \le 50$không lưu phương trình bậc hai hoặc$N \log N$theo cách tiếp cận truy vấn, vì$50 \cdot 10^5$vẫn chỉ cho phép tuyến tính hoặc gần tuyến tính trên mỗi logic truy vấn. 

Trường hợp cạnh tinh tế xuất phát từ quy tắc ranh giới. Nếu một chiếc máy bay khởi hành đúng lúc$l$, nó không được tính, mặc dù việc kiểm tra chồng chéo ngây thơ như$a_i \le r$Và$d_i \ge l$sẽ bao gồm nó không chính xác. 

Ví dụ, nếu một mặt phẳng có khoảng$[5, 10]$và một truy vấn là$[10, 12]$, một cuộc kiểm tra giao lộ ngây thơ sẽ coi nó là hoạt động lúc 10 giờ, nhưng tuyên bố không bao gồm các chuyến khởi hành ở ranh giới, vì vậy nó sẽ không đóng góp gì cả. 

Một trường hợp đặc biệt khác là truy vấn bắt đầu hoặc kết thúc chính xác ở một số lượng lớn lượt truy cập. Số người đến ranh giới được tính, vì vậy các khoảng thời gian bắt đầu chính xác tại$l$vẫn phải được đưa vào tập hoạt động. 

## Phương pháp tiếp cận 

Một cách trực tiếp để trả lời mỗi truy vấn là mô phỏng thời gian. Đối với mỗi truy vấn, chúng ta có thể lặp lại trên tất cả các mặt phẳng và kiểm tra xem khoảng thời gian của chúng có trùng lặp với khoảng thời gian truy vấn theo quy tắc biên hay không, sau đó tính toán số lần trùng lặp tối đa trong tất cả các thời điểm liên quan. 

Điều này đúng nhưng đắt tiền. Đối với mỗi truy vấn, chúng tôi có thể sẽ kiểm tra$10^5$khoảng thời gian và sau đó xác định sự chồng chéo theo thời gian, dẫn đến khoảng$O(NQ)$chỉ để lọc, và tệ hơn nếu chúng ta cố gắng quét thời gian liên tục. Chi phí ẩn là việc phát hiện sự chồng chéo tối đa đòi hỏi phải suy luận về thứ tự sự kiện hoặc quét theo thời gian, điều này khiến cho việc mô phỏng mỗi truy vấn đơn giản trở nên quá chậm. 

Quan sát chính là mỗi truy vấn chỉ quan tâm đến số lượng khoảng thời gian hoạt động tối đa bên trong một cửa sổ cố định. Đây là một phép biến đổi cổ điển: thay vì suy nghĩ theo thời gian liên tục, chúng ta chuyển đổi mỗi mặt phẳng thành hai sự kiện, một sự kiện đến và một sự kiện rời đi, sau đó rút gọn vấn đề thành việc đếm tiền tố theo thời gian sự kiện được sắp xếp. 

Sau khi các sự kiện được sắp xếp, chúng ta có thể xây dựng một mảng tổng tiền tố biểu thị số lượng mặt phẳng đang hoạt động tại mỗi ranh giới sự kiện. Sau đó, truy vấn giảm xuống còn tìm giá trị tổng tiền tố tối đa trong một phạm vi giới hạn của chỉ số sự kiện. Đó là truy vấn phạm vi tối đa trên một mảng tĩnh, có thể được trả lời bằng bảng hoặc cây phân đoạn thưa thớt. 

Bởi vì$Q \le 50$, chúng tôi có thể đủ khả năng xử lý trước một lần trên toàn cầu, sau đó trả lời từng truy vấn bằng cách ánh xạ giới hạn thời gian của nó vào các chỉ mục trong danh sách sự kiện được sắp xếp và thực hiện truy vấn tối đa trong phạm vi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(NQ)$ĐẾN$O(N^2)$tùy theo phương pháp |$O(1)$ĐẾN$O(N)$| Quá chậm | 
| Quét + RMQ |$O(N \log N + Q \log N)$|$O(N \log N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta biến mỗi khoảng thành hai sự kiện, nhưng chúng ta phải tôn trọng quy tắc biên một cách cẩn thận. 

1. Trình bày mỗi máy bay dưới dạng sự kiện +1 tại thời điểm đến$a_i$và sự kiện -1 vào thời điểm khởi hành$d_i$. Chúng tôi coi các chuyến đến là bao gồm và các chuyến khởi hành là độc quyền bằng cách xử lý việc đặt hàng một cách chính xác trong quá trình sắp xếp. Điều này đảm bảo việc đếm chính xác tại các thời điểm ranh giới chính xác. 
2. Sắp xếp tất cả các sự kiện theo thời gian và nếu hai sự kiện có cùng thời gian, hãy xử lý các sự kiện đến trước các sự kiện khởi hành. Thứ tự này mã hóa quy tắc rằng những người đến ranh giới được tính trong khi những người khởi hành thì không. 
3. Quét qua các sự kiện đã sắp xếp và tính toán một mảng tổng tiền tố trong đó mỗi vị trí lưu trữ số lượng mặt phẳng hoạt động sau khi xử lý sự kiện đó. Điều này mang lại một hàm không đổi từng phần theo thời gian. 
4. Đối với mỗi thời gian sự kiện riêng biệt, hãy lưu trữ giá trị tổng tiền tố tương ứng. Điều này nén dòng thời gian vào tối đa$2N$điểm. 
5. Xây dựng cấu trúc phạm vi tối đa trên mảng tổng tiền tố này để chúng ta có thể truy vấn số lượng mặt phẳng hoạt động tối đa trên bất kỳ phân đoạn liền kề nào một cách hiệu quả. 
6. Đối với mỗi truy vấn$[l, r]$, tìm chỉ mục sự kiện đầu tiên có thời gian là$\ge l$và chỉ mục sự kiện cuối cùng có thời gian là$\le r$. Chúng xác định phân đoạn của dòng thời gian giao với cửa sổ truy vấn. 
7. Sử dụng cấu trúc phạm vi tối đa để tính tổng tiền tố tối đa trên phân đoạn này, đưa ra câu trả lời cho truy vấn. 

Tại sao nó hoạt động phụ thuộc vào cấu trúc của đường quét. Giữa các thời điểm sự kiện liên tiếp, số lượng mặt phẳng hoạt động không thay đổi, do đó, mức tối đa trong bất kỳ khoảng truy vấn nào phải xảy ra tại một số ranh giới sự kiện bên trong khoảng thời gian đó. Tổng tiền tố sau khi sắp xếp các sự kiện thể hiện chính xác số lượng mặt phẳng hoạt động tại mỗi ranh giới như vậy, do đó, việc lấy mức tối đa trong phạm vi giới hạn sẽ nắm bắt chính xác mức tắc nghẽn cao nhất bên trong$[l, r]$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SparseTable:
    def __init__(self, arr):
        n = len(arr)
        self.n = n
        self.log = [0] * (n + 1)
        for i in range(2, n + 1):
            self.log[i] = self.log[i // 2] + 1

        k = self.log[n] + 1
        self.st = [[0] * n for _ in range(k)]
        self.st[0] = arr[:]

        j = 1
        while (1 << j) <= n:
            i = 0
            while i + (1 << j) <= n:
                self.st[j][i] = max(self.st[j - 1][i],
                                     self.st[j - 1][i + (1 << (j - 1))])
                i += 1
            j += 1

    def query(self, l, r):
        j = self.log[r - l + 1]
        return max(self.st[j][l], self.st[j][r - (1 << j) + 1])

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    d = list(map(int, input().split()))

    events = []
    for i in range(n):
        events.append((a[i], 1))
        events.append((d[i], -1))

    events.sort(key=lambda x: (x[0], -x[1]))

    times = []
    active = []
    cur = 0

    for t, v in events:
        cur += v
        times.append(t)
        active.append(cur)

    st = SparseTable(active)

    def lower_bound(x):
        lo, hi = 0, len(times)
        while lo < hi:
            mid = (lo + hi) // 2
            if times[mid] >= x:
                hi = mid
            else:
                lo = mid + 1
        return lo

    def upper_bound(x):
        lo, hi = 0, len(times)
        while lo < hi:
            mid = (lo + hi) // 2
            if times[mid] > x:
                hi = mid
            else:
                lo = mid + 1
        return lo - 1

    out = []
    for _ in range(q):
        l, r = map(int, input().split())
        L = lower_bound(l)
        R = upper_bound(r)
        if L > R:
            out.append("0")
        else:
            out.append(str(st.query(L, R)))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Bước sắp xếp sự kiện là nơi thực thi tính chính xác của ranh giới. Sắp xếp theo thời gian và sắp xếp các chuyến đến trước khi khởi hành đảm bảo rằng vào đúng thời điểm$t$, các lượt đến sẽ được bao gồm trước khi xóa vào cùng một dấu thời gian. 

Việc tích lũy tiền tố xây dựng số lượng mặt phẳng hoạt động chính xác sau mỗi sự kiện, do đó mỗi chỉ mục tương ứng với trạng thái ổn định. Sau đó, bảng thưa thớt cho phép truy vấn tối đa theo thời gian không đổi trên bất kỳ phân đoạn nào. 

Tìm kiếm nhị phân tách biệt chính xác phần dòng thời gian sự kiện giao với khoảng thời gian truy vấn, đảm bảo chúng tôi chỉ xem xét các trạng thái xảy ra trong$[l, r]$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
N = 5, Q = 2
a = [2, 7, 4, 5, 9]
d = [8, 11, 5, 8, 16]
queries:
[1, 10]
[8, 14]
```Sự kiện sau khi sắp xếp:```
(2,+1), (4,+1), (5,+1), (7,+1), (8,-1), (8,-1), (9,+1), (11,-1), (16,-1)
```Số lượng hoạt động tiền tố: 

| Chỉ mục sự kiện | Thời gian | Thay đổi | Đang hoạt động | 
| --- | --- | --- | --- | 
| 0 | 2 | +1 | 1 | 
| 1 | 4 | +1 | 2 | 
| 2 | 5 | +1 | 3 | 
| 3 | 7 | +1 | 4 | 
| 4 | 8 | -1 | 3 | 
| 5 | 8 | -1 | 2 | 
| 6 | 9 | +1 | 3 | 
| 7 | 11 | -1 | 2 | 
| 8 | 16 | -1 | 1 | 

Truy vấn$[1, 10]$chọn các chỉ số bao gồm thời gian từ 2 đến 9, cho hoạt động tối đa = 4. 

Truy vấn$[8, 14]$chọn các chỉ số trong khoảng từ 8 đến 11, cho hoạt động tối đa = 3. 

Điều này xác nhận rằng thuật toán nắm bắt chính xác sự chồng chéo đỉnh bên trong các cửa sổ bị hạn chế thay vì trên toàn bộ. 

### Ví dụ 2 

đầu vào:```
N = 3, Q = 1
a = [1, 3, 5]
d = [4, 6, 8]
query: [4, 4]
```Tại thời điểm 4, máy bay thứ hai vẫn hoạt động vì nó khởi hành lúc 6 giờ, trong khi máy bay thứ nhất khởi hành đúng lúc 4 giờ và bị loại trừ theo quy tắc ranh giới. 

Mặt phẳng hoạt động tại thời điểm truy vấn = 1. 

Việc trình bày sự kiện đảm bảo những người đến lúc 4 giờ được bao gồm trước khi khởi hành lúc 4 giờ, do đó trạng thái tiền tố phản ánh chính xác một khoảng thời gian hoạt động duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N + Q \log N)$| sắp xếp các sự kiện và xây dựng RMQ, cộng với các tìm kiếm nhị phân cho mỗi truy vấn | 
| Không gian |$O(N \log N)$| bàn thưa thớt nhiều nhất$2N$trạng thái sự kiện | 

Các ràng buộc cho phép lên đến$10^5$mặt phẳng, do đó quá trình tiền xử lý trong$N \log N$dễ dàng đủ nhanh và mỗi truy vấn thực sự có thời gian không đổi hoặc logarit. Chỉ với 50 truy vấn, giải pháp có thể thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return __import__("__main__").solve() if False else ""

# Since solve prints directly, we redefine a wrapper version for testing
def run(inp: str) -> str:
    import sys
    from io import StringIO
    backup = sys.stdin
    sys.stdin = StringIO(inp)

    from contextlib import redirect_stdout
    out = StringIO()
    with redirect_stdout(out):
        solve()

    sys.stdin = backup
    return out.getvalue().strip()

# sample
assert run("""5 2
2 7 4 5 9
8 11 5 8 16
1 10
8 14
""") == "4\n3"

# minimum size
assert run("""1 1
5
10
5 5
""") == "1"

# all disjoint
assert run("""3 1
1 10 20
2 11 21
5 15
""") == "1"

# full overlap
assert run("""3 1
1 2 3
10 10 10
1 10
""") == "3"

# boundary exclusion check
assert run("""2 1
1 10
5 10
10 10
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| máy bay đơn | 1 | trường hợp tối thiểu | 
| khoảng rời rạc | 1 | không có logic chồng chéo | 
| chồng chéo hoàn toàn | 3 | xếp chồng đỉnh | 
| trường hợp ranh giới | 1 | loại trừ khởi hành tại ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một mặt phẳng khởi hành chính xác tại ranh giới truy vấn. Đối với đầu vào:```
a = [1], d = [5], query = [5, 5]
```đầu ra đúng là 1 vì mặt phẳng vẫn hiện diện tối đa nhưng không bao gồm 5 tùy theo cách giải thích; với quy tắc đã cho, các chuyến khởi hành lúc 5 giờ bị loại trừ, do đó máy bay không đóng góp vào lúc 5. Thứ tự đường quét đảm bảo sự kiện -1 tại thời điểm 5 được áp dụng sau khi xem xét các chuyến đến, ngăn chặn việc đưa vào không chính xác. 

Một trường hợp khác xảy ra khi nhiều sự kiện có cùng dấu thời gian. Ví dụ:```
a = [5, 5], d = [10, 10], query = [5, 5]
```Cả hai máy bay đều đến điểm 5, vì vậy cả hai đều phải được tính, mang lại kết quả là 2. Việc sắp xếp các điểm đến trước khi khởi hành đảm bảo tiền tố đạt 2 trước bất kỳ phép trừ nào tại thời điểm đó.
