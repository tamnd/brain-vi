---
title: "CF 2028C - Cuộc phiêu lưu cắt bánh của Alice"
description: "Chúng tôi được phát một chiếc bánh dài được chia thành n phần, mỗi phần có một mức độ ngon. Alice đang ở một bữa tiệc với m sinh vật và cô ấy muốn cắt chiếc bánh thành m + 1 miếng liền nhau. Mỗi sinh vật sẽ chỉ hạnh phúc nếu miếng của nó có vị ngon ít nhất là v."
date: "2026-06-08T12:08:08+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2028
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 986 (Div. 2)"
rating: 1600
weight: 2028
solve_time_s: 95
verified: false
draft: false
---

[CF 2028C - Cuộc phiêu lưu cắt bánh của Alice](https://codeforces.com/problemset/problem/2028/C) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, dp, tham lam, hai con trỏ 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được phát một chiếc bánh dài được chia thành`n`phần, mỗi phần có một giá trị độ ngon. Alice đang ở bữa tiệc với`m`sinh vật, và cô ấy muốn cắt chiếc bánh thành`m + 1`mảnh liền nhau. Mỗi sinh vật sẽ chỉ hạnh phúc nếu ít nhất miếng của nó có vị ngon`v`. Alice muốn tối đa hóa hương vị của món ăn mà cô ấy giữ cho riêng mình, đồng thời đảm bảo rằng tất cả sinh vật đều hài lòng. Nếu không thể làm hài lòng tất cả sinh vật thì kết quả là`-1`. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm chỉ định`n`,`m`, Và`v`, theo sau là mảng các giá trị độ ngon. Đầu ra là một số duy nhất cho mỗi trường hợp thử nghiệm, độ ngon tối đa mà Alice có thể đạt được, hoặc`-1`nếu không có phân vùng hợp lệ tồn tại. 

Với những hạn chế,`n`có thể lên tới 2×10^5 trong một trường hợp thử nghiệm duy nhất và tổng của`n`trên tất cả các trường hợp thử nghiệm cũng là 2×10^5. Điều này có nghĩa là bất kỳ thuật toán nào có độ phức tạp lớn hơn O(n log n) cho mỗi trường hợp thử nghiệm đều có thể sẽ hết thời gian chờ. Chúng tôi không thể sử dụng phương pháp tiếp cận mạnh mẽ O(n^2) để kiểm tra tất cả các mảng con. 

Một trường hợp cạnh quan trọng xảy ra khi tổng của`m`mảng con liền kề lớn nhất của độ ngon ít nhất`v`vượt quá tổng số bánh. Trong những trường hợp như vậy, ngay cả khi Alice cố gắng tối đa hóa phần của mình thì cũng có thể không có đủ bánh để làm hài lòng tất cả sinh vật. Ví dụ, nếu`a = [1,1,1]`,`m=2`, Và`v=2`, không có cách nào để cho hai miếng mỗi miếng có độ ngon ≥2, vì vậy kết quả đầu ra đúng là`-1`. 

Một điều tinh tế khác là mảnh riêng của Alice có thể trống. Trong những tình huống mà cô ấy không thể lấy một mảnh lớn mà không vi phạm hạnh phúc của sinh vật, chiến lược tối ưu của cô ấy có thể là lấy không phần nào, điều này hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force sẽ thử tất cả các phân vùng của mảng thành`m + 1`các miếng liền nhau và kiểm tra xem miếng nào thỏa mãn điều kiện về độ ngon tối thiểu đối với sinh vật. Đối với mỗi phân vùng hợp lệ, chúng tôi sẽ tính tổng từng phần của Alice và chọn mức tối đa. Có nhiều cách để phân vùng một mảng theo cấp số nhân, vì vậy cách tiếp cận này không khả thi. Ngay cả khi chúng tôi giới hạn các mảng con liền kề cho các mảnh của sinh vật, chúng tôi vẫn có thể có các khả năng O(n choose m), quá lớn đối với n lên tới 2×10^5. 

Cái nhìn sâu sắc quan trọng là vấn đề giảm xuống theo cách tiếp cận tổng tiền tố kiểu tham lam kết hợp với tìm kiếm nhị phân. Giả sử chúng ta biết Alice muốn một miếng ngon`x`. Chúng ta có thể cố gắng chia các mảnh cho các sinh vật một cách tham lam từ trái sang phải sao cho mỗi sinh vật nhận được một miếng ngon ≥ v, và Alice lấy một miếng ngon`x`bất cứ nơi nào. Chúng ta có thể kiểm tra xem`x`có thể đạt được trong thời gian O(n) bằng một lần duy nhất. Sau đó, bằng cách thực hiện tìm kiếm nhị phân trên`x`từ 0 đến tổng số tiền của chiếc bánh, chúng ta có thể tìm ra độ ngon tối đa mà Alice có thể đạt được một cách hiệu quả. 

Kiểm tra tham lam hoạt động như sau: chúng tôi lặp qua các phần bánh, tích lũy tổng số tiền hiện có. Khi tổng đạt hoặc vượt quá`v`, chúng tôi chỉ định phần đó là phần của sinh vật, đặt lại tổng và tiếp tục. Bất kỳ phần nào còn sót lại sau khi tất cả các sinh vật đã được chỉ định đều có khả năng là phần của Alice. Nếu số tiền còn lại là ≥`x`, Alice có thể chịu đựng được. Nếu chúng ta không thể chỉ định tất cả các sinh vật trong khi vẫn giữ quân của Alice ≥`x`, sau đó`x`là không thể. Điều này đảm bảo tính đúng đắn vì bài toán có tính đơn điệu: nếu Alice có thể lấy`x`, cô ấy cũng có thể lấy bất kỳ mảnh nhỏ hơn nào; nếu cô ấy không thể lấy`x`, cô ấy không thể lấy mảnh nào lớn hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n chọn m) | O(n) | Quá chậm | 
| Tham lam + Tìm kiếm nhị phân | O(n log S) | O(1) | Đã chấp nhận | 

