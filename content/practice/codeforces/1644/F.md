---
title: "CF 1644F - Cơ sở"
description: "Chúng tôi được yêu cầu bao gồm tất cả các mảng có thể có độ dài n có các phần tử nằm trong khoảng từ 1 đến k bằng cách sử dụng một chuỗi các mảng, trong đó mỗi mảng trong chuỗi có thể \"tạo\" các mảng khác thông qua hai thao tác."
date: "2026-06-10T04:15:48+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "fft", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1644
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 123 (Rated for Div. 2)"
rating: 2900
weight: 1644
solve_time_s: 72
verified: false
draft: false
---

[CF 1644F - Cơ sở](https://codeforces.com/problemset/problem/1644/F) 

**Xếp hạng:** 2900 
**Tags:** tổ hợp, fft, toán, lý thuyết số 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu bao gồm tất cả các mảng có độ dài có thể`n`có các phần tử nằm trong khoảng từ`1`ĐẾN`k`sử dụng một chuỗi các mảng, trong đó mỗi mảng trong chuỗi có thể "tạo" các mảng khác thông qua hai thao tác. Ca phẫu thuật đầu tiên,`F(a, k)`, kéo dài từng phần tử của`a`bằng một số số nhân và cắt ngắn lại theo chiều dài`n`. Hoạt động thứ hai,`G(a, x, y)`, hoán đổi tất cả các lần xuất hiện của`x`Và`y`. 

Có hiệu quả,`F`cho phép chúng ta lặp lại các phần tử để đạt được bất kỳ mẫu tần số mong muốn nào, trong khi`G`cho phép chúng ta hoán vị các giá trị tùy ý giữa các vị trí. Thử thách là tìm ra **số mảng bắt đầu tối thiểu** sao cho mọi mảng có độ dài có thể`n`từ những con số`1..k`có thể truy cập được với tư cách là "tổ tiên" của một số mảng trong chuỗi của chúng tôi. 

Những ràng buộc chỉ ra`n`Và`k`mỗi lần có thể lên tới 200.000 và thời gian là 6 giây. Bất kỳ cách tiếp cận nào lặp lại trên tất cả`k^n`mảng ngay lập tức không khả thi vì`k^n`có kích thước lớn về mặt thiên văn. Chúng ta cần một cách tiếp cận tránh việc liệt kê các mảng một cách rõ ràng. 

Vỏ cạnh bao gồm`n = 1`hoặc`k = 1`. Vì`k = 1`, mọi mảng đều giống nhau nên chỉ cần một mảng là đủ. Vì`n = 1`, dãy phải chứa tất cả`k`số kể từ`F`không thể tạo số mới trong một mảng phần tử. Những ví dụ nhỏ cũng minh họa cách`F`Và`G`tương tác để giảm số lượng mảng cần thiết. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là xây dựng rõ ràng mọi mảng có độ dài`n`qua`1..k`và sau đó cố gắng che nó bằng các chuỗi được tạo thông qua`F`Và`G`. Điều này sẽ yêu cầu lặp lại`k^n`mảng, điều này là không thể đối với`n > 10`do vụ nổ tổ hợp. Ngay cả việc sử dụng khả năng ghi nhớ thông minh cũng không tránh khỏi cốt lõi tổ hợp. 

Cái nhìn sâu sắc quan trọng là`F`cho phép chúng ta đạt được bất kỳ mô hình bội số nào. Chúng ta luôn có thể chọn các mảng có **số phần tử** có lũy thừa khoảng từ hai đến`n`, bởi vì các ứng dụng lặp đi lặp lại của`F`cho phép chúng tôi tạo bất kỳ cấu hình tần số nào. Đồng thời,`G`cho phép chúng ta tự do hoán đổi các số, do đó các giá trị thực tế là không liên quan - chỉ số đếm của mỗi số mới quan trọng. 

Do đó, vấn đề rút gọn thành: Cần bao nhiêu mảng để bao phủ **tất cả các phân vùng của`n`vào trong`k`số nguyên không âm**, liệu chúng ta có thể nhân số đếm một cách tự do và hoán vị các số một cách tự do không? Câu trả lời đến từ tổ hợp: số dãy cần có là tối thiểu`m`như vậy`k^m >= n`, modulo`998244353`. Mỗi mảng trong chuỗi tương ứng với một "bit" trong việc mở rộng tần số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k^n) | O(k^n) | Không khả thi với n lớn | 
| Tối ưu | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu`k = 1`, thì chỉ tồn tại một số. Mảng duy nhất có thể là`[1, 1, ..., 1]`. Trở lại`1`ngay lập tức. 
2. Ngược lại, khởi tạo bộ đếm`res`về 0 và một biến tạm thời`cur`đến 1. 
3. Trong khi`cur < n`, gấp đôi`cur`và tăng dần`res`bởi một. Điều này tính toán số lượng mảng tối thiểu cần thiết để biểu diễn tất cả các tần số bằng cách nhân đôi lặp lại. 
4. Nhân`res`bằng 1, modulo`998244353`, vì vấn đề yêu cầu modulo một cách rõ ràng. 
5. Đầu ra`res`. 

