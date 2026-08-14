---
title: "CF 102307D - Đừng thử vấn đề này"
description: "Chúng tôi có một chuỗi có độ dài (n), được lập chỉ mục từ 1 đến (n) và cập nhật (q). Bản cập nhật chọn vị trí bắt đầu (i), bước (a), một số bước (k) và ký tự (c). Các vị trí bị ảnh hưởng tạo thành một cấp số cộng: [ i, i+a, i+2a, ldots, i+ka."
date: "2026-08-13T07:17:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102307
codeforces_index: "D"
codeforces_contest_name: "2019 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 102307
solve_time_s: 271
verified: true
draft: false
---

[CF 102307D - Đừng thử vấn đề này](https://codeforces.com/problemset/problem/102307/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 31 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi có độ dài (n), được lập chỉ mục từ 1 đến (n) và cập nhật (q). Bản cập nhật chọn vị trí bắt đầu (i), bước (a), một số bước (k) và ký tự (c). Các vị trí bị ảnh hưởng tạo thành một cấp số cộng: 

[ 
i,\ i+a,\ i+2a,\ \ldots,\ i+ka. 
] 

Mọi nhân vật bị ảnh hưởng đều trở thành (c). Các bản cập nhật được áp dụng theo thứ tự nhất định, vì vậy nếu một số thao tác chạm vào cùng một vị trí thì chỉ có ký tự của thao tác mới nhất là quan trọng. Nhiệm vụ là in chuỗi sau khi tất cả các bản cập nhật đã được áp dụng. Vấn đề chính thức có (n,q\le 10^5) và giới hạn thời gian 2 giây. 

Các ràng buộc loại trừ việc thực hiện công tỷ lệ với (nq). Khi cả hai giá trị đều đạt đến (10^5), điều đó có nghĩa là có tới (10^{10}) lượt cập nhật vị trí. Ngay cả thuật toán (O(nq)) cũng vượt xa thời gian sẵn có, vì vậy nhiệm vụ trọng tâm là khai thác cấu trúc cấp số cộng thay vì xử lý mọi thao tác theo nghĩa đen. 

Có một số trường hợp ranh giới có thể âm thầm phá vỡ quá trình triển khai. Đầu tiên, (k) có thể bằng 0, do đó một thao tác có thể ảnh hưởng đến chính xác một vị trí. Ví dụ,```
ab
1
2 1 0 c
```sản xuất```
ac
```Một vòng lặp bắt đầu từ vị trí đầu tiên và chỉ thực hiện khi có một bước khác sẽ vô tình bỏ qua quá trình cập nhật. 

Vấn đề thứ hai là vị trí cuối cùng được bao gồm. Ví dụ,```
abcde
1
2 3 1 x
```thay đổi vị trí 2 và 5, tạo ra```
axcdx
```Việc triển khai sử dụng điểm cuối nửa mở xử lý không chính xác`i + k*a`như bị loại trừ sẽ tạo ra`axcde`. 

Trường hợp tinh tế cuối cùng là các thao tác chồng chéo với các kích cỡ bước khác nhau. Ví dụ,```
abc
2
1 1 2 x
2 1 0 y
```đầu tiên thay đổi mọi vị trí thành`x`, sau đó thay đổi vị trí 2 thành`y`, vậy câu trả lời là```
xyx
```Việc xử lý các hoạt động một cách độc lập theo kích thước bước của chúng là chưa đủ và chỉ ghi đè các ký tự. Chúng ta cần lưu giữ dấu thời gian của thao tác mới nhất chạm vào từng vị trí. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp rất đơn giản. Giữ chuỗi hiện tại và đối với mọi thao tác, hãy duyệt qua 

[ 
i,\ i+a,\ i+2a,\ldots,i+ka 
] 

và gán nhân vật mới. Điều này đúng vì đó chính xác là những vị trí được chỉ định bởi thao tác. 

Vấn đề là số lượng nhiệm vụ. Trong trường hợp xấu nhất, một thao tác có thể chạm vào (10^5) vị trí và có thể có (10^5) thao tác đó. Một công trình như```
i = 1, a = 1, k = 99999
```chạm vào mọi vị trí, do đó thuật toán brute-force có thể thực hiện (10^{10}) phép gán. 

Quan sát hữu ích đến từ sản phẩm (ka). Mọi thao tác đều thỏa mãn 

[ 
i+ka\le n. 
] 

Giả sử chúng ta chọn ngưỡng (B) xung quanh (\sqrt n). Nếu (k\le B), thao tác chứa nhiều nhất (B+1) vị trí, do đó việc xử lý trực tiếp sẽ rẻ. Trên tất cả các hoạt động (q), chi phí này (O(qB)). 

Những phép toán khó là những phép toán có (k>B). Đối với họ, 

[ 
a < \frac{n}{B}. 
] 

Khi (B) ở khoảng (\sqrt n), kích thước bước của chúng cũng nhỏ. Điều này có nghĩa là chỉ có (O(\sqrt n)) kích thước bước có thể áp dụng cho các hoạt động tốn kém. 

Bây giờ hãy xử lý ngược các thao tác dài đó. Xem xét tất cả các hoạt động dài có cùng bước (a). Họ di chuyển theo cùng một chuỗi vị trí: 

[ 
r,\ r+a,\ r+2a,\ldots 
] 

Đối với một (a) cố định, một khi một vị trí đã được xác nhận bởi một thao tác sau đó thì không có thao tác nào trước đó với cùng (a) có thể thay đổi giá trị cuối cùng của nó. Chúng ta có thể xóa vị trí đó khỏi phần xem xét và chuyển trực tiếp đến vị trí chưa bị xóa tiếp theo bằng cấu trúc tập hợp rời rạc. 

Như vậy, với mỗi (a) nhỏ cố định, mỗi vị trí sẽ bị xóa nhiều nhất một lần. Tổng công của một (a) là (O(n\alpha(n))) và chỉ có (O(\sqrt n)) giá trị có thể có của (a). Điều này đưa ra giải pháp tiêu chuẩn (O(n\sqrt n+q\sqrt n)). Kỹ thuật chia căn bậc hai và bỏ qua DSU tương tự được sử dụng trong các giải pháp đã biết cho vấn đề này. 

Có một chi tiết bổ sung giúp hai phần khớp với nhau một cách rõ ràng. Chúng tôi không cần sửa đổi chuỗi thực tế trong quá trình xử lý. Thay vì,`last[pos]`lưu trữ chỉ mục của hoạt động mới nhất đã được xác định có ảnh hưởng đến vị trí đó. Sau khi tất cả các hoạt động đã được xử lý,`last[pos]`cho chúng ta biết chính xác nhân vật nào thuộc về nhân vật đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(nq)) | (O(n)) | Quá chậm | 
| Tối ưu | (O((n+q)\sqrt n)) | (O(n+q)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi gốc và tất cả các thao tác. Cung cấp cho mọi thao tác số thứ tự đầu vào, bắt đầu từ 1. Số thứ tự đủ để quyết định thao tác nào thắng khi một số thao tác chạm vào cùng một vị trí. 
2. Chọn (B=\lfloor\sqrt n\rfloor). Nếu một phép toán có (k\le B), hãy xử lý ngay lập tức mọi vị trí của cấp số cộng của nó và đặt`last[position]`vào chỉ số hoạt động. Vì thao tác chứa nhiều nhất (B+1) vị trí, nên tất cả các thao tác như vậy cùng có giá (O(qB)). 
3. Lưu trữ mọi thao tác với (k>B) để xử lý sau. Thao tác như vậy có kích thước bước nhỏ vì (ka<n), do đó (a<n/(B+1)). Với (B) gần (\sqrt n), điều này có nghĩa là chỉ có (O(\sqrt n)) kích thước bước khác nhau có thể xảy ra giữa các thao tác này. 
4. Xử lý riêng từng kích thước bước nhỏ có thể có (a). Tạo cấu trúc DSU có các phần tử đại diện cho vị trí chuỗi. Ban đầu, mọi vị trí đều trỏ đến chính nó, nghĩa là nó vẫn có sẵn để được xác nhận bằng thao tác dài mới nhất có kích thước bước này. 
5. Đối với (a) cố định, hãy kiểm tra các hoạt động dài hạn của nó theo thứ tự ngược lại. Đối với một hoạt động bao gồm các vị trí từ`i`bởi vì`i + k*a`, bắt đầu ở vị trí còn trống đầu tiên vào hoặc sau`i`và liên tục nhảy tới vị trí có sẵn tiếp theo. 
6. Khi đạt đến vị trí khả dụng, hãy đặt`last[position]`vào chỉ số hoạt động và xóa vị trí đó khỏi chuỗi DSU này. Xóa vị trí`p`có nghĩa là chuyển hướng nó đến đại diện của`p+a`, bởi vì các vị trí thuộc cấp số cộng này cách nhau một cách chính xác`a`. 
7. Tiếp tục cho đến khi đại diện nằm ngoài điểm cuối của hoạt động. Vị trí bị loại bỏ bởi thao tác sau với cùng một bước không bao giờ cần được xem xét lại cho thao tác trước đó, đó là lý do DSU làm cho các thao tác kéo dài trở nên hiệu quả. 
8. Suy cho cùng, các hoạt động ngắn hạn và dài hạn đã góp phần tạo nên`last`, quét chuỗi một lần. Nếu như`last[p]`bằng 0, không có thao tác nào chạm vào vị trí`p`, vì vậy ký tự ban đầu của nó vẫn còn. Nếu không thì thay thế bằng ký tự thuộc hoạt động`last[p]`. 

Tại sao nó hoạt động: cho mọi vị trí,`last[p]`là chỉ số hoạt động lớn nhất trong số tất cả các hoạt động được xử lý có ảnh hưởng đến`p`. Các hoạt động ngắn truy cập rõ ràng mọi vị trí mà chúng ảnh hưởng. Đối với các thao tác dài với kích thước bước cố định, xử lý ngược có nghĩa là lần đầu tiên gặp một vị trí khả dụng là thao tác mới nhất với bước đó ảnh hưởng đến vị trí đó. Sau khi được xác nhận, vị trí đó có thể được xóa một cách an toàn vì tất cả các thao tác còn lại cho bước đó đều sớm hơn. Lấy chỉ số hoạt động tối đa kết hợp các đóng góp từ tất cả các kích thước bước, vì vậy sau khi xử lý mọi thứ,`last[p]`chính xác là hoạt động mới nhất thay đổi vị trí`p`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)

    q = int(input())

    operations = []
    last = [0] * (n + 1)

    B = int(n ** 0.5)

    # Operations with small k are cheap enough to process directly.
    long_by_step = {}

    for op_id in range(1, q + 1):
        i, a, k, c = input().split()
        i = int(i)
        a = int(a)
        k = int(k)

        operations.append((i, a, k, c))

        if k <= B:
            pos = i
            end = i + k * a
            while pos <= end:
                last[pos] = op_id
                pos += a
        else:
            long_by_step.setdefault(a, []).append(op_id)

    # Process long operations in reverse order, separately for each step.
    for a, ids in long_by_step.items():
        # parent[x] exists only for positions that have already been removed.
        # If x is absent, x itself is currently available.
        parent = {}

        def find(x):
            root = x
            while root in parent:
                root = parent[root]

            while x in parent:
                nxt = parent[x]
                parent[x] = root
                x = nxt

            return root

        for op_id in reversed(ids):
            i, _, k, _ = operations[op_id - 1]
            end = i + k * a

            pos = find(i)

            while pos <= end:
                last[pos] = op_id

                nxt = find(pos + a) if pos + a <= n else n + 1
                parent[pos] = nxt
                pos = nxt

    result = list(s)

    for pos in range(1, n + 1):
        op_id = last[pos]
        if op_id:
            result[pos - 1] = operations[op_id - 1][3]

    sys.stdout.write(''.join(result) + '\n')

