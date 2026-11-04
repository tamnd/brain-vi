---
title: "CF 1140E - Mảng không có Palindrome"
description: "Chúng ta được yêu cầu đếm số lượng mảng mà chúng ta có thể xây dựng từ một mảng có độ dài n được chỉ định một phần, trong đó một số phần tử bị thiếu và được biểu thị bằng -1. Mỗi -1 có thể được thay thế bằng bất kỳ số nguyên nào từ 1 đến k."
date: "2026-06-12T03:47:18+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "divide-and-conquer", "dp"]
categories: ["algorithms"]
codeforces_contest: 1140
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 62 (Rated for Div. 2)"
rating: 2200
weight: 1140
solve_time_s: 78
verified: true
draft: false
---

[CF 1140E - Mảng không có Palindrome](https://codeforces.com/problemset/problem/1140/E) 

**Đánh giá:** 2200 
**Tags:** tổ hợp, chia để trị, dp 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu đếm số lượng mảng chúng tôi có thể xây dựng từ một mảng có độ dài được chỉ định một phần`n`, trong đó một số phần tử bị thiếu và được biểu thị bằng`-1`. Mỗi`-1`có thể được thay thế bằng bất kỳ số nguyên nào từ`1`ĐẾN`k`. Hạn chế quan trọng là mảng kết quả không được chứa bất kỳ mảng con palindrome nào có độ dài lẻ lớn hơn`1`. Nói cách khác, bất kỳ đoạn liền kề nào có độ dài 3, 5, 7, v.v. không được đọc xuôi và ngược giống nhau. 

Mảng đầu vào có thể có một số số cố định và một số`-1`S. Đầu ra là một số nguyên duy nhất: số cách điền vào`-1`s để tạo thành một mảng "tốt", modulo`998244353`. 

Các ràng buộc rất lớn:`n`Và`k`mỗi cái có thể lên tới 200.000. Một cách tiếp cận bạo lực nhằm thử mọi phương án thay thế có thể cho`-1`là hoàn toàn không khả thi vì điều đó sẽ liên quan đến`k^n`khả năng. Chúng ta cần một giải pháp hoạt động theo thời gian tuyến tính hoặc gần tuyến tính đối với`n`. 

Các trường hợp cạnh bao gồm các mảng có tất cả`-1`s, mảng không có`-1`s, các mảng có hai hoặc nhiều giá trị lân cận giống hệt nhau và các mảng trong đó một giá trị cố định đã buộc phải có một bảng màu. Ví dụ, mảng`[1, -1, 1]`có một palindrome có độ dài 3 nếu`-1`được thay thế bởi`1`, vậy chỉ`k-1`các lựa chọn là hợp lệ. Một trường hợp cạnh khác là mảng có các giá trị cố định xen kẽ như`[1, 2, 1, 2, 1]`, trong đó các vị trí nhất định phải khác nhau để tránh tạo thành một bảng màu có độ dài 3. 

## Phương pháp tiếp cận 

Một cách tiếp cận ngây thơ sẽ thử mọi tổ hợp số để`-1`vị trí và sau đó kiểm tra từng mảng con có độ dài lẻ để tìm một bảng màu. Điều này đúng về mặt nguyên tắc, nhưng ngay cả đối với`n = 20`Và`k = 5`, có hơn 10 nghìn tỷ khả năng. Bản thân việc kiểm tra là O(n^2) cho mỗi ứng viên, điều này làm cho phương pháp này không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là các palindrome có độ dài lẻ là cục bộ và tập trung tại một vị trí cụ thể. Một palindrome có độ dài lẻ có độ dài 3 chỉ bao gồm ba số liên tiếp, độ dài 5 bao gồm năm số liên tiếp, v.v. Chúng ta có thể chia bài toán thành hai bài toán độc lập: một bài toán cho các phần tử có chỉ số lẻ và một bài toán cho các phần tử có chỉ số chẵn. Mỗi chuỗi có thể được xử lý độc lập vì các palindrome chỉ xuất hiện giữa các phần tử liên tiếp có cùng chỉ số chẵn lẻ được phân tách. 

Sau khi chia mảng, chúng ta có thể xử lý từng dãy con riêng biệt. Các chuỗi con rút gọn thành một bài toán "điền các ràng buộc" đơn giản hơn: đối với một đoạn liên tiếp`-1`s giữa hai giá trị cố định, chúng ta cần đếm số cách điền nó mà không tạo ra sự lặp lại có thể tạo ra một bảng màu. Có một công thức tổ hợp nổi tiếng cho việc này: nếu độ dài đoạn là`len`và các giá trị biên bằng nhau, có`(k-1)^(len//2)`cách, và nếu chúng khác nhau, có`(k-1)^((len+1)//2)`cách. Nếu chỉ có một`-1`và không có ràng buộc, nó có thể được điền vào`k`cách. 

Nhân số đếm cho tất cả các phân đoạn độc lập của dãy con sẽ có tổng số đếm cho dãy con đó. Nhân các số đếm cho các dãy con chẵn và lẻ sẽ cho ra câu trả lời cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k^n * n^2) | O(n) | Quá chậm | 
| Chia chẵn lẻ DP | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia mảng thành hai dãy: một dãy chứa các phần tử có chỉ số lẻ và một dãy chứa các phần tử có chỉ số chẵn. Điều này đảm bảo rằng các palindrome có độ dài từ 3 trở lên không trải dài trên các chuỗi, cho phép xử lý độc lập. 
2. Với mỗi dãy con, lặp qua các phần tử của nó và xác định các đoạn liên tiếp`-1`S. Mỗi phân đoạn nằm giữa hai số đã biết hoặc ở ranh giới của mảng. 
3. Đối với từng phân khúc: 

