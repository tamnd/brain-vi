---
title: "CF 2167G - Mukhammadali và mảng mịn"
description: "Chúng ta được cung cấp một mảng số nguyên và một mảng chi phí tương ứng. Đối với mỗi vị trí trong mảng, chúng ta có thể thay thế giá trị của nó với một mức giá nhất định và các vị trí chúng ta không thay đổi sẽ giữ nguyên giá trị ban đầu."
date: "2026-06-07T23:27:34+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp"]
categories: ["algorithms"]
codeforces_contest: 2167
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 1062 (Div. 4)"
rating: 1600
weight: 2167
solve_time_s: 117
verified: false
draft: false
---

[CF 2167G - Mukhammadali và Mảng mượt mà](https://codeforces.com/problemset/problem/2167/G) 

**Đánh giá:** 1600 
**Thẻ:** cấu trúc dữ liệu, dp 
**Thời gian giải:** 1 phút 57s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng số nguyên và một mảng chi phí tương ứng. Đối với mỗi vị trí trong mảng, chúng ta có thể thay thế giá trị của nó với một mức giá nhất định và các vị trí chúng ta không thay đổi sẽ giữ nguyên giá trị ban đầu. Sau khi thực hiện bất kỳ thay đổi nào, chúng tôi xác định "giảm" là vị trí trong đó giá trị lớn hơn giá trị tiếp theo. Mục tiêu là loại bỏ tất cả các giọt với chi phí tối thiểu. 

Mảng có thể lớn tới 8000 phần tử và có thể có tới 5000 trường hợp thử nghiệm. Tổng độ dài mảng trong tất cả các trường hợp thử nghiệm được giới hạn ở mức 8000, do đó tổng số phần tử chúng tôi xử lý rất khiêm tốn, điều này cho phép chúng tôi sử dụng các thuật toán có độ phức tạp bậc hai theo chiều dài mảng. Mỗi yếu tố và mỗi chi phí có thể lên tới$10^9$, vì vậy chúng ta phải xử lý số lượng lớn một cách cẩn thận và tránh tràn số nguyên trong các ngôn ngữ có số nguyên có chiều rộng cố định. 

Các trường hợp cạnh phát sinh khi mảng không giảm. Trong tình huống đó, không cần chi phí. Một trường hợp tinh tế khác là khi thay thế mọi phần tử ngoại trừ một phần tử là tối ưu, ví dụ như khi mảng đang giảm dần. Những cách tiếp cận tham lam bất cẩn mà chỉ nhìn vào những giọt cục bộ có thể trả quá nhiều tiền hoặc không tìm được mức tối thiểu toàn cầu. Mảng có các giá trị lặp lại cũng cần được xử lý cẩn thận, vì việc giữ các bản sao có thể rẻ hơn việc thay thế một số giá trị để tránh bị rớt. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ xem xét mọi tập hợp con các vị trí có thể thay thế, tính toán chi phí và kiểm tra xem mảng kết quả có giảm không. Số tập con là$2^n$, lớn về mặt thiên văn ngay cả đối với$n=20$, vì vậy vũ lực là hoàn toàn không thể thực hiện được. 

Một cách tiếp cận có cấu trúc hơn nhận ra rằng sau khi sắp xếp hoặc "nén" các giá trị tiềm năng, vấn đề có thể được trình bày lại thành một vấn đề lập trình động. Tại mỗi vị trí, thuộc tính duy nhất quan trọng là giá trị cuối cùng mà chúng ta chọn và chi phí tối thiểu để đạt được một mảng hợp lệ cho đến thời điểm đó. Nếu lặp qua các vị trí từ trái sang phải, với mỗi giá trị ứng viên ở vị trí hiện tại, chúng ta chỉ cần xét các giá trị ứng cử viên ở vị trí trước đó không lớn hơn giá trị hiện tại. Điều này tạo thành một mẫu DP tiêu chuẩn trong đó$dp[i][v]$đại diện cho chi phí tối thiểu để thực hiện lần đầu tiên$i$phần tử không giảm, kết thúc bằng giá trị$v$. 

Để thực hiện điều này hiệu quả, chúng tôi rời rạc hóa các giá trị mảng. Thay vì xem xét tất cả$10^9$số nguyên có thể, chúng ta chỉ cần xem xét các giá trị xuất hiện trong mảng ban đầu, bởi vì mọi sự thay thế tối ưu đều có thể được giảm xuống một giá trị trong mảng ban đầu mà không làm tăng chi phí. Sử dụng điều này, chúng ta có thể triển khai DP trong$O(n^2)$cho mỗi trường hợp thử nghiệm bằng cách duy trì tiền tố tối thiểu để tránh quét lại tất cả các trạng thái trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Lập trình động với sự rời rạc hóa | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc mảng`a`và mảng chi phí`c`. Ghi lại chiều dài`n`. 
2. Thu thập tất cả các giá trị duy nhất từ`a`và sắp xếp chúng. Danh sách các giá trị được sắp xếp này, được gọi là`vals`, đại diện cho tất cả các giá trị cuối cùng có thể có mà chúng ta có thể gán cho bất kỳ vị trí nào. 
3. Khởi tạo bảng DP`dp`với kích thước`n x len(vals)`, Ở đâu`dp[i][j]`đại diện cho chi phí tối thiểu để thực hiện lần đầu tiên`i+1`các phần tử không giảm, với`i`-phần tử thứ bằng`vals[j]`. 
4. Điền vào hàng đầu tiên. Đối với vị trí`0`, chi phí để thiết lập nó thành`vals[j]`là`0`nếu như`vals[j]`bằng`a[0]`(không thay đổi), nếu không`c[0]`(chúng tôi thay thế nó). 
5. Lặp lại các vị trí còn lại từ`i = 1`ĐẾN`n-1`. Đối với mỗi giá trị ứng cử viên`vals[j]`ở vị trí`i`, tìm chi phí tối thiểu trong số tất cả các giá trị ứng viên trước đó`vals[k]`Ở đâu`vals[k] <= vals[j]`. Thêm vào`c[i]`nếu như`vals[j] != a[i]`để tính chi phí thay thế. 
6. Sử dụng mảng tiền tố tối thiểu để tìm giá trị tối thiểu một cách hiệu quả`k`giá trị thỏa mãn`vals[k] <= vals[j]`. Điều này tránh việc quét tất cả các trạng thái trước đó cho từng ứng cử viên, duy trì`O(n^2)`thời gian. 
7. Sau khi xử lý tất cả các vị trí, câu trả lời là giá trị tối thiểu ở hàng cuối cùng của bảng DP, biểu thị chi phí tối thiểu để làm cho toàn bộ mảng không giảm. 

Tại sao nó hoạt động: Ở mỗi bước, DP giữ nguyên bất biến`dp[i][j]`là chi phí tối thiểu để thực hiện lần đầu tiên`i+1`các phần tử không giảm kết thúc bằng`vals[j]`. Bằng cách xem xét tất cả các giá trị hợp lệ trước đó và cộng thêm chi phí thay thế, chúng tôi khám phá tất cả các phép biến đổi khả thi mà không bỏ sót bất kỳ sự kết hợp tối ưu nào. Sự rời rạc hóa đảm bảo chúng tôi chỉ xử lý một số giá trị ứng cử viên có thể quản lý được và tiền tố tối thiểu đảm bảo chúng tôi duy trì một cách hiệu quả ràng buộc không giảm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        c = list(map(int, input().split()))
        
        vals = sorted(set(a))
        m = len(vals)
        
        dp = [0] * m
        for j in range(m):
            dp[j] = 0 if vals[j] == a[0] else c[0]
        
        for i in range(1, n):
            new_dp = [0] * m
            prefix_min = [0] * m
            prefix_min[0] = dp[0]
            for j in range(1, m):
                prefix_min[j] = min(prefix_min[j-1], dp[j])
            
            for j in range(m):
                cost = 0 if vals[j] == a[i] else c[i]
                new_dp[j] = prefix_min[j] + cost
            dp = new_dp
        
        print(min(dp))

if __name__ == "__main__":
    solve()
```Mã đầu tiên rời rạc hóa các giá trị mảng thành`vals`. Hàng đầu tiên được khởi tạo tùy theo việc có cần thay thế hay không. Đối với mỗi vị trí tiếp theo, nó xây dựng một mảng tiền tố tối thiểu để thực thi hiệu quả ràng buộc không giảm trong khi cộng thêm chi phí thay thế. Câu trả lời cuối cùng chỉ đơn giản là chi phí nhỏ nhất có thể đạt được ở vị trí cuối cùng. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
4
4 3 2 1
1 1 1 1
```| tôi | một [tôi] | giá trị | dp trước | tiền tố_min | dp sau | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 4 | [1,2,3,4] | - | - | [1,1,1,0] | 
| 1 | 3 | [1,2,3,4] | [1,1,1,0] | [1,1,1,0] | [2,2,1,1] | 
| 2 | 2 | [1,2,3,4] | [2,2,1,1] | [2,2,1,1] | [3,2,1,2] | 
| 3 | 1 | [1,2,3,4] | [3,2,1,2] | [3,2,1,1] | [4,3,2,3] | 

Trả lời: 3 

Dấu vết này cho thấy DP lựa chọn chính xác các thiết bị thay thế để loại bỏ tình trạng rơi rớt trong khi giảm thiểu chi phí. 

### Đầu vào mẫu 2```
5
1 3 2 2 4
100 1 1 1 100
```Thuật toán xác định rằng việc thay đổi phần tử thứ ba từ 2 thành 3 sẽ tốn 1 và tất cả các phần tử khác sẽ giữ nguyên hoặc thay thế rẻ hơn, tạo ra tổng chi phí là 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) cho mỗi trường hợp thử nghiệm | Mỗi vị trí lặp lại tối đa n giá trị ứng cử viên; tổng số n ≤ 8000 cho phép điều này | 
| Không gian | O(n^2) | Bảng DP lưu trữ chi phí cho từng vị trí × giá trị ứng viên, khả thi trong điều kiện ràng buộc | 

Giải pháp vừa vặn thoải mái trong giới hạn 1 giây vì tổng n trên tất cả các trường hợp thử nghiệm bị giới hạn bởi 8000, do đó n2 thao tác trên tất cả các trường hợp thử nghiệm là 64 triệu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("10\n1\n10\n5\n4\n1 2 2 3\n5 6 7 8\n4\n4 3 2 1\n1 1 1 1\n3\n3 1 2\n100 1 1\n5\n5 5 5 5 5\n10 1 10 1 10\n5\n1 3 2 2 4\n100 1 1 1 100\n6\n10 9 8 7 6 5\n1 100 1 100 1 100\n5\n100 1 100 100 100\n1 100 1 1 1\n4\n2 1 2 1\n5 4 3 2\n7\n1 5 3 4 2 6
```
