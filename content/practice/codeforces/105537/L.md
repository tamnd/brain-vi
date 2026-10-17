---
title: "CF 105537L - Chuỗi con chung dài nhất"
description: "Chúng tôi có hai chuỗi và chúng tôi muốn xác định độ dài của đoạn liền kề dài nhất xuất hiện trong cả hai chuỗi. Đoạn liền kề ở đây có nghĩa là một chuỗi con nên các ký tự phải khớp theo thứ tự và không có khoảng trống."
date: "2026-06-27T01:01:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105537
codeforces_index: "L"
codeforces_contest_name: "2024-2025 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 105537
solve_time_s: 46
verified: true
draft: false
---

[CF 105537L - Chuỗi con chung dài nhất](https://codeforces.com/problemset/problem/105537/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai chuỗi và chúng tôi muốn xác định độ dài của đoạn liền kề dài nhất xuất hiện trong cả hai chuỗi. Đoạn liền kề ở đây có nghĩa là một chuỗi con nên các ký tự phải khớp theo thứ tự và không có khoảng trống. 

Đầu vào bao gồm hai dòng, mỗi dòng chứa một chuỗi trên một số bảng chữ cái. Đầu ra là một số nguyên biểu thị độ dài tối đa của chuỗi con tồn tại ở đâu đó bên trong cả hai chuỗi. 

Các ràng buộc đủ lớn đến mức bất kỳ cách tiếp cận nào so sánh tất cả các chuỗi con một cách rõ ràng đều trở nên không khả thi. Nếu cả hai chuỗi có độ dài lên tới khoảng 10^5 thì việc liệt kê tất cả các chuỗi con sẽ bao hàm khoảng O(n^2) ứng cử viên trên mỗi chuỗi và việc so sánh chúng sẽ bùng nổ thành ít nhất O(n^3) ở dạng đơn giản. Ngay cả những so sánh chuỗi con được tối ưu hóa dựa vào hàm băm nhưng vẫn lặp lại tất cả các cặp sẽ quá chậm trong trường hợp xấu nhất. 

Điều này ngay lập tức gợi ý rằng giải pháp phải tránh liệt kê rõ ràng các chuỗi con và thay vào đó dựa vào cấu trúc nén các so sánh chuỗi con lặp đi lặp lại. 

Một số tình huống khó khăn có ý nghĩa quan trọng trong thực tế. Nếu một chuỗi trống, câu trả lời là 0. Nếu các chuỗi không có ký tự chung thì câu trả lời cũng bằng 0, ngay cả khi cả hai đều dài. Nếu các chuỗi giống hệt nhau thì câu trả lời là độ dài đầy đủ của chuỗi. Trường hợp tinh tế hơn là các ký tự lặp lại, ví dụ`"aaaaa"`Và`"aaa"`, trong đó câu trả lời là độ dài tối thiểu và việc khớp chuỗi con đơn giản có thể dễ dàng đếm quá nhiều phần trùng lặp trừ khi được xử lý cẩn thận. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi tạo ra mọi chuỗi con của chuỗi đầu tiên và mọi chuỗi con của chuỗi thứ hai rồi so sánh chúng để tìm ra kết quả trùng khớp dài nhất. Ngay cả khi chúng tôi cố gắng tối ưu hóa việc so sánh bằng cách sử dụng hàm băm, số lượng chuỗi con vẫn là bậc hai, do đó, chúng tôi kết thúc với khoảng O(n^2) chuỗi con trên mỗi chuỗi và so sánh O(1) hoặc O(log n) trên mỗi cặp tùy thuộc vào việc triển khai. Điều này trở nên quá lớn một cách nhanh chóng. 

Một cách tiếp cận thô bạo hơn một chút là cố định một cặp vị trí bắt đầu, một vị trí trong mỗi chuỗi và mở rộng trong khi các ký tự khớp nhau. Đối với mỗi cặp, chúng tôi phát triển một cửa sổ phù hợp. Điều này đúng vì bất kỳ chuỗi con chung nào cũng phải tương ứng với một số vị trí bắt đầu được căn chỉnh. Tuy nhiên, điều này vẫn kiểm tra các cặp O(n^2) và có thể mở rộng lên tới O(n) mỗi cặp trong trường hợp xấu nhất, tạo ra hành vi O(n^3) trên các đầu vào đối nghịch như các ký tự lặp lại. 

Điều quan trọng là thay vì kiểm tra từng chuỗi con một cách độc lập, chúng ta có thể xử lý vấn đề như tìm tiền tố chung dài nhất giữa tất cả các hậu tố của hai chuỗi. Nếu chúng ta xem xét mọi hậu tố của chuỗi thứ nhất và mọi hậu tố của chuỗi thứ hai, thì bất kỳ chuỗi con chung nào cũng chính xác là tiền tố chung của một số cặp hậu tố. Điều này chuyển vấn đề thành tìm giá trị LCP tối đa trên tất cả các cặp hậu tố. 

Một mảng hậu tố hoặc máy tự động hậu tố nắm bắt tất cả các hậu tố và mối quan hệ của chúng một cách tự nhiên. Cách tiếp cận tự động hóa hậu tố đặc biệt rõ ràng ở đây. Một máy tự động hậu tố biểu diễn gọn gàng tất cả các chuỗi con của một chuỗi và mỗi trạng thái tương ứng với một tập hợp các chuỗi con có cùng vị trí cuối. Khi quét chuỗi thứ hai thông qua máy tự động được tạo từ chuỗi đầu tiên, chúng tôi có thể theo dõi chuỗi con dài nhất kết thúc tại mỗi vị trí xuất hiện trong chuỗi đầu tiên. 

Điều này làm giảm vấn đề so sánh tất cả các cặp chuỗi con với một lần quét tuyến tính duy nhất trên chuỗi thứ hai trong khi vẫn duy trì trạng thái khớp hiện tại trong máy tự động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(1) | Quá chậm | 
| Hậu tố Automaton | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một máy tự động hậu tố cho chuỗi đầu tiên, sau đó mô phỏng việc đi qua chuỗi thứ hai trên máy tự động này trong khi theo dõi kết quả khớp hợp lệ dài nhất kết thúc ở mỗi vị trí. 

1. Xây dựng một máy tự động hậu tố từ chuỗi đầu tiên. Mỗi trạng thái lưu trữ các chuyển tiếp cho các ký tự, một liên kết hậu tố và độ dài tối đa của chuỗi con được biểu thị bởi trạng thái đó. Cấu trúc này nén tất cả các chuỗi con của chuỗi đầu tiên thành trạng thái O(n). 
2. Khởi tạo hai biến,`state`chỉ vào trạng thái tự động ban đầu và`length`đại diện cho độ dài chuỗi con phù hợp hiện tại. 
3. Lặp lại từng ký tự của chuỗi thứ hai từ trái sang phải. 
4. Nếu có sự chuyển đổi từ trạng thái hiện tại bằng ký tự hiện tại, hãy làm theo nó và tăng`length`bởi một. Điều này có nghĩa là chúng tôi đã mở rộng chuỗi con hợp lệ tồn tại trong chuỗi đầu tiên. 
5. Nếu không có quá trình chuyển đổi như vậy, chúng tôi liên tục theo dõi các liên kết hậu tố cho đến khi tìm thấy trạng thái có quá trình chuyển đổi với ký tự hiện tại hoặc chúng tôi đạt đến gốc. Trong khi làm điều này, chúng tôi cũng giảm`length`tương ứng vì chúng tôi đang rút ngắn hậu tố phù hợp cho đến khi nó trở lại hợp lệ. 
6. Nếu chúng tôi tìm thấy một quá trình chuyển đổi hợp lệ sau dự phòng, hãy lấy nó và đặt`length`với độ dài của trạng thái chuyển tiếp đó cộng với một. Nếu không, hãy đặt lại`state`Và`length`về không. 
7. Theo dõi giá trị tối đa của`length`trên tất cả các vị trí trong chuỗi thứ hai. Giá trị này là câu trả lời. 

Bước dự phòng là cần thiết vì kết quả khớp hiện tại chỉ hợp lệ nếu nó tương ứng với chuỗi con trong chuỗi đầu tiên. Khi xảy ra sự không khớp, chúng ta phải tìm hậu tố dài nhất của chuỗi con hiện tại vẫn xuất hiện trong máy tự động. 

### Tại sao nó hoạt động 

Máy tự động hậu tố mã hóa mọi chuỗi con của chuỗi đầu tiên dưới dạng đường dẫn từ gốc. Tại bất kỳ thời điểm nào trong quá trình quét chuỗi thứ hai, thuật toán sẽ duy trì tính bất biến`length`tương ứng với hậu tố dài nhất của tiền tố được xử lý của chuỗi thứ hai cũng là chuỗi con của chuỗi đầu tiên và`state`là trạng thái tự động đại diện cho chuỗi con đó. Bất cứ khi nào xảy ra sự không khớp, các liên kết hậu tố sau sẽ loại bỏ hiệu quả các ký tự ở bên trái của chuỗi con hiện tại theo thứ tự độ dài giảm dần cho đến khi tồn tại một chuyển đổi hợp lệ. Bởi vì các liên kết hậu tố đại diện cho hậu tố thích hợp dài nhất cũng là tiền tố theo nghĩa tự động, điều này đảm bảo rằng chúng tôi không bao giờ bỏ qua chuỗi con ứng cử viên hợp lệ và không bao giờ đánh giá quá cao độ dài đối sánh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class State:
    __slots__ = ("next", "link", "length")
    def __init__(self):
        self.next = {}
        self.link = -1
        self.length = 0

def build_sa(s):
    st = [State()]
    last = 0

    for ch in s:
        cur = len(st)
        st.append(State())
        st[cur].length = st[last].length + 1

        p = last
        while p != -1 and ch not in st[p].next:
            st[p].next[ch] = cur
            p = st[p].link

        if p == -1:
            st[cur].link = 0
        else:
            q = st[p].next[ch]
            if st[p].length + 1 == st[q].length:
                st[cur].link = q
            else:
                clone = len(st)
                st.append(State())
                st[clone].length = st[p].length + 1
                st[clone].next = st[q].next.copy()
                st[clone].link = st[q].link

                while p != -1 and st[p].next[ch] == q:
                    st[p].next[ch] = clone
                    p = st[p].link

                st[q].link = st[cur].link = clone

        last = cur

    return st

def longest_common_substring(s, t):
    sa = build_sa(s)

    v = 0
    l = 0
    best = 0

    for ch in t:
        if ch in sa[v].next:
            v = sa[v].next[ch]
            l += 1
        else:
            while v != -1 and ch not in sa[v].next:
                v = sa[v].link
            if v == -1:
                v = 0
                l = 0
                continue
            l = sa[v].length + 1
            v = sa[v].next[ch]

        if l > best:
            best = l

    return best

s = input().strip()
t = input().strip()
print(longest_common_substring(s, t))
```Giải pháp bắt đầu bằng cách xây dựng một máy tự động hậu tố trên chuỗi đầu tiên. Mỗi trạng thái lưu trữ các chuyển đổi trong một từ điển, một liên kết hậu tố và độ dài tối đa của chuỗi con được biểu thị bởi trạng thái đó. Việc xây dựng đảm bảo rằng mọi chuỗi con của chuỗi đầu tiên tương ứng với một số đường dẫn trong máy tự động. 

Trong quá trình quét chuỗi thứ hai, chúng tôi duy trì trạng thái hiện tại và độ dài khớp. Nếu chúng tôi có thể mở rộng bằng cách sử dụng ký tự hiện tại, chúng tôi sẽ thực hiện trực tiếp. Nếu không, chúng tôi sẽ chuyển các liên kết hậu tố cho đến khi tìm thấy quá trình chuyển đổi hợp lệ hoặc quay lại thư mục gốc. Độ dài được điều chỉnh để phản ánh hậu tố hợp lệ dài nhất vẫn tồn tại trong máy tự động. Câu trả lời là độ dài trận đấu tối đa gặp phải trong quá trình quét này. 

Một chi tiết tinh tế là hành vi đặt lại khi chúng ta quay lại gốc mà vẫn không thể chuyển đổi. Nếu không đặt lại rõ ràng cả trạng thái và độ dài, độ dài đối sánh cũ có thể truyền không chính xác và vượt quá số lượng đối sánh. 

## Ví dụ đã hoạt động 

Hãy xem xét`s = "ababc"`Và`t = "babca"`. 

Chúng tôi theo dõi quá trình quét diễn ra như thế nào`t`. 

| Nhân vật | Chuyển đổi trạng thái | Chiều dài | Tốt nhất | 
| --- | --- | --- | --- | 
| b | gốc → b | 1 | 1 | 
| một | b → ba | 2 | 2 | 
| b | ba → bé | 3 | 3 | 
| c | bab → babc | 4 | 4 | 
| một | không khớp, dự phòng thành | 1 | 4 | 

Điều này thể hiện cách máy tự động mở rộng các kết quả khớp một cách tham lam và chỉ lùi lại khi cần thiết, bảo toàn chuỗi con hợp lệ dài nhất. 

Bây giờ hãy xem xét`s = "aaaaa"`Và`t = "aaa"`. 

| Nhân vật | Chuyển đổi trạng thái | Chiều dài | Tốt nhất | 
| --- | --- | --- | --- | 
| một | mở rộng | 1 | 1 | 
| một | mở rộng | 2 | 2 | 
| một | mở rộng | 3 | 3 | 

Trường hợp này hiển thị các ký tự lặp lại, trong đó máy tự động nằm trong vùng có khả năng tái sử dụng cao và liên tục mở rộng các kết quả khớp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi ký tự được xử lý một lần và việc truyền tải liên kết hậu tố sẽ giảm dần theo thời gian tuyến tính trên tất cả các lần chuyển đổi | 
| Không gian | O(n) | Máy tự động lưu trữ một số trạng thái và chuyển tiếp tuyến tính | 

Chi phí xây dựng là tuyến tính theo kích thước của chuỗi đầu tiên và quá trình quét là tuyến tính theo kích thước của chuỗi thứ hai. Điều này phù hợp thoải mái trong các ràng buộc thông thường lên tới 10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = inp.strip().split()
    if len(s) == 2:
        from sys import stdin
    import sys
    input = sys.stdin.readline

    class State:
        __slots__ = ("next", "link", "length")
        def __init__(self):
            self.next = {}
            self.link = -1
            self.length = 0

    def build_sa(s):
        st = [State()]
        last = 0
        for ch in s:
            cur = len(st)
            st.append(State())
            st[cur].length = st[last].length + 1
            p = last
            while p != -1 and ch not in st[p].next:
                st[p].next[ch] = cur
                p = st[p].link
            if p == -1:
                st[cur].link = 0
            else:
                q = st[p].next[ch]
                if st[p].length + 1 == st[q].length:
                    st[cur].link = q
                else:
                    clone = len(st)
                    st.append(State())
                    st[clone].length = st[p].length + 1
                    st[clone].next = st[q].next.copy()
                    st[clone].link = st[q].link
                    while p != -1 and st[p].next[ch] == q:
                        st[p].next[ch] = clone
                        p = st[p].link
                    st[q].link = st[cur].link = clone
            last = cur

        return st

    def solve(s, t):
        sa = build_sa(s)
        v = 0
        l = 0
        best = 0
        for ch in t:
            if ch in sa[v].next:
                v = sa[v].next[ch]
                l += 1
            else:
                while v != -1 and ch not in sa[v].next:
                    v = sa[v].link
                if v == -1:
                    v = 0
                    l = 0
                    continue
                l = sa[v].length + 1
                v = sa[v].next[ch]
            best = max(best, l)
        return str(best)

    s, t = inp.strip().split()
    return solve(s, t)

# provided samples
assert run("ababc babca") == "4", "sample 1"

# custom cases
assert run("a a") == "1", "single char match"
assert run("abc def") == "0", "no overlap"
assert run("aaaaa aaa") == "3", "repeated characters"
assert run("abcd abcd") == "4", "full match"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| một | 1 | chuỗi giống hệt nhau tối thiểu | 
| abc def | 0 | không có chuỗi con chung | 
| aaa aaa | 3 | nén ký tự lặp đi lặp lại | 
| abcd abcd | 4 | khớp chuỗi đầy đủ | 

## Vỏ cạnh 

Đối với các chuỗi trống hoặc một ký tự, máy tự động vẫn hoạt động chính xác vì trạng thái gốc không có chuyển đổi hoặc chuyển đổi đơn lẻ. Vì`"a"`Và`"a"`, quá trình quét ngay sau quá trình chuyển đổi hợp lệ và tạo ra độ dài 1. 

Đối với các chuỗi ký tự lặp lại như`"aaaaa"`Và`"aaa"`, mỗi bước nằm trong một chu kỳ nhỏ trong máy tự động. Logic dự phòng không bao giờ được kích hoạt, điều này xác nhận rằng thuật toán không yêu cầu liệt kê chuỗi con rõ ràng. 

Ví dụ, đối với các bảng chữ cái hoàn toàn rời rạc`"abc"`Và`"xyz"`, mọi ký tự đều gây ra sự dự phòng về trạng thái gốc và đặt lại ngay lập tức độ dài khớp. Giá trị tốt nhất vẫn bằng 0 xuyên suốt vì không có chuyển đổi nào thành công.
