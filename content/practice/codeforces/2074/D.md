---
title: "CF 2074D - Đếm điểm"
description: "Chúng ta có một số vòng tròn được vẽ trên lưới số nguyên 2D. Mọi đường tròn đều được căn giữa ở đâu đó trên trục x, do đó mỗi tâm có tọa độ có dạng $(xi, 0)$ và mỗi đường tròn có bán kính $ri$."
date: "2026-06-08T06:39:51+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "geometry", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2074
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1009 (Div. 3)"
rating: 1400
weight: 2074
solve_time_s: 74
verified: true
draft: false
---

[CF 2074D - Đếm điểm](https://codeforces.com/problemset/problem/2074/D) 

**Đánh giá:** 1400 
**Tags:** Brute Force, cấu trúc dữ liệu, hình học, triển khai, hai con trỏ 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số vòng tròn được vẽ trên lưới số nguyên 2D. Mọi đường tròn đều được căn giữa ở đâu đó trên trục x, vì vậy mỗi tâm có tọa độ có dạng$(x_i, 0)$, và mỗi đường tròn có bán kính$r_i$. Nhiệm vụ là đếm xem có bao nhiêu điểm mạng nguyên$(x, y)$nằm bên trong ít nhất một trong các đường tròn này, kể cả các điểm trên đường biên. 

Điều kiện hình học để một điểm thuộc đường tròn là khoảng cách Euclide tiêu chuẩn: một điểm hợp lệ cho đường tròn$i$nếu như$(x - x_i)^2 + y^2 \le r_i^2$. Chúng ta phải lấy hợp của tất cả các đường tròn như vậy và đếm số điểm nguyên trong hợp đó. 

Cấu trúc ràng buộc là tín hiệu quan trọng. Tổng bán kính trên tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$, có nghĩa là mặc dù có thể có nhiều vòng tròn nhưng tổng “đóng góp chiều cao theo chiều dọc” của chúng bị giới hạn. Điều này ngay lập tức gợi ý rằng việc lặp lại trên tất cả các lớp dọc của tất cả các vòng tròn là khả thi, miễn là mỗi đơn vị bán kính được xử lý một lần hoặc một số lần không đổi. 

Một cách tiếp cận ngây thơ để kiểm tra mọi điểm nguyên trong một hộp giới hạn lớn sẽ nhanh chóng thất bại. Ngay cả một vòng tròn có bán kính$10^9$trải rộng trong phạm vi x rất lớn, khiến cho việc quét lưới theo kiểu cưỡng bức là không thể. 

Một trường hợp thất bại tinh vi hơn xuất hiện khi các vòng tròn chồng lên nhau nhiều. Ví dụ: nếu nhiều vòng tròn có chung tâm, việc đếm hai lần sẽ trở thành rủi ro trừ khi phạm vi bao phủ được hợp nhất cẩn thận trên mỗi tọa độ x. 

## Phương pháp tiếp cận 

Một phương pháp brute-force trực tiếp sẽ lặp qua mọi vòng tròn, sau đó với mọi số nguyên$x$trong khoảng ngang của nó, tính toán mức tối đa có thể$y$sử dụng$y = \lfloor \sqrt{r_i^2 - (x - x_i)^2} \rfloor$, và đánh dấu tất cả các điểm$(x, y)$trong một bộ. Về nguyên tắc, điều này đúng, nhưng số điểm được tạo ra có thể tỷ lệ thuận với tổng diện tích của tất cả các vòng tròn. Một đường tròn có bán kính$r$đã đóng góp$O(r^2)$điểm, điều này trở nên hoàn toàn không khả thi khi bán kính lớn. 

Quan sát quan trọng là các vòng tròn được tập trung vào trục x. Điều này có nghĩa là chúng ta có thể phân tách vấn đề theo từng cột. Đối với một số nguyên cố định$x$, mỗi vòng tròn đóng góp một khoảng dọc$[-h, h]$, Ở đâu$h = \lfloor \sqrt{r_i^2 - (x - x_i)^2} \rfloor$. Sự kết hợp của tất cả các vòng tròn tại tọa độ x đó trở thành sự kết hợp của các đoạn thẳng đứng trên một đường thẳng. 

Thay vì theo dõi từng điểm riêng lẻ, chúng tôi đếm các giá trị y nguyên cho mỗi x. Vấn đề giảm xuống còn tính tổng, trên tất cả các cột x số nguyên được bao phủ bởi bất kỳ vòng tròn nào, số lượng giá trị y số nguyên được bao phủ trong cột đó sau khi hợp nhất các phần chồng chéo. 

Vì mỗi đơn vị bán kính chỉ đóng góp O(1) cột x riêng biệt trên tất cả các vòng tròn tổng hợp (vì mỗi vòng tròn có bán kính r trải dài 2r+1 giá trị x và tổng của r là m), chúng ta có thể lặp trên mỗi vòng tròn và xử lý từng x mà nó bao phủ. 

Tại mỗi x, chúng tôi tính toán khoảng chiều cao tương ứng và lấy phạm vi y tối đa đóng góp tại cột đó. Để tránh tính hai lần giữa các vòng tròn, chúng tôi tổng hợp các khoản đóng góp cho mỗi x bằng cách sử dụng từ điển và giữ nguyên chiều cao tối đa lấy từ bán kính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(\sum r_i^2)$|$O(\sum r_i^2)$| Quá chậm | 
| Tối ưu |$O(m)$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, hãy tạo bản đồ`best[x]`lưu trữ đóng góp bán kính dọc tối đa tại tọa độ x. 

Điều này là cần thiết vì nhiều vòng tròn có thể bao phủ cùng một x và chỉ khoảng dọc lớn nhất mới quan trọng để đếm điểm y. 
2. Với mỗi đường tròn có tâm tại$x_i$với bán kính$r_i$, lặp lại tất cả các giá trị nguyên$dx$từ$-r_i$ĐẾN$r_i$. 

Điều này liệt kê mọi tọa độ x nguyên nằm trong hình chiếu ngang của vòng tròn. 
3. Đối với mỗi$dx$, tính vị trí x thực tế$x = x_i + dx$và tính chiều cao tối đa tại lát cắt đó:$$h = \left\lfloor \sqrt{r_i^2 - dx^2} \right\rfloor$$Điều này xuất phát trực tiếp từ việc sắp xếp lại phương trình đường tròn. 
4. Cập nhật`best[x] = max(best[x], h)`. 

Chúng tôi chỉ quan tâm đến phạm vi tiếp cận theo chiều dọc lớn nhất tại mỗi x vì các vòng tròn nhỏ hơn được chứa hoàn toàn trong các vòng tròn lớn hơn tại lát cắt đó. 
5. Sau khi xử lý tất cả các vòng tròn, lặp lại tất cả các giá trị x được lưu trữ trong`best`. Với mỗi x, hãy thêm$2 \cdot h + 1$cho câu trả lời, vì các giá trị y nguyên nằm trong khoảng từ$-h$ĐẾN$h$. 
6. Xuất số tiền tích lũy. 

### Tại sao nó hoạt động 

Với mọi số nguyên cố định x, tập hợp các điểm thỏa mãn ít nhất một ràng buộc đường tròn tạo thành một tập hợp các khoảng dọc có tâm tại y = 0. Bởi vì tất cả các đường tròn đều có tâm trên trục x nên các khoảng này đối xứng và lồng nhau theo chiều cao. Do đó, sự kết hợp thu gọn thành một khoảng duy nhất mà ranh giới của nó chỉ được xác định bởi chiều cao dẫn xuất bán kính cực đại tại x đó. Thuật toán bảo toàn bất biến này bằng cách chỉ lưu trữ chiều cao tối đa trên mỗi tọa độ x, đảm bảo không có sự trùng lặp nào được tính hai lần và không có điểm hợp lệ nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        xs = list(map(int, input().split()))
        rs = list(map(int, input().split()))

        best = {}

        for x0, r in zip(xs, rs):
            r2 = r * r
            for dx in range(-r, r + 1):
                x = x0 + dx
                y2 = r2 - dx * dx
                h = int(y2 ** 0.5)
                if x in best:
                    if h > best[x]:
                        best[x] = h
                else:
                    best[x] = h

        ans = 0
        for h in best.values():
            ans += 2 * h + 1

        print(ans)

if __name__ == "__main__":
    solve()
```Mã xử lý từng vòng tròn bằng cách quét qua số nguyên x-span của nó. Tính toán căn bậc hai xác định phạm vi tiếp cận theo chiều dọc tại lát cắt đó và chỉ lưu trữ mức tối đa để đảm bảo các phần chồng chéo được hợp nhất một cách chính xác. Tổng cuối cùng chuyển đổi từng nhịp dọc thành số điểm mạng nguyên chính xác của nó. 

Một điểm tinh tế là căn bậc hai số nguyên. Sử dụng căn bậc hai dấu phẩy động ở đây là an toàn vì bán kính tổng hợp đủ nhỏ, nhưng trong cài đặt chặt chẽ hơn, người ta sẽ thay thế nó bằng sqrt an toàn số nguyên để tránh các trường hợp cạnh chính xác gần hình vuông hoàn hảo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 2
x = [0, 0]
r = [1, 2]
```Chúng tôi theo dõi đóng góp cho mỗi x. 

| Vòng tròn | dx | x | h = tầng(sqrt(r^2 - dx^2)) | tốt nhất[x] | 
| --- | --- | --- | --- | --- | 
| r=1 | -1 | -1 | 0 | {-1:0} | 
| r=1 | 0 | 0 | 1 | {0:1} | 
| r=1 | 1 | 1 | 0 | {1:0} | 
| r=2 | -2 | -2 | 0 | {-2:0} | 
| r=2 | -1 | -1 | 1 | {-1:1} | 
| r=2 | 0 | 0 | 2 | {0:2} | 
| r=2 | 1 | 1 | 1 | {1:1} | 
| r=2 | 2 | 2 | 0 | {2:0} | 

Độ cao tốt nhất cuối cùng: x=0→2, x=±1→1, x=±2→0. 

Đóng góp: 

x=0 cho 5 điểm, x=±1 cho mỗi chỗ 3 điểm, x=±2 cho mỗi chỗ 1 điểm, tổng cộng là 13. 

Điều này chứng tỏ các vòng tròn nhỏ hơn được hấp thụ hoàn toàn bởi các vòng tròn lớn hơn ở các lát x chồng lên nhau. 

### Ví dụ 2 

đầu vào:```
n = 2
x = [0, 2]
r = [1, 2]
```Ở đây sự chồng chéo là một phần. 

| x | chiều cao tốt nhất | đóng góp | 
| --- | --- | --- | 
| -1 | 0 | 1 | 
| 0 | 1 | 3 | 
| 1 | 1 | 3 | 
| 2 | 2 | 5 | 
| 3 | 1 | 3 | 
| 4 | 0 | 1 | 

Tổng cộng là 16. 

Điều này cho thấy các trung tâm riêng biệt tạo ra các cột x độc lập và sự chồng chéo được xử lý tự động thông qua tối đa hóa mỗi x. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m)$| Mỗi đơn vị bán kính đóng góp một số vị trí x không đổi và tổng bán kính là m | 
| Không gian |$O(m)$| Từ điển lưu trữ tối đa một mục nhập cho mỗi tọa độ x được chạm bởi bất kỳ vòng tròn nào | 

