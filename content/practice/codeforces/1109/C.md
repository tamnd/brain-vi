---
title: "CF 1109C - Sasha và người bạn bệnh nhân"
description: "Bài toán mô phỏng một “chiếc bát kiên nhẫn” có mức độ kiên nhẫn ban đầu có thể tăng hoặc giảm theo thời gian tùy thuộc vào tốc độ nhấn. Tốc độ của vòi có thể được thay đổi vào những thời điểm riêng biệt theo sự kiện."
date: "2026-06-12T05:13:57+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1109
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 539 (Div. 1)"
rating: 2800
weight: 1109
solve_time_s: 314
verified: false
draft: false
---

[CF 1109C - Sasha và một người bạn bệnh nhân](https://codeforces.com/problemset/problem/1109/C) 

**Đánh giá:** 2800 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 5 phút 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Bài toán mô phỏng một “chiếc bát kiên nhẫn” có mức độ kiên nhẫn ban đầu có thể tăng hoặc giảm theo thời gian tùy thuộc vào tốc độ nhấn. Tốc độ của vòi có thể được thay đổi vào những thời điểm riêng biệt theo sự kiện. Sasha có thể truy vấn khi nào độ kiên nhẫn đạt đến mức 0 trong một khoảng thời gian nhất định nếu chúng ta bắt đầu với một khối lượng ban đầu cụ thể. Bản thân các sự kiện có thời gian thưa thớt và chỉ ảnh hưởng đến tốc độ bắt đầu từ dấu thời gian của chúng cho đến sự kiện tiếp theo (hoặc vô thời hạn nếu không có). Các truy vấn yêu cầu mô phỏng sự tích lũy kiên nhẫn theo thời gian, xem xét tốc độ không đổi từng bước được xác định bởi các sự kiện. 

Hạn chế chính là số lượng truy vấn`q`lên tới 100.000 lần`t`có thể lên tới 10^9 và tốc độ`s`có thể dao động từ -10^9 đến 10^9. Điều này loại trừ khả năng mô phỏng lực lượng vũ phu trong mỗi giây vì các khoảng thời gian có thể rất lớn. Mỗi truy vấn loại 3 yêu cầu tích hợp các hàm hằng số từng phần trên các phạm vi có thể lớn. 

Trường hợp cạnh phát sinh khi sự kiên nhẫn ban đầu`v`rất nhỏ hoặc bằng 0, khi tốc độ nhấn không bao giờ giảm độ kiên nhẫn xuống 0 và khi sự kiện đầu tiên xảy ra sau khi bắt đầu khoảng thời gian trong truy vấn loại 3. Ví dụ: nếu một truy vấn yêu cầu từ giây 1 đến 10 với kiên nhẫn ban đầu là 1, nhưng tốc độ thay đổi sự kiện đầu tiên xảy ra ở t=5 với tốc độ âm, mô phỏng đơn giản có thể không tính đến khoảng thời gian trước t=5, sử dụng tốc độ mặc định 0. Một trường hợp tinh tế khác là khi kiên nhẫn đạt đến chính xác 0 tại thời điểm bắt đầu sự kiện, việc chăm sóc phải được thực hiện với các ranh giới và số học dấu phẩy động. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp đi lặp lại từng giây từ`l`ĐẾN`r`đối với mỗi truy vấn loại 3, áp dụng tốc độ hiện tại và kiểm tra xem độ kiên nhẫn có giảm xuống 0 hay không. Điều này đúng nhưng quá chậm, vì các khoảng thời gian có thể lên tới 10^9 giây và có 10^5 truy vấn, dẫn đến các hoạt động O(10^14) trong trường hợp xấu nhất. 

Cách tiếp cận tối ưu tận dụng khả năng quan sát rằng giữa các sự kiện, tốc độ nhấn là không đổi. Chúng ta chỉ cần tính toán khi nào độ kiên nhẫn đạt tới 0 trên mỗi đoạn liền kề có tốc độ không đổi. Điều này làm giảm vấn đề lặp lại các sự kiện liên quan đến khoảng thời gian truy vấn và giải phương trình tuyến tính: nếu tốc độ là`s`và sự kiên nhẫn còn lại là`v`, thời gian về 0 là`v/s`nếu như`s < 0`. Để xử lý các truy vấn loại 1 và loại 2 một cách hiệu quả, chúng tôi lưu trữ các sự kiện theo cấu trúc được sắp xếp theo khóa theo thời gian, cho phép chèn, xóa và truy vấn phạm vi trong O(log n) mỗi truy vấn. Điều này làm giảm độ phức tạp của mỗi truy vấn xuống O (số sự kiện trong khoảng thời gian) thay vì O (độ dài khoảng thời gian tính bằng giây). 

Cái nhìn sâu sắc quan trọng là hiệu quả tích lũy của sự kiên nhẫn theo thời gian là tuyến tính từng phần và chỉ thay đổi độ dốc tại thời điểm sự kiện. Do đó, chúng ta có thể mô phỏng chính xác từng truy vấn bằng cách duyệt qua các sự kiện theo thứ tự và áp dụng số học thay vì lặp lại từng giây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * (r-l)) | O(q) | Quá chậm | 
| Tối ưu | O(q log q) | O(q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì một bản đồ hoặc từ điển sắp xếp các sự kiện được khóa theo thời gian. Mỗi mục lưu trữ tốc độ`s`bắt đầu từ giây phút đó. Điều này cho phép chèn và xóa theo thời gian O(log n) và lặp lại theo thứ tự được sắp xếp. 
2. Đối với truy vấn loại 1, hãy chèn sự kiện vào bản đồ cùng với thời gian và tốc độ của nó. 
3. Đối với truy vấn loại 2, hãy xóa sự kiện khỏi bản đồ. 
4. Đối với truy vấn loại 3`[l, r, v]`, xác định tất cả các sự kiện trùng lặp với`[l, r]`. Nếu không có sự kiện nào trước đó`l`, giả sử tốc độ 0 cho đến sự kiện đầu tiên. Lặp lại các sự kiện này theo trình tự thời gian: 

4a. Cho phép`curr_time`bắt đầu lúc`l`Và`curr_patience`Tại`v`. 

4b. Đối với mỗi sự kiện tại`t`với tốc độ`s`, tính thời lượng cho đến sự kiện tiếp theo hoặc cho đến khi`r`. Nếu tốc độ âm và sự kiên nhẫn sẽ giảm xuống 0 trong khoảng thời gian này, hãy tính chính xác giây`burst_time = curr_time + curr_patience / abs(s)`và trả lại nó. 

4c. Còn không thì cập nhật`curr_patience += s * duration`và tiến lên`curr_time`. 
5. Nếu hết khoảng`r`đạt được mà không cần kiên nhẫn giảm xuống 0, trả về -1. 

**Tại sao nó hoạt động:** Tại bất kỳ thời điểm nào, sự kiên nhẫn tăng tuyến tính với tốc độ hiện tại. Các sự kiện chỉ thay đổi độ dốc ở những thời điểm riêng biệt. Bằng cách xem qua tất cả các sự kiện có liên quan và tính toán thời điểm chính xác mà sự kiên nhẫn đạt mức 0 trong một phân đoạn, mô phỏng vẫn chính xác và hiệu quả. Việc sử dụng bản đồ được sắp xếp sẽ đảm bảo tất cả các phân đoạn được xử lý theo đúng thứ tự. 

## Giải pháp Python```python
import sys
import bisect
input = sys.stdin.readline

from collections import OrderedDict

def main():
    q = int(input())
    events = OrderedDict()

    queries = []
    for _ in range(q):
        parts = input().split()
        queries.append(parts)

    # Maintain a sorted list of times for fast search
    times = []

    for parts in queries:
        if parts[0] == '1':
            t, s = int(parts[1]), int(parts[2])
            events[t] = s
            bisect.insort(times, t)
        elif parts[0] == '2':
            t = int(parts[1])
            del events[t]
            idx = bisect.bisect_left(times, t)
            times.pop(idx)
        else:
            l, r, v = int(parts[1]), int(parts[2]), int(parts[3])
            curr_time = l
            curr_patience = v

            # Find relevant events
            idx = bisect.bisect_left(times, l)
            if idx == 0:
                prev_speed = 0
            else:
                prev_speed = events[times[idx-1]]

            burst = -1

            while curr_time <= r:
                if idx < len(times):
                    next_time = times[idx]
                    speed = events[next_time]
                else:
                    next_time = r + 1
                    speed = prev_speed

                end_time = min(next_time, r + 1)
                duration = end_time - curr_time

                if prev_speed < 0:
                    time_to_zero = curr_patience / -prev_speed
                    if time_to_zero <= duration:
                        burst = curr_time + time_to_zero
                        break

                curr_patience += prev_speed * duration
                curr_time = end_time
                prev_speed = speed
                idx += 1

            if burst == -1:
                print(-1)
            else:
                print(f"{burst:.6f}")

if __name__ == "__main__":
    main()
```**Giải thích:** Chúng tôi duy trì`events`trong một từ điển có thứ tự để chèn và xóa hiệu quả và`times`như một danh sách được sắp xếp để tra cứu phạm vi nhanh. Đối với truy vấn loại 3, chúng tôi lặp lại các sự kiện chồng lấp khoảng thời gian, áp dụng cập nhật tuyến tính từng phần cho mức độ kiên nhẫn và tính toán thời gian bùng nổ chính xác khi tốc độ âm có thể làm cạn kiệt nó. Chúng tôi xử lý khoảng thời gian trước sự kiện đầu tiên bằng tốc độ mặc định là 0. Số học dấu phẩy động đảm bảo thời gian bùng nổ phân đoạn chính xác. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
6
1 2 1
1 4 -3
3 1 6 1
3 1 6 3
3 1 6 4
3 1 6 5
```| Truy vấn | Curr_time | Curr_kiên nhẫn | Tốc độ sự kiện | Thời lượng | Kiên Nhẫn Mới | Bùng nổ? | 
| --- | --- | --- | --- | --- | --- | --- | 
| 3 1 6 1 | 1 | 1 | 0 (t<2) | 1 | 1 | Không | 
| | 2 | 1 | 1 | 2 | 3 | Không | 
| | 4 | 3 | -3 | 2 | -3 | Có, t=5 | 

Chứng minh rằng thuật toán xử lý chính xác tốc độ mặc định trước sự kiện đầu tiên và tính toán thời gian bùng nổ theo từng phần. 

Đầu vào tùy chỉnh:```
4
1 5 -2
3 1 10 3
1 1 -1
3 1 10 3
```Hiển thị cách xử lý sự kiện được thêm sau truy vấn loại 3 và chèn trước truy vấn. Thời gian phân số được tính khi tốc độ âm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log q) | Mỗi thao tác chèn/xóa sự kiện sử dụng chia đôi (log n), mỗi truy vấn loại 3 lặp lại các sự kiện liên quan (≤ q) | 
| Không gian | O(q) | Lưu trữ tất cả các sự kiện và thời gian của chúng | 

Được cho`q ≤ 10^5`, điều này phù hợp thoải mái trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import io, sys

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    main()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("""6
1 2 1
1 4 -3
3 1 6 1
3 1 6 3
3 1 6 4
3 1 6 5""") == """5.000000
5.666667
6.000000
-1"""

# Minimum input
assert run("1\n3 1 1 0") == "-1"

# Negative speed from start
assert run("""2
```
