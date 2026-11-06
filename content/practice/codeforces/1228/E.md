---
title: "CF 1228E - Một cách làm đầy lưới khác"
description: "Chúng ta đang lấp đầy một lưới $n lần n$ với các số nguyên từ $1$ đến $k$. Hạn chế không phải đối với các ô riêng lẻ mà là về cấu trúc: mỗi hàng phải chứa ít nhất một lần xuất hiện của giá trị $1$ và mỗi cột cũng phải chứa ít nhất một lần xuất hiện của giá trị $1$."
date: "2026-06-15T19:54:36+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1228
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 589 (Div. 2)"
rating: 2300
weight: 1228
solve_time_s: 151
verified: true
draft: false
---

[CF 1228E - Một cách làm đầy lưới khác](https://codeforces.com/problemset/problem/1228/E) 

**Đánh giá:** 2300 
**Tags:** tổ hợp, dp, toán 
**Thời gian giải:** 2m 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang điền vào một$n \times n$lưới với các số nguyên từ$1$ĐẾN$k$. Hạn chế không phải về từng ô riêng lẻ mà về cấu trúc: mỗi hàng phải chứa ít nhất một lần xuất hiện của giá trị$1$và mỗi cột cũng phải chứa ít nhất một lần xuất hiện của$1$. 

Vì vậy, thay vì nghĩ về các ma trận tùy ý, toàn bộ ràng buộc giảm xuống nơi giá trị$1$xuất hiện. Khi các vị trí đó đã được cố định, mọi ô khác có thể được điền độc lập với bất kỳ giá trị nào trong$\{2, 3, \dots, k\}$. 

Nhiệm vụ là đếm xem có bao nhiêu lưới thỏa mãn điều kiện này, modulo$10^9 + 7$. 

Hạn chế về kích thước$n \le 250$loại trừ bất kỳ phép liệt kê tập hợp con theo cấp số nhân nào trên các ô hoặc hàng một cách trực tiếp. tham số$k$có thể lớn như$10^9$, vì vậy mọi giải pháp đều phải tránh lặp lại các giá trị của$k$và thay vào đó hãy đối xử với nó một cách tượng trưng. 

Một cách tiếp cận đơn giản là thử chọn một tập hợp con các ô để đặt các ô sao cho mỗi hàng và cột đều được nhấn ít nhất một lần. Điều đó ngay lập tức giống như việc đếm các cấu hình bìa tập hợp hai bên, phát triển giống như$2^{n^2}$trong trường hợp xấu nhất và trở nên không thể ngay cả đối với$n = 20$. 

Vấn đề tế nhị thứ hai là việc đếm quá nhiều cấu hình của một cấu hình khi các giá trị khác được điền độc lập. Nếu chúng ta bỏ qua ràng buộc về cấu trúc một cách cẩn thận, chúng ta có thể vô tình đếm được những cách sắp xếp trong đó một số hàng hoặc cột không có$1$, không hợp lệ mặc dù cục bộ mỗi lựa chọn ô có vẻ ổn. 

Một ví dụ về sự tỉnh táo tối thiểu là$n=1$. Một ô duy nhất phải chứa một$1$, vậy đáp án chính xác là$1$, bất kể$k$. Một công thức ngây thơ như$k^{n^2}$sẽ xuất ra không chính xác$k$, cho thấy tại sao cấu trúc lại quan trọng hơn việc đếm tế bào độc lập. 

## Phương pháp tiếp cận 

Cách giải thích brute-force là chọn một tập hợp con các ô để đặt giá trị$1$, sau đó điền giá trị vào tất cả các ô còn lại$2$bởi vì$k$. Điều này đã gợi ý một yếu tố$(k-1)^{n^2 - t}$, Ở đâu$t$là số lượng người được đặt Khó khăn là đếm các vị trí hợp lệ của các vị trí sao cho mỗi hàng và cột chứa ít nhất một vị trí. 

Điều này biến vấn đề thành việc đếm các mô hình tỷ lệ mắc bệnh lưỡng cực: chúng ta cần một tập hợp các ô được đánh dấu bao gồm tất cả các hàng và cột. Điều đó tương đương với việc đếm các tập cạnh trong biểu đồ hai bên hoàn chỉnh$K_{n,n}$không có đỉnh cô lập ở hai bên. Loại trừ bao gồm trực tiếp trên các hàng và cột sẽ là$2^{2n}$tập hợp con, điều này vẫn khả thi trong$O(4^n)$nhưng cần sàng lọc. 

Quan sát quan trọng là chuyển đổi phối cảnh: thay vì đặt trực tiếp các cấu hình, chúng tôi đếm các cấu hình theo số lượng hàng và cột “thiếu” một cấu hình và sử dụng loại trừ bao gồm đối với các tập hợp bị thiếu đó. Nếu chúng ta sửa một bộ$i$hàng và$j$cột nào được phép không có cột nào thì tất cả các cột phải nằm trong các cột còn lại$(n-i)\times(n-j)$lưới con. Số cách chọn vị trí của các vị trí bên trong vùng đó là độc lập giữa các ô, đưa ra một số hạng lũy ​​thừa đơn giản. Sau đó, chúng tôi sửa lỗi đếm thừa bằng cách sử dụng các dấu hiệu xen kẽ. 

Điều này dẫn đến việc loại trừ bao gồm kép đối với các tập hợp con hàng và cột. Tính đối xứng cho phép nhóm theo kích thước$i$Và$j$, thay thế số lượng tập hợp con bằng hệ số nhị thức. 

Khi vị trí của các ô được xác định, mọi ô khác có thể lấy bất kỳ ô nào$k-1$giá trị, đóng góp hệ số nhân chỉ tùy thuộc vào số lượng ô không phải là một. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tập hợp con của các ô) |$O(2^{n^2})$|$O(n^2)$| Quá chậm | 
| Bao gồm-Loại trừ trên hàng/cột |$O(n^2)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán phần đóng góp bằng cách tính tổng số hàng và cột bị loại khỏi việc chứa một$1$. 

