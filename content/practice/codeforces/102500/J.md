---
title: "CF 102500J - Jackdaws và quạ"
description: "Chúng tôi có một chuỗi các điểm bình luận. Nick được phép dành thời gian tạo tài khoản giả, trong đó mỗi tài khoản có thể thay đổi bất kỳ điểm nào đã chọn theo một trong hai hướng và anh ấy cũng có thể xóa nhận xét."
date: "2026-08-05T18:10:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102500
codeforces_index: "J"
codeforces_contest_name: "2019-2020 ICPC Northwestern European Regional Programming Contest (NWERC 2019)"
rating: 0
weight: 102500
solve_time_s: 189
verified: true
draft: false
---

[CF 102500J - Jackdaws và Crows](https://codeforces.com/problemset/problem/102500/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi các điểm bình luận. Nick được phép dành thời gian tạo tài khoản giả, trong đó mỗi tài khoản có thể thay đổi bất kỳ điểm nào đã chọn theo một trong hai hướng và anh ấy cũng có thể xóa nhận xét. Dãy số cuối cùng còn lại phải có mọi điểm khác 0 và các điểm lân cận có dấu ngược nhau. 

Nếu Nick tạo`f`tài khoản giả, điểm có giá trị tuyệt đối nhỏ hơn`f`có thể chuyển thành tích cực hoặc tiêu cực. Điểm như vậy hoạt động giống như một ký tự đại diện. Mọi điểm khác 0 khác vẫn giữ nguyên dấu ban đầu. Nhiệm vụ là chọn số lượng tài khoản cần tạo và số lượng bình luận cần xóa sao cho tổng thời gian được giảm thiểu. 

Độ dài mảng có thể đạt tới`5 * 10^5`, vì vậy việc kiểm tra mọi số lượng tài khoản có thể và xây dựng lại toàn bộ chuỗi sẽ quá chậm. Chúng tôi cần xử lý các thay đổi do việc tăng số lượng tài khoản gây ra gần như tuyến tính. Điểm số có thể lớn như`10^9`, nhưng chỉ ở những thời điểm khi điểm số trở thành ký tự đại diện quan trọng, vì vậy bản thân phạm vi số khổng lồ không phải là vấn đề. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. Điểm 0 không thể giữ nguyên trong chuỗi nếu không có tài khoản giả. Ví dụ:```
1 5 7
0
```Với số không có tài khoản, bình luận phải được loại bỏ, chi phí`7`. Với một tài khoản, nó có thể sử dụng được, chi phí`5`, vậy câu trả lời là`5`. Một nghiệm coi số 0 là dấu cố định thông thường sẽ cho kết quả sai. 

Một cái bẫy khác là các dấu bằng liên tiếp có thể cần phải loại bỏ nhiều lần. Ví dụ:```
3 1 100
1 1 1
```Không tạo tài khoản nào yêu cầu xóa cả ba nhận xét ngoại trừ một nhận xét, điều này sẽ tốn phí`200`. Tạo một tài khoản làm cho tất cả các giá trị trở nên linh hoạt, tính chi phí`1`. Giải pháp chỉ tính các cặp xấu liền kề có thể hiểu sai số lần xóa cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ thử mọi số lượng tài khoản giả có thể. Với mỗi giá trị của`f`, chúng tôi phân loại mọi điểm số, tính toán chuỗi con xen kẽ dài nhất có thể còn lại và trả tiền cho những nhận xét đã xóa. Điều này đúng vì nó kiểm tra rõ ràng mọi chiến lược có thể. Tuy nhiên, có thể có`O(10^9)`số lượng tài khoản có thể có và thậm chí hạn chế chúng ta ở những giá trị có ý nghĩa vẫn để lại`O(n)`khả năng xây dựng lại mảng chi phí`O(n)`. Trường hợp xấu nhất trở thành`O(n^2)`những hoạt động không thể thực hiện được`n = 5 * 10^5`. 

Quan sát quan trọng là điểm số chỉ thay đổi hành vi của nó một lần. Điểm có giá trị`x`trở nên linh hoạt chính xác khi số lượng tài khoản trở nên lớn hơn`|x|`. Giữa các thời điểm này, tập hợp các nhận xét linh hoạt không thay đổi nên chỉ cần kiểm tra đáp án tại các thời điểm đó.`n + 1`số lượng tài khoản thú vị. 

Đối với một số lượng tài khoản cố định, chỉ những nhận xét không phải ký tự đại diện mới quan trọng. Hãy xem xét hai nhận xét không phải ký tự đại diện liên tiếp theo thứ tự ban đầu. Hãy để có`k`nhận xét ký tự đại diện giữa chúng. Họ tạo ra xung đột chính xác khi các dấu hiệu không thể được thực hiện xen kẽ qua khoảng trống đó. Điều này xảy ra khi các dấu bằng có khoảng cách bằng nhau hoặc các dấu hiệu ngược nhau có khoảng cách có kích thước lẻ. Mỗi xung đột như vậy đòi hỏi phải loại bỏ một lần. 

Khi một nhận xét trở thành ký tự đại diện, nó sẽ biến mất khỏi danh sách các dấu hiệu cố định. Chỉ có hai bình luận cố định lân cận bị ảnh hưởng, do đó số lượng xung đột có thể được cập nhật theo thời gian không đổi. Danh sách liên kết đôi cho phép chúng tôi xóa các nhận xét mới linh hoạt một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân loại mọi điểm số theo dấu dương cố định, dấu âm cố định hoặc sự kiện sẽ trở thành ký tự đại diện. Điểm khác 0`s`trở nên linh hoạt tại`|s| + 1`tài khoản, trong khi số 0 trở nên linh hoạt tại`1`tài khoản. 
2. Bắt đầu với không có tài khoản giả mạo. Tất cả các điểm khác 0 được lưu giữ trong một danh sách liên kết các dấu hiệu cố định. Điểm 0 đóng góp một lần xóa bắt buộc cho mỗi điểm. Tính số cặp cố định liền kề xấu ban đầu. 
3. Sắp xếp tất cả các sự kiện ký tự đại diện theo số lượng tài khoản mà chúng xảy ra. Chỉ những số lượng tài khoản này mới cần được kiểm tra. 
4. Khi một sự kiện được xử lý, hãy loại bỏ vị trí đó khỏi danh sách liên kết các biển báo cố định. Trước khi loại bỏ nó, hãy loại bỏ các xung đột liên quan đến các hàng xóm cố định trước đó và tiếp theo của nó. Sau khi loại bỏ, hãy thêm xung đột mới giữa hai hàng xóm đó nếu cả hai vẫn tồn tại. 
5. Sau khi xử lý tất cả các sự kiện cho một số tài khoản cụ thể`f`, số lượng báo cáo cần thiết hiện tại đã được biết. Tổng chi phí là:```
f * c + removals * r
```Cập nhật câu trả lời tối thiểu. 

Tại sao nó hoạt động: đối với một số lượng tài khoản cố định, mọi nhận xét linh hoạt luôn có thể được gán dấu hiệu mà các tài khoản lân cận cần. Vấn đề duy nhất không thể tránh khỏi là giữa các nhận xét cố định liên tiếp có dấu và khoảng cách tương đương khiến cho việc luân phiên không thể thực hiện được. Việc loại bỏ chính xác một bình luận cho mỗi xung đột như vậy là đủ và cần thiết. Việc tăng số lượng tài khoản chỉ xóa các nhận xét cố định khỏi mức hiển thị đã giảm này, do đó, việc duy trì số lượng xung đột sau mỗi lần xóa sẽ duy trì số lượng báo cáo tối thiểu chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, c, r = map(int, input().split())
    s = list(map(int, input().split()))

    sign = [0] * n
    events = {}

    zero = 0
    for i, x in enumerate(s):
        if x == 0:
            zero += 1
            t = 1
        else:
            sign[i] = 1 if x > 0 else -1
            t = abs(x) + 1
        events.setdefault(t, []).append(i)

    prev = [-1] * n
    nxt = [-1] * n

    last = -1
    for i in range(n):
        if sign[i]:
            if last != -1:
                nxt[last] = i
                prev[i] = last
            last = i

    def bad(a, b):
        if a == -1 or b == -1:
            return 0
        gap = b - a - 1
        return 1 if ((sign[a] == sign[b]) == (gap % 2 == 0)) else 0

    removals = zero
    cur = -1
    for i in range(n):
        if sign[i]:
            if cur != -1:
                removals += bad(cur, i)
            cur = i

    ans = removals * r

    for f in sorted(events):
        for i in events[f]:
            if sign[i] == 0:
                removals -= 1
                continue

            a = prev[i]
            b = nxt[i]

            removals -= bad(a, i)
            removals -= bad(i, b)

            if a != -1:
                nxt[a] = b
            if b != -1:
                prev[b] = a

            removals += bad(a, b)

            sign[i] = 0

        ans = min(ans, f * c + removals * r)

    print(ans)

if __name__ == "__main__":
    solve()
```Danh sách liên kết chỉ chứa những bình luận vẫn buộc phải có dấu hiệu ban đầu. Khi một nhận xét trở thành ký tự đại diện, nó không còn ảnh hưởng đến xung đột vì nó luôn có thể được gán dấu hiệu cần thiết sau này. 

các`bad`chức năng là cốt lõi của giải pháp. Khoảng cách giữa hai nhận xét cố định xác định xem khoảng cách ký tự đại diện có lật ngược mức chẵn lẻ được yêu cầu hay không. Vì các nút liền kề trong danh sách liên kết không có nhận xét cố định giữa chúng, nên chênh lệch chỉ số của chúng trực tiếp đưa ra số lượng ký tự đại diện giữa chúng. 

Số nguyên Python đã hỗ trợ độ chính xác tùy ý, vì vậy các sản phẩm có chi phí lên tới`10^9`Và`5 * 10^5`bình luận được an toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp các vị trí sự kiện chiếm ưu thế; tất cả các cập nhật danh sách liên kết đều tuyến tính | 
| Không gian | O(n) | Mỗi mảng và bộ lưu trữ sự kiện chứa tối đa`n`mục | 

Giải pháp xử lý mọi bình luận với số lần không đổi. Bước sắp xếp có thể chấp nhận được đối với`5 * 10^5`sự kiện. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
4 10 50
8 8 2 -2
```| Tài khoản | Ký tự đại diện mới | Những xung đột còn lại | Chi phí | 
| --- | --- | --- | --- | 
| 0 | không | 2 | 100 | 
| 3 |`2`,`-2`trở nên linh hoạt | 1 | 80 | 
| 9 | tất cả đều trở nên linh hoạt | 0 | 90 | 

Lựa chọn tốt nhất là ba tài khoản và một báo cáo, đưa ra`80`. 

Đối với mẫu thứ hai:```
6 100 33
5 -13 0 0 -12 0
```| Tài khoản | Ký tự đại diện mới | Những xung đột còn lại | Chi phí | 
| --- | --- | --- | --- | 
| 0 | không | 3 lần xóa không | 99 | 
| 1 | mọi số không đều trở nên linh hoạt | 3 | 199 | 
| 6 | điểm`5`trở nên linh hoạt | 2 | 266 | 
| 13 | tất cả đều trở nên linh hoạt | 0 | 1300 | 

Giải pháp tốt nhất là xóa ngay ba bình luận không có ý kiến, sau đó trả tiền cho những xung đột còn lại, dẫn đến`132`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old

    n, c, r = map(int, data[:3])
    arr = list(map(int, data[3:]))

    sign = [0] * n
    events = {}
    zero = 0

    for i, x in enumerate(arr):
        if x == 0:
            zero += 1
            t = 1
        else:
            sign[i] = 1 if x > 0 else -1
            t = abs(x) + 1
        events.setdefault(t, []).append(i)

    prev = [-1] * n
    nxt = [-1] * n
    last = -1
    for i in range(n):
        if sign[i]:
            if last != -1:
                nxt[last] = i
                prev[i] = last
            last = i

    def bad(a, b):
        if a == -1 or b == -1:
            return 0
        return int(((sign[a] == sign[b]) == ((b - a - 1) % 2 == 0)))

    rem = zero
    last = -1
    for i in range(n):
        if sign[i]:
            if last != -1:
                rem += bad(last, i)
            last = i

    ans = rem * r
    for f in sorted(events):
        for i in events[f]:
            if sign[i] == 0:
                rem -= 1
            else:
                a, b = prev[i], nxt[i]
                rem -= bad(a, i) + bad(i, b)
                if a != -1:
                    nxt[a] = b
                if b != -1:
                    prev[b] = a
                rem += bad(a, b)
                sign[i] = 0
        ans = min(ans, f * c + rem * r)

    return str(ans)

assert run("4 10 50\n8 8 2 -2\n") == "80"
assert run("6 100 33\n5 -13 0 0 -12 0\n") == "132"
assert run("1 5 7\n0\n") == "5"
assert run("3 1 100\n1 1 1\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 5 7 / 0`|`5`| Zero trở thành ký tự đại diện | 
|`3 1 100 / 1 1 1`|`1`| Tất cả các dấu bằng và số lần xóa lớn | 
| Mẫu 1 |`80`| Nhận xét cố định và linh hoạt hỗn hợp | 
| Mẫu 2 |`132`| Nhiều số không và các sự kiện ký tự đại diện bị trì hoãn | 

## Vỏ cạnh 

Một nhận xét bằng 0 duy nhất thể hiện cách xử lý đặc biệt đối với các điểm không thể sử dụng được ở các tài khoản bằng 0:```
1 5 7
0
```Thuật toán bắt đầu với một lần loại bỏ bắt buộc. Tại số tài khoản`1`, số 0 trở thành ký tự đại diện, do đó số lần loại bỏ giảm xuống 0 và câu trả lời trở thành`5`. 

Một chuỗi các dấu bằng nhau giải thích tại sao xung đột phải được tính thông qua danh sách liên kết thay vì chỉ xem xét các cặp trong mảng ban đầu:```
3 1 100
1 1 1
```Danh sách liên kết ban đầu có hai cặp liền kề không tốt nên cần phải loại bỏ hai lần. Sau một tài khoản, mọi bình luận đều linh hoạt, danh sách liên kết trở nên trống rỗng và không cần báo cáo. Thuật toán kiểm tra trạng thái này và trả về`1`.
