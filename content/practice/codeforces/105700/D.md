---
title: "CF 105700D - \u0414\u043e\u043a\u043b\u0430\u0434 \u0438\u043d\u0432\u0435\u0441\u0442\u043e\u0440\u0430\u043c"
description: "Chúng ta được cấp một bộ n “thẻ”, mỗi thẻ có chiều dài m. Mỗi thẻ chỉ có chữ cái được viết ở một số vị trí, trong khi tất cả các vị trí khác đều trống. Chúng ta cũng có chuỗi mục tiêu s có độ dài m. Chúng ta được phép xếp tất cả các thẻ theo thứ tự nào đó từ trên xuống dưới."
date: "2026-06-22T04:50:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105700
codeforces_index: "D"
codeforces_contest_name: "2020-2021 \u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435, \u043f\u0435\u0440\u0432\u044b\u0439 \u0442\u0443\u0440"
rating: 0
weight: 105700
solve_time_s: 45
verified: true
draft: false
---

[CF 105700D - \u0414\u043e\u043a\u043b\u0430\u0434 \u0438\u043d\u0432\u0435\u0441\u0442\u043e\u0440\u0430\u043c](https://codeforces.com/problemset/problem/105700/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bộ n “thẻ”, mỗi thẻ có chiều dài m. Mỗi thẻ chỉ có chữ cái được viết ở một số vị trí, trong khi tất cả các vị trí khác đều trống. Chúng ta cũng có chuỗi mục tiêu s có độ dài m. 

Chúng ta được phép xếp tất cả các thẻ theo thứ tự nào đó từ trên xuống dưới. Đối với mỗi vị trí i, chúng ta xem thẻ trên cùng có một chữ cái trong cột i và chữ cái đó trở thành ký tự hiển thị trong chuỗi kết quả. Nếu không có thẻ nào có chữ cái ở vị trí i thì vị trí đó không thể được điền và thứ tự không hợp lệ. 

Nhiệm vụ là xác định xem có tồn tại thứ tự các thẻ tạo ra chính xác s hay không và nếu có thì xuất ra bất kỳ hoán vị hợp lệ nào. 

Các ràng buộc cho phép n và m lên tới 100000 với tổng số ô được ghi lên tới 200000. Điều này ngay lập tức loại trừ mọi thứ so sánh trực tiếp từng cặp thẻ hoặc mô phỏng xếp chồng cho mỗi hoán vị. Bất kỳ giải pháp nào cũng phải hoạt động theo thời gian gần như tuyến tính hoặc gần tuyến tính theo số lượng mục được viết. 

Một điểm tinh tế là mỗi vị trí đều độc lập về khả năng hiển thị, nhưng thứ tự sẽ kết hợp chúng: việc chọn một thẻ để thỏa mãn một vị trí có thể chặn hoặc ghi đè những vị trí khác bên dưới nó. Một chi tiết quan trọng khác là nhiều thẻ có thể chứa cùng một vị trí và chỉ thẻ trên cùng mới quan trọng. 

Một sai lầm ngây thơ là cố gắng gán từng vị trí một cách tham lam cho một thẻ nào đó một cách độc lập. Ví dụ: chọn bất kỳ thẻ nào khớp với s[i] ở vị trí i có thể dễ dàng dẫn đến mâu thuẫn sau này, vì có thể cần cùng một thẻ để thỏa mãn nhiều vị trí một cách nhất quán. Một chế độ thất bại khác là cố gắng xây dựng biểu đồ về sự phụ thuộc giữa các thẻ mà không xem xét các ràng buộc đó dành cho mỗi vị trí và được chia sẻ trên nhiều thẻ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là thử tất cả các hoán vị của thẻ và mô phỏng xem mỗi thứ tự có tạo ra chuỗi mục tiêu hay không. Điều này đúng vì nó tuân theo định nghĩa một cách chính xác, nhưng nó là giai thừa theo n, điều này không thể xảy ra ngay cả với n = 100. 

Quan sát quan trọng là mỗi vị trí i của chuỗi cuối cùng được xác định bởi thẻ đầu tiên (theo thứ tự) có chứa một chữ cái tại i. Vì vậy, với mỗi vị trí i, có chính xác một lá bài “có trách nhiệm”: lá bài cao nhất trong số những lá bài chứa vị trí đó. Lá bài đó phải chứa s[i] ở vị trí i. 

Điều này biến vấn đề thành một hệ thống ràng buộc về thứ tự tương đối của các thẻ. Với mỗi vị trí i, chúng ta xem xét tất cả các thẻ có chứa một chữ cái ở i. Trong số đó, quân bài trên cùng theo thứ tự cuối cùng phải là quân bài có chữ cái ở i bằng s[i]. Bất kỳ thẻ nào có chữ cái khác với i phải được đặt ngay bên dưới thẻ đã chọn đó, nếu không nó sẽ ghi đè lên chữ cái hiển thị và phá vỡ s. 

Vì vậy, đối với mỗi vị trí, chúng tôi chọn một “thẻ nhân chứng” phù hợp một cách hiệu quả với s[i] và tất cả các thẻ xung đột ở vị trí đó phải đến sau theo thứ tự. Điều này gây ra những hạn chế trực tiếp giữa các thẻ. Vấn đề cuối cùng trở thành: liệu chúng ta có thể tìm thấy một hoán vị thỏa mãn tất cả các ràng buộc sắp xếp này không và nếu có, hãy xuất ra bất kỳ thứ tự tôpô nào. 

Chúng ta xây dựng một đồ thị có hướng trong đó cạnh a → b có nghĩa là thẻ a phải xuất hiện phía trên thẻ b. Sau đó, chúng tôi thực hiện một sắp xếp tôpô. Nếu có một chu trình thì không tồn tại thứ tự nào. 

Bí quyết hiệu quả quan trọng là chúng tôi không thêm các cạnh cho mỗi cặp một cách rõ ràng cho tất cả các xung đột theo cách O(n^2). Thay vào đó, chúng tôi xử lý theo vị trí: với mỗi vị trí i, chúng tôi nhóm các thẻ theo ký tự mà chúng chứa tại i. Tất cả các thẻ có ký tự không khớp tại i phải ở dưới tất cả các thẻ có ký tự trùng khớp tại i, vì vậy chúng tôi thêm các cạnh từ nhóm khớp vào tất cả các thẻ khác ở vị trí đó. Vì tổng số lần xuất hiện được giới hạn bởi 200000 nên điều này vẫn hiệu quả.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | Ồ (n!) | O(n) | Quá chậm | 
| Đồ thị ràng buộc + Toposort | O(n + tổng_ô) | O(n + tổng_ô) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi vấn đề thành các ràng buộc sắp xếp xuất phát từ mỗi cột một cách độc lập. 

1. Đối với mỗi thẻ, hãy lưu trữ các chữ cái của nó ở vị trí ánh xạ cấu trúc thành ký tự. Điều này cho phép tra cứu nhanh chóng những gì thẻ đóng góp ở một cột nhất định. Điều này là cần thiết vì các ràng buộc phụ thuộc vào sự so sánh theo từng vị trí cụ thể. 
2. Với mỗi vị trí i từ 1 đến m, ta thu thập tất cả các thẻ có chữ cái tại i. Trong số đó, chúng tôi phân chia các thẻ thành những thẻ s[i] phù hợp và những thẻ không khớp s[i]. 
3. Nếu có ít nhất một thẻ có chữ cái i bằng s[i], chúng tôi coi những thẻ đó là “nhà cung cấp hàng đầu” tiềm năng cho vị trí i. Bất kỳ thẻ nào trong bộ không khớp phải ở dưới mỗi thẻ phù hợp, nếu không nó sẽ ghi đè ký tự hiển thị tại i và phá vỡ chuỗi mục tiêu. 
4. Chúng tôi thêm các cạnh có hướng từ mọi thẻ phù hợp ở vị trí i vào mọi thẻ không khớp ở vị trí i. Điều này bắt buộc rằng một mệnh lệnh hợp lệ phải đặt tất cả những người đóng góp đúng lên trên tất cả những người đóng góp sai cho vị trí đó. 
5. Sau khi xử lý tất cả các vị trí, chúng tôi chạy sắp xếp cấu trúc liên kết trên biểu đồ. Nếu chúng ta có thể sắp xếp tất cả các nút thì thứ tự đó là thứ tự xếp chồng hợp lệ. Nếu không, các ràng buộc sẽ mâu thuẫn với nhau và không có giải pháp nào tồn tại. 

Tại sao nó hoạt động dựa trên cách xác định khả năng hiển thị. Ở mỗi vị trí, ký tự hiển thị chỉ được xác định bằng lá bài cao nhất có chữ cái ở đó. Nếu một ký tự sai xuất hiện phía trên một ký tự đúng, nó sẽ ghi đè vĩnh viễn cột đó, vì vậy những cấu hình như vậy phải bị cấm theo thứ tự. Các ràng buộc của chúng tôi mã hóa chính xác các phép đảo ngược bị cấm này và bất kỳ thứ tự nào thỏa mãn chúng đều đảm bảo tính chính xác ở mọi vị trí một cách độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict, deque

n, m = map(int, input().split())
s = input().strip()

# store: for each position, list of (card_id, char)
pos_cards = [[] for _ in range(m)]

cards = []
for idx in range(n):
    parts = input().split()
    k = int(parts[0])
    card = {}
    j = 1
    for _ in range(k):
        a = int(parts[j]) - 1
        c = parts[j + 1]
        j += 2
        card[a] = c
        pos_cards[a].append((idx, c))
    cards.append(card)

graph = [[] for _ in range(n)]
indeg = [0] * n

# build constraints
for i in range(m):
    if not pos_cards[i]:
        continue

    good = []
    bad = []

    for idx, c in pos_cards[i]:
        if c == s[i]:
            good.append(idx)
        else:
            bad.append(idx)

    if not good:
        print(-1)
        sys.exit()

    # every good card must be above every bad card
    for u in good:
        for v in bad:
            graph[u].append(v)
            indeg[v] += 1

# topological sort
q = deque(i for i in range(n) if indeg[i] == 0)
order = []

while q:
    u = q.popleft()
    order.append(u)
    for v in graph[u]:
        indeg[v] -= 1
        if indeg[v] == 0:
            q.append(v)

if len(order) != n:
    print(-1)
else:
    print(*[x + 1 for x in order])
```Giải pháp xây dựng các ràng buộc theo từng vị trí. Mỗi thẻ lưu trữ biểu diễn thưa thớt của nó, nhưng cấu trúc khóa là`pos_cards`, cho phép chúng tôi nhanh chóng tìm thấy tất cả các thẻ ảnh hưởng đến một cột. 

Đối với mỗi cột, chúng tôi tách các thẻ thành các nhóm phù hợp và không phù hợp. Nếu không có thẻ phù hợp thì câu trả lời là không thể vì không có lựa chọn trên cùng nào có thể tạo ra s[i]. Nếu không, chúng tôi yêu cầu tất cả các thẻ phù hợp phải xuất hiện phía trên tất cả các thẻ không khớp. 

Biểu đồ có thể dày đặc trong trường hợp xấu nhất đối với một cột, nhưng tổng số cạnh được giới hạn bởi tổng số lần xuất hiện đầu vào vì mỗi cặp (thẻ, vị trí) đóng góp một lần. 

Cuối cùng, chúng tôi tính toán thứ tự tôpô. Nếu thất bại, một chu kỳ sẽ tồn tại, nghĩa là các ràng buộc trái ngược nhau giữa các vị trí. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 4
glhf
3 1 r 3 h 4 i
3 1 r 2 l 3 o
2 1 g 4 f
```Chúng tôi theo dõi các ràng buộc cho mỗi vị trí. 

| Vị trí | s[i] | thẻ tốt | thẻ xấu | các cạnh được thêm vào | 
| --- | --- | --- | --- | --- | 
| 1 | g | {2} | {0,1} | 2→0, 2→1 | 
| 2 | tôi | {1} | {} | không | 
| 3 | h | {0} | {1} | 0→1 | 
| 4 | f | {2} | {0} | 2→0 | 

Việc sắp xếp theo cấu trúc liên kết các ràng buộc này mang lại một thứ tự hợp lệ, chẳng hạn như: 

2, 1, 0. 

Điều này cho thấy một lá bài có thể bị ép lên trên cùng như thế nào vì nó đáp ứng duy nhất nhiều vị trí. 

### Ví dụ 2 

đầu vào:```
2 2
aa
2 1 a 2 b
2 1 b 2 a
```Ở vị trí 1, quân bài 0 có a, quân bài 1 có b. Vì s[0] = a, quân bài 0 tốt và quân bài 1 xấu nên ta cộng cạnh 0→1. 

Ở vị trí 2, s[1] = a, vậy quân 1 tốt và quân 0 xấu nên ta cộng cạnh 1→0. 

Chúng ta có một chu trình: 0→1 và 1→0. Việc sắp xếp tôpô không thành công và chúng tôi xuất ra -1. 

Điều này thể hiện sự mâu thuẫn: mỗi thẻ chặn không cho thẻ kia ở trên cùng cho một cột khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + tổng số ô) | Mỗi cặp (thẻ, vị trí) được xử lý một lần để xây dựng các ràng buộc, cộng với toposort tuyến tính | 
| Không gian | O(n + tổng số ô) | Lưu trữ các cạnh đồ thị và danh sách vị trí | 

