---
title: "CF 106431D - Đạo Cây (treaps)"
description: "Nhiệm vụ là duy trì cây tìm kiếm nhị phân với các quy tắc xử lý. Mỗi nút có một khóa xác định vị trí của nó trong cây tìm kiếm và giá trị độ cao hoạt động như mức độ ưu tiên."
date: "2026-06-25T09:39:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106431
codeforces_index: "D"
codeforces_contest_name: "Entrenamiento OIE Nivel Experto - Semana 12"
rating: 0
weight: 106431
solve_time_s: 35
verified: true
draft: false
---

[CF 106431D - Đạo Cây (treaps)](https://codeforces.com/problemset/problem/106431/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là duy trì cây tìm kiếm nhị phân với các quy tắc xử lý. Mỗi nút có một khóa xác định vị trí của nó trong cây tìm kiếm và giá trị độ cao hoạt động như mức độ ưu tiên. Chiều cao nhỏ hơn phải luôn gần gốc hơn, vì vậy mọi cha mẹ đều có chiều cao nhỏ hơn con cái của nó. 

Đầu vào là một chuỗi các lệnh được áp dụng cho cây trống ban đầu. Việc chèn sẽ cung cấp khóa và chiều cao mới, đồng thời phải đặt nút chính xác ở nơi quy tắc xử lý yêu cầu. Việc xóa sẽ loại bỏ một khóa trong khi vẫn giữ được cấu trúc còn lại. Thao tác in xuất ra cây bằng cách sử dụng biểu diễn đệ quy cần thiết, hiển thị mọi nút dưới dạng`key/height`và hiển thị đệ quy các phần tử con của nó. 

Khó khăn chính là việc chèn cây tìm kiếm nhị phân thông thường là không đủ. Việc thêm các nút ở các lá có thể vi phạm thứ tự chiều cao, do đó việc chèn phải xây dựng lại các phần của cây. Các ràng buộc cho phép thực hiện nhiều thao tác, với số phím lên tới$10^5$và độ cao lên tới$10^9$. Giải pháp đi qua mọi nút cho mỗi bản cập nhật sẽ trở thành giải pháp bậc hai khi nhiều lệnh được xử lý. Chúng ta cần mỗi sửa đổi để chỉ chạm vào chiều cao của cây, chiều cao này được kỳ vọng là logarit vì các trep vẫn cân bằng với xác suất cao. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Nếu nút được chèn có chiều cao nhỏ nhất thì nó sẽ trở thành nút gốc. Ví dụ:```
ADD 5 10
ADD 3 1
PRINT
```Đầu ra là:```
3/1(,5/10)
```Giải pháp luôn chèn theo khóa sẽ giữ sai`5`như là gốc. 

Việc xóa một nút có hai nút con đòi hỏi phải nối hai cạnh còn lại của nó chứ không chỉ đơn giản là xóa nút đó. Ví dụ:```
ADD 2 2
ADD 1 1
ADD 3 3
DEL 2
PRINT
```Đầu ra đúng là:```
1/1(,3/3)
```Việc xóa bất cẩn có thể để lại một cây không hợp lệ hoặc mất một cây con. 

Hoạt động phân chia cũng có một điều kiện biên. Chia theo khóa`k`phải đặt chìa khóa`k`chính nó trong kết quả bên trái. Nếu sự so sánh được thay đổi từ`<=`ĐẾN`<`, việc chèn và xóa xung quanh các khóa hiện có sẽ tạo ra cây không chính xác. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ lưu trữ cây dưới dạng cây tìm kiếm nhị phân thông thường và mô phỏng mọi thao tác theo cách đệ quy. Tìm kiếm theo khóa rất dễ dàng và việc chèn có thể đặt một nút dưới dạng một chiếc lá. Vấn đề là việc chèn lá sẽ bỏ qua độ cao. Một chuỗi các phím tăng dần với độ cao tăng dần sẽ tạo thành một chuỗi:```
1
 \
  2
   \
    3
     \
      4
```Sau đó$n$phần chèn thêm, một thao tác đơn lẻ có thể đi qua$O(n)$nút, đưa ra$O(n^2)$làm việc trên tất cả các lệnh. 

Quan sát quan trọng là các giá trị độ cao đã mô tả vị trí của các nút. Nút có chiều cao nhỏ nhất trong treap phải là nút gốc. Tất cả các nút có khóa nhỏ hơn thuộc về cây con bên trái của nó và tất cả các nút lớn hơn thuộc về cây con bên phải của nó. Điều này có nghĩa là một tre có thể được thay đổi bằng cách kết hợp hai thao tác nguyên thủy.`split`chia một treap thành hai treap hợp lệ xung quanh một khóa.`join`kết hợp hai kho dữ liệu hợp lệ trong đó mỗi khóa trong kho thứ nhất nhỏ hơn mọi khóa trong kho thứ hai. Cả hai thao tác đệ quy đều di chuyển xuống một đường dẫn, bởi vì mọi lệnh gọi đệ quy sẽ loại bỏ một bên của cây. Vì chiều cao dự kiến ​​của một treap là logarit nên các phép toán này rất hiệu quả. 

Việc chèn sẽ phân chia xung quanh khóa mới, tạo ra một nút đơn mới, sau đó nối các phần lại với nhau. Việc xóa sẽ trở thành hai phần tách biệt với nút mục tiêu, theo sau là một phần nối của các phần còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Chia/nối Treap | O(q log n) dự kiến ​​| O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Để chèn một nút`(k, h)`, chia kho hiện tại cho`k`. Cây bên trái chứa mọi khóa nhỏ hơn hoặc bằng`k`và cây bên phải chứa mọi khóa lớn hơn. Điều này tạo ra sự phân tách chính xác cần thiết cho cây tìm kiếm nhị phân. 
2. Tạo một kho lưu trữ mới chỉ chứa`(k, h)`. Nối phần bên trái với nút này, sau đó nối kết quả với phần bên phải. Thao tác nối chọn gốc bằng cách sử dụng độ cao, do đó điều kiện độ cao sẽ tự động được khôi phục. 
3. Để xóa phím`k`, chia cây thành`k`, tạo ra phần bên trái chứa các khóa lên tới`k`và phần bên phải chứa các phím lớn hơn. 
4. Chia phần bên trái cho`k - 1`. Phần phân chia thứ hai tách tất cả các khóa nhỏ hơn khỏi nút đơn có khóa`k`. 
5. Loại bỏ nút bị cô lập và nối hai cây còn lại. Vì mọi khóa ở cây bên trái đều nhỏ hơn mọi khóa ở cây bên phải nên kết quả là một bộ dữ liệu hợp lệ. 
6. Để in, in đệ quy nút hiện tại. Nếu thiếu một đứa trẻ, hãy in một vị trí trống. Ngược lại, in cây con con trong ngoặc đơn. 

Tại sao nó hoạt động: 

Điều bất biến là mọi cây con luôn thỏa mãn cả hai thuộc tính treap: khóa tuân theo thứ tự cây tìm kiếm nhị phân và chiều cao tuân theo thứ tự heap.`split`duy trì điều này vì nó chỉ thay đổi mối quan hệ cha-con trong khi xây dựng lại các đường dẫn bị ảnh hưởng một cách đệ quy.`join`giữ nguyên nó vì gốc phải là nút có chiều cao nhỏ nhất trong số hai cây đầu vào và các lệnh gọi đệ quy tiếp tục lý luận tương tự trên các cây con còn lại. Vì việc chèn và xóa chỉ bao gồm các thao tác hợp lệ này nên mọi cây trung gian và cây cuối cùng vẫn là một khối chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("key", "height", "left", "right")

    def __init__(self, key, height):
        self.key = key
        self.height = height
        self.left = None
        self.right = None

def join(a, b):
    if a is None:
        return b
    if b is None:
        return a

    if a.height < b.height:
        a.right = join(a.right, b)
        return a
    else:
        b.left = join(a, b.left)
        return b

def split(root, key):
    if root is None:
        return None, None

    if root.key <= key:
        left_part, right_part = split(root.right, key)
        root.right = left_part
        return root, right_part
    else:
        left_part, right_part = split(root.left, key)
        root.left = right_part
        return left_part, root

def add(root, key, height):
    left, right = split(root, key)
    return join(join(left, Node(key, height)), right)

def delete(root, key):
    middle, right = split(root, key)
    left, _ = split(middle, key - 1)
    return join(left, right)

def build_string(root):
    if root is None:
        return ""

    value = f"{root.key}/{root.height}"

    if root.left is None and root.right is None:
        return value

    return value + "(" + build_string(root.left) + "," + build_string(root.right) + ")"

def solve():
    root = None
    out = []

    for line in sys.stdin:
        if not line.strip():
            continue

        cmd = line.split()

        if cmd[0] == "ADD":
            root = add(root, int(cmd[1]), int(cmd[2]))
        elif cmd[0] == "DEL":
            root = delete(root, int(cmd[1]))
        else:
            out.append(build_string(root))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`Node`lớp chỉ lưu trữ thông tin cần thiết cho thuộc tính tre. Không cần có con trỏ gốc hoặc siêu dữ liệu bổ sung vì mọi sửa đổi đều có thể xây dựng lại đường dẫn bị ảnh hưởng theo cách đệ quy.`split`là hoạt động cốt lõi. Khi nút hiện tại thuộc về kết quả bên trái, nút con bên phải của nó phải được chia thêm, vì các khóa lớn hơn có thể vẫn cần phải di chuyển sang kết quả bên phải. Trường hợp đối xứng xử lý các nút lớn hơn khóa tách.`join`so sánh độ cao vì chiều cao nhỏ nhất phải trở thành gốc. Bên thua được gắn đệ quy bên dưới gốc đó. Hướng so sánh rất dễ bị đảo ngược một cách vô tình, điều này sẽ tạo ra vùng heap tối đa thay vì vùng heap tối thiểu cần thiết. 

Sử dụng xóa`split(root, key)`theo sau là`split(left_side, key - 1)`. Vì các khóa là số nguyên và duy nhất nên điều này sẽ tách biệt chính xác một nút. sử dụng`key`trong lần phân chia thứ hai sẽ khiến nút bị xóa không chính xác. 

Số nguyên Python không bị tràn nên giá trị chiều cao lớn sẽ an toàn. Độ sâu đệ quy dự kiến ​​sẽ duy trì ở mức logarit vì các trep được cân bằng với xác suất cao. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
ADD 1 23
ADD 2 19
ADD 4 95
ADD 6 13
```Trạng thái thay đổi như sau: 

| Hoạt động | Gốc | Cây con trái | Cây con bên phải | 
| --- | --- | --- | --- | 
| THÊM 1 23 | 23/1 | trống | trống | 
| THÊM 2 19 | 19/2 | 23/1 | trống | 
| THÊM 4 95 | 19/2 | 23/1 | 4/95 | 
| THÊM 6 13 | 13/6 | 19/2 | trống | 

Vị trí chèn cuối cùng`6`ở gốc vì chiều cao của nó là nhỏ nhất. Gốc trước đó trở thành một phần của cây con bên trái vì tất cả các khóa nhỏ hơn phải nằm ở bên đó. 

Một ví dụ khác:```
ADD 1 74
ADD 2 23
ADD 3 91
ADD 4 43
DEL 2
```| Hoạt động | Gốc | Thay đổi quan trọng | 
| --- | --- | --- | 
| THÊM 1 74 | 1/74 | Nút ban đầu | 
| THÊM 2 23 | 23/2 | Chiều cao mới nhỏ hơn trở thành root | 
| THÊM 3 91 | 23/2 | Đã thêm vào bên phải | 
| THÊM 4 43 | 23/2 | Chiều cao 43 di chuyển trên 3/91 | 
| DEL 2 | 43/4 | Phần bên trái và bên phải được nối với nhau | 

Điều này chứng tỏ rằng việc xóa không thay thế một nút một cách tùy tiện. Thao tác nối sẽ xây dựng lại tre duy nhất thỏa mãn cả hai quy tắc sắp xếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n) dự kiến ​​| Mỗi lần chia và nối đều tuân theo một đường dẫn treap và mỗi lệnh sử dụng một số lượng đường dẫn không đổi. | 
| Không gian | O(n) | Mỗi nút được lưu trữ một lần, với phép đệ quy sử dụng không gian ngăn xếp logarit dự kiến. | 

Các ràng buộc yêu cầu tránh quét tuyến tính. Hành vi logarit dự kiến ​​​​của treap làm cho mọi lệnh đều đủ hiệu quả đối với kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().splitlines()
    sys.stdin = old

    root = None
    ans = []

    for line in data:
        cmd = line.split()
        if not cmd:
            continue
        if cmd[0] == "ADD":
            root = add(root, int(cmd[1]), int(cmd[2]))
        elif cmd[0] == "DEL":
            root = delete(root, int(cmd[1]))
        else:
            ans.append(build_string(root))

    return "\n".join(ans)

assert run("""ADD 1 23
PRINT
ADD 2 19
PRINT
""") == """1/23
2/19(1/23,)"""

assert run("""ADD 1 1
ADD 2 2
ADD 3 3
ADD 4 4
ADD 5 5
PRINT
""") == "1/1(,2/2(,3/3(,4/4(,5/5))))"

assert run("""ADD 5 10
ADD 3 1
PRINT
""") == "3/1(,5/10)"

assert run("""ADD 2 2
ADD 1 1
ADD 3 3
DEL 2
PRINT
""") == "1/1(,3/3)"

assert run("""PRINT
""") == ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai phần chèn tăng dần |`2/19(1/23,)`| Thay thế gốc theo chiều cao | 
| Tăng phím và độ cao | Một món quà hình dây chuyền | Hành vi phân chia và nối ranh giới | 
| Chiều cao tối thiểu mới |`3/1(,5/10)`| Chèn một gốc mới | 
| Xóa root bằng 2 con |`1/1(,3/3)`| Xóa đúng thông qua các phép nối | 
| Bản in trống | Đầu ra trống | Xử lý cây trống | 

## Vỏ cạnh 

Khi một nút mới có chiều cao nhỏ nhất, mọi nút hiện có phải di chuyển xuống dưới nó. Đối với đầu vào:```
ADD 5 10
ADD 3 1
PRINT
```Sự phân chia đầu tiên tách cây cũ khỏi vị trí khóa mới. Nút mới có chiều cao`1`, Vì thế`join`làm cho nó trở thành gốc. Đầu ra là:```
3/1(,5/10)
```Khi xóa một nút có hai nút con thì cả hai nút còn lại đều phải tồn tại. Vì:```
ADD 2 2
ADD 1 1
ADD 3 3
DEL 2
PRINT
```Sự phân chia đầu tiên cô lập phần gốc và phần bên phải. Phần chia thứ hai tách con trái. Việc nối các cây còn lại sẽ tạo ra:```
1/1(,3/3)
```Thuật toán không bao giờ chọn thay thế theo cách thủ công, vì vậy nó tránh được việc phá vỡ thứ tự chiều cao. 

Một lỗi thường gặp là sự phân tách thứ hai trong quá trình xóa. Lệnh:```
DEL 2
```chỉ phải xóa chìa khóa`2`, không phải tất cả các phím cho đến`2`. Việc phân chia thứ hai sử dụng`key - 1`, để lại mọi khóa nhỏ hơn ở cây bên trái và chỉ cách ly nút đích. Điều này bảo tồn tất cả các khóa và giữ cho phép nối cuối cùng hợp lệ.
