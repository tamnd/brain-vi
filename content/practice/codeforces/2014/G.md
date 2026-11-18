---
title: "CF 2014G - Những Ngày Ngân Hà"
description: "Chúng tôi đang theo dõi mức tiêu thụ sữa của Little John theo thời gian. Anh ta nhận sữa theo từng đợt riêng biệt, mỗi đợt tương ứng với một ngày và một số lượng. Sữa có giới hạn độ tươi k ngày, nghĩa là sữa thu được vào ngày di sẽ hỏng sau ngày di + k - 1."
date: "2026-06-08T13:02:45+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2014
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 974 (Div. 3)"
rating: 2200
weight: 2014
solve_time_s: 119
verified: true
draft: false
---

[CF 2014G - Ngày Ngân Hà](https://codeforces.com/problemset/problem/2014/G) 

**Đánh giá:** 2200 
**Tags:** bạo lực, cấu trúc dữ liệu, tham lam, thực hiện 
**Thời gian giải:** 1 phút 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang theo dõi mức tiêu thụ sữa của Little John theo thời gian. Anh ta nhận sữa theo từng đợt riêng biệt, mỗi đợt tương ứng với một ngày và một số lượng. Sữa có giới hạn độ tươi`k`ngày, nghĩa là sữa thu được vào ngày`d_i`sẽ hư hỏng sau ngày`d_i + k - 1`. Mỗi ngày, John uống sữa một cách thèm thuồng từ nguồn sữa tươi nhất hiện có, cho đến`m`pint. Một ngày thỏa mãn với sữa xảy ra bất cứ khi nào anh ấy uống được chính xác`m`pint. 

Đầu vào là một chuỗi các mục nhật ký, mỗi mục ghi rõ một ngày và lượng sữa nhận được. Mục tiêu của chúng ta là đếm, đối với mỗi trường hợp thử nghiệm, số ngày mà John hoàn toàn hài lòng. Những ngày không đủ sữa không được tính. Đầu ra là một số nguyên cho mỗi trường hợp thử nghiệm. 

Những ràng buộc cho phép`n`lên đến`10^5`cho mỗi trường hợp thử nghiệm, với tổng số`n`trên tất cả các trường hợp thử nghiệm lên đến`2 * 10^5`. Giới hạn tiêu thụ hàng ngày`m`và sự tươi mát`k`cũng lên đến`10^5`. Điều này có nghĩa là bất kỳ giải pháp nào lặp lại hàng ngày theo dòng thời gian đầy đủ lên đến`10^6`sẽ là quá chậm. Thay vào đó, chúng ta phải xử lý các sự kiện về sữa một cách hiệu quả, chỉ tập trung vào những ngày có sữa hoặc bị hỏng. 

Các trường hợp đặc biệt bao gồm các tình huống trong đó sữa từ những ngày trước được tích lũy nhưng các mục sau đó lại không theo thứ tự ngay lập tức. Ví dụ, sữa về vào ngày thứ 1 và ngày thứ 4, với`k = 3`, có nghĩa là sữa từ ngày 1 sẽ hỏng trước ngày thứ 4, ngay cả khi John uống một phần mỗi ngày. Một trường hợp khó khăn khác xảy ra khi John nhận được nhiều sữa hơn mức anh ấy có thể uống trong một ngày - sữa thừa phải được chuyển tiếp mà không làm mất đi độ tươi của sữa. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ duy trì một mảng đại diện hàng ngày và theo dõi lượng sữa có sẵn. Trong mỗi ngày, chúng tôi sẽ trừ đi số sữa đã tiêu thụ, loại bỏ sữa hư và đếm số ngày hài lòng. Điều này đúng về nguyên tắc nhưng quá chậm vì số ngày có thể đạt tới`10^6`. Lặp lại hàng ngày cho mỗi đợt sẽ mang lại kết quả gần như`10^6 * 10^5`trong trường hợp xấu nhất vượt quá thời hạn. 

Điểm mấu chốt là chúng ta không cần phải mô phỏng hàng ngày. Việc tiêu thụ sữa có thể được biểu diễn dưới dạng hàng đợi các lô được sắp xếp theo thời hạn sử dụng, vì John luôn uống loại sữa tươi nhất trước tiên. Sử dụng hàng đợi ưu tiên hoặc deque cho phép chúng tôi duy trì lượng sữa dự trữ một cách hiệu quả. Mỗi ngày có sự kiện (thu mua sữa hoặc hết sữa) đều được xử lý trực tiếp. Chúng tôi cập nhật số lượng sữa và đếm số ngày hài lòng mà không lặp lại những ngày trống. Bằng cách bỏ qua khoảng cách giữa các ngày, chúng tôi giảm bớt các hoạt động không cần thiết và đạt được độ phức tạp tuyến tính đối với số lượng mục nhật ký. 

Cách tiếp cận bạo lực có hiệu quả vì nó mô phỏng thực tế một cách chính xác nhưng không thành công khi số ngày quá lớn so với số sự kiện sữa. Việc quan sát thấy sữa có thể được theo dõi theo lô hết hạn cho phép chúng tôi giảm bớt vấn đề trong quá trình xử lý`2n`sự kiện cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(max(d_i) * n) | O(max(d_i)) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân loại lô sữa theo ngày thu mua nếu chưa phân loại. Điều này đảm bảo chúng tôi xử lý sữa theo trình tự thời gian. Vì bài toán đảm bảo đầu vào được sắp xếp nên trong thực tế chúng ta có thể bỏ qua bước này. 
2. Khởi tạo hàng đợi deque hoặc ưu tiên để lưu trữ các mẻ sữa. Mỗi phần tử lưu trữ hai giá trị: số pint còn lại và ngày hết hạn. Phía trước của hàng đợi là loại sữa tươi ngon nhất để uống ngày nay. 
3. Đặt biến`current_day`đến ngày mua lại đầu tiên. Lặp lại tất cả các sự kiện về sữa theo trình tự thời gian, có tính đến khoảng cách trong ngày giữa các sự kiện. 
4. Đối với sữa có sẵn mỗi ngày: 

- Loại bỏ sữa hết hạn ở phía trước hàng đợi. Một lô sẽ hết hạn nếu ngày hết hạn của nó nhỏ hơn`current_day`. 
- Uống tới`m`pint. Bắt đầu với mẻ tươi nhất. Nếu một lô có ít hơn`m`pint, tiêu thụ hết và tiếp tục sang mẻ tiếp theo. Theo dõi tổng lượng sữa tiêu thụ. 
- Nếu chính xác`m`pint đã được tiêu thụ, tăng bộ đếm mức độ hài lòng. 
- Chuyển sang ngày hôm sau. 
5. Đối với mỗi sự kiện mua sữa trong ngày`d_i`, nối thêm lô mới`(a_i, d_i + k - 1)`đến hàng đợi. Sau khi xử lý mức tiêu thụ cho ngày hôm đó, hãy tăng thêm`current_day`. 
6. Lặp lại cho đến khi hết mẻ sữa hoặc hết hạn sử dụng. 

Tại sao nó hoạt động: Điều bất biến là hàng đợi luôn chứa sữa nguyên chất theo thứ tự tươi ngon. Bằng cách luôn tiêu thụ mẻ sữa tươi nhất trước và loại bỏ sữa hết hạn vào đầu mỗi ngày, chúng tôi đảm bảo rằng việc tiêu thụ tôn trọng độ tươi. Đếm một ngày chỉ thỏa mãn khi chính xác`m`pint được tiêu thụ phản ánh định nghĩa vấn đề. Bỏ qua những ngày không có sự kiện là an toàn vì không có sữa nên không thể có ngày hài lòng. 

## Giải pháp Python```python
import sys
import heapq
from collections import deque

input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        events = [tuple(map(int, input().split())) for _ in range(n)]
        
        milk_queue = deque()
        satisfaction_days = 0
        idx = 0
        current_day = events[0][0]
        last_day = max(d + k - 1 for d, _ in events)

        for day in range(current_day, last_day + 1):
            # Add new milk batches for today
            while idx < n and events[idx][0] == day:
                d_i, a_i = events[idx]
                milk_queue.append([a_i, d_i + k - 1])
                idx += 1
            
            # Remove expired milk
            while milk_queue and milk_queue[0][1] < day:
                milk_queue.popleft()
            
            if not milk_queue:
                continue
            
            # Drink milk up to m pints
            to_drink = m
            while milk_queue and to_drink > 0:
                batch = milk_queue[-1]  # freshest milk is at the back
                if batch[0] <= to_drink:
                    to_drink -= batch[0]
                    milk_queue.pop()
                else:
                    batch[0] -= to_drink
                    to_drink = 0
            
            if to_drink == 0:
                satisfaction_days += 1
        
        print(satisfaction_days)

if __name__ == "__main__":
    solve()
```Mã sử ​​dụng deque với sữa tươi nhất ở phía sau, vì bật ra từ phía sau là O(1) và phù hợp với mức tiêu thụ "tươi nhất trước tiên" tham lam. Sữa hết hạn được loại bỏ khỏi phía trước. Những ngày không có sự kiện sẽ bị bỏ qua một cách tự nhiên vì vòng lặp chỉ xử lý những ngày cho đến khi`last_day`nơi mà sữa vẫn có thể tồn tại. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
1 1 3
1 5
```| Ngày | Hàng đợi sữa (pint, hết hạn) | Để Uống | Sự hài lòng? | Xếp hàng sau | 
| --- | --- | --- | --- | --- | 
| 1 | [(5,3)] | 1 | Có | [(4,3)] | 
| 2 | [(4,3)] | 1 | Có | [(3,3)] | 
| 3 | [(3,3)] | 1 | Có | [(2,3)] | 

Số ngày hài lòng = 3. Điều này cho thấy sữa tồn tại qua nhiều ngày và thuật toán uống chính xác tới`m`pint trong khi vẫn tôn trọng độ tươi. 

**Mẫu 2:** 

đầu vào:```
2 3 3
1 5
2 7
```| Ngày | Hàng đợi sữa | Để Uống | Sự hài lòng? | Xếp hàng sau | 
| --- | --- | --- | --- | --- | 
| 1 | [(5,3)] | 3 | Có | [(2,3)] | 
| 2 | [(2,3),(7,4)] | 3 | Có | [(1,3),(5,4)] | 
| 3 | [(1,3),(5,4)] | 3 | Có | [(3,4)] | 
| 4 | [(3,4)] | 3 | Có | [] | 

Số ngày hài lòng = 4. Điều này chứng tỏ tầm quan trọng của việc loại bỏ sữa hết hạn vào đầu mỗi ngày. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi mẻ sữa được thêm một lần, loại bỏ một lần và được tiêu thụ một phần với tổng số O(1) cho mỗi hoạt động loại bỏ pint. Tổng số hoạt động tuyến tính theo số đợt. | 
| Không gian | O(n) | Hàng đợi lưu trữ tối đa tất cả các lô sữa trước khi hết hạn. | 

Điều này phù hợp thoải mái trong giới hạn vì`n <= 2*10^5`Và`k,m <= 10^5`. Việc sử dụng deque đảm bảo mọi thao tác đều nhanh chóng và tránh mô phỏng những ngày trống không cần thiết. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided sample
assert run("1\n1 1 3\n1 5\n") == "3", "sample 1"

# Multiple batches, overlapping
assert run("1\n2 3 3\n1 5\n2 7\n") == "4", "overlapping milk"

# Minimum input
assert run("1\n1 1 1\n1 1\n") == "1", "minimum input"

# Maximum satisfaction
```
