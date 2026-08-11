---
title: "CF 102396I - Trò ảo thuật"
description: "Artem bắt đầu bằng phép hoán vị tuần hoàn của các số từ (1) đến (n). Đối với mọi vị trí, anh ấy xem xét vị trí đó và hai vị trí tiếp theo, bao quanh ở phần cuối. Do đó, từ một hoán vị [ [a1,a2,ldots,an] ] anh ta tạo ra (n) bộ ba không có thứ tự [ {ai,a{i+1},a{i+2}}."
date: "2026-08-11T15:44:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102396
codeforces_index: "I"
codeforces_contest_name: "2019-2020 Saint-Petersburg Open High School Programming Contest (SpbKOSHP 19)"
rating: 0
weight: 102396
solve_time_s: 205
verified: true
draft: false
---

[CF 102396I - Trò ảo thuật](https://codeforces.com/problemset/problem/102396/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Artem bắt đầu bằng phép hoán vị tuần hoàn của các số từ (1) đến (n). Đối với mọi vị trí, anh ấy xem xét vị trí đó và hai vị trí tiếp theo, bao quanh ở phần cuối. Như vậy, từ một hoán vị 

[ 
[a_1,a_2,\ldots,a_n] 
] 

anh ta tạo ra (n) bộ ba không có thứ tự 

[ 
{a_i,a_{i+1},a_{i+2}}. 
] 

Thứ tự bên trong mỗi bộ ba bị phá hủy và bản thân các bộ ba cũng bị xáo trộn. Chúng tôi chỉ nhận được (n) bộ ba đó và phải khôi phục mọi hoán vị có thể tạo ra chúng. 

Khó khăn chính là lệnh ban đầu đã bị xóa hai lần. Chúng ta không thể biết bộ ba nào có trước và chúng ta không thể biết phần tử nào của bộ ba có trước. Giải pháp phải xây dựng lại trật tự tuần hoàn ẩn từ sự chồng chéo giữa các bộ ba. 

Ràng buộc (n\le 200.000) loại trừ mọi thứ bậc hai và chắc chắn mọi thứ liên quan đến hoán vị một cách rõ ràng. Với giới hạn một giây, giải pháp dự định chỉ cần xử lý một lượng thông tin không đổi trên mỗi bộ ba đầu vào, về cơ bản mang lại thời gian tuyến tính. Bản thân đầu vào chứa (3n) số nguyên, vì vậy công việc (O(n)) cũng là mục tiêu tự nhiên. 

Có hai trường hợp nhỏ trong đó quan sát chồng chéo chung hoạt động khác nhau. Khi (n=3), mọi cửa sổ đều chứa cả ba phần tử, do đó tất cả các bộ ba được báo cáo đều giống hệt nhau. Ví dụ,```
3
1 2 3
2 3 1
3 1 2
```có đầu ra đúng`1 2 3`, nhưng mọi hoán vị của (1,2,3) cũng đúng. Việc triển khai dự kiến ​​mỗi cặp xảy ra chính xác hai lần sẽ thất bại ở đây vì mọi cặp đều xảy ra ở cả ba bộ ba. 

Khi (n=4), mọi bộ ba được báo cáo chỉ đơn giản là toàn bộ tập hợp bốn giá trị đã loại bỏ một giá trị. Ví dụ,```
4
1 2 3
1 2 4
1 3 4
2 3 4
```có thể đến từ bất kỳ hoán vị nào của (1,2,3,4). Mỗi cặp không có thứ tự xuất hiện theo đúng hai bộ ba, do đó, việc xử lý các cặp xuất hiện hai lần dưới dạng các cạnh của chu trình ẩn sẽ tạo ra một đồ thị hoàn chỉnh thay vì một chu trình một cách không chính xác. Chúng ta có thể xử lý trực tiếp (n=4) bằng cách in (1,2,3,4), điều này luôn hợp lệ theo lời hứa rằng đầu vào đến từ một số hoán vị. 

Ngoài ra còn có một chi tiết trình bày quan trọng đối với (n) lớn hơn. Các bộ ba là các tập hợp, do đó cặp ((x,y)) phải được xử lý giống hệt với ((y,x)). Việc lưu trữ các cặp theo thứ tự đã sắp xếp sẽ tránh việc vô tình tạo ra hai cạnh đồ thị khác nhau cho cùng một cặp không có thứ tự. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ thử mọi hoán vị của (1,\ldots,n), tạo ra (n) bộ ba tuần hoàn của nó và so sánh các bộ ba đó với đầu vào. Điều này đúng vì đầu vào được đảm bảo có ít nhất một hoán vị tạo ra, do đó cuối cùng sẽ tìm thấy một hoán vị hợp lệ. Tuy nhiên, có (n!) hoán vị và việc kiểm tra một ứng cử viên yêu cầu (\Theta(n)) hoạt động. Tổng độ phức tạp là (\Theta(n\cdot n!)). Đối với (n=10), điều này có nghĩa là khoảng (10\cdot 10! = 36.288.000) lượt kiểm tra cửa sổ, trong khi giới hạn thực tế đạt đến (200.000). 

Lực lượng vũ phu hoạt động vì hoán vị ứng viên chứa chính xác thông tin chúng ta cần, nhưng nó không thành công vì nó tìm kiếm toàn bộ không gian hoán vị thay vì trích xuất các mối quan hệ cục bộ mà bộ ba bảo toàn. 

Hãy xem xét hai cửa sổ liên tiếp trong hoán vị tuần hoàn ban đầu: 

[ 
{a_i,a_{i+1},a_{i+2}} 
] 

và 

[ 
{a_{i+1},a_{i+2},a_{i+3}}. 
] 

Chúng có đúng hai phần tử chung là (a_{i+1}) và (a_{i+2}). Quan trọng hơn, hai phần tử chung tạo thành một cặp liên tiếp trong hoán vị ẩn. 

Bây giờ hãy xem xét một cặp lân cận thực sự (a_i,a_{i+1}). Nó xuất hiện trong đúng hai cửa sổ: cửa sổ bắt đầu tại (a_{i-1}) và cửa sổ bắt đầu tại (a_i). Do đó, với (n\ge5), các cặp không có thứ tự xuất hiện trong đúng hai bộ ba được báo cáo chính xác là các cặp lân cận của hoán vị tuần hoàn ẩn. 

Điều này cho chúng ta một biểu đồ về các giá trị (1,\ldots,n). Đối với mỗi bộ ba đầu vào ({x,y,z}), chúng tôi đếm số lần xuất hiện của ba cặp không có thứ tự ({x,y}), ({x,z}) và ({y,z}). Sau khi xử lý tất cả các bộ ba, chúng tôi giữ lại chính xác các cặp có số lượng là hai. Đối với (n\ge5), các cặp này tạo thành một chu trình chứa mọi giá trị đúng một lần. 

Khi đã biết chu trình đó, việc đi qua nó sẽ mang lại hoán vị mong muốn. Cả hướng và mọi phép quay theo chu kỳ đều hợp lệ vì cấu trúc ban đầu là tuần hoàn và các bộ ba không có thứ tự. 

Việc quan sát thấy các phần tử liên tiếp chính xác là các cặp xuất hiện hai lần sẽ làm giảm việc tái cấu trúc thành việc đếm cặp theo sau là một chu trình duyệt đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (\Theta(n\cdot n!)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n)) dự kiến ​​| (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc (n) bộ ba được báo cáo. Đối với mỗi bộ ba ((x,y,z)), hãy chuẩn hóa từng cặp trong số ba cặp của nó bằng cách lưu trữ điểm cuối nhỏ hơn trước. Tăng số lượng của ((x,y)), ((x,z)) và ((y,z)). Mỗi bộ ba đóng góp chính xác ba lần xuất hiện của cặp, vì vậy việc này đòi hỏi công việc không đổi trên mỗi dòng đầu vào. 
2. Nếu (n\le4), in (1,2,\ldots,n). Với (n=3), mọi bộ ba đều là ({1,2,3}), nên mọi hoán vị đều có tác dụng. Đối với (n=4), bốn cửa sổ là bốn bộ ba có thể có được bằng cách loại bỏ một giá trị, độc lập với thứ tự tuần hoàn, do đó hoán vị đồng nhất cũng hoạt động. 
3. Với (n\ge5), hãy tạo đồ thị vô hướng sử dụng mọi cặp có tần số đúng bằng hai. Cặp như vậy phải liên tiếp trong hoán vị ẩn vì mọi cặp liên tiếp đều thuộc về hai cửa sổ ngay trước và sau cặp đó. 
4. Bắt đầu từ giá trị (1), thêm nó vào câu trả lời và liên tục di chuyển đến hàng xóm chưa được ghé thăm. Vì đồ thị cặp hợp lệ chính xác là chu trình ẩn nên mỗi đỉnh có hai lân cận và phép duyệt đi qua mỗi giá trị đúng một lần. 
5. Dừng lại sau khi (n) giá trị đã được thu thập và in chúng. Chuỗi kết quả theo sau mỗi cặp xuất hiện hai lần, do đó, các cặp liên tiếp theo chu kỳ của nó chính xác là các cặp lân cận bị ẩn. 

### Tại sao nó hoạt động 

Đối với hoán vị ban đầu, mọi cặp liên tiếp (a_i,a_{i+1}) xuất hiện trong đúng hai cửa sổ, cụ thể là các cửa sổ bắt đầu tại (i-1) và (i). Đối với (n\ge5), không có cặp không liên tiếp nào có thể xuất hiện trong hai cửa sổ. Một cặp có các phần tử được phân tách bằng hai vị trí chỉ xuất hiện trong một cửa sổ có chiều dài ba và các cặp được phân tách bằng nhiều hơn hai vị trí sẽ không xuất hiện trong cửa sổ nào. Do đó các cặp có tần số 2 chính xác là các cạnh của hoán vị tuần hoàn. 

Do đó, đồ thị được xây dựng là một chu trình đơn chứa tất cả (n) giá trị. Đi qua chu trình đó mang lại hoán vị ban đầu cho phép quay và đảo chiều. Cả hai phép biến đổi đều bảo toàn tập hợp các bộ ba tuần hoàn không có thứ tự, do đó hoán vị được tạo ra luôn hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    pair_count = {}

    for _ in range(n):
        x, y, z = map(int, input().split())

        p = (x, y) if x < y else (y, x)
        pair_count[p] = pair_count.get(p, 0) + 1

        p = (x, z) if x < z else (z, x)
        pair_count[p] = pair_count.get(p, 0) + 1

        p = (y, z) if y < z else (z, y)
        pair_count[p] = pair_count.get(p, 0) + 1

    if n <= 4:
        print(*range(1, n + 1))
        return

    graph = [[] for _ in range(n + 1)]

    for (x, y), cnt in pair_count.items():
        if cnt == 2:
            graph[x].append(y)
            graph[y].append(x)

    ans = []
    prev = 0
    cur = 1

    for _ in range(n):
        ans.append(cur)

        nxt = graph[cur][0] if graph[cur][0] != prev else graph[cur][1]
        prev, cur = cur, nxt

    print(*ans)

if __name__ == "__main__":
    solve()
```Từ điển`pair_count`lưu trữ các cặp không có thứ tự. Sắp xếp từng cặp về mặt khái niệm là đủ và các biểu thức điều kiện tránh tạo ra một danh sách được sắp xếp không cần thiết cho mỗi cặp. 

Mỗi bộ ba đầu vào đóng góp chính xác ba mục vào từ điển. Vì có (n) bộ ba nên chỉ có (3n) lần xuất hiện của cặp được xử lý. 

Nhánh nhỏ-(n) xuất hiện trước khi xây dựng đồ thị vì đặc tính tần số-hai không hợp lệ cho (n=3) hoặc (n=4). Cụ thể, với (n=4), mỗi cặp xảy ra hai lần, do đó đồ thị kết quả sẽ là (K_4), chứ không phải chu trình mong muốn. 

Với (n\ge5), mỗi đỉnh có đúng hai đỉnh lân cận trong`graph`. Trong quá trình truyền tải,`prev`ghi lại đỉnh mà chúng ta đã đến từ đó. Tại đỉnh hiện tại, một người hàng xóm là`prev`, vậy hàng xóm còn lại là hướng mà chu kỳ tiếp tục. biểu thức```
nxt = graph[cur][0] if graph[cur][0] != prev else graph[cur][1]
```chọn người hàng xóm khác. 

Không có vấn đề tràn số nguyên trong Python. Số lượng cặp lớn nhất chỉ là (n), mặc dù đối với vấn đề này, đầu vào hợp lệ thực sự cung cấp số lượng cặp liên quan nhiều nhất là hai cho (n\ge5). 

Việc truyền tải có chủ ý không thêm kết quả quay lại cuối cùng vào đỉnh (1). Câu trả lời phải chứa từng giá trị hoán vị (n) một lần và cách giải thích tuần hoàn ngầm kết nối giá trị được in cuối cùng trở lại giá trị đầu tiên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
6
3 4 1
5 1 6
5 4 2
2 4 3
2 5 6
6 1 3
```Đếm tần số cặp và chỉ giữ lại tần số - hai cặp sẽ tạo ra chu trình ẩn 

[ 
1-3-4-2-5-6-1. 
] 

Việc truyền tải từ (1) tiến hành như sau. 

| Bước | Hiện tại | Trước | Được chọn tiếp theo | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 3 | 1 | 
| 2 | 3 | 1 | 4 | 1 3 | 
| 3 | 4 | 3 | 2 | 1 3 4 | 
| 4 | 2 | 4 | 5 | 1 3 4 2 | 
| 5 | 5 | 2 | 6 | 1 3 4 2 5 | 
| 6 | 6 | 5 | 1 | 1 3 4 2 5 6 | 

Quá trình chuyển đổi cuối cùng từ (6) sang (1) sẽ đóng chu trình nhưng không được in lần thứ hai. Kết quả là`1 3 4 2 5 6`, phù hợp với đầu ra mẫu. Tổng quát hơn, bắt đầu từ một đỉnh khác hoặc đi theo hướng ngược lại cũng sẽ tạo ra câu trả lời hợp lệ. 

### Mẫu 2 

Đầu vào là```
3
1 2 3
2 3 1
1 2 3
```Mỗi bộ ba được báo cáo đại diện cho cùng một tập hợp ({1,2,3}). Vì (n=3), thuật toán ngay lập tức lấy nhánh nhỏ. 

| Bước | Tình trạng | Hành động | Trả lời | 
| --- | --- | --- | --- | 
| 1 | (n\le4) | In (1,2,3) | 1 2 3 | 

Đầu ra`1 2 3`hợp lệ vì mọi cửa sổ tuần hoàn của bất kỳ hoán vị nào của ba giá trị đều chứa cả ba giá trị. Ví dụ này chứng minh tại sao việc cố gắng suy ra chu kỳ từ các cặp tần số mà không xử lý (n=3) riêng biệt sẽ không chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) dự kiến ​​| Có các phép chèn cặp (3n) và các phép toán đồ thị (O(n)). Từ điển Python cung cấp khả năng chèn và tra cứu (O(1)) dự kiến. | 
| Không gian | (O(n)) | Có nhiều nhất (3n) cặp không có thứ tự riêng biệt và có chính xác (2n) cạnh đồ thị được giữ lại cho (n\ge5). | 

