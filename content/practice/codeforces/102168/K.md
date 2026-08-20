---
title: "CF 102168K - \u041e\u0431\u0445\u043e\u0434 \u0434\u0435\u0440\u0435\u0432\u0430"
description: "Chúng ta có một cây có (n) đỉnh và (n-1) cạnh. Chúng ta cần đi qua mỗi cạnh đúng một lần. Cuộc đi bộ có thể bắt đầu ở bất kỳ đỉnh nào và bất cứ khi nào cuộc đi bộ hiện tại không thể tiếp tục dọc theo một cạnh không được sử dụng, chúng ta có thể dịch chuyển tức thời đến bất kỳ đỉnh nào khác và tiếp tục từ đó."
date: "2026-08-19T07:29:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102168
codeforces_index: "K"
codeforces_contest_name: "\u041b\u0438\u0447\u043d\u044b\u0439 \u0447\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0421\u0430\u043c\u0430\u0440\u0441\u043a\u043e\u0433\u043e \u0443\u043d\u0438\u0432\u0435\u0440\u0441\u0438\u0442\u0435\u0442\u0430 \u0441\u0440\u0435\u0434\u0438 \u043d\u043e\u0432\u0438\u0447\u043a\u043e\u0432 2018-2019"
rating: 0
weight: 102168
solve_time_s: 107
verified: true
draft: false
---

