---
title: "CF 41E - 3 chu kỳ"
description: "Chúng ta cần xây dựng một đồ thị vô hướng trên n thành phố sao cho không tồn tại tam giác nào. Một hình tam giác có nghĩa là ba thành phố riêng biệt nơi mỗi cặp được kết nối trực tiếp."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 41
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 40 (Div. 2)"
rating: 1900
weight: 41
solve_time_s: 108
verified: false
draft: false
---
[CF 41E - 3 chu kỳ](https://codeforces.com/problemset/problem/41/E) 

**Xếp hạng:** 1900 
**Tags:** thuật toán xây dựng, đồ thị, tham lam 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Ta cần xây dựng đồ thị vô hướng trên`n`những thành phố không có hình tam giác nào tồn tại. Một hình tam giác có nghĩa là ba thành phố riêng biệt nơi mỗi cặp được kết nối trực tiếp. Trong số tất cả các biểu đồ không có 3 chu trình, chúng ta muốn một biểu đồ có số đường tối đa có thể và chúng ta cũng phải in một công trình hợp lệ. 

Đầu vào chỉ chứa số lượng thành phố. Đầu ra trước tiên phải in số lượng đường tối đa, sau đó liệt kê từng con đường. 

Sự ràng buộc`n ≤ 100`đủ nhỏ để hầu hết mọi cách xây dựng thời gian đa thức đều có thể thực hiện được một cách thoải mái. Thậm chí một`O(n^3)`bước xác minh sẽ ổn vì`100^3 = 1,000,000`hoạt động. Thách thức thực sự không phải là hiệu suất, mà là khám phá cấu trúc đồ thị cực trị giúp tối đa hóa số cạnh trong khi tránh các hình tam giác. 

Một lỗi phổ biến là cố gắng thêm các cạnh một cách tham lam trong khi kiểm tra xem một hình tam giác có xuất hiện hay không. Cách tiếp cận đó có thể bị mắc kẹt trong các cấu hình tối ưu cục bộ nhưng không tối ưu toàn cục. 

Ví dụ, giả sử`n = 4`. 

Một công trình tham lam bất cẩn có thể xây dựng:```
1-2
2-3
3-4
```Đồ thị này chỉ có 3 cạnh. Nhưng câu trả lời tối ưu là 4 cạnh:```
1-3
1-4
2-3
2-4
```Đây là một biểu đồ lưỡng cực hoàn chỉnh với các phân vùng`{1,2}`Và`{3,4}`. 

Một trường hợp tế nhị khác là kỳ quặc`n`. 

Vì`n = 5`, việc chia thành hai nửa bằng nhau là không thể. Nếu chúng ta chọn phân vùng có kích thước`2`Và`3`, số cạnh trở thành`2 × 3 = 6`. 

Việc triển khai có lỗi có thể thử phân vùng không chính xác`1`Và`4`, chỉ tạo ra 4 cạnh. Kích thước phân vùng tối ưu phải cân bằng nhất có thể. 

Những trường hợp nhỏ nhất cũng quan trọng. 

Vì`n = 1`, đồ thị không có cạnh. 

Vì`n = 2`, chính xác có thể có một cạnh. 

Cấu trúc kết nối mù quáng tất cả các cặp trên hai phân vùng vẫn phải xử lý chính xác các phân vùng trống. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Xem xét mọi tập con có thể có của các cạnh trong số`n(n-1)/2`các cạnh ứng cử viên, kiểm tra xem biểu đồ có chứa hình tam giác hay không và giữ biểu đồ hợp lệ lớn nhất. 

Điều này hiệu quả vì định nghĩa rất rõ ràng: chúng ta chỉ cần loại bỏ các đồ thị chứa bất kỳ 3 chu trình nào. Bản thân việc phát hiện tam giác có thể được thực hiện trong`O(n^3)`bằng cách kiểm tra từng ba đỉnh. 

Vấn đề là số lượng đồ thị. có`2^(n(n-1)/2)`đồ thị vô hướng có thể có. Ngay cả đối với`n = 20`, điều này trở nên lớn về mặt thiên văn. Tìm kiếm toàn diện là hoàn toàn không thể. 

Một ý tưởng tham lam hợp lý hơn là thêm từng cạnh một trong khi tránh các hình tam giác. Điều đó vẫn không đảm bảo tính tối ưu vì các quyết định cục bộ có thể cản trở các lợi thế trong tương lai. 

Quan sát quan trọng là kết quả đồ thị cực trị cổ điển: đồ thị không có tam giác với số cạnh tối đa luôn là đồ thị lưỡng cực hoàn chỉnh với hai phần cân bằng nhất có thể. 

Tại sao lưỡng đảng lại quan trọng ở đây? Bất kỳ tam giác nào cũng cần có ba đỉnh mà mỗi cặp đều được kết nối. Trong đồ thị lưỡng cực, các đỉnh được chia thành hai nhóm và các cạnh chỉ đi qua các nhóm. Một tam giác sẽ cần hai đỉnh thuộc cùng một nhóm, nhưng các cạnh cùng nhóm không tồn tại. Vì vậy, đồ thị lưỡng cực tự động không có hình tam giác. 

Trong các đồ thị hai bên, nếu một phân vùng có kích thước`a`và cái kia có kích thước`b`, sau đó mọi cặp chéo có thể được kết nối, mang lại`a × b`các cạnh. 

Từ`a + b = n`, sản phẩm`a × b`được tối đa hóa khi hai giá trị càng gần nhau càng tốt. Vì vậy, kích thước phân vùng tối ưu là:```
⌊n/2⌋ and ⌈n/2⌉
```Sau đó, chúng ta chỉ cần kết nối mọi đỉnh từ phần đầu tiên với mọi đỉnh từ phần thứ hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(n²) · n³) | O(n²) | Quá chậm | 
| Tối ưu | O(n²) | O(1) không bao gồm đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị`n`. 
2. Chia các đỉnh thành hai nhóm càng đều nhau càng tốt. 

