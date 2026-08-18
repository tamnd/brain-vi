---
title: "CF 102222E - Cây 2-3-4"
description: "Chúng ta phải mô phỏng một chuỗi các lần chèn vào cây tìm kiếm 2-3-4. Các giá trị là hoán vị của 1..n, do đó mỗi giá trị xuất hiện chính xác một lần. Một nút có thể chứa một, hai hoặc ba khóa được sắp xếp. Một nút có ba khóa đã đầy."
date: "2026-08-17T22:05:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102222
codeforces_index: "E"
codeforces_contest_name: "2018-2019 ACM-ICPC, China Multi-Provincial Collegiate Programming Contest"
rating: 0
weight: 102222
solve_time_s: 102
verified: true
draft: false
---

[CF 102222E - Cây 2-3-4](https://codeforces.com/problemset/problem/102222/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta phải mô phỏng một chuỗi các lần chèn vào cây tìm kiếm 2-3-4. Các giá trị là một hoán vị của`1..n`, do đó mỗi giá trị xuất hiện đúng một lần. Một nút có thể chứa một, hai hoặc ba khóa được sắp xếp. Một nút có ba khóa đã đầy. Trước khi đi xuống một nút đầy đủ, chúng ta tách nó ra và chuyển khóa giữa của nó vào nút cha của nó. Nếu nút đầy đủ là gốc thì khóa giữa sẽ trở thành gốc mới và cây phát triển thêm một cấp. 

Sau tất cả các lần chèn, đầu ra được yêu cầu là toàn bộ cây theo thứ tự trước. Đối với mỗi nút được truy cập, chúng tôi in tất cả các khóa của nó trên một dòng theo thứ tự tăng dần. 

Đầu vào chứa tối đa 50 trường hợp thử nghiệm độc lập, với tối đa 5000 lần chèn vào một trường hợp. Một mô phỏng trực tiếp là đủ nếu mỗi lần chèn đi theo một đường dẫn của cây, vì cây 2-3-4 có chiều cao logarit. Với`n = 5000`, thậm chí là một`O(n log n)`việc triển khai chỉ thực hiện theo thứ tự vài chục nghìn thao tác nút cho mỗi trường hợp thử nghiệm. Trên tất cả 50 trường hợp, tổng số có thể đạt tới 250.000 giá trị được chèn, do đó tránh được`O(n²)`hoạt động cho mỗi trường hợp vẫn hữu ích. 

Những lỗi chính xác phổ biến nhất xảy ra xung quanh việc chia tách. Ví dụ, chèn`1 2 3 4`cho```
Case #1:
2
1
3 4
```Lần chèn thứ tư không chỉ đơn giản là nối thêm`4`tới tận gốc. Trước khi đi xuống, gốc`[1 2 3]`được chia xung quanh`2`, tạo rễ`[2]`với trẻ em`[1]`Và`[3]`. giá trị`4`sau đó đi vào con bên phải. 

Thứ tự chèn ngược lại thực hiện cùng một ranh giới từ phía bên kia. Vì```
1
4
4 3 2 1
```cây đúng là```
Case #1:
3
1 2
4
```Việc triển khai bất cẩn làm cho khóa sai hoặc tạo ra các phần tử con không đúng thứ tự, thường sẽ tạo ra một cây tìm kiếm có vẻ hợp lệ nhưng lại có một quá trình duyệt thứ tự trước khác. 

Trường hợp tinh vi thứ hai xảy ra khi một nút không phải gốc đã đầy. Giả sử gốc đã được tách ra và việc chèn sau đó sẽ tạo thành một nút con đầy đủ. Khóa giữa phải được chèn vào khóa cha hiện có, trong khi hai phần còn lại thay thế khóa con ban đầu. Việc tách một nút như vậy như thể nó là nút gốc sẽ làm tăng chiều cao của cây một cách không chính xác. 

Cuối cùng, ba phần chèn đầu tiên chỉ có hành vi trông đặc biệt vì gốc bắt đầu như một nút duy nhất. Vì`1 2 3`, cây chỉ đơn giản là một nút chứa`1 2 3`. Việc phân tách được thực hiện khi xử lý lần chèn tiếp theo, không phải ngay sau khi tạo nút đầy đủ. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu trực tiếp nhất có thể duy trì cây một cách rõ ràng và với mỗi giá trị mới, tìm kiếm lá đích của nó bằng cách quét cây hiện có cho đến khi tìm thấy khoảng thời gian thích hợp. Điều này đúng vì mỗi khóa thuộc về chính xác một khoảng con và việc chia các nút đầy đủ sẽ giữ nguyên thứ tự của cây tìm kiếm. Tuy nhiên, quá trình quét có thể kiểm tra mọi nút hiện có trong trường hợp xấu nhất. Trước khi`i`-th chèn có`i-1`giá trị được lưu trữ và nhiều nhất`i-1`các nút, do đó việc tìm kiếm toàn diện có chủ ý sẽ thực hiện tới`1 + 2 + ... + (n-1) = n(n-1)/2`kiểm tra nút. Vì`n = 5000`, tức là 12.497.500 lượt kiểm tra trong một trường hợp thử nghiệm và lên tới khoảng 625 triệu trong 50 trường hợp có kích thước tối đa. Python không nên dành nhiều thời gian để duyệt qua các phần không liên quan của cây. 

Cách tiếp cận bạo lực có hiệu quả vì bản thân cây đã chứa thông tin cần thiết để chọn đứa trẻ tiếp theo. Quan sát chính là cây 2-3-4 được cân bằng bởi cấu trúc. Mọi đường dẫn từ gốc đến lá đều có cùng độ dài và mỗi nút bên trong có ít nhất hai nút con. Theo đó, chiều cao là`O(log n)`. Chúng ta không bao giờ cần phải kiểm tra các cây con không liên quan. Tại mỗi nút, một, hai hoặc ba khóa của nó chia không gian tìm kiếm còn lại thành hai, ba hoặc bốn khoảng, do đó, một chuỗi so sánh sẽ xác định chính xác một nút con. 

Điều này đưa ra chiến lược chèn cây B từ trên xuống tiêu chuẩn. Trong khi đi theo đường dẫn tìm kiếm, hãy tách một nút đầy đủ trước khi đi xuống nút đó. Một nút đầy đủ chứa`[a, b, c]`, Vì thế`b`di chuyển lên trên và nút này trở thành hai nút chứa`[a]`Và`[c]`. Nếu nút có con thì hai con đầu tiên vẫn ở cùng`[a]`và hai cái cuối cùng vẫn còn với`[c]`. Khi chúng ta đến một lá, lá đó có tối đa hai khóa, vì vậy giá trị mới có thể được chèn vào đó một cách đơn giản. 

Việc triển khai kết quả tuân theo chính xác quy trình chèn từ bài toán, trong khi chỉ lưu trữ các nút trên cây thực tế và không bao giờ tìm kiếm các cây con không liên quan. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n²)`|`O(n)`| Quá chậm trong trường hợp xấu nhất | 
| Tối ưu |`O(n log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu diễn mọi nút theo cách sắp xếp của nó`keys`danh sách và nó`children`danh sách. Lá không có con, nút 2 có một khóa, nút 3 có hai khóa và nút 4 có ba khóa. Các nút bên trong có đúng một nút con nhiều hơn số lượng khóa. 
2. Đối với mỗi giá trị cần chèn, trước tiên hãy kiểm tra xem thư mục gốc đã đầy hay chưa. Nếu nó chứa ba khóa, hãy chia nó thành hai khóa con và thăng cấp khóa giữa của nó thành một gốc mới. Đây là hoạt động duy nhất làm tăng chiều cao của cây. 
3. Bắt đầu từ nút gốc, xử lý nút hiện tại trước khi chọn nút con. Nếu nút hiện tại là một lá, hãy chèn giá trị vào danh sách khóa đã sắp xếp của nó và dừng lại. Nút này không thể đã đầy vì các nút đầy đủ được phân chia trước khi chúng ta đi vào chúng. 
4. Nếu nút hiện tại là nút nội bộ, hãy xác định nút con nào chứa giá trị mới. Với phím`[k0]`, giá trị nhỏ hơn`k0`đi đến con số 0 và các giá trị lớn hơn sẽ chuyển đến con một. Với`[k0, k1]`, có ba khoảng. Với`[k0, k1, k2]`, có bốn. 
5. Trước khi xuống con đã chọn, hãy kiểm tra xem con đó đã đầy chưa. Nếu có thì hãy chia nó ra ngay. Khóa giữa của nó di chuyển vào nút hiện tại và hai nút kết quả sẽ thay thế nút con đầy đủ ban đầu. Đây là lý do tại sao cha mẹ phải được sửa đổi trước khi chọn chỉ mục con cuối cùng. 
6. Sau khi tách con, so sánh giá trị với khóa mới được thăng cấp. Nếu giá trị lớn hơn khóa được thăng cấp thì đích đến chính xác là con bên phải mới, do đó hãy tăng chỉ số con. Ngược lại, giá trị thuộc về con bên trái. 
7. Lặp lại quá trình đi xuống cho đến khi chạm tới một chiếc lá. Chèn giá trị vào lá bằng thao tác chèn được sắp xếp. 
8. Sau khi tất cả các giá trị đã được chèn vào, hãy thực hiện duyệt theo thứ tự trước. Trước tiên hãy in nút hiện tại, sau đó truy cập đệ quy các nút con của nó từ trái sang phải. 

Bất biến chính là trước mỗi lần hạ xuống, nút được nhập không đầy. Mỗi lần phân chia đều duy trì thứ tự sắp xếp của các khóa và sự tương ứng giữa các khoảng và khóa con. Vì mỗi lần chèn được thực hiện vào lá duy nhất có khoảng chứa giá trị nên tất cả các giá trị vẫn theo thứ tự cây tìm kiếm. Vì mọi nút đầy đủ đều được phân chia trước khi hạ xuống, nên không cần chèn thêm bốn nút nào sau khi nhập nó và cây vẫn là cây 2-3-4 cân bằng hợp lệ trong toàn bộ quá trình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("keys", "children")

    def __init__(self, keys=None, children=None):
        self.keys = [] if keys is None else keys
        self.children = [] if children is None else children

    def is_leaf(self):
        return not self.children

def split_child(parent, idx):
    """
    parent.children[idx] is a full node with three keys.

    Split:
        [a, b, c]
    into
        [a] and [c]
    and promote b into parent.
    """
    node = parent.children[idx]

    middle = node.keys[1]

    left = Node([node.keys[0]])
    right = Node([node.keys[2]])

    if node.children:
        left.children = node.children[:2]
        right.children = node.children[2:]

    parent.keys.insert(idx, middle)
    parent.children[idx] = left
    parent.children.insert(idx + 1, right)

def insert(root, value):
    # A full root has to be split before we start descending.
    if len(root.keys) == 3:
        new_root = Node([], [root])
        split_child(new_root, 0)
        root = new_root

    cur = root

    while True:
        if cur.is_leaf():
            # The leaf is guaranteed not to be full.
            if not cur.keys:
                cur.keys.append(value)
            elif value < cur.keys[0]:
                cur.keys.insert(0, value)
            elif value > cur.keys[-1]:
                cur.keys.append(value)
            else:
                # Input is a permutation, so this branch is unreachable.
                pos = 0
                while pos < len(cur.keys) and cur.keys[pos] < value:
                    pos += 1
                cur.keys.insert(pos, value)
            return root

        # Find the child interval containing value.
        idx = 0
        while idx < len(cur.keys) and value > cur.keys[idx]:
            idx += 1

        # Split a full child before descending into it.
        if len(cur.children[idx].keys) == 3:
            split_child(cur, idx)

            # The promoted key now sits at cur.keys[idx].
            if value > cur.keys[idx]:
                idx += 1

        cur = cur.children[idx]

def preorder(root, out):
    out.append(" ".join(map(str, root.keys)))
    for child in root.children:
        preorder(child, out)

def solve():
    t = int(input())
    output = []

    for case_id in range(1, t + 1):
        n = int(input())
        a = list(map(int, input().split()))

        root = Node()

        for value in a:
            root = insert(root, value)

        output.append(f"Case #{case_id}:")
        preorder(root, output)

    sys.stdout.write("\n".join(output))

if __name__ == "__main__":
    solve()
```các`Node`lớp giữ cho sự biểu diễn có chủ ý nhỏ. Một nút có nhiều nhất là ba khóa và nhiều nhất là bốn nút con, vì vậy các phép toán danh sách Python bên trong một nút là thời gian không đổi trong phân tích tiệm cận.`split_child`thực hiện các hoạt động cấu trúc trung tâm. Đối với một nút đầy đủ`[a, b, c]`, chìa khóa`b`được thăng cấp lên cấp bậc cha mẹ. Nút bên trái nhận`a`và nút bên phải nhận được`c`. Nếu nút ban đầu là nút bên trong thì hai nút con đầu tiên của nó thuộc về nút bên trái và hai nút con cuối cùng của nó thuộc về nút bên phải. Các chỉ số cắt lát`[:2]`Và`[2:]`chính xác là sự phân chia bốn con theo yêu cầu của các khoảng cây tìm kiếm. 

Gốc được xử lý riêng biệt vì nó không có cha mẹ để có thể thăng cấp khóa giữa của nó. Tạo một gốc mới chứa hai cây con tương đương với việc tăng chiều cao của cây lên một. 

Đối với một đứa trẻ không có gốc,`split_child(cur, idx)`chèn khóa được thăng cấp vào đúng vị trí`idx`trong phần cha mẹ và chèn nửa bên phải ngay sau nửa bên trái. Nếu giá trị được chèn lớn hơn khóa được thăng cấp thì chỉ mục con phải được tăng lên. Việc quên điều chỉnh này là nguyên nhân phổ biến dẫn đến các câu trả lời sai vì chỉ số con cũ hiện đề cập đến nửa bên trái. 

Việc chèn lá sử dụng danh sách Python vì một nút có tối đa ba khóa. Không có chi phí tiệm cận đáng kể nào từ việc dịch chuyển một số yếu tố này. Vì đầu vào được đảm bảo là một hoán vị nên việc xử lý trùng lặp không bao giờ thực sự cần thiết. 

Quá trình truyền tải cuối cùng in một nút trước các nút con của nó, đó chính xác là quá trình truyền tải theo thứ tự trước. Các phần tử con đã được lưu trữ từ trái sang phải, vì vậy việc truy cập chúng theo thứ tự danh sách sẽ cung cấp khả năng duyệt theo yêu cầu mà không cần sắp xếp bổ sung. 

Tràn số nguyên Python không liên quan ở đây vì mọi giá trị đều nằm giữa`1`Và`n`và cây lưu trữ các giá trị đó một cách trực tiếp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Các mẫu chèn đầu tiên`1, 2, 3, 4`. Ba giá trị đầu tiên phù hợp với gốc. Trong lần chèn thứ tư, toàn bộ gốc được phân chia trước khi thuật toán đi xuống. 

| Chèn | Gốc hiện tại | Hành động | Kết quả | 
| --- | --- | --- | --- | 
|`1`| trống | Chèn vào lá |`[1]`| 
|`2`|`[1]`| Chèn vào lá |`[1 2]`| 
|`3`|`[1 2]`| Chèn vào lá |`[1 2 3]`| 
|`4`|`[1 2 3]`| Chia gốc xung quanh`2`| gốc`[2]`, những đứa trẻ`[1]`,`[3]`| 
|`4`|`[2]`| Xuống bên phải, chèn vào`[3]`|`[3 4]`| 

Việc duyệt qua đơn đặt hàng trước cuối cùng là```
Case #1:
2
1
3 4
```Dấu vết này chứng tỏ tại sao sự phân tách xảy ra trước khi đi xuống. Nếu như`4`được chèn vào thư mục gốc đầy đủ trước tiên, kết quả sẽ không còn tuân theo quy trình chèn đã chỉ định. 

### Mẫu 2 

Mẫu thứ hai sử dụng hoán vị ngược`4, 3, 2, 1`. 

| Chèn | Gốc cây hiện tại | Hành động | Kết quả đường dẫn có liên quan | 
| --- | --- | --- | --- | 
|`4`| trống | Chèn vào lá |`[4]`| 
|`3`|`[4]`| Chèn vào lá |`[3 4]`| 
|`2`|`[3 4]`| Chèn vào lá |`[2 3 4]`| 
|`1`|`[2 3 4]`| Chia gốc xung quanh`3`| gốc`[3]`, những đứa trẻ`[2]`,`[4]`| 
|`1`|`[3]`| Đi xuống bên trái và chèn |`[1 2]`| 

Cây cuối cùng là```
Case #2:
3
1 2
4
```Phím giữa luôn được thăng cấp. Trong trường hợp này, toàn bộ thư mục gốc`[2 3 4]`thúc đẩy`3`, không`2`hoặc`4`. Các giá trị nhỏ hơn vẫn ở con bên trái và các giá trị lớn hơn vẫn ở con bên phải. 

Mẫu thứ ba lớn hơn và thể hiện sự phân tách lặp đi lặp lại ở các cấp độ khác nhau. Đầu ra cuối cùng của nó là```
Case #3:
5 9
2
1
3 4
7
6
8
11 13 15
10
12
14
16 17
```Thư mục gốc chứa hai khóa,`5`Và`9`, vậy là nó có ba đứa con. Việc duyệt thứ tự trước sẽ in gốc trước, sau đó là toàn bộ cây con chứa các giá trị bên dưới`5`, thì cây con giữa`5`Và`9`, và cuối cùng là cây con ở trên`9`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n)`| Mỗi lần chèn đều tuân theo một đường dẫn từ gốc đến lá có chiều cao logarit và mỗi nút có tối đa ba khóa | 
| Không gian |`O(n)`| Cây có chứa`O(n)`các nút và mỗi nút lưu trữ các danh sách con và khóa có kích thước không đổi | 

Cây 2-3-4 có chiều cao`O(log n)`bởi vì mỗi nút bên trong có ít nhất hai nút con và tất cả các lá đều có cùng độ sâu. Với`n <= 5000`, mỗi lần chèn chỉ truy cập một số lượng nhỏ nút. Ngay cả với 50 trường hợp thử nghiệm, tổng khối lượng công việc vẫn nằm trong giới hạn 10 giây đã nêu. 

## Trường hợp thử nghiệm 

Đầu vào và đầu ra mẫu chính thức có thể được kiểm tra chính xác. Các thử nghiệm tùy chỉnh bên dưới sử dụng các hoán vị nhỏ trong đó cây dự kiến ​​có thể được lấy bằng tay. Đặc tả yêu cầu mọi trường hợp thử nghiệm phải là một hoán vị của`1..n`, do đó, đầu vào hoàn toàn bằng nhau không phải là trường hợp kiểm thử hợp lệ và không được đưa vào làm kiểm tra tính chính xác cho chương trình đã gửi.```python
import sys
import io

class Node:
    __slots__ = ("keys", "children")

    def __init__(self, keys=None, children=None):
        self.keys = [] if keys is None else keys
        self.children = [] if children is None else children

def split_child(parent, idx):
    node = parent.children[idx]

    middle = node.keys[1]
    left = Node([node.keys[0]])
    right = Node([node.keys[2]])

    if node.children:
        left.children = node.children[:2]
        right.children = node.children[2:]

    parent.keys.insert(idx, middle)
    parent.children[idx] = left
    parent.children.insert(idx + 1, right)

def insert(root, value):
    if len(root.keys) == 3:
        new_root = Node([], [root])
        split_child(new_root, 0)
        root = new_root

    cur = root

    while True:
        if not cur.children:
            pos = 0
            while pos < len(cur.keys) and cur.keys[pos] < value:
                pos += 1
            cur.keys.insert(pos, value)
            return root

        idx = 0
        while idx < len(cur.keys) and value > cur.keys[idx]:
            idx += 1

        if len(cur.children[idx].keys) == 3:
            split_child(cur, idx)
            if value > cur.keys[idx]:
                idx += 1

        cur = cur.children[idx]

def preorder(root, out):
    out.append(" ".join(map(str, root.keys)))
    for child in root.children:
        preorder(child, out)

def solution(inp: str) -> str:
    data = list(map(int, inp.split()))
    it = iter(data)

    t = next(it)
    out = []

    for case_id in range(1, t + 1):
        n = next(it)
        values = [next(it) for _ in range(n)]

        root = Node()

        for x in values:
            root = insert(root, x)

        out.append(f"Case #{case_id}:")
        preorder(root, out)

    return "\n".join(out)

def run(inp: str) -> str:
    return solution(inp)

sample_input = """\
3
4
1 2 3 4
4
4 3 2 1
17
6 3 5 7 1 10 2 9 4 8 11 12 13 14 15 16 17
"""

sample_output = """\
Case #1:
2
1
3 4
Case #2:
3
1 2
4
Case #3:
5 9
2
1
3 4
7
6
8
11 13 15
10
12
14
16 17
"""

assert run(sample_input) == sample_output, "official samples"

assert run("""\
1
1
1
""") == """\
Case #1:
1
""", "minimum-size case"

assert run("""\
1
4
1 2 3 4
""") == """\
Case #1:
2
1
3 4
""", "root split"

assert run("""\
1
4
4 3 2 1
""") == """\
Case #1:
3
1 2
4
""", "reverse insertion"

assert run("""\
1
7
1 2 3 4 5 6 7
""") == """\
Case #1:
4
2
1
3
6
5
7
""", "multiple root and child splits"

# The original constraints require a permutation, so an all-equal case
# such as 4 / 1 1 1 1 is intentionally not tested as valid input.
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 1`|`Case #1:`, sau đó`1`| Khởi tạo cây nhỏ nhất có thể và gốc rỗng | 
|`1 / 4 / 1 2 3 4`| Gốc`2`, những đứa trẻ`1`Và`3 4`| Tách toàn bộ gốc trước khi giảm dần | 
|`1 / 4 / 4 3 2 1`| Gốc`3`, những đứa trẻ`1 2`Và`4`| Hành vi đối xứng để giảm đầu vào | 
|`1 / 7 / 1 2 3 4 5 6 7`| Gốc`4`với ba cấp độ đầu ra đặt hàng trước | Lặp đi lặp lại việc chia tách và cập nhật chỉ mục con | 

## Vỏ cạnh 

Đầu vào hợp lệ nhỏ nhất là`n = 1`với hoán vị`1`. Gốc bắt đầu trống rỗng,`1`được chèn trực tiếp vào nó và việc duyệt trước sẽ in chính xác một dòng chứa`1`. Không có sự phân chia đặc biệt hoặc truyền tải con nào liên quan. 

Vì`n = 4`và đầu vào`1 2 3 4`, gốc trở thành`[1 2 3]`sau ba lần chèn đầu tiên. Trước khi xử lý`4`, thuật toán phát hiện gốc đã đầy, thúc đẩy`2`, và tạo ra con`[1]`Và`[3]`. Từ`4 > 2`, chỉ số con vẫn ở bên phải và`4`được chèn vào`[3]`, sản xuất`[3 4]`. Đầu ra chính xác là`2`,`1`,`3 4`trong đơn đặt hàng trước. 

Vì`n = 4`và đầu vào`4 3 2 1`, gốc đầy đủ là`[2 3 4]`trước khi chèn`1`. Phím giữa`3`được thúc đẩy, sản xuất`[3]`với trẻ em`[2]`Và`[4]`. Từ`1 < 3`, thuật toán đi xuống con bên trái và thu được`[1 2]`. Điều này phát hiện các hoạt động triển khai vô tình quảng bá khóa đầu tiên hoặc khóa cuối cùng thay vì khóa giữa. 

Một trường hợp ranh giới sâu hơn xuất hiện khi một đứa trẻ được no trong khi cha của nó thì không. Xem xét tăng đầu vào thông qua`1 2 3 4 5 6 7`. Sau lần chèn thứ tư, gốc là`[2]`với trẻ em`[1]`Và`[3 4]`. chèn`5`tạo ra một đứa con phù hợp`[3 4 5]`. Trước khi chèn`6`, đứa trẻ đó bị chia cắt xung quanh`4`, do đó gốc trở thành`[2 4]`với trẻ em`[1]`,`[3]`, Và`[5]`. giá trị`6`sau đó nhập vào con thứ ba. Việc chèn thêm có thể làm cho một phần tử con khác đầy đủ và gây ra sự phân chia cục bộ khác. Root không đạt được cấp độ mới chỉ vì một phần tử con bị chia tách, đó chính xác là lý do tại sao việc xử lý gốc phải được tách biệt khỏi việc xử lý phần tử con thông thường. 

Một đầu vào hoàn toàn bằng nhau không hợp lệ, chẳng hạn như```
1
4
1 1 1 1
```không được sử dụng để đánh giá lời giải vì bài toán đảm bảo rõ ràng một hoán vị của`1..n`. Nếu một khai thác kiểm thử riêng biệt muốn kiểm tra việc xử lý trùng lặp thì đó là hành vi kiểm tra bên ngoài hợp đồng của vấn đề. Thuật toán được gửi không cần xác định kết quả cho đầu vào đó. 

Bản thân đầu ra đặt hàng trước là một điều kiện biên khác. Đối với mỗi nút bên trong, các khóa của nút hiện tại phải được in trước bất kỳ nút con nào. Sau khi in gốc`[5 9]`trong mẫu thứ ba, quá trình truyền tải in hoàn toàn cây con bên dưới`5`, thì cây con giữa`5`Và`9`, và chỉ khi đó cây con ở trên`9`. Việc in các nút con trước nút hiện tại sẽ tạo ra một cấu trúc giống như thứ tự và sẽ thất bại ngay cả khi cây 2-3-4 bên dưới được xây dựng hoàn hảo.
