---
title: "CF 101A - Bài tập về nhà"
description: "Chúng ta bắt đầu bằng một chuỗi chữ thường và có thể xóa tối đa k ký tự khỏi chuỗi đó. Các ký tự còn lại phải giữ nguyên thứ tự ban đầu vì việc xóa các ký tự sẽ tạo ra một chuỗi con."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 101
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 79 (Div. 1 Only)"
rating: 1200
weight: 101
solve_time_s: 125
verified: true
draft: false
---

[CF 101A - Bài tập về nhà](https://codeforces.com/problemset/problem/101/A) 

**Đánh giá:** 1200 
**Thẻ:** tham lam 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu bằng một chuỗi chữ thường và có thể xóa nhiều nhất`k`các ký tự từ đó. Các ký tự còn lại phải giữ nguyên thứ tự ban đầu vì việc xóa các ký tự sẽ tạo ra một chuỗi con. Mục tiêu của chúng tôi không phải là tối đa hóa độ dài còn lại mà là giảm thiểu số lượng chữ cái riêng biệt vẫn xuất hiện. 

Nếu một chữ cái xuất hiện dù chỉ một lần trong chuỗi cuối cùng thì chữ cái đó vẫn được tính là một ký tự riêng biệt. Để loại bỏ hoàn toàn một chữ cái khỏi kết quả, chúng ta phải xóa mọi lần xuất hiện của chữ cái đó. 

Điều này thay đổi vấn đề từ "chúng ta nên xóa vị trí nào?" thành "loại ký tự nào chúng ta nên xóa hoàn toàn?". Nếu một ký tự xuất hiện 7 lần, việc xóa ký tự đó sẽ mất 7 lần xóa. Nếu nó xuất hiện một lần thì việc xóa nó chỉ tốn 1 lần xóa. 

Chiều dài chuỗi có thể đạt tới`10^5`, điều này ngay lập tức loại trừ bất kỳ số mũ hoặc bậc hai nào trong`n`. Chúng tôi không thể thử xóa tất cả các tập hợp con ký tự và chúng tôi cũng không thể liên tục xây dựng lại các chuỗi lớn trong các vòng lặp lồng nhau. MỘT`O(n log n)`hoặc`O(n)`giải pháp là mục tiêu phù hợp. 

Có một số trường hợp khó xử lý. 

Coi như:```
aaaaa
4
```Chúng ta được phép xóa tối đa 4 ký tự, nhưng xóa cả 5 ký tự sẽ vượt quá giới hạn. Câu trả lời vẫn là 1 ký tự riêng biệt, không phải 0. Việc triển khai bất cẩn có thể xóa càng nhiều càng tốt và vô tình xóa toàn bộ chuỗi. 

Bây giờ hãy xem xét:```
abc
10
```Ở đây chúng ta có thể xóa mọi ký tự vì`k`lớn hơn chiều dài chuỗi. Câu trả lời đúng là 0 và chuỗi kết quả trống. Một số giải pháp không chính xác buộc phải giữ lại ít nhất một ký tự. 

Một trường hợp tế nhị khác là:```
aabbbcccc
5
```Các tần số là`2, 3, 4`. Nếu chúng ta xóa`'a'`Và`'b'`, chúng ta thực hiện đúng 5 lần xóa và giảm câu trả lời xuống còn 1 ký tự riêng biệt. Một chiến lược tham lam ngây thơ xóa các nhóm lớn nhất trước tiên sẽ thất bại nặng nề ở đây, bởi vì việc loại bỏ`'c'`có giá 4 và để lại hai chữ cái riêng biệt thay vì một. 

Quan sát quan trọng là việc loại bỏ một loại ký tự có chi phí cố định bằng tần số của nó. Để giảm thiểu số lượng ký tự riêng biệt còn lại, trước tiên chúng ta nên loại bỏ những loại ký tự rẻ nhất. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử từng tập hợp con các ký tự riêng biệt và kiểm tra xem việc xóa tất cả các lần xuất hiện của các ký tự đó có tốn nhiều nhất không`k`. Nếu tập hợp con bảng chữ cái hợp lệ, chúng tôi đếm xem còn lại bao nhiêu chữ cái riêng biệt và giữ mức tối thiểu. 

Điều này hiệu quả vì sự lựa chọn về cơ bản là về loại nhân vật chứ không phải vị trí. Sau khi chúng tôi quyết định những chữ cái nào biến mất hoàn toàn, chuỗi con thu được sẽ được xác định duy nhất bằng cách giữ lại tất cả các ký tự khác. 

Vấn đề là số lượng tập hợp con. Mặc dù bảng chữ cái chỉ chứa các chữ cái tiếng Anh viết thường nhưng vẫn có`2^26`tập hợp con, có khoảng 67 triệu khả năng. Đó là quá lớn so với giới hạn 2 giây. 

Thuộc tính cấu trúc quan trọng là mọi loại ký tự đều có chi phí xóa độc lập. Đang xóa`'a'`không bao giờ ảnh hưởng đến chi phí loại bỏ`'b'`. Chúng tôi đang lựa chọn hiệu quả càng nhiều loại nhân vật có tần suất phù hợp với ngân sách càng tốt`k`. 

Điều này biến thành một vấn đề tham lam. Giả sử hai loại ký tự có tần số 2 và 5. Nếu chúng ta chỉ có đủ khả năng cho một trong số chúng, việc xóa ký tự tần số-2 ít nhất cũng tốt như xóa ký tự tần số-5, vì cả hai đều giảm số lượng khác biệt chính xác 1, nhưng một ký tự thì rẻ hơn. 

Quan sát đó quyết định hoàn toàn chiến lược: 

1. Đếm tần số. 
2. Sắp xếp các loại ký tự theo tần số. 
3. Trước tiên, hãy xóa các nhóm nhỏ nhất trong khi chúng tôi vẫn còn đủ số lần xóa. 
4. Xây dựng lại câu trả lời chỉ sử dụng các loại ký tự còn lại. 

Kích thước bảng chữ cái chỉ có 26, vì vậy tần số sắp xếp về cơ bản là thời gian không đổi. Công việc chủ yếu là quét chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^26 · 26 + n) | O(26) | Quá chậm | 
| Tối ưu | O(n + 26 log 26) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi`s`và số nguyên`k`. 
2. Đếm số lần mỗi ký tự xuất hiện trong chuỗi. 

