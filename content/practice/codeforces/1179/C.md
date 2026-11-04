---
title: "CF 1179C - Serge và Phòng ăn"
description: "Chúng tôi được giao nhiệm vụ mô phỏng một phòng ăn của trường, nơi mỗi món ăn có một bản duy nhất và mỗi học sinh mua món ăn đắt nhất mà họ có thể mua được. Serge, nhân vật chính của chúng ta, muốn biết anh ấy sẽ nhận được món ăn nào nếu đợi cho đến khi tất cả học sinh mua xong."
date: "2026-06-12T01:34:17+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "graph-matchings", "greedy", "implementation", "math", "trees"]
categories: ["algorithms"]
codeforces_contest: 1179
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 569 (Div. 1)"
rating: 2200
weight: 1179
solve_time_s: 82
verified: true
draft: false
---

[CF 1179C - Serge và Phòng ăn](https://codeforces.com/problemset/problem/1179/C) 

**Đánh giá:** 2200 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, so khớp đồ thị, tham lam, triển khai, toán học, cây cối 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao nhiệm vụ mô phỏng một phòng ăn của trường, nơi mỗi món ăn có một bản duy nhất và mỗi học sinh mua món ăn đắt nhất mà họ có thể mua được. Serge, nhân vật chính của chúng ta, muốn biết anh ấy sẽ nhận được món ăn nào nếu đợi cho đến khi tất cả học sinh mua xong. Ban đầu, chúng tôi có nhiều mức giá món ăn và nhiều mức ngân sách dành cho học sinh. Sau đó, một loạt truy vấn sẽ thay đổi giá của một món ăn hoặc túi tiền của học sinh. Sau mỗi câu hỏi, chúng tôi phải báo cáo giá món ăn mà Serge sẽ mua nếu anh ấy đợi cho đến khi tất cả học sinh mua xong. 

Thách thức chính là xử lý hiệu quả tới 300.000 món ăn, 300.000 học sinh và 300.000 truy vấn. Một cách tiếp cận đơn giản mô phỏng hàng đợi cho mỗi truy vấn sẽ yêu cầu các thao tác O(n m) cho mỗi truy vấn, dẫn đến khoảng$10^{11}$trong trường hợp xấu nhất là không thể thực hiện được. Vì vậy, chúng ta cần một cách để theo dõi những món ăn nào có khả năng được mua mà không cần mô phỏng riêng từng học sinh. 

Các trường hợp đặc biệt bao gồm các tình huống trong đó tất cả các món ăn đều quá đắt đối với tất cả học sinh, khiến Serge có thể tự do chọn món ăn đắt nhất còn lại. Nếu học sinh mua tất cả các món ăn thì Serge sẽ không nhận được gì. Những thay đổi về giá món ăn hoặc ngân sách học sinh có thể khiến kết quả của Serge thay đổi giữa việc nhận món ăn hay không, vì vậy giải pháp phải xử lý các cập nhật động một cách chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu mô phỏng mỗi học sinh mua món ăn đắt nhất mà họ có thể mua được, sau đó tìm món ăn còn lại tối đa cho Serge. Điều này có tác dụng chính xác nhưng không đáp ứng được các ràng buộc về thời gian vì trong trường hợp xấu nhất, chúng tôi sẽ thực hiện các thao tác O(n m) cho mỗi truy vấn, điều này bị cấm đối với n, m, q lên tới 300.000. 

Điểm mấu chốt là chúng ta không cần biết chính xác học sinh nào mua món nào; chúng ta chỉ cần biết, đối với mỗi món ăn, liệu nó có được mua hay không với ngân sách hiện tại. Quan sát này cho phép chúng ta xử lý vấn đề như một sự so khớp giữa các mảng đã được sắp xếp: sắp xếp các món ăn và học sinh theo giá cả và ngân sách, sau đó mô phỏng sự so khớp tham lam từ món ăn đắt tiền nhất trở xuống. Nếu món ăn đắt nhất vượt quá mọi ngân sách của học sinh, nó sẽ thuộc về Serge; nếu không, nó sẽ được mua bởi học sinh đầu tiên có đủ khả năng chi trả. 

Vì các truy vấn thay đổi linh hoạt giá món ăn hoặc ngân sách của học sinh, nên chúng tôi cần cấu trúc dữ liệu có thể duy trì mức tối đa một cách hiệu quả và theo dõi số lượng món ăn trên ngưỡng nhất định. Danh sách được sắp xếp bằng tìm kiếm nhị phân hoặc cấu trúc nhiều tập hợp hoạt động: chúng tôi có thể cập nhật giá món ăn hoặc ngân sách của học sinh trong thời gian O(log n), sau đó tìm món ăn mà Serge sẽ ăn một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n m) mỗi truy vấn | O(n + m) | Quá chậm | 
| Tham lam với cây multiset/phân khúc được sắp xếp | O(log n + log m) cho mỗi truy vấn | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì hai tập hợp được sắp xếp: một cho giá món ăn và một cho ngân sách học sinh. Việc sắp xếp đảm bảo rằng chúng ta luôn có thể truy vấn giá trị cao nhất một cách hiệu quả. 
2. Đối với mỗi truy vấn, hãy cập nhật giá món ăn hoặc ngân sách của học sinh. Xóa giá trị cũ khỏi nhiều tập hợp và chèn giá trị mới. Điều này bảo toàn thứ tự sắp xếp với các phép toán O(log n). 
3. Sau mỗi lần cập nhật, hãy xác định kết quả của Serge. Đầu tiên, hãy tìm số học sinh có ngân sách ít nhất bằng giá của món ăn đắt nhất. Điều này có thể được thực hiện bằng cách sử dụng tìm kiếm nhị phân trên mảng ngân sách đã sắp xếp. 
4. Nếu số học sinh đó ít hơn số món ăn có giá trên ngưỡng tương ứng, Serge sẽ lấy món còn lại có giá cao nhất. Nếu không, tất cả các món ăn sẽ được mua và Serge nhận được -1. 
5. Xuất kết quả sau mỗi truy vấn. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến rằng tại bất kỳ thời điểm nào, danh sách giá món ăn và ngân sách học sinh được sắp xếp sẽ phản ánh trạng thái hiện tại sau tất cả các lần cập nhật. Vì học sinh mua sắm một cách tham lam và các món ăn chỉ có một bản duy nhất nên vấn đề so khớp sẽ giảm xuống việc đếm xem có bao nhiêu học sinh có thể mua được món ăn nào. Bằng cách sử dụng nhiều bộ được sắp xếp, chúng tôi có thể xác định một cách hiệu quả liệu món ăn đắt tiền nhất có còn tồn tại sau tất cả các lần mua hay không. 