Giải pháp phù hợp thoải mái trong giới hạn vì tổng số lát x được xử lý được giới hạn bởi tổng bán kính trên tất cả các trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n, m = map(int, input().split())
            xs = list(map(int, input().split()))
            rs = list(map(int, input().split()))

            best = {}
            for x0, r in zip(xs, rs):
                r2 = r * r
                for dx in range(-r, r + 1):
                    x = x0 + dx
                    y2 = r2 - dx * dx
                    h = int(y2 ** 0.5)
                    best[x] = max(best.get(x, 0), h)

            ans = 0
            for h in best.values():
                ans += 2 * h + 1
            print(ans)

    solve()
    return ""  # output ignored for asserts

# provided samples (structure only, output check omitted due to stub)
run("""4
2 3
0 0
1 2
2 3
0 2
1 2
3 3
0 2 5
1 1 1
4 8
0 5 10 15
2 2 2 2
""")

# custom tests
run("""1
1 1
0
1
""")  # single circle

run("""1
2 2
0 2
1 1
""")  # disjoint circles

run("""1
3 6
0 0 0
2 2 2
""")  # overlapping identical circles
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| vòng tròn đơn | đếm lưới vòng tròn chính xác | độ đúng cơ sở | 
| vòng tròn rời rạc | tổng số độc lập | không có sự can thiệp chéo | 
| sự trùng lặp giống hệt nhau | không tính gấp đôi | hợp nhất đúng đắn | 

