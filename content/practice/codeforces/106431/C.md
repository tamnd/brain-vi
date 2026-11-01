---
title: "CF 106431C - Đạo Cây (tìm cây)"
description: "Vấn đề yêu cầu chúng ta duy trì cây tìm kiếm nhị phân trong khi một chuỗi lệnh được thực thi. Cây bắt đầu trống rỗng."
date: "2026-06-25T09:38:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106431
codeforces_index: "C"
codeforces_contest_name: "Entrenamiento OIE Nivel Experto - Semana 12"
rating: 0
weight: 106431
solve_time_s: 37
verified: true
draft: false
---

[CF 106431C - Đạo Cây (tìm kiếm cây)](https://codeforces.com/problemset/problem/106431/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề yêu cầu chúng ta duy trì cây tìm kiếm nhị phân trong khi một chuỗi lệnh được thực thi. Cây bắt đầu trống rỗng. Lệnh chèn sẽ đặt một khóa mới vào đúng vị trí lá theo quy tắc cây tìm kiếm nhị phân thông thường, đồng thời bỏ qua thao tác nếu khóa đã có sẵn. Lệnh xóa sẽ loại bỏ một khóa nếu nó tồn tại, sử dụng quy tắc xóa BST tiêu chuẩn: thay thế một nút bằng hai nút con theo thứ tự trước nó. Lệnh in yêu cầu chúng ta xuất cây hiện tại bằng cách sử dụng biểu diễn trong ngoặc đơn đệ quy. 

Đầu vào là một luồng hoạt động chứ không phải là một cây tĩnh. Mỗi thao tác thay đổi cấu trúc mà các thao tác sau sẽ quan sát. Đầu ra là hình dạng chính xác của cây sau mỗi lần in, do đó chỉ lưu trữ bộ khóa là không đủ. Mối quan hệ cha mẹ và con cái phải được bảo tồn. 

Các ràng buộc được thiết kế xung quanh mô phỏng. Số lượng thao tác có thể đủ lớn để việc xây dựng lại toàn bộ cây sau mỗi lệnh sẽ rất lãng phí. Nếu có khoảng 100000 thao tác, phương pháp quét tất cả các nút hiện có cho mọi thao tác có thể đạt tới khoảng 10^10 thao tác trong trường hợp xấu nhất, vượt xa giới hạn một giây thông thường. Cách tiếp cận dự định là làm cho mỗi lần sửa đổi cây tỷ lệ thuận với chiều cao hiện tại của cây. Cây tìm kiếm nhị phân có thể trở thành một chuỗi, do đó chiều cao trong trường hợp xấu nhất là O(n), nhưng tổng số lệnh là yếu tố giới hạn thực sự và chúng ta cần tránh việc duyệt toàn bộ thêm. 

Các trường hợp phức tạp chủ yếu xoay quanh việc xóa và in. Ví dụ, xóa một lá thì phải loại bỏ chính xác nút đó.```
ADD 5
ADD 3
DEL 3
PRINT
```Đầu ra là:```
5
```Việc triển khai bất cẩn chỉ đánh dấu các nút là đã bị xóa nhưng không ngắt kết nối con trỏ cha vẫn có thể in ra nút con cũ. 

Xóa một nút có hai nút con là một lỗi phổ biến khác.```
ADD 5
ADD 3
ADD 7
ADD 4
DEL 5
PRINT
```Đầu ra đúng là:```
4(3,7)
```Số trước của 5 là 4, vì vậy 4 thay thế 5. Việc triển khai chọn số kế tiếp sẽ tạo ra một cây khác. 

Một cây trống cũng có thể.```
DEL 10
PRINT
```Đầu ra là:```

```Việc xóa sẽ không có tác dụng gì vì khóa không tồn tại. Việc in một cây trống sẽ tạo ra một dòng trống chứ không phải một điểm đánh dấu đặc biệt. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là biểu diễn mọi nút bằng khóa và con trỏ của nó tới nút con trái và nút phải. Để chèn, chúng tôi bắt đầu từ gốc và liên tục so sánh khóa mới với nút hiện tại cho đến khi tìm thấy vị trí con trống. Đây chính xác là cách BST được xác định, vì vậy quy trình là chính xác. Để xóa, chúng ta tìm kiếm nút rồi xử lý ba trường hợp: không có con, một con hoặc hai con. Việc in ấn chỉ là quá trình truyền tải đệ quy tuân theo định dạng được yêu cầu. 

Cách tiếp cận bạo lực sẽ là lưu trữ tất cả các khóa trong một mảng và xây dựng lại cây bất cứ khi nào có lệnh thay đổi nó. Điều này vẫn đúng vì việc chèn các khóa giống nhau theo cùng một thứ tự sẽ tạo lại cùng một BST. Tuy nhiên, việc xây dựng lại sau mỗi lệnh sẽ tốn O(n) công việc. Với tối đa 100000 thao tác, tốc độ này có thể trở thành khoảng O(n²), quá chậm. 

Quan sát quan trọng là hoạt động BST chỉ cần kiểm tra đường dẫn từ gốc đến khóa bị ảnh hưởng. Phần còn lại của cây đã ở trạng thái hợp lệ và không cần phải chạm vào. Việc xóa nút cũng chỉ thay đổi một số lượng nhỏ các liên kết xung quanh nút bị xóa. Điều này làm giảm mọi cập nhật để truyền tải theo chiều cao. 

Vấn đề còn lại là in ấn. Lệnh in phải truy cập vào mọi nút vì toàn bộ cấu trúc phải được xuất ra. Chi phí đó là không thể tránh khỏi vì bản thân đầu ra đã chứa toàn bộ cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q²) trong trường hợp xấu nhất | O(n) | Quá chậm | 
| Tối ưu | O(qh + tổng số nút được in) | O(n) | Đã chấp nhận | 

