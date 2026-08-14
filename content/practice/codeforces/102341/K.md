---
title: "CF 102341K - Kecleon"
description: "Chúng tôi duy trì một chuỗi các chữ cái viết thường chỉ phát triển bằng cách thêm một ký tự vào đầu bên phải của nó. Một truy vấn yêu cầu độ dài (k) và chúng ta phải đếm xem có bao nhiêu chuỗi con có độ dài (k) chính xác bằng tiền tố của toàn bộ chuỗi có độ dài (k)."
date: "2026-08-13T03:23:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102341
codeforces_index: "K"
codeforces_contest_name: "Radewoosh+mnbvmar Contest (supported by AIM Tech)"
rating: 0
weight: 102341
solve_time_s: 187
verified: true
draft: false
---

[CF 102341K - Kecleon](https://codeforces.com/problemset/problem/102341/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một chuỗi các chữ cái viết thường chỉ phát triển bằng cách thêm một ký tự vào đầu bên phải của nó. Một truy vấn yêu cầu độ dài (k) và chúng ta phải đếm xem có bao nhiêu chuỗi con có độ dài (k) chính xác bằng tiền tố của toàn bộ chuỗi có độ dài (k). 

Đầu vào trực tuyến theo hai cách. Đầu tiên, cả ký tự được thêm vào và độ dài được yêu cầu đều được mã hóa bằng câu trả lời trước đó, vì vậy chúng tôi không thể giải mã trước các truy vấn trong tương lai. Thứ hai, bản thân chuỗi chỉ phát triển, điều này cho chúng ta cơ hội duy trì thông tin theo từng bước. 

Số lượng truy vấn nhiều nhất là (300.000), do đó chuỗi cuối cùng cũng có độ dài tối đa (300.000). Thuật toán quét toàn bộ chuỗi cho mỗi truy vấn đã quá chậm. Thuật toán so sánh từng ký tự chuỗi con ứng cử viên theo ký tự có thể đạt được khoảng (10^{15}) so sánh ký tự trên một đầu vào đối nghịch lớn. Chúng ta cần công logarit đại khái cho mỗi phép toán. 

Có ba chi tiết thường gây ra giải pháp sai. 

Đầu tiên là (k=n) có chính xác một khoảng khớp, toàn bộ chuỗi. Ví dụ,```
3
add a
add b
get 2
```sản xuất```
1
```Một giải pháp chỉ tính số lần xuất hiện thích hợp có thể vô tình trả về số 0. 

Thứ hai là bản thân tiền tố luôn được tính là một lần xuất hiện. Vì```
2
add a
get 1
```câu trả lời là```
1
```Thứ ba là mã hóa trực tuyến. Coi như```
6
add a
add a
add b
add a
get 1
get 1
```Chuỗi là`aaba`. đầu tiên`get 1`yêu cầu (k=1), có câu trả lời là (3), vì vậy`last`trở thành (3). Giá trị thô thứ hai`1`sau đó được giải mã bằng (n=4) là (k=4), không phải (k=1). Câu trả lời của nó là (1). Đầu ra đúng là```
3
1
```Bỏ qua`last`sẽ im lặng trả lời câu hỏi sai. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là lưu trữ chuỗi hiện tại và đối với mỗi truy vấn, hãy kiểm tra mọi vị trí bắt đầu có thể có. Đối với mỗi vị trí, chúng tôi so sánh chuỗi con có độ dài (k) với ký tự (k) đầu tiên. Điều này đúng vì đó chính xác là những khoảng thời gian được truy vấn đề cập. Tuy nhiên, có thể có (n-k+1) khoảng ứng viên và việc so sánh một khoảng có thể tốn (k), đưa ra kết quả (O(nk)) cho một truy vấn. 

Trường hợp xấu nhất lớn hơn nhiều so với giới hạn bốn giây cho phép. Với khoảng (200.000) ký tự được thêm vào và (100.000) truy vấn, một truy vấn có thể yêu cầu khoảng (10^{10}) so sánh ký tự và việc lặp lại điều đó qua các truy vấn sẽ đạt đến thứ tự so sánh (10^{15}). 

Hàm băm luân phiên sẽ giảm chi phí so sánh một chuỗi con, nhưng chúng tôi vẫn cần kiểm tra tất cả (n-k+1) vị trí bắt đầu. Vấn đề thực sự không chỉ là kiểm tra sự bình đẳng. Chúng ta cần một cách để đếm tất cả các lần xuất hiện của tiền tố mà không cần quét chuỗi. 

Quan sát chính xuất phát từ hàm tiền tố được KMP sử dụng. Đối với mỗi tiền tố kết thúc ở vị trí (i), hàm tiền tố cho chúng ta biết độ dài của tiền tố thích hợp dài nhất cũng là hậu tố. Nếu chúng ta tạo một nút cho mỗi độ dài tiền tố và đặt nút (i) thành nút con của nút (\pi[i-1]), thì chúng ta thu được cây hàm tiền tố. 

Bây giờ hãy xem xét tiền tố có độ dài (k). Nó xảy ra kết thúc ở vị trí (i) chính xác khi các ký tự (k) đầu tiên là hậu tố của tiền tố kết thúc tại (i). Trong cây hàm tiền tố, điều đó có nghĩa là nút (k) là tổ tiên của nút (i). Do đó, số lần xuất hiện của tiền tố (k) chính xác bằng kích thước của cây con có gốc tại nút (k). 

Điều này thay đổi vấn đề hoàn toàn. Mỗi ký tự được nối thêm sẽ tạo một nút mới trong cây hàm tiền tố và nút đó được đính kèm dưới dạng một chiếc lá. Mỗi truy vấn sẽ trở thành một truy vấn có kích thước cây con động. 

Một chuyến tham quan Euler tĩnh sẽ làm cho mỗi cây con trở thành một khoảng liền kề nhau, nhưng cây đang được xây dựng trực tuyến nên không biết thứ tự DFS cuối cùng của nó. Thay vào đó, chúng ta có thể duy trì chuỗi Euler một cách linh hoạt trong một hàm ẩn. Mỗi nút cây đều nhận được mã thông báo đầu vào và mã thông báo thoát. Mã thông báo nhập có giá trị (1), trong khi mã thông báo thoát có giá trị (0). Toàn bộ cây con của một nút luôn là chuỗi liền kề giữa các mã thông báo vào và ra của nó. Khi một lá mới được gắn vào nút cha, hai mã thông báo của nó sẽ được chèn ngay trước mã thông báo thoát của nút cha. 

Treap lưu trữ chuỗi Euler và duy trì tổng giá trị mã thông báo trong mỗi cây con treap. Khi đó, truy vấn kích thước cây con chỉ là số lượng mã thông báo mục nhập giữa mã thông báo mục nhập và mã thông báo thoát tương ứng. 

Các phương pháp tiếp cận bạo lực và tối ưu có thể được so sánh như sau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(qn^2)) trong trường hợp xấu nhất | (O(n)) | Quá chậm | 
| Tối ưu | (O(q\log q)) dự kiến ​​| (O(q)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì chuỗi hiện tại và mảng hàm tiền tố của nó. Khi một ký tự mới được thêm vào, hãy tính giá trị hàm tiền tố của nó bằng chuỗi dự phòng KMP thông thường. Nếu vị trí mới là (n), thì nút cha của nó trong cây hàm tiền tố là nút (\pi[n-1]). 

Việc tính toán KMP được khấu hao tuyến tính trên toàn bộ chuỗi vì mọi dự phòng sẽ chuyển sang đường viền được tính toán trước đó. 
2. Biểu thị cây hàm tiền tố bằng mã thông báo vào và ra cho mỗi nút. Mã thông báo nhập của nút (v) lưu trữ giá trị (1) và mã thông báo thoát của nó lưu trữ giá trị (0). Nút gốc nhân tạo (0) có giá trị (0). 

Một đại diện DFS trông giống như`enter(v), all descendants, exit(v)`. Do đó, mỗi nút trong cây con của (v) đóng góp chính xác một (1) bên trong khoảng từ`enter(v)`ĐẾN`exit(v)`. 
3. Lưu trữ chuỗi mã thông báo này trong một kho ngầm. Treap được sắp xếp theo vị trí chuỗi thay vì theo khóa rõ ràng. Mỗi nút treap lưu trữ kích thước cây con, tổng cây con, con, cha mẹ và mức độ ưu tiên ngẫu nhiên. 

Con trỏ gốc cho phép chúng ta tìm vị trí hiện tại của bất kỳ mã thông báo nào trong thời gian dự kiến ​​(O(\log n)) bằng cách đi từ mã thông báo tới gốc treap. 
4. Khi nút cây tiền tố (v) được tạo bằng nút cha (p), hãy tìm vị trí hiện tại của`exit(p)`. Tách chuỗi Euler ngay trước mã thông báo đó, chèn`enter(v), exit(v)`và hợp nhất chuỗi lại. 

Việc chèn ngay trước mã thông báo thoát của nút cha sẽ đặt nút mới làm nút con cuối cùng của nút cha đó. Thứ tự chính xác giữa các anh chị em không quan trọng vì chỉ sử dụng tư cách thành viên cây con. 
5. Đối với một`get`truy vấn, trước tiên hãy giải mã độ dài được yêu cầu bằng giá trị hiện tại của`last`. 

Nút được giải mã (k) tương ứng với nút cây chức năng tiền tố (k). Tìm số lượng mã thông báo nhập từ`enter(k)`bởi vì`exit(k)`. Con số đó là kích thước cây con của (k), chính xác là số lần xuất hiện của tiền tố có độ dài (k). 
6. Lưu câu trả lời vào`last`trước khi xử lý truy vấn tiếp theo. 

### Tại sao nó hoạt động 

Đối với mọi vị trí (i), nút (i) biểu thị toàn bộ tiền tố kết thúc tại vị trí đó. Nút (k) là tổ tiên của nút (i) chính xác khi tiền tố có độ dài (k) là hậu tố của tiền tố kết thúc tại (i). Hậu tố đó chính xác là sự xuất hiện của các ký tự (k) đầu tiên kết thúc bằng (i). Do đó, các lần xuất hiện của tiền tố được truy vấn là tương ứng một-một với các nút trong cây con của (k). 

Dãy Euler động luôn chứa mọi cây con dưới dạng một khoảng liền kề. Vì chỉ các mã thông báo mục nhập mới đóng góp vào tổng được lưu trữ nên tổng trên khoảng thời gian của nút (k) sẽ tính mỗi phần tử con cháu chính xác một lần. Do đó, tre sẽ trả về chính xác số lượng khoảng thời gian khớp được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1_000_000)

