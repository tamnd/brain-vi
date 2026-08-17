---
title: "CF 102214H - Lịch trình"
description: "Bài toán yêu cầu chúng ta xem xét (n) bài học, trong đó bài học (i) chiếm khoảng thời gian ([li,ri]). Hai bài học tương thích nhau khi chúng không trùng nhau. Cho phép chạm vào một điểm cuối, vì vậy ([1,3]) và ([3,5]) tương thích. Chúng ta phải hủy đúng một buổi học."
date: "2026-08-18T00:17:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102214
codeforces_index: "H"
codeforces_contest_name: "\u041e\u0442\u043a\u0440\u044b\u0442\u043e\u0435 \u043b\u0438\u0447\u043d\u043e\u0435 \u043f\u0435\u0440\u0432\u0435\u043d\u0441\u0442\u0432\u043e \u0418\u041a\u0418\u0422 \u0421\u0424\u0423 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2015"
rating: 0
weight: 102214
solve_time_s: 98
verified: true
draft: false
---

[CF 102214H - Lịch trình](https://codeforces.com/problemset/problem/102214/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta xem xét (n) bài học, trong đó bài học (i) chiếm khoảng thời gian ([l_i,r_i]). Hai bài học tương thích nhau khi chúng không trùng nhau. Cho phép chạm vào một điểm cuối, vì vậy ([1,3]) và ([3,5]) tương thích. 

Chúng ta phải hủy đúng một buổi học. Mục tiêu là tìm ra mọi bài học mà việc hủy bỏ khiến tất cả các bài học còn lại theo cặp không bị chồng chéo. Đầu ra là số lượng bài học như vậy, theo sau là chỉ số của chúng theo thứ tự tăng dần. Vấn đề ban đầu của Codeforces được liệt kê là 31C, Lịch trình, với (n \le 5000), (l_i,r_i \le 10^6), giới hạn thời gian 2 giây và bộ nhớ 256 MB. 

Giới hạn (n \le 5000) là ràng buộc chính. Thuật toán (O(n^2)) thực hiện tối đa khoảng 25 triệu kiểm tra cặp, điều này hợp lý trong ngôn ngữ được biên dịch và vẫn có thể quản lý được trong Python được tối ưu hóa. Thuật toán (O(n^3)) sẽ yêu cầu khoảng (5000^3/2 = 62,5) tỷ so sánh cặp trong trường hợp xấu nhất, vượt xa giới hạn. Do đó, chúng ta cần tránh kiểm tra từng cặp riêng biệt cho từng bài học có thể bị hủy. 

Cách hữu ích nhất để suy nghĩ về vấn đề là xác định các cặp bài học thực sự xung đột với nhau. Nếu không có cặp xung đột nào, việc hủy bất kỳ bài học nào cũng có tác dụng. Nếu có các cặp xung đột, bài học chỉ có thể bị hủy nếu nó thuộc về mọi cặp xung đột. 

Trường hợp điểm cuối rất dễ bị xử lý sai. Đối với đầu vào```
2
1 3
3 5
```đầu ra đúng là```
2
1 2
```vì bài học chỉ chạm vào lúc 3. Thực hiện bất cẩn khi sử dụng`r >= l`vì điều kiện chồng chéo sẽ coi chúng là xung đột một cách không chính xác. 

Một trường hợp quan trọng khác là khi có nhiều xung đột nhưng lại có chung một bài học:```
3
1 10
2 3
4 5
```Đầu ra đúng là```
1
1
```Bài 1 và 2 xung đột, bài 1 và 3 xung đột. Loại bỏ bài 1 sẽ loại bỏ cả hai xung đột. Việc loại bỏ một trong hai bài học ngắn sẽ giữ nguyên xung đột còn lại. Một giải pháp chỉ tìm thấy một cặp xung đột và trả về cả hai điểm cuối của nó sẽ đưa ra bài học 1 và 2 không chính xác. 

Trường hợp ranh giới cuối cùng xảy ra khi lịch trình đã hợp lệ:```
3
1 2
2 4
4 7
```Đầu ra đúng là```
3
1 2 3
```Mọi cặp đều tương thích vì được phép chạm vào điểm cuối. Vì bài toán yêu cầu đúng một lần hủy nên mỗi bài học đều là một lựa chọn hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp xem xét mọi bài học có thể bị hủy bỏ. Đối với mỗi ứng viên, nó sẽ kiểm tra từng cặp trong số (n-1) bài học còn lại và kiểm tra xem các khoảng thời gian của chúng có trùng nhau hay không. Điều này đúng vì lịch trình kết quả có giá trị chính xác khi không có cặp nào còn lại giao nhau. Tuy nhiên, một ứng cử viên yêu cầu (O(n^2)) so sánh và có (n) ứng viên, cho (O(n^3)) thời gian. Với (n=5000), đây là thứ tự (62,5) tỷ cặp kiểm tra trong trường hợp xấu nhất. 

Cấu trúc của vấn đề cho phép chúng ta tránh lặp lại gần như toàn bộ công việc này. Thay vì hỏi, đối với mọi khả năng hủy bỏ, liệu một cặp có trùng nhau hay không, trước tiên chúng ta có thể xác định cặp nào trùng nhau trong lịch trình ban đầu. Giả sử có (m) cặp xung đột. Nếu bài học (i) bị hủy thì chính xác các cặp xung đột chứa (i) sẽ biến mất. Do đó, việc hủy bỏ hoạt động chính xác khi mỗi một trong (m) cặp xung đột đều chứa (i). 

Điều này đưa ra một tiêu chí đếm rất đơn giản. Với mỗi bài học, hãy đếm xem có bao nhiêu cặp xung đột trong đó. Nếu tổng số cặp xung đột là (m), thì bài học (i) là một phép hủy hợp lệ chính xác khi số lượng xung đột của nó là (m). 

Chúng ta có thể tìm thấy tất cả các cặp xung đột bằng cách kiểm tra từng cặp không có thứ tự một lần. Chỉ có (O(n^2)) cặp như vậy, vì vậy điều này phù hợp với ràng buộc. Chúng tôi thậm chí không cần phải lưu trữ các cặp. Chúng ta chỉ cần tổng số xung đột và trong mỗi bài học có bao nhiêu xung đột xảy ra. 

Hai cách tiếp cận có thể được so sánh như sau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^3)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n^2)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các khoảng thời gian của bài học và lưu trữ thời gian bắt đầu và kết thúc cùng với các chỉ số ban đầu của chúng. 
2. Tạo một mảng`conflicts`Ở đâu`conflicts[i]`đếm xem có bao nhiêu bài học khác chồng lên bài học (i). Cũng duy trì`total_conflicts`, tổng số cặp không có thứ tự chồng chéo. 
3. Kiểm tra từng cặp bài học (i<j). Chúng chồng lên nhau chính xác khi`l[i] < r[j] and l[j] < r[i]`. 

