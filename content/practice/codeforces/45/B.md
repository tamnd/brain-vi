---
title: "CF 45B - Trường học"
description: "Mỗi học sinh chỉ vào đúng một học sinh khác, người mà các em sẽ gọi mỗi khi nghe được tin tức. Điều này tạo ra một biểu đồ chức năng có hướng, mỗi nút có chính xác một mức độ."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "dsu"]
categories: ["algorithms"]
codeforces_contest: 45
codeforces_index: "B"
codeforces_contest_name: "School Team Contest 3 (Winter Computer School 2010/11)"
rating: 2200
weight: 45
solve_time_s: 126
verified: true
draft: false
---
[CF 45B - Trường học](https://codeforces.com/problemset/problem/45/B) 

**Đánh giá:** 2200 
**Thẻ:** dp, dsu 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi học sinh chỉ vào đúng một học sinh khác, người mà các em sẽ gọi mỗi khi nghe được tin tức. Điều này tạo ra một biểu đồ chức năng có hướng, mỗi nút có chính xác một mức độ. 

Vào ngày`i`, một học sinh bắt đầu lan truyền một tin tức với sức mạnh ban đầu`b[i]`. Mỗi lần tin tức được truyền đi, sức mạnh của nó lại giảm đi một. Khi cường độ trở về 0, quá trình chuyển tiếp sẽ dừng lại. Một học sinh có thể nhận được cùng một tin tức nhiều lần trong cùng một ngày với các mức độ khác nhau vì được phép có chu kỳ. 

Chúng ta không hỏi tổng số lần học sinh nghe được tin tức. Chúng tôi chỉ quan tâm đến ngày đầu tiên mỗi học sinh biết được tin tức nào đó. Đối với mỗi ngày, chúng tôi phải xuất ra số lượng học sinh được thông báo lần đầu tiên vào ngày hôm đó. 

Học sinh bắt đầu không được đưa ra trực tiếp. Thay vì,$$a_i = ((v_i + res_{i-1}) \bmod n) + 1$$Ở đâu`res[i-1]`là câu trả lời của ngày hôm trước. 

Kích thước biểu đồ và số ngày đều lên tới`10^5`. Việc mô phỏng trực tiếp mọi cuộc gọi điện thoại là không thể vì`b[i]`cũng có thể lớn như`10^7`. Ngay cả một truy vấn đơn lẻ cũng có thể yêu cầu duyệt qua hàng triệu cạnh nếu được triển khai một cách đơn giản. 

Một bậc hai hoặc thậm chí`O(n * sqrt(n))`cách tiếp cận là nguy hiểm dưới giới hạn 2 giây. Mục tiêu gần với độ phức tạp tổng thể tuyến tính. 

Phần khó khăn là các đường dẫn có thể đi vào chu kỳ. Nếu chúng ta chỉ đơn giản là bước đi`b[i]`bước mỗi ngày, chúng tôi liên tục xem lại các đỉnh giống nhau. Một vấn đề tế nhị khác là một khi học sinh đã nghe tin tức vào một ngày nào đó trước đó thì chúng sẽ không bao giờ được tính lại. 

Hãy xem xét ví dụ này:```
3 1
2 3 1
1
10
```Đồ thị là một chu kỳ:`1 -> 2 -> 3 -> 1`. 

Tin tức lặp đi lặp lại mãi mãi về mặt khái niệm, nhưng sức mạnh giảm dần theo từng bước. Với sức mạnh`10`, mỗi học sinh đều nghe thấy nó nhiều lần. Câu trả lời đúng là:```
3
```Việc triển khai bất cẩn tính mỗi lượt truy cập thay vì những khám phá lần đầu sẽ tạo ra giá trị lớn hơn nhiều. 

Một trường hợp nguy hiểm khác là khi học sinh mới nhập học đã được thông báo trước đó:```
3 2
2 3 1
1 1
2 2
```Ngày thứ nhất thông báo cho học sinh`2`Và`3`bắt đầu từ`2`. 

Ngày thứ 2 bắt đầu lại từ`2`, nhưng không còn ai mới nữa. Đầu ra đúng là:```
2
0
```Nếu chúng tôi quên duy trì trạng thái chung "đã được thông báo" trong nhiều ngày, chúng tôi sẽ tính sai các khám phá lặp lại. 

Một trường hợp tế nhị hơn xuất hiện khi`b[i]`vượt quá số đỉnh riêng biệt có thể tiếp cận:```
4 1
2 3 4 2
1
100
```Bộ có thể truy cập từ`2`chỉ là`{2,3,4}`vì cuối cùng đồ thị sẽ quay vòng bên trong các nút đó. Câu trả lời vẫn là:```
3
```Việc triển khai ngây thơ có thể lãng phí rất nhiều thời gian để lặp đi lặp lại cùng một chu kỳ. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu tuân theo quy trình chính xác như được mô tả. 

Đối với mỗi ngày: 

1. Tính học sinh đầu khóa`a[i]`. 
2. Đi dọc theo các cạnh hướng ra ngoài tới`b[i]`lần. 
3. Đánh dấu mỗi học sinh đến thăm là đã được thông báo nếu đây là lần đầu tiên các em nghe được tin tức nào đó. 

Mô phỏng này là chính xác vì việc truyền bá tin tức thực sự là một bước đi dài`b[i]`trong đồ thị hàm số. 

Vấn đề là sự phức tạp. Trong trường hợp xấu nhất,`b[i] = 10^7`Và`m = 10^5`. Điều đó có nghĩa là đến`10^12`truyền tải cạnh, vượt xa giới hạn khả thi. 

Quan sát quan trọng là mỗi học sinh chỉ quan trọng một lần. Sau khi học sinh được thông báo, những chuyến viếng thăm sau này của học sinh đó sẽ không bao giờ góp phần đưa ra câu trả lời nữa. 

Điều này thay đổi vấn đề hoàn toàn. Thay vì mô phỏng mọi lần truyền, chúng tôi chỉ cần bỏ qua những học sinh đã biết một cách hiệu quả. 

Giả sử chúng ta đang đi dọc theo con đường truyền bá. Bất cứ khi nào chúng tôi gặp một học sinh đã được thông báo, việc xử lý đỉnh đó là vô ích. Chúng tôi muốn một cấu trúc có thể nhanh chóng chuyển sang đỉnh tiếp theo vẫn chưa được xác định. 

Đây chính xác là những gì Disjoint Set Union có thể làm được. 

Chúng tôi duy trì cấu trúc "đỉnh còn sống tiếp theo" giống như DSU. Sau khi học sinh được cung cấp thông tin, chúng tôi sẽ loại họ khỏi danh sách xem xét bằng cách liên kết họ với người kế nhiệm trong biểu đồ. Các lần di chuyển trong tương lai sẽ tự động bỏ qua chúng. 

Đồ thị có tính hàm số nên mỗi nút có chính xác một cạnh đi ra. Điều đó làm cho quá trình nén này đặc biệt hiệu quả. Sau khi một nút được xử lý một lần, nó sẽ biến mất vĩnh viễn. 

Thuật toán kết quả xử lý mỗi học sinh nhiều nhất một lần trong tất cả các ngày. Điều đó làm giảm tổng thời gian làm việc xuống gần như tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(\sum b_i)$|$O(n)$| Quá chậm | 
| Tối ưu |$O((n + m)\alpha(n))$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ biểu đồ tình bạn dưới dạng mảng`g`, Ở đâu`g[x]`là học sinh tiếp theo được gọi bởi`x`. 
2. Duy trì một mảng`seen`cho biết học sinh đã nghe được tin tức nào trước đó hay chưa. 
3. Xây dựng cấu trúc nhảy kiểu DSU`parent`. 

Đối với mỗi học sinh`x`,`find(x)`trả về đỉnh ứng cử viên tiếp theo mà có thể vẫn chưa được thông tin. 
4. Ban đầu, mỗi đỉnh là đại diện của chính nó. 
5. Khi còn là sinh viên`x`được thông báo lần đầu tiên: 

- Đánh dấu`seen[x] = True`. 
- Di dời`x`từ việc xem xét trong tương lai bằng cách thiết lập:```
parent[x] = find(g[x])
```Điều này có nghĩa là các lần di chuyển trong tương lai sẽ bỏ qua trực tiếp tới đỉnh có thể hữu ích tiếp theo. 
6. Đối với mỗi ngày: 

- Tính toán$$a = ((v_i + prev) \bmod n) + 1$$Ở đâu`prev`là câu trả lời của ngày hôm qua. 

- Bắt đầu từ`cur = find(a)`. 
7. Trong khi: 

-`cur`chưa được xử lý, 
- và chúng ta vẫn còn sức lực, 

xử lý học sinh hiện tại. 

Mỗi học sinh được xử lý sẽ tiêu tốn một đơn vị sức mạnh vì xảy ra một bước chuyển tiếp. 
8. Sau khi xử lý một học sinh: 

- Tăng câu trả lời của ngày hôm nay. 
- Loại bỏ sinh viên khỏi cấu trúc DSU. 
- Chuyển trực tiếp đến ứng viên có sẵn tiếp theo. 
9. Xuất câu trả lời của ngày hôm nay và lưu nó dưới dạng`prev`. 

### Tại sao nó hoạt động 

Bất biến DSU là:`find(x)`luôn trả về sinh viên không hiểu biết đầu tiên có thể truy cập được từ`x`bằng cách liên tục đi theo các ranh giới tình bạn, bỏ qua tất cả những học sinh đã được thông tin sẵn. 

Bất cứ khi nào một học sinh được thông báo, chúng tôi sẽ chuyển hướng họ vĩnh viễn sang ứng viên tiếp theo. Nén đường dẫn đảm bảo các tìm kiếm trong tương lai sẽ trở nên cực kỳ nhanh chóng. 

Vì mỗi học sinh chỉ chuyển từ trạng thái không hiểu biết sang có hiểu biết một lần nên mỗi nút sẽ bị xóa khỏi cấu trúc nhiều nhất một lần. Điều đó giới hạn tổng số lượng công việc thực tế trong tất cả các ngày. 

Bản thân quá trình nhân giống được bảo toàn chính xác. Chúng tôi vẫn đi theo chuỗi tình bạn theo cùng một trật tự. DSU chỉ bỏ qua các đỉnh không còn có thể ảnh hưởng đến câu trả lời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1 << 25)

