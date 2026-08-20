---
title: "CF 102203G - \u0417\u0430\u043f\u0443\u0442\u044b\u0432\u0430\u043d\u0438\u0435 \u0441\u043b\u0435\u0434\u043e\u0432"
description: "Chỉ có 8 quận, vì vậy mỗi tác nhân có thể được biểu diễn bằng ma trận chuyển đổi nhị phân 8 × 8. Đối với đặc vụ (k), mục nhập (Ak[u][v]) chính xác là 1 khi đặc vụ đó có thể đưa Rick và Vallona từ quận (u) đến quận (v)."
date: "2026-08-18T11:20:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102203
codeforces_index: "G"
codeforces_contest_name: "\u0427\u0435\u0442\u0432\u0435\u0440\u0442\u0430\u044f \u041b\u0438\u043f\u0435\u0446\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e (8-11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 102203
solve_time_s: 200
verified: true
draft: false
---

[CF 102203G - \u0417\u0430\u043f\u0443\u0442\u044b\u0432\u0430\u043d\u0438\u0435 \u0441\u043b\u0435\u0434\u043e\u0432](https://codeforces.com/problemset/problem/102203/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chỉ có 8 quận, vì vậy mỗi tác nhân có thể được biểu diễn bằng ma trận chuyển đổi nhị phân 8 × 8. Đối với đặc vụ (k), mục nhập (A_k[u][v]) chính xác là 1 khi đặc vụ đó có thể đưa Rick và Vallona từ quận (u) đến quận (v). 

Đối với một truy vấn ([l,r,s,t]), các tác nhân phải được sử dụng theo thứ tự cố định (l,l+1,\ldots,r). Nếu chúng ta viết ma trận của chúng dưới dạng (A_l,A_{l+1},\ldots,A_r), thì số lượng đường đi có thể chính xác là mục ((s,t)) của 

[ 
A_lA_{l+1}\cdots A_r 
] 

trong đó phép nhân là phép nhân ma trận thông thường theo modulo 998244353. Phép nhân tính tổng trên mọi quận trung gian có thể có, do đó, một tích ma trận đơn sẽ tính mọi tuyến đường riêng biệt chính xác một lần. Các ràng buộc và mẫu chính thức được đưa ra trên trang vấn đề của Codeforces. 

Đầu vào chứa tối đa (10^5) tác nhân và (2\cdot10^5) truy vấn. Quét trực tiếp một truy vấn có thể chạm vào (10^5) ma trận, con số này đã quá nhiều khi lặp lại (2\cdot10^5) lần. Ngay cả phiên bản lập trình động tự nhiên, duy trì vectơ 8 phần tử và áp dụng một ma trận nhị phân trong 64 phép tính vô hướng, cũng có thể đạt tới 

[ 
2\cdot10^5\cdot10^5\cdot64=1,28\cdot10^{12} 
] 

các phép toán vô hướng. Cây phân đoạn thông thường giảm số lượng tích ma trận trên mỗi truy vấn xuống (O(\log n)), nhưng mỗi sản phẩm là một phép nhân ma trận đầy đủ 8 × 8, do đó Python vẫn sẽ thực hiện một khối lượng công việc khổng lồ. 

Điều kiện bất thường trong các truy vấn là chìa khóa. Không có khoảng thời gian truy vấn nào được chứa đúng cách bên trong một khoảng thời gian khác. Nếu hai khoảng được sắp xếp theo điểm cuối bên trái của chúng và khoảng đầu tiên có điểm cuối bên phải lớn hơn khoảng thứ hai thì khoảng thứ hai sẽ được chứa trong khoảng thứ nhất. Do đó, sau khi sắp xếp theo (l), các điểm cuối bên phải cũng không giảm. Các điểm cuối bên trái bằng nhau có thể được sắp xếp đơn giản bằng cách tăng điểm cuối bên phải. 

Điều này có nghĩa là phạm vi được truy vấn có thể được xem dưới dạng cửa sổ trượt. Điểm cuối bên trái chỉ di chuyển sang bên phải và điểm cuối bên phải chỉ di chuyển sang bên phải. Chúng ta cần một cấu trúc dữ liệu duy trì sản phẩm của một chuỗi đồng thời hỗ trợ nối thêm ở bên phải và xóa từ bên trái. Cấu trúc tổng hợp cửa sổ trượt hai ngăn tiêu chuẩn thực hiện chính xác điều đó đối với bất kỳ phép toán kết hợp nào, bao gồm cả phép nhân ma trận không giao hoán. 

Có một số trường hợp khó xử lý. Một khoảng có độ dài phải trả về trực tiếp mục nhập ma trận tương ứng. Ví dụ,```
1 1
9223372036854775808
1 1 1 1
```chỉ có sự chuyển đổi (1\thành 1), vì vậy câu trả lời là`1`. Việc xử lý truy vấn như thể nó có một sản phẩm trống sẽ trả về không chính xác một mục nhập ma trận danh tính thay vì chuyển đổi tác nhân thực tế. 

Một tuyến đường có thể quay lại cùng một quận, kể cả ngay lập tức. Ví dụ,```
2 1
9223372036854775808
9223372036854775808
1 2 1 1
```có chính xác một gốc, (1\to 1\to 1), vì vậy câu trả lời là`1`. Một giải pháp cho rằng mọi chuyển động đều phải thay đổi quận sẽ loại bỏ tuyến đường này một cách không chính xác. 

Cuối cùng, một ma trận có thể không chứa sự chuyển tiếp nào cả. Ví dụ,```
1 1
0
1 1 1 1
```có câu trả lời`0`. Việc triển khai vô tình sử dụng ma trận nhận dạng cho tác nhân 0 hoặc gây nhầm lẫn giữa nhận dạng sản phẩm trống với ma trận chuyển tiếp bằng 0 thực tế sẽ không thành công trong trường hợp này. 

## Phương pháp tiếp cận 

Phương pháp đúng trực tiếp nhất là xử lý mọi truy vấn một cách độc lập. Bắt đầu với một vectơ 8 phần tử chứa một phần tử ở quận bắt đầu và 0 phần tử ở nơi khác. Đối với mọi tác nhân từ (l) đến (r), hãy nhân vectơ này với ma trận chuyển đổi nhị phân của tác nhân đó. Sau tác nhân cuối cùng, thành phần tương ứng với (t) là câu trả lời. Điều này đúng vì sau khi xử lý (k) tác nhân đầu tiên, vectơ sẽ lưu số cách để đến mọi quận sau chính xác các tác nhân đó. 

Vấn đề là việc quét lặp đi lặp lại. Một truy vấn có thể chứa (10^5) tác nhân và có thể có (2\cdot10^5) truy vấn. Trường hợp xấu nhất là khoảng (1,28\cdot10^{12}) phép toán ma trận vectơ vô hướng, vượt xa giới hạn. 

Cấu trúc dữ liệu phạm vi sản phẩm tiêu chuẩn sẽ lưu trữ các sản phẩm của các phân khúc trong cây phân khúc. Sau đó, một truy vấn có thể kết hợp các ma trận được tính toán trước (O(\log n)). Đại số hoàn toàn hợp lệ vì phép nhân ma trận có tính kết hợp nhưng nó không khai thác điều kiện truy vấn đặc biệt. Với (2\cdot10^5) truy vấn và khoảng 17 cấp độ cây, đó là hàng triệu phép nhân ma trận 8 × 8. 

Quan sát mang tính quyết định là các truy vấn tạo thành một chuỗi đơn điệu sau khi sắp xếp. Ranh giới bên trái không bao giờ lùi lại và ranh giới bên phải cũng vậy. Do đó, chúng tôi có thể duy trì chính xác khoảng thời gian truy vấn hiện tại dưới dạng chuỗi FIFO của ma trận tác nhân. 

Trở ngại là phép nhân ma trận nói chung không khả nghịch. Nếu tích hiện tại là (A_lA_{l+1}\cdots A_r), việc loại bỏ (A_l) không thể thực hiện được bằng cách nhân với một nghịch đảo vì (A_l) có thể là số ít. Hàng đợi tổng hợp hai ngăn hoàn toàn tránh được điều này. Mỗi ngăn xếp lưu trữ một phần sản phẩm, do đó việc loại bỏ phần tử cũ nhất chỉ yêu cầu xây dựng lại ngăn xếp đối diện khi nó trống. Mỗi ma trận được di chuyển giữa các ngăn xếp nhiều nhất một lần, tạo ra nhiều phép toán đơn hình không đổi được khấu hao cho mỗi lần cập nhật cửa sổ. Đây là ý tưởng tổng hợp cửa sổ trượt tương tự thường được gọi là SWAG. 

Có một cách tối ưu hóa hữu ích khác dành riêng cho vấn đề này. Mọi ma trận tác nhân gốc đều là nhị phân. Khi thêm một tác nhân vào tập hợp ngăn xếp, một toán hạng luôn ở dạng nhị phân, do đó phép nhân có thể được thực hiện dưới dạng tổng của các hàng hoặc cột đã chọn thay vì 64 phép nhân mô-đun thông thường. Mã bên dưới sử dụng tối ưu hóa này. Khi trả lời một truy vấn, chúng tôi thậm chí không cần toàn bộ sản phẩm của hai tập hợp ngăn xếp. Chúng tôi chỉ cần một mục nhập, do đó, việc kết hợp hai tổng hợp chỉ tốn 8 số hạng cộng nhân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(mn\cdot8^2)) | (O(8^2)) | Quá chậm | 
| Cây phân đoạn | (O(m\log n\cdot8^3)) | (O(n\cdot8^2)) | Đúng nhưng đắt không cần thiết | 
| SWAG tối ưu | (O(n\cdot8^3+m\cdot8)) | (O(n\cdot8^2+m)) | Đã chấp nhận | 

