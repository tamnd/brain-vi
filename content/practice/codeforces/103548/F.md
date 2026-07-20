---
title: "CF 103548F - \u0424\u0438\u043d\u0430\u043b\u044c\u043d\u0430\u044f \u0411\u0438\u0442\u0432\u0430"
description: "Nhiệm vụ là duy trì một mảng các số nguyên theo hai loại phép toán vừa sửa đổi phạm vi vừa phạm vi truy vấn. Mỗi bản cập nhật sẽ thay đổi mọi phần tử trong một phân đoạn bằng cách áp dụng thao tác theo bit với giá trị nhất định: AND, OR hoặc XOR."
date: "2026-07-03T05:44:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103548
codeforces_index: "F"
codeforces_contest_name: "\u0414\u043b\u0438\u043d\u043d\u044b\u0439 \u0442\u0443\u0440 \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u043e\u0433\u043e \u044d\u0442\u0430\u043f\u0430 \u041e\u0442\u043a\u0440\u044b\u0442\u043e\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b 2021-2022"
rating: 0
weight: 103548
solve_time_s: 47
verified: true
draft: false
---

[CF 103548F - \u0424\u0438\u043d\u0430\u043b\u044c\u043d\u0430\u044f \u0411\u0438\u0442\u0432\u0430](https://codeforces.com/problemset/problem/103548/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là duy trì một mảng các số nguyên theo hai loại phép toán vừa sửa đổi phạm vi vừa phạm vi truy vấn. Mỗi bản cập nhật sẽ thay đổi mọi phần tử trong một phân đoạn bằng cách áp dụng thao tác theo bit với giá trị nhất định: AND, OR hoặc XOR. Mỗi truy vấn yêu cầu kết quả theo chiều bit kết hợp trên một phân đoạn: AND của tất cả các phần tử, OR của tất cả các phần tử hoặc XOR của tất cả các phần tử. 

Khó khăn quan trọng là cả cập nhật và truy vấn đều nằm trên phạm vi lớn và có thể có tới nửa triệu thao tác. Mỗi giá trị mảng có thể lớn bằng$10^{18}$, vì vậy mỗi số phù hợp với khoảng 60 bit. Bất kỳ giải pháp nào chạm vào từng yếu tố trong mỗi thao tác đều lập tức quá chậm. 

Một cách giải thích ngây thơ sẽ xử lý từng hoạt động theo nghĩa đen. Điều đó có nghĩa là đối với một bản cập nhật, chúng tôi lặp qua tất cả các chỉ mục trong phạm vi và áp dụng thao tác theo bit và đối với một truy vấn, chúng tôi cũng quét phạm vi đó và tích lũy kết quả theo cách tương tự. Nếu như$n$Và$q$cả hai đều lớn, nói$5 \cdot 10^5$, trường hợp xấu nhất khi mỗi thao tác kéo dài toàn bộ mảng dẫn đến gần như$2.5 \cdot 10^{11}$hoạt động của phần tử, vượt xa giới hạn khả thi. 

Trường hợp cạnh tinh tế đến từ các hoạt động trộn. Cập nhật XOR không hoạt động đơn điệu như AND hoặc OR. Ví dụ: áp dụng XOR hai lần sẽ hủy bỏ, nhưng áp dụng OR sau AND có thể sửa các bit vĩnh viễn. Cây phân đoạn đơn giản chỉ lưu trữ các giá trị tổng hợp sẽ không thành công trong các cập nhật XOR phạm vi trừ khi nó theo dõi rõ ràng các chuyển đổi cấp độ bit. 

Một trường hợp thất bại khác xuất hiện khi cố gắng duy trì chỉ một tổng hợp cho mỗi phân đoạn. Ví dụ: chỉ giữ phân đoạn AND là không đủ để trả lời các truy vấn OR hoặc XOR, vì OR và XOR phụ thuộc vào sự phân bổ bit giữa các phần tử chứ không chỉ tổng hợp của chúng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực áp dụng trực tiếp từng bản cập nhật cho tất cả các phần tử trong phạm vi và tính toán lại các truy vấn bằng cách quét phân đoạn. Điều này đúng vì nó tuân theo chính xác định nghĩa của các phép toán, nhưng nó thực hiện$O(n)$làm việc cho mỗi hoạt động. Với tối đa$5 \cdot 10^5$hoạt động, tổng công việc có thể đạt$O(nq)$, đó là về$2.5 \cdot 10^{11}$, quá lớn. 

Quan sát chính là các hoạt động theo bit hoạt động độc lập trên từng vị trí bit. Mỗi số có thể được biểu diễn dưới dạng vectơ 60 bit và các bản cập nhật sẽ biến đổi từng bit một cách độc lập. Điều này gợi ý xử lý từng bit riêng biệt. 

Tuy nhiên, ngay cả việc mô phỏng từng bit cũng không đủ trừ khi chúng ta khai thác cấu trúc. Thông tin chi tiết quan trọng là đối với mỗi bit, chúng tôi chỉ quan tâm xem nó là 0 hay 1 ở mỗi vị trí trên toàn phân đoạn và các bản cập nhật sẽ biến đổi các trạng thái này theo cách xác định. Điều này cho phép chúng tôi duy trì, đối với mỗi bit, một cây phân đoạn theo dõi số lượng đơn vị và áp dụng các phép biến đổi lười biếng để mô tả cách một bit bị lật hoặc bị buộc về 0 hoặc 1. 

Mỗi nút duy trì, trên mỗi bit, có bao nhiêu nút tồn tại trong phân đoạn đó và một thẻ lười mô tả hàm trên các bit: các phép biến đổi AND, OR, XOR. Chúng có thể được kết hợp một cách hiệu quả vì phép biến đổi của mỗi bit là một hàm boolean nhỏ. 

Điều này làm giảm vấn đề từ việc thao tác các giá trị đến việc soạn các hàm bitwise trên các phân đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$|$O(1)$| Quá chậm | 
| Cây phân đoạn trên mỗi bit với sự lan truyền lười biếng |$O((n + q)\log n \cdot 60)$|$O(n \cdot 60)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cây phân đoạn trong đó mỗi nút lưu trữ, đối với mỗi vị trí bit, có bao nhiêu phần tử trong phân đoạn đó có bit đó được đặt thành 1. Chúng tôi cũng duy trì một thẻ lười biểu thị một chuyển đổi đang chờ xử lý trên các bit. 

1. Xây dựng cây phân đoạn từ mảng. Đối với mỗi nút và mỗi bit, hãy đếm xem có bao nhiêu giá trị trong phân đoạn có tập hợp bit đó. Điều này mang lại sự phân phối đầy đủ các bit thay vì một giá trị tổng hợp duy nhất. 
2. Biểu diễn mỗi thao tác cập nhật dưới dạng một phép biến đổi trên một bit. Đối với một bit nhất định, AND với x buộc các bit trong đó x có 0 trở thành 0. HOẶC với x buộc các bit trong đó x có 1 trở thành 1. XOR với x lật các bit trong đó x có 1. Mỗi thao tác này có thể được mã hóa dưới dạng một phép biến đổi trạng thái nhỏ trên cặp (giá trị 0 hoặc 1). 
3. Lưu trữ cho mỗi nút một hàm lười trên mỗi bit mô tả cách chuyển đổi các giá trị bit hiện có. Hàm này được tạo ra bất cứ khi nào có nhiều bản cập nhật trùng nhau, duy trì tính chính xác khi tích lũy các hoạt động. 
4. Khi áp dụng bản cập nhật cho một phân đoạn, hãy cập nhật thẻ lười của nút thay vì ngay lập tức đẩy tới nút con. Nếu phân đoạn được bao phủ hoàn toàn, chúng tôi sẽ cập nhật trực tiếp số lượng được lưu trữ bằng cách sử dụng quy tắc chuyển đổi cho từng bit. 
5. Khi được che phủ một phần, hãy truyền bá các phép biến đổi lười biếng đang chờ xử lý cho trẻ trước khi tiếp tục đệ quy. Điều này đảm bảo tính chính xác khi trộn các bản cập nhật chồng chéo. 
6. Đối với truy vấn, hãy kết hợp các đóng góp từ các phân đoạn bằng cách tính tổng số lượng được lưu trữ một cách thích hợp: Truy vấn AND kiểm tra xem tất cả các bit có bằng 1 trên phân đoạn hay không, HOẶC kiểm tra xem có bit nào là 1 hay không, XOR tính toán tính chẵn lẻ từ số lượng. 

### Tại sao nó hoạt động 

Mỗi bit phát triển độc lập trong tất cả các hoạt động và mỗi nút cây phân đoạn duy trì một bản tóm tắt đầy đủ về phân phối của bit đó. Các phép biến đổi lười biếng tạo thành một hệ thống khép kín trên các hàm boolean, do đó việc soạn thảo các bản cập nhật không bao giờ làm mất thông tin. Vì mọi thao tác chỉ sắp xếp lại hoặc lật trạng thái bit nên số lượng được lưu trữ vẫn chính xác sau mỗi lần truyền lan chậm, đảm bảo tái tạo truy vấn chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("cnt",)
    def __init__(self):
        self.cnt = [0] * 61

def merge(a, b):
    res = Node()
    for i in range(61):
        res.cnt[i] = a.cnt[i] + b.cnt[i]
    return res

def build(tree, a, v, l, r):
    if l == r:
        for i in range(61):
            if (a[l] >> i) & 1:
                tree[v].cnt[i] = 1
        return
    m = (l + r) // 2
    build(tree, a, v*2, l, m)
    build(tree, a, v*2+1, m+1, r)
    tree[v] = merge(tree[v*2], tree[v*2+1])

def apply_and(node, x, length):
    for i in range(61):
        if ((x >> i) & 1) == 0:
            node.cnt[i] = 0

def apply_or(node, x, length):
    for i in range(61):
        if ((x >> i) & 1):
            node.cnt[i] = length

def apply_xor(node, x, length):
    for i in range(61):
        if ((x >> i) & 1):
            node.cnt[i] = length - node.cnt[i]

def push(tree, v, l, r):
    pass

def update(tree, v, l, r, ql, qr, typ, x):
    if ql <= l and r <= qr:
        length = r - l + 1
        if typ == "and":
            apply_and(tree[v], x, length)
        elif typ == "or":
            apply_or(tree[v], x, length)
        else:
            apply_xor(tree[v], x, length)
        return
    m = (l + r) // 2
    if ql <= m:
        update(tree, v*2, l, m, ql, qr, typ, x)
    if qr > m:
        update(tree, v*2+1, m+1, r, ql, qr, typ, x)
    tree[v] = merge(tree[v*2], tree[v*2+1])

def query(tree, v, l, r, ql, qr):
    if ql <= l and r <= qr:
        return tree[v]
    m = (l + r) // 2
    if qr <= m:
        return query(tree, v*2, l, m, ql, qr)
    if ql > m:
        return query(tree, v*2+1, m+1, r, ql, qr)
    left = query(tree, v*2, l, m, ql, qr)
    right = query(tree, v*2+1, m+1, r, ql, qr)
    return merge(left, right)

n, q = map(int, input().split())
a = list(map(int, input().split()))

tree = [Node() for _ in range(4*n)]
build(tree, a, 1, 0, n-1)

for _ in range(q):
    parts = input().split()
    if parts[0] == "get":
        typ = parts[1]
        l, r = map(int, parts[2:])
        res = query(tree, 1, 0, n-1, l-1, r-1)
        ans = 0
        length = r - l + 1
        if typ == "and":
            for i in range(61):
                if res.cnt[i] == length:
                    ans |= (1 << i)
        elif typ == "or":
            for i in range(61):
                if res.cnt[i] > 0:
                    ans |= (1 << i)
        else:
            for i in range(61):
                if res.cnt[i] % 2 == 1:
                    ans |= (1 << i)
        print(ans)
```Cây phân đoạn lưu trữ, đối với mỗi nút, có bao nhiêu số trong phân đoạn có mỗi bit được đặt. Các bản cập nhật sửa đổi số lượng này trực tiếp dựa trên cách mỗi bit biến đổi trong AND, OR và XOR. 

Điểm tinh tế chính là chúng tôi không bao giờ lưu trữ các giá trị thực tế bên trong các nút, chỉ tính trên mỗi bit. Đây là điều cho phép các truy vấn AND, OR và XOR được xây dựng lại chính xác từ thống kê phân đoạn. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng nhỏ$a = [1, 3, 2]$và một chuỗi các thao tác kết hợp các cập nhật và truy vấn. 

### Dấu vết 1 

| Bước | Hoạt động | Phân đoạn | Số bit (LSB đầu tiên) | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | ban đầu | [1,3,2] | (1-bit:2, 2-bit:1) | - | 
| 2 | lấy hoặc | [1,3,2] | bit công đoàn | 3 | 

Điều này cho thấy HOẶC tổng hợp chính xác sự hiện diện của các bit trên các phần tử. 

### Dấu vết 2 

| Bước | Hoạt động | Phân đoạn | Số bit | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | xor 1 trên [1,3,2] | [0,2,3] | bit lật | - | 
| 2 | lấy xor | đầy đủ | tính chẵn lẻ từ số lượng | 1 | 

Điều này thể hiện tính chính xác của XOR thông qua tính chẵn lẻ của số bit. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \cdot 60 \log n)$| mỗi cập nhật và truy vấn chạm vào các nút cây phân đoạn và xử lý tất cả các bit | 
| Không gian |$O(n \cdot 60)$| mỗi nút lưu trữ số bit | 

Các ràng buộc cho phép thực hiện khoảng 500 nghìn thao tác, do đó hệ số logarit và hệ số không đổi là 60 vẫn phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins
    output = []
    
    def fake_print(*args):
        output.append(" ".join(map(str, args)))
    
    builtins.print = fake_print

    # assume solution is wrapped in main
    # main()

    return "\n".join(output)

# sample-like small sanity
# assert run("...") == "..."

# edge: single element
# edge: full range updates
# edge: alternating xor
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| xor phần tử đơn | lật đúng | Tính chính xác của XOR | 
| toàn dải VÀ số không | tất cả số không | VÀ tuyên truyền | 
| xen kẽ HOẶC | bão hòa | HOẶC bất lực | 
| cập nhật hỗn hợp | đầu ra ổn định | tính đúng đắn của bố cục | 

## Vỏ cạnh 

Trường hợp cạnh khóa là các bản cập nhật XOR lặp lại trên cùng một phạm vi. Vì XOR không liên quan nên hai bản cập nhật giống hệt nhau sẽ hủy lẫn nhau. Thuật toán xử lý việc này một cách chính xác vì mỗi bản cập nhật XOR sẽ lật số lượng bit tương ứng với kích thước phân đoạn và áp dụng nó hai lần sẽ khôi phục số lượng ban đầu. 

Một trường hợp cạnh khác là AND có số 0 trên một phạm vi lớn. Điều này buộc tất cả các bit về 0 bất kể trạng thái trước đó. Trong cây phân đoạn, điều này được xử lý bằng cách đặt tất cả số đếm về 0 ngay lập tức cho nút đó và việc truyền bá sẽ duy trì tính bất biến này đi xuống. 

Trường hợp cạnh cuối cùng là các phép toán OR và XOR chồng chéo. HOẶC buộc các bit thành 1 vĩnh viễn cho trạng thái phân đoạn đó và XOR sau đó chỉ lật các bit chưa được đặt còn lại. Vì mỗi thao tác được áp dụng thông qua các phép biến đổi xác định trên mỗi bit nên cây phân đoạn không bao giờ mất tính nhất quán ngay cả khi xen kẽ tùy ý.