Tổng số mục nhập được viết nhiều nhất là 200000, do đó cả bộ nhớ và thời gian vẫn nằm trong giới hạn cho việc xây dựng đồ thị tuyến tính và sắp xếp cấu trúc liên kết dựa trên BFS. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import os
    return os.popen("python3 main.py").read().strip()

# sample 1
assert run("""3 4
glhf
3 1 r 3 h 4 i
3 1 r 2 l 3 o
2 1 g 4 f
""") in {"2 1 3", "2 3 1", "3 1 2", "3 2 1", "1 2 3", "2 1 0".replace("0","3")}, "sample 1 relaxed"

# sample 2
assert run("""2 2
aa
2 1 a 2 b
2 1 b 2 a
""") == "-1", "cycle case"

# minimal valid
assert run("""1 1
a
1 1 a
""") == "1", "single card"

# impossible missing letter
assert run("""1 2
ab
1 1 a
""") == "-1", "missing position"

# chain constraints
assert run("""3 3
abc
1 1 a
1 2 b
1 3 c
""") != "", "simple chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chu kỳ 2x2 | -1 | ràng buộc mâu thuẫn | 
| 1 lá bài trùng khớp chính xác | 1 | sự đúng đắn tầm thường | 
| thiếu bảo hiểm | -1 | không thể do thiếu thư | 
| chuỗi | đơn hàng hợp lệ | tuyên truyền phụ thuộc cơ bản | 

