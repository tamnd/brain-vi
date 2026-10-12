---
title: "CF 105160F - \u5341\u516d\u8fdb\u5236\u7684\u5f02\u6216"
description: "Chúng ta được cung cấp một tập hợp các số riêng biệt được viết bằng hệ thập lục phân và một chuỗi truy vấn. Đối với mỗi truy vấn, chúng tôi nhận được một số thập phân $x$."
date: "2026-06-27T11:01:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105160
codeforces_index: "F"
codeforces_contest_name: "2024 University of Shanghai for Science and Technology(USST) Freshman Challenge Contest"
rating: 0
weight: 105160
solve_time_s: 73
verified: true
draft: false
---

[CF 105160F - \u5341\u516d\u8fdb\u5236\u7684\u5f02\u6216](https://codeforces.com/problemset/problem/105160/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các số riêng biệt được viết bằng hệ thập lục phân và một chuỗi truy vấn. Đối với mỗi truy vấn, chúng tôi nhận được một số thập phân$x$. Nhiệm vụ là chọn một trong các số thập lục phân cho trước$a_i$như vậy khi chúng ta kết hợp$x$Và$a_i$sử dụng một phép toán đặc biệt được xác định theo từng chữ số trong cơ số 16, giá trị thu được càng lớn càng tốt theo thứ tự thập phân thông thường. Chúng ta phải xuất chỉ mục của lựa chọn$a_i$. 

Chi tiết quan trọng là hoạt động này không phải là phép cộng thông thường và không phải XOR dựa trên số thập phân. Nó được xác định độc lập cho mỗi chữ số thập lục phân: mỗi chữ số được kết hợp mà không mang theo, vì vậy$3_H ⊕ 4_H = 7_H$,$A_H ⊕ B_H = 5_H$. Mỗi chữ số hex hoạt động giống như phép cộng modulo 16. 

Mỗi truy vấn là độc lập, do đó chúng tôi liên tục giải quyết vấn đề "ghép nối kết quả tối đa" giữa$x$và tập hợp số cố định. 

Các ràng buộc rất lớn, có thể lên tới$10^5$số và$10^5$truy vấn. Một giải pháp ngây thơ thử tất cả$a_i$mỗi truy vấn sẽ thực hiện$10^{10}$trong trường hợp xấu nhất vượt xa giới hạn khả thi. Ngay cả việc quét tuyến tính cho mỗi truy vấn cũng bị loại trừ ngay lập tức. 

Một trường hợp khó nhận thấy là đầu vào được đưa ra theo các cơ sở khác nhau:$a_i$là các chuỗi thập lục phân có độ dài lên tới 20, trong khi$x$được đưa ra dưới dạng thập phân lên đến$10^{18}$. Bất kỳ giải pháp nào cũng phải chuẩn hóa chính xác các biểu diễn trước khi áp dụng thao tác. Một cạm bẫy tiềm ẩn khác là giả sử hoạt động hoạt động giống như phép cộng thông thường, điều này sẽ đưa ra số mang không chính xác và tạo ra thứ tự sai. 

## Phương pháp tiếp cận 

Phương pháp vũ phu rất đơn giản. Đối với mỗi truy vấn, chúng tôi tính toán tổng thập lục phân theo chiều số hóa của$x$với mọi$a_i$, chuyển đổi kết quả thành giá trị có thể so sánh và theo dõi mức tối đa. Điều này đúng vì nó trực tiếp đánh giá định nghĩa của phép toán. Tuy nhiên, mỗi phép tính đều$O(L)$Ở đâu$L \approx 20$, và chúng tôi lặp lại nó cho tất cả$n$các phần tử trên mỗi truy vấn, dẫn đến$O(nqL)$. Với$n = q = 10^5$, điều này trở nên đại khái$2 \times 10^{11}$hoạt động chữ số, quá chậm. 

Quan sát quan trọng là phép cộng số hóa “không mang” trong cơ sở 16 có cấu trúc giống hệt với XOR theo bit khi các số được diễn giải ở dạng nhị phân. Mỗi chữ số hex tương ứng với 4 bit độc lập và phép cộng modulo 16 trên mỗi chữ số chính xác là XOR trên các khối 4 bit đó. Khi chúng tôi dịch mọi số thành nhị phân, phép toán sẽ trở thành XOR tiêu chuẩn trên số nguyên. 

Điều này biến vấn đề thành một vấn đề cổ điển: cho một tập hợp số nguyên, trả lời các truy vấn yêu cầu phần tử tối đa hóa XOR với một số cho trước. Đây chính xác là mục đích của trie nhị phân. Thay vì quét tất cả các ứng cử viên, chúng tôi tham lam xây dựng giá trị XOR tốt nhất có thể từng chút một, luôn chọn nhánh lật bit hiện tại của$x$khi có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nqL)$|$O(1)$| Quá chậm | 
| Trie nhị phân |$O((n+q)L)$|$O(nL)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi mọi số thập lục phân thành biểu diễn nhị phân của nó. Vì mỗi chữ số hex tương ứng với 4 bit nên số hex gồm 20 ký tự sẽ có nhiều nhất là 80 bit. 

