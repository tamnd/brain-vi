---
title: "CF 102365C - Tìm kiếm việc làm"
description: "Chúng ta có một cây có (N) thành phố. Thành phố cuối chỉ đơn giản là một chiếc lá, một đỉnh có bậc chính xác bằng một. Bản thân cái cây đã bị chúng ta che giấu. Chúng ta không thể kiểm tra các cạnh của nó một cách trực tiếp."
date: "2026-08-14T02:54:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102365
codeforces_index: "C"
codeforces_contest_name: "UBC Programming Contest 2019 (UBCPC 2019)"
rating: 0
weight: 102365
solve_time_s: 141
verified: true
draft: false
---

[CF 102365C - Tìm kiếm việc làm](https://codeforces.com/problemset/problem/102365/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có (N) thành phố. Thành phố cuối chỉ đơn giản là một chiếc lá, một đỉnh có bậc chính xác bằng một. Bản thân cái cây đã bị chúng ta che giấu. Chúng ta không thể kiểm tra các cạnh của nó một cách trực tiếp. Thay vào đó, chúng ta có một oracle trả lời truy vấn đường dẫn thành viên: cho ba thành phố (a,b,c), nó cho chúng ta biết liệu (b) có thuộc đường dẫn duy nhất từ ​​(a) đến (c) hay không. 

Nhiệm vụ là xuất ra bất kỳ lá nào trong khi hỏi tối đa (N) câu hỏi như vậy. Vì đây là một bài toán tương tác nên đầu vào sau (N) bao gồm các câu trả lời do giám khảo cung cấp để đáp lại các truy vấn của chúng tôi. Do đó, mẫu là bản ghi chứ không phải là cặp đầu vào/đầu ra thông thường. 

Ràng buộc (N \le 2000) đủ lớn để việc xây dựng lại toàn bộ cây thông qua nhiều truy vấn là không khả thi với ngân sách truy vấn chỉ (N). Một giải pháp sử dụng các lệnh gọi oracle (O(N^2)) hoặc (O(N^3)) đã bị loại, bất kể mỗi hoạt động riêng lẻ có rẻ đến mức nào. Mục tiêu là tuyến tính về số lượng thành phố và yêu cầu bộ nhớ là không đáng kể vì cây ẩn không bao giờ cần phải lưu trữ. 

Có hai trường hợp tế nhị có thể phá vỡ cách tiếp cận bất cẩn. Đầu tiên, ứng cử viên ban đầu không nhất thiết phải là một chiếc lá. Đối với cây (1-2-3), bắt đầu từ thành phố (2) chỉ ổn nếu thuật toán biết cách di chuyển từ một đỉnh trong về phía con cháu. Đơn giản chỉ cần trả về thành phố (2) sẽ sai vì kết quả đầu ra đúng là (1) hoặc (3). 

Thứ hai, ứng cử viên có thể thay đổi nhiều lần và một đỉnh đã được kiểm tra không được khiến chúng ta quay trở lại gốc. Hãy xem xét cây (1-2-5-3). Bắt đầu từ thành phố (2), thành phố (3) có thể chuyển ứng viên đến (3), và thành phố sau (5) không được chuyển lại về (5). Truy vấn hỏi liệu ứng cử viên hiện tại có nằm trên con đường từ gốc tới thành phố mới hay không. Vì (3) không nằm trên đường (1) đến (5) nên câu trả lời là sai nên lá (3) được giữ nguyên. Việc giải thích bất cẩn về hướng đi có thể âm thầm đảo ngược quyết định này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng xác định xem mỗi thành phố có phải là một chiếc lá hay không. Một thành phố (b) không phải là một chiếc lá khi có hai thành phố khác có đường đi qua (b). Chúng ta có thể kiểm tra nhiều cặp (a,c) và hỏi liệu (b) có nằm trên đường đi của chúng hay không. Trong trường hợp xấu nhất, việc kiểm tra một ứng cử viên với tất cả các cặp thành phố khác yêu cầu truy vấn (\binom{N-1}{2}) và việc thực hiện điều đó đối với tất cả các ứng cử viên yêu cầu 

[ 
N\binom{N-1}{2} = \frac{N(N-1)(N-2)}{2}, 
] 

đó là (\Theta(N^3)). Đối với (N=2000), đây là khoảng (4\times 10^9) truy vấn, vượt xa (N) cho phép. 

Quan sát hữu ích là chúng ta không cần xác định xem mọi đỉnh có phải là một chiếc lá hay không. Chúng ta chỉ cần di chuyển một ứng cử viên về phía một chiếc lá. 

Rễ cây ẩn ở thành phố (1). Với bất kỳ thành phố (v) nào, hãy gọi thành phố khác (x) là hậu duệ của (v) nếu (v) nằm trên đường đi từ gốc (1) đến (x). Nhà tiên tri đưa ra chính xác bài kiểm tra mà chúng tôi cần. Truy vấn 

[ 
? \ 1\ v\ x 
] 

trả về true chính xác khi (v) là tổ tiên của (x). 

Giả sử ứng cử viên hiện tại của chúng tôi là (v). Nếu (v) không phải là lá thì vì không phải là gốc nên nó có ít nhất một con, và con đó có một số hậu duệ (x). Đối với (x) như vậy, truy vấn trả về true, vì vậy chúng ta có thể thay thế (v) bằng (x), di chuyển xuống dưới trong cây có gốc. 

Chi tiết quan trọng là chúng tôi xử lý mọi thành phố chính xác một lần. Nếu gặp một hậu duệ của ứng cử viên hiện tại sớm hơn, thì ứng viên đó đã chuyển sang cây con của ứng cử viên đó. Do đó, khi một thành phố sau trở thành ứng cử viên, nó không thể là tổ tiên của ứng cử viên đã được xử lý trước đó. Ứng viên chỉ di chuyển xuống dưới.

Điều này biến toàn bộ vấn đề thành một lần quét. Chúng ta bắt đầu với thành phố (2), sau đó kiểm tra các thành phố (3,4,\ldots,N). Bất cứ khi nào ứng cử viên hiện tại nằm trên đường từ thành phố (1) đến thành phố được kiểm tra, chúng tôi sẽ di chuyển ứng viên đến thành phố được kiểm tra đó. Sau khi tất cả các thành phố đã được xử lý, ứng viên không thể có thành phố kế thừa chưa được xử lý. Vì thế nó là một chiếc lá. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^3)) truy vấn | (O(1)) | Quá chậm | 
| Tối ưu | (O(N)) truy vấn | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chọn thành phố (1) làm gốc khái niệm và thành phố (2) làm ứng cử viên ban đầu. Chúng ta không cần biết bất kỳ cạnh thực tế nào. Thuộc tính duy nhất chúng tôi sử dụng là ý nghĩa của truy vấn đường dẫn với thành phố (1) là một điểm cuối. 
2. Xử lý mọi thành phố (c) từ (3) đến (N). Hỏi xem ứng cử viên hiện tại (v) có nằm trên đường từ thành phố (1) đến (c) không. 
3. Nếu câu trả lời là đúng, hãy thay (v) bằng (c). Trong cây có gốc tại (1), điều này có nghĩa là (c) nằm trong cây con có gốc tại (v), do đó việc di chuyển đến (c) sẽ di chuyển ứng viên ra xa gốc hơn. 
4. Nếu trả lời sai thì giữ nguyên (v). Thành phố (c) nằm ngoài cây con của (v) hoặc ở trên (v), vì vậy nó không thể được sử dụng để di chuyển ứng viên xuống dưới. 
5. Sau khi tất cả (N-2) truy vấn đã được thực hiện, hãy xuất ra ứng viên hiện tại. Ứng cử viên là một thành phố đầu cuối. 

