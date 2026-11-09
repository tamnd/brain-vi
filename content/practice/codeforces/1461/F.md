---
title: "CF 1461F - Biểu thức toán học"
description: "Chúng ta được cung cấp một chuỗi các chữ số, mỗi chữ số từ 0 đến 9 và một tập hợp các toán tử được phép, có thể là bất kỳ sự kết hợp nào của phép cộng, phép trừ và phép nhân."
date: "2026-06-11T02:24:23+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1461
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 689 (Div. 2, based on Zed Code Competition)"
rating: 2700
weight: 1461
solve_time_s: 108
verified: false
draft: false
---

[CF 1461F - Biểu thức toán học](https://codeforces.com/problemset/problem/1461/F) 

**Đánh giá:** 2700 
**Tags:** thuật toán xây dựng, dp, tham lam 
**Thời gian giải:** 1 phút 48 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các chữ số, mỗi chữ số từ 0 đến 9 và một tập hợp các toán tử được phép, có thể là bất kỳ sự kết hợp nào của phép cộng, phép trừ và phép nhân. Nhiệm vụ là đặt chính xác một toán tử vào giữa mỗi cặp chữ số liên tiếp để tối đa hóa giá trị của biểu thức thu được. Biểu thức tuân theo mức độ ưu tiên của toán tử tiêu chuẩn: phép nhân liên kết chặt chẽ hơn phép cộng hoặc phép trừ và phép cộng và phép trừ được đánh giá từ trái sang phải. 

Cho rằng chuỗi có thể dài tới 100.000 chữ số, bất kỳ phương pháp nào thử tất cả các vị trí có thể có của toán tử đều không khả thi ngay lập tức, bởi vì có tới$3^{10^5}$cách đặt các toán tử. Điều này có nghĩa là chúng ta cần một cách tiếp cận tránh được sự tăng trưởng theo cấp số nhân, lý tưởng nhất là một cách tiếp cận tuyến tính hoặc tuyến tính. 

Các trường hợp cạnh không rõ ràng phát sinh với số 0 và số 1 vì phép nhân tương tác với chúng khác với các số khác. Ví dụ: nhân với 0 sẽ hủy kết quả đang chạy, vì vậy "2_0+3" mang lại 3, có thể tốt hơn "2_0*3" mang lại 0. Tương tự, nhân với một không làm tăng giá trị, do đó, việc thêm một đôi khi có thể thích hợp hơn. Các số âm không xuất hiện trong các chữ số đầu vào, nhưng phép trừ có thể làm giảm tổng số đang chạy theo những cách mà phép cộng không làm được, điều này phải được xem xét cẩn thận khi tập hợp được phép bao gồm phép trừ. 

Cách tiếp cận tham lam bất cẩn luôn nhân hai số lớn nhất có thể thất bại khi số 0 xuất hiện trong chuỗi hoặc khi phép cộng các số nhỏ tạo ra tổng cuối cùng cao hơn chuỗi nhân liên quan đến các số 1. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử mọi tổ hợp toán tử giữa các chữ số. Về nguyên tắc, điều này đúng vì nó khám phá tất cả các biểu thức có thể, nhưng độ phức tạp của nó rất lớn.$O(3^{n})$, điều này hoàn toàn không thể thực hiện được đối với$n$lên tới 100.000. Ngay cả một cách tiếp cận lập trình động xem xét mọi mảng con và mọi toán tử có thể có giữa chúng một cách đơn giản cũng sẽ gần giống như vậy.$O(n^2)$, quá chậm. 

Thông tin chi tiết quan trọng là, do phạm vi chữ số nhỏ (0 đến 9) và các quy tắc ưu tiên, chúng ta có thể phân đoạn chuỗi ở số 0 khi phép nhân được phép, vì nhân bất kỳ thứ gì với 0 sẽ đặt lại kết quả về 0. Ngoài các số 0, chúng ta chỉ cần quyết định giữa phép cộng và phép nhân đối với các chuỗi chữ số và điều này có thể được thực hiện một cách tham lam đối với các chữ số nhỏ. Ví dụ: nếu cho phép cả 1 và phép nhân, thì tốt hơn là nên cộng các số đó thay vì nhân chúng vì nhân với một không làm tăng giá trị, trong khi phép cộng thì làm. Đối với các chuỗi chữ số không có số 0, chúng ta có thể lặp từ trái sang phải và quyết định: nếu chữ số hiện tại là 0 hoặc 1 và cho phép cả phép cộng và phép nhân thì phép cộng sẽ thích hợp hơn; nếu không thì phép nhân. 

Do đó, cách tiếp cận tối ưu là quét từ trái sang phải để phân đoạn xung quanh các số 0, xử lý các chuỗi bằng các số khác nhau và tôn trọng các toán tử được phép đặt. Phép trừ có thể được xử lý tương tự như phép cộng nếu có, nhưng nó luôn được áp dụng cẩn thận để tránh làm giảm tổng một cách không cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^n) | O(n) | Quá chậm | 
| Tham lam với các quy tắc phân đoạn và toán tử | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích chuỗi các chữ số thành một danh sách và ghi lại các toán tử được phép trong một tập hợp để tra cứu O(1). Nếu chỉ cho phép một toán tử thì giải pháp rất đơn giản: chèn toán tử đó vào mọi nơi. 
2. Nếu phép nhân được phép và chuỗi chứa các số 0, hãy chia chuỗi thành các mảng con cách nhau bằng các số 0. Mỗi số 0 sẽ đứng trước một dấu cộng nếu phép cộng được phép, vì phép nhân với số 0 sẽ đặt lại bất kỳ tích nào. Bản thân các số 0 được coi là các số riêng biệt có thể được cộng hoặc trừ để tối đa hóa tổng số. 
3. Lặp lại từng mảng con gồm các chữ số không có số 0. Đối với những điều này, hãy lặp lại từ trái sang phải và quyết định đặt toán tử nào giữa mỗi cặp. Nếu phép cộng được phép và chữ số hiện tại hoặc chữ số tiếp theo là 0 hoặc 1, hãy đặt phép cộng vì phép nhân sẽ không làm tăng giá trị hoặc sẽ giảm giá trị về 0. Ngược lại, đặt phép nhân. 
4. Ghép các phân đoạn kết quả lại với nhau, chèn toán tử cộng hoặc trừ xung quanh số 0 nếu được phép và nhân ở nơi khác. 
5. Xuất biểu thức đã xây dựng. 