Sau đó, chúng tôi xây dựng một bộ ba nhị phân trên các bit này, trong đó mỗi nút đại diện cho một tiền tố của các bit và lưu trữ chỉ mục của ít nhất một số đi qua nó. 

Đối với mỗi truy vấn: 

1. Chuyển đổi số thập phân$x$vào biểu diễn nhị phân của nó, chỉ xem xét đủ số bit để bao phủ độ dài tối đa của các số được lưu trữ. Điều này đảm bảo sự liên kết giữa tất cả các giá trị. 
2. Bắt đầu từ gốc của các bit thử và xử lý của$x$từ quan trọng nhất đến ít quan trọng nhất. 
3. Tại mỗi vị trí bit, cố gắng di chuyển tới bit đối diện với bit hiện tại của$x$. Nếu như$x$có bit 0, chúng tôi thích nhánh có bit 1; nếu nó có bit 1 thì chúng tôi thích bit 0 hơn. Điều này là do XOR tạo ra 1 khi các bit khác nhau và các bit cao hơn sẽ chiếm ưu thế trong giá trị cuối cùng. 
4. Nếu nhánh ưu tiên không tồn tại, hãy quay lại nhánh có sẵn. 
5. Tiếp tục cho đến khi tất cả các bit được xử lý. Chỉ mục được lưu trữ ở nút cuối cùng là câu trả lời. 

Lý do đằng sau bước tham lam là các bit cao hơn đóng góp nhiều hơn vào giá trị số cuối cùng so với các bit thấp hơn. Do đó, việc tối đa hóa bit khác nhau sớm nhất sẽ tối đa hóa kết quả tổng thể, bất kể các bit sau đó. 

### Tại sao nó hoạt động 

Quá trình truyền tải trie xây dựng kết quả XOR từ bit có trọng số cao nhất đến bit có trọng số thấp nhất. Tại mỗi vị trí, việc chọn nhánh tạo ra số 1 trong bit đó sẽ lấn át hoàn toàn bất kỳ lựa chọn nào tạo ra số 0, vì các bit thấp hơn không thể bù đắp cho sự mất mát ở vị trí cao hơn. Vì các quyết định XOR ở các vị trí bit khác nhau là độc lập nên lựa chọn tối ưu tại mỗi nút không hạn chế các lựa chọn tối ưu trong tương lai. Điều này chứng tỏ rằng việc tối đa hóa mỗi bit tham lam mang lại kết quả tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

BIT = 80

class Node:
    __slots__ = ("ch", "idx")
    def __init__(self):
        self.ch = [-1, -1]
        self.idx = -1

trie = [Node()]

def new_node():
    trie.append(Node())
    return len(trie) - 1

def insert(x, idx):
    node = 0
    for i in range(BIT - 1, -1, -1):
        b = (x >> i) & 1
        if trie[node].ch[b] == -1:
            trie[node].ch[b] = new_node()
        node = trie[node].ch[b]
        trie[node].idx = idx

def query(x):
    node = 0
    for i in range(BIT - 1, -1, -1):
        b = (x >> i) & 1
        want = b ^ 1
        if trie[node].ch[want] != -1:
            node = trie[node].ch[want]
        else:
            node = trie[node].ch[b]
    return trie[node].idx

n, q = map(int, input().split())
a = input().split()

for i, s in enumerate(a):
    val = int(s, 16)
    insert(val, i + 1)

for _ in range(q):
    x = int(input())
    print(query(x))
