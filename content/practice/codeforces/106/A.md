---
title: "CF 106A - Trò Chơi Bài"
description: "Chúng tôi được trao bộ bài tẩy cho trò chơi Durak và hai lá bài. Nhiệm vụ là quyết định xem lá bài đầu tiên có thể đánh bại lá bài thứ hai theo luật chơi hay không. Mỗi thẻ có một cấp bậc và một chất. Thứ hạng được sắp xếp như sau: Một quân bài có thể đánh bại một quân bài khác trong đúng hai tình huống."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 106
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 82 (Div. 2)"
rating: 1000
weight: 106
solve_time_s: 97
verified: true
draft: false
---

[CF 106A - Trò chơi bài](https://codeforces.com/problemset/problem/106/A) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được trao bộ bài tẩy cho trò chơi Durak và hai lá bài. Nhiệm vụ là quyết định xem lá bài đầu tiên có thể đánh bại lá bài thứ hai theo luật chơi hay không. 

Mỗi thẻ có một cấp bậc và một chất. Các cấp bậc được sắp xếp như sau:```
6 < 7 < 8 < 9 < T < J < Q < K < A
```Một quân bài có thể đánh bại quân bài khác trong đúng hai trường hợp. 

Nếu cả hai quân bài có cùng chất thì quân bài nào có thứ hạng cao hơn sẽ thắng. 

Nếu các chất khác nhau thì quân bài đầu tiên chỉ thắng nếu nó thuộc về quân bài tẩy còn quân bài thứ hai thì không. 

Kích thước đầu vào rất nhỏ, chỉ có hai quân bài và một bộ quân át chủ bài. Không có thách thức hiệu suất ở đây. Bất kỳ việc triển khai liên tục nào đều có thể dễ dàng đủ nhanh trong giới hạn. 

Phần khó khăn là xử lý các quy tắc theo đúng thứ tự. Một số trường hợp trông giống nhau nhưng hành xử khác nhau. 

Một sai lầm dễ mắc phải là quên rằng quân bài tẩy sẽ đánh bại bất kỳ quân bài không phải quân bài tẩy nào, bất kể cấp bậc. 

Ví dụ:```
Trump = H
6H  AS
```Câu trả lời đúng là:```
YES
```Mặc dù Át mạnh hơn nhiều so với 6 theo thứ tự xếp hạng bình thường, quân bài trái tim vẫn là quân át chủ bài. 

Một sai lầm phổ biến khác là để cho quân bài có thứ hạng cao hơn của bộ khác không phải quân át chủ bài giành chiến thắng. 

Ví dụ:```
Trump = D
AS  KC
```Câu trả lời đúng là:```
NO
```Chất bài khác nhau và không có quân bài nào là quân bài tẩy nên quân bài đầu tiên không thể đánh bại quân bài thứ hai. 

Trường hợp cạnh thứ ba là khi cả hai quân bài đều là quân bài tẩy. Trong tình huống đó, việc so sánh thứ hạng lại trở nên quan trọng. 

Ví dụ:```
Trump = S
7S  9S
```Câu trả lời đúng là:```
NO
```Cả hai quân bài đều là át chủ bài nên chúng ta so sánh cấp bậc bình thường, 7 yếu hơn 9. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là mã hóa thủ công mọi mối quan hệ chiến thắng có thể có giữa các quân bài. Vì chỉ có 36 quân bài nên về mặt lý thuyết, chúng ta có thể tính toán trước quân bài nào đánh bại quân bài khác và sau đó thực hiện tra cứu. 

Cách làm đó có hiệu quả vì số lượng thẻ cực kỳ ít. Một bảng so sánh đầy đủ sẽ chứa tối đa:```
36 × 36 = 1296
```các mối quan hệ. 

Tuy nhiên, điều này là không cần thiết. Luật chơi đã mô tả một quá trình so sánh trực tiếp. Thay vì lưu trữ từng cặp có thể, chúng ta có thể đánh giá mối quan hệ giữa hai thẻ ngay lập tức. 

Nhận xét quan trọng là chỉ có ba sự thật quan trọng: 

Đầu tiên, liệu các bộ quần áo có bằng nhau hay không. 

Thứ hai, liệu quân bài đầu tiên có phải là quân bài tẩy hay không. 

Thứ ba, thứ tự tương đối của các cấp bậc. 

Khi chúng tôi tổ chức các quy tắc xung quanh các điều kiện đó, việc triển khai sẽ trở nên đơn giản. 

Nếu chất bằng nhau, chúng ta so sánh giá trị xếp hạng. 

Nếu chất bài khác nhau, quân bài đầu tiên chỉ thắng khi đó là quân bài tẩy còn quân bài thứ hai thì không. 

Mọi thứ khác đều thua. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Tiền xử lý O(36²) | O(36²) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc bộ đồ của con át chủ bài. 
2. Đọc hai thẻ. 
3. Tách từng thẻ thành các thành phần cấp bậc và chất của nó. 
4. Lưu trữ thứ tự xếp hạng trong một chuỗi hoặc ánh xạ:```
"6789TJQKA"
```Chỉ số bên trong chuỗi này thể hiện sức mạnh của thẻ. 

1. Kiểm tra xem cả hai lá bài có cùng chất hay không. 

Nếu có, hãy so sánh vị trí xếp hạng của họ. Lá bài đầu tiên chỉ đánh bại lá bài thứ hai nếu chỉ số xếp hạng của nó lớn hơn. 

1. Nếu chất bài khác nhau, hãy kiểm tra xem quân bài đầu tiên có phải là quân bài tẩy và quân bài thứ hai không phải là quân bài tẩy hay không. 

Nếu điều kiện đó được giữ, hãy in`"YES"`. 

1. Trong tất cả các trường hợp còn lại, hãy in`"NO"`. 

### Tại sao nó hoạt động 

Thuật toán phản ánh trực tiếp các quy tắc trò chơi. 

Khi các bộ phù hợp với nhau, chỉ có thứ hạng quan trọng, vì vậy việc so sánh các chỉ số xếp hạng sẽ đưa ra người chiến thắng chính xác. 

Khi chất bài khác nhau, cách duy nhất để đánh bại quân bài khác là sử dụng quân bài tẩy chống lại quân bài không phải quân bài tẩy. Thuật toán kiểm tra chính xác điều kiện đó. 

Không có tình huống chiến thắng nào khác tồn tại trong các quy tắc, vì vậy mọi đầu vào có thể đều được xử lý chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    trump = input().strip()
    first, second = input().split()

    rank_order = "6789TJQKA"

    r1, s1 = first[0], first[1]
    r2, s2 = second[0], second[1]

    if s1 == s2:
        if rank_order.index(r1) > rank_order.index(r2):
            print("YES")
        else:
            print("NO")
    else:
        if s1 == trump and s2 != trump:
            print("YES")
        else:
            print("NO")

solve()
```Giải pháp bắt đầu bằng cách đọc quân bài tẩy và hai quân bài. Mỗi lá bài được biểu thị bằng chính xác hai ký tự, vì vậy việc trích xuất thứ hạng và chất chỉ đơn giản là lập chỉ mục vào chuỗi. 

Chuỗi`"6789TJQKA"`xác định thứ bậc cấp bậc. Chỉ số lớn hơn có nghĩa là thẻ mạnh hơn. Điều này tránh được những chuỗi điều kiện dài và giữ cho logic so sánh được gọn gàng. 

Nhánh chính đầu tiên kiểm tra xem các bộ đồ có khớp hay không. Nếu đúng như vậy thì luật chơi chỉ nói rằng thứ hạng chỉ quan trọng nên chúng tôi so sánh các chỉ số. 

Nếu các vụ kiện khác nhau, thứ hạng không còn quan trọng nữa trừ khi có sự tham gia của Trump. Mã kiểm tra xem quân bài đầu tiên có phải là quân bài tẩy hay không, còn quân bài thứ hai thì không. Đó là điều kiện chiến thắng của bộ đồ chéo duy nhất. 

Một điểm tinh tế là con át chủ bài yếu hơn vẫn đánh bại con át chủ bài mạnh hơn. Việc triển khai xử lý việc này một cách tự động vì việc so sánh thứ hạng bị bỏ qua khi các chất phù hợp khác nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
H
QH 9S
```| Biến | Giá trị | 
| --- | --- | 
| Trump | H | 
| đầu tiên | QH | 
| thứ hai | 9S | 
| s1 | H | 
| s2 | S | 

Các chất khác nhau nên việc so sánh thứ hạng bị bỏ qua. 

Lá bài đầu tiên thuộc về bộ bài tẩy, còn lá bài thứ hai thì không. 

Đầu ra:```
YES
```Ví dụ này thể hiện quy tắc át chủ bài ghi đè thứ tự xếp hạng bình thường. 

### Ví dụ 2 

đầu vào:```
S
7S 9S
```| Biến | Giá trị | 
| --- | --- | 
| Trump | S | 
| đầu tiên | 7S | 
| thứ hai | 9S | 
| s1 | S | 
| s2 | S | 
| xếp hạng(7) | 1 | 
| xếp hạng(9) | 3 | 

Các bộ đồ đều giống nhau nên chúng tôi so sánh cấp bậc trực tiếp. 

Từ`1 < 3`, quân bài đầu tiên yếu hơn. 

Đầu ra:```
NO
```Ví dụ này cho thấy rằng khi cả hai quân bài có cùng chất, ngay cả quân bài tẩy cũng tuân theo thứ tự xếp hạng bình thường. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số so sánh và tra cứu được thực hiện | 
| Không gian | O(1) | Thuật toán sử dụng một lượng bộ nhớ bổ sung không đổi | 

Giải pháp dễ dàng nằm trong giới hạn vì khối lượng công việc không bao giờ phụ thuộc vào kích thước đầu vào. Chỉ có hai thẻ được xử lý. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    trump = input().strip()
    first, second = input().split()

    rank_order = "6789TJQKA"

    r1, s1 = first[0], first[1]
    r2, s2 = second[0], second[1]

    if s1 == s2:
        if rank_order.index(r1) > rank_order.index(r2):
            print("YES")
        else:
            print("NO")
    else:
        if s1 == trump and s2 != trump:
            print("YES")
        else:
            print("NO")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup
    return out.getvalue()

# provided sample
assert run("H\nQH 9S\n") == "YES\n", "sample 1"

# same suit, higher rank wins
assert run("D\nAH KH\n") == "YES\n", "same suit higher rank"

# same suit, lower rank loses
assert run("C\n7S TS\n") == "NO\n", "same suit lower rank"

# trump beats stronger non-trump
assert run("H\n6H AS\n") == "YES\n", "trump overrides rank"

# different suits, no trump involved
assert run("D\nAS KC\n") == "NO\n", "different non-trump suits"

# both trump cards, compare rank normally
assert run("S\nJS QS\n") == "NO\n", "both trump cards"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`H / QH 9S`|`YES`| Thẻ Trump đánh bại không át chủ bài | 
|`D / AH KH`|`YES`| So sánh thứ hạng giống nhau | 
|`C / 7S TS`|`NO`| Thứ hạng thấp hơn thua trong cùng một bộ đồ | 
|`H / 6H AS`|`YES`| Trump yếu vẫn đánh bại không phải Trump mạnh | 
|`D / AS KC`|`NO`| Những bộ đồ không phải át chủ bài khác nhau không thể đánh bại nhau | 
|`S / JS QS`|`NO`| Trump vs Trump vẫn phụ thuộc vào cấp bậc | 

## Vỏ cạnh 

Hãy xem xét trường hợp một con át chủ bài yếu đánh bại một con át chủ bài mạnh. 

đầu vào:```
H
6H AS
```Đầu tiên, thuật toán sẽ thấy rằng các bộ đồ khác nhau. Nó không so sánh thứ hạng. Thay vào đó, nó kiểm tra tình trạng của con át chủ bài. Từ`6H`là Trump và`AS`không phải, câu trả lời sẽ trở thành`"YES"`. 

Bây giờ hãy xem xét hai vụ kiện khác nhau không phải là át chủ bài. 

đầu vào:```
D
AS KC
```Các chất khác nhau nên thuật toán sẽ kiểm tra xem quân bài đầu tiên có phải là quân bài tẩy hay không. Không phải vậy. Vì không áp dụng quy tắc chiến thắng nên kết quả là`"NO"`. 

Cuối cùng, hãy xem xét hai con át chủ bài. 

đầu vào:```
S
7S 9S
```Các chất đều bằng nhau nên thuật toán so sánh thứ hạng trực tiếp. chỉ số của`7`nhỏ hơn chỉ số của`9`, do đó lá bài đầu tiên bị mất và kết quả là`"NO"`. 

Những trường hợp này xác nhận rằng việc triển khai tách biệt chính xác logic giống nhau khỏi logic chính, vốn là chi tiết trọng tâm của vấn đề.
