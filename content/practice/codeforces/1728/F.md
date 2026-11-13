---
title: "CF 1728F - Ngư Dân"
description: "Mỗi ngư dân có một kích cỡ cá ai. Chúng tôi chọn thứ tự mà họ nói. Người đánh cá đầu tiên cho biết kích cỡ con cá thật của mình. Mỗi ngư dân sau này phải nói bội số nhỏ nhất của kích cỡ cá của mình lớn hơn giá trị đã công bố trước đó."
date: "2026-06-09T18:51:52+07:00"
tags: ["codeforces", "competitive-programming", "flows", "graph-matchings", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1728
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 135 (Rated for Div. 2)"
rating: 3100
weight: 1728
solve_time_s: 116
verified: true
draft: false
---

[CF 1728F - Ngư dân](https://codeforces.com/problemset/problem/1728/F) 

**Đánh giá:** 3100 
**Tags:** dòng chảy, khớp biểu đồ, tham lam 
**Thời gian giải:** 1 phút 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi ngư dân có một kích cỡ cá`a_i`. Chúng tôi chọn thứ tự mà họ nói. 

Người đánh cá đầu tiên cho biết kích cỡ con cá thật của mình. Mỗi ngư dân sau này phải nói bội số nhỏ nhất của kích cỡ cá của mình lớn hơn giá trị đã công bố trước đó. 

Các giá trị được công bố tạo thành một dãy tăng dần. Thứ tự là thứ duy nhất chúng tôi kiểm soát và chúng tôi muốn tổng của tất cả các giá trị được công bố càng nhỏ càng tốt. 

Nhận xét hữu ích đầu tiên là quá trình nói thực tế ít quan trọng hơn vẻ ngoài của nó. 

Giả sử ngư dân`i`cuối cùng công bố một số giá trị`x_i`. Vì mỗi ngư dân chỉ có thể thông báo bội số của`a_i`, mỗi`x_i`phải chia hết cho`a_i`. Chuỗi các giá trị được công bố tăng dần nên tất cả các giá trị được công bố đều khác biệt. 

Bây giờ đảo ngược quan điểm. Nếu chúng ta chỉ định cho mỗi ngư dân một bội số riêng biệt của kích thước cá của anh ta, sau đó sắp xếp các giá trị được chỉ định đó tăng dần thì thứ tự sắp xếp đó sẽ trở thành thứ tự nói hợp lệ. Mọi ngư dân thực sự sẽ thông báo chính xác giá trị được chỉ định, bởi vì đó là bội số nhỏ nhất có sẵn lớn hơn giá trị được chỉ định trước đó. 

Vì vậy, vấn đề trở thành: 

Chọn một bội số phân biệt của`a_i`cho mỗi ngư dân, giảm thiểu tổng số tiền. 

Những hạn chế rất rõ ràng. Chúng tôi có nhiều nhất`1000`ngư dân, trong khi kích cỡ cá có thể lớn bằng`10^6`. Bất kỳ giải pháp nào tùy thuộc vào độ lớn của các giá trị đều không thể hoạt động. Thuật toán phải phụ thuộc chủ yếu vào`n`. 

Một sai lầm phổ biến là nghĩ rằng chỉ thứ tự nói mới quyết định được mọi thứ. Coi như:```
2
2 2
```Các giá trị được công bố không thể đồng thời`2`. Một ngư dân phải nói`2`, người kia phải nói`4`, vậy câu trả lời là`6`. 

Một trường hợp tế nhị khác là:```
3
2 3 6
```Một chiến lược tham lam luôn sử dụng bội số nhỏ nhất hiện có có thể chọn`2`,`3`,`6`. Điều này thực sự là tối ưu, với tổng`11`. Nhưng nếu chúng ta có:```
3
2 2 2
```chúng ta cần ba bội số riêng biệt:`2`,`4`,`6`, đưa ra câu trả lời`12`. Sự khác biệt là hạn chế thực sự. 

Một cái bẫy cuối cùng là giả sử chúng ta cần bội số lớn tùy ý. Chỉ với`n`ngư dân, mỗi ngư dân chỉ cần xem xét đầu tiên của mình`n`bội số. Nếu một ngư dân nào đó được giao nhiệm vụ của mình`(n+1)`-st bội số, sau đó trong số đầu tiên của mình`n`bội số ít nhất một sẽ không được sử dụng, bởi vì chỉ có`n-1`những ngư dân khác đang chiếm giữ các giá trị. Thay thế bội số lớn hơn bằng bội số nhỏ hơn không sử dụng sẽ cải thiện giải pháp. 

Sự quan sát đó là điều làm cho vấn đề có thể giải quyết được. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Hãy thử mọi hoán vị của ngư dân, mô phỏng các giá trị đã công bố và giữ nguyên số tiền tối thiểu. 

Đối với một hoán vị cố định, việc mô phỏng rất dễ dàng. Điều khó khăn là có`n!`hoán vị. Ngay cả đối với`n = 15`, điều này đã hoàn toàn không khả thi và giới hạn thực tế là`1000`. 

Bước quan trọng là thay đổi quan điểm từ ra lệnh cho ngư dân sang ấn định giá trị đã công bố. 

Mỗi ngư dân phải nhận được một giá trị riêng biệt gấp bội kích thước con cá của mình. Khi các giá trị đó được chọn, thứ tự nói sẽ bắt buộc: sắp xếp các giá trị đã chọn. 

Vì vậy chúng ta không còn quan tâm đến hoán vị nữa. Chúng tôi quan tâm đến việc chọn một giá trị hợp lệ cho mỗi ngư dân. 

Vì chỉ là lần đầu tiên`n`bội số của mỗi kích cỡ cá có thể quan trọng, chúng tôi tạo ra tất cả các giá trị```
a_i * 1, a_i * 2, ..., a_i * n
```và loại bỏ các bản sao. 

Hãy coi mọi giá trị được tạo ra như một nút ở phía bên trái của biểu đồ lưỡng cực và mỗi người đánh cá là một nút ở phía bên phải. 

Một giá trị`v`được kết nối với ngư dân`i`nếu như`v`chia hết cho`a_i`. 

Việc chọn các giá trị được công bố riêng biệt giờ đây chính xác là chọn một kết quả phù hợp bao gồm tất cả ngư dân. 

Câu hỏi còn lại là làm thế nào để giảm thiểu tổng các giá trị đã chọn. 

Tất cả các nút giá trị đều có chi phí bằng giá trị số của chúng. Sau khi sắp xếp các nút giá trị ngày càng nhiều, chúng tôi xử lý chúng từ nhỏ nhất đến lớn nhất. Bất cứ khi nào việc thêm một nút giá trị làm tăng kích thước khớp tối đa lên một, chúng tôi sẽ giữ nó và thêm giá trị của nó vào câu trả lời. 

Đây là cách xây dựng tham lam cổ điển của cơ sở trọng số tối thiểu trong một matroid ngang. Về mặt vận hành, nó trở thành một chuỗi các phép tăng cường Kuhn. 

Kết quả là nhỏ gọn đáng ngạc nhiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n) | O(n) | Quá chậm | 
| Kết Hợp Tối Ưu + Xây Dựng Thứ Hạng Tham Lam | O(V · E) | O(V + E) | Đã chấp nhận | 

Đây`V`là số lượng các giá trị ứng viên riêng biệt, nhiều nhất là khoảng`n²`. 

## Hướng dẫn thuật toán 

1. Tạo tất cả các giá trị`a_i * k`cho mỗi ngư dân`i`và mọi`1 ≤ k ≤ n`. 
2. Sắp xếp các giá trị này và loại bỏ các giá trị trùng lặp. 
3. Tạo biểu đồ lưỡng cực. 

Phía bên trái chứa tất cả các giá trị ứng cử viên riêng biệt. 

Phía bên phải chứa các ngư dân. 
4. Đối với mỗi ngư dân`i`và mọi số nhân`k`từ`1`ĐẾN`n`, tìm chỉ số của giá trị`a_i * k`trong danh sách giá trị được sắp xếp và thêm một cạnh từ nút giá trị đó vào ngư dân`i`. 

Nút giá trị được kết nối chính xác với ngư dân có kích cỡ cá chia cho giá trị đó. 
5. Xử lý các nút giá trị từ nhỏ nhất đến lớn nhất. 
6. Duy trì sự phù hợp tối đa giữa các nút giá trị được xử lý và ngư dân. 
7. Khi xem xét một nút giá trị mới, hãy chạy một phép tăng Kuhn bắt đầu từ nút giá trị đó. 
8. Nếu tăng thành công, kích thước khớp tối đa sẽ tăng thêm một. Thêm giá trị số của nút này vào câu trả lời. 
9. Tiếp tục cho đến khi tất cả các nút giá trị được xử lý. 
10. Xuất số tiền tích lũy. 

### Tại sao nó hoạt động 

Các giá trị ứng viên tạo thành tập nền tảng của matroid ngang. Một tập hợp các nút giá trị sẽ độc lập nếu ngư dân có thể sử dụng chúng chấp nhận sự trùng khớp. 

Đối với matroid, cơ sở trọng lượng tối thiểu đạt được bằng cách xử lý các phần tử theo thứ tự trọng số tăng dần và lấy phần tử bất cứ khi nào nó tăng thứ hạng. 

Trong biểu đồ của chúng tôi, thứ hạng của một tập hợp các nút giá trị chính xác là kích thước của mức khớp tối đa có thể đạt được bằng cách sử dụng các giá trị đó. 

Khi xử lý các giá trị từ nhỏ nhất đến lớn nhất, việc tăng thành công có nghĩa là giá trị hiện tại tăng thứ hạng và phải thuộc về cơ sở trọng số tối thiểu tham lam. Việc nâng cấp không thành công có nghĩa là nó không tăng thứ hạng và không bao giờ có thể giúp tạo ra cơ sở rẻ hơn. 

Cơ sở cuối cùng có kích thước`n`, nghĩa là mỗi ngư dân được so khớp với một bội số hợp lệ riêng biệt. Tổng được tích lũy bởi quá trình tham lam chính xác là tổng tối thiểu có thể có của các giá trị được công bố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    vals = []
    for x in a:
        for k in range(1, n + 1):
            vals.append(x * k)

    vals = sorted(set(vals))
    m = len(vals)

    g = [[] for _ in range(m)]

    for i, x in enumerate(a):
        for k in range(1, n + 1):
            v = x * k
            idx = vals.index(v) if False else None

    pos = {v: i for i, v in enumerate(vals)}

    for i, x in enumerate(a):
        for k in range(1, n + 1):
            g[pos[x * k]].append(i)

    mt = [-1] * n
    used = [False] * m

    sys.setrecursionlimit(1 << 20)

    def kuhn(v):
        if used[v]:
            return False
        used[v] = True

        for to in g[v]:
            if mt[to] == -1 or kuhn(mt[to]):
                mt[to] = v
                return True

        return False

    ans = 0

    for v in range(m):
        used = [False] * m
        if kuhn(v):
            ans += vals[v]

    print(ans)

if __name__ == "__main__":
    solve()
```Khối đầu tiên tạo ra mọi giá trị ứng cử viên có liên quan. Chỉ có cái đầu tiên`n`bội số là cần thiết, điều này giữ cho đồ thị hữu hạn. 

Từ điển`pos`chuyển đổi một giá trị số thực tế thành vị trí của nó trong danh sách ứng cử viên được sắp xếp. Nếu không có bản đồ này, việc tìm kiếm liên tục bằng tìm kiếm nhị phân hoặc tìm kiếm tuyến tính sẽ tăng thêm chi phí không cần thiết. 

Biểu đồ được xây dựng từ các nút giá trị cho ngư dân. Một cạnh có nghĩa là ngư dân có thể sử dụng giá trị đó làm số đã công bố của mình.`mt[i]`lưu trữ nút giá trị hiện khớp với ngư dân`i`. 

Tìm kiếm Kuhn cố gắng chèn nút giá trị mới được xử lý vào kết quả khớp. Nếu tồn tại một đường dẫn tăng cường thì kích thước phù hợp sẽ tăng thêm một. 

Một chi tiết tinh tế đang được đặt lại`used`trước mỗi lần thử tăng cường. Mỗi DFS phải có trạng thái truy cập riêng. Việc sử dụng lại các nhãn hiệu cũ sẽ chặn các đường dẫn tăng cường hợp lệ một cách không chính xác. 

Câu trả lời được tăng lên chính xác khi kích thước phù hợp tăng lên. Đó chính xác là các nút giá trị được lựa chọn bởi quá trình xây dựng thứ hạng tham lam. 

Số nguyên Python dễ dàng xử lý câu trả lời lớn nhất có thể, vì giá trị ứng cử viên có thể đạt tới khoảng`10^9`và chúng ta có thể tổng hợp lại`1000`của họ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
1 8 2 3 2 2 3
```Các giá trị ứng viên có liên quan bắt đầu như sau:```
1, 2, 3, 4, 6, 8, 9, ...
```Quá trình kết hợp tham lam hoạt động như sau. 

| Giá trị | Kích thước phù hợp trước | Kích thước phù hợp sau | Đã thêm vào câu trả lời | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | 
| 2 | 1 | 2 | 2 | 
| 3 | 2 | 3 | 3 | 
| 4 | 3 | 4 | 4 | 
| 6 | 4 | 5 | 6 | 
| 8 | 5 | 6 | 8 | 
| 9 | 6 | 7 | 9 | 

Các giá trị được chọn là:```
1, 2, 3, 4, 6, 8, 9
```Tổng của chúng là:```
33
```Ví dụ này cho thấy cách thuật toán chỉ giữ lại các nút giá trị làm tăng kích thước khớp có thể đạt được. 

### Ví dụ 2 

đầu vào:```
10
5 6 5 6 5 6 5 6 5 6
```Các giá trị hữu ích đầu tiên là:```
5, 6, 10, 12, 15, 18, 20, 24, 25, 30
```| Giá trị | Kích thước phù hợp trước | Kích thước phù hợp sau | Đã thêm vào câu trả lời | 
| --- | --- | --- | --- | 
| 5 | 0 | 1 | 5 | 
| 6 | 1 | 2 | 6 | 
| 10 | 2 | 3 | 10 | 
| 12 | 3 | 4 | 12 | 
| 15 | 4 | 5 | 15 | 
| 18 | 5 | 6 | 18 | 
| 20 | 6 | 7 | 20 | 
| 24 | 7 | 8 | 24 | 
| 25 | 8 | 9 | 25 | 
| 30 | 9 | 10 | 30 | 

Tổng số tiền là:```
165
```Điều này chứng tỏ rằng kích thước cá lặp đi lặp lại đương nhiên đòi hỏi bội số ngày càng lớn hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(VE) | Tăng một Kuhn cho mỗi nút giá trị | 
| Không gian | O(V + E) | Đồ thị, khớp, mảng truy cập | 

Đây`V`là số lượng các giá trị ứng viên riêng biệt và`E`là số cạnh của đồ thị. Vì mỗi ngư dân đóng góp nhiều nhất`n`bội số ứng viên, cả hai đại lượng đều bị giới hạn bởi`O(n²)`. Với`n = 1000`, điều này phù hợp thoải mái với giới hạn của cuộc thi và phù hợp với giải pháp dự định. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    vals = []
    for x in a:
        for k in range(1, n + 1):
            vals.append(x * k)

    vals = sorted(set(vals))
    pos = {v: i for i, v in enumerate(vals)}

    m = len(vals)
    g = [[] for _ in range(m)]

    for i, x in enumerate(a):
        for k in range(1, n + 1):
            g[pos[x * k]].append(i)

    mt = [-1] * n
    sys.setrecursionlimit(1 << 20)

    def kuhn(v, used):
        if used[v]:
            return False
        used[v] = True

        for to in g[v]:
            if mt[to] == -1 or kuhn(mt[to], used):
                mt[to] = v
                return True

        return False

    ans = 0

    for v in range(m):
        used = [False] * m
        if kuhn(v, used):
            ans += vals[v]

    return str(ans)

# provided samples
assert run("7\n1 8 2 3 2 2 3\n") == "33", "sample 1"
assert run("10\n5 6 5 6 5 6 5 6 5 6\n") == "165", "sample 2"

# custom cases
assert run("1\n7\n") == "7", "single fisherman"
assert run("2\n2 2\n") == "6", "duplicate values"
assert run("3\n1 1 1\n") == "6", "first three multiples"
assert run("3\n2 3 6\n") == "11", "distinct compatible values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7`|`7`| Ví dụ kích thước tối thiểu | 
|`2 / 2 2`|`6`| Sự khác biệt của các giá trị được gán | 
|`3 / 1 1 1`|`6`| Bắt buộc phải có bội số liên tiếp | 
|`3 / 2 3 6`|`11`| Mối quan hệ chia hết hỗn hợp | 

## Vỏ cạnh 

Hãy xem xét:```
2
2 2
```Biểu đồ chứa các nút giá trị`2`Và`4`. Cả hai ngư dân đều có thể sử dụng một trong hai giá trị. Lần tăng cường thành công đầu tiên cần`2`. Lần tăng cường thành công thứ hai cần`4`. Câu trả lời trở thành`6`. 

Một giải pháp bất cẩn chỉ xem xét bội số nhỏ nhất của mỗi ngư dân sẽ tạo ra kết quả không chính xác.`4`. 

Bây giờ hãy xem xét:```
3
1 1 1
```Các bài tập riêng biệt hợp lệ duy nhất là`1`,`2`, Và`3`. Sự kết hợp tăng lên chính xác ở ba giá trị đó, tạo ra câu trả lời`6`. 

Điều này xác minh rằng kích thước cá lặp đi lặp lại buộc phải sử dụng bội số lớn hơn. 

Cuối cùng:```
3
2 3 6
```Giá trị ứng viên bắt đầu bằng`2, 3, 4, 6, ...`. 

Sự phù hợp tăng lên ở`2`, sau đó`3`, sau đó`6`. Giá trị`4`không tăng kích thước phù hợp và bị bỏ qua. Câu trả lời là`11`. 

Điều này minh họa tính bất biến trung tâm của thuật toán: một giá trị chỉ đóng góp vào câu trả lời nếu nó làm tăng thứ hạng của kết quả khớp hai bên hiện tại.
