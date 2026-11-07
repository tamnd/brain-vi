---
title: "CF 1380C - Tạo nhóm"
description: "Chúng tôi có một nhóm lập trình viên, mỗi người có một giá trị kỹ năng. Chúng tôi muốn thành lập càng nhiều đội càng tốt. Một nhóm hợp lệ nếu: $$(text{team size}) lần (text{kỹ năng tối thiểu trong nhóm}) ge x$$ Mỗi lập trình viên có thể thuộc về tối đa một nhóm và một số lập trình viên có thể vẫn chưa được sử dụng."
date: "2026-06-11T10:55:31+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1380
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 91 (Rated for Div. 2)"
rating: 1400
weight: 1380
solve_time_s: 91
verified: true
draft: false
---

[CF 1380C - Tạo nhóm](https://codeforces.com/problemset/problem/1380/C) 

**Đánh giá:** 1400 
**Tags:** bạo lực, dp, tham lam, thực hiện, phân loại 
**Thời gian giải:** 1 phút 31 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một nhóm lập trình viên, mỗi người có một giá trị kỹ năng. Chúng tôi muốn thành lập càng nhiều đội càng tốt. Một đội hợp lệ nếu:$$(\text{team size}) \times (\text{minimum skill in the team}) \ge x$$Mỗi lập trình viên có thể thuộc nhiều nhất một nhóm và một số lập trình viên có thể vẫn chưa được sử dụng. 

Nhiệm vụ là xác định số lượng đội hợp lệ tối đa có thể được tạo. 

Trường hợp thử nghiệm lớn nhất chứa tới$10^5$lập trình viên, và tổng của tất cả$n$giá trị trên các trường hợp thử nghiệm cũng nhiều nhất là$10^5$. Với kích thước đầu vào này, bất kỳ thuật toán nào liên tục thử các phân vùng khác nhau của người lập trình là không thể. Thậm chí một$O(n^2)$giải pháp sẽ yêu cầu khoảng$10^{10}$trong trường hợp xấu nhất vượt xa giới hạn. Chúng ta cần thứ gì đó xung quanh$O(n \log n)$hoặc tốt hơn. 

Điều khó khăn là chất lượng của đội chỉ phụ thuộc vào thành viên yếu nhất. Việc bổ sung thêm những lập trình viên mạnh hơn không cải thiện được kỹ năng tối thiểu, nhưng việc bổ sung thêm nhiều lập trình viên sẽ làm tăng quy mô nhóm. Một chiến lược phân nhóm ngây thơ có thể dễ dàng lãng phí những lập trình viên giỏi và làm giảm đi câu trả lời cuối cùng. 

Hãy xem xét ví dụ này:```
n = 4, x = 10
skills = [10, 10, 10, 10]
```Câu trả lời đúng là 4. Mỗi lập trình viên tự mình tạo thành một nhóm hợp lệ vì$1 \times 10 = 10$. Một cách tiếp cận bất cẩn khi kết hợp các lập trình viên thành các nhóm lớn hơn sẽ tạo ra ít nhóm hơn. 

Một trường hợp quan trọng khác là:```
n = 5, x = 10
skills = [5, 5, 5, 5, 5]
```Câu trả lời đúng là 2. Mỗi nhóm hợp lệ cần ít nhất hai lập trình viên vì$2 \times 5 = 10$. Chúng ta có thể tạo hai nhóm cỡ 2 và để lại một lập trình viên không được sử dụng. Việc triển khai nhấn mạnh vào việc sử dụng mọi người sẽ trả về sai 1. 

Trường hợp cạnh thứ ba là khi không thể thành lập đội:```
n = 4, x = 11
skills = [1, 3, 3, 7]
```Ngay cả việc sử dụng cả bốn lập trình viên cũng mang lại$4 \times 1 = 4$, vẫn dưới 11. Câu trả lời là 0. 

Những ví dụ này cho thấy rằng việc tối đa hóa số lượng nhóm không giống với việc tối đa hóa số lượng lập trình viên được sử dụng. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử nhiều cách khác nhau để phân chia các lập trình viên thành các nhóm và kiểm tra xem phân vùng nào thỏa mãn điều kiện. Phương pháp như vậy là đúng vì nó xem xét tất cả các khả năng, nhưng số lượng phân vùng tăng theo cấp số nhân. Ngay cả đối với vài chục lập trình viên thì điều đó cũng trở thành không thể. 

Để làm tốt hơn, chúng ta cần hiểu đặc điểm của một nhóm hợp lệ. 

Giả sử kỹ năng tối thiểu trong một nhóm là$m$. Khi đó đội có giá trị chính xác khi:$$(\text{team size}) \times m \ge x$$Nếu chúng ta sắp xếp các kỹ năng theo thứ tự giảm dần và xử lý các lập trình viên từ mạnh nhất đến yếu nhất, thì khi chúng ta tiếp cận được một lập trình viên có kỹ năng$a_i$, mọi lập trình viên được thu thập cho đến nay đều có ít nhất kỹ năng$a_i$. Nếu chúng ta quyết định rằng$a_i$sẽ là thành viên yếu nhất trong một đội, sau đó$a_i$trở thành kỹ năng tối thiểu của đội. 

Lúc đó, chúng ta chỉ cần biết đã tập hợp được bao nhiêu lập trình viên cho đội ngũ hiện tại. Nếu như:$$(\text{current size}) \times a_i \ge x$$thì một đội hợp lệ có thể được hoàn thành ngay lập tức. 

Quan sát này dẫn đến một chiến lược tham lam. Sắp xếp các kỹ năng từ lớn nhất đến nhỏ nhất. Xây dựng một nhóm bằng cách tích lũy các lập trình viên. Bất cứ khi nào nhóm hiện tại trở nên hợp lệ, hãy hoàn thiện nó ngay lập tức và bắt đầu xây dựng nhóm tiếp theo. 

Tại sao việc đóng nhóm ngay khi nó có hiệu lực là an toàn? 

Bởi vì mỗi lập trình viên bổ sung được nhóm này sử dụng có thể giúp thành lập một nhóm khác sau này. Khi một nhóm đã hợp lệ, việc mở rộng nhóm sẽ không bao giờ làm tăng số lượng đội. Đội hợp lệ nhỏ nhất luôn là sự lựa chọn tốt nhất. 

Điều này biến vấn đề thành một quá trình phân loại và quét đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tham lam tối ưu + Sắp xếp |$O(n \log n)$|$O(1)$phụ trợ (không bao gồm sắp xếp) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các kỹ năng theo thứ tự giảm dần. 
2. Khởi tạo bộ đếm`current_size = 0`. 
3. Khởi tạo`teams = 0`. 
4. Xử lý các kỹ năng được sắp xếp từ trái sang phải. 
5. Đối với mỗi giá trị kỹ năng`s`, thêm lập trình viên đó vào nhóm chưa hoàn thành hiện tại bằng cách tăng`current_size`. 
6. Điều trị`s`là kỹ năng tối thiểu của nhóm hiện tại. Vì chúng tôi đang xử lý theo thứ tự giảm dần nên mọi lập trình viên được thu thập trước đó đều có ít nhất kỹ năng`s`. 
7. Kiểm tra xem:$$current\_size \times s \ge x$$Nếu điều kiện này được giữ thì nhóm hiện tại đã thành lập một nhóm hợp lệ. 
8. Tăng`teams`bởi một. 
9. Đặt lại`current_size`về 0 và bắt đầu xây dựng đội tiếp theo. 
10. Sau khi tất cả các lập trình viên được xử lý, xuất ra`teams`. 

### Tại sao nó hoạt động 

Bất biến chính là trong khi quét mảng đã sắp xếp, giá trị kỹ năng hiện tại luôn là kỹ năng tối thiểu trong số tất cả các lập trình viên hiện đang tập hợp cho nhóm chưa hoàn thành. 

Bất cứ khi nào điều kiện`current_size * skill >= x`trở thành sự thật, chúng tôi có đủ lập trình viên để thành lập một nhóm hợp lệ có kỹ năng tối thiểu chính xác`skill`. Việc kết thúc nhóm ngay lập tức sử dụng số lượng lập trình viên ít nhất có thể cho nhóm đó. 

Bất kỳ giải pháp nào tiếp tục thêm lập trình viên sau khi nhóm đã hợp lệ chỉ tiêu tốn các tài nguyên có khả năng giúp tạo thêm các nhóm sau này. Vì các đội lớn hơn không bao giờ tăng số lượng đội nên việc tham lam hoàn thiện mọi đội hợp lệ không thể làm giảm câu trả lời tối ưu. 

Do đó, mọi nhóm được hình thành bởi thuật toán đều hợp lệ và mọi nhóm hợp lệ đều sử dụng số lượng lập trình viên sẵn có tối thiểu để tối đa hóa tổng số nhóm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    answers = []

    for _ in range(t):
        n, x = map(int, input().split())
        skills = list(map(int, input().split()))

        skills.sort(reverse=True)

        teams = 0
        current_size = 0

        for s in skills:
            current_size += 1

            if current_size * s >= x:
                teams += 1
                current_size = 0

        answers.append(str(teams))

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```Bước đầu tiên sắp xếp các kỹ năng theo thứ tự giảm dần. Điều này đảm bảo rằng bất cứ khi nào chúng ta kiểm tra một giá trị kỹ năng`s`, mọi lập trình viên đã được xếp vào nhóm chưa hoàn thành hiện tại đều có ít nhất kỹ năng`s`. Điều đó có nghĩa`s`chính xác là kỹ năng tối thiểu của đội ngũ tiềm năng đó.`current_size`lưu trữ số lượng lập trình viên đã được thu thập cho ứng viên nhóm hiện tại. Sau khi thêm một lập trình viên, chúng tôi kiểm tra xem điều kiện của nhóm có thỏa mãn hay không. 

Khi`current_size * s >= x`, một đội hợp lệ đã được thành lập. Sự lựa chọn tham lam là hoàn thành nó ngay lập tức và thiết lập lại`current_size`về không. Đây là chi tiết triển khai quan trọng. Việc quên đặt lại sẽ hợp nhất nhiều nhóm lại với nhau một cách không chính xác. 

Tất cả số học đều vừa vặn thoải mái bên trong các số nguyên Python. Sản phẩm lớn nhất có thể là:$$10^5 \times 10^9 = 10^{14}$$nằm trong phạm vi của Python. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
n = 5, x = 10
skills = [7, 11, 2, 9, 5]
```Sau khi sắp xếp:```
[11, 9, 7, 5, 2]
```| Kỹ năng | current_size trước khi kiểm tra | current_size × s | Đội được thành lập? | Đội | 
| --- | --- | --- | --- | --- | 
| 11 | 1 | 11 | Có | 1 | 
| 9 | 1 | 9 | Không | 1 | 
| 7 | 2 | 14 | Có | 2 | 
| 5 | 1 | 5 | Không | 2 | 
| 2 | 2 | 4 | Không | 2 | 

Câu trả lời cuối cùng:```
2
```Lập trình viên đầu tiên một mình thành lập một nhóm. Hai lập trình viên tiếp theo tạo thành một nhóm hợp lệ khác. Những lập trình viên còn lại không đủ khả năng để thành lập đội thứ ba. 

### Mẫu 2 

đầu vào:```
n = 4, x = 8
skills = [2, 4, 2, 3]
```Sau khi sắp xếp:```
[4, 3, 2, 2]
```| Kỹ năng | current_size trước khi kiểm tra | current_size × s | Đội được thành lập? | Đội | 
| --- | --- | --- | --- | --- | 
| 4 | 1 | 4 | Không | 0 | 
| 3 | 2 | 6 | Không | 0 | 
| 2 | 3 | 6 | Không | 0 | 
| 2 | 4 | 8 | Có | 1 | 

Câu trả lời cuối cùng:```
1
```Ví dụ này cho thấy rằng một số lập trình viên giỏi có thể cần phải được kết hợp trước khi đạt được yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế trong thời gian chạy | 
| Không gian |$O(1)$phụ trợ | Chỉ có một vài biến được sử dụng ngoài mảng đã sắp xếp | 

Tổng số lập trình viên trong tất cả các trường hợp thử nghiệm nhiều nhất là$10^5$. Sắp xếp nhiều giá trị yêu cầu đại khái$10^5 \log_2(10^5)$hoạt động, dễ dàng phù hợp trong thời hạn. Việc sử dụng bộ nhớ cũng thoải mái dưới mức giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string

import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))

        a.sort(reverse=True)

        teams = 0
        cur = 0

        for s in a:
            cur += 1
            if cur * s >= x:
                teams += 1
                cur = 0

        ans.append(str(teams))

    return "\n".join(ans)

