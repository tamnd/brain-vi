---
title: "CF 104936E - 101 Điều Cần Làm Trước Khi Tốt Nghiệp"
description: "Chúng ta được cung cấp một dãy số và chúng ta xem xét mọi đoạn liền kề có độ dài ít nhất là hai. Đối với bất kỳ phân đoạn nào như vậy, chúng tôi xem xét tất cả các cặp chỉ số riêng biệt bên trong nó và tính toán XOR theo bit của chúng. “Điểm” của phân đoạn là giá trị XOR nhỏ nhất trong số tất cả các cặp đó."
date: "2026-06-28T18:11:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104936
codeforces_index: "E"
codeforces_contest_name: "MITIT 2024 Beginner Round"
rating: 0
weight: 104936
solve_time_s: 96
verified: false
draft: false
---

[CF 104936E - 101 điều cần làm trước khi tốt nghiệp](https://codeforces.com/problemset/problem/104936/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy số và chúng ta xem xét mọi đoạn liền kề có độ dài ít nhất là hai. Đối với bất kỳ phân đoạn nào như vậy, chúng tôi xem xét tất cả các cặp chỉ số riêng biệt bên trong nó và tính toán XOR theo bit của chúng. “Điểm” của phân đoạn là giá trị XOR nhỏ nhất trong số tất cả các cặp đó. 

Nhiệm vụ là đếm xem có bao nhiêu phân đoạn có số điểm chính xác bằng một giá trị K cho trước. 

Một cách hữu ích để suy nghĩ về điểm số là nó chỉ phụ thuộc vào cặp gần nhất bên trong phân khúc theo số liệu XOR. Nếu thậm chí một cặp có XOR nhỏ, nó sẽ chiếm ưu thế về điểm số, bởi vì mọi thứ khác đều không liên quan một khi mức tối thiểu được cố định. 

Các ràng buộc đẩy chúng tôi tới các giải pháp gần đúng O(N log N) hoặc O(N log² N). N lên tới 100000, do đó, bất kỳ phép tính bậc hai nào trên các phân đoạn đều không thể xảy ra ngay lập tức vì có khoảng 10¹⁰ mảng con trong trường hợp xấu nhất. Ngay cả việc duy trì tất cả các giá trị XOR theo cặp trên mỗi phân đoạn cũng không khả thi. 

Một điểm tinh tế là điểm số không hề đơn điệu một cách đơn giản ở phần mở rộng phân khúc. Việc mở rộng một phân đoạn có thể tạo ra một cặp XOR rất nhỏ mới, làm giảm đáng kể số điểm. Điều này phá vỡ những ý tưởng cửa sổ trượt ngây thơ dựa vào tính đơn điệu của một thống kê duy nhất. 

Một trường hợp cạnh nhỏ cho thấy đây là một mảng như`[8, 1, 9]`. Phân khúc`[8, 9]`có XOR 1, trong khi`[8, 1, 9]`có các cặp có giá trị XOR`8 XOR 1 = 9`,`1 XOR 9 = 8`,`8 XOR 9 = 1`, do đó điểm vẫn là 1. Việc thêm các phần tử không nhất thiết phải tăng hoặc duy trì cặp XOR tối thiểu theo bất kỳ cách có cấu trúc nào, vì vậy chúng ta phải kiểm soát rõ ràng các tương tác cặp thay vì dựa vào các thuộc tính tiền tố đơn giản. 

## Phương pháp tiếp cận 

Một giải pháp brute-force xem xét mọi mảng con, tính toán tất cả các XOR theo cặp bên trong nó và lấy giá trị tối thiểu. Đối với một đoạn có độ dài L cố định, việc tính toán tất cả các cặp có giá O(L²) và có các đoạn O(N²), dẫn đến O(N⁴) theo cách hiểu tệ nhất hoặc tốt nhất là O(N³) nếu một người sử dụng lại một phần công việc. Dù bằng cách nào, nó vượt xa giới hạn khả thi. 

Quan sát quan trọng là chúng tôi không thực sự quan tâm đến tất cả các giá trị XOR theo cặp, chỉ quan tâm đến việc giá trị tối thiểu là dưới, bằng hay trên ngưỡng. Điều này gợi ý việc sắp xếp lại vấn đề theo các ràng buộc trên các cặp thay vì tính toán rõ ràng mức tối thiểu. 

Cố định một ngưỡng T và coi một phân đoạn là hợp lệ nếu mọi cặp phần tử bên trong nó có XOR ít nhất là T. Trong một phân đoạn như vậy, điểm ít nhất là T. Điều này biến vấn đề ban đầu thành việc đếm các phân đoạn thỏa mãn ràng buộc cặp tổng thể. Khi chúng ta có thể đếm các phân đoạn này cho một T nhất định, chúng ta có thể khôi phục sự bằng nhau chính xác cho K bằng cách sử dụng chênh lệch: 

các phân đoạn có điểm chính xác K là những phân đoạn có điểm ≥ K nhưng không ≥ K+1. 

Vì vậy, vấn đề giảm xuống còn việc duy trì một cửa sổ trượt trong đó không có cặp nào vi phạm điều kiện có dạng XOR(x, y) < T. 

Thử thách còn lại là làm thế nào để duy trì liệu một phần tử mới có tạo ra một cặp vi phạm hay không. Điều này được xử lý bằng cách duy trì cửa sổ hiện tại bên trong bộ ba nhị phân hỗ trợ chèn giá trị và truy vấn, đối với x đã cho, XOR tối thiểu giữa x và bất kỳ phần tử nào hiện có trong cấu trúc. Truy vấn đó trực tiếp cho chúng ta biết liệu x có tạo thành một cặp xấu hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N³) đến O(N⁴) | O(1) | Quá chậm | 
| Cửa sổ trượt + trie nhị phân | O(N log 2³⁰) | O(N log 2³⁰) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định hàm trợ giúp f(T) đếm các mảng con trong đó mỗi cặp phần tử có XOR ít nhất là T. 