Vì 8 là hằng số cố định nên độ phức tạp tối ưu là các phép toán ma trận (O(n+m)) hiệu quả. 

## Hướng dẫn thuật toán 

1. Chuyển đổi mọi số đầu vào 64 bit thành ma trận nhị phân 8 × 8. Chúng tôi lưu trữ mặt nạ tám hàng và mặt nạ tám cột, cùng với 64 mục được làm phẳng. Các mặt nạ cho phép chúng ta nhân một tổng hợp tùy ý với ma trận tác nhân nhị phân mà không thực hiện các phép nhân vô hướng không cần thiết. 
2. Đọc tất cả các truy vấn và sắp xếp chúng theo`(l, r)`. Thứ tự ban đầu được lưu trữ với mọi truy vấn để sau này có thể khôi phục các câu trả lời. Sắp xếp theo (l) là đủ để làm cho (r) không giảm vì họ truy vấn không có cách lồng thích hợp. 
3. Duy trì một cửa sổ trượt chứa chính xác các tác nhân hiện thuộc truy vấn đang được xử lý. Hai điểm cuối của nó là`left`Và`right`. Ban đầu cửa sổ trống. 
4. Để mở rộng cửa sổ sang bên phải, hãy thêm mọi tác nhân mới cho đến khi`right`bằng (r) của truy vấn hiện tại. Ngăn xếp phía sau lưu trữ các ma trận mới được thêm vào. Tổng hợp của nó là tích của tất cả các ma trận trong ngăn xếp đó theo thứ tự thời gian. 
5. Để di chuyển điểm cuối bên trái về phía trước, hãy xóa ma trận khỏi phía trước cửa sổ cho đến khi`left`bằng với truy vấn hiện tại (l). Nếu ngăn xếp phía trước không trống, ma trận cũ nhất có thể được lấy ra một cách đơn giản. Nếu nó trống, di chuyển mọi ma trận từ ngăn xếp sau vào ngăn xếp trước. Đảo ngược thứ tự của chúng làm cho ma trận cũ nhất trở thành phần tử trên cùng của ngăn xếp phía trước. 
6. Trong khi xây dựng lại ngăn xếp phía trước, hãy tính toán lại tổng hợp của nó từ ma trận cũ nhất đến ma trận mới nhất. Nếu ma trận mới là (A) và tổng hợp trước đó là (P), thì tổng hợp mới là (A P), không phải (PA). Thứ tự rất quan trọng vì phép nhân ma trận không có tính giao hoán. 
7. Sau khi cửa sổ biểu thị ([l,r]), tích của nó được chia thành tối đa hai tập hợp. Nếu có cả hai ngăn xếp thì sản phẩm đầy đủ sẽ là`front_product * back_product`. Nếu một ngăn xếp trống thì tổng thể của nó đã là toàn bộ sản phẩm. 
8. Chỉ cần nhập mục được yêu cầu ((s,t)). Nếu hai tập hợp là (F) và (B), hãy tính 

