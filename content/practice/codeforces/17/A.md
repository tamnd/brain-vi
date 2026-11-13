---
title: "CF 17A - Bài toán Noldbach"
description: "Cho ta hai số nguyên n và k. Chúng tôi xem xét tất cả các số nguyên tố từ 2 đến n. Trong số các số nguyên tố đó, chúng ta muốn đếm xem có bao nhiêu số có thể viết được dưới dạng:"
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 17
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 17"
rating: 1000
weight: 17
solve_time_s: 106
verified: true
draft: false
---
[CF 17A - Vấn đề Noldbach](https://codeforces.com/problemset/problem/17/A) 

**Đánh giá:** 1000 
**Tags:** lực lượng vũ phu, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cho ta hai số nguyên`n`Và`k`. Chúng tôi xem xét tất cả các số nguyên tố từ`2`lên đến`n`. Trong số các số nguyên tố đó, chúng ta muốn đếm xem có bao nhiêu số có thể viết được dưới dạng:$$p_i + p_{i+1} + 1$$Ở đâu`p_i`Và`p_{i+1}`là các số nguyên tố lân cận, nghĩa là các số nguyên tố liên tiếp theo thứ tự tăng dần. 

Ví dụ: các số nguyên tố liên tiếp`5`Và`7`sản xuất:$$5 + 7 + 1 = 13$$Từ`13`cũng là số nguyên tố, nó có giá trị. 

Nhiệm vụ là xác định xem ít nhất`k`số nguyên tố lên đến`n`thỏa mãn tính chất này. Nếu có thì in`YES`, nếu không thì in`NO`. 

Những hạn chế là rất nhỏ.`n`nhiều nhất là`1000`, do đó, ngay cả các phương pháp tạo số nguyên tố khá kém hiệu quả cũng có thể được chấp nhận. Một thuật toán bậc hai trên tất cả các số lên đến`1000`vẫn chạy ngay lập tức. Điều này thay đổi trọng tâm của vấn đề từ tối ưu hóa sang tính chính xác và triển khai rõ ràng. 

Phần khó khăn là diễn giải điều kiện một cách chính xác. Chúng ta không kiểm tra xem một số có thể được viết dưới dạng tổng của hai số nguyên tố bất kỳ cộng một hay không. Hai số nguyên tố phải liên tiếp trong danh sách các số nguyên tố có thứ tự. 

Hãy xem xét đầu vào này:```
27 2
```Các công trình hợp lệ là:$$5 + 7 + 1 = 13$$Và$$7 + 11 + 1 = 19$$Cả hai`13`Và`19`là số nguyên tố và nhiều nhất`27`, vậy câu trả lời là`YES`. 

Một lỗi phổ biến là cho phép các số nguyên tố không lân cận. Ví dụ:```
20 1
```Việc triển khai bất cẩn có thể sử dụng:$$3 + 13 + 1 = 17$$và kết luận rằng`17`tính. Điều đó sai vì`3`Và`13`không phải là các số nguyên tố lân cận. Các cặp lân cận đúng là`(2,3)`,`(3,5)`,`(5,7)`,`(7,11)`, vân vân. 

Một sai lầm dễ mắc phải khác là quên rằng bản thân số được tạo ra cũng phải là số nguyên tố và không được vượt quá`n`. 

Ví dụ:```
10 1
```Sử dụng các số nguyên tố lân cận:$$2 + 3 + 1 = 6$$

`6`không phải là số nguyên tố nên không được tính.$$3 + 5 + 1 = 9$$

`9`cũng không phải là nguyên tố. 

Không có số hợp lệ nào tồn tại nên câu trả lời đúng là`NO`. 

Một trường hợp nữa là`k = 0`. Trong tình huống đó câu trả lời luôn là`YES`, bởi vì việc có ít nhất 0 số nguyên tố hợp lệ sẽ tự động đúng. 

Ví dụ:```
2 0
```Đầu ra đúng là:```
YES
```Một giải pháp chỉ kiểm tra xem có tìm thấy một số số hợp lệ hay không và có thể in sai`NO`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp nhất là kiểm tra mọi số từ`2`ĐẾN`n`và thử tất cả các cặp số nguyên tố để xem liệu số đó có thể được biểu diễn dưới dạng:$$p_i + p_j + 1$$đồng thời kiểm tra xem các số nguyên tố có lân cận nhau hay không. 

Vì có khoảng`168`số nguyên tố lên đến`1000`, cách tiếp cận này vẫn đủ nhỏ để vượt qua. Công việc trong trường hợp xấu nhất là xung quanh:$$1000 \times 168^2$$đó chỉ là một vài triệu hoạt động. 

Phương pháp vũ phu hoạt động vì các ràng buộc rất nhỏ nhưng nó thực hiện những công việc không cần thiết. Biểu thức mà chúng ta quan tâm hoàn toàn được xác định bởi các số nguyên tố liên tiếp. Khi đã biết danh sách các số nguyên tố, mọi ứng cử viên hợp lệ chỉ đơn giản là:$$p_i + p_{i+1} + 1$$Không có lý do để kiểm tra sự kết hợp tùy ý. 

Quan sát này đơn giản hóa vấn đề một cách đáng kể. Đầu tiên chúng ta tạo ra tất cả các số nguyên tố lên đến`n`. Sau đó, chúng tôi lặp qua các cặp số nguyên tố liên tiếp, tính giá trị ứng viên và kiểm tra xem ứng viên đó có phải là số nguyên tố hay không và nhiều nhất là`n`. 

Để kiểm tra tính nguyên tố nhanh chóng, chúng ta lưu trữ tất cả các số nguyên tố trong một tập hợp. Sau đó việc kiểm tra tư cách thành viên trở thành thời gian không đổi. 

Cấu trúc của bài toán đương nhiên dẫn đến lời giải này vì điều kiện đã chỉ rõ các số nguyên tố liên tiếp. Thay vì tìm kiếm các cách biểu diễn số, chúng ta trực tiếp tạo ra tất cả các cách biểu diễn có thể có. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(P2) | O(P) | Đã chấp nhận | 
| Tối ưu | O(n log log n) | O(n) | Đã chấp nhận | 

Đây`P`là số số nguyên tố lên đến`n`. 

## Hướng dẫn thuật toán 

1. Tạo tất cả các số nguyên tố từ`2`ĐẾN`n`bằng cách sử dụng sàng Eratosthenes. 

Sàng đơn giản và đáng tin cậy đối với những ràng buộc nhỏ. Nó cũng cung cấp khả năng tra cứu nguyên thủy nhanh chóng sau này. 
2. Lưu trữ tất cả các số nguyên tố được tạo trong cả danh sách và tập hợp. 

Danh sách duy trì thứ tự các số nguyên tố cần thiết để truy cập các số nguyên tố lân cận. Bộ này cho phép kiểm tra liên tục xem một số có phải là số nguyên tố hay không. 
3. Lặp lại các cặp số nguyên tố liên tiếp. 

Đối với mỗi chỉ số`i`, lấy:$$primes[i] \text{ and } primes[i+1]$$4. Tính toán:$$candidate = primes[i] + primes[i+1] + 1$$Đây chính xác là hình thức được yêu cầu bởi vấn đề. 
5. Kiểm tra xem ứng viên có nằm trong tập nguyên tố và không vượt quá`n`. 

Nếu cả hai điều kiện giữ nguyên, hãy tăng số lượng. 
6. Sau khi xử lý tất cả các cặp lân cận, so sánh số đếm với`k`. 

Nếu số lượng ít nhất là`k`, in`YES`. Nếu không thì in`NO`. 

### Tại sao nó hoạt động 

Thuật toán kiểm tra mọi biểu thức có thể được định nghĩa bài toán cho phép, bởi vì mọi số hợp lệ phải đến từ chính xác một cặp số nguyên tố lân cận. Không có ứng cử viên hợp lệ nào bị bỏ qua. 

Đối với mỗi cặp liên tiếp, chúng tôi tính toán số duy nhất mà cặp đó có thể tạo ra. Việc kiểm tra tư cách thành viên trong tập hợp số nguyên tố đảm bảo rằng chúng ta chỉ tính các số nguyên tố. Vì mọi giá trị được đếm đều thỏa mãn kết cấu được yêu cầu và mọi kết cấu có thể có đều được kiểm tra nên số đếm cuối cùng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())

    is_prime = [True] * (n + 1)

    if n >= 0:
        is_prime[0] = False
    if n >= 1:
        is_prime[1] = False

    for i in range(2, int(n ** 0.5) + 1):
        if is_prime[i]:
            for j in range(i * i, n + 1, i):
                is_prime[j] = False

    primes = []
    for i in range(2, n + 1):
        if is_prime[i]:
            primes.append(i)

    prime_set = set(primes)

    count = 0

    for i in range(len(primes) - 1):
        candidate = primes[i] + primes[i + 1] + 1

        if candidate <= n and candidate in prime_set:
            count += 1

    print("YES" if count >= k else "NO")

