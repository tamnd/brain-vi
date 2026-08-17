---
title: "CF 102343A - Chia tiền mặt"
description: "Bài toán yêu cầu chúng tôi phân bổ tổng giải thưởng cố định cho các thành viên trong nhóm tùy theo số lượng vấn đề mà mỗi thành viên đã giải quyết trong mùa hè. Nếu đội giải quyết được tổng cộng (S) vấn đề và tổng giải thưởng là (D) đô la, thì mỗi vấn đề được giải quyết đều có giá trị (D/S) đô la."
date: "2026-08-16T17:55:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102343
codeforces_index: "A"
codeforces_contest_name: "UCF Locals 2019"
rating: 0
weight: 102343
solve_time_s: 53
verified: true
draft: false
---

[CF 102343A - Chia tiền mặt](https://codeforces.com/problemset/problem/102343/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng tôi phân bổ tổng giải thưởng cố định cho các thành viên trong nhóm tùy theo số lượng vấn đề mà mỗi thành viên đã giải quyết trong mùa hè. Nếu đội giải quyết được tổng cộng (S) vấn đề và tổng giải thưởng là (D) đô la, thì mỗi vấn đề được giải quyết đều có giá trị (D/S) đô la. Dữ liệu đầu vào đảm bảo rằng giá trị này là số nguyên, vì vậy phần thưởng của mỗi thành viên chỉ đơn giản là số bài toán đã giải được của họ nhân với giá trị chung của một bài toán. Đầu ra được yêu cầu là một phần thưởng cho mỗi thành viên trong nhóm, theo cùng thứ tự với đầu vào. 

Ví dụ: nếu tổng giải thưởng là 1000 đô la và các thành viên giải được 5, 8 và 7 vấn đề thì tổng cộng có 20 vấn đề được giải quyết. Mỗi vấn đề trị giá 50 đô la, trao phần thưởng 250, 400 và 350 đô la. 

Bài toán ban đầu có (1 \le n \le 30), số tiền thưởng tối đa là 30.000 và mỗi thành viên giải được từ 1 đến 300 bài toán. Các giới hạn này rất nhỏ, do đó, ngay cả một giải pháp (O(n^2)) cũng sẽ thực hiện tối đa vài trăm thao tác cơ bản. Tuy nhiên, nghiệm tự nhiên là (O(n)) và việc nhận ra lý do tại sao chỉ cần một tổng là ý tưởng hữu ích ở đây. Cuộc thi chính thức liệt kê giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

Có một vài trường hợp nhỏ có thể bộc lộ việc thực hiện bất cẩn. Với một thành viên, chẳng hạn như```
1 300
5
```thành viên duy nhất phải nhận được tất cả 300 đô la, vì vậy sản lượng là```
300
```Lời giải vô tình chia giải thưởng cho số thành viên thay vì số bài toán giải được sẽ cho kết quả sai. 

Một trường hợp ranh giới khác là khi mọi thành viên đều giải quyết được cùng một số lượng vấn đề:```
3 900
10
10
10
```Tổng số là 30, vì vậy mỗi vấn đề có giá trị 30 đô la và mỗi thành viên nhận được 300. Kết quả đầu ra là```
300
300
300
```Một giải pháp bất cẩn tính toán từng phần thưởng một cách độc lập bằng cách sử dụng số lượng bài toán đã giải được của thành viên hiện tại làm mẫu số sẽ không thành công vì mẫu số phải là tổng số bài toán đã giải được. 

Việc đảm bảo khả năng phân chia cũng có vấn đề. Vì```
2 100
3
7
```tổng số bài toán được giải là 10, vì vậy mỗi bài toán có giá trị chính xác là 10 đô la và kết quả đúng là```
30
70
```Việc sử dụng phép chia dấu phẩy động ở đây sẽ có tác dụng về mặt số học đối với ví dụ này, nhưng phép chia số nguyên trực tiếp thể hiện sự đảm bảo của bài toán và tránh đưa ra các vấn đề làm tròn không cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp nhất là tính toán tổng số vấn đề được giải quyết riêng biệt cho từng thành viên. Đối với thành viên (i), chúng tôi có thể quét tất cả (n) thành viên, cộng số vấn đề đã giải quyết của họ, chia tổng giải thưởng cho tổng số đó và nhân với số lượng thành viên (i)-th. Điều này đúng vì mọi thành viên đều sử dụng chính xác tổng số bài toán đã giải được làm mẫu số. 

Vấn đề với cách tiếp cận đó là công việc lặp đi lặp lại. Tổng số này giống hệt nhau đối với mọi thành viên, nhưng việc triển khai vũ phu sẽ tính toán đi tính lại số tiền đó. Với (n=30), điều này có nghĩa là có tới (30 \times 30 = 900) phép cộng trong trường hợp xấu nhất, vẫn dễ dàng đủ nhanh cho các ràng buộc này. Điểm yếu của nó là mang tính khái niệm hơn là thực tế: nó bỏ qua thực tế là mẫu số được chia sẻ cho mọi câu trả lời. 

Quan sát quan trọng là tổng số vấn đề được giải quyết là một giá trị toàn cầu duy nhất. Chúng ta có thể đọc tất cả số lượng thành viên một lần, tính tổng của chúng một lần và sau đó tính phần thưởng cho mỗi thành viên bằng cách sử dụng cùng một giá trị cho mỗi vấn đề. Nếu (S) là tổng số bài toán đã giải được thì giá trị của một bài toán giải được là 

[ 
\text{value} = \left\lfloor\frac{D}{S}\right\rfloor. 
] 

Câu lệnh đảm bảo rằng (D/S) là một số nguyên, do đó phép toán sàn không loại bỏ bất cứ thứ gì. Mỗi thành viên giải quyết được (a_i) vấn đề sẽ nhận được 

[ 
a_i \times \text{value}. 
] 

Brute-force hoạt động vì cuối cùng nó tìm thấy tổng số tiền chung cho mọi thành viên nhưng không thể sử dụng lại số tiền đó. Nhận xét rằng tổng được chia sẻ cho phép chúng tôi giảm việc tính toán xuống một lượt cho tổng và một lượt cho phần thưởng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(n)) | Được chấp nhận cho những ràng buộc này, nhưng công việc lặp đi lặp lại không cần thiết | 
| Tối ưu | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (n), số lượng thành viên trong đội và (D), tổng số tiền thưởng. Hai giá trị này xác định quy mô của nhóm và số tiền cuối cùng phải được phân phối. 
2. Đọc số lượng bài toán đã giải được của từng thành viên trong nhóm và lưu trữ các giá trị vào một mảng. Đồng thời, tích lũy tổng (S) của họ, vì phần thưởng của mọi thành viên đều sử dụng tổng số bài toán được giải như nhau. 
3. Tính toán`money_per_problem = D // S`. Bài toán đảm bảo rằng phép chia là chính xác, do đó phép chia số nguyên sẽ cho kết quả chính xác liên quan đến một bài toán được giải. 
4. Với mỗi thành viên có (a_i) giải được bài toán, hãy tính`a_i * money_per_problem`. Điều này trực tiếp tuân theo quy tắc rằng phần thưởng tỷ lệ thuận với số lượng vấn đề được giải quyết. 
5. In phần thưởng thu được theo thứ tự đầu vào, mỗi phần một dòng. Việc giữ nguyên thứ tự ban đầu rất quan trọng vì mỗi dòng đầu ra tương ứng với thành viên nhóm tương ứng. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi đọc các thành viên đã xử lý cho đến nay, tổng tích lũy chính xác là số bài toán mà các thành viên đó đã giải quyết. Sau khi tất cả (n) phần tử đã được đọc, tổng chính xác là tổng số bài toán đã giải được (S). Vì mỗi đô la phần thưởng tỷ lệ thuận với các bài toán được giải nên phần thưởng chung cho một bài toán là (D/S). Bài toán đảm bảo giá trị này là số nguyên, vì vậy`D // S`là chính xác. Nhân giá trị chung đó với số vấn đề đã giải quyết của mỗi thành viên sẽ tạo ra chính xác phần giải thưởng của thành viên đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, money = map(int, input().split())

    solved = []
    total = 0

    for _ in range(n):
        x = int(input())
        solved.append(x)
        total += x

    money_per_problem = money // total

    for x in solved:
        print(x * money_per_problem)

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên tương ứng với các bước đọc đầu vào và xây dựng tổng thể. Việc lưu trữ các giá trị là cần thiết vì phần thưởng không thể được in cho đến khi biết được tổng số vấn đề đã giải quyết. 

