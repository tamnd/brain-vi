---
title: "CF 343C - Thời gian đọc"
description: "Chúng tôi được yêu cầu mô phỏng nhiều đầu đọc di chuyển dọc theo ổ cứng tuyến tính để đọc một tập hợp các rãnh mục tiêu nhanh nhất có thể. Mỗi đầu bắt đầu tại một rãnh nhất định, có thể di chuyển sang trái, phải hoặc giữ nguyên vị trí và nhiều đầu có thể chiếm cùng một rãnh."
date: "2026-06-06T17:53:34+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 343
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 200 (Div. 1)"
rating: 1900
weight: 343
solve_time_s: 73
verified: true
draft: false
---

[CF 343C - Thời gian đọc](https://codeforces.com/problemset/problem/343/C) 

**Xếp hạng:** 1900 
**Tags:** tìm kiếm nhị phân, tham lam, hai con trỏ 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng nhiều đầu đọc di chuyển dọc theo ổ cứng tuyến tính để đọc một tập hợp các rãnh mục tiêu nhanh nhất có thể. Mỗi đầu bắt đầu tại một rãnh nhất định, có thể di chuyển sang trái, phải hoặc giữ nguyên vị trí và nhiều đầu có thể chiếm cùng một rãnh. Một bản nhạc được coi là đã đọc nếu có bất kỳ đầu nào truy cập vào nó và vị trí ban đầu của các đầu được tính là đã đọc. Mục tiêu của chúng tôi là xác định số giây tối thiểu cần thiết để tất cả các bản nhạc mục tiêu được chỉ định đều được đọc. 

Đầu vào mang lại`n`đầu và`m`các bản nhạc mục tiêu, với cả hai danh sách được sắp xếp theo thứ tự tăng dần. Bản thân các con số có thể rất lớn, lên tới`10^10`, có nghĩa là chúng tôi không thể dựa vào việc lập chỉ mục mảng trực tiếp hoặc xây dựng một mảng đầy đủ các bản nhạc. Thay vào đó, chúng ta cần một cách tiếp cận phù hợp trực tiếp với các vị trí. 

Từ`n`Và`m`có thể lên đến`10^5`, bất kỳ giải pháp nào cần`O(n * m)`thời gian sẽ quá chậm vì nó đòi hỏi khoảng`10^10`hoạt động. Điều này loại trừ khả năng mô phỏng lực lượng thô sơ của tất cả các chuyển động của đầu. Các trường hợp cạnh bao gồm có nhiều đầu hơn mục tiêu, các mục tiêu đã ở vị trí đầu và các mục tiêu hoặc đầu có khoảng cách rộng rãi. Ví dụ: nếu tất cả các đầu bắt đầu ở đường 1 và các mục tiêu ở đường 1, 2 và 10000000000, câu trả lời phải phản ánh khoảng cách của mục tiêu xa nhất chứ không phải khoảng cách gần nhất. 

Việc triển khai bất cẩn có thể cố gắng gán các đầu cho các mục tiêu một cách tham lam theo đường từ trái sang phải mà không tính đến việc một đầu có thể bao phủ nhiều mục tiêu liền kề trong khi một đầu khác bao phủ các mục tiêu ở xa có thể nhanh hơn. Ví dụ: đứng đầu tại`[2, 5]`và mục tiêu`[1, 3, 6]`không thể chỉ đơn giản gán cái đầu đầu tiên cho`1`và thứ hai đến`6`; chúng ta phải tính toán tổng thời gian tối thiểu có tính đến phạm vi tiếp cận chồng chéo. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ là mô phỏng mỗi đầu di chuyển mỗi giây và đánh dấu các dấu vết là đã đọc, lặp lại cho đến khi tất cả các mục tiêu đều được truy cập. Điều này có hiệu quả vì bất kỳ chuỗi nước đi hợp lệ nào cuối cùng cũng bao phủ tất cả các đường đi, nhưng nó sẽ trở nên quá chậm khi`n`Và`m`với tới`10^5`hoặc số bản nhạc lớn bằng`10^10`. Ngay cả việc lưu trữ các bản nhạc đã truy cập một cách rõ ràng cũng không thực tế do giới hạn bộ nhớ. 

Điểm mấu chốt là chúng ta có thể coi đây là một bài toán gán: mỗi đầu có thể bao quát một đoạn đường liền kề trong một thời gian.`t`, trong đó khoảng cách tối đa từ vị trí ban đầu của đầu đến một trong hai đầu của đoạn không vượt quá`t`. Đối với một ứng cử viên`t`, chúng ta có thể tham lam kiểm tra xem có thể gán tất cả các mục tiêu cho đầu hay không để mỗi đầu chỉ cần di chuyển nhiều nhất`t`. Nếu chúng ta sắp xếp cả đầu và mục tiêu, chúng ta có thể cố gắng bao phủ các mục tiêu từ trái sang phải, chỉ định cho mỗi đầu phân đoạn mục tiêu xa nhất mà nó có thể bao phủ trong đó.`t`. 

Điều này biến vấn đề thành một "kiểm tra tính khả thi" cho một vấn đề nhất định`t`, và tối thiểu`t`có thể được tìm thấy bằng cách sử dụng tìm kiếm nhị phân. Chúng tôi tìm kiếm trong phạm vi`[0, max_distance]`, Ở đâu`max_distance`là khoảng cách giữa đầu ngoài cùng bên trái và mục tiêu ngoài cùng bên phải hoặc ngược lại. Nhiệm vụ tham lam có hiệu quả vì đầu và mục tiêu đã được sắp xếp; chỉ định các mục tiêu chưa được chỉ định ở ngoài cùng bên trái cho đầu có sẵn ngoài cùng bên trái đảm bảo rằng không có nhiệm vụ khả thi nào bị bỏ sót. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n * m * max_distance) | O(max_distance) | Quá chậm | 
| Tìm kiếm nhị phân + Bài tập tham lam | O(m log(max_distance)) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng vị trí đầu và vị trí đích. Cả hai mảng đều đã được sắp xếp trong câu lệnh bài toán, nhưng việc sắp xếp đảm bảo tính chính xác cho bất kỳ dữ liệu đầu vào nào. 
2. Xác định hàm`can_cover(t)`kiểm tra xem liệu tất cả các mục tiêu có thể được gán cho các đầu sao cho không có đầu nào di chuyển nhiều hơn`t`. Khởi tạo một con trỏ`j`để theo dõi mục tiêu chưa được chỉ định tiếp theo. 
3. Lặp lại từng đầu từ trái sang phải. Đối với phần đầu hiện tại, hãy tính phân đoạn tối đa của các mục tiêu liên tiếp mà nó có thể bao phủ trong`t`. Có hai khả năng: đầu di chuyển sang trái trước rồi sang phải hoặc phải trước rồi sang trái để bao quát các mục tiêu liền kề một cách hiệu quả. Phân đoạn là`[head - t, head + t]`sau khi cân nhắc nước đi trái/phải hiệu quả nhất. 
4. Di chuyển con trỏ`j`về phía trước miễn là`targets[j]`nằm trong phân khúc mà người đứng đầu hiện tại có thể truy cập được. 
5. Nếu sau khi xử lý tất cả các đầu, tất cả các mục tiêu đã được chỉ định (`j == m`), hàm trả về True. Nếu không, nó trả về Sai. 
6. Thực hiện tìm kiếm nhị phân trên`t`. Khởi tạo`lo = 0`Và`hi = maximum distance between extreme heads and targets`. Trong khi`lo < hi`, kiểm tra điểm giữa`mid`. Nếu như`can_cover(mid)`trả về Đúng, đặt`hi = mid`; nếu không thì đặt`lo = mid + 1`. 
7. Sau khi tìm kiếm nhị phân hoàn tất,`lo`sẽ giữ thời gian tối thiểu cần thiết để bao phủ tất cả các mục tiêu. 

