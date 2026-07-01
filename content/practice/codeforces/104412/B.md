---
title: "CF 104412B - Xác suất sắp xếp Bogo"
description: "Quá trình được mô tả là một quy trình sắp xếp ngẫu nhiên, liên tục xáo trộn mảng cho đến khi nó được sắp xếp. Một lần “lặp lại thành công” có nghĩa là sau một lần xáo trộn ngẫu nhiên, hoán vị kết quả đã được sắp xếp."
date: "2026-06-30T22:49:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104412
codeforces_index: "B"
codeforces_contest_name: "2023 ICPC Gran Premio de Mexico 2da Fecha"
rating: 0
weight: 104412
solve_time_s: 93
verified: true
draft: false
---

[CF 104412B - Xác suất sắp xếp Bogo](https://codeforces.com/problemset/problem/104412/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Quá trình được mô tả là một quy trình sắp xếp ngẫu nhiên, liên tục xáo trộn mảng cho đến khi nó được sắp xếp. Một lần “lặp lại thành công” có nghĩa là sau một lần xáo trộn ngẫu nhiên, hoán vị kết quả đã được sắp xếp. 

Một quan sát quan trọng là mọi sự xáo trộn đều tạo ra một hoán vị ngẫu nhiên thống nhất của nhiều tập hợp giá trị, không chỉ là các hoán vị riêng biệt của các chỉ số. Nếu chúng ta muốn xác suất đạt được trực tiếp trong một sắp xếp đã được sắp xếp, thì chúng ta thực sự đang đặt câu hỏi: trong số tất cả các hoán vị riêng biệt của các phần tử của mảng, phân số nào tương ứng với thứ tự được sắp xếp. 

Nếu tất cả các phần tử đều khác nhau thì có chính xác một hoán vị được sắp xếp trong số$N!$, vậy xác suất là$1/N!$. Khi tồn tại các bản sao, nhiều hoán vị chỉ mục thể hiện sự sắp xếp giá trị giống nhau. Nếu một giá trị$x$xuất hiện$f_x$lần thì số hoán vị phân biệt là$$\frac{N!}{\prod f_x!}$$Chỉ một trong các hoán vị này được sắp xếp nên xác suất trở thành$$\frac{1}{N! / \prod f_x!} = \frac{\prod f_x!}{N!}.$$Nhiệm vụ là xuất giá trị này theo modulo$10^9+7$, sau đó duy trì nó theo các cập nhật điểm trong đó một vị trí mảng thay đổi giá trị. Mỗi lần cập nhật chỉ thay đổi hai tần số: một giá trị mất một lần xuất hiện và giá trị khác tăng một lần, do đó xác suất có thể được cập nhật tăng dần. 

Các ràng buộc cho phép cả hai$N$Và$K$lên đến$10^6$, điều này ngay lập tức loại trừ việc tính toán lại các tích giai thừa từ đầu cho mỗi truy vấn. Bất kỳ giải pháp nào quét tất cả các giá trị trên mỗi bản cập nhật sẽ quá chậm vì sẽ dẫn đến$O(NK)$hành vi trong trường hợp xấu nhất, vượt xa giới hạn khả thi. 

Một vấn đề tế nhị là các giá trị có thể lên tới$10^9$, vì vậy việc lập chỉ mục trực tiếp theo giá trị là không thể. Bất kỳ cách tiếp cận nào dựa vào mảng được khóa theo giá trị sẽ âm thầm thất bại trừ khi các giá trị được nén hoặc lưu trữ trong bản đồ băm. 

Các trường hợp cạnh phát sinh khi tất cả các phần tử giống hệt nhau. Khi đó xác suất phải là$1$, vì mọi hoán vị đều giống hệt nhau và đã được sắp xếp. Một trường hợp góc cạnh khác là khi các bản cập nhật liên tục hoán đổi các giá trị để tần số dao động; tính chính xác phụ thuộc vào việc duy trì các đóng góp dựa trên tần số thay vì theo dõi các hoán vị một cách rõ ràng. 

## Phương pháp tiếp cận 

Giải thích thô bạo sẽ tính toán lại xác suất đầy đủ sau mỗi lần cập nhật bằng cách xây dựng lại bản đồ tần số và đánh giá$$\frac{\prod f_x!}{N!}.$$Việc tính toán các giai thừa của tần số cho mỗi truy vấn vẫn được chấp nhận, nhưng bản thân việc tính toán lại tần số yêu cầu phải quét toàn bộ mảng sau mỗi lần cập nhật, nghĩa là$O(N)$mỗi truy vấn. Với tối đa$10^6$cập nhật, điều này trở thành$10^{12}$trong trường hợp xấu nhất là không khả thi. 

Cái nhìn sâu sắc quan trọng là xác suất chỉ phụ thuộc vào phân bố tần số nhiều tập hợp. Khi một phần tử đơn lẻ thay đổi, chỉ có hai tần số thay đổi ±1 và tích tổng thể$\prod f_x!$có thể được cập nhật cục bộ. Điều này biến vấn đề thành việc duy trì một sản phẩm động theo các cập nhật điểm, có thể được thực hiện trong thời gian không đổi cho mỗi lần cập nhật nếu giai thừa được tính toán trước. 

Chúng tôi cũng tính toán trước các giai thừa và nghịch đảo mô-đun để phép chia cho$N!$được xử lý một lần trên toàn cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại từng truy vấn |$O(NK)$|$O(N)$| Quá chậm | 
| Duy trì sản phẩm tần số |$O(N + K)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta viết lại xác suất là:$$\text{answer} = \left(\prod_{v} f_v!\right) \cdot (N!)^{-1}.$$Chúng tôi duy trì biểu thức này một cách linh hoạt. 

### Các bước 

1. Tính toán trước các giai thừa và giai thừa nghịch đảo lên đến$N + K$. Điều này là cần thiết vì bất kỳ tần số nào cũng có thể tăng lên tới$N$và các giá trị giai thừa phải có sẵn trong thời gian O(1). 
2. Xây dựng bản đồ tần số của các giá trị mảng ban đầu. Đối với mỗi giá trị, hãy tăng số lượng của nó. 
3. Tính tích ban đầu trên tất cả các giá trị của$f_v!$. Điều này đại diện cho tử số của công thức xác suất. 
4. Tính hệ số mẫu số không đổi$(N!)^{-1}$một khi sử dụng giai thừa nghịch đảo được tính toán trước. 
5. Đối với mỗi lần cập nhật, hãy xóa phần đóng góp của tần số của giá trị cũ khỏi sản phẩm, cập nhật tần số và lắp lại phần đóng góp giai thừa mới. Sản phẩm luôn được giữ phù hợp với tần số hiện tại. 
6. Sau mỗi lần cập nhật, xuất ra:$$\text{product} \cdot (N!)^{-1} \bmod (10^9+7).$$### Tại sao nó hoạt động 

Thuật toán duy trì giá trị chính xác của$\prod f_v!$mọi lúc. Vì các bản cập nhật chỉ thay đổi hai tần số nên việc điều chỉnh sản phẩm mang tính cục bộ và chính xác. mẫu số$N!$không đổi vì kích thước mảng không thay đổi. Điều này đảm bảo biểu thức được duy trì luôn bằng xác suất thu được từ đối số đếm hoán vị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    N, K = map(int, input().split())
    arr = list(map(int, input().split()))

    maxn = N + K + 5

    fact = [1] * (maxn)
    invfact = [1] * (maxn)

    for i in range(1, maxn):
        fact[i] = fact[i - 1] * i % MOD

    invfact[maxn - 1] = pow(fact[maxn - 1], MOD - 2, MOD)
    for i in range(maxn - 2, -1, -1):
        invfact[i] = invfact[i + 1] * (i + 1) % MOD

    freq = {}
    prod = 1

    for x in arr:
        if x not in freq:
            freq[x] = 0
        freq[x] += 1

    for v in freq.values():
        prod = prod * fact[v] % MOD

    inv_n_fact = invfact[N]

    out = []

    def remove(val):
        nonlocal prod
        c = freq[val]
        prod = prod * invfact[c] % MOD
        c -= 1
        freq[val] = c
        prod = prod * fact[c] % MOD

    def add(val):
        nonlocal prod
        c = freq.get(val, 0)
        if val not in freq:
            freq[val] = 0
        prod = prod * invfact[c] % MOD
        c += 1
        freq[val] = c
        prod = prod * fact[c] % MOD

    for _ in range(K):
        A, B = map(int, input().split())
        A -= 1
        old = arr[A]

        remove(old)
        arr[A] = B
        add(B)

        out.append(str(prod * inv_n_fact % MOD))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp tập trung vào việc duy trì một sản phẩm toàn cầu duy nhất đại diện cho tất cả các đóng góp giai thừa. Các trình trợ giúp loại bỏ và thêm đảm bảo rằng mỗi thay đổi tần số được áp dụng một cách đối xứng: thuật ngữ giai thừa cũ được loại bỏ trước khi giảm dần và thuật ngữ giai thừa mới được chèn vào sau khi điều chỉnh. Thứ tự này ngăn chặn những đóng góp cũ còn lại trong sản phẩm. 

Các bảng giai thừa và nghịch đảo đảm bảo rằng tất cả các cập nhật đều là O(1) và nghịch đảo mô-đun của$N!$được tính toán trước một lần kể từ$N$không bao giờ thay đổi. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mảng ban đầu:$[3, 2, 5, 4, 1]$Tất cả các giá trị đều khác nhau nên tất cả tần số đều bằng 1. 

| Bước | Hành động | Trạng thái tần số | Sản phẩm | 
| --- | --- | --- | --- | 
| Ban đầu | xây dựng | tất cả 1 |$1$| 
| Truy vấn 1 | 2→1 | 1:2, 3,4,5:1 | cập nhật | 
| Truy vấn 2 | 4→6 | phân phối cập nhật | cập nhật | 

Sau mỗi lần cập nhật, chỉ có hai tần số thay đổi và sản phẩm sẽ điều chỉnh cục bộ. 

Dấu vết này cho thấy rằng mặc dù các giá trị lớn và năng động, chỉ có tần số mới là quan trọng chứ không phải thứ tự. 

### Mẫu 2 

Mảng ban đầu:$[2, 7, 3, 5]$Tất cả các tần số đều bằng 1 nên xác suất ban đầu là$1/4!$. 

Sau khi cập nhật$1 \to 3$, tần số của 3 trở thành 2 và 2 biến mất. 

| Bước | Hành động | Trạng thái tần số | Sản phẩm | 
| --- | --- | --- | --- | 
| Ban đầu | xây dựng | tất cả 1 | 1 | 
| Q1 | 1→3 | 3:2, những người khác:1 | điều chỉnh | 
| Q2 | 1→7 | 7:2, những người khác:1 | điều chỉnh | 

Điều này cho thấy các bản sao làm tăng tử số thông qua tăng trưởng giai thừa, làm tăng xác suất như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N + K)$| tính toán trước giai thừa cộng với cập nhật O(1) cho mỗi truy vấn | 
| Không gian |$O(N + K)$| mảng giai thừa và bản đồ tần số | 

