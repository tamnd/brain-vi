---
title: "CF 13E - Lỗ"
description: "Chúng tôi có một dòng lỗ được lập chỉ mục từ trái sang phải. Mỗi lỗ có chiều dài bước nhảy. Nếu một quả bóng được thả vào lỗ i, nó sẽ ngay lập tức di chuyển đến i + a[i]. Từ đó nó lại nhảy bằng cách sử dụng giá trị của lỗ mới và điều này tiếp tục cho đến lần nhảy tiếp theo rời khỏi mảng."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dsu"]
categories: ["algorithms"]
codeforces_contest: 13
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 13"
rating: 2700
weight: 13
solve_time_s: 113
verified: true
draft: false
---
[CF 13E - Lỗ](https://codeforces.com/problemset/problem/13/E) 

**Đánh giá:** 2700 
**Thẻ:** cấu trúc dữ liệu, dsu 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dòng lỗ được lập chỉ mục từ trái sang phải. Mỗi lỗ có chiều dài bước nhảy. Nếu quả bóng được thả vào lỗ`i`, nó ngay lập tức chuyển sang`i + a[i]`. Từ đó nó lại nhảy bằng cách sử dụng giá trị của lỗ mới và điều này tiếp tục cho đến lần nhảy tiếp theo rời khỏi mảng. 

Hai loại hoạt động phải được xử lý trực tuyến. 

Thao tác đầu tiên thay đổi độ dài bước nhảy của một lỗ. 

Thao tác thứ hai bắt đầu một quả bóng từ một lỗ nào đó và yêu cầu hai giá trị: 

1. Lỗ cuối cùng được ghé thăm trước khi bóng thoát ra khỏi mảng. 
2. Tổng số lần nhảy được thực hiện. 

Cấu trúc thực sự là một đồ thị có hướng trong đó mỗi nút có chính xác một cạnh đi ra, tới một lỗ khác hoặc bên ngoài mảng. Vì mọi bước nhảy đều di chuyển hoàn toàn về bên phải nên không thể thực hiện được các chu kỳ. Mọi đường dẫn cuối cùng đều rời khỏi mảng. 

Các ràng buộc đủ lớn nên mô phỏng đơn giản quá chậm. Cả số lượng lỗ và số lượng truy vấn đều có thể đạt tới`10^5`. Một mô phỏng đơn giản của mọi truy vấn có thể đi qua gần như toàn bộ mảng mỗi lần. Trong trường hợp xấu nhất, hãy tưởng tượng mọi lỗ đều có giá trị`1`. Bắt đầu từ lỗ`1`yêu cầu`10^5`nhảy. Làm điều này cho`10^5`các truy vấn sẽ dẫn đến khoảng`10^10`hoạt động, vượt xa những gì phù hợp trong một giây. 

Hoạt động cập nhật là nguyên nhân khiến vấn đề trở nên phức tạp. Việc tính toán trước tất cả các câu trả lời một lần là không đủ vì việc thay đổi độ dài bước nhảy có thể ảnh hưởng đến nhiều đường dẫn. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét đầu vào này:```
3 1
3 1 1
1 1
```Bắt đầu từ lỗ`1`, bóng thoát ra ngay vì`1 + 3 = 4`. Câu trả lời đúng là:```
1 1
```Thực hiện bất cẩn có thể in ra lỗ hổng`4`là vị trí cuối cùng, mặc dù bài toán yêu cầu lỗ hợp lệ cuối cùng trước khi rời đi. 

Một trường hợp tinh vi khác xuất hiện khi một bản cập nhật thay đổi chỉ nhảy vào bên trong một vùng cục bộ.```
5 3
1 1 1 1 1
0 3 5
1 1
```Sau khi cập nhật, đường dẫn sẽ trở thành:`1 -> 2 -> 3 -> outside`Câu trả lời đúng là:```
3 3
```Nếu quá trình tiền xử lý không được xây dựng lại chính xác sau khi cập nhật, thông tin cũ từ chuỗi cũ có thể tồn tại và tạo ra đường dẫn dài hơn. 

Một lỗi phổ biến cuối cùng đến từ ranh giới khối.```
6 1
2 2 2 2 2 2
1 1
```Các bước nhảy là:`1 -> 3 -> 5 -> outside`Câu trả lời là:```
5 3
```Khi sử dụng phương pháp phân tách căn bậc hai, nhiều quá trình triển khai vô tình dừng lại ở chỉ mục cuối cùng của khối thay vì bước nhảy đầu tiên ra ngoài khối. Điều đó phá vỡ logic nén chuỗi. 

## Phương pháp tiếp cận 

Giải pháp brute-force mô phỏng trực tiếp chuyển động của quả bóng. 

Đối với một truy vấn bắt đầu từ lỗ`x`, liên tục nhảy tới`x + a[x]`cho đến khi vị trí rời khỏi mảng. Đếm số lần nhảy đã được thực hiện và ghi nhớ lỗ hợp lệ cuối cùng. Việc cập nhật không đáng kể vì chúng tôi chỉ sửa đổi một giá trị mảng. 

Điều này hoạt động vì biểu đồ không có tính tuần hoàn và mọi mô phỏng cuối cùng đều kết thúc. Vấn đề là thời gian chạy. Trong trường hợp xấu nhất, mỗi lần nhảy chỉ di chuyển sang phải một bước. Một truy vấn duy nhất sẽ có giá`O(n)`. Với`10^5`các truy vấn, tổng độ phức tạp sẽ trở thành`O(nm)`, đó là về`10^10`hoạt động. 

Quan sát quan trọng là các bước nhảy chỉ di chuyển về bên phải. Do đó, chúng ta có thể nhóm các chỉ mục thành các khối và nén tất cả các bước nhảy nằm trong cùng một khối. 

Giả sử chúng ta chia mảng thành các khối có kích thước khoảng`sqrt(n)`. Đối với mỗi chỉ số`i`, chúng tôi lưu trữ: 

1. Vị trí tiếp theo bên ngoài khối hiện tại có thể truy cập được từ`i`. 
2. Số lần nhảy cần thiết để đạt được điều đó. 
3. Đã ghé thăm lỗ cuối cùng trước khi rời khỏi khối. 

Giờ đây, truy vấn không còn thực hiện từng bước một nữa. Thay vào đó, nó nhảy từng khối một. Vì chỉ có khoảng`sqrt(n)`khối, mỗi truy vấn sẽ trở nên nhanh hơn nhiều. 

Cập nhật cũng có thể quản lý được. Thay đổi`a[i]`chỉ ảnh hưởng đến các đường dẫn bên trong cùng một khối vì tất cả các bước nhảy đều di chuyển sang phải. Chúng ta có thể xây dựng lại một khối trong`O(sqrt(n))`thời gian thay vì tính toán lại mọi thứ. 

Đây là cách phân rã căn bậc hai cổ điển với việc nén đường dẫn bên trong các khối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(nm)`|`O(1)`| Quá chậm | 
| Tối ưu |`O((n + m)\sqrt{n})`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia mảng thành các khối có kích thước xấp xỉ`sqrt(n)`. 

Nếu như`n = 10^5`, kích thước khối là khoảng`320`. Điều này mang lại khoảng`320`khối, giúp giữ cho cả truy vấn và cập nhật hiệu quả. 
2. Đối với mọi vị trí`i`, tính toán ba giá trị trợ giúp.`nxt[i]`lưu trữ vị trí đầu tiên đạt được bên ngoài khối hiện tại.`cnt[i]`lưu trữ số lần nhảy cần thiết để đạt được`nxt[i]`.`last[i]`lưu trữ lỗ hợp lệ cuối cùng được truy cập trước khi rời khỏi khối. 
3. Xử lý các chỉ số bên trong mỗi khối từ phải sang trái. 

Vì các bước nhảy luôn đi đến các chỉ số lớn hơn nên thông tin đích đã được tính toán khi xử lý`i`. 
4. Hãy để`j = i + a[i]`. 

Nếu như`j`rời khỏi mảng hoặc thuộc về khối khác: 

-`nxt[i] = j`-`cnt[i] = 1`-`last[i] = i`Nếu không thì:

-`nxt[i] = nxt[j]`-`cnt[i] = cnt[j] + 1`-`last[i] = last[j]`Điều này nén tất cả các bước nhảy bên trong khối thành một quá trình chuyển đổi. 
5. Để trả lời một truy vấn bắt đầu từ`x`, lặp đi lặp lại: 

- Thêm`cnt[x]`để trả lời. 
- Ghi`last[x]`là lỗ hợp lệ mới nhất. 
- Di chuyển đến`nxt[x]`. 

Tiếp tục cho đến khi`x > n`. 

Mỗi lần lặp sẽ bỏ qua toàn bộ khối bước nhảy. 
6. Để xử lý cập nhật tại vị trí`x`, thay đổi`a[x]`và chỉ xây dựng lại khối chứa`x`. 

Không có khối nào trước đó phụ thuộc vào khối này vì các bước nhảy di chuyển hoàn toàn về bên phải. 

### Tại sao nó hoạt động 

Đối với mỗi chỉ số`i`, bộ dữ liệu`(nxt[i], cnt[i], last[i])`tóm tắt toàn bộ chuỗi các bước nhảy bên trong khối hiện tại. 

Nếu bước nhảy tiếp theo đã rời khỏi khối thì bản tóm tắt sẽ có ngay lập tức. 

Ngược lại, đường đi từ`i`đầu tiên nhảy tới`j`bên trong cùng một khối và phần còn lại của đường dẫn chính xác là bản tóm tắt được tính toán trước của`j`. Vì các chỉ số được xử lý từ phải sang trái nên thông tin này đã chính xác. 

Trong quá trình truy vấn, liên tục theo dõi`nxt`tái tạo chính xác trình tự nhảy ban đầu nhưng được nén thành các chuyển tiếp khối. Số lần nhảy được giữ nguyên vì`cnt[i]`lưu trữ chính xác số lần nhảy bị bỏ qua. Lỗ thăm cuối cùng được giữ nguyên vì`last[i]`theo dõi vị trí hợp lệ cuối cùng trước khi vượt qua ranh giới khối hoặc rời khỏi mảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = [0] + list(map(int, input().split()))

    B = int(n ** 0.5) + 1

    nxt = [0] * (n + 1)
    cnt = [0] * (n + 1)
    last = [0] * (n + 1)

    def rebuild(block):
        l = block * B + 1
        r = min(n, (block + 1) * B)

        for i in range(r, l - 1, -1):
            j = i + a[i]

            if j > n or (j - 1) // B != block:
                nxt[i] = j
                cnt[i] = 1
                last[i] = i
            else:
                nxt[i] = nxt[j]
                cnt[i] = cnt[j] + 1
                last[i] = last[j]

    num_blocks = (n + B - 1) // B

    for b in range(num_blocks - 1, -1, -1):
        rebuild(b)

    out = []

    for _ in range(m):
        query = list(map(int, input().split()))

        if query[0] == 0:
            x, y = query[1], query[2]
            a[x] = y
            rebuild((x - 1) // B)
        else:
            x = query[1]

            total = 0
            ans_last = x

            while x <= n:
                total += cnt[x]
                ans_last = last[x]
                x = nxt[x]

            out.append(f"{ans_last} {total}")

    sys.stdout.write("\n".join(out))

solve()
```Mảng được lưu trữ bằng cách sử dụng chỉ mục dựa trên 1 vì câu lệnh vấn đề đương nhiên dựa trên 1. Điều này tránh việc điều chỉnh chỉ số lặp đi lặp lại trong quá trình nhảy. 

các`rebuild`hàm tính toán lại tất cả thông tin nén cho một khối. Việc xử lý các chỉ số từ phải sang trái là điều cần thiết. Khi xử lý`i`, điểm đến`j`luôn lớn hơn`i`, vì vậy thông tin nén của nó đã có sẵn. 

điều kiện```
if j > n or (j - 1) // B != block:
```kiểm tra xem bước nhảy tiếp theo có rời khỏi khối hiện tại hay không. các`(j - 1) // B`biểu thức chuyển đổi chỉ mục dựa trên 1 thành số khối dựa trên 0. Thiếu`-1`là một lỗi thường gặp. 

Trong các truy vấn, vòng lặp không mô phỏng từng bước nhảy riêng lẻ. Thay vào đó, mỗi lần lặp sẽ chuyển trực tiếp sang khối khác bằng cách sử dụng`nxt[x]`. Tổng số lần nhảy bị bỏ qua được tích lũy bằng cách sử dụng`cnt[x]`. 

Biến`ans_last`được cập nhật trước khi chuyển sang khối tiếp theo. Điều này đảm bảo nó luôn lưu trữ lỗ hợp lệ cuối cùng trước khi thoát khỏi mảng. 

Cập nhật chỉ xây dựng lại một khối. Việc tính toán lại toàn bộ cấu trúc sau mỗi lần cập nhật sẽ quá chậm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
8 5
1 1 1 1 1 2 8 2
1 1
0 1 3
1 1
0 3 4
1 2
```Giả sử kích thước khối là`3`. 

Giá trị nén ban đầu: 

| tôi | một [tôi] | nxt[i] | cnt[i] | cuối cùng[i] | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 4 | 3 | 3 | 
| 2 | 1 | 4 | 2 | 3 | 
| 3 | 1 | 4 | 1 | 3 | 
| 4 | 1 | 7 | 3 | 6 | 
| 5 | 1 | 7 | 2 | 6 | 
| 6 | 2 | 8 | 1 | 6 | 
| 7 | 8 | 15 | 1 | 7 | 
| 8 | 2 | 10 | 1 | 8 | 

Đang xử lý truy vấn`1 1`: 

| Hiện tại x | cnt[x] đã thêm | cuối cùng[x] | nxt[x] | 
| --- | --- | --- | --- | 
| 1 | 3 | 3 | 4 | 
| 4 | 3 | 6 | 7 | 
| 7 | 1 | 7 | 15 | 

Tổng số lần nhảy =`7`, lỗ cuối cùng =`7`. 

Sau khi cập nhật, thông tin nén chỉ thay đổi ở các khối bị ảnh hưởng. 

Dấu vết này cho thấy toàn bộ chuỗi bên trong các khối bị bỏ qua như thế nào trong một thao tác. 

### Ví dụ 2 

đầu vào:```
5 2
1 1 1 1 1
0 3 5
1 1
```Sau khi cập nhật, mảng trở thành: 

| tôi | một [tôi] | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 5 | 
| 4 | 1 | 
| 5 | 1 | 

Trình tự nhảy từ`1`là:`1 -> 2 -> 3 -> outside`Truy vấn dấu vết: 

| Hiện tại x | cnt[x] đã thêm | cuối cùng[x] | nxt[x] | 
| --- | --- | --- | --- | 
| 1 | 2 | 2 | 3 | 
| 3 | 1 | 3 | 8 | 

Trả lời:```
3 3
```Ví dụ này cho thấy tại sao việc xây dựng lại khối đã sửa đổi là cần thiết sau khi cập nhật. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O((n + m)\sqrt{n})`| Mỗi truy vấn và cập nhật chạm nhiều nhất`O(\sqrt{n})`khối | 
| Không gian |`O(n)`| Mảng`a`,`nxt`,`cnt`, Và`last`mỗi cửa hàng một giá trị cho mỗi lỗ | 

Với`n, m <= 10^5`, việc phân tích căn bậc hai là đủ nhanh. đại khái`320`các thao tác trên mỗi truy vấn hoặc cập nhật dễ dàng phù hợp với giới hạn thời gian trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        n, m = map(int, input().split())
        a = [0] + list(map(int, input().split()))

        B = int(n ** 0.5) + 1

        nxt = [0] * (n + 1)
        cnt = [0] * (n + 1)
        last = [0] * (n + 1)

        def rebuild(block):
            l = block * B + 1
            r = min(n, (block + 1) * B)

            for i in range(r, l - 1, -1):
                j = i + a[i]

                if j > n or (j - 1) // B != block:
                    nxt[i] = j
                    cnt[i] = 1
                    last[i] = i
                else:
                    nxt[i] = nxt[j]
                    cnt[i] = cnt[j] + 1
                    last[i] = last[j]

        num_blocks = (n + B - 1) // B

        for b in range(num_blocks - 1, -1, -1):
            rebuild(b)

        out = []

        for _ in range(m):
            q = list(map(int, input().split()))

            if q[0] == 0:
                x, y = q[1], q[2]
                a[x] = y
                rebuild((x - 1) // B)
            else:
                x = q[1]

                total = 0
                ans_last = x

                while x <= n:
                    total += cnt[x]
                    ans_last = last[x]
                    x = nxt[x]

                out.append(f"{ans_last} {total}")

        return "\n".join(out)

    return solve()

# provided sample
assert run(
"""8 5
1 1 1 1 1 2 8 2
1 1
0 1 3
1 1
0 3 4
1 2
"""
) == """8 7
8 5
7 3""", "sample 1"

# minimum size
assert run(
"""1 1
1
1 1
"""
) == "1 1", "single hole"

# all values equal
assert run(
"""5 1
1 1 1 1 1
1 1
"""
) == "5 5", "full linear chain"

# immediate exit
assert run(
"""3 1
3 1 1
1 1
"""
) == "1 1", "jump exits immediately"

# update changes path
assert run(
"""5 3
1 1 1 1 1
0 3 5
1 1
1 2
"""
) == """3 3
3 2""", "rebuild after update"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lỗ đơn |`1 1`| Hạn chế tối thiểu | 
| Tất cả những cái |`5 5`| Chuỗi dài nhất có thể | 
| Thoát ngay lập tức |`1 1`| Xử lý lỗ cuối cùng đúng cách | 
| Cập nhật đường dẫn thay đổi |`3 3`,`3 2`| Xây dựng lại khối thích hợp sau khi cập nhật | 

## Vỏ cạnh 

Hãy xem xét trường hợp bước nhảy đầu tiên thoát khỏi mảng.```
3 1
3 1 1
1 1
```Từ lỗ`1`, bước nhảy đi thẳng đến`4`, nằm ngoài mảng. Trong quá trình tiền xử lý: 

| tôi | j = tôi + a[i] | nxt[i] | cnt[i] | cuối cùng[i] | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | 4 | 1 | 1 | 

Vòng lặp truy vấn thực hiện chính xác một bước nhảy nén và báo cáo:```
1 1
```Thuật toán lưu trữ chính xác lỗ hợp lệ cuối cùng thay vì vị trí ngoài phạm vi. 

Bây giờ hãy xem xét một bản cập nhật làm thay đổi cấu trúc bên trong một khối.```
5 2
1 1 1 1 1
0 3 5
1 1
```Sau khi cập nhật`a[3] = 5`, xây dựng lại khối tính toán lại: 

| tôi | nxt[i] | cnt[i] | cuối cùng[i] | 
| --- | --- | --- | --- | 
| 3 | 8 | 1 | 3 | 
| 2 | 3 | 1 | 2 | 
| 1 | 3 | 2 | 2 | 

Truy vấn sau:`1 -> 3 -> outside`và trả về:```
3 3
```Chỉ khối đã sửa đổi mới được xây dựng lại, nhưng điều đó là đủ vì tất cả các cạnh đều hướng sang phải. 

Cuối cùng, hãy xem xét các bước nhảy tiếp đất chính xác trên ranh giới khối.```
6 1
2 2 2 2 2 2
1 1
```Giả sử kích thước khối là`2`. 

Con đường là:`1 -> 3 -> 5 -> outside`Quá trình tiền xử lý nhận ra rằng mỗi bước nhảy sẽ rời khỏi khối hiện tại ngay lập tức: 

| tôi | nxt[i] | cnt[i] | cuối cùng[i] | 
| --- | --- | --- | --- | 
| 1 | 3 | 1 | 1 | 
| 3 | 5 | 1 | 3 | 
| 5 | 7 | 1 | 5 | 

Truy vấn tích lũy ba bước nhảy nén và xuất ra chính xác:```
5 3
```Điều này xác nhận logic chuyển tiếp khối xử lý việc vượt qua ranh giới một cách chính xác.
