---
title: "CF 102606F - Tìm / -type f -or -type d"
description: "Đầu vào mô tả ảnh chụp nhanh hệ thống tập tin. Mỗi dòng là một đường dẫn tuyệt đối đại diện cho một tệp hoặc một thư mục, nhưng các dòng được trộn theo thứ tự ngẫu nhiên."
date: "2026-08-04T17:03:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102606
codeforces_index: "F"
codeforces_contest_name: "2020 ECNU Campus Online Invitational Contest"
rating: 0
weight: 102606
solve_time_s: 66
verified: true
draft: false
---

[CF 102606F - Tìm / -type f -or -type d](https://codeforces.com/problemset/problem/102606/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào mô tả ảnh chụp nhanh hệ thống tập tin. Mỗi dòng là một đường dẫn tuyệt đối đại diện cho một tệp hoặc một thư mục, nhưng các dòng được trộn theo thứ tự ngẫu nhiên. Một thư mục chỉ xuất hiện vì có một số tệp tồn tại bên trong nó, vì vậy mọi thư mục trong danh sách đều là tổ tiên của ít nhất một tệp được liệt kê. Nhiệm vụ là khôi phục có bao nhiêu mục được liệt kê là các tệp có tên kết thúc bằng`.eoj`. 

Khó khăn là đầu vào không cho chúng ta biết đường dẫn nào là tệp và đường dẫn nào là thư mục. Một con đường kết thúc ở`.eoj`không tự động là một câu trả lời hợp lệ vì một thư mục cũng có thể có hậu tố đó. Ví dụ,`/a.eoj/b`chứng minh rằng`/a.eoj`là một thư mục, không phải là một tập tin. 

Kích thước đầu vào đủ lớn để mọi đường dẫn phải được xử lý hiệu quả. Có thể có 100000 đường dẫn và tổng số ký tự trên tất cả các đường dẫn tối đa là 1000000. Điều này loại trừ các phương pháp so sánh từng cặp đường dẫn, vì điều đó có thể yêu cầu khoảng 10^10 thao tác. Cần có một cách tiếp cận gần tuyến tính trong tổng kích thước đầu vào. 

Các trường hợp phức tạp xuất phát từ việc nhầm lẫn tên với loại tệp. Coi như:```
1
/a.eoj
```Câu trả lời là`1`bởi vì không có đứa trẻ nào dưới`/a.eoj`, vì vậy mục này phải là một tập tin. 

Bây giờ hãy xem xét:```
2
/a.eoj
/a.eoj/b
```Câu trả lời là`0`. Một giải pháp bất cẩn chỉ kiểm tra hậu tố sẽ được tính`/a.eoj`, nhưng con đường thứ hai chứng minh rằng`/a.eoj`là một thư mục. 

Một trường hợp khác là:```
2
/a.eoj/b.eoj
/a.eoj
```Câu trả lời là`1`. con đường`/a.eoj`là một thư mục mặc dù nó kết thúc bằng`.eoj`, trong khi`/a.eoj/b.eoj`là một tập tin. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản là lưu trữ tất cả các đường dẫn và với mỗi đường dẫn kết thúc bằng`.eoj`, tìm kiếm đầu vào để kiểm tra xem liệu một đường dẫn khác có tiền tố theo sau là dấu gạch chéo hay không. Nếu tồn tại một đường dẫn dài hơn thì ứng viên là một thư mục. Điều này đúng vì mỗi thư mục phải có ít nhất một thư mục con. Tuy nhiên, với 100000 đường dẫn, điều này có thể yêu cầu so sánh từng cặp đường dẫn. Ngay cả khi bỏ qua chi phí so sánh chuỗi, điều này mang lại khoảng 10^10 kiểm tra, quá chậm. 

Quan sát quan trọng là chúng ta không cần phải kiểm tra mọi mối quan hệ tổ tiên có thể có. Thông tin duy nhất cần thiết là liệu một đường dẫn có con hay không. Một đường dẫn là một thư mục chính xác khi có ít nhất một đường dẫn khác bắt đầu bằng đường dẫn đó, theo sau là`/`. 

Điều này biến vấn đề thành một vấn đề tiền tố. Nếu chúng ta chèn mọi đường dẫn vào một tri, thì mỗi nút có con sẽ đại diện cho một thư mục. Mỗi nút lá đại diện cho một đường dẫn không có nút con, đường dẫn này phải là một tệp. Sau đó, chúng tôi chỉ đếm các nút lá có đường dẫn đầy đủ kết thúc bằng`.eoj`. 

Trie phù hợp một cách tự nhiên vì đầu vào đã được phân cấp. Tiền tố thư mục dùng chung được lưu trữ một lần và tổng lượng công việc tỷ lệ thuận với tổng số ký tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²L) | O(nL) | Quá chậm | 
| Trí | O(S) | O(S) | Đã chấp nhận | 