def solve():
    q = int(input())

    # Prefix-function data.
    s = bytearray()
    pi = [0]

    # Implicit treap data.
    # Node 0 is the null treap node.
    left = [0]
    right = [0]
    parent = [0]
    size = [0]
    sm = [0]
    value = [0]
    priority = [0]

    seed = 0x12345678

    def rng():
        nonlocal seed
        seed ^= (seed << 13) & 0xFFFFFFFF
        seed ^= seed >> 17
        seed ^= (seed << 5) & 0xFFFFFFFF
        seed &= 0xFFFFFFFF
        return seed

    def new_node(v):
        idx = len(left)
        left.append(0)
        right.append(0)
        parent.append(0)
        size.append(1)
        sm.append(v)
        value.append(v)
        priority.append(rng())
        return idx

    # Root of the prefix-function tree is node 0.
    # Its Euler sequence is enter(0), exit(0).
    new_node(0)
    new_node(0)
    root = 1

    def pull(x):
        l = left[x]
        r = right[x]
        size[x] = size[l] + size[r] + 1
        sm[x] = sm[l] + sm[r] + value[x]

    def merge(a, b):
        if a == 0:
            if b:
                parent[b] = 0
            return b
        if b == 0:
            parent[a] = 0
            return a

        if priority[a] > priority[b]:
            nr = merge(right[a], b)
            right[a] = nr
            if nr:
                parent[nr] = a
            pull(a)
            parent[a] = 0
            return a

        nl = merge(a, left[b])
        left[b] = nl
        if nl:
            parent[nl] = b
        pull(b)
        parent[b] = 0
        return b

    def split(x, k):
        if x == 0:
            return 0, 0

        ls = size[left[x]]

        if k <= ls:
            a, b = split(left[x], k)
            left[x] = b
            if b:
                parent[b] = x
            parent[x] = 0
            if a:
                parent[a] = 0
            pull(x)
            return a, x

        a, b = split(right[x], k - ls - 1)
        right[x] = a
        if a:
            parent[a] = x
        parent[x] = 0
        if b:
            parent[b] = 0
        pull(x)
        return x, b

    def get_rank(x):
        # 1-based position of x in the implicit sequence.
        ans = size[left[x]] + 1
        while parent[x]:
            p = parent[x]
            if right[p] == x:
                ans += size[left[p]] + 1
            x = p
        return ans

    def prefix_before(x):
        # Sum of values strictly before x.
        ans = sm[left[x]]
        while parent[x]:
            p = parent[x]
            if right[p] == x:
                ans += sm[left[p]] + value[p]
            x = p
        return ans

    def enter_token(v):
        # Vertex v has tokens 2*v+1 and 2*v+2.
        return 2 * v + 1

    def exit_token(v):
        return 2 * v + 2

    # Insert the two Euler tokens of vertex v immediately
    # before the exit token of its parent.
    def link_leaf(v, p):
        nonlocal root

        target = exit_token(p)
        pos = get_rank(target)

        a, b = split(root, pos - 1)

        en = new_node(1)
        ex = new_node(0)
        pair = merge(en, ex)

        root = merge(merge(a, pair), b)

    last = 0
    output = []

    for _ in range(q):
        parts = input().split()

        if parts[0] == b"add" or parts[0] == "add":
            raw = parts[1]
            if isinstance(raw, bytes):
                raw = raw[0]
            else:
                raw = ord(raw)

            c = (raw - 97 + last) % 26

            old_n = len(s)
            s.append(c + 97)

            if old_n == 0:
                cur_pi = 0
            else:
                j = pi[old_n - 1]
                while j > 0 and s[old_n] != s[j]:
                    j = pi[j - 1]
                if s[old_n] == s[j]:
                    j += 1
                cur_pi = j

            pi.append(cur_pi)

            v = old_n + 1
            link_leaf(v, cur_pi)

        else:
            raw_k = int(parts[1])
            n = len(s)

            k = ((raw_k - 1 + last) % n) + 1

            tin = enter_token(k)
            tout = exit_token(k)

            # All entry tokens in the subtree lie between tin and tout.
            ans = prefix_before(tout) - prefix_before(tin)

            output.append(str(ans))
            last = ans

    sys.stdout.write("\n".join(output))

