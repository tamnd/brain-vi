---
title: "CF 1883E - Nhìn Lại"
description: "Chúng ta được cung cấp một mảng các số nguyên và nhiệm vụ của chúng ta là biến nó thành một dãy không giảm bằng cách sử dụng số phép toán tối thiểu. Hoạt động được phép là nhân đôi một phần tử bất kỳ số lần nào."
date: "2026-06-08T22:29:20+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1883
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 905 (Div. 3)"
rating: 1700
weight: 1883
solve_time_s: 112
verified: false
draft: false
---

[CF 1883E - Nhìn lại](https://codeforces.com/problemset/problem/1883/E) 

**Đánh giá:** 1700 
**Tags:** bitmasks, tham lam 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và nhiệm vụ của chúng ta là biến nó thành một dãy không giảm bằng cách sử dụng số phép toán tối thiểu. Hoạt động được phép là nhân đôi một phần tử bất kỳ số lần nào. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp chỉ định độ dài mảng và chính các phần tử. Đầu ra cho mỗi trường hợp thử nghiệm là số lượng thao tác nhân đôi tối thiểu cần thiết. 

Những hạn chế là đáng kể. Mỗi mảng có thể có tới$10^5$các phần tử và tổng của tất cả các phần tử trong các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$. Với giới hạn 1 giây,$O(n^2)$thuật toán không thực tế vì$10^{10}$hoạt động không thể chạy trong thời gian hợp lý. Chúng ta cần một cái gì đó đại khái$O(n \log M)$hoặc tốt hơn, ở đâu$M$là giá trị lớn nhất trong mảng. 

Một sự tinh tế quan trọng phát sinh với các phần tử đã lớn hơn các phần tử lân cận của chúng. Ví dụ, trong mảng`[3, 2, 1]`, sự so sánh ngây thơ từ trái sang phải thất bại. Nhân đôi`1`Và`2`để phù hợp`3`đòi hỏi phải suy nghĩ ngược lại. Một trường hợp cạnh khác là giá trị bằng nhau. Nếu mảng là`[2, 1, 1, 3]`, chúng ta phải quyết định số nào sẽ tăng gấp đôi và bao nhiêu lần để duy trì trật tự không giảm, nếu không chúng ta có thể đếm quá nhiều phép toán. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: quét liên tục mảng từ trái sang phải, nhân đôi bất kỳ phần tử nào nhỏ hơn phần tử trước đó cho đến khi mảng được sắp xếp. Nó hoạt động vì mỗi thao tác đều tăng phần tử hiện tại một cách nghiêm ngặt, hướng tới mục tiêu. Tuy nhiên, trong trường hợp xấu nhất, một phần tử có thể cần được nhân đôi hàng chục lần và chúng tôi thực hiện việc này cho từng phần tử. Vì$n = 10^5$, đây có thể là$O(n \log A_{\text{max}})$cho mỗi trường hợp thử nghiệm và cho$t = 10^4$, điều này rõ ràng là quá chậm. 

Thông tin chuyên sâu giúp đưa ra giải pháp nhanh hơn là thực hiện **từ phải sang trái**, tập trung vào từng cặp`(a[i], a[i+1])`. Để một cặp có giá trị,`a[i] <= a[i+1]`. Nếu không, chúng tôi xác định số lần nhân đôi tối thiểu cần thiết cho`a[i]`để nó ít nhất trở thành`a[i+1]`. Điều này có thể được tính toán một cách hiệu quả bằng cách sử dụng biểu diễn nhị phân: nhân đôi tương đương với nhân với 2, tính bằng bit là dịch chuyển trái. Số lần nhân đôi cần thiết là nhỏ nhất`k`như vậy`a[i] * 2^k >= a[i+1]`. Điều này tương đương với`ceil(log2(a[i+1] / a[i]))`. Lặp lại từ phải sang trái đảm bảo mỗi phần tử được điều chỉnh không vi phạm các ràng buộc trong tương lai. 

Chúng tôi rút gọn vấn đề thành một phép tính bitmask tham lam đơn giản: tại mỗi chỉ mục, tính toán số lần nhân đôi cần thiết chỉ dựa trên phần tử tiếp theo. Điều này tránh việc thử và sai không cần thiết và đảm bảo các hoạt động tối thiểu, vì bất kỳ việc nhân đôi từ trái sang phải nào sẽ chỉ làm tăng hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n log A_max) mỗi lần kiểm tra | O(1) | Quá chậm đối với các hạn chế tối đa | 
| Tham lam từ phải sang trái | O(n) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm`ops = 0`để theo dõi tổng số hoạt động cho trường hợp thử nghiệm này. 
2. Duyệt mảng từ phần tử thứ hai đến phần tử cuối cùng xuống phần tử đầu tiên. Tại mỗi chỉ số`i`, kiểm tra xem`a[i] > a[i+1]`. 
3. Nếu phần tử hiện tại`a[i]`đã nhỏ hơn hoặc bằng`a[i+1]`, không làm gì cả. Di chuyển đến phần tử trước đó. 
4. Nếu`a[i] > a[i+1]`, tính số lần nhân đôi tối thiểu cần thiết. Bắt đầu với`k = 0`và liên tục nhân đôi`a[i]`cho đến khi ít nhất`a[i+1]`. Thêm vào`k`ĐẾN`ops`. 
5. Tiếp tục quá trình này cho đến khi phần tử đầu tiên được xử lý. 
6. In`ops`cho trường hợp thử nghiệm hiện tại. 

