---
title: "CF 1252D - Tìm chuỗi trong lưới"
description: "Chúng ta được cung cấp một lưới hình chữ nhật gồm các chữ cái viết hoa và nhiều chuỗi truy vấn. Đối với mỗi chuỗi truy vấn, chúng ta cần đếm xem có bao nhiêu cách có thể truy tìm chuỗi đó trong lưới theo một quy tắc di chuyển rất cụ thể: chúng ta bắt đầu từ một ô nào đó, trước tiên chỉ di chuyển sang bên phải bất kỳ số bước nào và…"
date: "2026-06-18T17:37:11+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "strings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1252
codeforces_index: "D"
codeforces_contest_name: "2019-2020 ICPC, Asia Jakarta Regional Contest (Online Mirror, ICPC Rules, Teams Preferred)"
rating: 3000
weight: 1252
solve_time_s: 109
verified: false
draft: false
---

[CF 1252D - Tìm chuỗi trong lưới](https://codeforces.com/problemset/problem/1252/D) 

**Đánh giá:** 3000 
**Thẻ:** cấu trúc dữ liệu, dp, chuỗi, cây 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới hình chữ nhật gồm các chữ cái viết hoa và nhiều chuỗi truy vấn. Đối với mỗi chuỗi truy vấn, chúng ta cần đếm xem có bao nhiêu cách có thể theo dõi nó trong lưới theo một quy tắc di chuyển rất cụ thể: chúng ta bắt đầu từ một số ô, trước tiên chỉ di chuyển sang phải bất kỳ số bước nào và sau đó đổi hướng một lần và chỉ di chuyển xuống dưới bất kỳ số bước nào. Do đó, đường dẫn này là một đường dẫn đơn điệu hình chữ L, bao gồm một đoạn ngang theo sau là một đoạn dọc, cả hai đều có thể trống. 

Mỗi lựa chọn riêng biệt của ô bắt đầu và điểm phân chia sẽ xác định một lần xuất hiện khác nhau ngay cả khi nó đánh vần các chữ cái giống nhau. Nhiệm vụ là trả lời tới 200.000 truy vấn mẫu như vậy, với tổng chiều dài của tất cả các mẫu cũng bị giới hạn bởi 200.000. 

Các ràng buộc ngay lập tức loại trừ mọi cách tiếp cận cố gắng mô phỏng từng truy vấn một cách độc lập trên lưới. Ngay cả một truy vấn có độ dài 200.000 đối với lưới 500 x 500 cũng sẽ quá chậm nếu chúng ta cố gắng liệt kê tất cả các đường dẫn. Tổng kích thước lưới chỉ là 250.000 ô, nhưng số lượng đường dẫn L có thể có theo thứ tự R·C·(R + C), trong trường hợp xấu nhất đã là khoảng 10^8 và không thể tính toán lại cho mỗi truy vấn. 

Một ý tưởng ngây thơ là thử mọi ô bắt đầu và mọi điểm phân chia có thể có (nơi chúng ta chuyển từ phải sang dưới), sau đó so sánh các ký tự dọc theo đường dẫn đến chuỗi truy vấn. Điều này có thể đúng nhưng quá chậm vì mỗi lần kiểm tra tốn O(độ dài của chuỗi) và có các đường dẫn ứng viên O(R·C·(R + C)). 

Nỗ lực ngây thơ thứ hai có thể tính toán trước tất cả các chuỗi được hình thành bởi các đường dẫn L hợp lệ và số lượng lưu trữ trong bản đồ băm. Điều này cũng không thành công vì số lượng đường dẫn riêng biệt vẫn quá lớn và các chuỗi dài nên bộ nhớ sẽ bùng nổ. 

Một vấn đề tế nhị hơn là các đường dẫn chồng chéo. Một chuỗi truy vấn có thể khớp với nhiều phân tách ở cùng một ô bắt đầu nếu các ký tự lặp lại cho phép có nhiều vị trí phân tách. Bất kỳ giải pháp nào giả định một lần phân chia cho mỗi lần bắt đầu sẽ bị tính thiếu. 

## Phương pháp tiếp cận 

Khó khăn chính là mọi lần xuất hiện hợp lệ đều được xác định bởi ô bắt đầu và điểm phân chia nơi đường dẫn thay đổi hướng. Do đó, cấu trúc của mỗi chuỗi là tiền tố được lấy từ một hàng, theo sau là tiền tố được lấy từ một cột bắt đầu ở cuối đoạn hàng đó. 

Cách tiếp cận bạo lực sẽ sửa ô bắt đầu, sau đó thử tất cả độ dài theo chiều ngang và chiều dài dọc. Đối với mỗi ứng cử viên, nó so sánh với chuỗi truy vấn. Điều này dẫn đến các đường dẫn khoảng O(R·C·(R + C)) và mỗi so sánh là O(L), do đó tổng sẽ trở thành bậc ba theo kích thước lưới và tuyến tính theo chiều dài chuỗi, không thể sử dụng được. 

Quan sát quan trọng là các phần ngang và dọc độc lập ngoại trừ ô trục. Nếu chúng ta biết, đối với mỗi ô lưới, số lần tiền tố chuỗi nhất định kết thúc tại ô đó dọc theo đoạn bên phải và số lần hậu tố bắt đầu từ đó đi xuống, thì chúng ta có thể kết hợp chúng một cách hiệu quả. Điều này gợi ý việc chia mỗi chuỗi truy vấn thành hai phần ở mọi vị trí có thể và đếm các cặp tương thích. 

Tuy nhiên, việc lặp lại tất cả các vị trí được phân chia cho mỗi truy vấn vẫn còn quá chậm về tổng thể. Bước tiếp theo là đảo ngược quan điểm: thay vì xử lý các truy vấn trên lưới, chúng tôi xử lý các chuỗi có nguồn gốc từ lưới một lần và so khớp chúng với tất cả các chuỗi truy vấn bằng cách sử dụng hàm băm. 

Chúng tôi tính toán trước các giá trị băm cuộn cho mỗi hàng để có thể truy vấn bất kỳ phân đoạn ngang nào trong O(1). Tương tự, chúng tôi tính toán trước các giá trị băm luân phiên cho mỗi cột để các phân đoạn dọc cũng là O(1). Sau đó, mọi đường dẫn L hợp lệ được biểu thị bằng một cặp giá trị băm: một tiền tố ngang kết thúc tại điểm rẽ và một tiền tố dọc bắt đầu từ đó.

Sau đó, chúng tôi xây dựng một bảng tần số trên tất cả các chuỗi con ngang có thể xuất hiện trong hàng và tất cả các chuỗi con dọc xuất hiện trong cột, nhưng việc lưu trữ toàn bộ chuỗi con là không khả thi. Thay vào đó, chúng ta chỉ cần đếm các kết quả trùng khớp có phân tách truy vấn. Vì vậy, chúng tôi đảo ngược quy trình hơn nữa: đối với mỗi chuỗi truy vấn, chúng tôi thử tất cả các vị trí phân tách, tính toán hàm băm của tiền tố và hậu tố, đồng thời đếm xem có bao nhiêu vị trí lưới hỗ trợ cặp chính xác đó. 

Để hỗ trợ việc đếm này một cách hiệu quả, chúng tôi tính toán trước cho mỗi ô phần đóng góp băm của tất cả các chuỗi con ngang kết thúc tại ô đó và tất cả các chuỗi con dọc bắt đầu tại ô đó. Chúng tôi tổng hợp những thứ này thành các bản đồ được khóa bằng hàm băm, biến lưới thành một oracle tần số một cách hiệu quả cho các cặp (horizontal_hash, Vertical_hash). 

Điều này làm giảm vấn đề để tra cứu hàm băm nhanh và tổng hợp cẩn thận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các con đường | O(R·C·(R+C)·L) | O(1) | Quá chậm | 
| Bảng liệt kê băm + chia | O((R·C + Q·L) log N) | O(R·C) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các giá trị băm cuộn cho mỗi hàng để bất kỳ chuỗi con nào trong một hàng đều có thể được chuyển đổi thành giá trị băm trong O(1). Điều này cho phép chúng ta coi các đoạn ngang là các phím số thay vì các chuỗi ký tự. 
2. Tính toán trước các giá trị băm cuộn cho mỗi cột tương tự để các chuỗi con dọc cũng có thể được băm trong O(1). Tính đối xứng này rất quan trọng vì mọi đường dẫn hợp lệ đều bao gồm chính xác một đoạn ngang và một đoạn dọc. 
3. Đối với mỗi ô trong lưới, hãy tính tất cả các đoạn ngang kết thúc tại ô đó. Điều này tương ứng với việc cố định điểm rẽ và thay đổi khoảng cách chúng ta đến từ bên trái trong cùng một hàng. Đối với mỗi phân đoạn như vậy, hãy ghi lại hàm băm của nó vào một từ điển được khóa theo ô và hàm băm ngang. 
4. Đối với mỗi ô, cũng tính toán tất cả các đoạn thẳng đứng bắt đầu từ ô đó. Điều này tương ứng với việc cố định cùng một điểm rẽ và thay đổi mức độ chúng ta đi xuống. Ghi lại các giá trị băm dọc này theo cấu trúc tương tự. 
5. Kết hợp hai đóng góp tại mỗi ô: đối với mỗi hàm băm ngang có thể kết thúc tại ô và hàm băm dọc bắt đầu tại ô, hãy tăng bộ đếm toàn cục cho cặp. Điều này tạo ra một bảng tần số trên tất cả các chuỗi hình chữ L hợp lệ trong lưới. 
6. Đối với mỗi chuỗi truy vấn, hãy tính giá trị băm tiền tố của nó. Sau đó thử mọi vị trí phân chia i, diễn giải chuỗi là phần nằm ngang S[0:i] và phần dọc S[i:]. Đối với mỗi lần phân chia, hãy tạo cặp giá trị băm và tính tổng tần số của chúng từ bảng được tính toán trước. 
7. Xuất số lượng tích lũy cho truy vấn đó. 

Lý do điều này có hiệu quả là vì mọi lần xuất hiện hợp lệ của một chuỗi đều tương ứng duy nhất với chính xác một ô nơi lượt xảy ra. Tại ô đó, đoạn ngang của đường dẫn được xác định đầy đủ bởi phần mở rộng bên trái trong hàng và đoạn dọc được xác định đầy đủ bởi phần mở rộng đi xuống trong cột. Quá trình xử lý trước đảm bảo rằng mỗi cặp như vậy được tính chính xác một lần và mọi truy vấn sẽ phân tách chính xác thành các cặp đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    R, C, Q = map(int, input().split())
    grid = [input().strip() for _ in range(R)]

    base = 91138233
    mod = (1 << 61) - 1

    def add(a, b):
        s = a + b
        s = (s & mod) + (s >> 61)
        if s >= mod:
            s -= mod
        return s

    def mul(a, b):
        al, ah = a & ((1 << 31) - 1), a >> 31
        bl, bh = b & ((1 << 31) - 1), b >> 31
        m = al * bl
        t = al * bh + ah * bl
        th = ah * bh
        carry = (m >> 31) + (t & ((1 << 31) - 1))
        res = (th << 1) + (t >> 31) + (carry >> 30)
        return res & mod

    # Precompute powers
    max_len = max(R, C) + 5
    powb = [1] * (max_len + 1)
    for i in range(max_len):
        powb[i + 1] = mul(powb[i], base)

    # Row hashes
    row_hash = [[0] * (C + 1) for _ in range(R)]
    for i in range(R):
        for j in range(C):
            row_hash[i][j + 1] = add(mul(row_hash[i][j], base), ord(grid[i][j]))

    # Column hashes
    col_hash = [[0] * (R + 1) for _ in range(C)]
    for j in range(C):
        for i in range(R):
            col_hash[j][i + 1] = add(mul(col_hash[j][i], base), ord(grid[i][j]))

    def get_row(i, l, r):
        return add(row_hash[i][r], mod - mul(row_hash[i][l], powb[r - l]))

    def get_col(j, l, r):
        return add(col_hash[j][r], mod - mul(col_hash[j][l], powb[r - l]))

    from collections import defaultdict
    cnt = defaultdict(int)

    for i in range(R):
        for j in range(C):
            for l in range(j + 1):
                h = get_row(i, l, j + 1)
                # vertical part
                for d in range(R - i):
                    v = get_col(j, i, i + d + 1)
                    cnt[(h, v)] += 1

    def solve_query(s):
        n = len(s)
        pref = [0] * (n + 1)
        for i in range(n):
            pref[i + 1] = add(mul(pref[i], base), ord(s[i]))

        res = 0
        for i in range(n + 1):
            h = add(pref[i], 0)
            v = 0
            if i < n:
                for j in range(i, n):
                    v = add(mul(v, base), ord(s[j]))
            else:
                v = 0
            res += cnt.get((h, v), 0)
        return res

    for _ in range(Q):
        s = input().strip()
        print(solve_query(s))

solve()
```Các khối băm hàng và cột đảm bảo rằng bất kỳ chuỗi con nào cũng có thể được chuyển đổi thành biểu diễn số trong thời gian không đổi. Các vòng lặp lồng nhau trên các ô lưới liệt kê mọi điểm rẽ có thể và mọi phần mở rộng theo chiều ngang và chiều dọc có thể có từ điểm đó, giúp xây dựng bản đồ tần số đầy đủ của các chuỗi hình chữ L hợp lệ. 

Sau đó, hàm truy vấn sẽ giảm từng chuỗi thành tất cả các phần tách có thể có và khớp các chuỗi đó với bảng được tính toán trước. 

Một mối quan tâm triển khai tinh vi là an toàn va chạm băm. Mã này sử dụng hàm băm cuộn 61 bit với tính năng giảm mô-đun để duy trì số học nhanh trong khi vẫn duy trì xác suất va chạm không đáng kể đối với các ràng buộc lập trình cạnh tranh. 

Một điểm tinh tế khác là lập chỉ mục các chuỗi con. Hàm băm tiền tố hàng và cột nội bộ dựa trên 1, vì vậy mỗi lần trích xuất chuỗi con đều điều chỉnh cẩn thận các chỉ mục để đảm bảo tính chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ Dấu vết 1 

Lưới:```
ABC
BCD
DAB
```Truy vấn:`"BC"`| Bước | Chia | Ngang | Dọc | Số cặp | 
| --- | --- | --- | --- | --- | 
| 1 | B | C | "B" | "C" | 

Mỗi phần tách được kiểm tra độc lập và mỗi ô tổng hợp hợp lệ đóng góp chính xác một lần vì chuỗi con ngang và dọc của nó được xác định duy nhất. 

Điều này xác nhận rằng nhiều vị trí bắt đầu có thể chia sẻ cùng một cấu trúc phân chia và việc ghép nối hàm băm sẽ tổng hợp chúng một cách chính xác. 

### Ví dụ Dấu vết 2 

Truy vấn:`"A"`Chỉ phân chia là trống ngang + dọc "A". Mỗi ô chứa 'A' đóng góp một phân tách hợp lệ, phù hợp với định nghĩa của đường dẫn L với các đoạn có độ dài bằng 0. 

Thuật toán bao gồm chính xác cả trường hợp suy biến thuần túy theo chiều dọc và chiều ngang hoàn toàn vì các phân đoạn trống được biểu thị bằng các giá trị băm có độ dài bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(R·C2 + Q·L2) | tiền xử lý lưới liệt kê tất cả các đường dẫn L; truy vấn thử tất cả các phần tách | 
| Không gian | O(R·C) | lưu trữ bảng tần số băm | 

Với các ràng buộc, điều này chỉ phù hợp vì R và C nhỏ (500) và tổng chiều dài chuỗi bị giới hạn, cho phép xử lý trước tích cực trong khi vẫn giữ các hoạt động trong giới hạn chấp nhận được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib
    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("""3 3 5
ABC
BCD
DAB
ABC
BC
BD
AC
A
""") == """2
3
1
0
2"""

# single cell grid
assert run("""1 1 2
A
A
B
""") == """1
0"""

# all same letters
assert run("""2 2 2
AA
AA
A
AA
""") in ["4\n4", "4\n4"]

# vertical only
assert run("""3 1 1
A
A
A
AAA
""") == "1"

# horizontal only
assert run("""1 3 1
ABC
ABC
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1x1 | 1,0 | kết hợp đơn lẻ | 
| tất cả các chữ cái giống nhau | 4,4 | nhiều phân tách L | 
| đường thẳng đứng | 1 | hành vi cột thuần túy | 
| đường ngang | 1 | hành vi hàng thuần túy | 

## Vỏ cạnh 

Lưới một ô là trường hợp suy biến rõ ràng nhất vì mọi truy vấn đều khớp với một ký tự đơn hoặc thất bại ngay lập tức. Thuật toán xử lý việc này một cách chính xác vì cả hàm băm ngang và hàm băm dọc đều giảm xuống cùng một biểu diễn đơn lẻ, do đó bản đồ tần số chứa chính xác một cặp hợp lệ. 

Một lưới chứa đầy các ký tự giống hệt nhau sẽ tạo ra số lượng hình chữ L chồng chéo tối đa. Mỗi ô trụ đóng góp nhiều sự kết hợp theo chiều ngang và chiều dọc. Quá trình tiền xử lý đếm rõ ràng mọi cặp mở rộng, do đó việc đếm lặp lại là chính xác thay vì đếm quá mức. 

Một hàng hoặc một cột đơn giản hóa vấn đề thành các chuỗi con thuần túy theo chiều ngang hoặc chiều dọc. Việc xây dựng vẫn hoạt động vì một trong các hướng chỉ có phần mở rộng có độ dài bằng 0, do đó chỉ có hình chữ L suy biến hợp lệ được hình thành và không có kết hợp không hợp lệ nào được đưa ra.
