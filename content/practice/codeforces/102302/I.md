---
title: "CF 102302I - Pokemino Vô Dụng"
description: "Hãy coi mỗi Pokemino là một điểm ((A,D)) trong mặt phẳng, trong đó tấn công là tọa độ ngang và phòng thủ là tọa độ dọc."
date: "2026-08-14T04:36:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102302
codeforces_index: "I"
codeforces_contest_name: "2019 USP-ICMC"
rating: 0
weight: 102302
solve_time_s: 229
verified: false
draft: false
---

[CF 102302I - Pokemino vô dụng](https://codeforces.com/problemset/problem/102302/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 49s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Hãy coi mỗi Pokemino là một điểm ((A,D)) trong mặt phẳng, trong đó tấn công là tọa độ ngang và phòng thủ là tọa độ dọc. Một Pokemino sẽ hữu ích nếu không có điểm nào mà Laersh sở hữu và không có điểm nào mà anh ta có thể có được bằng cách nhân giống, có hai tọa độ đều lớn hơn. 

Nhân giống mất trung bình có trọng số là hai điểm. Do đó, việc nhân giống lặp đi lặp lại sẽ mang lại chính xác bao lồi của tất cả các điểm đã bắt được. Vì vậy, câu hỏi hình học thực sự là: trong số các điểm đã chụp, điểm nào không bị chi phối hoàn toàn bởi bất kỳ điểm nào trên bao lồi của chúng? 

Câu trả lời được mô tả bằng ranh giới phía trên bên phải của bao lồi. Nếu chúng ta sắp xếp các điểm theo cách tấn công từ trái sang phải thì các điểm hữu ích sẽ tạo thành một chuỗi lồi. Một điểm mới có thể làm cho nhiều điểm liên tiếp trên chuỗi đó trở nên vô dụng nhưng không thể làm cho hai vùng riêng biệt biến mất độc lập. Vị trí này là yếu tố giúp thuật toán bảo trì thân tàu trực tuyến trở nên khả thi. 

Có tới (10^5) lần bắt giữ, trong khi mỗi lần tấn công và phòng thủ có thể lớn bằng (10^9). Thuật toán bậc hai sẽ yêu cầu khoảng (10^{10}) thao tác, vượt xa giới hạn một giây. Chúng tôi cần tổng công việc khoảng (O(N\log N)). Kích thước tọa độ cũng có nghĩa là các bài kiểm tra hình học phải được thực hiện với số học số nguyên chính xác. Trong C++, điều này yêu cầu số nguyên 64 bit vì tích chéo có thể đạt tới khoảng (10^{18}); Số nguyên Python tự động xử lý phạm vi này. 

Giá trị tấn công bằng nhau cần được xử lý đặc biệt. Hai Pokeminos có cùng đòn tấn công không thể thống trị hoàn toàn lẫn nhau vì cần phải có sự cải tiến nghiêm ngặt trong tấn công. Ví dụ,```
3
5 1
5 3
5 2
```có đầu ra```
0
0
0
```Việc thực hiện bất cẩn chỉ giữ mức phòng thủ cao nhất cho mỗi đòn tấn công sẽ loại bỏ sai hai Pokeminos còn lại. Chúng tôi sắp xếp các cuộc tấn công bằng nhau bằng cách giảm khả năng phòng thủ để nhóm dọc này được thể hiện chính xác. 

Cái bẫy thứ hai là sự bất bình đẳng nghiêm ngặt. Nếu một Pokemino nằm chính xác trên một đoạn giảm dần nối với hai đoạn khác thì nó vẫn hữu ích. Ví dụ,```
3
0 10
5 5
10 0
```có đầu ra```
0
0
0
```Pokemino ở giữa có thể được nhân giống từ các điểm cuối, nhưng điểm kết quả chính xác là ((5,5)), không hoàn toàn tốt hơn nó. Việc triển khai loại bỏ mọi điểm thẳng hàng sẽ đưa ra câu trả lời sai. 

Cái bẫy thứ ba là sự kết hợp lồi có thể thống trị Pokemino ngay cả khi cả bố và mẹ đều không làm như vậy một cách riêng lẻ. Ví dụ,```
3
0 10
10 0
5 4
```có đầu ra```
0
0
1
```Việc nhân giống hai Pokeminos đầu tiên có trọng lượng bằng nhau sẽ tạo ra ((5,5)), vượt trội hoàn toàn ((5,4)). Chỉ kiểm tra sự thống trị theo cặp trực tiếp sẽ bỏ lỡ trường hợp này. 

Cuối cùng, một điểm có thể vô dụng đơn giản vì điểm sau đó trực tiếp chiếm ưu thế. Vì```
3
0 0
2 2
1 1
```đầu ra là```
0
0
1
```Khi ((1,1)) đến, ((2,2)) đã được sở hữu và thống trị nó. Việc triển khai thân tàu chỉ kiểm tra hướng của ba điểm có thể bỏ lỡ trường hợp điểm cuối này trừ khi nó xử lý rõ ràng điểm đầu tiên của chuỗi được duy trì. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là xây dựng lại cấu trúc hình học sau mỗi lần chụp. Đối với mỗi tiền tố của đầu vào, chúng ta có thể xây dựng bao lồi của tất cả các Pokeminos hiện đang sở hữu, sau đó xác định những điểm bị bắt nào bị chi phối chặt chẽ bởi một số điểm của bao đó. Chi phí xây dựng thân tàu tiêu chuẩn (O(i\log i)) cho tiền tố có độ dài (i), sau đó là quét tuyến tính. Lặp lại điều này cho mọi chi phí tiền tố (O(N^2\log N)). Với (N=10^5), đây là thứ tự của (10^{11}) phép toán ở mức so sánh, do đó, việc xây dựng lại từ đầu không gần với giới hạn một giây. 

Cách tiếp cận bạo lực là đúng vì bao lồi chứa mọi điểm có thể đạt được thông qua việc nhân giống lặp đi lặp lại. Vấn đề là các tiền tố liên tiếp chỉ khác nhau một điểm được chèn vào, tuy nhiên giải pháp brute-force sẽ loại bỏ tất cả công việc hình học trước đó. 

Điều quan trọng cần lưu ý là các điểm hữu ích có một trật tự rất cứng nhắc. Sắp xếp chúng theo cách tăng sức tấn công và để tấn công bình đẳng bằng cách giảm khả năng phòng thủ. Hãy xem xét ba ứng cử viên liên tiếp (L,P,R). Nếu (P) nằm ngay bên dưới đoạn (LR), thì một số kết hợp lồi của (L) và (R) có đòn tấn công giống hệt như (P) và khả năng phòng thủ lớn hơn. Do đó (P) là vô ích và có thể bị xóa. 

Sử dụng tích chéo, điều kiện này là 

[ 
(L-P)\times(R-P)<0. 
] 

Nếu điểm là điểm đầu tiên trong cấu trúc có thứ tự thì không có lân cận bên trái. Nó chính xác là vô dụng khi người kế nhiệm ngay lập tức của nó có cả sức tấn công lớn hơn và khả năng phòng thủ lớn hơn. Nếu là điểm cuối cùng thì không thể vô dụng vì không có điểm sở hữu nào có sức tấn công lớn hơn. 

Thuộc tính động quan trọng là việc chèn một điểm chỉ có thể vô hiệu hóa các điểm lân cận xung quanh vị trí chèn đó. Khi một điểm bị xóa, nó không bao giờ cần phải quay trở lại, bởi vì các lần chụp trong tương lai chỉ phóng to bao lồi. Do đó, mỗi Pokemino được chèn một lần và bị xóa nhiều nhất một lần. Chúng ta có thể duy trì chuỗi có thứ tự trong cây tìm kiếm nhị phân cân bằng, cung cấp (O(\log N)) công việc cho mỗi lần chèn hoặc xóa. 

Python không cung cấp tập hợp thứ tự cân bằng tích hợp, do đó cách triển khai bên dưới sử dụng một trep ngẫu nhiên. Nó hỗ trợ chèn, xóa, tiền thân và kế thừa trong thời gian dự kiến ​​(O(\log N)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xây dựng lại thân tàu cho mọi tiền tố | (O(N^2\log N)) | (O(N)) | Quá chậm | 
| Chuỗi lồi động có treap | (O(N\log N)) dự kiến ​​| (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu thị mỗi Pokemino dưới dạng một điểm ((x,y)=(A,D)) và sắp xếp các điểm theo thứ tự tăng dần (x). Khi hai điểm có cùng (x), hãy sắp xếp điểm có (y) lớn hơn trước. Treap sử dụng khóa ((x,-y)) để thực hiện chính xác thứ tự này. 

Việc hòa này là cần thiết vì các Pokeminos tấn công ngang nhau không thể thống trị hoàn toàn lẫn nhau thông qua tấn công. 
2. Chỉ duy trì những Pokeminos hiện hữu ích. Trong tập hợp thứ tự này, một điểm có thể có nhiều nhất một điểm trước và một điểm kế tiếp, vì vậy liệu nó có trở nên vô dụng hay không có thể được xác định cục bộ. 
3. Sau khi chèn một điểm mới (P), trước tiên hãy kiểm tra xem bản thân (P) có vô dụng hay không. Nếu là điểm đầu tiên và điểm kế tiếp (R) thỏa mãn (R_x>P_x) và (R_y>P_y), thì (R) trực tiếp lấn át (P) nên (P) bị loại bỏ ngay lập tức. 
4. Nếu (P) có cả phần trước (L) và phần sau (R), hãy tính 

## (L_x-P_x)(R_y-P_y) 

(L_y-P_y)(R_x-P_x). 
]

Khi giá trị này âm, (P) nằm ngay bên dưới đoạn (LR). Sự kết hợp lồi thích hợp của (L) và (R) có sức tấn công tương tự (P) và khả năng phòng thủ cao hơn nên (P) vô dụng. 
5. Nếu điểm mới vẫn tồn tại, hãy kiểm tra lại điểm trước đó. Bất cứ khi nào người tiền nhiệm đó thỏa mãn bài kiểm tra vô dụng tương tự, hãy xóa nó. Việc chèn có thể làm cho một số điểm liên tiếp trở nên lỗi thời, vì vậy việc này tiếp tục cho đến khi điểm trước đó hợp lệ. 
6. Kiểm tra người kế nhiệm nhiều lần theo cách tương tự. Xóa bỏ mọi người kế thừa đã trở nên vô dụng. 
7. Sau khi loại bỏ tất cả các điểm không hợp lệ, kho chứa chính xác các Pokeminos hữu ích. Nếu (i) Pokeminos đã bị bắt và kho báu chứa (s) điểm, câu trả lời là (i-s). 

Lý do kiểm tra cục bộ là đủ là do bất biến chuỗi lồi được duy trì. Các điểm được giữ lại liên tiếp tạo thành ranh giới phía trên bên phải có liên quan của bao lồi. Một điểm bên dưới đoạn nối hai điểm lân cận của nó có thể tiếp cận được bằng cách nhân giống những điểm lân cận đó và bị chi phối nghiêm ngặt. Ngược lại, nếu mọi điểm bên trong được giữ lại nằm trên hoặc phía trên dây cung lân cận của nó và các điểm cuối thỏa mãn điều kiện trội trực tiếp thì không có tổ hợp lồi nào từ chuỗi được giữ lại có thể tạo ra một điểm tốt hơn. Việc chèn một điểm chỉ có thể thay thế một phần liền kề của ranh giới này, do đó việc xóa các điểm trước và sau không hợp lệ sẽ khôi phục hoàn toàn bất biến. 

Các điểm thẳng hàng được giữ lại một cách có chủ ý. Điểm thẳng hàng trên đoạn giảm dần không bị chi phối hoàn toàn bởi các điểm cuối của đoạn. Các cấu hình cộng tuyến có độ dốc dương được xử lý bằng thử nghiệm thống trị điểm cuối khi điểm thống trị đi vào chuỗi được duy trì. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline
sys.setrecursionlimit(1_000_000)

class Node:
    __slots__ = ("x", "y", "key", "prio", "left", "right")

    def __init__(self, x, y, prio):
        self.x = x
        self.y = y
        self.key = (x, -y)
        self.prio = prio
        self.left = None
        self.right = None

seed = 712367821

def rng():
    global seed
    seed ^= (seed << 13) & 0xFFFFFFFF
    seed ^= seed >> 17
    seed ^= (seed << 5) & 0xFFFFFFFF
    seed &= 0xFFFFFFFF
    return seed

def rotate_right(root):
    child = root.left
    root.left = child.right
    child.right = root
    return child

def rotate_left(root):
    child = root.right
    root.right = child.left
    child.left = root
    return child

def insert(root, node):
    if root is None:
        return node

    if node.key < root.key:
        root.left = insert(root.left, node)
        if root.left.prio < root.prio:
            root = rotate_right(root)
    else:
        root.right = insert(root.right, node)
        if root.right.prio < root.prio:
            root = rotate_left(root)

    return root

def merge(left, right):
    if left is None:
        return right
    if right is None:
        return left

    if left.prio < right.prio:
        left.right = merge(left.right, right)
        return left
    else:
        right.left = merge(left, right.left)
        return right

def erase(root, key):
    if root is None:
        return None

    if key == root.key:
        return merge(root.left, root.right)

    if key < root.key:
        root.left = erase(root.left, key)
    else:
        root.right = erase(root.right, key)

    return root

def predecessor(root, key):
    ans = None
    while root is not None:
        if root.key < key:
            ans = root
            root = root.right
        else:
            root = root.left
    return ans

def successor(root, key):
    ans = None
    while root is not None:
        if root.key > key:
            ans = root
            root = root.left
        else:
            root = root.right
    return ans

def cross(a, p, b):
    return (a.x - p.x) * (b.y - p.y) - \
           (a.y - p.y) * (b.x - p.x)

def inside(root, p):
    left = predecessor(root, p.key)
    right = successor(root, p.key)

    if right is None:
        return False

    if left is None:
        return right.x > p.x and right.y > p.y

    return cross(left, p, right) < 0

def solve():
    n = int(input())
    root = None
    useful = 0
    answer = []

    for _ in range(n):
        x, y = map(int, input().split())
        p = Node(x, y, rng())

        root = insert(root, p)
        useful += 1

        if inside(root, p):
            root = erase(root, p.key)
            useful -= 1
        else:
            while True:
                left = predecessor(root, p.key)
                if left is None or not inside(root, left):
                    break
                root = erase(root, left.key)
                useful -= 1

            while True:
                right = successor(root, p.key)
                if right is None or not inside(root, right):
                    break
                root = erase(root, right.key)
                useful -= 1

        answer.append(str(_ + 1 - useful))

    sys.stdout.write("\n".join(answer))

if __name__ == "__main__":
    solve()
```các`Node`lớp lưu trữ hai tọa độ, khóa thứ tự, mức độ ưu tiên ngẫu nhiên và hai con treap. Chìa khóa là`(x, -y)`, do đó, các phím nhỏ hơn tương ứng với đòn tấn công nhỏ hơn và để tấn công ngang bằng, phòng thủ lớn hơn. 

Các phép toán treap thực hiện các phép toán tập hợp theo thứ tự cần thiết cho thuật toán hình học. Các phép quay duy trì thuộc tính heap của mức độ ưu tiên ngẫu nhiên trong khi vẫn giữ nguyên thứ tự tọa độ.`predecessor`Và`successor`tìm các điểm lân cận ngay lập tức của một điểm mà không cần duyệt qua toàn bộ cấu trúc. các`inside`chức năng là lõi hình học. Điểm cuối cùng không thể vô ích vì không có gì có sức tấn công mạnh hơn. Điểm đầu tiên cần kiểm tra ưu thế trực tiếp. Mọi điểm khác đều được kiểm tra bằng cách sử dụng tích chéo. 

Quy trình chèn trước tiên sẽ thêm điểm và tăng số lượng điểm hữu ích. Nếu bản thân điểm mới không hợp lệ, nó sẽ bị xóa ngay lập tức. Nếu không, điểm trước và sau của nó có thể trở nên không hợp lệ do điểm mới đã thay đổi chuỗi. Hai vòng lặp while loại bỏ các điểm không hợp lệ liên tiếp đó. 

Biến`useful`tránh cần một trường có kích thước cây con trong treap. Mỗi lần chèn sẽ tăng nó một lần và mỗi lần xóa sẽ giảm nó một lần. Vì tổng số lần xóa nhiều nhất là (N), nên tổng số thao tác treap vẫn được mong đợi (O(N\log N)). 

Không có số học dấu phẩy động được sử dụng. Tích chéo được đánh giá trực tiếp bằng các số nguyên, giúp tránh các lỗi chính xác xung quanh các điểm thẳng hàng. Các số nguyên có độ chính xác tùy ý của Python cũng tránh tràn khi tọa độ gần (10^9). 

## Ví dụ đã hoạt động 

Đối với mẫu chính thức đầu tiên, đầu vào chứa mười điểm có hình học giữ mọi Pokemino bị bắt trên ranh giới hữu ích. Do đó, tre không bao giờ mất điểm. 

| Chụp | Điểm | Hành động | Số hữu ích | Đếm vô dụng | 
| --- | --- | --- | --- | --- | 
| 1 | (10, 0) | Chèn | 1 | 0 | 
| 2 | (10, 1) | Chèn | 2 | 0 | 
| 3 | (10, 2) | Chèn | 3 | 0 | 
| 4 | (9, 3) | Chèn | 4 | 0 | 
| 5 | (8, 4) | Chèn | 5 | 0 | 
| 6 | (7, 4) | Chèn | 6 | 0 | 
| 7 | (3, 4) | Chèn | 7 | 0 | 
| 8 | (2, 4) | Chèn | 8 | 0 | 
| 9 | (1, 4) | Chèn | 9 | 0 | 
| 10 | (0, 4) | Chèn | 10 | 0 | 

Các điểm tấn công ngang bằng tại (x=10) vẫn đồng thời hữu ích vì không có điểm nào trong số chúng có thể được cải thiện nghiêm ngặt về mặt tấn công. Các điểm còn lại tạo thành một chuỗi phòng thủ không tăng khi tấn công tăng lên, do đó không có sự kết hợp lồi nào tạo ra một điểm tốt hơn. Đầu ra là mười số không. 

Đối với mẫu chính thức thứ hai,```
5
3 6
6 4
6 9
7 2
10 8
```những thay đổi quan trọng xảy ra khi điểm thứ tư và thứ năm được chèn vào. 

| Chụp | Điểm | Đã xóa điểm | Số hữu ích | Đếm vô dụng | 
| --- | --- | --- | --- | --- | 
| 1 | (3, 6) | không | 1 | 0 | 
| 2 | (6, 4) | không | 2 | 0 | 
| 3 | (6, 9) | không | 3 | 0 | 
| 4 | (7, 2) | (6, 4) | 3 | 1 | 
| 5 | (10, 8) | (7, 2), (3, 6) | 2 | 3 | 

Ở lần chèn thứ tư,`(6,4)`có hàng xóm`(6,9)`Và`(7,2)`. Tích chéo của nó là âm, vì vậy nó nằm bên dưới đoạn kết nối của chúng và có thể đạt được với khả năng phòng thủ cao hơn trong cùng một cuộc tấn công. 

Ở lần chèn thứ năm,`(7,2)`đầu tiên trở nên không hợp lệ. Một khi nó được gỡ bỏ,`(3,6)`trở thành điểm đầu tiên và điểm kế tiếp của nó`(6,9)`trực tiếp chi phối nó. Do đó, ba trong số năm Pokeminos bị bắt đều vô dụng, mang lại kết quả```
0
0
1
2
3
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N\log N)) dự kiến ​​| Mỗi điểm được chèn một lần và bị xóa nhiều nhất một lần, với các thao tác treap được thực hiện dự kiến ​​(O(\log N)). | 
| Không gian | (O(N)) | Kho báu chứa tối đa (N) Pokeminos đã bắt được. | 

