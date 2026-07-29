---
title: "CF 102766D - Lại có dãy ngoặc thông thường?"
description: "Chúng ta có chính xác N dấu ngoặc mở và N dấu ngoặc đóng, vì vậy mọi câu trả lời hợp lệ là một chuỗi dấu ngoặc thông thường có độ dài 2N. Trong số tất cả các chuỗi như vậy, chúng ta chỉ cần đếm những chuỗi không thể có được bằng cách lấy một chuỗi có độ dài N và viết nó hai lần liên tiếp."
date: "2026-07-28T23:38:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102766
codeforces_index: "D"
codeforces_contest_name: "Codedigger Training Contest -String"
rating: 0
weight: 102766
solve_time_s: 56
verified: true
draft: false
---

[CF 102766D - Lại sử dụng chuỗi ngoặc thông thường?](https://codeforces.com/problemset/problem/102766/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có chính xác`N`dấu ngoặc mở và`N`đóng ngoặc, vì vậy mỗi câu trả lời hợp lệ là một chuỗi có độ dài trong ngoặc thông thường`2N`. Trong số tất cả các chuỗi như vậy, chúng ta chỉ cần đếm những chuỗi không thể có được bằng cách lấy một chuỗi có độ dài nào đó.`N`và viết nó hai lần liên tiếp. 

Một chuỗi được gọi`N`-định kỳ khi nửa đầu và nửa sau giống hệt nhau. Ví dụ, với`N = 2`, chuỗi`)()(`là định kỳ vì hai ký tự đầu tiên`)(`được lặp đi lặp lại, trong khi`)(()`thì không. 

Nhiệm vụ không phải là xây dựng các trình tự. Chúng tôi chỉ cần đếm modulo`10^9 + 7`. 

Ràng buộc`N <= 1000`và số lượng ca kiểm thử nhiều nhất là`1000`nói với chúng tôi rằng một`O(N)`hoặc`O(N log N)`phương pháp tiền xử lý là đủ. Giải pháp tạo ra các chuỗi ngoặc là không thể vì số lượng các chuỗi ngoặc thông thường là số Catalan, tăng theo cấp số nhân. Ngay cả đối với kích thước vừa phải`N`, bảng liệt kê trở nên quá lớn. 

Các trường hợp cạnh chính xuất phát từ việc nhầm lẫn độ dài của phần lặp lại với số cặp dấu ngoặc. Vì`N = 1`, chuỗi dấu ngoặc thông thường duy nhất là`()`. Không thể chia nó thành hai nửa khác rỗng bằng nhau nên đáp án là`1`. Một giải pháp bất cẩn luôn trừ đi một chuỗi tuần hoàn sẽ tạo ra kết quả không chính xác.`0`. 

Một trường hợp quan trọng khác là khi`N`thật kỳ quặc. Ví dụ, với đầu vào`N = 3`, tổng số chuỗi dấu ngoặc thông thường là`5`. Một chuỗi tuần hoàn sẽ cần nửa độ dài đầu tiên`3`chứa số dấu ngoặc mở và đóng bằng nhau. Điều đó là không thể vì một chuỗi có độ dài lẻ không thể có số lượng cả hai dấu ngoặc bằng nhau. Câu trả lời đúng là`5`, và một cách tiếp cận bất cẩn giả định mọi`N`tạo ra một phép trừ Catalan nhỏ hơn sẽ sai. 

Trường hợp biên cuối cùng là khi`N`là chẵn. Vì`N = 4`, các chuỗi thông thường được tính bằng`Catalan(4) = 14`. Những cái tuần hoàn chính xác là những chuỗi được hình thành bằng cách lặp lại một chuỗi có độ dài đều đặn`4`, cho`Catalan(2) = 2`trình tự không hợp lệ. Câu trả lời là`12`. Việc quên rằng nửa lặp lại phải được cân bằng sẽ làm mất đi quá nhiều chuỗi. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạo ra mọi chuỗi ngoặc thông thường với`N`cặp, kiểm tra xem nó có tuần hoàn không và đếm những cái không tuần hoàn. Điều này đúng vì nó kiểm tra chính xác tập hợp được yêu cầu. Tuy nhiên, số lượng chuỗi dấu ngoặc thông thường là`Catalan(N)`, xấp xỉ theo cấp số nhân. Vì`N = 1000`, thậm chí việc biểu thị không gian trả lời là không thể, vì vậy vũ lực không thể hoạt động. 

Quan sát hữu ích xuất phát từ việc xem xét một chuỗi dấu ngoặc đều đặn định kỳ trông như thế nào. Giả sử một chuỗi hợp lệ`S`có chiều dài`2N`và là`N`-định kỳ. Khi đó nó có dạng`T + T`, Ở đâu`T`có chiều dài`N`. 

Vì toàn bộ chuỗi được cân bằng nên tổng số dư khung của`T + T`phải bằng không. Do đó số dư của bản sao đầu tiên có giá trị nào đó`x`và bản sao thứ hai đóng góp một bản khác`x`, cho`2x = 0`. Kể từ đây`x = 0`, nghĩa là nửa đầu đã chứa cùng số dấu ngoặc mở và đóng. 

Bây giờ hãy xem xét tính hợp lệ. Mỗi tiền tố của toàn bộ chuỗi phải có ít nhất số dấu ngoặc mở bằng số dấu ngoặc đóng. Bản sao đầu tiên của`T`là tiền tố của chuỗi đầy đủ, vì vậy mọi tiền tố bên trong`T`phải hợp lệ. Từ`T`cũng có số dư bằng 0,`T`chính nó là một chuỗi ngoặc thông thường. 

Điều này mang lại một mối quan hệ một-một. Mọi`N`-chuỗi dấu ngoặc đều đặn tương ứng với một chuỗi dấu ngoặc thông thường có độ dài`N`, và dãy như vậy chỉ tồn tại khi`N`là chẵn. Nếu như`N = 2k`, số dãy tuần hoàn là`Catalan(k)`. 

Tổng số chuỗi dấu ngoặc thông thường có`N`cặp là`Catalan(N)`, vậy đáp án cuối cùng là:`Catalan(N) - Catalan(N / 2)`khi`N`là chẵn.`Catalan(N)`khi`N`thật kỳ quặc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ, xấp xỉ O(Catalan(N)) | Hàm mũ | Quá chậm | 
| Tối ưu | Tiền xử lý O(N) và O(1) cho mỗi truy vấn | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các giai thừa và giai thừa nghịch đảo lên đến`1000`. Số Catalan có thể được tính bằng công thức kết hợp:`Catalan(n) = C(2n, n) - C(2n, n + 1)`tương đương với:`Catalan(n) = (2n choose n) / (n + 1)`. 

Việc tính toán trước các giá trị này cho phép mọi truy vấn được trả lời ngay lập tức. 
2. Với mỗi truy vấn có giá trị`N`, bắt đầu với tổng số chuỗi dấu ngoặc thông thường, đó là`Catalan(N)`. 

Điều này đếm mọi chuỗi hợp lệ có thể có trước khi loại bỏ các chuỗi định kỳ. 
3. Nếu`N`là chẵn, trừ`Catalan(N / 2)`. 

Các chuỗi duy nhất bị loại bỏ chính xác là những chuỗi trong đó nửa đầu bản thân nó là một chuỗi khung thông thường. 
4. Nếu`N`là lẻ, không trừ bất cứ điều gì. 

Một nửa độ dài lẻ không thể chứa số dấu ngoặc mở và đóng bằng nhau, do đó không tồn tại chuỗi đều đặn định kỳ. 
5. In giá trị kết quả theo modulo`10^9 + 7`. 

Tại sao nó hoạt động: 

Bất biến đằng sau lời giải là mọi chuỗi không hợp lệ đều có nửa đầu duy nhất. Một chuỗi được tính là tuần hoàn phải là hai bản sao giống hệt nhau của nửa đó. Để chuỗi hoàn chỉnh là chính quy, một nửa phải có số dư bằng 0 và mọi tiền tố của nó phải hợp lệ. Đây chính xác là những điều kiện để một nửa trở thành một chuỗi dấu ngoặc thông thường. Do đó, các chuỗi tuần hoàn được tính chính xác bằng số Catalan nhỏ hơn và việc trừ chúng sẽ loại bỏ mọi chuỗi không mong muốn. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

MOD = 10**9 + 7

MAX_N = 1000

fact = [1] * (2 * MAX_N + 5)
for i in range(1, len(fact)):
    fact[i] = fact[i - 1] * i % MOD

inv_fact = [1] * (2 * MAX_N + 5)
inv_fact[-1] = pow(fact[-1], MOD - 2, MOD)
for i in range(len(inv_fact) - 1, 0, -1):
    inv_fact[i - 1] = inv_fact[i] * i % MOD

def comb(n, r):
    if r < 0 or r > n:
        return 0
    return fact[n] * inv_fact[r] % MOD * inv_fact[n - r] % MOD

catalan = [0] * (MAX_N + 1)
for i in range(MAX_N + 1):
    catalan[i] = (comb(2 * i, i) - comb(2 * i, i + 1)) % MOD

def solve():
    t = int(input())
    ans = []
    for _ in range(t):
        n = int(input())
        cur = catalan[n]
        if n % 2 == 0:
            cur -= catalan[n // 2]
        ans.append(str(cur % MOD))
    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Phần tiền xử lý xây dựng các giai thừa và giai thừa nghịch đảo để các kết hợp có thể được tính toán trong thời gian không đổi theo modulo. Chỉ số Catalan cần thiết lớn nhất là`1000`, nhưng tính toán`Catalan(1000)`yêu cầu`C(2000, 1000)`, do đó giai thừa được lưu trữ lên đến`2000`. 

Việc xử lý truy vấn tuân theo kết quả toán học trực tiếp. Đầu tiên nó lấy tổng số chuỗi dấu ngoặc thông thường. Khi`N`là số chẵn, các nửa lặp lại được tính bằng số Catalan`N / 2`cặp và bị loại bỏ. 

Phép trừ modulo được xử lý ở cuối với`% MOD`vì giá trị trung gian có thể trở thành âm sau khi loại bỏ các chuỗi tuần hoàn. 

## Ví dụ đã hoạt động 

cho`N = 2`, phép tính là: 

| Bước | N | Tổng số tiếng Catalan | Đếm định kỳ | Trả lời | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 2 | 2 | 0 | 2 | 
| Xóa định kỳ | 2 | 2 | Tiếng Catalan(1)=1 | 1 | 

Hai chuỗi đều đặn là`(())`Và`()()`. Dạng thứ hai là dạng lặp lại của`()`, nên chỉ còn lại một dãy. 

Vì`N = 4`, phép tính là: 

| Bước | N | Tổng số tiếng Catalan | Đếm định kỳ | Trả lời | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 4 | 14 | 0 | 14 | 
| Xóa định kỳ | 4 | 14 | Tiếng Catalan(2)=2 | 12 | 

Hai chuỗi bị loại bỏ được tạo bằng cách lấy hai chuỗi thông thường có hai cặp dấu ngoặc và lặp lại chúng hai lần. Mọi chuỗi hợp lệ khác đều không tuần hoàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(MAX_N + T) | Các giá trị Catalan được tính toán trước một lần và mỗi truy vấn được trả lời theo thời gian không đổi | 
| Không gian | O(MAX_N) | Chỉ các mảng giai thừa và giá trị Catalan được lưu trữ | 

Tối đa`N`đủ nhỏ để dễ dàng tính toán trước tất cả các giá trị trong giới hạn bộ nhớ. Quá trình xử lý truy vấn cuối cùng có thời gian không đổi một cách hiệu quả, giúp xử lý thoải mái số lượng trường hợp thử nghiệm tối đa. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 10**9 + 7

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    # insert the solution function here when running locally

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# Expected outputs from the actual solution:
assert run("4\n1\n2\n3\n4\n") == "1\n1\n5\n12\n", "samples"

# The following expected values validate the logic:
# N = 1: Catalan(1) = 1, no periodic sequence
# N = 2: Catalan(2) - Catalan(1) = 1
# N = 3: Catalan(3) = 5 because odd N has no periodic sequences
# N = 6: Catalan(6) - Catalan(3) = 132 - 5 = 127
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Kích thước tối thiểu không thể trừ định kỳ | 
|`2`|`1`| Trường hợp chẵn nhỏ nhất có một dãy tuần hoàn | 
|`3`|`5`| Một nửa chiều dài lẻ không thể cân bằng | 
|`6`|`127`| Trường hợp chẵn lớn hơn với phép trừ Catalan | 

## Vỏ cạnh 

cho`N = 1`, thuật toán tính toán`Catalan(1) = 1`. Từ`N`là kỳ quặc, nó không trừ đi bất cứ điều gì. Kết quả là`1`, khớp với trình tự duy nhất`()`. 

Vì`N = 3`, tổng số chuỗi hợp lệ là`Catalan(3) = 5`. Thuật toán kiểm tra tính chẵn lẻ và thấy rằng`3`là số lẻ nên số đếm không thay đổi. Điều này xử lý trường hợp không tồn tại nửa đầu hợp lệ của độ dài ba. 

Vì`N = 4`, thuật toán bắt đầu bằng`Catalan(4) = 14`. Vì độ dài là chẵn nên nó trừ đi`Catalan(2) = 2`. Hai chuỗi đó đại diện cho tất cả các nửa có thể lặp lại, để lại`12`chuỗi dấu ngoặc đều đặn không định kỳ. 

Đối với các giá trị được phép rất lớn như`N = 1000`, thuật toán không bao giờ tự xây dựng các chuỗi. Nó chỉ sử dụng các giá trị tổ hợp được tính toán trước, do đó nó tránh được sự tăng trưởng theo cấp số nhân của số lượng chuỗi ngoặc thực tế. 

Tôi cũng có thể cung cấp phiên bản biên tập cuộc thi ngắn hơn hoặc phiên bản tập trung vào bằng chứng chính thức hơn nếu cần.