Số lượng truy vấn thực tế là (N-2), thấp hơn giới hạn (N) một cách an toàn. 

### Tại sao nó hoạt động 

Duy trì bất biến rằng ứng viên hiện tại là một đỉnh trên chuỗi đi xuống trong cây có gốc và mọi đỉnh được xử lý nằm trong cây con của nó đều có khả năng di chuyển ứng viên xuống xa hơn. 

Khi truy vấn trả về true cho thành phố (c), ứng cử viên hiện tại (v) nằm trên đường dẫn (1) đến (c). Do đó (c) là hậu duệ của (v), do đó thay thế (v) bằng (c) là một bước đi xuống hợp lệ. 

Giả sử ứng cử viên cuối cùng (v) không phải là một chiếc lá. Vì (v\neq1) nên nó sẽ có con và do đó có ít nhất một con cháu (x). Bởi vì (x\neq v), nó xuất hiện ở đâu đó trong quá trình quét. Khi (x) được xử lý, ứng viên hiện tại là (v), trong trường hợp đó truy vấn sẽ trả về đúng và chuyển ứng viên xuống dưới hoặc ứng viên đã chuyển sang một hậu duệ khác của (v). Trong trường hợp sau, ứng cử viên hiện tại đã ở dưới (v) và đối số tương tự được áp dụng đệ quy. Do đó, một ứng cử viên nội bộ không thể sống sót sau quá trình quét hoàn chỉnh. Ứng cử viên cuối cùng duy nhất có thể là một chiếc lá. 

