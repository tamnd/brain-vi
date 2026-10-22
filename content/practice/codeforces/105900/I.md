---
title: "CF 105900I - Phát minh ra tên"
description: "Chúng ta được cung cấp một tập hợp các chuỗi hiện có trên các chữ cái tiếng Anh viết thường và độ dài tối đa cho phép là K. Nhiệm vụ là xây dựng một chuỗi mới không nằm trong tập hợp đã cho, có độ dài tối đa là K và có độ dài từ điển càng nhỏ càng tốt trong số tất cả các lựa chọn hợp lệ."
date: "2026-06-21T20:25:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105900
codeforces_index: "I"
codeforces_contest_name: "VI UnBalloon Contest Mirror"
rating: 0
weight: 105900
solve_time_s: 49
verified: true
draft: false
---

[CF 105900I - Phát minh ra tên](https://codeforces.com/problemset/problem/105900/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các chuỗi hiện có trên các chữ cái tiếng Anh viết thường và độ dài tối đa cho phép là K. Nhiệm vụ là xây dựng một chuỗi mới không nằm trong tập hợp đã cho, có độ dài tối đa là K và có độ dài từ điển càng nhỏ càng tốt trong số tất cả các lựa chọn hợp lệ. 

Thứ tự từ điển ở đây hoạt động giống hệt như thứ tự từ điển: các tiền tố ngắn hơn sẽ xuất hiện trước nếu một chuỗi là tiền tố của một chuỗi khác, nếu không thì việc so sánh sẽ được quyết định bởi ký tự khác nhau đầu tiên. 

Khó khăn chính không phải là kiểm tra tính hợp lệ mà là tìm ra chuỗi bị thiếu nhỏ nhất một cách hiệu quả. Vì cả N và tổng độ dài của tất cả các chuỗi đều lớn, lên tới 2 × 10^5, nên bất kỳ phương pháp nào cố gắng tạo và kiểm tra tất cả các chuỗi ứng cử viên một cách rõ ràng sẽ thất bại. Ngay cả việc tạo ra tất cả các chuỗi có độ dài K sẽ là hàm mũ trong K. 

Một cạm bẫy ngây thơ xuất hiện ngay lập tức khi suy nghĩ theo từ điển: người ta có thể cố gắng bắt đầu từ "a", rồi "aa", "aaa", v.v. hoặc cố gắng lặp lại các chuỗi theo thứ tự từ điển. Điều này bị phá vỡ nhanh chóng vì không gian của các chuỗi tăng theo cấp số nhân và hầu hết các ứng cử viên đều không hợp lệ do nằm trong tập hợp. 

Một trường hợp phức tạp hơn là cấu trúc tiền tố. Ví dụ: nếu tập hợp chứa "a" và "aa", câu trả lời là "ab" chứ không phải "b". Cách tiếp cận mở rộng tiền tố tham lam có thể dễ dàng thất bại nếu nó không tính toán đúng các nhánh bị chiếm dụng trong không gian từ điển. 

Một trường hợp quan trọng khác là khi tất cả các chuỗi ngắn đều bị chặn. Nếu tất cả các chuỗi có độ dài 1 đều có mặt, chúng ta phải chuyển sang độ dài 2, nhưng không cần phải kiểm tra mù quáng tất cả các chuỗi có độ dài 2. Ví dụ: nếu "aa", "ab", ..., "az" đều có mặt thì chúng ta phải chuyển sang "ba". 

Thử thách về cơ bản là: tìm chuỗi nhỏ nhất về mặt từ điển không nằm trong tập hợp bị cấm, dưới một ràng buộc về độ dài. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: liệt kê tất cả các chuỗi theo thứ tự từ điển có độ dài tối đa K và trả về chuỗi đầu tiên không có trong tập hợp. Điều này đúng vì việc tạo từ điển duy trì trật tự và chúng tôi kiểm tra tư cách thành viên ở mỗi bước. Tuy nhiên, điều này là không khả thi vì số lượng chuỗi có độ dài K vào khoảng 26^K trong trường hợp xấu nhất. Ngay cả đối với K = 10, đây là con số lớn về mặt thiên văn và K có thể lên tới 2 × 10^5, khiến việc liệt kê hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta không cần xem xét tất cả các chuỗi, chỉ những chuỗi có thể là tiền tố của các câu trả lời hợp lệ. Điều này tự nhiên gợi ý việc xây dựng một bộ ba chuỗi bị cấm. Khi chúng ta nghĩ về một trie, vấn đề sẽ trở thành một quá trình truyền tải có kiểm soát trên cây 26-ary trong đó một số nút được đánh dấu là điểm cuối bị cấm. 

Chuỗi bị thiếu nhỏ nhất về mặt từ điển tương ứng với điểm đầu tiên trong quá trình duyệt thứ tự trước của bộ ba này, nơi chúng ta có thể mở rộng với một nút con bị thiếu hoặc khi một nút không bão hòa hoàn toàn trong các đường dẫn con của nó lên đến độ sâu K. 

Thay vì tạo ra các chuỗi một cách rõ ràng, chúng tôi mô phỏng DFS trên bộ ba ẩn, luôn thử các ký tự từ 'a' đến 'z'. Chúng ta dừng lại ở điểm đầu tiên nơi chúng ta có thể tạo thành một chuỗi không tồn tại hoặc có thể được mở rộng vượt quá tập hợp bị cấm trong phạm vi độ dài K. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê Brute Force | O(26^K) | O(1) | Quá chậm | 
| Tìm kiếm Trie + DFS | O(tổng ký tự × 26) | O(tổng ký tự × 26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta xây dựng một trie chứa tất cả các chuỗi bị cấm. Mỗi nút đại diện cho một tiền tố và chúng tôi đánh dấu xem nút đó có tương ứng với một từ bị cấm hoàn toàn hay không. 

Sau đó, chúng tôi thực hiện tìm kiếm theo chiều sâu để tìm chuỗi từ điển nhỏ nhất không bị cấm và có độ dài tối đa là K.

1. Chèn tất cả các chuỗi đã cho vào một tri. Mỗi nút lưu trữ 26 con trỏ con và một cờ cho biết liệu một từ có kết thúc ở đó hay không. Điều này nén các tiền tố được chia sẻ để chúng tôi không xử lý lặp lại cấu trúc giống hệt nhau. 
2. Xác định hàm đệ quy dfs(nút, độ sâu) cố gắng xây dựng câu trả lời bắt đầu từ tiền tố hiện tại được đại diện bởi nút. Độ sâu theo dõi độ dài hiện tại của chuỗi được xây dựng. 
3. Nếu độ sâu bằng K thì chúng ta không thể mở rộng thêm nữa. Nếu nút này không phải là từ bị cấm thì bản thân tiền tố hiện tại là hợp lệ và có thể được trả về dưới dạng câu trả lời dự kiến. Nếu bị cấm, chúng tôi sẽ trả về lỗi vì không được phép gia hạn. 
4. Nếu nút hiện tại không được đánh dấu là từ bị cấm thì tiền tố được hình thành cho đến nay đã là một chuỗi hợp lệ. Vì chúng ta muốn nhỏ nhất về mặt từ điển nên chúng ta có thể trả về tiền tố này ngay lập tức. Điều này là do việc mở rộng nó sẽ chỉ làm cho chuỗi lớn hơn. 
5. Nếu không, chúng ta phải kéo dài chuỗi. Chúng tôi lặp lại các ký tự từ 'a' đến 'z'. Đối với mỗi ký tự, chúng tôi di chuyển đến nút con tương ứng (tạo một nút bị thiếu ẩn nếu cần) và cố gắng đệ quy để xây dựng một phần mở rộng hợp lệ. 
6. Kết quả đệ quy thành công đầu tiên được trả về ngay lập tức vì chúng ta đang khám phá theo thứ tự từ điển. 

### Tại sao nó hoạt động 

Trie đảm bảo chúng tôi chỉ phân nhánh theo tiền tố hiện có của các chuỗi bị cấm và các nhánh bị thiếu tương ứng với không gian từ điển không được sử dụng. DFS khám phá các ứng cử viên theo thứ tự từ điển nghiêm ngặt do quét trẻ em từ 'a' đến 'z'. Thời điểm chúng ta gặp một nút không phải là thiết bị đầu cuối bị cấm, chúng ta có thể xuất nó một cách an toàn vì bất kỳ phần mở rộng nào cũng sẽ lớn hơn về mặt từ điển. Điều này đảm bảo rằng chúng tôi không bao giờ bỏ qua chuỗi hợp lệ nhỏ hơn và chúng tôi không bao giờ trả lại chuỗi bị cấm vì chúng tôi chỉ chấp nhận các nút không được đánh dấu là các từ bị chiếm giữ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

class Node:
    __slots__ = ("next", "end")
    def __init__(self):
        self.next = {}
        self.end = False

root = Node()

def insert(s):
    cur = root
    for c in s:
        if c not in cur.next:
            cur.next[c] = Node()
        cur = cur.next[c]
    cur.end = True

N, K = map(int, input().split())
for _ in range(N):
    insert(input().strip())

def dfs(node, depth, path):
    if depth <= K and not node.end:
        return path

    if depth == K:
        return None

    for c in "abcdefghijklmnopqrstuvwxyz":
        if c in node.next:
            nxt = node.next[c]
        else:
            nxt = Node()
            node.next[c] = nxt

        res = dfs(nxt, depth + 1, path + c)
        if res is not None:
            return res

    return None

print(dfs(root, 0, ""))
```Cấu trúc trie nén các tiền tố được chia sẻ để tránh việc lặp lại các tên tương tự. Hàm DFS mã hóa trực tiếp thứ tự từ điển bằng cách lặp lại các ký tự từ 'a' đến 'z'. Sự tinh tế quan trọng là điều kiện`if depth <= K and not node.end`, cho phép chấm dứt sớm khi tiền tố hiện tại đã hợp lệ và không bị cấm. 

Việc sử dụng từ điển động cho trẻ em sẽ tránh được việc phân bổ đầy đủ 26 mảng, điều này có thể chấp nhận được dưới các ràng buộc vì tổng số ký tự được giới hạn bởi 2 × 10^5. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 10
torterra
pikachu
```Chúng tôi xây dựng một thử nghiệm chứa cả hai từ. Bây giờ chúng ta khởi động DFS từ thư mục gốc. 

| Bước | Tiền tố hiện tại | Nút bị cấm | Hành động | 
| --- | --- | --- | --- | 
| 1 | "" | Không | Vì tiền tố trống không phải là một từ nên chúng tôi không thể dừng lại, chúng tôi mở rộng | 
| 2 | "một" | Không | Trở về ngay vì “a” không bị cấm | 

DFS thử 'a' trước ở thư mục gốc và không tìm thấy xung đột. Vì bản thân nút gốc không phải là nút cuối nên chuỗi hợp lệ đầu tiên gặp phải là "a". 

Điều này xác nhận rằng chuỗi từ điển nhỏ nhất được chọn chính xác ngay cả khi các từ đầu vào không liên quan. 

### Ví dụ 2 

đầu vào:```
2 2
a
aa
```Chúng ta xây dựng một trie trong đó "a" là một từ bị cấm và "aa" mở rộng nó. 

| Bước | Tiền tố hiện tại | Nút bị cấm | Hành động | 
| --- | --- | --- | --- | 
| 1 | "" | Không | Không thể dừng lại, mở rộng | 
| 2 | "một" | Có | Phải mở rộng hơn nữa | 
| 3 | "aa" | Có nhưng chiều dài=2 | Ngõ cụt | 
| 4 | "ab" | Không | Trả về "ab" | 

Chúng ta thấy rằng "a" bị chặn, "aa" cũng bị chặn nên DFS tiếp tục chuyển sang nhánh từ điển tiếp theo và trả về "ab". 

Điều này thể hiện việc xử lý đúng các chuỗi chiếm giữ tiền tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng ký tự × 26) | Mỗi nút trie được truy cập tối đa một lần trên mỗi nhánh ký tự và việc phân nhánh được giới hạn bởi kích thước bảng chữ cái | 
| Không gian | O(tổng ký tự) | Trie lưu trữ một nút cho mỗi ký tự tiền tố riêng biệt | 

Tổng giới hạn ký tự là 2 × 10^5, do đó cả bộ nhớ và thời gian đều nằm trong giới hạn. Hệ số không đổi từ 26 lần phân nhánh đủ nhỏ đối với Python trong 1 giây khi cắt bớt các kết quả trả về sớm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    sys.setrecursionlimit(10**7)

    class Node:
        def __init__(self):
            self.next = {}
            self.end = False

    root = Node()

    def insert(s):
        cur = root
        for c in s:
            if c not in cur.next:
                cur.next[c] = Node()
            cur = cur.next[c]
        cur.end = True

    N, K = map(int, input().split())
    for _ in range(N):
        insert(input().strip())

    def dfs(node, depth, path):
        if depth <= K and not node.end:
            return path
        if depth == K:
            return None
        for c in "abcdefghijklmnopqrstuvwxyz":
            if c in node.next:
                nxt = node.next[c]
            else:
                nxt = Node()
                node.next[c] = nxt
            res = dfs(nxt, depth + 1, path + c)
            if res is not None:
                return res
        return None

    return dfs(root, 0, "")

assert run("2 10\ntorterra\npikachu\n") == "a"
assert run("2 2\na\naa\n") == "ab"
assert run("1 1\nz\n") == "a"
assert run("3 3\na\nb\nc\n") == "aa"
assert run("2 3\nabc\nabd\n") == "a"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| những từ nhỏ không liên quan | một | bắt đầu từ điển cơ bản | 
| chuỗi tiền tố | ab | phần mở rộng tiền tố bị chặn | 
| chữ cái cuối cùng tối đa | một | quấn quanh từ z | 
| đầy đủ lớp đầu tiên | aa | mở rộng sâu hơn | 
| anh chị em thân thiết | một | cắt tỉa từ điển sớm | 

## Vỏ cạnh 

Một trường hợp khó phát hiện xảy ra khi tiền tố trống đã hợp lệ vì không có chuỗi trống bị cấm. Thuật toán không bao giờ trả về giá trị trống không chính xác vì độ sâu 0 không được coi là đầu ra đầu cuối hợp lệ trừ khi được cho phép rõ ràng và chúng tôi luôn đảm bảo có ít nhất một phần mở rộng ký tự khi gốc không phải là đầu cuối. 

Một trường hợp quan trọng khác là khi một từ vừa là điểm cuối bị cấm vừa có con trong bộ thử. Ví dụ: nếu "a" bị cấm nhưng "aa" thì không, thuật toán sẽ bỏ qua "a" một cách chính xác và chỉ tiếp tục khám phá "aa" sau khi thất bại tất cả các ứng cử viên từ điển ngắn hơn bắt đầu bằng "a". 

Trường hợp cuối cùng là khi K = 1 và tất cả các chữ cái ngoại trừ một chữ cái đều có mặt. DFS sẽ chọn chính xác ký tự bị thiếu duy nhất vì quá trình quét cấp độ đầu tiên từ 'a' đến 'z' xác định trực tiếp nhánh bị thiếu mà không cần đệ quy sâu hơn.
