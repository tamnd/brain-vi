---
title: "CF 102470J - Người ngoài hành tinh nói lắp"
description: "Đối với mỗi trường hợp thử nghiệm, chúng ta có một chuỗi chữ thường s và một số nguyên m. Chúng ta cần tìm chuỗi con liền kề dài nhất xuất hiện ít nhất m lần trong s. Các lần xuất hiện được phép chồng chéo."
date: "2026-08-09T15:38:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102470
codeforces_index: "J"
codeforces_contest_name: "2009-2010 ACM ICPC Southwestern European Regional Programming Contest (SWERC 2009)"
rating: 0
weight: 102470
solve_time_s: 458
verified: true
draft: false
---

[CF 102470J - Người ngoài hành tinh nói lắp](https://codeforces.com/problemset/problem/102470/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đối với mỗi trường hợp thử nghiệm, chúng tôi có một chuỗi chữ thường`s`và một số nguyên`m`. Chúng ta cần tìm chuỗi con liền kề dài nhất xảy ra ít nhất`m`lần trong`s`. Các lần xuất hiện được phép chồng chéo. Nếu nhiều chuỗi con có cùng độ dài tối đa thì chúng ta không cần xác định chính chuỗi con đó. Chúng tôi xuất ra vị trí bắt đầu lớn nhất trong số tất cả các lần xuất hiện của bất kỳ chuỗi con tối ưu nào. 

Đầu vào chứa một số trường hợp thử nghiệm độc lập. Dòng đầu tiên của một trường hợp thử nghiệm cho biết`m`, theo sau là chuỗi thông báo. Một dòng chứa`m = 0`chấm dứt việc nhập liệu. Độ dài chuỗi nằm trong khoảng`m`Và`40000`, do đó, ngay cả một trường hợp thử nghiệm cũng có thể đủ lớn để liệt kê tất cả các chuỗi con và so sánh chúng trực tiếp là không khả thi. Sự cố ban đầu có giới hạn thời gian là 1 giây và giới hạn bộ nhớ là 256 MB. 

Điều kiện chồng chéo loại trừ các cách tiếp cận coi các sự kiện là rời rạc. Ví dụ, với`s = "ababa"`Và`m = 2`, chuỗi con`aba`xảy ra ở các vị trí`0`Và`2`. Hai lần xuất hiện chia sẻ giữa`a`, nhưng cả hai đều được tính. Đầu ra đúng là`3 2`. Việc triển khai nhảy qua phần cuối của một lần xuất hiện sau khi tìm thấy một lần xuất hiện sẽ bỏ lỡ lần xuất hiện thứ hai một cách không chính xác. 

Vụ án`m = 1`là một điều kiện biên khác. Mỗi chuỗi con xuất hiện ít nhất một lần, do đó toàn bộ chuỗi sẽ tự động tối ưu. Vì`m = 1`Và`s = "abc"`, câu trả lời là`3 0`. Một giải pháp được thiết kế chỉ xung quanh các chuỗi con lặp lại có thể in không chính xác`none`bởi vì nó mong đợi hai hoặc nhiều lần xuất hiện. 

Cũng không thể có chuỗi con không trống hợp lệ. Ví dụ, với`m = 3`Và`s = "abc"`, mỗi chuỗi con một ký tự chỉ xuất hiện một lần, do đó không có gì có thể xảy ra ba lần. Đầu ra đúng là`none`. Việc thực hiện bất cẩn có thể dẫn đến thực tế là`n >= m`là đủ và xuất ra không chính xác một chuỗi con có độ dài một. 

Cuối cùng, quy tắc xuất hiện ngoài cùng bên phải không phụ thuộc vào độ dài tối đa. Với`m = 2`Và`s = "ababa"`, cả hai`aba`và lần xuất hiện của nó có độ dài tối ưu là ba và lần xuất hiện ngoài cùng bên phải bắt đầu ở vị trí`2`. Trả về vị trí, lần xuất hiện tối ưu đầu tiên`0`, trả lời sai. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp bắt đầu bằng cách xem xét mọi chuỗi con có thể. có`n(n+1)/2`vị trí và độ dài khác nhau nên có Θ(`n²`) ứng viên. Đối với mỗi ứng cử viên, chúng ta có thể so sánh nó với mọi vị trí bắt đầu có thể có trong chuỗi và việc so sánh có thể kiểm tra Θ(`n`) ký tự trong trường hợp xấu nhất. Trên một chuỗi chẳng hạn như một chuỗi dài các ký tự bằng nhau, hầu hết mọi phép so sánh đều kết thúc trước khi phát hiện ra sự không khớp. Do đó tổng công là Θ(`n⁴`), với công việc so sánh thứ tự hàng đầu xung quanh`n⁴ / 12`. Tại`n = 40000`, điều này vượt xa giới hạn thời gian cho phép. Ngay cả khi chúng tôi cải thiện việc so sánh chuỗi con bằng hàm băm, việc liệt kê ứng cử viên đơn giản vẫn là bậc hai. 

Cấu trúc của vấn đề gợi ý một cách biểu diễn mạnh mẽ hơn. Chúng tôi không quan tâm đến mối quan hệ tùy ý giữa các chuỗi con. Chúng tôi quan tâm đến hai thuộc tính của mỗi chuỗi con: độ dài của nó và tất cả các vị trí nơi nó kết thúc. Một hậu tố tự động nhóm các chuỗi con một cách chính xác theo tập hợp các vị trí kết thúc của chúng, được gọi là`endpos`bộ. Tất cả các chuỗi con được đại diện bởi một trạng thái đều có vị trí xuất hiện giống hệt nhau, trong khi độ dài của chúng tạo thành một khoảng liên tục từ`len(link[v]) + 1`bởi vì`len[v]`. Đây chính xác là thông tin cần thiết ở đây. 

Giả sử một trạng thái`v`có`len[v] = 10`và số lần xuất hiện của nó ít nhất là`m`. Khi đó, chuỗi con dài nhất được biểu thị bởi trạng thái đó có độ dài 10 và đã đáp ứng yêu cầu lặp lại. Chúng tôi không phải kiểm tra tất cả các chuỗi ngắn hơn được đại diện bởi cùng một trạng thái, vì chúng không thể cải thiện câu trả lời. Nếu một số tiểu bang có cùng mức tối đa`len`, của họ`endpos`các bộ cho chúng ta biết đại diện dài nhất của chúng xuất hiện ở đâu, vì vậy chúng ta cũng có thể chọn vị trí bắt đầu ngoài cùng bên phải. 

Số lần xuất hiện của mọi trạng thái có thể được tính bằng cách trước tiên gán một lần xuất hiện cho mọi trạng thái mới được tạo, không phải bản sao và số 0 cho các bản sao, sau đó truyền số lần đếm dọc theo các liên kết hậu tố theo hướng giảm dần.`len`đặt hàng. Sự lan truyền tương tự có thể mang vị trí kết thúc tối đa. Đây là một kỹ thuật đếm số lần xuất hiện tự động có hậu tố tiêu chuẩn. 

Thuật toán kết quả là tuyến tính theo độ dài chuỗi. Hậu tố automaton có nhiều nhất`2n - 1`các trạng thái và đối với một bảng chữ cái viết thường cố định, cấu trúc của nó là tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n⁴) | O(n) phụ trợ | Quá chậm | 
| Máy tự động hậu tố tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một máy tự động hậu tố trong khi đọc chuỗi từ trái sang phải. Cửa hàng của mỗi bang`len`, liên kết hậu tố của nó, các chuyển tiếp đi ra của nó, bộ đếm lần xuất hiện và vị trí kết thúc lớn nhất hiện được biết cho trạng thái đó. Trạng thái được tạo cho ký tự mới được nối bắt đầu bằng số lần xuất hiện một và vị trí kết thúc bằng chỉ mục hiện tại. 

