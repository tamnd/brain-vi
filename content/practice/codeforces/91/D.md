---
title: "CF 91D - Vấn đề của người bán tạp hóa"
description: "Chúng tôi được cho một hoán vị của lọ. Bình i cuối cùng sẽ đứng ở vị trí i, nhưng sự sắp xếp hiện tại đã bị xáo trộn. Một thao tác cho phép chúng ta chọn bất kỳ tập hợp con nào có tối đa năm vị trí và hoán vị các lọ bên trong các vị trí đó một cách tùy ý."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 91
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 75 (Div. 1 Only)"
rating: 2600
weight: 91
solve_time_s: 145
verified: true
draft: false
---

[CF 91D - Vấn đề của người bán tạp hóa](https://codeforces.com/problemset/problem/91/D) 

**Đánh giá:** 2600 
**Tags:** thuật toán xây dựng, đồ thị, tham lam 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cho một hoán vị của lọ. Cái lọ`i`cuối cùng nên đứng ở vị trí`i`, nhưng sự sắp xếp hiện tại bị xáo trộn. 

Một thao tác cho phép chúng ta chọn bất kỳ tập hợp con nào có tối đa năm vị trí và hoán vị các lọ bên trong các vị trí đó một cách tùy ý. Các vị trí không cần phải liên tiếp. Nhiệm vụ không chỉ là sắp xếp hoán vị mà còn thực hiện nó bằng cách sử dụng số lượng thao tác tối thiểu có thể, đồng thời tự in các thao tác đó. 

Phần thú vị là một thao tác cực kỳ mạnh mẽ. Nếu chúng ta chọn năm vị trí, chúng ta có thể áp dụng bất kỳ hoán vị nào trên năm chiếc lọ đó chỉ trong một nước đi. Điều đó có nghĩa là vấn đề không nằm ở mô phỏng hoặc hoán đổi cục bộ. Đó là về việc hiểu mức độ rối loạn có thể được loại bỏ trong mỗi ca phẫu thuật. 

Kích thước đầu vào đạt`10^5`, do đó, bất kỳ cách tiếp cận nào tìm kiếm trên các trạng thái hoặc cố gắng tối ưu hóa toàn cầu bằng kỹ thuật hàm mũ đều không thể thực hiện được. Thậm chí một`O(n^2)`thuật toán nguy hiểm dưới giới hạn 2 giây. Chúng ta cần một cái gì đó gần với thời gian tuyến tính. 

Cấu trúc hoán vị ngay lập tức gợi ý sự phân hủy chu trình. Mọi hoán vị có thể được chia thành các chu trình độc lập và việc sửa một chu trình không bao giờ ảnh hưởng đến chu trình khác. Vì một thao tác có thể sắp xếp lại tối đa năm phần tử đã chọn một cách tùy ý, nên câu hỏi tự nhiên sẽ trở thành: có thể sửa được bao nhiêu chu kỳ trong một lần di chuyển? 

Có một số trường hợp nguy hiểm có thể dễ dàng phá vỡ việc triển khai bất cẩn. 

Một chu kỳ dài`1`đã đúng rồi và phải được bỏ qua. Ví dụ:```
1
1
```Câu trả lời là`0`. Việc triển khai có lỗi xử lý một cách mù quáng mọi chỉ mục như một phần của chu trình có thể tạo ra các hoạt động không cần thiết. 

Chu kỳ có độ dài tối đa`5`có thể được sửa chữa trong chính xác một thao tác. Ví dụ:```
5
2 3 4 5 1
```Đây là một 5 chu kỳ duy nhất. Chúng tôi chọn tất cả năm vị trí và xoay chúng theo đúng thứ tự trong một lần di chuyển. 

Chu kỳ dài là phần khó khăn. Coi như:```
8
2 3 4 5 6 7 8 1
```Đây là một chu kỳ dài`8`. Một kẻ tham lam ngây thơ liên tục sửa năm vị trí một cách độc lập có thể vô tình phá hủy các vị trí đã đúng hoặc sử dụng quá nhiều thao tác. Việc phân hủy tối ưu đòi hỏi phải cẩn thận chia chu trình thành các phần. 

Một trường hợp tinh vi khác xuất hiện với các chu kỳ có độ dài`6`. Vì chúng ta chỉ có thể thao tác tối đa năm vị trí nên một thao tác là không thể. Nhưng hai thao tác là đủ:```
6
2 3 4 5 6 1
```Một giải pháp giả định mọi nhu cầu của chu kỳ`ceil(len / 5)`hoạt động mang lại`2`, điều này đúng ở đây, nhưng việc xây dựng thực tế mới quan trọng. Chúng ta phải tạo ra các phép toán hợp lệ có thành phần sắp xếp hoán vị. 

Phần khó nhất của vấn đề không phải là số lượng mà là xây dựng các hoạt động đạt được mức tối thiểu một cách chính xác. 

## Phương pháp tiếp cận 

Quan điểm vũ phu là suy nghĩ trực tiếp về các phép toán trên hoán vị. Ở mỗi bước, chúng tôi chọn tối đa năm vị trí và áp dụng một số sắp xếp lại. Vì có khoảng`O(n^5)`các tập hợp con có thể có và nhiều hoán vị cho mỗi tập hợp con thì hệ số phân nhánh sẽ bùng nổ ngay lập tức. Ngay cả đối với`n = 20`, điều này đã vô vọng rồi. 

Một ý tưởng mạnh mẽ hơn một chút là xử lý các chu kỳ một cách độc lập. Giả sử một chu trình có độ dài`k`. Vì một thao tác có thể sắp xếp lại tối đa năm vị trí, nên có thể chúng ta có thể tham lam sửa tối đa năm phần tử bị đặt sai vị trí cùng một lúc. Điều này hoạt động đúng chức năng, nhưng không tối ưu. Sự phân hủy bất cẩn của một chu kỳ dài có thể gây lãng phí hoạt động. 

Quan sát quan trọng là một thao tác có thể giải quyết hoàn toàn bất kỳ chu trình nào có độ dài tối đa là năm. 

Thực hiện một chu kỳ:```
(a1 -> a2 -> a3 -> ... -> ak -> a1)
```Nếu như`k <= 5`, việc chọn tất cả các vị trí của chu trình cho phép chúng ta đưa từng lọ trực tiếp đến đúng vị trí của nó chỉ bằng một lần di chuyển. 

Điều đó thay đổi hoàn toàn vấn đề. Thay vì suy nghĩ về các hoán vị tùy ý, chúng ta chỉ cần hiểu cách phân tách các chu trình dài thành các chu trình nhỏ hơn, mỗi chu trình phù hợp với một thao tác. 

Giả sử một chu trình có độ dài`k > 5`. 

Một thao tác trên năm phần tử có thể giảm độ dài chu kỳ xuống bốn. Lý do là chúng ta có thể cố định vĩnh viễn bốn phần tử trong khi để các phần tử còn lại trong một chu kỳ nhỏ hơn. 

Ví dụ, từ một chu kỳ có độ dài`8`:```
(1 2 3 4 5 6 7 8)
```chúng ta có thể trích xuất 5 chu kỳ bao gồm:```
(1 2 3 4 5)
```Sau khi sửa chữa chúng một cách thích hợp, sự rối loạn còn lại sẽ trở thành một chu kỳ dài`4`. 

Điều này dẫn đến công thức tối ưu:```
operations = ceil((k - 1) / 4)
```bởi vì mỗi thao tác có thể loại bỏ tối đa bốn sai sót độc lập khỏi một chu kỳ. 

Thử thách còn lại mang tính xây dựng. Chúng ta cần một cách có hệ thống để phân chia chu trình và in các hoạt động hợp lệ. 

Thủ thuật tiêu chuẩn là giảm đệ quy. Trong khi chiều dài chu kỳ vượt quá`5`, chúng ta lấy năm đỉnh liên tiếp từ chu trình, áp dụng một thao tác cố định bốn đỉnh trong số đó và tiếp tục với chu trình dư nhỏ hơn. 

Khi độ dài chu kỳ còn lại đạt mức tối đa`5`, một thao tác cuối cùng sẽ giải quyết nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hoán vị và phân tách nó thành các chu trình rời nhau. 

Chúng tôi duy trì một mảng đã truy cập. Bắt đầu từ mọi vị trí chưa được ghé thăm, chúng tôi liên tục theo dõi`p[i]`cho đến khi quay lại từ đầu. 
2. Bỏ qua chu kỳ độ dài`1`. 

Những vị trí đó đã chính xác và không bao giờ cần xuất hiện trong bất kỳ thao tác nào. 
3. Đối với mỗi chu kỳ có độ dài tối đa`5`, giải quyết nó trong một thao tác. 

Giả sử chu kỳ là:```
c0 -> c1 -> c2 -> ... -> ck-1 -> c0
```Chúng tôi chọn những vị trí này và xoay các lọ về phía sau để mọi lọ đều di chuyển đến đúng vị trí của nó. 
4. Đối với chu kỳ dài hơn`5`, liên tục trích xuất các thao tác trên năm vị trí. 

Cho chu kỳ hiện tại là:```
[v0, v1, v2, ..., vm-1]
```Chúng tôi lấy:```
[v0, v1, v2, v3, v4]
```và thực hiện một thao tác sửa lỗi`v1, v2, v3, v4`. 

Sau thao tác này, chu trình còn lại sẽ trở thành:```
[v0, v5, v6, ..., vm-1]
```nên chiều dài của nó giảm đi bốn. 
5. Tiếp tục cho đến khi độ dài chu kỳ còn lại đạt mức tối đa`5`. 
6. Giải chu trình nhỏ cuối cùng bằng một thao tác. 
7. In tất cả các thao tác. 

### Tại sao nó hoạt động 

Điều bất biến là mọi thành phần chưa hoàn thiện luôn tồn tại một chu trình duy nhất. 

Mỗi thao tác trên năm phần tử sẽ loại bỏ vĩnh viễn bốn đỉnh khỏi chu trình đó. Những đỉnh đó trở nên cố định mãi mãi và không bao giờ tham gia nữa. 

Một chu kỳ dài`k`yêu cầu ít nhất`ceil((k - 1) / 4)`hoạt động vì một hoạt động có thể bao gồm tối đa năm yếu tố, nghĩa là tối đa bốn vị trí mới có thể trở thành chính xác. Thuật toán đạt được chính xác giới hạn này bằng cách thu nhỏ chu kỳ đi bốn lần mỗi lần. 

Vì các chu trình là độc lập nên việc giải quyết từng chu trình một cách tối ưu cũng mang lại giải pháp tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    p = [0] + list(map(int, input().split()))

    vis = [False] * (n + 1)
    ops = []

    def apply_cycle(cyc):
        k = len(cyc)

        pos = cyc
        target = cyc[1:] + cyc[:1]

        ops.append((k, pos[:], target[:]))

    for i in range(1, n + 1):
        if vis[i]:
            continue

        cur = []
        x = i

        while not vis[x]:
            vis[x] = True
            cur.append(x)
            x = p[x]

        m = len(cur)

        if m == 1:
            continue

        while m > 5:
            part = cur[:5]

            apply_cycle(part)

            cur = [cur[0]] + cur[5:]
            m = len(cur)

        apply_cycle(cur)

    print(len(ops))

    for k, b, c in ops:
        print(k)
        print(*b)
        print(*c)

if __name__ == "__main__":
    solve()
```Phần đầu tiên phân tách hoán vị thành các chu trình bằng cách sử dụng phương pháp truyền tải kiểu DFS tiêu chuẩn trên các cạnh hoán vị. Mỗi chỉ mục được truy cập chính xác một lần, vì vậy phần này là tuyến tính. 

Người trợ giúp`apply_cycle`chuyển đổi một chu trình trực tiếp thành một hoạt động của máy. Giả sử chu kỳ là:```
[a, b, c, d]
```Hiện nay:```
p[a] = b
p[b] = c
p[c] = d
p[d] = a
```Các bản đồ hoạt động:```
a -> b
b -> c
c -> d
d -> a
```sẽ đưa mọi bình về đúng vị trí của nó. 

Phần tế nhị nhất là thu hẹp chu kỳ lớn. Sau khi lấy năm đỉnh đầu tiên, thao tác sẽ cố định vĩnh viễn bốn trong số chúng trong khi đỉnh đầu tiên vẫn được kết nối với phần còn lại của chu trình. Thay thế:```
[v0, v1, v2, v3, v4, v5, ...]
```với:```
[v0, v5, ...]
```bảo tồn cấu trúc tuần hoàn còn lại. 

Một lỗi triển khai phổ biến là đảo ngược hướng ánh xạ. Câu lệnh định nghĩa thao tác là "jar từ vị trí`bi`đi đến vị trí`ci`". Trong một chu kỳ, chúng ta phải dịch chuyển vị trí về phía trước đúng một lần. 

Một lỗi dễ xảy ra khác là vô tình bao gồm các đỉnh đã được sửa trong các thao tác sau này. Quá trình thu nhỏ sẽ tự động tránh điều đó vì các đỉnh bị loại bỏ sẽ không bao giờ xuất hiện nữa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
3 5 6 1 2 4
```Sự phân rã chu trình mang lại:```
(1 3 6 4)
(2 5)
```#### Chu kỳ đầu tiên 

| Bước | Chu kỳ hiện tại | Vị trí hoạt động | Mục tiêu hoạt động | 
| --- | --- | --- | --- | 
| 1 | [1, 3, 6, 4] | [1, 3, 6, 4] | [3, 6, 4, 1] | 

Sau hoạt động này, các vị trí`1, 3, 4, 6`trở nên đúng đắn. 

#### Chu kỳ thứ hai 

| Bước | Chu kỳ hiện tại | Vị trí hoạt động | Mục tiêu hoạt động | 
| --- | --- | --- | --- | 
| 2 | [2, 5] | [2, 5] | [5, 2] | 

Hoán vị trở nên được sắp xếp. 

Dấu vết này cho thấy mọi chu kỳ có độ dài tối đa là năm đều được giải trực tiếp trong một nước đi. 

### Ví dụ 2 

đầu vào:```
8
2 3 4 5 6 7 8 1
```Đây là một chu kỳ:```
(1 2 3 4 5 6 7 8)
```#### Giai đoạn giảm 

| Bước | Chu kỳ hiện tại | Vị trí được lựa chọn | Kết quả chu kỳ còn lại | 
| --- | --- | --- | --- | 
| 1 | [1,2,3,4,5,6,7,8] | [1,2,3,4,5] | [1,6,7,8] | 

Sửa lỗi thao tác đầu tiên`2,3,4,5`. 

####Giai đoạn cuối 

| Bước | Chu kỳ hiện tại | Vị trí được lựa chọn | Mục tiêu | 
| --- | --- | --- | --- | 
| 2 | [1,6,7,8] | [1,6,7,8] | [6,7,8,1] | 

Bây giờ toàn bộ hoán vị đã được sắp xếp. 

Ví dụ này chứng tỏ tính bất biến rằng mỗi lần giảm sẽ rút ngắn chu kỳ hoạt động chính xác bằng bốn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi đỉnh đi vào một chu trình một lần và tham gia nhiều nhất một bước rút gọn | 
| Không gian | O(n) | Mảng đã truy cập, lưu trữ chu trình và danh sách thao tác | 

Giải pháp dễ dàng phù hợp với các ràng buộc. Truyền tuyến tính qua`10^5`các phần tử vẫn nằm trong giới hạn thời gian và mức sử dụng bộ nhớ vẫn ở mức khiêm tốn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    p = [0] + list(map(int, input().split()))

    vis = [False] * (n + 1)
    ops = []

    def apply_cycle(cyc):
        k = len(cyc)

        pos = cyc
        target = cyc[1:] + cyc[:1]

        ops.append((k, pos[:], target[:]))

    for i in range(1, n + 1):
        if vis[i]:
            continue

        cur = []
        x = i

        while not vis[x]:
            vis[x] = True
            cur.append(x)
            x = p[x]

        if len(cur) == 1:
            continue

        while len(cur) > 5:
            ops.append((5, cur[:5], cur[1:5] + [cur[0]]))
            cur = [cur[0]] + cur[5:]

        apply_cycle(cur)

    out = [str(len(ops))]

    for k, b, c in ops:
        out.append(str(k))
        out.append(" ".join(map(str, b)))
        out.append(" ".join(map(str, c)))

    return "\n".join(out)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# sample 1
assert run("6\n3 5 6 1 2 4\n").splitlines()[0] == "2"

# already sorted
assert run("1\n1\n").splitlines()[0] == "0"

# single 5-cycle
assert run("5\n2 3 4 5 1\n").splitlines()[0] == "1"

# single 6-cycle
assert run("6\n2 3 4 5 6 1\n").splitlines()[0] == "2"

# long cycle
assert run("8\n2 3 4 5 6 7 8 1\n").splitlines()[0] == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`0`hoạt động | Đã sắp xếp hoán vị | 
|`5 / 2 3 4 5 1`|`1`hoạt động | Chu kỳ tối đa có thể giải quyết trực tiếp | 
|`6 / 2 3 4 5 6 1`|`2`hoạt động | Ranh giới nơi một thao tác không còn đủ | 
|`8 / 2 3 4 5 6 7 8 1`|`2`hoạt động | Logic thu nhỏ chu kỳ đúng | 

## Vỏ cạnh 

Hãy xem xét hoán vị đã được sắp xếp:```
4
1 2 3 4
```Mỗi chu kỳ đều có độ dài`1`. Quá trình truyền tải sẽ truy cập vào từng nút, ngay lập tức phát hiện ra vòng tự lặp và bỏ qua nó. Không có hoạt động nào được thêm vào. Đầu ra là chính xác:```
0
```Bây giờ hãy xem xét chu trình không cần thiết nhỏ nhất:```
2
2 1
```Chu kỳ là:```
(1 2)
```Thuật toán áp dụng một thao tác:```
positions: 1 2
targets:   2 1
```Bình từ vị trí`1`di chuyển đến`2`và lọ từ`2`di chuyển đến`1`. Hoán vị sẽ được sắp xếp ngay lập tức. 

Trong một chu kỳ dài:```
9
2 3 4 5 6 7 8 9 1
```sự phân hủy chu trình mang lại:```
(1 2 3 4 5 6 7 8 9)
```Thuật toán xử lý đầu tiên:```
[1 2 3 4 5]
```rời đi:```
[1 6 7 8 9]
```Chu kỳ còn lại bây giờ có độ dài`5`, do đó một thao tác cuối cùng sẽ giải quyết được nó. Tổng số hoạt động:```
2
```phù hợp với giới hạn dưới:```
ceil((9 - 1) / 4) = 2
```Cuối cùng, hãy xem xét nhiều chu kỳ độc lập:```
7
2 1 4 3 6 7 5
```Sự phân hủy là:```
(1 2)
(3 4)
(5 6 7)
```Mỗi chu trình được giải quyết độc lập trong một thao tác. Không có thao tác nào can thiệp vào chu trình khác vì các chu trình chiếm các vị trí rời rạc.
