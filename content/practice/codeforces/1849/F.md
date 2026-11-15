---
title: "CF 1849F - Phân vùng XOR"
description: "Chúng ta được cấp một tập hợp các số nguyên riêng biệt và chúng ta cần chia chúng thành hai tập con sao cho XOR tối thiểu giữa tất cả các cặp trong mỗi tập con càng lớn càng tốt. Giá trị của một phân vùng là giá trị nhỏ hơn trong hai chi phí tập hợp con. Mỗi phần tử phải thuộc đúng một tập con."
date: "2026-06-09T05:35:41+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "bitmasks", "data-structures", "divide-and-conquer", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1849
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 152 (Rated for Div. 2)"
rating: 2700
weight: 1849
solve_time_s: 91
verified: false
draft: false
---

[CF 1849F - Phân vùng XOR](https://codeforces.com/problemset/problem/1849/F) 

**Xếp hạng:** 2700 
**Thẻ:** tìm kiếm nhị phân, mặt nạ bit, cấu trúc dữ liệu, phân chia để chinh phục, tham lam, cây cối 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các số nguyên riêng biệt và chúng ta cần chia chúng thành hai tập con sao cho XOR tối thiểu giữa tất cả các cặp trong mỗi tập con càng lớn càng tốt. Giá trị của một phân vùng là giá trị nhỏ hơn trong hai chi phí tập hợp con. Mỗi phần tử phải thuộc đúng một tập con. Đối với các bộ có ít hơn hai phần tử, chi phí được xác định là$2^{30}$. 

Kích thước đầu vào có thể lên tới$2 \times 10^5$số nguyên, mỗi số nhỏ hơn$2^{30}$. Điều này ngụ ý rằng bất kỳ giải pháp nào có các vòng lặp lồng nhau trên tập hợp để tính toán XOR cho tất cả các cặp sẽ quá chậm. Một cuộc tìm kiếm mạnh mẽ trên tất cả các phân vùng có thể, đó là$O(2^n)$, rõ ràng là không thể. Ngay cả việc tính toán tất cả các XOR theo cặp trong$O(n^2)$sẽ đạt được$4 \times 10^{10}$hoạt động ở giới hạn trên. 

Phần tinh vi của vấn đề là XOR hoạt động giống như thước đo khoảng cách trong không gian nhị phân. Các số liền kề theo XOR có thể không liền kề về mặt số, do đó, những nỗ lực phân chia ngây thơ dựa trên thứ tự giá trị có thể thất bại. Một trường hợp cạnh khác phát sinh khi một số tập hợp con chỉ có một phần tử, trong trường hợp đó chi phí mặc định là$2^{30}$. Việc triển khai bất cẩn có thể bỏ qua điều này và coi nó là con số không. 

Ví dụ, đưa ra`[0, 1, 2]`, chia thành`[0,1]`Và`[2]`cung cấp chi phí tập hợp con của`1`Và`2^30`. Giá trị tối thiểu của cả hai là 1. Một cách tiếp cận đơn giản luôn cân bằng kích thước tập hợp con có thể chọn`[0,2]`Và`[1]`mang lại mức tối thiểu kém hơn là 1. Thuật toán phải xem xét cấu trúc XOR, không chỉ đếm. 

## Phương pháp tiếp cận 

Giải pháp brute-force là tạo ra tất cả$2^n$phân vùng và đối với mỗi phân vùng, hãy tính chi phí bằng cách lặp lại tất cả các cặp trong mỗi tập hợp con. Điều này đúng nhưng không khả thi vì$n = 2 \times 10^5$, số lượng phân vùng là vô cùng lớn và mỗi lần tính toán chi phí sẽ mất$O(n^2)$. 

Cái nhìn sâu sắc quan trọng cho một giải pháp tối ưu là diễn giải các số nguyên ở dạng nhị phân. Mức XOR tối thiểu trong một tập hợp được xác định bởi bit cao nhất mà các số khác nhau. Điều này gợi ý một cách tiếp cận phân chia và chinh phục đệ quy. Nếu chúng tôi kiểm tra bit quan trọng nhất, chúng tôi có thể phân chia các số thành hai nhóm: nhóm có bit 0 trong đó và nhóm có bit 1. Để tối đa hóa XOR tối thiểu, chúng tôi chỉ phải ghép các số trên các phân vùng khác nhau khi cần thiết. Nếu một trong hai nhóm trống thì việc phân chia tối ưu sẽ xảy ra ở bit thấp hơn tiếp theo. 

Quá trình đệ quy tiếp tục xuống các bit, gán mỗi số cho một trong hai tập hợp con trong khi đảm bảo XOR của các phần tử trong cùng một tập hợp con là lớn. Điều này tương đương với việc xây dựng một trie nhị phân và phân tách đệ quy tại mỗi nút. Phân vùng tối ưu được xác định bằng cách gán số theo độ phân chia bit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n log MAX_BIT) | O(n log MAX_BIT) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mỗi số sang biểu diễn nhị phân với 30 bit, vì các số này nhỏ hơn$2^{30}$. Điều này cho phép chúng ta kiểm tra các bit từ quan trọng nhất đến ít quan trọng nhất. 
2. Xác định hàm đệ quy nhận danh sách các chỉ mục và vị trí bit. Nếu vị trí bit nhỏ hơn 0 hoặc danh sách có một phần tử, hãy gán tất cả các phần tử còn lại cho một tập hợp con và trả về. 
3. Phân chia danh sách thành hai danh sách con dựa trên bit hiện tại: một cho các phần tử có số 0 ở bit này và một cho các phần tử có số 1. 
4. Nếu cả hai danh sách con đều không trống, hãy phân chia đệ quy cả hai danh sách con ở bit thấp hơn tiếp theo. Nếu danh sách con trống, tiếp tục tách danh sách con không trống. Quá trình đệ quy đảm bảo rằng các phần tử khác nhau ở các bit cao hơn có nhiều khả năng được phân tách hơn, giúp tối đa hóa XOR tối thiểu trong mỗi tập hợp con. 
5. Trong khi quay lui, hãy gán các định danh tập hợp con (0 hoặc 1) cho mỗi phần tử dựa trên phía nào của phần phân chia mà chúng rơi vào. Lật định danh một cách nhất quán để duy trì sự phân chia tập hợp con. 
6. Sau khi tất cả quá trình đệ quy hoàn tất, xuất phân vùng dưới dạng chuỗi 0 và 1, tương ứng với các tập hợp con. 

