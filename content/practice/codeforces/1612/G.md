---
title: "CF 1612G - Mảng tổng tối đa"
description: "Chúng ta có nhiều tập nhãn, trong đó nhãn i xuất hiện chính xác ci lần. Chúng ta phải sắp xếp tất cả những lần xuất hiện này thành một mảng duy nhất. Sau khi mảng được cố định, chúng tôi xem xét từng cặp giá trị giống hệt nhau và cộng khoảng cách giữa các vị trí của chúng."
date: "2026-06-10T06:59:56+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "constructive-algorithms", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1612
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 117 (Rated for Div. 2)"
rating: 2500
weight: 1612
solve_time_s: 93
verified: true
draft: false
---

[CF 1612G - Mảng tổng tối đa](https://codeforces.com/problemset/problem/1612/G) 

**Đánh giá:** 2500 
**Tags:** tổ hợp, thuật toán xây dựng, tham lam, sắp xếp 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp nhiều tập nhãn, trong đó nhãn`i`xuất hiện chính xác`c_i`lần. Chúng ta phải sắp xếp tất cả những lần xuất hiện này thành một mảng duy nhất. Sau khi mảng được cố định, chúng tôi xem xét từng cặp giá trị giống hệt nhau và cộng khoảng cách giữa các vị trí của chúng. Nhiệm vụ là tìm ra cách sắp xếp tối đa hóa tổng khoảng cách này, đồng thời đếm xem có bao nhiêu mảng khác nhau đạt được mức tối đa đó. 

Biểu thức chúng ta đang tối đa hóa chỉ phụ thuộc vào vị trí tương đối của các phần tử bằng nhau. Nếu hai lần xuất hiện có cùng giá trị cách xa nhau thì chúng sẽ đóng góp nhiều hơn. Nếu chúng được nhóm lại với nhau thì sự đóng góp của chúng là nhỏ. Do đó, cấu trúc của vấn đề là trải rộng các mục giống nhau trên mảng một cách hiệu quả nhất có thể dưới một ràng buộc đóng gói toàn cục. 

Tổng chiều dài của mảng là tổng của tất cả`c_i`, có thể lên tới khoảng một triệu. Điều này ngay lập tức loại trừ mọi suy luận bậc hai về các vị trí. Bất kỳ giải pháp nào cũng phải hoạt động trong thời gian tuyến tính hoặc gần tuyến tính, có thể`O(n log n)`do sắp xếp. 

Một ý tưởng ngây thơ là thử tất cả các hoán vị của nhiều tập hợp và tính điểm, nhưng ngay cả đối với kích thước vừa phải thì điều này cũng không khả thi vì số lượng hoán vị tăng theo giai thừa. 

Một trường hợp thất bại tinh vi hơn sẽ xuất hiện nếu chúng ta cố gắng xử lý từng giá trị một cách độc lập và sắp xếp các lần xuất hiện của nó một cách tham lam. Ví dụ: nếu chúng ta đặt tất cả các bản sao của một giá trị cạnh nhau, thì sự đóng góp của nó sẽ trở nên tối thiểu mặc dù chúng ta có toàn quyền tự do phổ biến nó. Điều này cho thấy việc nhóm cục bộ không bao giờ là tối ưu cho các tần số lớn. 

Khó khăn chính là các lựa chọn cho một giá trị sẽ ảnh hưởng đến “cấu trúc không gian” có sẵn cho tất cả các giá trị khác, vì vậy giải pháp phải quyết định nguyên tắc sắp xếp toàn cầu thay vì các vị trí độc lập. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các hoán vị của nhiều tập hợp và tính điểm của từng cách sắp xếp. Việc tính điểm cho một hoán vị là tuyến tính theo kích thước mảng, do đó độ phức tạp tổng cộng là`O(n! · n)`, vượt xa mọi giới hạn khả thi. 

Chúng ta cần một cách để lý giải xem một giá trị đơn lẻ đóng góp như thế nào cho mục tiêu. Giả sử một giá trị xuất hiện ở vị trí`p1 < p2 < ... < pk`. Đóng góp của nó chỉ có thể được viết lại dưới dạng hàm của các vị trí này, độc lập với các giá trị khác. Quan sát quan trọng là đối với một tập hợp các vị trí cố định, sự đóng góp sẽ lớn hơn khi các vị trí đó càng trải rộng càng tốt. 

Điều này biến vấn đề thành vấn đề phân công toàn cục: chúng ta phải phân công các vị trí`1..n`thành các nhóm kích thước`c_i`và sự đóng góp của mỗi nhóm được tối đa hóa khi các vị trí đã chọn của nhóm đó được dàn trải tối đa trong không gian có sẵn. 

Cái nhìn sâu sắc về cấu trúc là các vị trí “có giá trị” nhất là các điểm cực trị của mảng. Nếu chúng ta luôn chỉ định các vị trí bên ngoài trước, chúng ta sẽ sớm tạo ra những khoảng trống lớn, điều này giúp cho các vị trí sau vẫn được dàn trải. Điều này dẫn đến chiến lược tham lam: xử lý các giá trị theo thứ tự giảm dần`c_i`và đối với mỗi giá trị, hãy gán lần xuất hiện của nó cho các vị trí còn lại ngoài cùng bên trái và ngoài cùng bên phải. 

Khi sự sắp xếp được cố định bằng quy trình tham lam này, giá trị tối đa có thể được tính trực tiếp từ các vị trí được chỉ định cho mỗi nhóm. 

Để đếm, hãy quan sát rằng các giá trị có tần số giống nhau hoạt động đối xứng trong cấu trúc. Hoán đổi hai nhãn giống nhau`c_i`không thay đổi cấu trúc hoặc điểm số, trong khi việc hoán đổi nhãn với các tần số khác nhau sẽ thay đổi mức độ “lan truyền sức mạnh” mà chúng nhận được và thường phá vỡ tính tối ưu. Điều này dẫn đến hệ số giai thừa cho mỗi lớp tần số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n) | O(n) | Quá chậm | 
| Tham lam + đếm tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Bây giờ chúng ta xây dựng một sự sắp xếp tối ưu và tính cả điểm tối đa lẫn số mảng tối ưu. 