Sự so sánh nghiêm ngặt này là điều làm cho các khoảng thời gian chia sẻ trở thành điểm cuối tương thích. Khi cặp trùng nhau, tăng dần`total_conflicts`,`conflicts[i]`, Và`conflicts[j]`. 
4. Sau khi xử lý xong tất cả các cặp, hãy kiểm tra từng bài (i). Nếu như`conflicts[i] == total_conflicts`, thêm chỉ mục của nó vào câu trả lời. 

Lý do là việc hủy (i) sẽ loại bỏ chính xác các cặp xung đột có chứa (i). Mọi xung đột sẽ biến mất khi và chỉ khi mọi xung đột đều chứa (i). 
5. In số chỉ mục hợp lệ và sau đó là các chỉ số đó theo thứ tự tăng dần. Vì chúng ta kiểm tra các bài học từ chỉ mục 1 đến (n), nên các chỉ mục được thu thập đã có thứ tự bắt buộc. 

### Tại sao nó hoạt động 

Hãy xem xét tập hợp tất cả các cặp bài học xung đột nhau. Gọi kích thước của nó là (m). Việc hủy bài học (i) chỉ có thể ảnh hưởng đến những xung đột có chứa (i), vì vậy chính xác`conflicts[i]`các cặp xung đột biến mất. Lịch trình còn lại không có sự trùng lặp chính xác khi tất cả (m) cặp xung đột biến mất. Do đó bài học (i) là một câu trả lời hợp lệ chính xác khi`conflicts[i] = m`. 

Nếu (m=0), mỗi bài học đều có số lượng xung đột bằng 0, do đó mọi bài học đều được chấp nhận chính xác. Nếu (m>0), sự bình đẳng tương tự sẽ xác định chính xác các bài học có trong mọi xung đột. Do đó, thuật toán đưa ra chính xác tất cả các phép hủy hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    intervals = [tuple(map(int, input().split())) for _ in range(n)]

    conflicts = [0] * n
    total_conflicts = 0

    for i in range(n):
        li, ri = intervals[i]
        for j in range(i + 1, n):
            lj, rj = intervals[j]

            # The intervals overlap only if neither one ends
            # at or before the other one starts.
            if li < rj and lj < ri:
                total_conflicts += 1
                conflicts[i] += 1
                conflicts[j] += 1

    answer = [
        i + 1
        for i in range(n)
        if conflicts[i] == total_conflicts
    ]

    print(len(answer))
    print(*answer)

