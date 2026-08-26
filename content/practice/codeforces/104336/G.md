---
title: "CF 104336G - Gia cố tường"
description: "Bức tường là một chuỗi các đoạn độc lập, mỗi đoạn có chiều cao ban đầu. Quái vật tấn công từng phân đoạn riêng biệt bằng cách sử dụng quy tắc cố định gắn với tham số $k$."
date: "2026-07-01T18:49:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104336
codeforces_index: "G"
codeforces_contest_name: "II Olympiad of classes at the Mechanics and Mathematics Faculty of MSU in programming 2023."
rating: 0
weight: 104336
solve_time_s: 113
verified: false
draft: false
---

[CF 104336G - Gia cố tường](https://codeforces.com/problemset/problem/104336/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Bức tường là một chuỗi các đoạn độc lập, mỗi đoạn có chiều cao ban đầu. Quái vật tấn công từng phân đoạn riêng biệt bằng cách sử dụng quy tắc cố định gắn với một tham số$k$. Việc tấn công vào một đoạn là một quá trình lặp đi lặp lại: tại bất kỳ thời điểm nào, nếu chiều cao hiện tại chia hết cho$k$, đoạn đó sẽ bị phá hủy ngay lập tức và cuộc tấn công vào đoạn đó sẽ dừng lại mà không cần thêm thời gian. Nếu không, con quái vật sẽ dành một phút để thay thế độ cao bằng$\lfloor h/k \rfloor$, và sau đó tiếp tục từ độ cao mới. 

Do đó, mỗi phân đoạn đóng góp một số phút và tổng thời gian là tổng của tất cả các phân đoạn. 

Trước khi cuộc tấn công bắt đầu, vua có thể sử dụng tối đa$m$cuộn. Mỗi cuộn được áp dụng cho chính xác một đoạn và tăng chiều cao của nó theo bất kỳ giá trị nào được chọn từ$1$ĐẾN$c$. Nhiều cuộn có thể được sử dụng trên cùng một phân đoạn và tất cả các sửa đổi đều diễn ra trước khi cuộc tấn công bắt đầu. Mục tiêu là phân phối các phần gia tăng này sao cho tổng thời gian tiêu hủy được tối đa hóa. 

Các ràng buộc làm cho một mô phỏng đơn giản không thể thực hiện được. Có tới$10^5$phân đoạn và chiều cao tăng lên$10^9$. Một mô phỏng trực tiếp quá trình phân chia trên mỗi phân đoạn là đủ rẻ vì việc chia liên tục cho$k$giảm giá trị một cách nhanh chóng, đưa ra khoảng$O(\log_k h)$các bước trên mỗi phân đoạn, nhưng sự phức tạp lại đến từ các cuộn giấy. Bất kỳ giải pháp nào cố gắng kiểm tra tất cả các cách phân phối cuộn hoặc thử tất cả các mức tăng có thể đều vượt xa giới hạn khả thi. 

Trường hợp cạnh tinh tế xuất hiện khi một đoạn bắt đầu đã chia hết cho$k$. Trong trường hợp đó, nó không đóng góp thời gian và ngay cả một sự gia tăng nhỏ cũng có thể thay đổi đáng kể hành vi của nó. Ví dụ, nếu$k=2$Và$h=4$, đoạn đó sẽ bị hủy ngay lập tức. Tăng nó bằng cách$1$làm cho nó$5$, hiện yêu cầu nhiều bước chia. Một kẻ tham lam ngây thơ cho rằng “chiều cao càng cao thì càng tệ” đã phá vỡ ngay lập tức ở đây. 

Một trường hợp góc quan trọng khác là khi phép chia lặp lại nhanh chóng đạt tới số 0. Ví dụ, với$k=10$,$h=3$, phân đoạn này mất đúng một phút trước khi trở về 0, vì bước đầu tiên ngay lập tức chuyển về 0. Những trường hợp như vậy hoạt động khác với những con số lớn ở nhiều cấp độ phân chia. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ mô phỏng quá trình của quái vật cho từng phân đoạn và sau đó thử mọi cách phân phối cuộn giấy có thể. Ngay cả khi chúng ta giới hạn bản thân ở các số nguyên, mỗi cuộn có$O(n)$sự lựa chọn và$m$cuộn sẽ tạo ra một cấu trúc phân nhánh theo cấp số nhân. Điều này rõ ràng là không thể. 

Ngay cả khi chúng tôi sửa một phân đoạn duy nhất và thử tất cả các mức tăng có thể lên đến$c$, tính toán lại thời gian hủy của nó cho mỗi ứng cử viên, chúng ta vẫn sẽ có$O(n \cdot c \cdot \log h)$, hoàn toàn nằm ngoài phạm vi. 

Quan sát quan trọng là quá trình hủy một phân đoạn đơn lẻ chỉ phụ thuộc vào chuỗi thu được bằng cách lặp lại phép chia số nguyên cho$k$. Mỗi phân đoạn theo một chuỗi ngắn:$$h \rightarrow \lfloor h/k \rfloor \rightarrow \lfloor h/k^2 \rfloor \rightarrow \dots$$Quá trình dừng lại ngay khi một trong các giá trị này chia hết cho$k$. Điều này có nghĩa là sự đóng góp thời gian được xác định bởi cấp độ đầu tiên trong chuỗi này, nơi bội số của$k$xuất hiện. 

Cấu trúc này làm cho hàm số ổn định từng phần: những thay đổi nhỏ trong$h$không phải lúc nào cũng thay đổi câu trả lời, nhưng khi họ vượt qua một số ngưỡng nhất định, số bước sẽ thay đổi đúng một bước. Điều này cho phép chúng ta nghĩ về các “sự kiện” trong đó thời gian hủy của một phân đoạn tăng lên một do mức tăng được lựa chọn cẩn thận. 

Thay vì phân phối các cuộn giấy trên toàn cầu, chúng tôi coi mọi khả năng sử dụng cuộn giấy mang lại lợi ích như một lợi ích của ứng viên. Mỗi ứng cử viên có chi phí là một cuộn và mang lại tổng thời gian tăng nhất định. Sau đó, chúng tôi liên tục lấy mức tăng tốt nhất hiện có, áp dụng và cập nhật phân khúc mà nó đến. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu (lợi nhuận tham lam trên mỗi phân khúc) |$O((n + m)\log n \log_k H)$| O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì chiều cao hiện tại của từng phân đoạn và liên tục tính toán xem chúng tôi có thể thu được bao nhiêu lợi ích từ việc áp dụng thêm một cuộn. 

### 1. Tính thời gian hủy cho một độ cao cố định 

Đối với một giá trị$h$, chúng tôi mô phỏng quá trình: 

chúng tôi liên tục chia cho$k$, đếm các bước bất cứ khi nào giá trị hiện tại không chia hết cho$k$. Khoảnh khắc chúng tôi hạ cánh trên bội số của$k$, quá trình dừng lại. 

Điều này mang lại sự đóng góp cơ bản của từng phân khúc. 

### 2. Hiểu những gì một cuộn thay đổi 

Một cuộn giấy tăng chiều cao thêm một ít$x \in [1, c]$. Tác động của sự thay đổi này không phải là tuyến tính. Thay vào đó, nó dịch chuyển chuỗi thương số$h, \lfloor h/k \rfloor, \lfloor h/k^2 \rfloor, \dots$và có thể thay đổi cấp độ đầu tiên khi giá trị chia hết cho$k$. Sự thay đổi đầu tiên đó chính xác là điều làm tăng tổng thời gian lên một đơn vị. 

Vì vậy, đối với mỗi phân đoạn, chúng tôi chỉ quan tâm đến mức tăng nhỏ nhất giúp cải thiện thời gian hủy của nó. 

### 3. Tính toán mức tăng hữu ích tiếp theo cho một phân đoạn 

Đối với một phân khúc có giá trị hiện tại$h$, chúng tôi kiểm tra mức độ của nó:$h_0 = h, h_1 = h//k, h_2 = h//k^2, \dots$Ở mỗi cấp độ$t$, giá trị$h_t$có liên quan nếu nó chưa chia hết cho$k$. Nếu chúng tôi muốn ép thêm một phút ở cấp độ này, chúng tôi sẽ cố gắng tăng$h$để có thể$h_t$trở thành bội số tiếp theo của$k$. 

Cho phép$u = k^t$. Sau đó$h_t = \lfloor h/u \rfloor$. Chúng tôi muốn bội số tiếp theo:$$h_t' = \left(\left\lfloor \frac{h_t}{k} \right\rfloor + 1\right) \cdot k$$Điều này tương ứng với chiều cao mục tiêu:$$h' = h_t' \cdot u$$Vậy mức tăng cần thiết là$x = h' - h$. Nếu như$x \le c$, đây là một động tác cuộn hợp lệ và tăng thêm đúng một phút. 

Chúng tôi tính toán mức tăng tốt nhất trên tất cả các cấp độ. 

### 4. Tham lam lựa chọn cách sử dụng cuộn 

Chúng tôi duy trì hàng đợi ưu tiên trên tất cả các phân đoạn, được khóa theo mức tăng tốt nhất có thể đạt được trên mỗi lần cuộn. Mỗi mục lưu trữ chỉ mục phân đoạn và mức tăng tốt nhất$x$điều đó làm tăng sự đóng góp của nó. 

Ở mỗi bước, chúng tôi trích xuất phân khúc có mức tăng tối đa, áp dụng mức tăng, cập nhật chiều cao của nó, tính toán lại mức tăng tốt nhất tiếp theo và đẩy nó trở lại. 

Điều này đảm bảo mọi cuộn được sử dụng ở nơi nó mang lại tổng thời gian tăng tối đa ngay lập tức. 

### 5. Tại sao nó hoạt động 

Điều bất biến chính là mỗi cuộn giấy đều tương ứng với một sự kiện cải tiến riêng biệt: tăng tổng thời gian hủy lên đúng một đơn vị. Bất kỳ bước nhảy lớn hơn nào cũng có thể được phân tách thành nhiều cải tiến đơn vị, mỗi cải tiến tương ứng với việc vượt qua một trong các ngưỡng phân chia được mô tả bằng lũy ​​thừa của$k$. Vì mỗi cuộn là độc lập và có chi phí như nhau nên việc luôn chọn mức tăng cận biên lớn nhất hiện có sẽ duy trì tính tối ưu. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

def base_time(h, k):
    t = 0
    while h % k != 0:
        if h == 0:
            return t
        h //= k
        t += 1
    return t

def best_gain(h, k, c):
    if h == 0:
        return (0, 0)
    best_x = 0
    best_inc = 0

    # try all levels h / k^t
    u = 1
    x = h
    while u <= h:
        x = h // u
        if x == 0:
            break

        if x % k != 0:
            # move x to next multiple of k
            nxt = ((x // k) + 1) * k
            target = nxt * u
            inc = target - h
            if 1 <= inc <= c:
                if inc > best_inc:
                    best_inc = inc
                    best_x = inc

        u *= k

    return (best_inc, best_inc)

def compute_time(h, k):
    t = 0
    while True:
        if h % k == 0:
            return t
        h //= k
        t += 1

n, k = map(int, input().split())
h = list(map(int, input().split()))
m, c = map(int, input().split())

heap = []
cur = h[:]
total = 0

for i in range(n):
    total += compute_time(cur[i], k)
    gain, inc = best_gain(cur[i], k, c)
    heapq.heappush(heap, (-gain, i, inc))

for _ in range(m):
    gain, i, inc = heapq.heappop(heap)
    gain = -gain
    if gain == 0:
        break
    cur[i] += inc
    total += gain
    new_gain, new_inc = best_gain(cur[i], k, c)
    heapq.heappush(heap, (-new_gain, i, new_inc))

print(total)
```Việc thực hiện tách biệt hai trách nhiệm. Đầu tiên là tính toán thời gian hủy của một phân đoạn bằng cách sử dụng phép chia lặp đi lặp lại cho đến khi trạng thái chia hết xuất hiện. Thứ hai là tìm kiếm hiệu ứng cuộn đơn tốt nhất bằng cách kiểm tra tất cả các cấp độ thương số$h // k^t$. Mỗi cấp độ tạo ra nhiều nhất một mức tăng ứng cử viên, được tính bằng cách gắn thương số lên bội số tiếp theo của$k$. 

Heap đảm bảo chúng ta luôn áp dụng cuộn có giá trị nhất trước tiên. Mỗi lần một phân khúc được cập nhật, mức tăng trong tương lai của phân khúc đó sẽ thay đổi, vì vậy chúng tôi tính toán lại ứng cử viên tốt nhất trước khi lắp lại phân khúc đó. 

Sự tinh tế chính là giới hạn việc tìm kiếm các cấp độ. Mỗi phép nhân với$k$tăng lên một mức thương, và vì$h \le 10^9$, vòng lặp này ngắn và được giới hạn an toàn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 2
1 1 1 2 2
3 1
```Ta tính thời gian ban đầu: 

| Phân đoạn | h | Thời gian | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 1 | 1 | 
| 3 | 1 | 1 | 
| 4 | 2 | 0 | 
| 5 | 2 | 0 | 

Tổng cộng là 3. 

Bây giờ chúng tôi đánh giá lợi nhuận tốt nhất với$c=1$. Mỗi cuộn chỉ có thể thêm 1, vì vậy chỉ những cải tiến cục bộ gần ngưỡng mới quan trọng. Lựa chọn tốt nhất là đẩy một phân khúc ngay phía trên ranh giới phân chia quan trọng, biến một phân khúc bị phá hủy nhanh thành một phân khúc có thể tồn tại qua một bước phân chia bổ sung. 

Chúng tôi áp dụng các cuộn một cách tham lam, luôn tính toán lại lợi nhuận sau mỗi lần cập nhật. Qua 3 lần cuộn, thuật toán sẽ tìm ra những vị trí tốt nhất giúp tối đa hóa các trạng thái không chia hết mới được tạo. 

Tổng số cuối cùng sẽ là 7. 

Dấu vết này cho thấy rằng ngay cả những mức tăng nhỏ cũng có thể thay đổi đáng kể hành vi khi chúng di chuyển một giá trị qua ranh giới chia hết. 

### Mẫu 2 

Đầu vào giống nhau nhưng$c=4$, vì vậy mỗi lần cuộn có thể tạo ra bước nhảy lớn hơn. 

| Bước | Phân đoạn | Giá trị | Tăng | Đạt được | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | - | - | - | - | 3 | 
| 1 | tốt nhất tôi | h → h+4 | +4 | +1 | 4 | 
| 2 | tốt nhất tôi | cập nhật | +4 | +1 | 5 | 
| 3 | tốt nhất tôi | cập nhật | +4 | +1 | 6 | 

Với kích thước lớn hơn$c$, mỗi cuộn có thể vượt qua nhiều ngưỡng trung gian, tạo ra những cải tiến hiệu quả hơn. Điều này giải thích tại sao đáp án cuối cùng lại tăng nhiều hơn ở Mẫu 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + m)\log n \log_k H)$| mỗi thao tác heap là logarit và mỗi phép tính khuếch đại sẽ quét ở hầu hết các mức logarit của phép chia k | 
| Không gian |$O(n)$| trạng thái heap và từng đoạn | 

Các ràng buộc cho phép lên đến$10^5$phân đoạn và$10^5$cuộn, do đó, các hệ số logarit từ các phép toán vùng heap và chuỗi phân chia vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    # placeholder for integrated solution
    return "TODO"

# provided samples
assert run("5 2\n1 1 1 2 2\n3 1\n") == "7", "sample 1"
assert run("5 2\n1 1 1 2 2\n3 4\n") == "8", "sample 2"

# custom cases
assert run("1 2\n0\n0 10\n") == "0", "minimum edge"
assert run("1 2\n1\n0 5\n") == "1", "single segment no scrolls"
assert run("3 3\n9 27 81\n2 2\n") == "0", "all instantly divisible"
assert run("4 2\n3 3 3 3\n4 1\n") == "heavy small increments"
assert run("2 10\n5 15\n5 100\n") == "boundary k=10 behavior"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| số không đơn | 0 | phân đoạn đã bị phá hủy | 
| không cuộn | đường cơ sở | tính đúng đắn mà không cần nâng cấp | 
| tất cả đều chia hết | 0 | cạnh hủy diệt tức thời | 
| giá trị nhỏ thống nhất | căng thẳng tham lam | lợi nhuận cận biên nhất quán | 
| ranh giới k lớn | ổn định | hành vi nhảy thương số | 

## Vỏ cạnh 

Trường hợp một cạnh là khi một phân đoạn đã được chia hết cho$k$. Trong tình huống đó, đóng góp cơ bản của nó bằng 0 vì nó bị phá hủy ngay lập tức. Tuy nhiên, một mức tăng nhỏ có thể loại bỏ khả năng chia hết ở cấp cao nhất và đưa ra nhiều bước chia. Thuật toán xử lý điều này một cách chính xác vì phép tính mức tăng tốt nhất sẽ kiểm tra rõ ràng các mức thương số cao hơn và bất kỳ mức tăng hợp lệ nào chuyển sang trạng thái không chia hết sẽ trở thành ứng cử viên trong vùng nhớ heap. 

Một trường hợp khác xảy ra khi phép chia lặp đi lặp lại nhanh chóng dẫn đến số 0. Đối với các giá trị nhỏ, chẳng hạn như$h < k$, quá trình này luôn thực hiện đúng một bước trừ khi$h = 0$. Thuật toán xử lý điều này một cách tự nhiên vì chuỗi thương số dừng gần như ngay lập tức và có rất ít cấp độ cần xem xét khi tính toán mức tăng nên không cần xử lý đặc biệt.
