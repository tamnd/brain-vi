---
title: "CF 105315G - Sinh Nhật Nagham"
description: "Chúng ta được cung cấp một biểu đồ có trọng số có hướng hoặc không có hướng cho từng trường hợp thử nghiệm, cùng với nút bắt đầu và nút mục tiêu được chỉ định."
date: "2026-06-23T15:06:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105315
codeforces_index: "G"
codeforces_contest_name: "JPC 4.0"
rating: 0
weight: 105315
solve_time_s: 55
verified: true
draft: false
---

[CF 105315G - Sinh nhật của Nagham](https://codeforces.com/problemset/problem/105315/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có trọng số có hướng hoặc không có hướng cho từng trường hợp thử nghiệm, cùng với nút bắt đầu và nút mục tiêu được chỉ định. Lộ trình là bất kỳ chuỗi các cạnh nào kết nối điểm bắt đầu với mục tiêu và các đỉnh và cạnh có thể lặp lại miễn là chúng tạo thành một bước đi hợp lệ trong biểu đồ. Giá trị của tuyến đường như vậy được xác định là AND theo bit của tất cả các trọng số cạnh dọc theo tuyến đường đó. Nhiệm vụ là tối đa hóa giá trị này trong số tất cả các tuyến hợp lệ có thể có từ đầu đến đích. 

Chi tiết cấu trúc quan trọng là cách hoạt động theo bit AND trên các đường dẫn. Khi một bit trở thành 0 trong bất kỳ trọng số cạnh nào được sử dụng dọc theo tuyến đường, bit đó sẽ bị mất vĩnh viễn đối với toàn bộ giá trị tuyến đường. Vì vậy, các tuyến đường dài hơn chỉ hạn chế giá trị cuối cùng, nhưng chúng vẫn có thể hữu ích vì chúng cho phép chúng ta tiếp cận các cạnh bảo toàn nhiều bit cao hơn. 

Các ràng buộc ngụ ý rằng chúng ta không thể liệt kê các đường dẫn. Với tối đa 10^5 nút và 3×10^5 cạnh cho mỗi trường hợp thử nghiệm, bất kỳ giải pháp nào phụ thuộc vào việc khám phá tất cả các bước đi hoặc thậm chí tất cả các đường dẫn đơn giản đều không thể thực hiện được. Ngay cả việc lập trình động theo kiểu đường dẫn ngắn nhất trên các tập hợp con cũng nằm ngoài tầm với. Hướng khả thi duy nhất là xử lý các bit một cách độc lập và giảm vấn đề xuống việc kiểm tra khả năng tiếp cận lặp đi lặp lại trong quá trình lọc cạnh. 

Một trường hợp thất bại tinh tế xuất hiện nếu chúng ta giả sử câu trả lời chỉ đơn giản là trọng số cạnh tối thiểu dọc theo một số đường đi ngắn nhất hoặc bất kỳ đường đi ngắn nhất nào. Ví dụ: hãy xem xét một biểu đồ trong đó tồn tại một đường dẫn trực tiếp từ s đến e với cạnh có trọng số thấp, nhưng chu trình dài hơn cho phép một đường dẫn khác chỉ sử dụng các cạnh có trọng số cao. Cách tiếp cận đường đi ngắn nhất tham lam sẽ chọn không chính xác cạnh trực tiếp và tạo ra giá trị AND nhỏ hơn, mặc dù tồn tại tuyến đường bảo toàn AND tốt hơn. 

Một cạm bẫy khác là giả sử tính đơn điệu về độ dài đường đi hoặc trọng số cạnh. Việc thêm các cạnh không nhất thiết phải làm giảm AND theo cách có thể dự đoán được trừ khi chúng ta thực thi một ràng buộc về những bit nào được phép. Đây là lý do tại sao vấn đề tự nhiên chuyển thành vấn đề lọc bit và kết nối thay vì con đường ngắn nhất cổ điển. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là xem xét tất cả các bước đi có thể từ s đến e và tính toán AND theo từng bit của trọng số dọc theo mỗi bước. Điều này đúng vì nó đánh giá rõ ràng mọi tuyến đường hợp lệ. Tuy nhiên, số lần đi bộ tăng theo cấp số nhân vì các chu kỳ có thể đi qua tùy ý nhiều lần. Ngay cả việc hạn chế các đường dẫn đơn giản cũng không giúp ích được gì, vì việc liệt kê chúng trong một biểu đồ dày đặc vẫn theo cấp số nhân trong trường hợp xấu nhất. 

Quan sát quan trọng là lật ngược quan điểm. Thay vì xây dựng các đường dẫn, chúng tôi quyết định những bit nào chúng tôi muốn giữ lại trong câu trả lời cuối cùng. Giả sử chúng ta đoán rằng một mặt nạ bit X nhất định có thể đạt được. Điều đó có nghĩa là tồn tại một đường đi từ s tới e sao cho mọi cạnh trên đường đi đó đều có tất cả các bit của tập X. Nói cách khác, tất cả các cạnh trên đường đi thuộc về sơ đồ con được hình thành bằng cách lọc các cạnh có trọng số chứa X là tập hợp con các bit. Vì vậy, tính khả thi giảm xuống việc kiểm tra kết nối trong biểu đồ được lọc. 

Điều này dẫn đến việc xây dựng bit tham lam từ bit cao nhất trở xuống. Chúng tôi bắt đầu với câu trả lời = 0. Đối với mỗi bit từ quan trọng nhất đến ít quan trọng nhất, chúng tôi cố gắng đặt nó và kiểm tra xem s và e có còn được kết nối chỉ bằng các cạnh có trọng số chứa tất cả các bit hiện được yêu cầu hay không. Nếu kết nối được giữ, chúng tôi sẽ giữ lại bit; nếu không chúng tôi loại bỏ nó. 

Việc kiểm tra kết nối có thể được thực hiện bằng DFS, BFS hoặc DSU trên các cạnh được lọc. Vì chúng tôi lặp lại việc kiểm tra này với tối đa 60 bit nên tổng độ phức tạp vẫn có thể quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các bước đi) | Hàm mũ | O(n + m) | Quá chậm | 
| Tham lam bitwise + kiểm tra kết nối | O(60 × (n + m)) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một mặt nạ câu trả lời ứng viên mà chúng tôi xây dựng từng chút một từ cao xuống thấp.

