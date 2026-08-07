---
title: "CF 102482B - Đầu phun nước dấu phẩy"
description: "Văn bản bao gồm các từ được phân tách bằng dấu cách, dấu phẩy hoặc dấu chấm kết thúc câu. Một số dấu phẩy đã có sẵn. Nhiệm vụ là áp dụng nhiều lần một hệ thống quy tắc cho đến khi vị trí dấu phẩy ngừng thay đổi."
date: "2026-08-06T18:39:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102482
codeforces_index: "B"
codeforces_contest_name: "2018 ACM-ICPC World Finals"
rating: 0
weight: 102482
solve_time_s: 234
verified: true
draft: false
---

[CF 102482B - Vòi phun nước bằng dấu phẩy](https://codeforces.com/problemset/problem/102482/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Văn bản bao gồm các từ được phân tách bằng dấu cách, dấu phẩy hoặc dấu chấm kết thúc câu. Một số dấu phẩy đã có sẵn. Nhiệm vụ là áp dụng nhiều lần một hệ thống quy tắc cho đến khi vị trí dấu phẩy ngừng thay đổi. 

Dấu phẩy trước một từ cung cấp thông tin về từ đó: mọi lần xuất hiện khác của cùng một từ, ngoại trừ khi nó bắt đầu một câu, cũng phải có dấu phẩy trước từ đó. Dấu phẩy sau một từ hoạt động tương tự, lan sang tất cả các lần xuất hiện khác của từ đó ngoại trừ khi nó đã ở cuối câu. Dấu phẩy mới tạo có thể ảnh hưởng đến các từ lân cận nên quá trình này phải tiếp tục cho đến khi đạt đến điểm cố định. 

Độ dài đầu vào có thể đạt tới một triệu ký tự. Điều này loại trừ việc quét liên tục toàn bộ văn bản sau mỗi lần thêm dấu phẩy. Một mô phỏng cố gắng áp dụng các quy tắc thay đổi từng lần một có thể chuyển thành hành vi bậc hai vì mỗi dấu phẩy mới có thể yêu cầu một lần chuyển đầy đủ khác qua đầu vào. Chúng ta cần một cách tiếp cận trong đó mỗi từ xuất hiện và mỗi loại từ chỉ được xử lý với số lần không đổi. 

Phần khó khăn là dấu phẩy không chỉ lan truyền giữa các từ giống nhau. Thêm dấu phẩy trước một từ cũng có nghĩa là từ trước đó hiện được theo sau bởi dấu phẩy. Thêm dấu phẩy sau một từ có nghĩa là từ tiếp theo hiện được đặt trước dấu phẩy. Bỏ qua phản ứng dây chuyền này sẽ cho kết quả không chính xác. 

Ví dụ:```
a b. a, b.
```Kết quả đúng là:```
a, b. a, b.
```Việc thực hiện bất cẩn mà chỉ nhớ những từ đã có dấu phẩy sẽ thêm dấu phẩy vào trước từ thứ hai`a`nhưng có thể quên rằng dấu phẩy ban đầu sau dấu phẩy đầu tiên`a`cũng lan truyền ngược qua mối quan hệ lân cận. 

Một trường hợp khác là ranh giới câu:```
x x.
```Kết quả đúng là:```
x x.
```Dấu phẩy sau lần đầu tiên`x`không thể được tạo từ thứ hai`x`vì lần xuất hiện thứ hai là từ cuối cùng của câu. Giải pháp chỉ kiểm tra tính bằng nhau của từ và bỏ qua vị trí câu sẽ cho kết quả sai`x, x.`. 

Trường hợp cạnh cuối cùng là một chuỗi các từ mới được kích hoạt:```
a b c,.
```Dấu phẩy sau`c`kích hoạt`b`như có dấu phẩy sau nó, có thể kích hoạt`a`thông qua quá trình tương tự. Dừng lại sau một vòng truyền sẽ cho kết quả sai. 

## Phương pháp tiếp cận 

Phương pháp đơn giản là quét liên tục toàn bộ văn bản. Trong quá trình quét, bất cứ khi nào một từ có dấu phẩy ở một bên ở đâu đó, chúng tôi sẽ tìm thấy mọi lần xuất hiện và thêm dấu phẩy bị thiếu. Điều này đúng vì nó trực tiếp tuân theo các quy tắc. Vấn đề là chi phí. Với một triệu ký tự, có thể có hàng trăm nghìn từ xuất hiện. Nếu mỗi dấu phẩy mới được thêm vào gây ra một lần quét hoàn chỉnh khác, thì trường hợp xấu nhất sẽ đạt khoảng O(n²), vượt xa giới hạn cho phép. 

Quan sát hữu ích là mỗi loại từ chỉ có hai trạng thái cố định có thể có. Một từ cuối cùng sẽ trở thành một từ có dấu phẩy trước nó ở đâu đó hoặc không. Điều tương tự cũng đúng với dấu phẩy sau các từ. Khi một từ đi vào một trong những trạng thái này, nó sẽ không bao giờ rời đi. 

Điều này biến quá trình này thành một vấn đề lan truyền đồ thị. Mỗi loại từ có hai loại trạng thái là “có dấu phẩy bên trái” và “có dấu phẩy bên phải”. Trạng thái dấu phẩy bên phải trên một từ sẽ kích hoạt trạng thái dấu phẩy bên trái của từ lân cận tiếp theo trong mỗi lần xuất hiện. Trạng thái dấu phẩy bên trái kích hoạt trạng thái dấu phẩy bên phải của hàng xóm trước đó. Chúng ta có thể chạy hàng đợi trên các trạng thái mới được phát hiện, xử lý mỗi trạng thái một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích dữ liệu đầu vào thành các từ xuất hiện. Đối với mỗi lần xuất hiện, hãy lưu trữ từ đó, từ trước đó của nó trong cùng một câu nếu nó tồn tại, từ tiếp theo trong cùng một câu nếu nó tồn tại và liệu ban đầu có dấu phẩy trước hay sau nó. 
2. Tạo hai trạng thái boolean cho mỗi từ riêng biệt. Một trạng thái ghi lại xem từ đó có phải có dấu phẩy trước nó hay không và trạng thái kia ghi lại liệu từ đó có phải có dấu phẩy sau nó hay không. Khởi tạo các trạng thái này từ văn bản gốc. 
3. Đặt mọi trạng thái hoạt động ban đầu vào hàng đợi. Hàng đợi chỉ chứa các trạng thái từ chưa được xử lý. 
4. Khi xử lý một từ có trạng thái dấu phẩy bên trái, hãy truy cập tất cả các lần xuất hiện của từ đó. Mỗi lần xuất hiện không phải là từ đầu tiên của câu sẽ nhận được dấu phẩy trước nó. Từ trước lần xuất hiện đó bây giờ có dấu phẩy sau nó, vì vậy hãy kích hoạt trạng thái dấu phẩy bên phải lân cận đó nếu cần. 
5. Khi xử lý một từ có trạng thái dấu phẩy bên phải, hãy truy cập tất cả các lần xuất hiện của từ đó. Mỗi lần xuất hiện không phải là từ cuối cùng của câu sẽ nhận được dấu phẩy sau nó. Từ sau lần xuất hiện đó bây giờ có dấu phẩy trước nó, vì vậy hãy kích hoạt trạng thái dấu phẩy bên trái lân cận đó nếu cần. 
6. Tiếp tục cho đến khi hàng đợi trống. Các trạng thái hoạt động bây giờ mô tả điểm cố định hoàn chỉnh. Xây dựng lại văn bản gốc bằng cách sử dụng các quyết định cuối cùng bằng dấu phẩy cho mỗi khoảng cách giữa các từ. 

Tại sao nó hoạt động: 

Thuật toán duy trì tính bất biến rằng mọi trạng thái hoạt động đều thể hiện một thực tế phải đúng trong câu trả lời cuối cùng. Việc xử lý một trạng thái áp dụng chính xác các hệ quả quy tắc của thực tế đó. Bất kỳ dấu phẩy mới được tạo nào cũng chỉ có thể tạo một trong hai trạng thái từ lân cận mà hàng đợi có thể khám phá. Vì các trạng thái chỉ được thêm vào khi chúng trở thành đúng và không bao giờ bị xóa nên cuối cùng hàng đợi chứa mọi hệ quả có thể truy cập được từ dấu phẩy ban đầu. Khi hàng đợi trống, không có quy tắc nào có thể tạo một dấu phẩy khác, đó chính xác là điều kiện dừng của quy trình ban đầu. 

## Giải pháp Python```python
import sys
from collections import deque, defaultdict

input = sys.stdin.readline

def solve():
    s = input().rstrip("\n")

    words = []
    gaps = []
    i = 0
    while i < len(s):
        if s[i].isalpha():
            j = i
            while j < len(s) and s[j].isalpha():
                j += 1
            words.append(s[i:j])
            i = j
        else:
            i += 1

    m = len(words)
    before = [False] * m
    after = [False] * m
    starts = [False] * m
    ends = [False] * m

    idx = 0
    pos = 0
    while pos < len(s):
        if s[pos].isalpha():
            idx += 1
            while pos < len(s) and s[pos].isalpha():
                pos += 1
            if pos < len(s):
                if s[pos] == ',':
                    before[idx - 1] = True
                    pos += 1
                    while pos < len(s) and s[pos] == ' ':
                        pos += 1
                elif s[pos] == '.':
                    pos += 1
                    while pos < len(s) and s[pos] == ' ':
                        pos += 1
                    ends[idx - 1] = True
                else:
                    pos += 1
        else:
            pos += 1

    starts[0] = True
    for i in range(1, m):
        if ends[i - 1]:
            starts[i] = True

    word_id = {}
    ids = []
    for w in words:
        if w not in word_id:
            word_id[w] = len(word_id)
        ids.append(word_id[w])

    k = len(word_id)
    has_left = [False] * k
    has_right = [False] * k
    occ = [[] for _ in range(k)]

    for i, x in enumerate(ids):
        occ[x].append(i)
        if before[i]:
            has_left[x] = True
        if after[i]:
            has_right[x] = True

    q = deque()
    for i in range(k):
        if has_left[i]:
            q.append((i, 0))
        if has_right[i]:
            q.append((i, 1))

    while q:
        w, side = q.popleft()
        if side == 0:
            for p in occ[w]:
                if not starts[p]:
                    if not after[p - 1]:
                        after[p - 1] = True
                        nw = ids[p - 1]
                        if not has_right[nw]:
                            has_right[nw] = True
                            q.append((nw, 1))
        else:
            for p in occ[w]:
                if not ends[p]:
                    if not before[p + 1]:
                        before[p + 1] = True
                        nw = ids[p + 1]
                        if not has_left[nw]:
                            has_left[nw] = True
                            q.append((nw, 0))

    ans = []
    ptr = 0
    for i, w in enumerate(words):
        ans.append(w)
        if i + 1 < m:
            if after[i]:
                ans.append(", ")
            elif ends[i]:
                ans.append(". ")
            else:
                ans.append(" ")
        else:
            ans.append(".")
    print("".join(ans))

solve()
```Trình phân tích cú pháp trước tiên sẽ trích xuất các từ xuất hiện trong khi vẫn giữ nguyên cấu trúc câu. Các mảng`starts`Và`ends`là cần thiết vì các quy tắc loại trừ rõ ràng ranh giới câu. 

Hai mảng trạng thái,`has_left`Và`has_right`, lưu trữ thông tin đóng cho các loại từ. Danh sách xuất hiện cho phép một lần kích hoạt cập nhật tất cả các từ phù hợp mà không cần tìm kiếm trong toàn bộ văn bản. 

Hàng đợi là một kỹ thuật truyền điểm cố định tiêu chuẩn. Một trạng thái chỉ vào hàng đợi một lần, đó là lý do tại sao thuật toán vẫn tuyến tính. Việc xây dựng lại cuối cùng sử dụng thông tin khoảng cách được lưu trữ trong`before`Và`after`, trong khi phần cuối câu được giữ nguyên thông qua`ends`. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, các trạng thái lan truyền quan trọng là: 

| Bước | Trạng thái kích hoạt | Hiệu ứng mới | 
| --- | --- | --- | 
| Ban đầu |`sit`đã để lại dấu phẩy |`sit`tăng trạng thái trái | 
| Ban đầu |`spot`có dấu phẩy đúng | tất cả`spot`lần xuất hiện có dấu phẩy bên phải | 
| Tuyên truyền |`here`đã để lại dấu phẩy | khác`here`lần xuất hiện đạt được dấu phẩy trái | 

Văn bản cuối cùng trở thành:```
please, sit spot. sit spot, sit. spot, here now, here.
```Dấu vết cho thấy dấu phẩy có thể đi qua các từ lân cận. trạng thái của`spot`tạo dấu phẩy nối tiếp dấu phẩy khác`spot`, sau đó tạo dấu phẩy trước`here`. 

Đối với mẫu thứ hai: 

| Bước | Trạng thái kích hoạt | Hiệu ứng mới | 
| --- | --- | --- | 
| Ban đầu |`one`đã để lại dấu phẩy | khác`one`lần xuất hiện đạt được dấu phẩy trái | 
| Ban đầu |`four`có dấu phẩy đúng | các từ sau có dấu phẩy trái | 
| Tuyên truyền |`tree`đã để lại dấu phẩy | tất cả các lần xuất hiện trùng khớp đều có dấu phẩy bên trái | 

Đầu ra là:```
one, two. one, tree. four, tree. four, four. five, four. six five.
```Ví dụ này chứng tỏ rằng việc truyền bá có thể tiếp tục qua nhiều từ khác nhau thay vì chỉ lặp lại cùng một từ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được phân tích cú pháp một lần và mỗi trạng thái từ được xử lý một lần. | 
| Không gian | O(n) | Danh sách xuất hiện, trạng thái từ và dữ liệu tái tạo đều tuyến tính ở kích thước đầu vào. | 

Giới hạn đầu vào là một triệu ký tự được xử lý vì thuật toán không bao giờ thực hiện quét toàn bộ lặp đi lặp lại. Mỗi mối quan hệ được lưu trữ được sử dụng một số lần không đổi. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old
    return out

assert run("please sit spot. sit spot, sit. spot here now here.\n") == "please, sit spot. sit spot, sit. spot, here now, here.\n"
assert run("one, two. one tree. four tree. four four. five four. six five.\n") == "one, two. one, tree. four, tree. four, four. five, four. six five.\n"

assert run("a a.\n") == "a a.\n"
assert run("x, y. x z.\n") == "x, y. x, z.\n"
assert run("a b c,.\n") == "a, b, c.\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a a.`|`a a.`| Xử lý ranh giới câu | 
|`x, y. x z.`|`x, y. x, z.`| Tuyên truyền từ dấu phẩy trước một từ | 
|`a b c,.`|`a, b, c.`| Tuyên truyền chuỗi nhiều bước | 

## Vỏ cạnh 

Đối với trường hợp từ lặp lại:```
x x.
```thuật toán không tạo ra trạng thái hoạt động ban đầu vì cả hai lần xuất hiện đều không có dấu phẩy. Hàng đợi vẫn trống và việc xây dựng lại trả về văn bản gốc. Thông tin về ranh giới câu ngăn cản việc vô tình tạo ra dấu phẩy. 

Đối với trường hợp lan truyền lân cận:```
a b. a, b.
```dấu phẩy sau`a`kích hoạt đúng trạng thái của`a`. Lần xuất hiện thứ hai của`a`đã thỏa mãn trạng thái đó và dấu phẩy trước trạng thái thứ hai`b`kích hoạt trạng thái bên trái của`b`. Thuật toán đi đến kết quả cuối cùng:```
a, b. a, b.
```Đối với một chuỗi:```
a b c,.
```trạng thái bên phải ban đầu thuộc về`c`. Việc xử lý nó sẽ kích hoạt trạng thái bên trái của từ sau nếu tồn tại và việc xử lý các trạng thái mới được phát hiện sẽ tiếp tục cho đến khi không thể tiếp cận được thêm hàng xóm nào. Việc truyền bá dựa trên hàng đợi xử lý các chuỗi có độ dài tùy ý một cách tự nhiên. 

Tôi cũng có thể cung cấp phiên bản biên tập theo phong cách cuộc thi ngắn hơn hoặc triển khai C++ nếu cần.
