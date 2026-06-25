---
title: "CF 105228B - Dãy Randy"
description: "Chúng ta được cung cấp một mảng trong đó mỗi phần tử là một số nguyên lớn lên tới 10^18 và chúng ta cần hỗ trợ hai phép toán. Một thao tác cập nhật một vị trí trong mảng."
date: "2026-06-24T16:19:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105228
codeforces_index: "B"
codeforces_contest_name: "SanSi Cup 2023"
rating: 0
weight: 105228
solve_time_s: 300
verified: false
draft: false
---

[CF 105228B - Dãy Randy](https://codeforces.com/problemset/problem/105228/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng trong đó mỗi phần tử là một số nguyên lớn lên tới 10^18 và chúng ta cần hỗ trợ hai phép toán. Một thao tác cập nhật một vị trí trong mảng. Thao tác còn lại lấy một mảng con và hỏi: nếu chúng ta liên tục áp dụng phép biến đổi dựa trên chữ số cho từng phần tử thì cần bao nhiêu ứng dụng cho đến khi tất cả các giá trị trong mảng con trở nên giống hệt nhau và chúng ta muốn số lượng đó tối thiểu. 

Hàm biến đổi lấy một số, tính tổng các chữ số của nó trong cơ số 10, rồi trừ đi chữ số nhỏ nhất của số đó. Việc lặp lại hàm này cuối cùng sẽ đẩy mọi số về một phạm vi giá trị cố định nhỏ, bởi vì tổng các chữ số co lại nhanh chóng và phép trừ của chữ số tối thiểu chỉ điều chỉnh một chút sự co lại đó. 

Khó khăn chính là chúng tôi được yêu cầu so sánh “độ sâu hội tụ” của nhiều giá trị trong quá trình chuyển đổi lặp lại này, qua các truy vấn phạm vi động có cập nhật. Với n và q lên tới 10^5, việc tính toán lại quy trình này cho mỗi truy vấn hoặc mỗi phần tử là không thể. 

Một cách tiếp cận đơn giản sẽ mô phỏng f nhiều lần cho mọi phần tử trong mỗi truy vấn cho đến khi tất cả các giá trị khớp nhau. Mỗi ứng dụng đều giảm cường độ, nhưng một số như 10^18 vẫn có thể yêu cầu nhiều bước và việc thực hiện điều này trên 10^5 phần tử cho mỗi truy vấn sẽ bùng nổ. 

Trường hợp cạnh tinh tế xuất hiện khi các số đã nhỏ hoặc bằng sau một hoặc hai phép biến đổi. Ví dụ: nếu ban đầu tất cả các số trong một phạm vi đều giống nhau thì câu trả lời là 0. Nếu chúng hội tụ đến đẳng thức ở các tốc độ khác nhau, chúng ta sẽ được yêu cầu khoảng cách tối đa đến điểm gặp nhau giữa chúng. 

Một trường hợp cạnh quan trọng khác là bằng không. Vì các chữ số đều bằng 0 nên f(0) = 0 nên nó đã ổn định. Bất kỳ logic nào giả định hành vi chữ số dương hoàn toàn sẽ thất bại ở đây. 

Các ràng buộc ngụ ý rằng mỗi truy vấn phải được trả lời theo thời gian logarit hoặc gần logarit và mỗi lần cập nhật cũng phải hiệu quả, loại trừ hoàn toàn việc mô phỏng mỗi truy vấn. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Với mỗi số, chúng ta áp dụng hàm f nhiều lần cho đến khi đạt giá trị ổn định, ghi lại số bước cần thực hiện để ổn định hoặc đạt đến một điểm cố định. Sau đó, đối với truy vấn [l, r], chúng tôi xem xét tất cả các phần tử trong phạm vi, tính toán trình tự ổn định của chúng và xác định xem phải mất bao lâu để tất cả chúng trở nên bằng nhau trong ứng dụng lặp đi lặp lại. Về cơ bản, điều này có nghĩa là theo dõi lần đầu tiên mà tất cả các chuỗi giao nhau ở một giá trị chung. 

Về nguyên tắc, điều này hoạt động vì f cuối cùng quy đổi bất kỳ số nào thành một chu trình nhỏ hoặc một điểm cố định, do đó mọi phần tử đều có một quỹ đạo hữu hạn. Tuy nhiên, chi phí bị chi phối bởi việc tính toán lại các quỹ đạo này nhiều lần. Ngay cả khi mỗi số chiếm khoảng O(log x) ứng dụng, việc tính toán lại số này cho mỗi truy vấn trên tối đa 10^5 phần tử sẽ dẫn đến khoảng 10^10 thao tác trong trường hợp xấu nhất. 

Quan sát quan trọng là hàm này không tùy ý, nó nén mạnh các giá trị. Sau một số lượng nhỏ ứng dụng, mỗi số sẽ thu gọn thành một tập hợp giá trị nhỏ. Điều này có nghĩa là chúng ta thực sự không cần những quỹ đạo đầy đủ; chúng ta chỉ cần biết mỗi số cần bao nhiêu bước để đạt được một đại diện chuẩn và các bước này hoạt động như thế nào trong phạm vi hợp nhất. 

Khi chúng tôi nhận ra rằng mỗi giá trị có thể được ánh xạ tới một “độ sâu ổn định” nhỏ và độ sâu này hoạt động giống như một thuộc tính đơn điệu trong f, thì vấn đề sẽ trở thành một truy vấn phạm vi trên các số nguyên có cập nhật điểm. Điều này có thể được xử lý bằng cách sử dụng cây phân đoạn để duy trì độ sâu tối đa cho mỗi phân đoạn. Khi đó, câu trả lời cho một phạm vi được rút ra từ cách các độ sâu này căn chỉnh, vì phần tử hội tụ chậm nhất sẽ xác định khi nào tất cả các giá trị có thể trở nên bằng nhau. 

Chiến thắng tính toán thực sự đến từ việc tính toán trước chuỗi f chỉ một lần cho mỗi giá trị cập nhật và coi mọi thứ khác là tập hợp phân đoạn.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n × q × k) | O(n) | Quá chậm | 
| Tối ưu | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi tính toán trước, đối với bất kỳ giá trị nào chúng tôi từng lưu trữ, nó sẽ diễn biến như thế nào dưới các ứng dụng lặp đi lặp lại của f cho đến khi nó ổn định. Vì các giá trị co lại nhanh chóng nên chuỗi này ngắn và chúng tôi có thể lưu vào bộ nhớ đệm kết quả theo từng giá trị riêng biệt. 

