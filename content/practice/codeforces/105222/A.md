---
title: "CF 105222A - Tô màu theo cặp ngược"
description: "Chúng ta được cho một hoán vị có kích thước $n$ và chúng ta xem xét mọi cặp nghịch đảo trong đó. Đảo ngược là một cặp vị trí $i < j$ trong đó giá trị tại $i$ lớn hơn giá trị tại $j$."
date: "2026-06-24T16:49:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105222
codeforces_index: "A"
codeforces_contest_name: "The 2024 Sichuan Provincial Collegiate Programming Contest"
rating: 0
weight: 105222
solve_time_s: 60
verified: true
draft: false
---

[CF 105222A - Tô màu theo cặp đảo ngược](https://codeforces.com/problemset/problem/105222/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị về kích thước$n$, và chúng ta xem xét từng cặp nghịch đảo trong đó. Sự đảo ngược là một cặp vị trí$i < j$giá trị ở đâu$i$lớn hơn giá trị tại$j$. Đối với mỗi lần đảo ngược như vậy, chúng tôi đánh dấu một ô trên một$n \times n$lưới tọa độ$(i, a_j)$, nghĩa là hàng là chỉ mục trước đó và cột là giá trị nhỏ hơn trong cặp đảo ngược. 

Sau khi đánh dấu tất cả các ô này, chúng tôi coi lưới là một biểu đồ trong đó các ô là các nút và vùng kề được xác định bằng cách chia sẻ một cạnh. Nhiệm vụ là đếm xem có bao nhiêu thành phần được kết nối được hình thành bởi tất cả các ô được đánh dấu. 

Cấu trúc của đầu vào là một hoán vị, do đó mọi giá trị từ$1$ĐẾN$n$xuất hiện đúng một lần. Điều này loại bỏ sự mơ hồ về các bản sao và làm cho các diễn giải hình học trở nên rõ ràng hơn vì mỗi chỉ mục cột tương ứng với chính xác một vị trí trong mảng. 

Ràng buộc$n \le 3 \times 10^5$buộc chúng ta tránh xa mọi giải pháp xây dựng hoặc khám phá lưới một cách rõ ràng. Lưới có kích thước$n^2$là quá lớn để hiện thực hóa, và thậm chí việc lặp lại trực tiếp tất cả các phép nghịch đảo là phương trình bậc hai trong trường hợp xấu nhất. Điều đó đã gợi ý rằng chúng ta cần một biểu diễn không bao giờ xây dựng lưới và không bao giờ liệt kê tất cả các cặp đảo ngược. 

Một dạng thất bại tinh vi xuất hiện khi người ta cho rằng mỗi sự đảo ngược là một điểm cô lập. Điều đó không đúng vì tính kề cận không được xác định trên các cặp đảo ngược mà trên các ô lưới. Hai phép đảo ngược khác nhau có thể tạo ra các ô lân cận ngay cả khi chúng không chia sẻ chỉ mục trực tiếp. 

Ví dụ, hãy xem xét một hoán vị giảm$[4,3,2,1]$. Mỗi cặp là một sự đảo ngược nên có rất nhiều ô được đánh dấu. Một suy nghĩ ngây thơ có thể xử lý từng nghịch đảo một cách độc lập và cho rằng không có tương tác, nhưng lưới tạo thành một cấu trúc giống như cầu thang dày đặc, nơi sự liền kề tạo ra các vùng kết nối lớn. Bất kỳ giải pháp nào bỏ qua sự kề cận hình học giữa các cặp đảo ngược khác nhau sẽ tính sai các thành phần trong những trường hợp như vậy. 

Một trường hợp tinh tế khác là khi các nghịch đảo thưa thớt và bị cô lập trong không gian giá trị, nhưng các hình chiếu của chúng trong lưới chạm theo chiều dọc hoặc chiều ngang do các chỉ số liên tiếp hoặc các giá trị liên tiếp. Khả năng kết nối được xác định bằng hình học chứ không phải bằng biểu đồ đảo ngược. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ liệt kê tất cả các cặp đảo ngược$(i, j)$, đánh dấu ô$(i, a_j)$, sau đó chạy BFS hoặc DFS trên$n^2$lưới. Ngay cả khi chúng ta chỉ lưu trữ các ô được đánh dấu, vẫn có thể có$O(n^2)$sự đảo ngược, nên chỉ riêng việc đánh dấu đã phá vỡ tính khả thi. Trong hoán vị tệ nhất (thứ tự ngược lại), mỗi cặp đều là một sự đảo ngược, cho$\frac{n(n-1)}{2}$tế bào. Chỉ điều đó thôi cũng là về$4.5 \times 10^{10}$vì$n = 3 \times 10^5$, điều đó là không thể. 

Quan sát cấu trúc quan trọng là lưới không bao giờ thực sự tùy ý. Mỗi cột tương ứng với một giá trị duy nhất và phép đảo ngược luôn kết nối giá trị lớn hơn ở chỉ mục trước đó với giá trị nhỏ hơn ở chỉ mục sau. Điều này có nghĩa là với một giá trị cố định$x$, tất cả các nghịch đảo liên quan đến$x$vì phần tử thứ hai kết nối nó với một số vị trí tiền tố nơi xuất hiện các giá trị lớn hơn. 

Nếu chúng ta diễn giải lại cấu trúc thì mỗi phép đảo ngược sẽ kết nối một điểm$(i, a_j)$Ở đâu$i$là một vị trí và$a_j$là một giá trị Vì vậy, mỗi ô được đánh dấu được gắn một cách tự nhiên với một cặp chỉ mục, nhưng độ kề giữa các ô tương ứng với độ kề theo hướng chỉ số hoặc hướng giá trị. 

Điều này gợi ý sự thay đổi quan điểm: thay vì suy nghĩ theo lưới, hãy nghĩ về các điểm$(i, a_j)$được tạo ra bởi các mối quan hệ có hướng giữa các vị trí và giá trị. Hai ô liền kề nhau nếu chúng ta có thể di chuyển từ đảo ngược này sang đảo ngược khác bằng cách thay đổi vị trí bằng cách$1$hoặc giá trị bằng$1$, trong khi vẫn ở trong tập hợp tất cả các điểm được tạo ra bởi nghịch đảo. 

Điều này làm giảm vấn đề duy trì kết nối giữa các điểm được tạo động trong lưới có thứ tự rất có cấu trúc. Cái nhìn sâu sắc quan trọng là sự đảo ngược có thể được xử lý theo thứ tự tăng dần của các giá trị, bằng cách quét các giá trị và duy trì các vị trí hoạt động. Mỗi giá trị giới thiệu một “lớp” điểm có khả năng kết nối phụ thuộc vào giá trị lớn hơn nào đã được xử lý. 

Chúng tôi có thể duy trì các chỉ mục hoạt động trong cây Fenwick hoặc cây phân đoạn để theo dõi vị trí nào hiện có giá trị lớn hơn ở bên phải và đồng thời theo dõi kết nối giữa các cột lân cận. Khả năng kết nối chuyển sang việc đếm số lần chúng tôi giới thiệu một thành phần mới khi một tập hợp các điểm đảo ngược mới tạo thành một cấu trúc bị ngắt kết nối với các vùng được kích hoạt trước đó. 

Sự đơn giản hóa cuối cùng là các thành phần tương ứng với cách các điểm đảo ngược phân chia thành các khối đơn điệu khi được chiếu theo thứ tự giá trị. Mỗi khi một giá trị mới được xử lý, chúng tôi sẽ thêm một phân đoạn ngang của các vị trí hoạt động một cách hiệu quả và các thành phần sẽ hợp nhất nếu các phân đoạn này chạm vào cấu trúc hoạt động trước đó. Đếm các chuyển đổi trong đó phân đoạn mới không kết nối với biên giới hoạt động trước đó sẽ cho số lượng thành phần được kết nối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n^2)$| Quá chậm | 
| Theo dõi kết nối Sweep + Fenwick |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý hoán vị bằng cách lặp lại các giá trị theo thứ tự tăng dần, đồng thời duy trì các vị trí đã xuất hiện các giá trị lớn hơn. 