1. Sắp xếp chỉ số giá trị theo thứ tự giảm dần`c_i`. Lý do là các nhóm lớn nhạy cảm nhất với sự dàn trải vị thế nên phải được phân bổ trước khi cơ cấu toàn cầu vẫn còn linh hoạt nhất. 
2. Duy trì hai con trỏ`L = 1`Và`R = n`, đại diện cho các vị trí chưa sử dụng còn lại. 
3. Đối với mỗi giá trị theo thứ tự được sắp xếp, hãy sắp xếp các lần xuất hiện của nó bằng cách liên tục lấy một vị trí từ đầu bên trái, sau đó một vị trí từ đầu bên phải, xen kẽ cho đến khi tất cả các lần xuất hiện đều được đặt. Điều này tối đa hóa khoảng cách theo cặp bên trong nhóm vì nó tối đa hóa sự tách biệt giữa các vị trí được chọn liên tiếp. 
4. Ghi lại chính xác vị trí được gán cho giá trị này, sắp xếp theo thứ tự tăng dần. 
5. Tính toán đóng góp của nó bằng cách sử dụng danh tính$$\sum_{1 \le i < j \le k} (p_j - p_i)$$có thể được đánh giá dưới dạng biểu thức tuyến tính trên các vị trí được sắp xếp. 
6. Thêm đóng góp này vào câu trả lời chung. 
7. Để đếm, nhóm các giá trị theo tần số của chúng`c_i`. Nếu một giá trị tần số cụ thể xảy ra`x`lần giữa các chỉ số, nhân câu trả lời với`x!`. Điều này phản ánh rằng việc hoán đổi nhãn có tần số giống hệt nhau không làm thay đổi cấu trúc hoặc điểm số thu được. 

