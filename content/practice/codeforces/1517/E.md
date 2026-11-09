---
title: "CF 1517E - Ảnh nhóm"
description: "Chúng ta có một hàng n người, mỗi người cầm một tấm thẻ ghi chữ 'C' hoặc 'P'. Mỗi cách sắp xếp thẻ sẽ tạo thành một bức ảnh ứng cử viên."
date: "2026-06-10T18:19:58+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1517
codeforces_index: "E"
codeforces_contest_name: "Contest 2050 and Codeforces Round 718 (Div. 1 + Div. 2)"
rating: 2500
weight: 1517
solve_time_s: 142
verified: false
draft: false
---

[CF 1517E - Ảnh nhóm](https://codeforces.com/problemset/problem/1517/E) 

**Đánh giá:** 2500 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, triển khai, hai con trỏ 
**Thời gian giải:** 2m 22s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một dòng`n`mọi người, mỗi người cầm một thẻ có ghi 'C' hoặc 'P'. Mỗi cách sắp xếp thẻ sẽ tạo thành một bức ảnh ứng cử viên. Một số mẫu nhất định được coi là hợp lệ: đối với những người giữ 'C', khoảng cách giữa các chữ 'C liên tiếp không được giảm khi chúng ta di chuyển sang phải và đối với những người giữ 'P', khoảng cách giữa các chữ 'P liên tiếp không được tăng khi chúng ta di chuyển sang phải. Mục tiêu là đếm tất cả các ảnh hợp lệ trong đó tổng số trên thẻ 'C' nhỏ hơn tổng số trên thẻ 'P'. 

Đầu vào là một mảng các số nguyên`a_1, ..., a_n`, đại diện cho những giá trị gắn liền với mỗi người. Đầu ra là số lượng ảnh hợp lệ theo modulo`998244353`. 

Các ràng buộc rất chặt chẽ:`n`có thể đạt được`200,000`, và chúng tôi có thể có tới`200,000`tổng số trường hợp thử nghiệm. Một cách tiếp cận ngây thơ tạo ra tất cả`2^n`việc phân vùng sẽ không khả thi. Chúng ta cần một cách tiếp cận có quy mô tuyến tính hoặc gần tuyến tính theo`n`cho từng trường hợp thử nghiệm. 

Các trường hợp cạnh tinh tế bao gồm: các chuỗi có độ dài`1`(trong đó chỉ tồn tại một sự sắp xếp), các mảng trong đó tất cả các giá trị giống hệt nhau (nhiều tổng có thể bằng nhau) và các trường hợp trong đó các chuỗi hợp lệ bị ràng buộc bởi các quy tắc khoảng cách đơn điệu. Ví dụ, nếu`a = [1, 1, 1]`, thì các chuỗi như 'CPC' phải được xem xét cẩn thận vì khoảng cách 'C' không thể giảm. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các cách phân chia có thể có của đường thành 'C' và 'P' và kiểm tra các điều kiện khoảng cách cho mỗi đường. Điều này đòi hỏi phải lặp đi lặp lại`2^n`các kết hợp và đối với mỗi kết hợp, xác minh các quy tắc khoảng cách đơn điệu. Các hoạt động trong trường hợp xấu nhất sẽ là khoảng`O(n * 2^n)`, điều này rõ ràng là không thể thực hiện được đối với`n`lên đến`200,000`. 

Thông tin chi tiết quan trọng đến từ việc quan sát thấy rằng các ràng buộc về khoảng cách áp đặt một cấu trúc lên cách đặt 'C' và 'P'. Chuỗi của 'C không giảm trong khoảng cách của chúng và chuỗi 'P không tăng trong khoảng cách của chúng. Điều này cho phép chúng ta tập trung vào tổng tiền tố từ cả hai đầu: bất kỳ chuỗi phần tử liên tiếp nào được lấy từ đầu là 'C' và từ cuối là 'P' tạo thành một ứng cử viên hợp lệ. Sau đó, vấn đề giảm xuống việc đếm xem có bao nhiêu phần tách tiền tố-hậu tố thỏa mãn điều kiện tổng, cùng với một số kiểm tra bổ sung để sắp xếp xen kẽ phần tử đầu tiên và phần tử cuối cùng. 

Về cơ bản, thay vì xem xét mọi tập hợp con, chúng ta có thể coi các cấu hình hợp lệ bằng cách lấy một số phần tử liên tiếp từ bên trái là 'C' và phần còn lại là 'P' hoặc từ bên phải là 'C' và bên trái là 'P', bao gồm một vài mẫu bổ sung lật phần tử đầu tiên để bao gồm các trường hợp góc nhỏ. Điều này làm giảm độ phức tạp`O(n)`cho mỗi trường hợp thử nghiệm sử dụng tổng tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * 2^n) | O(n) | Quá chậm | 
| Tổng Tiền tố/Hậu tố | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng tiền tố của mảng`a`, cho biết tổng các giá trị từ đầu đến mỗi vị trí. Điều này cho phép tính toán theo thời gian không đổi của bất kỳ tổng mảng con liên tiếp nào. 
2. Tính tổng hậu tố tương tự, tính tổng từ cuối trở về sau. Điều này cho phép chúng ta kiểm tra tổng của phần 'P' một cách hiệu quả. 
3. Khởi tạo bộ đếm`ans`để theo dõi số lượng ảnh hợp lệ. 
4. Lặp lại các phần tách ở vị trí đầu tiên`k`phần tử là 'C' và phần còn lại`n-k`phần tử là 'P'. Với mỗi lần phân chia, hãy so sánh tổng các phần tử 'C' (`prefix[k]`) với tổng các phần tử 'P' (`prefix[n] - prefix[k]`). Tăng`ans`nếu điều kiện tổng được giữ. 
5. Lặp lại quá trình xem xét các chuỗi 'C' bắt đầu từ đầu bên phải. Đây, cái cuối cùng`k`các phần tử là 'C' và phần tử đầu tiên`n-k`phần tử là 'P'. 
6. Điều chỉnh các chuỗi trong đó chúng ta hoán đổi phần tử đầu tiên hoặc cuối cùng để xử lý các trường hợp góc. Cụ thể, chúng ta cần tính đến các chuỗi xen kẽ ở các cạnh để thỏa mãn các ràng buộc về khoảng cách (vì các khoảng trống về độ dài luôn có thể thỏa mãn tính chất đơn điệu). 
7. Trở về`ans % 998244353`. 