Ở đây q là số lệnh và h là chiều cao của cây hiện tại. 

## Hướng dẫn thuật toán 

1. Lưu trữ cây bằng cách sử dụng các nút chứa khóa và các tham chiếu đến con trái và con phải của chúng. Giữ một tham chiếu đến nút gốc. Bản thân cấu trúc cây là thông tin chúng ta cần cho các hoạt động trong tương lai. 
2. Đối với thao tác THÊM, hãy bắt đầu từ gốc và so sánh khóa mới với từng nút được truy cập. Di chuyển sang trái để có phím nhỏ hơn và sang phải để có phím lớn hơn. Khi con trỏ con tiếp theo trống, hãy đính kèm nút mới vào đó. Nếu tìm thấy khóa trong quá trình tìm kiếm, hãy dừng lại vì các khóa trùng lặp sẽ bị bỏ qua. 
3. Đối với thao tác DEL, tìm kiếm khóa mục tiêu trong khi ghi nhớ nút cha. Nếu mất chìa khóa, hãy quay lại ngay. Nếu nút không có nút con, hãy xóa con trỏ cha tương ứng. Nếu nó có một con, hãy kết nối trực tiếp cha mẹ với con đó. 
4. Nếu nút bị xóa có hai nút con, hãy tìm nút ngoài cùng bên phải trong cây con bên trái của nó. Nút này là khóa lớn nhất nhỏ hơn khóa đã xóa, vì vậy việc thay thế nút đã xóa bằng nút này sẽ giữ cho thứ tự BST hợp lệ. Loại bỏ phần trước khỏi vị trí cũ của nó và kết nối lại các phần tử con. 
5. Đối với thao tác PRINT, hãy tạo biểu diễn đệ quy. Một nút bị thiếu sẽ đóng góp một chuỗi trống. Một chiếc lá chỉ đóng góp chìa khóa của nó. Bất kỳ nút nào khác đều đóng góp khóa của nó, theo sau là các biểu diễn của nút con của nó. 

