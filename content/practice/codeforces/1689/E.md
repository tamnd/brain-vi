---
title: "CF 1689E - ANDfinity"
description: "Chúng ta được cho một mảng các số nguyên không âm. Mỗi phần tử mảng tương ứng với một đỉnh trong đồ thị. Hai đỉnh được kết nối bằng một cạnh bất cứ khi nào giá trị AND theo bit của chúng là dương. Biểu đồ không được đảm bảo để được kết nối."
date: "2026-06-09T23:28:54+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "constructive-algorithms", "dfs-and-similar", "dsu", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1689
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 798 (Div. 2)"
rating: 2500
weight: 1689
solve_time_s: 150
verified: false
draft: false
---

[CF 1689E - ANDfinity](https://codeforces.com/problemset/problem/1689/E) 

**Đánh giá:** 2500 
**Tags:** bitmasks, brute Force, thuật toán xây dựng, dfs và tương tự, dsu, đồ thị 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên không âm. Mỗi phần tử mảng tương ứng với một đỉnh trong đồ thị. Hai đỉnh được kết nối bằng một cạnh bất cứ khi nào giá trị AND theo bit của chúng là dương. 

Biểu đồ không được đảm bảo để được kết nối. Chúng tôi có thể liên tục tăng hoặc giảm các phần tử mảng riêng lẻ đi một đơn vị, trong đó việc giảm chỉ được phép đối với các giá trị dương. Mỗi lần tăng hoặc giảm đều tốn một thao tác. 

Nhiệm vụ của chúng ta là tìm số phép toán tối thiểu cần thiết để làm cho biểu đồ được kết nối và xuất ra một mảng kết quả đạt được mức tối thiểu đó. 

Quan sát quan trọng nhất là cấu trúc đồ thị được xác định hoàn toàn bằng các bit tập hợp chung. Hai số liền kề nhau nếu chúng có ít nhất một bit chung bằng 1. Do đó, khả năng kết nối là đặc tính thể hiện cách các bit được phân bổ trên mảng chứ không phải của bản thân các giá trị số. 

Những hạn chế nhỏ đến mức đáng ngạc nhiên. Tổng của tất cả$n$trên toàn bộ đầu vào tối đa là 2000. Điều này có nghĩa là$O(n^2)$hoặc thậm chí$O(n^2 \log A)$giải pháp hoàn toàn có thể chấp nhận được. Vì các giá trị ở dưới$2^{30}$, chỉ có 30 vị trí bit liên quan. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
2
0 0
```Một đỉnh tương ứng với giá trị 0 không có bit nào được đặt, vì vậy nó bị cô lập với mọi thứ. Bất kỳ biểu đồ kết nối nào cũng không được chứa số 0. Một giải pháp chỉ kiểm tra khả năng kết nối giữa các đỉnh khác 0 sẽ cho kết quả thành công một cách không chính xác. 

Một trường hợp phức tạp khác là:```
2
3 12
```Biểu đồ bị ngắt kết nối vì:```
3  = 0011
12 = 1100
```AND của họ bằng 0. Một lần giảm duy nhất mang lại:```
3  = 0011
11 = 1011
```và bây giờ AND là dương. Câu trả lời tối thiểu là 1. 

Một trường hợp tinh tế cuối cùng là:```
4
8 8 1 1
```Sau khi loại bỏ số 0, biểu đồ vẫn bị ngắt kết nối. Một số ý tưởng mang tính xây dựng kết nối nó thành hai hoạt động, nhưng không nhất thiết phải là một. Lời giải phải phân biệt chính xác giữa đáp án 1 và đáp án 2. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ là tìm kiếm trên tất cả các chuỗi sửa đổi có thể có và kiểm tra kết nối sau mỗi thay đổi. 

Điều này rõ ràng là đúng về nguyên tắc, nhưng hoàn toàn không khả thi. Ngay cả việc giới hạn bản thân ở một vài mức tăng và giảm cho mỗi phần tử cũng đã tạo ra một không gian tìm kiếm theo cấp số nhân. 

Bước đột phá quan trọng là câu trả lời luôn cực kỳ nhỏ. 

Đầu tiên, mọi số 0 đều phải được thay đổi. Số 0 không đóng góp bit nào và không bao giờ có thể chia sẻ cạnh với bất kỳ thứ gì. Biến mọi số 0 thành một luôn là cách tối ưu vì nó tốn chính xác một thao tác trên mỗi số 0 và tạo ra một giá trị hữu ích. 

Sau khi tất cả các số 0 trở thành số 1, chúng ta kiểm tra biểu đồ kết quả. 

Nếu nó đã được kết nối, chúng ta đã hoàn thành. 

Mặt khác, nhận xét của người biên tập rất đáng chú ý: sau khi loại bỏ các số 0, chi phí bổ sung tối thiểu còn lại không bao giờ nhiều hơn hai. Trước tiên, chúng ta có thể kiểm tra xem một thao tác bổ sung có đủ hay không bằng cách thử mọi mức tăng đơn lẻ có thể và mọi mức giảm đơn lẻ có thể có. Từ$n \le 2000$, việc kiểm tra vũ phu này rẻ tiền. 

Nếu không có nỗ lực nào thành công thì câu trả lời phải có chính xác là hai. 

Bước cuối cùng là một cách mang tính xây dựng để đạt được hai hoạt động đó. Cho phép```
lowbit(x) = x & (-x)
```và xem xét tất cả các phần tử có lowbit là tối đa. 

Nếu chỉ có một phần tử như vậy, hãy giảm nó đi một phần tử. 

Nếu có nhiều phần tử như vậy, hãy giảm một trong số chúng và tăng phần tử khác. 

Sự sửa đổi đặc biệt này luôn kết nối biểu đồ. Bằng chứng xuất phát từ cấu trúc thành phần bit được sử dụng trong bài kiểm tra khả năng kết nối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm Brute Force qua hoạt động | Hàm mũ | Hàm mũ | Quá chậm | 
| Kiểm tra kết nối + Thử tất cả các thay đổi một bước + Sửa lỗi hai bước mang tính xây dựng |$O(n^2 \cdot 30)$|$O(30)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Kiểm tra kết nối 

Đồ thị trên các đỉnh có thể lớn nhưng chỉ có 30 bit vị trí. 

Thay vì xây dựng biểu đồ đỉnh một cách trực tiếp, hãy xây dựng biểu đồ trên các bit. 

Đối với mỗi số, hãy kết nối tất cả các bit đã đặt xuất hiện bên trong số đó. Nếu một số chứa các bit 1, 4 và 7 thì các bit đó thuộc cùng một thành phần vì số đó liên kết chúng với nhau. 

Sau khi xử lý tất cả các số, tất cả các vị trí bit xuất hiện ở bất kỳ đâu sẽ thuộc về một thành phần được kết nối duy nhất. Nếu không, đồ thị đỉnh ban đầu không thể kết nối được. 

### bước 

1. Đếm tất cả các số 0 trong mảng. 
2. Thay mỗi số 0 bằng một và cộng số thay thế vào đáp án. 
3. Kiểm tra xem đồ thị có được kết nối hay không. 
4. Nếu nó được kết nối, hãy xuất mảng hiện tại. 
5. Nếu không, hãy thử mọi chỉ mục. 

Đối với mỗi chỉ mục, nếu giá trị dương, hãy tạm thời giảm giá trị đó xuống một và kiểm tra khả năng kết nối. 

Nếu đạt được kết nối, hãy xuất mảng này. 
6. Khôi phục giá trị và tạm thời tăng giá trị lên một. 
7. Kiểm tra lại kết nối. 
8. Nếu đạt được kết nối, hãy xuất mảng này. 
9. Nếu không có một sửa đổi nào có tác dụng, hãy tìm giá trị lớn nhất của`lowbit(a[i])`. 
10. Thu thập tất cả các chỉ số có mức thấp tối đa đó. 
11. Nếu tồn tại chính xác một chỉ mục như vậy, hãy giảm phần tử đó đi một. 
12. Ngược lại, hãy giảm một trong các phần tử đó và tăng phần tử khác lên một phần tử. 
13. Thêm hai vào câu trả lời và xuất ra mảng kết quả. 

### Tại sao nó hoạt động 

Việc thay thế mọi số 0 bằng một là bắt buộc. Số 0 không chứa bit cố định nên nó luôn bị cô lập. Bất kỳ biểu đồ cuối cùng được kết nối nào cũng phải loại bỏ mọi số 0. 

Sau khi loại bỏ các số 0, trước tiên chúng tôi kiểm tra tất cả các khả năng yêu cầu chính xác một thao tác bổ sung. Vì mọi giải pháp một thao tác hợp pháp phải là một mức tăng hoặc một mức giảm trên một phần tử nên việc kiểm tra toàn diện là đủ. 

Khẳng định còn lại là nếu không tồn tại lời giải một thao tác thì luôn tồn tại lời giải hai thao tác. Cấu trúc lowbit từ bài xã luận chính thức đảm bảo điều này. Các phần tử có lowbit tối đa nằm ở "mức bit" cao nhất trong số tất cả các số. Việc điều chỉnh một hoặc hai trong số chúng bằng một sẽ thay đổi bit được đặt thấp nhất của chúng và hợp nhất các thành phần bit bị ngắt kết nối. Như vậy mọi trường hợp còn lại đều có thể giải được bằng đúng hai phép tính bổ sung. 

Vì chúng ta đã loại trừ các câu trả lời 0 và 1 nên câu trả lời tối thiểu có thể có phải là 2. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def connected(a):
    parent = list(range(30))

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def union(a, b):
        ra = find(a)
        rb = find(b)
        if ra != rb:
            parent[ra] = rb

    used = []

    for x in a:
        bits = []
        for b in range(30):
            if (x >> b) & 1:
                bits.append(b)

        if bits:
            used.extend(bits)
            root = bits[0]
            for b in bits[1:]:
                union(root, b)

    used = list(set(used))

    if not used:
        return False

    r = find(used[0])
    for b in used:
        if find(b) != r:
            return False

    return True

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        ops = 0

        for i in range(n):
            if a[i] == 0:
                a[i] = 1
                ops += 1

        if connected(a):
            print(ops)
            print(*a)
            continue

        found = False

        for i in range(n):
            if a[i] > 0:
                a[i] -= 1
                if connected(a):
                    print(ops + 1)
                    print(*a)
                    found = True
                    break
                a[i] += 1

            a[i] += 1
            if connected(a):
                print(ops + 1)
                print(*a)
                found = True
                break
            a[i] -= 1

        if found:
            continue

        best = max(x & -x for x in a)

        idx = [i for i in range(n) if (a[i] & -a[i]) == best]

        if len(idx) == 1:
            a[idx[0]] -= 1
        else:
            a[idx[0]] -= 1
            a[idx[1]] += 1

        print(ops + 2)
        print(*a)

solve()
```Việc thực hiện xoay quanh`connected()`chức năng. 

Thay vì xây dựng biểu đồ trên các vị trí mảng, nó xây dựng DSU trên các vị trí 30 bit. Mỗi số hợp nhất tất cả các bit đã đặt của nó thành một thành phần DSU duy nhất. Cuối cùng, tất cả các bit được sử dụng phải thuộc về một thành phần DSU để đồ thị được kết nối. Đây chính xác là tiêu chí được sử dụng trong giải pháp chính thức. 

Tìm kiếm một thao tác phải khôi phục cẩn thận các giá trị sau mỗi lần sửa đổi tạm thời. Quên khôi phục một phần tử là nguyên nhân phổ biến dẫn đến các câu trả lời sai. 

Tính toán lowbit sử dụng:```
x & -x
```để cô lập bit được đặt thấp nhất. Trong số tất cả các phần tử, chúng tôi tìm những phần tử có bit thấp tối đa và áp dụng bản sửa lỗi hai thao tác mang tính xây dựng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 3 4 5
```| Bước | Mảng | Đã kết nối | 
| --- | --- | --- | 
| Ban đầu | [1,2,3,4,5] | Có | 
| Không xử lý | [1,2,3,4,5] | Có | 
| Đầu ra | [1,2,3,4,5] | Có | 

Biểu đồ đã được kết nối vì đỉnh 3 chia sẻ bit với cả hai bên và đóng vai trò là cầu nối. 

### Ví dụ 2 

đầu vào:```
2
3 12
```| Bước | Mảng | Đã kết nối | 
| --- | --- | --- | 
| Ban đầu | [3,12] | Không | 
| Hãy thử -1 trước | [2,12] | Không | 
| Khôi phục | [3,12] | Không | 
| Hãy thử +1 trước | [4,12] | Có | 

Thuật toán phát hiện ra giải pháp một thao tác hợp lệ ngay lập tức. Bất kỳ giải pháp một thao tác nào cũng tối ưu vì ban đầu đồ thị không được kết nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 \cdot 30)$| Lên đến$2n$kiểm tra kết nối, mỗi lần xử lý$n$số và 30 bit | 
| Không gian |$O(30)$| DSU chỉ trên các vị trí 30 bit | 

Vì tổng số tiền của$n$trên tất cả các trường hợp thử nghiệm nhiều nhất là 2000, độ phức tạp này dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string

import sys
import io

def run(inp: str) -> str:
    # omitted, intended for local testing
    pass

# sample-style sanity checks

# already connected
inp = """1
5
1 2 3 4 5
"""

# minimum size with zeros
inp2 = """1
2
0 0
"""

# disconnected pair requiring one operation
inp3 = """1
2
3 12
"""

# all equal
inp4 = """1
4
7 7 7 7
"""

# large lowbit component case
inp5 = """1
4
8 8 1 1
"""

# boundary value
inp6 = """1
2
1073741823 1073741823
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0`| Mảng được kết nối sau hai lần sửa lỗi | Xử lý bằng không bắt buộc | 
|`3 12`| Một thao tác | Tìm kiếm một bước chính xác | 
|`7 7 7 7`| Không hoạt động | Biểu đồ đã được kết nối | 
|`8 8 1 1`| Sử dụng sửa chữa mang tính xây dựng | Chi nhánh hai hoạt động | 
| Cặp giá trị tối đa | Không hoạt động | Độ chính xác bit cao | 

## Vỏ cạnh 

Hãy xem xét:```
2
0 2
```Đỉnh tương ứng với số 0 không có cạnh. Thuật toán ngay lập tức chuyển đổi số 0 thành một. Sau đó nó kiểm tra kết nối trên`[1,2]`. Nếu vẫn bị ngắt kết nối thì các giai đoạn sau sẽ xử lý. Phần quan trọng là không có giải pháp kết nối nào có thể giữ được số 0. 

Coi như:```
2
3 12
```Sau khi xử lý bằng 0, không có gì thay đổi. Biểu đồ bị ngắt kết nối. Tìm kiếm một thao tác sẽ kiểm tra mọi mức tăng và giảm. Thay đổi`3`ĐẾN`4`sản lượng:```
4 & 12 = 4
```do đó biểu đồ trở nên được kết nối và thuật toán trả về chính xác câu trả lời 1. 

Hãy xem xét:```
4
8 8 1 1
```Không có sửa đổi một thao tác nào thành công. Thuật toán đạt đến giai đoạn xây dựng. Lowbit tối đa là 8, đạt được bởi cả hai bản sao 8. Một bản giảm và một bản khác tăng, tạo ra cấu hình được kết nối được đảm bảo trong đúng hai thao tác bổ sung. Vì tất cả các khả năng thực hiện một thao tác đều đã hết nên câu trả lời này là tối ưu.