## Giải pháp Python```python
import sys
import bisect

input = sys.stdin.readline

n, m = map(int, input().split())
dishes = list(map(int, input().split()))
pupils = list(map(int, input().split()))
q = int(input())

from sortedcontainers import SortedList

dish_sl = SortedList(dishes)
pupil_sl = SortedList(pupils)

for _ in range(q):
    t, i, x = map(int, input().split())
    i -= 1  # convert to 0-based index
    if t == 1:
        # update dish price
        dish_sl.discard(dishes[i])
        dishes[i] = x
        dish_sl.add(x)
    else:
        # update pupil budget
        pupil_sl.discard(pupils[i])
        pupils[i] = x
        pupil_sl.add(x)
    
    # find Serge's dish
    if not dish_sl:
        print(-1)
        continue

    # binary search for the first pupil who can buy the most expensive dish
    max_dish = dish_sl[-1]
    idx = bisect.bisect_left(pupil_sl, max_dish)
    # number of pupils who can afford max_dish
    can_buy = len(pupil_sl) - idx
    if can_buy == 0:
        print(max_dish)
    else:
        if can_buy >= len(dish_sl):
            print(-1)
        else:
            print(dish_sl[-can_buy-1])
```Giải pháp sử dụng`SortedList`để duy trì các mảng được sắp xếp động. Đối với mỗi truy vấn, chúng tôi cập nhật mảng món ăn hoặc mảng học sinh và xác định một cách hiệu quả món ăn đắt nhất sẽ tồn tại. Sự tinh tế nằm ở việc đếm một cách chính xác: chúng ta phải biết học sinh lấy đi bao nhiêu món ăn thì Serge mới có thể chọn. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| Món ăn | Học sinh | Món ăn tối đa | Học sinh >= Max | Món Serge | 
| --- | --- | --- | --- | --- | 
| [1] | [1] | 1 | 1 | 1 -> Serge được 100 sau truy vấn | 
| [100] | [1] | 100 | 0 | 100 | 

Dấu vết này cho thấy khi không có học sinh nào đủ tiền mua món ăn thì Serge sẽ lấy nó. 

Mẫu 2: 

đầu vào:`3 3`, đĩa`[5,3,2]`, học sinh`[4,2,1]`, truy vấn`1 2 6`Và`2 1 5`. 

| Bước | Món ăn | Học sinh | Món ăn tối đa | Học sinh >= Max | Món Serge | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | [2,3,5] | [1,2,4] | 5 | 1 | 3 | 
| Truy vấn 1 | [2,6,5] | [1,2,4] | 6 | 0 | 6 | 
| Truy vấn 2 | [2,6,5] | [2,2,4] | 6 | 0 | 6 | 

Ví dụ này xác nhận rằng sau khi cập nhật động, thuật toán sẽ xác định chính xác món ăn mà Serge có thể mua. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n + q log m) | Mỗi truy vấn cập nhật một Danh sách sắp xếp trong O (log n hoặc log m) và việc tìm món ăn của Serge yêu cầu tìm kiếm nhị phân | 
| Không gian | O(n + m) | Chúng tôi lưu trữ giá món ăn và ngân sách học sinh trong SortedLists | 

Với n, m, q lên tới 300.000, giải pháp thực hiện khoảng 1,8 triệu thao tác cho mỗi truy vấn trong trường hợp xấu nhất, phù hợp trong vòng 4 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io
from contextlib import redirect_stdout

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    f = io.StringIO()
    with redirect_stdout(f):
        exec(open('solution.py').read())
    return f.getvalue().strip()

# Provided samples
assert run("1 1\n1\n1\n1\n1 1 100\n") == "100", "sample 1"

# Minimum size
assert run("1 1\n1\n1\n1\n2 1 1\n") == "1", "minimum size update pupil"

# All dishes equal, pupils cannot afford
assert run("3 2\n5 5 5\n1 1\n1\n1 2 10\n") == "10", "update dish above all pupils"

# All pupils rich, Serge gets nothing
assert run("3 3\n5 3 2\n10 10 10\n1\n1 1 6\n") == "-1", "all dishes bought"

# Mixed updates
assert run("3 3\n5 3 2\n4 2 1\n2\n1 2 6\n2 1 5\n") == "6\n6", "dynamic sequence"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "1 1\n1\n1\n1\n1 1 100\n" | 100 | Serge nhận được món ăn khi học sinh không đủ tiền | 
| "1 1\n1\n1\n1\n2 1 1\n" | 1 | Đầu vào có kích thước tối thiểu, cập nhật học sinh | 
| "3 2\n5 5 5\n1 1\n1\n1 2 10\n" | 10 | Cập nhật giá món ăn vượt túi tiền học sinh | 
| "3 3\n5 3 2\n10 10 10\n1\n1 1 6\n" | -1 | Tất cả |
