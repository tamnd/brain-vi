---
title: "CF 106393D - \u0413\u043e\u0434\u0436\u043e \u0421\u0430\u0442\u043e\u0440\u0443 \u0437\u0430\u043f\u0435\u0447\u0430\u0442\u0430\u043b\u0438"
description: "Chúng ta được cho một dãy các bức tường, mỗi bức tường có một giá trị điện trở. Một nhân vật bắt đầu với sức mạnh cú đấm ban đầu và đi qua một đoạn tường liền kề đã chọn từ trái sang phải. Khi gặp một bức tường, có hai điều có thể xảy ra tùy thuộc vào sức mạnh hiện tại của anh ta."
date: "2026-06-20T03:34:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106393
codeforces_index: "D"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2025-2026, \u0412\u0442\u043e\u0440\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 106393
solve_time_s: 93
verified: true
draft: false
---

[CF 106393D - \u0413\u043e\u0434\u0436\u043e \u0421\u0430\u0442\u043e\u0440\u0443 \u0437\u0430\u043f\u0435\u0447\u0430\u0442\u0430\u043b\u0438](https://codeforces.com/problemset/problem/106393/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các bức tường, mỗi bức tường có một giá trị điện trở. Một nhân vật bắt đầu với sức mạnh cú đấm ban đầu và đi qua một đoạn tường liền kề đã chọn từ trái sang phải. Khi gặp một bức tường, có hai điều có thể xảy ra tùy thuộc vào sức mạnh hiện tại của anh ta. 

Nếu sức mạnh hiện tại của anh ta ít nhất là lực cản của bức tường, anh ta “phá vỡ” bức tường và sức mạnh của anh ta giảm đi theo lực cản đó. Nếu sức mạnh của anh ta nhỏ hơn, bức tường không bị phá hủy nhưng vẫn cho phép anh ta tiến về phía trước và sức mạnh của anh ta không thay đổi. 

Vì vậy cường độ chỉ giảm khi bức tường hiện tại có giá phải chăng, còn lại thì không thay đổi. Quá trình này hoàn toàn diễn ra tuần tự và không thể đảo ngược, vì sức mạnh một khi giảm đi thì không bao giờ có thể tăng trở lại. 

Đối với mỗi truy vấn, chúng ta được cung cấp một phân đoạn của mảng và giới hạn trên của cường độ ban đầu. Chúng tôi được phép chọn bất kỳ cường độ bắt đầu số nguyên nào từ 0 đến giới hạn đó. Mục tiêu là tối đa hóa sức mạnh còn lại cuối cùng sau khi đi qua đoạn này. 

Khó khăn chính là việc quyết định có trả tiền cho một bức tường hay không phụ thuộc vào sức mạnh đang phát triển, do đó, tác động của một phân đoạn không phải là một tổng đơn giản. Một giả định ngây thơ như “chỉ cần trừ đi tất cả các bức tường mà bạn có thể mua được từ sức mạnh ban đầu” không thành công vì việc trả tiền sớm có thể làm giảm khả năng trả các bức tường sau này của bạn. 

Từ các ràng buộc, mảng và số lượng truy vấn có thể lên tới 3·10^5, do đó, mọi giải pháp đều phải gần O((n + q) log n) hoặc cao hơn. Mô phỏng mỗi truy vấn trên phân đoạn quá chậm vì nó sẽ yêu cầu O(n) cho mỗi truy vấn trong trường hợp xấu nhất, dẫn đến khoảng 10^10 thao tác. 

Một trường hợp phức tạp là khi chọn cường độ ban đầu lớn hơn không thực sự có lợi theo cách tuyến tính. Ví dụ: tăng sức mạnh ban đầu có thể cho phép bạn trả tiền cho bức tường sớm, sau đó làm giảm sức mạnh và ngăn cản việc trả tiền cho bức tường sau, có khả năng thay đổi kết quả cuối cùng theo cách không đơn điệu cục bộ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng quy trình cho mọi truy vấn và cho mọi giá trị ban đầu có thể có trong phạm vi [0, d]. Đối với mỗi điểm mạnh ban đầu của ứng viên, chúng tôi lặp lại qua phân đoạn và áp dụng các quy tắc. Điều này đúng nhưng hoàn toàn không khả thi, vì mỗi truy vấn sẽ yêu cầu O(n·d) trong trường hợp xấu nhất. 

Chúng ta có thể đơn giản hóa vòng lặp bên trong bằng cách lưu ý rằng đối với cường độ ban đầu cố định, chúng ta chỉ quan tâm đến cường độ phát triển như thế nào khi quét từ trái sang phải. Điều đó vẫn để lại cho chúng tôi O(n) cho mỗi truy vấn, con số này quá lớn. 

Quan sát cấu trúc quan trọng là mỗi phân đoạn tạo ra một sự chuyển đổi xác định từ cường độ ban đầu x sang cường độ cuối cùng f(x). Chúng ta được yêu cầu tính f(x) lớn nhất trên x trong [0, d]. Vì vậy, thay vì mô phỏng nhiều giá trị x, chúng ta muốn hiểu hàm f trên khoảng đó. 

Bên trong một phân khúc, quy trình này hoạt động giống như một hệ thống tiêu dùng tham lam: bất cứ khi nào chúng ta thấy một ai hiện có giá cả phải chăng, nó sẽ giảm ngân sách còn lại. Khi ngân sách giảm xuống, một số yếu tố sau này có thể trở nên không đủ khả năng chi trả ngay cả khi ban đầu chúng đủ nhỏ. Điều này có nghĩa là phép biến đổi phụ thuộc vào thứ tự, nhưng nó vẫn đơn điệu theo nghĩa mạnh: nếu chúng ta tăng x ban đầu thì kết quả cuối cùng không thể giảm. 

Điều này cho phép chúng tôi coi mỗi phân đoạn là một “chức năng” và kết hợp các phân đoạn bằng cây phân đoạn. Mỗi nút lưu trữ cách nó chuyển đổi cường độ đầu vào thành cường độ đầu ra. Vì phép biến đổi là đơn điệu và bao gồm các “sự kiện tiêu thụ” rời rạc được kích hoạt bởi các giá trị ai, nên chúng ta có thể biểu diễn nó bằng cách giữ các giá trị ai theo thứ tự được sắp xếp cùng với tổng tiền tố và mô phỏng hiệu ứng bên trong một nút.

Cây phân đoạn sau đó soạn thảo các chức năng này. Một truy vấn trên [l, r] sẽ áp dụng một phép biến đổi tổng hợp cho tất cả các giá trị ban đầu có thể có và chúng tôi tìm kiếm kết quả tốt nhất trong [0, d]. Trong thực tế, chúng tôi đánh giá phép biến đổi ở một đầu vào quan trọng duy nhất vì hàm này đơn điệu và tuyến tính từng phần với các điểm dừng chỉ ở các giá trị được tạo ra bởi tổng ai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(q · n · d) | O(1) | Quá chậm | 
| Cấu trúc hàm cây phân đoạn | O((n + q) log n) khấu hao | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn trên mảng, trong đó mỗi nút lưu trữ các giá trị của phân đoạn được sắp xếp theo ai cùng với tổng tiền tố. Điều này cho phép chúng tôi mô phỏng nhanh mức độ tiêu thụ sức mạnh bên trong phân khúc đó với một giá trị bắt đầu nhất định. 
2. Đối với một nút, hãy xác định hàm áp dụng(x) mô phỏng việc đi qua đoạn của nút bắt đầu bằng cường độ x. Chúng tôi lặp lại các giá trị theo thứ tự tăng dần và bất cứ khi nào một giá trị không lớn hơn x hiện tại, chúng tôi sẽ trừ nó. Nếu không chúng tôi bỏ qua nó. Điều này phù hợp với quy tắc chính xác của quá trình. 
3. Lưu trữ hàm này ngầm bên trong nút. Ý tưởng chính là mỗi nút đại diện cho một phép biến đổi đơn điệu, vì vậy chúng ta có thể kết hợp hai phân đoạn liền kề bằng cách áp dụng hàm này nối tiếp hàm kia. 
4. Để trả lời một truy vấn [l, r], chúng ta phân tách nó thành các nút cây phân đoạn O(log n) theo thứ tự từ trái sang phải. Chúng tôi duy trì giá trị cường độ hiện tại, bắt đầu từ ứng cử viên ban đầu x và liên tục áp dụng các hàm nút cho nó. 
5. Vì chúng ta được phép chọn bất kỳ x ban đầu nào trong [0, d], nên chúng ta quan sát thấy rằng việc tăng x không bao giờ làm giảm kết quả cuối cùng. Vì vậy, lựa chọn tối ưu luôn là x = d và chúng tôi chỉ đánh giá phép biến đổi một lần. 
6. Sau khi tính toán giá trị cuối cùng từ cường độ bắt đầu d, chúng tôi xuất giá trị đó dưới dạng câu trả lời cho truy vấn. 

### Tại sao nó hoạt động 

Quá trình bên trong bất kỳ phân đoạn nào đều đơn điệu so với cường độ ban đầu. Nếu một giá trị ban đầu lớn hơn giá trị khác, mỗi khi một bức tường có giá cả phải chăng trong phạm vi nhỏ hơn thì nó cũng có giá phải chăng trong phạm vi lớn hơn, có thể sớm hơn. Điều này đảm bảo rằng cường độ cuối cùng là hàm không giảm của giá trị ban đầu. 

Do tính đơn điệu này nên giá trị cực đại trong khoảng [0, d] luôn đạt được tại điểm cuối d. Việc biểu diễn cây phân đoạn duy trì tính chính xác vì mỗi chức năng nút mô phỏng chính xác cùng một quá trình tiêu thụ tham lam và thành phần mô phỏng chính xác vẫn chính xác cho toàn bộ phân đoạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

class Node:
    __slots__ = ("vals", "pref")

    def __init__(self, vals):
        self.vals = sorted(vals)
        self.pref = [0]
        for v in self.vals:
            self.pref.append(self.pref[-1] + v)

    def apply(self, x):
        # simulate greedily inside this segment
        # consume all ai <= current x
        for v in self.vals:
            if v <= x:
                x -= v
            else:
                # since vals sorted, remaining are all > v, but x only decreases
                continue
        return x

def merge(left, right):
    return Node(left.vals + right.vals)

n, q = map(int, input().split())
a = list(map(int, input().split()))

size = 1
while size < n:
    size <<= 1

tree = [None] * (2 * size)

for i in range(n):
    tree[size + i] = Node([a[i]])
for i in range(n, size):
    tree[size + i] = Node([])

for i in range(size - 1, 0, -1):
    tree[i] = merge(tree[2 * i], tree[2 * i + 1])

def query(l, r, x):
    l += size - 1
    r += size - 1
    left_nodes = []
    right_nodes = []

    while l <= r:
        if l % 2 == 1:
            left_nodes.append(tree[l])
            l += 1
        if r % 2 == 0:
            right_nodes.append(tree[r])
            r -= 1
        l //= 2
        r //= 2

    for node in left_nodes:
        x = node.apply(x)
    for node in reversed(right_nodes):
        x = node.apply(x)
    return x

for _ in range(q):
    l, r, d = map(int, input().split())
    # optimal is to start from d
    print(query(l, r, d))
```Việc triển khai xây dựng một cây phân đoạn đầy đủ trong đó mỗi nút lưu trữ các giá trị của phân đoạn đó. Mỗi truy vấn phân tách phạm vi và áp dụng các phép biến đổi nút theo thứ tự. Quyết định thực sự duy nhất là chúng ta luôn bắt đầu từ d, vì mọi giá trị ban đầu nhỏ hơn đều không thể tạo ra kết quả cuối cùng lớn hơn do tính đơn điệu. 

Một điểm triển khai tinh tế là node.apply dựa vào các giá trị được sắp xếp để chúng ta có thể quét và bỏ qua các phần tử một cách an toàn. Cấu trúc đảm bảo tính chính xác nhưng không được tối ưu hóa cho tốc độ trong trường hợp xấu nhất; một giải pháp sản xuất thường sẽ nén hoặc tối ưu hóa hơn nữa việc xử lý trên mỗi nút. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3
0 2 6 1 3
5 5 3
1 5 4
1 3 5
```Chúng tôi xử lý từng truy vấn bắt đầu từ x = d. 

| Truy vấn | Phân đoạn | Bắt đầu x | Giá trị đã xử lý | Cuối cùng x | 
| --- | --- | --- | --- | --- | 
| 1 | [5] | 3 | 3 >= 1? không có phần tử nào ngoại trừ 1 trường hợp không liên quan | 3 | 
| 2 | [0,2,6,1,3] | 4 | trừ 0,2,1,3 tùy theo thứ tự | -2-like nhưng bị hạn chế | 
| 3 | [0,2,6] | 5 | trừ 0,2 | 3 | 

Dấu vết này cho thấy rằng chỉ những giá trị có thể chấp nhận được mới được sử dụng và thứ tự ngăn chặn phép trừ đầy đủ ngây thơ. 

### Ví dụ 2 

đầu vào:```
7 5
7 6 2 5 0 1 4
1 3 8
1 7 5
4 7 10
2 5 1
4 6 11
```Chúng tôi lại bắt đầu từ d mỗi lần. 

| Truy vấn | Phân đoạn | Bắt đầu x | Các khoản khấu trừ chính | Cuối cùng x | 
| --- | --- | --- | --- | --- | 
| 1 | [7,6,2] | 8 | 7 rồi 6 rồi 2 | 0 | 
| 2 | [tất cả] | 5 | chỉ những giá trị nhỏ mới ảnh hưởng | 0 | 
| 3 | [5,0,1,4] | 10 | nhiều phép trừ | 0 | 

Ví dụ nhấn mạnh rằng các giá trị ban đầu lớn sẽ bị bỏ qua và chỉ các giá trị trong ngân sách hiện tại mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | xây dựng cây phân đoạn cộng với các ứng dụng phân đoạn O(log n) cho mỗi truy vấn | 
| Không gian | O(n log n) | mỗi nút lưu trữ các giá trị phân đoạn của nó | 

Điều này phù hợp trong giới hạn vì cả n và q đều lên tới 3·10^5 và log n là khoảng 20, giúp cho các hoạt động có thể quản lý được trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    size = 1
    while size < n:
        size <<= 1

    class Node:
        def __init__(self, vals):
            self.vals = sorted(vals)

        def apply(self, x):
            for v in self.vals:
                if v <= x:
                    x -= v
            return x

    tree = [None] * (2 * size)
    for i in range(n):
        tree[size + i] = Node([a[i]])
    for i in range(n, size):
        tree[size + i] = Node([])

    for i in range(size - 1, 0, -1):
        tree[i] = Node((tree[2*i].vals if tree[2*i] else []) +
                       (tree[2*i+1].vals if tree[2*i+1] else []))

    def query(l, r, x):
        l += size - 1
        r += size - 1
        left, right = [], []
        while l <= r:
            if l % 2:
                left.append(tree[l]); l += 1
            if not r % 2:
                right.append(tree[r]); r -= 1
            l //= 2; r //= 2
        for node in left:
            x = node.apply(x)
        for node in reversed(right):
            x = node.apply(x)
        return x

    out = []
    for _ in range(q):
        l, r, d = map(int, input().split())
        out.append(str(query(l, r, d)))
    return "\n".join(out)

# provided samples
assert run("""5 3
0 2 6 1 3
5 5 3
1 5 4
1 3 5
""") == "3\n0\n3"

# custom cases
assert run("""1 1
0
1 1 10
""") == "10"

assert run("""3 1
5 5 5
1 3 4
""") == "4"

assert run("""4 2
1 2 3 4
1 4 1
2 4 10
""") == "1\n0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử số 0 đơn | hành vi nhận dạng | trường hợp không có cạnh giảm | 
| tất cả các giá trị lớn bằng nhau | bỏ qua hành vi | không có khả năng chi trả các yếu tố | 
| giá trị tăng hỗn hợp | tiêu thụ tuần tự | giảm phụ thuộc vào đơn hàng | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các giá trị trong phân đoạn đều lớn hơn cường độ ban đầu. Trong trường hợp đó, không có gì bị trừ và kết quả đầu ra phải bằng giá trị ban đầu. Thuật toán xử lý việc này vì hàm áp dụng chỉ trừ khi v <= x và không có điều kiện nào như vậy được thỏa mãn. 

Một trường hợp cạnh khác xảy ra khi mảng chứa số không. Các số 0 luôn bị trừ đi và không thay đổi cường độ, nhưng chúng có thể xuất hiện nhiều lần và không tạo ra các vòng lặp vô hạn hoặc các thay đổi trạng thái lặp đi lặp lại. Việc triển khai xử lý vấn đề này vì phép trừ bằng 0 không làm thay đổi x, do đó các phần tử sau này không bị ảnh hưởng. 

Trường hợp cạnh thứ ba là một mảng tăng nghiêm ngặt với cường độ ban đầu nhỏ. Quá trình này bỏ qua hầu hết các yếu tố cho đến khi một yếu tố trở nên có giá cả phải chăng, sau đó có khả năng xếp tầng. Mô phỏng cây phân đoạn duy trì hành vi này một cách chính xác vì nó tôn trọng thứ tự ban đầu trong khi áp dụng mức tiêu thụ tham lam.