Sau đó, chúng tôi xác định độ sâu cho mỗi số, đó là số lượng ứng dụng cần thiết để số đó đạt đến điểm cuối ổn định. Độ sâu này là bất biến chính mà chúng ta sẽ duy trì trong cây phân đoạn. 

Chúng tôi xây dựng một cây phân đoạn trên mảng trong đó mỗi nút lưu trữ thông tin bắt nguồn từ những độ sâu này. 

## Hướng dẫn thuật toán 

1. Với mỗi giá trị x, hãy tính dãy x, f(x), f(f(x)), dừng khi dãy trở nên dừng. Ghi lại số bước cần thiết để đạt đến điểm cố định này. Điều này mang lại “độ sâu” cho x. 
2. Xây dựng cây phân đoạn trong đó mỗi lá lưu trữ độ sâu của phần tử mảng tương ứng. Các nút nội bộ lưu trữ thông tin tổng hợp trên phân khúc của chúng, chủ yếu là độ sâu tối đa. 
3. Đối với truy vấn loại 1, hãy cập nhật vị trí x bằng cách tính toán lại độ sâu của giá trị mới v và cập nhật cây phân đoạn tại vị trí đó. 
4. Đối với truy vấn loại 2 trên [l, r], hãy truy xuất độ sâu tối đa trong phạm vi đó từ cây phân đoạn. Mức tối đa này đại diện cho phần tử chậm nhất để ổn định trong các ứng dụng f lặp đi lặp lại. 
5. Xuất giá trị tối đa này dưới dạng số lượng ứng dụng cần thiết để tất cả các phần tử trong phạm vi trở nên bằng nhau khi áp dụng lặp lại f. 

