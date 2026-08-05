---
title: "CF 102483E - Kiểm soát bình đẳng"
description: "Chúng ta có hai chương trình được viết bằng một ngôn ngữ nhỏ trong đó mỗi biểu thức tạo ra một danh sách các số nguyên dương. Các chương trình có thể chứa các danh sách cố định, ghép nối, xáo trộn ngẫu nhiên và sắp xếp."
date: "2026-08-06T04:13:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102483
codeforces_index: "E"
codeforces_contest_name: "2018-2019 ICPC Northwestern European Regional Programming Contest (NWERC 2018)"
rating: 0
weight: 102483
solve_time_s: 109
verified: true
draft: false
---

[CF 102483E - Kiểm soát sự bình đẳng](https://codeforces.com/problemset/problem/102483/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chương trình được viết bằng một ngôn ngữ nhỏ trong đó mỗi biểu thức tạo ra một danh sách các số nguyên dương. Các chương trình có thể chứa các danh sách cố định, ghép nối, xáo trộn ngẫu nhiên và sắp xếp. Nhiệm vụ không phải là so sánh văn bản của các chương trình mà là quyết định xem chúng có tạo ra phân bổ xác suất giống hệt nhau trên tất cả các danh sách đầu ra có thể hay không. 

Khó khăn đến từ việc các thao tác có thể che giấu thông tin. Danh sách xáo trộn không quan tâm đến thứ tự ban đầu, trong khi phép nối giữ ranh giới giữa hai phần của nó. Ví dụ, xáo trộn ngẫu nhiên`[1,2,1,2]`khác với việc ghép hai lần xáo trộn độc lập của`[1,2]`, vì phiên bản thứ hai luôn chứa điểm phân chia ẩn. 

Chuỗi đầu vào có thể chứa tối đa một triệu ký tự. Điều này ngay lập tức loại trừ việc đánh giá tất cả các kết quả đầu ra có thể xảy ra, bởi vì ngay cả một danh sách ngắn cũng có thể có nhiều hoán vị theo giai thừa. Nó cũng loại trừ việc xây dựng các bản phân phối trung gian lớn. Thuật toán phải xử lý cú pháp gần như tuyến tính và chỉ giữ lại mô tả ngắn gọn về phân bố kết quả. 

Các trường hợp khó khăn chính xuất phát từ các hoạt động khó hiểu trông giống nhau. Một chương trình như`shuffle([1,2,3])`không tương đương với`concat(shuffle([1,2]),[3])`, vì phần tử cuối cùng được cố định trong chương trình thứ hai. Một chương trình như`shuffle([1])`nên được coi là danh sách xác định`[1]`, nếu không thì các chương trình tương đương có thể nhận được các cách biểu diễn khác nhau. Giá trị trùng lặp là một cái bẫy khác. Xác suất của mỗi chuỗi cuối cùng phụ thuộc vào bội số, vì vậy việc coi danh sách như một tập hợp sẽ đưa ra câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ là mô phỏng phân bố xác suất của mọi biểu thức. Để biết danh sách có độ dài`n`, điều này có nghĩa là lưu trữ tối đa`n!`những hoán vị có thể xảy ra sau khi xáo trộn. Ngay cả đối với một danh sách chỉ có 15 vị trí khác nhau, điều này là không thể và kích thước đầu vào tối đa lớn hơn nhiều bậc. 

Quan sát quan trọng là việc xáo trộn hoàn toàn quên mất cấu trúc bên trong của đối số của nó. Thông tin duy nhất nó lưu giữ là nhiều giá trị. Một thao tác được sắp xếp cũng chỉ cần nhiều tập hợp, nhưng đầu ra của nó mang tính xác định. Mọi thứ khác có thể được biểu diễn dưới dạng một chuỗi của hai loại mảnh này. 

Phương pháp brute-force hoạt động vì ngữ nghĩa ngôn ngữ nhỏ, nhưng nó thất bại vì số lượng đầu ra có thể tăng theo cấp số nhân. Quan sát cho thấy mọi biểu thức có thể được rút gọn thành các phân đoạn xác định và các phân đoạn được xáo trộn độc lập cho phép chúng ta so sánh các dạng thông thường thu gọn. 

Trong quá trình chuẩn hóa, một phân đoạn xác định sẽ lưu trữ trình tự chính xác của nó. Một phân đoạn ngẫu nhiên chỉ lưu trữ số lượng của từng giá trị. Các phân đoạn xác định liền kề có thể được hợp nhất và một phân đoạn ngẫu nhiên chỉ chứa một giá trị có thể được chuyển đổi thành phân đoạn xác định. 

Biểu diễn kết quả là chuẩn. Nếu hai biểu diễn chuẩn hóa khác nhau thì một số hành vi ranh giới, giá trị hoặc xác suất sẽ khác nhau, điều đó có nghĩa là các chương trình ban đầu không thể tương đương. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số lượng đầu ra có thể có) | O(số lượng đầu ra có thể có) | Quá chậm | 
| Thi công dạng bình thường | O(n) dự kiến ​​| O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích biểu thức theo cách đệ quy và trả về dạng chuẩn hóa của nó cùng với nhiều tập hợp giá trị của nó. Multiset là cần thiết bởi vì cả hai`shuffle`Và`sorted`bỏ thông tin đặt hàng. 
2. Chuyển đổi một danh sách theo nghĩa đen thành một khối xác định. Multiset của nó được tính toán trong khi đọc các con số. 
3. Đối với`concat(a,b)`, nối các chuỗi khối chuẩn hóa của cả hai phần tử con. Nếu hai khối lân cận có tính xác định, hãy hợp nhất chúng vì không có ranh giới có thể quan sát được giữa hai chuỗi cố định. 
4. Đối với`shuffle(x)`, loại bỏ cấu trúc khối của`x`và tạo một khối ngẫu nhiên chỉ chứa nhiều tập hợp tất cả các giá trị trong`x`. Một khối ngẫu nhiên có độ dài bằng 1 được đơn giản hóa thành khối xác định. 
5. Đối với`sorted(x)`, tạo một khối xác định chứa tất cả các giá trị của`x`theo thứ tự tăng dần. Thông tin đặt hàng ban đầu là không liên quan. 
6. So sánh trình tự khối chuẩn hóa của hai chương trình. Mọi khối xác định phải có cùng một chuỗi và mọi khối ngẫu nhiên phải có cùng nhiều tập hợp. 