Đầu vào chứa (3n) số nguyên, do đó quá trình xử lý tuyến tính gần đạt mức tối ưu. Với (n\le200.000), thuật toán chỉ thực hiện một số lượng không đổi các phép toán từ điển và kề cận trên mỗi bộ ba đầu vào và duy trì thoải mái trong giới hạn bộ nhớ 512 MB đã nêu và mục tiêu một giây, tùy thuộc vào môi trường thực thi Python thông thường. 

## Trường hợp thử nghiệm 

Trường hợp "tất cả các giá trị bằng nhau" được yêu cầu không thể là phép thử hợp lệ cho vấn đề này vì hoán vị ẩn chứa mọi giá trị chính xác một lần và mỗi bộ ba được báo cáo chứa ba giá trị riêng biệt. Thay vào đó, trường hợp cạnh giá trị trùng lặp có ý nghĩa là trường hợp (n=3), trong đó tất cả các bộ ba được báo cáo đều bằng nhau dưới dạng tập hợp. 

Đối với thử nghiệm dựa trên xác nhận, sẽ an toàn hơn khi xác thực rằng hoán vị được tạo ra sẽ tái tạo bộ ba đầu vào, thay vì yêu cầu một đầu ra cụ thể, vì vấn đề rõ ràng cho phép nhiều hoán vị hợp lệ.```python
import sys
import io
from collections import Counter

def solve_data(inp: str) -> str:
    data = inp.strip().split()
    it = iter(data)

    n = int(next(it))
    pair_count = {}

    for _ in range(n):
        x = int(next(it))
        y = int(next(it))
        z = int(next(it))

        for a, b in ((x, y), (x, z), (y, z)):
            if a > b:
                a, b = b, a
            pair_count[(a, b)] = pair_count.get((a, b), 0) + 1

    if n <= 4:
        return " ".join(map(str, range(1, n + 1)))

    graph = [[] for _ in range(n + 1)]

    for (x, y), cnt in pair_count.items():
        if cnt == 2:
            graph[x].append(y)
            graph[y].append(x)

    ans = []
    prev = 0
    cur = 1

    for _ in range(n):
        ans.append(cur)
        if graph[cur][0] != prev:
            nxt = graph[cur][0]
        else:
            nxt = graph[cur][1]
        prev, cur = cur, nxt

    return " ".join(map(str, ans))

def valid_permutation(output: str, inp: str) -> bool:
    tokens = list(map(int, output.split()))
    data = list(map(int, inp.split()))

    n = data[0]
    if len(tokens) != n or sorted(tokens) != list(range(1, n + 1)):
        return False

    given = Counter()
    pos = 1

    for _ in range(n):
        triple = tuple(sorted(data[pos:pos + 3]))
        given[triple] += 1
        pos += 3

    produced = Counter()
    for i in range(n):
        triple = tuple(sorted((
            tokens[i],
            tokens[(i + 1) % n],
            tokens[(i + 2) % n],
        )))
        produced[triple] += 1

    return given == produced

sample1 = """\
6
3 4 1
5 1 6
5 4 2
2 4 3
2 5 6
6 1 3
"""

sample2 = """\
3
1 2 3
2 3 1
1 2 3
"""

assert solve_data(sample1) == "1 3 4 2 5 6", "sample 1"
assert solve_data(sample2) == "1 2 3", "sample 2"

case_n3 = """\
3
1 2 3
3 1 2
2 3 1
"""
out = solve_data(case_n3)
assert valid_permutation(out, case_n3), "n=3 duplicate triples"

case_n4 = """\
4
1 2 3
1 2 4
1 3 4
2 3 4
"""
out = solve_data(case_n4)
assert valid_permutation(out, case_n4), "n=4 complete pair graph"

case_n5 = """\
5
1 2 3
2 3 4
3 4 5
4 5 1
5 1 2
"""
out = solve_data(case_n5)
assert valid_permutation(out, case_n5), "n=5 pair-frequency boundary"

case_n6_reversed = """\
6
6 5 4
5 4 3
4 3 2
3 2 1
2 1 6
1 6 5
"""
out = solve_data(case_n6_reversed)
assert valid_permutation(out, case_n6_reversed), "reversed cycle"

# Large boundary case.
n = 200000
large_lines = [str(n)]
perm = list(range(1, n + 1))

for i in range(n):
    large_lines.append(
        f"{perm[i]} {perm[(i + 1) % n]} {perm[(i + 2) % n]}"
    )

large_case = "\n".join(large_lines)
out = solve_data(large_case)
assert valid_permutation(out, large_case), "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 |`1 3 4 2 5 6`| Tái thiết bình thường từ tần số cặp | 
| Mẫu 2 |`1 2 3`| (n=3), trong đó mọi bộ ba đều giống hệt nhau | 
|`n=3`với ba bản được sắp xếp lại | Bất kỳ hoán vị hợp lệ nào | Bộ ba báo cáo trùng lặp | 
| Bốn bộ ba chứa mọi tập hợp con 3 của ({1,2,3,4}) |`1 2 3 4`hợp lệ | (n=4), trong đó mỗi cặp xảy ra hai lần | 
| Năm bộ ba tuần hoàn | Bất kỳ vòng quay hoặc đảo ngược nào | Kích thước đầu tiên trong đó đồ thị tần số hai trở thành một chu trình | 
| Sáu giá trị theo thứ tự tuần hoàn ngược | Bất kỳ sự tái thiết theo chu kỳ hợp lệ nào | Định hướng và xoay theo chu kỳ không thành vấn đề | 
| (n=200.000) chu kỳ nhận dạng | Bất kỳ hoán vị hợp lệ nào | Kích thước đầu vào tối đa và hành vi thời gian tuyến tính | 

## Vỏ cạnh 

Với (n=3), hãy xem xét```
3
1 2 3
3 1 2
2 3 1
```Sau khi sắp xếp từng bộ ba, cả ba đầu vào trở thành ((1,2,3)). Do đó, mỗi cặp đều có tần số ba chứ không phải hai. Thuật toán không cố gắng xây dựng biểu đồ cặp và in trực tiếp`1 2 3`. Tất cả các cửa sổ tuần hoàn của nó đều là ({1,2,3}), vì vậy đầu ra hợp lệ. 

Với (n=4), hãy xem xét```
4
1 2 3
1 2 4
1 3 4
2 3 4
```Mỗi cặp xuất hiện đúng hai bộ ba. Do đó, việc triển khai tần số chung hai sẽ kết nối mọi cặp, tạo ra sáu cạnh thay vì bốn cạnh của một chu kỳ. Thuật toán tránh điều này bằng cách in`1 2 3 4`ngay lập tức. Cửa sổ tuần hoàn của nó là ({1,2,3}), ({2,3,4}), ({1,3,4}) và ({1,2,4}), chính xác là bốn bộ ba đầu vào. 

Với (n=5), hành vi đặc biệt biến mất. Lấy```
5
1 2 3
2 3 4
3 4 5
4 5 1
5 1 2
```Các cặp xuất hiện hai lần là 

[ 
(1,2),(2,3),(3,4),(4,5),(1,5). 
] 

Chúng tạo thành chu trình (1-2-3-4-5-1). Bắt đầu từ (1), việc duyệt tạo ra`1 2 3 4 5`. Điều này xác nhận ranh giới mà tại đó đặc tính tần số hai trở nên hợp lệ. 

Thứ tự bên trong bộ ba đầu vào cũng không thể tin cậy được. Trong mẫu 1, bộ ba`3 4 1`thể hiện chính xác thông tin giống như`1 3 4`. Thuật toán không bao giờ dựa vào thứ tự đầu vào vì mọi cặp đều được chuẩn hóa trước khi đếm. Tương tự như vậy, sáu bộ ba được báo cáo có thể xuất hiện theo thứ tự tùy ý, nhưng tần số cặp độc lập với thứ tự đọc các bộ ba. 

Cuối cùng, bản thân câu trả lời không nhất thiết phải khớp với ký tự hoán vị ban đầu của Artem cho ký tự. Nếu hoán vị ẩn là (1,2,3,4,5), thì (3,4,5,1,2) biểu thị cùng một thứ tự tuần hoàn và (5,4,3,2,1) biểu thị nó theo hướng ngược lại. Cả hai đều tạo ra chính xác cùng một tập hợp các bộ ba không có thứ tự. Việc duyệt đồ thị khai thác sự tự do này một cách tự nhiên bằng cách chọn đỉnh (1) làm điểm bắt đầu và chọn tùy ý một trong hai hướng chu kỳ.
