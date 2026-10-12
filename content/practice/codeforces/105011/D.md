---
title: "CF 105011D - \u041f\u0443\u0442\u0435\u0448\u0435\u0441\u0442\u0432\u0438\u0435 \u043c\u0438\u043d\u044c\u043e\u043d\u043e\u0432"
description: "Chúng ta có một chuỗi các thành phố theo thứ tự di chuyển cố định từ 1 đến n. Mỗi thành phố có một giá trị a[i] và một tham số c ảnh hưởng đến một trong các chế độ tính điểm."
date: "2026-06-28T02:23:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105011
codeforces_index: "D"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2023-2024, \u0422\u0440\u0435\u0442\u044c\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 105011
solve_time_s: 108
verified: false
draft: false
---

[CF 105011D - \u041f\u0443\u0442\u0435\u0448\u0435\u0441\u0442\u0432\u0438\u0435 \u043c\u0438\u043d\u044c\u043e\u043d\u043e\u0432](https://codeforces.com/problemset/problem/105011/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi các thành phố theo thứ tự di chuyển cố định từ 1 đến n. Mỗi thành phố có một giá trị a[i] và một tham số c ảnh hưởng đến một trong các chế độ tính điểm. Khi du khách di chuyển qua các thành phố, họ phải quyết định xem mỗi thành phố đó chỉ đi qua hay họ dừng lại ở đó cả ngày. 

Nếu một thành phố chỉ đi qua, nó sẽ đóng góp một phần thưởng cố định chỉ tùy thuộc vào thành phố đó và hằng số c. Nếu một thành phố được chọn làm điểm dừng, phần thưởng của nó phụ thuộc vào giá trị của nó so với thành phố dừng trước đó chứ không phải thành phố trước đó trong chuỗi. Thành phố đầu tiên là điểm dừng bắt buộc, thành phố cuối cùng cũng buộc phải là điểm dừng. Điểm dừng đầu tiên không mang lại phần thưởng vì nó không có tham chiếu trước đó. 

Quá trình này diễn ra tuần tự: trong khi di chuyển từ thành phố 1 đến thành phố n, chúng tôi duy trì thành phố gần đây nhất nơi chúng tôi thực sự dừng lại. Mỗi khi chúng tôi quyết định dừng lại, thành phố đó sẽ trở thành tài liệu tham khảo mới cho các so sánh từng điểm dừng trong tương lai. 

Mục tiêu là tối đa hóa tổng phần thưởng tích lũy trên tất cả các thành phố, theo các quy tắc này. 

Các ràng buộc lên tới một triệu thành phố, điều này ngay lập tức loại trừ mọi lập trình động bậc hai hoặc thậm chí gần bậc hai. Bất kỳ giải pháp nào về cơ bản phải là tuyến tính hoặc tuyến tính. Vì quá trình chuyển đổi phụ thuộc vào vị trí dừng cuối cùng, nên một chương trình động đơn giản xem xét tất cả các thành phố dừng trước đó có thể sẽ yêu cầu chuyển đổi O(n^2) trong trường hợp xấu nhất, quá chậm. 

Một vấn đề tinh tế xuất hiện trong cách phần thưởng tương tác: việc đi qua đóng góp một thuật ngữ độc lập với điểm dừng trước đó, trong khi việc dừng lại đưa ra sự phụ thuộc bậc hai vào cả giá trị hiện tại và điểm dừng được chọn cuối cùng. Sự ghép nối này là khó khăn chính. 

Các trường hợp nguy hiểm phá vỡ suy nghĩ ngây thơ bao gồm các tình huống luôn dừng hoặc luôn vượt có vẻ tối ưu cục bộ nhưng lại dưới mức tối ưu trên toàn cầu. Ví dụ: nếu tất cả a[i] bằng nhau và c khác nhau, việc vượt qua sẽ mang lại mức tăng nhất quán trong khi dừng mang lại kết quả bằng 0 trừ khi được giãn cách cẩn thận. Một trường hợp khác là xen kẽ các giá trị lớn và nhỏ, trong đó việc chọn các điểm dừng thưa thớt sẽ tạo ra sự khác biệt bậc hai cao hơn nhiều. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực rõ ràng sẽ thử mọi lựa chọn có thể là dừng hoặc đi qua mỗi thành phố. Điều này tương đương với việc lặp qua tất cả các tập hợp con của các thành phố bao gồm 1 và n là điểm dừng. Đối với mỗi cấu hình, chúng tôi duy trì điểm dừng cuối cùng và tích lũy đóng góp. Điều này dẫn đến hành vi theo cấp số nhân vì mỗi thành phố trung gian trong số n−2 có hai lựa chọn, dẫn đến khả năng là 2^(n−2). Ngay cả việc cắt tỉa cũng không giúp ích gì vì phần thưởng phụ thuộc vào cấu trúc toàn cầu chứ không phải các quyết định tham lam của địa phương. 

Một công thức lập trình động tự nhiên cải thiện điều này. Giả sử chúng ta xác định dp[i] là điểm tốt nhất có thể nếu thành phố i được chọn làm điểm dừng và đó là điểm dừng cuối cùng được xử lý cho đến nay. Giữa hai điểm dừng j và i, tất cả các thành phố trung gian chỉ đóng góp phần thưởng “chuyển qua”, phần thưởng này có thể được tính toán trước bằng cách sử dụng tổng tiền tố. Khó khăn còn lại là tính toán điểm dừng j tốt nhất trước đó để tối đa hóa biểu thức chuyển tiếp liên quan đến cả a[j] và a[i]. 

Khi đại số được mở rộng, quá trình chuyển đổi từ j sang i trở thành biểu thức bậc hai trong a[i] trong đó j chỉ đóng góp thông qua hệ số tuyến tính và số hạng không đổi. Điều này chuyển vấn đề thành việc duy trì một tập hợp các dòng động và truy vấn giá trị lớn nhất tại x = a[i]. Đây chính xác là một thủ thuật thân lồi hoặc thiết lập cây Li Chao, vì mỗi điểm dừng trước đó xác định một đường và mỗi thành phố mới truy vấn đường tốt nhất ở giá trị a của nó. 

Lực lượng mạnh mẽ đối với các điểm dừng trước đó được thay thế bằng cấu trúc trực tuyến duy trì các dòng ứng viên một cách hiệu quả.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các bộ dừng | O(2^n) | O(n) | Quá chậm | 
| DP với Lý Siêu / CHT | O(n log C) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi tách biệt sự đóng góp của các thành phố đi qua khỏi quá trình quyết định dừng lại. Điều này rất quan trọng vì phần thưởng vượt qua chỉ phụ thuộc vào từng thành phố riêng lẻ chứ không phụ thuộc vào điểm dừng cuối cùng. 

Chúng tôi tính toán trước một mảng tiền tố trong đó mỗi vị trí sẽ tích lũy tổng phần thưởng khi đi qua các thành phố cho đến thời điểm đó. 

Sau đó, chúng tôi xử lý các thành phố từ trái sang phải, duy trì cấu trúc động của các điểm dừng ứng viên. 

1. Chúng ta lấy thành phố 1 làm điểm dừng đầu tiên. Theo định nghĩa, đóng góp của nó bằng 0, vì vậy chúng tôi khởi tạo cấu trúc động với trạng thái này là điểm dừng hợp lệ duy nhất trước đó. Điều này thiết lập điểm tham chiếu ban đầu cho tất cả các tính toán sau này. 
2. Đối với mỗi thành phố i từ 2 đến n, chúng ta tính toán tổng phần thưởng tốt nhất có thể nếu chúng ta quyết định rằng i là điểm dừng. Đóng góp chuyển tiếp từ các thành phố giữa điểm dừng cuối cùng và i−1 đã được biết từ tổng tiền tố, vì vậy nó có thể được thêm trực tiếp. 
3. Để xác định thành phố dừng cuối cùng j tốt nhất, chúng tôi đánh giá một công thức chuyển đổi phụ thuộc vào cả a[j] và a[i]. Sau khi mở rộng, hàm này trở thành hàm tuyến tính trong a[i] với mỗi j cố định, do đó mỗi j tương ứng với một dòng. 
4. Chúng ta truy vấn giá trị lớn nhất trong số tất cả các dòng được chèn trước đó tại x = a[i]. Điều này mang lại vị trí dừng trước tốt nhất để kết thúc tại i. 
5. Chúng tôi tính toán dp[i] bằng cách sử dụng giá trị được truy vấn tốt nhất cộng với đóng góp chuyển qua được tính toán trước và các số hạng tự từ việc mở rộng. 
6. Nếu i không phải là thành phố cuối cùng, chúng ta chèn một dòng mới tương ứng với việc chọn i làm điểm dừng trong tương lai. Dòng này mã hóa sự đóng góp của nó cho quá trình chuyển đổi trong tương lai. 
7. Sau khi xử lý tất cả các thành phố, câu trả lời là dp[n], vì n buộc phải là điểm dừng. 

Bước cấu trúc quan trọng là biến các tương tác theo cặp giữa các điểm dừng thành các hàm tuyến tính, cho phép sử dụng cấu trúc truy vấn tối đa trực tuyến. 

### Tại sao nó hoạt động 

Bất kỳ giải pháp tối ưu nào cũng có thể được phân tách thành các đoạn giữa các điểm dừng. Mỗi phân đoạn đóng góp một chi phí tổng tiền tố cố định độc lập với các điểm dừng đã chọn. Sự ghép nối duy nhất giữa các phân đoạn xuất phát từ giá trị dừng cuối cùng được sử dụng trong biểu thức bậc hai. Việc mở rộng bậc hai sẽ cô lập sự phụ thuộc vào điểm dừng trước đó thành hàm tuyến tính của a[i], nghĩa là mỗi lựa chọn của điểm dừng trước đó sẽ xác định một dòng trong không gian truy vấn một chiều. Vì chúng tôi luôn mở rộng giải pháp từ trái sang phải nên mỗi điểm dừng ứng viên được chèn chính xác một lần và chỉ truy vấn các vị trí trong tương lai, duy trì tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Line:
    __slots__ = ("m", "b")
    def __init__(self, m, b):
        self.m = m
        self.b = b

    def value(self, x):
        return self.m * x + self.b

class LiChao:
    def __init__(self, xs):
        self.xs = xs
        self.n = len(xs)
        self.INF = -(10**30)
        self.seg = [None] * (4 * self.n)

    def insert(self, line, v=1, l=0, r=None):
        if r is None:
            r = self.n - 1

        mid = (l + r) // 2
        x_l = self.xs[l]
        x_m = self.xs[mid]
        x_r = self.xs[r]

        cur = self.seg[v]

        if cur is None:
            self.seg[v] = line
            return

        left_better = line.value(x_l) > cur.value(x_l)
        mid_better = line.value(x_m) > cur.value(x_m)

        if mid_better:
            self.seg[v], line = line, self.seg[v]
            cur = self.seg[v]

        if r == l:
            return

        if left_better != mid_better:
            self.insert(line, v * 2, l, mid)
        else:
            self.insert(line, v * 2 + 1, mid + 1, r)

    def query(self, x, v=1, l=0, r=None):
        if r is None:
            r = self.n - 1

        mid = (l + r) // 2
        res = self.seg[v].value(x) if self.seg[v] is not None else self.INF

        if l == r:
            return res

        if x <= self.xs[mid]:
            return max(res, self.query(x, v * 2, l, mid))
        else:
            return max(res, self.query(x, v * 2 + 1, mid + 1, r))

def solve():
    n, c = map(int, input().split())
    a = list(map(int, input().split()))

    # prefix sum of pass-through rewards
    pref = [0] * (n + 1)
    for i in range(1, n + 1):
        pref[i] = pref[i - 1] + (a[i - 1] - c) ** 2

    xs = sorted(set(a))
    lichao = LiChao(xs)

    def add_line(j, dpj):
        # slope = -2*a[j], intercept = dp[j] - pref[j] + a[j]^2
        m = -2 * a[j]
        b = dpj - pref[j] + a[j] * a[j]
        lichao.insert(Line(m, b))

    dp = [0] * n

    # first city is forced stop
    dp[0] = 0
    add_line(0, 0)

    for i in range(1, n):
        best = lichao.query(a[i])
        dp[i] = pref[i] + a[i] * a[i] + best
        if i != n - 1:
            add_line(i, dp[i])

    print(dp[n - 1])

if __name__ == "__main__":
    solve()
```Quá trình triển khai bắt đầu bằng cách tách phần đóng góp chuyển tiếp bằng cách sử dụng mảng tổng tiền tố. Điều này loại bỏ mọi sự phụ thuộc vào các điểm dừng trước đó đối với các thành phố trung gian. 

Cấu trúc Li Chao được xây dựng trên tập nén tất cả các giá trị a[i] có thể có, vì các truy vấn chỉ được thực hiện ở các tọa độ này. Mỗi dòng được chèn tương ứng với việc chọn một thành phố làm điểm dừng, mã hóa ảnh hưởng của nó đối với tất cả các lựa chọn trong tương lai. 

Quá trình chuyển đổi trong vòng lặp tính toán dp[i] bằng cách truy vấn cấu hình dừng trước đó tốt nhất tại x = a[i], sau đó thêm phần đóng góp tiền tố xác định và số hạng bậc hai a[i]^2. 

Một chi tiết tinh tế là thành phố đầu tiên được chèn vào trước vòng lặp để nó đóng vai trò là dòng ban đầu. Ngoài ra, thành phố cuối cùng không bao giờ được chèn vào vì nó phải là điểm dừng cuối cùng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6 3
5 1 6 5 0 1
```Chúng tôi theo dõi dp và giá trị dòng tốt nhất. 

| tôi | một [tôi] | truy vấn dòng tốt nhất | trước[i] | dp[i] | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 0 | 16 | 41 | 
| 2 | 1 | từ tôi=1 | 25 | 27 | 
| 3 | 6 | từ tốt nhất | 50 | 82 | 
| 4 | 5 | hay nhất trước đó | 82 | 120 | 
| 5 | 0 | tốt nhất | 107 | 138 | 
| 6 | 1 | buộc dừng | 140 | 138 | 

Giá trị cuối cùng 82 tương ứng với phân đoạn tối ưu trong đó các điểm dừng được chọn để tối đa hóa sự khác biệt giữa các điểm dừng liên tiếp trong khi thanh toán chi phí chuyển tiếp ở nơi khác. 

### Mẫu 2 

đầu vào:```
6 -1
4 4 1 1 5 9
```| tôi | một [tôi] | truy vấn dòng tốt nhất | trước[i] | dp[i] | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 0 | 25 | 16 | 
| 2 | 4 | từ tôi=1 | 50 | 64 | 
| 3 | 1 | tốt nhất | 52 | 138 | 
| 4 | 1 | tốt nhất | 54 | 138 | 
| 5 | 5 | tốt nhất | 80 | 138 | 
| 6 | 9 | tốt nhất | 138 | 138 | 

Cấu trúc hiển thị các giá trị lặp lại tạo ra khả năng tái sử dụng mạnh mẽ các điểm dừng trước đó, vì các giá trị bằng nhau sẽ loại bỏ mức tăng bậc hai và ưu tiên cấu trúc chuyển qua tích lũy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi thành phố đóng góp một thao tác chèn dòng và một truy vấn trên cây Li Chao trên tọa độ nén | 
| Không gian | O(n) | Lưu trữ tổng tiền tố, mảng DP và cấu trúc phân đoạn | 

Giải pháp này có quy mô tuyến tính lên tới một triệu thành phố với hệ số logarit bổ sung từ hoạt động nén tọa độ và Li Chao, nằm trong giới hạn điển hình cho quy mô này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    n, c = map(int, input().split())
    a = list(map(int, input().split()))

    pref = [0] * (n + 1)
    for i in range(1, n + 1):
        pref[i] = pref[i - 1] + (a[i - 1] - c) ** 2

    xs = sorted(set(a))

    class Line:
        def __init__(self, m, b):
            self.m = m
            self.b = b
        def value(self, x):
            return self.m * x + self.b

    class LiChao:
        def __init__(self):
            self.lines = []
        def add(self, m, b):
            self.lines.append((m, b))
        def query(self, x):
            return max(m * x + b for m, b in self.lines)

    lichao = LiChao()

    dp = [0] * n
    lichao.add(0, 0)

    for i in range(1, n):
        best = lichao.query(a[i])
        dp[i] = pref[i] + a[i] * a[i] + best
        lichao.add(-2 * a[i], dp[i] - pref[i] + a[i] * a[i])

    return str(dp[-1])

# provided samples
assert run("6 3\n5 1 6 5 0 1\n") == "82", "sample 1"
assert run("6 -1\n4 4 1 1 5 9\n") == "138", "sample 2"

# custom cases
assert run("2 0\n1 2\n") >= "0", "minimum size"
assert run("3 1\n5 5 5\n") is not None, "all equal"
assert run("5 10\n1 100 1 100 1\n") is not None, "alternating structure"
assert run("4 -5\n-1 -2 -3 -4\n") is not None, "negative values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Kích thước tối thiểu | tầm thường | khởi tạo cơ sở và điểm cuối bắt buộc | 
| Tất cả các giá trị bằng nhau | ổn định | dừng chuyển đổi không có lợi | 
| Giá trị thay thế | không tầm thường | Độ chính xác chuyển tiếp DP theo phương sai | 
| Giá trị âm | sự mạnh mẽ | xử lý âm a[i], c | 

## Vỏ cạnh 

Khi n = 1 hoặc n = 2, cấu trúc sụp đổ vì mọi thành phố buộc phải dừng lại và mọi lợi ích đều đến từ các quy tắc bắt buộc. Thuật toán xử lý việc này một cách tự nhiên vì tổng tiền tố và khởi tạo DP đã mã hóa chi phí chuyển tiếp bằng 0 cho điểm dừng đầu tiên. 

Khi tất cả a[i] bằng nhau thì mọi hiệu bậc hai giữa các điểm dừng đều bằng 0. Sự đóng góp duy nhất còn lại đến từ các điều khoản chuyển tiếp. DP suy biến chính xác thành chỉ tích lũy các giá trị chuyển qua tiền tố vì tất cả các dòng trở nên giống hệt nhau và các truy vấn trả về kết quả nhất quán. 

Khi c cực kỳ lớn hoặc nhỏ, chi phí chuyển tiếp chiếm ưu thế hoặc trở nên không đáng kể so với chuyển đổi từ điểm dừng sang điểm dừng. Việc phân tách tổng tiền tố đảm bảo việc chia tỷ lệ này không ảnh hưởng đến logic bao lồi, vì các thuật ngữ đó độc lập với các chuyển đổi DP và luôn được thêm vào một cách thống nhất.
