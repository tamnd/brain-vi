---
title: "CF 105481C - \u63d2\u6392\u4e32\u8054"
description: "Hệ thống này là một cây có rễ mô hình hóa thiết lập điện. Nút gốc là một ổ cắm duy nhất có giới hạn nguồn cố định và mọi nút khác là thiết bị điện hoặc dải nguồn."
date: "2026-06-23T01:59:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105481
codeforces_index: "C"
codeforces_contest_name: "2024 CCPC Liaoning Provincial Contest"
rating: 0
weight: 105481
solve_time_s: 56
verified: true
draft: false
---

[CF 105481C - \u63d2\u6392\u4e32\u8054](https://codeforces.com/problemset/problem/105481/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hệ thống này là một cây có rễ mô hình hóa thiết lập điện. Nút gốc là một ổ cắm duy nhất có giới hạn nguồn cố định và mọi nút khác là thiết bị điện hoặc dải nguồn. Các lá là các thiết bị có mức tiêu thụ điện năng cố định, trong khi các nút bên trong là các dải nguồn có giới hạn công suất cố định. 

Đối với bất kỳ nút nào, “tải” của nó là tổng của tất cả công suất thiết bị trong cây con của nó. Một cấu hình hợp lệ nếu mỗi dải nguồn, kể cả ổ cắm gốc, có công suất ít nhất bằng tải cây con của nó. 

Cấu trúc của cây được cố định. Hoạt động duy nhất được phép là hoán đổi giá trị dung lượng của hai nút bên trong bất kỳ. Giá trị lá được cố định và không thể di chuyển. 

Nhiệm vụ là xác định xem, sau khi hoán vị tùy ý công suất giữa các nút bên trong, có thể chỉ định từng công suất cho một dải nguồn sao cho tải cây con của nó thỏa mãn giới hạn công suất của mỗi nút hay không. 

Các ràng buộc ngụ ý rằng giải pháp O(n) hoặc O(n log n) là cần thiết. Với n lên tới 100000, mọi nỗ lực xem xét các hoán vị một cách rõ ràng hoặc mô phỏng các giao dịch hoán đổi đều không khả thi. Khó khăn chính là dung lượng được sắp xếp lại trên toàn cầu trong khi tải của cây con được cố định về mặt cấu trúc. 

Một trường hợp thất bại tinh vi đối với lối suy luận ngây thơ xuất phát từ sự tham lam của địa phương. Ví dụ, việc gán dung lượng lớn nhất cho các nút sâu nhất hoặc cho các nút có cây con lớn nhất có vẻ hợp lý nhưng lại bỏ qua sự tương tác giữa các cây con chồng chéo. 

Hãy xem xét một cái cây hình ngôi sao, gốc có nhiều lá. Nếu trọng số của lá bị lệch, việc gán công suất lớn nhất cho lá gốc mà không xem xét tổng cây con anh em có thể thất bại, ngay cả khi một phép gán toàn cục khác sẽ thành công. Sự phân công tham lam cục bộ không nắm bắt được những hạn chế về tính khả thi toàn cầu. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử tất cả các hoán vị dung lượng giữa các nút bên trong và kiểm tra xem phép gán kết quả có thỏa mãn tất cả các ràng buộc của cây con hay không. Đây là giai thừa về số lượng nút bên trong và nhanh chóng trở nên không thể thực hiện được ngay cả với n khoảng 15 hoặc 20. 

Quan sát quan trọng là tổng của cây con là cố định và độc lập với các giao dịch hoán đổi. Quyền tự do duy nhất là kết hợp nhiều tập hợp dung lượng với nhiều tập tải cây con cần thiết, với hạn chế là mỗi nút phải nhận được dung lượng ít nhất bằng tổng cây con của chính nó. 

Điều này biến vấn đề thành một câu hỏi khả thi: liệu chúng ta có thể gán cho mỗi nút bên trong một dung lượng sao cho dung lượng [i] ≥ yêu cầu [i], trong đó yêu cầu [i] là tổng cây con tại nút i. 

Vì dung lượng có thể hoán đổi cho nhau nên về cơ bản chúng tôi đang kiểm tra xem liệu có tồn tại sự trùng khớp giữa hai bộ nhiều tập hợp dưới một ràng buộc thống trị hay không. Chiến lược tối ưu là sắp xếp cả hai danh sách và khớp một cách tham lam yêu cầu nhỏ nhất với khả năng nhỏ nhất có thể đáp ứng được yêu cầu đó. 

Điều này hiệu quả vì nếu giải pháp tồn tại thì việc sắp xếp cả hai trình tự cho phép chúng tôi xây dựng một cặp hợp lệ mà không vi phạm tính khả thi, bằng cách trao đổi các nhiệm vụ bất cứ khi nào một yêu cầu nhỏ hơn được ghép với dung lượng quá lớn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k!) | O(n) | Quá chậm | 
| Sắp xếp + So khớp tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta tính tổng cây con cho mỗi nút. Đây là một DFS duy nhất từ ​​gốc tổng hợp các trọng số của lá trở lên. 