## Giải pháp Python 

Nhiệm vụ ban đầu mang tính tương tác nên chương trình phải in từng truy vấn, xóa đầu ra và sau đó đọc câu trả lời của giám khảo. Người trợ giúp`ask`đóng gói giao thức này.```python
import sys

input = sys.stdin.readline

def solve():
    n = int(input())

    current = 2

    for c in range(3, n + 1):
        print("?", 1, current, c, flush=True)

        response = int(input())
        if response == -1:
            return

        if response == 1:
            current = c

    print("!", current, flush=True)

if __name__ == "__main__":
    solve()
```Dòng đầu tiên ghi (N), sau đó không có cạnh cây nào. Biến`current`lưu trữ phần trạng thái duy nhất mà thuật toán cần. Thành phố`1`vẫn cố định là gốc. 

Vòng lặp bắt đầu lúc`3`bởi vì thành phố`2`đã là ứng cử viên ban đầu. Mỗi lần lặp lại yêu cầu chính xác một truy vấn. Khi phản hồi là`1`, ứng cử viên trở thành thành phố mới được kiểm tra vì ứng cử viên hiện tại là tổ tiên của nó trong cây bắt nguồn từ thành phố`1`. 

phản hồi`-1`là tín hiệu của trọng tài cho thấy sự tương tác đã thất bại. Chương trình phải chấm dứt ngay lập tức thay vì đưa ra một truy vấn khác.`flush=True`là điều cần thiết trong một vấn đề tương tác. Nếu không có nó, Python có thể đệm truy vấn thay vì gửi nó cho giám khảo, khiến chương trình phải chờ mãi một câu trả lời mà thẩm phán không bao giờ nhận được yêu cầu. 

Không có vấn đề tràn số nguyên trong Python và thuật toán chỉ lưu trữ hai biến số nguyên. Câu trả lời cuối cùng được in kèm theo yêu cầu`!`tiền tố và chương trình kết thúc sau đó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu phù hợp với đường dẫn ba thành phố 

[ 
1 - 2 - 3. 
] 

Cả hai thành phố (1) và (3) đều là lá. Thuật toán của chúng tôi được phép xuất ra một trong hai. 

| Bước | Ứng viên hiện tại | Thành phố được kiểm tra | Truy vấn | Phản hồi | Ứng viên mới | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 2 | | | | 2 | 
| 1 | 2 | 3 | 2 có nằm trên đường dẫn (1) đến (3) không? | 1 | 3 | 

Thuật toán xuất ra thành phố (3). Điều này khác với bản ghi của mẫu, sử dụng một chuỗi truy vấn hợp lệ khác và thành phố đầu ra (1), nhưng cả hai câu trả lời đều là thành phố cuối. 

### Mẫu 2 

Đối với dấu vết thứ hai, hãy xem xét cây có gốc 

[ 
1-2,\qquad 1-3,\qquad 3-4,\qquad 3-5. 
] 

Những chiếc lá là (2,4,5). Ứng cử viên ban đầu (2) đã là một lá, vì vậy mọi truy vấn sau đó phải giữ nguyên. 

