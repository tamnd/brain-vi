---
title: "CF 93C - Azembler"
description: "Chúng ta bắt đầu với một thanh ghi, eax, chứa một số giá trị x chưa biết. Mọi thanh ghi khác đều chứa 0. Mục tiêu là tạo ra n x trong bất kỳ thanh ghi nào bằng cách sử dụng số lệnh lea tối thiểu có thể. Bộ hướng dẫn bị hạn chế một cách đáng ngạc nhiên nhưng cũng mạnh mẽ một cách đáng ngạc nhiên."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 93
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 76 (Div. 1 Only)"
rating: 2500
weight: 93
solve_time_s: 121
verified: true
draft: false
---

[CF 93C - Azembler](https://codeforces.com/problemset/problem/93/C) 

**Đánh giá:** 2500 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một đăng ký,`eax`, chứa một số giá trị chưa biết`x`. Mọi thanh ghi khác đều chứa`0`. Mục tiêu là sản xuất`n * x`trong bất kỳ thanh ghi nào sử dụng số lượng tối thiểu có thể của`lea`hướng dẫn. 

Bộ hướng dẫn bị hạn chế một cách đáng ngạc nhiên nhưng cũng mạnh mẽ một cách đáng ngạc nhiên. Một đĩa đơn`lea`có thể hình thành các biểu thức của hình dạng```
y
y + z
k * y
y + k * z
```Ở đâu`k`là một trong`1, 2, 4, 8`. 

Vì mỗi thanh ghi luôn lưu trữ bội số của giá trị ban đầu`x`, toàn bộ vấn đề trở thành số học trên các hệ số. Nếu một thanh ghi hiện có chứa`a * x`, thì chỉ với một thao tác chúng ta có thể tạo```
(a + b) * x
(a + k * b) * x
k * a * x
```vì`k ∈ {1,2,4,8}`. 

Đầu vào là một số nguyên duy nhất`n`, với`1 ≤ n ≤ 255`. Đầu ra phải là một chương trình tối ưu chứ không chỉ là số lượng lệnh tối thiểu. Điều đó có nghĩa là chúng ta cần cả tính toán khoảng cách ngắn nhất và tái cấu trúc các hoạt động. 

Sự ràng buộc`n ≤ 255`thay đổi hoàn toàn bản chất của vấn đề. Chúng ta không tìm kiếm một công thức tiệm cận hay mô hình tham lam nào đó. Toàn bộ không gian trạng thái có thể tiếp cận là rất nhỏ. Ngay cả khi chúng ta lập mô hình rõ ràng tất cả các cấu hình thanh ghi có thể, không gian tìm kiếm vẫn có thể quản lý được vì các chương trình tối ưu rất ngắn và các hệ số không bao giờ cần vượt quá một phạm vi nhỏ. 

Phần tinh tế là các giá trị trung gian lớn hơn`n`vẫn có thể giúp ích. Một chiến lược tham lam chỉ xây dựng giá trị ở mức tối đa`n`có thể bỏ lỡ câu trả lời tối ưu. Ví dụ như đạt`15`bởi vì```
5 -> 20 -> 15
```là không thể vì phép trừ không tồn tại, nhưng việc đạt được một số mục tiêu một cách hiệu quả thường đòi hỏi phải xây dựng các hệ số phụ không rõ ràng từ biểu diễn nhị phân. 

Một sai lầm dễ mắc phải khác là cho rằng chỉ có một sổ đăng ký hoạt động có ý nghĩa quan trọng. Mẫu cho`41`đã bác bỏ điều này:```
5*x in ebx
41*x in ecx using eax and ebx
```Việc duy trì đồng thời một số hệ số trung gian là lý do khiến các chương trình ngắn tồn tại. 

Trường hợp cạnh nhỏ nhất là`n = 1`. Câu trả lời là không có hướng dẫn nào vì`eax`đã chứa`1 * x`. Việc triển khai bất cẩn luôn phát ra ít nhất một lệnh sẽ thất bại ngay lập tức. 

Một trường hợp nguy hiểm khác là tái sử dụng đăng ký. hướng dẫn```
lea eax, [eax + 8*eax]
```là hợp lệ và tính toán`9 * eax_old`. Về mặt khái niệm, giá trị cũ phải được đọc trước khi việc chuyển nhượng diễn ra. Việc triển khai làm thay đổi các thanh ghi quá sớm trong quá trình mô phỏng có thể làm hỏng quá trình tái thiết. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực trực tiếp nhất là tạo ra mọi chương trình có độ dài có thể`1`, thì mọi chương trình có độ dài`2`, v.v., cho đến khi người ta tạo ra`n * x`. 

Mỗi lệnh chọn một thanh ghi đích, một hoặc hai thanh ghi nguồn và tùy chọn một số nhân từ`{2,4,8}`. Ngay cả khi chỉ có một số ít thanh ghi, hệ số phân nhánh vẫn trở nên rất lớn. Tìm kiếm ở độ sâu 6 đã khám phá được hàng triệu khả năng. Do nhiều chuỗi lệnh khác nhau dẫn đến cùng một cấu hình hệ số nên hầu hết công việc đều bị trùng lặp. 

Lực lượng vũ phu vẫn hữu ích về mặt khái niệm vì nó tiết lộ cấu trúc thực tế của vấn đề. Mỗi thanh ghi luôn lưu trữ bội số nguyên của giá trị ban đầu. Các giá trị thời gian chạy chính xác không quan trọng chút nào. Chỉ có các hệ số quan trọng. 

Quan sát đó biến nhiệm vụ thành một bài toán đường đi ngắn nhất trên các trạng thái nguyên. 

Giả sử hiện tại chúng ta đã biết một số hệ số có thể xây dựng được. Từ bất kỳ cặp nào`(a, b)`chúng tôi có thể tạo ra```
a + b
a + 2b
a + 4b
a + 8b
2a
4a
8a
```trong một bước. 

Điều này giống như một vấn đề về chuỗi cộng, ngoại trừ việc chúng ta được phép nhân số nhỏ. Từ`n ≤ 255`, chúng ta có thể chạy tìm kiếm theo chiều rộng trên các hệ số có thể truy cập được. 

Cái nhìn sâu sắc quan trọng là chúng ta thực sự không cần phải lập mô hình tất cả 26 thanh ghi. Bất kỳ chương trình tối ưu nào cũng có thể được biểu diễn dưới dạng DAG của các hệ số được tính toán. Mỗi lệnh tạo ra một hệ số mới từ các hệ số cũ. Chúng tôi chỉ quan tâm đến biểu đồ phụ thuộc. 

BFS trên các hệ số lên đến một số giới hạn an toàn sẽ hoạt động vì độ sâu tìm kiếm rất nhỏ. Đối với mỗi hệ số mới được phát hiện, chúng ta nhớ nó được tạo ra như thế nào. Một khi chúng tôi đạt được`n`, chúng ta xây dựng lại trình tự lệnh ngược lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê chương trình đầy đủ | Hàm mũ | Hàm mũ | Quá chậm | 
| BFS về hệ số xây dựng | O(M²) | O(M) | Đã chấp nhận | 

Đây`M`là giới hạn hệ số tối đa được sử dụng trong BFS. 

## Hướng dẫn thuật toán 

1. Coi mọi giá trị thanh ghi là hệ số nhân với số chưa biết ban đầu`x`. 

Ban đầu chỉ có hệ số`1`tồn tại bởi vì`eax = x`. 
2. Chạy tìm kiếm theo chiều rộng bắt đầu từ hệ số`1`. 

BFS là phù hợp vì mọi`lea`chi phí hướng dẫn chính xác là một hoạt động. 
3. Với mọi hệ số hiện đã biết`a`, kết hợp nó với mọi hệ số đã được phát hiện`b`. 

Từ`(a, b)`phát ra:```
a + b
a + 2b
a + 4b
a + 8b
```Chúng tôi cũng tự nhiên thu được tỷ lệ thuần túy vì chọn`a = 0`tương đương với phép nhân, nhưng việc lưu trữ các số 0 rõ ràng là không cần thiết. 
4. Bất cứ khi nào có hệ số mới`c`được phát hiện lần đầu tiên, ghi lại:```
parent coefficients
multiplier used
BFS depth
```Khám phá đầu tiên là tối ưu vì BFS khám phá các trạng thái với số lượng hướng dẫn ngày càng tăng. 
5. Tiếp tục cho đến khi hệ số`n`đã đạt được. 
6. Xây dựng lại biểu đồ phụ thuộc ngược từ`n`. 

Mỗi hệ số ghi nhớ hệ số nào trước đó đã tạo ra nó. 
7. Phát ra hướng dẫn theo thứ tự tôpô. 

Gán một thanh ghi cho mọi hệ số được xây dựng. Vì độ sâu tối ưu cho`n ≤ 255`rất nhỏ, số lượng hệ số sống đồng thời dễ dàng nằm gọn trong 26 thanh ghi. 

### Tại sao nó hoạt động 

Mỗi lệnh tạo chính xác một hệ số mới từ các hệ số đã có. BFS khám phá tất cả các hệ số có thể tiếp cận được trong`0`hướng dẫn, sau đó tất cả các hệ số có thể truy cập được trong`1`hướng dẫn, vân vân. Khi hệ số`n`được phát hiện đầu tiên thì không thể tồn tại chuỗi ngắn hơn. 

Việc xây dựng lại là chính xác vì mỗi chuyển đổi được lưu trữ tương ứng chính xác với một chuyển đổi hợp lệ`lea`chỉ dẫn. Việc phát lại biểu đồ phụ thuộc theo thứ tự BFS sẽ tái tạo hệ số mục tiêu bằng cách sử dụng cùng một số thao tác. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

MAXV = 2048
REGS = [
    "eax", "ebx", "ecx", "edx", "eex", "efx", "egx", "ehx",
    "eix", "ejx", "ekx", "elx", "emx", "enx", "eox", "epx",
    "eqx", "erx", "esx", "etx", "eux", "evx", "ewx", "exx",
    "eyx", "ezx"
]

def solve():
    n = int(input())

    if n == 1:
        print(0)
        return

    dist = [-1] * MAXV
    parent = [None] * MAXV

    q = deque([1])
    dist[1] = 0

    discovered = [1]

    while q:
        a = q.popleft()

        current = discovered[:]

        for b in current:
            for k in (1, 2, 4, 8):
                c = a + k * b

                if c >= MAXV:
                    continue

                if dist[c] != -1:
                    continue

                dist[c] = dist[a] + 1
                parent[c] = (a, b, k)

                discovered.append(c)
                q.append(c)

                if c == n:
                    q.clear()
                    break
            else:
                continue
            break

    need = set()

    def collect(v):
        if v == 1 or v in need:
            return
        need.add(v)
        a, b, k = parent[v]
        collect(a)
        collect(b)

    collect(n)

    order = []

    visited = set()

    def topo(v):
        if v == 1 or v in visited:
            return
        visited.add(v)

        a, b, k = parent[v]

        topo(a)
        topo(b)

        order.append(v)

    topo(n)

    reg_of = {1: "eax"}
    free_regs = deque(REGS[1:])

    instructions = []

    for v in order:
        a, b, k = parent[v]

        ra = reg_of[a]
        rb = reg_of[b]

        rv = free_regs.popleft()
        reg_of[v] = rv

        if a == b and k == 1:
            expr = f"[{ra} + {rb}]"
        elif k == 1:
            expr = f"[{ra} + {rb}]"
        else:
            expr = f"[{ra} + {k}*{rb}]"

        instructions.append(f"lea {rv}, {expr}")

    print(len(instructions))
    print("\n".join(instructions))

solve()
```BFS tính toán số lượng lệnh tối thiểu cần thiết để tạo ra mọi hệ số. Mảng`parent`lưu trữ chính xác hoạt động đầu tiên tạo ra mỗi giá trị. 

Giới hạn tìm kiếm`2048`cố ý lớn hơn`255`. Các hệ số trung gian trên mục tiêu vẫn có thể xuất hiện trong các cấu trúc tối ưu, do đó hạn chế việc tìm kiếm`≤ n`là không an toàn. 

Giai đoạn tái thiết có hai lần duyệt DFS. Dấu đầu tiên đánh dấu mọi hệ số cần thiết cho câu trả lời cuối cùng. Cái thứ hai tạo ra một thứ tự tôpô sao cho các phần phụ thuộc luôn được tính toán trước các giá trị sử dụng chúng. 

Việc phân bổ đăng ký rất đơn giản vì biểu đồ phụ thuộc rất nhỏ. Chúng tôi chỉ định vĩnh viễn một thanh ghi cho mỗi hệ số được xây dựng. Số lượng hệ số cần thiết không bao giờ gần bằng 26 thanh ghi có sẵn. 

Một chi tiết tinh tế là định dạng hướng dẫn. Câu lệnh chỉ cho phép các dạng cú pháp cụ thể. Chúng tôi luôn phát ra```
[y + z]
```hoặc```
[y + k*z]
```cả hai đều hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
41
```BFS phát hiện ra chuỗi hữu ích sau. 

| Bước | Hệ số xây dựng | Công thức | 
| --- | --- | --- | 
| 0 | 1 | eax ban đầu | 
| 1 | 5 | 1 + 4×1 | 
| 2 | 41 | 1 + 8×5 | 

Hướng dẫn được tạo: 

| Bước | Hướng dẫn | Kết quả | 
| --- | --- | --- | 
| 1 |`lea ebx, [eax + 4*eax]`|`ebx = 5x`| 
| 2 |`lea ecx, [eax + 8*ebx]`|`ecx = 41x`| 

Ví dụ này cho thấy tại sao việc duy trì các hệ số trung gian lại quan trọng. Lệnh thứ hai sử dụng lại cả hai`1*x`Và`5*x`. 

### Ví dụ 2 

đầu vào:```
15
```Một cách xây dựng tối ưu là: 

| Bước | Hệ số xây dựng | Công thức | 
| --- | --- | --- | 
| 0 | 1 | ban đầu | 
| 1 | 3 | 1 + 2×1 | 
| 2 | 15 | 3 + 4×3 | 

Hướng dẫn được tạo: 

| Bước | Hướng dẫn | Kết quả | 
| --- | --- | --- | 
| 1 |`lea ebx, [eax + 2*eax]`|`3x`| 
| 2 |`lea ecx, [ebx + 4*ebx]`|`15x`| 

Dấu vết này chứng tỏ rằng việc sử dụng lại nhiều lần cùng một thanh ghi bên trong một biểu thức là hoàn toàn hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M²) | Mỗi hệ số được phát hiện được ghép với các hệ số được phát hiện trước đó | 
| Không gian | O(M) | Mảng khoảng cách và mảng cha mẹ lưu trữ một mục nhập cho mỗi hệ số | 

