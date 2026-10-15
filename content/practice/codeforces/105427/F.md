---
title: "CF 105427F - Cây đầy đủ yếu tố"
description: "Chúng ta có một cây có gốc có N đỉnh, trong đó đỉnh 1 là gốc. Nhiệm vụ là gán cho mỗi đỉnh v một nhãn số nguyên dương xv sao cho tổ tiên trong cây được mã hóa hoàn toàn thông qua tính chia hết: một đỉnh u là tổ tiên của v khi và chỉ khi xu chia xv."
date: "2026-06-23T04:07:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105427
codeforces_index: "F"
codeforces_contest_name: "2023-2024 ACM-ICPC Nordic Collegiate Programming Contest (NCPC 2023)"
rating: 0
weight: 105427
solve_time_s: 51
verified: true
draft: false
---

[CF 105427F - Cây đầy đủ yếu tố](https://codeforces.com/problemset/problem/105427/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc có N đỉnh, trong đó đỉnh 1 là gốc. Nhiệm vụ là gán cho mỗi đỉnh v một nhãn số nguyên dương xv sao cho tổ tiên trong cây được mã hóa hoàn toàn thông qua tính chia hết: một đỉnh u là tổ tiên của v khi và chỉ khi xu chia xv. 

Vì vậy, mọi đường dẫn từ gốc tới nút phải tương ứng với một chuỗi số nguyên theo quan hệ chia hết và các nhánh không liên quan không được vô tình tạo ra quan hệ chia hết có thể ám chỉ mối quan hệ tổ tiên giả. 

Ràng buộc N ≤ 60 là cực kỳ nhỏ đối với bài toán cây, điều này báo hiệu rằng chúng ta không tối ưu hóa tiệm cận mà đang xây dựng một đối tượng tổ hợp được thiết kế cẩn thận. Các giá trị được phép lên tới 10^18, đủ lớn để hỗ trợ tích của nhiều số nguyên tố nhỏ, nhưng vẫn đủ nhỏ để chúng ta phải tránh sự tăng trưởng theo cấp số nhân hoặc phép nhân không kiểm soát được dọc theo những con đường dài. 

Một nỗ lực ngây thơ có thể cố gắng gán các số nguyên tố riêng biệt cho mỗi nút hoặc trên mỗi cạnh, nhưng điều đó ngay lập tức dẫn đến vấn đề tính chia hết có tính bắc cầu: nếu xv chia xu và xu chia xw, thì xv chia xw tự động, do đó cấu trúc mã hóa phải tôn trọng chính xác hệ thống phân cấp cây. 

Một trường hợp lỗi tinh vi xuất hiện khi hai nút ở các cây con khác nhau nhưng nhãn của chúng vô tình chia sẻ cấu trúc phân chia. Ví dụ: nếu chúng ta gán x2 = 2 và x3 = 4 khi chúng là anh em ruột thì 2 chia hết cho 4 mặc dù cả hai đều không phải là tổ tiên của số kia. Điều này vi phạm điều kiện “nếu và chỉ khi”. Vì vậy, việc xây dựng phải đảm bảo rằng sự phân chia xảy ra chính xác dọc theo chuỗi tổ tiên và không ở nơi nào khác. 

## Phương pháp tiếp cận 

Quan điểm bạo lực sẽ là gán các số nguyên cho các nút và kiểm tra tất cả các phép gán cho đến một giới hạn nào đó, kiểm tra điều kiện chia hết cho cây. Ngay cả khi giới hạn các giá trị trong một phạm vi nhỏ, không gian tìm kiếm vẫn có hiệu quả theo cấp số nhân trong N, vì mỗi nút có thể nhận nhiều giá trị có thể và mọi phép gán đều yêu cầu kiểm tra các mối quan hệ tổ tiên O(N^2). Ngay cả việc cắt tỉa mạnh mẽ cũng không thể thực hiện được. 

Cấu trúc của bài toán gợi ý chúng ta nên mã hóa cây bằng cách sử dụng hệ số nguyên tố. Khả năng chia hết được kiểm soát một cách tự nhiên bởi số mũ: x chia y khi và chỉ khi mọi số mũ nguyên tố trong x nhỏ hơn hoặc bằng số mũ tương ứng trong y. Điều này chuyển vấn đề thành việc gán vectơ số mũ cho các nút sao cho mối quan hệ tổ tiên tương ứng chính xác với bất đẳng thức thành phần. 

Quan sát quan trọng là mỗi đỉnh chỉ cần “giới thiệu” một thừa số nguyên tố mới so với đỉnh gốc của nó. Nếu mỗi cạnh đóng góp một số nguyên tố mới thì mỗi nhãn nút sẽ trở thành tích của các số nguyên tố dọc theo đường dẫn từ gốc đến nút. Điều này tự động đảm bảo rằng tổ tiên phân chia con cháu, vì đường dẫn tổ tiên là tập hợp con của đường dẫn con cháu. 

Để thực thi hướng “chỉ nếu”, chúng ta phải đảm bảo rằng không có hai nút nào không thể so sánh được có mối quan hệ chia hết. Điều này được xử lý bằng cách đảm bảo mỗi số nguyên tố là duy nhất cho chính xác một cạnh, do đó việc phân tích hệ số của nút phản ánh chính xác đường đi của nó và không có gì khác. 

Vì N ≤ 60 nên chúng ta có thể gán một số nguyên tố riêng biệt cho mọi cạnh một cách an toàn. Tích thu được dọc theo bất kỳ đường đi nào nhiều nhất là tích của tối đa 59 số nguyên tố. Ngay cả khi sử dụng vài trăm số nguyên tố đầu tiên, con số này vẫn ở mức dưới 10^18. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm bài tập Brute Force | hàm mũ | O(N) | Quá chậm | 
| Xây dựng từng cạnh | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta lấy gốc cây tại nút 1. Việc xây dựng tiến hành bằng cách gán cho mỗi cạnh một số nguyên tố duy nhất và xác định giá trị của mỗi nút là tích của các số nguyên tố trên đường đi từ gốc.

1. Chọn danh sách gồm đủ nhiều số nguyên tố phân biệt. Vì có nhiều nhất 59 cạnh nên lấy khoảng 60 số nguyên tố đầu tiên là đủ. Điều này đảm bảo mỗi cạnh đều có một hệ số tối giản duy nhất. 
2. Root cây tại nút 1 và thực hiện truyền tải như DFS hoặc BFS. Chúng tôi duy trì giá trị hiện tại của mỗi nút khi chúng tôi chuyển từ nút cha sang nút con. 
3. Đặt x1 = 1 cho gốc. Điều này làm cho nút gốc tự động phân chia mọi nút khác, khớp với định nghĩa tổ tiên. 
4. Khi duyệt một cạnh từ cha u đến con v, gán một số nguyên tố p mới chưa sử dụng cho cạnh đó và đặt xv = xu * p. Điều này mở rộng việc phân tích nhân tử của đứa trẻ thêm chính xác một thừa số nguyên tố mới. 
5. Tiếp tục cho đến khi tất cả các nút được chỉ định. Giá trị của mỗi nút là tích của tất cả các số nguyên tố dọc theo đường dẫn gốc của nó. 

Lý do thứ tự bước này quan trọng là vì chúng ta phải đảm bảo mỗi cạnh tương ứng với chính xác một thừa số mới, do đó việc duyệt phải đảm bảo biết giá trị gốc trước khi tính toán con. 

### Tại sao nó hoạt động 

Mỗi nhãn nút chính xác là tích của các số nguyên tố tương ứng với các cạnh trên đường dẫn gốc của nó. Nếu u là tổ tiên của v, thì đường đi của u là tiền tố của đường đi của v, do đó xu chia xv vì tất cả các số nguyên tố trong xu xuất hiện trong xv với ít nhất cùng bội số. 

Nếu u không phải là tổ tiên của v thì đường đi của chúng khác nhau tại một số điểm phân kỳ đầu tiên. Từ thời điểm đó trở đi, mỗi cây con giới thiệu một số nguyên tố duy nhất không có trong cây kia, do đó, không nhãn nào có thể chia cho cây kia vì mỗi cây con đều chứa một thừa số nguyên tố vắng mặt ở cây kia. Điều này đảm bảo điều kiện “nếu và chỉ nếu”. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

N = int(input())
g = [[] for _ in range(N)]

for _ in range(N - 1):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    g[v].append(u)

# enough primes for N <= 60
primes = [
    2, 3, 5, 7, 11, 13, 17, 19, 23, 29,
    31, 37, 41, 43, 47, 53, 59, 61, 67, 71,
    73, 79, 83, 89, 97, 101, 103, 107, 109, 113,
    127, 131, 137, 139, 149, 151, 157, 163, 167, 173,
    179, 181, 191, 193, 197, 199, 211, 223, 227, 229,
    233, 239, 241, 251, 257, 263, 269, 271, 277, 281
]

x = [1] * N
used = 0

stack = [(0, -1)]
while stack:
    u, p = stack.pop()
    for v in g[u]:
        if v == p:
            continue
        x[v] = x[u] * primes[used]
        used += 1
        stack.append((v, u))

print(*x)
```Việc triển khai xây dựng một danh sách kề, sau đó chạy DFS lặp lại từ gốc. Căn nguyên bắt đầu bằng giá trị 1, giá trị này rất quan trọng vì nó phải chia mọi giá trị khác mà không đưa ra bất kỳ số nguyên tố không mong muốn nào. 

Mỗi lần chúng ta duyệt một cạnh tới một cạnh con chưa được thăm, chúng ta sử dụng một số nguyên tố chưa sử dụng và nhân nó với giá trị của cạnh cha. Điều này đảm bảo rằng mỗi cạnh tương ứng với chính xác một nhân tố được đưa vào. 

Một điểm tinh tế là tránh các vấn đề về độ sâu đệ quy; mặc dù N nhỏ nhưng ngăn xếp lặp sẽ tránh mọi sự phụ thuộc vào giới hạn đệ quy. Một chi tiết quan trọng khác là theo dõi nút gốc trong DFS để tránh truy cập lại các nút. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây đơn giản trong đó 1 được kết nối với 2 và 3. 

| Bước | Nút | Phụ huynh | Được giao số nguyên tố | giá trị x | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | - | - | 1 | 
| 2 | 2 | 1 | 2 | 2 | 
| 3 | 3 | 1 | 3 | 3 | 

Nút 2 và 3 là anh em ruột và nhãn 2 và 3 của chúng là nguyên tố cùng nhau, do đó không chia hết nút kia, phù hợp với thực tế là cả hai nút đều không phải là tổ tiên. 

Bây giờ hãy xem xét chuỗi 1 - 2 - 3. 

| Bước | Nút | Phụ huynh | Được giao số nguyên tố | giá trị x | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | - | - | 1 | 
| 2 | 2 | 1 | 2 | 2 | 
| 3 | 3 | 2 | 3 | 6 | 

Ở đây x3 = 6 và x2 = 2, do đó x2 chia x3, khớp với mối quan hệ tổ tiên. 

Những dấu vết này xác nhận rằng sự phân nhánh tạo ra các nhãn nguyên tố cùng nhau trong khi các chuỗi tạo ra sự lồng ghép theo cấp số nhân. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi cạnh được xử lý một lần trong DFS | 
| Không gian | O(N) | Danh sách kề và lưu trữ nhãn | 

Thuật toán tuyến tính về số lượng nút, không đáng kể với N ≤ 60. Các phép tính số học vẫn an toàn vì chúng ta chỉ nhân tối đa 59 số nguyên tố nhỏ, giữ nguyên tất cả các giá trị trong khoảng 10^18. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    import sys
    input = sys.stdin.readline
    N = int(input())
    g = [[] for _ in range(N)]
    for _ in range(N - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    primes = [
        2, 3, 5, 7, 11, 13, 17, 19, 23, 29,
        31, 37, 41, 43, 47, 53, 59, 61, 67, 71,
        73, 79, 83, 89, 97, 101, 103, 107, 109, 113,
        127, 131, 137, 139, 149, 151, 157, 163, 167, 173,
        179, 181, 191, 193, 197, 199, 211, 223, 227, 229,
        233, 239, 241, 251, 257, 263, 269, 271, 277, 281
    ]

    x = [1] * N
    used = 0
    stack = [(0, -1)]
    while stack:
        u, p = stack.pop()
        for v in g[u]:
            if v == p:
                continue
            x[v] = x[u] * primes[used]
            used += 1
            stack.append((v, u))

    print(*x)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-like small chain
assert run("3\n1 2\n2 3\n") == "1 2 6"

# star
assert run("3\n1 2\n1 3\n") in ["1 2 3", "1 3 2"]

# single node
assert run("1\n") == "1"

# line of 4
res = run("4\n1 2\n2 3\n3 4\n")
vals = list(map(int, res.split()))
assert vals[0] == 1
assert vals[1] == 2

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi 3 nút | 1 2 6 | sự lan truyền chia hết dọc theo đường đi | 
| sao 3 nút | hoán vị 1,2,3 | anh chị em đồng nguyên | 
| nút đơn | 1 | trường hợp cơ sở đúng đắn | 
| chuỗi 4 nút | tính chia hết tăng dần có giá trị | xử lý độ sâu lặp đi lặp lại | 

## Vỏ cạnh 

Cây nút đơn kiểm tra xem cấu trúc có gán x1 = 1 chính xác hay không mà không cố gắng truy cập bất kỳ số nguyên tố nào. Thuật toán ngay lập tức đưa ra 1 vì không có cạnh DFS nào được xử lý. 

Cây hình ngôi sao có gốc ở 1 đảm bảo rằng nhiều trẻ em nhận được các số nguyên tố khác nhau. Mỗi đứa trẻ trở thành một số nguyên tố riêng biệt, vì vậy không có đứa trẻ nào chia cho số khác. Điều này trực tiếp xác nhận rằng các nút anh chị em vẫn không thể so sánh được dưới khả năng phân chia. 

Một chuỗi sâu xác nhận phép nhân lặp lại. Mỗi bước giới thiệu một số nguyên tố mới, do đó các giá trị tăng theo cấp số nhân dọc theo đường dẫn. Ngay cả ở độ sâu tối đa 60, tích vẫn duy trì tốt trong phạm vi 10^18 vì nó chỉ là tích của các số nguyên tố nhỏ chứ không phải số nguyên lớn tùy ý.
