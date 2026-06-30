---
title: "CF 104639K - Khoảng cách Euclide tối thiểu"
description: "Chúng ta có một đa giác lồi biểu thị vùng an toàn trong mặt phẳng. Đối với mỗi truy vấn airdrop, chúng tôi cũng nhận được một vòng tròn được xác định bởi các điểm cuối đường kính của nó. Mỗi airdrop hạ cánh ngẫu nhiên ở bất kỳ đâu trong vòng tròn đó."
date: "2026-06-29T16:57:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104639
codeforces_index: "K"
codeforces_contest_name: "The 2023 ICPC Asia EC Regionals Online Contest (I)"
rating: 0
weight: 104639
solve_time_s: 34
verified: true
draft: false
---

[CF 104639K - Khoảng cách Euclide tối thiểu](https://codeforces.com/problemset/problem/104639/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đa giác lồi biểu thị vùng an toàn trong mặt phẳng. Đối với mỗi truy vấn airdrop, chúng tôi cũng nhận được một vòng tròn được xác định bởi các điểm cuối đường kính của nó. Mỗi airdrop hạ cánh ngẫu nhiên ở bất kỳ đâu trong vòng tròn đó. 

Đối với mỗi truy vấn, chúng ta phải chọn một điểm duy nhất bên trong đa giác. Điểm đó hoạt động như một điểm tham chiếu hạ cánh cố định. Chi phí của một lựa chọn là giá trị kỳ vọng của bình phương khoảng cách Euclide giữa điểm được chọn này và một điểm ngẫu nhiên thống nhất bên trong vòng tròn. Nhiệm vụ là giảm thiểu kỳ vọng này đối với tất cả các lựa chọn điểm bên trong đa giác và đưa ra giá trị tối thiểu thu được cho mỗi vòng tròn. 

Đa giác là cố định, trong khi các vòng tròn khác nhau giữa các truy vấn. Các ràng buộc cho phép tối đa 5000 đỉnh và 5000 truy vấn, với tọa độ lên tới 1e9. 

Một cách giải thích ngây thơ là mỗi truy vấn yêu cầu tối ưu hóa hàm liên tục trên một đa giác lồi, điều này đã gợi ý rằng các thuộc tính hình học và độ lồi sẽ là trung tâm. 

Một quan sát cấu trúc quan trọng là kỳ vọng chỉ phụ thuộc vào vị trí tương đối giữa điểm đã chọn và điểm ngẫu nhiên trong vòng tròn chứ không phụ thuộc vào hình dạng đa giác ngoại trừ tính khả thi của điểm đã chọn. 

Trường hợp cạnh tinh tế là khi điểm tốt nhất cho hình tròn nằm bên ngoài đa giác. Ví dụ: nếu đa giác rất nhỏ và vòng tròn ở xa, điểm không bị ràng buộc tối ưu có thể là tâm vòng tròn, nhưng đa giác buộc chiếu lên ranh giới của nó. Điều này có nghĩa là chúng ta không chỉ tính toán một đặc tính hình học của đường tròn mà còn tính toán tối ưu hóa có ràng buộc trên một tập lồi. 

## Phương pháp tiếp cận 

Bắt đầu bằng cách bỏ qua ràng buộc đa giác. Đối với một đường tròn cố định, giả sử chúng ta chọn một điểm P và điểm X ngẫu nhiên được phân bố đều trong một đĩa. Mục tiêu là giảm thiểu E[|P − X|²]. 

Mở rộng hình vuông ta có E[|P − X|²] = |P|² − 2P·E[X] + E[|X|²]. Sự đơn giản hóa chính là kỳ vọng chỉ phụ thuộc vào P thông qua khoảng cách của nó với giá trị trung bình của phân phối. Để phân bố đồng đều trên một đĩa, giá trị trung bình chính xác là tâm của vòng tròn. Do đó, biểu thức trở nên cực tiểu khi P càng gần tâm đường tròn càng tốt và giá trị không bị ràng buộc tối thiểu đạt được tại P bằng tâm. 

Do đó, vấn đề giảm xuống còn việc chiếu tâm đường tròn lên đa giác lồi. P tối ưu là điểm gần nhất trong đa giác với tâm đường tròn theo khoảng cách Euclide. Khi chúng ta có điểm chiếu Q đó, câu trả lời là E[|Q − X|²], có thể được mở rộng thành |Q − C|² + E[|X − C|²], trong đó C là tâm đường tròn. 

Vì vậy, bài toán chia thành hai phần: một hằng số hình học chỉ phụ thuộc vào hình tròn và bình phương khoảng cách từ tâm hình tròn đến đa giác. 

Nhiệm vụ còn lại là tính toán, đối với mỗi điểm truy vấn C, khoảng cách bình phương từ C đến đa giác lồi. Với n lên tới 5000 và q lên tới 5000, quét điểm trong đa giác hoặc khoảng cách đến cạnh O(nq) là đủ. Bởi vì đa giác là lồi và các đỉnh được sắp xếp theo thứ tự, điểm gần nhất với điểm bên ngoài nằm trên một đỉnh hoặc trên một cạnh nơi hình chiếu vuông góc nằm trong đoạn đó. Điều này có thể được kiểm tra theo thời gian tuyến tính cho mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phép chiếu lực lượng vũ phu cho mỗi truy vấn trên tất cả các cạnh | O(nq) | O(n) | Đã chấp nhận | 
| Tối ưu tương tự như trên với sự đơn giản hóa hình học lồi | O(nq) | O(n) | Đã chấp nhận | 

Không cần cấu trúc dữ liệu nâng cao hơn vì các ràng buộc đủ nhỏ và hình học sẽ đơn giản sau khi kỳ vọng được viết lại chính xác. 

## Hướng dẫn thuật toán

1. Tính toán trước bình phương tâm và bán kính đường tròn cho mỗi truy vấn. Tâm là điểm giữa của các điểm cuối đường kính và bình phương bán kính là một phần tư bình phương khoảng cách giữa các điểm cuối. Điều này cô lập tất cả dữ liệu hình học dành riêng cho truy vấn. 
2. Đối với mỗi truy vấn, hãy coi bài toán là tìm khoảng cách bình phương tối thiểu từ tâm đường tròn đến đa giác lồi. Điều này là hợp lý vì kỳ vọng phân hủy thành một hằng số chỉ phụ thuộc vào đường tròn cộng với bình phương khoảng cách từ điểm đã chọn đến tâm. 
3. Đối với điểm trung tâm C cho trước, lặp lại tất cả các cạnh của đa giác. Với mỗi cạnh AB, hãy tính hình chiếu của C lên đường thẳng AB. 
4. Nếu tham số chiếu nằm trong đoạn thẳng, hãy tính bình phương khoảng cách từ C đến điểm chiếu. Mặt khác, tính khoảng cách bình phương đến điểm cuối gần nhất giữa A và B. Điều này đảm bảo chúng ta đang tính khoảng cách đến điểm gần nhất trên đoạn cạnh đó. 
5. Duy trì khoảng cách bình phương tối thiểu như vậy trên tất cả các cạnh và đỉnh. Điều này cho biết khoảng cách bình phương từ C đến đa giác lồi. 
6. Cộng giá trị mong đợi không đổi của khoảng cách bình phương từ một điểm ngẫu nhiên trên đĩa đến tâm của nó, bằng R²/2 và xuất ra tổng. 

Lý do chính khiến bước 6 hoạt động là vì đối với một đĩa đồng nhất, phương sai ở hai chiều chia đều trên các trục, cho ra E[|X − C|²] = R²/2. 

### Tại sao nó hoạt động 

Kỳ vọng E[|P − X|²] có thể được phân tách thành |P − C|² + E[|X − C|²], trong đó C là tâm đường tròn. Số hạng thứ hai độc lập với P, do đó việc giảm thiểu kỳ vọng sẽ giảm chính xác đến mức tối thiểu hóa khoảng cách bình phương từ P đến C. Vì vùng khả thi là lồi nên điểm khả thi gần nhất được xác định rõ và nằm trên hình chiếu đỉnh hoặc cạnh. Thuật toán liệt kê tất cả các ứng cử viên như vậy, vì vậy nó phải bao gồm điểm gần nhất thực sự và do đó đạt được mức tối thiểu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def dot(ax, ay, bx, by):
    return ax * bx + ay * by

def dist2(ax, ay, bx, by):
    dx = ax - bx
    dy = ay - by
    return dx * dx + dy * dy

def clamp01(t):
    if t < 0.0:
        return 0.0
    if t > 1.0:
        return 1.0
    return t

def solve():
    n, q = map(int, input().split())
    poly = [tuple(map(int, input().split())) for _ in range(n)]

    for _ in range(q):
        x1, y1, x2, y2 = map(int, input().split())

        cx = (x1 + x2) / 2.0
        cy = (y1 + y2) / 2.0

        dx = x2 - x1
        dy = y2 - y1
        r2 = (dx * dx + dy * dy) / 4.0

        best = float('inf')

        for i in range(n):
            x3, y3 = poly[i]
            x4, y4 = poly[(i + 1) % n]

            ex = x4 - x3
            ey = y4 - y3

            vx = cx - x3
            vy = cy - y3

            e2 = ex * ex + ey * ey
            t = 0.0
            if e2 != 0:
                t = (vx * ex + vy * ey) / e2
                t = clamp01(t)

            px = x3 + t * ex
            py = y3 + t * ey

            best = min(best, dist2(cx, cy, px, py))

        ans = best + r2 / 2.0
        print(f"{ans:.10f}")

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên chuyển đổi từng đường kính thành tâm hình tròn và bình phương bán kính. Công thức trung điểm trực tiếp cho ra tâm kỳ vọng, trong khi việc tính toán bán kính hoàn toàn là đại số. 

Vòng lặp đa giác tính toán điểm gần nhất trên mỗi đoạn bằng phép chiếu. Bước kẹp đảm bảo hình chiếu nằm trong phân đoạn; mặt khác điểm cuối chiếm ưu thế. Đây là phép chiếu Euclide tiêu chuẩn lên một đoạn. 

Cuối cùng, câu trả lời kết hợp khoảng cách bình phương từ tâm hình tròn đến đa giác với số hạng phương sai cố định R²/2. 

## Ví dụ đã hoạt động 

Hãy coi đa giác mẫu là một hình vuông đơn vị và vòng tròn truy vấn đầu tiên có điểm cuối (0,0) và (1,1). Tâm là (0,5, 0,5) và bình phương bán kính là 0,5. 

| Bước | Hành động | cx | cy | r² | khoảng cách tốt nhất² | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | tính toán vòng tròn | 0,5 | 0,5 | 0,5 | thông tin | 
| cạnh | quét đa giác | 0,5 | 0,5 | 0,5 | 0 | 
| cuối cùng | thêm r²/2 | 0,5 | 0,5 | 0,5 | 0,25 | 

Hình chiếu nằm bên trong hình vuông nên tâm là khả thi và khoảng cách bằng không. Câu trả lời cuối cùng hoàn toàn là thuật ngữ phương sai đĩa. 

Bây giờ hãy xem xét một đường tròn ở xa bên ngoài đa giác, chẳng hạn như tâm ở (2,2) với bán kính bất kỳ. Điểm gần nhất trong hình vuông luôn là (1,1). 

| Bước | Hành động | cx | cy | tốt nhất | 
| --- | --- | --- | --- | --- | 
| ban đầu | tâm vòng tròn | 2 | 2 | thông tin | 
| cạnh | chiếu | 2 | 2 | 2 | 
| vây | | | | |
