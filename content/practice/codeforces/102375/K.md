---
title: "CF 102375K - <<\u041a\u043e\u043d\u0442\u0430\u043a\u0442>> \u0434\u043b\u044f \u0434\u0432\u043e\u0438\u0445"
description: "Chúng tôi có một từ điển các từ đã biết. Đối với mỗi truy vấn, một mục từ điển được chọn làm từ bí mật (S) và một số nguyên (K) xác định số lần người chơi thứ hai đoán không thành công trước khi một chữ cái khác của (S) được tiết lộ."
date: "2026-08-12T22:43:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102375
codeforces_index: "K"
codeforces_contest_name: "\u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434 \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442\u0430 \u0421\u0435\u0432\u0435\u0440\u043e-\u0417\u0430\u043f\u0430\u0434\u0430 \u0420\u043e\u0441\u0441\u0438\u0438 \u0438 \u041c\u043e\u0441\u043a\u0432\u044b ICPC 2019"
rating: 0
weight: 102375
solve_time_s: 457
verified: true
draft: false
---

[CF 102375K - <<\u041a\u043e\u043d\u0442\u0430\u043a\u0442>> \u0434\u043b\u044f \u0434\u0432\u043e\u0438\u0445](https://codeforces.com/problemset/problem/102375/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 37 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một từ điển các từ đã biết. Đối với mỗi truy vấn, một mục từ điển được chọn làm từ bí mật (S) và một số nguyên (K) xác định số lần người chơi thứ hai đoán không thành công trước khi một chữ cái khác của (S) được tiết lộ. 

Tại bất kỳ thời điểm nào người chơi cũng biết tiền tố của (S). Họ có thể đặt tên cho bất kỳ từ trong từ điển nào chưa được sử dụng trước đây có tiền tố đó. Bản thân từ bí mật cũng có trong từ điển và cách viết giống nhau vẫn là các mục từ điển khác nhau nên mỗi lần xuất hiện phải được tính riêng. 

Valera muốn trò chơi kéo dài càng lâu càng tốt. Đối với truy vấn ((w,K)), câu trả lời bắt buộc là số lần đoán lớn nhất có thể, bao gồm cả lần đoán thành công nếu cuối cùng người chơi đoán được từ bí mật. 

Tổng độ dài của tất cả các từ trong từ điển tối đa là (2\cdot10^5) và có thể có (2\cdot10^5) truy vấn. Điều này loại trừ việc thực hiện công việc tương ứng với toàn bộ từ điển cho mỗi truy vấn. Ngay cả một giải pháp xử lý trước số lượng tiền tố nhưng sau đó duyệt toàn bộ từ bí mật cho mỗi truy vấn cũng có thể đạt được khoảng (2\cdot10^{10}) thao tác khi nhiều truy vấn đề cập đến một từ dài. Giải pháp phải làm cho quá trình tiền xử lý về cơ bản là tuyến tính trong tổng kích thước đầu vào và giữ cho mỗi truy vấn ở dạng logarit hoặc gần với nó. 

Có một số trường hợp khó xử lý. Đầu tiên, từ bí mật có thể là từ duy nhất có chữ cái đầu tiên. Ví dụ,```
1
a
1
1 1
```có câu trả lời`1`. Không có trường hợp đoán sai nên người chơi đặt tên ngay cho từ bí mật. Một công thức chỉ tính những lần đoán không thành công sẽ trả về 0 không chính xác. 

Thứ hai, các mục từ điển trùng lặp rất quan trọng. Coi như```
3
aa
ab
ab
2
2 2
```Câu trả lời là`3`. Với (K=2), người chơi có thể đặt tên`aa`và sự xuất hiện khác của`ab`, sau đó người chơi đầu tiên tiết lộ rằng toàn bộ từ đó là`ab`. Việc coi từ điển như một bộ sẽ làm mất đi một dự đoán có sẵn và tạo ra kết quả sai. 

Thứ ba, những từ được đặt tên trước đó không thể được sử dụng lại. Ví dụ,```
4
abc
abd
abe
abf
2
1 2
```có câu trả lời`4`. Trong vòng đầu tiên, hai trong số`abd`,`abe`,`abf`phải được đặt tên. Chỉ còn lại một từ sai khi tiền tố trở thành`ab`, do đó vòng thứ hai bao gồm lần đoán sai đó, sau đó là lần đoán thành công`abc`. Việc đếm số lượng từ dưới mỗi tiền tố một cách độc lập sẽ cho rằng những từ tương tự có thể được sử dụng lại một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng trò chơi. Đối với mỗi truy vấn, chúng tôi có thể kiểm tra tất cả các từ trong từ điển khớp với tiền tố hiện đã biết, chọn những từ sai phù hợp, loại bỏ chúng khỏi xem xét, hiển thị chữ cái tiếp theo sau (K) đoán và tiếp tục. Điều này đúng vì lựa chọn chiến lược duy nhất là sử dụng những từ phù hợp không được sử dụng làm dự đoán sai. 

Việc triển khai theo nghĩa đen sẽ quét tất cả (N) từ trong từ điển ở mọi tiền tố có giá (O(N|S|)) cho một truy vấn. Trong giới hạn nhất định, điều này có thể đạt được khoảng (2\cdot10^{15}) kiểm tra cơ bản đối với tất cả các truy vấn trong một cấu trúc có nhiều truy vấn và một từ bí mật dài, vì vậy nó không thể sử dụng được. 

Một cải tiến tự nhiên là xây dựng một bộ ba và lưu trữ bao nhiêu mục từ điển đi qua mỗi tiền tố. Sau đó, một truy vấn chỉ có thể kiểm tra các tiền tố của từ bí mật của nó. Điều này làm giảm một truy vấn xuống còn (O(|S|)), nhưng (2\cdot10^5) truy vấn vẫn có thể yêu cầu (O(Q\cdot |S|)) hoạt động, vốn quá lớn. 

Quan sát quan trọng là ngừng suy nghĩ về từng từ riêng lẻ và thay vào đó hãy ấn định thời hạn cho mỗi từ trong từ điển sai. Đối với một từ sai (T), gọi (d) là độ dài của tiền tố chung dài nhất của (T) và từ bí mật (S). Từ có thể được đoán trong các vòng (1,\ldots,d), nhưng sau khi chữ cái thứ (d) được tiết lộ thì từ đó không còn là từ đoán hợp lệ nữa. 

Mỗi vòng cần đoán sai chính xác (K) nếu trò chơi tiếp tục. Do đó, bài toán trở thành bài toán lập kế hoạch: mỗi từ sai là một công việc có thời hạn (d) và mỗi vòng đều có các ô (K). Người chơi nên sử dụng những từ có thời hạn sớm hơn trước vì những từ đó sẽ biến mất sớm hơn. 

Gọi (C_d) là số mục từ điển sai có tiền tố chung dài nhất với (S) có độ dài ít nhất là (d). Trong thuật ngữ trie, đây chỉ đơn giản là số mục từ điển có (d) chữ cái đầu tiên của (S), trừ đi mục nhập bí mật. 

Giả sử chúng ta muốn hoàn thành (r) toàn bộ vòng. Hãy xem xét các vòng (r-d+1) cuối cùng, từ (d) đến (r). Mỗi lần đoán được đặt ở đó phải sử dụng một từ có thời hạn ít nhất là (d) và có (K(r-d+1)) số lần đoán bắt buộc. Do đó chúng ta cần 

[ 
C_d \ge K(r-d+1) 
] 

với mọi (d\le r). 

Điều kiện này cũng đủ. Các tập hợp từ có thời hạn ít nhất (d) được lồng vào nhau, do đó, đối số lập kế hoạch tham lam thông thường được áp dụng: luôn sử dụng một từ có sẵn với thời hạn nhỏ nhất. Những bất đẳng thức về hậu tố chính xác là những điều kiện về năng lực mà lịch trình tham lam đó yêu cầu. 

Sắp xếp lại bất đẳng thức cho 

[ 
r \le d-1+\left\lfloor\frac{C_d}{K}\right\rfloor. 
] 

Do đó, số vòng hoàn thành tối đa là 

[ 
r=\min_d\left(d-1+\left\lfloor\frac{C_d}{K}\right\rfloor\right). 
] 

Biểu thức bên trong mức tối thiểu có dạng đặc biệt hữu ích: 

\left\lfloor\frac{C_d+K(d-1)}{K}\right\rfloor. 
] 

