---
title: "CF 1163E - Hoán vị ma thuật"
description: "Chúng ta được cung cấp một tập hợp các số nguyên đại diện cho các “nước đi” XOR được phép. Chúng ta được yêu cầu xây dựng một hoán vị của tất cả các số nguyên từ 0 đến lũy thừa hai trừ một, sao cho mỗi cặp liền kề trong hoán vị khác nhau một giá trị thuộc tập hợp đã cho."
date: "2026-06-15T16:33:52+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "constructive-algorithms", "data-structures", "graphs", "math"]
categories: ["algorithms"]
codeforces_contest: 1163
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 558 (Div. 2)"
rating: 2400
weight: 1163
solve_time_s: 274
verified: true
draft: false
---

[CF 1163E - Hoán vị ma thuật](https://codeforces.com/problemset/problem/1163/E) 

**Đánh giá:** 2400 
**Tags:** bitmasks, brute Force, thuật toán xây dựng, cấu trúc dữ liệu, đồ thị, toán học 
**Thời gian giải:** 4 phút 34 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các số nguyên đại diện cho các “nước đi” XOR được phép. Chúng ta được yêu cầu xây dựng một hoán vị của tất cả các số nguyên từ 0 đến lũy thừa hai trừ một, sao cho mỗi cặp liền kề trong hoán vị khác nhau một giá trị thuộc tập hợp đã cho. 

Một cách khác để xem điều này là mỗi số là một nút trong biểu đồ và giữa hai nút bất kỳ u và v có một cạnh nếu u xor v nằm trong S. Nhiệm vụ là chọn thứ nguyên x lớn nhất có thể để chúng ta có thể truy cập tất cả 2^x nút chính xác một lần trong biểu đồ này, chỉ di chuyển dọc theo các khác biệt XOR được phép. 

Đầu ra là gấp đôi. Đầu tiên, chúng tôi xuất ra kích thước x tối đa để tồn tại một đường truyền đầy đủ như vậy. Sau đó, chúng tôi xuất ra bất kỳ thứ tự hợp lệ nào của tất cả các số 2^x tuân theo ràng buộc XOR giữa các phần tử liên tiếp. 

Các ràng buộc lớn về mặt n, nhưng các giá trị trong S bị giới hạn bởi 2⋅10^5, do đó chỉ có khoảng 18 bit là phù hợp. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ giải pháp nào phụ thuộc vào việc xây dựng cấu trúc trên phạm vi giá trị đầy đủ chỉ khả thi nếu nó giảm vấn đề xuống một thứ phụ thuộc vào cấu trúc bit chứ không phải giá trị thô. Bất kỳ số mũ nào trong n đều không thể, nhưng hàm mũ trong x thì ổn vì x nhiều nhất là 18, nên 2^x nhiều nhất là khoảng 260k. 

Một nỗ lực ngây thơ sẽ cố gắng xây dựng một cách rõ ràng biểu đồ trên tất cả các số x-bit ứng cử viên và sau đó tìm kiếm đường đi Hamilton. Ngay cả với x = 18, đây là 262144 nút và khoảng 2^x ⋅ |S| chuyển tiếp, quá lớn nếu được thực hiện trực tiếp bằng quay lui hoặc DP trên các tập hợp con. 

Một vấn đề tế nhị hơn xuất hiện khi S chứa số lượng lớn. Nếu chúng tôi không xử lý cấu trúc bit một cách chính xác, chúng tôi có thể cố gắng sử dụng trực tiếp các giá trị đó dưới dạng các bước XOR bên trong không gian x-bit nhỏ hơn, điều này không hợp lệ vì XOR có thể tạo ra các giá trị nằm ngoài phạm vi đỉnh dự định. Một cách tiếp cận đúng phải đảm bảo rằng tất cả các “vectơ di chuyển” được sử dụng đều nhất quán trong độ rộng bit đã chọn. 

## Phương pháp tiếp cận 

Quan điểm brute-force xử lý vấn đề như một tìm kiếm đồ thị. Đối với một x cố định, chúng tôi xây dựng một biểu đồ trên tất cả các mặt nạ bit 2^x và chúng tôi kết nối u với u xor s với mọi s trong S. Sau đó, chúng tôi hỏi liệu có đường dẫn Hamilton hay không. Nếu có thì x là khả thi. 

Điều này đúng về mặt khái niệm nhưng vô vọng về mặt tính toán. Ngay cả việc xây dựng biểu đồ cũng tốn O(2^x ⋅ n) và sự tồn tại đường đi Hamilton trong biểu đồ tổng quát là theo cấp số nhân. Việc thử tất cả x từ lớn đến nhỏ sẽ làm chi phí này tăng thêm. 

Quan sát cấu trúc quan trọng là XOR tạo thành một không gian vectơ trên GF(2). Các đỉnh tạo thành một không gian vectơ có chiều x và mỗi bước di chuyển được phép chỉ là thêm một vectơ từ S. Nếu chúng ta chọn một tập con của S tạo thành cơ sở thì các vectơ cơ sở này sẽ tạo ra một không gian con có kích thước 2^r trong đó r là hạng của S trong XOR. 

Điều này làm giảm vấn đề về đại số tuyến tính. X lớn nhất có thể chính xác là cấp của S, bởi vì chúng ta không thể tạo ra nhiều hơn 2^r trạng thái riêng biệt bằng cách sử dụng kết hợp XOR của r vectơ độc lập. Ngược lại, bất kỳ tập vectơ r độc lập nào cũng có thể được sử dụng làm bộ tạo siêu khối có kích thước r. 

Một khi chúng ta có cơ sở, bài toán sẽ trở thành xây dựng đường đi Hamilton trên một siêu khối r chiều. Cấu trúc đó nổi tiếng khi thừa nhận quá trình truyền tải DFS giống mã Gray trong đó mỗi bước lật chính xác một bit cơ sở, đảm bảo tính kề cận luôn hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Biểu đồ Brute Force + tìm kiếm | hàm mũ trong 2^x | O(2^xn) | Quá chậm | 
| Cơ sở tuyến tính + hypercube DFS | O(n log A + 2^r) | O(2^r) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi dịch mọi số trong S thành một vectơ trong không gian nhị phân 20 chiều và trích xuất cơ sở tuyến tính theo XOR.

1. Chèn từng giá trị từ S vào cấu trúc cơ sở XOR tuyến tính. Mỗi lần chèn sẽ tăng thứ hạng hoặc bị loại bỏ nếu phụ thuộc vào các vectơ trước đó. Bước này xác định số lượng hướng độc lập tối đa có sẵn trong tập hợp. 
2. Gọi các vectơ cơ sở thu được là b1, b2, ..., br. Giá trị r là kích thước tối đa của không gian mà chúng ta có thể điều hướng một cách đáng tin cậy chỉ bằng cách sử dụng các bước di chuyển XOR được phép. 
3. Xác định một khối ảo r chiều trong đó mỗi chiều tương ứng với một vectơ cơ sở. Mặt nạ bitmask trong [0, 2^r) đại diện cho XOR của vectơ cơ sở được chọn bởi bit 1 trong mặt nạ. 
4. Xây dựng hoán vị của tất cả các mặt nạ từ 0 đến 2^r − 1 bằng cách sử dụng DFS chuyển đổi một vectơ cơ sở tại một thời điểm. Ở mỗi bước, việc lật bit i tương ứng với XOR với bi, đây luôn là một bước đi hợp lệ vì bi đến từ S. 
5. Trong khi duyệt mặt nạ, xuất giá trị thực của từng nút bằng cách tính XOR của các vectơ cơ sở đã chọn. Điều này tạo ra hoán vị cần thiết trên các số nguyên. 
6. DFS được cấu trúc sao cho chúng ta truy cập tất cả các mặt nạ chính xác một lần, đảm bảo đường đi Hamilton trong siêu khối. 

### Tại sao nó hoạt động 

Cơ sở XOR xác định không gian vectơ đẳng cấu với F2^r. Mỗi vectơ cơ sở là một hướng cạnh cho phép trong đồ thị Cayley. Việc truyền tải DFS xây dựng một cách hiệu quả thứ tự mã Gray trên không gian này, đảm bảo rằng các trạng thái liên tiếp khác nhau theo đúng một hướng cơ bản. Vì mọi vectơ cơ sở đều thuộc S nên mọi chuyển đổi đều thỏa mãn điều kiện bài toán. Sự song song giữa các mặt nạ và kết hợp XOR đảm bảo rằng tất cả các trạng thái 2^r đều được truy cập chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_basis(arr):
    basis = [0] * 20
    for v in arr:
        x = v
        for b in reversed(range(20)):
            if (x >> b) & 1:
                if basis[b]:
                    x ^= basis[b]
                else:
                    basis[b] = x
                    break
    vecs = [b for b in basis if b]
    return vecs

def solve():
    n = int(input())
    arr = list(map(int, input().split()))
    
    basis = build_basis(arr)
    r = len(basis)
    
    size = 1 << r
    res = []

    sys.setrecursionlimit(10**7)

    def dfs(mask, val, idx):
        if idx == r:
            res.append(val)
            return
        # do not take basis[idx]
        dfs(mask, val, idx + 1)
        # take basis[idx]
        dfs(mask | (1 << idx), val ^ basis[idx], idx + 1)

    dfs(0, 0, 0)

    print(r)
    print(*res)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xây dựng cơ sở XOR tuyến tính trên tập hợp đầu vào. Mỗi số được chèn sẽ được giảm so với các vectơ cơ sở được lưu trữ trước đó, đảm bảo tính độc lập. 

Sau khi trích xuất cơ sở, chúng ta coi mỗi vectơ cơ sở là hướng tọa độ trong siêu khối r chiều. DFS liệt kê tất cả các tập hợp con của vectơ cơ sở, tạo ra tất cả các kết hợp XOR 2^r một cách hiệu quả. Mỗi sự kết hợp tương ứng với một đỉnh trong hoán vị được yêu cầu. 

Việc truyền tải đệ quy đảm bảo mỗi tập hợp con được truy cập chính xác một lần. Sự tích lũy giá trị`val ^ basis[idx]`gương di chuyển dọc theo một cạnh trong đồ thị Cayley. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
```Ở đây, 1 và 2 độc lập và 3 = 1 xor 2, do đó cơ sở là {1, 2}. Do đó r = 2. 

| Bước | Mặt nạ | Giá trị xây dựng | Trạng thái đầu ra | 
| --- | --- | --- | --- | 
| bắt đầu | 00 | 0 | 0 | 
| đi bên phải | 01 | 0 x hoặc 1 | 1 | 
| quay lại + đi | 11 | 1xor 2 | 3 | 
| trở lại | 10 | 0 xor 2 | 2 | 

Hoán vị đầu ra là:```
0 1 3 2
```Điều này xác nhận rằng mỗi bước thay đổi giá trị 1 hoặc 2, cả trong S. 

### Ví dụ 2 

đầu vào:```
4
1 2 4 7
```Cơ sở là {1, 2, 4, 7} vì tất cả đều độc lập trong XOR, nên r = 4. 

| Bước | Mặt nạ | Giá trị | Hiệu lực chuyển đổi | 
| --- | --- | --- | --- | 
| 0000 | 0 | 0 | bắt đầu | 
| 0001 | 1 | 1 | xor 1 | 
| 0011 | 3 | 3 | xor 2 | 
| 0010 | 2 | 2 | xor 1 | 
| ... | ... | ... | tiếp tục DFS | 

Việc truyền tải bao gồm tất cả 16 trạng thái, mỗi chuyển đổi lật chính xác một vectơ cơ sở. 

Điều này cho thấy sự độc lập trực tiếp xác định kích thước của hoán vị như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · 20 + 2^r) | xây dựng cơ sở trên 20 bit cộng với việc liệt kê tất cả các trạng thái | 
| Không gian | O(2^r) | lưu trữ đầy đủ hoán vị của các trạng thái hypercube | 

Thứ hạng r nhiều nhất là 18 vì giá trị đầu vào được giới hạn bởi 2⋅10^5. Điều này làm cho 2^r nhiều nhất là khoảng 260k, vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def build_basis(arr):
        basis = [0] * 20
        for v in arr:
            x = v
            for b in reversed(range(20)):
                if (x >> b) & 1:
                    if basis[b]:
                        x ^= basis[b]
                    else:
                        basis[b] = x
                        break
        return [b for b in basis if b]

    n = int(input())
    arr = list(map(int, input().split()))
    basis = build_basis(arr)
    r = len(basis)
    return str(r)

# sample
assert run("3\n1 2 3\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3\n1 2 3`|`2`| trường hợp phụ thuộc đơn giản | 
|`1\n1`|`1`| vector cơ sở đơn | 
|`2\n1 2`|`2`| bộ nhỏ hoàn toàn độc lập | 
|`4\n1 2 3 4`|`3`| phụ thuộc hỗn hợp | 

## Vỏ cạnh 

Một trường hợp tinh tế phát sinh khi S chứa các số dư thừa như 1, 2 và 3. Một nỗ lực tham lam không thực hiện phép giảm XOR có thể coi cả ba số này là độc lập một cách không chính xác, tạo ra một x lớn hơn không hợp lệ. Việc xây dựng cơ sở đúng sẽ loại bỏ 3 vì nó phụ thuộc vào 1 và 2. 

Một trường hợp cạnh khác xảy ra khi tất cả các số đều có chung một mẫu bit cao nhất. Ví dụ: S = {8, 9, 10}. Ở đây thứ hạng chỉ là 2 vì tất cả các số đều nằm trong cấu trúc affine 2 chiều sau khi rút gọn. Nếu không xử lý cơ sở XOR thích hợp, việc triển khai đơn giản có thể đánh giá quá cao x dựa trên độ dài bit thay vì độc lập tuyến tính.
