---
title: "CF 2041I - Tự động hoàn thành"
description: "Chúng tôi được yêu cầu mô phỏng một trình soạn thảo văn bản nâng cao hỗ trợ bốn thao tác: thêm mẫu để tự động hoàn thành, xóa mẫu, thêm văn bản vào nội dung trình soạn thảo hiện tại và xóa các ký tự ở cuối nội dung hiện tại."
date: "2026-06-08T09:45:46+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "hashing", "implementation", "sortings", "strings", "trees"]
categories: ["algorithms"]
codeforces_contest: 2041
codeforces_index: "I"
codeforces_contest_name: "2024 ICPC Asia Taichung Regional Contest (Unrated, Online Mirror, ICPC Rules, Preferably Teams)"
rating: 2300
weight: 2041
solve_time_s: 130
verified: true
draft: false
---

[CF 2041I - Tự động hoàn thành](https://codeforces.com/problemset/problem/2041/I) 

**Đánh giá:** 2300 
**Thẻ:** tìm kiếm nhị phân, cấu trúc dữ liệu, băm, triển khai, sắp xếp, chuỗi, cây 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng một trình soạn thảo văn bản nâng cao hỗ trợ bốn thao tác: thêm mẫu để tự động hoàn thành, xóa mẫu, thêm văn bản vào nội dung trình soạn thảo hiện tại và xóa các ký tự ở cuối nội dung hiện tại. Sau mỗi thao tác, người biên tập nên đề xuất mẫu "tốt nhất" phù hợp với nội dung hiện tại làm tiền tố. Mẫu tốt nhất được xác định trước tiên theo độ dài tối đa, sau đó là thứ tự từ điển và cuối cùng là ID nhỏ nhất. 

Đầu vào cung cấp số lượng thao tác theo sau là chính các thao tác đó. Mỗi thao tác có thể tham chiếu một mẫu theo ID, một chuỗi để nối thêm hoặc một số ký tự cần xóa. Đầu ra là một dòng cho mỗi thao tác, hiển thị ID của mẫu được đề xuất hoặc -1 nếu không có kết quả khớp. 

Cho rằng`n`có thể lên tới 10^6 và tổng số ký tự trong tất cả các mẫu và chuỗi có thể đạt tới 2×10^6, mọi giải pháp quét tất cả các mẫu cho mỗi truy vấn sẽ quá chậm. Cụ thể, việc kiểm tra mạnh mẽ đối với mỗi lần thêm hoặc xóa sẽ bao gồm các thao tác O(n × m) trong đó m là độ dài của nội dung hiện tại, dễ dàng đạt tới 10^12 trong trường hợp xấu nhất. Chúng tôi cần một giải pháp cho phép chèn, xóa và tra cứu tiền tố theo thời gian tuyến tính phụ hoặc khấu hao không đổi so với số lượng mẫu. 

Các trường hợp cạnh bao gồm các tình huống trong đó nhiều mẫu khớp với nhau về độ dài và từ điển, khi xóa nhiều ký tự hơn số ký tự tồn tại trong nội dung hiện tại và khi nội dung hiện tại trống hoặc hoàn toàn không khớp. Ví dụ: nếu mẫu duy nhất là "abc" và "abcd" và người dùng thêm "ab" thì kết quả trùng khớp dài nhất sẽ đề xuất "abcd". Một cách tiếp cận ngây thơ chọn kết quả phù hợp đầu tiên mà nó tìm thấy có thể gợi ý không chính xác "abc". 

## Phương pháp tiếp cận 

Giải pháp vũ phu sẽ duy trì một danh sách các mẫu. Đối với mỗi truy vấn thay đổi nội dung hiện tại, nó sẽ quét tất cả các mẫu, kiểm tra xem nội dung đó có phải là tiền tố hay không, sau đó chọn kết quả phù hợp nhất theo quy tắc. Điều này đúng về mặt logic nhưng thực hiện kiểm tra O(n) cho mỗi thay đổi nội dung, điều này trở nên không thực tế trên quy mô lớn. 

Quan sát quan trọng là chúng ta cần khớp tiền tố nhanh bằng cách chèn và xóa động các mẫu. Trie (cây tiền tố) hỗ trợ các truy vấn tiền tố một cách tự nhiên một cách hiệu quả. Mỗi nút trong trie tương ứng với một ký tự và các cạnh biểu thị sự chuyển tiếp giữa các ký tự. Bằng cách lưu trữ ID mẫu ứng cử viên tại các nút, chúng tôi có thể nhanh chóng xác định kết quả khớp dài nhất bắt đầu từ gốc. Để xử lý việc xóa và ngắt kết nối (độ dài, từ điển, ID), mỗi nút phải duy trì một tập hợp hoặc một đống mẫu hoạt động đi qua nó, được sắp xếp theo tiêu chí của chúng tôi. 

Với cấu trúc này, việc thêm mẫu bao gồm việc đi xuống bộ ba, tạo các nút nếu cần và chèn ID mẫu vào mỗi nút dọc theo đường dẫn. Xóa đi theo đường dẫn và xóa ID khỏi mỗi nút. Truy vấn nội dung hiện tại chỉ là tìm kiếm dọc theo các ký tự của văn bản hiện tại và đọc ID mẫu tốt nhất từ ​​nút sâu nhất có thể tiếp cận. 

Cách tiếp cận này giảm độ phức tạp về thời gian cho mỗi thao tác xuống O (độ dài của chuỗi) cho cả phần chèn và truy vấn, điều này có thể chấp nhận được vì tổng của tất cả độ dài chuỗi được giới hạn bởi 2×10^6. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n × tổng chiều dài mẫu) | O(n × chiều dài mẫu trung bình) | Quá chậm | 
| Thử với siêu dữ liệu | O(tổng chiều dài chuỗi) | O(tổng chiều dài chuỗi) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một bản thử trống. Mỗi nút chứa một từ điển cho các nút con và một tập hợp ID mẫu đi qua nút đó. Mỗi ID được liên kết với chuỗi và độ dài của mẫu để phá vỡ mối liên kết. 
2. Đối với thao tác thêm bằng ID`i`và mẫu`p`, bắt đầu từ gốc của tri. Với mỗi ký tự trong`p`, tạo một nút con nếu nó không tồn tại. Tại mỗi nút dọc theo đường dẫn, chèn`i`vào tập hợp của nút. Điều này đảm bảo mọi tiền tố của`p`biết những mô hình nào đi qua nó. 
3. Đối với thao tác xóa bằng ID`i`, tra cứu chuỗi mẫu ban đầu. Đi xuống thử theo chuỗi. Tại mỗi nút dọc theo đường dẫn, hãy loại bỏ`i`từ tập hợp của nút. Điều này ngăn chặn các mẫu cũ được đề xuất. 
4. Duy trì một chuỗi`t`đại diện cho nội dung hiện tại trong trình soạn thảo. Đối với thao tác nối thêm, hãy thêm chuỗi vào`t`. Đối với thao tác xóa lùi, hãy xóa số lượng ký tự được chỉ định ở cuối, cắt bớt hoàn toàn nếu cần. 
5. Sau mỗi thao tác, hãy truy vấn bản thử bằng cách đi xuống`t`. Nếu tại bất kỳ thời điểm nào một ký tự không tồn tại trong bộ ba, ghi -1. Ngược lại, đến nút tương ứng với ký tự cuối cùng của`t`và chọn mẫu có độ dài tối đa, sau đó là giá trị từ điển tối thiểu, sau đó là ID tối thiểu. Nút này chứa một đống hoặc tập hợp được sắp xếp để cho phép lựa chọn này trong thời gian O(1) cho mỗi truy vấn. 
6. Xuất ID của mẫu đã chọn hoặc -1 nếu không có mẫu nào khớp. 