[ 
(FB)[s][t]=\sum_{k=0}^{7}F[s][k]B[k][t]. 
] 

Điều này chỉ cần tám phép nhân vô hướng. 

1. Lưu trữ câu trả lời theo chỉ mục gốc của truy vấn. Sau khi tất cả các truy vấn được sắp xếp đã được xử lý, hãy in câu trả lời theo thứ tự ban đầu. 

### Tại sao nó hoạt động 

Điều bất biến là ngay trước khi trả lời một truy vấn ([l,r]), hai ngăn xếp cùng nhau biểu diễn chính xác các ma trận (A_l,A_{l+1},\ldots,A_r) theo thứ tự đó. Ngăn xếp sau lưu trữ các ma trận theo thứ tự thời gian trong tổng hợp của nó, trong khi ngăn xếp phía trước lưu trữ các ma trận từ cũ nhất đến mới nhất trong tổng hợp của nó. Khi ngăn xếp phía sau được chuyển sang ngăn xếp phía trước, sự đảo ngược vật lý sẽ thay đổi thứ tự ngăn xếp thành thứ tự thời gian và mỗi tập hợp phía trước mới được hình thành dưới dạng (A P), giữ nguyên thứ tự sản phẩm. Do đó tích được biểu thị bằng hai tổng luôn luôn chính xác (A_lA_{l+1}\cdots A_r). Do đó, mục nhập ma trận được yêu cầu sẽ đếm chính xác các tuyến đường từ (s) đến (t). 