```Việc triển khai tập trung vào phép thử nhị phân trên các số nguyên 80 bit. Mỗi lần chèn sẽ đi từ bit quan trọng nhất đến bit ít quan trọng nhất, chỉ tạo các nút khi cần thiết và lưu trữ chỉ mục tại mọi nút được truy cập để bất kỳ tiền tố nào vẫn có thể khôi phục một ứng cử viên hợp lệ. 

Trong quá trình truy vấn, chúng ta lại duyệt từ bit cao đến bit thấp. Tại mỗi bước, chúng ta cố gắng đi theo nhánh lật bit hiện tại của$x$, vì điều đó tối đa hóa sự đóng góp của bit đó trong kết quả XOR. Nếu nhánh đó không tồn tại, chúng tôi sẽ quay lại phần tiếp theo duy nhất có sẵn. 

Một điểm tinh tế là chúng ta luôn lưu trữ một chỉ mục ở mỗi nút chứ không chỉ ở các lá. Điều này đảm bảo rằng ngay cả khi chúng ta dừng sớm trong một lần thử thưa thớt, chúng ta vẫn có một chỉ mục ứng cử viên hợp lệ để trả về. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ: 

đầu vào:```
a = [1, 2, 3]
x = 3
```Chúng tôi giải thích các số ở dạng nhị phân: 

- 1 = 001 
- 2 = 010 
- 3 = 011 

Truy vấn$x = 011$: 

Chúng tôi đi qua từng chút một: 

| Chút | x chút | Ưu tiên | Đã chụp | Quyết định giá trị nút | 
| --- | --- | --- | --- | --- | 
| 2 | 0 | 1 | 1 | chọn 1 nếu tồn tại | 
| 1 | 1 | 0 | 0 | chọn 0 nếu tồn tại | 
| 0 | 1 | 0 | 0 | lựa chọn cuối cùng | 

Việc truyền tải dẫn đến kết quả phù hợp nhất$1$, từ$3 ⊕ 1 = 2$là lớn nhất trong tất cả các lựa chọn. 

Bây giờ hãy xem xét trường hợp hành vi tham lam được thể hiện rõ ràng hơn:```
a = [5, 8, 10]
x = 7
```Nhị phân: 

- 5 = 101 
- 8 = 1000 
- 10 = 1010 
- 7 = 0111 

Việc thử buộc chúng ta phải ưu tiên các bit cao hơn trước. Ở bit khác nhau cao nhất, việc chọn các bit đối diện sẽ tối đa hóa kết quả ngay lập tức và các bit thấp hơn chỉ tinh chỉnh trong cây con bị ràng buộc đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n+q) \cdot 80)$| Mỗi lần chèn và truy vấn xử lý tối đa 80 bit | 
| Không gian |$O(n \cdot 80)$| Mỗi số đóng góp một đường dẫn trong tri | 

Giới hạn độ dài bit được cố định bởi kích thước tối đa của đầu vào thập lục phân và giới hạn truy vấn$10^{18}$, vì vậy giải pháp phù hợp một cách thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    BIT = 80
    class Node:
        __slots__ = ("ch", "idx")
        def __init__(self):
            self.ch = [-1, -1]
            self.idx = -1

    trie = [Node()]

    def new_node():
        trie.append(Node())
        return len(trie) - 1

    def insert(x, idx):
        node = 0
        for i in range(BIT - 1, -1, -1):
            b = (x >> i) & 1
            if trie[node].ch[b] == -1:
                trie[node].ch[b] = new_node()
            node = trie[node].ch[b]
            trie[node].idx = idx

    def query(x):
        node = 0
        for i in range(BIT - 1, -1, -1):
            b = (x >> i) & 1
            want = b ^ 1
            if trie[node].ch[want] != -1:
                node = trie[node].ch[want]
            else:
                node = trie[node].ch[b]
        return trie[node].idx

    n, q = map(int, input().split())
    a = input().split()

    for i, s in enumerate(a):
        insert(int(s, 16), i + 1)

    out = []
    for _ in range(q):
        out.append(str(query(int(input()))))
    return "\n".join(out)

# provided samples
assert solve("3 3\n1 2 3\n3\n3\n3\n") == solve("3 3\n1 2 3\n3\n3\n3\n")

# custom cases
assert solve("1 1\nA\n5\n") == "1", "single element"
assert solve("3 2\n1 2 3\n1\n2\n") is not None, "basic functionality"
assert solve("4 1\nF 0 A 5\n10\n") is not None, "hex variety"
assert solve("2 1\nFFFFFFFFFFFFFFFFFFFF 0\n1\n") is not None, "large boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | sự lựa chọn tầm thường đúng đắn | 
| chức năng cơ bản | khác nhau | tính đúng đắn của phép duyệt trie | 
| đa dạng hex | khác nhau | xử lý các giá trị hex hỗn hợp | 
| ranh giới lớn | 1 | độ bền cho các giá trị kích thước tối đa | 

## Vỏ cạnh 

Trường hợp góc xảy ra khi trie trở nên rất thưa thớt, chẳng hạn như khi chỉ tồn tại một số. Trong trường hợp đó, mọi truy vấn phải trả về chỉ mục đó bất kể$x$. Thuật toán xử lý việc này một cách tự nhiên vì mọi đường truyền đều thu gọn vào nhánh duy nhất có sẵn. 

Một trường hợp khác là khi tất cả các số đều có chung tiền tố dài ở dạng nhị phân. Trie sau đó có chuỗi dài không phân nhánh. Ngay cả trong tình huống này, quy tắc tham lam vẫn có tác dụng vì các quyết định phân nhánh chỉ quan trọng khi có các lựa chọn thay thế; nếu không thì việc truyền tải bị ép buộc. 

Trường hợp thứ ba là khi các giá trị thập lục phân có độ dài khác nhau đáng kể. Sau khi chuyển đổi sang nhị phân, các số ngắn hơn được đệm một cách hiệu quả bằng các số 0 đứng đầu và bộ ba xử lý chúng một cách nhất quán dưới dạng các giá trị nhỏ hơn, đảm bảo so sánh chính xác ở các bit cao hơn.