solve()
```Phần đầu tiên xây dựng sàng.`is_prime[x]`cho chúng ta biết liệu`x`là nguyên tố. Bắt đầu từ`i * i`tránh được công việc dư thừa vì bội số nhỏ hơn đã được xử lý bởi các số nguyên tố nhỏ hơn. 

Sau khi sàng, chúng tôi thu thập tất cả các số nguyên tố vào một danh sách. Thứ tự của chúng có ý nghĩa quan trọng vì bài toán đặc biệt yêu cầu các số nguyên tố lân cận. 

Phiên bản tập hợp của các số nguyên tố được sử dụng để tra cứu nhanh. Nếu không có nó, việc kiểm tra xem một ứng cử viên có phải là số nguyên tố hay không sẽ yêu cầu các tìm kiếm tuyến tính lặp đi lặp lại hoặc các bài kiểm tra tính nguyên tố lặp đi lặp lại. 

Vòng lặp trên các cặp liên tiếp là cốt lõi của giải pháp. sử dụng`range(len(primes) - 1)`ngăn truy cập ngoài cuối danh sách khi đọc`primes[i + 1]`. 

Điều kiện:```
candidate <= n and candidate in prime_set
```là quan trọng. Ngay cả khi số được tạo là số nguyên tố, nó chỉ được tính nếu nằm trong phạm vi cho phép. 

Cuối cùng, chúng tôi so sánh số lượng với`k`và in câu trả lời cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
27 2
```Số nguyên tố được tạo:$$[2, 3, 5, 7, 11, 13, 17, 19, 23]$$| Các số nguyên tố lân cận | Ứng viên | Xuất sắc? | Đếm | 
| --- | --- | --- | --- | 
| 2, 3 | 6 | Không | 0 | 
| 3, 5 | 9 | Không | 0 | 
| 5, 7 | 13 | Có | 1 | 
| 7, 11 | 19 | Có | 2 | 
| 11, 13 | 25 | Không | 2 | 
| 13, 17 | 31 | Quá lớn | 2 | 
| 17, 19 | 37 | Quá lớn | 2 | 
| 19, 23 | 43 | Quá lớn | 2 | 

