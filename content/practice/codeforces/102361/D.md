---
title: "CF 102361D - Thập phân"
description: "Đối với mỗi trường hợp thử nghiệm, chúng ta được cho một số nguyên dương (n) và chúng ta cần quyết định xem cuối cùng biểu diễn thập phân của phân số (1/n) có kết thúc hay không."
date: "2026-08-13T00:07:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102361
codeforces_index: "D"
codeforces_contest_name: "2019 China Collegiate Programming Contest Qinhuangdao Onsite"
rating: 0
weight: 102361
solve_time_s: 49
verified: true
draft: false
---

[CF 102361D - Số thập phân](https://codeforces.com/problemset/problem/102361/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đối với mỗi trường hợp thử nghiệm, chúng ta được cho một số nguyên dương (n) và chúng ta cần quyết định xem cuối cùng biểu diễn thập phân của phân số (1/n) có kết thúc hay không. Nếu nó có thể được viết bằng một số hữu hạn các chữ số sau dấu thập phân, chúng ta sẽ in`No`, vì bài toán hỏi số thập phân có phải là vô hạn hay không. Nếu việc khai triển thập phân tiếp tục mãi mãi, chúng ta in`Yes`. 

Ví dụ: (1/5=0,2), do đó việc mở rộng thập phân của nó kết thúc và câu trả lời là`No`. Mặt khác, (1/3=0,333\ldots), nên đáp án là`Yes`. 

Có nhiều nhất 100 trường hợp thử nghiệm và mỗi (n) nhiều nhất là 100. Những ràng buộc này cực kỳ nhỏ. Ngay cả một giải pháp (O(Tn)) cũng thực hiện tối đa khoảng 10.000 lần lặp cơ bản, do đó không có mối lo ngại nào về hiệu suất thực tế. Chúng ta vẫn có thể rút ra điều kiện lý thuyết số để giải từng trường hợp kiểm thử theo thời gian logarit. 

Các trường hợp cạnh chính là các mẫu số như (1), (2), (4), (5), (8), (10), (20) và (25). Ví dụ, đầu vào```
1
1
```phải sản xuất```
No
```bởi vì (1/1=1), không có phần phân số vô hạn. Việc triển khai bất cẩn giả định rằng mọi phân số đều phải có các chữ số sau dấu thập phân có thể phân loại sai phân số thành vô hạn. 

Một trường hợp ranh giới khác là```
1
10
```câu trả lời của ai cũng là`No`, bởi vì (1/10=0,1). Mẫu số chứa cả hệ số 2 và hệ số 5, và cả hai đều được phép. 

Trường hợp ngược lại là```
1
6
```câu trả lời của ai là`Yes`, bởi vì (1/6=0,1666\ldots). Thừa số 3 vẫn giữ nguyên mẫu số sau khi loại bỏ tất cả các thừa số của 2 và 5. Việc triển khai chỉ kiểm tra xem (n) là chẵn hay lẻ sẽ bỏ sót trường hợp này. 

## Phương pháp tiếp cận 

Một cách trực tiếp để giải quyết vấn đề là mô phỏng phép chia dài. Khi tính toán (1/n), số dư hiện tại được nhân với 10 trước khi tạo ra chữ số thập phân tiếp theo. Nếu phần còn lại bằng 0 thì số thập phân chấm dứt. Nếu số dư lặp lại thì chuỗi chữ số tương tự sẽ bắt đầu lặp lại, do đó số thập phân là vô hạn. Có nhiều nhất (n) phần dư có thể có, vì vậy một trường hợp kiểm thử sẽ mất (O(n)) thời gian. Với (T\le100) và (n\le100), trường hợp xấu nhất là ít hơn 10.000 lần chuyển đổi còn lại, do đó phương pháp này dễ dàng đủ nhanh đối với các ràng buộc đã cho. 

Quan sát hữu ích hơn đến từ cấu trúc tận cùng của phân số thập phân. Số thập phân hữu hạn luôn có dạng (a/10^k) đối với một số số nguyên (a) và (k). Vì (10^k=2^k5^k), các thừa số nguyên tố duy nhất có thể xuất hiện ở mẫu số rút gọn của số thập phân tận cùng là 2 và 5. 

Bởi vì tử số ở đây chính xác là 1 nên phân số (1/n) đã bị rút gọn. Do đó, biểu diễn thập phân của nó kết thúc chính xác khi (n) không chứa thừa số nguyên tố nào ngoài 2 và 5. 

Chúng ta có thể kiểm tra điều này bằng cách liên tục chia (n) cho 2 nếu có thể, sau đó chia liên tục cho 5 nếu có thể. Nếu giá trị còn lại là 1 thì mọi thừa số nguyên tố đều là 2 hoặc 5, do đó số thập phân kết thúc và câu trả lời là`No`. Nếu vẫn còn một số yếu tố khác thì số thập phân là vô hạn và câu trả lời là`Yes`. 

Phương pháp brute-force hoạt động vì phép chia dài cuối cùng đạt đến số dư bằng 0 hoặc lặp lại số dư. Việc quan sát hệ số hóa có thể thay thế nhiều chữ số thập phân được mô phỏng bằng một số phép chia. Đối với bài toán này, cả hai phương pháp đều đủ nhanh, nhưng giải pháp dựa trên hệ số nắm bắt trực tiếp điều kiện toán học và tổng quát hóa rõ ràng cho câu hỏi tiêu chuẩn về việc liệu một số hữu tỷ có khai triển thập phân tận cùng hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(Tn)) | (O(n)) | Đã chấp nhận | 
| Tối ưu | (O(T\log n)) | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (n) cho test case hiện tại. Chúng ta chỉ cần kiểm tra các thừa số nguyên tố của nó nên không cần xây dựng biểu diễn thập phân. 
2. Chia (n) cho 2 nhiều lần khi n chia hết cho 2. Mọi thừa số của 2 đều được phép có trong mẫu số của số thập phân tận cùng. 
3. Liên tục chia giá trị còn lại cho 5 khi giá trị đó chia hết cho 5. Các thừa số của 5 là các thừa số nguyên tố được phép khác. 
4. Kiểm tra giá trị còn lại. Nếu nó chính xác bằng 1 thì mẫu số ban đầu chỉ gồm thừa số 2 và 5, do đó (1/n) kết thúc và ta in`No`. 
5. Nếu giá trị còn lại lớn hơn 1, nó chứa một số thừa số nguyên tố khác 2 hoặc 5. Hệ số đó không thể chia bất kỳ lũy thừa nào của 10, do đó việc khai triển thập phân không thể kết thúc và chúng ta in`Yes`. 

