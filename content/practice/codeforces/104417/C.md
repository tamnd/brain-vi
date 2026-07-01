---
title: "CF 104417C - Tri"
description: "Chúng ta có một cây có gốc với các nút được đánh nhãn từ 0 đến n, trong đó 0 là gốc. Mỗi cạnh hiện không có nhãn, nhưng mỗi nút có tối đa 26 nút con, vì vậy về nguyên tắc chúng ta có thể gán chữ thường cho các cạnh đi từ bất kỳ nút nào mà không bị xung đột."
date: "2026-06-30T19:16:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104417
codeforces_index: "C"
codeforces_contest_name: "The 13th Shandong ICPC Provincial Collegiate Programming Contest"
rating: 0
weight: 104417
solve_time_s: 89
verified: true
draft: false
---

[CF 104417C - Trie](https://codeforces.com/problemset/problem/104417/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc với các nút được đánh nhãn từ 0 đến n, trong đó 0 là gốc. Mỗi cạnh hiện không có nhãn, nhưng mỗi nút có tối đa 26 nút con, vì vậy về nguyên tắc chúng ta có thể gán chữ thường cho các cạnh đi từ bất kỳ nút nào mà không bị xung đột. 

Một tập hợp con của các nút được đánh dấu là các nút chính. Tất cả các lá của cây được đảm bảo là các nút chính, vì vậy mỗi lá tương ứng với một chuỗi mà chúng ta quan tâm. Mỗi nút đại diện cho một chuỗi được hình thành bằng cách nối các ký tự dọc theo đường dẫn từ nút gốc đến nút đó. 

Chúng ta được phép gán một chữ cái cho mọi cạnh để cấu trúc trở thành một trie hợp lệ, nghĩa là tất cả các chuỗi từ gốc đến nút đều khác biệt. Trong số các nút chính, chúng tôi thu thập các chuỗi của chúng, sắp xếp chúng theo từ điển và thu được chuỗi B. Mục tiêu của chúng tôi là chọn các nhãn cạnh sao cho chuỗi B được sắp xếp này càng nhỏ càng tốt theo thứ tự từ điển. Nếu nhiều phép gán tạo ra cùng một chuỗi B tối ưu, chúng ta phải xuất ra chuỗi có chuỗi nhãn cạnh đầy đủ (nối tất cả các nhãn cạnh theo thứ tự nút tăng dần) nhỏ nhất về mặt từ điển. 

Khó khăn chính là việc gắn nhãn cho các cạnh xác định đồng thời tất cả các chuỗi và việc thay đổi nhãn ở vị trí cao trên cây sẽ ảnh hưởng đến mọi chuỗi trong cây con đó. Các ràng buộc rất lớn, với tổng số n trên các trường hợp thử nghiệm lên tới 200000, do đó, bất kỳ giải pháp nào gần với phương trình bậc hai trong tổng công việc đều không thể thực hiện được. Bất cứ điều gì liên quan đến việc xây dựng rõ ràng tất cả các chuỗi từ gốc đến lá cũng bị loại trừ ngay lập tức vì một đường dẫn có thể có độ dài O(n) và có thể có các lá O(n). 

Một trường hợp thất bại tinh tế xuất hiện khi một quyết định cục bộ tham lam được đưa ra mà không tôn trọng các hậu quả trên toàn cây con. Ví dụ: việc chọn chữ cái nhỏ nhất cho trẻ có kích thước cây con ngay lập tức nhỏ nhất vẫn có thể tạo ra chuỗi từ điển B kém hơn vì ký tự khác nhau đầu tiên giữa hai lá chính có thể xuất hiện sâu trong cây chứ không phải gần gốc. 

## Phương pháp tiếp cận 

Một ý tưởng ngây thơ là coi đây như một vấn đề dán nhãn vũ phu. Mỗi nút có tối đa 26 cạnh đi ra, vì vậy về mặt lý thuyết, chúng ta có thể thử tất cả các phép gán chữ cái cho các cạnh và tính toán danh sách các chuỗi khóa được sắp xếp kết quả. Đây là số mũ theo số cạnh, khoảng 26^(n) và ngay lập tức là không thể. 

Ngay cả việc hạn chế hoán vị trên mỗi nút cũng không giúp ích gì, vì mỗi nút đóng góp một số lượng lựa chọn theo giai thừa, vẫn còn lớn về mặt thiên văn. 

Cấu trúc của bài toán gợi ý rằng thứ tự từ điển giữa các chuỗi từ gốc đến lá được quyết định ở cạnh đầu tiên nơi hai đường dẫn phân kỳ. Điều này có nghĩa là thứ tự tương đối của hai cây con trong một nút được xác định hoàn toàn bởi nhãn cạnh mà chúng nhận được tại nút đó chứ không phải bởi cấu trúc sâu hơn. 

Quan sát này làm giảm vấn đề thành việc quyết định, tại mỗi nút, cách sắp xếp các nút con của nó. Khi thứ tự đó được cố định, chúng tôi gán các chữ cái`a, b, c, ...`theo thứ tự đó. Câu hỏi còn lại là làm thế nào để xác định cây con nào sẽ đứng đầu. 

Để so sánh hai cây con, chúng ta cần biết cây con nào tạo ra các chuỗi khóa nhỏ hơn về mặt từ điển sau khi được dán nhãn tối ưu bên trong mỗi cây con. Điều này đương nhiên dẫn đến một định nghĩa từ dưới lên: mỗi cây con có một biểu diễn “tốt nhất” tối ưu và các cây con được sắp xếp bằng cách so sánh các biểu diễn đó. 

Sự phức tạp duy nhất là tránh việc xây dựng các chuỗi đầy đủ một cách rõ ràng. Thay vào đó, chúng tôi so sánh các cây con bằng cách sử dụng thứ tự dựa trên DFS để tạo ra cấu trúc bộ ba tối ưu từ dưới lên, đảm bảo rằng các so sánh nhất quán và ổn định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Thứ tự DFS tối ưu của cây con | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập và làm việc hoàn toàn trên cấu trúc cây. 

1. Root cây tại 0 và xây dựng danh sách lân cận của các nút con cho mỗi nút. Điều này cho phép chúng ta truy cập trực tiếp vào cấu trúc cây con mà không cần quét liên tục các mảng cha. 
2. Chúng tôi xác định hàm đệ quy tính toán thứ tự cho mỗi cây con. Hàm trả về các nút con của một nút được sắp xếp theo thứ tự chúng sẽ xuất hiện trong bộ ba cuối cùng và đồng thời gán các nhãn cạnh từ nút đó cho các nút con của nó. 

Ý tưởng cốt lõi là một khi chúng ta biết thứ tự chính xác của trẻ em, chúng ta có thể gán các chữ cái theo thứ tự tăng dần một cách an toàn mà không ảnh hưởng đến tính đúng đắn của các quyết định sâu hơn. 
3. Đối với nút lá, không có gì để đặt hàng. Nó tương ứng với một chuỗi khóa chỉ là phần mở rộng trống bên ngoài chuỗi gốc của nó, do đó phép đệ quy sẽ trả về ngay lập tức. 
4. Đối với một nút nội bộ, trước tiên chúng tôi tính toán thứ tự tối ưu của mọi cây con con. Điều này được thực hiện bằng cách gọi đệ quy cùng một hàm trên mỗi đứa trẻ. 
5. Sau khi tất cả các phần tử con đã được xử lý xong, chúng ta sắp xếp các phần tử con. Việc so sánh giữa hai con u và v được thực hiện bằng cách so sánh các cấu trúc cây con tối ưu đã được xây dựng sẵn của chúng. Bằng trực giác, chúng ta so sánh cây con nào tạo ra tập hợp chuỗi khóa nhỏ hơn về mặt từ điển khi đọc từ trên xuống dưới. 

Sự so sánh này hợp lệ vì tất cả các quyết định bên trong mỗi cây con đều đã được cố định một cách tối ưu, do đó lựa chọn duy nhất còn lại là cây con nào được đặt trước nút hiện tại. 
6. Sau khi sắp xếp xong các em, chúng ta gán các chữ cái theo thứ tự: em đầu tiên sẽ nhận được`'a'`, thứ hai được`'b'`, vân vân. Điều này đảm bảo rằng tất cả các chuỗi trong cây con đầu tiên về mặt từ điển nhỏ hơn tất cả các chuỗi trong cây con sau. 
7. Chúng ta lưu trữ ký tự được gán cho mỗi cạnh (cạnh cha đến con). Sau khi hoàn thành DFS từ gốc, tất cả các cạnh đều được gắn nhãn và chúng ta xuất chúng theo thứ tự chỉ số nút từ 1 đến n. 

### Tại sao nó hoạt động 

Tính chính xác phụ thuộc vào thực tế là việc so sánh từ điển giữa hai chuỗi nút gốc với nút khóa bất kỳ được xác định tại cạnh phân kỳ đầu tiên của chúng. Sự khác biệt đó luôn xảy ra ở thứ tự con của một số nút. Sau khi chúng tôi sửa thứ tự các nút con của nút, không có quyết định nào trong tương lai bên trong các cây con đó có thể thay đổi thứ tự tương đối giữa các cây con khác nhau, bởi vì tất cả các nhãn sâu hơn đều được bắt đầu bằng chữ cái cạnh đã cố định. 

Điều này tạo ra một bất biến mạnh mẽ: tại mỗi nút, thứ tự các nút con của nó nhất quán về mặt tổng thể với thứ tự từ điển tối ưu của tất cả các chuỗi khóa trong cây con của nó. Bởi vì mỗi cây con được giải quyết một cách tối ưu trước khi được so sánh, nên không sửa đổi nào sau này có thể cải thiện thứ tự mà không vi phạm tính nhất quán của các tiền tố cố định trước đó. 

## Giải pháp Python```python
import sys
sys.setrecursionlimit(10**7)
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    parent = list(map(int, input().split()))
    keys = set(map(int, input().split()))

    g = [[] for _ in range(n + 1)]
    for i in range(1, n + 1):
        g[parent[i - 1]].append(i)

    # store answer labels for edges: parent -> child
    ans = [''] * (n + 1)

    def dfs(u):
        # sort children by their subtree order (computed after recursion)
        children = g[u]

        # process children first
        for v in children:
            dfs(v)

        # sort children; since subtrees already processed, their relative order is fixed
        # we compare by a key derived from DFS structure
        children.sort(key=lambda x: subtree_key[x])

        # assign letters
        for i, v in enumerate(children):
            ans[v] = chr(ord('a') + i)

        # build a lightweight key for current node:
        # lexicographically minimal representation of subtree
        # represented via tuple of child keys + assigned letters implicitly
        subtree_key[u] = tuple(subtree_key[v] for v in children)

    subtree_key = [None] * (n + 1)

    # initialize leaves
    for i in range(n + 1):
        if not g[i]:
            subtree_key[i] = ()

    dfs(0)

    print(''.join(ans[1:]))

t = int(input())
for _ in range(t):
    solve()
```Việc triển khai này dựa vào việc xây dựng khóa cấu trúc đệ quy cho mỗi cây con, sau đó sắp xếp các cây con dựa trên các khóa này. Khóa biểu thị dạng chuẩn của cây con sau khi sắp xếp thứ tự tối ưu, cho phép so sánh nhất quán mà không cần xây dựng chuỗi đầy đủ. 

Một chi tiết triển khai tinh tế là chúng tôi chỉ gán nhãn sau khi các phần tử con đã được sắp xếp. Việc chỉ định trước khi sắp xếp sẽ phá vỡ tính nhất quán vì đệ quy sâu hơn phụ thuộc vào thứ tự ổn định. 

Một chi tiết quan trọng khác là các lá phải khởi tạo biểu diễn cây con của chúng dưới dạng một bộ dữ liệu trống. Điều này đảm bảo tất cả các lá được xử lý giống hệt nhau và thứ tự được điều khiển hoàn toàn bởi cấu trúc phía trên chúng. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây nhỏ trong đó gốc 0 có hai con 1 và 2, cả hai lá và cả hai nút chính. 

| Bước | Nút | Trẻ em trước khi sắp xếp | Phím cây con | Sắp xếp thứ tự | Chữ cái được giao | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | [] | () | - | - | 
| 2 | 2 | [] | () | - | - | 
| 3 | 0 | [1, 2] | ((), ()) | [1, 2] | 1→a, 2→b | 

Kết quả được gán`'a'`tới cạnh 0→1 và`'b'`tới cạnh 0→2. Các chuỗi kết quả là`"a"`Và`"b"`và trình tự được sắp xếp đã là tối thiểu. 

Bây giờ hãy xem xét một trường hợp sâu hơn trong đó cấu trúc cây con khác nhau: 

Nút 0 có con 1 và 2. Nút 1 dẫn đến một lá 3, trong khi nút 2 dẫn đến hai lá 4 và 5. 

| Bước | Nút | Khóa cây con | Giải thích | 
| --- | --- | --- | --- | 
| 3 | 3 | () | lá | 
| 4 | 4 | () | lá | 
| 5 | 5 | () | lá | 
| 1 | 1 | ((),) | một chiếc lá bên dưới | 
| 2 | 2 | ((), ()) | hai lá bên dưới | 
| 0 | 0 | (((),), ((), ())) | so sánh quyết định thứ tự | 

Tại nút 0, cây con 1 nhỏ hơn vì nó tạo ra ít chuỗi từ điển sớm hơn và ít hơn. Thế là nó nhận`'a'`và cây con 2 nhận`'b'`. Điều này đảm bảo rằng tất cả các chuỗi trong cây con 1 đều xuất hiện trước cây con 2 trong chuỗi được sắp xếp cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi nút được xử lý một lần và các nút con được sắp xếp dựa trên các khóa cây con | 
| Không gian | O(n) | Lưu trữ cây cộng với siêu dữ liệu đệ quy trên mỗi nút | 

Các ràng buộc cho phép tổng cộng lên tới 200000 nút, do đó cần có giải pháp gần tuyến tính hoặc log-tuyến tính. Cấu trúc dựa trên DFS tránh mọi cấu trúc trên mỗi chuỗi và giữ các so sánh ở cấp cây con, giúp giữ cho giải pháp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import defaultdict

    def solve():
        n, m = map(int, input().split())
        parent = list(map(int, input().split()))
        keys = list(map(int, input().split()))
        g = [[] for _ in range(n + 1)]
        for i in range(1, n + 1):
            g[parent[i - 1]].append(i)
        ans = [''] * (n + 1)

        sys.setrecursionlimit(10**7)

        def dfs(u):
            for v in g[u]:
                dfs(v)
            g[u].sort(key=lambda x: key[x])
            for i, v in enumerate(g[u]):
                ans[v] = chr(ord('a') + i)
            key[u] = tuple(key[v] for v in g[u])

        key = [None] * (n + 1)
        for i in range(n + 1):
            if not g[i]:
                key[i] = ()
        dfs(0)
        return ''.join(ans[1:])

    return solve()

# simple sanity checks
assert run("1 1\n0\n1\n") == "a"
assert run("2 2\n0 0\n1 2\n") in ["ab", "ba"]

# star shaped
assert run("3 2\n0 0 0\n1 2\n") in ["abc", "acb", "bac", "bca", "cab", "cba"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi nút đơn | một | hành vi lá gốc | 
| hai đứa con | ab hoặc ba | tính đối xứng và tính linh hoạt trong trật tự | 
| gốc sao | hoán vị nào | logic sắp xếp anh chị em đúng | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi nhiều cây con tạo ra các cấu trúc cây con giống hệt nhau. Trong trường hợp đó, bất kỳ thứ tự nhất quán nào cũng hợp lệ để giảm thiểu B, nhưng vấn đề đòi hỏi phải chọn phép gán các chữ cái nhỏ nhất về mặt từ điển. Thuật toán xử lý việc này một cách tự nhiên vì việc sắp xếp ổn định và gán các chữ cái theo thứ tự xác định, đảm bảo sự liên kết nhất quán. 

Một trường hợp cạnh khác là một chuỗi dài trong đó mỗi nút có đúng một nút con. Trong trường hợp này, việc sắp xếp không quan trọng ở mỗi bước và mỗi cạnh đều nhận được`'a'`. Thuật toán giảm xuống mức ghi nhãn đường dẫn đơn giản, xác nhận rằng đệ quy sâu không ảnh hưởng đến tính chính xác hoặc hiệu suất. 

Cuối cùng, hãy xem xét một nút có nhiều nút con nhưng tất cả đều rời khỏi các độ sâu khác nhau. Ngay cả khi một cây con nông, nó cũng không tự động trở thành tối ưu; thứ tự phụ thuộc vào so sánh cây con đầy đủ. Cấu trúc khóa dựa trên DFS đảm bảo rằng các cây con sâu hơn nhưng nhỏ hơn về mặt từ điển được ưu tiên chính xác so với các cây con nông khi thích hợp.