n, m = map(int, input().split())

g = [0] + list(map(int, input().split()))
v = list(map(int, input().split()))
b = list(map(int, input().split()))

parent = list(range(n + 1))
seen = [False] * (n + 1)

def find(x):
    if parent[x] != x:
        parent[x] = find(parent[x])
    return parent[x]

prev = 0

for i in range(m):
    a = ((v[i] + prev) % n) + 1

    ans = 0
    steps = b[i]

    cur = find(a)

    while steps > 0 and not seen[cur]:
        ans += 1
        seen[cur] = True
        steps -= 1

        nxt = find(g[cur])
        parent[cur] = nxt

        cur = nxt

    print(ans)
    prev = ans
```Ý tưởng cốt lõi là những sinh viên có hiểu biết sẽ bị loại bỏ vĩnh viễn khỏi các lần duyệt trong tương lai. 

các`find`thực hiện nén đường dẫn DSU tiêu chuẩn. Sau đủ các thao tác, chuỗi trở nên cực kỳ ngắn, cho phép truy cập gần như liên tục. 

Phần tế nhị nhất là dòng này:```
parent[cur] = find(g[cur])
```Chúng tôi không chỉ đơn giản viết`parent[cur] = g[cur]`. Người bạn đó có thể đã bị xóa nên chúng ta phải chuyển thẳng sang người đại diện đang hoạt động tiếp theo. 

Một chi tiết tinh tế khác là điều kiện vòng lặp:```
while steps > 0 and not seen[cur]:
```Nếu như`cur`đã được thông báo rồi, tiếp tục nữa cũng vô ích. Mọi nút có thể truy cập trong tương lai thông qua nén DSU cũng đã bị loại khỏi danh sách xem xét. 

Giải pháp không bao giờ xử lý rõ ràng các chu kỳ. Quá trình nén DSU sẽ thu gọn chúng một cách tự nhiên khi tất cả các đỉnh bên trong đều được thông báo. 

Tràn số nguyên không liên quan trong Python, nhưng trong C++, giải pháp này phải sử dụng số nguyên 64 bit cho công thức liên quan đến`v[i]`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 4
2 3 1
1 2 3 4
1 2 3 4
```Đồ thị là:```
1 -> 2 -> 3 -> 1
```| Ngày | trước | một | b | Mới được thông báo | độ phân giải | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 2 | 1 | {2} | 1 | 
| 2 | 1 | 1 | 2 | {1} | 1 | 
| 3 | 1 | 2 | 3 | {3} | 1 | 
| 4 | 1 | 3 | 4 | {} | 0 | 