Giới hạn khấu hao theo sau vì mỗi tác nhân được thêm một lần và bất cứ khi nào ngăn xếp phía trước được xây dựng lại, mọi tác nhân đã di chuyển sẽ được chuyển một lần trước khi được bật ra. Một tác nhân không thể được chuyển qua lại nhiều lần dưới một cửa sổ trượt đơn điệu. Do đó tổng số lần cập nhật tổng hợp ngăn xếp là tuyến tính theo (n). 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

MOD = 998244353

# Positions of set bits for every 8-bit mask.
BIT_POS = [()]
for mask in range(1, 256):
    cur = []
    x = mask
    while x:
        b = x & -x
        cur.append(b.bit_length() - 1)
        x -= b
    BIT_POS.append(tuple(cur))

def parse_agent(x):
    rows = [0] * 8
    cols = [0] * 8
    flat = [0] * 64

    # Bit 63 is matrix position (0, 0), bit 0 is (7, 7).
    for p in range(64):
        bit = (x >> (63 - p)) & 1
        if bit:
            i = p >> 3
            j = p & 7
            rows[i] |= 1 << j
            cols[j] |= 1 << i
            flat[p] = 1

    return (tuple(rows), tuple(cols), tuple(flat))

def mul_right_binary(a, cols):
    """
    Compute A * B, where A is a general 8x8 matrix and B is binary.
    """
    out = array('I', [0]) * 64

    for i in range(8):
        base = i << 3
        for j in range(8):
            pos = BIT_POS[cols[j]]

            if not pos:
                continue

            if len(pos) == 1:
                value = a[base + pos[0]]
            else:
                value = 0
                for k in pos:
                    value += a[base + k]
                value %= MOD

            out[base + j] = value

    return out