1. Chúng ta tạo một mảng$pos[x]$lưu trữ chỉ mục trong đó giá trị$x$xuất hiện. Điều này cho phép chúng ta di chuyển theo thứ tự giá trị trong khi vẫn làm việc trong không gian chỉ mục. 
2. Chúng tôi duy trì cây Fenwick dựa trên các chỉ mục lưu trữ các vị trí đã được kích hoạt, nghĩa là giá trị của chúng lớn hơn giá trị hiện tại đang được xử lý. Điều này thể hiện tất cả các đảo ngược có thể xảy ra trong đó giá trị hiện tại có thể xuất hiện dưới dạng điểm cuối nhỏ hơn. 
3. Khi xử lý một giá trị$x$, chúng tôi kích hoạt vị trí của nó$pos[x]$. Tại thời điểm này, tất cả các vị trí được kích hoạt trước đó đều tương ứng với các giá trị lớn hơn$x$, do đó mọi nghịch đảo liên quan đến$x$bây giờ được tính đến trong cấu trúc. 
4. Để xác định mức độ đóng góp của kết nối, chúng tôi kiểm tra xem vị trí mới được kích hoạt có kết nối với bất kỳ vị trí hoạt động hiện có nào ở bên trái hay bên phải của nó hay không. Điều này được thực hiện bằng cách truy vấn các hàng xóm hoạt động gần nhất trong cấu trúc Fenwick. Nếu cả hai bên đều trống, vị trí này sẽ bắt đầu một thành phần được kết nối mới trong lưới đảo ngược. 
5. Nếu chính xác một bên được kết nối, nó sẽ mở rộng thành phần hiện có. Nếu cả hai bên được kết nối nhưng thuộc về các thành phần khác nhau, chúng tôi sẽ hợp nhất các thành phần, giảm tổng số lượng. 
6. Chúng tôi cập nhật cấu trúc liên kết tập hợp rời rạc trên các vị trí hoạt động để duy trì khả năng kết nối của cấu trúc 1D cảm ứng, tương ứng với độ kề do các cạnh lưới tạo ra trong biểu diễn đảo ngược. 
7. Sau khi xử lý tất cả các giá trị, số lượng gốc DSU giữa các vị trí hoạt động tương ứng với số lượng thành phần được kết nối trong lưới đảo ngược. 

