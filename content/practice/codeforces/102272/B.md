---
title: "CF 102272B - \u0110\u1ebfm Th\u1ecf"
description: "Ta có một hàng gồm (N) con thỏ, con thứ (i) có thể được biểu hiện theo số (typi). Với mỗi đoạn liên tiếp ([l,r]), điểm của đoạn là số lượng khác nhau xuất hiện trong các con thỏ từ vị trí (l) đến (r). Bài toán không yêu cầu tìm một đoạn tốt nhất."
date: "2026-08-17T11:13:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102272
codeforces_index: "B"
codeforces_contest_name: "HCW 19 Individual Day 1"
rating: 0
weight: 102272
solve_time_s: 143
verified: false
draft: false
---

[CF 102272B - \u0110\u1ebfm Th\u1ecf](https://codeforces.com/problemset/problem/102272/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Ta có một hàng gồm (N) con thỏ, con thứ (i) có thể được biểu hiện theo số (typ_i). Với mỗi đoạn liên tiếp ([l,r]), điểm của đoạn là số lượng khác nhau xuất hiện trong các con thỏ từ vị trí (l) đến (r). 

Bài toán không yêu cầu tìm một đoạn tốt nhất. Ta phải cộng điểm của tất cả các đoạn liên tiếp có thể chọn: 

[ 
\sum_{1\le l\le r\le N} f(l,r). 
] 

Vì vậy, với mỗi đoạn, ta chỉ cần biết bao nhiêu điểm tương tự khác nhau xuất hiện trong đoạn đó, sau đó cộng tất cả các giá trị lại. 

Giới hạn (N) lên tới (10^6), và tổng (N) của toàn bộ bài kiểm tra không vượt quá (2\cdot10^6). Số đoạn liên tiếp đã có 

[ 
\frac{N(N+1)}2, 
] 

tức khoảng (5\cdot10^{11}) khi (N=10^6). Chỉ dành riêng công việc duyệt qua từng đoạn không thể thực hiện trong thời gian giới hạn. Một thuật toán (O(N^2)) cũng quá chậm, còn (O(N^3)) hoàn toàn không khả thi. Ta cần thời gian xuống (O(N)) cho mỗi bài kiểm tra, hoặc ít nhất là tính năng tuyến tính gần nhất. 

Giá trị (typ_i) có thể tăng lên (10^9), nên không thể sử dụng một mảng có kích thước bằng giá trị tương tự. Ta cần một sơ đồ cấu trúc giống thỏ sang liên quan thông tin, có giới hạn như từ điển. 

Kết quả cũng có thể rất lớn. Với (N=10^6) và tất cả các con thỏ có điểm khác nhau, mọi đoạn có độ dài của nó. Khi đó tổng số là 

[ 
1+2+\cdots+N 
] 

theo từng điểm bắt đầu, tương thích 

[ 
\frac{N(N+1)(N+2)}6 
=166667166667000000. 
] 

Giá trị này vượt quá giới hạn của số nguyên 32 bit. Python sử dụng tùy chọn xác thực số nguyên nên không cần xử lý công việc số. 

Một trường biên dịch khác là (N=1). With input gồm có một con thỏ như`5`, có chỉ đoạn ([1,1]), nên đáp án là (1). Cách phát triển khai báo sử dụng sai số hoặc khởi tạo tổng thể từ vị trí (0) có thể tạo ra kết quả sai. 

Trường hợp tất cả các con thỏ giống nhau cũng dễ gây khó chịu. Với`7 7 7`, có sáu đoạn nhưng mỗi đoạn chỉ chứa một đoạn giống nhau, nên đáp án là (6), không có số lượng phần tử đã được duyệt. Khi một lần xuất hiện lại tương tự, lần xuất hiện mới phải thay thế đóng góp của lần xuất hiện cũ. 

Một lỗi khác thường xuất hiện ở lần xuất hiện đầu tiên của cùng một lỗi. Với`1 2 1`, khi xử lý con thỏ thứ ba, giống`1`đã xuất hiện ở vị trí (1). Ta phải thay đổi vị trí cuối cùng của`1`từ (1) thành (3), chứ không cộng thêm một điểm giống nhau. Đáp án đúng là (9). 

## Phương pháp tiếp cận 

Cách trực tiếp nhất là xét nghiệm từng đoạn ([l,r]), duyệt các phần tử trong đoạn và đưa ra các phần tử tương tự vào một bộ. Kích thước của tập hợp sau khi duyệt xong chính là các số khác nhau của các đoạn, nên cách này đúng về mặt logic. 

Tuy nhiên, tổng số phần tử phải kiểm tra tất cả các đoạn là 

\frac{N(N+1)(N+2)}6. 
] 