[CF 102168K - \u041e\u0431\u0445\u043e\u0434 \u0434\u0435\u0440\u0435\u0432\u0430](https://codeforces.com/problemset/problem/102168/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có (n) đỉnh và (n-1) cạnh. Chúng ta cần đi qua mỗi cạnh đúng một lần. Cuộc đi bộ có thể bắt đầu ở bất kỳ đỉnh nào và bất cứ khi nào cuộc đi bộ hiện tại không thể tiếp tục dọc theo một cạnh không được sử dụng, chúng ta có thể dịch chuyển tức thời đến bất kỳ đỉnh nào khác và tiếp tục từ đó. Mục tiêu là giảm thiểu số lần dịch chuyển tức thời. 

Một phần duy nhất không bị gián đoạn của quá trình truyền tải là một đường nhỏ: nó sử dụng mọi cạnh trong phần đó đúng một lần, mặc dù các đỉnh có thể được viếng thăm nhiều lần. Do đó, vấn đề có thể được xem như là phân chia tất cả các cạnh của cây thành càng ít đường đi càng tốt. Nếu các cạnh có thể được bao phủ bởi (k) đường dẫn, chúng ta cần dịch chuyển chính xác (k-1) vì đường đầu tiên có thể được bắt đầu tự do. 

Đầu vào chứa (n), theo sau là (n-1) cặp số đỉnh mô tả các cạnh của cây. Đầu ra là số lần dịch chuyển tối thiểu. 

Ràng buộc (n \le 200000) loại trừ mọi thứ kiểm tra nhiều tập hợp con cạnh theo cấp số nhân và thậm chí nhiều thuật toán bậc hai quá chậm trong giới hạn hai giây. Giải pháp tuyến tính hoặc (O(n\log n)) là mục tiêu tự nhiên. Vì đồ thị là một cây nên số cạnh của nó đã là (O(n)), do đó biểu diễn danh sách kề sẽ cung cấp bộ nhớ tuyến tính. 

Trường hợp ranh giới đầu tiên là cây có một đỉnh và không có cạnh:```
1
```Câu trả lời đúng là`0`. Không có gì để vượt qua, vì vậy không cần dịch chuyển tức thời. Một công thức mù quáng giả định ít nhất một dấu vết và trừ đi một dấu vết có thể tạo ra câu trả lời phủ định. 

Trường hợp quan trọng thứ hai là một đường dẫn đơn giản:```
4
1 2
2 3
3 4
```Câu trả lời đúng là`0`, bởi vì chúng ta có thể đơn giản đi bộ từ đỉnh 1 đến 2 và 3 đến 4. Một cách tiếp cận bất cẩn có thể tính mọi đỉnh có bậc khác với hai là yêu cầu dịch chuyển tức thời, nhưng chỉ có số đỉnh bậc lẻ mới quan trọng. 

Cây phân nhánh cho kết quả khác:```
4
1 2
1 3
1 4
```Câu trả lời đúng là`1`. Đỉnh 1 và cả 3 lá đều có bậc lẻ nên có 4 đỉnh bậc lẻ. Một đường đi liên tục chỉ có thể có hai điểm cuối lẻ, vì vậy cần có ít nhất hai đường đi, nghĩa là một đường dịch chuyển. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp có thể coi các cạnh là các đối tượng phải được chia thành các đường nhỏ. Người ta có thể liệt kê mọi tập hợp con của các cạnh, xác định xem tập hợp con đó có thể được duyệt dưới dạng một đường hay không, sau đó sử dụng lập trình động trên các tập hợp con để tìm số lượng đường nhỏ nhất bao phủ tất cả các cạnh. Điều này đúng vì mọi khả năng phân tách thành các vệt đều xuất hiện trong số các tập hợp con đó. Tuy nhiên, một cây có (n) đỉnh có (n-1) cạnh, cho ra (2^{n-1}) tập hợp cạnh có thể có. Ngay cả khi bỏ qua công việc bổ sung cần thiết để xác thực từng tập hợp con, đó đã là khoảng (2^{199999}) trạng thái ở giới hạn tối đa, vì vậy cách tiếp cận này gần như không thể thực hiện được ngay lập tức. 

Brute-force hoạt động vì nó tìm kiếm rõ ràng thông qua tất cả các cách có thể để phân chia đường truyền. Quan sát quan trọng là cây có cấu trúc cấp độ rất cứng nhắc. Đối với bất kỳ đồ thị nào, một đường có 0 hoặc hai đỉnh bậc lẻ trong các cạnh thuộc đường đó. Khi kết hợp một số đường nhỏ, mọi đỉnh bậc lẻ của biểu đồ ban đầu phải là điểm cuối của một trong các đường đó. Trong một cây, không có chu trình nào có thể thay đổi cách tính toán này và mọi tập hợp đỉnh bậc lẻ đều có thể được ghép nối để tạo nên các đường đi cần thiết. 

Cho cây chứa (O) đỉnh bậc lẻ. Mỗi đường đi đóng góp tối đa hai điểm cuối lẻ, do đó cần có ít nhất (O/2) đường đi. Có thể đạt được giới hạn dưới này: ghép các đỉnh bậc lẻ và phân tách các cạnh của cây thành các đường nhỏ với các điểm cuối đó. Do đó số lượng đường nhỏ nhất là (O/2), miễn là có ít nhất một cạnh. 

Số lần dịch chuyển cần thiết ít hơn một lần so với số đường mòn. Vì vậy, đối với một cây không rỗng, 

[ 
\text{answer}=\frac{O}{2}-1. 
] 

Với (n=1), không có cạnh nào và câu trả lời là 0. 

Có một cách thậm chí còn đơn giản hơn để viết kết quả cho mọi cây có ít nhất hai đỉnh. Một cây luôn có ít nhất hai lá nên nó luôn có ít nhất hai đỉnh bậc lẻ. Ta chỉ cần đếm xem có bao nhiêu đỉnh bậc lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n2^n)) | (O(2^n)) | Quá chậm | 
| Đếm độ lẻ | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cây và duy trì bậc của mỗi đỉnh. Mỗi cạnh đầu vào sẽ tăng bậc của cả hai điểm cuối của nó vì cạnh đó đóng góp một cạnh phụ cho mỗi đỉnh. 
2. Đếm các đỉnh có bậc lẻ. Gọi số này (lẻ). Bổ đề bắt tay đảm bảo rằng (số lẻ) là số chẵn. 
3. Nếu (n=1), xuất ra`0`. Đồ thị không có cạnh nên không thể thực hiện quá trình truyền tải. 
4. Ngược lại, số vệt liên tục tối thiểu là (lẻ/2). Mỗi đường có thể chiếm tối đa hai điểm cuối lẻ, điểm này đưa ra giới hạn dưới và cấu trúc của đồ thị vô hướng cho phép đạt được giới hạn này. 
5. Vì đường đầu tiên không cần dịch chuyển tức thời trước khi nó bắt đầu, nên việc nối các đường (lẻ/2) yêu cầu dịch chuyển chính xác (lẻ/2-1). Xuất giá trị đó. 

### Tại sao nó hoạt động 

Xem xét bất kỳ đường truyền hợp lệ nào và phân chia nó ở mỗi lần dịch chuyển. Mỗi phần kết quả là một đường nhỏ sử dụng một tập hợp các cạnh rời nhau. Trong một đường, tất cả các đỉnh bên trong đều có bậc chẵn trong đường đó, trong khi các điểm cuối của nó có bậc lẻ. Do đó, một đường nhỏ có thể chiếm tối đa hai đỉnh bậc lẻ của cây ban đầu, vì vậy các đường nhỏ (lẻ/2) là không thể tránh khỏi. 