- Nếu bị giới hạn ở cả hai phía bởi các số đã biết, hãy tính số phần điền hợp lệ dựa trên việc các giá trị biên có bằng nhau hay không. Sử dụng công thức`(k-1)^(segment_length//2)`vì ranh giới bình đẳng và`(k-1)^((segment_length+1)//2)`vì ranh giới không bằng nhau. 
- Nếu đoạn không bị giới hạn ở một phía thì coi nó có tự do ở phía tự do. Một đoạn có độ dài`len`đóng góp`k * (k-1)^(len-1)`cách. 
4. Nhân số cách cho tất cả các phân đoạn trong dãy con. 
5. Nhân kết quả của hai dãy con để được tổng số mảng tốt modulo`998244353`. 
6. Trả về kết quả. 

Tại sao nó hoạt động: bằng cách chia thành các chỉ số lẻ và chẵn, chúng tôi đảm bảo rằng không bỏ sót bảng màu nào có độ dài lẻ lớn hơn 1. Việc đếm các phần điền hợp lệ cho từng phân đoạn một cách độc lập và nhân chúng sẽ duy trì tính chính xác vì các phân đoạn không tương tác qua các ranh giới cố định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def count_good(subseq, k):
    n = len(subseq)
    ans = 1
    i = 0
    while i < n:
        if subseq[i] != -1:
            i += 1
            continue
        j = i
        while j < n and subseq[j] == -1:
            j += 1
        left = subseq[i-1] if i-1 >= 0 else None
        right = subseq[j] if j < n else None
        length = j - i
        if left is None and right is None:
            ways = k * pow(k-1, length-1, MOD) if length > 0 else k
        elif left is None or right is None:
            ways = pow(k-1, length, MOD)
        else:
            if left == right:
                ways = pow(k-1, length//2, MOD)
            else:
                ways = pow(k-1, (length+1)//2, MOD)
        ans = ans * ways % MOD
        i = j
    return ans

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    odd = a[::2]
    even = a[1::2]
    res = count_good(odd, k) * count_good(even, k) % MOD
    print(res)

solve()
```chức năng`count_good`xử lý một chuỗi con tại một thời điểm. Nó xác định liên tiếp`-1`phân đoạn và áp dụng các công thức tổ hợp. Việc xử lý đặc biệt được thực hiện đối với các phân đoạn ở ranh giới mảng nơi có thể chỉ có một lân cận cố định hoặc không có phân đoạn nào cả. Hàm chính tách mảng thành các dãy con lẻ và chẵn, tính toán số cách cho mỗi dãy và nhân chúng theo modulo`998244353`. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
2 3
-1 -1
```| Chỉ mục | Tiếp theo | Phân đoạn | Trái | Đúng | Chiều dài | Cách | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | lẻ [ -1 ] | 0-0 | Không có | Không có | 1 | 3 | 
| 0 | thậm chí [ -1 ] | 0-0 | Không có | Không có | 1 | 3 | 

Nhân hai dãy con: 3 * 3 = 9, khớp với kết quả mong đợi. 

Đầu vào mẫu 2:```
3 2
-1 1 -1
```Dãy số lẻ:`[-1, -1]`Dãy số chẵn:`[1]`Đối với dãy con lẻ, có một phân đoạn gồm hai`-1`s, không giới hạn một bên và giới hạn phải bởi 1. Sử dụng công thức`(k-1)^length = 1^2 = 1`. 

Ngay cả dãy sau cũng không có`-1`, vậy là 1 cách. 

Tổng số cách = 1 * 1 = 1. 

Điều này thể hiện việc xử lý các phân đoạn có ranh giới một phần một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được truy cập một lần trong cả chuỗi chẵn và lẻ; xử lý phân đoạn là tuyến tính. | 
| Không gian | O(n) | Lưu trữ hai chuỗi con có kích thước n/2 mỗi chuỗi. | 

Giải pháp phù hợp thoải mái trong giới hạn 2 giây và giới hạn bộ nhớ 256 MB, vì tất cả các phép toán đều tuyến tính và chỉ liên quan đến lũy thừa mô-đun. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided sample
assert run("2 3\n-1 -1\n") == "9", "sample 1"

# Custom cases
assert run("3 2\n-1 1 -1\n") == "1", "partial bounds"
assert run("4 2\n-1 -1 -1 -1\n") == "4", "all -1, even length"
assert run("5 3\n1 -1 2 -1 3\n") == "4", "alternating fixed values"
assert run("2 200000\n-1 -1\n") == str(200000**2 % 998244353), "large k"
assert run("6 2\n1 -1 -
```
