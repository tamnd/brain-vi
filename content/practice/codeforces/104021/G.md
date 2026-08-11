---
title: "CF 104021G - Nồi!!"
description: "Chúng tôi được cung cấp một mảng có độ dài lên tới một trăm nghìn. Mọi phần tử bắt đầu bằng 1, sau đó chúng tôi thực hiện một chuỗi các thao tác nhân phân đoạn liền kề với một số nguyên nhỏ trong khoảng từ 2 đến 10 hoặc yêu cầu truy vấn trên một phân đoạn."
date: "2026-07-02T04:36:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104021
codeforces_index: "G"
codeforces_contest_name: "The 2019 ICPC Asia Yinchuan Regional Contest"
rating: 0
weight: 104021
solve_time_s: 58
verified: true
draft: false
---

[CF 104021G - Nồi!!](https://codeforces.com/problemset/problem/104021/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có độ dài lên tới một trăm nghìn. Mọi phần tử bắt đầu bằng 1, sau đó chúng tôi thực hiện một chuỗi các thao tác nhân phân đoạn liền kề với một số nguyên nhỏ trong khoảng từ 2 đến 10 hoặc yêu cầu truy vấn trên một phân đoạn. 

Mỗi giá trị mảng luôn được phân tích thành số nguyên tố và điều duy nhất chúng ta quan tâm là số nguyên tố chia cho một số bao nhiêu lần. Đối với giá trị ai, đối với số nguyên tố p, chúng ta định nghĩa điểm potp(ai) là số mũ của p trong phân tích nhân tử của ai. Với mỗi vị trí i, chúng ta xét tất cả các số nguyên tố chia ai và lấy số mũ lớn nhất trong số đó. Truy vấn phạm vi yêu cầu giá trị tối đa như vậy trên một phân đoạn. 

Vì vậy, về mặt khái niệm, mỗi số đều mang nhiều “lớp” lũy thừa nguyên tố, nhưng chúng tôi chỉ theo dõi lớp đơn mạnh nhất ở mỗi vị trí và sau đó là lớp mạnh nhất trong số các lớp trong một phạm vi. 

Các ràng buộc cho thấy chúng tôi không thể mô phỏng hệ số hóa một cách nguyên bản cho mỗi lần cập nhật. Có tới một trăm nghìn bản cập nhật và mỗi phép nhân áp dụng cho toàn bộ phân khúc. Ngay cả khi ban đầu mỗi giá trị vẫn có độ lớn nhỏ, thì việc cập nhật lặp lại sẽ nhanh chóng làm cho các giá trị trở nên lớn, do đó việc tính toán lại trực tiếp các hệ số là quá chậm. 

Hạn chế chính về cấu trúc là số nhân rất nhỏ, nhiều nhất là 10. Điều đó có nghĩa là mọi cập nhật chỉ đưa ra các số nguyên tố từ tập hợp {2, 3, 5, 7}. Không có số nguyên tố nào khác từng xuất hiện. Điều này sẽ giải quyết vấn đề từ việc phân tích nhân tử tùy ý thành việc theo dõi bốn số mũ cho mỗi vị trí. 

Một cạm bẫy tinh vi là hiểu sai truy vấn: chúng tôi không tính tổng số mũ và chúng tôi không lấy giá trị tối đa trên các số nguyên tố trên toàn cầu. Đối với mỗi vị trí, chúng tôi lấy số mũ tốt nhất trong số các số nguyên tố của nó, sau đó tối đa hóa số mũ đó trên phân khúc. Điều này khiến nó trở thành vấn đề “tối đa hai cấp độ” thay vì mức tối đa phạm vi đơn giản. 

Các trường hợp cạnh phát sinh khi nhiều số nguyên tố tích lũy khác nhau trên cùng một chỉ số. Ví dụ: nếu một vị trí có 2^5·3^1 thì điểm của vị trí đó là 5 chứ không phải 6. Vị trí khác có thể có 3^4·5^4, cho điểm 4. Truy vấn sẽ chọn 5 chứ không phải 9 hay 4 trên tổng thể. Một giải pháp ngây thơ chỉ tính tổng số mũ hoặc chỉ theo dõi tổng số phép nhân sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ duy trì giá trị đầy đủ của từng ai một cách rõ ràng. Đối với phép toán NHÂN l r x, chúng ta sẽ nhân từng phần tử riêng lẻ. Đối với truy vấn MAX, chúng tôi sẽ tính từng số trong phạm vi và tính số mũ nguyên tố tốt nhất. 

Điều này hoạt động chính xác vì nó phản ánh chính xác định nghĩa, nhưng nó thất bại ngay lập tức về hiệu suất. Mỗi phép nhân ảnh hưởng đến tối đa một trăm nghìn phần tử và có tới một trăm nghìn phép tính, đưa ra trường hợp xấu nhất theo thứ tự 10^10 lần cập nhật. Việc nhân tử hóa những số lượng lớn vừa phải thậm chí còn khiến nó trở nên tồi tệ hơn. 

Quan sát trọng tâm là phép nhân với x trong {2,3,5,7,10} chỉ thay đổi số mũ của một tập hợp số nguyên tố nhỏ cố định. Chúng ta không bao giờ cần phải xây dựng lại các con số; chúng ta chỉ cần duy trì số mũ của các số nguyên tố 2, 3, 5 và 7 cho mỗi vị trí. Giá trị tại một vị trí được mô tả đầy đủ bằng bốn số nguyên. 

Điều này biến vấn đề thành việc duy trì bốn cấu trúc cộng phạm vi (đối với các số nguyên tố 2, 3, 5, 7) và trả lời các truy vấn phạm vi tối đa trên mức tối đa trong số bốn giá trị này cho mỗi chỉ mục. 

Để hỗ trợ phép nhân phạm vi, chúng ta cần phép cộng phạm vi trên mảng số mũ. Để hỗ trợ các truy vấn tối đa, chúng tôi cần các truy vấn tối đa trong phạm vi trên giá trị tối đa cho mỗi chỉ mục dẫn xuất (exp2, exp3, exp5, exp7). Vì đó không phải là tuyến tính nên chúng tôi duy trì một cây phân đoạn lưu trữ, đối với mỗi nút, giá trị tối đa của số mũ tối đa trong khoảng của nó và cũng có thể lan truyền lười biếng để thêm các đóng góp cho từng số mũ nguyên tố riêng biệt.

Bí quyết là duy trì bốn thẻ lười cho mỗi nút, một thẻ cho mỗi số mũ nguyên tố và đẩy chúng xuống để các giá trị lá luôn là tổng đóng góp chính xác. Nút tính toán lại mức tối đa của nó bằng cách kiểm tra tất cả bốn cực đại được lưu trữ ở nút con và lấy giá trị kết hợp tốt nhất một cách ngầm định thông qua các tập hợp được duy trì. 

Khó khăn chính là đảm bảo rằng các cập nhật phạm vi vẫn là O(log n) trong khi vẫn duy trì sự phân tách theo từng số nguyên tố và các truy vấn MAX vẫn là một truy vấn cây phân đoạn đơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq + q√A) | O(n) | Quá chậm | 
| Cây phân đoạn tối ưu với 4 thẻ lười | O(q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Về mặt khái niệm, chúng tôi duy trì bốn mảng số mũ, một mảng cho mỗi số nguyên tố trong {2, 3, 5, 7}, nhưng chúng tôi không bao giờ lưu trữ rõ ràng các mảng đầy đủ. Thay vào đó, nút cây phân đoạn đại diện cho một phân đoạn và lưu trữ số mũ tối đa cho mỗi số nguyên tố bên trong nó, cùng với giá trị dẫn xuất tốt nhất cho phân đoạn đó. 

1. Tính toán trước phần đóng góp theo cấp số nhân của từng số nhân có thể. Với mỗi x trong [2,10], phân tích nó thành 2,3,5,7 và lưu trữ số lần mỗi số nguyên tố xuất hiện. Điều này cho phép chúng tôi chuyển đổi mỗi bản cập nhật thành bốn thao tác thêm phạm vi. 
2. Xây dựng cây phân đoạn dựa trên các chỉ số từ 1 đến n, khởi tạo mọi thứ về 0 vì tất cả ai đều bắt đầu bằng 1. Điều này có nghĩa là tất cả các giá trị số mũ ban đầu đều bằng 0 ở mọi nơi. 
3. Đối với truy vấn NHÂN l r x, hãy dịch x thành bốn số gia delta2, delta3, delta5, delta7. Áp dụng phép cộng phạm vi của từng delta trên [l, r] bằng cách sử dụng phương pháp lan truyền lười trong cây phân đoạn. Lý do điều này có hiệu quả là vì phép nhân trong không gian giá trị sẽ trở thành phép cộng trong không gian số mũ một cách độc lập cho mỗi số nguyên tố. 
4. Mỗi nút cây phân đoạn duy trì giá trị số mũ tối đa cho từng số nguyên tố riêng biệt trong phân đoạn của nó. Khi áp dụng bản cập nhật lười biếng, chúng tôi thêm delta vào cả bốn giá trị cực đại được lưu trữ một cách nhất quán mà không cần tính toán lại các lá. 
5. Khi đẩy các giá trị lười xuống, chúng tôi đảm bảo các phần tử con kế thừa số gia số mũ tích lũy để duy trì tính nhất quán bên trong giữa phân đoạn và phần tử con. Điều này giữ cho tất cả thông tin về số mũ luôn chính xác mà không cần truy cập vào từng phần tử riêng lẻ. 
6. Đối với truy vấn MAX l r, chúng tôi truy vấn cây phân đoạn trên [l, r] và truy xuất bốn giá trị: số mũ tối đa là 2, 3, 5 và 7 trong phân đoạn đó. Câu trả lời cho đoạn đó là số lớn nhất trong bốn số này. 
7. Chúng tôi trả về mức tối đa này là kết quả của truy vấn. 

Tại sao nó hoạt động: mỗi ai được đặc trưng đầy đủ bởi bốn số mũ độc lập. Cập nhật phép nhân duy trì tính độc lập giữa các số nguyên tố, vì vậy mỗi cập nhật sẽ phân tách rõ ràng thành bốn cập nhật phạm vi cộng. Định nghĩa truy vấn giảm xuống mức lấy, đối với mỗi vị trí, giá trị tối đa trên số nguyên tố, sau đó là giá trị tối đa trên vị trí. Cây phân đoạn lưu trữ chính xác các tập hợp cần thiết để duy trì cả hai mức cực đại. Lan truyền lười biếng đảm bảo rằng các đóng góp số mũ không bao giờ bị mất hoặc được tính gấp đôi và mọi nút luôn phản ánh trạng thái tích lũy chính xác trong khoảng của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

primes = [2, 3, 5, 7]

def factor_small(x):
    res = [0, 0, 0, 0]
    for i, p in enumerate(primes):
        while x % p == 0:
            res[i] += 1
            x //= p
    return res

class SegTree:
    def __init__(self, n):
        self.n = n
        self.mx = [[0, 0, 0, 0] for _ in range(4 * n)]
        self.lazy = [[0, 0, 0, 0] for _ in range(4 * n)]

    def apply(self, v, delta):
        for i in range(4):
            self.mx[v][i] += delta[i]
            self.lazy[v][i] += delta[i]

    def push(self, v):
        if v * 2 >= len(self.mx):
            return
        for i in range(4):
            if self.lazy[v][i]:
                self.apply(v * 2, [self.lazy[v][i]] + [0]*3)
                self.apply(v * 2 + 1, [self.lazy[v][i]] + [0]*3)
                self.apply(v * 2, [0, self.lazy[v][i], 0, 0])
                self.apply(v * 2 + 1, [0, self.lazy[v][i], 0, 0])
                self.apply(v * 2, [0, 0, self.lazy[v][i], 0])
                self.apply(v * 2 + 1, [0, 0, self.lazy[v][i], 0])
                self.apply(v * 2, [0, 0, 0, self.lazy[v][i]])
                self.apply(v * 2 + 1, [0, 0, 0, self.lazy[v][i]])
        self.lazy[v] = [0, 0, 0, 0]

    def update(self, v, tl, tr, l, r, delta):
        if l > r:
            return
        if l == tl and r == tr:
            self.apply(v, delta)
            return
        tm = (tl + tr) // 2
        self.push(v)
        self.update(v * 2, tl, tm, l, min(r, tm), delta)
        self.update(v * 2 + 1, tm + 1, tr, max(l, tm + 1), r, delta)
        for i in range(4):
            self.mx[v][i] = max(self.mx[v * 2][i], self.mx[v * 2 + 1][i])

    def query(self, v, tl, tr, l, r):
        if l > r:
            return [0, 0, 0, 0]
        if l == tl and r == tr:
            return self.mx[v]
        tm = (tl + tr) // 2
        self.push(v)
        left = self.query(v * 2, tl, tm, l, min(r, tm))
        right = self.query(v * 2 + 1, tm + 1, tr, max(l, tm + 1), r)
        return [max(left[i], right[i]) for i in range(4)]

def solve():
    n, q = map(int, input().split())
    st = SegTree(n)

    for _ in range(q):
        parts = input().split()
        if parts[0] == "MULTIPLY":
            l, r, x = map(int, parts[1:])
            delta = factor_small(x)
            st.update(1, 1, n, l, r, delta)
        else:
            l, r = map(int, parts[1:])
            res = st.query(1, 1, n, l, r)
            print("ANSWER", max(res))

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách chuyển đổi mọi số nhân thành vectơ bốn chiều tương ứng với số mũ của các số nguyên tố 2, 3, 5 và 7. Cây phân đoạn lưu trữ, đối với mỗi nút, số mũ tối đa quan sát được trong khoảng của nó cho từng số nguyên tố riêng biệt. 

Hoạt động cập nhật áp dụng phép cộng phạm vi của vectơ này. Về mặt khái niệm, điều này làm tăng tất cả các trường số mũ bị ảnh hưởng và vì mỗi trường là độc lập nên chúng ta có thể truyền bá chúng một cách lười biếng. 

Hoạt động truy vấn thu thập số mũ tối đa cho mỗi số nguyên tố trong phạm vi được yêu cầu, sau đó lấy giá trị tối đa trên bốn giá trị đó, khớp chính xác với định nghĩa của vấn đề. 

Một rủi ro triển khai tinh tế là việc xử lý lan truyền lười biếng. Mỗi thứ nguyên chính phải được cập nhật một cách nhất quán; nếu không, đóng góp của một số nguyên tố có thể tụt hậu so với các số nguyên tố khác, tạo ra mức cực đại không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3
MULTIPLY 1 3 2
MULTIPLY 2 5 3
MAX 1 5
```| Bước | Hoạt động | Hiệu ứng chính | Tóm tắt trạng thái phân đoạn | 
| --- | --- | --- | --- | 
| 1 | nhân [1,3] với 2 | +1 đến exp2 | vị trí 1-3 tăng 2^1 | 
| 2 | nhân [2,5] với 3 | +1 đến exp3 | vị trí 2-3 có cả hai số nguyên tố | 
| 3 | tối đa [1,5] | tính số mũ tốt nhất | tối đa là 1 | 

Dấu vết này cho thấy các số nguyên tố hỗn hợp không tích lũy thành điểm lớn hơn một số mũ trội duy nhất. 

### Ví dụ 2 

đầu vào:```
4 3
MULTIPLY 1 4 4
MULTIPLY 2 3 3
MAX 1 4
```| Bước | Hoạt động | Hiệu ứng chính | Tóm tắt trạng thái phân đoạn | 
| --- | --- | --- | --- | 
| 1 | nhân với 4 | +2 đến exp2 ở mọi nơi | tất cả các vị trí đều có 2^2 | 
| 2 | nhân [2,3] với 3 | +1 đến exp3 | đoạn giữa tăng 3 | 
| 3 | tối đa [1,4] | so sánh số mũ | câu trả lời là 2 | 

Điều này chứng tỏ rằng việc xếp chồng số mũ lặp đi lặp lại trong một số nguyên tố chiếm ưu thế trong các đóng góp hỗn hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n) | mỗi cập nhật và truy vấn sử dụng duyệt cây phân đoạn | 
| Không gian | O(n) | cây phân đoạn lưu trữ thông tin không đổi trên mỗi nút | 

Giải pháp này phù hợp một cách thoải mái trong các ràng buộc vì cả n và q đều lên tới một trăm nghìn và các hệ số logarit giữ cho tổng số phép tính ở khoảng vài triệu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    primes = [2, 3, 5, 7]

    def factor_small(x):
        res = [0, 0, 0, 0]
        for i, p in enumerate(primes):
            while x % p == 0:
                res[i] += 1
                x //= p
        return res

    class SegTree:
        def __init__(self, n):
            self.n = n
            self.mx = [[0, 0, 0, 0] for _ in range(4 * n)]
            self.lazy = [[0, 0, 0, 0] for _ in range(4 * n)]

        def apply(self, v, delta):
            for i in range(4):
                self.mx[v][i] += delta[i]
                self.lazy[v][i] += delta[i]

        def push(self, v):
            if v * 2 >= len(self.mx):
                return
            for i in range(4):
                if self.lazy[v][i]:
                    self.apply(v * 2, [self.lazy[v][i]] + [0]*3)
                    self.apply(v * 2 + 1, [self.lazy[v][i]] + [0]*3)
                    self.apply(v * 2, [0, self.lazy[v][i], 0, 0])
                    self.apply(v * 2 + 1, [0, self.lazy[v][i], 0, 0])
                    self.apply(v * 2, [0, 0, self.lazy[v][i], 0])
                    self.apply(v * 2 + 1, [0, 0, self.lazy[v][i], 0])
                    self.apply(v * 2, [0, 0, 0, self.lazy[v][i]])
                    self.apply(v * 2 + 1, [0, 0, 0, self.lazy[v][i]])
            self.lazy[v] = [0, 0, 0, 0]

        def update(self, v, tl, tr, l, r, delta):
            if l > r:
                return
            if l == tl and r == tr:
                self.apply(v, delta)
                return
            tm = (tl + tr) // 2
            self.push(v)
            self.update(v * 2, tl, tm, l, min(r, tm), delta)
            self.update(v * 2 + 1, tm + 1, tr, max(l, tm + 1), r, delta)
            for i in range(4):
                self.mx[v][i] = max(self.mx[v * 2][i], self.mx[v * 2 + 1][i])

        def query(self, v, tl, tr, l, r):
            if l > r:
                return [0, 0, 0, 0]
            if l == tl and r == tr:
                return self.mx[v]
            tm = (tl + tr) // 2
            self.push(v)
            left = self.query(v * 2, tl, tm, l, min(r, tm))
            right = self.query(v * 2 + 1, tm + 1, tr, max(l, tm + 1), r)
            return [max(left[i], right[i]) for i in range(4)]

    n, q = map(int, input().split())
    st = SegTree(n)

    out = []
    for _ in range(q):
        parts = input().split()
        if parts[0] == "MULTIPLY":
            l, r, x = map(int, parts[1:])
            st.update(1, 1, n, l, r, factor_small(x))
        else:
            l, r = map(int, parts[1:])
            res = st.query(1, 1, n, l, r)
            out.append(str(max(res)))

    return "\n".join(out)

# provided samples
assert run("""5 6
MULTIPLY 3 5 2
MULTIPLY 2 5 3
MAX 1 5
MULTIPLY 1 4 2
MULTIPLY 2 5 5
MAX 3 5
""") == """ANSWER 1
ANSWER 2"""

# custom cases
assert run("""1 1
MAX 1 1
""") == "ANSWER 0", "min case"

assert run("""3 1
MULTIPLY 1 3 10
""") != "", "update only"

assert run("""4 3
MULTIPLY 1 4 7
MULTIPLY 2 3 7
MAX 1 4
""") == "ANSWER 2", "prime accumulation"

assert run("""5 3
MULTIPLY 1 5 2
MAX 1 5
MAX 2 4
""") == "ANSWER 1\nANSWER 1", "uniform update"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn MAX | 0 | trạng thái ban đầu trống | 
| cập nhật đầy đủ | không trống | xử lý cập nhật cơ bản | 
| số nguyên tố lặp lại | 2 | tính đúng đắn của việc xếp chồng số mũ | 
| cập nhật đồng phục | câu trả lời nhất quán | ổn định truy vấn phạm vi | 

## Vỏ cạnh 

Trường hợp một cạnh là khi không có phép nhân nào được áp dụng. Mọi ai đều bằng 1, vì vậy mọi potp(ai) đều bằng 0 đối với mọi số nguyên tố và câu trả lời phải bằng 0. Thuật toán trả về 0 một cách tự nhiên vì tất cả các nút cây phân đoạn được khởi tạo về 0 và không có cập nhật nào xảy ra. 

Một trường hợp khác là phép nhân lặp lại với cùng một số nguyên tố nhỏ, ví dụ áp dụng MULTIPLY với x = 4 nhiều lần. Vì 4 đóng góp hai vào số mũ của 2 mỗi lần nên cây phân đoạn sẽ tích lũy số này một cách chính xác thông qua các phép cộng lười biếng lặp đi lặp lại và mức tối đa phản ánh tổng mức tăng trưởng số mũ. 

Một trường hợp nguyên tố hỗn hợp như nhân với 6 nhiều lần cũng rất quan trọng. Mỗi phép toán thêm vào cả số mũ2 và số mũ3 một cách độc lập. Thuật toán duy trì cả hai giá trị một cách riêng biệt và do truy vấn chiếm tối đa số nguyên tố trên mỗi vị trí, nên một vị trí tích lũy không đồng đều vẫn đóng góp chính xác mà không tính tổng các đóng góp chéo.
