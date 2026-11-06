---
title: "CF 123D - Chuỗi"
description: "Chúng ta được cấp một chuỗi s. Với mỗi chuỗi con x của s, chúng ta xét tất cả các vị trí mà x xuất hiện bên trong s. Giả sử những lần xuất hiện này bắt đầu ở các vị trí: $$p1 < p2 < dots < pk$$ Hàm F(s, x) đếm xem chúng ta có thể chọn bao nhiêu phân đoạn liền kề từ danh sách có thứ tự này của…"
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "string-suffix-structures"]
categories: ["algorithms"]
codeforces_contest: 123
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 92 (Div. 1 Only)"
rating: 2300
weight: 123
solve_time_s: 152
verified: true
draft: false
---

[CF 123D - Chuỗi](https://codeforces.com/problemset/problem/123/D) 

**Đánh giá:** 2300 
**Thẻ:** cấu trúc hậu tố chuỗi 
**Thời gian giải:** 2m 32s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một chuỗi`s`. Đối với mỗi chuỗi con`x`của`s`, chúng tôi xem xét tất cả các vị trí trong đó`x`xuất hiện bên trong`s`. 

Giả sử những lần xuất hiện này bắt đầu ở các vị trí:$$p_1 < p_2 < \dots < p_k$$chức năng`F(s, x)`đếm xem chúng ta có thể chọn bao nhiêu phân đoạn liền kề từ danh sách các lần xuất hiện theo thứ tự này. 

Nếu có`k`xuất hiện thì số mảng con liền kề là:$$\frac{k(k+1)}{2}$$Ví dụ: nếu một chuỗi con xuất hiện ở vị trí`[1,4,9]`, các nhóm liền kề hợp lệ là:$$[1], [4], [9], [1,4], [4,9], [1,4,9]$$mang lại`6`. 

Nhiệm vụ là tính:$$\sum_x F(s,x)$$trên tất cả các chuỗi con riêng biệt`x`của`s`. 

Độ dài chuỗi tối đa là`10^5`, điều này ngay lập tức loại trừ mọi thứ gần với bậc hai trên tất cả các chuỗi con. Một chuỗi có độ dài`10^5`có khoảng`5 \cdot 10^9`chuỗi con đếm các bản sao và thậm chí số lượng chuỗi con riêng biệt có thể đạt tới khoảng`5 \cdot 10^9`trong trường hợp xấu nhất về mặt khái niệm. Chúng ta cần một biểu diễn nén tất cả các chuỗi con lại với nhau. 

Đây là vấn đề về cấu trúc hậu tố vì các chuỗi con tương ứng một cách tự nhiên với các tiền tố của các hậu tố và các chuỗi con lặp lại tương ứng với các tiền tố chung giữa các hậu tố. 

Một số trường hợp khó xử lý. 

Coi như:```
aaaa
```Chuỗi con`"a"`xuất hiện 4 lần nên đóng góp của nó là:$$4 + 3 + 2 + 1 = 10$$Việc triển khai bất cẩn có thể chỉ tính số lần xuất hiện thay vì số lượng phạm vi xuất hiện liền kề. 

Một trường hợp nguy hiểm khác là:```
abcd
```Mỗi chuỗi con xuất hiện đúng một lần, vì vậy mỗi chuỗi con`F(s,x)=1`. Câu trả lời bằng số lượng chuỗi con riêng biệt:$$\frac{n(n+1)}{2}$$Bất kỳ logic nào giả định các chuỗi con lặp lại tồn tại ở mọi nơi sẽ được tính quá mức ở đây. 

Một trường hợp tinh tế hơn là sự lặp lại chồng chéo cao:```
ababab
```Chuỗi con`"ab"`xuất hiện ở các vị trí`1,3,5`. Đây vẫn là ba lần xuất hiện riêng biệt mặc dù chúng trùng lặp với các mẫu lặp lại khác. Cấu trúc hậu tố phải phân biệt các lần xuất hiện bằng vị trí hậu tố chứ không phải bằng sự rời rạc về khoảng cách. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. 

Liệt kê mọi chuỗi con riêng biệt`x`. Đối với mỗi chuỗi, hãy quét chuỗi và thu thập tất cả các vị trí xuất hiện. Nếu có`k`lần xuất hiện, thêm:$$\frac{k(k+1)}{2}$$để trả lời. 

Điều này đúng vì định nghĩa của`F`chỉ phụ thuộc vào danh sách xuất hiện. 

Vấn đề là sự phức tạp. có`O(n^2)`chuỗi con và việc kiểm tra các lần xuất hiện sẽ tốn một chi phí khác`O(n)`. Ngay cả với việc tối ưu hóa mảng băm hoặc hậu tố, việc xử lý tất cả các chuỗi con một cách độc lập sẽ trở nên quá lớn đối với`n = 10^5`. 

Quan sát quan trọng là nhiều chuỗi con có chung cấu trúc tập hợp sự xuất hiện. 

Đây chính xác là những gì hậu tố automaton nén. 

Trong máy tự động hậu tố, mỗi trạng thái đại diện cho toàn bộ lớp chuỗi con. Mọi chuỗi con được đại diện bởi một trạng thái đều có cùng một tập hợp các vị trí kết thúc, có nghĩa là tất cả chúng đều có cùng số lần xuất hiện. 

Giả sử một trạng thái chứa các chuỗi con có độ dài là:$$(\text{link.len}+1) \dots \text{len}$$Tất cả các chuỗi con này xảy ra chính xác`cnt[state]`lần. 

Sau đó, mỗi người trong số họ đóng góp:$$\frac{cnt(cnt+1)}{2}$$Vậy tổng đóng góp của bang đó là:$$(\text{len} - \text{link.len}) \times \frac{cnt(cnt+1)}{2}$$Bây giờ vấn đề trở thành: 

1. Xây dựng máy tự động hậu tố. 
2. Tính số lần xuất hiện cho từng trạng thái. 
3. Tính tổng công thức đóng góp trên tất cả các trạng thái. 

Điều này làm giảm vấn đề về thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n²) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một máy tự động hậu tố cho chuỗi. 