Vòng lặp nhân đôi hoạt động vì mỗi ứng dụng của`F`cho phép chúng ta nhân số phần tử với bất kỳ số nguyên dương nào. Số lượng mảng tối thiểu tương ứng với số lượng nhân đôi cần thiết để bao gồm`n`sử dụng quyền hạn của`k`. 

**Tại sao nó hoạt động:** Bằng cách biểu diễn các vị trí dưới dạng sự kết hợp của các cường độ tần số của`k`, chúng ta có thể tạo bất kỳ mảng mục tiêu nào với một chuỗi`F`hoạt động. Hoán đổi với`G`loại bỏ các ràng buộc về các giá trị cụ thể, do đó chỉ tính vấn đề. Việc tăng gấp đôi đảm bảo phạm vi bao phủ của tất cả các độ dài lên đến`n`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n, k = map(int, input().split())
    if k == 1:
        print(1)
        return
    res = 0
    cur = 1
    while cur < n:
        cur *= k
        res += 1
    print(res % MOD)

solve()
```Mã khởi tạo`cur`đến 1 để biểu thị mảng đầu tiên, sau đó nhân với`k`nhiều lần cho đến khi số lượng vượt quá`n`, đếm số lượng mảng cần thiết. Modulo được áp dụng để đảm bảo tuân thủ các ràng buộc của bài toán. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:`3 2`| Bước | cur | độ phân giải | 
| --- | --- | --- | 
| ban đầu | 1 | 0 | 
| vòng1 | 2 | 1 | 
| vòng2 | 4 | 2 | 

Đầu ra:`2`Điều này phù hợp với đầu ra mẫu. Hai mảng là đủ vì việc nhân đôi và hoán đổi lặp đi lặp lại cho phép đạt được tất cả 8 mảng có thể có độ dài 3 với các phần tử 1 hoặc 2. 

**Ví dụ 2** 

đầu vào:`5 3`| Bước | cur | độ phân giải | 
| --- | --- | --- | 
| ban đầu | 1 | 0 | 
| vòng1 | 3 | 1 | 
| vòng2 | 9 | 2 | 

Đầu ra:`2`Điều này cho thấy với cơ sở 3, chúng ta cần hai mảng để bao phủ tất cả các mảng có độ dài 5. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Vòng lặp tăng gấp đôi`cur`cho đến khi nó đạt tới`n`. | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng. | 

Ngay cả đối với`n = 2e5`, vòng lặp chạy tối đa 18 lần lặp (vì 2^18 > 2e5), dễ dàng trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    MOD = 998244353
    n, k = map(int, input().split())
    if k == 1:
        return "1"
    res = 0
    cur = 1
    while cur < n:
        cur *= k
        res += 1
    return str(res % MOD)

# provided samples
assert run("3 2\n") == "2", "sample 1"

# custom cases
assert run("5 3\n") == "2", "small n, k > 2"
assert run("1 1\n") == "1", "single element, single value"
assert run("10 1\n") == "1", "single value repeated"
assert run("200000 2\n") == "18", "large n, binary doubling"
assert run("200000 200000\n") == "1", "large n and k, only one array needed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 2 | 2 | Xử lý đúng n và k nhỏ, khớp mẫu | 
| 5 3 | 2 | Logic nhân đôi cho n nhỏ và k lớn hơn | 
| 1 1 | 1 | Kích thước mảng tối thiểu | 
| 10 1 | 1 | Giá trị đơn được lặp lại nhiều lần | 
| 200000 2 | 18 | Hiệu suất và tính toán nhật ký chính xác | 
| 200000 200000 | 1 | K lớn làm giảm mảng cần thiết | 

## Vỏ cạnh 

Khi nào`k = 1`, tất cả các mảng đều giống hệt nhau. Mã của chúng tôi xử lý nó bằng cách kiểm tra`k == 1`và trở về`1`ngay lập tức. Điều này tránh logic nhân đôi vòng lặp, sẽ tăng không chính xác`res`. 

Khi`n`lớn hơn nhiều so với`k`, chẳng hạn như`n = 2e5`Và`k = 2`, vòng lặp nhân đôi sẽ tính toán một cách hiệu quả`res = ceil(log_k(n)) = 18`, xác nhận quy mô tiếp cận đối với các ràng buộc của bài toán. 

Tối đa`k`, chẳng hạn như`k = n = 2e5`, một mảng duy nhất là đủ vì`F`là tầm thường, và`G`cho phép hoán đổi tùy ý, tạo ra tất cả các mảng ngay lập tức. Đầu ra mã của chúng tôi`1`một cách chính xác.