Với (N=10^6), con số này khoảng (1.67\cdot10^{17}). Ngay cả khi mỗi thao tác được đặt chỉ bị mất (O(1)), khối lượng công việc này vẫn quá lớn. Một biến thể tốt hơn là cố định (l), rồi tăng (r) và tập duy trì, nhưng vẫn có (O(N^2)) đoạn phải xét, khoảng (5\cdot10^{11}) đoạn trong trường hợp lớn nhất. 

Ta cần thay đổi cách đếm. Vì xét nghiệm từng đoạn và hỏi đoạn đó có bao nhiêu điểm giống nhau, hãy cố gắng xác định điểm phải (r) và tính tổng điểm của tất cả các đoạn kết thúc tại (r). 

Một cụ thể tương tự. Gọi (p) là vị trí xuất hiện cuối cùng của cùng một đoạn tiền tố (1,\ldots,r). Một đoạn ([l,r]) chứa đoạn này giống khi và chỉ khi (l\le p). Có đúng (p) lựa chọn cho (l), từ (1) đến (p). Vì vậy, điều này cũng tương tự như đóng góp chính xác (p) vào tổng điểm của tất cả các đoạn kết thúc tại (r). 

Đây là thời điểm cuối cùng của bài toán. Với mỗi (r), nếu biết vị trí xuất hiện cuối cùng của tất cả các vị trí tương tự, thì tổng điểm của tất cả các đoạn cuối tại (r) đơn giản là tổng các vị trí cuối cùng. 

Khi chuyển từ (r-1) sang (r), chỉ có con thỏ ở vị trí (r) thay đổi vị trí xuất hiện cuối cùng. Nếu điều này chưa từng được xuất hiện, hãy thêm (r) vào tổng hợp. Nếu nó từng xuất hiện cuối cùng ở vị trí (p), ta thay (p) bằng (r), tổng mức tăng thêm (r-p). Như vậy, mỗi phần tử được xử lý chỉ một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^3)) if browser lại từng đoạn | (O(N)) | Quá chậm | 
| Duy trì bộ cho từng điểm bắt đầu | (O(N^2)) | (O(N)) | Quá chậm | 
| Tối ưu | (O(N)) trung bình | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duyệt các con thỏ từ trái sang phải. Sử dụng từ điển`last`để lưu trữ vị trí cuối cùng của mỗi vị trí. 
2. Duy trì biến`current`, là vị trí xuất hiện cuối cùng của tất cả các vị trí tương tự đã xuất hiện trong đoạn tiền hiện tại. 

Nếu đang ở vị trí (r),`current`chính là tổng điểm của mọi đoạn có dạng ([l,r]). Lý do tương tự có vị trí cuối cùng (p), và nó xuất hiện trong đoạn kết thúc đúng (p) tại (r), tương ứng với (l=1,\ldots,p). 
3. Khi gặp nhau`x`ở vị trí (r), check`last[x]`. 

Nếu`x`chưa xuất hiện, tương tự này chưa có đóng góp trong`current`,. ta cộng (r). 

Nếu`x`đã xuất hiện cuối cùng ở vị trí (p), đóng góp cũ của nó là (p), còn đóng góp mới là (r). Vì vậy ta chỉ cần cộng (r-p) vào`current`. 
4. Cập nhật`last[x] = r`. Sau thao tác này, từ điển phản ánh chính xác vị trí cuối cùng của từng giống trong tiền tố (1,\ldots,r). 
5. Cộng`current`vào câu trả lời.`current`là tổng điểm của tất cả các đoạn kết thúc tại (r), nên cộng nó qua mọi (r) sẽ thu được tổng điểm của toàn bộ các đoạn. 
6. Lặp lại cho đến vị trí (N), rồi đáp án. 

### Tại sao nó hoạt động 

