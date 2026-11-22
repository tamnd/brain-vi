---
title: "CF 39J - Kiểm tra chính tả"
description: "Chúng tôi được cung cấp hai chuỗi chữ thường. Chuỗi đầu tiên dài hơn chuỗi thứ hai đúng một ký tự. Chúng tôi muốn tìm mọi vị trí trong chuỗi đầu tiên sao cho việc loại bỏ ký tự ở vị trí đó sẽ làm cho hai chuỗi giống hệt nhau."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "hashing", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 39
codeforces_index: "J"
codeforces_contest_name: "School Team Contest 1 (Winter Computer School 2010/11)"
rating: 1500
weight: 39
solve_time_s: 94
verified: true
draft: false
---
[CF 39J - Kiểm tra chính tả](https://codeforces.com/problemset/problem/39/J) 

**Đánh giá:** 1500 
**Tags:** băm, triển khai, chuỗi 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai chuỗi chữ thường. Chuỗi đầu tiên dài hơn chuỗi thứ hai đúng một ký tự. Chúng tôi muốn tìm mọi vị trí trong chuỗi đầu tiên sao cho việc loại bỏ ký tự ở vị trí đó sẽ làm cho hai chuỗi giống hệt nhau. 

Nhiệm vụ không phải là thực hiện các chỉnh sửa tùy tiện. Chỉ được phép xóa một lần và nó phải xảy ra trong chuỗi dài hơn. Câu trả lời bao gồm tất cả các vị trí dựa trên 1 hợp lệ nơi việc xóa này có tác dụng. 

Các ràng buộc là phần làm thay đổi hoàn toàn bản chất của vấn đề. Mỗi chuỗi có thể chứa tối đa$10^6$nhân vật. Một giải pháp bậc hai là không thể ở đây. Thậm chí một$O(n^2)$thuật toán với hằng số nhỏ sẽ yêu cầu khoảng$10^{12}$trong trường hợp xấu nhất vượt xa thời hạn. Cần phải có một cách tiếp cận tuyến tính hoặc gần tuyến tính. 

Việc mô phỏng trực tiếp cho mọi vị trí xóa là nguy hiểm vì việc sao chép hoặc xây dựng lại chuỗi liên tục trở nên cực kỳ tốn kém ở quy mô này. Thậm chí còn tạo$n$chuỗi có độ dài tạm thời$n$sẽ sử dụng một lượng lớn bộ nhớ và thời gian. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét trường hợp mọi thao tác xóa đều có hiệu quả: 

đầu vào:```
aaaa
aaa
```Đầu ra:```
4
1 2 3 4
```Loại bỏ bất kỳ ký tự nào sẽ tạo ra`"aaa"`. Giải pháp dựa trên sự không khớp bất cẩn có thể dừng lại sau khi tìm được vị trí hợp lệ đầu tiên và bỏ lỡ các vị trí khác. 

Một trường hợp phức tạp khác xuất hiện khi sự không khớp xảy ra ở gần cuối: 

đầu vào:```
abcd
abc
```Đầu ra:```
1
4
```Ba ký tự đầu tiên đã khớp. Việc xóa hợp lệ duy nhất là ký tự cuối cùng. Các thuật toán chỉ tìm kiếm sự không khớp đầu tiên có thể kết luận sai rằng không cần xóa. 

Các ký tự lặp đi lặp lại cũng tạo ra sự mơ hồ: 

đầu vào:```
abcc
abc
```Đầu ra:```
1
4
```Xóa cái cuối cùng`'c'`hoạt động, nhưng xóa cái đầu tiên`'c'`không. Phương pháp dựa trên tần số đơn giản sẽ thất bại vì cả hai chuỗi đều chứa số lượng ký tự gần như giống nhau. 

Cuối cùng, có những trường hợp không có câu trả lời hợp lệ: 

đầu vào:```
abcd
axy
```Đầu ra:```
0
```Ngay cả sau khi xóa một ký tự khỏi`"abcd"`, các chuỗi không bao giờ có thể bằng nhau. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Hãy thử xóa mọi ký tự khỏi chuỗi dài hơn và so sánh kết quả với chuỗi ngắn hơn. 

Đối với một chuỗi có độ dài$n$, có$n$xóa ứng viên. Xây dựng và so sánh từng chi phí chuỗi kết quả$O(n)$, do đó tổng độ phức tạp trở thành$O(n^2)$. 

Với$n = 10^6$, cách tiếp cận này sẽ yêu cầu khoảng$10^{12}$các thao tác của nhân vật. Điều đó hoàn toàn không thể thực hiện được. 

Điều quan trọng là sau khi xóa một ký tự, các phần còn lại của chuỗi phải xếp hàng hoàn hảo. 

Giả sử chúng ta xóa vị trí$i$từ chuỗi dài hơn$s$. Sau đó: 

- Tiền tố trước$i$phải khớp rồi. 
- Hậu tố theo sau$i$TRONG$s$phải khớp với hậu tố tương ứng trong$t$, dịch chuyển một vị trí. 

Điều này tự nhiên gợi ý tiền xử lý tiền tố và hậu tố phù hợp. 

Định nghĩa:

-`pref[i]`như thể`s[0:i] == t[0:i]`-`suff[i]`như thể`s[i:] == t[i-1:]`Định nghĩa hậu tố giải thích cho sự thay đổi một ký tự do xóa. 

Bây giờ một vị trí$i$là hợp lệ nếu: 

- mọi thứ trước đây$i$trận đấu 
- mọi chuyện sau đó$i$trận đấu sau khi chuyển 

Vì vậy, chúng tôi chỉ cần kiểm tra thời gian liên tục cho mỗi vị trí sau quá trình tiền xử lý tuyến tính. 

Điều này làm giảm độ phức tạp từ bậc hai sang tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai chuỗi`s`Và`t`. 
2. Hãy để`n = len(s)`. Từ`s`được đảm bảo dài hơn một ký tự,`len(t) = n - 1`. 
3. Xây dựng mảng khớp tiền tố`pref`.`pref[i]`có nghĩa là lần đầu tiên`i`ký tự của cả hai chuỗi giống hệt nhau. 

Khởi tạo:```
pref[0] = True
```Sau đó với mỗi`i`từ`0`ĐẾN`n-2`:```
pref[i + 1] = pref[i] and (s[i] == t[i])
```Điều này hoạt động vì tiền tố khớp với`i+1`chỉ khi các tiền tố trước khớp và các ký tự hiện tại bằng nhau. 
4. Xây dựng mảng kết hợp hậu tố`suff`.`suff[i]`có nghĩa:```
s[i:] == t[i-1:]
```Các chỉ số được dịch chuyển vì một ký tự từ`s`được cho là đã bị xóa. 

Khởi tạo:```
suff[n] = True
```Sau đó lặp lại:```
suff[i] = suff[i + 1] and (s[i] == t[i - 1])
```5. Kiểm tra từng vị trí xóa`i`. 

Đang xóa`s[i]`hoạt động nếu:```
pref[i] and suff[i + 1]
```

`pref[i]`đảm bảo các phần bên trái khớp nhau.`suff[i + 1]`đảm bảo các bộ phận phù hợp sau khi bỏ qua`s[i]`. 
6. Xuất ra tất cả các vị trí hợp lệ bằng cách sử dụng chỉ mục dựa trên 1. 

### Tại sao nó hoạt động 

Đối với bất kỳ vị trí xóa nào$i$, chuỗi kết quả bao gồm hai phần không thay đổi: 

- tiền tố trước$i$- hậu tố theo sau$i$Điều kiện tiền tố kiểm tra xem:```
s[0:i] == t[0:i]
```Điều kiện hậu tố kiểm tra rằng:```
s[i+1:] == t[i:]
```Nếu cả hai đều đúng thì mọi ký tự trong chuỗi kết quả đều khớp với chuỗi đích. 

Nếu một trong hai điều kiện không thành công, một số ký tự sẽ khác và việc xóa không thể thực hiện được. 

Vì mọi vị trí ứng viên đều được kiểm tra chính xác theo các điều kiện cần và đủ này nên thuật toán là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
t = input().strip()

n = len(s)

pref = [False] * (n + 1)
pref[0] = True

for i in range(n - 1):
    pref[i + 1] = pref[i] and (s[i] == t[i])

suff = [False] * (n + 1)
suff[n] = True

for i in range(n - 1, 0, -1):
    suff[i] = suff[i + 1] and (s[i] == t[i - 1])

ans = []

for i in range(n):
    if pref[i] and suff[i + 1]:
        ans.append(i + 1)

print(len(ans))

if ans:
    print(*ans)
```Mảng tiền tố lưu trữ xem các tiền tố có khớp chính xác với một độ dài nhất định hay không. Việc sử dụng phương pháp truyền bá boolean tích lũy sẽ tránh được việc tính toán lại các phép so sánh nhiều lần. 

Mảng hậu tố tinh tế hơn một chút vì các chỉ số được dịch chuyển. Sau khi xóa`s[i]`, tính cách`s[i+1]`phải phù hợp với`t[i]`. Đó là lý do tại sao việc so sánh hậu tố sử dụng`t[i-1]`trong quá trình tiền xử lý. 

Vòng lặp ngược bắt đầu từ`n - 1`và dừng lại ở`1`bởi vì`t[i - 1]`phải còn hiệu lực. Chức vụ`0`không cần giá trị hậu tố vì việc xóa ký tự đầu tiên chỉ phụ thuộc vào`suff[1]`. 

Vòng lặp cuối cùng kiểm tra mọi khả năng xóa một cách độc lập trong thời gian không đổi. 

Việc triển khai tránh hoàn toàn việc xây dựng các chuỗi tạm thời. Điều này rất quan trọng đối với các đầu vào lớn vì việc cắt lặp đi lặp lại sẽ làm tăng đáng kể cả thời gian chạy và mức sử dụng bộ nhớ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
abdrakadabra
abrakadabra
```| tôi | s[i] | trước[i] | đủ[i+1] | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | một | Đúng | Sai | Không | 
| 1 | b | Đúng | Sai | Không | 
| 2 | d | Đúng | Đúng | Có | 
| 3 | r | Sai | Sai | Không | 

Đầu ra:```
1
3
```Xóa ký tự thứ ba`'d'`biến đổi`"abdrakadabra"`vào trong`"abrakadabra"`. 

Ví dụ này minh họa cách thuật toán tách biệt vị trí không khớp chính xác trong khi vẫn xác minh tất cả các phân đoạn xung quanh. 

### Ví dụ 2 

đầu vào:```
aaaa
aaa
```| tôi | s[i] | trước[i] | đủ[i+1] | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | một | Đúng | Đúng | Có | 
| 1 | một | Đúng | Đúng | Có | 
| 2 | một | Đúng | Đúng | Có | 
| 3 | một | Đúng | Đúng | Có | 

Đầu ra:```
4
1 2 3 4
```Mỗi lần xóa đều tạo ra chuỗi kết quả giống nhau. 

Trường hợp này xác nhận thuật toán xử lý chính xác các ký tự lặp lại và không dừng lại sau lần khớp đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một đường chuyền tiến, một đường chuyền lùi, một đường chuyền xác thực | 
| Không gian |$O(n)$| Mảng tiền tố và hậu tố | 

Với chuỗi lên đến$10^6$ký tự, độ phức tạp tuyến tính chính xác là những gì vấn đề yêu cầu. Giải pháp chỉ thực hiện một vài lần truyền dữ liệu và dễ dàng phù hợp với giới hạn thời gian. Việc sử dụng bộ nhớ cũng an toàn dưới giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    s = input().strip()
    t = input().strip()

    n = len(s)

    pref = [False] * (n + 1)
    pref[0] = True

    for i in range(n - 1):
        pref[i + 1] = pref[i] and (s[i] == t[i])

    suff = [False] * (n + 1)
    suff[n] = True

    for i in range(n - 1, 0, -1):
        suff[i] = suff[i + 1] and (s[i] == t[i - 1])

    ans = []

    for i in range(n):
        if pref[i] and suff[i + 1]:
            ans.append(i + 1)

    out = [str(len(ans))]

    if ans:
        out.append(" ".join(map(str, ans)))

    print("\n".join(out))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return result

# provided sample
assert run(
    "abdrakadabra\nabrakadabra\n"
) == "1\n3\n", "sample 1"

# minimum size
assert run(
    "ab\na\n"
) == "1\n2\n", "minimum size"

# all deletions valid
assert run(
    "aaaa\naaa\n"
) == "4\n1 2 3 4\n", "all positions valid"

# deletion at beginning
assert run(
    "xabc\nabc\n"
) == "1\n1\n", "delete first character"

# deletion at end
assert run(
    "abcd\nabc\n"
) == "1\n4\n", "delete last character"

# no valid deletion
assert run(
    "abcd\naxy\n"
) == "0\n", "impossible case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`ab / a`|`2`| Đầu vào có ý nghĩa nhỏ nhất | 
|`aaaa / aaa`|`1 2 3 4`| Nhiều câu trả lời hợp lệ | 
|`xabc / abc`|`1`| Xử lý đúng vị trí đầu tiên | 
|`abcd / abc`|`4`| Xử lý đúng vị trí cuối cùng | 
|`abcd / axy`|`0`| Chuyển đổi bất khả thi | 

## Vỏ cạnh 

Xem xét các ký tự lặp lại: 

đầu vào:```
aaaa
aaa
```Mảng tiền tố vẫn đúng ở mọi vị trí vì tất cả các ký tự được so sánh đều bằng nhau. Mảng hậu tố cũng đúng ở mọi nơi vì lý do tương tự. 

Đối với mỗi chỉ số`i`:```
pref[i] = True
suff[i+1] = True
```Vì vậy, tất cả các vị trí đều được chấp nhận. 

Bây giờ hãy xem xét việc xóa ở cuối: 

đầu vào:```
abcd
abc
```Việc so sánh tiền tố thành công đối với ba ký tự đầu tiên:```
a == a
b == b
c == c
```Khi kiểm tra xóa ở vị trí 4:```
pref[3] = True
suff[4] = True
```Điều kiện hậu tố hoàn toàn đúng vì không còn gì sau ký tự bị xóa. 

Thuật toán xuất ra chính xác:```
1
4
```Bây giờ hãy xem xét một trường hợp không thể xảy ra: 

đầu vào:```
abcd
axy
```Việc so sánh tiền tố không thành công ngay sau ký tự đầu tiên:```
b != x
```Không có vị trí xóa nào có thể thỏa mãn đồng thời cả điều kiện tiền tố và hậu tố. 

Các đầu ra thuật toán:```
0
```Cuối cùng, hãy xem xét sự mơ hồ gây ra bởi các chữ cái lặp đi lặp lại: 

đầu vào:```
abcc
abc
```Xóa cái cuối cùng`'c'`hoạt động:```
abc == abc
```Xóa cái đầu tiên`'c'`sản xuất:```
abc != abc
```Thực ra:```
abcc -> abc
```chỉ sau khi loại bỏ ký tự thứ tư. 

Các điều kiện tiền tố và hậu tố phân biệt chính xác các trường hợp này, do đó thuật toán đưa ra:```
1
4
```
