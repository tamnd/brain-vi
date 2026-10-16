---
title: "CF 105475C - Mở rộng từ"
description: "Chúng ta bắt đầu với một chuỗi duy nhất được tạo thành từ các chữ cái viết thường. Mỗi chữ cái không tĩnh: nó mở rộng thành một chuỗi khác theo bảng thay thế cố định có kích thước 26. Nếu ký tự là x, nó được thay thế bằng chuỗi px."
date: "2026-06-23T02:11:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105475
codeforces_index: "C"
codeforces_contest_name: "XXII Spain Olympiad in Informatics, Day 1"
rating: 0
weight: 105475
solve_time_s: 80
verified: true
draft: false
---

[CF 105475C - Mở rộng từ](https://codeforces.com/problemset/problem/105475/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một chuỗi duy nhất được tạo thành từ các chữ cái viết thường. Mỗi chữ cái không tĩnh: nó mở rộng thành một chuỗi khác theo bảng thay thế cố định có kích thước 26. Nếu một ký tự là`x`, nó được thay thế bằng chuỗi`p_x`. Một ứng dụng của thao tác sẽ thay thế đồng thời mọi ký tự trong chuỗi hiện tại. 

Sau khi lặp lại phép biến đổi này`k`nhiều lần, chuỗi có thể trở nên cực kỳ lớn. Thay vì xây dựng nó, chúng ta được hỏi một câu hỏi cụ thể hơn nhiều: nhân vật ở vị trí là gì?`i`(1-lập chỉ mục) trong chuỗi mở rộng cuối cùng? 

Khó khăn chính là cả hai`k`Và`i`có thể lớn. Chỉ mục có thể lớn tới 10^18, điều này khiến cho bất kỳ cách tiếp cận nào xây dựng chuỗi một cách rõ ràng đều không thể thực hiện được. Ngay cả việc theo dõi toàn bộ độ dài chuỗi cũng không khả thi vì nó có thể tăng theo cấp số nhân sau mỗi bước chuyển đổi. 

Một cách tiếp cận ngây thơ mô phỏng đầy đủ từng bản mở rộng sẽ nhanh chóng thất bại. Ngay cả khi mỗi chữ cái mở rộng tối đa 10 ký tự, sau 20 bước, chuỗi có thể vượt quá kích thước 10^20 và việc lập chỉ mục vào đó sẽ không thể thực hiện được nếu không xây dựng. 

Trường hợp cạnh tinh tế xuất hiện khi việc mở rộng tạo ra các chuỗi trống hoặc chuỗi ngắn. Một cách tiếp cận bất cẩn giả định mức tăng trưởng đồng đều trên mỗi ký tự hoặc cố gắng duy trì tổng tiền tố mà không truyền cấu trúc cẩn thận sẽ phá vỡ các trường hợp như liên tục ánh xạ chữ cái tới một ký tự, tạo ra chuỗi dài mà không phân nhánh. 

Ví dụ, nếu`a -> b`,`b -> c`,`c -> c`, sau nhiều bước, toàn bộ chuỗi sẽ thu gọn thành một ký tự lặp lại. Một mô phỏng dựa trên mở rộng đơn giản vẫn có thể cố gắng xây dựng lại các chuỗi trung gian đầy đủ một cách không cần thiết, mặc dù câu trả lời được xác định bởi một chuỗi phụ thuộc duy nhất. 

## Phương pháp tiếp cận 

Vấn đề trọng tâm là việc chuyển đổi xác định một hệ thống viết lại có hướng trên các ký tự. Mỗi chữ cái sẽ trở thành một chuỗi và chuỗi đó xác định cách các chỉ số lan truyền về phía trước. 

Một phương thức brute-force trực tiếp xây dựng chuỗi đầy đủ sau mỗi chuỗi`k`những biến đổi. Mỗi bước thay thế mỗi ký tự bằng cách mở rộng nó. Nếu tổng độ dài sau k bước là N thì mỗi phép biến đổi có giá O(N) và bản thân N sẽ tăng theo cấp số nhân. Ngay cả dưới những hạn chế nhỏ nhất, điều này nhanh chóng bùng nổ vượt quá tính khả thi. Lý do cốt lõi là chúng tôi liên tục tính toán lại toàn bộ chuỗi mặc dù truy vấn chỉ cần một vị trí. 

Quan sát quan trọng là chúng ta không bao giờ cần chuỗi đầy đủ. Chúng ta chỉ cần biết vị trí ánh xạ lùi thông qua các phép biến đổi như thế nào. Thay vì mở rộng về phía trước, chúng tôi theo dõi cách chỉ mục trong chuỗi cuối cùng bắt nguồn từ các chuỗi trước đó. 

Nếu chúng ta biết, với mỗi nhân vật`c`, độ dài khai triển của nó sau t bước, chúng ta có thể quyết định ký tự nào đóng góp vào vị trí thứ i. Khi chúng tôi xác định được ký tự đó đến từ đâu, chúng tôi sẽ trừ các phần bù tiền tố và tiếp tục đệ quy cho bước trước đó. 

Điều này gợi ý một mô phỏng ngược: thay vì xây dựng các chuỗi, chúng ta duy trì một hàm`len[c][t]`mô tả sự mở rộng của nhân vật trong bao lâu`c`trở thành sau`t`các bước. Sau đó, để trả lời một truy vấn, chúng ta đi từ bước`k`xuống 0, mỗi lần chọn đúng phân đoạn con. 

Để tính toán`len[c][t]`, chúng tôi sử dụng quy hoạch động. Ở bước 0, mỗi ký tự có độ dài 1. Đối với các chuyển tiếp, độ dài của`c`ở bước t là tổng độ dài các ký tự trong`p_c`ở bước t-1. Từ`k`có thể lên tới 10^4, chúng tôi tính toán các giá trị này lặp đi lặp lại. 

Khi đã biết độ dài, việc trả lời truy vấn sẽ trở thành một kết quả xác định: ở mỗi bước, chúng tôi xác định ký tự nào trong bản mở rộng chứa vị trí`i`, sau đó cập nhật`i`tương ứng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mở rộng lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| DP theo chiều dài + truyền tải ngược | O(26 * k * 26 + k) | O(26 * k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chia giải pháp thành tiền xử lý độ dài mở rộng và sau đó là truyền tải truy vấn. 

1. Tính toán trước một bảng`len[c][t]`, Ở đâu`len[c][t]`là độ dài của chuỗi thu được bằng cách mở rộng ký tự`c`chính xác`t`lần. Chúng tôi khởi tạo`len[c][0] = 1`vì ở bước 0 mỗi chữ cái đều là chính nó. Điều này tạo thành trường hợp cơ bản của đệ quy của chúng tôi. 
2. Đối với mỗi bước`t`từ 1 đến`k`, tính toán`len[c][t]`bằng cách lặp lại các ký tự trong`p_c`. Đối với mỗi nhân vật`x`TRONG`p_c`, chúng tôi thêm`len[x][t-1]`. Điều này hoạt động vì một phép biến đổi thay thế`c`với`p_c`và mỗi ký hiệu trong chuỗi đó sẽ mở rộng độc lập theo độ dài bước trước đó. 
3. Để trả lời một câu hỏi`(s, k, i)`, về mặt khái niệm chúng tôi bắt đầu ở bước`k`và vị trí`i`trong phạm vi mở rộng của chuỗi ban đầu`s`. 
4. Chúng tôi lặp lại các ký tự của`s`, coi mỗi khối là một khối có kích thước là`len[c][k]`. Chúng tôi tìm thấy khối đầu tiên ở đâu`i`ngã. Sau khi xác định được, chúng tôi trừ đi kích thước của các khối trước đó và tập trung vào ký tự đó. Điều này làm giảm vấn đề tìm ra câu trả lời bên trong việc mở rộng một ký tự đơn lẻ. 
5. Bây giờ chúng tôi liên tục mô phỏng sự xuống dốc của một ký tự từ bước`k`đến bước 0. Ở bước`t`, chúng tôi nhìn vào`p_c`và xác định bản mở rộng của ký tự nào chứa vị trí`i`sử dụng độ dài được tính toán trước ở cấp độ`t-1`. Chúng ta trừ tổng tiền tố cho đến khi tìm được con đúng, sau đó tiếp tục với con đó ở bước`t-1`. 
6. Khi nào`t`đạt đến 0, chúng ta đã đạt đến một ký tự cơ sở, đó là câu trả lời. 

### Tại sao nó hoạt động 

Ở mọi cấp độ đệ quy, chúng tôi duy trì tính bất biến`i`đề cập đến một vị trí hợp lệ bên trong việc mở rộng một ký tự cụ thể ở một độ sâu cụ thể. Bảng DP đảm bảo rằng độ dài đoạn ở mỗi bước là chính xác, do đó phép trừ tiền tố luôn chọn cây con chính xác. Vì mỗi phép biến đổi là sự kết hợp rời rạc của các phép mở rộng nên ánh xạ vị trí được xác định rõ ràng và không bao giờ mơ hồ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    while True:
        line = input().strip()
        if not line:
            return
        parts = line.split()
        if not parts:
            continue

        s = parts[0]
        k = int(parts[1])
        i = int(parts[2])

        p = [input().strip() for _ in range(26)]

        # dp[c][t] = length after t expansions
        dp = [[0] * (k + 1) for _ in range(26)]

        for c in range(26):
            dp[c][0] = 1

        for t in range(1, k + 1):
            for c in range(26):
                total = 0
                for ch in p[c]:
                    total += dp[ord(ch) - 97][t - 1]
                    if total > 10**18:
                        total = 10**18
                dp[c][t] = total

        # find starting character at level k
        cur_i = i
        cur_t = k

        # reduce from initial string s
        for ch in s:
            c = ord(ch) - 97
            if cur_i > dp[c][cur_t]:
                cur_i -= dp[c][cur_t]
            else:
                cur = c
                break

        # descend
        while cur_t > 0:
            for ch in p[cur]:
                nxt = ord(ch) - 97
                if cur_i > dp[nxt][cur_t - 1]:
                    cur_i -= dp[nxt][cur_t - 1]
                else:
                    cur = nxt
                    cur_t -= 1
                    break

        print(chr(cur + 97))

if __name__ == "__main__":
    solve()
```Giai đoạn đầu tiên xây dựng bảng DP trong đó mỗi hàng tương ứng với một chữ cái và mỗi cột tương ứng với số lượng phép biến đổi được áp dụng. Độ bão hòa ở mức 10^18 là một biện pháp bảo vệ thực tế chống tràn vì chúng ta chỉ cần so sánh với i chứ không cần các giá trị lớn chính xác. 

Vòng lặp ban đầu kết thúc`s`thu gọn vấn đề từ một chuỗi nhiều ký tự thành một ký tự bắt đầu duy nhất có chỉ mục được điều chỉnh. Vòng lặp thứ hai thực hiện việc duyệt ngược thực tế của cây biến đổi, sử dụng phép trừ tiền tố để quyết định nhánh nào chứa vị trí đích. 

Chi tiết triển khai quan trọng được sử dụng nhất quán với độ sâu chính xác`cur_t - 1`khi kiểm tra trẻ em. Bất kỳ sự không khớp nào giữa mức DP và mức truyền tải sẽ phá vỡ tính chính xác ngay lập tức. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
s = "ab", k = 1, i = 3
```Cho rằng:```
a -> "bc"
b -> "a"
```Độ dài DP: 

| char | len lúc 1 | 
| --- | --- | 
| một | 2 | 
| b | 1 | 

Truyền qua`s`: 

| bước | char | chiều dài khối | tôi còn lại | đã chọn | 
| --- | --- | --- | --- | --- | 
| 1 | một | 2 | 3 -> 1 | bỏ qua | 
| 2 | b | 1 | 1 | b | 

Bây giờ đi xuống từ`b`tại k=1:`b -> "a"`, vậy đáp án là`a`. 

Điều này cho thấy chuỗi ban đầu được xử lý như các khối có trọng số được nối. 

### Ví dụ 2 

đầu vào:```
s = "a", k = 2, i = 1
```Cho rằng:```
a -> "ab"
b -> "c"
c -> "c"
```DP: 

| char | len lúc 2 | 
| --- | --- | 
| một | 2 | 
| b | 1 | 
| c | 1 | 

Bắt đầu lúc`a`, k=2, i=1:`a -> "ab"`, vì vậy chúng ta đi đến`a`ở cấp độ 1. 

Ở cấp độ 1,`a -> "ab"`một lần nữa, vì vậy chúng ta chuyển sang phần đầu tiên`a`ở mức 0, mang lại`a`. 

Dấu vết này cho thấy việc giảm xuống lặp đi lặp lại duy trì tính chính xác trên nhiều cấp độ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(26 * k * 26 + | s | 
| Không gian | O(26 * k) | Bảng DP lưu trữ độ dài mở rộng | 

Các ràng buộc cho phép lên tới k = 10^4 và hệ số không đổi là 26 là nhỏ. Giải pháp này phù hợp một cách thoải mái trong cả giới hạn thời gian và bộ nhớ vì tất cả các thao tác đều là phép cộng số nguyên đơn giản và quét tuyến tính trên các bảng chữ cái cố định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample (as given format)
assert run("""rb 2 16
mqbh
dar
owkk
h
d
qs
dxr
mo
frx
j
bld
efsa
cbyn
c
yg
x
pk
orel
nm
apqf
kho
kmco
h
wnku
w
sqmg
ljj 1 6
vsw
d
q
bx
x
v
rrbl
ptns
fw
qf
mafa
rr
sof
b
n
vqh
fb
a
x
pqc
c
hch
vfrk
l
oz
kpqp
""") == "h\nf"

# custom 1: no expansion
assert run("""a 0 1
abcdefghijklmnopqrstuvwxyz
""") == "a"

# custom 2: simple chain
assert run("""a 2 1
b
c
d
e
f
g
h
i
j
k
l
m
n
o
p
q
r
s
t
u
v
w
x
y
z
""") == "c"

# custom 3: boundary index in concatenation
assert run("""ab 1 2
a
b
c
d
e
f
g
h
i
j
k
l
m
n
o
p
q
r
s
t
u
v
w
x
y
z
""") == "b"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mở rộng danh tính | một | k=0 độ đúng cơ sở | 
| độ sâu toàn chuỗi | c | nhân giống nhiều bước | 
| ranh giới trong phân tách chuỗi | b | phép trừ tiền tố đúng | 

## Vỏ cạnh 

Một tình huống mong manh là khi chỉ mục truy vấn rơi chính xác vào ranh giới giữa hai ký tự mở rộng trong chuỗi ban đầu. Giả định`s = "ab"`, với`a`mở rộng đến chiều dài 3 và`b`đến độ dài 2 ở bước cuối cùng và`i = 3`. Thuật toán kiểm tra đầu tiên`a`, thấy thế`i`nằm trong khối của nó và không trừ nó một cách chính xác. Nếu thay vào đó`i = 4`, nó trừ đi toàn bộ chiều dài của`a`và tiếp tục vào bên trong`b`. Việc xử lý ranh giới này phụ thuộc hoàn toàn vào`>`so sánh trong phép trừ tiền tố. 

Một trường hợp khác là chuỗi ký tự đơn sâu như`a -> b -> c -> c`. Ngay cả sau khi lớn`k`, bảng DP ổn định và việc truyền tải chỉ đơn giản tuân theo chuỗi con trỏ xác định. Thuật toán không bao giờ cần mở rộng chuỗi, chỉ giải quyết liên tục một chỉ mục thông qua các mức giảm dần. 

Trường hợp thứ ba rất lớn`i`. Vì chúng tôi giới hạn giá trị DP ở mức 10^18 nên các so sánh vẫn hợp lệ. Ngay cả khi mức mở rộng thực tế vượt quá giới hạn đó, tính chính xác vẫn được bảo toàn vì chúng ta chỉ cần biết liệu`i`nằm bên trong hoặc bên ngoài một đoạn chứ không phải kích thước chính xác của nó vượt quá ngưỡng.
