---
title: "CF 102409C - Xor trên cây"
description: "Chúng ta có một cây có trọng số với (N) đỉnh. Mỗi cạnh lưu trữ một số nguyên và đối với hai đỉnh bất kỳ, chúng tôi xác định giá trị đường dẫn của chúng là XOR của tất cả các trọng số cạnh trên đường dẫn duy nhất kết nối chúng. Nhiệm vụ là cộng các giá trị đường đi này trên mỗi cặp đỉnh riêng biệt không có thứ tự."
date: "2026-08-10T15:15:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102409
codeforces_index: "C"
codeforces_contest_name: "Semana i 2019"
rating: 0
weight: 102409
solve_time_s: 417
verified: true
draft: false
---

[CF 102409C - Xor trong cây](https://codeforces.com/problemset/problem/102409/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 57 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có trọng số với (N) đỉnh. Mỗi cạnh lưu trữ một số nguyên và đối với hai đỉnh bất kỳ, chúng tôi xác định giá trị đường dẫn của chúng là XOR của tất cả các trọng số cạnh trên đường dẫn duy nhất kết nối chúng. Nhiệm vụ là cộng các giá trị đường đi này trên mỗi cặp đỉnh riêng biệt không có thứ tự. 

Cấu trúc cây cho chúng ta một tính chất quan trọng: giữa hai đỉnh bất kỳ có đúng một đường đi. Thử thách không phải là tìm ra một đường đi riêng lẻ mà là tránh số lượng đường đi khổng lồ khi (N) đạt tới (10^5). 

Với (10^5) đỉnh thì hầu như có (5\times10^9) cặp không có thứ tự. Ngay cả thuật toán (O(N^2)) cũng đã quá chậm trước khi xem xét bất kỳ công việc nào cần thiết để xử lý từng đường dẫn. Cần có giải pháp gần với thời gian tuyến tính. Trọng số cạnh tối đa là (10^9), vì vậy mọi XOR có liên quan đều nằm trong phạm vi 30 bit, vì (10^9 < 2^{30}). Điều này làm cho chiến lược đếm mỗi bit trở nên thực tế. 

Có một số trường hợp nhỏ trong đó việc triển khai có thể âm thầm gặp trục trặc. Đầu tiên là cây chứa một đỉnh duy nhất:```
1
```Không có cặp đỉnh phân biệt nào nên đáp án là (0). Mã giả định ít nhất một cạnh có thể bị lỗi trong trường hợp này. 

Một trường hợp hữu ích khác là cây có hai đỉnh:```
2
1 2 1000000000
```Cặp duy nhất có đường dẫn XOR (1000000000), vì vậy câu trả lời là (1000000000). Điều này kiểm tra cả việc đếm cặp và ranh giới trên của trọng số cạnh. 

Trường hợp thứ ba là:```
3
1 2 1
2 3 2
```Ba giá trị cặp là (1), (2) và (1\oplus2=3), đưa ra câu trả lời là (6). Việc triển khai bất cẩn coi đường dẫn XOR giống như một tổng thông thường sẽ tạo ra (1+2+(1+2)=6) ở đây do trùng hợp ngẫu nhiên, do đó, một ví dụ ít đối xứng hơn sẽ hữu ích cho việc kiểm tra hoạt động XOR thực tế. 

Cuối cùng, hãy xem xét các trọng số cạnh bằng nhau:```
4
1 2 7
1 3 7
1 4 7
```Mỗi lá có giá trị root-XOR (7), trong khi gốc có giá trị (0). Ba cặp gốc tới lá đóng góp (7) mỗi cặp, trong khi mỗi đường đi từ lá tới lá đều có XOR (7\oplus7=0). Câu trả lời đúng là (21). Việc đếm các cạnh thay vì giá trị XOR sẽ gán không chính xác (14) cho đường dẫn từ lá này sang lá khác. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp có thể bắt đầu từ mỗi cặp đỉnh, tìm đường đi duy nhất của nó, XOR trọng số các cạnh dọc theo đường đi đó và thêm kết quả vào câu trả lời. Điều này đúng vì nó đánh giá chính xác số lượng yêu cầu cho mỗi cặp. 

Vấn đề là số lượng công việc lặp đi lặp lại. Trong cây có dạng đường đi, hai đỉnh gần hai đầu đối diện có thể có đường đi chứa gần như (N) cạnh. Có các cặp (N(N-1)/2), do đó việc xử lý từng đường dẫn một cách độc lập có thể yêu cầu theo thứ tự: 

[ 
\frac{N(N-1)}{2}(N-1)=O(N^3) 
] 

các chuyến thăm cạnh. Đối với (N=10^5), đây là khoảng (5\times10^{14}) hoạt động cạnh trong trường hợp xấu nhất, vượt xa giới hạn thời gian cho phép. Ngay cả một cách triển khai hiệu quả hơn để tìm thấy mọi cặp sử dụng thông tin cây được tính toán trước vẫn sẽ có các cặp (O(N^2)), tức là quá nhiều. 

Quan sát quan trọng là các đường dẫn XOR có thể được biểu diễn bằng cách sử dụng một giá trị được gắn vào mỗi đỉnh. Chọn đỉnh (1) làm gốc tùy ý và xác định 

[ 
A[v]=\text{XOR của tất cả các trọng số cạnh trên đường đi từ }1\text{ đến }v. 
] 

Giả sử các đường đi từ gốc đến (u) và (v) gặp nhau ở tổ tiên chung thấp nhất của chúng. Tiền tố chung xuất hiện trong cả hai đường dẫn gốc, do đó XOR của nó xuất hiện hai lần và bị hủy vì 

[ 
x\oplus x=0. 
] 

Những gì còn lại chính xác là XOR của đường đi từ (u) đến (v). Do đó, 

[ 
P(u,v)=A[u]\oplus A[v]. 
] 

Điều này loại bỏ hoàn toàn nhu cầu tìm đường dẫn giữa các cặp. 

Bây giờ bài toán trở thành: cho (N) số (A[1],A[2],\ldots,A[N]), tìm tổng của (A[u]\oplus A[v]) trên mọi cặp không có thứ tự. 

XOR có thể được xử lý độc lập từng bit một. Đối với một bit cụ thể, một cặp đóng góp (1) tại bit đó chính xác khi một điểm cuối có bit đó bằng (0) và điểm cuối kia có bit đó bằng (1). Chính xác thì nếu các giá trị (c_1) chứa bit và (c_0=N-c_1) thì không 

[ 
c_0c_1 
] 

cặp đóng góp chút đó. Do đó, sự đóng góp của nó cho câu trả lời cuối cùng là 

[ 
c_0c_1\cdot 2^b. 
] 

Chúng ta có thể tính toán tất cả các giá trị root-XOR bằng cách duyệt cây, đếm các bit được đặt trên các giá trị đó và kết hợp số lượng. Brute-force hoạt động vì mọi cặp đều có thể được đánh giá độc lập nhưng không thành công vì có quá nhiều cặp. Quan sát rằng mọi đường dẫn XOR chỉ đơn giản là XOR của hai XOR tiền tố gốc làm giảm vấn đề đồ thị thành một số tuyến tính các giá trị đỉnh theo sau là số lượng 30 bit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^3)) trường hợp xấu nhất | (O(N)) | Quá chậm | 
| Tối ưu | (O(30N)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề cho cây. Mỗi mục kề kề lưu trữ đỉnh lân cận và trọng số của cạnh kết nối. 
2. Gốc cây tại đỉnh (1) và duyệt nó lặp đi lặp lại bằng một ngăn xếp. Cửa hàng`xor_value[v]`, XOR của tất cả các trọng số cạnh từ đỉnh (1) đến (v). Ban đầu,`xor_value[1] = 0`. 
3. Khi duyệt một cạnh từ đỉnh đã thăm (u) tới đỉnh chưa thăm (v) với trọng số (w), gán 

[ 
xor_value[v]=xor_value[u]\oplus w. 
] 

Vì đồ thị là một cây nên có chính xác một đường đi từ gốc đến mọi đỉnh, nên lần đầu tiên chúng ta đến (v), giá trị này chính là XOR đường dẫn gốc chính xác của nó. 
4. Sau khi duyệt, hãy xem xét mọi bit từ (0) đến (29). Đếm xem có bao nhiêu giá trị root-XOR được đặt bit đó. Gọi số này là (c_1) và đặt (c_0=N-c_1). 
5. Đối với bit này, mỗi cặp có một giá trị chứa bit và một giá trị không chứa nó sẽ đóng góp (1) cho XOR. Có chính xác (c_1c_0) cặp không có thứ tự như vậy. Thêm 

[ 
c_1c_0(1\ll b) 
] 

để trả lời. 
6. Sau khi xử lý tất cả 30 bit, hãy in câu trả lời tích lũy. Mỗi bit được tính độc lập, do đó, đóng góp có trọng số của chúng tái tạo lại chính xác tổng của tất cả các XOR theo cặp. 

### Tại sao nó hoạt động 

Bất biến trung tâm là với mọi đỉnh (v),`xor_value[v]`bằng XOR của các cạnh trên đường đi duy nhất từ ​​gốc tới (v). Đối với hai đỉnh bất kỳ (u) và (v), phần chung của đường dẫn gốc của chúng được duyệt hai lần khi tính toán`xor_value[u] XOR xor_value[v]`, vì vậy nó hủy bỏ. Các cạnh còn lại chính xác là các cạnh trên đường đi duy nhất giữa (u) và (v), cho 

[ 
P(u,v)=xor_value[u]\oplus xor_value[v]. 
] 

Đối với mỗi bit, XOR có bit đó được đặt chính xác khi hai toán hạng của nó khác nhau ở vị trí đó. Nếu các đỉnh (c_1) có bit và (c_0) không có thì có chính xác các cặp (c_1c_0) có các giá trị khác nhau tại bit đó. Do đó, thuật toán đếm sự đóng góp của mỗi bit cho mỗi cặp không có thứ tự chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    graph = [[] for _ in range(n)]

    for _ in range(n - 1):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append((v, w))
        graph[v].append((u, w))

    xor_value = [0] * n
    parent = [-1] * n
    parent[0] = 0

    stack = [0]

    while stack:
        u = stack.pop()

        for v, w in graph[u]:
            if v == parent[u]:
                continue

            parent[v] = u
            xor_value[v] = xor_value[u] ^ w
            stack.append(v)

    answer = 0

    for bit in range(30):
        mask = 1 << bit
        ones = 0

        for value in xor_value:
            if value & mask:
                ones += 1

        zeros = n - ones
        answer += ones * zeros * mask

    print(answer)

if __name__ == "__main__":
    solve()
```Danh sách kề đại diện cho cây và lưu trữ mỗi cạnh theo cả hai hướng vì cây ban đầu không có hướng. Có (2(N-1)) mục kề nhau, do đó biểu diễn vẫn có kích thước tuyến tính. 

Truyền tải lặp lại thay thế DFS đệ quy. Một chuỗi có (10^5) đỉnh có thể có độ sâu đệ quy (10^5), không an toàn đối với đệ quy Python thông thường. Ngăn xếp rõ ràng tránh được vấn đề đó.`xor_value[0]`được khởi tạo bằng 0 vì đường dẫn từ gốc đến chính nó không chứa cạnh. Bất cứ khi nào đạt được một phần tử con chưa được thăm, giá trị của nó là giá trị của phần tử cha được XOR với trọng số của cạnh kết nối. 

các`parent`mảng là đủ để ngăn chặn việc quay lại ngay lập tức qua cạnh vừa được sử dụng. Vì đầu vào được đảm bảo là một cây nên không có đỉnh nào được truy cập trước đó có thể gây ra chu trình. Gốc gốc được đặt thành chính nó để quá trình truyền tải có giá trị gốc được xác định ngay từ đầu. 

Vòng lặp cuối cùng kiểm tra chính xác 30 bit. Mỗi trọng số cạnh tối đa là (10^9), vì vậy mọi XOR của trọng số cạnh đều ở dưới (2^{30}). Dù sao thì số nguyên Python cũng không bị tràn, nhưng việc giới hạn vòng lặp ở 30 bit liên quan này sẽ giúp thuật toán luôn chính xác và hiệu quả. 

phép nhân`ones * zeros * mask`có thể lớn hơn số nguyên 32 bit. Số nguyên Python tự động tăng lên khi cần thiết, do đó không cần xử lý tràn rõ ràng. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên,```
4
1 2 1
2 3 4
2 4 2
```Root ở đỉnh (1) cho các giá trị root-XOR sau. 

| Đỉnh | Phụ huynh | Trọng lượng cạnh | XOR gốc | 
| --- | --- | --- | --- | 
| 1 | không | 0 | 0 | 
| 2 | 1 | 1 | 1 | 
| 3 | 2 | 4 | 5 | 
| 4 | 2 | 2 | 3 | 

Bây giờ có thể thu được XOR theo cặp mà không cần đi qua bất kỳ đường dẫn nào. 

| Cặp | Giá trị XOR gốc | Cặp XOR | 
| --- | --- | --- | 
| 1, 2 | (0,1) | 1 | 
| 1, 3 | (0,5) | 5 | 
| 1, 4 | (0,3) | 3 | 
| 2, 3 | (1,5) | 4 | 
| 2, 4 | (1,3) | 2 | 
| 3, 4 | (5,3) | 6 | 

Tổng của họ là 

[ 
1+5+3+4+2+6=21. 
] 

Phương pháp đếm bit cho kết quả tương tự. Ví dụ: tại bit (0), các giá trị root-XOR là (0,1,5,3), do đó ba giá trị được đặt và một thì không. Bit đó đóng góp (3\cdot1\cdot1=3). Lặp lại điều này cho tất cả các bit có liên quan sẽ cho (21). 

Ví dụ thứ hai, hãy xem xét cây```
3
1 2 1
2 3 2
```Việc truyền tải tạo ra các giá trị này. 

| Đỉnh | Phụ huynh | Trọng lượng cạnh | XOR gốc | 
| --- | --- | --- | --- | 
| 1 | không | 0 | 0 | 
| 2 | 1 | 1 | 1 | 
| 3 | 2 | 2 | 3 | 

Với mỗi bit, chúng ta đếm xem có bao nhiêu giá trị chứa nó. 

| Chút | Các giá trị có bit được đặt | Những cái | Số không | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 1, 3 | 2 | 1 | (2\cdot1\cdot1=2) | 
| 1 | 3 | 1 | 2 | (1\cdot2\cdot2=4) | 

Tất cả các bit cao hơn đều không có đóng góp, vì vậy câu trả lời là (2+4=6). 

Ba đường dẫn thực tế có các giá trị XOR (1), (2) và (3), tổng của chúng cũng bằng (6). Ví dụ này chứng tỏ tại sao phương thức này đếm chính xác các cặp không có thứ tự: mỗi cặp được biểu thị một lần bằng một giá trị bit 0 và một giá trị một bit ở mỗi bit khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N+30N)=O(N)) | Cây được duyệt một lần, sau đó 30 bit được quét trên tất cả (N) đỉnh. | 
| Không gian | (O(N)) | Danh sách kề, mảng cha, mảng XOR gốc và ngăn xếp truyền tải đều tuyến tính trong (N). | 

Đối với (N=10^5), thuật toán thực hiện khoảng vài triệu thao tác đơn giản thay vì hàng tỷ hoặc hàng nghìn tỷ thao tác đường dẫn theo cặp. Việc sử dụng bộ nhớ cũng tuyến tính, thoải mái trong giới hạn 256 MB. Truyền tải lặp lại đặc biệt hữu ích trong Python vì nó tránh được các lỗi sâu đệ quy trên các cây có độ mất cân bằng cao. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm dưới đây cho thấy giải pháp thông qua một`solve_case`để mỗi xác nhận có thể thực thi độc lập. Trường hợp kích thước tối đa được tạo dưới dạng một chuỗi có (100000) đỉnh và trọng lượng cạnh (1). Các giá trị XOR gốc của nó xen kẽ giữa (0) và (1), đưa ra các giá trị (50000) của mỗi loại và câu trả lời là (2,5\times10^9).```python
import io
import sys

def solve_case(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        input = sys.stdin.readline

        n = int(input())
        graph = [[] for _ in range(n)]

        for _ in range(n - 1):
            u, v, w = map(int, input().split())
            u -= 1
            v -= 1
            graph[u].append((v, w))
            graph[v].append((u, w))

        xor_value = [0] * n
        parent = [-1] * n
        parent[0] = 0

        stack = [0]

        while stack:
            u = stack.pop()

            for v, w in graph[u]:
                if v == parent[u]:
                    continue

                parent[v] = u
                xor_value[v] = xor_value[u] ^ w
                stack.append(v)

        answer = 0

        for bit in range(30):
            mask = 1 << bit
            ones = 0

            for value in xor_value:
                if value & mask:
                    ones += 1

            zeros = n - ones
            answer += ones * zeros * mask

        print(answer)
        return sys.stdout.getvalue().strip()

    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert solve_case(
    """4
1 2 1
2 3 4
2 4 2
"""
) == "21", "sample 1"

# Minimum-size tree
assert solve_case(
    """1
"""
) == "0", "single vertex"

# Maximum edge weight
assert solve_case(
    """2
1 2 1000000000
"""
) == "1000000000", "maximum edge weight"

# Three vertices, different edge weights
assert solve_case(
    """3
1 2 1
2 3 2
"""
) == "6", "different XOR values"

# All equal edge weights
assert solve_case(
    """4
1 2 7
1 3 7
1 4 7
"""
) == "21", "equal edge weights"

# Maximum-size chain, all edge weights equal to 1.
# Root XOR values alternate 0, 1, 0, 1, ...
n = 100000
lines = [str(n)]
for i in range(1, n):
    lines.append(f"{i} {i + 1} 1")

large_case = "\n".join(lines) + "\n"

# There are 50000 zero-valued and 50000 one-valued root XORs.
# Every zero/one pair contributes 1.
assert solve_case(large_case) == "2500000000", "maximum-size chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`0`| Không có cặp nào tồn tại nên tổng trống phải bằng 0. | 
|`2 / 1 2 1000000000`|`1000000000`| Giá trị cạnh tối đa và chính xác một cặp không có thứ tự. | 
|`3 / 1 2 1 / 2 3 2`|`6`| Các giá trị cạnh khác nhau và đường dẫn nhiều cạnh XOR. | 
|`4 / 1 2 7 / 1 3 7 / 1 4 7`|`21`| Trọng số cạnh bằng nhau và hủy XOR từ lá này sang lá khác. | 
| Chuỗi (100000) đỉnh có trọng số (1) |`2500000000`| Tối đa (N), cây sâu, duyệt lặp và câu trả lời lớn. | 

## Vỏ cạnh 

Đối với một đỉnh duy nhất,```
1
```danh sách kề trống, quá trình truyền tải sẽ rời đi`xor_value[0]`bằng 0 và mỗi bit đều có số 0. Câu trả lời vẫn là con số không. Không có nỗ lực truy cập vào một cạnh hoặc cặp không tồn tại. 

Để có trọng lượng cạnh tối đa,```
2
1 2 1000000000
```các giá trị XOR gốc là (0) và (1000000000). Đối với mỗi bit được đặt của (1000000000), có chính xác một đỉnh có giá trị bằng 0 và một đỉnh có giá trị một, vì vậy mỗi bit như vậy đóng góp lũy thừa của nó bằng hai. Tổng của chúng được xây dựng lại chính xác (1000000000). Điều này xác nhận rằng thuật toán không phụ thuộc vào các giá trị cạnh nhỏ. 

Đối với trọng số cạnh bằng nhau,```
4
1 2 7
1 3 7
1 4 7
```các giá trị XOR gốc là (0,7,7,7). Tại mỗi bit chứa trong (7), có ba số một và một số 0, tạo ra (3) cặp đóng góp. Vì (7) chứa ba bit nên tổng số là (3+6+12=21). Các cặp giữa hai lá không đóng góp vì (7\oplus7=0). Đây chính xác là hành vi hủy bỏ được sử dụng trong bằng chứng. 

Đối với cây sâu, chuỗi có kích thước tối đa có (100000) đỉnh. Một DFS đệ quy sẽ cần phải lặp lại gần như tất cả (100000) đỉnh, điều này không an toàn trong Python. Ngăn xếp lặp truy cập vào cùng một đỉnh mà không cần đệ quy. Với mọi cạnh bằng (1), các giá trị gốc-XOR xen kẽ giữa (0) và (1). Có (50000) mỗi cái, vì vậy bit có liên quan duy nhất đóng góp 

[ 
50000\cdot50000=2500000000. 
] 

Việc triển khai thu được kết quả đó mà không cần liệt kê các cặp đỉnh khoảng (5\times10^9). 

Đối với con đường```
3
1 2 1
2 3 2
```các giá trị root-XOR là (0,1,3). Các giá trị theo cặp là (0\oplus1=1), (0\oplus3=3) và (1\oplus3=2), cho ra (6). Bất biến truyền tải được hiển thị trực tiếp tại đây: XOR các giá trị của hai điểm cuối sẽ loại bỏ tiền tố chung khỏi gốc và để lại chính xác XOR của đường dẫn kết nối của chúng.
