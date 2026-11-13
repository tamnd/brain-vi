---
title: "CF 1696A - NIT orz!"
description: "Chúng ta được cung cấp một mảng các số nguyên và một số $z$. Chúng ta được phép chọn liên tục bất kỳ phần tử $ai$ nào và thực hiện hai thao tác theo bit: cập nhật $ai$ thành $ai operatorname{or} z$ và đồng thời cập nhật $z$ thành $ai operatorname{and} z$."
date: "2026-06-09T22:33:06+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1696
codeforces_index: "A"
codeforces_contest_name: "Codeforces Global Round 21"
rating: 800
weight: 1696
solve_time_s: 123
verified: true
draft: false
---

[CF 1696A - NIT orz!](https://codeforces.com/problemset/problem/1696/A) 

**Đánh giá:** 800 
**Tags:** bitmasks, tham lam 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và một số$z$. Chúng ta được phép chọn liên tục bất kỳ phần tử nào$a_i$và thực hiện hai thao tác bitwise: cập nhật$a_i$ĐẾN$a_i \operatorname{or} z$và đồng thời cập nhật$z$ĐẾN$a_i \operatorname{and} z$. Mục tiêu là xác định giá trị tối đa mà bất kỳ phần tử nào trong mảng có thể đạt tới sau khi áp dụng các thao tác này với số lần bất kỳ. 

Mảng đại diện cho các giá trị ban đầu mà chúng ta có thể thao tác và$z$là một giá trị truyền các bit của nó một cách có chọn lọc vào các phần tử mảng thông qua các phép toán OR trong khi làm mất các bit thông qua các phép toán AND. Vấn đề cơ bản là hiểu làm thế nào các bit có thể di chuyển từ$z$các phần tử của mảng và phép toán nào sẽ tạo ra số lớn nhất. 

Các ràng buộc ở mức vừa phải: mỗi mảng có tối đa 2000 phần tử và tổng của$n$trên tất cả các trường hợp thử nghiệm là nhiều nhất$10^4$. Điều này có nghĩa là thuật toán bậc hai cho mỗi trường hợp thử nghiệm là khả thi, nhưng thuật toán mô phỏng số lượng thao tác không giới hạn sẽ có rủi ro. Các trường hợp biên bao gồm các mảng có kích thước 1, các trường hợp trong đó$z$bằng 0 và các mảng trong đó tất cả các phần tử đều bằng$z$. Việc triển khai ngây thơ áp dụng thao tác một cách mù quáng cho đến khi hội tụ có thể bị kẹt hoặc thực hiện các thao tác không cần thiết. Ví dụ, nếu$a = [7]$Và$z = 3$, việc áp dụng lặp đi lặp lại thao tác không làm tăng mức tối đa vượt quá 7 và các vòng lặp bất cẩn có thể làm cho việc tính toán trở nên phức tạp hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng áp dụng thao tác trên mọi phần tử nhiều lần cho đến khi không có thay đổi nào xảy ra. Điều này đúng vì mọi ứng dụng đều tăng$a_i$hoặc giảm$z$, nên cuối cùng nó sẽ ổn định. Tuy nhiên, trong trường hợp xấu nhất,$z$có thể có tối đa 30 bit và đối với mỗi phần tử, chúng tôi có thể thử tất cả các kết hợp chuyển bit. Với$n = 2000$, điều này có thể dẫn đến hàng triệu thao tác, làm cho hoạt động brute-force bị chậm lại. 

Điểm mấu chốt là thứ tự thực hiện các thao tác không thành vấn đề nếu mục tiêu của chúng ta chỉ là phần tử tối đa. Mỗi thao tác$a_i = a_i \operatorname{or} z$chỉ có thể tăng$a_i$, Và$z = a_i \operatorname{and} z$chỉ có thể giảm$z$. Do đó, giá trị tối đa mà bất kỳ phần tử nào cũng có thể đạt được bằng cách chọn phần tử tối đa hóa$a_i \operatorname{or} z$trong một bước duy nhất. Việc áp dụng nhiều thao tác trên các phần tử khác nhau không thể mang lại giá trị cao hơn cho phần tử tối đa so với thao tác OR đơn lẻ tốt nhất. 

Điều này làm giảm vấn đề tính toán$\max(a_i \operatorname{or} z)$tổng thể$i$, tuyến tính theo kích thước của mảng. Cách tiếp cận này hiệu quả và phù hợp thoải mái trong những hạn chế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * 2^30) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Mỗi trường hợp thử nghiệm đều độc lập nên chúng tôi xử lý chúng một cách tuần tự. 
2. Với mỗi test case, hãy đọc$n$Và$z$, theo sau là mảng$a$chiều dài$n$. Các giá trị này xác định trường hợp vấn đề. 
3. Khởi tạo một biến`max_val`về không. Điều này sẽ theo dõi giá trị tối đa có thể đạt được trên tất cả các phần tử mảng. 
4. Lặp lại từng phần tử$a_i$trong mảng. 
5. Tính toán`candidate = a_i | z`. Điều này thể hiện giá trị$a_i$sẽ đạt được nếu chúng ta áp dụng thao tác một lần với hiện tại$z$. Chỉ một thao tác này là đủ để xem xét vì không có sự kết hợp nào của nhiều thao tác tạo ra giá trị tối đa cao hơn kết quả OR lớn nhất. 
6. Cập nhật`max_val`là giá trị tối đa của giá trị hiện tại của nó và`candidate`. 
7. Sau khi xử lý xong tất cả các phần tử, in`max_val`như là kết quả của trường hợp thử nghiệm. 

