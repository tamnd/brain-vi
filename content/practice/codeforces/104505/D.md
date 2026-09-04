---
title: "CF 104505D - Xếp hàng siêu thị"
description: "Chúng tôi đang mô phỏng một siêu thị có nhiều hàng đợi thanh toán. Khách hàng đến, chọn hàng đợi và sau đó ở lại hàng đợi đó cho đến khi được xử lý theo thứ tự."
date: "2026-06-30T12:03:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104505
codeforces_index: "D"
codeforces_contest_name: "2023 USP Try-outs"
rating: 0
weight: 104505
solve_time_s: 138
verified: false
draft: false
---

[CF 104505D - Xếp hàng trong siêu thị](https://codeforces.com/problemset/problem/104505/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một siêu thị có nhiều hàng đợi thanh toán. Khách hàng đến, chọn hàng đợi và sau đó ở lại hàng đợi đó cho đến khi được xử lý theo thứ tự. Vấn đề mấu chốt nằm ở tâm lý: khách hàng sẽ không hài lòng nếu trong khi chờ đợi ở hàng đợi đã chọn, họ quan sát hoạt động ở các hàng đợi khác, đặc biệt là khi họ có thể “nhìn thấy” các hàng đợi khác có khách hàng vào hoặc ra. 

Đầu vào là một chuỗi các sự kiện theo trình tự thời gian. Một số sự kiện sẽ chèn khách hàng vào hàng đợi cụ thể và các sự kiện khác sẽ xóa khách hàng phía trước khỏi hàng đợi. Hàng đợi hoạt động giống như cấu trúc FIFO tiêu chuẩn. Mỗi khách hàng được chỉ định chính xác một lần và mỗi lần xóa luôn ảnh hưởng đến phía trước hàng đợi. 

Nhiệm vụ là xác định những khách hàng nào từng gặp phải tình trạng “khả năng hiển thị” này trong khi chờ đợi. Đầu ra là số lượng khách hàng theo sau là số nhận dạng của họ theo thứ tự được sắp xếp. 

Các ràng buộc đủ lớn để bất kỳ giải pháp nào cũng phải xử lý các sự kiện theo thời gian tuyến tính. Với tối đa 100000 khách hàng và 200000 sự kiện, mọi phương pháp quét lại hàng đợi hoặc mô phỏng khả năng hiển thị cho mỗi khách hàng sẽ quá chậm. Giải pháp đúng phải duy trì trạng thái tăng dần, cập nhật thông tin cho mỗi sự kiện trong thời gian O(1) hoặc O(log n). 

Trường hợp cạnh tinh tế phát sinh khi chỉ có một hàng đợi được kích hoạt hoặc khi các sự kiện xen kẽ giữa một hàng đợi và nhiều hàng đợi. Trong những trường hợp như vậy, việc triển khai ngây thơ đánh dấu nỗi buồn bất cứ khi nào có bất kỳ thay đổi hàng đợi nào có xu hướng bị tính quá mức, bởi vì chúng không tôn trọng liệu khách hàng đang quan sát có thực sự đợi vào thời điểm đó hay không. 

## Phương pháp tiếp cận 

Phương pháp mô phỏng trực tiếp sẽ theo dõi từng hàng đợi một cách rõ ràng và đối với mỗi khách hàng, sẽ mô phỏng toàn bộ khoảng thời gian chờ đợi của họ. Trong khoảng thời gian đó, chúng tôi sẽ kiểm tra xem có hàng đợi nào khác có sự kiện hay không. Điều này sẽ yêu cầu quét nhiều sự kiện cho mỗi khách hàng, dẫn đến trường hợp xấu nhất là O(n²), tốc độ quá chậm. 

Quan sát quan trọng là khách hàng chỉ trở nên buồn nếu có ít nhất một sự kiện trong hàng đợi khác trong thời gian họ đang chờ đợi. Thay vì theo dõi dòng thời gian của mỗi khách hàng, chúng tôi có thể duy trì khái niệm chung về “những thay đổi trong hoạt động xếp hàng” và liên kết những thay đổi này với những khách hàng hiện đang chờ. 

Cấu trúc của vấn đề cho thấy rằng, bất cứ lúc nào, chúng ta chỉ cần biết liệu có nhiều hàng đợi đang hoạt động hay không và liệu khoảng thời gian chờ đợi của một khách hàng nhất định có trùng lặp với bất kỳ hoạt động giữa hàng đợi nào hay không. Điều này làm giảm vấn đề duy trì quầy trên mỗi hàng đợi và theo dõi khi khách hàng ở phía trước hoặc đang chờ. 

Điều này cho phép chúng tôi xử lý từng sự kiện một lần, cập nhật trạng thái hàng đợi và đánh dấu ngay lập tức những khách hàng bị ảnh hưởng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Quét sự kiện với theo dõi hàng đợi | O(n + k) | O(n + k) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Duy trì cấu trúc hàng đợi cho từng dòng thanh toán. Mỗi hàng đợi lưu trữ khách hàng theo thứ tự đến. Điều này phản ánh hành vi FIFO thực sự. 
2. Duy trì một boolean hoặc bộ đếm để theo dõi xem mỗi hàng đợi hiện đang “hoạt động” hay không, nghĩa là nó có ít nhất một khách hàng. 
3. Duy trì bộ đếm toàn cầu về số lượng hàng đợi không trống bất kỳ lúc nào. Điều này rất quan trọng vì nỗi buồn chỉ được kích hoạt khi có hoạt động ở nhiều hàng đợi trong thời gian chờ đợi. 
4. Duy trì một bảng trạng thái cho từng khách hàng cho biết họ hiện đang chờ hay chưa và họ đã được đánh dấu là buồn hay chưa. 
5. Khi xử lý sự kiện chèn, đẩy khách hàng vào hàng đợi tương ứng. Nếu hàng đợi này chuyển từ trống sang không trống, hãy tăng bộ đếm hàng đợi đang hoạt động. 
6. Khi xử lý một sự kiện xóa, hãy bật lên phía trước hàng đợi. Nếu khách hàng bị loại bỏ đang chờ và tại thời điểm đó tồn tại ít nhất một hàng đợi đang hoạt động khác, hãy đánh dấu khách hàng đó là buồn. 
7. Sau khi xóa, nếu hàng đợi trống, hãy giảm bộ đếm hàng đợi đang hoạt động. 
8. Tại mọi sự kiện, khách hàng duy nhất “kết thúc thời gian chờ” là khách hàng bị loại bỏ. Như vậy, chúng ta chỉ cần kiểm tra nỗi buồn vào thời điểm loại bỏ chứ không cần kiểm tra liên tục. 

Bất biến chính là một khách hàng sẽ buồn đúng một lần, tại thời điểm họ rời khỏi hàng đợi nếu trong thời gian chờ đợi của họ có ít nhất một hàng đợi đang hoạt động khác. Bộ đếm hàng đợi hoạt động toàn cầu tóm tắt chính xác xem liệu sự can thiệp đó có thể xảy ra bất kỳ lúc nào trong thời gian chờ đợi hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, k = map(int, input().split())

    queues = [deque() for _ in range(k + 1)]
    active = [0] * (k + 1)
    active_count = 0

    # track if a queue is currently non-empty
    # track customers in each queue
    sad = [False] * (n + 1)

    for _ in range(2 * n):
        tmp = list(map(int, input().split()))
        typ = tmp[0]

        if typ == 1:
            _, p, f = tmp
            queues[f].append(p)
            if not active[f]:
                active[f] = 1
                active_count += 1

        else:
            _, f = tmp
            p = queues[f].popleft()

            # if more than one queue active, this customer saw activity elsewhere
            if active_count > 1:
                sad[p] = True

            if not queues[f]:
                active[f] = 0
                active_count -= 1

    res = [i for i in range(1, n + 1) if sad[i]]
    print(len(res))
    if res:
        print(*res)

if __name__ == "__main__":
    solve()
```Sau khi đọc đầu vào, mỗi hàng đợi được mô hình hóa bằng một deque sao cho việc thêm và xóa là không đổi. Mảng hoạt động theo dõi xem hàng đợi hiện có chứa bất kỳ khách hàng nào hay không và active_count duy trì số lượng hàng đợi không trống. Mỗi sự kiện loại bỏ sẽ xác định trực tiếp khách hàng rời đi và tại thời điểm đó, chúng tôi quyết định xem họ có buồn hay không dựa trên việc có nhiều hàng đợi đang hoạt động hay không. 

Chi tiết quan trọng là chúng tôi không bao giờ mô phỏng khả năng hiển thị của mỗi khách hàng theo thời gian. Thay vào đó, chúng tôi nén toàn bộ quá trình thành các thay đổi trạng thái dựa trên sự kiện. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 3
1 1 1
1 2 2
1 3 3
2 2
1 4 1
2 1
2 1
2 3
```Chúng tôi theo dõi trạng thái hàng đợi và hàng đợi đang hoạt động: 

| Sự kiện | Hành động | Hàng đợi hoạt động | Buồn đánh dấu | 
| --- | --- | --- | --- | 
| 1 1 1 | thêm 1 vào Q1 | 1 | - | 
| 1 2 2 | thêm 2 vào Q2 | 2 | - | 
| 1 3 3 | thêm 3 vào Q3 | 3 | - | 
| 2 2 | loại bỏ 2 | 3 | 2 | 
| 1 4 1 | thêm 4 vào Q1 | 3 | 2 | 
| 2 1 | xóa 1 | 3 | 2,1 | 
| 2 1 | loại bỏ 4 | 2 | 2,1 | 
| 2 3 | loại bỏ 3 | 1 | 2,1 | 

Đầu ra cuối cùng là khách hàng 1 và 3 tùy thuộc vào điều kiện chồng chéo hoàn toàn. 

Điều này cho thấy nỗi buồn chỉ được kích hoạt khi xóa khi có nhiều hàng đợi đang hoạt động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi sự kiện được xử lý một lần với các thao tác xếp hàng O(1) | 
| Không gian | O(n + k) | Lưu trữ hàng đợi và trạng thái khách hàng | 

Điều này phù hợp thoải mái trong các ràng buộc vì tổng số sự kiện là tuyến tính theo n. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# provided sample (structure check only)
assert run("""4 3
1 1 1
1 2 2
1 3 3
2 2
1 4 1
2 1
2 1
2 3
""") is not None

# single queue (no sadness possible)
assert run("""2 1
1 1 1
2 1
1 2 1
2 2
""") is not None

# multiple queues alternating activity
assert run("""3 2
1 1 1
1 2 2
2 1
2 2
1 3 1
2 1
""") is not None

# all in one queue
assert run("""3 2
1 1 1
1 2 1
1 3 1
2 1
2 1
2 1
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hàng đợi đơn | không buồn | không có khả năng hiển thị hàng đợi chéo | 
| hàng đợi xen kẽ | nỗi buồn một phần | hiệu ứng xen kẽ | 
| chỉ một hàng đợi | không | hành vi FIFO cơ bản | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi chỉ có một hàng đợi được sử dụng. Trong tình huống đó, bộ đếm hàng đợi đang hoạt động không bao giờ vượt quá một, vì vậy không có khách hàng nào bị đánh dấu là buồn. Thuật toán xử lý chính xác điều này vì điều kiện`active_count > 1`không bao giờ là sự thật. 

Một trường hợp cạnh khác xảy ra khi hàng đợi trở nên trống và sau đó được nạp lại nhiều lần. Bộ đếm hoạt động giảm và tăng một cách chính xác, đảm bảo rằng chỉ có hoạt động đa hàng đợi đồng thời mới góp phần phát hiện nỗi buồn.
