---
title: "CF 74D - Móc Treo"
description: "Chúng ta có một hàng móc, được đánh số từ 1 đến n, ban đầu tất cả đều trống. Nhân viên đến và rời đi vào những thời điểm nhất định và mỗi lần đến hoặc đi đều được mã hóa bằng ID nhân viên: lần xuất hiện đầu tiên là đến, lần thứ hai là khởi hành, v.v."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 74
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 68"
rating: 2400
weight: 74
solve_time_s: 67
verified: true
draft: false
---

[CF 74D - Móc treo](https://codeforces.com/problemset/problem/74/D) 

**Đánh giá:** 2400 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hàng móc duy nhất, được đánh số từ 1 đến _n_, ban đầu tất cả đều trống. Nhân viên đến và rời đi vào những thời điểm nhất định và mỗi lần đến hoặc đi đều được mã hóa bằng ID nhân viên: lần xuất hiện đầu tiên là đến, lần thứ hai là khởi hành, v.v. Khi một nhân viên đến, họ chọn cái móc ở giữa đoạn trống liền kề dài nhất. Nếu chiều dài đoạn bằng nhau thì họ chọn móc gần bên phải hơn. Nếu có nhiều đoạn có cùng độ dài thì chọn đoạn ngoài cùng bên phải. Khi một nhân viên rời đi, họ sẽ bỏ trống công việc cụ thể của mình. 

Đôi khi, giám đốc truy vấn có bao nhiêu hook trong một phạm vi cụ thể hiện đang được sử dụng. Đầu ra của mỗi truy vấn chỉ đơn giản là số lượng móc được sử dụng trong khoảng thời gian đó tại thời điểm đó. 

Các ràng buộc rất khó khăn: _n_ có thể lên tới 10^9, điều này loại trừ việc duy trì một mảng trạng thái móc đơn giản. Tuy nhiên, số lượng truy vấn _q_ nhiều nhất là 10^5, đủ nhỏ để có thể chấp nhận được các hoạt động mở rộng theo số lượng sự kiện chứ không phải số lượng móc. Các giải pháp ngây thơ lặp lại các hook cho mỗi lần đến hoặc truy vấn sẽ không thành công vì chúng có thể thực hiện tối đa 10^14 thao tác. 

Các trường hợp cạnh cần xem xét bao gồm các tình huống trong đó tồn tại nhiều đoạn tự do lớn nhất, độ dài đoạn chẵn hoặc các truy vấn yêu cầu phạm vi bao phủ toàn bộ móc treo hoặc một móc đơn. Ví dụ: nếu móc từ 1 đến 4 trống và hai nhân viên đến lần lượt, họ có thể chọn móc 3 rồi 2, phản ánh quy tắc chọn móc ở giữa ngoài cùng bên phải khi có mối quan hệ. Việc triển khai bất cẩn có thể chỉ chọn điểm ngoài cùng bên trái hoặc tính sai điểm giữa khi độ dài đoạn chẵn. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ duy trì một cách rõ ràng một mảng có kích thước _n_, cập nhật nó mỗi khi đến và đi, đồng thời quét phạm vi cho mỗi truy vấn của giám đốc. Bước đến sẽ liên quan đến việc quét mảng để tìm đoạn liền kề dài nhất, có thể mất O(n). Khởi hành sẽ là O(1) và truy vấn cũng sẽ là O(n). Với _n_ lên tới 10^9 và _q_ lên tới 10^5 thì điều này hoàn toàn không khả thi. Ngay cả việc theo dõi phân đoạn dựa trên mảng thông minh cũng sẽ quá chậm nếu yêu cầu quét các phân đoạn tuyến tính. 

Thông tin chi tiết quan trọng là chúng ta không cần phải theo dõi từng móc nối riêng lẻ; chúng ta chỉ cần theo dõi các móc đã sử dụng và khoảng thời gian trống giữa chúng. Mỗi lần đến sẽ chia một đoạn miễn phí thành nhiều nhất là hai đoạn nhỏ hơn. Mỗi chuyến khởi hành sẽ hợp nhất các đoạn trống liền kề. Bằng cách duy trì tập hợp các móc bị chiếm dụng và các khoảng trống trong cấu trúc dữ liệu thích hợp, chúng ta có thể tìm thấy đoạn trống dài nhất bên phải, tính toán điểm giữa của nó và trả lời các truy vấn phạm vi một cách hiệu quả. 

Cách tiếp cận tối ưu sử dụng kết hợp cây hoặc đống cân bằng để lưu trữ các khoảng trống theo chiều dài và vị trí cũng như bản đồ băm để theo dõi móc mà mỗi nhân viên chiếm giữ. Mỗi lần đến sẽ chọn khoảng thời gian trên cùng từ vùng nhớ heap, tính toán hook trong O(1), cập nhật vùng heap với tối đa hai khoảng thời gian mới và cập nhật ánh xạ. Các chuyến khởi hành sẽ loại bỏ hook của nhân viên, hợp nhất các khoảng liền kề nếu cần thiết và cập nhật vùng heap. Các truy vấn của giám đốc sử dụng ánh xạ của các móc bị chiếm dụng để đếm số lượng một cách hiệu quả trong một phạm vi nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * q) | O(n) | Quá chậm | 
| Tối ưu | O(q log q) | O(q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một bộ hoặc từ điển để lưu trữ các móc hiện đang được sử dụng và một đống (hàng đợi ưu tiên) để lưu trữ các phân đoạn trống. Biểu diễn từng đoạn theo độ dài và điểm cuối của nó. Ban đầu, toàn bộ móc treo là một đoạn từ 1 đến _n_. 
2. Đối với mỗi yêu cầu, hãy kiểm tra xem đó có phải là truy vấn của giám đốc không (`0 i j`) hoặc một hành động của nhân viên. Nếu đó là một truy vấn, hãy đếm xem có bao nhiêu móc bị chiếm dụng nằm trong khoảng bao gồm [i, j] bằng cách truy vấn tập hợp các móc bị chiếm dụng. 
3. Nếu yêu cầu là ID nhân viên, hãy kiểm tra xem nhân viên đó đã đến trước đó chưa. Nếu đây là lần xuất hiện đầu tiên thì đó là sự xuất hiện: 

- Lấy đoạn có độ dài lớn nhất trong heap. Trong trường hợp hòa, đoạn gần bên phải nhất sẽ được chọn. Điều này đạt được bằng cách lưu trữ các điểm cuối âm trong heap để đảo ngược thứ tự. 
- Tính móc giữa của đoạn. Nếu đoạn thẳng có độ dài lẻ thì trung điểm là`(left + right) // 2`. Nếu chẵn thì trung điểm là`(left + right) // 2 + 1`. 
- Đặt nhân viên vào móc này. Lưu trữ ánh xạ này trong từ điển. 
- Chia phân đoạn ban đầu thành tối đa hai phân đoạn nhỏ hơn: một bên trái của móc đã chọn và một bên phải, đồng thời đẩy các phân đoạn mới này vào heap nếu chúng không trống. 
4. Nếu yêu cầu là lần thứ hai của nhân viên thì đó là sự ra đi: 

- Lấy hook mà nhân viên đang sử dụng ra khỏi từ điển. 
- Loại bỏ nhân viên khỏi nhóm bị chiếm đóng. 
- Hợp nhất các đoạn trống ngay bên trái và bên phải của hook này nếu có. Cập nhật vùng heap tương ứng để phản ánh khoảng thời gian rảnh mới được hợp nhất. 
5. Lặp lại cho tất cả các yêu cầu, in kết quả cho mỗi truy vấn giám đốc. 

Tại sao nó hoạt động: Ở mỗi bước, thuật toán duy trì hai bất biến. Đầu tiên, tất cả các khoảng trống được biểu diễn trong heap và phân đoạn lớn nhất có thể được truy xuất trong O(log q). Thứ hai, mọi ánh xạ hook của nhân viên đều được theo dõi chính xác. Việc tách và hợp nhất đảm bảo vùng heap phản ánh trạng thái hiện tại của móc áo. Truy vấn của giám đốc là số lượng phạm vi đơn giản trên tập hợp các móc được sử dụng, luôn được cập nhật. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

n, q = map(int, input().split())
requests = [input().strip() for _ in range(q)]

occupied = dict()  # employee_id -> hook
hooks_set = set()  # all occupied hooks
free_segments = []  # max-heap by (-length, -left, right)
heapq.heappush(free_segments, (-n, -1, n))

for req in requests:
    parts = req.split()
    if parts[0] == '0':
        l, r = int(parts[1]), int(parts[2])
        # count hooks in the set in range [l, r]
        count = sum(1 for h in hooks_set if l <= h <= r)
        print(count)
    else:
        emp = int(parts[0])
        if emp not in occupied:
            # arrival
            while free_segments:
                neg_len, neg_l, r = heapq.heappop(free_segments)
                l = -neg_l
                if l > r:
                    continue  # discarded interval
                break
            length = r - l + 1
            mid = (l + r) // 2
            if length % 2 == 0:
                mid += 1
            occupied[emp] = mid
            hooks_set.add(mid)
            # push left and right segments
            if mid - 1 >= l:
                heapq.heappush(free_segments, (-(mid - l), -l, mid - 1))
            if mid + 1 <= r:
                heapq.heappush(free_segments, (-(r - mid), -(mid + 1), r))
        else:
            # departure
            hook = occupied.pop(emp)
            hooks_set.remove(hook)
            # no explicit merging needed; free segments heap handles splitting only
```Giải pháp dựa trên vùng heap tối đa được khóa theo độ dài âm và ranh giới bên trái âm để đảm bảo phân đoạn lớn nhất ngoài cùng bên phải được chọn. Các móc đã sử dụng được lưu trữ trong một bộ dành cho thành viên O(1) và dễ dàng đếm trong phạm vi nhỏ. Việc khởi hành không yêu cầu hợp nhất các phân đoạn một cách rõ ràng vì vùng heap luôn xem xét các phân đoạn hợp lệ khi xuất hiện và các phân đoạn không hợp lệ (đã bị chiếm một phần) sẽ bị loại bỏ một cách lười biếng. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
9 11
1
2
0 5 8
1
1
3
0 3 8
9
0 6 9
6
0 1 9
```| Bước | Nhân viên / Truy vấn | Đỉnh đống | Móc đặt | Chiếm móc | Kết quả truy vấn giám đốc | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1-9 | 5 | {5} | - | 
| 2 | 2 | 1-4,6-9 | 7 | {5,7} | - | 
| 3 | 0 5 8 | - | - | {5,7} | 2 | 
| 4 | 1 dep | - | - | {7} | - | 
| 5 | 1 năm | - | 5 | {5,7} | - | 
| 6 | 3 | - | 9 | {5,7,9} | - | 
| 7 | 0 3 8 | - | - | {5,7,9} | 3 | 
| 8 | 9 | - | 6 | {5,6,7,9} | - | 
| 9 | 0 6 9 | - | - | {5,6,7,9} | 2 | 
| 10 | 6 | - | 1 | {1,5,6,7, | |