Đối với một cái cây, giới hạn dưới này có thể đạt được. Tổng quát hơn, mọi đồ thị được kết nối với các đỉnh bậc lẻ (O>0) có thể có các cạnh của nó được phân tách thành các vệt (O/2) bằng cách ghép các đỉnh lẻ và áp dụng phân tách Euler-trail. Một cây được kết nối và không có sự phức tạp nào từ các chu trình, do đó, giới hạn tương tự sẽ được áp dụng trực tiếp. Do đó, số lượng đường đi tối thiểu chính xác là (lẻ/2) và số lần dịch chuyển tối thiểu là chính xác (lẻ/2-1). Trường hợp (n=1) được xử lý riêng vì nó không chứa dấu vết nào cả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    degree = [0] * n

    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        degree[u] += 1
        degree[v] += 1

    if n == 1:
        print(0)
        return

    odd = sum(d & 1 for d in degree)
    print(odd // 2 - 1)

if __name__ == "__main__":
    solve()
```Mảng độ đủ để giải quyết vấn đề nên không cần lưu trữ danh sách kề. Đối với mọi cạnh ((u,v)), mã tăng dần`degree[u]`Và`degree[v]`. Sau khi tất cả (n-1) cạnh đã được xử lý, tính chẵn lẻ của từng bậc được biết. 

biểu hiện`d & 1`là một khi`d`là số lẻ và bằng 0 khi nó chẵn, do đó tổng của nó sẽ đếm chính xác các đỉnh bậc lẻ. Bởi vì số đỉnh bậc lẻ trong bất kỳ đồ thị vô hướng nào đều là số chẵn,`odd // 2`là một số nguyên. 

Trường hợp đặc biệt`n == 1`phải được xử lý trước khi áp dụng công thức. Trong trường hợp đó`odd`sẽ bằng không, và`odd // 2 - 1`sẽ sản xuất không chính xác`-1`. 

Không có đệ quy trong quá trình triển khai, điều này tránh được các giới hạn về độ sâu đệ quy của Python đối với các cây có hình dạng là một đường dẫn dài. Số học duy nhất liên quan đến nhiều nhất các giá trị (n), do đó, tràn số nguyên không phải là vấn đề trong Python hoặc trong các triển khai có chiều rộng cố định thông thường. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là đường dẫn```
4
1 2
2 3
3 4
```Trạng thái độ sau khi xử lý từng cạnh là: 

| Cạnh đã được xử lý | bằng cấp[1] | bằng cấp[2] | bằng cấp[3] | bằng cấp[4] | Đỉnh lẻ | 
| --- | --- | --- | --- | --- | --- | 
| không | 0 | 0 | 0 | 0 | 0 | 
| 1-2 | 1 | 1 | 0 | 0 | 2 | 
| 2-3 | 1 | 2 | 1 | 0 | 2 | 
| 3-4 | 1 | 2 | 2 | 1 | 2 | 

Có hai đỉnh lẻ, 1 và 4. Do đó, cây cần (2/2=1) đường mòn và (1-1=0) dịch chuyển tức thời. Việc truyền tải có thể đơn giản là`1 -> 2 -> 3 -> 4`. 

### Mẫu 2 

Đầu vào là ngôi sao ba cạnh:```
4
1 2
1 3
1 4
```Trạng thái độ là: 

| Cạnh đã được xử lý | bằng cấp[1] | bằng cấp[2] | bằng cấp[3] | bằng cấp[4] | Đỉnh lẻ | 
| --- | --- | --- | --- | --- | --- | 
| không | 0 | 0 | 0 | 0 | 0 | 
| 1-2 | 1 | 1 | 0 | 0 | 2 | 
| 1-3 | 2 | 1 | 1 | 0 | 2 | 
| 1-4 | 3 | 1 | 1 | 1 | 4 | 

Bốn đỉnh đều có bậc lẻ. Một đường nhỏ có thể bao phủ tối đa hai điểm cuối lẻ này, vì vậy cần có hai đường nhỏ. Ví dụ: một con đường có thể đi qua`2 -> 1 -> 3`, sau đó chúng ta dịch chuyển đến đỉnh 4 và đi qua cạnh còn lại`4 -> 1`. Do đó câu trả lời là`2 - 1 = 1`. 

Ví dụ này chứng tỏ tại sao chỉ tìm kiếm một đường Euler duy nhất là không đủ. Toàn bộ cây không có chính xác 0 hoặc 2 đỉnh bậc lẻ, nhưng nó có thể được chia thành số lượng đường nhỏ nhất có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Có (n-1) cạnh cần xử lý và (n) độ cần kiểm tra. | 
| Không gian | (O(n)) | Mảng độ chứa một số nguyên trên mỗi đỉnh. | 

Với (n\le 200000), thuật toán chỉ thực hiện một lượng công việc không đổi trên mỗi cạnh và đỉnh. Nó tránh được cả chi phí truyền tải đồ thị và liệt kê trạng thái hàm mũ, do đó, nó phù hợp thoải mái với giới hạn hai giây và 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run the solution on an input string and return its output
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    degree = [0] * n

    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        degree[u] += 1
        degree[v] += 1

    if n == 1:
        print(0)
        return

    odd = sum(d & 1 for d in degree)
    print(odd // 2 - 1)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run(
    """4
1 2
2 3
3 4
"""
) == "0", "sample 1"

# Provided sample 2
assert run(
    """4
1 2
1 3
1 4
"""
) == "1", "sample 2"

# Minimum-size tree
assert run(
    """1
"""
) == "0", "single vertex"

# Three-vertex path
assert run(
    """3
1 2
2 3
"""
) == "0", "path has one continuous trail"

# Five-vertex star
assert run(
    """5
1 2
1 3
1 4
1 5
"""
) == "1", "four leaves give two trails"

# Larger branching tree
assert run(
    """7
1 2
1 3
2 4
2 5
3 6
3 7
"""
) == "2", "six odd-degree vertices require three trails"

# Maximum-size path
n = 200000
path = [str(n)]
for i in range(1, n):
    path.append(f"{i} {i + 1}")
assert run("\n".join(path) + "\n") == "0", "maximum-size path"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`0`| Bộ cạnh trống và hộp đựng có kích thước tối thiểu đặc biệt | 
| Đường dẫn ba đỉnh |`0`| Chính xác là hai điểm cuối bậc lẻ | 
| Sao năm đỉnh |`1`| Một số lá độ lẻ và chuyển đổi từ đường mòn sang dịch chuyển tức thời | 
| Cây phân nhánh cân bằng bảy đỉnh |`2`| Sáu đỉnh bậc lẻ và công thức tổng quát | 
| Đường đi có 200000 đỉnh |`0`| Kích thước đầu vào tối đa và hiệu suất thời gian tuyến tính | 

## Vỏ cạnh 

Đối với một đỉnh duy nhất,```
1
```bậc của đỉnh duy nhất bằng 0, tức là chẵn, vì vậy`odd = 0`. Thuật toán có tính chất rõ ràng`n == 1`chi nhánh và lợi nhuận`0`. Điều này tránh việc hiểu sự vắng mặt của các cạnh là một vệt đòi hỏi phải trừ đi một cạnh từ số lượng vệt. 

Đối với một con đường,```
4
1 2
2 3
3 4
```độ là (1,2,2,1). Chỉ có đỉnh 1 và 4 là lẻ, cho`odd = 2`. Công thức tạo ra`2 // 2 - 1 = 0`. Toàn bộ tập hợp cạnh đã là một vệt nên không cần dịch chuyển tức thời. 

Đối với một ngôi sao,```
4
1 2
1 3
1 4
```độ là (3,1,1,1), nên cả bốn đỉnh đều là số lẻ. Thuật toán được`odd = 4`, tính toán hai đường dẫn cần thiết và trả về`4 // 2 - 1 = 1`. Một sự phân hủy có thể là`2 -> 1 -> 3`Và`4 -> 1`, với một lần dịch chuyển giữa chúng. 

Một cái cây có thể có nhiều lá và đây là lúc các phương pháp tiếp cận chỉ dựa trên việc tìm ra con đường dài nhất có thể thất bại. Ví dụ,```
7
1 2
1 3
2 4
2 5
3 6
3 7
```có trình tự mức độ (2,3,3,1,1,1,1). Các đỉnh 2, 3, 4, 5, 6 và 7 là các đỉnh lẻ nên`odd = 6`. Ba con đường là cần thiết và đủ, mang lại`3 - 1 = 2`dịch chuyển tức thời. Câu trả lời phụ thuộc vào tất cả các đỉnh bậc lẻ, không chỉ ở đường kính hay số lượng lá. 

Cuối cùng, một đường dẫn chứa 200000 đỉnh nhấn mạnh vào hình dạng thực hiện hơn là công thức toán học. Mỗi đỉnh bên trong có bậc hai, trong khi hai điểm cuối có bậc một. Thuật toán thực hiện cập nhật cạnh (199999) và vượt qua các giá trị 200000 độ, tạo ra`0`. Vì nó không sử dụng DFS đệ quy và chỉ lưu trữ mảng độ nên nó vẫn an toàn ngay cả đối với cây có độ sâu tối đa này.
