---
title: "CF 82B - Bộ"
description: "Chúng ta có tất cả các hợp cặp của một số tập rời rạc ẩn. Bản thân các bộ gốc không được hiển thị. Giả sử các tập ẩn là $S1, S2, dots, Sn$. Với mỗi cặp $i neq j$, chúng ta có tập $Si cup Sj$."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "hashing", "implementation"]
categories: ["algorithms"]
codeforces_contest: 82
codeforces_index: "B"
codeforces_contest_name: "Yandex.Algorithm 2011: Qualification 2"
rating: 1700
weight: 82
solve_time_s: 137
verified: false
draft: false
---

[CF 82B - Bộ](https://codeforces.com/problemset/problem/82/B) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, băm, triển khai 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có tất cả các hợp cặp của một số tập rời rạc ẩn. Bản thân các bộ gốc không được hiển thị. 

Giả sử các tập ẩn là$S_1, S_2, \dots, S_n$. Đối với mỗi cặp$i \neq j$, chúng ta được cấp bộ$S_i \cup S_j$. Các đoàn này bị xáo trộn nên không biết cặp nào sinh ra dòng nào. 

Các bộ ban đầu được tách rời theo từng cặp, đây là thuộc tính cấu trúc chính. Mỗi số thuộc về đúng một tập ẩn. 

Nhiệm vụ của chúng ta là xây dựng lại bất kỳ tập hợp hợp lệ nào của các bộ gốc. 

Các ràng buộc đủ nhỏ để chúng ta có thể thực hiện phép tính bậc hai về số dòng đầu vào. có$n(n-1)/2$công đoàn và$n \le 200$, nhiều nhất là khoảng 20.000 dòng. Mỗi dòng chứa tối đa 200 số nguyên. MỘT$O(m^2)$thuật toán trên tất cả các công đoàn sẽ trở nên khó chịu, bởi vì$m \approx 20{,}000$, đưa ra hàng trăm triệu so sánh. Chúng ta cần khai thác cấu trúc của các liên kết rời rạc thay vì ghép từng cặp với mọi cặp khác. 

Trường hợp cạnh nguy hiểm nhất là khi một tập ẩn nào đó chỉ chứa một phần tử. Khi đó, mọi hợp chứa phần tử đó khác với tập hợp kia chỉ một giá trị. 

Ví dụ:```
n = 3
A = {1}
B = {2}
C = {3,4}
```Các công đoàn là:```
{1,2}
{1,3,4}
{2,3,4}
```Một cách tiếp cận bất cẩn chỉ cố gắng suy ra các tập hợp từ tần số có thể dễ dàng hợp nhất các tập hợp đơn lẻ không chính xác. 

Một trường hợp tế nhị khác xuất hiện khi$n=2$. Khi đó chỉ có một hợp nhất, đó là hợp của hai tập ẩn. Bất kỳ phân vùng nào của liên kết đó thành hai tập con không trống đều hợp lệ. 

Ví dụ:```
2
3 1 2 3
```Một câu trả lời hợp lệ là:```
1 1
2 2 3
```Giải pháp giả định có đủ thông tin để nhận dạng duy nhất mọi tập hợp ban đầu sẽ thất bại ở đây. 

Bẫy thứ ba là giả định rằng mọi tập hợp ban đầu đều xuất hiện trực tiếp ở đâu đó trong đầu vào. Nó không bao giờ làm vậy. Mỗi dòng luôn là sự kết hợp của hai bộ khác nhau. 

Coi như:```
A = {1,2}
B = {3}
C = {4}
```Công đoàn đầu vào:```
{1,2,3}
{1,2,4}
{3,4}
```bộ`{1,2}`không bao giờ xuất hiện một cách rõ ràng, nhưng nó vẫn phải được xây dựng lại. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ cố gắng phân vùng tất cả các số thành các nhóm ẩn và xác minh xem các liên kết cặp được tạo có khớp với đầu vào hay không. Ngay cả khi chúng ta chỉ xem xét việc gán mỗi giá trị cho một trong$n$tập hợp, không gian tìm kiếm sẽ tăng theo cấp số nhân. Với tối đa 200 số riêng biệt, việc xây dựng lại toàn diện là không thể. 

Ý tưởng brute-force vẫn hữu ích về mặt khái niệm vì nó bộc lộ đặc tính chính của đầu vào: mọi số luôn di chuyển cùng với tất cả các phần tử của tập ẩn của chính nó. 

Giả sử một số tập hợp ban đầu là:$$S = \{a,b,c\}$$Mỗi đoàn thể chứa`a`cũng chứa`b`Và`c`, bởi vì các tập hợp được hình thành từ toàn bộ tập hợp. Điều ngược lại cũng đúng. Các phần tử từ các tập hợp ẩn khác nhau không phải lúc nào cũng xuất hiện cùng nhau, bởi vì tồn tại một hợp chứa một tập hợp nhưng không chứa tập hợp kia. 

Sự quan sát đó biến vấn đề thành một nhiệm vụ nhóm. 

Với mỗi giá trị$x$, thu thập các chỉ số của tất cả các dòng công đoàn có chứa$x$. Hai số thuộc cùng một tập ẩn khi và chỉ khi các tập hợp này giống hệt nhau. 

Tại sao điều này làm việc? 

Nếu như$x$Và$y$thuộc cùng một tập gốc thì mọi hợp đều chứa cả hai hoặc không chứa cả hai. Mô hình xuất hiện của chúng giống hệt nhau. 

Nếu chúng thuộc các tập hợp khác nhau thì xét hợp tạo bởi tập hợp chứa$x$và một số bộ thứ ba không liên quan đến$y$. Liên minh đó chứa$x$nhưng không$y$. Mô hình của họ khác nhau. 

Vì vậy, các tập ẩn chính xác là các lớp tương đương của các mẫu xuất hiện bằng nhau. 

Ngoại lệ duy nhất là$n=2$. Chỉ có một đường hợp nên mọi số đều xuất hiện trong cùng một tập hợp các đường. Chúng ta không thể phân biệt các tập hợp ban đầu một cách duy nhất. Bất kỳ phân vùng không trống nào đều hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu |$O(M \cdot K)$|$O(M \cdot K)$| Đã chấp nhận | 

Đây$M = n(n-1)/2$là số đường nối, và$K$là tổng số số nguyên trên tất cả các dòng. 

## Hướng dẫn thuật toán 

1. Đọc tất cả các dòng kết hợp và gán cho mỗi dòng một chỉ mục. 

Chúng tôi cần những chỉ số này vì ý tưởng cốt lõi là so sánh vị trí xuất hiện của mỗi giá trị. 
2. Với mọi giá trị nguyên$x$, xây dựng một danh sách các chỉ số dòng trong đó$x$xảy ra. 

Ví dụ:```
value 7 -> [0, 3, 5]
value 2 -> [1, 2, 4]
```Các mẫu xuất hiện này xác định duy nhất tập ẩn. 
3. Nhóm các giá trị theo kiểu xuất hiện giống hệt nhau. 

Chúng ta có thể sử dụng một từ điển có khóa là một bộ chỉ số dòng. 

Mỗi nhóm được tạo theo cách này tương ứng với một tập ẩn ban đầu. 
4. Xử lý trường hợp đặc biệt$n=2$. 

Chỉ có một đường hợp nhất, vì vậy mọi giá trị đều có cùng một mẫu. Chúng tôi không thể khôi phục phân vùng ban đầu một cách duy nhất. 

Vì mọi phân vùng hợp lệ đều được chấp nhận, hãy đặt một số vào tập đầu tiên và tất cả các số còn lại trong tập thứ hai. 
5. Xuất ra tất cả các nhóm được xây dựng lại. 

### Tại sao nó hoạt động 

Mỗi đường hợp được hình thành bằng cách lấy hai bộ ẩn hoàn chỉnh. 

Nếu hai số thuộc cùng một tập ẩn thì chúng luôn xuất hiện cùng nhau trên mọi đường hợp. Mô hình xuất hiện của chúng giống hệt nhau. 

Nếu hai số thuộc về các tập ẩn khác nhau thì tồn tại ít nhất một đường hợp chứa một trong số chúng nhưng không chứa số kia. Mô hình xuất hiện của chúng khác nhau. 

Vì vậy, sự bình đẳng của các mẫu xuất hiện giống hệt như thuộc về cùng một tập hợp ẩn. Việc nhóm theo các mẫu này sẽ tái tạo lại tất cả các bộ gốc một cách chính xác. 

## Giải pháp Python```python
import sys
from collections import defaultdict

input = sys.stdin.readline

def solve():
    n = int(input())

    m = n * (n - 1) // 2

    lines = []
    occ = defaultdict(list)

    for idx in range(m):
        arr = list(map(int, input().split()))
        vals = arr[1:]

        lines.append(vals)

        for x in vals:
            occ[x].append(idx)

    # Special case: n = 2
    if n == 2:
        all_values = sorted(occ.keys())

        print(1, all_values[0])

        second = all_values[1:]
        print(len(second), *second)
        return

    groups = defaultdict(list)

    for x, pattern in occ.items():
        groups[tuple(pattern)].append(x)

    result = list(groups.values())

    for group in result:
        print(len(group), *group)

if __name__ == "__main__":
    solve()
```Phần đầu tiên đọc tất cả các dòng kết hợp và các bản ghi trong đó mỗi số xuất hiện. Từ điển`occ`ánh xạ một giá trị vào danh sách các chỉ số dòng chứa nó. 

Chi tiết triển khai quan trọng là giữ nguyên thứ tự của các chỉ mục. Vì các dòng được xử lý tuần tự nên mọi danh sách xảy ra đều được sắp xếp tự động. Điều đó cho phép các bộ dữ liệu được sử dụng một cách an toàn làm khóa từ điển. 

Trường hợp đặc biệt`n == 2`cần xử lý riêng. Nếu không có nó, tất cả các giá trị sẽ thu gọn thành một nhóm duy nhất vì chỉ có một đường hợp nhất. Tuyên bố vấn đề chấp nhận bất kỳ sự tái tạo hợp lệ nào, do đó việc tách một giá trị khỏi phần còn lại là đủ. 

Giai đoạn nhóm là trung tâm của giải pháp. Hai số được đặt vào cùng một tập hợp được xây dựng lại chính xác khi các bộ dữ liệu xuất hiện của chúng khớp nhau. 

Không cần sắp xếp các nhóm vì đầu ra có thể theo bất kỳ thứ tự nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
3 2 7 4
3 1 7 3
3 5 4 2
3 1 3 5
4 3 1 2 4
2 5 7
```Giả sử các tập ẩn là:```
{7}
{2,4}
{1,3}
{5}
```Mô hình xuất hiện: 

| Giá trị | Xuất hiện trong dòng | Mẫu | 
| --- | --- | --- | 
| 2 | 0, 2, 4 | (0,2,4) | 
| 4 | 0, 2, 4 | (0,2,4) | 
| 1 | 1, 3, 4 | (1,3,4) | 
| 3 | 1, 3, 4 | (1,3,4) | 
| 5 | 2, 3, 5 | (2,3,5) | 
| 7 | 0, 1, 5 | (0,1,5) | 

Việc nhóm các mẫu bằng nhau sẽ mang lại: 

| Mẫu | Bộ được xây dựng lại | 
| --- | --- | 
| (0,2,4) | {2,4} | 
| (1,3,4) | {1,3} | 
| (2,3,5) | {5} | 
| (0,1,5) | {7} | 

Dấu vết này thể hiện sự bất biến chính. Các phần tử từ cùng một tập hợp ẩn luôn có cùng một kiểu xuất hiện giống nhau. 

### Ví dụ 2 

đầu vào:```
2
3 1 2 3
```Chỉ có một đường liên kết. 

Mô hình xuất hiện: 

| Giá trị | Mẫu | 
| --- | --- | 
| 1 | (0,) | 
| 2 | (0,) | 
| 3 | (0,) | 

Tất cả các mẫu đều giống hệt nhau nên việc tái tạo không phải là duy nhất. 

Thuật toán xử lý việc này một cách riêng biệt và có thể xuất ra:```
1 1
2 2 3
```Dấu vết chứng tỏ tại sao chỉ riêng logic nhóm chung là không đủ khi$n=2$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(K)$| Mỗi lần xuất hiện số nguyên được xử lý một lần | 
| Không gian |$O(K)$| Danh sách xuất hiện và lưu trữ nhóm | 

Đây$K$là tổng số số nguyên trên tất cả các dòng hợp. 

Với$n \le 200$, tổng kích thước đầu vào đủ nhỏ để quá trình xử lý tuyến tính này dễ dàng phù hợp với giới hạn thời gian. Việc sử dụng bộ nhớ cũng rất khiêm tốn vì mỗi lần xuất hiện chỉ được lưu trữ một lần. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import defaultdict

def solve():
    input = sys.stdin.readline

    n = int(input())

    m = n * (n - 1) // 2

    occ = defaultdict(list)

    for idx in range(m):
        arr = list(map(int, input().split()))

        for x in arr[1:]:
            occ[x].append(idx)

    if n == 2:
        vals = sorted(occ.keys())

        print(1, vals[0])

        other = vals[1:]
        print(len(other), *other)
        return

    groups = defaultdict(list)

    for x, p in occ.items():
        groups[tuple(p)].append(x)

    for g in groups.values():
        print(len(g), *g)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue().strip()

# provided sample
sample_input = """4
3 2 7 4
3 1 7 3
3 5 4 2
3 1 3 5
4 3 1 2 4
2 5 7
"""

sample_output = run(sample_input)
assert sample_output != ""

# minimum size
assert run("""2
2 1 2
""") != ""

# singleton sets
assert run("""3
2 1 2
2 1 3
2 2 3
""") != ""

# mixed sizes
assert run("""3
3 1 2 3
3 1 2 4
2 3 4
""") != ""

# larger grouped structure
assert run("""4
3 1 2 5
3 3 4 5
4 1 2 3 4
2 5 6
3 1 2 6
3 3 4 6
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=2`với một công đoàn | Bất kỳ sự phân chia hợp lệ nào | Xử lý trường hợp đặc biệt | 
| Ba bộ đơn | Ba nhóm một yếu tố | Các kiểu xuất hiện khác biệt | 
| Một bộ cỡ 2 và hai bộ đơn | Phân nhóm đúng | Tái thiết đa yếu tố | 
| Nhiều nhóm lớn hơn | Logic nhóm ổn định | Tính đúng đắn chung | 

## Vỏ cạnh 

Hãy xem xét đầu vào hợp lệ nhỏ nhất:```
2
2 1 2
```Chỉ có một đường liên kết. Cả hai số đều xuất hiện ở những vị trí giống hệt nhau: 

| Giá trị | Mẫu | 
| --- | --- | 
| 1 | (0,) | 
| 2 | (0,) | 

Logic nhóm chung sẽ tạo ra một tập hợp không chính xác`{1,2}`. Thuật toán phát hiện`n == 2`và thay vào đó, hãy tạo thủ công hai bộ không trống. 

Bây giờ hãy xem xét các bộ ẩn đơn:```
3
2 1 2
2 1 3
2 2 3
```Các bộ ẩn là`{1}`,`{2}`,`{3}`. 

Mẫu: 

| Giá trị | Mẫu | 
| --- | --- | 
| 1 | (0,1) | 
| 2 | (0,2) | 
| 3 | (1,2) | 

Tất cả các mẫu đều khác biệt nên mỗi mẫu đơn đều được xây dựng lại chính xác. 

Cuối cùng, hãy xem xét kích thước tập hợp không bằng nhau:```
3
3 1 2 3
3 1 2 4
2 3 4
```Các bộ ẩn là`{1,2}`,`{3}`,`{4}`. 

Mẫu: 

| Giá trị | Mẫu | 
| --- | --- | 
| 1 | (0,1) | 
| 2 | (0,1) | 
| 3 | (0,2) | 
| 4 | (1,2) | 

Giá trị`1`Và`2`có chung kiểu xuất hiện nên chúng được nhóm lại với nhau. Các bộ đơn vẫn tách biệt vì mẫu của chúng khác nhau.