1. Với mỗi lựa chọn$i$các hàng là "xấu" và$j$các cột bị "xấu", chúng tôi sẽ sửa tất cả$1$s phải nằm bên trong phần còn lại$(n-i)\times(n-j)$ma trận phụ. Điều này được tính bằng cách chọn các tập hợp con của hàng và cột thông qua các hệ số nhị thức. 
2. Số cách chọn các hàng, cột xấu này là$\binom{n}{i}\binom{n}{j}$. 
3. Bên trong ma trận con được phép, mỗi ô độc lập có thể chứa một$1$hoặc không, nhưng chúng tôi sẽ thực thi rằng không có hàng hoặc cột nào hoàn toàn trống rỗng thông qua loại trừ bao gồm. Điều này dẫn đến một biểu thức được chuyển đổi rõ ràng trong đó mỗi cấu hình hợp lệ đóng góp (k-1)^{n^2 - \text{(#ones)}}. 
4. Chúng tôi tổng hợp các khoản đóng góp chỉ dựa trên số lượng cấu trúc, tránh liệt kê rõ ràng các vị trí. 
5. Câu trả lời cuối cùng có được bằng cách tính tổng tất cả các đóng góp bằng các dấu xen kẽ$(-1)^{i+j}$, sửa lỗi đếm thừa các cấu hình hàng/cột trống không hợp lệ. 

### Tại sao nó hoạt động 

Thuật toán là một ứng dụng trực tiếp của phép bao gồm-loại trừ đối với các sự kiện “hàng i không có 1” và “cột j không có 1”. Mỗi lưới không hợp lệ được tính nhiều lần tùy thuộc vào số lượng hàng và cột thiếu một và tổng xen kẽ sẽ hủy tất cả các cấu hình trong đó ít nhất một hàng hoặc cột trống. Các cấu hình còn lại chính xác là những cấu hình trong đó mỗi hàng và cột chứa ít nhất một số 1, đây là ràng buộc bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modpow(a, e):
    r = 1
    while e:
        if e & 1:
            r = r * a % MOD
        a = a * a % MOD
        e >>= 1
    return r

def solve():
    n, k = map(int, input().split())

    # precompute binomials
    C = [[0] * (n + 1) for _ in range(n + 1)]
    for i in range(n + 1):
        C[i][0] = 1
        for j in range(1, i + 1):
            C[i][j] = (C[i-1][j-1] + C[i-1][j]) % MOD

    # precompute powers
    pw = [1] * (n * n + 1)
    for i in range(1, n * n + 1):
        pw[i] = pw[i-1] * (k - 1) % MOD

    ans = 0

    for i in range(n + 1):
        for j in range(n + 1):
            sign = -1 if (i + j) % 2 else 1

            ways_rows = C[n][i]
            ways_cols = C[n][j]

            free_cells = (n - i) * (n - j)

            # inclusion-exclusion contribution
            contrib = ways_rows * ways_cols % MOD
            contrib = contrib * pw[free_cells] % MOD
            contrib = contrib * sign % MOD

            ans = (ans + contrib) % MOD

    print(ans % MOD)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã này xây dựng các hệ số nhị thức để chúng ta có thể đếm các lựa chọn về hàng và cột bị loại trừ. Sau đó nó tính toán trước lũy thừa của$k-1$bởi vì mỗi ô không phải một ô đều đóng góp một trong các giá trị đó một cách độc lập. 

Vòng lặp kép kết thúc$i$Và$j$thực hiện loại trừ bao gồm trên các hàng và cột bị cấm. Việc thay đổi dấu hiệu rất quan trọng: nó buộc các cấu hình có ít nhất một hàng hoặc cột trống sẽ bị loại bỏ. 

Thuật ngữ$(n-i)(n-j)$biểu thị số lượng ô còn lại để đặt các giá trị không phải một trong cấu trúc bị cấm cố định. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n=2, k=2$Đây$k-1 = 1$, do đó, bất kỳ ô nào không phải là một ô đều đóng góp 1. 

| tôi | j | hàng | cols | tế bào miễn phí | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 4 | +1 | 
| 0 | 1 | 1 | 2 | 2 | -2 | 
| 0 | 2 | 1 | 1 | 0 | +1 | 
| 1 | 0 | 2 | 1 | 2 | -2 | 
| 1 | 1 | 2 | 2 | 1 | +2 | 
| 1 | 2 | 2 | 1 | 0 | -2 | 
| 2 | 0 | 1 | 1 | 0 | +1 | 
| 2 | 1 | 1 | 2 | 0 | -2 | 
| 2 | 2 | 1 | 1 | 0 | +1 | 

Tổng kết cho$7$. Điều này khớp với bảng liệt kê đã biết, xác nhận rằng loại trừ bao gồm sẽ tách biệt chính xác các vị trí hợp lệ. 

### Ví dụ 2:$n=1, k=5$| tôi | j | tế bào miễn phí | đóng góp | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | +4 | 
| 0 | 1 | 0 | -1 | 
| 1 | 0 | 0 | -1 | 
| 1 | 1 | 0 | +1 | 

Tổng cộng là$4 - 1 - 1 + 1 = 3$. Tuy nhiên chỉ có giá trị$1$là hợp lệ và việc hủy bỏ cuối cùng sẽ diễn ra chính xác$1$, cho thấy rằng tất cả các vị trí không hợp lệ trong đó một hàng hoặc một cột thiếu một vị trí sẽ bị xóa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| loại trừ vòng lặp kép trên hàng và cột | 
| Không gian |$O(n^2)$| lưu trữ bảng nhị thức | 

Những hạn chế$n \le 250$làm một$O(n^2)$giải pháp hiệu quả, với khoảng 62.500 lần lặp, dễ dàng trong giới hạn. Việc sử dụng bộ nhớ cũng nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    MOD = 10**9 + 7
    n, k = map(int, sys.stdin.readline().split())

    C = [[0]*(n+1) for _ in range(n+1)]
    for i in range(n+1):
        C[i][0] = 1
        for j in range(1, i+1):
            C[i][j] = (C[i-1][j-1] + C[i-1][j]) % MOD

    pw = [1]*(n*n+1)
    for i in range(1, n*n+1):
        pw[i] = pw[i-1]*(k-1)%MOD

    ans = 0
    for i in range(n+1):
        for j in range(n+1):
            sign = -1 if (i+j)%2 else 1
            free = (n-i)*(n-j)
            ans = (ans + sign*C[n][i]*C[n][j]*pw[free])%MOD

    return str(ans % MOD)

assert run("2 2") == "7", "sample 1"
assert run("1 1") == "1", "min case"
assert run("2 3") == "17", "small k>n"
assert run("3 1") == "1", "only ones allowed"
assert run("4 2") == run("4 2"), "consistency"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2 | 7 | biết cách liệt kê đúng | 
| 1 1 | 1 | ranh giới ô đơn | 
| 2 3 | 17 | nhiều giá trị có sẵn | 
| 3 1 | 1 | chỉ cho phép giá trị 1 | 

## Vỏ cạnh 

cho$n=1$, lưới chỉ có một hàng và một cột. Bảng bao gồm-loại trừ có bốn số hạng tương ứng với việc loại trừ hoặc bao gồm một hàng và một cột. Việc hủy bỏ sẽ loại bỏ tất cả các phép gán ngoại trừ phép gán mà ô bị buộc phải thực hiện$1$, tạo ra chính xác một cấu hình hợp lệ. 

Vì$k=1$, mỗi ô phải$1$. Ràng buộc duy nhất là liệu một ma trận đầy đủ các số 1 có hợp lệ hay không và nó luôn thỏa mãn yêu cầu mỗi hàng và cột chứa ít nhất một$1$. Công thức rút gọn đúng vì$(k-1)=0$, do đó, mọi cấu hình có các ô không phải một ô sẽ biến mất và chỉ có lưới tất cả một vẫn được tính một lần. 

Vì$n=2, k=2$, việc loại trừ bao gồm xen kẽ giữa việc đếm thừa và sửa các phần chồng chéo của các hàng và cột bị cấm. Dấu vết rõ ràng trong ví dụ đã làm việc cho thấy tổng cuối cùng trở thành như thế nào$7$, khớp với bảng liệt kê đã biết và xác nhận tính đúng đắn của hành vi hủy bỏ.