Sau khi xử lý vị trí (r), với mỗi hiện thị tương tự trong tiền tố (1,\ldots,r), từ điển lưu vị trí xuất hiện cuối cùng (p). Một đoạn ([l,r]) chứa chính xác tương tự khi (l\le p), do đó, nó xuất hiện trong đúng (p) đoạn kết thúc tại (r). Làm điều đó`current`bằng tổng số lần xuất hiện của tất cả các đoạn tương tự trên toàn bộ các đoạn kết thúc tại (r), cũng chính là tổng các số khác nhau của các đoạn đó. Khi cộng`current`vào đáp án ở mọi (r), mỗi cặp ((l,r)) được tính đúng một lần và mỗi cặp giống nhau trong đoạn được tính đúng một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    answers = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        last = {}
        current = 0
        answer = 0

        for r, x in enumerate(a, 1):
            old = last.get(x)

            if old is None:
                current += r
            else:
                current += r - old

            last[x] = r
            answer += current

        answers.append(str(answer))

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```

`last`lưu vị trí cuối cùng được xuất ra giống nhau, đúng với bước 1 và bước 3 của thuật toán. Từ điển được sử dụng vì giá trị tương tự có thể tăng lên (10^9), nên không thể sử dụng trực tiếp một mảng chỉ số`typ`. 

Biến`current`được cập nhật trước khi cộng vào`answer`. Ở vị trí (r), nó phải mô tả các đoạn cuối đúng tại (r), không phải các đoạn cuối tại (r-1). 

Biểu thức`current += r - old`là phần dễ sai nhất. Nếu một cái tương tự trước đó xuất hiện tại`old`, các đoạn có (l\le old) đã chứa các đoạn tương tự trước khi thêm phần tử thứ (r). Lựa chọn mới là (l=old+1,\ldots,r), có đúng (r-old) lựa chọn, nên đóng góp tăng cường đúng số lượng đó. 

Với lần xuất hiện đầu tiên,`old`không tồn tại và giống như mới đóng góp (r), vì mọi đoạn ([l,r]) với (l\le r) đều chứa nó. 

Chỉ số`r`bắt đầu từ (1) nhờ`enumerate(a, 1)`. Điều này làm cho vị trí cuối cùng có thể sử dụng trực tiếp làm số lượng lựa chọn của (l), tránh phải cộng hoặc trừ (1) ở nhiều nơi. 

Python không bị giới hạn số nguyên 64 bit, nên`answer`có thể chứa kết quả lớn nhất mà không cần loại dữ liệu đặc biệt. 

Một chi tiết về đầu vào là đề cho tổng (N) trên tất cả bài kiểm tra không quá (2\cdot10^6). Vì vậy, việc đọc từng bài kiểm tra và tính toán tuyến tính xử lý là phù hợp. Từ điển cũng được tạo mới cho từng bài kiểm tra để không lưu trữ dữ liệu của bài kiểm tra trước đó. 

## Ví dụ đã hoạt động 

### Trường hợp mẫu 1 

With array`1 2 3`, mỗi con thỏ có một con riêng biệt. Ta có các trạng thái sau: 

| (r) |`x`|`last`sau khi cập nhật |`current`|`answer`| 
| --- | --- | --- | --- | --- | 
| 1 | 1 |`{1: 1}`| 1 | 1 | 
| 2 | 2 |`{1: 1, 2: 2}`| 3 | 4 | 
| 3 | 3 |`{1: 1, 2: 2, 3: 3}`| 6 | 10 | 

Ở vị trí 1 có một đoạn kết thúc ở 1 và không chứa một đoạn tương tự, nên`current = 1`. Sang vị trí 2, có hai điểm giống với vị trí cuối cùng như 1 và 2, nên tổng là (3). Sang vị trí 3, tổng là (1+2+3=6). Cộng lại được (1+3+6=10), đúng mẫu. 

### Trường hợp mẫu 2 

With array`1 2 2 3`, trạng thái chi tiết là: 

| (r) |`x`|`old`|`current`sau khi cập nhật |`answer`| 
| --- | --- | --- | --- | --- | 
| 1 | 1 | chưa có | 1 | 1 | 
| 2 | 2 | chưa có | 3 | 4 | 
| 3 | 2 | 2 | 4 | 8 | 
| 4 | 3 | chưa có | 7 | 15 | 

Ở (r=3), like`2`xuất hiện lại. Trước đó, vị trí cuối cùng của nó là 2, nên đóng góp lời khuyên tương tự`2`đổi từ 2 thành 3. Vì vậy`current`tăng đúng (3-2=1), từ 3 đến 4. 

Ở (r=4), like`3`xuất hiện lần đầu và đóng góp bổ sung 4.`current`trở thành thành công (1+3+4=8). Tổng cuối cùng là (1+3+4+8=16), mẫu kết quả đúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N)) trung bình | Mỗi con thỏ thực hiện một lần nghiên cứu và cập nhật từ điển | 
| Không gian | (O(N)) | Từ điển có nhiều nhất một phần tử cho mỗi phần tử giống nhau | 

Vì tổng (N) của tất cả các bài kiểm tra không vượt quá (2\cdot10^6), tổng số thao tác là tuyến tính theo kích thước đầu vào. Đây là điều khác biệt được quyết định với (O(N^2)), vốn có thể phải xử lý hàng trăm tỷ lệ đoạn khi (N=10^6). Bộ nhớ (O(N)) cũng nằm trong giới hạn 512 MB với cách lưu một vị trí cuối cùng cho mỗi cùng. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    t = int(input())
    answers = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        last = {}
        current = 0
        answer = 0

        for r, x in enumerate(a, 1):
            old = last.get(x)

            if old is None:
                current += r
            else:
                current += r - old

            last[x] = r
            answer += current

        answers.append(str(answer))

    sys.stdout.write("\n".join(answers))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    try:
        solve()
        return output.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run(
    """2
3
1 2 3
4
1 2 2 3
"""
) == "10\n16\n", "provided samples"

# Minimum size
assert run(
    """1
1
5
"""
) == "1\n", "single rabbit"

# All rabbits have the same type
assert run(
    """1
3
7 7 7
"""
) == "6\n", "all equal"

# Repeated type with a gap
assert run(
    """1
3
1 2 1
"""
) == "9\n", "repeated type"

# Large answer and all distinct types
assert run(
    """1
5
1 2 3 4 5
"""
) == "35\n", "all distinct"

# Maximum-size test, all equal
n = 1_000_000
expected = n * (n + 1) // 2
inp = "1\n" + str(n) + "\n" + ("7 " * (n - 1)) + "7\n"
assert run(inp) == str(expected) + "\n", "maximum N"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 5`|`1`| Kích thước nhỏ nhất và trình xử lý vị trí đầu tiên | 
|`1 / 3 / 7 7 7`|`6`| Cùng một liên tục xuất hiện tương tự, kiểm tra công việc thay thế vị trí cuối cùng | 
|`1 / 3 / 1 2 1`|`9`| Xuất hiện tương tự sau một khoảng cách, kiểm tra`r - old`| 
|`1 / 5 / 1 2 3 4 5`|`35`| Tất cả đều giống nhau, kiểm tra tổng đóng góp tăng dần | 
| (N=10^6), tất cả bằng nhau`7`|`500000500000`| Giới hạn lớn nhất, tính năng tuyến tính thời gian và khả năng xử lý đáp ứng lớn | 

