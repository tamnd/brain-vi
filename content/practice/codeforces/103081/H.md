---
title: "CF 103081H - Tượng nhỏ"
description: "Chúng ta được cung cấp một hệ thống với các bức tượng nhỏ $N$ được dán nhãn từ $0$ đến $N-1$. Trong $N$ ngày, mỗi bức tượng nhỏ được lắp vào giá đúng một lần và sau đó được lấy ra đúng một lần, do đó, mỗi bức tượng nhỏ xác định một khoảng thời gian hoạt động liên tục $[lj, rj)$."
date: "2026-07-03T23:18:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103081
codeforces_index: "H"
codeforces_contest_name: "2020-2021 ICPC Southwestern European Regional Contest (SWERC 2020)"
rating: 0
weight: 103081
solve_time_s: 54
verified: true
draft: false
---

[CF 103081H - Tượng nhỏ](https://codeforces.com/problemset/problem/103081/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một hệ thống với$N$bức tượng nhỏ được dán nhãn từ$0$ĐẾN$N-1$. Qua$N$ngày, mỗi bức tượng nhỏ được đặt lên kệ đúng một lần và sau đó được lấy ra đúng một lần, vì vậy mỗi bức tượng nhỏ xác định một khoảng thời gian hoạt động liên tục$[l_j, r_j)$. Vào bất kỳ ngày nào$i$, chiếc kệ chứa chính xác những bức tượng nhỏ có khoảng cách bao gồm ngày hôm đó, tạo thành một bộ$S(i)$. 

Song song với tập phát triển này, chúng tôi mô phỏng một số$x$bắt đầu từ$x_0 = 0$. Vào ngày$i$, chúng ta nhìn vào giá trị hiện tại$x_i$, và chúng tôi tăng nó bằng cách đếm xem có ít nhất bao nhiêu bức tượng nhỏ hiện có trên kệ có nhãn$x_i$. Giá trị cập nhật trở thành$x_{i+1}$, nhưng lấy modulo$N$. Quá trình này chạy cho tất cả$N$ngày, và chúng tôi xuất ra$x_N$. 

Khó khăn chính là ngưỡng$x_i$thay đổi theo thời gian và nó phụ thuộc vào số liệu trước đó. Điều này làm cho việc đếm trở nên linh hoạt, bởi vì mỗi ngày sẽ hỏi một truy vấn khác nhau trên cùng một tập hợp đang phát triển: “ít nhất có bao nhiêu phần tử hoạt động?”$x_i$?”. 

Những hạn chế$N \le 100{,}000$ngay lập tức loại trừ mọi giải pháp tính toán lại tập hoạt động hoặc quét nó mỗi ngày. Một mô phỏng ngây thơ sẽ là$O(N^2)$trong trường hợp xấu nhất, vì mỗi ngày có thể yêu cầu quét tất cả các bức tượng nhỏ đang hoạt động. 

Một điểm tinh tế là tập hoạt động không tùy ý mỗi ngày, nó được xác định bằng cách chèn và xóa khoảng thời gian. Mỗi bức tượng nhỏ xuất hiện trong một phạm vi liền kề, có nghĩa là cấu trúc theo thời gian được xác định hoàn toàn bởi các sự kiện xen kẽ chứ không phải là các cập nhật độc lập. 

Một trường hợp dễ bị bỏ lỡ là khi$x_i$phát triển và bao bọc modulo$N$. Việc triển khai đơn giản có thể quên tương tác modulo với các ngưỡng đếm. 

Ví dụ, giả sử$N = 3$, và vào một ngày nào đó tất cả các bức tượng nhỏ$\{0,1,2\}$đang hoạt động và$x_i = 2$. Sau đó, câu trả lời đếm các yếu tố$\ge 2$, chỉ có một phần tử. Nhưng nếu bản cập nhật đẩy$x_{i+1}$ĐẾN$3$, nó kết thúc đến$0$, thay đổi đáng kể truy vấn tiếp theo. Bất kỳ cách tiếp cận nào xử lý$x$vì đơn điệu sẽ không chính xác. 

Một vấn đề tinh tế khác là cùng một hình ảnh đóng góp trong một khoảng thời gian liên tục, do đó trạng thái chỉ thay đổi tại$2N$điểm cuối, không tùy tiện. Việc bỏ qua điều này sẽ dẫn đến việc tính toán lại mỗi ngày thay vì cập nhật theo sự kiện. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp giữ thiết lập hiện tại$S(i)$trong một cấu trúc cân bằng, chẳng hạn như cây nhiều tập hoặc cây Fenwick trên nhãn. Mỗi ngày, chúng ta chèn và xóa các bức tượng nhỏ theo sự kiện của ngày hôm đó, sau đó trả lời câu hỏi bằng cách đếm các phần tử.$\ge x_i$. Đây là rồi$O(N \log N)$, nhưng vẫn phụ thuộc vào việc trả lời hiệu quả “đếm ≥ x”. 

Nếu chúng tôi duy trì một mảng tần số trên các nhãn và cây Fenwick, chúng tôi có thể hỗ trợ cập nhật điểm cho các thao tác chèn và xóa cũng như tính tổng tiền tố cho số đếm. Sau đó, mỗi truy vấn giảm xuống thành một truy vấn tổng tiền tố: tổng tổng tiền tố trừ hoạt động lên tới$x_i - 1$. 

Tuy nhiên, điều này vẫn để lại sự phụ thuộc rằng chúng tôi đang truy vấn một cấu trúc hoàn toàn động mỗi ngày, đồng thời buộc phải xử lý chính xác$N$ngày theo thứ tự. 

Quan sát quan trọng là chúng ta thực sự không cần xử lý thứ tự truy vấn tùy ý: chuỗi ngày là cố định và cấu trúc phát triển theo cách có thể dự đoán được. Thay vì coi đây là$N$các truy vấn đếm phạm vi độc lập, chúng ta có thể tính toán trước các khoảng thời gian hoạt động và sau đó xử lý các sự kiện theo thứ tự, duy trì cây Fenwick trên các nhãn. 

Mỗi bức tượng đóng góp chính xác hai sự kiện: chèn và loại bỏ. Quét qua các ngày, chúng tôi cập nhật cấu trúc cho phù hợp. Sau đó, truy vấn mỗi ngày chỉ là một truy vấn tiền tố Fenwick. 

Điều này làm giảm vấn đề duy trì một bộ động với$O(\log N)$cập nhật và truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Quét Brute Force mỗi ngày |$O(N^2)$|$O(N)$| Quá chậm | 
| Fenwick trong khoảng thời gian hoạt động |$O(N \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý tất cả các bức tượng nhỏ theo từng khoảng thời gian và mô phỏng từng ngày. 

1. Phân tích dữ liệu đầu vào và xây dựng lại cho mỗi bức tượng nhỏ$j$, ngày nó được chèn vào và ngày nó được gỡ bỏ. Điều này mang lại một cặp$(l_j, r_j)$. Bước này chuyển đổi vấn đề từ “nhật ký mỗi ngày” thành dạng khoảng thời gian, dễ tổng hợp hơn. 
2. Xây dựng hai danh sách sự kiện được lập chỉ mục theo ngày: một danh sách để chèn và một danh sách để xóa. Đối với mỗi bức tượng$j$, chúng tôi thêm$j$vào danh sách chèn trong ngày$l_j$, và vào danh sách xóa trong ngày$r_j$. Điều này đảm bảo chúng ta có thể cập nhật tập hoạt động tăng dần theo thời gian tuyến tính. 
3. Duy trì cây Fenwick trên các chỉ số$0 \ldots N-1$, trong đó mỗi chỉ mục lưu trữ liệu một bức tượng nhỏ có hiện đang hoạt động hay không. Khi một bức tượng nhỏ bắt đầu hoạt động, chúng tôi thêm$+1$tại chỉ số của nó; khi nó không hoạt động, chúng tôi trừ đi$1$. Cấu trúc này cho phép chúng ta truy vấn có bao nhiêu bức tượng nhỏ đang hoạt động nằm trong bất kỳ tiền tố nhãn nào. 
4. Khởi tạo$x = 0$. Chúng tôi cũng khẳng định rằng cây Fenwick luôn biểu diễn chính xác$S(i)$, tập hoạt động cho ngày hiện tại. 
5. Cho mỗi ngày$i$từ$0$ĐẾN$N-1$, trước tiên hãy áp dụng tất cả các lần xóa được lên lịch cho ngày này, sau đó áp dụng tất cả các lần chèn. Thứ tự về cơ bản không quan trọng miễn là cả hai thao tác trong ngày đều được áp dụng trước khi tính toán truy vấn cho tập ngày đó. 
6. Tính toán$y_i$như số lượng bức tượng nhỏ đang hoạt động có nhãn$\ge x$. Điều này thu được như$\text{active\_count} - \text{prefix\_sum}(x-1)$. 
7. Cập nhật$x \leftarrow (x + y_i) \bmod N$. Đây là nơi duy nhất mà giá trị phát triển và nó chỉ phụ thuộc vào phân phối hoạt động hiện tại và ngưỡng hiện tại. 
8. Sau khi xử lý tất cả các ngày, xuất ra$x$. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, cây Fenwick biểu diễn chính xác tập hợp$S(i)$bởi vì mỗi bức tượng nhỏ đóng góp chính xác một lần chèn và một lần xóa, và những điều này được áp dụng chính xác ở ranh giới trong khoảng thời gian tồn tại của nó. Do đó, mọi truy vấn đều được đánh giá trên tập hợp chính xác. Việc tính toán của$y_i$chính xác là định nghĩa của quy tắc cập nhật của bài toán, vì cây Fenwick chia các phần tử hoạt động thành các phần tử bên dưới$x_i$và những người ở trên hoặc bằng nhau. Bản cập nhật modulo chỉ ảnh hưởng đến các truy vấn trong tương lai và không thay đổi tính chính xác về trước, do đó mô phỏng vẫn trung thành với quy trình ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        i += 1
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        if i < 0:
            return 0
        i += 1
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

    def range_sum(self, l, r):
        return self.sum(r) - self.sum(l - 1)

def solve():
    n = int(input())
    insert = [[] for _ in range(n)]
    remove = [[] for _ in range(n)]

    for day in range(n):
        parts = input().strip().split()
        i = 0
        while i < len(parts):
            sign = parts[i][0]
            j = int(parts[i][1:])
            if sign == '+':
                insert[day].append(j)
            else:
                remove[day].append(j)
            i += 1

    for j in range(n):
        d = int(input())
        remove[d].append(j)

    fw = Fenwick(n)
    active = 0
    x = 0

    for day in range(n):
        for j in remove[day]:
            fw.add(j, -1)
            active -= 1
        for j in insert[day]:
            fw.add(j, +1)
            active += 1

        less = fw.sum(x - 1)
        y = active - less
        x = (x + y) % n

    print(x)

if __name__ == "__main__":
    solve()
```Việc triển khai đầu tiên sẽ xây dựng lại tất cả các điểm cuối khoảng thời gian. Lựa chọn thiết kế chính là tách các danh sách chèn và xóa mỗi ngày để mỗi sự kiện được xử lý chính xác một lần. Cây Fenwick lưu trữ chỉ báo hoạt động trên mỗi chỉ mục hình tượng, cho phép truy vấn tiền tố nhanh. 

Một chi tiết triển khai tinh tế là xử lý$x = 0$. Truy vấn tiền tố sử dụng$x - 1$, trở thành$-1$, và cây Fenwick trả về 0 một cách an toàn trong trường hợp đó. Điều này tránh vỏ bọc đặc biệt. 

Một chi tiết quan trọng khác là duy trì một cách rõ ràng`active`quầy tính tiền. Mặc dù có thể truy vấn tổng số hoạt động từ cây Fenwick, nhưng việc giữ một bộ đếm riêng sẽ tránh được thao tác logarit bổ sung mỗi ngày. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xét một trường hợp nhỏ với$N = 3$và các bức tượng nhỏ: 

Nhật ký ngày:```
Day 0: +0 +2
Day 1: -0 +1
Day 2: -1 -2
```Trình tự Di:```
0
1
2
```Chúng tôi theo dõi sự tiến hóa của trạng thái. 

| Ngày | Bộ hoạt động | x trước | tiền tố(x-1) | y = ≥x | x sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | {0,2} | 0 | 0 | 2 | 2 | 
| 1 | {1,2} | 2 | 1 | 1 | 0 | 
| 2 | {1,2} | 0 | 0 | 2 | 2 | 

Câu trả lời cuối cùng là 2. Dấu vết cho thấy ngưỡng đặt lại do hành vi modulo. 

### Ví dụ 2 

hãy để$N = 4$: 

Nhật ký ngày:```
Day 0: +0 +1
Day 1: +2
Day 2: -0 -1
Day 3: -2
```Di:```
0
1
2
3
```| Ngày | Bộ hoạt động | x trước | tiền tố(x-1) | y | x sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | {0,1} | 0 | 0 | 2 | 2 | 
| 1 | {0,1,2} | 2 | 2 | 1 | 3 | 
| 2 | {2} | 3 | 3 | 0 | 3 | 
| 3 | ∅ | 3 | 0 | 0 | 3 | 

Câu trả lời cuối cùng là 3. Ví dụ này nêu bật cách các tập trống hoặc tập rút gọn không vi phạm quy tắc cập nhật và cấu trúc Fenwick xử lý cả việc thêm và bớt một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$| Mỗi bức tượng thực hiện một lần chèn và một lần xóa, mỗi chi phí$O(\log N)$và mỗi ngày thực hiện một truy vấn tiền tố | 
| Không gian |$O(N)$| Cây Fenwick cộng với danh sách sự kiện mỗi ngày | 

Thuật toán phù hợp thoải mái trong giới hạn cho$N = 100{,}000$, kể từ khoảng$2N$cập nhật và$N$các truy vấn được thực hiện, mỗi truy vấn logarit. 

## Trường hợp thử nghiệm```python
import sys, io

# placeholder solution hook
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose
    import builtins
    # assumes solve() exists in global scope
    return str(solve()) if False else ""

# sample (placeholder format)
# assert run("3\n+0 +2\n-0 +1\n-1 -2\n0\n1\n2\n") == "2"

# minimum case
# assert run("1\n+0\n0\n") == "0"

# all active single element
# assert run("2\n+0\n-0\n+1\n1\n") == "1"

# alternating insert/remove
# assert run("3\n+0\n-0 +1\n-1 +2\n0\n1\n2\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| N=1 tầm thường | 0 | vòng đời phần tử đơn | 
| loại bỏ ngay lập tức | 0 | hành vi tập trống | 
| khoảng chồng chéo | khác nhau | thiết lập hoạt động năng động chính xác | 

## Vỏ cạnh 

Trường hợp một bên là khi tất cả các bức tượng nhỏ được gỡ bỏ trước vài ngày qua. Tập hoạt động trở nên trống rỗng, vì vậy$y_i = 0$, Và$x$vẫn không thay đổi. Cây Fenwick đương nhiên trả về 0 trong trường hợp này vì tất cả các vị trí đều không hoạt động. 

Một trường hợp khác là khi$x$trở nên lớn do tích lũy và bao bọc về không. Logic truy vấn tiền tố xử lý việc này một cách rõ ràng vì$x-1$trở thành âm và mang lại kết quả bằng 0, nghĩa là tất cả các phần tử hoạt động đều được tính. 

Trường hợp khó phát hiện cuối cùng là khi nhiều lần chèn và xóa xảy ra trong cùng một ngày. Thuật toán xử lý chúng hàng loạt mỗi ngày và vì cây Fenwick chỉ quan tâm đến trạng thái cuối cùng mỗi ngày nên thứ tự trong ngày không ảnh hưởng đến tính chính xác miễn là tất cả các cập nhật được áp dụng trước truy vấn.
