---
title: "CF 105687D - Bà mối"
description: "Chúng ta được cung cấp một chuỗi chữ thường và nhiều truy vấn chuỗi con. Đối với mỗi khoảng truy vấn $[l, r]$, chúng tôi chỉ xem xét chuỗi con đó. Hai chữ cái giống nhau có thể tạo thành một kết quả trùng khớp và mỗi ký tự có thể thuộc nhiều nhất một kết quả khớp."
date: "2026-06-25T06:12:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105687
codeforces_index: "D"
codeforces_contest_name: "AlgoChief Sprint Round 2"
rating: 0
weight: 105687
solve_time_s: 50
verified: true
draft: false
---

[CF 105687D - Người mai mối](https://codeforces.com/problemset/problem/105687/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chữ thường và nhiều truy vấn chuỗi con. Đối với mỗi khoảng truy vấn$[l, r]$, chúng ta chỉ nhìn vào chuỗi con đó. 

Hai chữ cái giống nhau có thể tạo thành một kết quả trùng khớp và mỗi ký tự có thể thuộc nhiều nhất một kết quả khớp. Mục tiêu là đạt được số lượng kết quả phù hợp lớn nhất có thể trong chuỗi con đã chọn. Trước khi tạo kết quả khớp, chúng ta có thể thay đổi ký tự, thay thế bất kỳ vị trí nào bằng bất kỳ chữ cái viết thường nào mà chúng ta muốn. 

Đối với mỗi truy vấn, chúng ta phải tìm số lượng thay đổi ký tự tối thiểu cần thiết để chuỗi con có thể đạt được số lượng kết quả khớp tối đa có thể. 

Nếu một chuỗi con có độ dài$m$, số cặp rời nhau lớn nhất có thể là$\lfloor m/2 \rfloor$. Mọi sự sắp xếp cuối cùng hợp lệ đều phải để lại nhiều nhất một ký tự chưa ghép đôi. 

Kích thước đầu vào lớn. Trên tất cả các trường hợp thử nghiệm, tổng giá trị của$n + q$nhiều nhất là$2 \cdot 10^5$. Một giải pháp quét mọi ký tự của mọi chuỗi con được truy vấn sẽ yêu cầu$O(nq)$hoạt động trong trường hợp xấu nhất, quá chậm. Chúng ta cần một cái gì đó gần gũi$O(1)$hoặc$O(\text{alphabet size})$mỗi truy vấn. 

Một số tình huống rất dễ xử lý sai. 

Coi như:```
substring = "abcdef"
```Mỗi tần số là 1. Độ dài là 6 nên chúng ta cần 3 cặp. Chúng ta phải thực hiện đúng 3 thay đổi, ví dụ:```
abcdef -> aabbcc
```Một ý tưởng tham lam như "thay đổi các ký tự cho đến khi tất cả các chữ cái trở nên bằng nhau" sẽ sử dụng nhiều thao tác hơn mức cần thiết. 

Coi như:```
substring = "aaabbb"
```Các tần số là$3$Và$3$. Chúng tôi đã có hai cặp:```
aaa -> one pair + one leftover
bbb -> one pair + one leftover
```Chỉ còn thiếu một cặp nữa nên đáp án là 1. 

Hãy xem xét:```
substring = "abcde"
```Chiều dài là số lẻ. Một nhân vật được phép giữ nguyên chưa từng có. Câu trả lời là 2, không phải 3:```
abcde -> aabcd
```Đây là nơi nhiều triển khai mắc lỗi khi cố gắng ghép nối mọi ký tự. 

## Phương pháp tiếp cận 

Chế độ xem brute-force là kiểm tra chuỗi con được truy vấn, đếm tần số và bằng cách nào đó tìm kiếm số lượng chỉnh sửa tối thiểu cần thiết để đạt được cấu hình hoàn toàn có thể ghép nối. Điều đó nhanh chóng trở nên không thực tế vì có thể có tới$10^5$truy vấn cho mỗi trường hợp thử nghiệm. Ngay cả việc tính toán lại tần số từ đầu cho mỗi truy vấn cũng sẽ tốn kém$O(nq)$hoạt động trong trường hợp xấu nhất. 

Quan sát quan trọng đến từ việc xem xét tính chẵn lẻ. 

Giả sử một chữ cái xuất hiện$c$lần. Nó góp phần$\lfloor c/2 \rfloor$các cặp hiện có. Tổng hợp tất cả các chữ cái sẽ cho số cặp đã có. 

Đặt độ dài chuỗi con là$m$, và để`odd`là số chữ cái có tần số lẻ. 

sử dụng$$\sum \left\lfloor \frac{c_i}{2} \right\rfloor
=
\frac{m - \text{odd}}{2},$$số cặp hiện tại hoàn toàn được xác định bởi số tần số lẻ. 

Số cặp tối đa có thể có là$\lfloor m/2 \rfloor$. Số cặp còn thiếu là$$\left\lfloor \frac{m}{2} \right\rfloor
-
\frac{m-\text{odd}}{2}.$$Đánh giá biểu thức này cho: 

Thậm chí$m$:$$\frac{\text{odd}}{2}$$Đối với số lẻ$m$:$$\frac{\text{odd}-1}{2}$$Cả hai trường hợp đều chính xác$$\left\lfloor \frac{\text{odd}}{2} \right\rfloor.$$Bây giờ hãy xem xét một sự thay đổi ký tự. Chúng ta có thể lấy một ký tự còn sót lại từ một chữ cái tần số lẻ và biến nó thành một ký tự tần số lẻ khác, loại bỏ hai số lẻ và tạo thêm một cặp. Vì vậy, mỗi cặp bổ sung được yêu cầu sẽ tốn chính xác một lần thay đổi. 

Vấn đề giảm xuống còn việc tìm số tần số lẻ trong mỗi chuỗi con được truy vấn. 

Vì bảng chữ cái chỉ chứa 26 chữ cái viết thường nên chúng ta có thể xây dựng các mảng tần số tiền tố. Sau đó, mỗi truy vấn sẽ xây dựng lại 26 tần số trong$O(26)$thời gian và đếm xem có bao nhiêu số lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$mỗi truy vấn |$O(1)$| Quá chậm | 
| Tối ưu |$O(26)$mỗi truy vấn |$O(26n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng số lượng tiền tố tần số cho tất cả 26 chữ cái viết thường. 
2. Đối với mỗi truy vấn$[l, r]$, tính tần số của mỗi chữ cái bên trong chuỗi con bằng cách sử dụng các khác biệt về tiền tố. 
3. Đếm xem trong số 26 tần số đó có bao nhiêu tần số lẻ. Gọi giá trị này`odd`. 
4. Đầu ra`odd // 2`. 

Lý do bước 4 có hiệu quả là vì mọi thay đổi đều có thể hợp nhất hai nhóm tần số lẻ thành một cặp bổ sung. Nếu có`odd`số lẻ, chúng ta phải loại bỏ tất cả trừ nhiều nhất một trong số chúng. Số lần loại bỏ cần thiết là chính xác`odd // 2`. 

### Tại sao nó hoạt động 

Điều bất biến là tần số có tính chẵn lẻ chẵn sẽ không đóng góp ký tự nào không khớp, trong khi tần số có tính chẵn lẻ lẻ đóng góp chính xác một ký tự không khớp. 

Một chuỗi con với`odd`các chữ cái tần số lẻ có chính xác`odd`những nhân vật chưa từng có. Để đạt được mức khớp tối đa có thể, tối đa một ký tự chưa khớp có thể còn lại. Mỗi lần chỉnh sửa có thể giảm số lượng ký tự chưa khớp xuống còn hai, vì nó di chuyển một ký tự từ nhóm lẻ này sang nhóm lẻ khác. Do đó số lần chỉnh sửa tối thiểu cần thiết chính xác là`odd // 2`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, q = map(int, input().split())
        s = input().strip()

        pref = [[0] * (n + 1) for _ in range(26)]

        for i, ch in enumerate(s, 1):
            idx = ord(ch) - ord('a')

            for c in range(26):
                pref[c][i] = pref[c][i - 1]

            pref[idx][i] += 1

        ans = []

        for _ in range(q):
            l, r = map(int, input().split())

            odd = 0

            for c in range(26):
                freq = pref[c][r] - pref[c][l - 1]
                odd += freq & 1

            ans.append(str(odd // 2))

        sys.stdout.write("\n".join(ans) + "\n")

solve()
```Mảng tiền tố lưu trữ số lượng tích lũy cho mỗi chữ cái. Tần số của một chữ cái trong khoảng truy vấn có được bằng cách trừ hai giá trị tiền tố. 

Đối với mỗi truy vấn, chúng tôi không bao giờ kiểm tra chuỗi con đó. Chúng tôi chỉ kiểm tra số lượng 26 chữ cái. Câu trả lời chỉ phụ thuộc vào số lượng số lẻ trong số đó. 

Lỗi triển khai phổ biến nhất là quên rằng các truy vấn được lập chỉ mục 1. sử dụng`pref[c][r] - pref[c][l - 1]`tránh được lỗi từng cái một. 

Một điểm tinh tế khác là câu trả lời không dựa trên độ dài chuỗi con. Khi đã biết tần số, chỉ có mô hình chẵn lẻ mới quan trọng. Hai chuỗi con có độ dài hoàn toàn khác nhau có thể tạo ra cùng một câu trả lời nếu chúng có cùng số chữ cái có tần số lẻ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 1
abcdef
1 6
```Tần suất bên trong truy vấn: 

| Thư | Tần số | Số lẻ? | 
| --- | --- | --- | 
| một | 1 | Có | 
| b | 1 | Có | 
| c | 1 | Có | 
| d | 1 | Có | 
| e | 1 | Có | 
| f | 1 | Có |`odd = 6`| lẻ | trả lời | 
| --- | --- | 
| 6 | 3 | 

Đầu ra:```
3
```Điều này chứng tỏ trường hợp cực đoan mà mọi nhân vật đều không thể so sánh được. Ba lần chỉnh sửa tạo ra ba cặp. 

### Ví dụ 2 

đầu vào:```
6 1
aaabbb
1 6
```Tần số: 

| Thư | Tần số | Số lẻ? | 
| --- | --- | --- | 
| một | 3 | Có | 
| b | 3 | Có |`odd = 2`| lẻ | trả lời | 
| --- | --- | 
| 2 | 1 | 

Đầu ra:```
1
```Ví dụ này cho thấy rằng chỉ có tính chẵn lẻ mới quan trọng. Mặc dù đã có nhiều chữ lặp lại nhưng vẫn còn hai nhóm lẻ cần chỉnh sửa một lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(26q + 26n)$| Xây dựng tiền tố và kiểm tra 26 chữ cái cho mỗi truy vấn | 
| Không gian |$O(26n)$| Số tiền tố cho mỗi chữ cái | 

Vì kích thước bảng chữ cái được cố định ở mức 26 nên thời gian chạy thực tế là tuyến tính trong tổng kích thước đầu vào. Với$\sum(n+q) \le 2 \cdot 10^5$, điều này thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    out = []

    t = int(input())

    for _ in range(t):
        n, q = map(int, input().split())
        s = input().strip()

        pref = [[0] * (n + 1) for _ in range(26)]

        for i, ch in enumerate(s, 1):
            idx = ord(ch) - ord('a')

            for c in range(26):
                pref[c][i] = pref[c][i - 1]

            pref[idx][i] += 1

        for _ in range(q):
            l, r = map(int, input().split())

            odd = 0
            for c in range(26):
                freq = pref[c][r] - pref[c][l - 1]
                odd += freq & 1

            out.append(str(odd // 2))

    return "\n".join(out)

# provided sample
assert run(
"""2
6 4
abcdef
1 6
2 2
3 6
1 4
6 3
aaabbb
1 3
4 6
1 6
"""
) == "\n".join(["3", "0", "2", "2", "0", "0", "1"])

# minimum size
assert run(
"""1
1 1
a
1 1
"""
) == "0"

# all equal
assert run(
"""1
5 1
aaaaa
1 5
"""
) == "0"

# all distinct odd length
assert run(
"""1
5 1
abcde
1 5
"""
) == "2"

# off-by-one boundary query
assert run(
"""1
4 2
abca
1 1
1 4
"""
) == "\n".join(["0", "1"])
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`0`| Kích thước tối thiểu | 
|`aaaaa`|`0`| Đã tối ưu | 
|`abcde`|`2`| Chuỗi con có độ dài lẻ | 
|`abca`với các truy vấn về ranh giới |`0`,`1`| Tính chính xác của việc lập chỉ mục tiền tố | 

## Vỏ cạnh 

Hãy xem xét:```
1
1 1
a
1 1
```Mảng tần số chứa một số lẻ. Thuật toán tính toán:```
odd = 1
answer = 1 // 2 = 0
```Một ký tự đơn lẻ không thể tạo thành một cặp, nhưng nó đã tối ưu vì cho phép một ký tự không khớp. 

Coi như:```
1
1
abcdef
1 6
```Tất cả sáu tần số đều là số lẻ.```
odd = 6
answer = 6 // 2 = 3
```Thuật toán xác định chính xác rằng cần phải chỉnh sửa ba lần để tạo ra ba cặp. 

Coi như:```
1
1
abcde
1 5
```Có năm tần số lẻ.```
odd = 5
answer = 5 // 2 = 2
```Một tần số lẻ có thể vẫn còn vì độ dài là số lẻ. Công thức tự động xử lý việc này mà không cần bất kỳ trường hợp đặc biệt nào. 

Coi như:```
1
1
aaabbb
1 6
```Các tần số là$3$Và$3$.```
odd = 2
answer = 1
```Việc thay đổi một ký tự còn sót lại từ nhóm này sang nhóm khác sẽ tạo ra cặp ký tự bị thiếu cuối cùng, phù hợp với kết quả tối ưu.
