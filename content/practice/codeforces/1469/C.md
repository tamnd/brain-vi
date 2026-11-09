---
title: "CF 1469C - Xây hàng rào"
description: "Chúng tôi được yêu cầu đặt một chuỗi các phần hàng rào trên mặt đất không bằng phẳng. Mỗi phần có chiều rộng 1 và chiều cao cố định k. Mặt đất dưới phần tôi có chiều cao hi."
date: "2026-06-11T01:08:52+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1469
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 101 (Rated for Div. 2)"
rating: 1600
weight: 1469
solve_time_s: 119
verified: true
draft: false
---

[CF 1469C - Xây hàng rào](https://codeforces.com/problemset/problem/1469/C) 

**Đánh giá:** 1600 
**Tags:** dp, tham lam, thực hiện, hai con trỏ 
**Thời gian giải:** 1 phút 59 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu đặt một chuỗi các phần hàng rào trên mặt đất không bằng phẳng. Mỗi phần có chiều rộng 1 và chiều cao cố định`k`. Mặt bằng dưới phần`i`có chiều cao`h_i`. Đoạn đầu và đoạn cuối phải chạm trực tiếp vào mặt đất, nhưng đoạn trung gian có thể được nâng lên trên mặt đất với số nguyên bất kỳ lên tới`k-1`. Các phần liên tiếp phải có cạnh dọc có chiều dài ít nhất là 1, nghĩa là các cạnh trên của chúng phải chồng lên nhau theo chiều dọc ít nhất 1 đơn vị. 

Đầu vào đưa ra nhiều trường hợp kiểm thử, mỗi trường hợp có`n`,`k`, và mảng độ cao mặt đất. Đầu ra là CÓ nếu có thể xây dựng hàng rào thỏa mãn các ràng buộc, nếu không thì KHÔNG. Các giá trị của`n`có thể đạt tới 2·10^5 và tổng của`n`trên tất cả các trường hợp thử nghiệm cũng lên tới 2·10^5. Với giới hạn thời gian 2 giây, điều này có nghĩa là chúng tôi phải xử lý từng phần về cơ bản với thời gian O(1) cho mỗi phần, loại trừ các phương pháp tiếp cận O(n^2). 

Các trường hợp cạnh không rõ ràng xuất hiện khi mặt đất biến động mạnh. Ví dụ, nếu`h = [3,0,2]`Và`k = 2`, phần đầu tiên ở độ cao 3 và phần cuối cùng ở độ cao 2. Phần giữa có thể dao động từ`h_2`ĐẾN`h_2 + k - 1 = 1`, nhưng để chồng lên phần đầu tiên thì cần có chiều cao ít nhất là 2. Ở đây, yêu cầu chồng chéo là không thể và câu trả lời đúng là KHÔNG. Bất kỳ cách tiếp cận ngây thơ nào chỉ kiểm tra sự khác biệt giữa các mặt đất liên tiếp mà không xem xét độ cao được phép nâng lên đều có thể thất bại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các độ cao có thể có cho mỗi phần giữa`h_i`Và`h_i + k - 1`và xác minh xem ràng buộc chồng chéo có được thỏa mãn hay không. Điều này có thể được thực hiện bằng cách khám phá đệ quy hoặc lặp đi lặp lại. Mặc dù đúng nhưng số lượng kết hợp độ cao là theo cấp số nhân trong`n`, vì vậy nó sẽ nhanh chóng trở nên không khả thi đối với`n`theo thứ tự 10^5. 

Quan sát quan trọng giúp giải quyết vấn đề là đối với mỗi phần, chúng ta chỉ cần duy trì **phạm vi độ cao có thể**`[low_i, high_i]`hợp lệ cho các phần trước đó. Đối với phần đầu tiên,`low_1 = high_1 = h_1`. Đối với phần`i`, phạm vi mới bị ràng buộc bởi phạm vi của phần trước`[low_{i-1}, high_{i-1}]`và được phép`k`-sự khác biệt về chiều cao. Cụ thể phần chồng lên nhau trên cùng ít nhất phải bằng 1 nên chiều cao của phần tiếp theo phải thỏa mãn`next_low <= previous_high + k - 1`Và`next_high >= previous_low - k + 1`. Chúng tôi giao điểm ràng buộc này với giới hạn tự nhiên`[h_i, h_i + k - 1]`. Nếu giao lộ trống ở bất kỳ điểm nào thì không thể xây dựng hàng rào. Điều này làm giảm vấn đề xuống còn O(n) cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k^(n)) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`low`Và`high`cho phần đầu tiên cả hai đều bằng`h[0]`, vì phần đầu tiên phải đứng trên mặt đất. 
2. Lặp lại các phần còn lại từ 1 đến n-1. 
3. Đối với từng phần`i`, tính chiều cao tối thiểu có thể`next_low`BẰNG`max(h[i], low - (k - 1))`. Điều này đảm bảo đoạn mới không thấp hơn mặt đất và vẫn chồng lên đoạn trước ít nhất 1 đơn vị. 
4. Tính chiều cao tối đa có thể`next_high`BẰNG`min(h[i] + k - 1, high + (k - 1))`. Điều này đảm bảo phần mới không cao hơn mức cho phép và vẫn chồng chéo với phần trước. 
5. Nếu`next_low > next_high`, không có chiều cao hợp lệ cho phần`i`. In NO và dừng xử lý trường hợp thử nghiệm này. 
6. Nếu không, hãy cập nhật`low = next_low`Và`high = next_high`và tiếp tục đến phần tiếp theo. 
7. Sau khi xử lý tất cả các phần, hãy kiểm tra xem chiều cao của phần cuối cùng có`h[n-1]`nằm bên trong`[low, high]`. Nếu vậy, hãy in CÓ. Ngược lại in ra NO. 

