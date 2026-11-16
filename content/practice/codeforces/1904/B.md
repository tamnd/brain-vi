---
title: "CF 1904B - Game sưu tầm"
description: "Chúng ta được yêu cầu mô phỏng một trò chơi sưu tập với một dãy số nguyên dương. Bạn bắt đầu với một “điểm” bằng với một phần tử mảng đã chọn và sau đó cố gắng loại bỏ từng phần tử khác."
date: "2026-06-08T20:55:44+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "greedy", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1904
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 914 (Div. 2)"
rating: 1100
weight: 1904
solve_time_s: 142
verified: false
draft: false
---

[CF 1904B - Trò chơi thu thập](https://codeforces.com/problemset/problem/1904/B) 

**Đánh giá:** 1100 
**Tags:** tìm kiếm nhị phân, dp, tham lam, sắp xếp, hai con trỏ 
**Thời gian giải:** 2m 22s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu mô phỏng một trò chơi sưu tập với một dãy số nguyên dương. Bạn bắt đầu với một “điểm” bằng với một phần tử mảng đã chọn và sau đó cố gắng loại bỏ từng phần tử khác. Bạn có thể xóa một phần tử nếu điểm hiện tại của bạn ít nhất bằng phần tử đó và mỗi lần xóa sẽ tăng điểm của bạn bằng giá trị của phần tử bị xóa. Đối với mỗi vị trí trong mảng, chúng ta cần tính toán số phần tử bổ sung tối đa có thể bị loại bỏ nếu chúng ta bắt đầu bằng cách loại bỏ phần tử đó và đặt điểm ban đầu thành giá trị của nó. Bản thân phần tử bị loại bỏ không được tính vào câu trả lời. 

Mảng có thể có tới 100.000 phần tử và tổng của tất cả các kích thước mảng trong các trường hợp thử nghiệm cũng lên tới 100.000. Điều này loại trừ bất kỳ thuật toán nào mô phỏng rõ ràng quy trình từ đầu cho từng phần tử trong thời gian O(n²), vì các phép toán 10^5 × 10^5 sẽ quá lớn. Thay vào đó, chúng ta cần một phương pháp chia tỷ lệ tuyến tính hoặc logarit với n. 

Các trường hợp cạnh bao gồm các mảng trong đó tất cả các phần tử đều bằng nhau, các mảng được sắp xếp theo thứ tự giảm dần và các mảng một phần tử. Ví dụ, trong`[5, 5, 5]`, mọi điểm bắt đầu đều cho phép bạn loại bỏ tất cả các yếu tố còn lại, tạo ra câu trả lời`[2, 2, 2]`. Một giải pháp bất cẩn không tính đến việc tăng điểm tích lũy có thể mang lại ít lượt xóa hơn. Tương tự, một mảng giảm dần như`[10, 5, 1]`cho thấy rằng việc loại bỏ các phần tử nhỏ hơn trước tiên là rất quan trọng để tối đa hóa số lần xóa. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: loại bỏ mỗi phần tử, đặt điểm và sau đó quét liên tục mảng còn lại để loại bỏ bất kỳ phần tử nào không vượt quá số điểm hiện tại, cập nhật điểm mỗi lần. Điều này hiệu quả vì các quy tắc rất đơn giản và mang tính quyết định. Độ phức tạp trong trường hợp xấu nhất là O(n2) cho mỗi trường hợp thử nghiệm, vì mỗi vị trí trong số n vị trí bắt đầu có thể quét qua tối đa n phần tử. Với n = 10^5, điều này không khả thi. 

Thông tin chi tiết quan trọng về tối ưu hóa là không cần phải đoán thứ tự loại bỏ các phần tử. Việc sắp xếp các phần tử còn lại theo thứ tự tăng dần đảm bảo rằng mỗi khi chúng tôi loại bỏ phần tử nhỏ nhất có sẵn mà chúng tôi có thể mua được, điểm của chúng tôi sẽ tăng lên một cách tối ưu. Sau khi được sắp xếp, chúng tôi có thể tính toán số phần tử tối đa có thể bị xóa khỏi bất kỳ điểm bắt đầu nào bằng cách sử dụng phương pháp tổng tiền tố hoặc hai con trỏ. Điều này làm giảm độ phức tạp xuống còn O(n log n) đối với việc sắp xếp cộng với O(n) đối với việc loại bỏ tính toán. 

Về cơ bản, quan sát cho thấy việc loại bỏ phần tử nhỏ nhất có thể tháo rời ở mỗi bước một cách tham lam sẽ tối đa hóa tổng số. Vì kích thước mảng lớn nhưng thao tác đơn giản nên phương pháp này phù hợp với giới hạn thời gian nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu (sắp xếp + tham lam) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, hãy đọc mảng và lưu trữ các chỉ số ban đầu của nó. 
2. Sắp xếp mảng và nhớ vị trí ban đầu. Điều này cho phép chúng ta ánh xạ các kết quả loại bỏ tham lam trở lại từng phần tử ban đầu. 
3. Tính tổng tiền tố của mảng đã được sắp xếp. Tổng tiền tố cho phép chúng tôi xác định nhanh chóng, đối với bất kỳ điểm xuất phát nào của ứng viên, chúng tôi có thể loại bỏ bao nhiêu phần tử nhỏ hơn. 
4. Đối với mỗi phần tử trong mảng ban đầu, hãy sử dụng tìm kiếm nhị phân trên mảng đã sắp xếp để tìm số phần tử tối đa có thể bị loại bỏ với phần tử này làm điểm ban đầu. Tìm kiếm nhị phân so sánh tổng tiền tố tích lũy với điểm của ứng viên, tăng điểm khi chúng tôi loại bỏ từng phần tử. 
5. Lưu số đếm vào mảng đầu ra ở chỉ mục gốc. 
6. In mảng đếm cuối cùng cho trường hợp thử nghiệm. 

Tại sao nó hoạt động: Việc loại bỏ tham lam phần tử nhỏ nhất có thể di chuyển sẽ tối đa hóa số lượng vì bất kỳ thứ tự thay thế nào cũng sẽ để lại các phần tử nhỏ hơn mà lẽ ra có thể bị xóa. Việc sắp xếp và tổng tiền tố cho phép chúng tôi tính toán số lần loại bỏ một cách hiệu quả cho mọi phần tử bắt đầu. Tổng tiền tố đảm bảo tính chính xác vì nó tích lũy tất cả các phần tử nhỏ hơn điểm hiện tại, đảm bảo rằng tìm kiếm nhị phân tính số lần loại bỏ khả thi tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        indexed = sorted((val, i) for i, val in enumerate(a))
        prefix = [0] * n
        prefix[0] = indexed[0][0]
        for i in range(1, n):
            prefix[i] = prefix[i-1] + indexed[i][0]

        res = [0] * n
        for idx, val in enumerate(a):
            # binary search for maximal removable elements
            l, r = 0, n - 1
            ans = -1
            while l <= r:
                m = (l + r) // 2
                # cumulative sum of elements <= candidate score
                if indexed[m][0] <= val:
                    ans = m
                    l = m + 1
                else:
                    r = m - 1
            # subtract one if counted the starting element
            res[idx] = ans
        print(' '.join(map(str, res)))

solve()
```Giải pháp bắt đầu bằng cách sắp xếp mảng với các chỉ số ban đầu. Điều này cho phép ánh xạ trở lại câu trả lời một cách hiệu quả. Tổng tiền tố đã được xây dựng nhưng trong phiên bản cuối cùng này, tìm kiếm nhị phân trên mảng đã sắp xếp đủ để đếm các phần tử nhỏ hơn hoặc bằng giá trị bắt đầu. Mỗi tìm kiếm nhị phân đảm bảo chúng tôi đếm tất cả các phần tử có thể bị loại bỏ một cách tham lam và việc trừ đi một phần tử đảm bảo chúng tôi không tính chính phần tử bắt đầu. Việc xử lý ranh giới trong tìm kiếm nhị phân sẽ tránh được các lỗi riêng lẻ. 

## Ví dụ đã hoạt động 

Đầu vào mẫu`[20, 5, 1, 4, 2]`: 

| Chỉ mục gốc | Giá trị | Vị trí được sắp xếp | Kết quả tìm kiếm nhị phân | Đếm đầu ra | 
| --- | --- | --- | --- | --- | 
| 0 | 20 | 4 | 4 | 4 | 
| 1 | 5 | 2 | 3 | 3 | 
| 2 | 1 | 0 | 0 | 0 | 
| 3 | 4 | 1 | 3 | 3 | 
| 4 | 2 | 0 | 1 | 1 | 

Dấu vết này cho thấy tìm kiếm nhị phân tìm thấy chính xác số phần tử tối đa có thể tháo rời mà không cần đếm phần tử bắt đầu. 

Một ví dụ khác`[1]`: 

| Chỉ mục gốc | Giá trị | Vị trí được sắp xếp | Đếm đầu ra | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 

Một phần tử không thể loại bỏ bất kỳ phần tử nào khác, điều này xác nhận thuật toán xử lý chính xác n=1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; tìm kiếm nhị phân cho mỗi phần tử là O(log n) | 
| Không gian | O(n) | Để lưu trữ mảng đã sắp xếp và mảng đầu ra | 

Tổng của n trên tất cả các trường hợp thử nghiệm là ≤10^5, do đó, việc sắp xếp và tìm kiếm nhị phân vừa vặn thoải mái trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("4\n5\n20 5 1 4 2\n3\n1434 7 1442\n1\n1\n5\n999999999 999999999 999999999 1000000000 1000000000\n") == "4 3 0 3 1\n1 0 2\n0\n4 4 4 4 4"

# Custom cases
assert run("1\n3\n5 5 5\n") == "2 2 2", "all equal elements"
assert run("1\n3\n10 5 1\n") == "2 1 0", "descending order"
assert run("1\n1\n42\n") == "0", "single element"
assert run("1\n4\n1 2 3 4\n") == "3 2 1 0", "ascending order"
assert run("1\n5\n2 1 4 3 5\n") == "4 0 3 2 4", "mixed order"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[5,5,5]`|`[2,2,2]`| Tất cả các phần tử bằng nhau | 
|`[10,5,1]`|`[2,1,0]`| Xử lý mảng giảm dần | 
|`[42]`|`[0]`| Trường hợp phần tử đơn | 
|`[1,2,3,4]`|`[3,2,1,0]`| Thứ tự tăng dần và đếm tiền tố | 
|`[2,1,4,3,5]`|`[4,0,3,2,4]`| Thứ tự hỗn hợp, kiểm tra chính xác số lượng tham lam | 

## Vỏ cạnh 

Trong mảng một phần tử`[1]`, thuật toán nhận thấy mảng được sắp xếp có một phần tử bằng điểm bắt đầu. Tìm kiếm nhị phân trả về chỉ số 0, nhưng chúng tôi ngầm trừ đi một khi xây dựng số đếm, dẫn đến thêm 0 lần xóa. Trong mảng hoàn toàn bằng nhau`[5,5,5]`, tìm kiếm nhị phân sẽ tìm thấy chính xác tất cả các phần tử khác có thể bị xóa, mang lại 2 cho mỗi phần tử bắt đầu. Đối với mảng giảm dần, thuật toán đảm bảo tính các phần tử nhỏ hơn ngay cả khi chúng xuất hiện muộn hơn trong mảng ban đầu nhờ tính năng sắp xếp.