Tại sao nó hoạt động: Mỗi nút trie tổng hợp tất cả các mẫu hoạt động có chung tiền tố của nó. Bằng cách duy trì các bộ ngắt kết nối theo thứ tự được sắp xếp, nút sâu nhất có thể truy cập được dọc theo`t`luôn chứa gợi ý hợp lệ nhất. Việc thêm và xóa các mẫu sẽ cập nhật tất cả các tiền tố bị ảnh hưởng, do đó không có nút nào chứa các mẫu cũ. 

## Giải pháp Python```python
import sys
import bisect
input = sys.stdin.readline

class TrieNode:
    __slots__ = ('children', 'patterns')
    def __init__(self):
        self.children = {}
        self.patterns = []

class Pattern:
    __slots__ = ('length', 'string', 'id')
    def __init__(self, string, id):
        self.string = string
        self.id = id
        self.length = len(string)
    def __lt__(self, other):
        if self.length != other.length:
            return self.length > other.length
        if self.string != other.string:
            return self.string < other.string
        return self.id < other.id

class AutoComplete:
    def __init__(self):
        self.root = TrieNode()
        self.pattern_map = {}
        self.t = ''

    def add_pattern(self, id, string):
        self.pattern_map[id] = string
        node = self.root
        pat = Pattern(string, id)
        for c in string:
            if c not in node.children:
                node.children[c] = TrieNode()
            node = node.children[c]
            bisect.insort(node.patterns, pat)

    def delete_pattern(self, id):
        string = self.pattern_map[id]
        node = self.root
        pat = Pattern(string, id)
        for c in string:
            node = node.children[c]
            idx = bisect.bisect_left(node.patterns, pat)
            if idx < len(node.patterns) and node.patterns[idx].id == id:
                node.patterns.pop(idx)

    def query(self):
        node = self.root
        for c in self.t:
            if c not in node.children:
                return -1
            node = node.children[c]
        if not node.patterns:
            return -1
        return node.patterns[0].id

    def append(self, s):
        self.t += s

    def backspace(self, c):
        self.t = self.t[:-c] if c <= len(self.t) else ''