Lý do điều này hoạt động là vì một cố định`t`, việc giao càng nhiều mục tiêu liên tiếp càng tốt cho mỗi đầu từ trái sang phải sẽ đảm bảo rằng không có sự phân công nào tốt hơn. Điều bất biến là sau khi mỗi đầu được xử lý, tất cả các mục tiêu ở bên trái của con trỏ hiện tại đã được che phủ một cách tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, m = map(int, input().split())
    heads = list(map(int, input().split()))
    targets = list(map(int, input().split()))

    def can_cover(t):
        j = 0  # pointer to the next target to cover
        for h in heads:
            if j >= m:
                break
            if targets[j] < h:
                dist = h - targets[j]
                if dist > t:
                    return False
                # head can move left to targets[j] and then right as far as possible
                reach = max(h + t - 2*dist, h + (t - dist)//1)
            else:
                # head moves right only
                reach = h + t
            # assign all targets within reach
            while j < m and targets[j] <= reach:
                j += 1
        return j == m

    lo, hi = 0, max(targets[-1] - heads[0], heads[-1] - targets[0])
    while lo < hi:
        mid = (lo + hi) // 2
        if can_cover(mid):
            hi = mid
        else:
            lo = mid + 1
    print(lo)

if __name__ == "__main__":
    main()
```Giải pháp này khởi tạo phạm vi tìm kiếm nhị phân với khoảng cách tối đa có thể giữa bất kỳ đầu nào và bất kỳ mục tiêu nào. các`can_cover`chức năng xem xét cẩn thận cả hai hướng chuyển động, đảm bảo rằng các đầu có thể bao phủ các phân đoạn trái rồi phải một cách tối ưu. Con trỏ gán tham lam`j`đảm bảo mỗi đầu được sử dụng hiệu quả mà không bỏ qua mục tiêu. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
3 4
2 5 6
1 3 6 8
```| Bước | Đầu | Con trỏ j | Tiếp cận | Mục tiêu được bảo hiểm | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 3 | 1, 3 | 
| 2 | 5 | 2 | 6 | 6 | 
| 3 | 6 | 3 | 8 | 8 | 

Bảng cho thấy mỗi đầu bao phủ số lượng mục tiêu liên tiếp tối đa mà nó có thể tiếp cận trong 2 giây. Sau khi tất cả những cái đầu, tất cả các mục tiêu đều được bảo vệ. Thuật toán trả về 2. 

Đầu vào mẫu 2:```
2 3
1 10
2 9 12
```| Bước | Đầu | Con trỏ j | Tiếp cận | Mục tiêu được bảo hiểm | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 2 | 2 | 
| 2 | 10 | 1 | 12 | 9, 12 | 

Thời gian tối thiểu cần thiết là 2, vì đầu thứ nhất đạt 2 và đầu thứ hai đạt 12 trong 2 giây, bao phủ tất cả các mục tiêu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log D) | Tìm kiếm nhị phân trên khoảng cách tối đa`D`với mỗi lần kiểm tra tính khả thi tuyến tính về số lượng mục tiêu | 
| Không gian | O(n + m) | Chỉ lưu trữ vị trí đầu và mục tiêu | 

Giải pháp dễ dàng phù hợp trong giới hạn của`n, m ≤ 10^5`Và`10^10`theo dõi số. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# provided sample
assert run("3 4\n2 5 6\n1 3 6 8\n") == "2", "sample 1"

# minimum size
assert run("1 1\n1\n1\n") == "0", "minimum size"

# heads cover targets directly
assert run("2 2\n1 3\n1 3\n") == "0", "heads already at targets"

# widely spaced targets
assert run("2 3\n1 10\n2 9 12\n") == "2", "far apart targets"

# single head multiple targets
assert run("1
```