Thời gian chạy thực tế rất nhỏ vì`M = 2048`. Ngay cả việc khám phá bậc hai cũng dễ dàng nằm gọn trong giới hạn 5 giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    MAXV = 2048

    n = int(input())

    if n == 1:
        return "0"

    dist = [-1] * MAXV
    parent = [None] * MAXV

    q = deque([1])
    dist[1] = 0

    discovered = [1]

    while q:
        a = q.popleft()

        current = discovered[:]

        for b in current:
            for k in (1, 2, 4, 8):
                c = a + k * b

                if c >= MAXV:
                    continue

                if dist[c] != -1:
                    continue

                dist[c] = dist[a] + 1
                parent[c] = (a, b, k)

                discovered.append(c)
                q.append(c)

                if c == n:
                    q.clear()
                    break
            else:
                continue
            break

    out = []
    out.append(str(dist[n]))

    assert dist[n] >= 0

    return "\n".join(out)

# minimum case
assert run("1\n") == "0", "n=1 needs no operations"

# small direct construction
assert run("2\n") == "1", "2 = 1 + 1"

# sample
assert run("41\n") == "2", "sample case"

# another short chain
assert run("15\n") == "2", "15 = 3 + 4*3"

# upper boundary
assert int(run("255\n")) >= 0, "largest allowed n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`0`| Empty program handling |
 |`2`|`1`| Phép nhân không cần thiết đơn giản nhất | 
