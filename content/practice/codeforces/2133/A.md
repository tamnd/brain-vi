---
title: "CF 2133A - Đá đỏ?"
description: "Chúng ta được yêu cầu quyết định xem liệu một chuỗi các bánh răng có thể được sắp xếp thành một đường thẳng sao cho việc quay bánh răng ngoài cùng bên trái với tốc độ một vòng/giây sẽ dẫn đến bánh răng ngoài cùng bên phải quay chính xác với tốc độ một vòng/giây hay không."
date: "2026-06-08T02:45:12+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2133
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1044 (Div. 2)"
rating: 800
weight: 2133
solve_time_s: 74
verified: true
draft: false
---

[CF 2133A - Đá đỏ?](https://codeforces.com/problemset/problem/2133/A) 

**Đánh giá:** 800 
**Tags:** Brute Force, cấu trúc dữ liệu, triển khai, toán học 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu quyết định xem liệu một chuỗi các bánh răng có thể được sắp xếp thành một đường thẳng sao cho việc quay bánh răng ngoài cùng bên trái với tốc độ một vòng/giây sẽ dẫn đến bánh răng ngoài cùng bên phải quay chính xác với tốc độ một vòng/giây hay không. Mỗi bánh răng có một số răng và tốc độ của bất kỳ bánh răng nào so với bánh răng lân cận tỷ lệ thuận với tỷ lệ các răng. Cụ thể, nếu một thiết bị có$y$răng truyền động một bánh răng với$x$răng quay với tốc độ$z$, bánh răng tiếp theo quay với tốc độ$z \cdot y / x$. Chúng tôi được cung cấp nhiều trường hợp thử nghiệm, mỗi trường hợp có tối đa 100 bánh răng và số lượng răng nằm trong khoảng từ 2 đến 100. 

Bài toán rút gọn về việc sắp xếp các số nguyên$a_1, \dots, a_n$sao cho tích của các tỉ số liên tiếp dọc theo đường thẳng bằng 1. Viết tích này một cách tường minh nếu dãy được sắp xếp là$b_1, b_2, \dots, b_n$, tốc độ bánh răng cuối cùng là$$1 \cdot \frac{b_1}{b_2} \cdot \frac{b_2}{b_3} \cdots \frac{b_{n-1}}{b_n} = \frac{b_1}{b_n}.$$Vì vậy, bánh răng ngoài cùng bên phải quay với tốc độ 1 khi và chỉ khi bánh răng đầu tiên và bánh răng cuối cùng có cùng số răng. Đây là sự đơn giản hóa quan trọng: tất cả các bánh răng trung gian đều bị loại bỏ. 

Vì điều này, vấn đề tương đương với việc kiểm tra xem có tồn tại số đếm bánh răng có thể được đặt ở cả hai đầu của chuỗi hay không. 

Các trường hợp cạnh bao gồm khi tất cả các bánh răng có cùng số răng, tức là "CÓ" hoặc khi không có số nào xuất hiện ít nhất hai lần, tức là "KHÔNG". Ví dụ, nếu các bánh răng`[2, 3]`, không có cách sắp xếp nào thỏa mãn yêu cầu. 

Các ràng buộc đủ nhỏ để cho phép đếm đơn giản: với$n$lên tới 100 và$t$lên tới 1000, một$O(n^2)$cách tiếp cận bạo lực là khả thi, nhưng việc đếm số lần xuất hiện là$O(n)$cho mỗi trường hợp thử nghiệm và sạch hơn nhiều. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ thử tất cả các hoán vị của các bánh răng và mô phỏng chuyển động quay của từng bánh răng. Vì$n = 100$, có$100!$hoán vị, vốn rất lớn và không thể thực hiện được. Ngay cả việc mô phỏng các tỷ số cho mỗi hoán vị cũng không cần thiết vì tốc độ cuối cùng chỉ phụ thuộc vào bánh răng đầu tiên và cuối cùng. 

Quan sát quan trọng là các tỷ số kính thiên văn: tích của các lần liên tiếp$\frac{y}{x}$đơn giản hóa để$\frac{b_1}{b_n}$. Điều này làm giảm vấn đề từ việc mô phỏng chuyển động của bánh răng sang việc kiểm tra đơn giản xem bánh răng đầu tiên và bánh răng cuối cùng có bằng nhau hay không. Do đó, giải pháp tối ưu là kiểm tra xem có số lượng răng nào xuất hiện ít nhất hai lần trong đầu vào hay không. Nếu được thì chúng ta có thể đặt nó ở hai đầu và sắp xếp các bánh răng còn lại tùy ý ở giữa. Nếu không thì không có sự sắp xếp nào đáp ứng được yêu cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng tất cả các hoán vị) | Ồ (n!) | O(n) | Quá chậm | 
| Đếm số lần xuất hiện, kiểm tra số lần trùng lặp | O(n) cho mỗi trường hợp thử nghiệm | O(100) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Mỗi trường hợp thử nghiệm sẽ được xử lý độc lập. 
2. Với mỗi test, đọc số bánh răng$n$và răng của họ được đếm trong một danh sách$a$. 
3. Đếm số lần xuất hiện của mỗi số răng bằng từ điển hoặc mảng. Điều này sẽ cho phép chúng tôi biết số nào xuất hiện nhiều lần. 
4. Kiểm tra xem có số lượng nào ít nhất là 2 hay không. Nếu số lượng đó tồn tại, hãy in "CÓ". Ngược lại in ra "KHÔNG". 
5. Tiếp tục test case tiếp theo cho đến khi tất cả được xử lý. 