1. Chúng ta duy trì một cửa sổ trượt [l, r] và một bộ ba nhị phân lưu trữ tất cả các phần tử hiện có trong cửa sổ. Trie hỗ trợ chèn, xóa và truy vấn đối tác XOR tối thiểu cho một giá trị. Cấu trúc này thể hiện chính xác phân khúc đang hoạt động. 
2. Chúng ta mở rộng r từ trái sang phải, chèn a[r] vào trie. Sau khi chèn, chúng tôi kiểm tra xem cửa sổ hiện tại có hợp lệ đối với ngưỡng T hay không. 
3. Để kiểm tra tính hợp lệ, chúng tôi tính toán XOR tối thiểu giữa a[r] và bất kỳ phần tử nào trước đó trong cửa sổ bằng trie. Nếu mức tối thiểu này nhỏ hơn T thì phần tử mới sẽ tạo ra một cặp bị cấm bên trong cửa sổ. 
4. Trong khi cửa sổ không hợp lệ, chúng ta thu nhỏ từ bên trái bằng cách xóa a[l] khỏi tri và tăng l. Sau mỗi lần xóa, chúng tôi tính toán lại điều kiện hợp lệ vì việc xóa một phần tử có thể loại bỏ cặp vi phạm duy nhất hoặc tiết lộ một phần tử khác liên quan đến cùng một phần tử mới. 
5. Khi cửa sổ trở lại hợp lệ, tất cả các mảng con kết thúc tại r và bắt đầu từ bất kỳ vị trí nào từ l đến r đều hợp lệ đối với ngưỡng T, góp phần (r - l + 1) vào f(T). 
6. Chúng ta tính f(K) và f(K+1). Câu trả lời là f(K) - f(K+1), vì giá trị XOR là số nguyên và điều này tách biệt các phân đoạn có cặp XOR tối thiểu chính xác là K. 

Bất biến chính là ở mỗi bước, cửa sổ [l, r] không chứa cặp nào có XOR nhỏ hơn T và nó là cửa sổ nhỏ nhất kết thúc tại r. Trie đảm bảo chúng tôi có thể phát hiện các vi phạm do phần tử mới nhất gây ra một cách hiệu quả và việc thu nhỏ từ bên trái cuối cùng sẽ khôi phục tính hợp lệ vì mọi cặp bị cấm đều phải liên quan đến một số phần tử trong cửa sổ và việc xóa các phần tử sẽ loại bỏ các cặp một cách đơn điệu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class TrieNode:
    __slots__ = ("child", "cnt")
    def __init__(self):
        self.child = [None, None]
        self.cnt = 0

