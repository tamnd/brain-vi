---
title: "CF 102343K - So khớp mã"
description: "Chúng ta có một sổ mã chứa (N) chuỗi chữ số riêng biệt. Một trong những chuỗi đó được truyền đi. James bắt đầu nghe ở một chữ số ngẫu nhiên thống nhất trong chuỗi được truyền đi, vì vậy mọi thứ trước vị trí đó có thể đã bị bỏ sót."
date: "2026-08-17T10:27:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102343
codeforces_index: "K"
codeforces_contest_name: "UCF Locals 2019"
rating: 0
weight: 102343
solve_time_s: 203
verified: true
draft: false
---

[CF 102343K - So khớp mã](https://codeforces.com/problemset/problem/102343/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một sổ mã chứa (N) chuỗi chữ số riêng biệt. Một trong những chuỗi đó được truyền đi. James bắt đầu nghe ở một chữ số ngẫu nhiên thống nhất trong chuỗi được truyền đi, vì vậy mọi thứ trước vị trí đó có thể đã bị bỏ sót. 

Sau khi James nghe được một số chữ số liên tiếp, anh ấy coi mọi mục trong sổ mã vẫn có thể là thông điệp được truyền đi. Vì anh ta không biết mình bắt đầu nghe tin nhắn ứng cử viên từ đâu nên các chữ số anh ta đã nghe chỉ cần xuất hiện dưới dạng chuỗi con liền kề của ứng cử viên đó. Ngay khi có chính xác một mục trong sổ mã chứa toàn bộ chuỗi được nghe cho đến nay, James sẽ biết thông báo và dừng lại. 

Có một thông tin bổ sung ở cuối. Một giây sau chữ số cuối cùng của tin nhắn được truyền đi, sự im lặng diễn ra. Nếu James đã đi đến cuối mà không phân biệt được thông điệp, sự im lặng đó cho anh biết rằng trình tự anh nghe được phải kết thúc ở cuối thông điệp dự kiến. Nếu chính xác một ứng viên có hậu tố đó, anh ta có thể xác định được thông điệp. Nếu không thì không thể xác định được thông báo cho vị trí bắt đầu đó. 

Dữ liệu đầu vào chứa tối đa (100{,}000) chuỗi sổ mã và tổng độ dài của chúng tối đa là (100{,}000). Cuộc thi ban đầu đưa ra giới hạn thời gian là hai giây và giới hạn bộ nhớ 256 MB. Tổng chiều dài bị giới hạn là ràng buộc khóa: một thuật toán tỷ lệ thuận với tổng kích thước đầu vào là lý tưởng, trong khi việc so sánh liên tục mọi chuỗi con với mọi từ mã sẽ là bậc hai và có thể đạt tới khoảng (10^{10}) phép toán cấp ký tự. 

Có hai trường hợp ranh giới dễ bỏ sót. Đầu tiên, việc chỉ duy nhất sau chữ số cuối cùng không tự động có nghĩa là cần thêm một giây để im lặng. Nếu chuỗi hoàn chỉnh nghe được chỉ chứa trong một từ mã thì James sẽ biết từ mã đó ngay sau chữ số cuối cùng đó. Ví dụ, với```
2
12
123
```tin nhắn`12`có thể được nhận dạng sau khi nghe cả hai chữ số khi James bắt đầu ở chữ số đầu tiên, do đó vị trí bắt đầu mất 2 giây chứ không phải 3. Vị trí bắt đầu còn lại nghe thấy`2`; chữ số đó xuất hiện trong cả hai tin nhắn, nhưng chỉ sau khi im lặng`12`có thể kết thúc ở đó nên mất 2 giây. 

Thứ hai, sự im lặng cuối cùng có thể phân biệt các thông điệp ngay cả khi bản thân chuỗi chữ số không phải là duy nhất. Ví dụ,```
2
12
23
```nếu như`12`được truyền đi và James bắt đầu ở phần cuối cùng của nó`2`, thính giác`2`riêng nó là mơ hồ vì cả hai từ mã đều chứa`2`. Sau sự im lặng, chỉ`12`có thể có`2`là chữ số cuối cùng của nó, vì vậy thời gian là 2 giây. Một giải pháp xử lý mọi hậu tố đầy đủ không rõ ràng là không thể sẽ xảy ra trường hợp này sai. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ kiểm tra mọi vị trí bắt đầu có thể có của mỗi tin nhắn. Đối với một vị trí cố định, nó sẽ mở rộng chuỗi con được quan sát từng ký tự một và hỏi chuỗi sổ mã nào chứa chuỗi con đó. Việc kiểm tra trực tiếp tất cả các từ mã là đúng vì định nghĩa của một ứng cử viên chính xác là trình tự được quan sát xảy ra ở đâu đó bên trong từ mã đó. 

Vấn đề là tìm kiếm chuỗi con lặp đi lặp lại. Nếu tổng chiều dài đầu vào là (S) thì có thể có (S) vị trí bắt đầu. Trên tất cả các cặp thông báo, việc kiểm tra mọi vị trí bắt đầu có thể yêu cầu so sánh ký tự (\Theta(S^2)) trong trường hợp xấu nhất, tức là khoảng (10^{10}) thao tác khi (S=100{,}000). Công việc lặp đi lặp lại giống nhau đang được thực hiện vì nhiều chuỗi con khác nhau có chung tiền tố. 

Quan sát hữu ích là đối với một vị trí bắt đầu cố định, chúng ta chỉ cần một số: tiền tố dài nhất của hậu tố còn lại cũng xuất hiện trong một số từ mã khác. Giả sử tiền tố được chia sẻ dài nhất này có độ dài (L). Sau khi nghe các chữ số (L+1), không có từ mã nào khác chứa chuỗi con được quan sát, vì vậy James có thể xác định được thông báo ngay lập tức. Do đó, mọi vị trí bắt đầu có thể được rút gọn thành truy vấn có tiền tố chung dài nhất đối với các hậu tố thuộc các từ mã khác. 

Một mảng hậu tố cung cấp chính xác cấu trúc được yêu cầu. Đặt tất cả các từ mã thành một chuỗi, phân tách các từ mã liên tiếp bằng các ký hiệu phân cách khác nhau. Mỗi hậu tố bắt đầu bằng một chữ số bây giờ thể hiện sự tiếp tục có thể được quan sát. Theo thứ tự mảng hậu tố, LCP tối đa có hậu tố từ một từ mã khác đạt được bằng hậu tố gần nhất từ ​​một từ mã khác ở hai bên. Chúng ta có thể thu được các giá trị LCP này theo thời gian tuyến tính sau khi xây dựng mảng hậu tố. 

Trường hợp còn lại là khi toàn bộ hậu tố còn lại vẫn xuất hiện trong một từ mã khác. Khi đó việc kiểm tra chuỗi con thông thường không thể phân biệt được các tin nhắn. Chúng tôi xây dựng riêng một bộ ba từ mã đảo ngược. Một nút trong bộ ba này đại diện cho một hậu tố và số lượng được lưu trữ của nó cho chúng ta biết có bao nhiêu từ mã kết thúc bằng hậu tố đó. Nếu số đếm chính xác là một, khoảng lặng cuối cùng sẽ phân biệt tin nhắn sau một giây nữa. Nếu số lượng lớn hơn một thì vị trí bắt đầu đó là không thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(S^2)) so sánh ký tự | (O(S)) | Quá chậm | 
| Mảng hậu tố + Trie ngược | (O(S\log S)) | (O(S)) | Đã chấp nhận | 