def mul_left_binary(rows, b):
    """
    Compute A * B, where A is binary and B is a general 8x8 matrix.
    """
    out = array('I', [0]) * 64

    for i in range(8):
        pos = BIT_POS[rows[i]]
        base = i << 3

        if not pos:
            continue

        if len(pos) == 1:
            src = pos[0] << 3
            for j in range(8):
                out[base + j] = b[src + j]
        else:
            for j in range(8):
                value = 0
                for k in pos:
                    value += b[(k << 3) + j]
                out[base + j] = value % MOD

    return out

def entry_product(a, b, s, t):
    """
    Return (A * B)[s][t], without constructing A * B.
    """
    base_a = s << 3
    value = 0

    for k in range(8):
        value += a[base_a + k] * b[(k << 3) + t]

    return value % MOD

def solve():
    n, m = map(int, input().split())

    agents = []
    for _ in range(n):
        agents.append(parse_agent(int(input())))

    queries = []
    for idx in range(m):
        l, r, s, t = map(int, input().split())
        queries.append((l - 1, r - 1, s - 1, t - 1, idx))

    # For non-nested intervals, sorting by l makes r nondecreasing.
    queries.sort(key=lambda q: (q[0], q[1]))

    # Each entry is (raw_agent, aggregate_of_stack).
    #
    # Back stack:
    #   top is the newest element.
    #   aggregate is product from oldest to newest.
    #
    # Front stack:
    #   top is the oldest element.
    #   aggregate is product from oldest to newest.
    back = []
    front = []

    left = 0
    right = -1

    answers = [0] * m

    for ql, qr, s, t, idx in queries:
        while right < qr:
            right += 1
            raw = agents[right]

            if back:
                old_agg = back[-1][1]
                agg = mul_right_binary(old_agg, raw[1])
            else:
                agg = raw[2]

            back.append((raw, agg))

        while left < ql:
            if not front:
                # Transfer back -> front.
                while back:
                    raw, _ = back.pop()

                    if front:
                        old_agg = front[-1][1]
                        agg = mul_left_binary(raw[0], old_agg)
                    else:
                        agg = raw[2]

                    front.append((raw, agg))

            front.pop()
            left += 1

        if front:
            f = front[-1][1]

            if back:
                b = back[-1][1]
                answers[idx] = entry_product(f, b, s, t)
            else:
                answers[idx] = f[(s << 3) + t]
        else:
            b = back[-1][1]
            answers[idx] = b[(s << 3) + t]

    sys.stdout.write('\n'.join(map(str, answers)))

if __name__ == "__main__":
    solve()