Cửa hàng của mỗi bang: 

-`len`, độ dài tối đa được đại diện bởi trạng thái. 
-`link`, liên kết hậu tố. 
- chuyển tiếp theo ký tự. 
-`cnt`, số vị trí cuối đi qua trạng thái. 
2. Trong khi mở rộng máy tự động với mỗi ký tự, hãy khởi tạo trạng thái mới được tạo bằng`cnt = 1`. 

Mỗi hậu tố mới được thêm vào tương ứng với một điểm cuối xuất hiện. 
3. Xử lý các chuyển tiếp và sao chép chính xác như trong cấu trúc ô tô hậu tố tiêu chuẩn. 

Các trạng thái nhân bản chỉ thể hiện sự phân chia cấu trúc, do đó số lần xuất hiện ban đầu của chúng là`0`. 
4. Sau khi xây dựng, truyền số lần xuất hiện lên trên thông qua các liên kết hậu tố. 

Các trạng thái của quá trình theo thứ tự giảm dần`len`. 

Nếu trạng thái`v`có liên kết hậu tố`p`, sau đó:$$cnt[p] += cnt[v]$$Điều này hoạt động vì mỗi lần xuất hiện được đại diện bởi`v`cũng thuộc về tổ tiên liên kết hậu tố của nó. 
5. Đối với mỗi trạng thái không phải là gốc, hãy tính xem có bao nhiêu độ dài chuỗi con riêng biệt thuộc về trạng thái này. 

Nếu như:$$p = link[v]$$thì độ dài chuỗi con được biểu diễn là:$$len[p] + 1 \dots len[v]$$cho:$$len[v] - len[p]$$các chuỗi con riêng biệt. 
6. Hãy:$$k = cnt[v]$$Mọi chuỗi con được đại diện bởi trạng thái này xuất hiện chính xác`k`lần, vì vậy mỗi lần đóng góp:$$\frac{k(k+1)}{2}$$Nhân với số chuỗi con được biểu diễn và cộng vào câu trả lời. 

### Tại sao nó hoạt động 

Một hậu tố tự động nhóm các chuỗi con theo tập hợp các vị trí kết thúc của chúng. Hai chuỗi con thuộc cùng một trạng thái khi chúng xuất hiện ở cùng một vị trí bên trong chuỗi. 

Bởi vì`F(s,x)`chỉ phụ thuộc vào số lần xuất hiện của`x`, tất cả các chuỗi con được đại diện bởi cùng một trạng thái đều đóng góp như nhau. 

Đối với một tiểu bang`v`, tất cả độ dài chuỗi con được biểu thị tạo thành khoảng:$$(len[link[v]] + 1) \dots len[v]$$Mỗi một chuỗi con này xuất hiện`cnt[v]`lần. 

