---
title: "CF 1845A - Số nguyên bị cấm"
description: "Chúng ta được yêu cầu xác định xem liệu tổng mục tiêu n có thể được hình thành bằng cách sử dụng bất kỳ số nguyên nào từ 1 đến k, ngoại trừ một số nguyên bị cấm duy nhất x hay không. Nếu có thể, chúng ta phải xây dựng một dãy số nguyên hợp lệ có tổng bằng n."
date: "2026-06-09T05:58:56+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1845
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 151 (Rated for Div. 2)"
rating: 800
weight: 1845
solve_time_s: 313
verified: false
draft: false
---

[CF 1845A - Số nguyên bị cấm](https://codeforces.com/problemset/problem/1845/A) 

**Đánh giá:** 800 
**Tas:** xây dựng thuật toán, thực hiện, toán, lý thuyết số 
**Thời gian giải:** 5 phút 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định xem tổng mục tiêu có`n`có thể được hình thành bằng cách sử dụng bất kỳ số nguyên nào từ`1`ĐẾN`k`, không bao gồm một số nguyên bị cấm`x`. Nếu có thể, chúng ta phải xây dựng một dãy số nguyên hợp lệ có tổng bằng chính xác`n`. Mỗi trường hợp thử nghiệm cung cấp ba số nguyên:`n`(tổng mục tiêu),`k`(số nguyên lớn nhất hiện có) và`x`(số nguyên bị cấm). 

Các hạn chế là nhỏ:`n`,`k`, Và`x`tối đa là 100 và có thể có tới 100 trường hợp thử nghiệm. Điều này cho phép các thuật toán có độ phức tạp xấp xỉ O(n2), nhưng nên sử dụng các cách tiếp cận mang tính xây dựng O(n) hoặc O(1) đơn giản hơn. Bởi vì chúng ta được phép sao chép không giới hạn mỗi số được phép, nên vấn đề giảm xuống còn việc tìm nhiều tập hợp số từ tập hợp đó.`[1, k] \ {x}`tổng cộng là`n`. 

Trường hợp biên xuất hiện khi số nguyên bị cấm`x`là rất nhỏ hoặc rất lớn. Ví dụ, nếu`x = 1`Và`n = 1`, ta không thể tính tổng vì số nhỏ nhất cho phép là 2. Tương tự, nếu`x = k = 1`, tập hợp các số nguyên có thể sử dụng là trống và mọi số nguyên dương`n`là không thể. Một sự tinh tế khác là nếu`n`nhỏ hơn số được phép nhỏ nhất (ví dụ:`n = 2`Nhưng`1`bị cấm), không thể đạt được mục tiêu. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ thử tất cả các tổ hợp số nguyên từ`1`ĐẾN`k`loại trừ`x`. Điều này có thể được thực hiện đệ quy hoặc bằng lập trình động. Độ phức tạp về thời gian của nó cao, có khả năng là O(n·k) và không cần thiết đối với các`n`. 

Một quan sát quan trọng giúp đơn giản hóa vấn đề: vì chúng ta có thể sử dụng số lượng số nguyên được phép không giới hạn nên chúng ta chỉ cần kiểm tra xem liệu`n`có thể được hình thành bằng cách sử dụng các bản sao lặp lại của số nguyên nhỏ nhất được phép. Nếu như`1`không bị cấm, chúng ta luôn có thể hình thành`n`bằng cách lấy`n`bản sao của`1`. Nếu như`1`bị cấm nhưng`2`được cho phép,`n`ít nhất phải có`2`và có thể được viết dưới dạng tổng của hai (hoặc kết hợp của hai và ba nếu có). Bản chất mang tính xây dựng cho phép chúng ta xuất ra một chuỗi tham lam đơn giản: điền càng nhiều càng tốt với số nguyên nhỏ nhất được phép, sau đó điều chỉnh với số nhỏ nhất tiếp theo nếu cần. 

Thông tin chi tiết này giúp giảm vấn đề thành một kiểm tra đơn giản trên số nguyên nhỏ nhất được phép và sau đó xây dựng tổng trực tiếp, mang lại thời gian O(n) cho mỗi trường hợp thử nghiệm, quá đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·k) | O(n) | Hoạt động nhưng không cần thiết | 
| Tham lam/Xây dựng | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định số nguyên nhỏ nhất cho phép`a`. Đây là 1 (nếu`x != 1`) hoặc 2 (nếu`x = 1`Và`k >= 2`). Nếu như`x = 1`Và`k = 1`, không cho phép số nguyên và câu trả lời là "KHÔNG". 
2. Xác định xem`n`có thể truy cập bằng cách sử dụng`a`. Nếu như`n < a`, in "KHÔNG" vì chúng tôi không thể tạo`n`với số nguyên lớn hơn`n`. Nếu không, tiến hành xây dựng trình tự. 
3. Xây dựng trình tự một cách tham lam. Nếu như`a = 1`, lấy`n`bản sao của 1. Nếu`a = 2`, Và`n`là chẵn, lấy`n / 2`bản sao của 2. Nếu`n`thật kỳ quặc và`k >= 3`Và`3`được phép, lấy một 3 và`(n - 3)/2`bản sao của 2. Nếu không thì không thể. 
4. Xuất ra "YES", tổng số số nguyên trong dãy và chính dãy đó. 

