---
title: "CF 106157J - Sắp xếp Joust"
description: "Chúng ta được cung cấp một tập hợp các quy tắc sắp xếp giữa các chữ cái viết thường. Một quy tắc như a < b có nghĩa là mọi lần xuất hiện của a phải xuất hiện trước mọi lần xuất hiện của b trong từ được sắp xếp lại cuối cùng. Quy tắc a b có nghĩa tương tự như b < a. Đầu vào cũng chứa một từ."
date: "2026-06-25T11:19:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106157
codeforces_index: "J"
codeforces_contest_name: "2025 United Kingdom and Ireland Programming Contest (UKIEPC 2025)"
rating: 0
weight: 106157
solve_time_s: 37
verified: true
draft: false
---

[CF 106157J - Sắp xếp Joust](https://codeforces.com/problemset/problem/106157/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các quy tắc sắp xếp giữa các chữ cái viết thường. Một quy tắc như`a < b`có nghĩa là mỗi lần xuất hiện của`a`phải xuất hiện trước mỗi lần xuất hiện của`b`trong từ được sắp xếp lại cuối cùng. Một quy tắc`a > b`có nghĩa tương tự như`b < a`. 

Đầu vào cũng chứa một từ. Chúng ta có thể sắp xếp lại các chữ cái theo cách chúng ta muốn, nhưng sự sắp xếp cuối cùng phải đồng thời thỏa mãn mọi quy tắc sắp xếp. 

Nhiệm vụ là xuất ra bất kỳ sự sắp xếp lại hợp lệ nào của từ. Nếu các quy tắc mâu thuẫn với nhau, khiến cho không thể thỏa mãn tất cả chúng cùng một lúc, chúng ta phải in`IMPOSSIBLE`. 

Một quan sát hữu ích là chỉ có 26 chữ cái có thể. Mặc dù số lượng quy tắc có thể lên tới 700 và độ dài từ có thể lên tới 100.000, nhưng biểu đồ phụ thuộc thực tế chỉ được xây dựng trên 26 đỉnh. 

Độ dài từ lớn cho chúng ta biết rằng việc so sánh hoặc di chuyển nhiều lần các ký tự riêng lẻ sẽ gây lãng phí. Thay vào đó, các ràng buộc đề xuất tách vấn đề thành hai phần. Đầu tiên, xác định thứ tự toàn cầu hợp lệ của các chữ cái. Thứ hai, sử dụng tần số của mỗi chữ cái để xây dựng câu trả lời một cách hiệu quả. 

Một nguồn sai lầm phổ biến là việc giải quyết những mâu thuẫn gián tiếp. 

Coi như:```
3
a < b
b < c
c < a
abc
```Các quy tắc tạo thành một chu kỳ. Không có sự sắp xếp nào có thể đặt`a`trước`b`,`b`trước`c`, Và`c`trước`a`cùng một lúc. Đầu ra đúng là:```
IMPOSSIBLE
```Một trường hợp tế nhị khác là khi hai chữ cái không có mối quan hệ nào.```
1
b < n
banana
```Quy tắc chỉ hạn chế`b`Và`n`. lá thư`a`không liên quan đến cả hai. Bất kỳ sự sắp xếp nào mà tất cả`b`nhân vật đến trước tất cả`n`ký tự là hợp lệ. Nhiều câu trả lời tồn tại. 

Trường hợp cạnh thứ ba xảy ra khi từ không chứa một số chữ cái bị ràng buộc.```
2
x < y
y < z
banana
```Các ràng buộc vẫn tham gia vào việc phát hiện chu kỳ, nhưng các chữ cái bị thiếu sẽ không tạo ra bản sao nào cho đầu ra. Một trật tự tôpô hợp lệ vẫn được yêu cầu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử các hoán vị khác nhau của các chữ cái xuất hiện trong từ và kiểm tra xem mọi quy tắc có được thỏa mãn hay không. Điều này ngay lập tức không thể thực hiện được. Ngay cả khi chỉ có 10 chữ cái riêng biệt xuất hiện, vẫn có`10! = 3,628,800`các đơn đặt hàng có thể. 

Cấu trúc của bài toán hạn chế hơn nhiều so với bài toán sắp xếp lại tổng quát. Mọi quy tắc chỉ mô tả thứ tự tương đối giữa các loại chữ cái. Vị trí chính xác của từng bản sao không quan trọng. 

Điều này gợi ý việc biểu diễn mỗi chữ cái dưới dạng một đỉnh trong biểu đồ có hướng. Đối với một quy tắc`a < b`, chúng ta thêm một cạnh`a → b`. Mọi thỏa thuận cuối cùng hợp lệ đều phải đặt thư`a`trước chữ cái`b`, vì vậy mọi nghiệm hợp lệ đều tương ứng với thứ tự tôpô của đồ thị này. 

Nếu đồ thị chứa một chu trình thì không tồn tại thứ tự tôpô nào và không thể trả lời được. 

Nếu đồ thị không theo chu kỳ, thứ tự tôpô sẽ đưa ra thứ tự toàn cục hợp lệ gồm tất cả 26 chữ cái. Một khi đã biết thứ tự đó thì việc xây dựng từ rất dễ dàng. Đếm số lần mỗi chữ cái xuất hiện trong từ gốc, sau đó xuất ra tất cả các bản sao của mỗi chữ cái theo thứ tự tôpô. 

Đồ thị rất nhỏ, chỉ có 26 đỉnh, vì vậy việc sắp xếp tôpô về cơ bản là thời gian không đổi. Công việc chủ yếu là đếm ký tự và viết chuỗi đầu ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k!) | O(k) | Quá chậm | 
| Tối ưu | O(n + \|s\|) | O(n + 26) | Đã chấp nhận | 

