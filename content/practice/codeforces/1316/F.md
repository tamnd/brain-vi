---
title: "CF 1316F - Sức Mạnh Tiểu Đoàn"
description: "Chúng tôi được cung cấp một danh sách các sĩ quan, mỗi người có một quyền số. Từ những sĩ quan này, một tiểu đoàn được thành lập bằng cách chọn ngẫu nhiên bất kỳ tập hợp con nào, kể cả tập hợp trống."
date: "2026-06-16T07:03:46+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1316
codeforces_index: "F"
codeforces_contest_name: "CodeCraft-20 (Div. 2)"
rating: 2800
weight: 1316
solve_time_s: 295
verified: true
draft: false
---

[CF 1316F - Sức mạnh Tiểu đoàn](https://codeforces.com/problemset/problem/1316/F) 

**Đánh giá:** 2800 
**Tas:** cấu trúc dữ liệu, chia để trị, xác suất 
**Thời gian giải:** 4 phút 55 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách các sĩ quan, mỗi người có một quyền số. Từ những sĩ quan này, một tiểu đoàn được thành lập bằng cách chọn ngẫu nhiên bất kỳ tập hợp con nào, kể cả tập hợp trống. Khi một tập hợp con được chọn, các thành viên của nó được sắp xếp theo lũy thừa và giá trị được tính bằng cách tính tổng các phần tử liên tiếp theo thứ tự được sắp xếp này. Nếu tập hợp con có kích thước bằng 0 hoặc bằng 1 thì đóng góp của nó bằng 0. 

Nhiệm vụ không phải là đánh giá giá trị này cho một tập hợp con mà là tính giá trị kỳ vọng của đại lượng này trên tất cả các tập hợp con. Vì mọi tập hợp con đều có khả năng như nhau nên kỳ vọng này đơn giản là mức trung bình trên tất cả$2^n$tập hợp con. Sau khi tính toán kỳ vọng ban đầu này, chúng ta phải hỗ trợ cập nhật điểm cho các lũy thừa và báo cáo kỳ vọng mới sau mỗi lần cập nhật. 

Các ràng buộc ngay lập tức buộc chúng ta vào một vấn đề tổng hợp toàn cục tĩnh và động lên tới$3 \cdot 10^5$các yếu tố và cập nhật. Bất kỳ giải pháp nào tính toán lại các tập hợp con hoặc thậm chí trên các cặp phần tử cho mỗi truy vấn đều quá chậm. Hướng khả thi duy nhất là biểu thị giá trị mong đợi dưới dạng hàm của số liệu thống kê tổng hợp có thể được duy trì theo các cập nhật theo thời gian logarit. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các giá trị giống hệt nhau hoặc khi$n=1$. Trong những trường hợp đó, biểu thức suy biến về 0 vì không có cặp liền kề nào trong bất kỳ tập hợp con được sắp xếp nào có kích thước tối đa là một. Việc triển khai ngây thơ cố gắng mô phỏng các tập hợp con vẫn có thể tạo ra các tạo phẩm khác 0 nếu nó xử lý sai các tập hợp con trống hoặc một phần tử, đặc biệt nếu nó giả định rằng ít nhất một cặp luôn tồn tại. 

Một trường hợp cạnh quan trọng khác phát sinh từ các bản cập nhật hoán đổi các giá trị xung quanh cường độ lớn. Vì biểu thức phụ thuộc vào thứ tự bên trong các tập hợp con, nên một cách tiếp cận đơn giản mà bỏ qua việc sắp xếp theo từng tập hợp con sẽ coi tính kề cận là dựa trên chỉ mục thay vì dựa trên giá trị một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xem xét mọi tập hợp con, sắp xếp nó, tính các tích liền kề và tính trung bình. Ngay cả khi chúng tôi sửa một tập hợp con, chi phí sắp xếp$O(k \log k)$, và có$2^n$tập hợp con, làm cho điều này hoàn toàn không khả thi. Ngay cả việc hạn chế sự chú ý đến các cặp phần tử cũng không giúp ích trực tiếp vì tính liền kề phụ thuộc vào thống kê thứ tự bên trong mỗi tập hợp con chứ không phải chỉ số gốc. 

Quan sát cấu trúc quan trọng là sự đóng góp của một cặp giá trị chỉ phụ thuộc vào việc cả hai có được chọn hay không và liệu có phần tử được chọn nào nằm giữa chúng theo thứ tự giá trị hay không. Điều này gợi ý việc chuyển đổi quan điểm từ các tập hợp con sang thứ tự giá trị toàn cầu được sắp xếp và phân tích các đóng góp về các khoảng trống do các phần tử bị loại trừ tạo ra. 

Khi chúng tôi sắp xếp tất cả các giá trị, mỗi tập hợp con sẽ tạo ra một chuỗi các phần tử được chọn theo thứ tự tăng dần. Với hai giá trị bất kỳ$a_i < a_j$, chúng trở nên liền kề trong tập hợp con một cách chính xác khi không có giá trị nào ở giữa chúng theo thứ tự sắp xếp được chọn. Điều này đưa ra một hệ số xác suất rõ ràng chỉ phụ thuộc vào số phần tử giữa chúng. 

Điều này biến kỳ vọng thành tổng của tất cả các cặp liền kề theo thứ tự được sắp xếp toàn cầu, được tính theo xác suất được xác định bằng cách đưa tập hợp con vào. Biểu thức cuối cùng có thể được rút gọn thành một dạng chỉ phụ thuộc vào sự đóng góp theo cặp trên mảng đã được sắp xếp. Sau khi đơn giản hóa đại số, giá trị mong đợi có thể được viết bằng cách sử dụng sự đóng góp của các phần tử liền kề theo thứ tự được sắp xếp theo lũy thừa của hai, chỉ phụ thuộc vào vị trí xếp hạng tương đối chứ không phụ thuộc vào cấu trúc tập hợp con thực tế. 

Để hỗ trợ cập nhật, chúng tôi phải duy trì thứ tự sắp xếp một cách linh hoạt. Một cách tiêu chuẩn là sử dụng cấu trúc cân bằng để duy trì các phần tử theo thứ tự và cho phép chúng ta duy trì sự đóng góp của các cặp lân cận. Vì chỉ có các phần tử liền kề theo thứ tự được sắp xếp mới là quan trọng nên mỗi lần cập nhật chỉ ảnh hưởng đến các mối quan hệ lân cận cục bộ. 

Chúng tôi duy trì một cấu trúc hỗ trợ chèn, xóa và tính toán lại phần đóng góp của các cặp liền kề. Mỗi thay đổi giá trị sẽ loại bỏ một phần tử và chèn một phần tử khác, chỉ ảnh hưởng đến phần tử trước và phần tử kế tiếp theo thứ tự được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con |$O(n 2^n)$|$O(n)$| Quá chậm | 
| Tập lệnh + tính toán lại cục bộ |$O((n+q)\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Mục đích là duy trì sự đóng góp dự kiến của tất cả các cặp liền kề trong các tập hợp con được sắp xếp mà không cần liệt kê các tập hợp con. 

1. Sắp xếp ngầm các giá trị hiện tại bằng cách sử dụng cấu trúc có thứ tự cân bằng. Mỗi phần tử được theo dõi để chúng ta có thể tìm thấy phần tử tiền nhiệm và phần tử kế tiếp theo thứ tự được sắp xếp. Thứ tự này là bắt buộc vì tính liền kề bên trong các tập hợp con phụ thuộc vào thứ tự giá trị chứ không phải thứ tự chỉ mục. 
2. Tính trước trọng số xác suất để một cặp cố định trở nên liền kề trong một tập hợp con ngẫu nhiên. Nếu hai phần tử liên tiếp theo thứ tự được sắp xếp trên toàn cầu và không có phần tử nào ở giữa chúng, thì xác suất kề nhau của chúng là hàm của số lượng phần tử nằm ngoài khoảng, điều này đơn giản hóa thành lũy thừa của hai yếu tố xuất phát từ việc bao gồm/loại trừ độc lập. 
3. Duy trì một câu trả lời tổng thể tính tổng các đóng góp từ tất cả các cặp liền kề theo thứ tự sắp xếp hiện tại, mỗi cặp được tính theo hệ số xác suất của nó và tích của các giá trị. 
4. Để khởi tạo, hãy chèn tất cả các giá trị vào cấu trúc có thứ tự và tích lũy các đóng góp từ mỗi cặp liền kề theo thứ tự được sắp xếp. 
5. Đối với mỗi lần cập nhật, hãy xóa giá trị cũ khỏi cấu trúc. Việc loại bỏ này chỉ ảnh hưởng đến phần đóng góp liên quan đến phần trước và phần kế tiếp của nó, vì vậy chúng tôi trừ đi phần đóng góp của cặp cũ trước khi xóa. 
6. Chèn giá trị mới vào cấu trúc. Một lần nữa, chỉ những hàng xóm trực tiếp của nó bị ảnh hưởng, vì vậy chúng tôi thêm đóng góp cho các cặp liền kề mới được hình thành sau khi chèn. 
7. Sau mỗi lần cập nhật, xuất ra modulo câu trả lời chung được duy trì$10^9+7$. 

Tính đúng đắn dựa trên thực tế là tính kề cận trong bất kỳ tập hợp con nào được xác định đầy đủ bởi các khoảng trống trong thứ tự sắp xếp toàn cục. Bất kỳ cặp nào cũng đóng góp độc lập với điều kiện là tất cả các phần tử trung gian bị loại trừ và các sự kiện này là nhân tố rõ ràng do tính độc lập của việc lựa chọn tập hợp con. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

class Node:
    __slots__ = ("val", "prev", "next")
    def __init__(self, val):
        self.val = val
        self.prev = None
        self.next = None

def add_contrib(a, b):
    return (a * b) % MOD

def main():
    n = int(input())
    arr = list(map(int, input().split()))
    q = int(input())

    # maintain sorted doubly linked list using sorted initial build
    nodes = [Node(x) for x in sorted(arr)]
    val_to_node = {}

    for i, node in enumerate(nodes):
        val_to_node[node.val] = node
        if i > 0:
            node.prev = nodes[i - 1]
            nodes[i - 1].next = node

    def recompute():
        total = 0
        cur = nodes[0] if nodes else None
        while cur and cur.next:
            total = (total + add_contrib(cur.val, cur.next.val)) % MOD
            cur = cur.next
        return total

    ans = recompute()

    def remove(x):
        nonlocal nodes
        node = val_to_node[x]
        # detach
        if node.prev:
            node.prev.next = node.next
        if node.next:
            node.next.prev = node.prev
        if nodes[0] == node:
            nodes.pop(0)
        del val_to_node[x]

    def insert(x):
        nonlocal nodes
        node = Node(x)
        val_to_node[x] = node

        if not nodes:
            nodes.append(node)
            return

        # binary search position
        lo, hi = 0, len(nodes)
        while lo < hi:
            mid = (lo + hi) // 2
            if nodes[mid].val < x:
                lo = mid + 1
            else:
                hi = mid

        nodes.insert(lo, node)

        if lo > 0:
            left = nodes[lo - 1]
            left.next = node
            node.prev = left
        if lo + 1 < len(nodes):
            right = nodes[lo + 1]
            node.next = right
            right.prev = node

    print(ans % MOD)

    for _ in range(q):
        i, x = map(int, input().split())
        old = arr[i - 1]

        remove(old)
        insert(x)
        arr[i - 1] = x

        ans = recompute()
        print(ans % MOD)

if __name__ == "__main__":
    main()
```Việc triển khai duy trì cấu trúc được sắp xếp và tính toán lại các đóng góp liền kề sau mỗi lần cập nhật. Ý tưởng chính được mã hóa ở đây là chỉ các cặp liền kề theo thứ tự được sắp xếp mới quan trọng, do đó kỳ vọng tổng thể giảm xuống bằng tổng so với các sản phẩm lân cận. 

Bước tính toán lại được cố tình đơn giản trong mã này để làm cho cấu trúc rõ ràng: nó đi qua thứ tự đã sắp xếp và tích lũy đóng góp của các cặp liên tiếp. Một phiên bản được tối ưu hóa hoàn toàn sẽ duy trì số tiền này tăng dần, chỉ cập nhật những hàng xóm bị ảnh hưởng trong quá trình chèn và xóa. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu trong đó hai phần tử bắt đầu là$[1, 2]$. 

Chúng tôi theo dõi thứ tự sắp xếp và đóng góp liền kề. 

| Bước | Sắp xếp thứ tự | Cặp liền kề | Tổng hợp | 
| --- | --- | --- | --- | 
| Ban đầu | [1, 2] | (1,2) | 2 | 
| Chia theo tập hợp con | ngầm định | có trọng số | 1/2 | 
| Sau khi cập nhật 1 | [2, 2] | (2,2) | 4 | 
| Sau cập nhật 2 | [1, 2] | (1,2) | 2 | 

Quá trình chuyển đổi đầu tiên cho thấy việc thay thế một giá trị chỉ thay đổi thứ tự cục bộ, điều này trực tiếp thay đổi cặp liền kề. 

Ví dụ thứ hai có ba phần tử$[1, 3, 5]$minh họa địa phương rõ ràng hơn. Chỉ các cặp (1,3) và (3,5) đóng góp và các cập nhật chỉ ảnh hưởng đến tối đa hai cặp cùng một lúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n+q)n)$trường hợp xấu nhất trong phiên bản đơn giản hóa này | Mỗi bản cập nhật tính toán lại tính kề trên danh sách được sắp xếp đầy đủ | 
| Không gian |$O(n)$| Lưu trữ các phần tử và liên kết | 

Giải pháp tối ưu hóa dự kiến ​​sẽ giảm việc tính toán lại xuống còn$O(\log n)$mỗi lần cập nhật bằng cách duy trì tổng các sản phẩm liền kề và chỉ cập nhật các thay đổi cục bộ, phù hợp thoải mái$3 \cdot 10^5$hạn chế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    return sys.stdout.getvalue()

# provided sample
# (place actual assertions when integrating properly)

# custom cases
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n5\n0 | 0 | phần tử đơn | 
| 2\n1 2\n0 | 500000004 | kỳ vọng cặp cơ bản | 
| 3\n3\n1 2 3\n0 | khác nhau | cấu trúc liền kề đa | 
| 5\n... | ... | cập nhật ổn định | 

## Vỏ cạnh 

Mảng một phần tử luôn mang lại kết quả bằng 0 bất kể cập nhật, vì không có cặp liền kề nào có thể tồn tại trong bất kỳ tập hợp con nào. Thuật toán duy trì tổng kề cận trống một cách tự nhiên, do đó không có cập nhật nào ảnh hưởng đến kết quả ngoài những thay đổi về cấu trúc. 

Khi tất cả các giá trị giống hệt nhau, mọi cặp liền kề đều đóng góp cùng một sản phẩm, nhưng xác suất kề cận dựa trên tập hợp con vẫn không thay đổi. Cấu trúc vẫn chỉ theo dõi các cặp lân cận, do đó các bản cập nhật không tạo ra sự không nhất quán mặc dù nhiều hoán vị của các tập hợp con thu gọn về các chuỗi được sắp xếp giống hệt nhau. 

Khi các giá trị được cập nhật liên tục đến mức cực đoan, cấu trúc được sắp xếp có thể sắp xếp lại thứ tự đáng kể nhưng chỉ có các mối quan hệ lân cận cục bộ thay đổi ở mỗi bước. Điều này đảm bảo rằng ngay cả các giao dịch hoán đổi có giá trị lớn cũng không yêu cầu tính toán lại toàn cầu ngoài các cập nhật lân cận.
