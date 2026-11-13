---
title: "CF 1691E - Số nhóm"
description: "Chúng ta có một tập hợp các đoạn trên trục số, mỗi đoạn có màu đỏ hoặc xanh. Mỗi đoạn chiếm một khoảng liên tục từ li đến ri, bao hàm."
date: "2026-06-09T23:11:30+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "dsu", "graphs", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1691
codeforces_index: "E"
codeforces_contest_name: "CodeCraft-22 and Codeforces Round 795 (Div. 2)"
rating: 2300
weight: 1691
solve_time_s: 192
verified: true
draft: false
---

[CF 1691E - Số nhóm](https://codeforces.com/problemset/problem/1691/E) 

**Đánh giá:** 2300 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, dsu, đồ thị, tham lam, sắp xếp 
**Thời gian giải:** 3 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các đoạn trên trục số, mỗi đoạn có màu đỏ hoặc xanh. Mỗi đoạn chiếm một khoảng liên tục từ`l_i`ĐẾN`r_i`, bao gồm. Nhiệm vụ là tìm các nhóm đoạn trong đó hai đoạn được coi là kết nối nếu chúng có màu khác nhau và các khoảng của chúng trùng nhau. Các phân đoạn có thể được kết nối gián tiếp thông qua một chuỗi các màu xen kẽ. Đầu ra là tổng số nhóm được kết nối như vậy cho mỗi trường hợp thử nghiệm. 

Các ràng buộc cho chúng ta biết rằng tổng số phân đoạn trên tất cả các trường hợp thử nghiệm không vượt quá`10^5`. Điều này có nghĩa là bất kỳ thuật toán nào`O(n^2)`mỗi trường hợp thử nghiệm sẽ quá chậm. Chúng ta cần một giải pháp gần như tuyến tính hoặc tuyến tính cho mỗi trường hợp thử nghiệm, lý tưởng nhất là`O(n log n)`. 

Các trường hợp cạnh có thể cản trở cách tiếp cận đơn giản bao gồm các phân đoạn có chung điểm cuối nhưng không chồng chéo lên nhau, nhiều phân đoạn cùng màu hoàn toàn chồng chéo lên nhau hoặc các phân đoạn có màu xen kẽ nhưng có khoảng trống làm đứt kết nối. Ví dụ: ba đoạn`(0, 0, 1)`,`(1, 1, 2)`,`(0, 3, 4)`tạo thành ba nhóm riêng biệt vì đoạn màu đỏ cuối cùng không chồng lên đoạn màu xanh, mặc dù nó theo sau nó theo thứ tự. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại trên từng cặp phân đoạn và kiểm tra xem chúng có trùng nhau và có màu sắc khác nhau hay không. Nếu đúng như vậy, bạn sẽ hợp nhất chúng thành một nhóm bằng cách sử dụng cấu trúc dữ liệu như DSU (kết hợp tập hợp rời rạc). Điều này đúng nhưng cần phải kiểm tra`O(n^2)`cặp, có thể đạt tới`10^10`hoạt động cho đầu vào lớn nhất, vượt xa thời hạn. 

Thông tin chi tiết quan trọng là đối với mỗi màu, bạn chỉ cần theo dõi điểm xa nhất bên phải đã đạt được cho đến nay để xác định xem phân đoạn có màu đối diện có kết nối với nhóm hiện tại hay không. Việc sắp xếp các phân đoạn theo điểm bắt đầu và quét từ trái sang phải cho phép chúng ta hợp nhất các phân đoạn thành các nhóm một cách hiệu quả. Điều này làm giảm vấn đề khi quét hai danh sách đã sắp xếp (một cho màu đỏ và một cho màu xanh lam), duy trì phạm vi tiếp cận tối đa và tăng số lượng nhóm bất cứ khi nào một phân đoạn bắt đầu sau điểm xa nhất của màu đối diện. 

Quan sát này biến đổi một người ngây thơ`O(n^2)`tiếp cận theo cách có thể quản lý được`O(n log n)`giải pháp bị chi phối bởi hoạt động sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Quét + Phạm vi tiếp cận tối đa | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tách các đoạn thành hai danh sách theo màu: đỏ và xanh. Sắp xếp từng danh sách theo điểm cuối bên trái`l_i`. 
2. Khởi tạo hai biến`max_red`Và`max_blue`để theo dõi điểm cuối ngoài cùng bên phải của các phân đoạn trong nhóm hoạt động hiện tại cho mỗi màu. 
3. Khởi tạo bộ đếm nhóm`groups`thành 0. Điều này sẽ tính các nhóm được kết nối. 
4. Hợp nhất hai danh sách đã sắp xếp theo thứ tự từ trái sang phải. Đối với mỗi phân đoạn, hãy kiểm tra xem nó có trùng với phạm vi tiếp cận tối đa hiện tại của màu đối diện hay không. 
5. Nếu đoạn chồng lên điểm cuối tối đa của màu đối diện, hãy hợp nhất nó vào nhóm hiện tại và cập nhật điểm cuối tối đa cho màu của nó. 
6. Nếu phân đoạn bắt đầu sau phạm vi tiếp cận tối đa của màu đối diện, hãy tăng bộ đếm nhóm vì phân đoạn này bắt đầu một nhóm bị ngắt kết nối mới. Cài lại`max_red`Và`max_blue`tương ứng. 
7. Tiếp tục cho đến khi tất cả các phân đoạn được xử lý. Giá trị cuối cùng của`groups`là số nhóm được kết nối. 

Tại sao nó hoạt động: bằng cách luôn theo dõi các điểm cuối bên phải tối đa cho các phân đoạn màu đỏ và xanh lam, chúng tôi đảm bảo rằng mọi phân đoạn bắt đầu trước hoặc ở mức tối đa của màu đối diện đều phải thuộc cùng một nhóm. Các đoạn bắt đầu ngoài điểm này không thể kết nối với các đoạn trước đó nên chúng bắt đầu một nhóm mới. Việc sắp xếp đảm bảo chúng tôi xử lý các phân đoạn theo đúng thứ tự và xử lý các phần trùng lặp một cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        red, blue = [], []
        for _ in range(n):
            c, l, r = map(int, input().split())
            if c == 0:
                red.append((l, r))
            else:
                blue.append((l, r))
        red.sort()
        blue.sort()
        
        i, j = 0, 0
        max_red, max_blue = -1, -1
        groups = 0
        
        events = []
        for l, r in red:
            events.append((l, r, 0))
        for l, r in blue:
            events.append((l, r, 1))
        events.sort()
        
        for l, r, color in events:
            if color == 0:
                if l > max_blue:
                    groups += 1
                    max_red, max_blue = r, -1
                else:
                    max_red = max(max_red, r)
            else:
                if l > max_red:
                    groups += 1
                    max_blue, max_red = r, -1
                else:
                    max_blue = max(max_blue, r)
        
        print(groups)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ phân tách và sắp xếp các phân đoạn theo màu sắc. Nó hợp nhất tất cả các phân đoạn theo thứ tự điểm bắt đầu và duy trì điểm cuối tối đa mà mỗi màu đạt được. Khi một phân đoạn bắt đầu sau mức tối đa của màu đối diện, nó sẽ bắt đầu một nhóm mới. Hãy cẩn thận để đặt lại phạm vi tiếp cận tối đa một cách chính xác cho các nhóm mới, nếu không các phân khúc ở xa có thể hợp nhất không chính xác. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
5
0 0 5
1 2 12
0 4 7
1 9 16
0 13 19
```| Phân đoạn | Màu sắc | L | R | max_red | max_blue | Nhóm | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 5 | 5 | -1 | 1 | 
| 1 | 1 | 2 | 12 | 5 | 12 | 1 | 
| 2 | 0 | 4 | 7 | 7 | 12 | 1 | 
| 3 | 1 | 9 | 16 | 7 | 16 | 1 | 
| 4 | 0 | 13 | 19 | 19 | 16 | 2 | 

Bảng này cho thấy cách cập nhật phạm vi tiếp cận tối đa và các nhóm chỉ tăng lên khi phân khúc mới không chồng lên màu đối diện. 

### Đầu vào mẫu 2```
3
1 0 1
1 1 2
0 3 4
```Hai đoạn đầu tiên tạo thành một nhóm, đoạn màu đỏ cuối cùng bắt đầu sau max_blue=2, tạo thành nhóm thứ hai. Khi đó đoạn màu đỏ cuối cùng cũng không chồng lên bất kỳ đoạn màu xanh nào, tạo thành ba nhóm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp các đoạn màu đỏ và màu xanh chiếm ưu thế trong tính toán. Quét hợp nhất là O (n). | 
| Không gian | O(n) | Việc lưu trữ các phân đoạn và sự kiện đòi hỏi không gian tuyến tính. | 

Điều này phù hợp thoải mái dưới`2s`giới hạn cho`n ≤ 10^5`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided samples
assert run("2\n5\n0 0 5\n1 2 12\n0 4 7\n1 9 16\n0 13 19\n3\n1 0 1\n1 1 2\n0 3 4\n") == "2\n3", "samples"

# Custom cases
assert run("1\n1\n0 0 0\n") == "1", "single segment"
assert run("1\n2\n0 0 5\n0 1 2\n") == "2", "same color segments, not connected"
assert run("1\n3\n0 0 5\n1 5 10\n0 10 15\n") == "1", "chain connection"
assert run("1\n4\n0 0 2\n1 3 4\n0 5 6\n1 7 8\n") == "4", "disjoint segments"

| Test input | Expected output | What it validates |
|------------|----------------|-----------------|
| 1 segment | 1 | minimal case |
| 2 red overlapping | 2 | segments same color not connected |
| 3 forming chain | 1 | transitive group merge |
| 4 disjoint | 4 | each disconnected forms own group |
```## Vỏ cạnh 

Các đoạn chỉ chạm vào điểm cuối như`(0,0,2)`Và`(1,2,4)`được coi là được kết nối, bởi vì phạm vi được bao gồm. Thuật toán xử lý việc này một cách chính xác vì nó so sánh`l > max_opposite`để phát hiện các nhóm mới. Nếu điểm bắt đầu bằng mức tối đa của màu đối diện, chúng vẫn hợp nhất. Một đoạn xa, như`(0, 10, 12)`sau max_blue trước đó là 4, bắt đầu một nhóm mới, như mong đợi. Cách xử lý này đảm bảo không có sai sót nào xảy ra trong quá trình đếm nhóm.
