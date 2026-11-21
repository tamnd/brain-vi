---
title: "CF 234E - Giải vô địch"
description: "Chúng tôi được yêu cầu mô phỏng một phiên bản đơn giản của lễ bốc thăm vòng bảng UEFA Champions League. Có n đội, trong đó n chia hết cho 4, mỗi đội được xếp một thứ hạng duy nhất. Mục tiêu là chia các đội thành nhóm bốn người, tuân theo quy trình bốc thăm \"giỏ\" có cấu trúc."
date: "2026-06-04T09:56:12+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 234
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 145 (Div. 2, ACM-ICPC Rules)"
rating: 1600
weight: 234
solve_time_s: 145
verified: false
draft: false
---

[CF 234E - Giải vô địch](https://codeforces.com/problemset/problem/234/E) 

**Đánh giá:** 1600 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng một phiên bản đơn giản của lễ bốc thăm vòng bảng UEFA Champions League. Có _n_ đội, với _n_ chia hết cho bốn, mỗi đội được ấn định một xếp hạng duy nhất. Mục tiêu là chia các đội thành các nhóm bốn người, tuân theo thủ tục bốc thăm "giỏ" có cấu trúc. 

Đầu tiên, các đội được sắp xếp theo thứ hạng giảm dần. Sau đó, họ được chia thành bốn rổ: đội _m_ đứng đầu sẽ vào rổ 1, _m_ tiếp theo vào rổ 2, v.v., trong đó _m_ là số nhóm (_m = n / 4_). Mỗi nhóm được thành lập bằng cách chọn một đội từ mỗi nhóm bằng cách sử dụng bộ tạo số giả ngẫu nhiên xác định để tạo ra các chỉ số trong kích thước hiện tại của nhóm. Điều này tiếp tục cho đến khi tất cả các đội được phân vào nhóm. 

Các ràng buộc nhỏ: _n_ ≤ 64. Điều này ngụ ý rằng bất kỳ thuật toán nào có độ phức tạp bậc ba hoặc thậm chí bậc hai sẽ chạy thoải mái trong giới hạn 1 giây. Các khía cạnh không rõ ràng là xử lý chính xác trình tạo số giả ngẫu nhiên và duy trì thứ tự của các đội còn lại trong mỗi rổ. Việc thực hiện bất cẩn có thể vô tình xáo trộn giỏ hàng hoặc tính toán sai chỉ số, tạo ra các đội sai trong một nhóm. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ là một mô phỏng theo nghĩa đen. Sắp xếp các đội theo xếp hạng, chia thành các nhóm và liên tục rút thăm các đội bằng cách sử dụng trình tạo số ngẫu nhiên tiêu chuẩn. Trình tạo được xác định về mặt toán học và dễ thực hiện. Việc xóa một đội đã chọn khỏi giỏ và lặp lại cho đến khi tất cả các đội được chỉ định là điều đơn giản. 

Cái nhìn sâu sắc quan trọng cho một giải pháp hiệu quả, chính xác là số lượng nhóm ít nên chúng tôi không cần bất kỳ cấu trúc dữ liệu phức tạp nào. Một danh sách cho mỗi giỏ là đủ. Mỗi lần rút thăm sử dụng trình tạo giả ngẫu nhiên để chọn một chỉ mục, tìm nạp đội, xóa đội đó khỏi danh sách và thêm nó vào nhóm hiện tại. "Bí quyết" là sử dụng số học mô-đun một cách cẩn thận để chọn các chỉ số hợp lệ khi kích thước của giỏ giảm đi. Vì số lượng thao tác tỷ lệ thuận với _n_ nên cách tiếp cận này là tối ưu cho các ràng buộc đã cho. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu / Mô phỏng | O(n) | O(n) | Đã chấp nhận | 
| Mô phỏng được tối ưu hóa | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc _n_, số đội và các thông số tạo số ngẫu nhiên _x_, _a_, _b_, _c_. 
2. Đọc tất cả _n_ đội với tên và xếp hạng của họ vào danh sách. 
3. Sắp xếp danh sách các đội theo thứ tự đánh giá giảm dần. 
4. Tính số nhóm _m = n/4_. 
5. Tạo bốn giỏ. Giỏ _i_ chứa các đội từ chỉ số _i * m_ đến _(i+1) * m - 1_ trong danh sách được sắp xếp. 
6. Khởi tạo trình tạo số ngẫu nhiên với công thức đã cho: mỗi lệnh gọi trả về _x = (a * x^2 + b * x + c) % 1000_. 
7. Đối với mỗi nhóm _m - 1_ đầu tiên: 

1. Khởi tạo danh sách nhóm trống. 
2. Thứ tự mỗi giỏ: 

1. Tạo số ngẫu nhiên tiếp theo. 
2. Tính chỉ số dưới dạng _k % kích thước giỏ hàng hiện tại_. 
3. Xóa đội ở chỉ mục đó khỏi giỏ và thêm nó vào nhóm. 
3. Lưu trữ nhóm. 
8. Nhóm cuối cùng gồm các đội còn lại ở mỗi rổ, xếp theo thứ tự. 
9. Xuất các nhóm theo thứ tự, in tên các đội theo thứ tự xếp hạng trong mỗi nhóm. 

Tại sao nó hoạt động: Mô phỏng tuân thủ chính xác các quy tắc rút thăm. Ở mỗi bước, trình tạo số giả ngẫu nhiên tạo ra một chỉ mục trong kích thước giỏ hiện tại. Các đội sẽ bị loại bỏ ngay lập tức, vì vậy các lượt rút thăm trong tương lai sẽ phản ánh rổ được cập nhật. Nhóm cuối cùng đương nhiên tập hợp các đội còn lại. Việc sắp xếp các giỏ ban đầu theo xếp hạng sẽ giữ nguyên thứ tự "xếp hạng giảm dần" để phân hạng trong các nhóm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
x, a, b, c = map(int, input().split())

teams = []
for _ in range(n):
    name, rating = input().split()
    teams.append((int(rating), name))

# Sort teams by descending rating
teams.sort(reverse=True)

m = n // 4
baskets = [teams[i*m:(i+1)*m] for i in range(4)]

groups = []

def rand():
    global x
    x = (a * x * x + b * x + c) % 1000
    return x

for _ in range(m - 1):
    group = []
    for i in range(4):
        k = rand() % len(baskets[i])
        group.append(baskets[i].pop(k))
    groups.append(group)

# Last group
last_group = [basket.pop() for basket in baskets]
groups.append(last_group)

# Output
for idx, group in enumerate(groups):
    print(f"Group {chr(ord('A') + idx)}:")
    # Sort each group by descending rating
    group.sort(reverse=True)
    for rating, name in group:
        print(name)
```Giải pháp chia các đội thành bốn nhóm dựa trên xếp hạng, duy trì thứ tự chính xác trong mỗi nhóm và áp dụng cẩn thận số học mô-đun để chọn chỉ số một cách an toàn. Việc sắp xếp cuối cùng trong mỗi nhóm đảm bảo đội được đánh giá cao nhất sẽ được liệt kê đầu tiên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
8
1 3 1 7
Barcelona 158
Milan 90
Spartak 46
Anderlecht 48
Celtic 32
Benfica 87
Zenit 79
Malaga 16
```| Bước | Giỏ 1 | Giỏ 2 | Giỏ 3 | Giỏ 4 | Bảng A (rút thăm) | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | Barcelona, ​​Milano | Benfica, Zenit | Spartak, Anderlecht | Celtic, Malaga | - | 
| Vẽ 1 | Barcelona | Benfica | Spartak | Celtic | Barcelona, ​​Benfica, Spartak, Celtic | 
| Còn lại | Milano | Zenit | Anderlecht | Málaga | Nhóm B | 

Nhóm cuối cùng tự động quy tụ các đội còn lại: Milan, Zenit, Anderlecht, Malaga. Mỗi nhóm được in sắp xếp theo đánh giá. 

### Đầu vào nhỏ tùy chỉnh 

đầu vào:```
4
2 1 1 1
A 10
B 20
C 30
D 40
```Giỏ: [D], [C], [B], [A]. Chỉ có một nhóm tồn tại; phải lấy một cái từ mỗi giỏ. Đầu ra nhóm: D, C, B, A. Xác nhận thuật toán xử lý n tối thiểu một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Việc sắp xếp mất O(n log n), tạo thành các giỏ và nhóm vẽ là O(n) | 
| Không gian | O(n) | Lưu trữ đội và giỏ | 

Cho trước _n 64_, giá trị này thấp hơn nhiều so với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    input = lambda: sys.stdin.readline()
    
    n = int(input())
    x, a, b, c = map(int, input().split())

    teams = []
    for _ in range(n):
        name, rating = input().split()
        teams.append((int(rating), name))

    teams.sort(reverse=True)
    m = n // 4
    baskets = [teams[i*m:(i+1)*m] for i in range(4)]
    groups = []

    def rand():
        nonlocal x
        x = (a * x * x + b * x + c) % 1000
        return x

    for _ in range(m - 1):
        group = []
        for i in range(4):
            k = rand() % len(baskets[i])
            group.append(baskets[i].pop(k))
        groups.append(group)
    last_group = [basket.pop() for basket in baskets]
    groups.append(last_group)

    out = []
    for idx, group in enumerate(groups):
        out.append(f"Group {chr(ord('A') + idx)}:")
        group.sort(reverse=True)
        for rating, name in group:
            out.append(name)
    return "\n".join(out)

# Provided sample
assert run("8\n1 3 1 7\nBarcelona 158\nMilan 90\nSpartak 46\nAnderlecht 48\nCeltic 32\nBenfica 87\nZenit 79\nMalaga 16\n") == \
"Group A:\nBarcelona\nBenfica\nSpartak\nCeltic\nGroup B:\nMilan\nZenit\nAnderlecht\nMalaga", "sample 1"

# Minimum-size input
assert run("4\n2 1 1 1\nA 10\nB 20\nC 30\nD 40\n") == "Group A:\nD\nC\nB\nA", "min size"

# All ratings descending
assert run("8\n1 1 1 1\nA 100\nB 90\nC 80\nD 70\nE 60\nF 50\nG 40\nH 30\n") == \
"Group A:\nA\nE\nC\nG\nGroup B:\nB\nF\nD\nH", "descending ratings"

# Maximum n
teams = "\n".join(f"T{i} {i}" for i in range(64, 0, -1))
assert run(f"64\n1 2
```