Tại sao nó hoạt động: các số nguyên nhỏ nhất được phép chiếm ưu thế trong cách xây dựng tổng vì chúng mang lại sự linh hoạt nhất. Bất kỳ số tiền nào`n`đó ít nhất là số nguyên được phép nhỏ nhất có thể được xây dựng bằng cách sử dụng bội số của số nguyên đó, với những điều chỉnh nhỏ bằng cách sử dụng số nguyên được phép nhỏ nhất tiếp theo nếu`n`không chia hết cho số nguyên nhỏ nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k, x = map(int, input().split())
    
    if k == 1 and x == 1:
        print("NO")
        continue
    
    if x != 1:
        # 1 is allowed
        print("YES")
        print(n)
        print("1 " * n)
    else:
        # 1 is forbidden, use 2 if possible
        if n == 1:
            print("NO")
        elif n % 2 == 0:
            print("YES")
            print(n // 2)
            print("2 " * (n // 2))
        elif k >= 3:
            print("YES")
            count = 1 + (n - 3) // 2
            print(count)
            print("3 " + "2 " * ((n - 3) // 2))
        else:
            print("NO")
```Giải pháp đầu tiên xử lý trường hợp suy biến khi không cho phép số. Sau đó nó kiểm tra xem`1`có thể sử dụng được, có thể xây dựng ngay. Nếu như`1`bị cấm, giải pháp sử dụng`2`thậm chí`n`Và`3 + 2`kỳ quặc`n`khi có thể. Điều này phù hợp với logic mang tính xây dựng được mô tả ở trên. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`n = 10, k = 3, x = 2`| Bước | Số nguyên được phép | Xây dựng trình tự | Trình tự | 
| --- | --- | --- | --- | 
| Xác định nhỏ nhất | 1 | được phép 1 | - | 
| Xây dựng trình tự | n=10 | Mất mười 1 giây | 1 1 1 1 1 1 1 1 1 1 | 

Đầu ra:```
YES
10
1 1 1 1 1 1 1 1 1 1
```**Ví dụ 2:**`n = 5, k = 2, x = 1`| Bước | Số nguyên được phép | Xây dựng trình tự | Trình tự | 
| --- | --- | --- | --- | 
| Xác định nhỏ nhất | 2 | 1 cấm, dùng 2 | n=5 lẻ, k<3 không điều chỉnh được | 
| Kết quả | không thể | - | - | 

Đầu ra:```
NO
```Những dấu vết này cho thấy thuật toán xử lý chính xác cả trường hợp có thể và không thể, bao gồm cả tổng nhỏ và số nguyên bị cấm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Việc xây dựng chuỗi có thể cần tới n số nguyên trong trường hợp xấu nhất | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Lưu trữ trình tự đầu ra | 

Với`n <= 100`Và`t <= 100`, thời gian trong trường hợp xấu nhất là 10.000 thao tác, dễ dàng phù hợp với giới hạn 2 giây. Bộ nhớ cũng ở mức tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    exec(open('solution.py').read())  # assume solution is in solution.py
    return out.getvalue().strip()

# provided samples
assert run("5\n10 3 2\n5 2 1\n4 2 1\n7 7 3\n6 1 1\n") == \
"""YES
10
1 1 1 1 1 1 1 1 1 1
NO
YES
2
2 2
YES
1
7
NO""", "Sample 1"

# custom cases
assert run("1\n1 1 1\n") == "NO", "Minimum input, forbidden 1"
assert run("1\n2 2 1\n") == "YES\n1\n2", "n even, 1 forbidden, 2 allowed"
assert run("1\n3 3 1\n") == "YES\n2\n3 2", "n odd, 1 forbidden, 2 and 3 allowed"
assert run("1\n5 5 2\n") == "YES\n5\n1 1 1 1 1", "1 allowed, simple sum"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | KHÔNG | Không có sẵn số nguyên | 
| 2 2 1 | CÓ 2 | Sử dụng 2 khi 1 bị cấm, thậm chí n | 
| 3 3 1 | CÓ 2 | Sử dụng 3 + 2 khi 1 bị cấm, lẻ n | 
| 5 5 2 | CÓ 5 | Sử dụng trực tiếp số nguyên nhỏ nhất được phép | 

## Vỏ cạnh 

Nếu số được phép duy nhất là 2 và`n`là số lẻ, thuật toán sẽ đưa ra "KHÔNG" chính xác vì không có cách nào tính tổng một số lẻ chỉ bằng 2 giây. Ví dụ,`n = 5, k = 2, x = 1`mang lại "KHÔNG". Nếu như`1`được phép, thậm chí lớn`n`được xây dựng tầm thường bằng cách sử dụng`n`bản sao của 1. Nếu`n`bằng số nguyên bị cấm nhưng có sẵn các số nguyên khác, thuật toán chọn số nguyên nhỏ nhất cho phép, đảm bảo có thể đạt được tổng mà không cần sử dụng số bị cấm.