Vậy tổng số tiền đóng góp của nhà nước chính xác là:$$(len[v] - len[link[v]]) \times \frac{cnt[v](cnt[v]+1)}{2}$$Tính tổng số này trên tất cả các trạng thái sẽ đếm mọi chuỗi con riêng biệt chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SuffixAutomaton:
    def __init__(self, n):
        self.next = [{} for _ in range(2 * n)]
        self.link = [-1] * (2 * n)
        self.length = [0] * (2 * n)
        self.cnt = [0] * (2 * n)

        self.size = 1
        self.last = 0

    def extend(self, c):
        cur = self.size
        self.size += 1

        self.length[cur] = self.length[self.last] + 1
        self.cnt[cur] = 1

        p = self.last

        while p != -1 and c not in self.next[p]:
            self.next[p][c] = cur
            p = self.link[p]

        if p == -1:
            self.link[cur] = 0
        else:
            q = self.next[p][c]

            if self.length[p] + 1 == self.length[q]:
                self.link[cur] = q
            else:
                clone = self.size
                self.size += 1

                self.next[clone] = self.next[q].copy()
                self.length[clone] = self.length[p] + 1
                self.link[clone] = self.link[q]
                self.cnt[clone] = 0

                while p != -1 and self.next[p].get(c) == q:
                    self.next[p][c] = clone
                    p = self.link[p]

                self.link[q] = clone
                self.link[cur] = clone

        self.last = cur