Đây`n`là số quy tắc đặt hàng và`|s|`là độ dài từ. 

## Hướng dẫn thuật toán 

1. Tạo đồ thị có hướng có 26 đỉnh, mỗi đỉnh là một chữ cái viết thường. 
2. Đối với mọi quy tắc: 

- Nếu quy tắc là`a < b`, thêm cạnh`a → b`. 
- Nếu quy tắc là`a > b`, thêm cạnh`b → a`. 

Điều này mã hóa mối quan hệ ưu tiên cần thiết. 
3. Tính mức độ của mỗi đỉnh. 
4. Chạy sắp xếp tôpô của Kahn. 

- Bắt đầu với mọi đỉnh có bậc bằng 0. 
- Liên tục loại bỏ một đỉnh như vậy và nối nó vào thứ tự tôpô. 
- Giảm mức độ của các nước láng giềng hướng ngoại của nó. 
- Bất cứ khi nào hàng xóm đạt tới mức 0, hãy thêm nó vào hàng đợi. 
5. Nếu xử lý ít hơn 26 đỉnh thì đồ thị chứa một chu trình. đầu ra`IMPOSSIBLE`. 
6. Đếm tần số của mỗi chữ cái trong từ đã cho. 
7. Duyệt thứ tự tôpô đã tính toán. Đối với mỗi chữ cái, hãy thêm chữ cái đó chính xác số lần xuất hiện trong từ. 
8. Xuất chuỗi kết quả. 

### Tại sao nó hoạt động 

Mỗi cạnh trong biểu đồ thể hiện một ràng buộc thứ tự bắt buộc. Thuật toán của Kahn tạo ra một thứ tự trong đó mọi cạnh có hướng đều đi từ chữ cái trước đến chữ cái sau. 

Khi chúng ta xuất tất cả các bản sao của một chữ cái theo thứ tự tôpô này, mỗi lần xuất hiện của chữ cái nguồn sẽ xuất hiện trước mỗi lần xuất hiện của chữ cái đích ở mọi cạnh. Vì thứ tự tôpô thỏa mãn tất cả các cạnh nên nó cũng thỏa mãn mọi quy tắc sắp xếp. 

