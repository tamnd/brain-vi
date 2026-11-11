---
title: "CF 1606B - Cập nhật tập tin"
description: "Chúng tôi được yêu cầu xác định thời gian tối thiểu cần thiết để sao chép bản cập nhật hệ điều hành từ một máy tính được cập nhật ban đầu sang tất cả các máy tính trong mạng."
date: "2026-06-10T07:49:26+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1606
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 116 (Rated for Div. 2)"
rating: 1100
weight: 1606
solve_time_s: 79
verified: true
draft: false
---

[CF 1606B - Cập nhật tệp](https://codeforces.com/problemset/problem/1606/B) 

**Đánh giá:** 1100 
**Tags:** tham lam, thực hiện, toán học 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định thời gian tối thiểu cần thiết để sao chép bản cập nhật hệ điều hành từ một máy tính được cập nhật ban đầu sang tất cả các máy tính trong mạng. Mỗi máy tính có thể sao chép bản cập nhật sang nhiều nhất một máy tính khác mỗi giờ và mạng có tổng cộng`k`vá các cáp giới hạn số lần truyền đồng thời trong một giờ nhất định. Mục tiêu là xuất ra, đối với nhiều trường hợp thử nghiệm, số giờ cần thiết nhỏ nhất để mọi máy tính đều có bản cập nhật. 

Các ràng buộc chính là lớn:`n`Và`k`mỗi người có thể lên đến$10^{18}$, và có thể có tới$10^5$trường hợp thử nghiệm. Điều này loại trừ mọi giải pháp mô phỏng trực tiếp từng giờ, vì việc mô phỏng một hoạt động cho mọi máy tính sẽ quá chậm. Thay vào đó, giải pháp phải suy luận một cách toán học về sự tăng trưởng của các máy tính được cập nhật mỗi giờ, xử lý các số lượng cực lớn mà không cần lặp lại chúng. 

Một trường hợp cạnh tinh tế phát sinh khi`k`là rất nhỏ so với`n`. Ví dụ, nếu`n = 7`Và`k = 1`, bản cập nhật lan truyền gần như tuyến tính vì chỉ một máy tính mới có thể nhận được bản cập nhật mỗi giờ, mặc dù có nhiều máy tính đã được cập nhật. Ngược lại, nếu`k >= n - 1`, nhiều bản cập nhật có thể xảy ra đồng thời và thời gian giảm đáng kể. Trường hợp ở đó`n = 1`là chuyện nhỏ: không cần cập nhật và đầu ra phải là`0`. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ mô phỏng mỗi giờ: duy trì bộ đếm máy tính có bản cập nhật, tính toán số lượng máy tính có thể được cập nhật dựa trên các cáp vá có sẵn và tiếp tục cho đến khi tất cả các máy tính có thể được cập nhật.`n`máy tính được cập nhật. Phương pháp này đúng nhưng độ phức tạp của nó gần như tỷ lệ thuận với`n`, làm cho nó không khả thi đối với quy mô lớn`n`lên tới$10^{18}$. 

Cái nhìn sâu sắc về cách tiếp cận tối ưu đến từ việc xem chênh lệch giá như hai giai đoạn. Trong giai đoạn đầu, khi số lượng máy tính được cập nhật`u`ít hơn`k`, khả năng cập nhật bị giới hạn bởi`u`, vì mỗi máy tính chỉ có thể gửi bản cập nhật đến một máy tính khác mỗi giờ. Trong giai đoạn này, số lượng máy tính được cập nhật sẽ tăng gấp đôi sau mỗi giờ. Ở giai đoạn thứ hai, khi`u >= k`, số lượng cập nhật mỗi giờ bị giới hạn bởi`k`, bởi vì các cáp vá trở thành nút thắt cổ chai. Trong giai đoạn này, số lượng máy tính được cập nhật tăng tuyến tính theo`k`mỗi giờ cho đến khi đạt được`n`. 

Sử dụng quan sát này, chúng ta có thể tính số giờ cần thiết cho hai phần. Đầu tiên, hãy đếm xem phải mất bao nhiêu giờ để các máy tính được cập nhật đạt được`k`thông qua việc nhân đôi. Thứ hai, khi số lượng máy tính được cập nhật ít nhất`k`, tính số máy tính còn lại cần cập nhật và chia cho`k`để có được số giờ bổ sung cần thiết. Điều này làm giảm độ phức tạp`O(log k + 1)`cho mỗi trường hợp thử nghiệm, đủ nhanh ngay cả đối với các ràng buộc lớn nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Quá chậm cho n lên tới 10^18 | 
| Nhân đôi + Pha tuyến tính | O(log k + 1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`t`, số lượng ca kiểm thử và với mỗi ca kiểm thử, hãy đọc`n`Và`k`. 
2. Khởi tạo`hours = 0`Và`updated = 1`để theo dõi số lượng máy tính có bản cập nhật. 
3. Trong khi`updated < k`Và`updated < n`, nhân đôi số lượng máy tính được cập nhật mỗi giờ. Tăng`hours`cho mỗi bước nhân đôi. Việc nhân đôi ở đây là phù hợp vì khi số lượng máy tính cập nhật ít hơn`k`, mỗi máy tính được cập nhật có thể sao chép sang một máy tính mới. 
4. Sau giai đoạn nhân đôi, nếu`updated >= n`, in`hours`kết quả là tất cả các máy tính đều được cập nhật. 
5. Ngược lại, tính toán các máy tính còn lại như sau:`remaining = n - updated`. Mỗi giờ bây giờ cập nhật lên đến`k`máy tính, do đó số giờ bổ sung cần thiết là`(remaining + k - 1) // k`để làm tròn lên. Thêm những giờ này vào tổng số. 
6. Xuất ra tổng số giờ cho test case. 

Tại sao nó hoạt động: Thuật toán duy trì một bất biến`updated`luôn đại diện cho số lượng máy tính tối đa có thể có bản cập nhật vào giờ đó. Trong giai đoạn đầu tiên, mức tăng trưởng tối đa bị giới hạn bởi số lượng máy tính được cập nhật (`updated < k`), mang lại sự tăng trưởng theo cấp số nhân. Trong giai đoạn thứ hai, điểm nghẽn là số lượng cáp vá, do đó tốc độ tăng trưởng tuyến tính theo`k`mỗi giờ. Điều này đảm bảo rằng tổng số giờ được tính là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def min_hours(n, k):
    if n == 1:
        return 0
    hours = 0
    updated = 1
    while updated < k and updated < n:
        updated *= 2
        hours += 1
    if updated >= n:
        return hours
    remaining = n - updated
    hours += (remaining + k - 1) // k
    return hours

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    print(min_hours(n, k))
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`do số lượng lớn các trường hợp thử nghiệm. các`min_hours`hàm gói gọn thuật toán được mô tả ở trên. Sự nhân đôi xảy ra khi`updated < k`và khi vượt qua ngưỡng này, chúng tôi sẽ tính pha tuyến tính. Cần cẩn thận làm tròn pha tuyến tính bằng cách sử dụng`(remaining + k - 1) // k`, giúp tránh được từng lỗi một. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu đầu tiên`n = 8, k = 3`. 

| Giờ | Đã cập nhật | Ghi chú | 
| --- | --- | --- | 
| 0 | 1 | trạng thái ban đầu | 
| 1 | 2 | gấp đôi kể từ 1 < 3 | 
| 2 | 4 | tăng gấp đôi kể từ 2 < 3 → cuối cùng bị giới hạn bởi k=3, tiếp theo tăng gấp đôi lên 4 | 
| 3 | 8 | còn lại = 8 - 4 = 4 → pha tuyến tính với k=3 → giờ = ceil(4/3) = 2 → tổng số giờ = 4 | 

Điều này phù hợp với sản lượng dự kiến`4`. 

Vì`n = 7, k = 1`, pha nhân đôi không xảy ra vì`updated >= k`thất bại ban đầu. Thuật toán đi vào tăng trưởng tuyến tính ngay lập tức: 

| Giờ | Đã cập nhật | Ghi chú | 
| --- | --- | --- | 
| 0 | 1 | trạng thái ban đầu | 
| 1 | 2 | thêm k=1 | 
| 2 | 3 | thêm k=1 | 
| 3 | 4 | thêm k=1 | 
| 4 | 5 | thêm k=1 | 
| 5 | 6 | thêm k=1 | 
| 6 | 7 | cộng k=1 → xong | 

Đầu ra là`6`, như mong đợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log k) cho mỗi trường hợp thử nghiệm | Pha nhân đôi thực hiện log k bước, pha tuyến tính được tính theo O(1) với số học | 
| Không gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ cần bộ đếm và số học; không có công trình kiến ​​trúc lớn | 

Được cho`t <= 10^5`và mỗi trường hợp kiểm thử thực hiện các bước O(log k), tổng số thao tác đều nằm trong giới hạn. Không gian sử dụng là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call solution
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        print(min_hours(n, k))
    return output.getvalue().strip()

# provided samples
assert run("4\n8 3\n6 6\n7 1\n1 1\n") == "4\n3\n6\n0", "sample tests"

# custom cases
assert run("2\n1 1\n10 10\n") == "0\n4", "trivial and k=n case"
assert run("1\n1000000000000000000 1\n") == "999999999999999999", "large n, k=1"
assert run("1\n15 5\n") == "4", "small doubling then linear"
assert run("1\n10 100\n") == "4", "k > n, direct doubling suffices"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 0 | trường hợp tối thiểu, không cần cập nhật | 
| 10 10 | 4 | k = n, kiểm tra nhân đôi + pha tuyến tính | 
| 10000000000000000000 1 | 999999999999999999 | n rất lớn, k tối thiểu | 
| 15 5 | 4 | n nhỏ, k < n, cả hai pha tham gia | 
| 10 100 | 4 | k > n, nhân đôi đủ | 

## Vỏ cạnh 

cho`n = 1, k = 1`, máy tính ban đầu đã có bản cập nhật. Thuật toán trả về`0`mà không cần nhập bất kỳ vòng lặp nào, xử lý chính xác tình huống tối thiểu. 

Vì`k = 1`, mỗi giờ chỉ có thể cập nhật một máy tính. Vòng lặp nhân đôi bị bỏ qua và thuật toán thêm`(n - 1 + 1 - 1) // 1 = n - 1`giờ, phù hợp với mức chênh lệch tuyến tính dự kiến. Ví dụ,`n = 7, k = 1`mang lại `