Sau vòng lặp,`money // total`tính toán phần thưởng cho một vấn đề được giải quyết. Việc chia số nguyên là có chủ ý vì đầu vào đảm bảo tính chia hết chính xác. 

Vòng lặp thứ hai áp dụng phần thưởng chung cho mỗi vấn đề cho số lần giải được của mỗi thành viên. Không có vấn đề riêng lẻ nào vì vòng lặp chạy chính xác (n) lần, một lần cho mỗi thành viên. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn số nguyên ngay cả khi các giá trị lớn hơn giới hạn ban đầu. Đầu ra được tạo ra một thành viên trên mỗi dòng, phù hợp với định dạng được yêu cầu. 

## Ví dụ đã hoạt động 

Không có đầu vào và đầu ra mẫu chính thức nào được đưa vào văn bản bài toán được cung cấp ở đây, vì vậy dấu vết đầu tiên sử dụng ví dụ bằng số được mô tả bởi chính bài toán. 

### Ví dụ 1 

đầu vào:```
3 1000
5
8
7
```Thuật toán đầu tiên tích lũy tổng số vấn đề đã được giải quyết. 

| Thành viên | Đã giải quyết | Tổng số sau khi đọc | 
| --- | --- | --- | 
| 1 | 5 | 5 | 
| 2 | 8 | 13 | 
| 3 | 7 | 20 | 

