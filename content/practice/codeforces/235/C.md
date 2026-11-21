---
title: "CF 235C - Nhiệm vụ theo chu kỳ"
description: "Chúng ta được cấp một chuỗi văn bản lớn s, được cố định cho toàn bộ dữ liệu đầu vào. Khi đó chúng ta nhận được nhiều chuỗi truy vấn x. Đối với truy vấn x, chúng ta không chỉ quan tâm đến bản thân x. Mọi phép quay theo chu kỳ của x đều được coi là tương đương."
date: "2026-06-04T16:31:32+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "string-suffix-structures", "strings"]
categories: ["algorithms"]
codeforces_contest: 235
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 146 (Div. 1)"
rating: 2700
weight: 235
solve_time_s: 156
verified: true
draft: false
---

[CF 235C - Nhiệm vụ theo chu kỳ](https://codeforces.com/problemset/problem/235/C) 

**Đánh giá:** 2700 
**Tags:** cấu trúc dữ liệu, cấu trúc hậu tố chuỗi, chuỗi 
**Thời gian giải:** 2 phút 36 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi văn bản lớn`s`, được cố định cho toàn bộ đầu vào. Sau đó chúng tôi nhận được nhiều chuỗi truy vấn`x`. 

Đối với một truy vấn`x`, chúng tôi không chỉ quan tâm đến`x`chính nó. Bất kỳ vòng quay tuần hoàn nào của`x`được coi là tương đương. Ví dụ, đối với`x = "abcd"`, các dây`"abcd"`,`"bcda"`,`"cdab"`, Và`"dabc"`tất cả đều thuộc cùng một lớp tuần hoàn. 

Đối với mỗi truy vấn, chúng ta phải đếm có bao nhiêu chuỗi con của`s`đều bằng bất kỳ phép quay nào của`x`. 

Chiều dài của`s`có thể đạt được`10^6`và tổng độ dài của tất cả các truy vấn cũng tối đa`10^6`. Giải pháp quét toàn bộ văn bản riêng biệt cho từng truy vấn là không thể ngay lập tức. Thậm chí một`O(|s|)`thủ tục cho mỗi truy vấn sẽ yêu cầu khoảng`10^12`hoạt động trong trường hợp xấu nhất. 

Các ràng buộc gợi ý một bước tiền xử lý nặng nề trên`s`, tiếp theo là xử lý gần như tuyến tính trong tổng chiều dài truy vấn. 

Có một số tình huống tế nhị mà một giải pháp ngây thơ có thể xử lý sai. 

Coi như:```
s = "aaaaa"
x = "aa"
```Các vòng quay của`"aa"`vẫn còn`"aa"`. Nếu chúng tôi tạo tất cả các phép quay và tính tổng số lần xuất hiện của chúng, chúng tôi sẽ đếm cùng một chuỗi hai lần. Câu trả lời đúng là`4`, không`8`. 

Một ví dụ khác:```
s = "ababab"
x = "abab"
```Các phép quay là:```
abab
baba
abab
baba
```Chỉ có hai chuỗi riêng biệt tồn tại. Đếm tất cả bốn phép quay riêng biệt sẽ tạo ra các bản sao. 

Một cạm bẫy khác xuất hiện khi chuyển động quay không xảy ra trong`s`không hề. Ví dụ:```
s = "abc"
x = "de"
```Mỗi vòng quay sẽ đóng góp bằng 0 và thuật toán phải phát hiện điều này một cách hiệu quả mà không cần tìm kiếm toàn bộ văn bản. 

Khó khăn chính là xử lý đồng thời tất cả các sự cố xảy ra trong`s`, tất cả các phép quay theo chu kỳ của truy vấn và các phép quay trùng lặp. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. 

Đối với một truy vấn có độ dài`m`, tạo ra tất cả`m`các phép quay. Với mỗi vòng quay, hãy đếm xem nó xuất hiện bao nhiêu lần trong`s`. Một mảng hậu tố, máy tự động hậu tố hoặc cấu trúc băm có thể trả lời một truy vấn xuất hiện một cách nhanh chóng. 

Vấn đề là một truy vấn có độ dài`m`có`m`các phép quay. Vì tổng chiều dài truy vấn có thể đạt tới`10^6`, tổng số vòng quay được tạo có thể đạt khoảng`10^12`. Ngay cả khi mỗi vòng quay được xử lý trong thời gian không đổi, điều này vẫn vượt xa giới hạn. 

Quan sát quan trọng là tất cả các phép quay của một chuỗi xuất hiện dưới dạng chiều dài-`m`chuỗi con bên trong`x + x`. 

Ví dụ:```
x      = abcd
x + x  = abcdabcd
```Các phép quay chính xác là các chuỗi con có độ dài 4 có vị trí bắt đầu nằm ở nửa đầu. 

Điều này biến vấn đề thành: 

> Trong số tất cả các chiều dài khác nhau-`m`chuỗi con của`x + x`tương ứng với các phép quay, tính tổng số lần xuất hiện của chúng bên trong`s`. 

Bây giờ một máy tự động hậu tố trở nên rất tự nhiên. 

Sau khi xây dựng một máy tự động hậu tố trên`s`, mọi trạng thái biểu diễn một lớp tương đương vị trí cuối. Nếu chúng ta truyền bá số lần xuất hiện thông qua các liên kết hậu tố, thì mỗi trạng thái sẽ lưu trữ số lần xuất hiện của mỗi chuỗi được biểu thị bởi trạng thái đó. 

Trong khi quét`x + x`, chúng tôi có thể duy trì hậu tố dài nhất hiện được khớp trong máy tự động. Bất cứ khi nào độ dài phù hợp ít nhất là`m`, chúng ta biết rằng một phép quay có chiều dài`m`kết thúc ở đây. 

Thử thách còn lại là xác định trạng thái tương ứng với độ dài chính xác đó.`m`sợi dây. 

Trong một máy tự động có hậu tố, một trạng thái`v`đại diện cho tất cả các độ dài trong khoảng:```
(len(link(v)) + 1) ... len(v)
```Vì vậy, đối với một chuỗi có độ dài chính xác`m`, chúng tôi leo lên các liên kết hậu tố cho đến khi độ dài gốc trở nên nhỏ hơn`m`. Trạng thái kết quả đại diện duy nhất cho độ dài đó-`m`chuỗi con. 

Các lần xuất hiện khác nhau của cùng một vòng quay có thể xuất hiện nhiều lần trong khi quét`x + x`. Chúng tôi chỉ muốn mỗi vòng quay riêng biệt một lần, vì vậy, chúng tôi đánh dấu thời gian và đếm từng trạng thái nhiều nhất một lần cho mỗi truy vấn. 

Điều này mang lại một giải pháp tuyến tính trong tổng chiều dài truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng chiều dài truy vấn²) hoặc tệ hơn | O(1) | Quá chậm | 
| Giải Pháp SAM Tối Ưu | O( | s | + tổng chiều dài truy vấn) | 