Cho phép:```
left = n // 2
right = n - left
```Nhóm đầu tiên sẽ chứa các đỉnh`1...left`, và nhóm thứ hai sẽ chứa các đỉnh`left+1...n`. 
3. Tạo một danh sách các cạnh trống. 
4. Với mọi đỉnh`i`trong nhóm đầu tiên, kết nối nó với mọi đỉnh`j`trong nhóm thứ hai. 

Điều này tạo ra một biểu đồ lưỡng cực hoàn chỉnh. 
5. In số cạnh. 
6. In mọi cạnh được lưu trữ. 

### Tại sao nó hoạt động 

Đồ thị là đồ thị lưỡng cực nên không có cạnh nào tồn tại giữa các đỉnh trong cùng một phân vùng. Bất kỳ tam giác nào cũng cần ít nhất một cạnh như vậy, điều này là không thể. Vì vậy, việc xây dựng luôn không có hình tam giác. 

Bây giờ hãy xem xét số cạnh. Nếu kích thước phân vùng là`a`Và`b`, đồ thị chứa`a × b`các cạnh. Từ`a + b = n`, tích này lớn nhất khi hai số cân bằng nhất có thể. Công trình của chúng tôi sử dụng chính xác các kích thước phân vùng đó, vì vậy không có đồ thị không có tam giác nào có thể chứa nhiều cạnh hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

left = n // 2

edges = []

for i in range(1, left + 1):
    for j in range(left + 1, n + 1):
        edges.append((i, j))

print(len(edges))

for u, v in edges:
    print(u, v)
```Mã trực tiếp thực hiện bằng chứng mang tính xây dựng. 

Biến`left`lưu trữ kích thước của phân vùng đầu tiên. Phân vùng thứ hai tự động chứa các đỉnh còn lại. 

Các vòng lặp lồng nhau tạo ra mọi cạnh chéo giữa hai phân vùng. Vì mỗi cặp được thêm đúng một lần nên các cạnh trùng lặp không thể xuất hiện. 

Phạm vi đỉnh được lựa chọn cẩn thận để tránh sai sót từng cái một. Phân vùng đầu tiên là:```
1 ... left
```Phân vùng thứ hai là:```
left + 1 ... n
```Điều này hoạt động chính xác cho cả chẵn và lẻ`n`. 

Ví dụ, khi`n = 5`:```
left = 2
first group  = {1, 2}
second group = {3, 4, 5}
```Tất cả sáu cạnh chéo được thêm vào. 

Việc sử dụng bộ nhớ rất nhỏ vì nhiều nhất`2500`các cạnh tồn tại khi`n = 100`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
```Các phân vùng trở thành:```
{1}
{2, 3}
```| Bước | Hiện tại tôi | Hiện tại j | Đã thêm cạnh | Tổng số cạnh | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | - | - | - | 0 | 
| 1 | 1 | 2 | (1,2) | 1 | 
| 2 | 1 | 3 | (1,3) | 2 | 

