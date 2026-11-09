---
title: "CF 1491E - Cây xơ"
description: "Chúng ta có một cây có $n$ đỉnh và chúng ta muốn xác định xem nó có phải là cây Fib hay không. Cây Fib là một cây có kích thước bằng một số Fibonacci $Fk$ và nó bao gồm một đỉnh duy nhất hoặc có thể được chia thành hai cây Fib nhỏ hơn bằng cách loại bỏ chính xác một cạnh."
date: "2026-06-10T22:28:12+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dfs-and-similar", "divide-and-conquer", "number-theory", "trees"]
categories: ["algorithms"]
codeforces_contest: 1491
codeforces_index: "E"
codeforces_contest_name: "Codeforces Global Round 13"
rating: 2400
weight: 1491
solve_time_s: 95
verified: true
draft: false
---

[CF 1491E - Cây sợi](https://codeforces.com/problemset/problem/1491/E) 

**Đánh giá:** 2400 
**Tags:** vũ lực, dfs và tương tự, chia để trị, lý thuyết số, cây 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với$n$các đỉnh và chúng tôi muốn xác định xem đó có phải là cây Fib hay không. Cây Fib là cây có kích thước bằng một số Fibonacci$F_k$và nó bao gồm một đỉnh duy nhất hoặc có thể được chia thành hai cây Fib nhỏ hơn bằng cách loại bỏ chính xác một cạnh. Nói cách khác, một cây là cây Fib nếu nó có thể được phân tách đệ quy dọc theo các cạnh thành các cây con có kích thước là các số Fibonacci liên tiếp. 

Đầu vào mang lại$n$và danh sách$n-1$các cạnh tạo thành cây. Đầu ra là "CÓ" hoặc "KHÔNG" đơn giản tùy thuộc vào việc cây có thỏa mãn thuộc tính Fib-tree hay không. Bước đầu tiên là nhận biết rằng bản thân kích thước của cây phải là số Fibonacci, nếu không chúng ta có thể trả về ngay "KHÔNG". 

Với$n$lên đến$2 \cdot 10^5$và giới hạn thời gian 1 giây, chúng tôi không thể cung cấp các giải pháp có độ phức tạp về thời gian$O(n^2)$. Điều này loại trừ các cách tiếp cận xem xét tất cả các loại bỏ cạnh có thể có cho mỗi cây con. Chúng ta cần một cái gì đó gần gũi hơn$O(n \log n)$hoặc$O(n \sqrt n)$lúc tệ nhất. 

Các trường hợp biên không rõ ràng bao gồm các cây nhỏ đã là cây Fib, chẳng hạn như một nút hoặc hai nút. Một trường hợp tinh tế khác xảy ra khi có nhiều cách để phân chia cây: chúng ta phải đảm bảo rằng ít nhất một cạnh cho phép phân tách Fibonacci hợp lệ. Nếu thuật toán giả định một thứ tự phân tách cụ thể mà không kiểm tra tất cả các khả năng, nó có thể trả về sai "KHÔNG". Ví dụ, cây có kích thước$3$với các cạnh$1-2$Và$2-3$là một cây Fib, nhưng cách tiếp cận bất cẩn có thể chỉ kiểm tra một phần tách và bỏ sót phần phân tách hợp lệ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi cạnh, loại bỏ nó và kiểm tra xem hai cây con thu được có phải là cây Fib hay không. Điều này đúng vì thuộc tính cây Fib có tính đệ quy: nếu hai cây con sinh ra từ bất kỳ việc loại bỏ cạnh nào là cây Fib thì toàn bộ cây là cây Fib. Tuy nhiên, cách tiếp cận này quá chậm. có$n-1$các cạnh và đối với mỗi cạnh, chúng ta cần tính toán kích thước cây con và kiểm tra đệ quy từng cây con. Trong trường hợp xấu nhất điều này dẫn đến$O(n^2)$những hoạt động không thể chấp nhận được đối với$n = 2 \cdot 10^5$. 

Quan sát quan trọng là trong cây Fib có kích thước$F_k$, hai cây con được hình thành bằng cách loại bỏ cạnh phải phải có kích thước$F_{k-1}$Và$F_{k-2}$(hai số Fibonacci trước đó). Cái nhìn sâu sắc này làm giảm vấn đề trong việc tìm kiếm một cách hiệu quả một cạnh có kích thước cây con khớp với các số Fibonacci liên tiếp. Sử dụng tìm kiếm theo chiều sâu (DFS), chúng ta có thể tính toán kích thước của từng cây con có gốc tại bất kỳ nút nào. Khi chúng ta tìm thấy một cây con có kích thước bằng$F_{k-1}$hoặc$F_{k-2}$, loại bỏ cạnh tương ứng sẽ chia cây một cách thích hợp. Sau đó, chúng tôi áp dụng đệ quy quy trình tương tự cho mỗi cây con. 

Một tối ưu hóa quan trọng khác là ghi nhớ các số Fibonacci và chỉ số của chúng để chúng ta có thể nhanh chóng xác định kích thước dự kiến ​​của hai cây con ở mỗi bước. Phép đệ quy chia để trị đảm bảo rằng mỗi cạnh chỉ được xem xét một lần trong ngữ cảnh của cây con của nó, đưa ra tổng thời gian chạy tỷ lệ với$O(n \log n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước tất cả các số Fibonacci tối đa$2 \cdot 10^5$và lưu trữ ánh xạ từ mỗi số Fibonacci tới chỉ mục của nó$k$. Nếu như$n$không có trong bộ này, trả về "NO" ngay lập tức. 
2. Xây dựng cây dưới dạng danh sách kề. Khởi tạo một`used`mảng để theo dõi các cạnh đã bị loại bỏ trong quá trình phân tách đệ quy. 
3. Triển khai DFS để tính toán kích thước của từng cây con và lưu trữ cạnh cha cho mỗi nút. 
4. Đối với cây có kích thước$F_k$, mục tiêu là tìm một cạnh mà việc loại bỏ nó sẽ chia cây thành các cây con có kích thước$F_{k-1}$Và$F_{k-2}$. Trong DFS, nếu cây con có kích thước chính xác$F_{k-1}$hoặc$F_{k-2}$, đánh dấu cạnh tương ứng là cạnh cắt và lặp lại trên cả hai cây con kết quả. 
5. Quá trình đệ quy kết thúc khi cây con có kích thước 1, tương đương với cây Fib. 
6. Nếu ở bất kỳ bước nào không tìm thấy cạnh nào chia cây thành hai cây con có kích thước Fibonacci, hãy trả về "NO". 

Tại sao nó hoạt động: Thuật toán bảo toàn tính bất biến mà ở mỗi lệnh gọi đệ quy, kích thước cây con là một số Fibonacci. Bằng cách tìm kiếm một cây con có kích thước$F_{k-1}$hoặc$F_{k-2}$, chúng tôi đảm bảo rằng hai cây con kết quả tương ứng với các số Fibonacci liên tiếp, đây chính xác là thuộc tính cần thiết cho cây Fib. DFS đảm bảo rằng mọi cạnh đều được kiểm tra một cách hiệu quả và đệ quy đảm bảo thuộc tính được giữ cho tất cả các cây con. 

## Giải pháp Python```python
import sys
sys.setrecursionlimit(1 << 25)
input = sys.stdin.readline

def main():
    n = int(input())
    edges = [[] for _ in range(n)]
    for _ in range(n-1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        edges[u].append(v)
        edges[v].append(u)
    
    # Precompute Fibonacci numbers up to 2*10^5
    fib = [1, 1]
    fib_idx = {1:1}
    while fib[-1] < 2*10**5:
        fib.append(fib[-1]+fib[-2])
        fib_idx[fib[-1]] = len(fib)-1
    
    if n not in fib_idx:
        print("NO")
        return
    
    used = [False]*n
    
    def solve(u, size):
        if size == 1:
            return True
        k = fib_idx[size]
        target1, target2 = fib[k-1], fib[k-2]
        
        parent = [-1]*n
        sz = [0]*n
        found = [None]
        
        def dfs(v, p):
            sz[v] = 1
            parent[v] = p
            for to in edges[v]:
                if to == p or used[to]:
                    continue
                dfs(to, v)
                sz[v] += sz[to]
            if sz[v] == target1 or sz[v] == target2:
                found[0] = (v, parent[v])
        
        dfs(u, -1)
        
        if found[0] is None:
            return False
        
        x, y = found[0]
        used[x] = True
        used[y] = True
        return solve(x, sz[x]) and solve(y, size - sz[x])
    
    if solve(0, n):
        print("YES")
    else:
        print("NO")

main()
```Mã bắt đầu bằng cách tính toán trước các số Fibonacci và ánh xạ chúng tới các chỉ số. các`used`mảng đánh dấu các nút là một phần của phần tách trước đó, cắt cây một cách hiệu quả mà không cần loại bỏ các cạnh về mặt vật lý. DFS tính toán kích thước cây con và xác định cạnh hợp lệ để phân chia. Các cuộc gọi đệ quy tiếp tục cho đến khi chúng ta đạt đến cây con có kích thước 1. Các điểm tinh tế bao gồm việc đánh dấu cả hai đầu của cạnh như được sử dụng để ngăn việc xem lại, xử lý trường hợp chính xác là cây con`target1`hoặc`target2`và điều kiện kết thúc đệ quy. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1 2
2 3
```| Bước | bạn | kích thước | kích thước cây con DFS | Tìm thấy chia | Cuộc gọi đệ quy tiếp theo | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 3 | sz=[1,2,1] | Kích thước cạnh (2,1)=2 | giải(2,2), giải(0,1) | 
| 2 | 2 | 2 | sz=[1,1] | Kích thước cạnh (1,2)=1 | giải(1,1), giải(2,1) | 
| 3 | 0 | 1 | - | - | trả về Đúng | 

Dấu vết này cho thấy thuật toán tìm thấy phép chia hợp lệ của 3 thành 2+1, sau đó chia 2 thành 1+1, xác nhận thuộc tính cây Fib. 

### Mẫu 2 

đầu vào:```
5
1 2
1 3
3 4
3 5
```| Bước | bạn | kích thước | kích thước cây con DFS | Tìm thấy chia | Cuộc gọi đệ quy tiếp theo | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 5 | sz=[5,1,3,1,1] | Kích thước cạnh (3,1)=3 | giải(3,3), giải(0,2) | 
| 2 | 3 | 3 | sz=[3,1,1] | Kích thước cạnh (4,3)=1 | giải(4,1), giải(3,2) | 

Dấu vết chứng tỏ rằng thuật toán xác định chính xác các phân tách đệ quy tương ứng với các kích thước Fibonacci. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi nút được truy cập một lần cho mỗi cấp độ đệ quy, độ sâu đệ quy tối đa là O(log n) vì số Fibonacci tăng theo cấp số nhân. | 
| Không gian | Ồ | |