Chúng ta cần những tần số này vì việc xóa một ký tự sẽ tốn chính xác tần số của nó. 
3. Lưu trữ tất cả`(frequency, character)`ghép đôi và sắp xếp chúng theo thứ tự tần số tăng dần. 

Các loại ký tự rẻ nhất nên được xem xét đầu tiên, bởi vì mỗi ký tự bị loại bỏ hoàn toàn sẽ làm giảm số lượng riêng biệt chính xác bằng 1. 
4. Duyệt qua danh sách đã sắp xếp. 

Nếu tần suất hiện tại nhỏ hơn hoặc bằng ngân sách xóa còn lại`k`, xóa toàn bộ loại ký tự đó: 

- trừ tần số của nó khỏi`k`- đánh dấu ký tự là đã bị xóa 

Nếu không thì dừng xóa ký tự. 

Khi chúng tôi không đủ khả năng cung cấp tần số nhỏ nhất hiện tại, chúng tôi cũng không thể cung cấp bất kỳ tần số nào sau này vì danh sách đã được sắp xếp. 
5. Xây dựng chuỗi cuối cùng bằng cách chỉ giữ lại các ký tự chưa bị xóa. 

Điều này sẽ tự động giữ nguyên thứ tự ban đầu, do đó kết quả là một dãy con hợp lệ. 
6. Đếm xem còn lại bao nhiêu ký tự khác biệt và in ra: 

- số ký tự riêng biệt còn lại 
- dãy kết quả 

### Tại sao nó hoạt động 

Mỗi loại ký tự bị xóa mang lại lợi ích chính xác như nhau, giảm số lượng ký tự khác biệt xuống 1. Sự khác biệt duy nhất giữa các lựa chọn là chi phí của chúng, bằng với tần suất của chúng. 

