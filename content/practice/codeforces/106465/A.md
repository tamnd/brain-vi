---
title: "CF 106465A - Bóng"
description: "Cọc được xây dựng bằng cách thả các quả bóng theo một thứ tự vật lý cố định. Quả bóng tiếp theo luôn đi vào vị trí trống cao nhất có thể đỡ được nó và khi nhiều vị trí có cùng độ cao thì Antonella có thể chọn bất kỳ vị trí nào trong số đó."
date: "2026-06-25T08:57:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106465
codeforces_index: "A"
codeforces_contest_name: "Lincatoria Problems"
rating: 0
weight: 106465
solve_time_s: 41
verified: true
draft: false
---

[CF 106465A - Bóng](https://codeforces.com/problemset/problem/106465/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cọc được xây dựng bằng cách thả các quả bóng theo một thứ tự vật lý cố định. Quả bóng tiếp theo luôn đi vào vị trí trống cao nhất có thể đỡ được nó và khi nhiều vị trí có cùng độ cao thì Antonella có thể chọn bất kỳ vị trí nào trong số đó. Mỗi quả bóng có một màu và sau toàn bộ quá trình xây dựng, chúng tôi xem xét các nhóm bóng chạm được kết nối với nhau, tất cả đều có cùng màu. Nhiệm vụ là tìm ra nhóm lớn nhất như vậy trong số tất cả các đống có thể được tạo ra. 

Đầu vào đưa ra số lượng quả bóng và sau đó là màu sắc của các quả bóng theo thứ tự chính xác mà chúng được chèn vào. Đầu ra là một số nguyên: kích thước tối đa có thể có của thành phần đơn sắc được kết nối trong đống cuối cùng. 

Giới hạn 150 quả bóng đủ nhỏ để chúng ta có thể thực hiện phép tính lũy thừa theo số lượng thực tế các lựa chọn có ý nghĩa, nhưng không tính theo số lượng quả bóng. Việc tìm kiếm trên mọi vị trí có thể sẽ có hệ số phân nhánh rất lớn. Điều quan trọng là quá trình vật lý hạn chế nghiêm ngặt các hình dạng có thể có. Đống chỉ có thể có khoảng 17 hàng vì số tam giác thứ 17 đã là 153 nên số lượng quyết định trái/phải độc lập ít. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. 

Khi chỉ có một quả bóng thì không có lựa chọn nào khác và câu trả lời luôn là 1. 

Ví dụ đầu vào:```
1
7
```Đầu ra là:```
1
```Việc triển khai bất cẩn có thể cố gắng xây dựng cấu trúc hàng trước khi kiểm tra xem hàng hoàn chỉnh có tồn tại hay không, gây ra việc lập chỉ mục không chính xác. 

Một trường hợp khác là khi tất cả các quả bóng có cùng màu. Toàn bộ cọc được kết nối với nhau, bất kể các lựa chọn được đưa ra khi xây dựng nó. 

Ví dụ đầu vào:```
5
1 1 1 1 1
```Đầu ra là:```
5
```Giải pháp chỉ kiểm tra các quả bóng liền kề trong cùng một hàng sẽ bỏ sót các điểm tiếp xúc chéo giữa các hàng. 

Trường hợp phức tạp cuối cùng là hàng cuối cùng chưa hoàn chỉnh. Ví dụ:```
4
1 5 5 1
```Đầu ra là:```
2
```Sau ba quả bóng, hình tam giác đầu tiên đã hoàn thành. Quả bóng thứ tư phải kéo dài hàng dưới cùng và hai bên có thể tạo ra bố cục phản chiếu. Quả bóng cuối cùng chạm vào hai quả bóng trước đó nhưng không thể kết nối nó thông qua một vị trí không tồn tại ở phía chưa hoàn thành. Việc coi hàng cuối cùng như thể nó đã hoàn chỉnh sẽ tạo ra các kết nối sai. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp của tất cả các lựa chọn có thể có sẽ phân nhánh mỗi khi tồn tại hai vị trí cao như nhau. Đối với một đống 150 quả bóng, điều này có nghĩa là phải khám phá quá nhiều cách sắp xếp. Ý tưởng về lực lượng vũ phu là đúng vì mọi cọc được tạo ra đều có thể được mô phỏng và kiểm tra, nhưng số lượng khả năng sẽ tăng theo cấp số nhân với số lượng lựa chọn. 

Quan sát hữu ích đến từ việc nhìn vào hình học. Các vị trí sẵn có cao nhất luôn lấp đầy từng cọc theo hàng. Các quả bóng tạo thành các hàng có chiều dài là 1, 2, 3, v.v. từ trên xuống dưới. Khi một hàng bắt đầu được lấp đầy, quyền tự do duy nhất là hàng mới sẽ phát triển sang trái hay sang phải. Số lượng hàng có thể có là rất nhỏ nên số lượng các quyết định độc lập cũng rất nhỏ. 

150 quả bóng có thể chiếm tối đa 17 hàng vì tổng độ dài của 17 hàng đầu tiên là 153. Chúng ta có thể liệt kê mọi hướng có thể có của các hàng này. Đối với mỗi hướng, chúng tôi xây dựng biểu đồ liên hệ tương ứng của các quả bóng và chạy truyền tải biểu đồ để tìm thành phần được kết nối lớn nhất chứa một màu duy nhất. 

Số lượng bố cục nhiều nhất là khoảng 2^17, tức là khoảng 131 nghìn. Mỗi bố cục chỉ có 150 quả bóng, vì vậy việc kiểm tra tất cả các bố cục là điều thiết thực. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số lượng bố cục × N²) với số lượng bố cục theo cấp số nhân | O(N2) | Quá chậm nếu các lựa chọn được xử lý độc lập trên mỗi quả bóng | 
| Tối ưu | O(2^R × N²), trong đó R là số hàng | O(N2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia chuỗi chèn thành các hàng có độ dài 1, 2, 3, v.v. Hàng đầu tiên chứa quả bóng cuối cùng được thêm vào hàng đó vì các hàng được điền từ dưới lên, vì vậy chúng ta đảo ngược thứ tự xây dựng khi tạo bố cục hình học. 
2. Liệt kê mọi hướng trái/phải có thể có của các hàng. Một hàng có thể được phản chiếu và những lựa chọn này mô tả mọi ngăn xếp có thể có mà quá trình chèn có thể tạo ra. Có nhiều nhất là 17 hàng nên số lượng liệt kê nhỏ. 
3. Đối với mỗi hướng, đặt mỗi quả bóng ở tọa độ nguyên trên lưới tam giác. Các vị trí liên tiếp trong cùng một hàng chạm nhau và các vị trí ở các hàng lân cận chạm nhau theo đường chéo khi tọa độ của chúng khác nhau một lượng thích hợp. 
4. Xây dựng đồ thị kề của các quả bóng chạm nhau. Hai quả bóng được kết nối nếu chúng có chung một cạnh trong biểu đồ này. 
5. Chạy tìm kiếm chuyên sâu đầu tiên từ mọi quả bóng không được ghé thăm. Trong quá trình di chuyển, chỉ tiếp tục đi qua những quả bóng có cùng màu với quả bóng bắt đầu. Kích thước của quá trình truyền tải đó là một kích thước cụm có thể có. 
6. Giữ kích thước cụm lớn nhất được tìm thấy trên tất cả các hướng được tạo. 

Tại sao nó hoạt động: 

Quy tắc chèn không cho phép cọc ổn định tùy ý. Quy tắc vị trí cao nhất hiện có buộc cọc phải được điền vào các hàng hình tam giác. Quyền tự do duy nhất còn lại là mỗi hàng sẽ mở rộng về phía nào khi có hai lựa chọn có chiều cao bằng nhau. Do đó, việc liệt kê tất cả các hướng của hàng bao gồm mọi đống có thể. Đối với bất kỳ cọc cố định nào, đồ thị đi qua sẽ truy cập chính xác các quả bóng trong mỗi thành phần đơn sắc được kết nối, do đó mức tối đa tìm thấy cho cọc đó là chính xác. Lấy mức tối đa trên tất cả các cọc có thể sẽ đưa ra câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    colors = list(map(int, input().split()))

    rows = []
    idx = n - 1
    length = 1
    while idx >= 0:
        take = min(length, idx + 1)
        row = list(range(idx - take + 1, idx + 1))
        rows.append(row)
        idx -= take
        length += 1

    rows.reverse()

    row_count = len(rows)
    answer = 0

    # Coordinates for rows from top to bottom.
    base_positions = []
    for r, row in enumerate(rows):
        start = -r
        base_positions.append([start + 2 * i for i in range(len(row))])

    for mask in range(1 << row_count):
        coords = [None] * n
        positions = []
        for r, row in enumerate(rows):
            cur = base_positions[r][:]
            if (mask >> r) & 1:
                cur.reverse()
            positions.append(cur)
            for ball, x in zip(row, cur):
                coords[ball] = (x, r)

        graph = [[] for _ in range(n)]
        pos_to_ball = {}
        for i, p in enumerate(coords):
            pos_to_ball[p] = i

        directions = [(2, 0), (-2, 0), (1, 1), (-1, 1), (1, -1), (-1, -1)]
        for i, (x, y) in enumerate(coords):
            for dx, dy in directions:
                j = pos_to_ball.get((x + dx, y + dy))
                if j is not None:
                    graph[i].append(j)

        visited = [False] * n
        for i in range(n):
            if not visited[i]:
                color = colors[i]
                stack = [i]
                visited[i] = True
                size = 0
                while stack:
                    u = stack.pop()
                    size += 1
                    for v in graph[u]:
                        if not visited[v] and colors[v] == color:
                            visited[v] = True
                            stack.append(v)
                answer = max(answer, size)

    print(answer)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của mã sẽ xây dựng lại cấu trúc hàng. Thứ tự đầu vào là thứ tự chèn, trong khi các hàng hình học dễ xây dựng nhất từ ​​​​các quả bóng được chèn cuối cùng về phía sau. Vòng lặp liên tục loại bỏ độ dài hàng hoàn chỉnh cuối cùng khỏi chuỗi. 

Vòng liệt kê thử mọi hướng có thể có của mỗi hàng. Các hàng được lưu trữ với khả năng đảo ngược, thể hiện sự lựa chọn kéo dài hàng đó sang phía đối diện. 

Tọa độ được chỉ định sao cho các quả bóng chạm vào khác nhau theo một trong sáu hướng lưới tam giác lân cận. Một từ điển từ tọa độ đến chỉ số bóng cho phép chúng ta xây dựng biểu đồ mà không cần tìm kiếm qua tất cả các cặp bóng. 

Tìm kiếm theo chiều sâu đầu tiên chỉ đếm các quả bóng có cùng màu với quả bóng bắt đầu. Mảng đã truy cập được chia sẻ trong quá trình truyền tải đó vì các quả bóng có màu khác không thể thuộc cùng một cụm. Giá trị tối đa trên tất cả các bố cục là kết quả cuối cùng. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên: 

đầu vào:```
3
1 2 1
```Các hàng là một quả bóng nằm trên hai quả bóng. Có thể có hai tấm gương, nhưng cả hai đều tạo ra cùng kích thước cụm. 

| Bước | Hàng | Cụm hiện tại | 
| --- | --- | --- | 
| Xây dựng hàng | trên: bóng 3, dưới: bóng 1 và 2 | không | 
| Gán màu | màu trên 1, màu dưới 1 và 2 | màu 1 nối trên và dưới | 
| DFS | thăm bóng 1 và 3 | cỡ 2 | 

Câu trả lời là:```
2
```Điều này chứng tỏ rằng phải bao gồm sự tiếp xúc theo đường chéo giữa các hàng. 

Đối với mẫu thứ hai: 

đầu vào:```
4
1 5 5 1
```Các bố cục có thể là hình ảnh phản chiếu. 

| Bước | Hàng | Cụm hiện tại | 
| --- | --- | --- | 
| Xây dựng hàng | trên: bóng 3, dưới: bóng 1, 2 | không | 
| Thêm bóng thứ tư | hàng dưới cùng trở thành ba quả bóng | bóng mới chạm vào hai bóng cũ | 
| DFS | các nhóm cùng màu được kiểm tra | nhóm lớn nhất có cỡ 2 | 

Câu trả lời là:```
2
```Điều này thể hiện tác động của việc hàng dưới cùng không đầy đủ và tránh cho rằng mọi hàng đều đầy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^R × N²) | Có tối đa 17 lựa chọn hướng hàng và mỗi bố cục yêu cầu xây dựng biểu đồ và duyệt qua nó | 
| Không gian | O(N2) | Đồ thị kề chỉ chứa một số cạnh không đổi trên mỗi quả bóng, nhưng biểu diễn được giới hạn bởi N² | 

