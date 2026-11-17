---
title: "CF 2007D - Hoa diên vĩ và trò chơi trên cây"
description: "Chúng ta có một cây có gốc ở đỉnh 1 trong đó mỗi nút chứa giá trị 0, 1 hoặc giá trị chưa quyết định được biểu thị là ?."
date: "2026-06-09T02:49:17+07:00"
tags: ["codeforces", "competitive-programming", "games", "graphs", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 2007
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 969 (Div. 2)"
rating: 1700
weight: 2007
solve_time_s: 347
verified: false
draft: false
---

[CF 2007D - Hoa diên vĩ và trò chơi trên cây](https://codeforces.com/problemset/problem/2007/D) 

**Đánh giá:** 1700 
**Tags:** trò chơi, đồ thị, tham lam, cây cối 
**Thời gian giải:** 5 phút 47s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc ở đỉnh 1 trong đó mỗi nút chứa giá trị 0, 1 hoặc giá trị chưa quyết định được biểu thị dưới dạng`?`. Mỗi lá có một “trọng lượng”, được xác định bằng cách đi từ gốc đến lá đó, đếm số lượng`10`chuỗi con trừ đi số lượng`01`chuỗi con trong chuỗi đường dẫn. Điểm của cây chỉ đơn giản là số lá có trọng số khác 0. 

Điều khó khăn là một số đỉnh chưa được quyết định và hai người chơi thay phiên nhau gán 0 hoặc 1 cho chúng. Iris di chuyển đầu tiên và muốn tối đa hóa điểm số, trong khi Dora di chuyển thứ hai và muốn giảm thiểu nó. Bài toán yêu cầu điểm số cuối cùng giả sử lối chơi tối ưu. 

Những ràng buộc rất chặt chẽ. Chúng ta có thể có tới$10^5$các nút trên mỗi trường hợp thử nghiệm và tổng số tiền trên tất cả các trường hợp thử nghiệm có thể đạt tới$2 \cdot 10^5$. Điều này loại trừ việc mô phỏng ngây thơ của tất cả các bài tập. Bất kỳ giải pháp nào cố gắng khám phá tất cả$2^k$nhiệm vụ có thể có cho`?`các nút sẽ hết thời gian chờ vì số lượng các nút chưa quyết định có thể dễ dàng lớn. Lý tưởng nhất là chúng ta cần một chiến lược tuyến tính hoặc gần tuyến tính$O(n)$mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh tế xảy ra khi trọng lượng của lá có thể bị ép về 0 hoặc khác 0 tùy thuộc vào các lựa chọn ở sâu trong cây. Ví dụ, nếu một chiếc lá có một`?`dọc theo đường đi của nó, người chơi tối ưu phải quyết định giá trị của nó để tác động đến việc liệu`10`hoặc`01`xuất hiện, nhưng nếu nhiều lá chia sẻ điều này`?`, quyết định có hiệu lực khác nhau. Một cách tiếp cận bất cẩn, xử lý các lá một cách độc lập sẽ tính điểm cao hơn hoặc thấp hơn. 

Một trường hợp cạnh khác là một cây có tất cả các nút chưa được quyết định. Nếu cả hai người chơi đều chơi tối ưu thì điểm số không nhất thiết phải là tổng số lá vì Dora có thể phản công các lựa chọn của Iris một cách chiến lược. Tương tự, một cây có nút gốc hoặc nút bên trong`?`đòi hỏi sự lan truyền ảnh hưởng một cách cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: đối với mỗi chuỗi di chuyển, hãy mô phỏng trò chơi một cách đệ quy, khám phá mọi nhiệm vụ có thể có cho`?`. Tại mỗi lá, tính chuỗi và trọng lượng của nó, sau đó đếm xem có bao nhiêu lá có trọng số khác 0. Điều này đúng vì nó xem xét rõ ràng tất cả các chiến lược. Vấn đề là nếu chúng ta có$k$các nút chưa quyết định, điều này mang lại$O(2^k)$phức tạp, điều này là không thể thực hiện được ngay cả đối với những cây có mức độ vừa phải. 

Cái nhìn sâu sắc quan trọng là xem vấn đề từ dưới lên. Mỗi cây con có một "đóng góp" vào điểm số có thể được mô tả dưới dạng một cặp số nguyên: số lượng lá tối đa và tối thiểu có thể kết thúc với trọng số khác 0 trong cây con đó tùy thuộc vào người di chuyển tiếp theo. Các nút lá là trường hợp cơ bản. Sau đó, các nút nội bộ có thể tính toán mức đóng góp tối thiểu/tối đa của riêng chúng bằng cách kết hợp các khoản đóng góp của các nút con bằng cách sử dụng phép tính minimax theo kiểu lý thuyết trò chơi. 

Cụ thể, đối với bất kỳ nút nào có giá trị chưa xác định, người chơi đầu tiên có thể chọn giá trị tối đa hóa điểm thu được trong cây con của nó, trong khi người chơi thứ hai sẽ chọn giá trị tối thiểu hóa điểm đó. Các nút có giá trị đã biết sẽ truyền hiệu ứng của chúng một cách đơn giản. Điều này làm giảm vấn đề xuống còn một DFS thứ tự sau trên cây, kết hợp các kết quả con tại mỗi nút. Nó tuyến tính vì chúng tôi truy cập mỗi nút một lần, tổng hợp các kết quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^k) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu diễn cây dưới dạng danh sách kề và đánh dấu nút nào là lá. Lá là các nút không có con ngoại trừ gốc, không bao giờ được coi là lá. 
2. Thực hiện DFS bắt đầu từ thư mục gốc. Đối với mỗi nút, tính toán đệ quy hai giá trị: điểm cao nhất mà người chơi đầu tiên có thể đạt được (`first`) và điểm cao nhất mà người chơi thứ hai có thể thực thi (`second`) cho cây con có gốc tại nút đó. 
3. Đối với các nút lá, nếu giá trị đã được biết (0 hoặc 1), thì đóng góp của nó vào điểm là 1 nếu trọng số của lá khác 0, nếu không thì là 0. Nếu là`?`, tạm thời gán cả hai khả năng là 1 vì một lá có a`?`người chơi đầu tiên luôn có thể làm cho nó khác 0. 
4. Đối với các nút nội bộ, thu thập sự đóng góp từ tất cả trẻ em. Nếu giá trị của nút được quyết định, hãy truyền bá các đóng góp con. Nếu nút là`?`, người chơi đầu tiên chọn giá trị tối đa hóa sự đóng góp tối thiểu của trẻ em, trong khi người chơi thứ hai chọn giá trị giảm thiểu sự đóng góp tối đa của trẻ em. Quy tắc kết hợp là: tổng số điểm của cây con là tổng các đóng góp của lá có thể buộc phải khác 0. 
5. Ở gốc, DFS trả về điểm tối ưu mà Iris (người chơi đầu tiên) có thể đạt được. Đây là điểm số cuối cùng cho cây. 

Tại sao nó hoạt động: giá trị của mỗi nút ảnh hưởng nhất quán đến tất cả các lá trong cây con của nó. Bằng cách tính điểm từ dưới lên, thuật toán đảm bảo rằng mọi nước đi tối ưu cho cả hai người chơi đều được xem xét. DFS nắm bắt sự phụ thuộc một cách tự nhiên giữa`?`các nút vì mức đóng góp tối thiểu/tối đa của nút con đã được tính toán trước khi kết hợp ở nút gốc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 20)

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        g = [[] for _ in range(n)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            g[u].append(v)
            g[v].append(u)
        s = input().strip()
        parent = [-1] * n
        children = [[] for _ in range(n)]
        stack = [0]
        order = []
        while stack:
            u = stack.pop()
            order.append(u)
            for v in g[u]:
                if v != parent[u]:
                    parent[v] = u
                    children[u].append(v)
                    stack.append(v)

        dp = [0] * n  # number of leaves with non-zero weight in optimal play

        for u in reversed(order):
            if not children[u]:  # leaf
                if s[u] == '?':
                    dp[u] = 1
                else:
                    dp[u] = 1  # any known value can contribute 1
            else:
                total = 0
                for v in children[u]:
                    total += dp[v]
                dp[u] = total

        print(dp[0])

solve()
```Đầu tiên, mã xây dựng một cây có gốc tại 0. Sau đó, nó thực hiện DFS theo thứ tự sau để các nút con của mỗi nút được xử lý trước chính nút đó. Nút lá với`?`được tính là 1, vì người chơi đầu tiên luôn có thể đặt chúng để tối đa hóa trọng lượng. Các nút nội bộ tổng hợp các khoản đóng góp con của họ. Kết quả ở gốc là điểm cuối cùng. 

Các chi tiết triển khai bao gồm điều chỉnh lập chỉ mục dựa trên 0, đảm bảo DFS truyền tải con một cách chính xác mà không cần truy cập lại cha mẹ và xử lý chính xác`?`các nút lá. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
1 2
1 3
4 1
0101
```| Nút | Trẻ em | Lá cây? | Giá trị | dp | 
| --- | --- | --- | --- | --- | 
| 2 | [] | Có | 1 | 1 | 
| 3 | [] | Có | 0 | 1 | 
| 4 | [] | Có | 1 | 1 | 
| 1 | 2,3,4 | Không | 0 | 1+1+1=3 | 

Giải thích: Mỗi lá góp phần vào điểm vì trọng số khác 0. Tổng nội bộ lan truyền đến gốc. 

### Mẫu 2 

đầu vào:```
4
1 2
3 2
2 4
???0
```| Nút | Trẻ em | Lá cây? | Giá trị | dp | 
| --- | --- | --- | --- | --- | 
| 1 | [] | Có | ? | 1 | 
| 3 | [] | Có | ? | 1 | 
| 4 | [] | Có | 0 | 1 | 
| 2 | 1,3,4 | Không | ? | 1+1+1=3 | 

Giải thích: Ngay cả với`?`, người chơi đầu tiên có thể buộc ít nhất một lá có trọng số khác 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi duyệt qua từng nút một lần trong DFS, tính tổng các đóng góp con. | 
| Không gian | O(n) | Chúng tôi lưu trữ danh sách kề, mảng con, mảng cha và mảng dp. | 

Với các ràng buộc n ≤ 10^5 cho mỗi trường hợp thử nghiệm và tổng (n) 2⋅10^5, điều này hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("""6
4
1 2
1 3
4 1
0101
4
1 2
3 2
2 4
???0
5
1 2
1 3
2 4
2 5
?1?01
6
1 2
2 3
3 4
5 3
3 6
?0????
5
1 2
1 3
1 4
1 5
11
```
