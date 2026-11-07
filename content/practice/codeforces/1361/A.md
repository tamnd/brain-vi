---
title: "CF 1361A - Johnny và sự đóng góp"
description: "Chúng ta được cho một đồ thị vô hướng. Mỗi đỉnh đại diện cho một blog và mỗi blog có một số chủ đề mong muốn t[i]. Johnny viết blog từng cái một. Khi viết blog, anh ấy chỉ nhìn vào những người hàng xóm đã viết blog."
date: "2026-06-11T12:47:51+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "graphs", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1361
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 647 (Div. 1) - Thanks, Algo Muse!"
rating: 1700
weight: 1361
solve_time_s: 247
verified: true
draft: false
---

[CF 1361A - Johnny và sự đóng góp](https://codeforces.com/problemset/problem/1361/A) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, đồ thị, tham lam, sắp xếp 
**Thời gian giải:** 4 phút 7 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng. Mỗi đỉnh đại diện cho một blog và mỗi blog có một số chủ đề mong muốn`t[i]`. 

Johnny viết blog từng cái một. Khi viết blog, anh ấy chỉ nhìn vào những người hàng xóm đã viết blog. Trong số các chủ đề của họ, anh ấy tìm thấy số nguyên dương nhỏ nhất không xuất hiện và gán chính xác chủ đề đó cho blog hiện tại. 

Các chủ đề mong muốn đã được cố định. Nhiệm vụ của chúng ta là tìm thứ tự viết blog sao cho quy tắc Johnny tạo ra chính xác chủ đề cho trước cho mọi đỉnh. Nếu không có thứ tự như vậy tồn tại, chúng ta phải xuất ra`-1`. 

Biểu đồ chứa tới`5 * 10^5`đỉnh và`5 * 10^5`các cạnh. Những giới hạn này ngay lập tức loại trừ mọi thứ bậc hai. Thậm chí`O(n√n)`sẽ không thoải mái. Vì bản thân đồ thị chỉ chứa`O(n + m)`dữ liệu, một giải pháp được chấp nhận phải ở gần tuyến tính hoặc`O((n + m) log n)`. 

Phần khó khăn là các chủ đề mong muốn không phải là những nhãn tùy ý. Chúng phải phù hợp với quy tắc giống mex mà Johnny sử dụng. Một đỉnh có chủ đề`k`chắc hẳn đã xem mọi chủ đề từ`1`bởi vì`k - 1`trong số các hàng xóm được viết trước đó của nó và nó không được nhìn thấy chủ đề`k`. 

Một số trường hợp phức tạp có thể dễ dàng phá vỡ một công trình đơn giản. 

Xét hai đỉnh liền kề có cùng chủ đề:```
2 1
1 2
1 1
```Nếu một trong số chúng được viết đầu tiên, thì cái còn lại đã nhìn thấy chủ đề`1`, nên nó cũng không nhận được topic`1`. Câu trả lời đúng là`-1`. 

Xét một đỉnh có chủ đề là`3`, nhưng nó chỉ có hàng xóm với chủ đề`1`Và`4`:```
3 2
1 2
1 3
3 1 4
```Đề tài`2`bị thiếu khỏi vùng lân cận của nó. Không có thứ tự nào có thể khiến Johnny giao chủ đề`3`tới đỉnh đó. Câu trả lời đúng là`-1`. 

Một trường hợp tinh vi khác là khi tất cả các chủ đề nhỏ hơn bắt buộc đều tồn tại nhưng một người hàng xóm có cùng chủ đề:```
3 3
1 2
2 3
1 3
2 1 2
```đỉnh`1`có chủ đề`2`. Nó nhìn thấy một người hàng xóm với chủ đề`1`, điều đó tốt, nhưng nó cũng có một người hàng xóm cùng chủ đề`2`. Khi hàng xóm chủ đề-2 đó được xử lý sớm hơn, chủ đề`2`đã có mặt trong số những người hàng xóm bằng văn bản, ngăn không cho mex bị`2`. Trường hợp này là không thể. 

Giải pháp phải xác minh tất cả các điều kiện này một cách hiệu quả. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử xây dựng một mệnh lệnh và liên tục mô phỏng quy tắc của Johnny. Đối với mỗi đỉnh, chúng ta có thể kiểm tra các đỉnh lân cận đã được xử lý, tính toán chủ đề còn thiếu nhỏ nhất và kiểm tra xem nó có bằng giá trị mong muốn hay không. 

Vấn đề là quyết định đỉnh nào tiếp theo. Khám phá các đơn đặt hàng có thể là theo cấp số nhân. Ngay cả khi bằng cách nào đó chúng ta đã sửa một thứ tự, việc tính toán lại mex từ đầu cho mỗi đỉnh có thể yêu cầu phải quét nhiều đỉnh lân cận nhiều lần, dẫn đến kết quả gần đúng.`O(nm)`làm việc trong tình huống dày đặc. 

Quan sát quan trọng xuất phát từ việc xem xét điều gì sẽ xảy ra nếu tồn tại một lệnh hợp lệ. 

Giả sử một đỉnh có chủ đề mong muốn`t`. 

Để Johnny giao chủ đề`t`, mọi chủ đề`1, 2, ..., t-1`phải đã xuất hiện trong số những người hàng xóm được viết trước đó. Vì các chủ đề đó phải tồn tại trước khi đỉnh này được viết nên tất cả các đỉnh có số chủ đề nhỏ hơn phải được xử lý sớm hơn. 

Điều này ngay lập tức gợi ý thứ tự có ý nghĩa duy nhất: xử lý các đỉnh theo thứ tự tăng dần của chủ đề mong muốn. Các đỉnh có cùng chủ đề có thể xuất hiện theo bất kỳ thứ tự tương đối nào. 

Bây giờ hãy xem xét một đỉnh`v`với chủ đề`t[v]`. 

Khi tất cả các đỉnh chủ đề nhỏ hơn đã được xử lý, tập hợp các chủ đề hiển thị trong`v`vùng lân cận của nó phải đủ chính xác để làm cho mex bằng nhau`t[v]`. 

Điều đó có nghĩa là: 

1. Không hàng xóm nào có thể có chủ đề`t[v]`. 
2. Mọi chủ đề từ`1`ĐẾN`t[v]-1`phải xuất hiện trong số các chủ đề của hàng xóm của nó. 

Nếu các điều kiện này đúng cho mọi đỉnh thì chỉ cần sắp xếp các đỉnh theo chủ đề là đã có thứ tự viết hợp lệ. Tất cả các lân cận có chủ đề nhỏ hơn đều được xử lý sớm hơn, tất cả các lân cận có cùng chủ đề đều vắng mặt và mọi chủ đề nhỏ hơn bắt buộc đều có mặt. 

Điều này chuyển vấn đề từ việc xây dựng một trật tự phức tạp thành việc xác minh điều kiện cục bộ cho mọi đỉnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Theo cấp số nhân hoặc tệ hơn | Lớn | Quá chậm | 
| Tối ưu | O((n + m) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc biểu đồ và chủ đề mong muốn của mỗi đỉnh. 
2. Tạo thứ tự các đỉnh bằng cách tăng giá trị chủ đề. 

Nếu tồn tại một giải pháp hợp lệ thì mọi đỉnh có chủ đề nhỏ hơn phải được xử lý trước mọi đỉnh có chủ đề lớn hơn. 
3. Đi qua các đỉnh theo thứ tự đã sắp xếp này. 
4. Đối với đỉnh hiện tại`v`, thu thập tập hợp các giá trị chủ đề xuất hiện trong số tất cả các hàng xóm của nó. 

Chúng tôi chỉ cần nhãn chủ đề chứ không cần danh tính hàng xóm. 
5. Nếu chủ đề`t[v]`xuất hiện trong số các chủ đề lân cận, câu trả lời là không thể. 

Johnny không thể nhận được chủ đề`t[v]`nếu chủ đề đó đã tồn tại giữa những người hàng xóm. 
6. Kiểm tra xem số lượng chủ đề lân cận có nhỏ hơn`t[v]`bằng`t[v] - 1`. 

Vì chủ đề là số nguyên dương nên điều này có nghĩa là mọi chủ đề`1, 2, ..., t[v]-1`có mặt đúng như yêu cầu. 

Một bộ được sử dụng nên việc trùng lặp không thành vấn đề. 
7. Nếu một trong hai lần kiểm tra không thành công, xuất ra`-1`. 
8. Nếu tất cả các đỉnh đều đạt, xuất các đỉnh theo thứ tự chủ đề tăng dần. 

### Tại sao nó hoạt động 

Xét một đỉnh`v`với chủ đề mong muốn`t`. 

Nếu có lệnh viết hợp lệ, mọi chủ đề từ`1`bởi vì`t-1`phải đã xuất hiện trong số những người hàng xóm được viết trước đó của`v`. Do đó,`v`phải có hàng xóm mang tất cả những giá trị chủ đề đó. Ngoài ra, chủ đề`t`bản thân nó không thể xuất hiện giữa các hàng xóm, nếu không chủ đề còn thiếu nhỏ nhất sẽ lớn hơn`t`. 

Đây là những điều kiện cần thiết. 

Họ cũng có đủ. Sau khi sắp xếp các đỉnh theo chủ đề tăng dần, mọi hàng xóm có chủ đề nhỏ hơn đảm bảo sẽ được xử lý sớm hơn. Vì mỗi chủ đề`1..t-1`tồn tại trong vùng lân cận và chủ đề`t`không, chủ đề còn thiếu nhỏ nhất trong số những người hàng xóm đã viết chính xác là`t`. Vì vậy, Johnny chỉ định chủ đề mong muốn. 

Vì mọi đỉnh đều thỏa mãn đối số này một cách độc lập nên thứ tự sắp xếp có giá trị cho toàn bộ biểu đồ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    g = [[] for _ in range(n)]

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    t = list(map(int, input().split()))

    order = sorted(range(n), key=lambda x: t[x])

    for v in order:
        cur = t[v]

        seen = set()

        for to in g[v]:
            topic = t[to]

            if topic == cur:
                print(-1)
                return

            if topic < cur:
                seen.add(topic)

        if len(seen) != cur - 1:
            print(-1)
            return

    print(*[v + 1 for v in order])

if __name__ == "__main__":
    solve()
```Biểu đồ được lưu trữ dưới dạng danh sách kề vì cả hai`n`Và`m`có thể đạt được`5 * 10^5`. 

Bước sắp xếp tạo ra thứ tự viết của ứng viên. Đây là quan sát trọng tâm của giải pháp: mọi thứ tự hợp lệ đều phải tôn trọng các giá trị chủ đề tăng dần. 

Đối với mỗi đỉnh, chỉ có chủ đề lân cận mới quan trọng. bộ`seen`lưu trữ các chủ đề riêng biệt nhỏ hơn chủ đề hiện tại. Các bản sao sẽ tự động bị bỏ qua. 

điều kiện```
len(seen) == cur - 1
```hoạt động vì mọi chủ đề đều tích cực. Nếu có chính xác`cur - 1`các chủ đề nhỏ hơn riêng biệt và tất cả chúng đều ít hơn`cur`, chúng phải chính xác là tập hợp`{1, 2, ..., cur - 1}`. 

Việc kiểm tra hàng xóm có cùng chủ đề là điều cần thiết. Nếu không có thử nghiệm này, các trường hợp như hai đỉnh liền kề đều được dán nhãn`1`sẽ vượt qua sai. 

Không tồn tại mối lo ngại về tràn số nguyên vì tất cả các giá trị đều vừa khít với số nguyên Python. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3
1 2
2 3
3 1
2 1 3
```Thứ tự sắp xếp theo chủ đề là`[2, 1, 3]`. 

| Đỉnh | Chủ đề | Chủ đề hàng xóm nhỏ hơn | Cùng chủ đề Hàng xóm tồn tại | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 2 | 1 | {} | Không | Có | 
| 1 | 2 | {1} | Không | Có | 
| 3 | 3 | {1,2} | Không | Có | 

Đầu ra:```
2 1 3
```Ví dụ này cho thấy hành vi dự định. Mỗi đỉnh nhìn thấy chính xác các chủ đề cần thiết để làm cho chủ đề của nó có giá trị mex. 

### Ví dụ không thể thực hiện được 

đầu vào:```
2 1
1 2
1 1
```Thứ tự sắp xếp là`[1, 2]`. 

| Đỉnh | Chủ đề | Chủ đề hàng xóm nhỏ hơn | Cùng chủ đề Hàng xóm tồn tại | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | {} | Có | Không | 

Thuật toán ngay lập tức xuất ra:```
-1
```Điều này chứng tỏ tại sao các đỉnh có chủ đề bằng nhau liền kề lại bị cấm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Chi phí sắp xếp O(n log n), chi phí quét đồ thị O(n + m) | 
| Không gian | O(n + m) | Danh sách kề cộng với các tập hợp tạm thời | 

Với`n, m ≤ 5 * 10^5`, việc truyền tải biểu đồ là tuyến tính ở kích thước đầu vào và bước sắp xếp dễ dàng đủ nhanh trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m = map(int, input().split())

    g = [[] for _ in range(n)]

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    t = list(map(int, input().split()))

    order = sorted(range(n), key=lambda x: t[x])

    for v in order:
        cur = t[v]

        seen = set()

        for to in g[v]:
            topic = t[to]

            if topic == cur:
                return "-1\n"

            if topic < cur:
                seen.add(topic)

        if len(seen) != cur - 1:
            return "-1\n"

    return " ".join(str(v + 1) for v in order) + "\n"

# sample 1
assert run(
"""3 3
1 2
2 3
3 1
2 1 3
"""
) == "2 1 3\n"

# minimum graph
assert run(
"""1 0
1
"""
) == "1\n"

# adjacent equal topics
assert run(
"""2 1
1 2
1 1
"""
) == "-1\n"

# missing smaller topic
assert run(
"""3 2
1 2
1 3
3 1 4
"""
) == "-1\n"

# disconnected graph
assert run(
"""3 0
1 1 1
"""
) == "1 2 3\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn |`1`| Ví dụ nhỏ nhất có thể | 
| Hai đỉnh chủ đề-1 liền kề |`-1`| Xung đột chủ đề bình đẳng | 
| Thiếu chủ đề dưới mục tiêu |`-1`| Các chủ đề vùng lân cận bắt buộc phải tồn tại | 
| Biểu đồ trống với tất cả chủ đề 1 | Bất kỳ hoán vị | Các thành phần bị ngắt kết nối hoạt động chính xác | 

## Vỏ cạnh 

Xem xét các đỉnh liền kề có chủ đề bằng nhau.```
2 1
1 2
1 1
```Đối với đỉnh`1`, tập chủ đề hàng xóm chứa chủ đề`1`, tương đương với chủ đề của chính nó. Thuật toán không kiểm tra và xuất ra cùng một chủ đề`-1`. Nếu việc kiểm tra này bị bỏ qua, phiên bản sẽ xuất hiện hợp lệ một cách không chính xác. 

Hãy xem xét một đỉnh thiếu một chủ đề nhỏ hơn được yêu cầu.```
3 2
1 2
1 3
3 1 4
```đỉnh`1`muốn chủ đề`3`. Hàng xóm của nó có chủ đề`{1,4}`. Tập các chủ đề lân cận nhỏ hơn là`{1}`. Kích thước của nó là`1`, Nhưng`3 - 1 = 2`. Thuật toán từ chối chính xác cá thể vì chủ đề`2`vắng mặt. 

Xem xét các chủ đề nhỏ hơn bị trùng lặp.```
4 3
1 2
1 3
1 4
3 1 1 1
```đỉnh`1`muốn chủ đề`3`. Hàng xóm của nó đóng góp chủ đề`{1,1,1}`. Một số người hàng xóm ngây thơ có thể nghĩ sai rằng có đủ chủ đề nhỏ hơn. Bộ này loại bỏ các bản sao, chỉ để lại`{1}`. Vì chủ đề`2`bị thiếu, thuật toán xuất ra`-1`. 

Hãy xem xét một đồ thị bị ngắt kết nối.```
3 0
1 1 1
```Mọi đỉnh đều muốn có chủ đề`1`. Mỗi đỉnh không có lân cận, do đó tập hợp các chủ đề nhỏ hơn trống và việc kiểm tra cùng một chủ đề không bao giờ được kích hoạt. Tất cả các đỉnh đều vượt qua xác thực và mọi thứ tự đều hợp lệ. Thuật toán đưa ra thứ tự sắp xếp.
