---
title: "CF 102443L - Du hành thời gian"
description: "Có n thành phố và t cấu hình con đường lịch sử. Cấu hình tôi mô tả chính xác những con đường hai chiều tồn tại tại thời điểm lịch sử đó. Trong cuộc hành trình, cỗ máy thời gian gửi cho chúng ta một chuỗi cố định gồm k cấu hình a1, a2, ..., ak."
date: "2026-08-08T13:17:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102443
codeforces_index: "L"
codeforces_contest_name: "2019-2020 Russia Team Open, High School Programming Contest (VKOSHP 19)"
rating: 0
weight: 102443
solve_time_s: 106
verified: true
draft: false
---

[CF 102443L - Du hành thời gian](https://codeforces.com/problemset/problem/102443/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

có`n`thành phố và`t`cấu hình đường lịch sử. Cấu hình`i`mô tả chính xác những con đường hai chiều tồn tại vào thời điểm lịch sử đó. Trong cuộc hành trình, cỗ máy thời gian gửi chúng ta qua một chuỗi cố định`k`cấu hình,`a_1, a_2, ..., a_k`. Sau mỗi lần nhảy, chúng ta có thể ở lại thành phố hiện tại hoặc đi qua chính xác một con đường tồn tại trong cấu hình đó. 

Chúng tôi bắt đầu ở thành phố`1`trước lần nhảy đầu tiên. Nếu chúng ta sử dụng đường trong thời gian`i`-lần nhảy thứ , chúng ta đến điểm cuối khác của nó tại vị trí thời gian`i`. Mục tiêu là đến được thành phố`n`ở vị trí nhỏ nhất có thể`i`. Lần nhảy đầu tiên được tính nên việc đến được thành phố`n`trong khi sử dụng đường ở`a_1`đưa ra câu trả lời`1`. Nếu thành phố`n`không bao giờ có thể đạt được, câu trả lời là`-1`. 

Các giới hạn liên quan là`n, t, k <= 2 * 10^5`, trong khi tổng số đường trên tất cả các cấu hình lịch sử nhiều nhất là`2 * 10^5`. Điều này loại trừ bất cứ điều gì liên tục quét tất cả các con đường cho mọi vị trí thời gian. Một cấu hình duy nhất có thể chứa`2 * 10^5`đường và có thể xuất hiện ở tất cả`2 * 10^5`các vị trí đã tạo ra`4 * 10^10`kiểm tra cạnh. Một thuật toán xung quanh`O((n + m + k) log(n + k))`, Ở đâu`m`là tổng số đường là phù hợp. 

Có hai chi tiết khó xác định về thời gian thường gây ra các giải pháp sai. Đầu tiên, một cạnh không thể được sử dụng vào cùng thời điểm chúng ta đến thành phố xuất phát của nó, bởi vì chỉ có thể đi qua một con đường sau mỗi lần nhảy. Ví dụ:```
3 1
2
1 2
2 3
1
1
```Câu trả lời là`-1`. Cả hai con đường đều tồn tại vào thời điểm`1`, nhưng chúng ta chỉ có thể đi qua một trong số chúng tại thời điểm đó. Giải pháp tìm kiếm sự xuất hiện lớn hơn hoặc bằng thời gian hiện tại sẽ thu được kết quả không chính xác`1`cho toàn bộ con đường. 

Thứ hai, vị trí lần đầu tiên phải được coi là có thể sử dụng được ngay cả khi chúng tôi bắt đầu trước bất kỳ thời gian nào được liệt kê. Ví dụ:```
2 1
1
1 2
1
1
```Câu trả lời là`1`. Chúng tôi bắt đầu tại thành phố`1`với thời gian hiệu lực là`0`, do đó sự xuất hiện ở vị trí`1`là lần đầu tiên con đường có thể sử dụng được. 

Trường hợp cạnh thứ ba là sự xuất hiện lặp đi lặp lại của cùng một cấu hình có thể được phân tách bằng các cấu hình khác. Ví dụ:```
3 1
1
1 3
3
1 1 1
```Câu trả lời là`1`, vì lần xuất hiện đầu tiên là đủ rồi. Tổng quát hơn, nếu một thành phố có thể tiếp cận được ở vị trí`2`, không thể sử dụng lại đường có cùng cấu hình tại vị trí`2`, nhưng nó có thể được sử dụng ở lần xuất hiện tiếp theo. Thuật toán phải duy trì trật tự nghiêm ngặt này. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp duy trì tập hợp các thành phố có thể truy cập được sau mỗi lần nhảy. Đối với mỗi lần xuất hiện cấu hình`a_i`, chúng tôi kiểm tra các con đường của nó và đánh dấu điểm cuối của bất kỳ con đường nào mà điểm cuối khác hiện có thể đến được. Điều này đúng vì một con đường có thể được sử dụng tại thời điểm đó, do đó, chính xác các đỉnh ở khoảng cách biểu đồ nhiều nhất là một đường so với tập hợp có thể tiếp cận hiện tại đều có thể tiếp cận được. 

Vấn đề là cùng một cấu hình có thể xảy ra nhiều lần. Giả sử một cấu hình lịch sử chứa tất cả`2 * 10^5`những con đường được phép và trình tự chứa toàn bộ cấu hình đó`2 * 10^5`các vị trí thời gian. Việc quét bộ cạnh hoàn chỉnh của nó ở mọi vị trí có chi phí lên tới`4 * 10^10`kiểm tra cạnh. Các ràng buộc được thiết kế để làm cho phương pháp này không thể thực hiện được. 

Quan sát hữu ích là mọi con đường đều thuộc về một cấu hình lịch sử và toàn bộ chuỗi các khoảnh khắc đều được biết trước. Đối với mỗi cấu hình, chúng ta có thể lưu trữ các vị trí mà nó xuất hiện. Bây giờ hãy xem xét một con đường cố định`(u, v)`thuộc cấu hình`g`. 

Giả sử thời gian sớm nhất mà chúng ta có thể ở thành phố`u`là`d`. Chúng tôi không thể sử dụng con đường này ở vị trí`d`, bởi vì đạt tới`u`ở vị trí`d`đã sử dụng chuyển động của con đường duy nhất có sẵn tại vị trí đó. Chúng tôi cần vị trí đầu tiên lớn hơn`d`ở cấu hình nào`g`xuất hiện. Nếu vị trí đó là`w`, thì chúng ta có thể di chuyển từ`u`ĐẾN`v`vào đúng thời điểm`w`. 

Điều này biến mọi con đường thông thường thành một sự chuyển tiếp phụ thuộc vào thời gian. Với thời gian đến sớm nhất hiện tại tại một điểm cuối, tìm kiếm nhị phân sẽ tìm thấy sự xuất hiện hợp pháp sớm nhất của cấu hình đường. Khi chúng ta có những chuyển đổi như vậy, bài toán có cấu trúc tương tự như bài toán đường đi ngắn nhất: việc đến một thành phố sớm hơn không bao giờ có thể làm cho quá trình chuyển đổi sau trở nên tồi tệ hơn, vì vậy thuật toán Dijkstra được áp dụng. 

Phương pháp vũ lực có hiệu quả vì nó mô phỏng rõ ràng mọi thời điểm lịch sử. Nó thất bại vì những con đường giống nhau có thể được kiểm tra đi kiểm tra lại. Thay vào đó, quan sát cho thấy rằng mỗi cạnh có thể được truy vấn về lần xuất hiện có thể sử dụng tiếp theo của nó làm giảm mô phỏng lặp lại thành một tìm kiếm nhị phân cho mỗi lần thư giãn cạnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(km)`trong trường hợp xấu nhất |`O(n + m)`| Quá chậm | 
| Tối ưu |`O(m log k + m log n + k)`|`O(n + m + k)`| Đã chấp nhận | 

Đây`m`biểu thị tổng số đường trên tất cả các cấu hình lịch sử. 

## Hướng dẫn thuật toán 

1. Đọc từng con đường và lưu trữ nó trong một đồ thị vô hướng thông thường. Cùng với mỗi cạnh, hãy lưu trữ mã định danh của cấu hình lịch sử nơi con đường đó tồn tại. Chúng ta không cần giữ các biểu đồ riêng biệt vì mã định danh cấu hình đủ để xác định khi nào cạnh có thể được sử dụng. 
2. Đọc trình tự của`k`cấu hình lịch sử. Đối với mọi cấu hình`g`, lưu trữ một danh sách được sắp xếp chứa tất cả các vị trí`i`vì cái gì`a_i = g`. Bản thân chuỗi đầu vào đã được sắp xếp sẵn nên các danh sách này được sắp xếp một cách tự nhiên. 
3. Xác định`dist[v]`là vị trí thời gian sớm nhất tại thành phố nào`v`có thể đạt được. Ban đầu`dist[1] = 0`, vì chúng tôi ở thành phố`1`trước lần nhảy đầu tiên. Mọi khoảng cách khác đều là vô cùng. 
4. Đặt thành phố`1`thành một đống tối thiểu và chạy thuật toán Dijkstra. Khi thành phố`u`được xóa khỏi heap với thời điểm tốt nhất hiện tại`dist[u]`, kiểm tra mọi con đường`(u, v)`sự cố xảy ra với nó. 
5. Giả sử đường`(u, v)`thuộc về cấu hình`g`. Nhìn vào danh sách được sắp xếp các vị trí trong đó`g`xuất hiện và tìm kiếm nhị phân cho vị trí đầu tiên lớn hơn`dist[u]`. Đây là thời điểm sớm nhất mà chúng ta có thể đi qua con đường này sau khi đến`u`. 
6. Nếu vị trí đó là`w`, sau đó chúng ta có thể đạt được`v`vào thời điểm đó`w`. Thư giãn`dist[v]`với`w`. Nếu như`w`nhỏ hơn giá trị hiện tại của`dist[v]`, chèn cặp mới vào heap. 
7. Dừng lại khi vào thành phố`n`bị xóa khỏi heap, vì Dijkstra loại bỏ các đỉnh theo thứ tự không giảm của khoảng cách đường đi ngắn nhất cuối cùng của chúng. Nếu heap trở nên trống trước tiên, thành phố`n`không thể truy cập được và câu trả lời là`-1`. 

Bất biến quan trọng là`dist[u]`là thời điểm sớm nhất có thể để chúng ta có thể đứng trong thành phố`u`. Đối với một cạnh thuộc cấu hình`g`, mọi giao dịch hợp pháp sau khi đạt được`u`phải xảy ra khi xảy ra`g`nghiêm ngặt sau`dist[u]`. Tìm kiếm nhị phân chọn sự xuất hiện sớm nhất như vậy, do đó quá trình chuyển đổi mang lại sự xuất hiện sớm nhất có thể tại điểm cuối khác thông qua cạnh đó. Sau đó, Dijkstra xem xét những chuyển đổi sớm nhất này theo thứ tự tăng dần, chính xác như đối với các đường đi ngắn nhất không âm thông thường. Vì mọi quá trình chuyển đổi đều tiến về phía trước theo thời gian nên không có quá trình chuyển đổi nào có thể dẫn trở lại trạng thái trước đó. 

## Giải pháp Python```python
import sys
import heapq
from bisect import bisect_right

input = sys.stdin.readline

def solve():
    n, t = map(int, input().split())

    graph = [[] for _ in range(n)]

    for g in range(t):
        m = int(input())
        for _ in range(m):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            graph[u].append((v, g))
            graph[v].append((u, g))

    k = int(input())
    sequence = list(map(int, input().split()))

    occurrences = [[] for _ in range(t)]
    for i, g in enumerate(sequence, 1):
        occurrences[g - 1].append(i)

    INF = k + 1
    dist = [INF] * n
    dist[0] = 0

    pq = [(0, 0)]

    while pq:
        du, u = heapq.heappop(pq)

        if du != dist[u]:
            continue

        if u == n - 1:
            print(du)
            return

        for v, g in graph[u]:
            times = occurrences[g]

            # We must use the road at a strictly later time.
            pos = bisect_right(times, du)

            if pos == len(times):
                continue

            nd = times[pos]

            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))

    print(-1)