## Hướng dẫn thuật toán 

1. Xây dựng automaton hậu tố cho chuỗi văn bản`s`. 
2. Đối với mỗi trạng thái không phải bản sao mới được tạo, hãy khởi tạo số lần xuất hiện của nó thành`1`. 
3. Sắp xếp các trạng thái theo độ dài và truyền số đếm từ các trạng thái dài hơn đến trạng thái liên kết hậu tố cha của chúng. Sau bước này, mỗi trạng thái sẽ lưu trữ số lần xuất hiện của chuỗi con được biểu diễn bên trong`s`. 
4. Đối với một truy vấn`x`chiều dài`m`, xây dựng`t = x + x`. 
5. Chỉ quét cái đầu tiên`2m - 1`nhân vật của`t`. Những vị trí này tạo ra chính xác`m`các phép quay tuần hoàn. 
6. Bảo trì: 

-`p`, trạng thái tự động hiện tại. 
-`cur_len`, độ dài của hậu tố phù hợp dài nhất. 
7. Đối với mỗi ký tự, thực hiện quá trình chuyển đổi khớp tự động hậu tố tiêu chuẩn. Nếu quá trình chuyển đổi không tồn tại, hãy theo các liên kết hậu tố cho đến khi nó tồn tại hoặc quay lại thư mục gốc. 
8. Bất cứ khi nào`cur_len >= m`, leo lên các liên kết hậu tố trong khi`len(link(p)) >= m`. 

Sau lần điều chỉnh này, trạng thái`p`là trạng thái duy nhất có khoảng chứa độ dài`m`. Trạng thái đó đại diện cho vòng quay hiện tại. 
9. Nếu trạng thái này chưa được tính trong truy vấn hiện tại, hãy thêm số lần xuất hiện của nó vào câu trả lời và đánh dấu nó bằng dấu thời gian truy vấn hiện tại. 
10. Xuất ra câu trả lời tích lũy. 

### Tại sao nó hoạt động 

Mỗi vòng quay tuần hoàn của`x`xuất hiện chính xác một lần dưới dạng độ dài-`m`chuỗi con kết thúc ở vị trí nào đó bên trong chuỗi đầu tiên`2m - 1`nhân vật của`x + x`. 

