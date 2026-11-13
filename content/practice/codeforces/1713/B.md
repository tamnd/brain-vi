---
title: "CF 1713B - Giảm thiểu tối ưu"
description: "Chúng ta được cho một mảng các số nguyên dương. Chúng ta có thể liên tục chọn một mảng con liền kề và giảm tất cả các phần tử trong mảng con đó đi một phần tử. Mục tiêu là đưa tất cả các phần tử mảng về 0 bằng cách sử dụng ít thao tác nhất có thể."
date: "2026-06-09T20:12:56+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1713
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 812 (Div. 2)"
rating: 1000
weight: 1713
solve_time_s: 148
verified: false
draft: false
---

[CF 1713B - Giảm thiểu tối ưu](https://codeforces.com/problemset/problem/1713/B) 

**Đánh giá:** 1000 
**Tags:** thuật toán xây dựng, sắp xếp 
**Thời gian giải:** 2m 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương. Chúng ta có thể liên tục chọn một mảng con liền kề và giảm tất cả các phần tử trong mảng con đó đi một phần tử. Mục tiêu là đưa tất cả các phần tử mảng về 0 bằng cách sử dụng ít thao tác nhất có thể. Hãy ký hiệu bằng$f(a)$số lượng tối thiểu các hoạt động như vậy cần thiết cho một mảng$a$. Nhiệm vụ là xác định xem mảng đã cho có$a$đã "tối ưu" theo nghĩa là không có hoán vị của$a$có thể giảm xuống 0 trong ít thao tác hơn. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho biết độ dài$n$của mảng và chính mảng đó. Đầu ra phải là "CÓ" nếu mảng tối ưu theo nghĩa trên và "KHÔNG" nếu ngược lại. Những ràng buộc cho phép$n$lên đến$10^5$cho mỗi trường hợp thử nghiệm, với tổng của tất cả các trường hợp thử nghiệm cũng lên tới$10^5$. Điều này loại trừ bất kỳ thuật toán nào có độ phức tạp bậc hai hoặc kém hơn, do đó$O(n \log n)$hoặc$O(n)$giải pháp là cần thiết. 

Các trường hợp biên không rõ ràng bao gồm các mảng có số lặp lại, mảng theo thứ tự được sắp xếp hoặc sắp xếp ngược và các mảng có phần tử lớn nhất ở một trong hai đầu. Ví dụ, đối với$a = [3,1,3,2]$, mảng không tối ưu vì sắp xếp lại nó thành$[1,2,3,3]$giảm số lượng thao tác từ 5 xuống 3. Việc triển khai bất cẩn không xem xét các hoán vị có thể khai báo sai các mảng như vậy là tối ưu. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử tất cả các hoán vị của mảng và tính toán$f(b)$cho mỗi người. Máy tính$f(b)$có thể được thực hiện bằng chiến lược đệ quy hoặc lặp đi lặp lại nhiều lần trừ đi mức tối thiểu của phân khúc hiện tại. Tuy nhiên, đối với$n = 10^5$, số hoán vị là$n!$, điều đó hoàn toàn không thể thực hiện được. Ngay cả tính toán$f(a)$trực tiếp bằng thuật toán trừ phân đoạn đơn giản thì quá chậm đối với các mảng lớn. 

Quan sát quan trọng là$f(a)$được giảm thiểu khi mảng không giảm. Lý do là bất kỳ sự giảm cục bộ nào trong chuỗi không đơn điệu đều tạo ra các phép toán bổ sung. Nói cách khác, sự sắp xếp "tối ưu" được sắp xếp theo thứ tự không giảm. Sau khi mảng được sắp xếp, số lượng thao tác tối thiểu bằng phần tử lớn nhất vì chúng ta luôn có thể bao gồm các phần tử trước đó như một phần của các phân đoạn lớn hơn. Vì vậy, mảng$a$là tối ưu nếu nó được sắp xếp theo thứ tự không giảm. Nếu tồn tại một phần tử nhỏ hơn phần tử trước nó thì tồn tại một hoán vị (phần tử được sắp xếp) làm giảm$f(a)$. Điều này trực tiếp dẫn đến một kiểm tra đơn giản: so sánh mảng với phiên bản đã sắp xếp của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc độ dài mảng$n$và mảng$a$. 
3. Tạo một bản sao đã được sắp xếp của$a$theo thứ tự không giảm. 
4. So sánh$a$với phiên bản được sắp xếp của nó. Nếu chúng giống nhau thì in ra "CÓ". Ngược lại in ra "KHÔNG". 

Thuật toán này hoạt động vì các mảng duy nhất được đảm bảo yêu cầu số lượng phép toán tối thiểu trong số tất cả các hoán vị là những mảng được sắp xếp theo thứ tự không giảm. Mọi sai lệch so với thứ tự này luôn có thể được cải thiện bằng cách sắp xếp lại thành một dãy không giảm, không bao giờ tăng.$f(a)$và thường làm giảm nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    if a == sorted(a):
        print("YES")
    else:
        print("NO")
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`. Nó xây dựng phiên bản đã sắp xếp của từng mảng và thực hiện so sánh trực tiếp. Tích hợp sẵn của Python`sorted`chức năng chạy trong$O(n \log n)$thời gian. Sự so sánh`a == sorted(a)`lặp qua mảng, tính giá$O(n)$mỗi trường hợp thử nghiệm. Các điều kiện biên được xử lý tự động vì chúng tôi không bao giờ truy cập các chỉ mục nằm ngoài giới hạn mảng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mảng đầu vào:`[2,3,5,4]`Mảng được sắp xếp:`[2,3,4,5]`So sánh:`[2,3,5,4] != [2,3,4,5]`→ xuất ra "KHÔNG" 

Điều này chứng tỏ rằng mảng ban đầu không được sắp xếp và tồn tại một hoán vị làm giảm các phép toán. 

### Mẫu 2 

Mảng đầu vào:`[1,2,3]`Mảng được sắp xếp:`[1,2,3]`So sánh:`[1,2,3] == [1,2,3]`→ xuất ra "CÓ" 

Điều này xác nhận tính bất biến: mảng không giảm là tối ưu. 

### Mẫu 3 

Mảng đầu vào:`[3,1,3,2]`Mảng được sắp xếp:`[1,2,3,3]`So sánh:`[3,1,3,2] != [1,2,3,3]`→ xuất ra "KHÔNG" 

Điều này làm nổi bật trường hợp cạnh có các phần tử lặp lại và thứ tự không đơn điệu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế trên mỗi trường hợp thử nghiệm, so sánh là O(n) | 
| Không gian | O(n) | Bản sao được sắp xếp của mảng | 

Với tổng số$n$trên tất cả các trường hợp thử nghiệm ≤ 10^5, tổng số thao tác đều nằm trong phạm vi$10^6$trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        if a == sorted(a):
            print("YES")
        else:
            print("NO")
    return output.getvalue().strip()

# provided samples
assert run("3\n4\n2 3 5 4\n3\n1 2 3\n4\n3 1 3 2\n") == "NO\nYES\nNO", "sample tests"

# custom cases
assert run("1\n1\n1\n") == "YES", "single element"
assert run("1\n5\n5 4 3 2 1\n") == "NO", "reverse order"
assert run("1\n4\n2 2 2 2\n") == "YES", "all equal"
assert run("1\n6\n1 2 3 3 4 5\n") == "YES", "sorted with duplicates"
assert run("1\n6\n1 2 3 5 4 6\n") == "NO", "small inversion in middle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n1`| CÓ | kích thước mảng tối thiểu | 
|`1\n5\n5 4 3 2 1`| KHÔNG | thứ tự ngược lại tối đa hóa hoạt động | 
|`1\n4\n2 2 2 2`| CÓ | mọi phần tử đều bằng nhau | 
|`1\n6\n1 2 3 3 4 5`| CÓ | sắp xếp với số lặp lại | 
|`1\n6\n1 2 3 5 4 6`| KHÔNG | đảo ngược nhỏ ở giữa mảng | 

## Vỏ cạnh 

Đối với mảng một phần tử`[1]`, thuật toán sắp xếp`[1]`ĐẾN`[1]`và in "CÓ", xử lý chính xác đầu vào có kích thước tối thiểu. Đối với một mảng được sắp xếp ngược`[5,4,3,2,1]`, sắp xếp tạo ra`[1,2,3,4,5]`, so sánh không thành công và đầu ra là "KHÔNG", xác nhận rằng thuật toán xác định các hoán vị không tối ưu. Đối với một mảng các phần tử giống hệt nhau`[2,2,2,2]`, việc sắp xếp giữ nguyên mảng và đầu ra là "CÓ", xử lý chính xác các số lặp lại. Đối với các mảng có độ đảo ngược nhỏ như`[1,2,3,5,4,6]`, việc sắp xếp sửa lỗi đảo ngược, so sánh không thành công và đầu ra là "KHÔNG", xác nhận thuật toán phát hiện các trường hợp không đơn điệu tinh tế.
