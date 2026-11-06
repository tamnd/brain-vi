---
title: "CF 1249F - Tập hợp con trọng lượng tối đa"
description: "Chúng ta được cho một cây trong đó mỗi đỉnh mang một trọng số dương. Nhiệm vụ là chọn một tập hợp con các đỉnh sao cho tổng trọng số đã chọn là lớn nhất, nhưng với một hạn chế hình học nghiêm ngặt: bất kỳ hai đỉnh được chọn nào cũng phải cách nhau hơn $k$ các cạnh trong cây."
date: "2026-06-15T22:01:02+07:00"
tags: ["codeforces", "competitive-programming", "dp", "trees"]
categories: ["algorithms"]
codeforces_contest: 1249
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 595 (Div. 3)"
rating: 2200
weight: 1249
solve_time_s: 360
verified: true
draft: false
---

[CF 1249F - Tập hợp con trọng lượng tối đa](https://codeforces.com/problemset/problem/1249/F) 

**Đánh giá:** 2200 
**Thẻ:** dp, cây cối 
**Thời gian giải:** 6 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cây trong đó mỗi đỉnh mang một trọng số dương. Nhiệm vụ là chọn một tập hợp con các đỉnh sao cho tổng trọng số được chọn là lớn nhất, nhưng với một hạn chế hình học chặt chẽ: hai đỉnh được chọn bất kỳ phải lớn hơn$k$các cạnh cách nhau trên cây. 

Tương tự, khi chọn một đỉnh, chúng ta “cấm” chọn bất kỳ đỉnh nào trong khoảng cách$k$từ nó. Mục tiêu là đặt các đỉnh đã chọn trên cây sao cho khoảng cách theo cặp của chúng đều lớn hơn$k$, đồng thời tối đa hóa tổng trọng lượng. 

Ràng buộc$n \le 200$đủ nhỏ để lập trình động bậc hai hoặc bậc ba trên các trạng thái trên các nút là khả thi. Tuy nhiên, việc liệt kê tập hợp con đơn giản vẫn không thể thực hiện được vì$2^n$phát triển vượt xa tính khả thi ngay cả đối với$n = 200$. Điều này đẩy chúng ta tới một DP có cấu trúc trên cây. 

Một điểm tinh tế là hạn chế không cục bộ ở các cạnh, nó lan truyền qua các khoảng cách trong cây. Điều này làm cho bài toán về cơ bản khác với bài toán tập độc lập cổ điển trên cây (tương ứng với$k=1$). Ở đây, “vùng loại trừ” của nút được chọn là bán kính-$k$quả bóng trên cây. 

Các trường hợp biên phát sinh khi các nút có trọng số cao được nhóm lại. Ví dụ, hãy xem xét một chuỗi$1-2-3-4$với$k=2$. Chọn nút 1 cấm nút 2 và 3, nhưng nút 4 vẫn hợp lệ. Việc lựa chọn cực đại cục bộ một cách tham lam có thể thất bại: chọn 2 và 4 mang lại nhiều trọng số hơn so với chỉ chọn 1 trong một số cấu hình trọng số, nhưng việc chọn 2 cũng cấm 1 và 3, thay đổi tính khả thi xuôi dòng theo những cách không cục bộ. 

Một trường hợp cạnh khác là khi$k=0$. Khi đó không có hạn chế nào tồn tại và tất cả các nút đều có thể được chọn. Một DP ngây thơ luôn thực thi việc loại trừ cha-con sẽ bị tính thiếu một cách không chính xác. 

Cuối cùng, khi$k$lớn (gần bằng$n$), việc chọn cả hai nút là không thể trừ khi chúng cách nhau rất xa, vì vậy câu trả lời tối ưu thường suy biến thành đỉnh đơn tối đa. Một giải pháp đúng đắn phải xử lý quá trình chuyển đổi này một cách suôn sẻ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử tất cả các tập hợp con của đỉnh và kiểm tra tính hợp lệ bằng cách tính toán tất cả các khoảng cách theo cặp. Ngay cả khi khoảng cách được tính toán trước bằng BFS từ mỗi nút, việc kiểm tra một tập hợp con vẫn tốn kém$O(n^2)$và liệt kê các tập hợp con chi phí$2^n$, dẫn đến đại khái$O(2^n \cdot n^2)$, điều đó là không thể thực hiện được. 

Quan sát chính là ràng buộc mang tính cục bộ theo nghĩa khoảng cách của cây. Nếu chúng ta sửa một nút như đã chọn, vùng cấm của nút đó là vùng lân cận hình cây con được giới hạn bởi khoảng cách$k$. Điều này gợi ý DP trong đó trạng thái phải nhớ mức độ “gần” của chúng tôi với nút đã chọn ở trên cây. 

Thủ thuật tiêu chuẩn là root cây và xác định một DP để theo dõi, đối với mỗi nút, khoảng cách từ nút tổ tiên được chọn gần nhất. Tuy nhiên, việc lưu trữ khoảng cách chính xác tới tất cả các nút đã chọn là quá lớn. Thay vào đó, chúng tôi nén thông số này thành một tham số giới hạn: khoảng cách từ nút hiện tại đến nút được chọn gần nhất trong cây con được xử lý của nó. 

Điều này dẫn đến DP trên các nút trong đó mỗi trạng thái thể hiện liệu một nút có được chọn hay không và nút được chọn gần nhất trong cây con hoặc tổ tiên của nó cách xa bao nhiêu, giới hạn ở mức$k$. Từ$k \le 200$, chúng ta có thể duy trì một bảng DP có kích thước$O(nk)$trên mỗi nút, kết hợp các nút con thông qua các chuyển đổi giống như chiếc ba lô trên cây. 

Bước hợp nhất giống như lan truyền đa trạng thái: khi xử lý một nút, chúng tôi xem xét liệu nút đó có được chọn hay không và nếu có thì tất cả các nút trong khoảng cách$k$trong cây con của nó không thể được chọn. Khi nó không được chọn, trẻ em có thể độc lập đóng góp với các trạng thái khoảng cách được cập nhật. 

Điều này biến đổi giới hạn khoảng cách toàn cục thành các chuyển tiếp cục bộ trên cây có gốc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force |$O(2^n \cdot n^2)$|$O(n^2)$| Quá chậm | 
| Cây DP với các trạng thái khoảng cách |$O(nk^2)$|$O(nk)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây ở nút 1 và xác định bảng DP cho mỗi nút$u$. 

Cho phép$dp_u[d]$biểu thị trọng số tối đa trong cây con của$u$với điều kiện nút được chọn gần nhất ở trên hoặc bên trong cây con ở khoảng cách$d$, Ở đâu$d$được giới hạn ở$k$. giá trị$d=0$có nghĩa$u$được chọn và các giá trị lớn hơn biểu thị khoảng cách ngày càng tăng đến nút được chọn gần nhất. 

Chúng tôi tuyên truyền thông tin từ dưới lên. 

1. Khởi tạo từng nút$u$với mảng DP trong đó tất cả các trạng thái được đặt thành âm vô cực, ngoại trừ trường hợp$u$được chọn. Nếu như$u$được chọn, nó góp phần$a_u$và tất cả trẻ em phải tôn trọng rằng không có nút nào trong khoảng cách$k$có thể được lựa chọn theo những cách trái ngược nhau. 
2. Xử lý từng nút con một, hợp nhất các bảng DP của chúng vào bảng DP của nút hiện tại. Hoạt động hợp nhất xem xét tất cả các cặp trạng thái từ DP hiện tại và DP con, cập nhật khoảng cách một cách thích hợp bằng cách tăng giá trị khoảng cách lên 1 khi di chuyển lên trên. 
3. Nếu trạng thái con chỉ ra nút được chọn ở khoảng cách$d$, thì theo quan điểm của nút cha mẹ thì nút đó ở khoảng cách$d+1$, giới hạn ở$k$. Sự dịch chuyển này rất quan trọng vì khoảng cách tăng đúng bằng một khi di chuyển lên một cạnh. 
4. Trong quá trình hợp nhất, chúng tôi đảm bảo tính khả thi bằng cách loại bỏ bất kỳ sự kết hợp nào trong đó hai nút được chọn sẽ ở khoảng cách xa nhau$\le k$. Điều này được thực thi ngầm bằng cách không cho phép các trạng thái vi phạm ngưỡng khoảng cách khi kết hợp các bảng DP. 
5. Sau khi xử lý tất cả các phần tử con, chúng tôi hoàn thiện$dp_u$bằng cách xem xét cả hai trường hợp:$u$được chọn hoặc không được chọn và truyền kết quả lên trên. 

Cuối cùng, câu trả lời là giá trị lớn nhất trong số tất cả các trạng thái của nút gốc. 

### Tại sao nó hoạt động 

Bất biến DP là bất biến đối với mọi nút$u$,$dp_u[d]$thể hiện chính xác trọng số tối ưu của việc chọn một tập hợp con hợp lệ bên trong cây con của$u$, phù hợp với điều kiện biên là nút được chọn gần nhất bên ngoài cây con ở khoảng cách$d$. Mỗi cây con được tối ưu hóa độc lập theo ràng buộc ranh giới này và việc hợp nhất các cây con sẽ duy trì tính khả thi vì khoảng cách trong cây được xác định duy nhất thông qua đường dẫn gốc. Vì mọi xung đột lựa chọn đều phải đi qua tổ tiên chung thấp nhất nên DP đảm bảo rằng mọi cặp bị cấm đều được phát hiện tại điểm hợp nhất của chúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    sys.setrecursionlimit(10**7)

    INF_NEG = -10**18

    # dp[u][d] where d in [0..k], capped distance to nearest chosen node above
    def dfs(u, p):
        dp = [[INF_NEG] * (k + 1) for _ in range(1)]
        dp[0][k] = 0  # no restriction coming from above

        for v in g[u]:
            if v == p:
                continue
            child = dfs(v, u)

            new_dp = [[INF_NEG] * (k + 1) for _ in range(len(dp) + len(child))]

            for i in range(len(dp)):
                for j in range(len(child)):
                    for d1 in range(k + 1):
                        for d2 in range(k + 1):
                            if dp[i][d1] == INF_NEG or child[j][d2] == INF_NEG:
                                continue

                            nd1 = min(k, d1
```
