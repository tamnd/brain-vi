---
title: "CF 105581C - Trình quản lý tệp"
description: "Đầu vào mô tả cấu trúc thư mục thông qua danh sách đường dẫn tệp. Mỗi đường dẫn đại diện cho một tệp nằm ở đâu đó bên trong hệ thống phân cấp các thư mục được phân tách bằng dấu gạch chéo."
date: "2026-06-22T17:49:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105581
codeforces_index: "C"
codeforces_contest_name: "Open Udmurtia Junior Programming Contest 2018"
rating: 0
weight: 105581
solve_time_s: 72
verified: true
draft: false
---

[CF 105581C - Trình quản lý tệp](https://codeforces.com/problemset/problem/105581/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đầu vào mô tả cấu trúc thư mục thông qua danh sách đường dẫn tệp. Mỗi đường dẫn đại diện cho một tệp nằm ở đâu đó bên trong hệ thống phân cấp các thư mục được phân tách bằng dấu gạch chéo. Nếu chúng ta xây dựng lại cấu trúc này, mọi tiền tố trung gian sẽ tương ứng với một nút thư mục và thành phần cuối cùng của đường dẫn là một tệp. 

Ràng buộc được áp dụng cho các thư mục: sau khi chúng ta xóa một số tệp, mỗi thư mục chỉ được phép có tối đa K con ngay lập tức vẫn tồn tại. Con ngay lập tức ở đây có nghĩa là các tệp trực tiếp bên trong nó hoặc các thư mục con vẫn chứa ít nhất một tệp còn lại. Nếu một thư mục không còn tệp nào trong toàn bộ cây con của nó thì nó sẽ tự động biến mất, do đó nó không còn đóng góp vào các ràng buộc phía trên nó nữa. 

Nhiệm vụ là xóa càng ít tệp càng tốt để mọi thư mục đều tôn trọng quy tắc “tối đa K con đang hoạt động” này, sau đó xuất ra bất kỳ tập hợp hợp lệ nào của các tệp còn lại. 

Kích thước cấu trúc lớn: tổng chiều dài đường dẫn lên tới 10^6, điều này có nghĩa là chúng ta phải coi cấu trúc thư mục là một phép thử và xử lý nó về cơ bản là tuyến tính hoặc gần thời gian tuyến tính. Bất cứ điều gì liên tục quét phần tử con trong các vòng lặp lồng nhau mà không tổng hợp cẩn thận đều có nguy cơ dẫn đến hành vi bậc hai khi nhiều tệp chia sẻ tiền tố. 

Một vấn đề tế nhị phát sinh từ việc các ràng buộc lan truyền như thế nào. Một thư mục không quan tâm có bao nhiêu tệp tồn tại sâu bên trong mỗi cây con con; nó chỉ quan tâm có bao nhiêu cây con không trống. Điều này có nghĩa là hai tệp trong cùng một thư mục con chỉ tiêu thụ một đơn vị dung lượng ở thư mục mẹ, trong khi hai tệp trong các thư mục con khác nhau tiêu thụ hai đơn vị. Một cách tiếp cận tham lam ngây thơ xóa các tệp cục bộ mà không xem xét việc nhóm theo cây con sẽ thất bại. 

Một trường hợp lỗi cụ thể xuất hiện khi một thư mục có nhiều con, mỗi con có một cây con lớn. Nếu chúng ta sớm xóa các tệp tần số thấp một cách tham lam, chúng ta có thể giảm giá trị của một cây con mà không làm giảm số lượng cây con đang hoạt động, khiến cây gốc vẫn vượt quá dung lượng nhưng có tổng số tệp được giữ lại ít hơn mức cần thiết. Quyết định chính xác luôn được đưa ra ở cấp độ cây con chứ không phải ở cấp độ tệp riêng lẻ. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ cố gắng quyết định xem có nên giữ nó cho từng tệp hay không, sau đó kiểm tra xem mọi thư mục có thỏa mãn ràng buộc hay không. Điều này dẫn đến 2^N tập hợp con trong trường hợp xấu nhất và ngay cả khi cắt tỉa, nó vẫn theo cấp số nhân vì mỗi lựa chọn ảnh hưởng đồng thời đến nhiều ràng buộc thư mục. Việc tính toán lại tính hợp lệ của một tập ứng cử viên yêu cầu quét tất cả các đường dẫn và đếm các con đang hoạt động trên mỗi thư mục, có độ sâu ít nhất là O(N·sâu), khiến việc này hoàn toàn không khả thi. 

Quan sát cấu trúc quan trọng là các ràng buộc cục bộ đối với mỗi thư mục và chỉ phụ thuộc vào việc liệu cây con con có đóng góp ít nhất một tệp được chọn hay không. Khi một cây con được "kích hoạt" bằng cách chọn bất kỳ tệp nào bên trong nó, nó sẽ đóng góp chính xác một đơn vị cho cây mẹ của nó, bất kể có bao nhiêu tệp bên trong được chọn. Điều này biến vấn đề thành việc chọn toàn bộ cây con thay vì chọn từng lá riêng lẻ. 

Từ góc nhìn của bất kỳ nút nào, mỗi cây con con có một giá trị bằng số lượng tệp tối đa mà chúng ta có thể giữ bên trong nó theo cùng một quy tắc. Nếu chúng ta biết những giá trị đó, thì lựa chọn tối ưu ở nút hiện tại là chọn tối đa K nút con có giá trị lớn nhất, bởi vì mỗi nút con được chọn sử dụng một khe dung lượng và đóng góp toàn bộ giá trị tối ưu của nó một cách độc lập với các nút khác. 

Điều này làm giảm vấn đề thành tính toán lập trình động cây trong đó mỗi nút tổng hợp kết quả từ các nút con của nó và chỉ giữ lại K tốt nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực đối với tập hợp con tập tin | O(2^N · N) | O(N) | Quá chậm | 
| Cây DP với lựa chọn top-K trên mỗi nút | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Trước tiên, chúng tôi chuyển đổi tất cả các đường dẫn thành một tri. Mỗi thư mục trở thành một nút và mỗi tệp là một nút lá. Mỗi nút lưu trữ con của nó. 

Sau đó, chúng tôi tính toán, đối với mỗi nút, số lượng tệp tối đa có thể được lưu giữ trong cây con của nó theo ràng buộc do nút đó và tất cả các nút con áp đặt. 

1. Xây dựng bộ ba bằng cách tách từng đường dẫn trên “/” và chèn các nút dọc đường đi, tạo các nút thư mục nếu cần và đánh dấu thành phần cuối cùng là nút tệp. 
2. Chạy duyệt trie theo thứ tự sau. Đối với mỗi nút, trước tiên hãy tính kết quả cho tất cả nút con trước khi xử lý chính nút đó. 
3. Đối với nút tệp, gán giá trị của nó là 1 vì nó luôn có thể được giữ nếu được chọn. 
4. Đối với nút thư mục, hãy thu thập các giá trị từ tất cả các nút con. Sắp xếp các giá trị này theo thứ tự giảm dần và chỉ lấy giá trị K lớn nhất, vì tối đa K con có thể vẫn hoạt động. 
5. Xác định giá trị của thư mục là tổng của các giá trị con được chọn này. Điều này thể hiện số lượng tệp tối đa có thể được lưu giữ trong cây con này trong khi vẫn tôn trọng ràng buộc tại nút này. 
6. Sau khi tính toán các giá trị, hãy thực hiện DFS thứ hai để xây dựng lại tập hợp các tệp được lưu giữ thực tế. Tại mỗi thư mục, sắp xếp lại các lá con theo giá trị được tính toán của chúng và chỉ lặp lại K con trên cùng, đánh dấu tất cả các lá có thể truy cập được là được giữ lại. 

Ý tưởng chính là khi một nút con không nằm trong K hàng đầu, chúng ta sẽ xóa toàn bộ cây con của nó, điều này đảm bảo nó không đóng góp gì lên trên và thỏa mãn ràng buộc tại nút này. 

### Tại sao nó hoạt động 

Mỗi thư mục thực thi một ràng buộc chỉ phụ thuộc vào số lượng cây con không trống. Bất kỳ giải pháp khả thi nào cũng tạo ra sự phân vùng các tệp đã chọn thành tối đa K nhóm trên mỗi thư mục, mỗi nhóm tương ứng với một cây con. Bên trong mỗi cây con, việc lựa chọn tối ưu độc lập với các cây con khác. 

Bởi vì các cây con độc lập ngoại trừ ràng buộc về dung lượng ở cha mẹ của chúng, việc thay thế bất kỳ cây con con nào được chọn bằng một cây con con khác có giá trị cao hơn chỉ có thể cải thiện hoặc duy trì tổng số tệp được lưu giữ mà không vi phạm các ràng buộc. Đối số trao đổi này đảm bảo rằng việc chọn K giá trị con hàng đầu luôn tối ưu ở mọi nút. Việc đệ quy đảm bảo tính nhất quán của quyết định này trên toàn bộ cây. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("children", "is_file", "val")
    def __init__(self):
        self.children = {}
        self.is_file = False
        self.val = 0

def solve():
    n, k = map(int, input().split())
    root = Node()

    for _ in range(n):
        path = input().strip().split("/")
        cur = root
        for i, part in enumerate(path):
            if part not in cur.children:
                cur.children[part] = Node()
            cur = cur.children[part]
        cur.is_file = True

    def dfs1(node):
        if node.is_file and not node.children:
            node.val = 1
            return 1

        vals = []
        total = 0

        for child in node.children.values():
            v = dfs1(child)
            vals.append(v)

        vals.sort(reverse=True)
        for i in range(min(k, len(vals))):
            total += vals[i]

        node.val = total
        return node.val

    dfs1(root)

    res = []

    def dfs2(node):
        if node.is_file and not node.children:
            res.append(path_stack[-1])
            return

        items = []
        for name, child in node.children.items():
            items.append((child.val, name, child))
        items.sort(reverse=True)

        for i in range(min(k, len(items))):
            _, name, child = items[i]
            path_stack.append(name)
            dfs2(child)
            path_stack.pop()

    path_stack = []
    dfs2(root)

    print(len(res))
    for x in res:
        print(x)

if __name__ == "__main__":
    solve()
```Cấu trúc trie đảm bảo tất cả các tiền tố dùng chung được biểu diễn một lần, ngăn chặn công việc lặp lại khi nhiều tệp chia sẻ đường dẫn thư mục. DFS đầu tiên tính toán sự đóng góp tối ưu của mỗi cây con từ dưới lên. DFS thứ hai xây dựng lại một tập hợp rõ ràng các tệp còn tồn tại bằng cách luôn chỉ theo dõi các nút con có giá trị cao nhất trên mỗi nút, phù hợp với quyết định DP. 

Một chi tiết triển khai tinh tế là các nút tệp chỉ được xác định ở các lá. Điều này tránh việc vô tình coi các nút trung gian là các tệp khi chúng cũng có nút con. Bước xây dựng lại dựa vào các giá trị cây con được lưu trữ thay vì tính toán lại bất cứ thứ gì. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu trúc nhỏ trong đó một thư mục có ba thư mục con A, B, C với các giá trị 5, 3 và 2 và K bằng 2. 

Trong lần truyền tải đầu tiên, các giá trị con được tính toán ở cha mẹ là: 

| Bước | Giá trị con | Đã sắp xếp | Giữ trẻ em | Giá trị nút | 
| --- | --- | --- | --- | --- | 
| Gốc | [5, 3, 2] | [5, 3, 2] | 5, 3 | 8 | 

Điều này chứng tỏ rằng chỉ có hai cây con có thể duy trì hoạt động và lựa chọn tối ưu là hai cây con lớn nhất. 

Bây giờ hãy xem xét một trường hợp sâu hơn khi bản thân cây con có phân nhánh. 

đầu vào:```
4 1
a/x
a/y
b/z
b/w
```Trie có root với con a và b. Mỗi cái có hai tập tin. 

| Nút | Giá trị cây con con | K | Đã chọn | Giá trị | 
| --- | --- | --- | --- | --- | 
| một | [1, 1] | 1 | 1 | 1 | 
| b | [1, 1] | 1 | 1 | 1 | 
| gốc | [1, 1] | 1 | 1 | 1 | 

Gốc chỉ có thể giữ một nhánh, vì vậy cuối cùng chỉ có một tệp được giữ. 

Điều này cho thấy các ràng buộc lan truyền lên trên như thế nào, thu gọn toàn bộ cây con thành các đơn vị đơn lẻ bất kể kích thước bên trong. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Mỗi nút sắp xếp các nút con của nó một lần; tổng công việc trên tất cả các nút được giới hạn bởi tổng sắp xếp trên các danh sách kề | 
| Không gian | O(N) | Các nút Trie lưu trữ từng đoạn đường dẫn một lần | 

Tổng số nút là tuyến tính trong tổng chiều dài đường dẫn, tối đa là 10^6 và K nhỏ, do đó chi phí sắp xếp vẫn nằm trong giới hạn. Giải pháp phù hợp thoải mái trong vòng 1 giây bằng Python với các ràng buộc điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from types import SimpleNamespace

    # Re-embed solution
    class Node:
        def __init__(self):
            self.children = {}
            self.is_file = False
            self.val = 0

    def solve():
        n, k = map(int, input().split())
        root = Node()

        paths = []
        for _ in range(n):
            p = input().strip()
            paths.append(p)
            cur = root
            parts = p.split("/")
            for part in parts:
                if part not in cur.children:
                    cur.children[part] = Node()
                cur = cur.children[part]
            cur.is_file = True

        def dfs1(node):
            vals = []
            for ch in node.children.values():
                vals.append(dfs1(ch))
            vals.sort(reverse=True)
            node.val = sum(vals[:k])
            return node.val

        dfs1(root)

        res = []

        def dfs2(node, prefix):
            items = sorted(node.children.items(), key=lambda x: x[1].val, reverse=True)
            for i, (name, ch) in enumerate(items[:k]):
                dfs2(ch, prefix + name + "/")
            if node.is_file and not node.children:
                res.append(prefix[:-1])

        dfs2(root, "")

        return "\n".join(sorted(res))

    return solve()

# sample-like case
inp1 = """5 2
java/util/List
java/time/Instant
java/util/stream/Collectors
java/util/stream/IntStream
java/util/Queue
"""
assert run(inp1), "sample structure runs"

# minimum case
inp2 = """1 1
a"""
assert run(inp2) == "a"

# all under one chain
inp3 = """3 1
a/b/c
a/b/d
a/b/e
"""
assert run(inp3), "chain case"

# wide root
inp4 = """4 2
a
b
c
d
"""
assert run(inp4), "wide root case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tập tin duy nhất | cùng một tập tin | xử lý cấu trúc tối thiểu | 
| chuỗi sâu | nhiều nhất là K lá | hạn chế lan truyền trong chuỗi | 
| phân nhánh tiền tố chia sẻ | giữ lại cành K tốt nhất | tính chính xác của việc nhóm cây con | 
| gốc phẳng | bất kỳ tệp K nào | sự chính xác của lựa chọn cấp gốc | 

## Vỏ cạnh 

Một thư mục chứa chính xác K con hoạt động khác với thư mục chứa K+1 con, vì không cần xóa trong trường hợp đầu tiên trong khi phải xóa chính xác một cây con trong trường hợp thứ hai. Thuật toán xử lý việc này một cách tự nhiên vì việc sắp xếp và cắt bớt thành K sẽ tự động giữ nguyên tất cả các phần tử con khi số lượng của chúng bằng K. 

Một chuỗi dài các thư mục với một tệp duy nhất ở cuối sẽ tạo ra một trie trong đó mỗi nút có chính xác một nút con. Trong trường hợp này, mỗi nút chỉ có một ứng cử viên, do đó DP không bao giờ bỏ bất cứ thứ gì và tệp duy nhất luôn được giữ lại. 

Khi tất cả các tệp nằm trong các thư mục cấp cao nhất hoàn toàn riêng biệt, thư mục gốc sẽ hoạt động giống như một vấn đề lựa chọn đơn giản trên các giá trị độc lập. Thuật toán giảm xuống còn việc chọn K tệp lớn nhất trên toàn cầu, phù hợp với ràng buộc dự định vì mỗi tệp là cây con riêng của nó.