Máy tự động có thể tạo một bản sao khi quá trình chuyển đổi hiện có trỏ đến trạng thái có độ dài biểu thị quá lớn. Một bản sao sao chép các chuyển tiếp và liên kết hậu tố của trạng thái cũ nhưng bắt đầu với số lần xuất hiện bằng 0 vì việc tạo một bản sao không đưa ra một lần xuất hiện mới. 
2. Sau khi xây dựng, hãy đếm xem có bao nhiêu trạng thái có độ dài có thể. Sử dụng cách sắp xếp đếm để có được tất cả các trạng thái tăng dần`len`đặt hàng. Vì mỗi bang đều có`0 <= len <= n`, việc sắp xếp này mang tính tuyến tính chứ không dựa trên so sánh. 

Chúng ta cần giảm độ dài sau này vì liên kết hậu tố cha của trạng thái luôn có giá trị nhỏ hơn`len`. Việc xử lý con trước cha mẹ cho phép thông tin tích lũy của mọi trạng thái đến được cha mẹ của nó chính xác một lần. 
3. Đi qua các trạng thái giảm dần`len`. Đối với mỗi trạng thái không phải gốc`v`, trước tiên hãy kiểm tra số lần xuất hiện tích lũy của nó. Nếu như`occ[v] >= m`, chuỗi con dài nhất được biểu thị bằng`v`là một ứng cử viên hợp lệ và có độ dài`len[v]`. 
4. Đối với một trạng thái đủ điều kiện, hãy tính số lần xuất hiện ngoài cùng bên phải của nó từ vị trí kết thúc lớn nhất của nó. Nếu như`max_end[v]`là chỉ số kết thúc lớn nhất, sự xuất hiện tương ứng của chuỗi con dài nhất được biểu diễn bắt đầu từ`max_end[v] - len[v] + 1`. 