if __name__ == "__main__":
    solve()
```Danh sách khoảng lưu trữ dữ liệu đầu vào chính xác như đã cho, vì vậy chỉ mục bài học ban đầu chỉ đơn giản là vị trí dựa trên số 0 cộng với một. Không cần phải sắp xếp các khoảng, điều đó cũng có nghĩa là thứ tự đầu ra sẽ đến một cách tự nhiên. 

Các vòng lặp lồng nhau sử dụng`i + 1`như là điểm khởi đầu cho`j`, vì vậy mỗi cặp không có thứ tự được kiểm tra đúng một lần. Điều kiện chồng chéo sử dụng các bất đẳng thức nghiêm ngặt. Ví dụ,`[1,3]`Và`[3,7]`không thỏa mãn`1 < 7`Và`3 < 3`với nhau, do đó chúng được xử lý chính xác là không chồng chéo. 

Đối với mỗi cặp xung đột, cả hai điểm cuối của cặp đó đều nhận được một số lượng xung đột. Nếu một bài học có năm cặp xung đột khác nhau thì bộ đếm của nó sẽ trở thành năm. Tổng số bộ đếm cũng trở thành năm nếu đó là những xung đột duy nhất, do đó bài học đó là một sự hủy bỏ hợp lệ. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn. Trên thực tế, số cặp xung đột lớn nhất có thể chỉ là (\binom{5000}{2}=12,497,500), đủ nhỏ cho số học số nguyên thông thường. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên là```
3
3 10
20 30
1 3
```Ba bài học tương thích theo từng cặp. Bài học đầu tiên và thứ ba chạm vào thời điểm thứ 3, trong khi bài học thứ hai bắt đầu muộn hơn nhiều. 

| (i) | (j) | Khoảng thời gian (i) | Khoảng thời gian (j) | Chồng chéo? |`total_conflicts`| 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | [3,10] | [20,30] | Không | 0 | 
| 1 | 3 | [3,10] | [1,3] | Không | 0 | 
| 2 | 3 | [20,30] | [1,3] | Không | 0 | 

Cuối cùng,`total_conflicts = 0`và mỗi bài học đều có`conflicts[i] = 0`. 

| Bài học |`conflicts[i]`|`total_conflicts`| Có hiệu lực? | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | Có | 
| 2 | 0 | 0 | Có | 
| 3 | 0 | 0 | Có | 

Đầu ra là`3`theo sau là`1 2 3`. Điều này chứng tỏ trường hợp lịch trình ban đầu đã có hiệu lực. 

### Mẫu 2 

Mẫu thứ hai là```
4
3 10
20 30
1 3
1 39
```Bài 4 trùng với các bài khác. Ba bài học còn lại không trùng lặp với nhau. 

| (i) | (j) | Chồng chéo? |`conflicts[i]`|`conflicts[j]`| Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | Không | 0 | 0 | 0 | 
| 1 | 3 | Không | 0 | 0 | 0 | 
| 1 | 4 | Có | 1 | 1 | 1 | 
| 2 | 3 | Không | 0 | 0 | 1 | 
| 2 | 4 | Có | 1 | 2 | 2 | 
| 3 | 4 | Có | 1 | 3 | 3 | 

Trạng thái cuối cùng là 

| Bài học |`conflicts[i]`|`total_conflicts`| Có hiệu lực? | 
| --- | --- | --- | --- | 
| 1 | 1 | 3 | Không | 
| 2 | 1 | 3 | Không | 
| 3 | 1 | 3 | Không | 
| 4 | 3 | 3 | Có | 

Chỉ có bài 4 thuộc về cả 3 cặp xung đột nên việc loại bỏ nó sẽ giải quyết được mọi xung đột. Đầu ra là`1`theo sau là`4`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^2)) | Mỗi cặp bài học không có thứ tự sẽ được kiểm tra một lần. | 
| Không gian | (O(n)) | Các khoảng thời gian và một bộ đếm xung đột cho mỗi bài học được lưu trữ. | 

Với (n=5000), vòng lặp cặp kiểm tra tối đa 12.497.500 cặp. Đó là thang đo dự kiến ​​cho các ràng buộc nhất định và nhỏ hơn đáng kể so với khoảng 62,5 tỷ so sánh được yêu cầu bởi phương pháp brute-force (O(n^3)). Việc sử dụng bộ nhớ là tuyến tính theo số lượng bài học. 

## Trường hợp thử nghiệm```python
# helper: run solution logic on input string, return output string
import sys
import io

