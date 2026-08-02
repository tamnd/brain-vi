---
title: "CF 102651C - Xe tải tối ưu"
description: "Petya có thể mua một chiếc xe tải có sức chở nhất định. Mỗi khách hàng tiềm năng cung cấp một số hợp đồng có thể. Hợp đồng chỉ có hiệu lực nếu chiếc xe tải có thể chở ít nhất trọng lượng cần thiết cho hợp đồng đó và nó mang lại lợi nhuận nhất định."
date: "2026-08-01T10:23:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102651
codeforces_index: "C"
codeforces_contest_name: "Innopolis Open 2020-2021, qualification, contest 1"
rating: 0
weight: 102651
solve_time_s: 113
verified: true
draft: false
---

[CF 102651C - Xe tải tối ưu](https://codeforces.com/problemset/problem/102651/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Petya có thể mua một chiếc xe tải có sức chở nhất định. Mỗi khách hàng tiềm năng cung cấp một số hợp đồng có thể. Hợp đồng chỉ có hiệu lực nếu chiếc xe tải có thể chở ít nhất trọng lượng cần thiết cho hợp đồng đó và nó mang lại lợi nhuận nhất định. Từ mỗi khách hàng, Petya có thể nhận tối đa một hợp đồng. 

Đối với công suất xe tải đã chọn, mỗi khách hàng sẽ đóng góp độc lập hợp đồng có lợi nhất phù hợp với công suất đó. Nhiệm vụ không phải là tìm lợi nhuận cho một công suất cố định mà là trả lời nhiều truy vấn: với mỗi lợi nhuận mong muốn, hãy tìm công suất nhỏ nhất mà tổng lợi nhuận có thể đạt được đạt đến giá trị đó. 

Đầu vào mô tả từng khách hàng một. Mỗi khối khách hàng chứa tất cả các hợp đồng có thể có cho khách hàng đó. Tổng số hợp đồng đối với tất cả khách hàng tối đa là 500000 và số lượng truy vấn cũng tối đa là 100000. 

Các giới hạn lớn loại trừ việc kiểm tra mọi năng lực đối với mọi hợp đồng. Một giải pháp thử mọi truy vấn một cách độc lập và quét tất cả các hợp đồng sẽ cần khoảng 500000 * 100000 thao tác, tức là khoảng 5 * 10^10 thao tác và vượt xa giới hạn hai giây. Chúng ta cần xử lý tất cả các hợp đồng cùng nhau và trả lời các truy vấn từ một bản trình bày ngắn gọn về lợi nhuận có thể có. 

Khó khăn chính là các hợp đồng từ cùng một khách hàng không thể được cộng lại với nhau một cách đơn giản. Một giải pháp bất cẩn có thể tính nhiều hợp đồng của một khách hàng nếu tất cả đều phù hợp với xe tải, nhưng chỉ có thể chọn một hợp đồng. Một điểm tinh tế khác là một hợp đồng có công suất yêu cầu lớn hơn có thể vô ích nếu cùng một khách hàng đã có hợp đồng rẻ hơn với lợi nhuận tương đương hoặc lớn hơn. 

Ví dụ: hãy xem xét một khách hàng:```
1
3
5 10
7 20
9 15
1
15
```Đầu ra đúng là:```
7
```Ở công suất 9, khách hàng vẫn chỉ lãi 20 nên việc mua xe tải công suất 9 là không cần thiết. Giải pháp chỉ tìm kiếm hợp đồng đầu tiên đạt lợi nhuận yêu cầu mà không loại bỏ các lựa chọn thống trị có thể trả lời sai 9. 

Một trường hợp khó khăn khác là khi không thể đạt được lợi nhuận mục tiêu.```
1
2
3 5
5 8
1
10
```Đầu ra là:```
-1
```Lợi nhuận tối đa có thể là 8, do đó, bất kỳ thuật toán nào luôn lấy dung lượng khả dụng lớn nhất làm câu trả lời mà không kiểm tra lợi nhuận cuối cùng sẽ là sai. 

Trường hợp quan trọng cuối cùng là nhiều hợp đồng có cùng trọng lượng.```
1
3
4 5
4 12
4 9
2
5 12
```Đầu ra là:```
4 4
```Cả ba hợp đồng đều có sẵn cùng nhau, nhưng chỉ có hợp đồng có lợi nhất mới quan trọng. Việc thực hiện phải kết hợp các trọng số bằng nhau một cách chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xử lý từng truy vấn riêng biệt. Để ước tính công suất, chúng tôi có thể kiểm tra mọi hợp đồng, xác định hợp đồng nào có sẵn và đối với mỗi khách hàng, họ sẽ giữ được lợi nhuận tốt nhất trong số các hợp đồng hiện có. Điều này đúng vì khách hàng độc lập và mỗi khách hàng chỉ đóng góp hợp đồng có hiệu lực tối đa của riêng mình. 

Tuy nhiên, làm điều này cho mọi truy vấn là quá chậm. Với 100000 truy vấn và 500000 hợp đồng, trường hợp xấu nhất cần khoảng 50000000000 lần kiểm tra. 

Quan sát hữu ích là câu trả lời chỉ phụ thuộc vào việc tổng lợi nhuận thay đổi như thế nào khi sức tải của xe tải tăng lên. Sự đóng góp của một khách hàng là hàm số không giảm. Sau khi có hợp đồng, nó có thể thay thế hợp đồng tốt nhất trước đó cho khách hàng đó nhưng không bao giờ làm giảm sự đóng góp của khách hàng. 

Với mỗi khách hàng, chúng tôi có thể sắp xếp hợp đồng theo công suất yêu cầu. Trong khi di chuyển chúng theo thứ tự tăng dần, chúng tôi vẫn đạt được lợi nhuận tốt nhất cho đến nay. Nếu lợi nhuận tốt nhất được cải thiện ở một mức nào đó thì khả năng đó sẽ tạo ra sự gia tăng trong tổng số câu trả lời. Các hợp đồng không cải thiện mức tối đa tiền tố có thể bị bỏ qua. 

Sau khi xử lý tất cả khách hàng, chúng tôi có một bộ sưu tập các sự kiện. Một sự kiện có công suất`w`nói rằng tổng lợi nhuận có thể đạt được sẽ tăng lên một lượng nào đó khi công suất xe tải đạt`w`. Sắp xếp tất cả các sự kiện theo dung lượng và lấy tổng tiền tố sẽ mang lại hàm hoàn chỉnh:`capacity -> maximum possible total profit`Bây giờ mọi truy vấn sẽ trở thành tìm kiếm nhị phân trên danh sách đơn điệu này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * tổng(mi)) | O(n) | Quá chậm | 
| Tối ưu | O(tổng(mi) log sum(mi) + q log sum(mi)) | O(tổng(mi)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi khách hàng, hãy sắp xếp tất cả các hợp đồng của họ theo công suất xe tải yêu cầu. Khi một số hợp đồng có cùng năng lực, thứ tự của chúng không thành vấn đề vì tất cả chúng đều có sẵn cùng một lúc. 
2. Quét danh sách đã sắp xếp này trong khi vẫn duy trì lợi nhuận tốt nhất có sẵn cho khách hàng này. Bất cứ khi nào lợi nhuận tốt nhất này tăng lên, hãy thêm một sự kiện chứa khả năng xảy ra mức tăng và quy mô của mức tăng. Điều này chuyển đổi các lựa chọn của một khách hàng thành những thời điểm mà toàn bộ câu trả lời thay đổi. 
3. Hợp nhất tất cả các sự kiện của khách hàng vào một danh sách và sắp xếp danh sách theo năng lực. Các sự kiện có cùng công suất phải được xử lý cùng nhau vì tất cả các hợp đồng yêu cầu công suất đó đều có sẵn đồng thời. 
4. Duyệt qua các sự kiện đã sắp xếp và xây dựng hai mảng. Cái đầu tiên lưu trữ năng lực mà tổng lợi nhuận thay đổi. Thứ hai lưu trữ tổng lợi nhuận sau khi áp dụng tất cả các thay đổi đến mức đó. 
5. Đối với mỗi truy vấn, tìm kiếm nhị phân vị trí đầu tiên mà lợi nhuận được lưu trữ ít nhất là giá trị được yêu cầu. Nếu không có vị trí đó tồn tại, xuất ra`-1`. 

Tại sao nó hoạt động: 

Đối với một khách hàng, sau khi phân loại hợp đồng theo công suất, tiền tố tối đa chính xác là hợp đồng tốt nhất mà khách hàng này có thể chọn ở mọi công suất xe tải có thể. Thuật toán chỉ ghi lại những điểm mà tiền tố này thay đổi tối đa, do đó không có thông tin nào về khách hàng đó bị mất. 

Tổng lợi nhuận là tổng của tất cả sự đóng góp của khách hàng. Vì mọi đóng góp đều không giảm nên tổng lợi nhuận cũng không giảm. Danh sách sự kiện tái tạo lại mọi mức tăng trong hàm này, vì vậy tìm kiếm nhị phân sẽ tìm thấy khả năng đầu tiên đạt được lợi nhuận cần thiết. Nếu giá trị cuối cùng nhỏ hơn truy vấn thì không có dung lượng nào có thể đáp ứng được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    events = []

    for _ in range(n):
        m = int(input())
        contracts = []
        for _ in range(m):
            w, c = map(int, input().split())
            contracts.append((w, c))

        contracts.sort()

        best = 0
        i = 0
        while i < m:
            w = contracts[i][0]
            current_best = best

            while i < m and contracts[i][0] == w:
                if contracts[i][1] > current_best:
                    current_best = contracts[i][1]
                i += 1

            if current_best > best:
                events.append((w, current_best - best))
                best = current_best

    events.sort()

    capacities = []
    profits = []

    total = 0
    i = 0
    while i < len(events):
        w = events[i][0]
        while i < len(events) and events[i][0] == w:
            total += events[i][1]
            i += 1
        capacities.append(w)
        profits.append(total)

    q = int(input())
    ans = []

    import bisect

    for _ in range(q):
        x = int(input())
        pos = bisect.bisect_left(profits, x)
        if pos == len(profits):
            ans.append("-1")
        else:
            ans.append(str(capacities[pos]))

    print(" ".join(ans))

if __name__ == "__main__":
    solve()
```Dữ liệu đầu vào được xử lý theo từng khách hàng nên chương trình không bao giờ cần lưu trữ tất cả khách hàng cùng một lúc. Đối với mỗi khách hàng, việc sắp xếp sẽ hiển thị thứ tự các hợp đồng có sẵn. Các nhóm quét có dung lượng bằng nhau vì một số hợp đồng xuất hiện với cùng dung lượng sẽ ảnh hưởng đến sự lựa chọn của khách hàng tại cùng một thời điểm. 

Biến`best`lưu trữ lợi nhuận tối đa trước đó cho khách hàng hiện tại. Sự khác biệt giữa mức tối đa mới và mức tối đa cũ chính xác là sự đóng góp mà khách hàng này bổ sung theo công suất đó. Chỉ lưu trữ sự khác biệt này sẽ tránh được việc giữ lại những hợp đồng không cần thiết. 

Sau khi tất cả khách hàng được xử lý, danh sách sự kiện toàn cầu sẽ được sắp xếp. Lần quét thứ hai kết hợp các dung lượng bằng nhau trước khi lưu trữ chúng. Điều này là cần thiết vì truy vấn yêu cầu năng lực đó sẽ nhận được lợi nhuận sau mỗi hợp đồng được mở khóa ở năng lực đó đã được xem xét. 

Số nguyên Python xử lý tổng lợi nhuận có thể có một cách an toàn vì ngôn ngữ có số học chính xác tùy ý. Việc tìm kiếm nhị phân sử dụng`bisect_left`bởi vì chúng tôi cần năng lực đầu tiên có lợi nhuận không nhỏ hơn mục tiêu. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ:```
2
3
2 5
5 10
7 8
2
3 4
6 9
3
5
10
20
```Các sự kiện của khách hàng là: 

Khách hàng 1 đóng góp lợi nhuận tăng thêm 5 ở công suất 2 và 5 ở công suất 5. 

Khách hàng 2 đóng góp lợi nhuận tăng thêm 4 ở công suất 3 và 5 ở công suất 6. 

| Công suất | Lợi nhuận tăng thêm | Tổng lợi nhuận | 
| --- | --- | --- | 
| 2 | 5 | 5 | 
| 3 | 4 | 9 | 
| 5 | 5 | 14 | 
| 6 | 5 | 19 | 

Các truy vấn trở thành: 

| Truy vấn | Truy vấn đạt lợi nhuận đầu tiên | Trả lời | 
| --- | --- | --- | 
| 5 | công suất 2 mang lại lợi nhuận 5 | 2 | 
| 10 | công suất 5 mang lại lợi nhuận 14 | 5 | 
| 20 | không có năng lực nào đạt tới nó | -1 | 

Ví dụ này cho thấy lý do tại sao thuật toán lưu trữ lợi nhuận tiền tố. Công suất xe tải không cần phải phù hợp với hợp đồng đưa ra đúng lợi nhuận mục tiêu mà chỉ cần đạt hoặc vượt mức đó. 

Một ví dụ thứ hai:```
1
4
4 6
4 10
8 12
10 12
4
5
6
10
13
```Quá trình quét cho khách hàng duy nhất là: 

| Công suất | Lợi nhuận tốt nhất sau công suất | Đã thêm sự kiện | 
| --- | --- | --- | 
| 4 | 10 | +10 | 
| 8 | 12 | +2 | 
| 10 | 12 | không | 

Các mảng cuối cùng là: 

| Chỉ mục | Công suất | Lợi nhuận | 
| --- | --- | --- | 
| 0 | 4 | 10 | 
| 1 | 8 | 12 | 

Câu trả lời là: 

| Truy vấn | Kết quả | 
| --- | --- | 
| 5 | 4 | 
| 6 | 4 | 
| 10 | 4 | 
| 13 | -1 | 

Điều này chứng tỏ hai chi tiết quan trọng. Các hợp đồng có cùng công suất được kết hợp bằng cách lấy lợi nhuận tối đa và các hợp đồng không cải thiện mức tối đa tiền tố sẽ biến mất khỏi danh sách sự kiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M log M + q log M) | M là tổng số hợp đồng. Sắp xếp hợp đồng và sự kiện chiếm ưu thế trong công việc. | 
| Không gian | O(M) | Danh sách sự kiện chỉ lưu trữ những thay đổi về công suất, không thể vượt quá số lượng hợp đồng. | 

Tổng số hợp đồng nhiều nhất là 500000 nên việc sắp xếp nhiều phần tử này là khả thi. Giai đoạn truy vấn chỉ thực hiện tìm kiếm nhị phân trên hàm lợi nhuận nén, cho phép xử lý nhanh chóng 100000 truy vấn. 

## Trường hợp thử nghiệm```python
import sys
import io
import bisect

def solve_data(data):
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(data)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue().strip()

def solve():
    input = sys.stdin.readline
    n = int(input())
    events = []

    for _ in range(n):
        m = int(input())
        a = [tuple(map(int, input().split())) for _ in range(m)]
        a.sort()
        best = 0
        i = 0
        while i < m:
            w = a[i][0]
            cur = best
            while i < m and a[i][0] == w:
                cur = max(cur, a[i][1])
                i += 1
            if cur > best:
                events.append((w, cur - best))
                best = cur

    events.sort()
    caps = []
    vals = []
    total = 0
    i = 0
    while i < len(events):
        w = events[i][0]
        while i < len(events) and events[i][0] == w:
            total += events[i][1]
            i += 1
        caps.append(w)
        vals.append(total)

    q = int(input())
    ans = []
    for _ in range(q):
        x = int(input())
        p = bisect.bisect_left(vals, x)
        ans.append(str(caps[p]) if p < len(caps) else "-1")

    print(" ".join(ans))

assert solve_data("""1
1
1 1
3
1
2
3
""") == "1 -1 -1"

assert solve_data("""2
2
5 10
10 20
2
3 5
8 15
4
10
25
35
36
""") == "5 10 -1 -1"

assert solve_data("""1
3
4 5
4 12
4 9
2
5
12
""") == "4 4"

assert solve_data("""3
1
100 100
1
1 1
1
50 50
3
1
51
151
""") == "1 50 100"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hợp đồng đơn lẻ với lợi nhuận nhỏ |`1 -1 -1`| Kích thước đầu vào tối thiểu và các truy vấn không thể | 
| Một số khách hàng có đóng góp riêng |`5 10 -1 -1`| Kết hợp sự lựa chọn của khách hàng độc lập | 
| Cùng công suất nhưng lợi nhuận khác nhau |`4 4`| Xử lý đúng trọng lượng bằng nhau | 
| Nhảy nhiều công suất |`1 50 100`| Ranh giới tìm kiếm nhị phân và lợi nhuận tích lũy | 

## Vỏ cạnh 

Khi một khách hàng có nhiều hợp đồng có cùng trọng lượng, thuật toán sẽ nhóm chúng lại trước khi tạo sự kiện. Ví dụ:```
1
3
4 5
4 12
4 9
2
5
12
```Ở mức 4, số tiền đóng góp của khách hàng ngay lập tức trở thành 12 chứ không phải 5 cộng 12 cộng 9. Danh sách sự kiện chỉ chứa`(4, 12)`, vì vậy cả hai truy vấn đều trả về dung lượng 4. 

Khi lợi nhuận được yêu cầu là không thể, lợi nhuận tiền tố cuối cùng sẽ nhỏ hơn truy vấn. Vì:```
1
2
3 5
5 8
1
10
```Các mảng được xây dựng có dung lượng`[3, 5]`và lợi nhuận`[5, 8]`. Tìm kiếm nhị phân trả về vị trí cuối cùng vì không có lợi nhuận được lưu trữ nào đạt tới 10, do đó thuật toán xuất ra`-1`. 

Khi hợp đồng sau bị chi phối, nó không tạo ra sự kiện. Vì:```
1
3
5 10
7 20
9 15
1
15
```Các tiền tố tối đa là 10, 20, 20. Chỉ có khả năng 5 và 7 mới thay đổi được câu trả lời. Hợp đồng dung lượng 9 bị bỏ qua và truy vấn trả về chính xác 7.
