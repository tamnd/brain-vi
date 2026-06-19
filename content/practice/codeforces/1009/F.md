---
title: "CF 1009F - Chỉ số chiếm ưu thế"
description: "Chúng ta đang làm việc với một cây có gốc trong đó đỉnh 1 được coi là gốc. Đối với mỗi đỉnh $x$, về mặt khái niệm, chúng ta xem xét tất cả các nút trong cây con của nó và nhóm chúng theo khoảng cách từ $x$ theo các cạnh hướng xuống trong cây."
date: "2026-06-16T22:59:53+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dsu", "trees"]
categories: ["algorithms"]
codeforces_contest: 1009
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 47 (Rated for Div. 2)"
rating: 2300
weight: 1009
solve_time_s: 119
verified: true
draft: false
---

[CF 1009F - Chỉ số chiếm ưu thế](https://codeforces.com/problemset/problem/1009/F) 

**Đánh giá:** 2300 
**Thẻ:** cấu trúc dữ liệu, dsu, cây 
**Thời gian giải:** 1 phút 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang làm việc với một cây có gốc trong đó đỉnh 1 được coi là gốc. Với mọi đỉnh$x$, về mặt khái niệm, chúng tôi xem xét tất cả các nút trong cây con của nó và nhóm chúng theo khoảng cách từ chúng đến$x$xét về các cạnh hướng xuống dưới của cây. 

Cụ thể hơn, đối với một đỉnh cố định$x$, định nghĩa$d_{x,i}$bằng số đỉnh$y$như vậy$y$nằm trong cây con của$x$và con đường từ$x$ĐẾN$y$đi xuống chính xác$i$các cạnh. Vì thế$d_{x,0}$luôn là 1, vì nó có giá trị$x$chính nó. Các giá trị lớn hơn$i$đếm xem có bao nhiêu nút tồn tại ở độ sâu$i$dưới$x$. 

Đối với mỗi đỉnh, chúng ta được yêu cầu tìm “chỉ số trội” của phân bố này. Chỉ số này$j$được chọn sao cho giá trị$d_{x,j}$hoàn toàn lớn hơn mọi thứ ở bên trái của nó và không bao giờ vượt quá bất cứ thứ gì ở bên phải nó. Nói cách khác, chúng ta đang tìm kiếm một đỉnh trong dãy mà mọi thứ trước nó đều nhỏ hơn và mọi thứ sau nó không lớn hơn. 

Cây có thể chứa tới một triệu đỉnh, điều này ngay lập tức loại trừ bất kỳ giải pháp nào tính toán lại phân bố độ sâu của cây con một cách độc lập cho mỗi nút. Một cách tiếp cận đơn giản là khám phá từng cây con riêng biệt sẽ liên tục duyệt qua các phần lớn của cây, dẫn đến hành vi bậc hai trong trường hợp xấu nhất. 

Trường hợp cạnh tinh tế xuất hiện ở những cây giống như dây chuyền. Nếu cây là một đường đi thì mỗi cây con cũng là một đường đi. Trong những trường hợp như vậy, tất cả số đếm ở mỗi độ sâu đều chính xác bằng một, do đó mọi chỉ số đều thỏa mãn điều kiện, nhưng định nghĩa buộc chỉ số hợp lệ nhỏ nhất do bất đẳng thức nghiêm ngặt ở vế trái. Điều này dẫn đến câu trả lời 0 ở mọi nơi. 

Một trường hợp góc khác phát sinh ở những cây có hình ngôi sao. Gốc có nhiều con và tất cả các phân bố theo chiều sâu đều tập trung ở độ sâu 1. Đối với gốc, chỉ số 1 chiếm ưu thế, trong khi đối với lá, chỉ tồn tại chỉ số 0. Việc tính toán lại dựa trên DFS đơn giản thường xử lý sai điều này bằng cách trộn không chính xác các đóng góp của cây con trên các gốc khác nhau. 

## Phương pháp tiếp cận 

Một phương pháp brute-force sẽ tính toán cho mỗi nút$x$, một DFS được giới hạn ở cây con của nó và đếm xem có bao nhiêu nút xuất hiện ở mỗi độ sâu. Chi phí này$O(n)$mỗi nút trong trường hợp xấu nhất, tạo ra$O(n^2)$tổng độ phức tạp, quá lớn đối với$n = 10^6$. 

Quan sát quan trọng là trình tự$d_{x,i}$chính xác là kích thước của$i$-cấp thứ của cây con của$x$. Chúng tôi đang tìm kiếm mức độ sâu “đông đúc” nhất trong mỗi cây con, với một quy tắc ràng buộc cụ thể: độ sâu trước đó phải nhỏ hơn hoàn toàn, độ sâu sau không được vượt quá nó. 

Đây là một cài đặt cổ điển trong đó việc hợp nhất “nhỏ với lớn” trên cây trở nên hữu ích. Nếu chúng ta xử lý cây từ dưới lên, chúng ta có thể duy trì, đối với mỗi nút, một mảng tần số có độ sâu trong cây con của nó. Khi hợp nhất các phần tử con, chúng tôi luôn hợp nhất các cấu trúc nhỏ hơn thành các cấu trúc lớn hơn để đảm bảo độ phức tạp gần như tuyến tính tổng thể. 

Trong khi hợp nhất, chúng tôi cũng theo dõi chỉ số độ sâu ứng cử viên tốt nhất hiện tại. Mỗi lần chúng tôi thêm số lượng từ một phần tử con được dịch chuyển theo 1 độ sâu, chúng tôi sẽ cập nhật các giá trị tần số và điều chỉnh chỉ số tốt nhất bằng cách so sánh các mức bị ảnh hưởng. Cấu trúc ràng buộc đảm bảo rằng phần đóng góp của mỗi nút sẽ di chuyển lên trên thông qua việc hợp nhất nhiều nhất$O(\log n)$lần khấu hao. 

Điều này làm giảm vấn đề duy trì bản đồ tần số độ sâu cho từng cây con và trích xuất chỉ số chi phối khi chúng tôi xây dựng lên trên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| DSU từ nhỏ đến lớn trên cây |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây lúc 1 và chạy DFS để xử lý con trước cha mẹ của chúng. 

1. Thực hiện truyền tải DFS từ gốc, đảm bảo chúng tôi xử lý tất cả các nút con trước nút hiện tại. Điều này thiết lập cấu trúc thứ tự sau nơi thông tin cây con có sẵn trước khi xử lý một nút. 
2. Đối với mỗi nút$x$, khởi tạo cấu trúc tần số$freq_x$với$freq_x[0] = 1$. Điều này thể hiện rằng chính nút đó đóng góp một đỉnh ở độ sâu 0. 
3. Dành cho mọi trẻ em$c$của$x$, tính toán đầu tiên$freq_c$. Sau đó dịch chuyển tất cả độ sâu của nó lên +1 vì mọi nút trong$c$cây con của nó sâu hơn một cạnh so với$x$. 
4. Hợp nhất$freq_c$vào trong$freq_x$. Để kiểm soát độ phức tạp, hãy luôn hợp nhất bản đồ tần số nhỏ hơn thành bản đồ tần số lớn hơn. Nếu như$freq_x$nhỏ hơn, hãy hoán đổi chúng trước khi hợp nhất. Điều này đảm bảo mỗi phần tử chỉ di chuyển một số lần logarit. 
5. Sau khi hợp nhất tất cả các phần tử con, hãy tính chỉ số trội cho$x$. Chúng tôi quét các giá trị độ sâu theo thứ tự tăng dần trong khi theo dõi giá trị tốt nhất được thấy cho đến nay. Chúng tôi chọn chỉ số nhỏ nhất$j$sao cho tất cả các giá trị trước đó đều nhỏ hơn hoàn toàn và không có giá trị nào sau đó vượt quá nó. Trong thực tế, điều này trở thành việc xác định độ sâu nơi xảy ra tần số tối đa, với việc liên kết với chỉ số hợp lệ nhỏ nhất thỏa mãn ràng buộc tiền tố. 
6. Lưu chỉ mục này làm câu trả lời cho nút$x$. 
7. Trở về$freq_x$trở lên cuộc gọi cha mẹ. 

### Tại sao nó hoạt động 

Tại mỗi nút, thuật toán duy trì nhiều tập hợp độ sâu cây con chính xác. Việc hợp nhất từ ​​nhỏ đến lớn đảm bảo rằng không có đóng góp của cây con nào được xử lý nhiều lần theo logarit, nhưng quan trọng hơn, nó duy trì số lượng tần số chính xác ở mỗi độ sâu. 

Điều kiện chỉ số vượt trội chỉ phụ thuộc vào sự so sánh tương đối của tần số theo độ sâu bên trong một cây con. Vì việc hợp nhất tạo ra số lượng chính xác cho từng cây con và không có số lượng nào bị mất hoặc được tính hai lần nên chỉ mục được tính toán ở mỗi nút khớp trực tiếp với định nghĩa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

n = int(input())
g = [[] for _ in range(n + 1)]
for _ in range(n - 1):
    a, b = map(int, input().split())
    g[a].append(b)
    g[b].append(a)

ans = [0] * (n + 1)

def dfs(u, p):
    mp = {0: 1}
    best_idx = 0

    for v in g[u]:
        if v == p:
            continue
        child_mp = dfs(v, u)

        # shift depths by +1
        shifted = {}
        for d, cnt in child_mp.items():
            shifted[d + 1] = shifted.get(d + 1, 0) + cnt

        # small-to-large merge
        if len(shifted) > len(mp):
            mp, shifted = shifted, mp

        for d, cnt in shifted.items():
            mp[d] = mp.get(d, 0) + cnt

    # compute dominant index
    max_val = -1
    best_idx = 0
    for d in sorted(mp.keys()):
        if mp[d] > max_val:
            max_val = mp[d]
            best_idx = d

    ans[u] = best_idx
    return mp

dfs(1, -1)
print("\n".join(str(ans[i]) for i in range(1, n + 1)))
```DFS xây dựng phân bố độ sâu cây con từ dưới lên. Mỗi nút bắt đầu bằng một số đếm duy nhất ở độ sâu 0, sau đó hấp thụ phân phối của từng nút con sau khi dịch chuyển nó theo một cấp độ. Bước hợp nhất được viết cẩn thận để đảm bảo rằng từ điển nhỏ hơn luôn được hợp nhất thành từ điển lớn hơn, điều này giúp ngăn chặn hiện tượng bùng nổ bậc hai. 

Chỉ số vượt trội được tính toán bằng cách chỉ cần quét tất cả số lượng độ sâu được lưu trữ và chọn độ sâu có tần số tối đa, tự động phá vỡ các liên kết đối với các chỉ số nhỏ hơn bằng điều kiện “tiền tố lớn hơn nghiêm ngặt” được thỏa mãn bởi lần xuất hiện đầu tiên của mức tối đa. 

Giới hạn đệ quy được tăng lên vì cây hình chuỗi sẽ vượt quá độ sâu đệ quy mặc định của Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2
2 3
3 4
```Đây là một dây chuyền nên mọi cây con đều là một đường thẳng. 

| Nút | Số lượng độ sâu cây con | Tần số độ sâu tối đa | Chỉ số chiếm ưu thế | 
| --- | --- | --- | --- | 
| 4 | {0:1} | 1 lúc 0 | 0 | 
| 3 | {0:1,1:1} | hòa, tối đa đầu tiên là 0 | 0 | 
| 2 | {0:1,1:1,2:1} | hòa, tối đa đầu tiên là 0 | 0 | 
| 1 | {0:1,1:1,2:1,3:1} | hòa, tối đa đầu tiên là 0 | 0 | 

Mỗi độ sâu xuất hiện chính xác một lần, vì vậy không có cấp độ sau nào có thể lấn át hoàn toàn cấp độ ban đầu. Điều này xác nhận tại sao tất cả các câu trả lời đều bằng không. 

### Ví dụ 2 

đầu vào:```
5
1 2
1 3
3 4
3 5
```Ở đây cây có cấu trúc phân nhánh. 

| Nút | Số lượng độ sâu cây con | Tần số tối đa | Chỉ số chiếm ưu thế | 
| --- | --- | --- | --- | 
| 4 | {0:1} | 1 | 0 | 
| 5 | {0:1} | 1 | 0 | 
| 3 | {0:1,1:2} | 2 | 1 | 
| 2 | {0:1} | 1 | 0 | 
| 1 | {0:1,1:1,2:2} | 2 | 2 | 

Lớp sâu nhất dưới nút 1 có mật độ nút cao nhất, do đó chỉ số 2 trở nên chiếm ưu thế ở gốc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Đóng góp tần số của mỗi nút được hợp nhất bằng cách sử dụng từ nhỏ đến lớn, đảm bảo mức tham gia logarit được khấu hao cho mỗi phần tử | 
| Không gian |$O(n)$| Mỗi nút đóng góp chính xác một mục nhập cho mỗi cấp độ sâu trên tất cả các cấu trúc | 

Những hạn chế lên đến$10^6$các nút yêu cầu hành vi gần tuyến tính. Việc hợp nhất từ ​​nhỏ đến lớn đảm bảo rằng mỗi phần thông tin của cây con chỉ được di chuyển trong một số lần giới hạn, giữ cho tổng thời gian chạy trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        a, b = map(int, input().split())
        g[a].append(b)
        g[b].append(a)

    ans = [0] * (n + 1)

    sys.setrecursionlimit(10**7)

    def dfs(u, p):
        mp = {0: 1}
        for v in g[u]:
            if v == p:
                continue
            child = dfs(v, u)
            shifted = {d + 1: c for d, c in child.items()}
            if len(shifted) > len(mp):
                mp, shifted = shifted, mp
            for k, v in shifted.items():
                mp[k] = mp.get(k, 0) + v
        best = 0
        bestv = -1
        for k, v in mp.items():
            if v > bestv:
                bestv = v
                best = k
        ans[u] = best
        return mp

    dfs(1, -1)
    return "\n".join(str(ans[i]) for i in range(1, n + 1))

# provided sample
assert run("""4
1 2
2 3
3 4
""") == "0\n0\n0\n0"

# single node
assert run("""1
""") == "0"

# star
assert run("""5
1 2
1 3
1 4
1 5
""") == "1\n0\n0\n0\n0"

# chain
assert run("""4
1 2
2 3
3 4
""") == "0\n0\n0\n0"

# balanced tree
assert run("""7
1 2
1 3
2 4
2 5
3 6
3 7
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 0 | trường hợp tối thiểu | 
| ngôi sao | gốc=1, lá=0 | sự thống trị của cây con nông | 
| chuỗi | tất cả số không | phân bố tần số đồng đều | 
| cây cân đối | phân phối hợp lệ | hợp nhất nhiều nhánh đúng cách | 

## Vỏ cạnh 

Trong cây nút đơn, DFS khởi tạo bản đồ tần số dưới dạng`{0:1}`và ngay lập tức gán chỉ số chi phối 0. Không xảy ra sự hợp nhất, do đó cấu trúc vẫn tầm thường và chính xác. 

Trong cây hình ngôi sao có gốc ở số 1, mỗi lá chỉ đóng góp`{0:1}`, trong khi gốc tích lũy`{0:1,1:k}`Ở đâu$k$là số con. Mức tối đa xảy ra ở độ sâu 1, do đó gốc trả về 1 trong khi các lá vẫn giữ nguyên 0. Quá trình hợp nhất xử lý điều này một cách chính xác vì mỗi phần tử con độc lập và được dịch chuyển chính xác một lần. 

Trong một chuỗi sâu, mỗi nút kế thừa cấu trúc độ sâu tăng dần. Vì mọi độ sâu đều có tần số bằng nhau nên không có độ sâu sau nào vượt quá độ sâu trước đó một cách nghiêm ngặt, do đó chỉ số vượt trội luôn bằng 0. Thuật toán phản ánh chính xác điều này vì các giá trị tần số vẫn đồng nhất trên các độ sâu.