Ở đây (S) là tổng chiều dài của tất cả các từ mã. 

## Hướng dẫn thuật toán 

1. Đọc tất cả các từ mã và ghép chúng thành một chuỗi số nguyên. Các chữ số sử dụng các giá trị từ 1 đến 10, trong khi mỗi từ mã nhận được giá trị phân tách duy nhất của riêng nó. Các dấu phân cách ngăn chặn hậu tố từ một từ mã vô tình trùng khớp qua ranh giới với một từ mã khác. 
2. Xây dựng mảng hậu tố của chuỗi được nối bằng cách nhân đôi tiền tố với sắp xếp đếm. Một trọng điểm nhỏ hơn mọi ký hiệu khác sẽ được thêm vào trong quá trình xây dựng, sau đó bị xóa khỏi mảng hậu tố cuối cùng. 
3. Tính mảng LCP bằng thuật toán Kasai.`lcp[r]`lưu trữ độ dài tiền tố chung của hậu tố ở thứ hạng mảng hậu tố`r`và hậu tố ngay trước nó. 
4. Đối với mỗi vị trí mảng hậu tố, hãy xác định LCP tối đa có hậu tố thuộc một từ mã khác. Quét từ trái sang phải rồi từ phải sang trái. Trong quá trình quét, khi mã định danh từ mã thay đổi, hậu tố trước đó sẽ trở thành hậu tố gần nhất từ ​​một từ mã khác. Trong khi vẫn ở trong cùng một từ mã, hãy giữ LCP tối thiểu gặp phải kể từ từ mã khác gần nhất đó. Mức tối thiểu này chính xác là LCP với hậu tố khác gần nhất. 
5. Đối với mỗi vị trí chữ số gốc, hãy lưu trữ độ dài tiền tố chia sẻ tối đa thu được. Nếu giá trị là (L), thì các chữ số (L) đầu tiên vẫn có thể bị nhầm lẫn với một tin nhắn khác, trong khi chữ số tiếp theo, nếu tồn tại, sẽ làm cho tin nhắn trở thành duy nhất. 
6. Xây dựng bộ ba chứa mọi từ mã theo thứ tự ngược lại. Mỗi nút được truy cập sẽ lưu trữ bao nhiêu từ mã đi qua nó. Do đó, một nút đại diện cho một hậu tố và số lượng của nó chính xác là số lượng từ mã có hậu tố đó. 
7. Xử lý từng từ mã từ phải sang trái. Tại vị trí (i), hãy`remaining = len(word) - i`. Nếu tiền tố chia sẻ dài nhất được tính toán trước với từ mã khác nhỏ hơn`remaining`, thì quan sát duy nhất đầu tiên xảy ra sau`best + 1`các chữ số, vì vậy hãy cộng số đó vào thời gian nghe. 
8. Nếu không có chuỗi con duy nhất nào xuất hiện trước khi tin nhắn kết thúc, hãy kiểm tra nút trie tương ứng với toàn bộ hậu tố còn lại. Nếu chính xác một từ mã kết thúc bằng hậu tố đó, James sẽ biết câu trả lời sau khi nghe hậu tố đó và sau đó im lặng một giây, đưa ra`remaining + 1`giây. Nếu ít nhất hai từ mã có hậu tố đó thì vị trí bắt đầu này là không thể, vì vậy toàn bộ câu trả lời cho thông báo đó là`Impossible`. 
9. Tính trung bình thời gian nghe trên tất cả các vị trí bắt đầu có thể. Mọi vị trí chữ số đều có khả năng là điểm bắt đầu như nhau, vì vậy hãy chia tổng cho độ dài tin nhắn. 