Số cuối cùng là`2`, ít nhất là`k = 2`, do đó đầu ra là:```
YES
```Dấu vết này cho thấy cách chỉ xem xét các số nguyên tố lân cận và cách các ứng cử viên lớn hơn`n`bị bỏ qua. 

### Ví dụ 2 

đầu vào:```
45 7
```Số nguyên tố lên tới`45`:$$[2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43]$$| Các số nguyên tố lân cận | Ứng viên | Xuất sắc? | Đếm | 
| --- | --- | --- | --- | 
| 2, 3 | 6 | Không | 0 | 
| 3, 5 | 9 | Không | 0 | 
| 5, 7 | 13 | Có | 1 | 
| 7, 11 | 19 | Có | 2 | 
| 11, 13 | 25 | Không | 2 | 
| 13, 17 | 31 | Có | 3 | 
| 17, 19 | 37 | Có | 4 | 
| 19, 23 | 43 | Có | 5 | 
| 23, 29 | 53 | Quá lớn | 5 | 

Chỉ một`5`số nguyên tố hợp lệ tồn tại, nhưng`k = 7`, vậy đáp án là:```
NO
```Ví dụ này chứng tỏ rằng số đếm có thể ngừng tăng rất lâu trước khi tất cả các số nguyên tố được xử lý. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log log n) | Sàng Eratosthenes thống trị thời gian chạy | 
| Không gian | O(n) | Mảng và bộ lưu trữ thông tin nguyên thủy | 

