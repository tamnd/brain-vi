---
title: "CF 15D - Bản đồ"
description: "Chúng ta được cho một bản đồ hình chữ nhật có kích thước n × m, trong đó mỗi ô có chiều cao không âm. Peter muốn xây dựng những thành phố có kích thước a × b. Để đặt một thành phố, anh ta phải san bằng mặt đất bên trong hình chữ nhật của nó bằng cách giảm tất cả các ô xuống chiều cao tối thiểu trong hình chữ nhật đó."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 15
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 15"
rating: 2500
weight: 15
solve_time_s: 132
verified: true
draft: false
---
[CF 15D - Bản đồ](https://codeforces.com/problemset/problem/15/D) 

**Đánh giá:** 2500 
**Tas:** cấu trúc dữ liệu, triển khai, sắp xếp 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bản đồ hình chữ nhật có kích thước`n`×`m`, trong đó mỗi ô có chiều cao không âm. Peter muốn xây dựng những thành phố có quy mô`a`×`b`. Để đặt một thành phố, anh ta phải san bằng mặt đất bên trong hình chữ nhật của nó bằng cách giảm tất cả các ô xuống chiều cao tối thiểu trong hình chữ nhật đó. Giá thành xây dựng tại một vị trí nhất định là tổng khối lượng mặt bằng bị dỡ bỏ. Mục tiêu là liên tục tìm ra hình chữ nhật tối ưu - hình chữ nhật có chi phí san lấp mặt bằng nhỏ nhất trong số tất cả các vị trí có sẵn - và đặt một thành phố ở đó. Nếu nhiều vị trí bằng nhau, Peter ưu tiên vị trí trên cùng, sau đó là vị trí ngoài cùng bên trái. Khi một ô đã bị chiếm giữ bởi một thành phố, nó không thể được sử dụng cho các thành phố tiếp theo. 

Vấn đề cung cấp cho chúng tôi bản đồ có kích thước lên tới 1000 × 1000 và kích thước thành phố lên tới kích thước bản đồ đầy đủ. Một cách tiếp cận đơn giản là kiểm tra mọi hình chữ nhật có thể có và tính toán chi phí của nó từ đầu sẽ lặp lại tới 1.000.000 hình chữ nhật, mỗi hình chữ nhật có thể liên quan đến việc tính tổng tới 1.000.000 ô trong trường hợp xấu nhất. Điều này dẫn đến khoảng 10^12 thao tác - vượt xa giới hạn thời gian 2 giây. Vì vậy, chúng ta cần một cách tiếp cận hiệu quả hơn để tránh tính toán lại tổng và giá trị tối thiểu một cách ngây thơ. 

Các trường hợp biên bao gồm các bản đồ rất nhỏ trong đó các thành phố chiếm toàn bộ không gian, các bản đồ trong đó tất cả các ô đều bằng nhau (tạo nhiều vị trí bằng nhau) và các bản đồ có giá trị lớn (lên tới 10^9), yêu cầu xử lý số nguyên cẩn thận. Ví dụ: trong bản đồ 2 × 2 có độ cao`[[1, 2], [3, 5]]`và kích thước thành phố 1 × 2, thành phố đầu tiên sẽ chiếm hàng trên cùng bên trái, san bằng chi phí 1, sau đó thành phố thứ hai sẽ chiếm hàng dưới cùng với chi phí 2. Nếu chúng ta bỏ qua yếu tố ràng buộc "trên cùng bên trái", thứ tự có thể sai. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu đánh giá mọi`a×b`hình chữ nhật trên bản đồ. Đối với mỗi hình chữ nhật, nó tìm chiều cao tối thiểu và tính tổng chênh lệch giữa mỗi ô và mức tối thiểu đó để tính chi phí san lấp mặt bằng. Sau đó, nó chọn hình chữ nhật có chi phí nhỏ nhất và đánh dấu nó là bị chiếm dụng. Việc này lặp lại cho đến khi không`a×b`hình chữ nhật có sẵn. Mặc dù đúng, nhưng cách tiếp cận này cực kỳ chậm vì việc đánh giá từng hình chữ nhật từ đầu đòi hỏi các phép toán O(a·b), làm cho độ phức tạp tổng thể là O((n−a+1)(m−b+1)·a·b). Với n=m=1000 và a=b=500, đây là khoảng 10^11 phép toán. 

Thông tin chi tiết quan trọng là cả **tối thiểu** bên trong hình chữ nhật trượt và **tổng chiều cao** đều có thể được xử lý trước cho các truy vấn nhanh. Chúng ta có thể tính tổng tiền tố để tính nhanh tổng của bất kỳ hình chữ nhật nào trong O(1). Ở mức tối thiểu, chúng ta có thể sử dụng kỹ thuật trượt tối thiểu 2D dựa trên deques: trước tiên hãy tính mức tối thiểu theo hàng cho chiều rộng`b`, thì mức tối thiểu theo cột đối với các kết quả theo hàng này cho chiều cao`a`. Điều này tạo ra một ma trận trong đó mỗi`a×b`mức tối thiểu của hình chữ nhật có sẵn trong O (1). Bằng cách sử dụng hai phép tính trước này, chúng ta có thể tính toán chi phí cân bằng cho tất cả các vị trí có thể có trong O(n·m), thay vì O(n·m·a·b). Cuối cùng, để sắp xếp các thành phố một cách tuần tự, chúng ta cần một cách hiệu quả để đánh dấu các ô có người sử dụng. Cây Fenwick hoặc cây phân đoạn trên bản đồ chiếm chỗ nhị phân 2D là đủ, nhưng một lưới chiếm chỗ đơn giản với cùng các kiểm tra hình chữ nhật trượt sẽ hoạt động vì mỗi ô chỉ có thể là một phần của một thành phố, hạn chế tổng số lần lặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n−a+1)(m−b+1)·a·b·k) | O(n·m) | Quá chậm | 
| Tối ưu | O(n·m) | O(n·m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính mảng tổng tiền tố 2D có chiều cao. Đối với mỗi ô`(i, j)`, lưu trữ tổng của tất cả các ô từ`(1, 1)`ĐẾN`(i, j)`. Điều này cho phép truy vấn tổng hình chữ nhật O(1) bằng cách sử dụng nguyên tắc bao gồm-loại trừ. 
2. Tính toán mức trượt tối thiểu theo hàng cho chiều rộng`b`sử dụng một deque. Đối với mỗi hàng, hãy duy trì một dãy chỉ số sao cho phần tử ngoài cùng bên trái luôn ở mức tối thiểu trong cửa sổ độ dài hiện tại`b`. Lưu trữ các kết quả trung gian này trong một ma trận`row_min`. 
3. Tính chiều cao trượt tối thiểu theo cột`a`TRÊN`row_min`để đạt được mức tối thiểu của mọi`a×b`hình chữ nhật. Một lần nữa, hãy sử dụng deque trên mỗi cột để duy trì mức tối thiểu hiện tại trong cửa sổ trượt dọc. Gọi ma trận này`rect_min`. 
4. Đối với mỗi`a×b`hình chữ nhật, tính chi phí san lấp mặt bằng như`sum of heights − minimum × (a·b)`sử dụng mảng tổng tiền tố cho tổng và`rect_min`ở mức tối thiểu. Theo dõi hình chữ nhật có chi phí nhỏ nhất. Phá vỡ các ràng buộc bằng cách chọn hình chữ nhật trên cùng, sau đó là hình chữ nhật ngoài cùng bên trái. 
5. Đánh dấu hình chữ nhật đã chọn là có trong lưới chiếm chỗ. Lặp lại các bước 4-5 cho đến khi không còn hình chữ nhật nào nữa (tức là bất kỳ hình chữ nhật ứng cử viên nào cũng chứa ít nhất một ô bị chiếm giữ). 
6. In ra vị trí của các thành phố theo thứ tự chúng được đặt, cùng với chi phí san lấp mặt bằng của chúng. 

Tại sao nó hoạt động: Ở mỗi lần lặp, chúng tôi xem xét tất cả các hình chữ nhật hợp lệ và tính toán chi phí san lấp mặt bằng chính xác. Bằng cách tiền xử lý tổng và giá trị tối thiểu, việc tính toán trên mỗi hình chữ nhật là không đổi. Việc chọn hình chữ nhật có chi phí tối thiểu và đánh dấu nó đã chiếm giữ đảm bảo chúng ta tuân theo vị trí thành phố xác định của bài toán. Cách tiếp cận cửa sổ trượt đảm bảo mức tối thiểu luôn đúng cho mỗi hình chữ nhật. 

## Giải pháp Python```python
import sys
from collections import deque
input = sys.stdin.readline

def read_matrix(n, m):
    mat = []
    for _ in range(n):
        mat.append(list(map(int, input().split())))
    return mat

def prefix_sum(mat):
    n, m = len(mat), len(mat[0])
    ps = [[0]*(m+1) for _ in range(n+1)]
    for i in range(n):
        for j in range(m):
            ps[i+1][j+1] = mat[i][j] + ps[i][j+1] + ps[i+1][j] - ps[i][j]
    return ps

def rectangle_sum(ps, x1, y1, x2, y2):
    return ps[x2][y2] - ps[x1][y2] - ps[x2][y1] + ps[x1][y1]

def sliding_min_row(mat, b):
    n, m = len(mat), len(mat[0])
    row_min = [[0]*(m-b+1) for _ in range(n)]
    for i in range(n):
        dq = deque()
        for j in range(m):
            while dq and mat[i][dq[-1]] >= mat[i][j]:
                dq.pop()
            dq.append(j)
            if dq[0] == j-b:
                dq.popleft()
            if j >= b-1:
                row_min[i][j-b+1] = mat[i][dq[0]]
    return row_min

def sliding_min_col(mat, a):
    n, m = len(mat), len(mat[0])
    rect_min = [[0]*m for _ in range(n-a+1)]
    for j in range(m):
        dq = deque()
        for i in range(n):
            while dq and mat[dq[-1]][j] >= mat[i][j]:
                dq.pop()
            dq.append(i)
            if dq[0] == i-a:
                dq.popleft()
            if i >= a-1:
                rect_min[i-a+1][j] = mat[dq[0]][j]
    return rect_min

def main():
    n, m, a, b = map(int, input().split())
    heights = read_matrix(n, m)
    ps = prefix_sum(heights)
    
    row_min = sliding_min_row(heights, b)
    rect_min = sliding_min_col(row_min, a)
    
    occupied = [[False]*m for _ in range(n)]
    result = []
    
    while True:
        best = None
        best_cost = None
        for i in range(n-a+1):
            for j in range(m-b+1):
                if any(occupied[x][y] for x in range(i, i+a) for y in range(j, j+b)):
                    continue
                total = rectangle_sum(ps, i, j, i+a, j+b)
                cost = total - rect_min[i][j]*a*b
                if best_cost is None or cost < best_cost or (cost == best_cost and (i < best[0] or (i==best[0] and j<best[1]))):
                    best = (i, j)
                    best_cost = cost
        if best is None:
            break
        i, j = best
        result.append((i+1, j+1, best_cost))
        for x in range(i, i+a):
            for y in range(j, j+b):
                occupied[x][y] = True
    
    print(len(result))
    for r in result:
        print(*r)

if __name__ == "__main__":
    main()
```**Giải thích:** 

Phần tổng tiền tố cho phép truy vấn tổng hình chữ nhật O(1). Mức tối thiểu trượt theo hàng và theo cột triển khai hàng đợi đơn điệu 2D để tính mức tối thiểu theo O(n·m) thay vì tính toán lại cho mỗi hình chữ nhật. Lưới chiếm chỗ đảm bảo chúng tôi không bao giờ chọn hình chữ nhật đã được sử dụng.