### Tại sao nó hoạt động 

Giả sử (1/n) có khai triển thập phân hữu hạn. Khi đó có một số (k) sao cho 

[ 
\frac{1}{n}=\frac{m}{10^k} 
] 

cho một số nguyên (m). Sắp xếp lại sẽ có (10^k=mn), nên (n) phải chia (10^k). Vì (10^k=2^k5^k), mọi thừa số nguyên tố của (n) phải là 2 hoặc 5. 

Ngược lại, nếu (n=2^a5^b), hãy chọn (k=\max(a,b)). Khi đó (n) chia (10^k), do đó (1/n) có thể được biểu thị bằng mẫu số (10^k), cho một khai triển thập phân hữu hạn. 

Thuật toán loại bỏ chính xác tất cả thừa số 2 và 5. Như vậy giá trị còn lại chính xác là 1 khi số thập phân kết thúc. Điều này làm cho câu trả lời được in ra chính xác cho mọi đầu vào có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())

        while n % 2 == 0:
            n //= 2

        while n % 5 == 0:
            n //= 5

        if n == 1:
            print("No")
        else:
            print("Yes")

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên loại bỏ mọi thừa số của 2 khỏi mẫu số. Điều kiện sử dụng`n % 2 == 0`trước phép chia, do đó vòng lặp dừng chính xác khi không còn thừa số 2. 

Vòng lặp thứ hai thực hiện thao tác tương tự cho 5. Thứ tự của hai vòng này không ảnh hưởng đến kết quả vì phép nhân các thừa số nguyên tố có tính chất giao hoán. 

Sau cả hai vòng lặp,`n == 1`có nghĩa là ban đầu không có gì ngoại trừ thừa số 2 và 5. Trong trường hợp đó số thập phân là hữu hạn nên câu trả lời bắt buộc là`No`. Nếu như`n`lớn hơn 1 thì vẫn còn thừa số nguyên tố khác, nên đáp án là`Yes`. 

Số nguyên Python không gặp vấn đề tràn ở đây và thuật toán chỉ giảm`n`. Đầu vào lớn nhất chỉ là 100, do đó việc triển khai cũng thoải mái trong giới hạn đầu vào đã nêu. 

## Ví dụ đã hoạt động 

Mẫu bao gồm hai trường hợp thử nghiệm (n=5) và (n=3). 

Với (n=5), thuật toán loại bỏ hệ số 5 và đạt 1. 

| Bước | Hiện tại (n) | Hành động | 
| --- | --- | --- | 
| Bắt đầu | 5 | Kiểm tra các yếu tố | 
| Xóa 2 | 5 | Không chia hết cho 2 | 
| Xóa 5 | 1 | Chia cho 5 | 
| Cuối cùng | 1 | In`No`| 