Với`n ≤ 1000`, thời gian chạy rất nhỏ. Việc sàng và tất cả các vòng lặp gần như hoàn thành ngay lập tức trong thời gian giới hạn và mức sử dụng bộ nhớ không đáng kể so với mức sẵn có.`64 MB`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, k = map(int, input().split())

    is_prime = [True] * (n + 1)

    if n >= 0:
        is_prime[0] = False
    if n >= 1:
        is_prime[1] = False

    for i in range(2, int(n ** 0.5) + 1):
        if is_prime[i]:
            for j in range(i * i, n + 1, i):
                is_prime[j] = False

    primes = []

    for i in range(2, n + 1):
        if is_prime[i]:
            primes.append(i)

    prime_set = set(primes)

    count = 0

    for i in range(len(primes) - 1):
        candidate = primes[i] + primes[i + 1] + 1

        if candidate <= n and candidate in prime_set:
            count += 1

    print("YES" if count >= k else "NO")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue().strip()

# provided samples
assert run("27 2\n") == "YES", "sample 1"
assert run("45 7\n") == "NO", "sample 2"

# minimum input
assert run("2 0\n") == "YES", "k=0 should always work"

# no valid constructions
assert run("10 1\n") == "NO", "no prime fits the condition"

# exactly one valid prime
assert run("13 1\n") == "YES", "13 = 5 + 7 + 1"

# boundary around valid count
assert run("19 2\n") == "YES", "13 and 19 both count"

# impossible large k
assert run("1000 1000\n") == "NO", "count can never reach 1000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 0`|`YES`| Tay cầm`k = 0`chính xác | 
|`10 1`|`NO`| Từ chối ứng cử viên không chính thức | 
|`13 1`|`YES`| Phát hiện một công trình hợp lệ | 
|`19 2`|`YES`| Đếm nhiều cặp lân cận hợp lệ | 
|`1000 1000`|`NO`| Xử lý các mục tiêu lớn không thể | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
20 1
```Thuật toán tạo ra các cặp số nguyên tố lân cận:$$(2,3), (3,5), (5,7), (7,11), \dots$$Ứng cử viên của họ là:$$6, 9, 13, 19, \dots$$

`13`Và`19`hợp lệ vì chúng là số nguyên tố và nhiều nhất`20`. 

Thuật toán không bao giờ xem xét các biểu thức như:$$3 + 13 + 1 = 17$$bởi vì`3`Và`13`không phải là các số nguyên tố lân cận. Điều này ngăn chặn việc đếm quá mức. 

Bây giờ hãy xem xét:```
10 1
```Các ứng cử viên là:$$2 + 3 + 1 = 6$$Và$$3 + 5 + 1 = 9$$Không phải là số nguyên tố. Tra cứu tập hợp nguyên tố từ chối chính xác cả hai giá trị, do đó số lượng vẫn còn`0`và đầu ra trở thành`NO`. 

Cuối cùng, hãy xem xét:```
2 0
```Danh sách nguyên tố chỉ chứa`[2]`, do đó không có cặp lân cận nào cả. Số lượng ở lại`0`. 

Kiểm tra so sánh cuối cùng:$$0 \ge 0$$điều đó đúng, vì vậy thuật toán in ra`YES`. Điều này xử lý logic trường hợp trống một cách chính xác mà không cần bất kỳ sự phân nhánh đặc biệt nào.