| Bước | Ứng viên hiện tại | Thành phố được kiểm tra | Truy vấn | Phản hồi | Ứng viên mới | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 2 | | | | 2 | 
| 1 | 2 | 3 | 2 có nằm trên đường dẫn (1) đến (3) không? | 0 | 2 | 
| 2 | 2 | 4 | 2 có nằm trên đường dẫn (1) đến (4) không? | 0 | 2 | 
| 3 | 2 | 5 | 2 có nằm trên đường dẫn (1) đến (5) không? | 0 | 2 | 

Câu trả lời cuối cùng là (2). Dấu vết này chứng tỏ tại sao một câu trả lời sai không được khiến thí sinh thay đổi. Thành phố (2) nằm trong một nhánh khác với các thành phố (3,4,5), vì vậy không có thành phố nào trong số đó là hậu duệ của (2). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N)) | Có chính xác một truy vấn oracle cho mỗi thành phố từ (3) đến (N). | 
| Không gian | (O(1)) | Chỉ (N), ứng viên hiện tại và phản hồi hiện tại được lưu trữ. | 

Thuật toán yêu cầu (N-2) truy vấn, phù hợp với tối đa (N) truy vấn còn chỗ trống. Với (N\le2000), việc tính toán được thực hiện cục bộ là không đáng kể. Chi phí chủ yếu là sự tương tác với thẩm phán và giải pháp giữ cho chi phí đó ở mức tuyến tính. 

## Trường hợp thử nghiệm 

Vì vấn đề ban đầu có tính chất tương tác nên một vấn đề bình thường`run(input)`không thể kiểm tra giải pháp chính thức bằng cách cung cấp cho nó một đầu vào tĩnh hoàn chỉnh. Câu trả lời của giám khảo phụ thuộc vào cây ẩn và các truy vấn do chương trình tạo ra. Đối với thử nghiệm ngoại tuyến, cách tiếp cận rõ ràng là giữ nguyên logic lựa chọn ứng viên và thay thế oracle tương tác bằng một trình mô phỏng biết cây. 