Đối với một từ bí mật cố định, mỗi độ sâu (d) sẽ đóng góp một dòng 

[ 
f_d(x)=C_d+(d-1)x, 
] 

và một truy vấn yêu cầu giá trị tối thiểu của những dòng này tại (x=K). Đây chính xác là một bài toán đánh lừa bao lồi dưới. Chúng tôi xây dựng phần thân một lần cho mỗi từ trong từ điển và trả lời từng truy vấn bằng tìm kiếm nhị phân trên phần thân đó. 

Sau khi (r) hoàn thành các vòng, nếu (r) nhỏ hơn độ dài bí mật thì vòng tiếp theo không thể chứa (K) dự đoán sai. Gọi (W=N-1) là tổng số mục từ điển sai. Khi đó số từ sai còn dùng được là 

[ 
R=\min(C_{r+1}, W-rK). 
] 

Người chơi đặt tên cho các từ (R) đó rồi đặt tên cho từ bí mật thành công nên đáp án cuối cùng là 

[ 
rK+R+1. 
] 

Nếu (r) bằng độ dài bí mật thì tất cả các vòng đã hoàn thành và người chơi đầu tiên tiết lộ toàn bộ từ, do đó câu trả lời đơn giản là (rK).

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng theo nghĩa đen | (O(QN | S | )) trong trường hợp xấu nhất | (O(N)) | Quá chậm | 
| Trie plus mô phỏng | (O(\tổng | S | +Q | S | )) | (O(\tổng | S | )) | Quá chậm | 
| Trie cộng với vỏ lồi | (O(L+Q\log L)), trong đó (L) là tổng chiều dài từ | (O(L)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một bộ ba chứa mọi từ trong từ điển. Mỗi nút trie lưu trữ bao nhiêu mục từ điển đi qua nó. Các từ giống nhau được chèn riêng biệt nên tính đa dạng của chúng được bảo toàn tự động. 
2. Với mỗi từ trong từ điển (S), hãy đi qua đường đi của nó trong bộ thử. Với mỗi độ dài tiền tố (d), hãy lưu trữ 

[ 
C_d=\text{count(tiền tố có độ dài }d)-1. 
] 

Phép trừ sẽ loại bỏ mục từ điển cụ thể được chọn làm từ bí mật. Các mục giống hệt khác vẫn được tính, đó chính xác là những gì trò chơi yêu cầu. 

1. Giải thích mọi từ sai như một công việc có thời hạn là tiền tố chung dài nhất của nó với (S). Một từ có thời hạn (d) có thể được đoán trong bất kỳ vòng (d) đầu tiên nào. Giá trị (C_d) chính xác là số lượng công việc có thời hạn ít nhất là (d). 
2. Với mỗi độ dài tiền tố (d), hãy tạo dòng 

[ 
y=C_d+(d-1)x. 
] 

Đối với giá trị truy vấn (K), giá trị dòng tối thiểu chia cho (K) sẽ cho số vòng hoàn thành tối đa: 

[ 
r=\min\left(|S|,\left\lfloor\frac{\min_d(C_d+K(d-1))}{K}\right\rfloor\right). 
] 

Bao lồi chỉ lưu trữ các dòng có thể tối ưu, do đó mức tối thiểu có thể được tìm thấy theo thời gian logarit. 

1. Nếu (r=|S|), trò chơi sẽ tồn tại ở mọi vòng. Sau khi (K) đoán ở vòng chung kết, tất cả các chữ cái đều lộ ra nên đáp án là (rK). 
2. Mặt khác, tiền tố đã biết tiếp theo có độ dài (r+1). Có (C_{r+1}) mục từ điển sai dưới tiền tố đó, nhưng một số (rK) dự đoán trước đó có thể đến từ cùng cây con này. Số lượng từ sai còn có thể sử dụng tối thiểu còn lại là 

[ 
R=\min(C_{r+1},N-1-rK). 
] 

Người chơi có thể đặt tên cho tất cả (R) trong số đó và sau đó phải đặt tên cho từ bí mật nên câu trả lời là (rK+R+1). 

1. Lặp lại truy vấn thân cho mọi cặp được yêu cầu ((w,K)). Quá trình xử lý trước một từ độc lập với mọi từ khác, do đó, các cặp truy vấn trùng lặp không yêu cầu thực hiện thêm thao tác nào. 

Tại sao nó hoạt động 

Bất biến trung tâm là mỗi từ sai chỉ được đặc trưng bởi vòng cuối cùng mà nó vẫn có thể được đặt tên, cụ thể là độ dài tiền tố chung dài nhất của nó với từ bí mật. Để hoàn thành (r) vòng, vòng (r-d+1) cuối cùng yêu cầu (K(r-d+1)) từ có thời hạn ít nhất là (d). Các bất đẳng thức (C_d\ge K(r-d+1)) là cần thiết và vì các tập đủ điều kiện được lồng nhau nên chúng đủ cho chiến lược tham lam luôn dành những từ có thời hạn sớm nhất trước tiên. Bao lồi tính toán (r) lớn nhất thỏa mãn tất cả các bất đẳng thức này. Sau khi hoàn thành nhiều vòng hoàn chỉnh, công thức cho (R) sẽ tính chính xác có thể còn lại bao nhiêu từ sai có thể sử dụng được, sau đó từ bí mật nhất thiết phải là lần đoán tiếp theo nếu trò chơi chưa kết thúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_hull(values):
    hull = []

    for slope, intercept in enumerate(values):
        while len(hull) >= 2:
            m1, b1 = hull[-2]
            m2, b2 = hull[-1]
            m3, b3 = slope, intercept

            # l2 is redundant if the intersection of l1,l2
            # is not to the right of the intersection of l2,l3.
            if (b1 - b2) * (m3 - m2) <= (b2 - b3) * (m2 - m1):
                hull.pop()
            else:
                break

        hull.append((slope, intercept))

    return hull

def query_hull(hull, x):
    lo = 0
    hi = len(hull) - 1

    while lo < hi:
        mid = (lo + hi) // 2

        m1, b1 = hull[mid]
        m2, b2 = hull[mid + 1]

        if m1 * x + b1 <= m2 * x + b2:
            hi = mid
        else:
            lo = mid + 1

    m, b = hull[lo]
    return m * x + b

def solve():
    n = int(input())
    words = [input().strip() for _ in range(n)]

    children = [{}]
    count = [0]

    # Build the trie and count how many dictionary entries
    # pass through every node.
    for word in words:
        node = 0
        for ch in word:
            nxt = children[node].get(ch)
            if nxt is None:
                nxt = len(children)
                children[node][ch] = nxt
                children.append({})
                count.append(0)

            node = nxt
            count[node] += 1

    hulls = [None] * n
    prefix_counts = [None] * n

    # For every possible secret word, prepare C_d and its
    # lower hull of lines C_d + (d-1) * x.
    for idx, word in enumerate(words):
        node = 0
        values = []

        for ch in word:
            node = children[node][ch]
            values.append(count[node] - 1)

        prefix_counts[idx] = values
        hulls[idx] = build_hull(values)

    q = int(input())
    total_wrong = n - 1
    out = []

    for _ in range(q):
        w, k = map(int, input().split())
        w -= 1

        values = prefix_counts[w]
        length = len(values)
        hull = hulls[w]

        minimum = query_hull(hull, k)
        rounds = minimum // k

        if rounds > length:
            rounds = length

        completed = rounds * k

        if rounds == length:
            out.append(str(completed))
            continue

        remaining = min(values[rounds], total_wrong - completed)
        answer = completed + remaining + 1
        out.append(str(answer))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc xây dựng trie tuân theo phần đầu tiên của hướng dẫn. Mỗi lần xuất hiện của một từ sẽ tăng số bộ đếm trên đường dẫn tiền tố hoàn chỉnh của nó, do đó, hai cách viết giống hệt nhau sẽ đóng góp hai mục nhập riêng biệt. 

Đối với mỗi từ,`values[d]`cửa hàng (C_{d+1}). Bản thân từ đó sẽ bị trừ đúng một lần bởi`count[node] - 1`, trong khi các mục từ điển khác có cùng cách viết vẫn tồn tại.`build_hull`nhận các đường có độ dốc là (0,1,2,\ldots). Việc so sánh sản phẩm chéo tránh tọa độ giao điểm dấu phẩy động. Vì tất cả các giá trị đều là số nguyên và có thể lớn bằng khoảng (K|S|), số học số nguyên đương nhiên cũng đủ. Số nguyên Python không có vấn đề tràn.`query_hull`so sánh hai đường thân liền kề. Thân dưới làm cho các giá trị dòng được sắp xếp xung quanh mức tối thiểu của chúng, vì vậy tìm kiếm nhị phân sẽ tìm ra dòng tối ưu trong (O(\log |S|)). 

Biểu thức được trả về bởi thân tàu là 

[ 
M=\min_d(C_d+K(d-1)). 
] 

Chia nó cho (K) với phép chia số nguyên sẽ cho số vòng hoàn thành tối đa. các`rounds > length`Guard xử lý trường hợp lý thuyết trong đó biểu thức tối thiểu sẽ vượt quá số lượng chữ cái. 

Tính toán cuối cùng sử dụng`values[rounds]`, bởi vì`rounds`vòng hoàn chỉnh có nghĩa là tiền tố đã biết tiếp theo có độ dài`rounds + 1`, tương ứng với chỉ số dựa trên 0`rounds`. các`+1`ở câu trả lời cuối cùng là đoán thành công từ bí mật khi trò chơi dừng lại trước khi tất cả các chữ cái được tiết lộ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Hãy xem xét từ điển đầu tiên`asassin`. Số từ sai có liên quan của nó dọc theo đường dẫn bí mật là 

[ 
C_1=5,\qquad C_2=2,\qquad C_3=0. 
] 

Số lượng tiền tố sau này cũng bằng 0, vì vậy chúng không bao giờ có thể cải thiện mức tối thiểu của thân tàu. 

Với (K=1), ba dòng liên quan là (5), (2+x) và (2x). Mức tối thiểu của họ tại (x=1) là (2), vì vậy có thể có hai vòng hoàn chỉnh. Tiền tố tiếp theo không có từ sai, do đó từ bí mật sẽ được đoán ở lần thử thứ ba. 

| Độ dài tiền tố (d) | (C_d) | Dòng (C_d+(d-1)K), (K=1) | 
| --- | --- | --- | 
| 1 | 5 | 5 | 
| 2 | 2 | 3 | 
| 3 | 0 | 2 | 

Do đó (r=2) và câu trả lời là (2\cdot1+0+1=3). 

Với (K=2), các giá trị dòng là (5,4,4). Tối thiểu là (4), cho (r=2). Một lần nữa không còn từ nào sai ở tiền tố thứ ba, vì vậy câu trả lời là (4+1=5). 

Với (K=3), giá trị dòng là (5,5,6). Tối thiểu là (5), cho (r=1). Sau ba lần đoán ở vòng đầu tiên, hai từ sai có thể sử dụng được vẫn ở tiền tố thứ hai. Người chơi đặt tên cho hai người đó rồi đoán từ bí mật. 

| (K) | Thân tàu tối thiểu | Hoàn thành vòng (r) | Còn lại dự đoán sai | Câu trả lời cuối cùng | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 0 | 3 | 
| 2 | 4 | 2 | 0 | 5 | 
| 3 | 5 | 1 | 2 | 6 | 

Dấu vết này chứng minh tại sao các từ được sử dụng trước đó phải được coi là công việc được lên lịch thay vì được tính độc lập ở mọi tiền tố. Đối với (K=3), vòng đầu tiên sử dụng một số từ mà lẽ ra thuộc về tiền tố sâu hơn. 

### Mẫu 2 

Lấy mục từ điển 2,`ab`, như một từ bí mật. Có hai mục từ điển khác dưới tiền tố`a`:`aa`và lần xuất hiện thứ hai của`ab`. Như vậy (C_1=2). Dưới tiền tố đầy đủ`ab`, chỉ có sự xuất hiện khác của`ab`vẫn còn, do đó (C_2=1). 

Với (K=1), các đường có giá trị (2) và (2). Có thể thực hiện được hai vòng hoàn chỉnh. 

| Độ dài tiền tố (d) | (C_d) | Dòng (C_d+(d-1)K), (K=1) | 
| --- | --- | --- | 
| 1 | 2 | 2 | 
| 2 | 1 | 2 | 

Họ tên người chơi`aa`, sau đó người chơi đầu tiên tiết lộ`b`. Người chơi tiếp theo đặt tên cho người khác`ab`, và người chơi đầu tiên kết thúc trò chơi vì toàn bộ từ đã bị lộ. Câu trả lời là`2`. 

Đối với (K=2), giá trị dòng là (2) và (3), do đó chỉ có thể thực hiện được một vòng hoàn chỉnh. Sau hai lần đoán ở vòng đó, không sai`ab`mục nhập vẫn còn và từ bí mật được đoán thành công. 

| (K) | Thân tàu tối thiểu | Hoàn thành vòng (r) | Còn lại dự đoán sai | Câu trả lời cuối cùng | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 0 | 2 | 
| 2 | 2 | 1 | 0 | 3 | 

Truy vấn thứ hai giải thích tại sao cách viết trùng lặp phải giữ nguyên các mục từ điển riêng biệt. thứ hai`ab`là một sự đoán sai chính đáng mặc dù cách viết của nó giống hệt với từ bí mật. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(L+Q\log L)) | Cấu trúc trie và cấu trúc toàn bộ thân tàu lấy (O(L)), trong đó (L) là tổng chiều dài của tất cả các từ. Mỗi truy vấn thực hiện một tìm kiếm nhị phân thân. | 
| Không gian | (O(L)) | Các mảng trie, đếm tiền tố, bao lồi và các từ được lưu trữ chứa tổng thông tin (O(L)). | 