Bất biến về độ chính xác là với mọi vị trí bắt đầu,`best`là số lượng tối đa các chữ số được quan sát ban đầu cũng xuất hiện liền kề trong một từ mã khác. Do đó, mọi quan sát về độ dài nhiều nhất`best`là mơ hồ, trong khi việc quan sát độ dài`best + 1`, nếu nó tồn tại, chỉ xảy ra trong từ mã được truyền đi. Nếu hậu tố còn lại có độ dài tối đa`best`, không có quan sát chỉ bằng chữ số nào có thể phân biệt được thông báo và phép thử ngược lại sẽ kiểm tra chính xác xem liệu sự im lặng cuối cùng có để lại một hoặc nhiều thông báo có thể hay không. Do đó thời gian tính toán chính xác là thời gian James cần cho vị trí xuất phát đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def suffix_array(a):
    """Suffix array of an integer sequence, O(n log n)."""
    s = a + [0]  # 0 is the unique sentinel
    n = len(s)

    alphabet = max(s) + 1
    cnt = [0] * alphabet
    for x in s:
        cnt[x] += 1

    pos = [0] * alphabet
    for i in range(1, alphabet):
        pos[i] = pos[i - 1] + cnt[i - 1]

    p = [0] * n
    for x in s:
        p[pos[x]] = p[pos[x]] + 1
        pos[x] += 1

    # The previous counting-sort construction above needs positions
    # reconstructed from counts.
    pos = [0] * alphabet
    for i in range(1, alphabet):
        pos[i] = pos[i - 1] + cnt[i - 1]

    p = [0] * n
    for i, x in enumerate(s):
        p[pos[x]] = i
        pos[x] += 1

    c = [0] * n
    classes = 1
    c[p[0]] = 0

    for i in range(1, n):
        if s[p[i]] != s[p[i - 1]]:
            classes += 1
        c[p[i]] = classes - 1

    length = 1

    while length < n:
        pn = [0] * n
        for i in range(n):
            x = p[i] - length
            if x < 0:
                x += n
            pn[i] = x

        cnt = [0] * classes
        for x in pn:
            cnt[c[x]] += 1

        pos = [0] * classes
        for i in range(1, classes):
            pos[i] = pos[i - 1] + cnt[i - 1]

        new_p = [0] * n
        for x in pn:
            cls = c[x]
            new_p[pos[cls]] = x
            pos[cls] += 1

        cn = [0] * n
        new_classes = 1
        cn[new_p[0]] = 0

        for i in range(1, n):
            cur = new_p[i]
            prev = new_p[i - 1]

            cur_second = cur + length
            if cur_second >= n:
                cur_second -= n

            prev_second = prev + length
            if prev_second >= n:
                prev_second -= n

            if c[cur] != c[prev] or c[cur_second] != c[prev_second]:
                new_classes += 1

            cn[cur] = new_classes - 1

        p = new_p
        c = cn
        classes = new_classes
        length <<= 1

    # Remove the suffix consisting only of the sentinel.
    return p[1:]