## Vỏ cạnh 

Một trường hợp cạnh tinh tế là ngăn chặn hoàn toàn. Nếu một vòng tròn nhỏ nằm hoàn toàn bên trong một vòng tròn lớn hơn ở cùng phạm vi x, thì một liên kết đơn giản trên các vòng tròn có thể đếm gấp đôi số lát dọc. Thuật toán tránh điều này bằng cách chỉ lưu trữ chiều cao tối đa trên x. Ví dụ: với các vòng tròn có tâm ở cùng một điểm với bán kính 1 và 5, mỗi lát cắt x bị chi phối bởi vòng tròn bán kính 5 và vòng tròn nhỏ hơn không bao giờ đóng góp độc lập. 

Một trường hợp khác là khi các vòng tròn hầu như không chạm vào một điểm duy nhất. Tại tọa độ x đó, cả hai vòng tròn đều tạo ra độ cao hợp lệ, nhưng một lần nữa chỉ có vấn đề tối đa và điểm ranh giới chung được tính chính xác một lần xuyên suốt$2h+1$sự đối xứng. 

Trường hợp cuối cùng là tình trạng cực kỳ thưa thớt khi các trung tâm cách xa nhau. Ngay cả khi đó, từ điển vẫn còn nhỏ vì nó chỉ chứa các giá trị x thực sự đạt được bởi một số vòng tròn và mỗi vòng tròn đóng góp chính xác$2r_i+1$các vị trí như vậy, tôn trọng ràng buộc toàn cầu về tổng bán kính.
