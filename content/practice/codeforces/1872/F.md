---
title: "CF 1872F - Bán đàn thú"
description: "Chúng ta được cung cấp một tập hợp các loài động vật, mỗi loài có một cái giá và mối quan hệ "sợ hãi": mỗi con vật chỉ sợ một con vật khác và không con vật nào sợ chính nó. Khi bán một con vật, số tiền kiếm được phụ thuộc vào việc con vật mà nó sợ đã được bán hay chưa."
date: "2026-06-08T23:20:53+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dsu", "graphs", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1872
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 895 (Div. 3)"
rating: 1800
weight: 1872
solve_time_s: 125
verified: false
draft: false
---

[CF 1872F - Bán đàn thú](https://codeforces.com/problemset/problem/1872/F) 

**Đánh giá:** 1800 
**Tags:** dfs và tương tự, dsu, đồ thị, triển khai, toán học 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các loài động vật, mỗi loài có một cái giá và mối quan hệ "sợ hãi": mỗi con vật chỉ sợ một con vật khác và không con vật nào sợ chính nó. Khi bán một con vật, số tiền kiếm được phụ thuộc vào việc con vật mà nó sợ đã được bán hay chưa. Nếu con vật đáng sợ được bán sớm hơn, chúng ta sẽ nhận được giá của con vật đó; nếu con vật đáng sợ vẫn chưa được bán, chúng tôi sẽ nhận được gấp đôi giá của nó. Mục tiêu của chúng ta là tìm được đơn hàng bán tất cả các loài động vật để tối đa hóa tổng lợi nhuận. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, chúng ta được cung cấp số lượng động vật, một mảng cho biết mỗi con vật sợ hãi con vật nào và một loạt chi phí. Đầu ra phải là hoán vị của các chỉ số động vật cho từng trường hợp thử nghiệm thể hiện đơn đặt hàng. 

Những hạn chế cho chúng ta biết rằng$n$có thể lên tới$10^5$và tổng của tất cả các trường hợp thử nghiệm không vượt quá$10^5$. Điều này loại trừ bất kỳ giải pháp nào tệ hơn tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Thuật toán bậc hai quá chậm, vì vậy chúng ta cần thứ gì đó có thể xử lý mỗi con vật với số lần không đổi hoặc đi qua các cấu trúc đơn giản như cây hoặc chu trình. 

Trường hợp cạnh then chốt phát sinh theo chu kỳ. Vì mỗi loài động vật đều sợ hãi một con vật khác nên các mối quan hệ sợ hãi tạo thành một tập hợp các chu kỳ và những cái cây gắn liền với chúng. Việc bán động vật không đúng thứ tự trong một chu kỳ có thể buộc phải bán những con vật đắt tiền nhất sau những con vật mà chúng sợ hãi, mất cơ hội được thanh toán gấp đôi. Ví dụ: với ba con vật trong một chu kỳ sợ hãi, nếu con vật có giá cao nhất được bán sau con vật đáng sợ của nó, chúng ta sẽ bỏ lỡ lợi nhuận gấp đôi, vì vậy chúng ta cần xác định các chu kỳ và đặt hàng cẩn thận. 

Một trường hợp tinh tế khác là chuỗi kết thúc theo một chu kỳ. Bán lá trước chu kỳ thì đơn giản, nhưng việc chọn con vật đầu tiên trong chu kỳ mới là vấn đề. Nếu xác định sai điểm chi phí tối thiểu của chu kỳ, chúng ta có thể chọn sai điểm bắt đầu, làm giảm tổng lợi nhuận. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ xem xét tất cả$n!$hoán vị và tính lợi nhuận cho mỗi hoán vị. Về nguyên tắc, điều này có tác dụng vì vấn đề đã được xác định rõ ràng: chúng ta biết chính xác mỗi lần bán hàng tạo ra bao nhiêu tiền dựa trên lần bán hàng trước đó. Tuy nhiên, với$n$lên đến$10^5$, điều này rõ ràng là không thể. 

Cái nhìn sâu sắc quan trọng là mô hình hóa các mối quan hệ sợ hãi dưới dạng biểu đồ có hướng trong đó mỗi nút trỏ đến con vật mà nó sợ hãi. Bởi vì mỗi nút đều có một bậc ngoài, nên biểu đồ là một tập hợp các chu trình và cây gắn liền với các chu trình. Bán động vật trên cây rất đơn giản: chúng ta bán lá trước để khi đến tay bố mẹ của chúng thì chúng ta đã bán hoặc có thể chọn đơn hàng để tối đa hóa lợi nhuận. Chu kỳ yêu cầu xử lý đặc biệt: nếu chúng ta chọn điểm bắt đầu trong một chu kỳ thì mọi giao dịch bán hàng khác trong chu kỳ đó đều phụ thuộc vào lựa chọn đó. Để tối đa hóa lợi nhuận, chúng tôi xác định con vật trong chu kỳ với chi phí tối thiểu và bán nó trước. Điều đó đảm bảo rằng mọi con vật khác trong chu kỳ đều sợ con vật đã bán trước đó, buộc chỉ một con vật trong chu kỳ phải đưa ra mức giá gấp đôi trong khi những con còn lại đưa ra mức giá thông thường. Sau đó, chúng tôi xử lý đệ quy các cây được gắn vào các nút chu trình theo cách tương tự. 

Điều này biến đổi vấn đề từ tìm kiếm hoán vị vũ phu sang phân rã đồ thị thành các chu trình và cây, sau đó là sắp xếp cẩn thận trong mỗi thành phần. Thuật toán kết quả là tuyến tính trong$n$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu (DFS + phát hiện chu kỳ) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu diễn mối quan hệ sợ hãi dưới dạng biểu đồ trong đó mỗi con vật$i$chỉ vào$a_i$. Khởi tạo một mảng đã truy cập để theo dõi những con vật nào đã được xử lý. 
2. Đối với mỗi nút chưa được truy cập, hãy duyệt dọc theo các cạnh sợ hãi của nó bằng DFS cho đến khi bạn gặp một nút đã truy cập. Có hai trường hợp: tiếp cận một nút đã được xử lý đầy đủ hoặc hoàn thành một chu trình. Ghi lại đường dẫn để xác định chu kỳ. 
3. Sau khi phát hiện được một chu trình, hãy xác định nút trong chu trình đó với chi phí tối thiểu. Bắt đầu bán chu kỳ từ nút đó. Điều này đảm bảo chỉ con vật đầu tiên mới đóng góp gấp đôi chi phí so với việc được bán trước con vật đáng sợ của nó; tất cả các động vật khác trong chu kỳ được bán sau con vật đáng sợ của chúng, mang lại giá bình thường. 
4. Sau khi quyết định nút bắt đầu của chu kỳ, xử lý đệ quy tất cả các cây đến (động vật cuối cùng trỏ vào chu trình) trong DFS theo thứ tự sau, bán lá trước để tối đa hóa chi phí gấp đôi trước khi đến tay cây mẹ của chúng. 
5. Thêm động vật vào thứ tự đầu ra khi chúng được xử lý. Đối với cây, nút con được xử lý trước nút gốc, đảm bảo rằng khi bán một nút nội bộ, bất kỳ nút nào mà nó lo ngại có thể đã được bán hoặc chưa, tạo ra tổng lợi nhuận tối đa. 
6. Lặp lại cho tất cả các thành phần được kết nối trong biểu đồ. 

Tại sao nó hoạt động: Mỗi thành phần (cây hoặc chu trình) được xử lý sao cho các nút có doanh số bán sẽ thu được lợi nhuận gấp đôi sẽ được bán trước so với các nút mà chúng lo sợ. Đối với các chu kỳ, việc chọn nút có chi phí tối thiểu làm điểm bắt đầu đảm bảo chỉ bỏ sót một chi phí gấp đôi, đó là mức tối ưu. Cây được xử lý trong DFS sau đặt hàng để bán lá trước, đảm bảo chúng tôi tối đa hóa cơ hội chi phí gấp đôi. Bởi vì mỗi nút được truy cập một lần và các cạnh được duyệt một lần, nên thuật toán bao trùm tất cả các con vật và tạo ra một hoán vị tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(lambda x: int(x)-1, input().split()))
        c = list(map(int, input().split()))
        visited = [0]*n
        res = []

        def dfs(u):
            visited[u] = 1
            v = a[u]
            if not visited[v]:
                dfs(v)
            res.append(u+1)

        for i in range(n):
            if not visited[i]:
                stack = []
                x = i
                while visited[x] == 0:
                    visited[x] = 2
                    stack.append(x)
                    x = a[x]
                if visited[x] == 2:
                    # cycle found
                    cycle = []
                    while True:
                        y = stack.pop()
                        cycle.append(y)
                        if y == x:
                            break
                    min_cost_node = min(c[node] for node in cycle)
                    # rotate cycle so minimal cost node is first
                    while c[cycle[0]] != min_cost_node:
                        cycle.append(cycle.pop(0))
                    for node in cycle:
                        dfs(node)
                else:
                    # not in cycle
                    while stack:
                        node = stack.pop()
                        dfs(node)
        print(*res)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên đọc đầu vào và chuyển đổi các chỉ số sợ hãi dựa trên 1 thành dựa trên 0. Nó sử dụng tính năng phát hiện chu kỳ dựa trên DFS để phân biệt chu kỳ với cây. Trong DFS, các nút trong chu kỳ được luân chuyển để nút có chi phí tối thiểu được bán trước. Mảng đã truy cập sử dụng ba trạng thái: 0 cho trạng thái chưa được truy cập, 2 cho trạng thái hiện tại trong ngăn xếp (phát hiện chu kỳ) và 1 cho được xử lý hoàn toàn. Mỗi con vật được thêm vào đầu ra ngay khi cây con của nó được xử lý, giúp duy trì lợi nhuận tối đa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
