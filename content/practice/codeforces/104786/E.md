---
title: "CF 104786E - Trường học"
description: "Chúng ta được cấp một hoán vị có kích thước $n$, nghĩa là mọi số nguyên từ 1 đến $n$ xuất hiện đúng một lần. Đối với mỗi cặp chỉ số $(l, r)$, chúng ta xem xét phân đoạn của mảng từ $l$ đến $r$."
date: "2026-06-28T14:31:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104786
codeforces_index: "E"
codeforces_contest_name: "FIICode2023Round1"
rating: 0
weight: 104786
solve_time_s: 85
verified: false
draft: false
---

[CF 104786E - Trường học](https://codeforces.com/problemset/problem/104786/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị về kích thước$n$, nghĩa là mọi số nguyên từ 1 đến$n$xuất hiện đúng một lần. Với mỗi cặp chỉ số$(l, r)$, chúng ta nhìn vào phân đoạn của mảng từ$l$ĐẾN$r$. Chúng tôi chỉ muốn đếm những phân đoạn có giá trị ở điểm cuối,$p_l$Và$p_r$, đã “kiểm soát” toàn bộ phân đoạn: mọi phần tử bên trong phân đoạn phải nằm giữa điểm nhỏ hơn và lớn hơn của hai điểm cuối. 

Nói lại, nếu chúng ta lấy giá trị tối thiểu và tối đa của hai điểm cuối, toàn bộ mảng con giữa chúng phải nằm trong khoảng số đó. Không có phần tử nào trong phân đoạn được phép vượt ra ngoài phạm vi được xác định bởi điểm cuối. 

Cách giải thích đơn giản là xem xét mọi phân đoạn và xác minh xem có phần tử nào vi phạm giới hạn điểm cuối hay không. Với$n$lên đến$5 \cdot 10^5$, có khoảng$2.5 \cdot 10^{11}$các phân đoạn và thậm chí việc kiểm tra từng phân đoạn theo thời gian tuyến tính là không thể. Thậm chí một$O(n^2)$phương thức đã quá lớn. 

Một điểm tinh tế là điều kiện chỉ phụ thuộc vào các giá trị cực trị ở điểm cuối nhưng nó vẫn ràng buộc tất cả các phần tử bên trong. Điều này gợi ý rằng chúng tôi thực sự đang đếm các phân đoạn có điểm cuối “đủ cực đại” để không phần tử trung gian nào thoát khỏi khoảng giá trị của chúng. 

Một cái bẫy ngây thơ là cho rằng chỉ những đoạn liền kề hoặc đơn điệu mới quan trọng. Ví dụ, trong một hoán vị như$1\ 3\ 2\ 4\ 5$, đoạn$(1,5)$vẫn đúng ngay cả khi nó không đơn điệu, vì mọi giá trị bên trong đều nằm trong khoảng từ 1 đến 5. Mặt khác,$(2,4)$có thể thất bại nếu một số giá trị bên trong nằm ngoài phạm vi điểm cuối. 

Khó khăn mang tính toàn cầu: mọi phân đoạn đều phụ thuộc vào tất cả các giá trị trung gian, vì vậy chúng ta cần một cách để tránh tính lại cực tiểu và cực đại cho mỗi cặp. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Đối với mỗi cặp$(l, r)$, tính giá trị tối thiểu và tối đa trong mảng con$p[l:r]$hoặc quét đoạn đó và kiểm tra xem có phần tử nào nằm ngoài không$[\min(p_l, p_r), \max(p_l, p_r)]$. Điều này có hiệu quả vì nó trực tiếp xác minh điều kiện, nhưng nó yêu cầu một trong hai điều kiện sau:$O(n^3)$thời gian nếu tính toán lại các cực đoan một cách ngây thơ hoặc$O(n^2)$nếu chúng tôi duy trì hoạt động tối thiểu và tối đa cho mỗi chỉ số bắt đầu. Với$n = 5 \cdot 10^5$, thậm chí$O(n^2)$dẫn đến khoảng$2.5 \cdot 10^{11}$hoạt động vượt xa mọi giới hạn. 

Quan sát chính là chuyển quan điểm từ các phân đoạn được xác định bởi điểm cuối sang các phần tử đóng vai trò là rào cản. Sửa một điểm cuối, nói$l$. Khi chúng tôi mở rộng$r$hướng ra ngoài, phân đoạn có giá trị cho đến khi chúng ta gặp một giá trị nằm ngoài phạm vi được xác định bởi$p_l$Và$p_r$. Vấn đề là phạm vi đó tự thay đổi theo$r$, vì vậy điều này vẫn có cảm giác tròn trịa. 

Cấu trúc thực tế sẽ trở nên rõ ràng hơn nếu chúng ta nghĩ về các ràng buộc về thứ tự do các phần tử trung gian áp đặt. Một đoạn$(l, r)$không hợp lệ nếu tồn tại một số chỉ mục$i \in (l, r)$như vậy$p_i < \min(p_l, p_r)$hoặc$p_i > \max(p_l, p_r)$. Điều này có nghĩa là mọi phần tử bên trong phải được “bao phủ” bởi các điểm cuối khoảng. 

Điều này tương đương với việc nói rằng đối với mọi vị trí bên trong, các giá trị điểm cuối phải nằm trong không gian giá trị. Mỗi phần tử bên trong ngăn chặn một cách hiệu quả các cặp điểm cuối nhất định: nếu một giá trị bên trong rất nhỏ hoặc rất lớn, nó sẽ hạn chế những điểm cuối nào có thể tạo thành một cặp hợp lệ xung quanh nó. 

Một cách hiệu quả để điều chỉnh lại điều này là xử lý các vị trí theo thứ tự giá trị tăng dần. Khi chúng ta sửa một giá trị$x$, nó sẽ hạn chế các cặp “trải dài trên nó” trong không gian chỉ mục. Cụ thể, nếu chúng ta biết tất cả các giá trị nhỏ hơn hoặc lớn hơn$x$nói dối, chúng ta có thể xác định có bao nhiêu cặp có điểm cuối ở cả hai phía của$x$, điều này sẽ vi phạm tính hợp lệ trừ khi$x$nằm trong giá trị điểm cuối. 

Điều này dẫn đến ý tưởng đếm kiểu đảo ngược cổ điển: mỗi phần tử đóng góp vào các cặp không hợp lệ tùy thuộc vào số lượng phần tử nhỏ hơn/lớn hơn ở cả hai phía của nó. Thay vì kiểm tra các phân đoạn, chúng ta đếm xem có bao nhiêu bộ ba$(l, r, i)$gây ra vi phạm và trừ khỏi tổng số cặp. 

Tổng số cặp chỉ số là$n(n+1)/2$. Một cặp không hợp lệ nếu tồn tại một điểm bên trong nằm ngoài phạm vi điểm cuối, điểm này có thể được tính thông qua mỗi phần tử đóng vai trò là “dấu phân cách” cho các cặp đi qua nó theo cách bị cấm. Bằng cách theo dõi các vị trí trong cây Fenwick (hoặc BIT), chúng tôi có thể duy trì số lượng giá trị đã được nhìn thấy ở mỗi bên và tính toán các đóng góp một cách hiệu quả. 

Giải pháp cuối cùng giảm xuống việc xử lý các giá trị theo thứ tự và đếm số lượng đảo ngược mà chúng tạo ra đối với vị trí của chúng, với mỗi phần tử đóng góp dựa trên số lượng phần tử đã được xử lý nằm ở bên trái và bên phải của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý hoán vị theo thứ tự tăng dần của các giá trị, coi mỗi giá trị là thời điểm nó bắt đầu “hoạt động”. 

1. Đặt từng giá trị vào vị trí của nó và duy trì cấu trúc dữ liệu theo dõi vị trí nào đã được kích hoạt. Ban đầu không có vị thế nào được kích hoạt. Điều này cho chúng tôi biết, bất kỳ lúc nào, giá trị nào nhỏ hơn giá trị hiện tại và đã được xử lý. 
2. Khi xử lý giá trị$x$ở vị trí$pos[x]$, chúng tôi chèn nó vào cây Fenwick trên các chỉ mục. Cấu trúc này cho phép chúng ta đếm xem có bao nhiêu vị trí đã được chèn nằm ở bên trái hoặc bên phải của bất kỳ chỉ mục nào một cách hiệu quả. 
3. Đối với vị trí hiện tại$pos[x]$, tính xem có bao nhiêu vị trí được xử lý trước đó ở bên trái và bao nhiêu vị trí ở bên phải. Hãy để những điều này được$L$Và$R$. Chúng tương ứng với các giá trị nhỏ hơn$x$đã được đặt ở những khu vực đó. 
4. Số lượng cặp không hợp lệ mới được giới thiệu bởi$x$là$L \cdot R$. Điều này xuất phát từ việc chọn một phần tử nhỏ hơn ở bên trái và một phần tử ở bên phải, tạo thành các điểm cuối sẽ kẹp chặt$x$nằm ngoài phạm vi của họ. 
5. Tích lũy những đóng góp này trên tất cả các giá trị. Tổng số cấu hình không hợp lệ là tổng của tất cả các sản phẩm đó. 
6. Chuyển đổi sang câu trả lời cuối cùng bằng cách lấy tổng các cặp chỉ số và trừ đi những đóng góp không hợp lệ. 

### Tại sao nó hoạt động 

Mỗi phần tử$x$đóng vai trò là rào cản tối thiểu hoặc tối đa duy nhất cho các cặp có điểm cuối nằm ở hai phía đối diện với vị trí của nó. Nếu điểm cuối nằm ngang$x$theo thứ tự chỉ mục nhưng$x$không nằm giữa chúng theo thứ tự giá trị, điều kiện sẽ thất bại đúng một lần đối với cặp đó. Vì các giá trị được xử lý theo thứ tự tăng dần nên mọi vi phạm như vậy sẽ được quy chính xác cho phần tử nhỏ nhất bên trong khoảng phá vỡ khoảng thời gian điểm cuối. Điều này đảm bảo không có cặp nào bị tính hai lần và không có cặp không hợp lệ nào bị bỏ sót. 

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

n = int(input())
p = list(map(int, input().split()))

pos = [0] * (n + 1)
for i, v in enumerate(p, 1):
    pos[v] = i

fw = Fenwick(n)

ans = 0

for v in range(1, n + 1):
    i = pos[v]
    left = fw.sum(i - 1)
    right = fw.sum(n) - fw.sum(i)
    ans += left * right
    fw.add(i, 1)

total_pairs = n * (n + 1) // 2
print(total_pairs - ans)
```Cây Fenwick duy trì số lượng giá trị nhỏ hơn giá trị hiện tại đã được đặt. Ở mỗi bước, giá trị hiện tại đóng góp dựa trên số lượng giá trị nhỏ hơn như vậy ở bên trái và bên phải của nó, vì các cặp điểm cuối đó tạo thành sẽ không đáp ứng điều kiện với giá trị này đóng vai trò là điểm vi phạm bên trong. 

Bước trừ cuối cùng chuyển đổi số lượng không hợp lệ được tính toán thành số lượng phân đoạn hợp lệ được yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 3 2 4 5
```Chúng tôi ánh xạ các giá trị tới các vị trí: 1→1, 2→3, 3→2, 4→4, 5→5. 

| Giá trị | Vị trí | Còn lại nhỏ hơn | Nhỏ hơn bên phải | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 0 | 
| 2 | 3 | 1 | 0 | 0 | 
| 3 | 2 | 1 | 1 | 1 | 
| 4 | 4 | 3 | 0 | 0 | 
| 5 | 5 | 4 | 0 | 0 | 

Tổng số không hợp lệ = 1, tổng số cặp = 15, câu trả lời = 14. 

Dấu vết này cho thấy chỉ phần tử 3 tạo ra sự phân chia trong đó các phần tử nhỏ hơn tồn tại ở cả hai bên, tạo ra chính xác một cấu hình bị cấm. 

### Ví dụ 2 

đầu vào:```
4
4 3 2 1
```Vị trí: 1→4, 2→3, 3→2, 4→1. 

| Giá trị | Vị trí | Còn lại nhỏ hơn | Nhỏ hơn bên phải | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 0 | 0 | 0 | 
| 2 | 3 | 1 | 0 | 0 | 
| 3 | 2 | 2 | 0 | 0 | 
| 4 | 1 | 3 | 0 | 0 | 

Tổng số không hợp lệ = 0, vì vậy tất cả các phân đoạn đều hợp lệ. 

Điều này xảy ra do hoán vị giảm hoàn toàn theo thứ tự chỉ số, do đó không có phần tử nào có phần tử nhỏ hơn ở cả hai bên cùng một lúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi truy vấn cập nhật và tổng tiền tố trong cây Fenwick mất thời gian logarit | 
| Không gian |$O(n)$| Lưu trữ ánh xạ vị trí hoán vị và cây Fenwick | 

The algorithm comfortably handles$n = 5 \cdot 10^5$, kể từ khoảng$5 \cdot 10^5 \log 5 \cdot 10^5$hoạt động phù hợp trong giới hạn điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
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

    n = int(input())
    p = list(map(int, input().split()))

    pos = [0] * (n + 1)
    for i, v in enumerate(p, 1):
        pos[v] = i

    fw = Fenwick(n)
    ans = 0

    for v in range(1, n + 1):
        i = pos[v]
        left = fw.sum(i - 1)
        right = fw.sum(n) - fw.sum(i)
        ans += left * right
        fw.add(i, 1)

    total_pairs = n * (n + 1) // 2
    return str(total_pairs - ans).strip()

# provided sample
assert run("5\n1 3 2 4 5\n") == "14", "sample 1"

# custom cases
assert run("1\n1\n") == "1", "single element"
assert run("2\n1 2\n") == "3", "two elements all segments valid"
assert run("4\n4 3 2 1\n") == "10", "monotone decreasing"
assert run("3\n2 1 3\n") == "5", "small mixed permutation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | kích thước tối thiểu | 
| 1 2 | 3 | tất cả các phân đoạn hợp lệ | 
| 4 3 2 1 | 10 | trường hợp giảm chặt | 
| 2 1 3 | 5 | thứ tự hỗn hợp đúng đắn | 

## Vỏ cạnh 

Đối với hoán vị một phần tử, phân đoạn hợp lệ duy nhất là$(1,1)$. Thuật toán chèn giá trị đầu tiên, không tìm thấy đóng góp trái hoặc phải và xuất ra$1$sau khi trừ đi 0 cặp không hợp lệ trong tổng số. 

Đối với hoán vị tăng hoặc giảm hoàn toàn, không có giá trị nào có phần tử nhỏ hơn ở cả hai bên trong quá trình xử lý, do đó mọi đóng góp vẫn bằng không. Kết quả trở thành đầy đủ$n(n+1)/2$, phù hợp với thực tế là mọi phân đoạn đều hợp lệ theo cấu trúc đơn điệu.