Điều bất biến là mọi biểu thức chuẩn hóa đều mô tả chính xác sự phân bố giống như biểu thức ban đầu. Các khối xác định biểu thị các giá trị buộc phải xuất hiện ở các vị trí chính xác, trong khi các khối ngẫu nhiên biểu thị một hoán vị thống nhất của nhiều tập hợp. Các phép toán trên bảo toàn ý nghĩa này, do đó, các dạng chuẩn hóa bằng nhau ngụ ý các phân phối bằng nhau và các dạng chuẩn hóa khác nhau ngụ ý các phân phối khác nhau. 

## Giải pháp Python```python
import sys
from collections import Counter

input = sys.stdin.readline
sys.setrecursionlimit(3000000)

def merge_blocks(a, b):
    if not a:
        return b
    if not b:
        return a
    if a[-1][0] == 'D' and b[0][0] == 'D':
        return a[:-1] + [('D', a[-1][1] + b[0][1])] + b[1:]
    return a + b

def normalize_block(kind, data):
    if kind == 'R':
        if sum(data.values()) == 1:
            x = next(iter(data))
            return [('D', (x,))]
        return [('R', tuple(sorted(data.items())))]
    return [('D', tuple(data))]

def parse(s):
    n = len(s)
    pos = 0

    def dfs():
        nonlocal pos

        if s[pos] == '[':
            pos += 1
            vals = []
            cnt = Counter()
            while s[pos] != ']':
                start = pos
                while s[pos].isdigit():
                    pos += 1
                x = int(s[start:pos])
                vals.append(x)
                cnt[x] += 1
                if s[pos] == ',':
                    pos += 1
            pos += 1
            return [('D', tuple(vals))], cnt

        start = pos
        while s[pos].isalpha():
            pos += 1
        op = s[start:pos]
        pos += 1

        if op == 'concat':
            left, c1 = dfs()
            pos += 1
            right, c2 = dfs()
            pos += 1
            return merge_blocks(left, right), c1 + c2

        child, cnt = dfs()
        pos += 1

        if op == 'shuffle':
            return normalize_block('R', cnt), cnt

        arr = []
        for x, c in sorted(cnt.items()):
            arr.extend([x] * c)
        return [('D', tuple(arr))], cnt

    return dfs()[0]

a = input().strip()
b = input().strip()