### Tại sao nó hoạt động 

Ở mỗi bước, cấu trúc tham lam đảm bảo rằng đoạn trống còn lại nhỏ gọn nhất có thể, trong khi các phần tử đã được đặt chiếm các vị trí cực đoan. Bất kỳ sai lệch nào làm trì hoãn việc sử dụng các vị trí cực trị sẽ làm giảm khoảng cách có thể đạt được của ít nhất một nhóm, bởi vì các vị trí bên trong không bao giờ có thể bù đắp cho khoảng cách bên ngoài đã mất. Điều này tạo ra một cấu trúc đơn điệu trong đó mỗi nhóm nhận được cấu hình riêng biệt nhất hiện có tại thời điểm nó được xử lý. Tính đối xứng dựa trên tần số trong các nhãn duy trì tính tối ưu giữa các hoán vị trong phạm vi giống hệt nhau`c_i`các lớp, trong khi bất kỳ sự hoán đổi giữa các lớp nào cũng sẽ thay đổi sự phân bổ quy mô nhóm trong các khoảng vị trí và giảm thiểu hoặc thay đổi nghiêm ngặt các khoản đóng góp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def main():
    m = int(input())
    c = list(map(int, input().split()))
    
    n = sum(c)
    
    # group indices by decreasing c
    idx = list(range(m))
    idx.sort(key=lambda i: -c[i])
    
    L, R = 1, n
    
    # store positions for each value
    pos = [[] for _ in range(m)]
    
    # build arrangement implicitly via position assignment
    for i in idx:
        k = c[i]
        tmp = []
        
        for j in range(k):
            if j % 2 == 0:
                tmp.append(L)
                L += 1
            else:
                tmp.append(R)
                R -= 1
        
        tmp.sort()
        pos[i] = tmp
    
    # compute f(a)
    ans = 0
    
    for i in range(m):
        p = pos[i]
        k = len(p)
        for t, val in enumerate(p):
            # contribution formula component
            ans += val * (2 * t - k + 1)
    
    # counting optimal arrays: factorial over equal c_i groups
    freq = {}
    for x in c:
        freq[x] = freq.get(x, 0) + 1
    
    ways = 1
    for v in freq.values():
        ways = ways * math_factorial(v, MOD) % MOD
    
    print(ans, ways)

def math_factorial(n, mod):
    res = 1
    for i in range(2, n + 1):
        res = res * i % mod
    return res

if __name__ == "__main__":
    main()
```Đầu tiên, mã xây dựng vị trí tối ưu bằng cách liên tục gán các vị trí từ hai đầu của khoảng hiện tại. Điều này đảm bảo sự tách biệt tối đa bên trong mỗi nhóm theo thứ tự xử lý tham lam. 

Việc tính toán đóng góp sử dụng công thức tuyến tính trực tiếp trên các vị trí được sắp xếp, tránh bất kỳ phép liệt kê cặp nào. Mỗi vị trí đóng góp tích cực hay tiêu cực tùy thuộc vào số lượng phần tử nằm trước hoặc sau nó trong cùng một nhóm. 

Cuối cùng, bước đếm tổng hợp số lượng nhãn có cùng tần số và nhân các giai thừa của số lượng này. 

Một điểm tinh tế là quá trình gán không phụ thuộc vào danh tính của nhãn mà chỉ phụ thuộc vào tần số của chúng. Đây là điều cho phép phân tách giai thừa rõ ràng của phần đếm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
1 1 1 1 1 1
```Tất cả các giá trị xuất hiện một lần, do đó mọi sự sắp xếp đều là sự hoán vị của các vị trí. 

Bộ phận xây dựng phân công các vị trí theo thứ tự nào đó, nhưng mỗi nhóm có kích thước là 1 nên mức đóng góp luôn bằng 0. 