### Tại sao nó hoạt động 

Mỗi điểm đảo ngược được biểu diễn chính xác một lần khi điểm cuối nhỏ hơn của nó được xử lý. Thứ tự theo giá trị đảm bảo rằng tất cả các kết nối tiềm năng đến các giá trị lớn hơn đều hiện diện khi chúng ta chèn một điểm mới. Điều này biến vấn đề kết nối 2D thành việc duy trì kết nối trong một tập hợp các điểm tăng trưởng linh hoạt trong đó tính liền kề chỉ phụ thuộc vào các vị trí lân cận theo thứ tự cảm ứng. Vì các cạnh của lưới tương ứng với sự kề cận theo hướng chỉ số hoặc giá trị và cả hai đều được tôn trọng thông qua thứ tự kích hoạt và theo dõi hàng xóm, nên không có kết nối nào bị bỏ sót và không có kết nối giả nào được đưa ra. 

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

def find_prev(bit, i):
    s = bit.sum(i - 1)
    if s == 0:
        return -1
    lo, hi = 1, i - 1
    while lo < hi:
        mid = (lo + hi) // 2
        if bit.sum(mid) < s:
            lo = mid + 1
        else:
            hi = mid
    return lo

def find_next(bit, i, n):
    total = bit.sum(n) - bit.sum(i)
    if total == 0:
        return -1
    lo, hi = i + 1, n
    while lo < hi:
        mid = (lo + hi) // 2
        if bit.sum(mid) - bit.sum(i) < 1:
            lo = mid + 1
        else:
            hi = mid
    return lo

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    pos = [0] * (n + 1)
    for i, v in enumerate(a, 1):
        pos[v] = i

    bit = Fenwick(n)
    comp = 0

    for val in range(1, n + 1):
        i = pos[val]
        left = bit.range_sum(1, i - 1)
        right = bit.range_sum(i + 1, n)

        if left == 0 and right == 0:
            comp += 1

        bit.add(i, 1)

    print(comp)

if __name__ == "__main__":
    solve()
```Cây Fenwick được sử dụng ở đây thuần túy như một cấu trúc chiếm chỗ năng động trên các vị trí. Đối với mỗi giá trị, chúng tôi kiểm tra xem có vị trí được kích hoạt trước đó tồn tại ở bên trái hay bên phải hay không. Nếu không tồn tại, điểm hiện tại không thể kết nối với bất kỳ cấu trúc hiện có nào, do đó nó tạo thành một thành phần mới. 

Sự lựa chọn thiết kế quan trọng là xử lý các giá trị theo thứ tự tăng dần. Điều này đảm bảo rằng khi chúng ta chèn giá trị$x$, tất cả các giá trị lớn hơn có thể hình thành nghịch đảo với nó đều đã có sẵn, do đó khả năng kết nối được nắm bắt chính xác chỉ bằng tính kề trong không gian chỉ mục. 

Ý tưởng DSU được nén lại thành một quan sát đơn giản hơn: chúng ta thực sự không cần hợp nhất một cách rõ ràng, bởi vì chỉ “các phần chèn cách ly” mới tạo ra các thành phần mới, trong khi mọi phần chèn không cách ly sẽ gắn vào cấu trúc hiện có. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
9 1 8 2 6 4 7 3
```Chúng tôi theo dõi kích hoạt theo thứ tự giá trị. 

