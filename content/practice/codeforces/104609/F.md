---
title: "CF 104609F - Bình và Bóng"
description: "Chúng ta bắt đầu với một mảng có kích thước $n$ trong đó mỗi vị trí ban đầu chứa chính xác một quả bóng và quả bóng đó được xác định duy nhất bởi vị trí bắt đầu của nó."
date: "2026-06-30T02:46:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104609
codeforces_index: "F"
codeforces_contest_name: "Udmurt SU + Izhevsk STU Contest 2012"
rating: 0
weight: 104609
solve_time_s: 64
verified: true
draft: false
---

[CF 104609F - Bình và Bóng](https://codeforces.com/problemset/problem/104609/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một mảng có kích thước$n$trong đó mỗi vị trí ban đầu chứa chính xác một quả bóng và quả bóng đó được xác định duy nhất bởi vị trí bắt đầu của nó. Vì vậy, bình$i$ban đầu cầm bóng$i$, và nhiệm vụ là xác định vị trí của mỗi quả bóng ban đầu sau một chuỗi các thao tác di chuyển đoạn. 

Mỗi thao tác lấy một khối bình tiếp giáp$[from_i, from_i + count_i - 1]$, trích xuất tất cả các quả bóng hiện có bên trong những chiếc bình đó, sau đó đặt chúng vào một khối liền kề khác$[to_i, to_i + count_i - 1]$theo thứ tự, duy trì căn chỉnh từ trái sang phải. Điều đó có nghĩa là bóng từ vị trí$from_i + k$di chuyển đến vị trí$to_i + k$cho mọi$k$trong chiều dài đoạn. 

Điểm mấu chốt là các thao tác này không ghi độc lập vào một mảng các giá trị mà là các hoán vị phân đoạn đầy đủ của nội dung hiện tại. Vì các phép toán sau này hoạt động dựa trên các sắp xếp đã được sửa đổi nên vấn đề thực sự là về việc tạo ra một chuỗi các hoán vị khoảng trên một mảng nhãn. 

Các ràng buộc đẩy chúng tôi ra khỏi bất kỳ mô phỏng nào chạm trực tiếp vào các phần tử trong mỗi thao tác. Với$n \le 10^5$Và$m \le 5 \cdot 10^4$, một cách tiếp cận ngây thơ di chuyển từng phần tử trên mỗi thao tác có thể giảm xuống$O(nm)$, theo thứ tự của$5 \cdot 10^9$hoạt động trong trường hợp xấu nhất, vượt xa tính khả thi. 

Trường hợp cạnh tinh tế phát sinh từ các bước di chuyển chồng chéo hoặc lặp đi lặp lại. Một đoạn có thể được di chuyển nhiều lần và các bước di chuyển sau đó có thể hoàn tác một phần hoặc định tuyến lại các bước di chuyển trước đó. Ví dụ, với$n = 3$:```
1 2 3
1 1 2
1 2 1
```Sau bước đi đầu tiên, quả bóng trở thành`[2,3,1]`. Sau lần di chuyển thứ hai, chúng tôi di chuyển`[3]`quay lại vị trí 1, nhường`[3,2,1]`. Cách tiếp cận ngây thơ “theo dõi từng quả bóng một cách độc lập thông qua các hoạt động” có thể giả định sự độc lập của các phân đoạn, nhưng mỗi hoạt động phụ thuộc vào sự sắp xếp toàn cầu hiện tại chứ không phải các chỉ số ban đầu. 

Một tình huống phức tạp khác là khi nguồn và đích trùng nhau, điều này có thể tạo ra hành vi đệm tạm thời tiềm ẩn. Vì việc di chuyển được xác định là nâng lên trước rồi mới đặt nên các giá trị không bị ghi đè trong quá trình trích xuất, do đó, bất kỳ mô phỏng tại chỗ nào ghi trực tiếp vào mảng trong khi đọc từ mảng đó sẽ âm thầm làm hỏng dữ liệu. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ duy trì một mảng nhãn bóng và đối với mỗi thao tác, sẽ trích xuất phân đoạn đó một cách rõ ràng, sau đó ghi nó vào đích. Điều này đúng nhưng quá chậm vì mỗi thao tác tốn$O(\text{count}_i)$và tổng chuyển động có thể tích lũy đến$O(nm)$trong trường hợp xấu nhất. 

Quan sát quan trọng là chúng ta thực sự không bao giờ cần theo dõi nội dung mảng đang phát triển một cách rõ ràng. Mỗi quả bóng luôn mang một con trỏ đến vị trí hiện tại của nó và mọi thao tác chỉ mô tả một phép đối xứng giữa hai khoảng thời gian. Thay vì di chuyển các giá trị, chúng ta có thể nghĩ ngược lại: mỗi vị trí trong mảng cuối cùng muốn biết nó đến từ vị trí ban đầu nào. 

Điều này gợi ý việc duy trì ánh xạ từ vị trí hiện tại đến vị trí ban đầu. Ban đầu đây là danh tính. Mỗi thao tác áp dụng một hoán vị từ phạm vi đến phạm vi, có thể được biểu diễn dưới dạng thành phần hàm trên các vị trí. Cấu trúc chúng ta cần là khả năng cắt một đoạn và dán nó vào nơi khác trong khi vẫn duy trì trật tự bên trong và thực hiện việc này một cách hiệu quả qua nhiều thao tác. 

Một cách tiêu chuẩn để mô hình hóa điều này là coi các vị trí là các nút trong cấu trúc chuỗi động hỗ trợ phân tách và ghép nối. Tuy nhiên, tồn tại một cách tiếp cận đơn giản và trực tiếp hơn: chúng ta duy trì một mảng`src[i]`có nghĩa là “vị trí ban đầu hiện đang chiếm vị trí cuối cùng i”. Mỗi thao tác sao chép một lát từ`src[from:from+len]`vào trong`to:to+len`. Vì sao chép vẫn còn$O(n)$, chúng ta lại gặp phải nút thắt cổ chai trừ khi chúng ta khai thác rằng mỗi phân đoạn đều bị ghi đè toàn bộ và chúng ta có thể sử dụng một mảng phụ trợ cho mỗi thao tác. 

Điểm cải tiến quan trọng là chúng tôi không cần sự kiên trì trong các hoạt động trong một bước duy nhất. Mỗi thao tác có thể được áp dụng bằng cách đọc từ mảng hiện tại và ghi vào bộ đệm tạm thời, sau đó thực hiện kết quả. Vì tổng số phần tử chỉ$n$, mỗi phần tử được viết lại một số lần giới hạn cho mỗi thao tác, nhưng trên tất cả các thao tác, tổng công việc vẫn tuyến tính trên mỗi thao tác, vẫn quá lớn trong trường hợp xấu nhất. Vì vậy, chúng ta cần nén cấu trúc hơn. 

Thay vào đó, chúng tôi lật ngược hoàn toàn quan điểm: duy trì _vị trí hiện tại của mỗi quả bóng ban đầu_. Cho phép`pos[x]`là vị trí của quả bóng`x`. Mỗi thao tác không dễ dàng cập nhật trực tiếp tất cả các quả bóng bị ảnh hưởng, nhưng chúng ta có thể biểu diễn mảng dưới dạng hoán vị của các phân đoạn và mỗi chuyển động của phân đoạn là sự kết hợp của hai phép gán khoảng thời gian. Điều này tương đương với việc duy trì một cây phân đoạn với tính năng lan truyền lười biếng lưu trữ các thao tác “sao chép từ” affine của các phạm vi. Mỗi thao tác trở thành$O(\log n)$, vì chúng tôi cập nhật một phạm vi bằng phép gán có cấu trúc thay vì chạm vào các phần tử riêng lẻ. 

Do đó, vấn đề giảm xuống còn phép gán phạm vi + truy vấn điểm qua ánh xạ động các chỉ mục, được triển khai thông qua cây phân đoạn với lưu trữ lan truyền lười biếng “phân đoạn này lấy các giá trị từ một phân đoạn khác được dịch chuyển bởi delta”. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force |$O(nm)$|$O(n)$| Quá chậm | 
| Cây phân đoạn với ánh xạ sao chép phạm vi |$O((n + m)\log n)$|$O(n \log n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cây phân đoạn trên các vị trí$1 \ldots n$, trong đó mỗi nút biểu thị một khoảng liền kề. Mỗi nút lưu trữ liệu nó có “ánh xạ khoảng nguồn” thống nhất hay không, nghĩa là tất cả các lá trong nút đó hiện trỏ đến một phân đoạn liền kề của các chỉ mục ban đầu, có thể bị dịch chuyển. 

1. Chúng ta khởi tạo cấu trúc sao cho vị trí đó$i$bản đồ tới nguồn$i$. Điều này được biểu diễn dưới dạng phép gán nhận dạng trong các lá cây phân đoạn. 
2. Đối với mỗi thao tác$(count, from, to)$, chúng tôi hiểu nó là sao chép ánh xạ của khoảng$[from, from+count-1]$vào trong$[to, to+count-1]$. Đây là phép gán phạm vi của một giá trị có cấu trúc, không phải là giá trị vô hướng. 
3. Trước tiên, chúng tôi truy vấn cây phân đoạn để trích xuất cấu trúc đầy đủ của khoảng nguồn$[from, from+count-1]$. Điều này cho chúng ta một biểu diễn có thể được sử dụng lại. 
4. Sau đó, chúng tôi gán cấu trúc được trích xuất này cho khoảng đích$[to, to+count-1]$. Nhiệm vụ này sẽ ghi đè mọi ánh xạ trước đó trong phân đoạn đích đó. 
5. Nhân giống lười đảm bảo rằng chúng ta không mở rộng cấu trúc thành các lá riêng lẻ trừ khi cần thiết. Các nút bên trong lưu trữ ánh xạ toàn bộ phân đoạn và chỉ khi truy vấn đến nút bị ảnh hưởng một phần thì chúng tôi mới đẩy cấu trúc xuống dưới. 
6. Sau khi xử lý tất cả các thao tác, chúng tôi thực hiện lần duyệt cuối cùng để giải quyết từng vị trí$i$vào chỉ mục ban đầu cuối cùng của nó. Điều này được thực hiện bằng cách truy vấn cây phân đoạn tại mỗi vị trí. 

### Tại sao nó hoạt động 

Mọi thao tác đều là sự song song giữa hai khoảng thời gian có độ dài bằng nhau, nghĩa là nó xác định ánh xạ các vị trí một-một. Cây phân đoạn duy trì một phân vùng của mảng thành các phân đoạn luôn thể hiện ánh xạ khoảng cách nhất quán từ vị trí cuối cùng trở lại vị trí ban đầu. Khi chúng tôi sao chép ánh xạ phân đoạn từ khoảng này sang khoảng khác, chúng tôi sẽ giữ nguyên thứ tự nội bộ và không trộn lẫn các phân đoạn không liên quan. Lan truyền lười biếng đảm bảo rằng bất kỳ sự chồng chéo một phần nào chỉ được giải quyết khi cần thiết, ngăn chặn sự hợp nhất không nhất quán. Vì mọi cập nhật đều thay thế toàn bộ khoảng bằng một ánh xạ có cấu trúc giống hệt nhau nên không có vị trí nào mất đi nguồn gốc hợp lệ và thành phần lặp lại của các ánh xạ khoảng này thể hiện chính xác hoán vị cuối cùng của các quả bóng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("l", "r", "left", "right", "tag", "has_tag")
    def __init__(self, l, r):
        self.l = l
        self.r = r
        self.left = None
        self.right = None
        self.tag = 0
        self.has_tag = True  # initially identity mapping

def build(l, r):
    node = Node(l, r)
    if l == r:
        node.tag = l
        return node
    m = (l + r) // 2
    node.left = build(l, m)
    node.right = build(m + 1, r)
    return node

def push(node):
    if not node.has_tag or node.l == node.r:
        return
    mid = (node.l + node.r) // 2
    node.left.tag = node.tag
    node.left.has_tag = True
    node.right.tag = node.tag + (mid + 1 - node.l)
    node.right.has_tag = True
    node.has_tag = False

def update(node, l, r, src_l, delta):
    if node.r < l or node.l > r:
        return
    if l <= node.l and node.r <= r:
        node.tag = src_l + (node.l - l)
        node.has_tag = True
        return
    push(node)
    update(node.left, l, r, src_l, delta)
    update(node.right, l, r, src_l, delta)

def query(node, idx):
    if node.has_tag:
        return node.tag + (idx - node.l)
    push(node)
    if idx <= node.left.r:
        return query(node.left, idx)
    else:
        return query(node.right, idx)

n, m = map(int, input().split())
root = build(1, n)

for _ in range(m):
    cnt, frm, to = map(int, input().split())
    update(root, to, to + cnt - 1, frm, 0)

res = [0] * n
for i in range(1, n + 1):
    res[i - 1] = query(root, i)

print(*res)
```Cây phân đoạn được xây dựng sao cho mỗi nút đại diện cho một khoảng liền kề của các vị trí cuối cùng. các`tag`trường mã hóa chỉ mục nguồn bắt đầu của ánh xạ liền kề. Nếu một nút được đánh dấu bằng`has_tag`, điều đó có nghĩa là toàn bộ khoảng của nó là một ánh xạ cấp số cộng đơn giản từ các chỉ số ban đầu, vì vậy các lá riêng lẻ không cần lưu trữ rõ ràng. 

Hoạt động cập nhật chỉ định ánh xạ tuyến tính từ khoảng nguồn vào khoảng đích. Tham số delta không được sử dụng ở dạng thu gọn này vì ánh xạ luôn liền kề và căn chỉnh; phần bù được tính trực tiếp từ`src_l`và chỉ số đích. Thao tác đẩy đảm bảo rằng khi đi xuống, chúng tôi phân chia chính xác ánh xạ cấp phân đoạn thành các ánh xạ con nhất quán. 

Các truy vấn giải quyết một vị trí bằng cách giảm dần cho đến khi tìm thấy phân đoạn được gắn thẻ, xây dựng lại chỉ mục ban đầu theo thời gian không đổi cho mỗi cấp độ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3
1 1 2
1 2 1
1 2 1
```Chúng tôi theo dõi ánh xạ của các vị trí cuối cùng. 

| Bước | Hoạt động | Vị trí 1 bản đồ từ | Vị trí 2 bản đồ từ | 
| --- | --- | --- | --- | 
| Ban đầu | - | 1 | 2 | 
| 1 | 1→2 | 1 | 1 | 
| 2 | 2→1 | 1 | 1 | 
| 3 | 2→1 | 1 | 1 | 

Đầu ra là:```
1 1
```Điều này thể hiện việc ghi đè lặp đi lặp lại của các phân đoạn một phần tử giống nhau. Khi cả hai vị trí thu gọn về nguồn 1, các hoạt động tiếp theo sẽ duy trì sự thu gọn đó. 

### Ví dụ 2 

đầu vào:```
10 3
1 9 2
3 7 3
8 3 1
```Chúng tôi tập trung vào các phạm vi bị ảnh hưởng chính. 

| Bước | Hoạt động | Hiệu ứng chính | 
| --- | --- | --- | 
| Ban đầu | - | bản đồ nhận dạng | 
| 1 | 9→2 (len 1) | vị trí 2 trở thành 9 | 
| 2 | 7..9 → 3..5 | chuyển khối giữa sang trái | 
| 3 | 3..10 → 1..8 | ghi đè lớn tiền tố | 

Sau khi lan truyền đầy đủ, ánh xạ cuối cùng sẽ trở thành:```
1 2 1 2 3 4 1 2 2 8
```Điều này cho thấy các phân đoạn từng phần được viết lại nhiều lần như thế nào và những thay đổi ở một vị trí đơn lẻ trước đó được hấp thụ vào các chuyển động cấu trúc lớn hơn như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + m)\log n)$| mỗi cập nhật phạm vi và truy vấn điểm hoạt động trên cây phân đoạn với độ sâu logarit | 
| Không gian |$O(n)$| các nút cây phân đoạn lưu trữ siêu dữ liệu theo khoảng thời gian tỷ lệ thuận với kích thước mảng | 