# provided sample
assert run(
"""3
5 10
7 11 2 9 5
4 8
2 4 2 3
4 11
1 3 3 7
"""
) == "2\n1\n0"

# minimum size
assert run(
"""1
1 1
1
"""
) == "1"

# single programmer cannot form team
assert run(
"""1
1 2
1
"""
) == "0"

# all equal values
assert run(
"""1
5 10
5 5 5 5 5
"""
) == "2"

# every programmer alone forms a team
assert run(
"""1
4 10
10 10 10 10
"""
) == "4"

# boundary style case
assert run(
"""1
6 12
6 6 6 6 6 6
"""
) == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`|`1`| Đầu vào hợp lệ nhỏ nhất | 
|`1 2 / 1`|`0`| Trường hợp nhỏ nhất không thể | 
| Năm lập trình viên có kỹ năng 5,`x=10`|`2`| Lập trình viên còn sót lại có thể vẫn chưa được sử dụng | 
| Bốn lập trình viên có kỹ năng 10,`x=10`|`4`| Đội một thành viên | 
| Sáu lập trình viên có kỹ năng 6,`x=12`|`3`| Ngưỡng bình đẳng chính xác | 

## Vỏ cạnh 

Hãy xem xét:```
1
5 10
5 5 5 5 5
```Sau khi sắp xếp, không có gì thay đổi. Quá trình quét tiến hành như sau:```
size=1 -> 1*5=5
size=2 -> 2*5=10 -> team
size=1 -> 1*5=5
size=2 -> 2*5=10 -> team
size=1 -> end
```Câu trả lời là 2. Một lập trình viên vẫn chưa được sử dụng. Thuật toán không bao giờ cố gắng ép buộc tất cả các lập trình viên vào các nhóm. 

Bây giờ hãy xem xét:```
1
4 10
10 10 10 10
```Mỗi bước thỏa mãn ngay điều kiện:```
1*10 = 10
```Một nhóm được thành lập sau mỗi lập trình viên, tạo ra 4 nhóm. Chiến lược tham lam tự nhiên phát hiện ra rằng các nhóm đơn lẻ là tối ưu. 

Cuối cùng, hãy xem xét:```
1
4 11
1 3 3 7
```Thứ tự sắp xếp:```
7 3 3 1
```Quá trình quét tạo ra:```
1*7 = 7
2*3 = 6
3*3 = 9
4*1 = 4
```Không có séc nào đạt tới 11, vì vậy không có đội nào được thành lập. Thuật toán trả về chính xác 0, ngay cả sau khi sử dụng tất cả các lập trình viên có sẵn.