Giả sử một giải pháp tối ưu xóa một ký tự có tần số 5 nhưng giữ lại một ký tự khác có tần số 2. Việc hoán đổi các lựa chọn này không thể làm tăng tổng số lần xóa được sử dụng và vẫn xóa một ký tự riêng biệt. Việc lặp lại đối số trao đổi này sẽ biến đổi bất kỳ giải pháp tối ưu nào thành giải pháp luôn xóa các tần số nhỏ nhất trước tiên. 

Vì tính chất đó nên chiến lược tham lam là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
k = int(input())

freq = [0] * 26

for ch in s:
    freq[ord(ch) - ord('a')] += 1

pairs = []

for i in range(26):
    if freq[i] > 0:
        pairs.append((freq[i], chr(i + ord('a'))))

pairs.sort()

removed = set()

for cnt, ch in pairs:
    if cnt <= k:
        k -= cnt
        removed.add(ch)
    else:
        break

result = []

for ch in s:
    if ch not in removed:
        result.append(ch)

answer = ''.join(result)

print(len(set(answer)))
print(answer)
```Phần đầu tiên đếm tần số bằng cách sử dụng một mảng có kích thước cố định có độ dài 26. Việc này nhanh hơn và đơn giản hơn so với việc sử dụng từ điển vì bảng chữ cái đã được biết trước. 

các`(frequency, character)`các cặp được sắp xếp sao cho loại ký tự rẻ nhất xuất hiện đầu tiên. Vòng lặp tham lam sẽ loại bỏ toàn bộ nhóm ký tự trong khi ngân sách cho phép. 

Điều kiện dừng là quan trọng. Một khi tần số quá lớn thì mọi tần số sau đó cũng quá lớn vì mảng đã được sắp xếp. Tiếp tục vòng lặp sẽ không đạt được gì. 

Bước xây dựng lại cuối cùng sẽ quét chuỗi gốc và bỏ qua các ký tự đã bị xóa. Điều này tự động bảo toàn thứ tự tương đối, do đó kết quả vẫn là một dãy con. 

Một điểm tinh tế là trường hợp chuỗi trống. Nếu mọi loại ký tự bị loại bỏ,`answer`trở thành`""`, Và`len(set(answer))`đánh giá đúng để`0`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
aaaaa
4
```Tần số: 

| Nhân vật | Tần số | 
| --- | --- | 
| một | 5 | 

Các cặp được sắp xếp: 

| Bước | Cặp hiện tại | k Trước | Có thể loại bỏ? | k Sau | Đã xóa | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (5, a) | 4 | Không | 4 | {} | 

Việc xây dựng lại chuỗi giữ tất cả các ký tự: 

| Vị trí | Nhân vật | LOẠI BỎ? | Kết quả | 
| --- | --- | --- | --- | 
| 1 | một | Không | một | 
| 2 | một | Không | aa | 
| 3 | một | Không | aaa | 
| 4 | một | Không | aaa | 
| 5 | một | Không | aaa | 

Câu trả lời cuối cùng:```
1
aaaaa
```Ví dụ này cho thấy tại sao chúng ta không thể luôn xóa mọi thứ có thể. Loại ký tự duy nhất tốn 5 lần xóa, nhưng ngân sách chỉ có 4. 

### Ví dụ 2 

đầu vào:```
aaaabbb
4
```Tần số: 

| Nhân vật | Tần số | 
| --- | --- | 
| một | 4 | 
| b | 3 | 

Các cặp được sắp xếp: 

| Bước | Cặp hiện tại | k Trước | Có thể loại bỏ? | k Sau | Đã xóa | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (3, b) | 4 | Có | 1 | {b} | 
| 2 | (4, a) | 1 | Không | 1 | {b} | 

Xây dựng lại chuỗi: 

| Vị trí | Nhân vật | LOẠI BỎ? | Kết quả | 
| --- | --- | --- | --- | 
| 1 | một | Không | một | 
| 2 | một | Không | aa | 
| 3 | một | Không | aaa | 
| 4 | một | Không | aaa | 
| 5 | b | Có | aaa | 
| 6 | b | Có | aaa | 
| 7 | b | Có | aaa | 

