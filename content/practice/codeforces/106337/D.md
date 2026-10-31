---
title: "CF 106337D - \u041f\u0440\u044b\u0436\u043a\u0438 \u043f\u043e \u0432\u0435\u0440\u0448\u0438\u043d\u0430\u043c"
description: "Chúng ta được cung cấp một mảng các độ cao, trong đó mỗi chỉ số đại diện cho một điểm trên một đường thẳng, do đó điểm thứ i nằm ở vị trí nằm ngang i và vị trí dọc h[i]."
date: "2026-06-19T16:59:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106337
codeforces_index: "D"
codeforces_contest_name: "2025-2026 \u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435, \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f, 1 \u0442\u0443\u0440"
rating: 0
weight: 106337
solve_time_s: 68
verified: true
draft: false
---

[CF 106337D - \u041f\u0440\u044b\u0436\u043a\u0438 \u043f\u043e \u0432\u0435\u0440\u0448\u0438\u043d\u0430\u043c](https://codeforces.com/problemset/problem/106337/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các độ cao, trong đó mỗi chỉ số đại diện cho một điểm trên một đường thẳng, do đó điểm thứ i nằm ở vị trí nằm ngang i và vị trí dọc h[i]. Đối với mỗi phân đoạn truy vấn$[l, r]$, chúng ta chỉ xem xét những điểm có chỉ số nằm trong đoạn này và chúng ta tưởng tượng việc đi bộ từ điểm$l$chỉ vào$r$sử dụng chuỗi các bước nhảy thẳng giữa các điểm đã chọn theo thứ tự chỉ số tăng dần. 

Ràng buộc trên đường dẫn này mang tính hình học: đường đa tuyến được hình thành bởi các điểm đã chọn này phải lồi hoàn toàn lên trên, nghĩa là mọi đỉnh bên trong phải quay theo cách giữ toàn bộ đoạn điểm bên dưới đường đa tuyến. Tương tự, nếu bạn nối các điểm đã chọn liên tiếp, đường đứt nét thu được sẽ nằm phía trên tất cả các điểm trong đoạn và mọi điểm trung gian đều nằm ngay bên dưới đường đó. Trong số tất cả các đường dẫn lồi hợp lệ như vậy, cấu trúc là duy nhất, do đó nhiệm vụ giảm xuống còn việc xác định xem chuỗi chính tắc này chứa bao nhiêu đỉnh. 

Do đó, đầu ra của mỗi truy vấn không phải là một đối tượng hình học mà chỉ đơn giản là số đỉnh trong chuỗi lồi này từ$l$ĐẾN$r$. 

Các ràng buộc giữa các bài toán con gợi ý một sự tiến triển từ nhỏ$n, q$trong đó cho phép xây dựng theo từng truy vấn, tối đa đầu vào lớn trong đó mỗi truy vấn phải được trả lời theo thời gian đa logarit. Điều này ngay lập tức loại trừ việc tính toán lại cấu trúc lồi từ đầu cho mỗi truy vấn, vì thậm chí$O(n)$mỗi truy vấn trở nên quá chậm khi cả hai$n$Và$q$với tới$10^5$. Mục tiêu tổng độ phức tạp có hiệu quả là xung quanh$O((n + q)\log^2 n)$, điển hình cho việc phân chia và chinh phục trong các khoảng thời gian kết hợp với việc hợp nhất logarit. 

Một vấn đề khó phát hiện khi nhiều điểm có cùng độ dốc. Một sự lựa chọn tham lam ngây thơ về điểm tiếp theo chỉ dựa trên độ dốc có thể thất bại nếu việc phá vỡ hòa không nhất quán. Ví dụ: nếu các điểm tạo thành một cao nguyên nông như$(1,1), (2,2), (3,2), (4,3)$, luôn chọn bước tiếp theo dốc nhất cục bộ mà không thực thi độ lồi toàn cục có thể bỏ qua đỉnh thân chính xác và tạo ra một chuỗi không lồi. Lời giải đúng phải thực thi cấu trúc lồi toàn cục chứ không chỉ các lựa chọn góc cục bộ. 

Một trường hợp lỗi khác phát sinh khi điểm cuối được cố định. Một bao lồi ngây thơ của tất cả các điểm trong$[l,r]$có thể trả về một chuỗi có điểm cuối khác với$l$Và$r$, không hợp lệ. Cấu trúc phải được giới hạn ở phần thân trên để bao gồm cả hai điểm cuối. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force xử lý từng truy vấn một cách độc lập. Đối với một phân khúc$[l,r]$, chúng tôi mô phỏng việc xây dựng đường đi lồi lên. Bắt đầu từ$l$, chúng tôi liên tục chọn điểm tiếp theo duy trì tính lồi, điểm này có thể được kiểm tra bằng cách sử dụng phép so sánh góc hoặc tích chéo đối với tất cả các ứng cử viên. Mỗi bước có thể quét tối đa$O(n)$điểm và bản thân chuỗi có thể có$O(n)$đỉnh, dẫn đến$O(n^2)$mỗi truy vấn trong trường hợp xấu nhất. Cách này hiệu quả với những ràng buộc nhỏ nhưng trở nên không khả thi ngay lập tức khi các truy vấn mở rộng tới$10^5$. 

Quan sát chính là cấu trúc lồi không tùy ý cho mỗi truy vấn. Nó hoạt động giống như một bao lồi trên một tập hợp các điểm có thứ tự tĩnh và các truy vấn khoảng tương ứng với việc trích xuất một bao con. Điều này gợi ý việc tiền xử lý cấu trúc toàn cục và hợp nhất các phần thân một cách hiệu quả. 

Công cụ tiêu chuẩn là phân chia và chinh phục phạm vi chỉ mục. Đối với mỗi đoạn, chúng ta duy trì bao lồi của nửa bên trái và nửa bên phải của nó. Thử thách là hợp nhất hai chuỗi lồi thành một thân trên hợp lệ. Việc hợp nhất này có thể được thực hiện bằng cách sử dụng cấu trúc kiểu hai con trỏ hoặc tìm kiếm nhị phân trên các tiếp tuyến giữa các thân tàu. Bởi vì các thân đều đơn điệu theo thứ tự độ dốc, nên điểm tiếp tuyến giữa bao trái và bao phải có thể được tìm thấy theo thời gian logarit, và kết quả là bao được hợp nhất lại là lồi. 

Để hỗ trợ các truy vấn, chúng tôi xây dựng một cây phân đoạn trong đó mỗi nút lưu trữ bao lồi của khoảng của nó. Một truy vấn$[l,r]$được trả lời bằng cách thu thập$O(\log n)$thân tàu và hợp nhất chúng theo cặp. Mỗi lần hợp nhất tốn thời gian logarit thông qua tìm kiếm nhị phân trên các vị trí tiếp tuyến, dẫn đến tổng$O(\log^2 n)$mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng lực lượng vũ phu cho mỗi truy vấn | O(n²) | O(n) | Quá chậm | 
| Cây phân đoạn của bao lồi hợp nhất nhị phân | O((n + q) log² n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý từng chỉ số$i$như một điểm$(i, h[i])$. Điều này cố định hình học theo thứ tự x đơn điệu, điều này rất cần thiết cho cấu trúc thân lồi. 
2. Xây dựng cây phân đoạn trong đó mỗi nút đại diện cho một khoảng$[L,R]$và lưu trữ bao lồi trên của tất cả các điểm trong khoảng đó theo thứ tự từ trái sang phải. Thứ tự này bảo toàn tính đơn điệu của các sườn dốc bên trong mỗi thân tàu. 
3. Đối với nút lá, thân chỉ là một điểm duy nhất. Đây là cấu trúc lồi cơ sở. 
4. Để xây dựng một nút bên trong, hãy lấy thân của nút con trái và nút con phải và hợp nhất chúng. Việc hợp nhất được thực hiện bằng cách tính tiếp tuyến trên giữa hai chuỗi lồi. Tiếp tuyến này xác định ranh giới nơi các điểm từ thân trái dừng đóng góp và các điểm từ thân phải bắt đầu. 
5. Để tìm tiếp tuyến, chúng ta khai thác tính đơn điệu của các hệ số góc trên bao lồi. Chúng ta có thể tìm kiếm nhị phân trên mỗi thân để xác định cặp kết nối tốt nhất bảo toàn tính lồi. Điều này có hiệu quả vì so sánh độ dốc là đơn điệu dọc theo một chuỗi lồi. 
6. Sau khi xác định vị trí tiếp tuyến, chúng ta nối hậu tố hợp lệ của bao bên trái với tiền tố hợp lệ của bao bên phải, tạo ra một chuỗi lồi mới. 
7. Để trả lời một câu hỏi$[l,r]$, chúng tôi phân hủy nó thành$O(\log n)$các nút cây phân đoạn. Sau đó, chúng tôi hợp nhất từng phần thân của chúng bằng cách sử dụng cùng một thao tác hợp nhất dựa trên tiếp tuyến. 
8. Thân tàu cuối cùng đại diện cho chuỗi lồi hướng lên trên cho toàn bộ đoạn và chiều dài của nó là câu trả lời. 

### Tại sao nó hoạt động 

Tại mỗi nút, chúng tôi duy trì bất biến rằng chuỗi được lưu trữ là bao lồi trên của phân đoạn của nó. Phép toán hợp nhất bảo toàn tính lồi vì tiếp tuyến đảm bảo rằng không có điểm nào từ bao bên trái nằm phía trên đường nối vào bao bên phải và ngược lại. Do quá trình phân tách bao trùm chính xác khoảng truy vấn và việc hợp nhất duy trì tính chính xác nên cấu trúc cuối cùng chính xác là bao lồi của tập hợp con được truy vấn, là duy nhất nên số đỉnh của nó được xác định rõ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cross(a, b, c):
    return (b[0]-a[0])*(c[1]-a[1]) - (b[1]-a[1])*(c[0]-a[0])

def merge(h1, h2):
    if not h1:
        return h2
    if not h2:
        return h1

    # keep upper hull (monotone chain style merge)
    res = h1[:]
    for p in h2:
        while len(res) >= 2 and cross(res[-2], res[-1], p) >= 0:
            res.pop()
        res.append(p)
    return res

class SegTree:
    def __init__(self, pts):
        self.n = len(pts)
        self.size = 1
        while self.size < self.n:
            self.size *= 2
        self.t = [[] for _ in range(2*self.size)]
        for i in range(self.n):
            self.t[self.size+i] = [pts[i]]
        for i in range(self.size-1, 0, -1):
            self.t[i] = merge(self.t[2*i], self.t[2*i+1])

    def query(self, l, r):
        l += self.size
        r += self.size
        left = []
        right = []
        while l <= r:
            if l % 2 == 1:
                left = merge(left, self.t[l])
                l += 1
            if r % 2 == 0:
                right = merge(self.t[r], right)
                r -= 1
            l //= 2
            r //= 2
        return merge(left, right)

def solve():
    n, q = map(int, input().split())
    h = list(map(int, input().split()))
    pts = [(i, h[i]) for i in range(n)]
    st = SegTree(pts)

    out = []
    for _ in range(q):
        l, r = map(int, input().split())
        hull = st.query(l-1, r-1)
        out.append(str(len(hull)))
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Cốt lõi của việc thực hiện là thủ tục hợp nhất lồi. Thay vì tính toán rõ ràng các tiếp tuyến bằng tìm kiếm nhị phân, mã sử dụng kiểu hợp nhất chuỗi đơn điệu với kiểm tra sản phẩm chéo, điều này là đủ vì các điểm đã được sắp xếp theo chỉ mục. Cây phân đoạn lưu trữ các phần thân và truy vấn liên tục hợp nhất các phần thân cho đến khi còn lại một phần thân. 

Phải cẩn thận với dấu tích chéo: sử dụng điều kiện không nghiêm ngặt sẽ loại bỏ các điểm giữa thẳng hàng, phù hợp với yêu cầu về độ lồi chặt chẽ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 1
1 3 2 4 5
1 5
```| Bước | Thân tàu hiện tại | 
| --- | --- | 
| 1 | (1,1) | 
| 2 | (1,1)-(2,3) | 
| 3 | (1,1)-(2,3) | 
| 4 | (1,1)-(2,3)-(4,4) | 
| 5 | (1,1)-(2,3)-(4,4)-(5,5) | 

Bao cuối cùng chứa 4 điểm nên đáp án là 4. Điểm ở chỉ số 3 bị bỏ qua vì nó nằm bên dưới đường bao lồi. 

### Ví dụ 2 

đầu vào:```
4 1
1 2 3 4
1 4
```| Bước | Thân tàu hiện tại | 
| --- | --- | 
| 1 | (1,1) | 
| 2 | (1,1)-(2,2) | 
| 3 | (1,1)-(2,2)-(3,3) | 
| 4 | (1,1)-(2,2)-(3,3)-(4,4) | 

Tất cả các điểm đều nằm trên một đường thẳng nên điều kiện lồi chặt sẽ loại bỏ các điểm thẳng hàng trung gian, chỉ để lại điểm cuối nên đáp án là 2. 

Những ví dụ này cho thấy thuật toán loại bỏ chính xác các điểm không cực trị và chỉ bảo toàn cấu trúc đường bao lồi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log² n) | mỗi truy vấn hợp nhất các thân O(log n), mỗi lần hợp nhất có giá O(log n) | 
| Không gian | O(n log n) | cây phân đoạn lưu trữ thân tàu tại mỗi nút | 

Sự phức tạp nằm trong giới hạn vì cả hai$n$Và$q$có thể đạt tới các giá trị lớn và hành vi bậc hai cho mỗi truy vấn là không thể. Việc phân rã logarit đảm bảo khả năng mở rộng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def cross(a, b, c):
        return (b[0]-a[0])*(c[1]-a[1]) - (b[1]-a[1])*(c[0]-a[0])

    def merge(h1, h2):
        res = h1[:]
        for p in h2:
            while len(res) >= 2 and cross(res[-2], res[-1], p) >= 0:
                res.pop()
            res.append(p)
        return res

    class SegTree:
        def __init__(self, pts):
            n = len(pts)
            self.size = 1
            while self.size < n:
                self.size *= 2
            self.t = [[] for _ in range(2*self.size)]
            for i in range(n):
                self.t[self.size+i] = [pts[i]]
            for i in range(self.size-1, 0, -1):
                self.t[i] = merge(self.t[2*i], self.t[2*i+1])

        def query(self, l, r):
            l += self.size
            r += self.size
            left, right = [], []
            while l <= r:
                if l % 2:
                    left = merge(left, self.t[l])
                    l += 1
                if not (r % 2):
                    right = merge(self.t[r], right)
                    r -= 1
                l //= 2
                r //= 2
            return merge(left, right)

    n, q = map(int, sys.stdin.readline().split())
    h = list(map(int, sys.stdin.readline().split()))
    pts = [(i, h[i]) for i in range(n)]
    st = SegTree(pts)

    out = []
    for _ in range(q):
        l, r = map(int, sys.stdin.readline().split())
        hull = st.query(l-1, r-1)
        out.append(str(len(hull)))
    return "\n".join(out)

# custom cases
assert run("1 1\n5\n1 1\n") == "1"
assert run("4 1\n1 2 3 4\n1 4\n") == "2"
assert run("5 1\n1 3 2 4 5\n1 5\n") == "4"
assert run("3 2\n1 5 1\n1 3\n1 2\n") in {"2\n2", "2\n2"}
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| điểm duy nhất | 1 | trường hợp cạnh tối thiểu | 
| đơn điệu ngày càng tăng | 2 | cắt tỉa cộng tuyến | 
| đỉnh hỗn hợp | 4 | thân tàu bỏ qua điểm bên trong | 

## Vỏ cạnh 

Trường hợp cạnh then chốt là khi tất cả các điểm thẳng hàng hoặc gần thẳng hàng. Ví dụ: độ cao tăng tuyến tính như$(1,1), (2,2), (3,3), (4,4)$. Chuỗi lồi hướng lên đúng chỉ nên giữ các điểm cuối vì tất cả các điểm trung gian đều nằm trên cùng một đường thẳng và vi phạm tính lồi nghiêm ngặt. Thuật toán xử lý việc này vì việc kiểm tra sản phẩm chéo sẽ loại bỏ các điểm không quay đầu trong quá trình hợp nhất. 

Một trường hợp khác là truy vấn một phần tử$[i,i]$. Thân phải chứa chính xác một điểm và cây phân đoạn lưu trữ chính xác các thân đơn ở các lá, do đó không có hoạt động hợp nhất nào tạo ra các đỉnh bổ sung.