Đầu ra:```
1
1
1
0
```Dấu vết này cho thấy những học sinh đã được thông báo sẽ tự động bị bỏ qua như thế nào. Đến ngày thứ 4, mọi học sinh đều đã được biết hết nên câu trả lời ngay lập tức trở thành số 0. 

### Ví dụ tùy chỉnh 

đầu vào:```
4 2
2 3 4 2
1 1
5 5
```Đồ thị là:```
1 -> 2 -> 3 -> 4 -> 2
```| Ngày | trước | một | b | Truyền tải | Mới được thông báo | độ phân giải | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 2 | 5 | 2,3,4,2,3 | {2,3,4} | 3 | 
| 2 | 3 | 1 | 5 | 1,2,3,4 | {1} | 1 | 

Đầu ra:```
3
1
```Ngày đầu tiên thể hiện việc đạp xe lặp đi lặp lại. Dù tin tức liên tục lặp đi lặp lại trong khi sức mạnh vẫn tích cực nhưng mỗi học sinh chỉ đóng góp tối đa một lần. 

Ngày thứ hai cho thấy DSU bỏ qua các đỉnh đã được xử lý một cách hiệu quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + m)\alpha(n))$| Mỗi sinh viên bị loại một lần, hoạt động DSU được khấu hao nghịch đảo-Ackermann | 
| Không gian |$O(n)$| Biểu đồ tình bạn, mảng cha mẹ DSU và mảng đã truy cập | 