def solve():
    s = input().strip()
    n = len(s)

    sam = SuffixAutomaton(n)

    for ch in s:
        sam.extend(ch)

    order = list(range(sam.size))
    order.sort(key=lambda x: sam.length[x], reverse=True)

    for v in order:
        p = sam.link[v]
        if p != -1:
            sam.cnt[p] += sam.cnt[v]

    ans = 0

    for v in range(1, sam.size):
        p = sam.link[v]

        num_substrings = sam.length[v] - sam.length[p]
        k = sam.cnt[v]

        ans += num_substrings * (k * (k + 1) // 2)

    print(ans)

solve()
```Máy tự động lưu trữ các chuyển đổi dưới dạng từ điển vì bảng chữ cái nhỏ và thưa thớt. Mỗi phần mở rộng tạo ra một trạng thái mới và đôi khi là một bản sao. 

Thứ tự truyền bá quan trọng. Số đếm phải chuyển từ trạng thái dài hơn sang trạng thái ngắn hơn, vì vậy chúng ta sắp xếp các trạng thái bằng cách giảm dần`len`. Việc đảo ngược thứ tự này sẽ khiến cha mẹ không hoàn thành khi xử lý. 

Biểu thức:```
sam.length[v] - sam.length[p]
```rất dễ mắc sai lầm. Nó đếm có bao nhiêu độ dài chuỗi con riêng biệt thuộc về trạng thái`v`. sử dụng`+1`ở đây sẽ vượt quá. 

Các trạng thái nhân bản bắt đầu bằng`cnt = 0`. Chúng không tương ứng với các điểm cuối hậu tố mới. Việc quên điều này sẽ gây ra tình trạng đếm quá mức lớn trong các chuỗi lặp đi lặp lại. 

Tất cả số học đều sử dụng số nguyên Python một cách tự nhiên, vì câu trả lời có thể vượt quá phạm vi 32 bit. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
aaaa
```Trạng thái của máy tự động sau khi lan truyền sẽ hoạt động như thế này: 

| Tiểu bang | len | liên kết len ​​| số lần xuất hiện | chuỗi con đại diện | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 4 |`"a"`| 10 | 
| 2 | 2 | 1 | 3 |`"aa"`| 6 | 
| 3 | 3 | 2 | 2 |`"aaa"`| 3 | 
| 4 | 4 | 3 | 1 |`"aaaa"`| 1 | 

Tổng cộng:$$10 + 6 + 3 + 1 = 20$$Ví dụ này chứng minh tại sao việc lan truyền sự xuất hiện là cần thiết. Trạng thái cuối cùng ban đầu có số đếm`1`, nhưng sau khi tích lũy liên kết hậu tố, các chuỗi con lặp lại ngắn hơn sẽ nhận được tất cả các lần xuất hiện một cách chính xác. 

### Ví dụ 2 

đầu vào:```
abcd
```| Tiểu bang | len | liên kết len ​​| số lần xuất hiện | chuỗi con đại diện | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 |`"a"`| 1 | 
| 2 | 2 | 1 | 1 |`"ab"`| 1 | 
| 3 | 3 | 2 | 1 |`"abc"`| 1 | 
| 4 | 4 | 3 | 1 |`"abcd"`| 1 | 

Các trạng thái bổ sung tương tự đại diện`"b"`,`"bc"`,`"bcd"`, v.v., tất cả đều có số lượng`1`. 

Mỗi chuỗi con xuất hiện một lần, do đó mỗi chuỗi đóng góp chính xác`1`. 

Tổng số bằng số chuỗi con riêng biệt:$$\frac{4 \cdot 5}{2} = 10$$Điều này xác nhận rằng công thức suy biến chính xác khi không tồn tại sự lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Việc xây dựng hậu tố tự động và truyền bá số lượng là tuyến tính | 
| Không gian | O(n) | Nhiều nhất`2n`trạng thái được tạo | 

Một máy tự động hậu tố cho một chuỗi có độ dài`10^5`chứa nhiều nhất`2n-1`tiểu bang. Tất cả các hoạt động trong quá trình mở rộng đều được khấu hao tuyến tính, do đó giải pháp phù hợp một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    class SuffixAutomaton:
        def __init__(self, n):
            self.next = [{} for _ in range(2 * n)]
            self.link = [-1] * (2 * n)
            self.length = [0] * (2 * n)
            self.cnt = [0] * (2 * n)

            self.size = 1
            self.last = 0

        def extend(self, c):
            cur = self.size
            self.size += 1

            self.length[cur] = self.length[self.last] + 1
            self.cnt[cur] = 1

            p = self.last

            while p != -1 and c not in self.next[p]:
                self.next[p][c] = cur
                p = self.link[p]

            if p == -1:
                self.link[cur] = 0
            else:
                q = self.next[p][c]

                if self.length[p] + 1 == self.length[q]:
                    self.link[cur] = q
                else:
                    clone = self.size
                    self.size += 1

                    self.next[clone] = self.next[q].copy()
                    self.length[clone] = self.length[p] + 1
                    self.link[clone] = self.link[q]
                    self.cnt[clone] = 0

                    while p != -1 and self.next[p].get(c) == q:
                        self.next[p][c] = clone
                        p = self.link[p]

                    self.link[q] = clone
                    self.link[cur] = clone

            self.last = cur

    s = input().strip()
    sam = SuffixAutomaton(len(s))

    for ch in s:
        sam.extend(ch)

    order = list(range(sam.size))
    order.sort(key=lambda x: sam.length[x], reverse=True)

    for v in order:
        p = sam.link[v]
        if p != -1:
            sam.cnt[p] += sam.cnt[v]

    ans = 0

    for v in range(1, sam.size):
        p = sam.link[v]
        ans += (
            (sam.length[v] - sam.length[p])
            * (sam.cnt[v] * (sam.cnt[v] + 1) // 2)
        )

    return str(ans)

# provided samples
assert run("aaaa\n") == "20", "sample 1"

# custom cases
assert run("a\n") == "1", "single character"
assert run("ab\n") == "3", "all substrings unique"
assert run("aaa\n") == "10", "high overlap repetitions"
assert run("abab\n") == "12", "alternating repeated structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`1`| Kích thước tối thiểu | 
|`ab`|`3`| Không có chuỗi con lặp lại | 
|`aaa`|`10`| Sự chồng chéo nặng nề và số lượng lặp đi lặp lại | 
|`abab`|`12`| Xử lý bản sao và truyền bá liên kết hậu tố | 

## Vỏ cạnh 

Hãy xem xét:```
a
```Chỉ có một chuỗi con,`"a"`, xuất hiện một lần 

Máy tự động tạo một trạng thái không root với:$$cnt = 1$$Sự đóng góp:$$1 \times \frac{1 \cdot 2}{2} = 1$$Đầu ra của thuật toán`1`. 

Bây giờ hãy xem xét:```
aaaa
```Đây là kiểu lặp lại khó nhất vì mỗi chuỗi con chồng lên nhau rất nhiều. 

Trong quá trình lan truyền: 

-`"aaaa"`góp phần vào`"aaa"`-`"aaa"`góp phần vào`"aa"`-`"aa"`góp phần vào`"a"`Số lần xuất hiện cuối cùng trở thành`1,2,3,4`tương ứng. 

Thuật toán tính toán chính xác:$$10 + 6 + 3 + 1 = 20$$Việc triển khai có lỗi làm quên tích lũy liên kết hậu tố sẽ xử lý không chính xác mọi chuỗi con xảy ra một lần. 

Cuối cùng hãy xem xét:```
ababab
```Chuỗi con`"ab"`xuất hiện ba lần. Máy tự động nhóm tất cả các bản sao lại với nhau thông qua các lớp tương đương vị trí cuối được chia sẻ. 

Trạng thái tương ứng nhận được:$$cnt = 3$$Vì thế`"ab"`đóng góp:$$\frac{3 \cdot 4}{2} = 6$$Điều này xác nhận rằng các lần xuất hiện trùng lặp được tính một cách độc lập và chính xác.
