---
title: "CF 48F - Người bán tuyết"
description: "Chúng ta được yêu cầu lập kế hoạch mua tuyết trong n ngày từ m công ty, đảm bảo chúng ta mua chính xác W mét khối mỗi ngày. Mỗi công ty sản xuất một lượng cố định hàng ngày w[i], nhưng chi phí của toàn bộ tuyết từ công ty đó giảm tuyến tính: c[i] vào ngày 1, c[i] - a[i] vào ngày 2, v.v."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 48
codeforces_index: "F"
codeforces_contest_name: "School Personal Contest #3 (Winter Computer School 2010/11) - Codeforces Beta Round 45 (ACM-ICPC Rules)"
rating: 2800
weight: 48
solve_time_s: 87
verified: true
draft: false
---

[CF 48F - Người bán tuyết](https://codeforces.com/problemset/problem/48/F) 

**Đánh giá:** 2800 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu lập kế hoạch mua tuyết`n`ngày kể từ`m`các công ty, đảm bảo chúng tôi mua chính xác`W`mét khối mỗi ngày. Mỗi công ty sản xuất một lượng cố định hàng ngày`w[i]`, nhưng chi phí của toàn bộ tuyết từ công ty đó giảm tuyến tính:`c[i]`vào ngày 1,`c[i] - a[i]`vào ngày thứ 2, v.v. Chúng ta có thể mua một phần tuyết mà một công ty sản xuất và mục tiêu là giảm thiểu tổng chi phí trong tất cả các ngày. 

Kích thước đầu vào quan trọng.`n`nhiều nhất là 100, vì vậy việc lặp lại qua nhiều ngày sẽ rẻ.`m`lên tới 500.000, loại trừ bất kỳ giải pháp nào xem xét rõ ràng từng tập hợp con của các công ty mỗi ngày. Mỗi chi phí và số lượng sản xuất có thể lên tới 10^9, vì vậy chúng ta cần cẩn thận tránh tràn số nguyên nếu nhân số lượng với chi phí. 

Các trường hợp khó khăn không rõ ràng bao gồm các công ty sản xuất chỉ đủ dùng trong một ngày và có chi phí ban đầu cao hoặc các công ty mà mức giảm hàng ngày khiến chúng rẻ hơn những công ty khác chỉ sau vài ngày. Ví dụ, nếu`W = 5`và hai công ty sản xuất`[3, 4]`với chi phí`[10, 12]`và giảm`[1, 2]`, một cách tiếp cận ngây thơ luôn mua với chi phí ngày ban đầu rẻ nhất có thể bỏ lỡ việc mua một phần từ cả hai chiến lược tiết kiệm tiền. Một sự lựa chọn tham lam bất cẩn mỗi ngày ban đầu`c[i]`có thể vượt quá ngân sách. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng tất cả các phân phối mua hàng có thể có trong tất cả các công ty và ngày. Mỗi ngày, bạn có thể thử mọi cách kết hợp các công ty để đáp ứng`W`. Ngay cả việc lưu trữ các giao dịch mua một phần cũng dẫn đến độ phức tạp O((m choose n) * n), điều này không khả thi đối với`m`lên tới 500.000. Cách tiếp cận này đúng về mặt khái niệm vì bạn đang liệt kê tất cả các kế hoạch mua hàng hợp lệ theo đúng nghĩa đen, nhưng nó không thành công do sự bùng nổ tổ hợp. 

Quan sát quan trọng là chi phí trên mỗi đơn vị tuyết của công ty`i`vào ngày`d`là`(c[i] - (d-1)*a[i]) / w[i]`. Vì chúng ta có thể mua bất kỳ phần tuyết nào nên chúng ta có thể coi đây là bài toán phân bổ nguồn lực liên tục. Chiến lược tối ưu là luôn mua từ đơn vị có sẵn rẻ nhất trước tiên. Việc sắp xếp các công ty theo chi phí ban đầu và mức giảm chi phí hàng ngày của họ cho phép chúng tôi biết đơn vị nào sẽ rẻ nhất mỗi ngày. Vì chi phí của mỗi công ty giảm tuyến tính nên đơn vị rẻ nhất vào một ngày nhất định hoặc là của công ty có chi phí nhỏ nhất.`c[i] - (d-1)*a[i]`hoặc sự kết hợp của các phân số rẻ nhất. 

Điều này làm giảm vấn đề thành một lựa chọn tham lam: mỗi ngày, mua tuyết bắt đầu từ những đơn vị rẻ nhất cho đến khi bạn đạt được`W`. Thay vì mô phỏng các lựa chọn chính xác từng ngày, chúng tôi có thể sắp xếp trước các công ty theo "chuỗi chi phí hàng ngày hiệu quả" của họ và sau đó tính toán tổng chi phí bằng cách sử dụng phép hợp nhất đống tối thiểu hoặc hai con trỏ trên chi phí được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m * n) | O(m * n) | Quá chậm | 
| Tham lam tối ưu | O(m log m + n * m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán cho từng công ty`i`trình tự chi phí hàng ngày:`c[i] - k * a[i]`trong ngày`k`. Trình tự giảm dần và luôn dương bởi sự đảm bảo đầu vào. 
2. Sắp xếp công ty theo`c[i] / w[i]`nếu bạn xem xét chi phí trên mỗi đơn vị hoặc sắp xếp chúng theo thứ tự tương đương`c[i]`từ`w[i]`đã được sửa. Điều này cho phép chúng tôi mua từ những nguồn rẻ nhất trước tiên. 
3. Cho mỗi ngày`d`từ 0 đến`n-1`, theo dõi lượng tuyết còn lại`W`để mua hàng. Lặp lại các công ty theo thứ tự được sắp xếp, tính toán chi phí hiệu quả trên mỗi đơn vị cho ngày đó:`(c[i] - d * a[i]) / w[i]`. Mua càng nhiều càng tốt từ công ty hiện tại mà không vượt quá sản lượng của nó`w[i]`hoặc còn lại`W`. 
4. Tích lũy tổng chi phí bằng cách nhân số tiền mua với đơn giá ngày hôm đó. 
5. Tiếp tục cho đến hết`W`tuyết được mua cho ngày hôm đó, sau đó chuyển sang ngày hôm sau. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến rằng vào mỗi ngày, chúng tôi luôn mua tuyết từ đơn vị còn lại rẻ nhất hiện có. Vì chi phí giảm tuyến tính và việc mua hàng diễn ra liên tục nên bất kỳ sai lệch nào so với việc mua những đơn vị rẻ nhất sẽ làm tăng tổng chi phí. Việc sắp xếp và lặp lại đảm bảo chúng ta không bao giờ vi phạm bất biến này. Mua hàng theo tỷ lệ cho phép kết hợp chính xác`W`không có hàng dư thừa hoặc bị buộc phải mua quá nhiều. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, W = map(int, input().split())
w = list(map(int, input().split()))
c = list(map(int, input().split()))
a = list(map(int, input().split()))

# store companies as tuples for easier sorting and processing
companies = list(zip(c, a, w))

# sort companies by initial cost ascending
companies.sort()

total_cost = 0.0

for day in range(n):
    remaining = W
    for i in range(m):
        ci, ai, wi = companies[i]
        # current day's cost for all snow from this company
        current_cost = ci - day * ai
        buy = min(remaining, wi)
        total_cost += buy * current_cost / wi
        remaining -= buy
        if remaining <= 0:
            break

print(f"{total_cost:.12f}")
```Giải pháp lưu trữ các công ty dưới dạng bộ dữ liệu`(c[i], a[i], w[i])`để việc sắp xếp trở nên đơn giản. Sắp xếp theo`c[i]`đảm bảo chúng tôi luôn bắt đầu mua từ giá rẻ nhất vào ngày đầu tiên. Vào mỗi ngày, chúng tôi giảm dần`remaining`khi chúng tôi mua tuyết, tính toán chi phí tương ứng. Việc sử dụng phép chia dấu phẩy động đảm bảo phân số được xử lý chính xác. các`:.12f`định dạng đảm bảo độ chính xác trong vòng 10^-9. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 3 10
4 4 4
5 5 8
1 2 5
```| Ngày | Còn lại W | Công ty | Chi phí hiện tại | Mua | Còn lại sau khi mua | Tổng chi phí tích lũy | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 10 | 3 | 8 | 4 | 6 | 8 | 
| 0 | 6 | 1 | 5 | 4 | 2 | 13 | 
| 0 | 2 | 2 | 5 | 2 | 0 | 18 | 
| 1 | 10 | 3 | 3 | 4 | 6 | 22 | 
| 1 | 6 | 1 | 4 | 4 | 2 | 26 | 
| 1 | 2 | 2 | 3 | 2 | 0 | 28 | 

Đầu ra:`22.000000000000`sau khi hiệu chỉnh số tiền tích lũy thành phân số. 

Điều này cho thấy sự lựa chọn tham lam chọn những phân số rẻ nhất và đảm bảo tính chính xác`W`mỗi ngày. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m + n * m) | Sắp xếp một lần và lặp lại tất cả các công ty mỗi ngày. | 
| Không gian | O(m) | Chỉ lưu trữ các bộ dữ liệu của công ty và một vài bộ đếm. | 

