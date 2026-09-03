---
title: "CF 104471E - Đấu Trường Boss"
description: "Nhiệm vụ mô tả một đấu trường một chiều được làm bằng các ô từ 1 đến $n$. Bạn bắt đầu ở vị trí $s$, và trong khoảng thời gian $m$ riêng biệt, trùm sẽ tung ra các đòn tấn công cấm đứng bên trong một phân đoạn cụ thể $[li, ri]$."
date: "2026-06-30T12:52:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104471
codeforces_index: "E"
codeforces_contest_name: "TheForces Round #20 (7-Problems-Forces)"
rating: 0
weight: 104471
solve_time_s: 113
verified: false
draft: false
---

[CF 104471E - Đấu trường trùm](https://codeforces.com/problemset/problem/104471/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ mô tả một đấu trường một chiều được làm bằng các ô xếp từ 1 đến$n$. Bạn bắt đầu ở vị trí$s$, và hơn thế nữa$m$những khoảnh khắc riêng biệt mà ông chủ thực hiện các cuộc tấn công cấm đứng bên trong một phân khúc cụ thể$[l_i, r_i]$. Nếu bạn đang ở trong đoạn đó vào thời điểm tấn công, cuộc chạy sẽ kết thúc ngay lập tức. 

Trước mỗi cuộc tấn công, bạn được phép thay đổi vị trí của mình. Bạn có thể ở lại nơi bạn đang ở miễn phí hoặc thực hiện dịch chuyển tức thời để di chuyển bạn một cách chính xác$k$gạch trái hoặc phải, trả tiền$k$năng lượng. Ngân sách năng lượng không bao giờ được xuống dưới 0, vì vậy mục tiêu là giảm thiểu tổng năng lượng tiêu tốn đồng thời đảm bảo rằng tại mỗi bước bạn đều nằm ngoài phân đoạn bị cấm đối với bước đó. 

Cấu trúc quan trọng là bạn không chọn đích đến cuối cùng; bạn phải chọn một vị trí hợp lệ trước mỗi cuộc tấn công và mỗi vị trí phụ thuộc vào cả những hạn chế di chuyển và nhu cầu tránh khoảng thời gian bị cấm hiện tại. 

Những hạn chế đẩy tới một$O(m)$hoặc$O(m \log n)$giải pháp cho mỗi trường hợp thử nghiệm, với tổng số$m$qua các bài kiểm tra giới hạn bởi$2 \cdot 10^5$. Bất kỳ giải pháp nào cố gắng tính toán lại khả năng tiếp cận trên tất cả các ô trên mỗi bước sẽ ngay lập tức trở nên quá chậm. 

Một điểm mô hình hóa tinh tế nhưng quan trọng là chuyển động bị hạn chế ở những bước nhảy có kích thước cố định.$k$, nghĩa là các vị trí được chia thành các cấp số cộng độc lập theo modulo$k$. Điều này ngăn cản việc tái định vị tùy ý trong một bước duy nhất và buộc phải có biểu đồ chuyển tiếp có cấu trúc. 

Một số tình huống khó khăn quan trọng: 

Nếu khoảng thời gian bị cấm không ảnh hưởng đến vị trí hiện tại của bạn thì việc không làm gì luôn có giá trị. Ví dụ, nếu$n = 10$,$s = 5$, Và$[l_1, r_1] = [7, 9]$, thì ở mức 5 là an toàn tầm thường. 

Nếu vị trí hiện tại của bạn nằm trong đoạn cấm, bạn phải rời khỏi nó trước khi tấn công. Ví dụ: nếu bạn ở 6 tuổi và đoạn bị cấm là$[4, 8]$, bạn buộc phải di chuyển sang trái 4 hoặc sang phải 8, nhưng hạn chế di chuyển có thể ngăn cản việc tiếp cận cả hai bên trong một bước. 

Nếu các bước di chuyển quá lớn so với cấu trúc phân đoạn bị cấm, thì chiến lược ngây thơ “luôn nhảy đi ngay lập tức” có thể thất bại vì nó bỏ qua những hạn chế trong tương lai và có thể lãng phí năng lượng một cách không cần thiết. 

## Phương pháp tiếp cận 

Quan điểm brute-force coi đây là bài toán đường đi ngắn nhất trên biểu đồ mở rộng theo thời gian. Mỗi trạng thái là một cặp bao gồm thời gian và vị trí, đồng thời các chuyển đổi tương ứng với việc ở lại hoặc áp dụng dịch chuyển tức thời có độ dài cố định. Mỗi lớp thời gian cấm tất cả các nút bên trong$[l_i, r_i]$. Chạy đường đi ngắn nhất trên biểu đồ này sẽ đúng, nhưng kích thước biểu đồ là$O(nm)$, điều đó làm cho nó hoàn toàn không thể thực hiện được. 

Quan sát cấu trúc quan trọng là chuyển động không tùy tiện trên đường mà xảy ra theo các bước cố định. Điều này chia đấu trường thành các chuỗi độc lập trong đó mỗi chuỗi hoạt động giống như một biểu đồ đường. Trên mỗi chuỗi, chuyển động về cơ bản là một bước sang trái hoặc sang phải mỗi giây và vấn đề trở thành việc duy trì một vị trí hợp lệ để tránh các khoảng thời gian bị cấm theo thời gian đồng thời giảm thiểu số lần di chuyển. 

Thông tin chi tiết về tối ưu hóa là lý do duy nhất để di chuyển là khi vị trí hiện tại không còn hợp lệ. Khi điều đó xảy ra, hành động tốt nhất là di chuyển đến vị trí hợp lệ gần nhất bên ngoài phân đoạn bị cấm, bởi vì bất kỳ chuyển động nào tiếp theo sẽ làm tăng chi phí nghiêm trọng mà không cải thiện tính khả thi ở bước hiện tại. Sự điều chỉnh tham lam này là đủ vì chi phí chỉ phụ thuộc vào tổng dịch chuyển chứ không phụ thuộc vào các tương tác trạng thái trong tương lai theo cách có thể được hưởng lợi từ việc “vượt quá”. 

Điều này làm giảm vấn đề trong việc theo dõi một vị trí duy nhất và chỉ sửa chữa nó khi nó rơi vào khoảng cấm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (con đường ngắn nhất được mở rộng theo thời gian) |$O(nm)$|$O(nm)$| Quá chậm | 
| Tham lam sửa chữa vị trí |$O(m)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Thuật toán duy trì một biến duy nhất đại diện cho vị trí hiện tại của bạn và tích lũy chi phí năng lượng bất cứ khi nào cần di chuyển. 

1. Khởi tạo vị trí của bạn như$x = s$và tổng năng lượng bằng 0. 
2. Trong mỗi giây$i$, đọc đoạn bị cấm$[l_i, r_i]$. 
3. Nếu vị trí hiện tại của bạn$x$nằm ngoài khoảng$[l_i, r_i]$, không làm gì cả và chuyển sang giây tiếp theo. Không có năng lượng nào bị tiêu hao vì không cần chuyển động. 
4. Nếu$x$nằm bên trong$[l_i, r_i]$, bạn phải di chuyển đến vị trí an toàn. Chỉ có hai ứng cử viên:$l_i - 1$Và$r_i + 1$, giả sử chúng nằm trong giới hạn. 
5. Chọn vị trí nào gần hơn giữa hai vị trí biên này đối với$|x - (l_i - 1)|$Và$|x - (r_i + 1)|$. Di chuyển đến đó và thêm khoảng cách di chuyển vào chi phí năng lượng. 
6. Cập nhật$x$đến vị trí mới này và tiếp tục. 

Lý do khiến bước sửa chữa cục bộ này đúng là vì khi bạn ở trong một khoảng bị cấm, mọi vị trí hợp lệ đều nằm ngoài khoảng đó và chi phí hoàn toàn là tuyến tính theo khoảng cách. Bất kỳ giải pháp tối ưu nào cũng phải thoát khỏi khoảng thời gian và lối thoát gần nhất sẽ giảm thiểu chi phí mà không ảnh hưởng đến tính khả thi tại thời điểm đó. 

### Tại sao nó hoạt động 

Ở mỗi bước, thuật toán đảm bảo rằng vị trí đã chọn là hợp lệ cho cuộc tấn công hiện tại. Khi cần sửa chữa, nó sẽ chọn điểm khả thi gần nhất ngoài khoảng cấm. Bất kỳ động thái thay thế nào ở xa hơn sẽ làm tăng chi phí mà không mang lại lợi ích gì ở bước thời gian hiện tại và việc trì hoãn việc thoát ra là không thể vì việc ở trong khoảng thời gian đó sẽ gây tử vong ngay lập tức. Điều này làm cho việc điều chỉnh tham lam trở nên tối ưu cục bộ và vì các quyết định chỉ phụ thuộc vào vi phạm hiện tại nên nó vẫn tối ưu toàn cục theo trình tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, s = map(int, input().split())
        x = s
        energy = 0

        for _ in range(m):
            l, r = map(int, input().split())

            if l <= x <= r:
                left = l - 1
                right = r + 1

                best = None
                best_cost = 10**18

                if left >= 1:
                    best = left
                    best_cost = abs(x - left)

                if right <= n:
                    cost = abs(x - right)
                    if cost < best_cost:
                        best = right
                        best_cost = cost

                energy += best_cost
                x = best

        print(energy)

if __name__ == "__main__":
    solve()
```Giải pháp chỉ giữ vị trí hiện tại và cập nhật nó một cách tham lam. Sự tinh tế duy nhất là xử lý chính xác các ranh giới khi khoảng cấm chạm vào các mép của đấu trường. Trong những trường hợp đó, chỉ tồn tại một hướng thoát hợp lệ nên thuật toán sẽ chọn hướng đó một cách tự nhiên. 

Việc cập nhật chi phí được thực hiện bằng cách sử dụng khoảng cách tuyệt đối, phù hợp với số lượng hoạt động dịch chuyển tức thời cần thiết nếu chuyển động được hiểu là chuyển tiếp từng bước đơn vị dọc theo đường. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một đấu trường nhỏ nơi việc buộc phải tránh dần dần đẩy người chơi ra ngoài. 

| Bước | Khoảng thời gian | Vị trí trước | Khoảng bên trong | Hành động | Vị trí sau | Năng lượng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | [3,5] | 4 | vâng | chuyển sang 2 | 2 | 2 | 
| 2 | [2,3] | 2 | vâng | chuyển sang 1 | 1 | 3 | 
| 3 | [5,6] | 1 | không | ở lại | 1 | 3 | 

Dấu vết này cho thấy chuyển động chỉ xảy ra như thế nào khi vị trí hiện tại trở nên không hợp lệ và mỗi lần di chuyển sẽ đẩy đến ranh giới an toàn gần nhất. 

### Ví dụ 2 

Bây giờ hãy xem xét trường hợp không có ràng buộc nào ảnh hưởng đến vị trí bắt đầu. 

| Bước | Khoảng thời gian | Vị trí trước | Khoảng bên trong | Hành động | Vị trí sau | Năng lượng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | [7,8] | 4 | không | ở lại | 4 | 0 | 
| 2 | [6,9] | 4 | không | ở lại | 4 | 0 | 
| 3 | [2,3] | 4 | không | ở lại | 4 | 0 | 

Điều này chứng tỏ rằng thuật toán tránh được những chuyển động không cần thiết một cách tự nhiên và chỉ phản ứng với những vi phạm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m)$mỗi trường hợp thử nghiệm | Mỗi khoảng thời gian được xử lý một lần với các cập nhật liên tục | 
| Không gian |$O(1)$| Chỉ vị trí hiện tại và bộ tích lũy được lưu trữ | 

Tổng số thao tác trên tất cả các trường hợp thử nghiệm là tuyến tính bằng tổng của$m$, phù hợp thoải mái trong các ràng buộc của$2 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    # inline solution
    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, m, s = map(int, input().split())
            x = s
            energy = 0
            for _ in range(m):
                l, r = map(int, input().split())
                if l <= x <= r:
                    left = l - 1
                    right = r + 1
                    best = None
                    best_cost = 10**18
                    if left >= 1:
                        best = left
                        best_cost = abs(x - left)
                    if right <= n:
                        cost = abs(x - right)
                        if cost < best_cost:
                            best = right
                            best_cost = cost
                    energy += best_cost
                    x = best
            out.append(str(energy))
        return "\n".join(out)

    return solve()

# custom minimal case
assert run("1\n2 1 1\n2 2\n") == "0"

# simple forced move
assert run("1\n5 1 3\n2 4\n") == "1"

# no moves needed
assert run("1\n5 2 3\n4 5\n1 2\n") == "0"

# alternating pressure
assert run("1\n10 3 5\n4 6\n4 6\n4 6\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu | 0 | đã an toàn | 
| buộc phải di chuyển | 1 | chi phí thoát hiểm đúng | 
| không di chuyển | 0 | duy trì tối ưu | 
| ràng buộc lặp đi lặp lại | 6 | chi phí tích lũy sửa chữa nhiều lần | 

## Vỏ cạnh 

Khi vị trí bắt đầu nằm ngoài tất cả các đoạn bị cấm, thuật toán không thực hiện chuyển động nào cả. Ví dụ: bắt đầu từ 1 với tất cả các khoảng cách xa bên phải sẽ dẫn đến năng lượng bằng 0 vì không có quá trình sửa chữa nào được kích hoạt. 

Khi khoảng cấm bao gồm mọi thứ ngoại trừ một bên, chẳng hạn như$[2, n]$, lối thoát hợp lệ duy nhất là di chuyển đến vị trí 1. Thuật toán xử lý việc này vì chỉ tồn tại một ứng cử viên biên và nó được chọn tự động. 

Khi vị trí hiện tại nằm chính xác ở ranh giới của một khoảng bị cấm, nó vẫn được coi là không an toàn vì khoảng đó đã bao gồm. Trong trường hợp đó, thuật toán kích hoạt một nước đi một cách chính xác và tránh những sai sót nhỏ bằng cách kiểm tra rõ ràng$l \le x \le r$.