Đầu ra cuối cùng:```
2
1 2
1 3
```Dấu vết này cho thấy mọi cạnh đều đi qua ranh giới phân vùng. Vì đỉnh`2`Và`3`không được kết nối, không có hình tam giác tồn tại. 

### Ví dụ 2 

đầu vào:```
5
```Các phân vùng trở thành:```
{1, 2}
{3, 4, 5}
```| Bước | Hiện tại tôi | Hiện tại j | Đã thêm cạnh | Tổng số cạnh | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | - | - | - | 0 | 
| 1 | 1 | 3 | (1,3) | 1 | 
| 2 | 1 | 4 | (1,4) | 2 | 
| 3 | 1 | 5 | (1,5) | 3 | 
| 4 | 2 | 3 | (2,3) | 4 | 
| 5 | 2 | 4 | (2,4) | 5 | 
| 6 | 2 | 5 | (2,5) | 6 | 

Đầu ra cuối cùng:```
6
1 3
1 4
1 5
2 3
2 4
2 5
```Ví dụ này chứng minh tại sao việc phân vùng cân bằng lại quan trọng. MỘT`2 × 3`chia cho 6 cạnh, lớn hơn bất kỳ sự thay thế không cân bằng nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi cặp chéo giữa hai phân vùng được xử lý một lần | 
| Không gian | O(n²) | Danh sách cạnh có thể chứa tối đa khoảng n2/4 cạnh | 

Với`n ≤ 100`, số cạnh được tạo tối đa chỉ là`2500`. Giải pháp dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())

    left = n // 2

    edges = []

    for i in range(1, left + 1):
        for j in range(left + 1, n + 1):
            edges.append((i, j))

    print(len(edges))

    for u, v in edges:
        print(u, v)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("3\n") == (
    "2\n"
    "1 2\n"
    "1 3\n"
), "sample 1"

# minimum size
assert run("1\n") == "0\n", "single vertex"

# two vertices
assert run("2\n") == (
    "1\n"
    "1 2\n"
), "single edge"

# odd n
assert run("5\n") == (
    "6\n"
    "1 3\n"
    "1 4\n"
    "1 5\n"
    "2 3\n"
    "2 4\n"
    "2 5\n"
), "balanced odd partition"

# even n
assert run("4\n") == (
    "4\n"
    "1 3\n"
    "1 4\n"
    "2 3\n"
    "2 4\n"
), "balanced even partition"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`0`cạnh | Xử lý chính xác biểu đồ trống | 
|`2`| Một cạnh | Đồ thị không tầm thường nhỏ nhất | 
|`4`| 4 cạnh | Cấu trúc lưỡng cực cân bằng đúng đắn cho n chẵn | 
|`5`| 6 cạnh | Xây dựng lưỡng cực cân bằng đúng cho n lẻ | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
```Thuật toán tính toán:```
left = 0
```Vì vậy phân vùng đầu tiên trống và phân vùng thứ hai chứa đỉnh`1`. Các vòng lặp lồng nhau không bao giờ thực thi, tạo ra các cạnh bằng 0:```
0
```Điều này đúng vì không có con đường nào có thể tồn tại. 

Bây giờ hãy xem xét:```
2
```Các phân vùng trở thành:```
{1}
{2}
```Thuật toán thêm chính xác một cạnh:```
1 2
```Không có tam giác nào có thể tồn tại chỉ với hai đỉnh. 

Trường hợp kích thước lẻ thì tinh tế hơn:```
5
```Các phân vùng trở thành kích thước`2`Và`3`. Thuật toán tạo ra`2 × 3 = 6`các cạnh. 

Triển khai có lỗi khi sử dụng phân vùng`1`Và`4`sẽ chỉ tạo ra 4 cạnh:```
1-2
1-3
1-4
1-5
```Đồ thị đó không có tam giác nhưng không tối đa. Sự phân chia cân bằng là cần thiết để tối đa hóa sản phẩm. 

Cuối cùng, hãy xem xét:```
6
```Các phân vùng được cân bằng hoàn hảo:```
{1,2,3}
{4,5,6}
```Đồ thị chứa`3 × 3 = 9`các cạnh. Bất kỳ nỗ lực nào để thêm một cạnh vào bên trong một trong hai phân vùng sẽ ngay lập tức tạo ra một hình tam giác. 

Ví dụ, thêm cạnh`(1,2)`tạo ra:```
1-2-4-1
```bởi vì cả hai`1`Và`2`đã được kết nối với`4`. Điều này xác nhận tại sao cấu trúc lưỡng cực hoàn chỉnh là tối đa.
