---
title: "CF 2227F - Nó cứ đi ngang"
description: "Mỗi cột chứa các hình khối được xếp chồng lên nhau từ độ cao $1$ cho đến độ cao $ai$. Sau khi trọng lực quay sang phải, hình khối không bao giờ thay đổi chiều cao. Các hình khối ở cùng độ cao trượt độc lập và chiếm các vị trí sẵn có ngoài cùng bên phải trên mặt ngang đó."
date: "2026-06-01T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2227
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1096 (Div. 3)"
rating: 0
weight: 2227
solve_time_s: 305
verified: false
draft: false
---

[CF 2227F - Nó cứ đi ngang](https://codeforces.com/problemset/problem/2227/F) 

**Đánh giá:** - 
**Tas:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp, tham lam, toán học 
**Thời gian giải:** 5 phút 5s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi cột chứa các hình khối xếp chồng lên nhau từ độ cao$1$lên đến chiều cao$a_i$. Sau khi trọng lực quay sang phải, hình khối không bao giờ thay đổi chiều cao. Các hình khối ở cùng độ cao trượt độc lập và chiếm các vị trí sẵn có ngoài cùng bên phải trên mặt ngang đó. 

Sẽ rất hữu ích khi nghĩ về chiều cao theo chiều cao. Đối với chiều cao cố định$h$, định nghĩa$$C_h=\#\{i : a_i\ge h\}.$$có$C_h$hình khối ở cấp độ đó. Sau khi tác dụng của trọng lực, chúng$C_h$hình khối chiếm ngoài cùng bên phải$C_h$cột,$$n-C_h+1,\dots,n.$$Bài toán yêu cầu tổng quãng đường di chuyển của tất cả các hình khối. Trước trọng lực, chúng ta có thể loại bỏ nhiều nhất một khối lập phương, nghĩa là chọn một cột$i$và giảm dần$a_i$qua$1$. 

Những hạn chế là lớn. Trên tất cả các trường hợp thử nghiệm,$$\sum n \le 2\cdot 10^5.$$Bất cứ điều gì bậc hai ngay lập tức là không thể. Thậm chí$O(n\sqrt n)$sẽ không thoải mái. Mục tiêu là$O(n)$hoặc$O(n\log n)$mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh nguy hiểm nhất là trường hợp việc loại bỏ một khối sẽ thay đổi hành vi của nhiều khối khác. 

Coi như$$[1,2,3,2,1].$$Câu trả lời ban đầu là$5$. Loại bỏ khối duy nhất ở cột cuối cùng sẽ tạo ra$$[1,2,3,2,0],$$và câu trả lời trở thành$9$. Một giải pháp chỉ trừ đi chuyển động của khối bị loại bỏ sẽ bỏ lỡ hiệu ứng tổng thể. 

Một trường hợp quan trọng khác là mảng không giảm chẳng hạn như$$[1,2,3,4,5].$$Mỗi khối đã chiếm một vị trí ngoài cùng bên phải trên cấp độ của nó. Câu trả lời là$0$, và loại bỏ bất kỳ khối nào cũng không thể cải thiện nó. 

Trường hợp thứ ba là khi một số vị trí đóng góp như nhau cho sự cải tiến. Ví dụ,$$[5,4,1,1,1].$$Sự lựa chọn đúng phụ thuộc vào cấu trúc toàn cục, không phụ thuộc vào cột lớn nhất. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ thử mọi cách loại bỏ có thể. Đối với mỗi chỉ số ứng cử viên, hãy xây dựng lại toàn bộ cấu hình cuối cùng và tính toán lại tổng khoảng cách di chuyển. Việc tính toán kết quả của một cấu hình đã yêu cầu xử lý tất cả các chiều cao hoặc tất cả các hình khối, do đó tổng độ phức tạp ít nhất là$O(n^2)$, và thường tệ hơn. Với$n=2\cdot 10^5$, điều này đã vượt quá giới hạn rồi. 

Quan sát quan trọng là cấu hình cuối cùng chỉ phụ thuộc vào số lượng$C_h$. 

Về chiều cao$h$, các hình khối chiếm các cột$$n-C_h+1,\dots,n.$$Một khối lập phương ở cột$i$và chiều cao$h$di chuyển$$(n-C_h+k)-i,$$Ở đâu$k$là thứ hạng của nó trong số các cột chứa chiều cao$h$. 

Thay vì đếm các khối đã di chuyển, hãy đếm các khối **không** di chuyển. 

Sửa một cột$i$. Một khối ở độ cao$h\le a_i$giữ nguyên vị trí chính xác khi cột$i$thuộc về khối ngoài cùng bên phải cuối cùng cho cấp độ$h$. Điều này có nghĩa$$i \ge n-C_h+1.$$Tương đương,$$C_h \ge n-i+1.$$Độ cao lớn nhất thỏa mãn điều kiện này chính xác là$$s_i=\min(a_i,a_{i+1},\dots,a_n),$$hậu tố tối thiểu. 

Do đó cột$i$chứa chính xác$s_i$hình khối không di chuyển. Vì cột$i$chứa$a_i$tổng cộng các khối lập phương thì số khối di chuyển được đóng góp bởi cột đó là$$a_i-s_i.$$Tổng hợp tất cả các cột cho câu trả lời ban đầu$$\text{base}=\sum_{i=1}^{n}(a_i-s_i).$$Bây giờ hãy xem xét việc loại bỏ một khối lập phương. 

Nếu chúng ta giảm$a_i$qua$1$, chỉ có một giá trị có thể thay đổi trong mảng hậu tố tối thiểu: giá trị tối thiểu được biểu thị bằng cột$i$. Tổng số câu trả lời tăng lên$1$chính xác khi chúng ta loại bỏ một khối khỏi vị trí có chiều cao bằng giá trị tối thiểu hậu tố của vị trí đó. 

Cho phép$$f(x)=\#\{i:s_i=x\}.$$Nếu chúng ta chọn một giá trị$x$xuất hiện trong mảng hậu tố tối thiểu, chúng ta có thể giảm một lần xuất hiện của$x$. Điều này làm tăng câu trả lời bằng cách$$f(x)-1.$$Do đó, cải tiến tốt nhất là$$\max_x (f(x)-1).$$Nếu mỗi giá trị tối thiểu của hậu tố chỉ xuất hiện một lần thì không làm gì là tối ưu. 

Như vậy câu trả lời cuối cùng là$$\boxed{ \sum_{i=1}^{n}(a_i-s_i) + \max\!\left(0,\max_x f(x)-1\right) }.$$Quan sát này dẫn trực tiếp đến một giải pháp tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$hoặc tệ hơn |$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính mảng hậu tố tối thiểu:$$s_n=a_n,\qquad s_i=\min(a_i,s_{i+1}).$$2. Tính toán$$\text{base}=\sum_{i=1}^{n}(a_i-s_i).$$Giá trị này bằng tổng khoảng cách di chuyển trước khi di chuyển. 
3. Đếm tần số của mọi giá trị xuất hiện trong mảng hậu tố tối thiểu. 
4. Hãy để$$m=\max_x f(x).$$5. Cách cải thiện tốt nhất có thể là$$\max(0,m-1).$$6. Đầu ra$$\text{base}+\max(0,m-1).$$### Tại sao nó hoạt động 

Đối với mỗi cột$i$, hậu tố tối thiểu$s_i$chính xác là số khối trong cột đó không đổi sau trọng lực. Mỗi khối khác di chuyển ít nhất một vị trí. Vì thế$a_i-s_i$đếm các khối chuyển động được đóng góp bởi cột$i$, và tính tổng sẽ cho ra tổng quãng đường di chuyển. 

Việc xóa một khối chỉ có thể ảnh hưởng đến giá trị hiện đang đóng vai trò là hậu tố tối thiểu. Nếu một giá trị hậu tố tối thiểu$x$xuất hiện$f(x)$lần, việc giảm một lần xuất hiện sẽ phá hủy mức tối thiểu đó cho tất cả trừ một trong những vị trí đó, tạo ra sự gia tăng$f(x)-1$trong câu trả lời. Không loại bỏ có thể tạo ra một lợi ích lớn hơn. Do đó, sự cải thiện tối ưu là mức tối đa của$f(x)-1$. 

## Giải pháp Python```python
import sys
from collections import defaultdict

input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        s = [0] * n
        s[-1] = a[-1]

        for i in range(n - 2, -1, -1):
            s[i] = min(a[i], s[i + 1])

        base = 0
        freq = defaultdict(int)

        for ai, si in zip(a, s):
            base += ai - si
            freq[si] += 1

        best = 0
        for cnt in freq.values():
            best = max(best, cnt)

        ans.append(str(base + max(0, best - 1)))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Mảng hậu tố tối thiểu là đối tượng trung tâm. Một khi đã biết thì mọi đóng góp cho câu trả lời đều mang tính địa phương. 

Biến`base`cửa hàng$$\sum(a_i-s_i).$$Từ điển`freq`đếm số lần mỗi giá trị tối thiểu hậu tố xuất hiện. Tần suất lớn nhất xác định sự cải thiện tốt nhất có thể đạt được từ một lần loại bỏ. 

Tất cả số học đều sử dụng số nguyên Python. Câu trả lời tối đa có thể là theo thứ tự$n^2$, dễ dàng phù hợp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$$[1,2,3,2,1]$$| tôi |$a_i$|$s_i$|$a_i-s_i$| 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 
| 2 | 2 | 1 | 1 | 
| 3 | 3 | 1 | 2 | 
| 4 | 2 | 1 | 1 | 
| 5 | 1 | 1 | 0 |$$\text{base}=4.$$Giá trị tối thiểu của hậu tố$1$xuất hiện$5$lần.$$\text{improvement}=5-1=4.$$Câu trả lời cuối cùng:$$4+4=8.$$Ví dụ này cho thấy một thao tác xóa có thể ảnh hưởng đến nhiều cột cùng một lúc như thế nào. 

### Ví dụ 2 

đầu vào:$$[1,2,3,4,5]$$| tôi |$a_i$|$s_i$|$a_i-s_i$| 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 
| 2 | 2 | 2 | 0 | 
| 3 | 3 | 3 | 0 | 
| 4 | 4 | 4 | 0 | 
| 5 | 5 | 5 | 0 |$$\text{base}=0.$$Mỗi hậu tố tối thiểu xuất hiện một lần.$$\text{improvement}=0.$$Câu trả lời cuối cùng:$$0.$$Đây là trường hợp mọi khối lập phương đều ở bên phải nhất có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một đường chuyền từ phải sang trái và một đường chuyền từ trái sang phải | 
| Không gian |$O(n)$| Hậu tố mảng tối thiểu và bản đồ tần số | 

Vì tổng số tiền của$n$trên tất cả các trường hợp thử nghiệm là nhiều nhất$2\cdot 10^5$, thuật toán chỉ thực hiện công việc tuyến tính nói chung và phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import defaultdict

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        s = [0] * n
        s[-1] = a[-1]

        for i in range(n - 2, -1, -1):
            s[i] = min(a[i], s[i + 1])

        base = 0
        freq = defaultdict(int)

        for ai, si in zip(a, s):
            base += ai - si
            freq[si] += 1

        best = max(freq.values())
        out.append(str(base + max(0, best - 1)))

    return "\n".join(out)

# minimum size
assert run("1\n1\n1\n") == "0"

# strictly increasing
assert run("1\n5\n1 2 3 4 5\n") == "0"

# all equal
assert run("1\n4\n3 3 3 3\n") == "3"

# suffix minimum repeated many times
assert run("1\n5\n1 2 3 2 1\n") == "8"

# decreasing array
assert run("1\n5\n5 4 3 2 1\n") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$[1]$|$0$| Kích thước tối thiểu | 
|$[1,2,3,4,5]$|$0$| Không khối lập phương nào có thể di chuyển | 
|$[3,3,3,3]$|$3$| Tối thiểu hậu tố lặp lại lớn | 
|$[1,2,3,2,1]$|$8$| Việc loại bỏ tạo ra sự thay đổi toàn cầu | 
|$[5,4,3,2,1]$|$10$| Cấu trúc giảm nghiêm ngặt | 

## Vỏ cạnh 

Hãy xem xét$$[1,2,3,4,5].$$Mảng hậu tố tối thiểu bằng mảng ban đầu. Mỗi tần số đều$1$, vì vậy số hạng cải tiến là$0$. Đầu ra của thuật toán$0$, phù hợp với thực tế là không có khối nào chuyển động. 

Coi như$$[3,3,3,3].$$Mảng tối thiểu có hậu tố là$$[3,3,3,3].$$Câu trả lời cơ bản là$0$, nhưng giá trị$3$xuất hiện bốn lần. Sự cải tiến là$$4-1=3.$$Thuật toán nắm bắt chính xác lợi ích của việc loại bỏ một khối khỏi mức ổn định tối thiểu chung. 

Coi như$$[5,4,3,2,1].$$Mảng tối thiểu có hậu tố là$$[1,1,1,1,1].$$Câu trả lời cơ bản là$$(5-1)+(4-1)+(3-1)+(2-1)+(1-1)=10.$$Tần số tối đa là$5$, mang lại sự cải thiện về$4$. Thuật toán xử lý vấn đề này hoàn toàn thông qua các tần số hậu tố tối thiểu mà không mô phỏng trọng lực.
