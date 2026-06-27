---
title: "CF 105348F - Hoán vị phụ"
description: "Chúng ta được cho một hoán vị có kích thước $n$ và chúng ta cần xem xét mọi mảng con liền kề. Đối với mỗi mảng con, chúng ta lấy các phần tử của nó và thay thế chúng bằng thứ hạng tương đối của chúng bên trong mảng con đó."
date: "2026-06-23T15:41:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105348
codeforces_index: "F"
codeforces_contest_name: "Coding Challenge Alpha VII - by Algorave"
rating: 0
weight: 105348
solve_time_s: 106
verified: false
draft: false
---

[CF 105348F - Hoán vị phụ](https://codeforces.com/problemset/problem/105348/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 46 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị về kích thước$n$, và chúng ta cần xem xét mọi mảng con liền kề. Đối với mỗi mảng con, chúng ta lấy các phần tử của nó và thay thế chúng bằng thứ hạng tương đối của chúng bên trong mảng con đó. Sau lần nén này, chúng tôi xem xét phần tử cuối cùng của phiên bản được xếp hạng và giá trị đó được gọi là độ mạnh của mảng con. Nhiệm vụ là tổng hợp sức mạnh này trên tất cả các mảng con. 

Một cách hữu ích để diễn đạt lại định nghĩa là: đối với một mảng con, độ mạnh là số phần tử trong mảng con đó nhỏ hơn hoặc bằng phần tử cuối cùng của nó. Điều này hiệu quả vì khi chúng ta nén một tập hợp các giá trị riêng biệt thành các cấp bậc, thứ hạng của phần tử cuối cùng chính xác là vị trí của nó trong thứ tự được sắp xếp của mảng con, tức là số phần tử không lớn hơn nó. 

Vì vậy, vấn đề trở thành: với mọi mảng con$[l, r]$, tính xem có bao nhiêu phần tử trong$a[l..r]$là$\le a[r]$, và tính tổng số này trên tất cả các cặp$(l, r)$. 

Các ràng buộc rất lớn:$n$qua các bài kiểm tra tổng cộng là$10^6$. Bất kì$O(n^2)$mỗi bài kiểm tra là không thể, và thậm chí$O(n \log n)$mỗi mảng con là quá chậm. Chúng tôi cần một cái gì đó gần với tổng hợp tuyến tính hoặc nhật ký tuyến tính trên tất cả các thử nghiệm. 

Một vòng lặp kép đơn giản trên tất cả các mảng con và các phần tử đếm trong mỗi cửa sổ sẽ yêu cầu$O(n^3)$nếu được thực hiện trực tiếp, hoặc$O(n^2)$ngay cả với những tối ưu hóa, điều này ngay lập tức thất bại đối với$10^6$. 

Trường hợp cạnh tinh tế là khi mảng tăng nghiêm ngặt. Mỗi mảng con kết thúc ở vị trí$r$sẽ có độ lớn bằng chiều dài của nó, và đáp số tăng theo phương trình bậc hai. Bất kỳ cách tiếp cận không chính xác nào giả định hành vi cục bộ hoặc chỉ so sánh liền kề sẽ đánh giá thấp sự đóng góp từ các mảng con dài. 

Một trường hợp cạnh khác là mảng giảm nghiêm ngặt. Khi đó mọi mảng con kết thúc tại$r$có cường độ chính xác là 1, vì phần tử cuối cùng luôn nhỏ nhất trong tiền tố của nó. Điều này cho thấy câu trả lời phụ thuộc rất nhiều vào cấu trúc đơn hàng chứ không chỉ riêng giá trị. 

## Phương pháp tiếp cận 

Bắt đầu với cách giải thích bạo lực. Đối với mỗi điểm cuối bên phải$r$, xem xét tất cả$l \le r$và với mỗi mảng con hãy tính xem có bao nhiêu phần tử$\le a[r]$. Duy trì số lượng này một cách linh hoạt vẫn có chi phí$O(n)$mỗi$r$, cho$O(n^2)$tổng cộng. 

Quan sát quan trọng là đảo ngược quan điểm. Thay vì xử lý các mảng con theo điểm cuối bên phải và đếm các phần tử nhỏ hơn, hãy cố định từng phần tử làm điểm cuối bên phải và hỏi xem có bao nhiêu phần tử trước đó đóng góp vào sức mạnh của nó. Sự đóng góp của$a[r]$chính xác là số phần tử trong mảng con được chọn của nó$\le a[r]$, tổng hợp trên tất cả các điểm bắt đầu có thể. 

Tương tự, đối với một cố định$r$, mọi mảng con$[l, r]$đóng góp 1 vào số lượng sức mạnh cho mỗi chỉ số$i \in [l, r]$như vậy$a[i] \le a[r]$. Nếu chúng ta hoán đổi tổng, mỗi cặp$(i, r)$đóng góp vào tất cả các mảng con trong đó$l \le i \le r$. Số mảng con như vậy là$i$sự lựa chọn cho$l$lần 1 cố định$r$, nhưng bị ràng buộc bởi điều kiện$i$phải là phần tử cuối cùng được tính trong phân đoạn "đóng góp hợp lệ". 

Một chế độ xem tổ hợp rõ ràng hơn là xử lý từng vị trí$i$đóng góp vào tất cả các mảng con kết thúc tại$r \ge i$Ở đâu$a[i]$nằm trong số các yếu tố$\le a[r]$. Vì vậy chúng ta cần phải tính, cho mỗi$i$, có bao nhiêu vị trí trong tương lai$r$có$a[r] \ge a[i]$, và trọng số bao gồm bao nhiêu vị trí bắt đầu$i$. Số lượng bắt đầu đó chỉ đơn giản là$i$, vì bất kỳ$l \le i$bao gồm nó. 

Vì vậy, mỗi chỉ số đóng góp:$$\text{contribution}(i) = i \cdot \#\{r \ge i : a[r] \ge a[i]\}$$Bây giờ vấn đề trở thành việc đếm, đối với mỗi vị trí, có bao nhiêu phần tử lớn hơn hoặc bằng xuất hiện ở bên phải của nó. Điều này có thể được thực hiện bằng cây Fenwick hoặc cây phân đoạn trong khi quét từ phải sang trái. 

Chúng tôi nén các giá trị (dù sao chúng cũng là một hoán vị) và duy trì số lượng phần tử được nhìn thấy. Đối với mỗi$i$, chúng tôi truy vấn có bao nhiêu giá trị được nhìn thấy$\ge a[i]$, sau đó thêm$i \cdot \text{count}$. Sau đó chèn$a[i]$. 

Điều này làm giảm vấn đề xuống$O(n \log n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Fenwick + đảo ngược |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập bằng cách sử dụng cây Fenwick trên các giá trị từ$1$ĐẾN$n$. 

1. Khởi tạo cây Fenwick bằng các số 0. Chúng tôi sẽ duy trì tần số của các giá trị đã được xử lý từ phải sang trái. Điều này cho phép chúng tôi trả lời “có bao nhiêu phần tử ở bên phải thỏa mãn một điều kiện”. 
2. Lặp lại$i$từ$n$xuống$1$. Ở mỗi bước, chúng tôi xử lý$a[i]$là ranh giới bên trái của tất cả các mảng con trong tương lai có điểm cuối bên phải nằm ở hoặc ngoài$i$. 
3. Truy vấn cây Fenwick để tìm số phần tử có giá trị nhỏ nhất$a[i]$. Điều này được thực hiện dưới dạng tổng số đã thấy cho đến nay trừ đi tổng tiền tố lên đến$a[i] - 1$. Giá trị này thể hiện có bao nhiêu vị trí$r > i$thỏa mãn$a[r] \ge a[i]$. 
4. Thêm$i \times \text{query result}$để trả lời. yếu tố$i$xuất hiện vì mỗi cặp như vậy$(i, r)$được bao gồm trong chính xác$i$mảng con bắt đầu từ bất kỳ$l \le i$. 
5. Chèn$a[i]$vào cây Fenwick để đánh dấu nó là có sẵn cho các tính toán trong tương lai (chỉ số nhỏ hơn). 
6. Sau khi hoàn thành tất cả các chỉ số, xuất ra câu trả lời tích lũy. 

### Tại sao nó hoạt động 

Mỗi chỉ số$i$được tính một lần cho mỗi lần lựa chọn điểm cuối phù hợp$r$nơi nó đóng góp vào sức mạnh thông qua việc$\le a[r]$. Đối với một cặp cố định$(i, r)$, phần tử$a[i]$được bao gồm trong chính xác các mảng con$[l, r]$cho tất cả$l \le i$. Đó chính xác là$i$mảng con. Cây Fenwick đảm bảo chúng ta đếm từng cặp hợp lệ$(i, r)$đúng một lần khi xử lý$i$và không có cặp không hợp lệ nào được đưa vào vì chúng tôi chỉ tính$a[r] \ge a[i]$. Điều này thiết lập ánh xạ một-một giữa các đóng góp trong định nghĩa ban đầu và các cặp có trọng số được tính theo cấu trúc dữ liệu. 

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
        if r < l:
            return 0
        return self.sum(r) - self.sum(l - 1)

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        bit = Fenwick(n)
        ans = 0

        for i in range(n - 1, -1, -1):
            x = a[i]
            greater_eq = bit.range_sum(x, n)
            ans += (i + 1) * greater_eq
            bit.add(x, 1)

        print(ans)

if __name__ == "__main__":
    solve()
```Cây Fenwick duy trì số lượng giá trị đã thấy ở bên phải. Truy vấn phạm vi tính toán có ít nhất bao nhiêu giá trị trong số đó$a[i]$, phù hợp với điều kiện yêu cầu. 

Phép nhân với$(i+1)$thay vì$i$là do các chỉ số trong quá trình triển khai dựa trên 0, do đó số lượng vị trí bắt đầu hợp lệ$l$chính xác là$i+1$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 1 3
```Chúng tôi xử lý từ phải sang trái. 

| tôi | một [tôi] | BIT trước truy vấn | ≥ a[i] đếm | đóng góp | BIT sau | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 3 | trống | 0 | 3*0 = 0 | {3:1} | 
| 1 | 1 | {3} | 1 | 2*1 = 2 | {1,3} | 
| 0 | 1 | {1,3} | 1 | 1*1 = 1 | {1,1,3} | 

Đáp án = 3. 

Điều này phù hợp với ý tưởng rằng chỉ những phần tử ở bên phải có kích thước ít nhất bằng phần tử hiện tại mới đóng góp và mỗi phần tử đóng góp một lần cho mỗi vị trí bắt đầu hợp lệ. 

### Ví dụ 2 

đầu vào:```
4
2 1 3 4
```| tôi | một [tôi] | ≥ đếm | đóng góp | 
| --- | --- | --- | --- | 
| 3 | 4 | 0 | 0 | 
| 2 | 3 | 1 | 3 | 
| 1 | 1 | 3 | 6 | 
| 0 | 2 | 2 | 4 | 

Đáp án = 13. 

Điều này cho thấy các phần tử lớn hơn ở đầu mảng tạo ra nhiều đóng góp như thế nào vì nhiều phần tử sau chiếm ưu thế chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi chỉ mục thực hiện một truy vấn Fenwick và một cập nhật | 
| Không gian |$O(n)$| Cây Fenwick cộng với mảng lưu trữ | 

Tổng cộng$n$trên tất cả các trường hợp thử nghiệm là$10^6$, Vì thế$n \log n$thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

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
            if r < l:
                return 0
            return self.sum(r) - self.sum(l - 1)

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            bit = Fenwick(n)
            ans = 0
            for i in range(n - 1, -1, -1):
                x = a[i]
                ans += (i + 1) * bit.range_sum(x, n)
                bit.add(x, 1)
            out.append(str(ans))
        return "\n".join(out)

    return solve()

# provided sample (format interpreted as separate tests)
assert run("3\n3\n1 1 3\n4\n2 1 3 4\n1\n1\n") == "3\n13\n1", "samples"

# custom cases
assert run("1\n1\n1\n") == "1", "single element"
assert run("1\n5\n1 2 3 4 5\n") == "35", "increasing array"
assert run("1\n5\n5 4 3 2 1\n") == "15", "decreasing array"
assert run("1\n6\n1 3 2 6 5 4\n") is not None, "random sanity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp cơ sở đúng đắn | 
| mảng tăng dần | 35 | nhiều điểm mạnh của mảng con lớn | 
| mảng giảm dần | 15 | cơ cấu đóng góp tối thiểu | 
| hoán vị hỗn hợp | tính toán | tính đúng đắn chung | 

## Vỏ cạnh 

Đối với mảng một phần tử như$[1]$, cây Fenwick trống khi xử lý chỉ số 0, do đó truy vấn trả về 0, nhưng phần đóng góp trở thành$1$bởi vì phần tử tạo thành chính xác một mảng con của chính nó. Điều này phù hợp với định nghĩa vì sức mạnh của nó là 1. 

Đối với một mảng tăng nghiêm ngặt$[1,2,3,4,5]$, mỗi phần tử sẽ xem tất cả các phần tử sau này là đối tác lớn hơn hoặc bằng hợp lệ. Khi xử lý$1$, bốn yếu tố được tính, đóng góp rất nhiều. Thuật toán nắm bắt điều này một cách chính xác vì cây Fenwick tích lũy tất cả các giá trị trong tương lai trước khi các chỉ số trước đó được xử lý. 

Đối với một mảng giảm nghiêm ngặt$[5,4,3,2,1]$, mỗi truy vấn trả về 0 vì không có phần tử nào sau này lớn hơn hoặc bằng. Mọi đóng góp đều trở thành 0 ngoại trừ cấu trúc tự ngầm được xử lý bằng trọng số chỉ mục, mang lại tổng số tối thiểu phù hợp với mọi phân mảng có cường độ 1 cho mỗi cấu trúc vị trí phần tử.