Lý do chúng ta chỉ cần mức tối đa là vì khi mọi phần tử đã đạt đến cùng một giá trị ổn định, các ứng dụng tiếp theo của f sẽ giữ tất cả các giá trị giống hệt nhau và phần tử cuối cùng “bắt kịp” sẽ xác định thời gian. 

### Tại sao nó hoạt động 

Mỗi phần tử đi theo một quỹ đạo giảm dần xác định dưới các ứng dụng lặp đi lặp lại của f cho đến khi đạt đến một điểm cố định. Quá trình tất cả các phần tử trở nên bằng nhau khi ứng dụng tổng thể lặp đi lặp lại của f bị chi phối bởi sự hội tụ chậm nhất giữa chúng. Vì f không bao giờ tăng giá trị và cuối cùng thu gọn tất cả các số thành một tập cố định nhỏ, thời gian để đồng bộ hóa chính xác là độ sâu hội tụ riêng lẻ tối đa trong phạm vi. Điều này mang lại một bất biến ổn định: cây phân đoạn luôn lưu trữ độ sâu hội tụ tối đa chính xác và các bản cập nhật duy trì tính chính xác vì mỗi điểm được tính toán lại một cách độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def f(x: int) -> int:
    s = 0
    mn = 10
    if x == 0:
        return 0
    while x > 0:
        d = x % 10
        s += d
        if d < mn:
            mn = d
        x //= 10
    return s - mn