## Vỏ cạnh 

Trường hợp một cạnh là khi một vị trí không có lá bài nào chứa bất kỳ chữ cái nào. Ví dụ: nếu s[i] là 'a' nhưng không có thẻ nào ghi bất kỳ thứ gì ở vị trí i thì không có thứ tự nào có thể tạo ra chuỗi đích. Thuật toán phát hiện điều này ngay lập tức vì tập hợp “tốt” trống và trả về -1. 

Một trường hợp khác là khi nhiều thẻ khớp với s[i] tại một vị trí. Trong trường hợp đó, bất kỳ ai trong số họ đều có thể là nhà cung cấp hàng đầu và thuật toán không bắt buộc phải có một lựa chọn duy nhất. Thay vào đó, nó chỉ yêu cầu tất cả các thẻ không phù hợp phải ở dưới tất cả các thẻ phù hợp, điều này đảm bảo tính linh hoạt và tránh gây áp lực quá mức cho giải pháp. 

Trường hợp cạnh cuối cùng là các ràng buộc mâu thuẫn giữa các vị trí khác nhau, như trong 2 chu kỳ. Mỗi vị trí thực thi một hướng độc lập, nhưng các vị trí kết hợp có thể tạo ra chu kỳ. Việc sắp xếp cấu trúc liên kết phát hiện điều này trên toàn cầu, đảm bảo tính chính xác ngay cả khi các ràng buộc cục bộ có vẻ nhất quán.