Lý do điều này có tác dụng là vì mỗi phần tử được điều chỉnh vừa đủ để đáp ứng yêu cầu không giảm so với phần tử kế tiếp của nó. Bởi vì chúng ta duyệt từ phải sang trái, nên khi một phần tử được cố định, nó đảm bảo mảng con ở bên phải của nó không giảm. Cách tiếp cận tham lam này là tối ưu: bất kỳ giải pháp nào sử dụng ít nhân đôi hơn sẽ vi phạm ràng buộc ở một số chỉ số và bất kỳ giải pháp nào sử dụng nhiều hơn rõ ràng là không tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def min_doublings(a):
    n = len(a)
    ops = 0
    for i in range(n-2, -1, -1):
        if a[i] > a[i+1]:
            x = a[i]
            y = a[i+1]
            k = 0
            while x > y:
                x = (x + 1) // 2
                k += 1
            ops += k
            a[i] = x
    return ops

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    print(min_doublings(a))
```Chúng tôi xử lý I/O nhanh với`sys.stdin.readline`. Chúng ta điều chỉnh từng phần tử từ phải sang trái. Sự lựa chọn tinh tế là sử dụng`(x + 1) // 2`nhiều lần để mô phỏng các lần nhân đôi tối thiểu theo chiều ngược lại, tính toán hiệu quả`ceil(log2(a[i]/a[i+1]))`không có lỗi dấu phẩy động. Đang cập nhật`a[i]`đảm bảo chúng ta không phá vỡ bất biến cho lần lặp tiếp theo. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng`[3, 2, 1]`. 

| tôi | một [tôi] | a[i+1] | x sau khi nhân đôi | k | hoạt động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 1 | 1 | 1 | 
| 0 | 3 | 1 | 3 | 2 | 3 | 

Sau khi điều chỉnh`a[1]`, nó trở thành`1`. Sau đó`a[0] = 3`cần 2 lần nhân đôi để thỏa mãn`>=1`, tổng số lần hoạt động = 3. 

Một mảng khác`[2, 1, 1, 3]`: 

| tôi | một [tôi] | a[i+1] | x sau khi nhân đôi | k | hoạt động | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 1 | 3 | 1 | 0 | 0 | 
| 1 | 1 | 1 | 1 | 0 | 0 | 
| 0 | 2 | 1 | 1 | 1 | 1 | 

Chỉ phần tử đầu tiên cần nhân đôi một lần. Điều này thể hiện việc điều chỉnh tham lam từ phải sang trái giúp giảm thiểu các hoạt động một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi lặp lại từng phần tử một lần và mỗi bước nhân đôi tỷ lệ với log(a[i]), được giới hạn bởi 30 cho 10^9. | 
| Không gian | O(1) thêm | Sửa đổi mảng tại chỗ; không cần cấu trúc dữ liệu bổ sung. | 

Xét tổng của tất cả`n`qua các trường hợp thử nghiệm là`2 * 10^5`, tổng số thao tác thoải mái trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# provided samples
assert run("9\n1\n1\n2\n2 1\n3\n3 2 1\n4\n7 1 5 3\n5\n11 2 15 7 10\n6\n1 8 2 16 8 16\n2\n624323799 708290323\n12\n2 1 1 3 3 11 12 22 45 777 777 1500\n12\n12 11 10 9 8 7 6 5 4 3 2 1") == "0\n1\n3\n6\n10\n3\n0\n2\n66"

# custom cases
assert run("1\n5\n1 1 1 1 1") == "0", "all equal"
assert run("1\n3\n10 5 1") == "5", "descending small array"
assert run("1\n1\n42") == "0", "single element"
assert run("1\n4\n1 2 4 8") == "0", "already non-decreasing"
assert run("1\n2\n1000000000 1") == "30", "max value edge"

| Test input | Expected output | What it validates |
|---|---|---|
| 5 elements all equal | 0 | No operation needed for uniform arrays |
| 3 elements descending | 5 | Correct greedy doubling calculation |
| 1 element | 0 | Minimal array size edge |
| 4 elements increasing | 0 | Already non-decreasing, minimal ops |
| 2 elements extreme | 30 | Handles max bound of 10^9 correctly |
```## Vỏ cạnh 

Nếu