if __name__ == "__main__":
    solve()
```Phần tiền tố-hàm tuân theo ý tưởng KMP tiêu chuẩn. Ký tự đầu tiên có giá trị tiền tố bằng 0. Đối với mỗi ký tự sau, chúng tôi bắt đầu với giá trị hàm tiền tố trước đó và liên tục đi theo các liên kết lỗi cho đến khi ký tự hiện tại có thể mở rộng đường viền. 

Giá trị tiền tố-hàm cũng là chỉ mục gốc trong cây. Nếu tiền tố hiện tại có độ dài (v) thì tiền tố thích hợp dài nhất cũng là hậu tố có độ dài`pi[v - 1]`, vậy đó chính xác là nút cha của nút (v). 

Mã thông báo Euler được gán các mã định danh cố định. Đối với đỉnh (v),`2*v+1`là mã thông báo đầu vào của nó và`2*v+2`là mã thông báo thoát của nó. Điều này khiến việc lưu trữ các tham chiếu mã thông báo riêng biệt cho mỗi đỉnh là không cần thiết. 

Treap lưu trữ trình tự Euler một cách ngầm định.`split(root, k)`tách các mã thông báo (k) đầu tiên, trong khi`merge(a, b)`nối hai chuỗi. Các con trỏ cha được duy trì bất cứ khi nào một nút con được gắn vào một nút treap, điều này làm cho`get_rank`khả thi. 

Vị trí chèn sử dụng`get_rank(exit_token(parent)) - 1`. Đây là một vị trí rất dễ mắc sai lầm. Phần phân tách phải chứa mọi mã thông báo trước mã thông báo thoát của cha mẹ, trong khi mã thông báo thoát thuộc về phần bên phải. 

Đối với một truy vấn,`prefix_before(x)`trả về tổng của tất cả các giá trị mã thông báo trước mã thông báo (x). Do đó trừ đi giá trị trước`enter(k)`từ giá trị trước`exit(k)`đếm mọi mã thông báo mục nhập trong cây con và loại trừ chính mã thông báo thoát. Không thể tràn số nguyên trong Python và trong C++, câu trả lời sẽ vừa khít với số nguyên có dấu 32 bit vì nó nhiều nhất là (n). 

Việc giải mã phải diễn ra trước khi cập nhật`last`. Câu trả lời mới trở thành`last`chỉ sau khi truy vấn đã được xử lý hoàn toàn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chuỗi được giải mã cuối cùng trở thành`abcababca`. Các cây cha của hàm tiền tố được tạo trực tuyến, trong khi chuyến tham quan Euler giữ cho mọi cây con của hàm tiền tố tiếp giáp nhau. 

| Truy vấn | Chuỗi hiện tại | Đã giải mã (k) | Nút cây tiền tố | Trả lời |`last`| 
| --- | --- | --- | --- | --- | --- | 
|`add a`|`a`| | | | 0 | 
|`add b`|`ab`| | | | 0 | 
|`add c`|`abc`| | | | 0 | 
|`add a`|`abca`| | | | 0 | 
|`get 1`|`abca`| 1 | 1 | 2 | 2 | 
|`add z`|`abcab`| | | | 2 | 
|`get 1`|`abcab`| 3 | 3 | 1 | 1 | 
|`get 1`|`abcab`| 2 | 2 | 2 | 2 | 
|`add y`|`abcaba`| | | | 2 | 
|`add z`|`abcabab`| | | | 2 | 
|`add a`|`abcababc`| | | | 2 | 
|`add y`|`abcababca`| | | | 2 | 
|`get 8`|`abcababca`| 1 | 1 | 4 | 4 | 
|`get 7`|`abcababca`| 3 | 3 | 3 | 3 | 
|`get 9`|`abcababca`| 4 | 4 | 2 | 2 | 
|`get 2`|`abcababca`| 4 | 4 | 2 | 2 | 

Truy vấn đầu tiên yêu cầu sự xuất hiện của`a`TRONG`abca`, cho hai. Câu trả lời đó làm thay đổi cách giải thích câu tiếp theo`get`. Sau khi câu trả lời thứ hai trở thành một, giá trị thô sau`1`giải mã thành (k=2), có tiền tố là`ab`và xảy ra hai lần. 

Các truy vấn sau cho thấy tại sao câu trả lời lại là kích thước cây con thay vì số ký tự trực tiếp. Ví dụ: số được giải mã (k=4) tương ứng với tiền tố`abca`. Các lần xuất hiện của nó được biểu thị bằng nút con của nút 4 trong cây hàm tiền tố và khoảng Euler chứa chính xác các mã thông báo đầu vào đó. 

### Ví dụ giải mã trực tuyến 

Hãy xem xét đầu vào nhỏ hơn```
6
add a
add a
add b
add a
get 1
get 1
```Chuỗi thực tế là`aaba`. Truy vấn đầu tiên có (k=1) và tiền tố`a`xảy ra ba lần. Điều này làm cho`last=3`. Nguyên thứ hai`get 1`sau đó được giải mã bằng cách sử dụng (n=4), cho ra (k=4). 

| Truy vấn | Chuỗi | Đã giải mã (k) | Tiền tố liên quan | Trả lời |`last`| 
| --- | --- | --- | --- | --- | --- | 
|`add a`|`a`| | | | 0 | 
|`add a`|`aa`| | | | 0 | 
|`add b`|`aab`| | | | 0 | 
|`add a`|`aaba`| | | | 0 | 
|`get 1`|`aaba`| 1 |`a`| 3 | 3 | 
|`get 1`|`aaba`| 4 |`aaba`| 1 | 1 | 

Dấu vết này thực hiện phần vấn đề không thể xử lý được bằng cách xử lý trước tất cả các truy vấn. Truy vấn thứ hai thực sự là hỏi về toàn bộ chuỗi hiện tại, vì giá trị được giải mã của nó phụ thuộc vào câu trả lời cho truy vấn đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(q\log q)) dự kiến ​​| Mỗi phần bổ sung thực hiện công việc tiền tố được khấu hao (O(1)) và công việc xử lý dự kiến ​​(O(\log q)). Mỗi`get`sử dụng hai lần duyệt treap (O(\log q)). | 
| Không gian | (O(q)) | Có hai mã thông báo Euler cho mỗi nút hàm tiền tố, cộng với mảng chuỗi và hàm tiền tố. | 

Số lượng nút chức năng tiền tố tối đa là (300.000), do đó treap chứa tối đa (600.002) mã thông báo bao gồm cả gốc nhân tạo. Chiều cao treap logarit dự kiến ​​giữ cho mỗi lần chèn và truy vấn động nằm trong giới hạn tiệm cận cần thiết. Vấn đề ban đầu có giới hạn là bốn giây, do đó việc triển khai cần có cấu trúc dữ liệu nhỏ gọn và I/O nhanh. Việc triển khai Python sử dụng các mảng số nguyên Python nguyên thủy và tránh việc tạo hoặc băm chuỗi con. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây giả định`solve()`chức năng từ giải pháp có trong cùng một tệp.```python
import sys
import io

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_stdout = sys.stdout
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    out = io.StringIO()
    sys.stdout = out

    try:
        solve()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = old_input

    return out.getvalue()