if __name__ == "__main__":
    solve()
```Cấu trúc biểu đồ lưu trữ mỗi con đường hai lần, một lần từ mỗi điểm cuối và ghi nhớ cấu hình lịch sử của nó. Điều này chuyển đổi bộ sưu tập biểu đồ ban đầu thành một biểu đồ thưa thớt với nhãn thời gian trên các cạnh. 

Danh sách lần xuất hiện được xây dựng sau khi đọc chuỗi thời gian hoàn chỉnh. Bởi vì các vị trí được nối từ`1`bởi vì`k`, mọi danh sách đã được sắp xếp và có thể được tìm kiếm bằng`bisect_right`. 

Sự lựa chọn của`bisect_right(times, du)`là chi tiết triển khai trung tâm.`bisect_right`trả về lần xuất hiện đầu tiên lớn hơn`du`. sử dụng`bisect_left`sẽ cho phép đi ngang qua một cạnh cùng lúc với thành phố hiện tại, cho phép đi qua hai con đường một cách không chính xác sau một lần nhảy. 

Các cửa hàng heap`(time, city)`, do đó thời gian đến nhỏ nhất đã biết sẽ được xử lý trước tiên. Một mục nhập cũ bị bỏ qua khi thời gian lưu trữ của nó khác với`dist[u]`, đây là cách triển khai xóa lười tiêu chuẩn của Dijkstra. 

Nhiều nhất là mọi lúc`k`, Vì thế`INF = k + 1`là đủ. Số nguyên Python không bị tràn, nhưng dù sao cũng không cần trọng điểm lớn ở đây. 

Lối thoát sớm khi vào TP.`n`được bật ra là an toàn vì trật tự đống của Dijkstra đảm bảo rằng việc nới lỏng sau này không thể tạo ra thời gian đến thành phố đó nhỏ hơn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, trình tự là```
2 1 2 1 2 1
```cấu hình vậy`1`xuất hiện ở các vị trí`2, 4, 6`, trong khi cấu hình`2`xuất hiện ở các vị trí`1, 3, 5`. 

Những thư giãn quan trọng là: 

| Thành phố nổi bật | Thời điểm hiện tại | Cạnh | Cấu hình | Vị trí có thể sử dụng tiếp theo | Thành phố cập nhật | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 - 2 | 1 | 2 |`dist[2] = 2`| 
| 2 | 2 | 2 - 3 | 2 | 3 |`dist[3] = 3`| 
| 2 | 2 | 1 - 2 | 1 | 4 | không cải thiện | 
| 3 | 3 | 3 - 4 | 1 | 4 |`dist[4] = 4`| 
| 3 | 3 | 3 - 5 | 2 | 5 |`dist[5] = 5`| 

Thành phố`5`được bật lên với khoảng cách`5`, vậy câu trả lời là`5`. Sự chuyển đổi từ thành phố`3`đến thành phố`5`đặc biệt sử dụng vị trí`5`, không phải vị trí`3`, vì vị trí`3`đã được tiêu thụ khi thành phố`3`đã đạt được. 

Đối với mẫu thứ hai, cấu hình`1`xuất hiện ở các vị trí`1, 3, 4, 5`và cấu hình`2`chỉ xuất hiện ở vị trí`2`. 

| Thành phố nổi bật | Thời điểm hiện tại | Cạnh | Cấu hình | Vị trí có thể sử dụng tiếp theo | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 - 2 | 1 | 1 |`dist[2] = 1`| 
| 1 | 0 | 1 - 3 | 1 | 1 |`dist[3] = 1`| 
| 2 | 1 | 1 - 2 | 1 | 3 | không cải thiện | 
| 3 | 1 | 3 - 4 | 1 | 3 |`dist[4] = 3`| 
| 4 | 3 | 4 - 5 | 2 | không | không thể | 

Đường từ TP.`4`đến thành phố`5`thuộc về cấu hình`2`, nhưng cấu hình`2`chỉ xuất hiện ở vị trí`2`. Vào thời điểm thành phố`4`có thể truy cập được, sự việc đó đã xảy ra trong quá khứ. Do đó thành phố`5`vẫn không thể truy cập được và câu trả lời là`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(m log k + m log n + k)`| Mỗi cạnh có hướng được kiểm tra một lần trong mỗi lần trích xuất Dijkstra và lần xuất hiện có thể sử dụng tiếp theo của nó được tìm thấy bằng tìm kiếm nhị phân | 
| Không gian |`O(n + m + k)`| Đồ thị lưu trữ`2m`các cạnh có hướng, trong khi tất cả các danh sách xuất hiện cùng chứa`k`vị trí | 

Với`m <= 2 * 10^5`,`k <= 2 * 10^5`, Và`n <= 2 * 10^5`, thuật toán chỉ thực hiện công việc logarit cho mỗi lần giãn đường thay vì quét liên tục các đường cho mọi vị trí thời gian. Việc sử dụng bộ nhớ là tuyến tính theo kích thước đầu vào thực tế và vừa vặn trong giới hạn 512 MB. 

## Trường hợp thử nghiệm 

Dây nịt sau đây sử dụng tương tự`solve()`thực hiện từ giải pháp. các`run`trình trợ giúp tạm thời thay thế đầu vào tiêu chuẩn và ghi lại đầu ra tiêu chuẩn.```python
# Paste the solve() function from the solution above before this test code.