Phần khai thác sau đây sử dụng định nghĩa đường dẫn chính xác từ vấn đề để trả lời các truy vấn mô phỏng. Mẫu được cung cấp được biểu thị bằng đường dẫn tương thích (1-2-3), trong khi các trường hợp tùy chỉnh thực hiện các ngôi sao, chuỗi và nhánh.```python
# Offline version of the algorithm for testing.
# The real submission uses the interactive solve() above.

def find_leaf(n, edges):
    graph = [[] for _ in range(n + 1)]
    for u, v in edges:
        graph[u].append(v)
        graph[v].append(u)

    parent = [0] * (n + 1)
    parent[1] = -1
    stack = [1]

    while stack:
        v = stack.pop()
        for to in graph[v]:
            if to == parent[v]:
                continue
            parent[to] = v
            stack.append(to)

    def is_on_path_root_to(v, x):
        while x != 0:
            if x == v:
                return True
            x = parent[x]
        return False

    current = 2

    for c in range(3, n + 1):
        if is_on_path_root_to(current, c):
            current = c

    return current

def run(n, edges):
    return find_leaf(n, edges)

# Provided sample, represented by the compatible tree 1-2-3.
answer = run(3, [(1, 2), (2, 3)])
assert answer in {1, 3}, "sample 1"

# Minimum-size tree.
answer = run(3, [(1, 2), (1, 3)])
assert answer in {2, 3}, "minimum-size tree"

# Star centered at 1.
answer = run(
    6,
    [(1, 2), (1, 3), (1, 4), (1, 5), (1, 6)]
)
assert answer in {2, 3, 4, 5, 6}, "star"

# Chain with the initial candidate already a leaf.
answer = run(
    7,
    [(1, 2), (2, 3), (3, 4), (4, 5), (5, 6), (6, 7)]
)
assert answer in {1, 7}, "chain"

# Branching tree designed to catch an incorrect interpretation
# that moves upward when the query answer is 0.
answer = run(
    7,
    [(1, 2), (2, 3), (2, 4), (4, 5), (4, 6), (6, 7)]
)
assert answer in {3, 5, 7}, "branching tree"

# Larger boundary-style case.
n = 2000
edges = [(1, 2)] + [(i, i + 1) for i in range(2, n)]
answer = run(n, edges)
assert answer in {1, n}, "n = 2000 chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| (1-2-3) | (1) hoặc (3) | Cung cấp mẫu và số lượng thành phố tối thiểu | 
| Ngôi sao tập trung tại (1) | Bất kỳ thành phố nào từ (2) đến (6) | Nhiều lá và phân nhánh ngay | 
| Chuỗi (1-2-3-4-5-6-7) | (1) hoặc (7) | Cây sâu và chuyển động đi xuống lặp đi lặp lại | 
| (1-2), (2-3), (2-4), (4-5), (4-6), (6-7) | (3), (5), hoặc (7) | Phân nhánh và giải thích chính xác một truy vấn sai | 
| Xích có (N=2000) | (1) hoặc (2000) | Đầu vào kích thước tối đa và hành vi tuyến tính | 

Trình mô phỏng ngoại tuyến chỉ xây dựng lại mối quan hệ cấp trên để thử nghiệm. Giải pháp tương tác đã gửi không bao giờ có quyền truy cập vào các cạnh đó. 

## Vỏ cạnh 

Đối với cây có kích thước tối thiểu với đầu vào```
3
```chỉ có hai truy vấn có sẵn trong giao thức và thuật toán chỉ cần một truy vấn. Nó bắt đầu với thành phố (2) và hỏi liệu thành phố (2) có nằm trên đường từ (1) đến (3) hay không. Nếu câu trả lời sai, thành phố (2) là một chiếc lá và được trả về. Nếu câu trả lời là đúng, thành phố (3) nằm xa hơn trong cây có gốc và trở thành ứng cử viên. Vì cây ba đỉnh luôn có hai lá nên kết quả nào cũng đúng. 

Đối với một ngôi sao như```
1
/|\
2 3 4
```ứng cử viên ban đầu (2) đã là một chiếc lá. Mọi truy vấn liên quan đến thành phố (3) và (4) đều hỏi liệu (2) có nằm trên đường đi bắt đầu từ tâm (1) hay không. Câu trả lời luôn sai vì những đường đi đó đi thẳng qua (1) chứ không phải qua (2). Ứng viên còn lại (2). 

Đối với một chuỗi dài như```
1 - 2 - 3 - 4 - 5 - 6
```thuật toán bắt đầu tại (2). Truy vấn về thành phố (3) là đúng, vì vậy ứng viên trở thành (3). Truy vấn cho (4) là đúng, theo sau là câu trả lời đúng cho (5) và (6). Ứng cử viên cuối cùng là (6), một chiếc lá. Điều này chứng tỏ rằng thuật toán có thể đi xuống cây gốc nhiều lần và vẫn chỉ sử dụng một truy vấn cho mỗi thành phố. 

Đối với một cây phân nhánh như```
      1
      |
      2
     / \
    3   4
       / \
      5   6
           \
            7
```ứng viên bắt đầu ở (2). Thành phố (3) là hậu duệ của (2) nên ứng viên trở thành (3). Khi thành phố (4) được kiểm tra, truy vấn sẽ hỏi liệu (3) có nằm trên đường từ (1) đến (4) hay không. Không, vì vậy ứng viên vẫn ở (3). Các thành phố sau này (5,6,7) cũng nằm ngoài cây con có gốc tại (3), nên ứng cử viên còn lại (3), là một chiếc lá. Trường hợp này mắc phải sai lầm phổ biến là coi một truy vấn sai là bằng chứng cho thấy thành phố được kiểm tra nên thay thế ứng viên. 

Đối với kích thước tối đa (N=2000), vòng lặp thực hiện chính xác các truy vấn (1998). Thuật toán không phân bổ biểu đồ, ngăn xếp đệ quy hoặc mảng phụ trợ, do đó mức sử dụng bộ nhớ không đổi. Số lượng truy vấn nằm dưới giới hạn nghiêm ngặt là (2000), đây là hạn chế chính của vấn đề.
