---
title: "CF 104887H - Harana"
description: "Mỗi vị trí trong đầu vào đại diện cho một nốt trong bài hát. Tại vị trí i, có một nốt chủ ý si và Bob thực sự hát bi. Nếu không có gì khác thay đổi thì sự không khớp ở vị trí i đơn giản là sự khác biệt tuyệt đối giữa hai giá trị này."
date: "2026-06-28T09:02:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104887
codeforces_index: "H"
codeforces_contest_name: "2023 Abakoda Long Contest"
rating: 0
weight: 104887
solve_time_s: 73
verified: true
draft: false
---

[CF 104887H - Harana](https://codeforces.com/problemset/problem/104887/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi vị trí trong đầu vào đại diện cho một nốt trong bài hát. Tại vị trí`i`, có một ghi chú dự định`s_i`và Bob thực sự hát`b_i`. Nếu không có gì thay đổi thì vị trí không khớp`i`chỉ đơn giản là sự khác biệt tuyệt đối giữa hai giá trị này. 

Bob được phép dịch chuyển phím nhạc, nghĩa là thêm một hằng số nguyên vào tất cả các nốt trong một đoạn liền kề. Việc chuyển phím không ảnh hưởng đến giọng hát của Bob; nó tự thay đổi bài hát. Sau khi dịch chuyển, mỗi vị trí so sánh giá trị cố định của Bob`b_i`chống lại giá trị bài hát đã sửa đổi. 

Đối với điểm phân chia đã chọn`m`, bài hát được chia thành hai đoạn. Phân đoạn đầu tiên sử dụng một phép dịch số nguyên`k_1`và phân đoạn thứ hai sử dụng một ca khác`k_2`. Mục tiêu là chọn cả hai ca sao cho tổng số không khớp tuyệt đối được giảm thiểu. 

Một quan sát quan trọng là việc dịch chuyển một đoạn bằng`k`biến mỗi thuật ngữ thành một biểu thức có dạng`|b_i - (s_i + k)|`, vậy vấn đề thực sự là ở việc chọn một số nguyên`k`sắp xếp tốt nhất một tập hợp số. 

Kích thước đầu vào tăng lên`2 × 10^5`, điều này ngay lập tức loại trừ mọi cách tiếp cận thử tất cả các ca có thể có hoặc tính toán lại chi phí từ đầu cho mỗi lần phân chia. Thậm chí một`O(n^2)`giải pháp tính toán lại từng phân đoạn một cách độc lập sẽ yêu cầu theo thứ tự`4 × 10^10`trong trường hợp xấu nhất vượt xa giới hạn. 

Một trường hợp phức tạp xuất hiện khi tất cả sự khác biệt bị loại bỏ ở một phân đoạn nhưng không loại bỏ ở phân đoạn kia. Ví dụ, nếu`b_i - s_i`không đổi trong một phân khúc, sự dịch chuyển tối ưu làm cho chi phí bằng 0 cho phân khúc đó và bất kỳ phương pháp sai nào giả định tính độc lập trên mỗi chỉ mục đều có thể bỏ lỡ điều này hoàn toàn. 

## Phương pháp tiếp cận 

Bắt đầu bằng cách viết lại biểu thức bên trong giá trị tuyệt đối. Đối với một phân đoạn cố định và thay đổi`k`, mỗi thuật ngữ trở thành`|b_i - (s_i + k)| = |(b_i - s_i) - k|`. 

Định nghĩa`x_i = b_i - s_i`. Khi đó chi phí của mỗi phân khúc sẽ giảm thiểu`sum |x_i - k|`trên số nguyên`k`. 

Đây là một cấu trúc cổ điển: chúng ta đang chọn một số nguyên duy nhất`k`giúp giảm thiểu độ lệch tuyệt đối so với nhiều giá trị. Sự lựa chọn tối ưu của`k`là điểm trung bình của đoạn đó và chi phí tối thiểu là tổng khoảng cách đến điểm trung vị đó. 

Vì vậy mỗi lần chia`m`yêu cầu tính toán hai bài toán 1D độc lập: tiền tố`[1..m]`và hậu tố`[m+1..n]`. Tổng câu trả lời là tổng chi phí sai lệch tuyệt đối tối ưu của chúng. 

Giải pháp vũ lực sẽ, đối với mỗi`m`, tính lại giá trị tối ưu`k_1`Và`k_2`bằng cách sắp xếp cả hai nửa và đánh giá chi phí trung bình. Điều đó mang lại`O(n)`làm việc mỗi lần phân chia, dẫn đến`O(n^2 log n)`nói chung là quá chậm. 

Việc tối ưu hóa xuất phát từ việc nhận thấy rằng cả tiền tố và hậu tố đều có thể được xử lý tăng dần. Khi chúng ta mở rộng một đoạn bằng một phần tử, chúng ta có thể duy trì trung vị của nó và tổng khoảng cách đến nó bằng cách sử dụng hai vùng dữ liệu và tổng chạy. Điều này làm giảm việc tính toán chi phí từng phân khúc thành khấu hao`O(log n)`mỗi lần chèn. 

Các giá trị hậu tố được xử lý bằng cách xử lý mảng ngược lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tính toán lại mỗi lần chia) | O(n^2 log n) | O(n) | Quá chậm | 
| Tối ưu (hai đống + tiền tố/hậu tố DP) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

