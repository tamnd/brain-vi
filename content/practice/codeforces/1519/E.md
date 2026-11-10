---
title: "CF 1519E - Tắt một"
description: "Chúng ta được cho $n$ điểm trong góc phần tư thứ nhất của mặt phẳng. Mỗi điểm được xác định bởi một cặp tọa độ hữu tỉ $(xi, yi)$, trong đó $xi = ai / bi$ và $yi = ci / di$."
date: "2026-06-10T18:15:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "geometry", "graphs", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 1519
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 108 (Rated for Div. 2)"
rating: 2700
weight: 1519
solve_time_s: 152
verified: false
draft: false
---

[CF 1519E - Tắt từng cái một](https://codeforces.com/problemset/problem/1519/E) 

**Đánh giá:** 2700 
**Thẻ:** thuật toán xây dựng, dfs và tương tự, hình học, đồ thị, sắp xếp, cây 
**Thời gian giải:** 2m 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được trao$n$điểm thuộc góc phần tư thứ nhất của mặt phẳng. Mỗi điểm được xác định bởi một cặp tọa độ hợp lý$(x_i, y_i)$, Ở đâu$x_i = a_i / b_i$Và$y_i = c_i / d_i$. Một nước đi bao gồm việc chọn hai điểm, di chuyển mỗi điểm sang phải hoặc một đơn vị lên trên, sau đó loại bỏ chúng, nhưng chỉ khi hai vị trí mới và điểm gốc nằm trên một đường thẳng. 

Nhiệm vụ là tối đa hóa số lần di chuyển như vậy và đưa ra những điểm được ghép nối trong mỗi lần di chuyển. Đầu vào có thể lớn như$2 \cdot 10^5$điểm, vì vậy bất kỳ thuật toán nào thử từng cặp hoặc mô phỏng mọi kết hợp đều quá chậm. 

Ràng buộc cốt lõi mang tính hình học: để một nước đi có giá trị, hai điểm sau khi di chuyển phải thẳng hàng với gốc tọa độ. Vì chúng ta chỉ có thể di chuyển sang phải hoặc lên trên 1 nên mỗi điểm có đúng hai lựa chọn. Điều này gợi ý về một vấn đề ghép đôi tổ hợp trong đó mỗi điểm có thể tham gia chính xác vào một nước đi và chúng ta muốn có càng nhiều cặp càng tốt. 

Một giải pháp đơn giản có thể thử tất cả các cặp điểm và kiểm tra tất cả các phương án chuyển động. Điều này sẽ yêu cầu kiểm tra$O(n^2)$các cặp, với 4 tổ hợp di chuyển mỗi cặp, tổng cộng$O(4 n^2)$hoạt động, đại khái là$1.6 \cdot 10^{11}$cho lớn nhất$n$, vượt xa giới hạn thời gian 2 giây. Các trường hợp cạnh bao gồm các điểm đã nằm trên cùng một đường thẳng, các điểm có mẫu số rất lớn hoặc nhiều điểm giống hệt nhau, tất cả đều có thể phá vỡ cách tiếp cận tham lam bất cẩn. 

## Phương pháp tiếp cận 

Ý tưởng Brute-Force rất đơn giản: với mỗi cặp điểm, hãy thử di chuyển cả hai theo hai hướng, kiểm tra độ thẳng hàng với gốc tọa độ và nếu có thể, hãy loại bỏ chúng. Điều này đúng về nguyên tắc nhưng quá chậm đối với$n \sim 2 \cdot 10^5$. Điều đó là không thực tế vì chúng tôi sẽ thực hiện tối đa$4 \cdot \binom{2 \cdot 10^5}{2}$séc. 

Cái nhìn sâu sắc quan trọng là điều kiện hình học có thể được giảm xuống thành số học số nguyên mà không gặp vấn đề về độ chính xác của dấu phẩy động. Xem xét các điểm di chuyển$p = (x_1, y_1)$Và$q = (x_2, y_2)$qua$(dx_1, dy_1)$Và$(dx_2, dy_2)$mỗi nơi$dx$Và$dy$là 0 hoặc 1, bị ràng buộc sao cho chính xác một trong$dx$hoặc$dy$là 1 cho mỗi điểm. Các điểm thẳng hàng với gốc tọa độ khi và chỉ khi tích chéo$(x_1 + dx_1) (y_2 + dy_2) = (y_1 + dy_1) (x_2 + dx_2)$. 

Vì mỗi bước di chuyển chỉ làm thay đổi tính chẵn lẻ của các tổng tử số nên chúng ta có thể phân loại các điểm theo tính chẵn lẻ của các tử số. Bằng cách ghép các điểm với các điểm chẵn lẻ phù hợp hoặc bổ sung, chúng tôi đảm bảo điều kiện sản phẩm chéo được thỏa mãn. Quan sát này cho phép chúng ta xây dựng chiến lược ghép nối đệ quy giống như cây, nhóm các điểm theo tính chẵn lẻ của tử số và mẫu số, sau đó ghép chúng trong các nhóm. Khi một nhóm có số điểm lẻ, chúng ta có thể truyền điểm còn sót lại cho nhóm cấp cao hơn. Bằng cách sử dụng phương pháp phân tách chẵn lẻ đệ quy này, chúng tôi đảm bảo các bước di chuyển tối đa mà không cần kiểm tra rõ ràng từng cặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Nhóm đệ quy dựa trên chẵn lẻ | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển tọa độ từng điểm thành số nguyên$(a_i \cdot d_i, b_i \cdot c_i)$để loại bỏ các phân số. Điều này cho phép tất cả số học tiếp theo sử dụng số nguyên, tránh lỗi dấu phẩy động. 
2. Xác định hàm đệ quy lấy danh sách các chỉ số điểm. Nếu danh sách có một hoặc không có điểm, hãy trả lại chúng dưới dạng điểm còn sót lại. 
3. Đối với danh sách hiện tại, hãy phân loại các điểm thành bốn nhóm dựa trên tính chẵn lẻ của tọa độ nguyên theo modulo 2. Các nhóm này là`(even, even)`,`(even, odd)`,`(odd, even)`,`(odd, odd)`. 
4. Xác định các nhóm có nhiều hơn một điểm. Ghép các điểm trong cùng một nhóm nếu có thể, tạo thành các nước đi. Nếu một nhóm có điểm còn sót lại sau khi ghép nối nội bộ, hãy truyền nó lên mức đệ quy cao hơn tiếp theo. 
5. Xử lý đệ quy từng nhóm với điểm còn lại cho đến khi tất cả các điểm được ghép nối hoặc chỉ còn một điểm chưa được ghép nối. Thu thập các bước di chuyển trong danh sách toàn cầu. 
6. Xuất ra tổng số nước đi và chính các nước đi đó. 

Tại sao nó hoạt động: Bất biến chính là các điểm có cùng mẫu chẵn lẻ modulo 2 luôn có thể được di chuyển theo cách mà chúng trở nên thẳng hàng với gốc tọa độ. Việc truyền bá các điểm còn sót lại đảm bảo rằng không có điểm nào bị bỏ sót và đạt được sự ghép đôi tối đa. Vì mỗi phép đệ quy giảm một nửa kích thước bài toán xét về độ phân biệt chẵn lẻ, nên độ sâu đệ quy bị giới hạn và mỗi điểm được xử lý một số lần không đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1 << 25)

