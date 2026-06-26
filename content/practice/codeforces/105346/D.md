---
title: "CF 105346D - Cơn ác mộng ngày 24"
description: "Chúng ta có một dãy các tòa nhà được đánh số từ trái sang phải, trong đó mỗi tòa nhà chứa một số lượng sinh viên nhất định. Freddy luôn bắt đầu từ tòa nhà đầu tiên và chỉ có thể tiến về phía trước theo thứ tự."
date: "2026-06-23T15:33:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105346
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 09-13-24 Div. 2 (Beginner)"
rating: 0
weight: 105346
solve_time_s: 87
verified: false
draft: false
---

[CF 105346D - Cơn ác mộng ngày 24](https://codeforces.com/problemset/problem/105346/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dãy các tòa nhà được đánh số từ trái sang phải, trong đó mỗi tòa nhà chứa một số lượng sinh viên nhất định. Freddy luôn bắt đầu từ tòa nhà đầu tiên và chỉ có thể tiến về phía trước theo thứ tự. Đối với mỗi giá trị truy vấn, chúng tôi muốn biết tiền tố nhỏ nhất của các tòa nhà có tổng số sinh viên ít nhất bằng giá trị truy vấn đó. Nếu ngay cả sau khi đã ghé thăm tất cả các tòa nhà mà tổng số vẫn nhỏ hơn truy vấn thì câu trả lời là không thể. 

Về cơ bản đây là vấn đề tích lũy tiền tố. Mỗi truy vấn yêu cầu vị trí đầu tiên nơi tổng tiền tố đạt hoặc vượt quá ngưỡng. 

Các ràng buộc cho phép lên tới 100.000 tòa nhà và 100.000 truy vấn. Một mô phỏng trực tiếp cho mỗi truy vấn sẽ quét từ đầu mỗi lần, dẫn đến 10^10 thao tác trong trường hợp xấu nhất, quá chậm trong Python. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào tính toán lại các khoản tiền nhiều lần. 

Các trường hợp cạnh phát sinh khi các giá trị xây dựng bao gồm số không. Một cách tiếp cận con trỏ ngây thơ giả định tiến độ luôn được thực hiện trên mỗi tòa nhà có thể thất bại nếu nhiều tòa nhà liên tiếp không đóng góp gì. Ví dụ: nếu tất cả a_i bằng 0 và q_j dương thì câu trả lời đúng là -1 và bất kỳ phương pháp nào dựa vào tiến trình tăng dần đều phải kiểm tra rõ ràng khả năng tiếp cận so với tổng. 

Một trường hợp tinh vi khác là khi q_j bằng 0. Câu trả lời đúng luôn là 1, vì không có học sinh nào yêu cầu tham quan số lượng tòa nhà không đáng kể, nhưng tòa nhà đầu tiên vẫn là tiền tố tối thiểu. 

## Phương pháp tiếp cận 

Phương pháp brute-force xử lý từng truy vấn một cách độc lập. Với một giá trị q nhất định, chúng tôi bắt đầu từ tòa nhà đầu tiên và tích lũy số lượng học sinh cho đến khi đạt hoặc vượt quá q. Điều này đúng vì nó trực tiếp tuân theo định nghĩa bài toán. Tuy nhiên, trong trường hợp xấu nhất, mỗi truy vấn có thể yêu cầu quét tất cả n tòa nhà. Với m truy vấn, giá trị này trở thành O(nm), tối đa 10^10 thao tác. 

Quan sát chính là tất cả các truy vấn đều hoạt động trên cùng một cấu trúc tiền tố tĩnh. Khi chúng tôi tính tổng tiền tố của mảng, mọi truy vấn sẽ chuyển thành vấn đề tìm kiếm: tìm chỉ mục i nhỏ nhất sao cho prefix[i] ≥ q. Vì tổng tiền tố không giảm nên chúng ta có thể sử dụng tìm kiếm nhị phân cho mỗi truy vấn. Điều này làm giảm mỗi truy vấn xuống O(log n), nâng tổng số lên O((n + m) log n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Tiền tố + Tìm kiếm nhị phân | O((n + m) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Tính tổng tiền tố trên mảng tòa nhà, trong đó prefix[i] lưu tổng số học sinh từ tòa nhà 1 đến tòa nhà i. Điều này biến vấn đề thành khả năng tiếp cận phạm vi thay vì tính tổng lặp lại. 
2. Với mỗi truy vấn q, hãy kiểm tra xem q có lớn hơn tiền tố tổng [n] hay không. Nếu đúng như vậy, hãy ghi -1 ngay lập tức vì ngay cả việc truy cập tất cả các tòa nhà cũng không đủ. 
3. Nếu q bằng 0, xuất ra 1, vì tiền tố hợp lệ nhỏ nhất là tòa nhà đầu tiên. Điều này xử lý trường hợp suy biến trong đó không cần tích lũy. 
4. Mặt khác, thực hiện tìm kiếm nhị phân trên mảng tiền tố để tìm chỉ số i nhỏ nhất sao cho tiền tố[i] ≥ q. Điều này có hiệu quả vì tổng tiền tố không giảm một cách đơn điệu. 
5. Xuất i cho mỗi truy vấn. 

### Tại sao nó hoạt động 

Mảng tổng tiền tố mã hóa khả năng tiếp cận tích lũy: prefix[i] thể hiện chính xác những gì có thể đạt được bằng cách dừng ở việc xây dựng i. Vì các giá trị không bao giờ giảm khi chúng ta di chuyển sang phải, tiền tố điều kiện[i] ≥ q xác định vùng liền kề của các câu trả lời hợp lệ. Tìm kiếm nhị phân khai thác cấu trúc đơn điệu này để xác định chỉ mục hợp lệ đầu tiên. Thuật toán không thể bỏ lỡ một câu trả lời hợp lệ vì khi tiền tố[i] vượt quá q, tất cả các chỉ số sau cũng thỏa mãn nó, đảm bảo tính chính xác của ranh giới được tìm thấy đầu tiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    q = list(map(int, input().split()))

    prefix = [0] * n
    prefix[0] = a[0]
    for i in range(1, n):
        prefix[i] = prefix[i - 1] + a[i]

    total = prefix[-1]
    out = []

    for x in q:
        if x == 0:
            out.append("1")
            continue
        if x > total:
            out.append("-1")
            continue

        lo, hi = 0, n - 1
        ans = n - 1

        while lo <= hi:
            mid = (lo + hi) // 2
            if prefix[mid] >= x:
                ans = mid
                hi = mid - 1
            else:
                lo = mid + 1

        out.append(str(ans + 1))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xây dựng một mảng tổng tiền tố để mọi truy vấn trở thành tìm kiếm trên tổng tích lũy thay vì tổng lặp lại. Tìm kiếm nhị phân được triển khai thủ công để tránh chi phí và kiểm soát rõ ràng vị trí đầu tiên nơi thỏa mãn ràng buộc tiền tố. Sự chuyển đổi`ans + 1`là bắt buộc vì mảng không được lập chỉ mục nội bộ nhưng vấn đề sử dụng các vị trí xây dựng được lập chỉ mục một. 

Việc xử lý đặc biệt đối với`x == 0`ngăn chặn việc tìm kiếm nhị phân không cần thiết và đảm bảo tính chính xác cho điều kiện biên trong đó tiền tố đầu tiên luôn hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 7, m = 5
a = [10, 8, 2, 4, 4, 8, 9]
q = [10, 15, 25, 41, 100]
```Mảng tiền tố: 

| tôi | tiền tố[i] | 
| --- | --- | 
| 1 | 10 | 
| 2 | 18 | 
| 3 | 20 | 
| 4 | 24 | 
| 5 | 28 | 
| 6 | 36 | 
| 7 | 45 | 

| Truy vấn | Các bước tìm kiếm nhị phân | Chỉ số trả lời | Đầu ra | 
| --- | --- | --- | --- | 
| 10 | đạt i=1 | 1 | 1 | 
| 15 | đầu tiên ≥15 là i=2 | 2 | 2 | 
| 25 | đầu tiên ≥25 là i=5 | 5 | 5 | 
| 41 | đầu tiên ≥41 là i=7 | 7 | 7 | 
| 100 | tổng số < 100 | không | -1 | 

Điều này xác nhận rằng tìm kiếm nhị phân tìm thấy chính xác tiền tố ngoài cùng bên trái đáp ứng ngưỡng và từ chối chính xác các truy vấn không thể truy cập. 

### Ví dụ 2 

đầu vào:```
n = 5, m = 3
a = [0, 0, 5, 0, 10]
q = [1, 5, 15]
```Mảng tiền tố: 

| tôi | tiền tố[i] | 
| --- | --- | 
| 1 | 0 | 
| 2 | 0 | 
| 3 | 5 | 
| 4 | 5 | 
| 5 | 15 | 

| Truy vấn | Hành vi tìm kiếm | Chỉ số trả lời | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | bỏ qua số 0, hạ cánh ở 3 | 3 | 3 | 
| 5 | lượt truy cập đầu tiên ở mức 3 | 3 | 3 | 
| 15 | phần tử cuối cùng chính xác | 5 | 5 | 

Ví dụ này chứng minh tính đúng đắn khi có các tòa nhà có giá trị bằng 0, trong đó tiến độ không được đảm bảo ở mọi bước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | xây dựng tiền tố là O(n), mỗi tìm kiếm nhị phân truy vấn là O(log n) | 
| Không gian | O(n) | mảng tiền tố lưu trữ tổng tích lũy | 

Các ràng buộc cho phép tổng số phép tính lên tới 2 × 10^5 ở dạng logarit, phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    q = list(map(int, input().split()))

    prefix = [0] * n
    prefix[0] = a[0]
    for i in range(1, n):
        prefix[i] = prefix[i - 1] + a[i]

    total = prefix[-1]
    res = []

    for x in q:
        if x == 0:
            res.append("1")
            continue
        if x > total:
            res.append("-1")
            continue

        lo, hi = 0, n - 1
        ans = n - 1
        while lo <= hi:
            mid = (lo + hi) // 2
            if prefix[mid] >= x:
                ans = mid
                hi = mid - 1
            else:
                lo = mid + 1

        res.append(str(ans + 1))

    return "\n".join(res)

# provided sample
assert run("""7 5
10 8 2 4 4 8 9
10 15 25 41 100
""") == """1
2
5
7
-1"""

# all zeros
assert run("""5 3
0 0 0 0 0
0 1 5
""") == """1
-1
-1"""

# single building
assert run("""1 2
5
3 5
""") == """-1
1"""

# increasing prefix boundary
assert run("""4 3
1 2 3 4
1 10 11
""") == """1
4
-1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không | 1, -1, -1 | số tiền không thể truy cập và không xử lý | 
| tòa nhà đơn | -1, 1 | độ chính xác kích thước cạnh tối thiểu | 
| tiền tố tăng dần | 1, 4, -1 | tìm kiếm ranh giới chính xác | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi tất cả các giá trị xây dựng bằng không. Trong trường hợp này, mảng tiền tố không bao giờ tăng, vì vậy mọi truy vấn dương đều phải trả về -1 ngay lập tức. Thuật toán xử lý điều này vì tiền tố điều kiện tìm kiếm nhị phân[mid] ≥ x không bao giờ được thỏa mãn và việc kiểm tra tổng sẽ sớm từ chối truy vấn. 

Một trường hợp đặc biệt khác là khi tòa nhà đầu tiên đã đáp ứng được truy vấn. Ví dụ: a = [100, ...], q = 50. Mảng tiền tố bắt đầu ở một giá trị lớn, do đó tìm kiếm nhị phân trả về chính xác chỉ mục 0, chuyển thành đầu ra 1. Độ chính xác phụ thuộc vào việc tìm kiếm chỉ mục hợp lệ ngoài cùng bên trái chứ không chỉ bất kỳ chỉ mục hợp lệ nào.