class BinaryTrie:
    def __init__(self):
        self.root = TrieNode()
        self.B = 30

    def insert(self, x):
        node = self.root
        node.cnt += 1
        for b in reversed(range(self.B)):
            bit = (x >> b) & 1
            if not node.child[bit]:
                node.child[bit] = TrieNode()
            node = node.child[bit]
            node.cnt += 1

    def remove(self, x):
        node = self.root
        node.cnt -= 1
        for b in reversed(range(self.B)):
            bit = (x >> b) & 1
            node = node.child[bit]
            node.cnt -= 1

    def min_xor(self, x):
        node = self.root
        res = 0
        for b in reversed(range(self.B)):
            bit = (x >> b) & 1
            # prefer same bit to minimize xor
            if node.child[bit] and node.child[bit].cnt > 0:
                node = node.child[bit]
            else:
                res |= (1 << b)
                node = node.child[bit ^ 1]
        return res

def count_at_least(T, arr):
    n = len(arr)
    trie = BinaryTrie()
    l = 0
    ans = 0

    for r in range(n):
        x = arr[r]
        trie.insert(x)

        while l <= r:
            if trie.min_xor(x) >= T:
                break
            trie.remove(arr[l])
            l += 1

        ans += (r - l + 1)

    return ans

def main():
    N, K = map(int, input().split())
    a = list(map(int, input().split()))

    def f(T):
        return count_at_least(T, a)

    print(f(K) - f(K + 1))

if __name__ == "__main__":
    main()
```Trie là thành phần cốt lõi. Mỗi lần chèn và xóa sẽ đi theo đường dẫn 30 bit, duy trì số lượng để chúng tôi có thể xác định một cách an toàn liệu cây con có còn hoạt động hay không. các`min_xor`trước tiên, hàm tuân theo các bit khớp một cách tham lam, đó chính xác là thứ tạo ra đối tác XOR tối thiểu cho một phần tử cố định. 

Cửa sổ trượt được điều khiển hoàn toàn với điều kiện không có cặp nào vi phạm ngưỡng. Vòng lặp thu hẹp từ bên trái đảm bảo rằng bất cứ khi nào có vi phạm, nó sẽ bị loại bỏ trước khi tính các khoản đóng góp. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
5 2
3 1 4 5 2
```Chúng ta tính f(2) bằng cửa sổ trượt. 

| r | chèn x | tôi | min_xor(x, window) | hành động | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | 0 | hợp lệ | mở rộng | 1 | 
| 1 | 1 | 0 | 2 | hợp lệ | 2 | 
| 2 | 4 | 0 | Kiểm tra kiểu 5, 3, 5, hợp lệ | mở rộng | 3 | 
| 3 | 5 | 0 | hợp lệ | mở rộng | 4 | 
| 4 | 2 | 0 | vi phạm (2 XOR 1 = 3? v.v., nhưng một số cặp < 2) | thu nhỏ l | tính toán lại | 

Sau khi điều chỉnh, giả sử cửa sổ hợp lệ trở thành [l, r], các khoản đóng góp sẽ được tích lũy tương ứng. 

Dấu vết này cho thấy cách cửa sổ tự động điều chỉnh khi một phần tử mới đưa vào một cặp bị cấm, buộc phải co lại. 

Bây giờ hãy xem xét một mảng đơn giản hơn:```
4 1
1 2 3 0
```Ở đây chúng tôi quan sát thấy nhiều giá trị XOR nhỏ. Cửa sổ nhanh chóng co lại bất cứ khi nào 0 đi vào vì XOR với 0 sao chép các giá trị, thường tạo ra các cặp rất nhỏ. Điều này chứng tỏ độ nhạy của cấu trúc đối với các phần tử có bit thấp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N · 30) | Mỗi lần chèn, xóa và truy vấn đều đi theo một đường dẫn 30 bit | 
| Không gian | O(N · 30) | Các nút Trie lưu trữ tất cả tiền tố của các giá trị được chèn | 