1. Khởi tạo mặt nạ trả lời thành 0. Điều này biểu thị rằng ban đầu chúng ta đang cho phép tất cả các đường dẫn, vì chưa có ràng buộc bit nào được thực thi. 
2. Lặp lại các bit từ 59 xuống 0. Chúng tôi xem xét liệu mỗi bit có thể được đưa vào câu trả lời cuối cùng hay không. 
3. Đối với bit b nhất định, tạm thời xây dựng mặt nạ kiểm tra bằng câu trả lời hiện tại cộng với bit này. 
4. Xây dựng hoặc duyệt qua một sơ đồ con chỉ bao gồm các cạnh có trọng số chứa tất cả các bit trong mặt nạ kiểm tra. Bộ lọc này đảm bảo rằng bất kỳ đường dẫn nào được tìm thấy trong biểu đồ này sẽ bảo toàn tất cả các bit trong mặt nạ theo bit AND. 
5. Chạy tìm kiếm kết nối (thường là BFS hoặc DFS) từ s. Nếu chúng ta có thể tiếp cận e thì tồn tại một tuyến hợp lệ bảo toàn tất cả các bit trong mặt nạ kiểm tra, vì vậy chúng ta đặt vĩnh viễn bit này trong câu trả lời. 
6. Nếu không thể truy cập được e, hãy loại bỏ bit này và giữ nguyên câu trả lời trước đó. 

Lý do sự lựa chọn tham lam này là hợp lệ là vì các bit cao hơn chiếm ưu thế hơn các bit thấp hơn trong biểu diễn nhị phân. Khi một bit cao hơn được phát hiện là không khả thi thì không có thao tác nào sau này có thể khôi phục nó, vì vậy chúng tôi sẽ loại bỏ nó một cách an toàn. Ngược lại, nếu một bit khả thi, việc giữ nó không bao giờ ngăn cản chúng ta có khả năng thêm các bit thấp hơn sau này, vì các bit thấp hơn chỉ thắt chặt ràng buộc hơn nữa. 

### Tại sao nó hoạt động 

Ở bất kỳ bước nào, mặt nạ hiện tại đại diện cho một tập hợp các cạnh tương thích với mọi bit đã được chọn. Kiểm tra kết nối đảm bảo rằng tồn tại ít nhất một đường dẫn chứa đầy đủ trong sơ đồ con bị ràng buộc này. Do đó, bất biến là sau khi xử lý bit b, tồn tại một đường dẫn từ s đến e chỉ sử dụng các cạnh có trọng số chứa tất cả các bit được chọn. Vì các quyết định về bit chỉ hạn chế biểu đồ nên khi tính khả thi được xác nhận, nó vẫn nhất quán với tất cả các bit được chấp nhận trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, s, e = map(int, input().split())
        
        edges = []
        adj_all = [[] for _ in range(n + 1)]
        
        for _ in range(m):
            a, b, w = map(int, input().split())
            edges.append((a, b, w))
            adj_all[a].append((b, w))
            adj_all[b].append((a, w))
        
        def can(mask):
            from collections import deque
            vis = [False] * (n + 1)
            dq = deque([s])
            vis[s] = True
            
            while dq:
                u = dq.popleft()
                if u == e:
                    return True
                for v, w in adj_all[u]:
                    if not vis[v] and (w & mask) == mask:
                        vis[v] = True
                        dq.append(v)
            return False
        
        ans = 0
        for bit in range(59, -1, -1):
            if can(ans | (1 << bit)):
                ans |= (1 << bit)
        
        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai giữ một danh sách kề đầy đủ và thực hiện BFS cho mỗi lần thử bit. Kiểm tra chìa khóa`(w & mask) == mask`buộc mọi cạnh được sử dụng sẽ bảo toàn tất cả các bit hiện được yêu cầu trong câu trả lời. 

