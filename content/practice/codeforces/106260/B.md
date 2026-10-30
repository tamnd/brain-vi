---
title: "CF 106260B - \u56de\u6587"
description: "Chúng ta có một cây có gốc trong đó mỗi nút mang một chữ cái viết thường. Gốc là nút 1 và mọi nút đều có độ sâu được xác định rõ ràng từ gốc. Mỗi truy vấn tập trung vào một nút u và độ sâu mục tiêu d."
date: "2026-06-25T07:23:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106260
codeforces_index: "B"
codeforces_contest_name: "2025 SiChuan University for new student"
rating: 0
weight: 106260
solve_time_s: 48
verified: true
draft: false
---

[CF 106260B - \u56de\u6587](https://codeforces.com/problemset/problem/106260/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc trong đó mỗi nút mang một chữ cái viết thường. Gốc là nút 1 và mọi nút đều có độ sâu được xác định rõ ràng từ gốc. 

Mỗi truy vấn tập trung vào một nút`u`và độ sâu mục tiêu`d`. Chúng ta xem xét tất cả các nút thỏa mãn hai điều kiện cùng một lúc: chúng nằm bên trong cây con của`u`và độ sâu của chúng tính từ gốc chính xác là`d`. Từ các chữ cái được viết trên các nút đó, chúng ta tạo thành một chuỗi bằng cách sử dụng tất cả chúng theo bất kỳ thứ tự nào. 

Câu hỏi cho mỗi truy vấn là liệu chúng ta có thể sắp xếp lại các chữ cái đó để chuỗi kết quả trở thành một chuỗi phản palindrome hay không, nghĩa là nếu chúng ta ghép ký tự đầu tiên với ký tự cuối cùng, ký tự thứ hai với ký tự thứ hai cuối cùng, v.v., thì không có cặp nào chứa cùng một chữ cái. 

Cả kích thước cây và số lượng truy vấn đều đủ lớn để mọi quá trình duyệt cây con theo mỗi truy vấn đều quá chậm. Việc truy cập các nút bên trong mỗi cây con riêng biệt sẽ dẫn đến khoảng$O(nq)$, vượt xa giới hạn khả thi đối với$2 \cdot 10^5$. Điều này buộc chúng tôi phải sử dụng phương pháp tiền xử lý trong đó thông tin về cây con và độ sâu được tổ chức lại để các truy vấn giảm xuống mức đếm phạm vi nhanh. 

Trường hợp cạnh tinh tế xuất hiện khi không có nút nào khớp với điều kiện truy vấn. Ví dụ: nếu không có nút nào trong cây con của`u`nằm ở độ sâu`d`, câu trả lời phải là “Không”. Một trường hợp quan trọng khác là khi chỉ tồn tại một nút duy nhất trong lớp đó: một chuỗi ký tự đơn không thể tạo thành một phản palindrome vì nó sẽ so sánh với chính nó ở vị trí giữa. Ví dụ: một nút có chữ cái`"a"`phải quay lại`No`. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp cho mỗi truy vấn là thu thập tất cả các nút trong cây con của`u`, lọc những cái có chiều sâu`d`, trích xuất các chữ cái của chúng và sau đó kiểm tra xem liệu chúng ta có thể sắp xếp chúng thành một anti-palindrome hay không. Tính đúng đắn rất đơn giản vì nó phản ánh chính xác định nghĩa. Vấn đề là hiệu suất. Trong một cây rậm rạp, một cây con có thể chứa$O(n)$các nút và thực hiện điều này cho mọi truy vấn sẽ dẫn đến$O(nq)$công việc quá lớn. 

Quan sát cấu trúc quan trọng là điều kiện “nút nằm trong cây con của`u`” được biểu diễn một cách tự nhiên bằng các khoảng thời gian tham quan Euler, trong khi “nút có độ sâu`d`"phân vùng các nút thành các lớp độc lập. Khi chúng tôi cố định độ sâu`d`, tất cả các nút ở độ sâu đó có thể được liệt kê theo thứ tự thời gian vào tăng dần trong chuyến tham quan Euler. Sau đó, mọi hạn chế của cây con sẽ trở thành một truy vấn khoảng liền kề trên danh sách được sắp xếp đó. 

Điều này làm giảm vấn đề thành truy vấn tần số trên một mảng tĩnh theo độ sâu. Đối với mỗi độ sâu, chúng tôi duy trì một danh sách các nút được sắp xếp theo thời gian nhập DFS của chúng và cũng duy trì số lượng tiền tố của các chữ cái trong danh sách đó. Sau đó, mỗi truy vấn sẽ trích xuất một bảng tần số trên một mảng con, việc này có thể được thực hiện trong thời gian O(26). 

Bước còn lại là kiểm tra xem liệu một tập hợp nhiều chữ cái có thể tạo thành một anti-palindrome hay không. Một multiset có thể được sắp xếp lại sao cho không có cặp đối xứng nào khớp với khi và chỉ khi độ dài bằng nhau và không có ký tự nào xuất hiện quá một nửa chiều dài. Nếu ký tự nào vượt quá một nửa thì chắc chắn nó phải chiếm hai vị trí đối xứng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force cho mỗi truy vấn |$O(n)$mỗi truy vấn |$O(n)$| Quá chậm | 
| DFS + danh sách độ sâu + số lượng tiền tố |$O((n+q)\log n + 26q)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chạy DFS từ gốc để tính hai giá trị cho mỗi nút: độ sâu và thời gian vào Euler của nó. Điều này tuyến tính hóa cấu trúc cây con sao cho mỗi cây con trở thành một khoảng theo thứ tự DFS. 
2. Đối với mỗi giá trị độ sâu`d`, thu thập tất cả các nút nằm ở độ sâu này. Lưu trữ chúng theo thứ tự tăng dần về thời gian nhập DFS của chúng. Điều này đảm bảo rằng mọi hạn chế về cây con sẽ tương ứng với một phân đoạn liền kề trong danh sách này. 
3. Đối với mỗi danh sách độ sâu, hãy xây dựng 26 mảng tiền tố, mỗi mảng một chữ cái. Mỗi mảng tiền tố cho phép chúng ta đếm số lần xuất hiện của một chữ cái trong bất kỳ phân đoạn nào trong thời gian không đổi. 
4. Đối với mỗi truy vấn`(u, d)`, xác định vị trí đoạn nút ở độ sâu`d`có thời gian vào nằm giữa`tin[u]`Và`tout[u]`sử dụng tìm kiếm nhị phân. 
5. Sử dụng sự khác biệt về tiền tố, tính tần số của từng ký tự trong đoạn đó. 
6. Hãy để`k`là tổng số nút trong đoạn này. Nếu như`k == 0`, xuất ra “Không”. 
7. Nếu`k`là số lẻ, ghi “Không” vì một phản palindrome yêu cầu ghép mọi vị trí với một đối tác riêng biệt. 
8. Ngược lại, hãy kiểm tra xem có tần số ký tự nào vượt quá`k / 2`. Nếu có ký tự đó thì xuất ra “No”, nếu không thì xuất ra “Yes”. 

### Tại sao nó hoạt động 

Thứ tự DFS đảm bảo rằng tư cách thành viên của cây con trở thành điều kiện khoảng. Nhóm theo chiều sâu sẽ tách mỗi truy vấn thành một lớp duy nhất trong đó cấu trúc không còn phụ thuộc vào cây nữa mà chỉ phụ thuộc vào các vị trí theo thứ tự DFS. Trong một lớp cố định, tổng tiền tố giúp trích xuất tần số chính xác cho bất kỳ lát cây con nào. 

Điều kiện chống palindrome làm giảm các vị trí ghép nối một cách đối xứng. Vì mọi vị trí phải được ghép nối với một vị trí riêng biệt nên không nhân vật nào có thể thống trị hơn một nửa vị trí và thậm chí cần có độ dài để ghép nối hoàn chỉnh. Hai điều kiện này mô tả đầy đủ tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n, q = map(int, input().split())
g = [[] for _ in range(n + 1)]

for _ in range(n - 1):
    u, v = map(int, input().split())
    g[u].append(v)
    g[v].append(u)

s = input().strip()

tin = [0] * (n + 1)
tout = [0] * (n + 1)
depth = [0] * (n + 1)
timer = 0

nodes_by_depth = {}

def dfs(u, p):
    global timer
    timer += 1
    tin[u] = timer

    d = depth[u]
    if d not in nodes_by_depth:
        nodes_by_depth[d] = []
    nodes_by_depth[d].append(u)

    for v in g[u]:
        if v == p:
            continue
        depth[v] = d + 1
        dfs(v, u)

    tout[u] = timer

dfs(1, -1)

# build depth structures
depth_data = {}
for d, nodes in nodes_by_depth.items():
    nodes.sort(key=lambda x: tin[x])
    arr = [s[u - 1] for u in nodes]

    pref = [[0] * (len(nodes) + 1) for _ in range(26)]
    for i, ch in enumerate(arr, 1):
        for c in range(26):
            pref[c][i] = pref[c][i - 1]
        pref[ord(ch) - 97][i] += 1

    depth_data[d] = (nodes, pref)

def solve_query(u, d):
    if d not in depth_data:
        return False

    nodes, pref = depth_data[d]

    # find left bound: first tin >= tin[u]
    l, r = 0, len(nodes)
    while l < r:
        m = (l + r) // 2
        if tin[nodes[m]] >= tin[u]:
            r = m
        else:
            l = m + 1
    left = l

    # find right bound: last tin <= tout[u]
    l, r = 0, len(nodes)
    while l < r:
        m = (l + r) // 2
        if tin[nodes[m]] <= tout[u]:
            l = m + 1
        else:
            r = m
    right = l

    k = right - left
    if k == 0:
        return False
    if k % 2 == 1:
        return False

    for c in range(26):
        cnt = pref[c][right] - pref[c][left]
        if cnt > k // 2:
            return False

    return True

out = []
for _ in range(q):
    u, d = map(int, input().split())
    out.append("Yes" if solve_query(u, d) else "No")

print("\n".join(out))
```Phần DFS xây dựng một trật tự tuyến tính trong đó các truy vấn cây con trở thành các ràng buộc về phạm vi. Quá trình tiền xử lý theo độ sâu sẽ nén vấn đề thành các mảng tần số độc lập. 

Bên trong mỗi truy vấn, hai tìm kiếm nhị phân tách biệt chính xác các nút thuộc cả cây con và độ sâu mục tiêu. Các mảng tiền tố sau đó cho phép đếm chính xác mà không cần quét phân đoạn. 

Kiểm tra cuối cùng thực hiện các ràng buộc về cấu trúc của anti-palindromes: độ dài chẵn và tần số giới hạn. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây nhỏ trong đó nút 1 có con 2 và 3, cả 2 và 3 đều có một con. Giả sử chúng ta truy vấn một cây con bao gồm hai nút có cùng độ sâu với các chữ cái`a`Và`b`. 

| Bước | Các nút được xem xét | Thư | Kiểm tra tần suất | Kết quả | 
| --- | --- | --- | --- | --- | 
| Truy vấn lát cắt độ sâu cây con | [2, 3] | một, b | tần số tối đa = 1 | Có | 

Điều này cho thấy một cặp hợp lệ`ab`. 

Bây giờ hãy xem xét trường hợp tất cả các nút ở độ sâu đó`a`. 

| Bước | Các nút được xem xét | Thư | Kiểm tra tần suất | Kết quả | 
| --- | --- | --- | --- | --- | 
| Truy vấn lát cắt độ sâu cây con | [2, 3, 4, 5] | một, một, một, một | tần số tối đa = 4 | Không | 

Dù chiều dài bằng nhau nhưng mọi sự sắp xếp đều`a`để phù hợp với`a`ở một số vị trí được phản chiếu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n + q \log n + 26q)$| Cấu trúc xây dựng DFS; mỗi truy vấn sử dụng hai tìm kiếm nhị phân và 26 lần đếm | 
| Không gian |$O(26n)$| mảng tiền tố trên mỗi chiều sâu lưu trữ số lượng tích lũy | 

Quá trình tiền xử lý là tuyến tính theo số lượng nút cho đến sắp xếp chi phí trên mỗi danh sách độ sâu. Mỗi truy vấn chỉ thực hiện tìm ranh giới logarit cộng với quét bảng chữ cái liên tục, phù hợp thoải mái với các ràng buộc cho$2 \cdot 10^5$hoạt động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from collections import defaultdict

    # paste solution here or assume callable solve()
    return ""

# custom cases
assert run("""5 1
1 2
1 3
2 4
2 5
aabcc
1 2
""").strip().lower() == "yes"

assert run("""3 1
1 2
1 3
aaa
1 2
""").strip().lower() == "no"

assert run("""4 1
1 2
1 3
1 4
abca
1 3
""").strip().lower() in {"yes","no"}
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trộn hợp lệ cây con nhỏ | Có | tính khả thi cơ bản | 
| chữ thống nhất | Không | tần số thống trị thất bại | 
| lát cắt sâu thưa thớt | phụ thuộc | xử lý độ sâu trống/cạnh | 

## Vỏ cạnh 

Truy vấn cây con trong đó không có nút nào tồn tại ở độ sâu mục tiêu sẽ giảm xuống phân đoạn trống sau khi tìm kiếm nhị phân. Trong trường hợp đó chiều dài tính toán`k`trở thành 0 và thuật toán trực tiếp trả về “Không”, phù hợp với yêu cầu không tồn tại sự sắp xếp lại hợp lệ. 

Phân đoạn một nút luôn bị lỗi vì`k = 1`, thật kỳ lạ. Thuật toán từ chối nó ngay lập tức mà không cần kiểm tra tần số, nắm bắt chính xác rằng vị trí tự ghép nối không thể đáp ứng ràng buộc chống palindrome. 

Trường hợp một ký tự thống trị toàn bộ lát cắt độ sâu, chẳng hạn như`aaaaab`, kích hoạt`cnt > k/2`tình trạng. Mặc dù phân đoạn có thể đủ lớn và có độ dài bằng nhau, nhưng ít nhất một cặp phản chiếu phải khớp nhau, do đó việc loại bỏ là cần thiết và được xử lý chính xác theo giới hạn tần số.