Tại sao nó hoạt động: Tốc độ của bánh răng ngoài cùng bên phải chỉ phụ thuộc vào bánh răng đầu tiên và bánh răng cuối cùng trong cách sắp xếp vì các tỷ số trung gian bị triệt tiêu. Việc đặt một bánh răng lặp lại ở cả hai đầu đảm bảo rằng tích tỷ số bằng 1, thỏa mãn điều kiện. Không cần có sự sắp xếp nào khác và việc kiểm tra là đầy đủ và cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    gears = list(map(int, input().split()))
    counts = {}
    found = False
    for x in gears:
        counts[x] = counts.get(x, 0) + 1
        if counts[x] >= 2:
            found = True
            break
    print("YES" if found else "NO")
```Giải pháp đọc từng trường hợp thử nghiệm, đếm số lần xuất hiện của từng số răng bằng từ điển và ngắt sớm nếu bất kỳ số nào xuất hiện hai lần. Việc nghỉ sớm đảm bảo chúng tôi không xử lý các số còn lại một cách không cần thiết sau khi tìm ra giải pháp. Chúng tôi sử dụng`dict.get`để đơn giản hóa việc đếm tăng dần mà không cần kiểm tra có điều kiện. 

## Ví dụ đã hoạt động 

Đầu vào mẫu`[2, 5, 5]`: 

| Thiết bị | Đếm | Đã tìm thấy >=2? | 
| --- | --- | --- | 
| 5 | {5:1} | Không | 
| 5 | {5:2} | Có | 

Thuật toán in "CÓ". 

đầu vào`[2, 2, 3]`: 

| Thiết bị | Đếm | Đã tìm thấy >=2? | 
| --- | --- | --- | 
| 2 | {2:1} | Không | 
| 3 | {2:1, 3:1} | Không | 

Không có số nào xuất hiện hai lần nên thuật toán in ra "NO". 

Những dấu vết này cho thấy thuật toán xác định chính xác liệu có tồn tại một bản sao để đặt ở cả hai đầu hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Việc đếm số lần xuất hiện của mỗi bánh răng là tuyến tính theo số lượng bánh răng. | 
| Không gian | O(100) | Số lượng răng riêng biệt tối đa là 99 (2-100), do đó từ điển bị giới hạn. | 

Với$t \le 1000$Và$n \le 100$, điều này dẫn đến tối đa 100.000 thao tác, nằm trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        gears = list(map(int, input().split()))
        counts = {}
        found = False
        for x in gears:
            counts[x] = counts.get(x, 0) + 1
            if counts[x] >= 2:
                found = True
                break
        output.append("YES" if found else "NO")
    return "\n".join(output)

# provided samples
assert run("5\n2\n5 5\n4\n6 3 6 9\n2\n2 3\n7\n30 10 12 10 10 9 18\n5\n2 4 8 16 32\n") == "YES\nYES\nNO\nYES\nNO", "sample 1"

# custom cases
assert run("1\n2\n2 2\n") == "YES", "all equal"
assert run("1\n3\n2 3 4\n") == "NO", "all distinct, small n"
assert run("1\n4\n3 3 4 5\n") == "YES", "one duplicate in middle"
assert run("1\n100\n" + " ".join(str(i) for i in range(2, 102)) + "\n") == "NO", "large n, all distinct"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[2,2]`| CÓ | kích thước tối thiểu, tất cả đều bằng nhau | 
|`[2,3,4]`| KHÔNG | n nhỏ, tất cả đều khác biệt | 
|`[3,3,4,5]`| CÓ | một bản sao không ở cuối | 
|`[2..101]`| KHÔNG | n lớn, tất cả đều khác biệt | 

## Vỏ cạnh 

Thuật toán xử lý trường hợp tất cả các bánh răng đều giống hệt nhau bằng cách đếm và tìm ngay một bánh răng trùng lặp. Ví dụ,`[5, 5, 5]`mang lại "CÓ" vì 5 xuất hiện ba lần. Khi tất cả các bánh răng đều khác biệt, chẳng hạn như`[2, 3, 4]`, số lượng từ điển không hiển thị trùng lặp và thuật toán xuất ra "KHÔNG". Đầu vào có kích thước tối đa với 100 giá trị riêng biệt vẫn tạo ra "NO" một cách hiệu quả vì từ điển đếm vẫn còn nhỏ và dấu ngắt sớm không bao giờ được kích hoạt.
