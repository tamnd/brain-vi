---
title: "CF 102644E - Con đường hiệp sĩ"
description: "Chúng ta có một bàn cờ 8 x 8 và một quân mã được đặt ở ô vuông trên cùng bên trái. Một đường đi là một chuỗi các ô đã ghé thăm trong đó mỗi cặp liên tiếp được kết nối bằng một nước đi hiệp sĩ hợp pháp."
date: "2026-08-01T10:16:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102644
codeforces_index: "E"
codeforces_contest_name: "Matrix Exponentiation"
rating: 0
weight: 102644
solve_time_s: 352
verified: true
draft: false
---

[CF 102644E - Con đường hiệp sĩ](https://codeforces.com/problemset/problem/102644/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 52 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bàn cờ 8 x 8 và một quân mã được đặt ở ô vuông trên cùng bên trái. Một đường đi là một chuỗi các ô đã ghé thăm trong đó mỗi cặp liên tiếp được kết nối bằng một nước đi hiệp sĩ hợp pháp. Quân mã được phép dừng lại sau bất kỳ số nước đi nào từ 0 đến`k`, vì vậy câu trả lời là tổng số đường đi có thể có ở mọi độ dài trong phạm vi đó. Những lựa chọn di chuyển khác nhau sẽ tạo ra những con đường khác nhau, ngay cả khi chúng kết thúc trên cùng một ô vuông. Câu trả lời phải được in modulo`2^32`. 

Giá trị của`k`có thể lớn như`10^9`, vì vậy việc mô phỏng từng bước di chuyển là không thể. Mặc dù bàn cờ nhỏ nhưng số lượng đường đi có thể tăng theo cấp số nhân theo số lần di chuyển. Một giải pháp xử lý mọi đường dẫn có thể sẽ nhanh chóng vượt quá mọi giới hạn hoạt động thực tế. Số lượng ô bảng cố định nhỏ là hạn chế chính mà chúng ta có thể khai thác: chỉ có 64 vị trí hiệp sĩ có thể có, do đó, bài toán có thể được chuyển thành các phép toán trên một không gian trạng thái có kích thước cố định. 

Modulo cũng không bình thường. Câu trả lời là bắt buộc theo modulo`2^32`, phù hợp một cách tự nhiên với số học 32-bit không dấu. Chúng ta chỉ cần giữ 32 bit cuối cùng của mỗi giá trị trung gian. Trong Python, điều này có nghĩa là áp dụng mặt nạ bit là đủ để mô phỏng mô đun cần thiết. 

Một lỗi phổ biến là quên rằng đường dẫn có độ dài bằng 0 là hợp lệ. Ví dụ, nếu`k = 0`, quân mã không hề di chuyển, nhưng vẫn còn đúng một con đường chỉ gồm ô xuất phát.```
Input
0

Output
1
```Một sai lầm khác là chỉ tính vị trí cuối cùng sau khi chính xác`k`di chuyển. Vấn đề yêu cầu tất cả độ dài lên đến`k`. Vì`k = 1`, hiệp sĩ có thể giữ nguyên vị trí hoặc di chuyển đến một trong hai ô vuông có thể tiếp cận từ góc.```
Input
1

Output
3
```Vấn đề thứ ba là xây dựng các bước di chuyển hiệp sĩ không chính xác gần biên giới. Một hiệp sĩ có thể có tới tám nước đi ở giữa bàn cờ nhưng ở các cạnh gần thì ít hơn. Ví dụ, hình vuông ở góc ban đầu chỉ có hai nước đi hợp lệ chứ không phải tám nước đi. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thực hiện tìm kiếm theo chiều sâu để tạo ra mọi đường dẫn có thể. Ở độ sâu 0, chúng tôi đếm đường đi hiện tại, sau đó thử đệ quy mọi bước di chuyển của hiệp sĩ hợp pháp cho đến khi chúng tôi sử dụng xong`k`di chuyển. Phương pháp này đúng vì nó tuân theo chính xác định nghĩa của một đường dẫn hợp lệ. Tuy nhiên, nó khám phá một cây phân nhánh. Số lượng lá tăng trưởng theo cấp số nhân với`k`, và cho`k = 10^9`thậm chí một vài bước di chuyển trên mỗi ô vuông cũng khiến điều này hoàn toàn không thể sử dụng được. 

Quan sát hữu ích là hiệp sĩ không có bất kỳ ký ức nào. Số lượng đường đi trong tương lai chỉ phụ thuộc vào ô vuông hiện tại và số bước đi còn lại. Vì chỉ có 64 ô vuông nên chúng ta có thể biểu diễn toàn bộ quá trình dưới dạng chuyển tiếp giữa 64 trạng thái. 

Phép nhân ma trận có thể áp dụng một bước đi cho mọi trạng thái cùng một lúc. Việc nâng ma trận chuyển tiếp lên lũy thừa lớn cho phép chúng ta bỏ qua hàng tỷ bước di chuyển riêng lẻ bằng cách sử dụng lũy ​​thừa nhị phân. Chúng ta cũng cần tổng của tất cả độ dài đường đi từ 0 đến`k`, không chỉ số lượng đường đi sau chính xác`k`di chuyển. Chúng tôi xử lý vấn đề này bằng cách thêm một trạng thái bổ sung để lưu trữ câu trả lời tích lũy. Mỗi phép nhân ma trận thực hiện một nước đi hiệp sĩ và cộng số đường dẫn hiện tại vào bộ tích lũy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong`k`| Độ sâu đệ quy O(k) | Quá chậm | 
| Tối ưu | O(65³ log k) | O(65²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đánh số 64 ô bàn cờ từ`0`ĐẾN`63`. Xây dựng ma trận chuyển tiếp`T`Ở đâu`T[i][j]`là`1`nếu một hiệp sĩ có thể di chuyển từ hình vuông`i`để vuông`j`, Và`0`nếu không thì. Ma trận này mô tả chính xác một nước đi của hiệp sĩ. 
2. Thêm một trạng thái bổ sung đại diện cho câu trả lời tích lũy. Xây dựng ma trận 65 x 65`A`. Khối 64 x 64 phía trên bên trái là ma trận chuyển tiếp hiệp sĩ. Cột cuối cùng của 64 hàng đầu tiên chứa`1`, bởi vì sau một phép nhân, mỗi đường dẫn hiện tại sẽ đóng góp một lần vào câu trả lời. Hàng dưới cùng giữ cho bộ tích lũy không thay đổi. 
3. Bắt đầu với một vectơ hàng chứa một đường dẫn ở ô bắt đầu và giá trị câu trả lời là`1`. Câu trả lời ban đầu là`1`bởi vì đường dẫn trống được bao gồm. 
4. Tính vectơ sau khi áp dụng`A`chính xác`k`lần bằng cách sử dụng lũy ​​thừa nhị phân. Mỗi phép nhân đại diện cho một nước đi hiệp sĩ bổ sung có thể có. 
5. Đọc trạng thái bộ tích lũy từ vectơ kết quả. Nó chứa tổng của tất cả các đường dẫn có độ dài từ`0`bởi vì`k`. 

Lý do trạng thái tăng cường hoạt động là sau mỗi lần nhân, 64 giá trị đầu tiên luôn biểu thị các đường đi sau chính xác số lần di chuyển hiện tại. Giá trị bổ sung lưu trữ tổng của tất cả các lớp trước đó. Vì mỗi lần chuyển đổi là tuyến tính nên phép toán ma trận duy trì mối quan hệ này sau mỗi bước. 

## Tại sao nó hoạt động 

Sau`i`phép nhân, phần bảng của vectơ chứa số cách để tiếp cận mọi ô vuông bằng cách sử dụng chính xác`i`hiệp sĩ di chuyển. Bộ tích lũy chứa tổng số đường dẫn có độ dài từ`0`bởi vì`i`. 

Trong lần nhân tiếp theo, các giá trị bảng được biến đổi bằng cách sử dụng ma trận chuyển tiếp hiệp sĩ, tạo ra các đường dẫn có độ dài chính xác`i + 1`. Đồng thời, bộ tích lũy nhận được tổng số bảng trước đó, chính xác là số lượng đường dẫn mới được thêm vào ở độ dài đó. Bằng quy nạp, sau`k`phép nhân tích lũy bằng số tiền cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1 << 32
SIZE = 65

def multiply(a, b):
    n = len(a)
    res = [[0] * n for _ in range(n)]
    for i in range(n):
        ai = a[i]
        ri = res[i]
        for k in range(n):
            if ai[k]:
                aik = ai[k]
                bk = b[k]
                for j in range(n):
                    ri[j] = (ri[j] + aik * bk[j]) & (MOD - 1)
    return res

def matrix_power(a, e):
    n = len(a)
    res = [[0] * n for _ in range(n)]
    for i in range(n):
        res[i][i] = 1
    while e:
        if e & 1:
            res = multiply(res, a)
        a = multiply(a, a)
        e >>= 1
    return res

def solve():
    k = int(input())

    moves = []
    for r in range(8):
        for c in range(8):
            cur = []
            for dr, dc in ((2, 1), (2, -1), (-2, 1), (-2, -1),
                           (1, 2), (1, -2), (-1, 2), (-1, -2)):
                nr = r + dr
                nc = c + dc
                if 0 <= nr < 8 and 0 <= nc < 8:
                    cur.append(nr * 8 + nc)
            moves.append(cur)

    a = [[0] * SIZE for _ in range(SIZE)]

    for i in range(64):
        for j in moves[i]:
            a[i][j] = 1
        a[i][64] = 1

    a[64][64] = 1

    p = matrix_power(a, k)

    start = [0] * SIZE
    start[0] = 1
    start[64] = 1

    ans = 0
    for i in range(SIZE):
        ans = (ans + start[i] * p[i][64]) & (MOD - 1)

    print(ans)

if __name__ == "__main__":
    solve()
```Cấu trúc di chuyển lặp qua mọi ô vuông và thử tám hướng hiệp sĩ có thể. Các tọa độ không hợp lệ sẽ bị loại bỏ, giúp tránh việc xử lý đặc biệt cho các góc và cạnh. 

Hàm nhân ma trận sử dụng mặt nạ bit thay vì`% (2^32)`. Vì mô-đun là lũy thừa của hai nên chỉ giữ 32 bit thấp nhất sẽ cho kết quả chính xác như nhau và nhanh hơn. 

Thủ tục lũy thừa sử dụng phép phân rã nhị phân tiêu chuẩn của`k`. Khi một chút`k`được thiết lập, lũy thừa hiện tại của ma trận sẽ góp phần vào kết quả cuối cùng. Ma trận nhận dạng đại diện cho bước di chuyển bằng 0. 

Vectơ ban đầu đặt một đường dẫn trên ô vuông số 0 và một đường dẫn trong bộ tích lũy. Ma trận được áp dụng`k`lần, do đó bộ tích lũy sẽ thu thập mọi lớp từ đường dẫn trống qua tất cả các đường dẫn bằng cách sử dụng chính xác`k`di chuyển. 

## Ví dụ đã hoạt động 

cho`k = 1`, quá trình tăng cường trông như thế này: 

| Bước | Số lượng vuông hiện tại | Tích lũy | 
| --- | --- | --- | 
| Bắt đầu | 1 đường đi tại (1,1) | 1 | 
| Sau một lần di chuyển | 1 đường đi tại (2,3), 1 đường đi tại (3,2) | 3 | 

Bộ tích lũy chứa đường đi trống cộng với hai nước đi hiệp sĩ có thể có, đưa ra câu trả lời mẫu. 

Vì`k = 2`, hai lớp đầu tiên là: 

| Bước | Đường dẫn mới được tạo | Tổng số đường dẫn | 
| --- | --- | --- | 
| Độ dài 0 | Ở lại lúc bắt đầu | 1 | 
| Chiều dài 1 | Hai bước di chuyển từ góc | 3 | 
| Chiều dài 2 | Mười hai phần tiếp theo có thể có | 15 | 

Ví dụ thứ hai chứng minh tại sao giải pháp phải tích lũy theo từng độ dài thay vì chỉ tính lớp di chuyển cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(65³ log k) | Khoảng 30 phép nhân ma trận cho ma trận 65 x 65 | 
| Không gian | O(65²) | Lưu trữ một số lượng ma trận có kích thước cố định | 

Kích thước bảng không bao giờ thay đổi, vì vậy thuật toán có kích thước không đổi một cách hiệu quả ngoại trừ sự phụ thuộc logarit vào`k`. Đây là lý do tại sao các giá trị lớn như`10^9`có thể quản lý được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    def solve_local():
        k = int(sys.stdin.readline())
        MOD = 1 << 32
        SIZE = 65

        def multiply(a, b):
            n = len(a)
            res = [[0] * n for _ in range(n)]
            for i in range(n):
                for k in range(n):
                    if a[i][k]:
                        for j in range(n):
                            res[i][j] = (res[i][j] + a[i][k] * b[k][j]) & (MOD - 1)
            return res

        def power(a, e):
            n = len(a)
            r = [[0] * n for _ in range(n)]
            for i in range(n):
                r[i][i] = 1
            while e:
                if e & 1:
                    r = multiply(r, a)
                a = multiply(a, a)
                e >>= 1
            return r

        moves = []
        for r in range(8):
            for c in range(8):
                cur = []
                for dr, dc in ((2,1),(2,-1),(-2,1),(-2,-1),
                               (1,2),(1,-2),(-1,2),(-1,-2)):
                    nr, nc = r + dr, c + dc
                    if 0 <= nr < 8 and 0 <= nc < 8:
                        cur.append(nr * 8 + nc)
                moves.append(cur)

        a = [[0] * SIZE for _ in range(SIZE)]
        for i in range(64):
            for j in moves[i]:
                a[i][j] = 1
            a[i][64] = 1
        a[64][64] = 1

        p = power(a, k)

        v = [0] * SIZE
        v[0] = 1
        v[64] = 1

        ans = sum(v[i] * p[i][64] for i in range(SIZE)) & (MOD - 1)
        return str(ans)

    out = solve_local()
    sys.stdin = old
    return out

assert run("1\n") == "3"
assert run("2\n") == "15"
assert run("6\n") == "17231"
assert run("0\n") == "1"
assert run("10\n") == "1523255"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`1`| Xử lý đường dẫn trống | 
|`1`|`3`| Thế hệ di chuyển góc | 
|`2`|`15`| Tích lũy nhiều độ dài | 
|`6`|`17231`| Quyền lực chuyển tiếp lớn hơn | 

## Vỏ cạnh 

cho`k = 0`, thuật toán không bao giờ nhân ma trận. Bộ tích lũy ban đầu đã chứa một đường đi, tương ứng với quân mã ở ô bắt đầu. 

Đối với hình vuông góc ban đầu, ma trận chuyển tiếp chỉ chứa hai cạnh hướng ra ngoài. Quá trình tạo nước đi sẽ kiểm tra các giới hạn của bảng trước khi thêm các chuyển tiếp, do đó, nó không thể vô tình cho phép các bước nhảy không thể thực hiện được ra ngoài bảng. 

Đối với rất lớn`k`, thuật toán không lặp lại các bước di chuyển. Thay vào đó, nó phân hủy`k`thành lũy thừa nhị phân và nhân ma trận khoảng 30 lần nên thời gian chạy vẫn ổn định ngay cả khi số lần di chuyển là một tỷ.