Với (N=10^5), (O(N\log N)) phù hợp với ràng buộc, trong khi việc xây dựng lại phần thân cho mọi tiền tố sẽ yêu cầu công việc gần đúng bậc hai. Việc sử dụng bộ nhớ là tuyến tính và duy trì ở mức dưới 256 MB. 

## Trường hợp thử nghiệm 

Khai thác sau đây giả định giải pháp đã gửi được lưu dưới dạng`solution.py`. Nó thực thi chương trình chính xác đó, do đó các cuộc kiểm thử không lặp lại việc triển khai.```python
# helper: run the submitted solution and return its output
import sys
import io
import subprocess

def run(inp: str) -> str:
    result = subprocess.run(
        [sys.executable, "solution.py"],
        input=inp,
        text=True,
        capture_output=True,
        check=True,
    )
    return result.stdout.strip()

# Official sample 1
sample1 = """\
10
10 0
10 1
10 2
9 3
8 4
7 4
3 4
2 4
1 4
0 4
"""
assert run(sample1) == "\n".join(["0"] * 10), "sample 1"

# Official sample 2
sample2 = """\
5
3 6
6 4
6 9
7 2
10 8
"""
assert run(sample2) == "\n".join(["0", "0", "1", "2", "3"]), "sample 2"

# Minimum-size input
assert run("1\n0 0\n") == "0", "single Pokemino"

# Equal attack values must all remain useful
same_attack = """\
4
5 0
5 100
5 50
5 1
"""
assert run(same_attack) == "\n".join(["0", "0", "0", "0"]), \
    "equal attack values"

# Direct dominance and positive-slope collinearity
positive_line = """\
3
0 0
2 2
1 1
"""
assert run(positive_line) == "\n".join(["0", "0", "1"]), \
    "direct dominance and insertion order"

# Convex combination can dominate without either parent doing so
convex = """\
3
0 10
10 0
5 4
"""
assert run(convex) == "\n".join(["0", "0", "1"]), \
    "convex combination"

# Maximum-size test with boundary coordinates.
# Every point has attack 0, so no point can have strictly greater attack.
n = 100000
lines = [str(n)]
for i in range(n):
    lines.append(f"0 {10**9 - i}")
large = "\n".join(lines) + "\n"
expected = "\n".join(["0"] * n)
assert run(large) == expected, "maximum-size equal-attack test"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0 0`|`0`| Đầu vào tối thiểu và xử lý hàng xóm trống | 
| Bốn điểm với cuộc tấn công`5`|`0 0 0 0`| Tấn công bình đẳng và thứ tự tùy chỉnh | 
|`(0,0), (2,2), (1,1)`|`0 0 1`| Sự thống trị trực tiếp và trường hợp ranh giới điểm đầu tiên | 
|`(0,10), (10,0), (5,4)`|`0 0 1`| Sự thống trị được tạo ra bằng cách nhân giống hai Pokeminos | 
| (100000) điểm khi tấn công`0`| (100000) số 0 | Kích thước đầu vào tối đa, tọa độ ranh giới và hiệu suất | 