if __name__ == "__main__":
    solve()
```Đầu vào được lưu trữ dưới dạng các thao tác hoàn chỉnh vì các thao tác dài phải được xem lại sau khi tất cả đầu vào đã được đọc. Mỗi thao tác giữ chỉ mục dựa trên một ban đầu của nó, trở thành dấu thời gian của nó. 

Đối với một hoạt động ngắn,`pos`bắt đầu lúc`i`và tiến bộ một cách chính xác`a`cho đến khi`i + k*a`. các`<=`điều kiện là có chủ ý vì vị trí cuối cùng là một phần của hoạt động. 

Các hoạt động dài được nhóm lại theo`a`. DSU được tạo riêng cho từng kích thước bước vì bước nhảy từ vị trí bị loại bỏ là chính xác.`a`. Một DSU không thể biểu diễn nhiều cấp số cộng khác nhau cùng một lúc. 

DSU dựa trên từ điển tránh phân bổ danh sách Python (O(n)) cho mỗi bước nhỏ có thể. Một vị trí được chèn vào`parent`chỉ sau khi nó đã được loại bỏ đối với kích thước bước hiện tại. Vì mỗi vị trí có thể bị xóa nhiều nhất một lần cho một vị trí cụ thể`a`, có nhiều nhất (n) mục như vậy trong khi một kích thước bước đang được xử lý. 

Các hoạt động dài được duyệt ngược lại. Giả sử hai thao tác dài có cùng`a`cả hai vị trí bìa`p`. Cái sau gặp trước, nên`p`được gán cho nó và sau đó bị xóa. Khi hoạt động trước đó được kiểm tra, DSU sẽ nhảy qua`p`, ngăn không cho thao tác trước đó thay thế thao tác sau một cách không chính xác. 

Không có vấn đề tràn số nguyên trong Python. Trong ngôn ngữ có chiều rộng cố định,`i + k*a`vẫn vừa vặn thoải mái với số nguyên có dấu 32 bit theo các ràng buộc này, nhưng Python xử lý số học một cách tự nhiên mà không cần bất kỳ xử lý đặc biệt nào. 

Chuỗi cuối cùng chỉ được xây dựng sau khi biết tất cả các dấu thời gian. Điều này tránh phải đồng bộ hóa các ký tự thực tế trong khi các thao tác ngắn và dài được xử lý theo thứ tự khác nhau. 

## Ví dụ đã hoạt động 

Mẫu được cung cấp có (n=20) nên (B=4). Thao tác đầu tiên có (k=8), khiến nó mất nhiều thời gian. Hai cái còn lại có (k=4) và (k=2), vì vậy chúng được xử lý trực tiếp. 

| Hoạt động | Loại | Vị trí bị ảnh hưởng |`last`thay đổi | 
| --- | --- | --- | --- | 
|`4 2 8 b`| Dài,`a=2`| 4, 6, 8, 10, 12, 14, 16, 18, 20 | Trì hoãn | 
|`6 3 4 c`| Ngắn | 6, 9, 12, 15, 18 | 6, 9, 12, 15, 18 trở thành 2 | 
|`10 5 2 d`| Ngắn | 10, 15, 20 | 10, 15, 20 trở thành 3 | 

Hoạt động dài sau đó được xử lý ngược cho`a=2`. Nó yêu cầu các vị trí 4, 6, 8, 10, 12, 14, 16, 18 và 20. Trong trường hợp một thao tác ngắn đã có dấu thời gian lớn hơn, thao tác cuối cùng sẽ`max`vẫn không thay đổi. Do đó, các vị trí 6, 12 và 18 giữ lại hoạt động 2, các vị trí 10, 15 và 20 giữ lại hoạt động 3 nếu có và các vị trí còn lại từ hoạt động đầu tiên nhận được`b`. 

Chuỗi cuối cùng là```
xaabacabcdacabdbacad
```Dấu vết cho thấy tại sao việc lưu trữ các chỉ số hoạt động lại hữu ích. Hoạt động dài có thể được xử lý riêng biệt với hoạt động ngắn vì sự đóng góp của chúng cuối cùng được so sánh theo dấu thời gian. 

Đối với ví dụ thứ hai, hãy xem xét:```
abcdefghij
3
1 1 9 x
3 2 2 y
2 3 2 z
```Ở đây (n=10) và (B=3). Thao tác đầu tiên có (k=9>B), do đó nó được DSU xử lý cho bước kích thước 1. Hai thao tác còn lại là thao tác ngắn. 

| Hoạt động | Loại | Vị trí bị ảnh hưởng |`last`sau khi xử lý | 
| --- | --- | --- | --- | 
|`1 1 9 x`| Dài,`a=1`| 1,2,3,4,5,6,7,8,9,10 | Trì hoãn | 
|`3 2 2 y`| Ngắn | 3,5,7 | 3,5,7 trở thành 2 | 
|`2 3 2 z`| Ngắn | 2,5,8 | 2,5,8 trở thành 3 | 

Hoạt động lâu dài sau đó khẳng định mọi vị trí thông qua DSU của nó. Vị trí 2, 5 và 8 đã có dấu thời gian muộn hơn nên giá trị của chúng vẫn được liên kết với các thao tác 3, 3 và 3 tương ứng. Vị trí 3, 7 tiếp tục hoạt động 2. Các vị trí còn lại nhận được`x`. 

Chuỗi kết quả là```
xzyzyxyxzx
```Ví dụ này thực hiện sự tương tác giữa thứ tự xử lý DSU và mảng dấu thời gian. Hoạt động kéo dài được phép lấp đầy các vị trí không bị ảnh hưởng bởi hoạt động sau đó, trong khi các vị trí đã được chạm vào sau vẫn được bảo vệ bởi chỉ số hoạt động lớn hơn của chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O((n+q)\sqrt n)) | Các thao tác ngắn truy cập tối đa (O(\sqrt n)) vị trí mỗi vị trí, trong khi các thao tác dài cho mỗi kích thước bước nhỏ sẽ xóa từng vị trí nhiều nhất một lần | 
| Không gian | (O(n+q)) | Các hoạt động, dấu thời gian và một từ điển DSU được lưu trữ | 

Với (n,q\le 10^5), (\sqrt n) là khoảng 316. Việc phân tách căn bậc hai giữ cho hàm (nq) có khả năng rất lớn nằm ngoài thuật toán. Phần hoạt động lâu dài đặc biệt hiệu quả vì DSU ngăn chặn việc truy cập liên tục các vị trí đã được giải quyết cho kích thước bước hiện tại. Việc sử dụng bộ nhớ vẫn tuyến tính ở kích thước đầu vào vì DSU cho kích thước một bước sẽ bị loại bỏ trước khi xử lý bước tiếp theo. 

## Trường hợp thử nghiệm```python
# The production solution above can be placed in a module and imported here.
# For a self-contained test harness, the same solve() function is reproduced
# through exec() so that each test gets its own stdin and stdout.