```các`parse_agent`hàm tuân theo thứ tự bit của câu lệnh một cách chính xác. Bit quan trọng nhất biểu thị vị trí ma trận ((0,0)), do đó vị trí`p`sử dụng bit`63 - p`. Đây là nguồn phổ biến dẫn đến các câu trả lời sai vì việc xử lý bit có trọng số thấp nhất làm phần tử ma trận đầu tiên sẽ đảo ngược toàn bộ quá trình mã hóa. 

các`rows`Và`cols`mặt nạ chứa cấu trúc nhị phân cần thiết cho các quy trình nhân được tối ưu hóa. Vì`A * B`với hệ nhị phân`B`, mỗi mục đầu ra là tổng của các mục được chọn từ một hàng`A`, trong đó các chỉ số được chọn được xác định bởi mặt nạ cột của`B`. Ý tưởng đối xứng được sử dụng cho`A * B`khi`A`là nhị phân. 

Hai ngăn xếp chứa cả tác nhân thô và tổng hợp thuộc tiền tố ngăn xếp đó. Tổng hợp phía sau được cập nhật dưới dạng`old_aggregate * new_agent`. Tổng hợp phía trước được xây dựng lại như`new_agent * old_aggregate`, bởi vì tác nhân được chuyển cũ hơn mọi thứ đã có trong ngăn xếp phía trước. 

Mã cố tình tránh việc xây dựng tích của các tập hợp trước và sau khi trả lời một truy vấn. Chỉ cần một mục nhập ma trận, vì vậy`entry_product`tính trực tiếp tích số chấm tám số hạng tương ứng. 

các`array('I')`type lưu trữ các mục tổng hợp dưới dạng số nguyên không dấu 32 bit thay vì các đối tượng số nguyên Python. Mọi giá trị đều được giảm theo modulo 998244353, vì vậy bốn byte là đủ. Điều này làm giảm đáng kể mức tiêu thụ bộ nhớ khi cửa sổ chứa ma trận gần (10^5). 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn trong số học thông thường. Modulo rõ ràng sau khi tính tổng tối đa tám thuật ngữ sẽ giữ các mục tổng hợp bên dưới mô-đun, trong khi tích số chấm truy vấn cuối cùng cũng được giảm modulo giá trị được yêu cầu. 

## Ví dụ đã hoạt động 

Mẫu chính thức là:```
3 3
9241386504218214000
4692768438333080000
4620710844295152000
1 2 3 4
1 3 1 3
3 3 1 2
```Đầu ra của nó là:```
0
1
1
```Truy vấn đầu tiên sử dụng tác nhân 1 và 2. Tác nhân 1 không có chuyển tiếp đi từ quận 3, do đó số lượng tuyến đường ngay lập tức trở thành 0. Truy vấn thứ hai sử dụng cả ba tác nhân và có lộ trình duy nhất (1\to2\to3). Truy vấn thứ ba chỉ sử dụng tác nhân 3 và có quá trình chuyển đổi duy nhất (1\to2). Đây chính xác là kết quả mẫu chính thức. 

Đối với hành vi cửa sổ trượt, hãy xem xét hai tác nhân tạo thành một chuỗi:```
2 3
4611686018427387904
9007199254740992
1 1 1 2
1 2 1 3
2 2 2 3
```Số đầu tiên chỉ đại diện cho (1\to2) và số thứ hai chỉ đại diện cho (2\to3). 

| Truy vấn | Cửa sổ sau khi cập nhật | Sản phẩm phía trước | Trở lại sản phẩm | Trả lời | 
| --- | --- | --- | --- | --- | 
|`[1,1]`,`1 -> 2`|`[1]`| trống | (A_1) | 1 | 
|`[1,2]`,`1 -> 3`|`[1,2]`| (A_1) | (A_2) | 1 | 
|`[2,2]`,`2 -> 3`|`[2]`| (A_2) | trống | 1 | 

Sau truy vấn đầu tiên, cửa sổ chỉ chứa tác nhân 1. Truy vấn thứ hai mở rộng ranh giới bên phải, do đó tác nhân 2 được đẩy vào ngăn xếp lùi. Mục nhập được yêu cầu của (A_1A_2) là một. Truy vấn thứ ba di chuyển ranh giới bên trái từ 1 lên 2, do đó tác nhân 1 bị xóa khỏi phía trước và tổng hợp còn lại chính xác là (A_2). 

Ví dụ thứ hai minh họa các lượt truy cập lặp lại và phạm vi truy vấn trùng lặp:```
2 3
9223372036854775808
9223372036854775808
1 2 1 1
1 2 1 1
2 2 1 1
```Cả hai tác nhân chỉ có quá trình chuyển đổi (1\to1). 

| Truy vấn | Trái | Đúng | Lộ trình hiện tại | Trả lời | 
| --- | --- | --- | --- | --- | 
| đầu tiên | 1 | 2 | (1\to1\to1) | 1 | 
| thứ hai | 1 | 2 | (1\to1\to1) | 1 | 
| thứ ba | 2 | 2 | (1\to1) | 1 | 

Các truy vấn trùng lặp không gây ra bất kỳ vấn đề đặc biệt nào vì quá trình xử lý được sắp xếp có thể trả lời các cửa sổ giống hệt nhau liên tiếp. Quận lặp lại cũng được giữ nguyên vì sự chuyển đổi ma trận từ một quận sang chính nó là một cạnh hoàn toàn hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\cdot8^3+m\cdot8)) | Mỗi tác nhân được đẩy một lần và được chuyển nhiều nhất một lần, trong khi mỗi truy vấn chỉ cần một tích số chấm 8 số hạng | 
| Không gian | (O(n\cdot8^2+m)) | Tập hợp ngăn xếp chứa ma trận 8 × 8 có kích thước không đổi và tất cả các truy vấn được lưu trữ để sắp xếp | 

Hệ số (8^3=512) được cố định bởi vấn đề, do đó hành vi tiệm cận là tuyến tính về số lượng tác nhân và truy vấn. Tính đơn điệu của cả hai điểm cuối là điều ngăn cản việc chèn và xóa tác nhân nhiều lần. Giới hạn bộ nhớ 256 MB được xử lý khi triển khai Python bằng cách lưu trữ ma trận tổng hợp trong mảng 32 bit nhỏ gọn. 

## Trường hợp thử nghiệm```python
# The following tests assume the solution code above has already been defined.
# The helper temporarily replaces the global input/output streams.