Tại sao nó hoạt động: thao tác OR đơn điệu đối với từng phần tử và thao tác AND trên$z$chỉ giảm bit trong các hoạt động tiếp theo. Do đó, giá trị cao nhất có thể đạt được trong một bước duy nhất thể hiện giá trị tối đa có thể có trong bất kỳ chuỗi hoạt động nào. Không có chuỗi nhiều thao tác nào có thể vượt qua mức tối đa này vì`a_i | z`đã thiết lập tất cả các bit có thể được chuyển từ$z$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, z = map(int, input().split())
    a = list(map(int, input().split()))
    max_val = 0
    for x in a:
        max_val = max(max_val, x | z)
    print(max_val)
```Mã tuân theo thuật toán một cách chính xác. Đọc đầu vào với`sys.stdin.readline`đảm bảo đầu vào nhanh cho mảng lớn. Phép toán OR được tính toán cho từng phần tử một cách độc lập và`max_val`theo dõi mức tối đa toàn cầu. Điều này tránh các hoạt động không cần thiết trên`z`vì giá trị giảm dần của nó không bao giờ tạo ra mức tối đa cao hơn. Tránh được các lỗi từng cái một vì mảng được lập chỉ mục bằng 0 trong nội bộ, mặc dù sự cố sử dụng tính năng lập chỉ mục một. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
2 3
3 4
```| a_i | a_i | z | a_i | ứng cử viên = a_i | z | giá trị tối đa | 
| --- | --- | --- | --- | --- | --- | --- | 
| 3 | 3 | 3 | 3 | 3 | 3 | 3 | 
| 4 | 4 | 3 | 4 | 7 | 3 | 7 | 

Giá trị tối đa là 7. 

Đối với đầu vào:```
5 5
0 2 4 6 8
```| a_i | a_i | z | a_i | ứng cử viên = a_i | z | giá trị tối đa | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 5 | 0 | 5 | 5 | 5 | 
| 2 | 2 | 5 | 2 | 7 | 5 | 7 | 
| 4 | 4 | 5 | 4 | 5 | 5 | 7 | 
| 6 | 6 | 5 | 6 | 7 | 5 | 7 | 
| 8 | 8 | 5 | 8 | 13 | 5 | 13 | 

Giá trị tối đa là 13, xác nhận thuật toán chọn đúng OR tốt nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được xử lý một lần bằng một thao tác OR. | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng`a`cho từng trường hợp thử nghiệm. | 

Cho rằng tổng của$n$trên tất cả các trường hợp thử nghiệm không vượt quá$10^4$, giải pháp sẽ chạy hiệu quả trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n, z = map(int, input().split())
        a = list(map(int, input().split()))
        max_val = 0
        for x in a:
            max_val = max(max_val, x | z)
        print(max_val)
    return output.getvalue().strip()

# provided samples
assert run("5\n2 3\n3 4\n5 5\n0 2 4 6 8\n1 9\n10\n5 7\n7 15 30 29 27\n3 39548743\n10293834 10284344 13635445") == "7\n13\n11\n31\n48234367", "sample 1"

# custom cases
assert run("1\n1 0\n0") == "0", "single element zero"
assert run("1\n3 0\n1 2 4") == "4", "z zero, pick max element"
assert run("1\n4 15\n0 0 0 0") == "15", "all zeros, z non-zero"
assert run("1\n5 31\n1 2 4 8 16") == "31", "z maximum, spreads to any element"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0\n0 | 0 | Mảng một phần tử, z=0 | 
| 3 0\n1 2 4 | 4 | z=0 không tăng bất kỳ phần tử nào, chọn max a_i | 
| 4 15\n0 0 0 0 | 15 | z chuyển tất cả các bit sang bất kỳ phần tử nào | 
| 5 31\n1 2 4 8 16 | 31 | Z tối đa kết hợp với bất kỳ a_i nào để có giá trị lớn nhất | 

## Vỏ cạnh 

Khi nào`z`bằng 0, phép toán OR không thay đổi bất kỳ phần tử nào, vì vậy câu trả lời đơn giản là giá trị lớn nhất của mảng ban đầu. Ví dụ, nếu`a = [1, 2, 4]`Và`z = 0`, mỗi`a_i | z`bằng`a_i`, cho tối đa là 4. Khi tất cả các phần tử bằng 0 và`z`khác không, mức tối đa trở thành chính xác`z`bởi vì mọi phần tử HOẶC z đều bằng z. Nếu như`z`đã lớn hơn tất cả các phần tử mảng, hãy áp dụng nó