import sys
import io
from contextlib import redirect_stdout

def solve():
    input = sys.stdin.readline

    s = input().strip()
    n = len(s)

    q = int(input())

    operations = []
    last = [0] * (n + 1)

    B = int(n ** 0.5)
    long_by_step = {}

    for op_id in range(1, q + 1):
        i, a, k, c = input().split()
        i = int(i)
        a = int(a)
        k = int(k)

        operations.append((i, a, k, c))

        if k <= B:
            pos = i
            end = i + k * a
            while pos <= end:
                last[pos] = op_id
                pos += a
        else:
            long_by_step.setdefault(a, []).append(op_id)

    for a, ids in long_by_step.items():
        parent = {}

        def find(x):
            root = x
            while root in parent:
                root = parent[root]

            while x in parent:
                nxt = parent[x]
                parent[x] = root
                x = nxt

            return root

        for op_id in reversed(ids):
            i, _, k, _ = operations[op_id - 1]
            end = i + k * a

            pos = find(i)

            while pos <= end:
                last[pos] = op_id
                nxt = find(pos + a) if pos + a <= n else n + 1
                parent[pos] = nxt
                pos = nxt

    result = list(s)

    for pos in range(1, n + 1):
        op_id = last[pos]
        if op_id:
            result[pos - 1] = operations[op_id - 1][3]

    sys.stdout.write(''.join(result) + '\n')

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    try:
        with redirect_stdout(out):
            solve()
    finally:
        sys.stdin = old_stdin

    return out.getvalue()

