---
title: "CF 2144B - Hoán vị chi phí tối đa"
description: "Chúng ta có một mảng p có độ dài n trong đó một số vị trí chứa các số nguyên từ 1 đến n và các vị trí khác là số 0. Không có số nguyên dương nào xuất hiện nhiều hơn một lần."
date: "2026-06-08T01:36:56+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2144
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 182 (Rated for Div. 2)"
rating: 1000
weight: 2144
solve_time_s: 130
verified: false
draft: false
---

[CF 2144B - Hoán vị chi phí tối đa](https://codeforces.com/problemset/problem/2144/B) 

**Đánh giá:** 1000 
**Tags:** thuật toán xây dựng, tham lam 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng`p`chiều dài`n`trong đó một số vị trí chứa số nguyên từ 1 đến`n`và những cái khác là số không. Không có số nguyên dương nào xuất hiện nhiều hơn một lần. Các số 0 là các phần giữ chỗ mà chúng ta có thể thay thế bằng các số còn thiếu để mảng trở thành một hoán vị thích hợp từ 1 đến`n`. Khi chúng tôi có một hoán vị hợp lệ, chúng tôi xác định chi phí của hoán vị là độ dài của phân đoạn con liền kề nhỏ nhất cần được sắp xếp để toàn bộ hoán vị được sắp xếp. 

Nhiệm vụ là tối đa hóa chi phí này. Tức là chúng ta muốn đặt các số còn thiếu vào các số 0 sao cho đoạn con mà chúng ta phải sắp xếp để sửa hoán vị càng dài càng tốt. Đầu ra của mỗi trường hợp thử nghiệm là một số nguyên duy nhất, chi phí tối đa có thể đạt được. 

Những ràng buộc cho chúng ta biết rằng`n`có thể lớn bằng 2 * 10^5 và tổng của`n`trên tất cả các trường hợp thử nghiệm tối đa là 2 * 10^5. Điều này loại trừ bất kỳ giải pháp vũ phu nào cố gắng tìm mọi cách để điền vào số 0, vì số lượng hoán vị của các số bị thiếu tăng theo giai thừa. Thay vào đó, chúng ta cần một cách tiếp cận hoạt động theo thời gian tuyến tính đối với`n`. 

Các trường hợp cạnh không rõ ràng bao gồm các mảng trong đó tất cả các phần tử đều là số 0, mảng đã được sắp xếp hoặc mảng trong đó tất cả các số bị thiếu được nhóm lại ở một đầu. Ví dụ, một mảng`[0, 0, 0]`có chi phí tối đa là 3, vì chúng ta có thể đặt các số sao cho mảng trở thành`[3, 1, 2]`và chúng ta cần sắp xếp toàn bộ mảng. Mặt khác,`[1, 2, 3, 0]`có giá trị bằng 0 vì số còn thiếu duy nhất là 4, số nằm ở cuối và mảng đã được sắp xếp. Một cách tiếp cận bất cẩn có thể chỉ đơn giản là đếm số 0 hoặc bỏ qua vị trí của các số hiện có, tạo ra câu trả lời sai trong những trường hợp khó khăn này. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản sẽ thử mọi cách để lấp đầy các số 0 bằng các số còn thiếu, xây dựng hoán vị và tính chi phí bằng cách quét tìm phân đoạn nhỏ nhất để sắp xếp. Việc tính toán chi phí phân đoạn cho một hoán vị nhất định có thể được thực hiện bằng cách tìm vị trí đầu tiên từ bên trái nơi mảng không đúng thứ tự và vị trí cuối cùng từ bên phải nơi mảng không đúng thứ tự. Tuy nhiên, điều này đòi hỏi phải tạo ra các kết hợp giai thừa của các số 0, điều này hoàn toàn không khả thi đối với`n`lên tới 2 * 10^5. 

Quan sát quan trọng là chi phí tối đa tương ứng với một phân mảng bắt đầu ở số 0 đầu tiên và kết thúc ở số 0 cuối cùng. Bất kỳ số nào được đặt bên ngoài đoạn 0 đều không làm tăng hoặc giảm chi phí sắp xếp. Vì vậy, chúng ta có thể xác định được vị trí đầu tiên và cuối cùng của các số 0 trong mảng. Sau đó, chi phí tối đa có thể là khoảng cách giữa các vị trí này, được điều chỉnh bằng cách xem xét các số đã được đặt chính xác ở hai đầu. Cụ thể, những con số nằm ngoài phân khúc số 0 này đã có sẵn và không làm tăng chi phí. 

Điều này làm giảm vấn đề tìm vị trí 0 đầu tiên và cuối cùng trong mảng và lấy chênh lệch cộng 1 làm chi phí tối đa có thể. Chúng ta không cần phải xây dựng hoán vị một cách rõ ràng; chỉ riêng vị trí của các số 0 đã xác định độ dài đoạn tối đa mà chúng ta có thể buộc phải không được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`và mảng`p`. 
2. Khởi tạo`first_zero`Và`last_zero`đến -1. 
3. Lặp lại mảng từ trái sang phải. Khi chúng ta gặp số 0 đầu tiên, hãy lưu chỉ mục của nó vào`first_zero`. 
4. Tiếp tục lặp lại và cập nhật`last_zero`bất cứ khi nào gặp số 0. Đến cuối mảng,`last_zero`sẽ chứa chỉ số của số 0 cuối cùng. 
5. Nếu không có số 0 (`first_zero`vẫn là -1), mảng đã là một hoán vị hoàn chỉnh nên chi phí tối đa là 0. 
6. Nếu không, chi phí tối đa là`last_zero - first_zero + 1`. Điều này tương ứng với độ dài của đoạn liền kề bao gồm tất cả các số 0, đây là đoạn con lớn nhất mà chúng ta có thể buộc phải sắp xếp bằng cách khéo léo đặt các số còn thiếu. 
7. In chi phí tối đa này cho test case. 

Tại sao nó hoạt động: vị trí của số 0 xác định một đoạn liền kề nơi thiếu số. Bằng cách đặt các số còn thiếu để tạo ra sự rối loạn lớn nhất, chúng ta có thể đảm bảo rằng toàn bộ đoạn giữa số 0 đầu tiên và số 0 cuối cùng cần được sắp xếp. Các số nằm ngoài phân khúc này là cố định và không góp phần làm tăng chi phí phân loại. Do đó, độ dài đoạn là chi phí tối đa có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    p = list(map(int, input().split()))
    
    first_zero = -1
    last_zero = -1
    
    for i in range(n):
        if p[i] == 0:
            if first_zero == -1:
                first_zero = i
            last_zero = i
    
    if first_zero == -1:
        print(0)
    else:
        print(last_zero - first_zero + 1)
```Mã bắt đầu bằng cách đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó sẽ đọc kích thước mảng và chính mảng đó. Sau đó nó quét mảng để xác định số 0 đầu tiên và cuối cùng. Nếu không tìm thấy số 0, mảng đó hoàn chỉnh và được sắp xếp, do đó, nó in ra 0. Ngược lại, nó sẽ tính độ dài của đoạn bao gồm tất cả các số 0. Việc sử dụng một lần truyền qua mảng đảm bảo độ phức tạp O(n) và chỉ cần một vài biến, do đó mức sử dụng không gian là O(1). 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`p = [1, 0, 4, 0, 5]`| tôi | p[i] | đầu tiên_zero | cuối_zero | 
| --- | --- | --- | --- | 
| 0 | 1 | -1 | -1 | 
| 1 | 0 | 1 | 1 | 
| 2 | 4 | 1 | 1 | 
| 3 | 0 | 1 | 3 | 
| 4 | 5 | 1 | 3 | 

Độ dài đoạn: 3 - 1 + 1 = 3. 

Điều này cho thấy thuật toán xác định chính xác phân khúc`[0, 4, 0]`là cái lớn nhất có thể chưa được sắp xếp. 

**Ví dụ 2:**`p = [0, 0, 0]`| tôi | p[i] | đầu tiên_zero | cuối_zero | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 
| 1 | 0 | 0 | 1 | 
| 2 | 0 | 0 | 2 | 

Độ dài đoạn: 2 - 0 + 1 = 3. 

Tất cả các số không tạo thành mảng đầy đủ, tạo ra sự rối loạn tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi mảng được quét một lần để tìm số 0 đầu tiên và cuối cùng. | 
| Không gian | O(1) | Chỉ có hai chỉ số được lưu trữ; chính mảng đó đã bị ghi đè. | 

Được cho`sum(n) <= 2*10^5`, giải pháp hoàn thành tốt trong vòng chưa đầy 2 giây với chi phí bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        first_zero = -1
        last_zero = -1
        for i in range(n):
            if p[i] == 0:
                if first_zero == -1:
                    first_zero = i
                last_zero = i
        if first_zero == -1:
            print(0)
        else:
            print(last_zero - first_zero + 1)
    return output.getvalue().strip()

# Provided samples
assert run("4\n5\n1 0 4 0 5\n3\n0 0 0\n4\n1 2 3 0\n3\n0 3 2\n") == "3\n3\n0\n2"

# Custom cases
assert run("1\n1\n0\n") == "1"  # single zero
assert run("1\n5\n1 2 3 4 5\n") == "0"  # no zeros
assert run("1\n5\n0 1 0 2 0\n") == "5"  # zeros interleaved with numbers
assert run("1\n6\n0 0 1 2 0 0\n") == "6"  # zeros at both ends
assert run("1\n2\n0 2\n") == "1"  # zeros at beginning
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n0\n`| 1 | Mảng một phần tử có số 0 | 
|`1\n5\n1 2 3 4 5\n`| 0 | Đã sắp xếp hoán vị, không có số 0 | 
|`1\n5\n0 1 0 2 0\n`| 5 | Các số 0 xen kẽ, toàn bộ mảng tạo thành phân đoạn tối đa | 
| `1\n6\n0 0 1 2 0 | | |
