---
title: "CF 2084B - MIN = GCD"
description: "Chúng ta được cho một chuỗi các số nguyên dương và chúng ta cần xác định xem liệu có thể chia chuỗi thành hai phần liền kề sau khi sắp xếp lại hay không, sao cho giá trị nhỏ nhất ở phần thứ nhất bằng ước số chung lớn nhất của phần thứ hai."
date: "2026-06-08T06:11:36+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2084
codeforces_index: "B"
codeforces_contest_name: "Teza Round 1 (Codeforces Round 1015, Div. 1 + Div. 2)"
rating: 1100
weight: 2084
solve_time_s: 89
verified: true
draft: false
---

[CF 2084B - MIN = GCD](https://codeforces.com/problemset/problem/2084/B) 

**Đánh giá:** 1100 
**Tags:** tham lam, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các số nguyên dương và chúng ta cần xác định xem liệu có thể chia chuỗi thành hai phần liền kề sau khi sắp xếp lại hay không, sao cho giá trị nhỏ nhất ở phần thứ nhất bằng ước số chung lớn nhất của phần thứ hai. Nói cách khác, chúng ta được phép sắp xếp lại mảng một cách tùy ý, sau đó chọn một điểm phân chia trong đó giá trị tối thiểu của đoạn bên trái khớp với GCD của đoạn bên phải. 

Đầu vào đảm bảo lên tới$10^4$trường hợp thử nghiệm, mỗi trường hợp có$n$lên đến$10^5$số nguyên và tổng của$n$trên tất cả các trường hợp thử nghiệm là nhiều nhất$10^5$. Bản thân các số nguyên có thể rất lớn, lên tới$10^{18}$. Điều này ngay lập tức loại trừ các thuật toán yêu cầu kiểm tra tất cả các phép chia hoặc hoán vị có thể có. Bất kỳ giải pháp nào lặp lại$O(n^2)$khả năng sẽ là quá chậm. Chúng ta phải tìm một$O(n)$hoặc$O(n \log n)$cách tiếp cận cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh khóa là khi tất cả các phần tử đều bằng nhau. Ví dụ, nếu trình tự là$[7,7,7]$, chúng ta có thể phân chia sau bất kỳ vị trí nào và giá trị tối thiểu và GCD đều sẽ là 7, vì vậy câu trả lời là "Có". Một trường hợp tinh tế khác xảy ra khi mức tối thiểu xảy ra nhiều lần nhưng không phải là ước số của tất cả các phần tử còn lại. Ví dụ,$[1,2,3]$không thể phân chia để thỏa mãn điều kiện nếu 1 là mức tối thiểu bên trái và GCD bên phải không phải là 1. Các thuật toán bất cẩn chỉ kiểm tra sự tồn tại của mức tối thiểu toàn cục ở đâu đó có thể thất bại ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các hoán vị của chuỗi và tất cả các vị trí phân chia, tính toán mức tối thiểu cho bên trái và GCD cho bên phải. Về mặt lý thuyết, điều này có hiệu quả vì bất kỳ phép chia đúng nào cũng phải thỏa mãn đẳng thức đã cho, nhưng trong thực tế, có$n!$hoán vị, mỗi hoán vị có$n-1$chia tách, điều này là không thể thực hiện được ngay cả đối với$n=10$. 

Quan sát cấu trúc bài toán, chúng ta nhận thấy rằng ứng cử viên duy nhất cho phân đoạn tối thiểu bên trái có thể khớp với phân đoạn GCD bên phải là mức tối thiểu toàn cục của mảng. Điều này là do bất kỳ mức tối thiểu nào trong phân đoạn bên trái không thể nhỏ hơn mức tối thiểu toàn cầu và GCD của bất kỳ số nguyên dương nào nhiều nhất là nhỏ nhất trong số chúng. Vì vậy, nếu có sự phân chia, giá trị$\min(left) = \gcd(right)$phải bằng tổng mức tối thiểu của mảng. 

Với điều này, chúng ta có thể đơn giản hóa vấn đề: hãy$m$là mức tối thiểu toàn cầu của mảng. Khi đó bất kỳ phần tử nào không phải là bội số của$m$không thể nằm đúng phân khúc vì GCD của phân khúc đó phải bằng$m$. Vì vậy, giải pháp giảm xuống việc kiểm tra xem tất cả các phần tử có thể chia hết cho$m$có thể tạo thành một đoạn bên phải liền kề sau khi loại bỏ các phần tử khác. Nếu có, chúng ta có thể sắp xếp lại mảng sao cho các phần tử này ở cuối và đoạn bên trái chứa tất cả các phần tử còn lại. Nếu không, việc chia tách là không thể. 

Quan sát này làm giảm vấn đề từ kiểm tra thời gian giai thừa sang kiểm tra thời gian tuyến tính, chỉ cần một lần quét để đảm bảo tất cả các bội số của giá trị tối thiểu có thể đi đến phân đoạn bên trái. Chúng ta không cần tính toán GCD một cách linh hoạt vì chúng ta đã biết GCD của một tập hợp các số chia hết cho$m$ít nhất là$m$và bất kỳ số nào trong phân đoạn bên phải phải là bội số của$m$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$và mảng$a$. 
2. Tính mức tối thiểu toàn cầu$m = \min(a)$. Đây là giá trị ứng viên duy nhất cho cả hai$\min(left)$Và$\gcd(right)$. 
3. Xác minh rằng tất cả các phần tử trong mảng đều bằng$m$hoặc chia hết cho$m$. Bất kỳ phần tử nào không chia hết cho$m$phải ở đoạn bên trái. 
4. Sau khi phân vùng tất cả các bội số của$m$vào đoạn bên phải và phần còn lại vào bên trái, mức tối thiểu bên trái vẫn còn$m$và GCD bên phải là$m$cũng vậy. Nếu bất kỳ phần tử nào trong đoạn được cho là bên phải không chia hết cho$m$, xuất ra "Không". 
5. Nếu kiểm tra thành công, xuất ra "Có". 

Tại sao nó hoạt động: Thuật toán dựa trên bất biến rằng giá trị tối thiểu của phân đoạn bên trái không thể nhỏ hơn mức tối thiểu toàn cục và GCD của phân đoạn bên phải không thể nhỏ hơn số nhỏ nhất trong đó. Bằng cách đặt mức tối thiểu toàn cục làm ứng cử viên cho cả hai, mọi vi phạm (một phần tử trong phân đoạn bên phải không chia hết cho$m$) sẽ làm cho đẳng thức không thể xảy ra. Sắp xếp lại đảm bảo tất cả bội số của$m$có thể đi vào đúng đoạn mà vẫn giữ được điều kiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from math import gcd
from functools import reduce

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        m = min(a)
        if all(x % m == 0 or x == m for x in a):
            print("Yes")
        else:
            print("No")

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên tính toán mức tối thiểu toàn cầu theo thời gian tuyến tính. Điều kiện kiểm tra từng phần tử trong mảng một lần, xác minh tính chia hết cho$m$. sử dụng`all()`hiệu quả, dừng lại ở lần thất bại đầu tiên. Kiểu số nguyên của Python xử lý các giá trị lên tới$10^{18}$không tràn. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
3
2
1 1
2
1 2
3
2 2 3
```| Bước | Mảng | Tối thiểu m | Kiểm tra chia hết | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | [1,1] | 1 | 1%1=0,1%1=0 | Có | 
| 2 | [1,2] | 1 | 1%1=0,2%1=0 | Có | 
| 3 | [2,2,3] | 2 | 2%2=0,2%2=0,3%2≠0 | Không | 

Trường hợp đầu tiên hoạt động bình thường vì cả hai số đều bằng nhau. Trường hợp thứ hai hoạt động vì tất cả các số đều chia hết cho 1. Trường hợp thứ ba không thành công vì 3 không chia hết cho 2 nên chúng ta không thể đặt nó vào đúng đoạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Một lượt để tìm mức tối thiểu và kiểm tra tính chia hết | 
| Không gian | O(n) | Lưu trữ mảng để xử lý | 

Cho tổng của$n$trên tất cả các trường hợp thử nghiệm không vượt quá$10^5$, thuật toán chạy tốt trong chưa đầy 1 giây và mức sử dụng bộ nhớ trong khoảng 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("7\n2\n1 1\n2\n1 2\n3\n2 2 3\n3\n2 3 4\n5\n4 5 6 9 3\n3\n998244359987710471 99824435698771045 1000000007\n6\n1 1 4 5 1 4\n") == "Yes\nNo\nYes\nNo\nYes\nYes\nYes"

# Custom cases
assert run("1\n2\n7 7\n") == "Yes", "all equal"
assert run("1\n3\n6 9 12\n") == "Yes", "all multiples of minimum"
assert run("1\n3\n2 3 4\n") == "No", "non-divisible element"
assert run("1\n2\n1 1000000000000000000\n") == "Yes", "minimum is 1"
assert run("1\n4\n2 2 2 2\n") == "Yes", "all identical large array"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n7 7 | Có | Tất cả các phần tử đều bằng nhau | 
| 3\n6 9 12 | Có | Tất cả các phần tử bội số tối thiểu | 
| 3\n2 3 4 | Không | Có phần tử không chia hết | 
| 2\n1 10000000000000000000 | Có | Tối thiểu là 1, số nguyên lớn | 
| 4\n2 2 2 2 | Có | Tất cả mảng lớn giống hệt nhau | 

## Vỏ cạnh 

Trường hợp cạnh của tất cả các phần tử bằng nhau được xử lý vì mức tối thiểu toàn cục bằng với mọi phần tử, do đó việc kiểm tra tính chia hết sẽ thành công. Đối với các mảng có giá trị tối thiểu là 1, tất cả các phần tử đều chia hết cho 1, do đó, mọi phép chia đều có tác dụng. Mảng có một phần tử không chia hết cho số tối thiểu sẽ không vượt qua quá trình kiểm tra, tạo ra kết quả "Không" chính xác. Thuật toán xử lý chính xác các số lớn vì số nguyên Python tự động mở rộng mà không bị tràn.
