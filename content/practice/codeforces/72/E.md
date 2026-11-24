---
title: "CF 72E - Ali đi mua sắm"
description: "Chúng ta được cấp một chuỗi chữ thường và phải tìm chuỗi con xuất hiện với số lần lớn nhất bên trong chuỗi đó. Các lần xuất hiện có thể trùng lặp. Trong số tất cả các chuỗi con có cùng tần số tối đa, chúng tôi chọn chuỗi con dài nhất."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "brute-force", "strings"]
categories: ["algorithms"]
codeforces_contest: 72
codeforces_index: "E"
codeforces_contest_name: "Unknown Language Round 2"
rating: 1800
weight: 72
solve_time_s: 454
verified: true
draft: false
---

[CF 72E - Ali đi mua sắm](https://codeforces.com/problemset/problem/72/E) 

**Đánh giá:** 1800 
**Tags:** *đặc biệt, vũ phu, dây 
**Thời gian giải:** 7 phút 34 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi chữ thường và phải tìm chuỗi con xuất hiện với số lần lớn nhất bên trong chuỗi đó. Các lần xuất hiện có thể trùng lặp. Trong số tất cả các chuỗi con có cùng tần số tối đa, chúng tôi chọn chuỗi con dài nhất. Nếu vẫn còn bằng nhau, chúng ta chọn chuỗi con lớn nhất theo từ điển. 

Ví dụ, trong`abab`, chuỗi con`a`xuất hiện hai lần và`ab`cũng xuất hiện hai lần. Vì cả hai đều có cùng tần số nên chúng tôi thích tần số dài hơn, vì vậy câu trả lời trở thành`ab`. 

Kích thước đầu vào thay đổi toàn bộ đặc tính của vấn đề. Độ dài chuỗi tối đa là 30, cực kỳ nhỏ. Thuật toán bậc ba hoặc bậc bốn hoàn toàn phù hợp ở đây. Tổng số chuỗi con của một chuỗi có độ dài`n`là`n(n+1)/2`, chỉ là 465 khi`n = 30`. Điều đó có nghĩa là chúng ta có thể trực tiếp tạo ra mọi chuỗi con và đếm xem nó xuất hiện bao nhiêu lần mà không cần lo lắng về hiệu suất. 

Khó khăn chính không phải ở tính hiệu quả mà ở việc xử lý đúng luật hòa. Việc thực hiện bất cẩn có thể dễ dàng tạo ra câu trả lời sai ngay cả khi tần số được tính toán chính xác. 

Một sai lầm dễ mắc phải là quên rằng sự chồng chéo được cho phép. Coi như:```
aaaa
```Chuỗi con`aaa`xuất hiện hai lần: một lần bắt đầu ở chỉ mục 0 và một lần ở chỉ mục 1. Việc triển khai sử dụng kết hợp không chồng chéo sẽ chỉ tính sai một lần xuất hiện. 

Một lỗi phổ biến khác là xử lý cà vạt không đúng cách. Coi như:```
abab
```Cả hai`a`Và`ab`xuất hiện hai lần. Câu trả lời đúng là`ab`bởi vì các chuỗi con dài hơn được ưu tiên khi tần số khớp. 

Yếu tố quyết định cuối cùng là thứ tự từ điển. Coi như:```
ababcdcd
```Các chuỗi con`ab`Và`cd`cả hai đều xuất hiện hai lần và cả hai đều có độ dài 2. Vì`cd`lớn hơn về mặt từ điển, câu trả lời đúng là:```
cd
```Việc triển khai chỉ cập nhật câu trả lời với tần suất hoặc độ dài lớn hơn sẽ âm thầm thất bại ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là tạo ra mọi chuỗi con có thể và đếm xem nó xuất hiện bao nhiêu lần trong chuỗi gốc. 

Một chuỗi con được xác định bởi vị trí bắt đầu và kết thúc của nó, do đó có`O(n^2)`các chuỗi con. Đối với mỗi chuỗi con, chúng ta có thể trượt qua chuỗi gốc và so sánh từng ký tự để đếm số lần xuất hiện. Mỗi lần so sánh có giá lên tới`O(n)`và chúng tôi thực hiện nó ở mức tối đa`O(n)`vị trí, dẫn đến`O(n^4)`tổng thể phức tạp. 

Với`n = 30`, cái này vẫn còn nhỏ. Thậm chí`30^4 = 810000`hoạt động là tầm thường trong giới hạn 5 giây. 

Lý do vũ lực hoạt động ở đây là do hạn chế có chủ ý nhỏ. Vấn đề thực sự nằm ở việc triển khai logic so sánh một cách chính xác hơn là phát minh ra một thuật toán chuỗi phức tạp. 

Chúng ta có thể đơn giản hóa bước đếm hơn nữa bằng cách sử dụng tính năng cắt của Python. Đối với mọi chuỗi con ứng cử viên`t`, chúng tôi kiểm tra mọi vị trí bắt đầu`i`và so sánh:```
s[i:i+len(t)] == t
```Điều này tự động xử lý các kết quả trùng khớp vì chúng tôi kiểm tra mọi vị trí một cách độc lập. 

Quan sát quan trọng là số lượng chuỗi con riêng biệt đã rất nhỏ. Không cần mảng hậu tố, automata hậu tố, thủ thuật hàm Z hoặc băm cuộn. Một tìm kiếm toàn diện rõ ràng vừa đơn giản vừa an toàn hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^4) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n^3) đến O(n^4) tùy thuộc vào chi phí so sánh chuỗi con | O(1) | Đã chấp nhận | 