import sys
import io
from contextlib import redirect_stdout

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    out = io.StringIO()
    try:
        with redirect_stdout(out):
            solve()
    finally:
        sys.stdin = old_stdin

    return out.getvalue().strip()

# Provided sample 1
sample1 = """\
5 2
4
1 2
2 3
3 4
4 5
2
2 3
3 5
6
2 1 2 1 2 1
"""
assert run(sample1) == "5", "sample 1"

# Provided sample 2
sample2 = """\
5 2
3
1 2
3 1
4 3
2
2 1
4 5
5
1 2 1 1 1
"""
assert run(sample2) == "-1", "sample 2"

# Minimum-size input, first occurrence is immediately usable.
case_min = """\
2 1
1
1 2
1
1
"""
assert run(case_min) == "1", "minimum size"

# Two roads exist simultaneously, but only one can be used per time.
case_same_time = """\
3 1
2
1 2
2 3
1
1
"""
assert run(case_same_time) == "-1", "cannot traverse two roads at one time"

# Repeated occurrences of the same configuration.
case_repeated = """\
3 1
2
1 2
2 3
2
1 1
"""
assert run(case_repeated) == "2", "repeated configuration"

# Boundary case: a road becomes available again after the city is reached.
case_strict = """\
3 1
2
1 2
2 3
2
1 1
"""
assert run(case_strict) == "2", "strictly later occurrence"

