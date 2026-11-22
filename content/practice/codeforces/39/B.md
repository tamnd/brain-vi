---
title: "CF 39B - Tăng trưởng thu nhập công ty"
description: "Chúng ta được cho một dãy số nguyên biểu thị thu nhập hàng năm của một công ty bắt đầu từ năm 2001. Số đầu tiên là thu nhập năm 2001, số thứ hai là thu nhập năm 2002, v.v. Những giá trị này có thể âm nếu công ty bị lỗ trong năm đó."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 39
codeforces_index: "B"
codeforces_contest_name: "School Team Contest 1 (Winter Computer School 2010/11)"
rating: 1300
weight: 39
solve_time_s: 207
verified: true
draft: false
---
[CF 39B - Tăng trưởng thu nhập của công ty](https://codeforces.com/problemset/problem/39/B) 

**Đánh giá:** 1300 
**Thẻ:** tham lam 
**Thời gian giải:** 3m 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên biểu thị thu nhập hàng năm của một công ty bắt đầu từ năm 2001. Số đầu tiên là thu nhập năm 2001, số thứ hai là thu nhập năm 2002, v.v. Những giá trị này có thể âm nếu công ty bị lỗ trong năm đó. Petya muốn đưa ra một kịch bản tăng trưởng "hoàn hảo", trong đó thu nhập tăng tuyến tính 1 tỷ mỗi năm: 1 tỷ trong năm đầu tiên, 2 tỷ trong năm thứ hai, 3 tỷ trong năm thứ ba, v.v. Vì thu nhập thực tế có thể không tuân theo mô hình hoàn hảo này nên anh ta muốn trích xuất một chuỗi các năm có thu nhập khớp chính xác với 1, 2, 3, … theo thứ tự. 

Mục tiêu của chúng ta là chọn dãy con dài nhất như vậy. Sau đó, chúng tôi xuất ra độ dài của nó và số năm tương ứng (chỉ số 2000 + năm). 

Ràng buộc đầu vào nhỏ:`n`tối đa là 100. Điều này có nghĩa là thuật toán có các phép toán O(n^2) là khả thi vì ngay cả 10.000 phép toán cũng không đáng kể đối với các bộ xử lý hiện đại dưới giới hạn 2 giây. Thu nhập có thể âm nên cách tiếp cận ngây thơ dừng lại ở điểm không phù hợp đầu tiên sẽ thất bại. Chúng ta cũng cần xử lý khả năng không có năm nào phù hợp với mức tăng trưởng hoàn hảo, trong trường hợp đó câu trả lời là 0. 

Các trường hợp đặc biệt không rõ ràng bao gồm các chuỗi trong đó tồn tại nhiều năm phù hợp cho cùng một giá trị mục tiêu. Ví dụ, trong`[1, 1, 2, 2, 3]`, chuỗi hoàn hảo có thể lấy số 1 đầu tiên hoặc số 1 thứ hai. Một trường hợp cạnh khác là khi tất cả các số đều âm, ví dụ:`[-1, -2, -3]`, tạo ra kết quả bằng 0. Một cách tiếp cận bất cẩn có thể trả về các chỉ số mà không kiểm tra giá trị thực tế so với mức tăng trưởng hoàn hảo dự kiến. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử từng dãy con của mảng thu nhập, kiểm tra xem các phần tử của nó có tạo thành mức tăng trưởng hoàn hảo hay không. Vì số dãy con có độ dài k là tổ hợp nên tổng số thao tác tăng theo cấp số nhân, khoảng O(2^n). Vì`n = 100`, điều này hoàn toàn không thể thực hiện được. 

Điểm mấu chốt là bài toán này tương đương với việc tìm dãy con dài nhất khớp với một dãy tăng dần cho trước`1, 2, 3, …`. Đây chính xác là một biến thể của bài toán Chuỗi tăng dần dài nhất (LIS), nhưng thay vì so sánh tùy ý, mỗi phần tử phải khớp với một giá trị kỳ vọng cụ thể. Bởi vì`n`nhỏ, phương pháp lập trình động hoạt động hiệu quả. 

Chúng tôi lặp qua mảng trong khi vẫn duy trì mảng DP trong đó`dp[i]`là độ dài của chuỗi hoàn hảo dài nhất kết thúc ở vị trí`i`. Đối với mỗi vị trí, chúng tôi kiểm tra tất cả các vị trí trước đó để xem liệu chúng tôi có thể mở rộng chuỗi hay không. Điều này làm giảm vấn đề từ cấp số nhân xuống O(n^2), điều này có thể chấp nhận được với các ràng buộc. Chúng tôi cũng lưu trữ một`prev`mảng để xây dựng lại chuỗi năm thực tế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Lập trình động | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`dp`chiều dài`n`với số không.`dp[i]`sẽ lưu trữ độ dài của chuỗi con hoàn hảo dài nhất kết thúc tại chỉ mục`i`. 
2. Khởi tạo một mảng`prev`chiều dài`n`với`-1`để lưu trữ chỉ mục trước đó trong chuỗi để xây dựng lại. 
3. Lặp lại mảng bằng chỉ mục`i`từ 0 đến n-1. Đối với mỗi phần tử`a[i]`, hãy tính giá trị mục tiêu của nó theo trình tự hoàn hảo như`dp[i] + 1`. Nếu như`a[i]`bằng`dp[i] + 1`và nó có thể mở rộng bất kỳ chuỗi con nào trước đó, cập nhật`dp[i]`ĐẾN`dp[j] + 1`đối với một số người`j < i`Ở đâu`dp[j]`được tối đa hóa và ghi lại`prev[i] = j`. Nếu không có phần tử trước đó có thể mở rộng nó, hãy đặt`dp[i] = 1`. 
4. Sau khi đổ đầy`dp`, tìm chỉ số`max_index`với lớn nhất`dp[max_index]`. Điều này cho biết độ dài của chuỗi hoàn hảo dài nhất. 
5. Xây dựng lại trình tự bằng cách làm theo`prev`mảng từ`max_index`ngược lại. Chuyển đổi từng chỉ số mảng thành năm thực tế bằng cách thêm 2001. 
6. In độ dài của dãy và số năm được tái tạo. Nếu không có phần tử nào khớp với mức tăng trưởng hoàn hảo, ghi 0. 

Tại sao nó hoạt động: Ở mỗi bước,`dp[i]`thể hiện chính xác chuỗi hoàn hảo dài nhất kết thúc tại`i`bởi vì nó chỉ xem xét các phần mở rộng hợp lệ. các`prev`mảng đảm bảo chúng ta có thể xây dựng lại dãy con theo thứ tự. Thuật toán xem xét một cách có hệ thống tất cả các khả năng kết thúc một chuỗi hoàn hảo ở mỗi vị trí, do đó nó không thể bỏ lỡ chuỗi dài nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

dp = [0] * n
prev = [-1] * n

for i in range(n):
    if a[i] == 1:
        dp[i] = 1
    for j in range(i):
        if dp[j] > 0 and a[i] == dp[j] + 1:
            if dp[j] + 1 > dp[i]:
                dp[i] = dp[j] + 1
                prev[i] = j

# Find the index of the maximum length
max_len = 0
max_index = -1
for i in range(n):
    if dp[i] > max_len:
        max_len = dp[i]
        max_index = i

if max_len == 0:
    print(0)
else:
    sequence = []
    idx = max_index
    while idx != -1:
        sequence.append(2001 + idx)
        idx = prev[idx]
    sequence.reverse()
    print(max_len)
    print(*sequence)
```Đầu tiên chúng ta khởi tạo`dp`Và`prev`để theo dõi các chuỗi dài nhất và đường dẫn tái thiết của chúng. Chúng tôi lặp lại từng cặp`(j, i)`với`j < i`để kiểm tra xem chúng ta có thể mở rộng một chuỗi hay không. Đây là lúc việc xử lý cẩn thận mức tăng trưởng hoàn hảo dự kiến ​​là rất quan trọng; không có`a[i] == dp[j] + 1`, dãy sau sẽ không hợp lệ. Việc tái thiết cuối cùng sử dụng`prev`để xây dựng trình tự ngược lại và sau đó đảo ngược nó theo thứ tự thời gian. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
10
-2 1 1 3 2 3 4 -10 -2 5
```| tôi | một [tôi] | dp[i] | trước[i] | 
| --- | --- | --- | --- | 
| 0 | -2 | 0 | -1 | 
| 1 | 1 | 1 | -1 | 
| 2 | 1 | 1 | -1 | 
| 3 | 3 | 0 | -1 | 
| 4 | 2 | 2 | 1 | 
| 5 | 3 | 3 | 4 | 
| 6 | 4 | 4 | 5 | 
| 7 | -10 | 0 | -1 | 
| 8 | -2 | 0 | -1 | 
| 9 | 5 | 5 | 6 | 

Trình tự được xây dựng lại: 2002, 2005, 2006, 2007, 2010 

Điều này chứng tỏ rằng thuật toán theo dõi chính xác các chuỗi con và bỏ qua những năm không liên quan. 

**Đầu vào tùy chỉnh:**```
5
-1 -2 -3 -4 -5
```Tất cả`dp[i]`vẫn là 0, do đó đầu ra là 0. Điều này xác nhận trường hợp cạnh không có năm hợp lệ được xử lý. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Hai vòng lặp lồng nhau trên n phần tử, khả thi với n ≤ 100 | 
| Không gian | O(n) | Hai mảng`dp`Và`prev`có kích thước n, cộng với chuỗi được xây dựng lại | 

Thuật toán rất phù hợp với các ràng buộc: 10.000 thao tác trong cửa sổ 2 giây là không đáng kể và mức sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    dp = [0] * n
    prev = [-1] * n
    for i in range(n):
        if a[i] == 1:
            dp[i] = 1
        for j in range(i):
            if dp[j] > 0 and a[i] == dp[j] + 1:
                if dp[j] + 1 > dp[i]:
                    dp[i] = dp[j] + 1
                    prev[i] = j
    max_len = 0
    max_index = -1
    for i in range(n):
        if dp[i] > max_len:
            max_len = dp[i]
            max_index = i
    if max_len == 0:
        return "0"
    sequence = []
    idx = max_index
    while idx != -1:
        sequence.append(2001 + idx)
        idx = prev
```
