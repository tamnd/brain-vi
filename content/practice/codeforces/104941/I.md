---
title: "CF 104941I - Tôi Điệp Viên"
description: "Chúng ta được cung cấp một lưới có $n$ hàng và $m$ cột. Mỗi ô đại diện cho một cửa sổ có thể sáng hoặc tối. Cấu hình bị hạn chế theo hai cách. Đầu tiên, mỗi hàng $i$ có một số lượng cố định $ai$ cửa sổ sáng."
date: "2026-06-28T18:19:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104941
codeforces_index: "I"
codeforces_contest_name: "SLPC 2024 Open Division"
rating: 0
weight: 104941
solve_time_s: 67
verified: false
draft: false
---

[CF 104941I - Tôi theo dõi](https://codeforces.com/problemset/problem/104941/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới với$n$hàng và$m$cột. Mỗi ô đại diện cho một cửa sổ có thể sáng hoặc tối. Cấu hình bị hạn chế theo hai cách. 

Đầu tiên, mỗi hàng$i$có một số cố định$a_i$của các cửa sổ có ánh sáng. Thứ hai, không được có hai cửa sổ có ánh sáng chạm vào nhau theo chiều ngang hoặc chiều dọc. Điều đó có nghĩa là nếu một cửa sổ được chiếu sáng thì các cửa sổ bên trái và bên phải của nó trong cùng một hàng phải tối và các cửa sổ ngay phía trên và bên dưới cửa sổ đó cũng phải tối. 

Nhiệm vụ là đếm xem tồn tại bao nhiêu cấu hình toàn cục hợp lệ của các cửa sổ sáng, modulo$10^9+7$. 

Các ràng buộc có chiều rộng nhỏ nhưng chiều cao vừa phải lớn:$n \le 30$,$m \le 23$. Điều này ngay lập tức gợi ý rằng chúng ta có thể thực hiện công việc theo cấp số nhân trên các hàng hoặc mặt nạ bit có kích thước$2^m$, nhưng không phải trên lưới đầy đủ. Ràng buộc theo chiều ngang là cục bộ đối với các hàng, trong khi ràng buộc theo chiều dọc kết hợp các hàng liền kề, điều này gợi ý rõ ràng về lập trình động theo từng hàng với trạng thái mặt nạ bit. 

Một cách tiếp cận ngây thơ sẽ thử tất cả$2^{nm}$các cấu hình có kích thước lớn về mặt thiên văn. Ngay cả việc hạn chế theo số lượng hàng vẫn để lại quá nhiều khả năng. Khó khăn chính là thực thi đồng thời cả hai ràng buộc kề trong khi khớp tổng số hàng. 

Trường hợp cạnh tinh tế xuất hiện khi$a_i$lớn so với$m$. Nếu như$a_i > \lceil m/2 \rceil$, riêng hàng đó không thể thỏa mãn quy tắc không kề nhau theo chiều ngang, vì số lượng ô không liền kề tối đa trong một hàng có độ dài$m$là$\lceil m/2 \rceil$. Trong những trường hợp như vậy, câu trả lời phải bằng 0 ngay lập tức. 

Một trường hợp cạnh khác là khi$n = 1$. Sau đó, vấn đề giảm xuống còn việc đếm các tập hợp độc lập hợp lệ trên một biểu đồ đường dẫn có kích thước cố định, hoàn toàn là tổ hợp trên mỗi hàng. 

## Phương pháp tiếp cận 

Giải thích bạo lực coi mỗi ô là một biến nhị phân và kiểm tra tất cả các ràng buộc trên toàn cầu. Điều này đúng nhưng không khả thi. Kích thước không gian trạng thái là$2^{nm}$và thậm chí kiểm tra các ràng buộc trên mỗi cấu hình là$O(nm)$, dẫn đến$O(nm2^{nm})$, vượt xa giới hạn. 

Chúng tôi tinh chỉnh quan điểm bằng cách nhận thấy rằng các ràng buộc là cục bộ. Liền kề theo chiều ngang chỉ ảnh hưởng trong một hàng, trong khi kề dọc chỉ ảnh hưởng đến các hàng liền kề. Điều này gợi ý việc tách lưới thành các trạng thái hàng. 

Mỗi hàng có thể được biểu diễn dưới dạng bitmask có độ dài$m$, trong đó 1 biểu thị cửa sổ sáng. Mặt nạ hàng hợp lệ không được chứa các số 1 liền kề. Ngoài ra, số lượng 1 trong hàng$i$phải bằng$a_i$. 

Bây giờ, lưới trở thành một chuỗi các mặt nạ hàng với các hạn chế về khả năng tương thích: các hàng liền kề không được chia sẻ số 1 trong cùng một cột. Nghĩa là, đối với mặt nạ hàng$x$Và$y$, chúng tôi yêu cầu$x \& y = 0$. 

Điều này biến vấn đề thành việc đếm các đường dẫn trong biểu đồ phân lớp: mỗi lớp là một hàng, các nút là mặt nạ hợp lệ và các cạnh thể hiện tính tương thích. 

Cái nhìn sâu sắc quan trọng là$m \le 23$, do đó, có thể quản lý được số lượng mặt nạ hợp lệ không có số 1 liền kề (được giới hạn bởi mức tăng Fibonacci, xấp xỉ$F_{25} \approx 10^5$). Điều này cho phép lập trình động trên các trạng thái. 

Chúng tôi tính toán trước tất cả các mặt nạ hợp lệ và số lượng của chúng. Sau đó, chúng tôi tính toán trước các chuyển tiếp giữa các mặt nạ không chồng lên nhau theo chiều dọc. Cuối cùng, chúng tôi chạy DP từng hàng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^{nm})$|$O(nm)$| Quá chậm | 
| Mặt nạ bit DP |$O(n \cdot S^2)$Ở đâu$S \approx F_{m+2}$|$O(S^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định trạng thái là cấu hình hàng hợp lệ được mã hóa dưới dạng mặt nạ bit. 

## 1. Tạo mặt nạ hàng hợp lệ 

Chúng tôi liệt kê tất cả các mặt nạ từ$0$ĐẾN$2^m - 1$. Mặt nạ hợp lệ nếu nó không chứa các bit được đặt liền kề. Điều này đảm bảo sự kề cận theo chiều ngang được thỏa mãn. 

Chúng tôi cũng ghi lại số lượng bit đã đặt trong mỗi mặt nạ để thực thi các ràng buộc hàng. 

## 2. Nhóm mặt nạ theo hàng yêu cầu 

Đối với mỗi hàng$i$, chúng tôi chỉ cho phép mặt nạ có số lượng bằng$a_i$. Điều này sẽ loại bỏ sớm các trạng thái không hợp lệ và giảm chuyển tiếp DP. 

## 3. Khả năng tương thích tính toán trước 

Đối với bất kỳ hai mặt nạ hợp lệ$x$Và$y$, chúng tôi xác định chúng tương thích nếu$(x \& y) = 0$. Điều này thực thi ràng buộc kề dọc. 

Chúng tôi tính toán trước cho mỗi mặt nạ một danh sách tất cả các mặt nạ tương thích ở hàng tiếp theo. 

## 4. Khởi tạo lập trình động 

Đối với hàng đầu tiên, chúng tôi đặt$dp[mask] = 1$cho tất cả các mặt nạ hợp lệ với số lượng$a_1$. 

Điều này thể hiện tất cả các cách để đặt các cửa sổ có ánh sáng ở hàng đầu tiên phù hợp với các ràng buộc. 

## 5. Chuyển tiếp theo hàng 

Đối với mỗi hàng$i$từ 2 đến$n$, chúng ta tính toán một bảng DP mới: 

Cho mỗi chiếc mặt nạ$cur$hợp lệ cho hàng$i$, chúng tôi tổng hợp tất cả các mặt nạ trước đó$prev$tương thích và hợp lệ cho hàng$i-1$. 

Điều này xây dựng tất cả các cấu hình từng phần hợp lệ theo từng hàng. 

## 6. Tổng hợp cuối cùng 

Câu trả lời là tổng của tất cả các giá trị DP ở hàng cuối cùng. 

### Tại sao nó hoạt động 

Ở mỗi bước, trạng thái DP thể hiện chính xác số cách để điền vào tất cả các hàng cho đến hàng hiện tại sao cho: 

hàng hiện tại được cố định thành mặt nạ hợp lệ và tất cả các ràng buộc kề trước đó đều được thỏa mãn. Quá trình chuyển đổi duy trì cả giá trị theo chiều ngang (bằng cách xây dựng mặt nạ) và giá trị theo chiều dọc (bằng cách lọc tương thích). Vì mỗi lưới đầy đủ hợp lệ tương ứng với chính xác một chuỗi mặt nạ hàng nên không có cấu hình nào bị bỏ sót hoặc bị tính gấp đôi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    max_mask = 1 << m
    valid = []
    pop = [0] * max_mask

    for mask in range(max_mask):
        if mask & (mask << 1):
            continue
        pop[mask] = bin(mask).count("1")
        valid.append(mask)

    # group by popcount
    by_pop = [[] for _ in range(m + 1)]
    for mask in valid:
        by_pop[pop[mask]].append(mask)

    # precompute compatibility
    compat = {}
    for x in valid:
        compat[x] = []
        for y in valid:
            if x & y == 0:
                compat[x].append(y)

    # initial dp
    first = a[0]
    dp = {mask: 0 for mask in valid}
    for mask in by_pop[first]:
        dp[mask] = 1

    # transitions
    for i in range(1, n):
        need = a[i]
        new_dp = {mask: 0 for mask in valid}
        for cur in by_pop[need]:
            total = 0
            for prev in compat[cur]:
                total = (total + dp[prev]) % MOD
            new_dp[cur] = total
        dp = new_d_
```