Điều này khớp với (1/5=0,2). Khi mẫu số trở thành 1, mọi thừa số nguyên tố ban đầu đã được chứng minh là tương thích với lũy thừa 10. 

Đối với (n=3), cả vòng lặp loại bỏ yếu tố đều không thay đổi giá trị. 

| Bước | Hiện tại (n) | Hành động | 
| --- | --- | --- | 
| Bắt đầu | 3 | Kiểm tra các yếu tố | 
| Xóa 2 | 3 | Không chia hết cho 2 | 
| Xóa 5 | 3 | Không chia hết cho 5 | 
| Cuối cùng | 3 | In`Yes`| 

Thừa số 3 còn lại chứng tỏ không có lũy thừa nào của 10 có thể chia hết cho mẫu số ban đầu. Do đó, số thập phân không thể kết thúc bằng khớp (1/3=0,333\ldots). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(T\log n)) | Mỗi phép chia cho 2 hoặc 5 làm giảm (n) một hệ số không đổi. | 
| Không gian | (O(1)) | Chỉ có mẫu số hiện tại và các biến vòng lặp được lưu trữ. | 

Với (T\le100) và (n\le100), mỗi ca kiểm thử chỉ yêu cầu một vài phép chia số nguyên. Tổng công việc rất nhỏ và việc sử dụng bộ nhớ liên tục không phụ thuộc vào số lượng trường hợp kiểm thử. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    t = int(input())

    for _ in range(t):
        n = int(input())

        while n % 2 == 0:
            n //= 2

        while n % 5 == 0:
            n //= 5

        print("No" if n == 1 else "Yes")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()
    output = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return output

# Provided sample
assert run("""2
5
3
""") == """No
Yes
""", "provided sample"

# Minimum-size input
assert run("""1
1
""") == """No
""", "n = 1 terminates immediately"

# Maximum-size input
assert run("""1
100
""") == """No
""", "100 = 2^2 * 5^2"

# Several powers of 2 and 5
assert run("""6
2
4
5
8
20
25
""") == """No
No
No
No
No
No
""", "all denominators contain only 2 and 5"

# Values containing another prime factor
assert run("""5
3
6
7
12
99
""") == """Yes
Yes
Yes
Yes
Yes
""", "each denominator has a prime factor other than 2 or 5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`No`| Giá trị tối thiểu và trường hợp đặc biệt khi mẫu số đã là 1 | 
|`1 / 100`|`No`| Giá trị tối đa và loại bỏ nhiều lần cả 2 và 5 | 
|`2, 4, 5, 8, 20, 25`| Tất cả`No`| Các lũy thừa và tích thuần túy của hai số nguyên tố cho phép | 
|`3, 6, 7, 12, 99`| Tất cả`Yes`| Sự hiện diện của thừa số nguyên tố bị cấm | 

## Vỏ cạnh 

Với (n=1), đầu vào là```
1
1
```Thuật toán đi vào cả hai vòng lặp loại bỏ yếu tố, nhưng không vòng lặp nào thực hiện vì 1 không chia hết cho 2 và 5. Giá trị cuối cùng đã là 1, vì vậy câu trả lời là`No`. Điều này xử lý chính xác thực tế rằng (1/1) là một số nguyên và có biểu diễn thập phân hữu hạn. 

Với (n=10), đầu vào là```
1
10
```Vòng lặp đầu tiên không làm gì vì 10 là số lẻ. Vòng lặp thứ hai chia 10 cho 5 và để lại 2, sau đó không chia 2 cho 5 nữa. Lúc đầu, điều này có thể dường như để lại một giá trị không phải là một, nhưng hệ số 2 vẫn phải được loại bỏ. Đây là lý do tại sao các vòng lặp trong thuật toán thực tế xử lý cả hai số nguyên tố một cách độc lập. Bắt đầu từ 10, vòng lặp 2 loại bỏ 2 trước, để lại 5 và vòng lặp 5 sau đó loại bỏ 5, đạt 1. Đầu ra là`No`. 

Với (n=6), đầu vào là```
1
6
```Vòng 2 thay đổi 6 thành 3. Vòng 5 không thể thay đổi 3. Giá trị còn lại là 3 nên đáp án là`Yes`. Đây là trường hợp đặc trưng khi loại bỏ tất cả các yếu tố được phép sẽ làm lộ ra yếu tố bị cấm. 

Với (n=100), đầu vào là```
1
100
```Mẫu số có thừa số là (100=2^2\cdot5^2). Thuật toán liên tục loại bỏ thừa số 2 cho đến khi đạt 25, sau đó liên tục loại bỏ thừa số 5 cho đến khi đạt 1. Câu trả lời là`No`, xác nhận rằng giá trị biên hoạt động chính xác giống như các mẫu số tận cùng nhỏ hơn.