## Vỏ cạnh 

Với (N=1), đầu vào```
1
1
5
```từ điển cấm đầu rỗng. Tại (r=1), giống`5`chưa xuất hiện nên`current`tăng từ 0 lên 1. Sau đó`answer`cũng trở thành 1. Chỉ có một đoạn ([1,1]), và chứa đúng một đoạn tương tự, nên kết quả là`1`. 

Với tất cả các con thỏ giống nhau, có giới hạn```
1
3
7 7 7
```tại (r=1),`current=1`. Tại (r=2), vị trí cũ của`7`là 1 nên`current`tăng (2-1=1), trở thành 2. Tại (r=3), nó tăng (3-2=1), trở thành 3. Tổng là (1+2+3=6). Mỗi đoạn chỉ có một đoạn giống và có tổng cộng sáu đoạn, nên kết quả phù hợp. 

Cùng xuất hiện lại sau một khoảng cách, bình luận```
1
3
1 2 1
```sau vị trí 1,`current=1`. Sau vị trí 2, hai vị trí giống nhau có vị trí cuối cùng là 1 và 2, nên`current=3`. Ở vị trí 3, cùng`1`có vị trí cũ cuối cùng là 1 và vị trí mới là 3. Đóng góp của nó tăng dần từ 1 lên 3, nên`current`tăng thêm 2, thành 5. Tổng đáp án là (1+3+5=9). Điều này cũng tương ứng trực tiếp với các điểm của sáu đoạn, lần như (1,2,2,1,2,1). 

Với tất cả các điểm khác nhau và (N=5),```
1
5
1 2 3 4 5
```

`current`lần như là (1,3,6,10,15), vì ở mỗi tiền tố mọi vị trí cuối cùng đều khác nhau. Đáp án là (35), đúng với tổng độ dài của tất cả các đoạn. Trường hợp này kiểm tra rằng thuật toán không vô tình xem các tương tự mới là chỉ đóng góp 1, trong khi một lần xuất hiện tương tự ở vị trí (r) thực tế đóng góp (r) đoạn kết thúc tại (r). 

Với (N=10^6) và tất cả đều bằng`7`, từ điển chỉ chứa một phần tử trong suốt quá trình chạy. Mỗi vị trí chỉ thực hiện một lần nghiên cứu và một lần cập nhật nên vẫn là (O(N)).`current`lần tăng 1 ở mỗi bước và đáp án cuối cùng là 

500000500000. 
] 

Trường hợp này vừa kiểm tra giới hạn kích thước đầu vào, vừa kiểm tra công việc không dùng (O(N^2)) bộ nhớ hoặc thời gian khi số lượng tương tự khác nhau rất nhỏ.