Tại sao nó hoạt động: điều bất biến là sau mỗi lần sửa đổi, mọi nút vẫn thỏa mãn quy tắc sắp xếp cây tìm kiếm nhị phân. Các phần chèn giữ nguyên nó vì nút mới được đặt chính xác ở vị trí trống hợp lệ đầu tiên. Việc xóa sẽ giữ nguyên nó vì việc thay thế hai nút con bằng nút trước đó theo thứ tự sẽ giữ cho tất cả các giá trị của cây con bên trái nhỏ hơn và tất cả các giá trị của cây con bên phải đều lớn hơn. Vì việc in chỉ đọc cấu trúc được duy trì nên đầu ra luôn mô tả cây hiện tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1 << 25)

class Node:
    __slots__ = ("key", "left", "right")
    def __init__(self, key):
        self.key = key
        self.left = None
        self.right = None

root = None

def add(key):
    global root
    if root is None:
        root = Node(key)
        return

    cur = root
    while True:
        if key == cur.key:
            return
        if key < cur.key:
            if cur.left is None:
                cur.left = Node(key)
                return
            cur = cur.left
        else:
            if cur.right is None:
                cur.right = Node(key)
                return
            cur = cur.right

def delete(key):
    global root
    parent = None
    cur = root

    while cur is not None and cur.key != key:
        parent = cur
        if key < cur.key:
            cur = cur.left
        else:
            cur = cur.right

    if cur is None:
        return

    if cur.left is None:
        child = cur.right
        if parent is None:
            root = child
        elif parent.left is cur:
            parent.left = child
        else:
            parent.right = child
    elif cur.right is None:
        child = cur.left
        if parent is None:
            root = child
        elif parent.left is cur:
            parent.left = child
        else:
            parent.right = child
    else:
        pred_parent = cur
        pred = cur.left
        while pred.right is not None:
            pred_parent = pred
            pred = pred.right

        cur.key = pred.key

        if pred_parent.left is pred:
            pred_parent.left = pred.left
        else:
            pred_parent.right = pred.left

def build(cur):
    if cur is None:
        return ""
    if cur.left is None and cur.right is None:
        return str(cur.key)
    return str(cur.key) + "(" + build(cur.left) + "," + build(cur.right) + ")"

def solve(data):
    global root
    root = None
    ans = []

    for line in data.splitlines():
        if not line:
            continue
        parts = line.split()
        if parts[0] == "ADD":
            add(int(parts[1]))
        elif parts[0] == "DEL":
            delete(int(parts[1]))
        else:
            ans.append(build(root))

    return "\n".join(ans)

