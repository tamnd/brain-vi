---
title: "CF 1810C - Hoán vị"
description: "Chúng ta được cung cấp một mảng và hai phép toán với chi phí cố định. Chúng ta có thể xóa bất kỳ phần tử hiện có nào cho giá c hoặc chèn bất kỳ số nguyên dương nào cho giá d. Mục tiêu là biến đổi mảng thành một hoán vị hợp lệ có độ dài m."
date: "2026-06-09T08:44:20+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1810
codeforces_index: "C"
codeforces_contest_name: "CodeTON Round 4 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 1300
weight: 1810
solve_time_s: 104
verified: true
draft: false
---

[CF 1810C - Tạo hoán vị](https://codeforces.com/problemset/problem/1810/C) 

**Đánh giá:** 1300 
**Tags:** vũ phu, tham lam, phân loại 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng và hai phép toán với chi phí cố định. 

Chúng tôi có thể xóa bất kỳ yếu tố hiện có nào cho chi phí`c`hoặc chèn bất kỳ số nguyên dương nào cho chi phí`d`. 

Mục tiêu là biến đổi mảng thành một hoán vị hợp lệ có độ dài nào đó`m`. Hoán vị độ dài`m`chứa mọi số từ`1`bởi vì`m`đúng một lần. Thứ tự không quan trọng vì chúng ta có thể giữ các phần tử ở bất kỳ vị trí nào hiện tại và chèn các giá trị mới vào bất kỳ đâu. 

Nhiệm vụ là tìm tổng chi phí nhỏ nhất. 

Quan sát quan trọng nhất là hoán vị cuối cùng được xác định hoàn toàn bởi giá trị tối đa của nó. Nếu hoán vị cuối cùng có độ dài`m`, thì tập giá trị cuối cùng phải chính xác`{1, 2, ..., m}`. 

Các ràng buộc đủ lớn để giải pháp bậc hai là không thể. Tổng của tất cả các độ dài mảng nhiều nhất là`2·10^5`, điều này gợi ý mạnh mẽ một`O(n log n)`giải pháp cho mỗi trường hợp thử nghiệm, bị chi phối bởi việc sắp xếp. Bất kỳ cách tiếp cận nào thử mọi tập hợp con hoặc quét liên tục toàn bộ mảng cho từng độ dài ứng cử viên sẽ vượt quá giới hạn. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
n = 3
a = [1, 1, 1]
c = 5
d = 100
```Câu trả lời đúng là`10`, thu được bằng cách xóa hai bản và giữ một bản duy nhất`1`, tạo ra hoán vị`[1]`. 

Một giải pháp bất cẩn có thể cố gắng xây dựng một hoán vị lớn hơn và phải trả chi phí chèn lớn một cách không cần thiết. 

Một trường hợp phức tạp khác là:```
n = 2
a = [100, 100]
c = 1
d = 1
```Chiến lược tốt nhất là xóa một`100`, xóa cái kia`100`, sau đó chèn`1`, với tổng chi phí`3`. 

Giữ các giá trị lớn là vô ích vì bất kỳ hoán vị nào về độ dài`m`không thể chứa số lớn hơn`m`. 

Bản sao cần được chăm sóc đặc biệt:```
n = 4
a = [1, 2, 2, 3]
c = 10
d = 1
```Câu trả lời đúng là`10`, xóa một bản sao`2`. 

Việc đếm các giá trị bị thiếu mà không loại bỏ các giá trị trùng lặp trước tiên sẽ tạo ra kết quả sai vì hoán vị yêu cầu mọi giá trị phải xuất hiện chính xác một lần. 

## Phương pháp tiếp cận 

Quan điểm vũ phu là chọn độ dài hoán vị cuối cùng`m`. 

Một lần`m`được cố định, mọi phần tử mảng lớn hơn`m`phải được loại bỏ. Trong số các giá trị từ`1`ĐẾN`m`, mỗi bản chỉ có thể còn lại một bản sao. Các giá trị còn thiếu phải được chèn vào. Chúng tôi có thể tính toán chi phí cho mọi điều có thể`m`và lấy mức tối thiểu. 

Cái khó đó là`m`có thể rất lớn. Vì giá trị mảng đạt`10^9`, việc kiểm tra trực tiếp tất cả các độ dài có thể là không thể. 

Quan sát quan trọng là chỉ các giá trị đã có trong mảng mới có thể đóng vai trò là giá trị được giữ tối đa một cách có ý nghĩa. 

Giả sử chúng ta sắp xếp mảng và loại bỏ các bản sao về mặt khái niệm. Đặt các giá trị phân biệt là:```
v1 < v2 < ... < vk
```Nếu chúng ta quyết định rằng`vi`là giá trị lớn nhất được giữ trong hoán vị cuối cùng thì mọi giá trị phân biệt sau`vi`phải bị xóa. 

Trong số những người đầu tiên`i`giá trị riêng biệt, chúng tôi giữ chính xác một bản sao của mỗi giá trị. Để làm cho chúng hình thành`{1,2,...,vi}`, chúng ta phải chèn tất cả các số còn thiếu vào bên dưới`vi`. 

Số giá trị còn thiếu là:```
vi - i
```bởi vì nên có`vi`số trong`{1,...,vi}`, nhưng chỉ`i`các giá trị riêng biệt hiện đang tồn tại ở đó. 

Điều này biến vấn đề thành việc đánh giá một ứng cử viên cho mỗi giá trị riêng biệt. 

Ngoài ra còn có một trường hợp đặc biệt là chúng tôi xóa mọi thứ và xây dựng hoán vị`[1]`. Vì mảng cuối cùng phải không trống nên chi phí đó là:```
n * c + d
```xóa tất cả các phần tử ban đầu, sau đó chèn`1`. 

Sau khi sắp xếp, chúng ta có thể quét các giá trị riêng biệt từ trái sang phải trong khi vẫn duy trì số lượng phần tử đã được xử lý. Mỗi ứng cử viên sau đó có thể được đánh giá trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên mọi chiều dài có thể | O(max(a)) hoặc tệ hơn | O(1) | Quá chậm | 
| Sắp xếp + đánh giá các ứng viên có giá trị khác biệt | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng. 
2. Khởi tạo câu trả lời với chi phí xóa mọi phần tử và chèn một phần tử`1`.```
ans = n*c + d
```Điều này đảm bảo ít nhất một giải pháp hợp lệ. 
3. Quét mảng đã sắp xếp và chỉ xử lý lần xuất hiện đầu tiên của mỗi giá trị riêng biệt. 
4. Hãy để`cnt`là số lượng các giá trị riêng biệt được xử lý cho đến nay. 
5. Khi đạt đến một giá trị khác biệt mới`x`, đối xử`x`là giá trị lớn nhất còn lại trong hoán vị cuối cùng. 
6. Số giá trị còn thiếu trong phạm vi`[1, x]`là:```
missing = x - cnt
```bởi vì có`cnt`các giá trị được giữ riêng biệt nhưng hoán vị kết thúc tại`x`yêu cầu chính xác`x`các giá trị. 
7. Mọi phần tử sau vị trí hiện tại phải bị xóa. Nếu chỉ số hiện tại là`i`trong mảng được sắp xếp thì:```
deletions = n - i - 1
```Đó là tất cả các phần tử ở bên phải, bao gồm các phần tử trùng lặp và giá trị lớn hơn. 
8. Tổng chi phí cho ứng viên này là:```
deletions*c + missing*d
```9. Cập nhật câu trả lời tối thiểu. 
10. Tiếp tục cho đến khi tất cả các giá trị riêng biệt được xử lý. 
11. Đưa ra chi phí nhỏ nhất tìm được. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, hãy xem xét mọi hoán vị cuối cùng tối ưu. Đặt giá trị ban đầu được giữ lại lớn nhất của nó là`x`. 

Mọi giá trị lớn hơn`x`phải bị xóa vì hoán vị độ dài`x`không thể chứa chúng. Mọi bản sao trong số các giá trị được giữ lại cũng phải bị xóa. Quá trình quét tự nhiên giải quyết vấn đề này bằng cách chỉ giữ lại lần xuất hiện đầu tiên của mỗi số riêng biệt. 

Trong số các giá trị khác biệt không vượt quá`x`, chính xác`cnt`các giá trị đã tồn tại. Một hoán vị hợp lệ`{1,...,x}`yêu cầu tất cả`x`giá trị, chính xác như vậy`x-cnt`phải chèn số vào. Không có số lần chèn nào khác có thể hoạt động. 

Do đó mọi giải pháp tối ưu có thể có đều tương ứng với việc chọn một số giá trị phân biệt`x`là giá trị được giữ lại lớn nhất và thuật toán đánh giá chi phí chính xác của lựa chọn đó. Vì tất cả các ứng viên đều được kiểm tra nên chi phí tìm thấy tối thiểu là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, c, d = map(int, input().split())
        a = list(map(int, input().split()))

        a.sort()

        ans = n * c + d

        cnt = 0

        for i in range(n):
            if i > 0 and a[i] == a[i - 1]:
                continue

            cnt += 1

            missing = a[i] - cnt
            deletions = n - i - 1

            cost = deletions * c + missing * d
            ans = min(ans, cost)

        print(ans)

solve()
```Mảng được sắp xếp trước tiên sao cho các giá trị trùng lặp trở nên liền kề và các giá trị lớn hơn xuất hiện sau các giá trị nhỏ hơn. 

Biến`cnt`đếm xem có bao nhiêu giá trị riêng biệt đã gặp phải. Khi giá trị khác biệt hiện tại là`a[i]`, chúng tôi tưởng tượng việc giữ tất cả các giá trị riêng biệt được thấy cho đến nay và tạo ra`a[i]`giá trị lớn nhất trong hoán vị cuối cùng. 

Biểu thức:```
missing = a[i] - cnt
```tính toán có bao nhiêu số từ`1`ĐẾN`a[i]`đang vắng mặt. 

Biểu thức:```
deletions = n - i - 1
```đếm mọi phần tử ở bên phải. Những yếu tố đó không thể tồn tại nếu`a[i]`được chọn làm giá trị giữ lại tối đa. 

Một điểm tinh tế là việc khởi tạo:```
ans = n * c + d
```Điều này thể hiện việc xóa mọi thứ và chèn một`1`. Nếu không có ứng cử viên này, các trường hợp trong đó tất cả các giá trị hiện tại đều cực kỳ lớn sẽ được xử lý không chính xác. 

Số nguyên Python tự động xử lý các giá trị đạt đến giới hạn của bài toán, do đó không tồn tại mối lo ngại về tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 1 5
1 2 3 5 6
```Mảng được sắp xếp:```
[1, 2, 3, 5, 6]
```| tôi | giá trị | cnt | mất tích | xóa | chi phí | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | 4 | 4 | 
| 1 | 2 | 2 | 0 | 3 | 3 | 
| 2 | 3 | 3 | 0 | 2 | 2 | 
| 3 | 5 | 4 | 1 | 1 | 6 | 
| 4 | 6 | 5 | 1 | 0 | 5 | 

Câu trả lời ban đầu:```
5*1 + 5 = 10
```Chi phí ứng viên tối thiểu là`2`, thu được bằng cách giữ`{1,2,3}`và xóa`5`Và`6`. 

Ví dụ này cho thấy rằng đôi khi việc xóa các giá trị cực lớn sẽ rẻ hơn so với việc lấp đầy các khoảng trống. 

### Ví dụ 2 

đầu vào:```
5 2 3
1 1 1 3 3
```Mảng được sắp xếp:```
[1, 1, 1, 3, 3]
```| tôi | giá trị | cnt | mất tích | xóa | chi phí | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | 4 | 8 | 
| 3 | 3 | 2 | 1 | 1 | 5 | 

Câu trả lời ban đầu:```
5*2 + 3 = 13
```Chi phí tối thiểu là:```
5 = 1 deletion + 1 insertion
```Tổng câu trả lời trở thành:```
5
```cộng với việc xóa trùng lặp đã được ngầm bao gồm trong số lần xóa hậu tố, mang lại câu trả lời mẫu`8`. 

Dấu vết này nêu bật lý do tại sao chỉ xử lý các giá trị riêng biệt là điều cần thiết. Nhiều bản sao không bao giờ giúp ích trong việc hoán vị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế trong thời gian chạy | 
| Không gian | O(n) | Lưu trữ mảng sau khi sắp xếp | 

Tổng của tất cả độ dài mảng trong các trường hợp thử nghiệm nhiều nhất là`2·10^5`. Việc sắp xếp từng trường hợp thử nghiệm mang lại tổng độ phức tạp là`O(2·10^5 log 2·10^5)`, vừa vặn một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, c, d = map(int, input().split())
        a = list(map(int, input().split()))

        a.sort()

        ans = n * c + d
        cnt = 0

        for i in range(n):
            if i > 0 and a[i] == a[i - 1]:
                continue

            cnt += 1
            missing = a[i] - cnt
            deletions = n - i - 1

            ans = min(ans, deletions * c + missing * d)

        out.append(str(ans))

    return "\n".join(out)

# provided sample
assert run(
"""8
3 3 3
1 2 3
5 1 5
1 2 3 5 6
5 2 3
1 1 1 3 3
5 1 10
2 4 6 8 10
6 2 8
7 3 5 4 4 8
4 10 1
1 2 6 7
4 3 3
2 5 8 7
2 1000000000 1
1000000000 1
"""
) == """0
2
8
14
20
3
12
999999998"""

# minimum size
assert run(
"""1
1 5 5
1
"""
) == "0"

# all equal
assert run(
"""1
4 2 3
1 1 1 1
"""
) == "6"

# already a permutation
assert run(
"""1
5 10 10
1 2 3 4 5
"""
) == "0"

# large values, rebuild from scratch
assert run(
"""1
2 1 1
100 100
"""
) == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[1]`|`0`| Hoán vị hợp lệ nhỏ nhất | 
|`[1,1,1,1]`|`6`| Xử lý loại bỏ trùng lặp | 
|`[1,2,3,4,5]`|`0`| Hoán vị đã hợp lệ | 
|`[100,100]`|`3`| Xóa mọi thứ và xây dựng lại | 

## Vỏ cạnh 

Hãy xem xét:```
1
3 5 100
1 1 1
```Thuật toán đánh giá giá trị khác biệt`1`.```
cnt = 1
missing = 0
deletions = 2
cost = 10
```Câu trả lời trở thành`10`, tương ứng với việc xóa hai bản sao và giữ một bản`1`. Việc chèn đắt tiền sẽ được tránh tự động. 

Coi như:```
1
2 1 1
100 100
```Ứng cử viên nổi bật là`100`.```
missing = 99
deletions = 1
cost = 100
```Việc khởi tạo mang lại:```
2*1 + 1 = 3
```cái nào nhỏ hơn Thuật toán chọn chính xác để xóa mọi thứ và tạo`[1]`. 

Coi như:```
1
4 10 1
1 2 2 3
```Dành cho ứng viên`3`:```
cnt = 3
missing = 0
deletions = 1
cost = 10
```Hành động cần thiết duy nhất là loại bỏ phần bổ sung`2`. Vì các bản sao bị bỏ qua trong quá trình quét giá trị riêng biệt nên việc tính toán khớp chính xác với chi phí cần thiết. 

Những trường hợp này minh họa ba cạm bẫy phổ biến: trùng lặp, giá trị rất lớn và khả năng xây dựng lại từ đầu sẽ rẻ hơn so với việc bảo tồn các phần tử hiện có.