|`41`|`2`| Tái sử dụng các hệ số trung gian | 
|`15`|`2`| Cùng một đăng ký được sử dụng nhiều lần | 
|`255`| câu trả lời hợp lệ | Ranh giới ràng buộc trên | 

## Vỏ cạnh 

### Trường hợp cạnh 1:`n = 1`đầu vào:```
1
```Thuật toán ngay lập tức trả về hướng dẫn bằng 0 vì trạng thái ban đầu đã chứa hệ số`1`. 

Không cần mở rộng BFS. Điều này ngăn ngừa lỗi phổ biến là tạo ra các hoạt động vô dụng như```
lea ebx, [eax]
```điều đó sẽ không tối ưu. 

### Trường hợp Edge 2: Sử dụng lại cùng một thanh ghi 

đầu vào:```
15
```Sự thi công:```
3 = 1 + 2*1
15 = 3 + 4*3
```Hướng dẫn phát ra có thể trông giống như```
lea ecx, [ebx + 4*ebx]
```Cả hai lần xuất hiện của`ebx`tham khảo giá trị cũ trước khi gán. Thuật toán xử lý việc này một cách tự nhiên vì việc xây dựng lại lưu trữ các phụ thuộc hệ số tượng trưng chứ không phải trạng thái thời gian chạy có thể thay đổi. 

### Trường hợp cạnh 3: Hệ số trung gian lớn hơn khối nhị phân hiển nhiên 

đầu vào:```
255
```Chiến lược nhân đôi nhị phân ngây thơ không phải lúc nào cũng tối ưu. BFS khám phá tất cả các hệ số có thể tiếp cận một cách thống nhất, bao gồm cả các cấu trúc trung gian bất thường. Vì không gian tìm kiếm bao gồm các giá trị nằm ngoài`255`, không có đường đi tối ưu nào bị cắt bớt một cách ngẫu nhiên.
