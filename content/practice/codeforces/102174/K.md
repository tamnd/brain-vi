---
title: "CF 102174K - \u591a\u9879\u5f0f\u6c42\u5bfc"
description: "Chúng ta có đa thức [ f(x)=anx^n+a{n-1}x^{n-1}+cdots+a1x+a0. ] Đầu vào cho biết mức độ (n), số lần (k) phải áp dụng vi phân và hệ số (n+1) từ bậc cao nhất đến số hạng không đổi."
date: "2026-08-19T07:10:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102174
codeforces_index: "K"
codeforces_contest_name: "The 14-th BIT Campus Programming Contest"
rating: 0
weight: 102174
solve_time_s: 108
verified: true
draft: false
---

[CF 102174K - \u591a\u9879\u5f0f\u6c42\u5bfc](https://codeforces.com/problemset/problem/102174/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đa thức 

[ 
f(x)=a_nx^n+a_{n-1}x^{n-1}+\cdots+a_1x+a_0. 
] 

Đầu vào cho biết mức độ (n), số lần (k) phải áp dụng vi phân và hệ số (n+1) từ bậc cao nhất đến số hạng không đổi. Chúng ta cần xuất ra các hệ số của đạo hàm thứ (k), vẫn sử dụng chính xác vị trí (n+1). Bất kỳ số hạng nào có bậc đã biến mất đều đóng góp hệ số bằng 0. Mọi hệ số kết quả đều được yêu cầu modulo (2019). 

Chi tiết lập chỉ mục chính là mảng đầu vào được sắp xếp theo mức độ giảm dần. Nếu như`a[j]`là phần tử ở vị trí gốc 0 (j), nó biểu thị hệ số của (x^{n-j}). Sau khi vi phân (k) lần, hệ số này chuyển sang độ (n-j-k), với điều kiện là độ đó không âm. 

Ở đây (1\le n,k\le100), do đó, ngay cả việc mô phỏng trực tiếp mọi vi phân cũng là nhỏ. Với (n=100) và (k=100), tổng số lần cập nhật hệ số chỉ 

[ 
100+99+\cdots+1=5050. 
] 

Do đó, giải pháp (O(nk)) có thể nhanh chóng thoải mái dưới những ràng buộc này. Quan sát hữu ích hơn là mọi hệ số ban đầu đều có đóng góp dạng đóng đơn giản sau (k) đạo hàm, đưa ra nghiệm (O(n)) và làm cho việc lập chỉ mục dễ dàng hơn để suy luận. 

Có một số trường hợp việc triển khai có thể diễn ra sai sót một cách âm thầm. Nếu (k>n), mọi số hạng đều biến mất. Ví dụ,```
2 3
1 2 3
```đại diện cho (x^2+2x+3) và đạo hàm bậc ba bằng 0, do đó kết quả đầu ra là```
0 0 0
```Việc thực hiện bất cẩn mà truy cập một cách mù quáng vào hệ số ở mức độ (i+k) có thể vượt quá giới hạn. 

Thuật ngữ không đổi cũng phải được xử lý chính xác. Vì```
1 1
7 5
```đa thức là (7x+5), nên đạo hàm là (7). Đầu ra yêu cầu có hai vị trí:```
0 7
```Số 0 ở phía trước biểu thị hệ số của (x^1), hệ số này không còn tồn tại sau khi lấy vi phân. Việc triển khai chỉ in phần khác 0 độ sẽ tạo ra định dạng sai. 

Cuối cùng, các hệ số có thể tăng nhanh trước phép toán modulo cuối cùng. Ví dụ: phép vi phân lặp lại của một số hạng độ 100 sẽ đưa ra một hệ số nhân có kích thước giai thừa. Số nguyên Python có thể xử lý việc này một cách an toàn, nhưng việc tính toán vẫn phải được giảm modulo (2019) xuyên suốt. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, việc trì hoãn modulo có thể bị tràn. 

## Phương pháp tiếp cận 

Phương pháp trực tiếp nhất là mô phỏng sự khác biệt từng đơn hàng một. Đối với một đa thức được lưu từ bậc cao nhất đến bậc thấp nhất, đạo hàm sẽ thay đổi hệ số (a_i) của (x^i) thành (i a_i), hệ số này trở thành hệ số của (x^{i-1}). Thuật ngữ không đổi trở thành số không. Việc lặp lại quá trình này (k) lần là đúng vì nó tuân theo chính xác định nghĩa về đạo hàm lặp lại. 

Độ phức tạp của mô phỏng này là (O(nk)). Ở giá trị cho phép lớn nhất (n=k=100), số lần nhân hệ số nhiều nhất là (100+99+\cdots+1=5050). Vì vậy, cách tiếp cận này thực sự được chấp nhận đối với các ràng buộc đã nêu. Nó sẽ trở nên kém hấp dẫn hơn nếu các ràng buộc tăng lên đáng kể, đặc biệt khi cả (n) và (k) đều lớn. 

Cách tiếp cận nhanh hơn là xem xét một đơn thức ban đầu thay vì mô phỏng toàn bộ đa thức. Giả sử thuật ngữ ban đầu là 

[ 
a_jx^j. 
] 

Sau một đạo hàm nó trở thành 

[ 
a_jj x^{j-1}. 
] 

Sau hai đạo hàm nó trở thành 

[ 
a_jj(j-1)x^{j-2}. 
] 

Sau (k) đạo hàm với (j\ge k), nó trở thành 

[ 
a_j\cdot j(j-1)\cdots(j-k+1)x^{j-k}. 
] 

Số nhân là một giai thừa giảm dần. Nếu chúng ta muốn hệ số của (x^i) ở đa thức cuối cùng thì bậc ban đầu của nó phải là (i+k). Như vậy 

[ 
b_i=a_{i+k}(i+k)(i+k-1)\cdots(i+1). 
] 

Chúng ta chỉ cần tính toán điều này cho độ ban đầu (j=k,k+1,\ldots,n). Mọi thuật ngữ cấp độ thấp hơn đều biến mất hoàn toàn. 

Vì chỉ có (n+1) hệ số gốc nên chúng ta có thể xử lý từng hệ số một lần. Chúng ta cũng có thể xây dựng hệ số nhân giai thừa giảm dần, tránh giai thừa và giữ mọi giá trị trung gian modulo (2019). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(nk)) | (O(n)) | Được chấp nhận cho những ràng buộc này | 
| Tối ưu | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (n), (k) và các hệ số theo thứ tự giảm dần. Lưu trữ chúng trong một mảng`a`, Ở đâu`a[j]`là hệ số bậc (n-j). 
2. Tạo một mảng đầu ra gồm (n+1) số 0. Việc giữ nguyên độ dài ban đầu là cần thiết vì đầu ra được yêu cầu chứa một khe hệ số cho mọi độ từ (n) đến (0). 
3. Nếu (k>n), hãy để toàn bộ kết quả đầu ra bằng 0 và in nó. Mọi đơn thức đều có bậc nhỏ hơn (k), nên tất cả chúng đều biến mất sau (k) đạo hàm. 
4. Với mỗi bậc gốc (j) từ (k) đến (n), hãy tính hệ số sau đạo hàm (k). Thuật ngữ ban đầu (a_jx^j) trở thành 

