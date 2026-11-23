---
title: "CF 433B - Những viên đá của Kuriyama Mirai"
description: "Chúng ta có một chuỗi các viên đá, mỗi viên có một giá trị bằng số. Kuriyama Mirai muốn hỏi đi hỏi lại hai loại câu hỏi: ở loại đầu tiên, cô ấy hỏi tổng chi phí của những viên đá trong một đoạn liền kề của trình tự ban đầu; ở loại thứ hai, cô ấy yêu cầu tổng của…"
date: "2026-06-07T02:42:48+07:00"
tags: ["codeforces", "competitive-programming", "dp", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 433
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 248 (Div. 2)"
rating: 1200
weight: 433
solve_time_s: 66
verified: true
draft: false
---

[CF 433B - Những viên đá của Kuriyama Mirai](https://codeforces.com/problemset/problem/433/B) 

**Đánh giá:** 1200 
**Tags:** dp, triển khai, sắp xếp 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi các viên đá, mỗi viên có một giá trị bằng số. Kuriyama Mirai muốn hỏi đi hỏi lại hai loại câu hỏi: ở loại đầu tiên, cô ấy hỏi tổng chi phí của những viên đá trong một đoạn liền kề của trình tự ban đầu; ở loại thứ hai, cô ấy yêu cầu tính tổng chi phí của các viên đá trong cùng một phân khúc nhưng sau khi sắp xếp tất cả các viên đá theo chi phí theo thứ tự không giảm. 

Đầu vào đầu tiên đưa ra số lượng viên đá, sau đó là chi phí riêng của chúng, tiếp theo là số lượng câu hỏi và chính các câu hỏi đó. Mỗi câu hỏi cung cấp loại (1 hoặc 2) và ranh giới phân đoạn. Đầu ra là tổng tương ứng với truy vấn đó. 

Cho rằng có thể có tới 100.000 viên đá và 100.000 truy vấn, một cách tiếp cận đơn giản tính tổng trực tiếp một phân đoạn cho mỗi truy vấn sẽ thực hiện các phép toán O(n * m) trong trường hợp xấu nhất. Con số này có thể lên tới 10^10 thao tác, vượt xa giới hạn hợp lý trong giới hạn thời gian 2 giây. Vì vậy, chúng ta cần một cách tiếp cận có thể xử lý nhiều truy vấn một cách hiệu quả, lý tưởng là ở O(1) cho mỗi truy vấn sau khi tiền xử lý. 

Các trường hợp cạnh không rõ ràng bao gồm các đoạn có độ dài 1, trong đó l bằng r, các đoạn bao phủ toàn bộ mảng và các mảng có tất cả các viên đá có giá trị như nhau. Một giải pháp bất cẩn có thể kết hợp việc lập chỉ mục dựa trên 1 và dựa trên 0 hoặc quên rằng tổng có thể vượt quá số nguyên 32 bit, dẫn đến tràn. Ví dụ, một mảng`v = [10^9, 10^9, 10^9]`và truy vấn`1 1 3`sẽ trả về 3*10^9, không vừa với số nguyên có dấu 32 bit. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: đối với mỗi truy vấn, lặp lại các chỉ số phân đoạn và tính tổng các giá trị. Điều này đúng vì nó trực tiếp thực hiện phát biểu vấn đề. Tuy nhiên, trường hợp xấu nhất là 100.000 viên đá và 100.000 truy vấn sẽ dẫn đến các hoạt động O(n*m), quá chậm. 

Thông tin chi tiết quan trọng là cả hai loại truy vấn đều yêu cầu tổng các phân đoạn liền kề. Đối với bất kỳ mảng nào, nếu chúng ta tính toán trước một mảng tổng tiền tố thì tổng của bất kỳ phân đoạn nào`[l, r]`có thể được tính bằng O(1) bằng công thức`prefix[r] - prefix[l-1]`. Đối với truy vấn loại 2, trước tiên chúng tôi sắp xếp mảng ban đầu và tính tổng tiền tố của nó. Điều này làm giảm thời gian mỗi truy vấn xuống O(1), trong khi quá trình tiền xử lý mất O(n log n) để sắp xếp và O(n) cho tổng tiền tố. 

Cách tiếp cận bạo lực hoạt động vì nó tính tổng trực tiếp các phần tử được yêu cầu nhưng không thành công đối với đầu vào lớn. Việc quan sát rằng tổng phân đoạn có thể được giảm xuống thành tổng tiền tố cho phép chúng tôi trả lời từng truy vấn trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*m) | O(n) | Quá chậm | 
| Tổng tiền tố + Sắp xếp | O(n log n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng đá`n`và mảng đá chi phí`v`. 
2. Xây dựng mảng tổng tiền tố`prefix_original`vì`v`Ở đâu`prefix_original[i] = v[0] + ... + v[i]`. Điều này cho phép truy vấn tổng O(1) trên mảng ban đầu. 
3. Tạo một bản sao của`v`và sắp xếp nó để có được`v_sorted`. Xây dựng một mảng tổng tiền tố`prefix_sorted`cho mảng đã được sắp xếp. 
4. Đọc số lượng truy vấn`m`. 
5. Đối với mỗi truy vấn`(type, l, r)`:

- Nếu như`type`là 1, tính tổng như sau`prefix_original[r] - prefix_original[l-1]`. 
- Nếu như`type`là 2, tính tổng như sau`prefix_sorted[r] - prefix_sorted[l-1]`. 
6. Xuất kết quả cho từng truy vấn. 

Điều này có tác dụng vì mảng tổng tiền tố duy trì tính bất biến mà`prefix[i]`bằng tổng của tất cả các phần tử từ chỉ số 1 đến i. Trừ hai tổng tiền tố sẽ cho ra chính xác tổng các phần tử giữa các chỉ số đó, khiến không thể tính sai tổng phân đoạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
v = list(map(int, input().split()))

# prefix sum for the original array
prefix_original = [0] * (n + 1)
for i in range(1, n + 1):
    prefix_original[i] = prefix_original[i - 1] + v[i - 1]

# prefix sum for the sorted array
v_sorted = sorted(v)
prefix_sorted = [0] * (n + 1)
for i in range(1, n + 1):
    prefix_sorted[i] = prefix_sorted[i - 1] + v_sorted[i - 1]

m = int(input())
for _ in range(m):
    t, l, r = map(int, input().split())
    if t == 1:
        print(prefix_original[r] - prefix_original[l - 1])
    else:
        print(prefix_sorted[r] - prefix_sorted[l - 1])
```Giải pháp đầu tiên là xây dựng các tổng tiền tố để tránh tính tổng lặp lại. sử dụng`n+1`mảng độ dài có số 0 đứng đầu giúp đơn giản hóa việc xử lý ranh giới và đảm bảo rằng tổng cho các phân đoạn bắt đầu từ chỉ mục 1 là chính xác mà không có trường hợp đặc biệt. Việc sắp xếp sẽ tạo ra mảng cho các truy vấn loại 2 và áp dụng thủ thuật tính tổng tiền tố tương tự. Cần cẩn thận để đọc đầu vào hiệu quả với`sys.stdin.readline`. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
6
6 4 2 7 2 7
3
2 3 6
1 3 4
1 1 6
```| Bước | tiền tố_origen | tiền tố_sắp xếp | Truy vấn | Tính toán | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | [0,6,10,12,19,21,28] | [0,2,4,6,11,18,25] | 2 3 6 | prefix_sorted[6]-prefix_sorted[2] = 25-4 | 21 | 
| Thứ hai | | | 1 3 4 | prefix_origen[4]-prefix_origen[2]=19-10 | 9 | 
| Thứ ba | | | 1 1 6 | prefix_origen[6]-prefix_origen[0]=28-0 | 28 | 

Điều này chứng tỏ rằng tổng tiền tố đưa ra câu trả lời theo thời gian không đổi cho bất kỳ phân đoạn nào. 

**Đầu vào tùy chỉnh:**```
5
1 1 1 1 1
2
1 1 5
2 1 5
```| Bước | tiền tố_origen | tiền tố_sắp xếp | Truy vấn | Tính toán | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | [0,1,2,3,4,5] | [0,1,2,3,4,5] | 1 1 5 | 5-0 | 5 | 
| Thứ hai | | | 2 1 5 | 5-0 | 5 | 

Điều này cho thấy thuật toán xử lý chính xác các phần tử bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + m) | Việc sắp xếp lấy O(n log n), tính tổng tiền tố lấy O(n), mỗi truy vấn m là O(1) | 
| Không gian | O(n) | Hai mảng tổng tiền tố có kích thước n+1, cộng với bản sao được sắp xếp | 

Cho n, m ≤ 10^5, điều này phù hợp thoải mái trong 2 giây và sử dụng tốt dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    n = int(input())
    v = list(map(int, input().split()))
    prefix_original = [0] * (n + 1)
    for i in range(1, n + 1):
        prefix_original[i] = prefix_original[i - 1] + v[i - 1]
    v_sorted = sorted(v)
    prefix_sorted = [0] * (n + 1)
    for i in range(1, n + 1):
        prefix_sorted[i] = prefix_sorted[i - 1] + v_sorted[i - 1]
    m = int(input())
    for _ in range(m):
        t, l, r = map(int, input().split())
        if t == 1:
            print(prefix_original[r] - prefix_original[l - 1])
        else:
            print(prefix_sorted[r] - prefix_sorted[l - 1])
    return output.getvalue().strip()

# provided sample
assert run("6\n6 4 2 7 2 7\n3\n2 3 6\n1 3 4\n1 1 6\n") == "24\n9\n28", "sample 1"

# all equal
assert run("5\n1 1 1 1 1\n2\n1 1 5\n2 1 5\n") == "5\n5", "all equal"

# single element
assert run("1\n42\n2\n1 1 1\n2 1 1\n") == "42\n42", "single element"

# large values
assert run("3\n1000000000 1000000000 1000000000\n1\n1 1 3\n") == "3000000000", "overflow test"

# segment of length 1
assert run("4\n3 1 4 1
```
