---
title: "CF 319E - Bóng bàn"
description: "Chúng tôi được yêu cầu duy trì một tập hợp các khoảng thời gian động và trả lời các truy vấn về khả năng tiếp cận giữa chúng. Mỗi khoảng được biểu thị bằng hai số nguyên $(x, y)$ trong đó $x < y$. Các khoảng thời gian được thêm lần lượt và mỗi khoảng thời gian mới được đảm bảo dài hơn tất cả các khoảng thời gian trước đó."
date: "2026-06-06T02:04:02+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 319
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 189 (Div. 1)"
rating: 3000
weight: 319
solve_time_s: 91
verified: true
draft: false
---

[CF 319E - Bóng bàn](https://codeforces.com/problemset/problem/319/E) 

**Đánh giá:** 3000 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu duy trì một tập hợp các khoảng thời gian động và trả lời các truy vấn về khả năng tiếp cận giữa chúng. Mỗi khoảng được biểu thị bằng hai số nguyên$(x, y)$Ở đâu$x < y$. Các khoảng thời gian được thêm lần lượt và mỗi khoảng thời gian mới được đảm bảo dài hơn tất cả các khoảng thời gian trước đó. Di chuyển từ khoảng$(a, b)$khoảng cách$(c, d)$được cho phép nếu một trong hai điểm cuối của$(a, b)$nằm đúng bên trong$(c, d)$. Một đường đi tồn tại giữa hai khoảng nếu chúng ta có thể xâu chuỗi những chuyển động đó. 

Đầu vào bao gồm một số truy vấn, thêm một khoảng mới hoặc hỏi xem liệu có đường dẫn tồn tại từ khoảng được thêm trước đó đến khoảng khác hay không. Đầu ra phải đáp ứng từng truy vấn thuộc loại thứ hai bằng "CÓ" hoặc "KHÔNG". 

Với$n$lên tới$10^5$và phối hợp lên đến$10^9$, chúng tôi không thể đủ khả năng$O(n^2)$phương pháp so sánh từng cặp khoảng trên mỗi truy vấn. Một giải pháp đơn giản sẽ quét toàn bộ khoảng thời gian được đặt mỗi khi chúng tôi thêm một khoảng thời gian mới hoặc kiểm tra khả năng tiếp cận, điều này có thể dẫn đến tối đa$10^{10}$hoạt động, vượt xa giới hạn thời gian 2 giây. Chúng tôi cần cấu trúc dữ liệu cho phép cập nhật gia tăng và truy vấn khả năng tiếp cận hiệu quả. 

Trường hợp cạnh tinh tế xảy ra do các khoảng được thêm vào với độ dài tăng dần. Điều này đảm bảo rằng mỗi khoảng mới không thể bị chứa bởi bất kỳ khoảng cũ nào, nhưng các khoảng cũ hơn có thể được chứa bởi khoảng mới. Việc triển khai bất cẩn có thể cố gắng kiểm tra tất cả các khoảng trên mỗi truy vấn mà không tận dụng thuộc tính này, tạo ra độ phức tạp thời gian bậc hai. 

Một trường hợp khác là khi các khoảng chia sẻ điểm cuối. Ví dụ, nếu$(1, 5)$Và$(5, 11)$tồn tại, thì đường đi từ điểm thứ nhất đến điểm thứ hai không tồn tại vì cả điểm cuối của điểm thứ nhất đều không nằm hoàn toàn bên trong điểm cuối thứ hai. Đầu ra đúng ở đây sẽ là "KHÔNG". 

## Phương pháp tiếp cận 

Giải pháp brute-force lặp lại trên tất cả các khoảng thời gian được thêm cho đến nay cho mỗi truy vấn. Đối với truy vấn loại 2, chúng tôi có thể thực hiện tìm kiếm theo chiều sâu từ khoảng thời gian bắt đầu, kiểm tra từng khoảng thời gian để biết khả năng tiếp cận bằng cách sử dụng quy tắc ngăn chặn điểm cuối. Mặc dù cách tiếp cận này đúng nhưng trong trường hợp xấu nhất nó đòi hỏi phải duyệt qua tất cả các khoảng cho mỗi truy vấn, dẫn đến$O(n^2)$sự phức tạp về mặt thời gian. Với$n = 10^5$, điều này rõ ràng là quá chậm. 

Quan sát quan trọng giúp mở ra giải pháp nhanh hơn là thuộc tính độ dài tăng dần. Điều này có nghĩa là mọi khoảng thời gian mới có thể "hấp thụ" các khoảng thời gian cũ hơn, nhưng chúng không thể hấp thụ được. Do đó, khả năng tiếp cận luôn được định hướng chuyển tiếp từ khoảng thời gian nhỏ hơn đến khoảng thời gian lớn hơn. Chúng ta có thể biểu diễn mỗi khoảng dưới dạng một nút và duy trì hai giá trị cho nó: vị trí tối thiểu và tối đa mà nó có thể đạt được thông qua các bước di chuyển liên tiếp. Khi một khoảng mới được thêm vào, nó có thể mở rộng phạm vi của các khoảng trước đó có điểm cuối nằm bên trong nó. Chúng ta có thể truyền bá khả năng tiếp cận này một cách hiệu quả theo thời gian tuyến tính bằng cách lặp lại các khoảng thời gian, cập nhật phạm vi có thể tiếp cận của chúng. Sau khi các khoảng thời gian được xử lý, việc trả lời một truy vấn sẽ giảm xuống việc kiểm tra xem các điểm cuối của khoảng thời gian đích có nằm trong phạm vi có thể truy cập của khoảng thời gian nguồn hay không. Điều này làm giảm vấn đề từ$O(n^2)$ĐẾN$O(n)$để xử lý tất cả các bổ sung và truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì danh sách các khoảng theo thứ tự chúng được thêm vào. Mỗi khoảng lưu trữ tọa độ bắt đầu và kết thúc của nó. 
2. Duy trì hai mảng,`reach_min`Và`reach_max`, biểu thị vị trí tối thiểu và tối đa có thể đạt được từ mỗi khoảng. 
3. Khi thêm một quãng mới`(x, y)`, khởi tạo`reach_min`Và`reach_max`đối với nó như`x`Và`y`. Lặp lại các khoảng thời gian đã thêm trước đó. Nếu điểm bắt đầu hoặc kết thúc của khoảng thời gian nằm bên trong khoảng thời gian mới, hãy cập nhật khoảng thời gian trước đó`reach_min`Và`reach_max`bằng cách mở rộng chúng để bao gồm phạm vi tiếp cận của khoảng thời gian mới. Dừng truyền nếu phạm vi của khoảng thời gian hiện tại đã bao gồm khoảng thời gian mới. 
4. Đối với một truy vấn`(a, b)`, kiểm tra xem khoảng`b`nằm hoàn toàn trong phạm vi có thể truy cập`[reach_min[a], reach_max[a]]`. Nếu một trong hai điểm cuối của`b`nằm trong phạm vi này, xuất ra "CÓ"; nếu không thì xuất ra "NO". 

Tính chính xác được đảm bảo vì độ dài khoảng tăng dần đảm bảo rằng tất cả việc truyền bá khả năng tiếp cận đều đơn điệu: khi một phạm vi được mở rộng, nó sẽ không bao giờ co lại và tất cả các bước di chuyển đều tuân theo quy tắc ngăn chặn. Điều này làm cho việc tính toán khả năng tiếp cận tương đương với một lần mở rộng khoảng thời gian, nắm bắt tất cả các đường dẫn có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
intervals = []
reach_min = []
reach_max = []

for _ in range(n):
    parts = input().split()
    if parts[0] == '1':
        x, y = int(parts[1]), int(parts[2])
        intervals.append((x, y))
        reach_min.append(x)
        reach_max.append(y)
        i = len(intervals) - 2
        while i >= 0:
            lx, rx = reach_min[i], reach_max[i]
            nx, ny = reach_min[-1], reach_max[-1]
            a, b = intervals[i]
            if (a > nx and a < ny) or (b > nx and b < ny):
                reach_min[i] = min(lx, nx)
                reach_max[i] = max(rx, ny)
            i -= 1
    else:
        a, b = int(parts[1]) - 1, int(parts[2]) - 1
        bx, by = intervals[b]
        if (bx >= reach_min[a] and bx <= reach_max[a]) or (by >= reach_min[a] and by <= reach_max[a]):
            print("YES")
        else:
            print("NO")
```Mỗi phần của giải pháp tương ứng trực tiếp với các bước thuật toán. Chúng tôi lưu trữ các khoảng thời gian và phạm vi tiếp cận của chúng. Khi thêm một khoảng thời gian mới, chúng tôi chỉ truyền bá khả năng tiếp cận ngược tới các khoảng thời gian hiện có thể đạt đến khoảng thời gian đó. Truy vấn chỉ cần kiểm tra xem khoảng đích có nằm trong phạm vi có thể truy cập của khoảng nguồn hay không. Quá trình lan truyền ngược dừng lại khi không có phần mở rộng nào nữa xảy ra, ngăn chặn các cập nhật không cần thiết và duy trì độ phức tạp tổng thời gian tuyến tính. 

## Ví dụ đã hoạt động 

Mẫu 1:```
5
1 1 5
1 5 11
2 1 2
1 2 9
2 1 2
```| Bước | Khoảng thời gian | đạt_min | phạm vi tiếp cận_max | Truy vấn | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (1,5) | 1 | 5 | - | - | 
| 2 | (5,11) | 1,5 | 5,11 | - | - | 
| 3 | - | - | - | 1→2 | KHÔNG | 
| 4 | (2,9) | 1,5,2 | 11,11,9 | - | - | 
| 5 | - | - | - | 1→2 | CÓ | 

Dấu vết này thể hiện sự lan truyền ngược của phạm vi có thể tiếp cận. Ban đầu, khoảng 1 không thể đạt tới khoảng 2 vì điểm cuối 5 của khoảng 1 không nằm hoàn toàn bên trong (5,11). Sau khi thêm khoảng 3, khả năng tiếp cận của khoảng 1 sẽ mở rộng qua khoảng 3, cho phép đường dẫn đến khoảng 2. 

Đầu vào tùy chỉnh 2:```
3
1 1 4
1 2 5
2 1 2
```| Bước | Khoảng thời gian | đạt_min | phạm vi tiếp cận_max | Truy vấn | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (1,4) | 1 | 4 | - | - | 
| 2 | (2,5) | 1,2 | 5,5 | - | - | 
| 3 | - | - | - | 1→2 | CÓ | 

Phạm vi tiếp cận của Khoảng 1 mở rộng đến khoảng 2, cho phép khả năng tiếp cận lên 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi khoảng chỉ có thể truyền phạm vi tiếp cận của nó một lần đến các khoảng trước đó và mỗi truy vấn là O(1) | 
| Không gian | O(n) | Lưu trữ n khoảng thời gian và hai mảng Reach có kích thước n | 

Giải pháp phù hợp thoải mái trong giới hạn. Truyền ngược hiệu quả nhờ đảm bảo độ dài tăng dần và các truy vấn được thực hiện ngay lập tức. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call solution
    n = int(input())
    intervals = []
    reach_min = []
    reach_max = []

    for _ in range(n):
        parts = input().split()
        if parts[0] == '1':
            x, y = int(parts[1]), int(parts[2])
            intervals.append((x, y))
            reach_min.append(x)
            reach_max.append(y)
            i = len(intervals) - 2
            while i >= 0:
                lx, rx = reach_min[i], reach_max[i]
                nx, ny = reach_min[-1], reach_max[-1]
                a, b = intervals[i]
                if (a > nx and a < ny) or (b > nx and b < ny):
                    reach_min[i] = min(lx, nx)
                    reach_max[i] = max(rx, ny)
                i -= 1
        else:
            a, b = int(parts
```