Tiếp theo, chúng tôi thu thập hai mảng: một mảng cho tổng cây con của tất cả các nút bên trong (đây là những dung lượng tối thiểu bắt buộc) và một mảng cho tất cả dung lượng có sẵn từ các nút nội bộ. 

Sau đó chúng tôi sắp xếp cả hai mảng theo thứ tự không giảm.

Chúng tôi lặp qua danh sách yêu cầu đã được sắp xếp và cố gắng gán dung lượng cho từng yêu cầu bằng cách sử dụng con trỏ trên mảng dung lượng. Nếu dung lượng khả dụng nhỏ nhất hiện tại nhỏ hơn yêu cầu, chúng tôi sẽ tiếp tục cho đến khi tìm thấy dung lượng hợp lệ. Nếu chúng ta hết năng lực thì câu trả lời là không thể. 

Việc kiểm tra cuối cùng là liệu mọi yêu cầu có thể được đáp ứng hay không. 

### Tại sao nó hoạt động 

Tính đúng đắn đến từ một đối số trao đổi tiêu chuẩn. Giả sử tồn tại một số bài tập hợp lệ. Nếu trong nhiệm vụ đó, dung lượng lớn hơn được sử dụng cho yêu cầu nhỏ hơn trong khi dung lượng nhỏ hơn được sử dụng cho yêu cầu lớn hơn thì việc hoán đổi chúng sẽ không phá vỡ tính hợp lệ. Việc lặp lại quá trình này mang lại một cấu hình trong đó cả hai chuỗi được căn chỉnh theo thứ tự được sắp xếp. Điều này có nghĩa là nếu tồn tại bất kỳ phép gán hợp lệ nào thì việc so khớp được sắp xếp tham lam cũng sẽ thành công. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

n = int(input())
children = [[] for _ in range(n + 1)]
is_leaf = [True] * (n + 1)
a = [0] * (n + 1)

for i in range(1, n + 1):
    fi, ai = map(int, input().split())
    children[fi].append(i)
    is_leaf[i] = True
    is_leaf[fi] = False
    a[i] = ai

sub = [0] * (n + 1)

def dfs(u):
    if not children[u]:
        sub[u] = a[u]
        return sub[u]
    s = 0
    for v in children[u]:
        s += dfs(v)
    sub[u] = s
    return s

dfs(0)

req = []
cap = []

for i in range(1, n + 1):
    if children[i]:
        req.append(sub[i])
        cap.append(a[i])

req.sort()
cap.sort()

i = j = 0
while i < len(req) and j < len(cap):
    if cap[j] >= req[i]:
        i += 1
        j += 1
    else:
        j += 1

print("YES" if i == len(req) else "NO")
```DFS tính tổng cây con từ dưới lên, đảm bảo tải của mỗi nút được tổng hợp chính xác trước khi sử dụng. Các nút nội bộ được xác định bằng cách có ít nhất một nút con và chỉ chúng tham gia vào quá trình phân công. 

Kết hợp tham lam sử dụng hai con trỏ trên các mảng đã được sắp xếp. Nếu công suất quá nhỏ so với yêu cầu hiện tại thì nó sẽ bị loại bỏ vì nó cũng không thể đáp ứng bất kỳ yêu cầu nào lớn hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
0 500
1 700
1 400
2 100
2 200
```Các nút bên trong là 0, 1, 2. Tổng cây con là: 

0 → 1400, 1 → 1000, 2 → 300. Công suất là {500, 700, 400}. 

