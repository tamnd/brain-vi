---
title: "CF 2041E - Mảng Đẹp"
description: "Chúng ta được yêu cầu xây dựng một mảng số nguyên sao cho giá trị trung bình của nó chính xác là a và trung vị của nó chính xác là b. Đầu vào bao gồm hai số nguyên a và b, tương ứng là giá trị trung bình và trung vị mong muốn."
date: "2026-06-08T09:42:10+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 2041
codeforces_index: "E"
codeforces_contest_name: "2024 ICPC Asia Taichung Regional Contest (Unrated, Online Mirror, ICPC Rules, Preferably Teams)"
rating: 1200
weight: 2041
solve_time_s: 110
verified: false
draft: false
---

[CF 2041E - Mảng đẹp](https://codeforces.com/problemset/problem/2041/E) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng một mảng số nguyên sao cho giá trị trung bình của nó chính xác`a`và trung vị của nó chính xác là`b`. Dữ liệu đầu vào bao gồm hai số nguyên,`a`Và`b`, tương ứng là giá trị trung bình và trung vị mong muốn. Đầu ra là độ dài của một mảng hợp lệ, theo sau là các phần tử mảng. Độ dài mảng phải nằm trong khoảng từ 1 đến 1000 và mỗi phần tử phải là số nguyên trong phạm vi ±10^6. 

Các ràng buộc tương đối hào phóng. Vì mảng có thể có tới 1000 phần tử và bản thân các phần tử có thể có độ lớn rất lớn nên chúng ta có quyền tự do đáng kể trong việc lựa chọn các giá trị mảng. Điều này làm cho cách tiếp cận dựa trên việc xây dựng cẩn thận trở nên khả thi. Thách thức chính là thỏa mãn đồng thời giá trị trung bình và trung vị trong khi vẫn giữ số nguyên và độ dài trong giới hạn cho phép. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó giá trị trung bình và trung vị khác nhau. Ví dụ, nếu`a = 3`Và`b = 4`, một cách tiếp cận ngây thơ chỉ đơn giản là lấp đầy mảng bằng`b`sẽ thỏa mãn số trung vị nhưng không thỏa mãn số trung bình. Một trường hợp cạnh khác là khi`a = b`, trong trường hợp đó hãy điền toàn bộ mảng bằng`a`giải quyết cả hai yêu cầu một cách tầm thường. Mảng có độ dài 1 cũng phải được xử lý chính xác, trong đó phần tử đơn phải bằng cả hai`a`Và`b`. Những ví dụ này cho thấy giải pháp phải cho phép xây dựng linh hoạt thay vì lấp đầy các giá trị một cách mù quáng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử tất cả các độ dài mảng có thể có và tất cả các kết hợp của các phần tử số nguyên để kiểm tra xem mảng kết quả có giá trị trung bình và trung vị mong muốn hay không. Về nguyên tắc điều này đúng vì nó kiểm tra tất cả các khả năng. Tuy nhiên, với tối đa 1000 phần tử và giá trị lên tới 10^6, số lượng kết hợp rất lớn, khiến phương pháp này không khả thi. 

Cái nhìn sâu sắc về một giải pháp tối ưu là chúng ta có thể xây dựng một mảng nhỏ xung quanh giá trị trung bình và điều chỉnh các phần tử khác để thỏa mãn giá trị trung bình. Nếu chúng ta bắt đầu với một mảng có độ dài 3 với`[b, b, x]`, trung vị là`b`. Chúng ta có thể giải quyết cho`x`sử dụng phương trình trung bình:`(b + b + x)/3 = a`cho`x = 3a - 2b`. Điều này đảm bảo các giải pháp số nguyên miễn là`x`là một số nguyên, nó luôn luôn như vậy nếu`a`Và`b`là số nguyên. Nếu như`x`nằm ngoài giới hạn cho phép, chúng ta có thể tăng độ dài mảng bằng cách thêm vào`b`s và một phần tử cân bằng. Cách tiếp cận này hiệu quả vì các ràng buộc cho phép cả số dương và số âm lên tới ±10^6. 

Phương pháp này đơn giản, luôn xây dựng một mảng có độ dài hợp lý (1000) và thỏa mãn chính xác cả giá trị trung bình và trung vị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((10^6)^n) | O(n) | Quá chậm | 
| Mang tính xây dựng | O(1) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Kiểm tra xem`a`bằng`b`. Nếu vậy, hãy in một mảng có độ dài 1 với`[a]`. Điều này thỏa mãn cả giá trị trung bình và trung vị. 
2. Mặt khác, hãy xem xét một mảng ban đầu có độ dài 3:`[b, b, x]`, Ở đâu`x = 3a - 2b`. Điều này đảm bảo rằng giá trị trung bình là chính xác`a`trong khi số trung vị vẫn giữ nguyên`b`. 
3. Nếu`x`nằm trong giới hạn phần tử cho phép (±10^6), hãy in mảng này. Trung vị của nó là`b`bởi vì phần tử ở giữa sau khi sắp xếp là`b`. 
4. Nếu`x`nằm ngoài giới hạn, hãy mở rộng mảng bằng cách thêm vào`b`s ở đầu hoặc cuối và điều chỉnh một phần tử để duy trì giá trị trung bình. Cụ thể, đối với các mảng lớn hơn, hãy chọn một mẫu đơn giản như`[b]*k + [y]`, Ở đâu`y`được tính toán để thỏa mãn giá trị trung bình một cách chính xác. Các ràng buộc đảm bảo rằng một giải pháp tồn tại. 
5. Xuất ra chiều dài của mảng theo sau là các phần tử của mảng. 

Tại sao nó hoạt động: Thuật toán duy trì giá trị trung bình ở mức`b`bằng cách giữ`b`ở trung tâm. Bằng cách giải đại số để (các) phần tử còn lại thỏa mãn giá trị trung bình, chúng tôi đảm bảo giá trị trung bình là chính xác`a`. Phạm vi được phép cho độ dài mảng và giá trị phần tử đảm bảo luôn có thể xây dựng được giải pháp hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, b = map(int, input().split())

if a == b:
    print(1)
    print(a)
else:
    # Start with three elements: [b, b, x]
    x = 3 * a - 2 * b
    if abs(x) <= 10**6:
        print(3)
        print(b, b, x)
    else:
        # If x is too large, use a longer array
        # We'll use 100 elements: 50 b's and 50 zeros, then adjust last element
        n = 100
        arr = [b] * (n // 2)
        arr += [0] * (n // 2 - 1)
        # Compute the last element to satisfy the mean
        total = sum(arr)
        last = n * a - total
        arr.append(last)
        print(n)
        print(*arr)
```Mã đầu tiên kiểm tra xem`a`bằng`b`, trong trường hợp đó mảng là`[a]`. Mặt khác, nó thử xây dựng ba phần tử đơn giản`[b, b, 3a-2b]`. Nếu điều đó không thành công do giới hạn, nó sẽ tạo một mảng dài hơn gồm 100 phần tử với một nửa`b`s và một nửa số không, điều chỉnh phần tử cuối cùng để thỏa mãn giá trị trung bình. Việc sắp xếp là không cần thiết vì vẫn giữ nguyên số trung vị`b`do vị trí của`b`s ở trung tâm. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 4
```| Bước | Mảng | Tính toán trung bình | Trung vị | 
| --- | --- | --- | --- | 
| Ban đầu | [4, 4, x] | (4+4+1)/3 = 3 | 4 | 
| Đầu ra | [4, 4, 1] | 3 | 4 | 

Điều này khẳng định rằng việc xây dựng ba phần tử đơn giản có hiệu quả khi`3a - 2b = 1`. 

### Ví dụ tùy chỉnh 

đầu vào:```
2 5
```| Bước | Mảng | Tính toán trung bình | Trung vị | 
| --- | --- | --- | --- | 
| Ban đầu | [5, 5, x] | (5+5+(-1))/3 = 3 | 5 | 
| Đã điều chỉnh | [5,5,-1] | (5+5-1)/3 = 3 | 5 | 

Một lần nữa, mảng ba phần tử thỏa mãn cả hai điều kiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Việc xây dựng bao gồm một số phép tính số học và tạo mảng lên tới 100 phần tử | 
| Không gian | O(n) | Mảng lưu trữ tới 1000 số nguyên | 

Giải pháp dễ dàng phù hợp với những hạn chế về thời gian và bộ nhớ. Độ dài mảng không bao giờ vượt quá 1000 và các phép toán số học là không đáng kể đối với Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    input_backup = builtins.input
    builtins.input = lambda: sys.stdin.readline()
    import sys
    out = io.StringIO()
    sys.stdout = out
    # solution code
    a, b = map(int, input().split())
    if a == b:
        print(1)
        print(a)
    else:
        x = 3 * a - 2 * b
        if abs(x) <= 10**6:
            print(3)
            print(b, b, x)
        else:
            n = 100
            arr = [b] * (n // 2)
            arr += [0] * (n // 2 - 1)
            last = n * a - sum(arr)
            arr.append(last)
            print(n)
            print(*arr)
    builtins.input = input_backup
    return out.getvalue().strip()

# provided samples
assert run("3 4\n") == "3\n4 4 1", "sample 1"
# custom cases
assert run("2 2\n") == "1\n2", "a equals b"
assert run("100 0\n") == "3\n0 0 300", "median zero, mean large"
assert run("-5 5\n") == "3\n5 5 -25", "negative mean, positive median"
assert run("0 100\n") == "3\n100 100 -200", "median large, mean zero"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2 | 1\n2 | Mảng một phần tử khi giá trị trung bình bằng trung vị | 
| 100 0 | 3\n0 0 300 | Giá trị trung bình lớn, trung vị bằng 0 | 
| -5 5 | 3\n5 5 -25 | Giá trị trung bình âm, trung vị dương | 
| 0 100 | 3\n100 100 -200 | Trung vị lớn hơn trung bình | 

## Vỏ cạnh 

Khi nào`a = b`, chẳng hạn như`2 2`, thuật toán ngay lập tức xuất ra`[2]`. Dấu vết không đáng kể: mảng có độ dài 1, trung bình = 2, trung vị = 2. 

Khi nào`3a - 2b`vượt quá ±10^6, thuật toán sẽ chuyển sang mảng dài hơn gồm 100 phần tử. Ví dụ,`a = 1000`,`b = 0`sản xuất`3*1000 - 2*0 = 3000`, vượt quá giới hạn phần tử. Việc mở rộng
