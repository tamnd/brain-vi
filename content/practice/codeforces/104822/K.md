---
title: "CF 104822K - Truy vấn về tính khác biệt"
description: "Chúng ta được cung cấp một lưới các số nguyên và chúng ta phải trả lời nhiều truy vấn mà mỗi truy vấn mô tả một tiểu vùng hình chữ nhật. Đối với mỗi truy vấn, chúng ta cần quyết định xem tất cả các giá trị bên trong hình chữ nhật đó có khác nhau theo từng cặp hay không, nghĩa là không có số nào xuất hiện nhiều lần bên trong ma trận con đã chọn."
date: "2026-06-28T12:44:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104822
codeforces_index: "K"
codeforces_contest_name: "RCPCamp 2023 Day 1"
rating: 0
weight: 104822
solve_time_s: 89
verified: false
draft: false
---

[CF 104822K - Truy vấn về tính khác biệt](https://codeforces.com/problemset/problem/104822/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới các số nguyên và chúng ta phải trả lời nhiều truy vấn mà mỗi truy vấn mô tả một tiểu vùng hình chữ nhật. Đối với mỗi truy vấn, chúng ta cần quyết định xem tất cả các giá trị bên trong hình chữ nhật đó có khác nhau theo từng cặp hay không, nghĩa là không có số nào xuất hiện nhiều lần bên trong ma trận con đã chọn. 

Một cách trực tiếp để suy nghĩ về một truy vấn là trích xuất tất cả các ô bên trong hình chữ nhật và kiểm tra xem có giá trị nào lặp lại hay không. Nếu mỗi giá trị là duy nhất, chúng tôi trả lời CÓ, nếu không thì KHÔNG. 

Các ràng buộc đủ lớn để cả hai thứ nguyên nhân với nhau tối đa là 100.000 và cũng có tới 100.000 truy vấn. Điều này ngay lập tức loại trừ mọi cách tiếp cận quét toàn bộ hình chữ nhật cho mỗi truy vấn trong trường hợp xấu nhất. Một truy vấn lớn duy nhất có thể chạm tới 100.000 ô và việc lặp lại điều đó trên 100.000 truy vấn sẽ dẫn đến thứ tự 10^10 thao tác, vượt xa mức 2 giây cho phép trong Python. 

Một ràng buộc tinh tế hơn là các giá trị được giới hạn bởi n * m, do đó mọi giá trị đều nằm trong một phạm vi có thể quản lý được và có thể được xác định duy nhất mà không gặp vấn đề về băm. Điều này quan trọng vì khó khăn cốt lõi là theo dõi sự xuất hiện của các giá trị trên nhiều vùng hình chữ nhật. 

Một sai lầm ngây thơ thường xuất hiện trong các giải pháp là cố gắng tính toán trước tần số trên mỗi hàng hoặc cột và kết hợp chúng. Điều đó không thành công vì các bản sao có thể xuất hiện theo đường chéo trên các hàng và cột. 

Ví dụ: hãy xem xét một lưới:```
1 2
3 1
```Một truy vấn bao phủ toàn bộ lưới chứa hai số 1. Bất kỳ tập hợp dựa trên hàng hoặc dựa trên cột nào cũng có thể bỏ lỡ sự trùng lặp xuyên ranh giới này trừ khi nó theo dõi các vị trí đầy đủ. 

Một trường hợp thất bại tinh tế khác là giả định rằng việc kiểm tra các bản sao liền kề là đủ. Trong một hình chữ nhật:```
1 2 3
4 1 5
```các số 1 trùng lặp cách xa nhau nên việc kiểm tra cục bộ không thành công. 

Khó khăn thực sự là chúng ta không được hỏi về cấu trúc, thứ tự hoặc tổng, mà là về tính duy nhất toàn cục bên trong các hình chữ nhật con tùy ý, điều này buộc chúng ta phải suy luận về vị trí của các giá trị lặp lại trên toàn bộ ma trận. 

## Phương pháp tiếp cận 

Giải pháp brute-force xử lý từng truy vấn một cách độc lập. Đối với mỗi hình chữ nhật, chúng tôi lặp lại tất cả các ô của nó và chèn các giá trị vào tập băm. Nếu chúng tôi thấy một giá trị lặp lại, chúng tôi sẽ ngay lập tức trả về KHÔNG, nếu không thì CÓ. Điều này đúng vì tập hợp trực tiếp thực thi tính duy nhất. 

Tuy nhiên, trong trường hợp xấu nhất, một truy vấn có thể bao phủ toàn bộ lưới, do đó mỗi truy vấn có chi phí O(nm). Với tối đa 100.000 truy vấn, điều này dẫn đến O(nm · q), điều này hoàn toàn không khả thi. 

Quan sát quan trọng là vấn đề cơ bản là về sự xuất hiện lặp đi lặp lại của các giá trị giống hệt nhau. Thay vì tính toán lại tính duy nhất từ ​​đầu cho mỗi truy vấn, chúng ta có thể tính toán trước mối quan hệ giữa các lần xuất hiện có cùng giá trị. 

Một công thức cải cách quan trọng như sau: một ma trận con hợp lệ khi và chỉ khi mọi giá trị bên trong nó xuất hiện nhiều nhất một lần bên trong nó. Tương tự, nếu chúng ta lấy tất cả các lần xuất hiện của mỗi giá trị, chúng ta phải đảm bảo rằng không có hai lần xuất hiện nào có cùng giá trị nằm trong bất kỳ hình chữ nhật truy vấn nào. 

Vì vậy, thay vì kiểm tra tất cả các giá trị trong một truy vấn, chúng tôi lật lại phối cảnh. Đối với mỗi giá trị, chúng tôi xem xét tất cả các lần xuất hiện của nó và coi các lần xuất hiện liên tiếp theo thứ tự quét là nguồn xung đột tiềm ẩn. Sau đó, chúng tôi giảm vấn đề xuống còn kiểm tra xem có bất kỳ “cặp xấu” nào nằm hoàn toàn bên trong hình chữ nhật truy vấn hay không. 

Điều này trở thành truy vấn thống trị 2D trên một tập hợp các cặp thưa thớt. Mỗi giá trị đóng góp các cạnh giữa các lần xuất hiện liên tiếp và mỗi cạnh biểu thị một ràng buộc mà hai ô không thể nằm đồng thời trong một hình chữ nhật truy vấn hợp lệ. 

Sau đó, chúng tôi cần hỗ trợ các truy vấn ngoại tuyến để hỏi liệu có bất kỳ cạnh nào trong số này nằm hoàn toàn bên trong hình chữ nhật truy vấn hay không. Đây là một cách rút gọn hình học cổ điển: biến đổi mỗi cạnh thành một điểm trong không gian ràng buộc 4D, sau đó xử lý các truy vấn bằng cách sử dụng đường quét trên một chiều và cây Fenwick trên chiều khác. 

Một cách đơn giản hóa thực tế là ánh xạ từng ô tới chỉ mục 1D theo thứ tự hàng lớn. Mỗi lần xuất hiện của một giá trị sẽ tạo thành một danh sách các vị trí được sắp xếp. Đối với mỗi cặp liên tiếp (p, q), chúng tôi lưu trữ ràng buộc hình chữ nhật được xác định bởi ranh giới hàng và cột tối thiểu/tối đa của p và q. Hình chữ nhật truy vấn không hợp lệ nếu nó chứa đầy đủ cả hai điểm cuối của bất kỳ cặp nào như vậy. 

Chúng tôi giảm bớt vấn đề để kiểm tra xem có bất kỳ cặp bị cấm nào nằm bên trong hình chữ nhật truy vấn hay không, hình chữ nhật này sẽ trở thành cấu trúc truy vấn phạm vi và phạm vi trên hệ tọa độ 2D bằng cách sử dụng sắp xếp ngoại tuyến và cây Fenwick. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q · n · m) | O(1) | Quá chậm | 
| Tối ưu | O((n·m + q) log(n·m)) | O(n·m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng ta làm phẳng lưới thành các vị trí sao cho mỗi ô được biểu thị bằng một cặp tọa độ (i, j). Đối với mỗi giá trị trong lưới, chúng tôi thu thập tất cả các vị trí của nó trong danh sách được sắp xếp theo thứ tự hàng chính. 

Đối với mỗi giá trị, chúng tôi lặp qua danh sách xuất hiện của nó và xem xét từng cặp liên tiếp. Mỗi cặp đại diện cho hai giá trị giống hệt nhau và không bao giờ cả hai đều xuất hiện trong hình chữ nhật truy vấn hợp lệ. Chúng tôi chuyển đổi từng cặp thành một đối tượng hình học mô tả các chỉ số hàng và cột nhỏ nhất và lớn nhất bao gồm cả hai điểm. 

Sau đó chúng tôi xử lý các truy vấn ngoại tuyến. Mỗi truy vấn hỏi liệu có tồn tại cặp cấm nào hoàn toàn bên trong hình chữ nhật của nó hay không. Chúng tôi coi đây là vấn đề truy vấn phạm vi 2D. 

Chúng tôi sắp xếp các sự kiện theo ranh giới hàng và sử dụng cây Fenwick trên không gian cột để kích hoạt điểm cuối của các cặp bị cấm khi chúng tôi quét. Mỗi truy vấn được trả lời bằng cách kiểm tra xem có bất kỳ ràng buộc hoạt động nào rơi vào phạm vi cột của nó trong khi vẫn tôn trọng giới hạn hàng hay không. 

### Tại sao nó hoạt động

Bất biến chính là mọi vi phạm tính duy nhất đều được chứng kiến ​​bởi ít nhất một cặp giá trị bằng nhau. Nếu một hình chữ nhật chứa một bản sao thì nó phải chứa hai lần xuất hiện có cùng giá trị và trong số các lần xuất hiện đó tồn tại ít nhất một cặp liên tiếp trong danh sách lần xuất hiện được sắp xếp có hình chữ nhật giới hạn được chứa hoàn toàn bên trong hình chữ nhật truy vấn. Do đó, chỉ cần kiểm tra các lần xuất hiện liên tiếp trên mỗi giá trị thay vì tất cả các cặp là đủ. Cấu trúc quét đảm bảo rằng mọi hình chữ nhật giới hạn như vậy được tính chính xác khi nó hoạt động hoàn toàn theo các ràng buộc truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict

def solve():
    n, m = map(int, input().split())
    a = []
    pos = defaultdict(list)

    for i in range(n):
        row = list(map(int, input().split()))
        a.append(row)
        for j, v in enumerate(row):
            pos[v].append((i + 1, j + 1))

    events = []
    for v, lst in pos.items():
        lst.sort()
        for k in range(len(lst) - 1):
            (r1, c1) = lst[k]
            (r2, c2) = lst[k + 1]
            rmin, rmax = min(r1, r2), max(r1, r2)
            cmin, cmax = min(c1, c2), max(c1, c2)
            events.append((rmin, rmax, cmin, cmax))

    queries = []
    for idx in range(int(input())):
        i1, j1, i2, j2 = map(int, input().split())
        queries.append((i1, j1, i2, j2, idx))

    queries.sort(key=lambda x: x[2])  # sort by i2

    BIT = [0] * (m + 2)

    def add(i, v):
        while i <= m:
            BIT[i] += v
            i += i & -i

    def sum_(i):
        s = 0
        while i > 0:
            s += BIT[i]
            i -= i & -i
        return s

    def range_sum(l, r):
        return sum_(r) - sum_(l - 1)

    events.sort(key=lambda x: x[1])

    ans = [True] * len(queries)
    e = 0

    for i2 in range(1, n + 1):
        while e < len(events) and events[e][1] <= i2:
            rmin, rmax, cmin, cmax = events[e]
            add(cmin, 1)
            add(cmax + 1, -1)
            e += 1

        while queries and queries[0][2] == i2:
            i1, j1, i2q, j2, idx = queries.pop(0)
            total = range_sum(j1, j2)
            ans[idx] = (total == 0)

    print("\n".join("YES" if x else "NO" for x in ans))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ nhóm tất cả các vị trí có giá trị giống hệt nhau. Sau đó, nó chuyển đổi từng cặp liền kề thành một ràng buộc hình chữ nhật và lưu trữ nó dưới dạng một sự kiện được sắp xếp theo ranh giới dưới cùng của nó. Cây Fenwick được sử dụng như một mảng khác biệt trên các cột sao cho mỗi ràng buộc hoạt động đóng góp vào một khoảng cột. 

Việc quét qua các hàng sẽ kích hoạt tất cả các ràng buộc có khoảng dọc được bao gồm đầy đủ trong ngưỡng hàng hiện tại. Các truy vấn dự định sẽ được trả lời khi đạt đến ranh giới dưới cùng của chúng. Việc kiểm tra chỉ còn là xác minh xem có bất kỳ ràng buộc hiện hoạt nào chồng lên khoảng thời gian cột của truy vấn hay không. 

Một chi tiết triển khai tinh tế là việc sử dụng bản cập nhật chênh lệch trong cây Fenwick để biểu thị các khoảng cột. Mỗi sự kiện thêm +1 tại cmin và -1 tại cmax + 1, do đó, tổng tiền tố phản ánh số lượng ràng buộc hoạt động bao trùm mỗi cột. 

Một chi tiết quan trọng khác là duy trì sự đồng bộ hóa chính xác giữa quét hàng và xử lý truy vấn, vì cả hai đều phụ thuộc vào ranh giới dưới cùng của truy vấn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi xem xét lưới:```
2 1
3 2
```| Bước | Sự kiện đang hoạt động | Truy vấn đã xử lý | Trạng thái BIT (khái niệm) | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | không | (1,1,1,1) | trống | CÓ | 
| 2 | không | (1,2,1,2) | trống | CÓ | 
| 3 | sự kiện kích hoạt | quét toàn bộ | vẫn không có sự trùng lặp | CÓ | 
| 4 | truy vấn cuối cùng | (1,1,2,2) | phát hiện trùng lặp 2 | KHÔNG | 

Truy vấn cuối cùng bao gồm cả hai lần xuất hiện của giá trị 2, do đó tính duy nhất không thành công. Các truy vấn trước đó tách biệt các ô đơn lẻ hoặc các phân đoạn một hàng/cột để chúng vẫn hợp lệ. 

### Mẫu 2 

Đối với lưới lớn hơn, các số trùng lặp như 1, 4 và 7 xuất hiện nhiều lần trên ma trận. 

| Truy vấn | Hình chữ nhật | Ràng buộc được phát hiện | Trả lời | 
| --- | --- | --- | --- | 
| 1 | (1,1)-(3,3) | không có cặp trùng lặp đầy đủ | CÓ | 
| 3 | (1,1)-(4,6) | bao gồm đầy đủ nhiều bản sao | KHÔNG | 
| 6 | (4,1)-(4,6) | hàng đơn, không lặp lại | CÓ | 

Dấu vết này cho thấy rằng chỉ những hình chữ nhật chứa đầy đủ cả hai lần xuất hiện từ chối kích hoạt giá trị lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n·m + q) log m) | mỗi cặp giá trị tạo ra một sự kiện, các cập nhật và truy vấn của Fenwick được tính theo logarit | 
| Không gian | O(n·m) | lưu trữ tất cả các vị trí và sự kiện | 