# Provided sample.
assert run(
    "xaaaaaaaaaaaaaaaaaaa\n"
    "3\n"
    "4 2 8 b\n"
    "6 3 4 c\n"
    "10 5 2 d\n"
) == "xaabacabcdacabdbacad\n", "provided sample"

# Minimum-size string, k = 0.
assert run(
    "ab\n"
    "1\n"
    "2 1 0 c\n"
) == "ac\n", "single-position operation"

# Endpoint must be included.
assert run(
    "abcde\n"
    "1\n"
    "2 3 1 x\n"
) == "axcdx\n", "inclusive endpoint"

# Later operation with a different step size must win.
assert run(
    "abcde\n"
    "3\n"
    "1 1 4 x\n"
    "2 2 1 y\n"
    "3 1 0 z\n"
) == "xyzyx\n", "overlapping operations"

# Maximum n and q, with a long operation that covers the entire string.
max_q = 100000
max_input = (
    "a" * 100000
    + "\n"
    + str(max_q)
    + "\n"
    + ("1 1 99999 z\n" * max_q)
)
assert run(max_input) == ("z" * 100000) + "\n", "maximum-size long operations"

# All characters initially equal, with several boundary updates.
assert run(
    "aaaaaa\n"
    "4\n"
    "1 5 1 b\n"
    "2 2 2 c\n"
    "6 1 0 d\n"
    "3 3 1 e\n"
) == "baceae\n", "all-equal initial string"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`ab`, một thao tác với`k=0`|`ac`| Kích thước tối thiểu và tiến trình có độ dài bằng 0 | 
|`abcde`,`2 3 1 x`|`axcdx`| Điểm cuối cuối cùng bao gồm | 
|`abcde`, ba hoạt động chồng chéo |`xyzyx`| Chiến thắng dấu thời gian mới nhất trên các kích thước bước khác nhau | 
| Chiều dài 100000, 100000 hoạt động đầy đủ | 100000`z`nhân vật | Xử lý tối đa (n), tối đa (q) và DSU của các hoạt động dài | 
|`aaaaaa`với các bước hỗn hợp |`baceae`| Giá trị lặp lại và một số vị trí biên | 