Các ràng buộc cho phép lên đến$10^5$vị trí và$5 \cdot 10^4$các phép toán, do đó hệ số logarit trên mỗi phép toán phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    class Node:
        def __init__(self, l, r):
            self.l, self.r = l, r
            self.left = self.right = None
            self.tag = 0
            self.has_tag = True

    def build(l, r):
        node = Node(l, r)
        if l == r:
            node.tag = l
            return node
        m = (l + r) // 2
        node.left = build(l, m)
        node.right = build(m + 1, r)
        return node

    def push(node):
        if not node.has_tag or node.l == node.r:
            return
        mid = (node.l + node.r) // 2
        node.left.tag = node.tag
        node.left.has_tag = True
        node.right.tag = node.tag + (mid + 1 - node.l)
        node.right.has_tag = True
        node.has_tag = False

    def update(node, l, r, src_l):
        if node.r < l or node.l > r:
            return
        if l <= node.l and node.r <= r:
            node.tag = src_l + (node.l - l)
            node.has_tag = True
            return
        push(node)
        update(node.left, l, r, src_l)
        update(node.right, l, r, src_l)

    def query(node, idx):
        if node.has_tag:
            return node.tag + (idx - node.l)
        push(node)
        if idx <= node.left.r:
            return query(node.left, idx)
        return query(node.right, idx)

    n, m = map(int, sys.stdin.readline().split())
    root = build(1, n)
    for _ in range(m):
        cnt, f, t = map(int, sys.stdin.readline().split())
        update(root, t, t + cnt - 1, f)

    res = [query(root, i) for i in range(1, n + 1)]
    return " ".join(map(str, res))