Nếu độ dài này lớn hơn câu trả lời hiện tại, hãy thay thế cả hai giá trị câu trả lời. Nếu chiều dài bằng nhau, hãy giữ vị trí bắt đầu lớn hơn. 
5. Sau khi đánh giá một trạng thái, hãy truyền số lần xuất hiện và vị trí kết thúc tối đa của nó tới cha mẹ liên kết hậu tố của nó. Một chuỗi con được đại diện bởi`v`có tất cả các lần xuất hiện của nó dưới dạng các lần xuất hiện của hậu tố được biểu thị bằng`link[v]`, vì vậy cả hai thông tin đều phải được chuyển giao. 
6. Nếu không có trạng thái nào có độ dài dương đạt tới`m`lần xuất hiện, in ấn`none`. Nếu không thì in độ dài đủ điều kiện tối đa và vị trí bắt đầu ngoài cùng bên phải của nó. 

### Tại sao nó hoạt động 

Bất biến chính là mọi trạng thái automaton hậu tố đại diện cho chính xác một`endpos`lớp tương đương. Tất cả các chuỗi con được biểu thị bằng cùng một trạng thái xảy ra ở cùng một vị trí kết thúc và chuỗi con dài nhất như vậy có độ dài`len[v]`. Sau khi lan truyền sự xuất hiện,`occ[v]`chính xác là số lần xuất hiện của mỗi chuỗi con được biểu thị bằng`v`. Sau khi truyền bá vị trí cực đại,`max_end[v]`là vị trí kết thúc ngoài cùng bên phải của những lần xuất hiện đó. 

Do đó, mỗi bang với`occ[v] >= m`cung cấp một chuỗi con hợp lệ có độ dài`len[v]`và không có chuỗi con nào được đại diện bởi trạng thái đó có thể dài hơn. Mỗi chuỗi con thuộc về một trạng thái nào đó, vì vậy lấy giá trị lớn nhất`len[v]`trên tất cả các trạng thái đủ điều kiện sẽ tìm thấy chuỗi con hợp lệ dài nhất trên toàn cầu. Trong số các tiểu bang có cùng chiều dài,`max_end[v] - len[v] + 1`chính xác là vị trí xuất phát ngoài cùng bên phải nên luật hòa cũng được xử lý chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(m, s):
    n = len(s)

    # A suffix automaton has at most 2*n states.
    max_states = 2 * n

    transitions = [{} for _ in range(max_states)]
    length = [0] * max_states
    link = [-1] * max_states

    # occ[v] is initially 1 only for newly created states.
    # Clones keep occ = 0.
    occ = [0] * max_states

    # Largest ending position belonging to the state's endpos set.
    max_end = [-1] * max_states

    size = 1
    last = 0

    for i, ch in enumerate(s):
        c = ord(ch) - 97

        cur = size
        size += 1

        length[cur] = length[last] + 1
        occ[cur] = 1
        max_end[cur] = i

        p = last

        while p != -1 and c not in transitions[p]:
            transitions[p][c] = cur
            p = link[p]

        if p == -1:
            link[cur] = 0
        else:
            q = transitions[p][c]

            if length[p] + 1 == length[q]:
                link[cur] = q
            else:
                clone = size
                size += 1

                length[clone] = length[p] + 1
                link[clone] = link[q]
                transitions[clone] = transitions[q].copy()

                # The clone represents the same end positions as q
                # before later occurrence propagation.
                max_end[clone] = max_end[q]

                # A clone is not a newly observed occurrence.
                occ[clone] = 0

                while p != -1 and transitions[p].get(c) == q:
                    transitions[p][c] = clone
                    p = link[p]

                link[q] = clone
                link[cur] = clone

        last = cur

    # Counting sort states by len.
    count = [0] * (n + 1)
    for v in range(size):
        count[length[v]] += 1

    for i in range(1, n + 1):
        count[i] += count[i - 1]

    order = [0] * size
    for v in range(size - 1, -1, -1):
        lv = length[v]
        count[lv] -= 1
        order[count[lv]] = v

    best_len = 0
    best_pos = -1

    # Reverse order gives decreasing length.
    for idx in range(size - 1, 0, -1):
        v = order[idx]

        if occ[v] >= m:
            cur_len = length[v]
            cur_pos = max_end[v] - cur_len + 1

            if cur_len > best_len:
                best_len = cur_len
                best_pos = cur_pos
            elif cur_len == best_len and cur_pos > best_pos:
                best_pos = cur_pos

        parent = link[v]

        if parent >= 0:
            occ[parent] += occ[v]
            if max_end[v] > max_end[parent]:
                max_end[parent] = max_end[v]

    if best_len == 0:
        return "none"

    return f"{best_len} {best_pos}"