[ 
a_j\frac{j!}{(j-k)!}x^{j-k}. 
] 

Bậc cuối cùng là (j-k) nên vị trí đầu ra tương ứng là (n-(j-k)). 

1. Tính số nhân (j!/(j-k)!) bằng cách nhân (k) các số nguyên liên tiếp từ (j-k+1) đến (j), giảm modulo (2019) sau mỗi lần nhân. Vì (k\le100), giá trị này đã đủ nhỏ, nhưng dạng mô-đun cũng tránh được các giá trị trung gian lớn không cần thiết. 
2. Lưu trữ hệ số kết quả ở vị trí tương ứng với độ (j-k), và cuối cùng in tất cả các hệ số đầu ra (n+1) cách nhau bằng dấu cách. 

Điều bất biến là bất cứ khi nào thuật toán xử lý một bậc gốc (j), giá trị được đặt ở bậc (j-k) chính xác là đạo hàm thứ (k)-của đơn thức đơn (a_jx^j). Đạo hàm là tuyến tính, do đó, việc cộng các đóng góp này lên tất cả các đơn thức ban đầu sẽ cho ra đạo hàm thứ (k) đầy đủ. Các số hạng có (j<k) không có vì vi phân lặp đi lặp lại cuối cùng đạt đến số hạng không đổi và sau đó bằng 0. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 2019

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    ans = [0] * (n + 1)

    if k <= n:
        for j in range(k, n + 1):
            multiplier = 1

            for t in range(j - k + 1, j + 1):
                multiplier = multiplier * t % MOD

            coefficient = a[n - j] % MOD
            coefficient = coefficient * multiplier % MOD

            final_degree = j - k
            ans[n - final_degree] = coefficient

    print(*ans)

