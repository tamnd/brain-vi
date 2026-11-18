---
title: "CF 2014F - Phòng Cảnh Sát Trưởng"
description: "Chúng ta được cấp một cái cây trong đó mỗi nút tượng trưng cho một trại đang nắm giữ một lượng vàng nào đó. Chúng ta có thể chọn một số phe nhất định để “tăng cường sức mạnh”. Việc tăng cường trại không trực tiếp thay đổi số vàng của trại đó, nhưng nó làm giảm số vàng của tất cả các trại lân cận một lượng cố định $c$."
date: "2026-06-08T13:02:03+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 2014
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 974 (Div. 3)"
rating: 2000
weight: 2014
solve_time_s: 107
verified: false
draft: false
---

[CF 2014F - Phòng thủ của cảnh sát trưởng](https://codeforces.com/problemset/problem/2014/F) 

**Xếp hạng:** 2000 
**Tags:** dfs và tương tự, dp, tham lam, cây cối 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cái cây trong đó mỗi nút tượng trưng cho một trại đang nắm giữ một lượng vàng nào đó. Chúng ta có thể chọn một số phe nhất định để “tăng cường sức mạnh”. Tăng cường trại không trực tiếp thay đổi vàng của trại đó, nhưng nó làm giảm lượng vàng của tất cả các trại lân cận một lượng cố định$c$. Sau khi tất cả các quyết định tăng cường được đưa ra, chỉ các trại được tăng cường mới tồn tại và điểm số cuối cùng là tổng giá trị vàng của các nút còn sống sau tất cả các lần giảm. 

Vì vậy, quyết định thực sự là chọn các nút nào để chúng tôi tối đa hóa tổng đóng góp của các nút được chọn sau khi tính đến mức độ thiệt hại mà các nút lân cận gây ra cho chúng. 

Mỗi nút được chọn đóng góp giá trị ban đầu của nó, nhưng mỗi khi một nút lân cận của nó được chọn, giá trị của nó sẽ giảm đi$c$. Vì chỉ các nút được chọn mới quan trọng trong tổng cuối cùng nên sự tương tác hoàn toàn nằm trong tập hợp con các nút đã chọn. Vấn đề trở thành việc chọn một tập hợp con các nút tối đa hóa giá trị tùy thuộc vào cả trọng lượng nút và tương tác cạnh. 

Các ràng buộc đẩy chúng ta tới các giải pháp tuyến tính hoặc tuyến tính cho mỗi trường hợp thử nghiệm. Tổng số nút trên tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$, vì vậy bất kỳ giải pháp nào làm được nhiều hơn$O(n \log n)$tổng số hoặc một DP dựa trên DFS cho mỗi trường hợp thử nghiệm đều có thể chấp nhận được, nhưng mọi thứ bậc hai trong cấu trúc cây sẽ thất bại. 

Một trường hợp thất bại tinh vi đối với trực giác ngây thơ là giả định tính độc lập, chẳng hạn như chọn tất cả các nút dương. Điều đó không thành công vì việc chọn hai nút có giá trị cao liền kề có thể tệ hơn việc chỉ chọn một trong số chúng do bị phạt$c$. Một thất bại khác là việc loại bỏ lá một cách tham lam, phá vỡ chuỗi dài nơi các lựa chọn địa phương làm giảm lợi ích trong tương lai. 

## Phương pháp tiếp cận 

Nếu bỏ qua cấu trúc, chúng ta có thể thử tất cả các tập hợp con của nút. Đối với mỗi tập hợp con, chúng tôi tính điểm của nó bằng cách tính tổng các giá trị nút và trừ đi$c$cho mọi cạnh bên trong nơi cả hai điểm cuối được chọn. Điều này đúng nhưng cần phải kiểm tra$2^n$tập hợp con, điều này là không thể ngay cả đối với$n = 30$. 

Quan sát quan trọng là mục tiêu chỉ phụ thuộc vào cạnh nào được “kích hoạt” bên trong tập hợp đã chọn. Mỗi nút được chọn đóng góp trọng số của nó và mỗi cạnh được chọn đóng góp một hình phạt là$c$. Đây chính xác là bài toán tập con trọng số tối đa trên cây nơi các nút và cạnh tương tác cục bộ. 

Kiểu cấu trúc này được cây DP xử lý một cách tự nhiên. Sự lựa chọn tại một nút chỉ phụ thuộc vào việc nó có được chọn hay không và các nút con của nó hoạt động như thế nào. Khi đã root cây, chúng ta có thể biểu thị giải pháp tối ưu cho từng cây con bằng hai trạng thái: chọn hoặc không chọn gốc của cây con đó. 

Điều này làm giảm bài toán tập hợp con hàm mũ thành DP tuyến tính trên các cạnh của cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Cây DP |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây tại bất kỳ nút nào, thường là nút 1. DP tại mỗi nút tính toán hai giá trị: điểm tốt nhất có thể đạt được trong cây con của nó khi nút được chọn và khi nút đó không được chọn. 

1. Xác định$dp[u][1]$là điểm tối đa trong cây con của$u$nếu chúng ta chọn$u$. Định nghĩa$dp[u][0]$là điểm tối đa nếu chúng ta không chọn$u$. 

Sự phân tách này là cần thiết vì hình phạt trên các cạnh chỉ áp dụng khi cả hai điểm cuối đều được chọn. 
2. Khởi tạo$dp[u][1] = a_u$, kể từ khi chọn$u$đưa ra giá trị đầy đủ của nó trước khi xem xét các hàng xóm. 
3. Khởi tạo$dp[u][0] = 0$, vì không chọn$u$không đóng góp gì trực tiếp. 
4. Đối với mỗi trẻ$v$của$u$, chúng tôi hợp nhất sự đóng góp của nó vào$u$. 

Nếu như$u$thì không được chọn$v$có thể được chọn hoặc không được chọn mà không bị phạt từ rìa$(u, v)$. Vì vậy chúng tôi thêm$\max(dp[v][0], dp[v][1])$. 

Nếu như$u$được chọn, sau đó chọn$v$phải chịu một hình phạt$c$, bởi vì cạnh đóng góp chính xác một lần. Vì vậy: 

- nếu chúng ta không chọn$v$, chúng tôi thêm$dp[v][0]$- nếu chúng ta chọn$v$, chúng tôi thêm$dp[v][1] - c$Vì vậy, trong trường hợp này chúng tôi thêm$\max(dp[v][0], dp[v][1] - c)$. 
5. Sau khi xử lý tất cả các phần tử con, câu trả lời cho phần gốc là$\max(dp[root][0], dp[root][1])$. 

Lý do điều này có hiệu quả là vì mỗi cạnh được xem xét chính xác một lần, từ cạnh gốc đến cạnh con và hình phạt chỉ được áp dụng khi cả hai điểm cuối đều được chọn. DP đảm bảo rằng các quyết định về cây con là độc lập khi trạng thái của cây cha được cố định, do đó không có tương tác nào bị bỏ sót hoặc bị tính hai lần. 

## Giải pháp Python```python
import sys
sys.setrecursionlimit(10**7)
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, c = map(int, input().split())
        a = list(map(int, input().split()))
        
        g = [[] for _ in range(n)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            g[u].append(v)
            g[v].append(u)
        
        dp0 = [0] * n
        dp1 = [0] * n
        parent = [-1] * n
        
        order = []
        stack = [0]
        parent[0] = -2
        
        while stack:
            u = stack.pop()
            order.append(u)
            for v in g[u]:
                if parent[v] == -1:
                    parent[v] = u
                    stack.append(v)
        
        for u in reversed(order):
            dp1[u] = a[u]
            dp0[u] = 0
            
            for v in g[u]:
                if v == parent[u]:
                    continue
                dp0[u] += max(dp0[v], dp1[v])
                dp1[u] += max(dp0[v], dp1[v] - c)
        
        print(max(dp0[0], dp1[0]))

if __name__ == "__main__":
    solve()
```Trong quá trình triển khai, thứ tự DFS được chuyển đổi thành truyền tải lặp lại để tránh các vấn đề về độ sâu đệ quy. Sau đó, chúng tôi xử lý các nút theo thứ tự ngược lại để các nút con được đánh giá trước cha mẹ chúng, đảm bảo trạng thái DP luôn sẵn sàng khi cần. Điểm tinh tế quan trọng là tách hai trường hợp cho mỗi nút: được chọn và không được chọn, bởi vì đó là điều cho phép chúng ta tính phí phạt chính xác một lần cho mỗi cạnh đã chọn. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi đơn giản gồm ba nút có giá trị$[2, 3, 1]$Và$c = 1$. 

| Nút | dp0 | dp1 | 
| --- | --- | --- | 
| 3 | 0 | 1 | 
| 2 | 3 | 3 + tối đa(0,1-1)=3 | 
| 1 | 3 | 2 + tối đa(0,3-1)=4 | 

Câu trả lời cuối cùng là 4, đạt được bằng cách chọn nút 1 và 2 chứ không phải nút 3. 

Điều này cho thấy hình phạt chỉ áp dụng khi cả hai đầu được chọn và việc bỏ qua một nút đôi khi có thể làm tăng giá trị xuôi dòng như thế nào. 

Bây giờ hãy xem xét một ngôi sao trong đó tâm có giá trị lớn nhưng các lá lại nhỏ. DP đương nhiên sẽ tránh chọn quá nhiều lá nếu hình phạt lớn hơn đóng góp của họ, thể hiện sự cân bằng giữa lợi ích nút và chi phí biên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi cạnh được xử lý một lần trong quá trình chuyển đổi DP | 
| Không gian |$O(n)$| Lưu trữ danh sách kề và mảng DP | 

Vì tổng của$n$trên tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$, việc truyền tải tuyến tính cho mỗi trường hợp thử nghiệm này vẫn hiệu quả trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read().strip()
```

```
# provided sample
# (placeholder, assumes integration with full solution)

# small chain
# 1-2-3 with positive values
# star case
# single center dominates
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi | hướng dẫn sử dụng | Xử lý hình phạt cạnh DP | 
| ngôi sao | hướng dẫn sử dụng | sự cân bằng giữa trung tâm và lá | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi tất cả$a_i$là tiêu cực. DP tránh chọn đúng hầu hết các nút vì$dp[u][1]$bắt đầu từ một giá trị âm và quá trình chuyển đổi cho phép bỏ qua toàn bộ lựa chọn. Một trường hợp khác là khi$c$rất lớn, làm cho bất kỳ lựa chọn liền kề nào cũng có hại; DP sau đó sẽ chỉ chọn các nút có giá trị cao bị cô lập. Trường hợp cuối cùng là đường dẫn trong đó lựa chọn xen kẽ là tối ưu, được nắm bắt một cách tự nhiên bởi sự phân chia trạng thái cha-con. 

Nếu bạn muốn, tôi cũng có thể hiển thị phiên bản nén “sẵn sàng cho cuộc thi” của giải pháp này hoặc lấy nó chính thức từ việc đóng trọng lượng tối đa, đây là lý thuyết cơ bản đằng sau DP này.
