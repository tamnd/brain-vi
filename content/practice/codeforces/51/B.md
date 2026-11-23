---
title: "CF 51B - Phân tích bảng HTML"
description: "Chúng ta được cung cấp một chuỗi đại diện cho ngôn ngữ bảng HTML được đơn giản hóa. Ngôn ngữ chỉ chứa ba loại thẻ: <table>, <tr> và <td>, cùng với các thẻ đóng phù hợp của chúng."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "expression-parsing"]
categories: ["algorithms"]
codeforces_contest: 51
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 48"
rating: 1700
weight: 51
solve_time_s: 217
verified: true
draft: false
---
[CF 51B - Phân tích bảng bHTML](https://codeforces.com/problemset/problem/51/B) 

**Đánh giá:** 1700 
**Thẻ:** phân tích biểu thức 
**Thời gian giải:** 3 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi đại diện cho ngôn ngữ bảng HTML được đơn giản hóa. Ngôn ngữ chỉ chứa ba loại thẻ:`<table>`,`<tr>`, Và`<td>`, cùng với các thẻ đóng phù hợp của chúng. Một bảng chứa các hàng, mỗi hàng chứa các ô và một ô trống hoặc chứa một bảng hoàn chỉnh khác. 

Nhiệm vụ là tìm số lượng ô trực tiếp bên trong mỗi bảng xuất hiện trong tài liệu, bao gồm cả các bảng lồng nhau. Với mỗi bảng, chúng ta đếm có bao nhiêu`<td>...</td>`các phần tử trực tiếp thuộc về bảng đó, không thuộc về các bảng được lồng bên trong các ô của nó. Sau khi thu thập tất cả các số đếm, chúng ta in chúng theo thứ tự không giảm. 

Độ dài đầu vào tối đa là 5000 ký tự. Nó đủ nhỏ cho các thuật toán tuyến tính hoặc bậc hai, nhưng việc phân tích lại đệ quy các chuỗi con lớn vẫn có thể trở nên tốn kém nếu thực hiện bất cẩn. Trình phân tích cú pháp khối sẽ nguy hiểm vì các hoạt động 5000³ vượt xa giới hạn thời gian. Trình phân tích cú pháp một lượt rõ ràng có ngăn xếp dễ dàng phù hợp với các ràng buộc. 

Ngữ pháp đảm bảo rằng thông tin đầu vào là hợp lệ, vì vậy chúng tôi không bao giờ cần khôi phục từ các thẻ không đúng định dạng. Mỗi thẻ mở đều có một thẻ đóng phù hợp và việc lồng ghép luôn chính xác. 

Một phần tinh tế của vấn đề là phân biệt giữa các ô trực tiếp và các ô thuộc các bảng lồng nhau. 

Hãy xem xét đầu vào này:```
<table><tr><td><table><tr><td></td></tr></table></td></tr></table>
```Bảng bên ngoài có chính xác một ô, mặc dù một ô khác tồn tại bên trong bảng lồng nhau. Đầu ra đúng là:```
1 1
```Một bộ đếm ngây thơ chỉ đơn giản là đếm tất cả`<td>`các thẻ bên trong một phạm vi bảng sẽ báo cáo không chính xác`2`cho bảng bên ngoài. 

Một sai lầm dễ mắc phải khác là quên rằng bàn không nhất thiết phải có hình chữ nhật. 

Ví dụ:```
<table><tr><td></td><td></td></tr><tr><td></td></tr></table>
```Bảng này có tổng cộng ba ô, không phải bốn. Các hàng có thể có độ dài khác nhau. 

Cạm bẫy thứ ba là xử lý từng dòng đầu vào thay vì nối tất cả các dòng trước. Câu lệnh rõ ràng cho phép ngắt dòng tùy ý. 

Ví dụ:```
<table><tr>
<td></td>
</tr></table>
```Những dòng này cùng nhau tạo thành một chuỗi hợp lệ duy nhất. Xử lý các dòng một cách độc lập sẽ phá vỡ trình phân tích cú pháp. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là phân tích từng bảng một cách độc lập. Bất cứ khi nào chúng ta gặp phải một`<table>`, chúng tôi tìm kiếm sự phù hợp của nó`</table>`, trích xuất chuỗi con đó và đếm xem có bao nhiêu chuỗi trực tiếp`<td>`thẻ thuộc về nó. Để tránh đếm các ô của bảng lồng nhau, chúng ta cần một bộ đếm lồng nhau khác trong khi quét chuỗi con. 

Điều này hoạt động hợp lý, nhưng nó lặp lại công việc nhiều lần. Trong trường hợp xấu nhất, các bảng lồng nhau sâu buộc chúng ta phải quét lại gần như toàn bộ chuỗi còn lại cho mỗi bảng. Nếu độ sâu làm tổ là`n`, tổng công sẽ trở thành gần đúng:```
n + (n - 1) + (n - 2) + ...
```đó là bậc hai. 

Với chiều dài 5000, thậm chí`O(n²)`có thể thành công nhưng việc triển khai trở nên lộn xộn vì chúng tôi liên tục tìm kiếm các thẻ phù hợp và liên tục duyệt qua các khu vực giống nhau. 

Cấu trúc ngữ pháp cho chúng ta một ý tưởng rõ ràng hơn. Mỗi thẻ xuất hiện theo thứ tự được lồng đúng cách, giống hệt như dấu ngoặc đơn. Điều đó có nghĩa là chúng ta có thể xử lý chuỗi một lần từ trái sang phải bằng cách sử dụng ngăn xếp. 

Quan sát quan trọng là số lượng ô của bảng được biết chính xác khi chúng ta đóng bảng đó. Mỗi trực tiếp`<td>`gặp phải khi bảng đang hoạt động thuộc về nó. Các bảng lồng nhau không gây trở ngại vì chúng được xử lý ở các mức ngăn xếp sâu hơn. 

Vì vậy, thay vì phân tích lại các chuỗi con, chúng tôi duy trì một chồng các bảng hiện đang mở. Khi chúng tôi nhìn thấy`<table>`, chúng ta tạo một bộ đếm mới. Khi chúng tôi nhìn thấy`<td>`, chúng ta tăng bộ đếm của bảng hiện tại. Khi chúng tôi nhìn thấy`</table>`, chúng tôi hoàn thiện bảng đó và lưu trữ số lượng của nó. 

Điều này biến vấn đề thành một lần quét tuyến tính đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Được chấp nhận nhưng không hiệu quả | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các dòng đầu vào và nối chúng thành một chuỗi. 

Ngữ pháp bỏ qua ranh giới dòng, do đó trình phân tích cú pháp phải hoạt động trên chuỗi kết hợp. 
2. Quét chuỗi từ trái sang phải. 

Mỗi mã thông báo có ý nghĩa là một thẻ hoàn chỉnh, chẳng hạn như`<table>`hoặc`</td>`. Vì bộ thẻ rất nhỏ và cố định nên chúng tôi có thể xác định các thẻ bằng cách sử dụng kiểm tra tiền tố chuỗi. 
3. Duy trì một ngăn xếp trong đó mỗi phần tử lưu trữ số lượng ô trực tiếp hiện tại của một bảng đang mở. 

Đỉnh của ngăn xếp luôn đại diện cho bảng mà chúng ta hiện đang ở bên trong. 
4. Khi gặp phải`<table>`, xô`0`lên ngăn xếp. 

Một bảng mới bắt đầu với 0 ô được tính cho đến nay. 
5. Khi gặp phải`<td>`, tăng phần tử trên cùng của ngăn xếp. 

Mọi`<td>`thuộc trực tiếp vào bảng hiện đang hoạt động. Ngay cả khi ô này sau đó chứa một bảng khác thì nó vẫn là một ô trực tiếp của bảng hiện tại. 
6. Bỏ qua`<tr>`,`</tr>`, Và`</td>`. 

Chúng ảnh hưởng đến tính hợp lệ của cấu trúc nhưng không làm thay đổi số lượng tế bào. 
7. Khi gặp phải`</table>`, lấy giá trị trên cùng từ ngăn xếp và thêm nó vào danh sách câu trả lời. 

Tại thời điểm này, bảng đã được xử lý hoàn toàn nên số ô trực tiếp của bảng là cuối cùng. 
8. Sau khi quét xong, sắp xếp số lượng đã thu thập. 
9. In các số đếm cách nhau bằng dấu cách. 

### Tại sao nó hoạt động 

Tại bất kỳ vị trí nào trong quá trình quét, ngăn xếp thể hiện chính xác chuỗi các bảng hiện đang mở từ ngoài cùng đến trong cùng. Mọi`<td>`thẻ thuộc về bảng mở trong cùng vì ngữ pháp chỉ cho phép các ô bên trong các hàng của bảng hiện tại. Các bảng lồng nhau tạo ra các cấp độ ngăn xếp mới, do đó các ô của chúng không bao giờ ảnh hưởng đến bộ đếm chính. Khi`</table>`xuất hiện, tất cả nội dung của bảng đó đã được xử lý, nghĩa là bộ đếm được lưu trữ đã đầy đủ và chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = "".join(line.strip() for line in sys.stdin)

    stack = []
    ans = []

    i = 0
    n = len(s)

    while i < n:
        if s.startswith("<table>", i):
            stack.append(0)
            i += 7

        elif s.startswith("</table>", i):
            ans.append(stack.pop())
            i += 8

        elif s.startswith("<td>", i):
            stack[-1] += 1
            i += 4

        elif s.startswith("</td>", i):
            i += 5

        elif s.startswith("<tr>", i):
            i += 4

        elif s.startswith("</tr>", i):
            i += 5

    ans.sort()
    print(*ans)

if __name__ == "__main__":
    solve()
```Bước đầu tiên nối tất cả các dòng đầu vào thành một chuỗi liên tục. sử dụng`strip()`loại bỏ các ký tự dòng mới trong khi vẫn giữ được đánh dấu thực tế. 

Trình phân tích cú pháp hoạt động với một chỉ mục duy nhất`i`. Vì mọi mã thông báo hợp lệ đều có độ dài cố định nên chúng tôi có thể nâng cao con trỏ ngay sau khi khớp thẻ. 

Ngăn xếp lưu trữ một số nguyên trên mỗi bảng đang mở. Khi một bảng mới bắt đầu, chúng tôi đẩy`0`. Mọi`<td>`chỉ tăng phần tử trên cùng vì quyền sở hữu trực tiếp luôn thuộc về bảng hiện tại. 

Thứ tự của các điều kiện hơi quan trọng vì`<table>`Và`</table>`chia sẻ tiền tố. Việc kiểm tra các thẻ mở trước khi đóng các thẻ giúp cho việc so khớp trở nên rõ ràng. 

Chúng tôi bỏ qua hoàn toàn các thẻ hàng vì chúng không ảnh hưởng đến câu trả lời được yêu cầu. Vai trò duy nhất của họ là thực thi tính hợp lệ của ngữ pháp, điều mà tuyên bố đã đảm bảo. 

Khi một bảng đóng lại, bộ đếm của nó đã hoàn tất, vì thế chúng ta đưa nó vào và lưu nó vào`ans`. 

Cuối cùng, việc sắp xếp tạo ra thứ tự không giảm cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
<table><tr><td></td></tr></table>
```Dấu vết: 

| Vị trí | Gắn thẻ | Xếp chồng trước | Hành động | Xếp chồng sau | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 |`<table>`|`[]`| đẩy 0 |`[0]`|`[]`| 
| 7 |`<tr>`|`[0]`| bỏ qua |`[0]`|`[]`| 
| 11 |`<td>`|`[0]`| tăng đầu |`[1]`|`[]`| 
| 15 |`</td>`|`[1]`| bỏ qua |`[1]`|`[]`| 
| 20 |`</tr>`|`[1]`| bỏ qua |`[1]`|`[]`| 
| 25 |`</table>`|`[1]`| bật lên để trả lời |`[]`|`[1]`| 

Kết quả được sắp xếp:```
1
```Ví dụ này xác nhận tính bất biến cơ bản mà mọi`<td>`tăng bảng hiện đang mở chính xác một lần. 

### Ví dụ 2 

đầu vào:```
<table><tr><td><table><tr><td></td><td></td></tr></table></td></tr></table>
```Dấu vết: 

| Vị trí | Gắn thẻ | Xếp chồng trước | Hành động | Xếp chồng sau | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 |`<table>`|`[]`| đẩy 0 |`[0]`|`[]`| 
| 11 |`<td>`|`[0]`| tăng đầu |`[1]`|`[]`| 
| 15 |`<table>`|`[1]`| đẩy 0 |`[1, 0]`|`[]`| 
| 26 |`<td>`|`[1, 0]`| tăng đầu |`[1, 1]`|`[]`| 
| 35 |`<td>`|`[1, 1]`| tăng đầu |`[1, 2]`|`[]`| 
| 53 |`</table>`|`[1, 2]`| bật lên để trả lời |`[1]`|`[2]`| 
| 72 |`</table>`|`[1]`| bật lên để trả lời |`[]`|`[2, 1]`| 

Kết quả được sắp xếp:```
1 2
```Dấu vết này cho thấy tại sao các bảng lồng nhau lại cần có ngăn xếp. Các ô của bảng bên trong chỉ ảnh hưởng đến bộ đếm bên trong. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi ký tự được xử lý một lần thông qua kiểm tra thẻ có độ dài cố định | 
| Không gian | O(n) | độ sâu ngăn xếp có thể đạt đến độ sâu lồng nhau của bảng | 

Độ dài đầu vào tối đa là 5000, do đó quá trình quét tuyến tính cực kỳ nhanh. Việc sử dụng bộ nhớ cũng rất nhỏ vì ngay cả việc lồng tối đa cũng chỉ yêu cầu một số nguyên cho mỗi bảng mở. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    s = "".join(line.strip() for line in sys.stdin)

    stack = []
    ans = []

    i = 0
    n = len(s)

    while i < n:
        if s.startswith("<table>", i):
            stack.append(0)
            i += 7

        elif s.startswith("</table>", i):
            ans.append(stack.pop())
            i += 8

        elif s.startswith("<td>", i):
            stack[-1] += 1
            i += 4

        elif s.startswith("</td>", i):
            i += 5

        elif s.startswith("<tr>", i):
            i += 4

        elif s.startswith("</tr>", i):
            i += 5

    ans.sort()
    print(*ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue().strip()

# provided sample
assert run("<table><tr><td></td></tr></table>\n") == "1", "sample 1"

# nested table
assert run(
    "<table><tr><td><table><tr><td></td></tr></table></td></tr></table>\n"
) == "1 1", "nested table"

# non-rectangular rows
assert run(
    "<table><tr><td></td><td></td></tr><tr><td></td></tr></table>\n"
) == "3", "different row sizes"

# multiple nesting levels
assert run(
    "<table><tr><td><table><tr><td><table><tr><td></td></tr></table></td></tr></table></td></tr></table>\n"
) == "1 1 1", "deep nesting"

# multiline input
assert run(
    "<table><tr>\n<td></td>\n</tr></table>\n"
) == "1", "line concatenation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bảng ô trống đơn |`1`| cấu trúc hợp lệ tối thiểu | 
| Bảng lồng nhau bên trong một ô |`1 1`| số lượng lồng nhau vẫn tách biệt | 
| Độ dài hàng không đồng đều |`3`| bàn không phải hình chữ nhật | 
| Lồng đệ quy sâu |`1 1 1`| xử lý ngăn xếp đệ quy | 
| Phân chia đầu vào theo dòng |`1`| nối các dòng đúng | 

## Vỏ cạnh 

Hãy xem xét lại trường hợp bảng lồng nhau:```
<table><tr><td><table><tr><td></td></tr></table></td></tr></table>
```Việc thực thi bắt đầu với bộ đếm bảng bên ngoài`[0]`. đầu tiên`<td>`thay đổi nó thành`[1]`. Khi bảng bên trong bắt đầu, ngăn xếp sẽ trở thành`[1, 0]`. Bên trong`<td>`chỉ tăng mức cao nhất, tạo ra`[1, 1]`. Sau khi đóng bảng bên trong, danh sách câu trả lời chứa`[1]`, và bộ đếm bên ngoài không thay đổi. Đóng bảng bên ngoài thêm một bảng khác`1`. Đầu ra được sắp xếp cuối cùng là:```
1 1
```Điều này xác nhận rằng các ô lồng nhau không bao giờ bị rò rỉ vào số lượng ô gốc. 

Bây giờ hãy xem xét một bảng không phải hình chữ nhật:```
<table><tr><td></td><td></td></tr><tr><td></td></tr></table>
```Trình phân tích cú pháp gặp ba`<td>`các thẻ trong khi cùng một bảng đang hoạt động, do đó bộ đếm của nó sẽ phát triển thành:```
0 -> 1 -> 2 -> 3
```Hàng không quan trọng để đếm. Đầu ra cuối cùng là:```
3
```Điều này tránh được lỗi phổ biến khi cho rằng tất cả các hàng có chiều rộng bằng nhau. 

Cuối cùng, hãy xem xét đầu vào nhiều dòng:```
<table><tr>
<td></td>
</tr></table>
```Thuật toán đầu tiên nối tất cả các dòng bị tước thành:```
<table><tr><td></td></tr></table>
```Sau đó, quá trình phân tích cú pháp diễn ra bình thường và tạo ra:```
1
```Nếu không nối, trình phân tích cú pháp sẽ coi ranh giới dòng mới là cấu trúc có ý nghĩa một cách không chính xác.
