---
title: "CF 1569A - Chuỗi con cân bằng"
description: "Chúng ta được cấp một chuỗi nhị phân chỉ gồm các ký tự a và b. Đối với mỗi trường hợp thử nghiệm, chúng ta phải tìm bất kỳ phân đoạn liền kề nào có số ký tự a chính xác bằng số ký tự b của nó."
date: "2026-06-10T11:36:40+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1569
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 113 (Rated for Div. 2)"
rating: 800
weight: 1569
solve_time_s: 167
verified: false
draft: false
---

[CF 1569A - Chuỗi con cân bằng](https://codeforces.com/problemset/problem/1569/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 47s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân chỉ bao gồm các ký tự`a`Và`b`. Đối với mỗi trường hợp thử nghiệm, chúng ta phải tìm bất kỳ phân đoạn liền kề nào có số lượng`a`ký tự chính xác bằng số lượng ký tự của nó`b`nhân vật. 

Đầu ra không yêu cầu chuỗi con cân bằng dài nhất hoặc số lượng chuỗi con cân bằng. Bất kỳ câu trả lời hợp lệ đều được chấp nhận. Nếu không có chuỗi con cân bằng tồn tại, chúng tôi in`-1 -1`. 

Những hạn chế là cực kỳ nhỏ. Mỗi chuỗi có độ dài tối đa là 50 và có thể có tối đa 1000 trường hợp kiểm thử. Ngay cả một thuật toán kiểm tra mọi chuỗi con có thể cũng sẽ phù hợp thoải mái trong giới hạn thời gian vì chỉ có khoảng 2500 chuỗi con trong một chuỗi có độ dài 50. 

Phần thú vị của vấn đề là tồn tại một quan sát đơn giản hơn nhiều. 

Hãy xem xét một chuỗi có độ dài 1.```
a
```Không có chuỗi con không trống nào có thể được cân bằng vì mỗi chuỗi con chỉ chứa một ký tự. Câu trả lời đúng là`-1 -1`. 

Một lỗi phổ biến là cho rằng mọi chuỗi đều chứa một chuỗi con cân bằng có độ dài ít nhất là 2. 

Hãy xem xét một chuỗi trong đó tất cả các ký tự đều giống hệt nhau.```
aaaaa
```Mỗi chuỗi con chỉ chứa`a`, vì vậy không ai có thể có số lượng bằng nhau`a`Và`b`. Câu trả lời đúng là`-1 -1`. 

Việc triển khai bất cẩn có thể chỉ tìm kiếm các chuỗi con có độ dài chẵn và cho rằng một chuỗi con phải tồn tại một cách không chính xác. 

Một trường hợp tinh tế khác là một chuỗi như:```
aba
```Toàn bộ chuỗi không cân bằng vì nó chứa hai`a`và một`b`. Tuy nhiên, chuỗi con`"ab"`được cân bằng. Vì bài toán chấp nhận bất kỳ chuỗi con cân bằng nào nên chúng ta nên dừng lại ngay khi tìm thấy một chuỗi con cân bằng. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Liệt kê từng cặp`(l, r)`, đếm số lượng`a`Và`b`bên trong chuỗi con đó và kiểm tra xem số đếm có khớp không. Vì có`O(n²)`chuỗi con và đếm các ký tự một cách ngây thơ`O(n)`thời gian, tổng độ phức tạp là`O(n³)`. 

Với`n ≤ 50`, thậm chí`50³ = 125000`hoạt động cho mỗi trường hợp thử nghiệm là hoàn toàn có thể quản lý được. Vì vậy, vũ lực sẽ được chấp nhận. 

Bí quyết thực sự đến từ việc xem xét chuỗi con cân bằng nhỏ nhất có thể. 

Một chuỗi con cân bằng phải chứa cùng số lượng`a`Và`b`. Chuỗi con cân bằng không trống ngắn nhất có độ dài 2 và các dạng duy nhất có thể có của nó là:```
ab
ba
```Bây giờ giả sử một chuỗi con cân bằng tồn tại ở đâu đó trong chuỗi. Chúng ta hãy xem xét hai ký tự đầu tiên khác nhau của nó. Vì chuỗi chỉ chứa`a`Và`b`, bất kỳ cặp không bằng nhau liền kề nào cũng là`"ab"`hoặc`"ba"`, đã được cân bằng rồi. 

Điều này có nghĩa là chúng ta không bao giờ cần tìm kiếm những chuỗi con dài. Chúng ta chỉ cần tìm hai ký tự lân cận khác nhau. 

Nếu một cặp như vậy tồn tại ở vị trí`i`Và`i+1`, sau đó là chuỗi con`[i, i+1]`được cân bằng và có thể được trả lại ngay lập tức. 

Nếu mọi cặp liền kề đều bằng nhau thì toàn bộ chuỗi chỉ bao gồm một ký tự lặp lại. Trong tình huống đó không có chuỗi con cân bằng nào có thể tồn tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi. 
2. Quét tất cả các cặp ký tự liền kề. 
3. Đối với từng vị trí`i`, kiểm tra xem`s[i] != s[i + 1]`. 
4. Nếu tìm thấy cặp như vậy, xuất ra`i + 1`Và`i + 2`sử dụng chỉ mục dựa trên 1. 
5. Dừng ngay việc xử lý test case hiện tại vì`"ab"`Và`"ba"`đều là chuỗi con cân bằng. 
6. Nếu quá trình quét kết thúc mà không tìm thấy bất kỳ ký tự liền kề nào khác, hãy xuất ra`-1 -1`. 

Lý do bước 6 đúng là vì chuỗi nhị phân không có ký tự khác nhau liền kề phải hoàn toàn bao gồm cùng một ký tự. Một chuỗi như vậy không thể chứa số lượng bằng nhau`a`Và`b`trong bất kỳ chuỗi con không trống nào. 

### Tại sao nó hoạt động 

Thuộc tính quan trọng là mọi cặp không bằng nhau liền kề tạo thành một chuỗi con cân bằng có độ dài 2. 

Nếu thuật toán trả về vị trí`(i, i+1)`, chuỗi con là`"ab"`hoặc`"ba"`. Cả hai đều chứa một`a`và một`b`, vậy câu trả lời là hợp lệ. 

Nếu thuật toán không tìm thấy cặp không bằng nhau liền kề thì mọi ký tự trong chuỗi đều giống hệt nhau. Mọi chuỗi con không trống cũng bao gồm các ký tự giống nhau nên không thể chứa cả hai`a`Và`b`. Do đó không tồn tại chuỗi con cân bằng. 

Thuật toán không bao giờ có thể xuất ra chuỗi con không hợp lệ và không bao giờ có thể bỏ sót chuỗi con cân bằng hiện có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    s = input().strip()

    found = False

    for i in range(n - 1):
        if s[i] != s[i + 1]:
            print(i + 1, i + 2)
            found = True
            break

    if not found:
        print(-1, -1)
```Việc thực hiện theo sau sự quan sát trực tiếp. 

Vòng lặp kiểm tra từng cặp liền kề một lần. Khi hai ký tự lân cận khác nhau, chuỗi con có độ dài 2 tương ứng được cân bằng nên câu trả lời sẽ được in ngay lập tức. 

Các chỉ số bên trong chuỗi dựa trên 0, nhưng vấn đề mong đợi các vị trí dựa trên 1. Đó là lý do tại sao đầu ra sử dụng`i + 1`Và`i + 2`. 

Biến`found`theo dõi xem chuỗi con hợp lệ đã được báo cáo chưa. If the loop finishes without setting it to`True`, chuỗi chỉ chứa một ký tự lặp lại và câu trả lời phải là`-1 -1`. 

Không có lo ngại về tràn vì thuật toán chỉ thực hiện các thao tác lập chỉ mục đơn giản. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chuỗi đầu vào:```
abbaba
```| tôi | s[i] | s[i+1] | Khác biệt? | Hành động | 
| --- | --- | --- | --- | --- | 
| 0 | một | b | Có | Đầu ra 1 2 | 

Cặp liền kề đầu tiên là`"ab"`, trong đó có chứa một`a`và một`b`. Thuật toán trả về ngay vị trí`(1, 2)`. 

Ví dụ này cho thấy chúng ta không cần tìm kiếm các chuỗi con cân bằng dài hơn. Cặp hợp lệ đầu tiên là đủ. 

### Ví dụ 2 

Chuỗi đầu vào:```
aaaaa
```| tôi | s[i] | s[i+1] | Khác biệt? | Hành động | 
| --- | --- | --- | --- | --- | 
| 0 | một | một | Không | Tiếp tục | 
| 1 | một | một | Không | Tiếp tục | 
| 2 | một | một | Không | Tiếp tục | 
| 3 | một | một | Không | Tiếp tục | 

Không tìm thấy cặp liền kề khác nhau. 

Đầu ra:```
-1 -1
```Dấu vết này thể hiện trường hợp toàn bộ chuỗi được tạo thành từ một ký tự. Vì mọi chuỗi con cũng chỉ chứa`a`, không có chuỗi con cân bằng nào tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần quét qua chuỗi | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng | 

Với`n ≤ 50`, thuật toán thực hiện tối đa 49 phép so sánh cho mỗi trường hợp thử nghiệm. Ngay cả với 1000 trường hợp thử nghiệm, tổng công việc vẫn rất nhỏ so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        s = input().strip()

        found = False

        for i in range(n - 1):
            if s[i] != s[i + 1]:
                out.append(f"{i + 1} {i + 2}")
                found = True
                break

        if not found:
            out.append("-1 -1")

    return "\n".join(out)

# provided sample
assert run(
"""4
1
a
6
abbaba
6
abbaba
9
babbabbaa
"""
) == """-1 -1
1 2
1 2
1 2""", "sample"

# minimum size, no answer
assert run(
"""1
1
b
"""
) == "-1 -1", "single character"

# smallest balanced substring
assert run(
"""1
2
ab
"""
) == "1 2", "length two balanced"

# all equal characters
assert run(
"""1
5
aaaaa
"""
) == "-1 -1", "all a"

# difference appears at the end
assert run(
"""1
5
aaaab
"""
) == "4 5", "boundary pair"

# maximum length style case
assert run(
"""1
50
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaab
"""
) == "49 50", "near maximum length"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`b`|`-1 -1`| Chuỗi có độ dài tối thiểu | 
|`ab`|`1 2`| Chuỗi con cân bằng nhỏ nhất có thể | 
|`aaaaa`|`-1 -1`| Tất cả các ký tự giống hệt nhau | 
|`aaaab`|`4 5`| Cặp khác nhau liền kề ở cuối | 
|`a...ab`(dài 50) |`49 50`| Xử lý ranh giới gần kích thước tối đa | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1
a
```Vòng lặp trên các cặp ký tự liền kề không bao giờ được thực thi vì không có cặp ký tự lân cận nào. Thuật toán đi đến cuối và in`-1 -1`. Điều này đúng vì một ký tự đơn không thể chứa số lượng bằng nhau`a`Và`b`. 

Hãy xem xét đầu vào:```
5
aaaaa
```Quá trình quét kiểm tra`(a,a)`bốn lần. Mọi so sánh đều bình đẳng nên không tìm được câu trả lời. Thuật toán in`-1 -1`. Mỗi chuỗi con chỉ chứa`a`, do đó không có chuỗi con cân bằng nào tồn tại. 

Hãy xem xét đầu vào:```
3
aba
```Tại`i = 0`, thuật toán so sánh`a`Và`b`. Chúng khác nhau, vì vậy nó xuất ra`1 2`. Chuỗi con`"ab"`chứa một`a`và một`b`, làm cho nó cân bằng. Việc toàn bộ chuỗi không cân bằng không thành vấn đề vì mọi chuỗi con cân bằng hợp lệ đều được chấp nhận. 

Hãy xem xét đầu vào:```
5
bbbba
```Ba cặp liền kề đầu tiên là`(b,b)`. Tại`i = 3`, cặp đó là`(b,a)`, khác nhau. Đầu ra của thuật toán`4 5`. Chuỗi con`"ba"`cân bằng nên đáp án đúng. Trường hợp này xác nhận rằng các cặp ở gần cuối chuỗi được xử lý chính xác.