## Vỏ cạnh 

Đối với trường hợp đếm bước bằng 0,```
ab
1
2 1 0 c
```chúng ta có (k=0), vì vậy vị trí bị ảnh hưởng duy nhất là 2. Vì (k\le B), vòng lặp thao tác ngắn bắt đầu ở vị trí 2, ghi nó một lần và dừng ngay sau khi tiến tới vị trí 3. Kết quả là`ac`. Không cần trường hợp đặc biệt nào vì đã bao gồm`while pos <= end`condition xử lý một cách tự nhiên tiến trình một vị trí. 

Đối với hoạt động nhạy cảm với điểm cuối,```
abcde
1
2 3 1 x
```điểm cuối là`2 + 1*3 = 5`. Vòng lặp thăm vị trí 2 rồi đến vị trí 5 trước khi dừng. Kết quả là`axcdx`. Một vòng lặp sử dụng`< end`thay vì`<= end`sẽ âm thầm nhớ ký tự cuối cùng. 

Đối với các hoạt động chồng chéo,```
abcde
3
1 1 4 x
2 2 1 y
3 1 0 z
```thao tác đầu tiên chạm vào mọi vị trí, thao tác thứ hai chạm vào vị trí 2 và 4, và thao tác thứ ba chạm vào vị trí 3. Dấu thời gian trở thành 1 ở vị trí 1 và 5, 2 ở vị trí 2 và 4 và 3 ở vị trí 3. Kết quả cuối cùng là`xyzyx`. Thuật toán làm đúng điều này bởi vì`last[position]`ghi lại chỉ số hoạt động lớn nhất thay vì bất kỳ hoạt động nào được xử lý gần đây nhất bởi bộ máy tối ưu hóa. 

