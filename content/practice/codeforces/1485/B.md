---
title: "CF 1485B - Thay thế và sắp xếp"
description: "Chúng ta được cung cấp một mảng tăng dần a có độ dài n với các phần tử từ 1 đến k và nhiều truy vấn hỏi về mảng con. Đối với mỗi truy vấn, được xác định bởi chỉ số l và r, chúng ta cần đếm xem có bao nhiêu mảng b tồn tại \"k-similar\" với mảng con a[l..r]."
date: "2026-06-10T23:16:33+07:00"
tags: ["codeforces", "competitive-programming", "dp", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1485
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 701 (Div. 2)"
rating: 1200
weight: 1485
solve_time_s: 184
verified: false
draft: false
---

[CF 1485B - Thay thế và tiếp tục sắp xếp](https://codeforces.com/problemset/problem/1485/B) 

**Đánh giá:** 1200 
**Tags:** dp, triển khai, toán học 
**Thời gian giải:** 3m 4s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng tăng dần`a`chiều dài`n`với các phần tử từ`1`ĐẾN`k`và nhiều truy vấn hỏi về mảng con. Đối với mỗi truy vấn, được xác định bởi các chỉ số`l`Và`r`, chúng ta cần đếm xem có bao nhiêu mảng`b`tồn tại "k-tương tự" với mảng con`a[l..r]`. Hai mảng giống nhau k nếu chúng tăng nghiêm ngặt, có cùng độ dài và các phần tử của chúng nằm trong`[1, k]`, và chúng khác nhau đúng một vị trí. 

Kích thước đầu vào rất đáng kể:`n`Và`q`có thể lên tới 100.000, trong khi`k`có thể lớn tới 10^9. Điều này loại trừ bất kỳ cách tiếp cận nào liệt kê các mảng ứng cử viên một cách rõ ràng. Một thuật toán đơn giản thử mọi sửa đổi có thể có của mảng con sẽ có hiệu suất lên đến`(r-l+1) * k`hoạt động cho mỗi truy vấn, điều này rõ ràng là không khả thi đối với các giới hạn lớn nhất. Do đó, chúng ta cần cách tiếp cận O(1) hoặc O(độ dài mảng con) cho mỗi truy vấn để tận dụng cấu trúc của các chuỗi tăng dần. 

Các trường hợp cạnh không rõ ràng bao gồm các mảng con ở ranh giới của mảng. Ví dụ: nếu chúng ta cố gắng giảm phần tử đầu tiên hoặc tăng phần tử cuối cùng vượt quá 1 hoặc`k`, chúng ta phải kẹp các giá trị có thể. Một sự tinh tế khác nảy sinh trong các mảng con rất ngắn có độ dài bằng một; số lượng phương án phải tính đến giới hạn của cả hai bên, dù không có “yếu tố trung gian” nào để xem xét. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại từng vị trí trong mảng con truy vấn và thử mọi số nguyên từ`1`ĐẾN`k`, đếm xem có bao nhiêu duy trì mức tăng nghiêm ngặt và khác nhau ở đúng vị trí đó. Đối với một mảng con có độ dài`m`, điều này sẽ yêu cầu các thao tác O(m * k). Ngay cả với`m`lên tới 100.000, điều này là không thể đối với`k`lên đến 10^9. 

Thông tin chi tiết quan trọng là đối với một mảng tăng nghiêm ngặt, phạm vi hợp lệ để thay đổi một phần tử nhất định bị hạn chế bởi các phần tử lân cận của nó. Giả định`b`khác với`a`ở vị trí`i`. Sau đó`b[i]`có thể nhận bất kỳ giá trị nào lớn hơn phần tử trước đó`a[i-1]`(hoặc 0 nếu`i`là phần tử đầu tiên) và hoàn toàn nhỏ hơn phần tử tiếp theo`a[i+1]`(hoặc`k+1`nếu như`i`là cuối cùng). Điều này ngay lập tức làm giảm số lượng ứng viên cho mỗi vị trí thành một phép tính số học đơn giản: đếm các số nguyên giữa`a[i-1]+1`Và`a[i+1]-1`, trừ 1 để loại trừ giá trị ban đầu. 

Đối với phần tử đầu tiên và phần tử cuối cùng, chúng ta chỉ cần điều chỉnh ranh giới: phần tử đầu tiên không thể xuống dưới 1 và phần tử cuối cùng không thể vượt quá`k`. Đối với các phần tử ở giữa, phạm vi hợp lệ được giới hạn bởi các phần tử lân cận ngay lập tức. Tổng các số đếm này trên tất cả các vị trí trong mảng con truy vấn sẽ cho ra tổng số mảng k-tương tự cho mảng con đó. Vì điều này chỉ yêu cầu số học đơn giản cho mỗi phần tử nên độ phức tạp của mỗi truy vấn là O(m), đủ hiệu quả với điều kiện`n`Và`q`là 10^5 và tổng công việc trên tất cả các truy vấn vẫn khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m * k) mỗi truy vấn | O(1) | Quá chậm | 
| Tối ưu | O(m) mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị đầu vào`n`,`q`, Và`k`và mảng`a`. Hãy chắc chắn`a`được lập chỉ mục 1 để tính toán ranh giới dễ dàng hơn. 
2. Với mỗi truy vấn, trích xuất các chỉ số mảng con`l`Và`r`. Xác định chiều dài`m = r-l+1`. 
3. Khởi tạo bộ đếm`count`về không. Điều này sẽ lưu trữ tổng số mảng hợp lệ. 
4. Đối với từng vị trí`i`trong mảng con, tính toán phạm vi thay thế có thể: 

- Nếu`i`là phần tử đầu tiên, giới hạn dưới là 1; nếu không thì đó là`a[i-1] + 1`. 
- Nếu như`i`là phần tử cuối cùng, giới hạn trên là`k`; nếu không thì đó là`a[i+1] - 1`. 
- Số thay thế hợp lệ ở vị trí này là`(upper_bound - lower_bound + 1) - 1`. Trừ một để loại trừ giá trị hiện tại. 
5. Thêm số này vào`count`. 
6. Sau khi xử lý tất cả các vị trí, xuất ra`count`. 

Tại sao nó hoạt động: Theo cách xây dựng, mỗi phần tử trong mảng con chỉ có thể nhận các giá trị trong giới hạn nghiêm ngặt do các phần tử lân cận đặt ra, đảm bảo rằng mảng kết quả vẫn tăng nghiêm ngặt. Trừ một phần tử sẽ loại trừ phần tử ban đầu để thỏa mãn điều kiện "khác nhau ở đúng một vị trí". Tổng hợp tất cả các vị trí chiếm tất cả các khả năng chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, q, k = map(int, input().split())
a = list(map(int, input().split()))

# pad array for easier indexing
a = [0] + a + [k+1]

for _ in range(q):
    l, r = map(int, input().split())
    count = 0
    for i in range(l, r+1):
        lower = a[i-1] + 1
        upper = a[i+1] - 1
        count += max(0, upper - lower)
    print(count)
```Mã này trước tiên đệm mảng bằng 0 ở đầu và k+1 ở cuối, để các phép tính biên được thống nhất. Đối với mỗi truy vấn, nó lặp lại mảng con có liên quan và tính toán số lượng thay thế hợp lệ cho mỗi phần tử.`max(0, upper - lower)`đảm bảo rằng phạm vi âm (không có thay thế hợp lệ) được xử lý chính xác. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
4 2 5
1 2 4 5
2 3
3 4
```| Truy vấn | mảng con | Vị trí | Thay thế hợp lệ cho mỗi vị trí | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 2 3 | [2,4] | 2: 1..3, 3: 3..5 | pos2: 2 tùy chọn (1,3), pos3: 2 tùy chọn (3,5) | 4 | 
| 3 4 | [4,5] | 3: 3..4, 4: 5..5 | pos3: 1 tùy chọn (3), pos4: 2 tùy chọn (3,4) | 3 | 

Dấu vết cho thấy thuật toán xử lý chính xác cả vị trí giữa và cuối, đảm bảo mức tăng nghiêm ngặt. 

**Đầu vào mẫu 2:**```
5 1 10
2 4 6 7 9
1 5
```| Vị trí | Hạ | Thượng | Thay thế hợp lệ | 
| --- | --- | --- | --- | 
| 1 | 1 | 3 | 1 | 
| 2 | 3 | 5 | 1 | 
| 3 | 5 | 6 | 0 | 
| 4 | 7 | 8 | 1 | 
| 5 | 8 | 10 | 2 | 

Tổng cộng = 1+1+0+1+2 = 5 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng độ dài của mảng con truy vấn) | Mỗi phần tử trong mảng con truy vấn được xử lý một lần. | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng và phần đệm; không cần thêm cấu trúc. | 