Nếu một chu trình tồn tại, một số tập hợp các chữ cái phải đứng trước một chữ cái khác trong cùng một chu trình. Không có sự sắp xếp tuyến tính nào có thể thỏa mãn yêu cầu đó, vì vậy`IMPOSSIBLE`là câu trả lời đúng 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

n = int(input())

adj = [[] for _ in range(26)]
indeg = [0] * 26
seen = set()

for _ in range(n):
    a, op, b = input().split()

    if op == "<":
        u = ord(a) - ord('a')
        v = ord(b) - ord('a')
    else:
        u = ord(b) - ord('a')
        v = ord(a) - ord('a')

    if (u, v) not in seen:
        seen.add((u, v))
        adj[u].append(v)
        indeg[v] += 1

s = input().strip()

q = deque(i for i in range(26) if indeg[i] == 0)

topo = []

while q:
    u = q.popleft()
    topo.append(u)

    for v in adj[u]:
        indeg[v] -= 1
        if indeg[v] == 0:
            q.append(v)

if len(topo) != 26:
    print("IMPOSSIBLE")
    sys.exit()

cnt = [0] * 26
for ch in s:
    cnt[ord(ch) - ord('a')] += 1

ans = []

for x in topo:
    ans.append(chr(x + ord('a')) * cnt[x])

print("".join(ans))
```Cấu trúc đồ thị chuyển đổi mọi so sánh thành một cạnh có hướng có hướng phù hợp với thứ tự yêu cầu. 

các`seen`set ngăn chặn các cạnh trùng lặp tăng độ một cách giả tạo. Ngay cả khi đầu vào đảm bảo các quy tắc riêng biệt, điều này làm cho việc triển khai trở nên mạnh mẽ. 

Thuật toán của Kahn tạo ra một thứ tự tôpô hợp lệ bất cứ khi nào có thứ tự đó. Việc kiểm tra chu kỳ được thực hiện bằng cách xác minh rằng tất cả 26 chữ cái đã được xử lý. 

Bước xây dựng cuối cùng sử dụng tần số thay vì sắp xếp toàn bộ chuỗi. Điều này rất quan trọng vì từ này có thể chứa tới 100.000 ký tự. Đang bổ sung`count(letter)`bản sao của mỗi ký tự trực tiếp là tuyến tính theo độ dài từ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
m > i
n < i
i > o
minion
```Các quy tắc trở thành: 

-`i < m`-`n < i`-`o < i`Tần số liên quan: 

| Thư | Đếm | 
| --- | --- | 
| tôi | 2 | 
| m | 1 | 
| n | 2 | 
| o | 1 | 

Một trật tự tôpô hợp lệ bắt đầu bằng: 

| Bước | Thư được chọn | Đặt hàng một phần | 
| --- | --- | --- | 
| 1 | n | n | 
| 2 | o | không | 
| 3 | tôi | không tôi | 
| 4 | m | không tôi là | 

Việc xây dựng từ từ tần số sẽ cho:```
noniim
```Điều này thỏa mãn`n < i`,`o < i`, Và`i < m`. 

### Ví dụ 2 

đầu vào:```
1
b < n
banana
```Tần số: 

| Thư | Đếm | 
| --- | --- | 
| một | 3 | 
| b | 1 | 
| n | 2 | 

Một trật tự tôpô hợp lệ có thể đặt: 

| Bước | Thư | 
| --- | --- | 
| 1 | một | 
| 2 | b | 
| 3 | n | 

Kết quả:```
aaabnn
```Điều kiện bắt buộc duy nhất là tất cả`b`nhân vật đến trước tất cả`n`nhân vật, được hài lòng. 

Ví dụ này chứng minh rằng các chữ cái không liên quan có thể xuất hiện ở bất kỳ đâu có liên quan đến nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + \|s\|) | Xây dựng đồ thị từ quy tắc cộng với đếm và xuất chữ | 
| Không gian | O(n + 26) | Đồ thị cạnh và mảng tần số | 