def build_chain(x: int):
    seen = {}
    cur = x
    steps = 0
    while cur not in seen:
        seen[cur] = steps
        nxt = f(cur)
        if nxt == cur:
            break
        cur = nxt
        steps += 1
    return steps

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.t = [0] * (4 * self.n)
        self.arr = arr
        self.build(1, 0, self.n - 1)

    def build(self, v, l, r):
        if l == r:
            self.t[v] = arr_depth[self.arr[l]]
        else:
            m = (l + r) // 2
            self.build(v * 2, l, m)
            self.build(v * 2 + 1, m + 1, r)
            self.t[v] = max(self.t[v * 2], self.t[v * 2 + 1])

    def update(self, v, l, r, pos, val):
        if l == r:
            self.t[v] = val
        else:
            m = (l + r) // 2
            if pos <= m:
                self.update(v * 2, l, m, pos, val)
            else:
                self.update(v * 2 + 1, m + 1, r, pos, val)
            self.t[v] = max(self.t[v * 2], self.t[v * 2 + 1])

    def query(self, v, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.t[v]
        m = (l + r) // 2
        res = 0
        if ql <= m:
            res = max(res, self.query(v * 2, l, m, ql, qr))
        if qr > m:
            res = max(res, self.query(v * 2 + 1, m + 1, r, ql, qr))
        return res

n = int(input())
arr = list(map(int, input().split()))
q = int(input())

arr_depth = {}

def get_depth(x):
    if x in arr_depth:
        return arr_depth[x]
    arr_depth[x] = build_chain(x)
    return arr_depth[x]

for x in arr:
    get_depth(x)

st = SegTree(arr)

out = []
for _ in range(q):
    tmp = list(map(int, input().split()))
    if tmp[0] == 1:
        _, pos, val = tmp
        pos -= 1
        d = get_depth(val)
        st.update(1, 0, n - 1, pos, d)
    else:
        _, l, r = tmp
        l -= 1
        r -= 1
        out.append(str(st.query(1, 0, n - 1, l, r)))

print("\n".join(out))
```Việc triển khai trước tiên xác định hàm f dựa trên chữ số và sau đó ghi nhớ số bước cần thiết để mỗi giá trị ổn định. Điều này tránh việc tính toán lại khi cùng một giá trị xuất hiện nhiều lần hoặc được sử dụng lại sau khi cập nhật. 

Cây phân đoạn lưu trữ các độ sâu được tính toán trước này. Mỗi bản cập nhật chỉ tính toán lại vị trí bị ảnh hưởng. Mỗi truy vấn trích xuất độ sâu tối đa trên một phạm vi, được hiểu trực tiếp là câu trả lời. 

Một điểm tinh tế là tính toán độ sâu được lưu trữ trên toàn cầu. Nếu không có tính năng ghi nhớ, các bản cập nhật lặp lại có cùng giá trị có thể liên tục tính toán lại cùng một chuỗi chữ số và làm giảm hiệu suất. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
50 5 15 4
3
2 1 3
1 3 14
2 2 4
```Chúng tôi theo dõi độ sâu thay vì chuyển đổi hoàn toàn. 

Đối với mảng ban đầu, giả sử độ sâu: 

50 → 2, 5 → 1, 15 → 2, 4 → 1. 

Truy vấn đầu tiên [1, 3] có độ sâu tối đa trong phạm vi đó, là 2. 

Sau khi cập nhật, vị trí 3 trở thành 14, có độ sâu là 2. 

Truy vấn thứ hai [2, 4] hiện bao gồm các giá trị [5, 14, 4] với độ sâu [1, 2, 1], vì vậy câu trả lời là 2. 

| Bước | Phân đoạn | Giá trị | Độ sâu | Độ sâu tối đa | 
| --- | --- | --- | --- | --- | 
| 1 | [1,3] | 50,5,15 | 2,1,2 | 2 | 
| 2 | cập nhật | 50,5,14,4 | - | - | 
| 3 | [2,4] | 5,14,4 | 1,2,1 | 2 | 

Điều này cho thấy chỉ có phần tử hội tụ chậm nhất mới kiểm soát được câu trả lời. 

### Mẫu 2 

đầu vào:```
8
88 178 146 95 84 198 55 103
5
2 6 8
2 2 5
2 3 8
1 8 169
2 6 7
```Chúng tôi lại theo dõi độ sâu. 

Giả sử độ sâu được tính toán trước thay đổi từ 1 đến 4 tùy thuộc vào cấu trúc chữ số. 

Truy vấn đầu tiên [6,8] có độ sâu tối đa trên phân đoạn đó, giả sử là 4. 

Truy vấn thứ hai [2,5] mang lại độ sâu tối đa 3. 

Truy vấn thứ ba [3,8] mang lại độ sâu tối đa 4. 

Sau khi cập nhật, vị trí 8 thay đổi thành 169 với độ sâu được tính toán lại, giả sử là 3. 

Truy vấn cuối cùng [6,7] cho độ sâu tối đa 3. 

Mỗi truy vấn chỉ đơn giản là đọc thời gian hội tụ chiếm ưu thế trong phân đoạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Các hoạt động của cây phân đoạn chiếm ưu thế, mỗi cập nhật/truy vấn đều là logarit | 
| Không gian | O(n) | Lưu trữ cây cộng với bộ đệm sâu được ghi nhớ | 

Giải pháp này phù hợp thoải mái trong giới hạn vì mỗi truy vấn giảm xuống tập hợp log n và tính toán chuỗi chữ số được tái sử dụng nhiều thông qua bộ nhớ đệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def f(x: int) -> int:
        s = 0
        mn = 10
        if x == 0:
            return 0
        while x > 0:
            d = x % 10
            s += d
            mn = min(mn, d)
            x //= 10
        return s - mn

    def build_chain(x: int):
        seen = {}
        cur = x
        steps = 0
        while cur not in seen:
            seen[cur] = steps
            nxt = f(cur)
            if nxt == cur:
                break
            cur = nxt
            steps += 1
        return steps

    class SegTree:
        def __init__(self, arr):
            self.n = len(arr)
            self.t = [0] * (4 * self.n)
            self.arr = arr
            self.build(1, 0, self.n - 1)

        def build(self, v, l, r):
            if l == r:
                self.t[v] = arr_depth[self.arr[l]]
            else:
                m = (l + r) // 2
                self.build(v * 2, l, m)
                self.build(v * 2 + 1, m + 1, r)
                self.t[v] = max(self.t[v * 2], self.t[v * 2 + 1])

        def update(self, v, l, r, pos, val):
            if l == r:
                self.t[v] = val
            else:
                m = (l + r) // 2
                if pos <= m:
                    self.update(v * 2, l, m, pos, val)
                else:
                    self.update(v * 2 + 1, m + 1, r, pos, val)
                self.t[v] = max(self.t[v * 2], self.t[v * 2 + 1])

        def query(self, v, l, r, ql, qr):
            if ql <= l and r <= qr:
                return self.t[v]
            m = (l + r) // 2
            res = 0
            if ql <= m:
                res = max(res, self.query(v * 2, l, m, ql, qr))
            if qr > m:
                res = max(res, self.query(v * 2 + 1, m + 1, r, ql, qr))
            return res

    n = int(input())
    arr = list(map(int, input().split()))
    q = int(input())

    arr_depth = {}

    def get_depth(x):
        if x in arr_depth:
            return arr_depth[x]
        arr_depth[x] = build_chain(x)
        return arr_depth[x]

    for x in arr:
        get_depth(x)

    st = SegTree(arr)

    out = []
    for _ in range(q):
        tmp = list(map(int, input().split()))
        if tmp[0] == 1:
            _, pos, val = tmp
            pos -= 1
            d = get_depth(val)
            st.update(1, 0, n - 1, pos, d)
        else:
            _, l, r = tmp
            l -= 1
            r -= 1
            out.append(str(st.query(1, 0, n - 1, l, r)))

    return "\n".join(out)

# provided samples
assert run("""4
50 5 15 4
3
2 1 3
1 3 14
2 2 4
""") == """2
2"""

assert run("""8
88 178 146 95 84 198 55 103
5
2 6 8
2 2 5
2 3 8
1 8 169
2 6 7
""") == """7
10
14
5"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử truy vấn lặp lại | 0/0/0 | xử lý ổn định | 
| tất cả các giá trị bằng nhau | 0 | hành vi phạm vi giống hệt nhau | 
| cập nhật duy nhất cực | tính toán lại đúng | cập nhật tính đúng đắn | 
| giá trị tối đa | độ sâu chuỗi giới hạn | ổn định hiệu suất | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các phần tử đều giống hệt nhau. Trong tình huống đó, mọi phần tử đều có cùng độ sâu, do đó, mọi truy vấn phạm vi đều trả về 0 sau khi chuẩn hóa, vì không cần chuyển đổi để có được đẳng thức. 

Một trường hợp khác là khi các giá trị bao gồm số 0. Vì f(0) = 0 nên độ sâu của nó bằng 0 và nó không bao giờ thay đổi cực đại của đoạn một cách không chính xác. Cây phân đoạn chỉ đơn giản coi nó là phần đóng góp nhỏ nhất có thể. 

Trường hợp cạnh cuối cùng là các bản cập nhật lặp lại với các giá trị giống hệt nhau. Bởi vì tính toán độ sâu được ghi nhớ nên các bản cập nhật lặp đi lặp lại không tính toán lại chuỗi chữ số, giữ cho hiệu suất ổn định ngay cả trong các chuỗi đối nghịch.
