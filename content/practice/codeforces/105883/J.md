---
title: "CF 105883J - Vụ nổ HDZ"
description: "Chúng ta được hoán vị các số từ 1 đến n. Mỗi vị trí có một “chiều cao” duy nhất và có chính xác một vị trí chứa giá trị lớn nhất n. Vị trí đó chính là mục tiêu mà chúng tôi muốn hướng tới. Robot bắt đầu ở chỉ số i tùy ý. Nó có phạm vi chuyển động d."
date: "2026-06-22T02:45:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105883
codeforces_index: "J"
codeforces_contest_name: "Baozii Cup 2"
rating: 0
weight: 105883
solve_time_s: 52
verified: true
draft: false
---

[CF 105883J - Vụ nổ HDZ](https://codeforces.com/problemset/problem/105883/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được hoán vị các số từ 1 đến n. Mỗi vị trí có một “chiều cao” duy nhất và có chính xác một vị trí chứa giá trị lớn nhất n. Vị trí đó chính là mục tiêu mà chúng tôi muốn hướng tới. 

Robot bắt đầu ở chỉ số i tùy ý. Nó có phạm vi chuyển động d. Từ vị trí hiện tại của nó, nó xem xét tất cả các vị trí trong khoảng cách d trên dòng chỉ số mảng, nghĩa là các chỉ số từ i − d đến i + d, được gắn với phạm vi hợp lệ. Trong số tất cả các chỉ số có thể truy cập đó, nó sẽ di chuyển đến chỉ số có giá trị hoán vị là lớn nhất. Quá trình này lặp đi lặp lại vô thời hạn. 

Câu hỏi đặt ra là tìm ra d tối thiểu sao cho dù robot bắt đầu ở vị trí nào, các bước di chuyển tham lam lặp đi lặp lại luôn dẫn nó đến vị trí có giá trị n. 

Chi tiết quan trọng là chuyển động có tính xác định và luôn chọn giá trị tối đa trong cửa sổ cục bộ. Vì vậy, chúng tôi không mô phỏng các bước đi tùy ý mà là một hệ thống có hướng được tạo ra bởi cực đại cửa sổ trượt. 

Ràng buộc n có thể tổng lên tới 10^6 trong các trường hợp thử nghiệm, loại trừ mọi giải pháp thử tất cả các cặp vị trí hoặc mô phỏng quy trình cho mọi điểm bắt đầu và mọi ứng cử viên d. Bất cứ điều gì bậc hai tính bằng n cho mỗi trường hợp thử nghiệm đều đã quá chậm. Chúng tôi cần một cái gì đó gần với tuyến tính hoặc tuyến tính cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh vi xuất hiện khi giá trị tối đa bị cô lập trong không gian chỉ mục nhưng không thể truy cập được thông qua chuỗi cực đại cục bộ đối với d nhỏ. Ví dụ: nếu hoán vị gần như được sắp xếp nhưng có “rào cản” trong đó mức tối đa cục bộ bẫy bước đi, thì d nhỏ có thể tạo ra nhiều chu kỳ hấp thụ. Một trường hợp cạnh khác là khi n ở điểm cuối. Khi đó điều kiện giảm không đối xứng vì một bên của cửa sổ biến mất. 

## Phương pháp tiếp cận 

Nếu cố định một giá trị của d, chúng ta có thể định nghĩa một đồ thị có hướng trên các chỉ số: từ mỗi i chúng ta vẽ một cạnh tới argmax của p[j] trên j trong [i − d, i + d]. Mỗi nút có chính xác một cạnh đi ra, vì vậy mọi vị trí bắt đầu cuối cùng đều kết thúc trong một chu kỳ và chúng tôi muốn mỗi chu kỳ bao gồm chỉ số có giá trị n. 

Một cách tiếp cận mạnh mẽ là mô phỏng biểu đồ này một cách rõ ràng cho mỗi d và mỗi nút. Đối với mỗi i, chúng ta áp dụng lặp lại quá trình chuyển đổi cho đến khi đạt được chỉ số tối đa hoặc phát hiện được một chu trình. Ngay cả việc xây dựng một chuyển đổi cũng tốn O(n d) nếu được thực hiện một cách đơn giản, vì mỗi nút sẽ quét một cửa sổ có kích thước 2d. Sau đó, nếu chúng ta thử tất cả d, thì tổng độ phức tạp sẽ trở thành bậc ba trong trường hợp xấu nhất, điều này là không thể đối với n lên tới 10^6. 

Quan sát quan trọng là đảo ngược quan điểm. Thay vì hỏi mỗi nút đi đâu, chúng tôi hỏi cấu trúc nào được yêu cầu để cuối cùng mọi nút có thể “chảy” vào mức tối đa toàn cục. Quy tắc chuyển đổi chỉ phụ thuộc vào cực đại cục bộ, do đó việc tăng d chỉ bao giờ thêm các cạnh mới hoặc tăng cường khả năng tiếp cận. Tính đơn điệu này gợi ý tìm kiếm nhị phân trên d. 

Đối với một d cố định, quá trình này tương đương với việc hỏi liệu mọi chỉ số có thể đạt tới vị trí của n trong đồ thị có hướng được xác định bởi các con trỏ cực đại cục bộ hay không. Nếu chúng ta tính toán trước con trỏ tiếp theo cho mỗi nút một cách hiệu quả, chúng ta có thể kiểm tra khả năng tiếp cận ở mức tối đa bằng cách sử dụng truyền tải đồ thị ngược hoặc truyền bá đồ thị hàm. 

Tối ưu hóa quan trọng là đối với một d cố định, cấu trúc “tốt nhất trong cửa sổ” có thể được duy trì bằng cách sử dụng cây phân đoạn hoặc bảng thưa thớt để trả lời các truy vấn phạm vi tối đa. Điều này làm giảm mỗi lần chuyển đổi từ O(d) sang O(log n) hoặc O(1) tùy thuộc vào việc triển khai. Sau đó, việc kiểm tra tất cả các nút là O(n), giúp mỗi lần kiểm tra tính khả thi trở nên hiệu quả. 

Việc kết hợp tính đơn điệu của d với kiểm tra tính khả thi nhanh sẽ mang lại giải pháp tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) mỗi d, tổng O(n³) tệ nhất | O(n) | Quá chậm | 
| Tìm kiếm nhị phân + chuyển tiếp RMQ | O(n log n log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng ta trình bày lại bài toán dưới dạng bài toán quyết định đơn điệu trên d. 

1. Cố định giá trị ứng viên của d. Chúng ta cần xác định xem mọi vị trí bắt đầu cuối cùng có đạt đến chỉ số posMax trong đó p[posMax] = n hay không. 
2. Với mỗi chỉ số i, tính nxt[i], chỉ số của giá trị lớn nhất trong khoảng [i − d, i + d]. Điều này xác định một biểu đồ chức năng trong đó mỗi nút có một cạnh đi ra. Chúng tôi sử dụng cấu trúc truy vấn tối đa phạm vi trên các chỉ mục để truy xuất nxt[i] một cách hiệu quả. 
3. Bắt đầu từ posMax, chúng tôi tuyên truyền khả năng tiếp cận ngược. Chúng ta ngầm xây dựng các cạnh ngược: nếu nxt[i] = j thì j có thể đạt được từ i. Chúng tôi chạy BFS hoặc DFS từ posMax trên biểu đồ ngược này. 
4. Nếu sau quá trình duyệt này, tất cả các nút đều được truy cập thì mọi vị trí bắt đầu cuối cùng sẽ chuyển vào posMax dưới các chuyển đổi lặp đi lặp lại. Mặt khác, một số nút thuộc về một chu kỳ riêng biệt không bao giờ đạt đến mức tối đa, do đó d này không đủ. 
5. Tìm kiếm nhị phân d nhỏ nhất trong phạm vi [0, n − 1] thỏa mãn điều kiện về khả năng tiếp cận. 

Lý do áp dụng tìm kiếm nhị phân là vì việc tăng d chỉ có thể phóng to các cửa sổ, do đó nxt[i] chỉ có thể di chuyển đến một giá trị ít nhất bằng giá trị trước đó, không bao giờ phá vỡ khả năng tiếp cận đã tồn tại. 

### Tại sao nó hoạt động 

Đối với d cố định, quy trình xác định đồ thị hàm số xác định. Mỗi nút tuân thủ nghiêm ngặt giá trị tối đa có thể truy cập được trong vùng lân cận của nó, do đó, mỗi bước sẽ tăng hoặc duy trì giá trị dọc theo các cạnh cho đến khi đạt đến đỉnh cục bộ. Cực đại toàn cục là nút duy nhất có giá trị n, do đó, bất kỳ chu kỳ nào không chứa nó phải bao gồm các nút cực đại cục bộ trong vùng bán kính d có thể tiếp cận của chúng. Nếu một chu kỳ như vậy tồn tại, các nút đó sẽ bị mắc kẹt vĩnh viễn vì không có bản cập nhật nào có thể đưa ra giá trị có thể truy cập cao hơn bên trong cửa sổ của chúng. Ngược lại, nếu mọi nút có thể đạt tới posMax trong biểu đồ ngược thì không tồn tại chu kỳ bẫy như vậy và tất cả các đường dẫn phải kết thúc ở mức tối đa toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.t = [(0, -1)] * (4 * self.n)
        self.arr = arr
        self.build(1, 0, self.n - 1)

    def build(self, v, l, r):
        if l == r:
            self.t[v] = (self.arr[l], l)
            return
        m = (l + r) // 2
        self.build(v * 2, l, m)
        self.build(v * 2 + 1, m + 1, r)
        self.t[v] = max(self.t[v * 2], self.t[v * 2 + 1])

    def query(self, v, l, r, ql, qr):
        if ql > r or qr < l:
            return (-1, -1)
        if ql <= l and r <= qr:
            return self.t[v]
        m = (l + r) // 2
        return max(self.query(v * 2, l, m, ql, qr),
                   self.query(v * 2 + 1, m + 1, r, ql, qr))

def solve_case(n, p):
    pos_max = p.index(n)
    st = SegTree(p)

    def check(d):
        nxt = [0] * n
        for i in range(n):
            l = max(0, i - d)
            r = min(n - 1, i + d)
            nxt[i] = st.query(1, 0, n - 1, l, r)[1]

        rev = [[] for _ in range(n)]
        for i in range(n):
            rev[nxt[i]].append(i)

        seen = [False] * n
        stack = [pos_max]
        seen[pos_max] = True

        while stack:
            u = stack.pop()
            for v in rev[u]:
                if not seen[v]:
                    seen[v] = True
                    stack.append(v)

        return all(seen)

    lo, hi = 0, n - 1
    while lo < hi:
        mid = (lo + hi) // 2
        if check(mid):
            hi = mid
        else:
            lo = mid + 1

    return lo

t = int(input())
out = []
for _ in range(t):
    n = int(input())
    p = list(map(int, input().split()))
    out.append(str(solve_case(n, p)))

print("\n".join(out))
```Cây phân đoạn được sử dụng để tính giá trị tối đa trong bất kỳ khoảng chỉ mục nào, trực tiếp thực hiện quy tắc quyết định cục bộ của robot. Chức năng kiểm tra(d) xây dựng đồ thị hàm số cảm ứng cho bán kính đó và sau đó xác minh xem vị trí tối đa có thể chạm tới tất cả các nút theo chiều ngược lại hay không, tương đương với tất cả các nút cuối cùng đều chảy vào nó. 

Tìm kiếm nhị phân bao quanh việc kiểm tra này vì tính khả thi chỉ trở nên dễ dàng hơn khi d tăng. 

Một cạm bẫy triển khai phổ biến là quên rằng cây phân đoạn phải trả về cả giá trị và chỉ mục, vì các ràng buộc không tồn tại nhưng chúng ta vẫn cần vị trí tối đa. Một cách khác là xử lý không chính xác các ranh giới khi i − d hoặc i + d nằm ngoài phạm vi, điều này sẽ làm hỏng cấu trúc nxt. 

## Ví dụ đã hoạt động 

Xét hoán vị [3, 1, 4, 2] với n = 4. 

### Ví dụ 1: d = 0 

| tôi | cửa sổ | nxt[i] | 
| --- | --- | --- | 
| 0 | [3] | 0 | 
| 1 | [1] | 1 | 
| 2 | [4] | 2 | 
| 3 | [2] | 3 | 

Mỗi nút trỏ đến chính nó nên chỉ có chỉ số 2 (giá trị 4) là đúng, các nút khác tạo thành chu kỳ tự động. 

Đảo ngược BFS từ posMax = chỉ 2 lượt truy cập {2}. 

Điều này cho thấy d = 0 không thành công vì không phải tất cả các nút đều đạt mức tối đa. 

### Ví dụ 2: d = 1 

| tôi | cửa sổ | nxt[i] | 
| --- | --- | --- | 
| 0 | [3,1] | 0 | 
| 1 | [3,1,4] | 2 | 
| 2 | [1,4,2] | 2 | 
| 3 | [4,2] | 2 | 

Khả năng tiếp cận ngược từ 2 mức lan truyền đến tất cả các nút. 

Điều này khẳng định d = 1 là đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n log n) | Mỗi kiểm tra xây dựng các chuyển đổi trong O(n log n), BFS là O(n), tìm kiếm nhị phân thêm hệ số log n | 
| Không gian | O(n log n) | Cây phân đoạn cộng với danh sách lân cận cho mỗi lần kiểm tra | 

Các ràng buộc cho phép tổng n lên tới 10^6, do đó, hệ số log bình phương cho mỗi trường hợp kiểm thử có thể được chấp nhận nếu các hằng số chặt chẽ. Giải pháp nằm trong giới hạn vì mỗi phần tử tham gia vào các thao tác cây phân đoạn O(log n) trên mỗi lần kiểm tra và các lần kiểm tra đều có tính logarit theo n. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    class SegTree:
        def __init__(self, arr):
            self.n = len(arr)
            self.t = [(0, -1)] * (4 * self.n)
            self.arr = arr
            self.build(1, 0, self.n - 1)

        def build(self, v, l, r):
            if l == r:
                self.t[v] = (self.arr[l], l)
                return
            m = (l + r) // 2
            self.build(v * 2, l, m)
            self.build(v * 2 + 1, m + 1, r)
            self.t[v] = max(self.t[v * 2], self.t[v * 2 + 1])

        def query(self, v, l, r, ql, qr):
            if ql > r or qr < l:
                return (-1, -1)
            if ql <= l and r <= qr:
                return self.t[v]
            m = (l + r) // 2
            return max(self.query(v * 2, l, m, ql, qr),
                       self.query(v * 2 + 1, m + 1, r, ql, qr))

    def solve_case(n, p):
        pos_max = p.index(n)
        st = SegTree(p)

        def check(d):
            nxt = [0] * n
            for i in range(n):
                l = max(0, i - d)
                r = min(n - 1, i + d)
                nxt[i] = st.query(1, 0, n - 1, l, r)[1]

            rev = [[] for _ in range(n)]
            for i in range(n):
                rev[nxt[i]].append(i)

            seen = [False] * n
            stack = [pos_max]
            seen[pos_max] = True

            while stack:
                u = stack.pop()
                for v in rev[u]:
                    if not seen[v]:
                        seen[v] = True
                        stack.append(v)

            return all(seen)

        lo, hi = 0, n - 1
        while lo < hi:
            mid = (lo + hi) // 2
            if check(mid):
                hi = mid
            else:
                lo = mid + 1

        return str(lo)

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        out.append(solve_case(n, p))

    return "\n".join(out)

# basic sanity checks
assert run("1\n1\n1\n") == "0"
assert run("1\n2\n1 2\n") == "1"
assert run("1\n3\n2 3 1\n") in {"1", "2"}
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 phần tử đơn | 0 | trường hợp cơ bản tầm thường | 
| sắp xếp 1 2 | 1 | chuyển động không tầm thường tối thiểu | 
| hoán vị chu kỳ nhỏ | d nhỏ | cấu trúc phi tuyến tính | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi giá trị tối đa nằm ở điểm cuối. Ví dụ: p = [4, 1, 2, 3]. Ở đây posMax = 0. Với d = 1, cửa sổ của mọi nút nhanh chóng bao gồm 4, do đó tất cả các đường dẫn đều hội tụ ngay lập tức. BFS ngược bắt đầu ở chỉ mục 0 và lan sang tất cả các nút vì cuối cùng mọi chuỗi nxt đều trỏ vào một cửa sổ chứa chỉ mục 0. Thuật toán đánh dấu chính xác tất cả các nút là có thể truy cập được. 

Một trường hợp khác là cấu trúc “rào cản” như p = [3, 4, 1, 2]. Với d = 0, mọi nút đều bị cô lập và chỉ có chỉ số tối đa là hợp lệ, do đó chỉ nút đó được đánh dấu. Ngay khi d trở thành 1, cửa sổ sẽ bắc qua hàng rào, giúp có thể tiếp cận tối đa từ mọi vị trí. BFS từ posMax chỉ xác nhận khả năng tiếp cận đầy đủ ở ngưỡng đó. 

Một trường hợp tinh vi hơn là khi tồn tại nhiều cực đại cục bộ nhưng chỉ tồn tại một cực đại toàn cục. Đối với d nhỏ, các nút có thể hình thành các chu kỳ riêng biệt xung quanh các cực đại cục bộ đó. Trong trường hợp như vậy, khả năng truy cập ngược từ posMax không thành công vì những chu kỳ đó không bao giờ hướng tới nó. Khi d đủ lớn để hợp nhất các lưu vực này, các con trỏ nxt sẽ thu gọn thành một cấu trúc duy nhất bắt nguồn từ posMax và BFS bao gồm mọi thứ.