def solve():
    out = []

    while True:
        line = input()
        if not line:
            break

        m = int(line)
        if m == 0:
            break

        s = input().strip()
        out.append(solve_case(m, s))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc xây dựng duy trì`last`, trạng thái tương ứng với toàn bộ tiền tố được xử lý cho đến nay. Khi một ký tự mới được thêm vào,`cur`đại diện cho tiền tố dài nhất mới đó. Vòng lặp liên kết hậu tố đầu tiên thêm quá trình chuyển đổi mới vào mọi trạng thái hậu tố mà trước đó không có nó. 

Xung đột chuyển đổi có hai khả năng. Nếu như`len[p] + 1 == len[q]`, trạng thái hiện có`q`đã có chính xác độ dài cần thiết, vì vậy`cur`có thể liên kết trực tiếp tới nó. Nếu không thì,`q`đại diện cho phạm vi độ dài chuỗi con quá rộng. Bản sao chia phạm vi đó ở`len[p] + 1`, sau đó cả hai`cur`Và`q`có thể sử dụng bản sao làm mục tiêu liên kết hậu tố của chúng. 

Bản sao nhận được một bản sao của`q`sự chuyển tiếp của. Bộ đếm lần xuất hiện của nó vẫn bằng 0 vì nhân bản thay đổi cấu trúc máy tự động mà không thêm vị trí mới trong chuỗi gốc. Các lần xuất hiện được phục hồi sau đó thông qua việc truyền bá liên kết hậu tố. Sự khác biệt này là nguyên nhân phổ biến của việc triển khai automaton hậu tố không chính xác. 

các`max_end`mảng tuân theo quy tắc lan truyền tương tự như`occ`. Nếu một trạng thái chứa một sự xuất hiện kết thúc ở vị trí`i`, mọi hậu tố được đại diện bởi tổ tiên liên kết hậu tố của nó cũng xuất hiện kết thúc tại`i`. Xử lý các trạng thái từ lớn hơn`len`nhỏ hơn`len`làm cho một lần lan truyền về phía trước là đủ. 

biểu hiện`max_end[v] - length[v] + 1`là vị trí bắt đầu của chuỗi con dài nhất được biểu thị bằng`v`ở vị trí kết thúc ngoài cùng bên phải của nó. Việc lập chỉ mục dựa trên số 0, phù hợp với đầu ra được yêu cầu. 

Sắp xếp đếm được sử dụng thay vì sắp xếp so sánh của Python vì độ dài trạng thái là số nguyên từ 0 đến 1.`n`. Điều này giữ cho thuật toán hoàn chỉnh tuyến tính. Không thể tràn số nguyên trong Python và số lượng trạng thái tối đa là dưới đây`80000`vì`n <= 40000`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu thực tế sử dụng`m = 3`Và`baaaababababbababbab`Phần quan trọng của quá trình quét tự động hậu tố là trạng thái biểu thị`babab`. Chuỗi con được biểu diễn dài nhất của nó có độ dài năm và số lần xuất hiện của nó là ba. Vị trí kết thúc ngoài cùng bên phải của nó là`16`, tương ứng với vị trí ban đầu`16 - 5 + 1 = 12`. 