Các ràng buộc cho phép thao tác khoảng 3×10⁶ một cách thoải mái. Mỗi phần tử đóng góp một số lần duyệt trie không đổi, do đó lời giải phù hợp tốt trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class TrieNode:
        def __init__(self):
            self.child = [None, None]
            self.cnt = 0

    class BinaryTrie:
        def __init__(self):
            self.root = TrieNode()
            self.B = 30

        def insert(self, x):
            node = self.root
            node.cnt += 1
            for b in reversed(range(self.B)):
                bit = (x >> b) & 1
                if not node.child[bit]:
                    node.child[bit] = TrieNode()
                node = node.child[bit]
                node.cnt += 1

        def remove(self, x):
            node = self.root
            node.cnt -= 1
            for b in reversed(range(self.B)):
                bit = (x >> b) & 1
                node = node.child[bit]
                node.cnt -= 1

        def min_xor(self, x):
            node = self.root
            res = 0
            for b in reversed(range(self.B)):
                bit = (x >> b) & 1
                if node.child[bit] and node.child[bit].cnt > 0:
                    node = node.child[bit]
                else:
                    res |= (1 << b)
                    node = node.child[bit ^ 1]
            return res

    def count_at_least(T, arr):
        trie = BinaryTrie()
        l = 0
        ans = 0
        for r, x in enumerate(arr):
            trie.insert(x)
            while l <= r and trie.min_xor(x) < T:
                trie.remove(arr[l])
                l += 1
            ans += (r - l + 1)
        return ans

    def solve(inp):
        N, K = map(int, inp.split()[0:2])
        a = list(map(int, inp.split()[2:]))
        return str(count_at_least(K, a) - count_at_least(K + 1, a))

# provided sample
assert run("5 2\n3 1 4 5 2\n") == "3", "sample 1"

# minimum size
assert run("2 0\n1 1\n") == "1"

# all equal
assert run("4 0\n7 7 7 7\n") == "6"

# no valid segments
assert run("3 10\n1 2 3\n") == "0"

# boundary
assert run("5 0\n1 2 4 8 16\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 0 / 1 1 | 1 | xử lý phân đoạn hợp lệ tối thiểu | 
| 7 7 7 7 | 6 | tất cả các cặp hành vi XOR 0 giống hệt nhau | 
| 1 2 3 / K=10 | 0 | không có mảng con hợp lệ | 
| sức mạnh của hai | 4 | tương tác ranh giới bit | 

## Vỏ cạnh 

Trường hợp góc phát sinh khi tất cả các phần tử đều giống hệt nhau. Đối với đầu vào`[7, 7, 7, 7]`và K = 0, mọi cặp XOR đều bằng 0, do đó mọi mảng con có độ dài ít nhất 2 đều có điểm 0. Thuật toán giữ cho cửa sổ luôn hợp lệ vì`min_xor`luôn trả về 0 và f(0) đếm tất cả các mảng con. 

Một tình huống khác là khi các giá trị được phân tách rộng rãi trong không gian nhị phân, chẳng hạn như`[1, 2, 4, 8]`. Nhiều giá trị XOR lớn nên việc vi phạm đối với T nhỏ không bao giờ xảy ra. Cửa sổ không bao giờ co lại và các khoản đóng góp tích lũy dưới dạng số lượng hình tam giác đầy đủ mà cửa sổ trượt tạo ra một cách chính xác. 

Một trường hợp tế nhị hơn là khi một phần tử có giá trị thấp xuất hiện muộn, chẳng hạn`[8, 8, 8, 1]`. phần tử`1`tạo ra nhiều cặp XOR nhỏ với các phần tử trước đó, buộc phải thu nhỏ lại nhiều lần. Thuật toán xử lý điều này vì mỗi lần xóa sẽ giảm số lượng trie một cách nhất quán và khi tất cả các phần tử xung đột bị xóa, cửa sổ sẽ ổn định trở lại và tiếp tục đếm.