import sys
import io

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_stdout = sys.stdout
    old_input = input

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    input = sys.stdin.readline

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        input = old_input
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Official sample
sample = """\
3 3
9241386504218214000
4692768438333080000
4620710844295152000
1 2 3 4
1 3 1 3
3 3 1 2
"""
assert run(sample) == "0\n1\n1", "official sample"

# Minimum-size input
minimum = """\
1 1
9223372036854775808
1 1 1 1
"""
assert run(minimum) == "1", "single agent, single self-loop"

# Zero matrix and impossible transitions
zero = """\
1 2
0
1 1 1 1
1 1 8 8
"""
assert run(zero) == "0\n0", "zero transition matrix"

# Boundary and off-by-one test.
# Agent 1: 1 -> 2
# Agent 2: 2 -> 3
chain = """\
2 4
4611686018427387904
9007199254740992
1 1 1 2
1 2 1 3
2 2 2 3
1 2 1 2
"""
assert run(chain) == "1\n1\n1\n0", "chain and interval boundaries"

# Equal matrices and repeated visits.
same = """\
3 3
9223372036854775808
9223372036854775808
9223372036854775808
1 3 1 1
1 2 1 1
2 3 2 2
"""
assert run(same) == "1\n1\n0", "equal matrices and repeated district"

# Maximum number of agents, with one query.
# Every agent has no transitions, so every answer is zero.
n = 100000
max_n = str(n) + " 1\n" + ("0\n" * n) + "1 " + str(n) + " 1 1\n"
assert run(max_n) == "0", "maximum n"

