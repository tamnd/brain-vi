---
title: "CF 104872F - Hình vuông kỳ diệu"
description: "Chúng ta có một lưới $n nhân n$ ban đầu chứa một hoán vị hoàn hảo của các số từ $1$ đến $n^2$. Thuộc tính xác định của lưới ban đầu là tổng của mỗi hàng bằng cùng một giá trị và tổng của mỗi cột cũng bằng cùng một giá trị đó."
date: "2026-06-28T10:27:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104872
codeforces_index: "F"
codeforces_contest_name: "2023-2024 Russia Team Open, High School Programming Contest (VKOSHP XXIV)"
rating: 0
weight: 104872
solve_time_s: 79
verified: false
draft: false
---

[CF 104872F - Hình vuông ma thuật](https://codeforces.com/problemset/problem/104872/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một$n \times n$lưới ban đầu chứa một hoán vị hoàn hảo của các số từ$1$ĐẾN$n^2$. Thuộc tính xác định của lưới ban đầu là tổng của mỗi hàng bằng cùng một giá trị và tổng của mỗi cột cũng bằng cùng một giá trị đó. Cấu trúc này bị phá hủy chỉ bằng một thao tác: hai ô đã bị hoán đổi giá trị. 

Nhiệm vụ là xác định tọa độ của hai ô được hoán đổi. Nếu chúng ta hoán đổi lại hai giá trị đó, lưới sẽ lại trở thành lưới "ma thuật" hợp lệ, nghĩa là tất cả tổng hàng và tổng cột đều bằng nhau. 

Hạn chế chính đó là$n$có thể lớn tới 1000, do đó lưới chứa tới một triệu ô. Bất kỳ giải pháp nào tính toán lại các thuộc tính hàng hoặc cột cho nhiều giao dịch hoán đổi ứng viên đều phải hoạt động hiệu quả theo thời gian tuyến tính trên lưới. Một bậc hai hoặc thậm chí$O(n^3)$mô phỏng các giao dịch hoán đổi là không khả thi, nhưng bất cứ điều gì dựa trên một lần chuyển qua lưới đều có thể chấp nhận được. 

Một điểm tinh tế là tất cả các giá trị đều khác biệt, do đó mỗi giá trị xác định duy nhất vị trí của nó. Điều này giúp loại bỏ sự mơ hồ: nếu một giá trị sai trong tổng hàng thì nó có thể được truy tìm đến một vị trí duy nhất. 

Một quan sát quan trọng khác là lưới khác với hình vuông ma thuật hợp lệ đúng hai ô. Điều đó có nghĩa là tất cả các vi phạm về cấu trúc đều được bản địa hóa và mọi hàng và cột đều chính xác ngoại trừ những vi phạm bị ảnh hưởng bởi các vị trí bị hoán đổi. 

Một trường hợp thất bại đơn giản phát sinh nếu người ta giả sử chỉ cần kiểm tra hàng hoặc chỉ cột. Hãy xem xét một phép hoán đổi giữ nguyên tổng hàng nhưng phá vỡ tổng cột hoặc ngược lại; những trường hợp như vậy buộc chúng ta phải suy luận về cả hai chiều cùng một lúc. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là xem xét từng cặp ô, hoán đổi chúng và kiểm tra xem lưới kết quả có trở thành ma thuật hay không. Đối với mỗi cặp, tính toán lại tổng chi phí của hàng và cột$O(n^2)$, và có$O(n^4)$cặp. Điều này dẫn đến sự phức tạp lớn về mặt thiên văn của$O(n^6)$, điều này hoàn toàn không khả thi ngay cả đối với những$n$. 

Chúng ta cần tránh mô phỏng các giao dịch hoán đổi. Cấu trúc của vấn đề gợi ý nên xem xét thông tin tổng hợp thay vì cấu hình. Trong một lưới ma thuật chính xác, tổng mỗi hàng bằng một hằng số$S$và tổng mỗi cột cũng bằng$S$. Nếu việc hoán đổi xảy ra, chính xác hai hàng và hai cột sẽ có tổng không chính xác, vì chỉ những hàng chứa các ô được hoán đổi mới bị ảnh hưởng. 

Điều này giúp giảm bớt vấn đề trong việc xác định hàng và cột nào “mất cân bằng” và sau đó định vị chính xác các ô chịu trách nhiệm. Vì các giá trị là khác nhau nên chúng ta có thể tính tổng kỳ vọng và so sánh chúng với tổng thực tế trong$O(n^2)$, sau đó suy ra hai hàng và cột bị ảnh hưởng. 

Khi chúng ta tách biệt hai hàng xấu và hai cột xấu, các phần tử được hoán đổi phải nằm ở giao điểm của chúng. Điều này làm giảm không gian ứng viên xuống tối đa bốn ô và chúng tôi có thể xác định cặp chính xác bằng cách kiểm tra xem hoán đổi nào khôi phục tính nhất quán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra hoán đổi vũ phu |$O(n^6)$|$O(1)$| Quá chậm | 
| Phân tích độ lệch hàng/cột |$O(n^2)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Tính tổng kỳ diệu dự kiến$S = \frac{n(n^2+1)}{2}$. Đây là tổng mỗi hàng và cột phải có trong một lưới chính xác vì lưới là một hoán vị của$1 \ldots n^2$. 
2. Tính tổng tất cả các hàng trong một lần duyệt qua lưới. Xác định các hàng có tổng khác với$S$. Đây là các hàng ứng viên có chứa một trong các phần tử được hoán đổi. 
3. Tính tổng các cột theo cách tương tự và xác định các cột có tổng khác nhau$S$. Đây là các cột ứng viên có chứa các phần tử được hoán đổi. 
4. Về bản chất của một lần hoán đổi, chính xác hai hàng và chính xác hai cột sẽ không chính xác. Gọi cho họ$r_1, r_2$Và$c_1, c_2$. Điều này xảy ra vì mỗi ô được hoán đổi sẽ ảnh hưởng đến chính xác một hàng và một cột. 
5. Xét bốn ô giao nhau:$(r_1,c_1), (r_1,c_2), (r_2,c_1), (r_2,c_2)$. Đây là những vị trí duy nhất có thể chứa các giá trị hoán đổi. 
6. Hãy thử hoán đổi giá trị của bất kỳ cặp nào trong số bốn ứng cử viên này và kiểm tra xem tổng hàng và cột có bằng nhau không$S$. Chính xác một lần hoán đổi sẽ thỏa mãn điều kiện này. 
7. Xuất tọa độ của cặp hợp lệ. 

### Tại sao nó hoạt động 

Mỗi ô được hoán đổi góp phần tạo ra một lỗi cộng vào chính xác một tổng hàng và một tổng cột. Vì có chính xác hai ô được hoán đổi, số lượng hàng và cột bị ảnh hưởng không thể vượt quá hai mỗi ô và không thể ít hơn hai trừ khi cả hai ô được hoán đổi đều nằm trong cùng một hàng hoặc cột, điều này vẫn sẽ tạo ra chính xác một mẫu mất cân bằng hàng hoặc cột nhất quán với cùng một logic giao nhau. Hạn chế này buộc tất cả cấu trúc không chính xác phải tập trung vào tối đa bốn tọa độ và không ô nào khác có thể ảnh hưởng đến tổng một cách độc lập. Do đó, việc hạn chế sự chú ý đến giao điểm của các hàng và cột bất thường là đủ để khôi phục lại hoán đổi ban đầu một cách duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = [list(map(int, input().split())) for _ in range(n)]

    S = n * (n * n + 1) // 2

    row_sum = [0] * n
    col_sum = [0] * n

    for i in range(n):
        for j in range(n):
            v = a[i][j]
            row_sum[i] += v
            col_sum[j] += v

    bad_rows = [i for i in range(n) if row_sum[i] != S]
    bad_cols = [j for j in range(n) if col_sum[j] != S]

    if len(bad_rows) == 1:
        bad_rows.append(bad_rows[0])
    if len(bad_cols) == 1:
        bad_cols.append(bad_cols[0])

    r1, r2 = bad_rows[0], bad_rows[1]
    c1, c2 = bad_cols[0], bad_cols[1]

    def check_swap(x1, y1, x2, y2):
        # simulate swap effect locally by computing affected rows/cols
        rs = row_sum[:]
        cs = col_sum[:]

        v1 = a[x1][y1]
        v2 = a[x2][y2]

        rs[x1] += v2 - v1
        rs[x2] += v1 - v2
        cs[y1] += v2 - v1
        cs[y2] += v1 - v2

        return all(rs[i] == S for i in range(n)) and all(cs[j] == S for j in range(n))

    candidates = [
        (r1, c1, r2, c2),
        (r1, c2, r2, c1)
    ]

    for x1, y1, x2, y2 in candidates:
        if check_swap(x1, y1, x2, y2):
            print(x1 + 1, y1 + 1)
            print(x2 + 1, y2 + 1)
            return

solve()
```Quá trình triển khai bắt đầu bằng cách đọc lưới và tính tổng hàng và cột trong một lượt. Đây là (O(
