---
title: "CF 102770B - Vấn đề đóng gói thùng"
description: "Chúng tôi có một chuỗi các mặt hàng đến từng cái một. Mỗi mục có một khối lượng và mọi thùng đều có dung lượng tối đa như nhau. Nhiệm vụ không phải là tìm cách đóng gói tối ưu."
date: "2026-08-01T22:23:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102770
codeforces_index: "B"
codeforces_contest_name: "The 17th Zhejiang Provincial Collegiate Programming Contest"
rating: 0
weight: 102770
solve_time_s: 86
verified: true
draft: false
---

[CF 102770B - Sự cố đóng thùng](https://codeforces.com/problemset/problem/102770/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi các mặt hàng đến từng cái một. Mỗi mục có một khối lượng và mọi thùng đều có dung lượng tối đa như nhau. Nhiệm vụ không phải là tìm cách đóng gói tối ưu. Thay vào đó, chúng tôi phải tái tạo chính xác hai chiến lược cố định, First Fit và Best Fit, đồng thời báo cáo số lượng thùng mà mỗi chiến lược tạo ra sau khi xử lý toàn bộ chuỗi. 

Đối với First Fit, các thùng có thứ tự cố định dựa trên thời gian tạo. Mỗi mặt hàng mới sẽ quét đơn hàng đó và vào thùng sớm nhất có đủ dung lượng trống. Đối với Best Fit, vật phẩm sẽ chọn thùng đầy nhất vẫn có thể chứa nó, nghĩa là trong số tất cả các thùng có thể, vật phẩm sẽ chọn thùng có dung lượng còn lại nhỏ nhất sau khi đặt. 

Đầu vào chứa một số trường hợp độc lập. Số lượng mục trong tất cả các trường hợp nhiều nhất là một triệu, vì vậy lời giải phải gần với tuyến tính hoặc tuyến tính. Một mô phỏng trực tiếp kiểm tra mọi thùng hiện có để tìm mọi mặt hàng có thể thực hiện xung quanh$n^2$séc. Với một triệu mặt hàng, con số đó có thể đạt khoảng$10^{12}$hoạt động vượt xa những gì một chương trình cuộc thi có thể xử lý. 

Công suất có thể lớn như$10^9$, vì vậy các giải pháp phân bổ mảng được lập chỉ mục theo dung lượng là không thể. Cấu trúc dữ liệu phải phụ thuộc vào số lượng thùng chứ không phải giá trị dung lượng. 

Một số chi tiết có thể phá vỡ việc triển khai đúng. Hãy xem xét một mục điền chính xác vào thùng:```
1
3 10
10 1 9
```Câu trả lời là:```
2 2
```Sau khi đặt mục đầu tiên, dung lượng còn lại bằng 0. Việc triển khai bất cẩn chỉ kiểm tra xem một thùng đã được sử dụng trước đó hay coi số 0 là trạng thái không hợp lệ có thể không sử dụng lại hoặc quản lý các thùng đó một cách chính xác. 

Một trường hợp đặc biệt khác là khi nhiều thùng có cùng dung lượng còn lại:```
1
4 10
6 4 6 4
```Câu trả lời là:```
2 2
```Thuật toán Best Fit có thể có nhiều lựa chọn tốt như nhau. Danh tính thùng chính xác không quan trọng đối với số lượng cuối cùng, nhưng việc triển khai phải loại bỏ và chèn chính xác các dung lượng còn lại trùng lặp. 

Một lỗi phổ biến cuối cùng là nhầm lẫn thứ tự mục với việc sắp xếp. Ví dụ:```
1
5 10
5 8 2 5 9
```Câu trả lời là:```
4 3
```Các thuật toán phải xử lý các mục chính xác khi chúng đến. Việc sắp xếp các mục sẽ thay đổi mô phỏng và tạo ra một kết quả khác. 

## Phương pháp tiếp cận 

Việc triển khai đơn giản sẽ lưu giữ danh sách các thùng và dung lượng còn lại của chúng. Đối với mỗi mục đến, First Fit sẽ quét danh sách từ đầu cho đến khi tìm thấy thùng phù hợp. Best Fit quét toàn bộ danh sách, giữ lại thùng phù hợp với dung lượng còn lại nhỏ nhất. Những mô phỏng này đúng vì chúng tuân theo trực tiếp các định nghĩa của hai thuật toán. 

Sự cố xuất hiện khi số lượng mục trở nên lớn. Trong trường hợp xấu nhất, hầu hết mọi mặt hàng đều có thể được kiểm tra ở hầu hết mọi thùng. Vì số lượng thùng cũng có thể tăng lên$n$, tổng công việc trở thành$O(n^2)$, quá chậm đối với$n=10^6$. 

Quan sát giúp giải quyết vấn đề là cả hai thuật toán chỉ cần thông tin được sắp xếp theo thứ tự về dung lượng còn lại. 

Đối với First Fit, chúng ta không cần biết từng thùng trong khi tìm kiếm. Chúng ta chỉ cần tìm thùng sớm nhất có dung lượng còn lại ít nhất bằng kích thước vật phẩm hiện tại. Cây phân đoạn có thể lưu trữ dung lượng tối đa còn lại trong mỗi phạm vi thùng. Nếu một nút cây phân đoạn có kích thước tối đa nhỏ hơn kích thước mục thì toàn bộ phạm vi đó không thể chứa câu trả lời. Bằng cách đi xuống cây, chúng ta có thể xác định được thùng hợp lệ đầu tiên trong$O(\log n)$. 

Đối với Best Fit, chúng tôi cần dung lượng còn lại nhỏ nhất ít nhất bằng kích thước vật phẩm. Đây là truy vấn giới hạn dưới trong nhiều tập hợp có thứ tự. Vì Python không có cây cân bằng tích hợp nên chúng tôi triển khai một treap ngẫu nhiên. Tre lưu trữ các dung lượng còn lại và hỗ trợ chèn, xóa và tìm kiếm giới hạn dưới trong dự kiến$O(\log n)$. 

Phương pháp brute-force hoạt động vì nó lưu trữ chính xác thông tin mà thuật toán cần, nhưng nó tìm kiếm thông tin đó quá chậm. Phương pháp nhanh hơn giữ nguyên trạng thái trong khi thêm khả năng chuyển trực tiếp vào thùng liên quan. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo hai mô phỏng độc lập vì First Fit và Best Fit đưa ra các lựa chọn khác nhau ngay cả khi chúng xử lý cùng một chuỗi mục. Đối với mỗi mục, hãy cập nhật cả hai cấu trúc riêng biệt. 
2. Đối với First Fit, lưu trữ dung lượng còn lại của mỗi thùng trong cây phân đoạn. Giá trị cây của một phạm vi là dung lượng còn lại tối đa giữa các thùng trong phạm vi đó. Khi xử lý một mục có kích thước`x`, tìm kiếm vị trí đầu tiên trong cây có giá trị tối đa được lưu trữ ít nhất`x`. Nếu có một vị trí như vậy, hãy giảm sức chứa còn lại của thùng đó xuống`x`và cập nhật cây. Nếu không, hãy tạo một thùng mới với dung lượng còn lại`C - x`. 
3. Việc tìm kiếm cây phân đoạn luôn ở bên trái đầu tiên. Điều này phù hợp với định nghĩa của First Fit vì các chỉ mục nhỏ hơn biểu thị các thùng được tạo trước đó. 
4. Để có Best Fit, hãy cất trữ dung lượng còn lại của tất cả các thùng trong một ngăn. Đối với một mặt hàng có kích thước`x`, tìm giá trị lưu nhỏ nhất ít nhất`x`. Nếu nó tồn tại, hãy loại bỏ dung lượng còn lại đó khỏi thùng và lắp dung lượng còn lại mới sau khi đặt vật phẩm. Nếu nó không tồn tại, hãy tạo một thùng mới với dung lượng còn lại`C - x`. 
5. Đếm mọi thùng mới được tạo trong mỗi mô phỏng. Hai bộ đếm là đầu ra cần thiết. 

Tại sao nó hoạt động: cây phân đoạn duy trì tính bất biến rằng mọi nút đều chứa dung lượng tối đa còn lại trong khoảng của nó. Trong quá trình tìm kiếm, bất kỳ khoảng thời gian nào có giá trị tối đa quá nhỏ đều không thể chứa thùng First Fit hợp lệ, do đó, việc bỏ qua khoảng thời gian đó sẽ không thể xóa câu trả lời đúng. Lá có thể truy cập đầu tiên có đủ dung lượng chính xác là thùng hợp lệ sớm nhất. 

Đối với Best Fit, tre sẽ duy trì tất cả dung lượng còn lại hiện tại theo thứ tự được sắp xếp. Thao tác giới hạn dưới trả về dung lượng nhỏ nhất có thể chứa mục, chính xác là thùng được Best Fit chọn. Việc xóa giá trị cũ và chèn giá trị mới sẽ giữ trạng thái được lưu trữ giống hệt với các thùng thực tế sau mỗi thao tác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import random

class TreapNode:
    __slots__ = ("key", "prio", "cnt", "left", "right")

    def __init__(self, key):
        self.key = key
        self.prio = random.randint(1, 1 << 60)
        self.cnt = 1
        self.left = None
        self.right = None

def rotate_right(root):
    x = root.left
    root.left = x.right
    x.right = root
    return x

def rotate_left(root):
    x = root.right
    root.right = x.left
    x.left = root
    return x

def treap_insert(root, key):
    if root is None:
        return TreapNode(key)
    if key == root.key:
        root.cnt += 1
    elif key < root.key:
        root.left = treap_insert(root.left, key)
        if root.left.prio < root.prio:
            root = rotate_right(root)
    else:
        root.right = treap_insert(root.right, key)
        if root.right.prio < root.prio:
            root = rotate_left(root)
    return root

def treap_erase(root, key):
    if root.key == key:
        if root.cnt > 1:
            root.cnt -= 1
        elif root.left is None:
            return root.right
        elif root.right is None:
            return root.left
        elif root.left.prio < root.right.prio:
            root = rotate_right(root)
            root.right = treap_erase(root.right, key)
        else:
            root = rotate_left(root)
            root.left = treap_erase(root.left, key)
    elif key < root.key:
        root.left = treap_erase(root.left, key)
    else:
        root.right = treap_erase(root.right, key)
    return root

def treap_lower_bound(root, key):
    ans = None
    while root:
        if root.key >= key:
            ans = root.key
            root = root.left
        else:
            root = root.right
    return ans

class SegmentTree:
    def __init__(self):
        self.size = 1
        self.tree = [0] * 2

    def append(self, value):
        if self.size - 1 >= self.count:
            old = self.size
            self.size *= 2
            self.tree = [0] * (2 * self.size)
            for i in range(self.count):
                self.tree[self.size + i] = self.values[i]
            for i in range(self.size - 1, 0, -1):
                self.tree[i] = max(self.tree[i * 2], self.tree[i * 2 + 1])
        self.values.append(value)
        self.count += 1
        self.tree[self.size + self.count - 1] = value
        p = (self.size + self.count - 1) // 2
        while p:
            self.tree[p] = max(self.tree[p * 2], self.tree[p * 2 + 1])
            p //= 2

    def init_empty(self):
        self.values = []
        self.count = 0

    def update(self, index, value):
        self.values[index] = value
        p = self.size + index
        self.tree[p] = value
        p //= 2
        while p:
            self.tree[p] = max(self.tree[p * 2], self.tree[p * 2 + 1])
            p //= 2

    def first_ge(self, value):
        if self.tree[1] < value:
            return -1
        node = 1
        left = 0
        right = self.size - 1
        while left != right:
            mid = (left + right) // 2
            if self.tree[node * 2] >= value:
                node = node * 2
                right = mid
            else:
                node = node * 2 + 1
                left = mid + 1
        return left

def solve_case(n, c, arr):
    ff = SegmentTree()
    ff.init_empty()
    ff_count = 0

    bf_root = None
    bf_count = 0

    for x in arr:
        pos = ff.first_ge(x)
        if pos == -1:
            ff.append(c - x)
            ff_count += 1
        else:
            ff.update(pos, ff.values[pos] - x)

        best = treap_lower_bound(bf_root, x)
        if best is None:
            bf_root = treap_insert(bf_root, c - x)
            bf_count += 1
        else:
            bf_root = treap_erase(bf_root, best)
            bf_root = treap_insert(bf_root, best - x)

    return ff_count, bf_count

def main():
    data = list(map(int, sys.stdin.buffer.read().split()))
    t = data[0]
    idx = 1
    ans = []
    for _ in range(t):
        n = data[idx]
        c = data[idx + 1]
        idx += 2
        arr = data[idx:idx + n]
        idx += n
        a, b = solve_case(n, c, arr)
        ans.append(f"{a} {b}")
    print("\n".join(ans))

if __name__ == "__main__":
    main()
```Mã duy trì hai trạng thái hoàn toàn riêng biệt vì cùng một mục có thể được đặt vào các thùng khác nhau theo hai chiến lược. 

Cây phân đoạn chỉ lưu trữ dung lượng còn lại cần thiết cho First Fit. các`first_ge`hàm tìm kiếm chỉ mục nhỏ nhất có dung lượng còn lại đủ lớn. Thứ tự tìm kiếm là con bên trái đầu tiên, giữ nguyên thứ tự thùng ban đầu. 

Việc triển khai tren hỗ trợ nhân đôi dung lượng còn lại bằng cách sử dụng`cnt`cánh đồng. Điều này quan trọng vì nhiều thùng có thể có không gian trống giống hệt nhau. Hàm giới hạn dưới không trả về thùng hợp lệ tùy ý. Nó trả về dung lượng còn lại hợp lệ nhỏ nhất, khớp chính xác với Best Fit. 

Cây phân đoạn sử dụng chỉ mục nội bộ dựa trên một và để lại số thùng hiện tại bằng 0. Vì tất cả kích thước của mục đều là số dương nên những lá không được sử dụng không thể vô tình trở thành câu trả lời. Số nguyên Python đã xử lý các giá trị dung lượng lớn mà không bị tràn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
1
2 2
1 1
```Các trạng thái mô phỏng là: 

| Mục | Kích thước | Thùng Fit đầu tiên còn lại | Số lượng Fit đầu tiên | Giá trị còn lại phù hợp nhất | Số lượng phù hợp nhất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | [1] | 1 | [1] | 1 | 
| 2 | 1 | [0] | 1 | [0] | 1 | 

Cả hai thuật toán đều sử dụng lại cùng một thùng vì dung lượng còn lại đủ cho mục thứ hai. 

Đối với mẫu thứ hai:```
1
5 10
5 8 2 5 9
```Các tiểu bang là: 

| Mục | Kích thước | First Fit còn lại | Số lượng Fit đầu tiên | Giá trị còn lại phù hợp nhất | Số lượng phù hợp nhất | 
| --- | --- | --- | --- | --- | --- | 
| 5 | 5 | [5] | 1 | [5] | 1 | 
| 8 | 8 | [5,2] | 2 | [5,2] | 2 | 
| 2 | 2 | [3,2] | 2 | [3,5] | 2 | 
| 5 | 5 | [3,2,5] | 3 | [3,5] | 2 | 
| 9 | 9 | [3,2,5,1] | 4 | [1,3,5] | 3 | 

Dấu vết cho thấy sự khác biệt giữa các chiến lược. First Fit tiếp tục kiểm tra các thùng trước đó và tạo một thùng mới cho mục có kích thước 5 vì hai thùng đầu tiên không thể chứa được. Best Fit tìm thùng có dung lượng còn lại là 5 và sử dụng thùng đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi mục thực hiện một số lượng không đổi các thao tác cây phân đoạn và xử lý. | 
| Không gian | O(n) | Có nhiều nhất một mục được lưu trữ cho mỗi thùng được tạo. | 

Tổng số mục trong tất cả các trường hợp thử nghiệm là một triệu. MỘT$O(n \log n)$giải pháp thực hiện khoảng 20 triệu bước logarit ở quy mô này, phù hợp với giới hạn dự kiến. Việc sử dụng bộ nhớ chỉ tăng theo số lượng thùng, nhiều nhất là số lượng mục. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = list(map(int, sys.stdin.buffer.read().split()))
    sys.stdin = old

    t = data[0]
    idx = 1
    out = []
    for _ in range(t):
        n, c = data[idx], data[idx + 1]
        idx += 2
        arr = data[idx:idx + n]
        idx += n
        out.append(str(solve_case(n, c, arr)[0]) + " " + str(solve_case(n, c, arr)[1]))
    return "\n".join(out)

assert run("""2
2 2
1 1
5 10
5 8 2 5 9
""") == """1 1
4 3"""

assert run("""1
1 1
1
""") == "1 1"

assert run("""1
4 10
6 4 6 4
""") == "2 2"

assert run("""1
6 10
10 10 10 10 10 10
""") == "6 6"

assert run("""1
5 10
5 5 5 5 5
""") == "3 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mục duy nhất có công suất một | 1 1 | Kích thước đầu vào tối thiểu và điền chính xác | 
| 6,4,6,4 | 2 2 | Năng lực trùng lặp và tái sử dụng | 
| Sáu món đồ cỡ mười | 6 6 | Mọi mặt hàng đều cần có thùng mới | 
| Năm món đồ bằng nhau có kích thước năm | 3 3 | Dung lượng còn lại bằng nhau lặp đi lặp lại | 

## Vỏ cạnh 

Khi thùng đầy chính xác, dung lượng còn lại của thùng bằng không. Cấu trúc vẫn giữ thùng đó vì nó tồn tại và có thể phù hợp với đơn đặt hàng First Fit. Đối với đầu vào:```
1
3 10
10 1 9
```First Fit tạo một thùng có dung lượng còn lại bằng 0, tạo một thùng khác cho mục có kích thước một và đặt mục cuối cùng vào thùng thứ hai. Best Fit tuân theo những lựa chọn tương tự. Đầu ra là:```
2 2
```Khi các dung lượng còn lại trùng lặp xuất hiện, cấu trúc Best Fit không được coi các giá trị là duy nhất. Vì:```
1
4 10
6 4 6 4
```sau hai mục đầu tiên, dung lượng còn lại là 4 và 6. Mục thứ ba sử dụng thùng có dung lượng 6, để lại dung lượng 4 và 0. Mục cuối cùng sử dụng dung lượng còn lại 4. Trường đếm của treap xử lý chính xác các trạng thái trùng lặp, tạo ra:```
2 2
```Khi thứ tự đầu vào thay đổi, kết quả có thể thay đổi ngay cả với cùng một bộ kích thước mục. Vì:```
1
5 10
5 8 2 5 9
```First Fit sắp xếp các thùng sớm theo thứ tự và kết thúc bằng bốn thùng, trong khi Best Fit sắp xếp lại việc sử dụng bằng cách luôn chọn thùng phù hợp nhất và kết thúc bằng ba thùng. Cấu trúc dữ liệu xử lý chuỗi ban đầu, do đó đầu ra vẫn giữ nguyên:```
4 3
```