BFS được khởi động lại cho mỗi bit, điều này có thể chấp nhận được vì ràng buộc về số bit là nhỏ và kích thước đồ thị ở mức vừa phải. Độ chính xác phụ thuộc vào việc không sử dụng lại trạng thái đã truy cập giữa các mặt nạ khác nhau, vì khả năng tiếp cận thay đổi theo từng ràng buộc được thêm vào. 

## Ví dụ đã hoạt động 

Hãy xem xét một biểu đồ nhỏ trong đó có nhiều đường dẫn cạnh tranh trong việc bảo toàn bit. 

đầu vào:```
1
4 4 1 4
1 2 8
2 4 12
1 3 12
3 4 8
```Chúng tôi đánh giá các bit từ cao đến thấp. 

| Chút | Mặt nạ dùng thử | Có thể tiếp cận s→e | Quyết định | Câu trả lời hiện tại | 
| --- | --- | --- | --- | --- | 
| 3 (8) | 8 | có qua 1-2-4 | giữ | 8 | 
| 2 (4) | 12 | không | vứt bỏ | 8 | 
| 1 (2) | 10 | không | vứt bỏ | 8 | 
| 0 (1) | 9 | không | vứt bỏ | 8 | 

Điều này thể hiện cách thuật toán khóa bit khả thi cao nhất trước tiên và tránh các bit thấp hơn có thể làm hỏng kết nối. 

Một ví dụ thứ hai: 

đầu vào:```
1
3 3 1 3
1 2 7
2 3 3
1 3 2
```| Chút | Mặt nạ dùng thử | Có thể tiếp cận s→e | Quyết định | Câu trả lời hiện tại | 
| --- | --- | --- | --- | --- | 
| 2 (4) | 4 | không | vứt bỏ | 0 | 
| 1 (2) | 2 | có qua 1-3 | giữ | 2 | 
| 0 (1) | 3 | không (ràng buộc lỗi cạnh 2-3) | vứt bỏ | 2 | 

Kết quả cho thấy các đường dẫn gián tiếp có thể lấn át các cạnh trực tiếp khi bảo toàn các bit cao hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(60 × (n + m)) | Mỗi bit kích hoạt một BFS trên biểu đồ, quét tất cả các cạnh một lần | 
| Không gian | O(n + m) | Danh sách kề cộng với mảng đã truy cập BFS | 

Giới hạn lên tới 3×10^5 cạnh và kiểm tra 60 bit mang lại khoảng 1,8×10^7 độ giãn cạnh cho mỗi trường hợp xấu nhất của trường hợp kiểm thử, phù hợp thoải mái trong các giới hạn điển hình trong Python nếu được triển khai với danh sách kề và các phép toán số nguyên đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    def solve():
        input = sys.stdin.readline
        t = int(input())
        out = []
        for _ in range(t):
            n, m, s, e = map(int, input().split())
            adj = [[] for _ in range(n + 1)]
            for _ in range(m):
                a, b, w = map(int, input().split())
                adj[a].append((b, w))
                adj[b].append((a, w))
            
            from collections import deque
            
            def can(mask):
                vis = [False] * (n + 1)
                dq = deque([s])
                vis[s] = True
                while dq:
                    u = dq.popleft()
                    if u == e:
                        return True
                    for v, w in adj[u]:
                        if not vis[v] and (w & mask) == mask:
                            vis[v] = True
                            dq.append(v)
                return False
            
            ans = 0
            for bit in range(59, -1, -1):
                if can(ans | (1 << bit)):
                    ans |= (1 << bit)
            out.append(str(ans))
        return "\n".join(out)
    
    return solve()

# simple cases
assert run("1\n2 1 1 2\n1 2 7\n") == "7"
assert run("1\n2 1 1 2\n1 2 1\n") == "1"
assert run("1\n3 2 1 3\n1 2 6\n2 3 1\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đường dẫn một cạnh | trọng lượng toàn cạnh | khả thi trực tiếp | 
| Trường hợp bit tối thiểu | 1 | độ chính xác từng bit | 
| Ngắt kết nối sau khi lọc | 0 | không thể truy cập dưới những ràng buộc | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi đường dẫn duy nhất có sẵn yêu cầu giảm bớt các bit cao để duy trì kết nối. Giả sử một đồ thị trong đó s kết nối với e thông qua cầu có trọng số thấp, nhưng các cạnh có trọng số cao tạo thành một chu trình bị ngắt khỏi e. Thuật toán sẽ loại bỏ chính xác các bit cao vì BFS dưới mặt nạ bị ràng buộc không thể tiếp cận được mục tiêu, mặc dù tồn tại một đường dẫn trực quan dài hơn trong biểu đồ đầy đủ. 

Một trường hợp khác là khi có nhiều thành phần tồn tại và khả năng kết nối phụ thuộc vào mặt nạ chính xác. Ví dụ: các cạnh có thể kết nối s với thành phần ở giữa dưới mặt nạ X, nhưng e chỉ dưới một tập hợp con các cạnh khác. BFS tính toán lại khả năng tiếp cận mới cho từng mặt nạ, đảm bảo rằng các giả định về khả năng tiếp cận cũ không bị rò rỉ giữa các lần thử bit.