Trong quá trình quét, máy tự động hậu tố duy trì hậu tố dài nhất của tiền tố được xử lý xảy ra trong`s`. Khi hậu tố phù hợp đó có độ dài ít nhất`m`, chiều dài-`m`hậu tố kết thúc ở vị trí hiện tại là phép quay của`x`. 

Trạng thái tự động hậu tố biểu thị tất cả độ dài chuỗi con trong một khoảng liền kề. Leo lên các liên kết hậu tố cho đến khi chiều dài gốc trở nên nhỏ hơn`m`chọn trạng thái duy nhất có khoảng chứa độ dài`m`. Trạng thái đó tương ứng chính xác với vòng quay hiện tại. 

Số lần xuất hiện được lưu trữ ở trạng thái đó bằng số lần chuỗi biểu diễn xuất hiện trong`s`. Các phép quay trùng lặp ánh xạ tới cùng một trạng thái và việc đánh dấu thời gian sẽ ngăn việc đếm chúng nhiều lần. Do đó, mỗi phép quay riêng biệt đóng góp chính xác một lần và câu trả lời là đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

max_states = 2 * len(s) + 5

link = [-1] * max_states
length = [0] * max_states
occ = [0] * max_states
nxt = [[0] * 26 for _ in range(max_states)]

size = 1
last = 0
link[0] = -1

for ch in s:
    c = ord(ch) - 97

    cur = size
    size += 1

    length[cur] = length[last] + 1
    occ[cur] = 1

    p = last
    while p != -1 and nxt[p][c] == 0:
        nxt[p][c] = cur
        p = link[p]

    if p == -1:
        link[cur] = 0
    else:
        q = nxt[p][c]
        if length[p] + 1 == length[q]:
            link[cur] = q
        else:
            clone = size
            size += 1

            length[clone] = length[p] + 1
            link[clone] = link[q]
            nxt[clone] = nxt[q][:]

            while p != -1 and nxt[p][c] == q:
                nxt[p][c] = clone
                p = link[p]

            link[q] = clone
            link[cur] = clone

    last = cur

max_len = len(s)

cnt = [0] * (max_len + 1)
for v in range(size):
    cnt[length[v]] += 1

for i in range(1, max_len + 1):
    cnt[i] += cnt[i - 1]

order = [0] * size
for v in range(size - 1, -1, -1):
    cnt[length[v]] -= 1
    order[cnt[length[v]]] = v

for i in range(size - 1, 0, -1):
    v = order[i]
    parent = link[v]
    if parent >= 0:
        occ[parent] += occ[v]

vis = [0] * size
timer = 0

q = int(input())

out = []

for _ in range(q):
    x = input().strip()
    m = len(x)

    t = x + x

    timer += 1
    ans = 0

    p = 0
    cur_len = 0

    for i in range(2 * m - 1):
        c = ord(t[i]) - 97

        while p != -1 and nxt[p][c] == 0:
            p = link[p]

        if p == -1:
            p = 0
            cur_len = 0
            continue

        cur_len = min(cur_len, length[p]) + 1
        p = nxt[p][c]

        while link[p] != -1 and length[link[p]] >= m:
            p = link[p]

        if cur_len >= m and vis[p] != timer:
            vis[p] = timer
            ans += occ[p]

    out.append(str(ans))

sys.stdout.write("\n".join(out))
```Phần đầu tiên xây dựng một máy tự động hậu tố cho chuỗi văn bản. Mọi trạng thái không sao chép đều bắt đầu bằng số lần xuất hiện`1`, tương ứng với một vị trí kết thúc hậu tố. 

Thứ tự vượt qua sắp xếp đếm nêu theo độ dài tăng dần. Việc xử lý thứ tự này ngược lại sẽ truyền số lần xuất hiện thông qua các liên kết hậu tố, tạo ra số liệu thống kê về số lần xuất hiện SAM tiêu chuẩn. 

Đối với mỗi truy vấn, chúng tôi quét`x + x`trong khi vẫn duy trì hậu tố dài nhất hiện xuất hiện trong máy tự động. bản cập nhật```
cur_len = min(cur_len, length[p]) + 1
```là công thức so khớp SAM tiêu chuẩn. Nó điều chỉnh chính xác độ dài phù hợp hiện tại sau khi nhảy liên kết hậu tố. 

Vòng lặp```
while link[p] != -1 and length[link[p]] >= m:
    p = link[p]