Đây,`L`là độ dài đường dẫn tối đa và`S`là tổng số ký tự trong tất cả các đường dẫn. 

## Hướng dẫn thuật toán 

1. Chia mọi đường dẫn thành các thành phần của nó và chèn các thành phần vào một thử nghiệm. Mỗi cạnh trie thể hiện việc di chuyển vào một thành phần tên thư mục hoặc tệp. 

Trie lưu trữ mối quan hệ cha-con ẩn bên trong danh sách được xáo trộn. Thứ tự đầu vào không còn quan trọng nữa vì các đường dẫn tự động kết nối với tổ tiên của chúng trong quá trình chèn. 

1. Đánh dấu nút đại diện cho điểm cuối của mỗi đường dẫn đầu vào hoàn chỉnh. 

Chỉ các nút tương ứng với các mục thực tế từ đầu vào mới được tính. Các nút trie trung gian chỉ có thể tồn tại vì chúng là tổ tiên của các đường dẫn dài hơn. 

1. Đi qua thử sau khi thi công. Đối với mỗi nút được đánh dấu, hãy kiểm tra xem nó có nút con nào không. 

Một nút được đánh dấu có các nút con đại diện cho một thư mục vì một đường dẫn đầu vào khác tiếp tục bên dưới nó. Một nút được đánh dấu không có nút con đại diện cho một tệp vì không có gì tồn tại bên dưới nó. 

1. Đối với mỗi nút lá được đánh dấu, hãy xây dựng lại hoặc lưu trữ tên đường dẫn đầy đủ của nó và kiểm tra xem thành phần cuối cùng có kết thúc bằng`.eoj`. Tăng câu trả lời nếu có. 

Hậu tố chỉ thuộc về thành phần cuối cùng. Tên thư mục kết thúc bằng`.eoj`bị bỏ qua vì nó không thể là một chiếc lá. 

Tại sao nó hoạt động: 

Điều bất biến là mọi nút trie có nút con tương ứng với một đường dẫn có ít nhất một đường dẫn đầu vào dài hơn bên dưới nó. Vì các thư mục không thể tồn tại nếu không có các tập tin bên trong chúng nên các nút đó phải là các thư mục. Ngược lại, một nút được đánh dấu không có nút con thì không có nút con trong đầu vào, vì vậy nó không thể là một thư mục mà phải là một tệp. Thuật toán đếm chính xác các mục lá có phần mở rộng được yêu cầu, phù hợp với định nghĩa của câu trả lời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("children", "terminal", "name")

    def __init__(self, name=""):
        self.children = {}
        self.terminal = False
        self.name = name

def solve():
    n = int(input())
    root = Node()

    for _ in range(n):
        path = input().strip()
        parts = path.split("/")[1:]
        cur = root
        for part in parts:
            if part not in cur.children:
                cur.children[part] = Node(part)
            cur = cur.children[part]
        cur.terminal = True

    ans = 0

    def dfs(node):
        nonlocal ans
        if node.terminal and not node.children and node.name.endswith(".eoj"):
            ans += 1
        for child in node.children.values():
            dfs(child)

    dfs(root)
    print(ans)

if __name__ == "__main__":
    solve()
```Giai đoạn chèn tạo một nút trie cho mỗi thành phần đường dẫn duy nhất bên dưới nút gốc của nó. Thành phần đầu tiên được chèn vào bên dưới gốc vì dấu gạch chéo ở đầu không phải là tên thư mục có ý nghĩa. 

các`terminal`cờ phân tách các đường dẫn thực sự xuất hiện trong đầu vào với các nút chỉ được tạo làm tổ tiên. Điều này quan trọng vì một thư mục có thể xuất hiện dưới dạng một mục được liệt kê, nhưng chỉ riêng một nút trung gian thì không đại diện cho một tệp hoặc thư mục trong câu trả lời. 

Trong DFS, điều kiện`not node.children`xác định lá. Việc kiểm tra hậu tố chỉ được áp dụng sau khi xác nhận nút là ứng cử viên tệp. Số nguyên Python có độ chính xác tùy ý, do đó không cần xử lý tràn cho số câu trả lời. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
/secret/eoj
/secret
/secret.eoj
```Trạng thái trie sau khi chèn là: 