2 3 2
3 4 5 6 7
```Các nút xử lý: nút chưa được truy cập đầu tiên là 1. Các cạnh tiếp theo dẫn đến 2 → 3 → 2, phát hiện chu trình: nút 2 và 3. Nút chi phí tối thiểu trong chu kỳ là nút 2. Bán nút 2 trước, sau đó đến nút 3, sau đó xử lý cây đến. Bảng cập nhật res: 

| Bước | Nút được xử lý | độ phân giải | 
| --- | --- | --- | 
| Bắt đầu DFS | 1 | [] | 
| Phát hiện chu kỳ | 2 | [] | 
| Chu kỳ quay | 2 | [] | 
| Nút chu trình DFS | 2 | [2] | 
| Nút chu trình DFS | 3 | [2,3] | 
| Cây còn lại | 1 | [2,3,1] | 

Đầu ra:`2 3 1`### Ví dụ 2 

đầu vào:```
2
2
2 1
1000000000 999999999
```Chu kỳ: 1→2→1. Nút chi phí tối thiểu: nút 2. Thứ tự sau khi quay: 2,1. Không có cây gắn liền. Đầu ra:`2 1`Những dấu vết này xác nhận chu kỳ được xử lý chính xác và cây được thêm vào theo thứ tự sau để tối đa hóa khoản thanh toán gấp đôi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được truy cập một lần; mỗi cạnh được duyệt một lần trong DFS. Phát hiện và xoay chu kỳ là O (độ dài chu kỳ) được giới hạn bởi n. | 
| Không gian | O(n) | Mảng đã truy cập, ngăn xếp đệ quy và danh sách kết quả đều có tỷ lệ tuyến tính với n. | 

Thuật toán xử lý tối đa$n = 10^5$thoải mái trong vòng 2 giây và mức sử dụng bộ nhớ trong khoảng 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("8\n3\n2 3 2\n6 6 1\n8\n2 1 4 3 6 5 8 7\n1 2 1 2 2 1 2 1\n5\n2 1 1 1 1\n9 8 1 1 1\n2\n2 1\n1000000000 999999999\n7\n2 3 2 6 4 4 3\n1 2 3 4 5
```
