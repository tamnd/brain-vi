---
title: "CF 102437B - Phá luật"
description: "Chúng ta bắt đầu với một chuỗi s có độ dài n. Chúng tôi có thể xóa nhiều lần một ký tự, nhưng chỉ có thể xóa ký tự hiện đang chiếm một trong hai vị trí đầu tiên hoặc một trong hai vị trí cuối cùng. Sau khi xóa chính xác n-k, các ký tự còn lại sẽ tạo thành mật khẩu."
date: "2026-08-09T12:40:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102437
codeforces_index: "B"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0427\u0435\u0442\u0432\u0451\u0440\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102437
solve_time_s: 373
verified: true
draft: false
---

[CF 102437B - Phá vỡ quy tắc](https://codeforces.com/problemset/problem/102437/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một chuỗi`s`chiều dài`n`. Chúng tôi có thể xóa nhiều lần một ký tự, nhưng chỉ có thể xóa ký tự hiện đang chiếm một trong hai vị trí đầu tiên hoặc một trong hai vị trí cuối cùng. Sau chính xác`n-k`xóa, các ký tự còn lại tạo thành mật khẩu. Trong số tất cả các mật khẩu có độ dài có thể`k`, chúng ta cần cái nhỏ nhất về mặt từ điển. 

Ràng buộc`n <= 500000`loại trừ mọi thứ bậc hai về độ dài chuỗi. Thậm chí`O(nk)`có thể trở thành bậc hai khi`k`gần với`n`, vì vậy chúng ta cần một giải pháp có công việc chính gần với tuyến tính hoặc tệ nhất là`O(n log n)`. Bảng chữ cái nhỏ gồm 26 chữ cái viết thường giúp đưa ra một số lựa chọn về ranh giới, nhưng bản thân nó không giải quyết được vấn đề vì việc so sánh từ điển có thể phụ thuộc vào một tiền tố chung dài. 

Trường hợp cạnh đầu tiên là`k = n`. Không thể xóa được nên câu trả lời chỉ đơn giản là chuỗi gốc. Ví dụ, với`s = "abc"`Và`k = 3`, câu trả lời là`abc`. Giải pháp luôn giả định ít nhất một lần xóa có thể vô tình truy cập vào vị trí thứ hai hoặc áp chót không hợp lệ. 

Trường hợp cạnh thứ hai là`k = 1`. Mỗi ký tự riêng lẻ có thể được để lại làm ký tự cuối cùng, vì chúng ta có thể xóa liên tục từ bên trái cho đến khi ký tự đó trở thành ký tự đầu tiên. Vì vậy, câu trả lời là ký tự nhỏ nhất của toàn bộ chuỗi. Vì`s = "zba"`Và`k = 1`, câu trả lời là`a`. 

Trường hợp cạnh thứ ba là`k = 2`. Trong trường hợp này, mỗi cặp vị trí có thể được coi là hai ký tự còn sống. Vì`s = "bac"`Và`k = 2`, các câu trả lời có thể bao gồm`ac`Và`ba`, vậy câu trả lời là`ac`. Chỉ cần sắp xếp hai ký tự nhỏ nhất sẽ gợi ý`ab`, Nhưng`ab`không phải là dãy con của`bac`và không thể sản xuất được. 

Một trường hợp ranh giới tinh vi hơn xuất hiện khi chỉ có một lần xóa. Vì`s = "abcde"`Và`k = 4`, chúng tôi không thể xóa`c`, vì ban đầu chỉ`a`,`b`,`d`, Và`e`có thể truy cập được. Các chuỗi có thể là`bcde`,`acde`,`abce`, Và`abcd`, vậy câu trả lời là`abcd`. Một giải pháp xử lý thao tác như xóa chuỗi con tùy ý sẽ cho phép không chính xác`abde`. 

## Phương pháp tiếp cận 

Một giải pháp brute-force trực tiếp có thể thử đệ quy tất cả bốn thao tác xóa. Điều này đúng vì mọi thao tác hợp pháp đều được xem xét rõ ràng, do đó mọi chuỗi có thể truy cập đều xuất hiện ở đâu đó trong cây đệ quy. Tuy nhiên, sau`n-k`việc xóa cây có tới`4^(n-k)`trình tự hoạt động. Với`n = 500000`, thậm chí một phần rất nhỏ của không gian tìm kiếm này cũng không thể được khám phá. Việc lưu trữ tất cả các chuỗi trung gian riêng biệt cũng quá tốn kém. 

Quan sát hữu ích đến từ việc xem xét các vị trí còn tồn tại hơn là các nhân vật biến mất. Trong quá trình thực hiện, các vị trí còn lại luôn có hình dạng rất hạn chế. Chúng bao gồm một khoảng liền kề, có thể có thêm một vị trí sống sót trước khoảng thời gian đó và có thể có thêm một vị trí sống sót sau khoảng thời gian đó. 

Để biết lý do tại sao, hãy bắt đầu với toàn bộ chuỗi, tức là một khoảng. Nếu xóa ký tự đầu tiên, chúng ta sẽ rút ngắn khoảng cách từ bên trái. Nếu chúng ta xóa ký tự thứ hai, ký tự đầu tiên có thể vẫn ở dạng đơn trong khi các ký tự còn lại vẫn liền kề nhau. Lập luận tương tự được áp dụng đối xứng ở bên phải. Việc lặp lại quá trình này không bao giờ có thể tạo ra một tập hợp các khoảng thời gian phức tạp. Nhiều nhất một đơn vị có thể được tách ra khỏi mỗi phía của khoảng liền kề trung tâm. 

Điều ngược lại cũng đúng. Giả sử các vị trí còn sống là một khoảng liền kề, với tùy chọn một ký tự còn sống trước nó và tùy chọn một ký tự sau nó. Mọi thứ trước khoảng giữa có thể bị xóa từ bên trái, giữ nguyên ký tự bên trái tùy chọn bằng cách xóa liên tục vị trí thứ hai. Mọi thứ sau khoảng trung tâm có thể được xử lý đối xứng từ bên phải. Do đó, mọi chuỗi có cấu trúc này đều có thể truy cập được. 

Vì vậy, mọi mật khẩu có thể truy cập đều có một trong bốn dạng. Nó có thể là một chuỗi con liền kề có độ dài`k`. Nó có thể là một ký tự theo sau là một chuỗi con liền kề có độ dài`k-1`. Nó có thể là một chuỗi con liền kề có độ dài`k-1`theo sau là một ký tự. Hoặc nó có thể là một ký tự, sau đó là một chuỗi con liền kề có độ dài`k-2`, sau đó là một ký tự. 

Thử thách còn lại là tìm ra chuỗi con nhỏ nhất về mặt từ điển có độ dài cố định một cách hiệu quả. Chúng tôi xây dựng một mảng hậu tố cho`s`. Đối với hai chuỗi con có cùng độ dài, thứ tự từ điển của chúng giống với thứ tự của các hậu tố tương ứng, trừ khi các chuỗi con bằng nhau. Chúng tôi nhóm các hậu tố theo thứ tự đầu tiên của chúng`m`các ký tự sử dụng mảng hậu tố và mảng LCP của nó. Điều này mang lại cho mọi chiều dài-`m`chuỗi con một thứ hạng từ điển nhỏ gọn. 

Khi đã có sẵn các cấp bậc này, mỗi trường hợp trong số bốn trường hợp cấu trúc sẽ trở thành một bản quét tuyến tính. Đối với hình thức`c + middle`, ký tự đầu tiên chiếm ưu thế so sánh, theo sau là thứ hạng của`middle`. Vì`middle + c`, chuỗi con ở giữa chiếm ưu thế đầu tiên, tiếp theo là ký tự cuối cùng. Vì`c + middle + d`, việc so sánh được thực hiện theo thứ tự`c`,`middle`,`d`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(4^(n-k))`tiểu bang | Hàm mũ | Quá chậm | 
| Tối ưu |`O(n log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tay cầm`k = 1`Và`k = 2`trực tiếp. Đối với một ký tự, chọn ký tự tối thiểu. Đối với hai ký tự, hãy quét mọi vị trí đầu tiên có thể và ghép nó với ký tự tối thiểu xuất hiện sau đó. Điều này hiệu quả vì mọi dãy con có hai vị trí đều có thể truy cập được. 
2. Đối với`k >= 3`, xây dựng mảng hậu tố của`s`với một trọng điểm nhỏ hơn mọi chữ cái viết thường. Mảng hậu tố đưa ra thứ tự từ điển của tất cả các hậu tố. 
3. Xây dựng mảng LCP bằng thuật toán Kasai. Giá trị LCP giữa các hậu tố liên tiếp cho chúng ta biết tiền tố chung của chúng dài bao nhiêu. 
4. Đối với chiều dài trung bình cần thiết`m`, quét mảng hậu tố và gán cùng thứ hạng cho các hậu tố liên tiếp có LCP ít nhất`m`. Hai vị trí có cùng thứ hạng chính xác khi chiều dài của chúng-`m`các chuỗi con bằng nhau. Các cấp bậc được sắp xếp theo từ điển. 
5. Xem xét mật khẩu chính xác là một chuỗi con liền kề có độ dài`k`. Trong số tất cả các vị trí bắt đầu hợp lệ, hãy chọn vị trí có độ dài nhỏ nhất-`k`thứ hạng chuỗi con. 
6. Xem xét mật khẩu dạng`c + middle`, Ở đâu`middle`có chiều dài`k-1`. Đối với mọi vị trí bắt đầu có thể có của`middle`, tốt nhất có thể`c`là ký tự nhỏ nhất xuất hiện trước vị trí đó. So sánh các ứng viên trước tiên bằng cách`c`, sau đó theo thứ hạng của`middle`. 
7. Xem xét mật khẩu dạng`middle + c`. Đối với mọi chuỗi con ở giữa có thể có độ dài`k-1`, chọn ký tự nhỏ nhất sau nó. Trước tiên hãy so sánh các ứng cử viên theo thứ hạng chuỗi con ở giữa, sau đó theo ký tự cuối cùng đó. 
8. Xem xét mật khẩu dạng`c + middle + d`, Ở đâu`middle`có chiều dài`k-2`. Đối với mỗi lần bắt đầu ở giữa có thể, hãy chọn ký tự nhỏ nhất trước nó và ký tự nhỏ nhất sau nó. So sánh các ứng viên theo`c`, rồi hạng trung, rồi`d`. 
9. Xây dựng lại ứng viên tốt nhất từ ​​mỗi mẫu trong số bốn mẫu và so sánh những mẫu ở cuối. Chỉ có bốn ứng cử viên hoàn chỉnh, vì vậy so sánh trực tiếp với họ sẽ tốn nhiều nhất`O(k)`tổng số công việc bổ sung. 

Tại sao nó hoạt động 

Bất biến trung tâm là mọi tập hợp vị trí còn tồn tại có thể tiếp cận chính xác là một khoảng liền kề với nhiều nhất một vị trí bổ sung ở mỗi bên. Các thao tác xóa sẽ bảo toàn thuộc tính này và mọi tập hợp có thuộc tính này có thể được xây dựng bằng cách xóa các ký tự không mong muốn khỏi phía tương ứng. 

Bốn trường hợp trong thuật toán liệt kê chính xác bốn hình dạng có thể có này. Trong mỗi trường hợp, các ký tự ranh giới được chọn sẽ được giảm thiểu độc lập vì chúng xuất hiện trước hoặc sau phần giữa liền kề. Thứ hạng bắt nguồn từ hậu tố so sánh chính xác các chuỗi con ở giữa vì tất cả các chuỗi con ở giữa trong một trường hợp đều có cùng độ dài. Do đó, mỗi trường hợp tạo ra mật khẩu có thể truy cập nhỏ nhất về mặt từ điển của nó và việc lấy mức tối thiểu trong số bốn ứng cử viên đó sẽ mang lại mức tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_suffix_array(s):
    n = len(s)

    # 1..26 are letters, 0 is the unique sentinel.
    a = [x - 96 for x in s] + [0]
    N = n + 1

    cnt = [0] * 27
    for x in a:
        cnt[x] += 1

    pos = [0] * 27
    for i in range(1, 27):
        pos[i] = pos[i - 1] + cnt[i - 1]

    p = [0] * N
    for i, x in enumerate(a):
        p[pos[x]] = i
        pos[x] += 1

    c = [0] * N
    classes = 1
    for i in range(1, N):
        if a[p[i]] != a[p[i - 1]]:
            classes += 1
        c[p[i]] = classes - 1

    shift = 1
    while shift < N:
        pn = [
            x - shift if x >= shift else x - shift + N
            for x in p
        ]

        cnt = [0] * classes
        for x in pn:
            cnt[c[x]] += 1

        pos = [0] * classes
        total = 0
        for i in range(classes):
            pos[i] = total
            total += cnt[i]

        p_new = [0] * N
        for x in pn:
            cls = c[x]
            p_new[pos[cls]] = x
            pos[cls] += 1

        c_new = [0] * N
        new_classes = 1
        for i in range(1, N):
            cur = p_new[i]
            prev = p_new[i - 1]

            cur_pair = (c[cur], c[(cur + shift) % N])
            prev_pair = (c[prev], c[(prev + shift) % N])

            if cur_pair != prev_pair:
                new_classes += 1

            c_new[cur] = new_classes - 1

        p = p_new
        c = c_new
        classes = new_classes
        shift <<= 1

    # Remove the sentinel suffix.
    return p[1:], a

def build_lcp(suffix_array, a):
    N = len(a)
    rank = [0] * N

    for i, pos in enumerate(suffix_array):
        rank[pos] = i

    lcp = [0] * N
    common = 0

    for i in range(N):
        r = rank[i]

        if r == 0:
            continue

        j = suffix_array[r - 1]

        while i + common < N and j + common < N:
            if a[i + common] != a[j + common]:
                break
            common += 1

        lcp[r] = common

        if common:
            common -= 1

    return lcp

def fixed_length_ranks(suffix_array, lcp, n, length):
    """
    rank[i] is the lexicographic rank of s[i:i+length].
    Equal substrings receive the same rank.
    """
    rank = [0] * n

    group = -1

    for idx, pos in enumerate(suffix_array):
        if idx == 0:
            group = 0
        elif lcp[idx] < length:
            group += 1

        rank[pos] = group

    return rank

def best_by_rank(rank, lo, hi):
    best = lo

    for i in range(lo + 1, hi + 1):
        if rank[i] < rank[best]:
            best = i

    return best

def solve_instance(s, k):
    n = len(s)

    if k == 1:
        return min(s)

    if k == 2:
        best = None
        right_min = s[-1]

        for i in range(n - 2, -1, -1):
            candidate = s[i] + right_min

            if best is None or candidate < best:
                best = candidate

            if s[i] < right_min:
                right_min = s[i]

        return best

    suffix_array, a = build_suffix_array(s)
    lcp = build_lcp(suffix_array, a)

    values = s.encode()

    # Prefix minima and suffix minima of characters.
    pref = bytearray(n)
    suf = bytearray(n + 1)

    pref[0] = values[0]
    for i in range(1, n):
        pref[i] = min(pref[i - 1], values[i])

    suf[n] = 123
    for i in range(n - 1, -1, -1):
        suf[i] = min(suf[i + 1], values[i])

    candidates = []

    # Case 1: one contiguous substring of length k.
    ranks = fixed_length_ranks(suffix_array, lcp, n, k)
    start = best_by_rank(ranks, 0, n - k)
    candidates.append(values[start:start + k])

    # Case 2: one character + substring of length k - 1.
    middle_len = k - 1
    ranks = fixed_length_ranks(
        suffix_array, lcp, n, middle_len
    )

    best_key = None
    best_start = -1
    best_left = -1

    for start in range(1, n - middle_len + 1):
        left_char = pref[start - 1]
        key = (left_char, ranks[start])

        if best_key is None or key < best_key:
            best_key = key
            best_start = start
            best_left = left_char

    candidates.append(
        bytes([best_left]) +
        values[best_start:best_start + middle_len]
    )

    # Case 3: substring of length k - 1 + one character.
    best_key = None
    best_start = -1
    best_right = -1

    for start in range(0, n - middle_len):
        end = start + middle_len
        right_char = suf[end]
        key = (ranks[start], right_char)

        if best_key is None or key < best_key:
            best_key = key
            best_start = start
            best_right = right_char

    candidates.append(
        values[best_start:best_start + middle_len] +
        bytes([best_right])
    )

    # Case 4: one character + substring of length k - 2
    # + one character.
    middle_len = k - 2
    ranks = fixed_length_ranks(
        suffix_array, lcp, n, middle_len
    )

    best_key = None
    best_start = -1
    best_left = -1
    best_right = -1

    for start in range(1, n - middle_len):
        end = start + middle_len

        left_char = pref[start - 1]
        right_char = suf[end]

        key = (left_char, ranks[start], right_char)

        if best_key is None or key < best_key:
            best_key = key
            best_start = start
            best_left = left_char
            best_right = right_char

    candidates.append(
        bytes([best_left]) +
        values[best_start:best_start + middle_len] +
        bytes([best_right])
    )

    return min(candidates).decode()

def solve():
    s = input().strip()
    k = int(input())
    print(solve_instance(s, k))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên xử lý hai giá trị nhỏ nhất của`k`riêng. Điều này tránh phải biểu diễn một khoảng trống ở giữa. Vì`k = 2`, mọi cặp vị trí còn sống đều có thể truy cập được, do đó, hậu tố tối thiểu từ phải sang trái là đủ. 

Đối với lớn hơn`k`,`build_suffix_array`thêm một lính canh nhỏ hơn mọi nhân vật thực. Cấu trúc nhân đôi tiêu chuẩn liên tục sắp xếp các hậu tố theo cặp lớp tương đương đại diện cho lớp đầu tiên của chúng.`2^h`nhân vật. Sắp xếp đếm giữ cho mỗi pha nhân đôi tuyến tính, đưa ra`O(n log n)`thời gian xây dựng.`build_lcp`sử dụng thuật toán Kasai. Mảng LCP là cần thiết vì chỉ có thứ hạng hậu tố đã phân biệt các hậu tố ngay cả khi lần đầu tiên chúng xuất hiện.`m`ký tự đều bằng nhau.`fixed_length_ranks`hợp nhất các hậu tố liên tiếp bất cứ khi nào tiền tố chung của chúng có độ dài ít nhất`m`, tạo ra các lớp từ điển chính xác có độ dài-`m`các chuỗi con. 

Tiền tố và hậu tố tối thiểu được lưu trữ trong`bytearray`đồ vật. Điều này giữ cho mức tiêu thụ bộ nhớ của chúng ở mức nhỏ trong khi vẫn cho phép truy cập liên tục vào ký tự bên ngoài nhỏ nhất có thể trong mỗi khoảng thời gian giữa. 

Phạm vi trong bốn trường hợp là khác nhau một cách có chủ ý. Đối với ký tự phụ bên trái, ký tự ở giữa ít nhất phải bắt đầu ở vị trí`1`, nhưng nó có thể kết thúc tại`n-1`. Đối với một ký tự phụ bên phải, phần giữa có thể bắt đầu tại`0`, nhưng phần cuối của nó phải để lại một ký tự sau nó. Với hai tính năng bổ sung, cả hai hạn chế đều được áp dụng. Đây là những nơi có nhiều khả năng xảy ra lỗi riêng lẻ nhất. 

Không có số nguyên nào có thể tràn trong Python và mảng hậu tố chỉ lưu trữ các chỉ số và thứ hạng số nguyên. Các ứng cử viên cuối cùng là các chuỗi byte, điều này cũng làm cho việc so sánh từ điển trở nên hiệu quả. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho`s = "abacaba"`Và`k = 3`, bốn dạng cấu trúc có độ dài trung bình`3`,`2`,`2`, Và`1`. 

| Mẫu | Xây dựng tốt nhất | Ứng viên | 
| --- | --- | --- | 
| Chỉ ở giữa |`aba`|`aba`| 
| Trái + giữa |`a`+`ab`|`aab`| 
| Giữa + phải |`ab`+`a`|`aba`| 
| Trái + giữa + phải |`a`+`a`+`a`|`aaa`| 

Hình thức cuối cùng sẽ thắng. Vị trí còn sót lại của nó là`1, 3, 5`. Bắt đầu từ`abacaba`, xóa ký tự thứ hai`b`, thì ký tự áp chót`b`, sau đó hai cái khác có thể truy cập được`c`Và`b`ký tự khi cần thiết, để lại`aaa`. 

Phần quan trọng của dấu vết là câu trả lời không phải là chuỗi con liền kề. Một giải pháp chỉ xem xét các chuỗi con sẽ dừng lại ở`aab`, trong khi các vị trí đơn trái và phải được phép làm`aaa`khả thi. 

### Mẫu 2 

cho`s = "qwerty"`Và`k = 2`, mọi cặp vị trí đều có thể truy cập được. 

| Vị trí đầu tiên | Nhân vật thứ hai tốt nhất có thể | Ứng viên | 
| --- | --- | --- | 
|`q`|`e`|`qe`| 
|`w`|`e`|`we`| 
|`e`|`r`|`er`| 
|`r`|`t`|`rt`| 
|`t`|`y`|`ty`| 

Ứng cử viên nhỏ nhất là`er`. 

Trường hợp này cũng chứng minh tại sao`k = 2`phím tắt rất hữu ích. Câu trả lời đơn giản là dãy con nhỏ nhất về mặt từ điển có độ dài bằng 2, có thể được tìm thấy với hậu tố tối thiểu mà không cần xây dựng một mảng hậu tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n)`| Cấu trúc mảng hậu tố chiếm ưu thế; LCP, thứ hạng có độ dài cố định và quá trình quét ứng viên là tuyến tính | 
| Không gian |`O(n)`| Mảng hậu tố, mảng LCP, mảng xếp hạng và mảng phụ đều tuyến tính | 

Với`n <= 500000`,`O(n log n)`là khả thi, trong khi không gian trạng thái brute-force tăng theo cấp số nhân. Mảng hậu tố được xây dựng một lần và ba độ dài chuỗi con cố định cần thiết cho các trường hợp cấu trúc được xử lý bằng quét tuyến tính sau đó. 

## Trường hợp thử nghiệm```
# Save the submitted solution as solution.py before running this block.
from solution import solve_instance

def run(inp: str) -> str:
    lines = inp.strip().splitlines()
    s = lines[0].strip()
    k = int(lines[1])
    return solve_instance(s, k)

# Provided samples
assert run("abacaba\n3\n") == "aaa", "sample 1"
assert run("qwerty\n2\n") == "er", "sample 2"

# Minimum-size input
assert run("z\n1\n") == "z", "minimum size"

# Two-character password, catches incorrect sorting of characters
assert run("bac\n2\n") == "ac", "two-character subsequence"

# Only one deletion is possible, so the interior character cannot be removed
assert run("abcde\n4\n") == "abcd", "one deletion boundary"

# All characters equal
assert run("aaaaa\n3\n") == "aaa", "all equal"

# No deletion is required
assert run("abc\n3\n") == "abc", "k equals n"

# Maximum-size case
s = "z" * 500000
assert run(s + "\n250000\n") == "z" * 250000, "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`z / 1`|`z`| Đầu vào tối thiểu có thể và`k = 1`| 
|`bac / 2`|`ac`| Dãy con hai vị trí và thứ tự | 
|`abcde / 4`|`abcd`| Các ký tự trong nội thất không thể xóa ngay lập tức | 
|`aaaaa / 3`|`aaa`| Ký tự bằng nhau và thứ hạng chuỗi con lặp lại | 
|`abc / 3`|`abc`| Không xóa | 
|`z...z / 250000`|`z...z`| Kích thước đầu vào tối đa và hậu tố lặp đi lặp lại nhiều | 

## Vỏ cạnh 

cho`s = "zba"`Và`k = 1`, thuật toán trả về ngay`a`. Mọi nhân vật đều có thể trở thành người sống sót duy nhất bằng cách xóa các ký tự ở phía thích hợp, vì vậy ký tự tối thiểu là đủ. 

Vì`s = "bac"`Và`k = 2`, thuật toán sẽ quét các vị trí đầu tiên có thể từ phải sang trái trong khi vẫn giữ ký tự nhỏ nhất ở bên phải của chúng. Nó xem xét`ac`từ vị trí`2,3`Và`ba`từ vị trí`1,3`, đang chọn`ac`. Sợi dây hấp dẫn`ab`không thể được hình thành vì các ký tự còn sống phải giữ nguyên trật tự ban đầu của chúng. 

Vì`s = "abcde"`Và`k = 4`, thao tác xóa duy nhất được thực hiện khi vẫn còn năm ký tự. Các ký tự có thể truy cập được là`a`,`b`,`d`, Và`e`, Vì thế`c`không thể được gỡ bỏ. Bốn chuỗi kết quả có giá trị tối thiểu`abcd`. Đặc tính cấu trúc cũng thấy trực tiếp điều này: xóa`c`sẽ để lại hai khoảng không đơn lẻ, đây không phải là một trong bốn dạng có thể truy cập được. 

Vì`s = "abacaba"`Và`k = 3`, mật khẩu tối ưu`aaa`có vị trí sống sót`1`,`3`, Và`5`. Các vị trí này tạo thành một dấu đơn bên trái, khoảng giữa một ký tự và một dấu đơn bên phải. Đây chính xác là hình dạng chung nhất được phép thực hiện bởi các thao tác xóa. 

Vì`s = "abc"`Và`k = 3`, every structural case that is valid reconstructs the original string, and no deletion is performed. Câu trả lời vẫn còn`abc`, xác nhận rằng các phép tính ranh giới không yêu cầu ký tự di động thực tế. 

Đối với chuỗi kích thước tối đa bao gồm toàn bộ`500000`bản sao của`z`, mọi độ dài có thể-`k`chuỗi con ở giữa giống hệt nhau. Quy trình xếp hạng có độ dài cố định nhóm chúng vào cùng một lớp tương đương, do đó, mọi vị trí bắt đầu hợp lệ đều được chấp nhận và câu trả lời thu được là kết quả mong đợi.`k`bản sao của`z`. 

Nếu bạn muốn, tôi cũng có thể cung cấp **phiên bản ngắn hơn theo phong cách Codeforces** của bài xã luận này, giữ nguyên bằng chứng nhưng giảm đáng kể phần thảo luận về việc triển khai.