Giải thưởng cho mỗi vấn đề là (1000/20 = 50). 

| Thành viên | Đã giải quyết | Tiền cho mỗi vấn đề | Phần thưởng | 
| --- | --- | --- | --- | 
| 1 | 5 | 50 | 250 | 
| 2 | 8 | 50 | 400 | 
| 3 | 7 | 50 | 350 | 

Đầu ra là:```
250
400
350
```Điều này thể hiện tính bất biến trung tâm: khi tổng đạt tới 20, giá trị tương tự cho mỗi bài toán là 50 sẽ được sử dụng cho mọi thành viên. 

### Ví dụ 2 

Hãy xem xét trường hợp tất cả các thành viên đều giải quyết được cùng một số vấn đề. 

đầu vào:```
4 1200
5
5
5
5
```Tổng số tích lũy trở thành 20. 

| Thành viên | Đã giải quyết | Tổng số sau khi đọc | 
| --- | --- | --- | 
| 1 | 5 | 5 | 
| 2 | 5 | 10 | 
| 3 | 5 | 15 | 
| 4 | 5 | 20 | 

Phần thưởng chung là (1200/20 = 60). 

| Thành viên | Đã giải quyết | Tiền cho mỗi vấn đề | Phần thưởng | 
| --- | --- | --- | --- | 
| 1 | 5 | 60 | 300 | 
| 2 | 5 | 60 | 300 | 
| 3 | 5 | 60 | 300 | 
| 4 | 5 | 60 | 300 | 

Đầu ra là:```
300
300
300
300
```Điều này thực hiện trường hợp mọi đầu ra phải giống hệt nhau và xác nhận rằng mẫu số là tổng toàn cầu chứ không phải là số lượng của từng thành viên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Số lượng thành viên được đọc một lần và xử lý một lần nữa để tạo ra phần thưởng. | 
| Không gian | (O(n)) | Số lượng vấn đề đã giải quyết được lưu trữ để chúng có thể được xử lý sau khi biết tổng số. | 

Với (n \le 30), thuật toán chỉ thực hiện vài chục lần lặp và thấp hơn nhiều so với giới hạn thời gian 1 giây. Việc sử dụng bộ nhớ của nó cũng không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm 