def build_lcp(a, sa):
    n = len(a)
    rank = [0] * n

    for i, p in enumerate(sa):
        rank[p] = i

    lcp = [0] * n
    h = 0

    for i in range(n):
        r = rank[i]

        if r == 0:
            continue

        j = sa[r - 1]

        while i + h < n and j + h < n and a[i + h] == a[j + h]:
            h += 1

        lcp[r] = h

        if h:
            h -= 1

    return rank, lcp

def solve():
    n = int(input())
    words = [input().strip() for _ in range(n)]

    # Concatenate all words. Each word gets its own separator.
    # Positions of actual digits are retained for later queries.
    a = []
    doc = []
    positions = [[] for _ in range(n)]

    for idx, word in enumerate(words):
        for ch in word:
            positions[idx].append(len(a))
            a.append(ord(ch) - ord('0') + 1)
            doc.append(idx)

        # Separators are all different and larger than digit symbols.
        a.append(11 + idx)
        doc.append(idx)

    # Suffix-array phase.
    sa = suffix_array(a)
    rank, lcp = build_lcp(a, sa)

    # best[r] = maximum LCP with a suffix from a different codeword.
    best = [0] * len(a)

    current_doc = doc[sa[0]]
    minimum = None

    for r in range(1, len(sa)):
        d = doc[sa[r]]

        if d != current_doc:
            current_doc = d
            minimum = lcp[r]
        elif minimum is not None:
            minimum = min(minimum, lcp[r])

        if minimum is not None:
            best[r] = minimum

    current_doc = doc[sa[-1]]
    minimum = None

    for r in range(len(sa) - 2, -1, -1):
        d = doc[sa[r]]

        if d != current_doc:
            current_doc = d
            minimum = lcp[r + 1]
        elif minimum is not None:
            minimum = min(minimum, lcp[r + 1])

        if minimum is not None:
            best[r] = max(best[r], minimum)

    # The suffix-array data is no longer needed.
    del sa
    del lcp
    del doc
    del a

    # Build a trie of reversed codewords.
    children = [{}]
    suffix_count = [0]

    for word in words:
        node = 0

        for ch in reversed(word):
            nxt = children[node].get(ch)

            if nxt is None:
                nxt = len(children)
                children[node][ch] = nxt
                children.append({})
                suffix_count.append(0)

            node = nxt
            suffix_count[node] += 1

    output = []

    for idx, word in enumerate(words):
        total_time = 0
        possible = True

        node = 0
        found_unique = False

        for i in range(len(word) - 1, -1, -1):
            ch = word[i]
            node = children[node][ch]

            remaining = len(word) - i
            global_pos = positions[idx][i]
            shared = best[rank[global_pos]]

            if shared < remaining:
                total_time += shared + 1
                found_unique = True
                break

        if not found_unique:
            # The complete remaining suffix never became unique
            # as an ordinary substring. Silence can distinguish it
            # only if exactly one codeword ends with it.
            if suffix_count[node] == 1:
                total_time += len(word) + 1
            else:
                possible = False

        if not possible:
            output.append("Impossible")
        else:
            output.append(f"{total_time / len(word):.10f}")

    sys.stdout.write("\n".join(output))