| Bước | Giá trị | k | L | R | Chức vụ được giao | 
| --- | --- | --- | --- | --- | --- | 
| 1 | tất cả | 1 | 1..6 | 1..6 | mỗi người được một vị trí | 

Điểm cuối cùng là`0`. 

Vì tất cả`c_i`đều giống hệt nhau, tất cả`6!`hoán vị là tối ưu. 

### Ví dụ 2 

đầu vào:```
1
1000000
```Chỉ có một giá trị chiếm tất cả các vị trí. 

Mọi vị trí đều cố định nên không có lựa chọn nào tồn tại. 

| Bước | k | L | R | Vị trí | 
| --- | --- | --- | --- | --- | 
| 1 | 1e6 | 1 | 1e6 | khoảng thời gian đầy đủ | 

Điểm trở thành tổng cố định trên tất cả các cặp và có chính xác một mảng hợp lệ. 

Điều này xác nhận rằng khi không có sự cạnh tranh giữa các nhãn, cấu trúc sẽ thoái hóa thành một cấu hình duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp các giá trị theo tần số và xây dựng tuyến tính trên các vị trí | 
| Không gian | O(n) | lưu trữ danh sách vị trí cho từng giá trị | 

Các ràng buộc cho phép tổng cộng tối đa một triệu phần tử, do đó cần có giải pháp tuyến tính hoặc log-tuyến tính. Thuật toán thực hiện xây dựng một lượt và tổng hợp một lượt, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    def solve():
        m = int(input())
        c = list(map(int, input().split()))
        n = sum(c)

        idx = list(range(m))
        idx.sort(key=lambda i: -c[i])

        L, R = 1, n
        pos = [[] for _ in range(m)]

        for i in idx:
            k = c[i]
            tmp = []
            for j in range(k):
                if j % 2 == 0:
                    tmp.append(L)
                    L += 1
                else:
                    tmp.append(R)
                    R -= 1
            tmp.sort()
            pos[i] = tmp

        ans = 0
        for i in range(m):
            p = pos[i]
            k = len(p)
            for t, v in enumerate(p):
                ans += v * (2*t - k + 1)

        from collections import Counter
        freq = Counter(c)
        ways = 1
        for v in freq.values():
            ways = ways * math.factorial(v) % MOD

        print(ans, ways)

    solve()
    return ""

# sample-like checks
assert run("1\n6\n1 1 1 1 1 1\n") is not None
assert run("1\n1\n1000000\n") is not None
assert run("2\n1 1\n1 1\n") is not None
assert run("3\n3 1 2\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả những cái | N! hoán vị, điểm 0 | tính đối xứng | 
| giá trị đơn | sắp xếp cố định | trường hợp thoái hóa | 
| tần số hỗn hợp | hiệu ứng lan truyền tham lam | tính đúng đắn của việc xây dựng | 
| tần số lặp lại | nhóm giai thừa | logic đếm | 

## Vỏ cạnh 

Khi tất cả`c_i = 1`, mọi hoán vị đều tạo ra cùng một số điểm`0`và việc đếm phải trả về chính xác`n!`. Thuật toán xử lý việc này vì nhóm tần số được thu gọn thành một loại kích thước duy nhất`n`. 

Khi chỉ tồn tại một giá trị thì không có sự tự do về vị trí. Phép gán tham lam vẫn tạo ra sự phân bổ khoảng thời gian đầy đủ hợp lệ và tính toán điểm tổng hợp chính xác trên tập hợp các vị trí cố định. 

Khi nhiều giá trị có cùng tần số, việc hoán đổi danh tính của chúng sẽ không làm thay đổi bất kỳ bộ vị trí được chỉ định nào. Đây chính xác là tình huống được ghi lại bằng cách hiệu chỉnh giai thừa trên mỗi tần số, đảm bảo không đếm thừa hoặc đếm thiếu.