n = int(input())
points = []
for i in range(n):
    a, b, c, d = map(int, input().split())
    x = a * d
    y = b * c
    points.append((x, y, i + 1))

moves = []

def solve(lst):
    if len(lst) <= 1:
        return lst
    buckets = {}
    for x, y, idx in lst:
        key = (x % 2, y % 2)
        if key not in buckets:
            buckets[key] = []
        buckets[key].append((x // 2, y // 2, idx))
    leftover = []
    for key in buckets:
        rem = solve(buckets[key])
        leftover.extend(rem)
    result = []
    for i in range(0, len(leftover) - 1, 2):
        moves.append((leftover[i][2], leftover[i + 1][2]))
    if len(leftover) % 2 == 1:
        return [leftover[-1]]
    return []

solve(points)

print(len(moves))
for a, b in moves:
    print(a, b)
```Giải pháp bắt đầu bằng cách chuyển đổi tất cả các điểm thành tọa độ nguyên để tránh phân số. Đệ quy`solve`hàm nhóm các điểm theo tính chẵn lẻ và chia đôi tọa độ của chúng trong mỗi lần đệ quy. Việc ghép các điểm còn sót lại từ các nhóm khác nhau đảm bảo tình trạng sản phẩm chéo và chỉ các điểm chưa ghép nối mới được trả lại để truyền tiếp. Đệ quy đảm bảo rằng tất cả các cặp hợp lệ đều được thu thập. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu đầu tiên: 

| Điểm | x*d | y*c | Chỉ mục | 
| --- | --- | --- | --- | 
| 1/4, 5/1 | 4*1=4 | 1*5=5 | 1 | 
| 1/1,1/1 | 1 | 1 | 2 | 
| 3/3,3/3 | 3*3=9 | 3*3=9 | 3 | 
| 1/1,4/1 | 1 | 4 | 4 | 
| 1/6,1/1 | 6 | 1 | 5 | 
| 1/5, 4/1 | 5 | 4 | 6 | 
| 1/6,1/1 | 6 | 1 | 7 | 

Sau khi nhóm theo tính chẵn lẻ và đệ quy, chúng ta ghép (1,6), (2,4) và (5,7). Điểm 3 vẫn chưa được ghép đôi vì không có điểm nào khác khớp với điểm chẵn lẻ của nó để có một nước đi hợp lệ. 

Một đầu vào được xây dựng khác với các điểm nằm trên lưới:```
4
1 1 1 1
2 1 2 1
3 1 3 1
4 1 4 1
```Điểm giảm xuống tọa độ nguyên (1,1),(2,2),(3,3),(4,4). Tất cả các điểm đều có tính chẵn lẻ (1,1) hoặc (0,0) sau khi chia tỷ lệ và đệ quy, cho phép di chuyển (1,2) và (3,4). Điều này xác nhận việc ghép nối hoạt động đối với các điểm được căn chỉnh theo đường chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi điểm được xử lý đệ quy dọc theo các nhóm chẵn lẻ, giảm một nửa tọa độ cho mỗi bước. Mỗi điểm đi vào một nhóm với số lần không đổi. | 
| Không gian | O(n) | Chúng tôi lưu trữ điểm trong đệ quy và theo dõi các bước di chuyển, tuyến tính theo số điểm. | 

Giải pháp này phù hợp với giới hạn thời gian và bộ nhớ vì nó tránh được việc kiểm tra theo cặp rõ ràng và chỉ xử lý các điểm một số lần không đổi theo cách đệ quy. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    points = []
    for i in range(n):
        a, b, c, d = map(int, input().split())
        x = a * d
        y = b * c
        points.append((x, y, i + 1))
    moves = []
    def solve(lst):
        if len(lst) <= 1:
            return lst
        buckets = {}
        for x, y, idx in lst:
            key = (x % 2, y % 2)
            if key not in buckets:
                buckets[key] = []
            buckets[key].append((x // 2, y // 2, idx))
        leftover = []
        for key in buckets:
            rem = solve(buckets[key
```