Ở đây (L\le2\cdot10^5) và (Q\le2\cdot10^5). Quá trình tiền xử lý là tuyến tính theo kích thước đầu vào thực tế, trong khi mỗi truy vấn chỉ chạm vào phần thân thuộc từ được yêu cầu. Thân lớn nhất được giới hạn bởi độ dài của từ đó, do đó giới hạn truy vấn logarit vẫn nhỏ. 

## Trường hợp thử nghiệm```python
import sys
import io

def build_hull(values):
    hull = []

    for slope, intercept in enumerate(values):
        while len(hull) >= 2:
            m1, b1 = hull[-2]
            m2, b2 = hull[-1]
            m3, b3 = slope, intercept

            if (b1 - b2) * (m3 - m2) <= (b2 - b3) * (m2 - m1):
                hull.pop()
            else:
                break

        hull.append((slope, intercept))

    return hull

def query_hull(hull, x):
    lo = 0
    hi = len(hull) - 1

    while lo < hi:
        mid = (lo + hi) // 2

        m1, b1 = hull[mid]
        m2, b2 = hull[mid + 1]

        if m1 * x + b1 <= m2 * x + b2:
            hi = mid
        else:
            lo = mid + 1

    m, b = hull[lo]
    return m * x + b

def solve_stream(stream):
    input = stream.readline

    n = int(input())
    words = [input().strip() for _ in range(n)]

    children = [{}]
    count = [0]

    for word in words:
        node = 0

        for ch in word:
            nxt = children[node].get(ch)

            if nxt is None:
                nxt = len(children)
                children[node][ch] = nxt
                children.append({})
                count.append(0)

            node = nxt
            count[node] += 1

    hulls = [None] * n
    prefix_counts = [None] * n

    for idx, word in enumerate(words):
        node = 0
        values = []

        for ch in word:
            node = children[node][ch]
            values.append(count[node] - 1)

        prefix_counts[idx] = values
        hulls[idx] = build_hull(values)

    q = int(input())
    total_wrong = n - 1
    answer = []

    for _ in range(q):
        w, k = map(int, input().split())
        w -= 1

        values = prefix_counts[w]
        length = len(values)

        rounds = query_hull(hulls[w], k) // k
        rounds = min(rounds, length)

        completed = rounds * k

        if rounds == length:
            answer.append(str(completed))
        else:
            remaining = min(values[rounds], total_wrong - completed)
            answer.append(str(completed + remaining + 1))

    return "\n".join(answer)