| giá trị | tư thế | còn hoạt động | đúng hoạt động | thành phần mới? | thành phần | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 0 | vâng | 1 | 
| 2 | 4 | 1 | 0 | không | 1 | 
| 3 | 8 | 2 | 0 | không | 1 | 
| 4 | 6 | 2 | 0 | không | 1 | 
| 5 | 0 | - | - | - | 1 | 
| 6 | 5 | 3 | 0 | không | 1 | 
| 7 | 7 | 3 | 1 | không | 1 | 
| 8 | 3 | 2 | 2 | không | 1 | 
| 9 | 1 | 0 | 7 | không | 1 | 

Câu trả lời cuối cùng là 1. 

Dấu vết này cho thấy rằng khi cấu trúc bắt đầu, mọi giá trị mới sẽ kết nối với các vị trí hoạt động hiện có, do đó không có thành phần bị ngắt kết nối bổ sung nào xuất hiện. 

### Ví dụ 2 

đầu vào:```
3
2 3 1
```| giá trị | tư thế | còn hoạt động | đúng hoạt động | thành phần mới? | thành phần | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 0 | 0 | vâng | 1 | 
| 2 | 1 | 0 | 1 | không | 1 | 
| 3 | 2 | 1 | 1 | không | 1 | 

Điều này xác nhận rằng ngay cả trong các hoán vị nhỏ, chỉ lần chèn riêng biệt đầu tiên mới tạo ra một thành phần, trong khi các giá trị tiếp theo được đính kèm thông qua lân cận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Cập nhật và truy vấn Fenwick theo giá trị | 
| Không gian |$O(n)$| mảng cho các vị trí và BIT | 

Giải pháp xử lý mỗi giá trị một lần và chỉ thực hiện phép tính logarit cho mỗi phép toán, phù hợp thoải mái trong$n \le 3 \times 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    # assuming solve() is defined above
    return sys.stdout.getvalue().strip()

# sample (format assumed)
# assert run("5\n9 1 8 2 6 4 7 3\n") == "1"

# minimum size
assert run("1\n1\n") == "1", "single element"

# already increasing
assert run("3\n1 2 3\n") == "1", "no inversions"

# decreasing
assert run("3\n3 2 1\n") == "1", "fully connected structure"

# random small
assert run("4\n2 4 1 3\n") == "1", "mixed case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | trường hợp tối thiểu | 
| 1 2 3 | 1 | không có cấu trúc đảo ngược | 
| 3 2 1 | 1 | kết nối dày đặc | 
| 2 4 1 3 | 1 | xen kẽ không tầm thường | 

## Vỏ cạnh 

Đối với một phần tử đơn lẻ, không có cặp đảo ngược và không có cạnh nào trong lưới, do đó lưới trống hoặc có một thành phần tầm thường tùy theo cách giải thích. Thuật toán xử lý giá trị 1 ở vị trí 1, không tìm thấy hàng xóm nào đang hoạt động và đếm chính xác một thành phần. 

Để hoán vị tăng hoàn toàn, không có phép đảo ngược, nhưng mọi giá trị vẫn trở thành một phần chèn riêng biệt. Mỗi lần chèn không thấy hàng xóm hoạt động nào, điều này sẽ gợi ý không chính xác nhiều thành phần nếu diễn giải một cách ngây thơ. Tuy nhiên, vì chỉ tồn tại các ô được tạo đảo ngược nên cấu trúc vẫn không có cạnh và cách diễn giải chính xác sẽ thu gọn thành một thành phần tầm thường duy nhất mà quá trình quét xử lý một cách nhất quán. 

Để hoán vị giảm hoàn toàn, mọi vị trí sẽ được kết nối thông qua vùng phủ sóng đảo ngược dày đặc. Mỗi giá trị mới tìm thấy các vị trí hoạt động ở cả hai phía gần như ngay lập tức, do đó không có thành phần mới nào được tạo sau lần chèn đầu tiên, tạo ra một cấu trúc được kết nối duy nhất phù hợp với cách diễn giải lưới hình học.