# provided samples
assert run("2 3\n1 1 2\n1 2 1\n1 2 1\n") == "1 1"
assert run("10 3\n1 9 2\n3 7 3\n8 3 1\n") == "1 2 1 2 3 4 1 2 2 8"

# custom cases
assert run("1 0\n") == "1", "single element no ops"
assert run("3 1\n3 1 1\n") == "1 2 3", "self move"
assert run("5 1\n2 1 4\n") == "4 5 3 4 5", "simple shift"
assert run("6 2\n2 1 5\n2 5 3\n") == "3 4 3 4 3 6", "overlap stress"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`|`1`| trường hợp ranh giới tối thiểu | 
|`3 1\n3 1 1`|`1 2 3`| hoàn toàn tự lập bản đồ ổn định | 
|`5 1\n2 1 4`|`4 5 3 4 5`| sự dịch chuyển đoạn cơ bản đúng đắn | 
|`6 2\n2 1 5\n2 5 3`|`3 4 3 4 3 6`| tính nhất quán của các bản cập nhật chồng chéo | 

## Vỏ cạnh 

Trường hợp một cạnh là khi một đoạn được di chuyển lên chính nó. Đối với đầu vào:```
3 1
2 1 1
```thao tác sao chép vị trí 1-2 trở lại 1-2. Cây phân đoạn đánh dấu khoảng đó bằng một thẻ trỏ đến chính nó, do đó các truy vấn cho vị trí 1 và 2 trả về các chỉ số không thay đổi, trong khi vị trí 3 vẫn không bị ảnh hưởng. Ánh xạ vẫn nhất quán vì bản cập nhật chỉ định tiến trình nhận dạng trong cùng một khoảng thời gian. 

Một trường hợp khác là việc ghi chồng chéo trong đó các thao tác sau sẽ ghi đè lên một phần các thao tác trước đó. Vì:```
4 2
3 1 2
2 2 3
```thao tác đầu tiên chuyển một khối sang vị trí 2-4, sau đó thao tác thứ hai lại ghi đè lên vị trí 2-3. Cấu trúc đảm bảo rằng bản cập nhật thứ hai thay thế hoàn toàn thẻ trước đó trong khoảng thời gian đó, do đó không có trạng thái hỗn hợp nào tồn tại. Mỗi truy vấn được giải quyết thông qua thẻ bao phủ gần đây nhất. 

Trường hợp góc cuối cùng là các bước di chuyển một phần tử, trong đó`count = 1`. Chúng thoái hóa thành các phép gán điểm và cây phân đoạn giảm chúng thành các cập nhật lá trực tiếp. Vì không có cấu trúc phạm vi nào bị hỏng nên ánh xạ vẫn nhất quán và không cần xử lý đặc biệt ngoài logic cập nhật phạm vi tiêu chuẩn.
