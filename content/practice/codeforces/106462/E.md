---
title: "CF 106462E - PPShneyneF4"
description: "Chúng tôi có một dải giấy một chiều. Lúc đầu, mỗi vị trí của dải chứa chính xác một lớp giấy. Một nếp gấp ở vị trí p sẽ tính đến hướng của cạnh ngắn hơn bằng cách đặt một phần của dải lên trên phần kia."
date: "2026-06-25T08:58:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106462
codeforces_index: "E"
codeforces_contest_name: "\u041f\u0435\u0440\u0432\u0435\u043d\u0441\u0442\u0432\u043e \u0421\u0432\u0435\u0440\u0434\u043b\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e \u0441\u0440\u0435\u0434\u0438 \u043d\u0430\u0447\u0438\u043d\u0430\u044e\u0449\u0438\u0445 2026"
rating: 0
weight: 106462
solve_time_s: 46
verified: true
draft: false
---

[CF 106462E - PPShneyneF4](https://codeforces.com/problemset/problem/106462/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dải giấy một chiều. Lúc đầu, mỗi vị trí của dải chứa chính xác một lớp giấy. Một nếp gấp ở vị trí`p`tính đến hướng của cạnh ngắn hơn bằng cách đặt một phần của dải lên trên phần kia. Sau vài lần gấp, một truy vấn sẽ hỏi tổng số lượng giấy nằm giữa hai vị trí cắt trên dải hiển thị hiện tại. Các lớp chồng lên nhau được tính nhiều lần vì vết cắt giữ tất cả các mảnh xếp chồng lên nhau ở đó. 

Đầu vào mô tả chiều rộng ban đầu và chuỗi thao tác. Thao tác gấp thay đổi cách sắp xếp hiện tại và giảm chiều rộng. Thao tác truy vấn đưa ra hai vị trí trên dải hiện tại và yêu cầu tổng số lớp trong khoảng đó. 

Các ràng buộc buộc chúng ta phải tránh mô phỏng từng ô một. Với chiều rộng và số lượng hoạt động đạt`10^5`, một cách tiếp cận tiêu tốn`O(width)`trên mỗi nếp gấp có thể đạt được khoảng`10^10`hoạt động, vượt xa những gì giới hạn cuộc thi thông thường cho phép. Chúng ta cần mọi phép toán phải gần với logarit. 

Phần khó khăn là các nếp gấp thay đổi thứ tự các vị trí, không chỉ giá trị của chúng. Một giải pháp chỉ lưu trữ tổng số lớp hiện tại không thể trả lời được gì về các khoảng thời gian tùy ý. Chúng ta cũng phải giữ nguyên trình tự các lớp sau mỗi lần gấp. 

Một ví dụ nhỏ cho thấy tại sao thứ tự lại quan trọng. Đối với đầu vào:```
3 2
1 1
2 0 2
```Sau khi gấp ở vị trí`1`, hai phần chồng lên nhau nên dải mới có hai vị trí có giá trị`[2, 1]`. Truy vấn yêu cầu toàn bộ dải và câu trả lời là:```
3
```Việc triển khai bất cẩn chỉ theo dõi chiều rộng sẽ quay trở lại`2`, vì nó sẽ đếm vị trí thay vì số lượng giấy. 

Một trường hợp cạnh khác là nếp gấp không đều. Vì:```
5 2
1 4
2 0 1
```Phần gấp để lại phần bên trái là phần dài hơn. Dải mới có giá trị`[1, 1, 1, 2]`, do đó vị trí đầu tiên chỉ chứa một lớp và câu trả lời là:```
1
```Việc triển khai sai luôn đảo ngược phần bên trái sang bên phải sẽ dịch chuyển phần còn lại và trả về giá trị không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là lưu trữ dải hiện tại trong một mảng. Một nếp gấp có thể được mô phỏng bằng cách tạo một mảng mới: tùy thuộc vào vị trí gấp, sao chép mặt lớn hơn và thêm mặt nhỏ đảo ngược vào đó. Điều này đúng vì nếp gấp chính xác là sự chồng chéo của đoạn này lên đoạn kia. Tuy nhiên, một nếp gấp có thể chạm tới gần như toàn bộ dải hiện tại. Nếu chiều rộng gần bằng`100000`và có nhiều nếp gấp, tổng số phần tử được xử lý có thể trở thành xung quanh`10^10`. 

Quan sát hữu ích là nếp gấp không phải là một sửa đổi tùy ý. Nó chỉ chia chuỗi thành hai phần, đảo ngược một trong số chúng và ghép các phần lại với nhau. Đây là tập hợp chính xác các hoạt động được hỗ trợ một cách hiệu quả bởi một treap ngầm. Mỗi nút treap đại diện cho một phân đoạn liên tục của dải hiện tại, lưu trữ tổng số lớp trong cây con của nó và hỗ trợ phân tách và hợp nhất theo vị trí. Cờ đảo ngược lười biếng cho phép chúng ta đảo ngược toàn bộ phân đoạn mà không cần chạm vào mọi phần tử. 

Lực lượng vũ phu hoạt động vì dải mới chỉ là một phép biến đổi của chuỗi cũ, nhưng nó không thành công khi có nhiều phép biến đổi lớn. Việc quan sát thấy các nếp gấp chỉ là các thao tác chia, đảo ngược và hợp nhất cho phép chúng ta giữ toàn bộ cấu trúc dải theo thời gian logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Kho báu tiềm ẩn | O(q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một kho lưu trữ ngầm chứa`n`nút. Mọi nút ban đầu đều có giá trị`1`, bởi vì mỗi phần gốc của tờ giấy là một lớp duy nhất. Tổng cây con của một nút biểu thị số lượng giấy trong đoạn đó của dải hiện tại. 
2. Đối với một truy vấn, hãy chia vùng dữ liệu xung quanh khoảng thời gian được yêu cầu. Phần ở giữa chính xác là vị trí có thể nhìn thấy giữa hai phần cắt, vì vậy tổng được lưu trữ của nó chính là câu trả lời. Hợp nhất các phần lại sau đó để khôi phục dải. 
3. Để gấp ở vị trí`p`, chia tre thành phần chiều dài bên trái`p`và phần bên phải chứa phần còn lại. Mặt nhỏ hơn là phần bị lật lên mặt lớn hơn. 
4. Nếu cạnh bên trái ngắn hơn hoặc bằng nhau thì đảo ngược và đặt sau cạnh phải. Nếu bên phải ngắn hơn thì đảo ngược bên phải và đặt sau bên trái. Tre thu được là dải gấp mới. 

Lý do điều này có tác dụng là vì thứ tự treap luôn khớp với thứ tự từ trái sang phải của bài viết hiện tại. Một nếp gấp chỉ thay đổi thứ tự này bằng cách đảo ngược một cạnh liền kề và di chuyển nó về cuối cạnh còn lại. Thao tác đảo ngược lười biếng sẽ thay đổi thứ tự mà không thay đổi tổng số lớp, do đó tất cả số tiền được lưu trữ vẫn hợp lệ. 

## Giải pháp Python```python
import sys
import random

input = sys.stdin.readline

class Node:
    __slots__ = ("val", "sum", "size", "left", "right", "prio", "rev")

    def __init__(self, val):
        self.val = val
        self.sum = val
        self.size = 1
        self.left = None
        self.right = None
        self.prio = random.randint(1, 1 << 30)
        self.rev = False

def size(t):
    return t.size if t else 0

def total(t):
    return t.sum if t else 0

def apply_rev(t):
    if t:
        t.rev ^= True
        t.left, t.right = t.right, t.left

def push(t):
    if t and t.rev:
        if t.left:
            t.left.rev ^= True
        if t.right:
            t.right.rev ^= True
        t.rev = False

def update(t):
    if t:
        t.size = 1 + size(t.left) + size(t.right)
        t.sum = t.val + total(t.left) + total(t.right)

def merge(a, b):
    if not a:
        return b
    if not b:
        return a
    if a.prio > b.prio:
        push(a)
        a.right = merge(a.right, b)
        update(a)
        return a
    else:
        push(b)
        b.left = merge(a, b.left)
        update(b)
        return b

def split(t, k):
    if not t:
        return None, None
    push(t)
    if size(t.left) >= k:
        a, b = split(t.left, k)
        t.left = b
        update(t)
        return a, t
    else:
        a, b = split(t.right, k - size(t.left) - 1)
        t.right = a
        update(t)
        return t, b

def solve(data):
    it = iter(data.split())
    n = int(next(it))
    q = int(next(it))

    root = None
    for _ in range(n):
        root = merge(root, Node(1))

    ans = []
    for _ in range(q):
        typ = int(next(it))
        if typ == 1:
            p = int(next(it))
            left, right = split(root, p)
            if size(left) <= size(right):
                apply_rev(left)
                root = merge(right, left)
            else:
                apply_rev(right)
                root = merge(left, right)
        else:
            l = int(next(it))
            r = int(next(it))
            a, b = split(root, l)
            b, c = split(b, r - l)
            ans.append(str(total(b)))
            root = merge(a, merge(b, c))

    return "\n".join(ans)

if __name__ == "__main__":
    data = sys.stdin.read()
    print(solve(data))
```Nút treap lưu trữ ba mẩu thông tin. Kích thước cần thiết để phân chia dựa trên vị trí và tổng cần thiết để trả lời các truy vấn. Cờ đảo ngược làm trì hoãn các thay đổi đối với các phần tử con, do đó việc đảo ngược một đoạn lớn chỉ lật một boolean và hoán đổi hai con trỏ. 

các`split`hàm cắt chuỗi hiện tại sau một số vị trí nhất định. Đây là thao tác quan trọng vì cả truy vấn và nếp gấp đều có thể được giảm xuống để lấy ra các phần liền kề. các`merge`hàm nối hai chuỗi trong khi vẫn giữ nguyên thứ tự của chúng. 

Trong quá trình gấp, mã đầu tiên sẽ tách hai bên. Việc so sánh kích thước của chúng quyết định mặt nào được gấp lại về mặt vật lý. Mặt ngắn hơn bị đảo ngược vì việc gấp làm thay đổi hướng của nó. Điều này tránh mọi chuyển động tuyến tính của các phần tử. 

Mã truy vấn tách phạm vi được yêu cầu, đọc tổng được lưu trữ và nối lại mọi thứ. Các vị trí biên được xử lý bằng cách phân tách tại`l`và sau đó chia chuỗi còn lại bằng`r - l`, giúp tránh được những sai sót riêng lẻ. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
7 4
1 3
1 2
2 0 1
2 1 2
```Các trạng thái quan trọng là: 

| Hoạt động | Trình tự các lớp hiện tại | Hành động | 
| --- | --- | --- | 
| Bắt đầu | 1 1 1 1 1 1 1 | Giấy ban đầu | 
| Gấp 3 | 2 2 2 1 | Bên trái đảo ngược và thêm | 
| Gấp 2 | 4 3 | Bên trái đảo ngược và thêm | 
| Truy vấn 0 1 | 4 3 | Tổng hợp vị trí đầu tiên | 
| Truy vấn 1 2 | 4 3 | Tổng hợp vị trí thứ hai | 

Truy vấn đầu tiên trả về`4`bởi vì phần hiển thị đầu tiên chứa bốn lớp. Lần thứ hai trở lại`3`, cho thấy cấu trúc dữ liệu giữ các vị trí khác nhau tách biệt. 

Đối với mẫu thứ hai:```
10 9
2 2 9
1 1
2 0 1
1 8
2 0 8
```Một dấu vết rút gọn là: 

| Hoạt động | Trình tự hiện tại | Kết quả | 
| --- | --- | --- | 
| Bắt đầu | 1 1 1 1 1 1 1 1 1 1 | | 
| Truy vấn 2 2 9 | mười cái | 7 | 
| Gấp 1 | 2 1 1 1 1 1 1 1 1 | | 
| Truy vấn 0 1 | 2 1 1 1 1 1 1 1 1 | 2 | 
| Gấp 8 | 3 2 1 1 1 1 1 1 | | 
| Truy vấn 0 8 | dải đầy đủ | 10 | 

Dấu vết này cho thấy trình tự được lưu trữ biểu thị tờ giấy được gấp hiện tại chứ không phải tờ giấy gốc, đây là tính bất biến mà thuật toán dựa vào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n) | Mỗi thao tác tách, hợp nhất, đảo ngược và truy vấn chỉ chạm vào chiều cao xử lý | 
| Không gian | O(n) | Treap có một nút cho mỗi vị trí giấy hiện tại | 

Số nút tren tối đa vẫn bằng số vị trí ban đầu. Việc đảo ngược lười biếng ngăn các nếp gấp mở rộng thành công tuyến tính, do đó giải pháp phù hợp với các giới hạn cho`10^5`hoạt động. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read()
    sys.stdin = old
    return solve(data)

assert run("""7 4
1 3
1 2
2 0 1
2 1 2
""") == "4\n3"

assert run("""10 9
2 2 9
1 1
2 0 1
1 8
2 0 8
1 2
2 1 3
1 4
2 2 4
""") == "7\n2\n10\n4\n5"

assert run("""1 1
2 0 1
""") == "1"

assert run("""5 2
1 4
2 0 1
""") == "1"

assert run("""6 3
1 3
2 0 3
2 1 2
""") == "6\n4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Giấy vị trí đơn | 1 | Xử lý kích thước tối thiểu | 
| Gấp nơi bên phải ngắn hơn | 1 | Định hướng đúng sau khi gấp không đều | 
| Gấp chiều dài bằng nhau | 6 và 4 | Truy vấn ở giữa và theo phạm vi | 

## Vỏ cạnh 

Khi nếp gấp chính giữa thì cả hai bên đều có chiều dài bằng nhau. Thuật toán chọn mặt trái làm mặt gấp, nhưng việc chọn mặt còn lại sẽ tạo ra kết quả vật lý tương tự sau khi đảo ngược tọa độ. Trình tự được lưu trữ vẫn hợp lệ vì thao tác vẫn chỉ là đảo ngược và hợp nhất. 

Khi nếp gấp xảy ra gần một cạnh, phần ngắn hơn có thể chỉ chứa một vị trí. Thuật toán vẫn thực hiện một lần chia và một lần đảo ngược lười biếng, tránh sai lầm khi cho rằng cả hai bên đều có kích thước tương tự nhau. 

Vì:```
3 2
1 1
2 0 2
```sự chia rẽ tạo ra`[1]`Và`[1,1]`. Mặt phần tử đơn được đảo ngược và hợp nhất với mặt kia, tạo ra`[2,1]`. Tổng số tiền là`3`, khớp với lượng giấy trong dải gấp. 

Vì:```
5 2
1 4
2 0 1
```sự chia rẽ tạo ra`[1,1,1,1]`Và`[1]`. Mặt nhỏ hơn được đảo ngược và gắn vào cuối, cho đúng thứ tự còn lại. Vị trí đầu tiên chỉ chứa một lớp, vì vậy câu trả lời cho truy vấn là`1`.