| Nút đường dẫn | Có con | Nhà ga | Đã tính | 
| --- | --- | --- | --- | 
| bí mật | vâng | vâng | không | 
| bí mật/eoj | không | vâng | không | 
| bí mật.eoj | không | vâng | vâng | 

Thư mục`/secret`bị từ chối vì nó có con. tập tin`/secret/eoj`không có hậu tố cần thiết. Chỉ một`/secret.eoj`góp phần vào câu trả lời, vì vậy kết quả là`1`. 

Đối với mẫu thứ hai:```
/cuber.eoj/qq.eoj
/cuber.eoj
```Trạng thái trie là: 

| Nút đường dẫn | Có con | Nhà ga | Đã tính | 
| --- | --- | --- | --- | 
| Cuber.eoj | vâng | vâng | không | 
| cuber.eoj/qq.eoj | không | vâng | vâng | 

Mặc dù`/cuber.eoj`kết thúc bằng`.eoj`, nó có một phần tử con và là một thư mục. Chiếc lá`/cuber.eoj/qq.eoj`là tập tin duy nhất được tính, đưa ra câu trả lời`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(S) | Mỗi ký tự được chèn một lần và mỗi nút trie được truy cập một lần | 
| Không gian | O(S) | Trie lưu trữ cấu trúc đường dẫn | 

Tổng độ dài đầu vào tối đa là 1000000 ký tự, do đó, giải pháp tuyến tính sẽ phù hợp thoải mái với các ràng buộc. Trie tránh so sánh tiền tố lặp lại và sử dụng trực tiếp cấu trúc hệ thống tập tin. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.readline

    class Node:
        def __init__(self, name=""):
            self.children = {}
            self.terminal = False
            self.name = name

    n = int(data())
    root = Node()

    for _ in range(n):
        parts = data().strip().split("/")[1:]
        cur = root
        for p in parts:
            if p not in cur.children:
                cur.children[p] = Node(p)
            cur = cur.children[p]
        cur.terminal = True

    ans = 0

    def dfs(x):
        nonlocal ans
        if x.terminal and not x.children and x.name.endswith(".eoj"):
            ans += 1
        for y in x.children.values():
            dfs(y)

    dfs(root)
    sys.stdin = old
    return str(ans) + "\n"

assert run("""3
/secret/eoj
/secret
/secret.eoj
""") == "1\n", "sample 1"

assert run("""2
/cuber.eoj/qq.eoj
/cuber.eoj
""") == "1\n", "sample 2"

assert run("""1
/a.eoj
""") == "1\n", "single file"

assert run("""2
/a.eoj
/a.eoj/b
""") == "0\n", "directory with eoj suffix"

assert run("""4
/a.eoj/b.eoj
/a.eoj
/x
/y.eoj
""") == "2\n", "mixed files and directories"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`/a.eoj`|`1`| Một tệp khớp hậu tố duy nhất | 
|`/a.eoj`Và`/a.eoj/b`|`0`| Một thư mục có tên kết thúc bằng`.eoj`| 
| Cây hỗn hợp |`2`| Nhiều chi nhánh độc lập | 

## Vỏ cạnh 

Việc kiểm tra hậu tố không thành công khi thư mục có tên kết thúc bằng`.eoj`. 

đầu vào:```
2
/a.eoj
/a.eoj/b
```Trong quá trình xây dựng thử nghiệm,`/a.eoj`nhận được một nút con`b`. DFS thấy rằng`/a.eoj`không phải là lá nên không được tính. Kết quả là`0`. 

Một tệp lồng nhau có tổ tiên khớp với hậu tố không được ảnh hưởng đến tổ tiên. 

đầu vào:```
2
/a.eoj/b.eoj
/a.eoj
```Trie chứa một đứa trẻ dưới`a.eoj`, do đó nút đó được coi là một thư mục. Nút cho`b.eoj`không có con và kết thúc bằng`.eoj`, vì vậy nó đóng góp một số lượng. Đầu ra là`1`. 

Đầu vào hợp lệ ngắn nhất chỉ chứa một đường dẫn. 

đầu vào:```
1
/x
```Trie chứa một nút lá cuối cùng. Từ`x`không kết thúc với`.eoj`, DFS trả về`0`. Điều này xác nhận rằng thuật toán không tính mọi tệp, chỉ các tệp có phần mở rộng được yêu cầu. 

Bạn có thể điều chỉnh thêm điều này thành định dạng biên tập cuộc thi ngắn hơn hoặc phiên bản kiểu blog giải thích hơn nếu cần.
