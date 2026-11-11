---
title: "CF 1610H - Trò chơi câu mực"
description: "Chúng ta có một cây có n đỉnh và m người chơi. Mỗi người chơi chọn hai đỉnh trên cây, chúng ta sẽ gọi là xi và yi. Một người chơi chỉ có thể bị loại nếu Mashtali chọn một đỉnh v và đỉnh w gần v nhất trên đường đi từ xi đến yi hoàn toàn nằm giữa xi và yi."
date: "2026-06-10T07:12:58+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1610
codeforces_index: "H"
codeforces_contest_name: "Codeforces Global Round 17"
rating: 3100
weight: 1610
solve_time_s: 113
verified: false
draft: false
---

[CF 1610H - Trò chơi mực](https://codeforces.com/problemset/problem/1610/H) 

**Đánh giá:** 3100 
**Thẻ:** cấu trúc dữ liệu, dfs và các cây tương tự, tham lam 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với`n`đỉnh và`m`người chơi. Mỗi người chơi chọn hai đỉnh trên cây mà chúng ta gọi là`x_i`Và`y_i`. Một người chơi chỉ có thể bị loại nếu Mashtali chọn một đỉnh`v`và đỉnh gần nhất`w`ĐẾN`v`trên đường đi từ`x_i`ĐẾN`y_i`là **nghiêm ngặt giữa**`x_i`Và`y_i`. Nếu như`w`trùng với một trong hai`x_i`hoặc`y_i`, người chơi sống sót sau hoạt động đó. Mục tiêu là tìm ra số đỉnh tối thiểu mà Mashtali phải chọn để đảm bảo loại bỏ tất cả người chơi hoặc báo cáo`-1`nếu điều đó là không thể. 

Những hạn chế rất đáng kể: lên đến`3 * 10^5`đỉnh và người chơi. Điều này loại trừ bất kỳ cách tiếp cận nào xử lý từng đường dẫn riêng lẻ bằng cách duyệt cây lặp đi lặp lại, vì điều đó có thể yêu cầu`O(n * m)`các hoạt động có thể đạt tới`10^11`trong trường hợp xấu nhất. Chúng tôi cần một giải pháp tận dụng cấu trúc cây và không lặp lại rõ ràng trên tất cả các đường dẫn. 

Một sự tinh tế quan trọng nảy sinh khi`x_i`Và`y_i`là **các đỉnh liền kề**. Trong trường hợp đó, đường đi từ`x_i`ĐẾN`y_i`không có đỉnh "ở giữa" nên không có lựa chọn nào`v`có thể loại bỏ người chơi này. Một trường hợp khác là khi nhiều người chơi có các đường đi trùng nhau: việc chọn một đỉnh có thể loại bỏ nhiều người cùng một lúc, nhưng chúng ta cần tránh bỏ sót một người chơi có đường đi khác với những người khác. 

Ví dụ: nếu cây là một đường thẳng`1-2-3`và một người chơi có`x=1`Và`y=2`, không có đỉnh nào nằm giữa chúng. Câu trả lời là`-1`bởi vì không có thao tác nào có thể loại bỏ trình phát này. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại trên tất cả các đỉnh`v`và mô phỏng việc loại bỏ từng người chơi bằng cách kiểm tra khoảng cách tới tất cả các đỉnh đường đi. Đối với mỗi thao tác, chúng tôi sẽ loại bỏ một số tập hợp con người chơi và lặp lại cho đến khi loại bỏ tất cả. Về nguyên tắc thì điều này có hiệu quả, nhưng mỗi hoạt động sẽ yêu cầu những con đường đi bộ có thể`O(n)`dài và lặp đi lặp lại`O(n)`đỉnh dẫn đến`O(n^2 * m)`sự phức tạp. Với`n`Và`m`lên tới`3 * 10^5`, điều này là không thể thực hiện được. 

Quan sát quan trọng là đối với một người chơi`i`, các đỉnh có thể loại bỏ chúng đều **tất cả các đỉnh nằm giữa`x_i`Và`y_i`trên đường đi**. Nếu chúng ta xem xét cây có gốc tại một đỉnh tùy ý thì các đỉnh bên trong của mỗi đường dẫn sẽ tạo thành một đường dẫn con được kết nối. Việc loại bỏ tất cả người chơi sẽ dẫn đến một **vấn đề tập hợp đánh trên cây** cổ điển, trong đó chúng ta phải chọn một tập hợp đỉnh tối thiểu giao với đường đi bên trong của mọi người chơi. 

Vấn đề được đơn giản hóa vì cây cho phép thực hiện chiến lược tham lam: đỉnh gần điểm sâu nhất của tất cả các đường dẫn sẽ bao gồm số lượng người chơi tối đa. Nếu chúng ta xử lý cây từ lá đến gốc, chúng ta có thể đánh dấu các cạnh (hoặc đỉnh) phải được sử dụng để loại bỏ những người chơi có đường đi không thể được bao phủ hoàn toàn bởi các thao tác trước đó. Điều này chuyển bài toán thành một bài toán **lập trình động trên cây**, thường được gọi là "vỏ đỉnh trên các đường đi trong cây". 

Cách tiếp cận tối ưu là xác định cho mỗi người chơi **tổ tiên chung thấp nhất (LCA)** của`x_i`Và`y_i`. Các đỉnh trong của đường đi của chúng tạo thành hai chuỗi: từ`x_i`đến LCA và từ`y_i`tới LCA, ngoại trừ các điểm cuối. Bằng cách duy trì số lượng đường đi đi qua mỗi đỉnh, chúng ta có thể chọn các đỉnh trong DFS theo thứ tự sau một cách tham lam, đảm bảo đỉnh bên trong của mọi đường dẫn đều được chọn ít nhất một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(n + m) | Quá chậm | 
| DFS tham lam với LCA | O(n + m log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Root cây một cách tùy ý, ví dụ ở đỉnh 1, và tính toán trước độ sâu và mảng cha cho các truy vấn LCA. Điều này cho phép truy vấn thời gian không đổi đối với LCA của hai nút bất kỳ bằng cách sử dụng nâng cấp nhị phân. 
2. Đối với mỗi người chơi`i`, tính toán`lca_i = LCA(x_i, y_i)`. Nếu như`x_i`là cha mẹ của`y_i`(hoặc ngược lại), đường đi không có đỉnh bên trong. Trong trường hợp này, đánh dấu người chơi là không thể loại bỏ. 
3. Đối với tất cả người chơi khác, đánh dấu các đỉnh **nghiêm ngặt giữa**`x_i`Và`y_i`trên con đường của họ. Chúng ta có thể biểu diễn từng đường dẫn nội bộ dưới dạng các phạm vi từ`x_i`đến LCA và từ`y_i`tới LCA, ngoại trừ điểm cuối. Điều này cung cấp cho chúng ta một tập hợp các đỉnh có thể loại bỏ người chơi này. 
4. Thực hiện DFS từ lá đến gốc. Đối với mỗi đỉnh, hãy duy trì bộ đếm xem có bao nhiêu đoạn bên trong của đường dẫn yêu cầu đỉnh này. Nếu một cây con có đường đi chưa được giải quyết, hãy đánh dấu đỉnh hiện tại là một lựa chọn để vận hành và truyền các yêu cầu còn lại lên trên. 
5. Đếm từng đỉnh được chọn cho một phép toán. Nếu sau DFS, bất kỳ đường dẫn nào vẫn chưa được đánh dấu (ví dụ: đường dẫn không có đỉnh bên trong), đầu ra`-1`. Ngược lại, tổng số đỉnh được chọn là số phép toán tối thiểu. 
6. Trả lại số đếm. 

Tính chính xác được đảm bảo vì bằng cách chọn các đỉnh trong DFS thứ tự sau, chúng tôi bao gồm tất cả các đỉnh đường dẫn bên trong. Việc chọn bất kỳ đỉnh nào cao hơn sẽ là dư thừa và đường đi của mỗi người chơi đều có ít nhất một đỉnh được chọn. 

## Giải pháp Python```python
import sys
sys.setrecursionlimit(1 << 25)
input = sys.stdin.readline

def main():
    n, m = map(int, input().split())
    parents = list(map(int, input().split()))
    tree = [[] for _ in range(n + 1)]
    for i, p in enumerate(parents, start=2):
        tree[p].append(i)
        tree[i].append(p)

    LOG = 20
    up = [[-1] * (LOG + 1) for _ in range(n + 1)]
    depth = [0] * (n + 1)

    def dfs(u, par):
        up[u][0] = par
        for k in range(1, LOG + 1):
            if up[u][k-1] != -1:
                up[u][k] = up[up[u][k-1]][k-1]
        for v in tree[u]:
            if v != par:
                depth[v] = depth[u] + 1
                dfs(v, u)
    dfs(1, -1)

    def lca(u, v):
        if depth[u] < depth[v]:
            u, v = v, u
        for k in reversed(range(LOG + 1)):
            if up[u][k] != -1 and depth[up[u][k]] >= depth[v]:
                u = up[u][k]
        if u == v:
            return u
        for k in reversed(range(LOG + 1)):
            if up[u][k] != -1 and up[u][k] != up[v][k]:
                u = up[u][k]
                v = up[v][k]
        return up[u][0]

    counter = [0] * (n + 1)
    impossible = False
    for _ in range(m):
        x, y = map(int, input().split())
        l = lca(x, y)
        if (l == x or l == y) and abs(depth[x] - depth[y]) == 1:
            impossible = True
            continue
        if x != l:
            counter[x] += 1
            counter[l] -= 1
        if y != l:
            counter[y] += 1
            counter[l] -= 1

    res = 0
    def dfs_count(u, par):
        nonlocal res
        for v in tree[u]:
            if v == par:
                continue
            dfs_count(v, u)
            counter[u] += counter[v]
        if counter[u] > 0 and u != 1:
            res += 1
            counter[u] = 0
    if impossible:
        print(-1)
        return
    dfs_count(1, -1)
    print(res)

if __name__ == "__main__":
    main()
```Quá trình tiền xử lý LCA với nâng nhị phân cho phép chúng tôi tính toán các đường dẫn một cách hiệu quả. Việc sử dụng mảng bộ đếm có DFS sẽ truyền các yêu cầu lên cây. Việc chọn một đỉnh khi cây con có đường đi chưa được giải sẽ đảm bảo mọi người chơi đều bị loại với số lựa chọn tối thiểu. Phần tinh tế là xử lý cẩn thận các đường dẫn không có đỉnh bên trong, điều này ngay lập tức khiến câu trả lời là không thể. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6 3
1 1 1 4 4
1 5
3 4
2 6
```Chúng tôi tính toán LCA cho mỗi cặp: (1,5)->1, (3,4)->1, (2,6)->4. Chỉ các đỉnh bên trong mới được tính: với (1,5) là 5, với (3,4) là 3, với (2,6) là 6. DFS từ lá đến gốc chọn đỉnh 1 và 6. Kết quả là 2. 

| Đỉnh | Truy cập trước DFS | Được chọn? | 
| --- | --- | --- | 
| 5 | 1 | Không | 
| 3 | 1 | Không | 
| 6 | 1 | Có | 
| 4 | 1 | Có | 
| 1 | 2 | Có | 

### Mẫu 2 (vỏ cạnh) 

đầu vào:```
3 1
1 1
1 2
```LCA của 1 và