Vì đồ thị chỉ chứa 26 đỉnh nên việc sắp xếp tôpô có hiệu quả là thời gian không đổi. Thời gian chạy bị chi phối bởi việc xử lý các quy tắc đầu vào và các ký tự của từ, dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())

    adj = [[] for _ in range(26)]
    indeg = [0] * 26
    seen = set()

    for _ in range(n):
        a, op, b = input().split()

        if op == "<":
            u = ord(a) - 97
            v = ord(b) - 97
        else:
            u = ord(b) - 97
            v = ord(a) - 97

        if (u, v) not in seen:
            seen.add((u, v))
            adj[u].append(v)
            indeg[v] += 1

    s = input().strip()

    q = deque(i for i in range(26) if indeg[i] == 0)
    topo = []

    while q:
        u = q.popleft()
        topo.append(u)

        for v in adj[u]:
            indeg[v] -= 1
            if indeg[v] == 0:
                q.append(v)

    if len(topo) != 26:
        return "IMPOSSIBLE\n"

    cnt = [0] * 26
    for ch in s:
        cnt[ord(ch) - 97] += 1

    ans = []
    for x in topo:
        ans.append(chr(x + 97) * cnt[x])

    return "".join(ans) + "\n"

# custom cases

out = run("""0
a
""")
assert sorted(out.strip()) == ['a']

assert run("""3
a < b
b < c
c < a
abc
""") == "IMPOSSIBLE\n"

out = run("""1
b < n
banana
""")
assert out.strip().count('a') == 3
assert out.strip().index('b') < out.strip().rindex('n')

out = run("""2
x < y
y < z
banana
""")
assert sorted(out.strip()) == sorted("banana")

out = run("""1
a > b
abba
""")
s = out.strip()
assert s.rfind('b') < s.find('a')
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Từ một chữ cái | Bất kỳ câu trả lời một chữ cái hợp lệ nào | Kích thước tối thiểu | 
| Xe đạp`a<b<b<c<c<a`|`IMPOSSIBLE`| Phát hiện chu kỳ | 
|`b<n`với`banana`| Bất kỳ sự sắp xếp hợp lệ nào | Xử lý đơn hàng một phần | 
| Ràng buộc đối với các chữ cái vắng mặt | Sắp xếp lại hợp lệ | Thiếu chữ cái trong word | 
|`a>b`với`abba`| Tất cả`b`trước tất cả`a`| Hướng cạnh đúng | 

## Vỏ cạnh 

Hãy xem xét đầu vào theo chu kỳ:```
3
a < b
b < c
c < a
abc
```Đồ thị chứa các cạnh`a → b`,`b → c`, Và`c → a`. Mọi đỉnh đều có bậc dương. Thuật toán của Kahn không thể xử lý hết 26 chữ cái nên thứ tự tôpô không đầy đủ. Các đầu ra thuật toán:```
IMPOSSIBLE
```điều này đúng vì không tồn tại thứ tự hợp lệ nào. 

Hãy xem xét một đầu vào có các chữ cái không liên quan:```
1
b < n
banana
```Đồ thị chỉ chứa một cạnh. Việc sắp xếp tôpô có thể đặt`a`bất cứ nơi nào liên quan đến`b`Và`n`. Chuỗi được tạo ra có thể là`aaabnn`,`baaann`, hoặc nhiều người khác. Mỗi câu trả lời hợp lệ giữ tất cả`b`ký tự trước tất cả`n`nhân vật. 

Xem xét các ràng buộc liên quan đến các chữ cái vắng mặt:```
2
x < y
y < z
banana
```Trật tự tôpô vẫn bao gồm`x`,`y`, Và`z`, nhưng tần số của chúng bằng không. Trong quá trình xây dựng, không có bản sao nào được thêm vào cho những chữ cái đó. Thuật toán đưa ra sự sắp xếp lại của`banana`và tôn trọng chính xác tất cả các ràng buộc mặc dù những chữ cái đó không bao giờ xuất hiện trong từ.