if __name__ == "__main__":
    solve()
```Giai đoạn nối gán một dấu phân cách khác nhau cho mỗi từ mã. Điều này còn hơn cả sự tiện lợi: nếu sử dụng lại cùng một dấu phân cách, hai hậu tố có thể nhận sai LCP vượt qua ranh giới từ mã. Các dấu phân cách duy nhất khiến điều đó không thể thực hiện được vì các hậu tố từ các từ mã khác nhau gặp các ký hiệu khác nhau ngay sau phần chữ số của chúng. 

Mảng hậu tố sử dụng một trọng điểm nhỏ hơn mọi ký hiệu thực. Tiền tố nhân đôi sắp xếp các dịch chuyển theo chu kỳ và trọng điểm chuyển đổi thứ tự đó thành thứ tự mảng hậu tố thông thường. Việc triển khai sử dụng cách sắp xếp đếm cho mỗi vòng nhân đôi, đưa ra (O(S\log S)) thay vì sắp xếp các hậu tố bằng cách sắp xếp so sánh ở mỗi vòng. 

Hai lần quét trên mảng LCP đáng được quan tâm đặc biệt. Giả sử thứ hạng mảng hậu tố hiện thuộc về từ mã A. Hậu tố gần nhất từ ​​một từ mã khác là thứ hạng gần đây nhất có tài liệu khác với A. LCP có hậu tố đó là giá trị LCP tối thiểu trong khoảng giữa hai cấp. Khi gặp một hậu tố A khác, việc kéo dài khoảng chỉ cần một hậu tố khác`min`hoạt động. Quét từ phải sang trái thực hiện phép tính đối xứng. 

Phép thử ngược lại chỉ được sử dụng cho trường hợp im lặng cuối cùng. Việc duyệt một từ từ ký tự cuối cùng đến ký tự đầu tiên tuân theo chính xác các hậu tố có thể được nghe thấy khi James bắt đầu ở mỗi vị trí và đến cuối.`suffix_count[node]`đếm các từ mã có hậu tố đó, do đó việc kiểm tra chính xác một ứng cử viên khớp trực tiếp với thông tin được cung cấp bởi sự im lặng. 

Số nguyên Python có độ chính xác tùy ý, do đó thời gian nghe tích lũy không có nguy cơ bị tràn. Phép chia cuối cùng chỉ được thực hiện sau khi tính tổng số nguyên chính xác và mười chữ số sau dấu thập phân mang lại độ chính xác cao hơn nhiều so với sai số tương đối bắt buộc (10^{-5}). 

## Ví dụ đã hoạt động 

Đối với mẫu được cung cấp, sổ mã là`17383`,`126`,`385`, Và`485`. Bảng sau đây liệt kê năm vị trí bắt đầu có thể có của`17383`. 

| Vị trí bắt đầu | Hậu tố còn lại | Tiền tố chia sẻ dài nhất | Số lượng hậu tố kết thúc | Thời gian | 
| --- | --- | --- | --- | --- | 
| 1 |`17383`| 1 | không cần thiết | 2 | 
| 2 |`7383`| 0 | không cần thiết | 1 | 
| 3 |`383`| 2 | không cần thiết | 3 | 
| 4 |`83`| 1 | không cần thiết | 2 | 
| 5 |`3`| 1 | 1 | 2 | 

Chữ số đầu tiên`1`cũng có mặt ở`126`, vì vậy một chữ số là không rõ ràng và`17`trở thành duy nhất sau hai giây. Bắt đầu lúc`7`, chữ số đó đã xác định được`17383`. Bắt đầu lúc`3`ở giữa, cả hai`17383`Và`385`bao gồm`38`, trong khi`383`chỉ xảy ra ở`17383`. Ở chữ số cuối cùng,`3`cũng xảy ra ở`385`, nhưng chỉ`17383`kết thúc bằng`3`, nên sự im lặng cuối cùng sẽ giải quyết được sự mơ hồ. Giá trị trung bình là ((2+1+3+2+2)/5=2), khớp với mẫu. 

Đối với ví dụ thứ hai, hãy xem xét```
3
12
23
45
```Các trạng thái cho mỗi tin nhắn là: 

| Tin nhắn | Bắt đầu | Hậu tố còn lại | Tiền tố chia sẻ dài nhất | Số lượng hậu tố kết thúc | Thời gian | 
| --- | --- | --- | --- | --- | --- | 
|`12`| 1 |`12`| 1 | không cần thiết | 2 | 
|`12`| 2 |`2`| 1 | 1 | 2 | 
|`23`| 1 |`23`| 1 | không cần thiết | 2 | 
|`23`| 2 |`3`| 0 | không cần thiết | 1 | 
|`45`| 1 |`45`| 0 | không cần thiết | 1 | 
|`45`| 2 |`5`| 0 | không cần thiết | 1 | 

Vì`12`, chữ số đầu tiên được chia sẻ với không có tin nhắn nào khác, trong khi chữ số cuối cùng`2`được chia sẻ dưới dạng một chuỗi con với`23`, vì vậy cần có sự im lặng khi bắt đầu từ đó. Vì`23`, đầu tiên`2`được chia sẻ nhưng`23`bản thân nó là duy nhất, trong khi`3`là duy nhất ngay lập tức. Tin nhắn`45`có thể nhận dạng được từ một trong hai chữ số. Kết quả đầu ra là`1.5000000000`,`1.5000000000`, Và`1.0000000000`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(S\log S)) | Cấu trúc mảng hậu tố chiếm ưu thế; LCP, quét và xây dựng trie là tuyến tính | 
| Không gian | (O(S)) | Tất cả các mảng, thông tin hậu tố, dấu phân cách và nút trie đều tuyến tính trong tổng chiều dài đầu vào | 

Đây (S\le100{,}000). Mảng hậu tố thực hiện các vòng nhân đôi (O(\log S)), mỗi vòng sử dụng tính năng sắp xếp tuyến tính, trong khi mỗi giai đoạn sau chỉ chạm vào mỗi ký tự một số lần không đổi. Do đó, thuật toán nằm trong giới hạn tiệm cận dự kiến ​​và tránh được sự so sánh chuỗi con lặp lại bậc hai của phương pháp brute-force. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây giả định giải pháp biên tập đã được lưu dưới dạng`solution.py`.```python
# helper: run solution on input string, return output string
import sys
import io
import solution

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    old_input = solution.input

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solution.input = sys.stdin.readline

    try:
        solution.solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        solution.input = old_input