Số lượng hàng lớn nhất có thể là nhỏ vì 150 quả bóng gần như lấp đầy một hình tam giác có 17 hàng. Do đó, kích thước liệt kê là khoảng 131 nghìn bố cục và mỗi bố cục chỉ xử lý 150 quả bóng, vừa vặn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue()

# provided samples
assert run("3\n1 2 1\n") == "2\n", "sample 1"
assert run("3\n1 1 1\n") == "3\n", "sample 2"

# custom cases
assert run("1\n7\n") == "1\n", "single ball"
assert run("5\n1 1 1 1 1\n") == "5\n", "all equal colors"
assert run("6\n1 2 2 1 2 3\n") == "3\n", "larger mixed example"
assert run("4\n1 5 5 1\n") == "2\n", "unfinished row boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7`|`1`| Cọc kích thước tối thiểu | 
|`5 / 1 1 1 1 1`|`5`| Kết nối đầy đủ các màu giống hệt nhau | 
|`6 / 1 2 2 1 2 3`|`3`| Kết nối qua nhiều hàng | 
|`4 / 1 5 5 1`|`2`| Xử lý một phần hàng cuối cùng | 

## Vỏ cạnh 

Đối với một quả bóng duy nhất:```
1
7
```Việc phân tách hàng tạo ra một hàng chứa một quả bóng. Không có cặp nào chạm nhau nên phép duyệt sẽ trả về một cặp. Đầu ra của thuật toán`1`. 

Đối với một đống bằng nhau:```
5
1 1 1 1 1
```Mọi hướng được tạo đều có cùng màu trên mọi đỉnh của biểu đồ liên hệ. Việc di chuyển từ bất kỳ quả bóng bắt đầu nào tới tất cả năm quả bóng, mang lại kích thước cụm tối đa có thể là`5`. 

Đối với trường hợp một phần hàng:```
4
1 5 5 1
```Việc phân rã tạo ra một hàng dưới cùng có ba vị trí và một hàng trên cùng có một vị trí. Việc liệt kê thử cả hai cách đặt quả bóng thứ tư. Trong mỗi trường hợp, việc xây dựng biểu đồ chỉ thêm các liên hệ thực giữa các quả bóng hiện có, do đó không có hàng xóm tưởng tượng nào ở vị trí bị thiếu có thể tham gia vào cụm. Thành phần đơn sắc lớn nhất vẫn giữ nguyên kích thước`2`.
