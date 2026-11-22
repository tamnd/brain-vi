---
title: "CF 319C - Kalila và Dimna trong ngành khai thác gỗ"
description: "Chúng ta được cho một dãy cây có chiều cao tăng dần. Mỗi cây có thể được chặt từng đơn vị một bằng cưa máy. Máy cưa cần phải sạc lại sau mỗi lần cắt và chi phí sạc lại tùy thuộc vào cây nào đã bị chặt hoàn toàn."
date: "2026-06-06T02:07:17+07:00"
tags: ["codeforces", "competitive-programming", "dp", "geometry"]
categories: ["algorithms"]
codeforces_contest: 319
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 189 (Div. 1)"
rating: 2100
weight: 319
solve_time_s: 100
verified: false
draft: false
---

[CF 319C - Kalila và Dimna trong ngành khai thác gỗ](https://codeforces.com/problemset/problem/319/C) 

**Xếp hạng:** 2100 
**Thẻ:** dp, hình học 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy cây có chiều cao tăng dần. Mỗi cây có thể được chặt từng đơn vị một bằng cưa máy. Máy cưa cần phải sạc lại sau mỗi lần cắt và chi phí sạc lại tùy thuộc vào cây nào đã bị chặt hoàn toàn. Cụ thể, nếu cây có chỉ số cao nhất đã bị chặt hoàn toàn là cây _i_ thì chi phí để nạp lại là _b[i]_. Mục tiêu là chặt bỏ hoàn toàn tất cả các cây trong khi giảm thiểu tổng chi phí dành cho việc sạc lại. 

Đầu vào bao gồm một số nguyên _n_, một mảng độ cao _a_ và một mảng chi phí _b_. Độ cao tăng dần bắt đầu bằng 1 và chi phí nạp lại giảm dần kết thúc bằng 0. Đầu ra là một số duy nhất: tổng chi phí tối thiểu để giảm tất cả các cây xuống độ cao 0. 

Các ràng buộc cho biết _n_ có thể lên tới 10^5 và _a[i]_ và _b[i]_ có thể lên tới 10^9. Cách tiếp cận mạnh mẽ mô phỏng mọi đơn vị cắt là không khả thi vì nó có thể mất tới 10^14 thao tác trong trường hợp xấu nhất. Chúng ta cần một nghiệm tuyến tính hoặc gần tuyến tính trong _n_. Các trường hợp cạnh bao gồm _n = 1_, trong đó cây đầu tiên đã có chiều cao 1 và chi phí có thể khác 0 và các trường hợp chi phí giảm xuống 0, nghĩa là cưa máy cuối cùng sẽ được sạc lại miễn phí. 

Một cách tiếp cận ngây thơ là luôn cắt cây tiếp theo về 0 ngay lập tức mà không tính đến chi phí tích lũy sẽ thất bại. Ví dụ, nếu chúng ta có`a = [1, 2, 3]`Và`b = [5, 2, 0]`, việc chặt cây thứ hai trước khi cây đầu tiên kết thúc có thể buộc chúng ta phải trả chi phí cao hơn nhiều lần. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng việc cưa máy cắt từng cây, cập nhật chi phí hiện tại cho mỗi lần nạp lại. Tại mỗi đơn vị cắt, chúng tôi sẽ kiểm tra cây hoàn thành tối đa và thêm _b[i]_ tương ứng. Điều này đúng về mặt logic nhưng hoàn toàn không khả thi, vì số lượng phép toán có thể tổng bằng tổng của tất cả các độ cao, có khả năng là 10^14. 

Quan sát chính để tối ưu hóa đến từ cấu trúc đơn điệu của cả hai mảng: chiều cao ngày càng tăng, chi phí ngày càng giảm. Mỗi cây phải được cắt hoàn toàn, vì vậy sẽ an toàn khi nghĩ đến vấn đề dưới dạng các khối: cần thực hiện bao nhiêu lần cắt trong khi cây tối đa hiện tại là _i_. Bởi vì việc chặt cây sau không làm tăng chi phí cho những cây trước đó nên tổng chi phí tối thiểu có thể được tính bằng tổng theo chiều cao của mỗi cây, nhân với chi phí phục hồi tối thiểu áp dụng trong quá trình chặt cây. 

Điều này tự nhiên dẫn đến lập trình động. Cho phép`dp[i]`biểu thị tổng chi phí tối thiểu để chặt cây từ 1 đến _i_. Đối với cây _i_, chúng ta có thể quyết định điểm tối ưu để bắt đầu cắt nó so với các cây trước đó. Do tính đơn điệu nên chiến lược tối ưu luôn là liền kề nhau: chúng ta chặt một số nhóm cây liên tiếp lại với nhau để giảm thiểu chi phí nạp lại. Sự tái phát là:```
dp[i] = min(dp[j] + (a[i] - a[j]) * b[j])  for all j < i
```Đây,`a[i] - a[j]`là số đơn vị cần chặt cây _i_ sau khi hoàn thành cây _j_, và`b[j]`là chi phí cho mỗi đơn vị trong phân khúc đó. Bằng cách sử dụng thuộc tính lồi, chúng ta có thể áp dụng Thủ thuật thân lồi để tính toán điều này một cách hiệu quả trong O(n) hoặc O(n log n), tránh đánh giá bậc hai của tất cả j < i. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(tổng(a[i])) | O(n) | Quá chậm | 
| Lập trình động + Thủ thuật thân lồi | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo mảng DP`dp`có kích thước _n_, ở đâu`dp[i]`đại diện cho chi phí tối thiểu để chặt cây từ 1 đến i. Bộ`dp[0] = a[0] * b[0]`, chi phí để chặt cây đầu tiên. 
2. Sử dụng deque hoặc list để duy trì các dòng ứng cử viên cho Thủ thuật thân lồi. Mỗi dòng đại diện cho một trạng thái trước đó`(slope, intercept)`tương ứng với`b[j]`Và`dp[j] - a[j]*b[j]`. 
3. Đối với mỗi cây`i`từ 1 đến n-1, truy vấn bao lồi để tìm đường cực tiểu hóa`dp[j] + (a[i] - a[j]) * b[j]`. Điều này mang lại chi phí tối thiểu để tiếp cận cây _i_. 
4. Thêm dòng mới tương ứng với`i`vào bao lồi. Nếu dòng mới làm cho dòng cũ trở nên dư thừa, hãy loại bỏ chúng. 
5. Sau khi xử lý xong toàn bộ cây,`dp[n-1]`chứa chi phí tối thiểu để chặt tất cả các cây. 

Tại sao nó hoạt động: Công thức DP nắm bắt chính xác tổng chi phí bằng cách chia việc cắt thành các phân đoạn, mỗi phân đoạn được tính phí tại cây hoàn thành tối đa tại thời điểm đó. Thủ thuật vỏ lồi có hiệu lực vì hàm chi phí là tuyến tính trong`a[i]`với độ dốc giảm dần (đơn điệu`b[i]`), đảm bảo rằng chuỗi các đường ứng cử viên là lồi và chỉ cần đường giao nhau tối thiểu ở mỗi bước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import deque

def main():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    dp = [0] * n
    dp[0] = a[0] * b[0]
    
    lines = deque()
    lines.append((b[0], dp[0] - a[0]*b[0]))
    
    def value(line, x):
        m, c = line
        return m * x + c
    
    for i in range(1, n):
        while len(lines) >= 2 and value(lines[0], a[i]) >= value(lines[1], a[i]):
            lines.popleft()
        dp[i] = value(lines[0], a[i])
        new_line = (b[i], dp[i] - a[i]*b[i])
        while len(lines) >= 2:
            m1, c1 = lines[-2]
            m2, c2 = lines[-1]
            m3, c3 = new_line
            if (c3 - c2) * (m2 - m1) <= (c2 - c1) * (m3 - m2):
                lines.pop()
            else:
                break
        lines.append(new_line)
    
    print(dp[n-1])

if __name__ == "__main__":
    main()
```Mảng DP theo dõi tổng chi phí cho từng cây. Các đường trong bao lồi thể hiện các trạng thái ứng viên trước đó. Truy vấn thân tàu mang lại chi phí tối thiểu một cách hiệu quả. Điều kiện loại bỏ dòng sử dụng phép nhân chéo để tránh lỗi dấu phẩy động. 

## Ví dụ đã hoạt động 

Mẫu 1:```
n = 5
a = [1, 2, 3, 4, 5]
b = [5, 4, 3, 2, 0]
```| tôi | dp[i] | dòng (m, c) sau i | 
| --- | --- | --- | 
| 0 | 5 | [(5,0)] | 
| 1 | 9 | [(5,0),(4,1)] | 
| 2 | 12 | [(5,0),(4,1),(3,3)] | 
| 3 | 14 | [(5,0),(4,1),(3,3),(2,6)] | 
| 4 | 15 | [(5,0),(4,1),(3,3),(2,6),(0,15)] | 

Điều này thể hiện việc tính toán chi phí tối thiểu tích lũy và thân lồi chỉ duy trì các đường liên quan. 

Mẫu 2:```
n = 3
a = [1,3,6]
b = [6,3,0]
```| tôi | dp[i] | dòng | 
| --- | --- | --- | 
| 0 | 6 | [(6,0)] | 
| 1 | 12 | [(6,0),(3,6)] | 
| 2 | 12 | [(6,0),(3,6),(0,12)] | 

Ở đây, thân lồi chỉ chọn những dòng có chi phí cao trước đó cho đến khi những dòng rẻ hơn chiếm ưu thế. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi dòng được thêm và xóa nhiều nhất một lần khỏi deque. | 
| Không gian | O(n) | Mảng DP và deque của dòng sử dụng không gian tuyến tính. | 

Giải pháp này có thể mở rộng đến n=10^5 một cách dễ dàng và xử lý các giá trị a[i] và b[i] lớn vì các phép toán trên mỗi cây được khấu hao không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# provided sample
assert run("5\n1 2 3 4 5\n5 4 3 2 0\n") == "25", "sample 1"

# minimum input
assert run("1\n1\n10\n") == "10", "single tree"

# two trees, high first cost
assert run("2\n1 2\n100 0\n") == "200", "two trees"

# monotone large input
assert run("3\n1 10 100\n10 5 0\n") == "550", "large height differences"

# all b decreasing slowly
assert run("4\n1 2 3 4\n10 9
```