def run(inp: str) -> str:
    return solve_stream(io.StringIO(inp)).strip()

# Provided sample 1
assert run("""\
6
asassin
assistant
astronaut
abrakadabra
abbey
automaton
9
1 1
1 2
1 3
4 1
4 2
4 3
6 1
6 2
6 3
""") == """\
3
5
6
3
4
5
2
3
4
""", "sample 1"

# Provided sample 2
assert run("""\
3
aa
ab
ab
6
1 1
2 1
1 2
3 2
2 2
3 1
""") == """\
2
2
3
3
3
2
""", "sample 2"

# Provided sample 3
assert run("""\
7
pit
pitbul
piter
pitstop
pitlane
petroleum
pistol
6
1 2
1 3
6 4
7 2
7 3
5 1
""") == """\
6
7
5
5
7
4
""", "sample 3"

# Minimum-size dictionary.
assert run("""\
1
a
1
1 1
""") == "1", "only word is the secret"

# Duplicate spellings and K at the boundary.
assert run("""\
3
aa
ab
ab
2
2 2
2 1
""") == """\
3
2
""", "duplicates must remain distinct"

# Previously used words cannot be reused.
assert run("""\
4
abc
abd
abe
abf
2
1 2
1 1
""") == """\
4
3
""", "reuse of guesses"