hãy để`x_i = b_i - s_i`. Chúng tôi muốn chi phí tiền tố và hậu tố có độ lệch tuyệt đối tối ưu. 

### 1. Chuyển đổi bài toán 

Thay thế từng cặp`(s_i, b_i)`với`x_i = b_i - s_i`. Từ bây giờ chi phí của mỗi phân khúc là`min_k sum |x_i - k|`. 

Giá trị dịch chuyển`k`biến mất khỏi cấu trúc ngoại trừ dưới dạng bộ chọn trung vị. 

### 2. Xây dựng chi phí tiền tố trực tuyến 

Chúng tôi duy trì nhiều tập hợp giá trị động với khả năng truy vấn: 

tổng khoảng cách tuyệt đối đến số nguyên tốt nhất`k`. 

Chúng tôi lưu trữ: 

- một đống tối đa cho nửa dưới 
- một đống tối thiểu cho nửa trên 
- tổng các phần tử trong mỗi đống 

Trung vị là đỉnh của vùng tối đa. 

Khi chúng tôi chèn từng`x_i`, chúng tôi cân bằng lại các đống sao cho kích thước khác nhau nhiều nhất là một, với vùng heap thấp hơn giữ phần tử phụ khi lẻ. 

Chi phí tiền tố tại mỗi vị trí được tính từ mức trung bình hiện tại. 

### 3. Tính chi phí từ trung vị 

Nếu trung vị là`m`, thì: 

Bên trái đóng góp`m * len(left) - sum(left)`. 

Bên phải đóng góp`sum(right) - m * len(right)`. 

Tổng hợp cả hai cho tổng độ lệch tuyệt đối. 

### 4. Xây dựng chi phí hậu tố 

Lặp lại quy trình tương tự trên mảng đảo ngược để có được chi phí hậu tố cho mọi vị trí bắt đầu. 

### 5. Kết hợp đáp án 

Đối với mỗi lần chia`m`, đầu ra:`prefix_cost[m] + suffix_cost[m+1]`. 

### Tại sao nó hoạt động 

Đối với bất kỳ đoạn cố định nào, hàm`sum |x_i - k|`lồi ở`k`trên số nguyên. Mức tối thiểu của nó đạt được ở mức trung bình và bất kỳ sai lệch nào so với mức trung bình sẽ làm tăng chi phí tuyến tính tùy theo số điểm nằm ở mỗi bên. Việc duy trì mức phân chia trung bình đảm bảo rằng ở mỗi bước, chúng tôi duy trì cấu trúc tối ưu của phân khúc, do đó, chi phí được tính toán luôn ở mức tối thiểu cho tiền tố hoặc hậu tố đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import heapq

INF = 10**30

def compute_costs(arr):
    low = []   # max heap via negatives
    high = []  # min heap
    sum_low = 0
    sum_high = 0

    def rebalance():
        nonlocal sum_low, sum_high
        if len(low) > len(high) + 1:
            x = -heapq.heappop(low)
            sum_low -= x
            heapq.heappush(high, x)
            sum_high += x
        elif len(high) > len(low):
            x = heapq.heappop(high)
            sum_high -= x
            heapq.heappush(low, -x)
            sum_low += x

    def add(x):
        nonlocal sum_low, sum_h
```