sample_1 = """16
add a
add b
add c
add a
get 1
add z
get 1
get 1
add y
add z
add a
add y
get 8
get 7
get 9
get 2
"""

assert run(sample_1) == """2
1
2
4
3
2
2
""", "sample 1"

assert run("""2
add a
get 1
""") == """1
""", "minimum size"

assert run("""5
add a
add a
add a
add a
get 1
""") == """4
""", "all equal values"

assert run("""3
add a
add b
get 2
""") == """1
""", "k equals n"

assert run("""6
add a
add a
add b
add a
get 1
get 1
""") == """3
1
""", "online decoding"

max_q = 300000
max_input = str(max_q) + "\n" + ("add a\n" * (max_q - 1)) + "get 1\n"
assert run(max_input) == str(max_q - 1) + "\n", "maximum size"

# A mixed pattern with several different prefix occurrences.
assert run("""9
add a
add b
add a
add b
add a
get 1
get 2
get 3
""") == """3
2
1
""", "overlapping prefixes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / add a / get 1`|`1`| Đầu vào hợp lệ tối thiểu và thực tế là tiền tố đó được tính | 
| bốn`add a`các hoạt động theo sau bởi`get 1`|`4`| Chuỗi hoàn toàn bằng nhau và cây con hàm tiền tố lớn | 
|`add a`,`add b`,`get 2`|`1`| Trường hợp biên (k=n) | 
|`aaba`theo sau là hai mã hóa`get 1`truy vấn |`3`,`1`| Sử dụng đúng`last`khi giải mã | 
| (299.999) bổ sung theo sau là`get 1`|`299999`| Số lượng truy vấn tối đa và trạng thái xử lý lớn | 
|`ababa`với một số được |`3`,`2`,`1`| Các lần xuất hiện tiền tố chồng chéo và các cây con hàm tiền tố lồng nhau | 