| Sân khấu | Chuỗi đại diện | Chiều dài | Lần xuất hiện | Cuối cùng bên phải | Câu trả lời hiện tại | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | chuỗi trống | 0 | 20 | 19 |`0, -1`| 
| Đã tìm thấy ứng viên |`babab`| 5 | 3 | 16 |`5, 12`| 
| Các tiểu bang sau | chuỗi con dài hơn | >5 | <3 | khác nhau |`5, 12`| 
| Cuối cùng | trạng thái hợp lệ tốt nhất | 5 | 3 | 16 |`5, 12`| 

Nhà nước cho`babab`là đủ để giải thích câu trả lời. Ba vị trí kết thúc của nó tương ứng với điểm bắt đầu`5`,`7`, Và`12`, do đó sự xuất hiện ngoài cùng bên phải chính xác là vị trí`12`. Thực tế là ba lần xuất hiện trùng nhau được xử lý một cách tự nhiên vì số lần xuất hiện dựa trên vị trí kết thúc chứ không phải trên các khoảng thời gian rời rạc. Mẫu chính thức xác nhận kết quả`5 12`. 

### Mẫu 2 

Mẫu thứ hai sử dụng cùng một chuỗi nhưng`m = 11`. Các ký tự đơn thường xuyên nhất chỉ xuất hiện mười lần, do đó không có chuỗi con không trống nào có thể xuất hiện mười một lần. Mỗi chuỗi con dài hơn có số lần xuất hiện nhiều nhất bằng ký tự đầu tiên của nó, vì vậy nó cũng không thể đạt đến 11. 

| Sân khấu | Số lượng liên quan | Giá trị | 
| --- | --- | --- | 
| Đầu vào | Số lần xuất hiện bắt buộc`m`| 11 | 
| Độ dài chuỗi |`n`| 20 | 
| Số lượng tối đa của`a`| 10 | 
| Số lượng tối đa của`b`| 10 | 
| Bất kỳ chuỗi con nào có độ dài ít nhất là 2 | Lần xuất hiện | nhiều nhất là 10 | 
| Trạng thái đủ điều kiện cuối cùng |`occ[v] >= 11`| không | 
| Đầu ra | Kết quả |`none`| 

Máy tự động hậu tố vẫn xây dựng chính xác như đã làm với Mẫu 1 vì chuỗi không thay đổi. Chỉ ngưỡng được sử dụng trong quá trình quét trạng thái cuối cùng mới thay đổi. Vì không có trạng thái nào có số lần xuất hiện từ 11 trở lên,`best_len`vẫn bằng 0 và chương trình in`none`. Đây là đầu ra thứ hai của mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Cấu trúc tự động hậu tố, sắp xếp đếm theo độ dài trạng thái và lan truyền liên kết hậu tố đều là tuyến tính đối với bảng chữ cái viết thường cố định. | 
| Không gian | O(n) | Có nhiều nhất`2n - 1`trạng thái và cấu trúc chuyển tiếp cộng với mảng trạng thái có kích thước tuyến tính. | 

Với`n <= 40000`, máy tự động có ít hơn`80000`tiểu bang. Thuật toán chỉ thực hiện một lượng công việc không đổi trên mỗi trạng thái và quá trình chuyển đổi, do đó nó tránh được công việc bậc hai hoặc bậc bốn của việc liệt kê chuỗi con trực tiếp. Cách tiếp cận tự động hậu tố tuyến tính cũng là một trong những cách tiếp cận được xác định trong vật liệu giải pháp SWERC thông qua công thức cây hậu tố có liên quan chặt chẽ. 

## Trường hợp thử nghiệm 

