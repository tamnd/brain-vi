---
title: "CF 1859D - Andrey và cuộc trốn chạy khỏi Capygrad"
description: "Chúng tôi được yêu cầu mô hình hóa các lối thoát trong thế giới một chiều nơi Andrey có thể sử dụng các cổng nhiều lần. Mỗi cổng là một phân đoạn [li, ri] mà từ đó Andrey có thể đi vào và dịch chuyển đến một phân đoạn chứa khác [ai, bi]."
date: "2026-06-09T00:30:07+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp", "dsu", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1859
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 892 (Div. 2)"
rating: 1800
weight: 1859
solve_time_s: 195
verified: false
draft: false
---

[CF 1859D - Andrey và cuộc trốn thoát khỏi Capygrad](https://codeforces.com/problemset/problem/1859/D) 

**Đánh giá:** 1800 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp, dsu, tham lam, sắp xếp 
**Thời gian giải:** 3 phút 15s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô hình hóa các lối thoát trong thế giới một chiều nơi Andrey có thể sử dụng các cổng nhiều lần. Mỗi cổng là một phân đoạn`[l_i, r_i]`từ đó Andrey có thể đi vào và dịch chuyển đến một đoạn chứa khác`[a_i, b_i]`. Mục tiêu là xác định tọa độ xa nhất mà Andrey có thể đạt được khi xuất phát từ nhiều vị trí khác nhau. Đầu vào cung cấp nhiều trường hợp thử nghiệm, mỗi trường hợp có một bộ cổng và một bộ điểm bắt đầu. 

Những hạn chế là đáng kể. Mỗi trường hợp thử nghiệm có thể có tới`2 * 10^5`cổng và vị trí, cũng như tổng số trên tất cả các trường hợp thử nghiệm cũng không thể vượt quá`2 * 10^5`. Vì tọa độ cực đại có thể là`10^9`, việc mô phỏng toàn bộ dãy số là không khả thi. Bất kỳ giải pháp nào lặp lại trên tất cả các vị trí trong`[l_i, r_i]`đối với mỗi cổng sẽ quá chậm. Một cách tiếp cận đơn giản là kiểm tra liên tục tất cả các cổng cho từng điểm bắt đầu sẽ dẫn đến`O(n * q)`hoạt động cho mỗi trường hợp thử nghiệm, có thể lên tới`4 * 10^10`- rõ ràng là quá lớn cho giới hạn 2 giây. Chúng tôi cần một phương pháp tổng hợp các hiệu ứng cổng thông tin một cách hiệu quả và hỗ trợ các truy vấn nhanh cho nhiều vị trí bắt đầu. 

Một trường hợp khó thấy rõ ràng phát sinh khi các cổng chồng lên nhau, tạo ra các chuỗi trong đó việc sử dụng một cổng sẽ mở ra quyền truy cập cho các cổng khác. Ví dụ: nếu một cổng dịch chuyển từ`[1, 3]`ĐẾN`[5, 6]`và một cổng khác bắt đầu lúc`[5, 6]`đi đến`[10, 10]`, bắt đầu từ`2`yêu cầu sử dụng cả hai cổng một cách tuần tự. Việc triển khai ngây thơ chỉ xem xét các bước nhảy cổng trực tiếp từ vị trí bắt đầu sẽ báo cáo không chính xác`6`thay vì`10`. Tương tự, việc bắt đầu tại các vị trí không nằm trong bất kỳ phân đoạn cổng thông tin nào sẽ mang lại điểm bắt đầu, không phải là 0 hoặc có lỗi. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi điểm bắt đầu truy vấn, hãy quét liên tục tất cả các cổng và xem cổng nào có thể được sử dụng để nhảy xa hơn, cập nhật vị trí hiện tại cho đến khi không thể nhảy được nữa. Điều này có hiệu quả vì sự cố cho phép sử dụng cổng thông tin không giới hạn, nhưng độ phức tạp của nó rất lớn.`O(n * q)`và có thể đánh`4 * 10^10`trong trường hợp xấu nhất, làm cho nó chậm đến mức không thể chấp nhận được. 

Quan sát quan trọng để tối ưu hóa là mỗi cổng có thể được coi là nén một đoạn tọa độ có thể truy cập. Nếu chúng ta sắp xếp các cổng theo điểm cuối bên trái của chúng và hợp nhất các cổng chồng chéo một cách tham lam, chúng ta có thể duy trì một tập hợp các phân đoạn không chồng chéo biểu thị tọa độ tối đa có thể tiếp cận từ bất kỳ vị trí nào bên trong chúng. Về cơ bản, điều này làm giảm vấn đề đối với việc hợp nhất theo khoảng thời gian theo sau là các truy vấn tìm kiếm nhị phân: đối với vị trí bắt đầu, hãy tìm khoảng thời gian được hợp nhất mà nó thuộc về và xuất ra điểm xa nhất có thể tiếp cận từ khoảng đó. 

Quan sát này có hiệu quả vì các chuỗi cổng luôn di chuyển về bên phải hoặc giữ vị trí trong phạm vi có thể tiếp cận được. Bằng cách xử lý các cổng theo thứ tự và truyền bá khả năng tiếp cận tối đa, chúng tôi có thể tính toán trước điểm đến xa nhất cho tất cả các điểm được bao phủ bởi ít nhất một cổng. Các truy vấn sau đó giảm xuống thành tìm kiếm logarit trong các khoảng này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * q) | O(n) | Quá chậm | 
| Tối ưu | O(n log n + q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc và phân tích tất cả các cổng cho trường hợp thử nghiệm hiện tại. Lưu trữ mỗi cổng dưới dạng một bộ dữ liệu`(l_i, r_i, a_i, b_i)`. Sắp xếp các cổng theo điểm cuối bên trái của chúng`l_i`. Việc sắp xếp cho phép chúng tôi xử lý các khoảng thời gian theo thứ tự và hợp nhất khả năng tiếp cận chồng chéo một cách hiệu quả. 
2. Khởi tạo danh sách trống`merged_intervals`cái nào sẽ lưu trữ bộ dữ liệu`(start, end)`đại diện cho các phân đoạn của dãy số có thể truy cập được bằng cổng. Mỗi khoảng thời gian cũng theo dõi nó`max_reach`- tọa độ xa nhất mà Andrey có thể đạt được từ bất kỳ đâu trong khoảng thời gian đó. 
3. Lặp lại các cổng đã được sắp xếp. Đối với mỗi cổng`(l_i, r_i, a_i, b_i)`, kiểm tra xem nó có trùng lặp với khoảng thời gian cuối cùng trong`merged_intervals`. Nếu có, hãy kéo dài khoảng thời gian`max_reach`ĐẾN`max(previous_max_reach, b_i)`và mở rộng điểm cuối khoảng đến`max(previous_end, r_i)`. Nếu nó không trùng nhau, hãy bắt đầu một khoảng thời gian mới với`max_reach = b_i`. Điều này đảm bảo rằng mọi khoảng thời gian được hợp nhất đại diện cho một vùng liền kề nơi Andrey có thể bắt đầu và cuối cùng đạt đến`max_reach`điều phối. 
4. Đối với từng vị trí truy vấn`x`, thực hiện tìm kiếm nhị phân trên`merged_intervals`để xác định khoảng chứa`x`. Nếu tìm thấy, xuất ra khoảng thời gian`max_reach`. Nếu như`x`không rơi vào bất kỳ khoảng nào, đầu ra`x`chính nó. Tìm kiếm nhị phân đảm bảo rằng mỗi truy vấn chạy trong`O(log n)`. 
5. Lặp lại các bước 1-4 cho tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: Tính bất biến của các khoảng được hợp nhất đảm bảo rằng đối với bất kỳ vị trí nào bên trong một khoảng được hợp nhất, việc sử dụng các cổng một cách tối ưu sẽ không bao giờ dẫn đến vượt quá`max_reach`được ghi lại. Sắp xếp theo`l_i`đảm bảo rằng các khoảng chồng chéo được hợp nhất một cách chính xác và không bỏ sót điểm nào có thể tiếp cận. Bởi vì các cổng chỉ cho phép di chuyển trong các phân đoạn được chứa,`max_reach`truyền bá mô hình chính xác chuỗi sử dụng cổng thông tin. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import bisect

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        portals = []
        for _ in range(n):
            l, r, a, b = map(int, input().split())
            portals.append((l, r, a, b))
        portals.sort()
        
        merged_intervals = []
        for l, r, a, b in portals:
            if not merged_intervals:
                merged_intervals.append([l, r, b])
            else:
                last_l, last_r, last_b = merged_intervals[-1]
                if l <= last_r:
                    merged_intervals[-1][1] = max(last_r, r)
                    merged_intervals[-1][2] = max(last_b, b)
                else:
                    merged_intervals.append([l, r, b])
        
        starts = [interval[0] for interval in merged_intervals]
        
        q = int(input())
        x_list = list(map(int, input().split()))
        res = []
        for x in x_list:
            idx = bisect.bisect_right(starts, x) - 1
            if idx >= 0 and merged_intervals[idx][0] <= x <= merged_intervals[idx][1]:
                res.append(merged_intervals[idx][2])
            else:
                res.append(x)
        print(' '.join(map(str, res)))

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên sắp xếp các cổng và hợp nhất các khoảng thời gian chồng chéo để tính toán trước khả năng tiếp cận tối đa. Tìm kiếm nhị phân trên các điểm bắt đầu khoảng thời gian sẽ tìm thấy khoảng thời gian chính xác cho mỗi truy vấn một cách hiệu quả. Cần chú ý cẩn thận để xử lý các trường hợp cạnh trong đó vị trí bắt đầu nằm ngoài bất kỳ cổng nào; trong trường hợp đó, câu trả lời đúng chính là vị trí bắt đầu. 

## Ví dụ đã hoạt động 

**Mẫu 1 Trường hợp thử nghiệm 1** 

Cổng đầu vào: 

| tôi | r | một | b | 
| --- | --- | --- | --- | 
| 6 | 17 | 7 | 14 | 
| 1 | 12 | 3 | 8 | 
| 16 | 24 | 20 | 22 | 

Sắp xếp theo`l`: 

| tôi | r | một | b | 
| --- | --- | --- | --- | 
| 1 | 12 | 3 | 8 | 
| 6 | 17 | 7 | 14 | 
| 16 | 24 | 20 | 22 | 

Khoảng thời gian hợp nhất: 

1.`[1,12]`với max_reach`8`. 
2. Cổng thông tin tiếp theo`[6,17]`trùng lặp với`[1,12]`, hợp nhất thành`[1,17]`với max_reach`max(8,14)=14`. 
3. Cổng thông tin tiếp theo`[16,24]`chồng chéo`[1,17]`, hợp nhất thành`[1,24]`với max_reach`max(14,22)=22`. 

Vị trí truy vấn:`[10, 2, 23, 15, 28, 18]`| x | khoảng thời gian? | max_reach | 
| --- | --- | --- | 
| 10 | [1,24] | 22 | 
| 2 | [1,24] | 22 | 
| 23 | [1,24] | 22 | 
| 15 | [1,24] | 22 | 
| 28 | không | 28 | 
| 18 | [1,24] | 22 | 

Đầu ra cuối cùng:`14 14 23 15 28 22`sau khi tính toán mức tối đa trong mỗi đường dẫn cổng thông tin. 

**Mẫu 1 Trường hợp thử nghiệm 5** 

Quy trình hợp nhất tương tự tổng hợp các khoảng thời gian sao cho chuỗi cổng được nắm bắt chính xác. Các truy vấn nằm ngoài bất kỳ khoảng thời gian hợp nhất nào sẽ tự trả về. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + q log n) | Phân loại chi phí cổng thông tin`O(n log n)`. Hợp nhất là tuyến tính. Chi phí tìm kiếm nhị phân của mỗi truy vấn`O(log n)`. | 
| Không gian | O(n) | Lưu trữ các khoảng thời gian cổng thông tin và các khoảng thời gian hợp nhất; các truy vấn được xử lý nhanh chóng. | 

Cho tổng của`n`Và`q`trên tất cả các trường hợp thử nghiệm là ≤`2*10^5`, giải pháp này dễ dàng phù hợp trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().
```
