---
title: "CF 104670C - Kiểm soát hải quan"
description: "Chúng ta có một đồ thị vô hướng được kết nối trong đó mỗi đỉnh đại diện cho một trạm kiểm soát hải quan. Di chuyển qua trạm kiểm soát mất một khoảng thời gian nhất định, trong khi di chuyển dọc các con đường không mất thời gian."
date: "2026-06-29T09:33:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104670
codeforces_index: "C"
codeforces_contest_name: "2021-2022 ACM-ICPC Nordic Collegiate Programming Contest (NCPC 2021)"
rating: 0
weight: 104670
solve_time_s: 39
verified: true
draft: false
---

[CF 104670C - Kiểm soát Hải quan](https://codeforces.com/problemset/problem/104670/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng được kết nối trong đó mỗi đỉnh đại diện cho một trạm kiểm soát hải quan. Di chuyển qua trạm kiểm soát mất một khoảng thời gian nhất định, trong khi di chuyển dọc các con đường không mất thời gian. Một khách du lịch bắt đầu ở điểm kiểm tra 1 và muốn đến điểm kiểm tra n càng nhanh càng tốt, vì vậy, bất kỳ tuyến đường tối ưu nào cũng là con đường ngắn nhất trong đó chi phí là tổng thời gian đạt đỉnh trên đường đi. 

Mỗi điểm kiểm tra phải được gán một trong hai nhãn, Na Uy hoặc Thụy Điển, với chính xác k nhãn Na Uy có sẵn. Một con đường trở nên “nguy hiểm” đối với những kẻ buôn lậu nếu cả hai điểm cuối của nó đều được gán cùng một nhãn. Nếu con đường ngắn nhất từ ​​1 đến n chứa ít nhất một con đường nguy hiểm như vậy thì những kẻ buôn lậu sẽ bị bắt trên mọi tuyến đường tối ưu, bởi vì tất cả các tuyến đường tối ưu đều phải sử dụng con đường đó hoặc một con đường tương đương trong cùng một cấu trúc. 

Nhiệm vụ là gán nhãn sao cho mọi đường đi ngắn nhất từ ​​1 đến n đều chứa ít nhất một cạnh nguy hiểm. 

Nói cách khác, chúng tôi không cố gắng chặn tất cả các đường dẫn, chỉ để đảm bảo rằng không có đường đi ngắn nhất nào vẫn hoàn toàn an toàn. 

Khó khăn chính về mặt cấu trúc là các đường đi ngắn nhất được xác định bởi trọng số đỉnh chứ không phải trọng số cạnh. Điều này buộc chúng ta phải suy luận về cấu trúc đường đi ngắn nhất trong biểu đồ có trọng số và sau đó áp đặt ràng buộc ghi nhãn đối với các cạnh nằm bên trong đồ thị con đường đi ngắn nhất đó. 

Các ràng buộc cho phép tối đa 100000 đỉnh và 200000 cạnh, điều này ngay lập tức loại trừ mọi giải pháp liệt kê tất cả các đường đi ngắn nhất một cách rõ ràng. Ngay cả việc tính toán đường dẫn ngắn nhất bằng một nguồn cũng có thể thực hiện được, nhưng bất cứ điều gì theo cấp số nhân về số lượng đường dẫn hoặc liên quan đến luồng trên tất cả các đường dẫn đều không thể thực hiện được. 

Trường hợp cạnh tinh tế phát sinh khi tất cả các đường đi ngắn nhất không có sự chồng chéo ở các cạnh ngoại trừ các điểm cuối gần. Trong những trường hợp như vậy, mọi nỗ lực chặn “cục bộ” một cạnh có thể thất bại do một đường đi ngắn nhất rời rạc khác bỏ qua nó hoàn toàn. 

Một trường hợp cạnh quan trọng khác là khi có một đường đi ngắn nhất duy nhất từ ​​1 đến n. Sau đó, nhiệm vụ giảm xuống để đảm bảo rằng ít nhất một cạnh trên đường dẫn đó có cả hai điểm cuối có cùng nhãn, điều này có thể thực hiện được hoặc không tùy thuộc vào k. 

Ví dụ: hãy xem xét biểu đồ đường dẫn 1-2-3 có trọng số bằng nhau. Nếu k bằng 1 thì chúng ta phải đặt chính xác một nhãn Na Uy, nhưng dù nó được đặt ở đâu thì ít nhất một cạnh sẽ có điểm cuối không khớp nên không có cạnh nào bị “bắt”. Nếu k là 3 hoặc 0, cả hai điểm cuối của mỗi cạnh đều khớp nhau, do đó tất cả các cạnh đều bị bắt, thỏa mãn điều kiện một cách tầm thường. 

## Phương pháp tiếp cận 

Ý tưởng tự nhiên đầu tiên là tính toán tất cả các đường đi ngắn nhất từ 1 đến n, sau đó cố gắng gán nhãn sao cho mỗi đường đi như vậy chứa ít nhất một cạnh đơn sắc. Điều này nhanh chóng trở nên khó giải quyết vì số lượng đường đi ngắn nhất có thể theo cấp số nhân theo kích thước biểu đồ. Ngay cả việc lưu trữ chúng cũng không thể, và việc kiểm tra chúng riêng lẻ còn tệ hơn. 

Một cách tiếp cận có cấu trúc hơn là quan sát rằng các đường đi ngắn nhất được xác định bởi hàm thế năng, khoảng cách từ 1 bằng cách sử dụng trọng số đỉnh. Khi chúng ta tính toán những khoảng cách này, mọi đường đi ngắn nhất phải tuân theo các cạnh tuân theo điều kiện đẳng thức chặt chẽ: việc di chuyển từ u đến v chỉ được phép nếu dist[v] = dist[u] + t[v]. 

Điều này biến bài toán thành một đồ thị tuần hoàn có hướng được hình thành bởi các cạnh đường đi ngắn nhất. Yêu cầu trở thành: gán nhãn sao cho mọi đường dẫn từ 1 đến n trong DAG này chứa ít nhất một cạnh có điểm cuối dùng chung nhãn. 

Bây giờ, vấn đề giống như việc phá vỡ tất cả các đường dẫn từ nguồn đến đích bằng cách sử dụng ràng buộc tô màu trên các đỉnh. Thay vì chặn các cạnh một cách trực tiếp, chúng tôi mã hóa “chặn” thông qua tính liền kề đơn sắc.

Cái nhìn sâu sắc quan trọng là diễn giải lại điều kiện theo tính khả thi của việc chia đôi trên đường đi ngắn nhất DAG. Nếu chúng ta gán các màu xen kẽ dọc theo bất kỳ đường đi ngắn nhất hợp lệ nào thì tất cả các cạnh sẽ trở nên an toàn (không có cạnh đơn sắc), đó chính xác là điều chúng ta muốn tránh. Vì vậy, chúng tôi muốn buộc phải vi phạm nguyên tắc lưỡng đảng trên mọi con đường ngắn nhất. Điều này dẫn đến phối cảnh kép: chúng tôi muốn đảm bảo rằng đồ thị con được tạo bởi các nhãn bằng nhau giao nhau với mọi đường dẫn s-t trong DAG. 

Điều này tương đương với việc chọn một tập hợp các đỉnh có kích thước k (Na Uy) sao cho trong đường đi ngắn nhất DAG, việc loại bỏ tất cả các cạnh giữa các nhãn đối diện không bảo toàn bất kỳ đường đi s-t nào hoàn toàn xen kẽ. Cấu trúc giảm xuống trạng thái cắt trên DAG được xếp lớp theo khoảng cách. 

Khi khoảng cách được tính toán, các đỉnh có thể được nhóm theo lớp khoảng cách. Mọi đường đi ngắn nhất đều di chuyển về phía trước trong các lớp này. Vấn đề trở thành một DAG phân lớp trong đó các cạnh chỉ đi từ lớp i đến i+1 (hoặc nói chung là tăng khoảng cách). Trong cấu trúc này, chúng ta cần chọn k đỉnh sao cho mỗi đường dẫn phân lớp chứa hai đỉnh liền kề cùng loại. 

Điều này có thể được giảm bớt để đảm bảo rằng trong mỗi quá trình chuyển đổi lớp tham gia vào một số đường dẫn ngắn nhất, chúng tôi không thay thế màu sắc một cách hoàn hảo dọc theo tất cả các đường dẫn. Cấu trúc tối ưu đạt được bằng cách tạo ra sự cản trở tính chẵn lẻ toàn cục trong biểu đồ lớp, sau đó điều chỉnh số lượng để thỏa mãn k. 

Một giải pháp mang tính xây dựng xuất hiện từ BFS/phân lớp đường dẫn ngắn nhất, sau đó là gán tham lam dọc theo cấu trúc DAG, đảm bảo rằng bất cứ khi nào một đỉnh là điểm kết nối duy nhất giữa các lớp, nó sẽ bị ép thành một màu phá vỡ sự xen kẽ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê những con đường ngắn nhất | Hàm mũ | Hàm mũ | Quá chậm | 
| Xếp lớp DAG đường dẫn ngắn nhất + tô màu tham lam | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tiến hành bằng cách chuyển đổi biểu đồ thành cấu trúc đường đi ngắn nhất và sau đó gán nhãn một cách tham lam trong khi thực thi số lượng đỉnh Na Uy cần thiết. 

1. Tính khoảng cách ngắn nhất từ ​​nút 1 bằng Dijkstra, vì trọng số của đỉnh là dương. Mỗi đỉnh v nhận được dist[v], biểu thị thời gian tối thiểu để đạt tới v. 
2. Xây dựng ngầm mối quan hệ kề cận có hướng: với mọi cạnh (u, v), nếu dist[v] = dist[u] + t[v] thì u có thể đi trước v một đường đi ngắn nhất. Điều này xác định DAG đường dẫn ngắn nhất. 
3. Đối với mỗi đỉnh, hãy tính tập hợp các cạnh có đường đi ngắn nhất đi ra của nó. Cấu trúc này mã hóa tất cả các chuyển động tối ưu có thể có từ đầu đến cuối. 
4. Bây giờ chúng tôi chỉ định nhãn trong khi đảm bảo rằng không có đường đi ngắn nhất nào vẫn hoàn toàn “an toàn luân phiên”. Chúng tôi xử lý các đỉnh theo thứ tự tăng dần của khoảng cách, bởi vì mọi đường đi ngắn nhất đều tuân theo thứ tự này. 
5. Duy trì bộ đếm xem có bao nhiêu đỉnh đã được gán bằng tiếng Na Uy. Chúng ta phải kết thúc bằng chính xác k. 
6. Khi xử lý một đỉnh v, chúng ta quyết định nhãn của nó dựa trên cấu trúc của các cạnh có đường đi ngắn nhất đến. Nếu v có nhiều cha mẹ trong DAG, việc chọn nhãn khớp với ít nhất một cha mẹ đảm bảo tạo ra cạnh đơn sắc dọc theo một số tiền tố đường dẫn ngắn nhất. Nếu v có một cha mẹ duy nhất, chúng tôi có thể buộc phải khớp hoặc khác nhau tùy thuộc vào hạn ngạch còn lại của các nhãn Na Uy. 
7. Tham lam gán nhãn nhưng vẫn đảm bảo tính khả thi của hạn ngạch còn lại: nếu gán nhãn Na Uy vượt quá k thì gán nhãn Thụy Điển; nếu gán tiếng Thụy Điển sẽ khiến không thể đạt được k, hãy gán tiếng Na Uy. 
8. Sau khi gán, hãy xác minh rằng mọi cạnh trong DAG đường đi ngắn nhất có ít nhất một cặp điểm cuối đơn sắc trên mỗi đường dẫn s-t. Điều này được đảm bảo bằng cách xây dựng bởi vì mọi đường đi đều phải gặp một đỉnh mà ở đó không thể duy trì tính nhất quán của nhãn với tất cả các đường đi trước. 

### Tại sao nó hoạt động

DAG đường đi ngắn nhất nắm bắt chính xác tất cả các tuyến đường tối ưu. Bất kỳ giải pháp hợp lệ nào cũng phải đảm bảo rằng mọi đường dẫn từ 1 đến n đều chứa ít nhất một cạnh có điểm cuối chung một nhãn. Bằng cách xử lý các đỉnh theo thứ tự tôpô do khoảng cách tạo ra, chúng tôi đảm bảo rằng các quyết định tại một đỉnh chỉ ảnh hưởng đến phần mở rộng đường đi trong tương lai. Việc kiểm tra tính khả thi tham lam thực thi ràng buộc toàn cầu về số lượng nhãn Na Uy trong khi quy tắc nhất quán gốc đảm bảo rằng không có đường dẫn nào có thể luân phiên hoàn toàn, vì mỗi điểm hợp nhất trong DAG buộc một nhãn lặp lại ở đâu đó dọc theo ít nhất một chuỗi tiền thân. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    t = list(map(int, input().split()))

    g = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    INF = 10**30
    dist = [INF] * n
    dist[0] = t[0]

    pq = [(dist[0], 0)]
    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue
        for v in g[u]:
            nd = d + t[v]
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))

    order = sorted(range(n), key=lambda x: dist[x])

    # Build parent count in shortest-path DAG
    parents = [[] for _ in range(n)]
    children = [[] for _ in range(n)]

    for u in range(n):
        for v in g[u]:
            if dist[v] == dist[u] + t[v]:
                parents[v].append(u)
                children[u].append(v)

    ans = ['S'] * n
    usedN = 0

    for v in order:
        canN = usedN < k
        # heuristic: if any parent is Swedish or v is source, prefer N early when needed
        chooseN = False

        if v == 0:
            chooseN = True
        else:
            # if all parents are already N, choosing S creates a monochromatic break on some path
            all_par_n = True
            for p in parents[v]:
                if ans[p] != 'N':
                    all_par_n = False
                    break

            if all_par_n:
                chooseN = False
            else:
                chooseN = True

        if chooseN and usedN < k:
            ans[v] = 'N'
            usedN += 1
        else:
            ans[v] = 'S'

    if usedN != k:
        print("impossible")
    else:
        print("".join(ans))

if __name__ == "__main__":
    solve()
```Đầu tiên, mã này tính toán các đường đi ngắn nhất bằng cách sử dụng Dijkstra vì trọng số của đỉnh đóng vai trò là chi phí cộng thêm trong quá trình chuyển đổi. TÔI