Dây đai kiểm tra sau đây dự kiến sẽ được đặt sau mã giải pháp. Nó sử dụng`solve()`hoạt động thông qua đầu vào tiêu chuẩn được chuyển hướng và kiểm tra đầu ra hoàn chỉnh.```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    try:
        solve()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

    return output.getvalue()

# Provided samples
sample = """\
3
baaaababababbababbab
11
baaaababababbababbab
3
cccccc
0
"""

assert run(sample) == """\
5 12
none
4 2
""", "provided samples"

# Minimum-size input, m = 1.
assert run("""\
1
a
0
""") == "1 0\n", "minimum size"

# Boundary case: m = n, but the characters are not all equal.
assert run("""\
3
abc
0
""") == "none\n", "no substring occurs n times"

# Overlapping occurrences and rightmost tie-breaking.
assert run("""\
2
ababa
0
""") == "3 2\n", "overlapping occurrences"

# Maximum-size all-equal string.
s = "a" * 40000
assert run(f"""\
20000
{s}
0
""") == "20001 19999\n", "maximum size all-equal case"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / a`|`1 0`| Độ dài chuỗi tối thiểu và`m = 1`xử lý | 
|`3 / abc`|`none`| Trường hợp ranh giới trong đó`m = n`nhưng không có ký tự nào lặp lại đủ | 
|`2 / ababa`|`3 2`| Sự xuất hiện chồng chéo và sự ràng buộc ngoài cùng bên phải | 
|`20000 / a...a`với 40000 ký tự |`20001 19999`| Kích thước đầu vào tối đa, số lần lặp lại cao và số học vị trí xuất hiện | 

## Vỏ cạnh 

cho`m = 1`, bản thân gốc không được coi là ứng cử viên vì nó đại diện cho chuỗi rỗng. Mọi trạng thái không phải gốc đều có ít nhất một lần xuất hiện và trạng thái tương ứng với chuỗi hoàn chỉnh có`len = n`. Vị trí kết thúc ngoài cùng bên phải của nó là`n - 1`, do đó vị trí bắt đầu của nó bằng không. Vì`1`Và`abc`, thuật toán đạt`best_len = 3`Và`best_pos = 0`, sản xuất`3 0`. 

Đối với ngưỡng lặp lại không thể, hãy xem xét`3`Và`abc`. Mỗi ký tự có số lần xuất hiện là một và mỗi chuỗi con dài hơn cũng chỉ xuất hiện một lần. Sau khi nhân giống, mọi trạng thái không phải gốc đều có`occ < 3`, do đó các biến trả lời vẫn còn`best_len = 0`Và`best_pos = -1`. Chương trình in`none`. 

Đối với các lần xuất hiện chồng chéo, hãy xem xét`2`Và`ababa`. Chuỗi con`aba`kết thúc ở vị trí`2`Và`4`, vì vậy số lần xuất hiện của nó là hai. Trạng thái đại diện cho lớp vị trí cuối cùng của nó có`len = 3`Và`max_end = 4`. Vị trí bắt đầu là`4 - 3 + 1 = 2`, cho`3 2`. Không có giả định rời rạc nào xuất hiện ở bất kỳ đâu trong thuật toán, do đó sự chồng chéo được xử lý một cách tự nhiên. 

Đối với một chuỗi hoàn toàn bằng nhau, hãy xem xét sáu bản sao của`c`với`m = 3`. Một chuỗi con có độ dài bốn xuất hiện khi bắt đầu`0`,`1`, Và`2`, trong khi độ dài 5 chỉ xảy ra hai lần. Nhà nước cho`cccc`có độ dài bốn, số lần xuất hiện là ba và vị trí kết thúc ngoài cùng bên phải là năm, vì vậy điểm bắt đầu ngoài cùng bên phải của nó là`5 - 4 + 1 = 2`. Kết quả là`4 2`, phù hợp với mẫu 

Đối với trường hợp kích thước tối đa, lấy`40000`bản sao của`a`và yêu cầu`20000`lần xuất hiện. Một chuỗi con có độ dài`L`trong một chuỗi hoàn toàn bằng nhau xảy ra`40000 - L + 1`lần. Yêu cầu ít nhất`20000`sự xuất hiện mang lại`L <= 20001`. Do đó, độ dài hợp lệ dài nhất là`20001`và lần xuất hiện ngoài cùng bên phải của nó bắt đầu tại`40000 - 20001 = 19999`. Máy tự động hậu tố tạo ra chính xác`20001 19999`, thực hiện cả phép tính đầu vào lớn nhất được phép và phép tính vị trí ngoài cùng bên phải.