# Maximum number of queries, with n = 1.
# Every query asks for the same self-loop.
m = 200000
max_m = "1 " + str(m) + "\n9223372036854775808\n"
max_m += ("1 1 1 1\n" * m)
assert run(max_m) == ("1\n" * m).rstrip("\n"), "maximum m"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`, một`1 -> 1`chuyển tiếp |`1`| Kích thước tối thiểu và khoảng thời gian dài một | 
| Một ma trận không |`0`,`0`| Chuyển tiếp bất khả thi và ranh giới quận`8`| 
| Chuỗi hai đại lý |`1`,`1`,`1`,`0`| Cập nhật điểm cuối bên trái và bên phải, bao gồm cả việc loại bỏ tác nhân đầu tiên | 
| Ba ma trận tự lặp giống hệt nhau |`1`,`1`,`0`| Các chuyến thăm lặp đi lặp lại, ma trận bằng nhau và một quận xuất phát không có chuyển tiếp | 
| (n=100000,m=1) |`0`| Số lượng đại lý tối đa | 
| (n=1,m=200000) | 200000 dòng chứa`1`| Số lượng truy vấn tối đa và các cửa sổ giống hệt nhau lặp lại | 

## Vỏ cạnh 

Trong khoảng thời gian một chiều dài, cửa sổ trượt chứa chính xác một ma trận. Giả sử đầu vào là```
1 1
9223372036854775808
1 1 1 1
```Số nhị phân chỉ có tập bit đầu tiên nên ma trận chứa (A[1][1]=1). Ranh giới bên phải được nâng lên tác nhân 1 và ranh giới bên trái không di chuyển. Do đó, ngăn xếp phía sau chứa một tổng hợp bằng (A_1) và câu trả lời sẽ đọc`(1,1)`nhập trực tiếp. Kết quả là`1`. 

Đối với ma trận chuyển tiếp bằng không,```
1 1
0
1 1 1 1
```trình phân tích cú pháp tạo ra 8 mặt nạ hàng 0 và 8 mặt nạ cột 0. Tổng hợp duy nhất cũng bằng không. Của nó`(1,1)`mục nhập bằng 0, vì vậy câu trả lời là`0`. Không có ma trận nhận dạng nào được đưa ra vì cửa sổ không trống. 

Đối với những lần truy cập nhiều lần, hãy cân nhắc```
2 1
9223372036854775808
9223372036854775808
1 2 1 1
```Cả hai ma trận chỉ chứa (1\to1). Tổng hợp là (A_1A_2), có`(1,1)`mục nhập là (1\cdot1=1). Tuyến đường (1\to1\to1) được tính chính xác một lần. Thuật toán không cho rằng các quận liên tiếp phải khác nhau. 

Trường hợp ranh giới tế nhị nhất là di chuyển điểm cuối bên trái. TRONG```
2 1
4611686018427387904
9007199254740992
2 2 2 3
```cửa sổ mong muốn chỉ là tác nhân 2. Quá trình xử lý trước tiên sẽ mở rộng điểm cuối bên phải thông qua tác nhân 2, do đó cả hai tác nhân đều tạm thời vào cửa sổ. Sau đó`left`chuyển từ 0 lên 1 và tác nhân cũ nhất sẽ bị loại bỏ. Tổng còn lại chính xác là (A_2), cho đáp án`1`. Đây là lý do tại sao mã sử dụng`while left < ql`thay vì chỉ so sánh các điểm cuối một lần. 

Khoảng thời gian truy vấn trùng lặp cũng hợp lệ vì hạn chế cấm ngăn chặn thích hợp, không lặp lại các truy vấn giống hệt nhau. Nếu hai truy vấn liên tiếp đều yêu cầu`[1,2]`, việc sắp xếp để chúng liền kề nhau và truy vấn thứ hai không thực hiện chuyển động cửa sổ nào cả. Nó chỉ đơn giản là đọc lại tổng hợp hai ngăn xếp tương tự. 

Mã hóa nhị phân là một chi tiết có ranh giới nhạy cảm khác. Đối với ma trận chỉ có cạnh là (1\to2), hàng đầu tiên là`01000000`, tương ứng với số nguyên (2^{62}=4611686018427387904). Việc sử dụng của trình phân tích cú pháp`63 - p`là ánh xạ giá trị thập phân đó tới hàng 1, cột 2 thay vì hàng 8, cột 7. Thứ tự bit đảo ngược sẽ khiến mọi thử nghiệm liên quan đến ma trận không đối xứng đều thất bại.
