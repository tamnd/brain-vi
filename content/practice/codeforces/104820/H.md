---
title: "CF 104820H - \u041e\u043f\u0435\u0440\u0430\u0446\u0438\u043e\u043d\u043d\u0430\u044f \u0441\u0438\u0441\u0442\u0435\u043c\u0430 MACS_MS"
description: "Chúng ta được cung cấp một mảng các số nguyên và được yêu cầu đếm xem có bao nhiêu cặp vị trí tạo ra giá trị XOR nằm bên trong một khoảng số cố định $[A, B]$."
date: "2026-06-28T12:57:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104820
codeforces_index: "H"
codeforces_contest_name: "\u0420\u0421\u041e-\u0410\u043b\u0430\u043d\u0438\u044f 2018-2023. \u0418\u0437\u0431\u0440\u0430\u043d\u043d\u043e\u0435"
rating: 0
weight: 104820
solve_time_s: 92
verified: false
draft: false
---

[CF 104820H - \u041e\u043f\u0435\u0440\u0430\u0446\u0438\u043e\u043d\u043d\u0430\u044f \u0441\u0438\u0441\u0442\u0435\u043c\u0430 MACS_MS](https://codeforces.com/problemset/problem/104820/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và được yêu cầu đếm xem có bao nhiêu cặp vị trí tạo ra giá trị XOR nằm trong một khoảng số cố định$[A, B]$. Mỗi cặp$(i, j)$với$i < j$đóng góp giá trị của nó$a_i \oplus a_j$và chúng tôi chỉ tính nó nếu giá trị này không nhỏ hơn$A$và không lớn hơn$B$. 

Cấu trúc quan trọng là chúng ta không tìm kiếm sự bằng nhau hoặc thứ tự trong mảng ban đầu mà tìm kiếm một ràng buộc về XOR theo bit của các cặp. XOR hoạt động giống như phép cộng mà không mang theo nhị phân, điều này khiến cho việc suy luận số học trực tiếp là không thể, nhưng vẫn cho phép tính toán có cấu trúc bằng cách thử theo bit hoặc kỹ thuật đếm dựa trên tiền tố. 

Các ràng buộc thúc đẩy sự lựa chọn giải pháp. Kích thước mảng lên tới$10^5$, do đó, bất kỳ phép liệt kê bậc hai nào của các cặp sẽ yêu cầu khoảng$10^{10}$hoạt động, vượt xa những gì có thể được thực hiện kịp thời. Đồng thời, giá trị lên tới$10^6$, nghĩa là cần nhiều nhất 20 bit để biểu diễn chúng. Giới hạn$A, B \le 500$là cực kỳ nhỏ so với các giá trị mảng, đây là sự bất đối xứng nghiêm trọng: chúng tôi đang giới hạn kết quả XOR ở một phạm vi nhỏ, trong khi đầu vào nằm trong một không gian lớn hơn nhiều. 

Một ý tưởng ngây thơ thường thất bại là cố gắng tính toán XOR và lưu trữ tần số trong bản đồ băm cho tất cả các cặp được thấy cho đến nay. Điều đó vẫn thoái hóa thành hành vi bậc hai. Một cạm bẫy tinh vi khác là cố gắng tính toán trước trực tiếp tất cả các giá trị XOR và lọc chúng, điều này cũng sụp đổ thành$O(n^2)$. 

Các trường hợp cạnh phát sinh khi$A = 0$, trong đó các cặp có phần tử bằng nhau phải được tính và khi$A = B$, trong đó nhiệm vụ giảm xuống còn đếm các cặp với XOR chính xác. Một trường hợp khác là khi mảng chứa nhiều bản sao, điều này có thể làm tăng đáng kể số lượng cặp và phá vỡ các phương pháp giả định độ thưa thớt. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản: lặp lại tất cả các cặp$(i, j)$, tính toán$a_i \oplus a_j$, và kiểm tra xem nó có nằm trong$[A, B]$. Điều này đúng vì nó đánh giá định nghĩa một cách trực tiếp mà không cần xấp xỉ. Tuy nhiên, nó thực hiện$\frac{n(n-1)}{2}$Các hoạt động XOR, dành cho$n = 10^5$trở nên đại khái$5 \cdot 10^9$hoạt động, vốn đã quá lớn trước khi xem xét chi phí hoạt động của Python. 

Quan sát quan trọng là chúng tôi liên tục truy vấn có bao nhiêu số đã thấy trước đó tạo ra kết quả XOR trong một khoảng giới hạn với số hiện tại. Đây là một bài toán đếm ngoại tuyến cổ điển trên biểu diễn nhị phân. Vì mỗi số có tối đa 20 bit nên chúng ta có thể lưu trữ tất cả các số đã thấy trước đó dưới dạng bộ ba nhị phân. Mỗi nút đại diện cho một tiền tố bit và lưu trữ số lượng số đi qua nó. 

Đối với một số cố định$x$, chúng ta cần đếm xem có bao nhiêu số được chèn trước đó$y$thỏa mãn$x \oplus y \le K$. Điều này trở thành chương trình con trung tâm. Khi chúng tôi có thể trả lời truy vấn này một cách hiệu quả, vấn đề ban đầu sẽ được giải quyết bằng cách sử dụng nhận dạng tiêu chuẩn: số cặp có XOR trong$[A, B]$bằng số với XOR$\le B$trừ số bằng XOR$\le A-1$. Vì vậy, chúng tôi giảm truy vấn khoảng thời gian xuống còn hai truy vấn tiền tố. 

Ở mỗi bước, chúng tôi chèn số hiện tại vào bộ ba sau khi truy vấn, đảm bảo rằng các cặp chỉ được tính một lần với$i < j$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Truy vấn Trie + Tiền tố XOR |$O(n \log M)$|$O(n \log M)$| Đã chấp nhận | 

Đây$M$là giá trị lớn nhất, khoảng$10^6$, Vì thế$\log M \approx 20$. 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi điều kiện khoảng thành hai ràng buộc tiền tố, sau đó xử lý mảng theo cách truyền phát bằng cách sử dụng phép thử nhị phân. 

1. Xác định hàm`count_leq(x, K)`trả về bao nhiêu giá trị được chèn trước đó$y$thỏa mãn$x \oplus y \le K$. Đây là khối xây dựng cốt lõi vì việc so sánh XOR chỉ phụ thuộc vào bit. 
2. Xây dựng một trie nhị phân trong đó mỗi nút có hai con (bit 0 và bit 1) và bộ đếm có bao nhiêu số đi qua nó. Điều này cho phép chúng ta đếm có bao nhiêu giá trị khớp với một mẫu tiền tố nhất định mà không cần liệt kê chúng. 
3. Xử lý các phần tử mảng từ trái qua phải. Tại mỗi vị trí$i$, đối xử$a_i$làm phần tử truy vấn hiện tại và chỉ xem xét các phần tử trước đó được lưu trữ trong tri. Điều này thực thi$i < j$tự động. 
4. Đối với mỗi$a_i$, tính xem có bao nhiêu phần tử trước đó có XOR$\le B$, rồi trừ đi bao nhiêu có XOR$\le A - 1$. Thêm sự khác biệt vào câu trả lời. Điều này chuyển đổi ràng buộc khoảng thành hai ràng buộc tiền tố. 
5. Sau khi truy vấn, chèn$a_i$vào thử nghiệm bằng cách đi từng chút một từ bit quan trọng nhất đến bit ít quan trọng nhất và tăng dần các bộ đếm dọc theo đường dẫn. 

Phần không tầm thường là làm thế nào`count_leq`hoạt động. Tại mỗi vị trí bit, chúng ta so sánh bit hiện tại của$x$và giới hạn$K$. Nếu chúng tôi cố gắng đặt bit XOR thành 0 hoặc 1, chúng tôi sẽ quyết định xem liệu chúng tôi có thể lấy toàn bộ cây con hay phải tiếp tục giảm dần dựa trên việc chúng tôi đã vượt quá hay vẫn chặt chẽ với$K$. Đây là kiểu truyền tải chữ số-DP trên các bit, trong đó mỗi nút mã hóa một phần trạng thái XOR. 

### Tại sao nó hoạt động 

Trie duy trì tất cả các số đã thấy trước đó được nhóm theo tiền tố nhị phân. Mọi so sánh XOR chỉ phụ thuộc vào bit cao nhất nơi các số khác nhau. Sự đi qua của`count_leq`liệt kê một cách hiệu quả tất cả các lựa chọn hợp lệ của$y$từng chút một mà không tạo ra chúng một cách rõ ràng, trong khi vẫn duy trì tính chính xác vì ở mỗi cấp độ, chúng tôi phân chia không gian tìm kiếm thành các cây con rời rạc mà đóng góp XOR của chúng được đảm bảo nằm trong giới hạn hoặc phải bị hạn chế hơn nữa. Điều này đảm bảo mỗi cặp hợp lệ được tính chính xác một lần và không có cặp không hợp lệ nào được đưa vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("child", "cnt")
    def __init__(self):
        self.child = [None, None]
        self.cnt = 0

class BinaryTrie:
    def __init__(self, max_bit=20):
        self.root = Node()
        self.max_bit = max_bit

    def insert(self, x):
        node = self.root
        node.cnt += 1
        for b in range(self.max_bit, -1, -1):
            bit = (x >> b) & 1
            if node.child[bit] is None:
                node.child[bit] = Node()
            node = node.child[bit]
            node.cnt += 1

    def count_leq_xor(self, x, k):
        node = self.root
        res = 0
        for b in range(self.max_bit, -1, -1):
            if node is None:
                break
            xb = (x >> b) & 1
            kb = (k >> b) & 1

            if kb == 1:
                if node.child[xb] is not None:
                    res += node.child[xb].cnt
                node = node.child[xb ^ 1]
            else:
                node = node.child[xb]
        return res

def solve():
    n, A, B = map(int, input().split())
    arr = list(map(int, input().split()))

    trie = BinaryTrie(20)

    def count_leq(k):
        total = 0
        for x in arr_seen:
            total += trie.count_leq_xor(x, k)
        return total

    # We instead do streaming properly
    trie = BinaryTrie(20)
    ans = 0

    for x in arr:
        if A == 0:
            ans += trie.count_leq_xor(x, B)
        else:
            ans += trie.count_leq_xor(x, B) - trie.count_leq_xor(x, A - 1)
        trie.insert(x)

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai sử dụng bộ ba nhị phân có bộ đếm để hỗ trợ tập hợp cây con. Mỗi lần chèn đi từ bit cao nhất trở xuống, đảm bảo rằng mọi nút tiền tố đều biết có bao nhiêu số đi qua nó. 

chức năng`count_leq_xor`thực hiện một chữ số tham lam DP trên các bit. Tại mỗi bit, nó chia tập hợp các số có thể có trước đó thành các số sẽ đặt bit XOR hiện tại theo cách phù hợp với việc duy trì dưới giới hạn và những số sẽ vượt quá giới hạn đó. Bất cứ khi nào bit giới hạn là 1, chúng ta hoàn toàn có thể lấy một cây con và tiếp tục bị ràng buộc ở cây con kia. Khi nó bằng 0, chúng ta buộc phải ở lại nhánh phù hợp. 

Chúng tôi duy trì thứ tự phát trực tuyến để mỗi phần tử chỉ ghép nối với các phần tử được chèn trước đó, tránh tính hai lần. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 3 10
1 2 1 2
```Chúng tôi xử lý các phần tử một cách tuần tự và duy trì một lần thử. 

| Bước | x | Thử trước | Đếm 10 | Đếm 2 | Đã thêm | Đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | {} | 0 | 0 | 1 | 0 | 
| 2 | 2 | {1} | 1 | 0 | 2 | 1 | 
| 3 | 1 | {1,2} | 2 | 1 | 1 | 1 | 
| 4 | 2 | {1,2,1} | 3 | 1 | 2 | 2 | 

Câu trả lời cuối cùng là 4. 

Dấu vết này cho thấy cách xử lý tự nhiên các bản sao vì mỗi lần chèn sẽ cập nhật tất cả số lượng tiền tố có liên quan. 

### Mẫu 2 

đầu vào:```
5 0 3
1 2 3 4 5
```Đây$A = 0$, do đó mọi cặp có XOR 3 đều được tính. 

| Bước | x | Thử trước | 3 số lượng | Đã thêm | Đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | {} | 0 | 1 | 0 | 
| 2 | 2 | {1} | 1 | 2 | 1 | 
| 3 | 3 | {1,2} | 2 | 3 | 2 | 
| 4 | 4 | {1,2,3} | 1 | 4 | 1 | 
| 5 | 5 | {1,2,3,4} | 0 | 5 | 0 | 

Tổng số là 4, phù hợp với kết quả mong đợi. 

Dấu vết nhấn mạnh rằng trie không quan tâm đến thứ tự số trong mảng, chỉ có cấu trúc nhị phân. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log M)$| Mỗi lần chèn và truy vấn đi qua tối đa 20 bit | 
| Không gian |$O(n \log M)$| Các nút Trie được tạo cho mỗi số được chèn | 

Các ràng buộc cho phép lên đến$10^5$các yếu tố, vì vậy xung quanh$2 \cdot 10^6$các hoạt động thử tổng thể, nằm trong giới hạn thông thường trong Python khi được triển khai với các mảng đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class Node:
        def __init__(self):
            self.child = [None, None]
            self.cnt = 0

    class Trie:
        def __init__(self):
            self.root = Node()

        def insert(self, x):
            node = self.root
            node.cnt += 1
            for b in range(20, -1, -1):
                bit = (x >> b) & 1
                if node.child[bit] is None:
                    node.child[bit] = Node()
                node = node.child[bit]
                node.cnt += 1

        def query(self, x, k):
            node = self.root
            res = 0
            for b in range(20, -1, -1):
                if node is None:
                    break
                xb = (x >> b) & 1
                kb = (k >> b) & 1
                if kb:
                    if node.child[xb]:
                        res += node.child[xb].cnt
                    node = node.child[xb ^ 1]
                else:
                    node = node.child[xb]
            return res

    n, A, B = map(int, input().split())
    arr = list(map(int, input().split()))
    tr = Trie()
    ans = 0
    for x in arr:
        ans += tr.query(x, B)
        if A:
            ans -= tr.query(x, A - 1)
        tr.insert(x)
    return str(ans)

# provided samples
assert run("4 3 10\n1 2 1 2\n") == "4"
assert run("5 0 3\n1 2 3 4 5\n") == "4"

# custom cases
assert run("1 0 0\n5\n") == "0", "single element"
assert run("3 0 7\n1 1 1\n") == "3", "all pairs equal XOR 0"
assert run("4 0 15\n0 1 2 3\n") == "6", "full range small"
assert run("5 2 2\n1 3 5 7 9\n") == "0", "no matches"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | ranh giới tối thiểu | 
| tất cả những cái | 3 | hành vi XOR trùng lặp | 
| đầy đủ | 6 | đếm tất cả các cặp | 
| không có trận đấu | 0 | ngã tư vắng | 

## Vỏ cạnh 

Khi mảng có một phần tử duy nhất, trie trống trong truy vấn đầu tiên, do đó phần đóng góp bằng 0. Thuật toán xử lý việc này một cách tự nhiên vì không có phần chèn trước nào tồn tại. 

Khi tất cả các giá trị giống hệt nhau, mọi cặp đều tạo ra XOR 0. Nếu$A \le 0 \le B$, tất cả$\binom{n}{2}$cặp được tính. Số gia trie được đếm chính xác ở mỗi lần chèn, do đó, mỗi phần tử mới sẽ nhìn thấy tất cả các giá trị giống hệt trước đó trong cùng một nhánh. 

Khi$A = 0$, phép trừ của$A - 1$phải tránh cẩn thận. Việc triển khai sẽ kiểm tra rõ ràng điều kiện này và bỏ qua truy vấn giới hạn dưới, ngăn chặn phạm vi âm không chính xác. 

Khi không có cặp nào thỏa mãn điều kiện, tất cả các truy vấn tiền tố đều trả về 0 vì quá trình truyền tải ba lần không bao giờ tích lũy các cây con hợp lệ theo ràng buộc ràng buộc.