Câu trả lời cuối cùng:```
1
aaaa
```Dấu vết này thể hiện rõ ràng nguyên tắc tham lam. Việc loại bỏ nhóm ký tự rẻ hơn trước tiên sẽ giúp giảm các chữ cái riêng biệt tốt nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + 26 log 26) | Đếm và xây dựng lại quét chuỗi một lần, sắp xếp tối đa 26 tần số | 
| Không gian | O(26) | Lưu trữ tần số và theo dõi ký tự bị xóa | 

Vì kích thước bảng chữ cái là cố định nên chi phí sắp xếp là không đổi. Giải pháp chủ yếu là quét tuyến tính trên chuỗi, dễ dàng phù hợp với giới hạn cho`n = 10^5`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    s = input().strip()
    k = int(input())

    freq = [0] * 26

    for ch in s:
        freq[ord(ch) - ord('a')] += 1

    pairs = []

    for i in range(26):
        if freq[i] > 0:
            pairs.append((freq[i], chr(i + ord('a'))))

    pairs.sort()

    removed = set()

    for cnt, ch in pairs:
        if cnt <= k:
            k -= cnt
            removed.add(ch)
        else:
            break

    ans = ''.join(ch for ch in s if ch not in removed)

    print(len(set(ans)))
    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("aaaaa\n4\n") == "1\naaaaa\n", "sample 1"

# delete one entire character group
assert run("aaaabbb\n4\n") == "1\naaaa\n", "remove b"

# delete everything
assert run("abc\n10\n") == "0\n\n", "empty result"

# minimum size input
assert run("a\n0\n") == "1\na\n", "single character"

# exact budget match
assert run("aabbbcccc\n5\n") == "1\ncccc\n", "remove a and b"

# no deletions allowed
assert run("abac\n0\n") == "3\nabac\n", "unchanged string"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a / 0`|`a`còn lại | Đầu vào kích thước tối thiểu | 
|`abc / 10`| Chuỗi trống | Trường hợp xóa toàn bộ | 
|`aaaabbb / 4`| Chỉ giữ lại`a`| Tham lam loại bỏ tần số nhỏ hơn | 
|`aabbbcccc / 5`| Chỉ giữ lại`cccc`| Tiêu thụ ngân sách chính xác | 
|`abac / 0`| Chuỗi gốc | Ranh giới không xóa | 

## Vỏ cạnh 

Hãy xem xét trường hợp không thể xóa tất cả các ký tự:```
aaaaa
4
```Danh sách tần số chỉ chứa`(5, 'a')`. Từ`5 > 4`, vòng lặp tham lam không loại bỏ gì cả. Bước tái cấu trúc giữ lại mọi ký tự, tạo ra`"aaaaa"`bằng 1 chữ cái rõ ràng. Điều này xác nhận thuật toán không bao giờ vượt quá ngân sách xóa. 

Bây giờ hãy xem xét thái cực ngược lại:```
abc
10
```Tần số được sắp xếp là`(1, 'a')`,`(1, 'b')`,`(1, 'c')`. 

Thuật toán loại bỏ cả ba nhóm: 

| Nhân vật | Chi phí | Còn lại k | 
| --- | --- | --- | 
| một | 1 | 9 | 
| b | 1 | 8 | 
| c | 1 | 7 | 

Chuỗi được xây dựng lại trống rỗng. Số lượng riêng biệt trở thành 0, giá trị này hợp lệ vì việc xóa tất cả các ký tự chỉ tốn 3. 

Một tình huống tinh tế khác là khi một số loại ký tự có cùng tần số:```
aabbcc
2
```Tất cả tần số là 2. Thuật toán có thể loại bỏ bất kỳ tần số nào trong số chúng, tùy thuộc vào thứ tự sắp xếp. Nếu nó loại bỏ`'a'`, kết quả trở thành`"bbcc"`bằng 2 chữ cái khác nhau. 

Điều này vẫn là tối ưu vì mọi lựa chọn xóa đều có chi phí như nhau. Bài toán chấp nhận bất kỳ câu trả lời hợp lệ nào, vì thế các ràng buộc không thành vấn đề.