data = sys.stdin.read()
print(solve(data))
```các`Node`lớp chỉ lưu trữ ba mẩu thông tin cần thiết cho BST. Việc sử dụng tính năng chèn và xóa lặp đi lặp lại sẽ tránh được các vấn đề về độ sâu đệ quy khi cây trở thành một chuỗi. 

Thủ tục chèn đi xuống cây cho đến khi tìm thấy một con trỏ bị thiếu. Việc kiểm tra sự bằng nhau được thực hiện trước khi di chuyển, điều này ngăn chặn việc chèn các khóa trùng lặp. 

Xóa là phần tế nhị nhất. Việc tìm kiếm giữ lại nút gốc vì việc thay đổi cây yêu cầu sửa đổi con trỏ dẫn đến nút bị loại bỏ. Trong trường hợp hai con, phần trước được tìm thấy bằng cách di chuyển sang trái một lần rồi đi xa nhất có thể sang bên phải. Nút trước không thể có nút con phù hợp nên sau khi sao chép khóa của nó vào nút bị xóa có thể loại bỏ như trường hợp 0 ​​hoặc một nút con. 

Chức năng in có tính đệ quy vì định dạng được yêu cầu có tính đệ quy tự nhiên. Việc xử lý chuỗi trống là điều cho phép các phần tử con bị thiếu bên trong nút không phải lá xuất hiện chính xác, chẳng hạn như`5(3,)`. 

## Ví dụ đã hoạt động 

Hãy xem xét trình tự này:```
ADD 5
ADD 3
ADD 7
PRINT
DEL 5
PRINT
```Trạng thái thực thi là: 

| Bước | Hoạt động | Gốc | Cây in | 
| --- | --- | --- | --- | 
| 1 | THÊM 5 | 5 | | 
| 2 | THÊM 3 | 5 | | 
| 3 | THÊM 7 | 5 | | 
| 4 | IN | 5 | 5(3,7) | 
| 5 | DEL 5 | 7 | | 
| 6 | IN | 7 | 3(,7) | 

Việc xóa thay thế 5 bằng số 3 trước nó. Thứ tự cây con vẫn hợp lệ vì mọi thứ bên trái của 3 đều nhỏ hơn và mọi thứ bên phải của 3 đều lớn hơn. 

Một ví dụ thứ hai:```
ADD 8
ADD 2
ADD 4
ADD 10
DEL 2
PRINT
DEL 100
PRINT
```| Bước | Hoạt động | Gốc | Cây in | 
| --- | --- | --- | --- | 
| 1 | THÊM 8 | 8 | | 
| 2 | THÊM 2 | 8 | | 
| 3 | THÊM 4 | 8 | | 
| 4 | THÊM 10 | 8 | | 
| 5 | DEL 2 | 8 | | 
| 6 | IN | 8 | 8(4,10) | 
| 7 | DEL 100 | 8 | | 
| 8 | IN | 8 | 8(4,10) | 

Lần xóa thứ hai không có hiệu lực vì thiếu chìa khóa. Dấu vết xác nhận rằng các thao tác không hợp lệ sẽ khiến cây không thay đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(qh + P) | Mỗi bản cập nhật tuân theo một đường dẫn từ gốc đến lá. Chi phí in P, số nút được ghi trên tất cả các lệnh in. | 
| Không gian | O(n) | Mỗi khóa hiện có chiếm một nút. | 

Giải pháp phù hợp với các ràng buộc vì nó không bao giờ xây dựng lại cây. Việc truyền tải đầy đủ duy nhất xảy ra khi đầu ra yêu cầu rõ ràng toàn bộ cấu trúc. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    return solve(inp).strip()

assert run("""ADD 5
ADD 3
ADD 7
PRINT
""") == "5(3,7)", "basic insert"

assert run("""ADD 5
ADD 3
ADD 7
DEL 5
PRINT
""") == "3(,7)", "delete root with two children"

assert run("""PRINT
DEL 1
PRINT
""") == "", "empty tree"

assert run("""ADD 1
ADD 2
ADD 3
DEL 2
PRINT
""") == "1(,3)", "delete node with one child"

assert run("""ADD 10
ADD 5
ADD 15
ADD 12
DEL 100
PRINT
""") == "10(5,15(12,))", "delete missing key"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chèn ba nút |`5(3,7)`| Xây dựng BST cơ bản | 
| Loại bỏ gốc |`3(,7)`| Xóa hai con | 
| In cây trống | dòng trống | Đại diện trống | 
| Loại bỏ một nút chuỗi |`1(,3)`| Thay thế một đứa trẻ | 
| Xóa giá trị vắng mặt |`10(5,15(12,))`| Bỏ qua các hoạt động không hợp lệ | 

## Vỏ cạnh 

Đối với trường hợp cây trống, đầu vào```
DEL 10
PRINT
```bắt đầu không có root. Việc tìm kiếm xóa ngay lập tức không thành công nên thư mục gốc vẫn trống. Việc in gọi hàm đệ quy trên nút bị thiếu và trả về một chuỗi trống. 

Đối với việc xóa hai con, hãy xem xét:```
ADD 6
ADD 2
ADD 9
ADD 4
DEL 6
PRINT
```Nút 6 có con ở cả hai bên. Thuật toán tìm 4 là nút ngoài cùng bên phải của cây con bên trái, sao chép khóa của nó vào gốc và xóa vị trí 4 cũ. Đầu ra trở thành:```
4(2,9)
```Giải pháp đơn giản loại bỏ 6 sẽ làm mất cả hai cây con. 

Để chèn trùng lặp, hãy xem xét:```
ADD 5
ADD 5
PRINT
```Lần chèn thứ hai đến nút hiện có bằng phím 5 và dừng lại. Cây còn lại:```
5
```Điều này quan trọng vì việc chèn các bản sao sẽ phá vỡ giả định rằng mọi khóa đều xuất hiện một lần, điều mà logic xóa dựa vào.