## Vỏ cạnh 

Để tấn công bình đẳng, hãy xem xét```
3
5 1
5 3
5 2
```Thứ tự bên trong tre là`(5,3)`,`(5,2)`,`(5,1)`. Không có điểm nào có sức tấn công lớn hơn`5`, vì vậy mặc dù các cách phòng thủ khác nhau nhưng không ai có thể bị thống trị hoàn toàn. Điểm cuối cùng tự động an toàn vì nó không có người kế nhiệm, trong khi những người hàng xóm tấn công ngang bằng không đạt được điều kiện thống trị trực tiếp vì người kế nhiệm của họ không có sức tấn công lớn hơn. Đầu ra là`0 0 0`. 

Đối với các điểm thẳng hàng trên đoạn giảm dần,```
3
0 10
5 5
10 0
```điểm giữa có tích chéo bằng 0. Thuật toán chỉ loại bỏ điểm khi tích chéo hoàn toàn âm, do đó`(5,5)`ở lại trong thân tàu. Nhân giống các điểm cuối có thể sinh sản`(5,5)`, nhưng không thể tạo ra một điểm hoàn toàn tốt hơn ở cả hai tọa độ. Đầu ra là`0 0 0`. 

Đối với sự thống trị lồi,```
3
0 10
10 0
5 4
```hai điểm đầu tiên vẫn hữu ích. Khi`(5,4)`đến, người tiền nhiệm của nó là`(0,10)`và người kế nhiệm của nó là`(10,0)`. Sản phẩm chéo là 