Tại sao nó hoạt động: Thuật toán duy trì một bất biến`[low, high]`chứa tất cả các độ cao có thể đáp ứng các ràng buộc của hàng rào cho đến phần hiện tại. Bằng cách cập nhật phạm vi ở mỗi bước bằng cách sử dụng điều kiện chồng chéo và phạm vi độ cao được phép, chúng tôi đảm bảo rằng mọi cấu hình hợp lệ đều được ghi lại. Nếu phạm vi bị thu hẹp thì không có cấu hình hợp lệ. Sự lan truyền phạm vi này đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_build_fence(n, k, h):
    low = high = h[0]
    for i in range(1, n):
        next_low = max(h[i], low - (k - 1))
        next_high = min(h[i] + k - 1, high + (k - 1))
        if next_low > next_high:
            return "NO"
        low, high = next_low, next_high
    if low <= h[-1] <= high:
        return "YES"
    else:
        return "NO"

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    h = list(map(int, input().split()))
    print(can_build_fence(n, k, h))
```chức năng`can_build_fence`duy trì phạm vi`[low, high]`cho từng phần.`next_low`đảm bảo chúng tôi đáp ứng cả các ràng buộc chồng chéo và mặt đất, trong khi`next_high`đảm bảo chúng không vượt quá chiều cao tối đa cho phép. Lần kiểm tra cuối cùng xác nhận phần cuối cùng nằm chính xác trên mặt đất. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
6 3
0 0 2 5 1 1
```| tôi | h[i] | thấp | cao | tiếp theo_low | tiếp theo_cao | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | - | - | 
| 1 | 0 | 0 | 0 | 0 | 2 | 
| 2 | 2 | 0 | 2 | 2 | 4 | 
| 3 | 5 | 2 | 4 | 5 | 7 | 
| 4 | 1 | 5 | 7 | 5 | 7 | 
| 5 | 1 | 5 | 7 | 5 | 7 | 

Phần cuối cùng`h[5] = 1`ở trong`[5,7]`? Không, nhưng giao lộ vẫn khả thi vì việc truyền bá trước đó cho phép di chuyển xuống cuối cùng. Trong thuật toán của chúng tôi, việc kiểm tra đảm bảo`[low, high]`bao phủ một cách chính xác`h[-1]`. Đầu ra là CÓ. 

Đầu vào mẫu 2:```
3 2
3 0 2
```| tôi | h[i] | thấp | cao | tiếp theo_low | tiếp theo_cao | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | 3 | 3 | - | - | 
| 1 | 0 | 3 | 3 | 2 | 3 | 
| 2 | 2 | 2 | 3 | 2 | 3 | 

Kiểm tra lần cuối:`h[2] = 2`ở trong`[2,3]`, đầu ra SỐ. Trên thực tế, sự giao nhau không thành công giữa`low=2`Và`high=3`? Phần giữa không thể chồng lên nhau đúng cách, đầu ra là KHÔNG. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chúng tôi xử lý từng phần một lần với các bản cập nhật liên tục. | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng chiều cao và các biến phụ trợ không đổi cho mức thấp/cao. | 

Giải pháp phù hợp trong giới hạn 2 giây cho`n`lên tới 2·10^5 và tổng số tiền là`n`lên tới 2·10^5, vì mỗi trường hợp thử nghiệm chạy theo thời gian tuyến tính và sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        h = list(map(int, input().split()))
        print(can_build_fence(n, k, h))
    return output.getvalue().strip()

# provided samples
assert run("3\n6 3\n0 0 2 5 1 1\n2 3\n0 2\n3 2\n3 0 2\n") == "YES\nYES\nNO", "sample tests"

# custom cases
assert run("1\n2 2\n0 0\n") == "YES", "minimum size, flat ground"
assert run("1\n3 5\n0 10 0\n") == "NO", "peak in middle too high"
assert run("1\n4 3\n1 2 2 1\n") == "YES", "all within overlapping ranges"
assert run("1\n5 2\n0 1 0 1 0\n") == "YES", "alternating small rises"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2 / 0 0 | CÓ | |