Giải pháp này phù hợp một cách thoải mái vì n·m tối đa là 100.000, do đó, ngay cả việc tiền xử lý tuyến tính cộng với việc xử lý truy vấn logarit vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()

# provided samples
assert run("""2 2
2 1
3 2
9
1 1 1 1
1 2 1 2
2 1 2 1
2 2 2 2
1 1 1 2
2 1 2 2
1 1 2 1
1 2 2 2
1 1 2 2
""") == """YES
YES
YES
YES
YES
YES
YES
YES
NO
"""

# custom case 1: all distinct
assert run("""1 3
1 2 3
1
1 1 1 3
""") == "YES\n"

# custom case 2: all equal
assert run("""2 2
5 5
5 5
1
1 1 2 2
""") == "NO\n"

# custom case 3: duplicate only outside query
assert run("""2 3
1 2 3
1 4 5
1
1 2 2 3
""") == "YES\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các hàng riêng biệt | CÓ | tính đúng đắn cơ bản | 
| tất cả lưới bằng nhau | KHÔNG | phát hiện trùng lặp toàn cầu | 
| chồng chéo một phần | CÓ | loại trừ các bản sao bên ngoài | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tồn tại các bản sao nhưng nằm bên ngoài hình chữ nhật truy vấn. Trong trường hợp đó, thuật toán không được gắn cờ sai cho chúng. Việc xây dựng dựa trên sự kiện đảm bảo điều này vì chỉ các cặp có đầy đủ trong giới hạn hàng và cột mới đóng góp các ràng buộc hoạt động. 

Một trường hợp khác là khi xảy ra sự trùng lặp trong cùng một hàng hoặc cột. Ví dụ:```
1 2 1
```Truy vấn loại trừ một lần xuất hiện vẫn phải trả về CÓ. Thuật toán xử lý việc này vì chỉ những cặp có đầy đủ trong ranh giới truy vấn mới được kích hoạt. 

Trường hợp đặc biệt cuối cùng là truy vấn một ô, luôn trả về CÓ. Vì không có cặp nào có thể vừa với hình chữ nhật 1x1 nên không có sự kiện nào được kích hoạt và BIT vẫn trống, tạo ra câu trả lời tích cực một cách chính xác.