# Large-size stress case: maximum n and k, with a direct road.
n = 200000
k = 200000
large_input = (
    f"{n} 1\n"
    f"1\n"
    f"1 {n}\n"
    f"{k}\n"
    + " ".join(["1"] * k)
    + "\n"
)
assert run(large_input) == "1", "large n and k"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1`, một con đường`1 2`, một lần xuất hiện |`1`| Đầu vào tối thiểu và lần sử dụng đầu tiên | 
|`3 1`, đường`1-2`,`2-3`, một lần xuất hiện |`-1`| Ngăn chặn hai lần truyền tải cùng một lúc | 
|`3 1`, chuỗi và hai lần xuất hiện |`2`| Sử dụng lại cấu hình tương tự ở vị trí sau | 
|`3 1`, chuỗi và hai lần xuất hiện giống hệt nhau |`2`| Tìm kiếm nhị phân sau này một cách nghiêm ngặt | 
|`n = 200000`,`k = 200000`, đường thẳng |`1`| Kích thước chuỗi và thành phố tối đa | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng đầu tiên là vị trí lần đầu tiên. Coi như:```
2 1
1
1 2
1
1
```Thuật toán bắt đầu với`dist[1] = 0`. Danh sách xuất hiện cho cấu hình`1`là`[1]`.`bisect_right([1], 0)`chỉ số trả về`0`, do đó đường có thể được sử dụng ở vị trí`1`Và`dist[2]`trở thành`1`. Câu trả lời là chính xác`1`. 

Trường hợp cạnh thứ hai là nhiều con đường trong một cấu hình lịch sử. Coi như:```
3 1
2
1 2
2 3
1
1
```Thành phố`1`có thể đến thành phố`2`vào thời điểm đó`1`, Nhưng`dist[2] = 1`có nghĩa là bất kỳ việc sử dụng đường nào sau đó từ thành phố`2`phải xảy ra nghiêm ngặt sau thời gian`1`. Không có sự xuất hiện sau này của cấu hình`1`, vậy thành phố`3`vẫn không thể truy cập được. Đầu ra của thuật toán`-1`. 

Trường hợp cạnh thứ ba là việc sử dụng lặp lại một cấu hình. Coi như:```
3 1
2
1 2
2 3
2
1 1
```Từ thành phố`1`, lần xuất hiện đầu tiên đến thành phố`2`vào thời điểm đó`1`. Khi xử lý cạnh từ thành phố`2`đến thành phố`3`,`bisect_right([1, 2], 1)`trả về lần xuất hiện thứ hai, vị trí`2`. Như vậy`dist[3] = 2`, đó chính xác là câu trả lời hợp lệ sớm nhất. 

Trường hợp cạnh thứ tư là một đích đến không thể truy cập được mặc dù có một số con đường tồn tại. Trong mẫu thứ hai, thành phố`4`đạt được vào thời điểm`3`, nhưng con đường duy nhất tới thành phố`5`thuộc về cấu hình`2`, sự xuất hiện duy nhất của nó là vào thời điểm`2`. Tìm kiếm nhị phân không tìm thấy sự xuất hiện nào lớn hơn`3`, do đó quá trình chuyển đổi đó đơn giản là không tồn tại. Dijkstra cuối cùng đã cạn kiệt tất cả các thành phố và đầu ra có thể tiếp cận`-1`. 

Quy tắc triển khai trung tâm đằng sau tất cả các trường hợp này là như nhau: đối với đường thuộc cấu hình`g`, sau khi đạt đến một điểm cuối tại một thời điểm`d`, chỉ lần xuất hiện đầu tiên của`g`thực sự lớn hơn`d`có liên quan. Khi quá trình chuyển đổi đó được thể hiện chính xác, phần còn lại của vấn đề là tính toán đường đi ngắn nhất đến sớm nhất theo tiêu chuẩn.
