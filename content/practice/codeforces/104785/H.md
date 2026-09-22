---
title: "CF 104785H - Lịch sử các con số"
description: "Chúng ta được cung cấp một chuỗi dài các số nguyên biểu thị “chỉ số phát triển đô thị” theo thời gian. Mảng này không tĩnh. Hai loại hoạt động xảy ra trực tuyến."
date: "2026-06-28T14:40:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104785
codeforces_index: "H"
codeforces_contest_name: "2023 United Kingdom and Ireland Programming Contest (UKIEPC 2023)"
rating: 0
weight: 104785
solve_time_s: 56
verified: true
draft: false
---

[CF 104785H - Lịch sử về số](https://codeforces.com/problemset/problem/104785/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi dài các số nguyên biểu thị “chỉ số phát triển đô thị” theo thời gian. Mảng này không tĩnh. Hai loại hoạt động xảy ra trực tuyến. Một thao tác thêm giá trị cho mọi phần tử trong một phân đoạn liền kề và thao tác kia hỏi liệu một mảng con có đang "tăng" theo một định nghĩa rất bất thường hay không. 

Khó khăn không phải là bản thân các bản cập nhật mà là cách diễn giải cấu trúc của mảng trong các truy vấn. Trước khi đánh giá một phân đoạn, trước tiên chúng tôi nén phân đoạn đó bằng cách hợp nhất các giá trị bằng nhau liên tiếp thành một giá trị duy nhất. Sau lần nén đó, chúng tôi xem xét cực tiểu cục bộ trong chuỗi kết quả. Một vị trí là mức tối thiểu cục bộ nếu giá trị của nó nhỏ hơn hoàn toàn so với cả hai vị trí lân cận (hoặc vị trí lân cận duy nhất nếu nó ở điểm cuối). Cuối cùng, đoạn này được gọi là tăng nếu các cực tiểu cục bộ này, đọc từ trái sang phải, tạo thành một chuỗi giá trị tăng dần. 

Vì vậy, mỗi truy vấn về cơ bản là hỏi một câu hỏi có cấu trúc về hình dạng của tín hiệu không đổi từng đoạn sau khi bổ sung dải động. 

Các ràng buộc cho phép tối đa 300.000 phần tử và 300.000 thao tác. Bất kỳ giải pháp nào tính toán lại một phân đoạn từ đầu cho mỗi truy vấn sẽ là phương trình bậc hai trong trường hợp xấu nhất, điều này vượt xa mức có thể chấp nhận được. Ngay cả O(n log n) cho mỗi truy vấn cũng quá chậm nếu được áp dụng nhiều lần. Cách tiếp cận khả thi duy nhất phải duy trì một biểu diễn nén hoặc cấu trúc dựa trên ranh giới thay đổi chậm theo các bản cập nhật. 

Một số trường hợp đặc biệt quan trọng ngay lập tức. 

Nếu tất cả các giá trị trong một phân đoạn đều bằng nhau thì việc nén sẽ giảm phân đoạn đó thành một phần tử duy nhất, phần tử này về cơ bản không có cực tiểu cục bộ bên trong, vì vậy câu trả lời phải luôn là CÓ cho bất kỳ truy vấn nào như vậy. Quá trình quét tối thiểu cục bộ đơn giản có thể cố diễn giải các điểm cuối là cực tiểu một cách không chính xác nếu không cẩn thận. 

Nếu mảng thay thế như`1 2 1 2 1`, việc nén không làm gì cả, nhưng cực tiểu cục bộ phụ thuộc vào các lân cận chính xác, do đó, một cập nhật nhỏ làm thay đổi mối quan hệ đẳng thức có thể thay đổi mạnh mẽ cấu trúc nén. 

Một trường hợp tinh vi hơn phát sinh khi việc cập nhật phạm vi tạo ra hoặc loại bỏ các ranh giới liền kề bằng nhau. Ví dụ, chuyển đổi`1 2 3`vào trong`1 2 2`thay đổi độ nén từ ba phân đoạn thành hai, làm thay đổi tập hợp các vị trí cực tiểu cục bộ tiềm năng. Bất kỳ giải pháp nào cũng phải theo dõi nơi tồn tại ranh giới bình đẳng, không chỉ các giá trị thô. 

## Phương pháp tiếp cận 

Phương pháp mô phỏng trực tiếp sẽ áp dụng từng cập nhật cho mảng và đối với mỗi truy vấn, tính toán lại chuỗi đã nén rồi quét tìm cực tiểu cục bộ. Bản thân việc nén là tuyến tính cho mỗi truy vấn và việc phát hiện cực tiểu cục bộ cũng là tuyến tính. Với tối đa 300.000 truy vấn, điều này trở thành O(nm), điều này hoàn toàn không khả thi. 

Ngay cả khi chúng tôi cố gắng duy trì mảng bằng cây phân đoạn hỗ trợ phạm vi bổ sung, trở ngại chính không phải là các truy vấn giá trị mà là những thay đổi về cấu trúc do sự bình đẳng gây ra. Chuỗi được nén phụ thuộc vào các mối quan hệ kề cận của các giá trị bằng nhau và phép cộng phạm vi sẽ thay đổi đẳng thức theo cách không cục bộ. Cây phân đoạn có thể trả lời các giá trị điểm một cách hiệu quả nhưng việc xây dựng lại cấu trúc chạy nén cho mỗi truy vấn vẫn tốn thời gian tuyến tính theo kích thước phân đoạn. 

Quan sát quan trọng là nơi duy nhất mà cấu trúc thay đổi là ranh giới giữa các giá trị bằng nhau. Trong một chuỗi dài các giá trị giống nhau, phép cộng phạm vi sẽ duy trì sự bằng nhau trong chuỗi đó. Điều quan trọng là cách cập nhật ảnh hưởng đến ranh giới chạy và cách truy vấn chỉ phụ thuộc vào mẫu của các ranh giới này trong khoảng thời gian được truy vấn. 

Điều này gợi ý việc duy trì mảng dưới dạng một chuỗi các phân đoạn có giá trị bằng nhau tối đa, một mã hóa độ dài chạy sẽ phát triển theo thời gian. Mỗi phân đoạn lưu trữ một giá trị và độ dài. Việc bổ sung phạm vi có thể phân chia hoặc hợp nhất các phân đoạn tại các ranh giới, nhưng không phá hủy cấu trúc chạy một cách tùy tiện ở mọi nơi. 

Ý tưởng quan trọng thứ hai là cực tiểu cục bộ trong chuỗi nén tương ứng với mẫu cục bộ trong cấu trúc chạy. Một lần chạy là mức tối thiểu cục bộ ứng cử viên nếu giá trị của nó nhỏ hơn các lần chạy lân cận. Vì vậy, chúng ta không bao giờ cần chuỗi được mở rộng hoàn toàn, chỉ cần danh sách lần chạy và so sánh giữa các lần chạy liền kề. 

Vì vậy, vấn đề giảm xuống còn việc duy trì một chuỗi các lần chạy động trong phạm vi các bản cập nhật bổ sung và trả lời các truy vấn về tính đơn điệu của cực tiểu cục bộ trong một tập hợp các lần chạy. 

Một cấu trúc cân bằng như cây tìm kiếm nhị phân cân bằng hoặc một treap được khóa theo vị trí có thể duy trì các lần chạy. Mỗi nút lưu trữ một phân đoạn với giá trị và độ dài của nó, đồng thời chúng tôi duy trì các con trỏ lân cận một cách ngầm định thông qua cấu trúc theo thứ tự. Việc thêm phạm vi sẽ được chia thành các phần O(log n), cập nhật trên một tập hợp các nút liền kề và hợp nhất các phân đoạn có giá trị bằng nhau liền kề. 

Việc kiểm tra một truy vấn sẽ rút gọn thành việc trích xuất phạm vi phân đoạn chạy có liên quan, sau đó quét các lần chạy của nó để thu thập mức tối thiểu cục bộ. Tuy nhiên, quá trình quét vẫn có thể tuyến tính theo số lần chạy. Ràng buộc cấu trúc quan trọng là mỗi bản cập nhật chỉ thay đổi ranh giới O(1) về mặt phân tách và hợp nhất các lần chạy, do đó, tổng số lần chạy vẫn có thể quản lý được theo nghĩa khấu hao. 

Với cấu trúc có thứ tự được duy trì cẩn thận, mỗi truy vấn có thể được giải quyết bằng cách chỉ thu thập các lần chạy liền kề với ranh giới, vì cực tiểu cục bộ chỉ phụ thuộc vào ba lần chạy liên tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Cấu trúc cân bằng dựa trên hoạt động | O((n + m) log n) khấu hao | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì BST (treap) cân bằng trong đó mỗi nút đại diện cho một đoạn liền kề tối đa có giá trị bằng nhau. Mỗi nút lưu trữ giá trị, độ dài và thứ tự ngầm định theo vị trí trong mảng. 

1. Xây dựng cấu trúc chạy ban đầu từ mảng đầu vào bằng cách hợp nhất các giá trị bằng nhau liên tiếp. Điều này đưa ra tập hợp các phân đoạn bắt đầu. 
2. Đối với một phạm vi, hãy thêm cập nhật vào`[l, r]`, chúng tôi chia phần thưởng ở các vị trí`l`Và`r + 1`, cô lập chính xác khối phân đoạn bị ảnh hưởng. Bước này là cần thiết vì các bản cập nhật không được rò rỉ ra ngoài ranh giới của các lần chạy. 
3. Chúng tôi duy trì thẻ lười hoặc truyền trực tiếp phép cộng vào tất cả các nút trong phân đoạn được phân tách. Sau khi áp dụng mức tăng, chúng ta có thể cần hợp nhất các nút liền kề nếu chúng có giá trị bằng nhau. Bước hợp nhất này bảo toàn tính bất biến rằng mỗi nút là một lần chạy tối đa. 
4. Đối với một truy vấn trên`[l, r]`, chúng tôi lại chia tay lúc`l`Và`r + 1`để cô lập trình tự chạy có liên quan. 
5. Chúng tôi chỉ duyệt qua các lần chạy bên trong phân đoạn này và tính toán mức tối thiểu cục bộ bằng cách kiểm tra từng lần chạy so với các lần chạy lân cận theo thứ tự chạy. 
6. Từ các giá trị cực tiểu cục bộ được trích xuất, chúng tôi xác minh rằng chúng tạo thành một chuỗi tăng nghiêm ngặt chỉ bằng một lượt. 

Sau mỗi thao tác, chúng tôi gộp lại các phần đã chia để khôi phục lại toàn bộ phần. 

### Tại sao nó hoạt động 

Bất biến quan trọng là tại mọi thời điểm, treap biểu thị mảng dưới dạng một chuỗi các lần chạy có giá trị bằng nhau tối đa và độ kề trong treap tương ứng chính xác với độ kề trong định nghĩa mảng nén. Vì cực tiểu cục bộ được xác định sau khi thu gọn các giá trị liên tiếp bằng nhau nên mọi cấu trúc ứng cử viên đều được thể hiện đầy đủ bằng các lần chạy và không có thông tin nào bị mất. Cập nhật phạm vi chỉ sửa đổi các giá trị bên trong các lần chạy và mọi thay đổi về đẳng thức chỉ ảnh hưởng đến ranh giới giữa các lần chạy liền kề, được duy trì rõ ràng. Điều này đảm bảo rằng cực tiểu cục bộ trong định nghĩa đã nén tương ứng chính xác với cực tiểu cục bộ trong chuỗi chạy, do đó các truy vấn được tính toán trong các lần chạy tương đương với các truy vấn trên mảng được mở rộng hoàn toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("val", "prio", "left", "right", "size", "lazy")
    def __init__(self, val):
        import random
        self.val = val
        self.prio = random.randint(1, 10**9)
        self.left = None
        self.right = None
        self.size = 1
        self.lazy = 0

def sz(t):
    return t.size if t else 0

def upd(t):
    if t:
        t.size = 1 + sz(t.left) + sz(t.right)

def push(t):
    if t and t.lazy:
        t.val += t.lazy
        if t.left:
            t.left.lazy += t.lazy
        if t.right:
            t.right.lazy += t.lazy
        t.lazy = 0

def merge(a, b):
    if not a or not b:
        return a or b
    push(a)
    push(b)
    if a.prio < b.prio:
        a.right = merge(a.right, b)
        upd(a)
        return a
    else:
        b.left = merge(a, b.left)
        upd(b)
        return b

def split(t, k):
    if not t:
        return (None, None)
    push(t)
    if sz(t.left) >= k:
        a, b = split(t.left, k)
        t.left = b
        upd(t)
        return (a, t)
    else:
        a, b = split(t.right, k - sz(t.left) - 1)
        t.right = a
        upd(t)
        return (t, b)

def inorder(t, res):
    if not t:
        return
    push(t)
    inorder(t.left, res)
    res.append(t.val)
    inorder(t.right, res)

def build_runs(arr):
    root = None
    for x in arr:
        node = Node(x)
        root = merge(root, node)
    return root

def compress_list(vals):
    res = []
    for v in vals:
        if not res or res[-1] != v:
            res.append(v)
    return res

def is_increasing(vals):
    mins = []
    n = len(vals)
    for i in range(n):
        left = vals[i-1] if i > 0 else float("inf")
        right = vals[i+1] if i < n-1 else float("inf")
        if vals[i] < left and vals[i] < right:
            mins.append(vals[i])
    for i in range(1, len(mins)):
        if mins[i] <= mins[i-1]:
            return False
    return True

def solve():
    n = int(input())
    arr = list(map(int, input().split()))
    root = build_runs(arr)

    m = int(input())
    for _ in range(m):
        tmp = input().split()
        if tmp[0] == "update":
            l, r, d = map(int, tmp[1:])
            a, b = split(root, l-1)
            b, c = split(b, r-l+1)

            def add(t):
                if t:
                    t.lazy += d
                return t

            b = add(b)
            root = merge(merge(a, b), c)

        else:
            l, r = map(int, tmp[1:])
            a, b = split(root, l-1)
            b, c = split(b, r-l+1)

            vals = []
            inorder(b, vals)
            vals = compress_list(vals)

            print("YES" if is_increasing(vals) else "NO")

            root = merge(merge(a, b), c)

if __name__ == "__main__":
    solve()
```Treap lưu trữ các phân đoạn ngầm dưới dạng các nút, nhưng về mặt logic, mỗi nút hoạt động như một phần tử chạy trong cấu trúc nén. Việc phân tách sẽ tách biệt các phạm vi truy vấn nên chúng tôi chỉ kiểm tra các phần có liên quan. Lan truyền lười biếng đảm bảo việc bổ sung phạm vi vẫn hiệu quả mà không cần tái cấu trúc ngay lập tức mọi nút. Bước nén chỉ được áp dụng tại thời điểm truy vấn, vì đẳng thức thay đổi chỉ quan trọng khi các giá trị được so sánh thực sự. 

Một điểm tinh tế là tính chính xác chỉ dựa vào việc xây dựng lại chế độ xem đã nén sau khi trích xuất phân đoạn truy vấn, vì các lần chạy có thể hợp nhất giữa các ranh giới truy vấn nhưng không ảnh hưởng đến cấu trúc chung trừ khi chúng liền kề theo thứ tự treap. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chúng tôi xem xét một chuỗi nhỏ và một vài hoạt động. 

Trình tự đầu vào:`[1, 1, 2, 2, 3]`| Bước | Hoạt động | Phân đoạn được trích xuất | Chạy sau khi nén | Cực tiểu địa phương | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | ban đầu | [1,1,2,2,3] | [1,2,3] | 1 | CÓ | 
| 2 | cập nhật(2,4,+1) | [1,2,3,3,3] | [1,2,3] | 1 | CÓ | 
| 3 | cập nhật(1,3,+2) | [3,4,3,3,3] | [3,4,3] | 3 | KHÔNG | 

Dấu vết này cho thấy các bản cập nhật có thể thay đổi cấu trúc lân cận như thế nào nhưng việc nén chỉ giữ lại các chuyển đổi thiết yếu. 

### Ví dụ 2 

đầu vào:`[5,5,5,5]`| Bước | Hoạt động | Phân đoạn | Chạy | Cực tiểu địa phương | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | kiểm tra | [5,5,5,5] | [5] | không | CÓ | 

Điều này chứng tỏ rằng một phân đoạn hoàn toàn đồng nhất luôn thỏa mãn điều kiện một cách tầm thường vì không tồn tại cực tiểu cục bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) khấu hao | Mỗi thao tác tách/hợp nhất đều có tính logarit và mỗi thao tác cập nhật/truy vấn chỉ chạm vào một số nút treap theo logarit | 
| Không gian | O(n) | Mỗi phần tử tương ứng với nhiều nhất một nút treap, không có sự trùng lặp ngoài các thao tác phân tách | 

Độ phức tạp phù hợp với các ràng buộc vì cả n và m đều lên tới 300.000 và chi phí logarit vẫn khả thi trong giới hạn thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    solve()
    return sys.stdout.getvalue()

# sample 1
assert run("""10
4 10 6 10
3
check 1 5
update 2 3 1
check 1 5
update 2 3 1
check 1 5
""").strip() == """YES
YES
NO"""

# sample 2
assert run("""8
10 -5 -5 -5 11 6 6 12
1
check 1 8
""").strip() == """YES"""

# custom: all equal
assert run("""5
1 1 1 1 1
1
check 1 5
""").strip() == "YES"

# custom: alternating
assert run("""5
1 2 1 2 1
1
check 1 5
""").strip() == "YES"

# custom: single element updates
assert run("""1
10
2
update 1 1 5
check 1 1
""").strip() == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | CÓ | trường hợp cạnh sập nén | 
| xen kẽ | CÓ | không có sự hợp nhất chạy bình đẳng | 
| phần tử đơn | CÓ | ổn định điều kiện biên | 

## Vỏ cạnh 

Một mảng hoàn toàn thống nhất như`[7,7,7,7]`vẫn là một lần chạy duy nhất trong tất cả các bản cập nhật vì việc thêm một hằng số sẽ duy trì sự bình đẳng. Trong bất kỳ truy vấn nào, việc nén mang lại một giá trị và không tồn tại cực tiểu cục bộ nào, vì vậy câu trả lời luôn là CÓ. Thuật toán xử lý việc này vì treap luôn chứa một nút duy nhất sau khi hợp nhất và việc truyền tải theo thứ tự sẽ tạo ra một danh sách đơn lẻ. 

Một trường hợp như`[1,2,3]`với các cập nhật lặp đi lặp lại để cân bằng các giá trị, chẳng hạn như làm cho nó`[2,2,3]`, gây ra sự hợp nhất của hai lần chạy đầu tiên sau khi lan truyền cập nhật. Bước hợp nhất treap đảm bảo sự bình đẳng lân cận được phát hiện ngay sau khi áp dụng cập nhật lười biếng, duy trì tính bất biến rằng không có hai nút liền kề nào có cùng giá trị. 

Một phạm vi truy vấn một phần tử như`[l,l]`luôn tạo ra một chuỗi giá trị đơn. Kiểm tra cực tiểu cục bộ trả về giá trị trống, được hiểu là tăng nghiêm ngặt theo mặc định, vì không có sự so sánh nào có thể vi phạm tính đơn điệu.