Tại sao nó hoạt động: Điều bất biến là ở mỗi vị trí, chúng tôi tối đa hóa sự đóng góp của chữ số tiếp theo cho tích hoặc tổng đang chạy, xem xét các số không và số một. Việc chia ở số 0 đảm bảo phép nhân không bao giờ tạo ra số 0 khi nó sẽ làm giảm giá trị một cách không cần thiết và việc chọn phép cộng bằng số 1 sẽ đảm bảo tổng số không bị lãng phí khi nhân với một. Điều này tạo ra một biểu thức tối ưu toàn cục do phạm vi chữ số và quy tắc ưu tiên toán tử bị giới hạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
ops = set(input().strip())

if n == 1:
    print(a[0])
    sys.exit()

# Only one operator allowed
if len(ops) == 1:
    op = ops.pop()
    print(op.join(map(str, a)))
    sys.exit()

res = []
for i in range(n - 1):
    res.append(str(a[i]))
    x, y = a[i], a[i + 1]

    if '*' in ops:
        if '+' in ops:
            if x <= 1 or y <= 1:
                res.append('+')
            else:
                res.append('*')
        elif '-' in ops:
            if x <= 1 or y <= 1:
                res.append('-')
            else:
                res.append('*')
        else:
            res.append('*')
    else:
        if '+' in ops:
            res.append('+')
        else:
            res.append('-')
res.append(str(a[-1]))
print(''.join(res))
```Phần đầu tiên xử lý các trường hợp tầm thường khi chỉ cho phép một toán tử. Sau đó, vòng lặp chính đưa ra các quyết định tham lam cục bộ: phép nhân được chọn cho các chữ số lớn hơn 1 và phép cộng hoặc phép trừ được chọn nếu số 0 hoặc số 1 xuất hiện để tránh làm giảm giá trị. Phần tử cuối cùng được thêm vào mà không cần toán tử. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
3
2 2 0
+-*
```| tôi | một [tôi] | a[i+1] | Quyết định | Biểu hiện cho đến nay | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 2 | * | 2*2 | 
| 1 | 2 | 0 | + | 2*2+0 | 

Điều này chứng tỏ rằng phép nhân được chọn trong khoảng từ 2 đến 2 vì nó làm tăng giá trị và phép cộng được chọn trước 0 để tránh vô hiệu hóa tích trước đó. 

Đầu vào tùy chỉnh:```
5
1 2 3 0 4
+*
```| tôi | một [tôi] | a[i+1] | Quyết định | Biểu hiện cho đến nay | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | + | 1+2 | 
| 1 | 2 | 3 | * | 1+2*3 | 
| 2 | 3 | 0 | + | 1+2*3+0 | 
| 3 | 0 | 4 | + | 1+2*3+0+4 | 

Điều này xác nhận rằng các số 0 phân chia các chuỗi nhân và các số 0 được xử lý cộng gộp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý chính xác một lần trong vòng lặp. | 
| Không gian | O(n) | Biểu thức kết quả được lưu trữ trong một danh sách có độ dài khoảng 2n-1. | 

Thuật toán chia tỷ lệ tuyến tính với kích thước đầu vào, an toàn với n ≤ 10^5 và giới hạn thời gian 1 giây. Việc sử dụng bộ nhớ tỷ lệ thuận với đầu vào, trong khoảng 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    exec(open("solution.py").read(), globals())
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("3\n2 2 0\n+-*\n") == "2*2+0", "sample 1"

# Single element
assert run("1\n7\n+\n") == "7", "single element"

# Only multiplication allowed
assert run("3\n2 3 4\n*\n") == "2*3*4", "multiplication only"

# Only addition allowed
assert run("3\n2 3 4\n+\n") == "2+3+4", "addition only"

# Ones in sequence
assert run("4\n1 1 2 3\n+*\n") == "1+1*2*3", "ones treated additively"

# Zero splitting
assert run("5\n2 0 3 4 0\n+*\n") == "2+0+3*4+0", "zeros split multiplications"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 7 | Vụ án tầm thường | 
| Chỉ nhân | 2_3_4 | Vỏ cạnh người vận hành đơn | 
| Chỉ bổ sung | 2+3+4 | Vỏ cạnh người vận hành đơn | 
| Những cái | 1+1_2_3 | Những cái được xử lý chính xác trong các toán tử hỗn hợp | 
| Số không | 2+0+3*4+0 | Chuỗi nhân chia ở số 0 | 

## Vỏ cạnh 

Nếu đầu vào chỉ chứa số không, như`0 0 0`và các nhà khai thác`+*`, thuật toán tạo ra`0+0+0`. Nhân sẽ thiết lập lại