# Maximum-size construction: 200000 identical one-letter words.
maximum_case = (
    "200000\n"
    + "a\n" * 200000
    + "2\n"
    + "1 1\n"
    + "1 200000\n"
)

assert run(maximum_case) == """\
1
200000
""", "maximum N and duplicate count"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / a / 1 / 1 1`|`1`| Từ điển tối thiểu và đoán thành công ngay lập tức | 
| Ba mục`aa, ab, ab`|`3, 2`| Các mục từ điển trùng lặp và hành vi ranh giới (K) | 
|`abc, abd, abe, abf`|`4, 3`| Những từ đã dùng trước đây không được tính lại | 
| 200000 bản sao`a`|`1, 200000`| Cực đại (N), cực đại (K) và bội số lớn | 

## Vỏ cạnh 

Đối với trường hợp một từ```
1
a
1
1 1
```trie chứa một nút cho tiền tố`a`, có số lượng từ điển là một. Sau khi trừ đi mục bí mật, (C_1=0). Thân chứa đường thẳng (y=0), do đó (r=0). Không còn từ sai nào và đáp án là (0+0+1=1). Thuật toán đếm chính xác số lần đoán thành công. 

Đối với cách viết trùng lặp,```
3
aa
ab
ab
1
2 2
```số lần thử cho`a`là ba, vì vậy (C_1=2). Tính cho`ab`là hai, vì vậy (C_2=1). Với (K=2), giá trị thân tàu là (2) và (3), cho ra (r=1). Sau hai lần đoán đầu tiên, không sai`ab`mục nhập vẫn còn, vì vậy từ bí mật được đoán tiếp theo. Kết quả là (2+0+1=3). Sự xuất hiện trùng lặp được bảo tồn trong suốt quá trình tiền xử lý. 

Đối với trường hợp không tái sử dụng,```
4
abc
abd
abe
abf
1
1 2
```có ba từ sai và đều có cùng thời hạn (2). Do đó (C_1=3) và (C_2=3). Với (K=2), thân tàu cho 

[ 
\min(3,3+2)=3, 
] 

vậy (r=1). Vòng đầu tiên dùng sai hai từ. Tại tiền tố`ab`, chỉ còn lại một từ sai nên vòng thứ hai có một lần đoán sai, sau đó là lần đoán thành công`abc`. Câu trả lời là (4). Mô hình thân tàu thể hiện thực tế là hai trong số ba từ đã được sử dụng. 

Đối với trường hợp trùng lặp tối đa, có (200000) bản sao`a`. Đối với (K=1), từ bí mật có thể bị trì hoãn do đoán sai, nhưng từ đó đã được tiết lộ đầy đủ sau vòng đó, đưa ra câu trả lời`1`. Với (K=200000), có (199999) lần xuất hiện sai và sau đó bản thân sự xuất hiện bí mật đó có thể được đặt tên, đưa ra chính xác`200000`. Thuật toán lưu trữ bội số thay vì loại bỏ từ điển, do đó cả hai trường hợp biên đều được xử lý chính xác.
