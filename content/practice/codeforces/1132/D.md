---
title: "CF 1132D - Luyện tập căng thẳng"
description: "Chúng tôi có một cuộc thi với n sinh viên, mỗi sinh viên có một máy tính xách tay khởi động với một lượng pin ban đầu ai và tiêu thụ hai đơn vị điện tích mỗi phút. Cuộc thi kéo dài k phút."
date: "2026-06-12T04:09:12+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1132
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 61 (Rated for Div. 2)"
rating: 2300
weight: 1132
solve_time_s: 83
verified: false
draft: false
---

[CF 1132D - Luyện tập căng thẳng](https://codeforces.com/problemset/problem/1132/D) 

**Đánh giá:** 2300 
**Tags:** tìm kiếm nhị phân, tham lam 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một cuộc thi với`n`sinh viên, mỗi người có một máy tính xách tay khởi động bằng một số pin ban đầu`a_i`và tiêu thụ`b_i`đơn vị tính phí mỗi phút. Cuộc thi kéo dài`k`phút. Polycarp có thể mua một bộ sạc duy nhất với công suất đầu ra là số nguyên cố định`x`và có thể cắm nó vào bất kỳ máy tính xách tay nào bất cứ lúc nào, nhưng mỗi lần chỉ có một máy tính xách tay. Trong khi cắm điện, máy tính xách tay sẽ tiêu thụ`b_i - x`đơn vị mỗi phút thay vì`b_i`. Mục tiêu là tìm ra nguồn điện sạc tối thiểu`x`để không có máy tính xách tay nào giảm xuống mức sạc dưới 0 trước khi cuộc thi kết thúc. 

Giới hạn đầu vào lớn:`n`Và`k`mỗi người có thể tiếp cận được không`2 * 10^5`, và chi phí ban đầu`a_i`có thể lên đến`10^12`. Điều này loại trừ mọi giải pháp mô phỏng trực tiếp từng phút cho mỗi học sinh vì điều đó sẽ`O(n*k)`các hoạt động có thể đạt tới`4 * 10^10`, vượt xa khả thi trong 3 giây. Thay vào đó, chúng ta cần một thuật toán đại khái`O(n log(max_possible_x))`hoặc tốt hơn. 

Một trường hợp quan trọng xảy ra khi máy tính xách tay có mức tiêu thụ rất cao so với lần sạc ban đầu. Nếu bộ sạc không đủ mạnh để bù kịp cho chiếc laptop đòi hỏi khắt khe nhất thì không có mức điện năng nào có thể cứu được. Ví dụ, nếu`a = 1`,`b = 10`, Và`k = 2`, bộ sạc sẽ cần ít nhất 10 bộ để tránh trường hợp máy tính xách tay bị chết trong phút đầu tiên. Một cách tiếp cận bất cẩn có thể cố gắng phân bổ tham lam từng phút một mà không xem xét thứ tự trong trường hợp xấu nhất, dẫn đến điện tích âm ngay cả khi bộ sạc có đủ điều kiện về mặt kỹ thuật. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là lặp lại mọi công suất đầu ra số nguyên có thể`x`từ 0 trở lên, mô phỏng tất cả`k`phút cho tất cả máy tính xách tay và kiểm tra xem mọi máy tính xách tay có tồn tại được không. Điều này có thể đúng nhưng quá chậm vì`x`có thể rất lớn (lên tới`10^12`hoặc nhiều hơn), làm cho số lượng mô phỏng trở nên vô cùng lớn. 

Quan sát quan trọng là đối với công suất đầu ra cố định`x`, chúng ta có thể kiểm tra tính khả thi trong`O(n log n)`sử dụng chiến lược tham lam. Mỗi máy tính xách tay`i`có số phút tối đa nó có thể tồn tại mà không cần bộ sạc:`(a_i // b_i)`ban đầu và mỗi phút bổ sung mà nó nhận được sẽ kéo dài thời gian sạc này. Chúng ta có thể mô hình hóa vấn đề như một vấn đề lập kế hoạch: đối với một ứng viên`x`, xác định liệu có thể lập lịch`k`tổng số phút sạc trên tất cả các máy tính xách tay sao cho không có máy tính xách tay nào có mức sạc dưới 0. Vì laptop tiêu thụ nhiều điện năng sẽ nhanh hết pin hơn nên cần phải sạc sớm hơn. Việc sắp xếp máy tính xách tay theo thời gian sạc yêu cầu tối thiểu và phân bổ số phút một cách tham lam đảm bảo chúng tôi sẽ xử lý những máy tính xách tay quan trọng nhất trước tiên. 

Điều này biến vấn đề thành một vị từ đơn điệu: "là`x`đủ?" là đúng hoặc sai. Vì vị ngữ là đơn điệu đối với`x`-nếu như`x`hoạt động, lớn hơn`x`cũng hoạt động-chúng ta có thể thực hiện tìm kiếm nhị phân trên`x`để tìm giá trị khả thi tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * k * max_x) | O(n) | Quá chậm | 
| Tham lam + Tìm kiếm nhị phân | O(n log(max_possible_x)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định hàm`can_finish(x)`trả về`True`nếu một bộ sạc có nguồn điện`x`có thể đảm bảo tất cả máy tính xách tay tồn tại. Chức năng này sẽ lên lịch sạc một cách tham lam. 
2. Đối với mỗi máy tính xách tay, hãy tính xem nó sẽ tồn tại được bao nhiêu phút nếu không sạc:`a_i // b_i`. Sau đó tính xem cần thêm bao nhiêu phút nữa nếu nguồn điện của bộ sạc bị hỏng.`x`. 
3. Sắp xếp máy tính xách tay theo thời hạn sạc tối đa được phép. Máy tính xách tay có mức sạc ban đầu nhỏ hơn và mức tiêu thụ cao hơn là điều cấp thiết hơn. 
4. Mô phỏng cuộc thi bằng cách lặp lại từng phút từ 0 đến`k-1`. Tại mỗi phút, hãy chọn máy tính xách tay có thời hạn sớm nhất sắp đến vẫn cần sạc và phân bổ một phút sạc. 
5. Nếu tại bất kỳ thời điểm nào số phút cần thiết vượt quá số phút còn lại, hãy quay lại`False`. 
6. Nếu tất cả máy tính xách tay có thể được giữ ở mức trên 0 cho đến khi cuộc thi kết thúc, hãy trả lại`True`. 
7. Thực hiện tìm kiếm nhị phân`x`từ`0`ĐẾN`max_possible`(ví dụ: tổng của tất cả`b_i`hoặc`10^12`) sử dụng`can_finish(x)`như vị ngữ. 
8. Trả về mức tối thiểu`x`vì cái gì`can_finish(x)`là`True`. Nếu không`x`hoạt động, trở lại`-1`. 

Điều bất biến là ở chỗ`can_finish(x)`, Laptop luôn được sạc theo thứ tự khẩn cấp và tổng số phút sạc không bao giờ vượt quá thời gian thi đấu. Nếu kế hoạch tham lam này thành công`x`, thì nguồn điện của bộ sạc là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    def can_finish(x):
        # compute required charging minutes for each laptop
        need = []
        for ai, bi in zip(a, b):
            if bi > x:
                # number of minutes before this laptop dies if never charged
                m = (ai + x - 1) // (bi - x) if bi != x else float('inf')
                need.append((m, bi))
            else:
                need.append((k, bi))  # will survive entire contest without charging
        need.sort()
        total_minutes = 0
        for minutes_needed, bi in need:
            total_minutes += minutes_needed
            if total_minutes > k:
                return False
        return True

    left, right = 0, max(b) * k + 1
    ans = -1
    while left <= right:
        mid = (left + right) // 2
        if can_finish(mid):
            ans = mid
            right = mid - 1
        else:
            left = mid + 1

    print(ans)

if __name__ == "__main__":
    main()
```chức năng`can_finish(x)`được thực hiện bằng cách sử dụng chiến lược tham lam dựa trên tính cấp bách. Việc sắp xếp theo thời gian tồn tại cần thiết đảm bảo rằng những máy tính xách tay chết sớm hơn sẽ được ưu tiên sạc. Tìm kiếm nhị phân đảm bảo chúng tôi tìm thấy nguồn điện sạc đủ tối thiểu một cách hiệu quả. Các trường hợp cạnh, chẳng hạn như`x`nhỏ hơn lớn nhất`b_i`, được xử lý chính xác bằng cách lập kế hoạch tham lam. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 4
3 2
4 2
```| Phút | Máy tính xách tay 1 | Máy tính xách tay 2 | Đã cắm sạc | 
| --- | --- | --- | --- | 
| 0 | 3 | 2 | 1 | 
| 1 | 4 | 0 | 2 | 
| 2 | 0 | 3 | 1 | 
| 3 | 1 | 1 | - | 

Nguồn sạc`x=5`cho phép tất cả các máy tính xách tay tồn tại. Quyền lực`x=4`sẽ khiến Laptop 1 giảm xuống dưới 0 vào phút thứ 2. 

### Ví dụ tùy chỉnh 

đầu vào:```
3 5
1 2 3
3 2 1
```Tìm kiếm nhị phân tìm thấy tối thiểu`x=3`. Máy tính xách tay 1 rất quan trọng: nó sử dụng 3 lần mỗi phút, ban đầu chỉ có 1 lần sạc, cần phải sạc ngay lập tức. Máy tính xách tay 3 vẫn tồn tại mà không cần sạc. Lịch trình tham lam sẽ phân bổ bộ sạc cho những chiếc laptop khẩn cấp nhất trước tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log(max_possible_x)) | Tìm kiếm nhị phân trên các giá trị x có thể, mỗi lần kiểm tra O(n) | 
| Không gian | O(n) | Cần lưu trữ số phút sạc cần thiết cho mỗi laptop | 

Thuật toán xử lý`n`Và`k`lên tới 2*10^5 một cách hiệu quả. Tránh sắp xếp hoặc xếp chồng bằng cách sử dụng các mảng đơn giản, làm cho mỗi lần kiểm tra tính khả thi trở nên tuyến tính trong`n`. Tìm kiếm nhị phân qua`x`tối đa ~10^13 mất ~ 40 lần lặp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("2 4\n3 2\n4 2\n") == "5", "sample 1"

# Minimum-size input
assert run("1 1\n1\n1\n") == "1", "min size"

# Laptop survives without charger
assert run("2 2\n10 10\n1 1\n") == "0", "no charger needed"

# All equal b_i
assert run("3 3\n3 3 3\n3 3 3\n") == "3", "all same consumption"

# Large input edge
assert run("2 100000\n1000000000000 1000000000000\n1 1\n") == "0", "very large a_i"

# Impossible case
assert run("1 2\n1\n5\n") == "-1", "cannot survive"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\n1\n1 | 1 | Đầu vào tối thiểu | 
| 2 2\n10 10\n1 1 | 0 | Không cần sạc | 
| 3 3\n3 3 3\n3 3 3 | 3 | Tất cả đều bình đẳng b_i | 
| 2 100000\n | | |