[ 
(-5)(-4)-(6)(5)=20-30=-10. 
] 

Điểm nằm ngay bên dưới đoạn nối với các đoạn lân cận của nó. Trung điểm của chúng là`(5,5)`, có cùng đòn tấn công và phòng thủ lớn hơn, vì vậy`(5,4)`bị xóa và câu trả lời trở thành`1`. 

Để thống trị trực tiếp tại điểm cuối,```
3
0 0
2 2
1 1
```điểm`(0,0)`được loại bỏ khi`(2,2)`đến vì tọa độ kế tiếp lớn hơn ở cả hai tọa độ. Điểm thứ ba`(1,1)`được chèn sau`(0,0)`đã biến mất rồi, vậy`(2,2)`trở thành người kế vị của nó và trực tiếp thống trị nó. Đầu ra là`0 0 1`. Trường hợp này giải thích tại sao không thể thay thế điều kiện điểm đầu tiên chỉ bằng thử nghiệm sản phẩm chéo. 

Đối với trường hợp ranh giới có kích thước tối đa, tất cả (100000) điểm có thể bị tấn công`0`và sự phòng thủ khác biệt giữa`0`Và`10^9`. Vì sự thống trị nghiêm ngặt đòi hỏi phải tấn công lớn hơn nên mọi điểm vẫn hữu ích bất kể phòng thủ. Treap vẫn thực hiện một lần chèn cho mỗi điểm và không có thao tác xóa nào xảy ra, do đó thuật toán vẫn nằm trong thời gian chạy dự kiến ​​(O(N\log N)).