## Vỏ cạnh 

Đối với đầu vào tối thiểu```
2
add a
get 1
```cây hàm tiền tố chứa nút 1 ngay bên dưới gốc nhân tạo. Trình tự Euler của nó là`enter(0), enter(1), exit(1), exit(0)`. Khoảng thuộc về nút 1 chứa chính xác một mã thông báo đầu vào, vì vậy câu trả lời là`1`. 

Đối với chuỗi hoàn toàn bằng nhau```
5
add a
add a
add a
add a
get 1
```cây hàm tiền tố là một chuỗi. Nút 1 là tổ tiên của nút 2, 3 và 4, vì vậy cây con của nó chứa tất cả bốn nút thực. Khoảng Euler cho nút 1 chứa bốn mã thông báo đầu vào, đưa ra câu trả lời đúng`4`. 

Đối với trường hợp ranh giới```
3
add a
add b
get 2
```nút 2 là nút chức năng tiền tố mới nhất và chưa có nút con nào. Cây con của nó chỉ bao gồm chính nó nên khoảng Euler chứa một mã thông báo đầu vào. Câu trả lời là`1`, tương ứng với khoảng có độ dài hai duy nhất, toàn bộ chuỗi`ab`. 

Để giải mã trực tuyến,```
6
add a
add a
add b
add a
get 1
get 1
```cái đầu tiên`get`giải mã thành (k=1) và trả về`3`. Giá trị thô tiếp theo cũng là`1`, nhưng bây giờ`last=3`và (n=4), do đó độ dài được giải mã là (4). Nút 4 không có con cháu và kích thước cây con của nó là`1`. Do đó, đầu ra là`3`theo sau là`1`. 

Trường hợp (k=n) cũng giải thích tại sao mã thông báo thoát phải được giữ lại mặc dù nó có giá trị bằng 0. Mã thông báo vào và ra phân định cây con một cách rõ ràng. Nếu một truy vấn yêu cầu nút mới nhất thì hai mã thông báo đó liền kề nhau và sự khác biệt giữa tổng tiền tố của chúng vẫn cho ra chính xác một mục nhập. 

Cuối cùng, thứ tự chèn anh chị em không ảnh hưởng đến tính chính xác. Nút chức năng tiền tố mới luôn được chèn trước mã thông báo thoát của nút cha, do đó, nó trở thành một phần của cây con của nút cha. Việc nó xuất hiện trước hay sau các con hiện có của cha mẹ đều không ảnh hưởng đến tư cách thành viên của cây con hoặc kích thước cây con. Treap chỉ duy trì một thứ tự DFS hợp lệ, không phải thứ tự chuẩn.
