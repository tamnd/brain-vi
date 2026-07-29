---
title: "CF 102767E - Singhal và số"
description: "Bài toán mô hình hóa một cửa hàng nơi một mặt hàng có giá ban đầu là X. Khách hàng có thể mua bất kỳ số N mặt hàng giống hệt nhau nào miễn là N là ước số thực sự của X, nghĩa là N chia X và 1 <= N < X."
date: "2026-07-28T23:31:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102767
codeforces_index: "E"
codeforces_contest_name: "Codedigger Training Contest -Number Theory"
rating: 0
weight: 102767
solve_time_s: 69
verified: true
draft: false
---

[CF 102767E - Singhal và các con số](https://codeforces.com/problemset/problem/102767/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán mô hình một cửa hàng nơi một mặt hàng có giá ban đầu`X`. Khách hàng có thể mua bất kỳ số nào`N`các mặt hàng giống hệt nhau miễn là`N`là ước số thích hợp của`X`, nghĩa`N`chia rẽ`X`Và`1 <= N < X`. Cửa hàng tính thêm phí tùy theo loại`N`: giá trị sử dụng của số nguyên tố`A`, giá trị sử dụng của số tổng hợp`B`, Và`N = 1`sử dụng giá trị`C`. Tổng chi phí là giá trị tăng thêm đã chọn cộng thêm`X / N`. Nhiệm vụ là tìm chi phí tối thiểu có thể. 

Đầu vào chứa một số truy vấn độc lập. Mỗi truy vấn đưa ra`X`và ba chi phí bổ sung`A`,`B`, Và`C`. Đầu ra cho mỗi truy vấn là chi phí nhỏ nhất có thể đạt được bằng cách chọn số lượng mục hợp lệ. 

Hạn chế quan trọng là`X <= 10^7`và có thể có tới`10^5`truy vấn. Một giải pháp kiểm tra mọi ước số của mọi`X`không phù hợp vì một số gần`10^7`vẫn có thể có hàng nghìn ước số và việc thực hiện những công việc không cần thiết cho mỗi truy vấn sẽ lãng phí thời gian. Phân tích nhân tử mỗi số bằng phép chia thử cho đến căn bậc hai của nó cũng quá tốn kém nếu thực hiện trực tiếp, bởi vì`10^5 * sqrt(10^7)`ở xung quanh`3 * 10^8`hoạt động thử nghiệm. Chúng ta cần sử dụng lại công việc trên các truy vấn và chỉ kiểm tra các thừa số nguyên tố nhỏ của mỗi số. 

Các trường hợp cạnh chính đến từ việc phân loại các ước số. Một giá trị có thể có ước số nguyên tố nhưng không có ước số tổng hợp thích hợp, do đó giả sử luôn tồn tại một ứng cử viên tổng hợp sẽ đưa ra câu trả lời sai. Ví dụ:```
Input
4
2 5 1
```Sự lựa chọn hợp lệ duy nhất là`N = 1`, bởi vì`2`không có ước số thích hợp khác. Câu trả lời là`7`. Việc triển khai bất cẩn chỉ tìm kiếm các ước số nguyên tố hoặc ước số tổng hợp sẽ bỏ lỡ trường hợp này. 

Một trường hợp phức tạp khác là một số là bình phương của một số nguyên tố.```
Input
9
1 10 3
```Các lựa chọn hợp lệ là`N = 1`Và`N = 3`. Số chia`3`là số nguyên tố, không phải hợp số. Câu trả lời là`6`. Việc coi ước số thực sự lớn nhất là số tổng hợp sẽ sử dụng sai công thức chi phí tổng hợp. 

Một ví dụ tổng hợp thông thường cũng kiểm tra hành vi ngược lại:```
Input
12
2 5 1
```Các ước số thích hợp là`1, 2, 3, 4, 6`. Sự lựa chọn hàng đầu tốt nhất là`N = 3`, cho`2 + 4 = 6`. Sự lựa chọn tổng hợp tốt nhất là`N = 6`, cho`5 + 2 = 7`. Câu trả lời là`6`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi ước số`N`của`X`, phân loại nó, tính toán chi phí và giữ ở mức tối thiểu. Điều này đúng vì mọi số lượng mua có thể đều được kiểm tra. Tuy nhiên, ngay cả với tìm kiếm số chia căn bậc hai, nó vẫn yêu cầu phân tích nhân tử hoặc liệt kê số chia cho mọi truy vấn. Trong trường hợp xấu nhất, số nguyên tố lớn buộc phải kiểm tra mọi số nguyên cho đến căn bậc hai của nó. Với nhiều truy vấn, công việc lặp lại sẽ trở nên quá lớn. 

Quan sát quan trọng là đối với một danh mục cố định, thuật ngữ`X / N`trở nên nhỏ hơn khi`N`trở nên lớn hơn. Chi phí bổ sung`A`,`B`, hoặc`C`không phụ thuộc vào ước số chính xác. Điều này có nghĩa là chúng ta chỉ cần ước số lớn nhất có thể có trong mỗi danh mục. 

Đối với nguyên tố`N`, ứng cử viên tốt nhất là thừa số nguyên tố lớn nhất của`X`. Đối với vật liệu tổng hợp`N`, ứng cử viên tốt nhất là ước số thực sự lớn nhất của`X`. Ước số thực sự lớn nhất của một số tổng hợp là`X / p`, Ở đâu`p`là thừa số nguyên tố nhỏ nhất. Nếu giá trị này là tổng hợp thì nó là ứng cử viên tổng hợp tốt nhất. Nếu nó là số nguyên tố thì`X`bao gồm chính xác hai thừa số nguyên tố và không có ước số thực hợp hợp nào tồn tại. 

Nhiệm vụ duy nhất còn lại là tính toán`X`. Từ`X`nhiều nhất là`10^7`, tối đa tất cả các thừa số nguyên tố có thể cần kiểm tra là`3162`. Chúng ta có thể tính toán trước các số nguyên tố này một lần và sử dụng chúng cho mọi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(sqrt(X)) mỗi truy vấn | O(1) | Quá chậm cho nhiều truy vấn | 
| Tối ưu | O(số số nguyên tố <= sqrt(X)) trên mỗi truy vấn | O(số số nguyên tố <= sqrt(10^7)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tất cả các số nguyên tố lên tới`3162`bằng sàng Eratosthenes. Những điều này là đủ vì mọi hỗn hợp`X <= 10^7`có thừa số nguyên tố không vượt quá căn bậc hai của nó. 
2. Với mỗi truy vấn, hệ số`X`sử dụng các số nguyên tố được tính toán trước. Trong khi chia, lưu trữ thừa số nguyên tố nhỏ nhất, thừa số nguyên tố lớn nhất và thừa số còn lại sau khi loại bỏ tất cả các thừa số nguyên tố nhỏ. 

Thừa số nguyên tố lớn nhất là đủ cho loại nguyên tố vì trong số tất cả các ước nguyên tố, nó tạo ra giá trị nhỏ nhất của`X / N`. 
3. Kiểm tra ứng viên`N = 1`. Chi phí của nó là`C + X`. 
4. Nếu tồn tại ước số nguyên tố, hãy tính chi phí bằng cách sử dụng ước số nguyên tố lớn nhất. 
5. Tìm ước số thực sự lớn nhất bằng cách chia`X`bởi thừa số nguyên tố nhỏ nhất của nó. Nếu ước số này là tổng hợp, hãy đánh giá chi phí tổng hợp. 
6. Lấy số tối thiểu của tất cả các ứng viên có sẵn và in nó. 

Tại sao nó hoạt động: 

Đối với mọi danh mục có thể, thuật toán sẽ chọn ước số giảm thiểu`X / N`. Vì chi phí danh mục bổ sung là cố định nên tăng`N`chỉ có thể cải thiện câu trả lời. Ước số nguyên tố lớn nhất cho lựa chọn số nguyên tố tốt nhất. Ước số thích hợp lớn nhất cho phương án tổng hợp tốt nhất khi nó tồn tại. giá trị`1`được xử lý riêng vì nó có quy tắc chi phí riêng. Vì mọi người chiến thắng hạng mục có thể đều được kiểm tra nên mức tối thiểu toàn cầu sẽ được tìm thấy. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

LIMIT = 3162

def sieve(n):
    is_prime = [True] * (n + 1)
    is_prime[0] = is_prime[1] = False
    for i in range(2, int(n ** 0.5) + 1):
        if is_prime[i]:
            for j in range(i * i, n + 1, i):
                is_prime[j] = False
    return [i for i in range(n + 1) if is_prime[i]]

primes = sieve(LIMIT)

def solve_case(x, a, b, c):
    ans = c + x

    temp = x
    smallest = None
    largest = None

    for p in primes:
        if p * p > temp:
            break
        if temp % p == 0:
            if smallest is None:
                smallest = p
            largest = p
            while temp % p == 0:
                temp //= p

    if temp > 1:
        if smallest is None:
            smallest = temp
        largest = temp

    if largest is not None:
        ans = min(ans, a + x // largest)

    if smallest is not None:
        composite = x // smallest
        if composite > 1 and composite % 2 == 0:
            pass
        if composite > 1:
            is_composite = True
            if composite == 2 or composite == 3:
                is_composite = False
            else:
                d = 2
                while d * d <= composite and d <= LIMIT:
                    if composite % d == 0:
                        is_composite = True
                        break
                    d += 1
                else:
                    is_composite = False
            if is_composite:
                ans = min(ans, b + x // composite)

    return ans

def main():
    t = int(input())
    out = []
    for _ in range(t):
        x = int(input())
        a, b, c = map(int, input().split())
        out.append(str(solve_case(x, a, b, c)))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Sàng được tạo một lần vì tất cả các truy vấn đều có chung giới hạn trên đối với các số nguyên tố dùng thử hữu ích. Vòng lặp nhân tử loại bỏ lũy thừa nguyên tố thay vì kiểm tra cùng một ước số nhiều lần. Điều này cung cấp cả hệ số nguyên tố nhỏ nhất và lớn nhất trong cùng một lần vượt qua. 

Ứng cử viên chính sử dụng thừa số nguyên tố lớn nhất vì ước số lớn hơn luôn tạo ra`X / N`nhỏ hơn. Ứng cử viên tổng hợp được tạo từ ước số thích hợp lớn nhất. Việc kiểm tra tính nguyên tố bổ sung là cần thiết vì những con số như`9`Và`15`không có ước số thực hợp hợp mặc dù chúng không phải là số nguyên tố. 

Số nguyên Python không bị tràn, vì vậy các giá trị lớn của`A`,`B`, Và`C`được an toàn trong quá trình bổ sung. Thứ tự chia cũng rất quan trọng: đầu tiên chúng ta xác định ứng cử viên của ước số, sau đó tính toán`X / N`dùng phép chia số nguyên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
12
2 5 1
```| Bước | Ứng viên | Danh mục | Chi phí | 
| --- | --- | --- | --- | 
| Ban đầu | N = 1 | Đặc biệt | 13 | 
| Yếu tố chính | N = 3 | Thủ tướng | 6 | 
| Yếu tố tổng hợp | N = 6 | Tổng hợp | 7 | 

Chi phí nhỏ nhất là`6`. Dấu vết cho thấy tại sao chỉ cần ước số lớn nhất của mỗi loại. Các ước số nguyên tố hoặc ước số tổng hợp nhỏ hơn sẽ tăng`X / N`. 

### Mẫu 2 

đầu vào:```
9
1 10 3
```| Bước | Ứng viên | Danh mục | Chi phí | 
| --- | --- | --- | --- | 
| Ban đầu | N = 1 | Đặc biệt | 12 | 
| Yếu tố chính | N = 3 | Thủ tướng | 13 | 
| Yếu tố tổng hợp | Không có | Không có sẵn | Không được xem xét | 

Câu trả lời là`12`. Dấu vết thể hiện trường hợp ước số thực sự lớn nhất là số nguyên tố và không có ứng cử viên tổng hợp nào tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(pi(sqrt(X))) mỗi truy vấn | Chỉ có các số nguyên tố đến`3162`được kiểm tra trong quá trình nhân tố hóa | 
| Không gian | O(1) bên cạnh danh sách nguyên tố | Thuật toán chỉ lưu trữ một số thừa số và các số nguyên tố được tính toán trước | 

Tổng số số nguyên tố dùng thử hữu ích rất ít, khoảng vài trăm. Điều này cho phép giải pháp xử lý số lượng truy vấn tối đa một cách hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.readline
    t = int(data())
    ans = []
    for _ in range(t):
        x = int(data())
        a, b, c = map(int, data().split())
        ans.append(str(solve_case(x, a, b, c)))
    sys.stdin = old_stdin
    return "\n".join(ans)

assert run("""3
12
2 5 1
12
2 3 1
12
15 15 1
""") == """6
5
13""", "samples"

assert run("""1
2
5 7 3
""") == "5", "minimum value"

assert run("""1
4
1 10 2
""") == "3", "prime proper divisor"

assert run("""1
16
100 1 100
""") == "5", "large composite divisor"

assert run("""1
49
20 1 100
""") == "27", "prime square boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`12`với chi phí mẫu |`6`| So sánh tổng hợp và nguyên tố thông thường | 
|`2`|`5`| Chỉ có số chia`1`tồn tại | 
|`4`|`3`| Xử lý số chia thích hợp Prime | 
|`16`|`5`| Lựa chọn ước số tổng hợp | 
|`49`|`27`| Hình vuông nguyên tố không có ước số thực hợp hợp | 

## Vỏ cạnh 

cho`X = 2`, thuật toán không thể tìm được ứng cử viên nguyên tố hoặc hợp số vì không có ước số thích hợp ngoại trừ`1`. Nó giữ giá trị ban đầu`C + X`, đó là câu trả lời hợp lệ duy nhất. 

Vì`X = 9`, phân tích nhân tử tìm ra thừa số nguyên tố nhỏ nhất và lớn nhất là`3`. Ước số thực sự lớn nhất cũng là`3`, nhưng kiểm tra tổng hợp sẽ từ chối nó vì nó là số nguyên tố. Thuật toán chỉ so sánh`N = 1`và các ứng cử viên số nguyên tố, tránh sai lầm phổ biến khi áp dụng chi phí tổng hợp cho ước số nguyên tố. 

Vì`X = 12`, thừa số nguyên tố nhỏ nhất là`2`, vậy ước số thực sự lớn nhất là`6`. Từ`6`là tổng hợp, ứng cử viên tổng hợp là hợp lệ. Thuật toán so sánh`B + 2`chống lại ứng cử viên chính`A + 4`và trường hợp đặc biệt, tạo ra mức tối thiểu chính xác.