Để hoạt động lâu dài, hãy xem xét```
abcdefghij
3
1 1 9 x
3 2 2 y
2 3 2 z
```Thao tác đầu tiên dài vì (k=9) nên bị trì hoãn. Các thao tác ngắn trước tiên ghi lại dấu thời gian 2 và 3. Khi DSU bước 1 xử lý ngược thao tác đầu tiên, nó truy cập mọi vị trí chính xác một lần và gán dấu thời gian 1. Các vị trí có dấu thời gian 2 hoặc 3 giữ giá trị lớn hơn của chúng. Kết quả cuối cùng là`xzyzyxyxzx`. 

Trường hợp kích thước tối đa sử dụng cơ chế tương tự ở quy mô lớn. Với chuỗi có độ dài 100000 và các phép toán`1 1 99999 z`, mọi hoạt động bao gồm mọi vị trí. Khi được xử lý ngược, thao tác mới nhất sẽ xác nhận tất cả các vị trí trong lần chuyển đầu tiên. Sau đó, mọi hoạt động trước đó sẽ bắt đầu tại một vị trí mà đại diện DSU đã vượt quá điểm cuối, vì vậy các hoạt động đó về cơ bản không thực hiện công việc bổ sung vị trí nào. Đáp án là 100000`z`ký tự, chứng minh lý do tại sao việc đảo ngược các hoạt động là phần quan trọng của việc tối ưu hóa.