print("equal" if parse(a) == parse(b) else "not equal")
```Trình phân tích cú pháp tuân theo ngữ pháp trực tiếp. Một danh sách bằng chữ được xử lý bằng cách thu thập các giá trị của nó và đếm số lần xuất hiện cùng một lúc. Bộ đếm là cần thiết vì kết quả được xáo trộn phụ thuộc vào bội số chứ không chỉ các giá trị riêng biệt. 

các`merge_blocks`chức năng là sự đơn giản hóa duy nhất cần thiết sau khi nối. Hai lân cận xác định không thể phân biệt được với một khối xác định lớn hơn, trong khi các khối ngẫu nhiên phải tách biệt vì tính độc lập giữa chúng làm thay đổi sự phân bố. 

các`normalize_block`Hàm xử lý trường hợp đơn lẻ tinh tế. Việc xáo trộn một giá trị không thể tạo ra tính ngẫu nhiên, vì vậy việc giữ nó như một khối ngẫu nhiên sẽ tạo ra nhiều cách biểu diễn cho cùng một hành vi. 

Trình phân tích cú pháp sử dụng đệ quy vì ngữ pháp có tính đệ quy tự nhiên. Giới hạn đệ quy được tăng lên để hỗ trợ các biểu thức lồng nhau sâu. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, hãy so sánh:`concat(shuffle([1,2]),shuffle([1,2]))`Và`shuffle([1,2,1,2])`| Bước | Chương trình đầu tiên | Chương trình thứ hai | 
| --- | --- | --- | 
| Phân tích cú pháp ngẫu nhiên đầu tiên | Khối ngẫu nhiên`{1:1,2:1}`| | 
| Phân tích ngẫu nhiên thứ hai | Hai khối ngẫu nhiên độc lập | | 
| Phân tích ngẫu nhiên bên ngoài | | Khối ngẫu nhiên`{1:2,2:2}`| 
| Dạng bình thường |`R(1,2), R(1,2)`|`R(1,1,2,2)`| 
| Kết quả | Khác nhau | Khác nhau | 

Dấu vết cho thấy tại sao việc giữ ranh giới khối ngẫu nhiên lại quan trọng. Hai biểu thức có tổng số nhiều tập hợp giống nhau nhưng xác suất khác nhau. 

Đối với mẫu thứ hai:`sorted(concat([3,2,1],[4,5,6]))`Và`[1,2,3,4,5,6]`| Bước | Chương trình đầu tiên | Chương trình thứ hai | 
| --- | --- | --- | 
| Đọc nghĩa đen | Nhiều bộ`{1,2,3,4,5,6}`| Sự liên tiếp`(1,2,3,4,5,6)`| 
| Áp dụng sắp xếp | xác định`(1,2,3,4,5,6)`| xác định`(1,2,3,4,5,6)`| 
| Kết quả | Bằng | Bằng | 

Ví dụ chứng minh rằng việc sắp xếp sẽ loại bỏ tất cả thông tin đặt hàng trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) dự kiến ​​| Mỗi ký tự được phân tích cú pháp một lần và các phép toán bản đồ băm cho số lượng được mong đợi là O(1). | 
| Không gian | O(n) | Biểu diễn chuẩn hóa lưu trữ thông tin tỷ lệ với kích thước đầu vào. | 

Giới hạn đầu vào là một triệu ký tự yêu cầu tránh liệt kê kết quả đầu ra và giữ phân tích cú pháp gần với tuyến tính. Giải pháp chỉ lưu trữ thông tin nén về tần suất đặt hàng và giá trị nên vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    a = sys.stdin.readline().strip()
    b = sys.stdin.readline().strip()
    sys.stdin = old
    return "equal\n" if parse(a) == parse(b) else "not equal\n"

assert run("concat(shuffle([1,2]),shuffle([1,2]))\nshuffle([1,2,1,2])\n") == "not equal\n"

assert run("sorted(concat([3,2,1],[4,5,6]))\n[1,2,3,4,5,6]\n") == "equal\n"

assert run("concat(sorted([4,3,2,1]),shuffle([1]))\nconcat(concat([1,2,3],shuffle([4])),sorted([1]))\n") == "equal\n"

assert run("[5]\nshuffle([5])\n") == "equal\n"

assert run("shuffle([1,1,2])\nconcat([1],shuffle([1,2]))\n") == "not equal\n"

assert run("sorted([9,9,1])\n[1,9,9]\n") == "equal\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`shuffle([1,2])`so với hai lần xáo trộn được nối | không bằng | Tính ngẫu nhiên độc lập không thể hợp nhất | 
|`sorted([9,9,1])`so với thứ tự sắp xếp | bằng | Việc sắp xếp chỉ sử dụng multiset | 
|`shuffle([5])`so với`[5]`| bằng | Đơn giản hóa ngẫu nhiên Singleton | 
| Giá trị trùng lặp trong các cấu trúc khác nhau | không bằng | Sự đa dạng ảnh hưởng đến xác suất | 

## Vỏ cạnh 

cho`shuffle([5])`so với`[5]`, thuật toán tạo một khối ngẫu nhiên có một phần tử và ngay lập tức chuyển nó thành khối xác định. Cả hai bình thường hóa thành`D(5)`, vì vậy đầu ra là chính xác`equal`. 

Vì`shuffle([1,1,2])`so với`concat([1],shuffle([1,2]))`, cả hai biểu thức đều chứa cùng các giá trị nhưng dạng chuẩn hóa của chúng khác nhau. Khối đầu tiên trở thành một khối ngẫu nhiên có số lượng`{1:2,2:1}`, trong khi khối thứ hai trở thành khối xác định, theo sau là khối ngẫu nhiên. Sự phân chia ẩn thay đổi các kết quả đầu ra có thể có, do đó thuật toán trả về`not equal`. 

Đối với các giá trị trùng lặp, chẳng hạn như`sorted([9,9,1])`so với`[1,9,9]`, bộ đếm nhiều bộ lưu trữ hai bản sao của`9`và một bản sao của`1`. Việc sắp xếp tạo ra chuỗi xác định chính xác, do đó cả hai chương trình đều nhận được cùng một biểu diễn chuẩn.