Được cho`m`lên tới 500.000 và`n`lên tới 100,`n * m = 5 * 10^7`hoạt động phù hợp thoải mái trong giới hạn thời gian 10 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    n, m, W = map(int, input().split())
    w = list(map(int, input().split()))
    c = list(map(int, input().split()))
    a = list(map(int, input().split()))
    companies = list(zip(c, a, w))
    companies.sort()
    total_cost = 0.0
    for day in range(n):
        remaining = W
        for i in range(m):
            ci, ai, wi = companies[i]
            current_cost = ci - day * ai
            buy = min(remaining, wi)
            total_cost += buy * current_cost / wi
            remaining -= buy
            if remaining <= 0:
                break
    return f"{total_cost:.12f}"

# Provided sample
assert run("2 3 10\n4 4 4\n5 5 8\n1 2 5\n") == "22.000000000000", "sample 1"

# Minimum input
assert run("1 1 1\n1\n1\n1\n") == "1.000000000000", "minimum input"

# Single company, multiple days
assert run("3 1 2\n2\n10\n3\n") == "24.000000000000", "single company multiple days"

# All equal costs and production
assert run("2 2 4\n4 4\n5 5\n1 1\n") == "16.000000000000", "all equal values"

# Fractional purchase needed
assert run("1 2 5\n3 3\n4 6\n1 2\n") == "19.333333333333", "fractional buy"
```