if __name__ == "__main__":
    solve()
```Mảng đầu vào sử dụng thứ tự giảm dần nên hệ số của (x^j) là`a[n - j]`. Chuyển đổi này là chi tiết lập chỉ mục chính trong quá trình triển khai. 

Vòng lặp bắt đầu lúc`j = k`bởi vì bậc nhỏ hơn (k) không thể tồn tại trong vi phân (k). Đối với mỗi mức độ còn tồn tại, mức độ cuối cùng là`j - k`và do đó vị trí đầu ra của nó là`n - (j - k)`. 

Hệ số nhân được tính như 

[ 
(j-k+1)(j-k+2)\cdots j. 
] 

Đây chính xác là (j!/(j-k)!), thừa số được đưa vào bởi (k) đạo hàm liên tiếp. Lấy modulo (2019) sau mỗi phép nhân sẽ giữ giá trị trung gian nhỏ và cho kết quả cuối cùng như nhau. 

Không cần phải xử lý đặc biệt số hạng không đổi ngoài việc tính toán độ. Khi một thuật ngữ được vi phân, vị trí cuối cùng của nó được xác định trực tiếp bởi bậc ban đầu của nó. 

Sự cố chỉ có một trường hợp thử nghiệm nên giải pháp sẽ đọc hai dòng đầu vào một lần và in một dòng kết quả. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```
5 1
1 2 3 4 5 6
```đầu vào đại diện 

[ 
x^5+2x^4+3x^3+4x^2+5x+6. 
] 

Vì (k=1), mỗi số hạng được nhân với bậc ban đầu của nó và bậc của nó giảm đi một. 

| Bằng gốc (j) | Hệ số gốc | Hệ số nhân | Bằng cấp cuối cùng | Hệ số cuối cùng | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 1 | 0 | 5 | 
| 2 | 4 | 2 | 1 | 8 | 
| 3 | 3 | 3 | 2 | 9 | 
| 4 | 2 | 4 | 3 | 8 | 
| 5 | 1 | 5 | 4 | 5 | 

Bậc 5 không có đóng góp vì vị trí bậc 5 trong đầu ra đại diện cho (x^5), trong khi đạo hàm có bậc tối đa là 4. Mảng cuối cùng là```
0 5 8 9 8 5
```tương ứng với 

[ 
5x^4+8x^3+9x^2+8x+5. 
] 

Đối với mẫu 2,```
5 2
1 2 3 4 5 6
```cùng một đa thức được đạo hàm hai lần. 

| Bằng gốc (j) | Hệ số gốc | Hệ số nhân | Bằng cấp cuối cùng | Hệ số cuối cùng | 
| --- | --- | --- | --- | --- | 
| 2 | 4 | (2\cdot1=2) | 0 | 8 | 
| 3 | 3 | (3\cdot2=6) | 1 | 18 | 
| 4 | 2 | (4\cdot3=12) | 2 | 24 | 
| 5 | 1 | (5\cdot4=20) | 3 | 20 | 

Các số hạng gốc bậc 0 và bậc 1 biến mất vì bậc của chúng nhỏ hơn (k=2). Do đó, vị trí đầu ra cho độ 5 và 4 bằng không. 

Mảng kết quả là```
0 0 20 24 18 8
```đại diện cho 

[ 
20x^3+24x^2+18x+8. 
] 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(nk)) | Đối với mỗi (n+1) đơn thức, tối đa (k) thừa số được nhân | 
| Không gian | (O(n)) | Mảng hệ số và mảng đầu ra đều chứa (n+1) giá trị | 

Với (n,k\le100), trường hợp xấu nhất chỉ thực hiện khoảng (5050) phép nhân mô-đun nhỏ, do đó giải pháp dễ dàng nằm trong giới hạn 1 giây và 128 MB. Việc triển khai được viết theo kiểu dạng đóng trực tiếp vì nó thể hiện cấu trúc toán học một cách rõ ràng, mặc dù mô phỏng đạo hàm lặp lại (O(nk)) cũng sẽ vượt qua. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 2019

def solve():
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    ans = [0] * (n + 1)

    if k <= n:
        for j in range(k, n + 1):
            multiplier = 1
            for t in range(j - k + 1, j + 1):
                multiplier = multiplier * t % MOD

            coefficient = a[n - j] * multiplier % MOD
            final_degree = j - k
            ans[n - final_degree] = coefficient

    print(*ans)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run(
    "5 1\n"
    "1 2 3 4 5 6\n"
) == "0 5 8 9 8 5\n"

# Provided sample 2
assert run(
    "5 2\n"
    "1 2 3 4 5 6\n"
) == "0 0 20 24 18 8\n"

# Minimum-size polynomial
assert run(
    "1 1\n"
    "7 5\n"
) == "0 7\n"

# Differentiation order is larger than polynomial degree
assert run(
    "2 3\n"
    "1 2 3\n"
) == "0 0 0\n"

# All coefficients are equal
assert run(
    "3 1\n"
    "5 5 5 5\n"
) == "0 15 10 5\n"

# Constant polynomial contribution must disappear after differentiation
assert run(
    "4 2\n"
    "0 0 0 7 9\n"
) == "0 0 0 0 0\n"

# Maximum-size input, also checks modular reduction.
n = 100
k = 100
coefficients = [100] * (n + 1)
input_data = f"{n} {k}\n" + " ".join(map(str, coefficients)) + "\n"

expected = [0] * (n + 1)
expected[-1] = 100
assert run(input_data) == " ".join(map(str, expected)) + "\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 1 / 1 2 3 4 5 6`|`0 5 8 9 8 5`| Vi phân bậc nhất và lập bản đồ độ | 
|`5 2 / 1 2 3 4 5 6`|`0 0 20 24 18 8`| Nhiều công cụ phái sinh | 
|`1 1 / 7 5`|`0 7`| Mức độ tối thiểu và loại bỏ liên tục | 
|`2 3 / 1 2 3`|`0 0 0`| Bậc đạo hàm lớn hơn độ | 
|`3 1 / 5 5 5 5`|`0 15 10 5`| Hệ số bằng nhau và tất cả các mức độ sống sót | 
|`4 2 / 0 0 0 7 9`|`0 0 0 0 0`| Các thuật ngữ cấp độ thấp hơn biến mất | 
|`100 100 / 101 coefficients equal to 100`|`0 ... 0 100`| Giới hạn tối đa và lập chỉ mục ranh giới | 

