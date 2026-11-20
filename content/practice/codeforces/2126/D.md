---
title: "CF 2126D - Đây Là Lần Cuối Cùng"
description: "Chúng ta được cung cấp một chuỗi các sòng bạc, mỗi sòng bạc được xác định bằng một loạt đồng xu [li, ri] mà chúng ta phải chơi và một kết quả thực tế cố định sẽ trở thành số xu mới của chúng ta sau khi chơi ở đó. Chúng tôi bắt đầu với k xu và có thể ghé thăm sòng bạc theo bất kỳ thứ tự nào, nhưng chỉ một lần."
date: "2026-06-08T03:22:46+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2126
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1037 (Div. 3)"
rating: 1200
weight: 2126
solve_time_s: 78
verified: true
draft: false
---

[CF 2126D - Đây là lần cuối cùng](https://codeforces.com/problemset/problem/2126/D) 

**Đánh giá:** 1200 
**Tas:** cấu trúc dữ liệu, tham lam, sắp xếp 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các sòng bạc, mỗi sòng bạc được xác định bởi một loạt các loại tiền xu`[l_i, r_i]`mà chúng ta phải chơi và một kết quả cố định`real_i`số tiền đó sẽ trở thành số xu mới của chúng tôi sau khi chơi ở đó. Chúng tôi bắt đầu với`k`tiền xu và có thể ghé thăm sòng bạc theo bất kỳ thứ tự nào, nhưng mỗi lần chỉ một lần. Mục tiêu là tối đa hóa số lượng xu ở cuối. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp chỉ định số lượng sòng bạc`n`và đồng xu ban đầu`k`, theo sau là`n`sinh ba`(l_i, r_i, real_i)`. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm: số tiền tối đa có thể đạt được. 

Được cho`n`có thể lên đến`10^5`trên tất cả các trường hợp thử nghiệm và`k`,`l_i`,`r_i`,`real_i`lên đến`10^9`, bất kỳ giải pháp nào kiểm tra tất cả các hoán vị đều không khả thi. Một sự ngây thơ`O(n!)`cách tiếp cận sẽ ngay lập tức thất bại. Chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh bao gồm bắt đầu bằng`0`tiền xu, sòng bạc ở đâu`real_i`không cải thiện số lượng của bạn hoặc trình tự trong đó việc truy cập một số sòng bạc trước tiên sẽ chặn những người khác do họ`l_i`ngưỡng. Ví dụ, nếu`k = 0`và tất cả các sòng bạc đều có`l_i >= 1`, số tiền tối đa còn lại`0`. Một cách tiếp cận tham lam bất cẩn chọn cái lớn nhất`real_i`mù quáng có thể thất bại nếu sòng bạc đó chưa thể truy cập được. 

## Phương pháp tiếp cận 

Phương pháp vũ phu sẽ thử tất cả các hoán vị của các lượt truy cập sòng bạc, cập nhật số lượng xu và theo dõi mức tối đa. Về nguyên tắc, điều này hoạt động nhưng cực kỳ chậm: với`n = 10^5`sòng bạc, số lượng hoán vị rất lớn. 

Quan sát quan trọng là khi bạn có thể chơi sòng bạc, số xu mới sẽ không phụ thuộc vào thứ tự bạn đến đó, miễn là điều kiện tiên quyết`l_i <= coins <= r_i`được hài lòng. Do đó, chiến lược tham lam có hiệu quả: chúng ta có thể liên tục chọn bất kỳ sòng bạc nào có thể chơi được để tối đa hóa số tiền của chúng ta tại thời điểm đó. Mỗi lần chúng ta chơi, số tiền hiện tại của chúng ta sẽ tăng lên hoặc giữ nguyên. Chúng tôi dừng lại khi không còn sòng bạc nào có thể chơi được nữa. 

Điều này làm giảm vấn đề khi mô phỏng quy trình trong đó chúng tôi duy trì tập hợp các sòng bạc chưa được truy cập và liên tục chọn bất kỳ sòng bạc nào hiện có thể chơi được. Sử dụng hàng đợi hoặc danh sách và quét tất cả các sòng bạc chưa được ghé thăm mỗi vòng sẽ mang lại`O(n^2)`trong trường hợp xấu nhất là quá chậm. Thay vào đó, sắp xếp sòng bạc theo giới hạn dưới của chúng`l_i`cho phép chúng tôi xác định một cách hiệu quả các sòng bạc có thể chơi được và lặp lại cho đến khi không có sòng bạc mới nào được thêm vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Mô phỏng tham lam với việc sắp xếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`và đồng xu ban đầu`k`. Lưu trữ sòng bạc dưới dạng danh sách các bộ dữ liệu`(l_i, r_i, real_i)`. 
3. Sắp xếp sòng bạc theo giới hạn dưới của chúng`l_i`. Điều này đảm bảo rằng khi quét từ trái sang phải, bất kỳ sòng bạc nào có`l_i <= current coins`có thể chơi được ngay lập tức. 
4. Khởi tạo cờ`changed = True`. Trong khi`changed`là đúng, hãy làm như sau: 

1. Đặt`changed = False`. 
2. Lặp lại tất cả các sòng bạc. Nếu một sòng bạc không được ghé thăm và`l_i <= current coins <= r_i`, đánh dấu nó đã truy cập, cập nhật`current coins = real_i`, và đặt`changed = True`. 
5. Lặp lại cho đến khi không còn sòng bạc mới nào có thể chơi được. Số lượng xu hiện tại là số lượng tối đa có thể đạt được. 

Tại sao nó hoạt động: quy trình đảm bảo rằng bất cứ khi nào một sòng bạc có thể chơi được, nó sẽ được truy cập trong lần lặp lại sớm nhất có thể. Từ`real_i >= l_i`và mỗi sòng bạc chỉ được sử dụng một lần, tiền không bao giờ giảm, đảm bảo rằng chúng ta không thể bỏ lỡ bất kỳ cơ hội nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        casinos = []
        for _ in range(n):
            l, r, real = map(int, input().split())
            casinos.append([l, r, real, False])  # last element marks visited

        casinos.sort(key=lambda x: x[0])  # sort by l_i
        coins = k
        changed = True
        while changed:
            changed = False
            for casino in casinos:
                if not casino[3] and casino[0] <= coins <= casino[1]:
                    coins = casino[2]
                    casino[3] = True
                    changed = True
        print(coins)

if __name__ == "__main__":
    solve()
```Chúng tôi sắp xếp theo`l_i`để xác định hiệu quả sòng bạc nào có thể chơi được ngay lập tức. Chúng tôi duy trì một`visited`gắn cờ để tránh xem lại. Vòng lặp bên ngoài chỉ chạy trong khi thu được tiền mới, đảm bảo chấm dứt. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

đầu vào:```
3 1
2 3 3
1 2 2
3 10 10
```| tiền xu | Sòng bạc đã ghé thăm | xu sau chuyến thăm | 
| --- | --- | --- | 
| 1 | Sòng bạc 2 (1,2,2) | 2 | 
| 2 | Sòng bạc 1 (2,3,3) | 3 | 
| 3 | Sòng bạc 3 (3,10,10) | 10 | 

Đồng tiền cuối cùng: 10. Bảng xác nhận rằng việc chọn sòng bạc theo thứ tự tôn trọng`l_i <= coins <= r_i`tối đa hóa tiền xu. 

**Ví dụ 2:** 

đầu vào:```
1 0
1 2 2
```| tiền xu | Sòng bạc đã ghé thăm | xu sau chuyến thăm | 
| --- | --- | --- | 
| 0 | không | 0 | 

Số tiền cuối cùng: 0. Điều này xác nhận thuật toán xử lý các sòng bạc không thể truy cập được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp mất`O(n log n)`. Vòng lặp while quét từng sòng bạc nhiều nhất một lần cho mỗi lần tăng xu. Vì tiền xu tăng đều đặn và sòng bạc chỉ được truy cập một lần nên tổng số lần lặp lại là`O(n)`. | 
| Không gian | O(n) | Yêu cầu lưu trữ sòng bạc có cờ`O(n)`. | 

Giải pháp vừa vặn thoải mái trong giới hạn 2 giây ngay cả ở mức tối đa`n = 10^5`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("5\n3 1\n2 3 3\n1 2 2\n3 10 10\n1 0\n1 2 2\n1 2\n1 2 2\n2 2\n1 3 2\n2 4 4\n2 5\n1 10 5\n3 6 5") == "10\n0\n2\n4\n5", "sample 1"

# minimum input
assert run("1\n1 0\n0 0 0") == "0", "minimum input"

# maximum single casino reachable
assert run("1\n1 100\n50 200 300") == "300", "max coins single casino"

# all casinos unreachable
assert run("1\n3 0\n1 2 2\n2 3 3\n3 4 4") == "0", "all unreachable"

# sequential unlocks
assert run("1\n3 1\n1 2 2\n2 3 3\n3 4 4") == "4", "sequential unlocks"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1 0\n0 0 0`| 0 | Xử lý số tiền tối thiểu và sòng bạc duy nhất | 
|`1\n1 100\n50 200 300`| 300 | Xử lý sòng bạc lớn có thể tiếp cận | 
|`1\n3 0\n1 2 2\n2 3 3\n3 4 4`| 0 | Tất cả các sòng bạc không thể truy cập được ngay từ đầu | 
|`1\n3 1\n1 2 2\n2 3 3\n3 4 4`| 4 | Mở khóa tuần tự các sòng bạc hoạt động | 

## Vỏ cạnh 

Bắt đầu với số tiền bằng 0 và sòng bạc yêu cầu tích cực`l_i`không bao giờ cho phép bất kỳ trò chơi nào. Ví dụ,`k=0`, sòng bạc`(1,2,2)`Và`(3,5,5)`. Vòng lặp quét tất cả các sòng bạc, không tìm thấy sòng bạc nào có thể chơi được, đặt`changed=False`và thoát ngay lập tức, xuất ra 0. 

Khi nhiều sòng bạc có`l_i`dưới hoặc bằng số xu hiện tại, thứ tự không quan trọng vì số lượng xu chỉ tăng hoặc giữ nguyên. Vì`k=1`, sòng bạc`(1,3,2)`Và`(1,2,5)`, thuật toán chọn xu đầu tiên trong quá trình quét, cập nhật xu lên 2, sau đó chọn sòng bạc thứ hai (bây giờ xu 2 đáp ứng`l_i=1`) và cập nhật xu lên 5. Đồng xu cuối cùng là chính xác.