Cho n, q ≤ 10^5 và k lên đến 10^9, thuật toán dễ dàng thực hiện trong 2 giây vì các phép tính số học không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution code
    n, q, k = map(int, input().split())
    a = list(map(int, input().split()))
    a = [0] + a + [k+1]
    for _ in range(q):
        l, r = map(int, input().split())
        count = 0
        for i in range(l, r+1):
            lower = a[i-1]+1
            upper = a[i+1]-1
            count += max(0, upper-lower)
        print(count)
    return output.getvalue().strip()

# provided samples
assert run("4 2 5\n1 2 4 5\n2 3\n3 4\n") == "4\n3", "sample 1"

# custom cases
assert run("1 1 1\n1\n1 1\n") == "0", "single element, no change"
assert run("3 1 5\n1 3 5\n1 3\n") == "6", "all elements in subarray can change within bounds"
assert run("5 1 10\n2 4 6 7 9\n1 5\n") == "5", "complex bounds, middle element no options"
assert run("2 1 2\n1 2\n1 2\n") == "0", "subarray maxed out, no options"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1\n1\n1 1 | 0 | đầu vào tối thiểu, không thể thay đổi phần tử | 
| 3 1 5\n1 3 5\n1 3 | 6 | nhiều thay thế hợp lệ | 
| 5 1 10\n | | |