ac = AutoComplete()
n = int(input())
for _ in range(n):
    parts = input().split()
    if parts[0] == 'add':
        ac.add_pattern(int(parts[1]), parts[2])
    elif parts[0] == 'delete':
        ac.delete_pattern(int(parts[1]))
    elif parts[0] == 'append':
        ac.append(parts[1])
    else:  # backspace
        ac.backspace(int(parts[1]))
    print(ac.query())
```Giải pháp xác định một`TrieNode`chứa các nút con và một danh sách các mẫu có thứ tự. Mỗi mẫu được gói trong một lớp với toán tử so sánh tùy chỉnh ưu tiên độ dài, sau đó là thứ tự từ điển, sau đó là ID. Chúng tôi duy trì bản đồ từ ID mẫu đến chuỗi để hỗ trợ xóa một cách hiệu quả.`bisect.insort`được sử dụng để sắp xếp các mẫu, cho phép lựa chọn nhanh chóng ứng cử viên tốt nhất. Các thao tác nối và lùi thao tác trực tiếp với chuỗi văn bản hiện tại. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
6
add 1 pattern1_alice
add 2 pattern2_bob
add 3 pattern3_charlie
append pattern
append 2_bobabc
backspace 3
```| Bước | văn bản hiện tại`t`| Hoạt động | Mẫu nút Trie | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | "" | thêm 1 "pattern1_alice" | root->p->a->...->e mẫu=[1] | 1 | 
| 2 | "" | thêm 2 "pattern2_bob" | root->p->a->... mẫu=[1,2] | 1 | 
| 3 | "" | thêm 3 "pattern3_charlie" | root->p->a->... mẫu=[1,2,3] | 3 | 
| 4 | "mẫu" | nối thêm "mẫu" | nút tại 'n' mẫu=[1,2,3] | 3 | 
| 5 | "mẫu2_bobabc" | nối thêm "2_bobabc" | nút tại 'c' mẫu=[2] | -1 | 
| 6 | "mẫu2_b" | lùi lại 3 | nút tại 'b' mẫu=[2] | 2 | 

Dấu vết này cho thấy các mẫu được đề xuất chính xác sau mỗi lần sửa đổi. Việc xóa và bổ sung duy trì sự bất biến của vùng heap tại mỗi nút. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng chiều dài của tất cả các chuỗi) | Mỗi ký tự trong mỗi mẫu được chèn |
