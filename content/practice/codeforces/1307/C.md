---
title: "CF 1307C - Con bò và thông điệp"
description: "Chúng ta được cung cấp một chuỗi các chữ cái tiếng Anh viết thường. Một thông báo là bất kỳ dãy con nào mà các chỉ số được chọn tạo thành một cấp số cộng. Sự tiến triển có thể có bất kỳ sự khác biệt tích cực nào, bao gồm cả trường hợp đặc biệt về độ dài một. Nhiệm vụ không phải là tìm thấy tin nhắn."
date: "2026-06-11T17:36:57+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "math", "strings"]
categories: ["algorithms"]
codeforces_contest: 1307
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 621 (Div. 1 + Div. 2)"
rating: 1500
weight: 1307
solve_time_s: 793
verified: true
draft: false
---

[CF 1307C - Con bò và thông điệp](https://codeforces.com/problemset/problem/1307/C) 

**Đánh giá:** 1500 
**Tags:** sức mạnh vũ phu, dp, toán học, chuỗi 
**Thời gian giải:** 13m 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các chữ cái tiếng Anh viết thường. Một thông báo là bất kỳ dãy con nào mà các chỉ số được chọn tạo thành một cấp số cộng. Sự tiến triển có thể có bất kỳ sự khác biệt tích cực nào, bao gồm cả trường hợp đặc biệt về độ dài một. 

Nhiệm vụ không phải là tìm thấy tin nhắn. Thay vào đó, chúng ta phải xác định số lần tin nhắn ẩn xuất hiện thường xuyên nhất. 

Thoạt nhìn, đây có vẻ giống như một bài toán về dãy con với cấp số cộng, điều này gợi ý điều gì đó phức tạp. Quan sát quan trọng là chúng ta chỉ cần tần số tối đa trên tất cả các chuỗi ẩn chứ không cần nhận dạng chuỗi. 

Chiều dài chuỗi có thể đạt tới$10^5$. Bất kỳ giải pháp nào liệt kê rõ ràng các cấp số cộng, dãy con hoặc thông báo ứng viên đều không thể thực hiện được ngay lập tức. Thậm chí$O(n^2)$là quá lớn. Chúng ta cần một cái gì đó gần với thời gian tuyến tính. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. 

Coi như:```
a
```Thông điệp ẩn duy nhất là`"a"`, xảy ra một lần. Câu trả lời là:```
1
```Một giải pháp chỉ đếm các tin nhắn có độ dài ít nhất là hai sẽ trả về 0 không chính xác. 

Coi như:```
aaaa
```Tin nhắn`"a"`xảy ra bốn lần. Tin nhắn`"aa"`xảy ra$\binom{4}{2}=6$lần. Câu trả lời là:```
6
```Giải pháp chỉ theo dõi các chữ cái đơn lẻ sẽ bỏ lỡ rằng thông báo gồm hai chữ cái có thể xuất hiện thường xuyên hơn. 

Coi như:```
abab
```Tin nhắn`"ab"`xảy ra bốn lần:$$(1,2),(1,4),(3,4),(3,2\text{ is invalid because order matters})$$Có hệ thống hơn, mọi lúc sớm hơn`a`có thể ghép đôi sau này`b`. Câu trả lời xuất phát từ các cặp chữ cái có thứ tự, không nhất thiết phải bằng nhau. 

Bài học quan trọng là tần số tối đa có thể đến từ chuỗi một chữ cái hoặc từ chuỗi hai chữ cái. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng liệt kê các tin nhắn ẩn và đếm số lần xuất hiện của chúng. Ngay cả việc hạn chế bản thân trong các cấp số cộng cũng không giúp ích được gì. Có nhiều dãy con theo cấp số nhân và số cấp số cộng bên trong một chuỗi có độ dài$10^5$là rất lớn. Điều này là hoàn toàn không thể thực hiện được. 

Sự đột phá đến từ việc hiểu được một định lý ẩn chứa bên trong bài toán. 

Giả sử một tin nhắn ẩn có độ dài ít nhất là ba. Mỗi lần xuất hiện của tin nhắn đó đều chứa ký tự đầu tiên và ký tự cuối cùng. Nếu chúng ta quên các ký tự ở giữa, mỗi lần xuất hiện sẽ ánh xạ tới một lần xuất hiện của thông báo gồm hai chữ cái được tạo bởi các điểm cuối đó. 

Ánh xạ này là tiêm chích. Các lần xuất hiện khác nhau của thông báo dài hơn sẽ tạo ra các cặp điểm cuối khác nhau. Điều đó có nghĩa là số lần xuất hiện của bất kỳ thông báo nào có độ dài ít nhất là ba không thể vượt quá số lần xuất hiện của một số thông báo hai chữ cái. 

Kết quả là tin nhắn ẩn thường xuyên nhất phải có độ dài một hoặc hai. 

Bây giờ vấn đề trở nên đơn giản hơn nhiều. 

Đối với độ dài một, chúng ta chỉ cần tần số của mỗi ký tự. 

Đối với độ dài hai, chúng ta cần đếm, cho mỗi cặp chữ cái có thứ tự$(a,b)$, có bao nhiêu cặp chỉ số$i<j$thỏa mãn:$$s_i=a,\quad s_j=b.$$Chỉ có 26 chữ cái viết thường nên chỉ có$26^2=676$các cặp đặt hàng. 

Chúng ta có thể xử lý chuỗi từ trái sang phải. Khi chúng ta nhìn thấy một nhân vật$c$, mọi ký tự đã thấy trước đó$x$tạo ra sự xuất hiện mới của cặp$(x,c)$. 

Điều này dẫn đến một giải pháp thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu |$O(26n)$|$O(26^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì một mảng`cnt[26]`Ở đâu`cnt[x]`là số lần chữ cái`x`đã xuất hiện cho đến nay. 
2. Duy trì ma trận`pairs[26][26]`Ở đâu`pairs[x][y]`lưu trữ số lần xuất hiện của tin nhắn hai chữ cái bao gồm chữ cái`x`theo sau là chữ cái`y`. 
3. Quét chuỗi từ trái sang phải. 
4. Giả sử ký tự hiện tại là`c`. 
5. Đối với mỗi chữ cái`x`từ`a`ĐẾN`z`, thêm vào`cnt[x]`ĐẾN`pairs[x][c]`. 

Mỗi lần xuất hiện trước đó của`x`có thể được ghép nối với sự xuất hiện hiện tại của`c`, tạo ra một lần xuất hiện mới của thông báo`xc`. 
6. Tăng`cnt[c]`bởi một. 
7. Sau khi xử lý toàn bộ chuỗi, câu trả lời là giá trị lớn nhất trong số tất cả các mục của`cnt`và tất cả các mục của`pairs`. 

### Tại sao nó hoạt động 

Đối với tin nhắn có một chữ cái,`cnt[c]`chính xác là số lần xuất hiện của tin nhắn đó. 

Đối với các tin nhắn có hai chữ cái, mỗi lần xuất hiện hợp lệ sẽ tương ứng với một cặp vị trí$i<j$. Khi vị trí$j$được xử lý, vị trí$i$đã được tính vào bên trong`cnt`. Thuật toán thêm chính xác một đóng góp cho cặp đó và không bao giờ tính lại. 

Câu hỏi còn lại là tại sao những tin nhắn dài hơn có thể bị bỏ qua. Để một tin nhắn$t$có độ dài ít nhất ba bắt đầu bằng chữ cái$a$và kết thúc bằng chữ cái$b$. Mỗi lần xuất hiện của$t$xác định sự xuất hiện duy nhất của tin nhắn hai chữ cái$ab$sử dụng vị trí được chọn đầu tiên và cuối cùng. Sự xuất hiện riêng biệt của$t$tạo ra các cặp điểm cuối riêng biệt. Kể từ đây:$$\text{occurrences}(t) \le \text{occurrences}(ab).$$Vì vậy, không có tin nhắn nào có độ dài ít nhất ba chữ cái có thể đánh bại được tin nhắn một hoặc hai chữ cái hay nhất. Thuật toán kiểm tra tất cả các ứng cử viên như vậy, do đó kết quả tối đa mà nó trả về là câu trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

cnt = [0] * 26
pairs = [[0] * 26 for _ in range(26)]

for ch in s:
    c = ord(ch) - ord('a')

    for x in range(26):
        pairs[x][c] += cnt[x]

    cnt[c] += 1

ans = max(cnt)

for i in range(26):
    for j in range(26):
        ans = max(ans, pairs[i][j])

print(ans)
```Mảng`cnt`theo dõi tất cả các tin nhắn một chữ cái. Ma trận`pairs`theo dõi tất cả các tin nhắn hai chữ cái. 

Thứ tự cập nhật quan trọng. Trước tiên, chúng ta phải thêm đóng góp từ các ký tự đã thấy trước đó và chỉ sau đó mới tăng số lượng ký tự hiện tại. Nếu không, một ký tự có thể ghép nối không chính xác với chính nó ở cùng một vị trí. 

Tất cả số đếm vừa vặn thoải mái bên trong số nguyên 64 bit. Trong trường hợp xấu nhất, một tin nhắn gồm hai chữ cái có thể xảy ra về:$$\binom{10^5}{2} \approx 5 \times 10^9,$$lớn hơn nhiều so với giới hạn 32-bit. Số nguyên Python tự động xử lý việc này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
aaabb
```Xử lý dấu vết: 

| Vị trí | Nhân vật | cnt[a] trước | cnt[b] trước | cặp[a][a] | cặp[a][b] | 
| --- | --- | --- | --- | --- | --- | 
| 1 | một | 0 | 0 | 0 | 0 | 
| 2 | một | 1 | 0 | 1 | 0 | 
| 3 | một | 2 | 0 | 3 | 0 | 
| 4 | b | 3 | 0 | 3 | 3 | 
| 5 | b | 3 | 1 | 3 | 6 | 

Giá trị cuối cùng: 

| Tin nhắn | Đếm | 
| --- | --- | 
| một | 3 | 
| b | 2 | 
| aa | 3 | 
| ab | 6 | 
| bb | 1 | 

Tối đa là 6, phù hợp với câu trả lời mẫu. 

### Ví dụ 2 

đầu vào:```
abc
```Xử lý dấu vết: 

| Vị trí | Nhân vật | Đóng góp cặp mới | 
| --- | --- | --- | 
| 1 | một | không | 
| 2 | b | ab += 1 | 
| 3 | c | ac += 1, bc += 1 | 

Tính toán cuối cùng: 

| Tin nhắn | Đếm | 
| --- | --- | 
| một | 1 | 
| b | 1 | 
| c | 1 | 
| ab | 1 | 
| ac | 1 | 
| bc | 1 | 

Mỗi tin nhắn ẩn xuất hiện nhiều nhất một lần nên câu trả lời là 1. 

Ví dụ này chứng tỏ rằng thuật toán xử lý các chuỗi có tất cả các chữ cái riêng biệt một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(26n)$| Đối với mỗi ký tự, chúng tôi lặp qua tất cả 26 chữ cái | 
| Không gian |$O(26^2)$| Ma trận đếm cặp có 676 mục | 

Từ$26$là một hằng số, thời gian chạy thực tế là tuyến tính theo độ dài chuỗi. Vì$n=10^5$, thuật toán thực hiện khoảng 2,6 triệu bản cập nhật, dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import io
import sys

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = sys.stdin.readline().strip()

    cnt = [0] * 26
    pairs = [[0] * 26 for _ in range(26)

    ]

    for ch in s:
        c = ord(ch) - ord('a')

        for x in range(26):
            pairs[x][c] += cnt[x]

        cnt[c] += 1

    ans = max(cnt)

    for i in range(26):
        for j in range(26):
            ans = max(ans, pairs[i][j])

    return str(ans)

# provided sample
assert solve("aaabb\n") == "6"

# minimum size
assert solve("a\n") == "1"

# all equal
assert solve("aaaa\n") == "6"

# distinct letters
assert solve("abc\n") == "1"

# alternating pattern
assert solve("abab\n") == "3"

# large repetition structure
assert solve("aabb\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`1`| Chuỗi kích thước tối thiểu | 
|`aaaa`|`6`| Tin nhắn hai chữ cái đánh bại tin nhắn một chữ cái | 
|`abc`|`1`| Không có tin nhắn lặp lại | 
|`abab`|`3`| Đếm cặp đặt hàng | 
|`aabb`|`4`| Tích lũy cặp chéo nhóm | 

## Vỏ cạnh 

Hãy xem xét:```
a
```Mảng`cnt`trở thành`[1,0,...]`. Không có số lượng cặp nào được tạo ra. Giá trị tối đa là 1, đúng. 

Coi như:```
aaaa
```Các bản cập nhật tạo ra:$$pairs[a][a] = 1+2+3 = 6.$$Số lượng ký tự đơn chỉ là 4. Thuật toán trả về đúng 6. 

Hãy xem xét:```
abab
```Khi thứ hai`b`được xử lý, đã có hai`a`các ký tự trước nó, vì vậy hai ký tự mới`ab`lần xuất hiện được thêm vào. Giá trị cuối cùng của`pairs[a][b]`trở thành 3, tương ứng với các cặp:$$(1,2),\ (1,4),\ (3,4).$$Thuật toán đếm từng cặp thứ tự hợp lệ đúng một lần. 

Coi như:```
abcdefghijklmnopqrstuvwxyz
```Mỗi nhân vật xuất hiện một lần. Mỗi tin nhắn gồm hai chữ cái xuất hiện nhiều nhất một lần. Giá trị tối đa trong số tất cả các giá trị được lưu trữ vẫn là 1, đây là câu trả lời đúng.