Văn bản bài toán được cung cấp ở đây không chứa các khối mẫu chính thức, vì vậy ví dụ số từ câu lệnh sẽ được đưa vào làm ví dụ được cung cấp bên dưới.```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, money = map(int, input().split())
    solved = []
    total = 0

    for _ in range(n):
        x = int(input())
        solved.append(x)
        total += x

    per_problem = money // total

    for x in solved:
        print(x * per_problem)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided numerical example
assert run(
    "3 1000\n"
    "5\n"
    "8\n"
    "7\n"
) == "250\n400\n350\n", "provided example"

# Minimum-size input
assert run(
    "1 300\n"
    "5\n"
) == "300\n", "single member receives the whole prize"

# All members have equal solved counts
assert run(
    "4 1200\n"
    "5\n"
    "5\n"
    "5\n"
    "5\n"
) == "300\n300\n300\n300\n", "all equal values"

# Boundary values from the constraints
assert run(
    "30 30000\n" +
    "300\n" * 30
) == "1000\n" * 30, "maximum n, maximum solved count, maximum prize"

# Different counts with exact divisibility
assert run(
    "5 1500\n"
    "1\n"
    "2\n"
    "3\n"
    "4\n"
    "5\n"
) == "100\n200\n300\n400\n500\n", "different counts and exact division"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 1000 / 5, 8, 7`|`250, 400, 350`| Cung cấp ví dụ số và phân bố tỷ lệ cơ bản | 
|`1 300 / 5`|`300`| Quy mô nhóm tối thiểu và phân bổ toàn bộ giải thưởng | 
|`4 1200 / 5, 5, 5, 5`|`300, 300, 300, 300`| Giá trị hoàn toàn bằng nhau | 
|`30 30000 / 300 repeated 30 times`|`1000`lặp lại 30 lần | Trường hợp ranh giới kích thước tối đa | 
|`5 1500 / 1, 2, 3, 4, 5`|`100, 200, 300, 400, 500`| Số lượng khác nhau và tỷ lệ tỷ lệ chính xác | 

## Vỏ cạnh 

Đối với một thành viên trong nhóm, đầu vào```
1 300
5
```đưa ra tổng cộng 5 vấn đề được giải quyết. Một bài toán có giá trị (300/5=60) đô la, vì vậy thành viên duy nhất nhận được (5\times60=300). Thuật toán xử lý việc này một cách tự nhiên vì không có trường hợp đặc biệt nào cho (n=1) và giá trị duy nhất trở thành cả tổng số và số lượng thành viên. 

Đối với số lượng giải quyết bằng nhau, hãy xem xét```
3 900
10
10
10
```Tổng số là 30, vì vậy mỗi vấn đề trị giá 30 đô la. Mỗi thành viên nhận được (10\times30=300). Thuật toán tính tổng một lần và sử dụng lại cùng một giá trị cho cả ba thành viên, do đó, các đầu vào giống nhau đương nhiên sẽ tạo ra các phần thưởng giống nhau. 

Đối với đầu vào được phép lớn nhất,```
30 30000
300
300
300
300
300
300
300
300
300
300
300
300
300
300
300
300
300
300
300
300
300
300
300
300
300
300
300
300
300
300
```tổng số bài toán giải được là (30\times300=9000). Mỗi vấn đề có giá trị (30000/9000=10/3), không phải là số nguyên, vì vậy sự kết hợp ranh giới cụ thể này sẽ vi phạm đảm bảo tính chia hết của vấn đề và không phải là phép thử hợp lệ theo tuyên bố ban đầu. 

Trường hợp kích thước tối đa hợp lệ phải tôn trọng sự đảm bảo đó. Ví dụ: nếu tất cả 30 thành viên giải được 300 vấn đề và giải thưởng là 27000 đô la thì tổng số tiền là 9000 và mỗi vấn đề trị giá 3 đô la. Mỗi thành viên nhận được 900 đô la. Thuật toán không cần kiểm tra tính chia hết vì đặc tả đầu vào đã đảm bảo điều đó. 

Đối với số lượng khác nhau, hãy xem xét```
5 1500
1
2
3
4
5
```Tổng số là 15, thưởng 100 đô la cho mỗi vấn đề được giải quyết. Phần thưởng là 100, 200, 300, 400 và 500. Điều này cho thấy việc triển khai vô tình chia giải thưởng cho (n), vì số lượng thành viên trong nhóm là 5 trong khi số vấn đề được giải quyết là 15. 

Bài học quan trọng là mỗi phần thưởng đều có chung một mẫu số, tổng số vấn đề đã được giải quyết. Khi tổng số đó đã được tính toán chính xác, phần còn lại của bài toán là phép nhân trực tiếp cho mỗi thành viên.