# Provided sample
sample1 = """\
4
17383
126
385
485
"""

assert run(sample1) == (
    "2.0000000000\n"
    "1.3333333333\n"
    "Impossible\n"
    "Impossible"
), "provided sample"

# Minimum-size input
assert run("1\n0\n") == "1.0000000000", "single one-digit codeword"

# Several overlapping strings, exercising substring ambiguity and silence
case2 = """\
3
12
23
45
"""

assert run(case2) == (
    "1.5000000000\n"
    "1.5000000000\n"
    "1.0000000000"
), "substring matching and final silence"

# Nested repeated digits, exercising full-suffix ambiguity
case3 = """\
3
1
11
111
"""

assert run(case3) == (
    "2.0000000000\n"
    "Impossible\n"
    "Impossible"
), "nested suffixes"

# Boundary case where the whole observed sequence becomes unique
case4 = """\
2
12
123
"""

assert run(case4) == (
    "2.0000000000\n"
    "2.0000000000"
), "unique full substring without extra silence"

# Maximum total length, one codeword consisting entirely of equal digits.
# Every observed digit already identifies the only codeword.
big_word = "0" * 100000
case5 = "1\n" + big_word + "\n"

assert run(case5) == "1.0000000000", "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`theo sau là`0`|`1.0000000000`| Đầu vào kích thước tối thiểu | 
|`12`,`23`,`45`|`1.5`,`1.5`,`1.0`| Sự mơ hồ và im lặng của chuỗi con thông thường | 
|`1`,`11`,`111`|`2.0`,`Impossible`,`Impossible`| Ký tự lặp đi lặp lại và hậu tố chồng chéo | 
|`12`,`123`|`2.0`,`2.0`| Ranh giới giữa chuỗi con duy nhất và khoảng lặng cuối cùng | 
| Một chuỗi 100000 số 0 |`1.0`| Tổng chiều dài tối đa và các chữ số bằng nhau | 