def solve_data(inp: str) -> str:
    data = inp.strip().split()
    it = iter(data)

    n = int(next(it))
    intervals = [(int(next(it)), int(next(it))) for _ in range(n)]

    conflicts = [0] * n
    total_conflicts = 0

    for i in range(n):
        li, ri = intervals[i]
        for j in range(i + 1, n):
            lj, rj = intervals[j]
            if li < rj and lj < ri:
                total_conflicts += 1
                conflicts[i] += 1
                conflicts[j] += 1

    answer = [
        str(i + 1)
        for i in range(n)
        if conflicts[i] == total_conflicts
    ]

    return f"{len(answer)}\n{' '.join(answer)}\n"

# Provided sample 1
assert solve_data(
    """3
3 10
20 30
1 3
"""
) == "3\n1 2 3\n", "sample 1"

# Provided sample 2
assert solve_data(
    """4
3 10
20 30
1 3
1 39
"""
) == "1\n4\n", "sample 2"

# Provided sample 3
assert solve_data(
    """3
1 5
2 6
3 7
"""
) == "0\n\n", "sample 3"

# Minimum size
assert solve_data(
    """1
5 10
"""
) == "1\n1\n", "single lesson"

# All lessons touch but never overlap
assert solve_data(
    """4
1 3
3 5
5 8
8 10
"""
) == "4\n1 2 3 4\n", "endpoint touching"

# One lesson is the common cause of every conflict
assert solve_data(
    """4
1 2
2 3
1 10
3 4
"""
) == "1\n3\n", "one common conflicting lesson"

# Maximum-size case, every interval is identical.
# After removing one interval, many conflicts still remain.
n = 5000
max_case = str(n) + "\n" + ("1 2\n" * n)
assert solve_data(max_case) == "0\n\n", "maximum size all-overlapping case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 5 10`|`1 / 1`| Kích thước tối thiểu và hủy bỏ chính xác một lần | 
| Bốn khoảng chạm vào điểm cuối |`4 / 1 2 3 4`| Sự bình đẳng của điểm cuối không được tính là trùng lặp | 
| Một khoảng chồng chéo mọi xung đột |`1 / 3`| Một câu trả lời hợp lệ có thể là điểm cuối chung của mọi xung đột | 
| 5000 khoảng giống hệt nhau |`0`| Kích thước đầu vào tối đa và bị từ chối khi một lần xóa không thể loại bỏ tất cả xung đột | 

## Vỏ cạnh 

Để chạm vào điểm cuối, hãy xem xét```
2
1 3
3 5
```Kiểm tra cặp đôi đánh giá`1 < 5`như đúng nhưng`3 < 3`là sai nên cặp này không được tính là xung đột. Tổng số xung đột bằng 0, cả hai bài học đều có số xung đột bằng 0 và đầu ra là```
2
1 2
```Đây là lý do tại sao sử dụng bất đẳng thức nghiêm ngặt thay vì`<=`là điều cần thiết. 

Đối với một bài học xung đột phổ biến, hãy xem xét```
3
1 10
2 3
4 5
```Các cặp`(1,2)`Và`(1,3)`xung đột, trong khi`(2,3)`không. Số lượng xung đột là`[2,1,1]`và tổng số xung đột là`2`. Chỉ có bài 1 có số lượng xung đột bằng tổng số nên kết quả là```
1
1
```Loại bỏ bài 1 sẽ loại bỏ cả hai xung đột cùng một lúc. 

Đối với một lịch trình đã hợp lệ, hãy xem xét```
3
1 2
2 4
4 7
```Mỗi cặp đều nằm hoàn toàn trước cặp tiếp theo hoặc chạm vào điểm cuối. Do đó tổng số xung đột là bằng không. Vì mỗi bài học đều không có xung đột nên mọi chỉ số đều thỏa mãn`conflicts[i] == total_conflicts`, sản xuất```
3
1 2 3
```Đối với biểu đồ xung đột dày đặc, hãy xem xét 5000 khoảng giống nhau`[1,2]`. Mọi cặp đều xung đột nên có 12.497.500 cặp xung đột. Mỗi bài học tham gia vào 4.999 bài học trong số đó. Từ`4999 != 12497500`, không có bài học nào có thể tự mình loại bỏ mọi xung đột và câu trả lời đúng là bằng 0. Thuật toán xử lý vấn đề này mà không cần xây dựng một tập hợp khổng lồ các cặp xung đột, bởi vì nó chỉ lưu trữ tổng số lượng và một số lượng sự cố cho mỗi bài học.
