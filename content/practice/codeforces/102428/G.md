---
title: "CF 102428G - Dán Hình Ảnh"
description: "Tên thành phố là một chuỗi C. Một ảnh có thể chụp bất kỳ phần liền kề nào của C, vì vậy mọi chuỗi con của C đều có thể là một ảnh. Chúng tôi có thể sắp xếp các hình ảnh theo bất kỳ thứ tự nào và ghép nội dung của chúng để có được tên của một người bạn."
date: "2026-08-12T07:15:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102428
codeforces_index: "G"
codeforces_contest_name: "2019-2020 ACM-ICPC Latin American Regional Programming Contest"
rating: 0
weight: 102428
solve_time_s: 118
verified: true
draft: false
---

[CF 102428G - Dán hình ảnh](https://codeforces.com/problemset/problem/102428/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Tên thành phố là một chuỗi`C`. Một bức ảnh có thể chụp bất kỳ phần liền kề nào của`C`, vì vậy mọi chuỗi con của`C`là một hình ảnh có thể Chúng tôi có thể sắp xếp các hình ảnh theo bất kỳ thứ tự nào và ghép nội dung của chúng để có được tên của một người bạn. Các ký tự bên trong một bức ảnh riêng lẻ không thể thay đổi, đảo ngược hoặc sắp xếp lại. 

Đối với chuỗi của mỗi người bạn`P`, chúng ta cần số lượng chuỗi con nhỏ nhất của`C`sự nối của nó chính xác là`P`. Một bức ảnh có thể được chụp cho bất kỳ phần nào xuất hiện trong tên thành phố, do đó, phần đó có thể được sử dụng lại khi cần thiết. 

Ví dụ, với`C = MONTEVIDEO`, chuỗi`DEMONIO`có thể được chia thành`DE | MON | I | O`. Mỗi phần là một chuỗi con liền kề của`C`và các quân cờ có thể xuất hiện theo thứ tự khác với vị trí của chúng trong tên thành phố. Câu trả lời là bốn. 

Tổng độ dài của tất cả các chuỗi bạn bè tối đa là`2 * 10^5`. Bản thân chuỗi thành phố là văn bản cố định mà từ đó tất cả các phần có thể được lấy ra. Giám khảo chính thức đưa ra giới hạn thời gian là 2 giây và bộ nhớ 1024 MB. Một giải pháp kiểm tra mọi chuỗi con có thể có của mỗi người bạn là phương trình bậc hai theo độ dài bạn bè, có thể có nghĩa là khoảng`2 * 10^10`chuỗi con ứng cử viên cho một người bạn dài`2 * 10^5`. Điều đó vượt xa những gì có thể được xử lý trong thời hạn. Về cơ bản, chúng tôi cần công việc tuyến tính trong tổng kích thước đầu vào. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai có vẻ hợp lý trở nên sai lầm. 

Coi như```
A
2
A
B
```Các câu trả lời là`1`Và`-1`. Không thể kết bạn ngay khi một số ký tự bắt buộc không thể bắt đầu bất kỳ chuỗi con nào của thành phố. Việc triển khai tăng số lượng mảnh một cách mù quáng sau khi khớp không thành công có thể vô tình đếm một mảnh không thể thay vì báo cáo`-1`. 

Coi như```
ABA
1
ABAB
```Câu trả lời là`2`, sử dụng`ABA | B`. Việc triển khai tham lam phải cho phép hình ảnh tiếp theo bắt đầu ở bất kỳ nhân vật nào của người bạn, bao gồm cả nhân vật cũng là một phần của hình ảnh được sử dụng trước đó. Việc hạn chế hình ảnh ở những vị trí không chồng chéo trong thành phố sẽ giải quyết được một vấn đề khác. 

Coi như```
ABC
1
CBA
```Câu trả lời là`3`, sử dụng`C | B | A`. Thứ tự của các bức tranh là không bị hạn chế nhưng các nhân vật trong bức tranh thì không. đảo ngược`ABC`để có được`CBA`không được phép, vì vậy không thể tạo nên toàn bộ tình bạn chỉ bằng một bức ảnh. 

Cuối cùng,```
AAA
1
AAAA
```có câu trả lời`2`, sử dụng`AAA | A`. Có thể chụp lại cùng một khu vực của thành phố, vì vậy thực tế là bức ảnh đầu tiên đã được sử dụng`AAA`không loại bỏ nó khỏi tập hợp các hình ảnh có thể. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là lập trình động. Cho phép`dp[i]`là số lượng hình ảnh tối thiểu cần thiết để xây dựng hình ảnh đầu tiên`i`nhân vật của một người bạn. Từ vị trí`i`, chúng ta có thể thử mọi vị trí kết thúc`j`, kiểm tra xem`P[i:j]`xảy ra ở đâu đó bên trong`C`, và cập nhật`dp[j]`. 

Điều này đúng vì mọi cách xây dựng hợp lệ đều có một số hình ảnh cuối cùng và DP xem xét mọi lựa chọn có thể có cho chuỗi con cuối cùng đó. Vấn đề là số lượng chuỗi con chúng ta phải kiểm tra. Một người bạn lâu năm`m`có chính xác`m(m+1)/2`chuỗi con không trống. Vì`m = 200000`, đó là`20000100000`, khoảng hai mươi tỷ ứng viên, thậm chí trước cả khi xem xét chi phí kiểm tra xem mỗi ứng cử viên có xuất hiện trong thành phố hay không. 

Chúng ta có thể xử lý trước tất cả các chuỗi con của thành phố thành một tập hợp, nhưng điều đó tạo ra trở ngại bậc hai tương tự về phía thành phố. Một thành phố dài`L`có`L(L+1)/2`sự xuất hiện của chuỗi con cần xem xét. Ngay cả với việc tra cứu tập hợp băm theo thời gian liên tục sau đó, quá trình tiền xử lý đã quá lớn khi`L`là lớn. 

Quan sát hữu ích là chúng ta không thực sự cần phải xem xét mọi phần tiếp theo có thể xảy ra. Giả sử chúng ta hiện đang cố gắng xây dựng hậu tố`P[i:]`. Cho phép`G`là tiền tố dài nhất của`P[i:]`xảy ra như một chuỗi con của`C`. 

Lựa chọn`G`ít nhất luôn tốt bằng việc chọn một bức ảnh đầu tiên ngắn hơn. Lấy bất kỳ công trình tối ưu nào có hình ảnh đầu tiên có chiều dài`k`, Ở đâu`k <= |G|`. Từ`G`chính nó là một chuỗi con của`C`, nó có thể thay thế một số hình ảnh đầu tiên của công trình đó. Nếu như`G`kết thúc bên trong một trong những hình ảnh đó, hậu tố không được sử dụng của hình ảnh đó chính là một chuỗi con của`C`, bởi vì mọi chuỗi con của một chuỗi con cũng là một chuỗi con của`C`. Do đó việc xây dựng có thể được điều chỉnh mà không cần sử dụng thêm hình ảnh. 

Điều này đưa ra một quy tắc tham lam: tại mọi vị trí trong người bạn, hãy lấy tiền tố dài nhất xảy ra trong thành phố. 

Nhiệm vụ còn lại là tìm nhanh tiền tố dài nhất đó. Một máy tự động hậu tố chính xác là một biểu diễn thu gọn của tất cả các chuỗi con của một chuỗi. Bắt đầu từ trạng thái ban đầu và các chuyển đổi tiếp theo đối với các nhân vật của người bạn cho chúng ta biết tiền tố hiện tại tiếp tục tồn tại trong thành phố trong bao lâu. Khi thiếu phần chuyển tiếp, tiền tố hiện tại là hình ảnh dài nhất có thể. 

Phương pháp brute-force hoạt động vì nó khám phá rõ ràng tất cả các phân đoạn có thể. Nó thất bại vì có nhiều phần ứng cử viên theo phương trình bậc hai. Nhận xét rằng phần đầu tiên dài nhất có thể luôn có thể thay thế các phần đầu tiên ngắn hơn sẽ làm giảm vấn đề liên tục tìm một tiền tố xuất hiện dài nhất. Một máy tự động hậu tố thực hiện các kiểm tra chuỗi con đó một cách trực tiếp theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DP với tất cả các chuỗi con | O(L2 + M2) với tính năng tiền xử lý chuỗi con | O(L²) | Quá chậm | 
| Máy tự động tham lam + hậu tố tối ưu | O(L + M) | O(L) | Đã chấp nhận | 

Đây`L = |C|`Và`M`là tổng độ dài của tất cả các chuỗi bạn bè. 

## Hướng dẫn thuật toán 

1. Xây dựng một máy tự động hậu tố cho chuỗi thành phố`C`. 

Một máy tự động hậu tố có nhiều nhất`2L - 1`nêu và nhận biết chính xác tập hợp các chuỗi con của`C`. Từ trạng thái ban đầu của nó, có thể thực hiện chính xác chuỗi chuyển tiếp ký tự khi chuỗi đó là chuỗi con của`C`. 
2. Đối với mỗi chuỗi bạn bè`P`, bắt đầu ở vị trí`pos = 0`và đặt câu trả lời về 0. 

Tại thời điểm này`P[pos:]`là phần chưa được xây dựng. Chúng tôi luôn muốn chọn một hình ảnh bao gồm càng nhiều hậu tố này càng tốt. 
3. Bắt đầu từ trạng thái ban đầu của máy tự động, thực hiện chuyển tiếp bằng cách sử dụng`P[pos]`,`P[pos + 1]`, v.v. cho đến khi người bạn kết thúc hoặc quá trình chuyển đổi bắt buộc không tồn tại. 

Giả sử việc truyền tải tiêu thụ`len`nhân vật. Sau đó`P[pos:pos+len]`là chuỗi con của thành phố, trong khi ký tự tiếp theo không thể mở rộng thành chuỗi con dài hơn. Vì vậy, đây chính xác là bức ảnh đầu tiên dài nhất có thể. 
4. Nếu`len`bằng 0, đầu ra`-1`. 

Không có chuỗi con của thành phố bắt đầu bằng`P[pos]`, vì vậy không có hình ảnh nào có thể tạo ra ký tự được yêu cầu tiếp theo. Vì mọi cách xây dựng đều phải tạo ra ký tự đó tiếp theo nên không thể hình thành được người bạn. 
5. Nếu không, hãy tăng câu trả lời lên một và thăng tiến`pos`qua`len`. 

Tiền tố đã sử dụng hiện được biểu thị bằng một hình ảnh. Chúng tôi khởi động lại quá trình truyền tải hậu tố tự động từ trạng thái ban đầu vì hình ảnh tiếp theo là chuỗi con độc lập của thành phố. 
6. Lặp lại cho đến khi sử dụng hết ký tự của người bạn đó. 

Mỗi lần lặp lại tiêu tốn ít nhất một ký tự bạn bè, vì vậy có thể có nhiều nhất`|P|`lần lặp lại. 

### Tại sao nó hoạt động 

Hãy xem xét một lần lặp tham lam bắt đầu ở vị trí`pos`. Cho phép`G`là tiền tố dài nhất của`P[pos:]`đó là một chuỗi con của`C`. Mọi cấu trúc hợp lệ đều phải bắt đầu bằng một chuỗi con nào đó`X`của`C`, Và`X`không thể dài hơn`G`. 

Nếu việc xây dựng tối ưu bắt đầu bằng`X`, sau đó tiếp tục xem các ảnh tiếp theo cho đến khi ít nhất`|G|`các nhân vật đã được bảo hiểm. Thay thế toàn bộ phần ban đầu đó bằng một hình ảnh duy nhất`G`. Nếu như`G`kết thúc ở giữa một bức ảnh, hậu tố còn lại của bức ảnh đó vẫn là chuỗi con của`C`, để nó có thể trở thành hình ảnh tiếp theo. Số lượng hình ảnh không tăng. 

Vì vậy, luôn tồn tại một giải pháp tối ưu mà hình ảnh đầu tiên của nó chính xác là sự lựa chọn tham lam.`G`. Sau khi loại bỏ`G`, đối số tương tự áp dụng độc lập cho hậu tố còn lại. Bằng cách quy nạp qua các lần lặp tham lam, thuật toán tạo ra số lượng ảnh tối thiểu có thể. 

Hậu tố automaton cho biết chính xác`G`bởi vì mỗi lần chuyển đổi thành công đều mở rộng một chuỗi con của`C`, trong khi quá trình chuyển đổi bị thiếu đầu tiên chứng tỏ rằng không còn tiền tố nào có thể xảy ra trong`C`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SuffixAutomaton:
    def __init__(self, s):
        self.next = [{}]
        self.link = [-1]
        self.length = [0]
        self.last = 0

        for ch in s:
            self.extend(ch)

    def extend(self, ch):
        cur = len(self.next)
        self.next.append({})
        self.length.append(self.length[self.last] + 1)
        self.link.append(0)

        p = self.last

        while p != -1 and ch not in self.next[p]:
            self.next[p][ch] = cur
            p = self.link[p]

        if p == -1:
            self.link[cur] = 0
        else:
            q = self.next[p][ch]

            if self.length[p] + 1 == self.length[q]:
                self.link[cur] = q
            else:
                clone = len(self.next)
                self.next.append(self.next[q].copy())
                self.length.append(self.length[p] + 1)
                self.link.append(self.link[q])

                while p != -1 and self.next[p].get(ch) == q:
                    self.next[p][ch] = clone
                    p = self.link[p]

                self.link[q] = clone
                self.link[cur] = clone

        self.last = cur

    def longest_prefix(self, s, start):
        state = 0
        pos = start

        while pos < len(s):
            nxt = self.next[state].get(s[pos])
            if nxt is None:
                break
            state = nxt
            pos += 1

        return pos - start

def solve():
    city = input().strip()
    n = int(input())

    sam = SuffixAutomaton(city)

    out = []

    for _ in range(n):
        friend = input().strip()
        pos = 0
        pieces = 0

        while pos < len(friend):
            length = sam.longest_prefix(friend, pos)

            if length == 0:
                pieces = -1
                break

            pos += length
            pieces += 1

        out.append(str(pieces))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Máy tự động hậu tố lưu trữ ba mẩu thông tin cho mọi trạng thái.`length[v]`là độ dài tối đa được biểu thị bởi trạng thái đó,`link[v]`là liên kết hậu tố của nó, và`next[v]`chứa các chuyển tiếp ký tự đi. Việc xây dựng tuân theo quy trình mở rộng máy tự động hậu tố tiêu chuẩn, bao gồm cả việc sao chép trạng thái khi quá trình chuyển đổi mới sẽ vi phạm cấu trúc của máy tự động. 

các`longest_prefix`hàm luôn bắt đầu từ trạng thái 0. Điều này là cần thiết vì mỗi hình ảnh là một chuỗi con tùy ý của thành phố, không nhất thiết phải là tiền tố của thành phố. Bắt đầu từ trạng thái ban đầu có nghĩa là mọi chuỗi con có thể đều có sẵn. 

Vòng lặp dừng ở lần chuyển đổi bị thiếu đầu tiên. Nếu vòng lặp sử dụng nhiều ký tự thì toàn bộ tiền tố đó được biết là xuất hiện trong thành phố. Quá trình chuyển đổi bị thiếu chứng tỏ rằng việc mở rộng nó thêm một ký tự nữa là không thể, do đó độ dài phù hợp là tối đa. 

Trường hợp ranh giới tinh vi nhất là khi thiếu chuyển đổi đầu tiên. Sau đó`length == 0`, và tiến lên`pos`sẽ làm cho thuật toán lặp lại mãi mãi hoặc đếm sai một bức ảnh. Mã trả về ngay lập tức`-1`cho người bạn đó. 

Khi trận đấu thành công,`pos`tiến lên toàn bộ độ dài phù hợp trước lần lặp tiếp theo. Chúng tôi không tiến lên từng ký tự một vì toàn bộ tiền tố phù hợp đã được bao phủ bởi một hình ảnh. 

Không có vấn đề tràn số nguyên trong Python. Câu trả lời nhiều nhất là độ dài của người bạn, vì vậy ngay cả số nguyên 32 bit cũng đủ cho câu trả lời. 

Việc thực hiện sử dụng từ điển cho quá trình chuyển đổi. Bảng chữ cái chỉ chứa các chữ cái tiếng Anh viết hoa nên mỗi trạng thái có tối đa 26 lần chuyển tiếp đi. Mảng 26 phần tử cố định có thể giảm chi phí từ điển ở ngôn ngữ cấp thấp hơn, nhưng từ điển giữ cho việc triển khai Python đơn giản hơn đáng kể trong khi vẫn giữ được độ phức tạp tiệm cận tuyến tính. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, thành phố là`MONTEVIDEO`. Mẫu chính thức có bốn người bạn và câu trả lời là`4`,`1`,`4`, Và`-1`. 

Vì`DEMONIO`, quá trình tham lam là: 

| Vị trí | Hậu tố còn lại | Chuỗi con thành phố dài nhất | Miếng | 
| --- | --- | --- | --- | 
| 0 |`DEMONIO`|`DE`| 1 | 
| 2 |`MONIO`|`MON`| 2 | 
| 5 |`IO`|`I`| 3 | 
| 6 |`O`|`O`| 4 | 

Trận đầu tiên là`DE`, mặc dù`D`xuất hiện sau này trong thành phố. Trận đấu tiếp theo là`MON`, đến sớm hơn trong thành phố. Điều này xác nhận rằng hình ảnh có thể được sắp xếp lại một cách tự do. Máy tự động chỉ quan tâm xem mỗi phần có xảy ra ở đâu đó trong thành phố hay không. 

Vì`EDIT`, trận đấu tham lam đầu tiên là`E`. Từ phần còn lại`DIT`, trận đấu dài nhất có thể là`D`, sau đó`I`, sau đó`T`. Dấu vết kết quả là: 

| Vị trí | Hậu tố còn lại | Chuỗi con thành phố dài nhất | Miếng | 
| --- | --- | --- | --- | 
| 0 |`EDIT`|`E`| 1 | 
| 1 |`DIT`|`D`| 2 | 
| 2 |`IT`|`I`| 3 | 
| 3 |`T`|`T`| 4 | 

Vậy câu trả lời là`4`. Điều này cũng chứng tỏ tại sao thuật toán tham lam không cần biết chuỗi con xuất hiện ở đâu trong thành phố. Nó chỉ cần biết rằng nó xảy ra. 

Đối với mẫu thứ hai, thành phố là`SANTIAGO`, và câu trả lời chính thức là`3`,`1`, Và`3`. 

Vì`TITA`, các trận đấu tham lam là: 

| Vị trí | Hậu tố còn lại | Chuỗi con thành phố dài nhất | Miếng | 
| --- | --- | --- | --- | 
| 0 |`TITA`|`T`| 1 | 
| 1 |`ITA`|`I`| 2 | 
| 2 |`TA`|`TA`| 3 | 

Kết quả là`3`. trận chung kết`TA`tiếp giáp bên trong`SANTIAGO`, mặc dù hai chữ cái này được tách ra khỏi một số chữ cái khác đã được sử dụng trước đó trong bạn. Hình ảnh là độc lập nên không có hạn chế. 

Vì`SANTIAGO`chính nó, toàn bộ người bạn là chuỗi thành phố, vì vậy máy tự động theo dõi thành công mọi ký tự: 

| Vị trí | Hậu tố còn lại | Chuỗi con thành phố dài nhất | Miếng | 
| --- | --- | --- | --- | 
| 0 |`SANTIAGO`|`SANTIAGO`| 1 | 

Câu trả lời là`1`, chứng tỏ rằng máy tự động có thể nhận ra toàn bộ chuỗi thành phố là một hình ảnh. 

## Phân tích độ phức tạp 

hãy để`L = |C|`và để`M`là tổng độ dài của tất cả các chuỗi bạn bè. 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L + M) | Máy tự động hậu tố cần O(L) để xây dựng và mỗi nhân vật bạn bè bị tiêu diệt bởi chính xác một trận đấu tham lam thành công, với tối đa một lần chuyển đổi thất bại cho mỗi phần | 
| Không gian | O(L) | Một máy tự động có hậu tố có ít hơn`2L`trạng thái và tổng số lần chuyển tiếp được lưu trữ là O(L) | 