## Vỏ cạnh 

Một từ mã duy nhất là trường hợp đơn giản nhất có thể. Với```
1
0
```từ mã duy nhất chứa mọi chuỗi con được quan sát, vì vậy James biết ngay thông báo sau khi nghe chữ số đầu tiên. Trie ngược cũng chứa chính xác một từ mã cho hậu tố`0`, nhưng trường hợp đó không bao giờ đạt được vì chuỗi con đã là duy nhất. Đầu ra là`1.0000000000`. 

Các từ mã chồng chéo có thể làm cho một chữ số trở nên mơ hồ trong khi chuỗi con dài hơn là duy nhất. Với```
2
12
123
```bắt đầu từ chữ số đầu tiên của`12`, quan sát được`1`xảy ra trong cả hai tin nhắn, nhưng`12`chỉ xảy ra ở lần đầu tiên nên thời gian chính xác là 2 giây. Bắt đầu từ trận chung kết`2`, chữ số được chia sẻ, nhưng chỉ`12`kết thúc bằng`2`, nên sự im lặng sẽ đưa ra câu trả lời sau 2 giây. Đầu ra cho`12`do đó`2.0000000000`. 

Hậu tố lặp đi lặp lại có thể làm cho sự im lặng không đủ. Với```
3
1
11
111
```chữ số`1`xảy ra trong cả ba tin nhắn. Vì`11`, thậm chí cả hậu tố hoàn chỉnh`11`là sự kết thúc của cả hai`11`Và`111`, nên sự im lặng không thể phân biệt được họ. Tin nhắn là không thể. Vì`111`, bắt đầu từ chữ số thứ hai tạo ra sự mơ hồ tương tự, trong khi bắt đầu từ chữ số cuối cùng sẽ để lại cả ba thông báo có thể cho đến khi im lặng. Đầu ra là`Impossible`cho cả hai tin nhắn dài hơn. 

Ranh giới chữ số cuối cùng đặc biệt dễ bị xử lý sai. Coi như```
2
12
23
```khi`12`được truyền đi và James bắt đầu ở phần cuối cùng của nó`2`. chữ số`2`xảy ra trong cả hai từ mã, vì vậy giải pháp chỉ chuỗi con sẽ khai báo sự mơ hồ mãi mãi. Trie ngược lại chỉ thấy rằng`12`kết thúc bằng`2`, vì vậy sau một chữ số và một giây im lặng, tin nhắn sẽ được biết. Thời gian chính xác cho vị trí xuất phát này là 2 giây. 

Trường hợp kích thước tối đa là một từ mã có (100{,}000) chữ số bằng nhau. Mỗi chuỗi con chỉ thuộc về từ mã đó, vì vậy mọi vị trí bắt đầu có thể có đều mất một giây. Giai đoạn mảng hậu tố vẫn xử lý tất cả (100{,}000) ký tự và tổng công việc vẫn giữ nguyên (O(S\log S)) thay vì trở thành bậc hai.
