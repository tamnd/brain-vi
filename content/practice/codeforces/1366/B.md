---
title: "CF 1366B - Trộn bài"
description: "Chúng ta có một mảng có kích thước $n$ được khởi tạo với tất cả các số 0 ngoại trừ một số 0 duy nhất ở vị trí $x$. Mảng được lập chỉ mục 1. Sau đó chúng ta có các phép toán $m$, mỗi phép toán được xác định bởi một phạm vi $[li, ri]$. Trong mỗi thao tác, chúng ta có thể hoán đổi hai phần tử bất kỳ trong phạm vi đó."
date: "2026-06-11T12:02:43+07:00"
tags: ["codeforces", "competitive-programming", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1366
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 89 (Rated for Div. 2)"
rating: 1300
weight: 1366
solve_time_s: 110
verified: true
draft: false
---

[CF 1366B - Trộn bài](https://codeforces.com/problemset/problem/1366/B) 

**Đánh giá:** 1300 
**Tags:** toán, hai con trỏ 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng kích thước$n$được khởi tạo với tất cả các số 0 ngoại trừ một số 0 ở vị trí$x$. Mảng được lập chỉ mục 1. Sau đó chúng tôi có$m$hoạt động, mỗi hoạt động được xác định bởi một phạm vi$[l_i, r_i]$. Trong mỗi thao tác, chúng ta có thể hoán đổi hai phần tử bất kỳ trong phạm vi đó. Nhiệm vụ là xác định, sau khi áp dụng các thao tác này theo bất kỳ thứ tự nào chúng ta chọn, có bao nhiêu vị trí trong mảng có thể chứa số 1. 

Mặc dù kích thước mảng$n$có thể rất lớn, lên tới$10^9$, số lượng hoạt động$m$nhỏ, nhiều nhất là 100. Điều này cho thấy rằng chúng ta không thể biểu diễn mảng một cách rõ ràng. Thay vào đó, chúng ta phải suy luận về các phạm vi và chuyển động của số 1 qua các phạm vi này. 

Một điểm tinh tế quan trọng là nhiều giao dịch hoán đổi trong các phạm vi chồng chéo có thể truyền bá hiệu quả số 1 đến bất kỳ vị trí nào được kết nối bằng sự kết hợp của các phạm vi. Ví dụ: nếu số 1 ban đầu ở vị trí 4 và thao tác đầu tiên bao gồm [1, 6] thì số 1 có thể đạt tới tất cả các vị trí từ 1 đến 6 ngay lập tức. Nếu một thao tác sau bao gồm [2, 3], nó sẽ không mở rộng phạm vi tiếp cận hơn nữa, nhưng nếu một thao tác khác bao gồm [5, 7] thì các vị trí có thể tiếp cận sẽ mở rộng đến [1, 7]. 

Các trường hợp biên không rõ ràng bao gồm: trường hợp 1 bắt đầu từ ranh giới (vị trí 1 hoặc$n$), các hoạt động đơn lẻ ([3,3]) và các hoạt động không trùng lặp với bất kỳ vị trí có thể tiếp cận hiện tại nào. Một giải pháp ngây thơ cố gắng mô phỏng tất cả các giao dịch hoán đổi có thể xảy ra sẽ thất bại vì$n$là quá lớn. Ví dụ, với$n = 10^9$, việc lưu trữ mảng hoặc lặp qua nó là không thể thực hiện được. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng trực tiếp các giao dịch hoán đổi bằng cách duy trì mảng. Đối với mỗi thao tác, chúng tôi sẽ thử tất cả các cặp hoán đổi trong phạm vi, kiểm tra xem cặp 1 có di chuyển hay không. Ngay cả khi chúng tôi chỉ theo dõi vị trí của số 1, việc thử mọi hoán đổi trong mọi phạm vi sẽ nhanh chóng trở nên khó khăn. Vì$m = 100$hoạt động với phạm vi lên đến kích thước$n$, cách tiếp cận này là không thể vì kích thước rất lớn của mảng. 

Thông tin chi tiết quan trọng là theo dõi **phạm vi vị trí liền kề nơi số 1 có thể xuất hiện** thay vì chính mảng đó. Ban đầu, phạm vi chỉ là [x, x]. Đối với mỗi thao tác, nếu phạm vi của nó trùng với phạm vi hiện tại của phạm vi 1, chúng ta có thể hợp nhất chúng: số 1 hiện có thể xuất hiện ở bất kỳ đâu trong sự kết hợp của phạm vi hiện tại và phạm vi của thao tác. Mặt khác, thao tác không ảnh hưởng đến các vị trí có thể có của số 1. Sau khi xử lý tất cả các thao tác, câu trả lời đơn giản là độ dài của phạm vi cuối cùng. 

Quan sát này làm giảm vấn đề thành việc theo dõi liên kết khoảng thời gian đơn giản, hiệu quả vì$m$nhỏ và$n$không cần phải được trình bày một cách rõ ràng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m * n) | O(n) | Quá chậm | 
| Tối ưu (theo dõi khoảng thời gian) | O(m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`left`Và`right`ĐẾN`x`. Các biến này đại diện cho phạm vi bao gồm hiện tại nơi số 1 có thể xuất hiện. 
2. Lặp lại từng thao tác được xác định bởi`[l_i, r_i]`. 
3. Nếu phạm vi hoạt động`[l_i, r_i]`chồng lên phạm vi hiện tại`[left, right]`(tức là,`l_i <= right`Và`r_i >= left`), hãy cập nhật phạm vi để bao gồm thao tác:`left = min(left, l_i)`Và`right = max(right, r_i)`. 
4. Sau khi xử lý tất cả các thao tác, số chỉ mục mà số 1 có thể kết thúc là`right - left + 1`. 
5. Lặp lại cho tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: Bất biến là vậy`left`Và`right`luôn bao gồm tất cả các vị trí mà số 1 có thể tiếp cận thông qua các hoạt động được xem xét cho đến nay. Bất cứ khi nào một hoạt động chồng lên phạm vi này, phạm vi có thể truy cập sẽ mở rộng để bao gồm nó. Không có thao tác nào ngoài phạm vi hiện tại có thể ảnh hưởng đến 1, vì vậy việc bỏ qua các thao tác không chồng chéo là đúng. Độ dài phạm vi cuối cùng chính xác là số lượng vị trí có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, x, m = map(int, input().split())
    left = right = x
    for _ in range(m):
        l, r = map(int, input().split())
        if l <= right and r >= left:
            left = min(left, l)
            right = max(right, r)
    print(right - left + 1)
```Giải pháp đọc số lượng trường hợp thử nghiệm và lặp lại chúng. Đối với mỗi trường hợp thử nghiệm, nó khởi tạo phạm vi có thể truy cập thành`[x, x]`. Mỗi thao tác đều được kiểm tra sự chồng chéo và phạm vi được cập nhật tương ứng. Cuối cùng, độ dài của phạm vi được in. Mã sử ​​dụng lập chỉ mục bao gồm và xử lý các phạm vi phần tử đơn một cách chính xác vì việc tính toán phạm vi`right - left + 1`làm việc cho`left = right`. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

đầu vào:```
6 4 3
1 6
2 3
5 5
```| Bước | Hoạt động | trái | đúng | 
| --- | --- | --- | --- | 
| Ban đầu | - | 4 | 4 | 
| 1 | [1,6] | 1 | 6 | 
| 2 | [2,3] | 1 | 6 | 
| 3 | [5,5] | 1 | 6 | 

Đầu ra:`6`Số 1 có thể đạt tới tất cả các vị trí từ 1 đến 6. 

**Ví dụ 2:** 

đầu vào:```
4 1 2
2 4
1 2
```| Bước | Hoạt động | trái | đúng | 
| --- | --- | --- | --- | 
| Ban đầu | - | 1 | 1 | 
| 1 | [2,4] | 1 | 1 | 
| 2 | [1,2] | 1 | 2 | 

Đầu ra:`2`Số 1 chỉ có thể đạt đến vị trí 1 và 2; thao tác đầu tiên không mở rộng phạm vi vì nó không chồng lên vị trí ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t*m) | Mỗi thao tác được xử lý một lần cho mỗi trường hợp thử nghiệm, với$m \le 100$| 
| Không gian | O(1) | Chỉ có hai số nguyên được theo dõi cho mỗi trường hợp thử nghiệm, bất kể$n$| 

Được cho$m \le 100$Và$t \le 100$, tổng số thao tác tối đa là 10.000, trong vòng 1 giây. Bộ nhớ không đáng kể vì chúng ta không lưu trữ mảng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n, x, m = map(int, input().split())
        left = right = x
        for _ in range(m):
            l, r = map(int, input().split())
            if l <= right and r >= left:
                left = min(left, l)
                right = max(right, r)
        print(right - left + 1)
    return output.getvalue().strip()

# Provided samples
assert run("3\n6 4 3\n1 6\n2 3\n5 5\n4 1 2\n2 4\n1 2\n3 3 2\n2 3\n1 2") == "6\n2\n3", "sample"

# Custom test cases
assert run("1\n1 1 1\n1 1") == "1", "single element array"
assert run("1\n10 5 2\n1 4\n7 10") == "1", "operations do not connect"
assert run("1\n10 5 3\n3 6\n5 8\n1 2") == "6", "overlapping ranges expand reach"
assert run("1\n5 3 0") == "1", "no operations"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 / 1 1 | 1 | Mảng một phần tử | 
| 10 5 2/1 4/7 10 | 1 | Các hoạt động không trùng lặp ban đầu 1 | 
| 10 5 3 / 3 6 / 5 8 / 1 2 | 6 | Hoạt động chồng chéo mở rộng phạm vi có thể tiếp cận | 
| 5 3 0 | 1 | Không hoạt động, 1 không thể di chuyển | 

## Vỏ cạnh 

Nếu 1 bắt đầu ở chỉ mục đầu tiên hoặc cuối cùng và các thao tác bao gồm các phạm vi bao gồm cả ranh giới thì thuật toán sẽ hợp nhất các phạm vi một cách chính xác. Ví dụ, với`n = 5, x = 5, m = 1`và vận hành`[4,5]`, phạm vi ban đầu`[5,5]`hợp nhất với`[4,5]`ĐẾN`[4,5]`. Đầu ra là`2`, điều đó đúng. Hoạt động đơn phần tử`[3,3]`không trùng với phạm vi hiện tại, giữ nguyên phạm vi, tạo ra kết quả đầu ra 1 một cách chính xác.