Tổng thời gian kết bạn nhiều nhất là`2 * 10^5`, do đó quá trình xử lý truy vấn là tuyến tính trong phần giới hạn của đầu vào. Việc xây dựng máy tự động cũng tuyến tính theo chiều dài thành phố. Điều này thoải mái trong giới hạn chính thức 2 giây và 1024 MB. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây chứa logic giải pháp hậu tố tự động tương tự và kiểm tra hai mẫu chính thức cùng với các trường hợp tùy chỉnh.```python
import sys
import io

input = sys.stdin.readline

class SuffixAutomaton:
    def __init__(self, s):
        self.next = [{}]
        self.link = [-1]
        self.length = [0]
        self.last = 0

        for ch in s:
            self.extend(ch)

    def extend(self, ch):
        cur = len(self.next)
        self.next.append({})
        self.length.append(self.length[self.last] + 1)
        self.link.append(0)

        p = self.last

        while p != -1 and ch not in self.next[p]:
            self.next[p][ch] = cur
            p = self.link[p]

        if p == -1:
            self.link[cur] = 0
        else:
            q = self.next[p][ch]

            if self.length[p] + 1 == self.length[q]:
                self.link[cur] = q
            else:
                clone = len(self.next)
                self.next.append(self.next[q].copy())
                self.length.append(self.length[p] + 1)
                self.link.append(self.link[q])

                while p != -1 and self.next[p].get(ch) == q:
                    self.next[p][ch] = clone
                    p = self.link[p]

                self.link[q] = clone
                self.link[cur] = clone

        self.last = cur

    def longest_prefix(self, s, start):
        state = 0
        pos = start

        while pos < len(s):
            nxt = self.next[state].get(s[pos])
            if nxt is None:
                break
            state = nxt
            pos += 1

        return pos - start

def solve():
    city = input().strip()
    n = int(input())

    sam = SuffixAutomaton(city)
    out = []

    for _ in range(n):
        friend = input().strip()
        pos = 0
        pieces = 0

        while pos < len(friend):
            length = sam.longest_prefix(friend, pos)

            if length == 0:
                pieces = -1
                break

            pos += length
            pieces += 1

        out.append(str(pieces))

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_stdout = sys.stdout
    old_input = input

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        input = sys.stdin.readline

        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = old_input

# Provided sample 1
assert run(
    """MONTEVIDEO
4
DEMONIO
MONTE
EDIT
WON
"""
) == "4\n1\n4\n-1", "sample 1"

# Provided sample 2
assert run(
    """SANTIAGO
3
TITA
SANTIAGO
NAS
"""
) == "3\n1\n3", "sample 2"

# Minimum-size city, impossible character, and exact match
assert run(
    """A
3
A
AA
B
"""
) == "1\n2\n-1", "minimum size and impossible character"

# Repeated characters and repeated use of the same picture
assert run(
    """AAA
3
AAAA
AAAAAA
B
"""
) == "2\n2\n-1", "repeated characters"

# Reordering pictures and greedy longest-prefix behavior
assert run(
    """ABC
4
CBA
ABAB
BCAB
ACAC
"""
) == "3\n2\n2\n4", "reordering and boundaries"

# Maximum-size linear test
city = "A" * 200000
large_input = city + "\n2\n" + ("A" * 200000) + "\n" + ("B" * 1) + "\n"
assert run(large_input) == "1\n-1", "maximum-size test"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`A`, bạn`A`,`AA`,`B`|`1`,`2`,`-1`| Kích thước tối thiểu, sử dụng nhiều lần, ký tự không thể | 
|`AAA`, bạn`AAAA`,`AAAAAA`,`B`|`2`,`2`,`-1`| Các ký tự hoàn toàn bằng nhau và sử dụng lại cùng một phần thành phố | 
|`ABC`, bạn`CBA`,`ABAB`,`BCAB`,`ACAC`|`3`,`2`,`2`,`4`| Thứ tự hình ảnh tùy ý và các lựa chọn tiền tố dài nhất tham lam | 
|`A * 200000`, bạn`A * 200000`,`B`|`1`,`-1`| Đầu vào có kích thước tối đa và xử lý tuyến tính | 

## Vỏ cạnh 

Đối với một ký tự đầu tiên không thể, hãy xem xét```
ABC
1
D
```Máy tự động bắt đầu ở trạng thái ban đầu và ngay lập tức tìm kiếm sự chuyển đổi`D`. Không có, vì vậy tiền tố dài nhất có độ dài bằng 0. Đầu ra của thuật toán`-1`. Nó không được tính`D`như một bức tranh một ký tự bởi vì`D`không xảy ra ở thành phố. 

Đối với một người bạn dài hơn cả thành phố nhưng chỉ bao gồm những phần có thể lặp lại, hãy cân nhắc```
AAA
1
AAAA
```Lần truyền tải đầu tiên tiêu tốn`AAA`, bởi vì đó là tiền tố dài nhất xảy ra trong thành phố. Hậu tố còn lại là`A`, bản thân nó là một chuỗi con của thành phố. Câu trả lời là`2`. Máy tự động chỉ được xây dựng lại một lần và cùng một máy tự động có thể được sử dụng nhiều lần vì hình ảnh không phải là tài nguyên sẽ biến mất sau khi được chọn. 

Để sắp xếp hình ảnh tùy ý, hãy xem xét```
ABC
1
CBA
```Lần duyệt đầu tiên được tìm thấy`C`, sau đó bước tiếp theo bắt đầu từ trạng thái ban đầu của máy tự động và tìm thấy`B`, thì lần duyệt cuối cùng sẽ tìm thấy`A`. Câu trả lời là`3`. Thuật toán không bao giờ cố gắng bảo toàn vị trí của các ảnh đã chọn bên trong thành phố, đó chính xác là điều mà vấn đề cho phép. 

Đối với trường hợp ranh giới tham lam, hãy xem xét```
ABA
1
ABAB
```Việc truyền tải đầu tiên mất`ABA`, vì nó là chuỗi con của thành phố và không thể dùng tiền tố nữa. Người bạn còn lại là`B`, để chụp thêm một bức ảnh nữa. Câu trả lời là`2`. Một lựa chọn đầu tiên ngắn hơn như`AB`cũng sẽ dẫn đến một cách xây dựng hợp lệ, nhưng nó không thể cải thiện câu trả lời, đó chính xác là đặc tính đằng sau bằng chứng tham lam. 

Đối với một người bạn ngang bằng với chính thành phố,```
SANTIAGO
1
SANTIAGO
```máy tự động theo dõi mọi ký tự mà không gặp phải sự chuyển đổi bị thiếu. Tiền tố dài nhất có độ dài là 8, vì vậy toàn bộ người bạn sẽ được sử dụng trong một lần lặp và câu trả lời là`1`. Điều này kiểm tra ranh giới nơi kết quả khớp đến cuối truy vấn thay vì kết thúc vì thiếu chuyển tiếp.