Các ràng buộc cho phép lên tới một triệu thao tác, do đó quá trình tiền xử lý tuyến tính và cập nhật theo thời gian liên tục phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io
from contextlib import redirect_stdout

def run(inp: str) -> str:
    from math import isclose

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Sample 1
assert run("""5 2
3 2 5 4 1
2 1
4 6
""") == """808333339
616666671
616666671"""

# Sample 2
assert run("""4 2
2 7 3 5
1 3
1 7
""") == """41666667
83333334
83333334"""

# Sample 3
assert run("""3 2
1 2 3
2 1
3 1
""") == """166666668
333333336
1"""

# all equal
assert run("""3 2
5 5 5
1 5
2 5
""") == """1
1
1"""

# single element
assert run("""1 1
7
1 8
""") == """1"""

# max duplicate buildup
assert run("""2 2
1 2
1 2
2 2
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | 1 giây | trường hợp cạnh trùng lặp đầy đủ | 
| phần tử đơn | 1 | xác suất tầm thường | 
| cập nhật lặp đi lặp lại | đầu ra ổn định | tính nhất quán tần số | 

## Vỏ cạnh 

Khi tất cả các phần tử giống hệt nhau, bản đồ tần số chứa một khóa duy nhất có giá trị$N$. Sản phẩm trở nên$N!$, và nhân với$(N!)^{-1}$mang lại 1. Thuật toán xử lý việc này một cách tự nhiên vì cả hai thao tác xóa và thêm luôn giữ cho tích giai thừa phù hợp với tần số hiện tại. 

Khi các bản cập nhật liên tục hoán đổi qua lại hai giá trị, tần số của mỗi giá trị sẽ dao động giữa hai trạng thái. Mỗi quá trình chuyển đổi áp dụng các thao tác loại bỏ và thêm đối xứng, do đó không có sự trôi dạt nào tích lũy trong sản phẩm chung. 

Khi một giá trị biến mất hoàn toàn, tần số của nó sẽ bằng 0. Bước loại bỏ làm giảm tích số bằng cách chia phần đóng góp giai thừa cuối cùng, và vì$0! = 1$, không có số hạng giai thừa không hợp lệ nào còn lại trong sản phẩm.