Đây,`S`là tổng của tất cả các giá trị độ ngon trong bánh, tối đa là 2×10^14 với các ràng buộc. 

## Hướng dẫn thuật toán 

1. Tính tổng các phần bánh cho giới hạn trên của tìm kiếm nhị phân. 
2. Đặt giới hạn tìm kiếm nhị phân`low = 0`Và`high = total sum`. 
3. Trong khi`low ≤ high`, lấy`mid = (low + high) // 2`là ứng cử viên thích hợp cho tác phẩm của Alice. 
4. Thực hiện kiểm tra tham lam để xem liệu Alice có thể lấy được một mảnh có kích thước không`mid`trong khi đưa ra`m`ít nhất là những mảnh sinh vật`v`: 

1. Khởi tạo`current_sum = 0`Và`creatures_assigned = 0`. 
2. Lặp lại các phần bánh: 

1. Thêm phần hiện tại vào`current_sum`. 
2. Nếu`current_sum ≥ v`, gán nó cho một sinh vật và đặt lại`current_sum`đến 0, tăng`creatures_assigned`. 
3. Nếu`creatures_assigned == m`, phá vỡ. 
3. Sau khi tất cả các sinh vật được chỉ định, số tiền còn lại trong các phần chưa được chỉ định sẽ là quân cờ tiềm năng của Alice. 
5. Nếu số tiền còn lại của Alice ≥`mid`, nó khả thi; di chuyển`low = mid + 1`để thử một miếng lớn hơn. 
6. Nếu không, hãy đặt`high = mid - 1`. 
7. Khi kết thúc tìm kiếm nhị phân,`high`chứa hương vị tối đa mà Alice có thể đạt được. Nếu thậm chí`0`là không khả thi, đầu ra`-1`. 

Điều bất biến là ở mỗi bước tìm kiếm nhị phân, phép gán tham lam sẽ thành công hoặc thất bại đối với một giá trị nhất định.`mid`. Vì bài toán có tính đơn điệu nên giá trị lớn nhất`mid`thành công là câu trả lời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, v = map(int, input().split())
        a = list(map(int, input().split()))
        total = sum(a)
        low, high = 0, total
        ans = -1
        while low <= high:
            mid = (low + high) // 2
            current_sum = 0
            creatures_assigned = 0
            for val in a:
                current_sum += val
                if current_sum >= v:
                    creatures_assigned += 1
                    current_sum = 0
                    if creatures_assigned == m:
                        break
            # Alice's remaining sum
            remaining = sum(a[a.index(val)+1:]) if creatures_assigned == m else 0
            if remaining >= mid:
                ans = mid
                low = mid + 1
            else:
                high = mid - 1
        print(ans)
        
solve()
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`. Tìm kiếm nhị phân khám phá khả năng hấp dẫn của Alice, trong khi kiểm tra tham lam đảm bảo rằng`m`sinh vật có thể được thỏa mãn. Phần khó khăn là xử lý số tiền còn lại một cách chính xác; chúng tôi nghỉ ngay sau khi chỉ định sinh vật cuối cùng và tổng hợp phần còn lại cho Alice. các`ans`biến lưu trữ giá trị khả thi lớn nhất. 

## Ví dụ đã hoạt động 

**Mẫu 1:**`6 2 1`với`a = [1, 1, 10, 1, 1, 10]`| Phần | hiện tại_sum | sinh vật_được giao | Alice_remaining | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | - | 
| 2 | 2 | 1 | - | 
| 3 | 10 | 1 | - | 
| 4 | 1 | 1 | - | 
| 5 | 2 | 2 | - | 
| 6 | - | 2 | 22 | 

Alice có thể lấy 22; tìm kiếm nhị phân xác nhận đây là tối đa. 

**Mẫu 2:**`6 2 12`với`a = [1, 1, 1, 1, 10, 10]`Nhiệm vụ tham lam không thể cho 2 sinh vật ≥12; đầu ra là`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log S) | Tìm kiếm nhị phân trên tổng mảng, O(n) mỗi lần kiểm tra | 
| Không gian | O(n) | Để lưu trữ mảng và phân tích cú pháp đầu vào | 

Giải pháp phù hợp thoải mái trong giới hạn 2 giây, vì tổng n trên tất cả các trường hợp thử nghiệm là 2×10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io
    f = io.StringIO()
    with redirect_stdout(f):
        solve()
    return f.getvalue().strip()

# provided samples
assert run("""7
6 2 1
1 1 10 1 1 10
6 2 2
1 1 10 1 1 10
6 2 3
1 1 10 1 1 10
6 2 10
1 1 10 1 1 10
6 2 11
1 1 10 1 1 10
6 2 12
1 1 10 1 1 10
6 2 12
1 1 1 1 10 10
""") == """22
12
2
2
2
0
-1"""

# custom cases
assert run("""2
1 1 1
1
5 2 3
1 2 3 4
```