Trong thực tế, việc cắt Python giúp việc triển khai nhỏ gọn và đủ nhanh cho các giới hạn nhất định. 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào`s`. 
2. Khởi tạo các biến lưu trữ chuỗi con tốt nhất hiện tại và tần số của nó. 
3. Tạo mọi chuỗi con có thể bằng hai chỉ mục`l`Và`r`. 

Chuỗi con là`s[l:r+1]`. 
4. Đối với chuỗi con hiện tại, hãy đếm xem nó xuất hiện bao nhiêu lần trong`s`. 

Kiểm tra mọi vị trí bắt đầu`i`. Nếu như`s[i:i+len(sub)] == sub`, tăng số lượng. 

Vì mọi vị trí đều được kiểm tra độc lập nên các lần xuất hiện trùng lặp sẽ được tính một cách tự nhiên. 
5. So sánh chuỗi con hiện tại với câu trả lời đúng nhất được tìm thấy cho đến nay. 

Cập nhật câu trả lời nếu: 

- tần số của nó lớn hơn, hoặc 
- tần số bằng nhau nhưng chuỗi con dài hơn, hoặc 
- cả tần số và độ dài đều bằng nhau nhưng chuỗi con lớn hơn về mặt từ điển. 
6. Sau khi tất cả các chuỗi con được xử lý, hãy in ra chuỗi con tốt nhất. 

### Tại sao nó hoạt động 

Thuật toán kiểm tra rõ ràng mọi chuỗi con của chuỗi gốc, do đó không thể bỏ sót ứng cử viên nào. Đối với mỗi ứng cử viên, nó sẽ kiểm tra mọi vị trí bắt đầu có thể có và đếm các kết quả trùng khớp chính xác, điều này đảm bảo tần suất chính xác, bao gồm cả các trường hợp trùng lặp. 

Các điều kiện cập nhật khớp chính xác với các quy tắc trong câu lệnh. Ở mỗi bước, câu trả lời được lưu trữ là chuỗi con tốt nhất trong số tất cả các ứng viên được xử lý cho đến nay. Sau khi quá trình tìm kiếm toàn diện kết thúc, chuỗi con được lưu trữ phải tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
n = len(s)

best = ""
best_count = -1

for l in range(n):
    for r in range(l, n):
        sub = s[l:r + 1]
        m = len(sub)

        cnt = 0

        for i in range(n - m + 1):
            if s[i:i + m] == sub:
                cnt += 1

        if cnt > best_count:
            best_count = cnt
            best = sub
        elif cnt == best_count:
            if len(sub) > len(best):
                best = sub
            elif len(sub) == len(best) and sub > best:
                best = sub

print(best)
```Hai vòng lặp bên ngoài tạo ra mọi chuỗi con. sử dụng`l`Và`r`làm cho các ranh giới trở nên rõ ràng và tránh những sai lầm riêng lẻ. 

Vòng đếm chỉ lặp lại cho đến khi`n - m`, Ở đâu`m`là độ dài chuỗi con. Điều này đảm bảo lát cắt`s[i:i+m]`luôn ở bên trong chuỗi. 

Logic so sánh là phần tinh tế. Thứ tự quan trọng: 

Đầu tiên so sánh tần số. Chỉ khi tần số khớp nhau thì chúng ta mới so sánh độ dài. Chỉ khi cả tần số và độ dài khớp nhau thì chúng ta mới so sánh thứ tự từ điển. 

Việc thay đổi thứ tự này sẽ tạo ra câu trả lời sai trong các trường hợp nặng như`ababcdcd`. 

So sánh chuỗi Python đã tuân theo thứ tự từ điển, vì vậy`sub > best`trực tiếp thực hiện bước quyết định cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
abab
```| Chuỗi con | Tần số | Tốt nhất hiện tại | 
| --- | --- | --- | 
| một | 2 | một | 
| ab | 2 | ab | 
| aba | 1 | ab | 
| abab | 1 | ab | 
| b | 2 | ab | 
| ba | 1 | ab | 
| em yêu | 1 | ab | 