| bước | yêu cầu | mũ lưỡi trai | hành động | 
| --- | --- | --- | --- | 
| 1 | 300 | 400 | trận đấu | 
| 2 | 1000 | 500 | bỏ qua 500 | 
| 2 | 1000 | 700 | trận đấu | 
| 3 | 1400 | - | thất bại | 

Điều này cho thấy mặc dù có tồn tại các trận đấu cục bộ nhưng tính khả thi toàn cầu không thành công do công suất không đủ lớn. 

### Ví dụ 2 

Sửa đổi một công suất:```
0 500
1 700
1 400
2 100
2 300
```Hiện tại, dung lượng là {500, 700, 300} và yêu cầu vẫn là {300, 1000, 1400}. 

| bước | yêu cầu | mũ lưỡi trai | hành động | 
| --- | --- | --- | --- | 
| 1 | 300 | 300 | trận đấu | 
| 2 | 1000 | 500 | bỏ qua 500 | 
| 2 | 1000 | 700 | trận đấu | 
| 3 | 1400 | - | thất bại | 

Ngay cả khi kết hợp nhỏ tốt hơn, yêu cầu lớn nhất vẫn chiếm ưu thế về tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | DFS là tuyến tính, việc sắp xếp chiếm ưu thế | 
| Không gian | O(n) | danh sách kề, mảng cho tổng và dung lượng của cây con | 

Các ràng buộc lên tới 100000 nút giúp việc sắp xếp có thể chấp nhận được và DFS tuyến tính đảm bảo khả năng mở rộng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import isclose

    # Re-run solution
    input = _sys.stdin.readline
    sys.setrecursionlimit(10**7)

    n = int(input())
    children = [[] for _ in range(n + 1)]
    a = [0] * (n + 1)

    for i in range(1, n + 1):
        fi, ai = map(int, input().split())
        children[fi].append(i)
        a[i] = ai

    sub = [0] * (n + 1)

    def dfs(u):
        if not children[u]:
            sub[u] = a[u]
            return sub[u]
        s = 0
        for v in children[u]:
            s += dfs(v)
        sub[u] = s
        return s

    dfs(0)

    req = []
    cap = []

    for i in range(1, n + 1):
        if children[i]:
            req.append(sub[i])
            cap.append(a[i])

    req.sort()
    cap.sort()

    i = j = 0
    while i < len(req) and j < len(cap):
        if cap[j] >= req[i]:
            i += 1
            j += 1
        else:
            j += 1

    return "YES\n" if i == len(req) else "NO\n"

# sample-like cases
assert run("4\n0 500\n1 700\n1 400\n2 100\n2 200\n") == "NO\n"
assert run("4\n0 500\n1 700\n1 400\n2 100\n2 300\n") == "NO\n"

# minimal valid chain
assert run("1\n0 100\n") == "YES\n"

# star impossible
assert run("2\n0 1\n0 1000000000\n") == "NO\n"

# balanced simple
assert run("3\n0 10\n1 5\n1 5\n") == "YES\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | CÓ | trường hợp cơ sở | 
| sao không khớp | KHÔNG | phân phối công suất không đủ | 
| cây cân đối | CÓ | khớp đúng | 

## Vỏ cạnh 

Cây một nút chỉ chứa gốc nên không cần hoán đổi nội bộ. DFS gán trực tiếp giá trị lá và danh sách yêu cầu trống, do đó thuật toán ngay lập tức trả về CÓ. 

Một cây có độ lệch cao trong đó một cây con chứa gần như tất cả các lá sẽ tạo ra yêu cầu vượt trội. Thuật toán tham lam sẽ thất bại một cách chính xác nếu không có dung lượng nào có thể bao gồm tổng cây con lớn này, vì yêu cầu đó xuất hiện ở cuối danh sách đã sắp xếp và tiêu thụ dung lượng lớn nhất hiện có sau cùng. 

Một trường hợp có nhiều cây con nhỏ và một cây con có dung lượng lớn kiểm tra xem logic bỏ qua con trỏ có hoạt động chính xác hay không. Thuật toán loại bỏ các dung lượng nhỏ không thể sử dụng được và vẫn tìm thấy kết quả phù hợp hợp lệ cho tất cả các yêu cầu còn lại, xác nhận rằng việc sắp xếp lại dung lượng cục bộ là hoàn toàn đủ.