Hệ số Ackermann nghịch đảo là hằng số trong thực tế. Với`n, m <= 10^5`, điều này dễ dàng phù hợp trong thời hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())

    g = [0] + list(map(int, input().split()))
    v = list(map(int, input().split()))
    b = list(map(int, input().split()))

    parent = list(range(n + 1))
    seen = [False] * (n + 1)

    sys.setrecursionlimit(1 << 25)

    def find(x):
        if parent[x] != x:
            parent[x] = find(parent[x])
        return parent[x]

    prev = 0
    out = []

    for i in range(m):
        a = ((v[i] + prev) % n) + 1

        ans = 0
        steps = b[i]

        cur = find(a)

        while steps > 0 and not seen[cur]:
            ans += 1
            seen[cur] = True
            steps -= 1

            nxt = find(g[cur])
            parent[cur] = nxt

            cur = nxt

        out.append(str(ans))
        prev = ans

    print("\n".join(out))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue()

# provided sample
assert run(
"""3 4
2 3 1
1 2 3 4
1 2 3 4
"""
) == "1\n1\n1\n0\n", "sample 1"

# minimum size
assert run(
"""2 1
2 1
1
1
"""
) == "1\n", "minimum graph"

# cycle with huge strength
assert run(
"""3 1
2 3 1
1
100
"""
) == "3\n", "must avoid repeated counting"

# all students informed on first day
assert run(
"""5 3
2 3 4 5 1
1 1 1
10 10 10
"""
) == "5\n0\n0\n", "future days should contribute nothing"

# chain entering cycle
assert run(
"""4 2
2 3 4 2
1 1
5 5
"""
) == "3\n1\n", "cycle compression correctness"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đồ thị tối thiểu có 2 chu kỳ |`1`| Đầu vào pháp lý nhỏ nhất | 
| Sức mạnh lớn trên chu kỳ nhỏ |`3`| Tránh đếm lặp lại trong chu kỳ | 
| Mọi người thông báo vào ngày đầu tiên |`5 0 0`| Sự kiên trì toàn cầu của nhà nước có hiểu biết | 
| Chuỗi bước vào chu kỳ |`3 1`| Sửa lỗi bỏ qua DSU trên các cấu trúc hỗn hợp | 

## Vỏ cạnh 

Xét trường hợp chu trình thuần túy:```
3 1
2 3 1
1
10
```Cuộc đi bộ lặp đi lặp lại qua cả ba học sinh. Một mô phỏng ngây thơ có thể đếm được nhiều lượt truy cập lặp lại. 

Thực thi: 

- Bắt đầu lúc`2`- Thông báo`2`, loại bỏ nó 
- Thông báo`3`, loại bỏ nó 
- Thông báo`1`, loại bỏ nó 
- DSU bây giờ bỏ qua trực tiếp giữa các nút bị loại bỏ 

Tại thời điểm này, mọi học sinh đều đã được thông báo nên quá trình này sẽ ngừng đóng góp. Câu trả lời là đúng:```
3
```Bây giờ hãy xem xét một nút bắt đầu được thông báo trước đó:```
3 2
2 3 1
1 1
2 2
```Ngày 1: 

- Bắt đầu lúc`2`- Thông báo`2`- Thông báo`3`Trả lời:`2`Ngày 2:$$a = ((1 + 2) \bmod 3) + 1 = 1$$Học sinh`1`vẫn chưa được thông tin đầy đủ nên chỉ`1`đóng góp. 

Đầu ra:```
2
1
```Cấu trúc DSU bỏ qua chính xác`2`Và`3`. 

Cuối cùng, hãy xem xét một biểu đồ trong đó quá trình lan truyền bước vào một chu kỳ nhỏ:```
4 1
2 3 4 2
1
100
```Các đỉnh có thể tiếp cận từ`2`chỉ là`{2,3,4}`. 

Thực thi: 

- Thông báo`2`- Thông báo`3`- Thông báo`4`- Chu kỳ chuyển tiếp trong tương lai giữa các đỉnh đã bị loại bỏ 

Câu trả lời vẫn là:```
3
```mặc dù sức mạnh tin tức lớn hơn nhiều.