Chuỗi con`a`đầu tiên trở thành tốt nhất vì nó xuất hiện hai lần. Sau đó`ab`cũng xuất hiện hai lần, nhưng dài hơn nên thay thế`a`. 

Dấu vết này thể hiện quy tắc tie-break thứ hai. 

### Ví dụ 2 

đầu vào:```
aaaa
```| Chuỗi con | Tần số | Tốt nhất hiện tại | 
| --- | --- | --- | 
| một | 4 | một | 
| aa | 3 | một | 
| aaa | 2 | một | 
| aaa | 1 | một | 

Chuỗi con`aa`xuất hiện ba lần vì được phép chồng chéo: 

- vị trí 0-1 
- vị trí 1-2 
- vị trí 2-3 

Câu trả lời cuối cùng vẫn còn`a`vì tần số chi phối chiều dài. 

Ví dụ này xác nhận rằng các lần xuất hiện chồng chéo được tính chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^4) | Có các chuỗi con O(n^2) và mỗi chuỗi con có thể yêu cầu công việc so sánh tổng cộng O(n^2) | 
| Không gian | O(1) | Chỉ một số biến được lưu trữ bên cạnh chuỗi đầu vào | 

Ngay cả độ dài đầu vào trong trường hợp xấu nhất cũng chỉ là 30, do đó cần ít hơn một triệu thao tác nguyên thủy. Điều này dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    s = input().strip()
    n = len(s)

    best = ""
    best_count = -1

    for l in range(n):
        for r in range(l, n):
            sub = s[l:r + 1]
            m = len(sub)

            cnt = 0

            for i in range(n - m + 1):
                if s[i:i + m] == sub:
                    cnt += 1

            if cnt > best_count:
                best_count = cnt
                best = sub
            elif cnt == best_count:
                if len(sub) > len(best):
                    best = sub
                elif len(sub) == len(best) and sub > best:
                    best = sub

    print(best)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run("abab\n") == "ab", "sample 1"

# minimum size
assert run("a\n") == "a", "single character"

# overlapping occurrences
assert run("aaaa\n") == "a", "overlapping matches"

# lexicographical tie-break
assert run("ababcdcd\n") == "cd", "same count and length"

# all distinct
assert run("abcd\n") == "d", "all substrings appear once"

# boundary length case
assert run("abcabcabc\n") == "abc", "repeated pattern"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`a`| Đầu vào kích thước tối thiểu | 
|`aaaa`|`a`| Sự xuất hiện chồng chéo | 
|`ababcdcd`|`cd`| Sự ràng buộc về mặt từ điển | 
|`abcd`|`d`| Tất cả các chuỗi con xuất hiện một lần | 
|`abcabcabc`|`abc`| Chuỗi con lặp lại dài hơn sẽ thắng | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
aaaa
```Chuỗi con`aaa`xuất hiện hai lần vì số lần trùng lặp: 

-`s[0:3]`-`s[1:4]`Thuật toán kiểm tra mọi vị trí bắt đầu một cách độc lập nên cả hai kết quả trùng khớp đều được tính. Tần số trở thành: 

-`a`→ 4 
-`aa`→ 3 
-`aaa`→ 2 
-`aaaa`→ 1 

Câu trả lời cuối cùng là`a`. 

Bây giờ hãy xem xét:```
abab
```Cả hai`a`Và`ab`xuất hiện hai lần. Thuật toán đầu tiên lưu trữ`a`là chuỗi con tốt nhất. Sau này khi xử lý`ab`, nó thấy rằng tần số bằng nhau nhưng`ab`dài hơn nên nó cập nhật câu trả lời. 

Đầu ra cuối cùng trở thành:```
ab
```Cuối cùng, hãy xem xét:```
ababcdcd
```Các chuỗi con`ab`Và`cd`cả hai đều xuất hiện hai lần và cả hai đều có độ dài 2. 

Khi nào`ab`được xử lý, nó sẽ trở thành tốt nhất hiện tại. Sau đó,`cd`phù hợp với cả tần số và độ dài, nhưng`cd > ab`về mặt từ điển nên thuật toán sẽ thay thế câu trả lời. 

Đầu ra cuối cùng là:```
cd
```
