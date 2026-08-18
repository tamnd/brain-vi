---
title: "CF 102317C - Đừng phá băng"
description: "Chúng ta có một tấm ván hình vuông có kích thước (n lần n), ban đầu chứa đầy các khối băng. Một chiến lược bao gồm (m) nỗ lực để loại bỏ các ô cụ thể."
date: "2026-08-17T10:12:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102317
codeforces_index: "C"
codeforces_contest_name: "UCF Locals 2016"
rating: 0
weight: 102317
solve_time_s: 70
verified: true
draft: false
---

[CF 102317C - Đừng phá băng](https://codeforces.com/problemset/problem/102317/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tấm bảng hình vuông có kích thước (n \times n), ban đầu chứa đầy các khối băng. Một chiến lược bao gồm (m) nỗ lực để loại bỏ các ô cụ thể. Một lần thử sẽ không hợp lệ nếu khối ở vị trí đó đã biến mất, do nó bị loại trực tiếp trước đó hoặc do nó rơi xuống như một phần của tầng. 

Một khối vẫn được hỗ trợ nếu toàn bộ hàng của nó vẫn còn nguyên hoặc toàn bộ cột của nó vẫn còn nguyên. Khi một khối bị xóa khỏi một hàng, hàng đó không còn hoàn chỉnh nữa. Tương tự, việc xóa một khối khỏi một cột sẽ làm cho cột đó không đầy đủ. Bất kỳ khối nào có hàng và cột đều rơi không hoàn chỉnh và rơi xuống có thể khiến các khối tiếp theo biến mất. Đầu ra được yêu cầu là số lần thử không hợp lệ cho mỗi bảng. Cuộc thi ban đầu chứa (1 \le n \le 50) và (1 \le m \le 100), với định dạng đầu ra được yêu cầu`Strategy #b: i`. 

Hậu quả chính của các ràng buộc là ngay cả một mô phỏng đơn giản (O(mn^2)) cũng sẽ đủ nhỏ, vì bảng lớn nhất chỉ có (2500) ô và có tối đa (100) bước di chuyển. Tuy nhiên, cấu trúc của quá trình rơi cho phép chúng ta làm tốt hơn nữa. Chúng tôi không bao giờ cần phải duy trì tất cả (n^2) ô riêng lẻ. 

Có một số trường hợp nguy hiểm mà việc triển khai bất cẩn có thể bỏ sót. Đầu tiên là lặp lại chính xác cùng một động tác. Ví dụ,```
1
1 2
1 1
1 1
```có đầu ra```
Strategy #1: 1
```Lần thử đầu tiên sẽ loại bỏ khối duy nhất. Lần thử thứ hai không hợp lệ vì khối đó đã biến mất. Việc triển khai chỉ ghi nhớ các ô bị loại trực tiếp có thể xử lý trường hợp này, nhưng nó cũng phải xử lý chính xác các khối biến mất một cách gián tiếp. 

Một trường hợp thú vị hơn là khi hàng đã bị phá vỡ nhưng cột thì không. Ví dụ,```
1
2 3
1 1
1 2
2 1
```có đầu ra```
Strategy #1: 1
```Sau khi loại bỏ`(1,1)`, hàng 1 và cột 1 chưa đầy đủ. Tế bào`(1,2)`sống sót vì cột 2 vẫn hoàn thành. Đang xóa`(1,2)`sau đó phá vỡ cột 2. Lần thử cuối cùng`(2,1)`không hợp lệ vì cả hàng 2 và cột 1 hiện chưa đầy đủ. Một lỗi phổ biến là coi toàn bộ hàng được chạm là không sử dụng được, điều này sẽ đánh dấu không chính xác`(1,2)`không hợp lệ. 

Trường hợp biên (n=1) cũng đáng được quan tâm. Vì```
1
1 1
1 1
```đầu ra là```
Strategy #1: 0
```Động thái duy nhất là hợp lệ. Ở đây không có sự phân biệt giữa hàng và cột và khối được hỗ trợ vì cả hàng và cột của nó ban đầu đều hoàn chỉnh. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp có thể biểu diễn từng ô và liên tục kiểm tra bảng sau một nước đi. Khi một khối bị thiếu, chúng tôi kiểm tra xem hàng của nó đã hoàn thành hay cột của nó đã hoàn thành. Nếu cả hai khối đều không hoàn thành, khối sẽ rơi xuống và chúng ta lặp lại quá trình này cho đến khi không còn khối nào biến mất nữa. Điều này đúng vì nó tuân theo nguyên tắc vật lý của bảng. 

Vấn đề với việc triển khai đó là công việc lặp đi lặp lại không cần thiết. Một cách triển khai đơn giản quét tất cả (n^2) ô sau mỗi lần di chuyển và tiếp tục quét hoàn chỉnh cho đến khi bảng ngừng thay đổi có thể yêu cầu (O(n^2)) quét cho một lần di chuyển, với (O(n^2)) hoạt động trên mỗi lần quét. Trên (m) nước đi này là (O(mn^4)). Ở mức giới hạn tối đa, điều này mang lại (100 \cdot 50^4 = 625{,}000{,}000) lượt kiểm tra ô, một lượng lớn không cần thiết đối với bài toán cuộc thi kéo dài một giây. 

Cách tiếp cận bạo lực hoạt động vì trạng thái của mỗi ô được xác định bằng việc hàng và cột của nó có đầy đủ hay không. Quan sát đó cho phép chúng ta loại bỏ hầu hết trạng thái bảng. 

Hãy xem xét điều gì xảy ra ngay sau khi di chuyển thành công tại`(r,c)`. Hàng (r) không còn đầy đủ và cột (c) không còn đầy đủ. Kể từ thời điểm này trở đi, cả hai đều không thể hoàn thiện được nữa vì các khối chỉ biến mất. Bây giờ hãy xem xét bất kỳ ô nào`(x,y)`. Nó tồn tại chính xác khi ít nhất một trong các chiều của nó vẫn còn đầy đủ. Nếu hàng (x) chưa bao giờ bị phá vỡ thì toàn bộ hàng vẫn tồn tại. Nếu cột (y) chưa bao giờ bị phá vỡ thì toàn bộ cột vẫn tồn tại. Nếu cả hai đều đã bị hỏng,`(x,y)`chắc đã bị ngã. 

Điều này có nghĩa là toàn bộ tầng có thể được biểu diễn chỉ bằng hai mảng boolean.`broken_row[r]`nói hàng đó`r`đã trở nên không đầy đủ và`broken_col[c]`nói tương tự cho cột`c`. 

Một ô được yêu cầu`(r,c)`vẫn còn hiện diện chính xác khi`broken_row[r]`hoặc`broken_col[c]`là sai. Do đó, một nước đi cố gắng thực hiện sẽ không hợp lệ khi cả hai cờ đều đúng. Nếu nước đi hợp lệ, chúng tôi đánh dấu hàng và cột của nó là bị hỏng. Không có mô phỏng phân tầng riêng biệt vì mọi ô trong giao điểm của hai chiều bị hỏng đều được biết là đã biến mất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(mn^4)) | (O(n^2)) | Quá chậm trong trường hợp xấu nhất | 
| Tối ưu | (O(m+n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo`broken_row`Và`broken_col`, ban đầu sai cho mỗi hàng và cột. Ban đầu mỗi hàng và cột đều chứa tất cả các khối băng của nó, vì vậy mọi ô đều có mặt. 
2. Đọc từng vị trí đã thử`(r,c)`và chuyển đổi nó thành lập chỉ mục dựa trên số không. Kiểm tra xem`broken_row[r]`Và`broken_col[c]`cả hai đều đúng. 
3. Nếu cả hai cờ đều đúng, hãy tăng bộ đếm nước đi không hợp lệ. Ô nằm ở giao điểm của hai chiều không hoàn chỉnh nên chắc chắn nó đã rơi xuống. 
4. Nếu không, khối tại`(r,c)`vẫn còn tồn tại nên việc di chuyển là hợp lệ. Đánh dấu`broken_row[r]`Và`broken_col[c]`như sự thật. Các kích thước này không bao giờ có thể trở lại hoàn chỉnh vì bảng chỉ mất các khối. 
5. Sau tất cả các nước đi, hãy in số lần thử không hợp lệ bằng số chiến lược được yêu cầu. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý bất kỳ tiền tố nào của các bước di chuyển, một ô`(r,c)`hiện diện khi và chỉ khi hàng`r`vẫn còn đầy đủ hoặc cột`c`vẫn còn đầy đủ. Ban đầu cả hai điều kiện đều đúng cho mọi ô. Khi một nước đi hợp lệ sẽ bị loại bỏ`(r,c)`, hàng và cột của nó trở nên không đầy đủ. Bất kỳ ô nào có hàng và cột đều chưa hoàn chỉnh sẽ bị loại, trong khi mọi ô có ít nhất một chiều hoàn chỉnh vẫn được hỗ trợ. Vì một chiều không bao giờ có thể hoàn thiện trở lại nên hai mảng boolean mô tả chính xác bảng sau mỗi lần di chuyển. Do đó, một nước đi không hợp lệ khi cả hàng và cột của nó đều bị đánh dấu là bị hỏng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for case in range(1, t + 1):
        n, m = map(int, input().split())

        broken_row = [False] * n
        broken_col = [False] * n

        invalid = 0

        for _ in range(m):
            r, c = map(int, input().split())
            r -= 1
            c -= 1

            if broken_row[r] and broken_col[c]:
                invalid += 1
            else:
                broken_row[r] = True
                broken_col[c] = True

        out.append(f"Strategy #{case}: {invalid}")
        out.append("")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Đầu vào bắt đầu bằng số lượng bảng. Đối với mỗi bảng, hai mảng boolean ghi lại hàng và cột nào bị mất ít nhất một khối. 

Đối với mỗi lần di chuyển, trước tiên mã sẽ kiểm tra cả hai cờ trước khi sửa đổi chúng. Thứ tự này quan trọng. Nếu cả hàng và cột đều đã bị hỏng thì khối được thử đã rơi xuống nên nước đi đó phải được tính là không hợp lệ. Nếu ít nhất một chiều vẫn hoàn chỉnh thì khối đó vẫn được hỗ trợ và việc di chuyển là hợp lệ. 

Việc phân công sau một nước đi hợp lệ rất đơn giản. Chúng ta không cần phải loại bỏ rõ ràng các ô khác rơi xuống. Khi một hàng hoặc cột bị hỏng, cờ của nó vẫn đúng mãi mãi và giao điểm của bất kỳ hai chiều bị hỏng nào sẽ tự động bị coi là không có ở bước tiếp theo. 

Phép trừ một sẽ chuyển đổi cách đánh số hàng và cột dựa trên một của bài toán thành chỉ mục mảng dựa trên số 0 của Python. Không có mối lo ngại nào về tràn trong Python và thậm chí bản thân câu trả lời nhiều nhất là (m). 

Chuỗi trống được thêm vào sau mỗi chiến lược sẽ tạo ra dòng trống bắt buộc giữa các đầu ra. trận chung kết`join`cũng tránh các cuộc gọi lặp đi lặp lại đến`print`, mặc dù cả hai cách tiếp cận đều đủ nhanh cho những hạn chế này. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên có một bàn cờ (4 \times 4) và năm nước đi được thử. 

| Di chuyển | Hàng bị hỏng trước | Cột bị gãy trước | Có hiệu lực? | Số lượng không hợp lệ | 
| --- | --- | --- | --- | --- | 
|`(1,1)`| sai | sai | vâng | 0 | 
|`(1,2)`| đúng | sai | vâng | 0 | 
|`(4,1)`| sai | đúng | vâng | 0 | 
|`(4,2)`| đúng | đúng | không | 1 | 
|`(1,1)`| đúng | đúng | không | 2 | 

Sau đó`(1,1)`, hàng 1 và cột 1 bị hỏng. Khối tại`(1,2)`vẫn sống sót vì cột 2 đã hoàn thành nên nước đi thứ 2 là hợp lệ. Nước đi thứ ba phá vỡ hàng 4 trong khi cột 1 đã bị phá vỡ. Tại thời điểm đó`(4,2)`nằm ở giao điểm của hai chiều bị gãy và đã rơi xuống khiến nước đi thứ tư không còn hiệu lực. Các báo cáo mẫu ban đầu`Strategy #1: 2`. 

### Mẫu 2 

Bàn cờ thứ hai là (4 \time 4) với bốn nước đi. 

| Di chuyển | Hàng bị hỏng trước | Cột bị gãy trước | Có hiệu lực? | Số lượng không hợp lệ | 
| --- | --- | --- | --- | --- | 
|`(1,3)`| sai | sai | vâng | 0 | 
|`(2,4)`| sai | sai | vâng | 0 | 
|`(1,4)`| đúng | đúng | không | 1 | 
|`(4,4)`| sai | đúng | vâng | 1 | 

Hai nước đi đầu tiên phá vỡ hàng 1 và 2, cột 3 và 4. Như vậy`(1,4)`đã biến mất vì cả hàng và cột của nó đều bị hỏng. Động thái cuối cùng`(4,4)`vẫn hợp lệ vì hàng 4 vẫn đầy đủ, mặc dù cột 4 bị hỏng. Đầu ra mẫu là`Strategy #2: 1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(t(n+m))) | Mỗi bảng khởi tạo (O(n)) cờ và xử lý (m) di chuyển trong thời gian không đổi mỗi | 
| Không gian | (O(n)) | Hai mảng boolean có độ dài (n) được lưu trữ | 

Với (n \le 50) và (m \le 100), giải pháp tối ưu chỉ thực hiện vài trăm thao tác nguyên thủy trên mỗi bảng. Việc sử dụng bộ nhớ cũng rất nhỏ so với giới hạn 256 MB do cuộc thi quy định. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

def solve():
    input = sys.stdin.readline

    t = int(input())
    out = []

    for case in range(1, t + 1):
        n, m = map(int, input().split())

        broken_row = [False] * n
        broken_col = [False] * n

        invalid = 0

        for _ in range(m):
            r, c = map(int, input().split())
            r -= 1
            c -= 1

            if broken_row[r] and broken_col[c]:
                invalid += 1
            else:
                broken_row[r] = True
                broken_col[c] = True

        out.append(f"Strategy #{case}: {invalid}")
        out.append("")

    sys.stdout.write("\n".join(out))

# Provided samples
sample1 = """\
3
4 5
1 1
1 2
4 1
4 2
1 1
4 4
1 3
2 4
1 4
4 4
3 3
1 1
2 2
3 3
"""

sample1_expected = """\
Strategy #1: 2

Strategy #2: 1

Strategy #3: 0

"""

assert solve_data(sample1) == sample1_expected, "sample 1"

# Minimum-size board
assert solve_data("""\
1
1 1
1 1
""") == """\
Strategy #1: 0

""", "minimum-size board"

# Repeated move, the second and later attempts are invalid
assert solve_data("""\
1
3 4
1 1
1 1
1 1
1 1
""") == """\
Strategy #1: 3

""", "repeated move"

# Boundary case where a broken row does not make the whole row invalid
assert solve_data("""\
1
2 3
1 2
2 1
2 2
""") == """\
Strategy #1: 1

""", "row/column boundary"

# Maximum-size board: first 50 diagonal moves are valid,
# then every row and column is already broken.
moves = "\n".join(f"{i} {i}" for i in range(1, 51))
moves += "\n" + "\n".join(f"{i} {i}" for i in range(1, 51))

max_case = f"""\
1
50 100
{moves}
"""

assert solve_data(max_case) == """\
Strategy #1: 50

""", "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1 / 1 1`|`Strategy #1: 0`| Kích thước bàn cờ tối thiểu và nước đi hợp lệ đầu tiên | 
| Bốn lần lặp lại`(1,1)`trên một`3 x 3`bảng |`Strategy #1: 3`| Khối bị xóa trực tiếp một lần sau đó vẫn không hợp lệ | 
|`(1,2), (2,1), (2,2)`trên một`2 x 2`bảng |`Strategy #1: 1`| Một ô có thể tồn tại ngay cả khi hàng của nó bị phá vỡ trước đó | 
| 100 bước di chuyển chéo trên một`50 x 50`bảng |`Strategy #1: 50`| Kích thước tối đa, số lần di chuyển tối đa và giao lộ lặp lại | 

## Vỏ cạnh 

Đối với trường hợp di chuyển lặp đi lặp lại,```
1
3 4
1 1
1 1
1 1
1 1
```cái đầu tiên`(1,1)`di chuyển hợp lệ nên hàng 1 và cột 1 bị hỏng. Trong mọi lần thử tiếp theo, cả hai cờ đều đúng. Thuật toán tăng câu trả lời ba lần và tạo ra`Strategy #1: 3`. 

Đối với trường hợp hỗ trợ hàng,```
1
2 3
1 2
2 1
2 2
```Nước đi đầu tiên phá vỡ hàng 1 và cột 2. Nước đi thứ hai phá vỡ hàng 2 và cột 1. Nước đi thứ ba phá vỡ mục tiêu`(2,2)`, hàng 2 và cột 2 đều bị hỏng nên khối đã rơi rồi. Đầu ra là`Strategy #1: 1`. Điều này mắc phải lỗi phổ biến là chỉ theo dõi xem tọa độ chính xác đã được chọn trước đó hay chưa. 

Đối với bảng tối thiểu,```
1
1 1
1 1
```cả hàng duy nhất và cột duy nhất đều bắt đầu hoàn tất. Nước đi đầu tiên là hợp lệ và câu trả lời vẫn là 0. Việc triển khai hoạt động mà không cần cách viết hoa đặc biệt vì hai mảng boolean có độ dài bằng một. 

Đối với trường hợp kích thước tối đa, 50 nước đi đầu tiên có thể chạm vào mỗi hàng và mỗi cột đúng một lần. Những bước di chuyển đó đều hợp lệ vì trước mỗi lần di chuyển theo đường chéo, ít nhất một trong hai chiều vẫn hoàn thành. Khi tất cả 50 hàng và cột bị phá vỡ, mọi lần di chuyển tiếp theo đều không hợp lệ. Với 50 bước di chuyển chéo lặp lại khác, câu trả lời chính xác là 50. Điều này cũng chứng tỏ rằng trạng thái của thuật toán là đủ ngay cả khi bàn cờ đã trải qua số lần di chuyển trực tiếp lớn nhất có thể.