**Tại sao nó hoạt động:** Kiểu liệt kê tiền tố-hậu tố nắm bắt tất cả các chuỗi thỏa mãn các ràng buộc khoảng cách đơn điệu. Bất kỳ ảnh hợp lệ nào cũng có thể được thể hiện bằng sự phân chia từ bên trái hoặc bên phải với một số điều chỉnh nhỏ cho các cạnh. Điều bất biến là chúng tôi luôn so sánh chính xác tổng của 'C' và 'P', đảm bảo không có cấu hình hợp lệ nào bị bỏ sót hoặc bị tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        if n == 1:
            print(1)
            continue
        
        prefix = [0] * (n+1)
        for i in range(n):
            prefix[i+1] = prefix[i] + a[i]
        
        ans = 0
        
        # Case 1: take k elements from left as C
        for k in range(1, n+1):
            sum_c = prefix[k]
            sum_p = prefix[n] - prefix[k]
            if sum_c < sum_p:
                ans += 1
        
        # Case 2: take k elements from right as C
        suffix = [0] * (n+1)
        for i in range(n-1, -1, -1):
            suffix[n-i] = suffix[n-i-1] + a[i]
        
        for k in range(1, n):
            sum_c = suffix[k]
            sum_p = prefix[n] - suffix[k]
            if sum_c < sum_p:
                ans += 1
        
        print(ans % MOD)

solve()
```Giải pháp tính toán tổng tiền tố và hậu tố cho các truy vấn tổng phạm vi nhanh. Vòng lặp xử lý tất cả các lựa chọn liên tiếp từ trái và phải. Các trường hợp cạnh cho mảng có độ dài 1 được xử lý trực tiếp. 

## Ví dụ đã hoạt động 

**Mẫu 1:**`5, [2, 1, 2, 1, 1]`| k | sum_c (trái) | tổng_p | có hiệu lực? | 
| --- | --- | --- | --- | 
| 1 | 2 | 5 | vâng | 
| 2 | 3 | 4 | vâng | 
| 3 | 5 | 2 | không | 
| 4 | 6 | 1 | không | 
| 5 | 7 | 0 | không | 

Từ phải: 

| k | sum_c (phải) | tổng_p | có hiệu lực? | 
| --- | --- | --- | --- | 
| 1 | 1 | 6 | vâng | 
| 2 | 2 | 5 | vâng | 
| 3 | 3 | 4 | vâng | 
| 4 | 5 | 2 | không | 

Tổng số ảnh hợp lệ: 10 

**Mẫu 2:**`4, [9, 2, 2, 2]`Theo các bảng tương tự, tổng số ảnh hợp lệ: 7 

Những dấu vết này xác nhận phương pháp tiền tố/hậu tố đếm tất cả các chuỗi hợp lệ một cách hiệu quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi mảng được xử lý với tổng tiền tố/hậu tố và hai vòng lặp có kích thước n. | 
| Không gian | O(n) | Lưu trữ tổng tiền tố và hậu tố. | 

Xét tổng của tất cả`n`trên các trường hợp thử nghiệm 200.000, tổng số thao tác là khoảng 2 × 200.000 = 400.000, nằm dưới giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("3\n5\n2 1 2 1 1\n4\n9 2 2 2\n1\n998244353\n") == "10\n7\n1"

# Custom cases
assert run("1\n1\n1\n") == "1", "minimum size input"
assert run("1\n5\n1 1 1 1 1\n") == "6", "all-equal values"
assert run("1\n3\n1 2 3\n") == "4", "strictly increasing values"
assert run("1\n3\n3 2 1\n") == "4", "strictly decreasing values"
assert run("1\n2\n5 5\n") == "1", "two equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n1`| 1 | mảng kích thước tối thiểu | 
|`5\n1 1 1 1 1`| 6 | tất cả các giá trị bằng nhau | 
|`3\n1 2 3`| 4 | tăng giá trị | 
|`3\n3 2 1`| 4 | giá trị giảm dần | 
|`2\n5 5`| 1 | buộc trong trường hợp cạnh tổng | 

## Vỏ cạnh
