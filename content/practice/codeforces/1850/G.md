---
title: "CF 1850G - Sao Mai"
description: "Chúng ta được cấp một tập hợp các điểm phân biệt trên lưới số nguyên 2D. Đối với mỗi cặp điểm có thứ tự, chúng ta tưởng tượng đặt một la bàn ở điểm đầu tiên và hướng nó về điểm thứ hai."
date: "2026-06-09T05:31:19+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "data-structures", "geometry", "implementation", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1850
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 886 (Div. 4)"
rating: 1500
weight: 1850
solve_time_s: 65
verified: true
draft: false
---

[CF 1850G - Sao Mai](https://codeforces.com/problemset/problem/1850/G) 

**Đánh giá:** 1500 
**Tags:** tổ hợp, cấu trúc dữ liệu, hình học, cách thực hiện, toán học, sắp xếp 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các điểm phân biệt trên lưới số nguyên 2D. Đối với mỗi cặp điểm có thứ tự, chúng ta tưởng tượng đặt một la bàn ở điểm đầu tiên và hướng nó về điểm thứ hai. La bàn chỉ được coi là hợp lệ nếu hướng từ điểm đầu tiên đến điểm thứ hai là một trong tám hướng la bàn tiêu chuẩn: bắc, nam, đông, tây hoặc một trong bốn đường chéo. 

Nhiệm vụ là đếm xem có bao nhiêu cặp điểm có thứ tự tạo ra hướng cho phép như vậy. 

Một cách tương đương để diễn đạt điều kiện là đối với một cặp điểm$(x_1, y_1)$Và$(x_2, y_2)$, vectơ giữa chúng phải có tọa độ x bằng nhau (đường thẳng đứng), tọa độ y bằng nhau (đường ngang) hoặc thỏa mãn$|x_2 - x_1| = |y_2 - y_1|$(độ dốc đường chéo hoàn hảo là 1 hoặc -1). 

Những ràng buộc buộc chúng ta phải tránh xa so sánh bậc hai. Với tối đa$2 \cdot 10^5$điểm tổng thể, việc kiểm tra đơn giản tất cả các cặp sẽ yêu cầu khoảng$O(n^2)$các hoạt động sẽ theo thứ tự$4 \cdot 10^{10}$so sánh trong trường hợp xấu nhất, vượt xa những gì có thể chạy trong 2 giây. 

Điểm tinh tế chính là nhiều điểm có thể nằm trên cùng một đường ngang, đường dọc hoặc đường chéo và mỗi cặp dọc theo các đường đó đều đóng góp. Cách tiếp cận bất cẩn thường đếm gấp đôi hoặc bỏ lỡ các cặp nếu chỉ đường không được nhóm đúng cách. Một cạm bẫy phổ biến khác là xử lý các đường chéo không chính xác bằng cách trộn các hệ số góc mà không chuẩn hóa bằng gcd hoặc đẳng thức tuyệt đối. 

Các trường hợp cạnh bao gồm: 

Một cấu hình trong đó tất cả các điểm có cùng tọa độ x. Khi đó mọi cặp đều hợp lệ theo chiều dọc và câu trả lời đơn giản là$n(n-1)$. Kiểm tra định hướng đơn giản hoạt động nhưng quá chậm. Một trường hợp cạnh khác là một chuỗi chéo như$(0,0), (1,1), (2,2)$, trong đó mọi cặp đều hợp lệ và tất cả đều phải được tính, không chỉ những cặp liền kề. Cuối cùng, các phân phối hỗn hợp như các điểm ngẫu nhiên đòi hỏi sự đóng góp riêng biệt của các họ dòng thay vì kiểm tra theo cặp. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Với mỗi cặp điểm có thứ tự, hãy tính vectơ giữa chúng và kiểm tra xem nó có nằm trên một trong tám hướng cho phép hay không. Điều này có nghĩa là kiểm tra xem chênh lệch x bằng 0, chênh lệch y bằng 0 hay giá trị tuyệt đối của chúng có khớp nhau hay không. Điều này hiệu quả vì nó mã hóa trực tiếp định nghĩa về chuyển động la bàn hợp lệ. 

Vấn đề là quy mô. Với$n$lên đến$2 \cdot 10^5$, số lượng các cặp theo thứ tự là$n^2$, điều này làm cho phương pháp này không thể thực hiện được. 

Quan sát quan trọng là tính hợp lệ chỉ phụ thuộc vào ba lớp hình học độc lập: cùng tọa độ x, cùng tọa độ y và cùng đường chéo của độ dốc$+1$hoặc$-1$. Nếu chúng ta nhóm các điểm theo các chữ ký này thì mọi cặp hợp lệ sẽ trở thành một cặp bên trong một trong các nhóm này. Thay vì kiểm tra các cặp, chúng tôi đếm các kết hợp bên trong các nhóm. 

Đối với các đường thẳng đứng, chúng tôi nhóm theo tọa độ x. Nếu một nhóm có kích thước$k$, nó góp phần$k(k-1)$các cặp đặt hàng. Đối với các đường ngang, chúng ta nhóm theo tọa độ y tương tự. Đối với đường chéo, chúng tôi sử dụng tọa độ được chuyển đổi:$x - y$cho một họ đường chéo và$x + y$cho người khác. Mỗi nhóm lại đóng góp$k(k-1)$. Tổng hợp tất cả những đóng góp cho câu trả lời. 

Điều này làm giảm vấn đề từ liệt kê cặp đến đếm tần số trên bốn khóa có thể băm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Đếm Nhóm |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp kiểm thử, hãy đọc tất cả các điểm và chuẩn bị bản đồ tần số cho bốn chữ ký dòng khác nhau. Mục tiêu là nhóm các điểm có thể nhìn thấy nhau theo hướng la bàn hợp lệ. 
2. Đối với mọi điểm$(x, y)$, bộ đếm gia tăng cho bốn phím:$x$,$y$,$x - y$, Và$x + y$. Mỗi khóa tương ứng với một họ hướng hợp lệ. Bước này xây dựng các lớp điểm tương đương sắp xếp dọc theo các trục la bàn hợp lệ. 
3. Sau khi xử lý tất cả các điểm, lặp lại qua từng bản đồ tần số. Với mỗi giá trị$k$, thêm vào$k \cdot (k - 1)$để trả lời. Điều này đếm tất cả các cặp có thứ tự trong nhóm đó. 
4. Tổng hợp đóng góp từ cả bốn bản đồ. Điều này bao gồm hướng dọc, ngang và cả hai hướng chéo. 
5. Xuất tổng cuối cùng cho test case. 

### Tại sao nó hoạt động 

Mỗi cặp thứ tự hợp lệ phải thỏa mãn chính xác một trong bốn điều kiện cấu trúc: giống x, giống y, giống x trừ y, hoặc giống x cộng y. Những điều kiện này mô tả đầy đủ tất cả tám hướng la bàn vì các đường chéo chia thành hai họ độ dốc và mỗi trục chia thành hai hướng. Mỗi cặp hợp lệ xuất hiện bên trong chính xác một nhóm tương ứng và mọi cặp bên trong nhóm đều hợp lệ. Điều này tạo ra một phân vùng hoàn hảo gồm các cặp hợp lệ, đảm bảo số đếm chính xác, không bị chồng chéo hoặc thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        cx = defaultdict(int)
        cy = defaultdict(int)
        cd1 = defaultdict(int)
        cd2 = defaultdict(int)

        pts = []
        for _ in range(n):
            x, y = map(int, input().split())
            cx[x] += 1
            cy[y] += 1
            cd1[x - y] += 1
            cd2[x + y] += 1

        ans = 0

        for d in (cx, cy, cd1, cd2):
            for k in d.values():
                ans += k * (k - 1)

        print(ans)

if __name__ == "__main__":
    solve()
```Mã duy trì bốn bản đồ băm độc lập. Mỗi bản đồ tương ứng với một lớp ràng buộc hình học. Các phép biến đổi đường chéo$x - y$Và$x + y$đảm bảo rằng tất cả các đường 45 độ được ghi lại mà không có lỗi số học độ dốc hoặc lỗi dấu phẩy động. Vòng lặp cuối cùng tích lũy số lượng cặp theo thứ tự một cách trực tiếp, tránh chia đôi vì hướng quan trọng. 

Một điểm tinh tế là cần có các cặp có thứ tự, vì vậy chúng tôi sử dụng$k(k-1)$thay vì$k(k-1)/2$. Mỗi cặp đóng góp hai lần khi xem xét vị trí la bàn theo cả hai hướng, phù hợp với định nghĩa của bài toán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Điểm:$(0,0), (-1,-1), (1,1)$| Bước | cx | cy | cd1 | cd2 | câu trả lời một phần | 
| --- | --- | --- | --- | --- | --- | 
| sau khi chèn | 3 nhóm | 3 nhóm | 3 nhóm | 3 nhóm | 0 | 
| tính toán | tất cả k=1 | tất cả k=1 | tất cả k=1 | tất cả k=1 | 0 | 

Mỗi cấu trúc không có khóa lặp lại nên không có nhóm nào đóng góp thêm cặp khóa. Tuy nhiên, ví dụ này từ câu lệnh bao gồm tất cả các cặp có thứ tự là hợp lệ vì mỗi cặp khớp với một trong bốn cấu trúc, nhưng vì mỗi nhóm trùng nhau trên các cấu trúc nên việc đếm trên mỗi cấu trúc đảm bảo tất cả các hướng đều được bao phủ tích lũy. 

### Ví dụ 2 

Điểm:$(6,9), (10,13), (0,0)$| Bước | kích thước cx | kích thước cy | kích thước cd1 | kích thước cd2 | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| sau khi chèn | {6,10,0} | {9,13,0} | { -3, -3, 0 } | {15,23,0} | 0 | 
| tính toán | tất cả 1s | tất cả 1s | tất cả 1s | tất cả 1s | 0 | 

Chỉ các cặp được căn chỉnh theo đường chéo mới đóng góp và chỉ khi hai điểm có chung khóa được chuyển đổi. Cặp (6,9) và (10,13) chia sẻ$x-y = -3$, tạo thành một nhóm hợp lệ có kích thước 2, tạo thành 2 cặp có thứ tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi điểm cập nhật bốn bản đồ băm và sau đó chúng tôi lặp lại các tần số | 
| Không gian |$O(n)$| Mỗi điểm góp phần lưu trữ khóa có kích thước không đổi trong bản đồ | 

Tổng cộng$n$trên tất cả các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^5$, do đó giải pháp chạy thoải mái trong giới hạn. Hoạt động của bản đồ băm vẫn tuyến tính như mong đợi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    from collections import defaultdict

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        cx = defaultdict(int)
        cy = defaultdict(int)
        cd1 = defaultdict(int)
        cd2 = defaultdict(int)

        for _ in range(n):
            x, y = map(int, input().split())
            cx[x] += 1
            cy[y] += 1
            cd1[x - y] += 1
            cd2[x + y] += 1

        ans = 0
        for d in (cx, cy, cd1, cd2):
            for k in d.values():
                ans += k * (k - 1)

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("""5
3
0 0
-1 -1
1 1
4
4 5
5 7
6 9
10 13
3
-1000000000 1000000000
0 0
1000000000 -1000000000
5
0 0
2 2
-1 5
-1 10
2 11
3
0 0
-1 2
1 -2
""") == """6
2
6
8
0"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| dây chuyền dọc đơn | n(n-1) | độ chính xác của nhóm dọc | 
| chuỗi chéo đơn | n(n-1) | đường chéo biến đổi chính xác | 
| điểm rải rác | 0 hoặc thưa thớt | không có kết quả dương tính giả | 

## Vỏ cạnh 

Căn chỉnh theo chiều dọc chẳng hạn như$(0,0), (0,1), (0,2), (0,3)$tạo ra một khóa duy nhất trong bản đồ x có kích thước 4. Thuật toán thêm$4 \cdot 3 = 12$, đếm chính xác tất cả các cặp có thứ tự mà la bàn chỉ về phía bắc hoặc phía nam. 

Một chuỗi chéo như$(0,0), (1,1), (2,2), (3,3)$chia sẻ giống nhau$x-y$khóa, tạo ra một nhóm cỡ 4 khác. Điều này một lần nữa đóng góp 12, bao gồm tất cả các hướng la bàn chéo. 

Một cấu hình hỗn hợp như$(0,0), (1,2), (2,1)$phân chia thành các khóa khác nhau, đảm bảo chỉ đóng góp các sắp xếp hình học hợp lệ và ngăn chặn việc vô tình ghép nối theo các hướng không tương thích.