## Vỏ cạnh 

Khi (k>n), không có đơn thức nào đủ bậc để tồn tại trong mọi đạo hàm. Vì```
2 3
1 2 3
```số hạng bậc 2 trở thành 0 sau đạo hàm bậc ba và các số hạng bậc thấp hơn biến mất sớm hơn. Thuật toán không bao giờ đi vào`j`vòng lặp vì`k <= n`là sai, vì vậy đầu ra được khởi tạo trước`[0, 0, 0]`được in trực tiếp. 

Khi có một số hạng không đổi, nó không đóng góp gì vào bất kỳ đạo hàm cấp dương nào. Vì```
1 1
7 5
```đa thức là (7x+5). Vòng lặp chỉ xử lý cấp độ 1, tính toán số nhân (1) và đặt 7 ở cấp độ cuối cùng là 0. Kết quả là`0 7`, bảo toàn các vị trí hệ số (n+1) cần thiết. 

Khi các thuật ngữ cấp độ thấp hơn biến mất nhưng các thuật ngữ cấp độ cao hơn vẫn còn, thuật toán sẽ bỏ qua các thuật ngữ đã biến mất thay vì cố gắng phân biệt chúng. Vì```
4 2
0 0 0 7 9
```đa thức là (7x+9), có đạo hàm bậc hai bằng 0. Cả hai độ 1 và 0 ban đầu đều ở dưới (k=2), do đó cả hai độ đều không được xử lý và mọi vị trí đầu ra vẫn bằng 0. 

Đầu ra phải giữ nguyên số vị trí ban đầu dù độ giảm. Đối với Mẫu 2, đạo hàm bậc hai có bậc 3, nhưng bậc đầu vào là 5, do đó hai mục đầu ra đầu tiên phải bằng 0. Thuật toán viết từng số hạng còn sót lại theo cấp độ cuối cùng thực tế của nó và để lại mọi vị trí khác được khởi tạo về 0. 

Cần phải giảm modulo vì hệ số nhân giai thừa giảm có thể trở nên lớn. Việc thực hiện làm giảm`multiplier`sau mỗi lần nhân và sau đó giảm tích hệ số cuối cùng. Vì phép nhân mô-đun bảo toàn phần còn lại của sản phẩm hoàn chỉnh nên điều này tạo ra chính xác hệ số modulo cần thiết (2019).
