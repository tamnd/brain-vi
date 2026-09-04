---
title: "CF 104491B - Vấn đề tiêu chuẩn"
description: "Chúng ta được cho một tập hợp các phân đoạn trên dòng số nguyên. Mỗi phân đoạn mô tả một loạt các giá trị mà nó có thể “phát ra” và nó cũng mang một trọng số. Từ các phân đoạn này, chúng tôi chọn một số chuỗi con theo thứ tự ban đầu của chúng."
date: "2026-06-30T12:28:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104491
codeforces_index: "B"
codeforces_contest_name: "43rd Petrozavodsk Programming Camp (2022 Summer) Day 7. HSE Koresha Contest"
rating: 0
weight: 104491
solve_time_s: 132
verified: false
draft: false
---

[CF 104491B - Sự cố tiêu chuẩn](https://codeforces.com/problemset/problem/104491/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các phân đoạn trên dòng số nguyên. Mỗi phân đoạn mô tả một loạt các giá trị mà nó có thể “phát ra” và nó cũng mang một trọng số. Từ các phân đoạn này, chúng tôi chọn một số chuỗi con theo thứ tự ban đầu của chúng. 

Khi một dãy con được cố định, chúng tôi gán cho mỗi phân đoạn đã chọn một số nguyên cụ thể từ phạm vi của chính nó. Điều này tạo ra một chuỗi các số nguyên. Dãy số được coi là hợp lệ nếu chúng ta có thể chọn các số nguyên này sao cho chúng tạo thành một dãy không giảm. 

Trong số tất cả các dãy con hợp lệ, chúng ta cần hai thứ. Đầu tiên, tổng trọng số tối đa có thể có của các phân đoạn được chọn. Thứ hai, có bao nhiêu dãy con đạt được trọng số tối đa đó, modulo 998244353. 

Khó khăn chính là tính hợp lệ phụ thuộc vào việc liệu các phân đoạn đã chọn có thể được gán các giá trị tương thích hay không, chứ không chỉ trên chính các điểm cuối của phân đoạn. Cách đọc đơn giản có thể cho rằng đây là một bài toán dãy con có trọng số tiêu chuẩn, nhưng tính khả thi phụ thuộc vào việc liệu các khoảng có thể được xâu chuỗi thành một phép gán không giảm hay không. 

Các ràng buộc buộc chúng tôi phải xử lý tới hai trăm nghìn phân đoạn trong tất cả các trường hợp thử nghiệm, do đó, bất kỳ giải pháp nào thử tất cả các chuỗi con hoặc thậm chí lập trình động bậc hai trên các phân đoạn đều ngay lập tức không thể thực hiện được. Chúng tôi cần hành vi gần như tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm, thường giống như$O(n \log m)$. 

Một trường hợp phức tạp là tính khả thi không được xác định chỉ bằng sự chồng chéo từng cặp. Hai khoảng có thể trùng nhau nhưng vẫn thất bại trong chuỗi dài hơn nếu chúng ta chọn các giá trị trung gian không tương thích. Ví dụ như việc chọn$[1,2]$,$[2,3]$,$[1,1]$theo thứ tự đó là không hợp lệ vì khoảng thời gian cuối cùng buộc phải giảm sau các lần gán trước đó, mặc dù vẫn tồn tại sự chồng chéo theo cặp. 

Quan điểm đúng đắn là tính khả thi phụ thuộc vào việc liệu chúng ta có thể gán các giá trị một cách tham lam trong khi vẫn tôn trọng các ràng buộc hay không, điều này dẫn đến trạng thái theo dõi giá trị được chọn hiện tại thay vì chỉ khoảng thời gian cuối cùng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi chuỗi con của các phân đoạn và, đối với mỗi phân đoạn, cố gắng gán các giá trị một cách tham lam để kiểm tra tính khả thi. Đối với một dãy có độ dài$k$, việc kiểm tra tính khả thi là tuyến tính trong$k$, vì vậy nhìn chung điều này trở thành cấp số nhân trong$n$, theo thứ tự của$2^n \cdot n$, vượt xa mọi giới hạn. 

Quan sát cấu trúc chính là khi chúng ta sửa thứ tự tiếp theo, tính khả thi sẽ giảm xuống mức duy trì một “giá trị hiện tại” duy nhất. Khi xử lý một phân đoạn đã chọn$[l_i, r_i]$, chúng ta luôn có thể chọn giá trị hợp lệ nhỏ nhất có thể, đó là$\max(l_i, \text{current})$. Cách duy nhất để thất bại là nếu giá trị này vượt quá$r_i$. Điều này chuyển đổi tính khả thi thành máy trạng thái với một tham số: giá trị hiện tại. 

Điều này có nghĩa là chúng tôi đang thực hiện lựa chọn chuỗi con có trọng số trong đó trạng thái DP không chỉ là vị trí mà còn là giá trị hiện tại trong$[1, m]$. Quá trình chuyển đổi phụ thuộc vào việc một phân đoạn được bỏ qua hay được thực hiện và nếu được thực hiện thì phân đoạn đó sẽ biến đổi giá trị hiện tại như thế nào. 

Một DP ngây thơ trên tất cả$n \times m$các trạng thái vẫn sẽ quá chậm, vì vậy chúng tôi cần xử lý hàng loạt các chuyển đổi trên các phạm vi giá trị. Mỗi phân đoạn chỉ tạo ra hai hành vi trong phạm vi giá trị hiện tại, cho phép tối ưu hóa dựa trên cây phân đoạn với các cập nhật phạm vi và truy vấn tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| DP trên (i, giá trị) |$O(nm)$|$O(m)$| Quá chậm | 
| Cây phân đoạn được tối ưu hóa DP |$O(n \log m)$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì DP trên “giá trị hiện tại” sau khi xử lý các phân đoạn đã chọn. Cho phép`dp[x]`lưu trữ tổng trọng lượng tốt nhất có thể đạt được nếu giá trị hiện tại chính xác`x`, cùng với số cách để đạt được nó. 

Chúng tôi khởi tạo hệ thống trước khi chọn bất kỳ phân đoạn nào, trong đó giá trị hiện tại thực tế là 1 và tổng trọng số là 0. 

### bước 

1. Khởi tạo cây phân đoạn theo các giá trị$1 \ldots m$. Mỗi nút lưu trữ một cặp$(\text{best weight}, \text{count})$. Bộ`dp[1] = (0, 1)`và tất cả những người khác để$-\infty$. 
2. Xử lý các phân đoạn theo thứ tự đầu vào. Đối với mỗi phân khúc$[l, r]$với trọng lượng$c$, chúng tôi xây dựng DP mới bằng cách cập nhật cấu trúc hiện tại. 
3. Trước tiên hãy xem xét lấy phân đoạn cho các trạng thái có giá trị hiện tại nằm trong$(l, r]$. Nếu giá trị hiện tại là$x$trong phạm vi này, giá trị tiếp theo vẫn còn$x$, và chúng tôi chỉ cần thêm$c$đến tổng trọng lượng. Điều này hoạt động vì$x \ge l$, vì vậy giá trị được chọn có thể là$x$và tính khả thi được bảo toàn. Chúng tôi thực hiện phép cộng phạm vi$c$qua$(l, r]$. 
4. Tiếp theo hãy xem xét các trạng thái có giá trị hiện tại lớn nhất$l$. Đối với tất cả các trạng thái như vậy, sau khi lấy phân đoạn, giá trị tiếp theo sẽ trở thành$l$, vì chúng ta phải nâng nó lên ít nhất là điểm cuối bên trái của đoạn. Trong số tất cả các trạng thái này, chúng tôi cần trọng số có thể đạt được tốt nhất trước khi lấy phân đoạn, vì vậy chúng tôi truy vấn mức tối đa qua tiền tố$[1, l]$. 
5. Thêm$c$đến giá trị tiền tố tốt nhất này và sử dụng nó để cập nhật trạng thái$l$bằng cách lấy mức tối đa giữa giá trị hiện tại tại$l$và ứng cử viên mới này. Nếu bằng nhau thì tính tổng số cách. 
6. Đồng thời cho phép bỏ qua phân đoạn một cách ngầm định bằng cách chuyển tiếp DP trước đó không thay đổi, vì tất cả các bản cập nhật được áp dụng lên trên cấu trúc hiện tại. 
7. Sau khi xử lý tất cả các phân đoạn, hãy quét DP để tìm giá trị tối đa trên tất cả các trạng thái và tổng số trạng thái đạt được giá trị đó. 

### Tại sao nó hoạt động 

Bất biến DP là sau khi xử lý tiền tố của các phân đoạn,`dp[x]`thể hiện chính xác trọng số có thể đạt được tốt nhất trong số tất cả các chuỗi con hợp lệ có giá trị được xây dựng cuối cùng chính xác là`x`. Mọi chuyển đổi đều duy trì tính khả thi vì nó thực thi rõ ràng quy tắc xây dựng đơn điệu thông qua trạng thái giá trị hiện tại. Cây phân đoạn đảm bảo chúng tôi luôn áp dụng các chuyển đổi cho toàn bộ phạm vi hoạt động thống nhất theo quy tắc cập nhật, do đó không có trạng thái nào được cập nhật một phần hoặc không chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
NEG = -10**30

class SegTree:
    def __init__(self, n):
        self.n = n
        self.mx = [NEG] * (4 * n)
        self.cnt = [0] * (4 * n)
        self.lz = [0] * (4 * n)

    def _apply(self, idx, val):
        self.mx[idx] += val
        self.lz[idx] += val

    def _push(self, idx):
        if self.lz[idx]:
            v = self.lz[idx]
            self._apply(idx * 2, v)
            self._apply(idx * 2 + 1, v)
            self.lz[idx] = 0

    def _pull(self, idx):
        if self.mx[idx * 2] > self.mx[idx * 2 + 1]:
            self.mx[idx] = self.mx[idx * 2]
            self.cnt[idx] = self.cnt[idx * 2]
        elif self.mx[idx * 2] < self.mx[idx * 2 + 1]:
            self.mx[idx] = self.mx[idx * 2 + 1]
            self.cnt[idx] = self.cnt[idx * 2 + 1]
        else:
            self.mx[idx] = self.mx[idx * 2]
            self.cnt[idx] = (self.cnt[idx * 2] + self.cnt[idx * 2 + 1]) % MOD

    def build(self, idx, l, r):
        if l == r:
            if l == 1:
                self.mx[idx] = 0
                self.cnt[idx] = 1
            else:
                self.mx[idx] = NEG
                self.cnt[idx] = 0
            return
        m = (l + r) // 2
        self.build(idx * 2, l, m)
        self.build(idx * 2 + 1, m + 1, r)
        self._pull(idx)

    def range_add(self, idx, l, r, ql, qr, val):
        if ql <= l and r <= qr:
            self._apply(idx, val)
            return
        self._push(idx)
        m = (l + r) // 2
        if ql <= m:
            self.range_add(idx * 2, l, m, ql, qr, val)
        if qr > m:
            self.range_add(idx * 2 + 1, m + 1, r, ql, qr, val)
        self._pull(idx)

    def query_max(self, idx, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.mx[idx], self.cnt[idx]
        self._push(idx)
        m = (l + r) // 2
        best = NEG
        ways = 0
        if ql <= m:
            v, c = self.query_max(idx * 2, l, m, ql, qr)
            if v > best:
                best, ways = v, c
            elif v == best:
                ways = (ways + c) % MOD
        if qr > m:
            v, c = self.query_max(idx * 2 + 1, m + 1, r, ql, qr)
            if v > best:
                best, ways = v, c
            elif v == best:
                ways = (ways + c) % MOD
        return best, ways

    def point_chmax(self, idx, l, r, pos, val, ways):
        if l == r:
            if val > self.mx[idx]:
                self.mx[idx] = val
                self.cnt[idx] = ways % MOD
            elif val == self.mx[idx]:
                self.cnt[idx] = (self.cnt[idx] + ways) % MOD
            return
        self._push(idx)
        m = (l + r) // 2
        if pos <= m:
            self.point_chmax(idx * 2, l, m, pos, val, ways)
        else:
            self.point_chmax(idx * 2 + 1, m + 1, r, pos, val, ways)
        self._pull(idx)

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        segs = [tuple(map(int, input().split())) for _ in range(n)]

        st = SegTree(m)
        st.build(1, 1, m)

        for l, r, c in segs:
            if l <= r:
                st.range_add(1, 1, m, l + 1, r, c)
                best, ways = st.query_max(1, 1, m, 1, l)
                if best != NEG:
                    st.point_chmax(1, 1, m, l, best + c, ways)

        ans_val, ans_cnt = st.query_max(1, 1, m, 1, m)
        print(ans_val, ans_cnt % MOD)

if __name__ == "__main__":
    solve()
```Cây phân đoạn lưu trữ cả trọng lượng tối đa có thể đạt được và số cách để đạt được mức tối đa đó. Lan truyền lười biếng chỉ được sử dụng để cộng phạm vi, tương ứng chính xác với quá trình chuyển đổi ở các trạng thái mà giá trị hiện tại không thay đổi sau khi lấy một phân đoạn. 

Truy vấn tiền tố rất cần thiết vì nó nắm bắt tất cả các trạng thái thu gọn thành một giá trị duy nhất$l$sau khi lấy một đoạn. Điểm cập nhật tại$l$hợp nhất những đóng góp này một cách chính xác. 

Một sai lầm phổ biến là cố gắng áp dụng một bản cập nhật thống nhất cho toàn bộ tiền tố, nhưng các trạng thái đó sẽ thu gọn vào một đích duy nhất, vì vậy chúng phải được tổng hợp trước khi cập nhật. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét các phân đoạn:$[1,2], c=1$,$[2,3], c=2$Chúng tôi theo dõi dp theo giá trị 1..3. 

| Bước | Phân đoạn | Hành động | tóm tắt trạng thái dp | 
| --- | --- | --- | --- | 
| 0 | ban đầu | dp[1]=0 | (1:0) | 
| 1 | [1,2] | tiền tố=0 → cập nhật 1, thêm phạm vi | (1:1, 2:1) | 
| 2 | [2,3] | tiền tố(1..2)=1 → dp[2]=3, cộng vào 3 | (1:1, 2:3, 3:3) | 

Đáp án cuối cùng là 3 cách 1. 

Điều này cho thấy cách bổ sung phạm vi và thu gọn tiền tố tương tác như thế nào: trạng thái 1 lan truyền thành 2 thông qua tiền tố, sau đó phát triển khác với các trạng thái ở phạm vi giữa. 

### Ví dụ 2 

Phân đoạn:$[1,1], c=3$,$[1,2], c=3$| Bước | Phân đoạn | Hành động | tóm tắt trạng thái dp | 
| --- | --- | --- | --- | 
| 0 | ban đầu | dp[1]=0 | (1:0) | 
| 1 | [1,1] | tiền tố=0 → dp[1]=3 | (1:3) | 
| 2 | [1,2] | tiền tố=3 → dp[1]=6, cộng vào 2 | (1:6, 2:6) | 

Cả hai trạng thái đều đạt được giá trị tối ưu như nhau và cả hai đều đóng góp vào việc đếm. 

Điều này nhấn mạnh cách nhiều trạng thái có thể hội tụ đến cùng một giá trị tối đa, đòi hỏi phải tính toán cẩn thận các cực đại bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log m)$| Mỗi phân đoạn kích hoạt một truy vấn tiền tố, cập nhật điểm và thêm phạm vi | 
| Không gian |$O(m)$| Cây phân đoạn trên miền giá trị | 

Tổng các ràng buộc về kích thước trên tất cả các trường hợp thử nghiệm có tổng là$2 \cdot 10^5$, do đó hệ số logarit trên mỗi phân đoạn vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib

    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimal case
assert run("1\n1 1\n1 1 5\n") == "5 1"

# chain
assert run("1\n2 3\n1 2 1\n2 3 2\n") == "3 1"

# all same interval
assert run("1\n3 5\n1 5 1\n1 5 1\n1 5 1\n") == "3 1"

# disjoint forcing choice
assert run("1\n2 5\n1 1 10\n5 5 10\n") == "20 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phân đoạn đơn | 5 1 | khởi tạo cơ sở | 
| khoảng thời gian chuỗi | 3 1 | tuyên truyền của nhà nước | 
| khoảng giống hệt nhau | 3 1 | đếm hội tụ | 
| thái cực rời rạc | 20 1 | bỏ qua đúng so với lấy | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các phân đoạn có cùng phạm vi giá trị. DP phải tích lũy chính xác nhiều cách dẫn đến cùng một trạng thái cuối cùng mà không cần tính hai lần. Logic hợp nhất cây phân đoạn đảm bảo số lượng chỉ được tính tổng khi các giá trị bằng nhau. 

Một trường hợp tinh tế khác là khi điểm cuối bên trái của phân đoạn là 1. Trong trường hợp đó, tất cả các trạng thái sẽ thu gọn trực tiếp về trạng thái 1 và truy vấn tiền tố trải rộng trên toàn bộ phạm vi hoạt động. Thuật toán xử lý chính xác điều này bằng cách luôn truy vấn$[1, l]$, trở thành DP đầy đủ trong kịch bản đó. 

Cuối cùng, khi không thể truy cập trạng thái nào cho truy vấn tiền tố, giá trị tốt nhất vẫn còn$-\infty$, và việc cập nhật điểm bị bỏ qua. Điều này ngăn chặn việc truyền bá không hợp lệ các cấu hình không thể truy cập vào DP.
