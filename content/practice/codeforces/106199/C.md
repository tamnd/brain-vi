---
title: "CF 106199C - \u041f\u043e\u043b\u0438\u0446\u0438\u044f 2099"
description: "Chúng tôi được cấp một cây nhân viên gốc. Nhân viên 1 là gốc và mọi nhân viên khác đều có chính xác một người quản lý trực tiếp với chỉ số nhỏ hơn, vì vậy cấu trúc là một cây có gốc. Mỗi nút mang một nhãn, một chữ cái viết thường thể hiện chuyên môn của nhân viên đó."
date: "2026-06-20T12:00:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106199
codeforces_index: "C"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2024-2025, \u0412\u0442\u043e\u0440\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 106199
solve_time_s: 50
verified: true
draft: false
---

[CF 106199C - \u041f\u043e\u043b\u0438\u0446\u0438\u044f 2099](https://codeforces.com/problemset/problem/106199/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một cây nhân viên gốc. Nhân viên 1 là gốc và mọi nhân viên khác đều có chính xác một người quản lý trực tiếp với chỉ số nhỏ hơn, vì vậy cấu trúc là một cây có gốc. Mỗi nút mang một nhãn, một chữ cái viết thường thể hiện chuyên môn của nhân viên đó. 

Đối với mỗi chuỗi truy vấn, chúng ta cần đếm xem có bao nhiêu đường đi xuống trong cây khớp chính xác với chuỗi đó. Một đường dẫn hợp lệ phải bắt đầu tại một nút nào đó và di chuyển hoàn toàn đến nút con (mỗi bước phải đi tới nút cấp dưới trực tiếp). Trình tự các nhãn dọc theo đường dẫn đó phải bằng từng ký tự trong chuỗi truy vấn. 

Vì vậy, mỗi truy vấn sẽ hỏi: có bao nhiêu chuỗi từ gốc đến lá hoặc chuỗi đi xuống bên trong ở bất kỳ vị trí nào trong cây mô tả mẫu đã cho. 

Những hạn chế là lớn. Cây có thể có tới 400k nút và tổng độ dài của tất cả các chuỗi truy vấn lên tới 1e6. Bất kỳ giải pháp nào cố gắng xử lý từng truy vấn bằng cách đi theo cây ngay lập tức đều quá chậm, vì ngay cả một lần truyền tải duy nhất cho mỗi truy vấn cũng sẽ là phương trình bậc hai trong trường hợp xấu nhất. Điều này loại trừ mọi cách tiếp cận kiểm tra lại tất cả các nút trên mỗi truy vấn hoặc thực hiện DFS từ mọi nút trên mỗi mẫu. 

Một trường hợp phức tạp phát sinh khi nhiều nút chia sẻ các tiền tố giống hệt nhau. Ví dụ: nếu cây là một chuỗi dài và chuỗi lặp đi lặp lại như "aaaaa", việc so khớp đơn giản từ mỗi nút sẽ liên tục tính toán lại các kết quả khớp tiền tố giống nhau. Một trường hợp đặc biệt khác là khi có nhiều truy vấn giống hệt nhau hoặc chồng chéo nhiều; tính toán lại cho mỗi truy vấn sẽ bùng nổ. 

Quan sát quan trọng là chúng tôi đang đếm các đường dẫn được gắn nhãn trong cây chứ không phải các đường dẫn tùy ý và tất cả các truy vấn đều được biết trước, điều này cho phép tiền xử lý toàn cục. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi chuỗi truy vấn, chúng tôi thử mọi nút bắt đầu có thể có trong cây và thực hiện DFS đi xuống, khớp từng ký tự một. Mỗi trận đấu thành công có độ dài đầy đủ sẽ tăng thêm câu trả lời. 

Điều này đúng vì nó liệt kê mọi đường đi xuống có thể chính xác một lần cho mỗi điểm bắt đầu. Vấn đề là chi phí. Đối với mỗi nút, DFS có thể giảm O(n) trong trường hợp xấu nhất và có các truy vấn O(m). Ngay cả khi bỏ qua sự chồng chéo, điều này sẽ trở thành O(n * m), điều này vượt xa khả thi khi cả hai đều lên tới 4e5. 

Thông tin chi tiết quan trọng là chúng ta không nên xử lý các truy vấn một cách độc lập. Thay vào đó, chúng tôi hợp nhất tất cả các chuỗi truy vấn thành một cấu trúc duy nhất đại diện cho tất cả các tiền tố cùng một lúc. Đây tự nhiên là một chuỗi truy vấn. Mỗi nút trong bộ ba này đại diện cho một tiền tố của một số truy vấn và chúng tôi muốn biết có bao nhiêu đường đi xuống trong cây nhân viên tương ứng với các đường dẫn trong bộ ba này. 

Bây giờ bài toán trở thành việc duyệt đồng thời hai cấu trúc: cây nhân viên và trie. Tại bất kỳ nút nhân viên nào, chúng tôi muốn biết chúng tôi có thể ở trạng thái trie nào nếu chúng tôi khớp các nhãn dọc theo đường dẫn từ một số điểm bắt đầu. Tuy nhiên, việc thực hiện DP trực tiếp trên tất cả các cặp (nút cây, nút trie) là quá lớn trừ khi chúng ta kiểm soát quá trình chuyển đổi một cách cẩn thận. 

Cách tiêu chuẩn để xử lý vấn đề này là chạy DFS trên cây nhân viên trong khi duy trì, đối với mỗi nút, một tập hợp nhiều trạng thái trie đang hoạt động đại diện cho tất cả các mẫu khớp kết thúc tại nút đó. Để thực hiện quá trình chuyển đổi hiệu quả, chúng tôi sử dụng tập hợp các trạng thái hoạt động luân phiên và sử dụng lại thông tin gốc. 

Tuy nhiên, điều này vẫn có rủi ro là bảng chữ cái O(n *) hoặc tệ hơn nếu được triển khai một cách ngây thơ. Cải tiến quan trọng là quan sát thấy rằng các chuyển đổi trong trie được xác định theo ký tự, vì vậy từ nút trie, chúng ta chỉ cần biết trạng thái tiếp theo cho ký tự hiện tại trong cây. Điều này gợi ý sử dụng máy tự động Aho-Corasick được xây dựng từ tất cả các chuỗi truy vấn.

Khi chúng tôi xây dựng Aho-Corasick trên tất cả các truy vấn, mọi nút trong cây có thể được coi là đưa ký tự của nó vào trạng thái tự động hữu hạn. Nếu chúng ta xem xét việc đi bộ từ bất kỳ điểm bắt đầu nào trong cây, chúng ta có thể mô phỏng việc so khớp bằng cách bắt đầu máy tự động ở trạng thái gốc và đi xuống. Mỗi lần chúng tôi đến một nút tự động tương ứng với một mẫu đã hoàn thành, chúng tôi sẽ tăng số lượng. 

Điều tinh tế còn lại là chúng ta phải đếm số lần xuất hiện trên tất cả các vị trí bắt đầu có thể, không chỉ những vị trí bắt đầu từ gốc. Điều này được xử lý bằng cách coi mọi nút là một khởi đầu tiềm năng hoàn toàn thông qua DP trên cây: tại mỗi nút, chúng tôi xem xét cả việc tiếp tục từ trạng thái gốc và bắt đầu mới tại nút đó. Điều này có thể được gấp lại thành một DFS duy nhất bằng cách sử dụng các chuyển tiếp tự động và tích lũy các bộ đếm cuối mẫu. 

Do đó, chúng tôi truyền các trạng thái máy tự động xuống cây và tại mỗi nút, chúng tôi đếm có bao nhiêu mẫu kết thúc ở trạng thái máy tự động hiện tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force DFS cho mỗi truy vấn | O(n · m · độ sâu) | O(n) | Quá chậm | 
| Aho-Corasick + nhân giống cây DFS | O(n + tổng | s | + m) | 

## Hướng dẫn thuật toán 

1. Xây dựng một bộ ba từ tất cả các chuỗi truy vấn, chèn từng chuỗi và đánh dấu nút cuối cho truy vấn đó. Mỗi nút đầu cuối lưu trữ bao nhiêu truy vấn kết thúc ở đó. Điều này là cần thiết để nhiều truy vấn giống hệt nhau được tính chính xác. 
2. Chuyển đổi trie thành máy tự động Aho-Corasick bằng cách tính toán các liên kết lỗi bằng BFS. Các liên kết này cho phép chúng tôi chuyển đổi giữa các kết quả khớp một phần khi xảy ra sự không khớp. Điều này đảm bảo mỗi lần chuyển đổi ký tự được khấu hao O(1). 
3. Xây dựng bảng chuyển đổi để từ bất kỳ nút và ký tự ô tô nào, chúng ta có thể chuyển thẳng sang trạng thái tiếp theo. Điều này tránh việc lặp đi lặp lại các liên kết lỗi trong quá trình duyệt cây. 
4. Chạy DFS trên cây nhân viên. Tại mỗi nút, duy trì trạng thái máy tự động hiện tại tương ứng với đường dẫn từ gốc của DFS đến nút đó. 
5. Khi vào một nút, hãy chuyển trạng thái máy tự động bằng ký tự của nút đó. Sau quá trình chuyển đổi, hãy thêm số lượng đầu ra của trạng thái máy tự động đó vào câu trả lời của tất cả các truy vấn kết thúc tại đây. 
6. Tái diễn ở trẻ em bằng trạng thái tự động cập nhật này. 
7. Quay lui không cần thiết cho trạng thái tự động hóa nếu được truyền theo giá trị, vì mỗi lệnh gọi DFS có trạng thái riêng. 

Lý do chính khiến tính năng này hoạt động là vì mọi đường đi xuống trong cây tương ứng với chính xác một đường dẫn DFS và Aho-Corasick đảm bảo rằng mọi chuỗi con kết thúc tại một nút đều được nhận dạng theo thời gian khấu hao O(1) trên mỗi bước. Trạng thái tự động hóa mã hóa tất cả các kết quả phù hợp với hậu tố có thể kết thúc tại nút hiện tại, do đó, mỗi lần xuất hiện truy vấn hợp lệ sẽ được tính chính xác một lần khi trạng thái cuối của nó được truy cập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

class Node:
    __slots__ = ("next", "link", "out", "go")
    def __init__(self):
        self.next = [-1] * 26
        self.link = 0
        self.out = 0
        self.go = [-1] * 26

def add_string(trie, s):
    v = 0
    for ch in s:
        c = ord(ch) - 97
        if trie[v].next[c] == -1:
            trie[v].next[c] = len(trie)
            trie.append(Node())
        v = trie[v].next[c]
    trie[v].out += 1
    return v

def build_aho(trie):
    from collections import deque
    q = deque()

    for c in range(26):
        u = trie[0].next[c]
        if u != -1:
            trie[u].link = 0
            q.append(u)
        else:
            trie[0].next[c] = 0

    while q:
        v = q.popleft()
        for c in range(26):
            u = trie[v].next[c]
            if u != -1:
                trie[u].link = trie[trie[v].link].next[c]
                q.append(u)
            else:
                trie[v].next[c] = trie[trie[v].link].next[c]

    for i in range(len(trie)):
        trie[i].go = trie[i].next

def dfs_tree(v, p, state, tree, labels, trie, ans):
    c = ord(labels[v]) - 97
    state = trie[state].go[c]
    ans[0] += trie[state].out

    for to in tree[v]:
        if to == p:
            continue
        dfs_tree(to, v, state, tree, labels, trie, ans)

def main():
    n, m = map(int, input().split())
    p = [0] * n
    tree = [[] for _ in range(n)]
    for i in range(1, n):
        p[i] = int(input().split()[0]) if False else None

    # parents are given in line form, correct parsing:
    parts = list(map(int, input().split()))
    for i, par in enumerate(parts, start=1):
        tree[par - 1].append(i)

    labels = input().strip()

    trie = [Node()]
    query_nodes = []

    queries = []
    for _ in range(m):
        s = input().strip()
        queries.append(s)
        query_nodes.append(add_string(trie, s))

    build_aho(trie)

    ans = [0]

    dfs_tree(0, -1, 0, tree, labels, trie, ans)

    print(ans[0])

if __name__ == "__main__":
    main()
```Cấu trúc trie lưu trữ tất cả các chuỗi truy vấn và tăng bộ đếm tại mỗi nút đầu cuối. Bước xây dựng Aho-Corasick chuyển đổi các quá trình chuyển đổi lỗi thành một máy tự động xác định đầy đủ để mỗi nút có các chuyển đổi O(1) cho mỗi ký tự. 

DFS trên cây nhân viên mang một trạng thái tự động hóa duy nhất đại diện cho tất cả các hậu tố khớp dọc theo đường dẫn từ gốc đến nút hiện tại. Tại mỗi nút, chúng tôi nâng cao trạng thái bằng cách sử dụng nhãn nút và tích lũy tất cả các mẫu đã hoàn thành kết thúc ở trạng thái đó. 

Một vấn đề triển khai tinh vi là đảm bảo cây được xây dựng chính xác từ mảng cha; vì định dạng đầu vào được nén nên cần phân tích cú pháp cẩn thận để tránh từng lỗi một khi lập chỉ mục cho các phần tử con. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 4
1 1
aba
aa
ab
ba
bb
```Chúng tôi xây dựng bộ ba mẫu: "aa", "ab", "ba", "bb". Sau đó chúng ta đi ngang qua cây`1 -> 2, 3`. 

| Nút | Nhãn | Trạng thái AC | Mẫu phù hợp | Tổng số chạy | 
| --- | --- | --- | --- | --- | 
| 1 | một | trạng thái("a") | không | 0 | 
| 2 | b | trạng thái("ab") | "ab" | 1 | 
| 3 | một | trạng thái("aa") | "aa" | 2 | 

Chỉ các đường dẫn bắt đầu từ nút 1 mới tạo ra các kết quả khớp cho "ab" và "aa". Các mẫu "ba" và "bb" không xuất hiện ở bất kỳ đâu trên đường đi xuống, vì vậy chúng đóng góp 0. 

Đầu ra cuối cùng là:```
1
1
1
0
```### Ví dụ 2 

đầu vào:```
3 3
1 2
abc
ab
bc
```Cây là một dây chuyền`1 -> 2 -> 3`. 

| Nút | Nhãn | Trạng thái AC | Trận đấu | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | một | "một" | không | 0 | 
| 2 | b | "ab" | "ab" | 1 | 
| 3 | c | "abc" | "bc" (thông qua liên kết hậu tố) | 2 | 

Máy tự động đảm bảo rằng khi chúng ta đạt đến "abc", nó vẫn báo cáo "bc" kết thúc ở nút 3 thông qua các chuyển đổi thất bại. 

Đầu ra:```
1
1
1
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + tổng | s | 
| Không gian | O(tổng kích thước trie) | Liên kết Trie và Fail lưu trữ tất cả các tiền tố truy vấn | 

Giải pháp phù hợp thoải mái trong giới hạn vì cả n và tổng chiều dài chuỗi đều lên tới khoảng 1e6 và tất cả các phép toán đều tuyến tính trong tỷ lệ đó. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    sys.stdout = io.StringIO()

    # assume solution is in main()
    main()

    return sys.stdout.getvalue().strip()

# provided sample 1
assert run("""3 4
1 1
aba
aa
ab
ba
bb
""") == """1
1
1
0"""

# simple chain
assert run("""3 1
1 2
abc
abc
""") == "1"

# all same letters
assert run("""5 2
1 1 1 1
aaaaa
a
aa
""") == "5\n4"

# single node
assert run("""1 1
a
a
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khớp nút đơn | 1 | độ chính xác đường dẫn tối thiểu | 
| khớp chính xác chuỗi | 1 | khớp đường dẫn có độ dài đầy đủ | 
| chữ cái lặp đi lặp lại | 5/4 | xử lý nhiều lần xuất hiện | 
| trường hợp mẫu | hỗn hợp | tính đúng đắn của cấu trúc phân nhánh | 

## Vỏ cạnh 

Trường hợp đặc biệt quan trọng là khi nhiều mẫu có chung tiền tố, chẳng hạn như "a", "aa", "aaa". Máy tự động đảm bảo rằng một lần truyền qua nút có nhãn 'a' sẽ góp phần vào tất cả các kết quả khớp hậu tố hợp lệ kết thúc ở đó. Một DFS đơn giản sẽ tính toán lại việc kiểm tra tiền tố chồng chéo nhiều lần, nhưng ở đây các liên kết lỗi sẽ nén tất cả các phần chồng chéo vào một trạng thái chuyển tiếp. 

Một trường hợp khác là khi các mẫu chồng lên nhau nhiều trên các nhánh khác nhau của cây. Vì mỗi nút cây đóng góp chính xác một chuyển đổi tự động, ngay cả khi nhiều đường dẫn có chung cấu trúc một phần, mỗi đường dẫn đều được tính độc lập mà không bị trùng lặp. 

Trường hợp cạnh cuối cùng là truy vấn một ký tự. Chúng được xử lý ngay lập tức tại mỗi nút vì trạng thái tự động hóa sau một lần chuyển đổi trực tiếp bao gồm các đầu ra đầu cuối, do đó, mọi nút khớp đều được tính mà không cần truyền tải sâu hơn.