```là bước quan trọng. Sau khi kết thúc, hãy nêu`p`là trạng thái duy nhất có khoảng biểu diễn chứa độ dài`m`. 

Mảng dấu thời gian tránh xóa mảng boolean có kích thước lên tới hai triệu cho mỗi truy vấn. Mỗi truy vấn nhận được một mã định danh duy nhất và trạng thái chỉ được tính một lần cho mã định danh đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
s = baabaabaaa
x = baa
```Sau đó:```
x + x = baabaa
```Chúng tôi quét đầu tiên`2m - 1 = 5`nhân vật. 

| Vị trí | Nhân vật | Độ dài phù hợp ≥ 3? | Xoay đại diện | 
| --- | --- | --- | --- | 
| 0 | b | Không | - | 
| 1 | một | Không | - | 
| 2 | một | Có | trừu kêu | 
| 3 | b | Có | aab | 
| 4 | một | Có | aba | 

Các phép quay khác biệt là:```
baa
aab
aba
```Sự xuất hiện của chúng được tính vào`s`là:```
3 + 2 + 2 = 7
```Trả lời:```
7
```Ví dụ này cho thấy mỗi vòng quay xuất hiện như thế nào dưới dạng chiều dài-`m`cửa sổ trong`x + x`. 

### Ví dụ 2 

đầu vào:```
s = aaaaa
x = aa
```Sau đó:```
x + x = aaaa
```Chúng tôi quét ba ký tự đầu tiên. 

| Vị trí | Xoay | Đã tính chưa? | 
| --- | --- | --- | 
| 1 | aa | Không | 
| 2 | aa | Có | 

Mặc dù tồn tại hai vị trí xoay nhưng cả hai đều biểu thị cùng một chuỗi. Cơ chế dấu thời gian chỉ đếm nó một lần. 

Số lần xuất hiện của`"aa"`TRONG`"aaaaa"`là`4`, vậy đáp án là:```
4
```Ví dụ này giải thích tại sao các phép quay trùng lặp phải được loại bỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | s | 
| Không gian | O( | s | 

Độ dài văn bản và tổng độ dài truy vấn tối đa là`10^6`. Một máy tự động có hậu tố chứa tối đa khoảng`2 · 10^6`trạng thái phù hợp thoải mái trong giới hạn bộ nhớ. Thời gian chạy tuyến tính nằm trong khoảng 3 giây khả dụng để triển khai được tối ưu hóa cao. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    # paste solution into solve()
    pass

# provided sample
assert run(
"""baabaabaaa
5
a
ba
baa
aabaa
aaba
"""
) == """7
5
7
3
5"""

# single character
assert run(
"""a
1
a
"""
) == """1"""

# duplicate rotations
assert run(
"""aaaaa
1
aa
"""
) == """4"""

# no rotation appears
assert run(
"""abc
1
de
"""
) == """0"""

# periodic query, many identical rotations
assert run(
"""ababab
1
abab
"""
) == """3"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`s="a", x="a"`|`1`| Kích thước tối thiểu | 
|`s="aaaaa", x="aa"`|`4`| Xoay trùng lặp | 
|`s="abc", x="de"`|`0`| Thiếu chuỗi | 
|`s="ababab", x="abab"`|`3`| Luân chuyển và khử trùng lặp định kỳ | 

## Vỏ cạnh 

Hãy xem xét:```
aaaaa
1
aa
```Các phép quay là:```
aa
aa
```Cả hai phép quay đều tương ứng với cùng một chuỗi. Trong khi quét`x + x`, trạng thái tự động hậu tố giống nhau được đạt tới nhiều lần. Mảng dấu thời gian chỉ cho phép lượt truy cập đầu tiên đóng góp. Đầu ra của thuật toán`4`, đó là số lần xuất hiện của`"aa"`TRONG`"aaaaa"`. 

Coi như:```
abc
1
de
```Trong quá trình khớp, không có chuyển đổi nào cho`'d'`tồn tại từ gốc. Máy tự động ngay lập tức đặt lại độ dài trận đấu hiện tại về 0. Không có nhà nước nào thỏa mãn`cur_len >= m`, vì vậy không có gì được thêm vào câu trả lời. Kết quả là đúng`0`. 

Coi như:```
ababab
1
abab
```Bốn vị trí xoay chỉ tạo ra hai chuỗi riêng biệt:```
abab
baba
```Cả hai ánh xạ tới các trạng thái tự động hậu tố cố định. Các lượt truy cập lặp lại sẽ bị bỏ qua bằng cách đánh dấu thời gian, do đó, mỗi vòng quay riêng biệt sẽ đóng góp chính xác một lần. Câu trả lời là tổng số lần xuất hiện của hai chuỗi đó.