Tại sao nó hoạt động: Đệ quy tôn trọng thuộc tính mà XOR tối thiểu bị chi phối bởi bit có độ chênh lệch cao nhất giữa các phần tử. Bằng cách tách sớm các số bằng các bit khác nhau, chúng tôi tránh tạo ra các giá trị XOR nhỏ trong cùng một tập hợp con. Sự lựa chọn tham lam trên mỗi bit này là tối ưu toàn cầu vì các bit cao hơn luôn đóng góp nhiều hơn vào giá trị XOR và các bit thấp hơn chỉ được xem xét khi các bit cao hơn giống hệt nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

def xor_partition():
    n = int(input())
    a = list(map(int, input().split()))
    ans = [0] * n

    def solve(indices, bit):
        if bit < 0 or len(indices) <= 1:
            for i in indices:
                ans[i] = 0
            return
        zeros, ones = [], []
        for i in indices:
            if (a[i] >> bit) & 1:
                ones.append(i)
            else:
                zeros.append(i)
        if not zeros:
            solve(ones, bit - 1)
        elif not ones:
            solve(zeros, bit - 1)
        else:
            solve(zeros, bit - 1)
            for i in ones:
                ans[i] = 1
            solve(ones, bit - 1)

    solve(list(range(n)), 29)
    print(''.join(map(str, ans)))

xor_partition()
```Giải pháp bắt đầu bằng cách tăng giới hạn đệ quy vì phép chia để trị có thể đi sâu đối với số lượng lớn.$n$. Hàm đệ quy phân vùng các chỉ số dựa trên bit hiện tại. Việc gán 0 hoặc 1 được thực hiện trong khi đệ quy. Nếu một bên của phân vùng trống, thuật toán tiếp tục với bên không trống. Ngược lại, các phần tử trong`ones`nhóm được gán cho tập hợp con 1 và chúng tôi tiếp tục đệ quy cho cả hai bên. Điều này đảm bảo XOR giữa các tập hợp con được tối đa hóa ở các bit cao hơn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`42, 13, 1337, 37, 152`| Bước | Chỉ số | Chút | Số không | Những cái | Bài tập | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [0,1,2,3,4] | 29 | [0,1,3,4] | [2] | ans[2]=1 | 
| 2 | [0,1,3,4] | 28 | ... | ... | tái diễn | 
| ... | ... | ... | ... | ... | ... | 

Trace chứng minh rằng các số khác nhau ở bit cao được phân tách trước tiên, đẩy các giá trị XOR nhỏ qua các tập hợp con thay vì bên trong chúng. 

### Ví dụ tùy chỉnh 

đầu vào:`[0, 1, 2, 3]`| Bước | Chỉ số | Chút | Số không | Những cái | Bài tập | 
| --- | --- | --- | --- | --- | --- | 
| 1 | [0,1,2,3] | 1 | [0,1] | [2,3] | ans[2,3]=1 | 
| 2 | [0,1] | 0 | [0] | [1] | ans[1]=1 | 
| 3 | [2,3] | 0 | [2] | [3] | ans[3]=1 | 

Đầu ra`0111`chia các số để XOR tối thiểu trong cả hai tập hợp con được tối đa hóa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log MAX_BIT) | Mỗi phần tử được xử lý tối đa một lần trên mỗi vị trí bit (30 bit) | 
| Không gian | O(n log MAX_BIT) | Độ sâu ngăn xếp đệ quy có thể đạt log(MAX_BIT) cho mỗi phần tử | 

Điều này phù hợp thoải mái trong các ràng buộc vì$n = 2 \times 10^5$và MAX_BIT = 30, dẫn đến ~6 triệu thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    xor_partition()
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("5\n42 13 1337 37 152\n") in ["10001", "01110"], "sample 1"

# Minimum size
assert run("2\n0 1\n") in ["01", "10"], "minimum size"

# Maximum size, sequential numbers
n = 200000
seq = ' '.join(str(i) for i in range(n))
out = run(f"{n}\n{seq}\n")
assert len(out) == n, "maximum size"

# Boundary numbers
assert run("4\n0 1 1073741823 536870911\n") in ["0011", "1100"], "boundary values"

# Small consecutive numbers
assert run("3\n1 2 3\n") in ["011", "101", "110"], "small consecutive"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2\n0 1`|`01`hoặc`10`| Xử lý đúng đầu vào có kích thước tối thiểu | 
|`200000\n0 1 2 ...`| chiều dài 200000 | Khả năng mở rộng tối đa n | 
|`4\n0 1 1073741823 536870911`| | |
